# Odoo Docker Deployment Guide (with `.env` Configuration)

This guide helps developers configure environment variables, install Docker, and deploy Odoo using Docker Compose (version 3.9) on Ubuntu 24.04.

---

## 0. Install Docker & Docker Compose (Ubuntu 24.04)

Before running Odoo, you must install Docker and Docker Compose.

### Step 1: Update System

```bash
sudo apt update && sudo apt upgrade -y
```

---

### Step 2: Install Required Packages

```bash
sudo apt install -y ca-certificates curl gnupg
```

---

### Step 3: Add Docker Official GPG Key

```bash
sudo install -m 0755 -d /etc/apt/keyrings

curl -fsSL https://download.docker.com/linux/ubuntu/gpg | \
sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg

sudo chmod a+r /etc/apt/keyrings/docker.gpg
```

---

### Step 4: Add Docker Repository

```bash
echo \
"deb [arch=$(dpkg --print-architecture) \
signed-by=/etc/apt/keyrings/docker.gpg] \
https://download.docker.com/linux/ubuntu \
$(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

---

### Step 5: Install Docker Engine & Compose

```bash
sudo apt update

sudo apt install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

---

### Step 6: Verify Installation

```bash
docker --version
docker compose version
```

---

### Step 7 (Optional): Run Docker Without sudo

```bash
sudo usermod -aG docker $USER
newgrp docker
```

---

## ⚠️ IMPORTANT: Addons Preparation (MUST READ BEFORE RUNNING)

Before starting the containers, you **must prepare your addons directories** correctly.

### ✅ Required Steps

1. **Add Custom Modules**

   ```
   ./odoo-docker/custom-addons/
   ```

2. **Add Enterprise Modules**

   ```
   ./odoo-docker/enterprise/
   ```

3. **Verify Structure**

```bash
odoo-docker/
├── config/
├── custom-addons/
└── enterprise/
```

### ❗ Why This Matters

Odoo uses this addons path:

```
/mnt/custom-addons,/mnt/enterprise,/usr/lib/python3/dist-packages/odoo/addons
```

If folders are empty:

* Modules won't appear
* Enterprise features won't load
* Errors may occur

---

## 1. Prerequisites

* Ubuntu 24.04
* Docker & Docker Compose installed
* Basic knowledge of Odoo

---

## 2. Project Structure

```bash
project-root/
├── docker-compose.yml
├── .env 
├── config/
├── custom-addons/
└── enterprise/
```

---

## 3. Create `.env` File

```bash
# Odoo
ODOO_VERSION=18.0
ODOO_PORT=8069
LONGPOLLING_PORT=8072
ODOO_BASE=./odoo

# PostgreSQL
POSTGRES_VERSION=15
POSTGRES_USER=odoo
POSTGRES_PASSWORD=odoo_secure_password
```

---

## 4. Run the Project

```bash
docker compose up -d
```

---

## 5. Access Odoo

```
http://localhost:8069
```

---

## 6. Useful Commands

```bash
docker compose down
docker compose restart odoo
docker compose logs -f odoo
```

---

## 7. Troubleshooting

* Check logs if Odoo fails
* Ensure DB credentials match
* Verify addons folders are not empty

---

This setup provides a clean, reproducible Odoo development environment using Docker on Ubuntu 24.04.
