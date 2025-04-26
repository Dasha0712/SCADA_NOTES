# Troubleshooting — CET, IET & MicroSCADA Errors

> Real site errors encountered and their solutions. Covers CET application, IET application, and MicroSCADA.

---

## Table of Contents

1. [CET Application Errors](#1-cet-application-errors)
2. [IET Application Errors](#2-iet-application-errors)
3. [MicroSCADA Errors](#3-microscada-errors)

---

## 1. CET Application Errors

---

### Error 1 — Online Diagnostics: "Not Okay / Not Ready" + No Polling

**Symptom:**
- Online diagnostics shows **Not Okay, Not Ready**
- No polling is happening at all

**Cause:**
- The **client name of the OPC Server** is not correctly assigned in the **Properties of the OPC Server**

**Solution:**
- Go to the **Properties of the OPC Server**
- Make sure the client name is **exactly correct** (e.g., `AA1KF11OPC1`)
- This name **must match exactly** with the client name given in the **SCD file**

> ⚠️ Even one character wrong will cause this error — check carefully.

---

### Error 2 — Online Diagnostics: "Okay / Ready" + No Polling

**Symptom:**
- Online diagnostics shows **Okay, Ready**
- But **no polling is happening**

**Cause:**
- The client name (OPC Server name e.g., `AA1KF11OPC1`) is **not assigned to the Report Control Block** of the imported SCD file

**Solution:**
- Open the imported SCD file
- Find the **Report Control Block**
- Make sure the client name is **assigned correctly**

> ⚠️ **Common mistake:** People type `0` (zero) instead of `O` (letter O) in the name.
> Example: `AA1KF11OPC1` — the last character is the number `1`, but the `O` before it must be the **letter O**, not zero.
> Double-check every character carefully.

---

### Error 3 — Value Comes in Online Diagnostics But Only Updates on Manual Refresh

**Symptom:**
- Value is visible in online diagnostics for that particular node
- But it **only updates when you manually click Refresh**
- Does not update automatically / in real time

**Solution:** 
- You have to delete the **Report client** which is located under **Report Enabled** -> you have to Chose which URCB is under client(AA1KF11OPC1 and particular dataset is in that URCB) which is going to report to scada client
- Now manually add the Client and and client name.
- give management properly.

---

## 2. IET Application Errors

---

### Error 1 — Empty IED Name While Assigning Client Numbers

**Symptom:**
- While assigning client numbers (putting 1, 2, 3... to clients), there is always an **empty IED name** visible in the list

**Cause:**
- Accidentally assigning a client number to the **empty IED entry** instead of the correct IED

**Solution:**
- When assigning client numbers, **make sure you do not assign any number to the empty IED name**
- Only assign numbers to valid, named IEDs in the list
- Skip the empty entry completely

> ⚠️ This is an easy mistake to make — always verify the IED name before assigning a number.

---

## 3. MicroSCADA Errors

---

### Error 1 — HMI: Static SLD Not Working Correctly

**Symptom:**
- Static SLD (Single Line Diagram) in HMI is not behaving correctly

**Solution:**
- If you are using a **static SLD**, make sure you **delete the "Manage Topology"** setting
- Or ensure you have **not made any changes** to the topology settings
- Topology management conflicts with static SLD behavior

---

### Error 2 — Gateway: Signal Not Coming to VINCI Software

**Symptom:**
- Signal is not reaching the VINCI software through the gateway

**Solution — Step 1:**
- Go to **System Configuration → Sys Config → Communication**
- **Stop** the PC Net connection
- **Start** it again (restart PC Net)

**Solution — Step 2 (if signal still not coming after Step 1):**
- Go to **COM500i**
- **Delete** the existing `NCC1` entry
- **Add a new NCC** entry — enter the correct **Station Number** and **Alarm Group**

> ⚠️ **Critical warning:** Make sure you do **NOT delete any signals** during this process.
> Signals are very hard to recover once deleted.

---

### Error 3 — Error Code 13222 (Wrong Node in Configuration)

**Symptom:**
- Error code **13222** appears

**Cause A — Wrong node assigned:**
- You put the wrong node in the configuration
- Example: putting the **ON node for Open Execute Command** and **OFF node for Close Execute Command** — this is **wrong**
- Make sure the correct node is mapped to the correct command direction

**Cause B — Executing command too quickly:**
- You executed a command in HMI and then tried to execute **another command too quickly**
- The system has not completed the first execution yet
- **Wait for the first command to complete** before sending another command

> ⚠️ Always verify node mapping for Open/Close commands before commissioning.

---

### Error 4 — PO List: Missing Nodes / Missing Pages

**Symptom:**
- Some nodes are missing from the Process Object list
- Station numbers not assigned to all nodes

**Cause:**
- The next page of nodes was **not visible** because the "Go to Next Page" button was not enabled
- Some nodes on later pages were missed entirely

**Solution:**
- In the **PO List**, make sure the **"Go to Next Page" button is enabled**
- Go through **every page** and clear the address for every node
- Make sure **all nodes are included**
- Make sure **station number is assigned to every node** without exception

> ⚠️ This same rule applies to the **PO List application** — enable the next page button there too, or you will miss nodes.

---

### Error 5 — Value Coming to CET Tool But Not to Any Other Application

**Symptom:**
- Signal value is visible in the CET tool
- But the value is **not reaching any other application** (VINCI, HMI, etc.)

**Cause:**
- Problem is in the **PL file and INI file configuration**
- During INI file creation, the **node number or station number** was entered differently from what was configured in the **System Control Panel application**

**Solution:**
- Check the **PL file** and **INI file**
- During INI file creation, make sure the **node number** and **station number** match **exactly** with what was configured in the System Control Panel
- Whenever any step asks you for a node number — **always put the same number consistently across all steps**

> ⚠️ Node number mismatch between INI file and System Control Panel is a silent error — data flows to CET but stops there.



*Last updated: April 2026 | Site Troubleshooting Notes*