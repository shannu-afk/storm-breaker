# Storm Breaker — Educational (Python) — CONSENT ONLY

> **READ THIS FIRST — LEGAL & ETHICAL WARNING**  
> This repository and instructions are for **educational, defensive, and lab testing** purposes **only**, on machines and networks you own or where you have **explicit, written permission** to test.  
> **Do NOT** use this tool to access cameras, microphones, locations, or personal data on devices you do not own or do not have clear written consent to test. Misuse may be illegal and unethical.

---

## Overview (what this repo contains)
- `st.py` — Python server script (entrypoint). It starts a local web server and (in lab/simulated mode) serves local pages that model IP/location/media endpoints.  
- `requirements.txt` — Python dependencies.  
- `templates/` (optional) — HTML templates (if present) like `camera_temp.html`, `mic_temp.html`, `location_temp.html`, `weather_temp.html`. If templates are not present, `st.py` runs in a default local UI mode or generates the pages dynamically (check script behavior).  
- `static/` (optional) — placeholder images/audio used for simulation.  
- This README — local-run instructions and safety guidance.

---

## Important Safety Rules (MUST READ)
1. Only run this on systems you own or explicitly control (use a local VM recommended).  
2. Do NOT expose the server to the public internet unless you have explicit written consent from everyone affected.  
3. Use only simulated media / placeholder files unless you are testing on a device you own and you understand the privacy/security implications.  
4. Keep auditable logs of all testing activity and permission records.  
5. If collaborating, obtain written consent from collaborators and keep records.

---

## Full copy-ready local install & run (commands)

> Run these commands in a local, isolated environment (VM or local machine you own). Replace placeholders as needed. The commands below are exactly what you'd run locally.

```bash
# 1) Clone the repository (local-only)
git clone https://github.com/shannu-afk/storm-breaker.git
cd storm-breaker

# 2) Create and activate a Python virtual environment (Linux / macOS)
python3 -m venv venv
source venv/bin/activate

# 3) Upgrade pip and install Python requirements
python -m pip install --upgrade pip setuptools wheel
pip install -r requirements.txt

# 4) Inspect repository contents (optional)
ls -la
ls -la templates    # check if templates/ exist
ls -la static       # check if sample media are present

# 5) Run the Python entrypoint (st.py)
#    The script will start a local server and print the local port it is using (e.g., 2525).
python3 st.py

# 6) Open the UI in a browser on the same machine (local-only)
#    Example (replace port if different):
#      http://localhost:2525/templates/camera_temp.html
#      http://localhost:2525/templates/location_temp.html
#      http://localhost:2525/templates/mic_temp.html

# 7) If you intentionally and lawfully need remote access for legitimate collaboration,
#    you may use a tunneling tool like ngrok — BUT ONLY WITH WRITTEN CONSENT FROM ALL PARTIES.
#    The commands below show how to install ngrok and start an http tunnel locally.
#    (Do NOT publish or distribute public links unless you have explicit permission.)

# Install ngrok (Linux / Debian-based example)
curl -s https://ngrok-agent.s3.amazonaws.com/ngrok.asc | sudo tee /etc/apt/trusted.gpg.d/ngrok.asc >/dev/null
echo "deb https://ngrok-agent.s3.amazonaws.com buster main" | sudo tee /etc/apt/sources.list.d/ngrok.list
sudo apt update && sudo apt install ngrok

# Start an ngrok tunnel for the printed port (replace <PORT> with the port shown by st.py)
ngrok http <PORT>

# ngrok will display a Forwarding URL (https://abcd1234.ngrok.io).
# Use that URL ONLY for authorized, consensual testing and only while all participants have been informed.

# 8) When finished, stop the server (press Ctrl+C) and deactivate venv
deactivate
##############################
# PORT FORWARDING (COPY-ONLY)
# IMPORTANT: ONLY use these commands in a lawful, consensual, and controlled environment.
# Do NOT expose devices you do not own or do not have explicit written permission to test.
##############################

# -----------------------------
# 1) Install ngrok (Debian/Ubuntu example)
# -----------------------------
# Add ngrok apt repo and install (run as a user with sudo)
curl -s https://ngrok-agent.s3.amazonaws.com/ngrok.asc | sudo tee /etc/apt/trusted.gpg.d/ngrok.asc >/dev/null
echo "deb https://ngrok-agent.s3.amazonaws.com buster main" | sudo tee /etc/apt/sources.list.d/ngrok.list
sudo apt update && sudo apt install -y ngrok

# -----------------------------
# 2) Authenticate ngrok (replace <YOUR_NGROK_AUTHTOKEN> with your ngrok authtoken)
# -----------------------------
# Get your authtoken from https://dashboard.ngrok.com/get-started/your-authtoken
ngrok authtoken <YOUR_NGROK_AUTHTOKEN>

# -----------------------------
# 3) Start an HTTP tunnel to a local port (replace <PORT> with the port printed by st.py)
# -----------------------------
# Example: if st.py shows port 2525
ngrok http 2525

# Optional: run in the background (Linux)
# 1) start ngrok in a detached tmux or nohup
nohup ngrok http 2525 &>/dev/null &

# 2) or use systemd service (example unit) to run ngrok persistently:
# Create /etc/systemd/system/ngrok.service with appropriate privileges and your authtoken configured.
# Example systemd unit (create/edit with sudo privileges):
# -------------------
# [Unit]
# Description=ngrok tunnel
# After=network.target
#
# [Service]
# ExecStart=/usr/bin/ngrok http 2525
# Restart=on-failure
# User=yourusername
#
# [Install]
# WantedBy=multi-user.target
# -------------------
# Then:
# sudo systemctl daemon-reload
# sudo systemctl enable --now ngrok.service

# -----------------------------
# 4) SSH reverse tunnel (alternative to ngrok) — only if you control the remote server
# -----------------------------
# This requires a remote server with a public IP and an SSH account (user@remote.example.com).
# The remote server must permit GatewayPorts or allow remote forwarding.
# Forward remote port 8080 to local 2525:
ssh -R 8080:localhost:2525 user@remote.example.com
# Then users can access http://remote.example.com:8080 (if firewall allows).
# WARNING: Only use this with servers you own/control and with proper firewall rules.

# -----------------------------
# 5) Example: how to map ngrok forwarding URL to local template paths
# -----------------------------
# After ngrok http 2525 runs, ngrok prints Forwarding URLs e.g.:
# Forwarding  http://abcd1234.ngrok.io  -> http://localhost:2525
# To access camera template via ngrok:
# https://abcd1234.ngrok.io/templates/camera_temp.html

# -----------------------------
# 6) Stopping ngrok or SSH tunnel
# -----------------------------
# If started in foreground: Ctrl+C
# If started with nohup: find PID and kill
ps aux | grep ngrok
kill <NGROK_PID>

# For ssh tunnels, Ctrl+C in the ssh session will close the tunnel.

# -----------------------------
# 7) Security & logging recommendations
# -----------------------------
# - Use ngrok authtoken and enable password or basic auth on any sensitive pages.
# - Do not share ngrok URLs publicly; share only with explicitly authorized collaborators.
# - Keep an audit log of who accessed the tunnel and when.
# - Use HTTPS forwarding URLs (ngrok provides https) and restrict routes behind simple auth if needed.

# -----------------------------
# CONTACT / CREDENTIALS 
# -----------------------------
# Name: K.Shanmukh
# NOTE: Do NOT paste credentials or secrets in public repos. Share credentials only via secure, private channels.
# Mail me for login credentials
# -----------------------------
# FINAL REMINDER
# -----------------------------
# These commands are provided for lawful, consented testing in isolated environments (VMs or lab networks).
# Misuse to access others' devices or private data is illegal and unethical. You are responsible for ensuring permission.
# -----------------------------
