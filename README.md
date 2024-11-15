# Grafana-Monitoring-Tool
Monitoring in Action: Real-Time Insights with Grafana, Prometheus, and Node Exporter.
## Introduction to the Tools
### Grafana: 
A data visualization and alerting tool that connects to Prometheus to create dynamic and customizable dashboards for monitoring metrics.
### Prometheus:
An open-source monitoring tool used to collect, store, and query metrics data. It pulls in data from different sources (like Node Exporter) at regular intervals.
### Node Exporter:
A tool that provides system-level metrics (CPU, memory, disk usage, etc.) from the server or node it’s installed on. This acts as a source of data for Prometheus.
## How the Tools Work Together
*Data Collection* Node Exporter collects system metrics from your server.
*Data Storage and Retrieval* Prometheus pulls metrics from Node Exporter and stores them. Prometheus also provides a query language (PromQL) for filtering and analyzing the metrics.
*Visualization* Grafana connects to Prometheus to retrieve these metrics and display them on custom dashboards. You can set up alerts in Grafana to notify you when specific conditions are met.

# Grafana Installation & Setup in Linux AMI
# Grafana Installation & Setup Guide

This guide covers two methods for installing Grafana on Linux AMI:
1. Standalone installation with Linux binaries
2. Installation using Docker

## Table of Contents
- [Standalone Grafana Installation on Linux](#standalone-grafana-installation-on-linux)
- [Grafana Installation using Docker](#grafana-installation-using-docker)
- [Accessing Grafana](#accessing-grafana)

---

### Standalone Grafana Installation on Linux

1. **Download Grafana**  
   Go to the [Grafana download page](https://grafana.com/grafana/download) and select:
   - **Linux**
   - **Standalone Linux Binaries**

2. **Extract the Package**  
   Run the following command to extract the downloaded file:
   ``` tar -zxvf grafana-enterprise-11.3.0+security-01.linux-amd64.tar.gz ```
   ``` cd grafana-v11.3.0+security-01/ ```
   To start Grafana server:
   ``` ./bin/grafana-server ```
   
