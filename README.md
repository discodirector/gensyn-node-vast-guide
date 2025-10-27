# Gensyn Node Guide

## Overview
Gensyn is an L1 blockchain purpose-built for deep learning computation. It’s designed to unite the computing power of network participants to accelerate and simplify machine learning tasks — while rewarding contributors for their compute.

The protocol enables the use of **consumer-grade hardware** — even regular PCs and laptops can contribute.

**As for us, we’ll be running a node on a GPU server rented via Vast.**

- Website: [gensyn.ai](https://www.gensyn.ai)
- Twitter: [@gensynai](https://x.com/gensynai)
- Docs: [rl-swarm](https://github.com/gensyn-ai/rl-swarm)
- Dashboard: [dashboard.gensyn.ai](https://dashboard.gensyn.ai)

## What’s happening?
In early April, a testnet was launched where we can provide computing power. Nodes can be deployed on CPU servers or GPU servers. If you choose GPU, you’ll earn points significantly faster.

Points are awarded for successfully completed computations. There’s a leaderboard where you can check the number of participants and find yourself.

Leaderboard link — https://dashboard.gensyn.ai/
<img width="1732" height="697" alt="image" src="https://github.com/user-attachments/assets/fc3d39b8-8820-4c90-8a27-395a7d6720ec" />

It’s obvious that this is a rewarded testnet, but also a very costly one. Renting a GPU server will cost anywhere from $65 to $300 per month.

Usually, such testnets pay off well and bring solid profit. I don’t see much sense in renting an expensive GPU, so I rent cards with 8 to 16 GB of VRAM.

- For 8 GB VRAM — you can run one node with a small model (you will need to change the optimizer).
- For 16 GB VRAM — you can run multiple nodes for different projects. For example, I run Gensyn and FortyTwo on the same machine.

---

## Installing PuTTY (Only for Windows)
1. Download from [official site](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html)
2. Choose the version that fits your system and install it.
3. You’ll get two programs: the terminal (Putty) and PuttyGen.
4. Launch PuttyGen, make sure **RSA** is selected at the bottom, then click **Generate**.
5. Move your mouse around in the blank field to generate a unique key.
6. Once it finishes, your key will appear in the "Key" field. Highlight and copy it.
7. Click Save Private Key, but don’t close the program yet. Remember where you saved the file — you’ll need it later.

---
## Renting the server

We’ll be renting a server via [vast.ai](https://cloud.vast.ai/?ref_id=124265). The process has a few gotchas, so let’s walk through it step by step.

1. Go to the [site](https://cloud.vast.ai/?ref_id=124265) and create an account. (Yes, that’s a referral link from a bad friend.)
2. Go to the **Account** menu.
3. On the right, click **ADD SSH KEY**, paste the key you generated earlier, and save.
4. Head over to the **Templates** menu and select a preconfigured setup. We’re looking for the one called **Pytorch (Vast).** Click **Select.**
    
    ![image](https://github.com/user-attachments/assets/a0d3935f-5db7-47ba-9517-6be58d448968)

    
5. Now for the fun part — choosing a GPU.
6. Go to the **Search** tab and set the filters at the top: `1x` and the name of the GPU you’re looking for.
    
    ![image](https://github.com/user-attachments/assets/0016c5c1-d064-4c31-b990-ecf97cb719a8)

7. On the left, set disk space to **40GB**.
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
sudo npm install -g yarn
sudo npm install -g n
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
**7. Hugging face token and choosing the model**

You’ll see a prompt in the console asking if you want to add your model to the Hugging Face Club — you can skip this step for now. Later, it's recommended to create an account and generate your own token (it's free). You’ll find the instructions below. The token can be added after restarting.

Next, you’ll need to choose a model — just copy and paste one from the list below:

   * `Gensyn/Qwen2.5-0.5B-Instruct`
   * `Qwen/Qwen3-0.6B`
   * `nvidia/AceInstruct-1.5B`
   * `dnotitia/Smoothie-Qwen3-1.7B`
   * `Gensyn/Qwen2.5-1.5B-Instruct`

**How to choose a model?**

* For GPUs with up to 16 GB of memory — choose the first or second model.
* For GPUs with 24 GB or more — go with the third or fifth model.



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
9. You should see a login screen. Sign in using your Google account.
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

---

💡If you want to run multiple nodes: before launching a new node on a different server, **you must log out from the first account**. Otherwise, both nodes will be linked to the same email.
So make it a habit — launch the node, then **log out immediately**.

---

### Want to run Gensyn on a low-VRAM GPU or run with the node from another project?

You need to replace the Adam optimizer with Adafactor in two config files.

Even if you don’t plan to run additional nodes, optimizing VRAM is still a good idea.

---

### Optimizing VRAM usage

First install the MobaXterm terminal to make your life easier. I explained how to do this [here](https://www.notion.so/1c7ec23807128039b6e3c0b621ad6cb9?pvs=21).

After you’ve installed the terminal and connected, go to the server’s file system and find the files at the paths below:

```jsx
/root/rl-swarm/venv/lib/python3.12/site-packages/genrl/trainer/grpo_trainer.py
```

```jsx
/root/rl-swarm/venv/lib/python3.12/site-packages/genrl/trainer/moe/moe_trainer.py
```

Open the first one and find the following line (you can invoke search with Ctrl + F).

```jsx
self.optimizer = torch.optim.Adam
```

Replace this line with the following

```jsx
self.optimizer = torch.optim.Adafactor
```

Now repeat with the second file. After this, VRAM consumption will be significantly reduced.

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

### **Getting the Swarm role**

If you already have a node running and you still haven’t received the role, here’s a detailed guide on how to do it.

- First, create an empty bot in [@BotFather](https://t.me/BotFather). It will issue a token — save it.
- Now go to [this bot](https://t.me/myidbot) and use the `/getid` command to display your Telegram ID. Save it.
- Next, get your account address in the [Gensyn dashboard](https://dashboard.gensyn.ai/). It’s on the right above the table with your nodes and highlighted in green.
- If you bought a node on Nodes Garden, fill out [this form](https://docs.google.com/forms/d/e/1FAIpQLSdByJJEasAsMAiofsHoYNRxn2z-k6DnUOS4d3b5PFiomPFCcQ/viewform) with this data. The service will do everything for you.
- If you set it up yourself, install the software on a rented server and enter your data there.
- Log in to your server. If it’s rented on Vast, you should work through tmux.
- Open a new tmux window by pressing `Ctrl+B`, then `C`.
    
    Enter the command:
    
    ```
    wget -O tg_role.sh https://raw.githubusercontent.com/VaniaHilkovets/GensynFix/main/tg_role.sh && chmod +x tg_role.sh && ./tg_role.sh
    
    ```
    
- After installation, enter your data: the bot token, the Gensyn account address, and the Telegram ID.
- Go to Discord, to the `swarm-link` channel, and enter the `/link-telegram` command.
- You will receive a command like `/verify <CODE>`.
- Send it to your bot in Telegram.
- After that you can check your role by sending a message in the chat.

### Getting the Block role

![image.png](attachment:fa436ac9-768a-4a40-ab12-ea8cc2032bff:image.png)

BlockAssist is an AI bot that learns from you in Minecraft. It starts out inexperienced but gets smarter as you play.

The simplest way to get the role is to use the OctaSpace service. The guys simplified the process to just entering a single Hugging Face token. At the same time it will cost the same as you would spend on Vast.

1. Go to Octa — https://marketplace.octa.space/
2. Connect your wallet, top up the balance with stablecoins USDC, USDT on the BSC or Ethereum networks. You literally need a couple of dollars.
3. Enter Block Assist in the search and select it.
    
    ![image.png](attachment:51522386-70a4-49e8-a092-47d78f37a536:image.png)
    
4. Now you need to choose a machine; focus on the green numbers — the higher they are, the more reliable the server. I decided not to skimp and took a 4090 for 25 cents an hour.
5. Click on the server; it will light up. After that, go back to the top of the page and click **Confige**.
6. You only need to enter HF_TOKEN as in the screenshot below and click Deploy at the top right.
    
    ![image.png](attachment:b7c01532-3f8e-4cfa-a2b0-0776d89efa22:image.png)
    
7. Next, follow this video guide.
    
    https://video.twimg.com/amplify_video/1954676529885036545/vid/avc1/2560x1306/I8TqTO4MhZSA5ZAA.mp4?tag=21
    
8. After you log into the server, you’ll see the desktop. Launch **Gensyn AI Block Assist** — a console window will open. Don’t press anything. A browser will automatically launch; sign in using your email. Two Minecraft windows will open automatically — wait until both reach the main menu.
9. Once both windows are open, return to the console and press **Enter** (you may need to press it a few times). The game will start in both windows.
10. In one window, an AI bot will play; in the other, you’ll be the player.
11. Your task is to build a house. **Blue blocks** mark where blocks should be placed, and **red blocks** mark those that are in the wrong position.
12. Start building — the AI agent will mirror your actions. At first, it will get in the way, but soon it will start helping you.
13. When the construction is finished, the progress bar will be around **98%**.
14. Go back to the console and press **Enter** (you may need to press it a few times).
    
    ![image.png](attachment:a853e71a-2fca-4604-a84c-1eb2e3a73d0d:image.png)
    
16. After loading you can shut down the server, and after that all that’s left is to keep an eye on the Discord for the role opening in the link-for-access channel.
    
    ![image.png](attachment:e8114b64-27ee-459b-98a4-c40da1ac8dab:image.png)

### Node Update

1. Make sure you're in the **/rl-swarm** directory
2. Run the command: **git pull**

### Creating a Backup

We’ll need this in order to restore the node on another machine.

You need to save 1 file:

- `swarm.pem.file`

**File path:**

- `/workspace/rl-swarm/swarm.pem`

To upload or download files, the easiest way is to use a different terminal like **MobaXterm** or **Termius** — they come with a built-in file explorer.

**We’ll be using MobaXterm**

1. Download the terminal from the official website — https://mobaxterm.mobatek.net/
2. Open it and create a new session under the **Sessions** tab.
3. Select **SSH** as the connection type.
4. **Remote host** — your server's IP address or domain.
5. Check **"Specify username"** and enter your login (usually `root`).
6. Find the **"Use private key"** option and select your `.ppk` file.
7. After that, you should be able to connect to your server.

Once connected, you should see all your server’s folders and files on the left panel. Locate the necessary files and right-click to download them.

---

## Final Notes
- Renting a GPU server costs $60–$300/month.
- Your points depend on hardware quality and AI model.
- Testnet is live — good opportunity to earn rewards.

---
