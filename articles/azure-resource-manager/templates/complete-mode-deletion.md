---
title: Tam modda silme
description: Azure Resource Manager şablonlarda kaynak türlerinin tamamlanma modu silme işlemini nasıl işleyeceğini gösterir.
ms.topic: conceptual
ms.date: 06/15/2020
ms.openlocfilehash: 4c16b8a0ffa1a4756a3063ca63f8c0eae2166f3e
ms.sourcegitcommit: 6571e34e609785e82751f0b34f6237686470c1f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/15/2020
ms.locfileid: "84791172"
---
# <a name="deletion-of-azure-resources-for-complete-mode-deployments"></a>Tüm mod dağıtımları için Azure kaynaklarını silme

Bu makalede, kaynak türlerinin, tamamlanmış modda dağıtılan bir şablonda olmadığında silinme işleminin nasıl işleneceği açıklanır.

Türü, tamamlanmış modla dağıtılan şablonda olmadığında, **Evet** ile işaretlenen kaynak türleri silinir.

**Hayır** ile işaretlenen kaynak türleri, şablonda olmadığında otomatik olarak silinmez; Ancak, üst kaynak silinirse bunlar silinir. Davranışın tam açıklaması için bkz. [Azure Resource Manager Dağıtım modları](deployment-modes.md).

[Bir şablonda birden fazla kaynak grubuna](cross-resource-group-deployment.md)dağıtırsanız, dağıtım işleminde belirtilen kaynak grubundaki kaynaklar silinebilir. İkincil kaynak gruplarındaki kaynaklar silinmez.

Kaynak sağlayıcısı ad alanına atlayın:
> [!div class="op_single_selector"]
> - [Microsoft. AAD](#microsoftaad)
> - [Microsoft. addons](#microsoftaddons)
> - [Microsoft. ADHybridHealthService](#microsoftadhybridhealthservice)
> - [Microsoft. Advisor](#microsoftadvisor)
> - [Microsoft. AlertsManagement](#microsoftalertsmanagement)
> - [Microsoft. AnalysisServices](#microsoftanalysisservices)
> - [Microsoft. Apimanane](#microsoftapimanagement)
> - [Microsoft. AppConfiguration](#microsoftappconfiguration)
> - [Microsoft. AppPlatform](#microsoftappplatform)
> - [Microsoft. kanıtlama](#microsoftattestation)
> - [Microsoft.Authorization](#microsoftauthorization)
> - [Microsoft. Automation](#microsoftautomation)
> - [Microsoft. AVS](#microsoftavs)
> - [Microsoft. Azure. Genfiliz](#microsoftazuregeneva)
> - [Microsoft. AzureActiveDirectory](#microsoftazureactivedirectory)
> - [Microsoft. AzureData](#microsoftazuredata)
> - [Microsoft. AzureStack](#microsoftazurestack)
> - [Microsoft. Azurestackhcı](#microsoftazurestackhci)
> - [Microsoft. AzureStackResourceMonitor](#microsoftazurestackresourcemonitor)
> - [Microsoft.Batch](#microsoftbatch)
> - [Microsoft. Faturalandırma](#microsoftbilling)
> - [Microsoft. BingMaps](#microsoftbingmaps)
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
> - [Microsoft. Biliveservices](#microsoftcognitiveservices)
> - [Microsoft. Commerce](#microsoftcommerce)
> - [Microsoft.Compute](#microsoftcompute)
> - [Microsoft. tüketim](#microsoftconsumption)
> - [Microsoft. Containerınstance](#microsoftcontainerinstance)
> - [Microsoft. ContainerRegistry](#microsoftcontainerregistry)
> - [Microsoft. ContainerService](#microsoftcontainerservice)
> - [Microsoft. CostManagement](#microsoftcostmanagement)
> - [Microsoft. Customerkasası](#microsoftcustomerlockbox)
> - [Microsoft. CustomProviders](#microsoftcustomproviders)
> - [Microsoft. DataBox](#microsoftdatabox)
> - [Microsoft. DataBoxEdge](#microsoftdataboxedge)
> - [Microsoft. Databricks](#microsoftdatabricks)
> - [Microsoft. DataCatalog](#microsoftdatacatalog)
> - [Microsoft. DataFactory](#microsoftdatafactory)
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
> - [Microsoft. DynamicsLcs](#microsoftdynamicslcs)
> - [Microsoft. EnterpriseKnowledgeGraph](#microsoftenterpriseknowledgegraph)
> - [Microsoft. EventGrid](#microsofteventgrid)
> - [Microsoft. EventHub](#microsofteventhub)
> - [Microsoft. deneme](#microsoftexperimentation)
> - [Microsoft. Falcon](#microsoftfalcon)
> - [Microsoft. Features](#microsoftfeatures)
> - [Microsoft. Gallery](#microsoftgallery)
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
> - [Microsoft.Intune](#microsoftintune)
> - [Microsoft. ıotcentral](#microsoftiotcentral)
> - [Microsoft. ıotspaces](#microsoftiotspaces)
> - [Microsoft. Keykasası](#microsoftkeyvault)
> - [Microsoft. Kubernetes](#microsoftkubernetes)
> - [Microsoft. KubernetesConfiguration](#microsoftkubernetesconfiguration)
> - [Microsoft.Kusto](#microsoftkusto)
> - [Microsoft. LabServices](#microsoftlabservices)
> - [Microsoft. Logic](#microsoftlogic)
> - [Microsoft. Machinöğrenim](#microsoftmachinelearning)
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
> - [Microsoft. Not defterleri](#microsoftnotebooks)
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
> - [Microsoft. SecurityGraph](#microsoftsecuritygraph)
> - [Microsoft. Securityınsights](#microsoftsecurityinsights)
> - [Microsoft. SerialConsole](#microsoftserialconsole)
> - [Microsoft.ServiceBus](#microsoftservicebus)
> - [Microsoft. ServiceFabric](#microsoftservicefabric)
> - [Microsoft. Servicefabrickafesi](#microsoftservicefabricmesh)
> - [Microsoft. Services](#microsoftservices)
> - [Microsoft. SignalRService](#microsoftsignalrservice)
> - [Microsoft. SoftwarePlan](#microsoftsoftwareplan)
> - [Microsoft. Solutions](#microsoftsolutions)
> - [Microsoft. SQL](#microsoftsql)
> - [Microsoft. SqlVirtualMachine](#microsoftsqlvirtualmachine)
> - [Microsoft.Storage](#microsoftstorage)
> - [Microsoft. StorageCache](#microsoftstoragecache)
> - [Microsoft. Storagerepce](#microsoftstoragereplication)
> - [Microsoft. Storagessync](#microsoftstoragesync)
> - [Microsoft. StorageSyncDev](#microsoftstoragesyncdev)
> - [Microsoft. Storagesyncınt](#microsoftstoragesyncint)
> - [Microsoft. StorSimple](#microsoftstorsimple)
> - [Microsoft. StreamAnalytics](#microsoftstreamanalytics)
> - [Microsoft. Subscription](#microsoftsubscription)
> - [Microsoft. SYNAPSE](#microsoftsynapse)
> - [Microsoft. Timeseriesınsights](#microsofttimeseriesinsights)
> - [Microsoft. Token](#microsofttoken)
> - [Microsoft. Virtualmachineımages](#microsoftvirtualmachineimages)
> - [Microsoft. VMware](#microsoftvmware)
> - [Microsoft. Vmwarechoparlör basit](#microsoftvmwarecloudsimple)
> - [Microsoft. VMwareOnAzure](#microsoftvmwareonazure)
> - [Microsoft. VnfManager](#microsoftvnfmanager)
> - [Microsoft. VSOnline](#microsoftvsonline)
> - [Microsoft. Web](#microsoftweb)
> - [Microsoft. Windowssavunma Deratp](#microsoftwindowsdefenderatp)
> - [Microsoft. WindowsESU](#microsoftwindowsesu)
> - [Microsoft. Windowsıot](#microsoftwindowsiot)
> - [Microsoft. WorkloadBuilder](#microsoftworkloadbuilder)
> - [Microsoft. WorkloadMonitor](#microsoftworkloadmonitor)

## <a name="microsoftaad"></a>Microsoft. AAD

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | DomainServices | Yes |
> | DomainServices/oucontainer | No |

## <a name="microsoftaddons"></a>Microsoft. addons

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | Destek sağlayıcıları | No |

## <a name="microsoftadhybridhealthservice"></a>Microsoft. ADHybridHealthService

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
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
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | konfigürasyonları | No |
> | Generatereyorumgeçişleri | No |
> | meta veriler | No |
> | Öneriler | No |
> | gizlemeleri | No |

## <a name="microsoftalertsmanagement"></a>Microsoft. AlertsManagement

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | actionRules | Yes |
> | alerts | No |
> | alertsList | No |
> | alertsMetaData | No |
> | alertsSummary | No |
> | alertsSummaryList | No |
> | smartDetectorAlertRules | Yes |
> | smartGroups | No |

## <a name="microsoftanalysisservices"></a>Microsoft. AnalysisServices

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | larý | Yes |

## <a name="microsoftapimanagement"></a>Microsoft. Apimanane

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | reportFeedback | No |
> | hizmet | Yes |
> | validateServiceName | No |

## <a name="microsoftappconfiguration"></a>Microsoft. AppConfiguration

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | Configurationmağazaların | Yes |
> | Configurationmağazaların/eventGridFilters | No |

## <a name="microsoftappplatform"></a>Microsoft. AppPlatform

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | Spring | Yes |
> | Yay/uygulamalar | No |
> | Yay/uygulamalar/dağıtımlar | No |

## <a name="microsoftattestation"></a>Microsoft. kanıtlama

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | attestationProviders | Yes |

## <a name="microsoftauthorization"></a>Microsoft.Authorization

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | classicAdministrators | No |
> | Datatakma adlar | No |
> | Denyasatamaları | No |
> | Erişimi yükseltme | No |
> | Findorphanroleatamalar | No |
> | kaynaktaki | No |
> | izinler | No |
> | Poliyasatamaları | No |
> | policyDefinitions | No |
> | policySetDefinitions | No |
> | privateLinkAssociations | No |
> | providerOperations | No |
> | resourceManagementPrivateLinks | No |
> | roleAssignments | No |
> | Roleatamasussusageölçümleri | No |
> | roleDefinitions | No |

## <a name="microsoftautomation"></a>Microsoft. Automation

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | automationAccounts | Yes |
> | automationAccounts/Configurations | Yes |
> | automationAccounts/Jobs | No |
> | automationAccounts/Privateendpointconnectionproxy 'Leri | No |
> | automationAccounts/privateEndpointConnections | No |
> | automationAccounts/privateLinkResources | No |
> | automationAccounts/runbook 'lar | Yes |
> | automationAccounts/softwareUpdateConfigurations | No |
> | automationAccounts/Web kancaları | No |

## <a name="microsoftavs"></a>Microsoft. AVS

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | Privatebulutlar | Yes |
> | Privatebulutlar/yetkilendirmeler | No |
> | Privatebulutlar/kümeler | No |
> | Privatebulutlar/hcxEnterpriseSites | No |

## <a name="microsoftazuregeneva"></a>Microsoft. Azure. Genfiliz

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | lý | No |
> | ortamlar/hesaplar | No |
> | ortamlar/hesaplar/ad alanları | No |
> | ortamlar/hesaplar/ad alanları/yapılandırma | No |

## <a name="microsoftazureactivedirectory"></a>Microsoft. AzureActiveDirectory

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | b2cDirectories | Yes |
> | b2ctenants | No |

## <a name="microsoftazuredata"></a>Microsoft. AzureData

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | Veri denetleyicileri | Yes |
> | Hybriddatayöneticileri | Yes |
> | Postgresınstances | Yes |
> | SQLInstances | Yes |
> | Sqlmanagedınstances | Yes |
> | Sqlserverınstances | Yes |
> | Sqlserverkayıtları | Yes |
> | Sqlserverkayıtları/sqlServers | No |

## <a name="microsoftazurestack"></a>Microsoft. AzureStack

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | cloudManifestFiles | No |
> | kayıtlarında | Yes |
> | kayıt/müşteri abonelikleri | No |
> | kayıtlar/ürünler | No |

## <a name="microsoftazurestackhci"></a>Microsoft. Azurestackhcı

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | leriniz | Yes |

## <a name="microsoftazurestackresourcemonitor"></a>Microsoft. AzureStackResourceMonitor

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | storageAccountMonitor | Yes |

## <a name="microsoftbatch"></a>Microsoft.Batch

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | batchAccounts | Yes |

## <a name="microsoftbilling"></a>Microsoft. Faturalandırma

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | billingAccounts | No |
> | billingAccounts/anlaşmalar | No |
> | billingAccounts/billingPermissions | No |
> | billingAccounts/billingProfiles | No |
> | billingAccounts/Billingprofiller/billingPermissions | No |
> | billingAccounts/billingProfiles/Billingroleatamaları | No |
> | billingAccounts/billingProfiles/billingRoleDefinitions | No |
> | billingAccounts/billingProfiles/Billingabonelikleri | No |
> | billingAccounts/billingProfiles/Createbillingroleatama | No |
> | billingAccounts/billingProfiles/müşteriler | No |
> | billingAccounts/billingProfiles/yönergeler | No |
> | billingAccounts/billingProfiles/faturalar | No |
> | billingAccounts/billingProfiles/faturalar/fiyat listesi | No |
> | billingAccounts/billingProfiles/faturalar/işlemler | No |
> | billingAccounts/billingProfiles/ınvoicesections | No |
> | billingAccounts/billingProfiles/ınvoicesections/billingPermissions | No |
> | billingAccounts/billingProfiles/ınvoicesections/Billingroleatamaları | No |
> | billingAccounts/billingProfiles/ınvoicesections/billingRoleDefinitions | No |
> | billingAccounts/billingProfiles/ınvoicesections/Billingabonelikleri | No |
> | billingAccounts/billingProfiles/ınvoicesections/Createbillingroleatama | No |
> | billingAccounts/billingProfiles/ınvoicesections/ınitiatetransfer | No |
> | billingAccounts/billingProfiles/ınvoicesections/ürünler | No |
> | billingAccounts/billingProfiles/ınvoicesections/ürünler/aktarım | No |
> | billingAccounts/billingProfiles/ınvoicesections/ürünler/updateAutoRenew | No |
> | billingAccounts/billingProfiles/ınvoicesections/işlemler | No |
> | billingAccounts/billingProfiles/ınvoicesections/aktarımlar | No |
> | billingAccounts/BillingProfiles/patchOperations | No |
> | billingAccounts/billingProfiles/paymentMethods | No |
> | billingAccounts/billingProfiles/ilkeler | No |
> | billingAccounts/billingProfiles/fiyat listesi | No |
> | billingAccounts/billingProfiles/pricesheetDownloadOperations | No |
> | billingAccounts/billingProfiles/ürünler | No |
> | billingAccounts/billingProfiles/işlemler | No |
> | billingAccounts/Billingroleatamaları | No |
> | billingAccounts/billingRoleDefinitions | No |
> | billingAccounts/Billingabonelikleri | No |
> | billingAccounts/Billingabonelikleri/faturalar | No |
> | billingAccounts/Createbillingroleatama | No |
> | billingAccounts/Createınvoicesectionoperations | No |
> | billingAccounts/müşteriler | No |
> | billingAccounts/müşteriler/billingPermissions | No |
> | billingAccounts/müşteriler/Billingabonelikleri | No |
> | billingAccounts/müşteriler/ınitiatetransfer | No |
> | billingAccounts/müşteriler/ilkeler | No |
> | billingAccounts/müşteriler/ürünler | No |
> | billingAccounts/müşteriler/işlemler | No |
> | billingAccounts/müşteriler/aktarımlar | No |
> | billingAccounts/departmanlar | No |
> | billingAccounts/KayıtSayısı | No |
> | billingAccounts/faturalar | No |
> | billingAccounts/ınvoicesections | No |
> | billingAccounts/ınvoicesections/billingSubscriptionMoveOperations | No |
> | billingAccounts/ınvoicesections/Billingabonelikleri | No |
> | billingAccounts/ınvoicesections/Billingabonelikleri/aktarımı | No |
> | billingAccounts/ınvoicesections/yükselt | No |
> | billingAccounts/ınvoicesections/ınitiatetransfer | No |
> | billingAccounts/ınvoicesections/patchOperations | No |
> | billingAccounts/ınvoicesections/productMoveOperations | No |
> | billingAccounts/Ürünler/Ürünler | No |
> | billingAccounts/ınvoicesections/ürünler/transfer | No |
> | billingAccounts/ınvoicesections/ürünler/updateAutoRenew | No |
> | billingAccounts/ınvoicesections/işlemler | No |
> | billingAccounts/ınvoicesections/aktarımlar | No |
> | billingAccounts/Lineofkredisi | No |
> | billingAccounts/patchOperations | No |
> | billingAccounts/paymentMethods | No |
> | billingAccounts/ürünler | No |
> | billingAccounts/işlemler | No |
> | Billingdönemler | No |
> | billingPermissions | No |
> | billingProperty | No |
> | Billingroleatamaları | No |
> | billingRoleDefinitions | No |
> | Createbillingroleatama | No |
> | bölümlerinin | No |
> | kayıt sayısı | No |
> | faturalardan | No |
> | girişinde | No |
> | aktarımlar/acceptTransfer | No |
> | aktarımlar/declineTransfer | No |
> | aktarımlar/operationStatus | No |
> | aktarımlar/validateTransfer | No |
> | validateAddress | No |

## <a name="microsoftbingmaps"></a>Microsoft. BingMaps

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | Mapapsıs | Yes |
> | updateCommunicationPreference | No |

## <a name="microsoftblockchain"></a>Microsoft. Blockzinciri

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | blockchainMembers | Yes |
> | Cordadmembers | Yes |
> | izleyicileri | Yes |

## <a name="microsoftblockchaintokens"></a>Microsoft. BlockchainTokens

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | TokenServices | Yes |
> | TokenServices/BlockchainNetworks | No |
> | TokenServices/Groups | No |
> | TokenServices/gruplar/hesaplar | No |
> | TokenServices/TokenTemplates | No |

## <a name="microsoftblueprint"></a>Microsoft. Blueprint

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | Şema tasmi | No |
> | Blueprintasbir/Atamaperations | No |
> | Blueprintasbir/işlemleri | No |
> | Blueprint | No |
> | planlar/yapıtlar | No |
> | planlar/sürümler | No |
> | planlar/sürümler/yapılar | No |

## <a name="microsoftbotservice"></a>Microsoft. BotService

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | botServices | Yes |
> | botServices/kanallar | No |
> | botServices/Connections | No |
> | diller | No |
> | templates | No |

## <a name="microsoftcache"></a>Microsoft. Cache

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | Redis | Yes |
> | Redsıs/EventGridFilters | No |
> | Redsıs/Privateendpointconnectionproxy 'Leri | No |
> | Redsıs/Privateendpointconnectionproxy/doğrulama | No |
> | Redsıs/privateEndpointConnections | No |
> | Redsıs/privateLinkResources | No |
> | redisEnterprise | Yes |

## <a name="microsoftcapacity"></a>Microsoft. Capacity

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | appliedReservations | No |
> | Oto Kotaartışı | No |
> | calculateExchange | No |
> | calculatePrice | No |
> | calculatePurchasePrice | No |
> | larına | No |
> | Ticari Vaalrezervler | No |
> | değişimi | No |
> | placePurchaseOrder | No |
> | Rezervler | No |
> | Rezervler/Hesaplaizterefund | No |
> | Rezervler/Birleştir | No |
> | Rezervler/rezervasyonlar | No |
> | Rezervler/rezervasyonlar/düzeltmeler | No |
> | Rezervler/geri dönüş | No |
> | Rezervler/Böl | No |
> | Rezervler/takas | No |
> | oluşturamaz | No |
> | resourceProviders | No |
> | kaynaklar | No |
> | validateReservationOrder | No |

## <a name="microsoftcdn"></a>Microsoft. CDN

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | CdnWebApplicationFirewallManagedRuleSets | No |
> | CdnWebApplicationFirewallPolicies | Yes |
> | edgenodes | No |
> | lerinize | Yes |
> | Profiller/uç noktalar | Yes |
> | Profiller/uç noktalar/customdomains | No |
> | Profiller/uç noktalar/origingroups | No |
> | Profiller/uç noktalar/kaynaklar | No |
> | Validatearaştırması | No |

## <a name="microsoftcertificateregistration"></a>Microsoft. CertificateRegistration

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | Sertifikadüzenleri | Yes |
> | certificateOrders/Certificates | No |
> | Validatecertificateregistrationınformation | No |

## <a name="microsoftchangeanalysis"></a>Microsoft. ChangeAnalysis

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | profil | No |
> | resourceChanges | No |

## <a name="microsoftclassiccompute"></a>Microsoft.ClassicCompute

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | Yetenek | No |
> | domainNames | Yes |
> | domainNames/yetenekleri | No |
> | domainNames/internalLoadBalancers | No |
> | domainNames/serviceCertificates | No |
> | domainNames/Yuvaları | No |
> | domainNames/yuvalar/roller | No |
> | domainNames/yuvalar/roller/metricDefinitions | No |
> | domainNames/yuvalar/roller/ölçümler | No |
> | moveSubscriptionResources | No |
> | operatingSystemFamilies | No |
> | operatingSystems | No |
> | quotas | No |
> | resourceTypes | No |
> | Validatesubscriptionmoveavaılabılıty | No |
> | virtualMachines | Yes |
> | virtualMachines/diagnosticSettings | No |
> | virtualMachines/metricDefinitions | No |
> | virtualMachines/ölçümler | No |

## <a name="microsoftclassicinfrastructuremigrate"></a>Microsoft. ClassicInfrastructureMigrate

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | classicInfrastructureResources | No |

## <a name="microsoftclassicnetwork"></a>Microsoft. ClassicNetwork

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | Yetenek | No |
> | expressRouteCrossConnections | No |
> | expressRouteCrossConnections/peerler | No |
> | gatewaySupportedDevices | No |
> | networkSecurityGroups | Yes |
> | quotas | No |
> | Rezervler | Yes |
> | virtualNetworks | Yes |
> | virtualNetworks/Remotevirtualnetworkpeeringproxy 'Leri | No |
> | virtualNetworks/Virtualnetworkpeerler | No |

## <a name="microsoftclassicstorage"></a>Microsoft. ClassicStorage

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | Yetenek | No |
> | disklerinden | No |
> | images | No |
> | osImages | No |
> | Osplatformımages | No |
> | Publicımages | No |
> | quotas | No |
> | storageAccounts | Yes |
> | storageAccounts/blobServices | No |
> | storageAccounts/fileServices | No |
> | storageAccounts/metricDefinitions | No |
> | storageAccounts/ölçümler | No |
> | storageAccounts/queueServices | No |
> | storageAccounts/Services | No |
> | storageAccounts/Services/diagnosticSettings | No |
> | storageAccounts/Services/metricDefinitions | No |
> | storageAccounts/Services/ölçümler | No |
> | storageAccounts/tableServices | No |
> | storageAccounts/Vmımages | No |
> | Vmımages | No |

## <a name="microsoftcognitiveservices"></a>Microsoft. Biliveservices

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | accounts | Yes |

## <a name="microsoftcommerce"></a>Microsoft. Commerce

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | RateCard | No |
> | Usagetoplamaları | No |

## <a name="microsoftcompute"></a>Microsoft.Compute

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | availabilitySets | Yes |
> | Diskeriþler | Yes |
> | diskEncryptionSets | Yes |
> | disklerinden | Yes |
> | Galeriler | Yes |
> | Galeriler/uygulamalar | No |
> | Galeriler/uygulamalar/sürümler | No |
> | Galeriler/görüntüler | No |
> | Galeriler/resimler/sürümler | No |
> | hostGroups | Yes |
> | hostGroups/konaklar | Yes |
> | images | Yes |
> | proximityPlacementGroups | Yes |
> | restorePointCollections | Yes |
> | restorePointCollections/restorePoints | No |
> | sharedVMExtensions | Yes |
> | sharedVMExtensions/sürümler | No |
> | Sharedvmımages | Yes |
> | Sharedvmımages/sürümler | No |
> | anlık görüntüler | Yes |
> | sshPublicKeys | Yes |
> | swiftizin | Yes |
> | virtualMachines | Yes |
> | virtualMachines/uzantıları | Yes |
> | virtualMachines/metricDefinitions | No |
> | virtualMachines/runCommands | Yes |
> | virtualMachineScaleSets | Yes |
> | virtualMachineScaleSets/uzantılar | No |
> | virtualMachineScaleSets/NetworkInterfaces | No |
> | virtualMachineScaleSets/Publicıpaddresses | No |
> | virtualMachineScaleSets/virtualMachines | No |
> | virtualMachineScaleSets/virtualMachines/NetworkInterfaces | No |

## <a name="microsoftconsumption"></a>Microsoft. tüketim

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | Aggregmalyt maliyeti | No |
> | Bakiyeler | No |
> | Bütçeler | No |
> | Ücretler | No |
> | CostTags | No |
> | iler | No |
> | etkinlikler | No |
> | Tahminler | No |
> | oluş | No |
> | Marketlerinden | No |
> | Pricesheets | No |
> | ürün | No |
> | Rezervde ayrıntıları | No |
> | ReservationRecommendationDetails | No |
> | Rezervationönerilere | No |
> | Rezervlerin Özeti | No |
> | Rezervlik Işlemleri | Hayır |
> | Etiketler | Hayır |
> | Kira | No |
> | Terimler | No |
> | UsageDetails | No |

## <a name="microsoftcontainerinstance"></a>Microsoft. Containerınstance

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | Kapsayıcı grupları | Yes |
> | serviceAssociationLinks | No |

## <a name="microsoftcontainerregistry"></a>Microsoft. ContainerRegistry

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | kayıt | Yes |
> | kayıt defterleri/agentPools | Yes |
> | kayıt defterleri/derlemeler | No |
> | kayıt defterleri/derlemeler/iptal | No |
> | kayıt defterleri/derlemeler/getLogLink | No |
> | kayıt defterleri/buildTasks | Yes |
> | kayıt defterleri/buildTasks/Steps | No |
> | kayıt defterleri/eventGridFilters | No |
> | kayıt defterleri/Exporthatlarının | No |
> | kayıt defterleri/generateCredentials | No |
> | kayıt defterleri/getBuildSourceUploadUrl 'Si | No |
> | kayıt defterleri/GetCredentials | No |
> | kayıt defterleri/ımportımage | No |
> | kayıt defterleri/ımporthatlarının | No |
> | kayıt defterleri/ardışık düzen eylemsizlik | No |
> | kayıt defterleri/Privateendpointconnectionproxy 'Leri | No |
> | kayıt defterleri/Privateendpointconnectionproxy/doğrulama | No |
> | kayıt defterleri/privateEndpointConnections | No |
> | kayıt defterleri/privateLinkResources | No |
> | kayıt defterleri/queueBuild | No |
> | kayıt defterleri/regenerateCredential | No |
> | kayıt defterleri/regenerateCredentials | No |
> | kayıt defterleri/çoğaltmalar | Yes |
> | kayıt defterleri/çalıştırmalar | No |
> | kayıt defterleri/çalıştırmalar/iptal | No |
> | kayıt defterleri/scheduleRun | No |
> | kayıt defterleri/Kapsameşlemler | No |
> | kayıt defterleri/Taskçalıştırmaları | No |
> | kayıt defterleri/görevler | Yes |
> | kayıt defterleri/belirteçler | No |
> | kayıt defterleri/updatePolicies | No |
> | kayıt defterleri/Web kancaları | Yes |
> | kayıt defterleri/Web kancaları/getCallbackConfig | No |
> | kayıt defterleri/Web kancaları/ping | No |

## <a name="microsoftcontainerservice"></a>Microsoft. ContainerService

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | containerServices | Yes |
> | Managedkümeler | Yes |
> | openShiftManagedClusters | Yes |

## <a name="microsoftcostmanagement"></a>Microsoft. CostManagement

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | Uyarılar | No |
> | BillingAccounts | No |
> | Bütçeler | No |
> | Cloudbağlayıcıları | No |
> | Bağlayıcılar | Yes |
> | costAllocationRules | No |
> | Departmanlar | No |
> | Boyutlar | No |
> | Kayıt sayısı | No |
> | Aktarımları | No |
> | ExternalBillingAccounts | No |
> | ExternalBillingAccounts/uyarılar | No |
> | ExternalBillingAccounts/Boyutlar | No |
> | ExternalBillingAccounts/tahmin | No |
> | ExternalBillingAccounts/sorgu | No |
> | Externalabonelikleri | No |
> | Externalabonelikleri/uyarıları | No |
> | Externalabonelikler/Boyutlar | No |
> | Externalabonelikler/tahmin | No |
> | Externalabonelikler/sorgu | No |
> | Tahmin | No |
> | Sorgu | No |
> | register | No |
> | Reportconfigs | No |
> | Raporlar | No |
> | Ayarlar | No |
> | showbackRules | No |
> | Görünümler | No |

## <a name="microsoftcustomerlockbox"></a>Microsoft. Customerkasası

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | istekleri | No |

## <a name="microsoftcustomproviders"></a>Microsoft. CustomProviders

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | içermektedir | No |
> | resourceProviders | Yes |

## <a name="microsoftdatabox"></a>Microsoft. DataBox

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | Çizelge | Yes |

## <a name="microsoftdataboxedge"></a>Microsoft. DataBoxEdge

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | DataBoxEdgeDevices | Yes |

## <a name="microsoftdatabricks"></a>Microsoft. Databricks

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | çalışma alanı | Yes |
> | çalışma alanları/dbWorkspaces | No |
> | çalışma alanları/storageEncryption | No |
> | çalışma alanları/Virtualnetworkpeerler | No |

## <a name="microsoftdatacatalog"></a>Microsoft. DataCatalog

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | larına | Yes |
> | veri katalogları | Yes |
> | veri katalogları/veri kaynakları | No |
> | veri katalogları/veri kaynakları/taramalar | No |
> | veri katalogları/veri kaynakları/taramalar/veri kümeleri | No |
> | veri katalogları/veri kaynakları/taramalar/filtreler | No |
> | veri katalogları/veri kaynakları/taramalar/Tetikleyiciler | No |
> | veri katalogları/scanrulesets | No |

## <a name="microsoftdatafactory"></a>Microsoft. DataFactory

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | Veri fabrikaları | Yes |
> | DataFactory/diagnosticSettings | No |
> | DataFactory/metricDefinitions | No |
> | dataFactorySchema | No |
> | larının | Yes |
> | Fabrika/tümleştirme çalışma zamanları | No |

## <a name="microsoftdatalakeanalytics"></a>Microsoft. DataLakeAnalytics

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | accounts | Yes |
> | hesaplar/dataLakeStoreAccounts | No |
> | hesaplar/storageAccounts | No |
> | hesaplar/storageAccounts/kapsayıcılar | No |
> | hesaplar/Transferanaliz tici | No |

## <a name="microsoftdatalakestore"></a>Microsoft. DataLakeStore

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | accounts | Yes |
> | hesaplar/eventGridFilters | No |
> | hesaplar/firewallRules | No |

## <a name="microsoftdatamigration"></a>Microsoft. DataMigration

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | services | Yes |
> | Hizmetler/Projeler | Yes |

## <a name="microsoftdataprotection"></a>Microsoft. DataProtection

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | Backupkasaları | Yes |

## <a name="microsoftdatashare"></a>Microsoft. DataShare

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | accounts | Yes |
> | hesaplar/paylaşımlar | No |
> | hesaplar/paylaşımlar/veri kümeleri | No |
> | hesaplar/paylaşımlar/davetler | No |
> | hesaplar/paylaşımlar/providersharesubscriptions | No |
> | hesaplar/paylaşımlar/synchronizationSettings | No |
> | hesaplar/parçalar esubscriptions | No |
> | hesaplar/parçalar esubscriptions/Consumersourcedataset 'ler | No |
> | hesaplar/parçalar esubscriptions/datasetmappings | No |
> | hesaplar/parçalar esubscriptions/Tetikleyiciler | No |

## <a name="microsoftdbformariadb"></a>Microsoft. Dbformarıdb

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | larý | Yes |
> | sunucular/danışmanları | No |
> | sunucular/anahtarlar | No |
> | sunucular/Privateendpointconnectionproxy 'Leri | No |
> | sunucular/privateEndpointConnections | No |
> | sunucular/privateLinkResources | No |
> | sunucular/Querymetinmetinleri | No |
> | sunucular/recoverableServers | No |
> | sunucular/topQueryStatistics | No |
> | sunucular/virtualNetworkRules | No |
> | sunucular/waitStatistics | No |

## <a name="microsoftdbformysql"></a>Microsoft. Dbformyısql

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | larý | Yes |
> | sunucular/danışmanları | No |
> | sunucular/anahtarlar | No |
> | sunucular/Privateendpointconnectionproxy 'Leri | No |
> | sunucular/privateEndpointConnections | No |
> | sunucular/privateLinkResources | No |
> | sunucular/Querymetinmetinleri | No |
> | sunucular/recoverableServers | No |
> | sunucular/topQueryStatistics | No |
> | sunucular/virtualNetworkRules | No |
> | sunucular/waitStatistics | No |

## <a name="microsoftdbforpostgresql"></a>Microsoft. DBforPostgreSQL

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | Sunucu grupları | Yes |
> | larý | Yes |
> | sunucular/danışmanları | No |
> | sunucular/anahtarlar | No |
> | sunucular/Privateendpointconnectionproxy 'Leri | No |
> | sunucular/privateEndpointConnections | No |
> | sunucular/privateLinkResources | No |
> | sunucular/Querymetinmetinleri | No |
> | sunucular/recoverableServers | No |
> | sunucular/topQueryStatistics | No |
> | sunucular/virtualNetworkRules | No |
> | sunucular/waitStatistics | No |
> | serversv2 | Yes |
> | Teksunucular | Yes |

## <a name="microsoftdeploymentmanager"></a>Microsoft. DeploymentManager

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | artifactSources | Yes |
> | piyasaya çıkarma | Yes |
> | Servicetopolojileri | Yes |
> | Servicetopolojileri/hizmetler | Yes |
> | Servicetopolojileri/hizmetler/serviceUnits | Yes |
> | adımlar | Yes |

## <a name="microsoftdesktopvirtualization"></a>Microsoft. DesktopVirtualization

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | applicationgroups | Yes |
> | applicationgroups/uygulamalar | No |
> | applicationgroups/masaüstleri | No |
> | applicationgroups/startmenuıtems | No |
> | Ana bilgisayar havuzları | Yes |
> | hostpools/oturumkonakları | No |
> | hostpools/sessionkonakları/usersessions | No |
> | hosthavuzlar/usersessions | No |
> | çalışma alanı | Yes |

## <a name="microsoftdevices"></a>Microsoft. Devices

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | Elaun havuzları | Yes |
> | Elaun havuzları/ıothubkiracılar | Yes |
> | Elaor havuzları/ıothubkiracılar/securitySettings | No |
> | Iothubs | Yes |
> | IotHubs/eventGridFilters | No |
> | IotHubs/securitySettings | No |
> | ProvisioningServices | Yes |
> | vardır | No |

## <a name="microsoftdevops"></a>Microsoft. DevOps

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | düzenler | Yes |

## <a name="microsoftdevspaces"></a>Microsoft. DevSpaces

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | denetleyiciler | Yes |

## <a name="microsoftdevtestlab"></a>Microsoft. DevTestLab

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | labcenters | Yes |
> | larda | Yes |
> | Laboratuvarlar/ortamlar | Yes |
> | Labs/Servicerunanlar | Yes |
> | Labs/virtualMachines | Yes |
> | cağını | Yes |

## <a name="microsoftdigitaltwins"></a>Microsoft. DigitalTwins

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | Digitaltwınsınstances | Yes |
> | Digitaltwınsınstances/endpoints | No |

## <a name="microsoftdocumentdb"></a>Microsoft.DocumentDB

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | databaseAccountNames | No |
> | Veritabanı hesapları | Yes |

## <a name="microsoftdomainregistration"></a>Microsoft. DomainRegistration

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | etki alanları | Yes |
> | Domains/Domainownershiptanýmlayýcýlarý | No |
> | generateSsoRequest | No |
> | topLevelDomains | No |
> | Validatedomainregistrationınformation | No |

## <a name="microsoftdynamicslcs"></a>Microsoft. DynamicsLcs

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | lcsprojects | No |
> | lcsprojects/clouddağıtımları | No |
> | lcsprojects/bağlayıcılar | No |

## <a name="microsoftenterpriseknowledgegraph"></a>Microsoft. EnterpriseKnowledgeGraph

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | services | Yes |

## <a name="microsofteventgrid"></a>Microsoft. EventGrid

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | etki alanları | Yes |
> | etki alanları/konular | No |
> | Eventabonelikleri | No |
> | Extensionkonuları | No |
> | partnerNamespaces | Yes |
> | partnerNamespaces/eventChannels | No |
> | iş ortağı kayıtları | Yes |
> | iş ortağı konuları | Yes |
> | iş ortağı konuları/Eventabonelikleri | No |
> | Sistem konuları | Yes |
> | Sistem konuları/Eventabonelikler | No |
> | konuları | Yes |
> | topicTypes | No |

## <a name="microsofteventhub"></a>Microsoft. EventHub

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | leriniz | Yes |
> | öznitelikleri | Yes |
> | ad alanları/authorizationrules | No |
> | ad alanları/diskalrecoveryconfigs | No |
> | ad alanları/eventhubs | No |
> | ad alanları/eventhubs/authorizationrules | No |
> | ad alanları/eventhubs/consumergroups | No |
> | ad alanları/networkrulesets | No |

## <a name="microsoftexperimentation"></a>Microsoft. deneme

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | experimentWorkspaces | Yes |

## <a name="microsoftfalcon"></a>Microsoft. Falcon

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | öznitelikleri | Yes |

## <a name="microsoftfeatures"></a>Microsoft. Features

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | Özellik sağlayıcıları | No |
> | özellikler | No |
> | sağlayıcılarla | No |
> | subscriptionFeatureRegistrations | No |

## <a name="microsoftgallery"></a>Microsoft. Gallery

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | kaydedemez | No |
> | gallergıtems | No |
> | generateartifactaccessuri | No |
> | myarea | No |
> | myarea/alan | No |
> | myarea/alan/alan | No |
> | myareas/Areas/Areas/gallergıtems | No |
> | myareas/Areas/gallergıtems | No |
> | myarea/gallergıtems | No |
> | register | No |
> | kaynaklar | No |
> | elde edilecek esourcesbyıd | No |

## <a name="microsoftgenomics"></a>Microsoft. Genomiks

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | accounts | Yes |

## <a name="microsoftguestconfiguration"></a>Microsoft. GuestConfiguration

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | Oto Managedaccounts | Yes |
> | Oto Managedvmconfigurationprofiles | Yes |
> | Configurationprofileatamalar | No |
> | Guestconfigurationatamaları | No |
> | yazılımıdır | No |
> | softwareUpdateProfile | No |
> | softwareUpdates | No |

## <a name="microsofthanaonazure"></a>Microsoft. HanaOnAzure

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | Hanaınstances | Yes |
> | Sapizleyicileri | Yes |

## <a name="microsofthardwaresecuritymodules"></a>Microsoft. HardwareSecurityModules

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | ayrılmış Atedhsms | Yes |

## <a name="microsofthdinsight"></a>Microsoft. HDInsight

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | leriniz | Yes |
> | kümeler/uygulamalar | No |

## <a name="microsofthealthcareapis"></a>Microsoft. Healthgelişme API 'leri

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | services | Yes |
> | Hizmetler/ıomtbağlayıcılar | No |
> | Hizmetler/ıomtbağlayıcılar/bağlantılar | No |
> | Hizmetler/ıomtbağlayıcılar/eşlemeler | No |
> | Hizmetler/Privateendpointconnectionproxy 'Leri | No |
> | Hizmetler/privateEndpointConnections | Yes |
> | Hizmetler/privateLinkResources | Yes |

## <a name="microsofthybridcompute"></a>Microsoft. HybridCompute

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | larla | Yes |
> | makineler/uzantılar | Yes |

## <a name="microsofthybriddata"></a>Microsoft. HybridData

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | Veri yöneticileri | Yes |

## <a name="microsofthybridnetwork"></a>Microsoft. HybridNetwork

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | cihazlar | Yes |
> | registeredSubscriptions | No |
> | larını | No |
> | satıcılar/SKU 'lar | No |
> | satıcılar/vnfs | No |
> | Virtualnetworkfunctionsku 'Ları | No |
> | vnfs | Yes |

## <a name="microsofthydra"></a>Microsoft. Hydra

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | bileşenleri | Yes |
> | networkScopes | Yes |

## <a name="microsoftimportexport"></a>Microsoft. ımportexport

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | Çizelge | Yes |

## <a name="microsoftintune"></a>Microsoft.Intune

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | diagnosticSettings | No |
> | diagnosticSettingsCategories | No |

## <a name="microsoftiotcentral"></a>Microsoft. ıotcentral

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | appTemplates | No |
> | Iotapps | Yes |

## <a name="microsoftiotspaces"></a>Microsoft. ıotspaces

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | Graf | Yes |

## <a name="microsoftkeyvault"></a>Microsoft. Keykasası

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | Silinkaults | No |
> | hsmPools | Yes |
> | managedHSMs | Yes |
> | kasaları | Yes |
> | kasa/erişim Ilkeleri | No |
> | kasa/eventGridFilters | No |
> | kasa/gizlilikler | No |

## <a name="microsoftkubernetes"></a>Microsoft. Kubernetes

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | Connectedkümeler | Yes |
> | registeredSubscriptions | No |

## <a name="microsoftkubernetesconfiguration"></a>Microsoft. KubernetesConfiguration

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | sourceControlConfigurations | No |

## <a name="microsoftkusto"></a>Microsoft.Kusto

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | leriniz | Yes |
> | kümeler/attacheddatabaseconfigurations | No |
> | kümeler/veritabanları | No |
> | kümeler/veritabanları/veri bağlantıları | No |
> | kümeler/veritabanları/eventhubconnections | No |
> | kümeler/veritabanları/princıpalasbir | No |
> | kümeler/veri bağlantıları | No |
> | kümeler/princıpalasbir | No |
> | kümeler/parçalar | No |

## <a name="microsoftlabservices"></a>Microsoft. LabServices

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | labaccounts | Yes |
> | kullanıcılar | No |

## <a name="microsoftlogic"></a>Microsoft. Logic

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | hostingEnvironments | Yes |
> | Tümleştirme hesapları | Yes |
> | ıntegrationserviceortamortamları | Yes |
> | ıntegrationserviceortamortamları/managedap | Yes |
> | ısotedenvironments | Yes |
> | sürdürülen | Yes |

## <a name="microsoftmachinelearning"></a>Microsoft. Machinöğrenim

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | Commitmentplanlar | Yes |
> | Hizmetleri | Yes |
> | Çalışma alanları | Yes |

## <a name="microsoftmachinelearningservices"></a>Microsoft.MachineLearningServices

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | çalışma alanı | Yes |
> | çalışma alanları/hesaplar | No |
> | çalışma alanları/eventGridFilters | No |

## <a name="microsoftmaintenance"></a>Microsoft. Maintenance

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | applyUpdates | No |
> | Configurationatamalar | No |
> | maintenanceConfigurations | Yes |
> | güncelleştirmeler | No |

## <a name="microsoftmanagedidentity"></a>Microsoft. Managedıdentity

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | Kimlikler | No |
> | Userassignedıdentities | Yes |

## <a name="microsoftmanagednetwork"></a>Microsoft. ManagedNetwork

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | managedNetworks | Yes |
> | managedNetworks/managedNetworkGroups | Yes |
> | managedNetworks/managedNetworkPeeringPolicies | Yes |
> | bildirim | Yes |

## <a name="microsoftmanagedservices"></a>Microsoft. ManagedServices

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | Pazar Placeryumuristrationdefinitions | No |
> | Registrationatamaları | No |
> | registrationDefinitions | No |

## <a name="microsoftmanagement"></a>Microsoft. Management

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | getEntities | No |
> | Yönetim grupları | No |
> | managementGroups/Settings | No |
> | kaynaklar | No |
> | startTenantBackfill | No |
> | tenantBackfillStatus | No |

## <a name="microsoftmaps"></a>Microsoft. Maps

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | accounts | Yes |
> | hesaplar/eventGridFilters | No |
> | hesaplar/privateAtlases | Yes |

## <a name="microsoftmarketplace"></a>Microsoft. Market

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | sunar | No |
> | offerTypes | No |
> | offerTypes/yayımcılar | No |
> | offerTypes/yayımcılar/teklifler | No |
> | offerTypes/yayımcılar/teklifler/planlar | No |
> | offerTypes/yayımcılar/teklifler/planlar/anlaşmalar | No |
> | offertypes/yayımcılar/teklifler/planlar/yapılandırmalarını | No |
> | offertypes/yayımcılar/teklifler/planlar/yapılandırmalarını/ımportımage | No |
> | privategallergıtems | No |
> | privateStoreClient | No |
> | Privatemağazaların | No |
> | Privatemağazaların/tekliflerin | No |
> | ürün | No |
> | Publishers | No |
> | Yayımcılar/teklifler | No |
> | Yayımcılar/teklifler/Düzeltme | No |
> | register | No |

## <a name="microsoftmarketplaceapps"></a>Microsoft. MarketplaceApps

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | classicDevServices | Yes |
> | updateCommunicationPreference | No |

## <a name="microsoftmarketplaceordering"></a>Microsoft. Marketplacesıralaması

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | anlaşmalar | No |
> | offertypes | No |

## <a name="microsoftmedia"></a>Microsoft. Media

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | mediaservices | Yes |
> | mediaservices/accountFilters | No |
> | mediaservices/varlıklar | No |
> | mediaservices/varlıklar/assetFilters | No |
> | mediaservices/contentKeyPolicies | No |
> | mediaservices/eventGridFilters | No |
> | mediaservices/liveEventOperations | No |
> | mediaservices/liveEvents | Yes |
> | mediaservices/liveEvents/Liveçıktılar | No |
> | mediaservices/liveEvents/Privateendpointconnectionproxy 'Leri | No |
> | mediaservices/liveOutputOperations | No |
> | mediaservices/Mediagraf | No |
> | mediaservices/streamingEndpointOperations | No |
> | mediaservices/streamingEndpoints | Yes |
> | mediaservices/streamingEndpoints/Privateendpointconnectionproxy 'Leri | No |
> | mediaservices/Streamingkonumlandırıcı | No |
> | mediaservices/streamingPolicies | No |
> | mediaservices/streamingPrivateEndpointConnectionProxyOperations | No |
> | mediaservices/dönüşümler | No |
> | mediaservices/dönüşümler/işler | No |

## <a name="microsoftmicroservices4spring"></a>Microsoft. Microservices4Spring

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | Appkümeler | Yes |

## <a name="microsoftmigrate"></a>Microsoft. Migrate

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | assessmentProjects | Yes |
> | migrateprojects | Yes |
> | moveCollections | Yes |
> | projeyle | Yes |

## <a name="microsoftmixedreality"></a>Microsoft. MixedReality

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | holographicsBroadcastAccounts | Yes |
> | objectUnderstandingAccounts | Yes |
> | remoteRenderingAccounts | Yes |
> | spatialAnchorsAccounts | Yes |

## <a name="microsoftnetapp"></a>Microsoft. NetApp

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | netAppAccounts | Yes |
> | netAppAccounts/accountBackups | No |
> | netAppAccounts/Capacityhavuzları | Yes |
> | netAppAccounts/Capacityhavuzları/birimleri | Yes |
> | netAppAccounts/Capacityhavuzlar/birimler/anlık görüntüler | Yes |
## <a name="microsoftnetwork"></a>Microsoft.Network

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | Applicationgateway 'ler | Yes |
> | applicationGatewayWebApplicationFirewallPolicies | Yes |
> | applicationSecurityGroups | Yes |
> | azureFirewallFqdnTags | No |
> | azureFirewalls | Yes |
> | Savunma Konakları | Yes |
> | bgpServiceCommunities | No |
> | bağlantının | Yes |
> | ddosCustomPolicies | Yes |
> | Ddosprotectionplanlar | Yes |
> | Dnsoperationdurumları | No |
> | dnszones | Yes |
> | dnszones/A | No |
> | dnszones/AAAA | No |
> | dnszones/tümü | No |
> | dnszones/CAA | No |
> | dnszones/CNAME | No |
> | dnszones/MX | No |
> | dnszones/NS | No |
> | dnszones/PTR | No |
> | dnszones/kayıt kümeleri | No |
> | dnszones/SOA | No |
> | dnszones/SRV | No |
> | dnszones/TXT | No |
> | Expressroutedevreleri | Yes |
> | expressRouteCrossConnections | Yes |
> | Expressroutegateway 'ler | Yes |
> | expressRoutePorts | Yes |
> | expressRouteServiceProviders | No |
> | firewallPolicies | Yes |
> | frontkapıların | Yes |
> | frontdoorWebApplicationFirewallManagedRuleSets | No |
> | frontdoorWebApplicationFirewallPolicies | Yes |
> | getDnsResourceReference | No |
> | ınternalnotify | No |
> | loadBalancers | Yes |
> | Localnetworkgateway 'ler | Yes |
> | Natgateway 'ler | Yes |
> | Networkıntpolicies Ilkeleri | Yes |
> | NetworkInterfaces | Yes |
> | networkProfiles | Yes |
> | networkSecurityGroups | Yes |
> | networkWatchers | Yes |
> | networkWatchers/Connectionmonitörleri | Yes |
> | networkWatchers/flowLogs | Yes |
> | networkWatchers/uzunluler | Yes |
> | networkWatchers/Pingkafesler | Yes |
> | p2sVpnGateways | Yes |
> | privateDnsOperationStatuses | No |
> | privateDnsZones | Yes |
> | privateDnsZones/A | No |
> | privateDnsZones/AAAA | No |
> | privateDnsZones/tümü | No |
> | privateDnsZones/CNAME | No |
> | privateDnsZones/MX | No |
> | privateDnsZones/PTR | No |
> | privateDnsZones/SOA | No |
> | privateDnsZones/SRV | No |
> | privateDnsZones/TXT | No |
> | privateDnsZones/virtualNetworkLinks | Yes |
> | privateEndpoints | Yes |
> | privateLinkServices | Yes |
> | Publicıpaddresses | Yes |
> | Publicıpöneklerini | Yes |
> | routeFilters | Yes |
> | routeTables | Yes |
> | serviceEndpointPolicies | Yes |
> | trafficManagerGeographicHierarchies | No |
> | trafficmanagerprofiles | Yes |
> | trafficmanagerprofiles/heatMaps | No |
> | trafficManagerUserMetricsKeys | No |
> | Virtualhub 'Lar | Yes |
> | virtualNetworkGateways | Yes |
> | virtualNetworks | Yes |
> | virtualNetworks/alt ağları | No |
> | virtualNetworkTaps | Yes |
> | Virtualwan | Yes |
> | Vpngateway 'ler | Yes |
> | vpnSites | Yes |
> | webApplicationFirewallPolicies | Yes |

## <a name="microsoftnotebooks"></a>Microsoft. Not defterleri

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | Not defteri proxy 'leri | No |

## <a name="microsoftnotificationhubs"></a>Microsoft. Notificationhub 'Lar

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | öznitelikleri | Yes |
> | ad alanları/Notificationhub 'Lar | Yes |

## <a name="microsoftobjectstore"></a>Microsoft. ObjectStore

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | osNamespaces | Yes |

## <a name="microsoftoffazure"></a>Microsoft. OffAzure

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | Hiper sanal siteler | Yes |
> | Importsites | Yes |
> | Sunucusiteleri | Yes |
> | VMwareSites | Yes |

## <a name="microsoftoperationalinsights"></a>Microsoft. Operationalınsights

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | leriniz | Yes |
> | deletedWorkspaces | No |
> | Bağlantı hedefleri | No |
> | Storageınsii configs | No |
> | çalışma alanı | Yes |
> | çalışma alanları/veri dışarı aktarmaları | No |
> | çalışma alanları/veri kaynakları | No |
> | çalışma alanları/linkedServices | No |
> | çalışma alanları/linkedStorageAccounts | No |
> | çalışma alanları/meta veriler | No |
> | çalışma alanları/sorgu | No |
> | çalışma alanları/Scopedprivatelinkproxy 'Leri | No |

## <a name="microsoftoperationsmanagement"></a>Microsoft. OperationsManagement

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | managementassociations | No |
> | managementconfigurations | Yes |
> | çözümler | Yes |
> | görünümler | Yes |

## <a name="microsoftpeering"></a>Microsoft. eşleme

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | Yasallıklar | No |
> | peerAsns | No |
> | eşlemeleri | Yes |
> | Peeringserviceülkeleriyle | No |
> | peeringServiceProviders | No |
> | peeringServices | Yes |

## <a name="microsoftpolicyinsights"></a>Microsoft.PolicyInsights

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | Poliyevents | No |
> | policyMetadata | No |
> | policyStates | No |
> | policyTrackedResources | No |
> | düzeltmeler | No |

## <a name="microsoftportal"></a>Microsoft. Portal

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> |  konsolları | No |
> | panolar | Yes |
> | userSettings | No |

## <a name="microsoftpowerbi"></a>Microsoft. PowerBI

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | workspaceCollections | Yes |

## <a name="microsoftpowerbidedicated"></a>Microsoft. Powerbiadanmış

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | kapasiteler | Yes |

## <a name="microsoftprojectbabylon"></a>Microsoft. ProjectBabylon

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | accounts | Yes |

## <a name="microsoftproviderhub"></a>Microsoft. ProviderHub

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | Providerkayıtları | No |
> | Providerkayıtlarıyla/resourceTypeRegistrations | No |
> | piyasaya çıkarma | Yes |

## <a name="microsoftquantum"></a>Microsoft. hisse

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | Çalışma alanları | Yes |

## <a name="microsoftrecoveryservices"></a>Microsoft. RecoveryServices

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | Backupkorunabilir | No |
> | kasaları | Yes |

## <a name="microsoftredhatopenshift"></a>Microsoft. RedHatOpenShift

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | OpenShiftClusters | Yes |

## <a name="microsoftrelay"></a>Microsoft. Relay

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | öznitelikleri | Yes |
> | ad alanları/authorizationrules | No |
> | ad alanları/hybridconnections | No |
> | ad alanları/hybridconnections/authorizationrules | No |
> | ad alanları/privateEndpointConnections | No |
> | ad alanları/wcfreyerleştiri | No |
> | ad alanları/wcfreyerleştirme/authorizationrules | No |

## <a name="microsoftresourcegraph"></a>Microsoft. ResourceGraph

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | lardır | Yes |
> | resourceChangeDetails | No |
> | resourceChanges | No |
> | kaynaklar | No |
> | resourcesHistory | No |
> | subscriptionsStatus | No |

## <a name="microsoftresourcehealth"></a>Microsoft. ResourceHealth

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | Kullanılabilirlik durumları | No |
> | Childadvailabilitydurumlar | No |
> | childResources | No |
> | acil sorun sorunları | No |
> | etkinlikler | No |
> | ımpactedresources | No |
> | meta veriler | No |
> | bildirimler | No |

## <a name="microsoftresources"></a>Microsoft. resources

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | calculateTemplateHash | No |
> | dağıtımlar | No |
> | dağıtımlar/işlemler | No |
> | deploymentScripts | Yes |
> | deploymentScripts/Günlükler | No |
> | Köprü | No |
> | notifyResourceJobs | No |
> | sağlayıcılarla | No |
> | resourceGroups | No |
> | Aboneliklerin | No |
> | Templatespec | Yes |
> | Templatespec/sürümler | Yes |
> | Kira | No |

## <a name="microsoftsaas"></a>Microsoft. SaaS

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | uygulamalar | Yes |
> | saasresources | No |

## <a name="microsoftsearch"></a>Microsoft. Search

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | resourceHealthMetadata | No |
> | searchServices | Yes |

## <a name="microsoftsecurity"></a>Microsoft.Security

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | adaptiveNetworkHardenings | No |
> | advancedThreatProtectionSettings | No |
> | alerts | No |
> | alertsSuppressionRules | No |
> | allowedConnections | No |
> | Applicationwhitedökümler | No |
> | assessmentMetadata | No |
> | değerlendirme | No |
> | Oto Dissalertsrules | No |
> | akışlarını otomatikleştirin | Yes |
> | Oto Provisioningsettings | No |
> | Uyumluluklarına | No |
> | dataCollectionAgents | No |
> | deviceSecurityGroups | No |
> | discoveredSecuritySolutions | No |
> | externalSecuritySolutions | No |
> | Informationprotectionpolicies | No |
> | ıotsecuritysolutions | Yes |
> | ıotsecuritysolutions/analiz Ticsmodeller | No |
> | ıotsecuritysolutions/Analticsmodeller/Aggreggıt uyarıları | No |
> | ıotsecuritysolutions/analiz Ticsmodeller/Aggreg, öneriler | No |
> | Jağaccesspolicies | No |
> | ilkeler | No |
> | fiyatlandırmalar | No |
> | Reve daha karmaşık bakım standartları | No |
> | Rekontrol ve Re, | No |
> | Rekontrol ve Re, Re, Re, | No |
> | secureScoreControlDefinitions | No |
> | secureScoreControls | No |
> | Securesçekirdekler | No |
> | Securesçekirdekler/secureScoreControls | No |
> | securityContacts | No |
> | securitySolutions | No |
> | securitySolutionsReferenceData | No |
> | Securitydurumlardan | No |
> | securityStatusesSummaries | No |
> | Sunucukullanılabilirliği | No |
> | ayarlar | No |
> | subAssessments | No |
> | Görevler | No |
> | anlatır | No |
> | çalışma alanı ayarları | No |

## <a name="microsoftsecuritygraph"></a>Microsoft. SecurityGraph

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | diagnosticSettings | No |
> | diagnosticSettingsCategories | No |

## <a name="microsoftsecurityinsights"></a>Microsoft. Securityınsights

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | toplamaları | No |
> | alertRules | No |
> | Alertrutatemplates | No |
> | automationRules | No |
> | yer işaretleri | No |
> | çalışmaların | No |
> | Veri bağlayıcıları | No |
> | dataConnectorsCheckRequirements | No |
> | varlıklar | No |
> | entityQueries | No |
> | olaylara | No |
> | officeConsents | No |
> | ayarlar | No |
> | Threatıntelligence | No |

## <a name="microsoftserialconsole"></a>Microsoft. SerialConsole

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | consoleServices | No |

## <a name="microsoftservicebus"></a>Microsoft.ServiceBus

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | öznitelikleri | Yes |
> | ad alanları/authorizationrules | No |
> | ad alanları/diskalrecoveryconfigs | No |
> | ad alanları/eventgridfilters | No |
> | ad alanları/networkrulesets | No |
> | ad alanları/kuyruklar | No |
> | ad alanları/kuyruklar/authorizationrules | No |
> | ad alanları/konular | No |
> | ad alanları/konular/authorizationrules | No |
> | ad alanları/konular/abonelikler | No |
> | ad alanları/konular/abonelikler/kurallar | No |
> | premiumMessagingRegions | No |

## <a name="microsoftservicefabric"></a>Microsoft. ServiceFabric

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | uygulamalar | Yes |
> | leriniz | Yes |
> | kümeler/uygulamalar | No |
> | Kapsayıcı grupları | Yes |
> | containerGroupSets | Yes |
> | edgeclusters | Yes |
> | edgeclusters/uygulamalar | No |
> | managedkümeler | Yes |
> | managedkümeler/nodetypes | No |
> | Mamak | Yes |
> | secretmağazaları | Yes |
> | secretmağazaları/sertifikaları | No |
> | secretmağazaları/gizli dizileri | No |
> | volumes | Yes |

## <a name="microsoftservicefabricmesh"></a>Microsoft. Servicefabrickafesi

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | uygulamalar | Yes |
> | Kapsayıcı grupları | Yes |
> | geçidinin | Yes |
> | Mamak | Yes |
> | kaynaklanır | Yes |
> | volumes | Yes |

## <a name="microsoftservices"></a>Microsoft. Services

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | Providerkayıtları | No |
> | Providerkayıtlarıyla/resourceTypeRegistrations | No |
> | piyasaya çıkarma | Yes |

## <a name="microsoftsignalrservice"></a>Microsoft. SignalRService

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | SignalR | Yes |
> | SignalR/eventGridFilters | No |

## <a name="microsoftsoftwareplan"></a>Microsoft. SoftwarePlan

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | Hybriduseavantajlar | No |

## <a name="microsoftsolutions"></a>Microsoft. Solutions

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | applicationDefinitions | Yes |
> | uygulamalar | Yes |
> | Jistekleri | Yes |

## <a name="microsoftsql"></a>Microsoft. SQL

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | ManagedInstances | Yes |
> | ManagedInstances/veritabanları | Yes |
> | ManagedInstances/veritabanları/backupShortTermRetentionPolicies | No |
> | ManagedInstances/veritabanları/şemalar/tablolar/sütunlar/sensitivityLabels | No |
> | ManagedInstances/veritabanları/ | No |
> | ManagedInstances/veritabanları/ek | No |
> | ManagedInstances/encryptionProtector | No |
> | ManagedInstances/anahtarlar | No |
> | ManagedInstances/Restokbledroppeddatabases/backupShortTermRetentionPolicies | No |
> | ManagedInstances/ | No |
> | larý | Yes |
> | sunucular/Yöneticiler | No |
> | sunucular/communicationLinks | No |
> | sunucular/veritabanları | Yes |
> | sunucular/encryptionProtector | No |
> | sunucular/firewallRules | No |
> | sunucular/anahtarlar | No |
> | sunucular/Restokbledroppeddatabases | No |
> | Sunucu/hizmet hedefleri | No |
> | sunucular/tdeCertificates | No |
> | Virtualkümeler | No |

## <a name="microsoftsqlvirtualmachine"></a>Microsoft. SqlVirtualMachine

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | SqlVirtualMachineGroups | Yes |
> | SqlVirtualMachineGroups/kullanılabilirliği Bilitygrouplisteners | No |
> | SqlVirtualMachines | Yes |

## <a name="microsoftstorage"></a>Microsoft.Storage

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | storageAccounts | Yes |
> | storageAccounts/blobServices | No |
> | storageAccounts/fileServices | No |
> | storageAccounts/queueServices | No |
> | storageAccounts/Services | No |
> | storageAccounts/Services/metricDefinitions | No |
> | storageAccounts/tableServices | No |
> | vardır | No |

## <a name="microsoftstoragecache"></a>Microsoft. StorageCache

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | önbelleklerinde | Yes |
> | önbellekler/Storagetaral | No |
> | Usagemodeller | No |

## <a name="microsoftstoragereplication"></a>Microsoft. Storagerepce

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | replicationGroups | No |

## <a name="microsoftstoragesync"></a>Microsoft. Storagessync

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | storageSyncServices | Yes |
> | storageSyncServices/registeredServers | No |
> | storageSyncServices/syncGroups | No |
> | storageSyncServices/syncGroups/cloudEndpoints | No |
> | storageSyncServices/syncGroups/serverEndpoints | No |
> | storageSyncServices/iş akışları | No |

## <a name="microsoftstoragesyncdev"></a>Microsoft. StorageSyncDev

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | storageSyncServices | Yes |
> | storageSyncServices/registeredServers | No |
> | storageSyncServices/syncGroups | No |
> | storageSyncServices/syncGroups/cloudEndpoints | No |
> | storageSyncServices/syncGroups/serverEndpoints | No |
> | storageSyncServices/iş akışları | No |

## <a name="microsoftstoragesyncint"></a>Microsoft. Storagesyncınt

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | storageSyncServices | Yes |
> | storageSyncServices/registeredServers | No |
> | storageSyncServices/syncGroups | No |
> | storageSyncServices/syncGroups/cloudEndpoints | No |
> | storageSyncServices/syncGroups/serverEndpoints | No |
> | storageSyncServices/iş akışları | No |

## <a name="microsoftstorsimple"></a>Microsoft. StorSimple

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | ilerinde | Yes |

## <a name="microsoftstreamanalytics"></a>Microsoft. StreamAnalytics

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | leriniz | Yes |
> | streammingjobs | Yes |

## <a name="microsoftsubscription"></a>Microsoft. Subscription

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | iptal | No |
> | CreateSubscription | No |
> | seçin | No |
> | yeniden adlandır | No |
> | SubscriptionDefinitions | No |
> | SubscriptionOperations | No |
> | Aboneliklerin | No |

## <a name="microsoftsynapse"></a>Microsoft. SYNAPSE

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | Privatelinkhub 'Ları | Yes |
> | çalışma alanı | Yes |
> | çalışma alanları/bigDataPools | Yes |
> | çalışma alanları/Operationdurumlar | No |
> | çalışma alanları/sqlPools | Yes |

## <a name="microsofttimeseriesinsights"></a>Microsoft. Timeseriesınsights

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | lý | Yes |
> | ortamlar/accessPolicies | No |
> | ortamlar/EventSources | Yes |
> | ortamlar/Referencedataset 'ler | Yes |

## <a name="microsofttoken"></a>Microsoft. Token

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | depolaya | Yes |
> | depolar/accessPolicies | No |
> | depolar/hizmetler | No |
> | depolar/hizmetler/belirteçler | No |

## <a name="microsoftvirtualmachineimages"></a>Microsoft. Virtualmachineımages

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | ımagetemplates | Yes |
> | ımagetemplates/Runçıktılar | No |

## <a name="microsoftvmware"></a>Microsoft. VMware

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | ArcZones | Yes |
> | ResourcePools | Yes |
> | VCenter | Yes |
> | VirtualMachines | Yes |
> | VirtualMachineTemplates | Yes |
> | VirtualNetworks | Yes |

## <a name="microsoftvmwarecloudsimple"></a>Microsoft. Vmwarechoparlör basit

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | ayrılmış Cloudnodes | Yes |
> | ayrılmış CloudService | Yes |
> | virtualMachines | Yes |

## <a name="microsoftvmwareonazure"></a>Microsoft. VMwareOnAzure

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | Privatebulutlar | Yes |

## <a name="microsoftvnfmanager"></a>Microsoft. VnfManager

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | cihazlar | Yes |
> | registeredSubscriptions | No |
> | larını | No |
> | satıcılar/SKU 'lar | No |
> | satıcılar/vnfs | No |
> | Virtualnetworkfunctionsku 'Ları | No |
> | vnfs | Yes |

## <a name="microsoftvsonline"></a>Microsoft. VSOnline

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | accounts | Yes |
> | Planlama | Yes |
> | registeredSubscriptions | No |

## <a name="microsoftweb"></a>Microsoft. Web

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | apiManagementAccounts | No |
> | apiManagementAccounts/Apiacl 'Ler | No |
> | apiManagementAccounts/API 'ler | No |
> | apiManagementAccounts/API/Apiacl 'Ler | No |
> | apiManagementAccounts/API/Connectionacl 'Ler | No |
> | apiManagementAccounts/API/bağlantı | No |
> | apiManagementAccounts/API/Connections/Connectionacl 'Ler | No |
> | apiManagementAccounts/API/localizedDefinitions | No |
> | apiManagementAccounts/Connectionacl 'Ler | No |
> | apiManagementAccounts/bağlantılar | No |
> | billingMeters | No |
> | sertifikalar | Yes |
> | Connectiongateway 'ler | Yes |
> | bağlantının | Yes |
> | Customapsıs | Yes |
> | Silinmi siteleri | No |
> | hostingEnvironments | Yes |
> | hostingEnvironments/eventGridFilters | No |
> | hostingEnvironments/multiRolePools | No |
> | hostingEnvironments/workerPools | No |
> | Kubeortamları | Yes |
> | publishingUsers | No |
> | Öneriler | No |
> | resourceHealthMetadata | No |
> | zamanları | No |
> | serverFarms | Yes |
> | Sunucugrupları/eventGridFilters | No |
> | Siteler | Yes |
> | siteler/yapılandırma  | No |
> | siteler/eventGridFilters | No |
> | siteler/hostNameBindings | No |
> | Sites/networkConfig | No |
> | siteler/premieraddons | Yes |
> | siteler/yuvalar | Yes |
> | siteler/yuvalar/eventGridFilters | No |
> | siteler/yuvalar/hostNameBindings | No |
> | siteler/yuvalar/networkConfig | No |
> | sourceControls | No |
> | staticSites | Yes |
> | doğrulamalısınız | No |
> | verifyHostingEnvironmentVnet | No |

## <a name="microsoftwindowsdefenderatp"></a>Microsoft. Windowssavunma Deratp

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | diagnosticSettings | No |
> | diagnosticSettingsCategories | No |

## <a name="microsoftwindowsesu"></a>Microsoft. WindowsESU

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | Çoğulactivationkeys | Yes |

## <a name="microsoftwindowsiot"></a>Microsoft. Windowsıot

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | DeviceServices | Yes |

## <a name="microsoftworkloadbuilder"></a>Microsoft. WorkloadBuilder

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | lerine | Yes |
> | iş yükleri/örnekler | No |
> | iş yükleri/sürümler | No |
> | iş yükleri/sürümler/yapılar | No |

## <a name="microsoftworkloadmonitor"></a>Microsoft. WorkloadMonitor

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | bileşenleri | No |
> | componentsSummary | No |
> | Izleme örnekleri | No |
> | Izleme ınstancessummary | No |
> | monitörün | No |
> | notificationSettings | Hayır |

## <a name="next-steps"></a>Sonraki adımlar

Aynı verileri bir virgülle ayrılmış değerler dosyası ile almak için [complete-mode-deletion.csv](https://github.com/tfitzmac/resource-capabilities/blob/master/complete-mode-deletion.csv)indirin.
