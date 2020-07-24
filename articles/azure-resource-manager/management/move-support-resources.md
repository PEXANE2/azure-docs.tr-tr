---
title: Kaynak türüne göre işlem desteğini taşıma
description: Yeni bir kaynak grubuna veya aboneliğe taşınabilecek Azure Kaynak türlerini listeler.
ms.topic: conceptual
ms.date: 07/13/2020
ms.openlocfilehash: 16197210326d73284a4a83edc7876e4faddded86
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87079502"
---
# <a name="move-operation-support-for-resources"></a>Kaynaklar için taşıma işlemi desteği

Bu makalede, bir Azure Kaynak türünün taşıma işlemini destekleyip desteklemediğini listelenmiştir. Ayrıca bir kaynağı taşırken göz önünde bulundurmanız gereken özel koşullar hakkında bilgi sağlar.

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
> - [Microsoft. ChangeAnalysis](#microsoftchangeanalysis)
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
> - [Microsoft. CostManagementExports](#microsoftcostmanagementexports)
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
> - [Microsoft. PowerPlatform](#microsoftpowerplatform)
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
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | DomainServices | Hayır | Hayır |
> | DomainServices/oucontainer | Hayır | Hayır |
> | yerlerini | Hayır | Hayır |
> | konumlar/operationresults | Hayır | Hayır |
> | işlemler | Hayır | Hayır |

## <a name="microsoftaadiam"></a>Microsoft. aadihar

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | diagnosticsettings | Hayır | Hayır |
> | diagnosticsettingscategories | Hayır | Hayır |
> | işlemler | Hayır | Hayır |
> | privatelinkforazuread | Yes | Yes |
> | Kira | Yes | Yes |

## <a name="microsoftaddons"></a>Microsoft. addons

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | operationresults | Hayır | Hayır |
> | işlemler | Hayır | Hayır |
> | destek sağlayıcıları | Hayır | Hayır |

## <a name="microsoftadhybridhealthservice"></a>Microsoft. ADHybridHealthService

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | aadsupportcases | Hayır | Hayır |
> | addsservices | Hayır | Hayır |
> | aracısını | Hayır | Hayır |
> | anonymousapiusers | Hayır | Hayır |
> | yapılandırma | Hayır | Hayır |
> | günlükler | Hayır | Hayır |
> | işlemler | Hayır | Hayır |
> | reports | Hayır | Hayır |
> | servicehealthölçümleri | Hayır | Hayır |
> | services | Hayır | Hayır |

## <a name="microsoftadvisor"></a>Microsoft. Advisor

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | konfigürasyonları | Hayır | Hayır |
> | generatereyorumgeçişleri | Hayır | Hayır |
> | meta veriler | Hayır | Hayır |
> | işlemler | Hayır | Hayır |
> | Öneriler | Hayır | Hayır |
> | gizlemeleri | Hayır | Hayır |

## <a name="microsoftalertsmanagement"></a>Microsoft. AlertsManagement

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | actionrules | Yes | Yes |
> | alerts | Hayır | Hayır |
> | alertslist | Hayır | Hayır |
> | alertsmetadata | Hayır | Hayır |
> | alertssummary | Hayır | Hayır |
> | alertssummarylist | Hayır | Hayır |
> | işlemler | Hayır | Hayır |
> | smartdetectoralertrules | Yes | Yes |
> | smartgroups | Hayır | Hayır |

## <a name="microsoftanalysisservices"></a>Microsoft. AnalysisServices

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | yerlerini | Hayır | Hayır |
> | konumlar/checknameavaılabılıty | Hayır | Hayır |
> | konumlar/operationresults | Hayır | Hayır |
> | konumlar/operationdurumlarının | Hayır | Hayır |
> | işlemler | Hayır | Hayır |
> | larý | Yes | Yes |

## <a name="microsoftapimanagement"></a>Microsoft. Apimanane

> [!IMPORTANT]
> Tüketim SKU 'suna ayarlanmış bir API Management hizmeti taşınamaz.

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | checkfeedbackrequired | Hayır | Hayır |
> | checknameavaılabılıty | Hayır | Hayır |
> | checkservicenameavaılabılıty | Hayır | Hayır |
> | işlemler | Hayır | Hayır |
> | reportfeedback | Hayır | Hayır |
> | hizmet | Yes | Yes |
> | validateservicename | Hayır | Hayır |

## <a name="microsoftappconfiguration"></a>Microsoft. AppConfiguration

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | checknameavaılabılıty | Hayır | Hayır |
> | configurationmağazaların | Yes | Yes |
> | configurationmağazaların/eventgridfilters | Hayır | Hayır |
> | yerlerini | Hayır | Hayır |
> | konumlar/operationsstatus | Hayır | Hayır |
> | işlemler | Hayır | Hayır |

## <a name="microsoftappplatform"></a>Microsoft. AppPlatform

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | yerlerini | Hayır | Hayır |
> | konumlar/checknameavaılabılıty | Hayır | Hayır |
> | konumlar/operationresults | Hayır | Hayır |
> | konumlar/OperationStatus | Hayır | Hayır |
> | işlemler | Hayır | Hayır |
> | yay | Yes | Yes |
> | yay/uygulamalar | Hayır | Hayır |
> | yay/uygulamalar/dağıtımlar | Hayır | Hayır |

## <a name="microsoftappservice"></a>Microsoft. AppService

> [!IMPORTANT]
> Bkz. [App Service taşıma Kılavuzu](./move-limitations/app-service-move-limitations.md).

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | apiapps | Hayır | Hayır |
> | appdentities | Hayır | Hayır |
> | geçidinin | Hayır | Hayır |

## <a name="microsoftattestation"></a>Microsoft. kanıtlama

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | attestationproviders | Yes | Yes |
> | işlemler | Hayır | Hayır |

## <a name="microsoftauthorization"></a>Microsoft.Authorization

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | CheckAccess | Hayır | Hayır |
> | classicadministrators | Hayır | Hayır |
> | datatakma adlar | Hayır | Hayır |
> | denyasatamaları | Hayır | Hayır |
> | erişimi yükseltme | Hayır | Hayır |
> | findorphanroleatamalar | Hayır | Hayır |
> | kaynaktaki | Hayır | Hayır |
> | işlemler | Hayır | Hayır |
> | OperationStatus | Hayır | Hayır |
> | izinler | Hayır | Hayır |
> | poliyasatamaları | Hayır | Hayır |
> | PolicyDefinitions | Hayır | Hayır |
> | policysetdefinitions | Hayır | Hayır |
> | privatelinkassociations | Hayır | Hayır |
> | provideroperations | Hayır | Hayır |
> | resourcemanagementprivatelinks | Hayır | Hayır |
> | roleassignments | Hayır | Hayır |
> | roleatamasussusageölçümleri | Hayır | Hayır |
> | roledefinitions | Hayır | Hayır |

## <a name="microsoftautomation"></a>Microsoft. Automation

> [!IMPORTANT]
> Runbook 'lar Otomasyon hesabıyla aynı kaynak grubunda bulunmalıdır.
>
> Daha fazla bilgi için bkz. [Azure Otomasyonu hesabınızı başka bir aboneliğe taşıma](../../automation/how-to/move-account.md?toc=/azure/azure-resource-manager/toc.json).

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | automationaccounts | Yes | Yes |
> | automationaccounts/Configurations | Yes | Yes |
> | automationaccounts/Jobs | Hayır | Hayır |
> | automationaccounts/privateendpointconnectionproxy 'leri | Hayır | Hayır |
> | automationaccounts/privateendpointconnections | Hayır | Hayır |
> | automationaccounts/privatelinkresources | Hayır | Hayır |
> | automationaccounts/runbook 'lar | Yes | Yes |
> | automationaccounts/softwareupdateconfigurations | Hayır | Hayır |
> | automationaccounts/Web kancaları | Hayır | Hayır |
> | işlemler | Hayır | Hayır |

## <a name="microsoftavs"></a>Microsoft. AVS

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | yerlerini | Hayır | Hayır |
> | konumlar/checkquotaavaılabılıty | Hayır | Hayır |
> | konumlar/checktrialavailability | Hayır | Hayır |
> | işlemler | Hayır | Hayır |
> | privatebulutlar | Yes | Yes |
> | privatebulutlar/kümeler | Hayır | Hayır |

## <a name="microsoftazureactivedirectory"></a>Microsoft. AzureActiveDirectory

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | b2cdirectories | Yes | Yes |
> | b2ctenants | Hayır | Hayır |
> | checknameavaılabılıty | Hayır | Hayır |
> | işlemler | Hayır | Hayır |

## <a name="microsoftazuredata"></a>Microsoft. AzureData

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | veri denetleyicileri | Hayır | Hayır |
> | hybriddatayöneticileri | Hayır | Hayır |
> | işlemler | Hayır | Hayır |
> | postgresınstances | Hayır | Hayır |
> | SQLInstances | Hayır | Hayır |
> | sqlmanagedınstances | Hayır | Hayır |
> | sqlserverınstances | Hayır | Hayır |
> | sqlserverkayıtları | Yes | Yes |
> | sqlserverkayıtları/SQLServers | Hayır | Hayır |

## <a name="microsoftazurestack"></a>Microsoft. AzureStack

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | cloudmanifestfiles | Hayır | Hayır |
> | işlemler | Hayır | Hayır |
> | kayıtlarında | Yes | Yes |
> | kayıt/müşteri abonelikleri | Hayır | Hayır |
> | kayıtlar/ürünler | Hayır | Hayır |

## <a name="microsoftazurestackhci"></a>Microsoft. Azurestackhcı

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | leriniz | Hayır | Hayır |
> | işlemler | Hayır | Hayır |

## <a name="microsoftbatch"></a>Microsoft.Batch

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | batchaccounts | Yes | Yes |
> | yerlerini | Hayır | Hayır |
> | konumlar/accountoperationresults | Hayır | Hayır |
> | konumlar/checknameavaılabılıty | Hayır | Hayır |
> | konumlar/kotalar | Hayır | Hayır |
> | işlemler | Hayır | Hayır |

## <a name="microsoftbilling"></a>Microsoft. Faturalandırma

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | billingaccounts | Hayır | Hayır |
> | billingaccounts/anlaşmalar | Hayır | Hayır |
> | billingaccounts/billingpermissions | Hayır | Hayır |
> | billingaccounts/billingprofiles | Hayır | Hayır |
> | billingaccounts/billingprofiles/availablebakiye | Hayır | Hayır |
> | billingaccounts/billingprofiller/billingpermissions | Hayır | Hayır |
> | billingaccounts/billingprofiles/billingroleatamaları | Hayır | Hayır |
> | billingaccounts/billingprofiles/billingroledefinitions | Hayır | Hayır |
> | billingaccounts/billingprofiles/billingabonelikleri | Hayır | Hayır |
> | billingaccounts/billingprofiles/createbillingroleatama | Hayır | Hayır |
> | billingaccounts/billingprofiles/müşteriler | Hayır | Hayır |
> | billingaccounts/billingprofiles/yönergeler | Hayır | Hayır |
> | billingaccounts/billingprofiles/faturalar | Hayır | Hayır |
> | billingaccounts/billingprofiles/faturalar/fiyat listesi | Hayır | Hayır |
> | billingaccounts/billingprofiles/faturalar/işlemler | Hayır | Hayır |
> | billingaccounts/billingprofiles/ınvoicesections | Hayır | Hayır |
> | billingaccounts/billingprofiles/ınvoicesections/billingpermissions | Hayır | Hayır |
> | billingaccounts/billingprofiles/ınvoicesections/billingroleatamaları | Hayır | Hayır |
> | billingaccounts/billingprofiles/ınvoicesections/billingroledefinitions | Hayır | Hayır |
> | billingaccounts/billingprofiles/ınvoicesections/billingabonelikleri | Hayır | Hayır |
> | billingaccounts/billingprofiles/ınvoicesections/createbillingroleatama | Hayır | Hayır |
> | billingaccounts/billingprofiles/ınvoicesections/ınitiatetransfer | Hayır | Hayır |
> | billingaccounts/billingprofiles/ınvoicesections/ürünler | Hayır | Hayır |
> | billingaccounts/billingprofiles/ınvoicesections/ürünler/aktarım | Hayır | Hayır |
> | billingaccounts/billingprofiles/ınvoicesections/ürünler/updateautorenew | Hayır | Hayır |
> | billingaccounts/billingprofiles/ınvoicesections/işlemler | Hayır | Hayır |
> | billingaccounts/billingprofiles/ınvoicesections/aktarımlar | Hayır | Hayır |
> | billingaccounts/billingprofiles/patchoperations | Hayır | Hayır |
> | billingaccounts/billingprofiles/paymentmethods | Hayır | Hayır |
> | billingaccounts/billingprofiles/ilkeler | Hayır | Hayır |
> | billingaccounts/billingprofiles/fiyat listesi | Hayır | Hayır |
> | billingaccounts/billingprofiles/pricesheetdownloadoperations | Hayır | Hayır |
> | billingaccounts/billingprofiles/ürünler | Hayır | Hayır |
> | billingaccounts/billingprofiles/işlemler | Hayır | Hayır |
> | billingaccounts/billingroleatamaları | Hayır | Hayır |
> | billingaccounts/billingroledefinitions | Hayır | Hayır |
> | billingaccounts/billingabonelikleri | Hayır | Hayır |
> | billingaccounts/billingabonelikleri/faturalar | Hayır | Hayır |
> | billingaccounts/createbillingroleatama | Hayır | Hayır |
> | billingaccounts/createınvoicesectionoperations | Hayır | Hayır |
> | billingaccounts/müşteriler | Hayır | Hayır |
> | billingaccounts/müşteriler/billingpermissions | Hayır | Hayır |
> | billingaccounts/müşteriler/billingabonelikleri | Hayır | Hayır |
> | billingaccounts/müşteriler/ınitiatetransfer | Hayır | Hayır |
> | billingaccounts/müşteriler/ilkeler | Hayır | Hayır |
> | billingaccounts/müşteriler/ürünler | Hayır | Hayır |
> | billingaccounts/müşteriler/işlemler | Hayır | Hayır |
> | billingaccounts/müşteriler/aktarımlar | Hayır | Hayır |
> | billingaccounts/departmanlar | Hayır | Hayır |
> | billingaccounts/KayıtSayısı | Hayır | Hayır |
> | billingaccounts/faturalar | Hayır | Hayır |
> | billingaccounts/ınvoicesections | Hayır | Hayır |
> | billingaccounts/ınvoicesections/billingsubscriptionmoveoperations | Hayır | Hayır |
> | billingaccounts/ınvoicesections/billingabonelikleri | Hayır | Hayır |
> | billingaccounts/ınvoicesections/billingabonelikleri/aktarımı | Hayır | Hayır |
> | billingaccounts/ınvoicesections/yükselt | Hayır | Hayır |
> | billingaccounts/ınvoicesections/ınitiatetransfer | Hayır | Hayır |
> | billingaccounts/ınvoicesections/patchoperations | Hayır | Hayır |
> | billingaccounts/ınvoicesections/productmoveoperations | Hayır | Hayır |
> | billingaccounts/Ürünler/Ürünler | Hayır | Hayır |
> | billingaccounts/ınvoicesections/ürünler/transfer | Hayır | Hayır |
> | billingaccounts/ınvoicesections/ürünler/updateautorenew | Hayır | Hayır |
> | billingaccounts/ınvoicesections/producttransfersresults | Hayır | Hayır |
> | billingaccounts/ınvoicesections/işlemler | Hayır | Hayır |
> | billingaccounts/ınvoicesections/aktarımlar | Hayır | Hayır |
> | billingaccounts/lineofkredisi | Hayır | Hayır |
> | billingaccounts/listınvoicesectionswithcreatesubscriptionpermission | Hayır | Hayır |
> | billingaccounts/operationresults | Hayır | Hayır |
> | billingaccounts/patchoperations | Hayır | Hayır |
> | billingaccounts/paymentmethods | Hayır | Hayır |
> | billingaccounts/ürünler | Hayır | Hayır |
> | billingaccounts/işlemler | Hayır | Hayır |
> | billingdönemler | Hayır | Hayır |
> | billingpermissions | Hayır | Hayır |
> | billingproperty | Hayır | Hayır |
> | billingroleatamaları | Hayır | Hayır |
> | billingroledefinitions | Hayır | Hayır |
> | createbillingroleatama | Hayır | Hayır |
> | bölümlerinin | Hayır | Hayır |
> | kayıt sayısı | Hayır | Hayır |
> | faturalardan | Hayır | Hayır |
> | operationresults | Hayır | Hayır |
> | işlemler | Hayır | Hayır |
> | OperationStatus | Hayır | Hayır |
> | girişinde | Hayır | Hayır |
> | aktarımlar/accepttransfer | Hayır | Hayır |
> | aktarımlar/declinetransfer | Hayır | Hayır |
> | aktarımlar/OperationStatus | Hayır | Hayır |
> | aktarımlar/validatetransfer | Hayır | Hayır |
> | ValidateAddress | Hayır | Hayır |

## <a name="microsoftbingmaps"></a>Microsoft. BingMaps

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | listcommunicationpreference | Hayır | Hayır |
> | mapapsıs | Hayır | Hayır |
> | işlemler | Hayır | Hayır |
> | updatecommunicationpreference | Hayır | Hayır |

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
> | cordadmembers | Hayır | Hayır |
> | yerlerini | Hayır | Hayır |
> | konumlar/blockchainmemberoperationresults | Hayır | Hayır |
> | konumlar/checknameavaılabılıty | Hayır | Hayır |
> | konumlar/listconsortiums | Hayır | Hayır |
> | konumlar/watcheroperationresults | Hayır | Hayır |
> | işlemler | Hayır | Hayır |
> | izleyicileri | Hayır | Hayır |

## <a name="microsoftblockchaintokens"></a>Microsoft. BlockchainTokens

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | işlemler | Hayır | Hayır |
> | tokenservices | Hayır | Hayır |

## <a name="microsoftblueprint"></a>Microsoft. Blueprint

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | şema tasmi | Hayır | Hayır |
> | blueprintasbir/atamaperations | Hayır | Hayır |
> | blueprintasbir/işlemleri | Hayır | Hayır |
> | Blueprint | Hayır | Hayır |
> | planlar/yapıtlar | Hayır | Hayır |
> | planlar/sürümler | Hayır | Hayır |
> | planlar/sürümler/yapılar | Hayır | Hayır |
> | işlemler | Hayır | Hayır |

## <a name="microsoftbotservice"></a>Microsoft. BotService

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | botservices | Yes | Yes |
> | botservices/kanallar | Hayır | Hayır |
> | botservices/Connections | Hayır | Hayır |
> | checknameavaılabılıty | Hayır | Hayır |
> | listauthserviceproviders | Hayır | Hayır |
> | işlemler | Hayır | Hayır |

## <a name="microsoftcache"></a>Microsoft. Cache

> [!IMPORTANT]
> Redsıs örneği için Azure önbelleği bir sanal ağla yapılandırıldıysa, örnek farklı bir aboneliğe taşınamaz. Bkz. [ağ taşıma sınırlamaları](./move-limitations/networking-move-limitations.md).

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | checknameavaılabılıty | Hayır | Hayır |
> | yerlerini | Hayır | Hayır |
> | konumlar/operationresults | Hayır | Hayır |
> | konumlar/operationsstatus | Hayır | Hayır |
> | işlemler | Hayır | Hayır |
> | Redis | Yes | Yes |
> | redsıs/eventgridfilters | Hayır | Hayır |
> | redsıs/privatelinkresources | Hayır | Hayır |
> | redisenterprise | Hayır | Hayır |

## <a name="microsoftcapacity"></a>Microsoft. Capacity

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | appliedreservations | Hayır | Hayır |
> | calculateexchange | Hayır | Hayır |
> | calculateprice | Hayır | Hayır |
> | calculatepurchaseprice | Hayır | Hayır |
> | larına | Hayır | Hayır |
> | checktekliflere | Hayır | Hayır |
> | checkpurchasestatus | Hayır | Hayır |
> | checkscopes | Hayır | Hayır |
> | ticari vaalrezervler | Hayır | Hayır |
> | değişimi | Hayır | Hayır |
> | listavantajlar | Hayır | Hayır |
> | işlemler | Hayır | Hayır |
> | placepurchaseorder | Hayır | Hayır |
> | rezervler | Hayır | Hayır |
> | rezervler/availablescopes | Hayır | Hayır |
> | rezervler/hesaplaizterefund | Hayır | Hayır |
> | rezervler/Birleştir | Hayır | Hayır |
> | rezervler/rezervasyonlar | Hayır | Hayır |
> | rezervler/rezervasyonlar/availablescopes | Hayır | Hayır |
> | rezervler/rezervasyonlar/düzeltmeler | Hayır | Hayır |
> | rezervler/geri dönüş | Hayır | Hayır |
> | rezervler/Böl | Hayır | Hayır |
> | rezervler/takas | Hayır | Hayır |
> | oluşturamaz | Hayır | Hayır |
> | kaynaklar | Hayır | Hayır |
> | validatereservationorder | Hayır | Hayır |

## <a name="microsoftcdn"></a>Microsoft. CDN

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | cdnwebapplicationfirewallmanagedrulesets | Hayır | Hayır |
> | cdnwebapplicationfirewallpolicies | Yes | Yes |
> | checknameavaılabılıty | Hayır | Hayır |
> | checkresourceusage | Hayır | Hayır |
> | edgenodes | Hayır | Hayır |
> | operationresults | Hayır | Hayır |
> | operationresults/profileresults | Hayır | Hayır |
> | operationresults/profileresults/endpointresults | Hayır | Hayır |
> | operationresults/profileresults/endpointresults/customdomainresults | Hayır | Hayır |
> | operationresults/profileresults/endpointresults/origingroupresults | Hayır | Hayır |
> | operationresults/profileresults/endpointresults/originresults | Hayır | Hayır |
> | işlemler | Hayır | Hayır |
> | lerinize | Yes | Yes |
> | Profiller/uç noktalar | Yes | Yes |
> | Profiller/uç noktalar/customdomains | Hayır | Hayır |
> | Profiller/uç noktalar/origingroups | Hayır | Hayır |
> | Profiller/uç noktalar/kaynaklar | Hayır | Hayır |
> | validatearaştırması | Hayır | Hayır |

## <a name="microsoftcertificateregistration"></a>Microsoft. CertificateRegistration

> [!IMPORTANT]
> Bkz. [App Service taşıma Kılavuzu](./move-limitations/app-service-move-limitations.md).

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | sertifikadüzenleri | Yes | Yes |
> | certificateorders/Certificates | Hayır | Hayır |
> | işlemler | Hayır | Hayır |
> | validatecertificateregistrationınformation | Hayır | Hayır |

## <a name="microsoftchangeanalysis"></a>Microsoft. ChangeAnalysis

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | işlemler | Hayır | Hayır |

## <a name="microsoftclassiccompute"></a>Microsoft.ClassicCompute

> [!IMPORTANT]
> Bkz. [klasik dağıtım taşıma Kılavuzu](./move-limitations/classic-model-move-limitations.md). Klasik dağıtım kaynakları, bu senaryoya özel bir işlemle abonelikler arasında taşınabilir.

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | özellikler | Hayır | Hayır |
> | checkdomainnameavaılabılıty | Hayır | Hayır |
> | DomainNames | Evet | Hayır |
> | DomainNames/yetenekleri | Hayır | Hayır |
> | DomainNames/internalloadbalancers | Hayır | Hayır |
> | DomainNames/servicecertificates | Hayır | Hayır |
> | DomainNames/Yuvaları | Hayır | Hayır |
> | DomainNames/yuvalar/roller | Hayır | Hayır |
> | DomainNames/yuvalar/roller/MetricDefinitions | Hayır | Hayır |
> | DomainNames/yuvalar/roller/ölçümler | Hayır | Hayır |
> | movesubscriptionresources | Hayır | Hayır |
> | operatingsystemfamilies | Hayır | Hayır |
> | operatingsystems | Hayır | Hayır |
> | işlemler | Hayır | Hayır |
> | operationdurumlar | Hayır | Hayır |
> | quotas | Hayır | Hayır |
> | resourcetypes | Hayır | Hayır |
> | validatesubscriptionmoveavaılabılıty | Hayır | Hayır |
> | virtualmachines | Yes | Hayır |
> | virtualmachines/diagnosticsettings | Hayır | Hayır |
> | virtualmachines/MetricDefinitions | Hayır | Hayır |
> | virtualmachines/ölçümler | Hayır | Hayır |

## <a name="microsoftclassicinfrastructuremigrate"></a>Microsoft. ClassicInfrastructureMigrate

> [!IMPORTANT]
> Bkz. [klasik dağıtım taşıma Kılavuzu](./move-limitations/classic-model-move-limitations.md). Klasik dağıtım kaynakları, bu senaryoya özel bir işlemle abonelikler arasında taşınabilir.

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | classicinfrastructureresources | Hayır | Hayır |

## <a name="microsoftclassicnetwork"></a>Microsoft. ClassicNetwork

> [!IMPORTANT]
> Bkz. [klasik dağıtım taşıma Kılavuzu](./move-limitations/classic-model-move-limitations.md). Klasik dağıtım kaynakları, bu senaryoya özel bir işlemle abonelikler arasında taşınabilir.

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | özellikler | Hayır | Hayır |
> | expressroutecrossconnections | Hayır | Hayır |
> | expressroutecrossconnections/peerler | Hayır | Hayır |
> | gatewaysupporteddevices | Hayır | Hayır |
> | networksecuritygroups | Hayır | Hayır |
> | işlemler | Hayır | Hayır |
> | quotas | Hayır | Hayır |
> | rezervler | Hayır | Hayır |
> | virtualnetworks | Hayır | Hayır |
> | virtualnetworks/remotevirtualnetworkpeeringproxy 'leri | Hayır | Hayır |
> | virtualnetworks/virtualnetworkpeerler | Hayır | Hayır |

## <a name="microsoftclassicstorage"></a>Microsoft. ClassicStorage

> [!IMPORTANT]
> Bkz. [klasik dağıtım taşıma Kılavuzu](./move-limitations/classic-model-move-limitations.md). Klasik dağıtım kaynakları, bu senaryoya özel bir işlemle abonelikler arasında taşınabilir.

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | özellikler | Hayır | Hayır |
> | checkstorageaccountavailability | Hayır | Hayır |
> | disklerinden | Hayır | Hayır |
> | images | Hayır | Hayır |
> | işlemler | Hayır | Hayır |
> | osimages | Hayır | Hayır |
> | osplatformımages | Hayır | Hayır |
> | publicımages | Hayır | Hayır |
> | quotas | Hayır | Hayır |
> | storageaccounts | Evet | Hayır |
> | storageaccounts/blobservices | Hayır | Hayır |
> | storageaccounts/fileservices | Hayır | Hayır |
> | storageaccounts/MetricDefinitions | Hayır | Hayır |
> | storageaccounts/ölçümler | Hayır | Hayır |
> | storageaccounts/queueservices | Hayır | Hayır |
> | storageaccounts/Services | Hayır | Hayır |
> | storageaccounts/Services/diagnosticsettings | Hayır | Hayır |
> | storageaccounts/Services/MetricDefinitions | Hayır | Hayır |
> | storageaccounts/Services/ölçümler | Hayır | Hayır |
> | storageaccounts/tableservices | Hayır | Hayır |
> | storageaccounts/vmımages | Hayır | Hayır |
> | vmımages | Hayır | Hayır |

## <a name="microsoftclassicsubscription"></a>Microsoft. ClassicSubscription

> [!IMPORTANT]
> Bkz. [klasik dağıtım taşıma Kılavuzu](./move-limitations/classic-model-move-limitations.md). Klasik dağıtım kaynakları, bu senaryoya özel bir işlemle abonelikler arasında taşınabilir.

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | işlemler | Hayır | Hayır |

## <a name="microsoftcognitiveservices"></a>Microsoft. Biliveservices

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | accounts | Yes | Yes |
> | checkdomainavaılabılıty | Hayır | Hayır |
> | yerlerini | Hayır | Hayır |
> | konumlar/checkskuavaılabılıty | Hayır | Hayır |
> | konumlar/deletevirtualnetworkoralt ağları | Hayır | Hayır |
> | konumlar/operationresults | Hayır | Hayır |
> | işlemler | Hayır | Hayır |

## <a name="microsoftcommerce"></a>Microsoft. Commerce

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | işlemler | Hayır | Hayır |
> | ratecard | Hayır | Hayır |
> | usagetoplamaları | Hayır | Hayır |

## <a name="microsoftcompute"></a>Microsoft.Compute

> [!IMPORTANT]
> Bkz. [sanal makine taşıma Kılavuzu](./move-limitations/virtual-machines-move-limitations.md).

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | availabilitysets | Yes | Yes |
> | diskeriþler | Hayır | Hayır |
> | diskencryptionsets | Hayır | Hayır |
> | disklerinden | Yes | Yes |
> | Galeriler | Hayır | Hayır |
> | Galeriler/görüntüler | Hayır | Hayır |
> | Galeriler/resimler/sürümler | Hayır | Hayır |
> | hostgroups | Hayır | Hayır |
> | hostgroups/konaklar | Hayır | Hayır |
> | images | Yes | Yes |
> | yerlerini | Hayır | Hayır |
> | konumlar/artifactpublishers | Hayır | Hayır |
> | konumlar/capsoperations | Hayır | Hayır |
> | konumlar/diskoperations | Hayır | Hayır |
> | konumlar/loganalytics | Hayır | Hayır |
> | konumlar/işlemler | Hayır | Hayır |
> | konumlar/yayımcılar | Hayır | Hayır |
> | konumlar/runcommands | Hayır | Hayır |
> | konumlar/kullanımlar | Hayır | Hayır |
> | konumlar/virtualmachines | Hayır | Hayır |
> | konumlar/vmsizes | Hayır | Hayır |
> | konumlar/vsmoperations | Hayır | Hayır |
> | işlemler | Hayır | Hayır |
> | proximityplacementgroups | Yes | Yes |
> | restorepointcollections | Hayır | Hayır |
> | restorepointcollections/restorepoints | Hayır | Hayır |
> | sharedvmextensions | Hayır | Hayır |
> | sharedvmımages | Hayır | Hayır |
> | sharedvmımages/sürümler | Hayır | Hayır |
> | anlık görüntüler | Yes | Yes |
> | sshpublickeys | Hayır | Hayır |
> | virtualmachines | Yes | Yes |
> | virtualmachines/uzantıları | Yes | Yes |
> | virtualmachines/MetricDefinitions | Hayır | Hayır |
> | virtualmachines/runcommands | Hayır | Hayır |
> | virtualmachinescalesets | Yes | Yes |
> | virtualmachinescalesets/uzantılar | Hayır | Hayır |
> | virtualmachinescalesets/NetworkInterfaces | Hayır | Hayır |
> | virtualmachinescalesets/publicıpaddresses | Hayır | Hayır |
> | virtualmachinescalesets/virtualmachines | Hayır | Hayır |
> | virtualmachinescalesets/virtualmachines/NetworkInterfaces | Hayır | Hayır |

## <a name="microsoftconsumption"></a>Microsoft. tüketim

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | aggregmalyt maliyeti | Hayır | Hayır |
> | dengeler | Hayır | Hayır |
> | bütçelerinin | Hayır | Hayır |
> | ücretleriyle | Hayır | Hayır |
> | costtags | Hayır | Hayır |
> | iler | Hayır | Hayır |
> | etkinlikler | Hayır | Hayır |
> | hesaplansın | Hayır | Hayır |
> | oluş | Hayır | Hayır |
> | marketlerinden | Hayır | Hayır |
> | operationresults | Hayır | Hayır |
> | işlemler | Hayır | Hayır |
> | OperationStatus | Hayır | Hayır |
> | pricesheets | Hayır | Hayır |
> | ürün | Hayır | Hayır |
> | rezervde ayrıntıları | Hayır | Hayır |
> | reservationrecommendationdetails | Hayır | Hayır |
> | rezervationönerilere | Hayır | Hayır |
> | rezervlerin Özeti | Hayır | Hayır |
> | rezervlik işlemleri | Hayır | Hayır |
> | tags | Hayır | Hayır |
> | Kira | Hayır | Hayır |
> | larındaki | Hayır | Hayır |
> | UsageDetails | Hayır | Hayır |

## <a name="microsoftcontainerinstance"></a>Microsoft. Containerınstance

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | kapsayıcı grupları | Hayır | Hayır |
> | yerlerini | Hayır | Hayır |
> | konumlar/önbellekedimages | Hayır | Hayır |
> | konumlar/yetenekler | Hayır | Hayır |
> | konumlar/deletevirtualnetworkoralt ağları | Hayır | Hayır |
> | konumlar/işlemler | Hayır | Hayır |
> | konumlar/kullanımlar | Hayır | Hayır |
> | işlemler | Hayır | Hayır |
> | serviceassociationlinks | Hayır | Hayır |

## <a name="microsoftcontainerregistry"></a>Microsoft. ContainerRegistry

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | checknameavaılabılıty | Hayır | Hayır |
> | yerlerini | Hayır | Hayır |
> | konumlar/yetkilendirme | Hayır | Hayır |
> | konumlar/deletevirtualnetworkoralt ağları | Hayır | Hayır |
> | konumlar/operationresults | Hayır | Hayır |
> | konumlar/setupauth | Hayır | Hayır |
> | işlemler | Hayır | Hayır |
> | kayıt | Yes | Yes |
> | kayıt defterleri/agentpools | Yes | Yes |
> | kayıt defterleri/agentpools/listqueuestatus | Hayır | Hayır |
> | kayıt defterleri/derlemeler | Hayır | Hayır |
> | kayıt defterleri/derlemeler/iptal | Hayır | Hayır |
> | kayıt defterleri/derlemeler/getloglink | Hayır | Hayır |
> | kayıt defterleri/BuildTasks | Yes | Yes |
> | kayıt defterleri/BuildTasks/listsourcerepositoryproperties | Hayır | Hayır |
> | kayıt defterleri/BuildTasks/Steps | Hayır | Hayır |
> | kayıt defterleri/BuildTasks/Steps/listbuildarguments | Hayır | Hayır |
> | kayıt defterleri/eventgridfilters | Hayır | Hayır |
> | kayıt defterleri/exporthatlarının | Hayır | Hayır |
> | kayıt defterleri/generatecredentials | Hayır | Hayır |
> | kayıt defterleri/getbuildsourceuploadurl 'si | Hayır | Hayır |
> | kayıt defterleri/GetCredentials | Hayır | Hayır |
> | kayıt defterleri/ımportımage | Hayır | Hayır |
> | kayıt defterleri/ımporthatlarının | Hayır | Hayır |
> | kayıt defterleri/listbuildsourceuploadurl 'si | Hayır | Hayır |
> | kayıt defterleri/listcredentials | Hayır | Hayır |
> | kayıt defterleri/ListPolicies | Hayır | Hayır |
> | kayıt defterleri/listkullanımlar | Hayır | Hayır |
> | kayıt defterleri/ardışık düzen eylemsizlik | Hayır | Hayır |
> | kayıt defterleri/privateendpointconnectionproxy 'leri | Hayır | Hayır |
> | kayıt defterleri/privateendpointconnectionproxy/doğrulama | Hayır | Hayır |
> | kayıt defterleri/privateendpointconnections | Hayır | Hayır |
> | kayıt defterleri/privatelinkresources | Hayır | Hayır |
> | kayıt defterleri/QueueBuild | Hayır | Hayır |
> | kayıt defterleri/regeneratecredential | Hayır | Hayır |
> | kayıt defterleri/regeneratecredentials | Hayır | Hayır |
> | kayıt defterleri/çoğaltmalar | Yes | Yes |
> | kayıt defterleri/çalıştırmalar | Hayır | Hayır |
> | kayıt defterleri/çalıştırmalar/iptal | Hayır | Hayır |
> | kayıt defterleri/çalıştırmalar/listlogsasurl 'si | Hayır | Hayır |
> | kayıt defterleri/schedulerun | Hayır | Hayır |
> | kayıt defterleri/kapsameşlemler | Hayır | Hayır |
> | kayıt defterleri/taskçalıştırmaları | Hayır | Hayır |
> | kayıt defterleri/taskçalıştırmaları/ListDetails | Hayır | Hayır |
> | kayıt defterleri/görevler | Yes | Yes |
> | kayıt defterleri/görevler/ListDetails | Hayır | Hayır |
> | kayıt defterleri/belirteçler | Hayır | Hayır |
> | kayıt defterleri/updatepolicies | Hayır | Hayır |
> | kayıt defterleri/Web kancaları | Yes | Yes |
> | kayıt defterleri/Web kancaları/getcallbackconfig | Hayır | Hayır |
> | kayıt defterleri/Web kancaları/ListEvents | Hayır | Hayır |
> | kayıt defterleri/Web kancaları/ping | Hayır | Hayır |

## <a name="microsoftcontainerservice"></a>Microsoft. ContainerService

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | containerservices | Hayır | Hayır |
> | yerlerini | Hayır | Hayır |
> | konumlar/openshiftclusters | Hayır | Hayır |
> | konumlar/operationresults | Hayır | Hayır |
> | konumlar/işlemler | Hayır | Hayır |
> | konumlar/düzenleyiciler | Hayır | Hayır |
> | managedkümeler | Hayır | Hayır |
> | openshiftmanagedclusters | Hayır | Hayır |
> | işlemler | Hayır | Hayır |

## <a name="microsoftcontentmoderator"></a>Microsoft. Contentmoderatör

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | uygulamalar | Hayır | Hayır |

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
> | billingaccounts | Hayır | Hayır |
> | bütçelerinin | Hayır | Hayır |
> | cloudbağlayıcıları | Hayır | Hayır |
> | bağlayıcılar | Yes | Yes |
> | bölümlerinin | Hayır | Hayır |
> | boyutlarına | Hayır | Hayır |
> | kayıt sayısı | Hayır | Hayır |
> | aktarımları | Hayır | Hayır |
> | externalbillingaccounts | Hayır | Hayır |
> | externalbillingaccounts/uyarılar | Hayır | Hayır |
> | externalbillingaccounts/Boyutlar | Hayır | Hayır |
> | externalbillingaccounts/tahmin | Hayır | Hayır |
> | externalbillingaccounts/sorgu | Hayır | Hayır |
> | externalabonelikleri | Hayır | Hayır |
> | externalabonelikleri/uyarıları | Hayır | Hayır |
> | externalabonelikler/Boyutlar | Hayır | Hayır |
> | externalabonelikler/tahmin | Hayır | Hayır |
> | externalabonelikler/sorgu | Hayır | Hayır |
> | öngörme | Hayır | Hayır |
> | işlemler | Hayır | Hayır |
> | sorgu | Hayır | Hayır |
> | register | Hayır | Hayır |
> | reportconfigs | Hayır | Hayır |
> | reports | Hayır | Hayır |
> | ayarlar | Hayır | Hayır |
> | showbackrules | Hayır | Hayır |
> | görünümler | Hayır | Hayır |

## <a name="microsoftcostmanagementexports"></a>Microsoft. CostManagementExports

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | işlemler | Hayır | Hayır |

## <a name="microsoftcustomerinsights"></a>Microsoft. Customerınsights

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | hub'lar | Hayır | Hayır |

## <a name="microsoftcustomerlockbox"></a>Microsoft. Customerkasası

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | işlemler | Hayır | Hayır |
> | istekleri | Hayır | Hayır |

## <a name="microsoftcustomproviders"></a>Microsoft. CustomProviders

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | içermektedir | Hayır | Hayır |
> | yerlerini | Hayır | Hayır |
> | konumlar/operationdurumlarının | Hayır | Hayır |
> | işlemler | Hayır | Hayır |
> | resourceproviders | Yes | Yes |

## <a name="microsoftdatabox"></a>Microsoft. DataBox

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | Çizelge | Hayır | Hayır |
> | yerlerini | Hayır | Hayır |
> | konumlar/availablesku 'lar | Hayır | Hayır |
> | konumlar/checknameavaılabılıty | Hayır | Hayır |
> | konumlar/operationresults | Hayır | Hayır |
> | konumlar/regionconfiguration | Hayır | Hayır |
> | konumlar/ValidateAddress | Hayır | Hayır |
> | konumlar/validategirişleri | Hayır | Hayır |
> | işlemler | Hayır | Hayır |

## <a name="microsoftdataboxedge"></a>Microsoft. DataBoxEdge

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | availablesku 'lar | Hayır | Hayır |
> | databoxedgedevices | Yes | Yes |
> | databoxedgedevices/checknameavaılabılıty | Hayır | Hayır |
> | işlemler | Hayır | Hayır |

## <a name="microsoftdatabricks"></a>Microsoft. Databricks

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | yerlerini | Hayır | Hayır |
> | konumlar/getnetworkpolicies | Hayır | Hayır |
> | konumlar/operationdurumlarının | Hayır | Hayır |
> | işlemler | Hayır | Hayır |
> | çalışma alanı | Hayır | Hayır |
> | çalışma alanları/dbworkspaces | Hayır | Hayır |
> | çalışma alanları/virtualnetworkpeerler | Hayır | Hayır |

## <a name="microsoftdatacatalog"></a>Microsoft. DataCatalog

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | larına | Yes | Yes |
> | checknameavaılabılıty | Hayır | Hayır |
> | veri katalogları | Hayır | Hayır |
> | yerlerini | Hayır | Hayır |
> | konumlar/işler | Hayır | Hayır |
> | konumlar/operationresults | Hayır | Hayır |
> | işlemler | Hayır | Hayır |

## <a name="microsoftdataconnect"></a>Microsoft. DataConnect

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | connectionyöneticileri | Hayır | Hayır |

## <a name="microsoftdataexchange"></a>Microsoft. DataExchange

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | paketlerle | Hayır | Hayır |
> | Planlama | Hayır | Hayır |

## <a name="microsoftdatafactory"></a>Microsoft. DataFactory

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | checkazuredatafactorynameavailability | Hayır | Hayır |
> | checkdatafactorynameavaılabılıty | Hayır | Hayır |
> | veri fabrikaları | Yes | Yes |
> | DataFactory/diagnosticsettings | Hayır | Hayır |
> | DataFactory/MetricDefinitions | Hayır | Hayır |
> | datafactoryschema | Hayır | Hayır |
> | larının | Yes | Yes |
> | Fabrika/tümleştirme çalışma zamanları | Hayır | Hayır |
> | yerlerini | Hayır | Hayır |
> | konumlar/configurefactoryrepo | Hayır | Hayır |
> | konumlar/getfeaturevalue | Hayır | Hayır |
> | işlemler | Hayır | Hayır |

## <a name="microsoftdatalake"></a>Microsoft. DataLake

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | datalakeaccounts | Hayır | Hayır |

## <a name="microsoftdatalakeanalytics"></a>Microsoft. DataLakeAnalytics

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | accounts | Yes | Yes |
> | hesaplar/datalakestoreaccounts | Hayır | Hayır |
> | hesaplar/storageaccounts | Hayır | Hayır |
> | hesaplar/storageaccounts/kapsayıcılar | Hayır | Hayır |
> | hesaplar/storageaccounts/kapsayıcılar/listsastokens | Hayır | Hayır |
> | yerlerini | Hayır | Hayır |
> | konumlar/yetenek | Hayır | Hayır |
> | konumlar/checknameavaılabılıty | Hayır | Hayır |
> | konumlar/operationresults | Hayır | Hayır |
> | konumlar/kullanımlar | Hayır | Hayır |
> | işlemler | Hayır | Hayır |

## <a name="microsoftdatalakestore"></a>Microsoft. DataLakeStore

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | accounts | Yes | Yes |
> | hesaplar/eventgridfilters | Hayır | Hayır |
> | hesaplar/FirewallRules | Hayır | Hayır |
> | yerlerini | Hayır | Hayır |
> | konumlar/yetenek | Hayır | Hayır |
> | konumlar/checknameavaılabılıty | Hayır | Hayır |
> | konumlar/deletevirtualnetworkoralt ağları | Hayır | Hayır |
> | konumlar/operationresults | Hayır | Hayır |
> | konumlar/kullanımlar | Hayır | Hayır |
> | işlemler | Hayır | Hayır |

## <a name="microsoftdatamigration"></a>Microsoft. DataMigration

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | yerlerini | Hayır | Hayır |
> | konumlar/checknameavaılabılıty | Hayır | Hayır |
> | konumlar/operationresults | Hayır | Hayır |
> | konumlar/operationdurumlarının | Hayır | Hayır |
> | işlemler | Hayır | Hayır |
> | services | Hayır | Hayır |
> | Hizmetler/Projeler | Hayır | Hayır |
> | Lara | Hayır | Hayır |

## <a name="microsoftdataprotection"></a>Microsoft. DataProtection

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | backupkasaları | Hayır | Hayır |
> | yerlerini | Hayır | Hayır |
> | işlemler | Hayır | Hayır |

## <a name="microsoftdatashare"></a>Microsoft. DataShare

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | accounts | Yes | Yes |
> | hesaplar/paylaşımlar | Hayır | Hayır |
> | hesaplar/paylaşımlar/veri kümeleri | Hayır | Hayır |
> | hesaplar/paylaşımlar/davetler | Hayır | Hayır |
> | hesaplar/paylaşımlar/providersharesubscriptions | Hayır | Hayır |
> | hesaplar/paylaşımlar/synchronizationsettings | Hayır | Hayır |
> | hesaplar/parçalar esubscriptions | Hayır | Hayır |
> | hesaplar/parçalar esubscriptions/consumersourcedataset 'ler | Hayır | Hayır |
> | hesaplar/parçalar esubscriptions/datasetmappings | Hayır | Hayır |
> | hesaplar/parçalar esubscriptions/Tetikleyiciler | Hayır | Hayır |
> | listdavetleri | Hayır | Hayır |
> | yerlerini | Hayır | Hayır |
> | konumlar/consumerdavetleri | Hayır | Hayır |
> | konumlar/operationresults | Hayır | Hayır |
> | konumlar/rejectinvitation | Hayır | Hayır |
> | işlemler | Hayır | Hayır |

## <a name="microsoftdbformariadb"></a>Microsoft. Dbformarıdb

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | checknameavaılabılıty | Hayır | Hayır |
> | yerlerini | Hayır | Hayır |
> | konumlar/azureasyncoperation | Hayır | Hayır |
> | konumlar/operationresults | Hayır | Hayır |
> | konumlar/performancekatmanları | Hayır | Hayır |
> | konumlar/privateendpointconnectionazureasyncoperation | Hayır | Hayır |
> | konumlar/privateendpointconnectionoperationresults | Hayır | Hayır |
> | konumlar/privateendpointconnectionproxyazureasyncoperation | Hayır | Hayır |
> | konumlar/privateendpointconnectionproxyoperationsonuçları | Hayır | Hayır |
> | konumlar/recommendedactionsessionsazureasyncoperation | Hayır | Hayır |
> | konumlar/recommendedactionsessionsoperationresults | Hayır | Hayır |
> | konumlar/securityalertpoliciesazureasyncoperation | Hayır | Hayır |
> | konumlar/securityalcertpoliciesoperationresults | Hayır | Hayır |
> | konumlar/sunucukeyazureasyncoperation | Hayır | Hayır |
> | konumlar/sunucukeyoperationsonuçları | Hayır | Hayır |
> | işlemler | Hayır | Hayır |
> | larý | Yes | Yes |
> | sunucular/danışmanları | Hayır | Hayır |
> | sunucular/privateendpointconnectionproxy 'leri | Hayır | Hayır |
> | sunucular/privateendpointconnections | Hayır | Hayır |
> | sunucular/privatelinkresources | Hayır | Hayır |
> | sunucular/querymetinmetinleri | Hayır | Hayır |
> | sunucular/recoverableservers | Hayır | Hayır |
> | sunucular/topquerystatistics | Hayır | Hayır |
> | sunucular/virtualnetworkrules | Hayır | Hayır |
> | sunucular/waitstatistics | Hayır | Hayır |

## <a name="microsoftdbformysql"></a>Microsoft. Dbformyısql

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | checknameavaılabılıty | Hayır | Hayır |
> | yerlerini | Hayır | Hayır |
> | konumlar/yönetimtolarzureasyncoperation | Hayır | Hayır |
> | konumlar/yönetimtoroperationsonuçları | Hayır | Hayır |
> | konumlar/azureasyncoperation | Hayır | Hayır |
> | konumlar/operationresults | Hayır | Hayır |
> | konumlar/performancekatmanları | Hayır | Hayır |
> | konumlar/privateendpointconnectionazureasyncoperation | Hayır | Hayır |
> | konumlar/privateendpointconnectionoperationresults | Hayır | Hayır |
> | konumlar/privateendpointconnectionproxyazureasyncoperation | Hayır | Hayır |
> | konumlar/privateendpointconnectionproxyoperationsonuçları | Hayır | Hayır |
> | konumlar/recommendedactionsessionsazureasyncoperation | Hayır | Hayır |
> | konumlar/recommendedactionsessionsoperationresults | Hayır | Hayır |
> | konumlar/securityalertpoliciesazureasyncoperation | Hayır | Hayır |
> | konumlar/securityalcertpoliciesoperationresults | Hayır | Hayır |
> | konumlar/sunucukeyazureasyncoperation | Hayır | Hayır |
> | konumlar/sunucukeyoperationsonuçları | Hayır | Hayır |
> | işlemler | Hayır | Hayır |
> | larý | Yes | Yes |
> | sunucular/danışmanları | Hayır | Hayır |
> | sunucular/anahtarlar | Hayır | Hayır |
> | sunucular/privateendpointconnectionproxy 'leri | Hayır | Hayır |
> | sunucular/privateendpointconnections | Hayır | Hayır |
> | sunucular/privatelinkresources | Hayır | Hayır |
> | sunucular/querymetinmetinleri | Hayır | Hayır |
> | sunucular/recoverableservers | Hayır | Hayır |
> | sunucular/topquerystatistics | Hayır | Hayır |
> | sunucular/virtualnetworkrules | Hayır | Hayır |
> | sunucular/waitstatistics | Hayır | Hayır |

## <a name="microsoftdbforpostgresql"></a>Microsoft. DBforPostgreSQL

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | checknameavaılabılıty | Hayır | Hayır |
> | yerlerini | Hayır | Hayır |
> | konumlar/yönetimtolarzureasyncoperation | Hayır | Hayır |
> | konumlar/yönetimtoroperationsonuçları | Hayır | Hayır |
> | konumlar/azureasyncoperation | Hayır | Hayır |
> | konumlar/operationresults | Hayır | Hayır |
> | konumlar/performancekatmanları | Hayır | Hayır |
> | konumlar/privateendpointconnectionazureasyncoperation | Hayır | Hayır |
> | konumlar/privateendpointconnectionoperationresults | Hayır | Hayır |
> | konumlar/privateendpointconnectionproxyazureasyncoperation | Hayır | Hayır |
> | konumlar/privateendpointconnectionproxyoperationsonuçları | Hayır | Hayır |
> | konumlar/recommendedactionsessionsazureasyncoperation | Hayır | Hayır |
> | konumlar/recommendedactionsessionsoperationresults | Hayır | Hayır |
> | konumlar/securityalertpoliciesazureasyncoperation | Hayır | Hayır |
> | konumlar/securityalcertpoliciesoperationresults | Hayır | Hayır |
> | konumlar/sunucukeyazureasyncoperation | Hayır | Hayır |
> | konumlar/sunucukeyoperationsonuçları | Hayır | Hayır |
> | işlemler | Hayır | Hayır |
> | sunucu grupları | Hayır | Hayır |
> | larý | Yes | Yes |
> | sunucular/danışmanları | Hayır | Hayır |
> | sunucular/anahtarlar | Hayır | Hayır |
> | sunucular/privateendpointconnectionproxy 'leri | Hayır | Hayır |
> | sunucular/privateendpointconnections | Hayır | Hayır |
> | sunucular/privatelinkresources | Hayır | Hayır |
> | sunucular/querymetinmetinleri | Hayır | Hayır |
> | sunucular/recoverableservers | Hayır | Hayır |
> | sunucular/topquerystatistics | Hayır | Hayır |
> | sunucular/virtualnetworkrules | Hayır | Hayır |
> | sunucular/waitstatistics | Hayır | Hayır |
> | serversv2 | Yes | Yes |
> | teksunucular | Yes | Yes |

## <a name="microsoftdeploymentmanager"></a>Microsoft. DeploymentManager

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | artifactsources | Yes | Yes |
> | operationresults | Hayır | Hayır |
> | işlemler | Hayır | Hayır |
> | piyasaya çıkarma | Yes | Yes |
> | servicetopolojileri | Yes | Yes |
> | servicetopolojileri/hizmetler | Yes | Yes |
> | servicetopolojileri/hizmetler/serviceunits | Yes | Yes |
> | adımlar | Yes | Yes |

## <a name="microsoftdesktopvirtualization"></a>Microsoft. DesktopVirtualization

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | applicationgroups | Yes | Yes |
> | applicationgroups/uygulamalar | Hayır | Hayır |
> | applicationgroups/masaüstleri | Hayır | Hayır |
> | applicationgroups/startmenuıtems | Hayır | Hayır |
> | Ana bilgisayar havuzları | Yes | Yes |
> | hostpools/oturumkonakları | Hayır | Hayır |
> | hostpools/sessionkonakları/usersessions | Hayır | Hayır |
> | hosthavuzlar/usersessions | Hayır | Hayır |
> | işlemler | Hayır | Hayır |
> | çalışma alanı | Yes | Yes |

## <a name="microsoftdevices"></a>Microsoft.Devices

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | checknameavaılabılıty | Hayır | Hayır |
> | checkprovisioningservicenameavaılabılıty | Hayır | Hayır |
> | elaun havuzları | Hayır | Hayır |
> | elaun havuzları/ıothubkiracılar | Hayır | Hayır |
> | ıothubs | Yes | Yes |
> | ıothubs/eventgridfilters | Hayır | Hayır |
> | ıothubs/SecuritySettings | Hayır | Hayır |
> | operationresults | Hayır | Hayır |
> | işlemler | Hayır | Hayır |
> | provisioningservices | Yes | Yes |
> | vardır | Hayır | Hayır |

## <a name="microsoftdevops"></a>Microsoft. DevOps

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | düzenler | Yes | Yes |

## <a name="microsoftdevspaces"></a>Microsoft. DevSpaces

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | denetleyiciler | Yes | Yes |
> | denetleyiciler/listconnectiondetails | Hayır | Hayır |
> | yerlerini | Hayır | Hayır |
> | konumlar/checkcontainerhostmapping | Hayır | Hayır |
> | konumlar/operationresults | Hayır | Hayır |
> | işlemler | Hayır | Hayır |

## <a name="microsoftdevtestlab"></a>Microsoft. DevTestLab

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | labcenters | Hayır | Hayır |
> | larda | Evet | Hayır |
> | Laboratuvarlar/ortamlar | Yes | Yes |
> | Labs/servicerunanlar | Yes | Yes |
> | Labs/virtualmachines | Evet | Hayır |
> | yerlerini | Hayır | Hayır |
> | konumlar/işlemler | Hayır | Hayır |
> | işlemler | Hayır | Hayır |
> | cağını | Yes | Yes |

## <a name="microsoftdigitaltwins"></a>Microsoft. DigitalTwins

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | digitaltwınsınstances | Hayır | Hayır |
> | digitaltwınsınstances/operationresults | Hayır | Hayır |
> | yerlerini | Hayır | Hayır |
> | işlemler | Hayır | Hayır |

## <a name="microsoftdocumentdb"></a>Microsoft.DocumentDB

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | databaseaccountnames | Hayır | Hayır |
> | veritabanı hesapları | Yes | Yes |
> | yerlerini | Hayır | Hayır |
> | konumlar/deletevirtualnetworkoralt ağları | Hayır | Hayır |
> | konumlar/operationresults | Hayır | Hayır |
> | konumlar/operationsstatus | Hayır | Hayır |
> | operationresults | Hayır | Hayır |
> | işlemler | Hayır | Hayır |

## <a name="microsoftdomainregistration"></a>Microsoft. DomainRegistration

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | checkdomainavaılabılıty | Hayır | Hayır |
> | etki alanları | Yes | Yes |
> | Domains/domainownershiptanýmlayýcýlarý | Hayır | Hayır |
> | generatessorequest | Hayır | Hayır |
> | listdomainönerilere | Hayır | Hayır |
> | işlemler | Hayır | Hayır |
> | topleveldomains | Hayır | Hayır |
> | validatedomainregistrationınformation | Hayır | Hayır |

## <a name="microsoftenterpriseknowledgegraph"></a>Microsoft. EnterpriseKnowledgeGraph

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | yerlerini | Hayır | Hayır |
> | konumlar/operationresults | Hayır | Hayır |
> | işlemler | Hayır | Hayır |
> | services | Yes | Yes |

## <a name="microsofteventgrid"></a>Microsoft. EventGrid

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | etki alanları | Yes | Yes |
> | etki alanları/konular | Hayır | Hayır |
> | eventabonelikleri | Hayır-bağımsız olarak taşınamaz, ancak otomatik olarak abone olunan kaynakla taşınabilir. | Hayır-bağımsız olarak taşınamaz, ancak otomatik olarak abone olunan kaynakla taşınabilir. |
> | extensionkonuları | Hayır | Hayır |
> | yerlerini | Hayır | Hayır |
> | konumlar/eventabonelikler | Hayır | Hayır |
> | konumlar/operationresults | Hayır | Hayır |
> | konumlar/operationsstatus | Hayır | Hayır |
> | konumlar/topictypes | Hayır | Hayır |
> | operationresults | Hayır | Hayır |
> | işlemler | Hayır | Hayır |
> | operationsstatus | Hayır | Hayır |
> | partnernamespaces | Yes | Yes |
> | partnernamespaces/eventchannels | Hayır | Hayır |
> | iş ortağı kayıtları | Hayır | Hayır |
> | iş ortağı konuları | Yes | Yes |
> | iş ortağı konuları/eventabonelikleri | Hayır | Hayır |
> | Sistem konuları | Yes | Yes |
> | Sistem konuları/eventabonelikler | Hayır | Hayır |
> | konuları | Yes | Yes |
> | topictypes | Hayır | Hayır |

## <a name="microsofteventhub"></a>Microsoft. EventHub

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | availableclusterregion | Hayır | Hayır |
> | checknameavaılabılıty | Hayır | Hayır |
> | checknamespaceavailability | Hayır | Hayır |
> | leriniz | Yes | Yes |
> | yerlerini | Hayır | Hayır |
> | konumlar/deletevirtualnetworkoralt ağları | Hayır | Hayır |
> | öznitelikleri | Yes | Yes |
> | ad alanları/authorizationrules | Hayır | Hayır |
> | ad alanları/diskalrecoveryconfigs | Hayır | Hayır |
> | ad alanları/diskalrecoveryconfigs/checknameavaılabılıty | Hayır | Hayır |
> | ad alanları/eventhubs | Hayır | Hayır |
> | ad alanları/eventhubs/authorizationrules | Hayır | Hayır |
> | ad alanları/eventhubs/consumergroups | Hayır | Hayır |
> | ad alanları/networkrulesets | Hayır | Hayır |
> | işlemler | Hayır | Hayır |
> | isteyin | Hayır | Hayır |

## <a name="microsoftexperimentation"></a>Microsoft. deneme

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | experimentworkspaces | Hayır | Hayır |
> | yerlerini | Hayır | Hayır |
> | konumlar/işlemler | Hayır | Hayır |

## <a name="microsoftfalcon"></a>Microsoft. Falcon

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | öznitelikleri | Yes | Yes |

## <a name="microsoftfeatures"></a>Microsoft. Features

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | özellik sağlayıcıları | Hayır | Hayır |
> | özellikler | Hayır | Hayır |
> | işlemler | Hayır | Hayır |
> | sağlayıcılarla | Hayır | Hayır |
> | subscriptionfeatureregistrations | Hayır | Hayır |

## <a name="microsoftgenomics"></a>Microsoft. Genomiks

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | accounts | Hayır | Hayır |

## <a name="microsoftguestconfiguration"></a>Microsoft. GuestConfiguration

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | Oto managedaccounts | Hayır | Hayır |
> | Oto managedvmconfigurationprofiles | Hayır | Hayır |
> | guestconfigurationatamaları | Hayır | Hayır |
> | işlemler | Hayır | Hayır |
> | yazılımıdır | Hayır | Hayır |
> | softwareupdateprofile | Hayır | Hayır |
> | softwareupdates | Hayır | Hayır |

## <a name="microsofthanaonazure"></a>Microsoft. HanaOnAzure

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | hanaınstances | Hayır | Hayır |
> | yerlerini | Hayır | Hayır |
> | konumlar/işlemler | Hayır | Hayır |
> | konumlar/operationsstatus | Hayır | Hayır |
> | işlemler | Hayır | Hayır |
> | sapizleyicileri | Yes | Yes |

## <a name="microsofthardwaresecuritymodules"></a>Microsoft. HardwareSecurityModules

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | ayrılmış atedhsms | Hayır | Hayır |
> | yerlerini | Hayır | Hayır |
> | işlemler | Hayır | Hayır |

## <a name="microsofthdinsight"></a>Microsoft. HDInsight

> [!IMPORTANT]
> HDInsight kümelerini yeni bir aboneliğe veya kaynak grubuna taşıyabilirsiniz. Bununla birlikte, HDInsight kümesiyle bağlantılı ağ kaynakları (sanal ağ, NIC veya yük dengeleyici gibi) için abonelikler arasında geçiş yapamazsınız. Ayrıca, küme için bir sanal makineye bağlı bir NIC 'ye yeni bir kaynak grubuna taşıyamazsınız.
>
> HDInsight kümesini yeni bir aboneliğe taşırken, önce diğer kaynakları (depolama hesabı gibi) taşıyın. Ardından, HDInsight kümesini kendi kendine taşıyın.

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | leriniz | Yes | Yes |
> | kümeler/uygulamalar | Hayır | Hayır |
> | kümeler/operationresults | Hayır | Hayır |
> | yerlerini | Hayır | Hayır |
> | konumlar/azureasyncoperations | Hayır | Hayır |
> | konumlar/billingspec | Hayır | Hayır |
> | konumlar/yetenekler | Hayır | Hayır |
> | konumlar/operationresults | Hayır | Hayır |
> | konumlar/kullanımlar | Hayır | Hayır |
> | konumlar/validatecreaterequest | Hayır | Hayır |
> | işlemler | Hayır | Hayır |

## <a name="microsofthealthcareapis"></a>Microsoft. Healthgelişme API 'leri

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | checknameavaılabılıty | Hayır | Hayır |
> | yerlerini | Hayır | Hayır |
> | konumlar/operationresults | Hayır | Hayır |
> | işlemler | Hayır | Hayır |
> | services | Yes | Yes |
> | Hizmetler/privateendpointconnections | Hayır | Hayır |
> | Hizmetler/privatelinkresources | Hayır | Hayır |

## <a name="microsofthybridcompute"></a>Microsoft. HybridCompute

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | yerlerini | Hayır | Hayır |
> | konumlar/operationresults | Hayır | Hayır |
> | konumlar/OperationStatus | Hayır | Hayır |
> | larla | Yes | Yes |
> | makineler/uzantılar | Yes | Yes |
> | işlemler | Hayır | Hayır |

## <a name="microsofthybriddata"></a>Microsoft. HybridData

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | veri yöneticileri | Yes | Yes |
> | işlemler | Hayır | Hayır |

## <a name="microsofthybridnetwork"></a>Microsoft. HybridNetwork

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | cihazlar | Hayır | Hayır |
> | yerlerini | Hayır | Hayır |
> | konumlar/operationdurumlarının | Hayır | Hayır |
> | işlemler | Hayır | Hayır |
> | vnfs | Hayır | Hayır |

## <a name="microsofthydra"></a>Microsoft. Hydra

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | bileşenleri | Hayır | Hayır |
> | yerlerini | Hayır | Hayır |
> | networkscopes | Hayır | Hayır |
> | işlemler | Hayır | Hayır |

## <a name="microsoftimportexport"></a>Microsoft. ımportexport

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | Çizelge | Yes | Yes |
> | yerlerini | Hayır | Hayır |
> | konumlar/operationresults | Hayır | Hayır |
> | işlemler | Hayır | Hayır |

## <a name="microsoftinsights"></a>Microsoft. Insights

> [!IMPORTANT]
> Yeni aboneliğe taşınmasının [abonelik kotalarını](azure-subscription-service-limits.md#azure-monitor-limits)aşmadığından emin olun.

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | actiongroups | Yes | Yes |
> | activitylogalerts | Hayır | Hayır |
> | alertrules | Yes | Yes |
> | autoscalesettings | Yes | Yes |
> | temel | Hayır | Hayır |
> | calculatebaseline | Hayır | Hayır |
> | bileşenleri | Yes | Yes |
> | bileşenler/olaylar | Hayır | Hayır |
> | bileşenler/linkedstorageaccounts | Hayır | Hayır |
> | bileşenler/meta veriler | Hayır | Hayır |
> | bileşenler/ölçümler | Hayır | Hayır |
> | bileşenler/pricingplanlar | Hayır | Hayır |
> | bileşenler/sorgu | Hayır | Hayır |
> | datacollectionrules | Hayır | Hayır |
> | diagnosticsettings | Hayır | Hayır |
> | diagnosticsettingscategories | Hayır | Hayır |
> | eventcategories | Hayır | Hayır |
> | eventTypes | Hayır | Hayır |
> | extendeddiagnosticsettings | Hayır | Hayır |
> | guestdiagnosticsettings | Hayır | Hayır |
> | listmigrationdate | Hayır | Hayır |
> | yerlerini | Hayır | Hayır |
> | konumlar/operationresults | Hayır | Hayır |
> | logdefinitions | Hayır | Hayır |
> | günlüğe kaydetme profilleri | Hayır | Hayır |
> | günlükler | Hayır | Hayır |
> | metricalerts | Hayır | Hayır |
> | metrictemeller | Hayır | Hayır |
> | metricbatch | Hayır | Hayır |
> | MetricDefinitions | Hayır | Hayır |
> | metricnamespaces | Hayır | Hayır |
> | metrics | Hayır | Hayır |
> | migratealertrules | Hayır | Hayır |
> | migratetonewpricingmodel | Hayır | Hayır |
> | çalışma kitapları | Hayır | Hayır |
> | notificationgroups | Hayır | Hayır |
> | işlemler | Hayır | Hayır |
> | privatelinkscopeoperationdurumlar | Hayır | Hayır |
> | privatelinkscopes | Hayır | Hayır |
> | privatelinkscopes/privateendpointconnectionproxy 'leri | Hayır | Hayır |
> | privatelinkscopes/privateendpointconnections | Hayır | Hayır |
> | privatelinkscopes/scopedresources | Hayır | Hayır |
> | rollbacktolegacypricingmodel | Hayır | Hayır |
> | scheduledqueryrules | Yes | Yes |
> | topology | Hayır | Hayır |
> | işlemler | Hayır | Hayır |
> | vminsightsonboardingstatuses | Hayır | Hayır |
> | Web testleri | Yes | Yes |
> | Web testleri/gettestresultfile | Hayır | Hayır |
> | çalışma kitapları | Yes | Yes |
> | workbooktemplates | Yes | Yes |

## <a name="microsoftiotcentral"></a>Microsoft. ıotcentral

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | apptemplates | Hayır | Hayır |
> | checknameavaılabılıty | Hayır | Hayır |
> | checksubdomainavaılabılıty | Hayır | Hayır |
> | ıotapps | Yes | Yes |
> | işlemler | Hayır | Hayır |

## <a name="microsoftiotspaces"></a>Microsoft. ıotspaces

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | checknameavaılabılıty | Yes | Yes |
> | çıkarılamıyor | Yes | Yes |
> | işlemler | Hayır | Hayır |

## <a name="microsoftkeyvault"></a>Microsoft. Keykasası

> [!IMPORTANT]
> Disk şifrelemesi için kullanılan anahtar kasaları aynı abonelikte veya abonelikler arasında bir kaynak grubuna taşınamaz.

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | checknameavaılabılıty | Hayır | Hayır |
> | silinkaults | Hayır | Hayır |
> | hsmpools | Hayır | Hayır |
> | yerlerini | Hayır | Hayır |
> | konumlar/silinmeler | Hayır | Hayır |
> | konumlar/deletevirtualnetworkoralt ağları | Hayır | Hayır |
> | konumlar/operationresults | Hayır | Hayır |
> | managedhsms | Hayır | Hayır |
> | işlemler | Hayır | Hayır |
> | kasaları | Yes | Yes |
> | kasa/erişim ilkeleri | Hayır | Hayır |
> | kasa/eventgridfilters | Hayır | Hayır |
> | kasa/gizlilikler | Hayır | Hayır |

## <a name="microsoftkubernetes"></a>Microsoft. Kubernetes

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | connectedkümeler | Yes | Yes |
> | yerlerini | Hayır | Hayır |
> | konumlar/operationdurumlarının | Hayır | Hayır |
> | işlemler | Hayır | Hayır |
> | registeredsubscriptions | Hayır | Hayır |

## <a name="microsoftkubernetesconfiguration"></a>Microsoft. KubernetesConfiguration

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | sourcecontrolconfigurations | Hayır | Hayır |

## <a name="microsoftkusto"></a>Microsoft.Kusto

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | leriniz | Yes | Yes |
> | kümeler/attacheddatabaseconfigurations | Hayır | Hayır |
> | kümeler/veritabanları | Hayır | Hayır |
> | kümeler/veritabanları/veri bağlantıları | Hayır | Hayır |
> | kümeler/veritabanları/eventhubconnections | Hayır | Hayır |
> | kümeler/veritabanları/princıpalasbir | Hayır | Hayır |
> | kümeler/princıpalasbir | Hayır | Hayır |
> | yerlerini | Hayır | Hayır |
> | konumlar/checknameavaılabılıty | Hayır | Hayır |
> | konumlar/operationresults | Hayır | Hayır |
> | işlemler | Hayır | Hayır |

## <a name="microsoftlabservices"></a>Microsoft. LabServices

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | labaccounts | Hayır | Hayır |
> | yerlerini | Hayır | Hayır |
> | konumlar/işlemler | Hayır | Hayır |
> | işlemler | Hayır | Hayır |
> | kullanıcılar | Hayır | Hayır |

## <a name="microsoftlocationbasedservices"></a>Microsoft. LocationBasedServices

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | accounts | Hayır | Hayır |

## <a name="microsoftlocationservices"></a>Microsoft. LocationServices

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | accounts | Hayır | Hayır |

## <a name="microsoftlogic"></a>Microsoft. Logic

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | hostingenvironments | Hayır | Hayır |
> | Tümleştirme hesapları | Yes | Yes |
> | ıntegrationserviceortamortamları | Evet | Hayır |
> | ıntegrationserviceortamortamları/managedap | Evet | Hayır |
> | ısotedenvironments | Hayır | Hayır |
> | yerlerini | Hayır | Hayır |
> | konumlar/iş akışları | Hayır | Hayır |
> | işlemler | Hayır | Hayır |
> | sürdürülen | Yes | Yes |

## <a name="microsoftmachinelearning"></a>Microsoft. Machinöğrenim

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | commitmentplanlar | Hayır | Hayır |
> | yerlerini | Hayır | Hayır |
> | konumlar/işlemler | Hayır | Hayır |
> | konumlar/operationsstatus | Hayır | Hayır |
> | işlemler | Hayır | Hayır |
> | Hizmetleri | Evet | Hayır |
> | çalışma alanı | Yes | Yes |

## <a name="microsoftmachinelearningcompute"></a>Microsoft. MachineLearningCompute

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

## <a name="microsoftmachinelearningmodelmanagement"></a>Microsoft. MachineLearningModelManagement

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | accounts | Hayır | Hayır |

## <a name="microsoftmachinelearningservices"></a>Microsoft.MachineLearningServices

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | yerlerini | Hayır | Hayır |
> | konumlar/computeoperationsstatus | Hayır | Hayır |
> | konumlar/kotalar | Hayır | Hayır |
> | konumlar/updatequotas | Hayır | Hayır |
> | konumlar/kullanımlar | Hayır | Hayır |
> | konumlar/vmsizes | Hayır | Hayır |
> | konumlar/çalışma alanı operationsstatus | Hayır | Hayır |
> | işlemler | Hayır | Hayır |
> | çalışma alanı | Hayır | Hayır |
> | çalışma alanları/hesaplar | Hayır | Hayır |
> | çalışma alanları/eventgridfilters | Hayır | Hayır |

## <a name="microsoftmaintenance"></a>Microsoft. Maintenance

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | ApplyUpdates | Hayır | Hayır |
> | configurationatamalar | Hayır | Hayır |
> | maintenanceconfigurations | Yes | Yes |
> | güncelleştirmeler | Hayır | Hayır |

## <a name="microsoftmanagedidentity"></a>Microsoft. Managedıdentity

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | lerinizde | Hayır | Hayır |
> | işlemler | Hayır | Hayır |
> | userassignedıdentities | Hayır | Hayır |

## <a name="microsoftmanagednetwork"></a>Microsoft. ManagedNetwork

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | managednetworks | Hayır | Hayır |
> | managednetworks/managednetworkgroups | Hayır | Hayır |
> | managednetworks/managednetworkpeeringpolicies | Hayır | Hayır |
> | bildirim | Hayır | Hayır |

## <a name="microsoftmanagedservices"></a>Microsoft. ManagedServices

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | Pazar placeryumuristrationdefinitions | Hayır | Hayır |
> | işlemler | Hayır | Hayır |
> | operationdurumlar | Hayır | Hayır |
> | registrationatamaları | Hayır | Hayır |
> | registrationdefinitions | Hayır | Hayır |

## <a name="microsoftmanagement"></a>Microsoft. Management

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | checknameavaılabılıty | Hayır | Hayır |
> | getentities | Hayır | Hayır |
> | managementgroups | Hayır | Hayır |
> | managementgroups/Settings | Hayır | Hayır |
> | operationresults | Hayır | Hayır |
> | operationresults/AsyncOperation | Hayır | Hayır |
> | işlemler | Hayır | Hayır |
> | kaynaklar | Hayır | Hayır |
> | starttenantbackfill | Hayır | Hayır |
> | tenantbackfillstatus | Hayır | Hayır |

## <a name="microsoftmaps"></a>Microsoft. Maps

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | accounts | Yes | Yes |
> | hesaplar/eventgridfilters | Hayır | Hayır |
> | hesaplar/privateatlases | Yes | Yes |
> | işlemler | Hayır | Hayır |

## <a name="microsoftmarketplace"></a>Microsoft. Market

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | listavailabletekliflere | Hayır | Hayır |
> | sunar | Hayır | Hayır |
> | offertypes | Hayır | Hayır |
> | offertypes/yayımcılar | Hayır | Hayır |
> | offertypes/yayımcılar/teklifler | Hayır | Hayır |
> | offertypes/yayımcılar/teklifler/planlar | Hayır | Hayır |
> | offertypes/yayımcılar/teklifler/planlar/anlaşmalar | Hayır | Hayır |
> | offertypes/yayımcılar/teklifler/planlar/yapılandırmalarını | Hayır | Hayır |
> | offertypes/yayımcılar/teklifler/planlar/yapılandırmalarını/ımportımage | Hayır | Hayır |
> | işlemler | Hayır | Hayır |
> | privategallergıtems | Hayır | Hayır |
> | privatestoreclient | Hayır | Hayır |
> | privatemağazaların | Hayır | Hayır |
> | privatemağazaların/tekliflerin | Hayır | Hayır |
> | ürün | Hayır | Hayır |
> | Publishers | Hayır | Hayır |
> | Yayımcılar/teklifler | Hayır | Hayır |
> | Yayımcılar/teklifler/Düzeltme | Hayır | Hayır |
> | register | Hayır | Hayır |

## <a name="microsoftmarketplaceapps"></a>Microsoft. MarketplaceApps

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | classicdevservices | Hayır | Hayır |
> | listcommunicationpreference | Hayır | Hayır |
> | işlemler | Hayır | Hayır |
> | updatecommunicationpreference | Hayır | Hayır |

## <a name="microsoftmarketplaceordering"></a>Microsoft. Marketplacesıralaması

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | anlaşmalar | Hayır | Hayır |
> | offertypes | Hayır | Hayır |
> | işlemler | Hayır | Hayır |

## <a name="microsoftmedia"></a>Microsoft. Media

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | checknameavaılabılıty | Hayır | Hayır |
> | yerlerini | Hayır | Hayır |
> | konumlar/checknameavaılabılıty | Hayır | Hayır |
> | mediaservices | Yes | Yes |
> | mediaservices/accountfilters | Hayır | Hayır |
> | mediaservices/varlıklar | Hayır | Hayır |
> | mediaservices/varlıklar/assetfilters | Hayır | Hayır |
> | mediaservices/contentkeypolicies | Hayır | Hayır |
> | mediaservices/eventgridfilters | Hayır | Hayır |
> | mediaservices/liveeventoperations | Hayır | Hayır |
> | mediaservices/liveevents | Yes | Yes |
> | mediaservices/liveevents/liveçıktılar | Hayır | Hayır |
> | mediaservices/liveoutputoperations | Hayır | Hayır |
> | mediaservices/streamingendpointoperations | Hayır | Hayır |
> | mediaservices/streamingendpoints | Yes | Yes |
> | mediaservices/streamingkonumlandırıcı | Hayır | Hayır |
> | mediaservices/streamingpolicies | Hayır | Hayır |
> | mediaservices/dönüşümler | Hayır | Hayır |
> | mediaservices/dönüşümler/işler | Hayır | Hayır |
> | işlemler | Hayır | Hayır |

## <a name="microsoftmicroservices4spring"></a>Microsoft. Microservices4Spring

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | appkümeler | Hayır | Hayır |

## <a name="microsoftmigrate"></a>Microsoft. Migrate

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | assessmentprojects | Hayır | Hayır |
> | yerlerini | Hayır | Hayır |
> | konumlar/assessmentoptions | Hayır | Hayır |
> | konumlar/checknameavaılabılıty | Hayır | Hayır |
> | migrateprojects | Hayır | Hayır |
> | movecollections | Hayır | Hayır |
> | işlemler | Hayır | Hayır |
> | projeyle | Hayır | Hayır |

## <a name="microsoftmixedreality"></a>Microsoft. MixedReality

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | holographicsbroadcastaccounts | Hayır | Hayır |
> | yerlerini | Hayır | Hayır |
> | konumlar/checknameavaılabılıty | Hayır | Hayır |
> | objectunderstandingaccounts | Hayır | Hayır |
> | işlemler | Hayır | Hayır |
> | remoterenderingaccounts | Yes | Yes |
> | spatialanchorsaccounts | Yes | Yes |

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
> | işlemler | Hayır | Hayır |

## <a name="microsoftnetwork"></a>Microsoft.Network

> [!IMPORTANT]
> Bkz. [ağ taşıma Kılavuzu](./move-limitations/networking-move-limitations.md).

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | applicationgatewaykullanılabilirliği blerequestheaders | Hayır | Hayır |
> | applicationgatewaykullanılabilirliği bleresponseheaders | Hayır | Hayır |
> | applicationgatewayavailableservervariables | Hayır | Hayır |
> | applicationgatewayavailablessloptions | Hayır | Hayır |
> | applicationgatewayavailablewafrulesets | Hayır | Hayır |
> | applicationgateway 'ler | Hayır | Hayır |
> | applicationgatewaywebapplicationfirewallpolicies | Hayır | Hayır |
> | applicationsecuritygroups | Yes | Yes |
> | azurefirewallfqdntags | Hayır | Hayır |
> | azurefirewalls | Hayır | Hayır |
> | Savunma Konakları | Hayır | Hayır |
> | bgpservicecommunities | Hayır | Hayır |
> | checkfrontdoornameavailability | Hayır | Hayır |
> | checktrafficmanagernameavailability | Hayır | Hayır |
> | bağlantının | Yes | Yes |
> | ddoscustompolicies | Yes | Yes |
> | ddosprotectionplanlar | Hayır | Hayır |
> | dnsoperationsonuçları | Hayır | Hayır |
> | dnsoperationdurumları | Hayır | Hayır |
> | dnszones | Yes | Yes |
> | dnszones/a | Hayır | Hayır |
> | dnszones/aaaa | Hayır | Hayır |
> | dnszones/tümü | Hayır | Hayır |
> | dnszones/CAA | Hayır | Hayır |
> | dnszones/CNAME | Hayır | Hayır |
> | dnszones/MX | Hayır | Hayır |
> | dnszones/NS | Hayır | Hayır |
> | dnszones/PTR | Hayır | Hayır |
> | dnszones/kayıt kümeleri | Hayır | Hayır |
> | dnszones/SOA | Hayır | Hayır |
> | dnszones/SRV | Hayır | Hayır |
> | dnszones/txt | Hayır | Hayır |
> | expressroutedevreleri | Hayır | Hayır |
> | expressroutegateway 'ler | Hayır | Hayır |
> | expressrouteserviceproviders | Hayır | Hayır |
> | firewallpolicies | Yes | Yes |
> | frontdooroperationresults | Hayır | Hayır |
> | frontkapıların | Hayır | Hayır |
> | frontkapaklı 'lar/ön uçlar | Hayır | Hayır |
> | frontdoorwebapplicationfirewallmanagedrulesets | Hayır | Hayır |
> | frontdoorwebapplicationfirewallpolicies | Hayır | Hayır |
> | getdnsresourcereference | Hayır | Hayır |
> | ınternalnotify | Hayır | Hayır |
> | ipayırmalar | Yes | Yes |
> | ipgroups | Yes | Yes |
> | loadbalancers | Evet-temel SKU<br>Standart SKU yok | Evet-temel SKU<br>Standart SKU yok |
> | localnetworkgateway 'ler | Yes | Yes |
> | yerlerini | Hayır | Hayır |
> | konumlar/oto approlarprivatelinkservices | Hayır | Hayır |
> | konumlar/availabletemsilciler | Hayır | Hayır |
> | konumlar/availableprivateendpointtypes | Hayır | Hayır |
> | konumlar/availableservicediğer adlar | Hayır | Hayır |
> | konumlar/baremetaltenants | Hayır | Hayır |
> | konumlar/batchnotifyprivateendpointsforresourcemove | Hayır | Hayır |
> | konumlar/batchvalidateprivateendpointsforresourcemove | Hayır | Hayır |
> | konumlar/checkivme Networkingsupport | Hayır | Hayır |
> | konumlar/checkdnsnameavaılabılıty | Hayır | Hayır |
> | konumlar/checkprivatelinkservicevisibility | Hayır | Hayır |
> | konumlar/commitinternalazurenetworkmanagerconfiguration | Hayır | Hayır |
> | konumlar/effectiveresourceownership | Hayır | Hayır |
> | konumlar/nfvoperationresults | Hayır | Hayır |
> | konumlar/nfvoperations | Hayır | Hayır |
> | konumlar/operationresults | Hayır | Hayır |
> | konumlar/işlemler | Hayır | Hayır |
> | konumlar/servicetags | Hayır | Hayır |
> | konumlar/setresourcesahiplik | Hayır | Hayır |
> | konumlar/supportedvirtuallarines | Hayır | Hayır |
> | konumlar/kullanımlar | Hayır | Hayır |
> | konumlar/validateresourcesahiplik | Hayır | Hayır |
> | konumlar/virtualnetworkavailableendpointservices | Hayır | Hayır |
> | natgateway 'ler | Hayır | Hayır |
> | networkexperimentprofiles | Hayır | Hayır |
> | networkıntpolicies ilkeleri | Yes | Yes |
> | NetworkInterfaces | Yes | Yes |
> | networkprofiles | Hayır | Hayır |
> | networksecuritygroups | Yes | Yes |
> | networkwatchers | Evet | Hayır |
> | networkwatchers/connectionmonitörleri | Evet | Hayır |
> | networkwatchers/flowlogs | Evet | Hayır |
> | networkwatchers/pingkafesler | Evet | Hayır |
> | işlemler | Hayır | Hayır |
> | p2svpngateways | Hayır | Hayır |
> | privatednsoperationresults | Hayır | Hayır |
> | privatednsoperationstatuses | Hayır | Hayır |
> | privatednszones | Yes | Yes |
> | privatednszones/a | Hayır | Hayır |
> | privatednszones/aaaa | Hayır | Hayır |
> | privatednszones/tümü | Hayır | Hayır |
> | privatednszones/CNAME | Hayır | Hayır |
> | privatednszones/MX | Hayır | Hayır |
> | privatednszones/PTR | Hayır | Hayır |
> | privatednszones/SOA | Hayır | Hayır |
> | privatednszones/SRV | Hayır | Hayır |
> | privatednszones/txt | Hayır | Hayır |
> | privatednszones/virtualnetworklinks | Yes | Yes |
> | privatednszonesinternal | Hayır | Hayır |
> | privateendpointredirectmaps | Hayır | Hayır |
> | privateendpoints | Yes | Yes |
> | privatelinkservices | Hayır | Hayır |
> | publicıpaddresses | Evet-temel SKU<br>Standart SKU yok | Evet-temel SKU<br>Standart SKU yok |
> | publicıpöneklerini | Yes | Yes |
> | routefilters | Hayır | Hayır |
> | routetables | Yes | Yes |
> | securitypartnerproviders | Yes | Yes |
> | serviceendpointpolicies | Yes | Yes |
> | trafficmanagergeographichierarchies | Hayır | Hayır |
> | trafficmanagerprofiles | Yes | Yes |
> | trafficmanagerprofiles/ısı haritalarını | Hayır | Hayır |
> | trafficmanagerusermetricskeys | Hayır | Hayır |
> | virtualhub 'lar | Hayır | Hayır |
> | virtualnetworkgateways | Yes | Yes |
> | virtualnetworks | Yes | Yes |
> | virtualnetworktaps | Hayır | Hayır |
> | virtualyönlendiriciler | Yes | Yes |
> | virtualwan | Hayır | Hayır |
> | vpngateway (sanal WAN) | Hayır | Hayır |
> | vpnserverconfigurations | Hayır | Hayır |
> | vpnsites (sanal WAN) | Hayır | Hayır |

## <a name="microsoftnotificationhubs"></a>Microsoft. Notificationhub 'Lar

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | checknameavaılabılıty | Hayır | Hayır |
> | checknamespaceavailability | Hayır | Hayır |
> | öznitelikleri | Yes | Yes |
> | ad alanları/notificationhub 'lar | Yes | Yes |
> | operationresults | Hayır | Hayır |
> | işlemler | Hayır | Hayır |

## <a name="microsoftobjectstore"></a>Microsoft. ObjectStore

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | osnamespaces | Yes | Yes |

## <a name="microsoftoffazure"></a>Microsoft. OffAzure

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | Hiper sanal siteler | Hayır | Hayır |
> | ımportsites | Hayır | Hayır |
> | işlemler | Hayır | Hayır |
> | sunucusiteleri | Hayır | Hayır |
> | vmwaresites | Hayır | Hayır |

## <a name="microsoftoperationalinsights"></a>Microsoft. Operationalınsights

> [!IMPORTANT]
> Yeni bir aboneliğe geçiş için [abonelik kotalarını](azure-subscription-service-limits.md#azure-monitor-limits)aşmayacağından emin olun.
>
> Bağlı bir Otomasyon hesabına sahip olan çalışma alanları taşınamaz. Bir taşıma işlemine başlamadan önce, tüm otomasyon hesaplarının bağlantısını kaldırmayı unutmayın.

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | leriniz | Hayır | Hayır |
> | deletedworkspaces | Hayır | Hayır |
> | bağlantı hedefleri | Hayır | Hayır |
> | yerlerini | Hayır | Hayır |
> | konumlar/operationdurumlarının | Hayır | Hayır |
> | işlemler | Hayır | Hayır |
> | storageınsii configs | Hayır | Hayır |
> | çalışma alanı | Yes | Yes |
> | çalışma alanları/veri kaynakları | Hayır | Hayır |
> | çalışma alanları/linkedservices | Hayır | Hayır |
> | çalışma alanları/linkedstorageaccounts | Hayır | Hayır |
> | çalışma alanları/meta veriler | Hayır | Hayır |
> | çalışma alanları/sorgu | Hayır | Hayır |
> | çalışma alanları/scopedprivatelinkproxy 'leri | Hayır | Hayır |

## <a name="microsoftoperationsmanagement"></a>Microsoft. OperationsManagement

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | managementassociations | Hayır | Hayır |
> | managementconfigurations | Yes | Yes |
> | işlemler | Hayır | Hayır |
> | çözümler | Yes | Yes |
> | görünümler | Yes | Yes |

## <a name="microsoftpeering"></a>Microsoft. eşleme

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | checkserviceprovideravaılabılıty | Hayır | Hayır |
> | Yasallıklar | Hayır | Hayır |
> | işlemler | Hayır | Hayır |
> | peerasns | Hayır | Hayır |
> | peeringlocations | Hayır | Hayır |
> | eşlemeleri | Hayır | Hayır |
> | peeringserviceülkeleriyle | Hayır | Hayır |
> | peeringserviceloing | Hayır | Hayır |
> | peeringserviceproviders | Hayır | Hayır |
> | peeringservices | Hayır | Hayır |

## <a name="microsoftpolicyinsights"></a>Microsoft. Poliyelei

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | asyncoperationsonuçları | Hayır | Hayır |
> | işlemler | Hayır | Hayır |
> | poliyevents | Hayır | Hayır |
> | policystates | Hayır | Hayır |
> | policytrackedresources | Hayır | Hayır |
> | düzeltmeler | Hayır | Hayır |

## <a name="microsoftportal"></a>Microsoft. Portal

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> |  konsolları | Hayır | Hayır |
> | panolar | Yes | Yes |
> | yerlerini | Hayır | Hayır |
> | konumlar/konsollar | Hayır | Hayır |
> | konumlar/UserSettings | Hayır | Hayır |
> | işlemler | Hayır | Hayır |
> | UserSettings | Hayır | Hayır |

## <a name="microsoftpowerbi"></a>Microsoft. PowerBI

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | yerlerini | Hayır | Hayır |
> | konumlar/checknameavaılabılıty | Hayır | Hayır |
> | workspacecollections | Yes | Yes |

## <a name="microsoftpowerbidedicated"></a>Microsoft. Powerbiadanmış

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | kapasiteler | Yes | Yes |
> | yerlerini | Hayır | Hayır |
> | konumlar/checknameavaılabılıty | Hayır | Hayır |
> | konumlar/operationresults | Hayır | Hayır |
> | konumlar/operationdurumlarının | Hayır | Hayır |
> | işlemler | Hayır | Hayır |

## <a name="microsoftpowerplatform"></a>Microsoft. PowerPlatform

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | işlemler | Hayır | Hayır |

## <a name="microsoftprojectbabylon"></a>Microsoft. ProjectBabylon

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | accounts | Hayır | Hayır |
> | checknameavaılabılıty | Hayır | Hayır |
> | işlemler | Hayır | Hayır |

## <a name="microsoftproviderhub"></a>Microsoft. ProviderHub

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | availableaccounts | Hayır | Hayır |
> | providerkayıtları | Hayır | Hayır |
> | providerkayıtlarıyla/resourcetyperegistrations | Hayır | Hayır |
> | piyasaya çıkarma | Hayır | Hayır |

## <a name="microsoftquantum"></a>Microsoft. hisse

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | yerlerini | Hayır | Hayır |
> | konumlar/operationdurumlarının | Hayır | Hayır |
> | işlemler | Hayır | Hayır |
> | çalışma alanı | Hayır | Hayır |

## <a name="microsoftrecoveryservices"></a>Microsoft. RecoveryServices

> [!IMPORTANT]
> Bkz. [Kurtarma Hizmetleri taşıma Kılavuzu](../../backup/backup-azure-move-recovery-services-vault.md?toc=/azure/azure-resource-manager/toc.json).

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | backupkorunabilir | Hayır | Hayır |
> | yerlerini | Hayır | Hayır |
> | konumlar/allocatedstamp | Hayır | Hayır |
> | konumlar/allocatestamp | Hayır | Hayır |
> | konumlar/backupaadproperties | Hayır | Hayır |
> | konumlar/backupcrossregionrestore | Hayır | Hayır |
> | konumlar/backupcrrjob | Hayır | Hayır |
> | konumlar/backupcrrjobs | Hayır | Hayır |
> | konumlar/backupcrroperationsonuçları | Hayır | Hayır |
> | konumlar/backupcrroperationsstatus | Hayır | Hayır |
> | konumlar/backupprevalidateprotection | Hayır | Hayır |
> | konumlar/BackupStatus | Hayır | Hayır |
> | konumlar/backupvalidatefeatures | Hayır | Hayır |
> | konumlar/checknameavaılabılıty | Hayır | Hayır |
> | işlemler | Hayır | Hayır |
> | replicationeligibilityresults | Hayır | Hayır |
> | kasaları | Yes | Yes |

## <a name="microsoftredhatopenshift"></a>Microsoft. RedHatOpenShift

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | yerlerini | Hayır | Hayır |
> | konumlar/operationresults | Hayır | Hayır |
> | konumlar/operationsstatus | Hayır | Hayır |
> | openshiftclusters | Hayır | Hayır |
> | işlemler | Hayır | Hayır |

## <a name="microsoftrelay"></a>Microsoft. Relay

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | checknameavaılabılıty | Hayır | Hayır |
> | öznitelikleri | Yes | Yes |
> | ad alanları/authorizationrules | Hayır | Hayır |
> | ad alanları/hybridconnections | Hayır | Hayır |
> | ad alanları/hybridconnections/authorizationrules | Hayır | Hayır |
> | ad alanları/privateendpointconnections | Hayır | Hayır |
> | ad alanları/wcfreyerleştiri | Hayır | Hayır |
> | ad alanları/wcfreyerleştirme/authorizationrules | Hayır | Hayır |
> | işlemler | Hayır | Hayır |

## <a name="microsoftresourcegraph"></a>Microsoft. ResourceGraph

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | işlemler | Hayır | Hayır |
> | lardır | Yes | Yes |
> | resourcechangedetails | Hayır | Hayır |
> | resourcechanges | Hayır | Hayır |
> | kaynaklar | Hayır | Hayır |
> | resourceshistory | Hayır | Hayır |
> | subscriptionsstatus | Hayır | Hayır |

## <a name="microsoftresourcehealth"></a>Microsoft. ResourceHealth

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | kullanılabilirlik durumları | Hayır | Hayır |
> | childadvailabilitydurumlar | Hayır | Hayır |
> | childresources | Hayır | Hayır |
> | acil sorun sorunları | Hayır | Hayır |
> | etkinlikler | Hayır | Hayır |
> | meta veriler | Hayır | Hayır |
> | bildirimler | Hayır | Hayır |
> | işlemler | Hayır | Hayır |

## <a name="microsoftresources"></a>Microsoft. resources

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | calculatetemplatehash | Hayır | Hayır |
> | checkpolicyuyumluluğu | Hayır | Hayır |
> | checkresourcename | Hayır | Hayır |
> | dağıtımlar | Hayır | Hayır |
> | dağıtımlar/işlemler | Hayır | Hayır |
> | deploymentscripts | Hayır | Hayır |
> | deploymentscripts/Günlükler | Hayır | Hayır |
> | Köprü | Hayır | Hayır |
> | yerlerini | Hayır | Hayır |
> | konumlar/deploymentscriptoperationsonuçları | Hayır | Hayır |
> | notifyresourcejobs | Hayır | Hayır |
> | operationresults | Hayır | Hayır |
> | işlemler | Hayır | Hayır |
> | sağlayıcılarla | Hayır | Hayır |
> | ResourceGroups | Hayır | Hayır |
> | kaynaklar | Hayır | Hayır |
> | Aboneliklerin | Hayır | Hayır |
> | Abonelikler/konumlar | Hayır | Hayır |
> | Abonelikler/operationresults | Hayır | Hayır |
> | Abonelikler/sağlayıcılar | Hayır | Hayır |
> | Abonelikler/ResourceGroups | Hayır | Hayır |
> | Abonelikler/ResourceGroups/kaynaklar | Hayır | Hayır |
> | Abonelikler/kaynaklar | Hayır | Hayır |
> | Abonelikler/etiket adları | Hayır | Hayır |
> | Abonelikler/etiket adları/tagvalues | Hayır | Hayır |
> | tags | Hayır | Hayır |
> | templatespec | Hayır | Hayır |
> | templatespec/sürümler | Hayır | Hayır |
> | Kira | Hayır | Hayır |

## <a name="microsoftsaas"></a>Microsoft. SaaS

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | uygulamalar | Yes | Hayır |
> | checkmoderneligibility | Hayır | Hayır |
> | checknameavaılabılıty | Hayır | Hayır |
> | operationresults | Hayır | Hayır |
> | işlemler | Hayır | Hayır |
> | saasresources | Hayır | Hayır |

## <a name="microsoftsearch"></a>Microsoft. Search

> [!IMPORTANT]
> Farklı bölgelerde çeşitli arama kaynaklarını tek bir işlemde taşıyamazsınız. Bunun yerine, bunları ayrı işlemlerde taşıyın.

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | checknameavaılabılıty | Hayır | Hayır |
> | checkservicenameavaılabılıty | Hayır | Hayır |
> | işlemler | Hayır | Hayır |
> | resourcehealthmetadata | Hayır | Hayır |
> | searchservices | Yes | Yes |

## <a name="microsoftsecurity"></a>Microsoft.Security

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | adaptivenetworkhardenings | Hayır | Hayır |
> | advancedthreatprotectionsettings | Hayır | Hayır |
> | alerts | Hayır | Hayır |
> | allowedconnections | Hayır | Hayır |
> | applicationwhitedökümler | Hayır | Hayır |
> | assessmentmetadata | Hayır | Hayır |
> | değerlendirme | Hayır | Hayır |
> | Oto dissalertsrules | Hayır | Hayır |
> | akışlarını otomatikleştirin | Yes | Yes |
> | Oto provisioningsettings | Hayır | Hayır |
> | karmaşık anceresults | Hayır | Hayır |
> | uyumluluklarına | Hayır | Hayır |
> | datacollectionagents | Hayır | Hayır |
> | devicesecuritygroups | Hayır | Hayır |
> | discoveredsecuritysolutions | Hayır | Hayır |
> | externalsecuritysolutions | Hayır | Hayır |
> | ınformationprotectionpolicies | Hayır | Hayır |
> | ıotsecuritysolutions | Yes | Yes |
> | ıotsecuritysolutions/analiz ticsmodeller | Hayır | Hayır |
> | ıotsecuritysolutions/analticsmodeller/aggreggıt uyarıları | Hayır | Hayır |
> | ıotsecuritysolutions/analiz ticsmodeller/Aggreg, öneriler | Hayır | Hayır |
> | jağaccesspolicies | Hayır | Hayır |
> | yerlerini | Hayır | Hayır |
> | konumlar/uyarılar | Hayır | Hayır |
> | konumlar/allowedconnections | Hayır | Hayır |
> | konumlar/uygulama beyaz listeleri | Hayır | Hayır |
> | Locations/discoveredsecuritysolutions | Hayır | Hayır |
> | konumlar/externalsecuritysolutions | Hayır | Hayır |
> | Locations/jağaccesspolicies ilkeleri | Hayır | Hayır |
> | konumlar/securitysolutions | Hayır | Hayır |
> | Locations/securitysolutionsreferencedata | Hayır | Hayır |
> | konumlar/görevler | Hayır | Hayır |
> | konumlar/topolojiler | Hayır | Hayır |
> | işlemler | Hayır | Hayır |
> | ilkeler | Hayır | Hayır |
> | fiyatlandırmalar | Hayır | Hayır |
> | Reve daha karmaşık bakım standartları | Hayır | Hayır |
> | rekontrol ve Re, | Hayır | Hayır |
> | rekontrol ve Re, Re, Re, | Hayır | Hayır |
> | securitycontacts | Hayır | Hayır |
> | securitysolutions | Hayır | Hayır |
> | securitysolutionsreferencedata | Hayır | Hayır |
> | securitydurumlardan | Hayır | Hayır |
> | securitystatusessummaries | Hayır | Hayır |
> | sunucukullanılabilirliği | Hayır | Hayır |
> | ayarlar | Hayır | Hayır |
> | subassessments | Hayır | Hayır |
> | Görevler | Hayır | Hayır |
> | anlatır | Hayır | Hayır |
> | çalışma alanı ayarları | Hayır | Hayır |

## <a name="microsoftsecurityinsights"></a>Microsoft. Securityınsights

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | toplamaları | Hayır | Hayır |
> | alertrules | Hayır | Hayır |
> | alertrutatemplates | Hayır | Hayır |
> | automationrules | Hayır | Hayır |
> | yer işaretleri | Hayır | Hayır |
> | çalışmaların | Hayır | Hayır |
> | veri bağlayıcıları | Hayır | Hayır |
> | dataconnectorscheckrequirements | Hayır | Hayır |
> | varlıklar | Hayır | Hayır |
> | entityqueries | Hayır | Hayır |
> | olaylar | Hayır | Hayır |
> | officeconsents | Hayır | Hayır |
> | işlemler | Hayır | Hayır |
> | ayarlar | Hayır | Hayır |
> | threatıntelligence | Hayır | Hayır |

## <a name="microsoftserialconsole"></a>Microsoft. SerialConsole

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | consoleservices | Hayır | Hayır |
> | yerlerini | Hayır | Hayır |
> | konumlar/consoleservices | Hayır | Hayır |
> | işlemler | Hayır | Hayır |

## <a name="microsoftservermanagement"></a>Microsoft. ServerManagement

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | geçidinin | Hayır | Hayır |
> | düğümlerini | Hayır | Hayır |

## <a name="microsoftservicebus"></a>Microsoft.ServiceBus

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | checknameavaılabılıty | Hayır | Hayır |
> | checknamespaceavailability | Hayır | Hayır |
> | yerlerini | Hayır | Hayır |
> | konumlar/deletevirtualnetworkoralt ağları | Hayır | Hayır |
> | öznitelikleri | Yes | Yes |
> | ad alanları/authorizationrules | Hayır | Hayır |
> | ad alanları/diskalrecoveryconfigs | Hayır | Hayır |
> | ad alanları/diskalrecoveryconfigs/checknameavaılabılıty | Hayır | Hayır |
> | ad alanları/eventgridfilters | Hayır | Hayır |
> | ad alanları/networkrulesets | Hayır | Hayır |
> | ad alanları/kuyruklar | Hayır | Hayır |
> | ad alanları/kuyruklar/authorizationrules | Hayır | Hayır |
> | ad alanları/konular | Hayır | Hayır |
> | ad alanları/konular/authorizationrules | Hayır | Hayır |
> | ad alanları/konular/abonelikler | Hayır | Hayır |
> | ad alanları/konular/abonelikler/kurallar | Hayır | Hayır |
> | işlemler | Hayır | Hayır |
> | premiummessagingregions | Hayır | Hayır |
> | isteyin | Hayır | Hayır |

## <a name="microsoftservicefabric"></a>Microsoft. ServiceFabric

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | uygulamalar | Hayır | Hayır |
> | leriniz | Yes | Yes |
> | kümeler/uygulamalar | Hayır | Hayır |
> | kapsayıcı grupları | Hayır | Hayır |
> | containergroupsets | Hayır | Hayır |
> | edgeclusters | Hayır | Hayır |
> | yerlerini | Hayır | Hayır |
> | konumlar/kümesürümleri | Hayır | Hayır |
> | konumlar/ortamlar | Hayır | Hayır |
> | konumlar/operationresults | Hayır | Hayır |
> | konumlar/işlemler | Hayır | Hayır |
> | managedkümeler | Hayır | Hayır |
> | Mamak | Hayır | Hayır |
> | işlemler | Hayır | Hayır |
> | secretmağazaları | Hayır | Hayır |
> | volumes | Hayır | Hayır |

## <a name="microsoftservicefabricmesh"></a>Microsoft. Servicefabrickafesi

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | uygulamalar | Yes | Yes |
> | kapsayıcı grupları | Hayır | Hayır |
> | geçidinin | Yes | Yes |
> | yerlerini | Hayır | Hayır |
> | konumlar/applicationoperations | Hayır | Hayır |
> | konumlar/gatewayoperations | Hayır | Hayır |
> | konumlar/networkoperations | Hayır | Hayır |
> | konumlar/secretoperations | Hayır | Hayır |
> | konumlar/birimsiz işlemler | Hayır | Hayır |
> | Mamak | Yes | Yes |
> | işlemler | Hayır | Hayır |
> | kaynaklanır | Yes | Yes |
> | volumes | Yes | Yes |

## <a name="microsoftservices"></a>Microsoft. Services

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | piyasaya çıkarma | Hayır | Hayır |

## <a name="microsoftsignalrservice"></a>Microsoft. SignalRService

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | yerlerini | Hayır | Hayır |
> | konumlar/checknameavaılabılıty | Hayır | Hayır |
> | konumlar/operationresults | Hayır | Hayır |
> | konumlar/operationdurumlarının | Hayır | Hayır |
> | konumlar/kullanımlar | Hayır | Hayır |
> | işlemler | Hayır | Hayır |
> | SignalR | Yes | Yes |
> | SignalR/eventgridfilters | Hayır | Hayır |

## <a name="microsoftsoftwareplan"></a>Microsoft. SoftwarePlan

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | hybriduseavantajlar | Hayır | Hayır |
> | işlemler | Hayır | Hayır |

## <a name="microsoftsolutions"></a>Microsoft. Solutions

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | applicationdefinitions | Hayır | Hayır |
> | uygulamalar | Hayır | Hayır |
> | jistekleri | Hayır | Hayır |
> | yerlerini | Hayır | Hayır |
> | konumlar/operationdurumlarının | Hayır | Hayır |
> | işlemler | Hayır | Hayır |

## <a name="microsoftsql"></a>Microsoft.Sql

> [!IMPORTANT]
> Bir veritabanı ve sunucu aynı kaynak grubunda olmalıdır. Bir SQL sunucusunu taşıdığınızda, tüm veritabanları da taşınır. Bu davranış, Azure SQL veritabanı ve Azure SYNAPSE Analytics veritabanları için geçerlidir.

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | checknameavaılabılıty | Hayır | Hayır |
> | ınstancepools | Hayır | Hayır |
> | yerlerini | Yes | Yes |
> | konumlar/yönetimtolarzureasyncoperation | Hayır | Hayır |
> | konumlar/yönetimtoroperationsonuçları | Hayır | Hayır |
> | Locations/auditingsettingsazureasyncoperation | Hayır | Hayır |
> | konumlar/auditingsettingsoperationsonuçları | Hayır | Hayır |
> | konumlar/yetenekler | Hayır | Hayır |
> | konumlar/databaseazureasyncoperation | Hayır | Hayır |
> | konumlar/databaseoperationresults | Hayır | Hayır |
> | konumlar/databaserestoreazureasyncoperation | Hayır | Hayır |
> | konumlar/deletevirtualnetworkoralt ağları | Hayır | Hayır |
> | konumlar/deletevirtualnetworkorsubnetsazureasyncoperation | Hayır | Hayır |
> | konumlar/deletevirtualnetworkorsubnetsoperationsonuçları | Hayır | Hayır |
> | konumlar/dnsoiasasyncoperation | Hayır | Hayır |
> | konumlar/dnsalıasoperationsonuçları | Hayır | Hayır |
> | konumlar/elasticpoolazureasyncoperation | Hayır | Hayır |
> | konumlar/elaetiketler pooloperationresults | Hayır | Hayır |
> | konumlar/encryptionkorunabilir torampareasyncoperation | Hayır | Hayır |
> | konumlar/encryptionkorunabilir sonuçları | Hayır | Hayır |
> | konumlar/extendedauditingsettingsazureasyncoperation | Hayır | Hayır |
> | konumlar/extendedauditingsettingsoperationsonuçları | Hayır | Hayır |
> | konumlar/failovergroupazureasyncoperation | Hayır | Hayır |
> | konumlar/failovergroupoperationsonuçları | Hayır | Hayır |
> | konumlar/firewallrulesazureasyncoperation | Hayır | Hayır |
> | konumlar/firewallrulesoperationsonuçları | Hayır | Hayır |
> | konumlar/instancefailovergroupazureasyncoperation | Hayır | Hayır |
> | konumlar/ınstancefailovergroupoperationsonuçları | Hayır | Hayır |
> | konumlar/ınstancefailovergroups | Hayır | Hayır |
> | konumlar/instancepoolazureasyncoperation | Hayır | Hayır |
> | konumlar/ınstancepooloperationresults | Hayır | Hayır |
> | konumlar/jobagentazureasyncoperation | Hayır | Hayır |
> | konumlar/jobagentoperationsonuçları | Hayır | Hayır |
> | konumlar/longtermretentionbackupazureasyncoperation | Hayır | Hayır |
> | konumlar/longtermretentionbackupoperationresults | Hayır | Hayır |
> | konumlar/longtermretentionbackups | Hayır | Hayır |
> | konumlar/longtermretentionmanagedinstancebackupazureasyncoperation | Hayır | Hayır |
> | konumlar/longtermretentionmanagedinstancebackupoperationresults | Hayır | Hayır |
> | konumlar/longtermretentionmanagedinstancebackups | Hayır | Hayır |
> | konumlar/longtermretentionmanagedinstances | Hayır | Hayır |
> | konumlar/longtermretentionpolicyazureasyncoperation | Hayır | Hayır |
> | konumlar/longtermretentionpolicyoperationresults | Hayır | Hayır |
> | konumlar/longtermretentionservers | Hayır | Hayır |
> | konumlar/manageddatabaseazureasyncoperation | Hayır | Hayır |
> | konumlar/manageddatabasemediaterestoreazureasyncoperation | Hayır | Hayır |
> | konumlar/manageddatabasetamterestoreoperationresults | Hayır | Hayır |
> | konumlar/manageddatabaseoperationresults | Hayır | Hayır |
> | konumlar/manageddatabaserestoreazureasyncoperation | Hayır | Hayır |
> | konumlar/manageddatabaserestoreoperationresults | Hayır | Hayır |
> | konumlar/managedınstanceazureasyncoperation | Hayır | Hayır |
> | konumlar/managedınstanceencryptionkorunabilir korunabilir | Hayır | Hayır |
> | konumlar/managedınstanceencryptionkorunabilir korunabilir sonuçları | Hayır | Hayır |
> | konumlar/managedinstancekeyazureasyncoperation | Hayır | Hayır |
> | konumlar/managedinstancekeyoperationresults | Hayır | Hayır |
> | konumlar/managedinstancelongtermretentionpolicyazureasyncoperation | Hayır | Hayır |
> | konumlar/managedinstancelongtermretentionpolicyoperationresults | Hayır | Hayır |
> | konumlar/managedınstanceoperationresults | Hayır | Hayır |
> | konumlar/managedinstancetdecertazureasyncoperation | Hayır | Hayır |
> | konumlar/managedınstancetdecertoperationresults | Hayır | Hayır |
> | konumlar/managedserversecurityalertpoliciesazureasyncoperation | Hayır | Hayır |
> | konumlar/managedserversecurityalcertpoliciesoperationresults | Hayır | Hayır |
> | konumlar/managedshorttermretentionpolicyazureasyncoperation | Hayır | Hayır |
> | konumlar/managedshorttermretentionpolicyoperationresults | Hayır | Hayır |
> | konumlar/bildirimler/notifsyncoperation | Hayır | Hayır |
> | konumlar/privateendpointconnectionazureasyncoperation | Hayır | Hayır |
> | konumlar/privateendpointconnectionoperationresults | Hayır | Hayır |
> | konumlar/privateendpointconnectionproxyazureasyncoperation | Hayır | Hayır |
> | konumlar/privateendpointconnectionproxyoperationsonuçları | Hayır | Hayır |
> | konumlar/securityalertpoliciesazureasyncoperation | Hayır | Hayır |
> | konumlar/securityalcertpoliciesoperationresults | Hayır | Hayır |
> | konumlar/serveryönetimtoredzureasyncoperation | Hayır | Hayır |
> | konumlar/serveryönetimtoroperationsonuçları | Hayır | Hayır |
> | konumlar/serverazureasyncoperation | Hayır | Hayır |
> | konumlar/sunucukeyazureasyncoperation | Hayır | Hayır |
> | konumlar/sunucukeyoperationsonuçları | Hayır | Hayır |
> | konumlar/serveroperationresults | Hayır | Hayır |
> | konumlar/shorttermretentionpolicyazureasyncoperation | Hayır | Hayır |
> | konumlar/shorttermretentionpolicyoperationresults | Hayır | Hayır |
> | konumlar/syncagentoperationsonuçları | Hayır | Hayır |
> | konumlar/syncdatabaseıds | Hayır | Hayır |
> | konumlar/syncgroupoperationsonuçları | Hayır | Hayır |
> | konumlar/syncmemberoperationresults | Hayır | Hayır |
> | konumlar/tdecertazureasyncoperation | Hayır | Hayır |
> | konumlar/tdecertoperationsonuçları | Hayır | Hayır |
> | konumlar/kullanımlar | Hayır | Hayır |
> | konumlar/virtualclusterazureasyncoperation | Hayır | Hayır |
> | konumlar/virtualclusteroperationresults | Hayır | Hayır |
> | konumlar/virtualnetworkrulesazureasyncoperation | Hayır | Hayır |
> | konumlar/virtualnetworkrulesoperationsonuçları | Hayır | Hayır |
> | konumlar/etkinlik/etkinlik 'p ıyassessmentscanazureasyncoperation | Hayır | Hayır |
> | konumlar/bu işlem scanıshsessmentscanoperationresults | Hayır | Hayır |
> | ManagedInstances | Hayır | Hayır |
> | ManagedInstances/Administrators | Hayır | Hayır |
> | ManagedInstances/veritabanları | Hayır | Hayır |
> | ManagedInstances/veritabanları/backuplongtermretentionpolicies | Hayır | Hayır |
> | ManagedInstances/veritabanları/ | Hayır | Hayır |
> | ManagedInstances/MetricDefinitions | Hayır | Hayır |
> | ManagedInstances/ölçümler | Hayır | Hayır |
> | ManagedInstances/recoverabledatabases | Hayır | Hayır |
> | ManagedInstances/tdecertificates | Hayır | Hayır |
> | ManagedInstances/ | Hayır | Hayır |
> | işlemler | Hayır | Hayır |
> | larý | Yes | Yes |
> | sunucular/yönetimtoroperationsonuçları | Hayır | Hayır |
> | sunucular/Yöneticiler | Hayır | Hayır |
> | sunucular/danışmanları | Hayır | Hayır |
> | sunucular/Aggreg, databaseölçümlerini | Hayır | Hayır |
> | sunucular/auditingpolicies | Hayır | Hayır |
> | Sunucu/auditingsettings | Hayır | Hayır |
> | sunucular/otomatik olarak ayarlama | Hayır | Hayır |
> | sunucular/communicationlinks | Hayır | Hayır |
> | sunucular/connectionpolicies | Hayır | Hayır |
> | sunucular/veritabanları | Yes | Yes |
> | sunucular/veritabanları/danışmanları | Hayır | Hayır |
> | sunucular/veritabanları/auditingpolicies | Hayır | Hayır |
> | sunucular/veritabanları/auditingsettings | Hayır | Hayır |
> | sunucular/veritabanları/denetimkayıtları | Hayır | Hayır |
> | sunucular/veritabanları/otomatik olarak ayarlama | Hayır | Hayır |
> | sunucular/veritabanları/backuplongtermretentionpolicies | Hayır | Hayır |
> | sunucular/veritabanları/backupshorttermretentionpolicies | Hayır | Hayır |
> | sunucular/veritabanları/connectionpolicies | Hayır | Hayır |
> | sunucular/veritabanları/datamaskingpolicies | Hayır | Hayır |
> | sunucular/veritabanları/datamaskingpolicies/kurallar | Hayır | Hayır |
> | sunucular/veritabanları/uzantılar | Hayır | Hayır |
> | sunucular/veritabanları/geobackuppolicies | Hayır | Hayır |
> | sunucular/veritabanları/MetricDefinitions | Hayır | Hayır |
> | sunucular/veritabanları/ölçümler | Hayır | Hayır |
> | sunucular/veritabanları/recommendedsensitivitylabels | Hayır | Hayır |
> | sunucular/veritabanları/securityalcertpolicies | Hayır | Hayır |
> | sunucular/veritabanları/syncgroups | Hayır | Hayır |
> | sunucular/veritabanları/syncgroups/SyncMembers | Hayır | Hayır |
> | sunucular/veritabanları/topqueries | Hayır | Hayır |
> | sunucular/veritabanları/topqueries/QueryText | Hayır | Hayır |
> | sunucular/veritabanları/transparentdataencryption | Hayır | Hayır |
> | sunucular/veritabanları/bir düzeltme, bir değerlendirme | Hayır | Hayır |
> | sunucular/veritabanları/ek bir deyişle | Hayır | Hayır |
> | sunucular/veritabanları/etkin taramalar | Hayır | Hayır |
> | sunucular/veritabanları/bu ayarlar | Hayır | Hayır |
> | sunucular/veritabanları/WorkloadGroups | Hayır | Hayır |
> | sunucular/databasesecuritypolicies | Hayır | Hayır |
> | sunucular/diskalrecoveryconfiguration | Hayır | Hayır |
> | Sunucu/dnsalses | Hayır | Hayır |
> | sunucular/elazpoolemates | Hayır | Hayır |
> | sunucular/elaun havuzları | Yes | Yes |
> | sunucular/elaor havuzları/danışmanları | Hayır | Hayır |
> | sunucular/elaun havuzları/MetricDefinitions | Hayır | Hayır |
> | sunucular/ela/havuzlar/ölçümler | Hayır | Hayır |
> | sunucular/encryptionprotector | Hayır | Hayır |
> | Servers/extendedauditingsettings | Hayır | Hayır |
> | sunucular/failovergroups 'lar | Hayır | Hayır |
> | sunucular/içeri aktarma | Hayır | Hayır |
> | sunucular/ımportexportoperationsonuçları | Hayır | Hayır |
> | sunucular/jobaccounts | Yes | Yes |
> | sunucular/jobagents | Yes | Yes |
> | sunucular/jobagents/işler | Hayır | Hayır |
> | sunucular/jobagents/işler/yürütmeler | Hayır | Hayır |
> | sunucular/jobagents/işler/adımlar | Hayır | Hayır |
> | sunucular/anahtarlar | Hayır | Hayır |
> | sunucular/operationresults | Hayır | Hayır |
> | sunucular/recommendedelasticpools | Hayır | Hayır |
> | sunucular/recoverabledatabases | Hayır | Hayır |
> | sunucular/restokbledroppeddatabases | Hayır | Hayır |
> | sunucular/securityalcertpolicies | Hayır | Hayır |
> | Sunucu/hizmet hedefleri | Hayır | Hayır |
> | sunucular/syncagents | Hayır | Hayır |
> | sunucular/tdecertificates | Hayır | Hayır |
> | sunucular/kullanımlar | Hayır | Hayır |
> | sunucular/virtualnetworkrules | Hayır | Hayır |
> | sunucular/ek bir deyişle | Hayır | Hayır |
> | virtualkümeler | Yes | Yes |

## <a name="microsoftsqlvirtualmachine"></a>Microsoft. SqlVirtualMachine

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | yerlerini | Hayır | Hayır |
> | konumlar/kullanılabilirlik bilitygrouplisteneroperationresults | Hayır | Hayır |
> | konumlar/operationtypes | Hayır | Hayır |
> | konumlar/sqlvirtualmachinegroupoperationresults | Hayır | Hayır |
> | konumlar/sqlvirtualmachineoperationresults | Hayır | Hayır |
> | işlemler | Hayır | Hayır |
> | sqlvirtualmachinegroups | Yes | Yes |
> | sqlvirtualmachinegroups/kullanılabilirliği bilitygrouplisteners | Hayır | Hayır |
> | sqlvirtualmachines | Yes | Yes |

## <a name="microsoftstorage"></a>Microsoft.Storage

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | checknameavaılabılıty | Hayır | Hayır |
> | yerlerini | Hayır | Hayır |
> | konumlar/asyncoperations | Hayır | Hayır |
> | konumlar/checknameavaılabılıty | Hayır | Hayır |
> | konumlar/deletevirtualnetworkoralt ağları | Hayır | Hayır |
> | konumlar/kullanımlar | Hayır | Hayır |
> | işlemler | Hayır | Hayır |
> | storageaccounts | Yes | Yes |
> | storageaccounts/blobservices | Hayır | Hayır |
> | storageaccounts/fileservices | Hayır | Hayır |
> | storageaccounts/listaccountsas | Hayır | Hayır |
> | storageaccounts/listservicesas | Hayır | Hayır |
> | storageaccounts/queueservices | Hayır | Hayır |
> | storageaccounts/Services | Hayır | Hayır |
> | storageaccounts/Services/MetricDefinitions | Hayır | Hayır |
> | storageaccounts/tableservices | Hayır | Hayır |
> | vardır | Hayır | Hayır |

## <a name="microsoftstoragecache"></a>Microsoft. StorageCache

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | önbelleklerinde | Hayır | Hayır |

## <a name="microsoftstoragesync"></a>Microsoft. Storagessync

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | yerlerini | Hayır | Hayır |
> | konumlar/checknameavaılabılıty | Hayır | Hayır |
> | konumlar/operationresults | Hayır | Hayır |
> | konumlar/işlemler | Hayır | Hayır |
> | konumlar/iş akışları | Hayır | Hayır |
> | işlemler | Hayır | Hayır |
> | storagesyncservices | Yes | Yes |
> | storagesyncservices/RegisteredServers | Hayır | Hayır |
> | storagesyncservices/syncgroups | Hayır | Hayır |
> | storagesyncservices/syncgroups/cloudendpoints | Hayır | Hayır |
> | storagesyncservices/syncgroups/serverendpoints | Hayır | Hayır |
> | storagesyncservices/iş akışları | Hayır | Hayır |

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

## <a name="microsoftstorsimple"></a>Microsoft. StorSimple

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | ilerinde | Hayır | Hayır |
> | işlemler | Hayır | Hayır |

## <a name="microsoftstreamanalytics"></a>Microsoft. StreamAnalytics

> [!IMPORTANT]
> Stream Analytics işleri çalışır durumda olduğunda taşınamaz.

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | leriniz | Hayır | Hayır |
> | yerlerini | Hayır | Hayır |
> | konumlar/kotalar | Hayır | Hayır |
> | işlemler | Hayır | Hayır |
> | streammingjobs | Yes | Yes |

## <a name="microsoftstreamanalyticsexplorer"></a>Microsoft. StreamAnalyticsExplorer

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | lý | Hayır | Hayır |
> | ortamlar/EventSources | Hayır | Hayır |
> | larında | Hayır | Hayır |
> | örnekler/ortamlar | Hayır | Hayır |
> | örnekler/ortamlar/EventSources | Hayır | Hayır |

## <a name="microsoftsubscription"></a>Microsoft. Subscription

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | iptal | Hayır | Hayır |
> | CreateSubscription | Hayır | Hayır |
> | seçin | Hayır | Hayır |
> | operationresults | Hayır | Hayır |
> | işlemler | Hayır | Hayır |
> | yeniden adlandır | Hayır | Hayır |
> | subscriptiondefinitions | Hayır | Hayır |
> | subscriptionoperations | Hayır | Hayır |
> | Aboneliklerin | Hayır | Hayır |

## <a name="microsoftsupport"></a>Microsoft. support

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | checknameavaılabılıty | Hayır | Hayır |
> | operationresults | Hayır | Hayır |
> | işlemler | Hayır | Hayır |
> | operationsstatus | Hayır | Hayır |
> | services | Hayır | Hayır |
> | Hizmetler/problemclassifications | Hayır | Hayır |
> | destek biletleri | Hayır | Hayır |

## <a name="microsoftsynapse"></a>Microsoft. SYNAPSE

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | checknameavaılabılıty | Hayır | Hayır |
> | işlemler | Hayır | Hayır |
> | çalışma alanı | Yes | Yes |
> | çalışma alanları/bigdatapools | Yes | Yes |
> | çalışma alanları/operationresults | Hayır | Hayır |
> | çalışma alanları/operationdurumlar | Hayır | Hayır |
> | çalışma alanları/sqlpools | Yes | Yes |

## <a name="microsofttimeseriesinsights"></a>Microsoft. Timeseriesınsights

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | lý | Yes | Yes |
> | ortamlar/accesspolicies | Hayır | Hayır |
> | ortamlar/EventSources | Yes | Yes |
> | ortamlar/referencedataset 'ler | Yes | Yes |
> | işlemler | Hayır | Hayır |

## <a name="microsofttoken"></a>Microsoft. Token

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | depolaya | Yes | Yes |
> | depolar/accesspolicies | Hayır | Hayır |
> | depolar/hizmetler | Hayır | Hayır |
> | depolar/hizmetler/belirteçler | Hayır | Hayır |

## <a name="microsoftvirtualmachineimages"></a>Microsoft. Virtualmachineımages

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | ımagetemplates | Hayır | Hayır |
> | ımagetemplates/runçıktılar | Hayır | Hayır |
> | yerlerini | Hayır | Hayır |
> | konumlar/işlemler | Hayır | Hayır |
> | işlemler | Hayır | Hayır |

## <a name="microsoftvisualstudio"></a>Microsoft. VisualStudio

> [!IMPORTANT]
> Azure DevOps aboneliğini değiştirmek için, bkz. [faturalandırma için kullanılan Azure aboneliğini değiştirme](/azure/devops/organizations/billing/change-azure-subscription?toc=/azure/azure-resource-manager/toc.json).

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | account | Hayır | Hayır |
> | Hesap/uzantı | Hayır | Hayır |
> | hesap/proje | Hayır | Hayır |
> | checknameavaılabılıty | Hayır | Hayır |
> | işlemler | Hayır | Hayır |

## <a name="microsoftvmware"></a>Microsoft. VMware

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | arczones | Hayır | Hayır |
> | yerlerini | Hayır | Hayır |
> | konumlar/operationdurumlarının | Hayır | Hayır |
> | işlemler | Hayır | Hayır |
> | ResourcePools | Hayır | Hayır |
> | vCenter | Hayır | Hayır |
> | virtualmachines | Hayır | Hayır |
> | virtualmachinetemplates | Hayır | Hayır |
> | virtualnetworks | Hayır | Hayır |

## <a name="microsoftvmwarecloudsimple"></a>Microsoft. Vmwarechoparlör basit

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | ayrılmış cloudnodes | Hayır | Hayır |
> | ayrılmış CloudService | Hayır | Hayır |
> | yerlerini | Hayır | Hayır |
> | konumlar/kullanılabilirlik | Hayır | Hayır |
> | konumlar/operationresults | Hayır | Hayır |
> | konumlar/privatebulutlar | Hayır | Hayır |
> | konumlar/privatebulutlar/ResourcePools | Hayır | Hayır |
> | konumlar/privatebulutlar/virtualmachinetemplates | Hayır | Hayır |
> | konumlar/privatebulutlar/virtualnetworks | Hayır | Hayır |
> | konumlar/kullanımlar | Hayır | Hayır |
> | işlemler | Hayır | Hayır |
> | virtualmachines | Hayır | Hayır |

## <a name="microsoftvnfmanager"></a>Microsoft. VnfManager

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | cihazlar | Hayır | Hayır |
> | yerlerini | Hayır | Hayır |
> | konumlar/operationdurumlarının | Hayır | Hayır |
> | işlemler | Hayır | Hayır |
> | vnfs | Hayır | Hayır |

## <a name="microsoftvsonline"></a>Microsoft. VSOnline

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | accounts | Hayır | Hayır |
> | işlemler | Hayır | Hayır |
> | Planlama | Hayır | Hayır |
> | registeredsubscriptions | Hayır | Hayır |

## <a name="microsoftweb"></a>Microsoft. Web

> [!IMPORTANT]
> Bkz. [App Service taşıma Kılavuzu](./move-limitations/app-service-move-limitations.md).

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | availableyığınları | Hayır | Hayır |
> | billingmeters | Hayır | Hayır |
> | sertifikalar | Hayır | Evet |
> | checknameavaılabılıty | Hayır | Hayır |
> | connectiongateway 'ler | Yes | Yes |
> | bağlantının | Yes | Yes |
> | customapsıs | Yes | Yes |
> | silinmi siteleri | Hayır | Hayır |
> | deploymentlocations | Hayır | Hayır |
> | coğrafi bölgeler | Hayır | Hayır |
> | hostingenvironments | Hayır | Hayır |
> | hostingenvironments/eventgridfilters | Hayır | Hayır |
> | hostingenvironments/multirolepools | Hayır | Hayır |
> | hostingenvironments/workerpools | Hayır | Hayır |
> | ıhostingenvironmentnameavailable | Hayır | Hayır |
> | ıhostnameavailable | Hayır | Hayır |
> | isusernameavailable | Hayır | Hayır |
> | kubeortamları | Yes | Yes |
> | listsitesassignedtohostname | Hayır | Hayır |
> | yerlerini | Hayır | Hayır |
> | konumlar/apioperations | Hayır | Hayır |
> | konumlar/connectiongatewayyüklemelerini | Hayır | Hayır |
> | konumlar/deletedsites | Hayır | Hayır |
> | konumlar/deletevirtualnetworkoralt ağları | Hayır | Hayır |
> | konumlar/extractapidefinitionfromwsdl | Hayır | Hayır |
> | konumlar/getnetworkpolicies | Hayır | Hayır |
> | konumlar/listwsdlınterfaces | Hayır | Hayır |
> | konumlar/managedap | Hayır | Hayır |
> | konumlar/operationresults | Hayır | Hayır |
> | konumlar/işlemler | Hayır | Hayır |
> | konumlar/çalışma zamanları | Hayır | Hayır |
> | işlemler | Hayır | Hayır |
> | publishingusers | Hayır | Hayır |
> | Öneriler | Hayır | Hayır |
> | resourcehealthmetadata | Hayır | Hayır |
> | zamanları | Hayır | Hayır |
> | serverfarms | Yes | Yes |
> | sunucugrupları/eventgridfilters | Hayır | Hayır |
> | Siteler | Yes | Yes |
> | siteler/eventgridfilters | Hayır | Hayır |
> | siteler/hostnamebindings | Hayır | Hayır |
> | Sites/networkconfig | Hayır | Hayır |
> | siteler/premieraddons | Yes | Yes |
> | siteler/yuvalar | Yes | Yes |
> | siteler/yuvalar/eventgridfilters | Hayır | Hayır |
> | siteler/yuvalar/hostnamebindings | Hayır | Hayır |
> | siteler/yuvalar/networkconfig | Hayır | Hayır |
> | sourcecontrols | Hayır | Hayır |
> | staticsites | Hayır | Hayır |
> | doğrulamalısınız | Hayır | Hayır |
> | verifyhostingenvironmentvnet | Hayır | Hayır |

## <a name="microsoftwindowsesu"></a>Microsoft. WindowsESU

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | yerlerini | Hayır | Hayır |
> | konumlar/operationdurumlarının | Hayır | Hayır |
> | çoğulactivationkeys | Hayır | Hayır |
> | işlemler | Hayır | Hayır |

## <a name="microsoftwindowsiot"></a>Microsoft. Windowsıot

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | deviceservices | Hayır | Hayır |
> | işlemler | Hayır | Hayır |

## <a name="microsoftworkloadbuilder"></a>Microsoft. WorkloadBuilder

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | yerlerini | Hayır | Hayır |
> | konumlar/operationdurumlarının | Hayır | Hayır |
> | işlemler | Hayır | Hayır |
> | lerine | Hayır | Hayır |

## <a name="microsoftworkloadmonitor"></a>Microsoft. WorkloadMonitor

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | bileşenleri | Hayır | Hayır |
> | componentssummary | Hayır | Hayır |
> | izleme örnekleri | Hayır | Hayır |
> | izleme ınstancessummary | Hayır | Hayır |
> | monitörün | Hayır | Hayır |
> | notificationsettings | Hayır | Hayır |
> | işlemler | Hayır | Hayır |

## <a name="third-party-services"></a>Üçüncü taraf hizmetleri

Üçüncü taraf hizmetler şu anda taşıma işlemini desteklemiyor.

## <a name="next-steps"></a>Sonraki adımlar
Kaynakları taşıma komutları için bkz. [kaynakları yeni kaynak grubuna veya aboneliğe taşıma](move-resource-group-and-subscription.md).

Aynı verileri bir virgülle ayrılmış değerler dosyası ile almak için [move-support-resources.csv](https://github.com/tfitzmac/resource-capabilities/blob/master/move-support-resources.csv)indirin.
