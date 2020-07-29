---
title: Kaynaklar için etiket desteği
description: Hangi Azure kaynak türlerinin etiketleri desteklediğini gösterir. Tüm Azure hizmetleri için ayrıntılar sağlar.
ms.topic: conceptual
ms.date: 07/28/2020
ms.openlocfilehash: 2ebf55a15233c401904460d459a118fc46142ac3
ms.sourcegitcommit: 46f8457ccb224eb000799ec81ed5b3ea93a6f06f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87338002"
---
# <a name="tag-support-for-azure-resources"></a>Azure kaynakları için etiket desteği
Bu makalede, bir kaynak türünün [etiketleri](tag-resources.md)destekleyip desteklemediğini açıklanmaktadır. Etiketi **destekleyen** sütun, kaynak türünün etiket için bir özelliğe sahip olup olmadığını gösterir. **Maliyet raporundaki etiket** etiketli sütun, kaynak türünün etiketi maliyet raporuna geçirip geçirmediğini belirtir. [Maliyet yönetimi maliyet analizi](../../cost-management-billing/costs/group-filter.md) ve [Azure Faturalandırma faturasında ve günlük kullanım verilerinde](../../cost-management-billing/manage/download-azure-invoice-daily-usage-date.md)maliyeti etiketlere göre görüntüleyebilirsiniz.

Aynı verileri bir virgülle ayrılmış değerler dosyası ile almak için [tag-support.csv](https://github.com/tfitzmac/resource-capabilities/blob/master/tag-support.csv)indirin.

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
> - [Microsoft. Insights](#microsoftinsights)
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
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | DomainServices | Yes | Yes |
> | DomainServices/oucontainer | Hayır | Hayır |

## <a name="microsoftaddons"></a>Microsoft. addons

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | Destek sağlayıcıları | Hayır | Hayır |

## <a name="microsoftadhybridhealthservice"></a>Microsoft. ADHybridHealthService

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | aadsupportcases | Hayır | Hayır |
> | addsservices | Hayır | Hayır |
> | aracısını | Hayır | Hayır |
> | anonymousapiusers | Hayır | Hayır |
> | yapılandırma | Hayır | Hayır |
> | günlükler | Hayır | Hayır |
> | reports | Hayır | Hayır |
> | servicehealthölçümleri | Hayır | Hayır |
> | services | Hayır | Hayır |

## <a name="microsoftadvisor"></a>Microsoft. Advisor

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | konfigürasyonları | Hayır | Hayır |
> | Generatereyorumgeçişleri | Hayır | Hayır |
> | meta veriler | Hayır | Hayır |
> | Öneriler | Hayır | Hayır |
> | gizlemeleri | Hayır | Hayır |

## <a name="microsoftalertsmanagement"></a>Microsoft. AlertsManagement

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | actionRules | Yes | Yes |
> | alerts | Hayır | Hayır |
> | alertsList | Hayır | Hayır |
> | alertsMetaData | Hayır | Hayır |
> | alertsSummary | Hayır | Hayır |
> | alertsSummaryList | Hayır | Hayır |
> | smartDetectorAlertRules | Yes | Yes |
> | smartGroups | Hayır | Hayır |

## <a name="microsoftanalysisservices"></a>Microsoft. AnalysisServices

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | larý | Yes | Yes |

## <a name="microsoftapimanagement"></a>Microsoft. Apimanane

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | reportFeedback | Hayır | Hayır |
> | hizmet | Yes | Yes |
> | validateServiceName | Hayır | Hayır |

## <a name="microsoftappconfiguration"></a>Microsoft. AppConfiguration

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | Configurationmağazaların | Yes | Yes |
> | Configurationmağazaların/eventGridFilters | Hayır | Hayır |

## <a name="microsoftappplatform"></a>Microsoft. AppPlatform

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | Spring | Yes | Yes |
> | Yay/uygulamalar | Hayır | Hayır |
> | Yay/uygulamalar/dağıtımlar | Hayır | Hayır |

## <a name="microsoftattestation"></a>Microsoft. kanıtlama

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | attestationProviders | Yes | Yes |
> | defaultProviders | Hayır | Hayır |

## <a name="microsoftauthorization"></a>Microsoft.Authorization

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | classicAdministrators | Hayır | Hayır |
> | Datatakma adlar | Hayır | Hayır |
> | Denyasatamaları | Hayır | Hayır |
> | Erişimi yükseltme | Hayır | Hayır |
> | Findorphanroleatamalar | Hayır | Hayır |
> | kaynaktaki | Hayır | Hayır |
> | izinler | Hayır | Hayır |
> | Poliyasatamaları | Hayır | Hayır |
> | policyDefinitions | Hayır | Hayır |
> | policySetDefinitions | Hayır | Hayır |
> | privateLinkAssociations | Hayır | Hayır |
> | providerOperations | Hayır | Hayır |
> | resourceManagementPrivateLinks | Hayır | Hayır |
> | roleAssignments | Hayır | Hayır |
> | Roleatamasussusageölçümleri | Hayır | Hayır |
> | roleDefinitions | Hayır | Hayır |

## <a name="microsoftautomation"></a>Microsoft. Automation

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | automationAccounts | Yes | Yes |
> | automationAccounts/Configurations | Yes | Yes |
> | automationAccounts/Jobs | Hayır | Hayır |
> | automationAccounts/Privateendpointconnectionproxy 'Leri | Hayır | Hayır |
> | automationAccounts/privateEndpointConnections | Hayır | Hayır |
> | automationAccounts/privateLinkResources | Hayır | Hayır |
> | automationAccounts/runbook 'lar | Yes | Yes |
> | automationAccounts/softwareUpdateConfigurations | Hayır | Hayır |
> | automationAccounts/Web kancaları | Hayır | Hayır |

>[!NOTE]
>Azure Otomasyonu yalnızca her bir Otomasyon kaynağı için en fazla 15 etiket adı/değer çifti oluşturmayı destekler.
> 

## <a name="microsoftavs"></a>Microsoft. AVS

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | Privatebulutlar | Yes | Yes |
> | Privatebulutlar/yetkilendirmeler | Hayır | Hayır |
> | Privatebulutlar/kümeler | Hayır | Hayır |
> | Privatebulutlar/hcxEnterpriseSites | Hayır | Hayır |

## <a name="microsoftazuregeneva"></a>Microsoft. Azure. Genfiliz

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | lý | Hayır | Hayır |
> | ortamlar/hesaplar | Hayır | Hayır |
> | ortamlar/hesaplar/ad alanları | Hayır | Hayır |
> | ortamlar/hesaplar/ad alanları/yapılandırma | Hayır | Hayır |

## <a name="microsoftazureactivedirectory"></a>Microsoft. AzureActiveDirectory

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | b2cDirectories | Evet | Hayır |
> | b2ctenants | Hayır | Hayır |

## <a name="microsoftazuredata"></a>Microsoft. AzureData

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | Veri denetleyicileri | Yes | Yes |
> | Hybriddatayöneticileri | Yes | Yes |
> | Postgresınstances | Yes | Yes |
> | SQLInstances | Yes | Yes |
> | Sqlmanagedınstances | Yes | Yes |
> | Sqlserverınstances | Yes | Yes |
> | Sqlserverkayıtları | Yes | Yes |
> | Sqlserverkayıtları/sqlServers | Hayır | Hayır |

## <a name="microsoftazurestack"></a>Microsoft. AzureStack

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | cloudManifestFiles | Hayır | Hayır |
> | edgeSubscriptions | Yes | Yes |
> | kayıtlarında | Yes | Yes |
> | kayıt/müşteri abonelikleri | Hayır | Hayır |
> | kayıtlar/ürünler | Hayır | Hayır |

## <a name="microsoftazurestackhci"></a>Microsoft. Azurestackhcı

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | leriniz | Yes | Yes |

## <a name="microsoftbatch"></a>Microsoft.Batch

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | batchAccounts | Yes | Yes |

## <a name="microsoftbilling"></a>Microsoft. Faturalandırma

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | billingAccounts | Hayır | Hayır |
> | billingAccounts/anlaşmalar | Hayır | Hayır |
> | billingAccounts/billingPermissions | Hayır | Hayır |
> | billingAccounts/billingProfiles | Hayır | Hayır |
> | billingAccounts/Billingprofiller/billingPermissions | Hayır | Hayır |
> | billingAccounts/billingProfiles/Billingroleatamaları | Hayır | Hayır |
> | billingAccounts/billingProfiles/billingRoleDefinitions | Hayır | Hayır |
> | billingAccounts/billingProfiles/Billingabonelikleri | Hayır | Hayır |
> | billingAccounts/billingProfiles/Createbillingroleatama | Hayır | Hayır |
> | billingAccounts/billingProfiles/müşteriler | Hayır | Hayır |
> | billingAccounts/billingProfiles/yönergeler | Hayır | Hayır |
> | billingAccounts/billingProfiles/faturalar | Hayır | Hayır |
> | billingAccounts/billingProfiles/faturalar/fiyat listesi | Hayır | Hayır |
> | billingAccounts/billingProfiles/faturalar/işlemler | Hayır | Hayır |
> | billingAccounts/billingProfiles/ınvoicesections | Hayır | Hayır |
> | billingAccounts/billingProfiles/ınvoicesections/billingPermissions | Hayır | Hayır |
> | billingAccounts/billingProfiles/ınvoicesections/Billingroleatamaları | Hayır | Hayır |
> | billingAccounts/billingProfiles/ınvoicesections/billingRoleDefinitions | Hayır | Hayır |
> | billingAccounts/billingProfiles/ınvoicesections/Billingabonelikleri | Hayır | Hayır |
> | billingAccounts/billingProfiles/ınvoicesections/Createbillingroleatama | Hayır | Hayır |
> | billingAccounts/billingProfiles/ınvoicesections/ınitiatetransfer | Hayır | Hayır |
> | billingAccounts/billingProfiles/ınvoicesections/ürünler | Hayır | Hayır |
> | billingAccounts/billingProfiles/ınvoicesections/ürünler/aktarım | Hayır | Hayır |
> | billingAccounts/billingProfiles/ınvoicesections/ürünler/updateAutoRenew | Hayır | Hayır |
> | billingAccounts/billingProfiles/ınvoicesections/işlemler | Hayır | Hayır |
> | billingAccounts/billingProfiles/ınvoicesections/aktarımlar | Hayır | Hayır |
> | billingAccounts/BillingProfiles/patchOperations | Hayır | Hayır |
> | billingAccounts/billingProfiles/paymentMethods | Hayır | Hayır |
> | billingAccounts/billingProfiles/ilkeler | Hayır | Hayır |
> | billingAccounts/billingProfiles/fiyat listesi | Hayır | Hayır |
> | billingAccounts/billingProfiles/pricesheetDownloadOperations | Hayır | Hayır |
> | billingAccounts/billingProfiles/ürünler | Hayır | Hayır |
> | billingAccounts/billingProfiles/işlemler | Hayır | Hayır |
> | billingAccounts/Billingroleatamaları | Hayır | Hayır |
> | billingAccounts/billingRoleDefinitions | Hayır | Hayır |
> | billingAccounts/Billingabonelikleri | Hayır | Hayır |
> | billingAccounts/Billingabonelikleri/faturalar | Hayır | Hayır |
> | billingAccounts/Createbillingroleatama | Hayır | Hayır |
> | billingAccounts/Createınvoicesectionoperations | Hayır | Hayır |
> | billingAccounts/müşteriler | Hayır | Hayır |
> | billingAccounts/müşteriler/billingPermissions | Hayır | Hayır |
> | billingAccounts/müşteriler/Billingabonelikleri | Hayır | Hayır |
> | billingAccounts/müşteriler/ınitiatetransfer | Hayır | Hayır |
> | billingAccounts/müşteriler/ilkeler | Hayır | Hayır |
> | billingAccounts/müşteriler/ürünler | Hayır | Hayır |
> | billingAccounts/müşteriler/işlemler | Hayır | Hayır |
> | billingAccounts/müşteriler/aktarımlar | Hayır | Hayır |
> | billingAccounts/departmanlar | Hayır | Hayır |
> | billingAccounts/departmanlar/billingPermissions | Hayır | Hayır |
> | billingAccounts/departmanlar/Billingroleatamaları | Hayır | Hayır |
> | billingAccounts/departmanlar/billingRoleDefinitions | Hayır | Hayır |
> | billingAccounts/KayıtSayısı | Hayır | Hayır |
> | billingAccounts/Kayıthesapsayısı/billingPermissions | Hayır | Hayır |
> | billingAccounts/KayıtSayısı/Billingroleatamaları | Hayır | Hayır |
> | billingAccounts/KayıtSayısı/billingRoleDefinitions | Hayır | Hayır |
> | billingAccounts/faturalar | Hayır | Hayır |
> | billingAccounts/faturalar/işlemler | Hayır | Hayır |
> | billingAccounts/ınvoicesections | Hayır | Hayır |
> | billingAccounts/ınvoicesections/billingSubscriptionMoveOperations | Hayır | Hayır |
> | billingAccounts/ınvoicesections/Billingabonelikleri | Hayır | Hayır |
> | billingAccounts/ınvoicesections/Billingabonelikleri/aktarımı | Hayır | Hayır |
> | billingAccounts/ınvoicesections/yükselt | Hayır | Hayır |
> | billingAccounts/ınvoicesections/ınitiatetransfer | Hayır | Hayır |
> | billingAccounts/ınvoicesections/patchOperations | Hayır | Hayır |
> | billingAccounts/ınvoicesections/productMoveOperations | Hayır | Hayır |
> | billingAccounts/Ürünler/Ürünler | Hayır | Hayır |
> | billingAccounts/ınvoicesections/ürünler/transfer | Hayır | Hayır |
> | billingAccounts/ınvoicesections/ürünler/updateAutoRenew | Hayır | Hayır |
> | billingAccounts/ınvoicesections/işlemler | Hayır | Hayır |
> | billingAccounts/ınvoicesections/aktarımlar | Hayır | Hayır |
> | billingAccounts/Lineofkredisi | Hayır | Hayır |
> | billingAccounts/patchOperations | Hayır | Hayır |
> | billingAccounts/paymentMethods | Hayır | Hayır |
> | billingAccounts/ürünler | Hayır | Hayır |
> | billingAccounts/işlemler | Hayır | Hayır |
> | Billingdönemler | Hayır | Hayır |
> | billingPermissions | Hayır | Hayır |
> | billingProperty | Hayır | Hayır |
> | Billingroleatamaları | Hayır | Hayır |
> | billingRoleDefinitions | Hayır | Hayır |
> | Createbillingroleatama | Hayır | Hayır |
> | bölümlerinin | Hayır | Hayır |
> | kayıt sayısı | Hayır | Hayır |
> | faturalardan | Hayır | Hayır |
> | girişinde | Hayır | Hayır |
> | aktarımlar/acceptTransfer | Hayır | Hayır |
> | aktarımlar/declineTransfer | Hayır | Hayır |
> | aktarımlar/operationStatus | Hayır | Hayır |
> | aktarımlar/validateTransfer | Hayır | Hayır |
> | validateAddress | Hayır | Hayır |

## <a name="microsoftbingmaps"></a>Microsoft. BingMaps

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | Mapapsıs | Yes | Yes |
> | updateCommunicationPreference | Hayır | Hayır |

## <a name="microsoftblockchain"></a>Microsoft. Blockzinciri

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | blockchainMembers | Yes | Yes |
> | Cordadmembers | Yes | Yes |
> | izleyicileri | Yes | Yes |

## <a name="microsoftblockchaintokens"></a>Microsoft. BlockchainTokens

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | TokenServices | Yes | Yes |
> | TokenServices/BlockchainNetworks | Hayır | Hayır |
> | TokenServices/Groups | Hayır | Hayır |
> | TokenServices/gruplar/hesaplar | Hayır | Hayır |
> | TokenServices/TokenTemplates | Hayır | Hayır |

## <a name="microsoftblueprint"></a>Microsoft. Blueprint

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | Şema tasmi | Hayır | Hayır |
> | Blueprintasbir/Atamaperations | Hayır | Hayır |
> | Blueprintasbir/işlemleri | Hayır | Hayır |
> | Blueprint | Hayır | Hayır |
> | planlar/yapıtlar | Hayır | Hayır |
> | planlar/sürümler | Hayır | Hayır |
> | planlar/sürümler/yapılar | Hayır | Hayır |

## <a name="microsoftbotservice"></a>Microsoft. BotService

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | botServices | Yes | Yes |
> | botServices/kanallar | Hayır | Hayır |
> | botServices/Connections | Hayır | Hayır |
> | diller | Hayır | Hayır |
> | templates | Hayır | Hayır |

## <a name="microsoftcache"></a>Microsoft. Cache

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | Redis | Yes | Yes |
> | Redsıs/EventGridFilters | Hayır | Hayır |
> | Redsıs/Privateendpointconnectionproxy 'Leri | Hayır | Hayır |
> | Redsıs/Privateendpointconnectionproxy/doğrulama | Hayır | Hayır |
> | Redsıs/privateEndpointConnections | Hayır | Hayır |
> | Redsıs/privateLinkResources | Hayır | Hayır |
> | redisEnterprise | Yes | Yes |

## <a name="microsoftcapacity"></a>Microsoft. Capacity

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | appliedReservations | Hayır | Hayır |
> | Oto Kotaartışı | Hayır | Hayır |
> | calculateExchange | Hayır | Hayır |
> | calculatePrice | Hayır | Hayır |
> | calculatePurchasePrice | Hayır | Hayır |
> | larına | Hayır | Hayır |
> | Ticari Vaalrezervler | Hayır | Hayır |
> | değişimi | Hayır | Hayır |
> | placePurchaseOrder | Hayır | Hayır |
> | Rezervler | Hayır | Hayır |
> | Rezervler/Hesaplaizterefund | Hayır | Hayır |
> | Rezervler/Birleştir | Hayır | Hayır |
> | Rezervler/rezervasyonlar | Hayır | Hayır |
> | Rezervler/rezervasyonlar/düzeltmeler | Hayır | Hayır |
> | Rezervler/geri dönüş | Hayır | Hayır |
> | Rezervler/Böl | Hayır | Hayır |
> | Rezervler/takas | Hayır | Hayır |
> | oluşturamaz | Hayır | Hayır |
> | resourceProviders | Hayır | Hayır |
> | kaynaklar | Hayır | Hayır |
> | validateReservationOrder | Hayır | Hayır |

## <a name="microsoftcdn"></a>Microsoft. CDN

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | CdnWebApplicationFirewallManagedRuleSets | Hayır | Hayır |
> | CdnWebApplicationFirewallPolicies | Yes | Yes |
> | edgenodes | Hayır | Hayır |
> | lerinize | Yes | Yes |
> | Profiller/uç noktalar | Yes | Yes |
> | Profiller/uç noktalar/customdomains | Hayır | Hayır |
> | Profiller/uç noktalar/origingroups | Hayır | Hayır |
> | Profiller/uç noktalar/kaynaklar | Hayır | Hayır |
> | Validatearaştırması | Hayır | Hayır |

## <a name="microsoftcertificateregistration"></a>Microsoft. CertificateRegistration

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | Sertifikadüzenleri | Yes | Yes |
> | certificateOrders/Certificates | Hayır | Hayır |
> | Validatecertificateregistrationınformation | Hayır | Hayır |

## <a name="microsoftchangeanalysis"></a>Microsoft. ChangeAnalysis

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | profil | Hayır | Hayır |
> | resourceChanges | Hayır | Hayır |

## <a name="microsoftclassiccompute"></a>Microsoft.ClassicCompute

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | özellikler | Hayır | Hayır |
> | domainNames | Hayır | Hayır |
> | domainNames/yetenekleri | Hayır | Hayır |
> | domainNames/internalLoadBalancers | Hayır | Hayır |
> | domainNames/serviceCertificates | Hayır | Hayır |
> | domainNames/Yuvaları | Hayır | Hayır |
> | domainNames/yuvalar/roller | Hayır | Hayır |
> | domainNames/yuvalar/roller/metricDefinitions | Hayır | Hayır |
> | domainNames/yuvalar/roller/ölçümler | Hayır | Hayır |
> | moveSubscriptionResources | Hayır | Hayır |
> | operatingSystemFamilies | Hayır | Hayır |
> | operatingSystems | Hayır | Hayır |
> | quotas | Hayır | Hayır |
> | resourceTypes | Hayır | Hayır |
> | Validatesubscriptionmoveavaılabılıty | Hayır | Hayır |
> | virtualMachines | Hayır | Hayır |
> | virtualMachines/diagnosticSettings | Hayır | Hayır |
> | virtualMachines/metricDefinitions | Hayır | Hayır |
> | virtualMachines/ölçümler | Hayır | Hayır |

## <a name="microsoftclassicinfrastructuremigrate"></a>Microsoft. ClassicInfrastructureMigrate

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | classicInfrastructureResources | Hayır | Hayır |

## <a name="microsoftclassicnetwork"></a>Microsoft. ClassicNetwork

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | özellikler | Hayır | Hayır |
> | expressRouteCrossConnections | Hayır | Hayır |
> | expressRouteCrossConnections/peerler | Hayır | Hayır |
> | gatewaySupportedDevices | Hayır | Hayır |
> | networkSecurityGroups | Hayır | Hayır |
> | quotas | Hayır | Hayır |
> | Rezervler | Hayır | Hayır |
> | virtualNetworks | Hayır | Hayır |
> | virtualNetworks/Remotevirtualnetworkpeeringproxy 'Leri | Hayır | Hayır |
> | virtualNetworks/Virtualnetworkpeerler | Hayır | Hayır |

## <a name="microsoftclassicstorage"></a>Microsoft. ClassicStorage

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | özellikler | Hayır | Hayır |
> | disklerinden | Hayır | Hayır |
> | images | Hayır | Hayır |
> | osImages | Hayır | Hayır |
> | Osplatformımages | Hayır | Hayır |
> | Publicımages | Hayır | Hayır |
> | quotas | Hayır | Hayır |
> | storageAccounts | Hayır | Hayır |
> | storageAccounts/blobServices | Hayır | Hayır |
> | storageAccounts/fileServices | Hayır | Hayır |
> | storageAccounts/metricDefinitions | Hayır | Hayır |
> | storageAccounts/ölçümler | Hayır | Hayır |
> | storageAccounts/queueServices | Hayır | Hayır |
> | storageAccounts/Services | Hayır | Hayır |
> | storageAccounts/Services/diagnosticSettings | Hayır | Hayır |
> | storageAccounts/Services/metricDefinitions | Hayır | Hayır |
> | storageAccounts/Services/ölçümler | Hayır | Hayır |
> | storageAccounts/tableServices | Hayır | Hayır |
> | storageAccounts/Vmımages | Hayır | Hayır |
> | Vmımages | Hayır | Hayır |

## <a name="microsoftcognitiveservices"></a>Microsoft. Biliveservices

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | accounts | Yes | Yes |

## <a name="microsoftcommerce"></a>Microsoft. Commerce

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | RateCard | Hayır | Hayır |
> | Usagetoplamaları | Hayır | Hayır |

## <a name="microsoftcompute"></a>Microsoft.Compute

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | availabilitySets | Yes | Yes |
> | Diskeriþler | Yes | Yes |
> | diskEncryptionSets | Yes | Yes |
> | disklerinden | Yes | Yes |
> | Galeriler | Yes | Yes |
> | Galeriler/uygulamalar | Hayır | Hayır |
> | Galeriler/uygulamalar/sürümler | Hayır | Hayır |
> | Galeriler/görüntüler | Hayır | Hayır |
> | Galeriler/resimler/sürümler | Hayır | Hayır |
> | hostGroups | Yes | Yes |
> | hostGroups/konaklar | Yes | Yes |
> | images | Yes | Yes |
> | proximityPlacementGroups | Yes | Yes |
> | restorePointCollections | Yes | Yes |
> | restorePointCollections/restorePoints | Hayır | Hayır |
> | sharedVMExtensions | Yes | Yes |
> | sharedVMExtensions/sürümler | Hayır | Hayır |
> | Sharedvmımages | Yes | Yes |
> | Sharedvmımages/sürümler | Hayır | Hayır |
> | anlık görüntüler | Yes | Yes |
> | sshPublicKeys | Yes | Yes |
> | virtualMachines | Yes | Yes |
> | virtualMachines/uzantıları | Yes | Yes |
> | virtualMachines/metricDefinitions | Hayır | Hayır |
> | virtualMachines/runCommands | Yes | Yes |
> | virtualMachineScaleSets | Yes | Yes |
> | virtualMachineScaleSets/uzantılar | Hayır | Hayır |
> | virtualMachineScaleSets/NetworkInterfaces | Hayır | Hayır |
> | virtualMachineScaleSets/Publicıpaddresses | Hayır | Hayır |
> | virtualMachineScaleSets/virtualMachines | Hayır | Hayır |
> | virtualMachineScaleSets/virtualMachines/NetworkInterfaces | Hayır | Hayır |

> [!NOTE]
> Genelleştirilmiş olarak işaretlenmiş bir sanal makineye etiket ekleyemezsiniz. [Set-AzVm-Genelleştirilmiş](/powershell/module/Az.Compute/Set-AzVM) veya [az VM generalize](/cli/azure/vm#az-vm-generalize)ile bir sanal makineyi genelleştirilemez.

## <a name="microsoftconnectedcache"></a>Microsoft. ConnectedCache

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | Önbellekenodes | Yes | Yes |

## <a name="microsoftconsumption"></a>Microsoft. tüketim

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | Aggregmalyt maliyeti | Hayır | Hayır |
> | Bakiyeler | Hayır | Hayır |
> | Bütçeler | Hayır | Hayır |
> | Ücretler | Hayır | Hayır |
> | CostTags | Hayır | Hayır |
> | iler | Hayır | Hayır |
> | etkinlikler | Hayır | Hayır |
> | Tahminler | Hayır | Hayır |
> | oluş | Hayır | Hayır |
> | Marketlerinden | Hayır | Hayır |
> | Fiyat listeleri | Hayır | Hayır |
> | ürün | Hayır | Hayır |
> | Rezervde ayrıntıları | Hayır | Hayır |
> | ReservationRecommendationDetails | Hayır | Hayır |
> | Rezervationönerilere | Hayır | Hayır |
> | Rezervlerin Özeti | Hayır | Hayır |
> | Rezervlik Işlemleri | Hayır | Hayır |
> | Etiketler | Hayır | Hayır |
> | Kira | Hayır | Hayır |
> | Terimler | Hayır | Hayır |
> | UsageDetails | Hayır | Hayır |

## <a name="microsoftcontainerinstance"></a>Microsoft. Containerınstance

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | Kapsayıcı grupları | Yes | Yes |
> | serviceAssociationLinks | Hayır | Hayır |

## <a name="microsoftcontainerregistry"></a>Microsoft. ContainerRegistry

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | kayıt | Yes | Yes |
> | kayıt defterleri/agentPools | Yes | Yes |
> | kayıt defterleri/derlemeler | Hayır | Hayır |
> | kayıt defterleri/derlemeler/iptal | Hayır | Hayır |
> | kayıt defterleri/derlemeler/getLogLink | Hayır | Hayır |
> | kayıt defterleri/buildTasks | Yes | Yes |
> | kayıt defterleri/buildTasks/Steps | Hayır | Hayır |
> | kayıt defterleri/eventGridFilters | Hayır | Hayır |
> | kayıt defterleri/Exporthatlarının | Hayır | Hayır |
> | kayıt defterleri/generateCredentials | Hayır | Hayır |
> | kayıt defterleri/getBuildSourceUploadUrl 'Si | Hayır | Hayır |
> | kayıt defterleri/GetCredentials | Hayır | Hayır |
> | kayıt defterleri/ımportımage | Hayır | Hayır |
> | kayıt defterleri/ımporthatlarının | Hayır | Hayır |
> | kayıt defterleri/ardışık düzen eylemsizlik | Hayır | Hayır |
> | kayıt defterleri/Privateendpointconnectionproxy 'Leri | Hayır | Hayır |
> | kayıt defterleri/Privateendpointconnectionproxy/doğrulama | Hayır | Hayır |
> | kayıt defterleri/privateEndpointConnections | Hayır | Hayır |
> | kayıt defterleri/privateLinkResources | Hayır | Hayır |
> | kayıt defterleri/queueBuild | Hayır | Hayır |
> | kayıt defterleri/regenerateCredential | Hayır | Hayır |
> | kayıt defterleri/regenerateCredentials | Hayır | Hayır |
> | kayıt defterleri/çoğaltmalar | Yes | Yes |
> | kayıt defterleri/çalıştırmalar | Hayır | Hayır |
> | kayıt defterleri/çalıştırmalar/iptal | Hayır | Hayır |
> | kayıt defterleri/scheduleRun | Hayır | Hayır |
> | kayıt defterleri/Kapsameşlemler | Hayır | Hayır |
> | kayıt defterleri/Taskçalıştırmaları | Hayır | Hayır |
> | kayıt defterleri/görevler | Yes | Yes |
> | kayıt defterleri/belirteçler | Hayır | Hayır |
> | kayıt defterleri/updatePolicies | Hayır | Hayır |
> | kayıt defterleri/Web kancaları | Yes | Yes |
> | kayıt defterleri/Web kancaları/getCallbackConfig | Hayır | Hayır |
> | kayıt defterleri/Web kancaları/ping | Hayır | Hayır |

## <a name="microsoftcontainerservice"></a>Microsoft. ContainerService

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | containerServices | Yes | Yes |
> | Managedkümeler | Yes | Yes |
> | openShiftManagedClusters | Yes | Yes |

## <a name="microsoftcostmanagement"></a>Microsoft. CostManagement

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | Uyarılar | Hayır | Hayır |
> | BillingAccounts | Hayır | Hayır |
> | Bütçeler | Hayır | Hayır |
> | Cloudbağlayıcıları | Hayır | Hayır |
> | Bağlayıcılar | Yes | Yes |
> | costAllocationRules | Hayır | Hayır |
> | Departmanlar | Hayır | Hayır |
> | Boyutlar | Hayır | Hayır |
> | Kayıt sayısı | Hayır | Hayır |
> | Aktarımları | Hayır | Hayır |
> | ExternalBillingAccounts | Hayır | Hayır |
> | ExternalBillingAccounts/uyarılar | Hayır | Hayır |
> | ExternalBillingAccounts/Boyutlar | Hayır | Hayır |
> | ExternalBillingAccounts/tahmin | Hayır | Hayır |
> | ExternalBillingAccounts/sorgu | Hayır | Hayır |
> | Externalabonelikleri | Hayır | Hayır |
> | Externalabonelikleri/uyarıları | Hayır | Hayır |
> | Externalabonelikler/Boyutlar | Hayır | Hayır |
> | Externalabonelikler/tahmin | Hayır | Hayır |
> | Externalabonelikler/sorgu | Hayır | Hayır |
> | Tahmin | Hayır | Hayır |
> | Sorgu | Hayır | Hayır |
> | register | Hayır | Hayır |
> | Reportconfigs | Hayır | Hayır |
> | Raporlar | Hayır | Hayır |
> | Ayarlar | Hayır | Hayır |
> | showbackRules | Hayır | Hayır |
> | Görünümler | Hayır | Hayır |

## <a name="microsoftcustomerlockbox"></a>Microsoft. Customerkasası

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | istekleri | Hayır | Hayır |

## <a name="microsoftcustomproviders"></a>Microsoft. CustomProviders

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | içermektedir | Hayır | Hayır |
> | resourceProviders | Yes | Yes |

## <a name="microsoftdatabox"></a>Microsoft. DataBox

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | Çizelge | Yes | Yes |

## <a name="microsoftdataboxedge"></a>Microsoft. DataBoxEdge

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | DataBoxEdgeDevices | Yes | Yes |

## <a name="microsoftdatabricks"></a>Microsoft. Databricks

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | çalışma alanı | Evet | Hayır |
> | çalışma alanları/dbWorkspaces | Hayır | Hayır |
> | çalışma alanları/storageEncryption | Hayır | Hayır |
> | çalışma alanları/Virtualnetworkpeerler | Hayır | Hayır |

## <a name="microsoftdatacatalog"></a>Microsoft. DataCatalog

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | larına | Yes | Yes |
> | veri katalogları | Yes | Yes |
> | veri katalogları/veri kaynakları | Hayır | Hayır |
> | veri katalogları/veri kaynakları/taramalar | Hayır | Hayır |
> | veri katalogları/veri kaynakları/taramalar/veri kümeleri | Hayır | Hayır |
> | veri katalogları/veri kaynakları/taramalar/Tetikleyiciler | Hayır | Hayır |

## <a name="microsoftdatafactory"></a>Microsoft. DataFactory

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | Veri fabrikaları | Evet | Hayır |
> | DataFactory/diagnosticSettings | Hayır | Hayır |
> | DataFactory/metricDefinitions | Hayır | Hayır |
> | dataFactorySchema | Hayır | Hayır |
> | larının | Evet | Hayır |
> | Fabrika/tümleştirme çalışma zamanları | Hayır | Hayır |

## <a name="microsoftdatalakeanalytics"></a>Microsoft. DataLakeAnalytics

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | accounts | Yes | Yes |
> | hesaplar/dataLakeStoreAccounts | Hayır | Hayır |
> | hesaplar/storageAccounts | Hayır | Hayır |
> | hesaplar/storageAccounts/kapsayıcılar | Hayır | Hayır |
> | hesaplar/Transferanaliz tici | Hayır | Hayır |

## <a name="microsoftdatalakestore"></a>Microsoft. DataLakeStore

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | accounts | Yes | Yes |
> | hesaplar/eventGridFilters | Hayır | Hayır |
> | hesaplar/firewallRules | Hayır | Hayır |

## <a name="microsoftdatamigration"></a>Microsoft. DataMigration

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | services | Hayır | Hayır |
> | Hizmetler/Projeler | Hayır | Hayır |

## <a name="microsoftdataprotection"></a>Microsoft. DataProtection

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | Backupkasaları | Yes | Yes |
> | Resourceoperationgatedenetleyicileri | Yes | Yes |

## <a name="microsoftdatashare"></a>Microsoft. DataShare

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | accounts | Yes | Yes |
> | hesaplar/paylaşımlar | Hayır | Hayır |
> | hesaplar/paylaşımlar/veri kümeleri | Hayır | Hayır |
> | hesaplar/paylaşımlar/davetler | Hayır | Hayır |
> | hesaplar/paylaşımlar/providersharesubscriptions | Hayır | Hayır |
> | hesaplar/paylaşımlar/synchronizationSettings | Hayır | Hayır |
> | hesaplar/parçalar esubscriptions | Hayır | Hayır |
> | hesaplar/parçalar esubscriptions/Consumersourcedataset 'ler | Hayır | Hayır |
> | hesaplar/parçalar esubscriptions/datasetmappings | Hayır | Hayır |
> | hesaplar/parçalar esubscriptions/Tetikleyiciler | Hayır | Hayır |

## <a name="microsoftdbformariadb"></a>Microsoft. Dbformarıdb

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | larý | Yes | Yes |
> | sunucular/danışmanları | Hayır | Hayır |
> | sunucular/anahtarlar | Hayır | Hayır |
> | sunucular/Privateendpointconnectionproxy 'Leri | Hayır | Hayır |
> | sunucular/privateEndpointConnections | Hayır | Hayır |
> | sunucular/privateLinkResources | Hayır | Hayır |
> | sunucular/Querymetinmetinleri | Hayır | Hayır |
> | sunucular/recoverableServers | Hayır | Hayır |
> | sunucular/topQueryStatistics | Hayır | Hayır |
> | sunucular/virtualNetworkRules | Hayır | Hayır |
> | sunucular/waitStatistics | Hayır | Hayır |

## <a name="microsoftdbformysql"></a>Microsoft. Dbformyısql

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | larý | Yes | Yes |
> | sunucular/danışmanları | Hayır | Hayır |
> | sunucular/anahtarlar | Hayır | Hayır |
> | sunucular/Privateendpointconnectionproxy 'Leri | Hayır | Hayır |
> | sunucular/privateEndpointConnections | Hayır | Hayır |
> | sunucular/privateLinkResources | Hayır | Hayır |
> | sunucular/Querymetinmetinleri | Hayır | Hayır |
> | sunucular/recoverableServers | Hayır | Hayır |
> | sunucular/topQueryStatistics | Hayır | Hayır |
> | sunucular/virtualNetworkRules | Hayır | Hayır |
> | sunucular/waitStatistics | Hayır | Hayır |

## <a name="microsoftdbforpostgresql"></a>Microsoft. DBforPostgreSQL

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | Sunucu grupları | Yes | Yes |
> | larý | Yes | Yes |
> | sunucular/danışmanları | Hayır | Hayır |
> | sunucular/anahtarlar | Hayır | Hayır |
> | sunucular/Privateendpointconnectionproxy 'Leri | Hayır | Hayır |
> | sunucular/privateEndpointConnections | Hayır | Hayır |
> | sunucular/privateLinkResources | Hayır | Hayır |
> | sunucular/Querymetinmetinleri | Hayır | Hayır |
> | sunucular/recoverableServers | Hayır | Hayır |
> | sunucular/topQueryStatistics | Hayır | Hayır |
> | sunucular/virtualNetworkRules | Hayır | Hayır |
> | sunucular/waitStatistics | Hayır | Hayır |
> | serversv2 | Yes | Yes |
> | Teksunucular | Yes | Yes |

## <a name="microsoftdeploymentmanager"></a>Microsoft. DeploymentManager

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | artifactSources | Yes | Yes |
> | piyasaya çıkarma | Yes | Yes |
> | Servicetopolojileri | Yes | Yes |
> | Servicetopolojileri/hizmetler | Yes | Yes |
> | Servicetopolojileri/hizmetler/serviceUnits | Yes | Yes |
> | adımlar | Yes | Yes |

## <a name="microsoftdesktopvirtualization"></a>Microsoft. DesktopVirtualization

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | applicationgroups | Yes | Yes |
> | applicationgroups/uygulamalar | Hayır | Hayır |
> | applicationgroups/masaüstleri | Hayır | Hayır |
> | applicationgroups/startmenuıtems | Hayır | Hayır |
> | Ana bilgisayar havuzları | Yes | Yes |
> | hostpools/oturumkonakları | Hayır | Hayır |
> | hostpools/sessionkonakları/usersessions | Hayır | Hayır |
> | hosthavuzlar/usersessions | Hayır | Hayır |
> | çalışma alanı | Yes | Yes |

## <a name="microsoftdevices"></a>Microsoft.Devices

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | Elaun havuzları | Yes | Yes |
> | Elaun havuzları/ıothubkiracılar | Yes | Yes |
> | Elaor havuzları/ıothubkiracılar/securitySettings | Hayır | Hayır |
> | Iothubs | Yes | Yes |
> | IotHubs/eventGridFilters | Hayır | Hayır |
> | IotHubs/securitySettings | Hayır | Hayır |
> | ProvisioningServices | Yes | Yes |
> | vardır | Hayır | Hayır |

## <a name="microsoftdevops"></a>Microsoft. DevOps

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | düzenler | Yes | Yes |

## <a name="microsoftdevspaces"></a>Microsoft. DevSpaces

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | denetleyiciler | Yes | Yes |

## <a name="microsoftdevtestlab"></a>Microsoft. DevTestLab

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | labcenters | Yes | Yes |
> | larda | Yes | Yes |
> | Laboratuvarlar/ortamlar | Yes | Yes |
> | Labs/Servicerunanlar | Yes | Yes |
> | Labs/virtualMachines | Yes | Yes |
> | cağını | Yes | Yes |

## <a name="microsoftdigitaltwins"></a>Microsoft. DigitalTwins

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | Digitaltwınsınstances | Yes | Yes |
> | Digitaltwınsınstances/endpoints | Hayır | Hayır |

## <a name="microsoftdocumentdb"></a>Microsoft.DocumentDB

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | databaseAccountNames | Hayır | Hayır |
> | Veritabanı hesapları | Yes | Yes |

## <a name="microsoftdomainregistration"></a>Microsoft. DomainRegistration

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | etki alanları | Yes | Yes |
> | Domains/Domainownershiptanýmlayýcýlarý | Hayır | Hayır |
> | generateSsoRequest | Hayır | Hayır |
> | topLevelDomains | Hayır | Hayır |
> | Validatedomainregistrationınformation | Hayır | Hayır |

## <a name="microsoftdynamicslcs"></a>Microsoft. DynamicsLcs

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | lcsprojects | Hayır | Hayır |
> | lcsprojects/clouddağıtımları | Hayır | Hayır |
> | lcsprojects/bağlayıcılar | Hayır | Hayır |

## <a name="microsoftenterpriseknowledgegraph"></a>Microsoft. EnterpriseKnowledgeGraph

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | services | Yes | Yes |

## <a name="microsofteventgrid"></a>Microsoft. EventGrid

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | etki alanları | Yes | Yes |
> | etki alanları/konular | Hayır | Hayır |
> | Eventabonelikleri | Hayır | Hayır |
> | Extensionkonuları | Hayır | Hayır |
> | partnerNamespaces | Yes | Yes |
> | partnerNamespaces/eventChannels | Hayır | Hayır |
> | iş ortağı kayıtları | Yes | Yes |
> | iş ortağı konuları | Yes | Yes |
> | iş ortağı konuları/Eventabonelikleri | Hayır | Hayır |
> | Sistem konuları | Yes | Yes |
> | Sistem konuları/Eventabonelikler | Hayır | Hayır |
> | konuları | Yes | Yes |
> | topicTypes | Hayır | Hayır |

## <a name="microsofteventhub"></a>Microsoft. EventHub

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | leriniz | Yes | Yes |
> | öznitelikleri | Yes | Yes |
> | ad alanları/authorizationrules | Hayır | Hayır |
> | ad alanları/diskalrecoveryconfigs | Hayır | Hayır |
> | ad alanları/eventhubs | Hayır | Hayır |
> | ad alanları/eventhubs/authorizationrules | Hayır | Hayır |
> | ad alanları/eventhubs/consumergroups | Hayır | Hayır |
> | ad alanları/networkrulesets | Hayır | Hayır |

## <a name="microsoftexperimentation"></a>Microsoft. deneme

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | experimentWorkspaces | Yes | Yes |

## <a name="microsoftfalcon"></a>Microsoft. Falcon

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | öznitelikleri | Yes | Yes |

## <a name="microsoftfeatures"></a>Microsoft. Features

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | Özellik sağlayıcıları | Hayır | Hayır |
> | özellikler | Hayır | Hayır |
> | sağlayıcılarla | Hayır | Hayır |
> | subscriptionFeatureRegistrations | Hayır | Hayır |

## <a name="microsoftgallery"></a>Microsoft. Gallery

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | kaydedemez | Hayır | Hayır |
> | gallergıtems | Hayır | Hayır |
> | generateartifactaccessuri | Hayır | Hayır |
> | myarea | Hayır | Hayır |
> | myarea/alan | Hayır | Hayır |
> | myarea/alan/alan | Hayır | Hayır |
> | myareas/Areas/Areas/gallergıtems | Hayır | Hayır |
> | myareas/Areas/gallergıtems | Hayır | Hayır |
> | myarea/gallergıtems | Hayır | Hayır |
> | register | Hayır | Hayır |
> | kaynaklar | Hayır | Hayır |
> | elde edilecek esourcesbyıd | Hayır | Hayır |

## <a name="microsoftgenomics"></a>Microsoft. Genomiks

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | accounts | Yes | Yes |

## <a name="microsoftguestconfiguration"></a>Microsoft. GuestConfiguration

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | Oto Managedaccounts | Yes | Yes |
> | Oto Managedvmconfigurationprofiles | Yes | Yes |
> | Configurationprofileatamalar | Hayır | Hayır |
> | Guestconfigurationatamaları | Hayır | Hayır |
> | yazılımıdır | Hayır | Hayır |
> | softwareUpdateProfile | Hayır | Hayır |
> | softwareUpdates | Hayır | Hayır |

## <a name="microsofthanaonazure"></a>Microsoft. HanaOnAzure

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | Hanaınstances | Yes | Yes |
> | Sapizleyicileri | Yes | Yes |

## <a name="microsofthardwaresecuritymodules"></a>Microsoft. HardwareSecurityModules

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | ayrılmış Atedhsms | Yes | Yes |

## <a name="microsofthdinsight"></a>Microsoft. HDInsight

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | leriniz | Yes | Yes |
> | kümeler/uygulamalar | Hayır | Hayır |

## <a name="microsofthealthcareapis"></a>Microsoft. Healthgelişme API 'leri

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | services | Yes | Yes |
> | Hizmetler/ıomtbağlayıcılar | Hayır | Hayır |
> | Hizmetler/ıomtbağlayıcılar/bağlantılar | Hayır | Hayır |
> | Hizmetler/ıomtbağlayıcılar/eşlemeler | Hayır | Hayır |
> | Hizmetler/Privateendpointconnectionproxy 'Leri | Hayır | Hayır |
> | Hizmetler/privateEndpointConnections | Yes | Yes |
> | Hizmetler/privateLinkResources | Yes | Yes |

## <a name="microsofthybridcompute"></a>Microsoft. HybridCompute

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | larla | Yes | Yes |
> | makineler/uzantılar | Yes | Yes |

## <a name="microsofthybriddata"></a>Microsoft. HybridData

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | Veri yöneticileri | Yes | Yes |

## <a name="microsofthybridnetwork"></a>Microsoft. HybridNetwork

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | cihazlar | Yes | Yes |
> | registeredSubscriptions | Hayır | Hayır |
> | larını | Hayır | Hayır |
> | satıcılar/SKU 'lar | Hayır | Hayır |
> | satıcılar/vnfs | Hayır | Hayır |
> | Virtualnetworkfunctionsku 'Ları | Hayır | Hayır |
> | vnfs | Yes | Yes |

## <a name="microsofthydra"></a>Microsoft. Hydra

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | bileşenleri | Yes | Yes |
> | networkScopes | Yes | Yes |

## <a name="microsoftimportexport"></a>Microsoft. ımportexport

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | Çizelge | Yes | Yes |

## <a name="microsoftinsights"></a>Microsoft. Insights

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | actionGroups | Yes | Yes |
> | activityLogAlerts | Yes | Yes |
> | alertrules | Yes | Yes |
> | autoscalesettings | Yes | Yes |
> | bileşenleri | Yes | Yes |
> | bileşenler/linkedStorageAccounts | Hayır | Hayır |
> | bileşenler/ProactiveDetectionConfigs | Hayır | Hayır |
> | diagnosticSettings | Hayır | Hayır |
> | guestDiagnosticSettings | Yes | Yes |
> | guestDiagnosticSettingsAssociation | Yes | Yes |
> | günlüğe kaydetme profilleri | Yes | Yes |
> | metricAlerts | Yes | Yes |
> | privateLinkScopes | Yes | Yes |
> | privateLinkScopes/privateEndpointConnections | Hayır | Hayır |
> | privateLinkScopes/scopedResources | Hayır | Hayır |
> | Querypack | Yes | Yes |
> | Querypack/sorgular | Hayır | Hayır |
> | scheduledQueryRules | Yes | Yes |
> | Web testleri | Yes | Yes |
> | çalışma kitapları | Yes | Yes |
> | workbooktemplates | Yes | Yes |

## <a name="microsoftintune"></a>Microsoft.Intune

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | diagnosticSettings | Hayır | Hayır |
> | diagnosticSettingsCategories | Hayır | Hayır |

## <a name="microsoftiotcentral"></a>Microsoft. ıotcentral

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | appTemplates | Hayır | Hayır |
> | Iotapps | Yes | Yes |

## <a name="microsoftiotspaces"></a>Microsoft. ıotspaces

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | Graf | Yes | Yes |

## <a name="microsoftkeyvault"></a>Microsoft. Keykasası

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | Silinkaults | Hayır | Hayır |
> | hsmPools | Yes | Yes |
> | managedHSMs | Yes | Yes |
> | kasaları | Yes | Yes |
> | kasa/erişim Ilkeleri | Hayır | Hayır |
> | kasa/eventGridFilters | Hayır | Hayır |
> | kasa/gizlilikler | Hayır | Hayır |

## <a name="microsoftkubernetes"></a>Microsoft. Kubernetes

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | Connectedkümeler | Yes | Yes |
> | registeredSubscriptions | Hayır | Hayır |

## <a name="microsoftkubernetesconfiguration"></a>Microsoft. KubernetesConfiguration

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | sourceControlConfigurations | Hayır | Hayır |

## <a name="microsoftkusto"></a>Microsoft.Kusto

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | leriniz | Yes | Yes |
> | kümeler/attacheddatabaseconfigurations | Hayır | Hayır |
> | kümeler/veritabanları | Hayır | Hayır |
> | kümeler/veritabanları/veri bağlantıları | Hayır | Hayır |
> | kümeler/veritabanları/eventhubconnections | Hayır | Hayır |
> | kümeler/veritabanları/princıpalasbir | Hayır | Hayır |
> | kümeler/veri bağlantıları | Hayır | Hayır |
> | kümeler/princıpalasbir | Hayır | Hayır |
> | kümeler/parçalar | Hayır | Hayır |

## <a name="microsoftlabservices"></a>Microsoft. LabServices

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | labaccounts | Yes | Yes |
> | kullanıcılar | Hayır | Hayır |

## <a name="microsoftlogic"></a>Microsoft. Logic

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | hostingEnvironments | Yes | Yes |
> | Tümleştirme hesapları | Yes | Yes |
> | ıntegrationserviceortamortamları | Yes | Yes |
> | ıntegrationserviceortamortamları/managedap | Hayır | Hayır |
> | ısotedenvironments | Yes | Yes |
> | sürdürülen | Yes | Yes |

## <a name="microsoftmachinelearning"></a>Microsoft. Machinöğrenim

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | Commitmentplanlar | Yes | Yes |
> | Hizmetleri | Yes | Yes |
> | Çalışma alanları | Yes | Yes |

## <a name="microsoftmachinelearningservices"></a>Microsoft.MachineLearningServices

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | çalışma alanı | Yes | Yes |
> | çalışma alanları/hesaplar | Hayır | Hayır |
> | çalışma alanları/eventGridFilters | Hayır | Hayır |

## <a name="microsoftmaintenance"></a>Microsoft. Maintenance

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | applyUpdates | Hayır | Hayır |
> | Configurationatamalar | Hayır | Hayır |
> | maintenanceConfigurations | Yes | Yes |
> | güncelleştirmeler | Hayır | Hayır |

## <a name="microsoftmanagedidentity"></a>Microsoft. Managedıdentity

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | Kimlikler | Hayır | Hayır |
> | Userassignedıdentities | Yes | Yes |

## <a name="microsoftmanagednetwork"></a>Microsoft. ManagedNetwork

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | managedNetworks | Yes | Yes |
> | managedNetworks/managedNetworkGroups | Yes | Yes |
> | managedNetworks/managedNetworkPeeringPolicies | Yes | Yes |
> | bildirim | Yes | Yes |

## <a name="microsoftmanagedservices"></a>Microsoft. ManagedServices

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | Pazar Placeryumuristrationdefinitions | Hayır | Hayır |
> | Registrationatamaları | Hayır | Hayır |
> | registrationDefinitions | Hayır | Hayır |

## <a name="microsoftmanagement"></a>Microsoft. Management

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | getEntities | Hayır | Hayır |
> | Yönetim grupları | Hayır | Hayır |
> | managementGroups/Settings | Hayır | Hayır |
> | kaynaklar | Hayır | Hayır |
> | startTenantBackfill | Hayır | Hayır |
> | tenantBackfillStatus | Hayır | Hayır |

## <a name="microsoftmaps"></a>Microsoft. Maps

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | accounts | Yes | Yes |
> | hesaplar/eventGridFilters | Hayır | Hayır |
> | hesaplar/privateAtlases | Yes | Yes |

## <a name="microsoftmarketplace"></a>Microsoft. Market

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | sunar | Hayır | Hayır |
> | offerTypes | Hayır | Hayır |
> | offerTypes/yayımcılar | Hayır | Hayır |
> | offerTypes/yayımcılar/teklifler | Hayır | Hayır |
> | offerTypes/yayımcılar/teklifler/planlar | Hayır | Hayır |
> | offerTypes/yayımcılar/teklifler/planlar/anlaşmalar | Hayır | Hayır |
> | offertypes/yayımcılar/teklifler/planlar/yapılandırmalarını | Hayır | Hayır |
> | offertypes/yayımcılar/teklifler/planlar/yapılandırmalarını/ımportımage | Hayır | Hayır |
> | privategallergıtems | Hayır | Hayır |
> | privateStoreClient | Hayır | Hayır |
> | Privatemağazaların | Hayır | Hayır |
> | Privatemağazaların/tekliflerin | Hayır | Hayır |
> | ürün | Hayır | Hayır |
> | Publishers | Hayır | Hayır |
> | Yayımcılar/teklifler | Hayır | Hayır |
> | Yayımcılar/teklifler/Düzeltme | Hayır | Hayır |
> | register | Hayır | Hayır |

## <a name="microsoftmarketplaceapps"></a>Microsoft. MarketplaceApps

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | classicDevServices | Yes | Yes |
> | updateCommunicationPreference | Hayır | Hayır |

## <a name="microsoftmarketplaceordering"></a>Microsoft. Marketplacesıralaması

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | anlaşmalar | Hayır | Hayır |
> | offertypes | Hayır | Hayır |

## <a name="microsoftmedia"></a>Microsoft. Media

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | mediaservices | Yes | Yes |
> | mediaservices/accountFilters | Hayır | Hayır |
> | mediaservices/varlıklar | Hayır | Hayır |
> | mediaservices/varlıklar/assetFilters | Hayır | Hayır |
> | mediaservices/contentKeyPolicies | Hayır | Hayır |
> | mediaservices/eventGridFilters | Hayır | Hayır |
> | mediaservices/liveEventOperations | Hayır | Hayır |
> | mediaservices/liveEvents | Yes | Yes |
> | mediaservices/liveEvents/Liveçıktılar | Hayır | Hayır |
> | mediaservices/liveEvents/Privateendpointconnectionproxy 'Leri | Hayır | Hayır |
> | mediaservices/liveOutputOperations | Hayır | Hayır |
> | mediaservices/Mediagraf | Hayır | Hayır |
> | mediaservices/streamingEndpointOperations | Hayır | Hayır |
> | mediaservices/streamingEndpoints | Yes | Yes |
> | mediaservices/streamingEndpoints/Privateendpointconnectionproxy 'Leri | Hayır | Hayır |
> | mediaservices/Streamingkonumlandırıcı | Hayır | Hayır |
> | mediaservices/streamingPolicies | Hayır | Hayır |
> | mediaservices/streamingPrivateEndpointConnectionProxyOperations | Hayır | Hayır |
> | mediaservices/dönüşümler | Hayır | Hayır |
> | mediaservices/dönüşümler/işler | Hayır | Hayır |

## <a name="microsoftmicroservices4spring"></a>Microsoft. Microservices4Spring

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | Appkümeler | Yes | Yes |

## <a name="microsoftmigrate"></a>Microsoft. Migrate

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | assessmentProjects | Yes | Yes |
> | migrateprojects | Yes | Yes |
> | moveCollections | Yes | Yes |
> | projeyle | Yes | Yes |

## <a name="microsoftmixedreality"></a>Microsoft. MixedReality

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | holographicsBroadcastAccounts | Yes | Yes |
> | objectUnderstandingAccounts | Yes | Yes |
> | remoteRenderingAccounts | Yes | Yes |
> | spatialAnchorsAccounts | Yes | Yes |

## <a name="microsoftnetapp"></a>Microsoft. NetApp

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | netAppAccounts | Evet | Hayır |
> | netAppAccounts/accountBackups | Hayır | Hayır |
> | netAppAccounts/Capacityhavuzları | Evet | Hayır |
> | netAppAccounts/Capacityhavuzları/birimleri | Evet | Hayır |
> | netAppAccounts/Capacityhavuzlar/birimler/anlık görüntüler | Hayır | Hayır |

## <a name="microsoftnetwork"></a>Microsoft.Network

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | Applicationgateway 'ler | Yes | Yes |
> | applicationGatewayWebApplicationFirewallPolicies | Yes | Yes |
> | applicationSecurityGroups | Yes | Yes |
> | azureFirewallFqdnTags | Hayır | Hayır |
> | azureFirewalls | Evet | Hayır |
> | Savunma Konakları | Evet | Hayır |
> | bgpServiceCommunities | Hayır | Hayır |
> | bağlantının | Yes | Yes |
> | ddosCustomPolicies | Yes | Yes |
> | Ddosprotectionplanlar | Yes | Yes |
> | Dnsoperationdurumları | Hayır | Hayır |
> | dnszones | Yes | Yes |
> | dnszones/A | Hayır | Hayır |
> | dnszones/AAAA | Hayır | Hayır |
> | dnszones/tümü | Hayır | Hayır |
> | dnszones/CAA | Hayır | Hayır |
> | dnszones/CNAME | Hayır | Hayır |
> | dnszones/MX | Hayır | Hayır |
> | dnszones/NS | Hayır | Hayır |
> | dnszones/PTR | Hayır | Hayır |
> | dnszones/kayıt kümeleri | Hayır | Hayır |
> | dnszones/SOA | Hayır | Hayır |
> | dnszones/SRV | Hayır | Hayır |
> | dnszones/TXT | Hayır | Hayır |
> | Expressroutedevreleri | Yes | Yes |
> | expressRouteCrossConnections | Yes | Yes |
> | Expressroutegateway 'ler | Yes | Yes |
> | expressRoutePorts | Yes | Yes |
> | expressRouteServiceProviders | Hayır | Hayır |
> | firewallPolicies | Yes | Yes |
> | frontkapıların | Evet, ancak sınırlı ( [aşağıdaki nota](#frontdoor)bakın) | Yes |
> | frontdoorWebApplicationFirewallManagedRuleSets | Evet, ancak sınırlı ( [aşağıdaki nota](#frontdoor)bakın) | Hayır |
> | frontdoorWebApplicationFirewallPolicies | Evet, ancak sınırlı ( [aşağıdaki nota](#frontdoor)bakın) | Yes |
> | getDnsResourceReference | Hayır | Hayır |
> | ınternalnotify | Hayır | Hayır |
> | loadBalancers | Yes | Yes |
> | Localnetworkgateway 'ler | Yes | Yes |
> | Natgateway 'ler | Yes | Yes |
> | Networkıntpolicies Ilkeleri | Yes | Yes |
> | NetworkInterfaces | Yes | Yes |
> | networkProfiles | Yes | Yes |
> | networkSecurityGroups | Yes | Yes |
> | networkWatchers | Yes | Yes |
> | networkWatchers/Connectionmonitörleri | Evet | Hayır |
> | networkWatchers/flowLogs | Hayır | Hayır |
> | networkWatchers/uzunluler | Evet | Hayır |
> | networkWatchers/Pingkafesler | Evet | Hayır |
> | p2sVpnGateways | Yes | Yes |
> | privateDnsOperationStatuses | Hayır | Hayır |
> | privateDnsZones | Yes | Yes |
> | privateDnsZones/A | Hayır | Hayır |
> | privateDnsZones/AAAA | Hayır | Hayır |
> | privateDnsZones/tümü | Hayır | Hayır |
> | privateDnsZones/CNAME | Hayır | Hayır |
> | privateDnsZones/MX | Hayır | Hayır |
> | privateDnsZones/PTR | Hayır | Hayır |
> | privateDnsZones/SOA | Hayır | Hayır |
> | privateDnsZones/SRV | Hayır | Hayır |
> | privateDnsZones/TXT | Hayır | Hayır |
> | privateDnsZones/virtualNetworkLinks | Yes | Yes |
> | privateEndpoints | Yes | Yes |
> | privateLinkServices | Yes | Yes |
> | Publicıpaddresses | Yes | Yes |
> | Publicıpöneklerini | Yes | Yes |
> | routeFilters | Yes | Yes |
> | routeTables | Yes | Yes |
> | serviceEndpointPolicies | Yes | Yes |
> | trafficManagerGeographicHierarchies | Hayır | Hayır |
> | trafficmanagerprofiles | Yes | Yes |
> | trafficmanagerprofiles/heatMaps | Hayır | Hayır |
> | trafficManagerUserMetricsKeys | Hayır | Hayır |
> | Virtualhub 'Lar | Yes | Yes |
> | virtualNetworkGateways | Yes | Yes |
> | virtualNetworks | Yes | Yes |
> | virtualNetworks/alt ağları | Hayır | Hayır |
> | virtualNetworkTaps | Yes | Yes |
> | Virtualwan | Evet | Hayır |
> | Vpngateway 'ler | Yes | Yes |
> | vpnSites | Yes | Yes |
> | webApplicationFirewallPolicies | Yes | Yes |

<a id="frontdoor"></a>

> [!NOTE]
> Azure ön kapı hizmeti için, kaynak oluştururken Etiketler uygulayabilirsiniz, ancak bu durumda etiketleri güncelleştirmek veya eklemek Şu anda desteklenmemektedir.


## <a name="microsoftnotebooks"></a>Microsoft. Not defterleri

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | Not defteri proxy 'leri | Hayır | Hayır |

## <a name="microsoftnotificationhubs"></a>Microsoft. Notificationhub 'Lar

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | öznitelikleri | Evet | Hayır |
> | ad alanları/Notificationhub 'Lar | Evet | Hayır |

## <a name="microsoftobjectstore"></a>Microsoft. ObjectStore

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | osNamespaces | Yes | Yes |

## <a name="microsoftoffazure"></a>Microsoft. OffAzure

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | Hiper sanal siteler | Yes | Yes |
> | Importsites | Yes | Yes |
> | Sunucusiteleri | Yes | Yes |
> | VMwareSites | Yes | Yes |

## <a name="microsoftoperationalinsights"></a>Microsoft. Operationalınsights

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | leriniz | Yes | Yes |
> | deletedWorkspaces | Hayır | Hayır |
> | Bağlantı hedefleri | Hayır | Hayır |
> | Storageınsii configs | Hayır | Hayır |
> | çalışma alanı | Yes | Yes |
> | çalışma alanları/veri dışarı aktarmaları | Hayır | Hayır |
> | çalışma alanları/veri kaynakları | Hayır | Hayır |
> | çalışma alanları/linkedServices | Hayır | Hayır |
> | çalışma alanları/linkedStorageAccounts | Hayır | Hayır |
> | çalışma alanları/meta veriler | Hayır | Hayır |
> | çalışma alanları/sorgu | Hayır | Hayır |
> | çalışma alanları/Scopedprivatelinkproxy 'Leri | Hayır | Hayır |

## <a name="microsoftoperationsmanagement"></a>Microsoft. OperationsManagement

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | managementassociations | Hayır | Hayır |
> | managementconfigurations | Yes | Yes |
> | çözümler | Yes | Yes |
> | görünümler | Yes | Yes |

## <a name="microsoftpeering"></a>Microsoft. eşleme

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | Yasallıklar | Hayır | Hayır |
> | peerAsns | Hayır | Hayır |
> | eşlemeleri | Yes | Yes |
> | Peeringserviceülkeleriyle | Hayır | Hayır |
> | peeringServiceProviders | Hayır | Hayır |
> | peeringServices | Yes | Yes |

## <a name="microsoftpolicyinsights"></a>Microsoft. Poliyelei

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | Poliyevents | Hayır | Hayır |
> | policyMetadata | Hayır | Hayır |
> | policyStates | Hayır | Hayır |
> | policyTrackedResources | Hayır | Hayır |
> | düzeltmeler | Hayır | Hayır |

## <a name="microsoftportal"></a>Microsoft. Portal

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> |  konsolları | Hayır | Hayır |
> | panolar | Yes | Yes |
> | userSettings | Hayır | Hayır |

## <a name="microsoftpowerbi"></a>Microsoft. PowerBI

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | Privatelinkservicesforpowerbı | Yes | Yes |
> | Kira | Yes | Yes |
> | Kiracılar/çalışma alanları | Hayır | Hayır |
> | workspaceCollections | Yes | Yes |

## <a name="microsoftpowerbidedicated"></a>Microsoft. Powerbiadanmış

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | kapasiteler | Yes | Yes |

## <a name="microsoftprojectbabylon"></a>Microsoft. ProjectBabylon

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | accounts | Yes | Yes |

## <a name="microsoftproviderhub"></a>Microsoft. ProviderHub

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | Providerkayıtları | Hayır | Hayır |
> | Providerkayıtları/Defaultrollout | Hayır | Hayır |
> | Providerkayıtlarıyla/resourceTypeRegistrations | Hayır | Hayır |
> | piyasaya çıkarma | Yes | Yes |

## <a name="microsoftquantum"></a>Microsoft. hisse

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | Çalışma alanları | Yes | Yes |

## <a name="microsoftrecoveryservices"></a>Microsoft. RecoveryServices

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | Backupkorunabilir | Hayır | Hayır |
> | kasaları | Yes | Yes |

## <a name="microsoftredhatopenshift"></a>Microsoft. RedHatOpenShift

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | OpenShiftClusters | Yes | Yes |

## <a name="microsoftrelay"></a>Microsoft. Relay

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | öznitelikleri | Yes | Yes |
> | ad alanları/authorizationrules | Hayır | Hayır |
> | ad alanları/hybridconnections | Hayır | Hayır |
> | ad alanları/hybridconnections/authorizationrules | Hayır | Hayır |
> | ad alanları/privateEndpointConnections | Hayır | Hayır |
> | ad alanları/wcfreyerleştiri | Hayır | Hayır |
> | ad alanları/wcfreyerleştirme/authorizationrules | Hayır | Hayır |

## <a name="microsoftresourcegraph"></a>Microsoft. ResourceGraph

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | lardır | Yes | Yes |
> | resourceChangeDetails | Hayır | Hayır |
> | resourceChanges | Hayır | Hayır |
> | kaynaklar | Hayır | Hayır |
> | resourcesHistory | Hayır | Hayır |
> | subscriptionsStatus | Hayır | Hayır |

## <a name="microsoftresourcehealth"></a>Microsoft. ResourceHealth

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | Kullanılabilirlik durumları | Hayır | Hayır |
> | Childadvailabilitydurumlar | Hayır | Hayır |
> | childResources | Hayır | Hayır |
> | acil sorun sorunları | Hayır | Hayır |
> | etkinlikler | Hayır | Hayır |
> | ımpactedresources | Hayır | Hayır |
> | meta veriler | Hayır | Hayır |
> | bildirimler | Hayır | Hayır |

## <a name="microsoftresources"></a>Microsoft. resources

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | calculateTemplateHash | Hayır | Hayır |
> | dağıtımlar | Evet | Hayır |
> | dağıtımlar/işlemler | Hayır | Hayır |
> | deploymentScripts | Yes | Yes |
> | deploymentScripts/Günlükler | Hayır | Hayır |
> | Köprü | Hayır | Hayır |
> | notifyResourceJobs | Hayır | Hayır |
> | sağlayıcılarla | Hayır | Hayır |
> | resourceGroups | Evet | Hayır |
> | Aboneliklerin | Evet | Hayır |
> | Templatespec | Yes | Yes |
> | Templatespec/sürümler | Yes | Yes |
> | Kira | Hayır | Hayır |

## <a name="microsoftsaas"></a>Microsoft. SaaS

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | uygulamalar | Yes | Yes |
> | saasresources | Hayır | Hayır |

## <a name="microsoftsearch"></a>Microsoft. Search

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | resourceHealthMetadata | Hayır | Hayır |
> | searchServices | Yes | Yes |

## <a name="microsoftsecurity"></a>Microsoft.Security

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | adaptiveNetworkHardenings | Hayır | Hayır |
> | advancedThreatProtectionSettings | Hayır | Hayır |
> | alerts | Hayır | Hayır |
> | alertsSuppressionRules | Hayır | Hayır |
> | allowedConnections | Hayır | Hayır |
> | Applicationwhitedökümler | Hayır | Hayır |
> | assessmentMetadata | Hayır | Hayır |
> | değerlendirme | Hayır | Hayır |
> | Oto Dissalertsrules | Hayır | Hayır |
> | akışlarını otomatikleştirin | Yes | Yes |
> | Oto Provisioningsettings | Hayır | Hayır |
> | Uyumluluklarına | Hayır | Hayır |
> | dataCollectionAgents | Hayır | Hayır |
> | deviceSecurityGroups | Hayır | Hayır |
> | discoveredSecuritySolutions | Hayır | Hayır |
> | externalSecuritySolutions | Hayır | Hayır |
> | Informationprotectionpolicies | Hayır | Hayır |
> | ıotsecuritysolutions | Yes | Yes |
> | ıotsecuritysolutions/analiz Ticsmodeller | Hayır | Hayır |
> | ıotsecuritysolutions/Analticsmodeller/Aggreggıt uyarıları | Hayır | Hayır |
> | ıotsecuritysolutions/analiz Ticsmodeller/Aggreg, öneriler | Hayır | Hayır |
> | ıotsecuritysolutions/ıotalerts | Hayır | Hayır |
> | ıotsecuritysolutions/ıotalerttypes | Hayır | Hayır |
> | Jağaccesspolicies | Hayır | Hayır |
> | ilkeler | Hayır | Hayır |
> | fiyatlandırmalar | Hayır | Hayır |
> | Reve daha karmaşık bakım standartları | Hayır | Hayır |
> | Rekontrol ve Re, | Hayır | Hayır |
> | Rekontrol ve Re, Re, Re, | Hayır | Hayır |
> | secureScoreControlDefinitions | Hayır | Hayır |
> | secureScoreControls | Hayır | Hayır |
> | Securesçekirdekler | Hayır | Hayır |
> | Securesçekirdekler/secureScoreControls | Hayır | Hayır |
> | securityContacts | Hayır | Hayır |
> | securitySolutions | Hayır | Hayır |
> | securitySolutionsReferenceData | Hayır | Hayır |
> | Securitydurumlardan | Hayır | Hayır |
> | securityStatusesSummaries | Hayır | Hayır |
> | Sunucukullanılabilirliği | Hayır | Hayır |
> | ayarlar | Hayır | Hayır |
> | subAssessments | Hayır | Hayır |
> | Görevler | Hayır | Hayır |
> | anlatır | Hayır | Hayır |
> | çalışma alanı ayarları | Hayır | Hayır |

## <a name="microsoftsecuritygraph"></a>Microsoft. SecurityGraph

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | diagnosticSettings | Hayır | Hayır |
> | diagnosticSettingsCategories | Hayır | Hayır |

## <a name="microsoftsecurityinsights"></a>Microsoft. Securityınsights

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | toplamaları | Hayır | Hayır |
> | alertRules | Hayır | Hayır |
> | Alertrutatemplates | Hayır | Hayır |
> | automationRules | Hayır | Hayır |
> | yer işaretleri | Hayır | Hayır |
> | çalışmaların | Hayır | Hayır |
> | Veri bağlayıcıları | Hayır | Hayır |
> | dataConnectorsCheckRequirements | Hayır | Hayır |
> | varlıklar | Hayır | Hayır |
> | entityQueries | Hayır | Hayır |
> | olaylar | Hayır | Hayır |
> | officeConsents | Hayır | Hayır |
> | ayarlar | Hayır | Hayır |
> | Threatıntelligence | Hayır | Hayır |

## <a name="microsoftserialconsole"></a>Microsoft. SerialConsole

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | consoleServices | Hayır | Hayır |

## <a name="microsoftservicebus"></a>Microsoft.ServiceBus

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | öznitelikleri | Yes | Yes |
> | ad alanları/authorizationrules | Hayır | Hayır |
> | ad alanları/diskalrecoveryconfigs | Hayır | Hayır |
> | ad alanları/eventgridfilters | Hayır | Hayır |
> | ad alanları/networkrulesets | Hayır | Hayır |
> | ad alanları/kuyruklar | Hayır | Hayır |
> | ad alanları/kuyruklar/authorizationrules | Hayır | Hayır |
> | ad alanları/konular | Hayır | Hayır |
> | ad alanları/konular/authorizationrules | Hayır | Hayır |
> | ad alanları/konular/abonelikler | Hayır | Hayır |
> | ad alanları/konular/abonelikler/kurallar | Hayır | Hayır |
> | premiumMessagingRegions | Hayır | Hayır |

## <a name="microsoftservicefabric"></a>Microsoft. ServiceFabric

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | uygulamalar | Yes | Yes |
> | leriniz | Yes | Yes |
> | kümeler/uygulamalar | Hayır | Hayır |
> | Kapsayıcı grupları | Yes | Yes |
> | containerGroupSets | Yes | Yes |
> | edgeclusters | Yes | Yes |
> | edgeclusters/uygulamalar | Hayır | Hayır |
> | managedkümeler | Yes | Yes |
> | managedkümeler/nodetypes | Hayır | Hayır |
> | Mamak | Yes | Yes |
> | secretmağazaları | Yes | Yes |
> | secretmağazaları/sertifikaları | Hayır | Hayır |
> | secretmağazaları/gizli dizileri | Hayır | Hayır |
> | volumes | Yes | Yes |

## <a name="microsoftservicefabricmesh"></a>Microsoft. Servicefabrickafesi

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | uygulamalar | Yes | Yes |
> | Kapsayıcı grupları | Yes | Yes |
> | geçidinin | Yes | Yes |
> | Mamak | Yes | Yes |
> | kaynaklanır | Yes | Yes |
> | volumes | Yes | Yes |

## <a name="microsoftservices"></a>Microsoft. Services

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | Providerkayıtları | Hayır | Hayır |
> | Providerkayıtlarıyla/resourceTypeRegistrations | Hayır | Hayır |
> | piyasaya çıkarma | Yes | Yes |

## <a name="microsoftsignalrservice"></a>Microsoft. SignalRService

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | SignalR | Yes | Yes |
> | SignalR/eventGridFilters | Hayır | Hayır |

## <a name="microsoftsoftwareplan"></a>Microsoft. SoftwarePlan

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | Hybriduseavantajlar | Hayır | Hayır |

## <a name="microsoftsolutions"></a>Microsoft. Solutions

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | applicationDefinitions | Yes | Yes |
> | uygulamalar | Yes | Yes |
> | Jistekleri | Yes | Yes |


## <a name="microsoftsql"></a>Microsoft. SQL

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | ManagedInstances | Yes | Yes |
> | ManagedInstances/veritabanları | Evet ( [aşağıdaki nota](#sqlnote)bakın) | Yes |
> | ManagedInstances/veritabanları/backupShortTermRetentionPolicies | Hayır | Hayır |
> | ManagedInstances/veritabanları/şemalar/tablolar/sütunlar/sensitivityLabels | Hayır | Hayır |
> | ManagedInstances/veritabanları/ | Hayır | Hayır |
> | ManagedInstances/veritabanları/ek | Hayır | Hayır |
> | ManagedInstances/encryptionProtector | Hayır | Hayır |
> | ManagedInstances/anahtarlar | Hayır | Hayır |
> | ManagedInstances/Restokbledroppeddatabases/backupShortTermRetentionPolicies | Hayır | Hayır |
> | ManagedInstances/ | Hayır | Hayır |
> | larý | Yes | Yes |
> | sunucular/Yöneticiler | Hayır | Hayır |
> | sunucular/communicationLinks | Hayır | Hayır |
> | sunucular/veritabanları | Evet ( [aşağıdaki nota](#sqlnote)bakın) | Yes |
> | sunucular/encryptionProtector | Hayır | Hayır |
> | sunucular/firewallRules | Hayır | Hayır |
> | sunucular/anahtarlar | Hayır | Hayır |
> | sunucular/Restokbledroppeddatabases | Hayır | Hayır |
> | Sunucu/hizmet hedefleri | Hayır | Hayır |
> | sunucular/tdeCertificates | Hayır | Hayır |
> | Virtualkümeler | Hayır | Hayır |

<a id="sqlnote"></a>

> [!NOTE]
> Ana veritabanı etiketleri desteklemez, ancak Azure SYNAPSE Analytics veritabanları da dahil olmak üzere diğer veritabanları, Etiketler ' i destekler. Azure SYNAPSE Analytics veritabanları etkin (duraklatılmış değil) durumda olmalıdır.

## <a name="microsoftsqlvirtualmachine"></a>Microsoft. SqlVirtualMachine

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | SqlVirtualMachineGroups | Yes | Yes |
> | SqlVirtualMachineGroups/kullanılabilirliği Bilitygrouplisteners | Hayır | Hayır |
> | SqlVirtualMachines | Yes | Yes |

## <a name="microsoftstorage"></a>Microsoft.Storage

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | storageAccounts | Yes | Yes |
> | storageAccounts/blobServices | Hayır | Hayır |
> | storageAccounts/fileServices | Hayır | Hayır |
> | storageAccounts/queueServices | Hayır | Hayır |
> | storageAccounts/Services | Hayır | Hayır |
> | storageAccounts/Services/metricDefinitions | Hayır | Hayır |
> | storageAccounts/tableServices | Hayır | Hayır |
> | vardır | Hayır | Hayır |

## <a name="microsoftstoragecache"></a>Microsoft. StorageCache

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | önbelleklerinde | Yes | Yes |
> | önbellekler/Storagetaral | Hayır | Hayır |
> | Usagemodeller | Hayır | Hayır |

## <a name="microsoftstoragereplication"></a>Microsoft. Storagerepce

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | replicationGroups | Hayır | Hayır |

## <a name="microsoftstoragesync"></a>Microsoft. Storagessync

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | storageSyncServices | Yes | Yes |
> | storageSyncServices/registeredServers | Hayır | Hayır |
> | storageSyncServices/syncGroups | Hayır | Hayır |
> | storageSyncServices/syncGroups/cloudEndpoints | Hayır | Hayır |
> | storageSyncServices/syncGroups/serverEndpoints | Hayır | Hayır |
> | storageSyncServices/iş akışları | Hayır | Hayır |

## <a name="microsoftstoragesyncdev"></a>Microsoft. StorageSyncDev

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | storageSyncServices | Yes | Yes |
> | storageSyncServices/registeredServers | Hayır | Hayır |
> | storageSyncServices/syncGroups | Hayır | Hayır |
> | storageSyncServices/syncGroups/cloudEndpoints | Hayır | Hayır |
> | storageSyncServices/syncGroups/serverEndpoints | Hayır | Hayır |
> | storageSyncServices/iş akışları | Hayır | Hayır |

## <a name="microsoftstoragesyncint"></a>Microsoft. Storagesyncınt

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | storageSyncServices | Yes | Yes |
> | storageSyncServices/registeredServers | Hayır | Hayır |
> | storageSyncServices/syncGroups | Hayır | Hayır |
> | storageSyncServices/syncGroups/cloudEndpoints | Hayır | Hayır |
> | storageSyncServices/syncGroups/serverEndpoints | Hayır | Hayır |
> | storageSyncServices/iş akışları | Hayır | Hayır |

## <a name="microsoftstorsimple"></a>Microsoft. StorSimple

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | ilerinde | Yes | Yes |

## <a name="microsoftstreamanalytics"></a>Microsoft. StreamAnalytics

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | leriniz | Yes | Yes |
> | streammingjobs | Evet (aşağıdaki nota bakın) | Yes |

> [!NOTE]
> Streamingjobs çalışırken bir etiket ekleyemezsiniz. Etiketi eklemek için kaynağı durdurun.

## <a name="microsoftsubscription"></a>Microsoft. Subscription

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | iptal | Hayır | Hayır |
> | CreateSubscription | Hayır | Hayır |
> | seçin | Hayır | Hayır |
> | yeniden adlandır | Hayır | Hayır |
> | SubscriptionDefinitions | Hayır | Hayır |
> | SubscriptionOperations | Hayır | Hayır |
> | Aboneliklerin | Hayır | Hayır |

## <a name="microsoftsynapse"></a>Microsoft. SYNAPSE

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | Privatelinkhub 'Ları | Yes | Yes |
> | çalışma alanı | Yes | Yes |
> | çalışma alanları/bigDataPools | Yes | Yes |
> | çalışma alanları/Operationdurumlar | Hayır | Hayır |
> | çalışma alanları/sqlPools | Yes | Yes |

## <a name="microsofttimeseriesinsights"></a>Microsoft. Timeseriesınsights

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | lý | Evet | Hayır |
> | ortamlar/accessPolicies | Hayır | Hayır |
> | ortamlar/EventSources | Evet | Hayır |
> | ortamlar/Referencedataset 'ler | Evet | Hayır |

## <a name="microsofttoken"></a>Microsoft. Token

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | depolaya | Yes | Yes |
> | depolar/accessPolicies | Hayır | Hayır |
> | depolar/hizmetler | Hayır | Hayır |
> | depolar/hizmetler/belirteçler | Hayır | Hayır |

## <a name="microsoftvirtualmachineimages"></a>Microsoft. Virtualmachineımages

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | ımagetemplates | Yes | Yes |
> | ımagetemplates/Runçıktılar | Hayır | Hayır |

## <a name="microsoftvmware"></a>Microsoft. VMware

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | ArcZones | Yes | Yes |
> | ResourcePools | Yes | Yes |
> | VCenter | Yes | Yes |
> | VirtualMachines | Yes | Yes |
> | VirtualMachineTemplates | Yes | Yes |
> | VirtualNetworks | Yes | Yes |

## <a name="microsoftvmwarecloudsimple"></a>Microsoft. Vmwarechoparlör basit

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | ayrılmış Cloudnodes | Yes | Yes |
> | ayrılmış CloudService | Yes | Yes |
> | virtualMachines | Yes | Yes |

## <a name="microsoftvmwareonazure"></a>Microsoft. VMwareOnAzure

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | Privatebulutlar | Yes | Yes |

## <a name="microsoftvnfmanager"></a>Microsoft. VnfManager

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | cihazlar | Yes | Yes |
> | registeredSubscriptions | Hayır | Hayır |
> | larını | Hayır | Hayır |
> | satıcılar/SKU 'lar | Hayır | Hayır |
> | satıcılar/vnfs | Hayır | Hayır |
> | Virtualnetworkfunctionsku 'Ları | Hayır | Hayır |
> | vnfs | Yes | Yes |

## <a name="microsoftvsonline"></a>Microsoft. VSOnline

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | accounts | Yes | Yes |
> | Planlama | Yes | Yes |
> | registeredSubscriptions | Hayır | Hayır |

## <a name="microsoftweb"></a>Microsoft. Web

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | apiManagementAccounts | Hayır | Hayır |
> | apiManagementAccounts/Apiacl 'Ler | Hayır | Hayır |
> | apiManagementAccounts/API 'ler | Hayır | Hayır |
> | apiManagementAccounts/API/Apiacl 'Ler | Hayır | Hayır |
> | apiManagementAccounts/API/Connectionacl 'Ler | Hayır | Hayır |
> | apiManagementAccounts/API/bağlantı | Hayır | Hayır |
> | apiManagementAccounts/API/Connections/Connectionacl 'Ler | Hayır | Hayır |
> | apiManagementAccounts/API/localizedDefinitions | Hayır | Hayır |
> | apiManagementAccounts/Connectionacl 'Ler | Hayır | Hayır |
> | apiManagementAccounts/bağlantılar | Hayır | Hayır |
> | billingMeters | Hayır | Hayır |
> | sertifikalar | Yes | Yes |
> | Connectiongateway 'ler | Yes | Yes |
> | bağlantının | Yes | Yes |
> | Customapsıs | Yes | Yes |
> | Silinmi siteleri | Hayır | Hayır |
> | hostingEnvironments | Yes | Yes |
> | hostingEnvironments/eventGridFilters | Hayır | Hayır |
> | hostingEnvironments/multiRolePools | Hayır | Hayır |
> | hostingEnvironments/workerPools | Hayır | Hayır |
> | Kubeortamları | Yes | Yes |
> | publishingUsers | Hayır | Hayır |
> | Öneriler | Hayır | Hayır |
> | resourceHealthMetadata | Hayır | Hayır |
> | zamanları | Hayır | Hayır |
> | serverFarms | Yes | Yes |
> | Sunucugrupları/eventGridFilters | Hayır | Hayır |
> | Siteler | Yes | Yes |
> | siteler/yapılandırma  | Hayır | Hayır |
> | siteler/eventGridFilters | Hayır | Hayır |
> | siteler/hostNameBindings | Hayır | Hayır |
> | Sites/networkConfig | Hayır | Hayır |
> | siteler/premieraddons | Yes | Yes |
> | siteler/yuvalar | Yes | Yes |
> | siteler/yuvalar/eventGridFilters | Hayır | Hayır |
> | siteler/yuvalar/hostNameBindings | Hayır | Hayır |
> | siteler/yuvalar/networkConfig | Hayır | Hayır |
> | sourceControls | Hayır | Hayır |
> | staticSites | Yes | Yes |
> | doğrulamalısınız | Hayır | Hayır |
> | verifyHostingEnvironmentVnet | Hayır | Hayır |

## <a name="microsoftwindowsdefenderatp"></a>Microsoft. Windowssavunma Deratp

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | diagnosticSettings | Hayır | Hayır |
> | diagnosticSettingsCategories | Hayır | Hayır |

## <a name="microsoftwindowsesu"></a>Microsoft. WindowsESU

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | Çoğulactivationkeys | Yes | Yes |

## <a name="microsoftwindowsiot"></a>Microsoft. Windowsıot

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | DeviceServices | Yes | Yes |

## <a name="microsoftworkloadbuilder"></a>Microsoft. WorkloadBuilder

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | lerine | Yes | Yes |
> | iş yükleri/örnekler | Hayır | Hayır |
> | iş yükleri/sürümler | Hayır | Hayır |
> | iş yükleri/sürümler/yapılar | Hayır | Hayır |

## <a name="microsoftworkloadmonitor"></a>Microsoft. WorkloadMonitor

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
> | ------------- | ----------- | ----------- |
> | bileşenleri | Hayır | Hayır |
> | componentsSummary | Hayır | Hayır |
> | Izleme örnekleri | Hayır | Hayır |
> | Izleme ınstancessummary | Hayır | Hayır |
> | monitörün | Hayır | Hayır |
> | notificationSettings | Hayır | Hayır |

## <a name="next-steps"></a>Sonraki adımlar

Kaynaklara etiketlerin nasıl uygulanacağını öğrenmek için bkz. [Azure kaynaklarınızı düzenlemek için etiketleri kullanma](tag-resources.md).
