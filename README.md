# SOC Attack Simulation and Log-Based Incident Analysis

## Overview
This project demonstrates the simulation and analysis of common cyber attack patterns in a controlled lab environment. The focus is on understanding attacker behavior and identifying detection patterns through system logs, similar to real-world Security Operations Center (SOC) workflows.

## Objectives
* Simulate real-world attack scenarios
* Analyze system logs to identify malicious activity
* Understand attack patterns and detection techniques
* Develop SOC-level incident analysis skills

## Lab Environment

* **Attacker Machine:** Kali Linux
* **Target Machine:** Ubuntu
* **Network Setup:** Virtualized (NAT-based internal network)

## Simulated Attack Scenarios
**1. SSH Brute Force Attack -** 
* Multiple login attempts were generated using automated password guessing.
* Logs showed repeated authentication failures from a single source.

**2. Port Scanning (Reconnaissance)**
* Target system was scanned to identify open ports and services.
* Demonstrates attacker reconnaissance phase.

## Log Analysis Approach
* Monitored authentication logs (/var/log/auth.log).
* Identified repeated failure patterns.
* Correlated activity with attack behavior.
* Anonymized sensitive data before documentation.

## Key Learnings
* Attack detection relies on behavior patterns, not just tools.
* Logs provide critical insight into system activity.
* Not all attacks are explicitly logged (e.g., port scanning).
* SOC analysis involves correlation, not just observation.

*Note* - All activities were performed in a controlled lab environment. The detailed implementation of the attacks and log analysis is in the attack-process folder. 

This project highlights how common attack techniques can be identified through log analysis and behavioral patterns, reinforcing the importance of monitoring and detection in cybersecurity operations.
