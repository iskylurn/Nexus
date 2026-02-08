# Nexus Node Setup

![image](https://framerusercontent.com/images/rj7WYEn8kN0pgryMvb583AzieU.png)

| X        | Minimum              |
|------------------|----------------------------|
| **CPU**          | 4+ |
| **RAM**          | 8+                     |
| **SSD**      | 50+ GB                   |
| **Internet**      | 50+ |
| **Ubuntu**      | Ubuntu 24 |

https://app.nexus.xyz/ - Sign up for the site using the EVM Wallet or Email address in the upper right corner.

-  ## 0. Become Root :

Before we start, let's get full control of the system to avoid "Permission Denied" errors. This command logs you in as the superuser (root)

```bash
sudo -i
```
  
-  ## 1. Install Core Tools : 

```bash
sudo apt update && sudo apt upgrade -y
```
```bash
sudo apt install curl iptables build-essential git -y
```

-  ## 2. Install Rust & Cargo : 

```bash
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
source $HOME/.cargo/env
```

-  ## 3. Grabbing the Source Code :

```bash
git clone https://github.com/nexus-xyz/network-api.git
cd network-api/clients/cli
```
-  ## 4. Custom RAM :
By default, Nexus asks for ~4GB RAM per thread. If you want to run 8 threads, your might crash. Let's fix that.

1. Open the config file:
```bash
nano src/consts.rs
```
2. Find this line: pub const PROJECTED_MEMORY_REQUIREMENT: u64 = 4294967296;

3. Replace 4294967296 with your choice

4. Specify how much RAM each Thread will consume

- For ~2GB per thread: Change the number to 2000000000
- For ~3GB per thread: Change the number to 3000000000

4. Save & Exit: Press CTRL+O, then Enter, then CTRL+X

-  ## ⚠️ Why are we doing this? :
If you don't change this number, your node will likely give an "Out of Memory" error or simply fail to start when you use the --max-threads command. By lowering this value, we "trick" the node into starting with less RAM, allowing our Threads to run smoothly on standard VPS setups.

- ## 5. Build the Beast:
```bash
cargo build --release
```

- ## 5. Start CLI :
```bash
./target/release/nexus-network start --node-id YOUR_ID --max-threads X
```





