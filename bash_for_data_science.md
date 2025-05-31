# Bash for Data Science: Cross-Platform Command Line Mastery

## Table of Contents
1. [Understanding the Command Line](#understanding-command-line)
2. [Essential Commands for Data Scientists](#essential-commands)
3. [File and Data Manipulation](#file-manipulation)
4. [Text Processing Power Tools](#text-processing)
5. [Automation and Scripting](#automation)
6. [Working with Remote Systems](#remote-systems)
7. [Data Science Workflows](#data-science-workflows)
8. [Cross-Platform Considerations](#cross-platform)

---

## Understanding the Command Line

The command line is like having a conversation with your computer using text commands instead of clicking buttons. Imagine you're in a library: using a GUI (Graphical User Interface) is like asking the librarian to fetch books for you, while using the command line is like knowing exactly where to find books yourself and being able to reorganize entire sections efficiently.

### Why Command Line Skills Matter for Data Science

As a data scientist, command line proficiency enables you to:
- Process large files that would crash GUI applications
- Automate repetitive tasks that would take hours of clicking
- Work on remote servers where GUIs aren't available
- Chain together powerful tools to create data pipelines
- Quick explore and analyze data before writing full programs

### Command Line Environments

**Bash (Bourne Again Shell)**
- Default on Linux and macOS
- Available on Windows through WSL (Windows Subsystem for Linux)
- Most common in data science due to Linux server prevalence

**Windows PowerShell**
- Native Windows command line
- Object-oriented approach (different from text-based Bash)
- Powerful for Windows-specific tasks

**Windows Command Prompt (CMD)**
- Legacy Windows command line
- Limited compared to PowerShell but still useful
- Works on all Windows systems

### Setting Up Your Environment

**For Windows Users:**

1. **Install WSL2 (Recommended for Bash compatibility)**
```powershell
# In PowerShell as Administrator
wsl --install

# After restart, set up Ubuntu
wsl --set-default-version 2
```

2. **Install Windows Terminal (Better experience)**
- Download from Microsoft Store
- Provides tabs, better colors, and easy switching between shells

3. **Choose Your Approach:**
- WSL for Linux/Bash compatibility
- PowerShell for Windows-native operations
- Both for maximum flexibility

---

## Essential Commands for Data Scientists

Let's learn commands in three columns: Bash, PowerShell, and CMD. Think of these as different dialects of the same language - they express similar ideas with different syntax.

### Navigation and File System

**Current Directory**
```bash
# Bash
pwd                    # Print Working Directory

# PowerShell
Get-Location          # or shorthand: pwd, gl

# CMD
cd                    # Shows current directory
```

**List Files**
```bash
# Bash
ls                    # Basic listing
ls -la                # Detailed listing with hidden files
ls -lh                # Human-readable file sizes

# PowerShell
Get-ChildItem         # or shorthand: ls, dir, gci
ls -Force             # Include hidden files
ls | Format-Table Length, Name  # Custom format

# CMD
dir                   # Basic listing
dir /a                # All files including hidden
dir /s                # Recursive listing
```

**Change Directory**
```bash
# Bash
cd /home/user/data    # Absolute path
cd ../                # Parent directory
cd ~                  # Home directory
cd -                  # Previous directory

# PowerShell
Set-Location C:\Users\data  # or shorthand: cd
cd ..                 # Parent directory
cd ~                  # User profile directory
cd -                  # Previous location (in PS 5+)

# CMD
cd C:\Users\data      # Change directory
cd ..                 # Parent directory
cd /d D:\             # Change drive and directory
```

**Create and Remove Directories**
```bash
# Bash
mkdir data_project    # Create directory
mkdir -p data/raw/2023  # Create nested directories
rmdir empty_folder    # Remove empty directory
rm -rf old_project    # Remove directory and contents

# PowerShell
New-Item -ItemType Directory data_project  # or mkdir
mkdir data\raw\2023 -Force  # Create nested
Remove-Item empty_folder    # or rmdir
Remove-Item old_project -Recurse -Force

# CMD
mkdir data_project    # or md
mkdir data\raw\2023   # Creates nested automatically
rmdir empty_folder    # Remove empty
rmdir /s /q old_project  # Remove with contents
```

### Working with Files

**Copy Files**
```bash
# Bash
cp file.txt backup.txt     # Copy file
cp -r folder/ backup/      # Copy directory recursively
cp *.csv data/            # Copy all CSV files

# PowerShell
Copy-Item file.txt backup.txt  # or cp
Copy-Item folder backup -Recurse
Copy-Item *.csv data\

# CMD
copy file.txt backup.txt   # Copy file
xcopy folder backup\ /E    # Copy directory
copy *.csv data\          # Copy multiple files
```

**Move and Rename**
```bash
# Bash
mv oldname.txt newname.txt  # Rename
mv file.txt ../data/        # Move to different directory
mv *.log archive/           # Move multiple files

# PowerShell
Move-Item oldname.txt newname.txt  # or mv
Move-Item file.txt ..\data\
Move-Item *.log archive\

# CMD
rename oldname.txt newname.txt  # or ren
move file.txt ..\data\
move *.log archive\
```

**Delete Files**
```bash
# Bash
rm file.txt               # Delete file
rm -i *.tmp              # Interactive deletion
rm -f locked_file.txt    # Force deletion

# PowerShell
Remove-Item file.txt      # or rm, del
Remove-Item *.tmp -Confirm
Remove-Item locked.txt -Force

# CMD
del file.txt             # or erase
del *.tmp /p             # Prompt for each file
del locked.txt /f        # Force deletion
```

### File Permissions and Attributes

**View Permissions**
```bash
# Bash
ls -l file.txt           # View permissions
stat file.txt            # Detailed file information

# PowerShell
Get-Acl file.txt         # Access Control List
Get-ItemProperty file.txt | Select-Object *

# CMD
attrib file.txt          # View attributes
icacls file.txt          # View permissions
```

**Change Permissions**
```bash
# Bash
chmod 755 script.sh      # Owner: rwx, Others: r-x
chmod +x script.sh       # Make executable
chmod -R 644 data/       # Recursive permission change

# PowerShell
# More complex due to Windows ACL system
$acl = Get-Acl file.txt
# Modify $acl
Set-Acl file.txt $acl

# CMD
attrib +r file.txt       # Set read-only
attrib -h file.txt       # Remove hidden attribute
```

---

## File and Data Manipulation

As a data scientist, you'll often need to quickly examine, filter, and transform files. These commands are your Swiss Army knife for data exploration.

### Viewing File Contents

**Display File Content**
```bash
# Bash
cat file.txt             # Display entire file
head -n 20 file.csv      # First 20 lines
tail -n 50 log.txt       # Last 50 lines
tail -f running.log      # Follow file updates in real-time

# PowerShell
Get-Content file.txt      # or gc, cat, type
Get-Content file.csv -Head 20  # First 20 lines
Get-Content log.txt -Tail 50   # Last 50 lines
Get-Content running.log -Wait  # Follow updates

# CMD
type file.txt            # Display entire file
more file.txt            # Page through file
# No direct equivalent for head/tail
```

**Search Within Files**
```bash
# Bash
grep "error" logfile.txt           # Find lines containing "error"
grep -i "error" *.log              # Case-insensitive search
grep -r "TODO" ./src/              # Recursive search
grep -E "^[0-9]+," data.csv        # Regex: lines starting with numbers

# PowerShell
Select-String "error" logfile.txt   # or sls
Select-String "error" *.log -CaseSensitive:$false
Get-ChildItem -Recurse | Select-String "TODO"
Select-String "^[0-9]+," data.csv

# CMD
findstr "error" logfile.txt         # Find string
findstr /i "error" *.log            # Case-insensitive
findstr /s "TODO" *.py              # Search subdirectories
```

### Data File Analysis

**Count Lines, Words, Characters**
```bash
# Bash
wc -l data.csv           # Count lines
wc -w document.txt       # Count words
wc -c file.bin          # Count bytes
du -h data/             # Directory size (human-readable)

# PowerShell
(Get-Content data.csv).Count  # Count lines
(Get-Content document.txt -Raw).Split().Count  # Words
(Get-Item file.bin).Length    # File size in bytes
Get-ChildItem data -Recurse | Measure-Object -Property Length -Sum

# CMD
find /c /v "" data.csv   # Count lines
# No direct word count
dir file.bin            # Shows file size
```

**Sort and Unique Operations**
```bash
# Bash
sort names.txt           # Sort alphabetically
sort -n numbers.txt      # Sort numerically
sort -k2 data.tsv       # Sort by second column
sort names.txt | uniq    # Remove duplicates
uniq -c sorted.txt      # Count occurrences

# PowerShell
Get-Content names.txt | Sort-Object
Get-Content numbers.txt | Sort-Object {[int]$_}
Import-Csv data.csv | Sort-Object Column2
Get-Content names.txt | Sort-Object -Unique
Get-Content sorted.txt | Group-Object | Select Count, Name

# CMD
sort names.txt          # Basic sort
# Limited sorting options in CMD
```

### File Comparison and Merging

**Compare Files**
```bash
# Bash
diff file1.txt file2.txt      # Show differences
diff -u old.py new.py         # Unified format (like Git)
comm sorted1.txt sorted2.txt  # Compare sorted files

# PowerShell
Compare-Object (Get-Content file1.txt) (Get-Content file2.txt)
# Shows which lines are different and in which file

# CMD
fc file1.txt file2.txt        # File compare
fc /b binary1 binary2         # Binary comparison
```

**Combine Files**
```bash
# Bash
cat file1.txt file2.txt > combined.txt  # Concatenate
paste -d',' col1.txt col2.txt > data.csv  # Merge columns

# PowerShell
Get-Content file1.txt, file2.txt | Set-Content combined.txt
# More complex column merging requires scripting

# CMD
copy file1.txt + file2.txt combined.txt  # Concatenate
type file1.txt file2.txt > combined.txt  # Alternative
```

---

## Text Processing Power Tools

Text processing is where command line truly shines for data science. These tools can process gigabytes of data in seconds.

### AWK - Pattern Scanning and Processing

AWK is like having a mini programming language designed specifically for text processing. It's particularly powerful for structured data like CSVs.

```bash
# Bash/WSL (AWK examples)

# Print specific columns (like selecting columns in pandas)
awk '{print $1, $3}' data.txt              # Print 1st and 3rd columns
awk -F',' '{print $2}' data.csv           # CSV: print 2nd column

# Filter rows (like pandas query)
awk '$3 > 100' data.txt                   # Rows where 3rd column > 100
awk -F',' '$2 == "Active"' users.csv      # CSV: status is "Active"

# Calculate sum (like pandas sum)
awk '{sum += $3} END {print sum}' numbers.txt
awk -F',' '{sum += $4} END {print "Total:", sum}' sales.csv

# Data transformation
awk '{print $1, $2 * 1.1}' prices.txt     # Add 10% to prices
awk 'NR > 1' data.csv                     # Skip header row

# PowerShell equivalents
# Print specific columns
Import-Csv data.csv | Select-Object Column1, Column3

# Filter rows
Import-Csv data.csv | Where-Object {$_.Column3 -gt 100}

# Calculate sum
Import-Csv sales.csv | Measure-Object -Property Sales -Sum

# Skip header (already handled by Import-Csv)
```

### SED - Stream Editor

SED is like find-and-replace on steroids. It can transform text as it flows through, making it perfect for cleaning data.

```bash
# Bash/WSL (SED examples)

# Basic substitution
sed 's/old/new/' file.txt                 # Replace first occurrence
sed 's/old/new/g' file.txt               # Replace all occurrences

# Clean data
sed 's/^[ \t]*//;s/[ \t]*$//' file.txt   # Remove leading/trailing whitespace
sed '/^$/d' file.txt                      # Delete empty lines
sed '1d' data.csv                         # Delete first line (header)

# Multiple operations
sed -e 's/USA/United States/g' -e 's/UK/United Kingdom/g' countries.txt

# In-place editing
sed -i 's/\r$//' file.txt                 # Remove Windows line endings
sed -i.bak 's/old/new/g' file.txt        # Edit with backup

# PowerShell equivalents
# Basic substitution
(Get-Content file.txt) -replace 'old', 'new'

# Clean data
(Get-Content file.txt) | ForEach-Object { $_.Trim() }
(Get-Content file.txt) | Where-Object { $_ -ne "" }
Get-Content data.csv | Select-Object -Skip 1

# Multiple operations
(Get-Content countries.txt) -replace 'USA', 'United States' -replace 'UK', 'United Kingdom'

# In-place editing
(Get-Content file.txt) -replace "`r`n", "`n" | Set-Content file.txt
```

### Advanced Text Processing Pipelines

The real power comes from combining these tools:

```bash
# Bash - Complex data processing pipeline
# Extract email addresses from log files, count by domain
grep -E -o "[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}" logs/*.log |
  awk -F'@' '{print $2}' |
  sort |
  uniq -c |
  sort -nr |
  head -10

# PowerShell equivalent
Get-ChildItem logs\*.log | 
  Select-String -Pattern "[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}" -AllMatches |
  ForEach-Object { $_.Matches.Value } |
  ForEach-Object { $_.Split('@')[1] } |
  Group-Object |
  Sort-Object Count -Descending |
  Select-Object -First 10 Count, Name
```

### JSON and CSV Processing

**Working with JSON**
```bash
# Bash (using jq - install with: sudo apt-get install jq)
jq '.' data.json                          # Pretty print
jq '.users[] | .name' data.json          # Extract names
jq '.[] | select(.age > 30)' users.json  # Filter by age

# PowerShell (native JSON support)
Get-Content data.json | ConvertFrom-Json  # Parse JSON
(Get-Content data.json | ConvertFrom-Json).users | Select-Object name
Get-Content users.json | ConvertFrom-Json | Where-Object { $_.age -gt 30 }

# CMD (limited JSON support, use PowerShell instead)
```

**CSV Processing**
```bash
# Bash
cut -d',' -f2,4 data.csv                 # Extract columns 2 and 4
awk -F',' 'NR==1 {print} $3 > 100' data.csv  # Header + filtered rows

# PowerShell
Import-Csv data.csv | Select-Object Column2, Column4
Import-Csv data.csv | Where-Object { $_.Column3 -gt 100 }

# Export back to CSV
Import-Csv input.csv | 
  Where-Object { $_.Status -eq 'Active' } |
  Export-Csv output.csv -NoTypeInformation
```

---

## Automation and Scripting

Automation transforms you from someone who processes data to someone who builds systems that process data automatically. Let's explore how to create scripts that save hours of manual work.

### Basic Script Structure

**Bash Script**
```bash
#!/bin/bash
# data_processor.sh - Process daily data files

# Variables
DATA_DIR="/home/user/data"
OUTPUT_DIR="/home/user/processed"
LOG_FILE="/home/user/logs/process_$(date +%Y%m%d).log"

# Function definition
process_file() {
    local file=$1
    echo "Processing $file..." | tee -a "$LOG_FILE"
    
    # Your processing logic here
    awk -F',' 'NR>1 {sum+=$3} END {print "Total:", sum}' "$file"
}

# Main logic
echo "Starting data processing at $(date)" | tee "$LOG_FILE"

# Process all CSV files
for file in "$DATA_DIR"/*.csv; do
    if [ -f "$file" ]; then
        process_file "$file"
    fi
done

echo "Processing complete at $(date)" | tee -a "$LOG_FILE"
```

**PowerShell Script**
```powershell
# data_processor.ps1 - Process daily data files

# Variables
$DataDir = "C:\Users\data"
$OutputDir = "C:\Users\processed"
$LogFile = "C:\Users\logs\process_$(Get-Date -Format 'yyyyMMdd').log"

# Function definition
function Process-DataFile {
    param($FilePath)
    
    "Processing $FilePath..." | Tee-Object -FilePath $LogFile -Append
    
    # Your processing logic here
    $data = Import-Csv $FilePath
    $total = ($data | Measure-Object -Property Amount -Sum).Sum
    "Total: $total"
}

# Main logic
"Starting data processing at $(Get-Date)" | Out-File $LogFile

# Process all CSV files
Get-ChildItem "$DataDir\*.csv" | ForEach-Object {
    Process-DataFile $_.FullName
}

"Processing complete at $(Get-Date)" | Tee-Object -FilePath $LogFile -Append
```

### Command Line Arguments

**Bash Arguments**
```bash
#!/bin/bash
# script.sh - Script with arguments

# Check argument count
if [ $# -lt 2 ]; then
    echo "Usage: $0 <input_file> <output_file>"
    exit 1
fi

# Assign arguments to variables
INPUT_FILE=$1
OUTPUT_FILE=$2
DELIMITER=${3:-","}  # Default to comma if not provided

echo "Processing $INPUT_FILE with delimiter '$DELIMITER'"
# Process file here
```

**PowerShell Parameters**
```powershell
# script.ps1 - Script with parameters
param(
    [Parameter(Mandatory=$true)]
    [string]$InputFile,
    
    [Parameter(Mandatory=$true)]
    [string]$OutputFile,
    
    [string]$Delimiter = ","
)

Write-Host "Processing $InputFile with delimiter '$Delimiter'"
# Process file here
```

### Error Handling

**Bash Error Handling**
```bash
#!/bin/bash
set -e  # Exit on error
set -u  # Exit on undefined variable
set -o pipefail  # Exit on pipe failure

# Function with error checking
safe_process() {
    local file=$1
    
    # Check if file exists
    if [ ! -f "$file" ]; then
        echo "Error: File $file not found" >&2
        return 1
    fi
    
    # Try processing with error handling
    if ! result=$(process_command "$file" 2>&1); then
        echo "Error processing $file: $result" >&2
        return 1
    fi
    
    echo "Successfully processed $file"
}

# Trap errors
trap 'echo "Error on line $LINENO"' ERR
```

**PowerShell Error Handling**
```powershell
# Set strict mode
Set-StrictMode -Version Latest
$ErrorActionPreference = "Stop"

function Safe-Process {
    param($FilePath)
    
    # Check if file exists
    if (-not (Test-Path $FilePath)) {
        Write-Error "File $FilePath not found"
        return
    }
    
    try {
        # Process file
        $result = Process-Command $FilePath
        Write-Host "Successfully processed $FilePath"
    }
    catch {
        Write-Error "Error processing ${FilePath}: $_"
    }
}
```

### Scheduling and Automation

**Cron (Linux/WSL)**
```bash
# Edit crontab
crontab -e

# Add scheduled tasks
# Format: minute hour day month day_of_week command

# Daily at 2 AM
0 2 * * * /home/user/scripts/daily_process.sh

# Every Monday at 9 AM
0 9 * * 1 /home/user/scripts/weekly_report.sh

# Every 30 minutes
*/30 * * * * /home/user/scripts/check_data.sh

# First day of month at midnight
0 0 1 * * /home/user/scripts/monthly_cleanup.sh
```

**Windows Task Scheduler (PowerShell)**
```powershell
# Create scheduled task
$action = New-ScheduledTaskAction -Execute "PowerShell.exe" `
    -Argument "-File C:\Scripts\daily_process.ps1"

$trigger = New-ScheduledTaskTrigger -Daily -At 2am

Register-ScheduledTask -TaskName "DailyDataProcess" `
    -Action $action -Trigger $trigger `
    -Description "Process daily data files"

# For CMD, use schtasks
# schtasks /create /tn "DailyProcess" /tr "C:\Scripts\process.bat" /sc daily /st 02:00
```

---

## Working with Remote Systems

Data scientists often work with remote servers for heavy computations or accessing large datasets. Here's how to work efficiently with remote systems.

### SSH Connections

**Basic SSH (Bash/WSL)**
```bash
# Connect to remote server
ssh username@server.example.com

# SSH with specific port
ssh -p 2222 username@server.example.com

# SSH with key authentication
ssh -i ~/.ssh/mykey.pem username@server.example.com

# Run command remotely
ssh username@server.example.com "df -h"

# SSH config file (~/.ssh/config)
# Host myserver
#     HostName server.example.com
#     User username
#     Port 22
#     IdentityFile ~/.ssh/mykey.pem

# Then simply: ssh myserver
```

**PowerShell SSH**
```powershell
# Windows 10+ includes OpenSSH
ssh username@server.example.com

# Or use PowerShell remoting for Windows servers
Enter-PSSession -ComputerName server.example.com -Credential (Get-Credential)
```

### File Transfer

**SCP (Secure Copy)**
```bash
# Copy file to remote
scp local_file.txt username@server:/remote/path/

# Copy from remote
scp username@server:/remote/file.txt ./local_path/

# Copy directory
scp -r local_dir username@server:/remote/path/

# With SSH key
scp -i ~/.ssh/mykey.pem file.txt username@server:/path/
```

**SFTP (Interactive)**
```bash
sftp username@server.example.com
# Interactive commands:
# put local_file.txt          # Upload
# get remote_file.txt         # Download
# ls                          # List remote files
# lls                         # List local files
# cd /remote/path             # Change remote directory
# lcd /local/path             # Change local directory
```

**PowerShell File Transfer**
```powershell
# Using Windows native tools
# For SSH-enabled servers, use scp as above

# For Windows shares
Copy-Item local_file.txt \\server\share\path\

# For Azure/Cloud
# Use cloud-specific cmdlets like az storage blob upload
```

### Remote Data Processing

**Running Remote Scripts**
```bash
# Execute local script on remote server
ssh username@server 'bash -s' < local_script.sh

# Send data through SSH for processing
cat large_file.csv | ssh username@server "awk '{sum+=$3} END {print sum}'"

# Remote Jupyter notebook
# On remote server:
jupyter notebook --no-browser --port=8889

# On local machine:
ssh -N -f -L localhost:8888:localhost:8889 username@server
# Then open http://localhost:8888 in browser
```

### Multiplexing with tmux/screen

**tmux Basics (Persistent Sessions)**
```bash
# Start new session
tmux new -s datasession

# Detach from session
# Press Ctrl+b, then d

# List sessions
tmux ls

# Reattach to session
tmux attach -t datasession

# Create new window in session
# Press Ctrl+b, then c

# Switch between windows
# Press Ctrl+b, then window number

# Split panes
# Ctrl+b, then % (vertical split)
# Ctrl+b, then " (horizontal split)
```

---

## Data Science Workflows

Let's combine everything into practical workflows that you'll use daily as a data scientist.

### Data Exploration Workflow

**Quick Data Profiling Script**
```bash
#!/bin/bash
# profile_data.sh - Quick data profiling for CSV files

profile_csv() {
    local file=$1
    echo "=== Profiling $file ==="
    
    # File size
    echo "File size: $(ls -lh "$file" | awk '{print $5}')"
    
    # Line count
    lines=$(wc -l < "$file")
    echo "Total lines: $lines"
    
    # Column count (from header)
    cols=$(head -1 "$file" | awk -F',' '{print NF}')
    echo "Columns: $cols"
    
    # Show header
    echo -e "\nHeader:"
    head -1 "$file"
    
    # Show sample
    echo -e "\nFirst 5 rows:"
    head -6 "$file" | tail -5
    
    # Check for missing values (empty fields)
    echo -e "\nEmpty fields by column:"
    awk -F',' '
    NR==1 {for(i=1;i<=NF;i++) header[i]=$i; next}
    {for(i=1;i<=NF;i++) if($i=="") empty[i]++}
    END {for(i=1;i<=NF;i++) print header[i] ": " empty[i]+0}
    ' "$file"
}

# Process all CSV files in current directory
for file in *.csv; do
    [ -f "$file" ] && profile_csv "$file"
done
```

**PowerShell Data Profiling**
```powershell
function Profile-CsvData {
    param($Path)
    
    Write-Host "=== Profiling $Path ===" -ForegroundColor Cyan
    
    # File info
    $fileInfo = Get-Item $Path
    Write-Host "File size: $($fileInfo.Length / 1MB) MB"
    
    # Load data
    $data = Import-Csv $Path
    Write-Host "Total rows: $($data.Count)"
    Write-Host "Columns: $($data[0].PSObject.Properties.Count)"
    
    # Show columns
    Write-Host "`nColumns:"
    $data[0].PSObject.Properties.Name | ForEach-Object { Write-Host "  - $_" }
    
    # Sample data
    Write-Host "`nFirst 5 rows:"
    $data | Select-Object -First 5 | Format-Table
    
    # Check for empty values
    Write-Host "`nEmpty values by column:"
    $columns = $data[0].PSObject.Properties.Name
    foreach ($col in $columns) {
        $empty = ($data | Where-Object { $_.$col -eq "" }).Count
        Write-Host "  $col : $empty"
    }
}

# Profile all CSV files
Get-ChildItem *.csv | ForEach-Object {
    Profile-CsvData $_.FullName
}
```

### Data Cleaning Pipeline

**Bash Data Cleaning**
```bash
#!/bin/bash
# clean_data.sh - Comprehensive data cleaning pipeline

clean_csv() {
    local input=$1
    local output=$2
    
    # Create temporary files
    temp1=$(mktemp)
    temp2=$(mktemp)
    
    # Step 1: Remove BOM and convert line endings
    sed '1s/^\xEF\xBB\xBF//' "$input" | dos2unix > "$temp1"
    
    # Step 2: Trim whitespace and remove empty lines
    awk -F',' '
    {
        # Trim each field
        for(i=1; i<=NF; i++) {
            gsub(/^[ \t]+|[ \t]+$/, "", $i)
            printf "%s%s", $i, (i<NF ? "," : "\n")
        }
    }' "$temp1" | grep -v '^[[:space:]]*$' > "$temp2"
    
    # Step 3: Standardize date formats (MM/DD/YYYY to YYYY-MM-DD)
    awk -F',' '
    {
        for(i=1; i<=NF; i++) {
            if($i ~ /^[0-9]{1,2}\/[0-9]{1,2}\/[0-9]{4}$/) {
                split($i, date, "/")
                $i = sprintf("%04d-%02d-%02d", date[3], date[1], date[2])
            }
        }
        print
    }' OFS=',' "$temp2" > "$output"
    
    # Cleanup
    rm "$temp1" "$temp2"
    
    echo "Cleaned $input -> $output"
}

# Process all files
mkdir -p cleaned_data
for file in raw_data/*.csv; do
    basename=$(basename "$file")
    clean_csv "$file" "cleaned_data/cleaned_$basename"
done
```

### Log Analysis Workflow

**Analyzing Application Logs**
```bash
#!/bin/bash
# analyze_logs.sh - Extract insights from application logs

# Find most common errors
echo "Top 10 Errors:"
grep -i "error" app.log | 
    sed 's/.*ERROR[[:space:]]*\([^:]*\).*/\1/' |
    sort | uniq -c | sort -nr | head -10

# Response time analysis
echo -e "\nResponse Time Statistics:"
grep "Response time:" app.log |
    awk '{print $NF}' |
    awk '{
        sum+=$1; count++
        if(NR==1 || $1<min) min=$1
        if(NR==1 || $1>max) max=$1
    }
    END {
        print "Average:", sum/count "ms"
        print "Min:", min "ms"
        print "Max:", max "ms"
    }'

# Requests per hour
echo -e "\nRequests per Hour:"
awk '/GET|POST/ {print $4}' access.log |
    sed 's/\[//; s/:.*$//' |
    uniq -c |
    awk '{print $2, $1}'
```

### Database Operations

**Export and Import Data**
```bash
# PostgreSQL
# Export to CSV
psql -h localhost -U username -d database \
    -c "COPY (SELECT * FROM table WHERE condition) TO STDOUT WITH CSV HEADER" \
    > export.csv

# Import from CSV
psql -h localhost -U username -d database \
    -c "\COPY table FROM 'data.csv' WITH CSV HEADER"

# MySQL
# Export
mysql -h localhost -u username -p database \
    -e "SELECT * FROM table WHERE condition" \
    | sed 's/\t/,/g' > export.csv

# MongoDB export
mongoexport --db database --collection collection \
    --query '{"status": "active"}' \
    --type csv --fields name,email,created \
    --out export.csv
```

---

## Cross-Platform Considerations

Working across different platforms requires understanding their differences and similarities. Here's how to write scripts that work everywhere.

### Path Handling

**Cross-Platform Path Script**
```bash
#!/bin/bash
# Bash - Handle paths carefully

# Detect OS
if [[ "$OSTYPE" == "linux-gnu"* ]]; then
    DATA_PATH="/home/$USER/data"
elif [[ "$OSTYPE" == "darwin"* ]]; then
    DATA_PATH="/Users/$USER/data"
elif [[ "$OSTYPE" == "msys" ]] || [[ "$OSTYPE" == "win32" ]]; then
    DATA_PATH="/c/Users/$USER/data"  # Git Bash style
fi

# Use variables for path separators if needed
SEP="/"  # Unix style
```

**PowerShell Cross-Platform**
```powershell
# PowerShell Core works on all platforms
$DataPath = Join-Path $HOME "data"

# OS-specific logic
if ($IsWindows) {
    # Windows-specific code
} elseif ($IsLinux) {
    # Linux-specific code
} elseif ($IsMacOS) {
    # macOS-specific code
}
```

### Line Endings

**Handle Different Line Endings**
```bash
# Convert between Unix (LF) and Windows (CRLF)
# Unix to Windows
unix2dos file.txt

# Windows to Unix
dos2unix file.txt

# Using sed
sed -i 's/\r$//' file.txt  # Remove CR
sed -i 's/$/\r/' file.txt  # Add CR

# PowerShell
(Get-Content file.txt) | Set-Content -NoNewline file_unix.txt
```

### Environment Variables

**Setting Environment Variables**
```bash
# Bash
export DATA_PATH="/home/user/data"
export PYTHONPATH="${PYTHONPATH}:/home/user/lib"

# Make permanent - add to ~/.bashrc or ~/.bash_profile
echo 'export DATA_PATH="/home/user/data"' >> ~/.bashrc

# PowerShell
$env:DATA_PATH = "C:\Users\data"
$env:Path += ";C:\Users\scripts"

# Make permanent
[Environment]::SetEnvironmentVariable("DATA_PATH", "C:\Users\data", "User")

# CMD
set DATA_PATH=C:\Users\data
# Permanent: setx DATA_PATH "C:\Users\data"
```

### Package Management

**Installing Tools Across Platforms**
```bash
# Ubuntu/Debian (including WSL)
sudo apt update
sudo apt install jq csvkit parallel

# macOS
brew install jq csvkit parallel

# Windows - Using Chocolatey
choco install jq wget grep

# Windows - Using scoop
scoop install jq busybox

# Python packages (cross-platform)
pip install pandas numpy csvkit
```

### Creating Universal Scripts

**Template for Cross-Platform Script**
```bash
#!/usr/bin/env bash
# universal_script.sh - Works on Linux, macOS, WSL

# Detect platform
detect_platform() {
    case "$OSTYPE" in
        linux*)   echo "linux" ;;
        darwin*)  echo "macos" ;;
        msys*)    echo "windows" ;;
        *)        echo "unknown" ;;
    esac
}

# Platform-specific settings
PLATFORM=$(detect_platform)
case $PLATFORM in
    linux|macos)
        PATH_SEP="/"
        NULL_DEVICE="/dev/null"
        ;;
    windows)
        PATH_SEP="\\"
        NULL_DEVICE="NUL"
        ;;
esac

# Use commands available everywhere
command_exists() {
    command -v "$1" >/dev/null 2>&1
}

# Prefer cross-platform tools
if command_exists python3; then
    PYTHON=python3
elif command_exists python; then
    PYTHON=python
else
    echo "Python not found!"
    exit 1
fi

# Your script logic here
echo "Running on $PLATFORM"
$PYTHON --version
```

### Tips for Cross-Platform Success

1. **Use High-Level Languages When Possible**
   - Python, R, and Julia work identically across platforms
   - Only drop to shell scripting when necessary

2. **Test on Multiple Platforms**
   - Use WSL on Windows for Linux compatibility
   - Use Docker for consistent environments
   - Set up CI/CD to test on multiple OS

3. **Document Platform Requirements**
   ```bash
   # At the top of your scripts
   # Requirements:
   # - Bash 4.0+ (macOS users: brew install bash)
   # - GNU coreutils (macOS users: brew install coreutils)
   # - Python 3.7+
   ```

4. **Provide Platform-Specific Instructions**
   ```markdown
   ## Installation
   
   ### Linux/WSL
   ```bash
   sudo apt install required-packages
   ```
   
   ### macOS
   ```bash
   brew install required-packages
   ```
   
   ### Windows
   ```powershell
   choco install required-packages
   ```
   ```

Remember: The command line is incredibly powerful for data science. Start with simple commands, build up to pipelines, and gradually incorporate automation. Each command you learn is a tool in your toolkit - the more tools you have, the more efficiently you can handle any data challenge that comes your way!