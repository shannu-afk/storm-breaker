# Storm Breaker — Educational Security Lab (SIMULATED)

> **IMPORTANT (READ FIRST):**  
> This repository and README are **only** for educational, defensive, or research purposes inside an isolated, controlled lab environment that you own or explicitly manage.  
> **Do NOT** attempt to access cameras, microphones, locations, or other private resources on devices you do not own or do not have explicit written permission to test. Misuse may be illegal and unethical.

---

## About (SAFE / SIMULATED)
**Storm Breaker (SIMULATED)** is a training / research project that demonstrates how a web server and local UI can model device permissions and sensor-like endpoints for learning purposes.  
All endpoints and UI pages in this repo **must** be used with mock or simulated data only. The project is intended to teach:

- server ↔ client interactions,  
- permission-like prompts and consent logging,  
- secure coding practices,  
- how to build defenses and detection for suspicious access patterns.

This README contains **local-only** setup and run instructions. It intentionally does **not** include any instructions to access real remote devices or to expose testing systems to the public internet.

---

## What this project contains (simulated)
- `st.py` or equivalent server script that serves local HTML pages and simulated endpoints.  
- `templates/` — UI pages (`camera_temp.html`, `mic_temp.html`, `location_temp.html`, `weather_temp.html`) that **render mock data** (simulated images/audio/geo).  
- `static/` — sample media files used for simulation (placeholder images / sample audio).  
- `requirements.txt` — Python dependencies (if Python used) OR `package.json` if Node used.  
- Example logging for consent and activity for defensive analysis.

---

## Tech stack (example)
- Python 3.8+ (Flask or simple HTTP server), or Node.js + Express (adjust based on repo code)  
- HTML/CSS/JS for frontend templates  
- Local JSON files for simulated data storage  
- No external services required for local lab

---

## Safety rules (must follow)
1. **Only run this on systems you control.** Use a VM (VirtualBox/VMware) or an isolated physical machine.  
2. **Do not** expose the server to the public internet. Do not use port forwarding, NAT traversal, or tunneling tools for testing real devices.  
3. Use **mock media files** (provided or generated) instead of capturing real camera/mic input.  
4. Keep logs of tests and obtain written permission for any collaborative lab where multiple people access the environment.  
5. If you discover a vulnerability in external software, follow responsible disclosure policies.

---

## Full copy-ready local setup & run (all commands below are local-only)

> **Run these commands from your terminal in a safe lab VM or local dev machine you own. Do not forward ports or expose to the internet.**

    # 1) Clone the repo (local only)
    git clone https://github.com/shannu-afk/storm-breaker.git
    cd storm-breaker

    # 2) Create and activate a Python virtual environment (Linux / macOS)
    python3 -m venv venv
    source venv/bin/activate

    # 3) Upgrade pip and install Python requirements (if this repository uses Python)
    python -m pip install --upgrade pip setuptools wheel
    pip install -r requirements.txt

    # 4) (If the repo uses Node for server) Alternatively install Node deps
    # npm install
    # or
    # yarn

    # 5) Confirm templates and simulated media exist
    # (Example: make sure templates/camera_temp.html and static/sample.jpg are present)
    ls -la templates
    ls -la static

    # 6) Run the server locally (example Python entrypoint - adjust to your script)
    # This must run only locally inside your VM or controlled machine.
    python3 st.py

    # 7) Open the UI on the same machine's browser:
    # Visit (example):
    #   http://localhost:2525/templates/camera_temp.html
    #   http://localhost:2525/templates/location_temp.html
    #   http://localhost:2525/templates/mic_temp.html
    # NOTE: These templates must use simulated media; do NOT plug into real device capture in this lab.

    # 8) Stopping the server
    # Press Ctrl+C in the terminal where the server runs.

    # 9) Deactivate the virtual environment when done
    deactivate

---

## How simulation is implemented (developer notes)

Templates (`templates/*.html`) should use local sample files under `static/` (images/audio/json). They should never request real device permissions (no `getUserMedia()` calls that access physical hardware unless you explicitly want to test local device prompts on *your own* machine).

Server endpoints (e.g., `/api/ip`, `/api/location`, `/api/camera`) should return mock JSON or pre-recorded media — e.g., `{"ip": "999.99.99.99"}` or `static/sample_image.jpg`.

Any consent workflow should record entries to a local log file with timestamp, action, and a clear consent flag.

---

## Example: safe mock endpoint (Flask illustrative example)

    # (Flask example)
    from flask import Flask, send_from_directory, jsonify
    app = Flask(__name__)

    @app.route('/api/ip')
    def ip():
        return jsonify({"ip": "999.99.999.99", "note": "simulated"})

    @app.route('/templates/camera_temp.html')
    def camera_ui():
        return send_from_directory('templates', 'camera_temp.html')

    # serve static simulated file
    @app.route('/static/<path:fn>')
    def static_files(fn):
        return send_from_directory('static', fn)

---

## Logging & consent example (required)
- Save consent records to `logs/consent.log` with format:
  
      2025-11-06T12:00:00Z | user: test | action: view_camera_template | consent: true

- Use logs for audit, teach how to detect suspicious access patterns.

---

## Testing exercises (safe)
1. Build a VM network containing two VMs: "attacker-sim" and "target-sim". Both are owned by you. Use the tools to simulate the attacker requesting simulated resources from target-sim. Analyze logs.  
2. Implement rate-limiting on endpoints and test how many requests trigger alerts.  
3. Replace mock images/audio with deliberately tampered files to study integrity checks.  
4. Practice writing a responsible disclosure report for a simulated flaw you find in your own lab.

---

## Contributing (safe changes)
- Submit PRs that **improve safety**, add defensive features, or replace any real-device access with simulated content.  
- Any code that enables access to real remote devices without consent will be rejected.

---

## License & Contact
- License: choose an appropriate open-source license (e.g., MIT) and clearly state that the project is for **educational/simulated use only**.  
- Author / Maintainer: Kodali Shanmukh Chowdary — use repo Issues for contact about safe research.

---

## Final reminder (cannot be overstated)
This repository is for simulations only. **Do not** use it to access other people’s devices or private data. If you need help converting parts of the repo to purely simulated implementations (e.g., swapping live capture for placeholder files), I can provide exact code snippets — but I will **not** provide instructions or code that enable non-consensual spying or intrusion.
