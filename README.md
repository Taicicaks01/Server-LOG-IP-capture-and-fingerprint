scans common log locations, Docker container logs, systemd/journal, Cloudflared service logs, and SSH/auth logs. Output: a single CSV /root/homelab-fingerprints/YYYYMMDD_fingerprints.csv plus a short summary printed to the terminal. You can run it manually or cron it daily.

What it captures:
•	timestamp (when request/event logged)
•	service (nginx, casaos, cloudflared, docker:<container>, sshd, journal)
•	src_ip (client IP from log or connection)
•	client_ip_header (CF-Connecting-IP / X-Forwarded-For if present)
•	method (HTTP method when available)
•	uri (path)
•	status (HTTP status)
•	user_agent (if present)
•	extra (raw matched line)
•	(optional) ja3 hash if tshark + ja3 plugin available and pcap files exist

How to use
1.	Save the script as /root/collect-fingerprints.sh
2.	Make executable: sudo chmod +x /root/collect-fingerprints.sh
3.	Run it: sudo /root/collect-fingerprints.sh
4.	Results will be under /root/homelab-fingerprints/ and the CSV path will be printed at the end.
