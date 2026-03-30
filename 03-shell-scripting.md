# Shell Scripting Basics

## What is a Shell Script?

A shell script is a plain text file containing a sequence of shell commands. Scripts let you automate repetitive tasks, combine commands, and build simple programs.

## Your First Script

```bash
#!/bin/bash
# This is a comment
echo "Hello, World!"
```

### Making it executable and running it
```bash
chmod +x hello.sh   # make executable
./hello.sh          # run it
```

The first line `#!/bin/bash` is called a **shebang** — it tells the OS which interpreter to use.

---

## Variables

```bash
#!/bin/bash

# Assigning variables (no spaces around =)
name="Alice"
age=30
pi=3.14

# Using variables
echo "Name: $name"
echo "Age: $age"
echo "Pi: ${pi}"   # braces for clarity

# Command output into variable
current_date=$(date)
files=$(ls | wc -l)
echo "Date: $current_date"
echo "Files in dir: $files"
```

### Special Variables
| Variable | Meaning |
|----------|---------|
| `$0` | Script name |
| `$1`, `$2`, ... | Positional arguments |
| `$#` | Number of arguments |
| `$@` | All arguments |
| `$?` | Exit status of last command |
| `$$` | PID of current script |
| `$USER` | Current username |
| `$HOME` | Home directory |
| `$PATH` | Command search path |

```bash
#!/bin/bash
echo "Script: $0"
echo "First arg: $1"
echo "All args: $@"
echo "Arg count: $#"
```

---

## User Input

```bash
#!/bin/bash
echo -n "Enter your name: "
read name
echo "Hello, $name!"

# Read with prompt
read -p "Enter age: " age
echo "You are $age years old"

# Read silently (for passwords)
read -sp "Password: " password
echo ""  # newline after silent read
```

---

## Conditionals

```bash
#!/bin/bash

# if / elif / else
if [ $age -ge 18 ]; then
    echo "Adult"
elif [ $age -ge 13 ]; then
    echo "Teenager"
else
    echo "Child"
fi
```

### Test Operators

**Numeric Comparisons:**
| Operator | Meaning |
|----------|---------|
| `-eq` | Equal |
| `-ne` | Not equal |
| `-lt` | Less than |
| `-le` | Less than or equal |
| `-gt` | Greater than |
| `-ge` | Greater than or equal |

**String Comparisons:**
| Operator | Meaning |
|----------|---------|
| `=` or `==` | Equal |
| `!=` | Not equal |
| `-z` | Empty string |
| `-n` | Non-empty string |

**File Tests:**
| Operator | Meaning |
|----------|---------|
| `-f file` | Is a regular file |
| `-d dir` | Is a directory |
| `-e path` | Exists |
| `-r file` | Readable |
| `-w file` | Writable |
| `-x file` | Executable |
| `-s file` | Non-empty file |

```bash
#!/bin/bash

# String comparison
if [ "$name" = "Alice" ]; then
    echo "Hello Alice!"
fi

# File check
if [ -f "/etc/passwd" ]; then
    echo "File exists"
fi

# Directory check
if [ -d "/tmp" ]; then
    echo "/tmp is a directory"
fi

# Logical AND (&&) and OR (||)
if [ $age -gt 18 ] && [ "$name" != "" ]; then
    echo "Adult with a name"
fi
```

---

## Loops

### for Loop
```bash
#!/bin/bash

# Loop over a list
for fruit in apple banana cherry; do
    echo "Fruit: $fruit"
done

# Loop over a range
for i in {1..5}; do
    echo "Number: $i"
done

# C-style for loop
for ((i=0; i<5; i++)); do
    echo "i = $i"
done

# Loop over files
for file in *.txt; do
    echo "Processing: $file"
done

# Loop over command output
for user in $(cut -d: -f1 /etc/passwd); do
    echo "User: $user"
done
```

### while Loop
```bash
#!/bin/bash

count=1
while [ $count -le 5 ]; do
    echo "Count: $count"
    ((count++))
done

# Read file line by line
while IFS= read -r line; do
    echo "Line: $line"
done < file.txt
```

### until Loop
```bash
#!/bin/bash

# until runs while condition is FALSE
n=0
until [ $n -ge 5 ]; do
    echo "n = $n"
    ((n++))
done
```

### Loop Control
```bash
break      # exit the loop
continue   # skip to next iteration
```

---

## Functions

```bash
#!/bin/bash

# Define a function
greet() {
    echo "Hello, $1!"
}

# Call the function
greet "Alice"
greet "Bob"

# Function with return value (exit code)
is_even() {
    if (( $1 % 2 == 0 )); then
        return 0  # 0 = success/true
    else
        return 1  # non-zero = failure/false
    fi
}

if is_even 4; then
    echo "4 is even"
fi

# Capture function output
get_date() {
    echo $(date +%Y-%m-%d)
}

today=$(get_date)
echo "Today is: $today"
```

---

## Exit Codes

Every command returns an exit code:
- `0` = success
- Non-zero = failure/error

```bash
#!/bin/bash

ls /nonexistent
echo "Exit code: $?"   # prints non-zero

ls /tmp
echo "Exit code: $?"   # prints 0

# Exit your script with a code
exit 0   # success
exit 1   # general error
```

---

## String Operations

```bash
#!/bin/bash

str="Hello, World!"

echo ${#str}           # length: 13
echo ${str^^}          # uppercase: HELLO, WORLD!
echo ${str,,}          # lowercase: hello, world!
echo ${str:7:5}        # substring: World
echo ${str/World/Linux} # replace: Hello, Linux!
echo ${str//l/L}       # replace all: HeLLo, WorLd!

# Remove prefix/suffix
filename="report.txt"
echo ${filename%.txt}  # remove .txt suffix: report
echo ${filename#re}    # remove 're' prefix: port.txt
```

---

## Arrays

```bash
#!/bin/bash

# Define array
fruits=("apple" "banana" "cherry")

echo ${fruits[0]}      # apple
echo ${fruits[1]}      # banana
echo ${fruits[@]}      # all elements
echo ${#fruits[@]}     # length: 3

# Add element
fruits+=("date")

# Loop over array
for fruit in "${fruits[@]}"; do
    echo "$fruit"
done

# Associative array (key-value)
declare -A colors
colors["red"]="#FF0000"
colors["green"]="#00FF00"
echo ${colors["red"]}
```

---

## Example: Practical Script

```bash
#!/bin/bash
# backup.sh - Simple backup script

SOURCE="$1"
DEST="$2"
DATE=$(date +%Y%m%d_%H%M%S)
BACKUP_NAME="backup_${DATE}.tar.gz"

# Validate arguments
if [ $# -ne 2 ]; then
    echo "Usage: $0 <source_dir> <dest_dir>"
    exit 1
fi

if [ ! -d "$SOURCE" ]; then
    echo "Error: Source directory '$SOURCE' not found"
    exit 1
fi

if [ ! -d "$DEST" ]; then
    mkdir -p "$DEST"
fi

# Create backup
tar -czf "${DEST}/${BACKUP_NAME}" "$SOURCE"

if [ $? -eq 0 ]; then
    echo "Backup created: ${DEST}/${BACKUP_NAME}"
else
    echo "Backup failed!"
    exit 1
fi
```

---

## Best Practices

1. **Always use `#!/bin/bash`** (or appropriate shebang) at the top
2. **Quote your variables**: use `"$var"` not `$var` to handle spaces
3. **Check exit codes** for critical commands
4. **Use `set -e`** to exit on any error: `set -e` at the top
5. **Use `set -u`** to error on undefined variables: `set -u`
6. **Add comments** to explain why, not what
7. **Test with small inputs** before running on production data

```bash
#!/bin/bash
set -euo pipefail   # exit on error, undefined var, pipe failure
```

---
Previous: [Common Commands](02-common-commands.md)
