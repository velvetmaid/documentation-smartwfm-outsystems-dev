---
title: Freeing Up Disk Space and Clearing Outsystems Logs
layout: home
parent: Other
nav_order: 2
---

# Freeing Up C Disk Space and Clearing Outsystems Logs

### Steps:

1. **Log in to Remote Desktop.**
2. **Check the storage of C Disk.**
   - If there is no free space (**0 bytes**), you won’t be able to access **Service Center** or **Service Studio**.
3. **Open "Event Viewer".**
   - You can find it in the **Windows Start Menu**.
4. **Clear logs in "Windows Logs".**
   - On the left panel, locate **Windows Logs**.
   - Open **Application, Security, and System** categories.
   - Right-click each one, select **Clear Log...**, and confirm with **Clear**.
5. **Delete logs in SQL Server Management Studio (SSMS).**
   - Open **SQL Server Management Studio (SSMS)**.
   - In the **INDIGIDEVDB01** database, find **outsystems_log**.
   - Right-click it and select **New Query**.
   - Open the Notepad containing the SQL script (or minimize it if already open).
   - Copy the script starting from:
     ```sql
     USE [outsystems_log]
     ```
     until the last line.
   - Paste it into the **Query Editor**.
   - Execute the script and ensure the result is **"Successfully"** without errors.
6. **Shrink the database.**
   - Right-click **outsystems_log**, then go to **Tasks > Shrink > Database**.
   - When the shrink database panel appears, click **OK**.
7. **Check the C Disk storage again.**
8. **Done.**
