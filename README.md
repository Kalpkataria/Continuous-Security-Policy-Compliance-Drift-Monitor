# Continuous Security Policy Compliance & Drift Monitor

A Python-based Windows security compliance monitoring system that continuously evaluates endpoint security configurations against a predefined security baseline, identifies configuration drift, manages security alerts, and generates compliance reports.

The project provides a centralized Flask dashboard to monitor Windows security settings and assess their alignment with CIS Controls, NIST SP 800-53 and ISO/IEC 27001.

## Project Overview

Maintaining secure Windows configurations is essential for protecting systems against unauthorized access, malware, misconfiguration and other security risks.

This project automates the assessment of selected Windows security configurations through PowerShell-based collectors and a Python compliance evaluation engine.

It stores historical scan results in SQLite, detects changes between scans, and presents security compliance information through a web-based dashboard.

## Key Features

- **15 Windows Security Controls:** Evaluates selected Windows security configurations.
- **Automated Security Assessment:** Collects configuration information using PowerShell.
- **Compliance Evaluation Engine:** Compares actual configurations against a predefined JSON security baseline.
- **Configuration Drift Detection:** Identifies security configuration changes between scans.
- **Security Alert Management:** Maintains security alerts with Open, Acknowledged and Resolved statuses.
- **Remediation Guidance:** Provides recommended actions, risk information and verification steps.
- **Framework Mapping:** Maps selected controls to CIS Controls, NIST SP 800-53 and ISO/IEC 27001.
- **SQLite Database:** Stores scan history, control results and security alerts.
- **Interactive Dashboard:** Displays compliance statistics, security controls and alert information.
- **Compliance Trend Analysis:** Visualizes compliance scores across historical scans.
- **Report Generation:** Generates PDF and CSV compliance reports.

## Windows Security Controls

The system currently evaluates the following 15 controls:

| No. | Security Control | Purpose |
|---|---|---|
| 1 | Windows Firewall | Checks whether firewall profiles are enabled. |
| 2 | Microsoft Defender Antivirus | Checks antivirus protection status. |
| 3 | Defender Real-Time Protection | Checks real-time protection status. |
| 4 | Windows Event Log Service | Checks whether the Event Log service is running. |
| 5 | User Account Control (UAC) | Checks whether UAC is enabled. |
| 6 | Guest Account Security | Checks whether the built-in Guest account is disabled. |
| 7 | SMBv1 Protocol Security | Checks whether the legacy SMBv1 protocol is disabled. |
| 8 | Windows Update Service | Checks whether the Windows Update service is disabled. |
| 9 | Automatic Updates Policy | Evaluates the configured automatic update policy. |
| 10 | PowerShell Script Block Logging | Checks whether Script Block Logging is enabled. |
| 11 | Windows Logon Audit Policy | Checks successful logon auditing configuration. |
| 12 | Remote Desktop Security | Checks whether Remote Desktop is disabled. |
| 13 | Automatic Screen Lock | Checks the configured machine inactivity timeout. |
| 14 | Minimum Password Length | Checks the minimum local password length. |
| 15 | Account Lockout Threshold | Checks the configured account lockout threshold. |

*Note: These are selected baseline checks, not an exhaustive assessment of every Windows security setting.*

## Configuration Drift Detection

The system compares the latest reliable compliance result for each control with its current result.

| Previous Status | Current Status | Drift Result |
|---|---|---|
| PASS | PASS | NO_CHANGE |
| PASS | FAIL | DRIFT_DETECTED |
| FAIL | PASS | RECOVERED |
| FAIL | FAIL | NO_CHANGE |
| No previous result | PASS or FAIL | NO_PREVIOUS_SCAN |
| Any previous result | ERROR | UNKNOWN |

This allows administrators to identify configuration changes that may affect endpoint security.

## Technology Stack

| Technology | Purpose |
|---|---|
| Python | Core programming language |
| Flask | Web dashboard and application backend |
| PowerShell | Windows security configuration collection |
| SQLite | Scan history and alert storage |
| HTML5 | Dashboard structure |
| CSS3 | Dashboard styling |
| JavaScript | Interactive dashboard functionality |
| Chart.js | Compliance trend visualization |
| JSON | Security baseline and framework mappings |
| Git & GitHub | Version control and project hosting |

## Project Architecture

```text
Security-Compliance-Monitor/
│
├── collectors/
│   ├── windows_collector.py
│   └── windows_event_collector.py
│
├── compliance/
│   ├── baseline.json
│   ├── engine.py
│   ├── storage.py
│   ├── frameworks.py
│   ├── framework_mapping.json
│   ├── alerts.py
│   ├── alert_storage.py
│   └── remediation.py
│
├── reports/
│   └── generate_reports.py
│
├── templates/
│   └── index.html
│
├── automated_scanner.py
├── simulate_attack.py
├── test_alert_management.py
├── test_alert_storage.py
├── app.py
├── compliance.db
└── README.md
```

The local database and virtual environment should not be committed to GitHub. The database shown above is created locally when the application runs.

## Installation and Setup

### Prerequisites

- Windows 10 or Windows 11
- Python 3.10 or later
- Git
- Visual Studio Code (recommended)
- Windows PowerShell

### 1. Clone the repository

```bash
git clone https://github.com/Kalpkataria/Continuous-Security-Policy-Compliance-Drift-Monitor.git
```

### 2. Navigate to the project directory

```bash
cd Continuous-Security-Policy-Compliance-Drift-Monitor
```

### 3. Create a virtual environment

```bash
python -m venv venv
```

### 4. Activate the virtual environment

```powershell
.\venv\Scripts\Activate.ps1
```

### 5. Install dependencies

If a `requirements.txt` file is included:

```bash
pip install -r requirements.txt
```

Otherwise, install the dependencies specified by the project, including Flask and any libraries used by the report-generation and notification modules.

### 6. Run the compliance assessment

```bash
python -m compliance.engine
```

This collects Windows security configuration information, evaluates the baseline and stores scan results in SQLite.

### 7. Start the Flask dashboard

```bash
python app.py
```

Open your browser and navigate to:

```text
http://127.0.0.1:5000
```

## Dashboard Functionality

The dashboard provides:

- Overall compliance score.
- Total, assessed, passed, failed and error controls.
- Security configuration drift information.
- Framework mappings for security controls.
- Security alert overview.
- Alert filtering by status and severity.
- Alert acknowledgement and resolution.
- Remediation recommendations.
- Previous security scan history.
- Compliance trend analysis.
- PDF and CSV report downloads.

## Security Alert Simulation

The project includes a simulation script for generating sample security alerts.

Run:

```bash
python simulate_attack.py
```

This creates synthetic alert records for testing the alert management and remediation functionality.

**Important:** Simulated alerts are test records. They do not represent actual attacks or compromise the Windows system.

## Compliance Score Calculation

The compliance score is calculated using successfully assessed controls:

```text
Compliance Score = (Passed Controls / Assessed Controls) × 100
```

ERROR results are excluded from the denominator because their security settings could not be evaluated.

For example:

```text
Total Controls:      15
Assessed Controls:  12
Passed Controls:    10
Failed Controls:     2
Errors:              3

Compliance Score = (10 / 12) × 100

Compliance Score = 83.3%
```

## Important Notes

- The project performs read-only Windows configuration checks.
- Some Windows settings may be unavailable depending on Windows edition, permissions or system configuration. Such results may be reported as ERROR.
- Compliance results depend on the configured baseline and the accuracy of the collected information.
- Framework mappings are reference alignments and do not independently establish formal certification or compliance.
- The project is a Windows endpoint configuration compliance monitor, not a full SIEM or endpoint detection and response platform.
- The simulated alerts are intended for development and testing.

## Future Enhancements

Potential future improvements include:

- Integration with centralized SIEM platforms.
- More comprehensive Windows security baseline checks.
- Improved event log collection and correlation where permissions allow.
- Automated scheduled compliance scanning.
- Email and notification-based alerting.
- Historical drift analytics.
- Additional security framework mappings.
- Multi-endpoint monitoring.

## Author

**Kalp Kataria**

M.Sc. Digital Forensics Science and Information Security  
National Forensic Sciences University (NFSU), Gandhinagar

GitHub: https://github.com/Kalpkataria

LinkedIn: https://www.linkedin.com/in/kalp-kataria-b6a259242/

---

**If you find this project useful, consider giving the repository a star!**
