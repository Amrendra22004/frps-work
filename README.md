# frps-work
## Install FRP on your EC2 instance
```
wget https://github.com/fatedier/frp/releases/download/v0.49.0/frp_0.49.0_linux_amd64.tar.gz
```
![image](https://github.com/user-attachments/assets/b9ee0609-4b91-4f45-b4dc-004a7bcb0550)

### -> Extract the downloaded FRP archive:
```
tar -xvzf frp_0.49.0_linux_amd64.tar.gz
cd frp_0.49.0_linux_amd64
```
![image](https://github.com/user-attachments/assets/b12ec3d5-e92d-4a29-b410-ef10c7f760d6)


-> Move the binaries to a system path
```
sudo mv frps /usr/local/bin/
sudo mv frpc /usr/local/bin/
```

### -> Configure FRP
Server Configuration
```
sudo mkdir -p /etc/frp
```
### -> Create the FRP server configuration file
```
sudo nano /etc/frp/frps.ini
```
Example configuration for the server:
``` txt
[common]
bind_port = 7000
vhost_http_port = 8080
vhost_https_port = 443
```
![image](https://github.com/user-attachments/assets/16c74b4e-a822-4139-81fa-94127677512d)

### -> Client Configuration (frpc)
-> Create the client configuration file:
```
nano frpc.ini
```
-> In frpc.ini file put this Example :
```
[common]
server_addr = your-ec2-public-ip
server_port = 7000

[web]
type = tcp
local_ip = 127.0.0.1
local_port = 80
remote_port = 8080
```
![image](https://github.com/user-attachments/assets/9010dfc4-9170-469f-81d5-91fa3897f2f3)

### ->  Start the FRP Server (frps)
-> Start the server in the background:
```
sudo frps -c /etc/frp/frps.ini &
```
![image](https://github.com/user-attachments/assets/e6d473b2-dc6f-4f52-9bdb-95f5ce1bca5a)





