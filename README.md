# Gensyn Node Guide

## Overview
Gensyn is an L1 blockchain purpose-built for deep learning computation. It allows participants to contribute compute power and earn rewards. This guide walks you through setting up a Gensyn node using a rented GPU server via Vast.ai.

- Website: [gensyn.ai](https://www.gensyn.ai)
- Twitter: [@gensynai](https://x.com/gensynai)
- Docs: [rl-swarm](https://github.com/gensyn-ai/rl-swarm)
- Dashboard: [dashboard.gensyn.ai](https://dashboard.gensyn.ai)

> 💡 You earn points for completed computations. GPU = faster rewards.

---

## Optional: Running Node on Windows PC
If you're not renting a server:
```sh
wsl --install
```
Follow [Microsoftʼs guide](https://learn.microsoft.com/en-us/windows/wsl/install).

---

## Renting a Server (Vast.ai)
1. Go to [Vast.ai](https://cloud.vast.ai/?ref_id=124265) and create an account
2. Add SSH key in **Account** → `ADD SSH KEY`
3. Choose a **Pytorch (Vast)** template
4. Set filters:
   - GPU: 1x RTX 3090 / 4090 / A100 / H100
   - Disk: ≥90GB
   - Internet speed: ≥300 Mbps
5. Click `Rent`

---

## Installing PuTTY
1. Download from [official site](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html)
2. Open `PuttyGen`, click `Generate`, move mouse
3. Save the private key, copy the public key

---

## Setting Up Server Access
1. Go to **Instances** on Vast
2. Add your SSH key to the instance
3. Open PuTTY:
   - Host Name: `root@<server-ip>`
   - Port: `<port-number>`
   - SSH > Auth > Credentials: load your private key file
4. Save the session, then `Open`

---

## Setting Up the Node
```sh
git clone https://github.com/gensyn-ai/rl-swarm.git
cd rl-swarm

sudo apt update
sudo apt install -y python3 python3-pip python3-venv git

python3 -m venv .venv
source .venv/bin/activate

pip install --upgrade pip
sudo apt install -y npm
sudo npm install -g yarn n
sudo n lts
node -v

./run_rl_swarm.sh
```
> 💡 Decline Hugging Face Club prompt.

---

## Accessing the Dashboard
To access the web dashboard locally:
1. Open PuTTY
2. Load your session → SSH > Tunnels
   - Source port: `3000`
   - Destination: `localhost:3000`
   - Click `Add`, then `Save`
3. Start the session, go to:
   - [http://localhost:3000](http://localhost:3000)
4. Log in with Google (Alchemy used under the hood)

> 💡 For multiple nodes, log out of each before switching accounts.

---

## Finding Yourself on the Leaderboard
1. Check logs for your node name:
```
INFO:hivemind_exp.trainer.hivemind_grpo_trainer:rabid amphibious donkey
```
2. Go to [dashboard.gensyn.ai](https://dashboard.gensyn.ai) and search by name

> 💡 Only top 100 nodes are listed.

---

## Useful Commands
```sh
# Detach terminal
Ctrl + B, then D

# New window
Ctrl + B, then C

# Switch between windows
Ctrl + B, then 0-9

# Navigate
cd            # go to root
ls            # list files
pwd           # show current path
```

---

## Final Notes
- Renting a GPU server costs $140–$300/month
- Your points depend on hardware quality
- Testnet is live — good opportunity to earn rewards!

---

Made with 💻 by [Your Name] | Inspired by Moni's Discover
