
# ⚠️ SMB Enumeration and Exploitation Guide

This guide covers methods for enumerating and exploiting SMB (Server Message Block) shares using various tools like `nmap`, `smbmap`, and `smbclient`. SMB is a protocol used for sharing files, printers, and other network services, and misconfigurations in SMB services can often lead to security vulnerabilities.

## 🔨 Tools Required
1. **nmap** - A network scanning tool that can discover SMB services and enumerate shares.
2. **smbmap** - A Python tool to enumerate SMB shares and check permissions.
3. **smbclient** - A command-line client for SMB that can be used for interacting with shares.

## ✏️ QUERY Search Engine 
1. **FOFA** => protocol="smb" && banner="Wordpress"
2. **SHODAN** => port:445 has_smb:true
3. **CENSYS** => services.smb.port:445
4. **ZOOM EYE** => port:445

## ⌛ Prerequisites
- Ensure you have permission to scan and access the target system before using these tools.
- Install the required tools:
  - `nmap`: Install with `sudo apt install nmap`.
  - `smbmap`: Install with `pip install smbmap`.
  - `smbclient`: Install with `sudo apt install smbclient`.

## ✍🏻 Steps to Enumerate SMB Shares

### 1. Using `nmap` to Enumerate SMB Shares
You can use `nmap` to scan for open SMB ports (usually port 445) and enumerate SMB shares. Here's the basic command to run a scan on a target:

```bash
nmap -p 445 --script smb-enum-shares.nse <target_ip>
```

Explanation of the flags:
- `-p 445`: Scans port 445, the default SMB port.
- `--script smb-enum-shares.nse`: Uses the `smb-enum-shares.nse` script to enumerate shares.

### Example:
```bash
nmap -p 445 --script smb-enum-shares.nse 192.168.1.100
```
### Mass scanning & Detect Vulnrable :
```bash
nmap -p 445 --script smb-vuln* -iL targets.txt -T4 --max-retries 3
```

This will provide you with a list of SMB shares on the target system.

### 2. Using `smbmap` to Enumerate SMB Shares
`smbmap` is a more specialized tool for interacting with SMB shares and checking permissions. To scan for shares on a target, use the following command:

```bash
smbmap -H <target_ip> -p 445
```

This will attempt to list the available SMB shares. You can add additional options to test specific shares or check for write permissions.

### Example:
```bash
smbmap -H 192.168.1.100 -p 445
```
### Mass scanning Open ports :
```bash
cat smb-targets.txt | xargs -I {} smbmap -H {} -p 445
```

### 3. Using `smbclient` to Access SMB Shares
`smbclient` is a command-line tool that allows you to interact with SMB shares directly. To connect to a share, use the following command:

```bash
smbclient //target_ip/share_name -U username {{Example = guest,root,admin,user,Administrator@<domain_name>}}
```

If you want to access the share without providing a username (e.g., for anonymous access), you can omit the `-U` flag:

```bash
smbclient //192.168.1.100/shared -U guest
```

Once connected, you can run commands like `ls` to list files and `get <file>` to download files.

### Example:
```bash
smbclient //192.168.1.100/public -U {{Example = guest,root,admin,user,Administrator@<domain_name>}}
```

## ✏️ Common SMB Misconfigurations

1. **Anonymous Access**: If a share allows guest access without authentication, this is a potential security risk. You can test this using `smbclient` or check with `nmap` and `smbmap`.

2. **Read/Write Permissions**: Shares that allow write access to anonymous users or non-administrative users can be exploited to upload malicious files or scripts.

3. **Unnecessary Shares**: Some systems may expose unnecessary or sensitive shares. Always check for shares like `ADMIN$`, `C$`, `IPC$`, etc.

## 📝 Example of Exploiting Write Access
If you discover a share with write permissions, you can upload a malicious file or script using `smbclient`. Here’s an example of uploading a file:

```bash
smbclient //192.168.1.100/public -U guest
put /path/to/local/file.txt
```

This will upload `file.txt` to the `public` share on the target.

## ⚠️ Conclusion
These tools and techniques are useful for discovering SMB shares and identifying potential vulnerabilities due to misconfigurations. Be cautious when testing these methods and ensure that you have authorization to access the systems you are scanning.

## 🔔 Additional Resources
- [nmap SMB Enumeration Script](https://nmap.org/nsedoc/scripts/smb-enum-shares.html)
- [smbmap GitHub Repository](https://github.com/ShawnDEvans/smbmap)
- [smbclient Manual](https://linux.die.net/man/1/smbclient)

## 💰 Support Me  

If you find this work helpful, you can support me:  
- [![Buy Me a Coffee](https://www.buymeacoffee.com/assets/img/custom_images/yellow_img.png)](https://buymeacoffee.com/ghost_sec)  

Thanks for your support! ❤️
