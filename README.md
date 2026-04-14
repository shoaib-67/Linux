# 🐧 Linux Commands Reference

A comprehensive cheat sheet covering essential Linux commands for developers — file operations, permissions, user management, networking, and powerful CLI tools.

---

## 📁 Essential Commands

| Command | Description | Example |
|---------|-------------|---------|
| `ls` | List directory contents | `ls -la` |
| `cd` | Change directory | `cd /var/log` |
| `mkdir` | Create directories | `mkdir alien |
| `rm` | Remove files/directories | `rm -rf node_modules` |
| `cp` | Copy files | `cp -r src/ backup/` |
| `mv` | Move or rename files | `mv old.sh new.sh` |
| `cat` | Display file contents | `cat /etc/os-release` |
| `echo` | Print to stdout | `echo "Hello $USER"` |
| `man` | Display manual pages | `man grep` |
| `pwd` | Print working directory | `pwd` |
| `touch` | Create empty file | `touch newfile.txt` |
| `head` | Show first N lines | `head -n 20 file.txt` |
| `tail` | Show last N lines | `tail -f app.log` |
| `less` | Page through file | `less bigfile.log` |
| `wc` | Count lines/words/chars | `wc -l file.txt` |

---

## 🔒 File Permissions, Ownership & Management

### Understanding Permission Notation

```
-rwxr-xr--
 ^^^         → owner  (read, write, execute)
    ^^^      → group  (read, no write, execute)
       ^^^   → others (read, no write, no execute)
```
$ ls -l
-rwxr-xr-- 1 alice developers 4096 Apr 14 10:22 script.sh
^─────────────────────────────────────────────────────────^
```
 
| Segment | Value | Meaning |
|---------|-------|---------|
| `[1]` File type | `-` | `-` file · `d` directory · `l` symlink · `c` char device |
| `[2]` Permissions | `rwxr-xr--` | owner=`rwx`(7) · group=`r-x`(5) · others=`r--`(4) |
| `[3]` Hard links | `1` | Number of hard links pointing to this inode |
| `[4]` Owner | `alice` | User who owns the file (`chown alice file`) |
| `[5]` Group | `developers` | Owning group (`chgrp developers file`) |
| `[6]` Size | `4096` | Size in bytes; use `ls -lh` for KB/MB/GB |
| `[7]` Modified | `Apr 14 10:22` | Last modification time; sort with `ls -lt` |
| `[8]` Filename | `script.sh` | File name; symlinks show as `link → target` |
### Numeric Permission Values

| Value | Permission |
|-------|------------|
| `7`   | rwx (read + write + execute) |
| `6`   | rw- (read + write) |
| `5`   | r-x (read + execute) |
| `4`   | r-- (read only) |
| `0`   | --- (no permission) |

### Commands

```bash
# chmod — change file permissions
chmod 755 script.sh           # rwxr-xr-x (standard for executables)
chmod 644 config.yml          # rw-r--r-- (standard for config files)
chmod +x deploy.sh            # add execute permission
chmod -R 644 docs/            # apply recursively



### Common Permission Patterns

```bash
chmod 755 script.sh           # Executable script
chmod 644 README.md           # Regular file
chmod 600 ~/.ssh/id_rsa       # Private key (owner only)
chmod 700 ~/.ssh/             # SSH directory
chmod 777 /tmp/shared/        # World-writable (use with caution)
```

---
# Password management
passwd alice                  # change alice's password
passwd -e alice               # force password change at next login
passwd -l alice               # lock account

# Groups
groupadd developers
groupadd -g 1050 devops
groupdel ol

# Who is logged in
who -a                        # all logged-in users
w                             # users and what they're doing
last -n 10                    # last 10 logins

---

## 🌐 Networking Commands

```bash
# Interface info
ip addr show                  # show all interfaces and IPs

# Routing
ip route show
ip route get 8.8.8.8          # trace route to destination
n | grep :80        # connections on port 80

# DNS
nslookup example.com
dig example.com A
host example.com

# HTTP requests
curl -I https://example.com                           # headers only
curl -X POST -d '{"key":"val"}' https://api/endpoint  # POST request
curl -o file.zip https://example.com/file.zip         # download file
wget -c https://example.com/file.tar.gz               # resumable download
wget -r -np https://site.com/docs/                    # mirror directory


##l Tools

### `grep` — Search Text

```bash
grep -r "TODO" ./src                  # recursive search
grep -n "error" app.log               # show line numbers
grep -i "warning" app.log             # case-insensitive
grep -v "^#" config.txt               # exclude comment lines
grep -E "^[0-9]+" data.txt            # extended regex
grep -l "pattern" *.txt               # list matching files only

### `find` — Search Files

```bash
find . -name "*.sh" -type f           # find shell scripts
find /var -mtime -1 -size +10M        # modified in last day, >10MB
find . -perm 777                      # find world-writable files
find . -empty                         # find empty files/dirs
find . -user root                     # find files owned by root

# Execute command on results
find . -name "*.log" -exec rm {} \;
find . -perm 777 -exec chmod 755 {} \;
find . -name "*.sh" | xargs chmod +x
```

### `locate` — Fast File Search

```bash
locate nginx.conf
locate -i readme.md               # case-insensitive
```

### `sed` — Stream Editor

```bash
sed -i 's/foo/bar/g' config.txt           # in-place replace all occurrences
sed -i 's/foo/bar/' config.txt            # replace first occurrence per line
sed -n '10,20p' bigfile.log               # print lines 10–20
sed '/^#/d' script.sh                     # delete comment lines
sed -i '/^$/d' file.txt                   # delete blank lines
sed 's/^/  /' file.txt                    # indent every line

# Multiple operations
sed -e 's/foo/bar/g' -e 's/baz/qux/g' file.txt
```

## 📚 Resources
Online
