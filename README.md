# Cloud-Defense-Security-Project

## System Perfomance Benchmarking and Load Testing
This part of the project demonstrates how to analyze system resources, test performance under load, compare a web server on the host versus in a container, and perform stress tests with high concurrency.

### 1. System Information Analysis
--- 
#### a) CPU / Cores and Bogomips
The first step towards perfomance benchmarking and load testing is collecting information about system's processor. This includes number of cores and the Bogomips value per core, providing a baseline understanding of the system's capacity and hardware characteristics.

!<-- **Number of CPUs/Core:** 2   **BogoMips per core:** 5587.06  
<img src="images/CPU-Information(lscpu).png" alt="CPU Information" width="450"/>


<img src="images/CPU-vuln.png" alt="CPU Bogomips" width="450"/> -->


<img src="images/CPU-Information(lscpu).png" alt="CPU Info (lscpu)" width="500"/> *This screenshot shows the `lscpu` output, including architecture, CPU model, number of cores, and the Bogomips value.* <img src="images/CPU-vuln.png" alt="CPU Vulnerabilities" width="500"/> *This screenshot lists CPU vulnerabilities and mitigation details provided by the system.* <img src="images/CPU-Bogomips-verify.png" alt="Bogomips from /proc/cpuinfo" width="500"/> *This screenshot confirms the Bogomips value using `cat /proc/cpuinfo | grep bogomips`, showing identical results for each core.* ```

**Commands Used:**
```bash
lscpu
cat/proc/cpuinfo | grep bogomips

````




### 2. Webserver Setup and Local Load Testing

### 3. Containerized Load Testing with Docker

### 4. High-Concurrency Stress Test
