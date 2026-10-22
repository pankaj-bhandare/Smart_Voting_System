# SmartVote Pro — Research & Election Lab

SmartVote Pro is an **educational desktop prototype** built with Python, Tkinter, OpenCV and SQLite. It is not certified or suitable for real elections.

## What's included

- Clean academic light interface with tabbed workflows and status cards.
- Fictional voter registry, webcam face enrollment, and experimental face matching.
- SQLite-backed election lifecycle and a database-level unique participation record per voter per election.
- Atomic electronic ballot plus simulated VVPAT-style paper-record creation.
- Reconciliation of electronic and simulated paper totals, record hashes, and audit-chain integrity.
- SHA-256 hash-linked audit log, SQLite integrity checks, and foreign-key checks.
- Optional offline voice announcements and accessibility preferences.
- Administrator PIN re-authentication for sensitive actions (election changes, voter registry changes, candidate management, backups and restores).
- Research Lab with aggregate health checks, regression-test runner, face-trial CSV analysis, and aggregate-only evaluation JSON export.
- Separate fictional demo database mode that never opens the primary database.

## Windows setup

1. Install Python 3.10–3.14 (64-bit) and enable **Add Python to PATH**.
2. Extract this folder to a writable location.
3. Double-click `run_windows.bat` for the normal project database (`smartvote.db`).
4. Double-click `run_demo.bat` to use an isolated fictional database (`smartvote_demo.db`). This is the preferred mode for demonstrations and experiments.

PowerShell alternative:

```powershell
py -m venv .venv
.\.venv\Scripts\python.exe -m pip install -r requirements.txt
.\.venv\Scripts\python.exe smartvote.py
```

To launch the isolated demo manually:

```powershell
.\.venv\Scripts\python.exe smartvote.py --demo
```

### Data protection

- The primary database is `smartvote.db`; isolated demo mode uses `smartvote_demo.db`.
- The first demo launch creates the demo database if it does not exist and seeds fictional sample voters. It does not copy or migrate data from `smartvote.db`.
- Use the normal mode only for your existing project data. Keep an independent backup before upgrading or restoring a database.
- Local administrator PIN settings are stored separately for primary and demo mode as `admin_access.json` and `admin_access_demo.json`. The PIN is salted and PBKDF2-HMAC-SHA256 hashed; it is not stored in plaintext.
- PIN re-authentication is a local application safeguard, not a strong OS security boundary. A user with access to the project files and Python environment can alter the application or settings.
- Backups include local face samples. Store them securely and do not share them with reports.

## Using the app

1. **Overview** shows current election and registry counts.
2. **Research Lab** runs aggregate system checks, launches the regression test suite, analyzes a CSV of measured face-verification trials, and exports an aggregate JSON report.
3. **Security Centre** checks SQLite integrity, foreign keys, audit-chain validity and reconciliation.
4. **Backup & Recovery Lab** inspects backup SHA-256, SQLite integrity, foreign keys and audit-chain health; runs a temporary restore drill without touching the live database; and creates/verifies external audit checkpoints.
5. **VVPAT & Reconciliation** compares electronic ballot totals with simulated paper records.
6. **Voter Registry** manages fictional voters. Sensitive changes require administrator PIN re-authentication.
7. **Face Enrollment** captures grayscale face samples from the webcam.
8. **Voting Booth** performs experimental face matching followed by manual confirmation and one-vote enforcement.
9. **Results**, **Audit Log**, **Voice & Notifications**, and **Admin Controls** support the remaining workflow.

### First administrator action

On the first sensitive operation, the app asks you to create an administrator PIN of at least 8 characters and confirm it. That first action is not performed yet; repeat it after the PIN is created. You can also create or change the PIN using **Admin Controls → Set / change admin PIN**. Primary and isolated demo modes use separate PIN files.

### Keyboard shortcuts

- `Ctrl+R`: refresh data
- `Ctrl+1`: Overview
- `Ctrl+2`: Security Centre
- `Ctrl+3`: Research Lab
- `Ctrl+4`: Backup & Recovery Lab

## Face-verification evaluation

Use `evaluation/face_verification_trials_template.csv` as a blank template. Record only consented, controlled tests; do not put real biometric samples or personal identifiers in the CSV. The required columns are `trial_id`, `trial_type`, `expected_accept`, `observed_accept`, `latency_ms`, and `notes`.

Analyze real observations with:

```powershell
.\.venv\Scripts\python.exe evaluation\analyze_face_trials.py evaluation\face_verification_trials.csv
```

The analyzer reports false acceptance rate (FAR), false rejection rate (FRR), overall trial accuracy and mean latency. Do not report invented results. Include the number of trials, camera, lighting, distance and limitations. Small samples do not establish biometric security.

## Tests

Run from this folder:

```powershell
.\.venv\Scripts\python.exe -m unittest discover -s tests -v
```

The automated tests use temporary databases and cover voting transactions, duplicate-vote prevention, paper reconciliation, audit tampering, backup/restore, migration preservation, privacy-minimized reports, face-trial metrics, administrator PIN hashing and database-path isolation. Tests do not prove that a physical webcam or installed speech voice works on every computer; those require manual checks on the target Windows laptop.

## Known limitations

- This is a college demonstration prototype, not a production election system.
- Haar detection and pixel-distance face matching are experimental, have no reliable liveness detection, and can accept impostors or reject genuine users. Manual confirmation is intentionally retained.
- The simulated paper record is not a physical VVPAT or certified paper ballot.
- A local hash chain can detect accidental or unsophisticated edits, but a privileged attacker who can rewrite the database may recompute it. External audit checkpoints can anchor a known audit row and detect changes to that row or chain, provided the checkpoint itself is kept separately and trusted. The checkpoint is not digitally signed; a privileged attacker may replace a checkpoint they can write. Stronger assurance needs cryptographically signed checkpoints and independently protected external audit controls.
- The PIN gate does not replace Windows account security, disk encryption, or OS file permissions.
- Do not use real voter records, government identity numbers, or real biometric samples.

## Project documents

- `docs/FINAL_YEAR_PROJECT_REPORT_GUIDE.md` — report structure and viva preparation.
- `docs/TEST_PLAN.md` — manual and automated test plan.
- `docs/THREAT_MODEL.md` — threat model and residual risks.
- `docs/NEXT_UPGRADE_NOTES.md` — earlier release notes.
- `docs/SECURITY_AND_RECOVERY_LAB.md` — checkpoint and recovery-drill procedures.
- `evaluation/analyze_face_trials.py` — face-verification metrics analyzer.
# Smart_Voting_System
