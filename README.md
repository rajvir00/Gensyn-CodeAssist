# Gensyn CodeAssist — Complete Easy Setup & Run Guide

## 🖥️ System Requirements

| Component | Minimum | Recommended |
|------------|----------|--------------|
| **CPU** | 2 cores | 4+ cores |
| **RAM** | 4 GB | 8 GB+ |
| **Storage** | 5 GB free | 10 GB free |
| **Python** | 3.10+ | 3.12 |
| **Docker** | Latest stable | Latest stable |

> 💡 CodeAssist runs entirely locally after setup (no GPU required).  
> For remote/VPS setups, you can access the web UI via SSH tunneling.

---

## STEP 1 — Update and Install Basics
```
sudo apt update && sudo apt install -y python3 python3-pip python3-venv curl git
```

## STEP 2 — Install Docker

```
sudo apt install -y ca-certificates curl gnupg
sudo install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] \
https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | \
sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt update
sudo apt install -y docker-ce docker-ce-cli containerd.io
sudo service docker start
```
### Test Docker:

```
sudo docker run hello-world
```

## STEP 3 — Install uv (Astral Fast Python Runner)

```
curl -LsSf https://astral.sh/uv/install.sh | sh
echo 'export PATH=$HOME/.local/bin:$PATH' >> ~/.bashrc
source ~/.bashrc
```

## STEP 4 — Clone and Run CodeAssist

### Install 
```
git clone https://github.com/gensyn-ai/codeassist.git
cd codeassist
```
### To update, run:
```
git pull
```
### ⚙️Change CodeAssist port 3000 -> 3001

> **Warning:** CodeAssist runs on port **3000** by default if you’re also running **RL-Swarm** , it will conflict. 

### 1. Edit compose.yml
```
nano compose.yml
```

**change "3000:3000" to "3001:3000"** in line 22

![compose](https://github.com/rajvir00/Gensyn-CodeAssist/blob/main/c1.PNG)

### 2. Edit run.py

```
nano run.py
```
**change "3000/tcp": 3000 to "3000/tcp": 3001** in line 302

![runpy](https://github.com/rajvir00/Gensyn-CodeAssist/blob/main/c2.PNG)
<br>



### Run Node
```
uv run run.py
```


<br>
🔑 Note: You’ll need a Hugging Face access token with write permission, which you can generate from your account at https://huggingface.co/settings/tokens
<br>

<br>
When you paste your Hugging Face token into the terminal, the input will be invisible for security reasons - it’s still being entered correctly, so don’t worry or get confused if you don’t see it appear on the screen.
<br><br>


## STEP 5 — Access from Remote (SSH Tunnel)

If CodeAssist runs on a remote server or VPS, you need to forward the ports to your local machine.

```
ssh -f -N -L 3000:localhost:3000 -L 8000:localhost:8000 -L 8001:localhost:8001 -L 8008:localhost:8008 username@your_server_ip
```
Replace username@your_server_ip with your actual SSH login
<br>

## STEP 6 — Access CodeAssist in Your Browser
Once the tunnel is active, open this in your local browser:

```
http://localhost:3001
```

A link will also appear in the terminal click it.
Log in with your email when prompted, and you’re ready to use CodeAssist!






















