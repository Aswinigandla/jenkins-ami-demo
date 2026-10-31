Jenkins + AWS EC2 demo
======================

Files
-----
- index.html: a simple static website.
- Jenkinsfile: a Jenkins Declarative Pipeline for checkout, basic validation,
  and deployment to an Amazon Linux EC2 instance running Nginx.

Before running
--------------
1. Launch an Amazon Linux 2023 EC2 instance.
2. Install and start Nginx on the instance:

   sudo dnf install nginx -y
   sudo systemctl enable --now nginx

3. In the EC2 security group, allow:
   - SSH (TCP 22) from your Jenkins machine's public IP.
   - HTTP (TCP 80) from the users who need to view the website.

4. In Jenkins, create a credential:
   - Kind: SSH Username with private key
   - ID: ec2-deploy-key
   - Username: ec2-user
   - Private key: the contents of your EC2 .pem private key

5. Edit Jenkinsfile and replace YOUR-EC2-PUBLIC-IP with the EC2 public IPv4.
6. Commit both files to the root of your GitHub repository.
7. Create a Jenkins Pipeline job using "Pipeline script from SCM":
   - SCM: Git
   - Repository URL: your GitHub repository URL
   - Branch: */main
   - Script Path: Jenkinsfile

Important
---------
- This Jenkinsfile is intended for Jenkins running on Windows with PowerShell
  and the Windows OpenSSH Client (ssh.exe and scp.exe) installed.
- It assumes a public GitHub repository. For a private repository, configure
  Git credentials in Jenkins.
- The EC2 instance must be reachable from the Windows machine running Jenkins.
- StrictHostKeyChecking=no is used only to simplify this beginner lab. For
  production, verify and manage the EC2 SSH host key securely.
- Never commit your .pem private key or passwords to GitHub.
- AWS resources may incur charges; stop or delete resources when not needed.
