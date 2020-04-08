---
title: 800 sayım sınırı olmayan kaynaklar
description: Bir kaynak grubunda 800'den fazla örneği olabilecek Azure kaynak türlerini listeler.
ms.topic: conceptual
ms.date: 04/06/2020
ms.openlocfilehash: 8644bec1a68acebff18cf83d17acb014784dc964
ms.sourcegitcommit: 6397c1774a1358c79138976071989287f4a81a83
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/07/2020
ms.locfileid: "80804784"
---
# <a name="resources-not-limited-to-800-instances-per-resource-group"></a>Kaynak grubu başına 800 örnekle sınırlı olmayan kaynaklar

Varsayılan olarak, her kaynak grubunda en fazla 800 kaynak türü örneği dağıtabilirsiniz. Ancak, bazı kaynak türleri 800 örnek sınırından muaftır. Bu makalede, bir kaynak grubunda 800'den fazla örneği olabilecek Azure kaynak türleri listelenilmiştir. Diğer tüm kaynak türleri 800 örnekle sınırlıdır.

Bazı kaynak türleri için, 800 örnek sınırının kaldırılması için desteğe başvurmanız gerekir. Bu kaynak türleri bu makalede belirtilmiştir.


## <a name="microsoftautomation"></a>Microsoft.Automation

* otomasyonHesaplar

## <a name="microsoftazurestack"></a>Microsoft.AzureStack

* Kayıt
* kayıtlar/müşteriAbonelikleri
* kayıtlar/ürünler

## <a name="microsoftbotservice"></a>Microsoft.BotService

* botServices - Varsayılan olarak, 800 örnekleri ile sınırlıdır. Bu sınır desteğe başvurarak artırılabilir.

## <a name="microsoftcompute"></a>Microsoft.Compute

* Disk
* Galerileri
* galeriler/resimler
* galeriler/resimler/sürümler
* images
* anlık görüntüler
* virtualMachines

## <a name="microsoftcontainerinstance"></a>Microsoft.ContainerInstance

* konteynerGruplar

## <a name="microsoftcontainerregistry"></a>Microsoft.ContainerRegistry

* kayıt defterleri/buildTasks
* kayıt defterleri/buildTasks/listSourceRepositoryProperties
* kayıt defterleri/buildGörevler/adımlar
* kayıt defterleri/buildTasks/steps/listBuildArguments
* kayıt defterleri/eventGridFilters
* kayıt defterleri/çoğaltmalar
* kayıt defterleri/görevleri
* kayıt defterleri/webhooks

## <a name="microsoftdbformariadb"></a>Microsoft.DBforMariaDB

* Sunucu

## <a name="microsoftdbformysql"></a>Microsoft.DBforMySQL

* Sunucu

## <a name="microsoftdbforpostgresql"></a>Microsoft.DBforPostgreSQL

* Servergroups
* Sunucu
* sunucularv2
* singleServers

## <a name="microsoftdevtestlab"></a>Microsoft.DevTestLab

* zamanlamaları - Varsayılan olarak, 800 örnekleri ile sınırlıdır. Bu sınır desteğe başvurarak artırılabilir.

## <a name="microsoftenterpriseknowledgegraph"></a>Microsoft.EnterpriseKnowledgeGraph

* services

## <a name="microsofteventhub"></a>Microsoft.EventHub

* Küme
* Ad alanları

## <a name="microsoftexperimentation"></a>Microsoft.Deneme

* denemeÇalışma alanları

## <a name="microsoftguestconfiguration"></a>Microsoft.GuestConfiguration

* otomatikManagedVmConfigurationProfiles
* configurationProfileAtamaları
* guestConfigurationAssignments
* Yazılım
* softwareUpdateProfile
* softwareUpdates

## <a name="microsoftinsights"></a>microsoft.insights

* metricalerts

## <a name="microsoftlogic"></a>Microsoft.Logic

* entegrasyonHesapları
* Iş akışı

## <a name="microsoftnetapp"></a>Microsoft.NetApp

* netAppHesapları
* netAppAccounts/capacityPools
* netAppAccounts/capacityPools/volumes
* netAppAccounts/capacityPools/volumes/mountTargets
* netAppAccounts/capacityPools/volumes/snapshots

## <a name="microsoftnetwork"></a>Microsoft.Network

* uygulamaGatewayWebApplicationFirewallPolicies
* uygulamaGüvenlikGrupları
* burcuHosts
* ddosProtectionPlans
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
* dnszones/recordsets
* ağNiyet Politikaları
* ağArayüzler
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
* publicIPAddresses - Varsayılan olarak, 800 örnekle sınırlıdır. Bu sınır desteğe başvurarak artırılabilir.
* serviceEndpointPolitikalar
* trafik yöneticisi profilleri
* virtualNetworkTaps

## <a name="microsoftportalsdk"></a>Microsoft.PortalSdk

* rootResources

## <a name="microsoftpowerbi"></a>Microsoft.PowerBI

* workspaceCollections - Varsayılan olarak, 800 örnekle sınırlıdır. Bu sınır desteğe başvurarak artırılabilir.

## <a name="microsoftrelay"></a>Microsoft.Relay

* Ad alanları

## <a name="microsoftscheduler"></a>Microsoft.Scheduler

* iş koleksiyonları

## <a name="microsoftservicebus"></a>Microsoft.ServiceBus

* Ad alanları

## <a name="microsoftservicefabricmesh"></a>Microsoft.ServiceFabricMesh

* uygulamalar
* konteynerGruplar
* Ağ geçit -leri
* Ağlar
* Sır -larını
* volumes

## <a name="microsoftstorage"></a>Microsoft.Storage

* depolamaHesapları

## <a name="microsoftweb"></a>Microsoft.Web

* apiManagementAccounts/apis
* Siteler

## <a name="next-steps"></a>Sonraki adımlar

Kotaların ve sınırların tam listesi için [Azure abonelik ve hizmet sınırları, kotalar ve kısıtlamalarbölümüne](azure-subscription-service-limits.md)bakın.
