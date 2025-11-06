# Jejak_Kuluk

```
   ____.       __          __           ____  __.      .__          __    
  |    | ____ |__|____    |  | __      |    |/ _|__ __|  |  __ __ |  | __
  |    |/ __ \|  \__  \   |  |/ /      |      < |  |  \  | |  |  \|  |/ /
/\__|    \  ___/| |/ __ \_ |    <       |    |  \|  |  /  |_|  |  /    < 
\________|\___  >__(____  / |__|_ \     |____|__ \____/|____/____/|__|_ \
              \/        \/       \/             \/                      \/
```

**Created by:** Wayan_Choir  
**Developed by:** Hornet Selem Team  
**License:** MIT  
**Version:** 1.0.3

---

## 🛡️ Description

**Jejak_Kuluk** (Indonesian for "Trace Footsteps") is a powerful all-in-one defensive cybersecurity CLI tool designed for Linux system administrators and security professionals. It monitors, analyzes, and fingerprints every action on your server to help detect intrusions, failed logins, brute-force attacks, suspicious network activity, and abnormal running processes.

Built with a hacker-themed dark terminal design, Jejak_Kuluk provides real-time security insights, historical forensic analysis, and an interactive dashboard similar to `htop` for process anomaly detection.

---

## ✨ Features

| Category | Description |
|----------|-------------|
| 🔍 **Log Analyzer** | Parse and analyze logs from `/var/log/auth.log`, `syslog`, `nginx`, and custom sources |
| 🧠 **Anomaly Detection** | Detect failed logins, brute-force attempts, SSH abuse, and suspicious access patterns |
| 🌐 **IP & Geo Lookup** | Display IP origin with country, city, and ASN/organization information |
| ⚠️ **Risk Scoring** | Intelligent threat level calculation: 🟩 Safe, 🟨 Suspicious, 🟥 Attack |
| 🕒 **Timeline Monitor** | Chronological activity tracking in real-time |
| 🚨 **Live Alert Mode** | Highlight new or unknown IPs with visual alerts |
| 📜 **Fingerprint Capture** | Record IP, port, timestamp, username, user-agent, and raw log lines |
| 📊 **Dashboard Mode (TUI)** | Interactive terminal dashboard using Rich library (htop-style) |
| 🧾 **Export Reports** | Generate daily reports in JSON or LOG format |
| 🛰️ **Network Monitoring** | Detect port scans and unusual traffic patterns with Scapy |
| 💾 **SQLite Database** | Store historical events for forensic analysis and investigation |
| 👁️ **Traceback Mode** | Investigate intruder fingerprints using combined log and network data |
| 🧩 **Process Anomaly Monitor** | Display running processes with CPU/memory usage and risk assessment |

---

## 🚀 Installation

### Prerequisites

- **Python 3.6+**
- **Linux system** (Ubuntu, Debian, CentOS, etc.)
- **Root/sudo access** (for reading system logs and network monitoring)

### Method 1: Quick Install (One-liner)

```bash
sudo curl -L https://github.com/Taicicaks01/jejak_kuluk/raw/main/jejak_kuluk.py -o /usr/local/bin/jejak_kuluk && sudo chmod +x /usr/local/bin/jejak_kuluk
```

### Method 2: Manual Install

```bash
# Clone the repository
git clone https://github.com/Taicicaks01/jejak_kuluk.git
cd jejak_kuluk

# Install dependencies
pip3 install rich psutil geoip2 scapy

# Make executable
chmod +x jejak_kuluk.py

# Optional: Install system-wide
sudo cp jejak_kuluk.py /usr/local/bin/jejak_kuluk
```

### Optional: GeoIP Database

For IP geolocation features, download the GeoLite2 database:

```bash
# Install GeoIP database
sudo mkdir -p /usr/share/GeoIP
cd /usr/share/GeoIP
sudo wget https://git.io/GeoLite2-City.mmdb
```

---

## 📖 Usage

### Basic Commands

```bash
# Display help
jejak_kuluk --help

# Analyze system logs
jejak_kuluk analyze

# Analyze last 5000 log lines
jejak_kuluk analyze --lines 5000

# Start live monitoring mode
jejak_kuluk live

# Launch interactive dashboard
jejak_kuluk dashboard

# Monitor process anomalies (like top)
jejak_kuluk process

# Trace specific IP address
jejak_kuluk trace --ip 192.168.1.100

# List all high-risk IPs
jejak_kuluk trace

# Export report to JSON
jejak_kuluk export --format json

# Export report to log file
jejak_kuluk export --format log --limit 5000
```

### Command Details

#### 📊 `analyze` - Log Analysis

Parses authentication logs, SSH attempts, sudo commands, and nginx requests. Detects brute-force attacks and suspicious patterns.

```bash
sudo jejak_kuluk analyze
```

**Output:**
- Recent security events table
- Brute-force attack alerts
- Risk scores with color coding
- Geographic information

#### 🔴 `live` - Real-time Monitoring

Continuously monitors logs and displays new events as they occur.

```bash
sudo jejak_kuluk live
```

**Features:**
- Real-time event streaming
- Automatic database storage
- Color-coded risk levels
- Press Ctrl+C to stop

#### 🖥️ `dashboard` - Interactive Dashboard

Launches a full-screen TUI dashboard with live updates.

```bash
sudo jejak_kuluk dashboard
```

**Panels:**
- Recent security events
- Process monitor (top 10)
- Statistics overview
- Auto-refresh every second

#### 🔍 `process` - Process Anomaly Monitor

Displays running processes with risk assessment, similar to `top` or `htop`.

```bash
jejak_kuluk process
```

**Detects:**
- Suspicious process names
- High CPU/memory usage
- Unusual network connections
- Known attack tools (netcat, cryptominers, etc.)

#### 🕵️ `trace` - Intruder Fingerprinting

Investigates specific IP addresses or lists high-risk attackers.

```bash
# Trace specific IP
jejak_kuluk trace --ip 203.0.113.42

# List all high-risk IPs
jejak_kuluk trace
```

**Shows:**
- Total events from IP
- Geographic location
- Event timeline
- Attack patterns
- Attempted usernames

#### 📤 `export` - Report Generation

Exports security data for compliance, auditing, or analysis.

```bash
# Export to JSON
jejak_kuluk export --format json --limit 1000

# Export to log file
jejak_kuluk export --format log
```

---

## 🗂️ Project Structure

```
jejak_kuluk/
├── jejak_kuluk.py          # Single-file application
├── README.md               # This file
└── ~/.jejak_kuluk.db       # SQLite database (auto-created)
```

### Database Schema

**events** table:
- `id`, `timestamp`, `event_type`, `ip_address`, `username`
- `port`, `country`, `city`, `risk_score`, `raw_log`, `fingerprint`

**process_anomalies** table:
- `id`, `timestamp`, `pid`, `name`, `cpu_percent`
- `memory_percent`, `connections`, `risk_level`

---

## 🎨 Screenshots

### Analyze Mode
```
Recent Security Events
┌────────────┬─────────────────┬─────────────────┬──────────────┬──────────────┐
│ Time       │ Type            │ IP              │ User         │ Risk         │
├────────────┼─────────────────┼─────────────────┼──────────────┼──────────────┤
│ Nov 06 ... │ ssh_failed      │ 203.0.113.42    │ root         │ 🟥 70        │
│ Nov 06 ... │ ssh_failed      │ 203.0.113.42    │ admin        │ 🟥 70        │
│ Nov 06 ... │ ssh_accepted    │ 192.168.1.100   │ sysadmin     │ 🟩 30        │
└────────────┴─────────────────┴─────────────────┴──────────────┴──────────────┘

🚨 BRUTE FORCE ATTACKS DETECTED:
  ⚠️  IP: 203.0.113.42 - 15 attempts
     Usernames: root, admin, user, test, ubuntu
```

### Process Monitor
```
Running Processes (Top 20)
┌─────────┬──────────────────┬──────────┬────────┬────────┬──────────────┬────────┐
│ PID     │ Name             │ User     │ CPU%   │ MEM%   │ Connections  │ Risk   │
├─────────┼──────────────────┼──────────┼────────┼────────┼──────────────┼────────┤
│ 1337    │ nginx            │ www-data │ 45.2   │ 12.3   │ 24           │ LOW    │
│ 2048    │ python3          │ root     │ 23.1   │ 8.7    │ 2            │ LOW    │
│ 6666    │ suspicious_proc  │ unknown  │ 85.4   │ 45.2   │ 127          │ HIGH   │
└─────────┴──────────────────┴──────────┴────────┴────────┴──────────────┴────────┘
```

---

## 🔧 Configuration

Jejak_Kuluk works out of the box with sensible defaults. Advanced users can modify:

- **Log paths:** Edit `log_paths` dictionary in `LogAnalyzer` class
- **Risk thresholds:** Modify `_calculate_risk()` method
- **Brute-force detection:** Adjust `threshold` in `detect_brute_force()`
- **Suspicious patterns:** Update `suspicious_patterns` in `ProcessMonitor`

---

## 🤝 Contributing

We welcome contributions from the community! Here's how you can help:

### Ways to Contribute

1. **Report Bugs:** Open an issue describing the bug and steps to reproduce
2. **Feature Requests:** Suggest new features or improvements
3. **Code Contributions:** Submit pull requests with bug fixes or new features
4. **Documentation:** Improve README, add examples, or create tutorials
5. **Testing:** Test on different Linux distributions and report compatibility

### Development Guidelines

```bash
# Fork the repository
git clone https://github.com/YOUR_USERNAME/jejak_kuluk.git
cd jejak_kuluk

# Create a feature branch
git checkout -b feature/amazing-feature

# Make your changes
# Test thoroughly

# Commit with clear messages
git commit -m "Add amazing feature: detailed description"

# Push and create pull request
git push origin feature/amazing-feature
```

### Code Style

- Follow PEP 8 Python style guidelines
- Add docstrings to all functions and classes
- Include comments for complex logic
- Maintain the single-file architecture
- Test with `sudo` privileges where required

---

## 🐛 Known Issues

- **GeoIP Database:** Requires manual download for full geolocation features
- **Scapy Permissions:** Network monitoring requires root/sudo access
- **Log Access:** Reading system logs requires appropriate permissions
- **Python 3.6+:** Not compatible with Python 2.x

---

## 📋 Requirements

### Required Libraries
```
rich>=10.0.0
psutil>=5.8.0
```

### Optional Libraries
```
geoip2>=4.0.0        # For IP geolocation
scapy>=2.4.0         # For network monitoring
```

### Install All Dependencies
```bash
pip3 install rich psutil geoip2 scapy
```

---

## 🔒 Security Considerations

- **Run with Caution:** This tool requires elevated privileges to read system logs
- **Database Security:** The SQLite database (`~/.jejak_kuluk.db`) contains sensitive security data
- **Network Monitoring:** Scapy packet capture may impact network performance
- **False Positives:** Review alerts carefully; not all flagged activity is malicious
- **Legal Compliance:** Ensure monitoring complies with your organization's policies

---

## 📜 License

MIT License

Copyright (c) 2024 Wayan_Choir & Hornet Selem Team

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.

---

## 🌟 Credits

**Created by:** Wayan_Choir  
**Developed by:** Hornet Selem Team  
**Contributors:** Open Source Community

### Special Thanks

- **Rich Library:** For beautiful terminal formatting
- **psutil:** For cross-platform process utilities
- **Scapy:** For powerful packet manipulation
- **GeoIP2:** For IP geolocation services

---

## 📞 Support

- **Issues:** [GitHub Issues](https://github.com/wayanchoir/jejak_kuluk/issues)
- **Discussions:** [GitHub Discussions](https://github.com/wayanchoir/jejak_kuluk/discussions)
- **Email:** wayanchoir@example.com

---

## 🚀 Roadmap

- [ ] Web-based dashboard interface
- [ ] Email/Telegram alert notifications
- [ ] Integration with SIEM systems
- [ ] Machine learning-based anomaly detection
- [ ] Docker container support
- [ ] Multi-server monitoring
- [ ] Custom rule engine
- [ ] Plugin architecture

---

## 📊 Statistics

![GitHub stars](https://img.shields.io/github/stars/Taicicaks01/jejak_kuluk?style=social)
![GitHub forks](https://img.shields.io/github/forks/Taicicaks01/jejak_kuluk?style=social)
![GitHub issues](https://img.shields.io/github/issues/Taicicaks01/jejak_kuluk)
![GitHub license](https://img.shields.io/github/license/Taicicaks01/jejak_kuluk)
![Python version](https://img.shields.io/badge/python-3.6+-blue.svg)

---

**⭐ If you find Jejak_Kuluk useful, please star the repository!**

**🤝 Contributions are welcome! Help us make server security better for everyone.**

---

```
Made with ❤️ by the Hornet Selem Team
For defenders, by defenders.
```
