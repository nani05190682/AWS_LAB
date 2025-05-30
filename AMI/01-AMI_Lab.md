# **AWS AMI Creation - Lab Exercise**  
**Objective**: Create a custom Amazon Machine Image (AMI) from an EC2 instance, launch a new instance from the AMI, and verify the configuration.  

## **Scenario**  
You are a Cloud engineer at a company that uses AWS for hosting web applications. The development team has requested a standardized Amazon Linux 2 instance with pre-installed software (Apache Web Server and Python) to ensure consistency across deployments.  

### **Lab Steps**  

#### **Step 1: Launch a Base EC2 Instance**  
1. Log in to the **AWS Management Console** and navigate to **EC2**.  
2. Click **Launch Instance** and select **Amazon Linux 2 AMI** (free tier eligible).  
3. Choose **t2.micro** (free tier) and proceed with default settings.  
4. In **Configure Instance Details**, keep defaults but ensure:  
   - **Auto-assign Public IP**: Enabled  
5. Add **Tags** (optional):  
   - Key: `Name`  
   - Value: `Base-Web-Server`  
6. Configure **Security Group**:  
   - Allow **SSH (22)** and **HTTP (80)** from anywhere (`0.0.0.0/0`).  
7. Review and launch the instance using an existing key pair or create a new one.
   

#### **Step 2: Connect to the Instance and Install Software**  
1. **SSH into the instance**:  
   ```bash
   ssh -i "your-key.pem" ec2-user@<Public-IP>
   ```
2. **Update packages and install Apache & Python**:  
   ```bash
   sudo yum update -y
   sudo yum install httpd python3 -y
   ```
3. **Start Apache and enable auto-start**:  
   ```bash
   sudo systemctl start httpd
   sudo systemctl enable httpd
   ```
4. **Create a simple test page**:  
   ```bash
   echo "<h1>This is a test page from Base AMI</h1>" | sudo tee /var/www/html/index.html
   ```
5. Verify the web server:  
   - Open a browser and visit `http://<Public-IP>` → You should see the test page.  

#### **Step 3: Create a Custom AMI**  
1. Go back to the **EC2 Dashboard** and select the running instance.  
2. Click **Actions** → **Image and templates** → **Create Image**.  
3. Provide details:  
   - **Image name**: `Custom-Web-Server-AMI`  
   - **Description**: "AMI with Apache and Python pre-installed"  
   - Leave other settings as default.  
4. Click **Create Image**.  
5. Navigate to **AMIs** under **Images** to monitor the creation status (may take a few minutes).  

#### **Step 4: Launch an Instance from the Custom AMI**  
1. Once the AMI status is **Available**, select it and click **Launch Instance**.  
2. Choose **t2.micro**, configure security groups (allow SSH & HTTP), and launch.  
3. After the instance is running, verify:  
   - Access the public IP in a browser → You should see the same test page.  
   - SSH into the instance and confirm Apache (`httpd`) is running:  
     ```bash
     sudo systemctl status httpd
     ```

#### **Step 5: Clean Up (Optional)**  
1. Terminate the test instances.  
2. Deregister the custom AMI (if no longer needed).  

### **Expected Outcome**  
✅ Successfully created a custom AMI with pre-installed software.  
✅ Launched a new EC2 instance from the AMI and verified the configuration.  
