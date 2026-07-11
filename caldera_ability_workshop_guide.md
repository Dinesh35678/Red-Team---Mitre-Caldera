# MITRE Caldera — Beginner's Guide to Building Abilities Across the ATT&CK Kill Chain

**Audience:** Workshop students new to Caldera and MITRE ATT&CK
**Environment:** Isolated lab only — never point these commands at systems you don't own
**Goal:** Understand the Ability structure, see a working example for every major ATT&CK tactic, and build your own

---

## 1. Before You Start

### 1.1 What is an Ability?
An **Ability** is Caldera's smallest building block — a single YAML-defined action mapped to one MITRE ATT&CK technique. Chain several abilities together and you get an **Adversary profile** (a full attack chain). Run that profile against agents and you get an **Operation**.

### 1.2 Anatomy of an Ability

```yaml
id: <GENERATE-UUID>
name: <short descriptive name>
description: <what this ability does and why>
tactic: <ATT&CK tactic, lowercase-with-dashes>
technique:
  attack_id: <ATT&CK technique ID, e.g. T1059>
  name: <official technique name>

platforms:
  windows:
    cmd:
      command: <the actual command to run>
```

| Field | Meaning |
|---|---|
| `id` | Unique UUID — generate one at [uuidgenerator.net](https://www.uuidgenerator.net/) or with `uuidgen` in a terminal |
| `tactic` | Which ATT&CK "stage" this belongs to (execution, persistence, etc.) |
| `technique` | The specific ATT&CK technique/sub-technique ID being demonstrated |
| `platforms` | OS + executor (cmd, psh, sh) + the command itself |

### 1.3 How to add an ability in the Caldera GUI
1. Log in → **Abilities** (top nav)
2. Click **+ Create Ability** (or **Import from YAML** if pasting the block directly)
3. Fill in Name / Description / Tactic / Technique
4. Add an executor (platform + command)
5. Click **Save**

Abilities can then be dragged into an **Adversary** profile and executed in an **Operation**.

---

## 2. Sample Abilities by ATT&CK Stage

Each stage below has **one working example** plus a **student task** to build a second one yourselves. All commands are lab-safe and non-destructive.

---

### Stage 1 — Initial Access (TA0001)

Initial Access abilities are usually simulated rather than "run" by Caldera itself (since Caldera operates *after* an agent is already deployed). In a workshop, we demonstrate this stage conceptually and simulate the follow-on action a payload would take once delivered.

**Example: Simulate a Malicious Macro Execution (T1204.002 – User Execution: Malicious File)**
```yaml
id: 11111111-1111-4111-8111-111111111111
name: Simulate Malicious File Execution
description: Simulates the user double-clicking a malicious attachment, launching calc.exe as a stand-in payload
tactic: initial-access
technique:
  attack_id: T1204.002
  name: Malicious File

platforms:
  windows:
    cmd:
      command: start calc.exe
```

**🎓 Student Task:**
Build an ability that simulates a user opening a "phishing" shortcut file (T1204.002 or T1566.001 spoofing). Have it launch `mspaint.exe` instead of calc.exe as the stand-in payload. Give it its own UUID, name, and description.

---

### Stage 2 — Execution (TA0002)

**Example 1: Launch Notepad (T1059)**
```yaml
id: 22222222-2222-4222-8222-222222222222
name: Launch Notepad
description: Opens Notepad as a benign execution demonstration
tactic: execution
technique:
  attack_id: T1059
  name: Command and Scripting Interpreter

platforms:
  windows:
    cmd:
      command: start notepad.exe
```

**Example 2: PowerShell Command Execution (T1059.001)**
```yaml
id: 33333333-3333-4333-8333-333333333333
name: PowerShell Command Execution
description: Demonstrates scripting interpreter execution via PowerShell
tactic: execution
technique:
  attack_id: T1059.001
  name: PowerShell

platforms:
  windows:
    psh:
      command: Get-Date
```

**🎓 Student Task:**
Create an ability that uses `cmd` executor to list the contents of `C:\Users\Public` using `dir`. Map it to T1059 and name it something descriptive like "Directory Listing via CMD."

---

### Stage 3 — Persistence (TA0003)

**Example 1: Scheduled Task Creation (T1053.005)**
```yaml
id: 44444444-4444-4444-8444-444444444444
name: Scheduled Task Creation
description: Creates a benign scheduled task to demonstrate persistence
tactic: persistence
technique:
  attack_id: T1053.005
  name: Scheduled Task

platforms:
  windows:
    cmd:
      command: schtasks /create /sc onlogon /tn "DemoTask" /tr "notepad.exe" /f
```

**Example 2: Registry Run Key Persistence (T1547.001)**
```yaml
id: 55555555-5555-4555-8555-555555555555
name: Registry Run Key Persistence
description: Adds a benign Run key entry to demonstrate registry persistence
tactic: persistence
technique:
  attack_id: T1547.001
  name: Registry Run Keys / Startup Folder

platforms:
  windows:
    cmd:
      command: reg add "HKCU\Software\Microsoft\Windows\CurrentVersion\Run" /v DemoApp /t REG_SZ /d "notepad.exe" /f
```

**🎓 Student Task:**
Build an ability that copies a dummy file (e.g. `notepad.exe` shortcut) into the current user's Startup folder path:
`C:\Users\<user>\AppData\Roaming\Microsoft\Windows\Start Menu\Programs\Startup`
Map it to **T1547.001** as well, but note it uses a different persistence mechanism (Startup Folder vs Run Key) — explain the difference in your `description` field.

---

### Stage 4 — Privilege Escalation (TA0004)

**Example 1: Check Current Privileges (T1548)**
```yaml
id: 66666666-6666-4666-8666-666666666666
name: Check Current Privileges
description: Lists the current user's privilege set to identify escalation paths
tactic: privilege-escalation
technique:
  attack_id: T1548
  name: Abuse Elevation Control Mechanism

platforms:
  windows:
    cmd:
      command: whoami /priv
```

**Example 2: Enumerate Processes for Injection Targets (T1055)**
```yaml
id: 77777777-7777-4777-8777-777777777777
name: Enumerate Running Processes for Injection Targets
description: Lists processes as a precursor to process-injection style escalation
tactic: privilege-escalation
technique:
  attack_id: T1055
  name: Process Injection

platforms:
  windows:
    psh:
      command: Get-Process | Select-Object Name, Id, Path
```

**🎓 Student Task:**
Create an ability that checks whether the current user is a member of the local Administrators group, using:
`net localgroup administrators`
Map it to **T1069.001** (Permission Groups Discovery: Local Groups) — note this is technically Discovery, but explain in your description why enumerating group membership often feeds into privilege escalation planning.

---

### Stage 5 — Defense Evasion (TA0005)

**Example 1: Clear PowerShell History (T1070.003)**
```yaml
id: 88888888-8888-4888-8888-888888888888
name: Clear PowerShell History
description: Clears the PSReadLine command history file to demonstrate evidence removal
tactic: defense-evasion
technique:
  attack_id: T1070.003
  name: Clear Command History

platforms:
  windows:
    psh:
      command: Remove-Item (Get-PSReadlineOption).HistorySavePath -Force -ErrorAction SilentlyContinue
```

**Example 2: Modify Registry to Hide File Extensions (T1112)**
```yaml
id: 99999999-9999-4999-8999-999999999999
name: Modify Registry to Hide File Extensions
description: Demonstrates defense evasion via registry modification
tactic: defense-evasion
technique:
  attack_id: T1112
  name: Modify Registry

platforms:
  windows:
    cmd:
      command: reg add "HKCU\Software\Microsoft\Windows\CurrentVersion\Explorer\Advanced" /v HideFileExt /t REG_DWORD /d 1 /f
```

**🎓 Student Task:**
Build an ability that clears the Windows Event Viewer "Application" log (a common evasion move):
`wevtutil cl Application`
Map it to **T1070.001** (Clear Windows Event Logs). ⚠️ In the workshop lab, discuss why this is heavily monitored/flagged in real environments — it's a strong detection signal.

---

### Stage 6 — Collection (TA0009)

**Example 1: Find Local Documents (T1005)**
```yaml
id: aaaaaaaa-aaaa-4aaa-8aaa-aaaaaaaaaaaa
name: Find Local Documents
description: Searches for common document types on the local system
tactic: collection
technique:
  attack_id: T1005
  name: Data from Local System

platforms:
  windows:
    cmd:
      command: dir /s /b C:\Users\*.docx C:\Users\*.xlsx
```

**Example 2: Take Screenshot (T1113)**
```yaml
id: bbbbbbbb-bbbb-4bbb-8bbb-bbbbbbbbbbbb
name: Take Screenshot
description: Captures the current desktop screen for collection demonstration
tactic: collection
technique:
  attack_id: T1113
  name: Screen Capture

platforms:
  windows:
    psh:
      command: Add-Type -AssemblyName System.Windows.Forms; [System.Windows.Forms.SendKeys]::SendWait('{PRTSC}')
```

**🎓 Student Task:**
Create an ability that searches for `.pdf` files under `C:\Users\Public` and lists them out, using `dir /s /b C:\Users\Public\*.pdf`. Map it to **T1005** as well, and explain in the description how an attacker might chain this with an exfiltration ability afterward.

---

### Stage 7 — Lateral Movement (TA0008)

**Example 1: Remote Share Access (T1021.002)**
```yaml
id: cccccccc-cccc-4ccc-8ccc-cccccccccccc
name: Remote Service Execution via Admin Share
description: Demonstrates connecting to a remote administrative share
tactic: lateral-movement
technique:
  attack_id: T1021.002
  name: SMB/Windows Admin Shares

platforms:
  windows:
    cmd:
      command: net use \\<target>\C$ /user:<domain>\<user> <password>
```

**Example 2: Remote PowerShell Session (T1021.006)**
```yaml
id: dddddddd-dddd-4ddd-8ddd-dddddddddddd
name: Remote PowerShell Session
description: Establishes a remote management session to another host
tactic: lateral-movement
technique:
  attack_id: T1021.006
  name: Windows Remote Management

platforms:
  windows:
    psh:
      command: Invoke-Command -ComputerName <target> -ScriptBlock { whoami }
```

**🎓 Student Task:**
Build an ability that uses `net view \\<target>` to enumerate shared resources on a remote lab machine before attempting to connect. Map it to **T1135** (Network Share Discovery), and add a note in your description explaining this would normally run *before* the two examples above in a real chain.

> ⚠️ Replace `<target>`, `<domain>`, `<user>`, `<password>` only with credentials/hosts that exist in your isolated lab.

---

### Stage 8 — Impact (TA0040)

These are intentionally **non-destructive** so nothing in the lab actually breaks.

**Example 1: Stop a Demo Service (T1489)**
```yaml
id: eeeeeeee-eeee-4eee-8eee-eeeeeeeeeeee
name: Stop a Demo Service
description: Stops a non-critical, pre-created demo service to illustrate service-stop impact
tactic: impact
technique:
  attack_id: T1489
  name: Service Stop

platforms:
  windows:
    cmd:
      command: sc stop DemoService
```

**Example 2: Rename Test File — Simulated Encryption (T1486)**
```yaml
id: ffffffff-ffff-4fff-8fff-ffffffffffff
name: Rename Test File (Simulated Data Encryption)
description: Renames a designated dummy test file to simulate encryption-style impact without real data loss
tactic: impact
technique:
  attack_id: T1486
  name: Data Encrypted for Impact

platforms:
  windows:
    cmd:
      command: ren C:\Users\Public\demo_test_file.txt demo_test_file.locked
```

**🎓 Student Task:**
Create an ability that renames the `.locked` demo file back to `.txt` (a "recovery" ability), mapped to the same technique but described as a rollback/cleanup step. This teaches students that every impact ability in a lab needs a corresponding cleanup step.

---

## 3. End-to-End Student Exercise: Build Your Own Ability From Scratch

Now that students have seen one example per stage, have them build **one brand-new ability** (not copied from above) following this checklist:

### Step-by-step task sheet

1. **Pick a tactic** you haven't fully explored yet (e.g. Discovery — TA0007).
2. **Pick a technique** from the [MITRE ATT&CK Matrix for Enterprise](https://attack.mitre.org/matrices/enterprise/windows/) that maps to it.
3. **Generate a UUID** for your ability.
4. **Write the YAML block** using the template:
   ```yaml
   id: <your-generated-uuid>
   name: <your ability name>
   description: <what it does, in your own words>
   tactic: <tactic-name>
   technique:
     attack_id: <TXXXX>
     name: <technique name>

   platforms:
     windows:
       cmd:
         command: <your command>
   ```
5. **Test it safely**: Run the raw command yourself in a lab VM's terminal *before* importing it into Caldera, so you know exactly what it does.
6. **Import it** into Caldera via **Abilities → Create Ability → Import from YAML**.
7. **Add it to an Adversary profile** alongside 2–3 abilities from earlier stages to build a mini kill chain.
8. **Run it as an Operation** in **manual/step-through mode** and explain to the group:
   - What ATT&CK tactic and technique this maps to
   - What a defender would look for to detect it
   - What log source (Sysmon, Windows Event Log, EDR) would catch it

### Suggested unclaimed techniques for students to choose from
| Tactic | Technique | ID |
|---|---|---|
| Discovery | Query Registry | T1012 |
| Discovery | File and Directory Discovery | T1083 |
| Discovery | Network Configuration Discovery | T1016 |
| Command and Control | Application Layer Protocol | T1071 |
| Exfiltration | Exfiltration Over Alternative Protocol | T1048 |
| Collection | Clipboard Data | T1115 |

---

## 4. Wrap-Up Checklist for Facilitators

- [ ] All students created at least 1 original ability
- [ ] Each student can explain their ability's tactic + technique out loud
- [ ] Each student ran their ability in an Operation (manual mode)
- [ ] Group discussed at least one detection method per ability created
- [ ] All destructive/impact abilities have a cleanup/rollback step
- [ ] Lab environment is isolated from production networks throughout

---

### Reference Links
- MITRE ATT&CK Matrix: https://attack.mitre.org/matrices/enterprise/
- Caldera GitHub: https://github.com/mitre/caldera
- Caldera Stockpile Plugin (pre-built abilities): bundled with Caldera under `plugins/stockpile`
