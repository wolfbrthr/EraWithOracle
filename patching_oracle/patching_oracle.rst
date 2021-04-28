.. _patching_oracle:

------------------------
Patching Oracle with Era
------------------------

Maintaining consistent patch levels across database servers in a traditional environment can be a very difficult process. Era makes this simple by providing a means of database engine patching through versioned software profiles. Groups of database servers can be patched or rolled back through Era using the web interface, via CLI, or API.

Each quarter, Oracle releases a grouping of patches referred to as a PSU. In this lab you will walk through the deployment and patching of both Oracle and Grid software for an Oracle 19c database using Era.

Patching Base Oracle VM
+++++++++++++++++++++++

In this exercise, you will apply the October PSU patches to your manually cloned VM, register the database server with Era, and then use it as the basis for creating a new version of your *UserXX*\ **_ORACLE_19C** Software Profile.

#. In **Prism Central**, note the IP address of your *UserXX*\ **_oracle_patched** VM.

#. Connect to your *UserXX*\ **-Oracle19cSource-Patched** VM via SSH using the following credentials:

   - **User Name** - `root`
   - **Password** - `Nutanix/4u`

#. Execute the following script to download and install the Oracle and Grid October PSU patches:

    .. code-block:: bash

      cd Downloads
      ./applypsu.sh

#. Observe that the script will first display the current patch level of the VM, note the April dates on the displayed releases. Press any key to continue the patch installation.

   .. figure:: images/7.png

#. If prompted, type **A** to overwrite any existing files while extracting the patch and follow any prompts to press any key to continue. The script should run for approximately 15-20 minutes.

   .. figure:: images/8.png

#. Once the script has finished, you may exit the Terminal/Putty session, and return to **Era > Database Server VMs > List**.

#. Click **+ Register > Oracle**, and then fill out the following **Database Server** fields:

   - **Nutanix Cluster** - EraCluster (default)
   - **IP Address or Name of VM** - *UserXX*\ **-Oracle19cSourceVM_Patched**
   - **Listener Port** - 1521 (default)
   - **Era Drive User** - `oracle`
   - **Oracle Database Home** - `/u02/app/oracle/product/19.0.0/dbhome_1`
   -  **Grid Infrastructure Home** - `/u01/app/19.0.0/grid`
   - **Provide Credentials Through** - Password
   - **Password** - `Nutanix/4u`

   .. figure:: images/9.png

#. Click **Register**

#. Once again, take note of *Missing Dependencies/Configurations* dialogue box. In this instance, *sshpass* is only required for Oracle RAC Patching, and can safely be ignored (i.e. warning for soft dependencies). Era provides these built-in checks to ensure a clean experience moving forward.

   .. figure:: images/2a.png

#. Click **Close**.

#. Select **Operations** from the dropdown menu to monitor the progress. This process should take <5 minutes. Please wait for the *Register Database Server VM* operation to successfully complete before moving on to the next step.

#. Select **Profiles** from the dropdown menu, and then **Software** from the left-hand menu.

#. Click your *UserXX*\ **_ORACLE_19C** Software Profile. Observe that Era provides complete introspection into the packages installed within the operating system, including the *Database Software* and *Grid Infrastructure Software*. Note the *Patches/Bug Fixes Found* list under *Database Software*.

   .. figure:: images/10.png

#. Click **+ Create** to create a new version based on the *UserXX*\ **-Oracle19cSource_Patched** VM you registered in the previous step.

#. Fill out the following fields and click **Next**:

   - **Name** - *UserXX*\ _ORACLE_19C (Oct19 PSU)
   - **Nutanix Cluster** - EraCluster
   - Select *UserXX*\ **-Oracle19cSource_Patched**

   .. figure:: images/10b.png

#. Add Software Profile Version Notes – (Optional)

   .. figure:: images/10c.png

#. Click **Create**.

#. Select **Operations** from the dropdown menu to monitor the progress. This process should take <5 minutes. Please wait for the *Create Software Profile Version* operation to successfully complete before moving on to the next step.

#. Return to **Era > Profiles > Software** and click your *UserXX*\ **_ORACLE_19C** Software Profile. Note the 2.0 version now appears, with additional patches found under **Database Software** and **Grid Infrastructure Software**.

   .. figure:: images/12.png

   Before you can apply the patched Software Profile to your *UserXX*\ **_oracle_prod** VM, the Software Profile must first be published, otherwise Era will not show the version as available or recommended for updating.

#. Select the **2.0** profile, and then click **Update**.

#. Under *Status*, select **Published**.

#. Under *Published By*, select **By publishing this version of the software profile, I understand that Era will recommend that all databases using an earlier versions of this software profile should update to this new version. The recommendation will appear on the Database Server VM home page.**

#. Click **Next**.

   .. figure:: images/13.png

#. Add Software Profile Version notes – (optional), and then click **Update**.

#. Return to **Era > Database Server VMs > List** and click on your *UserXX*\ **_oracle_prod** database server.

#. Under *Software Profile Version*, note that the newer, published software profile is being recommended as an available update to the database server. Click **Update**.

   .. figure:: images/15.png

#. Fill out the following fields, and then click **Update**.

   - **Update to Software Profile Version** - Select the desired patch profile from the drop-down menu (note: in a real environment you could potentially publish several options).
   - **Start Update** – Now

   - **Confirm this request by providing the name of the Database Server VM** UserXX_oracle_prod
   - Click **Update**


   .. figure:: images/16.png

   .. note::

      Era also offers the ability to schedule patching application, allowing you to select a pre-determined maintenance window. For clustered database deployments, Era supports rolling updates, ensuring database accessibility throughout the update process.

#. Select **Operations** from the dropdown menu to monitor the progress. This process should take 20-40 minutes. Please wait for the *Patch Database Server VM* operation to successfully complete before moving on to the next step.

   During the patching process, Era will gracefully bring down database and Grid services, shut down the VM, replace the relevant virtual disks with thin clones from the 2.0 Software Profile, and bring the database server back online.

#. Once the patching operation has completed, you can easily validate the VM is running with the patched software outside of Era. SSH into your *UserXX*\ **_oracle_prod** VM with the following credentials:

   - **User Name** - `oracle`
   - **Password** - `Nutanix/4u`

#. Execute the following command to display installed patch versions:

   .. code-block:: bash

      $ORACLE_HOME/OPatch/opatch lsinventory | egrep 'appl|desc'

   .. figure:: images/19.png

Takeaways
+++++++++

What are the key things we've learned in this lab?

- Software Profiles can be versioned and used to deploy consistent updates to existing database servers
- Software Profiles also simplify the patching process reducing the amount of manual patching needed in an environment
- Scheduling updates can be used to hit change windows or SLA uptime windows.
