.. _admin_oracle:

--------------------------
DB Administration with Era
--------------------------

We will now see how to perform normal database admin task with Era.

**In this lab you will Administor your ORACLE DB**

Explore Your Database
++++++++++++++++++++++

#. In **Era**, select **Databases** from the dropdown menu and **Sources** from the lefthand menu.

   .. figure:: images/1.png

#. Click into your *Initials*\ **-proddb**, this will take you back into the Database Summary page. This page provides details of the Database, Database Server access, Time Machine schedule, Compute/Network/Software profiles used to provision.

    - **Database Summary:**

    .. figure:: images/2.png

    - **Database Server:**

    .. figure:: images/3.png

    - **Time Machine:**

    .. figure:: images/4.png

    - **Profiles:**

    .. figure:: images/5.png

Snapshot Your Database
++++++++++++++++++++++

Before we take a manual snapshot of our Database, lets write a new table into our ProdDB.

Write New Table Into Database
.............................

#. SSH (Terminal/Putty) into your *Initials*\ -proddb VM

   - **User Name** - oracle
   - **Password** - Nutanix/4u

   .. code-block:: Bash

      ssh oracle@PRODDB IP

#. Launch **sqlplus**

   - **User Name** - sysdba
   - **Password** - oracle

   .. code-block:: Bash

      sqlplus

#.

Take Manual Snapshot of Database
................................

#. In **Era**, select **Databases** from the dropdown menu and **Sources** from the lefthand menu.

#. Click on the Time Machine for your Database *Initials*\ -proddb_TM

   .. figure:: images/

#. Click **Actions > Snapshot**.

   .. Figure:: images/

   - **Snapshot Name** - *Initials*\ -proddb-1st-Snapshot

   .. Figure:: images/

#. Click **Create**

#. Select **Operations** from the dropdown menu to monitor the registration. This process should take approximately 2-5 minutes.

Clone Your Database Server & Database
+++++++++++++++++++++++++++++++++++++

#. In **Era**, select **Time Machines** from the dropdown menu and select *Initials*\ -proddb_TM

#. Click **Actions > Clone Database > Single Node Database**.

   - **Snapshot** - *Initials*\ -proddb-1st-Snapshot (Date Time)

   .. figure:: images/

   - **Database Server** - Create New Server
   - **Database Server Name** - *Initials*\ -proddb_Clone1
   - **Compute Profile** - ORACLE_SMALL
   - **Network Profile** - Primary-ORACLE-Network
   - **Administrator Password** - Nutanix/4u

   .. figure:: images/

   - **Clone Name** - *Initials*\ _proddb

   .. figure:: images/

#. Click **Clone**

#. Select **Operations** from the dropdown menu to monitor the registration. This process should take approximately 30-50 minutes.

Delete Table and Clone Refresh
++++++++++++++++++++++++++++++

There are times when a table or other data gets deleted (by accident), and you would like to get it back. here we will delete a table and use the Era Clone Refresh action from the last snapshot we took.

Delete Table
............

#. SSH (Terminal/Putty) into your *Initials*\ -proddb_Clone1 VM

   - **User Name** - oracle
   - **Password** - Nutanix/4u

   .. code-block:: Bash

      ssh oracle@PRODDB_Clone1 IP

#. Launch **sqlplus**

   - **User Name** - sysdba
   - **Password** - oracle

   .. code-block:: Bash

      sqlplus

#.

Clone Refresh
.............

#. In **Era**, select **Databases** from the dropdown menu and **Clones** from the lefthand menu.

#. Select the Clone for your Database *Initials*\ _proddb and Click **Refresh**.

   - **Snapshot** - *Initials*\ _proddb-1st-Snapshot (Date Time)

#. Click **Refresh**

#. Select **Operations** from the dropdown menu to monitor the registration. This process should take approximately 2-5 minutes.

Verify Table is Back
....................

#. SSH (Terminal/Putty) into your *Initials*\ -proddb_Clone1 VM

   - **User Name** - oracle
   - **Password** - Nutanix/4u

   .. code-block:: Bash

      ssh oracle@PRODDB_Clone1 IP

#. Launch **sqlplus**

   - **User Name** - sysdba
   - **Password** - oracle

   .. code-block:: Bash

      sqlplus

#.

Takeaways
+++++++++
