`server()`函数参考代码：
```
import socket

def server(server_port):
    # 创建套接字
    server_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)

    # 绑定地址和端口
    server_address = ("", server_port)
    server_socket.bind(server_address)

    # 开始监听连接
    server_socket.listen(5)
    print(f"Server started and listening on port {server_port}")

    while True:
        # 接受客户端连接
        client_socket, client_address = server_socket.accept()
        print(f"Accepted connection from {client_address[0]}:{client_address[1]}")

        # 接收消息并打印到标准输出
        try:
            while True:
                data = client_socket.recv(1024)
                if not data:
                    break
                print(data.decode())
        except Exception as e:
            print(f"Error: {str(e)}")

        # 关闭客户端连接
        client_socket.close()

    # 关闭服务器套接字
    server_socket.close()
```
