# Infrastructure Configuration

## Introduction

In this lab we will build the infrastructure that we will use to run the rest of the workshop. 

The main three elements that we will be creating are a **Virtual Cloud Network** which helps you define your own data center network topology inside the Oracle Cloud by defining some of the following components (Subnets, Route Tables, Security Lists, Gateways, etc.), **Compute instance** using an image from the marketplace including the libraries need to execute the scripts needed to create and execute applications in Python. And finally an **Autonomous JSON Database** where we will allocate the JSON documents that we will ingest our Python apps with.

We will prepare the documents to be capable of accesing to **SODA APIs**, in particular, to create, drop, and list document collections using **APEX** as vehicle to visualize the JSON documents as we used to do with structure data. Tha capability is unique of Oracle Databases.

**Estimated Lab Time: 40 minutes.**

### Objectives

In this lab, you will:

* Create Virtual Cloud Network (VCN)
* Provision Compute Node for development
* Provision Oracle Autonomous JSON Database (AJD)
* Prepare Document Store

### Prerequisites

* An Oracle Free Tier, Always Free, or Paid Oracle Cloud Account


## Task 1: Create Virtual Cloud Network (VCN)

1. Login to Oracle cloud console: [cloud.oracle.com](https://cloud.oracle.com/)

    - Cloud Account Name: oci-tenant
    - **Next**
    
    ![cloud Account Name](./images/task1/cloud-account-name.png)

    - User Name: oci-username - email address provided
    - Password: oci-password
    - **Sign In**

    ![User Name & Password](./images/task1/username-password.png)    

2. Click on main menu ≡, then Networking > **Virtual Cloud Networks**. Select your Region and Compartment assigned by the instructor. 

    >**Note**: Use **Root** Compartment, oci-tenant(root), to create all resources for this workshop.

    ![Oracle Console Networking](./images/task1/oracle-console-networking.png)

3. Select your Region and Compartment assigned by the instructor. Click **Start VCN Wizard**.
    
    ![Oracle Console Networking Start Wizard](./images/task1/oracle-console-networking-start-wizard.png)

4. Select **Create VCN with Internet Connectivity**. Start **VCN Wizard**.

   ![Create VCN with Internet Connectivity](./images/task1/create-vcn-with-internet-connectivity.png)

5. Provide the following information:

    - VCN Name: **DEVCN**
    ```
    <copy>DEVCN</copy>
    ```
    - Compartment: Be sure you have selected the correct one for this workshop purpose. **Root** is the recommended one
    - Click **Next**

    ![vcnName & Compartment](./images/task1/vcn-name-compartment.png)

6. Review the information in the 'Review and Create Page' and Click **Create**.

    ![vcn Creation](./images/task1/vcn-creation.png)

7. The Resources have being created on the next page. Click **View Virtual Cloud Network** to access to the vcn.

    ![View vcn Page](./images/task1/view-vcn-page.png)
    ![DEVCN Detail](./images/task1/devcn-detail.png)

8. Click **Public Subnet-DEVCN**. 

    ![Public Subnet](./images/task1/public-subnet.png)

9. Click **Default Security List for DEVCN**.
    
    ![Default Security List for DEVCN](./images/task1/default-security-list-for-devcn.png)

10. Click **Add Ingress Rules**.

    ![Add Ingress Rules](./images/task1/add-ingress-rules.png)

11. Provide the following information:

    - CIDR Block: **0.0.0.0/0**
    ```
    <copy>0.0.0.0/0</copy>
    ```
    - Destination Port Range: **5000**
    ```
    <copy>5000</copy>
    ```
    - Description: **Python Flask**
    ```
    <copy>Python Flask</copy>
    ```
    - Click **+ Another Ingress Rule**

    ![Python Flask Rule](./images/task1/python-flask-rule.png)

12. Provide the following information:

    - CIDR Block: **0.0.0.0/0**
    ```
    <copy>0.0.0.0/0</copy>
    ```
    - Destination Port Range: **80**
    ```
    <copy>80</copy>
    ```
    - Description: **HTTP**
    ```
    <copy>HTTP</copy>
    ```
    - Click **Add Ingress Rules**
    
    ![Port 80 Rule](./images/task1/port80-rule-new.png)

13. You can check on the **Detail Page** that the 2 Ingress Rules have beed added.
    
    ![All Ingress Rules Added](./images/task1/all-ingress-rules-added-new.png)




1. On the Oracle Cloud Infrastructure Console, click **Database Actions** next to the big green box. Allow pop-ups from cloud.oracle.com.

    ![DB Actions](./images/task4/db-actions.png)

    If you need to **Sign in** again remember doing it as admin:
    - User: **admin**
    ```
    <copy>admin</copy>
    ```
    - Password: **DBlearnPTS#22_**
    ```
    <copy>DBlearnPTS#22_</copy>
    ```

2. Under the **Administration** section, click on **Database Users**.

    ![DB Actions - Database Users](./images/task4/database-actions-database-users.png)

3. Click **+ Create User**.

    ![DB Actions - Create User](./images/task4/create-user.png)

4. Create the new user using the following information in the **User** tab:

    - User Name: **DEMO**
    ```
    <copy>DEMO</copy>
    ```
    - Password: **DBlearnPTS#22_**
    ```
    <copy>DBlearnPTS#22_</copy>
    ```
    - Confirm Password: **DBlearnPTS#22_**
    ```
    <copy>DBlearnPTS#22_</copy>
    ```
    - Quota on tablespace DATA: **UNLIMITED**
    - Enable **Web Access** and **OML**

    ![DB Actions - Info DEMO User](./images/task4/demo-user-info.png)

4. Change to **Granted Roles** tab. Search by SODA_APP and select **Granted** and **Default**. After click **Create User**.

    ![DB Actions - Info DEMO Granted Roles](./images/task4/granted-roles.png)

    Be sure that the user has been created correctly, exactly like the screenshoot.
    
    ![DB Actions - DEMO User Ready](./images/task4/demo-user-ready.png)

5. Go back to the main menu ≡, then Oracle Database > **Autonomous JSON Database**. 

    ![AJD Dashboard](./images/task4/ajson-dashboard.png)

6. On **Tools tab**, under **Oracle Application Express**, click **Open APEX**. 

    ![Apex](./images/task4/apex.png)

7. On **Administration Services** login page, use password for **ADMIN**.

    - Password: **DBlearnPTS#22_**
    ```
    <copy>DBlearnPTS#22_</copy>
    ```

    ![Apex ADMIN](./images/task4/apex-admin.png)

8. Click **Create Workspace**.

    ![Apex Workspace](./images/task4/apex-workspace.png)

9. In the **How would you like to create your workspace?** screen, select **Existing Schema**.

    ![Apex Workspace](./images/task4/create-workspace.png)

10. Type the following information:

    - Database User: **DEMO**. Use the search menu to find DEMO and select it. You can't type on this field.
    - Workspace User: **DEMO**
    ```
    <copy>DEMO</copy>
    ```
    - Workspace Username: **DEMOWS**
    ```
    <copy>DEMOWS</copy>
    ```
    - Workspace Password: **DBlearnPTS#22_**
    ```
    <copy>DBlearnPTS#22_</copy>
    ```
    - Click **Create Workspace**
    
    ![Apex Workspace DEMO](./images/task4/create-workspace-info.png)
    
11. Click **DEMO** in the middle of the page to **Sign in** as **DEMO** user.
 
    ![Apex Login DEMO](./images/task4/apex-log-in-demo.png)
 
12. Click **Sign In** Page using the following information:

    - Workspace: **demo**
    ```
    <copy>demo</copy>
    ```
    - Username: **demows**
    ```
    <copy>demows</copy>
    ```
    - Password: **DBlearnPTS#22_**
    ```
    <copy>DBlearnPTS#22_</copy>
    ```

    ![Login DEMO](./images/task4/log-in-demo-new.png)

    **Oracle APEX** uses low-code development to let you build data-driven apps quickly without having to learn complex web technologies. This also gives you access to Oracle REST Data Services, that allows developers to readily expose and/or consume RESTful Web Services by defining REST end points.

13. Go again to **Database Actions** section if yout browser tab has being closed.

    ![DB Actions](./images/task4/db-actions.png)

14. **Sign out** as **ADMIN**.

    ![DB Actions ADMIN sign out](./images/task4/sign-out-admin.png)

15. **Sign in** as **DEMO** user.
    
    ![DB Actions sign in](./images/task4/database-actions-sign-in.png)

    - Username: **demo**
    ```
    <copy>demo</copy>
    ```
    - Password: **DBlearnPTS#22_**
    ```
    <copy>DBlearnPTS#22_</copy>
    ```

    ![DB Actions DEMO sign in](./images/task4/sign-in-demo.png)

    You should be connected now as **DEMO** user, check it on the right top corner side of the page.

    ![DB Actions DEMO](./images/task4/database-actions-demo.png)

    
16. Click **Development** > **JSON**, and follow the tips. This is the interface you will use to manage your JSON collections in this document store.

    ![DB Actions JSON](./images/task4/db-actions-json.png)

17. We will create a Collection to store JSON documents. Click **Create Collection**.

    ![DB Actions JSON Create Collection](./images/task4/create-collection.png)

18. Provide the following information:

    - Collection Name: **SimpleCollection**
    ```
    <copy>SimpleCollection</copy>
    ```
    - Click **Create** 
    
    ![DB Actions JSON Create Collection](./images/task4/create-simple-collection.png)

    You can see the new Collection under the JSON Collection section page.
    
    ![DB Actions JSON Create Collection](./images/task4/simple-collection.png)


*You can proceed to the next lab…*

## Acknowledgements
* **Author** - Valentin Leonard Tabacaru, Database Product Management and Priscila Iruela, Technology Product Strategy Director
* **Contributors** - Victor Martin Alvarez, Technology Product Strategy Director
* **Last Updated By/Date** - Priscila Iruela, July 2022

## Need Help?
Please submit feedback or ask for help using our [LiveLabs Support Forum](https://community.oracle.com/tech/developers/categories/livelabsdiscussions). Please click the **Log In** button and login using your Oracle Account. Click the **Ask A Question** button to the left to start a *New Discussion* or *Ask a Question*.  Please include your workshop name and lab name.  You can also include screenshots and attach files.  Engage directly with the author of the workshop.

If you do not have an Oracle Account, click [here](https://profile.oracle.com/myprofile/account/create-account.jspx) to create one.