Sure! Here's an example of a **Unit 5** markdown file (`Unit5.md`) for your DevOps Automation Lab, focusing on **Scheduling and Time-based Tasks** with **Cron** and **Managing Time in Nagios**. This step-by-step practical guide includes definitions, explanations, and answers for each task.

---

# CSV 206 – DevOps Automation Lab  
## Unit 5: Scheduling and Time-based Tasks with Cron, Managing Time in Nagios

### Introduction

In this unit, we will focus on two essential components for automating tasks and ensuring time-based operations in a DevOps environment:
1. **Scheduling tasks with Cron** – Automating repetitive tasks in Linux systems.
2. **Managing time-based tasks in Nagios** – Monitoring and alerting based on time conditions.

### Objectives

By the end of this unit, you will be able to:
1. Set up and manage Cron jobs to schedule tasks.
2. Use Cron to automate system maintenance tasks.
3. Configure Nagios to monitor and alert based on time conditions.
4. Understand and apply the concepts of time-based automation and monitoring in a DevOps environment.

### Prerequisites

- A Linux-based system (Ubuntu 20.04 LTS recommended)
- Basic knowledge of Bash scripting and system administration
- A working knowledge of **Nagios** (for time-based monitoring)
- **sudo** privileges on your system

---

## Part 1: Scheduling Time-based Tasks with Cron

Cron is a powerful utility in Unix-like systems used to schedule and automate the execution of tasks at specified times or intervals. It can be used to run scripts, commands, or programs on a fixed schedule.

### Step 1: Understanding the Cron Syntax

Cron jobs are configured in the **crontab** (cron table) file. Each line in the crontab file follows this format:

```
* * * * * command_to_execute
- - - - -
| | | | |  
| | | | +---- Day of week (0 - 6) (Sunday = 0)
| | | +------ Month (1 - 12)
| | +-------- Day of month (1 - 31)
| +---------- Hour (0 - 23)
+------------ Minute (0 - 59)
```

For example:
- `30 2 * * * /path/to/script.sh` – This cron job will execute the script at **2:30 AM** every day.

### Step 2: Creating Cron Jobs

1. **Open the crontab file** for editing:

   ```bash
   crontab -e
   ```

2. **Add a simple cron job** that runs a script every day at 2:30 AM:

   ```bash
   30 2 * * * /home/username/myscript.sh
   ```

3. **Save and close** the editor. This will schedule the script to run at the specified time.

### Step 3: Verify Cron Jobs

To verify your scheduled cron jobs, run:

```bash
crontab -l
```

This will display all cron jobs currently scheduled for your user.

### Step 4: Cron Job for System Maintenance

Let's create a cron job that runs a system maintenance script every Sunday at midnight.

1. Create a maintenance script, `maintenance.sh`:

   ```bash
   nano /home/username/maintenance.sh
   ```

2. Add the following content:

   ```bash
   #!/bin/bash
   echo "Running system maintenance..."
   sudo apt-get update -y
   sudo apt-get upgrade -y
   sudo apt-get autoremove -y
   ```

3. Make the script executable:

   ```bash
   chmod +x /home/username/maintenance.sh
   ```

4. Add the cron job to run it every Sunday at midnight:

   ```bash
   0 0 * * 0 /home/username/maintenance.sh
   ```

---

## Part 2: Managing Time-based Tasks in Nagios

Nagios is a powerful monitoring system used to monitor the status of various services, resources, and time-based conditions on a server. In this section, we will set up time-based monitoring and alerts using Nagios.

### Step 1: Install Nagios

1. **Install Nagios Core** on your system:

   ```bash
   sudo apt-get update
   sudo apt-get install nagios3
   ```

2. **Check Nagios service status**:

   ```bash
   sudo systemctl status nagios3
   ```

3. **Start and enable the Nagios service**:

   ```bash
   sudo systemctl start nagios3
   sudo systemctl enable nagios3
   ```

4. **Access the Nagios web interface**:
   Open a browser and go to `http://<your_server_ip>/nagios3`. Log in with the credentials (usually `nagiosadmin`).

### Step 2: Configure Time-based Monitoring in Nagios

1. **Create a time-based check for disk usage**. For example, we want to monitor disk usage but only send an alert if it exceeds 80% after 6 PM.

2. Open the Nagios configuration file for disk usage monitoring:

   ```bash
   sudo nano /usr/local/nagios/etc/objects/localhost.cfg
   ```

3. **Add a time-based condition** for disk usage:

   ```bash
   define service {
       use                     generic-service
       host_name               localhost
       service_description     Disk Usage
       check_command           check_disk!80%!90%
       check_interval          5
       retry_interval          1
       check_period            weekday
       notification_interval   60
       notification_period     24x7
   }
   ```

   - `check_command check_disk!80%!90%` will monitor the disk usage, alerting when it exceeds 80% and sending a critical alert at 90%.
   - `check_period weekday` ensures the check only runs during the weekday, but you can adjust this to your needs.

4. **Define the time period for monitoring**:

   Add the following to the same configuration file to set the weekdays and time range:

   ```bash
   define timeperiod {
       timeperiod_name  weekday
       alias            Weekdays
       sunday           00:00-00:00
       monday           18:00-23:59
       tuesday          18:00-23:59
       wednesday        18:00-23:59
       thursday         18:00-23:59
       friday           18:00-23:59
       saturday         00:00-00:00
   }
   ```

   This configuration ensures disk usage is only monitored and alerts are sent between 6 PM and midnight on weekdays.

### Step 3: Restart Nagios

After making the changes, restart Nagios to apply the new configuration:

```bash
sudo systemctl restart nagios3
```

### Step 4: Test the Configuration

1. To test the time-based alert, trigger a disk usage alert by manually filling up the disk or waiting for it to exceed the threshold.

2. You will receive an alert only if the disk usage exceeds the threshold during the defined time period.

---

### Exercises

1. **Create a cron job to backup system files** every day at 3:00 AM and send an email notification on success or failure.
   
   Answer:
   - Create a backup script (`backup.sh`) and schedule it using Cron.
   - Use `mail` to send an email on completion or failure.

2. **Modify Nagios to monitor CPU usage** and send alerts during weekends only.

   Answer:
   - Update the Nagios configuration to monitor CPU usage (`check_cpu`) and define a time period that includes Saturday and Sunday.

3. **Set up multiple time-based checks for Nagios** for different services, such as web server availability only during business hours (9 AM to 6 PM).

   Answer:
   - Define custom time periods for business hours and configure Nagios to check services like `http` availability only during these hours.

---

### Best Practices

1. **Cron Jobs:**
   - Use descriptive comments in crontab files for clarity.
   - Ensure scripts are executable (`chmod +x`).
   - Use `>>` to log output to a file for troubleshooting.

2. **Nagios:**
   - Always test configurations with the `nagios -v` command before restarting the service.
   - Use time-based monitoring sparingly to avoid unnecessary load on the system.
   - Configure email notifications correctly to ensure alerts are received.

---

### Conclusion

In this unit, you learned how to schedule tasks using Cron and manage time-based alerts and monitoring with Nagios. These skills are essential for automating tasks, monitoring system health, and ensuring that systems run smoothly in a DevOps environment.
