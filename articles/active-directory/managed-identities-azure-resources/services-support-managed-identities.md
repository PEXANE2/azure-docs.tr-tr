---
title: Azure Services that support managed identities - Azure AD
description: List of services that support managed identities for Azure resources and Azure AD authentication
services: active-directory
author: MarkusVi
ms.author: markvi
ms.date: 09/24/2019
ms.topic: conceptual
ms.service: active-directory
ms.subservice: msi
manager: markvi
ms.collection: M365-identity-device-management
ms.openlocfilehash: 8ead9b53f530a309d6bdb3bd384c29650bf5c8e6
ms.sourcegitcommit: d6b68b907e5158b451239e4c09bb55eccb5fef89
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/20/2019
ms.locfileid: "74224300"
---
# <a name="services-that-support-managed-identities-for-azure-resources"></a>Services that support managed identities for Azure resources

Managed identities for Azure resources provide Azure services with an automatically managed identity in Azure Active Directory. Using a managed identity, you can authenticate to any service that supports Azure AD authentication without having credentials in your code. We are in the process of integrating managed identities for Azure resources and Azure AD authentication across Azure. Check back often for updates.

> [!NOTE]
> Azure kaynakları için yönetilen kimlikler, Yönetilen Hizmet Kimliği (MSI) olarak bilinen hizmetin yeni adıdır.

## <a name="azure-services-that-support-managed-identities-for-azure-resources"></a>Azure kaynakları için yönetilen kimlikleri destekleyen Azure hizmetleri

The following Azure services support managed identities for Azure resources:

### <a name="azure-virtual-machines"></a>Azure Sanal Makineler

| Managed identity type | All Generally Available<br>Global Azure Regions | Azure Devlet Kurumları | Azure Almanya | Azure Çin 21Vianet |
| --- | --- | --- | --- | --- |
| System assigned | Kullanılabilir | Önizleme | Önizleme | Önizleme | 
| User assigned | Kullanılabilir | Önizleme | Önizleme | Önizleme |

Refer to the following list to configure managed identity for Azure Virtual Machines (in regions where available):

- [Azure portalda](qs-configure-portal-windows-vm.md)
- [PowerShell](qs-configure-powershell-windows-vm.md)
- [Azure CLI](qs-configure-cli-windows-vm.md)
- [Azure Resource Manager templates](qs-configure-template-windows-vm.md)
- [REST](qs-configure-rest-vm.md)

### <a name="azure-virtual-machine-scale-sets"></a>Azure Virtual Machine Scale Sets

|Managed identity type | All Generally Available<br>Global Azure Regions | Azure Devlet Kurumları | Azure Almanya | Azure Çin 21Vianet |
| --- | --- | --- | --- | --- |
| System assigned | Kullanılabilir | Önizleme | Önizleme | Önizleme |
| User assigned | Kullanılabilir | Önizleme | Önizleme | Önizleme |

Refer to the following list to configure managed identity for Azure Virtual Machine Scale Sets (in regions where available):

- [Azure portalda](qs-configure-portal-windows-vm.md)
- [PowerShell](qs-configure-powershell-windows-vm.md)
- [Azure CLI](qs-configure-cli-windows-vm.md)
- [Azure Resource Manager templates](qs-configure-template-windows-vm.md)
- [REST](qs-configure-rest-vm.md)

### <a name="azure-app-service"></a>Azure App Service

| Managed identity type | All Generally Available<br>Global Azure Regions | Azure Devlet Kurumları | Azure Almanya | Azure Çin 21Vianet |
| --- | --- | --- | --- | --- |
| System assigned | Kullanılabilir | Kullanılabilir | Kullanılabilir | Kullanılabilir |
| User assigned | Kullanılabilir | Kullanılamıyor | Kullanılamıyor | Kullanılamıyor |

Refer to the following list to configure managed identity for Azure App Service (in regions where available):

- [Azure portalda](/azure/app-service/overview-managed-identity#using-the-azure-portal)
- [Azure CLI](/azure/app-service/overview-managed-identity#using-the-azure-cli)
- [Azure PowerShell](/azure/app-service/overview-managed-identity#using-azure-powershell)
- [Azure Resource Manager şablonu](/azure/app-service/overview-managed-identity#using-an-azure-resource-manager-template)

### <a name="azure-blueprints"></a>Azure Blueprints

|Managed identity type | All Generally Available<br>Global Azure Regions | Azure Devlet Kurumları | Azure Almanya | Azure Çin 21Vianet |
| --- | --- | --- | --- | --- |
| System assigned | Kullanılabilir | Kullanılabilir | Kullanılamıyor | Kullanılamıyor |
| User assigned | Kullanılabilir | Kullanılabilir | Kullanılamıyor | Kullanılamıyor |

Refer to the following list to use a managed identity with [Azure Blueprints](../../governance/blueprints/overview.md):

- [Azure portal - blueprint assignment](../../governance/blueprints/create-blueprint-portal.md#assign-a-blueprint)
- [REST API - blueprint assignment](../../governance/blueprints/create-blueprint-rest-api.md#assign-a-blueprint)

### <a name="azure-functions"></a>Azure İşlevleri

Managed identity type |All Generally Available<br>Global Azure Regions | Azure Devlet Kurumları | Azure Almanya | Azure Çin 21Vianet |
| --- | --- | --- | --- | --- |
| System assigned | Kullanılabilir | Kullanılabilir | Kullanılabilir | Kullanılabilir |
| User assigned | Kullanılabilir | Kullanılamıyor | Kullanılamıyor | Kullanılamıyor |

Refer to the following list to configure managed identity for Azure Functions (in regions where available):

- [Azure portalda](/azure/app-service/overview-managed-identity#using-the-azure-portal)
- [Azure CLI](/azure/app-service/overview-managed-identity#using-the-azure-cli)
- [Azure PowerShell](/azure/app-service/overview-managed-identity#using-azure-powershell)
- [Azure Resource Manager şablonu](/azure/app-service/overview-managed-identity#using-an-azure-resource-manager-template)

### <a name="azure-logic-apps"></a>Azure Logic Apps

Managed identity type | All Generally Available<br>Global Azure Regions | Azure Devlet Kurumları | Azure Almanya | Azure Çin 21Vianet |
| --- | --- | --- | --- | --- |
| System assigned | Önizleme | Önizleme | Kullanılamıyor | Önizleme |
| User assigned | Kullanılamıyor | Kullanılamıyor | Kullanılamıyor | Kullanılamıyor |

Refer to the following list to configure managed identity for Azure Logic Apps (in regions where available):

- [Azure portalda](/azure/logic-apps/create-managed-service-identity#azure-portal-system-logic-app)
- [Azure Resource Manager şablonu](/azure/app-service/overview-managed-identity)

### <a name="azure-data-factory-v2"></a>Azure Data Factory V2

Managed identity type | All Generally Available<br>Global Azure Regions | Azure Devlet Kurumları | Azure Almanya | Azure Çin 21Vianet |
| --- | --- | --- | --- | --- |
| System assigned | Kullanılabilir | Kullanılamıyor | Kullanılamıyor | Kullanılamıyor |
| User assigned | Kullanılamıyor | Kullanılamıyor | Kullanılamıyor | Kullanılamıyor |

Refer to the following list to configure managed identity for Azure Data Factory V2 (in regions where available):

- [Azure portalda](~/articles/data-factory/data-factory-service-identity.md#generate-managed-identity)
- [PowerShell](~/articles/data-factory/data-factory-service-identity.md#generate-managed-identity-using-powershell)
- [REST](~/articles/data-factory/data-factory-service-identity.md#generate-managed-identity-using-rest-api)
- [SDK](~/articles/data-factory/data-factory-service-identity.md#generate-managed-identity-using-sdk)

### <a name="azure-api-management"></a>Azure API Management

Managed identity type | All Generally Available<br>Global Azure Regions | Azure Devlet Kurumları | Azure Almanya | Azure Çin 21Vianet |
| --- | --- | --- | --- | --- |
| System assigned | Kullanılabilir | Kullanılabilir | Kullanılamıyor | Kullanılamıyor |
| User assigned | Kullanılamıyor | Kullanılamıyor | Kullanılamıyor | Kullanılamıyor |

Refer to the following list to configure managed identity for Azure API Management (in regions where available):

- [Azure Resource Manager şablonu](/azure/api-management/api-management-howto-use-managed-service-identity)

### <a name="azure-container-instances"></a>Azure Container Instances

Managed identity type | All Generally Available<br>Global Azure Regions | Azure Devlet Kurumları | Azure Almanya | Azure Çin 21Vianet |
| --- | --- | --- | --- | --- |
| System assigned | Linux: Preview<br>Windows: Not available | Kullanılamıyor | Kullanılamıyor | Kullanılamıyor |
| User assigned | Linux: Preview<br>Windows: Not available | Kullanılamıyor | Kullanılamıyor | Kullanılamıyor |

Refer to the following list to configure managed identity for Azure Container Instances (in regions where available):

- [Azure CLI](~/articles/container-instances/container-instances-managed-identity.md)
- [Azure Resource Manager şablonu](~/articles/container-instances/container-instances-managed-identity.md#enable-managed-identity-using-resource-manager-template)
- [YAML](~/articles/container-instances/container-instances-managed-identity.md#enable-managed-identity-using-yaml-file)

### <a name="azure-container-registry-tasks"></a>Azure Container Registry Görevleri

Managed identity type | All Generally Available<br>Global Azure Regions | Azure Devlet Kurumları | Azure Almanya | Azure Çin 21Vianet |
| --- | --- | --- | --- | --- |
| System assigned | Kullanılabilir | Kullanılamıyor | Kullanılamıyor | Kullanılamıyor |
| User assigned | Önizleme | Kullanılamıyor | Kullanılamıyor | Kullanılamıyor |

Refer to the following list to configure managed identity for Azure Container Registry Tasks (in regions where available):

- [Azure CLI](~/articles/container-registry/container-registry-tasks-authentication-managed-identity.md)

## <a name="azure-services-that-support-azure-ad-authentication"></a>Azure services that support Azure AD authentication

The following services support Azure AD authentication, and have been tested with client services that use managed identities for Azure resources.

### <a name="azure-resource-manager"></a>Azure Resource Manager

Refer to the following list to configure access to Azure Resource Manager:

- [Assign access via Azure portal](howto-assign-access-portal.md)
- [Assign access via Powershell](howto-assign-access-powershell.md)
- [Assign access via Azure CLI](howto-assign-access-CLI.md)
- [Assign access via Azure Resource Manager template](../../role-based-access-control/role-assignments-template.md)

| Bulut | Kaynak kimliği | Durum |
|--------|------------|--------|
| Azure Global | `https://management.azure.com/`| Kullanılabilir |
| Azure Devlet Kurumları | `https://management.usgovcloudapi.net/` | Kullanılabilir |
| Azure Almanya | `https://management.microsoftazure.de/` | Kullanılabilir |
| Azure Çin 21Vianet | `https://management.chinacloudapi.cn` | Kullanılabilir |

### <a name="azure-key-vault"></a>Azure Key Vault

| Bulut | Kaynak kimliği | Durum |
|--------|------------|--------|
| Azure Global | `https://vault.azure.net`| Kullanılabilir |
| Azure Devlet Kurumları | `https://vault.usgovcloudapi.net` | Kullanılabilir |
| Azure Almanya |  `https://vault.microsoftazure.de` | Kullanılabilir |
| Azure Çin 21Vianet | `https://vault.azure.cn` | Kullanılabilir |

### <a name="azure-data-lake"></a>Azure Data Lake 

| Bulut | Kaynak kimliği | Durum |
|--------|------------|--------|
| Azure Global | `https://datalake.azure.net/` | Kullanılabilir |
| Azure Devlet Kurumları |  | Yok |
| Azure Almanya |   | Yok |
| Azure Çin 21Vianet |  | Yok |

### <a name="azure-sql"></a>Azure SQL 

| Bulut | Kaynak kimliği | Durum |
|--------|------------|--------|
| Azure Global | `https://database.windows.net/` | Kullanılabilir |
| Azure Devlet Kurumları | `https://database.usgovcloudapi.net/` | Kullanılabilir |
| Azure Almanya | `https://database.cloudapi.de/` | Kullanılabilir |
| Azure Çin 21Vianet | `https://database.chinacloudapi.cn/` | Kullanılabilir |

### <a name="azure-event-hubs"></a>Azure Event Hubs

| Bulut | Kaynak kimliği | Durum |
|--------|------------|--------|
| Azure Global | `https://eventhubs.azure.net` | Kullanılabilir |
| Azure Devlet Kurumları |  | Yok |
| Azure Almanya |   | Yok |
| Azure Çin 21Vianet |  | Yok |

### <a name="azure-service-bus"></a>Azure Service Bus

| Bulut | Kaynak kimliği | Durum |
|--------|------------|--------|
| Azure Global | `https://servicebus.azure.net`  | Kullanılabilir |
| Azure Devlet Kurumları |  | Kullanılabilir |
| Azure Almanya |   | Yok |
| Azure Çin 21Vianet |  | Yok |









### <a name="azure-storage-blobs-and-queues"></a>Azure Storage blobs and queues

| Bulut | Kaynak kimliği | Durum |
|--------|------------|--------|
| Azure Global | `https://storage.azure.com/` <br /><br />`https://<account>.blob.core.windows.net` <br /><br />`https://<account>.queue.core.windows.net` | Kullanılabilir |
| Azure Devlet Kurumları | `https://storage.azure.com/`<br /><br />`https://<account>.blob.core.usgovcloudapi.net` <br /><br />`https://<account>.queue.core.usgovcloudapi.net` | Kullanılabilir |
| Azure Almanya | `https://storage.azure.com/`<br /><br />`https://<account>.blob.core.cloudapi.de` <br /><br />`https://<account>.queue.core.cloudapi.de` | Kullanılabilir |
| Azure Çin 21Vianet | `https://storage.azure.com/`<br /><br />`https://<account>.blob.core.chinacloudapi.cn` <br /><br />`https://<account>.queue.core.chinacloudapi.cn` | Kullanılabilir |










### <a name="azure-analysis-services"></a>Azure Analysis Services

| Bulut | Kaynak kimliği | Durum |
|--------|------------|--------|
| Azure Global | `https://*.asazure.windows.net` | Kullanılabilir |
| Azure Devlet Kurumları | `https://*.asazure.usgovcloudapi.net` | Kullanılabilir |
| Azure Almanya | `https://*.asazure.cloudapi.de` | Kullanılabilir |
| Azure Çin 21Vianet | `https://*.asazure.chinacloudapi.cn` | Kullanılabilir |
