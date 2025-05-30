
---

## **Scenario 1: Golden AMI for Compliance (Security Hardened AMI)**  
**Objective**: Create a **security-hardened AMI** with pre-configured compliance settings (e.g., CIS Benchmark) for company-wide use.  

### **Lab Steps**  
1. **Launch a Base EC2 Instance** (Amazon Linux 2 or Ubuntu).  
2. **SSH into the instance** and apply security best practices:  
   ```bash
   # Disable root login via SSH
   sudo sed -i 's/PermitRootLogin yes/PermitRootLogin no/' /etc/ssh/sshd_config
   sudo systemctl restart sshd

   # Install and configure AWS Inspector Agent (optional)
   sudo curl -O https://inspector-agent.amazonaws.com/linux/latest/install
   sudo bash install

   # Enable automatic security updates
   sudo yum install -y yum-cron
   sudo systemctl enable yum-cron && sudo systemctl start yum-cron
   ```  
3. **Create an AMI** named `Hardened-Linux-AMI-CIS`.  
4. **Launch a new instance** from this AMI and verify:  
   - Try SSH as `root` → Should fail.  
   - Check `/var/log/secure` for login attempts.  

**Use Case**: Ensures all company instances meet security policies.  

