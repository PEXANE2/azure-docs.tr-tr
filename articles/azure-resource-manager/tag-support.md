---
title: Kaynaklar için Azure Resource Manager etiketi desteği
description: Hangi Azure kaynak türlerinin etiketleri desteklediğini gösterir. Tüm Azure hizmetleri için ayrıntılar sağlar.
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: reference
ms.date: 08/05/2019
ms.author: tomfitz
ms.openlocfilehash: 0e8df116a82f73af2348651a91aee3c34a595c45
ms.sourcegitcommit: c8a102b9f76f355556b03b62f3c79dc5e3bae305
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/06/2019
ms.locfileid: "68814090"
---
# <a name="tag-support-for-azure-resources"></a>Azure kaynakları için etiket desteği
Bu makalede, bir kaynak türünün [etiketleri](resource-group-using-tags.md)destekleyip desteklemediğini açıklanmaktadır. Etiketi **destekleyen** sütun, kaynak türünün etiket için bir özelliğe sahip olup olmadığını gösterir. **Maliyet raporundaki etiket** etiketli sütun, kaynak türünün etiketi maliyet raporuna geçirip geçirmediğini belirtir.

Aynı verileri bir virgülle ayrılmış değerler dosyası ile almak için, [tag-support. csv](https://github.com/tfitzmac/resource-capabilities/blob/master/tag-support.csv)dosyasını indirin.

Kaynak sağlayıcısı ad alanına atlayın:
> [!div class="op_single_selector"]
> - [Microsoft. AAD](#microsoftaad)
> - [Microsoft. AADDomainServices](#microsoftaaddomainservices)
> - [Microsoft. addons](#microsoftaddons)
> - [Microsoft. ADHybridHealthService](#microsoftadhybridhealthservice)
> - [Microsoft. Advisor](#microsoftadvisor)
> - [Microsoft. AlertsManagement](#microsoftalertsmanagement)
> - [Microsoft. AnalysisServices](#microsoftanalysisservices)
> - [Microsoft. Apimanane](#microsoftapimanagement)
> - [Microsoft. AppConfiguration](#microsoftappconfiguration)
> - [Microsoft. kanıtlama](#microsoftattestation)
> - [Microsoft.Authorization](#microsoftauthorization)
> - [Microsoft. Automation](#microsoftautomation)
> - [Microsoft. Azconfig](#microsoftazconfig)
> - [Microsoft. Azure. Genfiliz](#microsoftazuregeneva)
> - [Microsoft. AzureActiveDirectory](#microsoftazureactivedirectory)
> - [Microsoft. AzureData](#microsoftazuredata)
> - [Microsoft. AzureStack](#microsoftazurestack)
> - [Microsoft. Batch](#microsoftbatch)
> - [Microsoft. Faturalandırma](#microsoftbilling)
> - [Microsoft. BingMaps](#microsoftbingmaps)
> - [Microsoft. BizTalkServices](#microsoftbiztalkservices)
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
> - [Microsoft. Customerınsights](#microsoftcustomerinsights)
> - [Microsoft. Customerkasası](#microsoftcustomerlockbox)
> - [Microsoft. CustomProviders](#microsoftcustomproviders)
> - [Microsoft. DataBox](#microsoftdatabox)
> - [Microsoft. DataBoxEdge](#microsoftdataboxedge)
> - [Microsoft. Databricks](#microsoftdatabricks)
> - [Microsoft. DataCatalog](#microsoftdatacatalog)
> - [Microsoft. DataConnect](#microsoftdataconnect)
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
> - [Microsoft. ManagedLab](#microsoftmanagedlab)
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
> - [Microsoft. OffAzure](#microsoftoffazure)
> - [Microsoft. Operationalınsights](#microsoftoperationalinsights)
> - [Microsoft. OperationsManagement](#microsoftoperationsmanagement)
> - [Microsoft. eşleme](#microsoftpeering)
> - [Microsoft.PolicyInsights](#microsoftpolicyinsights)
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
> - [Microsoft.ServiceBus](#microsoftservicebus)
> - [Microsoft. ServiceFabric](#microsoftservicefabric)
> - [Microsoft. Servicefabrickafesi](#microsoftservicefabricmesh)
> - [Microsoft. Services](#microsoftservices)
> - [Microsoft. SignalRService](#microsoftsignalrservice)
> - [Microsoft. Sıterecovery](#microsoftsiterecovery)
> - [Microsoft. SoftwarePlan](#microsoftsoftwareplan)
> - [Microsoft. Solutions](#microsoftsolutions)
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

## <a name="microsoftaad"></a>Microsoft.AAD

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | DomainServices | Evet |Evet |
> | DomainServices/oucontainer | Hayır |Hayır |
> | DomainServices/ReplicaSets | Evet |Evet |

## <a name="microsoftaaddomainservices"></a>Microsoft. AADDomainServices

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | etki alanları | Hayır |Hayır |

## <a name="microsoftaddons"></a>Microsoft. addons

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | Destek sağlayıcıları | Hayır |Hayır |

## <a name="microsoftadhybridhealthservice"></a>Microsoft.ADHybridHealthService

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | aadsupportcases | Hayır |Hayır |
> | addsservices | Hayır |Hayır |
> | aracılar | Hayır |Hayır |
> | anonymousapiusers | Hayır |Hayır |
> | yapılandırma | Hayır |Hayır |
> | logs | Hayır |Hayır |
> | raporlar | Hayır |Hayır |
> | servicehealthölçümleri | Hayır |Hayır |
> | services | Hayır |Hayır |

## <a name="microsoftadvisor"></a>Microsoft.Advisor

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | konfigürasyonları | Hayır |Hayır |
> | Generatereyorumgeçişleri | Hayır |Hayır |
> | meta veriler | Hayır |Hayır |
> | Önerileri | Hayır |Hayır |
> | gizlemeleri | Hayır |Hayır |

## <a name="microsoftalertsmanagement"></a>Microsoft.AlertsManagement

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | actionRules | Evet |Evet |
> | alerts | Hayır |Hayır |
> | alertsList | Hayır |Hayır |
> | alertsMetaData | Hayır |Hayır |
> | alertsSummary | Hayır |Hayır |
> | alertsSummaryList | Hayır |Hayır |
> | lerimi | Hayır |Hayır |
> | smartDetectorAlertRules | Hayır |Hayır |
> | Smartdetectorruntimeortamortamları | Hayır |Hayır |
> | smartGroups | Hayır |Hayır |

## <a name="microsoftanalysisservices"></a>Microsoft.AnalysisServices

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | sunucu | Evet |Evet |

## <a name="microsoftapimanagement"></a>Microsoft.ApiManagement

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | reportFeedback | Hayır |Hayır |
> | hizmet | Evet |Evet |
> | validateServiceName | Hayır |Hayır |

## <a name="microsoftappconfiguration"></a>Microsoft. AppConfiguration

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | Configurationmağazaların | Evet |Evet |
> | Configurationmağazaların/eventGridFilters | Hayır |Hayır |

## <a name="microsoftattestation"></a>Microsoft. kanıtlama

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | attestationProviders | Hayır |Hayır |

## <a name="microsoftauthorization"></a>Microsoft. Authorization

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | classicAdministrators | Hayır |Hayır |
> | Datatakma adlar | Hayır |Hayır |
> | Denyasatamaları | Hayır |Hayır |
> | Erişimi yükseltme | Hayır |Hayır |
> | kaynaktaki | Hayır |Hayır |
> | izinler | Hayır |Hayır |
> | Poliyasatamaları | Hayır |Hayır |
> | policyDefinitions | Hayır |Hayır |
> | policySetDefinitions | Hayır |Hayır |
> | providerOperations | Hayır |Hayır |
> | roleAssignments | Hayır |Hayır |
> | roleDefinitions | Hayır |Hayır |

## <a name="microsoftautomation"></a>Microsoft. Automation

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | automationAccounts | Evet |Evet |
> | automationAccounts/Configurations | Evet |Evet |
> | automationAccounts/Jobs | Hayır |Hayır |
> | automationAccounts/runbook 'lar | Evet |Evet |
> | automationAccounts/softwareUpdateConfigurations | Hayır |Hayır |
> | automationAccounts/Web kancaları | Hayır |Hayır |

## <a name="microsoftazconfig"></a>Microsoft. Azconfig

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | Configurationmağazaların | Evet |Evet |
> | Configurationmağazaların/eventGridFilters | Hayır |Hayır |

## <a name="microsoftazuregeneva"></a>Microsoft. Azure. Genfiliz

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | lý | Hayır |Hayır |
> | ortamlar/hesaplar | Hayır |Hayır |
> | ortamlar/hesaplar/ad alanları | Hayır |Hayır |
> | ortamlar/hesaplar/ad alanları/yapılandırma | Hayır |Hayır |

## <a name="microsoftazureactivedirectory"></a>Microsoft.AzureActiveDirectory

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | b2cDirectories | Evet |Hayır |
> | b2ctenants | Hayır |Hayır |

## <a name="microsoftazuredata"></a>Microsoft. AzureData

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | Sqlserverkayıtları | Evet |Evet |
> | Sqlserverkayıtları/sqlServers | Hayır |Hayır |

## <a name="microsoftazurestack"></a>Microsoft.AzureStack

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | kayıtlarında | Evet |Evet |
> | kayıt/müşteri abonelikleri | Hayır |Hayır |
> | kayıtlar/ürünler | Hayır |Hayır |

## <a name="microsoftbatch"></a>Microsoft.Batch

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | batchAccounts | Evet |Evet |

## <a name="microsoftbilling"></a>Microsoft.Billing

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | billingAccounts | Hayır |Hayır |
> | billingAccounts/anlaşmalar | Hayır |Hayır |
> | billingAccounts/billingProfiles | Hayır |Hayır |
> | billingAccounts/billingProfiles/Billingabonelikleri | Hayır |Hayır |
> | billingAccounts/billingProfiles/faturalar | Hayır |Hayır |
> | billingAccounts/billingProfiles/faturalar/fiyat listesi | Hayır |Hayır |
> | billingAccounts/billingProfiles/ınvoicesections | Hayır |Hayır |
> | billingAccounts/BillingProfiles/patchOperations | Hayır |Hayır |
> | billingAccounts/billingProfiles/paymentMethods | Hayır |Hayır |
> | billingAccounts/billingProfiles/ilkeler | Hayır |Hayır |
> | billingAccounts/billingProfiles/fiyat listesi | Hayır |Hayır |
> | billingAccounts/billingProfiles/pricesheetDownloadOperations | Hayır |Hayır |
> | billingAccounts/billingProfiles/ürünler | Hayır |Hayır |
> | billingAccounts/billingProfiles/işlemler | Hayır |Hayır |
> | billingAccounts/Billingabonelikleri | Hayır |Hayır |
> | billingAccounts/Createınvoicesectionoperations | Hayır |Hayır |
> | billingAccounts/müşteriler | Hayır |Hayır |
> | billingAccounts/müşteriler/Billingabonelikleri | Hayır |Hayır |
> | billingAccounts/departmanlar | Hayır |Hayır |
> | billingAccounts/KayıtSayısı | Hayır |Hayır |
> | billingAccounts/faturalar | Hayır |Hayır |
> | billingAccounts/ınvoicesections | Hayır |Hayır |
> | billingAccounts/ınvoicesections/billingSubscriptionMoveOperations | Hayır |Hayır |
> | billingAccounts/ınvoicesections/Billingabonelikleri | Hayır |Hayır |
> | billingAccounts/ınvoicesections/Billingabonelikleri/aktarımı | Hayır |Hayır |
> | billingAccounts/ınvoicesections/yükselt | Hayır |Hayır |
> | billingAccounts/ınvoicesections/ınitiatetransfer | Hayır |Hayır |
> | billingAccounts/ınvoicesections/patchOperations | Hayır |Hayır |
> | billingAccounts/ınvoicesections/productMoveOperations | Hayır |Hayır |
> | billingAccounts/Ürünler/Ürünler | Hayır |Hayır |
> | billingAccounts/ınvoicesections/ürünler/transfer | Hayır |Hayır |
> | billingAccounts/ınvoicesections/ürünler/updateAutoRenew | Hayır |Hayır |
> | billingAccounts/ınvoicesections/işlemler | Hayır |Hayır |
> | billingAccounts/ınvoicesections/aktarımlar | Hayır |Hayır |
> | billingAccounts/Lineofkredisi | Hayır |Hayır |
> | billingAccounts/patchOperations | Hayır |Hayır |
> | billingAccounts/paymentMethods | Hayır |Hayır |
> | billingAccounts/ürünler | Hayır |Hayır |
> | billingAccounts/işlemler | Hayır |Hayır |
> | Billingdönemler | Hayır |Hayır |
> | billingPermissions | Hayır |Hayır |
> | billingProperty | Hayır |Hayır |
> | Billingroleatamaları | Hayır |Hayır |
> | billingRoleDefinitions | Hayır |Hayır |
> | Createbillingroleatama | Hayır |Hayır |
> | bölümlerinin | Hayır |Hayır |
> | kayıt sayısı | Hayır |Hayır |
> | faturalardan | Hayır |Hayır |
> | girişinde | Hayır |Hayır |
> | aktarımlar/acceptTransfer | Hayır |Hayır |
> | aktarımlar/declineTransfer | Hayır |Hayır |
> | aktarımlar/operationStatus | Hayır |Hayır |
> | validateAddress | Hayır |Hayır |

## <a name="microsoftbingmaps"></a>Microsoft.BingMaps

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | Mapapsıs | Evet |Evet |
> | updateCommunicationPreference | Hayır |Hayır |

## <a name="microsoftbiztalkservices"></a>Microsoft. BizTalkServices

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | BizTalk | Evet |Evet |

## <a name="microsoftblockchain"></a>Microsoft. Blockzinciri

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | blockchainMembers | Evet |Evet |
> | izleyici | Evet |Evet |

## <a name="microsoftblueprint"></a>Microsoft. Blueprint

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | Şema tasmi | Hayır |Hayır |
> | Blueprintasbir/Atamaperations | Hayır |Hayır |
> | Blueprintasbir/işlemleri | Hayır |Hayır |
> | Blueprint | Hayır |Hayır |
> | planlar/yapıtlar | Hayır |Hayır |
> | planlar/sürümler | Hayır |Hayır |
> | planlar/sürümler/yapılar | Hayır |Hayır |

## <a name="microsoftbotservice"></a>Microsoft. BotService

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | botServices | Evet |Evet |
> | botServices/kanallar | Hayır |Hayır |
> | botServices/Connections | Hayır |Hayır |
> | Diller | Hayır |Hayır |
> | templates | Hayır |Hayır |

## <a name="microsoftcache"></a>Microsoft.Cache

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | Redis | Evet |Evet |
> | RedisConfigDefinition | Hayır |Hayır |

## <a name="microsoftcapacity"></a>Microsoft. Capacity

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | appliedReservations | Hayır |Hayır |
> | calculateExchange | Hayır |Hayır |
> | calculatePrice | Hayır |Hayır |
> | calculatePurchasePrice | Hayır |Hayır |
> | larına | Hayır |Hayır |
> | Ticari Vaalrezervler | Hayır |Hayır |
> | değişimi | Hayır |Hayır |
> | placePurchaseOrder | Hayır |Hayır |
> | Rezervler | Hayır |Hayır |
> | Rezervler/Hesaplaizterefund | Hayır |Hayır |
> | Rezervler/Birleştir | Hayır |Hayır |
> | Rezervler/rezervasyonlar | Hayır |Hayır |
> | Rezervler/rezervasyonlar/düzeltmeler | Hayır |Hayır |
> | Rezervler/geri dönüş | Hayır |Hayır |
> | Rezervler/Böl | Hayır |Hayır |
> | Rezervler/takas | Hayır |Hayır |
> | rezervasyonlar | Hayır |Hayır |
> | kaynaklar | Hayır |Hayır |
> | validateReservationOrder | Hayır |Hayır |

## <a name="microsoftcdn"></a>Microsoft.Cdn

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | CdnWebApplicationFirewallManagedRuleSets | Hayır |Hayır |
> | CdnWebApplicationFirewallPolicies | Evet |Evet |
> | edgenodes | Hayır |Hayır |
> | profiles | Evet |Evet |
> | Profiller/uç noktalar | Evet |Evet |
> | Profiller/uç noktalar/customdomains | Hayır |Hayır |
> | Profiller/uç noktalar/kaynaklar | Hayır |Hayır |
> | Validatearaştırması | Hayır |Hayır |

## <a name="microsoftcertificateregistration"></a>Microsoft.CertificateRegistration

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | Sertifikadüzenleri | Evet |Evet |
> | certificateOrders/Certificates | Hayır |Hayır |
> | Validatecertificateregistrationınformation | Hayır |Hayır |

## <a name="microsoftclassiccompute"></a>Microsoft.ClassicCompute

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | Yetenek | Hayır |Hayır |
> | domainNames | Evet |Evet |
> | domainNames/yetenekleri | Hayır |Hayır |
> | domainNames/internalLoadBalancers | Hayır |Hayır |
> | domainNames/serviceCertificates | Hayır |Hayır |
> | domainNames/Yuvaları | Hayır |Hayır |
> | domainNames/yuvalar/roller | Hayır |Hayır |
> | domainNames/yuvalar/roller/metricDefinitions | Hayır |Hayır |
> | domainNames/yuvalar/roller/ölçümler | Hayır |Hayır |
> | moveSubscriptionResources | Hayır |Hayır |
> | operatingSystemFamilies | Hayır |Hayır |
> | operatingSystems | Hayır |Hayır |
> | quotas | Hayır |Hayır |
> | resourceTypes | Hayır |Hayır |
> | Validatesubscriptionmoveavaılabılıty | Hayır |Hayır |
> | virtualMachines | Evet |Evet |
> | virtualMachines/diagnosticSettings | Hayır |Hayır |
> | virtualMachines/metricDefinitions | Hayır |Hayır |
> | virtualMachines/ölçümler | Hayır |Hayır |

## <a name="microsoftclassicinfrastructuremigrate"></a>Microsoft. ClassicInfrastructureMigrate

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | classicInfrastructureResources | Hayır |Hayır |

## <a name="microsoftclassicnetwork"></a>Microsoft.ClassicNetwork

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | Yetenek | Hayır |Hayır |
> | expressRouteCrossConnections | Hayır |Hayır |
> | expressRouteCrossConnections/peerler | Hayır |Hayır |
> | gatewaySupportedDevices | Hayır |Hayır |
> | networkSecurityGroups | Evet |Evet |
> | quotas | Hayır |Hayır |
> | Rezervler | Evet |Evet |
> | virtualNetworks | Evet |Evet |
> | virtualNetworks/Remotevirtualnetworkpeeringproxy 'Leri | Hayır |Hayır |
> | virtualNetworks/Virtualnetworkpeerler | Hayır |Hayır |

## <a name="microsoftclassicstorage"></a>Microsoft.ClassicStorage

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | Yetenek | Hayır |Hayır |
> | diskler | Hayır |Hayır |
> | görüntüler | Hayır |Hayır |
> | osImages | Hayır |Hayır |
> | Osplatformımages | Hayır |Hayır |
> | Publicımages | Hayır |Hayır |
> | quotas | Hayır |Hayır |
> | storageAccounts | Evet |Evet |
> | storageAccounts/metricDefinitions | Hayır |Hayır |
> | storageAccounts/ölçümler | Hayır |Hayır |
> | storageAccounts/Services | Hayır |Hayır |
> | storageAccounts/Services/diagnosticSettings | Hayır |Hayır |
> | storageAccounts/Services/metricDefinitions | Hayır |Hayır |
> | storageAccounts/Services/ölçümler | Hayır |Hayır |
> | storageAccounts/Vmımages | Hayır |Hayır |
> | Vmımages | Hayır |Hayır |

## <a name="microsoftcognitiveservices"></a>Microsoft.CognitiveServices

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | hesaplar | Evet |Evet |

## <a name="microsoftcommerce"></a>Microsoft. Commerce

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | RateCard | Hayır |Hayır |
> | Usagetoplamaları | Hayır |Hayır |

## <a name="microsoftcompute"></a>Microsoft.Compute

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | availabilitySets | Evet |Evet |
> | diskEncryptionSets | Evet |Evet |
> | diskler | Evet |Evet |
> | Galeriler | Evet |Evet |
> | Galeriler/uygulamalar | Evet |Evet |
> | Galeriler/uygulamalar/sürümler | Evet |Evet |
> | Galeriler/görüntüler | Evet |Evet |
> | Galeriler/resimler/sürümler | Evet |Evet |
> | hostGroups | Evet |Evet |
> | hostGroups/konaklar | Evet |Evet |
> | görüntüler | Evet |Evet |
> | proximityPlacementGroups | Evet |Evet |
> | restorePointCollections | Evet |Evet |
> | restorePointCollections/restorePoints | Hayır |Hayır |
> | Sharedvmımages | Evet |Evet |
> | Sharedvmımages/sürümler | Evet |Evet |
> | anlık görüntüler | Evet |Evet |
> | virtualMachines | Evet |Evet |
> | virtualMachines/uzantıları | Evet |Evet |
> | virtualMachines/metricDefinitions | Hayır |Hayır |
> | virtualMachines/scriptJobs | Hayır |Hayır |
> | virtualMachines/Softwareupdatedağıtımları | Hayır |Hayır |
> | virtualMachineScaleSets | Evet |Evet |
> | virtualMachineScaleSets/uzantılar | Hayır |Hayır |
> | virtualMachineScaleSets/NetworkInterfaces | Hayır |Hayır |
> | virtualMachineScaleSets/Publicıpaddresses | Hayır |Hayır |
> | virtualMachineScaleSets/virtualMachines | Hayır |Hayır |
> | virtualMachineScaleSets/virtualMachines/NetworkInterfaces | Hayır |Hayır |

## <a name="microsoftconsumption"></a>Microsoft. tüketim

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | Aggregmalyt maliyeti | Hayır |Hayır |
> | Bakiyeler | Hayır |Hayır |
> | Bütçeler | Hayır |Hayır |
> | Ücretler | Hayır |Hayır |
> | CostTags | Hayır |Hayır |
> | iler | Hayır |Hayır |
> | olaylar | Hayır |Hayır |
> | Tahminler | Hayır |Hayır |
> | oluş | Hayır |Hayır |
> | Marketlerinden | Hayır |Hayır |
> | Pricesheets | Hayır |Hayır |
> | ürünler | Hayır |Hayır |
> | Rezervde ayrıntıları | Hayır |Hayır |
> | Rezervationönerilere | Hayır |Hayır |
> | Rezervlerin Özeti | Hayır |Hayır |
> | Rezervlik Işlemleri | Hayır |Hayır |
> | Tags | Hayır |Hayır |
> | Kira | Hayır |Hayır |
> | Koşullar | Hayır |Hayır |
> | UsageDetails | Hayır |Hayır |

## <a name="microsoftcontainerinstance"></a>Microsoft.ContainerInstance

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | Kapsayıcı grupları | Evet |Evet |
> | serviceAssociationLinks | Hayır |Hayır |

## <a name="microsoftcontainerregistry"></a>Microsoft.ContainerRegistry

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | kayıt | Evet |Evet |
> | kayıt defterleri/derlemeler | Hayır |Hayır |
> | kayıt defterleri/derlemeler/iptal | Hayır |Hayır |
> | kayıt defterleri/derlemeler/getLogLink | Hayır |Hayır |
> | kayıt defterleri/buildTasks | Evet |Evet |
> | kayıt defterleri/buildTasks/Steps | Hayır |Hayır |
> | kayıt defterleri/eventGridFilters | Hayır |Hayır |
> | kayıt defterleri/getBuildSourceUploadUrl 'Si | Hayır |Hayır |
> | kayıt defterleri/GetCredentials | Hayır |Hayır |
> | kayıt defterleri/ımportımage | Hayır |Hayır |
> | kayıt defterleri/queueBuild | Hayır |Hayır |
> | kayıt defterleri/regenerateCredential | Hayır |Hayır |
> | kayıt defterleri/regenerateCredentials | Hayır |Hayır |
> | kayıt defterleri/çoğaltmalar | Evet |Evet |
> | kayıt defterleri/çalıştırmalar | Hayır |Hayır |
> | kayıt defterleri/çalıştırmalar/iptal | Hayır |Hayır |
> | kayıt defterleri/scheduleRun | Hayır |Hayır |
> | kayıt defterleri/görevler | Evet |Evet |
> | kayıt defterleri/updatePolicies | Hayır |Hayır |
> | kayıt defterleri/Web kancaları | Evet |Evet |
> | kayıt defterleri/Web kancaları/getCallbackConfig | Hayır |Hayır |
> | kayıt defterleri/Web kancaları/ping | Hayır |Hayır |

## <a name="microsoftcontainerservice"></a>Microsoft.ContainerService

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | containerServices | Evet |Evet |
> | Managedkümeler | Evet |Evet |
> | openShiftManagedClusters | Evet |Evet |

## <a name="microsoftcontentmoderator"></a>Microsoft.ContentModerator

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | uygulamalar | Evet |Evet |
> | updateCommunicationPreference | Hayır |Hayır |

## <a name="microsoftcortanaanalytics"></a>Microsoft. Cortanaanalizi

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | hesaplar | Evet |Evet |

## <a name="microsoftcostmanagement"></a>Microsoft. CostManagement

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | Uyarılar | Hayır |Hayır |
> | BillingAccounts | Hayır |Hayır |
> | Bütçeler | Hayır |Hayır |
> | Cloudbağlayıcıları | Hayır |Hayır |
> | Bağlayıcılar | Evet |Evet |
> | Bölümler | Hayır |Hayır |
> | Boyutlar | Hayır |Hayır |
> | Kayıt sayısı | Hayır |Hayır |
> | Dışarı aktarmalar | Hayır |Hayır |
> | ExternalBillingAccounts | Hayır |Hayır |
> | ExternalBillingAccounts/uyarılar | Hayır |Hayır |
> | ExternalBillingAccounts/Boyutlar | Hayır |Hayır |
> | ExternalBillingAccounts/tahmin | Hayır |Hayır |
> | ExternalBillingAccounts/sorgu | Hayır |Hayır |
> | Externalabonelikleri | Hayır |Hayır |
> | Externalabonelikleri/uyarıları | Hayır |Hayır |
> | Externalabonelikler/Boyutlar | Hayır |Hayır |
> | Externalabonelikler/tahmin | Hayır |Hayır |
> | Externalabonelikler/sorgu | Hayır |Hayır |
> | Tahmin | Hayır |Hayır |
> | Sorgu | Hayır |Hayır |
> | kaydolunamadı | Hayır |Hayır |
> | Reportconfigs | Hayır |Hayır |
> | Raporlar | Hayır |Hayır |
> | Ayarlar | Hayır |Hayır |
> | showbackRules | Hayır |Hayır |
> | Görünümler | Hayır |Hayır |

## <a name="microsoftcustomerinsights"></a>Microsoft.CustomerInsights

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | Lara | Evet |Evet |
> | Hub/authorizationPolicies | Hayır |Hayır |
> | Hub 'lar/bağlayıcılar | Hayır |Hayır |
> | Hub 'lar/bağlayıcılar/eşlemeler | Hayır |Hayır |
> | Hub 'lar/etkileşimler | Hayır |Hayır |
> | Hub/KPI | Hayır |Hayır |
> | Hub 'lar/bağlantılar | Hayır |Hayır |
> | Hub 'lar/profiller | Hayır |Hayır |
> | Hub/Roleatamaları | Hayır |Hayır |
> | Hub 'lar/roller | Hayır |Hayır |
> | Hub 'lar/suggestTypeSchema | Hayır |Hayır |
> | Hub 'lar/görünümler | Hayır |Hayır |
> | Hub 'lar/widgetTypes | Hayır |Hayır |

## <a name="microsoftcustomerlockbox"></a>Microsoft. Customerkasası

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | istekler | Hayır |Hayır |

## <a name="microsoftcustomproviders"></a>Microsoft. CustomProviders

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | içermektedir | Hayır |Hayır |
> | resourceProviders | Evet |Evet |

## <a name="microsoftdatabox"></a>Microsoft. DataBox

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | işler | Evet |Evet |

## <a name="microsoftdataboxedge"></a>Microsoft. DataBoxEdge

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | DataBoxEdgeDevices | Evet |Evet |

## <a name="microsoftdatabricks"></a>Microsoft. Databricks

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | çalışma alanı | Evet |Hayır |
> | çalışma alanları/Virtualnetworkpeerler | Hayır |Hayır |

## <a name="microsoftdatacatalog"></a>Microsoft.DataCatalog

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | larına | Evet |Evet |
> | veri katalogları | Evet |Evet |
> | veri katalogları/scantargets | Hayır |Hayır |
> | veri katalogları/scantargets/veri kümeleri | Hayır |Hayır |

## <a name="microsoftdataconnect"></a>Microsoft. DataConnect

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | Connectionyöneticileri | Evet |Evet |

## <a name="microsoftdatafactory"></a>Microsoft.DataFactory

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | Veri fabrikaları | Evet |Hayır |
> | DataFactory/diagnosticSettings | Hayır |Hayır |
> | DataFactory/metricDefinitions | Hayır |Hayır |
> | dataFactorySchema | Hayır |Hayır |
> | larının | Evet |Hayır |
> | Fabrika/tümleştirme çalışma zamanları | Hayır |Hayır |

## <a name="microsoftdatalakeanalytics"></a>Microsoft.DataLakeAnalytics

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | hesaplar | Evet |Evet |
> | hesaplar/dataLakeStoreAccounts | Hayır |Hayır |
> | hesaplar/storageAccounts | Hayır |Hayır |
> | hesaplar/storageAccounts/kapsayıcılar | Hayır |Hayır |
> | hesaplar/Transferanaliz tici | Hayır |Hayır |

## <a name="microsoftdatalakestore"></a>Microsoft.DataLakeStore

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | hesaplar | Evet |Evet |
> | hesaplar/eventGridFilters | Hayır |Hayır |
> | hesaplar/firewallRules | Hayır |Hayır |

## <a name="microsoftdatamigration"></a>Microsoft. DataMigration

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | services | Evet |Evet |
> | Hizmetler/Projeler | Evet |Evet |
> | Lara | Evet |Evet |

## <a name="microsoftdatashare"></a>Microsoft. DataShare

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | hesaplar | Evet |Evet |
> | hesaplar/paylaşımlar | Hayır |Hayır |
> | hesaplar/paylaşımlar/veri kümeleri | Hayır |Hayır |
> | hesaplar/paylaşımlar/davetler | Hayır |Hayır |
> | hesaplar/paylaşımlar/providersharesubscriptions | Hayır |Hayır |
> | hesaplar/paylaşımlar/synchronizationSettings | Hayır |Hayır |
> | hesaplar/parçalar esubscriptions | Hayır |Hayır |
> | hesaplar/parçalar esubscriptions/Consumersourcedataset 'ler | Hayır |Hayır |
> | hesaplar/parçalar esubscriptions/datasetmappings | Hayır |Hayır |
> | hesaplar/parçalar esubscriptions/Tetikleyiciler | Hayır |Hayır |

## <a name="microsoftdbformariadb"></a>Microsoft. Dbformarıdb

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | sunucu | Evet |Evet |
> | sunucular/danışmanları | Hayır |Hayır |
> | sunucular/Querymetinmetinleri | Hayır |Hayır |
> | sunucular/recoverableServers | Hayır |Hayır |
> | sunucular/topQueryStatistics | Hayır |Hayır |
> | sunucular/virtualNetworkRules | Hayır |Hayır |
> | sunucular/waitStatistics | Hayır |Hayır |

## <a name="microsoftdbformysql"></a>Microsoft.DBforMySQL

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | sunucu | Evet |Evet |
> | sunucular/danışmanları | Hayır |Hayır |
> | sunucular/Querymetinmetinleri | Hayır |Hayır |
> | sunucular/recoverableServers | Hayır |Hayır |
> | sunucular/topQueryStatistics | Hayır |Hayır |
> | sunucular/virtualNetworkRules | Hayır |Hayır |
> | sunucular/waitStatistics | Hayır |Hayır |

## <a name="microsoftdbforpostgresql"></a>Microsoft.DBforPostgreSQL

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | Sunucu grupları | Evet |Evet |
> | sunucu | Evet |Evet |
> | sunucular/danışmanları | Hayır |Hayır |
> | sunucular/Querymetinmetinleri | Hayır |Hayır |
> | sunucular/recoverableServers | Hayır |Hayır |
> | sunucular/topQueryStatistics | Hayır |Hayır |
> | sunucular/virtualNetworkRules | Hayır |Hayır |
> | sunucular/waitStatistics | Hayır |Hayır |
> | serversv2 | Evet |Evet |

## <a name="microsoftdeploymentmanager"></a>Microsoft. DeploymentManager

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | artifactSources | Evet |Evet |
> | piyasaya çıkarma | Evet |Evet |
> | Servicetopolojileri | Evet |Evet |
> | Servicetopolojileri/hizmetler | Evet |Evet |
> | Servicetopolojileri/hizmetler/serviceUnits | Evet |Evet |
> | adımlar | Evet |Evet |

## <a name="microsoftdesktopvirtualization"></a>Microsoft. DesktopVirtualization

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | applicationgroups | Evet |Evet |
> | applicationgroups/uygulamalar | Hayır |Hayır |
> | applicationgroups/assignedusers | Hayır |Hayır |
> | applicationgroups/startmenuıtems | Hayır |Hayır |
> | Ana bilgisayar havuzları | Evet |Evet |
> | hostpools/oturumkonakları | Hayır |Hayır |
> | hostpools/sessionkonakları/usersessions | Hayır |Hayır |
> | hosthavuzlar/usersessions | Hayır |Hayır |
> | çalışma alanı | Evet |Evet |

## <a name="microsoftdevices"></a>Microsoft. Devices

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | Elaun havuzları | Evet |Evet |
> | Elaun havuzları/ıothubkiracılar | Evet |Evet |
> | Iothubs | Evet |Evet |
> | IotHubs/eventGridFilters | Hayır |Hayır |
> | ProvisioningServices | Evet |Evet |
> | vardır | Hayır |Hayır |

## <a name="microsoftdevops"></a>Microsoft. DevOps

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | ardışık düzenler | Evet |Evet |

## <a name="microsoftdevspaces"></a>Microsoft. DevSpaces

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | örleri | Evet |Evet |

## <a name="microsoftdevtestlab"></a>Microsoft.DevTestLab

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | labcenters | Evet |Evet |
> | larda | Evet |Evet |
> | Laboratuvarlar/ortamlar | Evet |Evet |
> | Labs/Servicerunanlar | Evet |Evet |
> | Labs/virtualMachines | Evet |Evet |
> | cağını | Evet |Evet |

## <a name="microsoftdocumentdb"></a>Microsoft.DocumentDB

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | databaseAccountNames | Hayır |Hayır |
> | Veritabanı hesapları | Evet |Evet |

## <a name="microsoftdomainregistration"></a>Microsoft.DomainRegistration

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | etki alanları | Evet |Evet |
> | Domains/Domainownershiptanýmlayýcýlarý | Hayır |Hayır |
> | generateSsoRequest | Hayır |Hayır |
> | topLevelDomains | Hayır |Hayır |
> | Validatedomainregistrationınformation | Hayır |Hayır |

## <a name="microsoftdynamicslcs"></a>Microsoft.DynamicsLcs

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | lcsprojects | Hayır |Hayır |
> | lcsprojects/clouddağıtımları | Hayır |Hayır |
> | lcsprojects/bağlayıcılar | Hayır |Hayır |

## <a name="microsoftenterpriseknowledgegraph"></a>Microsoft. EnterpriseKnowledgeGraph

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | services | Evet |Evet |

## <a name="microsofteventgrid"></a>Microsoft.EventGrid

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | etki alanları | Evet |Evet |
> | etki alanları/konular | Hayır |Hayır |
> | Eventabonelikleri | Hayır |Hayır |
> | Extensionkonuları | Hayır |Hayır |
> | konularıyla | Evet |Evet |
> | topicTypes | Hayır |Hayır |

## <a name="microsofteventhub"></a>Microsoft.EventHub

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | leriniz | Evet |Evet |
> | ad alanları | Evet |Evet |
> | ad alanları/authorizationrules | Hayır |Hayır |
> | ad alanları/diskalrecoveryconfigs | Hayır |Hayır |
> | ad alanları/eventhubs | Hayır |Hayır |
> | ad alanları/eventhubs/authorizationrules | Hayır |Hayır |
> | ad alanları/eventhubs/consumergroups | Hayır |Hayır |
> | ad alanları/networkrulesets | Hayır |Hayır |

## <a name="microsoftfeatures"></a>Microsoft. Features

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | SaaS Uygulamaları Geliştirme | Hayır |Hayır |
> | sağlayıcıları | Hayır |Hayır |

## <a name="microsoftgallery"></a>Microsoft. Gallery

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | kaydedemez | Hayır |Hayır |
> | gallergıtems | Hayır |Hayır |
> | generateartifactaccessuri | Hayır |Hayır |
> | myarea | Hayır |Hayır |
> | myarea/alan | Hayır |Hayır |
> | myarea/alan/alan | Hayır |Hayır |
> | myareas/Areas/Areas/gallergıtems | Hayır |Hayır |
> | myareas/Areas/gallergıtems | Hayır |Hayır |
> | myarea/gallergıtems | Hayır |Hayır |
> | kaydolunamadı | Hayır |Hayır |
> | kaynaklar | Hayır |Hayır |
> | elde edilecek esourcesbyıd | Hayır |Hayır |

## <a name="microsoftgenomics"></a>Microsoft. Genomiks

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | hesaplar | Evet |Evet |

## <a name="microsoftguestconfiguration"></a>Microsoft. GuestConfiguration

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | Guestconfigurationatamaları | Hayır |Hayır |
> | yazılımıdır | Hayır |Hayır |
> | softwareUpdateProfile | Hayır |Hayır |
> | softwareUpdates | Hayır |Hayır |

## <a name="microsofthanaonazure"></a>Microsoft. HanaOnAzure

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | Hanaınstances | Evet |Evet |
> | Sapizleyicileri | Evet |Evet |

## <a name="microsofthardwaresecuritymodules"></a>Microsoft. HardwareSecurityModules

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | ayrılmış Atedhsms | Evet |Evet |

## <a name="microsofthdinsight"></a>Microsoft.HDInsight

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | leriniz | Evet |Evet |
> | kümeler/uygulamalar | Hayır |Hayır |

## <a name="microsofthealthcareapis"></a>Microsoft. Healthgelişme API 'leri

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | services | Evet |Evet |

## <a name="microsofthybridcompute"></a>Microsoft. HybridCompute

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | makineler | Evet |Evet |

## <a name="microsofthybriddata"></a>Microsoft. HybridData

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | Veri yöneticileri | Evet |Evet |

## <a name="microsofthydra"></a>Microsoft. Hydra

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | bileşenleri | Evet |Evet |
> | networkScopes | Evet |Evet |

## <a name="microsoftimportexport"></a>Microsoft.ImportExport

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | işler | Evet |Evet |

## <a name="microsoftintune"></a>Microsoft. Intune

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | diagnosticSettings | Hayır |Hayır |
> | diagnosticSettingsCategories | Hayır |Hayır |

## <a name="microsoftiotcentral"></a>Microsoft. ıotcentral

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | appTemplates | Hayır |Hayır |
> | Iotapps | Evet |Evet |

## <a name="microsoftiotspaces"></a>Microsoft. ıotspaces

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | Graf | Evet |Evet |

## <a name="microsoftkeyvault"></a>Microsoft.KeyVault

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | Silinkaults | Hayır |Hayır |
> | hsmPools | Evet |Evet |
> | kasaları | Evet |Evet |
> | kasa/erişim Ilkeleri | Hayır |Hayır |
> | kasa/eventGridFilters | Hayır |Hayır |
> | kasa/gizlilikler | Hayır |Hayır |

## <a name="microsoftkusto"></a>Microsoft. kusto

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | leriniz | Evet |Evet |
> | kümeler/attacheddatabaseconfigurations | Hayır |Hayır |
> | kümeler/veritabanları | Hayır |Hayır |
> | kümeler/veritabanları/veri bağlantıları | Hayır |Hayır |
> | kümeler/veritabanları/eventhubconnections | Hayır |Hayır |

## <a name="microsoftlabservices"></a>Microsoft. LabServices

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | labaccounts | Evet |Evet |
> | kullanıcı | Hayır |Hayır |

## <a name="microsoftlogic"></a>Microsoft.Logic

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | hostingEnvironments | Evet |Evet |
> | Tümleştirme hesapları | Evet |Evet |
> | ıntegrationserviceortamortamları | Evet |Evet |
> | ısotedenvironments | Evet |Evet |
> | sürdürülen | Evet |Evet |

## <a name="microsoftmachinelearning"></a>Microsoft.MachineLearning

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | Commitmentplanlar | Evet |Evet |
> | Hizmetleri | Evet |Evet |
> | Çalışma Alanı | Evet |Evet |

## <a name="microsoftmachinelearningservices"></a>Microsoft. MachineLearningServices

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | çalışma alanı | Evet |Evet |
> | çalışma alanları/hesaplar | Hayır |Hayır |

## <a name="microsoftmanagedidentity"></a>Microsoft. Managedıdentity

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | Kimlikler | Hayır |Hayır |
> | Userassignedıdentities | Evet |Evet |

## <a name="microsoftmanagedlab"></a>Microsoft. ManagedLab

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | labaccounts | Evet |Evet |

## <a name="microsoftmanagedservices"></a>Microsoft. ManagedServices

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | Pazar Placeryumuristrationdefinitions | Hayır |Hayır |
> | Registrationatamaları | Hayır |Hayır |
> | registrationDefinitions | Hayır |Hayır |

## <a name="microsoftmanagement"></a>Microsoft. Management

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | getEntities | Hayır |Hayır |
> | Yönetim grupları | Hayır |Hayır |
> | kaynaklar | Hayır |Hayır |
> | startTenantBackfill | Hayır |Hayır |
> | tenantBackfillStatus | Hayır |Hayır |

## <a name="microsoftmaps"></a>Microsoft. Maps

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | hesaplar | Evet |Evet |
> | hesaplar/eventGridFilters | Hayır |Hayır |

## <a name="microsoftmarketplace"></a>Microsoft. Market

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | sunar | Hayır |Hayır |
> | offerTypes | Hayır |Hayır |
> | offerTypes/yayımcılar | Hayır |Hayır |
> | offerTypes/yayımcılar/teklifler | Hayır |Hayır |
> | offerTypes/yayımcılar/teklifler/planlar | Hayır |Hayır |
> | offerTypes/yayımcılar/teklifler/planlar/anlaşmalar | Hayır |Hayır |
> | offerTypes/yayımcılar/teklifler/planlar/configs | Hayır |Hayır |
> | offerTypes/yayımcılar/teklifler/planlar/configs/ımportımage | Hayır |Hayır |
> | privategallergıtems | Hayır |Hayır |
> | ürünler | Hayır |Hayır |
> | yayımcılar | Hayır |Hayır |
> | Yayımcılar/teklifler | Hayır |Hayır |
> | Yayımcılar/teklifler/Düzeltme | Hayır |Hayır |

## <a name="microsoftmarketplaceapps"></a>Microsoft. MarketplaceApps

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | classicDevServices | Evet |Evet |
> | updateCommunicationPreference | Hayır |Hayır |

## <a name="microsoftmarketplaceordering"></a>Microsoft. Marketplacesıralaması

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | anlaşmalar | Hayır |Hayır |
> | offertypes | Hayır |Hayır |

## <a name="microsoftmedia"></a>Microsoft. Media

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | mediaservices | Evet |Evet |
> | mediaservices/accountFilters | Hayır |Hayır |
> | mediaservices/varlıklar | Hayır |Hayır |
> | mediaservices/varlıklar/assetFilters | Hayır |Hayır |
> | mediaservices/contentKeyPolicies | Hayır |Hayır |
> | mediaservices/eventGridFilters | Hayır |Hayır |
> | mediaservices/liveEventOperations | Hayır |Hayır |
> | mediaservices/liveEvents | Evet |Evet |
> | mediaservices/liveEvents/Liveçıktılar | Hayır |Hayır |
> | mediaservices/liveOutputOperations | Hayır |Hayır |
> | mediaservices/streamingEndpointOperations | Hayır |Hayır |
> | mediaservices/streamingEndpoints | Evet |Evet |
> | mediaservices/Streamingkonumlandırıcı | Hayır |Hayır |
> | mediaservices/streamingPolicies | Hayır |Hayır |
> | mediaservices/dönüşümler | Hayır |Hayır |
> | mediaservices/dönüşümler/işler | Hayır |Hayır |

## <a name="microsoftmicroservices4spring"></a>Microsoft. Microservices4Spring

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | Appkümeler | Evet |Evet |

## <a name="microsoftmigrate"></a>Microsoft. Migrate

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | assessmentProjects | Evet |Evet |
> | migrateprojects | Evet |Evet |
> | projeyle | Evet |Evet |

## <a name="microsoftmixedreality"></a>Microsoft. MixedReality

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | remoteRenderingAccounts | Evet |Evet |
> | spatialAnchorsAccounts | Evet |Evet |

## <a name="microsoftnetapp"></a>Microsoft. NetApp

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | netAppAccounts | Evet |Evet |
> | netAppAccounts/Capacityhavuzları | Evet |Evet |
> | netAppAccounts/Capacityhavuzları/birimleri | Evet |Evet |
> | netAppAccounts/Capacityhavuzlar/Volumes/Mount hedefleri | Evet |Evet |
> | netAppAccounts/Capacityhavuzlar/birimler/anlık görüntüler | Evet |Evet |

## <a name="microsoftnetwork"></a>Microsoft.Network

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | Applicationgateway 'ler | Evet |Evet |
> | applicationGatewayWebApplicationFirewallPolicies | Evet |Evet |
> | applicationSecurityGroups | Evet |Evet |
> | azureFirewallFqdnTags | Hayır |Hayır |
> | azureFirewalls | Evet |Evet |
> | Savunma Konakları | Evet |Evet |
> | bgpServiceCommunities | Hayır |Hayır |
> | bağlantılar | Evet |Evet |
> | ddosCustomPolicies | Evet |Evet |
> | Ddosprotectionplanlar | Evet |Evet |
> | Dnsoperationdurumları | Hayır |Hayır |
> | dnszones | Evet |Evet |
> | dnszones/A | Hayır |Hayır |
> | dnszones/AAAA | Hayır |Hayır |
> | dnszones/tümü | Hayır |Hayır |
> | dnszones/CAA | Hayır |Hayır |
> | dnszones/CNAME | Hayır |Hayır |
> | dnszones/MX | Hayır |Hayır |
> | dnszones/NS | Hayır |Hayır |
> | dnszones/PTR | Hayır |Hayır |
> | dnszones/kayıt kümeleri | Hayır |Hayır |
> | dnszones/SOA | Hayır |Hayır |
> | dnszones/SRV | Hayır |Hayır |
> | dnszones/TXT | Hayır |Hayır |
> | Expressroutedevreleri | Evet |Evet |
> | expressRouteCrossConnections | Evet |Evet |
> | Expressroutegateway 'ler | Evet |Evet |
> | expressRoutePorts | Evet |Evet |
> | expressRouteServiceProviders | Hayır |Hayır |
> | firewallPolicies | Evet |Evet |
> | frontdoors | Evet, ancak sınırlı ( [aşağıdaki nota](#frontdoor)bakın) |Evet |
> | frontdoorWebApplicationFirewallManagedRuleSets | Evet, ancak sınırlı ( [aşağıdaki nota](#frontdoor)bakın) |Hayır |
> | frontdoorWebApplicationFirewallPolicies | Evet, ancak sınırlı ( [aşağıdaki nota](#frontdoor)bakın) |Evet |
> | getDnsResourceReference | Hayır |Hayır |
> | ınternalnotify | Hayır |Hayır |
> | loadBalancers | Evet |Hayır |
> | Localnetworkgateway 'ler | Evet |Evet |
> | Natgateway 'ler | Evet |Evet |
> | Networkıntpolicies Ilkeleri | Evet |Evet |
> | NetworkInterfaces | Evet |Evet |
> | networkProfiles | Evet |Evet |
> | networkSecurityGroups | Evet |Evet |
> | networkWatchers | Evet |Hayır |
> | networkWatchers/Connectionmonitörleri | Evet |Hayır |
> | networkWatchers/uzunluler | Evet |Hayır |
> | networkWatchers/Pingkafesler | Evet |Hayır |
> | p2sVpnGateways | Evet |Evet |
> | privateDnsOperationStatuses | Hayır |Hayır |
> | privateDnsZones | Evet |Evet |
> | privateDnsZones/A | Hayır |Hayır |
> | privateDnsZones/AAAA | Hayır |Hayır |
> | privateDnsZones/tümü | Hayır |Hayır |
> | privateDnsZones/CNAME | Hayır |Hayır |
> | privateDnsZones/MX | Hayır |Hayır |
> | privateDnsZones/PTR | Hayır |Hayır |
> | privateDnsZones/SOA | Hayır |Hayır |
> | privateDnsZones/SRV | Hayır |Hayır |
> | privateDnsZones/TXT | Hayır |Hayır |
> | privateDnsZones/virtualNetworkLinks | Evet |Evet |
> | privateEndpoints | Evet |Evet |
> | privateLinkServices | Evet |Evet |
> | Publicıpaddresses | Evet |Evet |
> | Publicıpöneklerini | Evet |Evet |
> | routeFilters | Evet |Evet |
> | routeTables | Evet |Evet |
> | Securegateyöntemlere | Evet |Evet |
> | serviceEndpointPolicies | Evet |Evet |
> | trafficManagerGeographicHierarchies | Hayır |Hayır |
> | trafficmanagerprofiles | Evet |Evet |
> | trafficmanagerprofiles/heatMaps | Hayır |Hayır |
> | trafficManagerUserMetricsKeys | Hayır |Hayır |
> | Virtualhub 'Lar | Evet |Evet |
> | virtualNetworkGateways | Evet |Evet |
> | virtualNetworks | Evet |Evet |
> | virtualNetworkTaps | Evet |Evet |
> | Virtualwan | Evet |Evet |
> | Vpngateway 'ler | Evet |Hayır |
> | vpnSites | Evet |Evet |
> | webApplicationFirewallPolicies | Evet |Evet |

<a id="frontdoor" />

> [!NOTE]
> Azure ön kapı hizmeti için, kaynak oluştururken Etiketler uygulayabilirsiniz, ancak bu durumda etiketleri güncelleştirmek veya eklemek Şu anda desteklenmemektedir.

## <a name="microsoftnotificationhubs"></a>Microsoft.NotificationHubs

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | ad alanları | Evet |Hayır |
> | ad alanları/Notificationhub 'Lar | Evet |Hayır |

## <a name="microsoftoffazure"></a>Microsoft. OffAzure

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | Hiper sanal siteler | Evet |Evet |
> | Importsites | Evet |Evet |
> | Sunucusiteleri | Evet |Evet |
> | VMwareSites | Evet |Evet |

## <a name="microsoftoperationalinsights"></a>Microsoft.OperationalInsights

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | cihazlar | Hayır |Hayır |
> | Bağlantı hedefleri | Hayır |Hayır |
> | Storageınsii configs | Hayır |Hayır |
> | çalışma alanı | Evet |Evet |
> | çalışma alanları/veri kaynakları | Hayır |Hayır |
> | çalışma alanları/linkedServices | Hayır |Hayır |
> | çalışma alanları/sorgu | Hayır |Hayır |

## <a name="microsoftoperationsmanagement"></a>Microsoft.OperationsManagement

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | managementassociations | Hayır |Hayır |
> | managementconfigurations | Evet |Evet |
> | çözümler | Evet |Evet |
> | görüntüleme | Evet |Evet |

## <a name="microsoftpeering"></a>Microsoft. eşleme

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | Yasallıklar | Hayır |Hayır |
> | peerAsns | Hayır |Hayır |
> | eşlemeler | Evet |Evet |

## <a name="microsoftpolicyinsights"></a>Microsoft. Poliyelei

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | Poliyevents | Hayır |Hayır |
> | policyStates | Hayır |Hayır |
> | policyTrackedResources | Hayır |Hayır |
> | düzeltmeler | Hayır |Hayır |

## <a name="microsoftportal"></a>Microsoft. Portal

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | konsolları | Hayır |Hayır |
> | panoların | Evet |Evet |
> | userSettings | Hayır |Hayır |

## <a name="microsoftpowerbi"></a>Microsoft. PowerBI

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | workspaceCollections | Evet |Evet |

## <a name="microsoftpowerbidedicated"></a>Microsoft.PowerBIDedicated

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | kapasiteler | Evet |Evet |

## <a name="microsoftrecoveryservices"></a>Microsoft. RecoveryServices

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | Backupkorunabilir | Hayır |Hayır |
> | kasaları | Evet |Evet |

## <a name="microsoftrelay"></a>Microsoft. Relay

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | ad alanları | Evet |Evet |
> | ad alanları/authorizationrules | Hayır |Hayır |
> | ad alanları/hybridconnections | Hayır |Hayır |
> | ad alanları/hybridconnections/authorizationrules | Hayır |Hayır |
> | ad alanları/wcfreyerleştiri | Hayır |Hayır |
> | ad alanları/wcfreyerleştirme/authorizationrules | Hayır |Hayır |

## <a name="microsoftremoteapp"></a>Microsoft. RemoteApp

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | hesaplar | Hayır |Hayır |
> | koleksiyonlarıyla | Evet |Evet |
> | Koleksiyonlar/uygulamalar | Hayır |Hayır |
> | Koleksiyonlar/SecurityPrincipals | Hayır |Hayır |
> | Templateımages | Hayır |Hayır |

## <a name="microsoftresourcegraph"></a>Microsoft. ResourceGraph

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | sorgu | Evet |Evet |
> | resourceChangeDetails | Hayır |Hayır |
> | resourceChanges | Hayır |Hayır |
> | kaynaklar | Hayır |Hayır |
> | resourcesHistory | Hayır |Hayır |
> | subscriptionsStatus | Hayır |Hayır |

## <a name="microsoftresourcehealth"></a>Microsoft. ResourceHealth

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | Kullanılabilirlik durumları | Hayır |Hayır |
> | Childadvailabilitydurumlar | Hayır |Hayır |
> | childResources | Hayır |Hayır |
> | olaylar | Hayır |Hayır |
> | ımpactedresources | Hayır |Hayır |
> | meta veriler | Hayır |Hayır |
> | bildirimler | Hayır |Hayır |

## <a name="microsoftresources"></a>Microsoft. resources

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | dağıtımlar | Hayır |Hayır |
> | dağıtımlar/işlemler | Hayır |Hayır |
> | Köprü | Hayır |Hayır |
> | notifyResourceJobs | Hayır |Hayır |
> | sağlayıcıları | Hayır |Hayır |
> | resourceGroups | Hayır |Hayır |
> | kaynaklar | Hayır |Hayır |
> | abonelik | Hayır |Hayır |
> | Abonelikler/sağlayıcılar | Hayır |Hayır |
> | Abonelikler/resourceGroups | Hayır |Hayır |
> | Abonelikler/ResourceGroups/kaynaklar | Hayır |Hayır |
> | Abonelikler/kaynaklar | Hayır |Hayır |
> | Abonelikler/etiket adları | Hayır |Hayır |
> | Abonelikler/etiket adları/tagValues | Hayır |Hayır |
> | tags | Hayır |Hayır |
> | Kira | Hayır |Hayır |

## <a name="microsoftsaas"></a>Microsoft. SaaS

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | uygulamalar | Evet |Evet |
> | saasresources | Hayır |Hayır |

## <a name="microsoftscheduler"></a>Microsoft. Scheduler

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | akışlar | Evet |Evet |
> | işlere | Evet |Evet |

## <a name="microsoftsearch"></a>Microsoft.Search

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | resourceHealthMetadata | Hayır |Hayır |
> | searchServices | Evet |Evet |

## <a name="microsoftsecurity"></a>Microsoft.Security

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | adaptiveNetworkHardenings | Hayır |Hayır |
> | advancedThreatProtectionSettings | Hayır |Hayır |
> | alerts | Hayır |Hayır |
> | allowedConnections | Hayır |Hayır |
> | Applicationwhitedökümler | Hayır |Hayır |
> | assessmentMetadata | Hayır |Hayır |
> | kopyalan | Hayır |Hayır |
> | Oto Provisioningsettings | Hayır |Hayır |
> | Uyumluluklarına | Hayır |Hayır |
> | dataCollectionAgents | Hayır |Hayır |
> | deviceSecurityGroups | Hayır |Hayır |
> | discoveredSecuritySolutions | Hayır |Hayır |
> | externalSecuritySolutions | Hayır |Hayır |
> | Informationprotectionpolicies | Hayır |Hayır |
> | ıotsecuritysolutions | Evet |Evet |
> | ıotsecuritysolutions/analiz Ticsmodeller | Hayır |Hayır |
> | ıotsecuritysolutions/Analticsmodeller/Aggreggıt uyarıları | Hayır |Hayır |
> | ıotsecuritysolutions/analiz Ticsmodeller/Aggreg, öneriler | Hayır |Hayır |
> | Jağaccesspolicies | Hayır |Hayır |
> | playbookConfigurations | Evet |Evet |
> | ilkeler | Hayır |Hayır |
> | fiyatlandırmalar | Hayır |Hayır |
> | Reve daha karmaşık bakım standartları | Hayır |Hayır |
> | Rekontrol ve Re, | Hayır |Hayır |
> | Rekontrol ve Re, Re, Re, | Hayır |Hayır |
> | securityContacts | Hayır |Hayır |
> | securitySolutions | Hayır |Hayır |
> | securitySolutionsReferenceData | Hayır |Hayır |
> | Securitydurumlardan | Hayır |Hayır |
> | securityStatusesSummaries | Hayır |Hayır |
> | Sunucukullanılabilirliği | Hayır |Hayır |
> | ayarlar | Hayır |Hayır |
> | Görevler | Hayır |Hayır |
> | anlatır | Hayır |Hayır |
> | çalışma alanı ayarları | Hayır |Hayır |

## <a name="microsoftsecuritygraph"></a>Microsoft. SecurityGraph

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | diagnosticSettings | Hayır |Hayır |
> | diagnosticSettingsCategories | Hayır |Hayır |

## <a name="microsoftsecurityinsights"></a>Microsoft. Securityınsights

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | Verilerinize dair öngörüler | Hayır |Hayır |
> | alertRules | Hayır |Hayır |
> | leriniz | Hayır |Hayır |
> | çalışmaların | Hayır |Hayır |
> | Veri bağlayıcıları | Hayır |Hayır |
> | varlıklar | Hayır |Hayır |
> | entityQueries | Hayır |Hayır |
> | officeConsents | Hayır |Hayır |
> | ayarlar | Hayır |Hayır |

## <a name="microsoftservicebus"></a>Microsoft.ServiceBus

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | ad alanları | Evet |Hayır |
> | ad alanları/authorizationrules | Hayır |Hayır |
> | ad alanları/diskalrecoveryconfigs | Hayır |Hayır |
> | ad alanları/eventgridfilters | Hayır |Hayır |
> | ad alanları/networkrulesets | Hayır |Hayır |
> | ad alanları/kuyruklar | Hayır |Hayır |
> | ad alanları/kuyruklar/authorizationrules | Hayır |Hayır |
> | ad alanları/konular | Hayır |Hayır |
> | ad alanları/konular/authorizationrules | Hayır |Hayır |
> | ad alanları/konular/abonelikler | Hayır |Hayır |
> | ad alanları/konular/abonelikler/kurallar | Hayır |Hayır |
> | premiumMessagingRegions | Hayır |Hayır |

## <a name="microsoftservicefabric"></a>Microsoft.ServiceFabric

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | uygulamalar | Evet |Evet |
> | leriniz | Evet |Evet |
> | kümeler/uygulamalar | Hayır |Hayır |
> | Kapsayıcı grupları | Evet |Evet |
> | containerGroupSets | Evet |Evet |
> | edgeclusters | Evet |Evet |
> | edgeclusters/uygulamalar | Hayır |Hayır |
> | ağlar | Evet |Evet |
> | secretmağazaları | Evet |Evet |
> | secretmağazaları/sertifikaları | Hayır |Hayır |
> | secretmağazaları/gizli dizileri | Hayır |Hayır |
> | birim | Evet |Evet |

## <a name="microsoftservicefabricmesh"></a>Microsoft. Servicefabrickafesi

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | uygulamalar | Evet |Evet |
> | Kapsayıcı grupları | Evet |Evet |
> | geçidinin | Evet |Evet |
> | ağlar | Evet |Evet |
> | gizli dizi | Evet |Evet |
> | birim | Evet |Evet |

## <a name="microsoftservices"></a>Microsoft. Services

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | Providerkayıtları | Hayır |Hayır |
> | Providerkayıtlarıyla/resourceTypeRegistrations | Hayır |Hayır |
> | piyasaya çıkarma | Evet |Evet |

## <a name="microsoftsignalrservice"></a>Microsoft. SignalRService

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | SignalR | Evet |Evet |
> | SignalR/eventGridFilters | Hayır |Hayır |

## <a name="microsoftsiterecovery"></a>Microsoft.SiteRecovery

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | SiteRecoveryVault | Evet |Evet |

## <a name="microsoftsoftwareplan"></a>Microsoft. SoftwarePlan

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | Hybriduseavantajlar | Hayır |Hayır |

## <a name="microsoftsolutions"></a>Microsoft. Solutions

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | applicationDefinitions | Evet |Evet |
> | uygulamalar | Evet |Evet |
> | Jistekleri | Evet |Evet |

## <a name="microsoftsql"></a>Microsoft. SQL

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | ManagedInstances | Evet | Evet |
> | ManagedInstances/veritabanları | Evet ( [aşağıdaki nota](#sqlnote)bakın) | Evet |
> | ManagedInstances/veritabanları/backupShortTermRetentionPolicies | Hayır | Hayır |
> | ManagedInstances/veritabanları/şemalar/tablolar/sütunlar/sensitivityLabels | Hayır | Hayır |
> | ManagedInstances/veritabanları/ | Hayır | Hayır |
> | ManagedInstances/veritabanları/ek | Hayır | Hayır |
> | ManagedInstances/encryptionProtector | Hayır | Hayır |
> | ManagedInstances/anahtarlar | Hayır | Hayır |
> | ManagedInstances/Restokbledroppeddatabases/backupShortTermRetentionPolicies | Hayır | Hayır |
> | ManagedInstances/ | Hayır | Hayır |
> | sunucu | Evet | Evet |
> | sunucular/Yöneticiler | Hayır | Hayır |
> | sunucular/communicationLinks | Hayır | Hayır |
> | sunucular/veritabanları | Evet ( [aşağıdaki nota](#sqlnote)bakın) | Evet |
> | sunucular/encryptionProtector | Hayır | Hayır |
> | sunucular/firewallRules | Hayır | Hayır |
> | sunucular/anahtarlar | Hayır | Hayır |
> | sunucular/Restokbledroppeddatabases | Hayır | Hayır |
> | Sunucu/hizmet hedefleri | Hayır | Hayır |
> | sunucular/tdeCertificates | Hayır | Hayır |

<a id="sqlnote" />

> [!NOTE]
> Ana veritabanı etiketleri desteklemez, ancak Azure SQL veri ambarı veritabanları da dahil olmak üzere diğer veritabanları, Etiketler ' i destekler. Azure SQL veri ambarı veritabanlarının etkin (duraklatılmış değil) durumda olması gerekir.

## <a name="microsoftsqlvirtualmachine"></a>Microsoft. SqlVirtualMachine

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | SqlVirtualMachineGroups | Evet |Evet |
> | SqlVirtualMachineGroups/AvailabilityGroupListeners | Hayır |Hayır |
> | SqlVirtualMachines | Evet |Evet |

## <a name="microsoftstorage"></a>Microsoft.Storage

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | storageAccounts | Evet |Evet |
> | storageAccounts/blobServices | Hayır |Hayır |
> | storageAccounts/fileServices | Hayır |Hayır |
> | storageAccounts/queueServices | Hayır |Hayır |
> | storageAccounts/Services | Hayır |Hayır |
> | storageAccounts/Services/metricDefinitions | Hayır |Hayır |
> | storageAccounts/tableServices | Hayır |Hayır |
> | vardır | Hayır |Hayır |

## <a name="microsoftstoragecache"></a>Microsoft. StorageCache

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | önbelleklerinde | Evet |Evet |
> | önbellekler/Storagetaral | Hayır |Hayır |
> | Usagemodeller | Hayır |Hayır |

## <a name="microsoftstoragereplication"></a>Microsoft. Storagerepce

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | replicationGroups | Hayır |Hayır |

## <a name="microsoftstoragesync"></a>Microsoft.StorageSync

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | storageSyncServices | Evet |Evet |
> | storageSyncServices/registeredServers | Hayır |Hayır |
> | storageSyncServices/syncGroups | Hayır |Hayır |
> | storageSyncServices/syncGroups/cloudEndpoints | Hayır |Hayır |
> | storageSyncServices/syncGroups/serverEndpoints | Hayır |Hayır |
> | storageSyncServices/iş akışları | Hayır |Hayır |

## <a name="microsoftstoragesyncdev"></a>Microsoft. StorageSyncDev

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | storageSyncServices | Evet |Evet |
> | storageSyncServices/registeredServers | Hayır |Hayır |
> | storageSyncServices/syncGroups | Hayır |Hayır |
> | storageSyncServices/syncGroups/cloudEndpoints | Hayır |Hayır |
> | storageSyncServices/syncGroups/serverEndpoints | Hayır |Hayır |
> | storageSyncServices/iş akışları | Hayır |Hayır |

## <a name="microsoftstoragesyncint"></a>Microsoft. Storagesyncınt

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | storageSyncServices | Evet |Evet |
> | storageSyncServices/registeredServers | Hayır |Hayır |
> | storageSyncServices/syncGroups | Hayır |Hayır |
> | storageSyncServices/syncGroups/cloudEndpoints | Hayır |Hayır |
> | storageSyncServices/syncGroups/serverEndpoints | Hayır |Hayır |
> | storageSyncServices/iş akışları | Hayır |Hayır |

## <a name="microsoftstorsimple"></a>Microsoft.StorSimple

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | ilerinde | Evet |Evet |

## <a name="microsoftstreamanalytics"></a>Microsoft.StreamAnalytics

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | streammingjobs | Evet (aşağıdaki nota bakın) |Evet |

> [!NOTE]
> Streamingjobs çalışırken bir etiket ekleyemezsiniz. Etiketi eklemek için kaynağı durdurun.

## <a name="microsoftsubscription"></a>Microsoft. Subscription

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | iptal | Hayır |Hayır |
> | CreateSubscription | Hayır |Hayır |
> | yeniden adlandır | Hayır |Hayır |
> | SubscriptionDefinitions | Hayır |Hayır |
> | SubscriptionOperations | Hayır |Hayır |

## <a name="microsofttimeseriesinsights"></a>Microsoft.TimeSeriesInsights

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | lý | Evet |Hayır |
> | ortamlar/accessPolicies | Hayır |Hayır |
> | ortamlar/EventSources | Evet |Hayır |
> | ortamlar/Referencedataset 'ler | Evet |Hayır |

## <a name="microsoftvmwarecloudsimple"></a>Microsoft. Vmwarechoparlör basit

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | ayrılmış Cloudnodes | Evet |Evet |
> | ayrılmış CloudService | Evet |Evet |
> | virtualMachines | Evet |Evet |

## <a name="microsoftweb"></a>Microsoft. Web

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | apiManagementAccounts | Hayır |Hayır |
> | apiManagementAccounts/Apiacl 'Ler | Hayır |Hayır |
> | apiManagementAccounts/API 'ler | Hayır |Hayır |
> | apiManagementAccounts/API/Apiacl 'Ler | Hayır |Hayır |
> | apiManagementAccounts/API/Connectionacl 'Ler | Hayır |Hayır |
> | apiManagementAccounts/API/bağlantı | Hayır |Hayır |
> | apiManagementAccounts/API/Connections/Connectionacl 'Ler | Hayır |Hayır |
> | apiManagementAccounts/API/localizedDefinitions | Hayır |Hayır |
> | apiManagementAccounts/Connectionacl 'Ler | Hayır |Hayır |
> | apiManagementAccounts/bağlantılar | Hayır |Hayır |
> | billingMeters | Hayır |Hayır |
> | sertifika | Evet |Evet |
> | Connectiongateway 'ler | Evet |Evet |
> | bağlantılar | Evet |Evet |
> | Customapsıs | Evet |Evet |
> | Silinmi siteleri | Hayır |Hayır |
> | işlevleri | Hayır |Hayır |
> | hostingEnvironments | Evet |Evet |
> | hostingEnvironments/multiRolePools | Hayır |Hayır |
> | hostingEnvironments/workerPools | Hayır |Hayır |
> | publishingUsers | Hayır |Hayır |
> | Önerileri | Hayır |Hayır |
> | resourceHealthMetadata | Hayır |Hayır |
> | zamanları | Hayır |Hayır |
> | serverFarms | Evet |Evet |
> | Sunucugrupları/eventGridFilters | Hayır |Hayır |
> | siteler | Evet |Evet |
> | siteler/yapılandırma  | Hayır | Hayır |
> | siteler/eventGridFilters | Hayır |Hayır |
> | siteler/hostNameBindings | Hayır |Hayır |
> | Sites/networkConfig | Hayır |Hayır |
> | siteler/premieraddons | Evet |Evet |
> | siteler/yuvalar | Evet |Evet |
> | siteler/yuvalar/eventGridFilters | Hayır |Hayır |
> | siteler/yuvalar/hostNameBindings | Hayır |Hayır |
> | siteler/yuvalar/networkConfig | Hayır |Hayır |
> | sourceControls | Hayır |Hayır |
> | doğrulamalısınız | Hayır |Hayır |
> | verifyHostingEnvironmentVnet | Hayır |Hayır |

## <a name="microsoftwindowsdefenderatp"></a>Microsoft. Windowssavunma Deratp

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | diagnosticSettings | Hayır |Hayır |
> | diagnosticSettingsCategories | Hayır |Hayır |

## <a name="microsoftwindowsiot"></a>Microsoft. Windowsıot

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | DeviceServices | Evet |Evet |

## <a name="microsoftworkloadmonitor"></a>Microsoft. WorkloadMonitor

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | bileşenleri | Hayır |Hayır |
> | componentsSummary | Hayır |Hayır |
> | Izleme örnekleri | Hayır |Hayır |
> | Izleme ınstancessummary | Hayır |Hayır |
> | monitörün | Hayır |Hayır |
> | notificationSettings | Hayır |Hayır |

## <a name="next-steps"></a>Sonraki adımlar

Kaynaklara etiketlerin nasıl uygulanacağını öğrenmek için bkz. [Azure kaynaklarınızı düzenlemek için etiketleri kullanma](resource-group-using-tags.md).
