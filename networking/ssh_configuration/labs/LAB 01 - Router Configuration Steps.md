For IP addressing details, see [[LAB 01 - Network Topology & Overview]].

Step 1. Interface IP setup

Router> enable
Router# configure terminal
Router(config)# interface GigabitEthernet 0/0/0
Router(config-if)# ip address 192.168.1.1 255.255.255.240
Router(config-if)# no shutdown
Router(config-if)# exit

### Step 2: Hostname & Domain Definition
Set a unique hostname and domain name required for cryptographic key generation

Router(config)# hostname Main
Main(config)# ip domain-name lab.local
### Step 3: RSA Cryptographic Key Generation
Generate a 2048-bit RSA key pair for encryption

Main(config)# crypto key generate rsa
# Select 2048 bits when prompted


### Step 4: Admin Credentials & SSHv2 Enforcement
Create a local administrator with full privileges and set SSH to version 2
Main(config)# username admin privilege 15 secret RouterPass123!
Main(config)# ip ssh version 2


### Step 5: VTY Hardening
Restrict virtual terminal access exclusively to local SSH logins[cite: 1]:

Main(config)# line vty 0 15
Main(config-line)# transport input ssh
Main(config-line)# login local
Main(config-line)# exit
## 4. Verification & Testing

> [!SUCCESS] **Client Connection**
> Open **PC0** > **Desktop** > **TELNET/SSH Client**. Select **SSH**, enter Host IP `192.168.1.1`, Username `admin`, and authenticate using `RouterPass123!`.

Verify active sessions directly on the router CLI

Main# show ip ssh
Main# show ssh

***

**Obsidian & Workflow Tips**

* **Plugin Integration:** Use **Advanced Tables** by typing `|` and using `Tab` to build tables automatically. Draw topologies in **Excalidraw** and embed them with `![[filename.excalidraw]]`.
* **Exporting PDF:** Use **Pandoc** via the Obsidian Command Palette (`Ctrl+P` / `Cmd+P` > *Pandoc: Export to PDF*) or use Obsidian's native `Export to PDF` option.
* **GitHub Repository Setup:** Create an `attachments/` folder for images/Excalidraw renders, keep your notes organized in subfolders by topic (e.g., `Switching/`, `Routing/`), and commit both `.md` files and rendered export files (like `.pdf` or `.png`) so GitHub renders them cleanly.