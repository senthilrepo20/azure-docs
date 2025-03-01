---
title: Deploy a private mobile network and site - ARM template
titleSuffix: Azure Private 5G Core Preview
description: Learn how to deploy a private mobile network and site using an Azure Resource Manager template (ARM template).
services: azure-resource-manager 
author: djrmetaswitch
ms.author: drichards
ms.service: azure-resource-manager
ms.topic: quickstart
ms.custom: subject-armqs
ms.date: 03/23/2022
---

# Quickstart: Deploy a private mobile network and site - ARM template

Azure Private 5G Core is an Azure cloud service for deploying and managing 5G core network functions on an Azure Stack Edge device, as part of an on-premises private mobile network for enterprises. This quickstart describes how to use an Azure Resource Manager template (ARM template) to deploy the following.

- A private mobile network.
- A site.
- The default service and SIM policy (as described in [Default service and SIM policy](default-service-sim-policy.md)).
- Optionally, one or more SIMs, and a SIM group.

[!INCLUDE [About Azure Resource Manager](../../includes/resource-manager-quickstart-introduction.md)]

If your environment meets the prerequisites and you're familiar with using ARM templates, select the **Deploy to Azure** button. The template will open in the Azure portal.

[![Deploy to Azure.](../media/template-deployments/deploy-to-azure.svg)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fquickstarts%2Fmicrosoft.mobilenetwork%2Fmobilenetwork-create-full-5gc-deployment%2Fazuredeploy.json)

## Prerequisites

- [Complete the prerequisite tasks for deploying a private mobile network](complete-private-mobile-network-prerequisites.md).
- Ensure you can sign in to the Azure portal using an account with access to the active subscription you identified in [Complete the prerequisite tasks for deploying a private mobile network](complete-private-mobile-network-prerequisites.md). This account must have the built-in Contributor or Owner role at the subscription scope.
- [Collect the required information to deploy a private mobile network](collect-required-information-for-private-mobile-network.md). If you want to provision SIMs, you'll need to prepare a JSON file containing your SIM information, as described in [JSON file format for provisioning SIMs](collect-required-information-for-private-mobile-network.md#json-file-format-for-provisioning-sims).
- Identify the names of the interfaces corresponding to ports 5 and 6 on the Azure Stack Edge Pro device in the site.
- [Collect the required information for a site](collect-required-information-for-a-site.md).

## Review the template

The template used in this quickstart is from [Azure Quickstart Templates](https://azure.microsoft.com/resources/templates/mobilenetwork-create-full-5gc-deployment). The template for this article is too long to show here. To view the template, see [azuredeploy.json](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/quickstarts/microsoft.mobilenetwork/mobilenetwork-create-full-5gc-deployment/azuredeploy.json).

The following Azure resources are defined in the template.

- [**Microsoft.MobileNetwork/mobileNetworks/dataNetworks**](/azure/templates/microsoft.mobilenetwork/mobilenetworks/datanetworks): a resource representing a data network.
- [**Microsoft.MobileNetwork/mobileNetworks/slices**](/azure/templates/microsoft.mobilenetwork/mobilenetworks/slices): a resource representing a network slice.
- [**Microsoft.MobileNetwork/mobileNetworks/services**](/azure/templates/microsoft.mobilenetwork/mobilenetworks/services): a resource representing a service.
- [**Microsoft.MobileNetwork/mobileNetworks/simPolicies**](/azure/templates/microsoft.mobilenetwork/mobilenetworks/simPolicies): a resource representing a SIM policy.
- [**Microsoft.MobileNetwork/mobileNetworks/sites**](/azure/templates/microsoft.mobilenetwork/mobilenetworks/sites): a resource representing your site as a whole.
- [**Microsoft.MobileNetwork/packetCoreControlPlanes/packetCoreDataPlanes/attachedDataNetworks**](/azure/templates/microsoft.mobilenetwork/packetcorecontrolplanes/packetcoredataplanes/attacheddatanetworks): a resource providing configuration for the packet core instance's connection to a data network.
- [**Microsoft.MobileNetwork/packetCoreControlPlanes/packetCoreDataPlanes**](/azure/templates/microsoft.mobilenetwork/packetcorecontrolplanes/packetcoredataplanes): a resource providing configuration for the user plane Network Functions of the packet core instance, including IP configuration for the user plane interface on the access network.
- [**Microsoft.MobileNetwork/packetCoreControlPlanes**](/azure/templates/microsoft.mobilenetwork/packetcorecontrolplanes): a resource providing configuration for the control plane Network Functions of the packet core instance, including IP configuration for the control plane interface on the access network.
- [**Microsoft.MobileNetwork/mobileNetworks**](/azure/templates/microsoft.mobilenetwork/mobilenetworks): a resource representing the private mobile network as a whole.
- [**Microsoft.MobileNetwork/simGroups**](/azure/templates/microsoft.mobilenetwork/simGroups): a resource representing a SIM group.
- [**Microsoft.MobileNetwork/simGroups/sims**](/azure/templates/microsoft.mobilenetwork/simGroups/sims): a resource representing a physical SIM or eSIM.

## Deploy the template

1. Select the following link to sign in to Azure and open a template.

    [![Deploy to Azure.](../media/template-deployments/deploy-to-azure.svg)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fquickstarts%2Fmicrosoft.mobilenetwork%2Fmobilenetwork-create-full-5gc-deployment%2Fazuredeploy.json)


1. Select or enter the following values, using the information you retrieved in [Prerequisites](#prerequisites).

    
    |Field  |Value  |
    |---------|---------|
    |**Subscription**     | Select the Azure subscription you want to use to create your private mobile network.        |
    |**Resource group**     | Create a new resource group.        |
    |**Region**     | Select **East US**.        |
    |**Location**     | Leave this field unchanged.        |
    |**Mobile Network Name**     | Enter a name for the private mobile network.        |
    |**Mobile Country Code**     | Enter the mobile country code for the private mobile network.        |
    |**Mobile Network Code**     | Enter the mobile network code for the private mobile network.        |
    |**Site Name**     | Enter a name for your site.        |
    |**Service Name**     | Leave this field unchanged.        |
    |**Sim Policy Name**     | Leave this field unchanged.        |
    |**Slice Name**     | Leave this field unchanged.        |
    |**Sim Group Name**     | If you want to provision SIMs, enter the name of the SIM group to which the SIMs will be added. Otherwise, leave this field blank.        |
    |**Sim Resources**     | If you want to provision SIMs, paste in the contents of the JSON file containing your SIM information. Otherwise, leave this field unchanged.       |
    | **Platform Type** | Ensure **AKS-HCI** is selected. |
    |**Control Plane Access Interface Name**     | Enter the name of the control plane interface on the access network. This must match the corresponding virtual network name on port 5 on your Azure Stack Edge Pro device.        |
    |**Control Plane Access Ip Address**    | Enter the IP address for the control plane interface on the access network.        |
    |**User Plane Access Interface Name**     | Enter the name of the user plane interface on the access network. This must match the corresponding virtual network name on port 5 on your Azure Stack Edge Pro device.        |
    | **User Plane Access Interface Ip Address** | Leave this field blank. |
    |**Access Subnet**     | Enter the network address of the access subnet in Classless Inter-Domain Routing (CIDR) notation.         |
    |**Access Gateway**     | Enter the access subnet default gateway.        |
    |**User Plane Data Interface Name**  | Enter the name of the user plane interface on the data network. This must match the corresponding virtual network name on port 6 on your Azure Stack Edge Pro device. |
    | **User Plane Data Interface Ip Address** | Leave this field blank. |
    |**User Plane Data Interface Subnet**  | Enter the network address of the data subnet in CIDR notation. |
    |**User Plane Data Interface Gateway**  | Enter the data subnet default gateway. |
    |**User Equipment Address Pool Prefix**  | Enter the network address of the subnet from which dynamic IP addresses must be allocated to User Equipment (UEs) in CIDR notation. You can omit this if you don't want to support dynamic IP address allocation. |
    |**User Equipment Static Address Pool Prefix**  | Enter the network address of the subnet from which static IP addresses must be allocated to User Equipment (UEs) in CIDR notation. You can omit this if you don't want to support static IP address allocation. |
    |**Data Network Name**  | Enter the name of the data network. |    
    |**Core Network Technology**  | Enter *5GC* for 5G, or *EPC* for 4G. |
    |**Napt Enabled** | Set this field depending on whether Network Address and Port Translation (NAPT) should be enabled for the data network.|
    | **Dns Addresses** | Enter the DNS server addresses. You can omit this if you don't want to configure a DNS server for the UEs in this data network. |
    |**Custom Location** | Enter the resource ID of the custom location that targets the Azure Kubernetes Service on Azure Stack HCI (AKS-HCI) cluster on the Azure Stack Edge Pro device in the site.|    

1. Select **Review + create**.
1. Azure will now validate the configuration values you've entered. You should see a message indicating that your values have passed validation.

     If the validation fails, you'll see an error message and the **Configuration** tab(s) containing the invalid configuration will be flagged. Select the flagged tab(s) and use the error messages to correct invalid configuration before returning to the **Review + create** tab.

1. Once your configuration has been validated, you can select **Create** to deploy the resources. The Azure portal will display a confirmation screen when the deployment is complete.

## Review deployed resources

1. On the confirmation screen, select **Go to resource group**.

    :::image type="content" source="media/template-deployment-confirmation.png" alt-text="Screenshot of the Azure portal showing a deployment confirmation for the ARM template.":::

1. Confirm that the following resources have been created in the resource group.

    - A **Mobile Network** resource representing the private mobile network as a whole.
    - A **Slice** resource representing a network slice.
    - A **Data Network** resource representing the data network.
    - A **Mobile Network Site** resource representing the site as a whole.
    - A **Packet Core Control Plane** resource representing the control plane function of the packet core instance in the site.
    - A **Packet Core Data Plane** resource representing the data plane function of the packet core instance in the site.
    - An **Attached Data Network** resource representing the site's view of the data network.
    - A **Service** resource representing the default service. 
    - A **SIM Policy** resource representing the default SIM policy.
    - A **SIM Group** resource (if you provisioned any SIMs). 

    :::image type="content" source="media/create-full-private-5g-core-deployment-arm-template/full-deployment-resource-group.png" alt-text="Screenshot of the Azure portal showing a resource group containing the resources for a full Azure Private 5G Core deployment." lightbox="media/create-full-private-5g-core-deployment-arm-template/full-deployment-resource-group.png":::

## Clean up resources

If you do not want to keep your deployment, [delete the resource group](../azure-resource-manager/management/delete-resource-group.md?tabs=azure-portal#delete-resource-group).

## Next steps

If you have kept your deployment, you can either begin designing policy control to determine how your private mobile network will handle traffic, or you can add more sites to your private mobile network.

- [Learn more about designing the policy control configuration for your private mobile network](policy-control.md)
- [Collect the required information for a site](collect-required-information-for-a-site.md)