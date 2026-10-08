# Continuous-Security-Policy-Compliance-Drift-Monitor
Policy-as-code compliance engine in Python: validates Linux host security settings against declarative policies, scores compliance and risk, detects configuration drift between scans, and outputs evidence-backed remediation reports.
# Continuous Security Policy Compliance & Drift Monitor

A Python policy-as-code engine that checks whether a system is configured the way
your security policy says it should be, and tells you when that changes.

## What it does
- **Defines policy as data:** security requirements live in JSON rules (setting path,
  operator, expected value, severity, remediation), not in code.
- **Collects real state:** a Linux collector reads SSH, password policy, firewall,
  services, logging, file permissions and kernel settings from the live host.
- **Validates automatically:** each rule is evaluated to PASS or FAIL with evidence
  (e.g. `firewall.enabled = False, expected True`).
- **Scores and rates risk:** simple and severity-weighted compliance scores, plus a
  Low / Medium / High / Critical risk level.
- **Detects drift:** compares each run with the previous one and flags
  REGRESSION, REMEDIATED, VALUE_CHANGED and NEW_RULE events.
- **Reports:** console output, `report.json` and `report.md`, with prioritized fixes.
- **Fits CI/CD:** `--fail-on critical` returns a non-zero exit code; `--watch N`
  re-scans continuously.

## Pipeline
Policy → expected state → collect actual state → compare → PASS/FAIL
→ score + risk + evidence → drift analysis → report / alert

## Quick start
    git clone https://github.com/<your-username>/security-policy-drift-monitor
    cd security-policy-drift-monitor
    python3 monitor.py                                          # demo against sample config
    sudo python3 monitor.py --collect --policy linux_policy.json  # audit this Linux host

## Example output
    FW-001   Host firewall enabled        FAIL  [critical]
    SSH-001  SSH root login disabled      PASS  [critical]
    Compliance score: 77.8% | Risk: Critical
    ! REGRESSION  FW-001: PASS -> FAIL (True -> False)

## Project structure
    monitor.py          engine: evaluation, scoring, risk, drift, reports
    collector.py        Linux host collector
    policy.json         sample policy
    linux_policy.json   CIS-inspired Linux baseline (13 rules)
    sample_config.json  demo target

## Limitations and roadmap
- Linux only; checks that cannot be collected are currently reported as FAIL
  (planned: an UNKNOWN status excluded from scoring).
- Planned: pytest suite, GitHub Actions CI, CIS/NIST control mapping,
  metrics export and webhook alerts.
