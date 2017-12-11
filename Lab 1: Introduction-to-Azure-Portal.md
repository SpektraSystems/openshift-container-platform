2. [Lab 1: Introduction to Azure Portal](#lab-1-introduction-to-azure-portal)	
   1. [Exercise 1: Log into your Azure Portal](#exercise-1-log-into-your-azure-portal)
   2. [Exercise 2: Verify access to the Subscription](#exercise-2-verify-access-to-the-subscription)
   
### Lab Overview
This lab will take you through Azure login and portal experience.

### Prerequisites
-	Windows or a Mac machine with HTML5 supported browser such as Microsoft Edge, Internet Explorer, Chrome or Firefox
-	You should have registered in the training portal https://azuretraining.spektrasystems.com and received the confirmation message with the credentials to login to the [Azure portal](http://portal.azure.com).
-	Red Hat Customer Portal login credentials so that the Azure instances can be registered with Red Hat Subscription Manager properly, and you must have enough OpenShift Container Platform entitlements to cover the chosen configuration.

![Registration Notification](https://github.com/ShivaniThadiyan/openshift-container-platform/blob/master/images/2registration_notification.jpg)


### Time Estimate

10 minutes

### Exercise 1: Log into your Azure Portal

In this exercise, you will log into the Azure Portal using your Azure credentials.
1.	**Launch** a browser and **Navigate** to https://portal.azure.com. Provide the credentials that you received via email. Click on **Sign In**.

![Azure Login](https://github.com/ShivaniThadiyan/openshift-container-platform/blob/master/images/3azure_login.jpg)

```
Note : At the first login, you may have to change the password, if asked for.
```

2.	**Enter** a new **password**. Then select **Update password and sign in**.

![Azure Login](https://github.com/ShivaniThadiyan/openshift-container-platform/blob/master/images/4update_password.jpg)

3.	Now, you will be directed to the Azure Dashboard

![Azure Dashboard](https://github.com/ShivaniThadiyan/openshift-container-platform/blob/master/images/5azure_dashboard.jpg)

### Exercise 2: Verify access to the Subscription
In this exercise, you will verify the type of role you are assigned in this Subscription.

1.	**Launch** a browser and **Navigate** to https://portal.azure.com. **Login** with the Microsoft Azure credentials you received via email.

![Azure Dashboard](https://github.com/ShivaniThadiyan/openshift-container-platform/blob/master/images/6azure_dashboard.jpg)

2. **Click** on **Microsoft Azure**  at the top left corner of the screen, to view the Dashboard.

![Microsoft Azure](https://github.com/ShivaniThadiyan/openshift-container-platform/blob/master/images/7microsoftazure.jpg)

3.	To toggle **show/hide** the Portal menu options with icon, **Click** on the **Show Menu** button. 

![Azure Menu](https://github.com/ShivaniThadiyan/openshift-container-platform/blob/master/images/8azure_menu.jpg)

4.	**Click** on the **Resource groups** button in the **Menu navigation** bar to view the **Resource groups** blade.

"![Resource Group](https://github.com/ShivaniThadiyan/openshift-container-platform/blob/master/images/9resourcegroup.jpg)

5.	You will see a Resource Group which you have access to, **click** on it.

![Select RG](https://github.com/ShivaniThadiyan/openshift-container-platform/blob/master/images/10select_rg.jpg)

```
Note:
The Resource Group shown here is for demo purpose only. Actual name of the Resouce Group that you see may differ.
```

6.	From the Resource Group blade that come up, **Select** the Access Control ( IAM ) which is on the left side of the blade.

![Access Control](https://github.com/ShivaniThadiyan/openshift-container-platform/blob/master/images/11access_control.jpg)

7.	In the new blade that come up, you can see the **role** that is assigned to you.

![Role](https://github.com/ShivaniThadiyan/openshift-container-platform/blob/master/https://github.com/ShivaniThadiyan/openshift-container-platform/blob/master/images/12role.jpg)
