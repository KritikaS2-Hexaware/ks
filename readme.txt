Oracle SQL Developer, v1.5.0.54.40
Release Notes


1.  Known Issues

1.1  General
- Print prints only one page that is a truncation of the current tab.
- Can't invoke SQL*Plus on Windows 2003.
- The menu item, and right-click off a Connection node, for invoking SQL*Plus does not work with connections whose passwords are not persisted.

1.2  Connections
- Cannot connect to remote database as OPS$ account.

1.3  Browse
- If connected as sys with sysdba role, Types node displays built in data-types (e.g. BLOB, DATE, DECIMAL, etc.)  If clicked on, will only see "create or replace".

1.4  Creating and Modifying Objects
- Editing Triggers - If you have comments before the 'BEGIN' they will be lost if you edit.  You will see when you click edit that they will not be there.  To preserve them, they need to be below the BEGIN or you will need to edit via the SQL Worksheet.

1.5  Table > Data
- Tables > Your_Table > Data - PageUp and PageDown buttons not working correctly if cursor is in the rownum column.

1.6  Export
- Cannot export if result set contains duplicate column names.


2.  Workarounds

2.1  To disable Code Insight
Run SQL Developer from a command line using the following statement:
   Windows : sqldeveloper -J-Dsdev.insight=false
   Linux or Mac: Run sh sqldeveloper -J-Dsdev.insight=false
or edit sqldeveloper.conf and add "AddVMOption -J-Dsdev.insight=false"

2.2  If DDL tab is null for all objects in a Connection
Your dbms_metadata might be loaded incorrectly.
If  this statement fails when executed in a SQL Worksheet against the Connection
   select dbms_metadata.get_ddl('TABLE',table_name , user ) from user_tables;
You need to reload $ORACLE_HOME/rdbms/admin/catmeta.sql

2.3  If Snippets are not accessible
You may have not done a clean install.  SQL Developer needs to be installed into a clean directory, not over a previous release.


3.  Accessibility Issues
The following is a list of known issues regarding accessibility using JAWS.

  - Connection dialog - success or failure of connection not read 
  - Connection Delete Confirmation Window - message not read
  - Connections > right-click Export - not read
  - Connections > Tables > right-click - not reading clear filter
  - Connections > Database Link - context menu and labels not read 
  - SQL Worksheet > tab names not read 
  - SQL Worksheet > Results - not read
  - SQL Worksheet > Results > Export - reads incorrectly 
  - Messages Log - not reading the contents
  - Key board navigation from Connections to Reports not possible 
  - New Gallery - not reading the contents, reads only dialog's title




4.  Globalization

4.1  Character Set Support
SQL Developer can connect to an Oracle Database irrespective of the database character set and the database national (NCHAR) character set.  All functionality which does not involve entering or displaying characters outside of the ASCII character set is expected to work correctly with any such database.

Due to bug #5088284 in the JDBC 10.2 driver, the database character set ZHT16HKSCS31 is currently not supported.

Support for displaying and entering characters in scripts outside of the ASCII repertoire is limited to those fulfilling the following conditions:

  - The character belongs to the database or to the national character set, depending on the datatype of the value containing the character, and
  - The script to which the character belongs is supported by the JRE installation on which SQL Developer is running � for example, appropriate fonts are available � and
  - The script does not require complex rendering, e.g. it is written left-to-right with no ligatures or character reordering.

Complex scripts, like Arabic, Hebrew, or Devanagari may work in many places, but they have not been tested and they are not guaranteed to work reliably.  For bidirectional scripts, e.g. Arabic and Hebrew, no functionality is provided to right-align displayed columns, nor to set directionality.  Full support for those languages is planned for a future release.


4.2  User Interface Translation
SQL Developer version 1.0 is available with English user interface only.  If the locale of the operating system is not English, a few UI elements, like default wizard buttons and Oracle error messages, may show up in the language of this locale.  If the locale of the operating system is Japanese, part of the user interface will appear in Japanese.  These translations come from the framework on which SQL Developer is built.  Full translation into selected languages is planned for a future release.


4.3  Support for Locale-Sensitive Date and Number Formatting
SQL Developer uses current database NLS session settings to format date/time and numeric datatypes.  If the settings are changed with the ALTER SESSION command, SQL Developer reads the new values from the database and use them for subsequent formatting.

Do not change the session time zone with ALTER SESSION.  To switch the time zone used for formatting, change the client O/S time zone setting and restart SQL Developer.


4.4  Known Globalization Issues
This version of SQL Developer has the following globalization issues:

  - Bug # 4918539:  ORA-ORA-06502 or ORA-01460 may occurs if a procedure is executed through the Run PL/SQL dialog box and a string with multibyte characters is assigned to one of the parameters.

    Workaround:  Run the procedure from the SQL Worksheet.  No debugging will be possible. 

  - Bug 4918586:  National character set datatypes (NCHAR, NVARCHAR2, NCLOB) are converted to the database character set datatypes (CHAR, VARCHAR2, CLOB) in the Run PL/SQL dialog box.

    Workaround:  Change the datatypes of local variables in the anonymous call block back to national character set datatypes before clicking on the �OK� button.

  - Bug 4924432:  Object names entered into SQL Developer are not validated against the database character set.

    Workaround:  Make sure that all characters in names you specify are valid in the database character set.

  - Bug 4924484:  Values to which NCHAR, NVARCHAR2, or NCLOB columns are modified may be unnecessarily converted to the database character set before being converted to the national character set and stored in the column.  This may lead to data loss, if the database character set is not AL32UTF8.

    Workaround:  None for the Data tab.  You may still update the column with an explicit UPDATE statement referencing the SQL UNISTR function.

  - Bug 4924518:  Only the SQL Worksheet statement input area (code editor) uses a configurable font.

    Workaround:  None.

  - Bug 4940989:  Only ASCII characters work in names and definitions of User Defined Reports.  Other characters become unreadable after SQL Developer has been restarted.

    Workaround:  None.  Use only ASCII characters.

  - Bug 5080543:  When an Oracle object definition  is opened for editing and the database character set is multibyte, CREATE OR REPLACE keywords may appear twice.

    Workaround:  Remove the superfluous keywords manually.

  - Bug 5084654:  The Columns tab for tables and views misses the length semantics indicator for character columns (BYTE vs. CHAR).

    Workaround:  Check the indicator in the generated DDL statement on the SQL tab.

  - Bug 5084687:  When a table to be edited contains a column with character length semantics, the Edit Table dialog box will show the byte length instead of the character length of this column.

    Workaround:  Correct the length manually.

  - Bug 5084677:  When a table to be edited contains a column with byte length semantics, the Edit Table dialog box will not initialize the value in the Units pop-up list to BYTE.

    Workaround:  Select the BYTE value manually.  This is not necessary, if the current session value of the NLS_LENGTH_SEMANTICS parameter is �BYTE�.

  - Bug 5085334:  If a numeric value is opened for editing in the Data tab for the first time after the table is refreshed, the decimal separator will always become dot, irrespective of the locale preference.

    Workaround:  Correct the separator manually.

  - Bug 5087402:  BINARY_FLOAT and BINARY_DOUBLE columns are always formatted using dot as the decimal separator.  When values of these datatypes are inserted or updated, the decimal separator must correspond to the locale preference.

    Workaround:  Correct the separator manually.
