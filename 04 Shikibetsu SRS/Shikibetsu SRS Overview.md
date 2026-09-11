---
tags: [project, srs, shikibetsu, software-design]
version: "2.1"
date: 2026-05-06
authors:
  - Cabillon, Jerxel
  - Castillo, Mark Vince
  - Llanes, Kharl
  - Mira, Gabriel Melvincent
  - Nuevaorlanda, Raekwon Benedict
  - Oliva, Karl Alfonso
---

# Shikibetsu: The Phishing Identification Game — SRS Overview

> [!info] Relationship to the research topic
> Shikibetsu is the applied software project built to test [[Karl Overview|Karl's research topic]] on gamified phishing-identification training. "Shikibetsu" is Japanese for identification/discernment.

## Purpose & Scope
A Windows application that trains graduating college students to distinguish authentic from malicious digital content, using *Papers, Please*-style gameplay ([[Pope 2013]]): the game shows randomized files/emails, the player decides if they're malware, and scores them on accuracy.

**In scope:** ransomware and spyware delivered via email links/attachments.
**Explicitly out of scope:** non-malware technical issues, offline social engineering (tailgating, vishing calls), and network-level security (firewalls, VPN configuration).

The goal is to turn abstract cybersecurity awareness into an intuitive, muscle-memory identification skill — directly testing the premise behind [[Awareness-Behavior Gap]] and [[Phishing Simulation Training Effectiveness]].

## Product Perspective
Standalone Windows app exposing users to simulated job-recruitment phishing scenarios in a safe, game-ified environment, to build early-exposure familiarity with real-world attack patterns.

## Product Features
**System-side:**
- **Email Display** — randomly generates scenarios (emails, links, files) representing spam, scams, phishing, or legitimate requests.
- **Decision Validation** — checks the player's decision against ground truth and updates score.
- **Report Generation** — compiles per-scenario performance into a feedback summary.

**User-side:**
- **Session Management** — new game or resume from saved progress.
- **Threat Inspection** — interactive tools to analyze emails/links/attachments.
- **Performance Review** — end-of-level report review.
- **Leaderboard** — view and submit scores online.

**Admin-side:**
- View leaderboard.
- Remove entries for suspected cheating (external tools modifying scores) or offensive submitted names.

## User Characteristics
Target users: graduating college students actively job-hunting — high digital literacy (can operate a Windows app), at least limited prior knowledge of email-based malware.

## Operating Environment
- 64-bit Windows 11+, 1.5GHz dual-core CPU, 2GB RAM, OpenGL 3.3-capable GPU, 500MB storage.
- Specs adapted from the *Papers, Please* Steam requirements, tuned for the Godot Engine.
- Internet connection required only for cloud save sync and leaderboard access.

## Constraints
- **Technology:** Godot/Unity engine; Java or SQL for minor features/data; AWS or Oracle Cloud for hosting (pay-as-you-go, beginner-friendly).
- **Financial:** free/open-source tools preferred; budget mainly reserved for cloud hosting.
- **Time:** one to two academic terms — hence the narrow scope of ransomware/spyware only.
- **Gameplay:** strictly "identification and sorting" mechanics, for feasibility and educational focus.

## Interface Structure
Office-themed UI simulating a professional workstation:
1. **Main Menu** — Start / Continue / Leaderboard / Exit, with hover tooltips.
2. **Leaderboard Screen** — read-only ranked entries (rank, name, shifts completed, tasks processed, score); the player's own entry is visually distinguished.
3. **Main Gameplay Screen** — Content Display Panel (emails/links/attachments), Decision Panel (Accept/Reject, one decision required per scenario), Inspection Panel (Check URL / Analyze Attachments / Verify Sender), Workload Indicator (quota + progress bar), Attachment Inspector (metadata + scan), Reference Panel (verified addresses/domains/safe file types).
4. **Pause Menu** — Resume / Retry / Quit, gameplay dimmed but visible behind it.
5. **End-of-Scenario Screen** — correct/incorrect tally, generated report, options to submit to leaderboard, advance, retry, or return to menu.

## Core Functional Flow
1. **Email Generation** — a scenario is drawn from a pool; it's tagged internally as "malware" if it meets criteria like a misspelled domain, a positive preliminary virus scan, or an improperly formatted sender name.
2. **Malware Identification** — the player uses the Inspection Panel (URL/sender check against a verified-domain list; attachment metadata + scan) to gather evidence before deciding.
3. **Warning Sign Look-Up** — a reference database of common malware templates and known bad sender addresses supports the player's decision.
4. **Report Generation** — points awarded/deducted per field and per email; **60% correct identification** is the pass/fail threshold. Passing unlocks leaderboard submission, next level, or retry-for-score; failing offers retry or quit.

## Nonfunctional Requirements
- **Performance:** single-player focus; malware-check processing can take up to 5 seconds — response time doesn't need to be instant.
- **Security:** AES-256 encryption, TLS 1.3 for any transmission, role-based access control with multi-factor authentication, immutable audit logs (7-year retention), supervisor approval for data exports, quarterly penetration testing. See [[Lee 2025]] and [[Panyam Bhattacharya Gujar 2024]].
- **Reliability:** auto-save every 5 minutes; basic error handling to prevent data loss on unexpected close; local execution with manual backup options.
- **Maintainability:** clean architecture with separated modules (document verification, UI, data storage); Git for version control; cross-platform framework (Java/Python/HTML-CSS-JS); JSON save files for portability/debuggability.

## Testing Approach
- **Unit testing:** Main Menu, Email Display, Decision Validation, Scoring Logic, Report Generation modules tested individually.
- **Integration testing:** Scoring + Gameplay Interface + Feedback + Navigation tested together (e.g., decisions correctly update score in real time; reports match gameplay results).
- **Acceptance testing:** target users must reach ≥60% accuracy, navigate without confusion as first-time users, show improvement across multiple levels, and report the game as understandable and engaging.

## Related
- [[Karl Overview]]
- [[Karl Justification]]
- [[Employment Scams Targeting Graduates]]
- [[Pope 2013]]
- [[Lee 2025]]
- [[Panyam Bhattacharya Gujar 2024]]
- [[Ngwese 2025]]
- [[Alahmari 2022]]
- [[Godot Engine System Requirements]]
- [[Cloudflare - What is TLS]]
