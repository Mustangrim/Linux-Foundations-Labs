# Linux File Permissions and Ownership

## Lab Overview
- **Description:** Master Linux file permissions and ownership for SOC environments
- **Author:** Mykyta Palamarchuk  
- **Role Focus:** SOC Analyst | Incident Response Specialist
- **Certification:** CompTIA Security+ (SY0-701)

## Learning Objectives
- Master basic file permissions (rwx rights) and numeric/alphabetical formats
- Understand file ownership through chown and chgrp operations  
- Implement special permissions (setUID, setGID, sticky bit)
- Apply security controls for SOC environments
- Execute real-world permission scenarios for incident response

## Introduction
File permissions are critical for maintaining security in Linux environments, especially in SOC operations where proper access control can prevent privilege escalation attacks and unauthorized data access. Understanding permission management is essential for incident response specialists who need to secure compromised systems and implement proper access controls.

## Практические секции с командами

### Basic File Permissions Analysis

**Scenario:** Investigating suspicious file activities during incident response

Understanding Linux file permissions is fundamental for SOC analysts. The permission structure follows the format `-rwxrwxrwx` where the first character indicates file type (- for file, d for directory), followed by three groups of permissions for owner, group, and others.

```bash
# Navigate to investigation directory
cd /home/student/dwarfs

# Analyze file permissions for security assessment
ls -l
```

![file-permissions-ls-command](assets/screenshots/file-permissions-ls-command.png)

The output shows various permission patterns. Notice how different files have different permission sets - this is crucial for security analysis. Files with unusual permissions like `--wx--wx--wx` (write and execute only) can indicate potential security risks or malicious activity.

**Security Analysis:**
- Files with world-writable permissions pose security risks
- Execute permissions on data files may indicate compromise
- Missing read permissions can hide malicious content

### Numeric Permission Management

**Scenario:** Securing investigation files with proper access controls

Numeric permissions use octal notation where read=4, write=2, execute=1. This method provides precise control over file access rights.

```bash
# Set secure permissions for incident response files
# Owner: read, write, execute (7) | Group: read only (4) | Others: no access (0)
chmod 740 /home/student/numerical.txt

# Verify the permission change
ls -l /home/student/numerical.txt
```

![chmod-numeric-permissions](assets/screenshots/chmod-numeric-permissions.png)

**Permission Calculation:**
- **Owner (7):** 4+2+1 = read, write, execute
- **Group (4):** 4 = read only  
- **Others (0):** no permissions

This configuration ensures that sensitive investigation data is accessible only to the analyst and readable by the team, while preventing unauthorized access.

### File Ownership Investigation

**Scenario:** Identifying file ownership during forensic analysis

File ownership determines who controls a file and which group has access. This is critical for understanding potential attack vectors and privilege escalation attempts.

```bash
# Investigate file ownership in the evidence directory
ls -l /home/student/dwarfs
```

![file-ownership-root-user](assets/screenshots/file-ownership-root-user.png)

**Key Observations:**
- Files owned by `root` require elevated privileges to modify
- Files owned by regular users may indicate lateral movement
- Group ownership shows collaborative access patterns

In this example, the `bashful` file is owned by root, which could indicate:
- Administrative tool or configuration file
- Potential privilege escalation target
- System-critical file requiring protection

### Changing File Ownership

**Scenario:** Securing evidence files during incident response

When handling digital evidence, proper ownership ensures chain of custody and prevents unauthorized modifications.

```bash
# Change ownership of evidence files to investigation team
sudo chown player:team /home/student/changeme.txt

# Verify ownership change
ls -l /home/student/changeme.txt
```

![chown-ownership-change](assets/screenshots/chown-ownership-change.png)

**Security Implications:**
- `sudo` required for ownership changes (administrative control)
- Proper ownership maintains evidence integrity
- Group ownership enables team collaboration while maintaining security

### Special Permissions Implementation

**Scenario:** Configuring advanced permissions for SOC tools

Special permissions provide enhanced security controls beyond basic rwx permissions. The setUID bit allows programs to run with owner privileges regardless of who executes them.

```bash
# Apply setUID permission to SOC analysis tool
chmod u+s /home/student/setuid.txt

# Alternative numeric method: chmod 4755 filename
```

![setuid-command-execution](assets/screenshots/setuid-command-execution.png)

### Verifying Special Permissions

After applying special permissions, verification ensures proper implementation:

```bash
# Verify setUID implementation
ls -l /home/student/setuid.txt
```

![setuid-special-permissions](assets/screenshots/setuid-special-permissions.png)

**Special Permission Indicators:**
- **setUID:** 's' in user execute position
- **setGID:** 's' in group execute position  
- **Sticky bit:** 't' in others execute position

The 's' character indicates that setUID is active, meaning this file will execute with the owner's privileges regardless of who runs it.

## Lab Validation

### Knowledge Check Questions
1. **What permission pattern indicates potential security risk?**
   - Answer: World-writable permissions (--w----w-) or unusual execute permissions on data files

2. **How do you identify files owned by root in a directory?**
   - Answer: Use `ls -l` and look for "root" in the owner column

3. **What does the 's' character in file permissions indicate?**
   - Answer: setUID or setGID special permissions are set

### Practical Validation
- Successfully identify unusual file permissions using `ls -l`
- Implement proper numeric permissions using `chmod`
- Change file ownership with appropriate privileges
- Configure special permissions for security tools

## Key Takeaways

### Technical Skills Developed
- **Permission Analysis:** Ability to quickly assess file security posture
- **Access Control:** Implementation of principle of least privilege
- **Ownership Management:** Proper handling of digital evidence

### Security Applications
- **Incident Response:** Secure handling of compromise evidence
- **System Hardening:** Remove excessive permissions from sensitive files
- **Forensic Analysis:** Maintain chain of custody through proper ownership

### Professional Development
- **SOC Operations:** Essential skills for Linux-based security monitoring
- **Compliance:** Meeting regulatory requirements for data protection
- **Enterprise Security:** Understanding multi-user environment controls

## Next Steps

### Immediate Practice
- Practice permission analysis on various file types
- Experiment with different numeric permission combinations
- Test special permissions in controlled environments

### Advanced Preparation  
- Study ACLs (Access Control Lists) for granular permissions
- Explore SELinux mandatory access controls
- Research container security and permission models

### Real-World Application
- Apply learned concepts to SOC environment setup
- Implement secure file handling procedures
- Develop incident response permission checklists





---

**Lab Completed:** Linux File Permissions and Ownership  
**Focus Area:** System Security & Access Control  
**Series Progress:** 5 of 8 labs completed
