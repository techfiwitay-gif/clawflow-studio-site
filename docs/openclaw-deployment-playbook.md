# OpenClaw Deployment Playbook (Client-Facing)

## 1) Local Deployment (Fastest Start)
Best for solo founders/operators who want to launch quickly at low cost.

### Step-by-step
1. **Prep machine**
   - Use a stable Windows or Linux machine that stays online during your work hours.
   - Ensure at least 8 GB RAM and 20+ GB free disk.
2. **Install prerequisites**
   - Install Git, Node.js (LTS), and Docker (if required by your workflows).
   - Verify terminal access and admin rights.
3. **Install OpenClaw**
   - Install OpenClaw using official docs flow.
   - Run health/status checks after install.
4. **Set up workspace**
   - Create project folder structure (memory, scripts, docs, backups).
   - Add baseline memory and SOP files.
5. **Connect channels**
   - Connect Telegram/WhatsApp/Discord based on client needs.
   - Validate inbound and outbound messages.
6. **Configure safety**
   - Add approval gates for risky actions.
   - Restrict credentials to least privilege.
7. **Deploy first automation**
   - Implement one high-ROI workflow first.
   - Test with real sample tasks.
8. **Handoff and backup**
   - Provide runbook + quick video walkthrough.
   - Export/backup workspace and memory files.

### Required commands (Windows + Ubuntu/WSL)
```powershell
wsl --install -d Ubuntu
wsl -d Ubuntu
```

```bash
sudo apt update && sudo apt upgrade -y
sudo apt install -y git curl build-essential
curl -fsSL https://deb.nodesource.com/setup_lts.x | sudo -E bash -
sudo apt install -y nodejs
node -v && npm -v && git --version
openclaw status
```

### Success checklist
- OpenClaw responds reliably
- 1 live workflow in production
- Backup and rollback instructions documented

---

## 2) Cloud VPS Deployment (24/7 Reliability)
Best for clients who need always-on uptime.

### Step-by-step
1. **Provision VPS**
   - Choose provider (Hetzner, DigitalOcean, Linode, AWS Lightsail).
   - Start with Ubuntu 22.04 LTS (2 vCPU / 4 GB RAM minimum).
2. **Harden server**
   - Create non-root sudo user.
   - Disable password SSH; enable SSH keys only.
   - Enable firewall (UFW) and fail2ban.
   - Install security updates.
3. **Install runtime stack**
   - Install Git, Node.js LTS, Docker, and OpenClaw dependencies.
   - Confirm versions and system health.
4. **Install OpenClaw**
   - Install in dedicated service directory.
   - Configure environment variables and secrets securely.
5. **Run as service**
   - Set OpenClaw as systemd service for auto-restart.
   - Validate boot persistence.
6. **Attach channels + tools**
   - Connect required messaging channels.
   - Test each integration end-to-end.
7. **Set observability**
   - Enable logs, uptime checks, and alerting.
   - Track key workflow failures.
8. **Backups and DR**
   - Daily backup of workspace/memory/config.
   - Document restore runbook.

### Required commands (Ubuntu VPS)
```bash
sudo apt update && sudo apt upgrade -y
sudo adduser deploy
sudo usermod -aG sudo deploy
sudo apt install -y ufw fail2ban git curl build-essential
sudo ufw allow OpenSSH
sudo ufw enable
sudo systemctl enable --now fail2ban
```

```bash
curl -fsSL https://deb.nodesource.com/setup_lts.x | sudo -E bash -
sudo apt install -y nodejs
node -v && npm -v
openclaw status
openclaw gateway status
```

### Success checklist
- 24/7 reachable service
- Auto-restart after reboot
- Monitoring + backups active

---

## 3) Hybrid Deployment (Security + Reliability)
Best for sensitive workflows that must keep some data local.

### Step-by-step
1. **Split responsibilities**
   - Cloud VPS handles orchestration and general automations.
   - Local host handles sensitive data/tools only.
2. **Design trust boundaries**
   - Define what can leave local environment and what cannot.
   - Document data classification (public/internal/restricted).
3. **Deploy cloud node**
   - Follow VPS deployment steps above.
4. **Deploy local node**
   - Install OpenClaw runtime on local machine.
   - Restrict local node to private-only workflows.
5. **Secure communications**
   - Use secure channels and scoped credentials.
   - Rotate secrets and revoke unused tokens.
6. **Workflow routing**
   - Route sensitive tasks to local node.
   - Route non-sensitive tasks to cloud node.
7. **Validation tests**
   - Confirm sensitive data never leaves local scope.
   - Test failover behavior when one node is offline.
8. **Operational handoff**
   - Provide routing rules + escalation SOP.

### Required commands (both nodes)
```bash
openclaw status
openclaw gateway status
```

```bash
# Cloud node maintenance
sudo apt update && sudo apt upgrade -y

# Local node maintenance
sudo apt update && sudo apt upgrade -y
```

### Success checklist
- Sensitive tasks remain local
- Cloud handles non-sensitive automation continuously
- Clear routing and incident SOPs documented

---

## 4) Managed Team Deployment (Done-for-You Ops)
Best for teams that need governance, multi-user control, and ongoing support.

### Step-by-step
1. **Discovery + architecture workshop**
   - Identify teams, use cases, risk profile, and success KPIs.
2. **Environment design**
   - Separate dev/staging/prod where needed.
   - Define access model by role.
3. **Identity and permissions**
   - Configure role-based access and approval flows.
   - Apply least-privilege policies.
4. **Production deployment**
   - Deploy OpenClaw in managed infrastructure.
   - Configure channels, workflows, and escalation paths.
5. **Compliance controls**
   - Logging, retention, and auditability settings.
   - Incident response and change control process.
6. **Enable monitoring and reporting**
   - Weekly usage, reliability, and issue reports.
   - SLA and response-time targets.
7. **Training + adoption**
   - Team onboarding sessions.
   - SOP documentation for admins and operators.
8. **Continuous optimization**
   - Monthly review of workflows and ROI.
   - Expand automation scope safely.

### Required commands (ops baseline)
```bash
openclaw status
openclaw gateway status
openclaw gateway restart
```

```bash
# Workspace backup example
tar -czf backup-$(date +%F).tar.gz /home/<user>/.openclaw/workspace
```

### Success checklist
- Governance model active
- Team trained and operating independently
- Ongoing optimization cadence established

---

## Recommended default for most clients
- **Start with Cloud VPS** for reliability.
- Use **Hybrid** if sensitive workflows are involved.
- Upgrade to **Managed Team** once multiple users/departments are active.
