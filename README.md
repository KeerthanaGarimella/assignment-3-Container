✅  README.md (Fully Submission-Ready)
markdown
Copy
Edit
# cdevops-gitea

k8s Gitea lab to take dev (SQLite-based) to prod (MySQL-based) setup.

---

## ⚡ TL;DR – Dev Mode Setup (for reference)

```bash
pip install ansible kubernetes
git submodule update --init --recursive
ansible-playbook up.yml
Wait until kubectl get pods shows all pods are running, then:

bash
Copy
Edit
kubectl port-forward svc/gitea-http 3000:3000
Access Gitea at: http://localhost:3000

📦 Gitea Production Setup with Helm, MySQL & ngrok
This project demonstrates deploying Gitea in production mode using:

Helm (for persistence)

External MySQL database

Kubernetes (via k3d)

Public access via ngrok tunnel

🎯 Assignment Objectives
✅ Use Helm to make Gitea repository data persistent

✅ Use an external MySQL database

✅ Expose Gitea publicly via ngrok

✅ Provide a clean, accurate README

🌐 Public Access Link
🔗 Live Gitea Instance:
👉 https://26dc15a26c40.ngrok-free.app

🚀 Production Deployment Instructions
1️⃣ Clone the Repository
bash
Copy
Edit
git clone https://github.com/KeerthanaGarimella/assignment-3-Container
cd assignment-3-Container
2️⃣ Install Dependencies (inside Codespace)
bash
Copy
Edit
pip install ansible kubernetes
helm repo add gitea-charts https://dl.gitea.io/charts/
helm repo update
3️⃣ Deploy MySQL (External DB)
bash
Copy
Edit
kubectl apply -f gitea/mysql.yml
4️⃣ Configure & Deploy Gitea using Helm
bash
Copy
Edit
helm upgrade --install gitea gitea-charts/gitea \
  --namespace default \
  --create-namespace \
  -f gitea/values.yaml
Your gitea/values.yaml must include:

yaml
Copy
Edit
postgresql:
  enabled: false

externalDatabase:
  host: mysql.default.svc.cluster.local
  port: 3306
  user: gitea
  password: gitea123
  database: giteadb

persistence:
  enabled: true
  size: 5Gi
5️⃣ Port Forward the Gitea Service
bash
Copy
Edit
kubectl port-forward svc/gitea-http 3000:3000
6️⃣ Start ngrok to Expose Gitea Publicly
⚠️ Don’t include ngrok setup in prod/up.yml. Use a separate one in ngrok/up.yml.

⬇️ Install ngrok (if not already):
bash
Copy
Edit
curl -s https://ngrok-agent.s3.amazonaws.com/ngrok.asc | sudo tee /etc/apt/trusted.gpg.d/ngrok.asc >/dev/null
echo "deb https://ngrok-agent.s3.amazonaws.com buster main" | sudo tee /etc/apt/sources.list.d/ngrok.list
sudo apt update && sudo apt install ngrok
ngrok config add-authtoken <your-token>
▶️ Start the tunnel:
bash
Copy
Edit
ngrok http 3000
✅ You’ll receive a public link like: https://26dc15a26c40.ngrok-free.app

📁 Folder Structure
swift
Copy
Edit
assignment-3-Container/
├── gitea/
│   ├── mysql.yml
│   ├── values.yaml
│   ├── up.yml
│   ├── down.yml
│
├── k8s/k3d/
│   ├── up.yml
│   ├── down.yml
│
├── ngrok/
│   └── up.yml
│
├── screenshots/
│   ├── gitea-ngrok-browser.png
│   ├── kubectl-get-pods.png
│   ├── ngrok-terminal.png
│   ├── helm-install-output.png
│   └── readme-ngrok-link.png
│
├── README.md
├── LICENSE
└── .gitignore
📸 Required Screenshots (in /screenshots folder)
✅ Gitea public homepage (via ngrok)

✅ kubectl get pods showing MySQL and Gitea

✅ ngrok terminal with forwarding URL

✅ Helm deployment success output

✅ This README.md with ngrok link

🔁 Optional: Start ngrok via Ansible
bash
Copy
Edit
ansible-playbook ngrok/up.yml
ngrok/up.yml contents:

yaml
Copy
Edit
---
- name: Start ngrok tunnel for Gitea
  hosts: localhost
  connection: local
  tasks:
    - name: Run ngrok to expose port 3000
      shell: ngrok http 3000
      async: 3600
      poll: 0
🧾 Notes
Developed and tested inside GitHub Codespaces

Public access is live only while ngrok and port-forward are active

All values and playbooks can be customized