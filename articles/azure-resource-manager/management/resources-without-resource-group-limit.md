---
title: 800 olmayan kaynaklar sayısı sınırı
description: Bir kaynak grubunda 800 taneden fazla örneğe sahip olabilir Azure Kaynak türlerini listeler.
ms.topic: conceptual
ms.date: 11/07/2019
ms.openlocfilehash: c4f452a13c2059c02bf675ca4fe80243257183d5
ms.sourcegitcommit: 51ed913864f11e78a4a98599b55bbb036550d8a5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/04/2020
ms.locfileid: "75659330"
---
# <a name="resources-not-limited-to-800-instances-per-resource-group"></a>Kaynak grubu başına 800 örneğiyle sınırlı kaynak yok

Varsayılan olarak, her kaynak grubunda bir kaynak türünün en fazla 800 örneğini dağıtabilirsiniz. Ancak, bazı kaynak türleri 800 örnek sınırından muaf tutulur. Bu makalede, bir kaynak grubunda 800 ' den fazla örneğe sahip olan Azure Kaynak türleri listelenmektedir. Diğer tüm kaynak türleri 800 örnek ile sınırlıdır.

Bazı kaynak türleri için, 800 örnek sınırının kaldırılmasını sağlamak üzere desteğe başvurmanız gerekir. Bu kaynak türleri bu makalede belirtilmiştir.


## <a name="microsoftautomation"></a>Microsoft. Automation

* automationAccounts

## <a name="microsoftazurestack"></a>Microsoft.AzureStack

* kayıtlarında
* kayıt/müşteri abonelikleri
* kayıtlar/ürünler

## <a name="microsoftbotservice"></a>Microsoft. BotService

* botServices-varsayılan olarak 800 örnek ile sınırlıdır. Bu sınır, destek ile iletişim kurarak artırılabilir.

## <a name="microsoftcompute"></a>Microsoft.Compute

* diskler
* images
* anlık görüntüler
* virtualMachines

## <a name="microsoftcontainerinstance"></a>Microsoft.ContainerInstance

* Kapsayıcı grupları

## <a name="microsoftcontainerregistry"></a>Microsoft.ContainerRegistry

* kayıt defterleri/buildTasks
* kayıt defterleri/buildTasks/listSourceRepositoryProperties
* kayıt defterleri/buildTasks/Steps
* kayıt defterleri/buildTasks/Steps/listBuildArguments
* kayıt defterleri/eventGridFilters
* kayıt defterleri/çoğaltmalar
* kayıt defterleri/görevler
* kayıt defterleri/Web kancaları

## <a name="microsoftdbformariadb"></a>Microsoft. Dbformarıdb

* sunucular

## <a name="microsoftdbformysql"></a>Microsoft.DBforMySQL

* sunucular

## <a name="microsoftdbforpostgresql"></a>Microsoft.DBforPostgreSQL

* Sunucu grupları
* sunucular
* serversv2

## <a name="microsoftenterpriseknowledgegraph"></a>Microsoft. EnterpriseKnowledgeGraph

* services

## <a name="microsofteventhub"></a>Microsoft.EventHub

* leriniz
* ad alanları

## <a name="microsoftexperimentation"></a>Microsoft. deneme

* experimentWorkspaces

## <a name="microsoftguestconfiguration"></a>Microsoft. GuestConfiguration

* Configurationprofileatamalar
* Guestconfigurationatamaları
* yazılım
* softwareUpdateProfile
* softwareUpdates

## <a name="microsoftlogic"></a>Microsoft.Logic

* Tümleştirme hesapları
* iş akışları

## <a name="microsoftnetapp"></a>Microsoft. NetApp

* netAppAccounts
* netAppAccounts/Capacityhavuzları
* netAppAccounts/Capacityhavuzları/birimleri
* netAppAccounts/Capacityhavuzlar/Volumes/Mount hedefleri
* netAppAccounts/Capacityhavuzlar/birimler/anlık görüntüler

## <a name="microsoftnetwork"></a>Microsoft.Network

* applicationGatewayWebApplicationFirewallPolicies
* applicationSecurityGroups
* Savunma Konakları
* Ddosprotectionplanlar
* dnszones
* dnszones/A
* dnszones/AAAA
* dnszones/CAA
* dnszones/CNAME
* dnszones/MX
* dnszones/NS
* dnszones/PTR
* dnszones/SOA
* dnszones/SRV
* dnszones/TXT
* dnszones/tümü
* dnszones/kayıt kümeleri
* Networkıntpolicies Ilkeleri
* NetworkInterfaces
* privateDnsZones
* privateDnsZones/A
* privateDnsZones/AAAA
* privateDnsZones/CNAME
* privateDnsZones/MX
* privateDnsZones/PTR
* privateDnsZones/SOA
* privateDnsZones/SRV
* privateDnsZones/TXT
* privateDnsZones/tümü
* privateDnsZones/virtualNetworkLinks
* privateEndpoints
* privateLinkServices
* Publicıpaddresses, varsayılan olarak 800 örnek Ile sınırlıdır. Bu sınır, destek ile iletişim kurarak artırılabilir.
* serviceEndpointPolicies
* trafficmanagerprofiles
* virtualNetworkTaps

## <a name="microsoftportalsdk"></a>Microsoft. PortalSdk

* rootResources

## <a name="microsoftpowerbi"></a>Microsoft. PowerBI

* workspaceCollections, varsayılan olarak 800 örnek ile sınırlıdır. Bu sınır, destek ile iletişim kurarak artırılabilir.

## <a name="microsoftrelay"></a>Microsoft. Relay

* ad alanları

## <a name="microsoftscheduler"></a>Microsoft. Scheduler

* işlere

## <a name="microsoftservicebus"></a>Microsoft.ServiceBus

* ad alanları

## <a name="microsoftservicefabricmesh"></a>Microsoft. Servicefabrickafesi

* uygulama
* Kapsayıcı grupları
* geçidinin
* ağlar
* kaynaklanır
* volumes

## <a name="microsoftstorage"></a>Microsoft.Storage

* storageAccounts

## <a name="microsoftweb"></a>Microsoft. Web

* apiManagementAccounts/API 'ler
* siteler

## <a name="next-steps"></a>Sonraki adımlar

Tüm kotalar ve limitlerin listesi için bkz. [Azure aboneliği ve hizmet limitleri, Kotalar ve kısıtlamalar](../../azure-resource-manager/management/azure-subscription-service-limits.md).
