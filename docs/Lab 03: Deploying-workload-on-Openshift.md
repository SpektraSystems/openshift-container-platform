## Lab 03: Deploying workload on OpenShift

* [Exercise 01: Deploy a 2 Tier Node JS Application on OpenShift](#exercise-01-deploy-a-2-tier-node-js-application-on-openshift)
* [Exercise 02: Installing OpenShift CLI](#exercise-02-installing-openshift-cli)
* [Exercise 03: Deployment in OpenShift using CLI](#exercise-03-deployment-in-openshift-using-cli)
* [Exercise 04: Create an App using Docker build](#exercise-04-create-an-app-using-docker-build)

### Lab overview
In this lab, we will deploy a workload on OpenShift.

### Prerequisites
- Lab 02 must be completed

### Time Estimate
45 minutes

### Exercise 01: Deploy a 2 Tier Node JS Application on OpenShift
In this exercise, you will deploy a 2 tier Node.js app on OpenShift and configure it to use the DB on Azure.

1.	**Launch** a browser and **Navigate** to https://portal.azure.com. **Login** with the Microsoft Azure credentials you received via email.
<img src="../images/43az_dashboard.jpg"/>

2.	Click on **+New** on the left side of the Dashboard.
<img src="../images/71new.jpg"/> 

3.	In the **New** blade that come up, Select **Databases**. 
<img src="../images/72newdatabase.jpg"/> 

4.	In the **Databases** blade appears. Select **Azure Cosmos DB**
<img src="../images/73cosmosdb.jpg"/> 

5.	In the **create** blade that come up, **configure** the **settings** as follows:

-	ID  :  **uniquename** (This name should be unique across Azure.)
-	API :  **MongoDB**
-	Subscription : Select the **existing** subscription
-	Resource Group : Choose **existing** and scroll down to see the Resource Group which is already there and select that)
-	Location: **(Select Resource Group Region)**

<img src="../images/74cosmosdb_create.jpg"/> 

And then Click on **Create**.

6.	You can see the **status** of the **deployment** from the **notifications** icon on top of the page.
<img src="../images/75notification.jpg"/> 

7.	Once the deployment is **successful**, click on **Go to resource** from the notifications tab.
<img src="../images/76dep_status.jpg"/>

8.	Now you will be directed to the deployed database.
<img src="../images/77database.jpg"/> 

9.	Now, click on **Connection String** under settings menu on the left side of the blade.
<img src="../images/78conn_string.jpg"/> 

10.	Now from the new blade that come up, **copy** the connection string for later use.
<img src="../images/79copy_connstring.jpg"/>  

11.	Now, open a new tab in a broswer and **navigate** to the **OpenShift console url**. **Login** into the OpenShift console using the **credentials** you **received via email** by Selecting AzureAD as authentication type.
<img src="../images/80openshift_console.jpg"/>  

### Exercise 02: Installing OpenShift CLI
#### COMMAND LINE INTERFACE
OpenShift ships with a feature rich web console as well as command line tools to provide users with a nice interface to work with applications deployed to the platform. The OpenShift tools are a single executable written in the Go programming language and is available for the following operating systems: 
-	Microsoft Windows 
-	Apple OS X
-	Linux

#### Installing the CLI
The easiest way to **download** the **CLI** is by accessing the **Command line tools** page on the web console.
1.	Click on down arrow key as shown in below screenshot and click on **Command Line Tools**.
<img src="../images/81cl_tools.jpg"/>  

2.	On **Command Line Tools** page, click on **Latest Release**.
<img src="../images/82cl_latestrelease.jpg"/>  

3.	Now, you need to **login** in to your **red hat account**(one which has license for OpenShift)
<img src="../images/83redhatlogin.jpg"/>

4.	Scroll down and click on **download**.

5.	Once the file has been downloaded, you will need to **extract** the **contents** of the same at below directories.</br>
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

7.	Now run below command on shell/command prompt to check the **version** of OpenShift client an to verify that it is successfully configured.
<img src="../images/84check_version.jpg"/>  

### Exercise 03: Deployment in OpenShift using CLI
In this exercise, you will learn how to create a new project on OpenShift and how to create an application from an existing docker image.
1.	Launch the command line and run below command and enter **username** and **password** as you have received in your lab mail.
```
oc login <URL of OpenShift:8443>
```

2.	Create an **OpenShift project** by running below command. 
<img src="../images/85openshift_cmnd.jpg"/> 

3.	Now you can see the **project** is created successfully.
```
oc get projects
```

4.	You can also check the **status** of the **project** by running the following command.
```
oc status
``` 
<img src="../images/86openshift_cmnd.jpg"/> 

5.	Create new **application** using below command 
```
oc new-app redhatworkshops/welcome-php --name=welcome
``` 
<img src="../images/87openshift_cmnd.jpg"/> 

6.	The above command uses the **docker image** to deploy a docker container in a pod. you will notice that a deployed pod runs and it starts an application pod as shown below.
```
oc get pods
``` 
<img src="../images/88openshift_cmnd.jpg"/> 

7.	To view the list of **services** in the project, run the following command 
```
oc get services
``` 
<img src="../images/89openshift_cmnd.jpg"/> 

8.	Now add a route to the service with the following command.
```
oc expose service welcome --name=welcomehost 
``` 
<img src="../images/90openshift_cmnd.jpg"/> 

9.	Now go to your **OpenShift platform** and click on applications>hostname, you can access the **application** from the browser and see the result.
<img src="../images/91browser.jpg"/> 

10.	To view all the **components** that were created in your **project**, run the command given below.
```
oc get all
``` 
<img src="../images/92vew_allproject.jpg"/> 

11.	Now you can **delete** all these **components** by running one command.
```
oc get all --all
```
<img src="../images/93delete.jpg"/> 

### Exercise 04: Create an App using Docker build
In this exercise, you will learn how to create an **application** from a Dockerfile. OpenShift takes Dockerfile as an input and generates your application docker image for you.

1.	You can create a new **project** or use **existing project** that created in exercise 3. To make sure you have the existing project run the following command.
<img src="../images/94openshift_cmnd.jpg"/>

2.	Now, we are using the Dockerfile as the basis to create a **docker image** for application. Run the command is given below.
```
oc new-app https://github.com/RedHatWorkshops/time --context-dir=rhel
```
<img src="../images/95openshift_cmnd.jpg"/>

3.	Now, look at the **buildconfig** by running the command shown below.
 ```
 oc get bc time -o json
 ```
<img src="../images/96buildconfig.jpg"/>

4.	To view the list of **build**, run command given below.
```
oc get builds
``` 
<img src="../images/97list_build.jpg"/>

5.	Run the command as shown below to look at the **build logs**.
```
oc  logs build/time-1 
```
<img src="../images/98build_logs.jpg"/>

6.	Now, we will do **deployment configuration** by running the following command.
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

7.	Now, you can get the **list of pods**, Run the following command given below.
```
oc get pods
```
<img src="../images/99list_pods.jpg"/> 

8.	Now, add a **route** to expose that service, Run the following command given below.
```
oc get services
``` 
<img src="../images/100expose_services.jpg"/> 

9.	Now, we **expose** the service as a route.
```
oc expose service time
```
<img src="../images/101expose_service_route.jpg"/> 

10.	Now, we check whether the **route** is exposed.
```
oc get routes
```
<img src="../images/102check_route.jpg"/> 

11.	To run the **application**, copy the host/port and paste in browser and you can see the result.
<img src="../images/103browser_result.jpg"/> 

[<Previous](/docs/Lab%2002:%20Deploying-OpenShift-cluster-using-ARM-templates.md) /
[Next>](/docs/Lab%2004:%20Integration-of-ACR-%20with-OpenShift.md)
