# ssh-honeypot-classifier
Classifies SSH honeypot (Cowrie) sessions into Reconnaissance, Credential Brute-Forcing and Payload Deployment using Random Forest and XGBoost, with a near-real-time Streamlit dashboard. UNITEN Final Year Project (work in progress).

# AI-Assisted Attacker Behaviour Classification for SSH Honeypot Logs

Final Year Project, Bachelor of Computer Science (Cyber Security) (Hons.), Universiti Tenaga Nasional (UNITEN)

**Student:** Clarice Dara
**Supervisor:** Assoc. Prof. Ts. Dr. Ravie Chandren Muniyandi

> **Work in progress.** FYP 1 (design) is complete. The system is being built and tested in FYP 2. No results are reported yet.

## About

SSH servers are probed and brute-forced constantly. A honeypot such as [Cowrie](https://github.com/cowrie/cowrie) records every attacker session, but analysts still read those logs by hand, label the same behaviour in different ways, and can miss multi-stage attacks.

This project builds a system that sorts each SSH honeypot session into a behaviour class automatically and shows the results on a dashboard in near real time.

## Behaviour classes

These three classes are defined for this project. The labelling rules are informed by MITRE ATT&CK.

| Class | What it covers |
|---|---|
| Reconnaissance | Looking around the system: listing files, checking users, hardware or network |
| Credential Brute-Forcing | Repeated login attempts within a session, or many sessions from one source |
| Payload Deployment | Trying to download, install or run malware, including failed attempts |

## System design

1. **Tier 1: Collect.** A Cowrie SSH honeypot on a Google Cloud VM records attacker sessions.
2. **Tier 2: Process and classify (Python).** Logs are copied to a separate private backend, grouped into sessions, decoded (Base64), turned into features (behaviour statistics + character n-gram TF-IDF), classified, and stored in SQLite.
3. **Tier 3: Show.** A read-only Streamlit dashboard, reached only through an SSH tunnel.

Attackers can only reach Tier 1. Data and models stay on the private backend. Models are trained offline.

## Method

- About 300 sessions labelled by hand, with part kept aside as an independent test set
- Random Forest vs XGBoost
- Resampling methods (such as SMOTE) compared to deal with rare classes
- Time-based train/test split
- Evaluated with macro-F1, MCC and per-class recall, and compared against the labelling rules alone

## Tech stack

Cowrie · Google Cloud · Python · scikit-learn · XGBoost · SQLite · Streamlit

## Progress

- [x] Problem, scope and literature review
- [x] Requirements and three-tier system design
- [x] Session features and labelling rules
- [ ] Deploy honeypot and collect data
- [ ] Build processing pipeline and dashboard
- [ ] Label sessions, train and compare models
- [ ] Test and measure dashboard delay

## Note on data

Raw honeypot logs, server addresses and credentials are **not** published in this repository.

** [View the dashboard prototype](https://claaricedara.github.io/ssh-honeypot-classifier/)**
