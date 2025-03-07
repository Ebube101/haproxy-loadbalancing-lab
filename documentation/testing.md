# **`testing.md` - HAProxy Testing Documentation**  

```markdown
# HAProxy Testing Documentation  

## 📌 Overview  
This document outlines the **test cases**, **performance tests**, and **health check logs** for the HAProxy load balancer.  

---

## **1️⃣ Basic Connectivity Test**  

### **Test Command:**  
```bash
curl -I http://<haproxy-server-ip>
```

### **Expected Result:**  
- Response headers should indicate a valid HTTP response (`200 OK`).  
- Requests should alternate between backend servers (`web1` and `web2`).  

📸 **Screenshot:** ![basic-connectivity-test](https://github.com/Ebube101/haproxy-loadbalancing-lab/blob/main/documentation/screenshots/basic-connectivity-test.png?raw=true) 

---

## **2️⃣ Load Balancing Test**  

### **Test Command:**  
```bash
for i in {1..10}; do curl -s http://<haproxy-server-ip> | grep "Server"; done
```

### **Expected Result:**  
- Requests should distribute evenly between `web1` and `web2` (round-robin).  

📸 **Screenshot:** ![load-balancing-test](https://github.com/Ebube101/haproxy-loadbalancing-lab/blob/main/documentation/screenshots/load-balancing-test.png?raw=true)  

---

## **3️⃣ Backend Server Health Check**  

### **Test Command:**  
```bash
sudo systemctl status haproxy
```

### **Expected Result:**  
- HAProxy should report both backend servers as **UP**.  

---

## **4️⃣ HAProxy Logs Verification**  

### **Test Command:**  
```bash
sudo journalctl -u haproxy --no-pager | tail -n 20
```

### **Expected Result:**  
- Logs should show successful connections and health check results.  

---

## **5️⃣ Performance Test**  

### **Test Command:**  
```bash
ab -n 1000 -c 50 http://<haproxy-server-ip>/
```
(Apache Benchmarking tool sends **1000 requests** with **50 concurrent connections**.)

### **Expected Result:**  
- HAProxy should efficiently distribute load without errors.  
- Average response time should be within acceptable limits.  

📸 **Screenshot:** ![perfomance-test](https://github.com/Ebube101/haproxy-loadbalancing-lab/blob/main/documentation/screenshots/performance-test.png)

---

## **6️⃣ Monitoring via HAProxy Stats Page**  

### **Test Command:**  
- Open a browser and navigate to:  
  ```
  http://<haproxy-server-ip>:8080/stats
  ```

### **Expected Result:**  
- The stats page should show active connections, backend health, and request distribution.  

📸 **Screenshot:** ![haproxy-stats](https://github.com/Ebube101/haproxy-loadbalancing-lab/blob/main/documentation/screenshots/haproxy-stats.png)

---

## **7️⃣ Failover Test (Simulating Backend Failure)**  

### **Test Command:**  
1. **Stop `web1` server**  
   ```bash
   sudo systemctl stop apache2  # If using Apache as backend
   ```
2. **Check HAProxy logs**  
   ```bash
   sudo journalctl -u haproxy --no-pager | tail -n 20
   ```

### **Expected Result:**  
- HAProxy should detect `web1` as DOWN and route all traffic to `web2`.  

---

## **8️⃣ Troubleshooting Steps**  

If any test fails, follow these steps:  

🔹 **Check HAProxy configuration**  
```bash
haproxy -c -f /etc/haproxy/haproxy.cfg
```

🔹 **Restart HAProxy service**  
```bash
sudo systemctl restart haproxy
```

🔹 **Verify backend health**  
```bash
curl -I http://192.168.1.101
curl -I http://192.168.1.102
```
---

## **9️⃣ Conclusion**  
- **HAProxy successfully balances load** between backend servers.  
- **Failover works correctly** when a backend server goes down.  
- **Performance tests confirm HAProxy can handle high traffic loads.**  
- **Monitoring tools** provide real-time insights into HAProxy operations.  

---
```
