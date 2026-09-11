---
marp: true
theme: gaia
paginate: true
header: 'Technology Understanding VOVIM'
style: |
  section {
    font-size: 28px;
    overflow-y: auto;
  }
  h1, h2 {
    color: #0b5cab;
  }
  code {
    color: #7a2e00;
  }
---

<!-- _class: lead -->

# Windows 11
## From everyday user to power user

**How does an IT operator think?**

---

# Goals for this session

After this presentation, you should be able to:

- work faster and more precisely in Windows 11
- find out what is actually happening on a PC
- troubleshoot systematically instead of guessing
- make changes in a controlled and reversible way
- document your work so someone else can take over

---

# Two different roles

| Everyday user | IT operator |
| --- | --- |
| Wants it to work | Wants to understand why it works |
| Tries random solutions | Tests one hypothesis at a time |
| Changes settings | Considers impact and risk |
| Fixes only their own PC | Creates a repeatable solution |
| May remember the solution | Documents the solution |

**Power user** does not mean knowing the most tricks. It means having better control.

---

# The basic rule: observe before changing

When something is wrong:

1. **Define the problem** - who, what, where and when?
2. **Collect facts** - error message, time, changes and symptoms.
3. **Form a hypothesis** - what do you think is the cause?
4. **Test the smallest thing first** - choose a safe test.
5. **Change one thing at a time**.
6. **Confirm and document** the result.

> A restart can be a test. It is not an explanation.

---

# Keyboard shortcuts that actually save time

- `Win + E` - File Explorer
- `Win + I` - Settings
- `Win + X` - administration shortcut menu
- `Win + V` - clipboard history
- `Win + Shift + S` - screen snipping
- `Alt + Tab` - switch between windows
- `Win + Ctrl + D` - new virtual desktop
- `Ctrl + Shift + Esc` - Task Manager

**Tip:** Learn three shortcuts well before learning ten new ones.

---

# Find and launch tools quickly

Press `Win` and type the name of the tool:

- **Terminal** - PowerShell and command line
- **Task Manager** - processes, performance and startup
- **Services** - background services
- **Event Viewer** - logs and system events
- **Device Manager** - hardware and drivers
- **Resource Monitor** - more detailed resource usage
- **Computer Management** - a combined administration console

Search is often faster than looking through menus.

---

# Task Manager: more than "End task"

Use the tabs to investigate:

- **Processes:** What is using CPU, memory, disk or network?
- **Performance:** Is the bottleneck the processor, memory, disk or network?
- **Startup apps:** What starts automatically?
- **Users:** Which sessions and processes are active?
- **Services:** Which services are running?

Look for patterns over time. A high value for one second is not necessarily a problem.

---

# Resource Monitor: `resmon`

Open **Resource Monitor** by pressing `Win + R`, typing `resmon` and pressing Enter.

Use the tool when Task Manager shows a symptom but you need more detail:

- **Overview:** which processes are using resources right now?
- **CPU:** which services and processes are waiting or loading the processor?
- **Memory:** is memory full, or is it being used effectively as cache?
- **Disk:** which process is reading or writing a lot?
- **Network:** which processes have active connections?

`resmon` is useful for a quick and detailed investigation of a slow PC.

---

# Reliability Monitor: `perfmon /rel`

Open **Reliability Monitor** by pressing `Win + R`, typing `perfmon /rel` and pressing Enter.

Use the tool to see a timeline of what has happened on the PC:

- application and Windows crashes
- hardware failures and unexpected shutdowns
- installations and uninstallations
- updates and driver changes
- when problems started and how often they occur

Start by finding the time when the problem appeared. Then compare the event with changes that happened just before it.

The reliability index is a clue, not a complete diagnosis. Use the event details and other logs to find the cause.

---

# File Explorer with control

Power users:

- show file extensions: **View -> Show -> File name extensions**
- use meaningful folder names and a tidy structure
- separate working files, archives, installers and backups
- check the path and date before overwriting anything
- avoid storing important files only on the desktop

**Important:** A synced folder is not automatically a backup.

---

# The terminal: ask the system directly

Open Windows Terminal and try:

```powershell
Get-ComputerInfo | Select-Object WindowsProductName, WindowsVersion
Get-Process | Sort-Object CPU -Descending | Select-Object -First 10
Get-Service | Where-Object Status -eq 'Running'
Get-PSDrive -PSProvider FileSystem
```

PowerShell provides information that can be read, filtered and reused.

**Think:** What do I want to know? Which tool can answer that?

---

# Networking: separate symptoms from causes

When "the internet does not work":

1. Is the problem limited to one device?
2. Does the device have an IP address?
3. Does communication with the local gateway work?
4. Does DNS lookup work?
5. Does the connection to an external service work?

Useful commands:

```powershell
ipconfig /all
ping 192.168.1.1
nslookup example.com
Test-NetConnection example.com -Port 443
```

Do not immediately change passwords or reinstall everything.

---

# Permissions and the administrator role

Windows distinguishes between:

- standard user and administrator
- read, write and execute access
- the user's profile and the entire machine
- what an application needs and what access it actually has

**Least privilege:** Use the minimum access needed to complete the task.

Before running something as administrator, ask:

- Why is it required?
- What will change?
- Can the change be rolled back?
- How will I confirm that it worked?

---

# Security is part of good operations

A power user:

- updates Windows and applications
- uses unique passwords and multifactor authentication
- locks the screen when leaving the workplace
- evaluates links, attachments and pop-ups critically
- installs software only from trusted sources
- protects personal data and student information

**Convenience is not a good reason to remove security.**

---

# Event Viewer: the system logbook

Open **Event Viewer** and look under:

- **Windows Logs -> System** - drivers, services and hardware
- **Windows Logs -> Application** - application errors
- **Windows Logs -> Security** - logins and access, depending on policy

Look for:

- time
- source
- event ID
- severity
- what happened immediately before the error

A log entry does not automatically mean that something is serious.

---

# Troubleshooting as a small science

| Step | Question |
| --- | --- |
| Problem | What is not working? |
| Scope | Does it affect one user or many? |
| Time | When did it start? What changed? |
| Hypothesis | Which cause fits the facts? |
| Test | Which safe test can distinguish the causes? |
| Action | What are we changing, and why? |
| Verification | Does it still work afterwards? |

Write down the result, even when the hypothesis was wrong.

---

# Change control on one PC

Before a change:

- record the current state
- back up important data
- agree on what "rollback" means
- choose a low-risk time

After the change:

- test the original function
- test relevant side effects
- record the setting, time and result
- clean up temporary files and tools

A good solution can be explained and repeated by someone else.

---

# Documentation: make knowledge shareable

A short operations log should contain:

```text
Date and time:
Problem and scope:
Observations and error messages:
Hypothesis:
Actions and commands:
Result:
Rollback:
Next step / who needs to be informed:
```

Write facts first. Clearly distinguish between **observed**, **assumed** and **confirmed**.

---

# Automate what is repeated

Automation is suitable when the task:

- happens often
- follows fixed rules
- has low risk if something goes wrong
- can first be tested on a small sample

Example: find the largest files in your home folder:

```powershell
Get-ChildItem $HOME -File -Recurse -ErrorAction SilentlyContinue |
  Sort-Object Length -Descending |
  Select-Object -First 10 FullName, Length
```

Automation without control can make mistakes faster.



<!--

# Practical task: operate a "problem PC"

Scenario: A user says that the PC is slow and that the browser "cannot find the internet".

Work in pairs:

1. Ask five clarifying questions.
2. Collect facts without changing the system.
3. Form two possible hypotheses.
4. Choose one safe test for each hypothesis.
5. Make one controlled change.
6. Submit a short operations log.

**Requirement:** No reinstalling, random deletion or "try everything" approach.

-->

---

# Power user checklist

Before closing a case, can you answer yes to these questions?

- I know what the actual problem was.
- I have separated facts from assumptions.
- I know what was changed.
- I have confirmed that the solution works.
- I have considered security and access.
- Someone else can follow the documentation.

This is the difference between being lucky and operating IT.

---

<!-- _class: lead -->

# Summary

## Power user = controlled curiosity

Use Windows tools to **observe**.

Use the operations method to **understand**.

Use documentation and security to **build trust**.

**The next time something fails: stop, ask, measure, test and write it down.**
