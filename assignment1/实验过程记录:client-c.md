关于`main()`函数中命令行参数传递：
![image](https://github.com/litterqi/Computer-Network/assets/123362884/353c371c-01dc-483d-99d6-1ce45a68c0bc)

`client()`函数参考代码：
```
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <sys/socket.h>
#include <netinet/in.h>
#include <arpa/inet.h>
#include <unistd.h>

#define BUFFER_SIZE 1024

int client(const char *server_ip, const char *server_port) {
    int sockfd;
    struct sockaddr_in server_addr;
    char buffer[BUFFER_SIZE];

    // 创建套接字
    sockfd = socket(AF_INET, SOCK_STREAM, 0);
    if (sockfd < 0) {
        perror("Socket creation failed");
        return -1;
    }

    // 设置服务器地址
    memset(&server_addr, 0, sizeof(server_addr));
    server_addr.sin_family = AF_INET;
    server_addr.sin_port = htons(atoi(server_port));
    if (inet_pton(AF_INET, server_ip, &server_addr.sin_addr) <= 0) {
        perror("Invalid server IP address");
        close(sockfd);
        return -1;
    }

    // 连接服务器
    if (connect(sockfd, (struct sockaddr *)&server_addr, sizeof(server_addr)) < 0) {
        perror("Connection failed");
        close(sockfd);
        return -1;
    }

    // 从标准输入读取消息，并发送给服务器
    while (fgets(buffer, BUFFER_SIZE, stdin) != NULL) {
        if (send(sockfd, buffer, strlen(buffer), 0) < 0) {
            perror("Message send failed");
            close(sockfd);
            return -1;
        }
    }

    close(sockfd);
    return 0;
}
```
