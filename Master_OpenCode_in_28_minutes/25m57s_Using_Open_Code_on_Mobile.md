# Using OpenCode on Mobile (25:57 - 28:03)

## Running OpenCode from Your Phone

OpenCode is designed for the terminal, but that doesn't mean you're tied to a desktop. Here's how to use OpenCode from your mobile device.

## Method 1: SSH Connection (Same WiFi)

The simplest approach — connect to your desktop from your phone over your local network.

### Windows + Android Setup

1. **On your Windows desktop (server):**
   - Install OpenSSH Server:
     ```
     # Windows Settings → Apps → Optional Features → Add "OpenSSH Server"
     # Or via PowerShell as Admin:
     Add-WindowsCapability -Online -Name OpenSSH.Server~~~~0.0.1.0
     Start-Service sshd
     Set-Service -Name sshd -StartupType 'Automatic'
     ```
   - Verify your local IP: `ipconfig` (look for IPv4 address, e.g., `192.168.1.100`)
   - Ensure port 22 is open in Windows Firewall

2. **On your Android phone (client):**
   - Install a terminal app like **Termux** from F-Droid
   - Or use **JuiceSSH** / **Termius** for a simpler SSH experience
   - Connect: `ssh username@192.168.1.100`
   - Once connected: navigate to your project and run `opencode`

3. **On your iPhone/iPad:**
   - Use **Termius** or **Blink Shell** from the App Store
   - Same SSH connection flow

### Requirements
- Both devices on the same WiFi network
- SSH server running on the desktop
- SSH client on the mobile device
- A terminal emulator that supports modern TUI features

### Limitations of Local SSH
- Only works when you're on your home network
- Latency can be noticeable on complex TUI interactions
- Screen size can make TUI navigation challenging

## Method 2: Internet-Accessible Server (Tunnel)

For remote access when you're away from home, set up a tunnel to your desktop.

### Option A: Tailscale (Recommended)
```bash
# Install Tailscale on both devices
# Connect both to the same Tailscale network
# SSH using the Tailscale IP:
ssh user@100.x.x.x
```

### Option B: Cloudflare Tunnel
```bash
# Install cloudflared on your desktop
cloudflared tunnel create opencode-tunnel
cloudflared tunnel route dns opencode-tunnel your-domain.com

# Set up SSH over the tunnel
# Connect from your phone:
ssh -o "ProxyCommand cloudflared access ssh --hostname %h" user@your-domain.com
```

### Option C: ngrok / bore
```bash
# Quick tunnel (less secure, use for testing)
ngrok tcp 22
# Then connect via the ngrok URL
```

### Option D: Custom VPS (Self-Hosted)
- Rent a cheap VPS (DigitalOcean, Hetzner, etc.)
- Connect your desktop to the VPS via WireGuard
- SSH from phone → VPS → Desktop

## Method 3: Using OpenCode Web

OpenCode also offers a **web interface** at opencode.ai that works in mobile browsers:
- No SSH required
- Limited compared to full TUI
- Good for quick tasks and answers
- Accessible from any device with a browser

## Making It Work Well on Mobile

### Optimizing the Terminal Experience

1. **Use a smaller font size**: Mobile screens need compact terminals
2. **Enable keyboard shortcuts**: Learn the essential shortcuts to avoid typing
3. **Use tmux or screen**: Keep sessions alive if your SSH disconnects
4. **Set up Zsh with plugins**: Autosuggestions and syntax highlighting help on mobile keyboards
5. **Create aliases for common commands**:
   ```bash
   alias oc="opencode"
   alias ocinit="opencode && /init"
   ```

### Recommended Mobile Terminal Apps

| App | Platform | Features |
|-----|----------|----------|
| Termux | Android | Full Linux environment, package manager |
| JuiceSSH | Android | Clean SSH client, keyboard shortcuts |
| Termius | iOS/Android | Cross-platform, sync configs |
| Blink Shell | iOS | Mosh support, local terminal, themes |
| a-Shell | iOS | Local terminal, Python, commands |

### Pro Tips

- **Use Mosh** instead of SSH for mobile: Mosh handles intermittent connections much better
- **Keep your PC on**: Use Wake-on-LAN to start your desktop remotely
- **Set up SSH keys**: Password-based login is painful on mobile keyboards
- **Use a Bluetooth keyboard**: For serious mobile coding sessions
- **Persist sessions with tmux**: `tmux new -s opencode` → run opencode → detach with `Ctrl+B D` → reconnect later

### The "Always-On" Setup

For the ultimate mobile experience:
1. Desktop stays at home, always on
2. Tailscale provides secure networking
3. SSH with Mosh for reliable connections
4. tmux for persistent sessions
5. OpenCode running in a tmux window
6. Connect from phone anytime, anywhere

This transforms your phone into a portable terminal that can orchestrate AI coding agents on your powerful desktop hardware.
