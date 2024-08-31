### Step 1: Download Oracle 19c for Windows

1. **Visit the Oracle Website:**
   - Go to the [Oracle Database Software Downloads page](https://www.oracle.com/database/technologies/oracle19c-windows-downloads.html).
   - Download the Oracle Database 19c (Windows x64) installer.

2. **Unzip the Downloaded Files:**
   - Extract the downloaded ZIP file to a directory on your system (e.g., `C:\Oracle19c`).

### Step 2: Install Oracle Database 19c

1. **Launch the Installer:**
   - Navigate to the extracted directory and run the `setup.exe` file.

2. **Configure Security Updates:**
   - On the "Configure Security Updates" screen, you can choose to provide an email address to receive security updates. You can also uncheck the box if you do not wish to receive updates.

3. **Select Installation Option:**
   - Choose **"Create and configure a single instance database"** and click **"Next."**

4. **System Class:**
   - Select **"Desktop class"** if you are installing on a personal computer. Choose **"Server class"** for a more advanced setup on a server environment.

5. **Specify Oracle Home User:**
   - You can choose to use an existing Windows user or create a new Windows user as the Oracle Home User. For simplicity, you can select **"Use Virtual Account"**.

6. **Specify Oracle Base and Software Location:**
   - Oracle Base: `C:\app\oracle\` (or any other preferred directory).
   - Software Location: `C:\app\oracle\product\19.0.0\dbhome_1` (default location).

7. **Database Edition:**
   - Choose the **"Enterprise Edition"** or **"Standard Edition 2"** depending on your license.

8. **Specify Database Identification:**
   - Enter a Global Database Name (e.g., `orcl`).
   - Oracle System Identifier (SID): `orcl` (default).

9. **Specify Configuration Options:**
   - **Storage Type:** Choose "File System" unless you need Oracle ASM (Advanced Storage Management).
   - **Character Set:** Choose **"Use Unicode (AL32UTF8)"** for broader compatibility.
   - **Sample Schemas:** Make sure to check the option to install **"Sample Schemas"**.

10. **Specify Administrative Passwords:**
    - Set passwords for the SYS, SYSTEM, and PDBADMIN accounts.

11. **Configuration Summary:**
    - Review the installation summary and click **"Install"** to begin the installation process.

### Step 3: Post-Installation Setup

1. **Verify Database Installation:**
   - Once the installation is complete, the Oracle Database Configuration Assistant (DBCA) will automatically create and configure the database.

2. **Access the Database:**
   - You can access the database using SQL*Plus or Oracle SQL Developer.
   - Example command to log in using SQL*Plus:
     ```bash
     sqlplus sys as sysdba
     ```

3. **Check Installed Sample Schemas:**
   - Run the following SQL query to verify that the sample schemas (HR, OE, PM, etc.) are installed:
     ```sql
     SELECT username FROM dba_users WHERE username IN ('HR', 'OE', 'PM', 'SH', 'IX', 'BI', 'SCOTT');
     ```

### Step 4: Verify and Use Sample Schemas

1. **Verify Sample Schema Data:**
   - Run sample queries to ensure that data is available in the sample schemas.
     ```sql
     SELECT * FROM hr.employees;
     SELECT * FROM oe.orders;
     ```

2. **Unlock Sample Schema Accounts:**
   - By default, the sample schema accounts may be locked. Unlock these accounts using the following commands:
     ```sql
     ALTER USER hr IDENTIFIED BY hr ACCOUNT UNLOCK;
     ALTER USER oe IDENTIFIED BY oe ACCOUNT UNLOCK;
     ALTER USER pm IDENTIFIED BY pm ACCOUNT UNLOCK;
     ALTER USER sh IDENTIFIED BY sh ACCOUNT UNLOCK;
     ALTER USER ix IDENTIFIED BY ix ACCOUNT UNLOCK;
     ALTER USER bi IDENTIFIED BY bi ACCOUNT UNLOCK;
     ```

3. **Change Sample Schema Passwords (Optional):**
   - Change the default passwords if needed:
     ```sql
     ALTER USER hr IDENTIFIED BY new_password;
     ```

### Step 5: Oracle Enterprise Manager (OEM) Setup (Optional)

1. **Access Oracle Enterprise Manager:**
   - Open a web browser and navigate to the Oracle Enterprise Manager URL:
     ```url
     https://localhost:5500/em
     ```
   - Log in using the `SYS` account with the `SYSDBA` role.

2. **Monitor and Manage the Database:**
   - Use Oracle Enterprise Manager to manage database operations, monitor performance, and configure additional features.

### Final Notes:
- Ensure your system meets the [Oracle 19c system requirements](https://docs.oracle.com/en/database/oracle/oracle-database/19/install-and-upgrade.html) before starting the installation.
- Regularly back up your Oracle database using RMAN (Recovery Manager) or other tools provided by Oracle.

Following these steps will help you successfully install and configure Oracle 19c on Windows, including the sample schemas.
