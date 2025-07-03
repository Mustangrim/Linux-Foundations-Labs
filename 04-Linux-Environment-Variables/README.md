# Linux Environment Variables

## Lab Overview

This advanced lab explores Linux environment and shell variables, essential for automation, configuration management, and security tool deployment. You'll master variable types, persistence mechanisms, and practical applications for cybersecurity operations. These skills enable efficient system configuration, security tool automation, and infrastructure management in SOC environments.

**Author:** Mykyta Palamarchuk  
**Role Focus:** SOC Analyst | Incident Response Specialist  
**Certification:** CompTIA Security+ (SY0-701)  
**Framework Alignment:** System automation and configuration for cybersecurity professionals

---

## Learning Objectives

- Distinguish between shell variables and environment variables for system configuration
- Master variable creation, modification, and deletion techniques for automation
- Implement variable promotion and demotion for flexible system management
- Configure persistent environment variables for consistent security tool operation
- Apply variable management skills in cybersecurity contexts and infrastructure automation
- Develop systematic approaches to environment configuration for security operations

## Introduction to Linux Variables

### Understanding Variable Types in Security Operations

Linux variables serve as the foundation for system automation, security tool configuration, and operational efficiency in cybersecurity environments. Understanding the distinction between shell variables and environment variables enables precise control over system behavior, tool deployment, and automated security processes.

### Variables in Cybersecurity Context

**Configuration Management:** Variables control security tool behavior, API endpoints, and operational parameters across different environments.

**Automation Foundation:** Security scripts rely on variables for flexible operation across development, testing, and production environments.

**Tool Integration:** Security platforms use environment variables for authentication, configuration, and integration with other systems.

**Session Management:** Variables maintain state information critical for incident response workflows and investigation processes.

---

## Shell Variables Fundamentals

### Understanding Shell Variable Scope

Shell variables exist only within the current shell session and are not inherited by child processes or subshells. This characteristic makes them ideal for temporary configurations, session-specific settings, and local script operations.

### Common Shell Variables

**System Information Variables:**
- `HOSTNAME` - Current system's network identifier
- `UID` - User identifier for the current session
- `BASH_VERSION` - Shell version in human-readable format
- `BASH_VERSINFO` - Shell version in machine-readable format
- `IFS` - Input Field Separator for command parsing

### Listing Shell Variables

**Complete Variable Display:**
```bash
set
```
Shows all shell variables, functions, and environment variables.

**Filtered Variable Display:**
```bash
set | more
```
Paginated output for easier review of extensive variable lists.

**Specific Variable Search:**
```bash
set | grep BASH=
```
Locates specific shell variables using pattern matching.

### Practical Shell Variable Identification

![Shell Variable Search](assets/screenshots/shell-variable-grep-search.png)

**Task: Locating Specific Shell Variables**

**Scenario:** Identifying system-specific variables for security script configuration.

**Command:**
```bash
set | grep SHELL_VAR
```

**Professional Application:** Security analysts frequently need to identify existing variables before configuring new automation scripts or integrating security tools.

### Creating Shell Variables

![Shell Variable Creation](assets/screenshots/shell-variable-creation.png)

**Basic Variable Assignment:**
```bash
MY_SHELL_VAR="Hello Linux World"
```

**Security-Focused Examples:**
```bash
INCIDENT_ID="IR-2025-001"
ANALYST_NAME="mykyta_palamarchuk"
INVESTIGATION_DATE="2025-01-10"
LOG_LEVEL="DEBUG"
```

**Variable Naming Best Practices:**
- Use descriptive names for clarity
- Follow consistent naming conventions
- Avoid special characters except underscores
- Use uppercase for environment variables
- Use lowercase or mixed case for local variables

---

## Environment Variables Mastery

### Understanding Environment Variable Inheritance

Environment variables are inherited by child shells and processes, making them essential for system-wide configuration, security tool deployment, and automated operations. Unlike shell variables, environment variables persist across process boundaries and subshell execution.

### Critical Environment Variables

**System Configuration Variables:**
- `SHELL` - Command interpreter for user sessions
- `USER` - Current user identifier (informational)
- `HOME` - User's home directory path
- `PWD` - Present working directory
- `OLDPWD` - Previous working directory
- `PATH` - Executable search path directories
- `_` - Last executed command

**Security Relevance:**
- `PATH` controls which security tools are accessible
- `HOME` determines configuration file locations
- `USER` provides context for access control and auditing

### Environment Variable Operations

**Listing All Environment Variables:**
```bash
printenv
```
Displays complete environment variable inventory.

**Specific Variable Inspection:**
```bash
printenv HOME
```
Shows individual variable values for verification.

**Multiple Variable Display:**
```bash
printenv HOME PATH USER
```
Efficiently examines multiple variables simultaneously.

### Practical Environment Variable Analysis

![Environment Variable Display](assets/screenshots/environment-variable-printenv.png)

**Task: Environment Variable Investigation**

**Scenario:** Auditing system environment for security configuration verification.

**Command:**
```bash
printenv ENV_VAR
```

**Alternative Method:**
```bash
echo $ENV_VAR
```

**Professional Application:** Security assessments often require environment variable analysis to identify potential misconfigurations or security vulnerabilities.

### Creating Environment Variables

![Environment Variable Export](assets/screenshots/environment-variable-export.png)

**Export Command Syntax:**
```bash
export MY_ENV_VAR="Linux Environment Variable"
```

**Security Tool Configuration Examples:**
```bash
export SPLUNK_HOME="/opt/splunk"
export NESSUS_API_KEY="your_api_key_here"
export INCIDENT_RESPONSE_PLAYBOOK="/opt/security/playbooks"
export THREAT_INTEL_FEED="https://api.threatintel.example.com"
```

**Verification Process:**
```bash
printenv MY_ENV_VAR
echo $MY_ENV_VAR
```

---

## Variable Management Operations

### Removing Variables from Environment

Variable cleanup is essential for security hygiene, preventing information leakage, and maintaining clean system environments during security operations.

![Variable Unset Operations](assets/screenshots/unset-variables-command.png)

**Single Variable Removal:**
```bash
unset VAR_NAME
```

**Multiple Variable Removal:**
```bash
unset ENV_VAR SHELL_VAR
```

**Security Applications:**
```bash
unset API_KEY SESSION_TOKEN TEMP_PASSWORD
```

**Best Practices for Variable Cleanup:**
- Remove sensitive variables after use
- Clear temporary configuration variables
- Unset variables containing credentials or tokens
- Document variable lifecycle in security procedures

### Variable Type Conversion

![Variable Promotion and Demotion](assets/screenshots/variable-promotion-demotion.png)

**Promoting Shell Variable to Environment Variable:**
```bash
export MY_SHELL_VAR
```
Converts local shell variable to environment variable inheritance.

**Demoting Environment Variable to Shell Variable:**
```bash
export -n MY_ENV_VAR
```
Removes environment variable status while preserving value.

**Practical Security Scenarios:**

**Promotion Example:**
```bash
SECURITY_TOOL="/opt/security/scanner"
export SECURITY_TOOL  # Now available to child processes
```

**Demotion Example:**
```bash
export -n TEMP_CONFIG  # Remove from environment, keep as shell variable
```

---

## Persistent Environment Variables

### Understanding Variable Persistence

Temporary variables exist only during the current session, while persistent variables automatically load in new shell sessions. Persistence is crucial for security tool configuration, automation scripts, and operational consistency across system reboots and user sessions.

### Configuration File Hierarchy

**User-Specific Configuration Files:**
- `~/.bashrc` - Variables for non-login bash shells
- `~/.bash_profile` - Variables for bash login shells
- `~/.profile` - Variables for any Bourne-compatible shell

**System-Wide Configuration Files:**
- `/etc/environment` - Global environment variables for all shells
- `/etc/profile` - Login shell environment configuration
- `/etc/profile.d/` - Modular environment configuration directory

### Implementing Persistent PATH Configuration

**Scenario:** Adding Golang binaries to system PATH for security tool development.

**Temporary PATH Modification:**
```bash
export PATH="$PATH:/usr/local/go/bin"
```

**Persistent Configuration Implementation:**

![Persistent PATH Configuration](assets/screenshots/persistent-path-configuration.png)

**Step 1: Create Configuration File**
```bash
sudo nano /etc/profile.d/gopath.sh
```

**Step 2: Add PATH Export**
```bash
export PATH="$PATH:/usr/local/go/bin"
```

**Step 3: Apply Configuration**
```bash
source /etc/profile.d/gopath.sh
```

**Step 4: Verify PATH Update**

![PATH Variable Verification](assets/screenshots/path-variable-verification.png)

```bash
echo $PATH
```

### Security Tool PATH Configuration

**Example: Adding Security Tools to PATH**
```bash
# Create security tools PATH configuration
sudo nano /etc/profile.d/security-tools.sh

# Add security tool directories
export PATH="$PATH:/opt/nmap/bin:/opt/burp:/opt/metasploit/bin"
export PATH="$PATH:/opt/wireshark/bin:/opt/nessus/bin"
```

**Benefits of Persistent Configuration:**
- Consistent tool availability across sessions
- Simplified command execution without full paths
- Standardized environment for all security team members
- Automated tool deployment and configuration

---

## Practical Exercises

### Exercise 1: Security Environment Setup

**Objective:** Configure a complete security analysis environment using variables.

**Scenario:** Setting up variables for a new security incident investigation.

**Tasks:**
1. Create shell variables for incident tracking:
   ```bash
   INCIDENT_NUMBER="IR-2025-001"
   ANALYST_NAME="mykyta_palamarchuk"
   INVESTIGATION_START=$(date '+%Y-%m-%d %H:%M:%S')
   ```

2. Promote critical variables to environment level:
   ```bash
   export INCIDENT_NUMBER
   export ANALYST_NAME
   ```

3. Create work directory variables:
   ```bash
   EVIDENCE_DIR="/home/analyst/incidents/$INCIDENT_NUMBER"
   REPORT_DIR="$EVIDENCE_DIR/reports"
   LOG_DIR="$EVIDENCE_DIR/logs"
   ```

4. Verify configuration:
   ```bash
   printenv | grep INCIDENT
   echo "Investigation: $INCIDENT_NUMBER by $ANALYST_NAME"
   ```

### Exercise 2: Security Tool Configuration

**Objective:** Configure environment variables for security tool integration.

**Scenario:** Setting up variables for automated security scanning and monitoring.

**Tasks:**
1. Configure scanner variables:
   ```bash
   export NMAP_OPTIONS="-sS -O -v"
   export SCAN_TARGETS="/etc/security/targets.txt"
   export SCAN_RESULTS="/var/log/security/scans"
   ```

2. Set monitoring variables:
   ```bash
   export SIEM_SERVER="splunk.company.com"
   export SIEM_PORT="8089"
   export LOG_RETENTION_DAYS="90"
   ```

3. Create authentication variables:
   ```bash
   export API_ENDPOINT="https://api.security.company.com"
   # Note: Never store actual credentials in variables
   export CONFIG_FILE="/etc/security/credentials.conf"
   ```

4. Test configuration:
   ```bash
   echo "Scanner config: $NMAP_OPTIONS"
   echo "SIEM endpoint: $SIEM_SERVER:$SIEM_PORT"
   ```

### Exercise 3: Persistent Security Configuration

**Objective:** Create persistent environment configuration for security operations.

**Scenario:** Establishing permanent environment variables for SOC team operations.

**Tasks:**
1. Create security team configuration file:
   ```bash
   sudo nano /etc/profile.d/security-soc.sh
   ```

2. Add security-specific variables:
   ```bash
   # SOC Team Environment Configuration
   export SOC_TEAM="Blue Team Alpha"
   export SOC_SHIFT="Day Shift"
   export INCIDENT_RESPONSE_PLAYBOOK="/opt/security/playbooks"
   export THREAT_INTEL_FEED="https://api.threatintel.company.com"
   
   # Security Tool Paths
   export PATH="$PATH:/opt/security/tools/bin"
   export PATH="$PATH:/opt/forensics/tools"
   
   # Log Analysis Configuration
   export DEFAULT_LOG_DIR="/var/log/security"
   export LOG_ANALYSIS_TOOLS="/opt/log-analysis"
   ```

3. Apply and verify configuration:
   ```bash
   source /etc/profile.d/security-soc.sh
   printenv | grep SOC
   echo $PATH | tr ':' '\n' | grep security
   ```

### Exercise 4: Variable Lifecycle Management

**Objective:** Practice variable creation, modification, and cleanup procedures.

**Setup:**
```bash
# Create test variables
TEST_VAR1="temporary_value"
export TEST_VAR2="environment_value"
SENSITIVE_DATA="confidential_info"
```

**Tasks:**
1. Variable inspection and modification:
   ```bash
   # Check current state
   set | grep TEST_VAR
   printenv | grep TEST_VAR
   
   # Modify variables
   TEST_VAR1="updated_temporary"
   export TEST_VAR2="updated_environment"
   ```

2. Variable type conversion:
   ```bash
   # Promote shell variable
   export TEST_VAR1
   
   # Demote environment variable
   export -n TEST_VAR2
   ```

3. Security cleanup:
   ```bash
   # Remove sensitive variables
   unset SENSITIVE_DATA TEST_VAR1 TEST_VAR2
   
   # Verify cleanup
   set | grep -E "TEST_VAR|SENSITIVE"
   ```

---

## Lab Validation

### Knowledge Check Questions

**Q1:** What command displays all current shell variables?  
**A:** `set`

**Q2:** How do you create an environment variable named "SECURITY_TOOL"?  
**A:** `export SECURITY_TOOL="value"`

**Q3:** What's the difference between shell variables and environment variables?  
**A:** Shell variables exist only in the current shell, while environment variables are inherited by child processes

**Q4:** Which command removes a variable from the environment?  
**A:** `unset variable_name`

**Q5:** How do you promote a shell variable to an environment variable?  
**A:** `export variable_name`

**Q6:** Where should you place persistent system-wide environment variables?  
**A:** `/etc/profile.d/` directory

### Practical Validation

Demonstrate proficiency by completing these tasks:

1. **Variable Creation:** Create both shell and environment variables with security-relevant names
2. **Variable Inspection:** Use appropriate commands to examine variable values and types
3. **Variable Modification:** Change variable values and convert between shell and environment types
4. **Persistent Configuration:** Create a persistent environment variable configuration file
5. **Variable Cleanup:** Safely remove variables from the environment

### Advanced Challenge

**Scenario:** Security Operations Center Environment Setup

1. Design a complete SOC environment variable configuration
2. Create persistent variables for security tools and procedures
3. Implement variables for incident response workflows
4. Configure PATH variables for security tool accessibility
5. Document the environment configuration for team use

**Success Criteria:**
- Proper variable scoping and inheritance
- Persistent configuration across sessions
- Security-focused variable naming and organization
- Clean variable lifecycle management
- Comprehensive documentation

---

## Key Takeaways

### Technical Skills Developed

- **Variable Type Mastery:** Complete understanding of shell vs environment variable characteristics
- **Configuration Management:** Systematic approach to system and application configuration
- **Persistence Implementation:** Ability to create lasting environment configurations
- **Variable Lifecycle:** Proper creation, modification, and cleanup procedures
- **Automation Foundation:** Prerequisites for advanced scripting and automation

### Security Applications

- **Tool Configuration:** Systematic setup of security tool environments and parameters
- **Automation Enablement:** Foundation for security script automation and orchestration
- **Environment Standardization:** Consistent configuration across SOC team members
- **Credential Management:** Understanding of secure variable handling for sensitive data
- **Incident Response:** Rapid environment setup for investigation and response activities

### Professional Development

- **SOC Operations:** Enhanced capability for security operations center environments
- **DevSecOps Integration:** Skills applicable to security automation and CI/CD pipelines
- **System Administration:** Advanced Linux administration capabilities for security infrastructure
- **Script Development:** Foundation skills for security automation and tool development
- **Team Collaboration:** Standardized approaches for shared security environments

---

## Next Steps

### Immediate Practice

- [ ] Create personal security environment variable configurations
- [ ] Practice variable type conversion and lifecycle management
- [ ] Implement persistent configurations for commonly used security tools
- [ ] Experiment with PATH modifications for tool accessibility

### Advanced Preparation

- [ ] Learn file permissions and ownership for secure configurations (Lab 05)
- [ ] Understand user management for multi-user security environments (Lab 06)
- [ ] Explore authentication mechanisms and SSH configuration (Lab 07-08)
- [ ] Study advanced scripting techniques using environment variables

### Real-World Application

- [ ] Configure your organization's security tool environment variables
- [ ] Create standardized environment configurations for security teams
- [ ] Develop automation scripts utilizing environment variables
- [ ] Document environment setup procedures for incident response

---

## Additional Resources

- [Bash Variables Guide](https://www.gnu.org/software/bash/manual/html_node/Shell-Variables.html)
- [Environment Variables Best Practices](https://12factor.net/config)
- [Linux Environment Configuration](https://www.cyberciti.biz/faq/set-environment-variable-linux/)
- [Security Automation with Environment Variables](https://www.sans.org/white-papers/linux-shell-scripting-security/)

**Lab Completed:** Linux Environment Variables  
**Focus Area:** System Configuration & Automation  
**Series Progress:** 4 of 8 labs completed

---

  
**Continue with [Lab 05: Linux File Permissions and Ownership](05-Linux-File-Permissions-and-Ownership.md)** 

---

*Building automation capabilities for cybersecurity excellence*
