---
title: Bölgeler arasında Azure kaynaklarını taşıma desteği
description: Azure bölgeleri arasında taşınabilecek Azure Kaynak türlerini listeler
author: rayne-wiselman
ms.service: azure-resource-manager
ms.topic: reference
ms.date: 08/25/2020
ms.author: raynew
ms.openlocfilehash: 18d4d84462d528b718d784ff6a16ecf990ed0d20
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "100094024"
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
> - [Microsoft. purview](#microsoftpurview)
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
> | DomainServices | No | 


## <a name="microsoftaadiam"></a>Microsoft. aadihar

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | diagnosticsettings | No |
> | diagnosticsettingscategories | No |
> | privatelinkforazuread | No |
> | Kira |  No |

## <a name="microsoftaddons"></a>MICROSOFT. Addons

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | destek sağlayıcıları | No |

## <a name="microsoftadhybridhealthservice"></a>Microsoft. ADHybridHealthService

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- | 
> | aadsupportcases | No |
> | addsservices | No | 
> | aracısını | No | 
> | anonymousapiusers | No |
> | yapılandırma | No | 
> | günlükler | No | 
> | reports | No | 
> | servicehealthölçümleri | No | 
> | services | No | 

## <a name="microsoftadvisor"></a>Microsoft. Advisor

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- | 
> | konfigürasyonları | No | 
> | generatereyorumgeçişleri | No |
> | meta veriler | No |
> | Öneriler | No |
> | gizlemeleri | No | 

## <a name="microsoftalertsmanagement"></a>Microsoft. AlertsManagement

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | actionrules | No | 
> | alerts | No | 
> | alertslist | No | 
> | alertsmetadata | No | 
> | alertssummary | No | 
> | alertssummarylist | No | 
> | smartdetectoralertrules | No | 
> | smartgroups | No | 

## <a name="microsoftanalysisservices"></a>Microsoft. AnalysisServices

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | larý | No |

## <a name="microsoftapimanagement"></a>Microsoft. Apimanane

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | reportfeedback | No |
> | hizmet |  Evet (şablonu kullanarak) <br/><br/> [API Management bölgeler arasında taşıyın](../../api-management/api-management-howto-migrate.md). | 

## <a name="microsoftappconfiguration"></a>Microsoft. AppConfiguration

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | configurationmağazaların | No | 
> | configurationmağazaların/eventgridfilters | No |

## <a name="microsoftappplatform"></a>Microsoft. AppPlatform

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- | 
> | yay | No | 

## <a name="microsoftappservice"></a>Microsoft. AppService

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | apiapps | Evet (şablonu kullanarak)<br/><br/> [App Service uygulamasını başka bir bölgeye taşıma](../../app-service/manage-move-across-regions.md) | 
> | appdentities | No | 
> | geçidinin | No | 

## <a name="microsoftattestation"></a>Microsoft. kanıtlama

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- | 
> | attestationproviders | No | 

## <a name="microsoftauthorization"></a>Microsoft.Authorization

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | classicadministrators | No | 
> | datatakma adlar | No | 
> | denyasatamaları | No | 
> | erişimi yükseltme | No | 
> | findorphanroleatamalar | No | 
> | kaynaktaki | No | 
> | izinler | No | 
> | poliyasatamaları | No | 
> | PolicyDefinitions | No | 
> | policysetdefinitions | No | 
> | privatelinkassociations | No | 
> | resourcemanagementprivatelinks | No | 
> | roleassignments | No | 
> | roleatamasussusageölçümleri | No | 
> | roledefinitions | No | 

## <a name="microsoftautomation"></a>Microsoft. Automation

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | automationaccounts | Evet (şablonu kullanarak) <br/><br/> [Coğrafi çoğaltma kullanma](../../automation/automation-managing-data.md#geo-replication-in-azure-automation) |  
> | automationaccounts/Configurations | No | 
> | automationaccounts/runbook 'lar | No | 

## <a name="microsoftavs"></a>Microsoft. AVS

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | Abonelik |
> | ------------- | ----------- | 
> | privatebulutlar | No | 


## <a name="microsoftazureactivedirectory"></a>Microsoft. AzureActiveDirectory

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | b2cdirectories | No | 
> | b2ctenants | No | 

## <a name="microsoftazuredata"></a>Microsoft. AzureData

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | veri denetleyicileri | No | 
> | hybriddatayöneticileri | No | 
> | postgresınstances | No | 
> | SQLInstances | No | 
> | sqlmanagedınstances | No |
> | sqlserverınstances | No | 
> | sqlserverkayıtları | No |

## <a name="microsoftazurestack"></a>Microsoft. AzureStack

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | cloudmanifestfiles | No |
> | kayıtlarında | No | 

## <a name="microsoftazurestackhci"></a>Microsoft. Azurestackhcı

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- | 
> | leriniz | No | 

## <a name="microsoftbatch"></a>Microsoft.Batch

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | batchaccounts |  Batch hesapları doğrudan bir bölgeden diğerine taşınamaz, ancak şablonu dışarı aktarmak, değiştirmek ve şablonu yeni bölgeye dağıtmak için bir şablon kullanabilirsiniz. <br/><br/> [Bir Batch hesabını bölgeler arasında taşıma](../../batch/best-practices.md#moving-batch-accounts-across-regions) hakkında bilgi edinin |

## <a name="microsoftbilling"></a>Microsoft. Faturalandırma

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- | 
> | billingaccounts | No | 
> | billingdönemler | No | 
> | billingpermissions | No | 
> | billingproperty | No | 
> | billingroleatamaları | No | 
> | billingroledefinitions | No | 
> | bölümlerinin | No | 
> | kayıt sayısı | No | 
> | faturalardan | No | 
> | girişinde | No | 

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
> | cordadmembers | No |
> | izleyicileri | No | 

## <a name="microsoftblockchaintokens"></a>Microsoft. BlockchainTokens

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | tokenservices | No |


## <a name="microsoftblueprint"></a>Microsoft. Blueprint

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | şema tasmi | No | 
> | Blueprint | No |

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
> | redisenterprise | No | 

## <a name="microsoftcapacity"></a>Microsoft. Capacity

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- | 
> | appliedreservations | No | 
> | calculateexchange | No | 
> | calculateprice | No | 
> | calculatepurchaseprice | No | 
> | larına | No | 
> | ticari vaalrezervler | No | 
> | değişimi | No |
> | rezervler | No | 
> | oluşturamaz | No | 
> | kaynaklar | No | 
> | validatereservationorder | No | 

## <a name="microsoftcdn"></a>Microsoft. CDN

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | cdnwebapplicationfirewallpolicies | No |
> | edgenodes | No
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
> | özellikler | No | 
> | DomainNames | Yes | Hayır |
> | quotas | No | 
> | resourcetypes | No |
> | validatesubscriptionmoveavaılabılıty | No | 
> | virtualmachines | No 

## <a name="microsoftclassicinfrastructuremigrate"></a>Microsoft. ClassicInfrastructureMigrate

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- | 
> | classicinfrastructureresources | No | 

## <a name="microsoftclassicnetwork"></a>Microsoft. ClassicNetwork

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | özellikler | No | 
> | expressroutecrossconnections | No | 
> | expressroutecrossconnections/peerler | No | 
> | gatewaysupporteddevices | No | 
> | networksecuritygroups | No |
> | quotas | No |
> | rezervler | No | 
> | virtualnetworks | No | 

## <a name="microsoftclassicstorage"></a>Microsoft. ClassicStorage

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | disklerinden | No | 
> | images | No | 
> | osimages | No | 
> | osplatformımages | No | 
> | publicımages | No | 
> | quotas | No | 
> | storageaccounts | Yes |  
> | vmımages | No |

## <a name="microsoftclassicsubscription"></a>Microsoft. ClassicSubscription

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- | 
> | işlemler | No | 

## <a name="microsoftcognitiveservices"></a>Microsoft. Biliveservices

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | accounts | No | 
> | Bilişsel Arama | El ile yapılan adımlarla desteklenir.<br/><br/> [Azure bilişsel arama hizmetinizi başka bir bölgeye taşıma](../../search/search-howto-move-across-regions.md) hakkında bilgi edinin

## <a name="microsoftcommerce"></a>Microsoft. Commerce

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- | 
> | ratecard | No | 
> | usagetoplamaları | No | 

## <a name="microsoftcompute"></a>Microsoft.Compute

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | availabilitysets | Yes <br/><br/> Kullanılabilirlik kümelerini taşımak için [Azure Kaynak taşıyıcısı](../../resource-mover/tutorial-move-region-virtual-machines.md) kullanın. | 
> | diskeriþler | No |
> | diskencryptionsets | No | 
> | disklerinden | Yes <br/><br/> Azure [kaynak taşıyıcısı](../../resource-mover/tutorial-move-region-virtual-machines.md) 'Ni kullanarak Azure VM 'lerini ve ilgili diskleri taşıyın. | 
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
> | sshpublickeys | No |
> | virtualmachines | Yes <br/><br/> Azure sanal makinelerini taşımak için [Azure Kaynak taşıyıcısı](../../resource-mover/tutorial-move-region-virtual-machines.md) kullanın. | 
> | virtualmachines/uzantıları | No | 
> | virtualmachinescalesets | No | 

## <a name="microsoftconsumption"></a>Microsoft. tüketim

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- | 
> | aggregmalyt maliyeti | No | 
> | dengeler | No | 
> | bütçelerinin | No | 
> | ücretleriyle | No | 
> | costtags | No | 
> | iler | No | 
> | etkinlikler | No | 
> | hesaplansın | No | 
> | oluş | No | 
> | marketlerinden | No | 
> | pricesheets | No | 
> | ürün | No | 
> | rezervde ayrıntıları | No | 
> | reservationrecommendationdetails | No | 
> | rezervationönerilere | No | 
> | rezervlerin Özeti | No | 
> | rezervlik işlemleri | No | 
> | etiketler | No | 
> | Kira | No | 
> | larındaki | No | 
> | UsageDetails | No | 


## <a name="microsoftcontainerinstance"></a>Microsoft. Containerınstance

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | kapsayıcı grupları | No | 
> | serviceassociationlinks | No |


## <a name="microsoftcontainerregistry"></a>Microsoft. ContainerRegistry

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | kayıt | No |  
> | kayıt defterleri/agentpools | No | 
> | kayıt defterleri/BuildTasks | No |  
> | kayıt defterleri/çoğaltmalar | No | 
> | kayıt defterleri/görevler | No |  
> | kayıt defterleri/Web kancaları | No | 

## <a name="microsoftcontainerservice"></a>Microsoft. ContainerService

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | containerservices | No |
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
> | alerts | No | 
> | billingaccounts | No | 
> | bütçelerinin | No | 
> | cloudbağlayıcıları | No | 
> | bağlayıcılar | No | 
> | bölümlerinin | No | 
> | boyutlarına | No | 
> | kayıt sayısı | No | 
> | aktarımları | No | 
> | externalbillingaccounts | No | 
> | tahmin | No | 
> | sorgu | No | 
> | register | No | 
> | reportconfigs | No | 
> | reports | No | 
> | ayarlar | No | 
> | showbackrules | No | 
> | görünümler | No | 

## <a name="microsoftcustomerinsights"></a>Microsoft. Customerınsights

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | hub'lar | No |  

## <a name="microsoftcustomerlockbox"></a>Microsoft. Customerkasası

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- | 
> | istekleri | No | 

## <a name="microsoftcustomproviders"></a>Microsoft. CustomProviders

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | içermektedir | No |
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
> | availablesku 'lar | No |
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

## <a name="microsoftdataprotection"></a>Microsoft. DataProtection

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- | ---------- |
> | backupkasaları | No | 

## <a name="microsoftdatashare"></a>Microsoft. DataShare

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | accounts | No | 

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
> | sunucu grupları | No | 
> | larý | Bir çapraz bölge okuma çoğaltmasını, var olan bir sunucuyu taşımak için kullanabilirsiniz. [Daha fazla bilgi edinin](../../postgresql/howto-move-regions-portal.md).
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


## <a name="microsoftdesktopvirtualization"></a>Microsoft. DesktopVirtualization

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- | 
> | applicationgroups | No | 
> | çalışma alanı | No | 

## <a name="microsoftdevices"></a>Microsoft.Devices

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | elaun havuzları | Hayır. Kaynak gösterilmez.
> | elaun havuzları/ıothubkiracılar | Hayır. Kaynak gösterilmez.
> | ıothubs | Evet. [Daha fazla bilgi edinin](../../iot-hub/iot-hub-how-to-clone.md)
> | provisioningservices | No | 

## <a name="microsoftdevops"></a>Microsoft. DevOps

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | denetleyiciler | No | 


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

## <a name="microsoftdigitaltwins"></a>Microsoft. DigitalTwins

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- | 
> | digitaltwınsınstances | Evet, yeni bölgedeki kaynakları yeniden oluşturarak. [Daha fazla bilgi edinin](../../digital-twins/how-to-move-regions.md) |

## <a name="microsoftdocumentdb"></a>Microsoft.DocumentDB

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | veritabanı hesapları | No | 
> | veritabanı hesapları | No | 

## <a name="microsoftdomainregistration"></a>Microsoft. DomainRegistration

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | etki alanları | No | 
> | generatessorequest | No | 
> | topleveldomains | No | 
> | validatedomainregistrationınformation | No |

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
> | eventabonelikleri | No |
> | extensionkonuları | No | 
> | partnernamespaces | No | 
> | iş ortağı kayıtları | No | 
> | iş ortağı konuları | No | 
> | Sistem konuları | No | 
> | konuları | No | 
> | topictypes | No | 

## <a name="microsofteventhub"></a>Microsoft. EventHub

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | leriniz | No |  
> | öznitelikleri | Evet (şablon ile)<br/><br/> [Bir olay hub 'ı ad alanını başka bir bölgeye taşıma](../../event-hubs/move-across-regions.md) | 
> | isteyin | No |  

## <a name="microsoftexperimentation"></a>Microsoft. deneme

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- | 
> | experimentworkspaces | No | 

## <a name="microsoftfalcon"></a>Microsoft. Falcon

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- | 
> | öznitelikleri | No | 

## <a name="microsoftfeatures"></a>Microsoft. Features

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- | 
> | özellik sağlayıcıları | No | 
> | özellikler | No | 
> | sağlayıcılarla | No | 
> | subscriptionfeatureregistrations | No | 

## <a name="microsoftgenomics"></a>Microsoft. Genomiks

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | accounts | No | 

## <a name="microsoftguestconfiguration"></a>Microsoft. GuestConfiguration

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- | 
> | Oto managedaccounts | No | 
> | Oto managedvmconfigurationprofiles | No | 
> | guestconfigurationatamaları | No | 
> | yazılımıdır | No | 
> | softwareupdateprofile | No | 
> | softwareupdates | No | 

## <a name="microsofthanaonazure"></a>Microsoft. HanaOnAzure

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | hanaınstances | No | 
> | sapizleyicileri | No |  

## <a name="microsofthardwaresecuritymodules"></a>Microsoft. HardwareSecurityModules

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- | 
> | ayrılmış atedhsms | No | 


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
> | makineler/uzantılar | No |

## <a name="microsofthybriddata"></a>Microsoft. HybridData

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | veri yöneticileri |  No | 

## <a name="microsofthybridnetwork"></a>Microsoft. HybridNetwork

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- | 
> | cihazlar | No | 
> | vnfs | No | 

## <a name="microsofthydra"></a>Microsoft. Hydra

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- | 
> | bileşenleri | No | 
> | networkscopes | No | 

## <a name="microsoftimportexport"></a>Microsoft. ımportexport

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | Çizelge |  No | 

## <a name="microsoftinsights"></a>Microsoft. Insights

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | accounts | Hayır. [Daha fazla bilgi edinin](../../azure-monitor/faq.md#how-do-i-move-an-application-insights-resource-to-a-new-region).
> | actiongroups |  No | 
> | activitylogalerts | No | 
> | alertrules |  No | 
> | autoscalesettings |  No | 
> | temel | No |
> | bileşenleri |  No |  
> | datacollectionrules | No | 
> | diagnosticsettings | No | 
> | diagnosticsettingscategories | No | 
> | eventcategories | No | 
> | eventTypes | No | 
> | extendeddiagnosticsettings | No | |
> | guestdiagnosticsettings | No | 
> | listmigrationdate | No | 
> | logdefinitions | No | 
> | günlüğe kaydetme profilleri | No | 
> | günlükler | Hayır | Hayır |
> | metricalerts | No | 
> | metrictemeller | No | 
> | metricbatch | No | 
> | MetricDefinitions | No | 
> | metricnamespaces | No | 
> | metrics | No | 
> | migratealertrules | No |
> | migratetonewpricingmodel | No | 
> | çalışma kitapları | No |
> | notificationgroups | No | 
> | privatelinkscopes | No |
> | rollbacktolegacypricingmodel | No |
> | scheduledqueryrules |  No | 
> | topology | No |
> | işlemler | No |
> | vminsightsonboardingstatuses | No |
> | Web testleri |  No | 
> | Web testleri/gettestresultfile | No |
> | çalışma kitapları |  No |  
> | workbooktemplates | No |


## <a name="microsoftiotcentral"></a>Microsoft. ıotcentral

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | apptemplates | No | 
> | ıotapps | No | 



## <a name="microsoftiothub"></a>Microsoft.IoTHub

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> |  ıothub |  Evet (kopya hub 'ı) <br/><br/> [IoT Hub 'ını başka bir bölgeye kopyalama](../../iot-hub/iot-hub-how-to-clone.md)

## <a name="microsoftiotspaces"></a>Microsoft. ıotspaces

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma |
> | ------------- | ----------- | 
> | çıkarılamıyor | No | 

## <a name="microsoftkeyvault"></a>Microsoft. Keykasası

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | silinkaults | No |
> | hsmpools | No | 
> | managedhsms | No |
> | kasaları |  No | 

## <a name="microsoftkubernetes"></a>Microsoft. Kubernetes

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- | 
> | connectedkümeler | No | 
> | registeredsubscriptions | No | 

## <a name="microsoftkubernetesconfiguration"></a>Microsoft. KubernetesConfiguration

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- | 
> | sourcecontrolconfigurations | No | 

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
> | kullanıcılar | No | 

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
> | ıntegrationserviceortamortamları/managedap | No |
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
> | teamaccounts | No | 


## <a name="microsoftmachinelearningmodelmanagement"></a>Microsoft. MachineLearningModelManagement

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | accounts | No | 


## <a name="microsoftmachinelearningservices"></a>Microsoft.MachineLearningServices

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | çalışma alanı | No | 

## <a name="microsoftmaintenance"></a>Microsoft. Maintenance

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | configurationatamalar | Evet. [Daha fazla bilgi edinin](../../virtual-machines/move-region-maintenance-configuration.md) | 
> | maintenanceconfigurations | Evet. [Daha fazla bilgi edinin](../../virtual-machines/move-region-maintenance-configuration-resources.md) |
> | güncelleştirmeler | No | 

## <a name="microsoftmanagedidentity"></a>Microsoft. Managedıdentity

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | lerinizde | No | 
> | userassignedıdentities | No | 

## <a name="microsoftmanagednetwork"></a>Microsoft. ManagedNetwork

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- | 
> | managednetworks | No | 
> | managednetworks/managednetworkgroups | No |
> | managednetworks/managednetworkpeeringpolicies | No | 
> | bildirim | No | 

## <a name="microsoftmanagedservices"></a>Microsoft. ManagedServices

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- | 
> | Pazar placeryumuristrationdefinitions | No | 
> | registrationatamaları | No |
> | registrationdefinitions | No | 

## <a name="microsoftmanagement"></a>Microsoft. Management

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- | 
> | getentities | No | 
> | managementgroups | No | 
> | managementgroups/Settings | No | 
> | kaynaklar | No | 
> | starttenantbackfill | No | 
> | tenantbackfillstatus | No | 

## <a name="microsoftmaps"></a>Microsoft. Maps

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | accounts |  Hayır, Azure Maps bir jeo-uzamsal hizmettir. 
> | hesaplar/privateatlases | No

## <a name="microsoftmarketplace"></a>Microsoft. Market

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | sunar | No | 
> | offertypes | No | 
> | privategallergıtems | No | 
> | privatestoreclient | No | 
> | privatemağazaların | No | 
> | ürün | No | 
> | Publishers | No | 
> | register | No | 

## <a name="microsoftmarketplaceapps"></a>Microsoft. MarketplaceApps

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- | 
> | classicdevservices | No | 

## <a name="microsoftmarketplaceordering"></a>Microsoft. Marketplacesıralaması

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- | 
> | anlaşmalar | No | 
> | offertypes | No | 

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
> | movecollections | No
> | projeyle | No | 

## <a name="microsoftmixedreality"></a>Microsoft. MixedReality

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- | ---------- |
> | holographicsbroadcastaccounts | No | 
> | objectunderstandingaccounts | No | 
> | remoterenderingaccounts | No | 
> | spatialanchorsaccounts | No | 

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
> | bgpservicecommunities | No |
> | bağlantının |  No | 
> | ddoscustompolicies |  No | 
> | ddosprotectionplanlar | No | 
> | dnszones |  No | 
> | expressroutedevreleri | No | 
> | expressroutegateway 'ler | No | 
> | expressrouteserviceproviders | No | 
> | firewallpolicies | No |
> | frontkapıların | No | 
> | ipayırmalar | No |
> | ipgroups | No |
> | loadbalancers | Yes <br/><br/> [Azure Kaynak taşıyıcısı](../../resource-mover/tutorial-move-region-virtual-machines.md) 'ni kullanarak iç ve dış yük dengeleyicileri taşıyın. |
> | localnetworkgateway 'ler |  No | 
> | natgateway 'ler |  No | 
> | networkexperimentprofiles | No |
> | networkıntpolicies ilkeleri |  No | 
> | NetworkInterfaces | Yes <br/><br/> NIC 'Leri taşımak için [Azure Kaynak taşıyıcısı](../../resource-mover/tutorial-move-region-virtual-machines.md) kullanın. | 
> | networkprofiles | No | 
> | networksecuritygroups | Yes <br/><br/> Ağ güvenlik gruplarını (NGSs) taşımak için [Azure Kaynak taşıyıcısı](../../resource-mover/tutorial-move-region-virtual-machines.md) kullanın. | 
> | networkwatchers |  No |  
> | networkwatchers/connectionmonitörleri |  No | 
> | networkwatchers/flowlogs |  No | 
> | networkwatchers/pingkafesler |  No | 
> | p2svpngateways | No | 
> | privatednszones |  No |  
> | privatednszones/virtualnetworklinks | No |> | privatednszonesinternal | No |
> | privateendpointredirectmaps | No |
> | privateendpoints | No | 
> | privatelinkservices | No | 
> | publicıpaddresses | Yes<br/><br/> Genel IP adreslerini taşımak için [Azure Kaynak taşıyıcısı](../../resource-mover/tutorial-move-region-virtual-machines.md) kullanın. |
> | publicıpöneklerini | No | 
> | routefilters | No | 
> | routetables |  No | 
> | securitypartnerproviders | No |
> | serviceendpointpolicies |  No | 
> | trafficmanagergeographichierarchies | No | 
> | trafficmanagerprofiles |  No | 
> | trafficmanagerusermetricskeys | No |
> | virtualhub 'lar | No | 
> | virtualnetworkgateways |  No |  
> | virtualnetworks |  No | 
> | virtualnetworktaps | No | 
> | virtualwan | No | 
> | vpngateway (sanal WAN) | No | 
> | vpnsites (sanal WAN) | No | 
> | vpnsites (sanal WAN) | No |


## <a name="microsoftnotificationhubs"></a>Microsoft. Notificationhub 'Lar

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | öznitelikleri |  No | 
> | ad alanları/notificationhub 'lar |  No |  

## <a name="microsoftobjectstore"></a>Microsoft. ObjectStore

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- | 
> | osnamespaces | No | 

## <a name="microsoftoffazure"></a>Microsoft. OffAzure

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- | 
> | Hiper sanal siteler | No | 
> | ımportsites | No | 
> | sunucusiteleri | No | 
> | vmwaresites | No | 

## <a name="microsoftoperationalinsights"></a>Microsoft. Operationalınsights

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | leriniz | No | 
> | deletedworkspaces | No | 
> | bağlantı hedefleri | No | 
> | storageınsii configs | No |
> | çalışma alanı | No |



## <a name="microsoftoperationsmanagement"></a>Microsoft. OperationsManagement

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | managementassociations | No |
> | managementconfigurations |  No | 
> | çözümler | No |
> | görünümler |  No | 

## <a name="microsoftpeering"></a>Microsoft. eşleme

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | Yasallıklar | No | 
> | peerasns | No | 
> | peeringlocations | No | 
> | eşlemeleri | No | 
> | peeringserviceülkeleriyle | No | 
> | peeringserviceloing | No | 
> | peeringserviceproviders | No | 
> | peeringservices | No | 

## <a name="microsoftpolicyinsights"></a>Microsoft. Poliyelei

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- | 
> | poliyevents | No | 
> | policystates | No | 
> | policytrackedresources | No | 
> | düzeltmeler | No | 

## <a name="microsoftportal"></a>Microsoft. Portal

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> |  konsolları | No |
> | panolar | No | 
> | UserSettings | No | 


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

## <a name="microsoftpurview"></a>Microsoft. purview

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | accounts | No | 

## <a name="microsoftproviderhub"></a>Microsoft. ProviderHub

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- | 
> | availableaccounts | No | 
> | providerkayıtları | No | 
> | piyasaya çıkarma | No | 

## <a name="microsoftquantum"></a>Microsoft. hisse

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- | 
> | çalışma alanı | No | 

## <a name="microsoftrecoveryservices"></a>Microsoft. RecoveryServices

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | replicationeligibilityresults | No |
> | kasaları | Hayır.<br/><br/> Azure bölgeleri arasında Azure Backup için kurtarma hizmetleri kasalarının taşınması desteklenmez.<br/><br/> Azure Site Recovery için kurtarma hizmetleri kasalarında, kasayı hedef bölgede [devre dışı bırakıp yeniden](../../site-recovery/move-vaults-across-regions.md) oluşturabilirsiniz. | 

## <a name="microsoftredhatopenshift"></a>Microsoft. RedHatOpenShift

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- | 
> | openshiftclusters | No | 

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
> | resourcechangedetails | No | 
> | resourcechanges | No | 
> | kaynaklar | No | 
> | resourceshistory | No | 
> | subscriptionsstatus | No | 

## <a name="microsoftresourcehealth"></a>Microsoft. ResourceHealth

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- | 
> | childresources | No | 
> | acil sorun sorunları | No | 
> | etkinlikler | No | 
> | meta veriler | No | 
> | bildirimler | No | 

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
> | uygulamalar |  No | 
> | saasresources | No | 


## <a name="microsoftsearch"></a>Microsoft. Search

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | resourcehealthmetadata | No |
> | searchservices |  No | 


## <a name="microsoftsecurity"></a>Microsoft.Security

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | adaptivenetworkhardenings | No | 
> | advancedthreatprotectionsettings | No | 
> | alerts | No | 
> | allowedconnections | No | 
> | applicationwhitedökümler | No | 
> | assessmentmetadata | No | 
> | değerlendirme | No | 
> | Oto dissalertsrules | No | 
> | akışlarını otomatikleştirin | No | 
> | Oto provisioningsettings | No |
> | karmaşık anceresults | No | 
> | uyumluluklarına | No | 
> | datacollectionagents | No | 
> | devicesecuritygroups | No | 
> | discoveredsecuritysolutions | No | 
> | externalsecuritysolutions | No | 
> | ınformationprotectionpolicies | No | 
> | ıotsecuritysolutions | No | 
> | ıotsecuritysolutions/analiz ticsmodeller | No | 
> | ıotsecuritysolutions/analticsmodeller/aggreggıt uyarıları | No | 
> | ıotsecuritysolutions/analiz ticsmodeller/Aggreg, öneriler | No | 
> | jağaccesspolicies | No | 
> | ilkeler | No | 
> | fiyatlandırmalar | No | 
> | Reve daha karmaşık bakım standartları | No | 
> | rekontrol ve Re, | No | 
> | rekontrol ve Re, Re, Re, | No | 
> | securitycontacts | No | 
> | securitysolutions | No | 
> | securitysolutionsreferencedata | No | 
> | securitydurumlardan | No | 
> | securitystatusessummaries | No | 
> | sunucukullanılabilirliği | No | 
> | ayarlar | No | 
> | subassessments | No |
> | Görevler | No | 
> | anlatır | No | 
> | çalışma alanı ayarları | No | 

## <a name="microsoftsecurityinsights"></a>Microsoft. Securityınsights

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- | 
> | toplamaları | No | 
> | alertrules | No | 
> | alertrutatemplates | No | 
> | automationrules | No |
> | çalışmaların | No | 
> | veri bağlayıcıları | No | 
> | varlıklar | No | 
> | entityqueries | No |
> | olaylar | No | 
> | officeconsents | No | 
> | ayarlar | No | 
> | threatıntelligence | No | 

## <a name="microsoftserialconsole"></a>Microsoft. SerialConsole

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- | 
> | consoleservices | No | 

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
> | premiummessagingregions | No | 
> | isteyin | No | 

## <a name="microsoftservicefabric"></a>Microsoft. ServiceFabric

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | uygulamalar | No | 
> | leriniz |  No |  
> | kapsayıcı grupları | No | 
> | containergroupsets | No | 
> | edgeclusters | No | 
> | managedkümeler | No |
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

## <a name="microsoftservices"></a>Microsoft. Services

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- | 
> | piyasaya çıkarma | No | 

## <a name="microsoftsignalrservice"></a>Microsoft. SignalRService

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | SignalR |  No |  

## <a name="microsoftsoftwareplan"></a>Microsoft. SoftwarePlan

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- | 
> | hybriduseavantajlar | No | 

## <a name="microsoftsolutions"></a>Microsoft. Solutions

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | appliancedefinitions | No | 
> | yaptı | No | 
> | jistekleri | No | 

## <a name="microsoftsql"></a>Microsoft.Sql

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | ınstancepools | No | 
> | yerlerini | No |
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
> | sqlvirtualmachinegroups |  No |  
> | sqlvirtualmachines |  No |  


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
> | leriniz | No |
> | streammingjobs |  No |  


## <a name="microsoftstreamanalyticsexplorer"></a>Microsoft. StreamAnalyticsExplorer

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | lý | No | 
> | larında | No | 

## <a name="microsoftsubscription"></a>Microsoft. Subscription

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- | 
> | Aboneliklerin | No | 

## <a name="microsoftsupport"></a>Microsoft. support

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- | 
> | services | No | 
> | destek biletleri | No | 

## <a name="microsoftsynapse"></a>Microsoft. SYNAPSE

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- | 
> | çalışma alanı | No | 
> | çalışma alanları/bigdatapools | No | 
> | çalışma alanları/sqlpools | No | 


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

## <a name="microsoftvmware"></a>Microsoft. VMware

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- | 
> | arczones | No | 
> | ResourcePools | No | 
> | vCenter | No | 
> | virtualmachines | No | 
> | virtualmachinetemplates | No | 
> | virtualnetworks | No | 

## <a name="microsoftvmwarecloudsimple"></a>Microsoft. Vmwarechoparlör basit

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | ayrılmış cloudnodes | No | 
> | ayrılmış CloudService | No | 
> | virtualmachines | No | 

## <a name="microsoftvnfmanager"></a>Microsoft. VnfManager

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- | 
> | cihazlar | No | 
> | vnfs | No | 

## <a name="microsoftvsonline"></a>Microsoft. VSOnline

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- | 
> | accounts | No | 
> | Planlama | No | 
> | registeredsubscriptions | No |


## <a name="microsoftweb"></a>Microsoft. Web

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | availableyığınları | No | 
> | billingmeters | No | 
> | sertifikalar | No | 
> | connectiongateway 'ler |  No |  
> | bağlantının |  No |  
> | customapsıs |  No | 
> | silinmi siteleri | No | 
> | deploymentlocations | No | 
> | coğrafi bölgeler | No | 
> | hostingenvironments | No | 
> | kubeortamları | No | 
> | publishingusers | No |
> | Öneriler | No | 
> | resourcehealthmetadata | No | 
> | zamanları | No | 
> | serverfarms | No |  
> | sunucugrupları/eventgridfilters | N
> | Siteler |  No | 
> | siteler/premieraddons |  No |  
> | siteler/yuvalar |  No |  
> | sourcecontrols | No |
> | staticsites | No | 

## <a name="microsoftwindowsesu"></a>Microsoft. WindowsESU

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- | 
> | çoğulactivationkeys | No |

## <a name="microsoftwindowsiot"></a>Microsoft. Windowsıot

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- |
> | deviceservices | No | 

## <a name="microsoftworkloadbuilder"></a>Microsoft. WorkloadBuilder

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- | 
> | lerine | No | 

## <a name="microsoftworkloadmonitor"></a>Microsoft. WorkloadMonitor

> [!div class="mx-tableFixed"]
> | Kaynak türü | Bölge taşıma | 
> | ------------- | ----------- | 
> | bileşenleri | No |
> | componentssummary | No | 
> | izleme örnekleri | No | 
> | izleme ınstancessummary | No | 
> | monitörün | No | 
## <a name="third-party-services"></a>Üçüncü taraf hizmetleri

Üçüncü taraf hizmetler şu anda taşıma işlemini desteklemiyor.

## <a name="next-steps"></a>Sonraki adımlar

Kaynak taşıyıcısı hizmeti hakkında [daha fazla bilgi edinin](../../resource-mover/overview.md) .

