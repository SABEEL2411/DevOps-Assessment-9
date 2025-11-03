# DevOps-Assessment-9
---
#  Flask + Express Deployment on AWS EC2 (Beginner Friendly)

This guide explains how to deploy a **Flask backend** and an **Express.js frontend** on an **AWS EC2 Ubuntu instance** — step by step, with all commands included.

---

##  Tools & Technologies Used

| Category | Tool / Version | Purpose |
|-----------|----------------|----------|
| Cloud | AWS EC2 (Ubuntu 22.04) | Hosting environment |
| Backend | Python 3 + Flask | REST API backend |
| Frontend | Node.js v20 + Express.js | Web server frontend |
| Version Control | Git / GitHub | Code management |
| Network | AWS Security Group | Opening ports (22, 5000, 3000) |

---

##  1. Launch EC2 Instance

1. Log in to AWS Console → **EC2 → Launch Instance**
2. Choose **Ubuntu 22.04 LTS (Free Tier)**
3. Instance type: `t2.micro`
4. Create key pair → download `flask.pem`
5. Configure security group:
   - Port **22** → SSH  
   - Port **5000** → Flask backend  
   - Port **3000** → Express frontend  
6. Click **Launch**

---

##  2. Connect to Your EC2 Instance

```bash
chmod 400 flask.pem
ssh -i flask.pem ubuntu@<your-ec2-public-ip>
```
---
##  3. Setup Flask Backend
- Step 1: Install Python and Flask
sudo apt update -y
sudo apt install python3 python3-pip -y
pip install flask
---
- Step 2: Create Flask App
mkdir flask-backend
cd flask-backend
---

- Step 3: Run Flask App
python3 app.py
---
## Test:

Visit in browser →
http://<your-ec2-public-ip>:5000
---
## 4. Setup Express Frontend
---
- Step 1: Install Node.js 20 and npm
sudo apt --fix-broken install -y
sudo apt purge -y nodejs npm
sudo rm -rf /etc/apt/sources.list.d/nodesource.list
curl -fsSL https://deb.nodesource.com/setup_20.x | sudo -E bash -
sudo apt install -y nodejs
node -v
npm -v
---
- Step 2: Create Express App
cd ~
mkdir express-frontend
cd express-frontend
npm init -y
npm install express
---

- Step 3: Run Express App
node app.js
---
## Test:

Visit →
http://<your-ec2-public-ip>:3000
---
## 5. Managing Running Servers
---
Check what’s using a port
sudo lsof -i :5000
sudo lsof -i :3000
---
Kill processes using those ports
sudo fuser -k 5000/tcp
sudo fuser -k 3000/tcp
---
Verify they stopped
sudo lsof -i :5000
sudo lsof -i :3000
---
## 6. Run Both Servers in Background
---
nohup python3 ~/flask-backend/app.py &
nohup node ~/express-frontend/app.js &
---
Check if running
ps aux | grep app
---
## 7. Verify in Browser
---
Service	URL
Flask Backend	http://<EC2-Public-IP>:5000
Express Frontend	http://<EC2-Public-IP>:3000
---
## 8. Common Troubleshooting
---
Issue	Fix
Address already in use	Run sudo fuser -k 5000/tcp
Permission denied (flask.pem)	Run chmod 400 flask.pem
npm not found	Reinstall Node.js using NodeSource setup
Flask not reachable	Ensure AWS Security Group allows port 5000 inbound
---
## 9. Helpful Commands Summary
---
### Kill all Flask & Express processes
sudo fuser -k 5000/tcp
sudo fuser -k 3000/tcp
---
### Restart Flask
cd ~/flask-backend
nohup python3 app.py &
---
## Restart Express
cd ~/express-frontend
nohup node app.js &
---
### View running processes
ps aux | grep app
---
===
