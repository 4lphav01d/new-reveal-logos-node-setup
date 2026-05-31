# Running a Node

A step-by-step guide for everyone

**Platform-aware** **Beginner-friendly** **No Docker knowledge needed**

Press Space or → to begin

---

## What is Logos?

Logos is a grassroots movement building **decentralized public infrastructure** for communities that want sovereignty over their digital lives.

### 🗣️ Logos Delivery
Messaging layer — private peer-to-peer communication that resists surveillance and censorship. Binary: **wakunode2**.

### 📦 Codex
Storage layer — secure decentralized storage enabling fully decentralized apps and file sharing.

### ⛓️ Nomos
Consensus layer — advanced privacy for a new era of decentralized applications and social institutions.

> "Logos provides the tech stack for communities to run their own infrastructure — no permission needed."

---

## What kind of node can you run?

Three main node types, each serving a different purpose:

### 🖥️ Logos Basecamp
A desktop app (GUI) that bundles everything — messaging, storage, wallet, and blockchain. **Easiest to start with.** Available as DMG (macOS) or AppImage (Linux).

### 💬 Logos Delivery — Messaging Node
A lightweight node for the Waku messaging layer. Runs relay, store, and filter protocols. Available via **Docker Compose**, **prebuilt binary**, or **source build**. Works on Linux, macOS, and (experimentally) Windows.

### ⛓️ Logos Blockchain Node
A headless CLI node that participates in consensus and staking on the Logos testnet. **Linux only** (x86_64 or Raspberry Pi 5). Available as a prebuilt binary.

Choose the one that fits your platform and interest!

---

## Platform Comparison

Which node works on your machine?

| Node Type | Linux | macOS | Windows | Easiest Method |
|-----------|-------|-------|---------|----------------|
| **Basecamp** | ✅ AppImage | ✅ DMG | ❌ | Download binary |
| **Logos Delivery** | ✅ | ✅ | ⚠️ Experimental | Docker Compose |
| **Blockchain** | ✅ | ❌ | ❌ | Binary download |

### Prerequisites common to all
- Stable internet connection
- 2–4 GB RAM (varies by node type)
- Terminal / command line comfort

### Special requirements
- **Blockchain node:** Linux, 64 GB+ storage
- **Logos Delivery:** Linea Sepolia RPC URL (free)
- **Basecamp:** Qt6 (bundled in binary)

---

## Four ways to run a Logos node

Pick the method that matches your comfort level:

1. **Desktop App (Basecamp)** — GUI application — download, open, done. Best for beginners. macOS & Linux.

2. **Docker Compose** — One command to run a messaging node. Best for Linux/macOS. No Docker expertise needed — every command is explained.

3. **Prebuilt Binary** — Download and run a compiled executable. Works for Logos Delivery (Linux/macOS) and Blockchain node (Linux).

4. **Build from Source** — For developers who want to compile from the git repo. Requires Rust, CMake, etc.

Methods 1, 2, and 3 are covered in detail.

---

## Wait — what is Docker?

A quick explainer before we use it:

### 🐳 Docker
Packages software into **containers** — like a lightweight virtual machine that has everything the software needs to run.
- ✅ No need to install dependencies manually
- ✅ Works the same on every OS
- ✅ Easy to start / stop / remove

### 📦 Docker Compose
A helper that runs multiple containers together with a single command. For Logos Delivery, it sets up: the node itself, a Grafana dashboard, and a PostgreSQL database.

> Think of Docker Compose like a "one-click launch" for a whole system.

---

## Method 1: Logos Basecamp

The easiest way to run a Logos node — a desktop application with a graphical interface. Available for **macOS** (DMG) and **Linux** (AppImage).

### Basecamp — Step by Step

1. **Go to the releases page:**
   ```
   https://github.com/logos-co/logos-basecamp/releases/latest
   ```

2. **Download the right file for your OS:**
   - **macOS:** `logos-basecamp-*.dmg`
   - **Linux:** `logos-basecamp-*.AppImage`

3. **Run the downloaded file:**
   - **macOS:** Open the `.dmg` and drag to Applications
   - **Linux:** Make executable and run:
   ```
   chmod +x logos-basecamp-*.AppImage
   ./logos-basecamp-*.AppImage
   ```

💡 Basecamp bundles blockchain, storage, messaging, and wallet in one app.

---

## Method 2: Logos Delivery via Docker Compose

A messaging node (**Waku** protocol) using Docker Compose. Works on **Linux**, **macOS**, and **Windows** (experimental). If you don't have Docker yet, the next slide shows how to install it.

### Prerequisite: Install Docker

If you don't have Docker, install it first:

**Linux**
```
curl -fsSL https://get.docker.com | sh
sudo usermod -aG docker $USER
newgrp docker
```

**macOS**
Download **Docker Desktop** from `docker.com/products/docker-desktop` and install it like any Mac app.

**Windows**
Install **WSL2** from `aka.ms/wsl2`, then install **Docker Desktop**.

Verify with: `docker --version` and `docker compose version`

### Logos Delivery — Step by Step

1. **Clone the compose repository:**
   ```
   git clone https://github.com/waku-org/nwaku-compose
   cd nwaku-compose
   ```
   The repo is still called `nwaku-compose` — this is correct.

2. **Copy the example environment file:**
   ```
   cp .env.example .env
   ```

3. **Edit the `.env` file** — set your RPC endpoint:
   ```
   # Open .env in any text editor
   # Find and set:
   RLN_RELAY_ETH_CLIENT_ADDRESS=https://linea-sepolia.infura.io/v3/YOUR_KEY
   ```
   Get a free Infura key at `infura.io` (Linea Sepolia)

4. **Start the node:**
   ```
   docker compose up -d
   ```
   The `-d` flag runs it in the background.

5. **Check the logs:**
   ```
   docker compose logs -f nwaku
   ```
   Press `Ctrl+C` to stop viewing logs (node keeps running).

### Logos Delivery — Verify & Dashboard

- **📊 Open the Grafana dashboard:** Visit `http://localhost:3000` in your browser. Default login: **admin** / **admin**
- **🔌 Check the REST API:** `curl http://localhost:8645/debug/v1/info | jq .`
- **🛑 Stop the node:** `docker compose down`

---

## Method 3: Logos Delivery via Prebuilt Binary

No Docker required — download and run directly on your system. Works on **Linux** (x86_64) and **macOS** (ARM64/Intel). Windows support is experimental.

### Logos Delivery Binary — Step by Step

1. **Go to the releases page:**
   ```
   https://github.com/waku-org/nwaku/releases
   ```
   Pick the latest stable version (e.g. v0.38.1).

2. **Download the archive for your platform:**
   ```
   # Linux x86_64:
   wget https://github.com/waku-org/nwaku/releases/download/\
   v0.38.1/nwaku-linux-x86_64-v0.38.1.tar.gz

   # macOS ARM64 (Apple Silicon):
   wget https://github.com/waku-org/nwaku/releases/download/\
   v0.38.1/nwaku-darwin-arm64-v0.38.1.tar.gz
   ```

3. **Extract the archive:**
   ```
   tar -xzf nwaku-*-v0.38.1.tar.gz
   cd build/
   ```

4. **Run the node (`wakunode2`):**
   ```
   ./wakunode2 --dns-discovery=true \
     --dns-discovery-url=enrtree://AIRVQ5DDA4FFWLRBCHJWUWOO6X6S4ZTZ5B667LQ6AJU6PEYDLRD5O@sandbox.waku.nodes.status.im \
     --discv5-discovery=true
   ```

💡 Use `./wakunode2 --help` to see all options.

---

## Method 4: Blockchain Node (Binary)

Run a full consensus node on the Logos testnet.

⚠️ **Linux only** (x86_64 or Raspberry Pi 5). Requires **64 GB+ storage** and **glibc 2.39+**.

### Blockchain Node — Step by Step

1. **Download the node binary and ZK circuits:**
   ```
   # Check https://github.com/logos-blockchain/logos-blockchain/releases/latest

   wget https://github.com/logos-blockchain/logos-blockchain/\
   releases/download/0.1.2/\
   logos-blockchain-circuits-v0.4.2-linux-x86_64.tar.gz

   wget https://github.com/logos-blockchain/logos-blockchain/\
   releases/download/0.1.2/\
   logos-blockchain-node-linux-x86_64-0.1.2.tar.gz
   ```

2. **Extract both archives:**
   ```
   tar -xf logos-blockchain-circuits-v0.4.2-linux-x86_64.tar.gz
   tar -xf logos-blockchain-node-linux-x86_64-0.1.2.tar.gz
   ```

3. **Move circuits to default location:**
   ```
   mv logos-blockchain-circuits-v0.4.2/ ~/.logos-blockchain-circuits
   ```

### Blockchain Node — Run & Verify

4. **Generate config with bootstrap peers:**
   ```
   ./logos-blockchain-node init \
     -p /ip4/65.109.51.37/udp/3000/quic-v1 \
     -p /ip4/65.109.51.37/udp/3001/quic-v1 \
     -p /ip4/65.109.51.37/udp/3002/quic-v1 \
     -p /ip4/65.109.51.37/udp/3003/quic-v1
   ```
   Add `--no-public-ip-check` if behind NAT/CGNAT.

5. **Start the node:**
   ```
   ./logos-blockchain-node user_config.yaml
   ```

6. **Check status:**
   ```
   curl -s http://localhost:8080/cryptarchia/info | jq .
   curl -s http://localhost:8080/network/info | jq .
   ```
   Look for mode "Bootstrapping" → "Online". Bootstrap takes 12–24 hours.

### Get Testnet Tokens

To stake on the blockchain node, you need testnet tokens:

1. **Find your wallet keys:**
   ```
   grep -A3 known_keys user_config.yaml
   ```

2. **Visit the faucet:**
   ```
   https://testnet.blockchain.logos.co/web/faucet/
   ```
   Enter one of the hex values from `known_keys`.

3. **Wait ~3.5 hours** for tokens to be eligible for staking.

---

## Tips & Troubleshooting

### 🔒 Port forwarding
If your node isn't reachable, ensure these ports are open: **3000–3003** UDP (blockchain), **60000** TCP, **9000** UDP (messaging), **8080** TCP (API).

### 🐌 Blockchain node is slow to sync
This is normal. Bootstrap takes 12–24 hours. Leave the node running overnight. Check logs with `journalctl` if running as a service.

### 🔄 Docker containers won't start
Run `docker compose logs` to see errors. Common fix: ensure ports aren't already in use (`lsof -i :8645`).

---

## Summary

You have 3 solid options to run a Logos node:

| # | Method | Best for | Effort |
|---|--------|----------|--------|
| 1 | **Basecamp** | Desktop GUI, all-in-one | ⭐⭐ (Easy) |
| 2 | **Logos Delivery (Docker)** | Messaging node, any OS | ⭐⭐⭐ (Medium) |
| 3 | **Logos Delivery (Binary)** | No Docker, lightweight | ⭐⭐⭐ (Medium) |
| 4 | **Blockchain node** | Linux, consensus/staking | ⭐⭐⭐⭐ (Advanced) |

> Start with **Basecamp** for the fastest path to seeing Logos in action. Use **Logos Delivery** to contribute to the messaging layer. Use the **blockchain node** on Linux for consensus participation.

🎉 You're now ready to run a Logos node!

---

## Need Help?

Resources to get you unstuck:

### 📚 Documentation
- Official docs: `docs.logos.co`
- Builder Hub: `build.logos.co`

### 💬 Community
- Discord: `discord.com/invite/logosnetwork`
- GitHub: `github.com/logos-co`

### 🛠️ Developer Support
- Developer office hours every Friday 12:00 UTC
- Logos Community Forum: `forum.logos.co`

> The Logos community is here to help. Don't hesitate to ask!
