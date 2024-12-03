# Unit 4: System Administration and Security

---

## Introduction

In this unit, we will perform practical tasks related to system administration and security, including:
1. Blocking the execution of forbidden commands to prevent critical system damage.
2. Monitoring disk usage and sending alerts when it exceeds a specified threshold.

These tasks are essential for maintaining system security and stability in a DevOps environment.

---

## Objectives

By the end of this unit, you will:
1. Implement a script to block forbidden commands.
2. Create a disk usage monitoring script that sends alerts for high usage.

---

## Prerequisites

- A Linux-based system (e.g., Ubuntu 20.04 LTS recommended).
- Basic knowledge of Bash scripting.
- Administrative (sudo) privileges on the system.

---

## Part 1: Blocking Forbidden Commands

### Step 1: Script to Block Commands

1. Create a file named `block_commands.sh`:
   ```bash
   nano block_commands.sh


2. Add the following code to the file:
   ```bash
   #!/bin/bash

   # List of forbidden commands
   FORBIDDEN_COMMANDS=("rm -rf /" "mkfs" "dd" "format")

   # Function to check if a command is forbidden
   is_forbidden() {
       local cmd="$1"
       for forbidden in "${FORBIDDEN_COMMANDS[@]}"; do
           if [[ "$cmd" == *"$forbidden"* ]]; then
               return 0
           fi
       done
       return 1
   }

   # Main script loop
   while true; do
       read -p "Enter a command: " user_command
       if is_forbidden "$user_command"; then
           echo "Error: This command is forbidden and cannot be executed."
       else
           echo "Executing: $user_command"
           eval "$user_command"
       fi
   done
   ```

3. Save the file and exit the editor:
   - Press `CTRL+O` to save.
   - Press `CTRL+X` to exit.

4. Make the script executable:
   ```bash
   chmod +x block_commands.sh
   ```

5. Run the script:
   ```bash
   ./block_commands.sh
   ```

6. Test the script by trying:
   - Forbidden commands like `rm -rf /` or `mkfs`.
   - Allowed commands like `ls` or `echo`.

---

## Part 2: Monitoring Disk Usage

### Step 1: Script to Monitor Disk Usage and Alert

1. Create a file named `monitor_disk_usage.sh`:
   ```bash
   nano monitor_disk_usage.sh
   ```

2. Add the following code to monitor disk usage:
   ```bash
   #!/bin/bash

   # Configuration
   THRESHOLD=80  # Disk usage threshold (%)
   CHECK_INTERVAL=300  # Check every 5 minutes (300 seconds)
   ALERT_EMAIL="admin@example.com"  # Change this to your email

   # Function to get disk usage
   get_disk_usage() {
       df -h / | awk 'NR==2 {print $5}' | sed 's/%//'
   }

   # Function to send email alerts
   send_alert() {
       local usage=$1
       echo "Disk usage alert: ${usage}% of disk space used." | mail -s "High Disk Usage Alert" "$ALERT_EMAIL"
   }

   # Main monitoring loop
   while true; do
       usage=$(get_disk_usage)
       if [ "$usage" -gt "$THRESHOLD" ]; then
           echo "Warning: Disk usage is at ${usage}%"
           send_alert "$usage"
       else
           echo "Disk usage is at ${usage}%"
       fi
       sleep "$CHECK_INTERVAL"
   done
   ```

3. Save the file and exit the editor:
   - Press `CTRL+O` to save.
   - Press `CTRL+X` to exit.

4. Make the script executable:
   ```bash
   chmod +x monitor_disk_usage.sh
   ```

5. Install `mailutils` for email alerts:
   ```bash
   sudo apt-get update
   sudo apt-get install mailutils
   ```

6. Run the script:
   ```bash
   ./monitor_disk_usage.sh
   ```

### Testing
- Fill your disk space intentionally to exceed the threshold and check if alerts are generated.
- Verify that the email notification is sent.

---

## Exercises

1. **Enhanced Logging for `block_commands.sh`:**
   - Modify the script to log forbidden command attempts to a file (`/var/log/forbidden_commands.log`).

2. **Monitor Multiple Partitions:**
   - Update the `monitor_disk_usage.sh` script to monitor multiple partitions, such as `/home` and `/var`.

3. **Auto Cleanup of Old Files:**
   - Extend the disk monitoring script to delete old files (older than 7 days) when disk usage is high.

---

## Best Practices

- Regularly review and update the forbidden commands list.
- Use a secure method to store sensitive information like email addresses.
- Regularly test monitoring and alerting scripts to ensure reliability.

---

## Conclusion

By completing this unit, you have learned how to:
- Block forbidden commands to prevent accidental or malicious system damage.
- Monitor disk usage and alert administrators about critical thresholds.

These tasks are foundational in maintaining a secure and reliable DevOps environment.
```

This `.md` file provides a clear, step-by-step guide suitable for a GitHub repository.
