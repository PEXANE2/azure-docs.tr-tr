---
title: Tam modda silme
description: Kaynak türlerinin Azure Kaynak Yöneticisi şablonlarında tam mod silme işlemini nasıl ele düzeye bildiğini gösterir.
ms.topic: conceptual
ms.date: 02/26/2020
ms.openlocfilehash: 5f797974212636460306c6a17869d6b8380545ab
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77664415"
---
# <a name="deletion-of-azure-resources-for-complete-mode-deployments"></a>Tam mod dağıtımları için Azure kaynaklarının silinmesi

Bu makalede, tam modda dağıtılan bir şablon da değilken kaynak türlerinin silme işlemini nasıl işlediği açıklanmaktadır.

Tür tam modla dağıtılan şablonda olmadığında **Evet** ile işaretlenmiş kaynak türleri silinir.

**Hayır** ile işaretlenmiş kaynak türleri şablonda olmadığında otomatik olarak silinmez; ancak, üst kaynak silinirse silinirler. Davranışın tam açıklaması için [Azure Kaynak Yöneticisi dağıtım modlarına](deployment-modes.md)bakın.

[Bir şablonda birden fazla kaynak grubuna](cross-resource-group-deployment.md)dağıtırsanız, dağıtım işleminde belirtilen kaynak grubundaki kaynaklar silinebilir. İkincil kaynak gruplarındaki kaynaklar silinmez.

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
> - [Microsoft.CortanaAnalytics](#microsoftcortanaanalytics)
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
> - [Microsoft.Kusto](#microsoftkusto)
> - [Microsoft.LabServices](#microsoftlabservices)
> - [Microsoft.Logic](#microsoftlogic)
> - [Microsoft.MachineLearning](#microsoftmachinelearning)
> - [Microsoft.MachineLearningServices](#microsoftmachinelearningservices)
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
> - [Microsoft.Notebook'lar](#microsoftnotebooks)
> - [Microsoft.Network](#microsoftnetwork)
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
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | DomainServices | Evet |
> | DomainServices / oucontainer | Hayır |

## <a name="microsoftaddons"></a>Microsoft.Addons

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | destekSağlayıcılar | Hayır |

## <a name="microsoftadhybridhealthservice"></a>Microsoft.ADHybridHealthService

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | aadsupportcases | Hayır |
> | hizmetler ekler | Hayır |
> | Ajan | Hayır |
> | anonimapiusers | Hayır |
> | yapılandırma | Hayır |
> | günlükler | Hayır |
> | reports | Hayır |
> | servicehealthmetrics | Hayır |
> | services | Hayır |

## <a name="microsoftadvisor"></a>Microsoft.Advisor

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | Yapılandırma | Hayır |
> | öneriler oluşturur | Hayır |
> | meta veriler | Hayır |
> | Öneriler | Hayır |
> | bastırma | Hayır |

## <a name="microsoftalertsmanagement"></a>Microsoft.AlertsManagement

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | actionKurallar | Evet |
> | alerts | Hayır |
> | alertsList | Hayır |
> | uyarılarıMetaData | Hayır |
> | alertsSummary | Hayır |
> | alertsSummaryList | Hayır |
> | Geribildirim | Hayır |
> | smartDetectorAlertRules | Evet |
> | smartDetectorRuntimeOrtamlar | Hayır |
> | smartGroups | Hayır |

## <a name="microsoftanalysisservices"></a>Microsoft.AnalysisServices

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | Sunucu | Evet |

## <a name="microsoftapimanagement"></a>Microsoft.ApiManagement

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | reportFeedback | Hayır |
> | hizmet | Evet |
> | doğrulaServiceName | Hayır |

## <a name="microsoftappconfiguration"></a>Microsoft.AppConfiguration

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | configurationMağazalar | Evet |
> | configurationStores / eventGridFilters | Hayır |

## <a name="microsoftappplatform"></a>Microsoft.AppPlatform

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | Spring | Evet |

## <a name="microsoftattestation"></a>Microsoft.Attestation

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | attestationSağlayıcıları | Hayır |

## <a name="microsoftauthorization"></a>Microsoft.Authorization

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | klasikYöneticiler | Hayır |
> | dataAliases | Hayır |
> | denyAtamaları | Hayır |
> | Access'i yükseltme | Hayır |
> | findOrphanRoleAssignments | Hayır |
> | Kilit | Hayır |
> | Izin | Hayır |
> | ilkeAtamalar | Hayır |
> | policyTanımlar | Hayır |
> | politikaSetDefinitions | Hayır |
> | sağlayıcıİşlemler | Hayır |
> | roleAtamaları | Hayır |
> | roleAssignmentsUsageMetrics | Hayır |
> | roleDefinitions | Hayır |

## <a name="microsoftautomation"></a>Microsoft.Automation

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | otomasyonHesaplar | Evet |
> | otomasyonHesaplar / yapılandırmalar | Evet |
> | automationAccounts / jobs | Hayır |
> | automationAccounts / privateEndpointConnectionProxies | Hayır |
> | automationAccounts / privateEndpointConnections | Hayır |
> | automationAccounts / privateLinkResources | Hayır |
> | automationAccounts / runbooks | Evet |
> | automationAccounts / softwareUpdateConfigurations | Hayır |
> | automationAccounts / webhooks | Hayır |

## <a name="microsoftazconfig"></a>Microsoft.Azconfig

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | configurationMağazalar | Evet |
> | configurationStores / eventGridFilters | Hayır |

## <a name="microsoftazuregeneva"></a>Microsoft.Azure.Cenevre

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | Ortam | Hayır |
> | ortamlar / hesaplar | Hayır |
> | ortamlar / hesaplar / ad alanları | Hayır |
> | ortamlar / hesaplar / ad alanları / yapılandırmalar | Hayır |

## <a name="microsoftazureactivedirectory"></a>Microsoft.AzureActiveDirectory

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | b2cDirectories | Evet |
> | b2ctenants | Hayır |

## <a name="microsoftazuredata"></a>Microsoft.AzureVerileri

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | hibridDataManagers | Evet |
> | postgresÖrnekleri | Evet |
> | sqlBigDataClusters | Evet |
> | sqlInstances | Evet |
> | Sqlserverregistrations | Evet |
> | sqlServerRegistrations / sqlServers | Hayır |

## <a name="microsoftazurestack"></a>Microsoft.AzureStack

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | Kayıt | Evet |
> | kayıtlar / müşteriAbonelikleri | Hayır |
> | kayıtlar / ürünler | Hayır |
> | doğrulamaTuşları | Hayır |

## <a name="microsoftbatch"></a>Microsoft.Batch

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | toplu Hesaplar | Evet |

## <a name="microsoftbilling"></a>Microsoft.Faturalandırma

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | faturahesapları | Hayır |
> | faturahesapları / anlaşmaları | Hayır |
> | faturahesapları / faturalamaİzinler | Hayır |
> | faturahesapları / faturaprofilleri | Hayır |
> | faturahesapları / faturalamaProfiller / faturalamaİzinler | Hayır |
> | faturalamaHesapları / faturalamaProfiller / faturalamaRol Atamaları | Hayır |
> | faturahesapları / faturalamaProfiller / faturalamaRol Tanımları | Hayır |
> | faturahesapları / faturalamaProfiller / faturalamaAbonelikler | Hayır |
> | faturahesapları / faturalamaProfiller / createBillingRoleAssignment | Hayır |
> | faturahesapları / faturalamaProfiller / müşteriler | Hayır |
> | faturahesapları / faturalamaProfiller / talimatlar | Hayır |
> | faturahesapları / faturalarProfiller / faturalar | Hayır |
> | faturahesapları / faturalamaProfiller / faturalar / fiyat sayfası | Hayır |
> | faturahesapları / faturalamaProfiller / faturalar / işlemler | Hayır |
> | faturahesapları / faturaprofilleri / faturaBölümleri | Hayır |
> | faturahesapları / faturalamaProfiller / faturaBölümler / faturalamaİzinler | Hayır |
> | faturalamaHesapları / faturalamaProfiller / faturaBölümler / faturalamaRol Atamaları | Hayır |
> | faturahesapları / faturalamaProfiller / faturaBölümler / faturalamaRol Tanımları | Hayır |
> | faturahesapları / faturaprofilleri / faturaBölümleri / faturalamaAbonelikler | Hayır |
> | faturalamaHesapları / faturaprofilleri / faturaBölümleri / createBillingRoleAssignment | Hayır |
> | faturahesapları / faturaprofilleri / faturaBölümler / başlatTransfer | Hayır |
> | faturahesapları / faturalamaProfiller / faturaBölümler / ürünler | Hayır |
> | faturahesapları / faturalamaProfiller / faturaBölümler / ürünler / transfer | Hayır |
> | faturahesapları / faturaprofilleri / faturaBölümler / ürünler / updateAutoRenew | Hayır |
> | faturahesapları / faturaprofilleri / faturaBölümleri / işlemleri | Hayır |
> | faturahesapları / faturaprofilleri / faturaBölümleri / transferleri | Hayır |
> | faturahesapları / Faturaprofilleri / patchİşlemleri | Hayır |
> | faturahesapları / faturalamaProfiller / ödeme Yöntemleri | Hayır |
> | faturahesapları / faturalamaProfiller / politikalar | Hayır |
> | faturahesapları / faturalamaProfiller / fiyat sayfası | Hayır |
> | faturahesapları / faturalamaProfiller / pricesheetDownloadOperations | Hayır |
> | faturahesapları / faturalamaProfiller / ürünler | Hayır |
> | faturahesapları / faturalamaProfiller / işlemler | Hayır |
> | faturahesapları / faturalamaRol Atamaları | Hayır |
> | faturahesapları / faturalamaRolTanımları | Hayır |
> | faturahesapları / faturaabonelikleri | Hayır |
> | faturahesapları / faturalarAbonelikler / faturalar | Hayır |
> | faturalamaHesapları / createBillingRoleAssignment | Hayır |
> | faturahesapları / createInvoiceSectionOperations | Hayır |
> | faturahesapları / müşteriler | Hayır |
> | faturahesapları / müşteriler / faturalamaİzinler | Hayır |
> | faturahesapları / müşteriler / faturaabonelikleri | Hayır |
> | faturahesapları / müşteriler / başlatTransfer | Hayır |
> | faturahesapları / müşteriler / poliçeler | Hayır |
> | faturahesapları / müşteriler / ürünler | Hayır |
> | faturahesapları / müşteriler / işlemler | Hayır |
> | faturahesapları / müşteriler / transferler | Hayır |
> | faturahesapları / bölümleri | Hayır |
> | faturalamaHesapları / kayıtHesapları | Hayır |
> | faturahesapları / faturaları | Hayır |
> | faturahesapları / faturaBölümleri | Hayır |
> | faturahesapları / faturaBölümleri / faturalamaAbonelikİşlemleri | Hayır |
> | faturahesapları / faturaBölümleri / faturaabonelikleri | Hayır |
> | faturahesapları / faturaBölümler / faturalamaAbonelikler / transfer | Hayır |
> | faturahesapları / faturaBölümler / yükseltme | Hayır |
> | faturahesapları / faturaBölümler / başlatTransfer | Hayır |
> | faturahesapları / faturaBölümler / yamaİşlemleri | Hayır |
> | faturahesapları / faturaBölümler / productMoveOperations | Hayır |
> | faturahesapları / faturaBölümler / ürünler | Hayır |
> | faturahesapları / faturaBölümler / ürünler / transfer | Hayır |
> | faturahesapları / faturaBölümler / ürünler / updateAutoRenew | Hayır |
> | faturahesapları / faturaBölümler / işlemler | Hayır |
> | faturahesapları / faturaBölümler / transferler | Hayır |
> | faturaHesapları / lineOfCredit | Hayır |
> | faturahesapları / yamaİşlemleri | Hayır |
> | faturahesapları / ödeme Yöntemleri | Hayır |
> | faturahesapları / ürünleri | Hayır |
> | faturahesapları / işlemleri | Hayır |
> | faturalamaDönemleri | Hayır |
> | faturalamaİzinler | Hayır |
> | faturalamaEmlak | Hayır |
> | faturalandırmaRol Atamaları | Hayır |
> | faturalamaRolTanımlar | Hayır |
> | createBillingRoleAssignment | Hayır |
> | Bölüm | Hayır |
> | enrollmentHesapları | Hayır |
> | Fatura | Hayır |
> | Transfer | Hayır |
> | transferler / acceptTransfer | Hayır |
> | transferler / düşüşTransfer | Hayır |
> | transferler / operationStatus | Hayır |
> | transferler / doğrulaTransfer | Hayır |
> | doğrulayan Adres | Hayır |

## <a name="microsoftbingmaps"></a>Microsoft.BingHaritalar

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | mapApis | Evet |
> | updateİletişimTercihi | Hayır |

## <a name="microsoftblockchain"></a>Microsoft.Blockchain

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | blockchainÜyeler | Evet |
> | cordaÜyeler | Evet |
> | Watchers | Evet |

## <a name="microsoftblueprint"></a>Microsoft.Blueprint

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | blueprintAtamaları | Hayır |
> | blueprintAtamaları / atamaİşlemleri | Hayır |
> | blueprintAtamaları / işlemleri | Hayır |
> | Plan | Hayır |
> | planlar / eserler | Hayır |
> | planları / sürümleri | Hayır |
> | planları / sürümleri / eserler | Hayır |

## <a name="microsoftbotservice"></a>Microsoft.BotService

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | botServices | Evet |
> | botServices / kanallar | Hayır |
> | botServices / bağlantılar | Hayır |
> | diller | Hayır |
> | templates | Hayır |

## <a name="microsoftcache"></a>Microsoft.Önbellek

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | Redis | Evet |

## <a name="microsoftcapacity"></a>Microsoft.Capacity

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | uygulamalı Rezervasyonlar | Hayır |
> | autoQuotaIncrease | Hayır |
> | calculateExchange | Hayır |
> | hesaplamaFiyatı | Hayır |
> | hesaplamaPurchasePrice | Hayır |
> | Katalog | Hayır |
> | commercialReservationOrders | Hayır |
> | Exchange | Hayır |
> | placePurchaseOrder | Hayır |
> | rezervasyonSiparişler | Hayır |
> | rezervasyonSiparişler / calculateRefund | Hayır |
> | reservationSiparişler / birleştirme | Hayır |
> | rezervasyonSiparişler / rezervasyonlar | Hayır |
> | rezervasyonSiparişler / rezervasyonlar / revizyonlar | Hayır |
> | reservationSiparişler / iade | Hayır |
> | reservationOrders / split | Hayır |
> | reservationSiparişler / takas | Hayır |
> | Rezervasyonlar | Hayır |
> | kaynakSağlayıcılar | Hayır |
> | kaynaklar | Hayır |
> | doğrulamaReservationOrder | Hayır |

## <a name="microsoftcdn"></a>Microsoft.Cdn

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | CdnWebApplicationFirewallManagedRuleSets | Hayır |
> | CdnWebApplicationFirewallPolicies | Evet |
> | kenar düğümleri | Hayır |
> | Profil | Evet |
> | profiller / uç noktalar | Evet |
> | profiller / uç noktalar / özel etki alanları | Hayır |
> | profiller / uç noktalar / origins | Hayır |
> | validateProbe | Hayır |

## <a name="microsoftcertificateregistration"></a>Microsoft.CertificateRegistration

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | sertifikaSiparişler | Evet |
> | sertifikaSiparişler / sertifikalar | Hayır |
> | doğrulamaSertifikaRegistrationInformation | Hayır |

## <a name="microsoftclassiccompute"></a>Microsoft.ClassicCompute

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | Yetenek -lerini | Hayır |
> | alan Adları | Evet |
> | domainNames / yetenekleri | Hayır |
> | domainNames / internalLoadBalancers | Hayır |
> | domainNames / serviceCertificates | Hayır |
> | alan Adları / yuvaları | Hayır |
> | alan Adları / yuvaları / rolleri | Hayır |
> | alan Adları / yuvalar / roller / metricDefinitions | Hayır |
> | alan Adları / yuvalar / roller / ölçümler | Hayır |
> | moveSubscriptionResources | Hayır |
> | işletimSistemiAileler | Hayır |
> | işletim Sistemleri | Hayır |
> | quotas | Hayır |
> | resourceTypes | Hayır |
> | doğrulamaAbonelikMoveAvailability | Hayır |
> | virtualMachines | Evet |
> | virtualMachines / diagnosticSettings | Hayır |
> | virtualMachines / metricDefinitions | Hayır |
> | virtualMachines / ölçümler | Hayır |

## <a name="microsoftclassicinfrastructuremigrate"></a>Microsoft.ClassicInfrastructureMigrate

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | klasikAltyapıKaynakları | Hayır |

## <a name="microsoftclassicnetwork"></a>Microsoft.ClassicNetwork

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | Yetenek -lerini | Hayır |
> | expressRouteCrossConnections | Hayır |
> | expressRouteCrossConnections / peerings | Hayır |
> | ağ geçidiDesteklenen Cihazlar | Hayır |
> | ağSecurityGroups | Evet |
> | quotas | Hayır |
> | ayrılmışIps | Evet |
> | sanalAğlar | Evet |
> | virtualNetworks / remoteVirtualNetworkPeeringProxies | Hayır |
> | virtualNetworks / virtualNetworkPeerings | Hayır |

## <a name="microsoftclassicstorage"></a>Microsoft.ClassicStorage

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | Yetenek -lerini | Hayır |
> | Disk | Hayır |
> | images | Hayır |
> | osImages | Hayır |
> | osPlatformImages | Hayır |
> | publicImages | Hayır |
> | quotas | Hayır |
> | depolamaHesapları | Evet |
> | storageAccounts / blobServices | Hayır |
> | storageAccounts / fileServices | Hayır |
> | storageAccounts / metricDefinitions | Hayır |
> | storageAccounts / ölçümler | Hayır |
> | storageAccounts / queueServices | Hayır |
> | storageAccounts / hizmetleri | Hayır |
> | storageAccounts / services / diagnosticAyarlar | Hayır |
> | storageAccounts / hizmetler / metricDefinitions | Hayır |
> | storageAccounts / hizmetler / ölçümler | Hayır |
> | storageAccounts / tableServices | Hayır |
> | storageAccounts / vmImages | Hayır |
> | vmImages | Hayır |

## <a name="microsoftcognitiveservices"></a>Microsoft.CognitiveServices

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | accounts | Evet |

## <a name="microsoftcommerce"></a>Microsoft.Commerce

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | RateCard | Hayır |
> | KullanımA Göre Toplamlar | Hayır |

## <a name="microsoftcompute"></a>Microsoft.Compute

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | availabilitySets | Evet |
> | diskŞifreleme Setleri | Evet |
> | Disk | Evet |
> | Galerileri | Evet |
> | galeriler / uygulamalar | Hayır |
> | galeriler / uygulamalar / sürümler | Hayır |
> | galeriler / resimler | Hayır |
> | galeriler / resimler / sürümler | Hayır |
> | hostGroups | Evet |
> | hostGroups / hosts | Evet |
> | images | Evet |
> | yakınlıkPlacementGroups | Evet |
> | restorePointCollections | Evet |
> | restorePointCollections / restorePoints | Hayır |
> | paylaşılanVMImages | Evet |
> | paylaşılanVMImages / sürümleri | Hayır |
> | anlık görüntüler | Evet |
> | virtualMachines | Evet |
> | virtualMachines / uzantıları | Evet |
> | virtualMachines / metricDefinitions | Hayır |
> | virtualMachineScaleSets | Evet |
> | virtualMachineScaleSets / uzantıları | Hayır |
> | virtualMachineScaleSets / ağArayüzler | Hayır |
> | virtualMachineScaleSets / publicIPAddresses | Hayır |
> | virtualMachineScaleSets / virtualMachines | Hayır |
> | virtualMachineScaleSets / virtualMachines / networkInterfaces | Hayır |

## <a name="microsoftconsumption"></a>Microsoft.Consumption

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | Toplu Maliyet | Hayır |
> | Bakiyeler | Hayır |
> | Bütçeler | Hayır |
> | Ücretler | Hayır |
> | MaliyetEtiketleri | Hayır |
> | Kredi | Hayır |
> | etkinlikler | Hayır |
> | Tahminler | Hayır |
> | Çok | Hayır |
> | Pazaryerleri | Hayır |
> | Fiyat listeleri | Hayır |
> | Ürünler | Hayır |
> | Rezervasyon Detayları | Hayır |
> | Rezervasyon Önerileri | Hayır |
> | Rezervasyon Özetleri | Hayır |
> | Rezervasyon İşlemleri | Hayır |
> | Etiketler | Hayır |
> | Kiracı | Hayır |
> | Koşullar | Hayır |
> | Kullanım Detayları | Hayır |

## <a name="microsoftcontainerinstance"></a>Microsoft.ContainerInstance

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | konteynerGruplar | Evet |
> | serviceAssociationLinks | Hayır |

## <a name="microsoftcontainerregistry"></a>Microsoft.ContainerRegistry

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | Kayıt | Evet |
> | kayıt defterleri / oluşturur | Hayır |
> | kayıt defterleri / oluşturur / iptal | Hayır |
> | kayıt defteri / oluşturur / getLogLink | Hayır |
> | kayıt defterleri / buildTasks | Evet |
> | kayıt defterleri / buildGörevler / adımlar | Hayır |
> | kayıt defterleri / eventGridFilters | Hayır |
> | kayıt defterleri / oluşturmak Kimlik Bilgileri | Hayır |
> | kayıt defteri / getBuildSourceUploadUrl | Hayır |
> | kayıt defterleri / GetCredentials | Hayır |
> | kayıt defteri / importImage | Hayır |
> | kayıt defterleri / privateEndpointConnectionProxies | Hayır |
> | kayıt defterleri / privateEndpointConnectionProxies / doğrulamak | Hayır |
> | kayıt defterleri / privateEndpointConnections | Hayır |
> | kayıt defterleri / privateLinkResources | Hayır |
> | kayıt defterleri / queueYapı | Hayır |
> | kayıt defteri / rejenereCredential | Hayır |
> | kayıt defterleri / rejenere Credentials | Hayır |
> | kayıt defterleri / çoğaltmalar | Evet |
> | kayıt defterleri / çalışır | Hayır |
> | kayıt defterleri / çalışır / iptal | Hayır |
> | kayıt defteri / programRun | Hayır |
> | kayıt defteri / scopeMaps | Hayır |
> | kayıt defterleri / taskRuns | Evet |
> | kayıt defterleri / görevleri | Evet |
> | kayıt defterleri / belirteçleri | Hayır |
> | kayıt defterleri / updatePolitikalar | Hayır |
> | kayıt defterleri / webhooks | Evet |
> | kayıt defteri / webhooks / getCallbackConfig | Hayır |
> | kayıt defteri / webhooks / ping | Hayır |

## <a name="microsoftcontainerservice"></a>Microsoft.ContainerService

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | konteynerHizmetleri | Evet |
> | yönetilen Kümeler | Evet |
> | openShiftManagedClusters | Evet |

## <a name="microsoftcortanaanalytics"></a>Microsoft.CortanaAnalytics

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | accounts | Evet |

## <a name="microsoftcostmanagement"></a>Microsoft.CostManagement

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | Uyarılar | Hayır |
> | Faturahesapları | Hayır |
> | Bütçeler | Hayır |
> | CloudConnectors | Hayır |
> | Bağlayıcılar | Evet |
> | Departmanlar | Hayır |
> | Boyutlar | Hayır |
> | Kayıt Hesapları | Hayır |
> | Ihracat | Hayır |
> | Dış Fatura hesapları | Hayır |
> | Dış Faturahesapları / Uyarılar | Hayır |
> | Dış Faturahesapları / Boyutlar | Hayır |
> | Dış Faturahesapları / Tahmin | Hayır |
> | Dış Fatura hesapları / Sorgu | Hayır |
> | Harici Abonelikler | Hayır |
> | Harici Abonelikler / Uyarılar | Hayır |
> | Harici Abonelikler / Boyutlar | Hayır |
> | ExternalSubscriptions / Tahmin | Hayır |
> | Harici Abonelikler / Sorgu | Hayır |
> | Tahmin | Hayır |
> | Sorgu | Hayır |
> | register | Hayır |
> | Reportconfigs | Hayır |
> | Reports | Hayır |
> | Ayarlar | Hayır |
> | showbackKurallar | Hayır |
> | Görünümler | Hayır |

## <a name="microsoftcustomerlockbox"></a>Microsoft.CustomerLockbox

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | Istek | Hayır |

## <a name="microsoftcustomproviders"></a>Microsoft.CustomProviders

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | Dernek | Hayır |
> | kaynakSağlayıcılar | Evet |

## <a name="microsoftdatabox"></a>Microsoft.DataBox

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | Işleri | Evet |

## <a name="microsoftdataboxedge"></a>Microsoft.DataBoxEdge

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | DataBoxEdgeCihazlar | Evet |

## <a name="microsoftdatabricks"></a>Microsoft.Databricks

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | çalışma alanı | Evet |
> | çalışma alanları / dbWorkspaces | Hayır |
> | çalışma alanları / depolamaŞifreleme | Hayır |
> | çalışma alanları / virtualNetworkPeerings | Hayır |

## <a name="microsoftdatacatalog"></a>Microsoft.DataCatalog

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | Katalog | Evet |
> | veri katalogları | Evet |
> | veri katalogları / veri kaynakları | Hayır |
> | datacatalogs / datasources / taramaları | Hayır |
> | datacatalogs / datasources / taramaları / veri kümeleri | Hayır |
> | datacatalogs / datasources / taramaları / tetikler | Hayır |

## <a name="microsoftdatafactory"></a>Microsoft.DataFactory

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | dataFabrikalar | Evet |
> | dataFactorys / diagnosticSettings | Hayır |
> | dataFabrikalar / metricDefinitions | Hayır |
> | dataFactorySchema | Hayır |
> | Fabrika | Evet |
> | fabrikalar / integrationRuntimes | Hayır |

## <a name="microsoftdatalakeanalytics"></a>Microsoft.DataLakeAnalytics

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | accounts | Evet |
> | hesaplar / dataLakeStoreAccounts | Hayır |
> | hesaplar / depolamaHesapları | Hayır |
> | hesaplar / depolamaHesapları / konteynerler | Hayır |
> | hesaplar / transferAnalyticsUnits | Hayır |

## <a name="microsoftdatalakestore"></a>Microsoft.DataLakeStore

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | accounts | Evet |
> | hesaplar / eventGridFilters | Hayır |
> | hesaplar / firewallRules | Hayır |

## <a name="microsoftdatamigration"></a>Microsoft.DataMigration

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | services | Evet |
> | hizmetler / projeler | Evet |

## <a name="microsoftdatashare"></a>Microsoft.DataShare

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | accounts | Evet |
> | hesaplar / hisse | Hayır |
> | hesaplar / paylaşımlar / veri kümeleri | Hayır |
> | hesaplar / hisse / davetiyeler | Hayır |
> | hesaplar / hisse / providersharesubscriptions | Hayır |
> | hesaplar / paylaşımlar / senkronizasyonAyarları | Hayır |
> | hesaplar / sharesubscriptions | Hayır |
> | hesaplar / sharesubscriptions / consumerSourceDataSets | Hayır |
> | hesaplar / sharesubscriptions / datasetmappings | Hayır |
> | hesaplar / sharesubscriptions / tetikleyiciler | Hayır |

## <a name="microsoftdbformariadb"></a>Microsoft.DBforMariaDB

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | Sunucu | Evet |
> | sunucular / danışmanlar | Hayır |
> | sunucular / anahtarlar | Hayır |
> | sunucular / privateEndpointConnectionProxies | Hayır |
> | sunucular / privateEndpointConnections | Hayır |
> | sunucular / privateLinkResources | Hayır |
> | sunucular / queryTexts | Hayır |
> | sunucular / recoverableServers | Hayır |
> | sunucular / topQueryStatistics | Hayır |
> | sunucular / virtualNetworkRules | Hayır |
> | sunucular / waitStatistics | Hayır |

## <a name="microsoftdbformysql"></a>Microsoft.DBforMySQL

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | Sunucu | Evet |
> | sunucular / danışmanlar | Hayır |
> | sunucular / anahtarlar | Hayır |
> | sunucular / privateEndpointConnectionProxies | Hayır |
> | sunucular / privateEndpointConnections | Hayır |
> | sunucular / privateLinkResources | Hayır |
> | sunucular / queryTexts | Hayır |
> | sunucular / recoverableServers | Hayır |
> | sunucular / topQueryStatistics | Hayır |
> | sunucular / virtualNetworkRules | Hayır |
> | sunucular / waitStatistics | Hayır |

## <a name="microsoftdbforpostgresql"></a>Microsoft.DBforPostgreSQL

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | Servergroups | Evet |
> | Sunucu | Evet |
> | sunucular / danışmanlar | Hayır |
> | sunucular / anahtarlar | Hayır |
> | sunucular / privateEndpointConnectionProxies | Hayır |
> | sunucular / privateEndpointConnections | Hayır |
> | sunucular / privateLinkResources | Hayır |
> | sunucular / queryTexts | Hayır |
> | sunucular / recoverableServers | Hayır |
> | sunucular / topQueryStatistics | Hayır |
> | sunucular / virtualNetworkRules | Hayır |
> | sunucular / waitStatistics | Hayır |
> | sunucularv2 | Evet |

## <a name="microsoftdeploymentmanager"></a>Microsoft.DeploymentManager

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | artifactSources | Evet |
> | kullanıma sunulması | Evet |
> | hizmetTopçular | Evet |
> | serviceTopolojiler / hizmetler | Evet |
> | serviceTopolojiler / hizmetler / serviceUnits | Evet |
> | adımlar | Evet |

## <a name="microsoftdesktopvirtualization"></a>Microsoft.DesktopVirtualization

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | uygulama grupları | Evet |
> | uygulama grupları / uygulamalar | Hayır |
> | uygulama grupları / masaüstü bilgisayarlar | Hayır |
> | uygulama grupları / startmenuitems | Hayır |
> | hostpools | Evet |
> | hostpools / sessionhosts | Hayır |
> | hostpools / sessionhosts / usersessions | Hayır |
> | hostpools / usersessions | Hayır |
> | çalışma alanı | Evet |

## <a name="microsoftdevices"></a>Microsoft.Devices

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | Elastik Havuzlar | Evet |
> | ElasticPools / IotHubTenants | Evet |
> | ElasticPools / IotHubTenants / securityAyarlar | Hayır |
> | IotHub'lar | Evet |
> | IotHubs / olayGridFilters | Hayır |
> | IotHubs / securityAyarlar | Hayır |
> | Tedarik Hizmetleri | Evet |
> | Kullanım | Hayır |

## <a name="microsoftdevops"></a>Microsoft.Devops

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | Boru hattı | Evet |

## <a name="microsoftdevspaces"></a>Microsoft.DevSpaces

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | denetleyiciler | Evet |

## <a name="microsoftdevtestlab"></a>Microsoft.DevTestLab

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | laboratuvar merkezleri | Evet |
> | Labs | Evet |
> | laboratuvarlar / ortamlar | Evet |
> | laboratuvarlar / serviceRunners | Evet |
> | laboratuvarlar / virtualMachines | Evet |
> | Zamanlama | Evet |

## <a name="microsoftdocumentdb"></a>Microsoft.DocumentDB

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | veritabanıHesap Adları | Hayır |
> | veritabanıHesapları | Evet |

## <a name="microsoftdomainregistration"></a>Microsoft.DomainRegistration

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | Etki alanları | Evet |
> | etki alanları / domainOwnershipIdentifiers | Hayır |
> | oluştururSsoRequest | Hayır |
> | topLevelDomains | Hayır |
> | doğrulanmasıDomainRegistrationInformation | Hayır |

## <a name="microsoftdynamicslcs"></a>Microsoft.DynamicsLcs

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | lcsprojects | Hayır |
> | lcsprojects / clouddeployments | Hayır |
> | lcsprojects / konektörler | Hayır |

## <a name="microsoftenterpriseknowledgegraph"></a>Microsoft.EnterpriseKnowledgeGraph

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | services | Evet |

## <a name="microsofteventgrid"></a>Microsoft.EventGrid

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | Etki alanları | Evet |
> | etki alanları / konular | Hayır |
> | eventAbonelikler | Hayır |
> | uzantıKonular | Hayır |
> | partnerNamespaces | Evet |
> | partnerNamespaces / eventChannels | Hayır |
> | partnerKayıtlar | Evet |
> | partnerKonular | Evet |
> | partnerKonular / eventSubscriptions | Hayır |
> | systemTopics | Evet |
> | systemKonular / eventSubscriptions | Hayır |
> | konuları | Evet |
> | topicTypes | Hayır |

## <a name="microsofteventhub"></a>Microsoft.EventHub

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | Küme | Evet |
> | Ad alanları | Evet |
> | namespaces / yetkilendirme kuralları | Hayır |
> | ad alanları / disasterrecoveryconfigs | Hayır |
> | ad alanları / eventhubs | Hayır |
> | ad alanları / eventhubs / yetkilendirme kuralları | Hayır |
> | ad alanları / eventhubs / tüketici grupları | Hayır |
> | ad alanları / networkrulesets | Hayır |

## <a name="microsoftfeatures"></a>Microsoft.Özellikler

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | özellikler | Hayır |
> | Sağlayıcı | Hayır |

## <a name="microsoftgallery"></a>Microsoft.Gallery

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | Kayıt | Hayır |
> | galeri öğeleri | Hayır |
> | artifactaccessuri oluşturmak | Hayır |
> | myareas | Hayır |
> | myareas / alanları | Hayır |
> | myareas / alanlar / alanlar | Hayır |
> | myareas / alanlar / alanlar / galeri öğeleri | Hayır |
> | myareas / alanlar / galeri öğeleri | Hayır |
> | myareas / galeri öğeleri | Hayır |
> | register | Hayır |
> | kaynaklar | Hayır |
> | retrieveresourcesbyid | Hayır |

## <a name="microsoftgenomics"></a>Microsoft.Genomik

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | accounts | Evet |

## <a name="microsoftguestconfiguration"></a>Microsoft.GuestConfiguration

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | otomatikYönetilen Hesaplar | Evet |
> | otomatikManagedVmConfigurationProfiles | Evet |
> | configurationProfileAtamaları | Hayır |
> | guestConfigurationAssignments | Hayır |
> | Yazılım | Hayır |
> | softwareUpdateProfile | Hayır |
> | softwareUpdates | Hayır |

## <a name="microsofthanaonazure"></a>Microsoft.HanaOnAzure

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | hanaInstances | Evet |
> | sapMonitörler | Evet |

## <a name="microsofthardwaresecuritymodules"></a>Microsoft.HardwareSecurityModules

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | adanmışHSMs | Evet |

## <a name="microsofthdinsight"></a>Microsoft.HDInsight

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | Küme | Evet |
> | kümeler / uygulamalar | Hayır |

## <a name="microsofthealthcareapis"></a>Microsoft.HealthcareApis

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | services | Evet |

## <a name="microsofthybridcompute"></a>Microsoft.HybridCompute

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | Makine | Evet |
> | makineler / uzantılar | Evet |

## <a name="microsofthybriddata"></a>Microsoft.HybridData

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | dataYöneticiler | Evet |

## <a name="microsofthydra"></a>Microsoft.Hydra

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | Bileşen | Evet |
> | ağScopes | Evet |

## <a name="microsoftimportexport"></a>Microsoft.ImportExport

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | Işleri | Evet |

## <a name="microsoftintune"></a>Microsoft.Intune

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | tanıAyarları | Hayır |
> | diagnosticSettingsCategories | Hayır |

## <a name="microsoftiotcentral"></a>Microsoft.IoTCentral

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | appTemplates | Hayır |
> | IoTApps | Evet |

## <a name="microsoftiotspaces"></a>Microsoft.IoTSpaces

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | Graf | Evet |

## <a name="microsoftkeyvault"></a>Microsoft.KeyVault

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | silinmiş Vaults | Hayır |
> | hsmPools | Evet |
> | Tonoz | Evet |
> | tonozlar / erişimPolitikaları | Hayır |
> | kasalar / olayGridFilters | Hayır |
> | kasalar / sırlar | Hayır |

## <a name="microsoftkusto"></a>Microsoft.Kusto

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | Küme | Evet |
> | kümeler / ekli veritabanı yapılandırmaları | Hayır |
> | kümeler / veritabanları | Hayır |
> | kümeleri / veritabanları / veri bağlantıları | Hayır |
> | kümeleri / veritabanları / eventhubconnections | Hayır |
> | kümeleri / veritabanları / ilkeler atamaları | Hayır |
> | kümeler / ilkeler atamaları | Hayır |
> | kümeler / paylaşılan kimlikler | Hayır |

## <a name="microsoftlabservices"></a>Microsoft.LabServices

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | laboratuvar hesapları | Evet |
> | kullanıcılar | Hayır |

## <a name="microsoftlogic"></a>Microsoft.Logic

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | hostingOrtamlar | Evet |
> | entegrasyonHesapları | Evet |
> | entegrasyonServiceOrtamlar | Evet |
> | entegrasyonServiceEnvironments / managedApis | Evet |
> | izole Ortamlar | Evet |
> | Iş akışı | Evet |

## <a name="microsoftmachinelearning"></a>Microsoft.MachineLearning

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | taahhütPlanlar | Evet |
> | Webservices | Evet |
> | Çalışma Alanları | Evet |

## <a name="microsoftmachinelearningservices"></a>Microsoft.MachineLearningServices

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | çalışma alanı | Evet |
> | çalışma alanları / hesaplamalar | Hayır |
> | çalışma alanları / eventGridFilters | Hayır |

## <a name="microsoftmanagedidentity"></a>Microsoft.ManagedIdentity

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | Kimlik | Hayır |
> | kullanıcıAssignedIdentities | Evet |

## <a name="microsoftmanagedservices"></a>Microsoft.ManagedServices

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | marketplaceRegistrationDefinitions | Hayır |
> | kayıtAtamaları | Hayır |
> | kayıtTanımlar | Hayır |

## <a name="microsoftmanagement"></a>Microsoft.Management

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | kuruluşlar | Hayır |
> | managementGroups | Hayır |
> | managementGroups / ayarlar | Hayır |
> | kaynaklar | Hayır |
> | başlangıçTenantBackfill | Hayır |
> | tenantBackfillStatus | Hayır |

## <a name="microsoftmaps"></a>Microsoft.Haritalar

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | accounts | Evet |
> | hesaplar / eventGridFilters | Hayır |

## <a name="microsoftmarketplace"></a>Microsoft.Marketplace

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | Sunmaktadır | Hayır |
> | offerTypes | Hayır |
> | offerTypes / yayıncılar | Hayır |
> | offerTypes / yayıncılar / teklifler | Hayır |
> | offerTypes / yayıncılar / teklifler / planlar | Hayır |
> | offerTypes / yayıncılar / teklifler / planlar / anlaşmalar | Hayır |
> | offerTypes / yayıncılar / teklifler / planlar / configs | Hayır |
> | offerTypes / yayıncılar / teklifler / planlar / configs / importImage | Hayır |
> | privategalleryitems | Hayır |
> | privateStoreClient | Hayır |
> | Ürünler | Hayır |
> | Yayıncı | Hayır |
> | yayıncılar / teklifler | Hayır |
> | yayıncılar / teklifler / değişiklikler | Hayır |

## <a name="microsoftmarketplaceapps"></a>Microsoft.MarketplaceApps

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | klasikDevServices | Evet |
> | updateİletişimTercihi | Hayır |

## <a name="microsoftmarketplaceordering"></a>Microsoft.MarketplaceSipariş

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | Anlaşma | Hayır |
> | teklif türleri | Hayır |

## <a name="microsoftmedia"></a>Microsoft.Media

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | medya hizmetleri | Evet |
> | mediaservices / accountFilters | Hayır |
> | mediaservices / varlıklar | Hayır |
> | mediaservices / varlıklar / varlıkFiltreler | Hayır |
> | mediaservices / contentKeyPolicies | Hayır |
> | mediaservices / eventGridFilters | Hayır |
> | mediaservices / liveEventOperations | Hayır |
> | mediaservices / liveEvents | Evet |
> | mediaservices / liveEvents / liveOutputs | Hayır |
> | mediaservices / liveOutputOperations | Hayır |
> | mediaservices / mediaGraphs | Hayır |
> | mediaservices / streamingEndpointOperations | Hayır |
> | mediaservices / streamingEndpoints | Evet |
> | mediaservices / streamingLocators | Hayır |
> | mediaservices / streamingPolitikalar | Hayır |
> | mediaservices / dönüştürür | Hayır |
> | mediaservices / dönüşümler / işler | Hayır |

## <a name="microsoftmicroservices4spring"></a>Microsoft.Microservices4Bahar

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | appClusters | Evet |

## <a name="microsoftmigrate"></a>Microsoft.Migrate

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | değerlendirmeProjeler | Evet |
> | göç projeleri | Evet |
> | moveCollections | Evet |
> | Proje | Evet |

## <a name="microsoftmixedreality"></a>Microsoft.MixedReality

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | holographicsYayın Hesapları | Evet |
> | objectUnderstandingAccounts | Evet |
> | remoteRenderingHesapları | Evet |
> | mekansalÇapaLarHesapları | Evet |
> | surfaceReconstructionAccounts | Evet |

## <a name="microsoftnetapp"></a>Microsoft.NetApp

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | netAppHesapları | Evet |
> | netAppAccounts / capacityPools | Evet |
> | netAppAccounts / capacityPools / volumes | Evet |
> | netAppAccounts / capacityPools / volumes / mountTargets | Evet |
> | netAppAccounts / capacityPools / hacimler / anlık görüntüler | Evet |

## <a name="microsoftnotebooks"></a>Microsoft.Notebook'lar

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | NotebookProxies | Hayır |
## <a name="microsoftnetwork"></a>Microsoft.Network

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | uygulamaAğ Ağ Geçitleri | Evet |
> | uygulamaGatewayWebApplicationFirewallPolicies | Evet |
> | uygulamaGüvenlikGrupları | Evet |
> | azureFirewallFqdnEtiketler | Hayır |
> | azureFirewalls | Evet |
> | burcuHosts | Evet |
> | bgpServiceTopluluklar | Hayır |
> | Bağlantı | Evet |
> | ddosCustomPolicies | Evet |
> | ddosProtectionPlans | Evet |
> | dnsOperationStatuses | Hayır |
> | dnszones | Evet |
> | dnszones / A | Hayır |
> | dnszones / AAAA | Hayır |
> | dnszones / tüm | Hayır |
> | dnszones / CAA | Hayır |
> | dnszones / CNAME | Hayır |
> | dnszones / MX | Hayır |
> | dnszones / NS | Hayır |
> | dnszones / PTR | Hayır |
> | dnszones / kayıt setleri | Hayır |
> | dnszones / SOA | Hayır |
> | dnszones / SRV | Hayır |
> | dnszones / TXT | Hayır |
> | expressRouteCircuits | Evet |
> | expressRouteCrossConnections | Evet |
> | expressRouteGateways | Evet |
> | expressRoutePorts | Evet |
> | expressRouteServiceProviders | Hayır |
> | güvenlik duvarıPolitikalar | Evet |
> | ön kapılar | Evet |
> | frontdoorWebApplicationFirewallManagedRuleSets | Hayır |
> | frontdoorWebApplicationFirewallPolicies | Evet |
> | getDnsResourceReference | Hayır |
> | internalNotify | Hayır |
> | yükDengeleyiciler | Evet |
> | yerelAğ Ağ Geçitleri | Evet |
> | natGateways | Evet |
> | ağNiyet Politikaları | Evet |
> | ağArayüzler | Evet |
> | ağProfilleri | Evet |
> | ağSecurityGroups | Evet |
> | ağ Watchers | Evet |
> | ağWatchers / bağlantıMonitörler | Evet |
> | ağ Watchers / lensler | Evet |
> | ağWatchers / pingMeshes | Evet |
> | p2sVpnAğ Geçitleri | Evet |
> | privateDnsOperationStatuses | Hayır |
> | privateDnsZones | Evet |
> | privateDnsZones / A | Hayır |
> | privateDnsZones / AAAA | Hayır |
> | privateDnsZones / tüm | Hayır |
> | privateDnsZones / CNAME | Hayır |
> | privateDnsZones / MX | Hayır |
> | privateDnsZones / PTR | Hayır |
> | privateDnsZones / SOA | Hayır |
> | privateDnsZones / SRV | Hayır |
> | privateDnsZones / TXT | Hayır |
> | privateDnsZones / virtualNetworkLinks | Evet |
> | privateEndpoints | Evet |
> | privateLinkServices | Evet |
> | publicIPAdresleri | Evet |
> | publicIPPrefixes | Evet |
> | rotaFiltreler | Evet |
> | routeTablolar | Evet |
> | serviceEndpointPolitikalar | Evet |
> | trafficManagerCoğrafi Hiyerarşileri | Hayır |
> | trafik yöneticisi profilleri | Evet |
> | trafficmanagerprofiles / heatMaps | Hayır |
> | trafficManagerUserMetricsKeys | Hayır |
> | virtualHub'lar | Evet |
> | virtualNetworkGateways | Evet |
> | sanalAğlar | Evet |
> | virtualNetworkTaps | Evet |
> | sanalWans | Evet |
> | vpnAğ Geçitleri | Evet |
> | vpnSiteler | Evet |
> | webApplicationFirewallPolitikalar | Evet |

## <a name="microsoftnotificationhubs"></a>Microsoft.NotificationHub'lar

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | Ad alanları | Evet |
> | ad alanları / notificationHubs | Evet |

## <a name="microsoftobjectstore"></a>Microsoft.ObjectStore

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | osNamespaces | Evet |

## <a name="microsoftoffazure"></a>Microsoft.OffAzure

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | HyperVSites | Evet |
> | İthalat Siteleri | Evet |
> | Sunucu Siteleri | Evet |
> | VMwareSiteleri | Evet |

## <a name="microsoftoperationalinsights"></a>Microsoft.OperationalInsights

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | Küme | Evet |
> | bağlantıHedefleri | Hayır |
> | depolamaInsightConfigs | Hayır |
> | çalışma alanı | Evet |
> | çalışma alanları / dataExports | Hayır |
> | çalışma alanları / dataSources | Hayır |
> | çalışma alanları / linkedServices | Hayır |
> | çalışma alanları / privateEndpointConnectionProxies | Hayır |
> | çalışma alanları / privateEndpointConnections | Hayır |
> | çalışma alanları / privateLinkResources | Hayır |
> | çalışma alanları / sorgu | Hayır |
> | çalışma alanları / scopedPrivateLinkProxies | Hayır |

## <a name="microsoftoperationsmanagement"></a>Microsoft.OperationsManagement

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | yönetim dernekleri | Hayır |
> | yönetim yapılandırmaları | Evet |
> | çözümler | Evet |
> | görünümler | Evet |

## <a name="microsoftpeering"></a>Microsoft.Peering

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | legacyPeerings | Hayır |
> | peerAsns | Hayır |
> | akranlar | Evet |
> | peeringServiceCountries | Hayır |
> | peeringServiceProviders | Hayır |
> | peeringServices | Evet |

## <a name="microsoftpolicyinsights"></a>Microsoft.PolicyInsights

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | politikaEtkinlikler | Hayır |
> | politikaMeta veriler | Hayır |
> | policyStates | Hayır |
> | policyTrackedResources | Hayır |
> | düzeltmeler | Hayır |

## <a name="microsoftportal"></a>Microsoft.Portal

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> |  konsolları | Hayır |
> | panolar | Evet |
> | userSettings | Hayır |

## <a name="microsoftpowerbi"></a>Microsoft.PowerBI

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | çalışma alanıKoleksiyonlar | Evet |

## <a name="microsoftpowerbidedicated"></a>Microsoft.PowerBIDedicated

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | Kapasite | Evet |

## <a name="microsoftprojectbabylon"></a>Microsoft.ProjectBabylon

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | accounts | Evet |

## <a name="microsoftrecoveryservices"></a>Microsoft.RecoveryServices

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | backupProtectedItems | Hayır |
> | Tonoz | Evet |

## <a name="microsoftrelay"></a>Microsoft.Relay

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | Ad alanları | Evet |
> | namespaces / yetkilendirme kuralları | Hayır |
> | ad alanları / melez bağlantılar | Hayır |
> | ad alanları / hibrit bağlantılar / yetkilendirme kuralları | Hayır |
> | isim boşlukları / wcfrelays | Hayır |
> | namespaces / wcfrelays / yetkilendirme kuralları | Hayır |

## <a name="microsoftremoteapp"></a>Microsoft.RemoteApp

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | accounts | Hayır |
> | Koleksiyon | Evet |
> | koleksiyonlar / uygulamalar | Hayır |
> | koleksiyonlar / güvenlik müdürleri | Hayır |
> | templateImages | Hayır |

## <a name="microsoftresourcegraph"></a>Microsoft.ResourceGraph

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | Sorgu | Evet |
> | kaynakChangeDetails | Hayır |
> | kaynak Değişiklikler | Hayır |
> | kaynaklar | Hayır |
> | kaynaklarTarih | Hayır |
> | aboneliklerDurum | Hayır |

## <a name="microsoftresourcehealth"></a>Microsoft.ResourceHealth

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | availabilityStatuses | Hayır |
> | childAvailabilityStatuses | Hayır |
> | childResources | Hayır |
> | ortaya çıkan sorunlar | Hayır |
> | etkinlikler | Hayır |
> | etkilenen Kaynaklar | Hayır |
> | meta veriler | Hayır |
> | bildirimler | Hayır |

## <a name="microsoftresources"></a>Microsoft.Resources

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | Dağıtım | Hayır |
> | dağıtımlar / işlemler | Hayır |
> | dağıtımScripts | Evet |
> | DeploymentScripts / günlükleri | Hayır |
> | Bağlantı | Hayır |
> | bildirmeResourceJobs | Hayır |
> | Sağlayıcı | Hayır |
> | resourceGroups | Hayır |
> | Abonelik | Hayır |
> | Kiracı | Hayır |

## <a name="microsoftsaas"></a>Microsoft.SaaS

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | uygulamalar | Evet |
> | saasresources | Hayır |

## <a name="microsoftsearch"></a>Microsoft.Search

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | kaynakSağlıkMetadata | Hayır |
> | aramaHizmetleri | Evet |

## <a name="microsoftsecurity"></a>Microsoft.Security

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | adaptifNetworkHardenings | Hayır |
> | gelişmişThreatProtectionSettings | Hayır |
> | alerts | Hayır |
> | izin Verilen Bağlantılar | Hayır |
> | uygulamaBeyaz listeler | Hayır |
> | değerlendirmeMeta veriler | Hayır |
> | değerlendirme | Hayır |
> | autoDismissAlertsRules | Hayır |
> | otomasyon | Evet |
> | Otomatik Sağlama Ayarları | Hayır |
> | Uyumluluklar | Hayır |
> | dataCollectionAgents | Hayır |
> | deviceSecurityGroups | Hayır |
> | keşfedilenSecuritySolutions | Hayır |
> | hariciSecuritySolutions | Hayır |
> | BilgiKoruma Politikaları | Hayır |
> | iotSecuritySolutions | Evet |
> | iotSecuritySolutions / analitikModeller | Hayır |
> | iotSecuritySolutions / analitikModeller / topluuyarılar | Hayır |
> | iotSecuritySolutions / analyticsModeller / topluöneriler | Hayır |
> | jitNetworkAccessPolicies | Hayır |
> | ağData | Hayır |
> | ilkeler | Hayır |
> | fiyatlandırma | Hayır |
> | mevzuatUyumStandartları | Hayır |
> | düzenlemeUyumlulukStandartları / regulatoryComplianceControls | Hayır |
> | düzenlemeUyumlulukStandartlar / düzenleyiciUygunlukDenetimler / düzenleyiciUygunlukDeğerlendirmeleri | Hayır |
> | securityKişiler | Hayır |
> | securitySolutions | Hayır |
> | securitySolutionsReferenceData | Hayır |
> | securityDurumlar | Hayır |
> | securityStatusesÖzetler | Hayır |
> | serverVulnerabilityAssessments | Hayır |
> | ayarlar | Hayır |
> | alt Değerlendirmeler | Hayır |
> | Görevler | Hayır |
> | Topoloji | Hayır |
> | çalışma alanıAyarlar | Hayır |

## <a name="microsoftsecuritygraph"></a>Microsoft.SecurityGraph

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | tanıAyarları | Hayır |
> | diagnosticSettingsCategories | Hayır |

## <a name="microsoftsecurityinsights"></a>Microsoft.SecurityInsights

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | Toplamalardan | Hayır |
> | alertKurallar | Hayır |
> | alertRuleTemplates | Hayır |
> | yer işaretleri | Hayır |
> | Durumda | Hayır |
> | dataConnectors | Hayır |
> | dataConnectorsCheckRequirements | Hayır |
> | Varlık | Hayır |
> | entityQueries | Hayır |
> | Olay | Hayır |
> | ofisOnay | Hayır |
> | ayarlar | Hayır |

## <a name="microsoftservicebus"></a>Microsoft.ServiceBus

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | Ad alanları | Evet |
> | namespaces / yetkilendirme kuralları | Hayır |
> | ad alanları / disasterrecoveryconfigs | Hayır |
> | ad alanları / eventgridfilters | Hayır |
> | ad alanları / networkrulesets | Hayır |
> | ad alanları / kuyruklar | Hayır |
> | ad alanları / kuyruklar / yetkilendirme kuralları | Hayır |
> | namespaces / konular | Hayır |
> | namespaces / konular / yetkilendirme kuralları | Hayır |
> | ad alanları / konular / abonelikler | Hayır |
> | namespaces / konular / abonelikler / kurallar | Hayır |
> | premiumMesajlaşmaBölgeler | Hayır |

## <a name="microsoftservicefabric"></a>Microsoft.ServiceFabric

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | uygulamalar | Evet |
> | Küme | Evet |
> | kümeler / uygulamalar | Hayır |
> | konteynerGruplar | Evet |
> | konteynerGroupSets | Evet |
> | kenar kümeleri | Evet |
> | kenarlı kümeleri / uygulamaları | Hayır |
> | yönetilen kümeler | Evet |
> | yönetilen kümeler / düğüm tipleri | Hayır |
> | Ağlar | Evet |
> | gizli mağazalar | Evet |
> | gizli mağazalar / sertifikalar | Hayır |
> | gizli mağazalar / sırlar | Hayır |
> | volumes | Evet |

## <a name="microsoftservicefabricmesh"></a>Microsoft.ServiceFabricMesh

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | uygulamalar | Evet |
> | konteynerGruplar | Evet |
> | Ağ geçit -leri | Evet |
> | Ağlar | Evet |
> | Sır -larını | Evet |
> | volumes | Evet |

## <a name="microsoftservices"></a>Microsoft.Services

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | sağlayıcıKayıtlar | Hayır |
> | sağlayıcıKayıtlar / resourceTypeRegistrations | Hayır |
> | kullanıma sunulması | Evet |

## <a name="microsoftsignalrservice"></a>Microsoft.SignalRService

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | SignalR | Evet |
> | SignalR / eventGridFilters | Hayır |

## <a name="microsoftsiterecovery"></a>Microsoft.SiteKurtarma

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | SiteRecoveryVault | Evet |

## <a name="microsoftsoftwareplan"></a>Microsoft.SoftwarePlan

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | hybridUseBenefits | Hayır |

## <a name="microsoftsolutions"></a>Microsoft.Solutions

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | uygulamaTanımlar | Evet |
> | uygulamalar | Evet |
> | jitRequests | Evet |

## <a name="microsoftspoolservice"></a>Microsoft.SpoolService

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | kayıtlıAbonelikler | Hayır |
> | Biriktirici | Evet |

## <a name="microsoftsql"></a>Microsoft.SQL

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | yönetilen Örnekler | Evet |
> | yönetilen Örnekler / veritabanları | Evet |
> | yönetilenInstances / veritabanları / yedeklemeShortTermRetentionPolicies | Hayır |
> | yönetilenInstances / veritabanları / şemalar / tablolar / sütunlar / sensitivityLabels | Hayır |
> | yönetilenÖrnekler / veritabanları / güvenlik açığıDeğerlendirmeleri | Hayır |
> | yönetilenInstances / veritabanları / güvenlik açığıDeğerlendirmeler / kurallar / taban çizgileri | Hayır |
> | yönetilenInstances / şifrelemeProtector | Hayır |
> | yönetilenInstances / anahtarlar | Hayır |
> | managedInstances / restorableDroppedDatabases / backupShortTermRetentionPolicies | Hayır |
> | yönetilenInstances / güvenlik açığıDeğerlendirmeler | Hayır |
> | Sunucu | Evet |
> | sunucular / yöneticiler | Hayır |
> | sunucular / iletişimLinkler | Hayır |
> | sunucular / veritabanları | Evet |
> | sunucular / şifrelemeProtector | Hayır |
> | sunucular / firewallRules | Hayır |
> | sunucular / anahtarlar | Hayır |
> | sunucular / restorableDroppedDatabases | Hayır |
> | sunucular / hizmet hedefleri | Hayır |
> | sunucular / tdeCertificates | Hayır |
> | virtualClusters | Hayır |

## <a name="microsoftsqlvirtualmachine"></a>Microsoft.SqlVirtualMachine

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | SqlVirtualMachineGroups | Evet |
> | SqlVirtualMachineGroups / AvailabilityGroupListeners | Hayır |
> | SqlVirtualMachines | Evet |

## <a name="microsoftstorage"></a>Microsoft.Storage

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | depolamaHesapları | Evet |
> | storageAccounts / blobServices | Hayır |
> | storageAccounts / fileServices | Hayır |
> | storageAccounts / queueServices | Hayır |
> | storageAccounts / hizmetleri | Hayır |
> | storageAccounts / hizmetler / metricDefinitions | Hayır |
> | storageAccounts / tableServices | Hayır |
> | Kullanım | Hayır |

## <a name="microsoftstoragecache"></a>Microsoft.StorageÖnbellek

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | Önbellek | Evet |
> | önbellekler / depolamaHedefler | Hayır |
> | kullanımModeller | Hayır |

## <a name="microsoftstoragereplication"></a>Microsoft.StorageÇoğaltma

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | çoğaltmaGrupları | Hayır |

## <a name="microsoftstoragesync"></a>Microsoft.StorageSync

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | depolamaSyncServices | Evet |
> | storageSyncServices / kayıtlıSunucular | Hayır |
> | storageSyncServices / syncGroups | Hayır |
> | storageSyncServices / syncGroups / cloudEndpoints | Hayır |
> | storageSyncServices / syncGroups / serverEndpoints | Hayır |
> | storageSyncServices / iş akışları | Hayır |

## <a name="microsoftstoragesyncdev"></a>Microsoft.StorageSyncDev

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | depolamaSyncServices | Evet |
> | storageSyncServices / kayıtlıSunucular | Hayır |
> | storageSyncServices / syncGroups | Hayır |
> | storageSyncServices / syncGroups / cloudEndpoints | Hayır |
> | storageSyncServices / syncGroups / serverEndpoints | Hayır |
> | storageSyncServices / iş akışları | Hayır |

## <a name="microsoftstoragesyncint"></a>Microsoft.StorageSyncInt

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | depolamaSyncServices | Evet |
> | storageSyncServices / kayıtlıSunucular | Hayır |
> | storageSyncServices / syncGroups | Hayır |
> | storageSyncServices / syncGroups / cloudEndpoints | Hayır |
> | storageSyncServices / syncGroups / serverEndpoints | Hayır |
> | storageSyncServices / iş akışları | Hayır |

## <a name="microsoftstorsimple"></a>Microsoft.StorSimple

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | Yöneticileri | Evet |

## <a name="microsoftstreamanalytics"></a>Microsoft.StreamAnalytics

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | akış işleri | Evet |

## <a name="microsoftsubscription"></a>Microsoft.Abonelik

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | iptal | Hayır |
> | Createsubscription | Hayır |
> | seçin | Hayır |
> | yeniden adlandır | Hayır |
> | AbonelikTanımlar | Hayır |
> | Abonelik İşlemleri | Hayır |

## <a name="microsofttimeseriesinsights"></a>Microsoft.TimeSeriesInsights

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | Ortam | Evet |
> | ortamlar / accessPolitikalar | Hayır |
> | ortamlar / olay kaynakları | Evet |
> | ortamlar / referenceDataSets | Evet |

## <a name="microsoftvmwarecloudsimple"></a>Microsoft.VMwareCloudBasit

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | adanmışCloudNodes | Evet |
> | özelCloudServices | Evet |
> | virtualMachines | Evet |

## <a name="microsoftvnfmanager"></a>Microsoft.VnfManager

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | cihazlar | Evet |
> | kayıtlıAbonelikler | Hayır |
> | Satıcı | Hayır |
> | satıcılar / skus | Hayır |
> | satıcılar / vnfs | Hayır |
> | vnfs | Evet |

## <a name="microsoftweb"></a>Microsoft.Web

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | apiManagementHesapları | Hayır |
> | apiManagementAccounts / apiAcls | Hayır |
> | apiManagementAccounts / apis | Hayır |
> | apiManagementAccounts / apis / apiAcls | Hayır |
> | apiManagementAccounts / apis / bağlantıAcls | Hayır |
> | apiManagementAccounts / apis / bağlantılar | Hayır |
> | apiManagementAccounts / apis / bağlantılar / bağlantılarAcls | Hayır |
> | apiManagementAccounts / apis / localizedDefinitions | Hayır |
> | apiManagementAccounts / bağlantıAcls | Hayır |
> | apiManagementAccounts / bağlantılar | Hayır |
> | faturalamaÖlçerler | Hayır |
> | sertifikalar | Evet |
> | bağlantıAğ Geçitleri | Evet |
> | Bağlantı | Evet |
> | customApis | Evet |
> | silinen Siteler | Hayır |
> | hostingOrtamlar | Evet |
> | hostingOrtamlar / olayGridFilters | Hayır |
> | hostingOrtamlar / multiRolePools | Hayır |
> | hostingOrtamlar / işçiHavuzları | Hayır |
> | publishingKullanıcılar | Hayır |
> | Öneriler | Hayır |
> | kaynakSağlıkMetadata | Hayır |
> | Çalıştırma | Hayır |
> | serverFarms | Evet |
> | serverFarms / eventGridFilters | Hayır |
> | Siteler | Evet |
> | siteler/config  | Hayır |
> | siteler / eventGridFilters | Hayır |
> | siteler / hostNameBindings | Hayır |
> | siteler / ağConfig | Hayır |
> | siteler / premieraddons | Evet |
> | siteler / yuvalar | Evet |
> | siteler / yuvalar / eventGridFilters | Hayır |
> | siteler / yuvalar / hostNameBindings | Hayır |
> | siteler / yuvalar / networkConfig | Hayır |
> | sourceControls | Hayır |
> | staticSites | Evet |
> | Doğrulamak | Hayır |
> | verifyHostingEnvironmentVnet | Hayır |

## <a name="microsoftwindowsdefenderatp"></a>Microsoft.WindowsDefenderATP

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | tanıAyarları | Hayır |
> | diagnosticSettingsCategories | Hayır |

## <a name="microsoftwindowsiot"></a>Microsoft.WindowsIoT

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | Cihaz Hizmetleri | Evet |

## <a name="microsoftworkloadmonitor"></a>Microsoft.WorkloadMonitor

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | Bileşen | Hayır |
> | componentsSummary | Hayır |
> | monitorInstances | Hayır |
> | monitorInstancesSummary | Hayır |
> | Monitör | Hayır |
> | bildirimAyarlar | Hayır |

## <a name="next-steps"></a>Sonraki adımlar

Virgülle ayrılmış değerler dosyasıyla aynı verileri almak için [tam mod-silme.csv'yi](https://github.com/tfitzmac/resource-capabilities/blob/master/complete-mode-deletion.csv)indirin.
