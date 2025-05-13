# Shh: Secretly Managing Your SSH Keys

Shh is an elegant command-line tool designed for **securely managing SSH keys and secrets** with **AWS Secrets Manager**. It ensures **seamless, automated, and encrypted** storage and retrieval of sensitive credentials, making your DevOps workflow more secure and efficient.

## 🚀 Features
- 🔐 **Secure SSH Key Storage** – Store and retrieve SSH private keys securely from AWS Secrets Manager.
- ⚡ **Fast & Efficient** – Handles key injection into `ssh-agent` on the fly without writing to disk.
- 🔄 **Seamless Integration** – Works effortlessly with AWS, Ansible, and Terraform.
- 🔍 **Advanced Metadata** – Tracks key details, versions, and automatic rotation schedules.
- 🔁 **Key Rotation** – Monitors key age and suggests rotation timeframes for enhanced security.
- 📎 **Public Key Support** – Upload `.pub` keys alongside private keys for seamless key management.
- 🌍 **Region Flexibility** – Configure AWS regions via CLI arguments or environment variables.

## 📦 Installation

### Prerequisites
- AWS CLI installed and configured with appropriate permissions
- `jq` for JSON processing
- `ssh-agent` running on your system

### Basic Installation
Clone the repository and place the scripts in your system's `PATH`:
```bash
git clone https://github.com/your-username/shh.git
cd shh
chmod +x shh shh-add shh-admin
sudo mv shh /usr/local/bin/
sudo mv shh-add /usr/local/bin/
sudo mv shh-admin /usr/local/bin/
```

### Environment Configuration
You can configure Shh with environment variables:
```bash
# Regional configuration (in order of precedence)
export SHH_REGION="us-east-2"  # Preferred region
export AWS_REGION="us-east-2"  # Alternative
export AWS_DEFAULT_REGION="us-east-2"  # AWS CLI default

# Secret name configuration
export SHH_SECRETS="my-ssh-keys"  # Name of your AWS Secrets Manager secret
```

## 🛠️ Usage

### 🔑 **Securely Add an SSH Key to AWS Secrets Manager**
The `shh-add` tool stores your SSH keys in AWS Secrets Manager with rich metadata:

```bash
# Basic usage - adds key to AWS Secrets Manager
shh-add ~/.ssh/mykey_ed25519 [property-name] [region]

# Add both private and public keys
shh-add ~/.ssh/mykey_ed25519 --pub

# Only upload the specified file, skip public key and fingerprint
shh-add ~/.ssh/mykey_ed25519 --only

# Add key to both AWS Secrets Manager and your local SSH agent
shh-add ~/.ssh/mykey_ed25519 --pub

# Skip adding to SSH agent
shh-add ~/.ssh/mykey_ed25519 --no-ssh-add

# Enable verbose debug output
shh-add ~/.ssh/mykey_ed25519 --debug
```

#### Key Metadata Features
Each key stored by `shh-add` automatically includes metadata with:
- Key type (ed25519, RSA, etc.) and size (bits)
- Creation and update timestamps
- Version tracking for key rotation history
- SSH key fingerprint for agent identification
- Recommended rotation date (90 days from upload)
- Comments from the original key

### 🔓 **Use SSH Keys with `shh`**
The `shh` command retrieves keys from AWS Secrets Manager and uses them with SSH:

```bash
# Basic syntax
shh user@hostname key-name [region] [options]

# Example: Connect to a server using a specific key
shh ubuntu@my-server mykey_ed25519 us-east-2

# Enable debug output
shh ubuntu@my-server mykey_ed25519 --debug
```

The `shh` command performs the following steps:
1. Securely retrieves the key from AWS Secrets Manager
2. Identifies key fingerprint from metadata
3. Checks if the key is already loaded in `ssh-agent`
4. Adds the key to `ssh-agent` in memory (no disk writes) if needed
5. Connects to the specified server

### 🔍 **Manage Keys with `shh-admin`**
The `shh-admin` tool helps you manage your secrets in AWS Secrets Manager:

```bash
# Interactive mode
shh-admin

# List all keys in the specified secret
shh-admin --list
shh-admin -l

# Specify AWS region
shh-admin --region us-east-2
shh-admin -r us-east-2

# Specify secret name
shh-admin --secret my-ssh-keys
shh-admin -s my-ssh-keys

# Show detailed information about a specific key
shh-admin --detail mykey_ed25519
shh-admin -d mykey_ed25519

# Enable debug output
shh-admin --debug
```

#### Key Management Features
- Displays key metadata including type, size, and rotation schedule
- Shows key version history and update timestamps
- Views public key contents when available
- Creates new AWS Secrets Manager secrets if they don't exist
- Verifies appropriate AWS IAM permissions

## 🔄 Key Rotation Best Practices
Shh includes key rotation features:
- Each key automatically has a recommended rotation date (90 days after creation)
- The `shh-admin --list` command shows when each key is due for rotation
- When a key is updated, the rotation history is preserved
- Version numbers are incremented automatically on each update

To rotate a key:
1. Generate a new SSH key: `ssh-keygen -t ed25519 -f ~/.ssh/new_key`
2. Add it to AWS Secrets Manager: `shh-add ~/.ssh/new_key mykey_ed25519 --pub`
3. The previous key data is preserved in the rotation history

## 🌍 AWS Region Configuration

Region priority (from highest to lowest):
1. Command-line argument (e.g., `shh-add ~/.ssh/mykey_ed25519 mykey us-east-2`)
2. `SHH_REGION` environment variable
3. `AWS_REGION` environment variable
4. `AWS_DEFAULT_REGION` environment variable
5. Default fallback (us-east-2)

## 🔧 Debug Options
All three commands support a `--debug` flag for troubleshooting:
```bash
shh user@host keyname --debug
shh-add ~/.ssh/mykey --debug
shh-admin --debug
```

## 🛡️ Security Considerations
- No SSH keys are ever written to disk during retrieval
- Keys are securely transmitted from AWS Secrets Manager to SSH agent in memory
- All AWS connections use your authenticated AWS CLI credentials
- Key fingerprints are stored to verify agent-loaded keys without requiring passphrase entry

## 🌍 Open Source & Contributions
We welcome contributions, improvements, and suggestions for enhancements.
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
