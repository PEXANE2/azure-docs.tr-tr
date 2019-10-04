---
title: Azure Kaynak türüne göre işlem desteğini taşıma
description: Yeni bir kaynak grubuna veya aboneliğe taşınabilecek Azure Kaynak türlerini listeler.
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: reference
ms.date: 10/03/2019
ms.author: tomfitz
ms.openlocfilehash: 81096a4299ec190cb4299f1b8ea4f9c2533d37ad
ms.sourcegitcommit: 4f7dce56b6e3e3c901ce91115e0c8b7aab26fb72
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/04/2019
ms.locfileid: "71948035"
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
> - [Microsoft. Authorization](#microsoftauthorization)
> - [Microsoft. Automation](#microsoftautomation)
> - [Microsoft. AzureActiveDirectory](#microsoftazureactivedirectory)
> - [Microsoft. AzureData](#microsoftazuredata)
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
> - [Microsoft. Microservices4Spring](#microsoftmicroservices4spring)
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
> - [Microsoft. ResourceGraph](#microsoftresourcegraph)
> - [Microsoft. SaaS](#microsoftsaas)
> - [Microsoft. Scheduler](#microsoftscheduler)
> - [Microsoft. Search](#microsoftsearch)
> - [Microsoft. Security](#microsoftsecurity)
> - [Microsoft. ServerManagement](#microsoftservermanagement)
> - [Microsoft. ServiceBus](#microsoftservicebus)
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
> - [Microsoft. VisualStudio](#microsoftvisualstudio)
> - [Microsoft. Vmwarechoparlör basit](#microsoftvmwarecloudsimple)
> - [Microsoft. Web](#microsoftweb)
> - [Microsoft. Windowsıot](#microsoftwindowsiot)
> - [Microsoft. WindowsVirtualDesktop](#microsoftwindowsvirtualdesktop)

## <a name="microsoftaad"></a>Microsoft. AAD

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Aboneliğiniz |
> | ------------- | ----------- | ---------- |
> | DomainServices | Hayır | Hayır |
> | DomainServices/replicasets | Hayır | Hayır |

## <a name="microsoftaadiam"></a>Microsoft. aadihar

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Aboneliğiniz |
> | ------------- | ----------- | ---------- |
> | Kira | Hayır | Hayır |

## <a name="microsoftalertsmanagement"></a>Microsoft. AlertsManagement

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Aboneliğiniz |
> | ------------- | ----------- | ---------- |
> | actionrules | Evet | Evet |

## <a name="microsoftanalysisservices"></a>Microsoft. AnalysisServices

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Aboneliğiniz |
> | ------------- | ----------- | ---------- |
> | sunucular | Evet | Evet |

## <a name="microsoftapimanagement"></a>Microsoft. Apimanane

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Aboneliğiniz |
> | ------------- | ----------- | ---------- |
> | hizmet | Evet | Evet |

## <a name="microsoftappconfiguration"></a>Microsoft. AppConfiguration

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Aboneliğiniz |
> | ------------- | ----------- | ---------- |
> | configurationmağazaların | Evet | Evet |

## <a name="microsoftappservice"></a>Microsoft. AppService

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Aboneliğiniz |
> | ------------- | ----------- | ---------- |
> | apiapps | Hayır | Hayır |
> | appdentities | Hayır | Hayır |
> | geçidinin | Hayır | Hayır |

> [!IMPORTANT]
> Bkz. [App Service taşıma Kılavuzu](./move-limitations/app-service-move-limitations.md).

## <a name="microsoftauthorization"></a>Microsoft. Authorization

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Aboneliğiniz |
> | ------------- | ----------- | ---------- |
> | poliyasatamaları | Hayır | Hayır |

## <a name="microsoftautomation"></a>Microsoft. Automation

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Aboneliğiniz |
> | ------------- | ----------- | ---------- |
> | automationaccounts | Evet | Evet |
> | automationaccounts/Configurations | Evet | Evet |
> | automationaccounts/runbook 'lar | Evet | Evet |

> [!IMPORTANT]
> Runbook 'lar Otomasyon hesabıyla aynı kaynak grubunda bulunmalıdır.

## <a name="microsoftazureactivedirectory"></a>Microsoft. AzureActiveDirectory

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Aboneliğiniz |
> | ------------- | ----------- | ---------- |
> | b2cdirectories | Evet | Evet |

## <a name="microsoftazuredata"></a>Microsoft. AzureData

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Aboneliğiniz |
> | ------------- | ----------- | ---------- |
> | sqlserverkayıtları | Hayır | Hayır |

## <a name="microsoftazurestack"></a>Microsoft. AzureStack

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Aboneliğiniz |
> | ------------- | ----------- | ---------- |
> | kayıtlarında | Evet | Evet |

## <a name="microsoftbackup"></a>Microsoft. Backup

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Aboneliğiniz |
> | ------------- | ----------- | ---------- |
> | backupvault | Hayır | Hayır |

## <a name="microsoftbatch"></a>Microsoft. Batch

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Aboneliğiniz |
> | ------------- | ----------- | ---------- |
> | batchaccounts | Evet | Evet |

## <a name="microsoftbatchai"></a>Microsoft. Batchaı

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Aboneliğiniz |
> | ------------- | ----------- | ---------- |
> | leriniz | Hayır | Hayır |
> | dosya sunucuları | Hayır | Hayır |
> | Çizelge | Hayır | Hayır |
> | çalışma alanları | Hayır | Hayır |

## <a name="microsoftbingmaps"></a>Microsoft. BingMaps

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Aboneliğiniz |
> | ------------- | ----------- | ---------- |
> | mapapsıs | Hayır | Hayır |

## <a name="microsoftbiztalkservices"></a>Microsoft. BizTalkServices

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Aboneliğiniz |
> | ------------- | ----------- | ---------- |
> | BIZ | Evet | Evet |

## <a name="microsoftblockchain"></a>Microsoft. Blockzinciri

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Aboneliğiniz |
> | ------------- | ----------- | ---------- |
> | blockchainmembers | Evet | Evet |
> | izleyicileri | Hayır | Hayır |

## <a name="microsoftblueprint"></a>Microsoft. Blueprint

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Aboneliğiniz |
> | ------------- | ----------- | ---------- |
> | şema tasmi | Hayır | Hayır |

## <a name="microsoftbotservice"></a>Microsoft. BotService

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Aboneliğiniz |
> | ------------- | ----------- | ---------- |
> | botservices | Evet | Evet |

## <a name="microsoftcache"></a>Microsoft. Cache

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Aboneliğiniz |
> | ------------- | ----------- | ---------- |
> | Redis | Evet | Evet |

> [!IMPORTANT]
> Redsıs örneği için Azure önbelleği bir sanal ağla yapılandırıldıysa, örnek farklı bir aboneliğe taşınamaz. Bkz. [ağ taşıma sınırlamaları](./move-limitations/networking-move-limitations.md).

## <a name="microsoftcdn"></a>Microsoft. CDN

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Aboneliğiniz |
> | ------------- | ----------- | ---------- |
> | cdnwebapplicationfirewallpolicies | Hayır | Hayır |
> | profiller | Evet | Evet |
> | Profiller/uç noktalar | Evet | Evet |

## <a name="microsoftcertificateregistration"></a>Microsoft. CertificateRegistration

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Aboneliğiniz |
> | ------------- | ----------- | ---------- |
> | sertifikadüzenleri | Evet | Evet |

> [!IMPORTANT]
> Bkz. [App Service taşıma Kılavuzu](./move-limitations/app-service-move-limitations.md).

## <a name="microsoftclassiccompute"></a>Microsoft. ClassicCompute

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Aboneliğiniz |
> | ------------- | ----------- | ---------- |
> | DomainNames | Evet | Hayır |
> | virtualmachines | Evet | Hayır |

> [!IMPORTANT]
> Bkz. [klasik dağıtım taşıma Kılavuzu](./move-limitations/classic-model-move-limitations.md). Klasik dağıtım kaynakları, bu senaryoya özel bir işlemle abonelikler arasında taşınabilir.

## <a name="microsoftclassicnetwork"></a>Microsoft. ClassicNetwork

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Aboneliğiniz |
> | ------------- | ----------- | ---------- |
> | networksecuritygroups | Hayır | Hayır |
> | rezervler | Hayır | Hayır |
> | virtualnetworks | Hayır | Hayır |

> [!IMPORTANT]
> Bkz. [klasik dağıtım taşıma Kılavuzu](./move-limitations/classic-model-move-limitations.md). Klasik dağıtım kaynakları, bu senaryoya özel bir işlemle abonelikler arasında taşınabilir.

## <a name="microsoftclassicstorage"></a>Microsoft. ClassicStorage

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Aboneliğiniz |
> | ------------- | ----------- | ---------- |
> | storageaccounts | Evet | Hayır |

> [!IMPORTANT]
> Bkz. [klasik dağıtım taşıma Kılavuzu](./move-limitations/classic-model-move-limitations.md). Klasik dağıtım kaynakları, bu senaryoya özel bir işlemle abonelikler arasında taşınabilir.

## <a name="microsoftcognitiveservices"></a>Microsoft. Biliveservices

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Aboneliğiniz |
> | ------------- | ----------- | ---------- |
> | hesaplar | Evet | Evet |

## <a name="microsoftcompute"></a>Microsoft. COMPUTE

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Aboneliğiniz |
> | ------------- | ----------- | ---------- |
> | availabilitysets | Evet | Evet |
> | diskencryptionsets | Hayır | Hayır |
> | disklerinden | Evet | Evet |
> | Galeriler | Hayır | Hayır |
> | Galeriler/görüntüler | Hayır | Hayır |
> | Galeriler/resimler/sürümler | Hayır | Hayır |
> | hostgroups | Hayır | Hayır |
> | hostgroups/konaklar | Hayır | Hayır |
> | görüntüler | Evet | Evet |
> | proximityplacementgroups | Hayır | Hayır |
> | restorepointcollections | Hayır | Hayır |
> | sharedvmımages | Hayır | Hayır |
> | sharedvmımages/sürümler | Hayır | Hayır |
> | anlık görüntüler | Evet | Evet |
> | virtualmachines | Evet | Evet |
> | virtualmachines/uzantıları | Evet | Evet |
> | virtualmachinescalesets | Evet | Evet |

> [!IMPORTANT]
> Bkz. [sanal makine taşıma Kılavuzu](./move-limitations/virtual-machines-move-limitations.md).

## <a name="microsoftcontainer"></a>Microsoft. Container

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Aboneliğiniz |
> | ------------- | ----------- | ---------- |
> | kapsayıcı grupları | Hayır | Hayır |

## <a name="microsoftcontainerinstance"></a>Microsoft. Containerınstance

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Aboneliğiniz |
> | ------------- | ----------- | ---------- |
> | kapsayıcı grupları | Hayır | Hayır |

## <a name="microsoftcontainerregistry"></a>Microsoft. ContainerRegistry

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Aboneliğiniz |
> | ------------- | ----------- | ---------- |
> | kayıt | Evet | Evet |
> | kayıt defterleri/BuildTasks | Evet | Evet |
> | kayıt defterleri/çoğaltmalar | Evet | Evet |
> | kayıt defterleri/görevler | Evet | Evet |
> | kayıt defterleri/Web kancaları | Evet | Evet |

## <a name="microsoftcontainerservice"></a>Microsoft. ContainerService

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Aboneliğiniz |
> | ------------- | ----------- | ---------- |
> | containerservices | Hayır | Hayır |
> | managedkümeler | Hayır | Hayır |
> | openshiftmanagedclusters | Hayır | Hayır |

## <a name="microsoftcontentmoderator"></a>Microsoft. Contentmoderatör

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Aboneliğiniz |
> | ------------- | ----------- | ---------- |
> | uygulamalar | Evet | Evet |

## <a name="microsoftcortanaanalytics"></a>Microsoft. Cortanaanalizi

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Aboneliğiniz |
> | ------------- | ----------- | ---------- |
> | hesaplar | Hayır | Hayır |

## <a name="microsoftcostmanagement"></a>Microsoft. CostManagement

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Aboneliğiniz |
> | ------------- | ----------- | ---------- |
> | bağlayıcılar | Evet | Evet |

## <a name="microsoftcustomerinsights"></a>Microsoft. Customerınsights

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Aboneliğiniz |
> | ------------- | ----------- | ---------- |
> | Lara | Evet | Evet |

## <a name="microsoftdatabox"></a>Microsoft. DataBox

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Aboneliğiniz |
> | ------------- | ----------- | ---------- |
> | Çizelge | Hayır | Hayır |

## <a name="microsoftdataboxedge"></a>Microsoft. DataBoxEdge

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Aboneliğiniz |
> | ------------- | ----------- | ---------- |
> | databoxedgedevices | Hayır | Hayır |

## <a name="microsoftdatabricks"></a>Microsoft. Databricks

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Aboneliğiniz |
> | ------------- | ----------- | ---------- |
> | çalışma alanları | Hayır | Hayır |

## <a name="microsoftdatacatalog"></a>Microsoft. DataCatalog

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Aboneliğiniz |
> | ------------- | ----------- | ---------- |
> | larına | Evet | Evet |
> | veri katalogları | Hayır | Hayır |

## <a name="microsoftdataconnect"></a>Microsoft. DataConnect

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Aboneliğiniz |
> | ------------- | ----------- | ---------- |
> | connectionyöneticileri | Hayır | Hayır |

## <a name="microsoftdataexchange"></a>Microsoft. DataExchange

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Aboneliğiniz |
> | ------------- | ----------- | ---------- |
> | paketler | Hayır | Hayır |
> | Planlama | Hayır | Hayır |

## <a name="microsoftdatafactory"></a>Microsoft. DataFactory

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Aboneliğiniz |
> | ------------- | ----------- | ---------- |
> | veri fabrikaları | Evet | Evet |
> | oluşturucular | Evet | Evet |

## <a name="microsoftdatalake"></a>Microsoft. DataLake

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Aboneliğiniz |
> | ------------- | ----------- | ---------- |
> | datalakeaccounts | Hayır | Hayır |

## <a name="microsoftdatalakeanalytics"></a>Microsoft. DataLakeAnalytics

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Aboneliğiniz |
> | ------------- | ----------- | ---------- |
> | hesaplar | Evet | Evet |

## <a name="microsoftdatalakestore"></a>Microsoft. DataLakeStore

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Aboneliğiniz |
> | ------------- | ----------- | ---------- |
> | hesaplar | Evet | Evet |

## <a name="microsoftdatamigration"></a>Microsoft. DataMigration

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Aboneliğiniz |
> | ------------- | ----------- | ---------- |
> | hizmetler | Hayır | Hayır |
> | Hizmetler/Projeler | Hayır | Hayır |
> | Lara | Hayır | Hayır |

## <a name="microsoftdbformariadb"></a>Microsoft. Dbformarıdb

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Aboneliğiniz |
> | ------------- | ----------- | ---------- |
> | sunucular | Evet | Evet |

## <a name="microsoftdbformysql"></a>Microsoft. Dbformyısql

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Aboneliğiniz |
> | ------------- | ----------- | ---------- |
> | sunucular | Evet | Evet |

## <a name="microsoftdbforpostgresql"></a>Microsoft. DBforPostgreSQL

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Aboneliğiniz |
> | ------------- | ----------- | ---------- |
> | sunucu grupları | Hayır | Hayır |
> | sunucular | Evet | Evet |
> | serversv2 | Evet | Evet |

## <a name="microsoftdeploymentmanager"></a>Microsoft. DeploymentManager

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Aboneliğiniz |
> | ------------- | ----------- | ---------- |
> | artifactsources | Evet | Evet |
> | piyasaya çıkarma | Evet | Evet |
> | servicetopolojileri | Evet | Evet |
> | servicetopolojileri/hizmetler | Evet | Evet |
> | servicetopolojileri/hizmetler/serviceunits | Evet | Evet |
> | olanları | Evet | Evet |

## <a name="microsoftdevices"></a>Microsoft. Devices

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Aboneliğiniz |
> | ------------- | ----------- | ---------- |
> | elaun havuzları | Hayır | Hayır |
> | elaun havuzları/ıothubkiracılar | Hayır | Hayır |
> | ıothubs | Evet | Evet |
> | provisioningservices | Evet | Evet |

## <a name="microsoftdevspaces"></a>Microsoft. DevSpaces

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Aboneliğiniz |
> | ------------- | ----------- | ---------- |
> | denetleyiciler | Evet | Evet |

## <a name="microsoftdevtestlab"></a>Microsoft. DevTestLab

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Aboneliğiniz |
> | ------------- | ----------- | ---------- |
> | labcenters | Hayır | Hayır |
> | labs | Evet | Hayır |
> | Laboratuvarlar/ortamlar | Evet | Evet |
> | Labs/servicerunanlar | Evet | Evet |
> | Labs/virtualmachines | Evet | Hayır |
> | cağını | Evet | Evet |

## <a name="microsoftdocumentdb"></a>Microsoft. DocumentDB

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Aboneliğiniz |
> | ------------- | ----------- | ---------- |
> | veritabanı hesapları | Evet | Evet |

## <a name="microsoftdomainregistration"></a>Microsoft. DomainRegistration

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Aboneliğiniz |
> | ------------- | ----------- | ---------- |
> | etki alanları | Evet | Evet |

## <a name="microsoftenterpriseknowledgegraph"></a>Microsoft. EnterpriseKnowledgeGraph

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Aboneliğiniz |
> | ------------- | ----------- | ---------- |
> | hizmetler | Evet | Evet |

## <a name="microsofteventgrid"></a>Microsoft. EventGrid

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Aboneliğiniz |
> | ------------- | ----------- | ---------- |
> | etki alanları | Evet | Evet |
> | konuları | Evet | Evet |

## <a name="microsofteventhub"></a>Microsoft. EventHub

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Aboneliğiniz |
> | ------------- | ----------- | ---------- |
> | leriniz | Evet | Evet |
> | ad alanları | Evet | Evet |

## <a name="microsoftgenomics"></a>Microsoft. Genomiks

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Aboneliğiniz |
> | ------------- | ----------- | ---------- |
> | hesaplar | Hayır | Hayır |

## <a name="microsofthanaonazure"></a>Microsoft. HanaOnAzure

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Aboneliğiniz |
> | ------------- | ----------- | ---------- |
> | hanaınstances | Hayır | Hayır |
> | sapizleyicileri | Evet | Evet |

## <a name="microsofthdinsight"></a>Microsoft. HDInsight

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Aboneliğiniz |
> | ------------- | ----------- | ---------- |
> | leriniz | Evet | Evet |

> [!IMPORTANT]
> HDInsight kümelerini yeni bir aboneliğe veya kaynak grubuna taşıyabilirsiniz. Bununla birlikte, HDInsight kümesiyle bağlantılı ağ kaynakları (sanal ağ, NIC veya yük dengeleyici gibi) için abonelikler arasında geçiş yapamazsınız. Ayrıca, küme için bir sanal makineye bağlı bir NIC 'ye yeni bir kaynak grubuna taşıyamazsınız.
>
> HDInsight kümesini yeni bir aboneliğe taşırken, önce diğer kaynakları (depolama hesabı gibi) taşıyın. Ardından, HDInsight kümesini kendi kendine taşıyın.

## <a name="microsofthealthcareapis"></a>Microsoft. Healthgelişme API 'leri

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Aboneliğiniz |
> | ------------- | ----------- | ---------- |
> | hizmetler | Evet | Evet |

## <a name="microsofthybridcompute"></a>Microsoft. HybridCompute

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Aboneliğiniz |
> | ------------- | ----------- | ---------- |
> | larla | Hayır | Hayır |

## <a name="microsofthybriddata"></a>Microsoft. HybridData

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Aboneliğiniz |
> | ------------- | ----------- | ---------- |
> | veri yöneticileri | Evet | Evet |

## <a name="microsoftimportexport"></a>Microsoft. ımportexport

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Aboneliğiniz |
> | ------------- | ----------- | ---------- |
> | Çizelge | Evet | Evet |

## <a name="microsoftinsights"></a>Microsoft. Insights

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Aboneliğiniz |
> | ------------- | ----------- | ---------- |
> | hesaplar | Hayır | Hayır |
> | actiongroups | Evet | Evet |
> | activitylogalerts | Hayır | Hayır |
> | alertrules | Evet | Evet |
> | autoscalesettings | Evet | Evet |
> | bileşenler | Evet | Evet |
> | guestdiagnosticsettings | Hayır | Hayır |
> | metricalerts | Hayır | Hayır |
> | notificationgroups | Hayır | Hayır |
> | notificationrules | Hayır | Hayır |
> | scheduledqueryrules | Evet | Evet |
> | Web testleri | Evet | Evet |
> | çalışma kitapları | Evet | Evet |

> [!IMPORTANT]
> Yeni aboneliğe taşınmasının [abonelik kotalarını](../azure-subscription-service-limits.md#azure-monitor-limits)aşmadığından emin olun.

## <a name="microsoftiotcentral"></a>Microsoft. ıotcentral

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Aboneliğiniz |
> | ------------- | ----------- | ---------- |
> | ıotapps | Evet | Evet |

## <a name="microsoftiotspaces"></a>Microsoft. ıotspaces

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Aboneliğiniz |
> | ------------- | ----------- | ---------- |
> | checknameavaılabılıty | Evet | Evet |
> | çıkarılamıyor | Evet | Evet |

## <a name="microsoftkeyvault"></a>Microsoft. Keykasası

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Aboneliğiniz |
> | ------------- | ----------- | ---------- |
> | hsmpools | Hayır | Hayır |
> | kasaları | Evet | Evet |

> [!IMPORTANT]
> Disk şifrelemesi için kullanılan anahtar kasaları aynı abonelikte veya abonelikler arasında bir kaynak grubuna taşınamaz.

## <a name="microsoftkusto"></a>Microsoft. kusto

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Aboneliğiniz |
> | ------------- | ----------- | ---------- |
> | leriniz | Evet | Evet |

## <a name="microsoftlabservices"></a>Microsoft. LabServices

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Aboneliğiniz |
> | ------------- | ----------- | ---------- |
> | labaccounts | Hayır | Hayır |

## <a name="microsoftlocationbasedservices"></a>Microsoft. LocationBasedServices

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Aboneliğiniz |
> | ------------- | ----------- | ---------- |
> | hesaplar | Hayır | Hayır |

## <a name="microsoftlocationservices"></a>Microsoft. LocationServices

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Aboneliğiniz |
> | ------------- | ----------- | ---------- |
> | hesaplar | Hayır | Hayır |

## <a name="microsoftlogic"></a>Microsoft. Logic

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Aboneliğiniz |
> | ------------- | ----------- | ---------- |
> | hostingenvironments | Hayır | Hayır |
> | Tümleştirme hesapları | Evet | Evet |
> | ıntegrationserviceortamortamları | Hayır | Hayır |
> | ısotedenvironments | Hayır | Hayır |
> | iş akışları | Evet | Evet |

## <a name="microsoftmachinelearning"></a>Microsoft. Machinöğrenim

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Aboneliğiniz |
> | ------------- | ----------- | ---------- |
> | commitmentplanlar | Evet | Evet |
> | Hizmetleri | Evet | Hayır |
> | çalışma alanları | Evet | Evet |

## <a name="microsoftmachinelearningcompute"></a>Microsoft. MachineLearningCompute

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Aboneliğiniz |
> | ------------- | ----------- | ---------- |
> | operationalizationkümeleri | Evet | Evet |

## <a name="microsoftmachinelearningexperimentation"></a>Microsoft. MachineLearningExperimentation

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Aboneliğiniz |
> | ------------- | ----------- | ---------- |
> | hesaplar | Hayır | Hayır |
> | hesaplar/çalışma alanları | Hayır | Hayır |
> | hesaplar/çalışma alanları/projeler | Hayır | Hayır |
> | teamaccounts | Hayır | Hayır |
> | teamaccounts/çalışma alanları | Hayır | Hayır |
> | teamaccounts/çalışma alanları/projeler | Hayır | Hayır |

## <a name="microsoftmachinelearningmodelmanagement"></a>Microsoft. MachineLearningModelManagement

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Aboneliğiniz |
> | ------------- | ----------- | ---------- |
> | hesaplar | Hayır | Hayır |

## <a name="microsoftmachinelearningoperationalization"></a>Microsoft. MachineLearningOperationalization

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Aboneliğiniz |
> | ------------- | ----------- | ---------- |
> | hostinghesapları | Hayır | Hayır |

## <a name="microsoftmachinelearningservices"></a>Microsoft. MachineLearningServices

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Aboneliğiniz |
> | ------------- | ----------- | ---------- |
> | çalışma alanları | Hayır | Hayır |

## <a name="microsoftmanagedidentity"></a>Microsoft. Managedıdentity

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Aboneliğiniz |
> | ------------- | ----------- | ---------- |
> | userassignedıdentities | Hayır | Hayır |

## <a name="microsoftmaps"></a>Microsoft. Maps

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Aboneliğiniz |
> | ------------- | ----------- | ---------- |
> | hesaplar | Evet | Evet |

## <a name="microsoftmarketplaceapps"></a>Microsoft. MarketplaceApps

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Aboneliğiniz |
> | ------------- | ----------- | ---------- |
> | classicdevservices | Hayır | Hayır |

## <a name="microsoftmedia"></a>Microsoft. Media

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Aboneliğiniz |
> | ------------- | ----------- | ---------- |
> | mediaservices | Evet | Evet |
> | mediaservices/liveevents | Evet | Evet |
> | mediaservices/streamingendpoints | Evet | Evet |

## <a name="microsoftmicroservices4spring"></a>Microsoft. Microservices4Spring

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Aboneliğiniz |
> | ------------- | ----------- | ---------- |
> | appkümeler | Hayır | Hayır |

## <a name="microsoftmigrate"></a>Microsoft. Migrate

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Aboneliğiniz |
> | ------------- | ----------- | ---------- |
> | assessmentprojects | Hayır | Hayır |
> | migrateprojects | Hayır | Hayır |
> | projeler | Hayır | Hayır |

## <a name="microsoftnetapp"></a>Microsoft. NetApp

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Aboneliğiniz |
> | ------------- | ----------- | ---------- |
> | netappaccounts | Hayır | Hayır |
> | netappaccounts/capacityhavuzları | Hayır | Hayır |
> | netappaccounts/capacityhavuzları/birimleri | Hayır | Hayır |
> | netappaccounts/capacityhavuzlar/Volumes/Mount hedefleri | Hayır | Hayır |
> | netappaccounts/capacityhavuzlar/birimler/anlık görüntüler | Hayır | Hayır |

## <a name="microsoftnetwork"></a>Microsoft. Network

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Aboneliğiniz |
> | ------------- | ----------- | ---------- |
> | applicationgateway 'ler | Hayır | Hayır |
> | applicationgatewaywebapplicationfirewallpolicies | Hayır | Hayır |
> | applicationsecuritygroups | Evet | Evet |
> | azurefirewalls | Evet | Evet |
> | Savunma Konakları | Hayır | Hayır |
> | bağlantılar | Evet | Evet |
> | ddoscustompolicies | Evet | Evet |
> | ddosprotectionplanlar | Hayır | Hayır |
> | dnszones | Evet | Evet |
> | expressroutedevreleri | Hayır | Hayır |
> | expressroutecrossconnections | Hayır | Hayır |
> | expressroutegateway 'ler | Hayır | Hayır |
> | expressrouteports | Hayır | Hayır |
> | frontkapıların | Hayır | Hayır |
> | frontdoorwebapplicationfirewallpolicies | Hayır | Hayır |
> | loadbalancers | Evet-temel SKU<br>Standart SKU yok | Evet-temel SKU<br>Standart SKU yok |
> | localnetworkgateway 'ler | Evet | Evet |
> | natgateway 'ler | Evet | Evet |
> | networkıntpolicies ilkeleri | Evet | Evet |
> | NetworkInterfaces | Evet | Evet |
> | networkprofiles | Hayır | Hayır |
> | networksecuritygroups | Evet | Evet |
> | networkwatchers | Evet | Evet |
> | networkwatchers/connectionmonitörleri | Evet | Evet |
> | networkwatchers/uzunluler | Evet | Evet |
> | networkwatchers/pingkafesler | Evet | Evet |
> | p2svpngateways | Hayır | Hayır |
> | privatednszones | Evet | Evet |
> | privatednszones/virtualnetworklinks | Evet | Evet |
> | privateendpoints | Hayır | Hayır |
> | privatelinkservices | Hayır | Hayır |
> | publicıpaddresses | Evet-temel SKU<br>Standart SKU yok | Evet-temel SKU<br>Standart SKU yok |
> | publicıpöneklerini | Evet | Evet |
> | routefilters | Hayır | Hayır |
> | routetables | Evet | Evet |
> | securegateyöntemlere | Evet | Evet |
> | serviceendpointpolicies | Evet | Evet |
> | trafficmanagerprofiles | Evet | Evet |
> | virtualhub 'lar | Hayır | Hayır |
> | virtualnetworkgateways | Evet | Evet |
> | virtualnetworks | Evet | Evet |
> | virtualnetworktaps | Hayır | Hayır |
> | virtualwan | Hayır | Hayır |
> | vpngateway (sanal WAN) | Hayır | Hayır |
> | vpnsites (sanal WAN) | Hayır | Hayır |
> | webapplicationfirewallpolicies | Evet | Evet |

> [!IMPORTANT]
> Bkz. [ağ taşıma Kılavuzu](./move-limitations/networking-move-limitations.md).

## <a name="microsoftnotificationhubs"></a>Microsoft. Notificationhub 'Lar

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Aboneliğiniz |
> | ------------- | ----------- | ---------- |
> | ad alanları | Evet | Evet |
> | ad alanları/notificationhub 'lar | Evet | Evet |

## <a name="microsoftoperationalinsights"></a>Microsoft. Operationalınsights

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Aboneliğiniz |
> | ------------- | ----------- | ---------- |
> | çalışma alanları | Evet | Evet |

> [!IMPORTANT]
> Yeni aboneliğe taşınmasının [abonelik kotalarını](../azure-subscription-service-limits.md#azure-monitor-limits)aşmadığından emin olun.

## <a name="microsoftoperationsmanagement"></a>Microsoft. OperationsManagement

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Aboneliğiniz |
> | ------------- | ----------- | ---------- |
> | managementconfigurations | Evet | Evet |
> | çözümler | Evet | Evet |
> | görünümler | Evet | Evet |

## <a name="microsoftpeering"></a>Microsoft. eşleme

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Aboneliğiniz |
> | ------------- | ----------- | ---------- |
> | eşlemeleri | Hayır | Hayır |

## <a name="microsoftportal"></a>Microsoft. Portal

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Aboneliğiniz |
> | ------------- | ----------- | ---------- |
> | panolar | Evet | Evet |

## <a name="microsoftportalsdk"></a>Microsoft. PortalSdk

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Aboneliğiniz |
> | ------------- | ----------- | ---------- |
> | rootresources | Hayır | Hayır |

## <a name="microsoftpowerbi"></a>Microsoft. PowerBI

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Aboneliğiniz |
> | ------------- | ----------- | ---------- |
> | workspacecollections | Evet | Evet |

## <a name="microsoftpowerbidedicated"></a>Microsoft. Powerbiadanmış

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Aboneliğiniz |
> | ------------- | ----------- | ---------- |
> | kapasiteler | Evet | Evet |

## <a name="microsoftprojectoxford"></a>Microsoft. ProjectOxford

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Aboneliğiniz |
> | ------------- | ----------- | ---------- |
> | hesaplar | Hayır | Hayır |

## <a name="microsoftrecoveryservices"></a>Microsoft. RecoveryServices

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Aboneliğiniz |
> | ------------- | ----------- | ---------- |
> | kasaları | Evet | Evet |

> [!IMPORTANT]
> Bkz. [Kurtarma Hizmetleri taşıma Kılavuzu](../backup/backup-azure-move-recovery-services-vault.md?toc=/azure/azure-resource-manager/toc.json).

## <a name="microsoftrelay"></a>Microsoft. Relay

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Aboneliğiniz |
> | ------------- | ----------- | ---------- |
> | ad alanları | Evet | Evet |

## <a name="microsoftresourcegraph"></a>Microsoft. ResourceGraph

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Aboneliğiniz |
> | ------------- | ----------- | ---------- |
> | sorgular | Evet | Evet |

## <a name="microsoftsaas"></a>Microsoft. SaaS

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Aboneliğiniz |
> | ------------- | ----------- | ---------- |
> | uygulamalar | Evet | Hayır |

## <a name="microsoftscheduler"></a>Microsoft. Scheduler

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Aboneliğiniz |
> | ------------- | ----------- | ---------- |
> | var | Evet | Evet |
> | işlere | Evet | Evet |

## <a name="microsoftsearch"></a>Microsoft. Search

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Aboneliğiniz |
> | ------------- | ----------- | ---------- |
> | searchservices | Evet | Evet |

> [!IMPORTANT]
> Farklı bölgelerde çeşitli arama kaynaklarını tek bir işlemde taşıyamazsınız. Bunun yerine, bunları ayrı işlemlerde taşıyın.

## <a name="microsoftsecurity"></a>Microsoft.Security

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Aboneliğiniz |
> | ------------- | ----------- | ---------- |
> | ıotsecuritysolutions | Evet | Evet |
> | playbookconfigurations | Hayır | Hayır |

## <a name="microsoftservermanagement"></a>Microsoft. ServerManagement

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Aboneliğiniz |
> | ------------- | ----------- | ---------- |
> | geçidinin | Hayır | Hayır |
> | düğümler | Hayır | Hayır |

## <a name="microsoftservicebus"></a>Microsoft. ServiceBus

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Aboneliğiniz |
> | ------------- | ----------- | ---------- |
> | ad alanları | Evet | Evet |

## <a name="microsoftservicefabric"></a>Microsoft. ServiceFabric

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Aboneliğiniz |
> | ------------- | ----------- | ---------- |
> | uygulamalar | Hayır | Hayır |
> | leriniz | Evet | Evet |
> | kümeler/uygulamalar | Hayır | Hayır |
> | kapsayıcı grupları | Hayır | Hayır |
> | containergroupsets | Hayır | Hayır |
> | edgeclusters | Hayır | Hayır |
> | ağlar | Hayır | Hayır |
> | secretmağazaları | Hayır | Hayır |
> | Dörtten | Hayır | Hayır |

## <a name="microsoftservicefabricmesh"></a>Microsoft. Servicefabrickafesi

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Aboneliğiniz |
> | ------------- | ----------- | ---------- |
> | uygulamalar | Evet | Evet |
> | kapsayıcı grupları | Hayır | Hayır |
> | geçidinin | Evet | Evet |
> | ağlar | Evet | Evet |
> | kaynaklanır | Evet | Evet |
> | Dörtten | Evet | Evet |

## <a name="microsoftsignalrservice"></a>Microsoft. SignalRService

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Aboneliğiniz |
> | ------------- | ----------- | ---------- |
> | SignalR | Evet | Evet |

## <a name="microsoftsiterecovery"></a>Microsoft. Sıterecovery

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Aboneliğiniz |
> | ------------- | ----------- | ---------- |
> | siterecoveryvault | Hayır | Hayır |

> [!IMPORTANT]
> Bkz. [Kurtarma Hizmetleri taşıma Kılavuzu](../backup/backup-azure-move-recovery-services-vault.md?toc=/azure/azure-resource-manager/toc.json).

## <a name="microsoftsolutions"></a>Microsoft. Solutions

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Aboneliğiniz |
> | ------------- | ----------- | ---------- |
> | appliancedefinitions | Hayır | Hayır |
> | yaptı | Hayır | Hayır |
> | applicationdefinitions | Hayır | Hayır |
> | uygulamalar | Hayır | Hayır |
> | jistekleri | Hayır | Hayır |

## <a name="microsoftsql"></a>Microsoft. SQL

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Aboneliğiniz |
> | ------------- | ----------- | ---------- |
> | ınstancepools | Hayır | Hayır |
> | ManagedInstances | Hayır | Hayır |
> | ManagedInstances/veritabanları | Hayır | Hayır |
> | sunucular | Evet | Evet |
> | sunucular/veritabanları | Evet | Evet |
> | sunucular/elaun havuzları | Evet | Evet |
> | virtualkümeler | Evet | Evet |

> [!IMPORTANT]
> Bir veritabanı ve sunucu aynı kaynak grubunda olmalıdır. Bir SQL sunucusunu taşıdığınızda, tüm veritabanları da taşınır. Bu davranış, Azure SQL veritabanı ve Azure SQL veri ambarı veritabanları için geçerlidir.

## <a name="microsoftsqlvirtualmachine"></a>Microsoft. SqlVirtualMachine

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Aboneliğiniz |
> | ------------- | ----------- | ---------- |
> | sqlvirtualmachinegroups | Evet | Evet |
> | sqlvirtualmachines | Evet | Evet |

## <a name="microsoftsqlvm"></a>Microsoft. SqlVM

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Aboneliğiniz |
> | ------------- | ----------- | ---------- |
> | dwvm | Hayır | Hayır |

## <a name="microsoftstorage"></a>Microsoft. Storage

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Aboneliğiniz |
> | ------------- | ----------- | ---------- |
> | storageaccounts | Evet | Evet |

## <a name="microsoftstoragecache"></a>Microsoft. StorageCache

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Aboneliğiniz |
> | ------------- | ----------- | ---------- |
> | önbelleklerinde | Hayır | Hayır |

## <a name="microsoftstoragesync"></a>Microsoft. Storagessync

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Aboneliğiniz |
> | ------------- | ----------- | ---------- |
> | storagesyncservices | Evet | Evet |

## <a name="microsoftstoragesyncdev"></a>Microsoft. StorageSyncDev

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Aboneliğiniz |
> | ------------- | ----------- | ---------- |
> | storagesyncservices | Hayır | Hayır |

## <a name="microsoftstoragesyncint"></a>Microsoft. Storagesyncınt

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Aboneliğiniz |
> | ------------- | ----------- | ---------- |
> | storagesyncservices | Hayır | Hayır |

## <a name="microsoftstorsimple"></a>Microsoft. StorSimple

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Aboneliğiniz |
> | ------------- | ----------- | ---------- |
> | ilerinde | Hayır | Hayır |

## <a name="microsoftstreamanalytics"></a>Microsoft. StreamAnalytics

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Aboneliğiniz |
> | ------------- | ----------- | ---------- |
> | streammingjobs | Evet | Evet |

> [!IMPORTANT]
> Stream Analytics işleri çalışır durumda olduğunda taşınamaz.

## <a name="microsoftstreamanalyticsexplorer"></a>Microsoft. StreamAnalyticsExplorer

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Aboneliğiniz |
> | ------------- | ----------- | ---------- |
> | ortamlar | Hayır | Hayır |
> | ortamlar/EventSources | Hayır | Hayır |
> | örnekler | Hayır | Hayır |
> | örnekler/ortamlar | Hayır | Hayır |
> | örnekler/ortamlar/EventSources | Hayır | Hayır |

## <a name="microsoftterraformoss"></a>Microsoft. Teroyformoss

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Aboneliğiniz |
> | ------------- | ----------- | ---------- |
> | providerkayıtları | Hayır | Hayır |
> | kaynaklar | Hayır | Hayır |

## <a name="microsofttimeseriesinsights"></a>Microsoft. Timeseriesınsights

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Aboneliğiniz |
> | ------------- | ----------- | ---------- |
> | ortamlar | Evet | Evet |
> | ortamlar/EventSources | Evet | Evet |
> | ortamlar/referencedataset 'ler | Evet | Evet |

## <a name="microsofttoken"></a>Microsoft. Token

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Aboneliğiniz |
> | ------------- | ----------- | ---------- |
> | depolar | Hayır | Hayır |

## <a name="microsoftvirtualmachineimages"></a>Microsoft. Virtualmachineımages

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Aboneliğiniz |
> | ------------- | ----------- | ---------- |
> | ımagetemplates | Hayır | Hayır |

## <a name="microsoftvisualstudio"></a>Microsoft. VisualStudio

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Aboneliğiniz |
> | ------------- | ----------- | ---------- |
> | hesabı | Evet | Evet |
> | Hesap/uzantı | Evet | Evet |
> | hesap/proje | Evet | Evet |

> [!IMPORTANT]
> Azure DevOps aboneliğini değiştirmek için, bkz. [faturalandırma için kullanılan Azure aboneliğini değiştirme](/azure/devops/organizations/billing/change-azure-subscription?toc=/azure/azure-resource-manager/toc.json).

## <a name="microsoftvmwarecloudsimple"></a>Microsoft. Vmwarechoparlör basit

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Aboneliğiniz |
> | ------------- | ----------- | ---------- |
> | ayrılmış cloudnodes | Evet | Evet |
> | ayrılmış CloudService | Evet | Evet |
> | virtualmachines | Evet | Evet |

## <a name="microsoftweb"></a>Microsoft. Web

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Aboneliğiniz |
> | ------------- | ----------- | ---------- |
> | sertifikalar | Hayır | Evet |
> | connectiongateway 'ler | Evet | Evet |
> | bağlantılar | Evet | Evet |
> | customapsıs | Evet | Evet |
> | hostingenvironments | Hayır | Hayır |
> | serverfarms | Evet | Evet |
> | barındıra | Evet | Evet |
> | siteler/premieraddons | Evet | Evet |
> | siteler/yuvalar | Evet | Evet |

> [!IMPORTANT]
> Bkz. [App Service taşıma Kılavuzu](./move-limitations/app-service-move-limitations.md).

## <a name="microsoftwindowsiot"></a>Microsoft. Windowsıot

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Aboneliğiniz |
> | ------------- | ----------- | ---------- |
> | deviceservices | Hayır | Hayır |

## <a name="microsoftwindowsvirtualdesktop"></a>Microsoft. WindowsVirtualDesktop

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Aboneliğiniz |
> | ------------- | ----------- | ---------- |
> | applicationgroups | Hayır | Hayır |
> | Ana bilgisayar havuzları | Hayır | Hayır |
> | çalışma alanları | Hayır | Hayır |

## <a name="third-party-services"></a>Üçüncü taraf hizmetleri

Üçüncü taraf hizmetler şu anda taşıma işlemini desteklemiyor.

## <a name="next-steps"></a>Sonraki adımlar
Kaynakları taşıma komutları için bkz. [kaynakları yeni kaynak grubuna veya aboneliğe taşıma](resource-group-move-resources.md).

Aynı verileri bir virgülle ayrılmış değerler dosyası ile almak için, [Move-support-resources. csv](https://github.com/tfitzmac/resource-capabilities/blob/master/move-support-resources.csv)dosyasını indirin.
