Here’s a detailed step-by-step guide for **Unit 6: Monitoring Tools and Techniques** for your DevOps Automation & Continuous Monitoring Lab:

---

# Unit 6: Monitoring Tools and Techniques
**Topics Covered:**
- Nagios Installation and Configuration
- Working with SNMP
- Log Monitoring using Nagios
- Implementing a Basic Prometheus and Grafana Monitoring Setup

---

## Introduction

In this unit, you will learn about popular monitoring tools used in a DevOps environment, such as **Nagios**, **SNMP (Simple Network Management Protocol)**, and **Prometheus/Grafana**. These tools help track the performance of infrastructure, applications, and services, ensuring the system's health and uptime.

---

## Objectives
By the end of this unit, you will be able to:
1. Install and configure **Nagios** for system monitoring.
2. Understand how **SNMP** is used for network monitoring.
3. Set up **log monitoring** using Nagios.
4. Implement a **basic Prometheus** and **Grafana** monitoring setup for system and application performance.

---

## Prerequisites
- A Linux-based system (Ubuntu 20.04 LTS recommended)
- Basic knowledge of Bash scripting
- sudo privileges on your system
- Some understanding of networking concepts for SNMP

---

### Part 1: **Nagios Installation and Configuration**

Nagios is a powerful monitoring system used to monitor servers, services, and applications.

#### 1.1 Install Nagios Core
To install Nagios Core on your system, follow the steps below:

1. Update the system:
   ```bash
   sudo apt-get update
   sudo apt-get upgrade
   ```

2. Install the required dependencies:
   ```bash
   sudo apt-get install -y autoconf gcc libssl-dev unzip apache2 libapache2-mod-php
   sudo apt-get install -y libgd-dev libmcrypt-dev
   ```

3. Download the latest Nagios Core tarball:
   ```bash
   cd /tmp
   wget https://github.com/NagiosEnterprises/nagioscore/releases/download/4.4.6/nagios-4.4.6.tar.gz
   ```

4. Extract the downloaded file:
   ```bash
   tar -zxvf nagios-4.4.6.tar.gz
   ```

5. Navigate to the Nagios directory and compile the source:
   ```bash
   cd nagios-4.4.6
   sudo ./configure --with-httpd-conf=/etc/apache2/sites-enabled
   sudo make all
   sudo make install
   ```

6. Install the Nagios command and configuration files:
   ```bash
   sudo make install-init
   sudo make install-commandmode
   sudo make install-config
   ```

7. Configure Nagios to run as a service:
   ```bash
   sudo systemctl enable nagios
   sudo systemctl start nagios
   ```

#### 1.2 Set Up Nagios Web Interface

1. Create a Nagios web user:
   ```bash
   sudo htpasswd -c /usr/local/nagios/etc/htpasswd.users nagiosadmin
   ```

2. Restart Apache to apply the changes:
   ```bash
   sudo systemctl restart apache2
   ```

3. Access the Nagios web interface by navigating to:
   ```
   http://<your_server_ip>/nagios
   ```
   Log in using the username `nagiosadmin` and the password you set.

---

### Part 2: **Working with SNMP**

**SNMP** is a protocol used for managing devices on IP networks. It allows monitoring and gathering information from routers, switches, servers, and other network devices.

#### 2.1 Install SNMP on Linux Server

1. Install SNMP and SNMP utilities:
   ```bash
   sudo apt-get install snmpd snmp
   ```

2. Configure the SNMP service:
   Edit the SNMP configuration file:
   ```bash
   sudo nano /etc/snmp/snmpd.conf
   ```

3. Modify the configuration to allow monitoring, for example:
   ```bash
   rocommunity public
   syslocation "Server Location"
   syscontact "Admin <admin@example.com>"
   ```

4. Restart the SNMP service:
   ```bash
   sudo systemctl restart snmpd
   ```

#### 2.2 Check SNMP Functionality

To test SNMP functionality:
```bash
snmpwalk -v2c -c public localhost
```
This should return a list of SNMP data points from the server.

---

### Part 3: **Log Monitoring using Nagios**

Monitoring logs can be a crucial aspect of maintaining server health. Nagios can be configured to monitor log files and send alerts based on specific events.

#### 3.1 Configure Log Monitoring with Nagios

1. Install the **Nagios Log Monitoring Plugin**:
   ```bash
   sudo apt-get install nagios-plugins
   ```

2. Add a custom Nagios service to monitor logs:
   Create a new configuration file for monitoring:
   ```bash
   sudo nano /usr/local/nagios/etc/objects/logcheck.cfg
   ```

3. Add the following service definition:
   ```bash
   define service{
       use                     generic-service
       host_name               localhost
       service_description     Check Logs
       check_command           check_logfiles!'-f /var/log/syslog -s 100'
       normal_check_interval   5
       retry_check_interval    1
   }
   ```

4. Check Nagios configuration:
   ```bash
   sudo nagios -v /usr/local/nagios/etc/nagios.cfg
   ```

5. Restart Nagios:
   ```bash
   sudo systemctl restart nagios
   ```

---

### Part 4: **Implementing a Basic Prometheus and Grafana Setup**

**Prometheus** is an open-source monitoring system that collects and stores metrics, and **Grafana** is a visualization tool that integrates with Prometheus to display metrics.

#### 4.1 Install Prometheus

1. Download Prometheus:
   ```bash
   wget https://github.com/prometheus/prometheus/releases/download/v2.44.0/prometheus-2.44.0.linux-amd64.tar.gz
   tar -xvf prometheus-2.44.0.linux-amd64.tar.gz
   cd prometheus-2.44.0.linux-amd64
   ```

2. Start Prometheus:
   ```bash
   ./prometheus --config.file=prometheus.yml
   ```

3. Access Prometheus Web UI:
   Visit `http://<your_server_ip>:9090` to access the Prometheus dashboard.

#### 4.2 Install Grafana

1. Install Grafana:
   ```bash
   sudo apt-get install -y software-properties-common
   sudo add-apt-repository "deb https://packages.grafana.com/oss/deb stable main"
   sudo apt-get update
   sudo apt-get install grafana
   ```

2. Start and enable Grafana:
   ```bash
   sudo systemctl enable grafana-server
   sudo systemctl start grafana-server
   ```

3. Access Grafana UI:
   Visit `http://<your_server_ip>:3000` to log into Grafana (default username/password: `admin`).

#### 4.3 Configure Grafana with Prometheus

1. Add Prometheus as a data source in Grafana:
   - Go to **Configuration > Data Sources** in Grafana.
   - Select **Prometheus** and set the URL as `http://localhost:9090`.

2. Create Dashboards to visualize metrics from Prometheus:
   - Go to **Create > Dashboard** and choose a visualization type (e.g., Graph, Table).
   - Select a Prometheus query to display the data.

---

### Exercises

1. Modify the **Nagios** configuration to monitor a custom service (e.g., web server or database).
2. Set up **SNMP traps** to receive alerts for network devices.
3. Create a **Prometheus alert** to notify you when disk space usage exceeds a certain threshold.
4. Build a custom **Grafana dashboard** to monitor CPU and memory usage over time.
5. Integrate **Nagios** with **Prometheus** to display Nagios status on Grafana.

---

### Best Practices

1. Regularly update your monitoring configurations to include new services or components.
2. Set appropriate alert thresholds to avoid alert fatigue.
3. Use Grafana dashboards to provide meaningful insights and trends over time.
4. Always test your monitoring setup in a staging environment before deploying it to production.
5. Implement security measures (such as restricting access to monitoring interfaces) to protect monitoring tools from unauthorized access.

---

### Conclusion

In this unit, you learned how to install and configure Nagios for system monitoring, use SNMP for network device management, and monitor logs via Nagios. Additionally, you implemented a basic Prometheus and Grafana setup for advanced monitoring and visualization. These tools provide essential monitoring capabilities for maintaining healthy and efficient infrastructure in a DevOps environment.

