# 🛡️ MITRE CALDERA on Kali Linux

```{=html}
<p align="center">
```
![MITRE](https://img.shields.io/badge/MITRE-CALDERA-red?style=for-the-badge)
![Kali](https://img.shields.io/badge/Kali-Linux-blue?style=for-the-badge)
![Python](https://img.shields.io/badge/Python-3.x-green?style=for-the-badge)

```{=html}
</p>
```
> Professional GitHub README for installing and using MITRE CALDERA on
> Kali Linux.

## 🚀 Installation

``` bash
git clone https://github.com/mitre/caldera.git --recursive
cd caldera
sudo apt update
sudo apt install -y python3-full python3-pip golang-go upx-ucl curl
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt
pip install docker
curl -fsSL https://deb.nodesource.com/setup_20.x | sudo -E bash -
sudo apt install -y nodejs
python3 server.py --insecure --build
```

## 🌐 Login

-   URL: http://localhost:8888
-   Username: `red`
-   Password: `admin`

## 📸 Suggested Screenshots

Add these images under `images/`:

  Screenshot       File
  ---------------- ---------------------------
  Login Page       images/login.png
  Dashboard        images/dashboard.png
  Plugins          images/plugins.png
  Sandcat Agent    images/sandcat.png
  Operation        images/operation.png
  ATT&CK Mapping   images/attack-mapping.png

Example:

``` md
![Dashboard](images/dashboard.png)
```

## 🛰️ Sandcat Agent Deployment

1.  Start CALDERA.
2.  Open **Agents → Sandcat**.
3.  Generate the agent for Windows/Linux.
4.  Execute it on the target.
5.  Verify the agent checks in.

## ⚔️ First Operation

1.  Create an Adversary.
2.  Create an Operation.
3.  Select the Sandcat agent.
4.  Launch.
5.  Review links and facts.

## 🗺️ MITRE ATT&CK Mapping

CALDERA maps executed abilities to ATT&CK tactics such as: - Initial
Access - Execution - Persistence - Discovery - Credential Access -
Lateral Movement - Collection - Exfiltration

## 📁 Repository Layout

``` text
images/
plugins/
conf/
data/
server.py
README.md
```
