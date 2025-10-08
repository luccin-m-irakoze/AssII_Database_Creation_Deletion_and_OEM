# Oracle PDB and OEM Configuration Report

---

## 1. Overview of Tasks

This practical exercise focused on Oracle Multitenant Database operations and Enterprise Manager (OEM) configuration.

---

### Task 1 – Create a New Pluggable Database (PDB)

A new PDB was created to store class work.

- **PDB Name:** `lu_pdb_25815`  
- **Admin User:** `luccin_plsqlauca_25815`  

**SQL Command:**
```sql
CREATE PLUGGABLE DATABASE lu_pdb_25815
  ADMIN USER luccin_plsqlauca_25815 IDENTIFIED BY Lu321
  FILE_NAME_CONVERT = (
     'C:\APP\LUCCI\PRODUCT\21C\ORADATA\XE\PDBSEED\',
     'C:\APP\LUCCI\PRODUCT\21C\ORADATA\XE\lu_pdb_25815\'
  );
````

Then opened the PDB:

```sql
ALTER PLUGGABLE DATABASE lu_pdb_25815 OPEN;
```

---

### Task 2 – Create and Delete a PDB

A second PDB was created and then deleted to demonstrate lifecycle management.

**Create Command:**

```sql
CREATE PLUGGABLE DATABASE lu_to_delete_pdb_25815
  ADMIN USER luccin_plsqlauca_25815 IDENTIFIED BY Lu321
  FILE_NAME_CONVERT = (
     'C:\APP\LUCCI\PRODUCT\21C\ORADATA\XE\PDBSEED\',
     'C:\APP\LUCCI\PRODUCT\21C\ORADATA\XE\lu_to_delete_pdb_25815\'
  );
```

**Delete Command:**

```sql
ALTER PLUGGABLE DATABASE lu_to_delete_pdb_25815 CLOSE IMMEDIATE;
DROP PLUGGABLE DATABASE lu_to_delete_pdb_25815 INCLUDING DATAFILES;
```

*Result:* Both creation and deletion completed successfully.

---

### Task 3 – Configure Oracle Enterprise Manager (OEM)

Initially, **Oracle Database 23ai Free** was installed on the host system.
However, **Oracle 23ai Free does not include Oracle Enterprise Manager Express (EM Express)**.

To complete the task:

* A **Virtual Machine (VM)** was created.
* Installed **Oracle Database 21c XE** (which includes EM Express by default).
* Configured and accessed EM successfully.

**Configuration Steps:**

```sql
-- Connect as SYS
CONNECT SYS AS SYSDBA;

-- Enable HTTPS port
BEGIN
  DBMS_XDB_CONFIG.SETHTTPSPORT(5500);
END;
/

-- Switch container and grant privileges
ALTER SESSION SET CONTAINER = lu_pdb_25815;
GRANT DBA, XDBADMIN, SELECT_CATALOG_ROLE TO luccin_plsqlauca_25815;
```

**Access URL:** [https://localhost:5500/em](https://localhost:5500/em)

**Login Details:**

* **Username:** `luccin_plsqlauca_25815`

---

## 2. Issues Encountered and Solutions

| **Issue**                          | **Cause**                              | **Solution**                                         |
| ---------------------------------- | -------------------------------------- | ---------------------------------------------------- |
| ORA-01031: Insufficient Privileges | Running admin commands as non-SYS user | Reconnected as `SYS AS SYSDBA` and reissued commands |
| “Not Implemented” error at `/em`   | Oracle 23ai Free lacks EM Express      | Installed Oracle 21c XE on a VM                      |

---

## 3. Conclusion

This exercise demonstrated:

* Creation and management of Oracle Pluggable Databases (PDBs)
* Configuration and access to **Oracle Enterprise Manager Express**
* Troubleshooting environment-specific issues in Oracle 23ai and 21c

Through hands-on setup and problem-solving, I gained practical experience with Oracle Multitenant architecture, administrative privileges, and database management interfaces.

---

## 4. Screenshots

1. **PDB Creation –** `lu_pdb_25815`:

   <img width="779" height="175" alt="Screenshot 2025-10-07 225249" src="https://github.com/user-attachments/assets/651aaf5b-942c-47b1-8a63-f4f367e65989" />

3. **PDB Deletion –** `lu_to_delete_pdb_25815`:
 
   <img width="629" height="238" alt="Screenshot 2025-10-07 225643" src="https://github.com/user-attachments/assets/2d643f7a-aacb-4243-89f5-84ddfd57b219" />

5. **OEM Dashboard –** (showing username and container):

   <img width="1522" height="954" alt="Screenshot 2025-10-07 232452" src="https://github.com/user-attachments/assets/588f24c6-5b88-4b04-ab89-00ba89c59a97" />
---

> **Issued Enountered:** Oracle 23ai Free does not include EM Express by default. To fulfill the lab requirements, Oracle 21c XE was installed in a Virtual Machine, which includes EM Express out-of-the-box.

---
