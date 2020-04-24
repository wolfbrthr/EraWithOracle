.. _deploy_oracle:

-----------------
Deploying Oracle
-----------------

Each quarter, Oracle releases a grouping of patches referred to as a PSU. **In this lab you will walk through the deployment and patching of both Oracle and Grid software for an Oracle 19c database using Era.**

Manual Oracle VM Deployment
+++++++++++++++++++++++++++

In this exercise, you will deploy an Oracle database VM, using pre-created disk images. This VM is the running Oracle 19c with April PSU patches applied.

#. In **Prism Central**, select :fa:`bars` **> Virtual Infrastructure > VMs**.

#. Click **Create VM**.

#. Select your assigned cluster and click **OK**.

#. Fill out the following fields:

   - **Name** - *Initials*\ _oracle_base
   - **Description** - (Optional) Description for your VM.
   - **vCPU(s)** - 2
   - **Number of Cores per vCPU** - 1
   - **Memory** - 8 GiB

   - Select **+ Add New Disk**
      - **Type** - DISK
      - **Operation** - Clone from Image Service
      - **Image** - 19c_bootdisk.qcow2
      - Select **Add**

   - Select **+ Add New Disk**
      - **Type** - DISK
      - **Operation** - Clone from Image Service
      - **Image** - 19c_disk1.qcow2
      - Select **Add**

   - Select **+ Add New Disk**
      - **Type** - DISK
      - **Operation** - Clone from Image Service
      - **Image** - 19c_disk2.qcow2
      - Select **Add**

   - Select **+ Add New Disk**
      - **Type** - DISK
      - **Operation** - Clone from Image Service
      - **Image** - 19c_disk3.qcow2
      - Select **Add**

   - Select **+ Add New Disk**
      - **Type** - DISK
      - **Operation** - Clone from Image Service
      - **Image** - 19c_disk4.qcow2
      - Select **Add**

   - Select **+ Add New Disk**
      - **Type** - DISK
      - **Operation** - Clone from Image Service
      - **Image** - 19c_disk5.qcow2
      - Select **Add**

   - Select **+ Add New Disk**
      - **Type** - DISK
      - **Operation** - Clone from Image Service
      - **Image** - 19c_disk6.qcow2
      - Select **Add**

   - Select **+ Add New Disk**
      - **Type** - DISK
      - **Operation** - Clone from Image Service
      - **Image** - 19c_disk7.qcow2
      - Select **Add**

   - Select **+ Add New Disk**
      - **Type** - DISK
      - **Operation** - Clone from Image Service
      - **Image** - 19c_disk8.qcow2
      - Select **Add**

   - Select **+ Add New Disk**
      - **Type** - DISK
      - **Operation** - Clone from Image Service
      - **Image** - 19c_disk9.qcow2
      - Select **Add**

   - Select **Add New NIC**
      - **VLAN Name** - *Assigned User VLAN*
      - Select **Add**

#. Click **Save** to create the VM.

   You will now create a copy of this VM which will later be used to install October PSU patches.

#. Once the VM has been created, select your *Initials*\ **_oracle_base** and click **Actions > Clone**.

   .. figure:: images/1.png

#. Change the name to *Initials*\ **_oracle_patched** and click **Save**.

#. Select both VMs and click **Actions > Power On**.

Register Oracle Server with Era
+++++++++++++++++++++++++++++++

In this exercise, you will register your April PSU VM and register it as version 1.0 of your Oracle 19c Software Profile. The Software Profile is a template containing both the operating system and database software, and can be used to deploy additional database servers.

#. In **Era**, select **Database Servers** from the dropdown menu and **List** from the lefthand menu.

#. Click **+ Register**.

#. Click **+ Register** and fill out the following **Database Server** fields:

   - **Engine** - Oracle
   - **IP Address or Name of VM** - *Initials*\ _oracle_base
   - **Database Version** - 19.0.0.0
   - **Era Drive User** - oracle
   - **Oracle Database Home** - /u02/app/oracle/product/19.0.0/dbhome_1
   - **Grid Infrastructure Home** - /u01/app/19.0.0/grid
   - **Provide Credentials Through** - Password
   - **Password** - Nutanix/4u

   .. note::

      The Era Drive User can be any user on the VM that has sudo access with NOPASSWD setting. Era will use this user's credentials to perform various operations, such as taking snapshots.

      Oracle Database Home is the directory where the Oracle database software is installed, and is a mandatory parameter for registering a database server.

      Grid Infrastructure Home is the directory where the Oracle Grid Infrastructure software is installed. This is only applicable for Oracle RAC or SIHA databases.

   .. figure:: images/2.png

#. Click **Register** and monitor the progress on the **Operations** page. This process should take approximately 2 minutes. Wait for the registration operation to successfully complete before moving on.

   Once the *Initials*\ **_oracle_base** server has been registered with Era, we need to create a software profile in order to deploy additional Oracle VMs.

#. Select **Profiles** from the dropdown menu and **Software** from the lefthand menu.

#. Click **+ Create** and fill out the following fields:

   - **Engine** - Oracle
   - **Type** - Single Instance
   - **Name** - *Initials*\ _ORACLE_19C
   - **Description** - (Optional)
   - **Database Server** - Select your registered *Initials*\ _oracle_base VM

   .. figure:: images/3.png

#. Click **Create**.

#. Select **Operations** from the dropdown menu to monitor the registration. This process should take approximately 5 minutes.

#. Once the profile creation completes successfully, power off your *Initials*\ **_oracle_base** VM in Prism.
