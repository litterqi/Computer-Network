`client()`函数参考代码：
```
def client(server_ip, server_port):
    """Open socket and send message from sys.stdin"""
    # 创建套接字
    client_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)

    # 连接服务器
    server_address = (server_ip, server_port)
    client_socket.connect(server_address)

    try:
        # 从 sys.stdin 中读取输入消息并发送到服务器
        while True:
            message = sys.stdin.readline()
            if not message:
                break
            client_socket.sendall(message.encode())

    except Exception as e:
        print(f"Error: {str(e)}")

    finally:
        # 关闭套接字
        client_socket.close()
```
