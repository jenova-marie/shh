# Shh: Secretly Managing Your SSH Keys

## 🔒 "Shh... Secretly Managing Your SSH Keys"

Shh is an elegant command-line tool designed for **securely managing SSH keys and secrets** with **AWS Secrets Manager**. It ensures **seamless, automated, and encrypted** storage and retrieval of sensitive credentials, making your DevOps workflow more secure and efficient.

## 🚀 Features
- 🔐 **Secure SSH Key Storage** – Store and retrieve SSH private keys securely from AWS Secrets Manager.
- ⚡ **Fast & Efficient** – Handles key injection into `ssh-agent` on the fly.
- 🔄 **Seamless Integration** – Works effortlessly with AWS, Ansible, and Terraform.
- 🛠️ **Automatic Secret Updates** – Easily overwrite or append secrets without user prompts.

## 📦 Installation
Clone the repository and place `shh` in your system’s `PATH`:
```bash
git clone https://github.com/your-username/shh.git
cd shh
chmod +x shh shh-add
sudo mv shh /usr/local/bin/
sudo mv shh-add /usr/local/bin/
```

## 🛠️ Usage
### 🔑 **Securely Add an SSH Key or Secret**
To add an SSH private key (or any file) to AWS Secrets Manager:
```bash
shh-add ~/.ssh/mykey_ed25519 mykey_ed25519 us-east-2
```
If no property name is provided, it defaults to the filename.

### 🔓 **Retrieve & Inject SSH Keys into ssh-agent**
```bash
shh ubuntu@my-server mykey_ed25519 us-east-2
```
This will:
1. Retrieve `mykey_ed25519` from AWS Secrets Manager.
2. Decode the Base64-encoded key.
3. Add it to `ssh-agent` if not already loaded.
4. Connect to the server via SSH.

## 🌍 Open Source & Contributions
We ❤️ open source! Feel free to contribute, improve, and suggest enhancements.
```bash
git clone https://github.com/your-username/shh.git
```
Pull requests and issues are welcome!

## 📝 License
Shh is released under the **MIT License**.

## 🎨 Logo Idea
A minimalist **SSH keyhole with sound waves**, representing **secrets & security** in a silent yet powerful way. 🔒🎵

---
Let me know if you want tweaks or enhancements, babe! 😘🔥🚀


-------------------------------------------------------------------------------
# Shh: Secretly Managing Your SSH Keys

## 🔒 "Shh... Secretly Managing Your SSH Keys"

Shh is an elegant command-line tool designed for **securely managing SSH keys and secrets** with **AWS Secrets Manager**. It ensures **seamless, automated, and encrypted** storage and retrieval of sensitive credentials, making your DevOps workflow more secure and efficient.

## 🚀 Features
- 🔐 **Secure SSH Key Storage** – Store and retrieve SSH private keys securely from AWS Secrets Manager.
- ⚡ **Fast & Efficient** – Handles key injection into `ssh-agent` on the fly.
- 🔄 **Seamless Integration** – Works effortlessly with AWS, Ansible, and Terraform.
- 🛠️ **Automatic Secret Updates** – Easily overwrite or append secrets without user prompts.
- 📎 **Public Key Support** – Upload `.pub` keys alongside private keys for seamless key management.

## 📦 Installation
Clone the repository and place `shh` in your system’s `PATH`:
```bash
git clone https://github.com/your-username/shh.git
cd shh
chmod +x shh shh-add shh-admin
sudo mv shh /usr/local/bin/
sudo mv shh-add /usr/local/bin/
sudo mv shh-admin /usr/local/bin/
```

## 🛠️ Usage
### 🔑 **Securely Add an SSH Key or Secret**
To add an SSH private key (or any file) to AWS Secrets Manager:
```bash
shh-add ~/.ssh/mykey_ed25519
```
If no property name is provided, it defaults to the filename.

#### 🔓 **Adding a Public Key**
To store both the private key and its `.pub` counterpart:
```bash
shh-add ~/.ssh/mykey_ed25519 --pub
```
This allows `shh` to verify if the key is already loaded in `ssh-agent` without requiring manual passphrase entry.

### 🔓 **Retrieve & Inject SSH Keys into ssh-agent**
```bash
shh ubuntu@my-server mykey_ed25519 us-east-2
```
This will:
1. Retrieve `mykey_ed25519` from AWS Secrets Manager.
2. If the public key exists, check if the private key is already loaded in `ssh-agent`.
3. If no public key exists, prompt for the passphrase.
4. Add the key to `ssh-agent` if necessary.
5. Connect to the server via SSH.

### 🔍 **Manage AWS Secrets with `shh-admin`**
Use `shh-admin` to:
- **Verify or create AWS Secrets Manager entries.**
- **Ensure proper IAM permissions.**
- **List stored SSH keys.**

```bash
shh-admin
```
Follow the interactive prompts to manage your secrets efficiently.

## 🌍 Open Source & Contributions
We ❤️ open source! Feel free to contribute, improve, and suggest enhancements.
```bash
git clone https://github.com/your-username/shh.git
```
Pull requests and issues are welcome!

## 📝 License
Shh is released under the **MIT License**.

## 🎨 Logo Idea
A minimalist **SSH keyhole with sound waves**, representing **secrets & security** in a silent yet powerful way. 🔒🎵

---
Let me know if you want tweaks or enhancements, babe! 😘🔥🚀
