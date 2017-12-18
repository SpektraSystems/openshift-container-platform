## Lab 04: Integration of ACR with OpenShift

### Lab overview
In this lab, we will integrate ACR with OpenShift.

### Prerequisites
- Lab 03 must be completed

### Time Estimate
45 minutes

### Exercise 01: Integrate ACR with OpenShift 
In this exercise, you will deploy an Azure Container Registry and integrate it with OpenShift. 

1.	**Launch** a browser and **Navigate** to [Azure Portal](https://portal.azure.com). **Login** with the Microsoft Azure credentials you received via email. 
2.	Click on **+New** on the left side of the Dashboard.
<img src="../images/104az_new.jpg"/> 

3.	In the **New** blade that come up, Select **Containers**. 
<img src="../images/105az_containers.jpg"/> 
 
4.	In the **Containers** blade appears. Select **Azure Container Registry**.
<img src="../images/106acr.jpg"/> 

5.	In the **create** blade that come up, **configure** the settings as follows:

-	Registry name  :  **uniquename** (This name should be unique across Azure.)
-	Subscription : Select the **existing** subscription
-	Resource Group : Choose Use existing and scroll down to see the Resource Group which is already there and select that)
-	Location: **South Central US**
-	Admin user: Select **Enable**
-	SKU: **Standard**

<img src="../images/107create_acr.jpg"/>  

And then **Click** on **Create**.

6.	You can see the status of the **deployment** from the **notifications** tab on top of the page.
<img src="../images/108notification.jpg"/> 

7.	Once the deployment is **successful**, click on Go to resource from the notifications tab.
<img src="../images/109dep_status.jpg"/> 

8.	Now you will be directed to the deployed container registry. Click on the **Access keys** under Settings section which is on the left side of the blade.
<img src="../images/110acr_accesskey.jpg"/> 

9.	Now you will be directed to the **Access keys** blade.
Copy the **Registry name**, **Login server**, **Username** and **password** to a text editor for later use.
<img src="../images/111acr_copy.jpg"/> 

10.	Now, open a new tab in a browser and navigate to the OpenShift console url. **Login** into the **OpenShift console** using the credentials you received via email by Selecting AzureAD as authentication type.
<img src="../images/112openshift_console.jpg"/> 

11.	Now you will be redirected to the **My Projects** page, where you will select the Project you created earlier.
<img src="../images/113myproject_page.jpg"/> 

12.	Once you are in the **project page**, click on **Applications** from the left side of the menu and in the new menu that comes up, click on **Pods**.
<img src="../images/114project_page.jpg"/> 

13.	Now you will be redirected to the **Pods page**. Click on the pod with status as running.
<img src="../images/115pods_page.jpg"/> 

14.	Now you will be directed to the **Details blade** of the selected Pod.
<img src="../images/116details_page.jpg"/> 

15.	From the details blade, copy the **Private Ip** of the node in which the Pod is running.
<img src="../images/117copy_details.jpg"/> 

16.	Now, **Open** a new tab in a browser and **Navigate** to https://portal.azure.com. **Login** with the Microsoft Azure credentials you received via email.
<img src="../images/43az_dashboard.jpg"/> 

17.	**Click** on **Cloud Shell**  at the top right corner of the screen, to open the cloud shell.
<img src="../images/119bash.jpg"/> 

18.	Now the **bash shell** will open up.
<img src="../images/120bashshell.jpg"/> 

19.	Now **execute** the following command. When promted, type **Yes** and you will be logged in to the OpenShift Master VM.
```
ssh ocpadmin@<copiedDNSNameofBastionVM>
```
```
Note: Substitute in the above command with the value of copied DNS Name of Bastion VM 
```
<img src="../images/121openshift_cmnd.jpg"/> 

20.	Now **execute** the following command in the cloud shell to pull a **docker image**. Copy the key into a text editor for later use.
```
ssh ocpadmin@<copiedPrivateIpOfNode>
```
```
Note: Substitute in the above command with the value of copied Private IP Address of Node in which pod is running.
``` 
<img src="../images/122openshift_cmnd.jpg"/> 

21.	Now **execute** the following command in the cloud shell to **login** in to root account. 
```
sudo su -
```
<img src="../images/123openshift_cmnd.jpg"/> 

22.	Now **execute** the following command in the cloud shell to check if the **docker** is **installed and running**. 
```
docker -v 
``` 
<img src="../images/124openshift_cmnd.jpg"/> 

23.	Now **execute** the following command in the cloud shell to **display** the list the **docker images** in the system. 
```
docker images
```
<img src="../images/125openshift_cmnd.jpg"/> 

24.	From the displayed results, **copy** the **Image name** with todoapp in the end.
<img src="../images/126openshift_cmnd.jpg"/> 

25.	Now **execute** the following command in the cloud shell to **tag** the existing docker image.
```
docker tag <ImageName> <ACRLoginServerUri>/sample/todoapp
```
```
Note: 	Substitute for ImageName and ACR Login Server URI with the copied values in the above command
```
<img src="../images/127openshift_cmnd.jpg"/> 

26.	Now **execute** the following command in the cloud shell to **login to docker registry**. When prompted, enter the **password** for ACR you copied earlier
```
docker login <acrServerLoginServerUri> -u <ACRUsername>
```
```
Note: Substitute for ACR Login Server URI and Username in the above command
```
<img src="../images/128openshift_cmnd.jpg"/> 

27.	Now **execute** the following command in the cloud shell to **push** the tagged **image** to azure container Registry. Copy the key into a text editor for later use.
```
docker push <ACRLoginServerUri>/sample/todoapp
```
```
Note: Substitute for ACRLoginServerUri in the above command
``` 
<img src="../images/129openshift_cmnd.jpg"/> 

28.	Once you have pushed the image to Azure Container Registry, click on **More services** on the left side of the menu on the dashboard.
<img src="../images/130az_moreservices.jpg"/> 

29.	In the new blade that come up, search in the Filter box at the top “Container registries” and then Select **Container Registries** from the search result.
<img src="../images/131search_acr.jpg"/> 

30.	On the blade, select the **Container Registry** which you have created.
<img src="../images/132select_acr.jpg"/> 

31.	Now you will be directed to the **Overview page** of the container registry.
<img src="../images/133overview_acr.jpg"/> 

32.	Now to check whether the image has been pushed to the repository, you can click on **Repositories** under Services on the menu on left side of the blade.
<img src="../images/134repositories.jpg"/> 

33.	In the next blade that come up, if the push has been **successful**, you can see sample/todapp repository there. 
<img src="../images/135repositoriesview.jpg"/> 

[<Previous](/docs/Lab%2003:%20Deploying-workload-on-Openshift.md) /
[Next>](/docs/Lab%2005:%20Additional-Labs.md)
