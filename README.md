
For this project, I built a real‑time log analyzer and security event detection system for the Raspberry Pi. The system continuously collects system logs, parses them into a structured format, stores them in a SQLite database, and analyzes the events to identify potentially malicious activity. When suspicious patterns are detected—such as multiple failed SSH login attempts—the analyzer generates alerts that include the source IP, target user, number of attempts, timestamp, and severity level. Unauthorized login attempts are extremely common on Linux devices, especially exposed or low‑cost systems like Raspberry Pis, so this tool is designed to detect and surface those threats as soon as they occur.

        The Directory Structure

loganalyzer

  ->loganalyzer 
  
-->analyzer.py

-->collector

---->collector_local.py

-->parser

---->parser.py

-->guides

---->guides.py

-->alerts

---->alert_storage.py

-->storage

---->storagedb.py

-->events.db

->raw_logs

-->auth.log.in

-->syslog.in 

->test_logs

-->test1.log

-->test2.log




         The Set Up

Raspberry Pi 4

Monitor

Raspbian OS

SSH enabled 

Python3 with sqlite3, dateuil, and pathlib 






        The Steps

  The Analyzer:
  
This reaches out to each of the components and loops constantly. The components are not separate and do not interact with each other when it's not through the analyzer. 

  Collector:
  
The collector uses a thread per log file and then follows them, similar to tail -f. When a SSH attack message appears, it immediately gets copied to “raw_logs/auth.log.in”. 

  Parser:
  
In the parser, every line is split into the three parts of timestamp, hostname, rest_of_line. Inside the rest the regular expression extracts the user, source IP, and event type. 

  Storage:
  
Each event directory is inserted into a previously set events table. 

  Guides:
  
The guide decides if the event is malicious enough to generate an alert. If it is, it will generate one. This is determined if any IP/user pair exceeds the threshold >= 1 during testing. 

  Alerts:
  
The alerts are stored in the previously set alerts table and print to the console in real time. 

        Security Considerations
->Sensitive Log Data

System logs may contain usernames, IP addresses, and other private information. Ensure that log files and the SQLite database are readable only by authorized users.

->Database Permissions

The events.db file should be protected with strict file permissions to prevent unauthorized access.

->Log Flooding Risks

Excessive or malicious log activity can overwhelm the system. Consider rate‑limiting, deduplication, or thresholds to reduce noise.

->Pipeline Integrity

If logs or analyzer code are modified, real alerts could be suppressed. Keep the project directory secured and restrict write access.

->Detection, Not Prevention

This tool identifies attacks but does not block them. Users should still secure SSH with strong passwords, key‑based authentication, and IP restrictions.


