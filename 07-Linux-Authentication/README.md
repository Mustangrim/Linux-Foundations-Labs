# Linux Authentication

## Lab Overview
This advanced lab explores Linux authentication mechanisms and SSH key management, essential for securing access to cybersecurity infrastructure and incident response systems. You'll master authentication file analysis, password security assessment, and SSH key-based authentication implementation critical for SOC operations, forensic investigations, and secure remote access management. These skills enable proper authentication hardening, security policy enforcement, and secure communication channels in enterprise security environments.

**Author:** Mykyta Palamarchuk  
**Role Focus:** SOC Analyst | Incident Response Specialist  
**Certification:** CompTIA Security+ (SY0-701)  
**Framework Alignment:** Authentication and access control for cybersecurity professionals

## Learning Objectives

- Master Linux authentication file analysis (/etc/passwd, /etc/shadow) for security assessment
- Understand password hash analysis and encryption algorithms for forensic investigations
- Implement SSH key-based authentication for secure SOC operations and incident response
- Apply authentication security controls and hardening techniques for compliance requirements
- Develop systematic approaches to authentication management for security operations
- Configure secure remote access solutions for cybersecurity teams and forensic environments

### Authentication in Cybersecurity Context

**Security Assessment:** Authentication file analysis reveals user privileges, password policies, and potential security vulnerabilities during security audits and penetration testing.

**Forensic Investigation:** Understanding authentication mechanisms enables incident responders to analyze user access patterns, identify unauthorized access, and trace attack vectors during forensic investigations.

**Access Hardening:** SSH key management and authentication controls provide secure remote access for SOC teams while reducing password-based attack vectors.

**Compliance Management:** Authentication controls support regulatory requirements through proper access documentation, password policies, and secure communication channels.

## Password File Analysis

### Understanding /etc/passwd Structure

The `/etc/passwd` file contains essential user account information and serves as the primary user database for Linux systems. Understanding its structure is crucial for security assessment and user management in enterprise environments.

![passwd-file-structure-diagram](assets/screenshots/passwd-file-structure-diagram.png)

**File Structure Components:**
- **Username:** User account identifier
- **Password Field:** Usually 'x' indicating shadow file usage
- **UID:** User ID for system identification
- **GID:** Primary group ID
- **GECOS:** User information (full name, contact details)
- **Home Directory:** User's personal directory path
- **Shell:** Default command interpreter

### Practical Authentication Analysis

**Scenario:** Conducting security assessment of user accounts during penetration testing or security audit.

![passwd-file-analysis-commands](assets/screenshots/passwd-file-analysis-commands.png)

**Task:** System User Analysis for Security Assessment
```bash
# Analyze root user configuration
grep "^root:" /etc/passwd

# Examine student account properties
grep "^student:" /etc/passwd

# Check games user shell restrictions
grep "^games:" /etc/passwd
```

**Security Analysis Applications:**
```bash
# Find users with shell access (potential security risk)
grep -v "/usr/sbin/nologin\|/sbin/nologin\|/bin/false" /etc/passwd

# Identify users with UID 0 (root privileges)
awk -F: '$3 == 0 {print $1}' /etc/passwd

# List users with home directories in non-standard locations
awk -F: '$6 !~ /^\/home\// && $3 >= 1000 {print $1 ":" $6}' /etc/passwd

# Find system accounts that might be misconfigured
awk -F: '$3 < 1000 && $7 != "/usr/sbin/nologin" && $7 != "/bin/false" {print $1}' /etc/passwd
```

**Professional Application:** Security analysts use passwd file analysis to identify potential security misconfigurations, unauthorized accounts, and access control violations during security assessments and compliance audits.

### User Group Management

**Scenario:** Modifying user group membership for role-based access control in security operations.

![usermod-add-user-to-group](assets/screenshots/usermod-add-user-to-group.png)

**Task:** Implementing Group-Based Access Control
```bash
# Add user to security-relevant group
sudo usermod -a -G games student

# Verify group membership change
grep "^games:" /etc/group
```

**Advanced Group Management for Security Teams:**
```bash
# Add SOC analyst to security tools groups
sudo usermod -a -G wireshark,tcpdump,docker soc_analyst

# Grant incident responder access to forensic tools
sudo usermod -a -G disk,sleuthkit,volatility ir_specialist

# Provide security engineer with administrative groups
sudo usermod -a -G sudo,adm,systemd-journal security_engineer

# Create security-focused group assignments
sudo usermod -a -G security_team,log_analysts,threat_hunters new_analyst
```

## Shadow File Security Analysis

### Understanding Password Security Storage

The `/etc/shadow` file stores encrypted passwords and password policy information, providing enhanced security over traditional password storage methods. This file is crucial for password security assessment and forensic analysis.

![shadow-file-detailed-analysis](assets/screenshots/shadow-file-detailed-analysis.png)

**Task:** Comprehensive Shadow File Analysis
```bash
# Detailed analysis of user password information
sudo awk -F: '/^root:/ {
    print "Username: " $1
    print "Encrypted Password: " $2
    print "Last Password Change: " $3
    print "Min Days Between Change: " $4
    print "Max Days Between Change: " $5
    print "Warning Period: " $6
    print "Inactive Period: " $7
    print "Expiration Date: " $8
    print "Unused: " $9
}' /etc/shadow
```

**Security Assessment Applications:**
```bash
# Find accounts with no password expiration
sudo awk -F: '$5 == "" || $5 == "99999" {print $1}' /etc/shadow

# Identify locked accounts
sudo awk -F: '$2 ~ /^!/ {print $1}' /etc/shadow

# Find accounts with expired passwords
sudo awk -F: '$3 != "" && $5 != "" && $3 + $5 < systime()/86400 {print $1}' /etc/shadow

# Check for weak password policies
sudo awk -F: '$4 < 1 || $5 > 365 {print $1 " - Weak policy"}' /etc/shadow
```

### Password Hash Algorithm Analysis

**Scenario:** Analyzing password encryption methods for security assessment and compliance verification.

![password-hash-algorithm-analysis](assets/screenshots/password-hash-algorithm-analysis.png)

**Task:** Hash Algorithm Security Assessment
```bash
# Analyze student password hash algorithm
sudo grep "^student:" /etc/shadow
```

**Hash Algorithm Security Analysis:**
- **$1$** - MD5 (Legacy, vulnerable)
- **$2$** - Blowfish (Moderate security)
- **$5$** - SHA-256 (Good security)
- **$6$** - SHA-512 (Strong security - recommended)

**Security Applications:**
```bash
# Find users with weak hash algorithms (MD5)
sudo awk -F: '$2 ~ /^\$1\$/ {print $1 " - MD5 (Weak)"}' /etc/shadow

# Identify modern hash algorithms in use
sudo awk -F: '$2 ~ /^\$6\$/ {print $1 " - SHA-512 (Strong)"}' /etc/shadow

# Audit password hash strength across all users
sudo awk -F: '{
    if ($2 ~ /^\$1\$/) print $1 " - MD5 (Weak)"
    else if ($2 ~ /^\$5\$/) print $1 " - SHA-256 (Good)"
    else if ($2 ~ /^\$6\$/) print $1 " - SHA-512 (Strong)"
    else if ($2 == "*" || $2 == "!") print $1 " - Locked"
}' /etc/shadow
```

### Root Account Security Verification

**Scenario:** Verifying root account security configuration for compliance and security hardening.

![root-password-security-check](assets/screenshots/root-password-security-check.png)

**Task:** Root Account Security Assessment
```bash
# Check root account password configuration
sudo grep "^root:" /etc/shadow
```

**Root Security Analysis:**
- **Password Field "*"** - Password authentication disabled
- **Password Field "!"** - Account locked
- **Hash Value** - Password authentication enabled (security risk)

**Security Hardening Applications:**
```bash
# Disable root password authentication (recommended)
sudo passwd -l root

# Verify root account is properly secured
sudo passwd -S root

# Check for any UID 0 accounts other than root
awk -F: '$3 == 0 && $1 != "root" {print "WARNING: " $1 " has UID 0"}' /etc/passwd
```

## SSH Key-Based Authentication

### Understanding SSH Key Security

SSH key-based authentication provides superior security compared to password authentication by using cryptographic key pairs. This method eliminates password-based attacks while enabling secure automation and remote access for security operations.

### SSH Key Generation

**Scenario:** Setting up secure key-based authentication for SOC analyst remote access to security infrastructure.

![ssh-keygen-key-generation](assets/screenshots/ssh-keygen-key-generation.png)

**Task:** Generating Secure SSH Key Pairs
```bash
# Generate RSA key pair with passphrase protection
ssh-keygen

# Use default location: /home/student/.ssh/id_rsa
# Set strong passphrase: MyNewSSHK3y
```

**Advanced SSH Key Generation:**
```bash
# Generate Ed25519 key (modern, secure algorithm)
ssh-keygen -t ed25519 -C "soc-analyst@company.com"

# Generate RSA key with specific bit length
ssh-keygen -t rsa -b 4096 -C "incident-responder@company.com"

# Generate ECDSA key for compatibility
ssh-keygen -t ecdsa -b 521 -C "security-engineer@company.com"

# Generate key with custom filename
ssh-keygen -t ed25519 -f ~/.ssh/forensics_key -C "forensics@company.com"
```

**Security Considerations:**
- **Passphrase Protection:** Always use strong passphrases to protect private keys
- **Key Algorithm:** Prefer Ed25519 > RSA 4096 > RSA 2048 for new keys
- **Key Management:** Store private keys securely, never share them
- **Key Rotation:** Regularly rotate keys for enhanced security

### SSH Key Deployment

**Scenario:** Deploying public keys to remote security systems for automated incident response and monitoring access.

![ssh-copy-id-deploy-key](assets/screenshots/ssh-copy-id-deploy-key.png)

**Task:** Secure Key Deployment to Remote Systems
```bash
# Deploy public key to remote server
ssh-copy-id student@server

# Use password authentication for initial setup
# Password: student
```

**Enterprise Key Deployment:**
```bash
# Deploy keys to multiple security servers
for server in soc-server forensics-server siem-server; do
    ssh-copy-id analyst@$server
done

# Deploy specific key to remote system
ssh-copy-id -i ~/.ssh/forensics_key.pub forensics@evidence-server

# Manual key deployment (when ssh-copy-id unavailable)
cat ~/.ssh/id_rsa.pub | ssh user@server "mkdir -p ~/.ssh && cat >> ~/.ssh/authorized_keys"

# Set proper permissions after manual deployment
ssh user@server "chmod 700 ~/.ssh && chmod 600 ~/.ssh/authorized_keys"
```

### SSH Key Authentication Verification

**Scenario:** Verifying successful SSH key deployment and testing secure authentication for security operations.

![authorized-keys-verification](assets/screenshots/authorized-keys-verification.png)

**Task:** Authentication Verification and Testing
```bash
# Connect using key-based authentication
ssh student@server
# Enter passphrase: MyNewSSHK3y

# Verify authorized_keys configuration
cat ~/.ssh/authorized_keys
```

**Key Management Best Practices:**
```bash
# Check authorized_keys file permissions
ls -la ~/.ssh/authorized_keys

# Verify SSH key fingerprints
ssh-keygen -l -f ~/.ssh/id_rsa.pub

# List all authorized keys with details
ssh-keygen -l -f ~/.ssh/authorized_keys

# Remove specific key from authorized_keys
ssh-keygen -R hostname ~/.ssh/authorized_keys
```

## Practical Exercises

### Exercise 1: Security Authentication Audit

**Objective:** Conduct comprehensive authentication security assessment for enterprise environment.

**Scenario:** Security team needs to audit authentication configurations across critical systems for compliance and security posture assessment.

**Tasks:**

1. **User Account Analysis:**
```bash
# Identify privileged accounts
awk -F: '$3 < 100 {print $1 " (UID: " $3 ")"}' /etc/passwd

# Find accounts with shell access
awk -F: '$7 !~ /nologin|false/ {print $1 " - Shell: " $7}' /etc/passwd

# Check for duplicate UIDs
awk -F: '{print $3}' /etc/passwd | sort | uniq -d
```

2. **Password Security Assessment:**
```bash
# Audit password hash algorithms
sudo awk -F: '{
    if ($2 ~ /^\$1\$/) weak++
    else if ($2 ~ /^\$6\$/) strong++
    else if ($2 == "*" || $2 == "!") locked++
} END {
    print "Weak (MD5): " weak
    print "Strong (SHA-512): " strong  
    print "Locked: " locked
}' /etc/shadow

# Check password expiration policies
sudo awk -F: '$5 > 90 || $5 == "" {print $1 " - Weak expiration policy"}' /etc/shadow
```

3. **SSH Configuration Review:**
```bash
# Check SSH key types in use
find ~/.ssh -name "*.pub" -exec ssh-keygen -l -f {} \;

# Audit authorized_keys across user accounts
for user_home in /home/*; do
    if [ -f "$user_home/.ssh/authorized_keys" ]; then
        echo "User: $(basename $user_home)"
        wc -l "$user_home/.ssh/authorized_keys"
    fi
done
```

### Exercise 2: Incident Response Authentication Setup

**Objective:** Configure secure authentication infrastructure for incident response team deployment.

**Scenario:** Major security incident requiring rapid deployment of incident response team with secure remote access to affected systems.

**Tasks:**

1. **Emergency Access Key Generation:**
```bash
# Generate incident-specific SSH keys
ssh-keygen -t ed25519 -f ~/.ssh/incident_2025_001 -C "ir-team@company.com"

# Create team access key
ssh-keygen -t rsa -b 4096 -f ~/.ssh/team_access -C "emergency-access"
```

2. **Secure Key Distribution:**
```bash
# Deploy emergency access keys
ssh-copy-id -i ~/.ssh/incident_2025_001.pub ir_lead@compromised-server
ssh-copy-id -i ~/.ssh/team_access.pub forensics@evidence-server

# Configure key-based access for automation
ssh-copy-id -i ~/.ssh/automation_key.pub monitor@siem-server
```

3. **Access Verification and Documentation:**
```bash
# Test incident response access
ssh -i ~/.ssh/incident_2025_001 ir_lead@compromised-server "whoami && date"

# Document deployed keys
for key in ~/.ssh/*.pub; do
    echo "Key: $key"
    ssh-keygen -l -f "$key"
    echo "---"
done
```

### Exercise 3: Authentication Hardening Implementation

**Objective:** Implement comprehensive authentication security controls for SOC infrastructure.

**Scenario:** SOC team needs to harden authentication mechanisms across security infrastructure to meet compliance requirements and security best practices.

**Tasks:**

1. **Password Policy Enforcement:**
```bash
# Lock accounts with weak passwords
sudo passwd -l legacy_account
sudo passwd -l service_account

# Set strong password policies for security accounts
sudo chage -M 60 soc_analyst
sudo chage -W 7 soc_analyst
sudo chage -I 30 soc_analyst
```

2. **SSH Security Hardening:**
```bash
# Generate new secure keys for team
ssh-keygen -t ed25519 -f ~/.ssh/soc_team_key -C "soc-hardened@company.com"

# Deploy hardened authentication
ssh-copy-id -i ~/.ssh/soc_team_key.pub soc@hardened-server

# Remove old/weak keys
sed -i '/ssh-rsa.*old-key/d' ~/.ssh/authorized_keys
```

3. **Access Control Verification:**
```bash
# Verify authentication hardening
sudo awk -F: '$2 !~ /^\$6\$/ && $2 != "*" && $2 != "!" {
    print $1 " - Non-SHA512 hash detected"
}' /etc/shadow

# Check SSH key strength
for key in ~/.ssh/*.pub; do
    key_type=$(ssh-keygen -l -f "$key" | awk '{print $4}')
    if [[ "$key_type" == "(RSA)" ]]; then
        bit_length=$(ssh-keygen -l -f "$key" | awk '{print $1}')
        if [ "$bit_length" -lt 2048 ]; then
            echo "Weak RSA key: $key ($bit_length bits)"
        fi
    fi
done
```

## Lab Validation

### Knowledge Check Questions

**Q1:** What character in the password field of /etc/passwd indicates shadow file usage?
**A:** `x`

**Q2:** Which hash algorithm is indicated by $6$ in /etc/shadow?
**A:** SHA-512

**Q3:** What does a "*" or "!" in the password field of /etc/shadow indicate?
**A:** The account cannot authenticate using password (locked)

**Q4:** What command generates an SSH key pair?
**A:** `ssh-keygen`

**Q5:** What file stores authorized SSH public keys for a user?
**A:** `~/.ssh/authorized_keys`

**Q6:** Which UID always represents the root user?
**A:** 0

### Practical Validation

Demonstrate proficiency by completing these tasks:

- **Authentication Analysis:** Successfully analyze /etc/passwd and /etc/shadow files for security assessment
- **Hash Security:** Identify and evaluate password hash algorithms for compliance verification
- **SSH Key Management:** Generate, deploy, and verify SSH key-based authentication
- **Security Hardening:** Implement authentication controls and access restrictions

### Advanced Challenge

**Scenario:** Enterprise Authentication Security Transformation

Design and implement a comprehensive authentication security program for a growing cybersecurity organization:

1. **Authentication Infrastructure Assessment:**
   - Audit existing authentication mechanisms across all systems
   - Identify security vulnerabilities and compliance gaps
   - Document current authentication policies and procedures

2. **Security Hardening Implementation:**
   - Implement strong password policies and hash algorithms
   - Deploy SSH key-based authentication across infrastructure
   - Configure authentication logging and monitoring

3. **Compliance and Documentation:**
   - Establish authentication security policies and procedures
   - Implement regular authentication security audits
   - Create incident response procedures for authentication compromise

**Success Criteria:**
- Comprehensive authentication security assessment
- Implementation of modern authentication controls
- Compliance with security policies and standards
- Effective authentication monitoring and incident response capabilities

## Key Takeaways

### Technical Skills Developed

- **Authentication Analysis:** Comprehensive understanding of Linux authentication file structures and security assessment
- **Password Security:** Advanced knowledge of password hash algorithms and security evaluation
- **SSH Key Management:** Practical skills in key generation, deployment, and management for secure access
- **Security Hardening:** Implementation of authentication controls and access restrictions
- **Forensic Investigation:** Authentication analysis skills for incident response and forensic examination

### Security Applications

- **Security Assessment:** Authentication file analysis for penetration testing and security audits
- **Incident Response:** SSH key management for rapid and secure incident response team deployment
- **Compliance Management:** Authentication controls supporting regulatory and policy requirements
- **Infrastructure Security:** Secure remote access implementation for SOC operations and security teams
- **Threat Investigation:** Authentication analysis for identifying unauthorized access and attack vectors

### Professional Development

- **SOC Operations:** Enhanced authentication security skills for security operations center environments
- **Incident Response:** Advanced remote access and secure communication capabilities
- **Compliance Expertise:** Understanding of authentication requirements and audit procedures
- **Security Engineering:** Skills for designing and implementing secure authentication infrastructure
- **Forensic Analysis:** Authentication-focused investigation techniques for cybersecurity incidents

## Next Steps

### Advanced Preparation

- Learn advanced SSH configuration and hardening techniques (Lab 08)
- Explore multi-factor authentication implementation
- Study certificate-based authentication and PKI infrastructure
- Research authentication integration with SIEM and security monitoring systems

### Real-World Application

- Implement enterprise authentication security policies and procedures
- Develop automated authentication auditing and compliance checking
- Create incident response procedures for authentication compromise scenarios
- Design secure remote access solutions for cybersecurity operations

## Additional Resources

- [SSH Key Management Best Practices](https://www.ssh.com/academy/ssh/keygen)
- [Linux Authentication Security Guide](https://www.cyberciti.biz/tips/linux-security.html)
- [Password Security and Hash Analysis](https://www.nist.gov/publications/digital-identity-guidelines-authentication-and-lifecycle-management)
- [Authentication for Security Operations](https://www.sans.org/white-papers/linux-authentication-security/)

**Lab Completed:** Linux Authentication  
**Focus Area:** Authentication & Access Control  
**Series Progress:** 7 of 8 labs completed

---

  
**Continue with [Lab 08: SSH Basics](08-SSH-Basics.md)** 

---

*Building secure authentication foundations for cybersecurity excellence*
