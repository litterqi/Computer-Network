`server()`函数参考代码：
```
#include <stdio.h>
#include <stdlib.h>
#include <sys/socket.h>
#include <netinet/in.h>
#include <unistd.h>

int server(char *server_port) {
  int sockfd, client_sockfd;
  struct sockaddr_in server_addr, client_addr;

  // 创建套接字
  if ((sockfd = socket(AF_INET, SOCK_STREAM, 0)) == -1) {
    perror("Error creating socket");
    return 1;
  }

  // 设置服务器地址和端口
  server_addr.sin_family = AF_INET;
  server_addr.sin_port = htons(atoi(server_port));
  server_addr.sin_addr.s_addr = INADDR_ANY;

  // 绑定套接字到服务器地址
  if (bind(sockfd, (struct sockaddr *)&server_addr, sizeof(server_addr)) == -1) {
    perror("Error binding socket");
    close(sockfd);
    return 1;
  }

  // 监听连接请求
  if (listen(sockfd, 5) == -1) {
    perror("Error listening");
    close(sockfd);
    return 1;
  }

  printf("Server listening on port %s\n", server_port);

  // 接受客户端连接
  socklen_t client_addr_len = sizeof(client_addr);
  if ((client_sockfd = accept(sockfd, (struct sockaddr *)&client_addr, &client_addr_len)) == -1) {
    perror("Error accepting client connection");
    close(sockfd);
    return 1;
  }

  printf("Client connected\n");

  // 接收客户端消息
  char buffer[1024];
  ssize_t recv_len = recv(client_sockfd, buffer, sizeof(buffer), 0);

  if (recv_len == -1) {
    perror("Error receiving message");
    close(client_sockfd);
    close(sockfd);
    return 1;
  }

  // 打印接收到的消息
  printf("Received message: %.*s\n", (int)recv_len, buffer);

  close(client_sockfd);
  close(sockfd);

  return 0;
}
```
关于其中使用到的函数功能解释：
![image](https://github.com/litterqi/Computer-Network/assets/123362884/f8537784-e06b-43cf-af90-459b21488ed7)
