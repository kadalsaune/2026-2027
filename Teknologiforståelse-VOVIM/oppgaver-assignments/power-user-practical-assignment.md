# 🛠️ Lab assignment: Windows 11 Power User

**Subject:** Technology Understanding

**Duration:** 60–90 minutes

**Purpose:** Learn to use Windows' built-in administration and troubleshooting tools like an IT technician, and document your findings systematically.

> 📌 **Documentation requirement:**
> Submit a short **Word document or PDF** answering all questions and including **screenshots** at the indicated places.
> *(Tip for screenshots: Use `Win + Shift + S` to capture a selection.)*

---

### Part 1: Fast navigation with `Win + R` (15 min)

In this part, you will explore system commands that IT technicians use to jump directly to administration tools instead of going through the Settings app.

1. Press `Win + R` to open **Run**.
2. Test the following commands one at a time:
   * `sysdm.cpl` (System Properties)
   * `ncpa.cpl` (Network Connections)
   * `devmgmt.msc` (Device Manager)
   * `compmgmt.msc` (Computer Management)

3. **Change the computer name:** In `sysdm.cpl`, click *Change...* and name the computer: `ELEV-PC-[YourName]`. *(Do not restart yet.)*
4. **📸 Documentation for Part 1:**
   * **Screenshot 1:** Take a screenshot of the `sysdm.cpl` window showing your new computer name.
   * **Written answer:** Open `ncpa.cpl`. What is the exact name of your active network adapter, and what is its status (for example Wi-Fi or Ethernet)?



---

### Part 2: A deep dive into processes with Resource Monitor (20 min)

When a user reports that the PC is slow, or that an application has "stopped responding", IT operations use **Resource Monitor** to isolate and analyze one process at a time.

1. Open the **Calculator** app and **Notepad** from the Start menu on your PC.
2. Open **Run** (`Win + R`) and type: `resmon`
3. Select the **CPU / Processor** tab at the top of the window.
4. In the **Processes** list, find the `CalculatorApp.exe` process (or `Calculator.exe` / `Notepad.exe`) and tick the boxes to the left of those processes.
5. Notice that the other sections at the bottom of the screen (such as **Associated Handles** and **Services**) now show information only for the programs you have selected.
6. Expand the **Associated Handles** section at the bottom of the **CPU** tab while keeping your selected process checked.
7. **📸 Documentation for Part 2:**
   * **Written answer 1:** What PID (Process ID) does the application you selected have in the list? *(Note: the PID changes every time a program starts!)*
   * **Written answer 2:** Select the **Network** tab at the top of Resource Monitor. Open your browser (for example Edge or Chrome) and load a page with a lot of content (for example vg.no or an HD video on YouTube). Which process reaches the highest value under `Total (B/sec)` in the network activity list?
   * **Screenshot 2:** Take a screenshot of Resource Monitor with one process checked in the CPU list so the lower section is filtered to your selected process.



---

### Part 3: Log analysis and the reliability index (25 min)

When a user reports that "the machine crashed on Tuesday", you need to investigate its history.

1. Open **Reliability Monitor** by pressing `Win + R` and typing:
   `perfmon /rel`
2. Study the graph showing the computer's reliability score over time, on a scale from 1 to 10.
3. Then open **Event Viewer** by typing `eventvwr.msc` in `Win + R`.
4. Navigate to **Windows Logs -> System**.
5. Click **Filter Current Log...** in the right-hand panel and select only **Critical** and **Error** events.
6. **📸 Documentation for Part 3:**
   * **Written answer 1:** What is your current reliability score (1–10) in Reliability Monitor?
   * **Written answer 2:** Choose one error message from **Event Viewer** (the System log). Record:
     * **Source:**
     * **Event ID:**
     * **Brief explanation of what the error concerned:**

   * **Screenshot 3:** Take a screenshot of the graph in Reliability Monitor (`perfmon /rel`).



---

### Part 4: System file checking in Terminal (15 min)

The final step for an IT operator when Windows behaves strangely is to verify that the system files are not damaged.

1. Right-click the Start menu (`Win + X`) and choose **Terminal (Admin)** or **PowerShell (Administrator)**.
2. Run the System File Checker command-line tool by typing:

```cmd
sfc /scannow
```

3. Wait until the scan reaches 100%.
4. **📸 Documentation for Part 4:**
   * **Screenshot 4:** Take a screenshot of the Terminal window after the scan is complete, showing the result message, for example *"Windows Resource Protection did not find any integrity violations"*.



---

### 📤 Checklist before submission

Your document must contain a total of **4 screenshots** and answers to the **5 written questions** from Parts 1–4.




