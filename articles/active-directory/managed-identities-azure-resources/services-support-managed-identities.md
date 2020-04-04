---
title: Yönetilen kimlikleri destekleyen Azure Hizmetleri - Azure AD
description: Azure kaynakları ve Azure AD kimlik doğrulaması için yönetilen kimlikleri destekleyen hizmetlerin listesi
services: active-directory
author: MarkusVi
ms.author: markvi
ms.date: 04/03/2020
ms.topic: conceptual
ms.service: active-directory
ms.subservice: msi
manager: markvi
ms.collection: M365-identity-device-management
ms.openlocfilehash: 4c7a65df100cd58561ce12ac2ae01281eebd419a
ms.sourcegitcommit: 62c5557ff3b2247dafc8bb482256fef58ab41c17
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2020
ms.locfileid: "80656042"
---
# <a name="services-that-support-managed-identities-for-azure-resources"></a>Azure kaynakları için yönetilen kimlikleri destekleyen hizmetler

Azure kaynakları için yönetilen kimlikler, Azure Etkin Dizini'nde otomatik olarak yönetilen bir kimlikle Azure hizmetlerine olanak sağlar. Yönetilen bir kimlik kullanarak, kodunuzda kimlik bilgileri olmadan Azure AD kimlik doğrulamasını destekleyen herhangi bir hizmetin kimliğini doğrulayabilirsiniz. Azure kaynakları için yönetilen kimlikleri ve Azure AD kimlik doğrulamasını Azure genelinde entegre etme sürecindeyiz. Güncelleştirmeler için sık sık kontrol edin.

> [!NOTE]
> Azure kaynakları için yönetilen kimlikler, Yönetilen Hizmet Kimliği (MSI) olarak bilinen hizmetin yeni adıdır.

## <a name="azure-services-that-support-managed-identities-for-azure-resources"></a>Azure kaynakları için yönetilen kimlikleri destekleyen Azure hizmetleri

Aşağıdaki Azure hizmetleri Azure kaynakları için yönetilen kimlikleri destekler:

### <a name="azure-virtual-machines"></a>Azure Sanal Makineler

| Yönetilen kimlik türü | Tüm Genel Olarak Kullanılabilir<br>Genel Azure Bölgeleri | Azure Kamu | Azure Almanya | Azure Çin 21Vianet |
| --- | :-: | :-: | :-: | :-: |
| Atanan sistem | ![Kullanılabilir][check] | ![Kullanılabilir][check] | Önizleme | Önizleme | 
| Atanan kullanıcı | ![Kullanılabilir][check] | ![Kullanılabilir][check] | Önizleme | Önizleme |

Azure Sanal Makineleri için yönetilen kimliği yapılandırmak için aşağıdaki listeye bakın (kullanılabilen bölgelerde):

- [Azure portal](qs-configure-portal-windows-vm.md)
- [PowerShell](qs-configure-powershell-windows-vm.md)
- [Azure CLI](qs-configure-cli-windows-vm.md)
- [Azure Resource Manager şablonları](qs-configure-template-windows-vm.md)
- [REST](qs-configure-rest-vm.md)

### <a name="azure-virtual-machine-scale-sets"></a>Azure Sanal Makine Ölçek Kümeleri

|Yönetilen kimlik türü | Tüm Genel Olarak Kullanılabilir<br>Genel Azure Bölgeleri | Azure Kamu | Azure Almanya | Azure Çin 21Vianet |
| --- | :-: | :-: | :-: | :-: |
| Atanan sistem | ![Kullanılabilir][check] | Önizleme | Önizleme | Önizleme |
| Atanan kullanıcı | ![Kullanılabilir][check] | Önizleme | Önizleme | Önizleme |

Azure Sanal Makine Ölçek Kümeleri için yönetilen kimliği yapılandırmak için aşağıdaki listeye bakın (kullanılabilen bölgelerde):

- [Azure portal](qs-configure-portal-windows-vm.md)
- [PowerShell](qs-configure-powershell-windows-vm.md)
- [Azure CLI](qs-configure-cli-windows-vm.md)
- [Azure Resource Manager şablonları](qs-configure-template-windows-vm.md)
- [REST](qs-configure-rest-vm.md)

### <a name="azure-app-service"></a>Azure App Service

| Yönetilen kimlik türü | Tüm Genel Olarak Kullanılabilir<br>Genel Azure Bölgeleri | Azure Kamu | Azure Almanya | Azure Çin 21Vianet |
| --- | :-: | :-: | :-: | :-: |
| Atanan sistem | ![Kullanılabilir][check] | ![Kullanılabilir][check] | ![Kullanılabilir][check] | ![Kullanılabilir][check] |
| Atanan kullanıcı | ![Kullanılabilir][check] | ![Kullanılabilir][check]  | ![Kullanılabilir][check]  | ![Kullanılabilir][check] |

Azure Uygulama Hizmeti için yönetilen kimliği yapılandırmak için aşağıdaki listeye bakın (kullanılabilen bölgelerde):

- [Azure portal](/azure/app-service/overview-managed-identity#using-the-azure-portal)
- [Azure CLI](/azure/app-service/overview-managed-identity#using-the-azure-cli)
- [Azure PowerShell](/azure/app-service/overview-managed-identity#using-azure-powershell)
- [Azure Kaynak Yöneticisi şablonu](/azure/app-service/overview-managed-identity#using-an-azure-resource-manager-template)

### <a name="azure-blueprints"></a>Azure Blueprints

|Yönetilen kimlik türü | Tüm Genel Olarak Kullanılabilir<br>Genel Azure Bölgeleri | Azure Kamu | Azure Almanya | Azure Çin 21Vianet |
| --- | :-: | :-: | :-: | :-: |
| Atanan sistem | ![Kullanılabilir][check] | ![Kullanılabilir][check] | Kullanılamaz | Kullanılamaz |
| Atanan kullanıcı | ![Kullanılabilir][check] | ![Kullanılabilir][check] | Kullanılamaz | Kullanılamaz |

[Azure Planları](../../governance/blueprints/overview.md)ile yönetilen bir kimliği kullanmak için aşağıdaki listeye bakın:

- [Azure portalı - plan ataması](../../governance/blueprints/create-blueprint-portal.md#assign-a-blueprint)
- [REST API - plan atama](../../governance/blueprints/create-blueprint-rest-api.md#assign-a-blueprint)

### <a name="azure-functions"></a>Azure İşlevleri

Yönetilen kimlik türü |Tüm Genel Olarak Kullanılabilir<br>Genel Azure Bölgeleri | Azure Kamu | Azure Almanya | Azure Çin 21Vianet |
| --- | :-: | :-: | :-: | :-: |
| Atanan sistem | ![Kullanılabilir][check] | ![Kullanılabilir][check] | ![Kullanılabilir][check] | ![Kullanılabilir][check] |
| Atanan kullanıcı | ![Kullanılabilir][check] | ![Kullanılabilir][check]  | ![Kullanılabilir][check]  | ![Kullanılabilir][check]  |

Azure İşlevleri için yönetilen kimliği yapılandırmak için aşağıdaki listeye bakın (kullanılabilen bölgelerde):

- [Azure portal](/azure/app-service/overview-managed-identity#using-the-azure-portal)
- [Azure CLI](/azure/app-service/overview-managed-identity#using-the-azure-cli)
- [Azure PowerShell](/azure/app-service/overview-managed-identity#using-azure-powershell)
- [Azure Kaynak Yöneticisi şablonu](/azure/app-service/overview-managed-identity#using-an-azure-resource-manager-template)

### <a name="azure-logic-apps"></a>Azure Logic Apps

Yönetilen kimlik türü | Tüm Genel Olarak Kullanılabilir<br>Genel Azure Bölgeleri | Azure Kamu | Azure Almanya | Azure Çin 21Vianet |
| --- | :-: | :-: | :-: | :-: |
| Atanan sistem | ![Kullanılabilir][check] | ![Kullanılabilir][check] | Kullanılamaz | ![Kullanılabilir][check] |
| Atanan kullanıcı | ![Kullanılabilir][check] | ![Kullanılabilir][check] | Kullanılamaz | ![Kullanılabilir][check] |


Azure Logic Apps için yönetilen kimliği yapılandırmak için aşağıdaki listeye bakın (kullanılabilen bölgelerde):

- [Azure portal](/azure/logic-apps/create-managed-service-identity#enable-system-assigned-identity-in-azure-portal)
- [Azure Kaynak Yöneticisi şablonu](/azure/app-service/overview-managed-identity)

### <a name="azure-data-factory-v2"></a>Azure Data Factory V2

Yönetilen kimlik türü | Tüm Genel Olarak Kullanılabilir<br>Genel Azure Bölgeleri | Azure Kamu | Azure Almanya | Azure Çin 21Vianet |
| --- | :-: | :-: | :-: | :-: |
| Atanan sistem | ![Kullanılabilir][check] | ![Kullanılabilir][check] | Kullanılamaz | ![Kullanılabilir][check] |
| Atanan kullanıcı | Kullanılamaz | Kullanılamaz | Kullanılamaz | Kullanılamaz |

Azure Veri Fabrikası V2 için yönetilen kimliği yapılandırmak için aşağıdaki listeye bakın (kullanılabilen bölgelerde):

- [Azure portal](~/articles/data-factory/data-factory-service-identity.md#generate-managed-identity)
- [PowerShell](~/articles/data-factory/data-factory-service-identity.md#generate-managed-identity-using-powershell)
- [REST](~/articles/data-factory/data-factory-service-identity.md#generate-managed-identity-using-rest-api)
- [SDK](~/articles/data-factory/data-factory-service-identity.md#generate-managed-identity-using-sdk)

### <a name="azure-api-management"></a>Azure API Management

Yönetilen kimlik türü | Tüm Genel Olarak Kullanılabilir<br>Genel Azure Bölgeleri | Azure Kamu | Azure Almanya | Azure Çin 21Vianet |
| --- | :-: | :-: | :-: | :-: |
| Atanan sistem | ![Kullanılabilir][check] | ![Kullanılabilir][check] | Kullanılamaz | ![Kullanılabilir][check] |
| Atanan kullanıcı | Önizleme | Önizleme | Kullanılamaz | Önizleme |

Azure API Yönetimi için yönetilen kimliği yapılandırmak için aşağıdaki listeye bakın (kullanılabilen bölgelerde):

- [Azure Kaynak Yöneticisi şablonu](/azure/api-management/api-management-howto-use-managed-service-identity)

### <a name="azure-container-instances"></a>Azure Container Instances

Yönetilen kimlik türü | Tüm Genel Olarak Kullanılabilir<br>Genel Azure Bölgeleri | Azure Kamu | Azure Almanya | Azure Çin 21Vianet |
| --- | :-: | :-: | :-: | :-: |
| Atanan sistem | Linux: Önizleme<br>Windows: Kullanılamıyor | Kullanılamaz | Kullanılamaz | Kullanılamaz |
| Atanan kullanıcı | Linux: Önizleme<br>Windows: Kullanılamıyor | Kullanılamaz | Kullanılamaz | Kullanılamaz |

Azure Kapsayıcı Örnekleri için yönetilen kimliği yapılandırmak için aşağıdaki listeye bakın (kullanılabilen bölgelerde):

- [Azure CLI](~/articles/container-instances/container-instances-managed-identity.md)
- [Azure Kaynak Yöneticisi şablonu](~/articles/container-instances/container-instances-managed-identity.md#enable-managed-identity-using-resource-manager-template)
- [YAML](~/articles/container-instances/container-instances-managed-identity.md#enable-managed-identity-using-yaml-file)

### <a name="azure-container-registry-tasks"></a>Azure Container Registry Görevleri

Yönetilen kimlik türü | Tüm Genel Olarak Kullanılabilir<br>Genel Azure Bölgeleri | Azure Kamu | Azure Almanya | Azure Çin 21Vianet |
| --- | :-: | :-: | :-: | :-: |
| Atanan sistem | ![Kullanılabilir][check] | Kullanılamaz | Kullanılamaz | Kullanılamaz |
| Atanan kullanıcı | Önizleme | Kullanılamaz | Kullanılamaz | Kullanılamaz |

Azure Kapsayıcı Kayıt Defteri Görevleri için yönetilen kimliği yapılandırmak için aşağıdaki listeye bakın (kullanılabilen bölgelerde):

- [Azure CLI](~/articles/container-registry/container-registry-tasks-authentication-managed-identity.md)

### <a name="azure-service-fabric"></a>Azure Service Fabric
[Hizmet Kumaş Uygulamaları için Yönetilen Kimlik](https://docs.microsoft.com/azure/service-fabric/concepts-managed-identity) Önizleme'de ve tüm bölgelerde mevcuttur.

Yönetilen kimlik türü | Tüm Genel Olarak Kullanılabilir<br>Genel Azure Bölgeleri | Azure Kamu | Azure Almanya | Azure Çin 21Vianet |
| --- | :-: | :-: | :-: | :-: |
| Atanan sistem | ![Kullanılabilir][check] | Kullanılamaz | Kullanılamaz | Kullanılamıyor |
| Atanan kullanıcı | ![Kullanılabilir][check] | Kullanılamaz | Kullanılamaz |Kullanılamaz |

Tüm bölgelerdeki Azure Hizmet Kumaşı uygulamaları için yönetilen kimliği yapılandırmak için aşağıdaki listeye bakın:
- [Azure Kaynak Yöneticisi şablonu](https://github.com/Azure-Samples/service-fabric-managed-identity/tree/anmenard-docs)

## <a name="azure-services-that-support-azure-ad-authentication"></a>Azure AD kimlik doğrulamasını destekleyen Azure hizmetleri

Aşağıdaki hizmetler Azure AD kimlik doğrulamasını destekler ve Azure kaynakları için yönetilen kimlikler kullanan istemci hizmetleriyle sınanmıştır.

### <a name="azure-resource-manager"></a>Azure Resource Manager

Azure Kaynak Yöneticisi'ne erişimi yapılandırmak için aşağıdaki listeye bakın:

- [Azure portalı üzerinden erişim atama](howto-assign-access-portal.md)
- [PowerShell üzerinden erişim atama](howto-assign-access-powershell.md)
- [Azure CLI ile erişim atama](howto-assign-access-CLI.md)
- [Azure Kaynak Yöneticisi şablonu yla erişim atama](../../role-based-access-control/role-assignments-template.md)

| Bulut | Kaynak kimliği | Durum |
|--------|------------|:-:|
| Azure Global | `https://management.azure.com/`| ![Kullanılabilir][check] |
| Azure Kamu | `https://management.usgovcloudapi.net/` | ![Kullanılabilir][check] |
| Azure Almanya | `https://management.microsoftazure.de/` | ![Kullanılabilir][check] |
| Azure Çin 21Vianet | `https://management.chinacloudapi.cn` | ![Kullanılabilir][check] |

### <a name="azure-key-vault"></a>Azure Key Vault

| Bulut | Kaynak kimliği | Durum |
|--------|------------|:-:|
| Azure Global | `https://vault.azure.net`| ![Kullanılabilir][check] |
| Azure Kamu | `https://vault.usgovcloudapi.net` | ![Kullanılabilir][check] |
| Azure Almanya |  `https://vault.microsoftazure.de` | ![Kullanılabilir][check] |
| Azure Çin 21Vianet | `https://vault.azure.cn` | ![Kullanılabilir][check] |

### <a name="azure-data-lake"></a>Azure Data Lake 

| Bulut | Kaynak kimliği | Durum |
|--------|------------|:-:|
| Azure Global | `https://datalake.azure.net/` | ![Kullanılabilir][check] |
| Azure Kamu |  | Kullanılamaz |
| Azure Almanya |   | Kullanılamaz |
| Azure Çin 21Vianet |  | Kullanılamaz |

### <a name="azure-sql"></a>Azure SQL 

| Bulut | Kaynak kimliği | Durum |
|--------|------------|:-:|
| Azure Global | `https://database.windows.net/` | ![Kullanılabilir][check] |
| Azure Kamu | `https://database.usgovcloudapi.net/` | ![Kullanılabilir][check] |
| Azure Almanya | `https://database.cloudapi.de/` | ![Kullanılabilir][check] |
| Azure Çin 21Vianet | `https://database.chinacloudapi.cn/` | ![Kullanılabilir][check] |

### <a name="azure-event-hubs"></a>Azure Event Hubs

| Bulut | Kaynak kimliği | Durum |
|--------|------------|:-:|
| Azure Global | `https://eventhubs.azure.net` | ![Kullanılabilir][check] |
| Azure Kamu |  | Kullanılamaz |
| Azure Almanya |   | Kullanılamaz |
| Azure Çin 21Vianet |  | Kullanılamaz |

### <a name="azure-service-bus"></a>Azure Service Bus

| Bulut | Kaynak kimliği | Durum |
|--------|------------|:-:|
| Azure Global | `https://servicebus.azure.net`  | ![Kullanılabilir][check] |
| Azure Kamu |  | ![Kullanılabilir][check] |
| Azure Almanya |   | Kullanılamaz |
| Azure Çin 21Vianet |  | Kullanılamaz |









### <a name="azure-storage-blobs-and-queues"></a>Azure Depolama lekeleri ve kuyrukları

| Bulut | Kaynak kimliği | Durum |
|--------|------------|:-:|
| Azure Global | `https://storage.azure.com/` <br /><br />`https://<account>.blob.core.windows.net` <br /><br />`https://<account>.queue.core.windows.net` | ![Kullanılabilir][check] |
| Azure Kamu | `https://storage.azure.com/`<br /><br />`https://<account>.blob.core.usgovcloudapi.net` <br /><br />`https://<account>.queue.core.usgovcloudapi.net` | ![Kullanılabilir][check] |
| Azure Almanya | `https://storage.azure.com/`<br /><br />`https://<account>.blob.core.cloudapi.de` <br /><br />`https://<account>.queue.core.cloudapi.de` | ![Kullanılabilir][check] |
| Azure Çin 21Vianet | `https://storage.azure.com/`<br /><br />`https://<account>.blob.core.chinacloudapi.cn` <br /><br />`https://<account>.queue.core.chinacloudapi.cn` | ![Kullanılabilir][check] |










### <a name="azure-analysis-services"></a>Azure Analysis Services

| Bulut | Kaynak kimliği | Durum |
|--------|------------|:-:|
| Azure Global | `https://*.asazure.windows.net` | ![Kullanılabilir][check] |
| Azure Kamu | `https://*.asazure.usgovcloudapi.net` | ![Kullanılabilir][check] |
| Azure Almanya | `https://*.asazure.cloudapi.de` | ![Kullanılabilir][check] |
| Azure Çin 21Vianet | `https://*.asazure.chinacloudapi.cn` | ![Kullanılabilir][check] |


[check]: media/services-support-managed-identities/check.png "Kullanılabilir"
