# Linux CLI Introduction

## Lab Overview

This foundational lab introduces the Linux command-line interface (CLI), providing essential navigation skills and fundamental concepts for cybersecurity professionals. You'll learn to navigate the Linux filesystem, understand directory structures, and master basic commands that form the foundation of all Linux system administration and security operations.

**Author:** Mykyta Palamarchuk  
**Role Focus:** SOC Analyst | Incident Response Specialist  
**Certification:** CompTIA Security+ (SY0-701)  
**Framework Alignment:** Linux fundamentals for cybersecurity operations

---

## Learning Objectives

- Master Linux command-line interface basics and navigation concepts
- Understand Linux filesystem hierarchy and directory structure standards
- Execute fundamental commands for system navigation and information gathering
- Differentiate between absolute and relative path navigation methods
- Apply CLI skills in practical cybersecurity scenarios and investigations
- Build confidence for advanced Linux system administration tasks

## Introduction to Command-Line Interface

### GUI vs CLI Philosophy

Have you ever wondered how everything works when you use your computer and interact with different applications in the graphical user interface (GUI)? Whether it's a simple calendar application or complex simulation software, GUI provides an intuitive way to interact with computers through visual elements like windows, icons, and menus.

However, before the GUI existed, there was the command-line interface (CLI), and to this day, many critical administrative tasks are performed exclusively through the CLI. For cybersecurity professionals, CLI proficiency is not optional—it's essential for effective incident response, system analysis, and security tool operation.

### Why CLI Matters for Cybersecurity

**Power and Precision:** CLI commands provide exact control over system operations, crucial for forensic analysis and incident response where precision matters.

**Automation Capability:** Command-line operations can be scripted and automated, enabling efficient security monitoring and response procedures.

**Remote Access:** Most security tools and servers operate in command-line environments, especially in enterprise and cloud infrastructures.

**Efficiency:** Experienced professionals can perform complex operations faster through CLI than through multiple GUI interactions.

---

## Understanding the Linux Prompt

### Prompt Structure and Components

The first element you'll encounter in any Linux terminal is the prompt—a text indicator showing system status and awaiting your commands.

![Linux Prompt Commands](assets/screenshots/linux-prompt-whoami-hostname-commands.png)

**Standard Linux Prompt Format:**
```
<username>@<hostname>:<working directory><privilege indicator>
```

### Prompt Components Explained

**Username:** The account name currently logged into the system
- Critical for security auditing and access control
- Determines available permissions and accessible resources

**Hostname:** The system's network identifier
- Essential for identifying which server you're working on
- Crucial in multi-system security environments

**Working Directory:** Current location in the filesystem
- `~` symbol indicates the user's home directory
- Shows your current position for relative path operations

**Privilege Indicator:** Visual representation of user permissions
- `$` indicates standard user privileges
- `#` indicates root (administrative) privileges
- Critical security indicator for understanding current access level

### Practical Command Examples

**Identifying Current User:**
```bash
whoami
```
Returns the username of the currently logged-in account.

**Determining System Hostname:**
```bash
hostname
```
Displays the system's network name, essential for identifying your working environment.

---

## Linux Filesystem Hierarchy

### Filesystem Hierarchy Standard (FHS)

The Filesystem Hierarchy Standard (FHS) defines the structure of Linux filesystems, providing consistency across different Linux distributions. Understanding this structure is crucial for cybersecurity professionals who need to navigate systems during investigations, configure security tools, and understand where critical files are located.

![Linux Filesystem Hierarchy Structure](assets/screenshots/linux-filesystem-hierarchy-structure.png)

### Critical Directory Functions

![Linux Filesystem Hierarchy Table](assets/screenshots/linux-filesystem-hierarchy-table.png)

**Security-Relevant Directories:**

- **`/etc`** - System configuration files, including security policies and service configurations
- **`/var`** - Variable files including logs, which are crucial for security analysis
- **`/home`** - User home directories containing personal files and configurations
- **`/tmp`** - Temporary files that may contain forensic evidence
- **`/bin` and `/sbin`** - System binaries and administrative tools

### Path Types and Navigation

**Absolute Paths:** Complete path from the filesystem root (`/`)
```bash
/var/www/index.html
```
- Always begins with forward slash (`/`)
- Provides exact location regardless of current directory
- Preferred for scripts and automation

**Relative Paths:** Path from current working directory
```bash
www/index.html
```
- Dependent on current location
- More convenient for interactive use
- Requires understanding of current position

---

## Navigation Fundamentals

### Essential Navigation Commands

#### Listing Directory Contents

The `ls` command reveals the contents of directories, providing crucial information for security analysis and system investigation.

![Directory Listing Command](assets/screenshots/ls-command-directory-listing.png)

**Basic Usage:**
```bash
ls /var/jim-stuff
```

**Advanced Options:**
- `ls -l` - Long format with permissions, ownership, and timestamps
- `ls -la` - Include hidden files (starting with `.`)
- `ls -lh` - Human-readable file sizes

#### Current Working Directory

Understanding your current location in the filesystem is fundamental for effective navigation and security operations.

![Current Working Directory](assets/screenshots/pwd-current-working-directory.png)

**Print Working Directory:**
```bash
pwd
```
This command displays your exact location in the filesystem hierarchy, essential for:
- Confirming your position before executing sensitive commands
- Understanding relative path operations
- Documenting your investigation path for forensic purposes

#### Changing Directories

Directory navigation is the foundation of Linux system administration and security analysis.

![Change Directory Navigation](assets/screenshots/cd-command-relative-navigation.png)

**Basic Navigation:**
```bash
cd /usr/local/lib/systemd/../../
```

**Navigation Shortcuts:**
- `cd` - Return to home directory
- `cd ..` - Move to parent directory
- `cd ../..` - Move up two directory levels
- `cd -` - Return to previous directory
- `cd .` - Stay in current directory (rarely used)

### Advanced Navigation Concepts

**Relative Path Navigation Example:**
Starting from: `/usr/local/lib/systemd/`
Command: `cd ../../`
Result: `/usr/local/`

**Step-by-step breakdown:**
1. First `..` moves from `/usr/local/lib/systemd/` to `/usr/local/lib/`
2. Second `..` moves from `/usr/local/lib/` to `/usr/local/`

---

## Practical Exercises

### Exercise 1: System Information Gathering

**Objective:** Gather basic system information using CLI commands.

**Tasks:**
1. Determine your current username
2. Identify the system hostname
3. Find your current working directory

**Commands:**
```bash
whoami
hostname
pwd
```

**Expected Learning:** Understanding basic system identification commands essential for security documentation and incident response.

### Exercise 2: Directory Exploration

**Objective:** Navigate the Linux filesystem and locate specific files.

**Scenario:** You're investigating a security incident and need to locate a configuration file left in `/var/jim-stuff`.

**Tasks:**
1. List contents of `/var/jim-stuff` directory
2. Identify the filename of the configuration file
3. Navigate to different system directories

**Commands:**
```bash
ls /var/jim-stuff
cd /var/jim-stuff
pwd
```

**Security Context:** In real incidents, investigators often need to locate suspicious files or configuration changes across the filesystem.

### Exercise 3: Path Navigation Mastery

**Objective:** Master absolute and relative path navigation.

**Challenge:** Navigate from `/usr/local/lib/systemd/` to `/usr/local/` using relative paths.

**Solution:**
```bash
cd /usr/local/lib/systemd/../../
pwd
```

**Expected Result:** `/usr/local/`

**Professional Application:** Efficient navigation saves time during incident response when every minute counts.

---

## Lab Validation

### Knowledge Check Questions

**Q1:** What command displays the current username?  
**A:** `whoami`

**Q2:** Which directory contains user home directories?  
**A:** `/home`

**Q3:** What does the `#` symbol indicate in a Linux prompt?  
**A:** Root (administrative) privileges

**Q4:** What file was found in `/var/jim-stuff` directory?  
**A:** `8ff6e4623173.conf`

**Q5:** After executing `cd /usr/local/lib/systemd/../../`, what is your current directory?  
**A:** `/usr/local/`

### Practical Validation

Demonstrate proficiency by completing these tasks without reference:

1. **Navigation Test:** Navigate to any directory and return to home using three different methods
2. **Information Gathering:** Collect username, hostname, and current directory in a single command sequence
3. **Path Understanding:** Explain the difference between `/home/user/file.txt` and `~/file.txt`

---

## Key Takeaways

### Technical Skills Developed

- **CLI Confidence:** Comfortable operation in command-line environments
- **Navigation Proficiency:** Efficient movement through Linux filesystem hierarchy
- **System Awareness:** Understanding of current user context and system identification
- **Path Mastery:** Confident use of both absolute and relative path navigation
- **Foundation Building:** Prerequisite skills for advanced Linux operations

### Security Applications

- **Incident Response:** Navigate compromised systems for evidence collection
- **System Analysis:** Explore filesystem during security assessments
- **Tool Configuration:** Navigate to security tool installation and configuration directories
- **Log Analysis:** Locate and access system logs for security investigation
- **Forensic Investigation:** Systematically examine filesystem evidence

### Professional Development

- **Operational Readiness:** Prepared for SOC environments that rely heavily on Linux systems
- **Certification Preparation:** Foundational skills for Linux+ and cybersecurity certifications
- **Career Advancement:** Essential skills for senior security roles and system administration
- **Automation Foundation:** Prerequisites for security scripting and automation tasks

---

## Next Steps

### Immediate Practice

- [ ] Practice navigation commands daily until they become muscle memory
- [ ] Explore your system's directory structure using `ls` and `cd`
- [ ] Create personal shortcuts and bookmarks for frequently accessed directories
- [ ] Document your learning with command history and notes

### Advanced Preparation

- [ ] Learn file permissions and ownership concepts (Lab 05)
- [ ] Understand file creation and editing techniques (Lab 02)
- [ ] Explore file management operations (Lab 03)
- [ ] Investigate environment variables and system configuration (Lab 04)

### Real-World Application

- [ ] Apply these skills in your current work environment
- [ ] Set up a personal Linux system for continued practice
- [ ] Join Linux user communities and forums for ongoing learning
- [ ] Practice with cybersecurity-focused Linux distributions like Kali Linux

---

## Additional Resources

- [Linux Command Line Basics](https://linuxcommand.org/lc3_learning_the_shell.php)
- [Filesystem Hierarchy Standard](https://refspecs.linuxfoundation.org/fhs.shtml)
- [Linux Navigation Cheat Sheet](https://www.cyberciti.biz/faq/linux-unix-getting-started-with-bash/)
- [SOC Analyst Linux Skills](https://www.sans.org/white-papers/linux-command-line-forensics/)

**Lab Completed:** Linux CLI Introduction  
**Focus Area:** Navigation & Filesystem Fundamentals  
**Series Progress:** 1 of 8 labs completed

---

  
**Continue with [Lab 02: Basic Linux File Editing](02-Basic-Linux-File-Editing.md)** 

---

*Building Linux expertise for cybersecurity excellence*
