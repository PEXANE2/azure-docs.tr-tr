---
title: Bölgeler arasında Azure kaynaklarını taşıma desteği
description: Azure bölgeleri arasında taşınabilecek Azure Kaynak türlerini listeler
author: rayne-wiselman
ms.service: azure-resource-manager
ms.topic: reference
ms.date: 08/25/2020
ms.author: raynew
ms.openlocfilehash: 49c5828e02bf96a536ff14f6b84e81f7adbe3090
ms.sourcegitcommit: fc8ce6ff76e64486d5acd7be24faf819f0a7be1d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/26/2021
ms.locfileid: "98806896"
---
# <a name="support-for-moving-azure-resources-across-regions"></a>Bölgeler arasında Azure kaynaklarını taşıma desteği

Bu makalede, bir Azure Kaynak türünün başka bir Azure bölgesine geçiş için desteklenip desteklenmediğini onaylar. 

Kaynak sağlayıcısı ad alanına atlayın:
> [!div class="op_single_selector"]
> - [Microsoft. AAD](#microsoftaad)
> - [Microsoft. aadihar](#microsoftaadiam)
> - [Microsoft. addons](#microsoftaddons)
> - [Microsoft. ADHybridHealthService](#microsoftadhybridhealthservice)
> - [Microsoft. Advisor](#microsoftadvisor)
> - [Microsoft. AlertsManagement](#microsoftalertsmanagement)
> - [Microsoft. AnalysisServices](#microsoftanalysisservices)
> - [Microsoft. Apimanane](#microsoftapimanagement)
> - [Microsoft. AppConfiguration](#microsoftappconfiguration)
> - [Microsoft. AppPlatform](#microsoftappplatform)
> - [Microsoft. AppService](#microsoftappservice)
> - [Microsoft. kanıtlama](#microsoftattestation)
> - [Microsoft.Authorization](#microsoftauthorization)
> - [Microsoft. Automation](#microsoftautomation)
> - [Microsoft. AVS](#microsoftavs)
> - [Microsoft. AzureActiveDirectory](#microsoftazureactivedirectory)
> - [Microsoft. AzureData](#microsoftazuredata)
> - [Microsoft. AzureStack](#microsoftazurestack)
> - [Microsoft. Azurestackhcı](#microsoftazurestackhci)
> - [Microsoft.Batch](#microsoftbatch)
> - [Microsoft. Faturalandırma](#microsoftbilling)
> - [Microsoft. BingMaps](#microsoftbingmaps)
> - [Microsoft. BizTalkServices](#microsoftbiztalkservices)
> - [Microsoft. Blockzinciri](#microsoftblockchain)
> - [Microsoft. BlockchainTokens](#microsoftblockchaintokens)
> - [Microsoft. Blueprint](#microsoftblueprint)
> - [Microsoft. BotService](#microsoftbotservice)
> - [Microsoft. Cache](#microsoftcache)
> - [Microsoft. Capacity](#microsoftcapacity)
> - [Microsoft. CDN](#microsoftcdn)
> - [Microsoft. CertificateRegistration](#microsoftcertificateregistration)
> - [Microsoft.ClassicCompute](#microsoftclassiccompute)
> - [Microsoft. ClassicInfrastructureMigrate](#microsoftclassicinfrastructuremigrate)
> - [Microsoft. ClassicNetwork](#microsoftclassicnetwork)
> - [Microsoft. ClassicStorage](#microsoftclassicstorage)
> - [Microsoft. ClassicSubscription](#microsoftclassicsubscription)
> - [Microsoft. Biliveservices](#microsoftcognitiveservices)
> - [Microsoft. Commerce](#microsoftcommerce)
> - [Microsoft.Compute](#microsoftcompute)
> - [Microsoft. tüketim](#microsoftconsumption)
> - [Microsoft. Containerınstance](#microsoftcontainerinstance)
> - [Microsoft. ContainerRegistry](#microsoftcontainerregistry)
> - [Microsoft. ContainerService](#microsoftcontainerservice)
> - [Microsoft. Contentmoderatör](#microsoftcontentmoderator)
> - [Microsoft. Cortanaanalizi](#microsoftcortanaanalytics)
> - [Microsoft. CostManagement](#microsoftcostmanagement)
> - [Microsoft. Customerınsights](#microsoftcustomerinsights)
> - [Microsoft. Customerkasası](#microsoftcustomerlockbox)
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
> - [Microsoft. DataProtection](#microsoftdataprotection)
> - [Microsoft. DataShare](#microsoftdatashare)
> - [Microsoft. Dbformarıdb](#microsoftdbformariadb)
> - [Microsoft. Dbformyısql](#microsoftdbformysql)
> - [Microsoft. DBforPostgreSQL](#microsoftdbforpostgresql)
> - [Microsoft. DeploymentManager](#microsoftdeploymentmanager)
> - [Microsoft. DesktopVirtualization](#microsoftdesktopvirtualization)
> - [Microsoft.Devices](#microsoftdevices)
> - [Microsoft. DevOps](#microsoftdevops)
> - [Microsoft. DevSpaces](#microsoftdevspaces)
> - [Microsoft. DevTestLab](#microsoftdevtestlab)
> - [Microsoft. DigitalTwins](#microsoftdigitaltwins)
> - [Microsoft.DocumentDB](#microsoftdocumentdb)
> - [Microsoft. DomainRegistration](#microsoftdomainregistration)
> - [Microsoft. EnterpriseKnowledgeGraph](#microsoftenterpriseknowledgegraph)
> - [Microsoft. EventGrid](#microsofteventgrid)
> - [Microsoft. EventHub](#microsofteventhub)
> - [Microsoft. deneme](#microsoftexperimentation)
> - [Microsoft. Falcon](#microsoftfalcon)
> - [Microsoft. Features](#microsoftfeatures)
> - [Microsoft. Genomiks](#microsoftgenomics)
> - [Microsoft. GuestConfiguration](#microsoftguestconfiguration)
> - [Microsoft. HanaOnAzure](#microsofthanaonazure)
> - [Microsoft. HardwareSecurityModules](#microsofthardwaresecuritymodules)
> - [Microsoft. HDInsight](#microsofthdinsight)
> - [Microsoft. Healthgelişme API 'leri](#microsofthealthcareapis)
> - [Microsoft. HybridCompute](#microsofthybridcompute)
> - [Microsoft. HybridData](#microsofthybriddata)
> - [Microsoft. HybridNetwork](#microsofthybridnetwork)
> - [Microsoft. Hydra](#microsofthydra)
> - [Microsoft. ımportexport](#microsoftimportexport)
> - [Microsoft. Insights](#microsoftinsights)
> - [Microsoft. ıotcentral](#microsoftiotcentral)
> - [Microsoft. ıotspaces](#microsoftiotspaces)
> - [Microsoft. Keykasası](#microsoftkeyvault)
> - [Microsoft. Kubernetes](#microsoftkubernetes)
> - [Microsoft. KubernetesConfiguration](#microsoftkubernetesconfiguration)
> - [Microsoft.Kusto](#microsoftkusto)
> - [Microsoft. LabServices](#microsoftlabservices)
> - [Microsoft. LocationBasedServices](#microsoftlocationbasedservices)
> - [Microsoft. LocationServices](#microsoftlocationservices)
> - [Microsoft. Logic](#microsoftlogic)
> - [Microsoft. Machinöğrenim](#microsoftmachinelearning)
> - [Microsoft. MachineLearningCompute](#microsoftmachinelearningcompute)
> - [Microsoft. MachineLearningExperimentation](#microsoftmachinelearningexperimentation)
> - [Microsoft. MachineLearningModelManagement](#microsoftmachinelearningmodelmanagement)
> - [Microsoft.MachineLearningServices](#microsoftmachinelearningservices)
> - [Microsoft. Maintenance](#microsoftmaintenance)
> - [Microsoft. Managedıdentity](#microsoftmanagedidentity)
> - [Microsoft. ManagedNetwork](#microsoftmanagednetwork)
> - [Microsoft. ManagedServices](#microsoftmanagedservices)
> - [Microsoft. Management](#microsoftmanagement)
> - [Microsoft. Maps](#microsoftmaps)
> - [Microsoft. Market](#microsoftmarketplace)
> - [Microsoft. MarketplaceApps](#microsoftmarketplaceapps)
> - [Microsoft. Marketplacesıralaması](#microsoftmarketplaceordering)
> - [Microsoft. Media](#microsoftmedia)
> - [Microsoft. Microservices4Spring](#microsoftmicroservices4spring)
> - [Microsoft. Migrate](#microsoftmigrate)
> - [Microsoft. MixedReality](#microsoftmixedreality)
> - [Microsoft. NetApp](#microsoftnetapp)
> - [Microsoft.Network](#microsoftnetwork)
> - [Microsoft. Notificationhub 'Lar](#microsoftnotificationhubs)
> - [Microsoft. ObjectStore](#microsoftobjectstore)
> - [Microsoft. OffAzure](#microsoftoffazure)
> - [Microsoft. Operationalınsights](#microsoftoperationalinsights)
> - [Microsoft. OperationsManagement](#microsoftoperationsmanagement)
> - [Microsoft. eşleme](#microsoftpeering)
> - [Microsoft. Poliyelei](#microsoftpolicyinsights)
> - [Microsoft. Portal](#microsoftportal)
> - [Microsoft. PowerBI](#microsoftpowerbi)
> - [Microsoft. Powerbiadanmış](#microsoftpowerbidedicated)
> - [Microsoft. ProjectBabylon](#microsoftprojectbabylon)
> - [Microsoft. ProviderHub](#microsoftproviderhub)
> - [Microsoft. hisse](#microsoftquantum)
> - [Microsoft. RecoveryServices](#microsoftrecoveryservices)
> - [Microsoft. RedHatOpenShift](#microsoftredhatopenshift)
> - [Microsoft. Relay](#microsoftrelay)
> - [Microsoft. ResourceGraph](#microsoftresourcegraph)
> - [Microsoft. ResourceHealth](#microsoftresourcehealth)
> - [Microsoft. resources](#microsoftresources)
> - [Microsoft. SaaS](#microsoftsaas)
> - [Microsoft. Search](#microsoftsearch)
> - [Microsoft.Security](#microsoftsecurity)
> - [Microsoft. Securityınsights](#microsoftsecurityinsights)
> - [Microsoft. SerialConsole](#microsoftserialconsole)
> - [Microsoft. ServerManagement](#microsoftservermanagement)
> - [Microsoft.ServiceBus](#microsoftservicebus)
> - [Microsoft. ServiceFabric](#microsoftservicefabric)
> - [Microsoft. Servicefabrickafesi](#microsoftservicefabricmesh)
> - [Microsoft. Services](#microsoftservices)
> - [Microsoft. SignalRService](#microsoftsignalrservice)
> - [Microsoft. SoftwarePlan](#microsoftsoftwareplan)
> - [Microsoft. Solutions](#microsoftsolutions)
> - [Microsoft.Sql](#microsoftsql)
> - [Microsoft. SqlVirtualMachine](#microsoftsqlvirtualmachine)
> - [Microsoft.Storage](#microsoftstorage)
> - [Microsoft. StorageCache](#microsoftstoragecache)
> - [Microsoft. Storagessync](#microsoftstoragesync)
> - [Microsoft. StorageSyncDev](#microsoftstoragesyncdev)
> - [Microsoft. Storagesyncınt](#microsoftstoragesyncint)
> - [Microsoft. StorSimple](#microsoftstorsimple)
> - [Microsoft. StreamAnalytics](#microsoftstreamanalytics)
> - [Microsoft. StreamAnalyticsExplorer](#microsoftstreamanalyticsexplorer)
> - [Microsoft. Subscription](#microsoftsubscription)
> - [Microsoft. support](#microsoftsupport)
> - [Microsoft. SYNAPSE](#microsoftsynapse)
> - [Microsoft. Timeseriesınsights](#microsofttimeseriesinsights)
> - [Microsoft. Token](#microsofttoken)
> - [Microsoft. Virtualmachineımages](#microsoftvirtualmachineimages)
> - [Microsoft. VisualStudio](#microsoftvisualstudio)
> - [Microsoft. VMware](#microsoftvmware)
> - [Microsoft. Vmwarechoparlör basit](#microsoftvmwarecloudsimple)
> - [Microsoft. VnfManager](#microsoftvnfmanager)
> - [Microsoft. VSOnline](#microsoftvsonline)
> - [Microsoft. Web](#microsoftweb)
> - [Microsoft. WindowsESU](#microsoftwindowsesu)
> - [Microsoft. Windowsıot](#microsoftwindowsiot)
> - [Microsoft. WorkloadBuilder](#microsoftworkloadbuilder)
> - [Microsoft. WorkloadMonitor](#microsoftworkloadmonitor)

## <a name="microsoftaad"></a>Microsoft. AAD

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | DomainServices | Hayır | 


## <a name="microsoftaadiam"></a>Microsoft. aadihar

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | diagnosticsettings | Hayır |
> | diagnosticsettingscategories | Hayır |
> | privatelinkforazuread | Hayır |
> | Kira |  Hayır |

## <a name="microsoftaddons"></a>MICROSOFT. Addons

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | destek sağlayıcıları | Hayır |

## <a name="microsoftadhybridhealthservice"></a>Microsoft. ADHybridHealthService

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- | 
> | aadsupportcases | Hayır |
> | addsservices | Hayır | 
> | aracısını | Hayır | 
> | anonymousapiusers | Hayır |
> | yapılandırma | Hayır | 
> | günlükler | Hayır | 
> | reports | Hayır | 
> | servicehealthölçümleri | Hayır | 
> | services | Hayır | 

## <a name="microsoftadvisor"></a>Microsoft. Advisor

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- | 
> | konfigürasyonları | Hayır | 
> | generatereyorumgeçişleri | Hayır |
> | meta veriler | Hayır |
> | Öneriler | Hayır |
> | gizlemeleri | Hayır | 

## <a name="microsoftalertsmanagement"></a>Microsoft. AlertsManagement

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | actionrules | Hayır | 
> | alerts | Hayır | 
> | alertslist | Hayır | 
> | alertsmetadata | Hayır | 
> | alertssummary | Hayır | 
> | alertssummarylist | Hayır | 
> | smartdetectoralertrules | Hayır | 
> | smartgroups | Hayır | 

## <a name="microsoftanalysisservices"></a>Microsoft. AnalysisServices

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | larý | Hayır |

## <a name="microsoftapimanagement"></a>Microsoft. Apimanane

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | reportfeedback | Hayır |
> | hizmet |  Evet (şablonu kullanarak) <br/><br/> [API Management bölgeler arasında taşıyın](../../api-management/api-management-howto-migrate.md). | 

## <a name="microsoftappconfiguration"></a>Microsoft. AppConfiguration

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | configurationmağazaların | Hayır | 
> | configurationmağazaların/eventgridfilters | Hayır |

## <a name="microsoftappplatform"></a>Microsoft. AppPlatform

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- | 
> | yay | Hayır | 

## <a name="microsoftappservice"></a>Microsoft. AppService

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | apiapps | Evet (şablonu kullanarak)<br/><br/> [App Service uygulamasını başka bir bölgeye taşıma](../../app-service/manage-move-across-regions.md) | 
> | appdentities | Hayır | 
> | geçidinin | Hayır | 

## <a name="microsoftattestation"></a>Microsoft. kanıtlama

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- | 
> | attestationproviders | Hayır | 

## <a name="microsoftauthorization"></a>Microsoft.Authorization

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | classicadministrators | Hayır | 
> | datatakma adlar | Hayır | 
> | denyasatamaları | Hayır | 
> | erişimi yükseltme | Hayır | 
> | findorphanroleatamalar | Hayır | 
> | kaynaktaki | Hayır | 
> | izinler | Hayır | 
> | poliyasatamaları | Hayır | 
> | PolicyDefinitions | Hayır | 
> | policysetdefinitions | Hayır | 
> | privatelinkassociations | Hayır | 
> | resourcemanagementprivatelinks | Hayır | 
> | roleassignments | Hayır | 
> | roleatamasussusageölçümleri | Hayır | 
> | roledefinitions | Hayır | 

## <a name="microsoftautomation"></a>Microsoft. Automation

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | automationaccounts | Evet (şablonu kullanarak) <br/><br/> [Coğrafi çoğaltma kullanma](../../automation/automation-managing-data.md#geo-replication-in-azure-automation) |  
> | automationaccounts/Configurations | Hayır | 
> | automationaccounts/runbook 'lar | Hayır | 

## <a name="microsoftavs"></a>Microsoft. AVS

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | Abonelik |
> | ------------- | ----------- | 
> | privatebulutlar | Hayır | 


## <a name="microsoftazureactivedirectory"></a>Microsoft. AzureActiveDirectory

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | b2cdirectories | Hayır | 
> | b2ctenants | Hayır | 

## <a name="microsoftazuredata"></a>Microsoft. AzureData

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | veri denetleyicileri | Hayır | 
> | hybriddatayöneticileri | Hayır | 
> | postgresınstances | Hayır | 
> | SQLInstances | Hayır | 
> | sqlmanagedınstances | Hayır |
> | sqlserverınstances | Hayır | 
> | sqlserverkayıtları | Hayır |

## <a name="microsoftazurestack"></a>Microsoft. AzureStack

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | cloudmanifestfiles | Hayır |
> | kayıtlarında | Hayır | 

## <a name="microsoftazurestackhci"></a>Microsoft. Azurestackhcı

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- | 
> | leriniz | Hayır | 

## <a name="microsoftbatch"></a>Microsoft.Batch

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | batchaccounts |  Batch hesapları doğrudan bir bölgeden diğerine taşınamaz, ancak şablonu dışarı aktarmak, değiştirmek ve şablonu yeni bölgeye dağıtmak için bir şablon kullanabilirsiniz. <br/><br/> [Bir Batch hesabını bölgeler arasında taşıma](../../batch/best-practices.md#moving-batch-accounts-across-regions) hakkında bilgi edinin |

## <a name="microsoftbilling"></a>Microsoft. Faturalandırma

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- | 
> | billingaccounts | Hayır | 
> | billingdönemler | Hayır | 
> | billingpermissions | Hayır | 
> | billingproperty | Hayır | 
> | billingroleatamaları | Hayır | 
> | billingroledefinitions | Hayır | 
> | bölümlerinin | Hayır | 
> | kayıt sayısı | Hayır | 
> | faturalardan | Hayır | 
> | girişinde | Hayır | 

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
> | blockchainmembers | Hayır <br/><br/> Blok zinciri ağı farklı bölgelerde düğümlere sahip olamaz. 
> | cordadmembers | Hayır |
> | izleyicileri | Hayır | 

## <a name="microsoftblockchaintokens"></a>Microsoft. BlockchainTokens

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | tokenservices | Hayır |


## <a name="microsoftblueprint"></a>Microsoft. Blueprint

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | şema tasmi | Hayır | 
> | Blueprint | Hayır |

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
> | redisenterprise | Hayır | 

## <a name="microsoftcapacity"></a>Microsoft. Capacity

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- | 
> | appliedreservations | Hayır | 
> | calculateexchange | Hayır | 
> | calculateprice | Hayır | 
> | calculatepurchaseprice | Hayır | 
> | larına | Hayır | 
> | ticari vaalrezervler | Hayır | 
> | değişimi | Hayır |
> | rezervler | Hayır | 
> | oluşturamaz | Hayır | 
> | kaynaklar | Hayır | 
> | validatereservationorder | Hayır | 

## <a name="microsoftcdn"></a>Microsoft. CDN

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | cdnwebapplicationfirewallpolicies | Hayır |
> | edgenodes | Hayır
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
> | özellikler | Hayır | 
> | DomainNames | Yes | Hayır |
> | quotas | Hayır | 
> | resourcetypes | Hayır |
> | validatesubscriptionmoveavaılabılıty | Hayır | 
> | virtualmachines | Hayır 

## <a name="microsoftclassicinfrastructuremigrate"></a>Microsoft. ClassicInfrastructureMigrate

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- | 
> | classicinfrastructureresources | Hayır | 

## <a name="microsoftclassicnetwork"></a>Microsoft. ClassicNetwork

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | özellikler | Hayır | 
> | expressroutecrossconnections | Hayır | 
> | expressroutecrossconnections/peerler | Hayır | 
> | gatewaysupporteddevices | Hayır | 
> | networksecuritygroups | Hayır |
> | quotas | Hayır |
> | rezervler | Hayır | 
> | virtualnetworks | Hayır | 

## <a name="microsoftclassicstorage"></a>Microsoft. ClassicStorage

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | disklerinden | Hayır | 
> | images | Hayır | 
> | osimages | Hayır | 
> | osplatformımages | Hayır | 
> | publicımages | Hayır | 
> | quotas | Hayır | 
> | storageaccounts | Yes |  
> | vmımages | Hayır |

## <a name="microsoftclassicsubscription"></a>Microsoft. ClassicSubscription

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- | 
> | işlemler | Hayır | 

## <a name="microsoftcognitiveservices"></a>Microsoft. Biliveservices

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | accounts | Hayır | 
> | Bilişsel Arama | El ile yapılan adımlarla desteklenir.<br/><br/> [Azure bilişsel arama hizmetinizi başka bir bölgeye taşıma](../../search/search-howto-move-across-regions.md) hakkında bilgi edinin

## <a name="microsoftcommerce"></a>Microsoft. Commerce

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- | 
> | ratecard | Hayır | 
> | usagetoplamaları | Hayır | 

## <a name="microsoftcompute"></a>Microsoft.Compute

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | availabilitysets | Yes <br/><br/> Kullanılabilirlik kümelerini taşımak için [Azure Kaynak taşıyıcısı](../../resource-mover/tutorial-move-region-virtual-machines.md) kullanın. | 
> | diskeriþler | Hayır |
> | diskencryptionsets | Hayır | 
> | disklerinden | Yes <br/><br/> Azure [kaynak taşıyıcısı](../../resource-mover/tutorial-move-region-virtual-machines.md) 'Ni kullanarak Azure VM 'lerini ve ilgili diskleri taşıyın. | 
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
> | sshpublickeys | Hayır |
> | virtualmachines | Yes <br/><br/> Azure sanal makinelerini taşımak için [Azure Kaynak taşıyıcısı](../../resource-mover/tutorial-move-region-virtual-machines.md) kullanın. | 
> | virtualmachines/uzantıları | Hayır | 
> | virtualmachinescalesets | Hayır | 

## <a name="microsoftconsumption"></a>Microsoft. tüketim

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- | 
> | aggregmalyt maliyeti | Hayır | 
> | dengeler | Hayır | 
> | bütçelerinin | Hayır | 
> | ücretleriyle | Hayır | 
> | costtags | Hayır | 
> | iler | Hayır | 
> | etkinlikler | Hayır | 
> | hesaplansın | Hayır | 
> | oluş | Hayır | 
> | marketlerinden | Hayır | 
> | pricesheets | Hayır | 
> | ürün | Hayır | 
> | rezervde ayrıntıları | Hayır | 
> | reservationrecommendationdetails | Hayır | 
> | rezervationönerilere | Hayır | 
> | rezervlerin Özeti | Hayır | 
> | rezervlik işlemleri | Hayır | 
> | etiketler | Hayır | 
> | Kira | Hayır | 
> | larındaki | Hayır | 
> | UsageDetails | Hayır | 


## <a name="microsoftcontainerinstance"></a>Microsoft. Containerınstance

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | kapsayıcı grupları | Hayır | 
> | serviceassociationlinks | Hayır |


## <a name="microsoftcontainerregistry"></a>Microsoft. ContainerRegistry

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | kayıt | Hayır |  
> | kayıt defterleri/agentpools | Hayır | 
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
> | alerts | Hayır | 
> | billingaccounts | Hayır | 
> | bütçelerinin | Hayır | 
> | cloudbağlayıcıları | Hayır | 
> | bağlayıcılar | Hayır | 
> | bölümlerinin | Hayır | 
> | boyutlarına | Hayır | 
> | kayıt sayısı | Hayır | 
> | aktarımları | Hayır | 
> | externalbillingaccounts | Hayır | 
> | tahmin | Hayır | 
> | sorgu | Hayır | 
> | register | Hayır | 
> | reportconfigs | Hayır | 
> | reports | Hayır | 
> | ayarlar | Hayır | 
> | showbackrules | Hayır | 
> | görünümler | Hayır | 

## <a name="microsoftcustomerinsights"></a>Microsoft. Customerınsights

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | hub'lar | Hayır |  

## <a name="microsoftcustomerlockbox"></a>Microsoft. Customerkasası

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- | 
> | istekleri | Hayır | 

## <a name="microsoftcustomproviders"></a>Microsoft. CustomProviders

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | içermektedir | Hayır |
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
> | availablesku 'lar | Hayır |
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

## <a name="microsoftdataprotection"></a>Microsoft. DataProtection

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- | ---------- |
> | backupkasaları | Hayır | 

## <a name="microsoftdatashare"></a>Microsoft. DataShare

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | accounts | Hayır | 

## <a name="microsoftdbformariadb"></a>Microsoft. Dbformarıdb

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | larý | Bir çapraz bölge okuma çoğaltmasını, var olan bir sunucuyu taşımak için kullanabilirsiniz. [Daha fazla bilgi edinin](../../postgresql/howto-move-regions-portal.md).<br/><br/> Hizmet, coğrafi olarak yedekli yedekleme depolaması ile sağlanmışsa, diğer bölgelerde geri yüklemek için coğrafi geri yükleme kullanabilirsiniz. [Daha fazla bilgi edinin](../../mariadb/concepts-business-continuity.md#recover-from-an-azure-regional-data-center-outage).

## <a name="microsoftdbformysql"></a>Microsoft. Dbformyısql

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | larý | Bir çapraz bölge okuma çoğaltmasını, var olan bir sunucuyu taşımak için kullanabilirsiniz. [Daha fazla bilgi edinin](../../mysql/howto-move-regions-portal.md).

## <a name="microsoftdbforpostgresql"></a>Microsoft. DBforPostgreSQL

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | sunucu grupları | Hayır | 
> | larý | Bir çapraz bölge okuma çoğaltmasını, var olan bir sunucuyu taşımak için kullanabilirsiniz. [Daha fazla bilgi edinin](../../postgresql/howto-move-regions-portal.md).
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


## <a name="microsoftdesktopvirtualization"></a>Microsoft. DesktopVirtualization

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- | 
> | applicationgroups | Hayır | 
> | çalışma alanı | Hayır | 

## <a name="microsoftdevices"></a>Microsoft.Devices

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | elaun havuzları | Hayır. Kaynak gösterilmez.
> | elaun havuzları/ıothubkiracılar | Hayır. Kaynak gösterilmez.
> | ıothubs | Evet. [Daha fazla bilgi edinin](../../iot-hub/iot-hub-how-to-clone.md)
> | provisioningservices | Hayır | 

## <a name="microsoftdevops"></a>Microsoft. DevOps

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | denetleyiciler | Hayır | 


## <a name="microsoftdevspaces"></a>Microsoft. DevSpaces

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | denetleyiciler | Hayır | 
> | AKS kümesi | Hayır<br/><br/> Başka bir bölgeye geçme hakkında [daha fazla bilgi edinin](../../dev-spaces/faq.md#can-i-migrate-my-aks-cluster-with-azure-dev-spaces-to-another-region) .

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

## <a name="microsoftdigitaltwins"></a>Microsoft. DigitalTwins

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- | 
> | digitaltwınsınstances | Evet, yeni bölgedeki kaynakları yeniden oluşturarak. [Daha fazla bilgi edinin](../../digital-twins/how-to-move-regions.md) |

## <a name="microsoftdocumentdb"></a>Microsoft.DocumentDB

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | veritabanı hesapları | Hayır | 
> | veritabanı hesapları | Hayır | 

## <a name="microsoftdomainregistration"></a>Microsoft. DomainRegistration

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | etki alanları | Hayır | 
> | generatessorequest | Hayır | 
> | topleveldomains | Hayır | 
> | validatedomainregistrationınformation | Hayır |

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
> | eventabonelikleri | Hayır |
> | extensionkonuları | Hayır | 
> | partnernamespaces | Hayır | 
> | iş ortağı kayıtları | Hayır | 
> | iş ortağı konuları | Hayır | 
> | Sistem konuları | Hayır | 
> | konuları | Hayır | 
> | topictypes | Hayır | 

## <a name="microsofteventhub"></a>Microsoft. EventHub

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | leriniz | Hayır |  
> | öznitelikleri | Evet (şablon ile)<br/><br/> [Bir olay hub 'ı ad alanını başka bir bölgeye taşıma](../../event-hubs/move-across-regions.md) | 
> | isteyin | Hayır |  

## <a name="microsoftexperimentation"></a>Microsoft. deneme

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- | 
> | experimentworkspaces | Hayır | 

## <a name="microsoftfalcon"></a>Microsoft. Falcon

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- | 
> | öznitelikleri | Hayır | 

## <a name="microsoftfeatures"></a>Microsoft. Features

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- | 
> | özellik sağlayıcıları | Hayır | 
> | özellikler | Hayır | 
> | sağlayıcılarla | Hayır | 
> | subscriptionfeatureregistrations | Hayır | 

## <a name="microsoftgenomics"></a>Microsoft. Genomiks

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | accounts | Hayır | 

## <a name="microsoftguestconfiguration"></a>Microsoft. GuestConfiguration

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- | 
> | Oto managedaccounts | Hayır | 
> | Oto managedvmconfigurationprofiles | Hayır | 
> | guestconfigurationatamaları | Hayır | 
> | yazılımıdır | Hayır | 
> | softwareupdateprofile | Hayır | 
> | softwareupdates | Hayır | 

## <a name="microsofthanaonazure"></a>Microsoft. HanaOnAzure

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | hanaınstances | Hayır | 
> | sapizleyicileri | Hayır |  

## <a name="microsofthardwaresecuritymodules"></a>Microsoft. HardwareSecurityModules

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- | 
> | ayrılmış atedhsms | Hayır | 


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
> | makineler/uzantılar | Hayır |

## <a name="microsofthybriddata"></a>Microsoft. HybridData

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | veri yöneticileri |  Hayır | 

## <a name="microsofthybridnetwork"></a>Microsoft. HybridNetwork

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- | 
> | cihazlar | Hayır | 
> | vnfs | Hayır | 

## <a name="microsofthydra"></a>Microsoft. Hydra

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- | 
> | bileşenleri | Hayır | 
> | networkscopes | Hayır | 

## <a name="microsoftimportexport"></a>Microsoft. ımportexport

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | Çizelge |  Hayır | 

## <a name="microsoftinsights"></a>Microsoft. Insights

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | accounts | Hayır. [Daha fazla bilgi edinin](../../azure-monitor/faq.md#how-do-i-move-an-application-insights-resource-to-a-new-region).
> | actiongroups |  Hayır | 
> | activitylogalerts | Hayır | 
> | alertrules |  Hayır | 
> | autoscalesettings |  Hayır | 
> | temel | Hayır |
> | bileşenleri |  Hayır |  
> | datacollectionrules | Hayır | 
> | diagnosticsettings | Hayır | 
> | diagnosticsettingscategories | Hayır | 
> | eventcategories | Hayır | 
> | eventTypes | Hayır | 
> | extendeddiagnosticsettings | Hayır | |
> | guestdiagnosticsettings | Hayır | 
> | listmigrationdate | Hayır | 
> | logdefinitions | Hayır | 
> | günlüğe kaydetme profilleri | Hayır | 
> | günlükler | Hayır | Hayır |
> | metricalerts | Hayır | 
> | metrictemeller | Hayır | 
> | metricbatch | Hayır | 
> | MetricDefinitions | Hayır | 
> | metricnamespaces | Hayır | 
> | metrics | Hayır | 
> | migratealertrules | Hayır |
> | migratetonewpricingmodel | Hayır | 
> | çalışma kitapları | Hayır |
> | notificationgroups | Hayır | 
> | privatelinkscopes | Hayır |
> | rollbacktolegacypricingmodel | Hayır |
> | scheduledqueryrules |  Hayır | 
> | topology | Hayır |
> | işlemler | Hayır |
> | vminsightsonboardingstatuses | Hayır |
> | Web testleri |  Hayır | 
> | Web testleri/gettestresultfile | Hayır |
> | çalışma kitapları |  Hayır |  
> | workbooktemplates | Hayır |


## <a name="microsoftiotcentral"></a>Microsoft. ıotcentral

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | apptemplates | Hayır | 
> | ıotapps | Hayır | 



## <a name="microsoftiothub"></a>Microsoft.IoTHub

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> |  ıothub |  Evet (kopya hub 'ı) <br/><br/> [IoT Hub 'ını başka bir bölgeye kopyalama](../../iot-hub/iot-hub-how-to-clone.md)

## <a name="microsoftiotspaces"></a>Microsoft. ıotspaces

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma |
> | ------------- | ----------- | 
> | çıkarılamıyor | Hayır | 

## <a name="microsoftkeyvault"></a>Microsoft. Keykasası

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | silinkaults | Hayır |
> | hsmpools | Hayır | 
> | managedhsms | Hayır |
> | kasaları |  Hayır | 

## <a name="microsoftkubernetes"></a>Microsoft. Kubernetes

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- | 
> | connectedkümeler | Hayır | 
> | registeredsubscriptions | Hayır | 

## <a name="microsoftkubernetesconfiguration"></a>Microsoft. KubernetesConfiguration

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- | 
> | sourcecontrolconfigurations | Hayır | 

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
> | kullanıcılar | Hayır | 

## <a name="microsoftlocationbasedservices"></a>Microsoft. LocationBasedServices

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | accounts | Hayır | 

## <a name="microsoftlocationservices"></a>Microsoft. LocationServices

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | accounts | Hayır, küresel bir hizmettir.

## <a name="microsoftlogic"></a>Microsoft. Logic

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | hostingenvironments | Hayır | 
> | Tümleştirme hesapları |  Hayır |  
> | ıntegrationserviceortamortamları | Hayır | 
> | ıntegrationserviceortamortamları/managedap | Hayır |
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
> | teamaccounts | Hayır | 


## <a name="microsoftmachinelearningmodelmanagement"></a>Microsoft. MachineLearningModelManagement

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | accounts | Hayır | 


## <a name="microsoftmachinelearningservices"></a>Microsoft.MachineLearningServices

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | çalışma alanı | Hayır | 

## <a name="microsoftmaintenance"></a>Microsoft. Maintenance

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | configurationatamalar | Evet. [Daha fazla bilgi edinin](../../virtual-machines/move-region-maintenance-configuration.md) | 
> | maintenanceconfigurations | Evet. [Daha fazla bilgi edinin](../../virtual-machines/move-region-maintenance-configuration-resources.md) |
> | güncelleştirmeler | Hayır | 

## <a name="microsoftmanagedidentity"></a>Microsoft. Managedıdentity

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | lerinizde | Hayır | 
> | userassignedıdentities | Hayır | 

## <a name="microsoftmanagednetwork"></a>Microsoft. ManagedNetwork

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- | 
> | managednetworks | Hayır | 
> | managednetworks/managednetworkgroups | Hayır |
> | managednetworks/managednetworkpeeringpolicies | Hayır | 
> | bildirim | Hayır | 

## <a name="microsoftmanagedservices"></a>Microsoft. ManagedServices

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- | 
> | Pazar placeryumuristrationdefinitions | Hayır | 
> | registrationatamaları | Hayır |
> | registrationdefinitions | Hayır | 

## <a name="microsoftmanagement"></a>Microsoft. Management

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- | 
> | getentities | Hayır | 
> | managementgroups | Hayır | 
> | managementgroups/Settings | Hayır | 
> | kaynaklar | Hayır | 
> | starttenantbackfill | Hayır | 
> | tenantbackfillstatus | Hayır | 

## <a name="microsoftmaps"></a>Microsoft. Maps

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | accounts |  Hayır, Azure Maps bir jeo-uzamsal hizmettir. 
> | hesaplar/privateatlases | Hayır

## <a name="microsoftmarketplace"></a>Microsoft. Market

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | sunar | Hayır | 
> | offertypes | Hayır | 
> | privategallergıtems | Hayır | 
> | privatestoreclient | Hayır | 
> | privatemağazaların | Hayır | 
> | ürün | Hayır | 
> | Publishers | Hayır | 
> | register | Hayır | 

## <a name="microsoftmarketplaceapps"></a>Microsoft. MarketplaceApps

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- | 
> | classicdevservices | Hayır | 

## <a name="microsoftmarketplaceordering"></a>Microsoft. Marketplacesıralaması

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- | 
> | anlaşmalar | Hayır | 
> | offertypes | Hayır | 

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
> | movecollections | Hayır
> | projeyle | Hayır | 

## <a name="microsoftmixedreality"></a>Microsoft. MixedReality

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- | ---------- |
> | holographicsbroadcastaccounts | Hayır | 
> | objectunderstandingaccounts | Hayır | 
> | remoterenderingaccounts | Hayır | 
> | spatialanchorsaccounts | Hayır | 

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
> | bgpservicecommunities | Hayır |
> | bağlantının |  Hayır | 
> | ddoscustompolicies |  Hayır | 
> | ddosprotectionplanlar | Hayır | 
> | dnszones |  Hayır | 
> | expressroutedevreleri | Hayır | 
> | expressroutegateway 'ler | Hayır | 
> | expressrouteserviceproviders | Hayır | 
> | firewallpolicies | Hayır |
> | frontkapıların | Hayır | 
> | ipayırmalar | Hayır |
> | ipgroups | Hayır |
> | loadbalancers | Yes <br/><br/> [Azure Kaynak taşıyıcısı](../../resource-mover/tutorial-move-region-virtual-machines.md) 'ni kullanarak iç ve dış yük dengeleyicileri taşıyın. |
> | localnetworkgateway 'ler |  Hayır | 
> | natgateway 'ler |  Hayır | 
> | networkexperimentprofiles | Hayır |
> | networkıntpolicies ilkeleri |  Hayır | 
> | NetworkInterfaces | Yes <br/><br/> NIC 'Leri taşımak için [Azure Kaynak taşıyıcısı](../../resource-mover/tutorial-move-region-virtual-machines.md) kullanın. | 
> | networkprofiles | Hayır | 
> | networksecuritygroups | Yes <br/><br/> Ağ güvenlik gruplarını (NGSs) taşımak için [Azure Kaynak taşıyıcısı](../../resource-mover/tutorial-move-region-virtual-machines.md) kullanın. | 
> | networkwatchers |  Hayır |  
> | networkwatchers/connectionmonitörleri |  Hayır | 
> | networkwatchers/flowlogs |  Hayır | 
> | networkwatchers/pingkafesler |  Hayır | 
> | p2svpngateways | Hayır | 
> | privatednszones |  Hayır |  
> | privatednszones/virtualnetworklinks | Hayır |> | privatednszonesinternal | Hayır |
> | privateendpointredirectmaps | Hayır |
> | privateendpoints | Hayır | 
> | privatelinkservices | Hayır | 
> | publicıpaddresses | Yes<br/><br/> Genel IP adreslerini taşımak için [Azure Kaynak taşıyıcısı](../../resource-mover/tutorial-move-region-virtual-machines.md) kullanın. |
> | publicıpöneklerini | Hayır | 
> | routefilters | Hayır | 
> | routetables |  Hayır | 
> | securitypartnerproviders | Hayır |
> | serviceendpointpolicies |  Hayır | 
> | trafficmanagergeographichierarchies | Hayır | 
> | trafficmanagerprofiles |  Hayır | 
> | trafficmanagerusermetricskeys | Hayır |
> | virtualhub 'lar | Hayır | 
> | virtualnetworkgateways |  Hayır |  
> | virtualnetworks |  Hayır | 
> | virtualnetworktaps | Hayır | 
> | virtualwan | Hayır | 
> | vpngateway (sanal WAN) | Hayır | 
> | vpnsites (sanal WAN) | Hayır | 
> | vpnsites (sanal WAN) | Hayır |


## <a name="microsoftnotificationhubs"></a>Microsoft. Notificationhub 'Lar

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | öznitelikleri |  Hayır | 
> | ad alanları/notificationhub 'lar |  Hayır |  

## <a name="microsoftobjectstore"></a>Microsoft. ObjectStore

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- | 
> | osnamespaces | Hayır | 

## <a name="microsoftoffazure"></a>Microsoft. OffAzure

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- | 
> | Hiper sanal siteler | Hayır | 
> | ımportsites | Hayır | 
> | sunucusiteleri | Hayır | 
> | vmwaresites | Hayır | 

## <a name="microsoftoperationalinsights"></a>Microsoft. Operationalınsights

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | leriniz | Hayır | 
> | deletedworkspaces | Hayır | 
> | bağlantı hedefleri | Hayır | 
> | storageınsii configs | Hayır |
> | çalışma alanı | Hayır |



## <a name="microsoftoperationsmanagement"></a>Microsoft. OperationsManagement

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | managementassociations | Hayır |
> | managementconfigurations |  Hayır | 
> | çözümler | Hayır |
> | görünümler |  Hayır | 

## <a name="microsoftpeering"></a>Microsoft. eşleme

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | Yasallıklar | Hayır | 
> | peerasns | Hayır | 
> | peeringlocations | Hayır | 
> | eşlemeleri | Hayır | 
> | peeringserviceülkeleriyle | Hayır | 
> | peeringserviceloing | Hayır | 
> | peeringserviceproviders | Hayır | 
> | peeringservices | Hayır | 

## <a name="microsoftpolicyinsights"></a>Microsoft. Poliyelei

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- | 
> | poliyevents | Hayır | 
> | policystates | Hayır | 
> | policytrackedresources | Hayır | 
> | düzeltmeler | Hayır | 

## <a name="microsoftportal"></a>Microsoft. Portal

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> |  konsolları | Hayır |
> | panolar | Hayır | 
> | UserSettings | Hayır | 


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

## <a name="microsoftprojectbabylon"></a>Microsoft. ProjectBabylon

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | accounts | Hayır | 

## <a name="microsoftproviderhub"></a>Microsoft. ProviderHub

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- | 
> | availableaccounts | Hayır | 
> | providerkayıtları | Hayır | 
> | piyasaya çıkarma | Hayır | 

## <a name="microsoftquantum"></a>Microsoft. hisse

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- | 
> | çalışma alanı | Hayır | 

## <a name="microsoftrecoveryservices"></a>Microsoft. RecoveryServices

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | replicationeligibilityresults | Hayır |
> | kasaları | Hayır.<br/><br/> Azure bölgeleri arasında Azure Backup için kurtarma hizmetleri kasalarının taşınması desteklenmez.<br/><br/> Azure Site Recovery için kurtarma hizmetleri kasalarında, kasayı hedef bölgede [devre dışı bırakıp yeniden](../../site-recovery/move-vaults-across-regions.md) oluşturabilirsiniz. | 

## <a name="microsoftredhatopenshift"></a>Microsoft. RedHatOpenShift

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- | 
> | openshiftclusters | Hayır | 

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
> | resourcechangedetails | Hayır | 
> | resourcechanges | Hayır | 
> | kaynaklar | Hayır | 
> | resourceshistory | Hayır | 
> | subscriptionsstatus | Hayır | 

## <a name="microsoftresourcehealth"></a>Microsoft. ResourceHealth

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- | 
> | childresources | Hayır | 
> | acil sorun sorunları | Hayır | 
> | etkinlikler | Hayır | 
> | meta veriler | Hayır | 
> | bildirimler | Hayır | 

## <a name="microsoftresources"></a>Microsoft. resources

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma |
> | ------------- | ----------- |
> | deploymentScripts |  Yes<br/><br/>[Microsoft. resources kaynaklarını yeni bölgeye taşıma](microsoft-resources-move-regions.md) |
> | Templatespec |  Yes<br/><br/>[Microsoft. resources kaynaklarını yeni bölgeye taşıma](microsoft-resources-move-regions.md) |  

## <a name="microsoftsaas"></a>Microsoft. SaaS

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | uygulamalar |  Hayır | 
> | saasresources | Hayır | 


## <a name="microsoftsearch"></a>Microsoft. Search

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | resourcehealthmetadata | Hayır |
> | searchservices |  Hayır | 


## <a name="microsoftsecurity"></a>Microsoft.Security

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | adaptivenetworkhardenings | Hayır | 
> | advancedthreatprotectionsettings | Hayır | 
> | alerts | Hayır | 
> | allowedconnections | Hayır | 
> | applicationwhitedökümler | Hayır | 
> | assessmentmetadata | Hayır | 
> | değerlendirme | Hayır | 
> | Oto dissalertsrules | Hayır | 
> | akışlarını otomatikleştirin | Hayır | 
> | Oto provisioningsettings | Hayır |
> | karmaşık anceresults | Hayır | 
> | uyumluluklarına | Hayır | 
> | datacollectionagents | Hayır | 
> | devicesecuritygroups | Hayır | 
> | discoveredsecuritysolutions | Hayır | 
> | externalsecuritysolutions | Hayır | 
> | ınformationprotectionpolicies | Hayır | 
> | ıotsecuritysolutions | Hayır | 
> | ıotsecuritysolutions/analiz ticsmodeller | Hayır | 
> | ıotsecuritysolutions/analticsmodeller/aggreggıt uyarıları | Hayır | 
> | ıotsecuritysolutions/analiz ticsmodeller/Aggreg, öneriler | Hayır | 
> | jağaccesspolicies | Hayır | 
> | ilkeler | Hayır | 
> | fiyatlandırmalar | Hayır | 
> | Reve daha karmaşık bakım standartları | Hayır | 
> | rekontrol ve Re, | Hayır | 
> | rekontrol ve Re, Re, Re, | Hayır | 
> | securitycontacts | Hayır | 
> | securitysolutions | Hayır | 
> | securitysolutionsreferencedata | Hayır | 
> | securitydurumlardan | Hayır | 
> | securitystatusessummaries | Hayır | 
> | sunucukullanılabilirliği | Hayır | 
> | ayarlar | Hayır | 
> | subassessments | Hayır |
> | Görevler | Hayır | 
> | anlatır | Hayır | 
> | çalışma alanı ayarları | Hayır | 

## <a name="microsoftsecurityinsights"></a>Microsoft. Securityınsights

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- | 
> | toplamaları | Hayır | 
> | alertrules | Hayır | 
> | alertrutatemplates | Hayır | 
> | automationrules | Hayır |
> | çalışmaların | Hayır | 
> | veri bağlayıcıları | Hayır | 
> | varlıklar | Hayır | 
> | entityqueries | Hayır |
> | olaylar | Hayır | 
> | officeconsents | Hayır | 
> | ayarlar | Hayır | 
> | threatıntelligence | Hayır | 

## <a name="microsoftserialconsole"></a>Microsoft. SerialConsole

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- | 
> | consoleservices | Hayır | 

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
> | premiummessagingregions | Hayır | 
> | isteyin | Hayır | 

## <a name="microsoftservicefabric"></a>Microsoft. ServiceFabric

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | uygulamalar | Hayır | 
> | leriniz |  Hayır |  
> | kapsayıcı grupları | Hayır | 
> | containergroupsets | Hayır | 
> | edgeclusters | Hayır | 
> | managedkümeler | Hayır |
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

## <a name="microsoftservices"></a>Microsoft. Services

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- | 
> | piyasaya çıkarma | Hayır | 

## <a name="microsoftsignalrservice"></a>Microsoft. SignalRService

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | SignalR |  Hayır |  

## <a name="microsoftsoftwareplan"></a>Microsoft. SoftwarePlan

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- | 
> | hybriduseavantajlar | Hayır | 

## <a name="microsoftsolutions"></a>Microsoft. Solutions

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | appliancedefinitions | Hayır | 
> | yaptı | Hayır | 
> | jistekleri | Hayır | 

## <a name="microsoftsql"></a>Microsoft.Sql

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | ınstancepools | Hayır | 
> | yerlerini | Hayır |
> | ManagedInstances | Yes <br/><br/> Yönetilen örnekleri bölgeler arasında taşıma hakkında [daha fazla bilgi edinin](../../azure-sql/database/move-resources-across-regions.md) . | 
> | ManagedInstances/veritabanları | Yes | 
> | larý | Yes | 
> | sunucular/veritabanları | Yes <br/><br/> Veritabanlarını bölgeler arasında taşıma hakkında [daha fazla bilgi edinin](../../azure-sql/database/move-resources-across-regions.md) .<br/><br/> Azure SQL veritabanlarını taşımak için Azure Kaynak taşıyıcısı kullanma hakkında [daha fazla bilgi edinin](../../resource-mover/tutorial-move-region-sql.md) .  | 
> | sunucular/elaun havuzları | Yes <br/><br/> Elastik havuzları bölgeler arasında taşıma hakkında [daha fazla bilgi edinin](../../azure-sql/database/move-resources-across-regions.md) .<br/><br/> Azure SQL elastik havuzlarını taşımak için Azure Kaynak taşıyıcısı kullanma hakkında [daha fazla bilgi edinin](../../resource-mover/tutorial-move-region-sql.md) .  | 
> | virtualkümeler | Yes | 

## <a name="microsoftsqlvirtualmachine"></a>Microsoft. SqlVirtualMachine

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | sqlvirtualmachinegroups |  Hayır |  
> | sqlvirtualmachines |  Hayır |  


## <a name="microsoftstorage"></a>Microsoft.Storage

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | storageaccounts | Yes<br/><br/> [Bir Azure Depolama hesabını başka bir bölgeye taşıma](../../storage/common/storage-account-move.md) | 

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
> | leriniz | Hayır |
> | streammingjobs |  Hayır |  


## <a name="microsoftstreamanalyticsexplorer"></a>Microsoft. StreamAnalyticsExplorer

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | lý | Hayır | 
> | larında | Hayır | 

## <a name="microsoftsubscription"></a>Microsoft. Subscription

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- | 
> | Aboneliklerin | Hayır | 

## <a name="microsoftsupport"></a>Microsoft. support

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- | 
> | services | Hayır | 
> | destek biletleri | Hayır | 

## <a name="microsoftsynapse"></a>Microsoft. SYNAPSE

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- | 
> | çalışma alanı | Hayır | 
> | çalışma alanları/bigdatapools | Hayır | 
> | çalışma alanları/sqlpools | Hayır | 


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

## <a name="microsoftvmware"></a>Microsoft. VMware

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- | 
> | arczones | Hayır | 
> | ResourcePools | Hayır | 
> | vCenter | Hayır | 
> | virtualmachines | Hayır | 
> | virtualmachinetemplates | Hayır | 
> | virtualnetworks | Hayır | 

## <a name="microsoftvmwarecloudsimple"></a>Microsoft. Vmwarechoparlör basit

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | ayrılmış cloudnodes | Hayır | 
> | ayrılmış CloudService | Hayır | 
> | virtualmachines | Hayır | 

## <a name="microsoftvnfmanager"></a>Microsoft. VnfManager

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- | 
> | cihazlar | Hayır | 
> | vnfs | Hayır | 

## <a name="microsoftvsonline"></a>Microsoft. VSOnline

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- | 
> | accounts | Hayır | 
> | Planlama | Hayır | 
> | registeredsubscriptions | Hayır |


## <a name="microsoftweb"></a>Microsoft. Web

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | availableyığınları | Hayır | 
> | billingmeters | Hayır | 
> | sertifikalar | Hayır | 
> | connectiongateway 'ler |  Hayır |  
> | bağlantının |  Hayır |  
> | customapsıs |  Hayır | 
> | silinmi siteleri | Hayır | 
> | deploymentlocations | Hayır | 
> | coğrafi bölgeler | Hayır | 
> | hostingenvironments | Hayır | 
> | kubeortamları | Hayır | 
> | publishingusers | Hayır |
> | Öneriler | Hayır | 
> | resourcehealthmetadata | Hayır | 
> | zamanları | Hayır | 
> | serverfarms | Hayır |  
> | sunucugrupları/eventgridfilters | N
> | Siteler |  Hayır | 
> | siteler/premieraddons |  Hayır |  
> | siteler/yuvalar |  Hayır |  
> | sourcecontrols | Hayır |
> | staticsites | Hayır | 

## <a name="microsoftwindowsesu"></a>Microsoft. WindowsESU

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- | 
> | çoğulactivationkeys | Hayır |

## <a name="microsoftwindowsiot"></a>Microsoft. Windowsıot

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | deviceservices | Hayır | 

## <a name="microsoftworkloadbuilder"></a>Microsoft. WorkloadBuilder

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- | 
> | lerine | Hayır | 

## <a name="microsoftworkloadmonitor"></a>Microsoft. WorkloadMonitor

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- | 
> | bileşenleri | Hayır |
> | componentssummary | Hayır | 
> | izleme örnekleri | Hayır | 
> | izleme ınstancessummary | Hayır | 
> | monitörün | Hayır | 
## <a name="third-party-services"></a>Üçüncü taraf hizmetleri

Üçüncü taraf hizmetler şu anda taşıma işlemini desteklemiyor.

## <a name="next-steps"></a>Sonraki adımlar

Kaynak taşıyıcısı hizmeti hakkında [daha fazla bilgi edinin](../../resource-mover/overview.md) .

