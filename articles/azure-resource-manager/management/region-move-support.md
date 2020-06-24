---
title: Bölgeler arasında Azure kaynaklarını taşıma desteği
description: Azure bölgeleri arasında taşınabilecek Azure Kaynak türlerini listeler
author: rayne-wiselman
ms.service: azure-resource-manager
ms.topic: reference
ms.date: 05/31/2020
ms.author: raynew
ms.openlocfilehash: 71c5c5881ab34dd9bc1a4da9e097b46ec21dcea1
ms.sourcegitcommit: 398fecceba133d90aa8f6f1f2af58899f613d1e3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/21/2020
ms.locfileid: "85124230"
---
# <a name="support-for-moving-azure-resources-across-regions"></a>Bölgeler arasında Azure kaynaklarını taşıma desteği

Bu makalede, bir Azure Kaynak türünün başka bir Azure bölgesine geçiş için desteklenip desteklenmediğini onaylar. 

Kaynak sağlayıcısı ad alanına atlayın:
> [!div class="op_single_selector"]
> - [Microsoft. AAD](#microsoftaad)
> - [Microsoft. aadihar](#microsoftaadiam)
> - [Microsoft. AlertsManagement](#microsoftalertsmanagement)
> - [Microsoft. AnalysisServices](#microsoftanalysisservices)
> - [Microsoft. Apimanane](#microsoftapimanagement)
> - [Microsoft. AppConfiguration](#microsoftappconfiguration)
> - [Microsoft. AppService](#microsoftappservice)
> - [Microsoft.Authorization](#microsoftauthorization)
> - [Microsoft. Automation](#microsoftautomation)
> - [Microsoft. AzureActiveDirectory](#microsoftazureactivedirectory)
> - [Microsoft. AzureData](#microsoftazuredata)
> - [Microsoft. AzureStack](#microsoftazurestack)
> - [Microsoft.Batch](#microsoftbatch)
> - [ChAIMicrosoft.Bat](#microsoftbatchai)
> - [Microsoft. BingMaps](#microsoftbingmaps)
> - [Microsoft. BizTalkServices](#microsoftbiztalkservices)
> - [Microsoft. Blockzinciri](#microsoftblockchain)
> - [Microsoft. Blueprint](#microsoftblueprint)
> - [Microsoft. BotService](#microsoftbotservice)
> - [Microsoft. Cache](#microsoftcache)
> - [Microsoft. CDN](#microsoftcdn)
> - [Microsoft. CertificateRegistration](#microsoftcertificateregistration)
> - [Microsoft.ClassicCompute](#microsoftclassiccompute)
> - [Microsoft. ClassicNetwork](#microsoftclassicnetwork)
> - [Microsoft. ClassicStorage](#microsoftclassicstorage)
> - [Microsoft. Biliveservices](#microsoftcognitiveservices)
> - [Microsoft.Compute](#microsoftcompute)
> - [Microsoft. Container](#microsoftcontainer)
> - [Microsoft. Containerınstance](#microsoftcontainerinstance)
> - [Microsoft. ContainerRegistry](#microsoftcontainerregistry)
> - [Microsoft. ContainerService](#microsoftcontainerservice)
> - [Microsoft. Contentmoderatör](#microsoftcontentmoderator)
> - [Microsoft. Cortanaanalizi](#microsoftcortanaanalytics)
> - [Microsoft. CostManagement](#microsoftcostmanagement)
> - [Microsoft. Customerınsights](#microsoftcustomerinsights)
> - [Microsoft. CustomProviders](#microsoftcustomproviders)
> - [Microsoft. DataBox](#microsoftdatabox)
> - [Microsoft. DataBoxEdge](#microsoftdataboxedge)
> - [Microsoft. Databricks](#microsoftdatabricks)
> - [Microsoft. DataCatalog](#microsoftdatacatalog)
> - [Microsoft. DataConnect](#microsoftdataconnect)
> - [Microsoft. DataExchange](#microsoftdataexchange)
> - [Microsoft. DataFactory](#microsoftdatafactory)
> - [Microsoft. DataLake](#microsoftdatalake)
> - [Microsoft. DataLakeAnalytics](#microsoftdatalakeanalytics)
> - [Microsoft. DataLakeStore](#microsoftdatalakestore)
> - [Microsoft. DataMigration](#microsoftdatamigration)
> - [Microsoft. DataShare](#microsoftdatashare)
> - [Microsoft. Dbformarıdb](#microsoftdbformariadb)
> - [Microsoft. Dbformyısql](#microsoftdbformysql)
> - [Microsoft. DBforPostgreSQL](#microsoftdbforpostgresql)
> - [Microsoft. DeploymentManager](#microsoftdeploymentmanager)
> - [Microsoft. Devices](#microsoftdevices)
> - [Microsoft. DevSpaces](#microsoftdevspaces)
> - [Microsoft. DevTestLab](#microsoftdevtestlab)
> - [Microsoft.DocumentDB](#microsoftdocumentdb)
> - [Microsoft. DomainRegistration](#microsoftdomainregistration)
> - [Microsoft. EnterpriseKnowledgeGraph](#microsoftenterpriseknowledgegraph)
> - [Microsoft. EventGrid](#microsofteventgrid)
> - [Microsoft. EventHub](#microsofteventhub)
> - [Microsoft. Genomiks](#microsoftgenomics)
> - [Microsoft. HanaOnAzure](#microsofthanaonazure)
> - [Microsoft. HDInsight](#microsofthdinsight)
> - [Microsoft. Healthgelişme API 'leri](#microsofthealthcareapis)
> - [Microsoft. HybridCompute](#microsofthybridcompute)
> - [Microsoft. HybridData](#microsofthybriddata)
> - [Microsoft. ımportexport](#microsoftimportexport)
> - [Microsoft. Insights](#microsoftinsights)
> - [Microsoft. ıotcentral](#microsoftiotcentral)
> - [Microsoft. ıotspaces](#microsoftiotspaces)
> - [Microsoft. Keykasası](#microsoftkeyvault)
> - [Microsoft.Kusto](#microsoftkusto)
> - [Microsoft. LabServices](#microsoftlabservices)
> - [Microsoft. LocationBasedServices](#microsoftlocationbasedservices)
> - [Microsoft. LocationServices](#microsoftlocationservices)
> - [Microsoft. Logic](#microsoftlogic)
> - [Microsoft. Machinöğrenim](#microsoftmachinelearning)
> - [Microsoft. MachineLearningCompute](#microsoftmachinelearningcompute)
> - [Microsoft. MachineLearningExperimentation](#microsoftmachinelearningexperimentation)
> - [Microsoft. MachineLearningModelManagement](#microsoftmachinelearningmodelmanagement)
> - [Microsoft. MachineLearningOperationalization](#microsoftmachinelearningoperationalization)
> - [Microsoft.MachineLearningServices](#microsoftmachinelearningservices)
> - [Microsoft. Managedıdentity](#microsoftmanagedidentity)
> - [Microsoft. Maps](#microsoftmaps)
> - [Microsoft. MarketplaceApps](#microsoftmarketplaceapps)
> - [Microsoft. Media](#microsoftmedia)
> - [Microsoft. Microservices4Spring](#microsoftmicroservices4spring)
> - [Microsoft. Migrate](#microsoftmigrate)
> - [Microsoft. NetApp](#microsoftnetapp)
> - [Microsoft.Network](#microsoftnetwork)
> - [Microsoft. Notificationhub 'Lar](#microsoftnotificationhubs)
> - [Microsoft. Operationalınsights](#microsoftoperationalinsights)
> - [Microsoft. OperationsManagement](#microsoftoperationsmanagement)
> - [Microsoft. eşleme](#microsoftpeering)
> - [Microsoft. Portal](#microsoftportal)
> - [Microsoft. PortalSdk](#microsoftportalsdk)
> - [Microsoft. PowerBI](#microsoftpowerbi)
> - [Microsoft. Powerbiadanmış](#microsoftpowerbidedicated)
> - [Microsoft. ProjectOxford](#microsoftprojectoxford)
> - [Microsoft. RecoveryServices](#microsoftrecoveryservices)
> - [Microsoft. Relay](#microsoftrelay)
> - [Microsoft. ResourceGraph](#microsoftresourcegraph)
> - [Microsoft. SaaS](#microsoftsaas)
> - [Microsoft. Scheduler](#microsoftscheduler)
> - [Microsoft. Search](#microsoftsearch)
> - [Microsoft.Security](#microsoftsecurity)
> - [Microsoft. ServerManagement](#microsoftservermanagement)
> - [Microsoft.ServiceBus](#microsoftservicebus)
> - [Microsoft. ServiceFabric](#microsoftservicefabric)
> - [Microsoft. Servicefabrickafesi](#microsoftservicefabricmesh)
> - [Microsoft. SignalRService](#microsoftsignalrservice)
> - [Microsoft. Solutions](#microsoftsolutions)
> - [Microsoft.Sql](#microsoftsql)
> - [Microsoft. SqlVirtualMachine](#microsoftsqlvirtualmachine)
> - [Microsoft. SqlVM](#microsoftsqlvm)
> - [Microsoft.Storage](#microsoftstorage)
> - [Microsoft. StorageCache](#microsoftstoragecache)
> - [Microsoft. Storagessync](#microsoftstoragesync)
> - [Microsoft. StorageSyncDev](#microsoftstoragesyncdev)
> - [Microsoft. Storagesyncınt](#microsoftstoragesyncint)
> - [Microsoft. StorSimple](#microsoftstorsimple)
> - [Microsoft. StreamAnalytics](#microsoftstreamanalytics)
> - [Microsoft. StreamAnalyticsExplorer](#microsoftstreamanalyticsexplorer)
> - [Microsoft. Teroyformoss](#microsoftterraformoss)
> - [Microsoft. Timeseriesınsights](#microsofttimeseriesinsights)
> - [Microsoft. Token](#microsofttoken)
> - [Microsoft. Virtualmachineımages](#microsoftvirtualmachineimages)
> - [Microsoft. VisualStudio](#microsoftvisualstudio)
> - [Microsoft. Vmwarechoparlör basit](#microsoftvmwarecloudsimple)
> - [Microsoft. Web](#microsoftweb)
> - [Microsoft. Windowsıot](#microsoftwindowsiot)
> - [Microsoft. WindowsVirtualDesktop](#microsoftwindowsvirtualdesktop)

## <a name="microsoftaad"></a>Microsoft. AAD

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- | 
> | DomainServices | No | 
> | DomainServices/replicasets | No | 

## <a name="microsoftaadiam"></a>Microsoft. aadihar

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | Kira | No |

## <a name="microsoftalertsmanagement"></a>Microsoft. AlertsManagement

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | actionrules | No | 

## <a name="microsoftanalysisservices"></a>Microsoft. AnalysisServices

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | larý | No |

## <a name="microsoftapimanagement"></a>Microsoft. Apimanane

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | hizmet |  Evet (şablonu kullanarak) <br/><br/> [API Management bölgeler arasında taşıyın](../../api-management/api-management-howto-migrate.md). | 

## <a name="microsoftappconfiguration"></a>Microsoft. AppConfiguration

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | configurationmağazaların | No | 

## <a name="microsoftappservice"></a>Microsoft. AppService

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | apiapps | Evet (şablonu kullanarak)<br/><br/> [App Service uygulamasını başka bir bölgeye taşıma](../../app-service/manage-move-across-regions.md) | 
> | appdentities | No | 
> | geçidinin | No | 


## <a name="microsoftauthorization"></a>Microsoft.Authorization

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | poliyasatamaları | No |

## <a name="microsoftautomation"></a>Microsoft. Automation

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | automationaccounts | Evet (şablonu kullanarak) <br/><br/> [Coğrafi çoğaltma kullanma](../../automation/automation-managing-data.md#geo-replication-in-azure-automation) |  
> | automationaccounts/Configurations | No | 
> | automationaccounts/runbook 'lar | No | 



## <a name="microsoftazureactivedirectory"></a>Microsoft. AzureActiveDirectory

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | b2cdirectories | No | 

## <a name="microsoftazuredata"></a>Microsoft. AzureData

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | sqlserverkayıtları | No |

## <a name="microsoftazurestack"></a>Microsoft. AzureStack

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | kayıtlarında | No | 

## <a name="microsoftbatch"></a>Microsoft.Batch

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | batchaccounts |  Batch hesapları doğrudan bir bölgeden diğerine taşınamaz, ancak şablonu dışarı aktarmak, değiştirmek ve şablonu yeni bölgeye dağıtmak için bir şablon kullanabilirsiniz. <br/><br/> [Bir Batch hesabını bölgeler arasında taşıma](../../batch/best-practices.md#moving-batch-accounts-across-regions) hakkında bilgi edinin |

## <a name="microsoftbatchai"></a>ChAI Microsoft.Bat

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | leriniz | No <br/><br/> Azure Batch AI hizmeti [kullanımdan kaldırıldı](https://docs.microsoft.com/previous-versions/azure/batch-ai/overview-what-happened-batch-ai).
> | dosya sunucuları | No | 
> | Çizelge | No | 
> | çalışma alanı | No | 

## <a name="microsoftbingmaps"></a>Microsoft. BingMaps

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | mapapsıs | No | 

## <a name="microsoftbiztalkservices"></a>Microsoft. BizTalkServices

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | biztalk | No | 

## <a name="microsoftblockchain"></a>Microsoft. Blockzinciri

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | blockchainmembers | No <br/><br/> Blok zinciri ağı farklı bölgelerde düğümlere sahip olamaz. 
> | izleyicileri | No | 

## <a name="microsoftblueprint"></a>Microsoft. Blueprint

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | şema tasmi | No | 

## <a name="microsoftbotservice"></a>Microsoft. BotService

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | botservices | No | 

## <a name="microsoftcache"></a>Microsoft. Cache

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | Redis | No | 


## <a name="microsoftcdn"></a>Microsoft. CDN

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | cdnwebapplicationfirewallpolicies | No |
> | lerinize | No | 
> | Profiller/uç noktalar | No | 

## <a name="microsoftcertificateregistration"></a>Microsoft. CertificateRegistration

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | sertifikadüzenleri | No | 


## <a name="microsoftclassiccompute"></a>Microsoft.ClassicCompute

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | DomainNames | Klasik hizmetler için hiçbir iş planlanmadı.
> | virtualmachines | No | 



## <a name="microsoftclassicnetwork"></a>Microsoft. ClassicNetwork

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | networksecuritygroups | Klasik hizmetler için hiçbir iş planlanmadı.
> | rezervler | No | 
> | virtualnetworks | No | 

## <a name="microsoftclassicstorage"></a>Microsoft. ClassicStorage

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | storageaccounts | Yes |  


## <a name="microsoftcognitiveservices"></a>Microsoft. Biliveservices

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | accounts | No | 
> | Bilişsel Arama | El ile yapılan adımlarla desteklenir.<br/><br/> [Azure bilişsel arama hizmetinizi başka bir bölgeye taşıma](../../search/search-howto-move-across-regions.md) hakkında bilgi edinin

## <a name="microsoftcompute"></a>Microsoft.Compute

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | availabilitysets | No | 
> | diskencryptionsets | No | 
> | disklerinden | No | 
> | Galeriler | No | 
> | Galeriler/görüntüler | No | 
> | Galeriler/resimler/sürümler | No | 
> | hostgroups | No | 
> | hostgroups/konaklar | No | 
> | images | No | 
> | proximityplacementgroups | No | 
> | restorepointcollections | No | 
> | sharedvmımages | No | 
> | sharedvmımages/sürümler | No | 
> | anlık görüntüler | No | 
> | virtualmachines | Yes | 
> | virtualmachines/uzantıları | No | 
> | virtualmachinescalesets | No | 

## <a name="microsoftcontainer"></a>Microsoft. Container

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | kapsayıcı grupları | No | 

## <a name="microsoftcontainerinstance"></a>Microsoft. Containerınstance

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | kapsayıcı grupları | No | 

## <a name="microsoftcontainerregistry"></a>Microsoft. ContainerRegistry

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | kayıt | No |  
> | kayıt defterleri/BuildTasks | No |  
> | kayıt defterleri/çoğaltmalar | No | 
> | kayıt defterleri/görevler | No |  
> | kayıt defterleri/Web kancaları | No | 

## <a name="microsoftcontainerservice"></a>Microsoft. ContainerService

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | containerservices | Hayır.<br/><br/> Hizmet [kullanımdan kaldırıldı](https://azure.microsoft.com/updates/azure-container-service-will-retire-on-january-31-2020/).
> | managedkümeler | No | 
> | openshiftmanagedclusters | No | 

## <a name="microsoftcontentmoderator"></a>Microsoft. Contentmoderatör

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | uygulamalar | No | 

## <a name="microsoftcortanaanalytics"></a>Microsoft. Cortanaanalizi

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | accounts | No | 

## <a name="microsoftcostmanagement"></a>Microsoft. CostManagement

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | bağlayıcılar | No |  

## <a name="microsoftcustomerinsights"></a>Microsoft. Customerınsights

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | hub'lar | No |  

## <a name="microsoftcustomproviders"></a>Microsoft. CustomProviders

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | resourceproviders | No | 

## <a name="microsoftdatabox"></a>Microsoft. DataBox

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | Çizelge | No | 

## <a name="microsoftdataboxedge"></a>Microsoft. DataBoxEdge

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | databoxedgedevices | No | 

## <a name="microsoftdatabricks"></a>Microsoft. Databricks

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | çalışma alanı | No | 

## <a name="microsoftdatacatalog"></a>Microsoft. DataCatalog

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | larına | No | 
> | veri katalogları | No | 

## <a name="microsoftdataconnect"></a>Microsoft. DataConnect

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | connectionyöneticileri | No | 

## <a name="microsoftdataexchange"></a>Microsoft. DataExchange

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | paketlerle | No | 
> | Planlama | No | 

## <a name="microsoftdatafactory"></a>Microsoft. DataFactory

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | veri fabrikaları | No | 
> | larının | No |  

## <a name="microsoftdatalake"></a>Microsoft. DataLake

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | datalakeaccounts | No | 

## <a name="microsoftdatalakeanalytics"></a>Microsoft. DataLakeAnalytics

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | accounts | No | 

## <a name="microsoftdatalakestore"></a>Microsoft. DataLakeStore

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | accounts | No | 

## <a name="microsoftdatamigration"></a>Microsoft. DataMigration

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | services | No | 
> | Hizmetler/Projeler | No | 
> | Lara | No | 

## <a name="microsoftdatashare"></a>Microsoft. DataShare

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | accounts | No | 

## <a name="microsoftdbformariadb"></a>Microsoft. Dbformarıdb

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | larý | Hizmet, coğrafi olarak yedekli yedekleme depolaması ile sağlanmışsa, diğer bölgelerde geri yüklemek için coğrafi geri yükleme kullanabilirsiniz. [Daha fazla bilgi edinin](../../mariadb/concepts-business-continuity.md#recover-from-an-azure-regional-data-center-outage)

## <a name="microsoftdbformysql"></a>Microsoft. Dbformyısql

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | larý | No |  

## <a name="microsoftdbforpostgresql"></a>Microsoft. DBforPostgreSQL

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | sunucu grupları | No | 
> | larý | No |  
> | serversv2 | No | 

## <a name="microsoftdeploymentmanager"></a>Microsoft. DeploymentManager

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | artifactsources | No | 
> | piyasaya çıkarma | No |  
> | servicetopolojileri | No | 
> | servicetopolojileri/hizmetler | No |  
> | servicetopolojileri/hizmetler/serviceunits | No | 
> | adımlar | No | 

## <a name="microsoftdevices"></a>Microsoft. Devices

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | elaun havuzları | Hayır. Kaynak gösterilmez.
> | elaun havuzları/ıothubkiracılar | Hayır. Kaynak gösterilmez.
> | ıothubs | Evet. [Daha fazla bilgi edinin](../../iot-hub/iot-hub-how-to-clone.md)
> | provisioningservices | No | 

## <a name="microsoftdevspaces"></a>Microsoft. DevSpaces

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | denetleyiciler | No | 
> | AKS kümesi | No<br/><br/> Başka bir bölgeye geçme hakkında [daha fazla bilgi edinin](../../dev-spaces/faq.md#can-i-migrate-my-aks-cluster-with-azure-dev-spaces-to-another-region) .

## <a name="microsoftdevtestlab"></a>Microsoft. DevTestLab

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | labcenters | No | 
> | larda | No | 
> | Laboratuvarlar/ortamlar | No |  
> | Labs/servicerunanlar | No | 
> | Labs/virtualmachines | No |  
> | cağını | No |  

## <a name="microsoftdocumentdb"></a>Microsoft.DocumentDB

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | veritabanı hesapları | No | 

## <a name="microsoftdomainregistration"></a>Microsoft. DomainRegistration

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | etki alanları | No | 

## <a name="microsoftenterpriseknowledgegraph"></a>Microsoft. EnterpriseKnowledgeGraph

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | services | No |  

## <a name="microsofteventgrid"></a>Microsoft. EventGrid

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | etki alanları | No |  
> | konuları | No | 

## <a name="microsofteventhub"></a>Microsoft. EventHub

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | leriniz | No |  
> | öznitelikleri | Evet (şablon ile)<br/><br/> [Bir olay hub 'ı ad alanını başka bir bölgeye taşıma](../../event-hubs/move-across-regions.md) | 

## <a name="microsoftgenomics"></a>Microsoft. Genomiks

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | accounts | No | 

## <a name="microsofthanaonazure"></a>Microsoft. HanaOnAzure

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | hanaınstances | No | 
> | sapizleyicileri | No |  

## <a name="microsofthdinsight"></a>Microsoft. HDInsight

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | leriniz | No | 

## <a name="microsofthealthcareapis"></a>Microsoft. Healthgelişme API 'leri

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | services | No |  

## <a name="microsofthybridcompute"></a>Microsoft. HybridCompute

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | larla | No | 

## <a name="microsofthybriddata"></a>Microsoft. HybridData

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | veri yöneticileri |  No | 

## <a name="microsoftimportexport"></a>Microsoft. ımportexport

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | Çizelge |  No | 

## <a name="microsoftinsights"></a>Microsoft. Insights

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | accounts | No | 
> | actiongroups |  No | 
> | activitylogalerts | No | 
> | alertrules |  No | 
> | autoscalesettings |  No | 
> | bileşenleri |  No |  
> | guestdiagnosticsettings | No | 
> | metricalerts | No | 
> | notificationgroups | No | 
> | notificationrules | No | 
> | scheduledqueryrules |  No | 
> | Web testleri |  No | 
> | çalışma kitapları |  No |  


## <a name="microsoftiotcentral"></a>Microsoft. ıotcentral

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | checknameavaılabılıty |  Hayır.<br/><br/> IoT Central, coğrafi bölgeler değil, coğrafi bölgeler ile birlikte kullanılabilir.
> | çıkarılamıyor | No

## <a name="microsoftiothub"></a>Microsoft. ıothub

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> |  ıothub |  Evet (kopya hub 'ı) <br/><br/> [IoT Hub 'ını başka bir bölgeye kopyalama](../../iot-hub/iot-hub-how-to-clone.md)

## <a name="microsoftiotspaces"></a>Microsoft. ıotspaces

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | checknameavaılabılıty |  No |  
> | çıkarılamıyor |  No | 

## <a name="microsoftkeyvault"></a>Microsoft. Keykasası

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | hsmpools | No | 
> | kasaları |  No | 


## <a name="microsoftkusto"></a>Microsoft.Kusto

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | leriniz |  No |  

## <a name="microsoftlabservices"></a>Microsoft. LabServices

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | labaccounts | No | 

## <a name="microsoftlocationbasedservices"></a>Microsoft. LocationBasedServices

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | accounts | No | 

## <a name="microsoftlocationservices"></a>Microsoft. LocationServices

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | accounts | Hayır, küresel bir hizmettir.

## <a name="microsoftlogic"></a>Microsoft. Logic

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | hostingenvironments | No | 
> | Tümleştirme hesapları |  No |  
> | ıntegrationserviceortamortamları | No | 
> | ısotedenvironments | No | 
> | sürdürülen |  No |  

## <a name="microsoftmachinelearning"></a>Microsoft. Machinöğrenim

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | commitmentplanlar |  No | 
> | Hizmetleri |  No | 
> | çalışma alanı |  No | 

## <a name="microsoftmachinelearningcompute"></a>Microsoft. MachineLearningCompute

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | operationalizationkümeleri |  No | 

## <a name="microsoftmachinelearningexperimentation"></a>Microsoft. MachineLearningExperimentation

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | accounts | No | 
> | hesaplar/çalışma alanları | No | 
> | hesaplar/çalışma alanları/projeler | No | 
> | teamaccounts | No | 
> | teamaccounts/çalışma alanları | No | 
> | teamaccounts/çalışma alanları/projeler | No | 

## <a name="microsoftmachinelearningmodelmanagement"></a>Microsoft. MachineLearningModelManagement

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | accounts | No | 

## <a name="microsoftmachinelearningoperationalization"></a>Microsoft. MachineLearningOperationalization

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | hostinghesapları | No | 

## <a name="microsoftmachinelearningservices"></a>Microsoft.MachineLearningServices

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | çalışma alanı | No | 

## <a name="microsoftmanagedidentity"></a>Microsoft. Managedıdentity

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | userassignedıdentities | No | 

## <a name="microsoftmaps"></a>Microsoft. Maps

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | accounts |  Hayır, Azure Maps bir jeo-uzamsal hizmettir. 

## <a name="microsoftmarketplaceapps"></a>Microsoft. MarketplaceApps

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | classicdevservices | Klasik hizmetler için hiçbir iş planlanmadı 

## <a name="microsoftmedia"></a>Microsoft. Media

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | mediaservices |  No | 
> | mediaservices/liveevents |  No | 
> | mediaservices/streamingendpoints |  No | 

## <a name="microsoftmicroservices4spring"></a>Microsoft. Microservices4Spring

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | appkümeler | No | 

## <a name="microsoftmigrate"></a>Microsoft. Migrate

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | assessmentprojects | No | 
> | migrateprojects | No | 
> | projeyle | No | 

## <a name="microsoftnetapp"></a>Microsoft. NetApp

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | netappaccounts | No | 
> | netappaccounts/capacityhavuzları | No | 
> | netappaccounts/capacityhavuzları/birimleri | No | 
> | netappaccounts/capacityhavuzlar/Volumes/Mount hedefleri | No | 
> | netappaccounts/capacityhavuzlar/birimler/anlık görüntüler | No | 

## <a name="microsoftnetwork"></a>Microsoft.Network

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | applicationgateway 'ler | No | 
> | applicationgatewaywebapplicationfirewallpolicies | No | 
> | applicationsecuritygroups |  No |  
> | azurefirewalls |  No |  
> | Savunma Konakları | No | 
> | bağlantının |  No | 
> | ddoscustompolicies |  No | 
> | ddosprotectionplanlar | No | 
> | dnszones |  No | 
> | expressroutedevreleri | No | 
> | expressroutecrossconnections | No | 
> | expressroutegateway 'ler | No | 
> | expressrouteports | No | 
> | frontkapıların | No | 
> | frontdoorwebapplicationfirewallpolicies | No | 
> | loadbalancers | Yes <br/><br/> Varolan yapılandırmayı bir şablon olarak dışa aktarabilir ve şablonu yeni bölgede dağıtabilirsiniz. [Dış](../..//load-balancer/move-across-regions-external-load-balancer-portal.md) veya [iç](../../load-balancer/move-across-regions-internal-load-balancer-portal.md) yük dengeleyiciyi taşımayı öğrenin. |
> | localnetworkgateway 'ler |  No | 
> | natgateway 'ler |  No | 
> | networkıntpolicies ilkeleri |  No | 
> | NetworkInterfaces | Yes | 
> | networkprofiles | No | 
> | networksecuritygroups | Yes | 
> | networkwatchers |  No |  
> | networkwatchers/connectionmonitörleri |  No | 
> | networkwatchers/uzunluler |  No | 
> | networkwatchers/pingkafesler |  No | 
> | p2svpngateways | No | 
> | privatednszones |  No |  
> | privatednszones/virtualnetworklinks |  No |  
> | privateendpoints | No | 
> | privatelinkservices | No | 
> | publicıpaddresses | Yes<br/><br/> Var olan genel IP adresi yapılandırmasını bir şablon olarak dışa aktarabilir ve şablonu yeni bölgede dağıtabilirsiniz. Genel IP adresini taşıma hakkında [daha fazla bilgi edinin](../../virtual-network/move-across-regions-publicip-portal.md) . |
> | publicıpöneklerini | No | 
> | routefilters | No | 
> | routetables |  No | 
> | serviceendpointpolicies |  No | 
> | trafficmanagerprofiles |  No | 
> | virtualhub 'lar | No | 
> | virtualnetworkgateways |  No |  
> | virtualnetworks |  No | 
> | virtualnetworktaps | No | 
> | virtualwan | No | 
> | vpngateway (sanal WAN) | No | 
> | vpnsites (sanal WAN) | No | 
> | webapplicationfirewallpolicies |  No | 


## <a name="microsoftnotificationhubs"></a>Microsoft. Notificationhub 'Lar

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | öznitelikleri |  No | 
> | ad alanları/notificationhub 'lar |  No |  

## <a name="microsoftoperationalinsights"></a>Microsoft. Operationalınsights

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | çalışma alanı |  No | 



## <a name="microsoftoperationsmanagement"></a>Microsoft. OperationsManagement

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | managementconfigurations |  No | 
> | görünümler |  No | 

## <a name="microsoftpeering"></a>Microsoft. eşleme

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | eşlemeleri | No | 

## <a name="microsoftportal"></a>Microsoft. Portal

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | panolar | No | 

## <a name="microsoftportalsdk"></a>Microsoft. PortalSdk

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | rootresources | No | 

## <a name="microsoftpowerbi"></a>Microsoft. PowerBI

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | workspacecollections |  No | 

## <a name="microsoftpowerbidedicated"></a>Microsoft. Powerbiadanmış

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | kapasiteler |  No | 

## <a name="microsoftprojectoxford"></a>Microsoft. ProjectOxford

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | accounts | No | 

## <a name="microsoftrecoveryservices"></a>Microsoft. RecoveryServices

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | kasaları | Hayır.<br/><br/> Azure bölgeleri arasında Azure Backup için kurtarma hizmetleri kasalarının taşınması desteklenmez.<br/><br/> Azure Site Recovery için kurtarma hizmetleri kasalarında, kasayı hedef bölgede [devre dışı bırakıp yeniden](https://docs.microsoft.com/azure/site-recovery/move-vaults-across-regions) oluşturabilirsiniz. | 


## <a name="microsoftrelay"></a>Microsoft. Relay

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | öznitelikleri |  No | 

## <a name="microsoftresourcegraph"></a>Microsoft. ResourceGraph

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | lardır |  No |  

## <a name="microsoftsaas"></a>Microsoft. SaaS

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | uygulamalar |  No | 

## <a name="microsoftscheduler"></a>Microsoft. Scheduler

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | flows |  No |  
> | işlere |  No | 

## <a name="microsoftsearch"></a>Microsoft. Search

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | searchservices |  No | 


## <a name="microsoftsecurity"></a>Microsoft.Security

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | ıotsecuritysolutions |  No | 
> | playbookconfigurations | No | 

## <a name="microsoftservermanagement"></a>Microsoft. ServerManagement

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | geçidinin | No | 
> | düğümlerini | No | 

## <a name="microsoftservicebus"></a>Microsoft.ServiceBus

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | öznitelikleri |  No | 

## <a name="microsoftservicefabric"></a>Microsoft. ServiceFabric

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | uygulamalar | No | 
> | leriniz |  No | 
> | kümeler/uygulamalar | No | 
> | kapsayıcı grupları | No | 
> | containergroupsets | No | 
> | edgeclusters | No | 
> | Mamak | No | 
> | secretmağazaları | No | 
> | volumes | No | 

## <a name="microsoftservicefabricmesh"></a>Microsoft. Servicefabrickafesi

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | uygulamalar |  No | 
> | kapsayıcı grupları | No | 
> | geçidinin |  No | 
> | Mamak |  No | 
> | kaynaklanır |  No | 
> | volumes |  No |  

## <a name="microsoftsignalrservice"></a>Microsoft. SignalRService

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | SignalR |  No |  

## <a name="microsoftsolutions"></a>Microsoft. Solutions

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | appliancedefinitions | No | 
> | yaptı | No | 
> | applicationdefinitions | No | 
> | uygulamalar | No | 
> | jistekleri | No | 

## <a name="microsoftsql"></a>Microsoft.Sql

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | ınstancepools | No | 
> | ManagedInstances | Yes | 
> | ManagedInstances/veritabanları | Yes | 
> | larý | Yes | 
> | sunucular/veritabanları | Yes | 
> | sunucular/elaun havuzları | Yes | 
> | virtualkümeler | Yes | 

## <a name="microsoftsqlvirtualmachine"></a>Microsoft. SqlVirtualMachine

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | sqlvirtualmachinegroups |  No |  
> | sqlvirtualmachines |  No |  

## <a name="microsoftsqlvm"></a>Microsoft. SqlVM

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | dwvm | No | 

## <a name="microsoftstorage"></a>Microsoft.Storage

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | storageaccounts | Yes<br/><br/> [Bir Azure Depolama hesabını başka bir bölgeye taşıma](../../storage/common/storage-account-move.md) | 

## <a name="microsoftstoragecache"></a>Microsoft. StorageCache

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | önbelleklerinde | No | 

## <a name="microsoftstoragesync"></a>Microsoft. Storagessync

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | storagesyncservices |  No | 

## <a name="microsoftstoragesyncdev"></a>Microsoft. StorageSyncDev

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | storagesyncservices | No | 

## <a name="microsoftstoragesyncint"></a>Microsoft. Storagesyncınt

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | storagesyncservices | No | 

## <a name="microsoftstorsimple"></a>Microsoft. StorSimple

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | ilerinde | No | 

## <a name="microsoftstreamanalytics"></a>Microsoft. StreamAnalytics

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | streammingjobs |  No |  


## <a name="microsoftstreamanalyticsexplorer"></a>Microsoft. StreamAnalyticsExplorer

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | lý | No | 
> | ortamlar/EventSources | No | 
> | larında | No | 
> | örnekler/ortamlar | No | 
> | örnekler/ortamlar/EventSources | No | 

## <a name="microsoftterraformoss"></a>Microsoft. Teroyformoss

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | providerkayıtları | No | 
> | kaynaklar | No | 

## <a name="microsofttimeseriesinsights"></a>Microsoft. Timeseriesınsights

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | lý |  No | 
> | ortamlar/EventSources |  No |  
> | ortamlar/referencedataset 'ler |  No | 

## <a name="microsofttoken"></a>Microsoft. Token

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | depolaya | No | 

## <a name="microsoftvirtualmachineimages"></a>Microsoft. Virtualmachineımages

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | ımagetemplates | No | 

## <a name="microsoftvisualstudio"></a>Microsoft. VisualStudio

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | account |  No | 
> | Hesap/uzantı |  No | 
> | hesap/proje |  No | 



## <a name="microsoftvmwarecloudsimple"></a>Microsoft. Vmwarechoparlör basit

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | ayrılmış cloudnodes | No | 
> | ayrılmış CloudService | No | 
> | virtualmachines | No | 

## <a name="microsoftweb"></a>Microsoft. Web

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | sertifikalar | No | 
> | connectiongateway 'ler |  No |  
> | bağlantının |  No |  
> | customapsıs |  No | 
> | hostingenvironments | No | 
> | serverfarms |  No |  
> | Siteler |  No | 
> | siteler/premieraddons |  No |  
> | siteler/yuvalar |  No |  


## <a name="microsoftwindowsiot"></a>Microsoft. Windowsıot

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | deviceservices | No | 

## <a name="microsoftwindowsvirtualdesktop"></a>Microsoft. WindowsVirtualDesktop

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | applicationgroups | No | 
> | Ana bilgisayar havuzları | No | 
> | çalışma alanı | No | 

## <a name="third-party-services"></a>Üçüncü taraf hizmetleri

Üçüncü taraf hizmetler şu anda taşıma işlemini desteklemiyor.
