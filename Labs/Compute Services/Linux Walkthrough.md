# Introduction to Amazon Linux AMI - Documentation Journey

## Executive Summary

This documentation captures the journey of completing an introductory Amazon Linux lab that covered the fundamental command-line interface (CLI) operations and the Linux manual page system. The lab provided essential foundation knowledge for working with Linux-based EC2 instances and reinforced core bash shell functionality within the Vocareum lab environment.

---

## Introduction to Amazon Linux and the CLI

### Overview

The lab was structured to provide a foundational understanding of Linux command-line interface operations within an Amazon Linux Amazon Machine Image (AMI). The focus was placed on:

- **SSH Access:** Secure connection to remotely-hosted EC2 instances
- **Linux Shell Fundamentals:** Understanding bash terminal operations
- **Manual Pages System:** Accessing comprehensive command documentation through the `man` command
- **CLI Navigation:** Basic command-line exploration and help system utilization

#### Key Concepts Covered:

- **Secure Shell (SSH)** was learned as the cryptographic protocol for secure remote system access
- **Amazon Linux AMI** was understood as a lightweight, pre-configured operating system image optimized for AWS environments
- **Manual Pages (man pages)** were explored as the standard Linux help system containing comprehensive command documentation
- **Bash Shell** was reinforced as the default command-line interpreter for command execution and system interaction
- **Key-Based Authentication** was demonstrated as a secure alternative to password-based SSH connections

---

## Lab Objectives Achieved

By the conclusion of this lab, the following objectives were successfully accomplished:

1. **Established** SSH connection to an Amazon Linux EC2 instance using key-based authentication
2. **Accessed** the manual page system using the `man` command
3. **Identified** major sections and headers within man pages (NAME, SYNOPSIS, DESCRIPTION, etc.)
4. **Demonstrated** proficiency in navigating man pages using keyboard controls
5. **Understood** the purpose and structure of Linux documentation system
6. **Applied** man page search functionality to locate command information

---

## Lab Environment Components

### Pre-Configured Infrastructure

The following components were provided as part of the lab environment:

- **Amazon EC2 - Command Host:** A t2.micro or t3.micro instance deployed in a public subnet with public IP accessibility
- **Public Subnet:** Network configuration allowing inbound SSH access on port 22
- **Amazon VPC:** Virtual network providing isolated compute resources
- **Security Groups:** Pre-configured to allow inbound SSH (port 22) traffic from the lab environment

### Lab Duration

**Estimated Duration:** 30 minutes  
**Actual Completion Time:** Approximately 25-35 minutes (depending on system responsiveness and user familiarity)

---

## Task 1: SSH Connection to Amazon Linux EC2 Instance (macOS)

### Accessing Lab Credentials

The connection process began by accessing the Details panel within the Vocareum lab interface:

1. Selected the Details drop-down menu displayed above the lab instructions
2. Credentials window displayed, showing connection options
3. Made note of the **PublicIP address** for the Command Host instance
4. Exited the Details panel by selecting the X

**[Screenshot: Details Panel with Lab Credentials]**
> <img width="534" height="194" alt="Screenshot 2026-06-23 at 00 23 32" src="https://github.com/user-attachments/assets/853a98fa-2ccb-452e-a2bf-ccced238c7ef" />
><img width="769" height="249" alt="Screenshot 2026-06-23 at 00 23 42" src="https://github.com/user-attachments/assets/4e545c70-a4fa-49d3-9bd1-443e85e10481" />



### macOS: SSH Client Configuration Path

For macOS users, the connection process leveraged the native SSH client tools built into the operating system.

#### Step 1: Download Private Key

From the Details panel, the **Download PEM** button was selected to download the `labsuser.pem` file.

**Key Details:**
- File name: `labsuser.pem`
- Format: PEM (Privacy Enhanced Mail) - standard Unix/Linux format
- Typical download location: Downloads directory
- Usage: Authentication credential for SSH connection

**[Screenshot: PEM File Download Interface]**
> <img width="535" height="200" alt="Screenshot 2026-06-23 at 00 35 42" src="https://github.com/user-attachments/assets/bde6abaf-6809-45d0-87ca-f638950fc781" />

#### Step 2: Terminal Setup

A terminal window was opened on the macOS system. The directory was changed to the location where the private key file was downloaded:

```bash
cd ~/Downloads
```

This command navigated to the Downloads folder where the `labsuser.pem` file had been saved by the browser.

**[Screenshot: Terminal - Directory Navigation]**
> <img width="630" height="98" alt="Screenshot 2026-06-23 at 00 41 55" src="https://github.com/user-attachments/assets/911a28b0-5bcb-4a63-92cc-ad4f6bed792c" />



#### Step 3: Modify Key File Permissions

The permissions on the private key file were modified to be read-only, as required by SSH for security purposes:

```bash
chmod 400 labsuser.pem
```

**Permission Explanation:**
- **chmod:** Change mode command for modifying file permissions
- **400:** Numeric representation of permissions
  - First digit (4): Owner read permission
  - Second digit (0): No group permissions
  - Third digit (0): No other user permissions
- **Purpose:** SSH refuses to use keys with overly permissive access; this restriction prevents unauthorized access to the private key

**Security Importance:**
The private key file contains sensitive authentication credentials. Setting permissions to `400` (read-only for owner) ensures that only the file owner can read the key, preventing other users or processes from accessing it.

**[Screenshot: Terminal - Key Permissions Modified]**
> <img width="381" height="39" alt="Screenshot 2026-06-23 at 00 48 47" src="https://github.com/user-attachments/assets/373a2205-d24e-4165-bc72-2502bc4b4665" />
><img width="675" height="38" alt="Screenshot 2026-06-23 at 00 45 12" src="https://github.com/user-attachments/assets/6e77a60b-07b2-497d-ab2e-14419037ae7f" />



#### Step 4: SSH Connection Command

The SSH connection was established using the following command:

```bash
ssh -i labsuser.pem ec2-user@<public-ip>
```

**Command Breakdown:**
- `ssh`: SSH client command for establishing secure remote connections
- `-i labsuser.pem`: Identity flag specifying which private key file to use for authentication
- `ec2-user`: Username for the remote system (default Amazon Linux user account)
- `<public-ip>`: PublicIP address noted from the Details panel (e.g., 203.0.113.42)

**Example command with actual IP:**
```bash
ssh -i labsuser.pem ec2-user@35.94.171.109
```

**[Screenshot: Terminal - SSH Connection Command Entered]**
> <img width="594" height="26" alt="Screenshot 2026-06-23 at 00 52 19" src="https://github.com/user-attachments/assets/7f445c4c-20b1-40f1-abeb-bf04011d8fcb" />


#### Step 5: Host Key Verification

Upon executing the SSH command, the system prompted to confirm the server's host key authenticity:

```
The authenticity of host '35.94.171.109 (35.94.171.109)' can't be established.
ECDSA key fingerprint is SHA256:AbCdEfGhIjKlMnOpQrStUvWxYz1234567890.
Are you sure you want to continue connecting (yes/no/[fingerprint])?
```

The response `yes` was entered to accept the connection and add the host key to the known hosts file:

```
yes
```

**What This Means:**
- **First Connection:** The first time connecting to a host, SSH cannot verify its authenticity
- **Host Key Fingerprint:** The system displays the host's cryptographic fingerprint for verification
- **Security Check:** Users can verify this fingerprint through another trusted channel to confirm connecting to the correct server
- **Known Hosts File:** After accepting, the host key is stored in `~/.ssh/known_hosts` for future connections

**[Screenshot: Terminal - Host Key Verification Prompt]**
> <img width="797" height="80" alt="Screenshot 2026-06-23 at 00 54 55" src="https://github.com/user-attachments/assets/26904f7e-951f-426b-8f0c-4163b09b2ef9" />


#### Step 6: Successful Connection

After confirming the host key, the SSH connection was established successfully. The terminal displayed the remote shell prompt:

```
[ec2-user@ip-10-0-1-161 ~]$
```

**Connection Confirmation Details:**
- **No password prompt:** Key-based authentication succeeded without requiring a password
- **Remote prompt:** The shell prompt changed to indicate connection to the remote system
- **Format breakdown:**
  - `ec2-user`: Currently logged-in user
  - `ip-10-0-1-161`: Hostname of the EC2 instance (based on its private IP)
  - `~`: Current directory (home directory)
  - `$`: Standard user prompt (indicates non-root user)

**[Screenshot: Terminal - SSH Connection Established]**
> <img width="811" height="261" alt="Screenshot 2026-06-23 at 00 57 27" src="https://github.com/user-attachments/assets/1c49cc06-ba8e-40c4-bf30-9707a21bebb4" />


---

## Task 2: Exercise - Explore the Linux Man Pages

### Understanding the Manual Page System

The Linux manual page system (commonly referred to as "man pages") was explored as the standard help system for command documentation. The `man` command provided access to comprehensive reference documentation for virtually all command-line utilities and system functions.

### Accessing the Man Pages

The manual pages for the `man` program itself were accessed by entering the following command at the remote terminal:

```bash
man man
```

Upon execution, the terminal displayed the comprehensive documentation for the `man` command in an interactive pager interface. The pager (typically `less` on modern Linux systems) presented the documentation in a scrollable format.

**[Screenshot: Terminal - man man Command Executed]**
> <img width="813" height="481" alt="Screenshot 2026-06-23 at 00 59 28" src="https://github.com/user-attachments/assets/9f3064e2-70d1-4fdf-904f-50389fcb3aa0" />


### Navigating Man Pages

The man pages were navigated using keyboard controls to explore different sections and content:

**Navigation Controls Utilized:**
- **Up Arrow Key:** Scroll up through the documentation one line at a time
- **Down Arrow Key:** Scroll down through the documentation one line at a time
- **Space Bar:** Move down by one full screen of content
- **b Key:** Move up by one full screen of content
- **g Key:** Jump to the beginning of the man page
- **G Key:** Jump to the end of the man page
- **/ Key:** Initiate search functionality within the man pages
- **n Key:** Find next occurrence of search term
- **N Key:** Find previous occurrence of search term
- **q Key:** Exit the man pages and return to the command prompt

**User Interaction Pattern:**
The up and down arrow keys were employed to explore different sections of the man page. The scrolling allowed for detailed examination of specific command documentation areas without losing context within the overall documentation.

### Identifying Major Man Page Sections

The man page was examined to identify standard documentation sections and headers. Consistent headers appeared across most man pages, providing predictable organization of information:

#### Primary Man Page Headers Identified

**NAME**
- Provided a one-line summary of the command
- Typically formatted as: `command-name - brief description`
- Allowed for quick understanding of the command's primary purpose

**SYNOPSIS**
- Displayed the correct syntax for using the command
- Showed all available options and arguments
- Used brackets `[]` to denote optional arguments
- Used ellipsis `...` to indicate repeatable arguments
- Example format: `command [options] [arguments]`

**DESCRIPTION**
- Provided detailed explanation of the command's functionality
- Contained the most comprehensive information about what the command does
- Often included section numbers indicating command categories

**OVERVIEW**
- High-level summary of the command's role and context
- Provided conceptual background information
- Explained the command's relationship to other utilities

**EXAMPLES**
- Demonstrated practical usage scenarios
- Showed real command invocations with typical options
- Served as quick reference for common use cases

**OPTIONS**
- Listed all available command-line flags and parameters
- Provided description of each option's behavior
- Indicated whether options required arguments

**FILES**
- Listed related configuration files or data files
- Indicated where the command stored information
- Specified file locations the command read or modified

**SEE ALSO**
- Referenced related commands and documentation
- Suggested complementary utilities for extended functionality
- Directed users to advanced topics

**[Screenshot: Terminal - Man Page Headers Overview]**
> <img width="1680" height="1050" alt="Screenshot 2026-06-23 at 01 00 43" src="https://github.com/user-attachments/assets/c8203b15-e798-4b58-8942-08ca32daaa32" />


#### Examining the DESCRIPTION Section in Detail

The DESCRIPTION header was examined with particular attention paid to **section numbers**. These section numbers categorized the type of command being documented:

```
DESCRIPTION
       man is the system's manual pager. Each page argument given to man is normally
       the name of a program, utility, or function. The manual page associated with
       each of these arguments is then found and displayed. A section, if provided, will
       direct man to look only in that section of the manual. The default action is to
       search in all of the available sections following a pre-defined order and print
       only the first page found.
```

**Man Page Section Numbers:**
- **1:** User commands and programs (regular utilities)
- **2:** System calls and kernel functions
- **3:** Library functions and library-related topics
- **4:** Special files and devices
- **5:** File formats and conventions
- **6:** Games and amusements
- **7:** Miscellaneous (conventions, standards, protocols)
- **8:** System administration commands and daemons
- **9:** Kernel routines and documentation

**Practical Application:**
Understanding these section numbers allowed for more precise man page lookups. For example:
- `man 1 passwd` would show the user command for changing passwords
- `man 5 passwd` would show the password file format documentation
- `man 8 passwd` would show the system administration version

**[Screenshot: Terminal - DESCRIPTION Header and Section Numbers]**
> <img width="1633" height="204" alt="Screenshot 2026-06-23 at 01 11 49" src="https://github.com/user-attachments/assets/09642f87-1a59-4dbe-98f4-4824f5bbff6a" />
><img width="800" height="28" alt="Screenshot 2026-06-23 at 01 12 03" src="https://github.com/user-attachments/assets/9d24d4e3-f77a-43c0-9ccc-287a8db2b04e" />
><img width="1034" height="25" alt="Screenshot 2026-06-23 at 01 12 24" src="https://github.com/user-attachments/assets/734d7903-d480-4968-8118-fa18355eb894" />



### Man Page Search Functionality

The man pages included a powerful search feature for locating specific information within documentation. The search function was initiated by pressing the forward slash `/` key:

```
/search-term
```

**Search Example:**
Within the `man man` documentation, a search for "section" would be performed:

```
/section
```

The pager would highlight the first occurrence of "section" in the documentation and display that section of the page. Subsequent occurrences could be navigated using:
- **n Key:** Jump to next occurrence
- **N Key:** Jump to previous occurrence

**Practical Use Case:**
When looking for information about a specific option or concept within a large man page, the search feature allowed rapid location of relevant sections without scrolling through the entire document.

### Exiting the Man Pages

Upon completing the exploration of the man pages, the exit command was executed by pressing the `q` key:

```
q
```

The `q` key was pressed while viewing the man pages, which immediately closed the pager interface and returned the terminal to the remote command prompt:

```
[ec2-user@ip-10-0-1-161 ~]$
```

**[Screenshot: Terminal - Return to Command Prompt]**
> <img width="579" height="46" alt="Screenshot 2026-06-23 at 01 14 29" src="https://github.com/user-attachments/assets/fc7ce08d-816f-43c2-9496-1de43aa88b3e" />


---

## Key Learnings and Best Practices

### SSH Key-Based Authentication

The lab reinforced the critical importance of secure authentication mechanisms:

- **Key-Based Security:** SSH key pairs provided stronger security than password-based authentication
- **Private Key Protection:** File permissions (chmod 400) were essential for maintaining key security
- **Platform Consistency:** macOS provided native SSH tools matching standard Unix/Linux approaches
- **First Connection Warning:** Host key verification prevented man-in-the-middle attacks on initial connections
- **Known Hosts Management:** The `~/.ssh/known_hosts` file maintained a record of trusted remote hosts

### Man Pages as Documentation Resources

The man page system was recognized as an invaluable learning and reference tool:

- **Comprehensive Coverage:** Virtually all command-line utilities included man page documentation
- **Standardized Format:** Consistent section headers across different commands enabled rapid information location
- **Offline Availability:** Man pages functioned without internet connection, providing immediate help
- **Section Numbers:** Understanding section numbers aided in locating the correct documentation for specific command types
- **Search Capability:** The forward-slash search functionality within man pages enabled rapid location of specific topics
- **Always Available:** The command `man -k keyword` could search for pages related to a specific topic

### Linux Command-Line Fundamentals

Essential command-line concepts were reinforced through practical application:

- **Default Users:** Understanding that `ec2-user` was the default user for Amazon Linux instances
- **Shell Prompts:** Recognizing the `$` prompt as an indicator of standard user permissions versus `#` for root
- **Command Syntax:** Learning that commands followed consistent patterns: `command [options] [arguments]`
- **Navigation Patterns:** Using keyboard controls to efficiently navigate through documentation and system interfaces
- **Directory Navigation:** Using `cd` (change directory) to move between folders in the file system
- **Path Specification:** Understanding relative paths (`~/Downloads`) and absolute paths

### AWS-Specific Linux Knowledge

Lab-specific insights relevant to AWS environments were gained:

- **Vocareum Lab Environment:** Understanding the pre-configured nature of lab infrastructure
- **EC2 Instance Access:** Learning that public subnet instances were directly accessible via SSH
- **Security Configuration:** Recognizing that security groups were pre-configured for lab requirements
- **AMI Consistency:** Understanding that Amazon Linux AMI provided a standard, predictable environment for lab exercises
- **Default User Account:** The `ec2-user` account was the designated default user for EC2 instances running Amazon Linux

---

## Comparison with Previous EC2 Lab

This lab served as a complementary foundation to the earlier Amazon EC2 Instance Setup lab:

| Aspect | EC2 Instance Setup Lab | Linux Walkthrough Lab |
|--------|----------------------|----------------------|
| **Focus** | EC2 instance lifecycle and web server deployment | Linux CLI and documentation system |
| **Infrastructure** | Creating and managing EC2 instances from scratch | Using pre-configured EC2 instances |
| **User Data** | Custom scripts for web server configuration | Not applicable (no custom configuration) |
| **Monitoring** | CloudWatch metrics and status checks | Manual command-line exploration |
| **Knowledge** | AWS Management Console operations | Linux system access and CLI tools |
| **Duration** | ~60 minutes | ~30 minutes |
| **Connection Method** | Browser-based Management Console | SSH from local terminal |

---

## Practical Applications

The knowledge gained in this lab was directly applicable to numerous real-world scenarios:

### System Administration Tasks

- **Remote Server Management:** Accessing production Linux servers via SSH for maintenance and troubleshooting
- **Documentation Lookup:** Using man pages to rapidly resolve command syntax questions without external resources
- **Script Development:** Understanding Linux command capabilities for shell script creation
- **Problem Diagnosis:** Using command-line tools accessed through SSH to identify and resolve system issues

### Cloud Operations

- **EC2 Instance Access:** Connecting to EC2 instances for application deployment and maintenance
- **Troubleshooting:** Using SSH access to diagnose instance issues and review system logs
- **Automation:** Building scripts that would leverage Linux commands documented in man pages
- **Production Access:** Establishing secure connections to production servers in AWS environments

### Learning and Development

- **Continued Education:** Using man pages as reference for learning new commands
- **Best Practices:** Understanding standard command documentation structure for efficient learning
- **Problem Solving:** Accessing detailed command options and examples to resolve specific technical challenges
- **Self-Service Support:** Reducing dependency on external resources by using built-in documentation

---

## Conclusion

The completion of this lab provided a solid foundation for Linux command-line operations within AWS environments. The journey covered essential SSH connection methods, manual page system navigation, and core Linux documentation concepts. These foundational skills proved essential for all subsequent hands-on work with Linux-based EC2 instances and command-line system administration.

Through practical application on a live Amazon Linux EC2 instance, the lab demonstrated that mastery of SSH and the man page system unlocked the ability to independently explore and learn new Linux capabilities, supporting continuous learning in cloud computing and systems administration.

**Lab Duration:** Approximately 30 minutes (as designed)  
**Status:** Successfully Completed  
**Key Achievement:** Demonstrated proficiency in SSH connection establishment from macOS and Linux manual page system navigation, establishing essential skills for cloud computing operations and independent learning.

---

## Appendix: Important Commands Reference

### SSH Connection Commands (macOS)

```bash
cd ~/Downloads                          # Navigate to Downloads folder
chmod 400 labsuser.pem                  # Set key permissions to read-only
ssh -i labsuser.pem ec2-user@<public-ip>  # Connect via SSH to EC2 instance
```

**Example with actual IP address:**
```bash
ssh -i labsuser.pem ec2-user@203.0.113.42
```

### Man Page Navigation Commands

```bash
man <command>           # View manual page for a command
man man                 # View manual page for man itself
man -k keyword          # Search for man pages related to keyword
man 5 <filename>        # View man page for file format (section 5)
man 8 <command>         # View man page for system command (section 8)

# Within man pages:
q                       # Exit the man pages
/search-term            # Search for text (press Enter to search)
n                       # Find next occurrence
N                       # Find previous occurrence
Space                   # Scroll down one page
b                       # Scroll up one page
g                       # Jump to beginning
G                       # Jump to end
```

### Useful Amazon Linux Commands

```bash
whoami                  # Display current user (ec2-user)
hostname                # Display instance hostname
ip addr show            # Display IP address configuration
uname -a                # Display system information and OS details
cat /etc/os-release     # Display Amazon Linux version information
pwd                     # Display current working directory
ls -la                  # List files with permissions
```

### SSH Key Management

```bash
ls -la ~/.ssh/          # View SSH configuration files
cat ~/.ssh/known_hosts  # View known hosts file
ssh-keygen -l -f labsuser.pem  # View fingerprint of key file
```

---

## Additional Resources

- **AWS EC2 Documentation:** https://docs.aws.amazon.com/ec2/
- **Amazon Linux Documentation:** https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/amazon-linux-ami-basics.html
- **SSH and Key Pairs:** https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-key-pairs.html
- **Linux Manual Pages Online:** https://man7.org/linux/man-pages/
- **SSH Protocol Information:** https://tools.ietf.org/html/rfc4251
- **Amazon Linux Security Best Practices:** https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-instance-addressing.html

