---
title: Tag support for resources
description: Shows which Azure resource types support tags. Provides details for all Azure services.
ms.topic: conceptual
ms.date: 11/22/2019
ms.openlocfilehash: 5b3e26d914887496eedde609404eaf0b380dbcc0
ms.sourcegitcommit: 4c831e768bb43e232de9738b363063590faa0472
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/23/2019
ms.locfileid: "74422130"
---
# <a name="tag-support-for-azure-resources"></a>Tag support for Azure resources
This article describes whether a resource type supports [tags](resource-group-using-tags.md). The column labeled **Supports tags** indicates whether the resource type has a property for the tag. The column labeled **Tag in cost report** indicates whether that resource type passes the tag to the cost report. You can view costs by tags in the [Cost Management cost analysis](../cost-management/quick-acm-cost-analysis.md#understanding-grouping-and-filtering-options) and the [Azure billing invoice and daily usage data](../billing/billing-download-azure-invoice-daily-usage-date.md).

To get the same data as a file of comma-separated values, download [tag-support.csv](https://github.com/tfitzmac/resource-capabilities/blob/master/tag-support.csv).

Jump to a resource provider namespace:
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
> - [Microsoft.Azure.Geneva](#microsoftazuregeneva)
> - [Microsoft.AzureActiveDirectory](#microsoftazureactivedirectory)
> - [Microsoft.AzureData](#microsoftazuredata)
> - [Microsoft.AzureStack](#microsoftazurestack)
> - [Microsoft.Batch](#microsoftbatch)
> - [Microsoft.Billing](#microsoftbilling)
> - [Microsoft.BingMaps](#microsoftbingmaps)
> - [Microsoft.Blockchain](#microsoftblockchain)
> - [Microsoft.Blueprint](#microsoftblueprint)
> - [Microsoft.BotService](#microsoftbotservice)
> - [Microsoft.Cache](#microsoftcache)
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
> - [Microsoft.DevOps](#microsoftdevops)
> - [Microsoft.DevSpaces](#microsoftdevspaces)
> - [Microsoft.DevTestLab](#microsoftdevtestlab)
> - [Microsoft.DocumentDB](#microsoftdocumentdb)
> - [Microsoft.DomainRegistration](#microsoftdomainregistration)
> - [Microsoft.DynamicsLcs](#microsoftdynamicslcs)
> - [Microsoft.EnterpriseKnowledgeGraph](#microsoftenterpriseknowledgegraph)
> - [Microsoft.EventGrid](#microsofteventgrid)
> - [Microsoft.EventHub](#microsofteventhub)
> - [Microsoft.Features](#microsoftfeatures)
> - [Microsoft.Gallery](#microsoftgallery)
> - [Microsoft.Genomics](#microsoftgenomics)
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
> - [Microsoft.Maps](#microsoftmaps)
> - [Microsoft.Marketplace](#microsoftmarketplace)
> - [Microsoft.MarketplaceApps](#microsoftmarketplaceapps)
> - [Microsoft.MarketplaceOrdering](#microsoftmarketplaceordering)
> - [Microsoft.Media](#microsoftmedia)
> - [Microsoft.Microservices4Spring](#microsoftmicroservices4spring)
> - [Microsoft.Migrate](#microsoftmigrate)
> - [Microsoft.MixedReality](#microsoftmixedreality)
> - [Microsoft.NetApp](#microsoftnetapp)
> - [Microsoft.Network](#microsoftnetwork)
> - [Microsoft.NotificationHubs](#microsoftnotificationhubs)
> - [Microsoft.ObjectStore](#microsoftobjectstore)
> - [Microsoft.OffAzure](#microsoftoffazure)
> - [Microsoft.OperationalInsights](#microsoftoperationalinsights)
> - [Microsoft.OperationsManagement](#microsoftoperationsmanagement)
> - [Microsoft.Peering](#microsoftpeering)
> - [Microsoft.PolicyInsights](#microsoftpolicyinsights)
> - [Microsoft.Portal](#microsoftportal)
> - [Microsoft.PowerBI](#microsoftpowerbi)
> - [Microsoft.PowerBIDedicated](#microsoftpowerbidedicated)
> - [Microsoft.RecoveryServices](#microsoftrecoveryservices)
> - [Microsoft.Relay](#microsoftrelay)
> - [Microsoft.RemoteApp](#microsoftremoteapp)
> - [Microsoft.ResourceGraph](#microsoftresourcegraph)
> - [Microsoft.ResourceHealth](#microsoftresourcehealth)
> - [Microsoft.Resources](#microsoftresources)
> - [Microsoft.SaaS](#microsoftsaas)
> - [Microsoft.Scheduler](#microsoftscheduler)
> - [Microsoft.Search](#microsoftsearch)
> - [Microsoft.Security](#microsoftsecurity)
> - [Microsoft.SecurityGraph](#microsoftsecuritygraph)
> - [Microsoft.SecurityInsights](#microsoftsecurityinsights)
> - [Microsoft.ServiceBus](#microsoftservicebus)
> - [Microsoft.ServiceFabric](#microsoftservicefabric)
> - [Microsoft.ServiceFabricMesh](#microsoftservicefabricmesh)
> - [Microsoft.Services](#microsoftservices)
> - [Microsoft.SignalRService](#microsoftsignalrservice)
> - [Microsoft.SiteRecovery](#microsoftsiterecovery)
> - [Microsoft.SoftwarePlan](#microsoftsoftwareplan)
> - [Microsoft.Solutions](#microsoftsolutions)
> - [Microsoft.SQL](#microsoftsql)
> - [Microsoft.SqlVirtualMachine](#microsoftsqlvirtualmachine)
> - [Microsoft.Storage](#microsoftstorage)
> - [Microsoft.StorageCache](#microsoftstoragecache)
> - [Microsoft.StorageReplication](#microsoftstoragereplication)
> - [Microsoft.StorageSync](#microsoftstoragesync)
> - [Microsoft.StorageSyncDev](#microsoftstoragesyncdev)
> - [Microsoft.StorageSyncInt](#microsoftstoragesyncint)
> - [Microsoft.StorSimple](#microsoftstorsimple)
> - [Microsoft.StreamAnalytics](#microsoftstreamanalytics)
> - [Microsoft.Subscription](#microsoftsubscription)
> - [Microsoft.TimeSeriesInsights](#microsofttimeseriesinsights)
> - [Microsoft.VMwareCloudSimple](#microsoftvmwarecloudsimple)
> - [Microsoft.Web](#microsoftweb)
> - [Microsoft.WindowsDefenderATP](#microsoftwindowsdefenderatp)
> - [Microsoft.WindowsIoT](#microsoftwindowsiot)
> - [Microsoft.WorkloadMonitor](#microsoftworkloadmonitor)

## <a name="microsoftaad"></a>Microsoft.AAD

> [!div class="mx-tableFixed"]
> | Kaynak türü | Supports tags | Tag in cost report |
> | ------------- | ----------- | ----------- |
> | DomainServices | Yes | Yes |
> | DomainServices / oucontainer | Hayır | Hayır |

## <a name="microsoftaddons"></a>Microsoft.Addons

> [!div class="mx-tableFixed"]
> | Kaynak türü | Supports tags | Tag in cost report |
> | ------------- | ----------- | ----------- |
> | supportProviders | Hayır | Hayır |

## <a name="microsoftadhybridhealthservice"></a>Microsoft.ADHybridHealthService

> [!div class="mx-tableFixed"]
> | Kaynak türü | Supports tags | Tag in cost report |
> | ------------- | ----------- | ----------- |
> | aadsupportcases | Hayır | Hayır |
> | addsservices | Hayır | Hayır |
> | agents | Hayır | Hayır |
> | anonymousapiusers | Hayır | Hayır |
> | yapılandırma | Hayır | Hayır |
> | günlükler | Hayır | Hayır |
> | reports | Hayır | Hayır |
> | servicehealthmetrics | Hayır | Hayır |
> | services | Hayır | Hayır |

## <a name="microsoftadvisor"></a>Microsoft.Advisor

> [!div class="mx-tableFixed"]
> | Kaynak türü | Supports tags | Tag in cost report |
> | ------------- | ----------- | ----------- |
> | configurations | Hayır | Hayır |
> | generateRecommendations | Hayır | Hayır |
> | meta veriler | Hayır | Hayır |
> | recommendations | Hayır | Hayır |
> | suppressions | Hayır | Hayır |

## <a name="microsoftalertsmanagement"></a>Microsoft.AlertsManagement

> [!div class="mx-tableFixed"]
> | Kaynak türü | Supports tags | Tag in cost report |
> | ------------- | ----------- | ----------- |
> | actionRules | Yes | Yes |
> | alerts | Hayır | Hayır |
> | alertsList | Hayır | Hayır |
> | alertsMetaData | Hayır | Hayır |
> | alertsSummary | Hayır | Hayır |
> | alertsSummaryList | Hayır | Hayır |
> | feedback | Hayır | Hayır |
> | smartDetectorAlertRules | Yes | Yes |
> | smartDetectorRuntimeEnvironments | Hayır | Hayır |
> | smartGroups | Hayır | Hayır |

## <a name="microsoftanalysisservices"></a>Microsoft.AnalysisServices

> [!div class="mx-tableFixed"]
> | Kaynak türü | Supports tags | Tag in cost report |
> | ------------- | ----------- | ----------- |
> | sunucular | Yes | Yes |

## <a name="microsoftapimanagement"></a>Microsoft.ApiManagement

> [!div class="mx-tableFixed"]
> | Kaynak türü | Supports tags | Tag in cost report |
> | ------------- | ----------- | ----------- |
> | reportFeedback | Hayır | Hayır |
> | hizmet | Yes | Yes |
> | validateServiceName | Hayır | Hayır |

## <a name="microsoftappconfiguration"></a>Microsoft.AppConfiguration

> [!div class="mx-tableFixed"]
> | Kaynak türü | Supports tags | Tag in cost report |
> | ------------- | ----------- | ----------- |
> | configurationStores | Yes | Yes |
> | configurationStores / eventGridFilters | Hayır | Hayır |

## <a name="microsoftappplatform"></a>Microsoft.AppPlatform

> [!div class="mx-tableFixed"]
> | Kaynak türü | Supports tags | Tag in cost report |
> | ------------- | ----------- | ----------- |
> | Spring | Yes | Yes |

## <a name="microsoftattestation"></a>Microsoft.Attestation

> [!div class="mx-tableFixed"]
> | Kaynak türü | Supports tags | Tag in cost report |
> | ------------- | ----------- | ----------- |
> | attestationProviders | Hayır | Hayır |

## <a name="microsoftauthorization"></a>Microsoft.Authorization

> [!div class="mx-tableFixed"]
> | Kaynak türü | Supports tags | Tag in cost report |
> | ------------- | ----------- | ----------- |
> | classicAdministrators | Hayır | Hayır |
> | dataAliases | Hayır | Hayır |
> | denyAssignments | Hayır | Hayır |
> | elevateAccess | Hayır | Hayır |
> | findOrphanRoleAssignments | Hayır | Hayır |
> | locks | Hayır | Hayır |
> | permissions | Hayır | Hayır |
> | policyAssignments | Hayır | Hayır |
> | policyDefinitions | Hayır | Hayır |
> | policySetDefinitions | Hayır | Hayır |
> | providerOperations | Hayır | Hayır |
> | roleAssignments | Hayır | Hayır |
> | roleDefinitions | Hayır | Hayır |

## <a name="microsoftautomation"></a>Microsoft.Automation

> [!div class="mx-tableFixed"]
> | Kaynak türü | Supports tags | Tag in cost report |
> | ------------- | ----------- | ----------- |
> | automationAccounts | Yes | Yes |
> | automationAccounts / configurations | Yes | Yes |
> | automationAccounts / jobs | Hayır | Hayır |
> | automationAccounts / runbooks | Yes | Yes |
> | automationAccounts / softwareUpdateConfigurations | Hayır | Hayır |
> | automationAccounts / webhooks | Hayır | Hayır |

## <a name="microsoftazconfig"></a>Microsoft.Azconfig

> [!div class="mx-tableFixed"]
> | Kaynak türü | Supports tags | Tag in cost report |
> | ------------- | ----------- | ----------- |
> | configurationStores | Yes | Yes |
> | configurationStores / eventGridFilters | Hayır | Hayır |

## <a name="microsoftazuregeneva"></a>Microsoft.Azure.Geneva

> [!div class="mx-tableFixed"]
> | Kaynak türü | Supports tags | Tag in cost report |
> | ------------- | ----------- | ----------- |
> | environments | Hayır | Hayır |
> | environments / accounts | Hayır | Hayır |
> | environments / accounts / namespaces | Hayır | Hayır |
> | environments / accounts / namespaces / configurations | Hayır | Hayır |

## <a name="microsoftazureactivedirectory"></a>Microsoft.AzureActiveDirectory

> [!div class="mx-tableFixed"]
> | Kaynak türü | Supports tags | Tag in cost report |
> | ------------- | ----------- | ----------- |
> | b2cDirectories | Yes | Hayır |
> | b2ctenants | Hayır | Hayır |

## <a name="microsoftazuredata"></a>Microsoft.AzureData

> [!div class="mx-tableFixed"]
> | Kaynak türü | Supports tags | Tag in cost report |
> | ------------- | ----------- | ----------- |
> | hybridDataManagers | Yes | Yes |
> | postgresInstances | Yes | Yes |
> | sqlBigDataClusters | Yes | Yes |
> | sqlInstances | Yes | Yes |
> | sqlServerRegistrations | Yes | Yes |
> | sqlServerRegistrations / sqlServers | Hayır | Hayır |

## <a name="microsoftazurestack"></a>Microsoft.AzureStack

> [!div class="mx-tableFixed"]
> | Kaynak türü | Supports tags | Tag in cost report |
> | ------------- | ----------- | ----------- |
> | registrations | Yes | Yes |
> | registrations / customerSubscriptions | Hayır | Hayır |
> | registrations / products | Hayır | Hayır |
> | verificationKeys | Hayır | Hayır |

## <a name="microsoftbatch"></a>Microsoft.Batch

> [!div class="mx-tableFixed"]
> | Kaynak türü | Supports tags | Tag in cost report |
> | ------------- | ----------- | ----------- |
> | batchAccounts | Yes | Yes |

## <a name="microsoftbilling"></a>Microsoft.Billing

> [!div class="mx-tableFixed"]
> | Kaynak türü | Supports tags | Tag in cost report |
> | ------------- | ----------- | ----------- |
> | billingAccounts | Hayır | Hayır |
> | billingAccounts / agreements | Hayır | Hayır |
> | billingAccounts / billingPermissions | Hayır | Hayır |
> | billingAccounts / billingProfiles | Hayır | Hayır |
> | billingAccounts / billingProfiles / billingPermissions | Hayır | Hayır |
> | billingAccounts / billingProfiles / billingRoleAssignments | Hayır | Hayır |
> | billingAccounts / billingProfiles / billingRoleDefinitions | Hayır | Hayır |
> | billingAccounts / billingProfiles / billingSubscriptions | Hayır | Hayır |
> | billingAccounts / billingProfiles / createBillingRoleAssignment | Hayır | Hayır |
> | billingAccounts / billingProfiles / customers | Hayır | Hayır |
> | billingAccounts / billingProfiles / invoices | Hayır | Hayır |
> | billingAccounts / billingProfiles / invoices / pricesheet | Hayır | Hayır |
> | billingAccounts / billingProfiles / invoiceSections | Hayır | Hayır |
> | billingAccounts / billingProfiles / invoiceSections / billingPermissions | Hayır | Hayır |
> | billingAccounts / billingProfiles / invoiceSections / billingRoleAssignments | Hayır | Hayır |
> | billingAccounts / billingProfiles / invoiceSections / billingRoleDefinitions | Hayır | Hayır |
> | billingAccounts / billingProfiles / invoiceSections / billingSubscriptions | Hayır | Hayır |
> | billingAccounts / billingProfiles / invoiceSections / createBillingRoleAssignment | Hayır | Hayır |
> | billingAccounts / billingProfiles / invoiceSections / initiateTransfer | Hayır | Hayır |
> | billingAccounts / billingProfiles / invoiceSections / products | Hayır | Hayır |
> | billingAccounts / billingProfiles / invoiceSections / products / transfer | Hayır | Hayır |
> | billingAccounts / billingProfiles / invoiceSections / products / updateAutoRenew | Hayır | Hayır |
> | billingAccounts / billingProfiles / invoiceSections / transactions | Hayır | Hayır |
> | billingAccounts / billingProfiles / invoiceSections / transfers | Hayır | Hayır |
> | billingAccounts / BillingProfiles / patchOperations | Hayır | Hayır |
> | billingAccounts / billingProfiles / paymentMethods | Hayır | Hayır |
> | billingAccounts / billingProfiles / policies | Hayır | Hayır |
> | billingAccounts / billingProfiles / pricesheet | Hayır | Hayır |
> | billingAccounts / billingProfiles / pricesheetDownloadOperations | Hayır | Hayır |
> | billingAccounts / billingProfiles / products | Hayır | Hayır |
> | billingAccounts / billingProfiles / transactions | Hayır | Hayır |
> | billingAccounts / billingRoleAssignments | Hayır | Hayır |
> | billingAccounts / billingRoleDefinitions | Hayır | Hayır |
> | billingAccounts / billingSubscriptions | Hayır | Hayır |
> | billingAccounts / billingSubscriptions / invoices | Hayır | Hayır |
> | billingAccounts / createBillingRoleAssignment | Hayır | Hayır |
> | billingAccounts / createInvoiceSectionOperations | Hayır | Hayır |
> | billingAccounts / customers | Hayır | Hayır |
> | billingAccounts / customers / billingPermissions | Hayır | Hayır |
> | billingAccounts / customers / billingSubscriptions | Hayır | Hayır |
> | billingAccounts / customers / initiateTransfer | Hayır | Hayır |
> | billingAccounts / customers / policies | Hayır | Hayır |
> | billingAccounts / customers / products | Hayır | Hayır |
> | billingAccounts / customers / transactions | Hayır | Hayır |
> | billingAccounts / customers / transfers | Hayır | Hayır |
> | billingAccounts / departments | Hayır | Hayır |
> | billingAccounts / enrollmentAccounts | Hayır | Hayır |
> | billingAccounts / invoices | Hayır | Hayır |
> | billingAccounts / invoiceSections | Hayır | Hayır |
> | billingAccounts / invoiceSections / billingSubscriptionMoveOperations | Hayır | Hayır |
> | billingAccounts / invoiceSections / billingSubscriptions | Hayır | Hayır |
> | billingAccounts / invoiceSections / billingSubscriptions / transfer | Hayır | Hayır |
> | billingAccounts / invoiceSections / elevate | Hayır | Hayır |
> | billingAccounts / invoiceSections / initiateTransfer | Hayır | Hayır |
> | billingAccounts / invoiceSections / patchOperations | Hayır | Hayır |
> | billingAccounts / invoiceSections / productMoveOperations | Hayır | Hayır |
> | billingAccounts / invoiceSections / products | Hayır | Hayır |
> | billingAccounts / invoiceSections / products / transfer | Hayır | Hayır |
> | billingAccounts / invoiceSections / products / updateAutoRenew | Hayır | Hayır |
> | billingAccounts / invoiceSections / transactions | Hayır | Hayır |
> | billingAccounts / invoiceSections / transfers | Hayır | Hayır |
> | billingAccounts / lineOfCredit | Hayır | Hayır |
> | billingAccounts / patchOperations | Hayır | Hayır |
> | billingAccounts / paymentMethods | Hayır | Hayır |
> | billingAccounts / products | Hayır | Hayır |
> | billingAccounts / transactions | Hayır | Hayır |
> | billingPeriods | Hayır | Hayır |
> | billingPermissions | Hayır | Hayır |
> | billingProperty | Hayır | Hayır |
> | billingRoleAssignments | Hayır | Hayır |
> | billingRoleDefinitions | Hayır | Hayır |
> | createBillingRoleAssignment | Hayır | Hayır |
> | departments | Hayır | Hayır |
> | enrollmentAccounts | Hayır | Hayır |
> | invoices | Hayır | Hayır |
> | transfers | Hayır | Hayır |
> | transfers / acceptTransfer | Hayır | Hayır |
> | transfers / declineTransfer | Hayır | Hayır |
> | transfers / operationStatus | Hayır | Hayır |
> | transfers / validateTransfer | Hayır | Hayır |
> | validateAddress | Hayır | Hayır |

## <a name="microsoftbingmaps"></a>Microsoft.BingMaps

> [!div class="mx-tableFixed"]
> | Kaynak türü | Supports tags | Tag in cost report |
> | ------------- | ----------- | ----------- |
> | mapApis | Yes | Yes |
> | updateCommunicationPreference | Hayır | Hayır |

## <a name="microsoftblockchain"></a>Microsoft.Blockchain

> [!div class="mx-tableFixed"]
> | Kaynak türü | Supports tags | Tag in cost report |
> | ------------- | ----------- | ----------- |
> | blockchainMembers | Yes | Yes |
> | cordaMembers | Yes | Yes |
> | izleyici | Yes | Yes |

## <a name="microsoftblueprint"></a>Microsoft.Blueprint

> [!div class="mx-tableFixed"]
> | Kaynak türü | Supports tags | Tag in cost report |
> | ------------- | ----------- | ----------- |
> | blueprintAssignments | Hayır | Hayır |
> | blueprintAssignments / assignmentOperations | Hayır | Hayır |
> | blueprintAssignments / operations | Hayır | Hayır |
> | blueprints | Hayır | Hayır |
> | blueprints / artifacts | Hayır | Hayır |
> | blueprints / versions | Hayır | Hayır |
> | blueprints / versions / artifacts | Hayır | Hayır |

## <a name="microsoftbotservice"></a>Microsoft.BotService

> [!div class="mx-tableFixed"]
> | Kaynak türü | Supports tags | Tag in cost report |
> | ------------- | ----------- | ----------- |
> | botServices | Yes | Yes |
> | botServices / channels | Hayır | Hayır |
> | botServices / connections | Hayır | Hayır |
> | languages | Hayır | Hayır |
> | şablonları | Hayır | Hayır |

## <a name="microsoftcache"></a>Microsoft.Cache

> [!div class="mx-tableFixed"]
> | Kaynak türü | Supports tags | Tag in cost report |
> | ------------- | ----------- | ----------- |
> | Redis | Yes | Yes |
> | RedisConfigDefinition | Hayır | Hayır |

## <a name="microsoftcapacity"></a>Microsoft.Capacity

> [!div class="mx-tableFixed"]
> | Kaynak türü | Supports tags | Tag in cost report |
> | ------------- | ----------- | ----------- |
> | appliedReservations | Hayır | Hayır |
> | calculateExchange | Hayır | Hayır |
> | calculatePrice | Hayır | Hayır |
> | calculatePurchasePrice | Hayır | Hayır |
> | catalogs | Hayır | Hayır |
> | commercialReservationOrders | Hayır | Hayır |
> | exchange | Hayır | Hayır |
> | placePurchaseOrder | Hayır | Hayır |
> | reservationOrders | Hayır | Hayır |
> | reservationOrders / calculateRefund | Hayır | Hayır |
> | reservationOrders / merge | Hayır | Hayır |
> | reservationOrders / reservations | Hayır | Hayır |
> | reservationOrders / reservations / revisions | Hayır | Hayır |
> | reservationOrders / return | Hayır | Hayır |
> | reservationOrders / split | Hayır | Hayır |
> | reservationOrders / swap | Hayır | Hayır |
> | reservations | Hayır | Hayır |
> | resources | Hayır | Hayır |
> | validateReservationOrder | Hayır | Hayır |

## <a name="microsoftcdn"></a>Microsoft.Cdn

> [!div class="mx-tableFixed"]
> | Kaynak türü | Supports tags | Tag in cost report |
> | ------------- | ----------- | ----------- |
> | CdnWebApplicationFirewallManagedRuleSets | Hayır | Hayır |
> | CdnWebApplicationFirewallPolicies | Yes | Yes |
> | edgenodes | Hayır | Hayır |
> | profiles | Yes | Yes |
> | profiles / endpoints | Yes | Yes |
> | profiles / endpoints / customdomains | Hayır | Hayır |
> | profiles / endpoints / origins | Hayır | Hayır |
> | validateProbe | Hayır | Hayır |

## <a name="microsoftcertificateregistration"></a>Microsoft.CertificateRegistration

> [!div class="mx-tableFixed"]
> | Kaynak türü | Supports tags | Tag in cost report |
> | ------------- | ----------- | ----------- |
> | certificateOrders | Yes | Yes |
> | certificateOrders / certificates | Hayır | Hayır |
> | validateCertificateRegistrationInformation | Hayır | Hayır |

## <a name="microsoftclassiccompute"></a>Microsoft.ClassicCompute

> [!div class="mx-tableFixed"]
> | Kaynak türü | Supports tags | Tag in cost report |
> | ------------- | ----------- | ----------- |
> | capabilities | Hayır | Hayır |
> | domainNames | Hayır | Hayır |
> | domainNames / capabilities | Hayır | Hayır |
> | domainNames / internalLoadBalancers | Hayır | Hayır |
> | domainNames / serviceCertificates | Hayır | Hayır |
> | domainNames / slots | Hayır | Hayır |
> | domainNames / slots / roles | Hayır | Hayır |
> | domainNames / slots / roles / metricDefinitions | Hayır | Hayır |
> | domainNames / slots / roles / metrics | Hayır | Hayır |
> | moveSubscriptionResources | Hayır | Hayır |
> | operatingSystemFamilies | Hayır | Hayır |
> | operatingSystems | Hayır | Hayır |
> | quotas | Hayır | Hayır |
> | resourceTypes | Hayır | Hayır |
> | validateSubscriptionMoveAvailability | Hayır | Hayır |
> | virtualMachines | Hayır | Hayır |
> | virtualMachines / diagnosticSettings | Hayır | Hayır |
> | virtualMachines / metricDefinitions | Hayır | Hayır |
> | virtualMachines / metrics | Hayır | Hayır |

## <a name="microsoftclassicinfrastructuremigrate"></a>Microsoft.ClassicInfrastructureMigrate

> [!div class="mx-tableFixed"]
> | Kaynak türü | Supports tags | Tag in cost report |
> | ------------- | ----------- | ----------- |
> | classicInfrastructureResources | Hayır | Hayır |

## <a name="microsoftclassicnetwork"></a>Microsoft.ClassicNetwork

> [!div class="mx-tableFixed"]
> | Kaynak türü | Supports tags | Tag in cost report |
> | ------------- | ----------- | ----------- |
> | capabilities | Hayır | Hayır |
> | expressRouteCrossConnections | Hayır | Hayır |
> | expressRouteCrossConnections / peerings | Hayır | Hayır |
> | gatewaySupportedDevices | Hayır | Hayır |
> | networkSecurityGroups | Hayır | Hayır |
> | quotas | Hayır | Hayır |
> | reservedIps | Hayır | Hayır |
> | virtualNetworks | Hayır | Hayır |
> | virtualNetworks / remoteVirtualNetworkPeeringProxies | Hayır | Hayır |
> | virtualNetworks / virtualNetworkPeerings | Hayır | Hayır |

## <a name="microsoftclassicstorage"></a>Microsoft.ClassicStorage

> [!div class="mx-tableFixed"]
> | Kaynak türü | Supports tags | Tag in cost report |
> | ------------- | ----------- | ----------- |
> | capabilities | Hayır | Hayır |
> | disks | Hayır | Hayır |
> | images | Hayır | Hayır |
> | osImages | Hayır | Hayır |
> | osPlatformImages | Hayır | Hayır |
> | publicImages | Hayır | Hayır |
> | quotas | Hayır | Hayır |
> | storageAccounts | Hayır | Hayır |
> | storageAccounts / blobServices | Hayır | Hayır |
> | storageAccounts / fileServices | Hayır | Hayır |
> | storageAccounts / metricDefinitions | Hayır | Hayır |
> | storageAccounts / metrics | Hayır | Hayır |
> | storageAccounts / queueServices | Hayır | Hayır |
> | storageAccounts / services | Hayır | Hayır |
> | storageAccounts / services / diagnosticSettings | Hayır | Hayır |
> | storageAccounts / services / metricDefinitions | Hayır | Hayır |
> | storageAccounts / services / metrics | Hayır | Hayır |
> | storageAccounts / tableServices | Hayır | Hayır |
> | storageAccounts / vmImages | Hayır | Hayır |
> | vmImages | Hayır | Hayır |

## <a name="microsoftcognitiveservices"></a>Microsoft.CognitiveServices

> [!div class="mx-tableFixed"]
> | Kaynak türü | Supports tags | Tag in cost report |
> | ------------- | ----------- | ----------- |
> | accounts | Yes | Yes |

## <a name="microsoftcommerce"></a>Microsoft.Commerce

> [!div class="mx-tableFixed"]
> | Kaynak türü | Supports tags | Tag in cost report |
> | ------------- | ----------- | ----------- |
> | RateCard | Hayır | Hayır |
> | UsageAggregates | Hayır | Hayır |

## <a name="microsoftcompute"></a>Microsoft.Compute

> [!div class="mx-tableFixed"]
> | Kaynak türü | Supports tags | Tag in cost report |
> | ------------- | ----------- | ----------- |
> | availabilitySets | Yes | Yes |
> | diskEncryptionSets | Yes | Yes |
> | disks | Yes | Yes |
> | galleries | Yes | Yes |
> | galleries / applications | Hayır | Hayır |
> | galleries / applications / versions | Hayır | Hayır |
> | galleries / images | Hayır | Hayır |
> | galleries / images / versions | Hayır | Hayır |
> | hostGroups | Yes | Yes |
> | hostGroups / hosts | Yes | Yes |
> | images | Yes | Yes |
> | proximityPlacementGroups | Yes | Yes |
> | restorePointCollections | Yes | Yes |
> | restorePointCollections / restorePoints | Hayır | Hayır |
> | sharedVMImages | Yes | Yes |
> | sharedVMImages / versions | Hayır | Hayır |
> | anlık görüntüler | Yes | Yes |
> | virtualMachines | Yes | Yes |
> | virtualMachines / extensions | Yes | Yes |
> | virtualMachines / metricDefinitions | Hayır | Hayır |
> | virtualMachineScaleSets | Yes | Yes |
> | virtualMachineScaleSets / extensions | Hayır | Hayır |
> | virtualMachineScaleSets / networkInterfaces | Hayır | Hayır |
> | virtualMachineScaleSets / publicIPAddresses | Hayır | Hayır |
> | virtualMachineScaleSets / virtualMachines | Hayır | Hayır |
> | virtualMachineScaleSets / virtualMachines / networkInterfaces | Hayır | Hayır |

## <a name="microsoftconsumption"></a>Microsoft.Consumption

> [!div class="mx-tableFixed"]
> | Kaynak türü | Supports tags | Tag in cost report |
> | ------------- | ----------- | ----------- |
> | AggregatedCost | Hayır | Hayır |
> | Bakiyeler | Hayır | Hayır |
> | Bütçeler | Hayır | Hayır |
> | Ücretler | Hayır | Hayır |
> | CostTags | Hayır | Hayır |
> | credits | Hayır | Hayır |
> | etkinlikler | Hayır | Hayır |
> | Tahminler | Hayır | Hayır |
> | lots | Hayır | Hayır |
> | Marketplaces | Hayır | Hayır |
> | Pricesheets | Hayır | Hayır |
> | products | Hayır | Hayır |
> | ReservationDetails | Hayır | Hayır |
> | ReservationRecommendations | Hayır | Hayır |
> | ReservationSummaries | Hayır | Hayır |
> | ReservationTransactions | Hayır | Hayır |
> | Etiketler | Hayır | Hayır |
> | tenants | Hayır | Hayır |
> | Koşullar | Hayır | Hayır |
> | UsageDetails | Hayır | Hayır |

## <a name="microsoftcontainerinstance"></a>Microsoft.ContainerInstance

> [!div class="mx-tableFixed"]
> | Kaynak türü | Supports tags | Tag in cost report |
> | ------------- | ----------- | ----------- |
> | containerGroups | Yes | Yes |
> | serviceAssociationLinks | Hayır | Hayır |

## <a name="microsoftcontainerregistry"></a>Microsoft.ContainerRegistry

> [!div class="mx-tableFixed"]
> | Kaynak türü | Supports tags | Tag in cost report |
> | ------------- | ----------- | ----------- |
> | registries | Yes | Yes |
> | registries / builds | Hayır | Hayır |
> | registries / builds / cancel | Hayır | Hayır |
> | registries / builds / getLogLink | Hayır | Hayır |
> | registries / buildTasks | Yes | Yes |
> | registries / buildTasks / steps | Hayır | Hayır |
> | registries / eventGridFilters | Hayır | Hayır |
> | registries / generateCredentials | Hayır | Hayır |
> | registries / getBuildSourceUploadUrl | Hayır | Hayır |
> | registries / GetCredentials | Hayır | Hayır |
> | registries / importImage | Hayır | Hayır |
> | registries / queueBuild | Hayır | Hayır |
> | registries / regenerateCredential | Hayır | Hayır |
> | registries / regenerateCredentials | Hayır | Hayır |
> | registries / replications | Yes | Yes |
> | registries / runs | Hayır | Hayır |
> | registries / runs / cancel | Hayır | Hayır |
> | registries / scheduleRun | Hayır | Hayır |
> | registries / scopeMaps | Hayır | Hayır |
> | registries / taskRuns | Yes | Yes |
> | registries / tasks | Yes | Yes |
> | registries / tokens | Hayır | Hayır |
> | registries / updatePolicies | Hayır | Hayır |
> | registries / webhooks | Yes | Yes |
> | registries / webhooks / getCallbackConfig | Hayır | Hayır |
> | registries / webhooks / ping | Hayır | Hayır |

## <a name="microsoftcontainerservice"></a>Microsoft.ContainerService

> [!div class="mx-tableFixed"]
> | Kaynak türü | Supports tags | Tag in cost report |
> | ------------- | ----------- | ----------- |
> | containerServices | Yes | Yes |
> | managedClusters | Yes | Yes |
> | openShiftManagedClusters | Yes | Yes |

## <a name="microsoftcortanaanalytics"></a>Microsoft.CortanaAnalytics

> [!div class="mx-tableFixed"]
> | Kaynak türü | Supports tags | Tag in cost report |
> | ------------- | ----------- | ----------- |
> | accounts | Yes | Yes |

## <a name="microsoftcostmanagement"></a>Microsoft.CostManagement

> [!div class="mx-tableFixed"]
> | Kaynak türü | Supports tags | Tag in cost report |
> | ------------- | ----------- | ----------- |
> | Uyarılar | Hayır | Hayır |
> | BillingAccounts | Hayır | Hayır |
> | Bütçeler | Hayır | Hayır |
> | CloudConnectors | Hayır | Hayır |
> | Bağlayıcılar | Yes | Yes |
> | Departments | Hayır | Hayır |
> | Boyutlar | Hayır | Hayır |
> | EnrollmentAccounts | Hayır | Hayır |
> | Dışarı aktarmalar | Hayır | Hayır |
> | ExternalBillingAccounts | Hayır | Hayır |
> | ExternalBillingAccounts / Alerts | Hayır | Hayır |
> | ExternalBillingAccounts / Dimensions | Hayır | Hayır |
> | ExternalBillingAccounts / Forecast | Hayır | Hayır |
> | ExternalBillingAccounts / Query | Hayır | Hayır |
> | ExternalSubscriptions | Hayır | Hayır |
> | ExternalSubscriptions / Alerts | Hayır | Hayır |
> | ExternalSubscriptions / Dimensions | Hayır | Hayır |
> | ExternalSubscriptions / Forecast | Hayır | Hayır |
> | ExternalSubscriptions / Query | Hayır | Hayır |
> | Forecast | Hayır | Hayır |
> | Sorgu | Hayır | Hayır |
> | register | Hayır | Hayır |
> | Reportconfigs | Hayır | Hayır |
> | Raporlar | Hayır | Hayır |
> | Ayarlar | Hayır | Hayır |
> | showbackRules | Hayır | Hayır |
> | Görünümler | Hayır | Hayır |

## <a name="microsoftcustomerlockbox"></a>Microsoft.CustomerLockbox

> [!div class="mx-tableFixed"]
> | Kaynak türü | Supports tags | Tag in cost report |
> | ------------- | ----------- | ----------- |
> | requests | Hayır | Hayır |

## <a name="microsoftcustomproviders"></a>Microsoft.CustomProviders

> [!div class="mx-tableFixed"]
> | Kaynak türü | Supports tags | Tag in cost report |
> | ------------- | ----------- | ----------- |
> | associations | Hayır | Hayır |
> | resourceProviders | Yes | Yes |

## <a name="microsoftdatabox"></a>Microsoft.DataBox

> [!div class="mx-tableFixed"]
> | Kaynak türü | Supports tags | Tag in cost report |
> | ------------- | ----------- | ----------- |
> | jobs | Yes | Yes |

## <a name="microsoftdataboxedge"></a>Microsoft.DataBoxEdge

> [!div class="mx-tableFixed"]
> | Kaynak türü | Supports tags | Tag in cost report |
> | ------------- | ----------- | ----------- |
> | DataBoxEdgeDevices | Yes | Yes |

## <a name="microsoftdatabricks"></a>Microsoft.Databricks

> [!div class="mx-tableFixed"]
> | Kaynak türü | Supports tags | Tag in cost report |
> | ------------- | ----------- | ----------- |
> | workspaces | Yes | Hayır |
> | workspaces / virtualNetworkPeerings | Hayır | Hayır |

## <a name="microsoftdatacatalog"></a>Microsoft.DataCatalog

> [!div class="mx-tableFixed"]
> | Kaynak türü | Supports tags | Tag in cost report |
> | ------------- | ----------- | ----------- |
> | catalogs | Yes | Yes |
> | datacatalogs | Yes | Yes |
> | datacatalogs / datasources | Hayır | Hayır |
> | datacatalogs / datasources / scans | Hayır | Hayır |
> | datacatalogs / datasources / scans / datasets | Hayır | Hayır |
> | datacatalogs / datasources / scans / triggers | Hayır | Hayır |

## <a name="microsoftdatafactory"></a>Microsoft.DataFactory

> [!div class="mx-tableFixed"]
> | Kaynak türü | Supports tags | Tag in cost report |
> | ------------- | ----------- | ----------- |
> | dataFactories | Yes | Hayır |
> | dataFactories / diagnosticSettings | Hayır | Hayır |
> | dataFactories / metricDefinitions | Hayır | Hayır |
> | dataFactorySchema | Hayır | Hayır |
> | factories | Yes | Hayır |
> | factories / integrationRuntimes | Hayır | Hayır |

## <a name="microsoftdatalakeanalytics"></a>Microsoft.DataLakeAnalytics

> [!div class="mx-tableFixed"]
> | Kaynak türü | Supports tags | Tag in cost report |
> | ------------- | ----------- | ----------- |
> | accounts | Yes | Yes |
> | accounts / dataLakeStoreAccounts | Hayır | Hayır |
> | accounts / storageAccounts | Hayır | Hayır |
> | accounts / storageAccounts / containers | Hayır | Hayır |
> | accounts / transferAnalyticsUnits | Hayır | Hayır |

## <a name="microsoftdatalakestore"></a>Microsoft.DataLakeStore

> [!div class="mx-tableFixed"]
> | Kaynak türü | Supports tags | Tag in cost report |
> | ------------- | ----------- | ----------- |
> | accounts | Yes | Yes |
> | accounts / eventGridFilters | Hayır | Hayır |
> | accounts / firewallRules | Hayır | Hayır |

## <a name="microsoftdatamigration"></a>Microsoft.DataMigration

> [!div class="mx-tableFixed"]
> | Kaynak türü | Supports tags | Tag in cost report |
> | ------------- | ----------- | ----------- |
> | services | Hayır | Hayır |
> | services / projects | Hayır | Hayır |

## <a name="microsoftdatashare"></a>Microsoft.DataShare

> [!div class="mx-tableFixed"]
> | Kaynak türü | Supports tags | Tag in cost report |
> | ------------- | ----------- | ----------- |
> | accounts | Yes | Yes |
> | accounts / shares | Hayır | Hayır |
> | accounts / shares / datasets | Hayır | Hayır |
> | accounts / shares / invitations | Hayır | Hayır |
> | accounts / shares / providersharesubscriptions | Hayır | Hayır |
> | accounts / shares / synchronizationSettings | Hayır | Hayır |
> | accounts / sharesubscriptions | Hayır | Hayır |
> | accounts / sharesubscriptions / consumerSourceDataSets | Hayır | Hayır |
> | accounts / sharesubscriptions / datasetmappings | Hayır | Hayır |
> | accounts / sharesubscriptions / triggers | Hayır | Hayır |

## <a name="microsoftdbformariadb"></a>Microsoft.DBforMariaDB

> [!div class="mx-tableFixed"]
> | Kaynak türü | Supports tags | Tag in cost report |
> | ------------- | ----------- | ----------- |
> | sunucular | Yes | Yes |
> | servers / advisors | Hayır | Hayır |
> | servers / privateEndpointConnectionProxies | Hayır | Hayır |
> | servers / privateEndpointConnections | Hayır | Hayır |
> | servers / privateLinkResources | Hayır | Hayır |
> | servers / queryTexts | Hayır | Hayır |
> | servers / recoverableServers | Hayır | Hayır |
> | servers / topQueryStatistics | Hayır | Hayır |
> | servers / virtualNetworkRules | Hayır | Hayır |
> | servers / waitStatistics | Hayır | Hayır |

## <a name="microsoftdbformysql"></a>Microsoft.DBforMySQL

> [!div class="mx-tableFixed"]
> | Kaynak türü | Supports tags | Tag in cost report |
> | ------------- | ----------- | ----------- |
> | sunucular | Yes | Yes |
> | servers / advisors | Hayır | Hayır |
> | servers / privateEndpointConnectionProxies | Hayır | Hayır |
> | servers / privateEndpointConnections | Hayır | Hayır |
> | servers / privateLinkResources | Hayır | Hayır |
> | servers / queryTexts | Hayır | Hayır |
> | servers / recoverableServers | Hayır | Hayır |
> | servers / topQueryStatistics | Hayır | Hayır |
> | servers / virtualNetworkRules | Hayır | Hayır |
> | servers / waitStatistics | Hayır | Hayır |

## <a name="microsoftdbforpostgresql"></a>Microsoft.DBforPostgreSQL

> [!div class="mx-tableFixed"]
> | Kaynak türü | Supports tags | Tag in cost report |
> | ------------- | ----------- | ----------- |
> | serverGroups | Yes | Yes |
> | sunucular | Yes | Yes |
> | servers / advisors | Hayır | Hayır |
> | servers / keys | Hayır | Hayır |
> | servers / privateEndpointConnectionProxies | Hayır | Hayır |
> | servers / privateEndpointConnections | Hayır | Hayır |
> | servers / privateLinkResources | Hayır | Hayır |
> | servers / queryTexts | Hayır | Hayır |
> | servers / recoverableServers | Hayır | Hayır |
> | servers / topQueryStatistics | Hayır | Hayır |
> | servers / virtualNetworkRules | Hayır | Hayır |
> | servers / waitStatistics | Hayır | Hayır |
> | serversv2 | Yes | Yes |

## <a name="microsoftdeploymentmanager"></a>Microsoft.DeploymentManager

> [!div class="mx-tableFixed"]
> | Kaynak türü | Supports tags | Tag in cost report |
> | ------------- | ----------- | ----------- |
> | artifactSources | Yes | Yes |
> | rollouts | Yes | Yes |
> | serviceTopologies | Yes | Yes |
> | serviceTopologies / services | Yes | Yes |
> | serviceTopologies / services / serviceUnits | Yes | Yes |
> | adımlar | Yes | Yes |

## <a name="microsoftdesktopvirtualization"></a>Microsoft.DesktopVirtualization

> [!div class="mx-tableFixed"]
> | Kaynak türü | Supports tags | Tag in cost report |
> | ------------- | ----------- | ----------- |
> | applicationgroups | Yes | Yes |
> | applicationgroups / applications | Hayır | Hayır |
> | applicationgroups / desktops | Hayır | Hayır |
> | applicationgroups / startmenuitems | Hayır | Hayır |
> | hostpools | Yes | Yes |
> | hostpools / sessionhosts | Hayır | Hayır |
> | hostpools / sessionhosts / usersessions | Hayır | Hayır |
> | hostpools / usersessions | Hayır | Hayır |
> | workspaces | Yes | Yes |

## <a name="microsoftdevices"></a>Microsoft.Devices

> [!div class="mx-tableFixed"]
> | Kaynak türü | Supports tags | Tag in cost report |
> | ------------- | ----------- | ----------- |
> | ElasticPools | Yes | Yes |
> | ElasticPools / IotHubTenants | Yes | Yes |
> | IotHubs | Yes | Yes |
> | IotHubs / eventGridFilters | Hayır | Hayır |
> | ProvisioningServices | Yes | Yes |
> | usages | Hayır | Hayır |

## <a name="microsoftdevops"></a>Microsoft.DevOps

> [!div class="mx-tableFixed"]
> | Kaynak türü | Supports tags | Tag in cost report |
> | ------------- | ----------- | ----------- |
> | pipelines | Yes | Yes |

## <a name="microsoftdevspaces"></a>Microsoft.DevSpaces

> [!div class="mx-tableFixed"]
> | Kaynak türü | Supports tags | Tag in cost report |
> | ------------- | ----------- | ----------- |
> | controllers | Yes | Yes |

## <a name="microsoftdevtestlab"></a>Microsoft.DevTestLab

> [!div class="mx-tableFixed"]
> | Kaynak türü | Supports tags | Tag in cost report |
> | ------------- | ----------- | ----------- |
> | labcenters | Yes | Yes |
> | labs | Yes | Yes |
> | labs / environments | Yes | Yes |
> | labs / serviceRunners | Yes | Yes |
> | labs / virtualMachines | Yes | Yes |
> | schedules | Yes | Yes |

## <a name="microsoftdocumentdb"></a>Microsoft.DocumentDB

> [!div class="mx-tableFixed"]
> | Kaynak türü | Supports tags | Tag in cost report |
> | ------------- | ----------- | ----------- |
> | databaseAccountNames | Hayır | Hayır |
> | databaseAccounts | Yes | Yes |

## <a name="microsoftdomainregistration"></a>Microsoft.DomainRegistration

> [!div class="mx-tableFixed"]
> | Kaynak türü | Supports tags | Tag in cost report |
> | ------------- | ----------- | ----------- |
> | domains | Yes | Yes |
> | domains / domainOwnershipIdentifiers | Hayır | Hayır |
> | generateSsoRequest | Hayır | Hayır |
> | topLevelDomains | Hayır | Hayır |
> | validateDomainRegistrationInformation | Hayır | Hayır |

## <a name="microsoftdynamicslcs"></a>Microsoft.DynamicsLcs

> [!div class="mx-tableFixed"]
> | Kaynak türü | Supports tags | Tag in cost report |
> | ------------- | ----------- | ----------- |
> | lcsprojects | Hayır | Hayır |
> | lcsprojects / clouddeployments | Hayır | Hayır |
> | lcsprojects / connectors | Hayır | Hayır |

## <a name="microsoftenterpriseknowledgegraph"></a>Microsoft.EnterpriseKnowledgeGraph

> [!div class="mx-tableFixed"]
> | Kaynak türü | Supports tags | Tag in cost report |
> | ------------- | ----------- | ----------- |
> | services | Yes | Yes |

## <a name="microsofteventgrid"></a>Microsoft.EventGrid

> [!div class="mx-tableFixed"]
> | Kaynak türü | Supports tags | Tag in cost report |
> | ------------- | ----------- | ----------- |
> | domains | Yes | Yes |
> | domains / topics | Hayır | Hayır |
> | eventSubscriptions | Hayır | Hayır |
> | extensionTopics | Hayır | Hayır |
> | topics | Yes | Yes |
> | topicTypes | Hayır | Hayır |

## <a name="microsofteventhub"></a>Microsoft.EventHub

> [!div class="mx-tableFixed"]
> | Kaynak türü | Supports tags | Tag in cost report |
> | ------------- | ----------- | ----------- |
> | clusters | Yes | Yes |
> | namespaces | Yes | Yes |
> | namespaces / authorizationrules | Hayır | Hayır |
> | namespaces / disasterrecoveryconfigs | Hayır | Hayır |
> | namespaces / eventhubs | Hayır | Hayır |
> | namespaces / eventhubs / authorizationrules | Hayır | Hayır |
> | namespaces / eventhubs / consumergroups | Hayır | Hayır |
> | namespaces / networkrulesets | Hayır | Hayır |

## <a name="microsoftfeatures"></a>Microsoft.Features

> [!div class="mx-tableFixed"]
> | Kaynak türü | Supports tags | Tag in cost report |
> | ------------- | ----------- | ----------- |
> | SaaS Uygulamaları Geliştirme | Hayır | Hayır |
> | providers | Hayır | Hayır |

## <a name="microsoftgallery"></a>Microsoft.Gallery

> [!div class="mx-tableFixed"]
> | Kaynak türü | Supports tags | Tag in cost report |
> | ------------- | ----------- | ----------- |
> | enroll | Hayır | Hayır |
> | galleryitems | Hayır | Hayır |
> | generateartifactaccessuri | Hayır | Hayır |
> | myareas | Hayır | Hayır |
> | myareas / areas | Hayır | Hayır |
> | myareas / areas / areas | Hayır | Hayır |
> | myareas / areas / areas / galleryitems | Hayır | Hayır |
> | myareas / areas / galleryitems | Hayır | Hayır |
> | myareas / galleryitems | Hayır | Hayır |
> | register | Hayır | Hayır |
> | resources | Hayır | Hayır |
> | retrieveresourcesbyid | Hayır | Hayır |

## <a name="microsoftgenomics"></a>Microsoft.Genomics

> [!div class="mx-tableFixed"]
> | Kaynak türü | Supports tags | Tag in cost report |
> | ------------- | ----------- | ----------- |
> | accounts | Yes | Yes |

## <a name="microsoftguestconfiguration"></a>Microsoft.GuestConfiguration

> [!div class="mx-tableFixed"]
> | Kaynak türü | Supports tags | Tag in cost report |
> | ------------- | ----------- | ----------- |
> | configurationProfileAssignments | Hayır | Hayır |
> | guestConfigurationAssignments | Hayır | Hayır |
> | software | Hayır | Hayır |
> | softwareUpdateProfile | Hayır | Hayır |
> | softwareUpdates | Hayır | Hayır |

## <a name="microsofthanaonazure"></a>Microsoft.HanaOnAzure

> [!div class="mx-tableFixed"]
> | Kaynak türü | Supports tags | Tag in cost report |
> | ------------- | ----------- | ----------- |
> | hanaInstances | Yes | Yes |
> | sapMonitors | Yes | Yes |

## <a name="microsofthardwaresecuritymodules"></a>Microsoft.HardwareSecurityModules

> [!div class="mx-tableFixed"]
> | Kaynak türü | Supports tags | Tag in cost report |
> | ------------- | ----------- | ----------- |
> | dedicatedHSMs | Yes | Yes |

## <a name="microsofthdinsight"></a>Microsoft.HDInsight

> [!div class="mx-tableFixed"]
> | Kaynak türü | Supports tags | Tag in cost report |
> | ------------- | ----------- | ----------- |
> | clusters | Yes | Yes |
> | clusters / applications | Hayır | Hayır |

## <a name="microsofthealthcareapis"></a>Microsoft.HealthcareApis

> [!div class="mx-tableFixed"]
> | Kaynak türü | Supports tags | Tag in cost report |
> | ------------- | ----------- | ----------- |
> | services | Yes | Yes |

## <a name="microsofthybridcompute"></a>Microsoft.HybridCompute

> [!div class="mx-tableFixed"]
> | Kaynak türü | Supports tags | Tag in cost report |
> | ------------- | ----------- | ----------- |
> | machines | Yes | Yes |
> | machines / extensions | Yes | Yes |

## <a name="microsofthybriddata"></a>Microsoft.HybridData

> [!div class="mx-tableFixed"]
> | Kaynak türü | Supports tags | Tag in cost report |
> | ------------- | ----------- | ----------- |
> | dataManagers | Yes | Yes |

## <a name="microsofthydra"></a>Microsoft.Hydra

> [!div class="mx-tableFixed"]
> | Kaynak türü | Supports tags | Tag in cost report |
> | ------------- | ----------- | ----------- |
> | components | Yes | Yes |
> | networkScopes | Yes | Yes |

## <a name="microsoftimportexport"></a>Microsoft.ImportExport

> [!div class="mx-tableFixed"]
> | Kaynak türü | Supports tags | Tag in cost report |
> | ------------- | ----------- | ----------- |
> | jobs | Yes | Yes |

## <a name="microsoftintune"></a>Microsoft.Intune

> [!div class="mx-tableFixed"]
> | Kaynak türü | Supports tags | Tag in cost report |
> | ------------- | ----------- | ----------- |
> | diagnosticSettings | Hayır | Hayır |
> | diagnosticSettingsCategories | Hayır | Hayır |

## <a name="microsoftiotcentral"></a>Microsoft.IoTCentral

> [!div class="mx-tableFixed"]
> | Kaynak türü | Supports tags | Tag in cost report |
> | ------------- | ----------- | ----------- |
> | appTemplates | Hayır | Hayır |
> | IoTApps | Yes | Yes |

## <a name="microsoftiotspaces"></a>Microsoft.IoTSpaces

> [!div class="mx-tableFixed"]
> | Kaynak türü | Supports tags | Tag in cost report |
> | ------------- | ----------- | ----------- |
> | Graph | Yes | Yes |

## <a name="microsoftkeyvault"></a>Microsoft.KeyVault

> [!div class="mx-tableFixed"]
> | Kaynak türü | Supports tags | Tag in cost report |
> | ------------- | ----------- | ----------- |
> | deletedVaults | Hayır | Hayır |
> | hsmPools | Yes | Yes |
> | vaults | Yes | Yes |
> | vaults / accessPolicies | Hayır | Hayır |
> | vaults / eventGridFilters | Hayır | Hayır |
> | vaults / secrets | Hayır | Hayır |

## <a name="microsoftkusto"></a>Microsoft.Kusto

> [!div class="mx-tableFixed"]
> | Kaynak türü | Supports tags | Tag in cost report |
> | ------------- | ----------- | ----------- |
> | clusters | Yes | Yes |
> | clusters / attacheddatabaseconfigurations | Hayır | Hayır |
> | clusters / databases | Hayır | Hayır |
> | clusters / databases / dataconnections | Hayır | Hayır |
> | clusters / databases / eventhubconnections | Hayır | Hayır |
> | clusters / sharedidentities | Hayır | Hayır |

## <a name="microsoftlabservices"></a>Microsoft.LabServices

> [!div class="mx-tableFixed"]
> | Kaynak türü | Supports tags | Tag in cost report |
> | ------------- | ----------- | ----------- |
> | labaccounts | Yes | Yes |
> | kullanıcı | Hayır | Hayır |

## <a name="microsoftlogic"></a>Microsoft.Logic

> [!div class="mx-tableFixed"]
> | Kaynak türü | Supports tags | Tag in cost report |
> | ------------- | ----------- | ----------- |
> | hostingEnvironments | Yes | Yes |
> | integrationAccounts | Yes | Yes |
> | integrationServiceEnvironments | Yes | Yes |
> | integrationServiceEnvironments / managedApis | Yes | Yes |
> | isolatedEnvironments | Yes | Yes |
> | workflows | Yes | Yes |

## <a name="microsoftmachinelearning"></a>Microsoft.MachineLearning

> [!div class="mx-tableFixed"]
> | Kaynak türü | Supports tags | Tag in cost report |
> | ------------- | ----------- | ----------- |
> | commitmentPlans | Yes | Yes |
> | webServices | Yes | Yes |
> | Çalışma alanları | Yes | Yes |

## <a name="microsoftmachinelearningservices"></a>Microsoft.MachineLearningServices

> [!div class="mx-tableFixed"]
> | Kaynak türü | Supports tags | Tag in cost report |
> | ------------- | ----------- | ----------- |
> | workspaces | Yes | Yes |
> | workspaces / computes | Hayır | Hayır |
> | workspaces / eventGridFilters | Hayır | Hayır |

## <a name="microsoftmanagedidentity"></a>Microsoft.ManagedIdentity

> [!div class="mx-tableFixed"]
> | Kaynak türü | Supports tags | Tag in cost report |
> | ------------- | ----------- | ----------- |
> | Identities | Hayır | Hayır |
> | userAssignedIdentities | Yes | Yes |

## <a name="microsoftmanagedservices"></a>Microsoft.ManagedServices

> [!div class="mx-tableFixed"]
> | Kaynak türü | Supports tags | Tag in cost report |
> | ------------- | ----------- | ----------- |
> | marketplaceRegistrationDefinitions | Hayır | Hayır |
> | registrationAssignments | Hayır | Hayır |
> | registrationDefinitions | Hayır | Hayır |

## <a name="microsoftmanagement"></a>Microsoft.Management

> [!div class="mx-tableFixed"]
> | Kaynak türü | Supports tags | Tag in cost report |
> | ------------- | ----------- | ----------- |
> | getEntities | Hayır | Hayır |
> | managementGroups | Hayır | Hayır |
> | resources | Hayır | Hayır |
> | startTenantBackfill | Hayır | Hayır |
> | tenantBackfillStatus | Hayır | Hayır |

## <a name="microsoftmaps"></a>Microsoft.Maps

> [!div class="mx-tableFixed"]
> | Kaynak türü | Supports tags | Tag in cost report |
> | ------------- | ----------- | ----------- |
> | accounts | Yes | Yes |
> | accounts / eventGridFilters | Hayır | Hayır |

## <a name="microsoftmarketplace"></a>Microsoft.Marketplace

> [!div class="mx-tableFixed"]
> | Kaynak türü | Supports tags | Tag in cost report |
> | ------------- | ----------- | ----------- |
> | offers | Hayır | Hayır |
> | offerTypes | Hayır | Hayır |
> | offerTypes / publishers | Hayır | Hayır |
> | offerTypes / publishers / offers | Hayır | Hayır |
> | offerTypes / publishers / offers / plans | Hayır | Hayır |
> | offerTypes / publishers / offers / plans / agreements | Hayır | Hayır |
> | offerTypes / publishers / offers / plans / configs | Hayır | Hayır |
> | offerTypes / publishers / offers / plans / configs / importImage | Hayır | Hayır |
> | privategalleryitems | Hayır | Hayır |
> | products | Hayır | Hayır |
> | publishers | Hayır | Hayır |
> | publishers / offers | Hayır | Hayır |
> | publishers / offers / amendments | Hayır | Hayır |

## <a name="microsoftmarketplaceapps"></a>Microsoft.MarketplaceApps

> [!div class="mx-tableFixed"]
> | Kaynak türü | Supports tags | Tag in cost report |
> | ------------- | ----------- | ----------- |
> | classicDevServices | Yes | Yes |
> | updateCommunicationPreference | Hayır | Hayır |

## <a name="microsoftmarketplaceordering"></a>Microsoft.MarketplaceOrdering

> [!div class="mx-tableFixed"]
> | Kaynak türü | Supports tags | Tag in cost report |
> | ------------- | ----------- | ----------- |
> | agreements | Hayır | Hayır |
> | offertypes | Hayır | Hayır |

## <a name="microsoftmedia"></a>Microsoft.Media

> [!div class="mx-tableFixed"]
> | Kaynak türü | Supports tags | Tag in cost report |
> | ------------- | ----------- | ----------- |
> | mediaservices | Yes | Yes |
> | mediaservices / accountFilters | Hayır | Hayır |
> | mediaservices / assets | Hayır | Hayır |
> | mediaservices / assets / assetFilters | Hayır | Hayır |
> | mediaservices / contentKeyPolicies | Hayır | Hayır |
> | mediaservices / eventGridFilters | Hayır | Hayır |
> | mediaservices / liveEventOperations | Hayır | Hayır |
> | mediaservices / liveEvents | Yes | Yes |
> | mediaservices / liveEvents / liveOutputs | Hayır | Hayır |
> | mediaservices / liveOutputOperations | Hayır | Hayır |
> | mediaservices / mediaGraphs | Hayır | Hayır |
> | mediaservices / streamingEndpointOperations | Hayır | Hayır |
> | mediaservices / streamingEndpoints | Yes | Yes |
> | mediaservices / streamingLocators | Hayır | Hayır |
> | mediaservices / streamingPolicies | Hayır | Hayır |
> | mediaservices / transforms | Hayır | Hayır |
> | mediaservices / transforms / jobs | Hayır | Hayır |

## <a name="microsoftmicroservices4spring"></a>Microsoft.Microservices4Spring

> [!div class="mx-tableFixed"]
> | Kaynak türü | Supports tags | Tag in cost report |
> | ------------- | ----------- | ----------- |
> | appClusters | Yes | Yes |

## <a name="microsoftmigrate"></a>Microsoft.Migrate

> [!div class="mx-tableFixed"]
> | Kaynak türü | Supports tags | Tag in cost report |
> | ------------- | ----------- | ----------- |
> | assessmentProjects | Yes | Yes |
> | migrateprojects | Yes | Yes |
> | projects | Yes | Yes |

## <a name="microsoftmixedreality"></a>Microsoft.MixedReality

> [!div class="mx-tableFixed"]
> | Kaynak türü | Supports tags | Tag in cost report |
> | ------------- | ----------- | ----------- |
> | holographicsBroadcastAccounts | Yes | Yes |
> | objectUnderstandingAccounts | Yes | Yes |
> | remoteRenderingAccounts | Yes | Yes |
> | spatialAnchorsAccounts | Yes | Yes |
> | surfaceReconstructionAccounts | Yes | Yes |

## <a name="microsoftnetapp"></a>Microsoft.NetApp

> [!div class="mx-tableFixed"]
> | Kaynak türü | Supports tags | Tag in cost report |
> | ------------- | ----------- | ----------- |
> | netAppAccounts | Yes | Hayır |
> | netAppAccounts / capacityPools | Yes | Hayır |
> | netAppAccounts / capacityPools / volumes | Yes | Hayır |
> | netAppAccounts / capacityPools / volumes / mountTargets | Yes | Hayır |
> | netAppAccounts / capacityPools / volumes / snapshots | Yes | Hayır |

## <a name="microsoftnetwork"></a>Microsoft.Network

> [!div class="mx-tableFixed"]
> | Kaynak türü | Supports tags | Tag in cost report |
> | ------------- | ----------- | ----------- |
> | applicationGateways | Yes | Yes |
> | applicationGatewayWebApplicationFirewallPolicies | Yes | Yes |
> | applicationSecurityGroups | Yes | Yes |
> | azureFirewallFqdnTags | Hayır | Hayır |
> | azureFirewalls | Yes | Hayır |
> | bastionHosts | Yes | Yes |
> | bgpServiceCommunities | Hayır | Hayır |
> | connections | Yes | Yes |
> | ddosCustomPolicies | Yes | Yes |
> | ddosProtectionPlans | Yes | Yes |
> | dnsOperationStatuses | Hayır | Hayır |
> | dnszones | Yes | Yes |
> | dnszones / A | Hayır | Hayır |
> | dnszones / AAAA | Hayır | Hayır |
> | dnszones / all | Hayır | Hayır |
> | dnszones / CAA | Hayır | Hayır |
> | dnszones / CNAME | Hayır | Hayır |
> | dnszones / MX | Hayır | Hayır |
> | dnszones / NS | Hayır | Hayır |
> | dnszones / PTR | Hayır | Hayır |
> | dnszones / recordsets | Hayır | Hayır |
> | dnszones / SOA | Hayır | Hayır |
> | dnszones / SRV | Hayır | Hayır |
> | dnszones / TXT | Hayır | Hayır |
> | expressRouteCircuits | Yes | Yes |
> | expressRouteCrossConnections | Yes | Yes |
> | expressRouteGateways | Yes | Yes |
> | expressRoutePorts | Yes | Yes |
> | expressRouteServiceProviders | Hayır | Hayır |
> | firewallPolicies | Yes | Yes |
> | frontdoors | Yes, but limited (see [note below](#frontdoor)) | Yes |
> | frontdoorWebApplicationFirewallManagedRuleSets | Yes, but limited (see [note below](#frontdoor)) | Hayır |
> | frontdoorWebApplicationFirewallPolicies | Yes, but limited (see [note below](#frontdoor)) | Yes |
> | getDnsResourceReference | Hayır | Hayır |
> | internalNotify | Hayır | Hayır |
> | loadBalancers | Yes | Hayır |
> | localNetworkGateways | Yes | Yes |
> | natGateways | Yes | Yes |
> | networkIntentPolicies | Yes | Yes |
> | networkInterfaces | Yes | Yes |
> | networkProfiles | Yes | Yes |
> | networkSecurityGroups | Yes | Yes |
> | networkWatchers | Yes | Hayır |
> | networkWatchers / connectionMonitors | Yes | Hayır |
> | networkWatchers / lenses | Yes | Hayır |
> | networkWatchers / pingMeshes | Yes | Hayır |
> | p2sVpnGateways | Yes | Yes |
> | privateDnsOperationStatuses | Hayır | Hayır |
> | privateDnsZones | Yes | Yes |
> | privateDnsZones / A | Hayır | Hayır |
> | privateDnsZones / AAAA | Hayır | Hayır |
> | privateDnsZones / all | Hayır | Hayır |
> | privateDnsZones / CNAME | Hayır | Hayır |
> | privateDnsZones / MX | Hayır | Hayır |
> | privateDnsZones / PTR | Hayır | Hayır |
> | privateDnsZones / SOA | Hayır | Hayır |
> | privateDnsZones / SRV | Hayır | Hayır |
> | privateDnsZones / TXT | Hayır | Hayır |
> | privateDnsZones / virtualNetworkLinks | Yes | Yes |
> | privateEndpoints | Yes | Yes |
> | privateLinkServices | Yes | Yes |
> | publicIPAddresses | Yes | Yes |
> | publicIPPrefixes | Yes | Yes |
> | routeFilters | Yes | Yes |
> | routeTables | Yes | Yes |
> | serviceEndpointPolicies | Yes | Yes |
> | trafficManagerGeographicHierarchies | Hayır | Hayır |
> | trafficmanagerprofiles | Yes | Yes |
> | trafficmanagerprofiles/heatMaps | Hayır | Hayır |
> | trafficManagerUserMetricsKeys | Hayır | Hayır |
> | virtualHubs | Yes | Yes |
> | virtualNetworkGateways | Yes | Yes |
> | virtualNetworks | Yes | Yes |
> | virtualNetworkTaps | Yes | Yes |
> | virtualWans | Yes | Yes |
> | vpnGateways | Yes | Hayır |
> | vpnSites | Yes | Yes |
> | webApplicationFirewallPolicies | Yes | Yes |

<a id="frontdoor" />

> [!NOTE]
> For Azure Front Door Service, you can apply tags when creating the resource, but updating or adding tags is not currently supported.


## <a name="microsoftnotificationhubs"></a>Microsoft.NotificationHubs

> [!div class="mx-tableFixed"]
> | Kaynak türü | Supports tags | Tag in cost report |
> | ------------- | ----------- | ----------- |
> | namespaces | Yes | Hayır |
> | namespaces / notificationHubs | Yes | Hayır |

## <a name="microsoftobjectstore"></a>Microsoft.ObjectStore

> [!div class="mx-tableFixed"]
> | Kaynak türü | Supports tags | Tag in cost report |
> | ------------- | ----------- | ----------- |
> | osNamespaces | Yes | Yes |

## <a name="microsoftoffazure"></a>Microsoft.OffAzure

> [!div class="mx-tableFixed"]
> | Kaynak türü | Supports tags | Tag in cost report |
> | ------------- | ----------- | ----------- |
> | HyperVSites | Yes | Yes |
> | ImportSites | Yes | Yes |
> | ServerSites | Yes | Yes |
> | VMwareSites | Yes | Yes |

## <a name="microsoftoperationalinsights"></a>Microsoft.OperationalInsights

> [!div class="mx-tableFixed"]
> | Kaynak türü | Supports tags | Tag in cost report |
> | ------------- | ----------- | ----------- |
> | clusters | Yes | Yes |
> | cihazlar | Hayır | Hayır |
> | linkTargets | Hayır | Hayır |
> | storageInsightConfigs | Hayır | Hayır |
> | workspaces | Yes | Yes |
> | workspaces / dataSources | Hayır | Hayır |
> | workspaces / linkedServices | Hayır | Hayır |
> | workspaces / query | Hayır | Hayır |

## <a name="microsoftoperationsmanagement"></a>Microsoft.OperationsManagement

> [!div class="mx-tableFixed"]
> | Kaynak türü | Supports tags | Tag in cost report |
> | ------------- | ----------- | ----------- |
> | managementassociations | Hayır | Hayır |
> | managementconfigurations | Yes | Yes |
> | çözümler | Yes | Yes |
> | views | Yes | Yes |

## <a name="microsoftpeering"></a>Microsoft.Peering

> [!div class="mx-tableFixed"]
> | Kaynak türü | Supports tags | Tag in cost report |
> | ------------- | ----------- | ----------- |
> | legacyPeerings | Hayır | Hayır |
> | peerAsns | Hayır | Hayır |
> | peerings | Yes | Yes |
> | peeringServiceProviders | Hayır | Hayır |
> | peeringServices | Yes | Yes |

## <a name="microsoftpolicyinsights"></a>Microsoft.PolicyInsights

> [!div class="mx-tableFixed"]
> | Kaynak türü | Supports tags | Tag in cost report |
> | ------------- | ----------- | ----------- |
> | policyEvents | Hayır | Hayır |
> | policyMetadata | Hayır | Hayır |
> | policyStates | Hayır | Hayır |
> | policyTrackedResources | Hayır | Hayır |
> | remediations | Hayır | Hayır |

## <a name="microsoftportal"></a>Microsoft.Portal

> [!div class="mx-tableFixed"]
> | Kaynak türü | Supports tags | Tag in cost report |
> | ------------- | ----------- | ----------- |
> | consoles | Hayır | Hayır |
> | dashboards | Yes | Yes |
> | userSettings | Hayır | Hayır |

## <a name="microsoftpowerbi"></a>Microsoft.PowerBI

> [!div class="mx-tableFixed"]
> | Kaynak türü | Supports tags | Tag in cost report |
> | ------------- | ----------- | ----------- |
> | workspaceCollections | Yes | Yes |

## <a name="microsoftpowerbidedicated"></a>Microsoft.PowerBIDedicated

> [!div class="mx-tableFixed"]
> | Kaynak türü | Supports tags | Tag in cost report |
> | ------------- | ----------- | ----------- |
> | capacities | Yes | Yes |

## <a name="microsoftrecoveryservices"></a>Microsoft.RecoveryServices

> [!div class="mx-tableFixed"]
> | Kaynak türü | Supports tags | Tag in cost report |
> | ------------- | ----------- | ----------- |
> | backupProtectedItems | Hayır | Hayır |
> | vaults | Yes | Yes |

## <a name="microsoftrelay"></a>Microsoft.Relay

> [!div class="mx-tableFixed"]
> | Kaynak türü | Supports tags | Tag in cost report |
> | ------------- | ----------- | ----------- |
> | namespaces | Yes | Yes |
> | namespaces / authorizationrules | Hayır | Hayır |
> | namespaces / hybridconnections | Hayır | Hayır |
> | namespaces / hybridconnections / authorizationrules | Hayır | Hayır |
> | namespaces / wcfrelays | Hayır | Hayır |
> | namespaces / wcfrelays / authorizationrules | Hayır | Hayır |

## <a name="microsoftremoteapp"></a>Microsoft.RemoteApp

> [!div class="mx-tableFixed"]
> | Kaynak türü | Supports tags | Tag in cost report |
> | ------------- | ----------- | ----------- |
> | accounts | Hayır | Hayır |
> | collections | Yes | Yes |
> | collections / applications | Hayır | Hayır |
> | collections / securityprincipals | Hayır | Hayır |
> | templateImages | Hayır | Hayır |

## <a name="microsoftresourcegraph"></a>Microsoft.ResourceGraph

> [!div class="mx-tableFixed"]
> | Kaynak türü | Supports tags | Tag in cost report |
> | ------------- | ----------- | ----------- |
> | sorgu | Yes | Yes |
> | resourceChangeDetails | Hayır | Hayır |
> | resourceChanges | Hayır | Hayır |
> | resources | Hayır | Hayır |
> | resourcesHistory | Hayır | Hayır |
> | subscriptionsStatus | Hayır | Hayır |

## <a name="microsoftresourcehealth"></a>Microsoft.ResourceHealth

> [!div class="mx-tableFixed"]
> | Kaynak türü | Supports tags | Tag in cost report |
> | ------------- | ----------- | ----------- |
> | availabilityStatuses | Hayır | Hayır |
> | childAvailabilityStatuses | Hayır | Hayır |
> | childResources | Hayır | Hayır |
> | etkinlikler | Hayır | Hayır |
> | impactedResources | Hayır | Hayır |
> | meta veriler | Hayır | Hayır |
> | notifications | Hayır | Hayır |

## <a name="microsoftresources"></a>Microsoft.Resources

> [!div class="mx-tableFixed"]
> | Kaynak türü | Supports tags | Tag in cost report |
> | ------------- | ----------- | ----------- |
> | deployments | Yes | Hayır |
> | deployments / operations | Hayır | Hayır |
> | deploymentScripts | Yes | Yes |
> | deploymentScripts / logs | Hayır | Hayır |
> | links | Hayır | Hayır |
> | notifyResourceJobs | Hayır | Hayır |
> | providers | Hayır | Hayır |
> | resourceGroups | Yes | Hayır |
> | subscriptions | Hayır | Hayır |
> | tenants | Hayır | Hayır |

## <a name="microsoftsaas"></a>Microsoft.SaaS

> [!div class="mx-tableFixed"]
> | Kaynak türü | Supports tags | Tag in cost report |
> | ------------- | ----------- | ----------- |
> | uygulama | Yes | Yes |
> | saasresources | Hayır | Hayır |

## <a name="microsoftscheduler"></a>Microsoft.Scheduler

> [!div class="mx-tableFixed"]
> | Kaynak türü | Supports tags | Tag in cost report |
> | ------------- | ----------- | ----------- |
> | jobcollections | Yes | Yes |

## <a name="microsoftsearch"></a>Microsoft.Search

> [!div class="mx-tableFixed"]
> | Kaynak türü | Supports tags | Tag in cost report |
> | ------------- | ----------- | ----------- |
> | resourceHealthMetadata | Hayır | Hayır |
> | searchServices | Yes | Yes |

## <a name="microsoftsecurity"></a>Microsoft.Security

> [!div class="mx-tableFixed"]
> | Kaynak türü | Supports tags | Tag in cost report |
> | ------------- | ----------- | ----------- |
> | adaptiveNetworkHardenings | Hayır | Hayır |
> | advancedThreatProtectionSettings | Hayır | Hayır |
> | alerts | Hayır | Hayır |
> | allowedConnections | Hayır | Hayır |
> | applicationWhitelistings | Hayır | Hayır |
> | assessmentMetadata | Hayır | Hayır |
> | assessments | Hayır | Hayır |
> | autoDismissAlertsRules | Hayır | Hayır |
> | automations | Yes | Yes |
> | AutoProvisioningSettings | Hayır | Hayır |
> | Compliances | Hayır | Hayır |
> | dataCollectionAgents | Hayır | Hayır |
> | deviceSecurityGroups | Hayır | Hayır |
> | discoveredSecuritySolutions | Hayır | Hayır |
> | externalSecuritySolutions | Hayır | Hayır |
> | InformationProtectionPolicies | Hayır | Hayır |
> | iotSecuritySolutions | Yes | Yes |
> | iotSecuritySolutions / analyticsModels | Hayır | Hayır |
> | iotSecuritySolutions / analyticsModels / aggregatedAlerts | Hayır | Hayır |
> | iotSecuritySolutions / analyticsModels / aggregatedRecommendations | Hayır | Hayır |
> | jitNetworkAccessPolicies | Hayır | Hayır |
> | networkData | Hayır | Hayır |
> | policies | Hayır | Hayır |
> | pricings | Hayır | Hayır |
> | regulatoryComplianceStandards | Hayır | Hayır |
> | regulatoryComplianceStandards / regulatoryComplianceControls | Hayır | Hayır |
> | regulatoryComplianceStandards / regulatoryComplianceControls / regulatoryComplianceAssessments | Hayır | Hayır |
> | securityContacts | Hayır | Hayır |
> | securitySolutions | Hayır | Hayır |
> | securitySolutionsReferenceData | Hayır | Hayır |
> | securityStatuses | Hayır | Hayır |
> | securityStatusesSummaries | Hayır | Hayır |
> | serverVulnerabilityAssessments | Hayır | Hayır |
> | ayarlar | Hayır | Hayır |
> | subAssessments | Hayır | Hayır |
> | Görevler | Hayır | Hayır |
> | topologies | Hayır | Hayır |
> | workspaceSettings | Hayır | Hayır |

## <a name="microsoftsecuritygraph"></a>Microsoft.SecurityGraph

> [!div class="mx-tableFixed"]
> | Kaynak türü | Supports tags | Tag in cost report |
> | ------------- | ----------- | ----------- |
> | diagnosticSettings | Hayır | Hayır |
> | diagnosticSettingsCategories | Hayır | Hayır |

## <a name="microsoftsecurityinsights"></a>Microsoft.SecurityInsights

> [!div class="mx-tableFixed"]
> | Kaynak türü | Supports tags | Tag in cost report |
> | ------------- | ----------- | ----------- |
> | aggregations | Hayır | Hayır |
> | alertRules | Hayır | Hayır |
> | alertRuleTemplates | Hayır | Hayır |
> | bookmarks | Hayır | Hayır |
> | cases | Hayır | Hayır |
> | dataConnectors | Hayır | Hayır |
> | entities | Hayır | Hayır |
> | entityQueries | Hayır | Hayır |
> | officeConsents | Hayır | Hayır |
> | ayarlar | Hayır | Hayır |

## <a name="microsoftservicebus"></a>Microsoft.ServiceBus

> [!div class="mx-tableFixed"]
> | Kaynak türü | Supports tags | Tag in cost report |
> | ------------- | ----------- | ----------- |
> | namespaces | Yes | Hayır |
> | namespaces / authorizationrules | Hayır | Hayır |
> | namespaces / disasterrecoveryconfigs | Hayır | Hayır |
> | namespaces / eventgridfilters | Hayır | Hayır |
> | namespaces / networkrulesets | Hayır | Hayır |
> | namespaces / queues | Hayır | Hayır |
> | namespaces / queues / authorizationrules | Hayır | Hayır |
> | namespaces / topics | Hayır | Hayır |
> | namespaces / topics / authorizationrules | Hayır | Hayır |
> | namespaces / topics / subscriptions | Hayır | Hayır |
> | namespaces / topics / subscriptions / rules | Hayır | Hayır |
> | premiumMessagingRegions | Hayır | Hayır |

## <a name="microsoftservicefabric"></a>Microsoft.ServiceFabric

> [!div class="mx-tableFixed"]
> | Kaynak türü | Supports tags | Tag in cost report |
> | ------------- | ----------- | ----------- |
> | uygulama | Yes | Yes |
> | clusters | Yes | Yes |
> | clusters / applications | Hayır | Hayır |
> | containerGroups | Yes | Yes |
> | containerGroupSets | Yes | Yes |
> | edgeclusters | Yes | Yes |
> | edgeclusters / applications | Hayır | Hayır |
> | networks | Yes | Yes |
> | secretstores | Yes | Yes |
> | secretstores / certificates | Hayır | Hayır |
> | secretstores / secrets | Hayır | Hayır |
> | volumes | Yes | Yes |

## <a name="microsoftservicefabricmesh"></a>Microsoft.ServiceFabricMesh

> [!div class="mx-tableFixed"]
> | Kaynak türü | Supports tags | Tag in cost report |
> | ------------- | ----------- | ----------- |
> | uygulama | Yes | Yes |
> | containerGroups | Yes | Yes |
> | gateways | Yes | Yes |
> | networks | Yes | Yes |
> | secrets | Yes | Yes |
> | volumes | Yes | Yes |

## <a name="microsoftservices"></a>Microsoft.Services

> [!div class="mx-tableFixed"]
> | Kaynak türü | Supports tags | Tag in cost report |
> | ------------- | ----------- | ----------- |
> | providerRegistrations | Hayır | Hayır |
> | providerRegistrations / resourceTypeRegistrations | Hayır | Hayır |
> | rollouts | Yes | Yes |

## <a name="microsoftsignalrservice"></a>Microsoft.SignalRService

> [!div class="mx-tableFixed"]
> | Kaynak türü | Supports tags | Tag in cost report |
> | ------------- | ----------- | ----------- |
> | SignalR | Yes | Yes |
> | SignalR / eventGridFilters | Hayır | Hayır |

## <a name="microsoftsiterecovery"></a>Microsoft.SiteRecovery

> [!div class="mx-tableFixed"]
> | Kaynak türü | Supports tags | Tag in cost report |
> | ------------- | ----------- | ----------- |
> | SiteRecoveryVault | Yes | Yes |

## <a name="microsoftsoftwareplan"></a>Microsoft.SoftwarePlan

> [!div class="mx-tableFixed"]
> | Kaynak türü | Supports tags | Tag in cost report |
> | ------------- | ----------- | ----------- |
> | hybridUseBenefits | Hayır | Hayır |

## <a name="microsoftsolutions"></a>Microsoft.Solutions

> [!div class="mx-tableFixed"]
> | Kaynak türü | Supports tags | Tag in cost report |
> | ------------- | ----------- | ----------- |
> | applicationDefinitions | Yes | Yes |
> | uygulama | Yes | Yes |
> | jitRequests | Yes | Yes |


## <a name="microsoftsql"></a>Microsoft.SQL

> [!div class="mx-tableFixed"]
> | Kaynak türü | Supports tags | Tag in cost report |
> | ------------- | ----------- | ----------- |
> | managedInstances | Yes | Yes |
> | managedInstances / databases | Hayır | Hayır |
> | managedInstances / databases / backupShortTermRetentionPolicies | Hayır | Hayır |
> | managedInstances / databases / schemas / tables / columns / sensitivityLabels | Hayır | Hayır |
> | managedInstances / databases / vulnerabilityAssessments | Hayır | Hayır |
> | managedInstances / databases / vulnerabilityAssessments / rules / baselines | Hayır | Hayır |
> | managedInstances / encryptionProtector | Hayır | Hayır |
> | managedInstances / keys | Hayır | Hayır |
> | managedInstances / restorableDroppedDatabases / backupShortTermRetentionPolicies | Hayır | Hayır |
> | managedInstances / vulnerabilityAssessments | Hayır | Hayır |
> | sunucular | Yes | Yes |
> | servers / administrators | Hayır | Hayır |
> | servers / communicationLinks | Hayır | Hayır |
> | servers / databases | Yes (see [note below](#sqlnote)) | Yes |
> | servers / encryptionProtector | Hayır | Hayır |
> | servers / firewallRules | Hayır | Hayır |
> | servers / keys | Hayır | Hayır |
> | servers / restorableDroppedDatabases | Hayır | Hayır |
> | servers / serviceobjectives | Hayır | Hayır |
> | servers / tdeCertificates | Hayır | Hayır |
> | virtualClusters | Hayır | Hayır |

<a id="sqlnote" />

> [!NOTE]
> The Master database doesn't support tags, but other databases, including Azure SQL Data Warehouse databases, support tags. Azure SQL Data Warehouse databases must be in Active (not Paused) state.

## <a name="microsoftsqlvirtualmachine"></a>Microsoft.SqlVirtualMachine

> [!div class="mx-tableFixed"]
> | Kaynak türü | Supports tags | Tag in cost report |
> | ------------- | ----------- | ----------- |
> | SqlVirtualMachineGroups | Yes | Yes |
> | SqlVirtualMachineGroups / AvailabilityGroupListeners | Hayır | Hayır |
> | SqlVirtualMachines | Yes | Yes |

## <a name="microsoftstorage"></a>Microsoft.Storage

> [!div class="mx-tableFixed"]
> | Kaynak türü | Supports tags | Tag in cost report |
> | ------------- | ----------- | ----------- |
> | storageAccounts | Yes | Yes |
> | storageAccounts / blobServices | Hayır | Hayır |
> | storageAccounts / fileServices | Hayır | Hayır |
> | storageAccounts / queueServices | Hayır | Hayır |
> | storageAccounts / services | Hayır | Hayır |
> | storageAccounts / services / metricDefinitions | Hayır | Hayır |
> | storageAccounts / tableServices | Hayır | Hayır |
> | usages | Hayır | Hayır |

## <a name="microsoftstoragecache"></a>Microsoft.StorageCache

> [!div class="mx-tableFixed"]
> | Kaynak türü | Supports tags | Tag in cost report |
> | ------------- | ----------- | ----------- |
> | caches | Yes | Yes |
> | caches / storageTargets | Hayır | Hayır |
> | usageModels | Hayır | Hayır |

## <a name="microsoftstoragereplication"></a>Microsoft.StorageReplication

> [!div class="mx-tableFixed"]
> | Kaynak türü | Supports tags | Tag in cost report |
> | ------------- | ----------- | ----------- |
> | replicationGroups | Hayır | Hayır |

## <a name="microsoftstoragesync"></a>Microsoft.StorageSync

> [!div class="mx-tableFixed"]
> | Kaynak türü | Supports tags | Tag in cost report |
> | ------------- | ----------- | ----------- |
> | storageSyncServices | Yes | Yes |
> | storageSyncServices / registeredServers | Hayır | Hayır |
> | storageSyncServices / syncGroups | Hayır | Hayır |
> | storageSyncServices / syncGroups / cloudEndpoints | Hayır | Hayır |
> | storageSyncServices / syncGroups / serverEndpoints | Hayır | Hayır |
> | storageSyncServices / workflows | Hayır | Hayır |

## <a name="microsoftstoragesyncdev"></a>Microsoft.StorageSyncDev

> [!div class="mx-tableFixed"]
> | Kaynak türü | Supports tags | Tag in cost report |
> | ------------- | ----------- | ----------- |
> | storageSyncServices | Yes | Yes |
> | storageSyncServices / registeredServers | Hayır | Hayır |
> | storageSyncServices / syncGroups | Hayır | Hayır |
> | storageSyncServices / syncGroups / cloudEndpoints | Hayır | Hayır |
> | storageSyncServices / syncGroups / serverEndpoints | Hayır | Hayır |
> | storageSyncServices / workflows | Hayır | Hayır |

## <a name="microsoftstoragesyncint"></a>Microsoft.StorageSyncInt

> [!div class="mx-tableFixed"]
> | Kaynak türü | Supports tags | Tag in cost report |
> | ------------- | ----------- | ----------- |
> | storageSyncServices | Yes | Yes |
> | storageSyncServices / registeredServers | Hayır | Hayır |
> | storageSyncServices / syncGroups | Hayır | Hayır |
> | storageSyncServices / syncGroups / cloudEndpoints | Hayır | Hayır |
> | storageSyncServices / syncGroups / serverEndpoints | Hayır | Hayır |
> | storageSyncServices / workflows | Hayır | Hayır |

## <a name="microsoftstorsimple"></a>Microsoft.StorSimple

> [!div class="mx-tableFixed"]
> | Kaynak türü | Supports tags | Tag in cost report |
> | ------------- | ----------- | ----------- |
> | managers | Yes | Yes |

## <a name="microsoftstreamanalytics"></a>Microsoft.StreamAnalytics

> [!div class="mx-tableFixed"]
> | Kaynak türü | Supports tags | Tag in cost report |
> | ------------- | ----------- | ----------- |
> | streamingjobs | Yes (see note below) | Yes |

> [!NOTE]
> You can't add a tag when streamingjobs is running. Stop the resource to add a tag.

## <a name="microsoftsubscription"></a>Microsoft.Subscription

> [!div class="mx-tableFixed"]
> | Kaynak türü | Supports tags | Tag in cost report |
> | ------------- | ----------- | ----------- |
> | cancel | Hayır | Hayır |
> | CreateSubscription | Hayır | Hayır |
> | enable | Hayır | Hayır |
> | rename | Hayır | Hayır |
> | SubscriptionDefinitions | Hayır | Hayır |
> | SubscriptionOperations | Hayır | Hayır |

## <a name="microsofttimeseriesinsights"></a>Microsoft.TimeSeriesInsights

> [!div class="mx-tableFixed"]
> | Kaynak türü | Supports tags | Tag in cost report |
> | ------------- | ----------- | ----------- |
> | environments | Yes | Hayır |
> | environments / accessPolicies | Hayır | Hayır |
> | environments / eventsources | Yes | Hayır |
> | environments / referenceDataSets | Yes | Hayır |

## <a name="microsoftvmwarecloudsimple"></a>Microsoft.VMwareCloudSimple

> [!div class="mx-tableFixed"]
> | Kaynak türü | Supports tags | Tag in cost report |
> | ------------- | ----------- | ----------- |
> | dedicatedCloudNodes | Yes | Yes |
> | dedicatedCloudServices | Yes | Yes |
> | virtualMachines | Yes | Yes |

## <a name="microsoftweb"></a>Microsoft.Web

> [!div class="mx-tableFixed"]
> | Kaynak türü | Supports tags | Tag in cost report |
> | ------------- | ----------- | ----------- |
> | apiManagementAccounts | Hayır | Hayır |
> | apiManagementAccounts / apiAcls | Hayır | Hayır |
> | apiManagementAccounts / apis | Hayır | Hayır |
> | apiManagementAccounts / apis / apiAcls | Hayır | Hayır |
> | apiManagementAccounts / apis / connectionAcls | Hayır | Hayır |
> | apiManagementAccounts / apis / connections | Hayır | Hayır |
> | apiManagementAccounts / apis / connections / connectionAcls | Hayır | Hayır |
> | apiManagementAccounts / apis / localizedDefinitions | Hayır | Hayır |
> | apiManagementAccounts / connectionAcls | Hayır | Hayır |
> | apiManagementAccounts / connections | Hayır | Hayır |
> | billingMeters | Hayır | Hayır |
> | certificates | Yes | Yes |
> | connectionGateways | Yes | Yes |
> | connections | Yes | Yes |
> | customApis | Yes | Yes |
> | deletedSites | Hayır | Hayır |
> | işlevler | Hayır | Hayır |
> | hostingEnvironments | Yes | Yes |
> | hostingEnvironments / multiRolePools | Hayır | Hayır |
> | hostingEnvironments / workerPools | Hayır | Hayır |
> | publishingUsers | Hayır | Hayır |
> | recommendations | Hayır | Hayır |
> | resourceHealthMetadata | Hayır | Hayır |
> | runtimes | Hayır | Hayır |
> | serverFarms | Yes | Yes |
> | serverFarms / eventGridFilters | Hayır | Hayır |
> | sites | Yes | Yes |
> | sites / config  | Hayır | Hayır |
> | sites / eventGridFilters | Hayır | Hayır |
> | sites / hostNameBindings | Hayır | Hayır |
> | sites / networkConfig | Hayır | Hayır |
> | sites / premieraddons | Yes | Yes |
> | sites / slots | Yes | Yes |
> | sites / slots / eventGridFilters | Hayır | Hayır |
> | sites / slots / hostNameBindings | Hayır | Hayır |
> | sites / slots / networkConfig | Hayır | Hayır |
> | sourceControls | Hayır | Hayır |
> | validate | Hayır | Hayır |
> | verifyHostingEnvironmentVnet | Hayır | Hayır |

## <a name="microsoftwindowsdefenderatp"></a>Microsoft.WindowsDefenderATP

> [!div class="mx-tableFixed"]
> | Kaynak türü | Supports tags | Tag in cost report |
> | ------------- | ----------- | ----------- |
> | diagnosticSettings | Hayır | Hayır |
> | diagnosticSettingsCategories | Hayır | Hayır |

## <a name="microsoftwindowsiot"></a>Microsoft.WindowsIoT

> [!div class="mx-tableFixed"]
> | Kaynak türü | Supports tags | Tag in cost report |
> | ------------- | ----------- | ----------- |
> | DeviceServices | Yes | Yes |

## <a name="microsoftworkloadmonitor"></a>Microsoft.WorkloadMonitor

> [!div class="mx-tableFixed"]
> | Kaynak türü | Supports tags | Tag in cost report |
> | ------------- | ----------- | ----------- |
> | components | Hayır | Hayır |
> | componentsSummary | Hayır | Hayır |
> | monitorInstances | Hayır | Hayır |
> | monitorInstancesSummary | Hayır | Hayır |
> | monitors | Hayır | Hayır |
> | notificationSettings | Hayır | Hayır |

## <a name="next-steps"></a>Sonraki adımlar

To learn how to apply tags to resources, see [Use tags to organize your Azure resources](resource-group-using-tags.md).
