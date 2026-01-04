study_plan

1.有项目时

- 上午：做项目相关的事情
- 下午：整理项目中的不足
- 晚上：学习与项目无关的事情	

2.没项目时

- 上午：学习嵌入式学习计划的内容视频
- 下午：做练习熟悉代码
- 晚上：整理一天的资料

刷题网站

- [牛客网](https://www.nowcoder.com/)

- [代码随想录](https://www.programmercarl.com/)

```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <winsock2.h>
#include <ws2tcpip.h>  // 提供 `inet_ntoa` 和现代 Socket API
#pragma comment(lib, "ws2_32.lib")  // 链接 Windows Socket 库

#define PORT 8080
#define MAX_CLIENTS 100
#define BUFFER_SIZE 1024

int main() {
    WSADATA wsaData;
    if (WSAStartup(MAKEWORD(2, 2), &wsaData) != 0) {
        printf("WSAStartup failed: %d\n", WSAGetLastError());
        return 1;
    }

    SOCKET server_fd, new_socket;
    struct sockaddr_in address;
    int opt = 1;
    int addrlen = sizeof(address);
    fd_set readfds;
    SOCKET client_sockets[MAX_CLIENTS] = { 0 };
    int max_sd;

    // 创建 TCP socket
    if ((server_fd = socket(AF_INET, SOCK_STREAM, 0)) == INVALID_SOCKET) {
        printf("socket failed: %d\n", WSAGetLastError());
        WSACleanup();
        return 1;
    }

    // 设置 socket 选项（允许地址复用）
    if (setsockopt(server_fd, SOL_SOCKET, SO_REUSEADDR, (const char*)&opt, sizeof(opt)) == SOCKET_ERROR) {
        printf("setsockopt failed: %d\n", WSAGetLastError());
        closesocket(server_fd);
        WSACleanup();
        return 1;
    }

    address.sin_family = AF_INET;
    address.sin_addr.s_addr = INADDR_ANY;
    address.sin_port = htons(PORT);

    // 绑定 socket 到端口
    if (bind(server_fd, (struct sockaddr*)&address, sizeof(address)) == SOCKET_ERROR) {
        printf("bind failed: %d\n", WSAGetLastError());
        closesocket(server_fd);
        WSACleanup();
        return 1;
    }

    // 开始监听
    if (listen(server_fd, 5) == SOCKET_ERROR) {
        printf("listen failed: %d\n", WSAGetLastError());
        closesocket(server_fd);
        WSACleanup();
        return 1;
    }

    printf("Server started on port %d\n", PORT);

    while (1) {
        FD_ZERO(&readfds);
        FD_SET(server_fd, &readfds);
        max_sd = server_fd;

        // 添加客户端 socket 到集合
        for (int i = 0; i < MAX_CLIENTS; i++) {
            if (client_sockets[i] > 0) {
                FD_SET(client_sockets[i], &readfds);
                if (client_sockets[i] > max_sd)
                    max_sd = client_sockets[i];
            }
        }

        // 等待活动
        int activity = select(max_sd + 1, &readfds, NULL, NULL, NULL);
        if (activity == SOCKET_ERROR) {
            printf("select error: %d\n", WSAGetLastError());
            continue;  // 继续循环，而不是退出
        }

        // 处理新连接
        if (FD_ISSET(server_fd, &readfds)) {
            if ((new_socket = accept(server_fd, (struct sockaddr*)&address, &addrlen)) == INVALID_SOCKET) {
                printf("accept failed: %d\n", WSAGetLastError());
                continue;
            }

            // 使用 inet_ntop 替代 inet_ntoa
            char ip_str[INET_ADDRSTRLEN];
            inet_ntop(AF_INET, &(address.sin_addr), ip_str, INET_ADDRSTRLEN);
            printf("New connection: socket fd %lld, IP %s, port %d\n",
                new_socket, ip_str, ntohs(address.sin_port));

            // 添加到客户端数组
            for (int i = 0; i < MAX_CLIENTS; i++) {
                if (client_sockets[i] == 0) {
                    client_sockets[i] = new_socket;
                    printf("Adding to list of sockets as %d\n", i);
                    break;
                }
            }
        }

        // 处理客户端 I/O
        for (int i = 0; i < MAX_CLIENTS; i++) {
            SOCKET sd = client_sockets[i];
            if (sd > 0 && FD_ISSET(sd, &readfds)) {
                char buffer[BUFFER_SIZE] = { 0 };
                int valread = recv(sd, buffer, BUFFER_SIZE, 0);

                // 客户端正常关闭
                if (valread == 0) {
                    getpeername(sd, (struct sockaddr*)&address, &addrlen);
                    char ip_str[INET_ADDRSTRLEN];  // 存储转换后的 IP 字符串
                    inet_ntop(AF_INET, &(address.sin_addr), ip_str, INET_ADDRSTRLEN);
                    printf("Client disconnected: IP %s, port %d\n", ip_str, ntohs(address.sin_port));

                    closesocket(sd);
                    client_sockets[i] = 0;
                }
                // 处理异常断开
                else if (valread == SOCKET_ERROR) {
                    int error = WSAGetLastError();
                    if (error == WSAECONNRESET) {
                        printf("Client unexpectedly disconnected\n");
                    }
                    else {
                        printf("recv error: %d\n", error);
                    }
                    closesocket(sd);
                    client_sockets[i] = 0;
                }
                // 处理接收到的数据
                else {
                    buffer[valread] = '\0';
                    printf("Received: %s\n", buffer);

                    // 回显数据
                    if (send(sd, buffer, valread, 0) == SOCKET_ERROR) {
                        printf("send error: %d\n", WSAGetLastError());
                        closesocket(sd);
                        client_sockets[i] = 0;
                    }
                }
            }
        }
    }

    // 清理（通常不会执行到这里，因为 while(1) 无限循环）
    closesocket(server_fd);
    WSACleanup();
    return 0;
}
```

