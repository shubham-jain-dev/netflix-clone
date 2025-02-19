# 🎬 Netflix Clone  

A fully functional Netflix clone built with **HTML5, CSS3, JavaScript** and deployed on **AWS EC2** with **GitHub Actions Workflow** for **CI/CD** automation.  

## 🚀 Tech Stack  
- **Frontend:** HTML5, CSS3, JavaScript  
- **Backend:** AWS Services  
- **Deployment:** AWS EC2, GitHub Actions Workflow (CI/CD)  

## 📚 Project Structure  
```
/netflix-clone  
  ├── frontend/  
  ├── backend/  
  ├── .github/workflows/ (GitHub Actions)  
  ├── package.json  
  ├── README.md  
```  

## 🚀 Installation & Setup  

### 1️⃣ Clone the Repository  
```bash
git clone https://github.com/shubham-jain-dev/netflix-clone.git  
cd netflix-clone
```  

### 2️⃣ Install Dependencies  
```bash
cd frontend && npm install  
cd ../backend && npm install  
```  

### 3️⃣ Start the Development Server  
```bash
cd backend && npm start  
cd ../frontend && npm start  
```  

---  

## ☁️ Deployment on AWS EC2 with GitHub Actions (CI/CD)  

### **Step 1: Launch an EC2 Instance & Download the Key Pair**  
- Go to **AWS EC2 Dashboard** and launch an **Ubuntu** instance.  
- Allow **SSH (port 22), HTTP (port 80), and HTTPS (port 443)** in Security Groups.  
- Download the **.pem** key pair for SSH access.  

### **Step 2: Create GitHub Secrets**  
Add the following **secrets** in your repository under **Settings → Secrets and Variables → Actions → New Repository Secret**:  

| Secret Name       | Description |
|------------------|-------------|
| `EC2_SSH_KEY`   | Paste your **private key** from the `.pem` file. |
| `HOST_DNS`      | Public **DNS** of your EC2 instance (e.g., `ec2-xx-xxx-xxx-xxx.compute.amazonaws.com`). |
| `USERNAME`      | Default is `ubuntu` for Ubuntu instances. |
| `TARGET_DIR`    | The path where your code will be deployed (e.g., `/home/ubuntu/netflix-clone`). |

### **Step 3: Create GitHub Actions Workflow**  
Create a **workflow file** in your repository:  

📂 **`.github/workflows/github-actions-ec2.yml`**  

```yaml
name: Push-to-EC2

# Run workflow when pushing to main branch
on:
  push:
    branches:
      - main

jobs:
  deploy:
    name: Deploy to EC2 on main branch push
    runs-on: ubuntu-latest

    steps:
      - name: Checkout the repository
        uses: actions/checkout@v2

      - name: Deploy to EC2
        uses: easingthemes/ssh-deploy@main
        env:
          SSH_PRIVATE_KEY: ${{ secrets.EC2_SSH_KEY }}
          REMOTE_HOST: ${{ secrets.HOST_DNS }}
          REMOTE_USER: ${{ secrets.USERNAME }}
          TARGET: ${{ secrets.TARGET_DIR }}

      - name: Run commands on EC2
        uses: appleboy/ssh-action@master
        with:
          host: ${{ secrets.HOST_DNS }}
          username: ${{ secrets.USERNAME }}
          key: ${{ secrets.EC2_SSH_KEY }}
          script: |
            sudo apt-get -y update
            sudo apt-get install -y apache2
            sudo systemctl start apache2
            sudo systemctl enable apache2
            cd home
            sudo mv * /var/www/html
```

### **Step 4: Push Changes & Trigger Deployment**  
Every time you **push to the `main` branch**, the workflow will:  
1. **Connect to EC2** using SSH.  
2. **Sync files** to the target directory.  
3. **Run commands** (install dependencies, restart the server).  

If you want to manually trigger deployment, go to **GitHub → Actions → Select Workflow → Run workflow**.  

---  

## 🚀 Updating the Code  
To deploy updates:  
```bash
git add .
git commit -m "Updated feature"
git push origin main
```
GitHub Actions will **automatically deploy** the changes to EC2.  

---  

## 🤝 Contributing  
Feel free to fork this repo and create pull requests!  

---

