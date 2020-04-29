---
title: Bölgeler arasında Azure kaynaklarını taşıma desteği
description: Azure bölgeleri arasında taşınabilecek Azure Kaynak türlerini listeler
author: rayne-wiselman
ms.service: azure-resource-manager
ms.topic: reference
ms.date: 01/20/2020
ms.author: raynew
ms.openlocfilehash: 9bc7dc66ccf3049ac878f7871c816e5ade1afde5
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "76760717"
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
> - [Microsoft. Batchaı](#microsoftbatchai)
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
> - [Microsoft. DocumentDB](#microsoftdocumentdb)
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
> | DomainServices | Hayır | 
> | DomainServices/replicasets | Hayır | 

## <a name="microsoftaadiam"></a>Microsoft. aadihar

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | Kira | Hayır |

## <a name="microsoftalertsmanagement"></a>Microsoft. AlertsManagement

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | actionrules | Hayır | 

## <a name="microsoftanalysisservices"></a>Microsoft. AnalysisServices

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | larý | Hayır |

## <a name="microsoftapimanagement"></a>Microsoft. Apimanane

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | hizmet |  Yes | 

## <a name="microsoftappconfiguration"></a>Microsoft. AppConfiguration

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | configurationmağazaların | Hayır | 

## <a name="microsoftappservice"></a>Microsoft. AppService

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | apiapps | Hayır | 
> | appdentities | Hayır | 
> | geçidinin | Hayır | 


## <a name="microsoftauthorization"></a>Microsoft.Authorization

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | poliyasatamaları | Hayır |

## <a name="microsoftautomation"></a>Microsoft. Automation

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | automationaccounts | Hayır | 
> | automationaccounts/Configurations | Hayır | 
> | automationaccounts/runbook 'lar | Hayır | 



## <a name="microsoftazureactivedirectory"></a>Microsoft. AzureActiveDirectory

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | b2cdirectories | Hayır | 

## <a name="microsoftazuredata"></a>Microsoft. AzureData

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | sqlserverkayıtları | Hayır |

## <a name="microsoftazurestack"></a>Microsoft. AzureStack

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | kayıtlarında | Hayır | 

## <a name="microsoftbatch"></a>Microsoft.Batch

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | batchaccounts | Hayır |

## <a name="microsoftbatchai"></a>Microsoft. Batchaı

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | leriniz | Hayır | 
> | dosya sunucuları | Hayır | 
> | Çizelge | Hayır | 
> | çalışma alanı | Hayır | 

## <a name="microsoftbingmaps"></a>Microsoft. BingMaps

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | mapapsıs | Hayır | 

## <a name="microsoftbiztalkservices"></a>Microsoft. BizTalkServices

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | biztalk | Hayır | 

## <a name="microsoftblockchain"></a>Microsoft. Blockzinciri

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | blockchainmembers | Hayır |
> | izleyicileri | Hayır | 

## <a name="microsoftblueprint"></a>Microsoft. Blueprint

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | şema tasmi | Hayır | 

## <a name="microsoftbotservice"></a>Microsoft. BotService

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | botservices | Hayır | 

## <a name="microsoftcache"></a>Microsoft. Cache

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | Redis | Hayır | 


## <a name="microsoftcdn"></a>Microsoft. CDN

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | cdnwebapplicationfirewallpolicies | Hayır |
> | lerinize | Hayır | 
> | Profiller/uç noktalar | Hayır | 

## <a name="microsoftcertificateregistration"></a>Microsoft. CertificateRegistration

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | sertifikadüzenleri | Hayır | 


## <a name="microsoftclassiccompute"></a>Microsoft.ClassicCompute

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | DomainNames | Hayır |  
> | virtualmachines | Hayır | 



## <a name="microsoftclassicnetwork"></a>Microsoft. ClassicNetwork

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | networksecuritygroups | Hayır |
> | rezervler | Hayır | 
> | virtualnetworks | Hayır | 

## <a name="microsoftclassicstorage"></a>Microsoft. ClassicStorage

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | storageaccounts | Yes |  


## <a name="microsoftcognitiveservices"></a>Microsoft. Biliveservices

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | accounts | Hayır | 

## <a name="microsoftcompute"></a>Microsoft.Compute

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | availabilitysets | Hayır | 
> | diskencryptionsets | Hayır | 
> | disklerinden | Hayır | 
> | Galeriler | Hayır | 
> | Galeriler/görüntüler | Hayır | 
> | Galeriler/resimler/sürümler | Hayır | 
> | hostgroups | Hayır | 
> | hostgroups/konaklar | Hayır | 
> | images | Hayır | 
> | proximityplacementgroups | Hayır | 
> | restorepointcollections | Hayır | 
> | sharedvmımages | Hayır | 
> | sharedvmımages/sürümler | Hayır | 
> | anlık görüntüler | Hayır | 
> | virtualmachines | Yes | 
> | virtualmachines/uzantıları | Hayır | 
> | virtualmachinescalesets | Hayır | 

## <a name="microsoftcontainer"></a>Microsoft. Container

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | kapsayıcı grupları | Hayır | 

## <a name="microsoftcontainerinstance"></a>Microsoft. Containerınstance

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | kapsayıcı grupları | Hayır | 

## <a name="microsoftcontainerregistry"></a>Microsoft. ContainerRegistry

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | kayıt | Hayır |  
> | kayıt defterleri/BuildTasks | Hayır |  
> | kayıt defterleri/çoğaltmalar | Hayır | 
> | kayıt defterleri/görevler | Hayır |  
> | kayıt defterleri/Web kancaları | Hayır | 

## <a name="microsoftcontainerservice"></a>Microsoft. ContainerService

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | containerservices | Hayır | 
> | managedkümeler | Hayır | 
> | openshiftmanagedclusters | Hayır | 

## <a name="microsoftcontentmoderator"></a>Microsoft. Contentmoderatör

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | uygulamalar | Hayır | 

## <a name="microsoftcortanaanalytics"></a>Microsoft. Cortanaanalizi

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | accounts | Hayır | 

## <a name="microsoftcostmanagement"></a>Microsoft. CostManagement

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | bağlayıcılar | Hayır |  

## <a name="microsoftcustomerinsights"></a>Microsoft. Customerınsights

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | hub'lar | Hayır |  

## <a name="microsoftcustomproviders"></a>Microsoft. CustomProviders

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | resourceproviders | Hayır | 

## <a name="microsoftdatabox"></a>Microsoft. DataBox

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | Çizelge | Hayır | 

## <a name="microsoftdataboxedge"></a>Microsoft. DataBoxEdge

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | databoxedgedevices | Hayır | 

## <a name="microsoftdatabricks"></a>Microsoft. Databricks

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | çalışma alanı | Hayır | 

## <a name="microsoftdatacatalog"></a>Microsoft. DataCatalog

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | larına | Hayır | 
> | veri katalogları | Hayır | 

## <a name="microsoftdataconnect"></a>Microsoft. DataConnect

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | connectionyöneticileri | Hayır | 

## <a name="microsoftdataexchange"></a>Microsoft. DataExchange

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | paketlerle | Hayır | 
> | Planlama | Hayır | 

## <a name="microsoftdatafactory"></a>Microsoft. DataFactory

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | veri fabrikaları | Hayır | 
> | larının | Hayır |  

## <a name="microsoftdatalake"></a>Microsoft. DataLake

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | datalakeaccounts | Hayır | 

## <a name="microsoftdatalakeanalytics"></a>Microsoft. DataLakeAnalytics

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | accounts | Hayır | 

## <a name="microsoftdatalakestore"></a>Microsoft. DataLakeStore

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | accounts | Hayır | 

## <a name="microsoftdatamigration"></a>Microsoft. DataMigration

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | services | Hayır | 
> | Hizmetler/Projeler | Hayır | 
> | Lara | Hayır | 

## <a name="microsoftdatashare"></a>Microsoft. DataShare

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | accounts | Hayır | 

## <a name="microsoftdbformariadb"></a>Microsoft. Dbformarıdb

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | larý | Hayır |  

## <a name="microsoftdbformysql"></a>Microsoft. Dbformyısql

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | larý | Hayır |  

## <a name="microsoftdbforpostgresql"></a>Microsoft. DBforPostgreSQL

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | sunucu grupları | Hayır | 
> | larý | Hayır |  
> | serversv2 | Hayır | 

## <a name="microsoftdeploymentmanager"></a>Microsoft. DeploymentManager

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | artifactsources | Hayır | 
> | piyasaya çıkarma | Hayır |  
> | servicetopolojileri | Hayır | 
> | servicetopolojileri/hizmetler | Hayır |  
> | servicetopolojileri/hizmetler/serviceunits | Hayır | 
> | adımlar | Hayır | 

## <a name="microsoftdevices"></a>Microsoft. Devices

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | elaun havuzları | Hayır | 
> | elaun havuzları/ıothubkiracılar | Hayır | 
> | ıothubs | Yes | 
> | provisioningservices | Hayır | 

## <a name="microsoftdevspaces"></a>Microsoft. DevSpaces

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | denetleyiciler | Hayır | 

## <a name="microsoftdevtestlab"></a>Microsoft. DevTestLab

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | labcenters | Hayır | 
> | larda | Hayır | 
> | Laboratuvarlar/ortamlar | Hayır |  
> | Labs/servicerunanlar | Hayır | 
> | Labs/virtualmachines | Hayır |  
> | cağını | Hayır |  

## <a name="microsoftdocumentdb"></a>Microsoft. DocumentDB

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | veritabanı hesapları | Hayır | 

## <a name="microsoftdomainregistration"></a>Microsoft. DomainRegistration

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | etki alanları | Hayır | 

## <a name="microsoftenterpriseknowledgegraph"></a>Microsoft. EnterpriseKnowledgeGraph

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | services | Hayır |  

## <a name="microsofteventgrid"></a>Microsoft. EventGrid

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | etki alanları | Hayır |  
> | konuları | Hayır | 

## <a name="microsofteventhub"></a>Microsoft. EventHub

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | leriniz | Hayır |  
> | öznitelikleri | Hayır | 

## <a name="microsoftgenomics"></a>Microsoft. Genomiks

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | accounts | Hayır | 

## <a name="microsofthanaonazure"></a>Microsoft. HanaOnAzure

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | hanaınstances | Hayır | 
> | sapizleyicileri | Hayır |  

## <a name="microsofthdinsight"></a>Microsoft. HDInsight

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | leriniz | Hayır | 

## <a name="microsofthealthcareapis"></a>Microsoft. Healthgelişme API 'leri

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | services | Hayır |  

## <a name="microsofthybridcompute"></a>Microsoft. HybridCompute

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | larla | Hayır | 

## <a name="microsofthybriddata"></a>Microsoft. HybridData

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | veri yöneticileri |  Hayır | 

## <a name="microsoftimportexport"></a>Microsoft. ımportexport

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | Çizelge |  Hayır | 

## <a name="microsoftinsights"></a>Microsoft. Insights

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | accounts | Hayır | 
> | actiongroups |  Hayır | 
> | activitylogalerts | Hayır | 
> | alertrules |  Hayır | 
> | autoscalesettings |  Hayır | 
> | bileşenleri |  Hayır |  
> | guestdiagnosticsettings | Hayır | 
> | metricalerts | Hayır | 
> | notificationgroups | Hayır | 
> | notificationrules | Hayır | 
> | scheduledqueryrules |  Hayır | 
> | Web testleri |  Hayır | 
> | çalışma kitapları |  Hayır |  


## <a name="microsoftiotcentral"></a>Microsoft. ıotcentral

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | ıotapps |  Hayır |  

## <a name="microsoftiotspaces"></a>Microsoft. ıotspaces

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | checknameavaılabılıty |  Hayır |  
> | çıkarılamıyor |  Hayır | 

## <a name="microsoftkeyvault"></a>Microsoft. Keykasası

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | hsmpools | Hayır | 
> | kasaları |  Hayır | 


## <a name="microsoftkusto"></a>Microsoft.Kusto

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | leriniz |  Hayır |  

## <a name="microsoftlabservices"></a>Microsoft. LabServices

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | labaccounts | Hayır | 

## <a name="microsoftlocationbasedservices"></a>Microsoft. LocationBasedServices

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | accounts | Hayır | 

## <a name="microsoftlocationservices"></a>Microsoft. LocationServices

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | accounts | Hayır | 

## <a name="microsoftlogic"></a>Microsoft. Logic

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | hostingenvironments | Hayır | 
> | Tümleştirme hesapları |  Hayır |  
> | ıntegrationserviceortamortamları | Hayır | 
> | ısotedenvironments | Hayır | 
> | sürdürülen |  Hayır |  

## <a name="microsoftmachinelearning"></a>Microsoft. Machinöğrenim

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | commitmentplanlar |  Hayır | 
> | Hizmetleri |  Hayır | 
> | çalışma alanı |  Hayır | 

## <a name="microsoftmachinelearningcompute"></a>Microsoft. MachineLearningCompute

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | operationalizationkümeleri |  Hayır | 

## <a name="microsoftmachinelearningexperimentation"></a>Microsoft. MachineLearningExperimentation

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | accounts | Hayır | 
> | hesaplar/çalışma alanları | Hayır | 
> | hesaplar/çalışma alanları/projeler | Hayır | 
> | teamaccounts | Hayır | 
> | teamaccounts/çalışma alanları | Hayır | 
> | teamaccounts/çalışma alanları/projeler | Hayır | 

## <a name="microsoftmachinelearningmodelmanagement"></a>Microsoft. MachineLearningModelManagement

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | accounts | Hayır | 

## <a name="microsoftmachinelearningoperationalization"></a>Microsoft. MachineLearningOperationalization

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | hostinghesapları | Hayır | 

## <a name="microsoftmachinelearningservices"></a>Microsoft.MachineLearningServices

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | çalışma alanı | Hayır | 

## <a name="microsoftmanagedidentity"></a>Microsoft. Managedıdentity

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | userassignedıdentities | Hayır | 

## <a name="microsoftmaps"></a>Microsoft. Maps

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | accounts |  Hayır |  

## <a name="microsoftmarketplaceapps"></a>Microsoft. MarketplaceApps

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | classicdevservices | Hayır | 

## <a name="microsoftmedia"></a>Microsoft. Media

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | mediaservices |  Hayır | 
> | mediaservices/liveevents |  Hayır | 
> | mediaservices/streamingendpoints |  Hayır | 

## <a name="microsoftmicroservices4spring"></a>Microsoft. Microservices4Spring

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | appkümeler | Hayır | 

## <a name="microsoftmigrate"></a>Microsoft. Migrate

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | assessmentprojects | Hayır | 
> | migrateprojects | Hayır | 
> | projeyle | Hayır | 

## <a name="microsoftnetapp"></a>Microsoft. NetApp

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | netappaccounts | Hayır | 
> | netappaccounts/capacityhavuzları | Hayır | 
> | netappaccounts/capacityhavuzları/birimleri | Hayır | 
> | netappaccounts/capacityhavuzlar/Volumes/Mount hedefleri | Hayır | 
> | netappaccounts/capacityhavuzlar/birimler/anlık görüntüler | Hayır | 

## <a name="microsoftnetwork"></a>Microsoft.Network

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | applicationgateway 'ler | Hayır | 
> | applicationgatewaywebapplicationfirewallpolicies | Hayır | 
> | applicationsecuritygroups |  Hayır |  
> | azurefirewalls |  Hayır |  
> | Savunma Konakları | Hayır | 
> | bağlantının |  Hayır | 
> | ddoscustompolicies |  Hayır | 
> | ddosprotectionplanlar | Hayır | 
> | dnszones |  Hayır | 
> | expressroutedevreleri | Hayır | 
> | expressroutecrossconnections | Hayır | 
> | expressroutegateway 'ler | Hayır | 
> | expressrouteports | Hayır | 
> | frontkapıların | Hayır | 
> | frontdoorwebapplicationfirewallpolicies | Hayır | 
> | loadbalancers | Evet-temel SKU<br>Standart SKU yok | Evet-temel SKU<br> -Evet standart SKU |
> | localnetworkgateway 'ler |  Hayır | 
> | natgateway 'ler |  Hayır | 
> | networkıntpolicies ilkeleri |  Hayır | 
> | NetworkInterfaces | Yes | 
> | networkprofiles | Hayır | 
> | networksecuritygroups | Yes | 
> | networkwatchers |  Hayır |  
> | networkwatchers/connectionmonitörleri |  Hayır | 
> | networkwatchers/uzunluler |  Hayır | 
> | networkwatchers/pingkafesler |  Hayır | 
> | p2svpngateways | Hayır | 
> | privatednszones |  Hayır |  
> | privatednszones/virtualnetworklinks |  Hayır |  
> | privateendpoints | Hayır | 
> | privatelinkservices | Hayır | 
> | publicıpaddresses | Evet-temel SKU<br>Standart SKU yok | Evet-temel SKU<br>Standart SKU yok |
> | publicıpöneklerini | Hayır | 
> | routefilters | Hayır | 
> | routetables |  Hayır | 
> | serviceendpointpolicies |  Hayır | 
> | trafficmanagerprofiles |  Hayır | 
> | virtualhub 'lar | Hayır | 
> | virtualnetworkgateways |  Hayır |  
> | virtualnetworks |  Hayır | 
> | virtualnetworktaps | Hayır | 
> | virtualwan | Hayır | 
> | vpngateway (sanal WAN) | Hayır | 
> | vpnsites (sanal WAN) | Hayır | 
> | webapplicationfirewallpolicies |  Hayır | 


## <a name="microsoftnotificationhubs"></a>Microsoft. Notificationhub 'Lar

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | öznitelikleri |  Hayır | 
> | ad alanları/notificationhub 'lar |  Hayır |  

## <a name="microsoftoperationalinsights"></a>Microsoft. Operationalınsights

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | çalışma alanı |  Hayır | 



## <a name="microsoftoperationsmanagement"></a>Microsoft. OperationsManagement

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | managementconfigurations |  Hayır | 
> | görünümler |  Hayır | 

## <a name="microsoftpeering"></a>Microsoft. eşleme

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | eşlemeleri | Hayır | 

## <a name="microsoftportal"></a>Microsoft. Portal

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | panolar | Hayır | 

## <a name="microsoftportalsdk"></a>Microsoft. PortalSdk

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | rootresources | Hayır | 

## <a name="microsoftpowerbi"></a>Microsoft. PowerBI

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | workspacecollections |  Hayır | 

## <a name="microsoftpowerbidedicated"></a>Microsoft. Powerbiadanmış

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | kapasiteler |  Hayır | 

## <a name="microsoftprojectoxford"></a>Microsoft. ProjectOxford

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | accounts | Hayır | 

## <a name="microsoftrecoveryservices"></a>Microsoft. RecoveryServices

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | kasaları | Hayır. [Kasayı devre dışı bırakın ve Site Recovery için yeniden oluşturun](https://docs.microsoft.com/azure/site-recovery/move-vaults-across-regions)  | 


## <a name="microsoftrelay"></a>Microsoft. Relay

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | öznitelikleri |  Hayır | 

## <a name="microsoftresourcegraph"></a>Microsoft. ResourceGraph

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | lardır |  Hayır |  

## <a name="microsoftsaas"></a>Microsoft. SaaS

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | uygulamalar |  Hayır | 

## <a name="microsoftscheduler"></a>Microsoft. Scheduler

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | flows |  Hayır |  
> | işlere |  Hayır | 

## <a name="microsoftsearch"></a>Microsoft. Search

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | searchservices |  Hayır | 


## <a name="microsoftsecurity"></a>Microsoft.Security

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | ıotsecuritysolutions |  Hayır | 
> | playbookconfigurations | Hayır | 

## <a name="microsoftservermanagement"></a>Microsoft. ServerManagement

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | geçidinin | Hayır | 
> | düğümlerini | Hayır | 

## <a name="microsoftservicebus"></a>Microsoft.ServiceBus

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | öznitelikleri |  Hayır | 

## <a name="microsoftservicefabric"></a>Microsoft. ServiceFabric

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | uygulamalar | Hayır | 
> | leriniz |  Hayır | 
> | kümeler/uygulamalar | Hayır | 
> | kapsayıcı grupları | Hayır | 
> | containergroupsets | Hayır | 
> | edgeclusters | Hayır | 
> | Mamak | Hayır | 
> | secretmağazaları | Hayır | 
> | volumes | Hayır | 

## <a name="microsoftservicefabricmesh"></a>Microsoft. Servicefabrickafesi

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | uygulamalar |  Hayır | 
> | kapsayıcı grupları | Hayır | 
> | geçidinin |  Hayır | 
> | Mamak |  Hayır | 
> | kaynaklanır |  Hayır | 
> | volumes |  Hayır |  

## <a name="microsoftsignalrservice"></a>Microsoft. SignalRService

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | SignalR |  Hayır |  

## <a name="microsoftsolutions"></a>Microsoft. Solutions

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | appliancedefinitions | Hayır | 
> | yaptı | Hayır | 
> | applicationdefinitions | Hayır | 
> | uygulamalar | Hayır | 
> | jistekleri | Hayır | 

## <a name="microsoftsql"></a>Microsoft.Sql

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | ınstancepools | Hayır | 
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
> | sqlvirtualmachinegroups |  Hayır |  
> | sqlvirtualmachines |  Hayır |  

## <a name="microsoftsqlvm"></a>Microsoft. SqlVM

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | dwvm | Hayır | 

## <a name="microsoftstorage"></a>Microsoft.Storage

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | storageaccounts | Yes | 

## <a name="microsoftstoragecache"></a>Microsoft. StorageCache

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | önbelleklerinde | Hayır | 

## <a name="microsoftstoragesync"></a>Microsoft. Storagessync

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | storagesyncservices |  Hayır | 

## <a name="microsoftstoragesyncdev"></a>Microsoft. StorageSyncDev

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | storagesyncservices | Hayır | 

## <a name="microsoftstoragesyncint"></a>Microsoft. Storagesyncınt

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | storagesyncservices | Hayır | 

## <a name="microsoftstorsimple"></a>Microsoft. StorSimple

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | ilerinde | Hayır | 

## <a name="microsoftstreamanalytics"></a>Microsoft. StreamAnalytics

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | streammingjobs |  Hayır |  


## <a name="microsoftstreamanalyticsexplorer"></a>Microsoft. StreamAnalyticsExplorer

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | lý | Hayır | 
> | ortamlar/EventSources | Hayır | 
> | larında | Hayır | 
> | örnekler/ortamlar | Hayır | 
> | örnekler/ortamlar/EventSources | Hayır | 

## <a name="microsoftterraformoss"></a>Microsoft. Teroyformoss

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | providerkayıtları | Hayır | 
> | kaynaklar | Hayır | 

## <a name="microsofttimeseriesinsights"></a>Microsoft. Timeseriesınsights

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | lý |  Hayır | 
> | ortamlar/EventSources |  Hayır |  
> | ortamlar/referencedataset 'ler |  Hayır | 

## <a name="microsofttoken"></a>Microsoft. Token

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | depolaya | Hayır | 

## <a name="microsoftvirtualmachineimages"></a>Microsoft. Virtualmachineımages

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | ımagetemplates | Hayır | 

## <a name="microsoftvisualstudio"></a>Microsoft. VisualStudio

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | account |  Hayır | 
> | Hesap/uzantı |  Hayır | 
> | hesap/proje |  Hayır | 



## <a name="microsoftvmwarecloudsimple"></a>Microsoft. Vmwarechoparlör basit

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | ayrılmış cloudnodes | Hayır | 
> | ayrılmış CloudService | Hayır | 
> | virtualmachines | Hayır | 

## <a name="microsoftweb"></a>Microsoft. Web

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | sertifikalar | Hayır | 
> | connectiongateway 'ler |  Hayır |  
> | bağlantının |  Hayır |  
> | customapsıs |  Hayır | 
> | hostingenvironments | Hayır | 
> | serverfarms |  Hayır |  
> | Siteler |  Hayır | 
> | siteler/premieraddons |  Hayır |  
> | siteler/yuvalar |  Hayır |  


## <a name="microsoftwindowsiot"></a>Microsoft. Windowsıot

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | deviceservices | Hayır | 

## <a name="microsoftwindowsvirtualdesktop"></a>Microsoft. WindowsVirtualDesktop

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | applicationgroups | Hayır | 
> | Ana bilgisayar havuzları | Hayır | 
> | çalışma alanı | Hayır | 

## <a name="third-party-services"></a>Üçüncü taraf hizmetleri

Üçüncü taraf hizmetler şu anda taşıma işlemini desteklemiyor.
