# Gensyn Node Guide

## Overview
Gensyn is an L1 blockchain purpose-built for deep learning computation. It allows participants to contribute compute power and earn rewards. This guide walks you through setting up a Gensyn node using a rented GPU server via Vast.ai.

- Website: [gensyn.ai](https://www.gensyn.ai)
- Twitter: [@gensynai](https://x.com/gensynai)
- Docs: [rl-swarm](https://github.com/gensyn-ai/rl-swarm)
- Dashboard: [dashboard.gensyn.ai](https://dashboard.gensyn.ai)

## What’s happening?
The testnet launched a few days ago. During this phase, you can contribute compute power. Nodes can be deployed on CPU servers or GPU servers. If you choose GPU, you’ll earn points significantly faster.

Points are awarded for successfully completed computations. There’s a leaderboard where you can check the number of participants and find yourself.

Leaderboard link — https://dashboard.gensyn.ai/

Clearly, this testnet offers rewards — but it’s also resource-intensive. Renting a GPU server will cost anywhere between $140 to $300 per month.

**Recommended hardware** — as mentioned earlier, the only real limitation is your budget. The better the hardware, the higher your rewards.

**For servers without a GPU:**

- arm64 or x86 CPU with at least 16GB RAM
  
**For GPU servers:**

- RTX 3090
- RTX 4090
- A100
- H100
I went with an RTX 3090. It’s the cheapest, but still good.

---

## Installing PuTTY
1. Download from [official site](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html)
2. Choose the version that fits your system and install it.
3. You’ll get two programs: the terminal (Putty) and PuttyGen.
4. Launch PuttyGen, make sure **RSA** is selected at the bottom, then click **Generate**.
5. Move your mouse around in the blank field to generate a unique key.
6. Once it finishes, your key will appear in the "Key" field. Highlight and copy it.
7. Click Save Private Key, but don’t close the program yet. Remember where you saved the file — you’ll need it later.

---
## Renting the Server

We’ll be renting a server via [vast.ai](https://cloud.vast.ai/?ref_id=124265). The process has a few gotchas, so let’s walk through it step by step.

1. Go to the [site](https://cloud.vast.ai/?ref_id=124265) and create an account. (Yes, that’s a referral link from a bad friend.)
2. Go to the **Account** menu.
3. On the right, click **ADD SSH KEY**, paste the key you generated earlier, and save.
4. Head over to the **Templates** menu and select a preconfigured setup. We’re looking for the one called **Pytorch (Vast).** Click **Select.**
    
    ![image](https://github.com/user-attachments/assets/a0d3935f-5db7-47ba-9517-6be58d448968)

    
5. Now for the fun part — choosing a GPU.
6. Go to the **Search** tab and set the filters at the top: `1x` and the name of the GPU you’re looking for.
    
    ![image](https://github.com/user-attachments/assets/0016c5c1-d064-4c31-b990-ecf97cb719a8)

7. On the left, set disk space to 90GB.
8. When choosing a GPU, make sure the internet speed is at least 300 Mbps.
    
    ![image](https://github.com/user-attachments/assets/b037f166-8726-4a8a-a411-de1972bde7a0)

9. There's also a **Duration** parameter — it shows how long you can rent that specific card.
10. Once you find a suitable GPU, click **Rent**.

### Setting Up the Server for Access

1. Once you’ve rented the server, go to the **Instances** tab.
2. You’ll see a list of your active servers.
3. Open PuttyGen again and copy your SSH key once more.
4. Click the key icon on the right.
    
    ![image](https://github.com/user-attachments/assets/f32841f0-10e6-4f32-9450-8dd221492913)

5. Paste your SSH key and click **ADD SSH KEY**. The window will close, and you’ll see a message saying **SSH KEY ADDED TO INSTANCE**.

---

### Logging into the Server

Let’s walk through using Putty on Windows.

1. Go back to the **Instances** tab on Vast.
    
    ![image](https://github.com/user-attachments/assets/1e6cb476-d26d-4bc9-8c44-95fab4bf9f5a)
    
2. Copy the address in the format `root@185.150.27.254`.
    
    ![image](https://github.com/user-attachments/assets/4ff86c07-a1c6-41d2-9083-6d3328ba2bd7)
    
3. Open Putty and paste the address into the **Host Name** field.
4. Go back to Vast and copy the **port number** listed **before** the IP you just copied.
5. Paste that number into Putty’s **Port** field.
6. In Putty, expand the **SSH** section on the left, then go to **Auth → Credentials**.
7. In the **Private key file** field, select your saved key file.
    
    ![image](https://github.com/user-attachments/assets/bf09d27b-f742-4044-b561-6d29d9f25426)
    
8. Go back to the **Session** tab and click **Save** to store your session settings.
9. Click **Open** — if everything is configured correctly, your server will boot up.
    
    ![image](https://github.com/user-attachments/assets/29f50937-90db-4d1d-8624-27bca13cfeed)
    Good evening :)

---

## Setting Up the Node

> 💡 Forget CTRL + C and CTRL + V - they don't work here.
- Right-click - paste
- Highlighting text - copy from terminal.
  

**1. Clone the repository and navigate into it:**
```sh
git clone https://github.com/gensyn-ai/rl-swarm.git
cd rl-swarm
```

**2. Update packages and install Python dependencies:**
```sh
sudo apt update
sudo apt install -y python3 python3-pip python3-venv git
```

**3. Create and activate a Python virtual environment:**
```sh
python3 -m venv .venv
source .venv/bin/activate
```

**4. Upgrade pip and install Node.js tools:**
```sh
pip install --upgrade pip
sudo apt install -y npm
sudo npm install -g yarn n
sudo n lts
```

**5. Check Node.js version:**
```sh
node -v
```

**6. Run the node setup script:**
```sh
./run_rl_swarm.sh
```

> 💡 You can create an account on Hugging Face and use your own access token. Also you can decline Hugging Face Club prompt and use public access. But in the future, you may face the problem of exceeding the limits.

---

## Logging into the Dashboard and Finalizing Setup

To log in, you’ll need to set up port forwarding. We'll use Putty so you don’t have to save your key in a separate file again.

1. **Close the terminal** by pressing **Ctrl + B**, then **D**. Open a new Putty session.
2. Load the saved session you created earlier.
3. On the left, go to **Connection → SSH → Tunnels**.
4. Enter the following:
   - Source port: `3000`
   - Destination: `localhost:3000`
   - Click `Add`
     ![image](https://github.com/user-attachments/assets/6956b7e0-0442-4d78-88be-9d2a2481211a)
5. Go back to **Session** and click **Save**.
6. Start the terminal by double-clicking the session name or pressing **Open**.
7. You should see logs like: `Waiting for userData.json to be created…`
8. Go to [http://localhost:3000](http://localhost:3000/)
9. You should see a login screen. Sign in using your Google account. The process goes through [Alchemy](https://www.alchemy.com/).
10. Once you’ve logged in, the logs should update and your node should start running.
---

## Finding Yourself on the Leaderboard
1. In your logs, find the name of your node. It should look like this:
```
INFO:hivemind_exp.trainer.hivemind_grpo_trainer:rabid amphibious donkey
```
2. In this example, the node name is rabid amphibious donkey.
3. Go to the [dashboard](https://dashboard.gensyn.ai/) and search for your node by name.
![image](https://github.com/user-attachments/assets/e3962507-dc15-46eb-a5c1-9a0a92fe59d7)
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

### Node Update

1. Make sure you're in the **/rl-swarm** directory
2. Run the command: **git pull**

### Troubleshooting

**Error:**

```jsx
Many Requests for url: https://huggingface.co/Gensyn/Qwen2.5-0.5B-Instruct/resolve/main/sentence_bert_config.json
```

This is a temporary rate limit issue from Hugging Face. It usually lasts for about 1 hour.

**Solution:**

1. Register an account [here](https://huggingface.co/settings/tokens)
2. Create your own access token — it has higher rate limits and should prevent this issue
3. When launching the node and you're prompted about Hugging Face Hub, press `Y` and paste your token
---

## Final Notes
- Renting a GPU server costs $140–$300/month
- Your points depend on hardware quality
- Testnet is live — good opportunity to earn rewards

---
