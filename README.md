# Nexus CLI

![image](https://framerusercontent.com/images/rj7WYEn8kN0pgryMvb583AzieU.png)

| X        | Minimum              |
|------------------|----------------------------|
| **CPU**          | 4+ |
| **RAM**          | 8+                     |
| **SSD**      | 50+ GB                   |
| **Internet**      | 50+ |
| **Ubuntu**      | Ubuntu 24 |

https://app.nexus.xyz/ - Sign up for the site using the EVM Wallet or Email address in the upper right corner.

## Become Root

Before we start, let's get full control of the system to avoid "Permission Denied" errors. This command logs you in as the superuser (root)

```bash
sudo -i
```
  
## 1. Install Core Tools

```bash
sudo apt update && sudo apt upgrade -y
```
```bash
sudo apt install curl iptables build-essential git -y
```

## 2. Install Rust & Cargo

```bash
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
source $HOME/.cargo/env
```

## 3. Grabbing the Source Code

```bash
git clone https://github.com/nexus-xyz/network-api.git
cd network-api/clients/cli
```
## 4. Custom RAM & (The "Thread vs RAM" Math)
Example you might have 32GB RAM and wonder: "Why can't i run 8 Threads?" The answer is the default
Reservation
- Default: 1 Thread 4.29 GB = 8 Threads 34.32 GB
- Custom: 1 Thread 2 GB = 8 Threads 16 GB

| Thread Count | Default (4GB/th) | Optimized (2GB/th) |
| :--- | :--- | :--- |
| 4 Threads | 17.1 GB RAM ❌ | 8 GB RAM ✅ |
| 8 Threads | 34.3 GB RAM ❌ | 16 GB RAM ✅ |

1. Open the config file:
```bash
nano src/consts.rs
```
2. Find this line: pub const: PROJECTED_MEMORY_REQUIREMENT: u64 = 4294967296;

3. Replace 4294967296 with your choice

- To use ~2GB per Thread: Change it to 2000000000
- To use ~3GB per Thread: Change it to 3000000000

5. Save & Exit: Press CTRL+O, then Enter, then CTRL+X

## 5. Build the Beast
```bash
cargo build --release
```

## 6. Start CLI :
```bash
./target/release/nexus-network start --node-id YOUR_ID --max-threads X
```





