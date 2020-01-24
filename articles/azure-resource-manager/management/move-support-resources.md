---
title: Kaynak türüne göre işlem desteğini taşıma
description: Yeni bir kaynak grubuna veya aboneliğe taşınabilecek Azure Kaynak türlerini listeler.
ms.topic: conceptual
ms.date: 01/22/2020
ms.openlocfilehash: 3fe404b8b501056f75ac45ccebc5bdbed1acbe43
ms.sourcegitcommit: af6847f555841e838f245ff92c38ae512261426a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/23/2020
ms.locfileid: "76705488"
---
# <a name="move-operation-support-for-resources"></a>Kaynaklar için taşıma işlemi desteği
Bu makalede, bir Azure Kaynak türünün taşıma işlemini destekleyip desteklemediğini listelenmiştir. Ayrıca bir kaynağı taşırken göz önünde bulundurmanız gereken özel koşullar hakkında bilgi sağlar.

Kaynak sağlayıcısı ad alanına atlayın:
> [!div class="op_single_selector"]
> - [Microsoft. AAD](#microsoftaad)
> - [Microsoft. aadihar](#microsoftaadiam)
> - [Microsoft. Advisor](#microsoftadvisor)
> - [Microsoft. AlertsManagement](#microsoftalertsmanagement)
> - [Microsoft. AnalysisServices](#microsoftanalysisservices)
> - [Microsoft. Apimanane](#microsoftapimanagement)
> - [Microsoft. AppConfiguration](#microsoftappconfiguration)
> - [Microsoft. AppPlatform](#microsoftappplatform)
> - [Microsoft. AppService](#microsoftappservice)
> - [Microsoft.Authorization](#microsoftauthorization)
> - [Microsoft. Automation](#microsoftautomation)
> - [Microsoft. AzureActiveDirectory](#microsoftazureactivedirectory)
> - [Microsoft. AzureData](#microsoftazuredata)
> - [Microsoft. AzureStack](#microsoftazurestack)
> - [Microsoft. Batch](#microsoftbatch)
> - [Microsoft. Batchaı](#microsoftbatchai)
> - [Microsoft. Faturalandırma](#microsoftbilling)
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
> - [Microsoft. tüketim](#microsoftconsumption)
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
> - [Microsoft. DataProtection](#microsoftdataprotection)
> - [Microsoft. DataShare](#microsoftdatashare)
> - [Microsoft. Dbformarıdb](#microsoftdbformariadb)
> - [Microsoft. Dbformyısql](#microsoftdbformysql)
> - [Microsoft. DBforPostgreSQL](#microsoftdbforpostgresql)
> - [Microsoft. DeploymentManager](#microsoftdeploymentmanager)
> - [Microsoft. Devices](#microsoftdevices)
> - [Microsoft. DevOps](#microsoftdevops)
> - [Microsoft. DevSpaces](#microsoftdevspaces)
> - [Microsoft. DevTestLab](#microsoftdevtestlab)
> - [Microsoft. DocumentDB](#microsoftdocumentdb)
> - [Microsoft. DomainRegistration](#microsoftdomainregistration)
> - [Microsoft. EnterpriseKnowledgeGraph](#microsoftenterpriseknowledgegraph)
> - [Microsoft. EventGrid](#microsofteventgrid)
> - [Microsoft. EventHub](#microsofteventhub)
> - [Microsoft. Genomiks](#microsoftgenomics)
> - [Microsoft. GuestConfiguration](#microsoftguestconfiguration)
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
> - [Microsoft. Kubernetes](#microsoftkubernetes)
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
> - [Microsoft. ManagedServices](#microsoftmanagedservices)
> - [Microsoft. Maps](#microsoftmaps)
> - [Microsoft. MarketplaceApps](#microsoftmarketplaceapps)
> - [Microsoft. Media](#microsoftmedia)
> - [Microsoft. Microservices4Spring](#microsoftmicroservices4spring)
> - [Microsoft. Migrate](#microsoftmigrate)
> - [Microsoft. NetApp](#microsoftnetapp)
> - [Microsoft. Network](#microsoftnetwork)
> - [Microsoft. Notificationhub 'Lar](#microsoftnotificationhubs)
> - [Microsoft. ObjectStore](#microsoftobjectstore)
> - [Microsoft. Operationalınsights](#microsoftoperationalinsights)
> - [Microsoft. OperationsManagement](#microsoftoperationsmanagement)
> - [Microsoft. eşleme](#microsoftpeering)
> - [Microsoft.PolicyInsights](#microsoftpolicyinsights)
> - [Microsoft. Portal](#microsoftportal)
> - [Microsoft. PortalSdk](#microsoftportalsdk)
> - [Microsoft. PowerBI](#microsoftpowerbi)
> - [Microsoft. Powerbiadanmış](#microsoftpowerbidedicated)
> - [Microsoft. ProjectBabylon](#microsoftprojectbabylon)
> - [Microsoft. ProjectOxford](#microsoftprojectoxford)
> - [Microsoft. ProviderHub](#microsoftproviderhub)
> - [Microsoft. RecoveryServices](#microsoftrecoveryservices)
> - [Microsoft. Relay](#microsoftrelay)
> - [Microsoft. ResourceGraph](#microsoftresourcegraph)
> - [Microsoft. ResourceHealth](#microsoftresourcehealth)
> - [Microsoft. resources](#microsoftresources)
> - [Microsoft. SaaS](#microsoftsaas)
> - [Microsoft. Scheduler](#microsoftscheduler)
> - [Microsoft. Search](#microsoftsearch)
> - [Microsoft. Security](#microsoftsecurity)
> - [Microsoft. Securityınsights](#microsoftsecurityinsights)
> - [Microsoft. ServerManagement](#microsoftservermanagement)
> - [Microsoft.ServiceBus](#microsoftservicebus)
> - [Microsoft. ServiceFabric](#microsoftservicefabric)
> - [Microsoft. Servicefabrickafesi](#microsoftservicefabricmesh)
> - [Microsoft. Services](#microsoftservices)
> - [Microsoft. SignalRService](#microsoftsignalrservice)
> - [Microsoft. SoftwarePlan](#microsoftsoftwareplan)
> - [Microsoft. Solutions](#microsoftsolutions)
> - [Microsoft. SQL](#microsoftsql)
> - [Microsoft. SqlVirtualMachine](#microsoftsqlvirtualmachine)
> - [Microsoft. SqlVM](#microsoftsqlvm)
> - [Microsoft. Storage](#microsoftstorage)
> - [Microsoft. Storagessync](#microsoftstoragesync)
> - [Microsoft. StorageSyncDev](#microsoftstoragesyncdev)
> - [Microsoft. Storagesyncınt](#microsoftstoragesyncint)
> - [Microsoft. StorSimple](#microsoftstorsimple)
> - [Microsoft. StreamAnalytics](#microsoftstreamanalytics)
> - [Microsoft. StreamAnalyticsExplorer](#microsoftstreamanalyticsexplorer)
> - [Microsoft. Subscription](#microsoftsubscription)
> - [Microsoft. support](#microsoftsupport)
> - [Microsoft. Teroyformoss](#microsoftterraformoss)
> - [Microsoft. Timeseriesınsights](#microsofttimeseriesinsights)
> - [Microsoft. Token](#microsofttoken)
> - [microsoft.visualstudio](#microsoftvisualstudio)
> - [Microsoft. Vmwarechoparlör basit](#microsoftvmwarecloudsimple)
> - [Microsoft. VSOnline](#microsoftvsonline)
> - [Microsoft. Web](#microsoftweb)
> - [Microsoft. Windowsıot](#microsoftwindowsiot)
> - [Microsoft. WorkloadMonitor](#microsoftworkloadmonitor)

## <a name="microsoftaad"></a>Microsoft.AAD

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | DomainServices | Hayır | Hayır |

## <a name="microsoftaadiam"></a>microsoft.aadiam

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | Kira | Hayır | Hayır |

## <a name="microsoftadvisor"></a>Microsoft.Advisor

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | yapılandırmalar | Hayır | Hayır |
> | Önerileri | Hayır | Hayır |
> | gizlemeleri | Hayır | Hayır |

## <a name="microsoftalertsmanagement"></a>Microsoft.AlertsManagement

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | actionrules | Evet | Evet |
> | alerts | Hayır | Hayır |
> | alertssummary | Hayır | Hayır |
> | smartdetectoralertrules | Evet | Evet |

## <a name="microsoftanalysisservices"></a>Microsoft.AnalysisServices

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | sunucular | Evet | Evet |

## <a name="microsoftapimanagement"></a>Microsoft.ApiManagement

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | hizmet | Evet | Evet |

## <a name="microsoftappconfiguration"></a>Microsoft. AppConfiguration

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | configurationmağazaların | Evet | Evet |

## <a name="microsoftappplatform"></a>Microsoft. AppPlatform

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | yay | Evet | Evet |

## <a name="microsoftappservice"></a>Microsoft. AppService

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | apiapps | Hayır | Hayır |
> | appdentities | Hayır | Hayır |
> | geçidinin | Hayır | Hayır |

> [!IMPORTANT]
> Bkz. [App Service taşıma Kılavuzu](./move-limitations/app-service-move-limitations.md).

## <a name="microsoftauthorization"></a>Microsoft. Authorization

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | CheckAccess | Hayır | Hayır |
> | denyasatamaları | Hayır | Hayır |
> | findorphanroleatamalar | Hayır | Hayır |
> | kilitler | Hayır | Hayır |
> | izinler | Hayır | Hayır |
> | poliyasatamaları | Hayır | Hayır |
> | PolicyDefinitions | Hayır | Hayır |
> | policysetdefinitions | Hayır | Hayır |
> | roleassignments | Hayır | Hayır |
> | roleatamasussusageölçümleri | Hayır | Hayır |
> | roledefinitions | Hayır | Hayır |

## <a name="microsoftautomation"></a>Microsoft. Automation

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | automationaccounts | Evet | Evet |
> | automationaccounts/Configurations | Evet | Evet |
> | automationaccounts/runbook 'lar | Evet | Evet |

> [!IMPORTANT]
> Runbook 'lar Otomasyon hesabıyla aynı kaynak grubunda bulunmalıdır.

## <a name="microsoftazureactivedirectory"></a>Microsoft.AzureActiveDirectory

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | b2cdirectories | Evet | Evet |

## <a name="microsoftazuredata"></a>Microsoft. AzureData

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | hybriddatayöneticileri | Hayır | Hayır |
> | postgresınstances | Hayır | Hayır |
> | sqlbıgdatakümeler | Hayır | Hayır |
> | SQLInstances | Hayır | Hayır |
> | sqlserverkayıtları | Evet | Evet |

## <a name="microsoftazurestack"></a>Microsoft.AzureStack

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | kayıtlarında | Evet | Evet |

## <a name="microsoftbatch"></a>Microsoft.Batch

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | batchaccounts | Evet | Evet |

## <a name="microsoftbatchai"></a>Microsoft.BatchAI

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | leriniz | Hayır | Hayır |
> | dosya sunucuları | Hayır | Hayır |
> | Çizelge | Hayır | Hayır |
> | çalışma alanları | Hayır | Hayır |

## <a name="microsoftbilling"></a>Microsoft.Billing

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | billingdönemler | Hayır | Hayır |
> | billingpermissions | Hayır | Hayır |
> | billingroleatamaları | Hayır | Hayır |
> | billingroledefinitions | Hayır | Hayır |
> | createbillingroleatama | Hayır | Hayır |

## <a name="microsoftbingmaps"></a>Microsoft.BingMaps

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | mapapsıs | Hayır | Hayır |

## <a name="microsoftbiztalkservices"></a>Microsoft. BizTalkServices

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | biztalk | Hayır | Hayır |

## <a name="microsoftblockchain"></a>Microsoft. Blockzinciri

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | blockchainmembers | Hayır | Hayır |
> | izleyici | Hayır | Hayır |

## <a name="microsoftblueprint"></a>Microsoft. Blueprint

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | şema tasmi | Hayır | Hayır |
> | Blueprint | Hayır | Hayır |

## <a name="microsoftbotservice"></a>Microsoft. BotService

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | botservices | Evet | Evet |

## <a name="microsoftcache"></a>Microsoft.Cache

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | Redis | Evet | Evet |

> [!IMPORTANT]
> Redsıs örneği için Azure önbelleği bir sanal ağla yapılandırıldıysa, örnek farklı bir aboneliğe taşınamaz. Bkz. [ağ taşıma sınırlamaları](./move-limitations/networking-move-limitations.md).

## <a name="microsoftcdn"></a>Microsoft.Cdn

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | cdnwebapplicationfirewallpolicies | Evet | Evet |
> | profiles | Evet | Evet |
> | Profiller/uç noktalar | Evet | Evet |

## <a name="microsoftcertificateregistration"></a>Microsoft.CertificateRegistration

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | sertifikadüzenleri | Evet | Evet |

> [!IMPORTANT]
> Bkz. [App Service taşıma Kılavuzu](./move-limitations/app-service-move-limitations.md).

## <a name="microsoftclassiccompute"></a>Microsoft.ClassicCompute

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | DomainNames | Evet | Hayır |
> | virtualmachines | Evet | Hayır |

> [!IMPORTANT]
> Bkz. [klasik dağıtım taşıma Kılavuzu](./move-limitations/classic-model-move-limitations.md). Klasik dağıtım kaynakları, bu senaryoya özel bir işlemle abonelikler arasında taşınabilir.

## <a name="microsoftclassicnetwork"></a>Microsoft.ClassicNetwork

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | networksecuritygroups | Hayır | Hayır |
> | rezervler | Hayır | Hayır |
> | virtualnetworks | Hayır | Hayır |

> [!IMPORTANT]
> Bkz. [klasik dağıtım taşıma Kılavuzu](./move-limitations/classic-model-move-limitations.md). Klasik dağıtım kaynakları, bu senaryoya özel bir işlemle abonelikler arasında taşınabilir.

## <a name="microsoftclassicstorage"></a>Microsoft.ClassicStorage

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | storageaccounts | Evet | Hayır |

> [!IMPORTANT]
> Bkz. [klasik dağıtım taşıma Kılavuzu](./move-limitations/classic-model-move-limitations.md). Klasik dağıtım kaynakları, bu senaryoya özel bir işlemle abonelikler arasında taşınabilir.

## <a name="microsoftcognitiveservices"></a>Microsoft.CognitiveServices

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | accounts | Evet | Evet |

## <a name="microsoftcompute"></a>Microsoft.Compute

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | availabilitysets | Evet | Evet |
> | diskencryptionsets | Hayır | Hayır |
> | diskler | Evet | Evet |
> | Galeriler | Hayır | Hayır |
> | Galeriler/görüntüler | Hayır | Hayır |
> | Galeriler/resimler/sürümler | Hayır | Hayır |
> | hostgroups | Hayır | Hayır |
> | hostgroups/konaklar | Hayır | Hayır |
> | images | Evet | Evet |
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

## <a name="microsoftconsumption"></a>Microsoft. tüketim

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | aggregmalyt maliyeti | Hayır | Hayır |
> | dengeler | Hayır | Hayır |
> | bütçelerinin | Hayır | Hayır |
> | ücretler | Hayır | Hayır |
> | costtags | Hayır | Hayır |
> | iler | Hayır | Hayır |
> | etkinlikler | Hayır | Hayır |
> | hesaplansın | Hayır | Hayır |
> | oluş | Hayır | Hayır |
> | Marketlerinden | Hayır | Hayır |
> | operationresults | Hayır | Hayır |
> | OperationStatus | Hayır | Hayır |
> | Pricesheets | Hayır | Hayır |
> | ürünler | Hayır | Hayır |
> | rezervde ayrıntıları | Hayır | Hayır |
> | rezervationönerilere | Hayır | Hayır |
> | rezervlerin Özeti | Hayır | Hayır |
> | rezervlik işlemleri | Hayır | Hayır |
> | etiketler | Hayır | Hayır |
> | Kira | Hayır | Hayır |
> | koşullar | Hayır | Hayır |
> | UsageDetails | Hayır | Hayır |

## <a name="microsoftcontainer"></a>Microsoft. Container

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | kapsayıcı grupları | Hayır | Hayır |

## <a name="microsoftcontainerinstance"></a>Microsoft.ContainerInstance

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | kapsayıcı grupları | Hayır | Hayır |
> | serviceassociationlinks | Hayır | Hayır |

## <a name="microsoftcontainerregistry"></a>Microsoft.ContainerRegistry

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | kayıt | Evet | Evet |
> | kayıt defterleri/BuildTasks | Evet | Evet |
> | kayıt defterleri/çoğaltmalar | Evet | Evet |
> | kayıt defterleri/taskçalıştırmaları | Evet | Evet |
> | kayıt defterleri/görevler | Evet | Evet |
> | kayıt defterleri/Web kancaları | Evet | Evet |

## <a name="microsoftcontainerservice"></a>Microsoft.ContainerService

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | containerservices | Hayır | Hayır |
> | managedkümeler | Hayır | Hayır |
> | openshiftmanagedclusters | Hayır | Hayır |

## <a name="microsoftcontentmoderator"></a>Microsoft.ContentModerator

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | uygulama | Hayır | Hayır |

## <a name="microsoftcortanaanalytics"></a>Microsoft. Cortanaanalizi

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | accounts | Hayır | Hayır |

## <a name="microsoftcostmanagement"></a>Microsoft. CostManagement

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | alerts | Hayır | Hayır |
> | bütçelerinin | Hayır | Hayır |
> | bağlayıcılar | Evet | Evet |
> | boyutlar | Hayır | Hayır |
> | dışarı aktarmalar | Hayır | Hayır |
> | externalabonelikleri | Hayır | Hayır |
> | öngörme | Hayır | Hayır |
> | sorgu | Hayır | Hayır |
> | Reportconfigs | Hayır | Hayır |
> | reports | Hayır | Hayır |
> | showbackrules | Hayır | Hayır |
> | görüntüleme | Hayır | Hayır |

## <a name="microsoftcustomerinsights"></a>Microsoft.CustomerInsights

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | hub'lar | Hayır | Hayır |

## <a name="microsoftcustomproviders"></a>Microsoft. CustomProviders

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | ilişkilendirmeler | Hayır | Hayır |
> | resourceproviders | Evet | Evet |

## <a name="microsoftdatabox"></a>Microsoft. DataBox

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | Çizelge | Hayır | Hayır |

## <a name="microsoftdataboxedge"></a>Microsoft. DataBoxEdge

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | databoxedgedevices | Hayır | Hayır |

## <a name="microsoftdatabricks"></a>Microsoft. Databricks

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | çalışma alanları | Hayır | Hayır |

## <a name="microsoftdatacatalog"></a>Microsoft.DataCatalog

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | larına | Evet | Evet |
> | veri katalogları | Hayır | Hayır |

## <a name="microsoftdataconnect"></a>Microsoft. DataConnect

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | connectionyöneticileri | Hayır | Hayır |

## <a name="microsoftdataexchange"></a>Microsoft. DataExchange

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | paketler | Hayır | Hayır |
> | planlar | Hayır | Hayır |

## <a name="microsoftdatafactory"></a>Microsoft.DataFactory

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | veri fabrikaları | Evet | Evet |
> | oluşturucular | Evet | Evet |

## <a name="microsoftdatalake"></a>Microsoft. DataLake

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | datalakeaccounts | Hayır | Hayır |

## <a name="microsoftdatalakeanalytics"></a>Microsoft.DataLakeAnalytics

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | accounts | Evet | Evet |

## <a name="microsoftdatalakestore"></a>Microsoft.DataLakeStore

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | accounts | Evet | Evet |

## <a name="microsoftdatamigration"></a>Microsoft. DataMigration

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | services | Hayır | Hayır |
> | Hizmetler/Projeler | Hayır | Hayır |
> | Lara | Hayır | Hayır |

## <a name="microsoftdataprotection"></a>Microsoft. DataProtection

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | backupkasaları | Hayır | Hayır |

## <a name="microsoftdatashare"></a>Microsoft. DataShare

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | accounts | Evet | Evet |

## <a name="microsoftdbformariadb"></a>Microsoft. Dbformarıdb

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | sunucular | Evet | Evet |

## <a name="microsoftdbformysql"></a>Microsoft.DBforMySQL

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | sunucular | Evet | Evet |

## <a name="microsoftdbforpostgresql"></a>Microsoft.DBforPostgreSQL

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | sunucu grupları | Hayır | Hayır |
> | sunucular | Evet | Evet |
> | serversv2 | Evet | Evet |

## <a name="microsoftdeploymentmanager"></a>Microsoft. DeploymentManager

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | artifactsources | Evet | Evet |
> | piyasaya çıkarma | Evet | Evet |
> | servicetopolojileri | Evet | Evet |
> | servicetopolojileri/hizmetler | Evet | Evet |
> | servicetopolojileri/hizmetler/serviceunits | Evet | Evet |
> | adımlar | Evet | Evet |

## <a name="microsoftdevices"></a>Microsoft. Devices

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | elaun havuzları | Hayır | Hayır |
> | elaun havuzları/ıothubkiracılar | Hayır | Hayır |
> | ıothubs | Evet | Evet |
> | provisioningservices | Evet | Evet |

## <a name="microsoftdevops"></a>Microsoft. DevOps

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | düzenler | Evet | Evet |

## <a name="microsoftdevspaces"></a>Microsoft. DevSpaces

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | denetleyiciler | Evet | Evet |

## <a name="microsoftdevtestlab"></a>Microsoft.DevTestLab

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | labcenters | Hayır | Hayır |
> | labs | Evet | Hayır |
> | Laboratuvarlar/ortamlar | Evet | Evet |
> | Labs/servicerunanlar | Evet | Evet |
> | Labs/virtualmachines | Evet | Hayır |
> | cağını | Evet | Evet |

## <a name="microsoftdocumentdb"></a>Microsoft.DocumentDB

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | veritabanı hesapları | Evet | Evet |

## <a name="microsoftdomainregistration"></a>Microsoft.DomainRegistration

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | etki alanları | Evet | Evet |

## <a name="microsoftenterpriseknowledgegraph"></a>Microsoft. EnterpriseKnowledgeGraph

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | services | Evet | Evet |

## <a name="microsofteventgrid"></a>Microsoft.EventGrid

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | etki alanları | Evet | Evet |
> | Eventabonelikleri | Hayır-bağımsız olarak taşınamaz, ancak otomatik olarak abone olunan kaynakla taşınabilir. | Hayır-bağımsız olarak taşınamaz, ancak otomatik olarak abone olunan kaynakla taşınabilir. |
> | eventabonelikleri | Hayır-bağımsız olarak taşınamaz, ancak otomatik olarak abone olunan kaynakla taşınabilir. | Hayır-bağımsız olarak taşınamaz, ancak otomatik olarak abone olunan kaynakla taşınabilir. |
> | extensionkonuları | Hayır | Hayır |
> | konuları | Evet | Evet |

## <a name="microsofteventhub"></a>Microsoft.EventHub

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | leriniz | Evet | Evet |
> | ad alanları | Evet | Evet |

## <a name="microsoftgenomics"></a>Microsoft. Genomiks

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | accounts | Hayır | Hayır |

## <a name="microsoftguestconfiguration"></a>Microsoft. GuestConfiguration

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | guestconfigurationatamaları | Hayır | Hayır |
> | yazılım | Hayır | Hayır |
> | softwareupdateprofile | Hayır | Hayır |
> | softwareupdates | Hayır | Hayır |

## <a name="microsofthanaonazure"></a>Microsoft. HanaOnAzure

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | hanaınstances | Hayır | Hayır |
> | sapizleyicileri | Evet | Evet |

## <a name="microsofthdinsight"></a>Microsoft.HDInsight

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | leriniz | Evet | Evet |

> [!IMPORTANT]
> HDInsight kümeleri, yeni bir abonelik veya kaynak grubuna taşıyabilirsiniz. Ancak, ağ kaynaklarını (örneğin, sanal ağ, NIC veya yük dengeleyici) HDInsight kümesine bağlı abonelikler arasında taşınamaz. Ayrıca, küme için bir sanal makineye ekli NIC yeni bir kaynak grubuna taşınamıyor.
>
> Bir HDInsight kümesi için yeni bir abonelik taşırken, önce diğer kaynakları (örneğin, depolama hesabı) taşıyın. Ardından, HDInsight kümesi, tek başına taşıyın.

## <a name="microsofthealthcareapis"></a>Microsoft. Healthgelişme API 'leri

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | services | Evet | Evet |

## <a name="microsofthybridcompute"></a>Microsoft. HybridCompute

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | larla | Evet | Evet |
> | makineler/uzantılar | Hayır | Hayır |

## <a name="microsofthybriddata"></a>Microsoft. HybridData

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | veri yöneticileri | Evet | Evet |

## <a name="microsoftimportexport"></a>Microsoft.ImportExport

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | Çizelge | Evet | Evet |

## <a name="microsoftinsights"></a>Microsoft. Insights

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | actiongroups | Evet | Evet |
> | activitylogalerts | Hayır | Hayır |
> | alertrules | Evet | Evet |
> | automatedexportsettings | Hayır | Hayır |
> | autoscalesettings | Evet | Evet |
> | taban çizgisi | Hayır | Hayır |
> | calculatebaseline | Hayır | Hayır |
> | bileşenleri | Evet | Evet |
> | diagnosticsettings | Hayır | Hayır |
> | diagnosticsettingscategories | Hayır | Hayır |
> | eventTypes | Hayır | Hayır |
> | extendeddiagnosticsettings | Hayır | Hayır |
> | logdefinitions | Hayır | Hayır |
> | günlükler | Hayır | Hayır |
> | metricalerts | Hayır | Hayır |
> | metrictemeller | Hayır | Hayır |
> | MetricDefinitions | Hayır | Hayır |
> | metricnamespaces | Hayır | Hayır |
> | metrics | Hayır | Hayır |
> | çalışma kitapları | Hayır | Hayır |
> | scheduledqueryrules | Evet | Evet |
> | topology | Hayır | Hayır |
> | işlem | Hayır | Hayır |
> | vminsightsonboardingstatuses | Hayır | Hayır |
> | Web testleri | Evet | Evet |
> | çalışma kitapları | Evet | Evet |
> | workbooktemplates | Evet | Evet |

> [!IMPORTANT]
> Yeni aboneliğe taşınmasının [abonelik kotalarını](azure-subscription-service-limits.md#azure-monitor-limits)aşmadığından emin olun.

## <a name="microsoftiotcentral"></a>Microsoft. ıotcentral

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | ıotapps | Evet | Evet |

## <a name="microsoftiotspaces"></a>Microsoft. ıotspaces

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | checknameavaılabılıty | Evet | Evet |
> | graph | Evet | Evet |

## <a name="microsoftkeyvault"></a>Microsoft.KeyVault

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | kasaları | Evet | Evet |

> [!IMPORTANT]
> Disk şifrelemesi için kullanılan anahtar kasaları aynı abonelikte veya abonelikler arasında bir kaynak grubuna taşınamaz.

## <a name="microsoftkubernetes"></a>Microsoft. Kubernetes

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | connectedkümeler | Hayır | Hayır |

## <a name="microsoftkusto"></a>Microsoft.Kusto

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | leriniz | Evet | Evet |

## <a name="microsoftlabservices"></a>Microsoft. LabServices

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | labaccounts | Hayır | Hayır |

## <a name="microsoftlocationbasedservices"></a>Microsoft.LocationBasedServices

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | accounts | Hayır | Hayır |

## <a name="microsoftlocationservices"></a>Microsoft. LocationServices

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | accounts | Hayır | Hayır |

## <a name="microsoftlogic"></a>Microsoft.Logic

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | hostingenvironments | Hayır | Hayır |
> | Tümleştirme hesapları | Evet | Evet |
> | ıntegrationserviceortamortamları | Evet | Hayır |
> | ıntegrationserviceortamortamları/managedap | Evet | Hayır |
> | ısotedenvironments | Hayır | Hayır |
> | iş akışları | Evet | Evet |

## <a name="microsoftmachinelearning"></a>Microsoft.MachineLearning

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | commitmentplanlar | Evet | Evet |
> | Hizmetleri | Evet | Hayır |
> | çalışma alanları | Evet | Evet |

## <a name="microsoftmachinelearningcompute"></a>Microsoft.MachineLearningCompute

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | operationalizationkümeleri | Hayır | Hayır |

## <a name="microsoftmachinelearningexperimentation"></a>Microsoft. MachineLearningExperimentation

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | accounts | Hayır | Hayır |
> | hesaplar/çalışma alanları | Hayır | Hayır |
> | hesaplar/çalışma alanları/projeler | Hayır | Hayır |
> | teamaccounts | Hayır | Hayır |
> | teamaccounts/çalışma alanları | Hayır | Hayır |
> | teamaccounts/çalışma alanları/projeler | Hayır | Hayır |

## <a name="microsoftmachinelearningmodelmanagement"></a>Microsoft.MachineLearningModelManagement

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | accounts | Hayır | Hayır |

## <a name="microsoftmachinelearningoperationalization"></a>Microsoft. MachineLearningOperationalization

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | hostinghesapları | Hayır | Hayır |

## <a name="microsoftmachinelearningservices"></a>Microsoft.MachineLearningServices

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | çalışma alanları | Hayır | Hayır |

## <a name="microsoftmanagedidentity"></a>Microsoft. Managedıdentity

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | lerinizde | Hayır | Hayır |
> | userassignedıdentities | Hayır | Hayır |

## <a name="microsoftmanagedservices"></a>Microsoft. ManagedServices

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | registrationatamaları | Hayır | Hayır |
> | registrationdefinitions | Hayır | Hayır |

## <a name="microsoftmaps"></a>Microsoft. Maps

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | accounts | Evet | Evet |

## <a name="microsoftmarketplaceapps"></a>Microsoft. MarketplaceApps

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | classicdevservices | Hayır | Hayır |

## <a name="microsoftmedia"></a>Microsoft. Media

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | mediaservices | Evet | Evet |
> | mediaservices/liveevents | Evet | Evet |
> | mediaservices/streamingendpoints | Evet | Evet |

## <a name="microsoftmicroservices4spring"></a>Microsoft. Microservices4Spring

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | appkümeler | Hayır | Hayır |

## <a name="microsoftmigrate"></a>Microsoft. Migrate

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | assessmentprojects | Evet | Evet |
> | migrateprojects | Evet | Evet |
> | projeler | Hayır | Hayır |

## <a name="microsoftnetapp"></a>Microsoft. NetApp

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | netappaccounts | Hayır | Hayır |
> | netappaccounts/backuppolicies | Hayır | Hayır |
> | netappaccounts/capacityhavuzları | Hayır | Hayır |
> | netappaccounts/capacityhavuzları/birimleri | Hayır | Hayır |
> | netappaccounts/capacityhavuzlar/Volumes/Mount hedefleri | Hayır | Hayır |
> | netappaccounts/capacityhavuzlar/birimler/anlık görüntüler | Hayır | Hayır |

## <a name="microsoftnetwork"></a>Microsoft.Network

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
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
> | expressroutegateway 'ler | Hayır | Hayır |
> | frontkapıların | Hayır | Hayır |
> | frontdoorwebapplicationfirewallpolicies | Hayır | Hayır |
> | loadbalancers | Evet-temel SKU<br>Standart SKU yok | Evet-temel SKU<br>Standart SKU yok |
> | localnetworkgateway 'ler | Evet | Evet |
> | networkexperimentprofiles | Evet | Evet |
> | networkıntpolicies ilkeleri | Evet | Evet |
> | NetworkInterfaces | Evet | Evet |
> | networkprofiles | Hayır | Hayır |
> | networksecuritygroups | Evet | Evet |
> | networkwatchers | Evet | Evet |
> | networkwatchers/connectionmonitörleri | Evet | Evet |
> | networkwatchers/flowlogs | Evet | Evet |
> | networkwatchers/uzunluler | Evet | Evet |
> | networkwatchers/pingkafesler | Evet | Evet |
> | p2svpngateways | Hayır | Hayır |
> | privatednszones | Evet | Evet |
> | privatednszones/virtualnetworklinks | Evet | Evet |
> | privateendpointredirectmaps | Hayır | Hayır |
> | privateendpoints | Hayır | Hayır |
> | privatelinkservices | Hayır | Hayır |
> | publicıpaddresses | Evet-temel SKU<br>Standart SKU yok | Evet-temel SKU<br>Standart SKU yok |
> | publicıpöneklerini | Evet | Evet |
> | routefilters | Hayır | Hayır |
> | routetables | Evet | Evet |
> | serviceendpointpolicies | Evet | Evet |
> | trafficmanagerprofiles | Evet | Evet |
> | virtualhub 'lar | Hayır | Hayır |
> | virtualnetworkgateways | Evet | Evet |
> | virtualnetworks | Evet | Evet |
> | virtualnetworktaps | Hayır | Hayır |
> | virtualyönlendiriciler | Evet | Evet |
> | virtualwan | Hayır | Hayır |
> | vpngateway (sanal WAN) | Hayır | Hayır |
> | vpnserverconfigurations | Hayır | Hayır |
> | vpnsites (sanal WAN) | Hayır | Hayır |
> | webapplicationfirewallpolicies | Evet | Evet |

> [!IMPORTANT]
> Bkz. [ağ taşıma Kılavuzu](./move-limitations/networking-move-limitations.md).

## <a name="microsoftnotificationhubs"></a>Microsoft.NotificationHubs

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | ad alanları | Evet | Evet |
> | ad alanları/notificationhub 'lar | Evet | Evet |

## <a name="microsoftobjectstore"></a>Microsoft. ObjectStore

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | osnamespaces | Evet | Evet |

## <a name="microsoftoperationalinsights"></a>Microsoft.OperationalInsights

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | storageınsii configs | Hayır | Hayır |
> | çalışma alanları | Evet | Evet |

> [!IMPORTANT]
> Yeni aboneliğe taşınmasının [abonelik kotalarını](azure-subscription-service-limits.md#azure-monitor-limits)aşmadığından emin olun.

## <a name="microsoftoperationsmanagement"></a>Microsoft.OperationsManagement

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | managementassociations | Hayır | Hayır |
> | managementconfigurations | Evet | Evet |
> | çözümler | Evet | Evet |
> | görüntüleme | Evet | Evet |

## <a name="microsoftpeering"></a>Microsoft. eşleme

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | eşlemeleri | Evet | Evet |
> | peeringservices | Hayır | Hayır |

## <a name="microsoftpolicyinsights"></a>Microsoft. Poliyelei

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | poliyevents | Hayır | Hayır |
> | policystates | Hayır | Hayır |
> | policytrackedresources | Hayır | Hayır |
> | düzeltmeler | Hayır | Hayır |

## <a name="microsoftportal"></a>Microsoft. Portal

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | panolar | Evet | Evet |

## <a name="microsoftportalsdk"></a>Microsoft. PortalSdk

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | rootresources | Hayır | Hayır |

## <a name="microsoftpowerbi"></a>Microsoft. PowerBI

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | workspacecollections | Evet | Evet |

## <a name="microsoftpowerbidedicated"></a>Microsoft.PowerBIDedicated

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | kapasiteler | Evet | Evet |

## <a name="microsoftprojectbabylon"></a>Microsoft. ProjectBabylon

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | accounts | Hayır | Hayır |

## <a name="microsoftprojectoxford"></a>Microsoft. ProjectOxford

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | accounts | Hayır | Hayır |

## <a name="microsoftproviderhub"></a>Microsoft. ProviderHub

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | piyasaya çıkarma | Hayır | Hayır |

## <a name="microsoftrecoveryservices"></a>Microsoft. RecoveryServices

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | backupkorunabilir | Hayır | Hayır |
> | replicationeligibilityresults | Hayır | Hayır |
> | kasaları | Evet | Evet |

> [!IMPORTANT]
> Bkz. [Kurtarma Hizmetleri taşıma Kılavuzu](../../backup/backup-azure-move-recovery-services-vault.md?toc=/azure/azure-resource-manager/toc.json).

## <a name="microsoftrelay"></a>Microsoft. Relay

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | ad alanları | Evet | Evet |

## <a name="microsoftresourcegraph"></a>Microsoft. ResourceGraph

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | sorgu | Evet | Evet |

## <a name="microsoftresourcehealth"></a>Microsoft. ResourceHealth

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | kullanılabilirlik durumları | Hayır | Hayır |
> | childadvailabilitydurumlar | Hayır | Hayır |
> | childresources | Hayır | Hayır |
> | etkinlikler | Hayır | Hayır |
> | bildirimler | Hayır | Hayır |

## <a name="microsoftresources"></a>Microsoft. resources

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | deploymentscripts | Hayır | Hayır |
> | bağlantılar | Hayır | Hayır |
> | etiketler | Hayır | Hayır |

## <a name="microsoftsaas"></a>Microsoft. SaaS

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | uygulama | Evet | Hayır |

## <a name="microsoftscheduler"></a>Microsoft. Scheduler

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | işlere | Evet | Evet |

## <a name="microsoftsearch"></a>Microsoft.Search

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | searchservices | Evet | Evet |

> [!IMPORTANT]
> Farklı bölgelerde çeşitli arama kaynaklarını tek bir işlemde taşıyamazsınız. Bunun yerine, bunları ayrı işlemlerde taşıyın.

## <a name="microsoftsecurity"></a>Microsoft.Security

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | adaptivenetworkhardenings | Hayır | Hayır |
> | advancedthreatprotectionsettings | Hayır | Hayır |
> | assessmentmetadata | Hayır | Hayır |
> | değerlendirme | Hayır | Hayır |
> | akışlarını otomatikleştirin | Evet | Evet |
> | karmaşık anceresults | Hayır | Hayır |
> | Uyumluluklarına | Hayır | Hayır |
> | datacollectionagents | Hayır | Hayır |
> | datacollectionresults | Hayır | Hayır |
> | devicesecuritygroups | Hayır | Hayır |
> | ınformationprotectionpolicies | Hayır | Hayır |
> | ıotsecuritysolutions | Evet | Evet |
> | sunucukullanılabilirliği | Hayır | Hayır |

## <a name="microsoftsecurityinsights"></a>Microsoft. Securityınsights

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | toplamaları | Hayır | Hayır |
> | alertrules | Hayır | Hayır |
> | alertrutatemplates | Hayır | Hayır |
> | yer işaretleri | Hayır | Hayır |
> | çalışmaların | Hayır | Hayır |
> | veri bağlayıcıları | Hayır | Hayır |
> | Varlık | Hayır | Hayır |
> | entityqueries | Hayır | Hayır |
> | officeconsents | Hayır | Hayır |
> | ayarlar | Hayır | Hayır |

## <a name="microsoftservermanagement"></a>Microsoft. ServerManagement

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | geçidinin | Hayır | Hayır |
> | düğüm | Hayır | Hayır |

## <a name="microsoftservicebus"></a>Microsoft.ServiceBus

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | ad alanları | Evet | Evet |

## <a name="microsoftservicefabric"></a>Microsoft.ServiceFabric

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | uygulama | Hayır | Hayır |
> | leriniz | Evet | Evet |
> | kümeler/uygulamalar | Hayır | Hayır |
> | kapsayıcı grupları | Hayır | Hayır |
> | containergroupsets | Hayır | Hayır |
> | edgeclusters | Hayır | Hayır |
> | ağlar | Hayır | Hayır |
> | secretmağazaları | Hayır | Hayır |
> | volumes | Hayır | Hayır |

## <a name="microsoftservicefabricmesh"></a>Microsoft. Servicefabrickafesi

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | uygulama | Evet | Evet |
> | geçidinin | Evet | Evet |
> | ağlar | Evet | Evet |
> | kaynaklanır | Evet | Evet |
> | volumes | Evet | Evet |

## <a name="microsoftservices"></a>Microsoft. Services

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | piyasaya çıkarma | Hayır | Hayır |

## <a name="microsoftsignalrservice"></a>Microsoft. SignalRService

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | SignalR | Evet | Evet |

## <a name="microsoftsoftwareplan"></a>Microsoft. SoftwarePlan

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | hybriduseavantajlar | Hayır | Hayır |

## <a name="microsoftsolutions"></a>Microsoft. Solutions

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | applicationdefinitions | Hayır | Hayır |
> | uygulama | Hayır | Hayır |
> | jistekleri | Hayır | Hayır |

## <a name="microsoftsql"></a>Microsoft.Sql

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | ınstancepools | Hayır | Hayır |
> | ManagedInstances | Hayır | Hayır |
> | ManagedInstances/veritabanları | Hayır | Hayır |
> | sunucular | Evet | Evet |
> | sunucular/veritabanları | Evet | Evet |
> | sunucular/elaun havuzları | Evet | Evet |
> | virtualkümeler | Evet | Evet |

> [!IMPORTANT]
> Bir veritabanı ve sunucu aynı kaynak grubunda olmalıdır. Bir SQL server taşıdığınızda, tüm veritabanlarını da taşınır. Bu davranış, Azure SQL veritabanı ve Azure SQL veri ambarı veritabanları için geçerlidir.

## <a name="microsoftsqlvirtualmachine"></a>Microsoft. SqlVirtualMachine

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | sqlvirtualmachinegroups | Evet | Evet |
> | sqlvirtualmachines | Evet | Evet |

## <a name="microsoftsqlvm"></a>Microsoft. SqlVM

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | dwvm | Hayır | Hayır |

## <a name="microsoftstorage"></a>Microsoft.Storage

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | storageaccounts | Evet | Evet |

## <a name="microsoftstoragesync"></a>Microsoft.StorageSync

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | storagesyncservices | Evet | Evet |

## <a name="microsoftstoragesyncdev"></a>Microsoft. StorageSyncDev

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | storagesyncservices | Hayır | Hayır |

## <a name="microsoftstoragesyncint"></a>Microsoft. Storagesyncınt

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | storagesyncservices | Hayır | Hayır |

## <a name="microsoftstorsimple"></a>Microsoft.StorSimple

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | ilerinde | Hayır | Hayır |

## <a name="microsoftstreamanalytics"></a>Microsoft.StreamAnalytics

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | streammingjobs | Evet | Evet |

> [!IMPORTANT]
> Stream Analytics işleri çalışır durumda olduğunda taşınamaz.

## <a name="microsoftstreamanalyticsexplorer"></a>Microsoft. StreamAnalyticsExplorer

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | ortamlar | Hayır | Hayır |
> | ortamlar/EventSources | Hayır | Hayır |
> | örnekler | Hayır | Hayır |
> | örnekler/ortamlar | Hayır | Hayır |
> | örnekler/ortamlar/EventSources | Hayır | Hayır |

## <a name="microsoftsubscription"></a>Microsoft. Subscription

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | CreateSubscription | Hayır | Hayır |

## <a name="microsoftsupport"></a>Microsoft. support

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | createsupportticket | Hayır | Hayır |
> | destek biletleri | Hayır | Hayır |

## <a name="microsoftterraformoss"></a>Microsoft. Teroyformoss

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | providerkayıtları | Hayır | Hayır |
> | resources | Hayır | Hayır |

## <a name="microsofttimeseriesinsights"></a>Microsoft.TimeSeriesInsights

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | ortamlar | Evet | Evet |
> | ortamlar/EventSources | Evet | Evet |
> | ortamlar/referencedataset 'ler | Evet | Evet |

## <a name="microsofttoken"></a>Microsoft. Token

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | depolar | Evet | Evet |

## <a name="microsoftvisualstudio"></a>Microsoft. VisualStudio

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | account | Hayır | Hayır |
> | Hesap/uzantı | Hayır | Hayır |
> | hesap/proje | Hayır | Hayır |

> [!IMPORTANT]
> Azure DevOps aboneliğini değiştirmek için, bkz. [faturalandırma için kullanılan Azure aboneliğini değiştirme](/azure/devops/organizations/billing/change-azure-subscription?toc=/azure/azure-resource-manager/toc.json).

## <a name="microsoftvmwarecloudsimple"></a>Microsoft. Vmwarechoparlör basit

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | ayrılmış cloudnodes | Hayır | Hayır |
> | ayrılmış CloudService | Hayır | Hayır |
> | virtualmachines | Hayır | Hayır |

## <a name="microsoftvsonline"></a>Microsoft. VSOnline

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | accounts | Evet | Evet |
> | planlar | Evet | Evet |

## <a name="microsoftweb"></a>Microsoft. Web

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | sertifikalar | Hayır | Evet |
> | connectiongateway 'ler | Evet | Evet |
> | bağlantılar | Evet | Evet |
> | customapsıs | Evet | Evet |
> | hostingenvironments | Hayır | Hayır |
> | serverfarms | Evet | Evet |
> | siteler | Evet | Evet |
> | siteler/premieraddons | Evet | Evet |
> | siteler/yuvalar | Evet | Evet |
> | staticsites | Hayır | Hayır |

> [!IMPORTANT]
> Bkz. [App Service taşıma Kılavuzu](./move-limitations/app-service-move-limitations.md).

## <a name="microsoftwindowsiot"></a>Microsoft. Windowsıot

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | deviceservices | Hayır | Hayır |

## <a name="microsoftworkloadmonitor"></a>Microsoft. WorkloadMonitor

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | bileşenleri | Hayır | Hayır |
> | izleme örnekleri | Hayır | Hayır |
> | monitörler | Hayır | Hayır |
> | notificationsettings | Hayır | Hayır |

## <a name="third-party-services"></a>Üçüncü taraf hizmetleri

Üçüncü taraf hizmetler şu anda taşıma işlemini desteklemiyor.

## <a name="next-steps"></a>Sonraki adımlar
Kaynakları taşıma komutları için bkz. [kaynakları yeni kaynak grubuna veya aboneliğe taşıma](move-resource-group-and-subscription.md).

Aynı verileri bir virgülle ayrılmış değerler dosyası ile almak için, [Move-support-resources. csv](https://github.com/tfitzmac/resource-capabilities/blob/master/move-support-resources.csv)dosyasını indirin.
