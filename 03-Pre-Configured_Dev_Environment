## **Scenario 2: Pre-Configured Dev Environment (Docker & Tools AMI)**  
**Objective**: Create a **custom AMI for developers** with Docker, Git, and common dev tools pre-installed.  

### **Lab Steps**  
1. **Launch an Ubuntu EC2 instance** (t2.medium recommended).  
2. **Install Docker, Git, and dependencies**:  
   ```bash
   sudo apt update -y
   sudo apt install -y docker.io git python3-pip
   sudo systemctl enable docker && sudo systemctl start docker
   sudo usermod -aG docker ubuntu  # Allow non-root Docker usage
   ```  
3. **Install AWS CLI & ECS CLI** (for AWS deployments):  
   ```bash
   curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
   unzip awscliv2.zip && sudo ./aws/install
   sudo curl -o /usr/local/bin/ecs-cli https://amazon-ecs-cli.s3.amazonaws.com/ecs-cli-linux-amd64-latest
   sudo chmod +x /usr/local/bin/ecs-cli
   ```  
4. **Create an AMI** named `Dev-Environment-AMI`.  
5. **Launch a new instance** and verify:  
   - Run `docker --version` → Should work without `sudo`.  
   - Test `git clone` and `aws configure`.  

**Use Case**: Speeds up developer onboarding with a ready-to-use environment.  

