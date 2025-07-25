# cdevops-gitea
k8s gitea lab to take dev (sqlite based) to prod (mysql based)

TLDR;

```bash
pip install ansible kubernetes
git submodule update --init --recursive
ansible-playbook up.yml
```

Wait until `kubectl get pod` shows all pods running and:

```bash
kubectl port-forward svc/gitea-http 3000:3000
```

Now you should be able to access gitea in development mode.

The challenge is to run this in production mode.

# 📦 Gitea Production Setup with Helm, MySQL & ngrok

This project demonstrates deploying Gitea in **production mode** using:
- Helm (for persistence and modular deployment)
- External MySQL database
- Kubernetes via k3d
- Public access through **ngrok tunnel**

---

## 🎯 Assignment Objectives

- ✅ Use Helm to make the Gitea repository data persistent
- ✅ Use an external MySQL database
- ✅ Expose the Gitea instance publicly using ngrok
- ✅ Provide a clear, easy-to-follow README

---

## 🌐 Public Access Link

🔗 **Access Gitea via ngrok:**  
👉 [https://26dc15a26c40.ngrok-free.app] https://26dc15a26c40.ngrok-free.app

---

## 🚀 Deployment Instructions

### 1️⃣ Clone the Repository & Navigate

```bash
git clone <your-repo-url>
cd assignment-3-Container
2️⃣ Install Dependencies (inside Codespace)
bash
Copy
Edit
pip install ansible kubernetes
helm repo add gitea-charts https://dl.gitea.io/charts/
helm repo update
3️⃣ Deploy MySQL
Apply the MySQL deployment and PVC defined in gitea/mysql.yml:

bash
Copy
Edit
kubectl apply -f gitea/mysql.yml
4️⃣ Configure Gitea for Production (via Helm)
Deploy Gitea using Helm with external DB:

bash
Copy
Edit
helm upgrade --install gitea gitea-charts/gitea \
  --namespace default \
  --create-namespace \
  -f gitea/values.yaml
✅ Ensure your gitea/values.yaml contains:

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
5️⃣ Port Forward Gitea
In terminal:

bash
Copy
Edit
kubectl port-forward svc/gitea-http 3000:3000
6️⃣ Expose via ngrok
In a second terminal:

bash
Copy
Edit
ngrok http 3000
You’ll get a URL like:

cpp
Copy
Edit
https://26dc15a26c40.ngrok-free.app
📁 Folder Structure
bash
Copy
Edit
assignment-3-Container/
│
├── gitea/
│   ├── mysql.yml         # MySQL Deployment + PVC
│   ├── values.yaml       # Helm override config
│   ├── up.yml            # Ansible playbook to start
│   ├── down.yml          # Ansible playbook to clean up
│
├── k8s/k3d/
│   ├── up.yml
│   ├── down.yml
│
├── ngrok/
│   └── up.yml            # Optional: for ngrok automation
│
├── README.md             # ← You're here
├── LICENSE
└── .gitignore
📸 Required Screenshots (attached separately)
✅ Gitea public homepage (via ngrok)

✅ kubectl get pods showing MySQL and Gitea

✅ ngrok terminal with public link

✅ Helm install command and confirmation

✅ This README.md showing public URL

🧾 Notes
Developed and tested in GitHub Codespace

ngrok must remain running for external access

All values and manifests can be customized as needed

### Points to Cover

## Marking

|Item|Out Of|
|--|--:|
|use [the gitea helm](https://gitea.com/gitea/helm-gitea) to make the repository data persistent|3|
|make gitea use external database|3|
|Use [this article](https://blog.techiescamp.com/using-ngrok-with-kubernetes/) to expose your gitea instance publically|2|
|make the README easy to use and ACCURATE|2|
|||
|total|10|
