# Linux Virtual Network Lab: Secure Services & Traffic Analysis

This is my second practical cybersecurity project.
I built a small Linux network using VirtualBox, with one server and one client.
On the server, I configured services (Apache, SSH, Samba, DHCP), secured them with UFW/iptables, and used Wireshark from the client to analyze plaintext vs encrypted traffic.

---

## Network Setup

* **Server (Ubuntu)** → `192.168.56.100`
* **Client (Ubuntu)** → `192.168.56.102`
* **Network mode** → Internal Network (VirtualBox)
* **Gateway** → `10.0.3.2` (VirtualBox NAT adapter for internet access)

![Netplan Config](images/1-netplan.png)

---

## Apache (HTTP) and SSH (Encrypted)

On the server (`192.168.56.100`):

```bash
sudo apt install apache2 -y
sudo apt install openssh-server -y
```

From the client (`192.168.56.102`):

```bash
curl http://192.168.56.100
ssh user@192.168.56.100
```

![Apache HTTP response](images/3.png)
![SSH login](images/4.png)

---

## Wireshark Traffic Analysis

* Captured HTTP traffic → credentials and content visible in plaintext.
* Captured SSH traffic → encrypted packets, no readable content.

 ![Wireshark HTTP plaintext](images/7.png)
 ![Wireshark SSH encrypted](images/8.png)

---

## Samba File Sharing

On the server:

```bash
sudo apt install samba -y
sudo nano /etc/samba/smb.conf
```

Configured a shared directory with restricted access.

On the client:

```bash
smbclient -L //192.168.56.100 -U username
```

   ![Samba Share](images/9.png)
   ![Samba Share](images/10.png)
   ![Samba Share](images/11.png)                        
                            

---

## Firewall with UFW / iptables

On the server:

```bash
sudo ufw allo 22/tcp
sudo ufw deny 80/tcp
sudo ufw enable
sudo ufw status
```

 ![UFW status](images/5.png)
 ![UFW status](images/6.png)                        
                            

From the client:


---

## Containers (Bonus)

On the server:

```bash
sudo apt install docker.io -y
sudo systemctl enable --now docker
sudo docker run hello-world
```

➡️ ![Docker test](images/12.png)

---

## Key Takeaways

* Saw the difference between plaintext HTTP and encrypted SSH traffic.
* Used firewall rules (UFW/iptables) to control which services are exposed.
* Configured Samba with access control.
* Practiced setting static IPs and running services inside a virtual network.
* First steps with Docker for container isolation.

---

## Next Steps

* Replace HTTP with HTTPS (TLS certificates).
* Add fail2ban to protect SSH from brute force.
* Strengthen Samba authentication.
* Deploy multiple containers with Docker Compose.

---

## Conclusion

This project complemented my first firewall lab.
I went from securing a single host to building a small Linux network with multiple services, securing them with firewall rules, and validating traffic with Wireshark.
It gave me hands-on experience in **defense in depth**: services, network, and encryption.
