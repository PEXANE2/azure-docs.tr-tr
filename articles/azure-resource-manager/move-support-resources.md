---
title: Azure Kaynak türüne göre işlem desteğini taşıma
description: Yeni bir kaynak grubuna veya aboneliğe taşınabilecek Azure Kaynak türlerini listeler.
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: reference
ms.date: 7/9/2019
ms.author: tomfitz
ms.openlocfilehash: 22493ad7998e9014c88c79e6ac5eee3bf1216119
ms.sourcegitcommit: 920ad23613a9504212aac2bfbd24a7c3de15d549
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/15/2019
ms.locfileid: "68226808"
---
# <a name="move-operation-support-for-resources"></a>Kaynaklar için taşıma işlemi desteği
Bu makalede, bir Azure Kaynak türünün taşıma işlemini destekleyip desteklemediğini listelenmiştir. Ayrıca bir kaynağı taşırken göz önünde bulundurmanız gereken özel koşullar hakkında bilgi sağlar.

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
> - [Microsoft. AzureStack](#microsoftazurestack)
> - [Microsoft. Backup](#microsoftbackup)
> - [Microsoft. Batch](#microsoftbatch)
> - [Microsoft. Batchaı](#microsoftbatchai)
> - [Microsoft. BingMaps](#microsoftbingmaps)
> - [Microsoft. BizTalkServices](#microsoftbiztalkservices)
> - [Microsoft. Blockzinciri](#microsoftblockchain)
> - [Microsoft. Blueprint](#microsoftblueprint)
> - [Microsoft. BotService](#microsoftbotservice)
> - [Microsoft. Cache](#microsoftcache)
> - [Microsoft. CDN](#microsoftcdn)
> - [Microsoft. CertificateRegistration](#microsoftcertificateregistration)
> - [Microsoft. ClassicCompute](#microsoftclassiccompute)
> - [Microsoft. ClassicNetwork](#microsoftclassicnetwork)
> - [Microsoft. ClassicStorage](#microsoftclassicstorage)
> - [Microsoft. Biliveservices](#microsoftcognitiveservices)
> - [Microsoft. COMPUTE](#microsoftcompute)
> - [Microsoft. Container](#microsoftcontainer)
> - [Microsoft. Containerınstance](#microsoftcontainerinstance)
> - [Microsoft. ContainerRegistry](#microsoftcontainerregistry)
> - [Microsoft. ContainerService](#microsoftcontainerservice)
> - [Microsoft. Contentmoderatör](#microsoftcontentmoderator)
> - [Microsoft. Cortanaanalizi](#microsoftcortanaanalytics)
> - [Microsoft. CostManagement](#microsoftcostmanagement)
> - [Microsoft. Customerınsights](#microsoftcustomerinsights)
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
> - [Microsoft. Dbformarıdb](#microsoftdbformariadb)
> - [Microsoft. Dbformyısql](#microsoftdbformysql)
> - [Microsoft. DBforPostgreSQL](#microsoftdbforpostgresql)
> - [Microsoft. DeploymentManager](#microsoftdeploymentmanager)
> - [Microsoft. Devices](#microsoftdevices)
> - [Microsoft. DevSpaces](#microsoftdevspaces)
> - [Microsoft. DevTestLab](#microsoftdevtestlab)
> - [Microsoft. DNS](#microsoftdns)
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
> - [Microsoft. kusto](#microsoftkusto)
> - [Microsoft. LabServices](#microsoftlabservices)
> - [Microsoft. LocationBasedServices](#microsoftlocationbasedservices)
> - [Microsoft. LocationServices](#microsoftlocationservices)
> - [Microsoft. Logic](#microsoftlogic)
> - [Microsoft. Machinöğrenim](#microsoftmachinelearning)
> - [Microsoft. MachineLearningCompute](#microsoftmachinelearningcompute)
> - [Microsoft. MachineLearningExperimentation](#microsoftmachinelearningexperimentation)
> - [Microsoft. MachineLearningModelManagement](#microsoftmachinelearningmodelmanagement)
> - [Microsoft. MachineLearningOperationalization](#microsoftmachinelearningoperationalization)
> - [Microsoft. MachineLearningServices](#microsoftmachinelearningservices)
> - [Microsoft. Managedıdentity](#microsoftmanagedidentity)
> - [Microsoft. Maps](#microsoftmaps)
> - [Microsoft. MarketplaceApps](#microsoftmarketplaceapps)
> - [Microsoft. Media](#microsoftmedia)
> - [Microsoft. Migrate](#microsoftmigrate)
> - [Microsoft. NetApp](#microsoftnetapp)
> - [Microsoft. Network](#microsoftnetwork)
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
> - [Microsoft. SaaS](#microsoftsaas)
> - [Microsoft. Scheduler](#microsoftscheduler)
> - [Microsoft. Search](#microsoftsearch)
> - [Microsoft. Security](#microsoftsecurity)
> - [Microsoft. ServerManagement](#microsoftservermanagement)
> - [Microsoft.ServiceBus](#microsoftservicebus)
> - [Microsoft. ServiceFabric](#microsoftservicefabric)
> - [Microsoft. Servicefabrickafesi](#microsoftservicefabricmesh)
> - [Microsoft. SignalRService](#microsoftsignalrservice)
> - [Microsoft. Sıterecovery](#microsoftsiterecovery)
> - [Microsoft. Solutions](#microsoftsolutions)
> - [Microsoft. SQL](#microsoftsql)
> - [Microsoft. SqlVirtualMachine](#microsoftsqlvirtualmachine)
> - [Microsoft. SqlVM](#microsoftsqlvm)
> - [Microsoft. Storage](#microsoftstorage)
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
> - [microsoft.visualstudio](#microsoftvisualstudio)
> - [Microsoft. Vmwarechoparlör basit](#microsoftvmwarecloudsimple)
> - [Microsoft. Web](#microsoftweb)
> - [Microsoft. Windowsıot](#microsoftwindowsiot)
> - [Microsoft. WindowsVirtualDesktop](#microsoftwindowsvirtualdesktop)

## <a name="microsoftaad"></a>Microsoft.AAD
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| DomainServices | Hayır | Hayır |

## <a name="microsoftaadiam"></a>microsoft.aadiam
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| Kira | Hayır | Hayır |

## <a name="microsoftalertsmanagement"></a>Microsoft.AlertsManagement
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| actionrules | Evet | Evet |

## <a name="microsoftanalysisservices"></a>Microsoft.AnalysisServices
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| Larý | Evet | Evet |

## <a name="microsoftapimanagement"></a>Microsoft.ApiManagement
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| hizmet | Evet | Evet |

## <a name="microsoftappconfiguration"></a>Microsoft. AppConfiguration
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| configurationmağazaların | Evet | Evet |

## <a name="microsoftappservice"></a>Microsoft. AppService
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| apiapps | Hayır | Hayır |
| appdentities | Hayır | Hayır |
| geçidinin | Hayır | Hayır |

> [!IMPORTANT]
> Bkz. [App Service taşıma Kılavuzu](./move-limitations/app-service-move-limitations.md).

## <a name="microsoftauthorization"></a>Microsoft. Authorization
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| poliyasatamaları | Hayır | Hayır |

## <a name="microsoftautomation"></a>Microsoft. Automation
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| automationaccounts | Evet | Evet |
| automationaccounts/Configurations | Evet | Evet |
| automationaccounts/runbook 'lar | Evet | Evet |

> [!IMPORTANT]
> Runbook 'lar Otomasyon hesabıyla aynı kaynak grubunda bulunmalıdır.

## <a name="microsoftazureactivedirectory"></a>Microsoft.AzureActiveDirectory
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| b2cdirectories | Evet | Evet |

## <a name="microsoftazurestack"></a>Microsoft.AzureStack
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| kayıtlarında | Evet | Evet |

## <a name="microsoftbackup"></a>Microsoft.Backup
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| backupvault | Hayır | Hayır |

## <a name="microsoftbatch"></a>Microsoft.Batch
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| batchaccounts | Evet | Evet |

## <a name="microsoftbatchai"></a>Microsoft.BatchAI
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| leriniz | Hayır | Hayır |
| dosya sunucuları | Hayır | Hayır |
| Çizelge | Hayır | Hayır |
| Onlarla | Hayır | Hayır |

## <a name="microsoftbingmaps"></a>Microsoft.BingMaps
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| mapapsıs | Hayır | Hayır |

## <a name="microsoftbiztalkservices"></a>Microsoft. BizTalkServices
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| biztalk | Evet | Evet |

## <a name="microsoftblockchain"></a>Microsoft. Blockzinciri
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| blockchainmembers | Evet | Evet |

## <a name="microsoftblueprint"></a>Microsoft. Blueprint
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| şema tasmi | Hayır | Hayır |

## <a name="microsoftbotservice"></a>Microsoft. BotService
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| botservices | Evet | Evet |

## <a name="microsoftcache"></a>Microsoft.Cache
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| Redis | Evet | Evet |

> [!IMPORTANT]
> Redsıs örneği için Azure önbelleği bir sanal ağla yapılandırıldıysa, örnek farklı bir aboneliğe taşınamaz. Bkz. [sanal ağların taşıma sınırlamaları](./move-limitations/virtual-network-move-limitations.md).

## <a name="microsoftcdn"></a>Microsoft.Cdn
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| profiles | Evet | Evet |
| Profiller/uç noktalar | Evet | Evet |

## <a name="microsoftcertificateregistration"></a>Microsoft.CertificateRegistration
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| sertifikadüzenleri | Evet | Evet |

> [!IMPORTANT]
> Bkz. [App Service taşıma Kılavuzu](./move-limitations/app-service-move-limitations.md).

## <a name="microsoftclassiccompute"></a>Microsoft.ClassicCompute
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| DomainNames | Evet | Hayır |
| virtualmachines | Evet | Hayır |

> [!IMPORTANT]
> Bkz. [klasik dağıtım taşıma Kılavuzu](./move-limitations/classic-model-move-limitations.md). Klasik dağıtım kaynakları, bu senaryoya özel bir işlemle abonelikler arasında taşınabilir.

## <a name="microsoftclassicnetwork"></a>Microsoft.ClassicNetwork
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| networksecuritygroups | Hayır | Hayır |
| rezervler | Hayır | Hayır |
| virtualnetworks | Hayır | Hayır |

> [!IMPORTANT]
> Bkz. [klasik dağıtım taşıma Kılavuzu](./move-limitations/classic-model-move-limitations.md). Klasik dağıtım kaynakları, bu senaryoya özel bir işlemle abonelikler arasında taşınabilir.

## <a name="microsoftclassicstorage"></a>Microsoft.ClassicStorage
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| storageaccounts | Evet | Hayır |

> [!IMPORTANT]
> Bkz. [klasik dağıtım taşıma Kılavuzu](./move-limitations/classic-model-move-limitations.md). Klasik dağıtım kaynakları, bu senaryoya özel bir işlemle abonelikler arasında taşınabilir.

## <a name="microsoftcognitiveservices"></a>Microsoft.CognitiveServices
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| accounts | Evet | Evet |

## <a name="microsoftcompute"></a>Microsoft.Compute
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| availabilitysets | Evet | Evet |
| Disklerinden | Evet | Evet |
| Galeriler | Hayır | Hayır |
| Galeriler/görüntüler | Hayır | Hayır |
| Galeriler/resimler/sürümler | Hayır | Hayır |
| hostgroups | Hayır | Hayır |
| hostgroups/konaklar | Hayır | Hayır |
| images | Evet | Evet |
| proximityplacementgroups | Hayır | Hayır |
| restorepointcollections | Hayır | Hayır |
| sharedvmımages | Hayır | Hayır |
| sharedvmımages/sürümler | Hayır | Hayır |
| görüntüsünü | Evet | Evet |
| virtualmachines | Evet | Evet |
| virtualmachines/uzantıları | Evet | Evet |
| virtualmachinescalesets | Evet | Evet |

> [!IMPORTANT]
> Bkz. [sanal makine taşıma Kılavuzu](./move-limitations/virtual-machines-move-limitations.md).

## <a name="microsoftcontainer"></a>Microsoft. Container
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| kapsayıcı grupları | Hayır | Hayır |

## <a name="microsoftcontainerinstance"></a>Microsoft.ContainerInstance
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| kapsayıcı grupları | Hayır | Hayır |

## <a name="microsoftcontainerregistry"></a>Microsoft.ContainerRegistry
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| kayıt | Evet | Evet |
| kayıt defterleri/BuildTasks | Evet | Evet |
| kayıt defterleri/çoğaltmalar | Evet | Evet |
| kayıt defterleri/görevler | Evet | Evet |
| kayıt defterleri/Web kancaları | Evet | Evet |

## <a name="microsoftcontainerservice"></a>Microsoft.ContainerService
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| containerservices | Hayır | Hayır |
| managedkümeler | Hayır | Hayır |
| openshiftmanagedclusters | Hayır | Hayır |

## <a name="microsoftcontentmoderator"></a>Microsoft.ContentModerator
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| uygulamalar | Evet | Evet |

## <a name="microsoftcortanaanalytics"></a>Microsoft. Cortanaanalizi
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| accounts | Hayır | Hayır |

## <a name="microsoftcostmanagement"></a>Microsoft. CostManagement
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| bağlayıcılar | Evet | Evet |

## <a name="microsoftcustomerinsights"></a>Microsoft.CustomerInsights
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| Lara | Evet | Evet |

## <a name="microsoftdatabox"></a>Microsoft. DataBox
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| Çizelge | Hayır | Hayır |

## <a name="microsoftdataboxedge"></a>Microsoft. DataBoxEdge
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| databoxedgedevices | Hayır | Hayır |

## <a name="microsoftdatabricks"></a>Microsoft. Databricks
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| Onlarla | Hayır | Hayır |

## <a name="microsoftdatacatalog"></a>Microsoft.DataCatalog
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| larına | Evet | Evet |
| veri katalogları | Hayır | Hayır |

## <a name="microsoftdataconnect"></a>Microsoft. DataConnect
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| connectionyöneticileri | Hayır | Hayır |

## <a name="microsoftdataexchange"></a>Microsoft. DataExchange
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| Paketlerle | Hayır | Hayır |
| Planlama | Hayır | Hayır |

## <a name="microsoftdatafactory"></a>Microsoft.DataFactory
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| veri fabrikaları | Evet | Evet |
| larının | Evet | Evet |

## <a name="microsoftdatalake"></a>Microsoft. DataLake
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| datalakeaccounts | Hayır | Hayır |

## <a name="microsoftdatalakeanalytics"></a>Microsoft.DataLakeAnalytics
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| accounts | Evet | Evet |

## <a name="microsoftdatalakestore"></a>Microsoft.DataLakeStore
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| accounts | Evet | Evet |

## <a name="microsoftdatamigration"></a>Microsoft. DataMigration
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| services | Hayır | Hayır |
| Hizmetler/Projeler | Hayır | Hayır |
| Lara | Hayır | Hayır |

## <a name="microsoftdbformariadb"></a>Microsoft. Dbformarıdb
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| Larý | Evet | Evet |

## <a name="microsoftdbformysql"></a>Microsoft.DBforMySQL
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| Larý | Evet | Evet |

## <a name="microsoftdbforpostgresql"></a>Microsoft.DBforPostgreSQL
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| sunucu grupları | Hayır | Hayır |
| Larý | Evet | Evet |
| serversv2 | Evet | Evet |

## <a name="microsoftdeploymentmanager"></a>Microsoft. DeploymentManager
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| artifactsources | Evet | Evet |
| piyasaya çıkarma | Evet | Evet |
| servicetopolojileri | Evet | Evet |
| servicetopolojileri/hizmetler | Evet | Evet |
| servicetopolojileri/hizmetler/serviceunits | Evet | Evet |
| adımlar | Evet | Evet |

## <a name="microsoftdevices"></a>Microsoft. Devices
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| elaun havuzları | Hayır | Hayır |
| elaun havuzları/ıothubkiracılar | Hayır | Hayır |
| ıothubs | Evet | Evet |
| provisioningservices | Evet | Evet |

## <a name="microsoftdevspaces"></a>Microsoft. DevSpaces
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| Örleri | Hayır | Hayır |

## <a name="microsoftdevtestlab"></a>Microsoft.DevTestLab
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| labcenters | Hayır | Hayır |
| larda | Evet | Hayır |
| Laboratuvarlar/ortamlar | Evet | Evet |
| Labs/servicerunanlar | Evet | Evet |
| Labs/virtualmachines | Evet | Hayır |
| cağını | Evet | Evet |

## <a name="microsoftdns"></a>Microsoft. DNS
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| dnszones | Hayır | Hayır |
| dnszones/a | Hayır | Hayır |
| dnszones/aaaa | Hayır | Hayır |
| dnszones/CNAME | Hayır | Hayır |
| dnszones/MX | Hayır | Hayır |
| dnszones/PTR | Hayır | Hayır |
| dnszones/SRV | Hayır | Hayır |
| dnszones/txt | Hayır | Hayır |
| trafficmanagerprofiles | Hayır | Hayır |

## <a name="microsoftdocumentdb"></a>Microsoft.DocumentDB
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| veritabanı hesapları | Evet | Evet |

## <a name="microsoftdomainregistration"></a>Microsoft.DomainRegistration
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| etki alanları | Evet | Evet |

## <a name="microsoftenterpriseknowledgegraph"></a>Microsoft. EnterpriseKnowledgeGraph
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| services | Evet | Evet |

## <a name="microsofteventgrid"></a>Microsoft.EventGrid
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| etki alanları | Evet | Evet |
| konularıyla | Evet | Evet |

## <a name="microsofteventhub"></a>Microsoft.EventHub
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| leriniz | Evet | Evet |
| Öznitelikleri | Evet | Evet |

## <a name="microsoftgenomics"></a>Microsoft. Genomiks
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| accounts | Hayır | Hayır |

## <a name="microsofthanaonazure"></a>Microsoft. HanaOnAzure
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| hanaınstances | Evet | Evet |

## <a name="microsofthdinsight"></a>Microsoft.HDInsight
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| leriniz | Evet | Evet |

> [!IMPORTANT]
> HDInsight kümeleri, yeni bir abonelik veya kaynak grubuna taşıyabilirsiniz. Ancak, ağ kaynaklarını (örneğin, sanal ağ, NIC veya yük dengeleyici) HDInsight kümesine bağlı abonelikler arasında taşınamaz. Ayrıca, küme için bir sanal makineye ekli NIC yeni bir kaynak grubuna taşınamıyor.
>
> Bir HDInsight kümesi için yeni bir abonelik taşırken, önce diğer kaynakları (örneğin, depolama hesabı) taşıyın. Ardından, HDInsight kümesi, tek başına taşıyın.

## <a name="microsofthealthcareapis"></a>Microsoft. Healthgelişme API 'leri
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| services | Evet | Evet |

## <a name="microsofthybridcompute"></a>Microsoft. HybridCompute
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| larla | Hayır | Hayır |

## <a name="microsofthybriddata"></a>Microsoft. HybridData
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| veri yöneticileri | Evet | Evet |

## <a name="microsoftimportexport"></a>Microsoft.ImportExport
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| Çizelge | Evet | Evet |

## <a name="microsoftinsights"></a>Microsoft. Insights
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| accounts | Hayır | Hayır |
| actiongroups | Evet | Evet |
| activitylogalerts | Hayır | Hayır |
| alertrules | Evet | Evet |
| autoscalesettings | Evet | Evet |
| Bileşenleri | Evet | Evet |
| guestdiagnosticsettings | Hayır | Hayır |
| metricalerts | Hayır | Hayır |
| notificationgroups | Hayır | Hayır |
| notificationrules | Hayır | Hayır |
| scheduledqueryrules | Evet | Evet |
| Web testleri | Evet | Evet |
| Kitabı | Evet | Evet |

> [!IMPORTANT]
> Yeni aboneliğe taşınmasının [abonelik kotalarını](../azure-subscription-service-limits.md#azure-monitor-limits)aşmadığından emin olun.

## <a name="microsoftiotcentral"></a>Microsoft. ıotcentral
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| ıotapps | Evet | Evet |

## <a name="microsoftiotspaces"></a>Microsoft. ıotspaces
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| checknameavaılabılıty | Evet | Evet |
| graph | Evet | Evet |

## <a name="microsoftkeyvault"></a>Microsoft.KeyVault
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| hsmpools | Hayır | Hayır |
| kasaları | Evet | Evet |

> [!IMPORTANT]
> Disk şifrelemesi için kullanılan anahtar kasaları aynı abonelikte veya abonelikler arasında bir kaynak grubuna taşınamaz.

## <a name="microsoftkusto"></a>Microsoft. kusto
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| leriniz | Evet | Evet |

## <a name="microsoftlabservices"></a>Microsoft. LabServices
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| labaccounts | Hayır | Hayır |

## <a name="microsoftlocationbasedservices"></a>Microsoft.LocationBasedServices
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| accounts | Evet | Evet |

## <a name="microsoftlocationservices"></a>Microsoft. LocationServices
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| accounts | Hayır | Hayır |

## <a name="microsoftlogic"></a>Microsoft.Logic
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| hostingenvironments | Hayır | Hayır |
| Tümleştirme hesapları | Evet | Evet |
| ıntegrationserviceortamortamları | Hayır | Hayır |
| ısotedenvironments | Hayır | Hayır |
| sürdürülen | Evet | Evet |

## <a name="microsoftmachinelearning"></a>Microsoft.MachineLearning
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| commitmentplanlar | Evet | Evet |
| Hizmetleri | Evet | Hayır |
| Onlarla | Evet | Evet |

## <a name="microsoftmachinelearningcompute"></a>Microsoft.MachineLearningCompute
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| operationalizationkümeleri | Evet | Evet |

## <a name="microsoftmachinelearningexperimentation"></a>Microsoft. MachineLearningExperimentation
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| accounts | Hayır | Hayır |
| hesaplar/çalışma alanları | Hayır | Hayır |
| hesaplar/çalışma alanları/projeler | Hayır | Hayır |
| teamaccounts | Hayır | Hayır |
| teamaccounts/çalışma alanları | Hayır | Hayır |
| teamaccounts/çalışma alanları/projeler | Hayır | Hayır |

## <a name="microsoftmachinelearningmodelmanagement"></a>Microsoft.MachineLearningModelManagement
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| accounts | Evet | Evet |

## <a name="microsoftmachinelearningoperationalization"></a>Microsoft. MachineLearningOperationalization
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| hostinghesapları | Hayır | Hayır |

## <a name="microsoftmachinelearningservices"></a>Microsoft. MachineLearningServices
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| Onlarla | Hayır | Hayır |

## <a name="microsoftmanagedidentity"></a>Microsoft. Managedıdentity
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| userassignedıdentities | Hayır | Hayır |

## <a name="microsoftmaps"></a>Microsoft. Maps
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| accounts | Evet | Evet |

## <a name="microsoftmarketplaceapps"></a>Microsoft. MarketplaceApps
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| classicdevservices | Hayır | Hayır |

## <a name="microsoftmedia"></a>Microsoft. Media
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| mediaservices | Evet | Evet |
| mediaservices/liveevents | Evet | Evet |
| mediaservices/streamingendpoints | Evet | Evet |

## <a name="microsoftmigrate"></a>Microsoft. Migrate
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| assessmentprojects | Hayır | Hayır |
| migrateprojects | Hayır | Hayır |
| projeyle | Hayır | Hayır |

## <a name="microsoftnetapp"></a>Microsoft. NetApp
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| netappaccounts | Hayır | Hayır |
| netappaccounts/capacityhavuzları | Hayır | Hayır |
| netappaccounts/capacityhavuzları/birimleri | Hayır | Hayır |
| netappaccounts/capacityhavuzlar/Volumes/Mount hedefleri | Hayır | Hayır |
| netappaccounts/capacityhavuzlar/birimler/anlık görüntüler | Hayır | Hayır |

## <a name="microsoftnetwork"></a>Microsoft.Network
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| applicationgateway 'ler | Hayır | Hayır |
| applicationgatewaywebapplicationfirewallpolicies | Hayır | Hayır |
| applicationsecuritygroups | Evet | Evet |
| azurefirewalls | Evet | Evet |
| Savunma Konakları | Hayır | Hayır |
| bağlantının | Evet | Evet |
| ddoscustompolicies | Evet | Evet |
| ddosprotectionplanlar | Hayır | Hayır |
| dnszones | Evet | Evet |
| expressroutedevreleri | Hayır | Hayır |
| expressroutecrossconnections | Hayır | Hayır |
| expressroutegateway 'ler | Hayır | Hayır |
| expressrouteports | Hayır | Hayır |
| frontkapıların | Hayır | Hayır |
| frontdoorwebapplicationfirewallpolicies | Hayır | Hayır |
| loadbalancers | Evet-temel SKU<br>Standart SKU yok | Evet-temel SKU<br>Standart SKU yok |
| localnetworkgateway 'ler | Evet | Evet |
| natgateway 'ler | Evet | Evet |
| networkıntpolicies ilkeleri | Evet | Evet |
| NetworkInterfaces | Evet | Evet |
| networkprofiles | Hayır | Hayır |
| networksecuritygroups | Evet | Evet |
| networkwatchers | Evet | Evet |
| networkwatchers/connectionmonitörleri | Evet | Evet |
| networkwatchers/uzunluler | Evet | Evet |
| networkwatchers/pingkafesler | Evet | Evet |
| p2svpngateways | Hayır | Hayır |
| privatednszones | Evet | Evet |
| privatednszones/virtualnetworklinks | Evet | Evet |
| privateendpoints | Hayır | Hayır |
| privatelinkservices | Hayır | Hayır |
| publicıpaddresses | Evet-temel SKU<br>Standart SKU yok | Evet-temel SKU<br>Standart SKU yok |
| publicıpöneklerini | Evet | Evet |
| routefilters | Hayır | Hayır |
| routetables | Evet | Evet |
| securegateyöntemlere | Evet | Evet |
| serviceendpointpolicies | Evet | Evet |
| trafficmanagerprofiles | Evet | Evet |
| virtualhub 'lar | Hayır | Hayır |
| virtualnetworkgateways | Evet | Evet |
| virtualnetworks | Evet | Evet |
| virtualnetworktaps | Hayır | Hayır |
| virtualwan | Hayır | Hayır |
| vpngateway 'ler | Hayır | Hayır |
| vpnsites | Hayır | Hayır |
| webapplicationfirewallpolicies | Evet | Evet |

> [!IMPORTANT]
> Bkz. [sanal ağlar taşıma Kılavuzu](./move-limitations/virtual-network-move-limitations.md).

## <a name="microsoftnotificationhubs"></a>Microsoft.NotificationHubs
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| Öznitelikleri | Evet | Evet |
| ad alanları/notificationhub 'lar | Evet | Evet |

## <a name="microsoftoperationalinsights"></a>Microsoft.OperationalInsights
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| Onlarla | Evet | Evet |

> [!IMPORTANT]
> Yeni aboneliğe taşınmasının [abonelik kotalarını](../azure-subscription-service-limits.md#azure-monitor-limits)aşmadığından emin olun.

## <a name="microsoftoperationsmanagement"></a>Microsoft.OperationsManagement
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| managementconfigurations | Evet | Evet |
| çözümler | Evet | Evet |
| Görünümler | Evet | Evet |

## <a name="microsoftpeering"></a>Microsoft. eşleme
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| eşlemeleri | Hayır | Hayır |

## <a name="microsoftportal"></a>Microsoft. Portal
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| Panoların | Evet | Evet |

## <a name="microsoftportalsdk"></a>Microsoft. PortalSdk
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| rootresources | Hayır | Hayır |

## <a name="microsoftpowerbi"></a>Microsoft. PowerBI
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| workspacecollections | Evet | Evet |

## <a name="microsoftpowerbidedicated"></a>Microsoft.PowerBIDedicated
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| kapasiteler | Evet | Evet |

## <a name="microsoftprojectoxford"></a>Microsoft. ProjectOxford
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| accounts | Hayır | Hayır |

## <a name="microsoftrecoveryservices"></a>Microsoft. RecoveryServices
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| kasaları | Evet | Evet |

> [!IMPORTANT]
> Bkz. [Kurtarma Hizmetleri taşıma Kılavuzu](../backup/backup-azure-move-recovery-services-vault.md?toc=/azure/azure-resource-manager/toc.json).

## <a name="microsoftrelay"></a>Microsoft. Relay
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| Öznitelikleri | Evet | Evet |

## <a name="microsoftsaas"></a>Microsoft. SaaS
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| uygulamalar | Evet | Hayır |

## <a name="microsoftscheduler"></a>Microsoft. Scheduler
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| var | Evet | Evet |
| işlere | Evet | Evet |

## <a name="microsoftsearch"></a>Microsoft.Search
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| searchservices | Evet | Evet |

> [!IMPORTANT]
> Farklı bölgelerde çeşitli arama kaynaklarını tek bir işlemde taşıyamazsınız. Bunun yerine, bunları ayrı işlemlerde taşıyın.

## <a name="microsoftsecurity"></a>Microsoft.Security
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| ıotsecuritysolutions | Evet | Evet |

## <a name="microsoftservermanagement"></a>Microsoft. ServerManagement
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| geçidinin | Hayır | Hayır |
| düğüm | Hayır | Hayır |

## <a name="microsoftservicebus"></a>Microsoft.ServiceBus
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| Öznitelikleri | Evet | Evet |

## <a name="microsoftservicefabric"></a>Microsoft.ServiceFabric
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| uygulamalar | Hayır | Hayır |
| leriniz | Evet | Evet |
| kapsayıcı grupları | Hayır | Hayır |
| containergroupsets | Hayır | Hayır |
| edgeclusters | Hayır | Hayır |
| Mamak | Hayır | Hayır |
| secretmağazaları | Hayır | Hayır |
| volumes | Hayır | Hayır |

## <a name="microsoftservicefabricmesh"></a>Microsoft. Servicefabrickafesi
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| uygulamalar | Evet | Evet |
| kapsayıcı grupları | Hayır | Hayır |
| geçidinin | Evet | Evet |
| Mamak | Evet | Evet |
| kaynaklanır | Evet | Evet |
| volumes | Evet | Evet |

## <a name="microsoftsignalrservice"></a>Microsoft. SignalRService
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| SignalR | Evet | Evet |

## <a name="microsoftsiterecovery"></a>Microsoft.SiteRecovery
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| siterecoveryvault | Hayır | Hayır |

> [!IMPORTANT]
> Bkz. [Kurtarma Hizmetleri taşıma Kılavuzu](../backup/backup-azure-move-recovery-services-vault.md?toc=/azure/azure-resource-manager/toc.json).

## <a name="microsoftsolutions"></a>Microsoft. Solutions
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| appliancedefinitions | Hayır | Hayır |
| yaptı | Hayır | Hayır |
| applicationdefinitions | Hayır | Hayır |
| uygulamalar | Hayır | Hayır |
| jistekleri | Hayır | Hayır |

## <a name="microsoftsql"></a>Microsoft.Sql
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| ınstancepools | Hayır | Hayır |
| ManagedInstances | Hayır | Hayır |
| ManagedInstances/veritabanları | Hayır | Hayır |
| Larý | Evet | Evet |
| sunucular/veritabanları | Evet | Evet |
| sunucular/elaun havuzları | Evet | Evet |
| virtualkümeler | Evet | Evet |

> [!IMPORTANT]
> Bir veritabanı ve sunucu aynı kaynak grubunda olmalıdır. Bir SQL server taşıdığınızda, tüm veritabanlarını da taşınır. Bu davranış, Azure SQL veritabanı ve Azure SQL veri ambarı veritabanları için geçerlidir.

## <a name="microsoftsqlvirtualmachine"></a>Microsoft. SqlVirtualMachine
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| sqlvirtualmachinegroups | Evet | Evet |
| sqlvirtualmachines | Evet | Evet |

## <a name="microsoftsqlvm"></a>Microsoft. SqlVM
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| dwvm | Hayır | Hayır |

## <a name="microsoftstorage"></a>Microsoft.Storage
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| storageaccounts | Evet | Evet |

## <a name="microsoftstoragecache"></a>Microsoft. StorageCache
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| önbelleklerinde | Hayır | Hayır |

## <a name="microsoftstoragesync"></a>Microsoft.StorageSync
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| storagesyncservices | Evet | Evet |

## <a name="microsoftstoragesyncdev"></a>Microsoft. StorageSyncDev
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| storagesyncservices | Hayır | Hayır |

## <a name="microsoftstoragesyncint"></a>Microsoft. Storagesyncınt
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| storagesyncservices | Hayır | Hayır |

## <a name="microsoftstorsimple"></a>Microsoft.StorSimple
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| ilerinde | Hayır | Hayır |

## <a name="microsoftstreamanalytics"></a>Microsoft.StreamAnalytics
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| streammingjobs | Evet | Evet |

> [!IMPORTANT]
> Stream Analytics işleri çalışır durumda olduğunda taşınamaz.

## <a name="microsoftstreamanalyticsexplorer"></a>Microsoft. StreamAnalyticsExplorer
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| lý | Hayır | Hayır |
| ortamlar/EventSources | Hayır | Hayır |
| örnekler | Hayır | Hayır |
| örnekler/ortamlar | Hayır | Hayır |
| örnekler/ortamlar/EventSources | Hayır | Hayır |

## <a name="microsoftterraformoss"></a>Microsoft. Teroyformoss
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| providerkayıtları | Hayır | Hayır |
| Kaynakların | Hayır | Hayır |

## <a name="microsofttimeseriesinsights"></a>Microsoft.TimeSeriesInsights
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| lý | Evet | Evet |
| ortamlar/EventSources | Evet | Evet |
| ortamlar/referencedataset 'ler | Evet | Evet |

## <a name="microsofttoken"></a>Microsoft. Token
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| depolaya | Hayır | Hayır |

## <a name="microsoftvirtualmachineimages"></a>Microsoft. Virtualmachineımages
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| ımagetemplates | Hayır | Hayır |

## <a name="microsoftvisualstudio"></a>Microsoft. VisualStudio
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| account | Evet | Evet |
| Hesap/uzantı | Evet | Evet |
| hesap/proje | Evet | Evet |

> [!IMPORTANT]
> Azure DevOps aboneliğini değiştirmek için, bkz. [faturalandırma için kullanılan Azure aboneliğini değiştirme](/azure/devops/organizations/billing/change-azure-subscription?toc=/azure/azure-resource-manager/toc.json).

## <a name="microsoftvmwarecloudsimple"></a>Microsoft. Vmwarechoparlör basit
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| ayrılmış cloudnodes | Evet | Evet |
| ayrılmış CloudService | Evet | Evet |
| virtualmachines | Evet | Evet |

## <a name="microsoftweb"></a>Microsoft. Web
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| Sertifika | Hayır | Evet |
| connectiongateway 'ler | Evet | Evet |
| bağlantının | Evet | Evet |
| customapsıs | Evet | Evet |
| hostingenvironments | Hayır | Hayır |
| serverfarms | Evet | Evet |
| barındıra | Evet | Evet |
| siteler/premieraddons | Evet | Evet |
| siteler/yuvalar | Evet | Evet |

> [!IMPORTANT]
> Bkz. [App Service taşıma Kılavuzu](./move-limitations/app-service-move-limitations.md).

## <a name="microsoftwindowsiot"></a>Microsoft. Windowsıot
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| deviceservices | Hayır | Hayır |

## <a name="microsoftwindowsvirtualdesktop"></a>Microsoft. WindowsVirtualDesktop
| Kaynak türü | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| applicationgroups | Hayır | Hayır |
| Ana bilgisayar havuzları | Hayır | Hayır |
| Onlarla | Hayır | Hayır |

## <a name="third-party-services"></a>Üçüncü taraf hizmetleri

Üçüncü taraf hizmetler şu anda taşıma işlemini desteklemiyor.

## <a name="next-steps"></a>Sonraki adımlar
Kaynakları taşıma komutları için bkz. [kaynakları yeni kaynak grubuna veya aboneliğe taşıma](resource-group-move-resources.md).

Aynı verileri bir virgülle ayrılmış değerler dosyası ile almak için, [Move-support-resources. csv](https://github.com/tfitzmac/resource-capabilities/blob/master/move-support-resources.csv)dosyasını indirin.
