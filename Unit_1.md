# CSV 206 – DevOps Automation Lab  
## Unit 1: Scripting Fundamentals, Conditional Statements, and Loops  

### Introduction  

In this lab, we'll explore the fundamentals of shell scripting, focusing on automating tasks using conditional statements and loops. These skills are essential for creating scripts that save time, improve productivity, and ensure system consistency in a DevOps environment.  

### Objectives  

By the end of this lab, you will:  
1. Understand the basics of shell scripting.  
2. Use conditional statements (`if`, `else`, `elif`) in Bash.  
3. Work with looping constructs (`for`, `while`, `until`).  
4. Automate common system administration tasks using scripts.  

---

### Prerequisites  

- A Linux-based system (Ubuntu 20.04 LTS recommended).  
- Basic understanding of the Linux command line.  
- Text editor like `nano` or `vim`.  
- `chmod` permissions to make scripts executable.  

---

### Part 1: Basics of Shell Scripting  

1. **Create a script to print a greeting message**  
    - Open a terminal and create a file named `greeting.sh`:  
      ```bash
      nano greeting.sh
      ```  
    - Add the following content:  
      ```bash
      #!/bin/bash
      # A simple script to display a greeting message
      
      echo "Welcome to the DevOps Automation Lab!"
      ```  
    - Save the file and make it executable:  
      ```bash
      chmod +x greeting.sh
      ./greeting.sh
      ```  

    **Output:**  
    ```
    Welcome to the DevOps Automation Lab!
    ```  

---

### Part 2: Conditional Statements  

2. **Create a script to check if a file exists**  
    - Create a file named `check_file.sh`:  
      ```bash
      nano check_file.sh
      ```  
    - Add the following content:  
      ```bash
      #!/bin/bash
      # Script to check if a file exists

      read -p "Enter the file name to check: " file_name
      if [ -e "$file_name" ]; then
          echo "The file '$file_name' exists."
      else
          echo "The file '$file_name' does not exist."
      fi
      ```  
    - Run the script:  
      ```bash
      chmod +x check_file.sh
      ./check_file.sh
      ```  

    **Explanation:**  
    - `-e` checks if the file exists.  
    - `if` evaluates the condition and executes the appropriate block.  

    **Output Example:**  
    ```
    Enter the file name to check: sample.txt  
    The file 'sample.txt' exists.
    ```  

---

### Part 3: Loops  

3. **Automate repetitive tasks using `for` loop**  
    - Create a file named `backup_files.sh`:  
      ```bash
      nano backup_files.sh
      ```  
    - Add the following content:  
      ```bash
      #!/bin/bash
      # Script to back up files in the current directory

      BACKUP_DIR="backup_$(date +%F)"
      mkdir -p "$BACKUP_DIR"

      for file in *; do
          if [ -f "$file" ]; then
              cp "$file" "$BACKUP_DIR"
              echo "Backed up $file to $BACKUP_DIR"
          fi
      done
      ```  
    - Run the script:  
      ```bash
      chmod +x backup_files.sh
      ./backup_files.sh
      ```  

    **Explanation:**  
    - `for file in *` loops through all files in the current directory.  
    - `mkdir -p` ensures the backup directory is created only once.  

    **Output Example:**  
    ```
    Backed up file1.txt to backup_2024-12-04  
    Backed up file2.log to backup_2024-12-04  
    ```  

4. **Monitor a directory using a `while` loop**  
    - Create a file named `monitor_directory.sh`:  
      ```bash
      nano monitor_directory.sh
      ```  
    - Add the following content:  
      ```bash
      #!/bin/bash
      # Monitor a directory for new files

      MONITOR_DIR="/path/to/directory"

      echo "Monitoring $MONITOR_DIR for new files..."
      while true; do
          ls "$MONITOR_DIR"
          sleep 5
      done
      ```  
    - Run the script:  
      ```bash
      chmod +x monitor_directory.sh
      ./monitor_directory.sh
      ```  

    **Explanation:**  
    - `while true` creates an infinite loop to continuously monitor the directory.  
    - `sleep 5` pauses execution for 5 seconds between iterations.  

---

### Part 4: Automation Scripts  

5. **Automate user account creation**  
    - Create a file named `create_users.sh`:  
      ```bash
      nano create_users.sh
      ```  
    - Add the following content:  
      ```bash
      #!/bin/bash
      # Script to automate user account creation

      read -p "Enter username: " username
      sudo useradd "$username"
      echo "User $username has been created successfully."
      ```  
    - Run the script:  
      ```bash
      chmod +x create_users.sh
      ./create_users.sh
      ```  

    **Explanation:**  
    - `sudo useradd` creates a new user.  
    - Input is taken dynamically using `read`.  

---

### Exercises  

1. **Enhance the `check_file.sh` script to handle directories:**  
   Modify the script to check if the input is a directory instead of a file.  
   Use the `-d` option in the conditional statement.  

2. **Update the `backup_files.sh` script to compress backups:**  
   Add a command to create a `.tar.gz` archive of the backup directory.  

3. **Create a script to monitor CPU usage:**  
   Use the `top` or `ps` commands inside a loop to check CPU-intensive processes.  

4. **Combine `monitor_directory.sh` and `backup_files.sh`:**  
   Create a unified script to monitor a directory and back up new files automatically.  

---
Here are the enhanced scripts with explanations for each exercise:

---

### 1. **Enhance the `check_file.sh` script to handle directories**

#### Script:
```bash
#!/bin/bash
# Script to check if the input is a directory

read -p "Enter the path to check: " path

if [ -d "$path" ]; then
    echo "The path '$path' is a directory."
else
    echo "The path '$path' is NOT a directory."
fi
```

#### Explanation:
- `-d "$path"`: Checks if the provided path is a directory.
- If the condition is true, it confirms the path is a directory; otherwise, it prints an error message.

---

### 2. **Update the `backup_files.sh` script to compress backups**

#### Script:
```bash
#!/bin/bash
# Script to back up files and compress them

SOURCE_DIR="/path/to/source"
BACKUP_DIR="/path/to/backup"
TIMESTAMP=$(date +"%Y%m%d_%H%M%S")
ARCHIVE_NAME="backup_$TIMESTAMP.tar.gz"

# Create backup directory if it doesn't exist
mkdir -p "$BACKUP_DIR"

# Copy files to backup directory
cp -r "$SOURCE_DIR"/* "$BACKUP_DIR/"

# Compress the backup directory
tar -czf "$BACKUP_DIR/$ARCHIVE_NAME" -C "$BACKUP_DIR" .

# Confirm backup
if [ $? -eq 0 ]; then
    echo "Backup and compression successful: $BACKUP_DIR/$ARCHIVE_NAME"
else
    echo "Backup failed."
fi
```

#### Explanation:
- `mkdir -p "$BACKUP_DIR"`: Ensures the backup directory exists.
- `tar -czf "$ARCHIVE_NAME"`: Creates a compressed `.tar.gz` archive of the backup directory.
- The script copies files from the source to the backup directory and compresses the result.

---

### 3. **Create a script to monitor CPU usage**

#### Script:
```bash
#!/bin/bash
# Script to monitor CPU usage

THRESHOLD=80  # Set CPU usage threshold

while true; do
    CPU_USAGE=$(top -bn1 | grep "Cpu(s)" | awk '{print $2 + $4}')
    echo "Current CPU usage: $CPU_USAGE%"

    if (( ${CPU_USAGE%.*} > THRESHOLD )); then
        echo "Warning: CPU usage is above $THRESHOLD%!"
    fi

    sleep 5  # Check every 5 seconds
done
```

#### Explanation:
- `top -bn1`: Runs `top` in batch mode to get CPU stats.
- `awk '{print $2 + $4}'`: Extracts the user and system CPU usage.
- `${CPU_USAGE%.*}`: Removes the decimal for integer comparison.
- If usage exceeds the threshold, a warning is printed.

---

### 4. **Combine `monitor_directory.sh` and `backup_files.sh`**

#### Script:
```bash
#!/bin/bash
# Unified script to monitor a directory and back up new files

MONITOR_DIR="/path/to/monitor"
BACKUP_DIR="/path/to/backup"
TIMESTAMP=$(date +"%Y%m%d_%H%M%S")
ARCHIVE_NAME="backup_$TIMESTAMP.tar.gz"

# Create backup directory if it doesn't exist
mkdir -p "$BACKUP_DIR"

# Monitor directory for new files
inotifywait -m -e create "$MONITOR_DIR" --format '%w%f' | while read NEW_FILE; do
    echo "New file detected: $NEW_FILE"

    # Copy new file to backup directory
    cp "$NEW_FILE" "$BACKUP_DIR"

    # Compress the backup directory
    tar -czf "$BACKUP_DIR/$ARCHIVE_NAME" -C "$BACKUP_DIR" .

    echo "Backup updated and compressed: $BACKUP_DIR/$ARCHIVE_NAME"
done
```

#### Explanation:
- `inotifywait -m -e create`: Monitors the directory for new files.
- `cp "$NEW_FILE" "$BACKUP_DIR"`: Copies the new file to the backup directory.
- `tar -czf "$ARCHIVE_NAME"`: Compresses the updated backup.
- The script continuously listens for changes and updates the backup dynamically.

---

### Summary:
These scripts address the given exercises:
1. **Check if the input is a directory** using the `-d` option.
2. **Compress backups** with `tar`.
3. **Monitor CPU usage** using `top`.
4. **Combine directory monitoring and backup** using `inotifywait`.

Each script provides functionality with clear feedback and can be further customized to suit your requirements. Let me know if you need more clarification!
### Best Practices  

1. **Error Handling:**  
   Always include checks for invalid input or command failures in your scripts.  

2. **Logging:**  
   Redirect script outputs to log files for future reference.  

3. **Permissions:**  
   Use the least privilege principle when executing scripts. Avoid running as `root` unless necessary.  

4. **Documentation:**  
   Use comments to explain each part of your script.  

5. **Testing:**  
   Test your scripts in a controlled environment before deploying them in production.  

---

### Conclusion  

In this lab, you have learned the fundamentals of scripting, including conditional statements and loops. These skills are essential for automating repetitive tasks, monitoring system resources, and simplifying system administration in a DevOps environment.  

With these scripting basics, you can start creating more complex and efficient automation workflows tailored to your needs.  
