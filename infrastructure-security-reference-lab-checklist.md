# Infrastructure Security Reference Lab — ADHD-Friendly Build Checklist

> **Rule:** Only work on one checkbox group at a time.  
> **Goal:** Finish small, working phases you can explain in an interview.  
> **Status key:** ⬜ Not started · 🟨 In progress · ✅ Complete · 🛑 Blocked

---

# Project Dashboard

## Current Focus
- [ ] Current phase:
- [ ] Today’s tiny goal:
- [ ] Time box: 30 / 60 / 90 minutes
- [ ] Stop point:

## Overall Progress
- [ ] Phase 0 — Repository setup
- [ ] Phase 1 — Linux hardening with Ansible
- [ ] Phase 2 — Network segmentation and firewall
- [ ] Phase 3 — Terraform and AWS
- [ ] Phase 4 — Monitoring and vulnerability management
- [ ] Phase 5 — CI/CD security
- [ ] Phase 6 — Kerberos and HPC fundamentals
- [ ] Phase 7 — Portfolio write-up

---

# Phase 0 — Create the Project

## What am I doing?
Creating one organized repository that will hold every part of the lab.

## Why does it matter?
It shows planning, documentation, Git usage, and security architecture thinking.

## Checklist
- [ ] Create GitHub repository: `infrastructure-security-reference-lab`
- [ ] Add a short README
- [ ] Credit the starter repository under **Inspiration**
- [ ] Add project status: `In Progress`
- [ ] Add this folder structure:

```text
ansible/
terraform/
networking/
monitoring/
vulnerability-management/
ci/
hpc/
docs/
screenshots/
runbooks/
```

- [ ] Add `.gitignore`
- [ ] Add `LICENSE`
- [ ] Add `ROADMAP.md`
- [ ] Draw a simple architecture diagram
- [ ] Commit: `docs: create initial project structure`

## Done when
- [ ] Repository is public
- [ ] README explains the project in under 60 seconds
- [ ] Architecture diagram is visible
- [ ] Next phase is clearly listed

## Be able to explain
- [ ] What the project is
- [ ] Why it is split into phases
- [ ] Why documentation is part of engineering

---

# Phase 1 — Linux Hardening with Ansible

## What am I doing?
Using Ansible to securely configure a Linux server instead of changing settings manually.

## Why does it matter?
It demonstrates Linux administration, automation, hardening, repeatability, and selected CIS-aligned controls.

---

## Step 1.1 — Build Two Linux VMs

### What am I doing?
Creating one machine to run Ansible and one machine for Ansible to manage.

### Checklist
- [ ] Create VM 1: `ansible-control`
- [ ] Create VM 2: `linux-server`
- [ ] Install Ubuntu Server
- [ ] Record each IP address
- [ ] Confirm both machines can reach each other
- [ ] Save one screenshot

### Done when
- [ ] Both VMs boot
- [ ] You know both IP addresses
- [ ] `ping` works between them

### Be able to explain
- [ ] Control node
- [ ] Managed node
- [ ] Why VMs are useful for labs

---

## Step 1.2 — Configure SSH

### What am I doing?
Allowing the Ansible control machine to securely connect to the managed server.

### Checklist
- [ ] Create an SSH key
- [ ] Copy the public key to the managed server
- [ ] Test SSH login
- [ ] Confirm login works without entering the server password
- [ ] Record the command used

### Test
```bash
ssh username@server-ip
```

### Done when
- [ ] SSH key login works
- [ ] You can connect reliably

### Be able to explain
- [ ] SSH
- [ ] Public and private keys
- [ ] Why private keys must stay secret

---

## Step 1.3 — Create Ansible Inventory

### What am I doing?
Telling Ansible which server it is responsible for managing.

### Checklist
- [ ] Install Ansible
- [ ] Create `ansible/inventory.ini`
- [ ] Add the managed server IP
- [ ] Create `ansible/ansible.cfg`
- [ ] Test connectivity

### Test
```bash
ansible all -i inventory.ini -m ping
```

### Done when
- [ ] Ansible returns `pong`

### Be able to explain
- [ ] Inventory
- [ ] Ansible module
- [ ] Why Ansible normally uses SSH

---

## Step 1.4 — Create the Base Playbook

### What am I doing?
Writing a repeatable checklist that prepares the Linux server.

### Checklist
- [ ] Create `site.yml`
- [ ] Update package lists
- [ ] Install security updates
- [ ] Install required packages
- [ ] Create an administrative user
- [ ] Set correct permissions
- [ ] Run the playbook

### Test
```bash
ansible-playbook -i inventory.ini site.yml
```

### Done when
- [ ] Playbook completes without errors
- [ ] Required packages are installed

### Be able to explain
- [ ] Playbook
- [ ] Task
- [ ] YAML
- [ ] Desired state

---

## Step 1.5 — Add SSH Hardening

### What am I doing?
Reducing the chance of unauthorized remote access.

### Checklist
- [ ] Disable direct root SSH login
- [ ] Disable password authentication
- [ ] Keep SSH-key authentication enabled
- [ ] Validate SSH configuration before restart
- [ ] Restart SSH safely
- [ ] Confirm you can still log in

### Validate
```bash
sshd -T
```

### Done when
- [ ] Root login is disabled
- [ ] Password login is disabled
- [ ] SSH key login still works

### Be able to explain
- [ ] Why root SSH is risky
- [ ] Why key authentication is stronger
- [ ] How bad SSH changes can lock you out

---

## Step 1.6 — Add Host Firewall

### What am I doing?
Allowing only the network traffic the server needs.

### Checklist
- [ ] Install UFW
- [ ] Allow SSH from the management network
- [ ] Deny unused inbound traffic
- [ ] Enable UFW
- [ ] Confirm the rules
- [ ] Run Nmap before and after

### Validate
```bash
sudo ufw status verbose
nmap server-ip
```

### Done when
- [ ] Only expected ports are exposed
- [ ] You captured before-and-after evidence

### Be able to explain
- [ ] Host firewall
- [ ] Port
- [ ] Allow rule
- [ ] Deny rule

---

## Step 1.7 — Add Basic CIS-Aligned Controls

### What am I doing?
Applying a small, documented set of security controls based on recognized hardening guidance.

### Checklist
- [ ] Enable automatic security updates
- [ ] Install and enable `auditd`
- [ ] Disable at least one unnecessary service
- [ ] Configure time synchronization
- [ ] Set secure file permissions
- [ ] Add selected kernel settings
- [ ] Create `docs/cis-control-mapping.md`

### Control table
| Control | Ansible task | Validation command | Result |
|---|---|---|---|
| Disable root SSH | | | |
| Enable firewall | | | |
| Enable auditd | | | |
| Auto updates | | | |

### Done when
- [ ] At least five controls are automated
- [ ] Every control has a validation command
- [ ] You call it “selected CIS-aligned controls,” not full compliance

### Be able to explain
- [ ] What CIS Benchmarks are
- [ ] Difference between hardening and compliance
- [ ] Why controls need validation

---

## Step 1.8 — Prove Idempotence

### What am I doing?
Showing that running automation again does not make unnecessary changes.

### Checklist
- [ ] Run the playbook once
- [ ] Save the results
- [ ] Run it a second time
- [ ] Confirm zero unexpected changes
- [ ] Save a screenshot
- [ ] Document any task that was not idempotent

### Done when
- [ ] Second run shows no unnecessary changes

### Be able to explain
- [ ] Idempotence
- [ ] Why repeatable automation matters
- [ ] Why non-idempotent tasks can be dangerous

---

# Phase 2 — Network Segmentation and Firewall

## What am I doing?
Splitting systems into separate trust zones and controlling traffic between them.

## Why does it matter?
It demonstrates network security, firewall design, segmentation, trust boundaries, and lateral-movement reduction.

---

## Step 2.1 — Build the Network Design

### What am I doing?
Planning where each system belongs before creating firewall rules.

### Checklist
- [ ] Create Management zone
- [ ] Create Internal zone
- [ ] Create DMZ zone
- [ ] Choose subnets
- [ ] Place each VM into a zone
- [ ] Draw the network diagram
- [ ] Create `networking/network-plan.md`

### Example
```text
Management: 10.10.10.0/24
Internal:   10.10.20.0/24
DMZ:        10.10.30.0/24
```

### Done when
- [ ] Every system has a zone
- [ ] Every zone has a purpose

### Be able to explain
- [ ] Subnet
- [ ] VLAN
- [ ] Firewall zone
- [ ] Trust boundary
- [ ] DMZ

---

## Step 2.2 — Deploy OPNsense or pfSense

### What am I doing?
Creating a virtual firewall that controls communication between zones.

### Checklist
- [ ] Create firewall VM
- [ ] Add one interface per network
- [ ] Assign IP addresses
- [ ] Confirm each zone can reach the firewall
- [ ] Back up the firewall configuration
- [ ] Save screenshots

### Done when
- [ ] Firewall routes traffic between zones
- [ ] Firewall dashboard is accessible from Management

### Be able to explain
- [ ] Router versus firewall
- [ ] Interface
- [ ] Gateway
- [ ] Stateful firewall

---

## Step 2.3 — Create Default-Deny Rules

### What am I doing?
Blocking cross-zone traffic unless it is specifically required.

### Checklist
- [ ] Deny DMZ to Management
- [ ] Deny DMZ to Internal
- [ ] Allow Management to servers over SSH
- [ ] Allow required monitoring traffic
- [ ] Allow HTTPS to the DMZ test service
- [ ] Document every rule and its reason

### Firewall rule table
| Source | Destination | Port | Allow/Deny | Reason |
|---|---|---:|---|---|
| Management | Linux server | 22 | Allow | Administration |
| DMZ | Management | Any | Deny | Prevent lateral movement |
| User | DMZ web server | 443 | Allow | Web access |

### Done when
- [ ] No “allow any to any” rules exist
- [ ] Every allow rule has a business reason

### Be able to explain
- [ ] Default deny
- [ ] Least privilege
- [ ] Source and destination
- [ ] Inbound and outbound traffic

---

## Step 2.4 — Test Allowed and Blocked Traffic

### What am I doing?
Proving that firewall rules work.

### Checklist
- [ ] Test an allowed SSH connection
- [ ] Test an allowed HTTPS connection
- [ ] Test DMZ to Management and confirm failure
- [ ] Test DMZ to Internal and confirm failure
- [ ] Capture firewall logs
- [ ] Save screenshots
- [ ] Write one troubleshooting note

### Test
```bash
nc -vz destination-ip port
```

### Done when
- [ ] Allowed paths succeed
- [ ] Denied paths fail
- [ ] Logs match the tests

### Be able to explain
- [ ] Positive test
- [ ] Negative test
- [ ] Why logs are needed
- [ ] How segmentation limits lateral movement

---

## Step 2.5 — Create a Small Threat Model

### What am I doing?
Connecting threats to controls and tests.

### Checklist
- [ ] Identify five important assets
- [ ] Identify one threat for each asset
- [ ] Identify one security control
- [ ] Identify how to test the control
- [ ] Create `docs/threat-model.md`

### Template
| Asset | Threat | Control | Validation |
|---|---|---|---|
| Ansible controller | Credential theft | SSH keys + Vault | Secret scan |
| DMZ server | Lateral movement | Segmentation | Blocked connection test |

### Done when
- [ ] Five rows are complete
- [ ] Every control has a validation method

### Be able to explain
- [ ] Asset
- [ ] Threat
- [ ] Risk
- [ ] Control
- [ ] Validation

---

# Phase 3 — Terraform and AWS Security Baseline

## What am I doing?
Using code to create a small, secure AWS environment.

## Why does it matter?
It demonstrates Terraform, cloud security, AWS networking, IAM, logging, encryption, and repeatable infrastructure.

---

## Step 3.1 — Create the Terraform Foundation

### Checklist
- [ ] Install Terraform
- [ ] Create `terraform/aws/`
- [ ] Add `providers.tf`
- [ ] Add `variables.tf`
- [ ] Add `outputs.tf`
- [ ] Add `main.tf`
- [ ] Add remote-state planning notes
- [ ] Run formatting and validation

### Test
```bash
terraform fmt
terraform init
terraform validate
terraform plan
```

### Done when
- [ ] Terraform plan works without errors

### Be able to explain
- [ ] Provider
- [ ] Resource
- [ ] Variable
- [ ] Output
- [ ] State

---

## Step 3.2 — Build AWS Networking

### Checklist
- [ ] Create a VPC
- [ ] Create one public subnet
- [ ] Create one private subnet
- [ ] Create route tables
- [ ] Create internet gateway
- [ ] Decide whether a NAT gateway is worth the cost
- [ ] Add security groups
- [ ] Tag all resources

### Done when
- [ ] Diagram matches deployed resources
- [ ] Private resources do not have public IPs

### Be able to explain
- [ ] VPC
- [ ] Public subnet
- [ ] Private subnet
- [ ] Route table
- [ ] Internet gateway
- [ ] NAT
- [ ] Security group

---

## Step 3.3 — Add Secure Compute Access

### Checklist
- [ ] Create one EC2 test instance
- [ ] Place it in the private subnet
- [ ] Create an IAM role
- [ ] Enable Systems Manager access
- [ ] Avoid public SSH where possible
- [ ] Restrict security groups
- [ ] Encrypt storage

### Done when
- [ ] Instance has no public IP
- [ ] You can access it through Systems Manager
- [ ] No long-lived AWS keys are stored in code

### Be able to explain
- [ ] IAM role versus access key
- [ ] Why public SSH increases exposure
- [ ] Least privilege
- [ ] Encryption at rest

---

## Step 3.4 — Add AWS Logging and Detection

### Checklist
- [ ] Enable CloudTrail
- [ ] Enable VPC Flow Logs
- [ ] Create encrypted S3 log storage
- [ ] Block public S3 access
- [ ] Add log-retention notes
- [ ] Optionally enable GuardDuty
- [ ] Add budget alerts

### Done when
- [ ] AWS activity is logged
- [ ] Network flow data is logged
- [ ] Log storage is not public

### Be able to explain
- [ ] CloudTrail
- [ ] VPC Flow Logs
- [ ] GuardDuty
- [ ] Centralized logging
- [ ] Why logs need protection

---

## Step 3.5 — Scan the Terraform

### Checklist
- [ ] Install Trivy or Checkov
- [ ] Scan the Terraform
- [ ] Save initial findings
- [ ] Fix at least three findings
- [ ] Scan again
- [ ] Save before-and-after evidence

### Done when
- [ ] Scan passes or remaining findings are explained

### Be able to explain
- [ ] IaC scanning
- [ ] Misconfiguration
- [ ] False positive
- [ ] Risk acceptance

---

# Phase 4 — Monitoring and Vulnerability Management

## What am I doing?
Finding weaknesses, fixing them, and proving they were resolved.

## Why does it matter?
This shows the full vulnerability-management lifecycle instead of only running a scanner.

---

## Step 4.1 — Add Wazuh Monitoring

### Checklist
- [ ] Deploy Wazuh manager
- [ ] Install agent on at least one server
- [ ] Confirm the agent is connected
- [ ] Generate a failed-login event
- [ ] Generate a file-change event
- [ ] Generate a service-change event
- [ ] Save screenshots
- [ ] Explain each alert

### Done when
- [ ] Three test alerts are visible
- [ ] You can explain what caused each one

### Be able to explain
- [ ] SIEM
- [ ] Agent
- [ ] Log source
- [ ] File integrity monitoring
- [ ] Detection versus prevention

---

## Step 4.2 — Run Vulnerability Scans

### Checklist
- [ ] Run Nmap
- [ ] Run Trivy against a container
- [ ] Run Greenbone/OpenVAS if practical
- [ ] Select only three findings
- [ ] Verify each finding manually
- [ ] Record severity and evidence

### Done when
- [ ] Three verified findings are documented

### Be able to explain
- [ ] Authenticated versus unauthenticated scan
- [ ] Vulnerability versus exposure
- [ ] Scanner result versus verified finding
- [ ] CVE
- [ ] Severity

---

## Step 4.3 — Remediate and Retest

### Checklist
- [ ] Prioritize the three findings
- [ ] Fix one with Ansible
- [ ] Fix one through configuration
- [ ] Patch one vulnerable package or image
- [ ] Rescan
- [ ] Show closure
- [ ] Document one exception scenario

### Vulnerability lifecycle table
| Finding | Risk | Fix | Retest | Status |
|---|---|---|---|---|
| | | | | |

### Done when
- [ ] All three findings have a final status
- [ ] Before-and-after evidence exists

### Be able to explain
- [ ] Prioritization
- [ ] Remediation
- [ ] Mitigation
- [ ] Retesting
- [ ] Exception management
- [ ] Compensating control

---

## Step 4.4 — Add Security Metrics

### Checklist
- [ ] Count managed assets
- [ ] Count hardened assets
- [ ] Count findings before remediation
- [ ] Count findings after remediation
- [ ] Track failed authentications
- [ ] Track blocked firewall attempts
- [ ] Track Ansible success rate
- [ ] Create one simple dashboard or table

### Done when
- [ ] Metrics use real lab data
- [ ] Every metric has a purpose

### Be able to explain
- [ ] Coverage metric
- [ ] Risk metric
- [ ] Operational metric
- [ ] Why vanity metrics are weak

---

# Phase 5 — CI/CD Security

## What am I doing?
Automatically checking infrastructure code before changes are merged.

## Why does it matter?
It demonstrates secure development practices and automated policy enforcement.

---

## Step 5.1 — Create GitHub Actions Workflow

### Checklist
- [ ] Create `.github/workflows/security.yml`
- [ ] Run `terraform fmt`
- [ ] Run `terraform validate`
- [ ] Run Trivy or Checkov
- [ ] Run `ansible-lint`
- [ ] Run `yamllint`
- [ ] Run Gitleaks
- [ ] Run ShellCheck
- [ ] Add Python tests if applicable

### Done when
- [ ] Workflow runs on pull requests
- [ ] Clean code passes

### Be able to explain
- [ ] CI/CD
- [ ] Pipeline
- [ ] Pull request
- [ ] Automated gate
- [ ] Shift-left security

---

## Step 5.2 — Prove the Pipeline Blocks Risk

### Checklist
- [ ] Create a test branch
- [ ] Add an insecure rule such as SSH from `0.0.0.0/0`
- [ ] Open a pull request
- [ ] Capture failed CI result
- [ ] Fix the rule
- [ ] Capture passed CI result
- [ ] Add screenshots to the README

### Done when
- [ ] One insecure change is blocked
- [ ] The corrected change passes

### Be able to explain
- [ ] Preventive control
- [ ] Policy as code
- [ ] Why automated checks reduce human error

---

# Phase 6 — Kerberos and HPC Fundamentals

## What am I doing?
Building a small cluster to learn centralized authentication and workload scheduling.

## Why does it matter?
It gives hands-on fundamentals for HPC, Kerberos, Slurm, and shared storage without pretending to be enterprise-scale.

---

## Step 6.1 — Build a Small Slurm Cluster

### Checklist
- [ ] Create `slurm-controller`
- [ ] Create `compute01`
- [ ] Create `compute02`
- [ ] Configure hostname resolution
- [ ] Configure consistent users
- [ ] Install Munge
- [ ] Install Slurm
- [ ] Join compute nodes
- [ ] Submit a hostname test job
- [ ] Submit a CPU test job
- [ ] Capture queue output

### Done when
- [ ] Both compute nodes are healthy
- [ ] At least two jobs complete successfully

### Be able to explain
- [ ] HPC
- [ ] Cluster
- [ ] Controller node
- [ ] Compute node
- [ ] Workload scheduler
- [ ] Job queue
- [ ] CPU and memory requests

---

## Step 6.2 — Add Kerberos Fundamentals

### Checklist
- [ ] Create Kerberos server
- [ ] Configure a realm
- [ ] Add one user principal
- [ ] Configure clients
- [ ] Test `kinit`
- [ ] Test `klist`
- [ ] Confirm invalid login fails
- [ ] Document time-sync requirement
- [ ] Protect keytab permissions

### Done when
- [ ] Valid user receives a ticket
- [ ] Invalid user does not
- [ ] Clients can contact the KDC

### Be able to explain
- [ ] KDC
- [ ] Realm
- [ ] Principal
- [ ] Ticket
- [ ] Keytab
- [ ] Why time synchronization matters

---

## Step 6.3 — Add Shared Storage

### Checklist
- [ ] Start with NFS
- [ ] Mount storage on both compute nodes
- [ ] Write a file from one node
- [ ] Read it from the other node
- [ ] Test file permissions
- [ ] Separate storage traffic if possible
- [ ] Document how Lustre differs from NFS
- [ ] Optional: create a small Lustre proof of concept

### Done when
- [ ] Both nodes use the same shared directory
- [ ] Permissions work correctly
- [ ] Lustre architecture is documented

### Be able to explain
- [ ] Shared storage
- [ ] Mount
- [ ] NFS
- [ ] Lustre
- [ ] Metadata server
- [ ] Object storage server
- [ ] Why HPC needs high-throughput storage

---

## Step 6.4 — Study MAAS as an Extension

### Checklist
- [ ] Document the MAAS machine lifecycle
- [ ] Explain PXE boot
- [ ] Explain commissioning
- [ ] Explain deployment
- [ ] Try one nested or physical-node proof of concept if hardware allows
- [ ] Document limitations honestly

### Done when
- [ ] MAAS lifecycle diagram exists
- [ ] One proof of concept is attempted or the design is fully documented

### Be able to explain
- [ ] Bare-metal provisioning
- [ ] PXE
- [ ] DHCP
- [ ] Commissioning
- [ ] Machine lifecycle

---

# Phase 7 — Portfolio Write-Up

## What am I doing?
Turning technical work into clear evidence that a hiring manager can understand quickly.

## Why does it matter?
A project only helps if another person can see what you built, why it matters, and how you proved it worked.

---

## Step 7.1 — Create the Main Case Study

### Checklist
- [ ] Project summary
- [ ] Problem statement
- [ ] Architecture diagram
- [ ] Technologies used
- [ ] Threat model
- [ ] Implementation steps
- [ ] Security decisions
- [ ] Validation evidence
- [ ] Troubleshooting example
- [ ] Results
- [ ] Lessons learned
- [ ] Future work
- [ ] Attribution

### Done when
- [ ] A hiring manager can understand the project in five minutes
- [ ] Every major claim has evidence

---

## Step 7.2 — Add Evidence

### Checklist
- [ ] Ansible first run
- [ ] Ansible idempotent run
- [ ] Before-and-after Nmap
- [ ] Firewall allowed test
- [ ] Firewall blocked test
- [ ] Firewall log
- [ ] Terraform plan
- [ ] IaC scan before and after
- [ ] Wazuh alerts
- [ ] Vulnerability remediation
- [ ] CI failure and pass
- [ ] Slurm job output
- [ ] Kerberos ticket output

### Done when
- [ ] Screenshots are readable
- [ ] Secrets, IPs, and account details are sanitized

---

## Step 7.3 — Prepare Interview Notes

### Use this format for every topic

**What it is:**  
One sentence.

**Why it matters:**  
One sentence.

**What I built:**  
Two sentences.

**How I validated it:**  
One sentence.

**What went wrong:**  
One sentence.

**What I would improve in production:**  
One sentence.

### Topics
- [ ] Terraform
- [ ] Ansible
- [ ] Linux hardening
- [ ] CIS guidance
- [ ] Network segmentation
- [ ] Firewalls
- [ ] AWS security
- [ ] Vulnerability management
- [ ] Wazuh
- [ ] CI/CD security
- [ ] Kerberos
- [ ] Slurm
- [ ] Lustre
- [ ] MAAS

---

# Troubleshooting Log Template

## Problem
What failed?

## Impact
What could not work because of it?

## First clue
What error or symptom did you see?

## Checks performed
- [ ] Logs checked
- [ ] Connectivity checked
- [ ] Service status checked
- [ ] Configuration checked
- [ ] Permissions checked

## Root cause
What was actually wrong?

## Fix
What did you change?

## Validation
How did you prove the issue was resolved?

## Prevention
What would reduce the chance of this happening again?

---

# Daily Work Card

## Today’s single goal
- [ ] 

## Maximum three tasks
- [ ] 
- [ ] 
- [ ] 

## Evidence to save
- [ ] Screenshot
- [ ] Command output
- [ ] Notes
- [ ] Git commit

## Stop when
- [ ] The tiny goal works
- [ ] Evidence is saved
- [ ] README is updated
- [ ] Changes are committed

## End-of-session note
**What worked:**  

**What broke:**  

**What I learned:**  

**Next tiny step:**  

---

# Definition of Finished

A phase is not finished because the tool installed successfully.

A phase is finished when:

- [ ] It works
- [ ] You tested it
- [ ] You saved evidence
- [ ] You documented what it does
- [ ] You documented one problem
- [ ] You can explain it simply
- [ ] You committed the work
