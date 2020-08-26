---
title: Tam modda silme
description: Azure Resource Manager şablonlarda kaynak türlerinin tamamlanma modu silme işlemini nasıl işleyeceğini gösterir.
ms.topic: conceptual
ms.date: 08/25/2020
ms.openlocfilehash: 08fed3420cf74d328dd9a92a6a2c9235c5d11300
ms.sourcegitcommit: b33c9ad17598d7e4d66fe11d511daa78b4b8b330
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/25/2020
ms.locfileid: "88855159"
---
# <a name="deletion-of-azure-resources-for-complete-mode-deployments"></a>Tüm mod dağıtımları için Azure kaynaklarını silme

Bu makalede, kaynak türlerinin, tamamlanmış modda dağıtılan bir şablonda olmadığında silinme işleminin nasıl işleneceği açıklanır.

Türü, tamamlanmış modla dağıtılan şablonda olmadığında, **Evet** ile işaretlenen kaynak türleri silinir.

**Hayır** ile işaretlenen kaynak türleri, şablonda olmadığında otomatik olarak silinmez; Ancak, üst kaynak silinirse bunlar silinir. Davranışın tam açıklaması için bkz. [Azure Resource Manager Dağıtım modları](deployment-modes.md).

[Bir şablonda birden fazla kaynak grubuna](cross-scope-deployment.md)dağıtırsanız, dağıtım işleminde belirtilen kaynak grubundaki kaynaklar silinebilir. İkincil kaynak gruplarındaki kaynaklar silinmez.

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
> - [Microsoft. oto Yönet](#microsoftautomanage)
> - [Microsoft. Automation](#microsoftautomation)
> - [Microsoft. AVS](#microsoftavs)
> - [Microsoft. Azure. Genfiliz](#microsoftazuregeneva)
> - [Microsoft. AzureActiveDirectory](#microsoftazureactivedirectory)
> - [Microsoft. AzureData](#microsoftazuredata)
> - [Microsoft. AzureStack](#microsoftazurestack)
> - [Microsoft. Azurestackhcı](#microsoftazurestackhci)
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
> - [Microsoft. Codespaces](#microsoftcodespaces)
> - [Microsoft. Biliveservices](#microsoftcognitiveservices)
> - [Microsoft. Commerce](#microsoftcommerce)
> - [Microsoft.Compute](#microsoftcompute)
> - [Microsoft. ConnectedCache](#microsoftconnectedcache)
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
> - [Microsoft.Devices](#microsoftdevices)
> - [Microsoft. DeviceUpdate](#microsoftdeviceupdate)
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
> | DomainServices/oucontainer | Hayır |

## <a name="microsoftaddons"></a>Microsoft. addons

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | Destek sağlayıcıları | Hayır |

## <a name="microsoftadhybridhealthservice"></a>Microsoft. ADHybridHealthService

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
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
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | Danışmanlaştırıp | Hayır |
> | konfigürasyonları | Hayır |
> | Generatereyorumgeçişleri | Hayır |
> | meta veriler | Hayır |
> | Öneriler | Hayır |
> | gizlemeleri | Hayır |

## <a name="microsoftalertsmanagement"></a>Microsoft. AlertsManagement

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | actionRules | Yes |
> | alerts | Hayır |
> | alertsList | Hayır |
> | alertsMetaData | Hayır |
> | alertsSummary | Hayır |
> | alertsSummaryList | Hayır |
> | smartDetectorAlertRules | Yes |
> | smartGroups | Hayır |

## <a name="microsoftanalysisservices"></a>Microsoft. AnalysisServices

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | larý | Yes |

## <a name="microsoftapimanagement"></a>Microsoft. Apimanane

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | reportFeedback | Hayır |
> | hizmet | Yes |
> | validateServiceName | Hayır |

## <a name="microsoftappconfiguration"></a>Microsoft. AppConfiguration

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | Configurationmağazaların | Yes |
> | Configurationmağazaların/eventGridFilters | Hayır |
> | Configurationmağazaların/keyValues | Hayır |

## <a name="microsoftappplatform"></a>Microsoft. AppPlatform

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | Spring | Yes |
> | Yay/uygulamalar | Hayır |
> | Yay/uygulamalar/dağıtımlar | Hayır |

## <a name="microsoftattestation"></a>Microsoft. kanıtlama

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | attestationProviders | Yes |
> | defaultProviders | Hayır |

## <a name="microsoftauthorization"></a>Microsoft.Authorization

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | Accessbelgeno Scheduledefinitions | Hayır |
> | Accessbelgeayarlarý Schedulesettings | Hayır |
> | classicAdministrators | Hayır |
> | Datatakma adlar | Hayır |
> | Denyasatamaları | Hayır |
> | Erişimi yükseltme | Hayır |
> | Findorphanroleatamalar | Hayır |
> | kaynaktaki | Hayır |
> | izinler | Hayır |
> | Poliyasatamaları | Hayır |
> | policyDefinitions | Hayır |
> | Policymuafiyet | Hayır |
> | policySetDefinitions | Hayır |
> | privateLinkAssociations | Hayır |
> | providerOperations | Hayır |
> | resourceManagementPrivateLinks | Yes |
> | roleAssignments | Hayır |
> | Roleatamasussusageölçümleri | Hayır |
> | roleDefinitions | Hayır |

## <a name="microsoftautomanage"></a>Microsoft. oto Yönet

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | accounts | Yes |
> | Configurationprofileatamalar | Hayır |
> | configurationProfilePreferences | Yes |

## <a name="microsoftautomation"></a>Microsoft. Automation

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | automationAccounts | Yes |
> | automationAccounts/Configurations | Yes |
> | automationAccounts/Jobs | Hayır |
> | automationAccounts/Privateendpointconnectionproxy 'Leri | Hayır |
> | automationAccounts/privateEndpointConnections | Hayır |
> | automationAccounts/privateLinkResources | Hayır |
> | automationAccounts/runbook 'lar | Yes |
> | automationAccounts/softwareUpdateConfigurations | Hayır |
> | automationAccounts/Web kancaları | Hayır |

## <a name="microsoftavs"></a>Microsoft. AVS

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | Privatebulutlar | Yes |
> | Privatebulutlar/yetkilendirmeler | Hayır |
> | Privatebulutlar/kümeler | Hayır |
> | Privatebulutlar/globalReachConnections | Hayır |
> | Privatebulutlar/hcxEnterpriseSites | Hayır |
> | Privatebulutlar/workloadNetworks | Hayır |
> | Privatebulutlar/workloadNetworks/dhcpConfigurations | Hayır |
> | Privatebulutlar/workloadNetworks/Gateway | Hayır |
> | Privatebulutlar/workloadNetworks/portMirroringProfiles | Hayır |
> | Privatebulutlar/workloadNetworks/segment | Hayır |
> | Privatebulutlar/workloadNetworks/virtualMachines | Hayır |
> | Privatebulutlar/workloadNetworks/vmGroups | Hayır |

## <a name="microsoftazuregeneva"></a>Microsoft. Azure. Genfiliz

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | lý | Hayır |
> | ortamlar/hesaplar | Hayır |
> | ortamlar/hesaplar/ad alanları | Hayır |
> | ortamlar/hesaplar/ad alanları/yapılandırma | Hayır |

## <a name="microsoftazureactivedirectory"></a>Microsoft. AzureActiveDirectory

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | b2cDirectories | Yes |
> | b2ctenants | Hayır |
> | Guestkullanımlar | Yes |

## <a name="microsoftazuredata"></a>Microsoft. AzureData

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | Veri denetleyicileri | Yes |
> | Postgresınstances | Yes |
> | Sqlmanagedınstances | Yes |
> | Sqlserverınstances | Yes |
> | Sqlserverkayıtları | Yes |
> | Sqlserverkayıtları/sqlServers | Hayır |

## <a name="microsoftazurestack"></a>Microsoft. AzureStack

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | cloudManifestFiles | Hayır |
> | edgeSubscriptions | Yes |
> | kayıtlarında | Yes |
> | kayıt/müşteri abonelikleri | Hayır |
> | kayıtlar/ürünler | Hayır |

## <a name="microsoftazurestackhci"></a>Microsoft. Azurestackhcı

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | leriniz | Yes |

## <a name="microsoftbatch"></a>Microsoft.Batch

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | batchAccounts | Yes |

## <a name="microsoftbilling"></a>Microsoft. Faturalandırma

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | billingAccounts | Hayır |
> | billingAccounts/anlaşmalar | Hayır |
> | billingAccounts/billingPermissions | Hayır |
> | billingAccounts/billingProfiles | Hayır |
> | billingAccounts/Billingprofiller/billingPermissions | Hayır |
> | billingAccounts/billingProfiles/Billingroleatamaları | Hayır |
> | billingAccounts/billingProfiles/billingRoleDefinitions | Hayır |
> | billingAccounts/billingProfiles/Billingabonelikleri | Hayır |
> | billingAccounts/billingProfiles/Createbillingroleatama | Hayır |
> | billingAccounts/billingProfiles/müşteriler | Hayır |
> | billingAccounts/billingProfiles/yönergeler | Hayır |
> | billingAccounts/billingProfiles/faturalar | Hayır |
> | billingAccounts/billingProfiles/faturalar/fiyat listesi | Hayır |
> | billingAccounts/billingProfiles/faturalar/işlemler | Hayır |
> | billingAccounts/billingProfiles/ınvoicesections | Hayır |
> | billingAccounts/billingProfiles/ınvoicesections/billingPermissions | Hayır |
> | billingAccounts/billingProfiles/ınvoicesections/Billingroleatamaları | Hayır |
> | billingAccounts/billingProfiles/ınvoicesections/billingRoleDefinitions | Hayır |
> | billingAccounts/billingProfiles/ınvoicesections/Billingabonelikleri | Hayır |
> | billingAccounts/billingProfiles/ınvoicesections/Createbillingroleatama | Hayır |
> | billingAccounts/billingProfiles/ınvoicesections/ınitiatetransfer | Hayır |
> | billingAccounts/billingProfiles/ınvoicesections/ürünler | Hayır |
> | billingAccounts/billingProfiles/ınvoicesections/ürünler/aktarım | Hayır |
> | billingAccounts/billingProfiles/ınvoicesections/ürünler/updateAutoRenew | Hayır |
> | billingAccounts/billingProfiles/ınvoicesections/işlemler | Hayır |
> | billingAccounts/billingProfiles/ınvoicesections/aktarımlar | Hayır |
> | billingAccounts/BillingProfiles/patchOperations | Hayır |
> | billingAccounts/billingProfiles/paymentMethods | Hayır |
> | billingAccounts/billingProfiles/ilkeler | Hayır |
> | billingAccounts/billingProfiles/fiyat listesi | Hayır |
> | billingAccounts/billingProfiles/pricesheetDownloadOperations | Hayır |
> | billingAccounts/billingProfiles/ürünler | Hayır |
> | billingAccounts/billingProfiles/işlemler | Hayır |
> | billingAccounts/billingProfiles/validateDetachPaymentMethodEligibility | Hayır |
> | billingAccounts/Billingroleatamaları | Hayır |
> | billingAccounts/billingRoleDefinitions | Hayır |
> | billingAccounts/Billingabonelikleri | Hayır |
> | billingAccounts/Billingabonelikleri/faturalar | Hayır |
> | billingAccounts/Createbillingroleatama | Hayır |
> | billingAccounts/Createınvoicesectionoperations | Hayır |
> | billingAccounts/müşteriler | Hayır |
> | billingAccounts/müşteriler/billingPermissions | Hayır |
> | billingAccounts/müşteriler/Billingabonelikleri | Hayır |
> | billingAccounts/müşteriler/ınitiatetransfer | Hayır |
> | billingAccounts/müşteriler/ilkeler | Hayır |
> | billingAccounts/müşteriler/ürünler | Hayır |
> | billingAccounts/müşteriler/işlemler | Hayır |
> | billingAccounts/müşteriler/aktarımlar | Hayır |
> | billingAccounts/departmanlar | Hayır |
> | billingAccounts/departmanlar/billingPermissions | Hayır |
> | billingAccounts/departmanlar/Billingroleatamaları | Hayır |
> | billingAccounts/departmanlar/billingRoleDefinitions | Hayır |
> | billingAccounts/KayıtSayısı | Hayır |
> | billingAccounts/Kayıthesapsayısı/billingPermissions | Hayır |
> | billingAccounts/KayıtSayısı/Billingroleatamaları | Hayır |
> | billingAccounts/KayıtSayısı/billingRoleDefinitions | Hayır |
> | billingAccounts/faturalar | Hayır |
> | billingAccounts/faturalar/işlemler | Hayır |
> | billingAccounts/ınvoicesections | Hayır |
> | billingAccounts/ınvoicesections/billingSubscriptionMoveOperations | Hayır |
> | billingAccounts/ınvoicesections/Billingabonelikleri | Hayır |
> | billingAccounts/ınvoicesections/Billingabonelikleri/aktarımı | Hayır |
> | billingAccounts/ınvoicesections/yükselt | Hayır |
> | billingAccounts/ınvoicesections/ınitiatetransfer | Hayır |
> | billingAccounts/ınvoicesections/patchOperations | Hayır |
> | billingAccounts/ınvoicesections/productMoveOperations | Hayır |
> | billingAccounts/Ürünler/Ürünler | Hayır |
> | billingAccounts/ınvoicesections/ürünler/transfer | Hayır |
> | billingAccounts/ınvoicesections/ürünler/updateAutoRenew | Hayır |
> | billingAccounts/ınvoicesections/işlemler | Hayır |
> | billingAccounts/ınvoicesections/aktarımlar | Hayır |
> | billingAccounts/Lineofkredisi | Hayır |
> | billingAccounts/patchOperations | Hayır |
> | billingAccounts/paymentMethods | Hayır |
> | billingAccounts/ürünler | Hayır |
> | billingAccounts/işlemler | Hayır |
> | Billingdönemler | Hayır |
> | billingPermissions | Hayır |
> | billingProperty | Hayır |
> | Billingroleatamaları | Hayır |
> | billingRoleDefinitions | Hayır |
> | Createbillingroleatama | Hayır |
> | bölümlerinin | Hayır |
> | kayıt sayısı | Hayır |
> | faturalardan | Hayır |
> | girişinde | Hayır |
> | aktarımlar/acceptTransfer | Hayır |
> | aktarımlar/declineTransfer | Hayır |
> | aktarımlar/operationStatus | Hayır |
> | aktarımlar/validateTransfer | Hayır |
> | validateAddress | Hayır |

## <a name="microsoftbingmaps"></a>Microsoft. BingMaps

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | Mapapsıs | Yes |
> | updateCommunicationPreference | Hayır |

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
> | TokenServices/BlockchainNetworks | Hayır |
> | TokenServices/Groups | Hayır |
> | TokenServices/gruplar/hesaplar | Hayır |
> | TokenServices/TokenTemplates | Hayır |

## <a name="microsoftblueprint"></a>Microsoft. Blueprint

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | Şema tasmi | Hayır |
> | Blueprintasbir/Atamaperations | Hayır |
> | Blueprintasbir/işlemleri | Hayır |
> | Blueprint | Hayır |
> | planlar/yapıtlar | Hayır |
> | planlar/sürümler | Hayır |
> | planlar/sürümler/yapılar | Hayır |

## <a name="microsoftbotservice"></a>Microsoft. BotService

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | botServices | Yes |
> | botServices/kanallar | Hayır |
> | botServices/Connections | Hayır |
> | diller | Hayır |
> | templates | Hayır |

## <a name="microsoftcache"></a>Microsoft. Cache

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | Redis | Yes |
> | Redsıs/EventGridFilters | Hayır |
> | Redsıs/Privateendpointconnectionproxy 'Leri | Hayır |
> | Redsıs/Privateendpointconnectionproxy/doğrulama | Hayır |
> | Redsıs/privateEndpointConnections | Hayır |
> | Redsıs/privateLinkResources | Hayır |
> | redisEnterprise | Yes |

## <a name="microsoftcapacity"></a>Microsoft. Capacity

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | appliedReservations | Hayır |
> | Oto Kotaartışı | Hayır |
> | calculateExchange | Hayır |
> | calculatePrice | Hayır |
> | calculatePurchasePrice | Hayır |
> | larına | Hayır |
> | Ticari Vaalrezervler | Hayır |
> | değişimi | Hayır |
> | placePurchaseOrder | Hayır |
> | Rezervler | Hayır |
> | Rezervler/Hesaplaizterefund | Hayır |
> | Rezervler/Birleştir | Hayır |
> | Rezervler/rezervasyonlar | Hayır |
> | Rezervler/rezervasyonlar/düzeltmeler | Hayır |
> | Rezervler/geri dönüş | Hayır |
> | Rezervler/Böl | Hayır |
> | Rezervler/takas | Hayır |
> | oluşturamaz | Hayır |
> | resourceProviders | Hayır |
> | kaynaklar | Hayır |
> | validateReservationOrder | Hayır |

## <a name="microsoftcdn"></a>Microsoft. CDN

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | CdnWebApplicationFirewallManagedRuleSets | Hayır |
> | CdnWebApplicationFirewallPolicies | Yes |
> | edgenodes | Hayır |
> | lerinize | Yes |
> | Profiller/uç noktalar | Yes |
> | Profiller/uç noktalar/customdomains | Hayır |
> | Profiller/uç noktalar/origingroups | Hayır |
> | Profiller/uç noktalar/kaynaklar | Hayır |
> | Validatearaştırması | Hayır |

## <a name="microsoftcertificateregistration"></a>Microsoft. CertificateRegistration

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | Sertifikadüzenleri | Yes |
> | certificateOrders/Certificates | Hayır |
> | Validatecertificateregistrationınformation | Hayır |

## <a name="microsoftchangeanalysis"></a>Microsoft. ChangeAnalysis

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | profil | Hayır |
> | resourceChanges | Hayır |

## <a name="microsoftclassiccompute"></a>Microsoft.ClassicCompute

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | özellikler | Hayır |
> | domainNames | Yes |
> | domainNames/yetenekleri | Hayır |
> | domainNames/internalLoadBalancers | Hayır |
> | domainNames/serviceCertificates | Hayır |
> | domainNames/Yuvaları | Hayır |
> | domainNames/yuvalar/roller | Hayır |
> | domainNames/yuvalar/roller/metricDefinitions | Hayır |
> | domainNames/yuvalar/roller/ölçümler | Hayır |
> | moveSubscriptionResources | Hayır |
> | operatingSystemFamilies | Hayır |
> | operatingSystems | Hayır |
> | quotas | Hayır |
> | resourceTypes | Hayır |
> | Validatesubscriptionmoveavaılabılıty | Hayır |
> | virtualMachines | Yes |
> | virtualMachines/diagnosticSettings | Hayır |
> | virtualMachines/metricDefinitions | Hayır |
> | virtualMachines/ölçümler | Hayır |

## <a name="microsoftclassicinfrastructuremigrate"></a>Microsoft. ClassicInfrastructureMigrate

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | classicInfrastructureResources | Hayır |

## <a name="microsoftclassicnetwork"></a>Microsoft. ClassicNetwork

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | özellikler | Hayır |
> | expressRouteCrossConnections | Hayır |
> | expressRouteCrossConnections/peerler | Hayır |
> | gatewaySupportedDevices | Hayır |
> | networkSecurityGroups | Yes |
> | quotas | Hayır |
> | Rezervler | Yes |
> | virtualNetworks | Yes |
> | virtualNetworks/Remotevirtualnetworkpeeringproxy 'Leri | Hayır |
> | virtualNetworks/Virtualnetworkpeerler | Hayır |

## <a name="microsoftclassicstorage"></a>Microsoft. ClassicStorage

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | özellikler | Hayır |
> | disklerinden | Hayır |
> | images | Hayır |
> | osImages | Hayır |
> | Osplatformımages | Hayır |
> | Publicımages | Hayır |
> | quotas | Hayır |
> | storageAccounts | Yes |
> | storageAccounts/blobServices | Hayır |
> | storageAccounts/fileServices | Hayır |
> | storageAccounts/metricDefinitions | Hayır |
> | storageAccounts/ölçümler | Hayır |
> | storageAccounts/queueServices | Hayır |
> | storageAccounts/Services | Hayır |
> | storageAccounts/Services/diagnosticSettings | Hayır |
> | storageAccounts/Services/metricDefinitions | Hayır |
> | storageAccounts/Services/ölçümler | Hayır |
> | storageAccounts/tableServices | Hayır |
> | storageAccounts/Vmımages | Hayır |
> | Vmımages | Hayır |

## <a name="microsoftcodespaces"></a>Microsoft. Codespaces

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | Planlama | Yes |
> | registeredSubscriptions | Hayır |

## <a name="microsoftcognitiveservices"></a>Microsoft. Biliveservices

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | accounts | Yes |
> | hesaplar/Privateendpointconnectionproxy 'Leri | Hayır |
> | hesaplar/privateEndpointConnections | Hayır |
> | hesaplar/privateLinkResources | Hayır |

## <a name="microsoftcommerce"></a>Microsoft. Commerce

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | RateCard | Hayır |
> | Usagetoplamaları | Hayır |

## <a name="microsoftcompute"></a>Microsoft.Compute

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | availabilitySets | Yes |
> | cloudServices | Yes |
> | Diskeriþler | Yes |
> | diskEncryptionSets | Yes |
> | disklerinden | Yes |
> | Galeriler | Yes |
> | Galeriler/uygulamalar | Hayır |
> | Galeriler/uygulamalar/sürümler | Hayır |
> | Galeriler/görüntüler | Hayır |
> | Galeriler/resimler/sürümler | Hayır |
> | hostGroups | Yes |
> | hostGroups/konaklar | Yes |
> | images | Yes |
> | proximityPlacementGroups | Yes |
> | restorePointCollections | Yes |
> | restorePointCollections/restorePoints | Hayır |
> | sharedVMExtensions | Yes |
> | sharedVMExtensions/sürümler | Hayır |
> | Sharedvmımages | Yes |
> | Sharedvmımages/sürümler | Hayır |
> | anlık görüntüler | Yes |
> | sshPublicKeys | Yes |
> | virtualMachines | Yes |
> | virtualMachines/uzantıları | Yes |
> | virtualMachines/metricDefinitions | Hayır |
> | virtualMachines/runCommands | Yes |
> | virtualMachineScaleSets | Yes |
> | virtualMachineScaleSets/uzantılar | Hayır |
> | virtualMachineScaleSets/NetworkInterfaces | Hayır |
> | virtualMachineScaleSets/Publicıpaddresses | Hayır |
> | virtualMachineScaleSets/virtualMachines | Hayır |
> | virtualMachineScaleSets/virtualMachines/NetworkInterfaces | Hayır |

## <a name="microsoftconnectedcache"></a>Microsoft. ConnectedCache

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | Önbellekenodes | Yes |

## <a name="microsoftconsumption"></a>Microsoft. tüketim

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | Aggregmalyt maliyeti | Hayır |
> | Bakiyeler | Hayır |
> | Bütçeler | Hayır |
> | Ücretler | Hayır |
> | CostTags | Hayır |
> | iler | Hayır |
> | etkinlikler | Hayır |
> | Tahminler | Hayır |
> | oluş | Hayır |
> | Marketlerinden | Hayır |
> | Fiyat listeleri | Hayır |
> | ürün | Hayır |
> | Rezervde ayrıntıları | Hayır |
> | ReservationRecommendationDetails | Hayır |
> | Rezervationönerilere | Hayır |
> | Rezervlerin Özeti | Hayır |
> | Rezervlik Işlemleri | Hayır |
> | Etiketler | Hayır |
> | Kira | Hayır |
> | Terimler | Hayır |
> | UsageDetails | Hayır |

## <a name="microsoftcontainerinstance"></a>Microsoft. Containerınstance

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | Kapsayıcı grupları | Yes |
> | serviceAssociationLinks | Hayır |

## <a name="microsoftcontainerregistry"></a>Microsoft. ContainerRegistry

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | kayıt | Yes |
> | kayıt defterleri/agentPools | Yes |
> | kayıt defterleri/derlemeler | Hayır |
> | kayıt defterleri/derlemeler/iptal | Hayır |
> | kayıt defterleri/derlemeler/getLogLink | Hayır |
> | kayıt defterleri/buildTasks | Yes |
> | kayıt defterleri/buildTasks/Steps | Hayır |
> | kayıt defterleri/eventGridFilters | Hayır |
> | kayıt defterleri/Exporthatlarının | Hayır |
> | kayıt defterleri/generateCredentials | Hayır |
> | kayıt defterleri/getBuildSourceUploadUrl 'Si | Hayır |
> | kayıt defterleri/GetCredentials | Hayır |
> | kayıt defterleri/ımportımage | Hayır |
> | kayıt defterleri/ımporthatlarının | Hayır |
> | kayıt defterleri/ardışık düzen eylemsizlik | Hayır |
> | kayıt defterleri/Privateendpointconnectionproxy 'Leri | Hayır |
> | kayıt defterleri/Privateendpointconnectionproxy/doğrulama | Hayır |
> | kayıt defterleri/privateEndpointConnections | Hayır |
> | kayıt defterleri/privateLinkResources | Hayır |
> | kayıt defterleri/queueBuild | Hayır |
> | kayıt defterleri/regenerateCredential | Hayır |
> | kayıt defterleri/regenerateCredentials | Hayır |
> | kayıt defterleri/çoğaltmalar | Yes |
> | kayıt defterleri/çalıştırmalar | Hayır |
> | kayıt defterleri/çalıştırmalar/iptal | Hayır |
> | kayıt defterleri/scheduleRun | Hayır |
> | kayıt defterleri/Kapsameşlemler | Hayır |
> | kayıt defterleri/Taskçalıştırmaları | Hayır |
> | kayıt defterleri/görevler | Yes |
> | kayıt defterleri/belirteçler | Hayır |
> | kayıt defterleri/updatePolicies | Hayır |
> | kayıt defterleri/Web kancaları | Yes |
> | kayıt defterleri/Web kancaları/getCallbackConfig | Hayır |
> | kayıt defterleri/Web kancaları/ping | Hayır |

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
> | Uyarılar | Hayır |
> | BillingAccounts | Hayır |
> | Bütçeler | Hayır |
> | Cloudbağlayıcıları | Hayır |
> | Bağlayıcılar | Yes |
> | costAllocationRules | Hayır |
> | Departmanlar | Hayır |
> | Boyutlar | Hayır |
> | Kayıt sayısı | Hayır |
> | Aktarımları | Hayır |
> | ExternalBillingAccounts | Hayır |
> | ExternalBillingAccounts/uyarılar | Hayır |
> | ExternalBillingAccounts/Boyutlar | Hayır |
> | ExternalBillingAccounts/tahmin | Hayır |
> | ExternalBillingAccounts/sorgu | Hayır |
> | Externalabonelikleri | Hayır |
> | Externalabonelikleri/uyarıları | Hayır |
> | Externalabonelikler/Boyutlar | Hayır |
> | Externalabonelikler/tahmin | Hayır |
> | Externalabonelikler/sorgu | Hayır |
> | Tahmin | Hayır |
> | Insights | Hayır |
> | Sorgu | Hayır |
> | register | Hayır |
> | Reportconfigs | Hayır |
> | Raporlar | Hayır |
> | Ayarlar | Hayır |
> | showbackRules | Hayır |
> | Görünümler | Hayır |

## <a name="microsoftcustomerlockbox"></a>Microsoft. Customerkasası

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | istekleri | Hayır |

## <a name="microsoftcustomproviders"></a>Microsoft. CustomProviders

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | içermektedir | Hayır |
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
> | çalışma alanları/dbWorkspaces | Hayır |
> | çalışma alanları/storageEncryption | Hayır |
> | çalışma alanları/Virtualnetworkpeerler | Hayır |

## <a name="microsoftdatacatalog"></a>Microsoft. DataCatalog

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | larına | Yes |

## <a name="microsoftdatafactory"></a>Microsoft. DataFactory

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | Veri fabrikaları | Yes |
> | DataFactory/diagnosticSettings | Hayır |
> | DataFactory/metricDefinitions | Hayır |
> | dataFactorySchema | Hayır |
> | larının | Yes |
> | Fabrika/tümleştirme çalışma zamanları | Hayır |

## <a name="microsoftdatalakeanalytics"></a>Microsoft. DataLakeAnalytics

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | accounts | Yes |
> | hesaplar/dataLakeStoreAccounts | Hayır |
> | hesaplar/storageAccounts | Hayır |
> | hesaplar/storageAccounts/kapsayıcılar | Hayır |
> | hesaplar/Transferanaliz tici | Hayır |

## <a name="microsoftdatalakestore"></a>Microsoft. DataLakeStore

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | accounts | Yes |
> | hesaplar/eventGridFilters | Hayır |
> | hesaplar/firewallRules | Hayır |

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
> | Resourceoperationgatedenetleyicileri | Yes |

## <a name="microsoftdatashare"></a>Microsoft. DataShare

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | accounts | Yes |
> | hesaplar/paylaşımlar | Hayır |
> | hesaplar/paylaşımlar/veri kümeleri | Hayır |
> | hesaplar/paylaşımlar/davetler | Hayır |
> | hesaplar/paylaşımlar/providersharesubscriptions | Hayır |
> | hesaplar/paylaşımlar/synchronizationSettings | Hayır |
> | hesaplar/parçalar esubscriptions | Hayır |
> | hesaplar/parçalar esubscriptions/Consumersourcedataset 'ler | Hayır |
> | hesaplar/parçalar esubscriptions/datasetmappings | Hayır |
> | hesaplar/parçalar esubscriptions/Tetikleyiciler | Hayır |

## <a name="microsoftdbformariadb"></a>Microsoft. Dbformarıdb

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | larý | Yes |
> | sunucular/danışmanları | Hayır |
> | sunucular/anahtarlar | Hayır |
> | sunucular/Privateendpointconnectionproxy 'Leri | Hayır |
> | sunucular/privateEndpointConnections | Hayır |
> | sunucular/privateLinkResources | Hayır |
> | sunucular/Querymetinmetinleri | Hayır |
> | sunucular/recoverableServers | Hayır |
> | sunucular/topQueryStatistics | Hayır |
> | sunucular/virtualNetworkRules | Hayır |
> | sunucular/waitStatistics | Hayır |

## <a name="microsoftdbformysql"></a>Microsoft. Dbformyısql

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | larý | Yes |
> | sunucular/danışmanları | Hayır |
> | sunucular/anahtarlar | Hayır |
> | sunucular/Privateendpointconnectionproxy 'Leri | Hayır |
> | sunucular/privateEndpointConnections | Hayır |
> | sunucular/privateLinkResources | Hayır |
> | sunucular/Querymetinmetinleri | Hayır |
> | sunucular/recoverableServers | Hayır |
> | sunucular/topQueryStatistics | Hayır |
> | sunucular/virtualNetworkRules | Hayır |
> | sunucular/waitStatistics | Hayır |

## <a name="microsoftdbforpostgresql"></a>Microsoft. DBforPostgreSQL

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | Flexibtaservers | Yes |
> | Sunucu grupları | Yes |
> | larý | Yes |
> | sunucular/danışmanları | Hayır |
> | sunucular/anahtarlar | Hayır |
> | sunucular/Privateendpointconnectionproxy 'Leri | Hayır |
> | sunucular/privateEndpointConnections | Hayır |
> | sunucular/privateLinkResources | Hayır |
> | sunucular/Querymetinmetinleri | Hayır |
> | sunucular/recoverableServers | Hayır |
> | sunucular/topQueryStatistics | Hayır |
> | sunucular/virtualNetworkRules | Hayır |
> | sunucular/waitStatistics | Hayır |
> | serversv2 | Yes |

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
> | applicationgroups/uygulamalar | Hayır |
> | applicationgroups/masaüstleri | Hayır |
> | applicationgroups/startmenuıtems | Hayır |
> | Ana bilgisayar havuzları | Yes |
> | hostpools/oturumkonakları | Hayır |
> | hostpools/sessionkonakları/usersessions | Hayır |
> | hosthavuzlar/usersessions | Hayır |
> | çalışma alanı | Yes |

## <a name="microsoftdevices"></a>Microsoft.Devices

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | Elaun havuzları | Yes |
> | Elaun havuzları/ıothubkiracılar | Yes |
> | Elaor havuzları/ıothubkiracılar/securitySettings | Hayır |
> | Iothubs | Yes |
> | IotHubs/eventGridFilters | Hayır |
> | IotHubs/securitySettings | Hayır |
> | ProvisioningServices | Yes |
> | vardır | Hayır |

## <a name="microsoftdeviceupdate"></a>Microsoft. DeviceUpdate

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | accounts | Yes |
> | hesaplar/örnekler | Yes |

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
> | Digitaltwınsınstances/endpoints | Hayır |

## <a name="microsoftdocumentdb"></a>Microsoft.DocumentDB

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | databaseAccountNames | Hayır |
> | Veritabanı hesapları | Yes |
> | Restokıbledatabaseaccounts | Hayır |

## <a name="microsoftdomainregistration"></a>Microsoft. DomainRegistration

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | etki alanları | Yes |
> | Domains/Domainownershiptanýmlayýcýlarý | Hayır |
> | generateSsoRequest | Hayır |
> | topLevelDomains | Hayır |
> | Validatedomainregistrationınformation | Hayır |

## <a name="microsoftdynamicslcs"></a>Microsoft. DynamicsLcs

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | lcsprojects | Hayır |
> | lcsprojects/clouddağıtımları | Hayır |
> | lcsprojects/bağlayıcılar | Hayır |

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
> | etki alanları/konular | Hayır |
> | Eventabonelikleri | Hayır |
> | Extensionkonuları | Hayır |
> | partnerNamespaces | Yes |
> | partnerNamespaces/eventChannels | Hayır |
> | iş ortağı kayıtları | Yes |
> | iş ortağı konuları | Yes |
> | iş ortağı konuları/Eventabonelikleri | Hayır |
> | Sistem konuları | Yes |
> | Sistem konuları/Eventabonelikler | Hayır |
> | konuları | Yes |
> | topicTypes | Hayır |

## <a name="microsofteventhub"></a>Microsoft. EventHub

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | leriniz | Yes |
> | öznitelikleri | Yes |
> | ad alanları/authorizationrules | Hayır |
> | ad alanları/diskalrecoveryconfigs | Hayır |
> | ad alanları/eventhubs | Hayır |
> | ad alanları/eventhubs/authorizationrules | Hayır |
> | ad alanları/eventhubs/consumergroups | Hayır |
> | ad alanları/networkrulesets | Hayır |

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
> | Özellik sağlayıcıları | Hayır |
> | özellikler | Hayır |
> | sağlayıcılarla | Hayır |
> | subscriptionFeatureRegistrations | Hayır |

## <a name="microsoftgallery"></a>Microsoft. Gallery

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | kaydedemez | Hayır |
> | gallergıtems | Hayır |
> | generateartifactaccessuri | Hayır |
> | myarea | Hayır |
> | myarea/alan | Hayır |
> | myarea/alan/alan | Hayır |
> | myareas/Areas/Areas/gallergıtems | Hayır |
> | myareas/Areas/gallergıtems | Hayır |
> | myarea/gallergıtems | Hayır |
> | register | Hayır |
> | kaynaklar | Hayır |
> | elde edilecek esourcesbyıd | Hayır |

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
> | Configurationprofileatamalar | Hayır |
> | Guestconfigurationatamaları | Hayır |
> | yazılımıdır | Hayır |
> | softwareUpdateProfile | Hayır |
> | softwareUpdates | Hayır |

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
> | kümeler/uygulamalar | Hayır |

## <a name="microsofthealthcareapis"></a>Microsoft. Healthgelişme API 'leri

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | services | Yes |
> | Hizmetler/ıomtbağlayıcılar | Hayır |
> | Hizmetler/ıomtbağlayıcılar/bağlantılar | Hayır |
> | Hizmetler/ıomtbağlayıcılar/eşlemeler | Hayır |
> | Hizmetler/Privateendpointconnectionproxy 'Leri | Hayır |
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
> | registeredSubscriptions | Hayır |
> | larını | Hayır |
> | satıcılar/vendorsku 'ları | Hayır |
> | satıcılar/vendorsku 'ları/önizleme abonelikleri | Hayır |
> | virtualnetworkfunctions | Yes |
> | virtualnetworkfunctionsatıcılarını | Hayır |

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
> | diagnosticSettings | Hayır |
> | diagnosticSettingsCategories | Hayır |

## <a name="microsoftiotcentral"></a>Microsoft. ıotcentral

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | appTemplates | Hayır |
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
> | Silinkaults | Hayır |
> | hsmPools | Yes |
> | managedHSMs | Yes |
> | kasaları | Yes |
> | kasa/erişim Ilkeleri | Hayır |
> | kasa/eventGridFilters | Hayır |
> | kasa/gizlilikler | Hayır |

## <a name="microsoftkubernetes"></a>Microsoft. Kubernetes

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | Connectedkümeler | Yes |
> | registeredSubscriptions | Hayır |

## <a name="microsoftkubernetesconfiguration"></a>Microsoft. KubernetesConfiguration

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | uzantılardan | Hayır |
> | sourceControlConfigurations | Hayır |

## <a name="microsoftkusto"></a>Microsoft.Kusto

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | leriniz | Yes |
> | kümeler/attacheddatabaseconfigurations | Hayır |
> | kümeler/veritabanları | Hayır |
> | kümeler/veritabanları/veri bağlantıları | Hayır |
> | kümeler/veritabanları/eventhubconnections | Hayır |
> | kümeler/veritabanları/princıpalasbir | Hayır |
> | kümeler/veri bağlantıları | Hayır |
> | kümeler/princıpalasbir | Hayır |
> | kümeler/parçalar | Hayır |

## <a name="microsoftlabservices"></a>Microsoft. LabServices

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | labaccounts | Yes |
> | kullanıcılar | Hayır |

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
> | çalışma alanları/hesaplar | Hayır |
> | çalışma alanları/eventGridFilters | Hayır |
> | çalışma alanları/ınenceendpoints | Yes |
> | çalışma alanları/ınenceendpoints/dağıtımlar | Yes |
> | çalışma alanları/linkedServices | Hayır |

## <a name="microsoftmaintenance"></a>Microsoft. Maintenance

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | applyUpdates | Hayır |
> | Configurationatamalar | Hayır |
> | maintenanceConfigurations | Yes |
> | publicMaintenanceConfigurations | Hayır |
> | güncelleştirmeler | Hayır |

## <a name="microsoftmanagedidentity"></a>Microsoft. Managedıdentity

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | Kimlikler | Hayır |
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
> | Pazar Placeryumuristrationdefinitions | Hayır |
> | Registrationatamaları | Hayır |
> | registrationDefinitions | Hayır |

## <a name="microsoftmanagement"></a>Microsoft. Management

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | getEntities | Hayır |
> | Yönetim grupları | Hayır |
> | managementGroups/Settings | Hayır |
> | kaynaklar | Hayır |
> | startTenantBackfill | Hayır |
> | tenantBackfillStatus | Hayır |

## <a name="microsoftmaps"></a>Microsoft. Maps

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | accounts | Yes |
> | hesaplar/eventGridFilters | Hayır |
> | hesaplar/privateAtlases | Yes |

## <a name="microsoftmarketplace"></a>Microsoft. Market

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | Macc | Hayır |
> | sunar | Hayır |
> | offerTypes | Hayır |
> | offerTypes/yayımcılar | Hayır |
> | offerTypes/yayımcılar/teklifler | Hayır |
> | offerTypes/yayımcılar/teklifler/planlar | Hayır |
> | offerTypes/yayımcılar/teklifler/planlar/anlaşmalar | Hayır |
> | offertypes/yayımcılar/teklifler/planlar/yapılandırmalarını | Hayır |
> | offertypes/yayımcılar/teklifler/planlar/yapılandırmalarını/ımportımage | Hayır |
> | privategallergıtems | Hayır |
> | privateStoreClient | Hayır |
> | Privatemağazaların | Hayır |
> | Privatemağazaların/tekliflerin | Hayır |
> | ürün | Hayır |
> | Publishers | Hayır |
> | Yayımcılar/teklifler | Hayır |
> | Yayımcılar/teklifler/Düzeltme | Hayır |
> | register | Hayır |

## <a name="microsoftmarketplaceapps"></a>Microsoft. MarketplaceApps

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | classicDevServices | Yes |
> | updateCommunicationPreference | Hayır |

## <a name="microsoftmarketplaceordering"></a>Microsoft. Marketplacesıralaması

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | anlaşmalar | Hayır |
> | offertypes | Hayır |

## <a name="microsoftmedia"></a>Microsoft. Media

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | mediaservices | Yes |
> | mediaservices/accountFilters | Hayır |
> | mediaservices/varlıklar | Hayır |
> | mediaservices/varlıklar/assetFilters | Hayır |
> | mediaservices/contentKeyPolicies | Hayır |
> | mediaservices/eventGridFilters | Hayır |
> | mediaservices/liveEventOperations | Hayır |
> | mediaservices/liveEvents | Yes |
> | mediaservices/liveEvents/Liveçıktılar | Hayır |
> | mediaservices/liveOutputOperations | Hayır |
> | mediaservices/Mediagraf | Hayır |
> | mediaservices/privateEndpointConnectionOperations | Hayır |
> | mediaservices/Privateendpointconnectionproxy 'Leri | Hayır |
> | mediaservices/privateEndpointConnections | Hayır |
> | mediaservices/streamingEndpointOperations | Hayır |
> | mediaservices/streamingEndpoints | Yes |
> | mediaservices/Streamingkonumlandırıcı | Hayır |
> | mediaservices/streamingPolicies | Hayır |
> | mediaservices/dönüşümler | Hayır |
> | mediaservices/dönüşümler/işler | Hayır |

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
> | netAppAccounts/accountBackups | Hayır |
> | netAppAccounts/Capacityhavuzları | Yes |
> | netAppAccounts/Capacityhavuzları/birimleri | Yes |
> | netAppAccounts/Capacityhavuzlar/birimler/anlık görüntüler | Hayır |
## <a name="microsoftnetwork"></a>Microsoft.Network

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | Applicationgateway 'ler | Yes |
> | applicationGatewayWebApplicationFirewallPolicies | Yes |
> | applicationSecurityGroups | Yes |
> | azureFirewallFqdnTags | Hayır |
> | azureFirewalls | Yes |
> | Savunma Konakları | Yes |
> | bgpServiceCommunities | Hayır |
> | bağlantının | Yes |
> | ddosCustomPolicies | Yes |
> | Ddosprotectionplanlar | Yes |
> | Dnsoperationdurumları | Hayır |
> | dnszones | Yes |
> | dnszones/A | Hayır |
> | dnszones/AAAA | Hayır |
> | dnszones/tümü | Hayır |
> | dnszones/CAA | Hayır |
> | dnszones/CNAME | Hayır |
> | dnszones/MX | Hayır |
> | dnszones/NS | Hayır |
> | dnszones/PTR | Hayır |
> | dnszones/kayıt kümeleri | Hayır |
> | dnszones/SOA | Hayır |
> | dnszones/SRV | Hayır |
> | dnszones/TXT | Hayır |
> | Expressroutedevreleri | Yes |
> | expressRouteCrossConnections | Yes |
> | Expressroutegateway 'ler | Yes |
> | expressRoutePorts | Yes |
> | expressRouteServiceProviders | Hayır |
> | firewallPolicies | Yes |
> | frontkapıların | Yes |
> | frontdoorWebApplicationFirewallManagedRuleSets | Hayır |
> | frontdoorWebApplicationFirewallPolicies | Yes |
> | getDnsResourceReference | Hayır |
> | ınternalnotify | Hayır |
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
> | privateDnsOperationStatuses | Hayır |
> | privateDnsZones | Yes |
> | privateDnsZones/A | Hayır |
> | privateDnsZones/AAAA | Hayır |
> | privateDnsZones/tümü | Hayır |
> | privateDnsZones/CNAME | Hayır |
> | privateDnsZones/MX | Hayır |
> | privateDnsZones/PTR | Hayır |
> | privateDnsZones/SOA | Hayır |
> | privateDnsZones/SRV | Hayır |
> | privateDnsZones/TXT | Hayır |
> | privateDnsZones/virtualNetworkLinks | Yes |
> | privateEndpoints | Yes |
> | privateLinkServices | Yes |
> | Publicıpaddresses | Yes |
> | Publicıpöneklerini | Yes |
> | routeFilters | Yes |
> | routeTables | Yes |
> | serviceEndpointPolicies | Yes |
> | trafficManagerGeographicHierarchies | Hayır |
> | trafficmanagerprofiles | Yes |
> | trafficmanagerprofiles/heatMaps | Hayır |
> | trafficManagerUserMetricsKeys | Hayır |
> | Virtualhub 'Lar | Yes |
> | virtualNetworkGateways | Yes |
> | virtualNetworks | Yes |
> | virtualNetworks/alt ağları | Hayır |
> | virtualNetworkTaps | Yes |
> | Virtualwan | Yes |
> | Vpngateway 'ler | Yes |
> | vpnSites | Yes |
> | webApplicationFirewallPolicies | Yes |

## <a name="microsoftnotebooks"></a>Microsoft. Not defterleri

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | Not defteri proxy 'leri | Hayır |

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
> | MasterSites | Yes |
> | Sunucusiteleri | Yes |
> | VMwareSites | Yes |

## <a name="microsoftoperationalinsights"></a>Microsoft. Operationalınsights

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | leriniz | Yes |
> | deletedWorkspaces | Hayır |
> | Bağlantı hedefleri | Hayır |
> | Storageınsii configs | Hayır |
> | çalışma alanı | Yes |
> | çalışma alanları/veri dışarı aktarmaları | Hayır |
> | çalışma alanları/veri kaynakları | Hayır |
> | çalışma alanları/linkedServices | Hayır |
> | çalışma alanları/linkedStorageAccounts | Hayır |
> | çalışma alanları/meta veriler | Hayır |
> | çalışma alanları/sorgu | Hayır |
> | çalışma alanları/Scopedprivatelinkproxy 'Leri | Hayır |

## <a name="microsoftoperationsmanagement"></a>Microsoft. OperationsManagement

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | managementassociations | Hayır |
> | managementconfigurations | Yes |
> | çözümler | Yes |
> | görünümler | Yes |

## <a name="microsoftpeering"></a>Microsoft. eşleme

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | Yasallıklar | Hayır |
> | peerAsns | Hayır |
> | eşlemeleri | Yes |
> | Peeringserviceülkeleriyle | Hayır |
> | peeringServiceProviders | Hayır |
> | peeringServices | Yes |

## <a name="microsoftpolicyinsights"></a>Microsoft. Poliyelei

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | belirlediğimizi karşıladığımızı | Hayır |
> | Poliyevents | Hayır |
> | policyMetadata | Hayır |
> | policyStates | Hayır |
> | policyTrackedResources | Hayır |
> | düzeltmeler | Hayır |

## <a name="microsoftportal"></a>Microsoft. Portal

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> |  konsolları | Hayır |
> | panolar | Yes |
> | userSettings | Hayır |

## <a name="microsoftpowerbi"></a>Microsoft. PowerBI

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | Privatelinkservicesforpowerbı | Yes |
> | Kira | Yes |
> | Kiracılar/çalışma alanları | Hayır |
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
> | Providerkayıtları | Hayır |
> | Providerkayıtları/Defaultrollout | Hayır |
> | Providerkayıtlarıyla/resourceTypeRegistrations | Hayır |
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
> | Backupkorunabilir | Hayır |
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
> | ad alanları/authorizationrules | Hayır |
> | ad alanları/hybridconnections | Hayır |
> | ad alanları/hybridconnections/authorizationrules | Hayır |
> | ad alanları/privateEndpointConnections | Hayır |
> | ad alanları/wcfreyerleştiri | Hayır |
> | ad alanları/wcfreyerleştirme/authorizationrules | Hayır |

## <a name="microsoftresourcegraph"></a>Microsoft. ResourceGraph

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | lardır | Yes |
> | resourceChangeDetails | Hayır |
> | resourceChanges | Hayır |
> | kaynaklar | Hayır |
> | resourcesHistory | Hayır |
> | subscriptionsStatus | Hayır |

## <a name="microsoftresourcehealth"></a>Microsoft. ResourceHealth

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | Kullanılabilirlik durumları | Hayır |
> | Childadvailabilitydurumlar | Hayır |
> | childResources | Hayır |
> | acil sorun sorunları | Hayır |
> | etkinlikler | Hayır |
> | ımpactedresources | Hayır |
> | meta veriler | Hayır |
> | bildirimler | Hayır |

## <a name="microsoftresources"></a>Microsoft. resources

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | calculateTemplateHash | Hayır |
> | dağıtımlar | Hayır |
> | dağıtımlar/işlemler | Hayır |
> | deploymentScripts | Yes |
> | deploymentScripts/Günlükler | Hayır |
> | Köprü | Hayır |
> | notifyResourceJobs | Hayır |
> | sağlayıcılarla | Hayır |
> | resourceGroups | Hayır |
> | Aboneliklerin | Hayır |
> | Templatespec | Yes |
> | Templatespec/sürümler | Yes |
> | Kira | Hayır |

## <a name="microsoftsaas"></a>Microsoft. SaaS

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | uygulamalar | Yes |
> | saasresources | Hayır |

## <a name="microsoftsearch"></a>Microsoft. Search

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | resourceHealthMetadata | Hayır |
> | searchServices | Yes |

## <a name="microsoftsecurity"></a>Microsoft.Security

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | adaptiveNetworkHardenings | Hayır |
> | advancedThreatProtectionSettings | Hayır |
> | alerts | Hayır |
> | alertsSuppressionRules | Hayır |
> | allowedConnections | Hayır |
> | Applicationwhitedökümler | Hayır |
> | assessmentMetadata | Hayır |
> | değerlendirme | Hayır |
> | Oto Dissalertsrules | Hayır |
> | akışlarını otomatikleştirin | Yes |
> | Oto Provisioningsettings | Hayır |
> | Uyumluluklarına | Hayır |
> | bağlayıcılar | Hayır |
> | dataCollectionAgents | Hayır |
> | deviceSecurityGroups | Hayır |
> | discoveredSecuritySolutions | Hayır |
> | externalSecuritySolutions | Hayır |
> | Informationprotectionpolicies | Hayır |
> | ıotsecuritysolutions | Yes |
> | ıotsecuritysolutions/analiz Ticsmodeller | Hayır |
> | ıotsecuritysolutions/Analticsmodeller/Aggreggıt uyarıları | Hayır |
> | ıotsecuritysolutions/analiz Ticsmodeller/Aggreg, öneriler | Hayır |
> | ıotsecuritysolutions/ıotalerts | Hayır |
> | ıotsecuritysolutions/ıotalerttypes | Hayır |
> | ıotsecuritysolutions/ıotönerilere | Hayır |
> | ıotsecuritysolutions/iotRecommendationTypes | Hayır |
> | Jağaccesspolicies | Hayır |
> | Jyer Ilkeleri | Hayır |
> | ilkeler | Hayır |
> | fiyatlandırmalar | Hayır |
> | Reve daha karmaşık bakım standartları | Hayır |
> | Rekontrol ve Re, | Hayır |
> | Rekontrol ve Re, Re, Re, | Hayır |
> | secureScoreControlDefinitions | Hayır |
> | secureScoreControls | Hayır |
> | Securesçekirdekler | Hayır |
> | Securesçekirdekler/secureScoreControls | Hayır |
> | securityContacts | Hayır |
> | securitySolutions | Hayır |
> | securitySolutionsReferenceData | Hayır |
> | Securitydurumlardan | Hayır |
> | securityStatusesSummaries | Hayır |
> | Sunucukullanılabilirliği | Hayır |
> | ayarlar | Hayır |
> | subAssessments | Hayır |
> | Görevler | Hayır |
> | anlatır | Hayır |
> | çalışma alanı ayarları | Hayır |

## <a name="microsoftsecuritygraph"></a>Microsoft. SecurityGraph

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | diagnosticSettings | Hayır |
> | diagnosticSettingsCategories | Hayır |

## <a name="microsoftsecurityinsights"></a>Microsoft. Securityınsights

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | toplamaları | Hayır |
> | alertRules | Hayır |
> | Alertrutatemplates | Hayır |
> | automationRules | Hayır |
> | yer işaretleri | Hayır |
> | çalışmaların | Hayır |
> | Veri bağlayıcıları | Hayır |
> | dataConnectorsCheckRequirements | Hayır |
> | varlıklar | Hayır |
> | entityQueries | Hayır |
> | olaylar | Hayır |
> | officeConsents | Hayır |
> | ayarlar | Hayır |
> | Threatıntelligence | Hayır |
> | watchlists | Hayır |

## <a name="microsoftserialconsole"></a>Microsoft. SerialConsole

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | consoleServices | Hayır |

## <a name="microsoftservicebus"></a>Microsoft.ServiceBus

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | öznitelikleri | Yes |
> | ad alanları/authorizationrules | Hayır |
> | ad alanları/diskalrecoveryconfigs | Hayır |
> | ad alanları/eventgridfilters | Hayır |
> | ad alanları/networkrulesets | Hayır |
> | ad alanları/kuyruklar | Hayır |
> | ad alanları/kuyruklar/authorizationrules | Hayır |
> | ad alanları/konular | Hayır |
> | ad alanları/konular/authorizationrules | Hayır |
> | ad alanları/konular/abonelikler | Hayır |
> | ad alanları/konular/abonelikler/kurallar | Hayır |
> | premiumMessagingRegions | Hayır |

## <a name="microsoftservicefabric"></a>Microsoft. ServiceFabric

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | uygulamalar | Yes |
> | leriniz | Yes |
> | kümeler/uygulamalar | Hayır |
> | Kapsayıcı grupları | Yes |
> | containerGroupSets | Yes |
> | edgeclusters | Yes |
> | edgeclusters/uygulamalar | Hayır |
> | managedkümeler | Yes |
> | managedkümeler/nodetypes | Hayır |
> | Mamak | Yes |
> | secretmağazaları | Yes |
> | secretmağazaları/sertifikaları | Hayır |
> | secretmağazaları/gizli dizileri | Hayır |
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
> | Providerkayıtları | Hayır |
> | Providerkayıtlarıyla/resourceTypeRegistrations | Hayır |
> | piyasaya çıkarma | Yes |

## <a name="microsoftsignalrservice"></a>Microsoft. SignalRService

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | SignalR | Yes |
> | SignalR/eventGridFilters | Hayır |

## <a name="microsoftsoftwareplan"></a>Microsoft. SoftwarePlan

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | Hybriduseavantajlar | Hayır |

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
> | ManagedInstances/veritabanları/backupShortTermRetentionPolicies | Hayır |
> | ManagedInstances/veritabanları/şemalar/tablolar/sütunlar/sensitivityLabels | Hayır |
> | ManagedInstances/veritabanları/ | Hayır |
> | ManagedInstances/veritabanları/ek | Hayır |
> | ManagedInstances/encryptionProtector | Hayır |
> | ManagedInstances/anahtarlar | Hayır |
> | ManagedInstances/Restokbledroppeddatabases/backupShortTermRetentionPolicies | Hayır |
> | ManagedInstances/ | Hayır |
> | larý | Yes |
> | sunucular/Yöneticiler | Hayır |
> | sunucular/communicationLinks | Hayır |
> | sunucular/veritabanları | Yes |
> | sunucular/encryptionProtector | Hayır |
> | sunucular/firewallRules | Hayır |
> | sunucular/anahtarlar | Hayır |
> | sunucular/Restokbledroppeddatabases | Hayır |
> | Sunucu/hizmet hedefleri | Hayır |
> | sunucular/tdeCertificates | Hayır |
> | Virtualkümeler | Hayır |

## <a name="microsoftsqlvirtualmachine"></a>Microsoft. SqlVirtualMachine

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | SqlVirtualMachineGroups | Yes |
> | SqlVirtualMachineGroups/kullanılabilirliği Bilitygrouplisteners | Hayır |
> | SqlVirtualMachines | Yes |

## <a name="microsoftstorage"></a>Microsoft.Storage

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | storageAccounts | Yes |
> | storageAccounts/blobServices | Hayır |
> | storageAccounts/fileServices | Hayır |
> | storageAccounts/queueServices | Hayır |
> | storageAccounts/Services | Hayır |
> | storageAccounts/Services/metricDefinitions | Hayır |
> | storageAccounts/tableServices | Hayır |
> | vardır | Hayır |

## <a name="microsoftstoragecache"></a>Microsoft. StorageCache

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | önbelleklerinde | Yes |
> | önbellekler/Storagetaral | Hayır |
> | Usagemodeller | Hayır |

## <a name="microsoftstoragereplication"></a>Microsoft. Storagerepce

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | replicationGroups | Hayır |

## <a name="microsoftstoragesync"></a>Microsoft. Storagessync

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | storageSyncServices | Yes |
> | storageSyncServices/registeredServers | Hayır |
> | storageSyncServices/syncGroups | Hayır |
> | storageSyncServices/syncGroups/cloudEndpoints | Hayır |
> | storageSyncServices/syncGroups/serverEndpoints | Hayır |
> | storageSyncServices/iş akışları | Hayır |

## <a name="microsoftstoragesyncdev"></a>Microsoft. StorageSyncDev

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | storageSyncServices | Yes |
> | storageSyncServices/registeredServers | Hayır |
> | storageSyncServices/syncGroups | Hayır |
> | storageSyncServices/syncGroups/cloudEndpoints | Hayır |
> | storageSyncServices/syncGroups/serverEndpoints | Hayır |
> | storageSyncServices/iş akışları | Hayır |

## <a name="microsoftstoragesyncint"></a>Microsoft. Storagesyncınt

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | storageSyncServices | Yes |
> | storageSyncServices/registeredServers | Hayır |
> | storageSyncServices/syncGroups | Hayır |
> | storageSyncServices/syncGroups/cloudEndpoints | Hayır |
> | storageSyncServices/syncGroups/serverEndpoints | Hayır |
> | storageSyncServices/iş akışları | Hayır |

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
> | acceptChangeTenant | Hayır |
> | deyim | Hayır |
> | iptal | Hayır |
> | changeTenantRequest | Hayır |
> | changeTenantStatus | Hayır |
> | CreateSubscription | Hayır |
> | seçin | Hayır |
> | yeniden adlandır | Hayır |
> | SubscriptionDefinitions | Hayır |
> | SubscriptionOperations | Hayır |
> | Aboneliklerin | Hayır |

## <a name="microsoftsynapse"></a>Microsoft. SYNAPSE

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | Privatelinkhub 'Ları | Yes |
> | çalışma alanı | Yes |
> | çalışma alanları/bigDataPools | Yes |
> | çalışma alanları/Operationdurumlar | Hayır |
> | çalışma alanları/sqlDatabases | Yes |
> | çalışma alanları/sqlPools | Yes |

## <a name="microsofttimeseriesinsights"></a>Microsoft. Timeseriesınsights

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | lý | Yes |
> | ortamlar/accessPolicies | Hayır |
> | ortamlar/EventSources | Yes |
> | ortamlar/Referencedataset 'ler | Yes |

## <a name="microsofttoken"></a>Microsoft. Token

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | depolaya | Yes |
> | depolar/accessPolicies | Hayır |
> | depolar/hizmetler | Hayır |
> | depolar/hizmetler/belirteçler | Hayır |

## <a name="microsoftvirtualmachineimages"></a>Microsoft. Virtualmachineımages

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | ımagetemplates | Yes |
> | ımagetemplates/Runçıktılar | Hayır |

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

## <a name="microsoftvnfmanager"></a>Microsoft. VnfManager

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | cihazlar | Yes |
> | registeredSubscriptions | Hayır |
> | larını | Hayır |
> | satıcılar/SKU 'lar | Hayır |
> | satıcılar/vnfs | Hayır |
> | Virtualnetworkfunctionsku 'Ları | Hayır |
> | vnfs | Yes |

## <a name="microsoftvsonline"></a>Microsoft. VSOnline

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | accounts | Yes |
> | Planlama | Yes |
> | registeredSubscriptions | Hayır |

## <a name="microsoftweb"></a>Microsoft. Web

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | apiManagementAccounts | Hayır |
> | apiManagementAccounts/Apiacl 'Ler | Hayır |
> | apiManagementAccounts/API 'ler | Hayır |
> | apiManagementAccounts/API/Apiacl 'Ler | Hayır |
> | apiManagementAccounts/API/Connectionacl 'Ler | Hayır |
> | apiManagementAccounts/API/bağlantı | Hayır |
> | apiManagementAccounts/API/Connections/Connectionacl 'Ler | Hayır |
> | apiManagementAccounts/API/localizedDefinitions | Hayır |
> | apiManagementAccounts/Connectionacl 'Ler | Hayır |
> | apiManagementAccounts/bağlantılar | Hayır |
> | billingMeters | Hayır |
> | sertifikalar | Yes |
> | Connectiongateway 'ler | Yes |
> | bağlantının | Yes |
> | Customapsıs | Yes |
> | Silinmi siteleri | Hayır |
> | hostingEnvironments | Yes |
> | hostingEnvironments/eventGridFilters | Hayır |
> | hostingEnvironments/multiRolePools | Hayır |
> | hostingEnvironments/workerPools | Hayır |
> | Kubeortamları | Yes |
> | publishingUsers | Hayır |
> | Öneriler | Hayır |
> | resourceHealthMetadata | Hayır |
> | zamanları | Hayır |
> | serverFarms | Yes |
> | Sunucugrupları/eventGridFilters | Hayır |
> | Siteler | Yes |
> | siteler/yapılandırma  | Hayır |
> | siteler/eventGridFilters | Hayır |
> | siteler/hostNameBindings | Hayır |
> | Sites/networkConfig | Hayır |
> | siteler/premieraddons | Yes |
> | siteler/yuvalar | Yes |
> | siteler/yuvalar/eventGridFilters | Hayır |
> | siteler/yuvalar/hostNameBindings | Hayır |
> | siteler/yuvalar/networkConfig | Hayır |
> | sourceControls | Hayır |
> | staticSites | Yes |
> | doğrulamalısınız | Hayır |
> | verifyHostingEnvironmentVnet | Hayır |

## <a name="microsoftwindowsdefenderatp"></a>Microsoft. Windowssavunma Deratp

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | diagnosticSettings | Hayır |
> | diagnosticSettingsCategories | Hayır |

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
> | iş yükleri/örnekler | Hayır |
> | iş yükleri/sürümler | Hayır |
> | iş yükleri/sürümler/yapılar | Hayır |

## <a name="microsoftworkloadmonitor"></a>Microsoft. WorkloadMonitor

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | bileşenleri | Hayır |
> | componentsSummary | Hayır |
> | Izleme örnekleri | Hayır |
> | Izleme ınstancessummary | Hayır |
> | monitörün | Hayır |
> | notificationSettings | Hayır |

## <a name="next-steps"></a>Sonraki adımlar

Aynı verileri bir virgülle ayrılmış değerler dosyası ile almak için [complete-mode-deletion.csv](https://github.com/tfitzmac/resource-capabilities/blob/master/complete-mode-deletion.csv)indirin.
