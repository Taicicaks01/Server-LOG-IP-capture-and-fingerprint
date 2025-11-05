a single-file terminal TUI tool (like htop but for fingerprints/logs) that you can run on your Ubuntu homelab. It shows live, updating panels:
•	Top source IPs (count)
•	Top User-Agents
•	Top endpoints (URIs)
•	Recent suspicious lines (from journal/docker/nginx/auth)
•	Quick stats (lines scanned, last refresh time)
Controls: q to quit, r to force-refresh, s to save a snapshot CSV to /root/homelab-fingerprints/.
No external Python packages required — only Python 3 (stdlib). Save the script, make it executable, run with sudo for full log access.

1. Script — save as /root/logwatch_tui.py || or save as "sudo logwatch_tui.py"
2. Make it executable & run : sudo chmod +x /root/logwatch_tui.py || or sudo chmod +x logwatch_tui.py

you can run the code using this command "sudo logwatch_tui.py"
