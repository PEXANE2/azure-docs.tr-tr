---
title: Kaynaklar için etiket desteği
description: Hangi Azure kaynak türlerinin etiketleri desteklediğini gösterir. Tüm Azure hizmetleri için ayrıntılar sağlar.
ms.topic: conceptual
ms.date: 04/06/2020
ms.openlocfilehash: e012126995136bec15dc360be5e91007b6f69f09
ms.sourcegitcommit: 6397c1774a1358c79138976071989287f4a81a83
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/07/2020
ms.locfileid: "80802501"
---
# <a name="tag-support-for-azure-resources"></a>Azure kaynakları için etiket desteği
Bu makalede, kaynak türünün [etiketleri](tag-resources.md)destekleyip desteklemediği açıklanmaktadır. **Destekler etiketleri** etiketli sütun, kaynak türünün etiket için bir özelliği olup olmadığını gösterir. Maliyet raporunda **Etiket** etiketli sütun, kaynak türünün etiketi maliyet raporuna geçirip geçirmeyeceğini gösterir. Maliyetleri Maliyet Yönetimi maliyet [analizinde,](../../cost-management-billing/costs/quick-acm-cost-analysis.md#understanding-grouping-and-filtering-options) Azure [fatura faturasında ve günlük kullanım verilerinde](../../cost-management-billing/manage/download-azure-invoice-daily-usage-date.md)etiketlere göre görüntüleyebilirsiniz.

Virgülle ayrılmış değerler dosyasıyla aynı verileri almak için [etiket-support.csv'yi](https://github.com/tfitzmac/resource-capabilities/blob/master/tag-support.csv)indirin.

Kaynak sağlayıcı ad alanına atla:
> [!div class="op_single_selector"]
> - [Microsoft.AAD](#microsoftaad)
> - [Microsoft.Addons](#microsoftaddons)
> - [Microsoft.ADHybridHealthService](#microsoftadhybridhealthservice)
> - [Microsoft.Advisor](#microsoftadvisor)
> - [Microsoft.AlertsManagement](#microsoftalertsmanagement)
> - [Microsoft.AnalysisServices](#microsoftanalysisservices)
> - [Microsoft.ApiManagement](#microsoftapimanagement)
> - [Microsoft.AppConfiguration](#microsoftappconfiguration)
> - [Microsoft.AppPlatform](#microsoftappplatform)
> - [Microsoft.Attestation](#microsoftattestation)
> - [Microsoft.Authorization](#microsoftauthorization)
> - [Microsoft.Automation](#microsoftautomation)
> - [Microsoft.Azconfig](#microsoftazconfig)
> - [Microsoft.Azure.Cenevre](#microsoftazuregeneva)
> - [Microsoft.AzureActiveDirectory](#microsoftazureactivedirectory)
> - [Microsoft.AzureVerileri](#microsoftazuredata)
> - [Microsoft.AzureStack](#microsoftazurestack)
> - [Microsoft.Batch](#microsoftbatch)
> - [Microsoft.Faturalandırma](#microsoftbilling)
> - [Microsoft.BingHaritalar](#microsoftbingmaps)
> - [Microsoft.Blockchain](#microsoftblockchain)
> - [Microsoft.BlockchainTokens](#microsoftblockchaintokens)
> - [Microsoft.Blueprint](#microsoftblueprint)
> - [Microsoft.BotService](#microsoftbotservice)
> - [Microsoft.Önbellek](#microsoftcache)
> - [Microsoft.Capacity](#microsoftcapacity)
> - [Microsoft.Cdn](#microsoftcdn)
> - [Microsoft.CertificateRegistration](#microsoftcertificateregistration)
> - [Microsoft.ClassicCompute](#microsoftclassiccompute)
> - [Microsoft.ClassicInfrastructureMigrate](#microsoftclassicinfrastructuremigrate)
> - [Microsoft.ClassicNetwork](#microsoftclassicnetwork)
> - [Microsoft.ClassicStorage](#microsoftclassicstorage)
> - [Microsoft.CognitiveServices](#microsoftcognitiveservices)
> - [Microsoft.Commerce](#microsoftcommerce)
> - [Microsoft.Compute](#microsoftcompute)
> - [Microsoft.Consumption](#microsoftconsumption)
> - [Microsoft.ContainerInstance](#microsoftcontainerinstance)
> - [Microsoft.ContainerRegistry](#microsoftcontainerregistry)
> - [Microsoft.ContainerService](#microsoftcontainerservice)
> - [Microsoft.CostManagement](#microsoftcostmanagement)
> - [Microsoft.CustomerLockbox](#microsoftcustomerlockbox)
> - [Microsoft.CustomProviders](#microsoftcustomproviders)
> - [Microsoft.DataBox](#microsoftdatabox)
> - [Microsoft.DataBoxEdge](#microsoftdataboxedge)
> - [Microsoft.Databricks](#microsoftdatabricks)
> - [Microsoft.DataCatalog](#microsoftdatacatalog)
> - [Microsoft.DataFactory](#microsoftdatafactory)
> - [Microsoft.DataLakeAnalytics](#microsoftdatalakeanalytics)
> - [Microsoft.DataLakeStore](#microsoftdatalakestore)
> - [Microsoft.DataMigration](#microsoftdatamigration)
> - [Microsoft.DataShare](#microsoftdatashare)
> - [Microsoft.DBforMariaDB](#microsoftdbformariadb)
> - [Microsoft.DBforMySQL](#microsoftdbformysql)
> - [Microsoft.DBforPostgreSQL](#microsoftdbforpostgresql)
> - [Microsoft.DeploymentManager](#microsoftdeploymentmanager)
> - [Microsoft.DesktopVirtualization](#microsoftdesktopvirtualization)
> - [Microsoft.Devices](#microsoftdevices)
> - [Microsoft.Devops](#microsoftdevops)
> - [Microsoft.DevSpaces](#microsoftdevspaces)
> - [Microsoft.DevTestLab](#microsoftdevtestlab)
> - [Microsoft.DocumentDB](#microsoftdocumentdb)
> - [Microsoft.DomainRegistration](#microsoftdomainregistration)
> - [Microsoft.DynamicsLcs](#microsoftdynamicslcs)
> - [Microsoft.EnterpriseKnowledgeGraph](#microsoftenterpriseknowledgegraph)
> - [Microsoft.EventGrid](#microsofteventgrid)
> - [Microsoft.EventHub](#microsofteventhub)
> - [Microsoft.Falcon](#microsoftfalcon)
> - [Microsoft.Özellikler](#microsoftfeatures)
> - [Microsoft.Gallery](#microsoftgallery)
> - [Microsoft.Genomik](#microsoftgenomics)
> - [Microsoft.GuestConfiguration](#microsoftguestconfiguration)
> - [Microsoft.HanaOnAzure](#microsofthanaonazure)
> - [Microsoft.HardwareSecurityModules](#microsofthardwaresecuritymodules)
> - [Microsoft.HDInsight](#microsofthdinsight)
> - [Microsoft.HealthcareApis](#microsofthealthcareapis)
> - [Microsoft.HybridCompute](#microsofthybridcompute)
> - [Microsoft.HybridData](#microsofthybriddata)
> - [Microsoft.Hydra](#microsofthydra)
> - [Microsoft.ImportExport](#microsoftimportexport)
> - [Microsoft.Intune](#microsoftintune)
> - [Microsoft.IoTCentral](#microsoftiotcentral)
> - [Microsoft.IoTSpaces](#microsoftiotspaces)
> - [Microsoft.KeyVault](#microsoftkeyvault)
> - [Microsoft.Kubernetes](#microsoftkubernetes)
> - [Microsoft.Kusto](#microsoftkusto)
> - [Microsoft.LabServices](#microsoftlabservices)
> - [Microsoft.Logic](#microsoftlogic)
> - [Microsoft.MachineLearning](#microsoftmachinelearning)
> - [Microsoft.MachineLearningServices](#microsoftmachinelearningservices)
> - [Microsoft.Maintenance](#microsoftmaintenance)
> - [Microsoft.ManagedIdentity](#microsoftmanagedidentity)
> - [Microsoft.ManagedServices](#microsoftmanagedservices)
> - [Microsoft.Management](#microsoftmanagement)
> - [Microsoft.Haritalar](#microsoftmaps)
> - [Microsoft.Marketplace](#microsoftmarketplace)
> - [Microsoft.MarketplaceApps](#microsoftmarketplaceapps)
> - [Microsoft.MarketplaceSipariş](#microsoftmarketplaceordering)
> - [Microsoft.Media](#microsoftmedia)
> - [Microsoft.Microservices4Bahar](#microsoftmicroservices4spring)
> - [Microsoft.Migrate](#microsoftmigrate)
> - [Microsoft.MixedReality](#microsoftmixedreality)
> - [Microsoft.NetApp](#microsoftnetapp)
> - [Microsoft.Network](#microsoftnetwork)
> - [Microsoft.Notebook'lar](#microsoftnotebooks)
> - [Microsoft.NotificationHub'lar](#microsoftnotificationhubs)
> - [Microsoft.ObjectStore](#microsoftobjectstore)
> - [Microsoft.OffAzure](#microsoftoffazure)
> - [Microsoft.OperationalInsights](#microsoftoperationalinsights)
> - [Microsoft.OperationsManagement](#microsoftoperationsmanagement)
> - [Microsoft.Peering](#microsoftpeering)
> - [Microsoft.PolicyInsights](#microsoftpolicyinsights)
> - [Microsoft.Portal](#microsoftportal)
> - [Microsoft.PowerBI](#microsoftpowerbi)
> - [Microsoft.PowerBIDedicated](#microsoftpowerbidedicated)
> - [Microsoft.ProjectBabylon](#microsoftprojectbabylon)
> - [Microsoft.Quantum](#microsoftquantum)
> - [Microsoft.RecoveryServices](#microsoftrecoveryservices)
> - [Microsoft.Relay](#microsoftrelay)
> - [Microsoft.RemoteApp](#microsoftremoteapp)
> - [Microsoft.ResourceGraph](#microsoftresourcegraph)
> - [Microsoft.ResourceHealth](#microsoftresourcehealth)
> - [Microsoft.Resources](#microsoftresources)
> - [Microsoft.SaaS](#microsoftsaas)
> - [Microsoft.Search](#microsoftsearch)
> - [Microsoft.Security](#microsoftsecurity)
> - [Microsoft.SecurityGraph](#microsoftsecuritygraph)
> - [Microsoft.SecurityInsights](#microsoftsecurityinsights)
> - [Microsoft.ServiceBus](#microsoftservicebus)
> - [Microsoft.ServiceFabric](#microsoftservicefabric)
> - [Microsoft.ServiceFabricMesh](#microsoftservicefabricmesh)
> - [Microsoft.Services](#microsoftservices)
> - [Microsoft.SignalRService](#microsoftsignalrservice)
> - [Microsoft.SiteKurtarma](#microsoftsiterecovery)
> - [Microsoft.SoftwarePlan](#microsoftsoftwareplan)
> - [Microsoft.Solutions](#microsoftsolutions)
> - [Microsoft.SpoolService](#microsoftspoolservice)
> - [Microsoft.SQL](#microsoftsql)
> - [Microsoft.SqlVirtualMachine](#microsoftsqlvirtualmachine)
> - [Microsoft.Storage](#microsoftstorage)
> - [Microsoft.StorageÖnbellek](#microsoftstoragecache)
> - [Microsoft.StorageÇoğaltma](#microsoftstoragereplication)
> - [Microsoft.StorageSync](#microsoftstoragesync)
> - [Microsoft.StorageSyncDev](#microsoftstoragesyncdev)
> - [Microsoft.StorageSyncInt](#microsoftstoragesyncint)
> - [Microsoft.StorSimple](#microsoftstorsimple)
> - [Microsoft.StreamAnalytics](#microsoftstreamanalytics)
> - [Microsoft.Abonelik](#microsoftsubscription)
> - [Microsoft.TimeSeriesInsights](#microsofttimeseriesinsights)
> - [Microsoft.VMwareCloudBasit](#microsoftvmwarecloudsimple)
> - [Microsoft.VnfManager](#microsoftvnfmanager)
> - [Microsoft.Web](#microsoftweb)
> - [Microsoft.WindowsDefenderATP](#microsoftwindowsdefenderatp)
> - [Microsoft.WindowsIoT](#microsoftwindowsiot)
> - [Microsoft.WorkloadMonitor](#microsoftworkloadmonitor)

## <a name="microsoftaad"></a>Microsoft.AAD

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporunda etiket |
> | ------------- | ----------- | ----------- |
> | DomainServices | Evet | Evet |
> | DomainServices / oucontainer | Hayır | Hayır |

## <a name="microsoftaddons"></a>Microsoft.Addons

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporunda etiket |
> | ------------- | ----------- | ----------- |
> | destekSağlayıcılar | Hayır | Hayır |

## <a name="microsoftadhybridhealthservice"></a>Microsoft.ADHybridHealthService

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporunda etiket |
> | ------------- | ----------- | ----------- |
> | aadsupportcases | Hayır | Hayır |
> | hizmetler ekler | Hayır | Hayır |
> | Ajan | Hayır | Hayır |
> | anonimapiusers | Hayır | Hayır |
> | yapılandırma | Hayır | Hayır |
> | günlükler | Hayır | Hayır |
> | reports | Hayır | Hayır |
> | servicehealthmetrics | Hayır | Hayır |
> | services | Hayır | Hayır |

## <a name="microsoftadvisor"></a>Microsoft.Advisor

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporunda etiket |
> | ------------- | ----------- | ----------- |
> | Yapılandırma | Hayır | Hayır |
> | öneriler oluşturur | Hayır | Hayır |
> | meta veriler | Hayır | Hayır |
> | Öneriler | Hayır | Hayır |
> | bastırma | Hayır | Hayır |

## <a name="microsoftalertsmanagement"></a>Microsoft.AlertsManagement

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporunda etiket |
> | ------------- | ----------- | ----------- |
> | actionKurallar | Evet | Evet |
> | alerts | Hayır | Hayır |
> | alertsList | Hayır | Hayır |
> | uyarılarıMetaData | Hayır | Hayır |
> | alertsSummary | Hayır | Hayır |
> | alertsSummaryList | Hayır | Hayır |
> | smartDetectorAlertRules | Evet | Evet |
> | smartGroups | Hayır | Hayır |

## <a name="microsoftanalysisservices"></a>Microsoft.AnalysisServices

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporunda etiket |
> | ------------- | ----------- | ----------- |
> | Sunucu | Evet | Evet |

## <a name="microsoftapimanagement"></a>Microsoft.ApiManagement

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporunda etiket |
> | ------------- | ----------- | ----------- |
> | reportFeedback | Hayır | Hayır |
> | hizmet | Evet | Evet |
> | doğrulaServiceName | Hayır | Hayır |

## <a name="microsoftappconfiguration"></a>Microsoft.AppConfiguration

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporunda etiket |
> | ------------- | ----------- | ----------- |
> | configurationMağazalar | Evet | Evet |
> | configurationStores / eventGridFilters | Hayır | Hayır |

## <a name="microsoftappplatform"></a>Microsoft.AppPlatform

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporunda etiket |
> | ------------- | ----------- | ----------- |
> | Spring | Evet | Evet |

## <a name="microsoftattestation"></a>Microsoft.Attestation

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporunda etiket |
> | ------------- | ----------- | ----------- |
> | attestationSağlayıcıları | Hayır | Hayır |

## <a name="microsoftauthorization"></a>Microsoft.Authorization

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporunda etiket |
> | ------------- | ----------- | ----------- |
> | klasikYöneticiler | Hayır | Hayır |
> | dataAliases | Hayır | Hayır |
> | denyAtamaları | Hayır | Hayır |
> | Access'i yükseltme | Hayır | Hayır |
> | findOrphanRoleAssignments | Hayır | Hayır |
> | Kilit | Hayır | Hayır |
> | Izin | Hayır | Hayır |
> | ilkeAtamalar | Hayır | Hayır |
> | policyTanımlar | Hayır | Hayır |
> | politikaSetDefinitions | Hayır | Hayır |
> | sağlayıcıİşlemler | Hayır | Hayır |
> | roleAtamaları | Hayır | Hayır |
> | roleAssignmentsUsageMetrics | Hayır | Hayır |
> | roleDefinitions | Hayır | Hayır |

## <a name="microsoftautomation"></a>Microsoft.Automation

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporunda etiket |
> | ------------- | ----------- | ----------- |
> | otomasyonHesaplar | Evet | Evet |
> | otomasyonHesaplar / yapılandırmalar | Evet | Evet |
> | automationAccounts / jobs | Hayır | Hayır |
> | automationAccounts / privateEndpointConnectionProxies | Hayır | Hayır |
> | automationAccounts / privateEndpointConnections | Hayır | Hayır |
> | automationAccounts / privateLinkResources | Hayır | Hayır |
> | automationAccounts / runbooks | Evet | Evet |
> | automationAccounts / softwareUpdateConfigurations | Hayır | Hayır |
> | automationAccounts / webhooks | Hayır | Hayır |

## <a name="microsoftazconfig"></a>Microsoft.Azconfig

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporunda etiket |
> | ------------- | ----------- | ----------- |
> | configurationMağazalar | Evet | Evet |
> | configurationStores / eventGridFilters | Hayır | Hayır |

## <a name="microsoftazuregeneva"></a>Microsoft.Azure.Cenevre

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporunda etiket |
> | ------------- | ----------- | ----------- |
> | Ortam | Hayır | Hayır |
> | ortamlar / hesaplar | Hayır | Hayır |
> | ortamlar / hesaplar / ad alanları | Hayır | Hayır |
> | ortamlar / hesaplar / ad alanları / yapılandırmalar | Hayır | Hayır |

## <a name="microsoftazureactivedirectory"></a>Microsoft.AzureActiveDirectory

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporunda etiket |
> | ------------- | ----------- | ----------- |
> | b2cDirectories | Evet | Hayır |
> | b2ctenants | Hayır | Hayır |

## <a name="microsoftazuredata"></a>Microsoft.AzureVerileri

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporunda etiket |
> | ------------- | ----------- | ----------- |
> | hibridDataManagers | Evet | Evet |
> | postgresÖrnekleri | Evet | Evet |
> | sqlInstances | Evet | Evet |
> | Sqlserverregistrations | Evet | Evet |
> | sqlServerRegistrations / sqlServers | Hayır | Hayır |

## <a name="microsoftazurestack"></a>Microsoft.AzureStack

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporunda etiket |
> | ------------- | ----------- | ----------- |
> | cloudManifestFiles | Hayır | Hayır |
> | Kayıt | Evet | Evet |
> | kayıtlar / müşteriAbonelikleri | Hayır | Hayır |
> | kayıtlar / ürünler | Hayır | Hayır |

## <a name="microsoftbatch"></a>Microsoft.Batch

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporunda etiket |
> | ------------- | ----------- | ----------- |
> | toplu Hesaplar | Evet | Evet |

## <a name="microsoftbilling"></a>Microsoft.Faturalandırma

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporunda etiket |
> | ------------- | ----------- | ----------- |
> | faturahesapları | Hayır | Hayır |
> | faturahesapları / anlaşmaları | Hayır | Hayır |
> | faturahesapları / faturalamaİzinler | Hayır | Hayır |
> | faturahesapları / faturaprofilleri | Hayır | Hayır |
> | faturahesapları / faturalamaProfiller / faturalamaİzinler | Hayır | Hayır |
> | faturalamaHesapları / faturalamaProfiller / faturalamaRol Atamaları | Hayır | Hayır |
> | faturahesapları / faturalamaProfiller / faturalamaRol Tanımları | Hayır | Hayır |
> | faturahesapları / faturalamaProfiller / faturalamaAbonelikler | Hayır | Hayır |
> | faturahesapları / faturalamaProfiller / createBillingRoleAssignment | Hayır | Hayır |
> | faturahesapları / faturalamaProfiller / müşteriler | Hayır | Hayır |
> | faturahesapları / faturalamaProfiller / talimatlar | Hayır | Hayır |
> | faturahesapları / faturalarProfiller / faturalar | Hayır | Hayır |
> | faturahesapları / faturalamaProfiller / faturalar / fiyat sayfası | Hayır | Hayır |
> | faturahesapları / faturalamaProfiller / faturalar / işlemler | Hayır | Hayır |
> | faturahesapları / faturaprofilleri / faturaBölümleri | Hayır | Hayır |
> | faturahesapları / faturalamaProfiller / faturaBölümler / faturalamaİzinler | Hayır | Hayır |
> | faturalamaHesapları / faturalamaProfiller / faturaBölümler / faturalamaRol Atamaları | Hayır | Hayır |
> | faturahesapları / faturalamaProfiller / faturaBölümler / faturalamaRol Tanımları | Hayır | Hayır |
> | faturahesapları / faturaprofilleri / faturaBölümleri / faturalamaAbonelikler | Hayır | Hayır |
> | faturalamaHesapları / faturaprofilleri / faturaBölümleri / createBillingRoleAssignment | Hayır | Hayır |
> | faturahesapları / faturaprofilleri / faturaBölümler / başlatTransfer | Hayır | Hayır |
> | faturahesapları / faturalamaProfiller / faturaBölümler / ürünler | Hayır | Hayır |
> | faturahesapları / faturalamaProfiller / faturaBölümler / ürünler / transfer | Hayır | Hayır |
> | faturahesapları / faturaprofilleri / faturaBölümler / ürünler / updateAutoRenew | Hayır | Hayır |
> | faturahesapları / faturaprofilleri / faturaBölümleri / işlemleri | Hayır | Hayır |
> | faturahesapları / faturaprofilleri / faturaBölümleri / transferleri | Hayır | Hayır |
> | faturahesapları / Faturaprofilleri / patchİşlemleri | Hayır | Hayır |
> | faturahesapları / faturalamaProfiller / ödeme Yöntemleri | Hayır | Hayır |
> | faturahesapları / faturalamaProfiller / politikalar | Hayır | Hayır |
> | faturahesapları / faturalamaProfiller / fiyat sayfası | Hayır | Hayır |
> | faturahesapları / faturalamaProfiller / pricesheetDownloadOperations | Hayır | Hayır |
> | faturahesapları / faturalamaProfiller / ürünler | Hayır | Hayır |
> | faturahesapları / faturalamaProfiller / işlemler | Hayır | Hayır |
> | faturahesapları / faturalamaRol Atamaları | Hayır | Hayır |
> | faturahesapları / faturalamaRolTanımları | Hayır | Hayır |
> | faturahesapları / faturaabonelikleri | Hayır | Hayır |
> | faturahesapları / faturalarAbonelikler / faturalar | Hayır | Hayır |
> | faturalamaHesapları / createBillingRoleAssignment | Hayır | Hayır |
> | faturahesapları / createInvoiceSectionOperations | Hayır | Hayır |
> | faturahesapları / müşteriler | Hayır | Hayır |
> | faturahesapları / müşteriler / faturalamaİzinler | Hayır | Hayır |
> | faturahesapları / müşteriler / faturaabonelikleri | Hayır | Hayır |
> | faturahesapları / müşteriler / başlatTransfer | Hayır | Hayır |
> | faturahesapları / müşteriler / poliçeler | Hayır | Hayır |
> | faturahesapları / müşteriler / ürünler | Hayır | Hayır |
> | faturahesapları / müşteriler / işlemler | Hayır | Hayır |
> | faturahesapları / müşteriler / transferler | Hayır | Hayır |
> | faturahesapları / bölümleri | Hayır | Hayır |
> | faturalamaHesapları / kayıtHesapları | Hayır | Hayır |
> | faturahesapları / faturaları | Hayır | Hayır |
> | faturahesapları / faturaBölümleri | Hayır | Hayır |
> | faturahesapları / faturaBölümleri / faturalamaAbonelikİşlemleri | Hayır | Hayır |
> | faturahesapları / faturaBölümleri / faturaabonelikleri | Hayır | Hayır |
> | faturahesapları / faturaBölümler / faturalamaAbonelikler / transfer | Hayır | Hayır |
> | faturahesapları / faturaBölümler / yükseltme | Hayır | Hayır |
> | faturahesapları / faturaBölümler / başlatTransfer | Hayır | Hayır |
> | faturahesapları / faturaBölümler / yamaİşlemleri | Hayır | Hayır |
> | faturahesapları / faturaBölümler / productMoveOperations | Hayır | Hayır |
> | faturahesapları / faturaBölümler / ürünler | Hayır | Hayır |
> | faturahesapları / faturaBölümler / ürünler / transfer | Hayır | Hayır |
> | faturahesapları / faturaBölümler / ürünler / updateAutoRenew | Hayır | Hayır |
> | faturahesapları / faturaBölümler / işlemler | Hayır | Hayır |
> | faturahesapları / faturaBölümler / transferler | Hayır | Hayır |
> | faturaHesapları / lineOfCredit | Hayır | Hayır |
> | faturahesapları / yamaİşlemleri | Hayır | Hayır |
> | faturahesapları / ödeme Yöntemleri | Hayır | Hayır |
> | faturahesapları / ürünleri | Hayır | Hayır |
> | faturahesapları / işlemleri | Hayır | Hayır |
> | faturalamaDönemleri | Hayır | Hayır |
> | faturalamaİzinler | Hayır | Hayır |
> | faturalamaEmlak | Hayır | Hayır |
> | faturalandırmaRol Atamaları | Hayır | Hayır |
> | faturalamaRolTanımlar | Hayır | Hayır |
> | createBillingRoleAssignment | Hayır | Hayır |
> | Bölüm | Hayır | Hayır |
> | enrollmentHesapları | Hayır | Hayır |
> | Fatura | Hayır | Hayır |
> | Transfer | Hayır | Hayır |
> | transferler / acceptTransfer | Hayır | Hayır |
> | transferler / düşüşTransfer | Hayır | Hayır |
> | transferler / operationStatus | Hayır | Hayır |
> | transferler / doğrulaTransfer | Hayır | Hayır |
> | doğrulayan Adres | Hayır | Hayır |

## <a name="microsoftbingmaps"></a>Microsoft.BingHaritalar

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporunda etiket |
> | ------------- | ----------- | ----------- |
> | mapApis | Evet | Evet |
> | updateİletişimTercihi | Hayır | Hayır |

## <a name="microsoftblockchain"></a>Microsoft.Blockchain

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporunda etiket |
> | ------------- | ----------- | ----------- |
> | blockchainÜyeler | Evet | Evet |
> | cordaÜyeler | Evet | Evet |
> | Watchers | Evet | Evet |

## <a name="microsoftblockchaintokens"></a>Microsoft.BlockchainTokens

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporunda etiket |
> | ------------- | ----------- | ----------- |
> | TokenServices | Evet | Evet |
> | TokenServices / BlockchainNetworks | Hayır | Hayır |
> | TokenServices / Gruplar | Hayır | Hayır |
> | TokenServices / Gruplar / Hesaplar | Hayır | Hayır |
> | TokenServices / TokenTemplates | Hayır | Hayır |

## <a name="microsoftblueprint"></a>Microsoft.Blueprint

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporunda etiket |
> | ------------- | ----------- | ----------- |
> | blueprintAtamaları | Hayır | Hayır |
> | blueprintAtamaları / atamaİşlemleri | Hayır | Hayır |
> | blueprintAtamaları / işlemleri | Hayır | Hayır |
> | Plan | Hayır | Hayır |
> | planlar / eserler | Hayır | Hayır |
> | planları / sürümleri | Hayır | Hayır |
> | planları / sürümleri / eserler | Hayır | Hayır |

## <a name="microsoftbotservice"></a>Microsoft.BotService

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporunda etiket |
> | ------------- | ----------- | ----------- |
> | botServices | Evet | Evet |
> | botServices / kanallar | Hayır | Hayır |
> | botServices / bağlantılar | Hayır | Hayır |
> | diller | Hayır | Hayır |
> | templates | Hayır | Hayır |

## <a name="microsoftcache"></a>Microsoft.Önbellek

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporunda etiket |
> | ------------- | ----------- | ----------- |
> | Redis | Evet | Evet |

## <a name="microsoftcapacity"></a>Microsoft.Capacity

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporunda etiket |
> | ------------- | ----------- | ----------- |
> | uygulamalı Rezervasyonlar | Hayır | Hayır |
> | autoQuotaIncrease | Hayır | Hayır |
> | calculateExchange | Hayır | Hayır |
> | hesaplamaFiyatı | Hayır | Hayır |
> | hesaplamaPurchasePrice | Hayır | Hayır |
> | Katalog | Hayır | Hayır |
> | commercialReservationOrders | Hayır | Hayır |
> | Exchange | Hayır | Hayır |
> | placePurchaseOrder | Hayır | Hayır |
> | rezervasyonSiparişler | Hayır | Hayır |
> | rezervasyonSiparişler / calculateRefund | Hayır | Hayır |
> | reservationSiparişler / birleştirme | Hayır | Hayır |
> | rezervasyonSiparişler / rezervasyonlar | Hayır | Hayır |
> | rezervasyonSiparişler / rezervasyonlar / revizyonlar | Hayır | Hayır |
> | reservationSiparişler / iade | Hayır | Hayır |
> | reservationOrders / split | Hayır | Hayır |
> | reservationSiparişler / takas | Hayır | Hayır |
> | Rezervasyonlar | Hayır | Hayır |
> | kaynakSağlayıcılar | Hayır | Hayır |
> | kaynaklar | Hayır | Hayır |
> | doğrulamaReservationOrder | Hayır | Hayır |

## <a name="microsoftcdn"></a>Microsoft.Cdn

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporunda etiket |
> | ------------- | ----------- | ----------- |
> | CdnWebApplicationFirewallManagedRuleSets | Hayır | Hayır |
> | CdnWebApplicationFirewallPolicies | Evet | Evet |
> | kenar düğümleri | Hayır | Hayır |
> | Profil | Evet | Evet |
> | profiller / uç noktalar | Evet | Evet |
> | profiller / uç noktalar / özel etki alanları | Hayır | Hayır |
> | profiller / uç noktalar / origingroups | Hayır | Hayır |
> | profiller / uç noktalar / origins | Hayır | Hayır |
> | validateProbe | Hayır | Hayır |

## <a name="microsoftcertificateregistration"></a>Microsoft.CertificateRegistration

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporunda etiket |
> | ------------- | ----------- | ----------- |
> | sertifikaSiparişler | Evet | Evet |
> | sertifikaSiparişler / sertifikalar | Hayır | Hayır |
> | doğrulamaSertifikaRegistrationInformation | Hayır | Hayır |

## <a name="microsoftclassiccompute"></a>Microsoft.ClassicCompute

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporunda etiket |
> | ------------- | ----------- | ----------- |
> | Yetenek -lerini | Hayır | Hayır |
> | alan Adları | Hayır | Hayır |
> | domainNames / yetenekleri | Hayır | Hayır |
> | domainNames / internalLoadBalancers | Hayır | Hayır |
> | domainNames / serviceCertificates | Hayır | Hayır |
> | alan Adları / yuvaları | Hayır | Hayır |
> | alan Adları / yuvaları / rolleri | Hayır | Hayır |
> | alan Adları / yuvalar / roller / metricDefinitions | Hayır | Hayır |
> | alan Adları / yuvalar / roller / ölçümler | Hayır | Hayır |
> | moveSubscriptionResources | Hayır | Hayır |
> | işletimSistemiAileler | Hayır | Hayır |
> | işletim Sistemleri | Hayır | Hayır |
> | quotas | Hayır | Hayır |
> | resourceTypes | Hayır | Hayır |
> | doğrulamaAbonelikMoveAvailability | Hayır | Hayır |
> | virtualMachines | Hayır | Hayır |
> | virtualMachines / diagnosticSettings | Hayır | Hayır |
> | virtualMachines / metricDefinitions | Hayır | Hayır |
> | virtualMachines / ölçümler | Hayır | Hayır |

## <a name="microsoftclassicinfrastructuremigrate"></a>Microsoft.ClassicInfrastructureMigrate

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporunda etiket |
> | ------------- | ----------- | ----------- |
> | klasikAltyapıKaynakları | Hayır | Hayır |

## <a name="microsoftclassicnetwork"></a>Microsoft.ClassicNetwork

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporunda etiket |
> | ------------- | ----------- | ----------- |
> | Yetenek -lerini | Hayır | Hayır |
> | expressRouteCrossConnections | Hayır | Hayır |
> | expressRouteCrossConnections / peerings | Hayır | Hayır |
> | ağ geçidiDesteklenen Cihazlar | Hayır | Hayır |
> | ağSecurityGroups | Hayır | Hayır |
> | quotas | Hayır | Hayır |
> | ayrılmışIps | Hayır | Hayır |
> | sanalAğlar | Hayır | Hayır |
> | virtualNetworks / remoteVirtualNetworkPeeringProxies | Hayır | Hayır |
> | virtualNetworks / virtualNetworkPeerings | Hayır | Hayır |

## <a name="microsoftclassicstorage"></a>Microsoft.ClassicStorage

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporunda etiket |
> | ------------- | ----------- | ----------- |
> | Yetenek -lerini | Hayır | Hayır |
> | Disk | Hayır | Hayır |
> | images | Hayır | Hayır |
> | osImages | Hayır | Hayır |
> | osPlatformImages | Hayır | Hayır |
> | publicImages | Hayır | Hayır |
> | quotas | Hayır | Hayır |
> | depolamaHesapları | Hayır | Hayır |
> | storageAccounts / blobServices | Hayır | Hayır |
> | storageAccounts / fileServices | Hayır | Hayır |
> | storageAccounts / metricDefinitions | Hayır | Hayır |
> | storageAccounts / ölçümler | Hayır | Hayır |
> | storageAccounts / queueServices | Hayır | Hayır |
> | storageAccounts / hizmetleri | Hayır | Hayır |
> | storageAccounts / services / diagnosticAyarlar | Hayır | Hayır |
> | storageAccounts / hizmetler / metricDefinitions | Hayır | Hayır |
> | storageAccounts / hizmetler / ölçümler | Hayır | Hayır |
> | storageAccounts / tableServices | Hayır | Hayır |
> | storageAccounts / vmImages | Hayır | Hayır |
> | vmImages | Hayır | Hayır |

## <a name="microsoftcognitiveservices"></a>Microsoft.CognitiveServices

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporunda etiket |
> | ------------- | ----------- | ----------- |
> | accounts | Evet | Evet |

## <a name="microsoftcommerce"></a>Microsoft.Commerce

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporunda etiket |
> | ------------- | ----------- | ----------- |
> | RateCard | Hayır | Hayır |
> | KullanımA Göre Toplamlar | Hayır | Hayır |

## <a name="microsoftcompute"></a>Microsoft.Compute

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporunda etiket |
> | ------------- | ----------- | ----------- |
> | availabilitySets | Evet | Evet |
> | diskŞifreleme Setleri | Evet | Evet |
> | Disk | Evet | Evet |
> | Galerileri | Evet | Evet |
> | galeriler / uygulamalar | Hayır | Hayır |
> | galeriler / uygulamalar / sürümler | Hayır | Hayır |
> | galeriler / resimler | Hayır | Hayır |
> | galeriler / resimler / sürümler | Hayır | Hayır |
> | hostGroups | Evet | Evet |
> | hostGroups / hosts | Evet | Evet |
> | images | Evet | Evet |
> | yakınlıkPlacementGroups | Evet | Evet |
> | restorePointCollections | Evet | Evet |
> | restorePointCollections / restorePoints | Hayır | Hayır |
> | paylaşılanVMExtensions | Evet | Evet |
> | sharedVMExtensions / sürümleri | Hayır | Hayır |
> | paylaşılanVMImages | Evet | Evet |
> | paylaşılanVMImages / sürümleri | Hayır | Hayır |
> | anlık görüntüler | Evet | Evet |
> | sshPublicKeys | Evet | Evet |
> | virtualMachines | Evet | Evet |
> | virtualMachines / uzantıları | Evet | Evet |
> | virtualMachines / metricDefinitions | Hayır | Hayır |
> | virtualMachineScaleSets | Evet | Evet |
> | virtualMachineScaleSets / uzantıları | Hayır | Hayır |
> | virtualMachineScaleSets / ağArayüzler | Hayır | Hayır |
> | virtualMachineScaleSets / publicIPAddresses | Hayır | Hayır |
> | virtualMachineScaleSets / virtualMachines | Hayır | Hayır |
> | virtualMachineScaleSets / virtualMachines / networkInterfaces | Hayır | Hayır |

## <a name="microsoftconsumption"></a>Microsoft.Consumption

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporunda etiket |
> | ------------- | ----------- | ----------- |
> | Toplu Maliyet | Hayır | Hayır |
> | Bakiyeler | Hayır | Hayır |
> | Bütçeler | Hayır | Hayır |
> | Ücretler | Hayır | Hayır |
> | MaliyetEtiketleri | Hayır | Hayır |
> | Kredi | Hayır | Hayır |
> | etkinlikler | Hayır | Hayır |
> | Tahminler | Hayır | Hayır |
> | Çok | Hayır | Hayır |
> | Pazaryerleri | Hayır | Hayır |
> | Fiyat listeleri | Hayır | Hayır |
> | Ürünler | Hayır | Hayır |
> | Rezervasyon Detayları | Hayır | Hayır |
> | Rezervasyon Önerileri | Hayır | Hayır |
> | Rezervasyon Özetleri | Hayır | Hayır |
> | Rezervasyon İşlemleri | Hayır | Hayır |
> | Etiketler | Hayır | Hayır |
> | Kiracı | Hayır | Hayır |
> | Koşullar | Hayır | Hayır |
> | Kullanım Detayları | Hayır | Hayır |

## <a name="microsoftcontainerinstance"></a>Microsoft.ContainerInstance

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporunda etiket |
> | ------------- | ----------- | ----------- |
> | konteynerGruplar | Evet | Evet |
> | serviceAssociationLinks | Hayır | Hayır |

## <a name="microsoftcontainerregistry"></a>Microsoft.ContainerRegistry

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporunda etiket |
> | ------------- | ----------- | ----------- |
> | Kayıt | Evet | Evet |
> | kayıt defterleri / agentPools | Evet | Evet |
> | kayıt defterleri / oluşturur | Hayır | Hayır |
> | kayıt defterleri / oluşturur / iptal | Hayır | Hayır |
> | kayıt defteri / oluşturur / getLogLink | Hayır | Hayır |
> | kayıt defterleri / buildTasks | Evet | Evet |
> | kayıt defterleri / buildGörevler / adımlar | Hayır | Hayır |
> | kayıt defterleri / eventGridFilters | Hayır | Hayır |
> | kayıt defterleri / oluşturmak Kimlik Bilgileri | Hayır | Hayır |
> | kayıt defteri / getBuildSourceUploadUrl | Hayır | Hayır |
> | kayıt defterleri / GetCredentials | Hayır | Hayır |
> | kayıt defteri / importImage | Hayır | Hayır |
> | kayıt defterleri / privateEndpointConnectionProxies | Hayır | Hayır |
> | kayıt defterleri / privateEndpointConnectionProxies / doğrulamak | Hayır | Hayır |
> | kayıt defterleri / privateEndpointConnections | Hayır | Hayır |
> | kayıt defterleri / privateLinkResources | Hayır | Hayır |
> | kayıt defterleri / queueYapı | Hayır | Hayır |
> | kayıt defteri / rejenereCredential | Hayır | Hayır |
> | kayıt defterleri / rejenere Credentials | Hayır | Hayır |
> | kayıt defterleri / çoğaltmalar | Evet | Evet |
> | kayıt defterleri / çalışır | Hayır | Hayır |
> | kayıt defterleri / çalışır / iptal | Hayır | Hayır |
> | kayıt defteri / programRun | Hayır | Hayır |
> | kayıt defteri / scopeMaps | Hayır | Hayır |
> | kayıt defterleri / taskRuns | Evet | Evet |
> | kayıt defterleri / görevleri | Evet | Evet |
> | kayıt defterleri / belirteçleri | Hayır | Hayır |
> | kayıt defterleri / updatePolitikalar | Hayır | Hayır |
> | kayıt defterleri / webhooks | Evet | Evet |
> | kayıt defteri / webhooks / getCallbackConfig | Hayır | Hayır |
> | kayıt defteri / webhooks / ping | Hayır | Hayır |

## <a name="microsoftcontainerservice"></a>Microsoft.ContainerService

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporunda etiket |
> | ------------- | ----------- | ----------- |
> | konteynerHizmetleri | Evet | Evet |
> | yönetilen Kümeler | Evet | Evet |
> | openShiftManagedClusters | Evet | Evet |

## <a name="microsoftcostmanagement"></a>Microsoft.CostManagement

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporunda etiket |
> | ------------- | ----------- | ----------- |
> | Uyarılar | Hayır | Hayır |
> | Faturahesapları | Hayır | Hayır |
> | Bütçeler | Hayır | Hayır |
> | CloudConnectors | Hayır | Hayır |
> | Bağlayıcılar | Evet | Evet |
> | Departmanlar | Hayır | Hayır |
> | Boyutlar | Hayır | Hayır |
> | Kayıt Hesapları | Hayır | Hayır |
> | Ihracat | Hayır | Hayır |
> | Dış Fatura hesapları | Hayır | Hayır |
> | Dış Faturahesapları / Uyarılar | Hayır | Hayır |
> | Dış Faturahesapları / Boyutlar | Hayır | Hayır |
> | Dış Faturahesapları / Tahmin | Hayır | Hayır |
> | Dış Fatura hesapları / Sorgu | Hayır | Hayır |
> | Harici Abonelikler | Hayır | Hayır |
> | Harici Abonelikler / Uyarılar | Hayır | Hayır |
> | Harici Abonelikler / Boyutlar | Hayır | Hayır |
> | ExternalSubscriptions / Tahmin | Hayır | Hayır |
> | Harici Abonelikler / Sorgu | Hayır | Hayır |
> | Tahmin | Hayır | Hayır |
> | Sorgu | Hayır | Hayır |
> | register | Hayır | Hayır |
> | Reportconfigs | Hayır | Hayır |
> | Raporlar | Hayır | Hayır |
> | Ayarlar | Hayır | Hayır |
> | showbackKurallar | Hayır | Hayır |
> | Görünümler | Hayır | Hayır |

## <a name="microsoftcustomerlockbox"></a>Microsoft.CustomerLockbox

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporunda etiket |
> | ------------- | ----------- | ----------- |
> | Istek | Hayır | Hayır |

## <a name="microsoftcustomproviders"></a>Microsoft.CustomProviders

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporunda etiket |
> | ------------- | ----------- | ----------- |
> | Dernek | Hayır | Hayır |
> | kaynakSağlayıcılar | Evet | Evet |

## <a name="microsoftdatabox"></a>Microsoft.DataBox

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporunda etiket |
> | ------------- | ----------- | ----------- |
> | Işleri | Evet | Evet |

## <a name="microsoftdataboxedge"></a>Microsoft.DataBoxEdge

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporunda etiket |
> | ------------- | ----------- | ----------- |
> | DataBoxEdgeCihazlar | Evet | Evet |

## <a name="microsoftdatabricks"></a>Microsoft.Databricks

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporunda etiket |
> | ------------- | ----------- | ----------- |
> | çalışma alanı | Evet | Hayır |
> | çalışma alanları / dbWorkspaces | Hayır | Hayır |
> | çalışma alanları / depolamaŞifreleme | Hayır | Hayır |
> | çalışma alanları / virtualNetworkPeerings | Hayır | Hayır |

## <a name="microsoftdatacatalog"></a>Microsoft.DataCatalog

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporunda etiket |
> | ------------- | ----------- | ----------- |
> | Katalog | Evet | Evet |
> | veri katalogları | Evet | Evet |
> | veri katalogları / veri kaynakları | Hayır | Hayır |
> | datacatalogs / datasources / taramaları | Hayır | Hayır |
> | datacatalogs / datasources / taramaları / veri kümeleri | Hayır | Hayır |
> | datacatalogs / datasources / taramaları / tetikler | Hayır | Hayır |

## <a name="microsoftdatafactory"></a>Microsoft.DataFactory

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporunda etiket |
> | ------------- | ----------- | ----------- |
> | dataFabrikalar | Evet | Hayır |
> | dataFactorys / diagnosticSettings | Hayır | Hayır |
> | dataFabrikalar / metricDefinitions | Hayır | Hayır |
> | dataFactorySchema | Hayır | Hayır |
> | Fabrika | Evet | Hayır |
> | fabrikalar / integrationRuntimes | Hayır | Hayır |

## <a name="microsoftdatalakeanalytics"></a>Microsoft.DataLakeAnalytics

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporunda etiket |
> | ------------- | ----------- | ----------- |
> | accounts | Evet | Evet |
> | hesaplar / dataLakeStoreAccounts | Hayır | Hayır |
> | hesaplar / depolamaHesapları | Hayır | Hayır |
> | hesaplar / depolamaHesapları / konteynerler | Hayır | Hayır |
> | hesaplar / transferAnalyticsUnits | Hayır | Hayır |

## <a name="microsoftdatalakestore"></a>Microsoft.DataLakeStore

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporunda etiket |
> | ------------- | ----------- | ----------- |
> | accounts | Evet | Evet |
> | hesaplar / eventGridFilters | Hayır | Hayır |
> | hesaplar / firewallRules | Hayır | Hayır |

## <a name="microsoftdatamigration"></a>Microsoft.DataMigration

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporunda etiket |
> | ------------- | ----------- | ----------- |
> | services | Hayır | Hayır |
> | hizmetler / projeler | Hayır | Hayır |

## <a name="microsoftdatashare"></a>Microsoft.DataShare

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporunda etiket |
> | ------------- | ----------- | ----------- |
> | accounts | Evet | Evet |
> | hesaplar / hisse | Hayır | Hayır |
> | hesaplar / paylaşımlar / veri kümeleri | Hayır | Hayır |
> | hesaplar / hisse / davetiyeler | Hayır | Hayır |
> | hesaplar / hisse / providersharesubscriptions | Hayır | Hayır |
> | hesaplar / paylaşımlar / senkronizasyonAyarları | Hayır | Hayır |
> | hesaplar / sharesubscriptions | Hayır | Hayır |
> | hesaplar / sharesubscriptions / consumerSourceDataSets | Hayır | Hayır |
> | hesaplar / sharesubscriptions / datasetmappings | Hayır | Hayır |
> | hesaplar / sharesubscriptions / tetikleyiciler | Hayır | Hayır |

## <a name="microsoftdbformariadb"></a>Microsoft.DBforMariaDB

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporunda etiket |
> | ------------- | ----------- | ----------- |
> | Sunucu | Evet | Evet |
> | sunucular / danışmanlar | Hayır | Hayır |
> | sunucular / anahtarlar | Hayır | Hayır |
> | sunucular / privateEndpointConnectionProxies | Hayır | Hayır |
> | sunucular / privateEndpointConnections | Hayır | Hayır |
> | sunucular / privateLinkResources | Hayır | Hayır |
> | sunucular / queryTexts | Hayır | Hayır |
> | sunucular / recoverableServers | Hayır | Hayır |
> | sunucular / topQueryStatistics | Hayır | Hayır |
> | sunucular / virtualNetworkRules | Hayır | Hayır |
> | sunucular / waitStatistics | Hayır | Hayır |

## <a name="microsoftdbformysql"></a>Microsoft.DBforMySQL

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporunda etiket |
> | ------------- | ----------- | ----------- |
> | Sunucu | Evet | Evet |
> | sunucular / danışmanlar | Hayır | Hayır |
> | sunucular / anahtarlar | Hayır | Hayır |
> | sunucular / privateEndpointConnectionProxies | Hayır | Hayır |
> | sunucular / privateEndpointConnections | Hayır | Hayır |
> | sunucular / privateLinkResources | Hayır | Hayır |
> | sunucular / queryTexts | Hayır | Hayır |
> | sunucular / recoverableServers | Hayır | Hayır |
> | sunucular / topQueryStatistics | Hayır | Hayır |
> | sunucular / virtualNetworkRules | Hayır | Hayır |
> | sunucular / waitStatistics | Hayır | Hayır |

## <a name="microsoftdbforpostgresql"></a>Microsoft.DBforPostgreSQL

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporunda etiket |
> | ------------- | ----------- | ----------- |
> | Servergroups | Evet | Evet |
> | Sunucu | Evet | Evet |
> | sunucular / danışmanlar | Hayır | Hayır |
> | sunucular / anahtarlar | Hayır | Hayır |
> | sunucular / privateEndpointConnectionProxies | Hayır | Hayır |
> | sunucular / privateEndpointConnections | Hayır | Hayır |
> | sunucular / privateLinkResources | Hayır | Hayır |
> | sunucular / queryTexts | Hayır | Hayır |
> | sunucular / recoverableServers | Hayır | Hayır |
> | sunucular / topQueryStatistics | Hayır | Hayır |
> | sunucular / virtualNetworkRules | Hayır | Hayır |
> | sunucular / waitStatistics | Hayır | Hayır |
> | sunucularv2 | Evet | Evet |
> | singleServers | Evet | Evet |

## <a name="microsoftdeploymentmanager"></a>Microsoft.DeploymentManager

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporunda etiket |
> | ------------- | ----------- | ----------- |
> | artifactSources | Evet | Evet |
> | kullanıma sunulması | Evet | Evet |
> | hizmetTopçular | Evet | Evet |
> | serviceTopolojiler / hizmetler | Evet | Evet |
> | serviceTopolojiler / hizmetler / serviceUnits | Evet | Evet |
> | adımlar | Evet | Evet |

## <a name="microsoftdesktopvirtualization"></a>Microsoft.DesktopVirtualization

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporunda etiket |
> | ------------- | ----------- | ----------- |
> | uygulama grupları | Evet | Evet |
> | uygulama grupları / uygulamalar | Hayır | Hayır |
> | uygulama grupları / masaüstü bilgisayarlar | Hayır | Hayır |
> | uygulama grupları / startmenuitems | Hayır | Hayır |
> | hostpools | Evet | Evet |
> | hostpools / sessionhosts | Hayır | Hayır |
> | hostpools / sessionhosts / usersessions | Hayır | Hayır |
> | hostpools / usersessions | Hayır | Hayır |
> | çalışma alanı | Evet | Evet |

## <a name="microsoftdevices"></a>Microsoft.Devices

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporunda etiket |
> | ------------- | ----------- | ----------- |
> | Elastik Havuzlar | Evet | Evet |
> | ElasticPools / IotHubTenants | Evet | Evet |
> | ElasticPools / IotHubTenants / securityAyarlar | Hayır | Hayır |
> | IotHub'lar | Evet | Evet |
> | IotHubs / olayGridFilters | Hayır | Hayır |
> | IotHubs / securityAyarlar | Hayır | Hayır |
> | Tedarik Hizmetleri | Evet | Evet |
> | Kullanım | Hayır | Hayır |

## <a name="microsoftdevops"></a>Microsoft.Devops

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporunda etiket |
> | ------------- | ----------- | ----------- |
> | Boru hattı | Evet | Evet |

## <a name="microsoftdevspaces"></a>Microsoft.DevSpaces

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporunda etiket |
> | ------------- | ----------- | ----------- |
> | denetleyiciler | Evet | Evet |

## <a name="microsoftdevtestlab"></a>Microsoft.DevTestLab

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporunda etiket |
> | ------------- | ----------- | ----------- |
> | laboratuvar merkezleri | Evet | Evet |
> | Labs | Evet | Evet |
> | laboratuvarlar / ortamlar | Evet | Evet |
> | laboratuvarlar / serviceRunners | Evet | Evet |
> | laboratuvarlar / virtualMachines | Evet | Evet |
> | Zamanlama | Evet | Evet |

## <a name="microsoftdocumentdb"></a>Microsoft.DocumentDB

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporunda etiket |
> | ------------- | ----------- | ----------- |
> | veritabanıHesap Adları | Hayır | Hayır |
> | veritabanıHesapları | Evet | Evet |

## <a name="microsoftdomainregistration"></a>Microsoft.DomainRegistration

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporunda etiket |
> | ------------- | ----------- | ----------- |
> | Etki alanları | Evet | Evet |
> | etki alanları / domainOwnershipIdentifiers | Hayır | Hayır |
> | oluştururSsoRequest | Hayır | Hayır |
> | topLevelDomains | Hayır | Hayır |
> | doğrulanmasıDomainRegistrationInformation | Hayır | Hayır |

## <a name="microsoftdynamicslcs"></a>Microsoft.DynamicsLcs

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporunda etiket |
> | ------------- | ----------- | ----------- |
> | lcsprojects | Hayır | Hayır |
> | lcsprojects / clouddeployments | Hayır | Hayır |
> | lcsprojects / konektörler | Hayır | Hayır |

## <a name="microsoftenterpriseknowledgegraph"></a>Microsoft.EnterpriseKnowledgeGraph

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporunda etiket |
> | ------------- | ----------- | ----------- |
> | services | Evet | Evet |

## <a name="microsofteventgrid"></a>Microsoft.EventGrid

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporunda etiket |
> | ------------- | ----------- | ----------- |
> | Etki alanları | Evet | Evet |
> | etki alanları / konular | Hayır | Hayır |
> | eventAbonelikler | Hayır | Hayır |
> | uzantıKonular | Hayır | Hayır |
> | partnerNamespaces | Evet | Evet |
> | partnerNamespaces / eventChannels | Hayır | Hayır |
> | partnerKayıtlar | Evet | Evet |
> | partnerKonular | Evet | Evet |
> | partnerKonular / eventSubscriptions | Hayır | Hayır |
> | systemTopics | Evet | Evet |
> | systemKonular / eventSubscriptions | Hayır | Hayır |
> | konuları | Evet | Evet |
> | topicTypes | Hayır | Hayır |

## <a name="microsofteventhub"></a>Microsoft.EventHub

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporunda etiket |
> | ------------- | ----------- | ----------- |
> | Küme | Evet | Evet |
> | Ad alanları | Evet | Evet |
> | namespaces / yetkilendirme kuralları | Hayır | Hayır |
> | ad alanları / disasterrecoveryconfigs | Hayır | Hayır |
> | ad alanları / eventhubs | Hayır | Hayır |
> | ad alanları / eventhubs / yetkilendirme kuralları | Hayır | Hayır |
> | ad alanları / eventhubs / tüketici grupları | Hayır | Hayır |
> | ad alanları / networkrulesets | Hayır | Hayır |

## <a name="microsoftfalcon"></a>Microsoft.Falcon

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporunda etiket |
> | ------------- | ----------- | ----------- |
> | Ad alanları | Evet | Evet |

## <a name="microsoftfeatures"></a>Microsoft.Özellikler

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporunda etiket |
> | ------------- | ----------- | ----------- |
> | özellikSağlayıcılar | Hayır | Hayır |
> | özellikler | Hayır | Hayır |
> | Sağlayıcı | Hayır | Hayır |
> | abonelikÖzellik Kayıtları | Hayır | Hayır |

## <a name="microsoftgallery"></a>Microsoft.Gallery

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporunda etiket |
> | ------------- | ----------- | ----------- |
> | Kayıt | Hayır | Hayır |
> | galeri öğeleri | Hayır | Hayır |
> | artifactaccessuri oluşturmak | Hayır | Hayır |
> | myareas | Hayır | Hayır |
> | myareas / alanları | Hayır | Hayır |
> | myareas / alanlar / alanlar | Hayır | Hayır |
> | myareas / alanlar / alanlar / galeri öğeleri | Hayır | Hayır |
> | myareas / alanlar / galeri öğeleri | Hayır | Hayır |
> | myareas / galeri öğeleri | Hayır | Hayır |
> | register | Hayır | Hayır |
> | kaynaklar | Hayır | Hayır |
> | retrieveresourcesbyid | Hayır | Hayır |

## <a name="microsoftgenomics"></a>Microsoft.Genomik

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporunda etiket |
> | ------------- | ----------- | ----------- |
> | accounts | Evet | Evet |

## <a name="microsoftguestconfiguration"></a>Microsoft.GuestConfiguration

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporunda etiket |
> | ------------- | ----------- | ----------- |
> | otomatikYönetilen Hesaplar | Evet | Evet |
> | otomatikManagedVmConfigurationProfiles | Evet | Evet |
> | configurationProfileAtamaları | Hayır | Hayır |
> | guestConfigurationAssignments | Hayır | Hayır |
> | Yazılım | Hayır | Hayır |
> | softwareUpdateProfile | Hayır | Hayır |
> | softwareUpdates | Hayır | Hayır |

## <a name="microsofthanaonazure"></a>Microsoft.HanaOnAzure

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporunda etiket |
> | ------------- | ----------- | ----------- |
> | hanaInstances | Evet | Evet |
> | sapMonitörler | Evet | Evet |

## <a name="microsofthardwaresecuritymodules"></a>Microsoft.HardwareSecurityModules

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporunda etiket |
> | ------------- | ----------- | ----------- |
> | adanmışHSMs | Evet | Evet |

## <a name="microsofthdinsight"></a>Microsoft.HDInsight

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporunda etiket |
> | ------------- | ----------- | ----------- |
> | Küme | Evet | Evet |
> | kümeler / uygulamalar | Hayır | Hayır |

## <a name="microsofthealthcareapis"></a>Microsoft.HealthcareApis

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporunda etiket |
> | ------------- | ----------- | ----------- |
> | services | Evet | Evet |

## <a name="microsofthybridcompute"></a>Microsoft.HybridCompute

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporunda etiket |
> | ------------- | ----------- | ----------- |
> | Makine | Evet | Evet |
> | makineler / uzantılar | Evet | Evet |

## <a name="microsofthybriddata"></a>Microsoft.HybridData

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporunda etiket |
> | ------------- | ----------- | ----------- |
> | dataYöneticiler | Evet | Evet |

## <a name="microsofthydra"></a>Microsoft.Hydra

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporunda etiket |
> | ------------- | ----------- | ----------- |
> | Bileşen | Evet | Evet |
> | ağScopes | Evet | Evet |

## <a name="microsoftimportexport"></a>Microsoft.ImportExport

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporunda etiket |
> | ------------- | ----------- | ----------- |
> | Işleri | Evet | Evet |

## <a name="microsoftintune"></a>Microsoft.Intune

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporunda etiket |
> | ------------- | ----------- | ----------- |
> | tanıAyarları | Hayır | Hayır |
> | diagnosticSettingsCategories | Hayır | Hayır |

## <a name="microsoftiotcentral"></a>Microsoft.IoTCentral

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporunda etiket |
> | ------------- | ----------- | ----------- |
> | appTemplates | Hayır | Hayır |
> | IoTApps | Evet | Evet |

## <a name="microsoftiotspaces"></a>Microsoft.IoTSpaces

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporunda etiket |
> | ------------- | ----------- | ----------- |
> | Graf | Evet | Evet |

## <a name="microsoftkeyvault"></a>Microsoft.KeyVault

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporunda etiket |
> | ------------- | ----------- | ----------- |
> | silinmiş Vaults | Hayır | Hayır |
> | hsmPools | Evet | Evet |
> | Tonoz | Evet | Evet |
> | tonozlar / erişimPolitikaları | Hayır | Hayır |
> | kasalar / olayGridFilters | Hayır | Hayır |
> | kasalar / sırlar | Hayır | Hayır |

## <a name="microsoftkubernetes"></a>Microsoft.Kubernetes

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporunda etiket |
> | ------------- | ----------- | ----------- |
> | bağlıKümeler | Evet | Evet |

## <a name="microsoftkusto"></a>Microsoft.Kusto

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporunda etiket |
> | ------------- | ----------- | ----------- |
> | Küme | Evet | Evet |
> | kümeler / ekli veritabanı yapılandırmaları | Hayır | Hayır |
> | kümeler / veritabanları | Hayır | Hayır |
> | kümeleri / veritabanları / veri bağlantıları | Hayır | Hayır |
> | kümeleri / veritabanları / eventhubconnections | Hayır | Hayır |
> | kümeleri / veritabanları / ilkeler atamaları | Hayır | Hayır |
> | kümeler / veri bağlantıları | Hayır | Hayır |
> | kümeler / ilkeler atamaları | Hayır | Hayır |
> | kümeler / paylaşılan kimlikler | Hayır | Hayır |

## <a name="microsoftlabservices"></a>Microsoft.LabServices

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporunda etiket |
> | ------------- | ----------- | ----------- |
> | laboratuvar hesapları | Evet | Evet |
> | kullanıcılar | Hayır | Hayır |

## <a name="microsoftlogic"></a>Microsoft.Logic

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporunda etiket |
> | ------------- | ----------- | ----------- |
> | hostingOrtamlar | Evet | Evet |
> | entegrasyonHesapları | Evet | Evet |
> | entegrasyonServiceOrtamlar | Evet | Evet |
> | entegrasyonServiceEnvironments / managedApis | Evet | Evet |
> | izole Ortamlar | Evet | Evet |
> | Iş akışı | Evet | Evet |

## <a name="microsoftmachinelearning"></a>Microsoft.MachineLearning

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporunda etiket |
> | ------------- | ----------- | ----------- |
> | taahhütPlanlar | Evet | Evet |
> | Webservices | Evet | Evet |
> | Çalışma Alanları | Evet | Evet |

## <a name="microsoftmachinelearningservices"></a>Microsoft.MachineLearningServices

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporunda etiket |
> | ------------- | ----------- | ----------- |
> | çalışma alanı | Evet | Evet |
> | çalışma alanları / hesaplamalar | Hayır | Hayır |
> | çalışma alanları / eventGridFilters | Hayır | Hayır |

## <a name="microsoftmaintenance"></a>Microsoft.Maintenance

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporunda etiket |
> | ------------- | ----------- | ----------- |
> | uygulamaGüncellemeler | Hayır | Hayır |
> | yapılandırmaAtamaları | Hayır | Hayır |
> | maintenanceYapılandırmalar | Evet | Evet |
> | güncelleştirmeler | Hayır | Hayır |

## <a name="microsoftmanagedidentity"></a>Microsoft.ManagedIdentity

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporunda etiket |
> | ------------- | ----------- | ----------- |
> | Kimlik | Hayır | Hayır |
> | kullanıcıAssignedIdentities | Evet | Evet |

## <a name="microsoftmanagedservices"></a>Microsoft.ManagedServices

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporunda etiket |
> | ------------- | ----------- | ----------- |
> | marketplaceRegistrationDefinitions | Hayır | Hayır |
> | kayıtAtamaları | Hayır | Hayır |
> | kayıtTanımlar | Hayır | Hayır |

## <a name="microsoftmanagement"></a>Microsoft.Management

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporunda etiket |
> | ------------- | ----------- | ----------- |
> | kuruluşlar | Hayır | Hayır |
> | managementGroups | Hayır | Hayır |
> | managementGroups / ayarlar | Hayır | Hayır |
> | kaynaklar | Hayır | Hayır |
> | başlangıçTenantBackfill | Hayır | Hayır |
> | tenantBackfillStatus | Hayır | Hayır |

## <a name="microsoftmaps"></a>Microsoft.Haritalar

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporunda etiket |
> | ------------- | ----------- | ----------- |
> | accounts | Evet | Evet |
> | hesaplar / eventGridFilters | Hayır | Hayır |
> | hesaplar / privateAtlases | Evet | Evet |

## <a name="microsoftmarketplace"></a>Microsoft.Marketplace

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporunda etiket |
> | ------------- | ----------- | ----------- |
> | Sunmaktadır | Hayır | Hayır |
> | offerTypes | Hayır | Hayır |
> | offerTypes / yayıncılar | Hayır | Hayır |
> | offerTypes / yayıncılar / teklifler | Hayır | Hayır |
> | offerTypes / yayıncılar / teklifler / planlar | Hayır | Hayır |
> | offerTypes / yayıncılar / teklifler / planlar / anlaşmalar | Hayır | Hayır |
> | offerTypes / yayıncılar / teklifler / planlar / configs | Hayır | Hayır |
> | offerTypes / yayıncılar / teklifler / planlar / configs / importImage | Hayır | Hayır |
> | privategalleryitems | Hayır | Hayır |
> | privateStoreClient | Hayır | Hayır |
> | privateMağazalar | Hayır | Hayır |
> | privateStores / teklifler | Hayır | Hayır |
> | Ürünler | Hayır | Hayır |
> | Yayıncı | Hayır | Hayır |
> | yayıncılar / teklifler | Hayır | Hayır |
> | yayıncılar / teklifler / değişiklikler | Hayır | Hayır |
> | register | Hayır | Hayır |

## <a name="microsoftmarketplaceapps"></a>Microsoft.MarketplaceApps

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporunda etiket |
> | ------------- | ----------- | ----------- |
> | klasikDevServices | Evet | Evet |
> | updateİletişimTercihi | Hayır | Hayır |

## <a name="microsoftmarketplaceordering"></a>Microsoft.MarketplaceSipariş

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporunda etiket |
> | ------------- | ----------- | ----------- |
> | Anlaşma | Hayır | Hayır |
> | teklif türleri | Hayır | Hayır |

## <a name="microsoftmedia"></a>Microsoft.Media

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporunda etiket |
> | ------------- | ----------- | ----------- |
> | medya hizmetleri | Evet | Evet |
> | mediaservices / accountFilters | Hayır | Hayır |
> | mediaservices / varlıklar | Hayır | Hayır |
> | mediaservices / varlıklar / varlıkFiltreler | Hayır | Hayır |
> | mediaservices / contentKeyPolicies | Hayır | Hayır |
> | mediaservices / eventGridFilters | Hayır | Hayır |
> | mediaservices / liveEventOperations | Hayır | Hayır |
> | mediaservices / liveEvents | Evet | Evet |
> | mediaservices / liveEvents / liveOutputs | Hayır | Hayır |
> | mediaservices / liveOutputOperations | Hayır | Hayır |
> | mediaservices / mediaGraphs | Hayır | Hayır |
> | mediaservices / streamingEndpointOperations | Hayır | Hayır |
> | mediaservices / streamingEndpoints | Evet | Evet |
> | mediaservices / streamingLocators | Hayır | Hayır |
> | mediaservices / streamingPolitikalar | Hayır | Hayır |
> | mediaservices / dönüştürür | Hayır | Hayır |
> | mediaservices / dönüşümler / işler | Hayır | Hayır |

## <a name="microsoftmicroservices4spring"></a>Microsoft.Microservices4Bahar

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporunda etiket |
> | ------------- | ----------- | ----------- |
> | appClusters | Evet | Evet |

## <a name="microsoftmigrate"></a>Microsoft.Migrate

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporunda etiket |
> | ------------- | ----------- | ----------- |
> | değerlendirmeProjeler | Evet | Evet |
> | göç projeleri | Evet | Evet |
> | moveCollections | Evet | Evet |
> | Proje | Evet | Evet |

## <a name="microsoftmixedreality"></a>Microsoft.MixedReality

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporunda etiket |
> | ------------- | ----------- | ----------- |
> | holographicsYayın Hesapları | Evet | Evet |
> | objectUnderstandingAccounts | Evet | Evet |
> | remoteRenderingHesapları | Evet | Evet |
> | mekansalÇapaLarHesapları | Evet | Evet |

## <a name="microsoftnetapp"></a>Microsoft.NetApp

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporunda etiket |
> | ------------- | ----------- | ----------- |
> | netAppHesapları | Evet | Hayır |
> | netAppAccounts / hesapYedekler | Hayır | Hayır |
> | netAppAccounts / capacityPools | Evet | Hayır |
> | netAppAccounts / capacityPools / volumes | Evet | Hayır |
> | netAppAccounts / capacityPools / hacimler / anlık görüntüler | Hayır | Hayır |

## <a name="microsoftnetwork"></a>Microsoft.Network

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporunda etiket |
> | ------------- | ----------- | ----------- |
> | uygulamaAğ Ağ Geçitleri | Evet | Evet |
> | uygulamaGatewayWebApplicationFirewallPolicies | Evet | Evet |
> | uygulamaGüvenlikGrupları | Evet | Evet |
> | azureFirewallFqdnEtiketler | Hayır | Hayır |
> | azureFirewalls | Evet | Hayır |
> | burcuHosts | Evet | Evet |
> | bgpServiceTopluluklar | Hayır | Hayır |
> | Bağlantı | Evet | Evet |
> | ddosCustomPolicies | Evet | Evet |
> | ddosProtectionPlans | Evet | Evet |
> | dnsOperationStatuses | Hayır | Hayır |
> | dnszones | Evet | Evet |
> | dnszones / A | Hayır | Hayır |
> | dnszones / AAAA | Hayır | Hayır |
> | dnszones / tüm | Hayır | Hayır |
> | dnszones / CAA | Hayır | Hayır |
> | dnszones / CNAME | Hayır | Hayır |
> | dnszones / MX | Hayır | Hayır |
> | dnszones / NS | Hayır | Hayır |
> | dnszones / PTR | Hayır | Hayır |
> | dnszones / kayıt setleri | Hayır | Hayır |
> | dnszones / SOA | Hayır | Hayır |
> | dnszones / SRV | Hayır | Hayır |
> | dnszones / TXT | Hayır | Hayır |
> | expressRouteCircuits | Evet | Evet |
> | expressRouteCrossConnections | Evet | Evet |
> | expressRouteGateways | Evet | Evet |
> | expressRoutePorts | Evet | Evet |
> | expressRouteServiceProviders | Hayır | Hayır |
> | güvenlik duvarıPolitikalar | Evet | Evet |
> | ön kapılar | Evet, ancak sınırlı [(aşağıdaki nota](#frontdoor)bakın) | Evet |
> | frontdoorWebApplicationFirewallManagedRuleSets | Evet, ancak sınırlı [(aşağıdaki nota](#frontdoor)bakın) | Hayır |
> | frontdoorWebApplicationFirewallPolicies | Evet, ancak sınırlı [(aşağıdaki nota](#frontdoor)bakın) | Evet |
> | getDnsResourceReference | Hayır | Hayır |
> | internalNotify | Hayır | Hayır |
> | yükDengeleyiciler | Evet | Hayır |
> | yerelAğ Ağ Geçitleri | Evet | Evet |
> | natGateways | Evet | Evet |
> | ağNiyet Politikaları | Evet | Evet |
> | ağArayüzler | Evet | Evet |
> | ağProfilleri | Evet | Evet |
> | ağSecurityGroups | Evet | Evet |
> | ağ Watchers | Evet | Hayır |
> | ağWatchers / bağlantıMonitörler | Evet | Hayır |
> | ağ Watchers / lensler | Evet | Hayır |
> | ağWatchers / pingMeshes | Evet | Hayır |
> | p2sVpnAğ Geçitleri | Evet | Evet |
> | privateDnsOperationStatuses | Hayır | Hayır |
> | privateDnsZones | Evet | Evet |
> | privateDnsZones / A | Hayır | Hayır |
> | privateDnsZones / AAAA | Hayır | Hayır |
> | privateDnsZones / tüm | Hayır | Hayır |
> | privateDnsZones / CNAME | Hayır | Hayır |
> | privateDnsZones / MX | Hayır | Hayır |
> | privateDnsZones / PTR | Hayır | Hayır |
> | privateDnsZones / SOA | Hayır | Hayır |
> | privateDnsZones / SRV | Hayır | Hayır |
> | privateDnsZones / TXT | Hayır | Hayır |
> | privateDnsZones / virtualNetworkLinks | Evet | Evet |
> | privateEndpoints | Evet | Evet |
> | privateLinkServices | Evet | Evet |
> | publicIPAdresleri | Evet | Evet |
> | publicIPPrefixes | Evet | Evet |
> | rotaFiltreler | Evet | Evet |
> | routeTablolar | Evet | Evet |
> | serviceEndpointPolitikalar | Evet | Evet |
> | trafficManagerCoğrafi Hiyerarşileri | Hayır | Hayır |
> | trafik yöneticisi profilleri | Evet | Evet |
> | trafficmanagerprofiles/heatMaps | Hayır | Hayır |
> | trafficManagerUserMetricsKeys | Hayır | Hayır |
> | virtualHub'lar | Evet | Evet |
> | virtualNetworkGateways | Evet | Evet |
> | sanalAğlar | Evet | Evet |
> | virtualNetworkTaps | Evet | Evet |
> | sanalWans | Evet | Evet |
> | vpnAğ Geçitleri | Evet | Hayır |
> | vpnSiteler | Evet | Evet |
> | webApplicationFirewallPolitikalar | Evet | Evet |

<a id="frontdoor" />

> [!NOTE]
> Azure Ön Kapı Hizmeti için, kaynağı oluştururken etiketler uygulayabilirsiniz, ancak güncelleştirme veya etiket ekleme şu anda desteklenmez.


## <a name="microsoftnotebooks"></a>Microsoft.Notebook'lar

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporunda etiket |
> | ------------- | ----------- | ----------- |
> | NotebookProxies | Hayır | Hayır |

## <a name="microsoftnotificationhubs"></a>Microsoft.NotificationHub'lar

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporunda etiket |
> | ------------- | ----------- | ----------- |
> | Ad alanları | Evet | Hayır |
> | ad alanları / notificationHubs | Evet | Hayır |

## <a name="microsoftobjectstore"></a>Microsoft.ObjectStore

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporunda etiket |
> | ------------- | ----------- | ----------- |
> | osNamespaces | Evet | Evet |

## <a name="microsoftoffazure"></a>Microsoft.OffAzure

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporunda etiket |
> | ------------- | ----------- | ----------- |
> | HyperVSites | Evet | Evet |
> | İthalat Siteleri | Evet | Evet |
> | Sunucu Siteleri | Evet | Evet |
> | VMwareSiteleri | Evet | Evet |

## <a name="microsoftoperationalinsights"></a>Microsoft.OperationalInsights

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporunda etiket |
> | ------------- | ----------- | ----------- |
> | Küme | Evet | Evet |
> | bağlantıHedefleri | Hayır | Hayır |
> | depolamaInsightConfigs | Hayır | Hayır |
> | çalışma alanı | Evet | Evet |
> | çalışma alanları / dataExports | Hayır | Hayır |
> | çalışma alanları / dataSources | Hayır | Hayır |
> | çalışma alanları / linkedServices | Hayır | Hayır |
> | çalışma alanları / linkedStorageAccounts | Hayır | Hayır |
> | çalışma alanları / sorgu | Hayır | Hayır |
> | çalışma alanları / scopedPrivateLinkProxies | Hayır | Hayır |

## <a name="microsoftoperationsmanagement"></a>Microsoft.OperationsManagement

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporunda etiket |
> | ------------- | ----------- | ----------- |
> | yönetim dernekleri | Hayır | Hayır |
> | yönetim yapılandırmaları | Evet | Evet |
> | çözümler | Evet | Evet |
> | görünümler | Evet | Evet |

## <a name="microsoftpeering"></a>Microsoft.Peering

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporunda etiket |
> | ------------- | ----------- | ----------- |
> | legacyPeerings | Hayır | Hayır |
> | peerAsns | Hayır | Hayır |
> | akranlar | Evet | Evet |
> | peeringServiceCountries | Hayır | Hayır |
> | peeringServiceProviders | Hayır | Hayır |
> | peeringServices | Evet | Evet |

## <a name="microsoftpolicyinsights"></a>Microsoft.PolicyInsights

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporunda etiket |
> | ------------- | ----------- | ----------- |
> | politikaEtkinlikler | Hayır | Hayır |
> | politikaMeta veriler | Hayır | Hayır |
> | policyStates | Hayır | Hayır |
> | policyTrackedResources | Hayır | Hayır |
> | düzeltmeler | Hayır | Hayır |

## <a name="microsoftportal"></a>Microsoft.Portal

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporunda etiket |
> | ------------- | ----------- | ----------- |
> |  konsolları | Hayır | Hayır |
> | panolar | Evet | Evet |
> | userSettings | Hayır | Hayır |

## <a name="microsoftpowerbi"></a>Microsoft.PowerBI

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporunda etiket |
> | ------------- | ----------- | ----------- |
> | çalışma alanıKoleksiyonlar | Evet | Evet |

## <a name="microsoftpowerbidedicated"></a>Microsoft.PowerBIDedicated

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporunda etiket |
> | ------------- | ----------- | ----------- |
> | Kapasite | Evet | Evet |

## <a name="microsoftprojectbabylon"></a>Microsoft.ProjectBabylon

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporunda etiket |
> | ------------- | ----------- | ----------- |
> | accounts | Evet | Evet |

## <a name="microsoftquantum"></a>Microsoft.Quantum

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporunda etiket |
> | ------------- | ----------- | ----------- |
> | Çalışma Alanları | Evet | Evet |

## <a name="microsoftrecoveryservices"></a>Microsoft.RecoveryServices

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporunda etiket |
> | ------------- | ----------- | ----------- |
> | backupProtectedItems | Hayır | Hayır |
> | Tonoz | Evet | Evet |

## <a name="microsoftrelay"></a>Microsoft.Relay

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporunda etiket |
> | ------------- | ----------- | ----------- |
> | Ad alanları | Evet | Evet |
> | namespaces / yetkilendirme kuralları | Hayır | Hayır |
> | ad alanları / melez bağlantılar | Hayır | Hayır |
> | ad alanları / hibrit bağlantılar / yetkilendirme kuralları | Hayır | Hayır |
> | isim boşlukları / wcfrelays | Hayır | Hayır |
> | namespaces / wcfrelays / yetkilendirme kuralları | Hayır | Hayır |

## <a name="microsoftremoteapp"></a>Microsoft.RemoteApp

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporunda etiket |
> | ------------- | ----------- | ----------- |
> | accounts | Hayır | Hayır |
> | Koleksiyon | Evet | Evet |
> | koleksiyonlar / uygulamalar | Hayır | Hayır |
> | koleksiyonlar / güvenlik müdürleri | Hayır | Hayır |
> | templateImages | Hayır | Hayır |

## <a name="microsoftresourcegraph"></a>Microsoft.ResourceGraph

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporunda etiket |
> | ------------- | ----------- | ----------- |
> | Sorgu | Evet | Evet |
> | kaynakChangeDetails | Hayır | Hayır |
> | kaynak Değişiklikler | Hayır | Hayır |
> | kaynaklar | Hayır | Hayır |
> | kaynaklarTarih | Hayır | Hayır |
> | aboneliklerDurum | Hayır | Hayır |

## <a name="microsoftresourcehealth"></a>Microsoft.ResourceHealth

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporunda etiket |
> | ------------- | ----------- | ----------- |
> | availabilityStatuses | Hayır | Hayır |
> | childAvailabilityStatuses | Hayır | Hayır |
> | childResources | Hayır | Hayır |
> | ortaya çıkan sorunlar | Hayır | Hayır |
> | etkinlikler | Hayır | Hayır |
> | etkilenen Kaynaklar | Hayır | Hayır |
> | meta veriler | Hayır | Hayır |
> | bildirimler | Hayır | Hayır |

## <a name="microsoftresources"></a>Microsoft.Resources

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporunda etiket |
> | ------------- | ----------- | ----------- |
> | Dağıtım | Evet | Hayır |
> | dağıtımlar / işlemler | Hayır | Hayır |
> | dağıtımScripts | Evet | Evet |
> | DeploymentScripts / günlükleri | Hayır | Hayır |
> | Bağlantı | Hayır | Hayır |
> | bildirmeResourceJobs | Hayır | Hayır |
> | Sağlayıcı | Hayır | Hayır |
> | resourceGroups | Evet | Hayır |
> | Abonelik | Evet | Hayır |
> | Kiracı | Hayır | Hayır |

## <a name="microsoftsaas"></a>Microsoft.SaaS

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporunda etiket |
> | ------------- | ----------- | ----------- |
> | uygulamalar | Evet | Evet |
> | saasresources | Hayır | Hayır |

## <a name="microsoftsearch"></a>Microsoft.Search

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporunda etiket |
> | ------------- | ----------- | ----------- |
> | kaynakSağlıkMetadata | Hayır | Hayır |
> | aramaHizmetleri | Evet | Evet |

## <a name="microsoftsecurity"></a>Microsoft.Security

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporunda etiket |
> | ------------- | ----------- | ----------- |
> | adaptifNetworkHardenings | Hayır | Hayır |
> | gelişmişThreatProtectionSettings | Hayır | Hayır |
> | alerts | Hayır | Hayır |
> | izin Verilen Bağlantılar | Hayır | Hayır |
> | uygulamaBeyaz listeler | Hayır | Hayır |
> | değerlendirmeMeta veriler | Hayır | Hayır |
> | değerlendirme | Hayır | Hayır |
> | autoDismissAlertsRules | Hayır | Hayır |
> | otomasyon | Evet | Evet |
> | Otomatik Sağlama Ayarları | Hayır | Hayır |
> | Uyumluluklar | Hayır | Hayır |
> | dataCollectionAgents | Hayır | Hayır |
> | deviceSecurityGroups | Hayır | Hayır |
> | keşfedilenSecuritySolutions | Hayır | Hayır |
> | hariciSecuritySolutions | Hayır | Hayır |
> | BilgiKoruma Politikaları | Hayır | Hayır |
> | iotSecuritySolutions | Evet | Evet |
> | iotSecuritySolutions / analitikModeller | Hayır | Hayır |
> | iotSecuritySolutions / analitikModeller / topluuyarılar | Hayır | Hayır |
> | iotSecuritySolutions / analyticsModeller / topluöneriler | Hayır | Hayır |
> | jitNetworkAccessPolicies | Hayır | Hayır |
> | ağData | Hayır | Hayır |
> | ilkeler | Hayır | Hayır |
> | fiyatlandırma | Hayır | Hayır |
> | mevzuatUyumStandartları | Hayır | Hayır |
> | düzenlemeUyumlulukStandartları / regulatoryComplianceControls | Hayır | Hayır |
> | düzenlemeUyumlulukStandartlar / düzenleyiciUygunlukDenetimler / düzenleyiciUygunlukDeğerlendirmeleri | Hayır | Hayır |
> | secureScoreControlDefinitions | Hayır | Hayır |
> | secureScoreControls | Hayır | Hayır |
> | secureScores | Hayır | Hayır |
> | secureScores / secureScoreControls | Hayır | Hayır |
> | securityKişiler | Hayır | Hayır |
> | securitySolutions | Hayır | Hayır |
> | securitySolutionsReferenceData | Hayır | Hayır |
> | securityDurumlar | Hayır | Hayır |
> | securityStatusesÖzetler | Hayır | Hayır |
> | serverVulnerabilityAssessments | Hayır | Hayır |
> | ayarlar | Hayır | Hayır |
> | alt Değerlendirmeler | Hayır | Hayır |
> | Görevler | Hayır | Hayır |
> | Topoloji | Hayır | Hayır |
> | çalışma alanıAyarlar | Hayır | Hayır |

## <a name="microsoftsecuritygraph"></a>Microsoft.SecurityGraph

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporunda etiket |
> | ------------- | ----------- | ----------- |
> | tanıAyarları | Hayır | Hayır |
> | diagnosticSettingsCategories | Hayır | Hayır |

## <a name="microsoftsecurityinsights"></a>Microsoft.SecurityInsights

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporunda etiket |
> | ------------- | ----------- | ----------- |
> | Toplamalardan | Hayır | Hayır |
> | alertKurallar | Hayır | Hayır |
> | alertRuleTemplates | Hayır | Hayır |
> | yer işaretleri | Hayır | Hayır |
> | Durumda | Hayır | Hayır |
> | dataConnectors | Hayır | Hayır |
> | dataConnectorsCheckRequirements | Hayır | Hayır |
> | Varlık | Hayır | Hayır |
> | entityQueries | Hayır | Hayır |
> | Olay | Hayır | Hayır |
> | ofisOnay | Hayır | Hayır |
> | ayarlar | Hayır | Hayır |

## <a name="microsoftservicebus"></a>Microsoft.ServiceBus

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporunda etiket |
> | ------------- | ----------- | ----------- |
> | Ad alanları | Evet | Hayır |
> | namespaces / yetkilendirme kuralları | Hayır | Hayır |
> | ad alanları / disasterrecoveryconfigs | Hayır | Hayır |
> | ad alanları / eventgridfilters | Hayır | Hayır |
> | ad alanları / networkrulesets | Hayır | Hayır |
> | ad alanları / kuyruklar | Hayır | Hayır |
> | ad alanları / kuyruklar / yetkilendirme kuralları | Hayır | Hayır |
> | namespaces / konular | Hayır | Hayır |
> | namespaces / konular / yetkilendirme kuralları | Hayır | Hayır |
> | ad alanları / konular / abonelikler | Hayır | Hayır |
> | namespaces / konular / abonelikler / kurallar | Hayır | Hayır |
> | premiumMesajlaşmaBölgeler | Hayır | Hayır |

## <a name="microsoftservicefabric"></a>Microsoft.ServiceFabric

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporunda etiket |
> | ------------- | ----------- | ----------- |
> | uygulamalar | Evet | Evet |
> | Küme | Evet | Evet |
> | kümeler / uygulamalar | Hayır | Hayır |
> | konteynerGruplar | Evet | Evet |
> | konteynerGroupSets | Evet | Evet |
> | kenar kümeleri | Evet | Evet |
> | kenarlı kümeleri / uygulamaları | Hayır | Hayır |
> | yönetilen kümeler | Evet | Evet |
> | yönetilen kümeler / düğüm tipleri | Hayır | Hayır |
> | Ağlar | Evet | Evet |
> | gizli mağazalar | Evet | Evet |
> | gizli mağazalar / sertifikalar | Hayır | Hayır |
> | gizli mağazalar / sırlar | Hayır | Hayır |
> | volumes | Evet | Evet |

## <a name="microsoftservicefabricmesh"></a>Microsoft.ServiceFabricMesh

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporunda etiket |
> | ------------- | ----------- | ----------- |
> | uygulamalar | Evet | Evet |
> | konteynerGruplar | Evet | Evet |
> | Ağ geçit -leri | Evet | Evet |
> | Ağlar | Evet | Evet |
> | Sır -larını | Evet | Evet |
> | volumes | Evet | Evet |

## <a name="microsoftservices"></a>Microsoft.Services

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporunda etiket |
> | ------------- | ----------- | ----------- |
> | sağlayıcıKayıtlar | Hayır | Hayır |
> | sağlayıcıKayıtlar / resourceTypeRegistrations | Hayır | Hayır |
> | kullanıma sunulması | Evet | Evet |

## <a name="microsoftsignalrservice"></a>Microsoft.SignalRService

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporunda etiket |
> | ------------- | ----------- | ----------- |
> | SignalR | Evet | Evet |
> | SignalR / eventGridFilters | Hayır | Hayır |

## <a name="microsoftsiterecovery"></a>Microsoft.SiteKurtarma

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporunda etiket |
> | ------------- | ----------- | ----------- |
> | SiteRecoveryVault | Evet | Evet |

## <a name="microsoftsoftwareplan"></a>Microsoft.SoftwarePlan

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporunda etiket |
> | ------------- | ----------- | ----------- |
> | hybridUseBenefits | Hayır | Hayır |

## <a name="microsoftsolutions"></a>Microsoft.Solutions

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporunda etiket |
> | ------------- | ----------- | ----------- |
> | uygulamaTanımlar | Evet | Evet |
> | uygulamalar | Evet | Evet |
> | jitRequests | Evet | Evet |

## <a name="microsoftspoolservice"></a>Microsoft.SpoolService

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporunda etiket |
> | ------------- | ----------- | ----------- |
> | kayıtlıAbonelikler | Hayır | Hayır |
> | Biriktirici | Evet | Evet |


## <a name="microsoftsql"></a>Microsoft.SQL

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporunda etiket |
> | ------------- | ----------- | ----------- |
> | yönetilen Örnekler | Evet | Evet |
> | yönetilen Örnekler / veritabanları | Evet [(aşağıdaki nota](#sqlnote)bakın) | Evet |
> | yönetilenInstances / veritabanları / yedeklemeShortTermRetentionPolicies | Hayır | Hayır |
> | yönetilenInstances / veritabanları / şemalar / tablolar / sütunlar / sensitivityLabels | Hayır | Hayır |
> | yönetilenÖrnekler / veritabanları / güvenlik açığıDeğerlendirmeleri | Hayır | Hayır |
> | yönetilenInstances / veritabanları / güvenlik açığıDeğerlendirmeler / kurallar / taban çizgileri | Hayır | Hayır |
> | yönetilenInstances / şifrelemeProtector | Hayır | Hayır |
> | yönetilenInstances / anahtarlar | Hayır | Hayır |
> | managedInstances / restorableDroppedDatabases / backupShortTermRetentionPolicies | Hayır | Hayır |
> | yönetilenInstances / güvenlik açığıDeğerlendirmeler | Hayır | Hayır |
> | Sunucu | Evet | Evet |
> | sunucular / yöneticiler | Hayır | Hayır |
> | sunucular / iletişimLinkler | Hayır | Hayır |
> | sunucular / veritabanları | Evet [(aşağıdaki nota](#sqlnote)bakın) | Evet |
> | sunucular / şifrelemeProtector | Hayır | Hayır |
> | sunucular / firewallRules | Hayır | Hayır |
> | sunucular / anahtarlar | Hayır | Hayır |
> | sunucular / restorableDroppedDatabases | Hayır | Hayır |
> | sunucular / hizmet hedefleri | Hayır | Hayır |
> | sunucular / tdeCertificates | Hayır | Hayır |
> | virtualClusters | Hayır | Hayır |

<a id="sqlnote" />

> [!NOTE]
> Ana veritabanı etiketleri desteklemez, ancak Azure SQL Veri Ambarı veritabanları da dahil olmak üzere diğer veritabanları destek etiketleri. Azure SQL Veri Ambarı veritabanları Etkin (Duraklatılmış değil) durumunda olmalıdır.

## <a name="microsoftsqlvirtualmachine"></a>Microsoft.SqlVirtualMachine

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporunda etiket |
> | ------------- | ----------- | ----------- |
> | SqlVirtualMachineGroups | Evet | Evet |
> | SqlVirtualMachineGroups / AvailabilityGroupListeners | Hayır | Hayır |
> | SqlVirtualMachines | Evet | Evet |

## <a name="microsoftstorage"></a>Microsoft.Storage

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporunda etiket |
> | ------------- | ----------- | ----------- |
> | depolamaHesapları | Evet | Evet |
> | storageAccounts / blobServices | Hayır | Hayır |
> | storageAccounts / fileServices | Hayır | Hayır |
> | storageAccounts / queueServices | Hayır | Hayır |
> | storageAccounts / hizmetleri | Hayır | Hayır |
> | storageAccounts / hizmetler / metricDefinitions | Hayır | Hayır |
> | storageAccounts / tableServices | Hayır | Hayır |
> | Kullanım | Hayır | Hayır |

## <a name="microsoftstoragecache"></a>Microsoft.StorageÖnbellek

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporunda etiket |
> | ------------- | ----------- | ----------- |
> | Önbellek | Evet | Evet |
> | önbellekler / depolamaHedefler | Hayır | Hayır |
> | kullanımModeller | Hayır | Hayır |

## <a name="microsoftstoragereplication"></a>Microsoft.StorageÇoğaltma

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporunda etiket |
> | ------------- | ----------- | ----------- |
> | çoğaltmaGrupları | Hayır | Hayır |

## <a name="microsoftstoragesync"></a>Microsoft.StorageSync

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporunda etiket |
> | ------------- | ----------- | ----------- |
> | depolamaSyncServices | Evet | Evet |
> | storageSyncServices / kayıtlıSunucular | Hayır | Hayır |
> | storageSyncServices / syncGroups | Hayır | Hayır |
> | storageSyncServices / syncGroups / cloudEndpoints | Hayır | Hayır |
> | storageSyncServices / syncGroups / serverEndpoints | Hayır | Hayır |
> | storageSyncServices / iş akışları | Hayır | Hayır |

## <a name="microsoftstoragesyncdev"></a>Microsoft.StorageSyncDev

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporunda etiket |
> | ------------- | ----------- | ----------- |
> | depolamaSyncServices | Evet | Evet |
> | storageSyncServices / kayıtlıSunucular | Hayır | Hayır |
> | storageSyncServices / syncGroups | Hayır | Hayır |
> | storageSyncServices / syncGroups / cloudEndpoints | Hayır | Hayır |
> | storageSyncServices / syncGroups / serverEndpoints | Hayır | Hayır |
> | storageSyncServices / iş akışları | Hayır | Hayır |

## <a name="microsoftstoragesyncint"></a>Microsoft.StorageSyncInt

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporunda etiket |
> | ------------- | ----------- | ----------- |
> | depolamaSyncServices | Evet | Evet |
> | storageSyncServices / kayıtlıSunucular | Hayır | Hayır |
> | storageSyncServices / syncGroups | Hayır | Hayır |
> | storageSyncServices / syncGroups / cloudEndpoints | Hayır | Hayır |
> | storageSyncServices / syncGroups / serverEndpoints | Hayır | Hayır |
> | storageSyncServices / iş akışları | Hayır | Hayır |

## <a name="microsoftstorsimple"></a>Microsoft.StorSimple

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporunda etiket |
> | ------------- | ----------- | ----------- |
> | Yöneticileri | Evet | Evet |

## <a name="microsoftstreamanalytics"></a>Microsoft.StreamAnalytics

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporunda etiket |
> | ------------- | ----------- | ----------- |
> | akış işleri | Evet (aşağıdaki nota bakın) | Evet |

> [!NOTE]
> Akış işleri çalışırken etiket ekemezsiniz. Etiket eklemek için kaynağı durdurun.

## <a name="microsoftsubscription"></a>Microsoft.Abonelik

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporunda etiket |
> | ------------- | ----------- | ----------- |
> | iptal | Hayır | Hayır |
> | Createsubscription | Hayır | Hayır |
> | seçin | Hayır | Hayır |
> | yeniden adlandır | Hayır | Hayır |
> | AbonelikTanımlar | Hayır | Hayır |
> | Abonelik İşlemleri | Hayır | Hayır |
> | Abonelik | Hayır | Hayır |

## <a name="microsofttimeseriesinsights"></a>Microsoft.TimeSeriesInsights

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporunda etiket |
> | ------------- | ----------- | ----------- |
> | Ortam | Evet | Hayır |
> | ortamlar / accessPolitikalar | Hayır | Hayır |
> | ortamlar / olay kaynakları | Evet | Hayır |
> | ortamlar / referenceDataSets | Evet | Hayır |

## <a name="microsoftvmwarecloudsimple"></a>Microsoft.VMwareCloudBasit

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporunda etiket |
> | ------------- | ----------- | ----------- |
> | adanmışCloudNodes | Evet | Evet |
> | özelCloudServices | Evet | Evet |
> | virtualMachines | Evet | Evet |

## <a name="microsoftvnfmanager"></a>Microsoft.VnfManager

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporunda etiket |
> | ------------- | ----------- | ----------- |
> | cihazlar | Evet | Evet |
> | kayıtlıAbonelikler | Hayır | Hayır |
> | Satıcı | Hayır | Hayır |
> | satıcılar / skus | Hayır | Hayır |
> | satıcılar / vnfs | Hayır | Hayır |
> | sanalNetworkFunctionSkus | Hayır | Hayır |
> | vnfs | Evet | Evet |

## <a name="microsoftweb"></a>Microsoft.Web

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporunda etiket |
> | ------------- | ----------- | ----------- |
> | apiManagementHesapları | Hayır | Hayır |
> | apiManagementAccounts / apiAcls | Hayır | Hayır |
> | apiManagementAccounts / apis | Hayır | Hayır |
> | apiManagementAccounts / apis / apiAcls | Hayır | Hayır |
> | apiManagementAccounts / apis / bağlantıAcls | Hayır | Hayır |
> | apiManagementAccounts / apis / bağlantılar | Hayır | Hayır |
> | apiManagementAccounts / apis / bağlantılar / bağlantılarAcls | Hayır | Hayır |
> | apiManagementAccounts / apis / localizedDefinitions | Hayır | Hayır |
> | apiManagementAccounts / bağlantıAcls | Hayır | Hayır |
> | apiManagementAccounts / bağlantılar | Hayır | Hayır |
> | faturalamaÖlçerler | Hayır | Hayır |
> | sertifikalar | Evet | Evet |
> | bağlantıAğ Geçitleri | Evet | Evet |
> | Bağlantı | Evet | Evet |
> | customApis | Evet | Evet |
> | silinen Siteler | Hayır | Hayır |
> | hostingOrtamlar | Evet | Evet |
> | hostingOrtamlar / olayGridFilters | Hayır | Hayır |
> | hostingOrtamlar / multiRolePools | Hayır | Hayır |
> | hostingOrtamlar / işçiHavuzları | Hayır | Hayır |
> | kubeOrtamlar | Evet | Evet |
> | publishingKullanıcılar | Hayır | Hayır |
> | Öneriler | Hayır | Hayır |
> | kaynakSağlıkMetadata | Hayır | Hayır |
> | Çalıştırma | Hayır | Hayır |
> | serverFarms | Evet | Evet |
> | serverFarms / eventGridFilters | Hayır | Hayır |
> | Siteler | Evet | Evet |
> | siteler / config  | Hayır | Hayır |
> | siteler / eventGridFilters | Hayır | Hayır |
> | siteler / hostNameBindings | Hayır | Hayır |
> | siteler / ağConfig | Hayır | Hayır |
> | siteler / premieraddons | Evet | Evet |
> | siteler / yuvalar | Evet | Evet |
> | siteler / yuvalar / eventGridFilters | Hayır | Hayır |
> | siteler / yuvalar / hostNameBindings | Hayır | Hayır |
> | siteler / yuvalar / networkConfig | Hayır | Hayır |
> | sourceControls | Hayır | Hayır |
> | staticSites | Evet | Evet |
> | Doğrulamak | Hayır | Hayır |
> | verifyHostingEnvironmentVnet | Hayır | Hayır |

## <a name="microsoftwindowsdefenderatp"></a>Microsoft.WindowsDefenderATP

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporunda etiket |
> | ------------- | ----------- | ----------- |
> | tanıAyarları | Hayır | Hayır |
> | diagnosticSettingsCategories | Hayır | Hayır |

## <a name="microsoftwindowsiot"></a>Microsoft.WindowsIoT

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporunda etiket |
> | ------------- | ----------- | ----------- |
> | Cihaz Hizmetleri | Evet | Evet |

## <a name="microsoftworkloadmonitor"></a>Microsoft.WorkloadMonitor

> [!div class="mx-tableFixed"]
> | Kaynak türü | Etiketleri destekler | Maliyet raporunda etiket |
> | ------------- | ----------- | ----------- |
> | Bileşen | Hayır | Hayır |
> | componentsSummary | Hayır | Hayır |
> | monitorInstances | Hayır | Hayır |
> | monitorInstancesSummary | Hayır | Hayır |
> | Monitör | Hayır | Hayır |
> | bildirimAyarlar | Hayır | Hayır |

## <a name="next-steps"></a>Sonraki adımlar

Etiketleri kaynaklara nasıl uygulayacağınızı öğrenmek için [Azure kaynaklarınızı düzenlemek için Etiketleri Kullan'a](tag-resources.md)bakın.
