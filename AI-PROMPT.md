# Shh Project Development Guide

## Project Overview

Shh is a security-focused toolkit for managing SSH keys using AWS Secrets Manager. The project prioritizes secure credential handling with zero disk writes for sensitive operations, beautiful terminal UI, and seamless integration with existing DevOps tools.

## Core Principles

1. **Security First**: Never write sensitive credentials to disk during operations
2. **Beautiful UX**: Create visually appealing terminal interfaces with consistent styling
3. **Seamless Integration**: Work naturally with existing DevOps tools and workflows
4. **Automation Support**: Enable CI/CD and scripted operations with non-interactive modes
5. **Best Practices Enforcement**: Encourage key rotation and proper security protocols

## Development Instructions

As an AI assistant, help continue development of the Shh toolkit by following these instructions:

### Code Analysis and Understanding

1. First examine the existing scripts in the repository:
   - shh-admin: For managing AWS secrets and environment configuration
   - shh-add: For adding SSH keys to AWS Secrets Manager
   - shh-env: For managing environment variables
   - shh-install: For installation and updates
   - shh: Main command for SSH connections using stored keys

2. Look for patterns in the codebase:
   - How error handling is implemented
   - How UI elements are formatted (box-drawing characters, colors, emojis)
   - How AWS interactions are secured
   - How parameters are validated

### Feature Development Guidelines

When implementing new features:

1. **Security Requirements**
   - All secret operations must occur in memory only
   - Use temporary files only when absolutely necessary, with proper permissions and cleanup
   - Validate all user inputs to prevent injection vulnerabilities
   - Follow AWS IAM best practices with least privilege principle
   - Implement comprehensive error handling for AWS operations
   - Include timeout mechanisms for sensitive operations

2. **UI Requirements**
   - Use consistent visual formatting:
     ```
     ╔═════════════════════════════════════════════════════════════════╗
     ║                      📋 Section Title                           ║
     ╚═════════════════════════════════════════════════════════════════╝
     ```
   - Follow the color scheme:
     - Hot pink/fuschia for headers and success messages
     - Yellow for warnings and important notes
     - White for standard text
     - Green for success indicators
   - Use emoji indicators consistently:
     - 💡 for tips and helpful information
     - ⚠️ for warnings
     - ✅ for success
     - ❌ for errors
   - Format command outputs with proper alignment using printf
   - Always provide progress feedback for operations that take time
   - Group related information visually

3. **Script Structure**
   - Begin with proper shebang and set -e
   - Include a usage() function showing all parameters
   - Include debug_log() function for verbose mode
   - Use consistent parameter parsing
   - Use SCRIPT_DIR to reference other scripts
   - Implement non-interactive mode for all tools
   - Add a --debug flag to all scripts
   - Include version information

4. **Documentation**
   - Update or create --help documentation within scripts
   - Update README.md with examples for new features
   - Add usage examples for both basic and advanced patterns
   - Document any new environment variables or configuration options
   - Create troubleshooting sections for potential issues

### Feature Roadmap

Prioritize implementing the following features in order:

1. **Enhanced Key Rotation System**
   - Create a dedicated shh-rotate script
   - Implement automatic fingerprint verification
   - Add support for scheduled rotations
   - Include rollback capability if rotation fails
   - Add notifications for upcoming rotation needs

2. **Team Collaboration Features**
   - Implement shared key management
   - Add multi-user access controls
   - Create activity logging for compliance
   - Support organizational structure in key naming

3. **Advanced Security Enhancements**
   - Add multi-factor authentication support
   - Implement IP-based access restrictions
   - Create key usage audit trail
   - Support temporary access credentials
   - Add connection logging

4. **Integration Expansions**
   - Support for Kubernetes secrets
   - Integration with HashiCorp Vault
   - Support for other cloud providers (GCP, Azure)
   - Ansible and Terraform plugin development

### Implementation Instructions

For each new feature:

1. **Planning Phase**
   - Identify the security implications first
   - Map out user workflows and interaction points
   - Design the UI elements and feedback mechanisms
   - Plan for both interactive and non-interactive usage
   - Identify potential edge cases and errors

2. **Implementation Checklist**
   - Create or modify scripts with consistent styling
   - Implement comprehensive error handling
   - Add appropriate debug logging
   - Ensure backward compatibility
   - Follow the UI guidelines precisely

3. **Testing Requirements**
   - Include success path testing
   - Test with invalid inputs
   - Test with AWS service interruptions
   - Verify security constraints are enforced
   - Test in both interactive and non-interactive modes

### Example Script Commands

Implement new scripts or features similar to these examples:

**Key Rotation Command:**
```bash
# Interactive key rotation
shh-rotate mykey_ed25519

# Force rotation without confirmation
shh-rotate mykey_ed25519 --force

# Schedule future rotation
shh-rotate mykey_ed25519 --schedule "2024-06-01"

# Rotation with specific bit length
shh-rotate mykey_ed25519 --bits 4096
```

**Team Collaboration Commands:**
```bash
# Share a key with a team member
shh-share mykey_ed25519 --user alice@example.com

# Revoke shared access
shh-share mykey_ed25519 --revoke --user bob@example.com

# List all shared keys
shh-share --list

# Share with an expiration time
shh-share mykey_ed25519 --user carol@example.com --expires "2024-12-31"
```

### Code Style Guidelines

1. **Bash Best Practices**
   - Use double quotes around variables
   - Use [[ ]] for conditional tests
   - Use local variables in functions
   - Add error handling for all commands that might fail
   - Use meaningful variable names
   - Add comments for complex logic

2. **AWS Interactions**
   - Always specify region
   - Handle pagination for lists
   - Implement retries for transient errors
   - Use appropriate error handling
   - Properly escape all input values

3. **Security Practices**
   - Validate all input parameters
   - Sanitize any data used in commands
   - Use principle of least privilege
   - Clean up temporary files with trap
   - Don't log sensitive information even in debug mode

## Security Review Checklist

Before considering any feature complete, verify:

- [ ] No sensitive data is written to disk without explicit user permission
- [ ] All temporary files are properly secured and cleaned up
- [ ] Error messages don't leak sensitive information
- [ ] Proper error handling for all operations
- [ ] Input validation for all user-provided parameters
- [ ] Follows the principle of least privilege for AWS operations
- [ ] Includes appropriate user feedback and confirmation for destructive operations
- [ ] Documentation clearly explains security implications
- [ ] Debug logs don't contain sensitive information
- [ ] Key material is never logged or displayed in plain text

This guide should inform all AI-assisted development for the Shh project, ensuring consistent, secure, and user-friendly implementations that align with the project's core principles.
