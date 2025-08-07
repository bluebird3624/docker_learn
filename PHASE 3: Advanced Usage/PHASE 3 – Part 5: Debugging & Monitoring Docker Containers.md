# ✅ PHASE 3 – Part 5: **Debugging & Monitoring Docker Containers**

---

## 🧠 Why Debug & Monitor?

- Catch errors before users do
    
- Understand performance bottlenecks
    
- Ensure uptime and reliability
    
- Gain insights for scaling and improvements
    

---

## 🛠️ Debugging Tools & Techniques

### 1. **Docker Logs**

- View container logs:
    

bash

CopyEdit

`docker logs <container_name>`

- Follow logs live:
    

bash

CopyEdit

`docker logs -f <container_name>`

---

### 2. **Exec Into Containers**

- Run shell inside running container for live debugging:
    

bash

CopyEdit

`docker exec -it <container_name> bash`

or if bash isn’t available:

bash

CopyEdit

`docker exec -it <container_name> sh`

---

### 3. **Inspect Container State**

- See container details:
    

bash

CopyEdit

`docker inspect <container_name>`

- Check health status:
    

bash

CopyEdit

`docker inspect --format='{{json .State.Health}}' <container_name>`

---

### 4. **Resource Usage**

- See CPU, memory usage of containers:
    

bash

CopyEdit

`docker stats`

---

## 📈 Monitoring Solutions

### a) **Docker Events**

- Monitor Docker daemon events (start, stop, restart):
    

bash

CopyEdit

`docker events`

---

### b) **Prometheus + cAdvisor**

- Collect metrics for container resource usage
    
- Visualize with Grafana dashboards
    

---

### c) **Logging Aggregation**

- Use ELK stack (Elasticsearch, Logstash, Kibana)
    
- Use cloud services like AWS CloudWatch or GCP Stackdriver
    

---

## 🧪 Exercise

1. Run a container that logs output.
    
2. Use `docker logs` and `docker exec` to explore.
    
3. Use `docker stats` to monitor resource usage.
    

---

## ✅ Summary

| Tool/Command                     | Purpose                          |
| -------------------------------- | -------------------------------- |
| `docker logs`                    | View container output logs       |
| `docker exec`                    | Shell into running containers    |
| `docker inspect`                 | Get detailed info                |
| `docker stats`                   | Monitor resource usage           |
| External tools (Prometheus, ELK) | Advanced monitoring and alerting |
