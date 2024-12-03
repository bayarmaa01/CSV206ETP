# Unit 3: Server Monitoring and Management

**Course:** DevOps Automation Lab (Automation & Continuous Monitoring Lab)

---

## Introduction

In this unit, you will focus on automating tasks for server monitoring and management:

1. Send a daily summary of web server requests via email.
2. Continuously monitor the web server status and restart it automatically if it stops running.

These tasks ensure reliable server operation and provide insights into server usage.

---

## Objectives

By the end of this unit, you will be able to:
1. Create a script to generate and email daily web server request summaries.
2. Develop a monitoring system to check web server status and restart it if required.
3. Automate server management using `cron` and `systemd` services.

---

## Prerequisites

- Linux-based system with a web server installed (e.g., Apache or Nginx).
- Email service configured (e.g., `sendmail` or `mailutils` installed).
- Basic knowledge of Bash scripting and system administration.

---

## Step-by-Step Practical

---

### Part 1: Email Daily Summary of Web Server Requests

**Overview:**  
We will create a script that processes the web server's access logs to generate a summary of requests and sends it via email.

---

#### Task: Generate and Email Web Server Request Summary

1. **Create a script file:**
   ```bash
   nano email_webserver_summary.sh
   Add the following code:

bash
Copy code
#!/bin/bash

# Configuration
LOG_FILE="/var/log/apache2/access.log"  # Replace with your server's access log path
SUMMARY_FILE="/tmp/webserver_summary.txt"
EMAIL="your_email@example.com"  # Replace with the recipient's email

# Generate summary
echo "Web Server Request Summary for $(date +"%Y-%m-%d")" > "$SUMMARY_FILE"
echo "----------------------------------------------" >> "$SUMMARY_FILE"
echo "Total Requests:" >> "$SUMMARY_FILE"
wc -l "$LOG_FILE" | awk '{print $1}' >> "$SUMMARY_FILE"

echo "Requests by Status Code:" >> "$SUMMARY_FILE"
awk '{print $9}' "$LOG_FILE" | sort | uniq -c | sort -nr >> "$SUMMARY_FILE"

echo "Top 10 Requested URLs:" >> "$SUMMARY_FILE"
awk '{print $7}' "$LOG_FILE" | sort | uniq -c | sort -nr | head -10 >> "$SUMMARY_FILE"

# Send summary via email
mail -s "Daily Web Server Summary" "$EMAIL" < "$SUMMARY_FILE"
echo "Summary emailed to $EMAIL"
Save and exit the editor: CTRL+O, CTRL+X.

Make the script executable:

bash
Copy code
chmod +x email_webserver_summary.sh
Test the script by running it manually:

bash
Copy code
./email_webserver_summary.sh
Schedule the script to run daily using cron:

Edit the crontab:
bash
Copy code
crontab -e
Add the following entry to run the script daily at midnight:
bash
Copy code
0 0 * * * /path/to/email_webserver_summary.sh
Part 2: Monitor and Restart the Web Server
Overview:
We will create a script to continuously monitor the web server's status and restart it if it's not running.

Task: Monitor Web Server Status and Restart if Required
Create a script file:

bash
Copy code
nano monitor_webserver.sh
Add the following code:

bash
Copy code
#!/bin/bash

# Configuration
WEB_SERVER="apache2"  # Replace with your web server name (e.g., nginx)
CHECK_INTERVAL=60  # Time interval (in seconds) between checks

while true; do
    # Check if the web server is running
    if ! systemctl is-active --quiet "$WEB_SERVER"; then
        echo "$(date): $WEB_SERVER is not running. Restarting..." >> /var/log/webserver_monitor.log
        systemctl restart "$WEB_SERVER"
        if systemctl is-active --quiet "$WEB_SERVER"; then
            echo "$(date): $WEB_SERVER restarted successfully." >> /var/log/webserver_monitor.log
        else
            echo "$(date): Failed to restart $WEB_SERVER!" >> /var/log/webserver_monitor.log
        fi
    fi
    sleep "$CHECK_INTERVAL"
done
Save and exit the editor: CTRL+O, CTRL+X.

Make the script executable:

bash
Copy code
chmod +x monitor_webserver.sh
Test the script by running it manually:

bash
Copy code
./monitor_webserver.sh
Task: Automate Web Server Monitoring
Create a systemd service file:

bash
Copy code
sudo nano /etc/systemd/system/webserver_monitor.service
Add the following content:

ini
Copy code
[Unit]
Description=Web Server Monitoring Service
After=network.target

[Service]
ExecStart=/path/to/monitor_webserver.sh
Restart=always

[Install]
WantedBy=multi-user.target
Reload systemd configuration:

bash
Copy code
sudo systemctl daemon-reload
Enable and start the service:

bash
Copy code
sudo systemctl enable webserver_monitor
sudo systemctl start webserver_monitor
Check the service status:

bash
Copy code
sudo systemctl status webserver_monitor
Exercises
Modify the email_webserver_summary.sh script to include the top 5 IP addresses making requests.
Enhance the monitor_webserver.sh script to send an email notification when the server is restarted.
Adjust the systemd service to log errors to a separate log file for better error tracking.
Best Practices
Ensure the web server logs are rotated to prevent large files from consuming disk space.
Periodically test the monitoring script and service to ensure reliability.
Secure sensitive information (e.g., email credentials) using environment variables or secure files.
Follow your organization’s incident response plan when the web server fails.
Conclusion
In this unit, you have learned how to:

Automate the daily generation and emailing of web server request summaries.
Monitor the web server continuously and restart it if necessary.
These tasks are essential for reliable server operations and proactive issue resolution in a DevOps environment. Keep practicing and refining your scripts for better efficiency.

css
Copy code

### Steps to Add to GitHub

1. Save the content to a file named `Unit_3.md`.
2. Add it to your GitHub repository:
   ```bash
   git add Unit_3.md
   git commit -m "Add Unit 3: Server Monitoring and Management"
   git push origin main
Verify the file is correctly displayed on your GitHub repository.
Let me know if you need further clarifications!
