# 2c.SIMULATING ARP /RARP PROTOCOLS
## AIM
To write a python program for simulating ARP protocols using TCP.
## ALGORITHM:
## Client:
1. Start the program
2. Using socket connection is established between client and server.
3. Get the IP address to be converted into MAC address.
4. Send this IP address to server.
5. Server returns the MAC address to client.
## Server:
1. Start the program
2. Accept the socket which is created by the client.
3. Server maintains the table in which IP and corresponding MAC addresses are
stored.
4. Read the IP address which is send by the client.
5. Map the IP address with its MAC address and return the MAC address to client.
P
## PROGRAM - ARP
arp_server.py
```
import socket
arp_table = {
    "192.168.1.1": "AA:BB:CC:DD:EE:01",
    "192.168.1.2": "AA:BB:CC:DD:EE:02",
    "192.168.1.3": "AA:BB:CC:DD:EE:03"
}
server_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
server_socket.bind(("localhost", 9999))
server_socket.listen(1)

print("ARP Server is running...")

conn, addr = server_socket.accept()
print(f"Connection from {addr}")

while True:
    ip_address = conn.recv(1024).decode()
    if not ip_address:
        break
    print(f"Received IP: {ip_address}")

    mac_address = arp_table.get(ip_address, "MAC not found")
    conn.send(mac_address.encode())

conn.close()
server_socket.close()
```
arp_client.py
```
import socket

client_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
client_socket.connect(("localhost", 9999))

ip_address = input("Enter IP Address to find MAC: ")
client_socket.send(ip_address.encode())

mac_address = client_socket.recv(1024).decode()
print(f"MAC Address for {ip_address} is: {mac_address}")

client_socket.close()
```
## OUPUT - ARP

<img width="1261" height="135" alt="image" src="https://github.com/user-attachments/assets/a8f8a799-e1e8-4067-a646-e49bf63ed3a1" />

## PROGRAM - RARP
rarp_server.py
```
import socket
rarp_table = {
    "AA:BB:CC:DD:EE:01": "192.168.1.1",
    "AA:BB:CC:DD:EE:02": "192.168.1.2",
    "AA:BB:CC:DD:EE:03": "192.168.1.3"
}

server_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
server_socket.bind(("localhost", 9998))
server_socket.listen(1)

print("RARP Server is running...")

conn, addr = server_socket.accept()
print(f"Connection from {addr}")

while True:
    mac_address = conn.recv(1024).decode()
    if not mac_address:
        break
    print(f"Received MAC: {mac_address}")

    ip_address = rarp_table.get(mac_address, "IP not found")
    conn.send(ip_address.encode())

conn.close()
server_socket.close()
```
rarp_client.py
```
import socket

client_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
client_socket.connect(("localhost", 9998))

mac_address = input("Enter MAC Address to find IP: ")
client_socket.send(mac_address.encode())

ip_address = client_socket.recv(1024).decode()
print(f"IP Address for {mac_address} is: {ip_address}")

client_socket.close()
```
## OUPUT -RARP

<img width="1267" height="227" alt="image" src="https://github.com/user-attachments/assets/acb27bbc-f319-4339-a35a-0e1ba598f3b7" />

## RESULT
Thus, the python program for simulating ARP protocols using TCP was successfully 
executed.
