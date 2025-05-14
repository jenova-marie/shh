# Shh: Secretly Managing Your SSH Keys

Shh is an elegant command-line toolkit designed for **securely managing SSH keys and secrets** with **AWS Secrets Manager**. It ensures **seamless, automated, and encrypted** storage and retrieval of sensitive credentials, making your DevOps workflow more secure and efficient.

## 💫 The Shh Revolution: One Key to Rule Them All

Shh fundamentally transforms how you manage SSH keys by **eliminating the need to store multiple SSH keys on your local system**. The core workflow is beautifully simple:

1. 🔑 **Create a key** - Generate an SSH key pair for a specific server or purpose
2. 🚀 **Upload to AWS** - Add the key to AWS Secrets Manager with `shh-add`
3. 🔥 **Shred locally** - Securely delete the key from your local system
4. 🌐 **Connect anytime** - Use `shh` to connect without having the key on your system

This revolutionary approach means:

- 🛡️ **Enhanced Security**: Your system is no longer a target for SSH key theft - keys exist only in AWS Secrets Manager and temporarily in memory during connections
- 🧠 **Zero Mental Overhead**: No more remembering which key is for which server or keeping track of key locations 
- 🔄 **Centralized Management**: Manage all your keys in one secure location with proper access controls
- 🤝 **Team Collaboration**: Share access to systems without sharing actual key files
- 🚫 **No More Key Sprawl**: Stop the endless multiplication of SSH keys across your system

The true power of Shh is that **you only need your personal AWS authentication** - all your server access keys remain securely stored in the cloud until the moment they're needed, then they're loaded directly into memory without touching disk.

## 💫 Project Philosophy

At its heart, Shh aims to solve a critical DevOps security challenge: **how to handle SSH keys securely across teams and environments**. 

By leveraging AWS Secrets Manager, Shh eliminates dangerous practices like:
- ❌ Storing unencrypted SSH keys in repositories
- ❌ Sharing keys through insecure channels like email or chat
- ❌ Managing keys without version control or rotation policies
- ❌ Writing sensitive credentials to disk during deployment

Instead, Shh provides:
- ✅ Zero disk writes for sensitive operations
- ✅ Beautiful and intuitive terminal UI
- ✅ Seamless integration with existing tools
- ✅ Intelligent key rotation and versioning
- ✅ Consistent security best practices

## 🚀 Features
- 🔐 **Secure SSH Key Storage** – Store and retrieve SSH private keys securely from AWS Secrets Manager.
- ⚡ **Fast & Efficient** – Handles key injection into `ssh-agent` on the fly without writing to disk.
- 🔄 **Seamless Integration** – Works effortlessly with AWS, Ansible, and Terraform.
- 🔍 **Advanced Metadata** – Tracks key details, versions, and automatic rotation schedules.
- 🔁 **Key Rotation** – Monitors key age and suggests rotation timeframes for enhanced security.
- 📎 **Public Key Support** – Upload `.pub` keys alongside private keys for seamless key management.
- 🌍 **Region Flexibility** – Configure AWS regions via CLI arguments or environment variables.
- ⚙️ **Environment Management** – Easily configure, persist, and manage Shh environment variables.
- 🖥️ **Beautiful UI** – Intuitive and visually appealing terminal interface with color-coding.
- 🔧 **Automation Support** – Fully scriptable for CI/CD pipelines and automated deployments.
- 🔄 **Self-Updating** – Easy in-place updates that keep your installation current with the latest features.

## 📦 Installation

### Prerequisites
- AWS CLI installed and configured with appropriate permissions
- `jq` for JSON processing (version 1.5+)
- `ssh-agent` running on your system
- `git` for cloning the repository
- Bash shell environment (version 4.0+)

### Quick Installation
The easiest way to install Shh is using our installation script:

```bash
# Basic installation with interactive prompts
curl -fsSL https://raw.githubusercontent.com/jenova-marie/shh/root/shh-install | bash

# Fully automated installation with HTTPS (recommended for CI/CD)
curl -fsSL https://raw.githubusercontent.com/jenova-marie/shh/root/shh-install | bash -s -- --https --auto

# Specify SSH as clone method (if you have GitHub SSH keys configured)
curl -fsSL https://raw.githubusercontent.com/jenova-marie/shh/root/shh-install | bash -s -- --ssh
```

This will:
1. Download and execute the installer script directly
2. Install all Shh components
3. Create symlinks in `/usr/local/bin`
4. Set proper permissions
5. Log all installation activities to `/var/log/shh.log`
6. Guide you through initial configuration

### Installation Options

The installer supports several options to customize the installation process:

| Option | Description |
|--------|-------------|
| `--ssh` | Use SSH for cloning the repository (requires GitHub SSH setup) |
| `--https` | Use HTTPS for cloning the repository (more reliable for CI/CD) |
| `--auto` | Fully automated installation with minimal prompts (uses defaults) |
| `install` | Explicitly specify installation mode (default if not specified) |
| `--help` | Show usage information and all available options |

Examples:
```bash
# Combine options for customized installation
curl -fsSL https://raw.githubusercontent.com/jenova-marie/shh/root/shh-install | bash -s -- install --https --auto
```

### Manual Installation
If you'd like to review the installer before running it (recommended):

```bash
# Download installation script ONLY (does NOT execute)
curl -fsSL https://raw.githubusercontent.com/jenova-marie/shh/root/shh-install -o shh-install

# Make it executable
chmod +x shh-install

# Run the installer (can add options here too)
./shh-install

# Example with options
./shh-install --https --auto
```

### Uninstallation
To remove Shh from your system:

```bash
# Interactive uninstallation (with prompts for confirmation)
curl -fsSL https://raw.githubusercontent.com/jenova-marie/shh/root/shh-install | bash -s -- uninstall

# Fully automated uninstallation (no prompts)
curl -fsSL https://raw.githubusercontent.com/jenova-marie/shh/root/shh-install | bash -s -- uninstall --auto

# If you have the script locally
./shh-install uninstall
./shh-install uninstall --auto  # Non-interactive mode
```

The uninstallation process will:
- Remove all symlinks from `/usr/local/bin`
- Delete the installation directory at `/usr/local/share/shh`
- Offer to clean up environment variables from your shell configuration files
- Preserve the log file at `/var/log/shh.log` for reference

## ⚙️ Configuration

### Environment Configuration
You can configure Shh with environment variables:

```bash
# Regional configuration (in order of precedence)
export SHH_REGION="us-east-2"  # Preferred region
export AWS_REGION="us-east-2"  # Alternative
export AWS_DEFAULT_REGION="us-east-2"  # AWS CLI default

# Secret name configuration
export SHH_SECRETS="my-ssh-keys"  # Name of your AWS Secrets Manager secret

# Debug configuration
export SHH_DEBUG="true"  # Enable debug mode
```

### Interactive Configuration (shh-env)
The `shh-env` tool provides a beautiful interactive interface for managing environment variables:

```bash
# Launch interactive menu
shh-env

# Display current environment configuration
shh-env --display

# Set environment variables for current session
shh-env --set SHH_REGION=us-west-2
shh-env --set SHH_SECRETS=prod-ssh-keys

# Reset all SHH environment variables to defaults
shh-env --reset

# Persist environment variables to your shell config
shh-env --persist

# Enable debug output
shh-env --debug
```

### AWS Secret Configuration (shh-admin)
The `shh-admin` tool helps create and manage your AWS Secrets Manager secret:

```bash
# Launch interactive mode
shh-admin

# Create or verify your AWS secret (non-interactive)
shh-admin --create

# Configure environment variables only
shh-admin --env

# List keys in your secret
shh-admin --list

# Update Shh to the latest version
shh-admin --update

# All options combined
shh-admin --region us-west-2 --secret prod-keys --list --debug
```

## 🛠️ Usage

### 🔄 **Drop-in SSH Replacement**

Shh is designed to be a **complete drop-in replacement** for the traditional SSH command. It supports **all standard SSH options and parameters**, passing them directly to the underlying SSH command after handling key retrieval.

```bash
# Use shh exactly like you would use ssh, with all the same parameters
shh -p 2222 -X -o ConnectTimeout=10 user@hostname

# Specify a custom key using -i (gets key from AWS Secrets Manager, not the filesystem)
shh -i myserver_key -p 2222 user@hostname

# Use with tools that expect ssh like scp or rsync
alias scp="scp -S shh"
rsync -e "shh" local/file user@remote:/path
```

All SSH flags and options work exactly as expected:
- Port specification (`-p`)
- X11 forwarding (`-X`, `-Y`)
- SSH options (`-o`)
- Command execution (`shh user@host command`)
- And any other standard SSH flag

You can seamlessly replace `ssh` with `shh` in your scripts, aliases, and workflows!

### 🔑 **Secure Key Generation and Destruction**

For maximum security, follow these best practices when creating and destroying SSH keys before storing them in AWS Secrets Manager.

#### Using Built-in Key Generation

Shh now includes built-in key generation capabilities, securely creating and uploading keys in a single step:

```bash
# Generate a key with interactive prompts
shh-add --generate server_name

# Non-interactive generation with defaults (ed25519, no passphrase)
shh-add --generate server_name --auto

# Generate a key directly in RAM (never touches disk)
shh-add --generate server_name --ram-disk --auto

# Generate and then securely shred the key files afterwards
shh-add --generate server_name --shred --auto

# Fully customized key generation
shh-add --generate --type rsa --bits 4096 --rounds 100 \
        --comment "production server $(date +%Y-%m-%d)" \
        --path ~/temp_keys/prod_key server_name
```

Key generation options:
- `--generate`: Activates key generation mode
- `--type TYPE`: Specifies key type (ed25519, rsa, ecdsa, dsa)
- `--bits BITS`: Sets key size in bits (RSA only, default: 4096)
- `--rounds N`: Sets KDF rounds for security (default: 100)
- `--comment TEXT`: Adds a descriptive comment to the key
- `--path PATH`: Specifies where to save the generated key
- `--auto`: Non-interactive mode, uses defaults for all prompts
- `--shred`: Securely deletes key files after uploading
- `--ram-disk`: Creates keys in RAM for zero disk persistence

#### Manual Key Generation

If you prefer to generate keys manually:

```bash
# Generate a strong, modern Ed25519 key (recommended)
ssh-keygen -t ed25519 -a 100 -f ~/temp_key -C "server_name $(date +%Y-%m-%d)"

# Or generate a strong RSA key (for legacy compatibility)
ssh-keygen -t rsa -b 4096 -a 100 -f ~/temp_key -C "server_name $(date +%Y-%m-%d)"
```

Key parameters explained:
- `-t ed25519`: Modern, secure, and fast algorithm (preferred)
- `-t rsa -b 4096`: Strong RSA key with 4096-bit length (for compatibility)
- `-a 100`: Increases key derivation iterations for enhanced security
- `-C "comment"`: Adds a descriptive comment with date for tracking
- `-f ~/temp_key`: Saves the key to a temporary location for immediate upload

After adding the key to AWS Secrets Manager with `shh-add`, securely destroy the local files:

```bash
# Method 1: Basic secure deletion with shred (most systems)
shred -uz ~/temp_key ~/temp_key.pub

# Method 2: Multi-pass overwrite for added security
shred -vfz -n 10 ~/temp_key ~/temp_key.pub
rm -f ~/temp_key ~/temp_key.pub

# Method 3: Use secure-delete tools if available
srm -vz ~/temp_key ~/temp_key.pub  # If secure-delete package is installed
```

For extremely sensitive environments:
```bash
# Recommended: Create keys in RAM disk for zero disk persistence
mkdir -p /dev/shm/temp_ssh
cd /dev/shm/temp_ssh
ssh-keygen -t ed25519 -a 100 -f ./key -C "server_name $(date +%Y-%m-%d)"
shh-add ./key server_name --pub
shred -uz ./key ./key.pub
cd -
rmdir /dev/shm/temp_ssh
```

These practices ensure:
- Strong cryptographic keys are generated
- Keys never touch persistent storage (or are securely erased)
- No sensitive material remains on your local system
- Key metadata includes creation date and purpose for tracking

### 🔁 **The Complete Shh Workflow**

Here's how to eliminate local SSH keys from your system while maintaining secure access:

```bash
# Traditional method (multiple steps):
# Step 1: Generate a new SSH key (you can use any name/path)
ssh-keygen -t ed25519 -f ~/temp_key

# Step 2: Upload to AWS Secrets Manager (include the public key too)
shh-add ~/temp_key server_name --pub

# Step 3: Securely shred the local key files
shred -u ~/temp_key ~/temp_key.pub

# NEW: Simplified one-step method with built-in generation:
# Generate, upload, and automatically shred in one command
shh-add --generate server_name --auto --shred

# For maximum security, generate directly in RAM
shh-add --generate server_name --auto --ram-disk

# Step 4: Connect to your server anytime with NO LOCAL KEY
shh user@hostname -i server_name
# OR use automatic key selection based on username
shh user@hostname
```

After completing these steps:
- ✅ The key exists ONLY in AWS Secrets Manager
- ✅ Your local system has ZERO SSH keys for that server
- ✅ When connecting, the key is securely retrieved from AWS and loaded directly into ssh-agent memory
- ✅ No sensitive data is ever written to disk during connection

This is the core value proposition of Shh - **you can access all your servers with just your AWS credentials**!

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

# Generate a new key and upload it in one step (with prompts)
shh-add --generate myserver_key

# Generate a key non-interactively and automatically shred it
shh-add --generate myserver_key --auto --shred

# Generate a key directly in RAM (never touches disk)
shh-add --generate myserver_key --ram-disk

# Generate a custom RSA key with options
shh-add --generate --type rsa --bits 4096 --comment "production key" myserver_key
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
# Basic syntax (uses username_ed25519 key by default)
shh user@hostname [region] [options]

# Specify a key with -i flag (SSH-style)
shh -i mykey_ed25519 user@hostname [region] 

# Standard SSH options are passed through
shh -i mykey_ed25519 -p 2222 user@hostname

# Enable debug output
shh --debug user@hostname

# Configure environment variables
shh --env
```

The `shh` command performs the following steps:
1. Determines which key to use:
   - If specified with `-i`, uses that key name
   - Otherwise, defaults to `username_ed25519` based on the user part of user@host
2. Securely retrieves the key from AWS Secrets Manager
3. Identifies key fingerprint from metadata
4. Checks if the key is already loaded in `ssh-agent`
5. Adds the key to `ssh-agent` in memory (no disk writes) if needed
6. Connects to the specified server

## 🏗️ Project Architecture

The Shh toolkit consists of several components, each with a specific purpose:

| Component | Description |
|-----------|-------------|
| **shh** | Main command for SSH connections using keys from AWS Secrets Manager |
| **shh-add** | Tool for adding SSH keys to AWS Secrets Manager |
| **shh-admin** | Administration utility for managing secrets, IAM permissions, and updates |
| **shh-env** | Environment variable management with beautiful UI |
| **shh-install** | Installer/uninstaller script with automation support |

### Installation Directory Structure
The toolkit is installed in:
- `/usr/local/share/shh/` - Main installation directory containing all scripts
- `/usr/local/bin/` - Symlinks to the scripts for easy command-line access
- `/var/log/shh.log` - System log file for installation and operation events

### Design Philosophy
The Shh toolkit follows these design principles:
- **Security First**: No sensitive data written to disk, all operations in memory
- **User Experience**: Beautiful UI with consistent color scheme and formatting
- **Integration**: Works with existing AWS and SSH tools seamlessly
- **Automation**: Full support for CI/CD pipelines and scripted operation
- **Best Practices**: Encourages key rotation and secure credential management

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
All Shh commands support a `--debug` flag for troubleshooting:
```bash
shh user@host keyname --debug
shh-add ~/.ssh/mykey --debug
shh-admin --debug
shh-env --debug
```

## 🛡️ Security Considerations
- No SSH keys are ever written to disk during retrieval
- Keys are securely transmitted from AWS Secrets Manager to SSH agent in memory
- All AWS connections use your authenticated AWS CLI credentials
- Key fingerprints are stored to verify agent-loaded keys without requiring passphrase entry
- All scripts use set -e to ensure they exit immediately on errors

## 🔍 Troubleshooting

### Common Issues

**Issue**: Script not found after installation
**Solution**: Check that symlinks were created properly in `/usr/local/bin`

```bash
ls -la /usr/local/bin/shh*
```

**Issue**: AWS authentication failures
**Solution**: Check your AWS credentials and run:

```bash
aws sts get-caller-identity
```

**Issue**: SSH agent not running
**Solution**: Start ssh-agent manually:

```bash
eval "$(ssh-agent -s)"
```

### Updating Shh

To update Shh to the latest version:

```bash
# Interactive update with confirmation
shh-admin --update

# When updating from an older version without the update feature
curl -fsSL https://raw.githubusercontent.com/jenova-marie/shh/root/shh-install | bash
```

The update process will:
- Download the latest installer from the GitHub repository
- Execute it to update all components
- Preserve your existing configuration and environment settings
- Provide feedback on the update status

### Logs and Debugging
The main log file is located at:
```
/var/log/shh.log
```

For verbose output, add the `--debug` flag to any command:
```bash
shh --debug user@host
```

## 🌟 Contributing to Shh

We welcome and encourage community contributions to Shh! Whether you're fixing bugs, improving documentation, or proposing new features, your help makes Shh better for everyone.

### Development Principles

When contributing to Shh, please keep these principles in mind:

1. **Security First**: All changes must maintain or enhance the security model of Shh. No sensitive data should ever be written to disk during key retrieval operations.

2. **Beautiful UI**: Maintain consistent visual styling with Unicode box-drawing characters, thoughtful color schemes, and emoji indicators for user feedback.

3. **Documentation**: Update documentation alongside code changes. Documentation should be clear, accurate, and provide examples.

4. **User Experience**: Make the tools intuitive and provide helpful feedback to users. Error messages should guide users toward solutions.

5. **Compatibility**: Ensure backward compatibility where possible, especially for scripted/automated uses.

### Getting Started with Development

1. Fork the repository
2. Clone your fork: `git clone https://github.com/YOUR-USERNAME/shh.git`
3. Create a feature branch: `git checkout -b my-new-feature`
4. Make your changes
5. Test thoroughly, especially edge cases
6. Commit your changes: `git commit -am 'Add some feature'`
7. Push to the branch: `git push origin my-new-feature`
8. Submit a pull request

### UI Guidelines

The Shh toolkit prioritizes a beautiful and consistent terminal UI:

- Use box-drawing characters (`╔═╗║╚╝`) for section headers
- Use color consistently:
  - Hot pink/fuschia for primary headings and success messages
  - Yellow for warnings and important notes
  - White for normal text
  - Green for success indicators
- Include emoji indicators for different types of messages:
  - 💡 for tips and helpful information
  - ⚠️ for warnings
  - ✅ for success
  - ❌ for errors
- Format output with clear spacing and alignment
- Group related information visually

### Security Review Checklist

Before submitting a pull request, ensure your code meets these security requirements:

- [ ] No sensitive data is written to disk without explicit user permission
- [ ] All temporary files are properly secured (permissions) and cleaned up
- [ ] Error messages don't leak sensitive information
- [ ] Proper error handling for all AWS operations
- [ ] Input validation for all user-provided parameters
- [ ] Follows the principle of least privilege for AWS operations

We look forward to your contributions and ideas to make Shh even better!

## 📝 License
Shh is released under the **MIT License**.

## AWS IAM Permissions Required

The following AWS IAM permissions are required for Shh to function properly:

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "secretsmanager:CreateSecret",
                "secretsmanager:GetSecretValue",
                "secretsmanager:PutSecretValue",
                "secretsmanager:UpdateSecret",
                "secretsmanager:DescribeSecret",
                "secretsmanager:ListSecrets"
            ],
            "Resource": "arn:aws:secretsmanager:*:*:secret:YOUR-SECRET-NAME-*"
        }
    ]
}
```

Replace `YOUR-SECRET-NAME` with your actual secret name (e.g., `ssh-keys`). For secrets with path-like structures (e.g., `Test/X/123`), use the full path in the resource name.

You can attach this policy to your IAM user or role through the AWS Management Console or AWS CLI.