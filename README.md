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
**Data Collection:** <br>
Node Exporter collects system metrics from your server.
<br>
**Data Storage and Retrieval:** <br>
Prometheus pulls metrics from Node Exporter and stores them. Prometheus also provides a query language (PromQL) for filtering and analyzing the metrics.
<br>
**Visualization:** <br>
Grafana connects to Prometheus to retrieve these metrics and display them on custom dashboards. You can set up alerts in Grafana to notify you when specific conditions are met.

### Standalone Grafana Installation on Linux

1. **Download Grafana**  
   Go to the [Grafana download page](https://grafana.com/grafana/download) and select:
   - **Linux**
   - **Standalone Linux Binaries**

2. **Extract the Package**  
   Run the following command to extract the downloaded file:
   ```sh
   tar -zxvf grafana-enterprise-11.3.0+security-01.linux-amd64.tar.gz
   ```
   ```sh
    cd grafana-v11.3.0+security-01/
   ```
   To start Grafana server:
   ```sh
   ./bin/grafana-server
   ```
3. **Accessing Grafana UI**
   After following the installation method, we can access Grafana by visiting:
   ```sh
   http://<Public-IPv4>:3000
   ```
### Grafana Installation using Docker on Linux
1. **Pull Grafana Docker Image**
   ```sh
   docker pull grafana/grafana
   ```
2. **Verify the Image and Run Grafana in a Container by assigning the port no. and verify the running container**
   ```sh
   docker images
   docker run -it -d -p 3000:3000 grafana/grafana
   docker ps
   ```
3. **Accessing Grafana UI**
   After following the installation method, we can access Grafana by visiting:
   ```sh
   http://<Public-IPv4>:3000
   ```
   
---

### Standalone Prometheus Installation on Linux

1. **Download Prometheus**  
   Go to the [Prometheus download page](https://prometheus.io/docs/introduction/first_steps/) and select:
   - **Linux**

2. **Download and Extract the Package**  
   Run the following commands to download and extract the Prometheus package:
   ```sh
   wget https://github.com/prometheus/prometheus/releases/download/v3.0.0-rc.1/prometheus-3.0.0-rc.1.linux-amd64.tar.gz
   tar -zxvf prometheus-3.0.0-rc.1.linux-amd64.tar.gz
   ```
3. **Navigate to Prometheus Directory & View Configuration File**
   ```sh
   cd prometheus-3.0.0-rc.1.linux-amd64/
   cat prometheus.yml
   ```
4. **Start Prometheus Server**
   ```sh
   ./prometheus --config.file=prometheus.yml
   ```
5. **Accessing Prometheus UI**
   After starting Prometheus, we can access it by visiting
   ```sh
   http://<Public-IPv4>:9090
   ```
### Prometheus Installation using Docker on Linux
1. **Pull Prometheus Docker Image**
   ```sh
   docker pull prom/prometheus
   ```
2. **Verify the Image and Run Prometheus in a Container by assigning the port no. and verify the running container**
   ```sh
   docker images
   docker run -it -d -p 9090:9090 prom/prometheus
   docker ps
   ```
3. **Accessing Prometheus UI**
   After following the installation method, we can access Prometheus by visiting:
   ```sh
   http://<Public-IPv4>:3000
   ```

---

### Node Exporter Installation and Configuration on Linux

1. **Download Node Exporter**  
   Go to the [Prometheus Node Exporter download page](https://prometheus.io/download/#node_exporter) and select:
   - **Linux**

   Use the following command to download the package:
   ```sh
   wget https://github.com/prometheus/node_exporter/releases/download/v1.8.2/node_exporter-1.8.2.linux-amd64.tar.gz
   ```
2. **Extract the Package & Navigate to Node Exporter Directory**
   ```sh
   tar -zxvf node_exporter-1.8.2.linux-amd64.tar.gz
   cd node_exporter-1.8.2.linux-amd64/
   ```
3. **Move Node Exporter to a Standard Location**
   ```sh
   sudo cp node_exporter /usr/local/bin
   ```
4. **Verify Node Exporter Installation & Navigate to the directory and check**
   ```sh
   cd /usr/local/bin
   ls
   ```
5. **Set Up Node Exporter User**
   Creating a system user for Node Exporter with restricted access:
   ```sh
   sudo useradd --no-create-home --shell /bin/false node_exporter
   ```
6. **Set Ownership for Node Exporter**
   Change ownership of the binary to the Node Exporter user
   ```sh
   sudo chown node_exporter:node_exporter /usr/local/bin/node_exporter
   ```
7. **Create a Systemd Service File**
   Navigate to the systemd configuration directory & Create a service file
   ```sh
   cd /etc/systemd/system/
   sudo vi node_exporter.service
   ```
   Add the following content:
   ```sh
   [Unit]
   Description=Node Exporter
   Wants=network-online.target
   After=network-online.target

   [Service]
   User=node_exporter
   Group=node_exporter
   Type=simple
   ExecStart=/usr/local/bin/node_exporter
   Restart=always
   RestartSec=3

   [Install]
   WantedBy=multi-user.target
   ```
   
8. **Reload and Enable Node Exporter Service**
   Reload systemd to apply changes and enable the Node Exporter service
   ```sh
   sudo systemctl daemon-reload
   sudo systemctl enable node_exporter
   ```
9. **Start and Verify Node Exporter**
   Start the Node Exporter service & Check the service status
   ```sh
   sudo systemctl start node_exporter
   sudo systemctl status node_exporter.service
   ```
10. **Accessing Node Exporter Metrics**
    After starting Node Exporter, we can access it by visiting:
    ```sh
    http://<Public-IPv4>:9100
    ``` 
---

### Attaching Node Exporter to Prometheus

This section explains how to configure Prometheus to monitor metrics from Node Exporter.

#### Step 1: Stop and Remove the Existing Prometheus Container  
Before attaching Node Exporter, stop and remove any previously running Prometheus container that might be using port 9090.

```sh
docker stop <container_id>
docker rm <container_id>
```
#### Step 2: Modify Prometheus Configuration File
Create and modify the ```prometheus.yml``` configuration file to add Node Exporter as a scrape target. Place this file in the root directory or another accessible location.
**Open the ```prometheus.yml``` file using a text editor**
```sh
vi /root/prometheus.yml
```
**Add the following scrape configuration for Node Exporter:**
```sh
scrape_configs:
  - job_name: 'node-exporter'
    static_configs:
      - targets: ['<public_ip>:9100']
```
Replace <public_ip> with the public IPv4 address of the server running Node Exporter.

#### Step 3: Restart Prometheus with the Updated Configuration
Mount the updated ```prometheus.yml``` configuration file into the Prometheus container and start a new container:
```sh
docker run -d --name=prometheus -p 9090:9090 -v /root/prometheus.yml:/etc/prometheus/prometheus.yml prom/prometheus
```
*This command:* <br>
- Runs Prometheus in detached mode (-d).<br>
- Maps port 9090 on the host to port 9090 in the container.<br>
- Mounts the updated configuration file located at /root/prometheus.yml to the Prometheus container’s configuration directory /etc/prometheus/prometheus.yml.

#### Step 4: Verify the Setup
**Access Prometheus UI: Open the Prometheus web interface**
```sh
http://<Public-IPv4>:9090
```
**Check Targets**
*Navigate to Status > Targets in the Prometheus UI. Ensure that the node-exporter job is listed and its status is UP.*

---

### Setting Up Grafana Dashboard for Node Exporter Metrics
Follow these steps to configure and import a pre-built Node Exporter dashboard into Grafana.

#### Step 1: Add Prometheus as a Data Source  
1. **Access Grafana UI**  
   Open Grafana in your browser:  
   ```sh
   http://<Public-IPv4>:3000
2. **Add Data Source**
- Click Add your First Data Source (or navigate to Configuration > Data Sources if this is not the first data source).
- Select Prometheus as the data source.
- Enter the following details: <br>
   Name: Prometheus (or any meaningful name).<br>
   URL: http://<Public-IPv4>:9090 (replace <Public-IPv4> with the Prometheus server's IP).
3. **Save & Test** <br>
   Click ```Save & Test``` to verify the connection.<br>
   Return to the Home page.
#### Step 2: Create a Dashboard
- Click Create your First Dashboard (or + > Dashboard from the sidebar menu).
- Select Import Dashboard to import a pre-built dashboard.
#### Step 3: Import Node Exporter Dashboard
1. Visit Grafana Dashboards.
2. Search for Node Exporter Full (a popular pre-built dashboard).
3. Choose one of the following options:
- Copy the Dashboard ID from the page (e.g., 1860).
- OR Download the JSON File to your system.
4. Go back to the Grafana UI and import the dashboard:
- Paste the copied Dashboard ID into the Import via grafana.com field.
- OR upload the downloaded JSON File.
5. Select the appropriate Prometheus Data Source from the dropdown during import.
#### Step 4: View and Monitor Metrics
Once the dashboard is imported:
- Navigate to ```Dashboards > Manage Dashboards``` to view the imported Node Exporter dashboard.
- The dashboard will display real-time metrics scraped by Prometheus from Node Exporter.
