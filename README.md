# Multi-Account Switcher for Claude Code

A simple tool to manage and switch between multiple Claude Code accounts on macOS, Linux, and WSL.

## Features

- **Multi-account management**: Add, remove, and list Claude Code accounts
- **Quick switching**: Switch between accounts with simple commands
- **Cross-platform**: Works on macOS, Linux, and WSL
- **Secure storage**: Uses system keychain (macOS) or protected files (Linux/WSL)
- **Smart keychain handling**: Automatically detects and manages different Claude Code keychain service names
- **Settings preservation**: Only switches authentication - your themes, settings, and preferences remain unchanged

## Installation

Download the script directly:

```bash
curl -O https://raw.githubusercontent.com/Second-Victor/cc-account-switcher-zsh/main/cc-switcher.zsh
chmod +x cc-switcher.zsh
```

## Usage

### Basic Commands

```bash
# Add current account to managed accounts
./cc-switcher.zsh --add-account

# List all managed accounts
./cc-switcher.zsh --list

# Switch to next account in sequence
./cc-switcher.zsh --switch

# Switch to specific account by number or email
./cc-switcher.zsh --switch-to 2
./cc-switcher.zsh --switch-to user2@example.com

# Remove an account
./cc-switcher.zsh --remove-account user2@example.com

# Show help
./cc-switcher.zsh --help
```

### First Time Setup

1. **Log into Claude Code** with your first account (make sure you're actively logged in)
2. Run `./cc-switcher.zsh --add-account` to add it to managed accounts
3. **Log out** and log into Claude Code with your second account
4. Run `./cc-switcher.zsh --add-account` again
5. Now you can switch between accounts with `./cc-switcher.zsh --switch`
6. **Important**: After each switch, restart Claude Code to use the new authentication

> **What gets switched:** Only your authentication credentials change. Your themes, settings, preferences, and chat history remain exactly the same.

## Requirements

- Zsh 5.0+
- `jq` (JSON processor)

### Installing Dependencies

**macOS:**

```bash
brew install jq
```

**Ubuntu/Debian:**

```bash
sudo apt install jq
```

## How It Works

The switcher stores account authentication data separately:

- **macOS**: Credentials in Keychain (with service name tracking), OAuth info in `~/.claude-switch-backup/`
- **Linux/WSL**: Both credentials and OAuth info in `~/.claude-switch-backup/` with restricted permissions

When switching accounts, it:

1. Backs up the current account's authentication data
2. Restores the target account's authentication data
3. Manages the correct keychain service name for each account (macOS)
4. Updates Claude Code's authentication files

## Troubleshooting

### If a switch fails

- Check that you have accounts added: `./cc-switcher.zsh --list`
- Verify Claude Code is closed before switching
- Try switching back to your original account

### If you can't add an account

- Make sure you're logged into Claude Code first
- Check that you have `jq` installed
- Verify you have write permissions to your home directory

### If Claude Code doesn't recognize the new account

- Make sure you restarted Claude Code after switching
- Check the current account: `./cc-switcher.zsh --list` (look for "(active)")

## Cleanup/Uninstall

To stop using this tool and remove all data:

1. Note your current active account: `./cc-switcher.zsh --list`
2. Remove the backup directory: `rm -rf ~/.claude-switch-backup`
3. Delete the script: `rm cc-switcher.zsh`

Your current Claude Code login will remain active.

## Security Notes

- Credentials stored in macOS Keychain or files with 600 permissions
- Authentication files are stored with restricted permissions (600)
- The tool requires Claude Code to be closed during account switches

## License

MIT License - see LICENSE file for details
