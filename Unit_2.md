Here’s a detailed **`Unit_2.md`** file for **Unit 2: File and Database Management Automation** for the **DevOps Automation Lab (Automation & Continuous Monitoring Lab)** syllabus. The file provides a step-by-step guide to automate deleting archive files older than two days and scheduling MySQL backups every 12 hours.

---

## **Unit 2: File and Database Management Automation**  
**Course:** DevOps Automation Lab (Automation & Continuous Monitoring Lab)  

---

### **Introduction**  
In this unit, you will automate essential file and database management tasks:  

1. Automatically delete archive files older than two days to conserve storage space.  
2. Schedule MySQL backups every 12 hours and move them to a designated backup directory for secure storage.  

These tasks help ensure efficient use of resources and reliable data management in a DevOps environment.  

---

### **Objectives**  

By the end of this unit, you will be able to:  
1. Automate file cleanup using Bash scripts.  
2. Schedule and execute MySQL database backups periodically.  
3. Organize and manage backups efficiently in a secure directory.  

---

### **Prerequisites**  

- Linux-based system (Ubuntu 20.04 LTS recommended).  
- Installed MySQL database (`sudo apt-get install mysql-server`).  
- MySQL user credentials with backup permissions.  
- Installed `cron` for task scheduling.  
- Basic knowledge of Bash scripting and Linux commands.  

---

### **Step-by-Step Practical**  

---

#### **Part 1: Automatically Delete Archive Files Older Than Two Days**  

**Overview:**  
We will create a script to identify and delete `.tar.gz` archive files older than two days in a specified directory.  

---

**Task: Delete Old Archive Files**  

1. Create a script file:  
   ```bash
   nano delete_old_archives.sh
   ```  

2. Add the following code:  
   ```bash
   #!/bin/bash

   # Define the target directory
   TARGET_DIR="/path/to/your/archive/directory"

   # Find and delete archive files older than 2 days
   find "$TARGET_DIR" -type f -name "*.tar.gz" -mtime +2 -exec rm -f {} \;

   echo "Old archive files deleted from $TARGET_DIR"
   ```  

3. Save and exit the editor (`CTRL+O`, `CTRL+X`).  

4. Make the script executable:  
   ```bash
   chmod +x delete_old_archives.sh
   ```  

5. Test the script by running it manually:  
   ```bash
   ./delete_old_archives.sh
   ```  

6. Automate the script with `cron` to run daily:  

   - Edit the crontab:  
     ```bash
     crontab -e
     ```  

   - Add the following entry to run the script daily at midnight:  
     ```bash
     0 0 * * * /path/to/delete_old_archives.sh
     ```  

   - Save and exit the editor.  

---

#### **Part 2: Schedule MySQL Backups Every 12 Hours**  

**Overview:**  
We will create a script to back up a MySQL database and move the backup file to a designated directory.  

---

**Task: Create MySQL Backup Script**  

1. Create a script file:  
   ```bash
   nano mysql_backup.sh
   ```  

2. Add the following code:  
   ```bash
   #!/bin/bash

   # Configuration
   BACKUP_DIR="/path/to/backup/directory"
   DB_NAME="your_database_name"
   DB_USER="your_mysql_username"
   DB_PASSWORD="your_mysql_password"

   # Create the backup directory if it doesn't exist
   mkdir -p "$BACKUP_DIR"

   # Generate backup file name with timestamp
   TIMESTAMP=$(date +"%Y%m%d_%H%M%S")
   BACKUP_FILE="$BACKUP_DIR/${DB_NAME}_backup_$TIMESTAMP.sql"

   # Take the MySQL backup
   mysqldump -u "$DB_USER" -p"$DB_PASSWORD" "$DB_NAME" > "$BACKUP_FILE"

   if [ $? -eq 0 ]; then
       echo "MySQL backup successful: $BACKUP_FILE"
   else
       echo "MySQL backup failed!"
       exit 1
   fi
   ```  

3. Save and exit the editor (`CTRL+O`, `CTRL+X`).  

4. Make the script executable:  
   ```bash
   chmod +x mysql_backup.sh
   ```  

5. Test the script by running it manually:  
   ```bash
   ./mysql_backup.sh
   ```  

---

**Task: Automate MySQL Backups Every 12 Hours**  

1. Edit the crontab:  
   ```bash
   crontab -e
   ```  

2. Add the following entry to run the script every 12 hours:  
   ```bash
   0 */12 * * * /path/to/mysql_backup.sh
   ```  

3. Save and exit the editor.  

---

### **Exercises**  

1. Modify the `delete_old_archives.sh` script to log deleted files to a log file for auditing.  
2. Extend the `mysql_backup.sh` script to compress the backup file using `gzip`.  
3. Create a script to rotate backups by keeping only the last 10 backups and deleting older ones.  
4. Combine both scripts into one and allow users to choose between file deletion and MySQL backup operations interactively.  
5. Integrate email notifications for successful or failed MySQL backups using `mailutils`.  

---

### **Best Practices**  

1. Use secure methods to store sensitive credentials (e.g., `.my.cnf` file or environment variables).  
2. Periodically test your scripts and backups to ensure they work as expected.  
3. Set up monitoring to alert if backups fail or disk space is low.  
4. Follow your organization’s data retention and backup policies.  

---

### **Conclusion**  

In this unit, you have learned how to:  
1. Automate file cleanup by deleting archives older than two days.  
2. Schedule MySQL backups every 12 hours and store them securely.  

These skills are crucial for efficient file and database management in any DevOps environment. Continue practicing and refining your scripts to meet real-world requirements.  

--- 

This **`Unit_2.md`** file is now ready for upload to GitHub! Let me know if you'd like to add additional features or customization.
