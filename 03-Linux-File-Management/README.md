# Linux File Management

## Lab Overview

This comprehensive lab focuses on advanced file and directory operations essential for cybersecurity professionals. You'll master file creation, deletion, copying, moving, and searching techniques critical for incident response, evidence management, and security tool administration. These skills form the operational foundation for SOC analysts managing digital forensics, log analysis, and security infrastructure maintenance.

**Author:** Mykyta Palamarchuk  
**Role Focus:** SOC Analyst | Incident Response Specialist  
**Certification:** CompTIA Security+ (SY0-701)  
**Framework Alignment:** File operations for cybersecurity and digital forensics

---

## Learning Objectives

- Master file and directory creation techniques for security operations
- Understand safe file deletion practices and recursive operations
- Execute efficient file copying and moving operations for evidence management
- Implement advanced file search techniques using find and grep commands
- Apply file management skills in cybersecurity contexts and incident response
- Develop systematic approaches to filesystem organization and security

## Filesystem Management for Cybersecurity

### The Foundation of Security Operations

Understanding Linux filesystem navigation represents the cornerstone of cybersecurity expertise. Whether you're conducting digital forensics, managing security logs, or configuring monitoring tools, efficient file management skills directly impact your effectiveness as a security professional.

### Why File Management Matters in Security

**Evidence Preservation:** Proper file handling ensures forensic integrity during incident investigations.

**Log Management:** Systematic organization of security logs enables effective threat hunting and analysis.

**Tool Configuration:** Security tools require precise file placement and organization for optimal operation.

**Backup and Recovery:** Critical security data must be properly copied and archived for business continuity.

**Automation Foundation:** File management skills enable scripting and automation of security operations.

---

## Creating Files and Directories

### Understanding File Creation Methods

Linux provides multiple approaches to file creation, each suited for different security scenarios. The `touch` command creates empty files instantly, perfect for establishing file structures, creating placeholders for logs, or initializing configuration files.

### Basic Creation Commands

**Creating Empty Files:**
```bash
touch filename.txt
```
Creates an empty file or updates timestamp of existing file.

**Creating Directories:**
```bash
mkdir directoryname
```
Creates a new directory structure for organizing security data.

**Advanced Directory Creation:**
```bash
mkdir -p parent/child/grandchild
```
Creates nested directory structures in a single command.

### Practical File and Directory Creation

![File and Directory Creation](assets/screenshots/mkdir-touch-file-directory-creation.png)

**Task: Organizing Security Evidence**

**Scenario:** Setting up a directory structure for a security incident investigation.

**Objective:** Create organized storage for evidence collection and analysis.

**Steps:**
1. Create evidence directory:
   ```bash
   mkdir savings
   ```

2. Navigate to the new directory:
   ```bash
   cd savings
   ```

3. Create evidence file:
   ```bash
   touch dollar.txt
   ```

**Professional Application:**
```bash
mkdir incident_IR-2025-001
cd incident_IR-2025-001
mkdir logs network_captures system_images
touch investigation_notes.txt timeline.txt indicators_of_compromise.txt
```

**Directory Structure Benefits:**
- **Organization:** Systematic evidence management
- **Accessibility:** Quick location of specific files
- **Documentation:** Clear separation of different evidence types
- **Collaboration:** Standardized structure for team investigations

### Verification and Listing

**View Directory Contents:**
```bash
ls -l
```
Displays detailed information including permissions, ownership, and timestamps—critical for forensic documentation.

**Enhanced Listing Options:**
- `ls -la` - Include hidden files (configuration files often start with .)
- `ls -lh` - Human-readable file sizes
- `ls -lt` - Sort by modification time (recent files first)
- `ls -lS` - Sort by file size (largest first)

---

## Removing Files and Directories

### Safe Deletion Practices

File deletion in cybersecurity environments requires careful consideration of data sensitivity, forensic requirements, and operational impact. Understanding the different deletion methods ensures appropriate handling of various file types.

### Deletion Commands and Methods

**Basic File Removal:**
```bash
rm filename.txt
```
Permanently deletes a single file.

**Directory Removal (Empty):**
```bash
rmdir emptydirectory
```
Removes only empty directories—provides safety against accidental deletion.

**Recursive Deletion:**
```bash
rm -r directoryname
```
Removes directories and all contents recursively.

### Understanding Recursive Operations

**Recursive Definition:** The command executes on all files in the specified directory and continues into subdirectories, applying the same operation to all discovered files and folders.

**Security Implications:**
- **Data Loss Prevention:** Recursive operations can delete large amounts of data
- **Forensic Integrity:** Ensure proper evidence preservation before deletion
- **Audit Trails:** Document deletion activities for compliance and investigation

### Practical Deletion Exercise

![Directory Removal](assets/screenshots/rm-recursive-directory-removal.png)

**Task: Secure Evidence Cleanup**

**Scenario:** Removing outdated evidence files after case closure.

**Command:**
```bash
rm -r eeny
```

**Security Considerations:**
- **Data Sanitization:** Ensure sensitive data is properly overwritten
- **Retention Policies:** Verify legal requirements before deletion
- **Backup Verification:** Confirm important data is backed up
- **Access Logging:** Document who deleted what and when

**Advanced Deletion Options:**
```bash
rm -rf directory    # Force removal without prompts
rm -i filename      # Interactive mode (asks for confirmation)
rm -v filename      # Verbose mode (shows what's being deleted)
```

**Professional Best Practices:**
1. **Always verify** the target before deletion
2. **Use absolute paths** for critical operations
3. **Test with ls** first to confirm target selection
4. **Document deletions** for audit purposes
5. **Consider shred** for sensitive file overwriting

---

## Copying and Moving Files

### File Operation Fundamentals

Copying and moving files represents core functionality for evidence management, backup operations, and system administration. Understanding the distinction between these operations is crucial for maintaining data integrity during security investigations.

### Copy Operations (cp)

**Basic File Copying:**
```bash
cp source.txt destination.txt
```
Creates an exact duplicate while preserving the original.

**Directory Copying:**
```bash
cp -r sourcedir/ destinationdir/
```
Recursively copies entire directory structures.

### Move Operations (mv)

**File Moving/Renaming:**
```bash
mv oldname.txt newname.txt
```
Moves or renames files and directories.

**Cross-Directory Moving:**
```bash
mv /source/file.txt /destination/
```
Relocates files between different directories.

### Practical Copying Exercise

![Directory Copying](assets/screenshots/cp-recursive-directory-copying.png)

**Task: Evidence Backup Creation**

**Scenario:** Creating backup copies of digital evidence for analysis while preserving originals.

**Command:**
```bash
cp -r /home/student/dwarfs /home/student/clones
```

**Result:** Creates `/home/student/clones/dwarfs/` with identical content to original.

**Professional Applications:**

**Evidence Preservation:**
```bash
cp -r original_evidence/ working_copy/
```
Maintains forensic integrity by working on copies.

**Log Backup:**
```bash
cp /var/log/auth.log /backup/auth_$(date +%Y%m%d).log
```
Creates timestamped log backups.

**Configuration Backup:**
```bash
cp /etc/security/config.conf /etc/security/config.conf.backup
```
Preserves original configurations before modifications.

### Advanced Copy and Move Options

**Preserve Attributes:**
```bash
cp -p source dest    # Preserve timestamps and permissions
cp -a source dest    # Archive mode (preserve everything)
```

**Interactive Operations:**
```bash
cp -i source dest    # Ask before overwriting
mv -i old new        # Ask before overwriting
```

**Verbose Operations:**
```bash
cp -v source dest    # Show what's being copied
mv -v old new        # Show what's being moved
```

---

## Finding Files and Directories

### Advanced File Location Techniques

The `find` command provides powerful search capabilities essential for security investigations, system audits, and evidence location. Understanding its various options enables efficient filesystem exploration during incident response.

### Basic Find Syntax

**Name-Based Search:**
```bash
find /search/path -name 'filename'
```
Locates files by exact name match.

**Wildcard Search:**
```bash
find /search/path -name '*pattern*'
```
Finds files containing the specified pattern.

### Find Command Parameters

**Search by Type:**
- `-type f` - Regular files only
- `-type d` - Directories only
- `-type l` - Symbolic links only

**Search by Permissions:**
- `-perm 755` - Exact permission match
- `-perm -755` - At least these permissions

**Search by Time:**
- `-mtime -7` - Modified within last 7 days
- `-atime +30` - Accessed more than 30 days ago

### Practical File Search Exercise

![Find Command Search](assets/screenshots/find-command-file-search.png)

**Task: Evidence Location**

**Scenario:** Locating a specific file during security investigation.

**Command:**
```bash
find /home/student -name 'waldo'
```

**Security Applications:**

**Suspicious File Detection:**
```bash
find /home -name "*.sh" -type f -perm -755
```
Locates executable shell scripts.

**Recent File Analysis:**
```bash
find /var/log -name "*.log" -mtime -1
```
Finds log files modified in the last 24 hours.

**Large File Investigation:**
```bash
find / -type f -size +100M
```
Identifies unusually large files that might indicate data exfiltration.

**Hidden File Discovery:**
```bash
find /home -name ".*" -type f
```
Locates hidden files often used by malware.

### Complex Find Operations

**Multiple Criteria:**
```bash
find /var -name "*.log" -type f -mtime -7 -size +1M
```
Finds log files modified in last week and larger than 1MB.

**Execute Actions:**
```bash
find /tmp -name "*.tmp" -exec rm {} \;
```
Finds and deletes all temporary files.

**Output to File:**
```bash
find /home -name "*.conf" > config_files.txt
```
Creates inventory of configuration files.

---

## Searching for Patterns in Files

### Content-Based File Analysis

The `grep` command enables pattern searching within files, making it indispensable for log analysis, configuration review, and evidence examination. This functionality is crucial for identifying specific events, security indicators, and suspicious activities.

### Basic Grep Functionality

**Simple Pattern Search:**
```bash
grep 'pattern' filename
```
Finds lines containing the specified pattern.

**Recursive Directory Search:**
```bash
grep -r 'pattern' /directory/
```
Searches all files within directory structure.

### Grep Options and Parameters

**Case Sensitivity:**
- `grep -i 'pattern'` - Case-insensitive search
- `grep 'Pattern'` - Case-sensitive search (default)

**Output Control:**
- `grep -n 'pattern'` - Show line numbers
- `grep -c 'pattern'` - Count matching lines
- `grep -v 'pattern'` - Show non-matching lines

**Context Display:**
- `grep -A 3 'pattern'` - Show 3 lines after match
- `grep -B 3 'pattern'` - Show 3 lines before match
- `grep -C 3 'pattern'` - Show 3 lines before and after

### Practical Pattern Search Exercise

![Grep Pattern Search](assets/screenshots/grep-pattern-search-files.png)

**Task: Security Log Analysis**

**Scenario:** Analyzing files for specific security events or indicators.

**Command:**
```bash
grep -r 'work' /home/student/dwarfs
```

**Result:** Identifies 7 files containing the pattern "work".

**Security Applications:**

**Failed Login Detection:**
```bash
grep -i "failed" /var/log/auth.log
```
Identifies authentication failures.

**IP Address Investigation:**
```bash
grep -r "192.168.1.100" /var/log/
```
Traces activities from specific IP address.

**Malware Signature Search:**
```bash
grep -r "eval.*base64" /var/www/
```
Searches for encoded malicious code patterns.

**Configuration Analysis:**
```bash
grep -n "password" /etc/security/*.conf
```
Locates password-related configuration entries.

### Advanced Grep Techniques

**Regular Expressions:**
```bash
grep -E '[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}' logfile
```
Finds IP addresses using regex patterns.

**Multiple Patterns:**
```bash
grep -E 'error|warning|critical' /var/log/syslog
```
Searches for multiple keywords simultaneously.

**File Type Filtering:**
```bash
grep -r --include="*.log" 'pattern' /var/
```
Searches only specific file types.

---

## Practical Exercises

### Exercise 1: Security Evidence Organization

**Objective:** Create a systematic evidence collection structure.

**Scenario:** Preparing for a security incident investigation requiring organized file management.

**Tasks:**
1. Create investigation directory structure:
   ```bash
   mkdir investigation_2025_001
   cd investigation_2025_001
   mkdir evidence logs analysis reports
   ```

2. Create initial documentation files:
   ```bash
   touch evidence/network_logs.pcap
   touch logs/system_events.log
   touch analysis/malware_analysis.txt
   touch reports/executive_summary.md
   ```

3. Verify structure:
   ```bash
   find . -type f
   ls -la */
   ```

### Exercise 2: Log File Management

**Objective:** Practice copying and organizing security logs.

**Scenario:** Backing up and organizing system logs for analysis.

**Setup:**
```bash
mkdir original_logs backup_logs analysis_workspace
echo "2025-01-10 Failed login from 192.168.1.100" > original_logs/auth.log
echo "2025-01-10 Suspicious file access detected" > original_logs/security.log
```

**Tasks:**
1. Create working copies:
   ```bash
   cp original_logs/* backup_logs/
   cp original_logs/* analysis_workspace/
   ```

2. Verify backup integrity:
   ```bash
   diff original_logs/auth.log backup_logs/auth.log
   ```

3. Search for security events:
   ```bash
   grep -r "Failed" analysis_workspace/
   grep -r "Suspicious" analysis_workspace/
   ```

### Exercise 3: Security File Audit

**Objective:** Locate and analyze security-relevant files.

**Scenario:** Conducting a security audit to identify potential vulnerabilities.

**Tasks:**
1. Find configuration files:
   ```bash
   find /etc -name "*.conf" -type f 2>/dev/null | head -10
   ```

2. Locate recently modified files:
   ```bash
   find /home -mtime -1 -type f 2>/dev/null
   ```

3. Search for security keywords:
   ```bash
   grep -r "password" /etc/security/ 2>/dev/null
   grep -r "admin" /etc/passwd
   ```

### Exercise 4: Evidence Cleanup Simulation

**Objective:** Safely remove outdated investigation files.

**Setup:**
```bash
mkdir old_investigation
touch old_investigation/case_notes.txt
touch old_investigation/evidence.img
mkdir old_investigation/temp_files
touch old_investigation/temp_files/cache.tmp
```

**Tasks:**
1. Review before deletion:
   ```bash
   find old_investigation -type f
   du -sh old_investigation
   ```

2. Selective cleanup:
   ```bash
   rm old_investigation/temp_files/cache.tmp
   rmdir old_investigation/temp_files
   ```

3. Complete removal:
   ```bash
   rm -r old_investigation
   ```

---

## Lab Validation

### Knowledge Check Questions

**Q1:** What command creates an empty file named "evidence.txt"?  
**A:** `touch evidence.txt`

**Q2:** How do you copy a directory and all its contents?  
**A:** `cp -r sourcedir/ destinationdir/`

**Q3:** What's the difference between `rm` and `rmdir`?  
**A:** `rm` removes files and can remove directories with `-r`, while `rmdir` only removes empty directories

**Q4:** Which command searches for files by name?  
**A:** `find`

**Q5:** How many files contained the pattern "work" in the dwarfs directory?  
**A:** 7

**Q6:** What does the `-r` flag do in `grep -r`?  
**A:** Searches recursively through all files in directories and subdirectories

### Practical Validation

Demonstrate proficiency by completing these tasks:

1. **File Organization:** Create a directory structure for security incident response
2. **Safe Deletion:** Remove test files and directories using appropriate commands
3. **Evidence Backup:** Copy important files while preserving timestamps
4. **File Location:** Use find to locate specific configuration files
5. **Content Analysis:** Use grep to search for security-related patterns

### Advanced Challenge

**Scenario:** Security Incident Response Drill

1. Create an incident response workspace with proper directory structure
2. Generate sample log files with security events
3. Use find and grep to locate suspicious activities
4. Create backup copies of evidence while preserving metadata
5. Document your investigation process in a findings report

**Success Criteria:**
- Systematic file organization
- Proper use of recursive operations
- Effective search and analysis techniques
- Safe file handling practices
- Complete documentation

---

## 📈 Key Takeaways

### Technical Skills Developed

- **File System Mastery:** Confident creation, deletion, and manipulation of files and directories
- **Advanced Search Capabilities:** Efficient location of files and content using find and grep
- **Evidence Management:** Proper handling and organization of digital evidence
- **Backup Operations:** Systematic copying and preservation of critical data
- **Security Analysis:** Pattern recognition and content analysis for threat detection

### Security Applications

- **Digital Forensics:** Systematic evidence collection and analysis workflows
- **Incident Response:** Rapid file location and analysis during security events
- **Log Management:** Efficient organization and analysis of security logs
- **Configuration Auditing:** Systematic review of system configuration files
- **Threat Hunting:** Proactive search for indicators of compromise

### Professional Development

- **SOC Operations:** Essential skills for security operations center environments
- **Forensic Readiness:** Prepared for digital forensics investigations
- **System Administration:** Competent file management for security infrastructure
- **Automation Foundation:** Building blocks for security scripting and automation
- **Compliance Support:** Organized approach to evidence preservation and audit trails

---

## Next Steps

### Immediate Practice

- [ ] Practice file operations daily with security-focused scenarios
- [ ] Create personal templates for incident response file structures
- [ ] Experiment with complex find and grep patterns
- [ ] Develop systematic approaches to evidence organization

### Advanced Preparation

- [ ] Learn environment variables for automation (Lab 04)
- [ ] Understand file permissions and security controls (Lab 05)
- [ ] Explore user management and access controls (Lab 06)
- [ ] Study authentication mechanisms and SSH (Lab 07-08)

### Real-World Application

- [ ] Apply these skills in your current security environment
- [ ] Create standard operating procedures for evidence handling
- [ ] Develop scripts for automated file management tasks
- [ ] Practice with real security log analysis scenarios

---

## Additional Resources

- [Linux Find Command Guide](https://www.gnu.org/software/findutils/manual/html_mono/find.html)
- [Grep and Regular Expressions](https://www.gnu.org/software/grep/manual/grep.html)
- [Digital Forensics File Management](https://www.sans.org/white-papers/digital-forensics-file-systems/)
- [Security Log Analysis Techniques](https://www.cyberciti.biz/faq/searching-multiple-words-string-using-grep/)

**Lab Completed:** Linux File Management  
**Focus Area:** File Operations & Search Techniques  
**Series Progress:** 3 of 8 labs completed

---

  
**Continue with [Lab 04: Linux Environment Variables](04-Linux-Environment-Variables.md)** 

---

*Mastering file management for cybersecurity excellence*
