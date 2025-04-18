# frps-work
## Install FRP on your EC2 instance and localhost both:
```
wget https://github.com/fatedier/frp/releases/download/v0.58.1/frp_0.58.1_linux_amd64.tar.gz

```
![image](https://github.com/user-attachments/assets/b9ee0609-4b91-4f45-b4dc-004a7bcb0550)

### -> Extract the downloaded FRP archive:
```
tar -xzf frp_0.58.1_linux_amd64.tar.gz
```
![image](https://github.com/user-attachments/assets/b12ec3d5-e92d-4a29-b410-ef10c7f760d6)

 - move to local folder (both for clent and server)
```
sudo mv frp_0.58.1_linux_amd64 /usr/local/frp
cd /usr/local/frp
```
# In EC-2 instance or server side:
 - Configure frps.ini: Edit the server configuration
```
sudo nano frps.ini
```

 - add this:
 ```
[common]
bind_port = 7000
token = your-secure-token
```
- make sure to add token (eg: - amrendra etc)

## Configure EC2 Security Group:
Go to the AWS EC2 console, select your instance, and edit the Security Group.
- Add inbound rules:
  - Type: Custom TCP, Port: 7000, Source: 0.0.0.0/0 (or restrict to your local IP for security).
  - Type: Custom TCP, Port: 8081, Source: 0.0.0.0/0 (for the forwarded service).
Save the rules.

### -> Run frps: Start the frp server:
```
./frps -c frps.ini
```
![Screenshot from 2025-04-18 22-54-23](https://github.com/user-attachments/assets/490cdd4b-c795-400f-9b86-aa4763d86cd2)


# Clent side or localhost
-> Configure frpc.ini: Edit the client configuration:
```
sudo nano frpc.ini
```
- add this:
```
[common]
server_addr = <EC2-Public-IP>
server_port = 7000
token = your-secure-token

[web]
type = tcp
local_ip = 127.0.0.1
local_port = 8080
remote_port = 8081
```
- make sure to add same token on server side and EC-2 public IPv4 don't just copy !!
  
### Run a Local Service (Optional): If no service is running on 127.0.0.1:8080, start a test web server:
```
python3 -m http.server 8080
```
- Run frpc: Start the frp client:
```
./frpc -c frpc.ini
```
![Screenshot from 2025-04-18 22-31-35](https://github.com/user-attachments/assets/fd148958-f223-44fa-bb45-d1d8636583b6)

- it will connect to server 
![Screenshot from 2025-04-18 22-30-53](https://github.com/user-attachments/assets/678ec57a-7dac-4b42-8672-1a8d429c11ba)

(if not, check inbound rules of ec-2 instance Security Group step from above)

- now
   - From any device (e.g., your local machine or another computer), open a browser and navigate to:
     ```
     http://<EC2-Public-IP>:8081
     ```
for example:-
- ec-2 instence:
![Screenshot from 2025-04-18 22-32-13](https://github.com/user-attachments/assets/4e34aa18-e058-4789-9ad6-22279d9ca924)

- in browser
![Screenshot from 2025-04-18 23-05-53](https://github.com/user-attachments/assets/c5ebe8e4-e6b6-45be-94cf-b7f74a1ea2ef)


     
