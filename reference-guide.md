# Project Setup — Jenkins + Maven + GitHub + Docker

> Note: corrections below reflect what actually worked as of Sept 2026 —
> the original tutorial's key/Java versions were outdated when I followed it.

## Step 1: Jenkins Server Setup on Linux VM

1. Create Ubuntu EC2 instance (t2.medium recommended; c7i-flex.large also works — same 2vCPU/4GB spec, and free-tier eligible on newer AWS accounts)
2. Enable port 8080 in Security Group inbound rules
3. Connect via SSH (`ssh -i key.pem ubuntu@<public-ip>`) — MobaXterm works too, but plain SSH is more transferable

### Install Java
```bash
sudo apt update
sudo apt install openjdk-21-jdk   # NOT 17 — Jenkins LTS requires 21+ as of late 2025
java -version
```

### Install Jenkins
```bash
sudo wget -O /usr/share/keyrings/jenkins-keyring.asc \
  https://pkg.jenkins.io/debian-stable/jenkins.io-2026.key
# ^ key rotated Dec 2025 — always check pkg.jenkins.io for the current year's key
echo "deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc]" \
  https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null
sudo apt update
sudo apt install jenkins -y
```

### Start & verify
```bash
sudo systemctl enable jenkins
sudo systemctl start jenkins
sudo systemctl status jenkins
```

If it fails, don't guess — check the real reason:
```bash
sudo journalctl -u jenkins -n 30 --no-pager
```

### Access Jenkins
