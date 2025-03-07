# **Implementation Documentation (implementation.md)**  

---

## **1️⃣ Overview**  
This document explains the implementation details of HAProxy as a load balancer for multiple backend web servers. It covers:  
- HAProxy installation and configuration  
- Load balancing algorithm used  
- Health checks for backend servers  
- Logging setup  
- Monitoring and troubleshooting  

---

## **2️⃣ Why Use HAProxy?**  
HAProxy is a **high-performance TCP/HTTP load balancer** that distributes network traffic across multiple backend servers. Benefits of using HAProxy:  
✅ **High availability** – Ensures failover and redundancy  
✅ **Scalability** – Distributes traffic efficiently  
✅ **Health checks** – Detects failed servers and reroutes traffic  
✅ **Logging and monitoring** – Provides visibility into request distribution  

---

## **3️⃣ System Architecture**  
📌 **Components Involved:**  
- **HAProxy** (Load balancer)  
- **Two backend Nginx web servers** (`web1` and `web2`)  
- **Client requests** distributed among backend servers    

---

## **4️⃣ Installation and Setup**  

### **Step 1: Install HAProxy**  
Run the following commands on the HAProxy server:  
```bash
sudo apt update
sudo apt install -y haproxy
```
---

### **Step 2: Configure HAProxy (`haproxy.cfg`)**  
📌 **Configuration file location:** `configs/haproxy.cfg`  

```haproxy
global
    log /dev/log local0
    log /dev/log local1 notice
    chroot /var/lib/haproxy
    stats socket /run/haproxy/admin.sock mode 660 level admin
    stats timeout 30s
    user haproxy
    group haproxy
    daemon

defaults
    log global
    option httplog
    option dontlognull
    timeout connect 5s
    timeout client  50s
    timeout server  50s

frontend http_front
    bind *:80
    default_backend http_back

backend http_back
    balance roundrobin
    server web1 192.168.1.101:80 check
    server web2 192.168.1.102:80 check
```
📌 **Configuration Breakdown:**  
- **Frontend (`http_front`)**: Listens for incoming requests on port **80**  
- **Backend (`http_back`)**: Uses a **round-robin** algorithm to distribute traffic  
- **Health checks (`check`)**: Ensures backend servers are responsive    

---

### **Step 3: Start and Enable HAProxy Service**  
```bash
sudo systemctl enable --now haproxy
sudo systemctl status haproxy
```
📸 **Screenshot:**  ![haproxy-status](https://github.com/Ebube101/haproxy-loadbalancing-lab/blob/main/documentation/screenshots/haproxy-status.png?raw=true)
🔹 **File location:** `documentation/screenshots/haproxy-status.png`  

---

## **5️⃣ Load Balancing Algorithm**  
📌 The **Round Robin** algorithm is used:  
- Requests are **distributed evenly** across servers.  
- Ideal for **stateless applications** (e.g., web servers).  

📌 Alternative options:  
- **Least Connection**: Sends requests to the server with the fewest active connections.  
- **Source Hashing**: Directs requests from the same client IP to the same backend.  

---

## **6️⃣ Backend Health Checks**  
HAProxy **automatically checks** if a backend server is alive.  

📌 **Health Check Setup in `haproxy.cfg`:**  
```haproxy
backend http_back
    balance roundrobin
    server web1 192.168.1.101:80 check
    server web2 192.168.1.102:80 check
```
📌 **How to View Health Checks:**  
```bash
sudo tail -f /var/log/haproxy.log
```
---

## **7️⃣ Logging Setup**  
HAProxy logs requests using **syslog**.  

📌 **Logging Setup in `haproxy.cfg`:**  
```haproxy
global
    log /dev/log local0
    log /dev/log local1 notice
```
📌 **View HAProxy Logs:**  
```bash
sudo journalctl -u haproxy --no-pager
```  
---

## **8️⃣ Monitoring HAProxy (Stats Page)**  
📌 **Enable HAProxy Stats Dashboard:**  
_Add this to `haproxy.cfg`_  
```haproxy
listen stats
    bind *:8080
    stats enable
    stats uri /stats
    stats refresh 5s
```
📌 **Access it in a browser:**  
```
http://server-ip:8080/stats
```
📸 **Screenshot:** ![haproxy-stats](https://github.com/Ebube101/haproxy-loadbalancing-lab/blob/main/documentation/screenshots/haproxy-stats.png?raw=true) 
🔹 **File location:** `documentation/screenshots/haproxy-stats.png`  

---

## **9️⃣ Troubleshooting**  

### **🔹 Check HAProxy Logs**  
```bash
sudo tail -f /var/log/haproxy.log
```

### **🔹 Verify HAProxy Configuration**
```bash
haproxy -c -f /etc/haproxy/haproxy.cfg
```

### **🔹 Restart HAProxy If Needed**
```bash
sudo systemctl restart haproxy
```
---

## **🔟 Conclusion**  
- HAProxy successfully **distributes traffic** across backend servers.  
- The **round-robin** algorithm ensures **equal request distribution**.  
- **Health checks** automatically detect server failures.  
- **Monitoring and logging** help track performance.  

📌 **Next Steps:**  
- Implement **HTTPS with SSL termination**.  
- Use **Least Connection** algorithm for better load handling.  

