# HAProxy Load Balancing Lab

## 🔹 Overview
This project demonstrates load balancing using HAProxy with multiple backend web servers. The goal is to distribute incoming traffic efficiently while ensuring high availability and failover.

## 🔹 Technologies Used
- HAProxy (Load Balancer)
- Ubuntu 22.04 (Server)
- Nginx (Backend Web Servers)
- Systemd (Service Management)

## 🔹 Setup Instructions

1. Clone the repository:
   ```bash
   git clone https://github.com/your-username/haproxy-loadbalancing-lab.git
   cd haproxy-loadbalancing-lab

2. Install HAProxy:
   ```bash
   sudo apt update && sudo apt install -y haproxy

3. Configure HAProxy:
   - Copy the configuration file from configs/haproxy.cfg to /etc/haproxy/haproxy.cfg
   ```bash
   sudo cp configs/haproxy.cfg /etc/haproxy/haproxy.cfg

4. Restart HAProxy:
   ```bash
   sudo systemctl restart haproxy

5. Verify HAProxy status:
   ```bash
   sudo systemctl status haproxy
   
For detailed documentation, see the documentation folder.
