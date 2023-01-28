# EZMENU - Simple Dynamic ISPF Menu

EZMENU is a simple way to build a ISPF menu without having to deal with
creating, or updating, an ISPF Panel.

## Installation and Configuration/Usage

### Installation

0. If you received this package via Git then execute the
   zginstall.rex (follow the prompts) and skip (1)
1. Execute the $RECV member to create the EXEC and PANELS datasets
   - applies only if you get this package from the www.cbttape.org
3. Copy the EXEC library into a library in your SYSEXEC (or SYSPROC)
   library concatenation in your TSO/ISPF Logon Procedure.
3. Copy the PANELS library into a library in your ISPPLIB library
   concatenation in your TSO/ISPF Logon Procedure.

### Configuration

Create a member in the same REXX library where EZMENU was copied under
any name (e.g. YOURMENU) with the following control records:

    *----------*-------------------------------------------*
    | Title:   | The Title to be used on the ISPF Menu     |
    *----------*-------------------------------------------*
    | Sort:    | Yes (Default) or No to sort the items     |
    *----------*-------------------------------------------*
    | Help:    | The name of an ISPF Tutorial Panel        |
    *----------*-------------------------------------------*
    | Command: | The Name of the Command or Application    |
    *----------*-------------------------------------------*
    | Desc:    | Description of the Command or Application |
    *----------*-------------------------------------------*
    | Action:  | The Action to take when selected          |
    *----------*-------------------------------------------*

The case is not important except for the description text.

All records must start in column 1. Any record that does not have one of
these keywords will be ignored.

The supported actions are:

    *---------------------*--------------------------------------*
    |     CMD(XXX)        | SELECT CMD(XXX) Newappl(cbt) Passlib |
    |     PGM(XXX)        | SELECT PGM(XXX)                      |
    |     PANEL(XXXX)     | SELECT PANEL(XXXX)                   |
    |     TSO xxxx        | Address TSO xxxx                     |
    |     DOCB/DOCV xxxx  | Browse or View doc member xxxx       |
    |     BROWSE dsn      | Browse dsn or dsn(member)            |
    |     EDIT   dsn      | Edit dsn or dsn(member)              |
    |     VIEW   dsn      | View dsn or dsn(member)              |
    *---------------------*--------------------------------------*
Notes:
 * If &zparm is found in any then a popup prompt for command options
   will be presented to the user.

 * The SCRNAME will be the command/application name for all but TSO.

# Sample Data

Must start in column 1

    Title: CBT Starter Kit ISPF Menu
    Sort: Yes
    Help: cbtmenuh
    Command: Document
    Desc: Some documentation for the included tools
    Action: DOCB
    Command: EDSL
    Desc: Enhanced Data Set List (312)
    Action: CMD(%edsl)
    Command: TRYIT
    Desc: Powerful Edit command to  test REXX, ISPF Panels, etc.
    Action: pgm(isptutor) parm(#tryit)

# Usage

To use your new menu invoke the EZMENU exec with a parameter of the
data member that you created.

From any ISPF command line or ISPF Option 6:

     TSO %EZMENU menu-data command

     menu-data is the data file from which the menu is constructed
     or
     menu-data is a dataset name referencing the menu data
        - data.set(menu)    * for a pds member
        - menu.data.set     * if sequential

     command is optional and is the command to invoke by name

As a ISPF Panel entry:

      CMD(%ezmenu cbtmenu &ztrail)
      or
      CMD(%ezmenu 'hlq.menu.pds(cbtmenu)' &ztrail)

As an ISPF Command Table entry:

       Select Cmd(%ezmenu cbtmenu &zprim)
       or
       Select CMD(%ezmenu 'hlq.menu.pds(cbtmenu)' &ztrail)

# License and Warranty

This is released under the MIT License (google it) and comes with
no warranty expressed or implied.
