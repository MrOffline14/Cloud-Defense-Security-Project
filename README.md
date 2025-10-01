# 📘 Project Overview – Cloud Security, Compliance & Performance Testing  

This project is based on a realistic case where a rapidly growing IT consultancy in Oslo aims to deliver IoT services using Microsoft Azure.  
The company faces increasing demand for scalable and secure services, which introduces challenges related to **architectural choices, regulatory requirements, security governance, and actual system performance under high load**.  

The project is divided into two interconnected parts:  

*   The first part defines a **theoretical architecture and governance model** for a hybrid cloud platform. Key areas of analysis include:  
    *   Selecting the appropriate service model and deployment strategy (PaaS and Hybrid Cloud).  
    *   Meeting regulatory frameworks (GDPR, CCPA, ISO 27001, cross-border data transfers).  
    *   Establishing security training and governance routines (Microsoft SDL, SSDLC).  
    *   Risk management and continuity planning based on the NIST CSF.  

    This section ensures that the solution is **robust on paper**, both technically and organizationally, and provides the foundation for further implementation.  

*   The second part validates and verifies the theoretical principles through **system benchmarking and load testing**. The focus is on:  
    *   Setting up a controlled Ubuntu environment and analyzing resources (CPU, network, bogomips).  
    *   Deploying Nginx and simulating high traffic (hundreds of thousands to millions of requests).  
    *   Evaluating how the system responds to potential DDoS-like scenarios.  
    *   Comparing performance and resource utilization between VM- and container-based (Docker) environments.  
    *   Determining whether the infrastructure can deliver stability and availability in line with business requirements.  

---

## 🔗 Project Structure  

- [Hybrid Cloud, Compliance & Risk Management](Hybrid-Cloud-Comp-Risk.md)  
  Covers the theoretical foundation: service models, deployment strategy, regulatory frameworks, secure development lifecycle, and risk management practices.  

- [System Benchmarking & Load Testing](System%20Benchmarking%20&%20Load%20Testing.md)  
  Validates the theoretical design through practical experiments, including stress testing, performance metrics, and stability analysis.  

---

## ✅ Key Takeaway  

By combining **theory and practice**, this project provides a holistic demonstration of cloud security architecture. From **strategic design principles** to **tested resilience under real-world conditions**.  

