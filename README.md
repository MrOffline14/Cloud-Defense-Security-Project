# Cloud-Defense-Security-Project

## Table of Contents

- [System Performance Benchmarking and Load Testing](#system-performance-benchmarking-and-load-testing)
  
  - [System Information Analysis](#system-information-analysis)
    - [CPU Architecture Profiling (Cores and Bogomips)](#cpu-architecture-profiling-cores-and-bogomips)  
    - [Hardware Vulnerability Enumeration and Mitigation](#hardware-vulnerability-enumeration-and-mitigation)  
    - [Bogomips Validation and Consistency Check](#bogomips-validation-and-consistency-check)  
    - [Network Service Enumeration (Open Ports Analysis)](#network-service-enumeration-open-ports-analysis)
      
  - [Webserver Setup and Local Load Testing](#webserver-setup-and-local-load-testing)
    - [Nginx Deployment and Service Validation](#nginx-deployment-and-service-validation)  
    - [DDoS Load Stress Simulation with ApacheBench](#ddos-load-stress-simulation-with-apachebench)
      - [High-Volume Request Execution (100k Requests @ Concurrency 100)](#high-volume-request-execution-100k-requests--concurrency-100)  
      - [Real-Time CPU Utilization Monitoring](#real-time-cpu-utilization-monitoring)
          
  - [Containerized Load Testing with Docker](#containerized-load-testing-with-docker)  
    - [Dockerized Environment Benchmarking (Nginx in Containers)](#dockerized-environment-benchmarking-nginx-in-containers)  
    - [Containerized DDoS Load Simulation (ApacheBench 100k)](#containerized-ddos-load-simulation-apachebench-100k)  
    - [Container Resource Utilization Analysis under Stress](#container-resource-utilization-analysis-under-stress)  
    - [Comparative Benchmark: Host vs Docker under DDoS Load](#comparative-benchmark-host-vs-docker-under-ddos-load)
        
  - [High-Concurrency Stress Test](#high-concurrency-stress-test)  
    - [Extreme Load Benchmarking (1M Requests @ Concurrency 1000)](#extreme-load-benchmarking-1m-requests--concurrency-1000)  




## System Perfomance Benchmarking and Load Testing
This part of the project demonstrates how to analyze system resources, test performance under load, compare a web server on the host versus in a container, and perform stress tests with high concurrency.

### System Information Analysis
#### CPU Architecture Profiling (Cores and Bogomips)
The first step towards perfomance benchmarking and load testing is collecting information about system's processor. This includes number of cores and the Bogomips value per core, providing a baseline understanding of the system's capacity and hardware characteristics.

The screenshot shows the result of running the `lscpu` command, which provides details such as CPU architecture, model, number of cores, and the Bogomips value.
<br></br>

 **Number of CPUs/Core:** 2   **BogoMips per core:** 5587.06  
<img src="images/CPU-Information(lscpu).png" alt="CPU Info (lscpu)" width="500"/> 



#### Hardware Vulnerability Enumeration and Mitigation

`lscpu` also lists CPU vulnerabilities along with mitigation details as shown under, which show the security measures the system applies to reduce the risk of exploiting known hardware vulnerabilities.

<img src="images/CPU-vuln.png" alt="CPU Vulnerabilities" width="500"/> 


#### Bogomips Validation

*Additionally, `cat /proc/cpuinfo | grep bogomips` can be used to confirm the Bogomips value directly from the CPU info file, showing identical results for each core.*

<img src="images/CPU-Bogomips-verify.png" alt="Bogomips from /proc/cpuinfo" width="500"/> 


#### Network Service Enumeration (Open Ports Analysis)

Checking open ports helps verify which services are actively listening on the system. 
In this project, the purpose is to confirm that only the necessary services (e.g., NGINX for load testing) are running, 
so that performance benchmarks take place in a controlled environment without interference from unrelated processes. 
Unnecessary services can be disabled to avoid extra resource usage or unintended exposure.


To list the open network ports, the command `ss -tuln` was used.  
This is the modern replacement for `netstat -tuln` and provides the same information 
about active TCP and UDP sockets.
<br></br>

**Open Ports:** 53 (UDP/TCP), 631 (UDP/TCP), 5353 (UDP), 44170 (UDP), 47868 (UDP)

<img src="images/NetworkPorts-services.png" alt="Open network ports" width="500"/>

💻**Commands Used:**
```bash

# Display detailed CPU architecture, including cores, threads, bogomips, and vulnerabilities
lscpu

# Verify bogomips value directly from /proc/cpuinfo for each core
cat/proc/cpuinfo | grep bogomips

# List all listening TCP and UDP ports with socket details
ss -tuln
```
---

### Webserver Setup and Local Load Testing
#### Nginx Deployment and Service Validation

The next step is to set up a web server to be used for load testing.  
For this project, **Nginx** was chosen due to its lightweight design and high performance.  

Nginx was successfully installed and started on the VM by running `sudo apt install nginx`.  
The installation process downloaded and configured the necessary packages.


<img src="images/Nginx-Installation.png" alt="Nginx installation process" width="500"/>


The Service status was verified using `systemctl status nginx` to confirm that the server was active and running.

<img src="images/Nginx-status.png" alt="Nginx service status" width="500"/>


#### DDoS - Load Stress Simulation using ApacheBench

##### High-Volume Request Execution (100k Requests @ Concurrency 100)

The command `ab -n 100000 -c 100 http://127.0.0.1/` was used to simulate 100,000 requests with a concurrency level of 100.  
This kind of stress test is designed to evaluate how the web server handles a sudden burst of requests, which resembles a Denial-of-Service (DoS) scenario.  

The picture under shows the execution progress of ApacheBench, including milestones of completed requests and the final summary confirming that all 100,000 requests were processed successfully without errors.

This demonstrates that the server was capable of maintaining stability under significant load.

<img src="images/DDoS-ApacheBenchmark.png" alt="ApacheBench DDoS simulation" width="500"/>


##### Real-Time CPU Utilization Monitoring

While the ApacheBench test was running, system resource usage was monitored using the `htop` command. The picture shows CPU utilization during the peak of the test, where the highest observed load reached **26%**.

This indicates that the system had sufficient spare capacity and was not fully saturated, which suggests that the hardware could handle an even higher load or additional concurrent processes. Monitoring resource usage alongside benchmark testing is critical to identifying potential bottlenecks such as CPU saturation, memory exhaustion, or I/O limitations.  

In this case, the relatively low CPU load under stress confirms that the bottleneck, if any, would more likely be in network throughput or application-level constraints rather than raw processing power.


<img src="images/DDoS-CPUload.png" alt="CPU load during DDoS simulation" width="500"/>


💻**Commands Used:**  
```bash
# Install the Nginx web server package
sudo apt install nginx

# Check that the Nginx service is loaded, enabled, and running
systemctl status nginx

# Run ApacheBench with 100,000 requests and concurrency level 100
ab -n 100000 -c 100 http://127.0.0.1/

# Monitor system resources during the load test
htop
```
---

### Containerized Load Testing with Docker


#### Dockerized Environment Benchmarking (Nginx in Containers)
Docker was introduced at this stage to extend the benchmarking from a bare-metal setup into a containerized environment. It was installed on the system using `sudo apt install docker.io`, and the Nginx web server was later started inside a container with `sudo docker run -d -p 80:80 nginx`. This made it possible to compare raw host performance with containerized performance, reflecting modern deployment practices where containers are widely used.  

A containerized setup ensures consistency by packaging the application with its dependencies, while also providing process isolation and options for fine-grained resource control. This allows for reproducible experiments and highlights the trade-off between efficiency and flexibility.  

In practice, repeating the experiment in Docker not only offers comparative data, but also mirrors industry-standard approaches where containerization and orchestration platforms such as Kubernetes are central to scalable deployments.

<img src="images/Docker-Install.png" alt="Docker Installation" width="500"/>


#### Containerized DDoS Load Simulation (ApacheBench 100k)

After installing Docker and running Nginx inside a container, the same Apache Benchmark (ab) stress test was repeated to observe performance differences compared to running Nginx directly on the host.

The goal was to evaluate how containerization impacts CPU utilization and request handling under heavy load.

*Apache Benchmark Execution (ab):*
The following image shows the execution of the Apache Benchmark command:

`ab -n 100000 -c 100 http://127.0.0.1/`

This command simulates 100,000 HTTP requests with a concurrency level of 100, targeting the Nginx server running inside Docker. The output confirms the completion of all requests.

<img src="images/Docker-AB-Benchmark.png" alt="Apache Benchmark in Docker" width="500"/>


#### Container Resource Utilization Analysis

The system resource usage was monitored in real-time using the `htop` command.

The following screenshot shows the system resource usage during the load test, monitored in real time with the `htop` command.

The CPU utilization peaked at approximately **9.8%** 

<img src="images/Docker-CPU-Load.png" alt="CPU Load during Docker Benchmark" width="500"/>


#### Comparison: Host vs Docker under Simulated DDoS Load

The benchmarking results reveal a clear distinction between running **Nginx directly on the host system** versus inside a **Docker container**. Both tests were executed under identical conditions with ApacheBench (`ab -n 100000 -c 100`), ensuring the comparison is **fair and reproducible**.  


📊 **Benchmark Summary**

| Environment       | Requests Sent | Concurrency | Peak CPU Load |
|-------------------|---------------|-------------|---------------|
| 🖥️ Host (bare-metal) | 100,000       | 100         | 🔴 **26%**    |
| 📦 Docker container  | 100,000       | 100         | 🟢 **9.8%**   |



🔎 **Analysis**  

- On bare metal, CPU utilization peaked at **26%**, while the Dockerized environment consumed **less than half** of that at **9.8%**.  
- Containerization introduces measurable differences in **workload scheduling** and **isolation** at the system level.  
- The reduced CPU load in Docker highlights how **abstraction layers** can enable more efficient task distribution, though at the expense of an additional software stack between the hardware and application.  
- This demonstrates the trade-off between:  
  - ⚡ **Maximum raw performance** (host)  
  - 🔄 **Operational benefits** (Docker) — portability, reproducibility, and alignment with cloud-native deployment models.  



✅ **Key Takeaway:**  
This comparison validates the **functional equivalence** of both setups while showing why containerized environments dominate modern infrastructure: they balance **efficiency, scalability, and isolation** against minimal performance overhead.

💻**Commands Used:**  
```bash
# Install Docker engine
sudo apt install docker.io

# Run an Nginx container on port 80
sudo docker run -d -p 80:80 nginx

# Simulate 100,000 HTTP requests with concurrency 100 inside Docker
ab -n 100000 -c 100 http://127.0.0.1/

# Monitor CPU load while the container is under stress
htop
```

---



### High-Concurrency Stress Test


#### 📊 Extreme Load Benchmarking (1M Requests @ Concurrency 1000)

The system was stress-tested with **1,000,000 requests** at a concurrency level of **1000** using Apache Benchmark (`ab`).  
This experiment was designed to determine the **maximum request threshold** the server could handle before risking unresponsiveness or failure.  
Such stress testing provides valuable insights into **scalability, fault tolerance, and robustness** of the system under near-extreme load conditions.  

<img src="images/ApacheBench-1M-Requests.png" alt="Apache Benchmark 1M requests" width="500"/>

---

📑 **Results Overview**  

| Metric                | Value             |
|-----------------------|-------------------|
| Total Requests Sent   | 1,000,000         |
| Concurrency Level     | 1000              |
| Outcome               | ✅ No crash, fully stable |
| Peak CPU Load Observed| ~**9.8%**         |


🔎 **Analysis**  

**Stability under extreme load:**  
The server successfully processed **1 million requests** without crashing or becoming unresponsive, highlighting strong stability even under simulated DDoS-like conditions.  

**Efficient resource utilization:**  
CPU load remained at ~**9.8%**, which is notably efficient given the high concurrency level. This suggests the server can handle intensive workloads without exhausting system resources.  

**Scalability confirmed:**  
By sustaining a concurrency of **1000**, the system demonstrated scalability, validating its ability to support high levels of parallel traffic.  

**Practical implication:**  
These results indicate that the system is capable of withstanding sudden surges in traffic while maintaining availability, which represents a critical capability for environments that are exposed to real-world denial-of-service threats. This resilience highlights not only the robustness of the server under high concurrency but also its reliability in scenarios where continuous availability is essential for operational stability.



🔄 **Comparison with Earlier Tests**  

| Test Scenario         | Requests Sent | Concurrency | Peak CPU Load | Outcome                        |
|-----------------------|---------------|-------------|---------------|--------------------------------|
| Host (bare-metal)     | 100,000       | 100         | 🔴 **26%**     | Stable, no crash               |
| Docker container      | 100,000       | 100         | 🟢 **9.8%**    | Stable, no crash               |
| Stress Threshold (Docker)| 1,000,000   | 1000        | 🟢 **~9.8%**   | Stable, no crash (very resilient)|



✅ **Takeaway:**  
This progression shows how the system scaled from handling **100k requests** (both host and Docker) to **1 million requests** under **10x concurrency** without degradation.  
This highlights not only the system’s **raw performance capacity**, but also the **efficiency and resilience** of the Dockerized environment under stress.  


💻 **Commands Used**  
```bash
# Stress test with 1 million requests and concurrency 1000
ab -n 1000000 -c 1000 http://127.0.0.1
```




