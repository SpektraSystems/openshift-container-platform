# AEC Open Shift | Lab Guide
## Contents
<!-- TOC -->

1. [Workshop Architecture and Objective](#workshop-architecture-and-objective)
   1. [Labs Objective](#labs-objective)	
   2. [Workshop Architecture after deploying ARM Template](#workshop-architecture-after-deploying-arm-template)	
2. [Lab 1: Introduction to Azure Portal](#lab-1-introduction-to-azure-portal)	
   1. [Exercise 1: Log into your Azure Portal](#exercise-1-log-into-your-azure-portal)
   2. [Exercise 2: Verify access to the Subscription](#exercise-2-verify-access-to-the-subscription)	
3. [Lab 2: Deploying Open Shift cluster using ARM templates](#lab-2-deploying-open-shift-cluster-using-arm-templates)
   1. [Exercise 1: Create an Azure AD Application](#exercise-1-create-an-azure-ad-application)
   2. [Exercise 2: Create a Keyvault](#exercise-2-create-a-keyvault)
   3. [Exercise 3: Deploy Openshift Cluster using ARM Template](#exercise-3-deploy-openshift-cluster-using-arm-template)
   4. [Exercise 4: Configure Azure AD Authentication](#exercise-4-configure-azure-ad-authentication)
4. [Lab 3: Deploying workload on Openshift](#lab-3-deploying-workload-on-openshift)
   1. [Exercise 1: Deploy a 2 Tier Node JS Application on Open Shift](#exercise-1-deploy-a-2-tier-node-js-application-on-open-shift)
   2. [Exercise 2: Installing OpenShift CLI](#exercise-2-installing-openshift-cli)
   3. [Exercise 3: Deployment in OpenShift using CLI](#exercise-3-deployment-in-openshift-using-cli)
   4. [Exercise 4: Create an App using Docker build](#exercise-4-create-an-app-using-docker-build)
5. [Lab 4: Integration of ACR with OpenShift](#lab-4-integration-of-acr-with-openshift)
   1. [Exercise 1: Integrate ACR with OpenShift](#exercise-1-integrate-acr-with-openshift)
<!-- /TOC -->

## Workshop Architecture and Objective
 As part of Hand on Lab, you will get following details via email. Make a note of these details as these shall be leveraged throughout the lab exercise
- Azure Access: Azure Username and Password (Default Password, Change at first login)

### Labs Objective
During this lab, you will deploy Open Shift cluster on Azure and integrate Azure AD Authentication and Azure Container Registry into Open Shift. Detailed steps to achieve this is as follows.
- Get Familiar with Azure Portal and Ansible Tower UI
-	Create an Azure AD Application for Authentication
-	Create a key vault to store SSH Key
-	Deploy Open Shift using ARM Template
-	Configure Azure AD Authentication
-	Deploy 2 Tier App on Open Shift
-	Integration of Azure Container Registry with Open Shift

### Workshop Architecture after deploying ARM Template
Following illustrates the architecture in your Azure deployment after completion of exercise’s part of workshop.
<img src="images/1workshop_arch.jpg"/>
## Lab 1: Introduction to Azure Portal
### Lab Overview
This lab will take you through Azure login and portal experience.

### Prerequisites
-	Windows or a Mac machine with HTML5 supported browser such as Microsoft Edge, Internet Explorer, Chrome or Firefox
-	You should have registered in the training portal https://azuretraining.spektrasystems.com and received the confirmation message with the credentials to login to the [Azure portal](http://portal.azure.com).
-	Red Hat Customer Portal login credentials so that the Azure instances can be registered with Red Hat Subscription Manager properly, and you must have enough OpenShift Container Platform entitlements to cover the chosen configuration.
<img src="images/2registration_notification.jpg"/>

### Time Estimate

10 minutes

### Exercise 1: Log into your Azure Portal

In this exercise, you will log into the Azure Portal using your Azure credentials.
1.	**Launch** a browser and **Navigate** to https://portal.azure.com. Provide the credentials that you received via email. Click on **Sign In**.
<img src="images/3azure_login.jpg"/>

```
Note : At the first login, you may have to change the password, if asked for.
```

2.	**Enter** a new **password**. Then select **Update password and sign in**.
<img src="images/4update_password.jpg"/>

3.	Now, you will be directed to the Azure Dashboard
<img src="images/5azure_dashboard.jpg"/>

### Exercise 2: Verify access to the Subscription
In this exercise, you will verify the type of role you are assigned in this Subscription.

1.	**Launch** a browser and **Navigate** to https://portal.azure.com. **Login** with the Microsoft Azure credentials you received via email.
<img src="images/6azure_dashboard.jpg"/>

2. **Click** on **Microsoft Azure**  at the top left corner of the screen, to view the Dashboard.
<img src="images/7microsoftazure.jpg"/>

3.	To toggle **show/hide** the Portal menu options with icon, **Click** on the **Show Menu** button. 
<img src="images/8azure_menu.jpg"/>

4.	**Click** on the **Resource groups** button in the **Menu navigation** bar to view the **Resource groups** blade.
<img src="images/9resourcegroup.jpg"/>

5.	You will see a Resource Group which you have access to, **click** on it.

<img src="images/10select_rg.jpg"/>

```
Note:
The Resource Group shown here is for demo purpose only. Actual name of the Resouce Group that you see may differ.
```

6.	From the Resource Group blade that come up, **Select** the Access Control ( IAM ) which is on the left side of the blade.
<img src="images/11access_control.jpg"/>

7.	In the new blade that come up, you can see the **role** that is assigned to you.
<img src="images/12role.jpg"/>

## Lab 2: Deploying Open Shift cluster using ARM templates
### Lab Overview
In this lab, you will learn how to deploy the Open Shift Cluster on Azure using ARM templates.
### Prerequisites
•	Lab 1 must be completed

### Time Estimate
120 minutes

### Exercise 1: Create an Azure AD Application
In this exercise, you will create an Azure AD App and retrieve the Client ID and Client secret values.
1.	**Launch** a browser and **Navigate** to https://portal.azure.com. **Login** with the Microsoft Azure credentials you received via email.
<img src="images/13azuredashboard.jpg"/>

2.	**Click** on the **Azure Active Directory** button in the **Menu navigation** bar to view the **Azure Active Directory** blade.
<img src="images/14selectazure_ad.jpg"/>

3.	You will be directed to the Azure Active Directory blade, **click** on **App registrations**.
<img src="images/15app_reg.jpg"/>

4.	In the next blade, **click** on **New Application Registration** on top of the blade.
<img src="images/16new_appreg.jpg"/>

5.	In the **Create** blade, **configure** as follows:

-	Name: **(Provide a unique value)**
-	Application type: **Web app/API**
-	Sign-on URL: https://contoso.com

```
Note: We will change this value later during the lab.
```

And then **click** on **Create**.

<img src="images/17createapp.jpg"/>

6.	You will be redirected to the **App registrations** blade. You can check the app has been created by typing the App Name in the search field.
<img src="images/18check_app.jpg"/>

If the app has been created, you can see it in the results as shown above.

7.	Click on the app you created and you will be directed to the App blade.

8.	Copy the Application Id and save it in a notepad or any text editor for later use.
<img src="images/19app_id.jpg"/>

9.	Now, **Click** on **Keys** in the settings blade.
<img src="images/20app_key.jpg"/>

10.	In the **Keys** blade, **configure** as follows:

- Description: **key1**
- Expires: **Never expires**

And **Click** on **Save.**

<img src="images/21save_key.jpg"/>

11.	After you click on save, the key value will be displayed which is the Client Secret.
Copy the value into the text editor where you saved the value of Application Id for later use.
<img src="images/22copy_key.jpg"/>

### Exercise 2: Create a Keyvault 
In this exercise, you will configure Azure Bash Cloud Shell and create a Key vault in the existing resource group and store the SSH key inside the vault. 
1.	**Launch** a browser and **Navigate** to https://portal.azure.com. **Login** with the Microsoft Azure credentials you received via email.
<img src="images/23azure_dashboard.jpg"/>

2.	**Click** on **Cloud Shell**  at the top right corner of the screen, to open the cloud shell.
<img src="images/24cloudshell.jpg"/>

3.	Then **Click** on **Bash ( Linux )**, and in the next page, **click** on **Show advanced settings**
<img src="images/25selectbash.jpg"/>
<img src="images/26advanced_settings.jpg"/>

4.	In the new blade, select the existing resource group, provide unique names under Create new(Storage account and File share) and **click** on **Create Storage**.
<img src="images/27create_storage.jpg"/>

5.	In a few minutes, the bash shell will come up.
<img src="images/28bashshell.jpg"/>

6.	Now execute the following command in the cloud shell to create a key vault in the existing resource group.
```
az keyvault create -n <uniquename> -g <ResourceGroup> -l <LocationOfResourceGroup> 
--enabled-for-template-deployment true
```
```
Note:
Provide the existing Resource Group name, it’s location and a unique name for key vault in the above command when executing
```
<img src="images/29create_keyvault_bash.jpg"/>

7.	Now execute the following command in the cloud shell to generate ssh key.
```
ssh-keygen
```
```
Note: Keep on clicking enter button until the key has been created.
```
<img src="images/30generate _ssh.jpg"/>

8.	Now execute the following command in the cloud shell to display the public ssh key. Copy the key into a text editor for later use.
```
cat .ssh/id_rsa.pub
```
```
Note: 
The copied SSH Key should be made into a single line. You will need this key for later use.
```
<img src="images/31display_publickey.jpg"/>

9.	Now execute the following command to store the generated key in the key vault.
```
az keyvault secret set --vault-name <keyvaultname> -n osdemovaultsecret --file ~/.ssh/id_rsa
```
```
Note:
Substitute for key vault name in the above command with the name of the keyvault created earlier when executing.
```
<img src="images/32store_key.jpg"/>

## Exercise 3: Deploy Openshift Cluster using ARM Template  
In this exercise, you will deploy the Openshift cluster on Azure using ARM Template . 
1.	**Launch** a browser and **Navigate** to https://github.com/SpektraSystems/openshift-container-platform

2.	Now **click** on **Deploy to Azure** button and you will be redirected to the azure portal. If prompted **login** with the Microsoft Azure credentials you received via email.
<img src="images/33deploytemplate.jpg"/>

3.	Now you will be directed to the Custom Deployment blade.
<img src="images/34custom_deployment.jpg"/>

4.	In the **Custom Deployment** blade, **configure** the settings as follows:
-	Resource Group : Choose Use **existing** and scroll down to see the Resource Group which is already there)
-	Openshift Password  :  **Provide a unique password**
-	Ssh Public Key :  **Provide the copied SSH key**
-	Rhsm Username or Org Id: **Provide the username of Redhat credentials**
-	Rhsm Password or Org Id: **Provide the password of Redhat credentials**
-	Rhsm Pool Id: **Provide the pool Id of Redhat OpenShift Subscription**
-	Key Vault Name : **Provide the Key Vault name you provided**
-	Key Vault Secret : **osdemovaultsecret**
-	Aad Auth App Name : **Provide the name of the AD App you created** 
-	Aad Auth App Id : **Provide the Client ID of the AD App you created** 
-	Aad Auth Client Secret : **Provide the secret key of the AD App**
**And** accept the terms of conditions.
<img src="images/35customdeployment_configure.jpg"/>
<img src="images/36custeomdeployment_purchase.jpg"/>

5.	And then **click** on **Purchase**.

6.	Once the deployment starts, you can see the progress in the notification bar at the top of the Azure portal.
<img src="images/37deployment_progress.jpg"/>

7.	Once the deployment is complete, you can see it in the notifications tab as Deployment succeeded. Now, **click** on **Go to resource group** from the notifications tab.
<img src="images/38dep_succeed.jpg"/>

8.	In the resource group blade that come up, you can see the deployments as Succeeded, click on that.
<img src="images/39resource_group.jpg"/>

9.	Select **Microsoft Template** from the new blade that come up.
<img src="images/40dep_status.jpg"/>

10.	From the new blade that come up, you can see the outputs of the deployment.
<img src="images/41dep_output.jpg"/>

11.	Copy the Openshift Console URL, Bastion DNS FQDN and OpenShift Master SSH by clicking on Copy to a text editor

12.	To verify that the deployment is working, **Open** a new tab in the browser and paste the copied URL.
```
Note: Skip the certificate warning
```

13.	Now you will be directed to the Openshift Console Login page.
<img src="images/42openshift_console.jpg"/>

```
Note: If the above page comes up, then the deployment is working.
```

## Exercise 4: Configure Azure AD Authentication
In this exercise, you will configure the AD App you created for Authentication into the Open Shift console.
1.	**Launch** a browser and **Navigate** to https://portal.azure.com. **Login** with the Microsoft Azure credentials you received via email.
<img src="images/43az_dashboard.jpg"/>

2.	**Click** on the **Azure Active Directory** button in the **Menu navigation** bar to view the **Azure Active Directory** blade.
<img src="images/44az_ad.jpg"/>

3.	You will be directed to the Azure Active Directory blade, **click** on **App registrations**.
<img src="images/45app_reg.jpg"/>

4.	You will be redirected to the **App registrations** blade. You can search the App by typing the name of the App you created earlier, in the search field.
<img src="images/46select_app.jpg"/>

5.	Click on the app you created and you will be directed to the App blade.
<img src="images/47app_blade.jpg"/>

6.	Now Click on Properties under Settings blade.
<img src="images/48app_properties.jpg"/>

7.	In the **Properties** blade, **edit** as follows:
-	App ID URI: (Provide the Open Shift Console URI)
-	Home Page URL type: (Provide the Open Shift Console URI)
And then **click** on **Save**.
<img src="images/49save_properties.jpg"/>

8.	Once you save the properties, close the properties blade.
<img src="images/50close_properties.jpg"/>

9.	Then you will be redirected to the Settings Blade of AD App. Click on the Reply URLs
<img src="images/51reply_url.jpg"/>

10.	Now modify the openshift console url by removing the ‘console’ from the end and appending ‘oauth2callback/AzureAD’ to the url and provide it in the Reply URL blade that come up and then Click on Save. 
<img src="images/52replyurl_save.jpg"/>

11.	Now go back to the setting blade of the App and Click on Required permissions.
<img src="images/53req_permission.jpg"/>

12.	Click on Grant Permissions in the blade that come up and then **Click** on **Yes**
<img src="images/54grant_permission.jpg"/>
<img src="images/55grantpermission_yes.jpg"/>


13.	Now to verify that the user is able to authenticate to Openshift console via Azure AD, **Open** a new tab in the browser and paste the Openshift Console URL which you copied earlier.
```
Note: Skip the certificate warning
```
<img src="images/67openshift_console.jpg"/>

14.	Now click on AzureAD, you will be redirected to the Login Page. Provide the Azure credentials you received via email over there and click on Sign in.
<img src="images/68sign_in.jpg"/>

15.	Once the login is successful, you will be redirected to the Openshift console.
<img src="images/69openshift_cp.jpg"/>
 
 
## Lab 3: Deploying workload on Openshift
### Lab overview
In this lab, we will deploy a workload on OpenShift.

### Prerequisites
- Lab 1 must be completed

### Time Estimate
45 minutes

### Exercise 1: Deploy a 2 Tier Node JS Application on Open Shift
In this exercise, you will deploy a 2 tier Node.js app on Open Shift and configure it to use the DB on Azure.

1.	**Launch** a browser and **Navigate** to https://portal.azure.com. **Login** with the Microsoft Azure credentials you received via email.
<img src="images/70az_dashboard.jpg"/>

2.	Click on +New on the left side of the Dashboard.
<img src="images/71new.jpg"/> 

3.	In the **New** blade that come up, Select **Databases**. 
<img src="images/72newdatabase.jpg"/> 

4.	In the **Databases** blade appears. Select **Azure Cosmos DB**
<img src="images/73cosmosdb.jpg"/> 

5.	In the create blade that come up, configure the settings as follows:

-	ID  :  **uniquename** (This name should be unique across Azure.)
-	API :  **MongoDB**
-	Subscription : Select the **existing** subscription
-	Resource Group : Choose Use existing and scroll down to see the Resource Group which is already there and select that)
-	Location: **South Central US**

<img src="images/74cosmosdb_create.jpg"/> 

And then Click on Create.

6.	You can see the status of the deployment from the notifications tab on top of the page.
<img src="images/75notification.jpg"/> 

7.	Once the deployment is successful, click on Go to resource from the notifications tab.
<img src="images/76dep_status.jpg"/>

8.	Now you will be directed to the deployed database.
<img src="images/77database.jpg"/> 

9.	Now, click on Connection String under settings menu on the left side of the blade.
<img src="images/78conn_string.jpg"/> 

10.	Now from the new blade that come up, copy the connection string for later use.
<img src="images/79copy_connstring.jpg"/>  

11.	Now, open a new tab in a broswer and navigate to the Openshift console url. Login into the Openshift console using the credentials you received via email by Selecting AzureAD as authentication type.
<img src="images/80openshift_console.jpg"/>  

### Exercise 2: Installing OpenShift CLI
#### COMMAND LINE INTERFACE
OpenShift ships with a feature rich web console as well as command line tools to provide users with a nice interface to work with applications deployed to the platform. The OpenShift tools are a single executable written in the Go programming language and is available for the following operating systems: 
-	Microsoft Windows 
-	Apple OS X
-	Linux

#### Installing the CLI
The easiest way to download the CLI is by accessing the **Command line tools** page on the web console.
1.	Click on down arrow key as shown in below screenshot and click on **Command Line Tools**.
<img src="images/81cl_tools.jpg"/>  

2.	On **Command Line Tools** page, click on **Latest Release**.
<img src="images/82cl_latestrelease.jpg"/>  

3.	Now, you need to login in to your red hat account(one which has license for Openshift)
<img src="images/83redhatlogin.jpg"/>

4.	scroll down and click on download.

5.	Once the file has been downloaded, you will need to extract the contents of the same at below directories.</br>
Windows: 	**C:\OpenShift**</br>
OS X: 		**~/OpenShift**</br>
Linux: 		**~/OpenShift**</br>

**Windows** : To extract a zip archive on windows, you will need a zip utility installed on your system. With newer versions of windows (greater than XP), this is provided by the operating system. Just right click on the downloaded file using file explorer and select to extract the contents to

**OS X** : Open a terminal window and change to the directory where you downloaded the file. Once you are in the directory, enter in the following command: 
```
$ tar zxvf <File_Name>
```

**Linux** : Open a terminal window and change to the directory where you downloaded the file. Once you are in the directory, enter in the following command: 
```
$ tar zxvf <File_Name>
```

6.	Now you will need to add **oc** to your system’s environment variable path:</br>
**Windows** : Open Command prompt and run below command:
```
set PATH=%PATH%;C:\OpenShift
```

**OS X**: Open shell and run below command.
```
$ export PATH=$PATH:~/OpenShift 
```

**Linux** : Open shell and run below command.
```
$ export PATH=$PATH:~/OpenShift
```

7.	Now run below command on shell/command prompt to check the version of OpenShift client an to verify that it is successfully configured.
<img src="images/84check_version.jpg"/>  

### Exercise 3: Deployment in OpenShift using CLI
In this exercise, you will learn how to create a new project on OpenShift and how to create an application from an existing docker image.
1.	Launch the command line and run below command and enter username and password as you have received in your lab mail.
```
oc login <URL of Openshift:8443>
```

2.	Create an OpenShift project by running below command. 
<img src="images/85openshift_cmnd.jpg"/> 

3.	Now you can see the project is created successfully.
```
oc get projects
```

4.	You can also check the status of the project by running the following command.
```
oc status
``` 
<img src="images/86openshift_cmnd.jpg"/> 

5.	Create new application using below command 
```
oc new-app redhatworkshops/welcome-php --name=welcome
``` 
<img src="images/87openshift_cmnd.jpg"/> 

6.	The above command uses the docker image to deploy a docker container in a pod. you will notice that a deployed pod runs and it starts an application pod as shown below.
```
oc get pods
``` 
<img src="images/88openshift_cmnd.jpg"/> 

7.	To view the list of services in the project, run the following command below
```
oc get services
``` 
<img src="images/89openshift_cmnd.jpg"/> 

8.	Now add a route to the service with the following command.
```
oc expose service welcome --name=welcomehost 
``` 
<img src="images/90openshift_cmnd.jpg"/> 

9.	Now go to your openshift platform and click on applications>hostname, you can access the application from the browser and see the result.
<img src="images/91browser.jpg"/> 

10.	To view all the components that were created in your project, run he command is given below.
```
oc get all
``` 
<img src="images/92vew_allproject.jpg"/> 

11.	Now you can delete all these components by running one command.
```
oc get all --all
```
<img src="images/93delete.jpg"/> 

### Exercise 4: Create an App using Docker build
In this exercise, you will learn how to create an application from a Dockerfile. OpenShift takes Dockerfile as an input and generates your application docker image for you.

1.	You can create a new project or use existing project that created in exercise 3. To make sure you have the existing project run the following command.
<img src="images/94openshift_cmnd.jpg"/>

2.	Now, we are using the Dockerfile as the basis to create a docker image for application. Run the command is given below.
```
oc new-app https://github.com/RedHatWorkshops/time --context-dir=rhel
```
<img src="images/95openshift_cmnd.jpg"/>

3.	Now, look at the buildconfig by running the command shown below.
 ```
 oc get bc time -o json
 ```
<img src="images/96buildconfig.jpg"/>

4.	To view the list of build, run command is given below.
```
oc get builds
``` 
<img src="images/97list_build.jpg"/>

5.	Run the command as shown below to look at the build logs.
```
oc  logs build/time-1 
```
<img src="images/98build_logs.jpg"/>

6.	Now we will deployment configuration by running the following command.
```
oc get dc -o json
{
    "apiVersion": "v1",
    "items": [
        {
            "apiVersion": "v1",
            "kind": "DeploymentConfig",
            "metadata": {
                "annotations": {
                    "openshift.io/generated-by": "OpenShiftNewApp"
                },
…………
…………
…………
                "creationTimestamp": "2017-11-10T11:22:28Z",
                "generation": 3,
                "labels": {
    "metadata": {},
    "resourceVersion": "",
    "selfLink": ""
}
```

7.	Now, you can get the list of pods, Run the following command is given below.
```
oc get pods
```
<img src="images/99list_pods.jpg"/> 

8.	Now, add a route to expose that service, Run the following command is given below.
```
oc get services
``` 
<img src="images/100expose_services.jpg"/> 

9.	Now, we expose the service as a route.
```
oc expose service time
```
<img src="images/101expose_service_route.jpg"/> 

10.	Now, we check the route is exposed.
```
oc get routes
```
<img src="images/102check_route.jpg"/> 

11.	For run the application, copy the host/port and paste in browser and you can see the result.
<img src="images/103browser_result.jpg"/> 


## Lab 4: Integration of ACR with OpenShift
### Exercise 1: Integrate ACR with OpenShift 
In this exercise, you will deploy an Azure Container Registry and integrate it with Open Shift. 

1.	**Launch** a browser and **Navigate** to https://portal.azure.com. **Login** with the Microsoft Azure credentials you received via email. 
2.	Click on **+New** on the left side of the Dashboard.
<img src="images/104az_new.jpg"/> 

3.	In the **New** blade that come up, Select **Containers**. 
<img src="images/105az_containers.jpg"/> 
 
4.	In the **Containers** blade appears. Select **Azure Container Registry**.
<img src="images/106acr.jpg"/> 

5.	In the **create** blade that come up, **configure** the settings as follows:

-	Registry name  :  **uniquename** (This name should be unique across Azure.)
-	Subscription : Select the **existing** subscription
-	Resource Group : Choose Use existing and scroll down to see the Resource Group which is already there and select that)
-	Location: **South Central US**
-	Admin user: Select **Enable**
-	SKU: **Standard**

<img src="images/107create_acr.jpg"/>  

And then **Click** on **Create**.

6.	You can see the status of the deployment from the notifications tab on top of the page.
<img src="images/108notification.jpg"/> 

7.	Once the deployment is successful, click on Go to resource from the notifications tab.
<img src="images/109dep_status.jpg"/> 

8.	Now you will be directed to the deployed container registry. Click on the Access keys under Settings section which is on the left side of the blade.
<img src="images/110acr_accesskey.jpg"/> 

9.	Now you will be directed to the Access keys blade.
Copy the Registry name, Login server, Username and password to a text editor for later use.
<img src="images/111acr_copy.jpg"/> 

10.	Now, open a new tab in a browser and navigate to the Openshift console url. Login into the Openshift console using the credentials you received via email by Selecting AzureAD as authentication type.
<img src="images/112openshift_console.jpg"/> 

11.	Now you will be redirected to the My Projects page, where you will select the Project you created earlier.
<img src="images/113myproject_page.jpg"/> 

12.	Once you are in the project page, click on Applications from the left side of the menu and in the new menu that comes up, click on Pods.
<img src="images/114project_page.jpg"/> 

13.	Now you will be redirected to the Pods page. Click on the pod with status as running.
<img src="images/115pods_page.jpg"/> 

14.	Now you will be directed to the Details blade of the selected Pod.
<img src="images/116details_page.jpg"/> 

15.	From the details blade, copy the Private Ip Address of the node in which the Pod is running.
<img src="images/117copy_details.jpg"/> 

16.	Now, **Open** a new tab in a browser and **Navigate** to https://portal.azure.com. **Login** with the Microsoft Azure credentials you received via email.
<img src="images/118az_dashboard.jpg"/> 

17.	**Click** on **Cloud Shell**  at the top right corner of the screen, to open the cloud shell.
<img src="images/119bash.jpg"/> 

18.	Now the bash shell will open up.
<img src="images/120bashshell.jpg"/> 

19.	Now execute the following command. When promted, type Yes and you will be logged in to the Openshift Master VM.
```
ssh ocpadmin@<copiedDNSNameofBastionVM>
```
```
Note: Substitute in the above command with the value of copied DNS Name of Bastion VM 
```
<img src="images/121openshift_cmnd.jpg"/> 

20.	Now execute the following command in the cloud shell to pull a docker image. Copy the key into a text editor for later use.
```
ssh ocpadmin@<copiedPrivateIpOfNode>
```
```
Note: Substitute in the above command with the value of copied Private IP Address of Node in which pod is running.
``` 
<img src="images/122openshift_cmnd.jpg"/> 

21.	Now execute the following command in the cloud shell to login in to root account. 
```
sudo su -
```
<img src="images/123openshift_cmnd.jpg"/> 

22.	Now execute the following command in the cloud shell to check if the docker is installed and running. 
```
docker -v 
``` 
<img src="images/124openshift_cmnd.jpg"/> 

23.	Now execute the following command in the cloud shell to display the list the docker images in the system. 
```
docker images
```
<img src="images/125openshift_cmnd.jpg"/> 

24.	From the displayed results, copy the Image name with todoapp in the end.
<img src="images/126openshift_cmnd.jpg"/> 

25.	Now execute the following command in the cloud shell to tag the existing docker image.
```
docker tag <ImageName> <ACRLoginServerUri>/sample/todoapp
```
```
Note: 	Substitute for ImageName and ACR Login Server URI with the copied values in the above command
```
<img src="images/127openshift_cmnd.jpg"/> 

26.	Now execute the following command in the cloud shell to login to docker registry. When prompted, enter the password for ACR you copied earlier
```
docker login <acrServerLoginServerUri> -u <ACRUsername>
```
```
Note: Substitute for ACR Login Server URI and Username in the above command
```
<img src="images/128openshift_cmnd.jpg"/> 

27.	Now execute the following command in the cloud shell to push the tagged image to azure container Registry. Copy the key into a text editor for later use.
```
docker push <ACRLoginServerUri>/sample/todoapp
```
```
Note: Substitute for ACRLoginServerUri in the above command
``` 
<img src="images/129openshift_cmnd.jpg"/> 

28.	Once you have pushed the image to Azure Container Registry, click on More services on the left side of the menu on the dashboard.
<img src="images/130az_moreservices.jpg"/> 

29.	In the new blade that come up, search in the Filter box at the top “Container registries” and then Select “Container Registries from 
the search result.
<img src="images/131search_acr.jpg"/> 

30.	On the blade, select the Container Registry which you have created.
<img src="images/132select_acr.jpg"/> 

31.	Now you will be directed to the Overview page of the container registry.
<img src="images/133overview_acr.jpg"/> 

32.	Now to check whether the image has been pushed to the repository, you can click on Repositories under Services on the menu on left side of the blade.
<img src="images/134repositories.jpg"/> 

33.	In the next blade that come up, if the push has been successful, you can see sample/todapp repository there. 
<img src="images/135repositoriesview.jpg"/> 




