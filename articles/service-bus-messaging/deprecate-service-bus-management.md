---
title: Azure mesajlaşma hizmetleri - Servis Yöneticisi' den Kaynak Yöneticisine
description: Bu makalede, amortismana alınan Azure Servis Yöneticisi REST API & PowerShell cmdlets'in Kaynak Yöneticisi REST API & PowerShell cmdlets'e eşleneme leri sağlanır.
services: service-bus-messaging, event-hubs, event-grid
documentationcenter: na
author: spelluru
editor: ''
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/22/2020
ms.author: spelluru
ms.openlocfilehash: d263381667319b98a28ee6168e2de75c4041b58a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77589915"
---
# <a name="deprecation-of-azure-service-manager-support-for-azure-service-bus-relay-and-event-hubs"></a>Azure Hizmet Veri, Geçiş ve Etkinlik Hub'ları için Azure Servis Yöneticisi desteğinin amortismanı

Yeni nesil bulut altyapı yığınımız Resource Manager, "klasik" Azure Hizmet Yönetimi modelinin (klasik dağıtım modeli) tamamen yerini alıyor. Sonuç olarak, klasik dağıtım modeli REST API'leri ve Servis Veri, Geçiş ve Etkinlik Hub'ları desteği 1 Kasım 2021'de kullanımdan kaldırılacaktır. Bu amortisman ilk [bir Microsoft Tech Community duyuru](https://techcommunity.microsoft.com/t5/Service-Bus-blog/Deprecating-Service-Management-support-for-Azure-Service-Bus/ba-p/370909)açıklandı , ama biz son zamanlarda orijinal duyuru tarihinden itibaren iki yıl daha amortisman süresini uzatmak için karar verdik. Kolay tanımlama için, bu `management.core.windows.net` API'lerin URI'lerinde var. Şimdi kullanmanız gereken amortismana kıvranan API'lerin ve Azure Kaynak Yöneticisi API sürümünün listesi için aşağıdaki tabloya bakın.

Servis Veri, Geçiş ve Etkinlik Hub'larını kullanmaya devam etmek için 31 Ekim 2021'e kadar Kaynak Yöneticisi'ne geçin. Kaynak gruplandırma, etiketler, kolaylaştırılmış dağıtım ve yönetim süreci ve ince taneli erişim gibi Kaynak Yöneticisi'nin ek avantajlarından yararlanmak için yakında eski API'leri kullanmaya devam eden tüm müşterileri teşvik ediyoruz rol tabanlı erişim denetimi (RBAC) kullanarak kontrol.

Azure Kaynak Yöneticisi ve Azure Hizmet Yöneticisi hakkında daha fazla bilgi için [TechNet Blog'una](https://blogs.technet.microsoft.com/meamcs/2016/12/22/difference-between-azure-service-manager-and-azure-resource-manager/)bakın.

Azure Hizmet Veri, Geçiş ve Etkinlik Hub'ları için Servis Yöneticisi ve Kaynak Yöneticisi API'leri hakkında daha fazla bilgi için REST API belgelerimize bakın:

- [Azure Servis Veri Servisi](/rest/api/servicebus/)
- [Azure Event Hubs](/rest/api/eventhub/)
- [Azure Geçişi](/rest/api/relay/)

## <a name="service-manager-rest-api---resource-manager-rest-api"></a>Servis Yöneticisi REST API - Kaynak Yöneticisi REST API

| Servis Yöneticisi API'leri (Amortismana Uğradı) | Kaynak Yöneticisi - Servis Veri Servisi API | Kaynak Yöneticisi - Olay Merkezi API | Kaynak Yöneticisi - Röle API |
| --------------- | ----------------- | ----------------- | ----------------- | 
| **Namespaces-GetNamespaceAsync** <br/>[Servis Otobüsü Ad Alanı Alın](/rest/api/servicebus/get-namespace)<br/>[Etkinlik Hub'ı Ad Alanı Alın](/rest/api/eventhub/get-event-hub)<br/>[Röle Namespace alın](/rest/api/servicebus/get-relays)<br/> ```GET https://management.core.windows.net/{subscription ID}/services/ServiceBus/Namespaces/{namespace name}``` | [get](/rest/api/servicebus/namespaces/get) | [get](/rest/api/eventhub/namespaces/get) | [get](/rest/api/relay/namespaces/get) |
| **BağlantıDetayları-GetConnectionDetails**<br/>Servis Veri Servisi/Olay Hub'ı/Röle GetConnectionDetals<br/>```GET https://management.core.windows.net/{subscription ID}/services/ServiceBus/Namespaces/{namespace name}/ConnectionDetails``` | [listkeys](/rest/api/servicebus/namespaces/listkeys) | [listkeys](/rest/api/eventhub/namespaces/listkeys) | [listkeys](/rest/api/relay/namespaces/listkeys) |
| **Konular-GetTopicsAsync**<br/>Service Bus<br/>```GET https://management.core.windows.net/{subscription ID}/services/ServiceBus/Namespaces/{namespace name}/topics? $skip={skip}&$top={top}``` | [list](/rest/api/servicebus/topics/listbynamespace) | &nbsp; | &nbsp; | 
| **Kuyruklar-GetQueueAsync** <br/>Service Bus<br/>```GET https://management.core.windows.net/{subscription ID}/services/ServiceBus/Namespaces/{namespace name}/queues/{queueName}``` | [get](/rest/api/servicebus/queues/get) | &nbsp; | &nbsp; | 
| **Röleler-GetRelaysAsync**<br/>[Röleleri Alın](/rest/api/servicebus/get-relays)<br/>```GET https://management.core.windows.net/{subscription ID}/services/ServiceBus/Namespaces/{namespace name}/relays? $skip={skip}&$top={top}```| &nbsp; | &nbsp; | [list](/rest/api/relay/wcfrelays/listbynamespace) | 
| **NamespaceAuthorizationRules-GetNamespaceAuthorizationRuleAsync**<br/>Servis Veri Gönderisi/Etkinlik Hub/Röle GetNamespaceAuthRule<br/>```GET https://management.core.windows.net/{subscription ID}/services/ServiceBus/Namespaces/{namespace name}/authorizationrules?``` | [yetkilendirme kuralı](/rest/api/servicebus/namespaces/getauthorizationrule) |[yetkilendirme kuralı](/rest/api/eventhub/namespaces/getauthorizationrule) | [yetkilendirme kuralı](/rest/api/relay/namespaces/getauthorizationrule) |
| **Namespaces-DeleteNamespaceAsync**<br/>[Servis Veri Servisi İsim Alanını Silme](/rest/api/servicebus/delete-namespace)<br/>[Olay Hub'ları İsim Alanını Silme](/rest/api/eventhub/delete-event-hub)<br/>[Röleler İsim Alanını Sil](/rest/api/servicebus/delete-namespace)<br/> ```DELETE  https://management.core.windows.net/{subscription ID}/services/ServiceBus/Namespaces/{namespace name}``` | [Silmek](/rest/api/servicebus/namespaces/delete) | [Silmek](/rest/api/eventhub/namespaces/delete) | [Silmek](/rest/api/relay/namespaces/delete) | 
| **MesajlaşmaSKUPlan-GetPlanAsync**<br/>Servis Veri Servisi/Olay Hub'ı/Röle İsim Alanı Alın<br/>```GET https://management.core.windows.net/{subscription ID}/services/ServiceBus/Namespaces/{namespace name}/MessagingPlan``` | [get](/rest/api/servicebus/namespaces/get) | [get](/rest/api/eventhub/namespaces/get) | [get](/rest/api/relay/namespaces/get) |
| **MesajlaşmaSKUPlan-UpdatePlanAsync**<br/>Servis Veri Servisi/Olay Hub'ı/Röle İsim Alanı Alın<br/>```PUT https://management.core.windows.net/{subscription ID}/services/ServiceBus/Namespaces/{namespace name}/MessagingPlan``` | [createorupdate](/rest/api/servicebus/namespaces/createorupdate) | [createorupdate](/rest/api/eventhub/namespaces/createorupdate) | [createorupdate](/rest/api/relay/namespaces/createorupdate) |
| **NamespaceAuthorizationRules-UpdateNamespaceAuthorizationRuleAsync**<br/>Servis Veri Servisi/Olay Hub'ı/Röle İsim Alanı Alın<br/>```PUT https://management.core.windows.net/{subscription ID}/services/ServiceBus/Namespaces/{namespace name}/AuthorizationRules/{rule name}``` | [createorupdate](/rest/api/servicebus/namespaces/createorupdate) | [createorupdateauthorizationrule](/rest/api/eventhub/namespaces/createorupdateauthorizationrule) | [createorupdateauthorizationrule](/rest/api/relay/namespaces/createorupdateauthorizationrule) | 
| **NamespaceAuthorizationRules-CreateNamespaceAuthorizationRuleAsync**<br/> 
Servis Veri Otobüsü/Olay Merkezi/Röle<br/>```PUT https://management.core.windows.net/{subscription ID}/services/ServiceBus/Namespaces/{namespace name}/AuthorizationRules/{rule name}``` |[createorupdate](/rest/api/servicebus/namespaces/createorupdate) | [createorupdateauthorizationrule](/rest/api/eventhub/namespaces/createorupdateauthorizationrule) | [createorupdateauthorizationrule](/rest/api/relay/namespaces/createorupdateauthorizationrule) |
| **NamespaceProperties-GetNamespacePropertiesAsync**<br/>[Servis Otobüsü Ad Alanı Alın](/rest/api/servicebus/get-namespace)<br/>[Etkinlik Hub'ları Ad Alanı Alın](/rest/api/eventhub/get-event-hub)<br/>[Röle Namespace alın](/rest/api/servicebus/get-relays)<br/>```GET https://management.core.windows.net/{subscription ID}/services/ServiceBus/Namespaces/{namespace name}``` | [get](/rest/api/servicebus/namespaces/get) | [get](/rest/api/eventhub/namespaces/get) | [get](/rest/api/relay/namespaces/get) |
| **BölgeKodları-GetRegionCodesAsync**<br/>Servis Veri Servisi/EventHub/Röle İsim Alanı Alın<br/>```GET https://management.core.windows.net/{subscription ID}/services/ServiceBus/Namespaces/{namespace name}``` | [listbysku](/rest/api/servicebus/regions/listbysku) | [listbysku](/rest/api/eventhub/regions/listbysku) | &nbsp; | 
| **NamespaceProperties-UpdateNamespacePropertyAsync**<br/>Servis Veri Otobüsü/EventHub/Röle<br/>```GET    https://management.core.windows.net/{subscription ID}/services/ServiceBus/Regions/``` | [createorupdate](/rest/api/servicebus/namespaces/createorupdate) | [createorupdate](/rest/api/eventhub/namespaces/createorupdate) | [createorupdate](/rest/api/relay/namespaces/createorupdate) |
| **EventHubsCrud-ListEventHubsAsync**<br/>[Etkinlik Hub'larını Listele](/rest/api/eventhub/list-event-hubs)<br/>```GET https://management.core.windows.net/{subscription ID}/services/ServiceBus/Namespaces/{namespace name}/eventhubs?$skip={skip}&$top={top}``` | &nbsp; | [list](/rest/api/eventhub/eventhubs/listbynamespace) | &nbsp; | 
| **EventHubsCrud-GetEventHubAsync**<br/>[Etkinlik Hub'larını Alın](/rest/api/eventhub/get-event-hub)<br/>```GET https://management.core.windows.net/{subscription ID}/services/ServiceBus/Namespaces/{namespace name}/eventhubs/{eventHubPath}``` | &nbsp; | [get](/rest/api/eventhub/eventhubs/get) | &nbsp; | 
| **NamespaceAuthorizationRules-DeleteNamespaceAuthorizationRuleAsync**<br/>Servis Veri Otobüsü/Olay Merkezi/Röle<br/>```DELETE https://management.core.windows.net/{subscription ID}/services/ServiceBus/Namespaces/{namespace name}/AuthorizationRules/{rule name}``` | [yetkilendirme kuralını silme](/rest/api/servicebus/namespaces/deleteauthorizationrule) | [yetkilendirme kuralını silme](/rest/api/eventhub/namespaces/deleteauthorizationrule) | [yetkilendirme kuralını silme](/rest/api/relay/namespaces/deleteauthorizationrule) |
| **NamespaceAuthorizationRules-GetNamespaceAuthorizationRulesAsync**<br/>Servis Veri Otobüsü/EventHub/Röle<br/>```GET https://management.core.windows.net/{subscription ID}/services/ServiceBus/Namespaces/{namespace name}/AuthorizationRules``` | [listauthorization kuralları](/rest/api/servicebus/namespaces/listauthorizationrules) | [listauthorization kuralları](/rest/api/eventhub/namespaces/listauthorizationrules) | [listauthorization kuralları](/rest/api/relay/namespaces/listauthorizationrules) |
| **NamespaceAvailability-IsNamespaceKullanılabilir**<br/>[Servis Veri Günü Ad Alanı Kullanılabilirliği](/rest/api/servicebus/check-namespace-availability)<br/>```GET https://management.core.windows.net/{subscription ID}/services/ServiceBus/CheckNamespaceAvailability/?namespace=<namespaceValue>``` | [checknameavailability](/rest/api/servicebus/namespaces/checknameavailability) | [checknameavailability](/rest/api/eventhub/namespaces/checknameavailability) | [checknameavailability](/rest/api/relay/namespaces/checknameavailability) |
| **Namespaces-CreateOrUpdateNamespaceAsync**<br/>Servis Veri Otobüsü/Olay Merkezi/Röle<br/>```PUT https://management.core.windows.net/{subscription ID}/services/ServiceBus/Namespaces/{namespace name}``` | [createorupdate](/rest/api/servicebus/namespaces/createorupdate) | [createorupdate](/rest/api/eventhub/namespaces/createorupdate) | [createorupdate](/rest/api/relay/namespaces/createorupdate) | 
| **Konular-GetTopicAsync**<br/>```GET https://management.core.windows.net/{subscription ID}/services/ServiceBus/Namespaces/{namespace name}/topics/{topicPath}``` | [get](/rest/api/servicebus/topics/get) | &nbsp; | &nbsp; |

## <a name="service-manager-powershell---resource-manager-powershell"></a>Servis Yöneticisi PowerShell - Kaynak Yöneticisi PowerShell
| Servis Yöneticisi PowerShell komutu (Deprecated) | Yeni Kaynak Yöneticisi Komutları | Daha Yeni Kaynak Yöneticisi Komutu |
| ----- | ----- | ----- | 
| [Get-AzureSBAuthorizationRule](/powershell/module/servicemanagement/azure/get-azuresbauthorizationrule?view=azuresmps-4.0.0) | [Get-AzureRmServiceBusAuthorizationRule](/powershell/module/azurerm.servicebus/get-azurermservicebusauthorizationrule?view=azurermps-6.13.0) | [Get-AzServiceBusAuthorizationRule](/powershell/module/az.servicebus/get-azservicebusauthorizationrule?view=azps-1.6.0) |
| [Get-AzureSBLocation](/powershell/module/servicemanagement/azure/get-azuresblocation?view=azuresmps-4.0.0) | [Al-AzureRmServiceBusGeoDRConfiguration](/powershell/module/azurerm.servicebus/get-azurermservicebusgeodrconfiguration?view=azurermps-6.13.0) | [Get-AzServiceBusGeoDRConfiguration](/powershell/module/az.servicebus/get-azservicebusgeodrconfiguration?view=azps-1.6.0) |
| [Get-AzureSBNamespace](/powershell/module/servicemanagement/azure/get-azuresbnamespace?view=azuresmps-4.0.0) | [Get-AzureRmServiceBusNamespace](/powershell/module/azurerm.servicebus/get-azurermservicebusnamespace?view=azurermps-6.13.0) | [Get-AzServiceBusNamespace](/powershell/module/az.servicebus/get-azservicebusnamespace?view=azps-1.6.0) |
| [New-AzureSBAuthorizationRule](/powershell/module/servicemanagement/azure/new-azuresbauthorizationrule?view=azuresmps-4.0.0) | [New-AzureRmServiceBusAuthorizationRule](/powershell/module/azurerm.servicebus/new-azurermservicebusauthorizationrule?view=azurermps-6.13.0) | [Yeni-AzServiceBusAuthorizationRule](/powershell/module/az.servicebus/new-azservicebusauthorizationrule?view=azps-1.6.0) |
| [New-AzureSBNamespace](/powershell/module/servicemanagement/azure/new-azuresbnamespace?view=azuresmps-4.0.0) | [New-AzureRmServiceBusNamespace](/powershell/module/azurerm.servicebus/new-azurermservicebusnamespace?view=azurermps-6.13.0) | [Yeni-AzServiceBusNamespace](/powershell/module/az.servicebus/new-azservicebusnamespace?view=azps-1.6.0) |
| [Kaldır-AzureRmRelayAuthorizationRule](/powershell/module/azurerm.relay/remove-azurermrelayauthorizationrule?view=azurermps-6.13.0) | [Remove-AzureRmEventHubAuthorizationRule](/powershell/module/azurerm.eventhub/remove-azurermeventhubauthorizationrule?view=azurermps-6.13.0) | [Remove-AzServiceBusAuthorizationRule](/powershell/module/az.servicebus/remove-azservicebusauthorizationrule?view=azps-1.6.0) |
| [Remove-AzureSBNamespace](/powershell/module/servicemanagement/azure/remove-azuresbnamespace?view=azuresmps-4.0.0) | [Remove-AzureRmServiceBusNamespace](/powershell/module/azurerm.servicebus/remove-azurermservicebusnamespace?view=azurermps-6.13.0) | [Kaldır-AzServiceBusNamespace](/powershell/module/az.servicebus/remove-azservicebusnamespace?view=azps-1.6.0) |
| [Set-AzureSBAuthorizationRule](/powershell/module/servicemanagement/azure/set-azuresbauthorizationrule?view=azuresmps-4.0.0) | [Set-AzureRmServiceBusAuthorizationRule](/powershell/module/azurerm.servicebus/set-azurermservicebusauthorizationrule?view=azurermps-6.13.0) | [Set-AzServiceBusAuthorizationRule](/powershell/module/az.servicebus/set-azservicebusauthorizationrule?view=azps-1.6.0) |

## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki belgelere bakın: 

- En son REST API belgeleri
    - [Azure Servis Veri Servisi](/rest/api/servicebus/)
    - [Azure Event Hubs](/rest/api/eventhub/)
    - [Azure Geçişi](/rest/api/relay/)
- En son PowerShell belgeleri
    - [Azure Servis Veri Servisi](/powershell/module/azurerm.servicebus/?view=azurermps-6.13.0#service_bus)
    - [Azure Event Hubs](/powershell/module/azurerm.eventhub/?view=azurermps-6.13.0#event_hub)
    - [Azure Event Grid](/powershell/module/azurerm.eventgrid/?view=azurermps-6.13.0#event_grid)
