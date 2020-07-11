---
title: Yönetilen kimlikleri destekleyen Azure hizmetleri-Azure AD
description: Azure kaynakları ve Azure AD kimlik doğrulaması için yönetilen kimlikleri destekleyen hizmetlerin listesi
services: active-directory
author: MarkusVi
ms.author: markvi
ms.date: 07/09/2020
ms.topic: conceptual
ms.service: active-directory
ms.subservice: msi
manager: markvi
ms.collection: M365-identity-device-management
ms.custom: references_regions
ms.openlocfilehash: 5bf397cac26d3cc46e11f8ac60110629a528cd62
ms.sourcegitcommit: f7e160c820c1e2eb57dc480b2a8fd6bef7053e91
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2020
ms.locfileid: "86229831"
---
# <a name="services-that-support-managed-identities-for-azure-resources"></a>Azure kaynakları için yönetilen kimlikleri destekleyen hizmetler

Azure kaynakları için Yönetilen kimlikler, Azure Active Directory ' de otomatik olarak yönetilen kimlik ile Azure hizmetleri sağlar. Yönetilen bir kimlik kullanarak, kodunuzda kimlik bilgileri olmadan Azure AD kimlik doğrulamasını destekleyen herhangi bir hizmette kimlik doğrulaması yapabilirsiniz. Azure kaynakları ve Azure AD kimlik doğrulaması için yönetilen kimlikleri Azure üzerinde tümleştirme sürecimiz vardır. Güncelleştirmeler için sık sık kontrol edin.

> [!NOTE]
> Azure kaynakları için yönetilen kimlikler, Yönetilen Hizmet Kimliği (MSI) olarak bilinen hizmetin yeni adıdır.

## <a name="azure-services-that-support-managed-identities-for-azure-resources"></a>Azure kaynakları için yönetilen kimlikleri destekleyen Azure hizmetleri

Aşağıdaki Azure hizmetleri Azure kaynakları için yönetilen kimlikleri destekler:


### <a name="azure-api-management"></a>Azure API Management

Yönetilen kimlik türü | Tüm genel kullanıma açık<br>Küresel Azure bölgeleri | Azure Kamu | Azure Almanya | Azure Çin 21Vianet |
| --- | :-: | :-: | :-: | :-: |
| Sistem atandı | ![Kullanılabilir][check] | ![Kullanılabilir][check] | Kullanılamaz | ![Kullanılabilir][check] |
| Kullanıcı atandı | Önizleme | Önizleme | Kullanılamaz | Önizleme |

Azure API Management yönetilen kimliğini (varsa) yapılandırmak için aşağıdaki listeye bakın:

- [Azure Resource Manager şablonu](/azure/api-management/api-management-howto-use-managed-service-identity)


### <a name="azure-app-service"></a>Azure App Service

| Yönetilen kimlik türü | Tüm genel kullanıma açık<br>Küresel Azure bölgeleri | Azure Kamu | Azure Almanya | Azure Çin 21Vianet |
| --- | :-: | :-: | :-: | :-: |
| Sistem atandı | ![Kullanılabilir][check] | ![Kullanılabilir][check] | ![Kullanılabilir][check] | ![Kullanılabilir][check] |
| Kullanıcı atandı | ![Kullanılabilir][check] | ![Kullanılabilir][check]  | ![Kullanılabilir][check]  | ![Kullanılabilir][check] |

Azure App Service yönetilen kimliği (varsa) yapılandırmak için aşağıdaki listeye bakın:

- [Azure portalındaki](/azure/app-service/overview-managed-identity#using-the-azure-portal)
- [Azure CLI](/azure/app-service/overview-managed-identity#using-the-azure-cli)
- [Azure PowerShell](/azure/app-service/overview-managed-identity#using-azure-powershell)
- [Azure Resource Manager şablonu](/azure/app-service/overview-managed-identity#using-an-azure-resource-manager-template)

### <a name="azure-arc-enabled-kubernetes"></a>Azure Arc özellikli Kubernetes

| Yönetilen kimlik türü | Tüm genel kullanıma açık<br>Küresel Azure bölgeleri | Azure Kamu | Azure Almanya | Azure Çin 21Vianet |
| --- | :-: | :-: | :-: | :-: |
| Sistem atandı | Önizleme | Kullanılamaz | Kullanılamaz | Kullanılamaz | 
| Kullanıcı atandı | Kullanılamaz | Kullanılamaz | Kullanılamaz | Kullanılamaz |

Azure Arc etkin Kubernetes Şu anda [sistem tarafından atanan kimliği desteklemektedir](https://docs.microsoft.com/azure/azure-arc/kubernetes/connect-cluster#azure-arc-agents-for-kubernetes). Yönetilen hizmet kimlik sertifikası, Azure ile iletişim kurmak için tüm Azure Arc etkinleştirilmiş Kubernetes aracıları tarafından kullanılır.

### <a name="azure-blueprints"></a>Azure Blueprints

|Yönetilen kimlik türü | Tüm genel kullanıma açık<br>Küresel Azure bölgeleri | Azure Kamu | Azure Almanya | Azure Çin 21Vianet |
| --- | :-: | :-: | :-: | :-: |
| Sistem atandı | ![Kullanılabilir][check] | ![Kullanılabilir][check] | Kullanılamaz | Kullanılamaz |
| Kullanıcı atandı | ![Kullanılabilir][check] | ![Kullanılabilir][check] | Kullanılamaz | Kullanılamaz |

[Azure şemaları](../../governance/blueprints/overview.md)ile yönetilen bir kimlik kullanmak için aşağıdaki listeye bakın:

- [Azure portal şeması atama](../../governance/blueprints/create-blueprint-portal.md#assign-a-blueprint)
- [REST API şeması atama](../../governance/blueprints/create-blueprint-rest-api.md#assign-a-blueprint)


### <a name="azure-cognitive-search"></a>Azure Bilişsel Arama

Yönetilen kimlik türü | Tüm genel kullanıma açık<br>Küresel Azure bölgeleri | Azure Kamu | Azure Almanya | Azure Çin 21Vianet |
| --- | :-: | :-: | :-: | :-: |
| Sistem atandı | ![Kullanılabilir][check] | ![Kullanılabilir][check] | Kullanılamaz | ![Kullanılabilir][check] |
| Kullanıcı atandı | Kullanılamaz | Kullanılamaz | Kullanılamaz | Kullanılamaz |


### <a name="azure-container-instances"></a>Azure Container Instances

Yönetilen kimlik türü | Tüm genel kullanıma açık<br>Küresel Azure bölgeleri | Azure Kamu | Azure Almanya | Azure Çin 21Vianet |
| --- | :-: | :-: | :-: | :-: |
| Sistem atandı | Linux: Önizleme<br>Windows: kullanılamıyor | Kullanılamaz | Kullanılamaz | Kullanılamaz |
| Kullanıcı atandı | Linux: Önizleme<br>Windows: kullanılamıyor | Kullanılamaz | Kullanılamaz | Kullanılamaz |

Azure Container Instances yönetilen kimliği (varsa) yapılandırmak için aşağıdaki listeye bakın:

- [Azure CLI](~/articles/container-instances/container-instances-managed-identity.md)
- [Azure Resource Manager şablonu](~/articles/container-instances/container-instances-managed-identity.md#enable-managed-identity-using-resource-manager-template)
- [YAML](~/articles/container-instances/container-instances-managed-identity.md#enable-managed-identity-using-yaml-file)


### <a name="azure-container-registry-tasks"></a>Azure Container Registry Görevleri

Yönetilen kimlik türü | Tüm genel kullanıma açık<br>Küresel Azure bölgeleri | Azure Kamu | Azure Almanya | Azure Çin 21Vianet |
| --- | :-: | :-: | :-: | :-: |
| Sistem atandı | ![Kullanılabilir][check] | Kullanılamaz | Kullanılamaz | Kullanılamaz |
| Kullanıcı atandı | Önizleme | Kullanılamaz | Kullanılamaz | Kullanılamaz |

Azure Container Registry görevler için yönetilen kimliği yapılandırmak üzere aşağıdaki listeye bakın (kullanılabilir yerlerde olan bölgelerde):

- [Azure CLI](~/articles/container-registry/container-registry-tasks-authentication-managed-identity.md)

### <a name="azure-data-explorer"></a>Azure Veri Gezgini

Yönetilen kimlik türü | Tüm genel kullanıma açık<br>Küresel Azure bölgeleri | Azure Kamu | Azure Almanya | Azure Çin 21Vianet |
| --- | :-: | :-: | :-: | :-: |
| Sistem atandı | ![Kullanılabilir][check] | ![Kullanılabilir][check] | Kullanılamaz | ![Kullanılabilir][check] |
| Kullanıcı atandı | Kullanılamaz | Kullanılamaz | Kullanılamaz | Kullanılamaz |

### <a name="azure-data-factory-v2"></a>Azure Data Factory V2

Yönetilen kimlik türü | Tüm genel kullanıma açık<br>Küresel Azure bölgeleri | Azure Kamu | Azure Almanya | Azure Çin 21Vianet |
| --- | :-: | :-: | :-: | :-: |
| Sistem atandı | ![Kullanılabilir][check] | ![Kullanılabilir][check] | Kullanılamaz | ![Kullanılabilir][check] |
| Kullanıcı atandı | Kullanılamaz | Kullanılamaz | Kullanılamaz | Kullanılamaz |

Azure Data Factory v2 için yönetilen kimliği yapılandırmak üzere aşağıdaki listeye bakın (varsa):

- [Azure portalındaki](~/articles/data-factory/data-factory-service-identity.md#generate-managed-identity)
- [PowerShell](~/articles/data-factory/data-factory-service-identity.md#generate-managed-identity-using-powershell)
- [REST](~/articles/data-factory/data-factory-service-identity.md#generate-managed-identity-using-rest-api)
- [SDK](~/articles/data-factory/data-factory-service-identity.md#generate-managed-identity-using-sdk)



### <a name="azure-event-grid"></a>Azure Event Grid 

Yönetilen kimlik türü |Tüm genel kullanıma açık<br>Küresel Azure bölgeleri | Azure Kamu | Azure Almanya | Azure Çin 21Vianet |
| --- | :-: | :-: | :-: | :-: |
| Sistem atandı | Önizleme | Kullanılamaz | Kullanılamaz | Kullanılamaz |
| Kullanıcı atandı | Kullanılamaz | Kullanılamaz  | Kullanılamaz  | Kullanılamaz |









### <a name="azure-functions"></a>Azure İşlevleri

Yönetilen kimlik türü |Tüm genel kullanıma açık<br>Küresel Azure bölgeleri | Azure Kamu | Azure Almanya | Azure Çin 21Vianet |
| --- | :-: | :-: | :-: | :-: |
| Sistem atandı | ![Kullanılabilir][check] | ![Kullanılabilir][check] | ![Kullanılabilir][check] | ![Kullanılabilir][check] |
| Kullanıcı atandı | ![Kullanılabilir][check] | ![Kullanılabilir][check]  | ![Kullanılabilir][check]  | ![Kullanılabilir][check]  |

Azure Işlevleri için yönetilen kimliği (varsa) yapılandırmak üzere aşağıdaki listeye bakın:

- [Azure portalındaki](/azure/app-service/overview-managed-identity#using-the-azure-portal)
- [Azure CLI](/azure/app-service/overview-managed-identity#using-the-azure-cli)
- [Azure PowerShell](/azure/app-service/overview-managed-identity#using-azure-powershell)
- [Azure Resource Manager şablonu](/azure/app-service/overview-managed-identity#using-an-azure-resource-manager-template)

### <a name="azure-iot-hub"></a>Azure IoT Hub

Yönetilen kimlik türü | Tüm genel kullanıma açık<br>Küresel Azure bölgeleri | Azure Kamu | Azure Almanya | Azure Çin 21Vianet |
| --- | :-: | :-: | :-: | :-: |
| Sistem atandı | ![Kullanılabilir][check] | ![Kullanılabilir][check] | Kullanılamaz | ![Kullanılabilir][check] |
| Kullanıcı atandı | Kullanılamaz | Kullanılamaz | Kullanılamaz | Kullanılamaz |

Azure Data Factory v2 için yönetilen kimliği yapılandırmak üzere aşağıdaki listeye bakın (varsa):

- [Azure portalındaki](../../iot-hub/virtual-network-support.md#turn-on-managed-identity-for-iot-hub)

### <a name="azure-importexport"></a>Azure İçeri/Dışarı Aktarma

Yönetilen kimlik türü | Tüm genel kullanıma açık<br>Küresel Azure bölgeleri | Azure Kamu | Azure Almanya | Azure Çin 21Vianet |
| --- | --- | --- | --- | --- |
| Sistem atandı | Azure Içeri aktarma verme hizmeti 'nin kullanılabildiği bölgede kullanılabilir | Önizleme | Kullanılabilir | Kullanılabilir |
| Kullanıcı atandı | Kullanılamaz | Kullanılamaz | Kullanılamaz | Kullanılamaz |

### <a name="azure-kubernetes-service-aks"></a>Azure Kubernetes Hizmeti (AKS)

| Yönetilen kimlik türü | Tüm genel kullanıma açık<br>Küresel Azure bölgeleri | Azure Kamu | Azure Almanya | Azure Çin 21Vianet |
| --- | :-: | :-: | :-: | :-: |
| Sistem atandı | ![Kullanılabilir][check] | ![Kullanılabilir][check] | Kullanılamaz | ![Kullanılabilir][check] | 
| Kullanıcı atandı | ![Kullanılabilir][check] | ![Kullanılabilir][check] | Kullanılamaz | ![Kullanılabilir][check] |


Daha fazla bilgi için bkz. [Azure Kubernetes hizmetinde Yönetilen kimlikler kullanma](https://docs.microsoft.com/azure/aks/use-managed-identity).


### <a name="azure-logic-apps"></a>Azure Logic Apps

Yönetilen kimlik türü | Tüm genel kullanıma açık<br>Küresel Azure bölgeleri | Azure Kamu | Azure Almanya | Azure Çin 21Vianet |
| --- | :-: | :-: | :-: | :-: |
| Sistem atandı | ![Kullanılabilir][check] | ![Kullanılabilir][check] | Kullanılamaz | ![Kullanılabilir][check] |
| Kullanıcı atandı | ![Kullanılabilir][check] | ![Kullanılabilir][check] | Kullanılamaz | ![Kullanılabilir][check] |


Azure Logic Apps yönetilen kimliği (varsa) yapılandırmak için aşağıdaki listeye bakın:

- [Azure portalındaki](/azure/logic-apps/create-managed-service-identity#enable-system-assigned-identity-in-azure-portal)
- [Azure Resource Manager şablonu](https://docs.microsoft.com/azure/logic-apps/logic-apps-azure-resource-manager-templates-overview)


### <a name="azure-policy"></a>Azure İlkesi

|Yönetilen kimlik türü | Tüm genel kullanıma açık<br>Küresel Azure bölgeleri | Azure Kamu | Azure Almanya | Azure Çin 21Vianet |
| --- | :-: | :-: | :-: | :-: |
| Sistem atandı | ![Kullanılabilir][check] | ![Kullanılabilir][check] | ![Kullanılabilir][check] | ![Kullanılabilir][check] |
| Kullanıcı atandı | Kullanılamaz | Kullanılamaz | Kullanılamaz | Kullanılamaz |

Azure Ilkesi için yönetilen kimliği (varsa) yapılandırmak üzere aşağıdaki listeye bakın:

- [Azure portalındaki](../../governance/policy/tutorials/create-and-manage.md#assign-a-policy)
- [PowerShell](../../governance/policy/how-to/remediate-resources.md#create-managed-identity-with-powershell)
- [Azure CLI](https://docs.microsoft.com/cli/azure/policy/assignment?view=azure-cli-latest#az-policy-assignment-create)
- [Azure Resource Manager şablonları](https://docs.microsoft.com/azure/templates/microsoft.authorization/policyassignments)
- [REST](https://docs.microsoft.com/rest/api/resources/policyassignments/create)


### <a name="azure-service-fabric"></a>Azure Service Fabric

[Service Fabric uygulamalar Için yönetilen kimlik](https://docs.microsoft.com/azure/service-fabric/concepts-managed-identity) önizleme aşamasındadır ve tüm bölgelerde kullanılabilir.

Yönetilen kimlik türü | Tüm genel kullanıma açık<br>Küresel Azure bölgeleri | Azure Kamu | Azure Almanya | Azure Çin 21Vianet |
| --- | :-: | :-: | :-: | :-: |
| Sistem atandı | ![Kullanılabilir][check] | Kullanılamaz | Kullanılamaz | Kullanılamıyor |
| Kullanıcı atandı | ![Kullanılabilir][check] | Kullanılamaz | Kullanılamaz |Kullanılamaz |

Tüm bölgelerde Azure Service Fabric uygulamaları için yönetilen kimliği yapılandırmak üzere aşağıdaki listeye bakın:

- [Azure Resource Manager şablonu](https://github.com/Azure-Samples/service-fabric-managed-identity/tree/anmenard-docs)

### <a name="azure-spring-cloud"></a>Azure Spring Cloud

| Yönetilen kimlik türü | Tüm genel kullanıma açık<br>Küresel Azure bölgeleri | Azure Kamu | Azure Almanya | Azure Çin 21Vianet |
| --- | :-: | :-: | :-: | :-: |
| Sistem atandı | ![Kullanılabilir][check] | Kullanılamaz | Kullanılamaz | Kullanılamaz | 
| Kullanıcı atandı | Kullanılamaz | Kullanılamaz | Kullanılamaz | Kullanılamaz |


Daha fazla bilgi için bkz. [Azure Spring Cloud uygulaması için sistem tarafından atanan yönetilen kimliği etkinleştirme](~/articles/spring-cloud/spring-cloud-howto-enable-system-assigned-managed-identity.md).


### <a name="azure-virtual-machine-scale-sets"></a>Azure Sanal Makine Ölçek Kümeleri

|Yönetilen kimlik türü | Tüm genel kullanıma açık<br>Küresel Azure bölgeleri | Azure Kamu | Azure Almanya | Azure Çin 21Vianet |
| --- | :-: | :-: | :-: | :-: |
| Sistem atandı | ![Kullanılabilir][check] | Önizleme | Önizleme | Önizleme |
| Kullanıcı atandı | ![Kullanılabilir][check] | Önizleme | Önizleme | Önizleme |

Azure sanal makine ölçek kümeleri (varsa) için yönetilen kimliği yapılandırmak üzere aşağıdaki listeye bakın:

- [Azure portalındaki](qs-configure-portal-windows-vm.md)
- [PowerShell](qs-configure-powershell-windows-vm.md)
- [Azure CLI](qs-configure-cli-windows-vm.md)
- [Azure Resource Manager şablonları](qs-configure-template-windows-vm.md)
- [REST](qs-configure-rest-vm.md)



### <a name="azure-virtual-machines"></a>Azure Sanal Makineler

| Yönetilen kimlik türü | Tüm genel kullanıma açık<br>Küresel Azure bölgeleri | Azure Kamu | Azure Almanya | Azure Çin 21Vianet |
| --- | :-: | :-: | :-: | :-: |
| Sistem atandı | ![Kullanılabilir][check] | ![Kullanılabilir][check] | Önizleme | Önizleme | 
| Kullanıcı atandı | ![Kullanılabilir][check] | ![Kullanılabilir][check] | Önizleme | Önizleme |

Azure sanal makineleri için yönetilen kimliği yapılandırmak üzere aşağıdaki listeye bakın (kullanılabilir yerlerde):

- [Azure portalındaki](qs-configure-portal-windows-vm.md)
- [PowerShell](qs-configure-powershell-windows-vm.md)
- [Azure CLI](qs-configure-cli-windows-vm.md)
- [Azure Resource Manager şablonları](qs-configure-template-windows-vm.md)
- [REST](qs-configure-rest-vm.md)


### <a name="azure-vm-image-builder"></a>Azure VM görüntü Oluşturucusu

| Yönetilen kimlik türü | Tüm genel kullanıma açık<br>Küresel Azure bölgeleri | Azure Kamu | Azure Almanya | Azure Çin 21Vianet |
| --- | :-: | :-: | :-: | :-: |
| Sistem atandı | Kullanılamaz | Kullanılamaz | Kullanılamaz | Kullanılamaz | 
| Kullanıcı atandı | [Desteklenen bölgelerde kullanılabilir](https://docs.microsoft.com/azure/virtual-machines/windows/image-builder-overview#regions) | Kullanılamaz | Kullanılamaz | Kullanılamaz |

Azure VM görüntü Oluşturucu için yönetilen kimliği yapılandırma hakkında bilgi edinmek için (varsa) bkz. [görüntü oluşturucuya genel bakış](https://docs.microsoft.com/azure/virtual-machines/windows/image-builder-overview#permissions).
### <a name="azure-signalr-service"></a>Azure SignalR Hizmeti

Yönetilen kimlik türü | Tüm genel kullanıma açık<br>Küresel Azure bölgeleri | Azure Kamu | Azure Almanya | Azure Çin 21Vianet |
| --- | :-: | :-: | :-: | :-: |
| Sistem atandı | Önizleme | Önizleme | Kullanılamaz | Önizleme |
| Kullanıcı atandı | Önizleme | Önizleme | Kullanılamaz | Önizleme |

Azure SignalR hizmeti için yönetilen kimliği (varsa) yapılandırmak üzere aşağıdaki listeye bakın:

- [Azure Resource Manager şablonu](../../azure-signalr/howto-use-managed-identity.md)

## <a name="azure-services-that-support-azure-ad-authentication"></a>Azure AD kimlik doğrulamasını destekleyen Azure hizmetleri

Aşağıdaki hizmetler Azure AD kimlik doğrulamasını destekler ve Azure kaynakları için Yönetilen kimlikler kullanan istemci hizmetleriyle test edilmiştir.

### <a name="azure-resource-manager"></a>Azure Resource Manager

Azure Resource Manager erişimi yapılandırmak için aşağıdaki listeye bakın:

- [Azure portal aracılığıyla erişim atama](howto-assign-access-portal.md)
- [PowerShell aracılığıyla erişim atama](howto-assign-access-powershell.md)
- [Azure CLı aracılığıyla erişim atama](howto-assign-access-CLI.md)
- [Azure Resource Manager şablonu aracılığıyla erişim atama](../../role-based-access-control/role-assignments-template.md)

| Bulut | Kaynak kimliği | Durum |
|--------|------------|:-:|
| Azure genel | `https://management.azure.com/`| ![Kullanılabilir][check] |
| Azure Kamu | `https://management.usgovcloudapi.net/` | ![Kullanılabilir][check] |
| Azure Almanya | `https://management.microsoftazure.de/` | ![Kullanılabilir][check] |
| Azure Çin 21Vianet | `https://management.chinacloudapi.cn` | ![Kullanılabilir][check] |

### <a name="azure-key-vault"></a>Azure Key Vault

| Bulut | Kaynak kimliği | Durum |
|--------|------------|:-:|
| Azure genel | `https://vault.azure.net`| ![Kullanılabilir][check] |
| Azure Kamu | `https://vault.usgovcloudapi.net` | ![Kullanılabilir][check] |
| Azure Almanya |  `https://vault.microsoftazure.de` | ![Kullanılabilir][check] |
| Azure Çin 21Vianet | `https://vault.azure.cn` | ![Kullanılabilir][check] |

### <a name="azure-data-lake"></a>Azure Data Lake

| Bulut | Kaynak kimliği | Durum |
|--------|------------|:-:|
| Azure genel | `https://datalake.azure.net/` | ![Kullanılabilir][check] |
| Azure Kamu |  | Kullanılamaz |
| Azure Almanya |   | Kullanılamaz |
| Azure Çin 21Vianet |  | Kullanılamaz |

### <a name="azure-sql"></a>Azure SQL

| Bulut | Kaynak kimliği | Durum |
|--------|------------|:-:|
| Azure genel | `https://database.windows.net/` | ![Kullanılabilir][check] |
| Azure Kamu | `https://database.usgovcloudapi.net/` | ![Kullanılabilir][check] |
| Azure Almanya | `https://database.cloudapi.de/` | ![Kullanılabilir][check] |
| Azure Çin 21Vianet | `https://database.chinacloudapi.cn/` | ![Kullanılabilir][check] |

### <a name="azure-event-hubs"></a>Azure Event Hubs

| Bulut | Kaynak kimliği | Durum |
|--------|------------|:-:|
| Azure genel | `https://eventhubs.azure.net` | ![Kullanılabilir][check] |
| Azure Kamu |  | Kullanılamaz |
| Azure Almanya |   | Kullanılamaz |
| Azure Çin 21Vianet |  | Kullanılamaz |

### <a name="azure-service-bus"></a>Azure Service Bus

| Bulut | Kaynak kimliği | Durum |
|--------|------------|:-:|
| Azure genel | `https://servicebus.azure.net`  | ![Kullanılabilir][check] |
| Azure Kamu |  | ![Kullanılabilir][check] |
| Azure Almanya |   | Kullanılamaz |
| Azure Çin 21Vianet |  | Kullanılamaz |









### <a name="azure-storage-blobs-and-queues"></a>Azure depolama Blobları ve kuyrukları

| Bulut | Kaynak kimliği | Durum |
|--------|------------|:-:|
| Azure genel | `https://storage.azure.com/` <br /><br />`https://<account>.blob.core.windows.net` <br /><br />`https://<account>.queue.core.windows.net` | ![Kullanılabilir][check] |
| Azure Kamu | `https://storage.azure.com/`<br /><br />`https://<account>.blob.core.usgovcloudapi.net` <br /><br />`https://<account>.queue.core.usgovcloudapi.net` | ![Kullanılabilir][check] |
| Azure Almanya | `https://storage.azure.com/`<br /><br />`https://<account>.blob.core.cloudapi.de` <br /><br />`https://<account>.queue.core.cloudapi.de` | ![Kullanılabilir][check] |
| Azure Çin 21Vianet | `https://storage.azure.com/`<br /><br />`https://<account>.blob.core.chinacloudapi.cn` <br /><br />`https://<account>.queue.core.chinacloudapi.cn` | ![Kullanılabilir][check] |

### <a name="azure-analysis-services"></a>Azure Analysis Services

| Bulut | Kaynak kimliği | Durum |
|--------|------------|:-:|
| Azure genel | `https://*.asazure.windows.net` | ![Kullanılabilir][check] |
| Azure Kamu | `https://*.asazure.usgovcloudapi.net` | ![Kullanılabilir][check] |
| Azure Almanya | `https://*.asazure.cloudapi.de` | ![Kullanılabilir][check] |
| Azure Çin 21Vianet | `https://*.asazure.chinacloudapi.cn` | ![Kullanılabilir][check] |

> [!Note]
> Microsoft Power BI, [yönetilen kimlikleri de destekler](https://docs.microsoft.com/azure/stream-analytics/powerbi-output-managed-identity).


[check]: media/services-support-managed-identities/check.png "Kullanılabileceğini"
