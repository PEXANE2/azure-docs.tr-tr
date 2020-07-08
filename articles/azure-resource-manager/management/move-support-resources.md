---
title: Kaynak türüne göre işlem desteğini taşıma
description: Yeni bir kaynak grubuna veya aboneliğe taşınabilecek Azure Kaynak türlerini listeler.
ms.topic: conceptual
ms.date: 07/06/2020
ms.openlocfilehash: 7f241e12200101e2f8f9efa7cf31e4483b2d4229
ms.sourcegitcommit: e132633b9c3a53b3ead101ea2711570e60d67b83
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/07/2020
ms.locfileid: "86044541"
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
> - [MICROSOFT. APPSERVICE](#microsoftappservice)
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
> - [MICROSOFT. BIZTALKSERVICES](#microsoftbiztalkservices)
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
> - [Microsoft. ConnectedCache](#microsoftconnectedcache)
> - [Microsoft. tüketim](#microsoftconsumption)
> - [Microsoft. Containerınstance](#microsoftcontainerinstance)
> - [Microsoft. ContainerRegistry](#microsoftcontainerregistry)
> - [Microsoft. ContainerService](#microsoftcontainerservice)
> - [MICROSOFT. CONTENTMODERATÖR](#microsoftcontentmoderator)
> - [MICROSOFT. CORTANAANALIZI](#microsoftcortanaanalytics)
> - [Microsoft. CostManagement](#microsoftcostmanagement)
> - [Microsoft. CostManagementExports](#microsoftcostmanagementexports)
> - [MICROSOFT. CUSTOMERıNSIGHTS](#microsoftcustomerinsights)
> - [Microsoft. Customerkasası](#microsoftcustomerlockbox)
> - [Microsoft. CustomProviders](#microsoftcustomproviders)
> - [Microsoft. DataBox](#microsoftdatabox)
> - [Microsoft. DataBoxEdge](#microsoftdataboxedge)
> - [Microsoft. Databricks](#microsoftdatabricks)
> - [Microsoft. DataCatalog](#microsoftdatacatalog)
> - [MICROSOFT. VERI bağlama](#microsoftdataconnect)
> - [MICROSOFT. DATAEXCHANGE](#microsoftdataexchange)
> - [Microsoft. DataFactory](#microsoftdatafactory)
> - [MICROSOFT. DATALAKE](#microsoftdatalake)
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
> - [Microsoft. Devices](#microsoftdevices)
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
> - [MICROSOFT. GENOMIKS](#microsoftgenomics)
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
> - [MICROSOFT. LOCATIONBASEDSERVICES](#microsoftlocationbasedservices)
> - [MICROSOFT. ISTEMCIDEKI LOCATIONSERVICES](#microsoftlocationservices)
> - [Microsoft. Logic](#microsoftlogic)
> - [Microsoft. Machinöğrenim](#microsoftmachinelearning)
> - [MICROSOFT. MACHINELEARNINGCOMPUTE](#microsoftmachinelearningcompute)
> - [MICROSOFT. MACHINELEARNINGEXPERIMENTATION](#microsoftmachinelearningexperimentation)
> - [MICROSOFT. MACHINELEARNINGMODELMANAGEMENT](#microsoftmachinelearningmodelmanagement)
> - [Microsoft.MachineLearningServices](#microsoftmachinelearningservices)
> - [Microsoft. Maintenance](#microsoftmaintenance)
> - [Microsoft. Managedıdentity](#microsoftmanagedidentity)
> - [MICROSOFT. MANAGEDNETWORK](#microsoftmanagednetwork)
> - [Microsoft. ManagedServices](#microsoftmanagedservices)
> - [Microsoft. Management](#microsoftmanagement)
> - [Microsoft. Maps](#microsoftmaps)
> - [Microsoft. Market](#microsoftmarketplace)
> - [Microsoft. MarketplaceApps](#microsoftmarketplaceapps)
> - [Microsoft. Marketplacesıralaması](#microsoftmarketplaceordering)
> - [Microsoft. Media](#microsoftmedia)
> - [MICROSOFT. MICROSERVICES4SPRING](#microsoftmicroservices4spring)
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
> - [Microsoft.PolicyInsights](#microsoftpolicyinsights)
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
> - [MICROSOFT. SUNUCUYÖNETIMI](#microsoftservermanagement)
> - [Microsoft.ServiceBus](#microsoftservicebus)
> - [Microsoft. ServiceFabric](#microsoftservicefabric)
> - [Microsoft. Servicefabrickafesi](#microsoftservicefabricmesh)
> - [MICROSOFT. SERVISLERE](#microsoftservices)
> - [Microsoft. SignalRService](#microsoftsignalrservice)
> - [Microsoft. SoftwarePlan](#microsoftsoftwareplan)
> - [Microsoft. Solutions](#microsoftsolutions)
> - [Microsoft.Sql](#microsoftsql)
> - [Microsoft. SqlVirtualMachine](#microsoftsqlvirtualmachine)
> - [Microsoft.Storage](#microsoftstorage)
> - [MICROSOFT. STORAGECACHE](#microsoftstoragecache)
> - [Microsoft. Storagessync](#microsoftstoragesync)
> - [MICROSOFT. STORAGESYNCDEV](#microsoftstoragesyncdev)
> - [MICROSOFT. STORAGESYNCıNT](#microsoftstoragesyncint)
> - [Microsoft. StorSimple](#microsoftstorsimple)
> - [Microsoft. StreamAnalytics](#microsoftstreamanalytics)
> - [MICROSOFT. STREAMANALYTICSEXPLORER](#microsoftstreamanalyticsexplorer)
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
> | azureadölçümlerini | Hayır | Hayır |
> | diagnosticsettings | Hayır | Hayır |
> | diagnosticsettingscategories | Hayır | Hayır |
> | işlemler | Hayır | Hayır |
> | privatelinkforazuread | Hayır | Hayır |
> | Kira | Hayır | Hayır |

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
> | actionrules | Hayır | Hayır |
> | alerts | Hayır | Hayır |
> | alertslist | Hayır | Hayır |
> | alertsmetadata | Hayır | Hayır |
> | alertssummary | Hayır | Hayır |
> | alertssummarylist | Hayır | Hayır |
> | işlemler | Hayır | Hayır |
> | smartdetectoralertrules | Hayır | Hayır |
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
> | larý | Hayır | Hayır |

## <a name="microsoftapimanagement"></a>Microsoft. Apimanane

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | checkfeedbackrequired | Hayır | Hayır |
> | checknameavaılabılıty | Hayır | Hayır |
> | checkservicenameavaılabılıty | Hayır | Hayır |
> | işlemler | Hayır | Hayır |
> | reportfeedback | Hayır | Hayır |
> | hizmet | Evet | Evet |
> | validateservicename | Hayır | Hayır |

> [!IMPORTANT]
> Tüketim SKU 'suna ayarlanmış bir API Management hizmeti taşınamaz.

## <a name="microsoftappconfiguration"></a>Microsoft. AppConfiguration

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | checknameavaılabılıty | Hayır | Hayır |
> | configurationmağazaların | Hayır | Hayır |
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
> | yay | Hayır | Hayır |
> | yay/uygulamalar | Hayır | Hayır |
> | yay/uygulamalar/dağıtımlar | Hayır | Hayır |

## <a name="microsoftappservice"></a>MICROSOFT. APPSERVICE

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | apiapps | Hayır | Hayır |
> | appdentities | Hayır | Hayır |
> | geçidinin | Hayır | Hayır |

> [!IMPORTANT]
> Bkz. [App Service taşıma Kılavuzu](./move-limitations/app-service-move-limitations.md).

## <a name="microsoftattestation"></a>Microsoft. kanıtlama

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | attestationproviders | Hayır | Hayır |
> | defaultproviders | Hayır | Hayır |
> | yerlerini | Hayır | Hayır |
> | konumlar/defaultProvider 'ın | Hayır | Hayır |
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

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | automationaccounts | Evet | Evet |
> | automationaccounts/Configurations | Hayır | Hayır |
> | automationaccounts/Jobs | Hayır | Hayır |
> | automationaccounts/privateendpointconnectionproxy 'leri | Hayır | Hayır |
> | automationaccounts/privateendpointconnections | Hayır | Hayır |
> | automationaccounts/privatelinkresources | Hayır | Hayır |
> | automationaccounts/runbook 'lar | Hayır | Hayır |
> | automationaccounts/softwareupdateconfigurations | Hayır | Hayır |
> | automationaccounts/Web kancaları | Hayır | Hayır |
> | işlemler | Hayır | Hayır |

> [!IMPORTANT]
> Runbook 'lar Otomasyon hesabıyla aynı kaynak grubunda bulunmalıdır.
>
> Daha fazla bilgi için bkz. [Azure Otomasyonu hesabınızı başka bir aboneliğe taşıma](../../automation/how-to/move-account.md?toc=/azure/azure-resource-manager/toc.json).

## <a name="microsoftavs"></a>Microsoft. AVS

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | yerlerini | Hayır | Hayır |
> | konumlar/checkquotaavaılabılıty | Hayır | Hayır |
> | konumlar/checktrialavailability | Hayır | Hayır |
> | işlemler | Hayır | Hayır |
> | privatebulutlar | Hayır | Hayır |
> | privatebulutlar/kümeler | Hayır | Hayır |

## <a name="microsoftazureactivedirectory"></a>Microsoft. AzureActiveDirectory

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | b2cdirectories | Evet | Evet |
> | b2ctenants | Hayır | Hayır |
> | checknameavaılabılıty | Hayır | Hayır |
> | guestkullanımlar | Evet | Evet |
> | işlemler | Hayır | Hayır |

## <a name="microsoftazuredata"></a>Microsoft. AzureData

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | veri denetleyicileri | Evet | Evet |
> | hybriddatayöneticileri | Evet | Evet |
> | işlemler | Hayır | Hayır |
> | postgresınstances | Evet | Evet |
> | SQLInstances | Evet | Evet |
> | sqlmanagedınstances | Evet | Evet |
> | sqlserverınstances | Evet | Evet |
> | sqlserverkayıtları | Evet | Evet |
> | sqlserverkayıtları/SQLServers | Hayır | Hayır |

## <a name="microsoftazurestack"></a>Microsoft. AzureStack

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | cloudmanifestfiles | Hayır | Hayır |
> | edgesubscriptions | Hayır | Hayır |
> | işlemler | Hayır | Hayır |
> | kayıtlarında | Hayır | Hayır |
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
> | batchaccounts | Evet | Evet |
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
> | billingaccounts/departmanlar/billingpermissions | Hayır | Hayır |
> | billingaccounts/departmanlar/billingroleatamaları | Hayır | Hayır |
> | billingaccounts/departmanlar/billingroledefinitions | Hayır | Hayır |
> | billingaccounts/KayıtSayısı | Hayır | Hayır |
> | billingaccounts/kayıthesapsayısı/billingpermissions | Hayır | Hayır |
> | billingaccounts/KayıtSayısı/billingroleatamaları | Hayır | Hayır |
> | billingaccounts/KayıtSayısı/billingroledefinitions | Hayır | Hayır |
> | billingaccounts/faturalar | Hayır | Hayır |
> | billingaccounts/faturalar/işlemler | Hayır | Hayır |
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

## <a name="microsoftbiztalkservices"></a>MICROSOFT. BIZTALKSERVICES

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
> | botservices | Hayır | Hayır |
> | botservices/kanallar | Hayır | Hayır |
> | botservices/Connections | Hayır | Hayır |
> | checknameavaılabılıty | Hayır | Hayır |
> | listauthserviceproviders | Hayır | Hayır |
> | işlemler | Hayır | Hayır |

## <a name="microsoftcache"></a>Microsoft. Cache

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | checknameavaılabılıty | Hayır | Hayır |
> | yerlerini | Hayır | Hayır |
> | konumlar/operationresults | Hayır | Hayır |
> | konumlar/operationsstatus | Hayır | Hayır |
> | işlemler | Hayır | Hayır |
> | Redis | Evet | Evet |
> | redsıs/eventgridfilters | Hayır | Hayır |
> | redsıs/privatelinkresources | Hayır | Hayır |
> | redisenterprise | Evet | Evet |

> [!IMPORTANT]
> Redsıs örneği için Azure önbelleği bir sanal ağla yapılandırıldıysa, örnek farklı bir aboneliğe taşınamaz. Bkz. [ağ taşıma sınırlamaları](./move-limitations/networking-move-limitations.md).

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
> | cdnwebapplicationfirewallpolicies | Hayır | Hayır |
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
> | lerinize | Evet | Evet |
> | Profiller/uç noktalar | Hayır | Hayır |
> | Profiller/uç noktalar/customdomains | Hayır | Hayır |
> | Profiller/uç noktalar/origingroups | Hayır | Hayır |
> | Profiller/uç noktalar/kaynaklar | Hayır | Hayır |
> | validatearaştırması | Hayır | Hayır |

## <a name="microsoftcertificateregistration"></a>Microsoft. CertificateRegistration

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | sertifikadüzenleri | Evet | Evet |
> | certificateorders/Certificates | Hayır | Hayır |
> | işlemler | Hayır | Hayır |
> | validatecertificateregistrationınformation | Hayır | Hayır |

> [!IMPORTANT]
> Bkz. [App Service taşıma Kılavuzu](./move-limitations/app-service-move-limitations.md).

## <a name="microsoftchangeanalysis"></a>Microsoft. ChangeAnalysis

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | işlemler | Hayır | Hayır |

## <a name="microsoftclassiccompute"></a>Microsoft.ClassicCompute

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | özellikler | Hayır | Hayır |
> | checkdomainnameavaılabılıty | Hayır | Hayır |
> | DomainNames | Hayır | Hayır |
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
> | virtualmachines | Hayır | Hayır |
> | virtualmachines/diagnosticsettings | Hayır | Hayır |
> | virtualmachines/MetricDefinitions | Hayır | Hayır |
> | virtualmachines/ölçümler | Hayır | Hayır |

> [!IMPORTANT]
> Bkz. [klasik dağıtım taşıma Kılavuzu](./move-limitations/classic-model-move-limitations.md). Klasik dağıtım kaynakları, bu senaryoya özel bir işlemle abonelikler arasında taşınabilir.

## <a name="microsoftclassicinfrastructuremigrate"></a>Microsoft. ClassicInfrastructureMigrate

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | classicinfrastructureresources | Hayır | Hayır |

> [!IMPORTANT]
> Bkz. [klasik dağıtım taşıma Kılavuzu](./move-limitations/classic-model-move-limitations.md). Klasik dağıtım kaynakları, bu senaryoya özel bir işlemle abonelikler arasında taşınabilir.

## <a name="microsoftclassicnetwork"></a>Microsoft. ClassicNetwork

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

> [!IMPORTANT]
> Bkz. [klasik dağıtım taşıma Kılavuzu](./move-limitations/classic-model-move-limitations.md). Klasik dağıtım kaynakları, bu senaryoya özel bir işlemle abonelikler arasında taşınabilir.

## <a name="microsoftclassicstorage"></a>Microsoft. ClassicStorage

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
> | storageaccounts | Hayır | Hayır |
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

> [!IMPORTANT]
> Bkz. [klasik dağıtım taşıma Kılavuzu](./move-limitations/classic-model-move-limitations.md). Klasik dağıtım kaynakları, bu senaryoya özel bir işlemle abonelikler arasında taşınabilir.

## <a name="microsoftclassicsubscription"></a>Microsoft. ClassicSubscription

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | işlemler | Hayır | Hayır |

> [!IMPORTANT]
> Bkz. [klasik dağıtım taşıma Kılavuzu](./move-limitations/classic-model-move-limitations.md). Klasik dağıtım kaynakları, bu senaryoya özel bir işlemle abonelikler arasında taşınabilir.

## <a name="microsoftcognitiveservices"></a>Microsoft. Biliveservices

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | accounts | Hayır | Hayır |
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

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | availabilitysets | Evet | Evet |
> | cloudservices | Hayır | Hayır |
> | diskeriþler | Hayır | Hayır |
> | diskencryptionsets | Hayır | Hayır |
> | disklerinden | Evet | Evet |
> | Galeriler | Hayır | Hayır |
> | Galeriler/görüntüler | Hayır | Hayır |
> | Galeriler/resimler/sürümler | Hayır | Hayır |
> | hostgroups | Hayır | Hayır |
> | hostgroups/konaklar | Hayır | Hayır |
> | images | Evet | Evet |
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
> | proximityplacementgroups | Evet | Evet |
> | restorepointcollections | Hayır | Hayır |
> | restorepointcollections/restorepoints | Hayır | Hayır |
> | sharedvmextensions | Hayır | Hayır |
> | sharedvmımages | Hayır | Hayır |
> | sharedvmımages/sürümler | Hayır | Hayır |
> | anlık görüntüler | Evet | Evet |
> | sshpublickeys | Hayır | Hayır |
> | swiftizin | Hayır | Hayır |
> | virtualmachines | Evet | Evet |
> | virtualmachines/uzantıları | Evet | Evet |
> | virtualmachines/MetricDefinitions | Hayır | Hayır |
> | virtualmachines/runcommands | Evet | Evet |
> | virtualmachinescalesets | Evet | Evet |
> | virtualmachinescalesets/uzantılar | Hayır | Hayır |
> | virtualmachinescalesets/NetworkInterfaces | Hayır | Hayır |
> | virtualmachinescalesets/publicıpaddresses | Hayır | Hayır |
> | virtualmachinescalesets/virtualmachines | Hayır | Hayır |
> | virtualmachinescalesets/virtualmachines/NetworkInterfaces | Hayır | Hayır |

> [!IMPORTANT]
> Bkz. [sanal makine taşıma Kılavuzu](./move-limitations/virtual-machines-move-limitations.md).

## <a name="microsoftconnectedcache"></a>Microsoft. ConnectedCache

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | önbellekenodes | Hayır | Hayır |

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
> | etiketler | Hayır | Hayır |
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
> | kayıt | Evet | Evet |
> | kayıt defterleri/agentpools | Hayır | Hayır |
> | kayıt defterleri/agentpools/listqueuestatus | Hayır | Hayır |
> | kayıt defterleri/derlemeler | Hayır | Hayır |
> | kayıt defterleri/derlemeler/iptal | Hayır | Hayır |
> | kayıt defterleri/derlemeler/getloglink | Hayır | Hayır |
> | kayıt defterleri/BuildTasks | Hayır | Hayır |
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
> | kayıt defterleri/çoğaltmalar | Hayır | Hayır |
> | kayıt defterleri/çalıştırmalar | Hayır | Hayır |
> | kayıt defterleri/çalıştırmalar/iptal | Hayır | Hayır |
> | kayıt defterleri/çalıştırmalar/listlogsasurl 'si | Hayır | Hayır |
> | kayıt defterleri/schedulerun | Hayır | Hayır |
> | kayıt defterleri/kapsameşlemler | Hayır | Hayır |
> | kayıt defterleri/taskçalıştırmaları | Hayır | Hayır |
> | kayıt defterleri/taskçalıştırmaları/ListDetails | Hayır | Hayır |
> | kayıt defterleri/görevler | Hayır | Hayır |
> | kayıt defterleri/görevler/ListDetails | Hayır | Hayır |
> | kayıt defterleri/belirteçler | Hayır | Hayır |
> | kayıt defterleri/updatepolicies | Hayır | Hayır |
> | kayıt defterleri/Web kancaları | Hayır | Hayır |
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

## <a name="microsoftcontentmoderator"></a>MICROSOFT. CONTENTMODERATÖR

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | uygulamalar | Hayır | Hayır |

## <a name="microsoftcortanaanalytics"></a>MICROSOFT. CORTANAANALIZI

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
> | bağlayıcılar | Hayır | Hayır |
> | costallocationrules | Hayır | Hayır |
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
> | tahmin | Hayır | Hayır |
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

## <a name="microsoftcustomerinsights"></a>MICROSOFT. CUSTOMERıNSIGHTS

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
> | resourceproviders | Hayır | Hayır |

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
> | databoxedgedevices | Evet | Evet |
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
> | larına | Hayır | Hayır |
> | checknameavaılabılıty | Hayır | Hayır |
> | veri katalogları | Hayır | Hayır |
> | yerlerini | Hayır | Hayır |
> | konumlar/işler | Hayır | Hayır |
> | konumlar/operationresults | Hayır | Hayır |
> | işlemler | Hayır | Hayır |

## <a name="microsoftdataconnect"></a>MICROSOFT. VERI bağlama

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | connectionyöneticileri | Hayır | Hayır |

## <a name="microsoftdataexchange"></a>MICROSOFT. DATAEXCHANGE

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
> | veri fabrikaları | Hayır | Hayır |
> | DataFactory/diagnosticsettings | Hayır | Hayır |
> | DataFactory/MetricDefinitions | Hayır | Hayır |
> | datafactoryschema | Hayır | Hayır |
> | larının | Evet | Evet |
> | Fabrika/tümleştirme çalışma zamanları | Hayır | Hayır |
> | yerlerini | Hayır | Hayır |
> | konumlar/configurefactoryrepo | Hayır | Hayır |
> | konumlar/getfeaturevalue | Hayır | Hayır |
> | işlemler | Hayır | Hayır |

## <a name="microsoftdatalake"></a>MICROSOFT. DATALAKE

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | datalakeaccounts | Hayır | Hayır |

## <a name="microsoftdatalakeanalytics"></a>Microsoft. DataLakeAnalytics

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | accounts | Hayır | Hayır |
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
> | accounts | Hayır | Hayır |
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
> | resourceoperationgatedenetleyicileri | Hayır | Hayır |

## <a name="microsoftdatashare"></a>Microsoft. DataShare

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | accounts | Evet | Evet |
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
> | larý | Evet | Evet |
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
> | larý | Evet | Evet |
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
> | larý | Evet | Evet |
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
> | serversv2 | Evet | Evet |
> | teksunucular | Evet | Evet |

## <a name="microsoftdeploymentmanager"></a>Microsoft. DeploymentManager

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | artifactsources | Hayır | Hayır |
> | operationresults | Hayır | Hayır |
> | işlemler | Hayır | Hayır |
> | piyasaya çıkarma | Hayır | Hayır |
> | servicetopolojileri | Hayır | Hayır |
> | servicetopolojileri/hizmetler | Hayır | Hayır |
> | servicetopolojileri/hizmetler/serviceunits | Hayır | Hayır |
> | adımlar | Hayır | Hayır |

## <a name="microsoftdesktopvirtualization"></a>Microsoft. DesktopVirtualization

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | applicationgroups | Hayır | Hayır |
> | applicationgroups/uygulamalar | Hayır | Hayır |
> | applicationgroups/masaüstleri | Hayır | Hayır |
> | applicationgroups/startmenuıtems | Hayır | Hayır |
> | Ana bilgisayar havuzları | Hayır | Hayır |
> | hostpools/oturumkonakları | Hayır | Hayır |
> | hostpools/sessionkonakları/usersessions | Hayır | Hayır |
> | hosthavuzlar/usersessions | Hayır | Hayır |
> | işlemler | Hayır | Hayır |
> | çalışma alanı | Hayır | Hayır |

## <a name="microsoftdevices"></a>Microsoft. Devices

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | checknameavaılabılıty | Hayır | Hayır |
> | checkprovisioningservicenameavaılabılıty | Hayır | Hayır |
> | elaun havuzları | Evet | Evet |
> | elaun havuzları/ıothubkiracılar | Evet | Evet |
> | ıothubs | Evet | Evet |
> | ıothubs/eventgridfilters | Hayır | Hayır |
> | ıothubs/SecuritySettings | Hayır | Hayır |
> | operationresults | Hayır | Hayır |
> | işlemler | Hayır | Hayır |
> | provisioningservices | Evet | Evet |
> | vardır | Hayır | Hayır |

## <a name="microsoftdevops"></a>Microsoft. DevOps

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | düzenler | Hayır | Hayır |

## <a name="microsoftdevspaces"></a>Microsoft. DevSpaces

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | denetleyiciler | Hayır | Hayır |
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
> | larda | Hayır | Hayır |
> | Laboratuvarlar/ortamlar | Hayır | Hayır |
> | Labs/servicerunanlar | Hayır | Hayır |
> | Labs/virtualmachines | Hayır | Hayır |
> | yerlerini | Hayır | Hayır |
> | konumlar/işlemler | Hayır | Hayır |
> | işlemler | Hayır | Hayır |
> | cağını | Hayır | Hayır |

## <a name="microsoftdigitaltwins"></a>Microsoft. DigitalTwins

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | digitaltwınsınstances | Hayır | Hayır |
> | digitaltwınsınstances/endpoints | Hayır | Hayır |
> | digitaltwınsınstances/operationresults | Hayır | Hayır |
> | yerlerini | Hayır | Hayır |
> | konumlar/checknameavaılabılıty | Hayır | Hayır |
> | işlemler | Hayır | Hayır |

## <a name="microsoftdocumentdb"></a>Microsoft.DocumentDB

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | databaseaccountnames | Hayır | Hayır |
> | veritabanı hesapları | Evet | Evet |
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
> | etki alanları | Evet | Evet |
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
> | services | Hayır | Hayır |

## <a name="microsofteventgrid"></a>Microsoft. EventGrid

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | etki alanları | Hayır | Hayır |
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
> | partnernamespaces | Hayır | Hayır |
> | partnernamespaces/eventchannels | Hayır | Hayır |
> | iş ortağı kayıtları | Hayır | Hayır |
> | iş ortağı konuları | Hayır | Hayır |
> | iş ortağı konuları/eventabonelikleri | Hayır | Hayır |
> | Sistem konuları | Hayır | Hayır |
> | Sistem konuları/eventabonelikler | Hayır | Hayır |
> | konuları | Hayır | Hayır |
> | topictypes | Hayır | Hayır |

## <a name="microsofteventhub"></a>Microsoft. EventHub

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | availableclusterregion | Hayır | Hayır |
> | checknameavaılabılıty | Hayır | Hayır |
> | checknamespaceavailability | Hayır | Hayır |
> | leriniz | Hayır | Hayır |
> | yerlerini | Hayır | Hayır |
> | konumlar/deletevirtualnetworkoralt ağları | Hayır | Hayır |
> | öznitelikleri | Hayır | Hayır |
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
> | öznitelikleri | Hayır | Hayır |

## <a name="microsoftfeatures"></a>Microsoft. Features

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | özellik sağlayıcıları | Hayır | Hayır |
> | özellikler | Hayır | Hayır |
> | işlemler | Hayır | Hayır |
> | sağlayıcılarla | Hayır | Hayır |
> | subscriptionfeatureregistrations | Hayır | Hayır |

## <a name="microsoftgenomics"></a>MICROSOFT. GENOMIKS

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
> | sapizleyicileri | Hayır | Hayır |

## <a name="microsofthardwaresecuritymodules"></a>Microsoft. HardwareSecurityModules

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | ayrılmış atedhsms | Hayır | Hayır |
> | yerlerini | Hayır | Hayır |
> | işlemler | Hayır | Hayır |

## <a name="microsofthdinsight"></a>Microsoft. HDInsight

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | leriniz | Evet | Evet |
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

> [!IMPORTANT]
> HDInsight kümelerini yeni bir aboneliğe veya kaynak grubuna taşıyabilirsiniz. Bununla birlikte, HDInsight kümesiyle bağlantılı ağ kaynakları (sanal ağ, NIC veya yük dengeleyici gibi) için abonelikler arasında geçiş yapamazsınız. Ayrıca, küme için bir sanal makineye bağlı bir NIC 'ye yeni bir kaynak grubuna taşıyamazsınız.
>
> HDInsight kümesini yeni bir aboneliğe taşırken, önce diğer kaynakları (depolama hesabı gibi) taşıyın. Ardından, HDInsight kümesini kendi kendine taşıyın.

## <a name="microsofthealthcareapis"></a>Microsoft. Healthgelişme API 'leri

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | checknameavaılabılıty | Hayır | Hayır |
> | yerlerini | Hayır | Hayır |
> | konumlar/operationresults | Hayır | Hayır |
> | işlemler | Hayır | Hayır |
> | services | Hayır | Hayır |
> | Hizmetler/privateendpointconnections | Hayır | Hayır |
> | Hizmetler/privatelinkresources | Hayır | Hayır |

## <a name="microsofthybridcompute"></a>Microsoft. HybridCompute

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | yerlerini | Hayır | Hayır |
> | konumlar/operationresults | Hayır | Hayır |
> | konumlar/OperationStatus | Hayır | Hayır |
> | larla | Hayır | Hayır |
> | makineler/uzantılar | Evet | Evet |
> | işlemler | Hayır | Hayır |

## <a name="microsofthybriddata"></a>Microsoft. HybridData

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | veri yöneticileri | Hayır | Hayır |
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
> | Çizelge | Evet | Evet |
> | yerlerini | Hayır | Hayır |
> | konumlar/operationresults | Hayır | Hayır |
> | işlemler | Hayır | Hayır |

## <a name="microsoftinsights"></a>Microsoft. Insights

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | actiongroups | Hayır | Hayır |
> | activitylogalerts | Hayır | Hayır |
> | alertrules | Evet | Evet |
> | autoscalesettings | Evet | Evet |
> | temel | Hayır | Hayır |
> | calculatebaseline | Hayır | Hayır |
> | bileşenleri | Evet | Evet |
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
> | scheduledqueryrules | Evet | Evet |
> | topology | Hayır | Hayır |
> | işlemler | Hayır | Hayır |
> | vminsightsonboardingstatuses | Hayır | Hayır |
> | Web testleri | Evet | Evet |
> | Web testleri/gettestresultfile | Hayır | Hayır |
> | çalışma kitapları | Hayır | Hayır |
> | workbooktemplates | Hayır | Hayır |

> [!IMPORTANT]
> Yeni aboneliğe taşınmasının [abonelik kotalarını](azure-subscription-service-limits.md#azure-monitor-limits)aşmadığından emin olun.

## <a name="microsoftiotcentral"></a>Microsoft. ıotcentral

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | apptemplates | Hayır | Hayır |
> | checknameavaılabılıty | Hayır | Hayır |
> | checksubdomainavaılabılıty | Hayır | Hayır |
> | ıotapps | Hayır | Hayır |
> | işlemler | Hayır | Hayır |

## <a name="microsoftiotspaces"></a>Microsoft. ıotspaces

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | checknameavaılabılıty | Hayır | Hayır |
> | çıkarılamıyor | Hayır | Hayır |
> | işlemler | Hayır | Hayır |

## <a name="microsoftkeyvault"></a>Microsoft. Keykasası

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | checknameavaılabılıty | Hayır | Hayır |
> | silinkaults | Hayır | Hayır |
> | hsmpools | Evet | Evet |
> | yerlerini | Hayır | Hayır |
> | konumlar/silinmeler | Hayır | Hayır |
> | konumlar/deletevirtualnetworkoralt ağları | Hayır | Hayır |
> | konumlar/operationresults | Hayır | Hayır |
> | managedhsms | Hayır | Hayır |
> | işlemler | Hayır | Hayır |
> | kasaları | Evet | Evet |
> | kasa/erişim ilkeleri | Hayır | Hayır |
> | kasa/eventgridfilters | Hayır | Hayır |
> | kasa/gizlilikler | Hayır | Hayır |

> [!IMPORTANT]
> Disk şifrelemesi için kullanılan anahtar kasaları aynı abonelikte veya abonelikler arasında bir kaynak grubuna taşınamaz.

## <a name="microsoftkubernetes"></a>Microsoft. Kubernetes

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | connectedkümeler | Hayır | Hayır |
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
> | leriniz | Hayır | Hayır |
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

## <a name="microsoftlocationbasedservices"></a>MICROSOFT. LOCATIONBASEDSERVICES

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | accounts | Hayır | Hayır |

## <a name="microsoftlocationservices"></a>MICROSOFT. ISTEMCIDEKI LOCATIONSERVICES

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | accounts | Hayır | Hayır |

## <a name="microsoftlogic"></a>Microsoft. Logic

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | hostingenvironments | Hayır | Hayır |
> | Tümleştirme hesapları | Evet | Evet |
> | ıntegrationserviceortamortamları | Hayır | Hayır |
> | ıntegrationserviceortamortamları/managedap | Hayır | Hayır |
> | ısotedenvironments | Hayır | Hayır |
> | yerlerini | Hayır | Hayır |
> | konumlar/iş akışları | Hayır | Hayır |
> | işlemler | Hayır | Hayır |
> | sürdürülen | Evet | Evet |

## <a name="microsoftmachinelearning"></a>Microsoft. Machinöğrenim

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | commitmentplanlar | Hayır | Hayır |
> | yerlerini | Hayır | Hayır |
> | konumlar/işlemler | Hayır | Hayır |
> | konumlar/operationsstatus | Hayır | Hayır |
> | işlemler | Hayır | Hayır |
> | Hizmetleri | Hayır | Hayır |
> | çalışma alanı | Hayır | Hayır |

## <a name="microsoftmachinelearningcompute"></a>MICROSOFT. MACHINELEARNINGCOMPUTE

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | operationalizationkümeleri | Hayır | Hayır |

## <a name="microsoftmachinelearningexperimentation"></a>MICROSOFT. MACHINELEARNINGEXPERIMENTATION

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | accounts | Hayır | Hayır |
> | hesaplar/çalışma alanları | Hayır | Hayır |
> | hesaplar/çalışma alanları/projeler | Hayır | Hayır |
> | teamaccounts | Hayır | Hayır |
> | teamaccounts/çalışma alanları | Hayır | Hayır |
> | teamaccounts/çalışma alanları/projeler | Hayır | Hayır |

## <a name="microsoftmachinelearningmodelmanagement"></a>MICROSOFT. MACHINELEARNINGMODELMANAGEMENT

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
> | maintenanceconfigurations | Hayır | Hayır |
> | güncelleştirmeler | Hayır | Hayır |

## <a name="microsoftmanagedidentity"></a>Microsoft. Managedıdentity

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | lerinizde | Hayır | Hayır |
> | işlemler | Hayır | Hayır |
> | userassignedıdentities | Hayır | Hayır |

## <a name="microsoftmanagednetwork"></a>MICROSOFT. MANAGEDNETWORK

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
> | accounts | Hayır | Hayır |
> | hesaplar/eventgridfilters | Hayır | Hayır |
> | hesaplar/privateatlases | Hayır | Hayır |
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
> | mediaservices | Hayır | Hayır |
> | mediaservices/accountfilters | Hayır | Hayır |
> | mediaservices/varlıklar | Hayır | Hayır |
> | mediaservices/varlıklar/assetfilters | Hayır | Hayır |
> | mediaservices/contentkeypolicies | Hayır | Hayır |
> | mediaservices/eventgridfilters | Hayır | Hayır |
> | mediaservices/liveeventoperations | Hayır | Hayır |
> | mediaservices/liveevents | Hayır | Hayır |
> | mediaservices/liveevents/liveçıktılar | Hayır | Hayır |
> | mediaservices/liveoutputoperations | Hayır | Hayır |
> | mediaservices/streamingendpointoperations | Hayır | Hayır |
> | mediaservices/streamingendpoints | Hayır | Hayır |
> | mediaservices/streamingkonumlandırıcı | Hayır | Hayır |
> | mediaservices/streamingpolicies | Hayır | Hayır |
> | mediaservices/dönüşümler | Hayır | Hayır |
> | mediaservices/dönüşümler/işler | Hayır | Hayır |
> | işlemler | Hayır | Hayır |

## <a name="microsoftmicroservices4spring"></a>MICROSOFT. MICROSERVICES4SPRING

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
> | remoterenderingaccounts | Hayır | Hayır |
> | spatialanchorsaccounts | Hayır | Hayır |

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
> | applicationsecuritygroups | Hayır | Hayır |
> | azurefirewallfqdntags | Hayır | Hayır |
> | azurefirewalls | Hayır | Hayır |
> | Savunma Konakları | Hayır | Hayır |
> | bgpservicecommunities | Hayır | Hayır |
> | checkfrontdoornameavailability | Hayır | Hayır |
> | checktrafficmanagernameavailability | Hayır | Hayır |
> | bağlantının | Hayır | Hayır |
> | ddoscustompolicies | Hayır | Hayır |
> | ddosprotectionplanlar | Hayır | Hayır |
> | dnsoperationsonuçları | Hayır | Hayır |
> | dnsoperationdurumları | Hayır | Hayır |
> | dnszones | Hayır | Hayır |
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
> | firewallpolicies | Hayır | Hayır |
> | frontdooroperationresults | Hayır | Hayır |
> | frontkapıların | Hayır | Hayır |
> | frontkapaklı 'lar/ön uçlar | Hayır | Hayır |
> | frontdoorwebapplicationfirewallmanagedrulesets | Hayır | Hayır |
> | frontdoorwebapplicationfirewallpolicies | Hayır | Hayır |
> | getdnsresourcereference | Hayır | Hayır |
> | ınternalnotify | Hayır | Hayır |
> | ipayırmalar | Hayır | Hayır |
> | ipgroups | Hayır | Hayır |
> | loadbalancers | Evet-temel SKU<br>Standart SKU yok | Evet-temel SKU<br>Standart SKU yok |
> | localnetworkgateway 'ler | Hayır | Hayır |
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
> | networkıntpolicies ilkeleri | Hayır | Hayır |
> | NetworkInterfaces | Hayır | Hayır |
> | networkprofiles | Hayır | Hayır |
> | networksecuritygroups | Hayır | Hayır |
> | networkwatchers | Evet | Hayır |
> | networkwatchers/connectionmonitörleri | Evet | Hayır |
> | networkwatchers/flowlogs | Evet | Hayır |
> | networkwatchers/pingkafesler | Evet | Hayır |
> | işlemler | Hayır | Hayır |
> | p2svpngateways | Hayır | Hayır |
> | privatednsoperationresults | Hayır | Hayır |
> | privatednsoperationstatuses | Hayır | Hayır |
> | privatednszones | Hayır | Hayır |
> | privatednszones/a | Hayır | Hayır |
> | privatednszones/aaaa | Hayır | Hayır |
> | privatednszones/tümü | Hayır | Hayır |
> | privatednszones/CNAME | Hayır | Hayır |
> | privatednszones/MX | Hayır | Hayır |
> | privatednszones/PTR | Hayır | Hayır |
> | privatednszones/SOA | Hayır | Hayır |
> | privatednszones/SRV | Hayır | Hayır |
> | privatednszones/txt | Hayır | Hayır |
> | privatednszones/virtualnetworklinks | Hayır | Hayır |
> | privatednszonesinternal | Hayır | Hayır |
> | privateendpointredirectmaps | Hayır | Hayır |
> | privateendpoints | Hayır | Hayır |
> | privatelinkservices | Hayır | Hayır |
> | publicıpaddresses | Evet-temel SKU<br>Standart SKU yok | Evet-temel SKU<br>Standart SKU yok |
> | publicıpöneklerini | Hayır | Hayır |
> | routefilters | Hayır | Hayır |
> | routetables | Hayır | Hayır |
> | securitypartnerproviders | Hayır | Hayır |
> | serviceendpointpolicies | Hayır | Hayır |
> | trafficmanagergeographichierarchies | Hayır | Hayır |
> | trafficmanagerprofiles | Hayır | Hayır |
> | trafficmanagerprofiles/ısı haritalarını | Hayır | Hayır |
> | trafficmanagerusermetricskeys | Hayır | Hayır |
> | virtualhub 'lar | Hayır | Hayır |
> | virtualnetworkgateways | Hayır | Hayır |
> | virtualnetworks | Hayır | Hayır |
> | virtualnetworktaps | Hayır | Hayır |
> | virtualyönlendiriciler | Hayır | Hayır |
> | virtualwan | Hayır | Hayır |
> | vpngateway (sanal WAN) | Hayır | Hayır |
> | vpnserverconfigurations | Hayır | Hayır |
> | vpnsites (sanal WAN) | Hayır | Hayır |

> [!IMPORTANT]
> Bkz. [ağ taşıma Kılavuzu](./move-limitations/networking-move-limitations.md).

## <a name="microsoftnotificationhubs"></a>Microsoft. Notificationhub 'Lar

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | checknameavaılabılıty | Hayır | Hayır |
> | checknamespaceavailability | Hayır | Hayır |
> | öznitelikleri | Hayır | Hayır |
> | ad alanları/notificationhub 'lar | Hayır | Hayır |
> | operationresults | Hayır | Hayır |
> | işlemler | Hayır | Hayır |

## <a name="microsoftobjectstore"></a>Microsoft. ObjectStore

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | osnamespaces | Hayır | Hayır |

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

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | leriniz | Evet | Evet |
> | deletedworkspaces | Hayır | Hayır |
> | bağlantı hedefleri | Hayır | Hayır |
> | yerlerini | Hayır | Hayır |
> | konumlar/operationdurumlarının | Hayır | Hayır |
> | işlemler | Hayır | Hayır |
> | storageınsii configs | Hayır | Hayır |
> | çalışma alanı | Evet | Evet |
> | çalışma alanları/veri kaynakları | Hayır | Hayır |
> | çalışma alanları/linkedservices | Hayır | Hayır |
> | çalışma alanları/linkedstorageaccounts | Hayır | Hayır |
> | çalışma alanları/meta veriler | Hayır | Hayır |
> | çalışma alanları/sorgu | Hayır | Hayır |
> | çalışma alanları/scopedprivatelinkproxy 'leri | Hayır | Hayır |

> [!IMPORTANT]
> Yeni bir aboneliğe geçiş için [abonelik kotalarını](azure-subscription-service-limits.md#azure-monitor-limits)aşmayacağından emin olun.
>
> Bağlı bir Otomasyon hesabına sahip olan çalışma alanları taşınamaz. Bir taşıma işlemine başlamadan önce, tüm otomasyon hesaplarının bağlantısını kaldırmayı unutmayın.

## <a name="microsoftoperationsmanagement"></a>Microsoft. OperationsManagement

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | managementassociations | Hayır | Hayır |
> | managementconfigurations | Hayır | Hayır |
> | işlemler | Hayır | Hayır |
> | çözümler | Hayır | Hayır |
> | görünümler | Hayır | Hayır |

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

## <a name="microsoftpolicyinsights"></a>Microsoft.PolicyInsights

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
> | panolar | Hayır | Hayır |
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
> | işlemler | Hayır | Hayır |
> | privatelinkservicesforpowerbı | Hayır | Hayır |
> | privatelinkservicesforpowerbı/operationresults | Hayır | Hayır |
> | Kira | Hayır | Hayır |
> | workspacecollections | Hayır | Hayır |

## <a name="microsoftpowerbidedicated"></a>Microsoft. Powerbiadanmış

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | kapasiteler | Evet | Evet |
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
> | providerkayıtları/defaultrollout | Hayır | Hayır |
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
> | kasaları | Evet | Evet |

> [!IMPORTANT]
> Bkz. [Kurtarma Hizmetleri taşıma Kılavuzu](../../backup/backup-azure-move-recovery-services-vault.md?toc=/azure/azure-resource-manager/toc.json).

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
> | öznitelikleri | Hayır | Hayır |
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
> | lardır | Evet | Evet |
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
> | etiketler | Hayır | Hayır |
> | templatespec | Hayır | Hayır |
> | templatespec/sürümler | Hayır | Hayır |
> | Kira | Hayır | Hayır |

## <a name="microsoftsaas"></a>Microsoft. SaaS

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | uygulamalar | Hayır | Hayır |
> | checkmoderneligibility | Hayır | Hayır |
> | checknameavaılabılıty | Hayır | Hayır |
> | operationresults | Hayır | Hayır |
> | işlemler | Hayır | Hayır |
> | saasresources | Hayır | Hayır |

## <a name="microsoftsearch"></a>Microsoft. Search

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | checknameavaılabılıty | Hayır | Hayır |
> | checkservicenameavaılabılıty | Hayır | Hayır |
> | işlemler | Hayır | Hayır |
> | resourcehealthmetadata | Hayır | Hayır |
> | searchservices | Evet | Evet |

> [!IMPORTANT]
> Farklı bölgelerde çeşitli arama kaynaklarını tek bir işlemde taşıyamazsınız. Bunun yerine, bunları ayrı işlemlerde taşıyın.

## <a name="microsoftsecurity"></a>Microsoft.Security

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | adaptivenetworkhardenings | Hayır | Hayır |
> | advancedthreatprotectionsettings | Hayır | Hayır |
> | alerts | Hayır | Hayır |
> | alertssuppressionrules | Hayır | Hayır |
> | allowedconnections | Hayır | Hayır |
> | applicationwhitedökümler | Hayır | Hayır |
> | assessmentmetadata | Hayır | Hayır |
> | değerlendirme | Hayır | Hayır |
> | Oto dissalertsrules | Hayır | Hayır |
> | akışlarını otomatikleştirin | Hayır | Hayır |
> | Oto provisioningsettings | Hayır | Hayır |
> | karmaşık anceresults | Hayır | Hayır |
> | uyumluluklarına | Hayır | Hayır |
> | datacollectionagents | Hayır | Hayır |
> | devicesecuritygroups | Hayır | Hayır |
> | discoveredsecuritysolutions | Hayır | Hayır |
> | externalsecuritysolutions | Hayır | Hayır |
> | ınformationprotectionpolicies | Hayır | Hayır |
> | ıotsecuritysolutions | Hayır | Hayır |
> | ıotsecuritysolutions/analiz ticsmodeller | Hayır | Hayır |
> | ıotsecuritysolutions/analticsmodeller/aggreggıt uyarıları | Hayır | Hayır |
> | ıotsecuritysolutions/analiz ticsmodeller/Aggreg, öneriler | Hayır | Hayır |
> | ıotsecuritysolutions/ıotalerts | Hayır | Hayır |
> | ıotsecuritysolutions/ıotalerttypes | Hayır | Hayır |
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
> | securescorecontroldefinitions | Hayır | Hayır |
> | securescorecontrols | Hayır | Hayır |
> | securesçekirdekler | Hayır | Hayır |
> | securesçekirdekler/securescorecontrols | Hayır | Hayır |
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
> | olaylara | Hayır | Hayır |
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

## <a name="microsoftservermanagement"></a>MICROSOFT. SUNUCUYÖNETIMI

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
> | öznitelikleri | Hayır | Hayır |
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
> | leriniz | Hayır | Hayır |
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
> | uygulamalar | Hayır | Hayır |
> | kapsayıcı grupları | Hayır | Hayır |
> | geçidinin | Hayır | Hayır |
> | yerlerini | Hayır | Hayır |
> | konumlar/applicationoperations | Hayır | Hayır |
> | konumlar/gatewayoperations | Hayır | Hayır |
> | konumlar/networkoperations | Hayır | Hayır |
> | konumlar/secretoperations | Hayır | Hayır |
> | konumlar/birimsiz işlemler | Hayır | Hayır |
> | Mamak | Hayır | Hayır |
> | işlemler | Hayır | Hayır |
> | kaynaklanır | Hayır | Hayır |
> | volumes | Hayır | Hayır |

## <a name="microsoftservices"></a>MICROSOFT. SERVISLERE

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
> | SignalR | Evet | Evet |
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

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | checknameavaılabılıty | Hayır | Hayır |
> | ınstancepools | Hayır | Hayır |
> | yerlerini | Evet | Evet |
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
> | larý | Hayır | Hayır |
> | sunucular/yönetimtoroperationsonuçları | Hayır | Hayır |
> | sunucular/Yöneticiler | Hayır | Hayır |
> | sunucular/danışmanları | Hayır | Hayır |
> | sunucular/Aggreg, databaseölçümlerini | Hayır | Hayır |
> | Sunucu/auditingsettings | Hayır | Hayır |
> | sunucular/otomatik olarak ayarlama | Hayır | Hayır |
> | sunucular/communicationlinks | Hayır | Hayır |
> | sunucular/veritabanları | Hayır | Hayır |
> | sunucular/veritabanları/danışmanları | Hayır | Hayır |
> | sunucular/veritabanları/auditingsettings | Hayır | Hayır |
> | sunucular/veritabanları/denetimkayıtları | Hayır | Hayır |
> | sunucular/veritabanları/otomatik olarak ayarlama | Hayır | Hayır |
> | sunucular/veritabanları/backuplongtermretentionpolicies | Hayır | Hayır |
> | sunucular/veritabanları/backupshorttermretentionpolicies | Hayır | Hayır |
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
> | sunucular/elaun havuzları | Hayır | Hayır |
> | sunucular/elaor havuzları/danışmanları | Hayır | Hayır |
> | sunucular/elaun havuzları/MetricDefinitions | Hayır | Hayır |
> | sunucular/ela/havuzlar/ölçümler | Hayır | Hayır |
> | sunucular/encryptionprotector | Hayır | Hayır |
> | Servers/extendedauditingsettings | Hayır | Hayır |
> | sunucular/failovergroups 'lar | Hayır | Hayır |
> | sunucular/içeri aktarma | Hayır | Hayır |
> | sunucular/ımportexportoperationsonuçları | Hayır | Hayır |
> | sunucular/jobaccounts | Hayır | Hayır |
> | sunucular/jobagents | Hayır | Hayır |
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
> | virtualkümeler | Hayır | Hayır |

> [!IMPORTANT]
> Bir veritabanı ve sunucu aynı kaynak grubunda olmalıdır. Bir SQL sunucusunu taşıdığınızda, tüm veritabanları da taşınır. Bu davranış, Azure SQL veritabanı ve Azure SQL veri ambarı veritabanları için geçerlidir.

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
> | sqlvirtualmachinegroups | Evet | Evet |
> | sqlvirtualmachinegroups/kullanılabilirliği bilitygrouplisteners | Hayır | Hayır |
> | sqlvirtualmachines | Evet | Evet |

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
> | storageaccounts | Evet | Evet |
> | storageaccounts/blobservices | Hayır | Hayır |
> | storageaccounts/fileservices | Hayır | Hayır |
> | storageaccounts/listaccountsas | Hayır | Hayır |
> | storageaccounts/listservicesas | Hayır | Hayır |
> | storageaccounts/queueservices | Hayır | Hayır |
> | storageaccounts/Services | Hayır | Hayır |
> | storageaccounts/Services/MetricDefinitions | Hayır | Hayır |
> | storageaccounts/tableservices | Hayır | Hayır |
> | vardır | Hayır | Hayır |

## <a name="microsoftstoragecache"></a>MICROSOFT. STORAGECACHE

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
> | storagesyncservices | Hayır | Hayır |
> | storagesyncservices/RegisteredServers | Hayır | Hayır |
> | storagesyncservices/syncgroups | Hayır | Hayır |
> | storagesyncservices/syncgroups/cloudendpoints | Hayır | Hayır |
> | storagesyncservices/syncgroups/serverendpoints | Hayır | Hayır |
> | storagesyncservices/iş akışları | Hayır | Hayır |

## <a name="microsoftstoragesyncdev"></a>MICROSOFT. STORAGESYNCDEV

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | storagesyncservices | Hayır | Hayır |

## <a name="microsoftstoragesyncint"></a>MICROSOFT. STORAGESYNCıNT

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

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | leriniz | Evet | Evet |
> | yerlerini | Hayır | Hayır |
> | konumlar/kotalar | Hayır | Hayır |
> | işlemler | Hayır | Hayır |
> | streammingjobs | Evet | Evet |

> [!IMPORTANT]
> Stream Analytics işleri çalışır durumda olduğunda taşınamaz.

## <a name="microsoftstreamanalyticsexplorer"></a>MICROSOFT. STREAMANALYTICSEXPLORER

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
> | privatelinkhub 'ları | Hayır | Hayır |
> | çalışma alanı | Hayır | Hayır |
> | çalışma alanları/bigdatapools | Hayır | Hayır |
> | çalışma alanları/operationresults | Hayır | Hayır |
> | çalışma alanları/operationdurumlar | Hayır | Hayır |
> | çalışma alanları/sqlpools | Hayır | Hayır |

## <a name="microsofttimeseriesinsights"></a>Microsoft. Timeseriesınsights

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | lý | Hayır | Hayır |
> | ortamlar/accesspolicies | Hayır | Hayır |
> | ortamlar/EventSources | Hayır | Hayır |
> | ortamlar/referencedataset 'ler | Hayır | Hayır |
> | işlemler | Hayır | Hayır |

## <a name="microsofttoken"></a>Microsoft. Token

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | depolaya | Hayır | Hayır |
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

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | account | Hayır | Hayır |
> | Hesap/uzantı | Hayır | Hayır |
> | hesap/proje | Hayır | Hayır |
> | checknameavaılabılıty | Hayır | Hayır |
> | işlemler | Hayır | Hayır |

> [!IMPORTANT]
> Azure DevOps aboneliğini değiştirmek için, bkz. [faturalandırma için kullanılan Azure aboneliğini değiştirme](/azure/devops/organizations/billing/change-azure-subscription?toc=/azure/azure-resource-manager/toc.json).

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

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | availableyığınları | Hayır | Hayır |
> | billingmeters | Hayır | Hayır |
> | sertifikalar | Hayır | Hayır |
> | checknameavaılabılıty | Hayır | Hayır |
> | connectiongateway 'ler | Evet | Evet |
> | bağlantının | Evet | Evet |
> | customapsıs | Evet | Evet |
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
> | kubeortamları | Hayır | Hayır |
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
> | serverfarms | Evet | Evet |
> | sunucugrupları/eventgridfilters | Hayır | Hayır |
> | Siteler | Evet | Evet |
> | siteler/eventgridfilters | Hayır | Hayır |
> | siteler/hostnamebindings | Hayır | Hayır |
> | Sites/networkconfig | Hayır | Hayır |
> | siteler/premieraddons | Hayır | Hayır |
> | siteler/yuvalar | Hayır | Hayır |
> | siteler/yuvalar/eventgridfilters | Hayır | Hayır |
> | siteler/yuvalar/hostnamebindings | Hayır | Hayır |
> | siteler/yuvalar/networkconfig | Hayır | Hayır |
> | sourcecontrols | Hayır | Hayır |
> | staticsites | Hayır | Hayır |
> | doğrulamalısınız | Hayır | Hayır |
> | verifyhostingenvironmentvnet | Hayır | Hayır |

> [!IMPORTANT]
> Bkz. [App Service taşıma Kılavuzu](./move-limitations/app-service-move-limitations.md).

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
