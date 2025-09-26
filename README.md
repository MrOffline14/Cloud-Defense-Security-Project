# Cloud-Defense-Security-Project

## System Perfomance Benchmarking and Load Testing
This part of the project demonstrates how to analyze system resources, test performance under load, compare a web server on the host versus in a container, and perform stress tests with high concurrency.

### 1. System Information Analysis
--- 
#### a) CPU / Cores and Bogomips
The first step towards perfomance benchmarking and load testing is collecting information about system's processor. This includes number of cores and the Bogomips value per core, providing a baseline understanding of the system's capacity and hardware characteristics.

The screenshot shows the result of running the `lscpu` command, which provides details such as CPU architecture, model, number of cores, and the Bogomips value.
<br></br>

 **Number of CPUs/Core:** 2   **BogoMips per core:** 5587.06  
<img src="images/CPU-Information(lscpu).png" alt="CPU Info (lscpu)" width="500"/> 




`lscpu` also lists CPU vulnerabilities along with mitigation details as shown under, which show the security measures the system applies to reduce the risk of exploiting known hardware vulnerabilities.

<img src="images/CPU-vuln.png" alt="CPU Vulnerabilities" width="500"/> 



*Additionally, `cat /proc/cpuinfo | grep bogomips` can be used to confirm the Bogomips value directly from the CPU info file, showing identical results for each core.*

<img src="images/CPU-Bogomips-verify.png" alt="Bogomips from /proc/cpuinfo" width="500"/> 




**Commands Used:**
```bash
lscpu
cat/proc/cpuinfo | grep bogomips

```


### b) Open Network Ports

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



### 2. Webserver Setup and Local Load Testing
---
### a) Nginx Installation

The next step is to set up a web server to be used for load testing.  
For this project, **Nginx** was chosen due to its lightweight design and high performance.  

Nginx was successfully installed and started on the VM. The installation process downloaded and configured the necessary packages, and the service status was checked to confirm that the server was running.


<img src="images/Nginx-Installation.png" alt="Nginx installation process" width="500"/>



### 3. Containerized Load Testing with Docker

### 4. High-Concurrency Stress Test
