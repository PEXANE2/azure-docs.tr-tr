---
title: Kaynak türüne göre tamamlanmış mod silme Azure Resource Manager
description: Azure Resource Manager şablonlarda kaynak türlerinin tamamlanma modu silme işlemini nasıl işleyeceğini gösterir.
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: reference
ms.date: 04/24/2019
ms.author: tomfitz
ms.openlocfilehash: 5ac442c0ae1e397fd1e8b58fdbcf61eb8712046c
ms.sourcegitcommit: af58483a9c574a10edc546f2737939a93af87b73
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/17/2019
ms.locfileid: "68302850"
---
# <a name="deletion-of-azure-resources-for-complete-mode-deployments"></a>Tüm mod dağıtımları için Azure kaynaklarını silme
Bu makalede, kaynak türlerinin, tamamlanmış modda dağıtılan bir şablonda olmadığında silinme işleminin nasıl işleneceği açıklanır.

Türü, tamamlanmış modla dağıtılan `Yes` şablonda olmadığında, ile işaretlenen kaynak türleri silinir. 

İle `No` işaretlenen kaynak türleri, şablonda olmadığında otomatik olarak silinmez; ancak, üst kaynak silinirse bunlar silinir. Davranışın tam açıklaması için bkz. [Azure Resource Manager Dağıtım modları](deployment-modes.md).


## <a name="microsoftaad"></a>Microsoft.AAD

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | DomainServices | Evet | 
> | DomainServices/oucontainer | Hayır | 

## <a name="microsoftaadiam"></a>microsoft.aadiam

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | diagnosticSettings | Hayır | 
> | diagnosticSettingsCategories | Hayır | 

## <a name="microsoftaddons"></a>Microsoft. addons

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | Destek sağlayıcıları | Hayır | 

## <a name="microsoftadhybridhealthservice"></a>Microsoft.ADHybridHealthService

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | aadsupportcases | Hayır | 
> | addsservices | Hayır | 
> | Aracısını | Hayır | 
> | anonymousapiusers | Hayır | 
> | yapılandırma | Hayır | 
> | logs | Hayır | 
> | raporlar | Hayır | 
> | services | Hayır | 

## <a name="microsoftadvisor"></a>Microsoft.Advisor

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | konfigürasyonları | Hayır | 
> | Generatereyorumgeçişleri | Hayır | 
> | Önerileri | Hayır | 
> | gizlemeleri | Hayır | 

## <a name="microsoftalertsmanagement"></a>Microsoft.AlertsManagement

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | actionRules | Hayır | 
> | alerts | Hayır | 
> | alertsList | Hayır | 
> | alertsSummary | Hayır | 
> | alertsSummaryList | Hayır | 
> | smartDetectorAlertRules | Hayır | 
> | Smartdetectorruntimeortamortamları | Hayır | 
> | smartGroups | Hayır | 

## <a name="microsoftanalysisservices"></a>Microsoft.AnalysisServices

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | Larý | Evet | 

## <a name="microsoftapimanagement"></a>Microsoft.ApiManagement

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | reportFeedback | Hayır | 
> | hizmet | Evet | 
> | validateServiceName | Hayır | 

## <a name="microsoftattestation"></a>Microsoft. kanıtlama

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | attestationProviders | Hayır | 

## <a name="microsoftauthorization"></a>Microsoft. Authorization

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | classicAdministrators | Hayır | 
> | Denyasatamaları | Hayır | 
> | Erişimi yükseltme | Hayır | 
> | kaynaktaki | Hayır | 
> | izinler | Hayır | 
> | Poliyasatamaları | Hayır | 
> | policyDefinitions | Hayır | 
> | policySetDefinitions | Hayır | 
> | providerOperations | Hayır | 
> | roleAssignments | Hayır | 
> | roleDefinitions | Hayır | 

## <a name="microsoftautomation"></a>Microsoft. Automation

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | automationAccounts | Evet | 
> | automationAccounts/Configurations | Evet | 
> | automationAccounts/Jobs | Hayır | 
> | automationAccounts/runbook 'lar | Evet | 
> | automationAccounts/softwareUpdateConfigurations | Hayır | 
> | automationAccounts/Web kancaları | Hayır | 

## <a name="microsoftazuregeneva"></a>Microsoft. Azure. Genfiliz

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | lý | Hayır | 
> | ortamlar/hesaplar | Hayır | 
> | ortamlar/hesaplar/ad alanları | Hayır | 
> | ortamlar/hesaplar/ad alanları/yapılandırma | Hayır | 

## <a name="microsoftazureactivedirectory"></a>Microsoft.AzureActiveDirectory

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | b2cDirectories | Evet | 

## <a name="microsoftazurestack"></a>Microsoft.AzureStack

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | kayıtlarında | Evet | 
> | kayıt/müşteri abonelikleri | Hayır | 
> | kayıtlar/ürünler | Hayır | 

## <a name="microsoftbatch"></a>Microsoft.Batch

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | batchAccounts | Evet | 

## <a name="microsoftbilling"></a>Microsoft.Billing

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | billingAccounts | Hayır | 
> | billingAccounts/billingProfiles | Hayır | 
> | billingAccounts/billingProfiles/Billingabonelikleri | Hayır | 
> | billingAccounts/billingProfiles/faturalar | Hayır | 
> | billingAccounts/billingProfiles/faturalar/fiyat listesi | Hayır | 
> | billingAccounts/billingProfiles/operationStatus | Hayır | 
> | billingAccounts/billingProfiles/paymentMethods | Hayır | 
> | billingAccounts/billingProfiles/ilkeler | Hayır | 
> | billingAccounts/billingProfiles/fiyat listesi | Hayır | 
> | billingAccounts/billingProfiles/ürünler | Hayır | 
> | billingAccounts/billingProfiles/işlemler | Hayır | 
> | billingAccounts/Billingabonelikleri | Hayır | 
> | billingAccounts/departmanlar | Hayır | 
> | billingAccounts/Eligibletekliflere | Hayır | 
> | billingAccounts/KayıtSayısı | Hayır | 
> | billingAccounts/faturalar | Hayır | 
> | billingAccounts/ınvoicesections | Hayır | 
> | billingAccounts/ınvoicesections/Billingabonelikleri | Hayır | 
> | billingAccounts/ınvoicesections/Billingabonelikleri/aktarımı | Hayır | 
> | billingAccounts/ınvoicesections/ımportrequests | Hayır | 
> | billingAccounts/ınvoicesections/ınitiateımportrequest | Hayır | 
> | billingAccounts/ınvoicesections/ınitiatetransfer | Hayır | 
> | billingAccounts/ınvoicesections/operationStatus | Hayır | 
> | billingAccounts/Ürünler/Ürünler | Hayır | 
> | billingAccounts/ınvoicesections/aktarımlar | Hayır | 
> | billingAccounts/ürünler | Hayır | 
> | billingAccounts/projeler | Hayır | 
> | billingAccounts/projeler/Billingabonelikleri | Hayır | 
> | billingAccounts/projeler/ımportrequests | Hayır | 
> | billingAccounts/projeler/ınitiateımportrequest | Hayır | 
> | billingAccounts/projeler/operationStatus | Hayır | 
> | billingAccounts/projeler/ürünler | Hayır | 
> | billingAccounts/işlemler | Hayır | 
> | Billingdönemler | Hayır | 
> | BillingPermissions | Hayır | 
> | billingProperty | Hayır | 
> | Billingroleatamaları | Hayır | 
> | BillingRoleDefinitions | Hayır | 
> | Createbillingroleatama | Hayır | 
> | bölümlerinin | Hayır | 
> | kayıt sayısı | Hayır | 
> | ımportrequests | Hayır | 
> | ımportrequests/Acceptımportrequest | Hayır | 
> | ımportrequests/Declineımportrequest | Hayır | 
> | faturalardan | Hayır | 
> | girişinde | Hayır | 
> | aktarımlar/acceptTransfer | Hayır | 
> | aktarımlar/declineTransfer | Hayır | 
> | aktarımlar/operationStatus | Hayır | 
> | Usageplanlar | Hayır | 

## <a name="microsoftbingmaps"></a>Microsoft.BingMaps

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | Mapapsıs | Evet | 
> | updateCommunicationPreference | Hayır | 

## <a name="microsoftbiztalkservices"></a>Microsoft. BizTalkServices

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | BizTalk | Evet | 

## <a name="microsoftblueprint"></a>Microsoft. Blueprint

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | Şema tasmi | Hayır | 
> | Blueprintasbir/Atamaperations | Hayır | 
> | Blueprintasbir/işlemleri | Hayır | 
> | Blueprint | Hayır | 
> | planlar/yapıtlar | Hayır | 
> | planlar/sürümler | Hayır | 
> | planlar/sürümler/yapılar | Hayır | 

## <a name="microsoftbotservice"></a>Microsoft. BotService

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | botServices | Evet | 
> | botServices/kanallar | Hayır | 
> | botServices/Connections | Hayır | 

## <a name="microsoftcache"></a>Microsoft.Cache

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | Redis | Evet | 
> | RedisConfigDefinition | Hayır | 

## <a name="microsoftcapacity"></a>Microsoft. Capacity

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | appliedReservations | Hayır | 
> | calculatePrice | Hayır | 
> | larına | Hayır | 
> | Ticari Vaalrezervler | Hayır | 
> | Rezervler | Hayır | 
> | Rezervler/Hesaplaizterefund | Hayır | 
> | Rezervler/Birleştir | Hayır | 
> | Rezervler/rezervasyonlar | Hayır | 
> | Rezervler/rezervasyonlar/düzeltmeler | Hayır | 
> | Rezervler/geri dönüş | Hayır | 
> | Rezervler/Böl | Hayır | 
> | Rezervler/takas | Hayır | 
> | oluşturamaz | Hayır | 
> | Kaynakların | Hayır | 
> | validateReservationOrder | Hayır | 

## <a name="microsoftcdn"></a>Microsoft.Cdn

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | edgenodes | Hayır | 
> | profiles | Evet | 
> | Profiller/uç noktalar | Evet | 
> | Profiller/uç noktalar/customdomains | Hayır | 
> | Profiller/uç noktalar/kaynaklar | Hayır | 
> | Validatearaştırması | Hayır | 

## <a name="microsoftcertificateregistration"></a>Microsoft.CertificateRegistration

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | Sertifikadüzenleri | Evet | 
> | certificateOrders/Certificates | Hayır | 
> | Validatecertificateregistrationınformation | Hayır | 

## <a name="microsoftclassiccompute"></a>Microsoft.ClassicCompute

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | Yetenek | Hayır | 
> | domainNames | Hayır | 
> | domainNames/yetenekleri | Hayır | 
> | domainNames/internalLoadBalancers | Hayır | 
> | domainNames/serviceCertificates | Hayır | 
> | domainNames/Yuvaları | Hayır | 
> | domainNames/yuvalar/roller | Hayır | 
> | moveSubscriptionResources | Hayır | 
> | operatingSystemFamilies | Hayır | 
> | operatingSystems | Hayır | 
> | quotas | Hayır | 
> | resourceTypes | Hayır | 
> | Validatesubscriptionmoveavaılabılıty | Hayır | 
> | VirtualMachines | Hayır | 
> | virtualMachines/diagnosticSettings | Hayır | 

## <a name="microsoftclassicinfrastructuremigrate"></a>Microsoft. ClassicInfrastructureMigrate

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | classicInfrastructureResources | Hayır | 

## <a name="microsoftclassicnetwork"></a>Microsoft.ClassicNetwork

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | Yetenek | Hayır | 
> | expressRouteCrossConnections | Hayır | 
> | expressRouteCrossConnections/peerler | Hayır | 
> | gatewaySupportedDevices | Hayır | 
> | networkSecurityGroups | Hayır | 
> | quotas | Hayır | 
> | Rezervler | Hayır | 
> | virtualNetworks | Hayır | 
> | virtualNetworks/Remotevirtualnetworkpeeringproxy 'Leri | Hayır | 
> | virtualNetworks/Virtualnetworkpeerler | Hayır | 

## <a name="microsoftclassicstorage"></a>Microsoft.ClassicStorage

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | Yetenek | Hayır | 
> | Disklerinden | Hayır | 
> | images | Hayır | 
> | osImages | Hayır | 
> | Osplatformımages | Hayır | 
> | Publicımages | Hayır | 
> | quotas | Hayır | 
> | storageAccounts | Hayır | 
> | storageAccounts/Services | Hayır | 
> | storageAccounts/Services/diagnosticSettings | Hayır | 
> | storageAccounts/Vmımages | Hayır | 
> | Vmımages | Hayır | 

## <a name="microsoftcognitiveservices"></a>Microsoft.CognitiveServices

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | accounts | Evet | 

## <a name="microsoftcommerce"></a>Microsoft. Commerce

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | RateCard | Hayır | 
> | Usagetoplamaları | Hayır | 

## <a name="microsoftcompute"></a>Microsoft.Compute

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | availabilitySets | Evet | 
> | Disklerinden | Evet | 
> | images | Evet | 
> | restorePointCollections | Evet | 
> | restorePointCollections/restorePoints | Hayır | 
> | Sharedvmımages | Evet | 
> | Sharedvmımages/sürümler | Evet | 
> | görüntüsünü | Evet | 
> | VirtualMachines | Evet | 
> | virtualMachines/diagnosticSettings | Hayır | 
> | virtualMachines/uzantıları | Evet | 
> | virtualMachineScaleSets | Evet | 
> | virtualMachineScaleSets/uzantılar | Hayır | 
> | virtualMachineScaleSets/NetworkInterfaces | Hayır | 
> | virtualMachineScaleSets/Publicıpaddresses | Hayır | 
> | virtualMachineScaleSets/virtualMachines | Hayır | 
> | virtualMachineScaleSets/virtualMachines/NetworkInterfaces | Hayır | 

## <a name="microsoftconsumption"></a>Microsoft. tüketim

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | Aggregmalyt maliyeti | Hayır | 
> | Bakiyeler | Hayır | 
> | Bütçeler | Hayır | 
> | Ücretleriyle | Hayır | 
> | CostTags | Hayır | 
> | iler | Hayır | 
> | etkinlikler | Hayır | 
> | Tahminler | Hayır | 
> | oluş | Hayır | 
> | Marketlerinden | Hayır | 
> | Pricesheets | Hayır | 
> | ürün | Hayır | 
> | Rezervde ayrıntıları | Hayır | 
> | Rezervationönerilere | Hayır | 
> | Rezervlerin Özeti | Hayır | 
> | Rezervlik Işlemleri | Hayır | 
> | Tags | Hayır | 
> | Koşullar | Hayır | 
> | UsageDetails | Hayır | 

## <a name="microsoftcontainerinstance"></a>Microsoft.ContainerInstance

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | Kapsayıcı grupları | Evet | 
> | serviceAssociationLinks | Hayır | 

## <a name="microsoftcontainerregistry"></a>Microsoft.ContainerRegistry

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | kayıt | Evet | 
> | kayıt defterleri/derlemeler | Hayır | 
> | kayıt defterleri/derlemeler/iptal | Hayır | 
> | kayıt defterleri/derlemeler/getLogLink | Hayır | 
> | kayıt defterleri/buildTasks | Evet | 
> | kayıt defterleri/buildTasks/Steps | Hayır | 
> | kayıt defterleri/eventGridFilters | Hayır | 
> | kayıt defterleri/getBuildSourceUploadUrl 'Si | Hayır | 
> | kayıt defterleri/GetCredentials | Hayır | 
> | kayıt defterleri/ımportımage | Hayır | 
> | kayıt defterleri/queueBuild | Hayır | 
> | kayıt defterleri/regenerateCredential | Hayır | 
> | kayıt defterleri/regenerateCredentials | Hayır | 
> | kayıt defterleri/çoğaltmalar | Evet | 
> | kayıt defterleri/çalıştırmalar | Hayır | 
> | kayıt defterleri/çalıştırmalar/iptal | Hayır | 
> | kayıt defterleri/scheduleRun | Hayır | 
> | kayıt defterleri/görevler | Evet | 
> | kayıt defterleri/updatePolicies | Hayır | 
> | kayıt defterleri/Web kancaları | Evet | 
> | kayıt defterleri/Web kancaları/getCallbackConfig | Hayır | 
> | kayıt defterleri/Web kancaları/ping | Hayır | 

## <a name="microsoftcontainerservice"></a>Microsoft.ContainerService

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | containerServices | Evet | 
> | Managedkümeler | Evet | 

## <a name="microsoftcontentmoderator"></a>Microsoft.ContentModerator

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | uygulamalar | Evet | 
> | updateCommunicationPreference | Hayır | 

## <a name="microsoftcortanaanalytics"></a>Microsoft. Cortanaanalizi

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | accounts | Evet | 

## <a name="microsoftcostmanagement"></a>Microsoft. CostManagement

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | Uyarılar | Hayır | 
> | billingAccounts | Hayır | 
> | Bağlayıcılar | Evet | 
> | bölümlerinin | Hayır | 
> | Boyutlar | Hayır | 
> | kayıt sayısı | Hayır | 
> | Sorgu | Hayır | 
> | kaydolunamadı | Hayır | 
> | Reportconfigs | Hayır | 
> | Raporlar | Hayır | 

## <a name="microsoftcustomerinsights"></a>Microsoft.CustomerInsights

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | Lara | Evet | 
> | Hub/authorizationPolicies | Hayır | 
> | Hub 'lar/bağlayıcılar | Hayır | 
> | Hub 'lar/bağlayıcılar/eşlemeler | Hayır | 
> | Hub 'lar/etkileşimler | Hayır | 
> | Hub/KPI | Hayır | 
> | Hub 'lar/bağlantılar | Hayır | 
> | Hub 'lar/profiller | Hayır | 
> | Hub/Roleatamaları | Hayır | 
> | Hub 'lar/roller | Hayır | 
> | Hub 'lar/suggestTypeSchema | Hayır | 
> | Hub 'lar/görünümler | Hayır | 
> | Hub 'lar/widgetTypes | Hayır | 

## <a name="microsoftdatabox"></a>Microsoft. DataBox

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | Çizelge | Evet | 

## <a name="microsoftdataboxedge"></a>Microsoft. DataBoxEdge

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | DataBoxEdgeDevices | Evet | 

## <a name="microsoftdatabricks"></a>Microsoft. Databricks

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | Onlarla | Evet | 
> | çalışma alanları/Virtualnetworkpeerler | Hayır | 

## <a name="microsoftdatacatalog"></a>Microsoft.DataCatalog

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | larına | Evet | 

## <a name="microsoftdataconnect"></a>Microsoft. DataConnect

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | Connectionyöneticileri | Evet | 

## <a name="microsoftdatafactory"></a>Microsoft.DataFactory

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | Veri fabrikaları | Evet | 
> | DataFactory/diagnosticSettings | Hayır | 
> | dataFactorySchema | Hayır | 
> | larının | Evet | 
> | Fabrika/tümleştirme çalışma zamanları | Hayır | 

## <a name="microsoftdatalakeanalytics"></a>Microsoft.DataLakeAnalytics

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | accounts | Evet | 
> | hesaplar/dataLakeStoreAccounts | Hayır | 
> | hesaplar/storageAccounts | Hayır | 
> | hesaplar/storageAccounts/kapsayıcılar | Hayır | 

## <a name="microsoftdatalakestore"></a>Microsoft.DataLakeStore

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | accounts | Evet | 
> | hesaplar/eventGridFilters | Hayır | 
> | hesaplar/firewallRules | Hayır | 

## <a name="microsoftdatamigration"></a>Microsoft. DataMigration

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | services | Evet | 
> | Hizmetler/Projeler | Evet | 

## <a name="microsoftdbformariadb"></a>Microsoft. Dbformarıdb

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | Larý | Evet | 
> | sunucular/recoverableServers | Hayır | 
> | sunucular/virtualNetworkRules | Hayır | 

## <a name="microsoftdbformysql"></a>Microsoft.DBforMySQL

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | Larý | Evet | 
> | sunucular/recoverableServers | Hayır | 
> | sunucular/virtualNetworkRules | Hayır | 

## <a name="microsoftdbforpostgresql"></a>Microsoft.DBforPostgreSQL

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | Larý | Evet | 
> | sunucular/danışmanları | Hayır | 
> | sunucular/Querymetinmetinleri | Hayır | 
> | sunucular/recoverableServers | Hayır | 
> | sunucular/topQueryStatistics | Hayır | 
> | sunucular/virtualNetworkRules | Hayır | 
> | sunucular/waitStatistics | Hayır | 

## <a name="microsoftdevices"></a>Microsoft. Devices

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | Iothubs | Evet | 
> | IotHubs/eventGridFilters | Hayır | 
> | ProvisioningServices | Evet | 
> | vardır | Hayır | 

## <a name="microsoftdevspaces"></a>Microsoft. DevSpaces

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | Örleri | Evet | 

## <a name="microsoftdevtestlab"></a>Microsoft.DevTestLab

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | larda | Evet | 
> | Labs/Servicerunanlar | Evet | 
> | Labs/virtualMachines | Evet | 
> | cağını | Evet | 

## <a name="microsoftdocumentdb"></a>Microsoft.DocumentDB

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | databaseAccountNames | Hayır | 
> | Veritabanı hesapları | Evet | 

## <a name="microsoftdomainregistration"></a>Microsoft.DomainRegistration

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | etki alanları | Evet | 
> | Domains/Domainownershiptanýmlayýcýlarý | Hayır | 
> | generateSsoRequest | Hayır | 
> | topLevelDomains | Hayır | 
> | Validatedomainregistrationınformation | Hayır | 

## <a name="microsoftdynamicslcs"></a>Microsoft.DynamicsLcs

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | lcsprojects | Hayır | 
> | lcsprojects/clouddağıtımları | Hayır | 
> | lcsprojects/bağlayıcılar | Hayır | 

## <a name="microsofteventgrid"></a>Microsoft.EventGrid

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | etki alanları | Evet | 
> | etki alanları/konular | Hayır | 
> | Eventabonelikleri | Hayır | 
> | Extensionkonuları | Hayır | 
> | konularıyla | Evet | 
> | topicTypes | Hayır | 

## <a name="microsofteventhub"></a>Microsoft.EventHub

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | leriniz | Evet | 
> | Öznitelikleri | Evet | 
> | ad alanları/authorizationrules | Hayır | 
> | ad alanları/diskalrecoveryconfigs | Hayır | 
> | ad alanları/eventhubs | Hayır | 
> | ad alanları/eventhubs/authorizationrules | Hayır | 
> | ad alanları/eventhubs/consumergroups | Hayır | 

## <a name="microsoftfeatures"></a>Microsoft. Features

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | SaaS Uygulamaları Geliştirme | Hayır | 
> | sağlayıcıları | Hayır | 

## <a name="microsoftgallery"></a>Microsoft. Gallery

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | kaydedemez | Hayır | 
> | gallergıtems | Hayır | 
> | generateartifactaccessuri | Hayır | 
> | myarea | Hayır | 
> | myarea/alan | Hayır | 
> | myarea/alan/alan | Hayır | 
> | myareas/Areas/Areas/gallergıtems | Hayır | 
> | myareas/Areas/gallergıtems | Hayır | 
> | myarea/gallergıtems | Hayır | 
> | kaydolunamadı | Hayır | 
> | Kaynakların | Hayır | 
> | elde edilecek esourcesbyıd | Hayır | 

## <a name="microsoftguestconfiguration"></a>Microsoft. GuestConfiguration

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | Guestconfigurationatamaları | Hayır | 
> | Yazılımıdır | Hayır | 

## <a name="microsofthanaonazure"></a>Microsoft. HanaOnAzure

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | Hanaınstances | Evet | 

## <a name="microsofthdinsight"></a>Microsoft.HDInsight

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | leriniz | Evet | 
> | kümeler/uygulamalar | Hayır | 

## <a name="microsoftimportexport"></a>Microsoft.ImportExport

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | Çizelge | Evet | 

## <a name="microsoftinformationprotection"></a>Microsoft. ınformationprotection

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | labelGroups | Hayır | 
> | labelGroups/Etiketler | Hayır | 
> | labelGroups/Labels/koşullar | Hayır | 
> | labelGroups/Labels/alt etiketleri | Hayır | 
> | labelGroups/Labels/alt etiketleri/koşulları | Hayır | 

## <a name="microsoftinsights"></a>Microsoft. Insights

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | actiongroups | Evet | 
> | activityLogAlerts | Evet | 
> | alertrules | Evet | 
> | automatedExportSettings | Hayır | 
> | autoscalesettings | Evet | 
> | çizgisi | Hayır | 
> | calculatebaseline | Hayır | 
> | Bileşenleri | Evet | 
> | bileşenler/olaylar | Hayır | 
> | bileşenler/Pricingplanlar | Hayır | 
> | bileşenler/sorgu | Hayır | 
> | diagnosticSettings | Hayır | 
> | diagnosticSettingsCategories | Hayır | 
> | eventCategories | Hayır | 
> | eventTypes | Hayır | 
> | extendedDiagnosticSettings | Hayır | 
> | logDefinitions | Hayır | 
> | günlüğe kaydetme profilleri | Hayır | 
> | logs | Hayır | 
> | metricAlerts | Evet |
> | migrateToNewPricingModel | Hayır | 
> | Çalışma kitapları | Hayır | 
> | sorgu | Hayır | 
> | rollbackToLegacyPricingModel | Hayır | 
> | scheduledqueryrules | Evet | 
> | vmInsightsOnboardingStatuses | Hayır | 
> | Web testleri | Evet | 
> | Kitabı | Evet | 

## <a name="microsoftintune"></a>Microsoft. Intune

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | diagnosticSettings | Hayır | 
> | diagnosticSettingsCategories | Hayır | 

## <a name="microsoftiotcentral"></a>Microsoft. ıotcentral

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | Iotapps | Evet | 

## <a name="microsoftiotspaces"></a>Microsoft. ıotspaces

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | Graf | Evet | 

## <a name="microsoftkeyvault"></a>Microsoft.KeyVault

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | Silinkaults | Hayır | 
> | kasaları | Evet | 
> | kasa/erişim Ilkeleri | Hayır | 
> | kasa/gizlilikler | Hayır | 

## <a name="microsoftkusto"></a>Microsoft. kusto

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | leriniz | Evet | 
> | kümeler/veritabanları | Hayır | 
> | kümeler/veritabanları/veri bağlantıları | Hayır | 
> | kümeler/veritabanları/eventhubconnections | Hayır | 

## <a name="microsoftlabservices"></a>Microsoft. LabServices

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | labaccounts | Evet | 
> | kullanıcılar | Hayır | 

## <a name="microsoftlocationbasedservices"></a>Microsoft.LocationBasedServices

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | accounts | Evet | 

## <a name="microsoftlocationservices"></a>Microsoft. LocationServices

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | accounts | Evet | 

## <a name="microsoftloganalytics"></a>Microsoft. LogAnalytics

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | logs | Hayır | 

## <a name="microsoftlogic"></a>Microsoft.Logic

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | Tümleştirme hesapları | Evet | 
> | sürdürülen | Evet | 

## <a name="microsoftmachinelearning"></a>Microsoft.MachineLearning

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | Commitmentplanlar | Evet | 
> | Hizmetleri | Evet | 
> | Çalışma Alanları | Evet | 

## <a name="microsoftmachinelearningexperimentation"></a>Microsoft. MachineLearningExperimentation

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | accounts | Evet | 
> | hesaplar/çalışma alanları | Evet | 
> | hesaplar/çalışma alanları/projeler | Evet | 
> | teamAccounts | Evet | 
> | teamAccounts/çalışma alanları | Evet | 
> | teamAccounts/çalışma alanları/projeler | Evet | 

## <a name="microsoftmachinelearningmodelmanagement"></a>Microsoft.MachineLearningModelManagement

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | accounts | Evet | 

## <a name="microsoftmachinelearningservices"></a>Microsoft. MachineLearningServices
> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | Onlarla | Evet | 
> | çalışma alanları/hesaplar | Hayır | 

## <a name="microsoftmanagedidentity"></a>Microsoft. Managedıdentity

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | Lerinizde | Hayır | 
> | Userassignedıdentities | Evet | 

## <a name="microsoftmanagement"></a>Microsoft. Management

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | getEntities | Hayır | 
> | Yönetim grupları | Hayır | 
> | Kaynakların | Hayır | 
> | startTenantBackfill | Hayır | 
> | tenantBackfillStatus | Hayır | 

## <a name="microsoftmaps"></a>Microsoft. Maps

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | accounts | Evet | 
> | hesaplar/eventGridFilters | Hayır | 

## <a name="microsoftmarketplace"></a>Microsoft. Market

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | sunar | Hayır | 
> | offerTypes | Hayır | 
> | offerTypes/yayımcılar | Hayır | 
> | offerTypes/yayımcılar/teklifler | Hayır | 
> | offerTypes/yayımcılar/teklifler/planlar | Hayır | 
> | offerTypes/yayımcılar/teklifler/planlar/anlaşmalar | Hayır | 
> | offerTypes/yayımcılar/teklifler/planlar/configs | Hayır | 
> | offerTypes/yayımcılar/teklifler/planlar/configs/ımportımage | Hayır | 
> | privategallergıtems | Hayır | 
> | ürün | Hayır | 

## <a name="microsoftmarketplaceapps"></a>Microsoft. MarketplaceApps

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | classicDevServices | Evet | 
> | updateCommunicationPreference | Hayır | 

## <a name="microsoftmarketplaceordering"></a>Microsoft. Marketplacesıralaması

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | anlaşmalar | Hayır | 
> | offertypes | Hayır | 

## <a name="microsoftmedia"></a>Microsoft. Media

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | mediaservices | Evet | 
> | mediaservices/accountFilters | Hayır | 
> | mediaservices/varlıklar | Hayır | 
> | mediaservices/varlıklar/assetFilters | Hayır | 
> | mediaservices/contentKeyPolicies | Hayır | 
> | mediaservices/eventGridFilters | Hayır | 
> | mediaservices/liveEventOperations | Hayır | 
> | mediaservices/liveEvents | Evet | 
> | mediaservices/liveEvents/Liveçıktılar | Hayır | 
> | mediaservices/liveOutputOperations | Hayır | 
> | mediaservices/streamingEndpointOperations | Hayır | 
> | mediaservices/streamingEndpoints | Evet | 
> | mediaservices/Streamingkonumlandırıcı | Hayır | 
> | mediaservices/streamingPolicies | Hayır | 
> | mediaservices/dönüşümler | Hayır | 
> | mediaservices/dönüşümler/işler | Hayır | 

## <a name="microsoftmigrate"></a>Microsoft. Migrate

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | projeyle | Evet | 

## <a name="microsoftnetwork"></a>Microsoft.Network

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | Applicationgateway 'ler | Evet | 
> | applicationSecurityGroups | Evet | 
> | azureFirewallFqdnTags | Hayır | 
> | azureFirewalls | Evet | 
> | bgpServiceCommunities | Hayır | 
> | bağlantının | Evet | 
> | ddosCustomPolicies | Evet | 
> | Ddosprotectionplanlar | Evet | 
> | Dnsoperationdurumları | Hayır | 
> | dnszones | Evet | 
> | dnszones/A | Hayır | 
> | dnszones/AAAA | Hayır | 
> | dnszones/tümü | Hayır | 
> | dnszones/CAA | Hayır | 
> | dnszones/CNAME | Hayır | 
> | dnszones/MX | Hayır | 
> | dnszones/NS | Hayır | 
> | dnszones/PTR | Hayır | 
> | dnszones/kayıt kümeleri | Hayır | 
> | dnszones/SOA | Hayır | 
> | dnszones/SRV | Hayır | 
> | dnszones/TXT | Hayır | 
> | Expressroutedevreleri | Evet | 
> | expressRouteServiceProviders | Hayır | 
> | frontkapıların | Evet | 
> | frontdoorWebApplicationFirewallPolicies | Evet | 
> | getDnsResourceReference | Hayır | 
> | ınterfaceendpoints | Evet | 
> | ınternalnotify | Hayır | 
> | loadBalancers | Evet | 
> | Localnetworkgateway 'ler | Evet | 
> | Natgateway 'ler | Evet | 
> | Networkıntpolicies Ilkeleri | Evet | 
> | NetworkInterfaces | Evet | 
> | networkProfiles | Evet | 
> | networkSecurityGroups | Evet | 
> | networkWatchers | Evet | 
> | networkWatchers/Connectionmonitörleri | Evet | 
> | networkWatchers/uzunluler | Evet | 
> | networkWatchers/Pingkafesler | Evet | 
> | privateLinkServices | Evet | 
> | Publicıpaddresses | Evet | 
> | Publicıpöneklerini | Evet | 
> | routeFilters | Evet | 
> | routeTables | Evet | 
> | serviceEndpointPolicies | Evet | 
> | trafficManagerGeographicHierarchies | Hayır | 
> | trafficmanagerprofiles | Evet | 
> | trafficmanagerprofiles/heatMaps | Hayır | 
> | Virtualhub 'Lar | Evet | 
> | virtualNetworkGateways | Evet | 
> | virtualNetworks | Evet | 
> | virtualNetworkTaps | Evet | 
> | Virtualwan | Evet | 
> | Vpngateway 'ler | Evet | 
> | vpnSites | Evet | 
> | webApplicationFirewallPolicies | Evet | 

## <a name="microsoftnotificationhubs"></a>Microsoft.NotificationHubs

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | Öznitelikleri | Evet | 
> | ad alanları/Notificationhub 'Lar | Evet | 

## <a name="microsoftoperationalinsights"></a>Microsoft.OperationalInsights

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | cihazlar | Hayır | 
> | Bağlantı hedefleri | Hayır | 
> | Storageınsii configs | Hayır | 
> | Onlarla | Evet | 
> | çalışma alanları/veri kaynakları | Hayır | 
> | çalışma alanları/linkedServices | Hayır | 
> | çalışma alanları/sorgu | Hayır | 

## <a name="microsoftoperationsmanagement"></a>Microsoft.OperationsManagement

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | managementassociations | Hayır | 
> | managementconfigurations | Evet | 
> | çözümler | Evet | 
> | Görünümler | Evet | 

## <a name="microsoftpolicyinsights"></a>Microsoft. Poliyelei

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | Poliyevents | Hayır | 
> | policyStates | Hayır | 
> | policyTrackedResources | Hayır | 
> | düzeltmeler | Hayır | 

## <a name="microsoftportal"></a>Microsoft. Portal

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | konsolları | Hayır | 
> | Panoların | Evet | 
> | userSettings | Hayır | 

## <a name="microsoftpowerbi"></a>Microsoft. PowerBI

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | workspaceCollections | Evet | 

## <a name="microsoftpowerbidedicated"></a>Microsoft.PowerBIDedicated

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | kapasiteler | Evet | 

## <a name="microsoftprojectoxford"></a>Microsoft. ProjectOxford

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | accounts | Evet | 

## <a name="microsoftrecoveryservices"></a>Microsoft. RecoveryServices

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | Backupkorunabilir | Hayır | 
> | kasaları | Evet | 

## <a name="microsoftrelay"></a>Microsoft. Relay

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | Öznitelikleri | Evet | 
> | ad alanları/authorizationrules | Hayır | 
> | ad alanları/hybridconnections | Hayır | 
> | ad alanları/hybridconnections/authorizationrules | Hayır | 
> | ad alanları/wcfreyerleştiri | Hayır | 
> | ad alanları/wcfreyerleştirme/authorizationrules | Hayır | 

## <a name="microsoftresourcegraph"></a>Microsoft. ResourceGraph

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | Kaynakların | Hayır | 
> | subscriptionsStatus | Hayır | 

## <a name="microsoftresourcehealth"></a>Microsoft. ResourceHealth

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | Kullanılabilirlik durumları | Hayır | 
> | Childadvailabilitydurumlar | Hayır | 
> | childResources | Hayır | 
> | etkinlikler | Hayır | 
> | ımpactedresources | Hayır | 
> | Bildirimi | Hayır | 

## <a name="microsoftresources"></a>Microsoft. resources

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | dağıtımlar | Hayır | 
> | dağıtımlar/işlemler | Hayır | 
> | Köprü | Hayır | 
> | notifyResourceJobs | Hayır | 
> | sağlayıcıları | Hayır | 
> | resourceGroups | Hayır | 
> | Kaynakların | Hayır | 
> | Aboneliklerin | Hayır | 
> | Abonelikler/sağlayıcılar | Hayır | 
> | Abonelikler/resourceGroups | Hayır | 
> | Abonelikler/ResourceGroups/kaynaklar | Hayır | 
> | Abonelikler/kaynaklar | Hayır | 
> | Abonelikler/etiket adları | Hayır | 
> | Abonelikler/etiket adları/tagValues | Hayır | 
> | Kira | Hayır | 

## <a name="microsoftsaas"></a>Microsoft. SaaS

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | uygulamalar | Evet | 
> | saasresources | Hayır | 

## <a name="microsoftscheduler"></a>Microsoft. Scheduler

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | var | Evet | 
> | işlere | Evet | 

## <a name="microsoftsearch"></a>Microsoft.Search

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | resourceHealthMetadata | Hayır | 
> | searchServices | Evet | 

## <a name="microsoftsecurity"></a>Microsoft.Security

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | advancedThreatProtectionSettings | Hayır | 
> | alerts | Hayır | 
> | allowedConnections | Hayır | 
> | yaptı | Hayır | 
> | Applicationwhitedökümler | Hayır | 
> | Oto Provisioningsettings | Hayır | 
> | Uyumluluklarına | Hayır | 
> | dataCollectionAgents | Hayır | 
> | discoveredSecuritySolutions | Hayır | 
> | externalSecuritySolutions | Hayır | 
> | Informationprotectionpolicies | Hayır | 
> | Jağaccesspolicies | Hayır | 
> | İzlemesinin | Hayır | 
> | izleme/kötü amaçlı yazılımdan koruma | Hayır | 
> | izleme/temel | Hayır | 
> | izleme/düzeltme eki uygulama | Hayır | 
> | Elerindeki | Hayır | 
> | fiyatlandırmalar | Hayır | 
> | securityContacts | Hayır | 
> | securitySolutions | Hayır | 
> | securitySolutionsReferenceData | Hayır | 
> | securityStatus | Hayır | 
> | securityStatus/endpoints | Hayır | 
> | securityStatus/alt ağları | Hayır | 
> | securityStatus/virtualMachines | Hayır | 
> | Securitydurumlardan | Hayır | 
> | securityStatusesSummaries | Hayır | 
> | ayarlar | Hayır | 
> | Görevler | Hayır | 
> | anlatır | Hayır | 
> | çalışma alanı ayarları | Hayır | 

## <a name="microsoftsecuritygraph"></a>Microsoft. SecurityGraph

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | diagnosticSettings | Hayır | 
> | diagnosticSettingsCategories | Hayır | 

## <a name="microsoftservicebus"></a>Microsoft.ServiceBus

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | Öznitelikleri | Evet | 
> | ad alanları/authorizationrules | Hayır | 
> | ad alanları/diskalrecoveryconfigs | Hayır | 
> | ad alanları/eventgridfilters | Hayır | 
> | ad alanları/kuyruklar | Hayır | 
> | ad alanları/kuyruklar/authorizationrules | Hayır | 
> | ad alanları/konular | Hayır | 
> | ad alanları/konular/authorizationrules | Hayır | 
> | ad alanları/konular/abonelikler | Hayır | 
> | ad alanları/konular/abonelikler/kurallar | Hayır | 
> | premiumMessagingRegions | Hayır | 

## <a name="microsoftservicefabric"></a>Microsoft.ServiceFabric

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | leriniz | Evet | 
> | kümeler/uygulamalar | Hayır | 

## <a name="microsoftservicefabricmesh"></a>Microsoft. Servicefabrickafesi

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | uygulamalar | Evet | 
> | geçidinin | Evet | 
> | Mamak | Evet | 
> | kaynaklanır | Evet | 
> | volumes | Evet | 

## <a name="microsoftsignalrservice"></a>Microsoft. SignalRService

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | SignalR | Evet | 

## <a name="microsoftsolutions"></a>Microsoft. Solutions

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | applianceDefinitions | Evet | 
> | yaptı | Evet | 
> | applicationDefinitions | Evet | 
> | uygulamalar | Evet | 
> | Jistekleri | Evet | 

## <a name="microsoftsql"></a>Microsoft. SQL

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | ManagedInstances | Evet |
> | ManagedInstances/veritabanları | Evet |
> | ManagedInstances/veritabanları/backupShortTermRetentionPolicies | Hayır |
> | ManagedInstances/veritabanları/şemalar/tablolar/sütunlar/sensitivityLabels | Hayır |
> | ManagedInstances/veritabanları/ | Hayır |
> | ManagedInstances/veritabanları/ek | Hayır |
> | ManagedInstances/encryptionProtector | Hayır |
> | ManagedInstances/anahtarlar | Hayır |
> | ManagedInstances/Restokbledroppeddatabases/backupShortTermRetentionPolicies | Hayır |
> | ManagedInstances/ | Hayır |
> | Larý | Evet | 
> | sunucular/Yöneticiler | Hayır | 
> | sunucular/communicationLinks | Hayır | 
> | sunucular/veritabanları | Evet | 
> | sunucular/encryptionProtector | Hayır | 
> | sunucular/firewallRules | Hayır | 
> | sunucular/anahtarlar | Hayır | 
> | sunucular/Restokbledroppeddatabases | Hayır | 
> | Sunucu/hizmet hedefleri | Hayır | 
> | sunucular/tdeCertificates | Hayır | 


## <a name="microsoftsqlvirtualmachine"></a>Microsoft. SqlVirtualMachine

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | SqlVirtualMachineGroups | Evet | 
> | SqlVirtualMachineGroups/AvailabilityGroupListeners | Hayır | 
> | SqlVirtualMachines | Evet | 

## <a name="microsoftstorage"></a>Microsoft.Storage

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | storageAccounts | Evet | 
> | storageAccounts/blobServices | Hayır | 
> | storageAccounts/fileServices | Hayır | 
> | storageAccounts/queueServices | Hayır | 
> | storageAccounts/Services | Hayır | 
> | storageAccounts/tableServices | Hayır | 
> | vardır | Hayır | 

## <a name="microsoftstoragesync"></a>Microsoft.StorageSync

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | storageSyncServices | Evet | 
> | storageSyncServices/registeredServers | Hayır | 
> | storageSyncServices/syncGroups | Hayır | 
> | storageSyncServices/syncGroups/cloudEndpoints | Hayır | 
> | storageSyncServices/syncGroups/serverEndpoints | Hayır | 
> | storageSyncServices/iş akışları | Hayır | 

## <a name="microsoftstorsimple"></a>Microsoft.StorSimple

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | ilerinde | Evet | 

## <a name="microsoftstreamanalytics"></a>Microsoft.StreamAnalytics

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | streammingjobs | Evet | 
> | streamingjobs/diagnosticSettings | Hayır | 

## <a name="microsoftsubscription"></a>Microsoft. Subscription

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | CreateSubscription | Hayır | 
> | SubscriptionDefinitions | Hayır | 
> | SubscriptionOperations | Hayır | 

## <a name="microsoftsupport"></a>Microsoft. support

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | destek biletleri | Hayır | 

## <a name="microsoftterraformoss"></a>Microsoft. Teroyformoss

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | Providerkayıtları | Evet | 
> | Kaynakların | Evet | 

## <a name="microsofttimeseriesinsights"></a>Microsoft.TimeSeriesInsights

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | lý | Evet | 
> | ortamlar/accessPolicies | Hayır | 
> | ortamlar/EventSources | Evet | 
> | ortamlar/Referencedataset 'ler | Evet | 

## <a name="microsoftvisualstudio"></a>Microsoft. VisualStudio

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | account | Evet | 
> | Hesap/uzantı | Evet | 
> | hesap/proje | Evet | 

## <a name="microsoftweb"></a>Microsoft. Web

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | apiManagementAccounts | Hayır | 
> | apiManagementAccounts/Apiacl 'Ler | Hayır | 
> | apiManagementAccounts/API 'ler | Hayır | 
> | apiManagementAccounts/API/Apiacl 'Ler | Hayır | 
> | apiManagementAccounts/API/Connectionacl 'Ler | Hayır | 
> | apiManagementAccounts/API/bağlantı | Hayır | 
> | apiManagementAccounts/API/Connections/Connectionacl 'Ler | Hayır | 
> | apiManagementAccounts/API/localizedDefinitions | Hayır | 
> | apiManagementAccounts/Connectionacl 'Ler | Hayır | 
> | apiManagementAccounts/bağlantılar | Hayır | 
> | billingMeters | Hayır | 
> | Sertifika | Evet | 
> | Connectiongateway 'ler | Evet | 
> | bağlantının | Evet | 
> | Customapsıs | Evet | 
> | Silinmi siteleri | Hayır | 
> | işlevleri | Hayır | 
> | hostingEnvironments | Evet | 
> | hostingEnvironments/multiRolePools | Hayır | 
> | hostingEnvironments/multiRolePools/örnekler | Hayır | 
> | hostingEnvironments/workerPools | Hayır | 
> | hostingEnvironments/workerPools/örnekleri | Hayır | 
> | publishingUsers | Hayır | 
> | Önerileri | Hayır | 
> | resourceHealthMetadata | Hayır | 
> | zamanları | Hayır | 
> | serverFarms | Evet | 
> | Sunucu grupları/çalışanlar | Hayır | 
> | barındıra | Evet | 
> | Sites/Domainownershiptanýmlayýcýlarý | Hayır | 
> | siteler/hostNameBindings | Hayır | 
> | siteler/örnekler | Hayır | 
> | siteler/örnekler/uzantılar | Hayır | 
> | siteler/premieraddons | Evet | 
> | siteler/öneriler | Hayır | 
> | siteler/resourceHealthMetadata | Hayır | 
> | siteler/yuvalar | Evet | 
> | siteler/yuvalar/hostNameBindings | Hayır | 
> | siteler/yuvalar/örnekler | Hayır | 
> | siteler/yuvalar/örnekler/uzantılar | Hayır | 
> | sourceControls | Hayır | 
> | doğrulamalısınız | Hayır | 
> | verifyHostingEnvironmentVnet | Hayır | 

## <a name="microsoftwindowsdefenderatp"></a>Microsoft. Windowssavunma Deratp

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | diagnosticSettings | Hayır | 
> | diagnosticSettingsCategories | Hayır | 

## <a name="microsoftwindowsiot"></a>Microsoft. Windowsıot

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | DeviceServices | Evet | 

## <a name="microsoftworkloadmonitor"></a>Microsoft. WorkloadMonitor

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | Bileşenleri | Hayır | 
> | componentsSummary | Hayır | 
> | Izleme örnekleri | Hayır | 
> | Izleme ınstancessummary | Hayır | 
> | Monitörün | Hayır | 
> | notificationSettings | Hayır | 

## <a name="next-steps"></a>Sonraki adımlar

Aynı verileri bir virgülle ayrılmış değerler dosyası ile almak için, [Complete-Mode-Deletion. csv](https://github.com/tfitzmac/resource-capabilities/blob/master/complete-mode-deletion.csv)dosyasını indirin.