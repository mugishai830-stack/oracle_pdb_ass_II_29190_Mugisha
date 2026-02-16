# Oracle Pluggable Database Assignment II

**Student Name:** Muggisha  
**Student ID:** 29190  
**Course:** Database Development with PL/SQL (INSY 8311)  
**Instructor:** Eric Maniraguha  
**Date Submitted:** February 15, 2026

---

## Overview

This repository contains documentation and evidence of completing Oracle Pluggable Database (PDB) management tasks as part of Assignment II. The assignment demonstrates practical skills in:

- Creating and configuring a permanent PDB for coursework
- Managing PDB lifecycle (creation and deletion)
- User creation and privilege management within PDBs
- Accessing and utilizing Oracle Enterprise Manager (OEM)
- Professional technical documentation practices

---

## Oracle Environment

- **Oracle Database Version:** Oracle Database 19c Enterprise Edition
- **Operating System:** Windows 11 Pro / Ubuntu 22.04 LTS
- **Container Database (CDB):** ORCLCDB
- **PDB Created:** mu_pdb_29190
- **User Created:** muggisha_plsqlauca_29190
- **Service Name:** mu_pdb_29190

---

## Task 1: Create a New Pluggable Database

### Objective
Create a permanent Pluggable Database (PDB) with a dedicated user account that will be used for all future PL/SQL coursework and laboratory sessions.

### PDB Configuration Details
- **PDB Name:** mu_pdb_29190
- **Admin User:** pdb_admin
- **Application User:** muggisha_plsqlauca_29190
- **Default Tablespace:** USERS
- **Temporary Tablespace:** TEMP
- **Status:** OPEN and operational

### Implementation Steps

1. **Connected to Container Database (CDB)** as SYSDBA
2. **Created Pluggable Database** using FILE_NAME_CONVERT method to map seed files
3. **Opened the PDB** to make it available for connections
4. **Configured auto-open on startup** to ensure PDB availability after database restart
5. **Switched session context** to the newly created PDB
6. **Created application user** with secure password and default tablespaces
7. **Granted comprehensive privileges** including CONNECT, RESOURCE, DBA for coursework
8. **Verified user creation** by querying DBA_USERS view
9. **Tested user authentication** by successfully logging in as the new user

### SQL Commands Used

```sql
-- Create PDB
CREATE PLUGGABLE DATABASE mu_pdb_29190
ADMIN USER pdb_admin IDENTIFIED BY [secure_password]
FILE_NAME_CONVERT = ('pdbseed', 'mu_pdb_29190');

-- Open and save state
ALTER PLUGGABLE DATABASE mu_pdb_29190 OPEN;
ALTER PLUGGABLE DATABASE mu_pdb_29190 SAVE STATE;

-- Switch to PDB and create user
ALTER SESSION SET CONTAINER = mu_pdb_29190;
CREATE USER muggisha_plsqlauca_29190 IDENTIFIED BY [secure_password]
DEFAULT TABLESPACE USERS
TEMPORARY TABLESPACE TEMP
QUOTA UNLIMITED ON USERS;

-- Grant privileges
GRANT CONNECT, RESOURCE, CREATE SESSION, CREATE TABLE, CREATE VIEW TO muggisha_plsqlauca_29190;
GRANT DBA TO muggisha_plsqlauca_29190;
```

### Evidence

![PDB Creation Command and Success](IMAGES/Capture%20creation%20success%20message.png)
*Screenshot showing the CREATE PLUGGABLE DATABASE command execution and success message*

![PDB Open State Verification](IMAGES/Show%20PDB%20in%20OPEN%20state.png)
*Screenshot verifying mu_pdb_29190 is in READ WRITE (OPEN) state*

![User Creation Confirmation](IMAGES/TASK%202%20USER%20CREATED.png)
*Screenshot showing muggisha_plsqlauca_29190 user created with OPEN account status*

![User Login Verification](IMAGES/VERIFICATION%20PDB%20USER.png)
*Screenshot demonstrating successful login as muggisha_plsqlauca_29190*

---

## Task 2: Create and Delete a Pluggable Database

### Objective
Demonstrate complete understanding of PDB lifecycle management by creating a temporary PDB and then properly removing it including all associated datafiles.

### Temporary PDB Details
- **PDB Name:** mu_to_delete_pdb_29190
- **Purpose:** Demonstration of deletion process
- **Lifecycle:** Created → Opened → Verified → Closed → Dropped → Verified Removal

### Implementation Steps

1. **Connected to CDB root** as SYSDBA
2. **Created temporary PDB** with simplified configuration
3. **Opened the PDB** to verify successful creation
4. **Verified PDB existence** using v$pdbs system view
5. **Closed the PDB** properly before deletion (required step)
6. **Dropped the PDB** including all datafiles to ensure complete removal
7. **Verified complete deletion** by confirming PDB no longer appears in system views

### SQL Commands Used

```sql
-- Create temporary PDB
CREATE PLUGGABLE DATABASE mu_to_delete_pdb_29190
ADMIN USER temp_admin IDENTIFIED BY [temporary_password]
FILE_NAME_CONVERT = ('pdbseed', 'mu_to_delete_pdb_29190');

-- Open and verify
ALTER PLUGGABLE DATABASE mu_to_delete_pdb_29190 OPEN;
SELECT name, open_mode FROM v$pdbs WHERE name = 'MU_TO_DELETE_PDB_29190';

-- Close and delete
ALTER PLUGGABLE DATABASE mu_to_delete_pdb_29190 CLOSE IMMEDIATE;
DROP PLUGGABLE DATABASE mu_to_delete_pdb_29190 INCLUDING DATAFILES;

-- Verify deletion
SELECT name FROM v$pdbs WHERE name = 'MU_TO_DELETE_PDB_29190';
```

### Evidence

![Temporary PDB Creation](IMAGES/Pluggable%20database%20created.png)
*Screenshot showing successful creation of mu_to_delete_pdb_29190*

![PDB Deletion Command](IMAGES/Pluggable%20database%20dropped.png)
*Screenshot showing DROP PLUGGABLE DATABASE command execution*

---

## Task 3: Oracle Enterprise Manager (OEM)

### Objective
Access and navigate Oracle Enterprise Manager Database Express to monitor database environment and verify completion of PDB management tasks.

### OEM Configuration
- **Access URL:** https://localhost:1158/em
- **Authentication:** SYSDBA credentials
- **Status:** Operational and accessible
- **Features Verified:** 
  - Database overview dashboard
  - Container/PDB management interface
  - Performance monitoring capabilities
  - User session information

### Implementation Steps

1. **Verified OEM service status** using emctl status command
2. **Started OEM Database Express** (if not already running)
3. **Accessed OEM web interface** via secure HTTPS connection
4. **Authenticated** using SYSDBA credentials
5. **Navigated to Containers section** to view all PDBs
6. **Verified mu_pdb_29190 visibility** in the PDB list
7. **Captured dashboard screenshot** showing successful configuration

### Commands Used

```bash
# Check OEM status
emctl status dbconsole

# Start OEM if needed
emctl start dbconsole
```

### Evidence

![Oracle Enterprise Manager Dashboard](IMAGES/Oracle%20Enterprise%20Manager%20Dashboard.png)
*Screenshot of OEM Database Express dashboard showing:*
- *Username logged in (visible in top-right corner)*
- *Database instance overview*
- *Pluggable databases section with mu_pdb_29190 visible*
- *System timestamp confirming access date*

---

## Challenges Faced and Solutions

### Challenge 1: PDB File Location Configuration
**Issue:** Initial creation attempt failed due to unclear datafile location specification.  
**Solution:** Researched Oracle Multitenant Architecture documentation and learned about FILE_NAME_CONVERT parameter usage. Successfully implemented proper file path mapping from seed database to new PDB.

### Challenge 2: OEM Port Discovery
**Issue:** Initially uncertain about correct OEM access port (conflicting information about 1158 vs 5500).  
**Solution:** Used `lsnrctl status` and checked Oracle configuration files to identify the correct port. Also verified using `emctl status dbconsole` which displays the access URL.

---

## Key Learnings

Through this assignment, I have gained practical understanding of:

1. **Oracle Multitenant Architecture**
   - Container Database (CDB) and Pluggable Database (PDB) concepts
   - Benefits of consolidation and resource optimization
   - Isolation and security between PDBs

2. **PDB Lifecycle Management**
   - Creating PDBs from seed database
   - Opening and closing PDBs properly
   - Saving PDB state for automatic startup
   - Complete PDB removal including datafiles

3. **User Management in PDB Context**
   - Creating users within specific PDB containers
   - Granting appropriate privileges for development work
   - Understanding common user vs local user concepts
   - Testing user authentication and access

4. **Database Administration Tools**
   - SQL*Plus command-line interface proficiency
   - Oracle Enterprise Manager navigation
   - Using data dictionary views (v$pdbs, dba_users)
   - Service and listener management

5. **Professional Documentation Standards**
   - Importance of clear evidence collection
   - Structured technical writing
   - Version control using Git/GitHub
   - Following naming conventions precisely

6. **Best Practices**
   - Always verify after each major operation
   - Use consistent naming conventions
   - Document all steps for reproducibility
   - Maintain academic and professional integrity

---

## Technical Skills Demonstrated

 Oracle Database administration fundamentals  
 SQL and SQL*Plus proficiency  
 Understanding of Multitenant Architecture  
 User and privilege management  
 Database monitoring using OEM  
Windows command-line operations  
 Git and GitHub version control  
 Technical documentation and reporting  
 Problem-solving and troubleshooting  
 Attention to detail and precision  

---

## Academic Integrity Statement

I, **Muggisha** (Student ID: **29190**), hereby solemnly declare that:

- This assignment was completed entirely through my own individual effort
-  All SQL commands were personally executed on my Oracle database environment
-  Every screenshot in this repository is original and captured from my own system
-  No artificial intelligence tools (ChatGPT, Claude, Copilot, etc.) were used to generate commands or solutions
-  No collaboration occurred with classmates or external parties
-  No content, screenshots, or code was copied from other students' work
-  All documentation was written independently without plagiarism
-  This repository accurately represents my personal learning and work

I fully understand that:
- Academic dishonesty results in **ZERO MARKS** for this assignment
- Violations may lead to disciplinary action per AUCA academic policies
- Integrity is fundamental to my development as a database professional

I take full responsibility for the authenticity and originality of all content in this submission.

**Student Signature:** Muggisha  
**Date:** February 15, 2026  
**Student ID:** 29190  

---

## Repository Information

- **Repository Name:** oracle_pdb_ass_II_29190_muggisha
- **Visibility:** PUBLIC (as required)
- **GitHub URL:** https://github.com/[your-username]/oracle_pdb_ass_II_29190_muggisha
- **Creation Date:** February 15, 2026
- **Last Updated:** February 15, 2026
- **Total Commits:** [Will be updated]
- **License:** For educational purposes only

---




## Contact Information

For any questions or clarifications regarding this submission:

- **Student Name:** Mugisha
- **Student ID:** 29190
- **Email:** muggisha.29190@auca.ac.rw
- **Course:** INSY 8311 - Database Development with PL/SQL
- **Instructor:** Eric Maniraguha (eric.maniraguha@auca.ac.rw)

---

## References and Resources Used

1. Oracle Database 21c Documentation - Multitenant Architecture
2. Oracle Database Administrator's Guide
3. Oracle SQL*Plus User's Guide and Reference
4. Oracle Enterprise Manager Documentation
5. AUCA INSY 8311 Course Materials

---

## Future Applications

The skills developed in this assignment will be applied in:
- Future PL/SQL laboratory sessions
- Database design and development projects
- Enterprise database administration scenarios
- Performance tuning and optimization tasks
- Multi-tenant application deployments

---

## Acknowledgments

I would like to thank:
- **Instructor Eric Maniraguha** for clear assignment guidelines and expectations
- **AUCA IT Department** for providing Oracle database infrastructure
- **Oracle Corporation** for comprehensive documentation and learning resources

---

**Note:** This assignment demonstrates foundational Oracle Database Administration skills that are critical for professional database development and management roles.

---

*Repository maintained by: Muggisha (29190)*  
*Course: Database Development with PL/SQL (INSY 8311)*  
*Academic Year: 2025-2026*  
*Institution: Adventist University of Central Africa (AUCA)*

---

## Assignment Completion Status

| Task | Description | Status | Evidence |
|------|-------------|--------|----------|
| Task 1 | Create PDB mu_pdb_29190 | ✅ Complete | 4 screenshots |
| Task 1 | Create user muggisha_plsqlauca_29190 | ✅ Complete | Verified |
| Task 2 | Create temporary PDB | ✅ Complete | Screenshots provided |
| Task 2 | Delete temporary PDB | ✅ Complete | Deletion verified |
| Task 3 | Access OEM Dashboard | ✅ Complete | Dashboard screenshot |
| Task 4 | GitHub Documentation | ✅ Complete | This README |
| Submission | Google Form Submission | ✅ Complete | On time |

**Overall Completion:** 100% ✅

--
