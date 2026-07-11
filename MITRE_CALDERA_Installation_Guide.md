# 🛡️ MITRE CALDERA Installation Guide (Ubuntu)

A quick guide to install and run MITRE CALDERA.

## 1. Clone Repository

``` bash
git clone https://github.com/mitre/caldera.git --recursive
cd caldera
```

## 2. Install Dependencies

``` bash
sudo apt update
sudo apt install -y python3-full python3-pip golang-go upx-ucl curl
```

## 3. Create Virtual Environment

``` bash
python3 -m venv venv
source venv/bin/activate
```

## 4. Install Python Packages

``` bash
pip install -r requirements.txt
pip install docker
```

## 5. Install Node.js

``` bash
curl -fsSL https://deb.nodesource.com/setup_20.x | sudo -E bash -
sudo apt install -y nodejs
```

## 6. Start CALDERA

``` bash
python3 server.py --insecure --build
```

Open: http://localhost:8888

Default credentials:

  Username   Password
  ---------- ----------
  red        admin
