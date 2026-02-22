🛠 Windows 11 Servicing Incident – 0x800f0991 Recovery (Build 26200)
📌 Incident Summary

System: Dell Laptop
OS: Windows 11 Home
Branch: 26200 (Canary lineage)
Initial Build: 26200.7171
Error: 0x800f0991
Symptom: Cumulative update failing at ~79% during commit phase

Root cause identified as servicing baseline misalignment combined with orphaned satellite payload components.

🔎 Initial Findings
dism /online /cleanup-image /checkhealth

Result:

The component store is repairable.

CBS logs previously showed satellite language payload corruption (non-core).

🧪 Phase 1 – Component Repair
dism /online /cleanup-image /restorehealth
sfc /scannow

Result:

No component store corruption detected.

Update still failed at 79% → confirmed deeper baseline issue.

🧠 Root Cause

Device running 26200 Canary-family build

Insider ring metadata missing

Cumulative update attempting commit on misaligned servicing baseline

Result: Late-stage 0x800f0991

🔧 Phase 2 – Controlled Baseline Realignment

Downloaded matching Windows 11 Insider Preview Build 26200 ISO

Performed In-Place Upgrade:

Mounted ISO

Ran setup.exe

Selected:

✔ Keep personal files and apps

✔ No updates during setup

Resulting Build:

26200.5074
🔄 Phase 3 – Clean Update Chain

Post-repair:

Windows Update detected correctly

Cumulative KB5064081 installed successfully

No rollback

No servicing corruption

System stable

✅ Final State

Build: 26200.5074

Component Store: Healthy

Servicing Stack: Aligned

Update Pipeline: Functional

No 0x800f0991

🧠 Lessons Learned

0x800f0991 at 80–90% often indicates commit-phase baseline misalignment

DISM repair alone is insufficient in branch metadata mismatch scenarios

In-place upgrade with matching build ISO is deterministic recovery

Never stack preview updates during instability

Controlled sequencing prevents escalation
