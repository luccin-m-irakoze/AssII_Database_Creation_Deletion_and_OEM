# Oracle PDB and OEM Configuration Report

**Student Name:** Luccin Irakoze  
**Student ID:** 25815  
**Course:** Oracle Database Administration (PL/SQL – AUCA)  
**Date:** October 2025  

---

## 1. Overview of Tasks

This practical exercise focused on Oracle Multitenant Database operations and Enterprise Manager (OEM) configuration.

---

### Task 1 – Create a New Pluggable Database (PDB)

A new PDB was created to store class work.

- **PDB Name:** `lu_pdb_25815`  
- **Admin User:** `luccin_plsqlauca_25815`  
- **Password:** `Lu321`

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

*(Insert screenshots for both steps here.)*

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
* **Container:** `LU_PDB_25815`

*(Insert screenshot of EM Express Dashboard showing username and container.)*

---

## 2. Issues Encountered and Solutions

| **Issue**                          | **Cause**                              | **Solution**                                         |
| ---------------------------------- | -------------------------------------- | ---------------------------------------------------- |
| ORA-01031: Insufficient Privileges | Running admin commands as non-SYS user | Reconnected as `SYS AS SYSDBA` and reissued commands |
| “Not Implemented” error at `/em`   | Oracle 23ai Free lacks EM Express      | Installed Oracle 21c XE on a VM                      |
| Invalid login in EM                | Used CDB instead of correct PDB        | Specified `LU_PDB_25815` as container during login   |

---

## 3. Conclusion

This exercise demonstrated:

* Creation and management of Oracle Pluggable Databases (PDBs)
* Configuration and access to **Oracle Enterprise Manager Express**
* Troubleshooting environment-specific issues in Oracle 23ai and 21c

Through hands-on setup and problem-solving, I gained practical experience with Oracle Multitenant architecture, administrative privileges, and database management interfaces.

---

## 4. Screenshots

1. **PDB Creation –** `lu_pdb_25815`
2. **PDB Deletion –** `lu_to_delete_pdb_25815`
3. **OEM Dashboard –** (showing username and container)

---

> **Note:** Oracle 23ai Free does not include EM Express by default. To fulfill the lab requirements, Oracle 21c XE was installed in a Virtual Machine, which includes EM Express out-of-the-box.

```
