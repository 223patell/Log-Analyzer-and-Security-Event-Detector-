
For this project, I built a real‑time log analyzer and security event detection system for the Raspberry Pi. The system continuously collects system logs, parses them into a structured format, stores them in a SQLite database, and analyzes the events to identify potentially malicious activity. When suspicious patterns are detected—such as multiple failed SSH login attempts—the analyzer generates alerts that include the source IP, target user, number of attempts, timestamp, and severity level. Unauthorized login attempts are extremely common on Linux devices, especially exposed or low‑cost systems like Raspberry Pis, so this tool is designed to detect and surface those threats as soon as they occur.

#Directory Structure
loganalyzer
  loganalyzer 
    analyzer.py
    collector
      collector_local.py
    parser
      parser.py
    guides
      guides.py
    alerts
      alert_storage.py
    storage
      storagedb.py
    events.db
  raw_logs
    auth.log.in
    syslog.in 
  test_logs
    test1.log
    test2.log
