# 3c.CREATION FOR FILE TRANSFER USING TCP SOCKETS
# Name: Kishor Kumar B
# Reg : 212223240072
# Date :
## AIM
To write a python program for creating File Transfer using TCP Sockets Links
## ALGORITHM:
1. Import the necessary python modules.
2. Create a socket connection using socket module.
3. Send the message to write into the file to the client file.
4. Open the file and then send it to the client in byte format.
5. In the client side receive the file from server and then write the content into it.
## PROGRAM
# Client :
~~~python
# Client
import socket
import os


HOST = '127.0.0.1'  
PORT = 7001

client_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
client_socket.connect((HOST, PORT))

file_name = input("Enter the file name to send (with extension): ")

if not os.path.isfile(file_name):
    print("File not found. Please check the file name and try again.")
else:

    client_socket.send(file_name.encode('utf-8'))
    file_size = os.path.getsize(file_name)
    client_socket.send(str(file_size).encode('utf-8'))

    with open(file_name, 'rb') as f:
        bytes_read = f.read(1024)
        while bytes_read:
            client_socket.send(bytes_read)
            bytes_read = f.read(1024)

    print(f"{file_name} has been sent.")

client_socket.close()


~~~
# Server :
~~~
# Server
import socket
import os

HOST = '127.0.0.1'  
PORT = 7001

server_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
server_socket.bind((HOST, PORT))
server_socket.listen(1)
print("Server is listening...")

conn, addr = server_socket.accept()
print(f"Connection from {addr} has been established.")

file_name = conn.recv(1024).decode('utf-8')
file_size = conn.recv(1024).decode('utf-8')
file_size = int(file_size)

with open(file_name, 'wb') as f:
    bytes_received = 0
    while bytes_received < file_size:
        data = conn.recv(1024)
        if not data:
            break
        f.write(data)
        bytes_received += len(data)

print(f"{file_name} has been received and saved.")
conn.close()
server_socket.close()


~~~
## OUPUT
![Output](https://github.com/user-attachments/assets/a2072ab7-de52-43cd-99d5-d768ac5c3b4f)


## RESULT
Thus, the python program for creating File Transfer using TCP Sockets Links was 
successfully created and executed.
