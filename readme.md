# Jejak_Kuluk 🔍

```
     ____.       __             __        .__       __        
    |    | ____ |__| ____ |  | __ | | ____ __| |  __ __| |  __ 
    |    |/ __ \|  |/ __ \| |/ / | |/ / |  \ | | |  \ |/ /  
    |    |  ___/ |  (  <_> )   <  |  <|  Y Y  \|  <_> >  <   
    |____|\___  >__|\____/|__|_ \____/|__|_|  /____/|__|_ \ 
             \/              \/          \/        \/        
```

**Created by:** Wayan_Choir  
**Developed by:** Hornet Selem Team  
**License:** MIT  

---

## 📖 Description

**Jejak_Kuluk** is a powerful all-in-one Linux defensive cybersecurity CLI tool designed to monitor, analyze, and fingerprint every action happening on your server. It helps system administrators identify intrusions, failed logins, brute-force attacks, IP origins, and trace unusual network activity — everything you need for forensic analysis and threat detection.

Whether you're managing a production server, investigating a security incident, or conducting routine audits, Jejak_Kuluk provides comprehensive visibility into your system's security posture.

## ✨ Features

| Category | Description |
|----------|-------------|
| 🔍 **Log Analyzer** | Read & display logs from `/var/log/auth.log`, syslog, nginx, apache, and more |
| 🧠 **Anomaly Detection** | Detect failed logins, brute force attacks, SSH abuse, and strange access patterns |
| 🌐 **IP & Geo Lookup** | Show IP origin (country, city, ASN/organization) using GeoIP |
| ⚠️ **Risk Scoring** | Calculate and display threat levels: 🟩 Safe 🟨 Suspicious 🟥 Attack |
| 🕒 **Timeline Monitor** | Show chronological activities in real-time |
| 🚨 **Live Alert Mode** | Highlight new or unknown IPs in red with continuous monitoring |
| 📜 **Fingerprint Capture** | Record IP, port, timestamp, username, user-agent, and raw log entries |
| 📊 **Interactive Dashboard** | Terminal UI dashboard showing top suspicious IPs and statistics |
| 🧾 **Export Reports** | Export daily reports in JSON format for further analysis |
| 🛰️ **Scapy Integration** | Detect port scans and strange network traffic (optional) |
| 💾 **SQLite Database** | Store historical events for forensic lookup and analysis |
| 👁️ **Traceback Mode** | Backtrack intruder fingerprints using combined data (logs + network) |

## 🚀 Installation

### Requirements

- **Python 3.7+**
- **Root/Sudo access** (for reading system logs)
- **Linux** (tested on Ubuntu, Debian, CentOS)

### Quick Install

#### One-line installer:
```bash
sudo curl -L https://github.com/wayanchoir/jejak_kuluk/raw/main/jejak_kuluk.py -o /usr/local/bin/jejak_kuluk && sudo chmod +x /usr/local/bin/jejak_kuluk
```

#### Manual Installation:

1. **Clone the repository:**
   ```bash
   git clone https://github.com/wayanchoir/jejak_kuluk.git
   cd jejak_kuluk
   ```

2. **Install dependencies:**
   ```bash
   pip3 install rich geoip2 scapy
   ```
   
   Or using requirements file:
   ```bash
   pip3 install -r requirements.txt
   ```

3. **Make executable:**
   ```bash
   chmod +x jejak_kuluk.py
   ```

4. **Optional: Install globally**
   ```bash
   sudo cp jejak_kuluk.py /usr/local/bin/jejak_kuluk
   ```

### Optional Dependencies

- **GeoIP Database** (for IP geolocation):
  ```bash
  # Ubuntu/Debian
  sudo apt install geoipupdate
  
  # Download GeoLite2 City database
  # Register at https://dev.maxmind.com/geoip/geolite2-free-geolocation-data
  # and follow setup instructions
  ```

- **Scapy** (for network monitoring):
  ```bash
  pip3 install scapy
  # May require additional system packages:
  sudo apt install tcpdump libpcap-dev
  ```

## 📚 Usage

### Basic Commands

```bash
# Analyze system logs
sudo jejak_kuluk analyze

# Live monitoring mode
sudo jejak_kuluk live

# Interactive dashboard
sudo jejak_kuluk dashboard

# Export report
sudo jejak_kuluk export

# Trace specific IP
sudo jejak_kuluk trace 192.168.1.100

# Show help
jejak_kuluk --help
```

### Command Examples

#### 1. **Analyze Logs**
Scan and analyze system logs for security events:
```bash
sudo jejak_kuluk analyze --lines 1000
```
This will:
- Parse auth.log, syslog, nginx, and apache logs
- Detect failed login attempts
- Identify brute force attacks
- Display risk scores for each event

#### 2. **Live Monitoring**
Monitor logs in real-time and get instant alerts:
```bash
sudo jejak_kuluk live --interval 3
```
Features:
- 🆕 Highlights new IPs in red
- ⚠️ Shows risk levels for each event
- 🔄 Auto-refreshes every N seconds

#### 3. **Interactive Dashboard**
View top suspicious IPs and statistics:
```bash
sudo jejak_kuluk dashboard
```
Displays:
- Top 15 suspicious IP addresses
- Total and failed attempt counts
- Geographic locations
- First and last seen timestamps

#### 4. **Export Reports**
Generate forensic reports for documentation:
```bash
sudo jejak_kuluk export
```
Creates a JSON file with:
- All events from last 24 hours
- IP addresses and locations
- Risk scores and event types
- Timestamp: `jejak_kuluk_report_20250106_143022.json`

#### 5. **IP Tracing**
Investigate a specific IP address:
```bash
sudo jejak_kuluk trace 203.0.113.42
```
Shows:
- Geographic location (city, country)
- ASN/Organization
- All historical events from this IP
- Risk assessment

## 📁 Directory Structure

```
jejak_kuluk/
├── jejak_kuluk.py          # Main application (single file)
├── README.md               # This file
├── LICENSE                 # MIT License
└── requirements.txt        # Python dependencies
```

The tool creates a SQLite database at `~/.jejak_kuluk.db` to store event history.

## 🎯 Use Cases

### 1. **Security Auditing**
- Daily security checks
- Compliance reporting
- Incident investigation

### 2. **Intrusion Detection**
- Real-time SSH brute force detection
- Port scan identification
- Suspicious IP tracking

### 3. **Forensic Analysis**
- Historical event lookup
- IP reputation tracking
- Attack pattern analysis

### 4. **System Administration**
- Monitor user logins
- Track failed authentication attempts
- Identify compromised accounts

## 🔧 Configuration

### Log Paths
The tool monitors these log files by default:
- `/var/log/auth.log` - SSH and authentication logs
- `/var/log/syslog` - System logs
- `/var/log/nginx/access.log` - Nginx web server
- `/var/log/nginx/error.log` - Nginx errors
- `/var/log/apache2/access.log` - Apache web server
- `/var/log/apache2/error.log` - Apache errors

### Database
Events are stored in SQLite at: `~/.jejak_kuluk.db`

To reset the database:
```bash
rm ~/.jejak_kuluk.db
```

## 🛡️ Security Considerations

- **Run with sudo**: Log files require root access
- **Sensitive data**: Reports may contain IP addresses and usernames
- **Network monitoring**: Scapy features require elevated privileges
- **Log retention**: Database grows over time; implement rotation if needed

## 🤝 Contributing

We welcome contributions! Here's how you can help:

1. **Fork the repository**
2. **Create a feature branch**: `git checkout -b feature/amazing-feature`
3. **Commit your changes**: `git commit -m 'Add amazing feature'`
4. **Push to the branch**: `git push origin feature/amazing-feature`
5. **Open a Pull Request**

### Areas for Contribution
- Additional log parsers (Docker, PostgreSQL, etc.)
- Integration with SIEM systems
- Enhanced anomaly detection algorithms
- Web dashboard interface
- Alert notification system (email, Slack, Telegram)
- Machine learning for threat detection

## 🐛 Bug Reports

Found a bug? Please open an issue with:
- Description of the problem
- Steps to reproduce
- Expected vs actual behavior
- System information (OS, Python version)
- Log snippets (if applicable)

## 📝 Changelog

### v1.0.0 (2025-01-06)
- Initial release
- Core log analysis functionality
- Live monitoring mode
- Interactive dashboard
- IP geolocation support
- SQLite event storage
- Export to JSON
- IP tracing capability

## 🙏 Credits

**Created by:** Wayan_Choir  
**Developed by:** Hornet Selem Team  

Special thanks to:
- The Python community
- Rich library by Will McGugan
- MaxMind for GeoLite2 database
- All contributors and testers

## 📄 License

MIT License

Copyright (c) 2025 Wayan_Choir

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

## 🌟 Star Us!

If you find Jejak_Kuluk useful, please give us a ⭐ on GitHub!

## 🔗 Links

- **GitHub Repository**: https://github.com/wayanchoir/jejak_kuluk
- **Issue Tracker**: https://github.com/wayanchoir/jejak_kuluk/issues
- **Documentation**: https://github.com/wayanchoir/jejak_kuluk/wiki

---

**Made with ❤️ by Hornet Selem Team**
