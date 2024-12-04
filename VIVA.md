### **CSV 206 – DevOps Automation Lab Viva Questions with Brief Answers**

---

### **Unit 1: Scripting Fundamentals**

1. **What are conditional statements in scripting?**  
   Conditional statements control the flow of execution based on conditions. Examples include `if`, `else`, `elif`, and `case`.

2. **What are loops in scripting?**  
   Loops are used to execute a block of code repeatedly. Common loops include `for`, `while`, and `until`.

3. **Why is automation scripting useful?**  
   Automation scripts save time and effort by reducing repetitive manual tasks, improving accuracy, and enhancing productivity.

4. **What is the difference between `for` and `while` loops?**  
   - `for` loop: Iterates over a sequence (e.g., a list or range).  
   - `while` loop: Runs as long as a condition is true.

5. **How can you pass arguments to a shell script?**  
   Arguments are passed using positional parameters like `$1`, `$2`, etc., or `$@` for all arguments.

---

### **Unit 2: File and Database Management Automation**

1. **How do you delete files older than two days automatically?**  
   Use the `find` command with `-mtime +2` and `-exec rm` options:  
   ```bash
   find /path/to/files -mtime +2 -exec rm {} \;
   ```

2. **What is the purpose of automating MySQL backups?**  
   To ensure regular and reliable data backup for disaster recovery.

3. **How do you schedule MySQL backups every 12 hours?**  
   Use a cron job like:  
   ```bash
   0 */12 * * * /path/to/backup_script.sh
   ```

4. **How can files be moved to a backup directory automatically?**  
   Use the `mv` command in a script to move files to the designated directory.

5. **What tools can be used for database backup automation?**  
   Common tools include `mysqldump`, `Percona XtraBackup`, and custom scripts.

---

### **Unit 3: Server Monitoring and Management**

1. **How do you check if a web server is running?**  
   Use the `systemctl` or `ps` command to check the status:  
   ```bash
   systemctl status apache2
   ```

2. **How can you restart a server automatically if it’s not running?**  
   Use a script with a conditional check and `systemctl restart` command, scheduled with cron.

3. **How do you email the summary of web server requests?**  
   Use `awk` or `grep` to process `/var/log/apache2/access.log` and the `mail` command to send the summary.

4. **What are the benefits of server monitoring?**  
   - Ensures uptime and performance.  
   - Detects issues proactively.  
   - Helps maintain SLA compliance.

5. **What tools are commonly used for server monitoring?**  
   Tools like Nagios, Prometheus, Grafana, and Zabbix are widely used.

---

### **Unit 4: System Administration and Security**

1. **How can you block forbidden commands?**  
   Use a `bashrc` script or shell restrictions (e.g., limited shell) to deny specific commands.

2. **How do you monitor disk usage?**  
   Use the `df` and `du` commands or scripts to monitor disk space.

3. **How can alerts be triggered if disk usage exceeds a threshold?**  
   Use a script with `df` and a comparison condition to send alerts via email or log them.

4. **What are common security practices for system administration?**  
   - Use strong passwords.  
   - Restrict access with firewalls.  
   - Regularly update software and OS.  
   - Monitor logs for suspicious activities.

5. **What is SELinux and its purpose?**  
   SELinux (Security-Enhanced Linux) enforces access control policies to enhance system security.

---

### **Unit 5: Scheduling and Time-based Tasks**

1. **What is Cron, and how is it used?**  
   Cron is a time-based job scheduler in Unix-like systems used to execute tasks at specific intervals.

2. **How do you add a cron job?**  
   Use `crontab -e` to edit the cron file and specify the schedule and command.

3. **What is the format of a cron job?**  
   ```bash
   m h dom mon dow command
   ```

4. **What is Nagios?**  
   Nagios is a monitoring system used to check the health of systems, networks, and infrastructure.

5. **How do you troubleshoot cron jobs not running?**  
   - Check the cron service status.  
   - Review logs in `/var/log/syslog`.  
   - Ensure the script is executable.

---

### **Unit 6: Monitoring Tools and Techniques**

1. **How do you install Nagios?**  
   Install Nagios with package managers or source, configure its plugins, and set up monitoring services.

2. **What is SNMP?**  
   SNMP (Simple Network Management Protocol) is used for managing and monitoring network devices.

3. **What is Prometheus, and why is it used?**  
   Prometheus is an open-source monitoring tool for collecting metrics and alerting based on time-series data.

4. **How do you integrate Prometheus with Grafana?**  
   Add Prometheus as a data source in Grafana and create dashboards using PromQL queries.

5. **What are logs, and why are they important?**  
   Logs provide a detailed record of system events and help in debugging, monitoring, and auditing.

---

These questions and answers cover essential concepts and practical tasks in each unit. Let me know if you need further explanations or specific examples!
