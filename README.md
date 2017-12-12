# Open Shift Cluster Workshop | Lab Guide

***

## Contents
<!-- TOC -->

* [Workshop Architecture and Objective](#workshop-architecture-and-objective)
   * [Labs Objective](#labs-objective)	
   * [Workshop Architecture after deploying ARM Template](#workshop-architecture-after-deploying-arm-template)	
* [Lab 01: Introduction to Azure Portal](docs/Lab%2001:%20Introduction-to-Azure-Portal.md)	
   * [Option 1: Using Preconfigured Environment](docs//Lab%2001:%20Introduction-to-Azure-Portal.md#option-1-using-preconfigured-environment)
     * [Exercise 00: Sign Up for Pre-configured Environment](docs//Lab%2001:%20Introduction-to-Azure-Portal.md#exercise-00-sign-up-for-pre-configured-environment)     
     * [Exercise 01: Log into your Azure Portal and Verify access to the Subscription](docs//Lab%2001:%20Introduction-to-Azure-Portal.md#exercise-01-log-into-your-azure-portal-and-verify-access-to-the-subscription)
   * [Option 2: Using own Subscription and configuring the Environment](docs//Lab%2001:%20Introduction-to-Azure-Portal.md#option-2-using-own-subscription-and-configuring-the-environment)
     * [Exercise 00: Log into your Azure Portal and deploy the pre-requisite environment](docs/Lab%2001:%20Introduction-to-Azure-Portal.md#exercise-00-log-into-your-azure-portal-and-deploy-the-pre-requisite-environment)
* [Lab 02: Deploying Open Shift cluster using ARM templates](docs/Lab%2002:%20Deploying-OpenShift-cluster-using-ARM-templates.md)
   * [Exercise 00: Create an Azure AD Application](docs/Lab%2002:%20Deploying-OpenShift-cluster-using-ARM-templates.md#exercise-00-create-an-azure-ad-application)
   * [Exercise 01: Create a Keyvault](docs/Lab%2002:%20Deploying-OpenShift-cluster-using-ARM-templates.md#exercise-01-create-a-keyvault)
   * [Exercise 02: Deploy Openshift Cluster using ARM Template](docs/Lab%2002:%20Deploying-OpenShift-cluster-using-ARM-templates.md#exercise-02-deploy-openshift-cluster-using-arm-template)
   * [Exercise 03: Configure Azure AD Authentication](docs/Lab%2002:%20Deploying-OpenShift-cluster-using-ARM-templates.md#exercise-03-configure-azure-ad-authentication)
* [Lab 03: Deploying workload on Openshift](docs/Lab%2003:%20Deploying-workload-on-Openshift.md)
   * [Exercise 00: Deploy a 2 Tier Node JS Application on Open Shift](docs/Lab%2003:%20Deploying-workload-on-Openshift.md#exercise-00-deploy-a-2-tier-node-js-application-on-open-shift)
   * [Exercise 01: Installing OpenShift CLI](docs/Lab%2003:%20Deploying-workload-on-Openshift.md#exercise-01-installing-openshift-cli)
   * [Exercise 02: Deployment in OpenShift using CLI](docs/Lab%2003:%20Deploying-workload-on-Openshift.md#exercise-02-deployment-in-openshift-using-cli)
   * [Exercise 03: Create an App using Docker build](docs/Lab%2003:%20Deploying-workload-on-Openshift.md#exercise-03-create-an-app-using-docker-build)
* [Lab 04: Integration of ACR with OpenShift](docs/Lab%2004:%20Integration-of-ACR-%20with-OpenShift.md)
   * [Exercise 00: Integrate ACR with OpenShift](docs/Lab%2004:%20Integration-of-ACR-%20with-OpenShift.md#exercise-00-integrate-acr-with-openshift)
<!-- /TOC -->

## Workshop Architecture and Objective
 As part of Hand on Lab, you will get following details via **email**. Make a note of these details as these shall be leveraged throughout the lab exercise
- Azure Access: Azure Username and Password (Default Password, Change at first login)
- Service Principal Details

### Labs Objective
During this lab, you will deploy Open Shift cluster on Azure and integrate Azure AD Authentication and Azure Container Registry into Open Shift. Detailed steps to achieve this is as follows.
- Get Familiar with Azure Portal
-	Create an Azure AD Application for Authentication
-	Create a key vault to store SSH Key
-	Deploy Open Shift using ARM Template
-	Configure Azure AD Authentication
-	Deploy 2 Tier App on Open Shift
-	Integration of Azure Container Registry with Open Shift

### Workshop Architecture after deploying ARM Template
Following illustrates the architecture in your Azure deployment after completion of exercise’s part of workshop.
<img src="images/1workshop_arch.jpg"/>

[Next>](docs/Lab%2001:%20Introduction-to-Azure-Portal.md)
