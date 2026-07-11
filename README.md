# 🛡️ MITRE CALDERA Installation Guide (kali linux)

A quick guide to install and run MITRE CALDERA.

# 1. Update the system
sudo apt update

# 2. Install system dependencies
sudo apt install -y git python3-full python3-pip golang-go upx-ucl curl

# 3. Clone the CALDERA repository
git clone https://github.com/mitre/caldera.git --recursive
cd caldera

# 4. Create and activate the Python virtual environment
python3 -m venv venv
source venv/bin/activate

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
  admin        admin

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

## ⭐ Support

If this guide helped you, consider giving the **MITRE CALDERA** repository a ⭐ on GitHub.

Happy Hunting! 🛡️
