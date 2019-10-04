---
title: Kaynak türüne göre tamamlanmış mod silme Azure Resource Manager
description: Azure Resource Manager şablonlarda kaynak türlerinin tamamlanma modu silme işlemini nasıl işleyeceğini gösterir.
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: conceptual
ms.date: 10/03/2019
ms.author: tomfitz
ms.openlocfilehash: cdc00ccc61e20865e993a18c72b930d9570896f2
ms.sourcegitcommit: f2d9d5133ec616857fb5adfb223df01ff0c96d0a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/03/2019
ms.locfileid: "71937044"
---
# <a name="deletion-of-azure-resources-for-complete-mode-deployments"></a>Tüm mod dağıtımları için Azure kaynaklarını silme

Bu makalede, kaynak türlerinin, tamamlanmış modda dağıtılan bir şablonda olmadığında silinme işleminin nasıl işleneceği açıklanır.

Türü, tamamlanmış modla dağıtılan şablonda olmadığında, **Evet** ile işaretlenen kaynak türleri silinir.

**Hayır** ile işaretlenen kaynak türleri, şablonda olmadığında otomatik olarak silinmez; Ancak, üst kaynak silinirse bunlar silinir. Davranışın tam açıklaması için bkz. [Azure Resource Manager Dağıtım modları](deployment-modes.md).

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
> - [Microsoft. Authorization](#microsoftauthorization)
> - [Microsoft. Automation](#microsoftautomation)
> - [Microsoft. Azconfig](#microsoftazconfig)
> - [Microsoft. Azure. Genfiliz](#microsoftazuregeneva)
> - [Microsoft. AzureActiveDirectory](#microsoftazureactivedirectory)
> - [Microsoft. AzureData](#microsoftazuredata)
> - [Microsoft. AzureStack](#microsoftazurestack)
> - [Microsoft. Batch](#microsoftbatch)
> - [Microsoft. Faturalandırma](#microsoftbilling)
> - [Microsoft. BingMaps](#microsoftbingmaps)
> - [Microsoft. Blockzinciri](#microsoftblockchain)
> - [Microsoft. Blueprint](#microsoftblueprint)
> - [Microsoft. BotService](#microsoftbotservice)
> - [Microsoft. Cache](#microsoftcache)
> - [Microsoft. Capacity](#microsoftcapacity)
> - [Microsoft. CDN](#microsoftcdn)
> - [Microsoft. CertificateRegistration](#microsoftcertificateregistration)
> - [Microsoft. ClassicCompute](#microsoftclassiccompute)
> - [Microsoft. ClassicInfrastructureMigrate](#microsoftclassicinfrastructuremigrate)
> - [Microsoft. ClassicNetwork](#microsoftclassicnetwork)
> - [Microsoft. ClassicStorage](#microsoftclassicstorage)
> - [Microsoft. Biliveservices](#microsoftcognitiveservices)
> - [Microsoft. Commerce](#microsoftcommerce)
> - [Microsoft. COMPUTE](#microsoftcompute)
> - [Microsoft. tüketim](#microsoftconsumption)
> - [Microsoft. Containerınstance](#microsoftcontainerinstance)
> - [Microsoft. ContainerRegistry](#microsoftcontainerregistry)
> - [Microsoft. ContainerService](#microsoftcontainerservice)
> - [Microsoft. Contentmoderatör](#microsoftcontentmoderator)
> - [Microsoft. Cortanaanalizi](#microsoftcortanaanalytics)
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
> - [Microsoft. DocumentDB](#microsoftdocumentdb)
> - [Microsoft. DomainRegistration](#microsoftdomainregistration)
> - [Microsoft. DynamicsLcs](#microsoftdynamicslcs)
> - [Microsoft. EnterpriseKnowledgeGraph](#microsoftenterpriseknowledgegraph)
> - [Microsoft. EventGrid](#microsofteventgrid)
> - [Microsoft. EventHub](#microsofteventhub)
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
> - [Microsoft. Hydra](#microsofthydra)
> - [Microsoft. ımportexport](#microsoftimportexport)
> - [Microsoft. Intune](#microsoftintune)
> - [Microsoft. ıotcentral](#microsoftiotcentral)
> - [Microsoft. ıotspaces](#microsoftiotspaces)
> - [Microsoft. Keykasası](#microsoftkeyvault)
> - [Microsoft. kusto](#microsoftkusto)
> - [Microsoft. LabServices](#microsoftlabservices)
> - [Microsoft. Logic](#microsoftlogic)
> - [Microsoft. Machinöğrenim](#microsoftmachinelearning)
> - [Microsoft. MachineLearningServices](#microsoftmachinelearningservices)
> - [Microsoft. Managedıdentity](#microsoftmanagedidentity)
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
> - [Microsoft. Network](#microsoftnetwork)
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
> - [Microsoft. RecoveryServices](#microsoftrecoveryservices)
> - [Microsoft. Relay](#microsoftrelay)
> - [Microsoft. RemoteApp](#microsoftremoteapp)
> - [Microsoft. ResourceGraph](#microsoftresourcegraph)
> - [Microsoft. ResourceHealth](#microsoftresourcehealth)
> - [Microsoft. resources](#microsoftresources)
> - [Microsoft. SaaS](#microsoftsaas)
> - [Microsoft. Scheduler](#microsoftscheduler)
> - [Microsoft. Search](#microsoftsearch)
> - [Microsoft. Security](#microsoftsecurity)
> - [Microsoft. SecurityGraph](#microsoftsecuritygraph)
> - [Microsoft. Securityınsights](#microsoftsecurityinsights)
> - [Microsoft. ServiceBus](#microsoftservicebus)
> - [Microsoft. ServiceFabric](#microsoftservicefabric)
> - [Microsoft. Servicefabrickafesi](#microsoftservicefabricmesh)
> - [Microsoft. Services](#microsoftservices)
> - [Microsoft. SignalRService](#microsoftsignalrservice)
> - [Microsoft. Sıterecovery](#microsoftsiterecovery)
> - [Microsoft. SoftwarePlan](#microsoftsoftwareplan)
> - [Microsoft. Solutions](#microsoftsolutions)
> - [Microsoft. SQL](#microsoftsql)
> - [Microsoft. SqlVirtualMachine](#microsoftsqlvirtualmachine)
> - [Microsoft. Storage](#microsoftstorage)
> - [Microsoft. StorageCache](#microsoftstoragecache)
> - [Microsoft. Storagerepce](#microsoftstoragereplication)
> - [Microsoft. Storagessync](#microsoftstoragesync)
> - [Microsoft. StorageSyncDev](#microsoftstoragesyncdev)
> - [Microsoft. Storagesyncınt](#microsoftstoragesyncint)
> - [Microsoft. StorSimple](#microsoftstorsimple)
> - [Microsoft. StreamAnalytics](#microsoftstreamanalytics)
> - [Microsoft. Subscription](#microsoftsubscription)
> - [Microsoft. Timeseriesınsights](#microsofttimeseriesinsights)
> - [Microsoft. Vmwarechoparlör basit](#microsoftvmwarecloudsimple)
> - [Microsoft. Web](#microsoftweb)
> - [Microsoft. Windowssavunma Deratp](#microsoftwindowsdefenderatp)
> - [Microsoft. Windowsıot](#microsoftwindowsiot)
> - [Microsoft. WorkloadMonitor](#microsoftworkloadmonitor)

## <a name="microsoftaad"></a>Microsoft. AAD

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tamamlanmış mod silme |
> | ------------- | ----------- |
> | DomainServices | Evet |
> | DomainServices/oucontainer | Hayır |
> | DomainServices/ReplicaSets | Evet |

## <a name="microsoftaddons"></a>Microsoft. addons

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tamamlanmış mod silme |
> | ------------- | ----------- |
> | Destek sağlayıcıları | Hayır |

## <a name="microsoftadhybridhealthservice"></a>Microsoft. ADHybridHealthService

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tamamlanmış mod silme |
> | ------------- | ----------- |
> | aadsupportcases | Hayır |
> | addsservices | Hayır |
> | aracılar | Hayır |
> | anonymousapiusers | Hayır |
> | yapılandırma | Hayır |
> | günlükler | Hayır |
> | raporlar | Hayır |
> | servicehealthölçümleri | Hayır |
> | hizmetler | Hayır |

## <a name="microsoftadvisor"></a>Microsoft. Advisor

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tamamlanmış mod silme |
> | ------------- | ----------- |
> | yapılandırmalar | Hayır |
> | Generatereyorumgeçişleri | Hayır |
> | meta veriler | Hayır |
> | Öneri | Hayır |
> | gizlemeleri | Hayır |

## <a name="microsoftalertsmanagement"></a>Microsoft. AlertsManagement

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tamamlanmış mod silme |
> | ------------- | ----------- |
> | actionRules | Evet |
> | Larınız | Hayır |
> | alertsList | Hayır |
> | alertsMetaData | Hayır |
> | alertsSummary | Hayır |
> | alertsSummaryList | Hayır |
> | geri bildirim | Hayır |
> | smartDetectorAlertRules | Evet |
> | Smartdetectorruntimeortamortamları | Hayır |
> | smartGroups | Hayır |

## <a name="microsoftanalysisservices"></a>Microsoft. AnalysisServices

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tamamlanmış mod silme |
> | ------------- | ----------- |
> | sunucular | Evet |

## <a name="microsoftapimanagement"></a>Microsoft. Apimanane

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tamamlanmış mod silme |
> | ------------- | ----------- |
> | reportFeedback | Hayır |
> | hizmet | Evet |
> | validateServiceName | Hayır |

## <a name="microsoftappconfiguration"></a>Microsoft. AppConfiguration

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tamamlanmış mod silme |
> | ------------- | ----------- |
> | Configurationmağazaların | Evet |
> | Configurationmağazaların/eventGridFilters | Hayır |

## <a name="microsoftappplatform"></a>Microsoft. AppPlatform

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tamamlanmış mod silme |
> | ------------- | ----------- |
> | Yay | Evet |

## <a name="microsoftattestation"></a>Microsoft. kanıtlama

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tamamlanmış mod silme |
> | ------------- | ----------- |
> | attestationProviders | Hayır |

## <a name="microsoftauthorization"></a>Microsoft. Authorization

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tamamlanmış mod silme |
> | ------------- | ----------- |
> | classicAdministrators | Hayır |
> | Datatakma adlar | Hayır |
> | Denyasatamaları | Hayır |
> | Erişimi yükseltme | Hayır |
> | kilitler | Hayır |
> | izinler | Hayır |
> | Poliyasatamaları | Hayır |
> | policyDefinitions | Hayır |
> | policySetDefinitions | Hayır |
> | providerOperations | Hayır |
> | roleAssignments | Hayır |
> | roleDefinitions | Hayır |

## <a name="microsoftautomation"></a>Microsoft. Automation

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tamamlanmış mod silme |
> | ------------- | ----------- |
> | automationAccounts | Evet |
> | automationAccounts/Configurations | Evet |
> | automationAccounts/Jobs | Hayır |
> | automationAccounts/runbook 'lar | Evet |
> | automationAccounts/softwareUpdateConfigurations | Hayır |
> | automationAccounts/Web kancaları | Hayır |

## <a name="microsoftazconfig"></a>Microsoft. Azconfig

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tamamlanmış mod silme |
> | ------------- | ----------- |
> | Configurationmağazaların | Evet |
> | Configurationmağazaların/eventGridFilters | Hayır |

## <a name="microsoftazuregeneva"></a>Microsoft. Azure. Genfiliz

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tamamlanmış mod silme |
> | ------------- | ----------- |
> | ortamlar | Hayır |
> | ortamlar/hesaplar | Hayır |
> | ortamlar/hesaplar/ad alanları | Hayır |
> | ortamlar/hesaplar/ad alanları/yapılandırma | Hayır |

## <a name="microsoftazureactivedirectory"></a>Microsoft. AzureActiveDirectory

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tamamlanmış mod silme |
> | ------------- | ----------- |
> | b2cDirectories | Evet |
> | b2ctenants | Hayır |

## <a name="microsoftazuredata"></a>Microsoft. AzureData

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tamamlanmış mod silme |
> | ------------- | ----------- |
> | Hybriddatayöneticileri | Evet |
> | Postgresınstances | Evet |
> | Sqlbıgdatakümeler | Evet |
> | SQLInstances | Evet |
> | Sqlserverkayıtları | Evet |
> | Sqlserverkayıtları/sqlServers | Hayır |

## <a name="microsoftazurestack"></a>Microsoft. AzureStack

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tamamlanmış mod silme |
> | ------------- | ----------- |
> | kayıtlarında | Evet |
> | kayıt/müşteri abonelikleri | Hayır |
> | kayıtlar/ürünler | Hayır |

## <a name="microsoftbatch"></a>Microsoft. Batch

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tamamlanmış mod silme |
> | ------------- | ----------- |
> | batchAccounts | Evet |

## <a name="microsoftbilling"></a>Microsoft. Faturalandırma

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tamamlanmış mod silme |
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
> | billingAccounts/billingProfiles/faturalar | Hayır |
> | billingAccounts/billingProfiles/faturalar/fiyat listesi | Hayır |
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
> | billingAccounts/Billingroleatamaları | Hayır |
> | billingAccounts/billingRoleDefinitions | Hayır |
> | billingAccounts/Billingabonelikleri | Hayır |
> | billingAccounts/Createbillingroleatama | Hayır |
> | billingAccounts/Createınvoicesectionoperations | Hayır |
> | billingAccounts/müşteriler | Hayır |
> | billingAccounts/müşteriler/Billingabonelikleri | Hayır |
> | billingAccounts/müşteriler/ınitiatetransfer | Hayır |
> | billingAccounts/müşteriler/ilkeler | Hayır |
> | billingAccounts/müşteriler/ürünler | Hayır |
> | billingAccounts/müşteriler/işlemler | Hayır |
> | billingAccounts/müşteriler/aktarımlar | Hayır |
> | billingAccounts/departmanlar | Hayır |
> | billingAccounts/KayıtSayısı | Hayır |
> | billingAccounts/faturalar | Hayır |
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
> | BillingPermissions | Hayır |
> | billingProperty | Hayır |
> | Billingroleatamaları | Hayır |
> | BillingRoleDefinitions | Hayır |
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
> | Kaynak türü | Tamamlanmış mod silme |
> | ------------- | ----------- |
> | Mapapsıs | Evet |
> | updateCommunicationPreference | Hayır |

## <a name="microsoftblockchain"></a>Microsoft. Blockzinciri

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tamamlanmış mod silme |
> | ------------- | ----------- |
> | blockchainMembers | Evet |
> | izleyicileri | Evet |

## <a name="microsoftblueprint"></a>Microsoft. Blueprint

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tamamlanmış mod silme |
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
> | Kaynak türü | Tamamlanmış mod silme |
> | ------------- | ----------- |
> | botServices | Evet |
> | botServices/kanallar | Hayır |
> | botServices/Connections | Hayır |
> | diller | Hayır |
> | templates | Hayır |

## <a name="microsoftcache"></a>Microsoft. Cache

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tamamlanmış mod silme |
> | ------------- | ----------- |
> | Redis | Evet |
> | RedisConfigDefinition | Hayır |

## <a name="microsoftcapacity"></a>Microsoft. Capacity

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tamamlanmış mod silme |
> | ------------- | ----------- |
> | appliedReservations | Hayır |
> | calculateExchange | Hayır |
> | calculatePrice | Hayır |
> | calculatePurchasePrice | Hayır |
> | larına | Hayır |
> | Ticari Vaalrezervler | Hayır |
> | Değişimi | Hayır |
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
> | kaynaklar | Hayır |
> | validateReservationOrder | Hayır |

## <a name="microsoftcdn"></a>Microsoft. CDN

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tamamlanmış mod silme |
> | ------------- | ----------- |
> | CdnWebApplicationFirewallManagedRuleSets | Hayır |
> | CdnWebApplicationFirewallPolicies | Evet |
> | edgenodes | Hayır |
> | profiller | Evet |
> | Profiller/uç noktalar | Evet |
> | Profiller/uç noktalar/customdomains | Hayır |
> | Profiller/uç noktalar/kaynaklar | Hayır |
> | Validatearaştırması | Hayır |

## <a name="microsoftcertificateregistration"></a>Microsoft. CertificateRegistration

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tamamlanmış mod silme |
> | ------------- | ----------- |
> | Sertifikadüzenleri | Evet |
> | certificateOrders/Certificates | Hayır |
> | Validatecertificateregistrationınformation | Hayır |

## <a name="microsoftclassiccompute"></a>Microsoft. ClassicCompute

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tamamlanmış mod silme |
> | ------------- | ----------- |
> | Yetenek | Hayır |
> | domainNames | Evet |
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
> | kota | Hayır |
> | resourceTypes | Hayır |
> | Validatesubscriptionmoveavaılabılıty | Hayır |
> | VirtualMachines | Evet |
> | virtualMachines/diagnosticSettings | Hayır |
> | virtualMachines/metricDefinitions | Hayır |
> | virtualMachines/ölçümler | Hayır |

## <a name="microsoftclassicinfrastructuremigrate"></a>Microsoft. ClassicInfrastructureMigrate

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tamamlanmış mod silme |
> | ------------- | ----------- |
> | classicInfrastructureResources | Hayır |

## <a name="microsoftclassicnetwork"></a>Microsoft. ClassicNetwork

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tamamlanmış mod silme |
> | ------------- | ----------- |
> | Yetenek | Hayır |
> | expressRouteCrossConnections | Hayır |
> | expressRouteCrossConnections/peerler | Hayır |
> | gatewaySupportedDevices | Hayır |
> | networkSecurityGroups | Evet |
> | kota | Hayır |
> | Rezervler | Evet |
> | virtualNetworks | Evet |
> | virtualNetworks/Remotevirtualnetworkpeeringproxy 'Leri | Hayır |
> | virtualNetworks/Virtualnetworkpeerler | Hayır |

## <a name="microsoftclassicstorage"></a>Microsoft. ClassicStorage

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tamamlanmış mod silme |
> | ------------- | ----------- |
> | Yetenek | Hayır |
> | Disklerinden | Hayır |
> | görüntüler | Hayır |
> | osImages | Hayır |
> | Osplatformımages | Hayır |
> | Publicımages | Hayır |
> | kota | Hayır |
> | storageAccounts | Evet |
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

## <a name="microsoftcognitiveservices"></a>Microsoft. Biliveservices

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tamamlanmış mod silme |
> | ------------- | ----------- |
> | hesaplar | Evet |

## <a name="microsoftcommerce"></a>Microsoft. Commerce

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tamamlanmış mod silme |
> | ------------- | ----------- |
> | RateCard | Hayır |
> | Usagetoplamaları | Hayır |

## <a name="microsoftcompute"></a>Microsoft. COMPUTE

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tamamlanmış mod silme |
> | ------------- | ----------- |
> | availabilitySets | Evet |
> | diskEncryptionSets | Evet |
> | Disklerinden | Evet |
> | Galeriler | Evet |
> | Galeriler/uygulamalar | Hayır |
> | Galeriler/uygulamalar/sürümler | Hayır |
> | Galeriler/görüntüler | Hayır |
> | Galeriler/resimler/sürümler | Hayır |
> | hostGroups | Evet |
> | hostGroups/konaklar | Evet |
> | görüntüler | Evet |
> | proximityPlacementGroups | Evet |
> | restorePointCollections | Evet |
> | restorePointCollections/restorePoints | Hayır |
> | Sharedvmımages | Evet |
> | Sharedvmımages/sürümler | Hayır |
> | anlık görüntüler | Evet |
> | VirtualMachines | Evet |
> | virtualMachines/uzantıları | Evet |
> | virtualMachines/metricDefinitions | Hayır |
> | virtualMachineScaleSets | Evet |
> | virtualMachineScaleSets/uzantılar | Hayır |
> | virtualMachineScaleSets/NetworkInterfaces | Hayır |
> | virtualMachineScaleSets/Publicıpaddresses | Hayır |
> | virtualMachineScaleSets/virtualMachines | Hayır |
> | virtualMachineScaleSets/virtualMachines/NetworkInterfaces | Hayır |

## <a name="microsoftconsumption"></a>Microsoft. tüketim

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tamamlanmış mod silme |
> | ------------- | ----------- |
> | Aggregmalyt maliyeti | Hayır |
> | Dengeler | Hayır |
> | Bütçelerinin | Hayır |
> | Ücretleriyle | Hayır |
> | CostTags | Hayır |
> | iler | Hayır |
> | olaylar | Hayır |
> | Hesaplansın | Hayır |
> | oluş | Hayır |
> | Marketlerinden | Hayır |
> | Pricesheets | Hayır |
> | ürün | Hayır |
> | Rezervde ayrıntıları | Hayır |
> | Rezervationönerilere | Hayır |
> | Rezervlerin Özeti | Hayır |
> | Rezervlik Işlemleri | Hayır |
> | Etiketler | Hayır |
> | Kira | Hayır |
> | Larındaki | Hayır |
> | UsageDetails | Hayır |

## <a name="microsoftcontainerinstance"></a>Microsoft. Containerınstance

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tamamlanmış mod silme |
> | ------------- | ----------- |
> | Kapsayıcı grupları | Evet |
> | serviceAssociationLinks | Hayır |

## <a name="microsoftcontainerregistry"></a>Microsoft. ContainerRegistry

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tamamlanmış mod silme |
> | ------------- | ----------- |
> | kayıt | Evet |
> | kayıt defterleri/derlemeler | Hayır |
> | kayıt defterleri/derlemeler/iptal | Hayır |
> | kayıt defterleri/derlemeler/getLogLink | Hayır |
> | kayıt defterleri/buildTasks | Evet |
> | kayıt defterleri/buildTasks/Steps | Hayır |
> | kayıt defterleri/eventGridFilters | Hayır |
> | kayıt defterleri/getBuildSourceUploadUrl 'Si | Hayır |
> | kayıt defterleri/GetCredentials | Hayır |
> | kayıt defterleri/ımportımage | Hayır |
> | kayıt defterleri/queueBuild | Hayır |
> | kayıt defterleri/regenerateCredential | Hayır |
> | kayıt defterleri/regenerateCredentials | Hayır |
> | kayıt defterleri/çoğaltmalar | Evet |
> | kayıt defterleri/çalıştırmalar | Hayır |
> | kayıt defterleri/çalıştırmalar/iptal | Hayır |
> | kayıt defterleri/scheduleRun | Hayır |
> | kayıt defterleri/görevler | Evet |
> | kayıt defterleri/updatePolicies | Hayır |
> | kayıt defterleri/Web kancaları | Evet |
> | kayıt defterleri/Web kancaları/getCallbackConfig | Hayır |
> | kayıt defterleri/Web kancaları/ping | Hayır |

## <a name="microsoftcontainerservice"></a>Microsoft. ContainerService

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tamamlanmış mod silme |
> | ------------- | ----------- |
> | containerServices | Evet |
> | Managedkümeler | Evet |
> | openShiftManagedClusters | Evet |

## <a name="microsoftcontentmoderator"></a>Microsoft. Contentmoderatör

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tamamlanmış mod silme |
> | ------------- | ----------- |
> | uygulamalar | Evet |
> | updateCommunicationPreference | Hayır |

## <a name="microsoftcortanaanalytics"></a>Microsoft. Cortanaanalizi

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tamamlanmış mod silme |
> | ------------- | ----------- |
> | hesaplar | Evet |

## <a name="microsoftcostmanagement"></a>Microsoft. CostManagement

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tamamlanmış mod silme |
> | ------------- | ----------- |
> | Uyarılar | Hayır |
> | billingAccounts | Hayır |
> | Bütçelerinin | Hayır |
> | Cloudbağlayıcıları | Hayır |
> | Larında | Evet |
> | bölümlerinin | Hayır |
> | Boyutlar | Hayır |
> | kayıt sayısı | Hayır |
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
> | t | Hayır |
> | Sorgu | Hayır |
> | register | Hayır |
> | Reportconfigs | Hayır |
> | Raporlar | Hayır |
> | Ayarlar | Hayır |
> | showbackRules | Hayır |
> | Görünümler | Hayır |

## <a name="microsoftcustomerlockbox"></a>Microsoft. Customerkasası

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tamamlanmış mod silme |
> | ------------- | ----------- |
> | istekler | Hayır |

## <a name="microsoftcustomproviders"></a>Microsoft. CustomProviders

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tamamlanmış mod silme |
> | ------------- | ----------- |
> | ilişkilendirmeler | Hayır |
> | resourceProviders | Evet |

## <a name="microsoftdatabox"></a>Microsoft. DataBox

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tamamlanmış mod silme |
> | ------------- | ----------- |
> | Çizelge | Evet |

## <a name="microsoftdataboxedge"></a>Microsoft. DataBoxEdge

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tamamlanmış mod silme |
> | ------------- | ----------- |
> | DataBoxEdgeDevices | Evet |

## <a name="microsoftdatabricks"></a>Microsoft. Databricks

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tamamlanmış mod silme |
> | ------------- | ----------- |
> | çalışma alanları | Evet |
> | çalışma alanları/Virtualnetworkpeerler | Hayır |

## <a name="microsoftdatacatalog"></a>Microsoft. DataCatalog

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tamamlanmış mod silme |
> | ------------- | ----------- |
> | larına | Evet |
> | veri katalogları | Evet |
> | veri katalogları/veri kaynakları | Hayır |
> | veri katalogları/veri kaynakları/taramalar | Hayır |
> | veri katalogları/veri kaynakları/taramalar/veri kümeleri | Hayır |
> | veri katalogları/veri kaynakları/taramalar/Tetikleyiciler | Hayır |
> | veri katalogları/scantargets | Hayır |
> | veri katalogları/scantargets/veri kümeleri | Hayır |

## <a name="microsoftdatafactory"></a>Microsoft. DataFactory

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tamamlanmış mod silme |
> | ------------- | ----------- |
> | Veri fabrikaları | Evet |
> | DataFactory/diagnosticSettings | Hayır |
> | DataFactory/metricDefinitions | Hayır |
> | dataFactorySchema | Hayır |
> | oluşturucular | Evet |
> | Fabrika/tümleştirme çalışma zamanları | Hayır |

## <a name="microsoftdatalakeanalytics"></a>Microsoft. DataLakeAnalytics

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tamamlanmış mod silme |
> | ------------- | ----------- |
> | hesaplar | Evet |
> | hesaplar/dataLakeStoreAccounts | Hayır |
> | hesaplar/storageAccounts | Hayır |
> | hesaplar/storageAccounts/kapsayıcılar | Hayır |
> | hesaplar/Transferanaliz tici | Hayır |

## <a name="microsoftdatalakestore"></a>Microsoft. DataLakeStore

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tamamlanmış mod silme |
> | ------------- | ----------- |
> | hesaplar | Evet |
> | hesaplar/eventGridFilters | Hayır |
> | hesaplar/firewallRules | Hayır |

## <a name="microsoftdatamigration"></a>Microsoft. DataMigration

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tamamlanmış mod silme |
> | ------------- | ----------- |
> | hizmetler | Evet |
> | Hizmetler/Projeler | Evet |

## <a name="microsoftdatashare"></a>Microsoft. DataShare

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tamamlanmış mod silme |
> | ------------- | ----------- |
> | hesaplar | Evet |
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
> | Kaynak türü | Tamamlanmış mod silme |
> | ------------- | ----------- |
> | sunucular | Evet |
> | sunucular/danışmanları | Hayır |
> | sunucular/Querymetinmetinleri | Hayır |
> | sunucular/recoverableServers | Hayır |
> | sunucular/topQueryStatistics | Hayır |
> | sunucular/virtualNetworkRules | Hayır |
> | sunucular/waitStatistics | Hayır |

## <a name="microsoftdbformysql"></a>Microsoft. Dbformyısql

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tamamlanmış mod silme |
> | ------------- | ----------- |
> | sunucular | Evet |
> | sunucular/danışmanları | Hayır |
> | sunucular/Querymetinmetinleri | Hayır |
> | sunucular/recoverableServers | Hayır |
> | sunucular/topQueryStatistics | Hayır |
> | sunucular/virtualNetworkRules | Hayır |
> | sunucular/waitStatistics | Hayır |

## <a name="microsoftdbforpostgresql"></a>Microsoft. DBforPostgreSQL

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tamamlanmış mod silme |
> | ------------- | ----------- |
> | Sunucu grupları | Evet |
> | sunucular | Evet |
> | sunucular/danışmanları | Hayır |
> | sunucular/Privateendpointconnectionproxy 'Leri | Hayır |
> | sunucular/privateEndpointConnections | Hayır |
> | sunucular/privateLinkResources | Hayır |
> | sunucular/Querymetinmetinleri | Hayır |
> | sunucular/recoverableServers | Hayır |
> | sunucular/topQueryStatistics | Hayır |
> | sunucular/virtualNetworkRules | Hayır |
> | sunucular/waitStatistics | Hayır |
> | serversv2 | Evet |

## <a name="microsoftdeploymentmanager"></a>Microsoft. DeploymentManager

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tamamlanmış mod silme |
> | ------------- | ----------- |
> | artifactSources | Evet |
> | piyasaya çıkarma | Evet |
> | Servicetopolojileri | Evet |
> | Servicetopolojileri/hizmetler | Evet |
> | Servicetopolojileri/hizmetler/serviceUnits | Evet |
> | olanları | Evet |

## <a name="microsoftdesktopvirtualization"></a>Microsoft. DesktopVirtualization

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tamamlanmış mod silme |
> | ------------- | ----------- |
> | applicationgroups | Evet |
> | applicationgroups/uygulamalar | Hayır |
> | applicationgroups/startmenuıtems | Hayır |
> | Ana bilgisayar havuzları | Evet |
> | hostpools/oturumkonakları | Hayır |
> | hostpools/sessionkonakları/usersessions | Hayır |
> | hosthavuzlar/usersessions | Hayır |
> | çalışma alanları | Evet |

## <a name="microsoftdevices"></a>Microsoft. Devices

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tamamlanmış mod silme |
> | ------------- | ----------- |
> | Elaun havuzları | Evet |
> | Elaun havuzları/ıothubkiracılar | Evet |
> | Iothubs | Evet |
> | IotHubs/eventGridFilters | Hayır |
> | ProvisioningServices | Evet |
> | vardır | Hayır |

## <a name="microsoftdevops"></a>Microsoft. DevOps

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tamamlanmış mod silme |
> | ------------- | ----------- |
> | düzenler | Evet |

## <a name="microsoftdevspaces"></a>Microsoft. DevSpaces

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tamamlanmış mod silme |
> | ------------- | ----------- |
> | denetleyiciler | Evet |

## <a name="microsoftdevtestlab"></a>Microsoft. DevTestLab

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tamamlanmış mod silme |
> | ------------- | ----------- |
> | labcenters | Evet |
> | labs | Evet |
> | Laboratuvarlar/ortamlar | Evet |
> | Labs/Servicerunanlar | Evet |
> | Labs/virtualMachines | Evet |
> | cağını | Evet |

## <a name="microsoftdocumentdb"></a>Microsoft. DocumentDB

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tamamlanmış mod silme |
> | ------------- | ----------- |
> | databaseAccountNames | Hayır |
> | Veritabanı hesapları | Evet |

## <a name="microsoftdomainregistration"></a>Microsoft. DomainRegistration

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tamamlanmış mod silme |
> | ------------- | ----------- |
> | etki alanları | Evet |
> | Domains/Domainownershiptanýmlayýcýlarý | Hayır |
> | generateSsoRequest | Hayır |
> | topLevelDomains | Hayır |
> | Validatedomainregistrationınformation | Hayır |

## <a name="microsoftdynamicslcs"></a>Microsoft. DynamicsLcs

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tamamlanmış mod silme |
> | ------------- | ----------- |
> | lcsprojects | Hayır |
> | lcsprojects/clouddağıtımları | Hayır |
> | lcsprojects/bağlayıcılar | Hayır |

## <a name="microsoftenterpriseknowledgegraph"></a>Microsoft. EnterpriseKnowledgeGraph

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tamamlanmış mod silme |
> | ------------- | ----------- |
> | hizmetler | Evet |

## <a name="microsofteventgrid"></a>Microsoft. EventGrid

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tamamlanmış mod silme |
> | ------------- | ----------- |
> | etki alanları | Evet |
> | etki alanları/konular | Hayır |
> | Eventabonelikleri | Hayır |
> | Extensionkonuları | Hayır |
> | konuları | Evet |
> | topicTypes | Hayır |

## <a name="microsofteventhub"></a>Microsoft. EventHub

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tamamlanmış mod silme |
> | ------------- | ----------- |
> | leriniz | Evet |
> | ad alanları | Evet |
> | ad alanları/authorizationrules | Hayır |
> | ad alanları/diskalrecoveryconfigs | Hayır |
> | ad alanları/eventhubs | Hayır |
> | ad alanları/eventhubs/authorizationrules | Hayır |
> | ad alanları/eventhubs/consumergroups | Hayır |
> | ad alanları/networkrulesets | Hayır |

## <a name="microsoftfeatures"></a>Microsoft. Features

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tamamlanmış mod silme |
> | ------------- | ----------- |
> | özellikler | Hayır |
> | sağlayıcılar | Hayır |

## <a name="microsoftgallery"></a>Microsoft. Gallery

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tamamlanmış mod silme |
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
> | Kaynak türü | Tamamlanmış mod silme |
> | ------------- | ----------- |
> | hesaplar | Evet |

## <a name="microsoftguestconfiguration"></a>Microsoft. GuestConfiguration

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tamamlanmış mod silme |
> | ------------- | ----------- |
> | Oto Managedvmconfigurationprofiles | Evet |
> | Guestconfigurationatamaları | Hayır |
> | yazılım | Hayır |
> | softwareUpdateProfile | Hayır |
> | softwareUpdates | Hayır |

## <a name="microsofthanaonazure"></a>Microsoft. HanaOnAzure

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tamamlanmış mod silme |
> | ------------- | ----------- |
> | Hanaınstances | Evet |
> | Sapizleyicileri | Evet |

## <a name="microsofthardwaresecuritymodules"></a>Microsoft. HardwareSecurityModules

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tamamlanmış mod silme |
> | ------------- | ----------- |
> | ayrılmış Atedhsms | Evet |

## <a name="microsofthdinsight"></a>Microsoft. HDInsight

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tamamlanmış mod silme |
> | ------------- | ----------- |
> | leriniz | Evet |
> | kümeler/uygulamalar | Hayır |

## <a name="microsofthealthcareapis"></a>Microsoft. Healthgelişme API 'leri

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tamamlanmış mod silme |
> | ------------- | ----------- |
> | hizmetler | Evet |

## <a name="microsofthybridcompute"></a>Microsoft. HybridCompute

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tamamlanmış mod silme |
> | ------------- | ----------- |
> | larla | Evet |

## <a name="microsofthybriddata"></a>Microsoft. HybridData

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tamamlanmış mod silme |
> | ------------- | ----------- |
> | Veri yöneticileri | Evet |

## <a name="microsofthydra"></a>Microsoft. Hydra

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tamamlanmış mod silme |
> | ------------- | ----------- |
> | bileşenler | Evet |
> | networkScopes | Evet |

## <a name="microsoftimportexport"></a>Microsoft. ımportexport

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tamamlanmış mod silme |
> | ------------- | ----------- |
> | Çizelge | Evet |

## <a name="microsoftintune"></a>Microsoft. Intune

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tamamlanmış mod silme |
> | ------------- | ----------- |
> | diagnosticSettings | Hayır |
> | diagnosticSettingsCategories | Hayır |

## <a name="microsoftiotcentral"></a>Microsoft. ıotcentral

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tamamlanmış mod silme |
> | ------------- | ----------- |
> | appTemplates | Hayır |
> | Iotapps | Evet |

## <a name="microsoftiotspaces"></a>Microsoft. ıotspaces

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tamamlanmış mod silme |
> | ------------- | ----------- |
> | çıkarılamıyor | Evet |

## <a name="microsoftkeyvault"></a>Microsoft. Keykasası

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tamamlanmış mod silme |
> | ------------- | ----------- |
> | Silinkaults | Hayır |
> | hsmPools | Evet |
> | kasaları | Evet |
> | kasa/erişim Ilkeleri | Hayır |
> | kasa/eventGridFilters | Hayır |
> | kasa/gizlilikler | Hayır |

## <a name="microsoftkusto"></a>Microsoft. kusto

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tamamlanmış mod silme |
> | ------------- | ----------- |
> | leriniz | Evet |
> | kümeler/attacheddatabaseconfigurations | Hayır |
> | kümeler/veritabanları | Hayır |
> | kümeler/veritabanları/veri bağlantıları | Hayır |
> | kümeler/veritabanları/eventhubconnections | Hayır |
> | kümeler/parçalar | Hayır |

## <a name="microsoftlabservices"></a>Microsoft. LabServices

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tamamlanmış mod silme |
> | ------------- | ----------- |
> | labaccounts | Evet |
> | kullanıcılar | Hayır |

## <a name="microsoftlogic"></a>Microsoft. Logic

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tamamlanmış mod silme |
> | ------------- | ----------- |
> | hostingEnvironments | Evet |
> | Tümleştirme hesapları | Evet |
> | ıntegrationserviceortamortamları | Evet |
> | ıntegrationserviceortamortamları/managedap | Evet |
> | ısotedenvironments | Evet |
> | iş akışları | Evet |

## <a name="microsoftmachinelearning"></a>Microsoft. Machinöğrenim

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tamamlanmış mod silme |
> | ------------- | ----------- |
> | Commitmentplanlar | Evet |
> | Hizmetleri | Evet |
> | Çalışma Alanları | Evet |

## <a name="microsoftmachinelearningservices"></a>Microsoft. MachineLearningServices

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tamamlanmış mod silme |
> | ------------- | ----------- |
> | çalışma alanları | Evet |
> | çalışma alanları/hesaplar | Hayır |

## <a name="microsoftmanagedidentity"></a>Microsoft. Managedıdentity

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tamamlanmış mod silme |
> | ------------- | ----------- |
> | Lerinizde | Hayır |
> | Userassignedıdentities | Evet |

## <a name="microsoftmanagedservices"></a>Microsoft. ManagedServices

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tamamlanmış mod silme |
> | ------------- | ----------- |
> | Pazar Placeryumuristrationdefinitions | Hayır |
> | Registrationatamaları | Hayır |
> | registrationDefinitions | Hayır |

## <a name="microsoftmanagement"></a>Microsoft. Management

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tamamlanmış mod silme |
> | ------------- | ----------- |
> | getEntities | Hayır |
> | Yönetim grupları | Hayır |
> | kaynaklar | Hayır |
> | startTenantBackfill | Hayır |
> | tenantBackfillStatus | Hayır |

## <a name="microsoftmaps"></a>Microsoft. Maps

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tamamlanmış mod silme |
> | ------------- | ----------- |
> | hesaplar | Evet |
> | hesaplar/eventGridFilters | Hayır |

## <a name="microsoftmarketplace"></a>Microsoft. Market

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tamamlanmış mod silme |
> | ------------- | ----------- |
> | sunar | Hayır |
> | offerTypes | Hayır |
> | offerTypes/yayımcılar | Hayır |
> | offerTypes/yayımcılar/teklifler | Hayır |
> | offerTypes/yayımcılar/teklifler/planlar | Hayır |
> | offerTypes/yayımcılar/teklifler/planlar/anlaşmalar | Hayır |
> | offerTypes/yayımcılar/teklifler/planlar/configs | Hayır |
> | offerTypes/yayımcılar/teklifler/planlar/configs/ımportımage | Hayır |
> | privategallergıtems | Hayır |
> | ürün | Hayır |
> | Publishers | Hayır |
> | Yayımcılar/teklifler | Hayır |
> | Yayımcılar/teklifler/Düzeltme | Hayır |

## <a name="microsoftmarketplaceapps"></a>Microsoft. MarketplaceApps

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tamamlanmış mod silme |
> | ------------- | ----------- |
> | classicDevServices | Evet |
> | updateCommunicationPreference | Hayır |

## <a name="microsoftmarketplaceordering"></a>Microsoft. Marketplacesıralaması

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tamamlanmış mod silme |
> | ------------- | ----------- |
> | anlaşmalar | Hayır |
> | offertypes | Hayır |

## <a name="microsoftmedia"></a>Microsoft. Media

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tamamlanmış mod silme |
> | ------------- | ----------- |
> | mediaservices | Evet |
> | mediaservices/accountFilters | Hayır |
> | mediaservices/varlıklar | Hayır |
> | mediaservices/varlıklar/assetFilters | Hayır |
> | mediaservices/contentKeyPolicies | Hayır |
> | mediaservices/eventGridFilters | Hayır |
> | mediaservices/liveEventOperations | Hayır |
> | mediaservices/liveEvents | Evet |
> | mediaservices/liveEvents/Liveçıktılar | Hayır |
> | mediaservices/liveOutputOperations | Hayır |
> | mediaservices/streamingEndpointOperations | Hayır |
> | mediaservices/streamingEndpoints | Evet |
> | mediaservices/Streamingkonumlandırıcı | Hayır |
> | mediaservices/streamingPolicies | Hayır |
> | mediaservices/dönüşümler | Hayır |
> | mediaservices/dönüşümler/işler | Hayır |

## <a name="microsoftmicroservices4spring"></a>Microsoft. Microservices4Spring

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tamamlanmış mod silme |
> | ------------- | ----------- |
> | Appkümeler | Evet |

## <a name="microsoftmigrate"></a>Microsoft. Migrate

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tamamlanmış mod silme |
> | ------------- | ----------- |
> | assessmentProjects | Evet |
> | migrateprojects | Evet |
> | projeler | Evet |

## <a name="microsoftmixedreality"></a>Microsoft. MixedReality

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tamamlanmış mod silme |
> | ------------- | ----------- |
> | holographicsBroadcastAccounts | Evet |
> | objectUnderstandingAccounts | Evet |
> | remoteRenderingAccounts | Evet |
> | spatialAnchorsAccounts | Evet |
> | surfaceReconstructionAccounts | Evet |

## <a name="microsoftnetapp"></a>Microsoft. NetApp

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tamamlanmış mod silme |
> | ------------- | ----------- |
> | netAppAccounts | Evet |
> | netAppAccounts/backupPolicies | Evet |
> | netAppAccounts/Capacityhavuzları | Evet |
> | netAppAccounts/Capacityhavuzları/birimleri | Evet |
> | netAppAccounts/Capacityhavuzları/birimleri/yedeklemeleri | Hayır |
> | netAppAccounts/Capacityhavuzlar/Volumes/Mount hedefleri | Evet |
> | netAppAccounts/Capacityhavuzlar/birimler/anlık görüntüler | Evet |
> | netAppAccounts/Vaults | Hayır |
## <a name="microsoftnetwork"></a>Microsoft. Network

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tamamlanmış mod silme |
> | ------------- | ----------- |
> | Applicationgateway 'ler | Evet |
> | applicationGatewayWebApplicationFirewallPolicies | Evet |
> | applicationSecurityGroups | Evet |
> | azureFirewallFqdnTags | Hayır |
> | azureFirewalls | Evet |
> | Savunma Konakları | Evet |
> | bgpServiceCommunities | Hayır |
> | bağlantılar | Evet |
> | ddosCustomPolicies | Evet |
> | Ddosprotectionplanlar | Evet |
> | Dnsoperationdurumları | Hayır |
> | dnszones | Evet |
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
> | Expressroutedevreleri | Evet |
> | expressRouteCrossConnections | Evet |
> | Expressroutegateway 'ler | Evet |
> | expressRoutePorts | Evet |
> | expressRouteServiceProviders | Hayır |
> | firewallPolicies | Evet |
> | frontkapıların | Evet |
> | frontdoorWebApplicationFirewallManagedRuleSets | Hayır |
> | frontdoorWebApplicationFirewallPolicies | Evet |
> | getDnsResourceReference | Hayır |
> | ınternalnotify | Hayır |
> | loadBalancers | Evet |
> | Localnetworkgateway 'ler | Evet |
> | Natgateway 'ler | Evet |
> | Networkıntpolicies Ilkeleri | Evet |
> | NetworkInterfaces | Evet |
> | networkProfiles | Evet |
> | networkSecurityGroups | Evet |
> | networkWatchers | Evet |
> | networkWatchers/Connectionmonitörleri | Evet |
> | networkWatchers/uzunluler | Evet |
> | networkWatchers/Pingkafesler | Evet |
> | p2sVpnGateways | Evet |
> | privateDnsOperationStatuses | Hayır |
> | privateDnsZones | Evet |
> | privateDnsZones/A | Hayır |
> | privateDnsZones/AAAA | Hayır |
> | privateDnsZones/tümü | Hayır |
> | privateDnsZones/CNAME | Hayır |
> | privateDnsZones/MX | Hayır |
> | privateDnsZones/PTR | Hayır |
> | privateDnsZones/SOA | Hayır |
> | privateDnsZones/SRV | Hayır |
> | privateDnsZones/TXT | Hayır |
> | privateDnsZones/virtualNetworkLinks | Evet |
> | privateEndpoints | Evet |
> | privateLinkServices | Evet |
> | Publicıpaddresses | Evet |
> | Publicıpöneklerini | Evet |
> | routeFilters | Evet |
> | routeTables | Evet |
> | Securegateyöntemlere | Evet |
> | serviceEndpointPolicies | Evet |
> | trafficManagerGeographicHierarchies | Hayır |
> | trafficmanagerprofiles | Evet |
> | trafficmanagerprofiles/heatMaps | Hayır |
> | trafficManagerUserMetricsKeys | Hayır |
> | Virtualhub 'Lar | Evet |
> | virtualNetworkGateways | Evet |
> | virtualNetworks | Evet |
> | virtualNetworkTaps | Evet |
> | Virtualwan | Evet |
> | Vpngateway 'ler | Evet |
> | vpnSites | Evet |
> | webApplicationFirewallPolicies | Evet |

## <a name="microsoftnotificationhubs"></a>Microsoft. Notificationhub 'Lar

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tamamlanmış mod silme |
> | ------------- | ----------- |
> | ad alanları | Evet |
> | ad alanları/Notificationhub 'Lar | Evet |

## <a name="microsoftobjectstore"></a>Microsoft. ObjectStore

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tamamlanmış mod silme |
> | ------------- | ----------- |
> | osNamespaces | Evet |

## <a name="microsoftoffazure"></a>Microsoft. OffAzure

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tamamlanmış mod silme |
> | ------------- | ----------- |
> | Hiper sanal siteler | Evet |
> | Importsites | Evet |
> | Sunucusiteleri | Evet |
> | VMwareSites | Evet |

## <a name="microsoftoperationalinsights"></a>Microsoft. Operationalınsights

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tamamlanmış mod silme |
> | ------------- | ----------- |
> | leriniz | Evet |
> | Cihazlarınız | Hayır |
> | Bağlantı hedefleri | Hayır |
> | Storageınsii configs | Hayır |
> | çalışma alanları | Evet |
> | çalışma alanları/veri kaynakları | Hayır |
> | çalışma alanları/linkedServices | Hayır |
> | çalışma alanları/sorgu | Hayır |

## <a name="microsoftoperationsmanagement"></a>Microsoft. OperationsManagement

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tamamlanmış mod silme |
> | ------------- | ----------- |
> | managementassociations | Hayır |
> | managementconfigurations | Evet |
> | çözümler | Evet |
> | görünümler | Evet |

## <a name="microsoftpeering"></a>Microsoft. eşleme

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tamamlanmış mod silme |
> | ------------- | ----------- |
> | Yasallıklar | Hayır |
> | peerAsns | Hayır |
> | eşlemeleri | Evet |
> | peeringServiceProviders | Hayır |
> | peeringServices | Evet |

## <a name="microsoftpolicyinsights"></a>Microsoft. Poliyelei

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tamamlanmış mod silme |
> | ------------- | ----------- |
> | Poliyevents | Hayır |
> | policyStates | Hayır |
> | policyTrackedResources | Hayır |
> | düzeltmeler | Hayır |

## <a name="microsoftportal"></a>Microsoft. Portal

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tamamlanmış mod silme |
> | ------------- | ----------- |
> | konsolları | Hayır |
> | panolar | Evet |
> | userSettings | Hayır |

## <a name="microsoftpowerbi"></a>Microsoft. PowerBI

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tamamlanmış mod silme |
> | ------------- | ----------- |
> | workspaceCollections | Evet |

## <a name="microsoftpowerbidedicated"></a>Microsoft. Powerbiadanmış

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tamamlanmış mod silme |
> | ------------- | ----------- |
> | kapasiteler | Evet |

## <a name="microsoftrecoveryservices"></a>Microsoft. RecoveryServices

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tamamlanmış mod silme |
> | ------------- | ----------- |
> | Backupkorunabilir | Hayır |
> | kasaları | Evet |

## <a name="microsoftrelay"></a>Microsoft. Relay

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tamamlanmış mod silme |
> | ------------- | ----------- |
> | ad alanları | Evet |
> | ad alanları/authorizationrules | Hayır |
> | ad alanları/hybridconnections | Hayır |
> | ad alanları/hybridconnections/authorizationrules | Hayır |
> | ad alanları/wcfreyerleştiri | Hayır |
> | ad alanları/wcfreyerleştirme/authorizationrules | Hayır |

## <a name="microsoftremoteapp"></a>Microsoft. RemoteApp

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tamamlanmış mod silme |
> | ------------- | ----------- |
> | hesaplar | Hayır |
> | koleksiyonları | Evet |
> | Koleksiyonlar/uygulamalar | Hayır |
> | Koleksiyonlar/SecurityPrincipals | Hayır |
> | Templateımages | Hayır |

## <a name="microsoftresourcegraph"></a>Microsoft. ResourceGraph

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tamamlanmış mod silme |
> | ------------- | ----------- |
> | sorgular | Evet |
> | resourceChangeDetails | Hayır |
> | resourceChanges | Hayır |
> | kaynaklar | Hayır |
> | resourcesHistory | Hayır |
> | subscriptionsStatus | Hayır |

## <a name="microsoftresourcehealth"></a>Microsoft. ResourceHealth

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tamamlanmış mod silme |
> | ------------- | ----------- |
> | Kullanılabilirlik durumları | Hayır |
> | Childadvailabilitydurumlar | Hayır |
> | childResources | Hayır |
> | olaylar | Hayır |
> | ımpactedresources | Hayır |
> | meta veriler | Hayır |
> | bildirimler | Hayır |

## <a name="microsoftresources"></a>Microsoft. resources

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tamamlanmış mod silme |
> | ------------- | ----------- |
> | dağıtımlar | Hayır |
> | dağıtımlar/işlemler | Hayır |
> | bağlantılar | Hayır |
> | notifyResourceJobs | Hayır |
> | sağlayıcılar | Hayır |
> | resourceGroups | Hayır |
> | kaynaklar | Hayır |
> | Aboneliklerin | Hayır |
> | Abonelikler/sağlayıcılar | Hayır |
> | Abonelikler/resourceGroups | Hayır |
> | Abonelikler/ResourceGroups/kaynaklar | Hayır |
> | Abonelikler/kaynaklar | Hayır |
> | Abonelikler/etiket adları | Hayır |
> | Abonelikler/etiket adları/tagValues | Hayır |
> | Kira | Hayır |

## <a name="microsoftsaas"></a>Microsoft. SaaS

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tamamlanmış mod silme |
> | ------------- | ----------- |
> | uygulamalar | Evet |
> | saasresources | Hayır |

## <a name="microsoftscheduler"></a>Microsoft. Scheduler

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tamamlanmış mod silme |
> | ------------- | ----------- |
> | işlere | Evet |

## <a name="microsoftsearch"></a>Microsoft. Search

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tamamlanmış mod silme |
> | ------------- | ----------- |
> | resourceHealthMetadata | Hayır |
> | searchServices | Evet |

## <a name="microsoftsecurity"></a>Microsoft.Security

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tamamlanmış mod silme |
> | ------------- | ----------- |
> | adaptiveNetworkHardenings | Hayır |
> | advancedThreatProtectionSettings | Hayır |
> | Larınız | Hayır |
> | allowedConnections | Hayır |
> | Applicationwhitedökümler | Hayır |
> | assessmentMetadata | Hayır |
> | kopyalan | Hayır |
> | Oto Provisioningsettings | Hayır |
> | Uyumluluklarına | Hayır |
> | dataCollectionAgents | Hayır |
> | deviceSecurityGroups | Hayır |
> | discoveredSecuritySolutions | Hayır |
> | externalSecuritySolutions | Hayır |
> | Informationprotectionpolicies | Hayır |
> | ıotsecuritysolutions | Evet |
> | ıotsecuritysolutions/analiz Ticsmodeller | Hayır |
> | ıotsecuritysolutions/Analticsmodeller/Aggreggıt uyarıları | Hayır |
> | ıotsecuritysolutions/analiz Ticsmodeller/Aggreg, öneriler | Hayır |
> | Jağaccesspolicies | Hayır |
> | networkData | Hayır |
> | playbookConfigurations | Evet |
> | ilkeler | Hayır |
> | fiyatlandırmalar | Hayır |
> | Reve daha karmaşık bakım standartları | Hayır |
> | Rekontrol ve Re, | Hayır |
> | Rekontrol ve Re, Re, Re, | Hayır |
> | securityContacts | Hayır |
> | securitySolutions | Hayır |
> | securitySolutionsReferenceData | Hayır |
> | Securitydurumlardan | Hayır |
> | securityStatusesSummaries | Hayır |
> | Sunucukullanılabilirliği | Hayır |
> | ayarlar | Hayır |
> | subAssessments | Hayır |
> | görevler | Hayır |
> | anlatır | Hayır |
> | çalışma alanı ayarları | Hayır |

## <a name="microsoftsecuritygraph"></a>Microsoft. SecurityGraph

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tamamlanmış mod silme |
> | ------------- | ----------- |
> | diagnosticSettings | Hayır |
> | diagnosticSettingsCategories | Hayır |

## <a name="microsoftsecurityinsights"></a>Microsoft. Securityınsights

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tamamlanmış mod silme |
> | ------------- | ----------- |
> | toplamaları | Hayır |
> | alertRules | Hayır |
> | Alertrutatemplates | Hayır |
> | yer işaretleri | Hayır |
> | çalışmaların | Hayır |
> | Veri bağlayıcıları | Hayır |
> | Varlık | Hayır |
> | entityQueries | Hayır |
> | officeConsents | Hayır |
> | ayarlar | Hayır |

## <a name="microsoftservicebus"></a>Microsoft. ServiceBus

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tamamlanmış mod silme |
> | ------------- | ----------- |
> | ad alanları | Evet |
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
> | Kaynak türü | Tamamlanmış mod silme |
> | ------------- | ----------- |
> | uygulamalar | Evet |
> | leriniz | Evet |
> | kümeler/uygulamalar | Hayır |
> | Kapsayıcı grupları | Evet |
> | containerGroupSets | Evet |
> | edgeclusters | Evet |
> | edgeclusters/uygulamalar | Hayır |
> | ağlar | Evet |
> | secretmağazaları | Evet |
> | secretmağazaları/sertifikaları | Hayır |
> | secretmağazaları/gizli dizileri | Hayır |
> | Dörtten | Evet |

## <a name="microsoftservicefabricmesh"></a>Microsoft. Servicefabrickafesi

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tamamlanmış mod silme |
> | ------------- | ----------- |
> | uygulamalar | Evet |
> | Kapsayıcı grupları | Evet |
> | geçidinin | Evet |
> | ağlar | Evet |
> | kaynaklanır | Evet |
> | Dörtten | Evet |

## <a name="microsoftservices"></a>Microsoft. Services

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tamamlanmış mod silme |
> | ------------- | ----------- |
> | Providerkayıtları | Hayır |
> | Providerkayıtlarıyla/resourceTypeRegistrations | Hayır |
> | piyasaya çıkarma | Evet |

## <a name="microsoftsignalrservice"></a>Microsoft. SignalRService

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tamamlanmış mod silme |
> | ------------- | ----------- |
> | SignalR | Evet |
> | SignalR/eventGridFilters | Hayır |

## <a name="microsoftsiterecovery"></a>Microsoft. Sıterecovery

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tamamlanmış mod silme |
> | ------------- | ----------- |
> | SiteRecoveryVault | Evet |

## <a name="microsoftsoftwareplan"></a>Microsoft. SoftwarePlan

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tamamlanmış mod silme |
> | ------------- | ----------- |
> | Hybriduseavantajlar | Hayır |

## <a name="microsoftsolutions"></a>Microsoft. Solutions

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tamamlanmış mod silme |
> | ------------- | ----------- |
> | applicationDefinitions | Evet |
> | uygulamalar | Evet |
> | Jistekleri | Evet |

## <a name="microsoftsql"></a>Microsoft. SQL

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tamamlanmış mod silme |
> | ------------- | ----------- |
> | ManagedInstances | Evet |
> | ManagedInstances/veritabanları | Evet |
> | ManagedInstances/veritabanları/backupShortTermRetentionPolicies | Hayır |
> | ManagedInstances/veritabanları/şemalar/tablolar/sütunlar/sensitivityLabels | Hayır |
> | ManagedInstances/veritabanları/ | Hayır |
> | ManagedInstances/veritabanları/ek | Hayır |
> | ManagedInstances/encryptionProtector | Hayır |
> | ManagedInstances/anahtarlar | Hayır |
> | ManagedInstances/Restokbledroppeddatabases/backupShortTermRetentionPolicies | Hayır |
> | ManagedInstances/ | Hayır |
> | sunucular | Evet |
> | sunucular/Yöneticiler | Hayır |
> | sunucular/communicationLinks | Hayır |
> | sunucular/veritabanları | Evet |
> | sunucular/encryptionProtector | Hayır |
> | sunucular/firewallRules | Hayır |
> | sunucular/anahtarlar | Hayır |
> | sunucular/Restokbledroppeddatabases | Hayır |
> | Sunucu/hizmet hedefleri | Hayır |
> | sunucular/tdeCertificates | Hayır |
> | Virtualkümeler | Hayır |

## <a name="microsoftsqlvirtualmachine"></a>Microsoft. SqlVirtualMachine

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tamamlanmış mod silme |
> | ------------- | ----------- |
> | SqlVirtualMachineGroups | Evet |
> | SqlVirtualMachineGroups/kullanılabilirliği Bilitygrouplisteners | Hayır |
> | SqlVirtualMachines | Evet |

## <a name="microsoftstorage"></a>Microsoft. Storage

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tamamlanmış mod silme |
> | ------------- | ----------- |
> | storageAccounts | Evet |
> | storageAccounts/blobServices | Hayır |
> | storageAccounts/fileServices | Hayır |
> | storageAccounts/queueServices | Hayır |
> | storageAccounts/Services | Hayır |
> | storageAccounts/Services/metricDefinitions | Hayır |
> | storageAccounts/tableServices | Hayır |
> | vardır | Hayır |

## <a name="microsoftstoragecache"></a>Microsoft. StorageCache

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tamamlanmış mod silme |
> | ------------- | ----------- |
> | önbelleklerinde | Evet |
> | önbellekler/Storagetaral | Hayır |
> | Usagemodeller | Hayır |

## <a name="microsoftstoragereplication"></a>Microsoft. Storagerepce

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tamamlanmış mod silme |
> | ------------- | ----------- |
> | replicationGroups | Hayır |

## <a name="microsoftstoragesync"></a>Microsoft. Storagessync

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tamamlanmış mod silme |
> | ------------- | ----------- |
> | storageSyncServices | Evet |
> | storageSyncServices/registeredServers | Hayır |
> | storageSyncServices/syncGroups | Hayır |
> | storageSyncServices/syncGroups/cloudEndpoints | Hayır |
> | storageSyncServices/syncGroups/serverEndpoints | Hayır |
> | storageSyncServices/iş akışları | Hayır |

## <a name="microsoftstoragesyncdev"></a>Microsoft. StorageSyncDev

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tamamlanmış mod silme |
> | ------------- | ----------- |
> | storageSyncServices | Evet |
> | storageSyncServices/registeredServers | Hayır |
> | storageSyncServices/syncGroups | Hayır |
> | storageSyncServices/syncGroups/cloudEndpoints | Hayır |
> | storageSyncServices/syncGroups/serverEndpoints | Hayır |
> | storageSyncServices/iş akışları | Hayır |

## <a name="microsoftstoragesyncint"></a>Microsoft. Storagesyncınt

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tamamlanmış mod silme |
> | ------------- | ----------- |
> | storageSyncServices | Evet |
> | storageSyncServices/registeredServers | Hayır |
> | storageSyncServices/syncGroups | Hayır |
> | storageSyncServices/syncGroups/cloudEndpoints | Hayır |
> | storageSyncServices/syncGroups/serverEndpoints | Hayır |
> | storageSyncServices/iş akışları | Hayır |

## <a name="microsoftstorsimple"></a>Microsoft. StorSimple

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tamamlanmış mod silme |
> | ------------- | ----------- |
> | ilerinde | Evet |

## <a name="microsoftstreamanalytics"></a>Microsoft. StreamAnalytics

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tamamlanmış mod silme |
> | ------------- | ----------- |
> | streammingjobs | Evet |

## <a name="microsoftsubscription"></a>Microsoft. Subscription

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tamamlanmış mod silme |
> | ------------- | ----------- |
> | İptal | Hayır |
> | CreateSubscription | Hayır |
> | Etkinleştirebilir | Hayır |
> | rename | Hayır |
> | SubscriptionDefinitions | Hayır |
> | SubscriptionOperations | Hayır |

## <a name="microsofttimeseriesinsights"></a>Microsoft. Timeseriesınsights

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tamamlanmış mod silme |
> | ------------- | ----------- |
> | ortamlar | Evet |
> | ortamlar/accessPolicies | Hayır |
> | ortamlar/EventSources | Evet |
> | ortamlar/Referencedataset 'ler | Evet |

## <a name="microsoftvmwarecloudsimple"></a>Microsoft. Vmwarechoparlör basit

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tamamlanmış mod silme |
> | ------------- | ----------- |
> | ayrılmış Cloudnodes | Evet |
> | ayrılmış CloudService | Evet |
> | VirtualMachines | Evet |

## <a name="microsoftweb"></a>Microsoft. Web

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tamamlanmış mod silme |
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
> | sertifikalar | Evet |
> | Connectiongateway 'ler | Evet |
> | bağlantılar | Evet |
> | Customapsıs | Evet |
> | Silinmi siteleri | Hayır |
> | işlevleri | Hayır |
> | hostingEnvironments | Evet |
> | hostingEnvironments/multiRolePools | Hayır |
> | hostingEnvironments/workerPools | Hayır |
> | publishingUsers | Hayır |
> | Öneri | Hayır |
> | resourceHealthMetadata | Hayır |
> | zamanları | Hayır |
> | serverFarms | Evet |
> | Sunucugrupları/eventGridFilters | Hayır |
> | barındıra | Evet |
> | siteler/yapılandırma  | Hayır |
> | siteler/eventGridFilters | Hayır |
> | siteler/hostNameBindings | Hayır |
> | Sites/networkConfig | Hayır |
> | siteler/premieraddons | Evet |
> | siteler/yuvalar | Evet |
> | siteler/yuvalar/eventGridFilters | Hayır |
> | siteler/yuvalar/hostNameBindings | Hayır |
> | siteler/yuvalar/networkConfig | Hayır |
> | sourceControls | Hayır |
> | doğrulamalısınız | Hayır |
> | verifyHostingEnvironmentVnet | Hayır |

## <a name="microsoftwindowsdefenderatp"></a>Microsoft. Windowssavunma Deratp

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tamamlanmış mod silme |
> | ------------- | ----------- |
> | diagnosticSettings | Hayır |
> | diagnosticSettingsCategories | Hayır |

## <a name="microsoftwindowsiot"></a>Microsoft. Windowsıot

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tamamlanmış mod silme |
> | ------------- | ----------- |
> | DeviceServices | Evet |

## <a name="microsoftworkloadmonitor"></a>Microsoft. WorkloadMonitor

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tamamlanmış mod silme |
> | ------------- | ----------- |
> | bileşenler | Hayır |
> | componentsSummary | Hayır |
> | Izleme örnekleri | Hayır |
> | Izleme ınstancessummary | Hayır |
> | monitörler | Hayır |
> | notificationSettings | Hayır |

## <a name="next-steps"></a>Sonraki adımlar

Aynı verileri bir virgülle ayrılmış değerler dosyası ile almak için, [Complete-Mode-Deletion. csv](https://github.com/tfitzmac/resource-capabilities/blob/master/complete-mode-deletion.csv)dosyasını indirin.
