# Order Entry Sample Schema  (ARCHIVED!)

   > [Oracle Official GitHub Directory](https://github.com/oracle-samples/db-sample-schemas/tree/main/order_entry)


## DESCRIPTON

Order Entry (`OE`) is a sample schema modeling a company's order system.

### SCHEMA VERSION

**ARCHIVED!**

This schema is archived and no longer maintained.

### RELEASE DATE

08-APR-2021

### SUPPORTED with DB VERSIONS

19c and lower

## INSTALL INSTRUCTIONS
1. Run `perl -p -i.bak -e 's#__SUB__CWD__#'$(pwd)'#g' *.sql */*.sql */*.dat` in this folder
2. Connect as privileged user with rights to create another user (`SYSTEM`, `ADMIN`, etc.)
3. Run the `oe_main.sql` script passing on the following parameters:
    1. `OE` schema name password
    2. The tablespace name where to install the schema into
    3. The temporary tablespace name for the `OE` user
    4. The password of the `HR` user
    5. The `SYS` password
    6. The full path of this parent directory
    7. The full path of a directory where to write the install log file to
    8. The version number `3`
    9. The connect string for the database you connected to

**Note:** If the `OE` schema already exists, it is removed/dropped and 
        a fresh `OE` schema is installed.

## UNINSTALL INSTRUCTIONS

1. Run as privileged user with rights to drop another user (`SYSTEM`, `ADMIN`, etc.)
2. Run `DROP USER oe CASCADE;`

## NOTES
This schema is archived and no longer maintained.
