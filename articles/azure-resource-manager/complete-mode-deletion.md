---
title: Tam modda silme
description: Shows how resource types handle complete mode deletion in Azure Resource Manager templates.
ms.topic: conceptual
ms.date: 11/19/2019
ms.openlocfilehash: 075dd5071d6c59c6cc73fff8d51a495b012665dd
ms.sourcegitcommit: d6b68b907e5158b451239e4c09bb55eccb5fef89
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/20/2019
ms.locfileid: "74232685"
---
# <a name="deletion-of-azure-resources-for-complete-mode-deployments"></a>Deletion of Azure resources for complete mode deployments

This article describes how resource types handle deletion when not in a template that is deployed in complete mode.

The resource types marked with **Yes** are deleted when the type isn't in the template deployed with complete mode.

The resource types marked with **No** aren't automatically deleted when not in the template; however, they're deleted if the parent resource is deleted. For a full description of the behavior, see [Azure Resource Manager deployment modes](deployment-modes.md).

If you deploy to [more than one resource group in a template](resource-manager-cross-resource-group-deployment.md), resources in the resource group specified in the deployment operation are eligible to be deleted. Resources in the secondary resource groups aren't deleted.

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
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | DomainServices | Yes |
> | DomainServices / oucontainer | Hayır |

## <a name="microsoftaddons"></a>Microsoft.Addons

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | supportProviders | Hayır |

## <a name="microsoftadhybridhealthservice"></a>Microsoft.ADHybridHealthService

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | aadsupportcases | Hayır |
> | addsservices | Hayır |
> | agents | Hayır |
> | anonymousapiusers | Hayır |
> | yapılandırma | Hayır |
> | günlükler | Hayır |
> | reports | Hayır |
> | servicehealthmetrics | Hayır |
> | services | Hayır |

## <a name="microsoftadvisor"></a>Microsoft.Advisor

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | configurations | Hayır |
> | generateRecommendations | Hayır |
> | meta veriler | Hayır |
> | recommendations | Hayır |
> | suppressions | Hayır |

## <a name="microsoftalertsmanagement"></a>Microsoft.AlertsManagement

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | actionRules | Yes |
> | alerts | Hayır |
> | alertsList | Hayır |
> | alertsMetaData | Hayır |
> | alertsSummary | Hayır |
> | alertsSummaryList | Hayır |
> | feedback | Hayır |
> | smartDetectorAlertRules | Yes |
> | smartDetectorRuntimeEnvironments | Hayır |
> | smartGroups | Hayır |

## <a name="microsoftanalysisservices"></a>Microsoft.AnalysisServices

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | sunucular | Yes |

## <a name="microsoftapimanagement"></a>Microsoft.ApiManagement

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | reportFeedback | Hayır |
> | hizmet | Yes |
> | validateServiceName | Hayır |

## <a name="microsoftappconfiguration"></a>Microsoft.AppConfiguration

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | configurationStores | Yes |
> | configurationStores / eventGridFilters | Hayır |

## <a name="microsoftappplatform"></a>Microsoft.AppPlatform

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | Spring | Yes |

## <a name="microsoftattestation"></a>Microsoft.Attestation

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | attestationProviders | Hayır |

## <a name="microsoftauthorization"></a>Microsoft.Authorization

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | classicAdministrators | Hayır |
> | dataAliases | Hayır |
> | denyAssignments | Hayır |
> | elevateAccess | Hayır |
> | findOrphanRoleAssignments | Hayır |
> | locks | Hayır |
> | permissions | Hayır |
> | policyAssignments | Hayır |
> | policyDefinitions | Hayır |
> | policySetDefinitions | Hayır |
> | providerOperations | Hayır |
> | roleAssignments | Hayır |
> | roleDefinitions | Hayır |

## <a name="microsoftautomation"></a>Microsoft.Automation

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | automationAccounts | Yes |
> | automationAccounts / configurations | Yes |
> | automationAccounts / jobs | Hayır |
> | automationAccounts / runbooks | Yes |
> | automationAccounts / softwareUpdateConfigurations | Hayır |
> | automationAccounts / webhooks | Hayır |

## <a name="microsoftazconfig"></a>Microsoft.Azconfig

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | configurationStores | Yes |
> | configurationStores / eventGridFilters | Hayır |

## <a name="microsoftazuregeneva"></a>Microsoft.Azure.Geneva

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | environments | Hayır |
> | environments / accounts | Hayır |
> | environments / accounts / namespaces | Hayır |
> | environments / accounts / namespaces / configurations | Hayır |

## <a name="microsoftazureactivedirectory"></a>Microsoft.AzureActiveDirectory

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | b2cDirectories | Yes |
> | b2ctenants | Hayır |

## <a name="microsoftazuredata"></a>Microsoft.AzureData

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | hybridDataManagers | Yes |
> | postgresInstances | Yes |
> | sqlBigDataClusters | Yes |
> | sqlInstances | Yes |
> | sqlServerRegistrations | Yes |
> | sqlServerRegistrations / sqlServers | Hayır |

## <a name="microsoftazurestack"></a>Microsoft.AzureStack

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | registrations | Yes |
> | registrations / customerSubscriptions | Hayır |
> | registrations / products | Hayır |
> | verificationKeys | Hayır |

## <a name="microsoftbatch"></a>Microsoft.Batch

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | batchAccounts | Yes |

## <a name="microsoftbilling"></a>Microsoft.Billing

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | billingAccounts | Hayır |
> | billingAccounts / agreements | Hayır |
> | billingAccounts / billingPermissions | Hayır |
> | billingAccounts / billingProfiles | Hayır |
> | billingAccounts / billingProfiles / billingPermissions | Hayır |
> | billingAccounts / billingProfiles / billingRoleAssignments | Hayır |
> | billingAccounts / billingProfiles / billingRoleDefinitions | Hayır |
> | billingAccounts / billingProfiles / billingSubscriptions | Hayır |
> | billingAccounts / billingProfiles / createBillingRoleAssignment | Hayır |
> | billingAccounts / billingProfiles / customers | Hayır |
> | billingAccounts / billingProfiles / invoices | Hayır |
> | billingAccounts / billingProfiles / invoices / pricesheet | Hayır |
> | billingAccounts / billingProfiles / invoiceSections | Hayır |
> | billingAccounts / billingProfiles / invoiceSections / billingPermissions | Hayır |
> | billingAccounts / billingProfiles / invoiceSections / billingRoleAssignments | Hayır |
> | billingAccounts / billingProfiles / invoiceSections / billingRoleDefinitions | Hayır |
> | billingAccounts / billingProfiles / invoiceSections / billingSubscriptions | Hayır |
> | billingAccounts / billingProfiles / invoiceSections / createBillingRoleAssignment | Hayır |
> | billingAccounts / billingProfiles / invoiceSections / initiateTransfer | Hayır |
> | billingAccounts / billingProfiles / invoiceSections / products | Hayır |
> | billingAccounts / billingProfiles / invoiceSections / products / transfer | Hayır |
> | billingAccounts / billingProfiles / invoiceSections / products / updateAutoRenew | Hayır |
> | billingAccounts / billingProfiles / invoiceSections / transactions | Hayır |
> | billingAccounts / billingProfiles / invoiceSections / transfers | Hayır |
> | billingAccounts / BillingProfiles / patchOperations | Hayır |
> | billingAccounts / billingProfiles / paymentMethods | Hayır |
> | billingAccounts / billingProfiles / policies | Hayır |
> | billingAccounts / billingProfiles / pricesheet | Hayır |
> | billingAccounts / billingProfiles / pricesheetDownloadOperations | Hayır |
> | billingAccounts / billingProfiles / products | Hayır |
> | billingAccounts / billingProfiles / transactions | Hayır |
> | billingAccounts / billingRoleAssignments | Hayır |
> | billingAccounts / billingRoleDefinitions | Hayır |
> | billingAccounts / billingSubscriptions | Hayır |
> | billingAccounts / billingSubscriptions / invoices | Hayır |
> | billingAccounts / createBillingRoleAssignment | Hayır |
> | billingAccounts / createInvoiceSectionOperations | Hayır |
> | billingAccounts / customers | Hayır |
> | billingAccounts / customers / billingPermissions | Hayır |
> | billingAccounts / customers / billingSubscriptions | Hayır |
> | billingAccounts / customers / initiateTransfer | Hayır |
> | billingAccounts / customers / policies | Hayır |
> | billingAccounts / customers / products | Hayır |
> | billingAccounts / customers / transactions | Hayır |
> | billingAccounts / customers / transfers | Hayır |
> | billingAccounts / departments | Hayır |
> | billingAccounts / enrollmentAccounts | Hayır |
> | billingAccounts / invoices | Hayır |
> | billingAccounts / invoiceSections | Hayır |
> | billingAccounts / invoiceSections / billingSubscriptionMoveOperations | Hayır |
> | billingAccounts / invoiceSections / billingSubscriptions | Hayır |
> | billingAccounts / invoiceSections / billingSubscriptions / transfer | Hayır |
> | billingAccounts / invoiceSections / elevate | Hayır |
> | billingAccounts / invoiceSections / initiateTransfer | Hayır |
> | billingAccounts / invoiceSections / patchOperations | Hayır |
> | billingAccounts / invoiceSections / productMoveOperations | Hayır |
> | billingAccounts / invoiceSections / products | Hayır |
> | billingAccounts / invoiceSections / products / transfer | Hayır |
> | billingAccounts / invoiceSections / products / updateAutoRenew | Hayır |
> | billingAccounts / invoiceSections / transactions | Hayır |
> | billingAccounts / invoiceSections / transfers | Hayır |
> | billingAccounts / lineOfCredit | Hayır |
> | billingAccounts / patchOperations | Hayır |
> | billingAccounts / paymentMethods | Hayır |
> | billingAccounts / products | Hayır |
> | billingAccounts / transactions | Hayır |
> | billingPeriods | Hayır |
> | billingPermissions | Hayır |
> | billingProperty | Hayır |
> | billingRoleAssignments | Hayır |
> | billingRoleDefinitions | Hayır |
> | createBillingRoleAssignment | Hayır |
> | departments | Hayır |
> | enrollmentAccounts | Hayır |
> | invoices | Hayır |
> | transfers | Hayır |
> | transfers / acceptTransfer | Hayır |
> | transfers / declineTransfer | Hayır |
> | transfers / operationStatus | Hayır |
> | transfers / validateTransfer | Hayır |
> | validateAddress | Hayır |

## <a name="microsoftbingmaps"></a>Microsoft.BingMaps

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | mapApis | Yes |
> | updateCommunicationPreference | Hayır |

## <a name="microsoftblockchain"></a>Microsoft.Blockchain

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | blockchainMembers | Yes |
> | cordaMembers | Yes |
> | izleyici | Yes |

## <a name="microsoftblueprint"></a>Microsoft.Blueprint

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | blueprintAssignments | Hayır |
> | blueprintAssignments / assignmentOperations | Hayır |
> | blueprintAssignments / operations | Hayır |
> | blueprints | Hayır |
> | blueprints / artifacts | Hayır |
> | blueprints / versions | Hayır |
> | blueprints / versions / artifacts | Hayır |

## <a name="microsoftbotservice"></a>Microsoft.BotService

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | botServices | Yes |
> | botServices / channels | Hayır |
> | botServices / connections | Hayır |
> | languages | Hayır |
> | şablonları | Hayır |

## <a name="microsoftcache"></a>Microsoft.Cache

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | Redis | Yes |
> | RedisConfigDefinition | Hayır |

## <a name="microsoftcapacity"></a>Microsoft.Capacity

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | appliedReservations | Hayır |
> | calculateExchange | Hayır |
> | calculatePrice | Hayır |
> | calculatePurchasePrice | Hayır |
> | catalogs | Hayır |
> | commercialReservationOrders | Hayır |
> | exchange | Hayır |
> | placePurchaseOrder | Hayır |
> | reservationOrders | Hayır |
> | reservationOrders / calculateRefund | Hayır |
> | reservationOrders / merge | Hayır |
> | reservationOrders / reservations | Hayır |
> | reservationOrders / reservations / revisions | Hayır |
> | reservationOrders / return | Hayır |
> | reservationOrders / split | Hayır |
> | reservationOrders / swap | Hayır |
> | reservations | Hayır |
> | resources | Hayır |
> | validateReservationOrder | Hayır |

## <a name="microsoftcdn"></a>Microsoft.Cdn

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | CdnWebApplicationFirewallManagedRuleSets | Hayır |
> | CdnWebApplicationFirewallPolicies | Yes |
> | edgenodes | Hayır |
> | profiles | Yes |
> | profiles / endpoints | Yes |
> | profiles / endpoints / customdomains | Hayır |
> | profiles / endpoints / origins | Hayır |
> | validateProbe | Hayır |

## <a name="microsoftcertificateregistration"></a>Microsoft.CertificateRegistration

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | certificateOrders | Yes |
> | certificateOrders / certificates | Hayır |
> | validateCertificateRegistrationInformation | Hayır |

## <a name="microsoftclassiccompute"></a>Microsoft.ClassicCompute

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | capabilities | Hayır |
> | domainNames | Yes |
> | domainNames / capabilities | Hayır |
> | domainNames / internalLoadBalancers | Hayır |
> | domainNames / serviceCertificates | Hayır |
> | domainNames / slots | Hayır |
> | domainNames / slots / roles | Hayır |
> | domainNames / slots / roles / metricDefinitions | Hayır |
> | domainNames / slots / roles / metrics | Hayır |
> | moveSubscriptionResources | Hayır |
> | operatingSystemFamilies | Hayır |
> | operatingSystems | Hayır |
> | quotas | Hayır |
> | resourceTypes | Hayır |
> | validateSubscriptionMoveAvailability | Hayır |
> | virtualMachines | Yes |
> | virtualMachines / diagnosticSettings | Hayır |
> | virtualMachines / metricDefinitions | Hayır |
> | virtualMachines / metrics | Hayır |

## <a name="microsoftclassicinfrastructuremigrate"></a>Microsoft.ClassicInfrastructureMigrate

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | classicInfrastructureResources | Hayır |

## <a name="microsoftclassicnetwork"></a>Microsoft.ClassicNetwork

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | capabilities | Hayır |
> | expressRouteCrossConnections | Hayır |
> | expressRouteCrossConnections / peerings | Hayır |
> | gatewaySupportedDevices | Hayır |
> | networkSecurityGroups | Yes |
> | quotas | Hayır |
> | reservedIps | Yes |
> | virtualNetworks | Yes |
> | virtualNetworks / remoteVirtualNetworkPeeringProxies | Hayır |
> | virtualNetworks / virtualNetworkPeerings | Hayır |

## <a name="microsoftclassicstorage"></a>Microsoft.ClassicStorage

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | capabilities | Hayır |
> | disks | Hayır |
> | images | Hayır |
> | osImages | Hayır |
> | osPlatformImages | Hayır |
> | publicImages | Hayır |
> | quotas | Hayır |
> | storageAccounts | Yes |
> | storageAccounts / blobServices | Hayır |
> | storageAccounts / fileServices | Hayır |
> | storageAccounts / metricDefinitions | Hayır |
> | storageAccounts / metrics | Hayır |
> | storageAccounts / queueServices | Hayır |
> | storageAccounts / services | Hayır |
> | storageAccounts / services / diagnosticSettings | Hayır |
> | storageAccounts / services / metricDefinitions | Hayır |
> | storageAccounts / services / metrics | Hayır |
> | storageAccounts / tableServices | Hayır |
> | storageAccounts / vmImages | Hayır |
> | vmImages | Hayır |

## <a name="microsoftcognitiveservices"></a>Microsoft.CognitiveServices

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | accounts | Yes |

## <a name="microsoftcommerce"></a>Microsoft.Commerce

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | RateCard | Hayır |
> | UsageAggregates | Hayır |

## <a name="microsoftcompute"></a>Microsoft.Compute

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | availabilitySets | Yes |
> | diskEncryptionSets | Yes |
> | disks | Yes |
> | galleries | Yes |
> | galleries / applications | Hayır |
> | galleries / applications / versions | Hayır |
> | galleries / images | Hayır |
> | galleries / images / versions | Hayır |
> | hostGroups | Yes |
> | hostGroups / hosts | Yes |
> | images | Yes |
> | proximityPlacementGroups | Yes |
> | restorePointCollections | Yes |
> | restorePointCollections / restorePoints | Hayır |
> | sharedVMImages | Yes |
> | sharedVMImages / versions | Hayır |
> | anlık görüntüler | Yes |
> | virtualMachines | Yes |
> | virtualMachines / extensions | Yes |
> | virtualMachines / metricDefinitions | Hayır |
> | virtualMachineScaleSets | Yes |
> | virtualMachineScaleSets / extensions | Hayır |
> | virtualMachineScaleSets / networkInterfaces | Hayır |
> | virtualMachineScaleSets / publicIPAddresses | Hayır |
> | virtualMachineScaleSets / virtualMachines | Hayır |
> | virtualMachineScaleSets / virtualMachines / networkInterfaces | Hayır |

## <a name="microsoftconsumption"></a>Microsoft.Consumption

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | AggregatedCost | Hayır |
> | Bakiyeler | Hayır |
> | Bütçeler | Hayır |
> | Ücretler | Hayır |
> | CostTags | Hayır |
> | credits | Hayır |
> | etkinlikler | Hayır |
> | Tahminler | Hayır |
> | lots | Hayır |
> | Marketplaces | Hayır |
> | Pricesheets | Hayır |
> | products | Hayır |
> | ReservationDetails | Hayır |
> | ReservationRecommendations | Hayır |
> | ReservationSummaries | Hayır |
> | ReservationTransactions | Hayır |
> | Etiketler | Hayır |
> | tenants | Hayır |
> | Koşullar | Hayır |
> | UsageDetails | Hayır |

## <a name="microsoftcontainerinstance"></a>Microsoft.ContainerInstance

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | containerGroups | Yes |
> | serviceAssociationLinks | Hayır |

## <a name="microsoftcontainerregistry"></a>Microsoft.ContainerRegistry

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | registries | Yes |
> | registries / builds | Hayır |
> | registries / builds / cancel | Hayır |
> | registries / builds / getLogLink | Hayır |
> | registries / buildTasks | Yes |
> | registries / buildTasks / steps | Hayır |
> | registries / eventGridFilters | Hayır |
> | registries / generateCredentials | Hayır |
> | registries / getBuildSourceUploadUrl | Hayır |
> | registries / GetCredentials | Hayır |
> | registries / importImage | Hayır |
> | registries / queueBuild | Hayır |
> | registries / regenerateCredential | Hayır |
> | registries / regenerateCredentials | Hayır |
> | registries / replications | Yes |
> | registries / runs | Hayır |
> | registries / runs / cancel | Hayır |
> | registries / scheduleRun | Hayır |
> | registries / scopeMaps | Hayır |
> | registries / taskRuns | Yes |
> | registries / tasks | Yes |
> | registries / tokens | Hayır |
> | registries / updatePolicies | Hayır |
> | registries / webhooks | Yes |
> | registries / webhooks / getCallbackConfig | Hayır |
> | registries / webhooks / ping | Hayır |

## <a name="microsoftcontainerservice"></a>Microsoft.ContainerService

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | containerServices | Yes |
> | managedClusters | Yes |
> | openShiftManagedClusters | Yes |

## <a name="microsoftcortanaanalytics"></a>Microsoft.CortanaAnalytics

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | accounts | Yes |

## <a name="microsoftcostmanagement"></a>Microsoft.CostManagement

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | Uyarılar | Hayır |
> | BillingAccounts | Hayır |
> | Bütçeler | Hayır |
> | CloudConnectors | Hayır |
> | Bağlayıcılar | Yes |
> | Departments | Hayır |
> | Boyutlar | Hayır |
> | EnrollmentAccounts | Hayır |
> | Dışarı aktarmalar | Hayır |
> | ExternalBillingAccounts | Hayır |
> | ExternalBillingAccounts / Alerts | Hayır |
> | ExternalBillingAccounts / Dimensions | Hayır |
> | ExternalBillingAccounts / Forecast | Hayır |
> | ExternalBillingAccounts / Query | Hayır |
> | ExternalSubscriptions | Hayır |
> | ExternalSubscriptions / Alerts | Hayır |
> | ExternalSubscriptions / Dimensions | Hayır |
> | ExternalSubscriptions / Forecast | Hayır |
> | ExternalSubscriptions / Query | Hayır |
> | Forecast | Hayır |
> | Sorgu | Hayır |
> | register | Hayır |
> | Reportconfigs | Hayır |
> | Raporlar | Hayır |
> | Ayarlar | Hayır |
> | showbackRules | Hayır |
> | Görünümler | Hayır |

## <a name="microsoftcustomerlockbox"></a>Microsoft.CustomerLockbox

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | requests | Hayır |

## <a name="microsoftcustomproviders"></a>Microsoft.CustomProviders

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | associations | Hayır |
> | resourceProviders | Yes |

## <a name="microsoftdatabox"></a>Microsoft.DataBox

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | jobs | Yes |

## <a name="microsoftdataboxedge"></a>Microsoft.DataBoxEdge

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | DataBoxEdgeDevices | Yes |

## <a name="microsoftdatabricks"></a>Microsoft.Databricks

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | workspaces | Yes |
> | workspaces / virtualNetworkPeerings | Hayır |

## <a name="microsoftdatacatalog"></a>Microsoft.DataCatalog

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | catalogs | Yes |
> | datacatalogs | Yes |
> | datacatalogs / datasources | Hayır |
> | datacatalogs / datasources / scans | Hayır |
> | datacatalogs / datasources / scans / datasets | Hayır |
> | datacatalogs / datasources / scans / triggers | Hayır |

## <a name="microsoftdatafactory"></a>Microsoft.DataFactory

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | dataFactories | Yes |
> | dataFactories / diagnosticSettings | Hayır |
> | dataFactories / metricDefinitions | Hayır |
> | dataFactorySchema | Hayır |
> | factories | Yes |
> | factories / integrationRuntimes | Hayır |

## <a name="microsoftdatalakeanalytics"></a>Microsoft.DataLakeAnalytics

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | accounts | Yes |
> | accounts / dataLakeStoreAccounts | Hayır |
> | accounts / storageAccounts | Hayır |
> | accounts / storageAccounts / containers | Hayır |
> | accounts / transferAnalyticsUnits | Hayır |

## <a name="microsoftdatalakestore"></a>Microsoft.DataLakeStore

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | accounts | Yes |
> | accounts / eventGridFilters | Hayır |
> | accounts / firewallRules | Hayır |

## <a name="microsoftdatamigration"></a>Microsoft.DataMigration

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | services | Yes |
> | services / projects | Yes |

## <a name="microsoftdatashare"></a>Microsoft.DataShare

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | accounts | Yes |
> | accounts / shares | Hayır |
> | accounts / shares / datasets | Hayır |
> | accounts / shares / invitations | Hayır |
> | accounts / shares / providersharesubscriptions | Hayır |
> | accounts / shares / synchronizationSettings | Hayır |
> | accounts / sharesubscriptions | Hayır |
> | accounts / sharesubscriptions / consumerSourceDataSets | Hayır |
> | accounts / sharesubscriptions / datasetmappings | Hayır |
> | accounts / sharesubscriptions / triggers | Hayır |

## <a name="microsoftdbformariadb"></a>Microsoft.DBforMariaDB

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | sunucular | Yes |
> | servers / advisors | Hayır |
> | servers / privateEndpointConnectionProxies | Hayır |
> | servers / privateEndpointConnections | Hayır |
> | servers / privateLinkResources | Hayır |
> | servers / queryTexts | Hayır |
> | servers / recoverableServers | Hayır |
> | servers / topQueryStatistics | Hayır |
> | servers / virtualNetworkRules | Hayır |
> | servers / waitStatistics | Hayır |

## <a name="microsoftdbformysql"></a>Microsoft.DBforMySQL

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | sunucular | Yes |
> | servers / advisors | Hayır |
> | servers / privateEndpointConnectionProxies | Hayır |
> | servers / privateEndpointConnections | Hayır |
> | servers / privateLinkResources | Hayır |
> | servers / queryTexts | Hayır |
> | servers / recoverableServers | Hayır |
> | servers / topQueryStatistics | Hayır |
> | servers / virtualNetworkRules | Hayır |
> | servers / waitStatistics | Hayır |

## <a name="microsoftdbforpostgresql"></a>Microsoft.DBforPostgreSQL

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | serverGroups | Yes |
> | sunucular | Yes |
> | servers / advisors | Hayır |
> | servers / keys | Hayır |
> | servers / privateEndpointConnectionProxies | Hayır |
> | servers / privateEndpointConnections | Hayır |
> | servers / privateLinkResources | Hayır |
> | servers / queryTexts | Hayır |
> | servers / recoverableServers | Hayır |
> | servers / topQueryStatistics | Hayır |
> | servers / virtualNetworkRules | Hayır |
> | servers / waitStatistics | Hayır |
> | serversv2 | Yes |

## <a name="microsoftdeploymentmanager"></a>Microsoft.DeploymentManager

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | artifactSources | Yes |
> | rollouts | Yes |
> | serviceTopologies | Yes |
> | serviceTopologies / services | Yes |
> | serviceTopologies / services / serviceUnits | Yes |
> | adımlar | Yes |

## <a name="microsoftdesktopvirtualization"></a>Microsoft.DesktopVirtualization

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | applicationgroups | Yes |
> | applicationgroups / applications | Hayır |
> | applicationgroups / desktops | Hayır |
> | applicationgroups / startmenuitems | Hayır |
> | hostpools | Yes |
> | hostpools / sessionhosts | Hayır |
> | hostpools / sessionhosts / usersessions | Hayır |
> | hostpools / usersessions | Hayır |
> | workspaces | Yes |

## <a name="microsoftdevices"></a>Microsoft.Devices

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | ElasticPools | Yes |
> | ElasticPools / IotHubTenants | Yes |
> | IotHubs | Yes |
> | IotHubs / eventGridFilters | Hayır |
> | ProvisioningServices | Yes |
> | usages | Hayır |

## <a name="microsoftdevops"></a>Microsoft.DevOps

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | pipelines | Yes |

## <a name="microsoftdevspaces"></a>Microsoft.DevSpaces

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | controllers | Yes |

## <a name="microsoftdevtestlab"></a>Microsoft.DevTestLab

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | labcenters | Yes |
> | labs | Yes |
> | labs / environments | Yes |
> | labs / serviceRunners | Yes |
> | labs / virtualMachines | Yes |
> | schedules | Yes |

## <a name="microsoftdocumentdb"></a>Microsoft.DocumentDB

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | databaseAccountNames | Hayır |
> | databaseAccounts | Yes |

## <a name="microsoftdomainregistration"></a>Microsoft.DomainRegistration

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | domains | Yes |
> | domains / domainOwnershipIdentifiers | Hayır |
> | generateSsoRequest | Hayır |
> | topLevelDomains | Hayır |
> | validateDomainRegistrationInformation | Hayır |

## <a name="microsoftdynamicslcs"></a>Microsoft.DynamicsLcs

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | lcsprojects | Hayır |
> | lcsprojects / clouddeployments | Hayır |
> | lcsprojects / connectors | Hayır |

## <a name="microsoftenterpriseknowledgegraph"></a>Microsoft.EnterpriseKnowledgeGraph

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | services | Yes |

## <a name="microsofteventgrid"></a>Microsoft.EventGrid

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | domains | Yes |
> | domains / topics | Hayır |
> | eventSubscriptions | Hayır |
> | extensionTopics | Hayır |
> | topics | Yes |
> | topicTypes | Hayır |

## <a name="microsofteventhub"></a>Microsoft.EventHub

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | clusters | Yes |
> | namespaces | Yes |
> | namespaces / authorizationrules | Hayır |
> | namespaces / disasterrecoveryconfigs | Hayır |
> | namespaces / eventhubs | Hayır |
> | namespaces / eventhubs / authorizationrules | Hayır |
> | namespaces / eventhubs / consumergroups | Hayır |
> | namespaces / networkrulesets | Hayır |

## <a name="microsoftfeatures"></a>Microsoft.Features

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | SaaS Uygulamaları Geliştirme | Hayır |
> | providers | Hayır |

## <a name="microsoftgallery"></a>Microsoft.Gallery

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | enroll | Hayır |
> | galleryitems | Hayır |
> | generateartifactaccessuri | Hayır |
> | myareas | Hayır |
> | myareas / areas | Hayır |
> | myareas / areas / areas | Hayır |
> | myareas / areas / areas / galleryitems | Hayır |
> | myareas / areas / galleryitems | Hayır |
> | myareas / galleryitems | Hayır |
> | register | Hayır |
> | resources | Hayır |
> | retrieveresourcesbyid | Hayır |

## <a name="microsoftgenomics"></a>Microsoft.Genomics

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | accounts | Yes |

## <a name="microsoftguestconfiguration"></a>Microsoft.GuestConfiguration

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | configurationProfileAssignments | Hayır |
> | guestConfigurationAssignments | Hayır |
> | software | Hayır |
> | softwareUpdateProfile | Hayır |
> | softwareUpdates | Hayır |

## <a name="microsofthanaonazure"></a>Microsoft.HanaOnAzure

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | hanaInstances | Yes |
> | sapMonitors | Yes |

## <a name="microsofthardwaresecuritymodules"></a>Microsoft.HardwareSecurityModules

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | dedicatedHSMs | Yes |

## <a name="microsofthdinsight"></a>Microsoft.HDInsight

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | clusters | Yes |
> | clusters / applications | Hayır |

## <a name="microsofthealthcareapis"></a>Microsoft.HealthcareApis

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | services | Yes |

## <a name="microsofthybridcompute"></a>Microsoft.HybridCompute

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | machines | Yes |
> | machines / extensions | Yes |

## <a name="microsofthybriddata"></a>Microsoft.HybridData

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | dataManagers | Yes |

## <a name="microsofthydra"></a>Microsoft.Hydra

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | components | Yes |
> | networkScopes | Yes |

## <a name="microsoftimportexport"></a>Microsoft.ImportExport

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | jobs | Yes |

## <a name="microsoftintune"></a>Microsoft.Intune

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | diagnosticSettings | Hayır |
> | diagnosticSettingsCategories | Hayır |

## <a name="microsoftiotcentral"></a>Microsoft.IoTCentral

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | appTemplates | Hayır |
> | IoTApps | Yes |

## <a name="microsoftiotspaces"></a>Microsoft.IoTSpaces

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | Graph | Yes |

## <a name="microsoftkeyvault"></a>Microsoft.KeyVault

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | deletedVaults | Hayır |
> | hsmPools | Yes |
> | vaults | Yes |
> | vaults / accessPolicies | Hayır |
> | vaults / eventGridFilters | Hayır |
> | vaults / secrets | Hayır |

## <a name="microsoftkusto"></a>Microsoft.Kusto

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | clusters | Yes |
> | clusters / attacheddatabaseconfigurations | Hayır |
> | clusters / databases | Hayır |
> | clusters / databases / dataconnections | Hayır |
> | clusters / databases / eventhubconnections | Hayır |
> | clusters / sharedidentities | Hayır |

## <a name="microsoftlabservices"></a>Microsoft.LabServices

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | labaccounts | Yes |
> | kullanıcı | Hayır |

## <a name="microsoftlogic"></a>Microsoft.Logic

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | hostingEnvironments | Yes |
> | integrationAccounts | Yes |
> | integrationServiceEnvironments | Yes |
> | integrationServiceEnvironments / managedApis | Yes |
> | isolatedEnvironments | Yes |
> | workflows | Yes |

## <a name="microsoftmachinelearning"></a>Microsoft.MachineLearning

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | commitmentPlans | Yes |
> | webServices | Yes |
> | Çalışma alanları | Yes |

## <a name="microsoftmachinelearningservices"></a>Microsoft.MachineLearningServices

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | workspaces | Yes |
> | workspaces / computes | Hayır |
> | workspaces / eventGridFilters | Hayır |

## <a name="microsoftmanagedidentity"></a>Microsoft.ManagedIdentity

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | Identities | Hayır |
> | userAssignedIdentities | Yes |

## <a name="microsoftmanagedservices"></a>Microsoft.ManagedServices

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | marketplaceRegistrationDefinitions | Hayır |
> | registrationAssignments | Hayır |
> | registrationDefinitions | Hayır |

## <a name="microsoftmanagement"></a>Microsoft.Management

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | getEntities | Hayır |
> | managementGroups | Hayır |
> | resources | Hayır |
> | startTenantBackfill | Hayır |
> | tenantBackfillStatus | Hayır |

## <a name="microsoftmaps"></a>Microsoft.Maps

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | accounts | Yes |
> | accounts / eventGridFilters | Hayır |

## <a name="microsoftmarketplace"></a>Microsoft.Marketplace

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | offers | Hayır |
> | offerTypes | Hayır |
> | offerTypes / publishers | Hayır |
> | offerTypes / publishers / offers | Hayır |
> | offerTypes / publishers / offers / plans | Hayır |
> | offerTypes / publishers / offers / plans / agreements | Hayır |
> | offerTypes / publishers / offers / plans / configs | Hayır |
> | offerTypes / publishers / offers / plans / configs / importImage | Hayır |
> | privategalleryitems | Hayır |
> | products | Hayır |
> | publishers | Hayır |
> | publishers / offers | Hayır |
> | publishers / offers / amendments | Hayır |

## <a name="microsoftmarketplaceapps"></a>Microsoft.MarketplaceApps

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | classicDevServices | Yes |
> | updateCommunicationPreference | Hayır |

## <a name="microsoftmarketplaceordering"></a>Microsoft.MarketplaceOrdering

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | agreements | Hayır |
> | offertypes | Hayır |

## <a name="microsoftmedia"></a>Microsoft.Media

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | mediaservices | Yes |
> | mediaservices / accountFilters | Hayır |
> | mediaservices / assets | Hayır |
> | mediaservices / assets / assetFilters | Hayır |
> | mediaservices / contentKeyPolicies | Hayır |
> | mediaservices / eventGridFilters | Hayır |
> | mediaservices / liveEventOperations | Hayır |
> | mediaservices / liveEvents | Yes |
> | mediaservices / liveEvents / liveOutputs | Hayır |
> | mediaservices / liveOutputOperations | Hayır |
> | mediaservices / mediaGraphs | Hayır |
> | mediaservices / streamingEndpointOperations | Hayır |
> | mediaservices / streamingEndpoints | Yes |
> | mediaservices / streamingLocators | Hayır |
> | mediaservices / streamingPolicies | Hayır |
> | mediaservices / transforms | Hayır |
> | mediaservices / transforms / jobs | Hayır |

## <a name="microsoftmicroservices4spring"></a>Microsoft.Microservices4Spring

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | appClusters | Yes |

## <a name="microsoftmigrate"></a>Microsoft.Migrate

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | assessmentProjects | Yes |
> | migrateprojects | Yes |
> | projects | Yes |

## <a name="microsoftmixedreality"></a>Microsoft.MixedReality

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | holographicsBroadcastAccounts | Yes |
> | objectUnderstandingAccounts | Yes |
> | remoteRenderingAccounts | Yes |
> | spatialAnchorsAccounts | Yes |
> | surfaceReconstructionAccounts | Yes |

## <a name="microsoftnetapp"></a>Microsoft.NetApp

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | netAppAccounts | Yes |
> | netAppAccounts / capacityPools | Yes |
> | netAppAccounts / capacityPools / volumes | Yes |
> | netAppAccounts / capacityPools / volumes / mountTargets | Yes |
> | netAppAccounts / capacityPools / volumes / snapshots | Yes |
## <a name="microsoftnetwork"></a>Microsoft.Network

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | applicationGateways | Yes |
> | applicationGatewayWebApplicationFirewallPolicies | Yes |
> | applicationSecurityGroups | Yes |
> | azureFirewallFqdnTags | Hayır |
> | azureFirewalls | Yes |
> | bastionHosts | Yes |
> | bgpServiceCommunities | Hayır |
> | connections | Yes |
> | ddosCustomPolicies | Yes |
> | ddosProtectionPlans | Yes |
> | dnsOperationStatuses | Hayır |
> | dnszones | Yes |
> | dnszones / A | Hayır |
> | dnszones / AAAA | Hayır |
> | dnszones / all | Hayır |
> | dnszones / CAA | Hayır |
> | dnszones / CNAME | Hayır |
> | dnszones / MX | Hayır |
> | dnszones / NS | Hayır |
> | dnszones / PTR | Hayır |
> | dnszones / recordsets | Hayır |
> | dnszones / SOA | Hayır |
> | dnszones / SRV | Hayır |
> | dnszones / TXT | Hayır |
> | expressRouteCircuits | Yes |
> | expressRouteCrossConnections | Yes |
> | expressRouteGateways | Yes |
> | expressRoutePorts | Yes |
> | expressRouteServiceProviders | Hayır |
> | firewallPolicies | Yes |
> | frontdoors | Yes |
> | frontdoorWebApplicationFirewallManagedRuleSets | Hayır |
> | frontdoorWebApplicationFirewallPolicies | Yes |
> | getDnsResourceReference | Hayır |
> | internalNotify | Hayır |
> | loadBalancers | Yes |
> | localNetworkGateways | Yes |
> | natGateways | Yes |
> | networkIntentPolicies | Yes |
> | networkInterfaces | Yes |
> | networkProfiles | Yes |
> | networkSecurityGroups | Yes |
> | networkWatchers | Yes |
> | networkWatchers / connectionMonitors | Yes |
> | networkWatchers / lenses | Yes |
> | networkWatchers / pingMeshes | Yes |
> | p2sVpnGateways | Yes |
> | privateDnsOperationStatuses | Hayır |
> | privateDnsZones | Yes |
> | privateDnsZones / A | Hayır |
> | privateDnsZones / AAAA | Hayır |
> | privateDnsZones / all | Hayır |
> | privateDnsZones / CNAME | Hayır |
> | privateDnsZones / MX | Hayır |
> | privateDnsZones / PTR | Hayır |
> | privateDnsZones / SOA | Hayır |
> | privateDnsZones / SRV | Hayır |
> | privateDnsZones / TXT | Hayır |
> | privateDnsZones / virtualNetworkLinks | Yes |
> | privateEndpoints | Yes |
> | privateLinkServices | Yes |
> | publicIPAddresses | Yes |
> | publicIPPrefixes | Yes |
> | routeFilters | Yes |
> | routeTables | Yes |
> | serviceEndpointPolicies | Yes |
> | trafficManagerGeographicHierarchies | Hayır |
> | trafficmanagerprofiles | Yes |
> | trafficmanagerprofiles / heatMaps | Hayır |
> | trafficManagerUserMetricsKeys | Hayır |
> | virtualHubs | Yes |
> | virtualNetworkGateways | Yes |
> | virtualNetworks | Yes |
> | virtualNetworkTaps | Yes |
> | virtualWans | Yes |
> | vpnGateways | Yes |
> | vpnSites | Yes |
> | webApplicationFirewallPolicies | Yes |

## <a name="microsoftnotificationhubs"></a>Microsoft.NotificationHubs

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | namespaces | Yes |
> | namespaces / notificationHubs | Yes |

## <a name="microsoftobjectstore"></a>Microsoft.ObjectStore

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | osNamespaces | Yes |

## <a name="microsoftoffazure"></a>Microsoft.OffAzure

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | HyperVSites | Yes |
> | ImportSites | Yes |
> | ServerSites | Yes |
> | VMwareSites | Yes |

## <a name="microsoftoperationalinsights"></a>Microsoft.OperationalInsights

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | clusters | Yes |
> | cihazlar | Hayır |
> | linkTargets | Hayır |
> | storageInsightConfigs | Hayır |
> | workspaces | Yes |
> | workspaces / dataSources | Hayır |
> | workspaces / linkedServices | Hayır |
> | workspaces / query | Hayır |

## <a name="microsoftoperationsmanagement"></a>Microsoft.OperationsManagement

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | managementassociations | Hayır |
> | managementconfigurations | Yes |
> | çözümler | Yes |
> | views | Yes |

## <a name="microsoftpeering"></a>Microsoft.Peering

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | legacyPeerings | Hayır |
> | peerAsns | Hayır |
> | peerings | Yes |
> | peeringServiceProviders | Hayır |
> | peeringServices | Yes |

## <a name="microsoftpolicyinsights"></a>Microsoft.PolicyInsights

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | policyEvents | Hayır |
> | policyMetadata | Hayır |
> | policyStates | Hayır |
> | policyTrackedResources | Hayır |
> | remediations | Hayır |

## <a name="microsoftportal"></a>Microsoft.Portal

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | consoles | Hayır |
> | dashboards | Yes |
> | userSettings | Hayır |

## <a name="microsoftpowerbi"></a>Microsoft.PowerBI

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | workspaceCollections | Yes |

## <a name="microsoftpowerbidedicated"></a>Microsoft.PowerBIDedicated

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | capacities | Yes |

## <a name="microsoftrecoveryservices"></a>Microsoft.RecoveryServices

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | backupProtectedItems | Hayır |
> | vaults | Yes |

## <a name="microsoftrelay"></a>Microsoft.Relay

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | namespaces | Yes |
> | namespaces / authorizationrules | Hayır |
> | namespaces / hybridconnections | Hayır |
> | namespaces / hybridconnections / authorizationrules | Hayır |
> | namespaces / wcfrelays | Hayır |
> | namespaces / wcfrelays / authorizationrules | Hayır |

## <a name="microsoftremoteapp"></a>Microsoft.RemoteApp

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | accounts | Hayır |
> | collections | Yes |
> | collections / applications | Hayır |
> | collections / securityprincipals | Hayır |
> | templateImages | Hayır |

## <a name="microsoftresourcegraph"></a>Microsoft.ResourceGraph

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | sorgu | Yes |
> | resourceChangeDetails | Hayır |
> | resourceChanges | Hayır |
> | resources | Hayır |
> | resourcesHistory | Hayır |
> | subscriptionsStatus | Hayır |

## <a name="microsoftresourcehealth"></a>Microsoft.ResourceHealth

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | availabilityStatuses | Hayır |
> | childAvailabilityStatuses | Hayır |
> | childResources | Hayır |
> | etkinlikler | Hayır |
> | impactedResources | Hayır |
> | meta veriler | Hayır |
> | notifications | Hayır |

## <a name="microsoftresources"></a>Microsoft.Resources

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | deployments | Hayır |
> | deployments / operations | Hayır |
> | deploymentScripts | Yes |
> | deploymentScripts / logs | Hayır |
> | links | Hayır |
> | notifyResourceJobs | Hayır |
> | providers | Hayır |
> | resourceGroups | Hayır |
> | resources | Hayır |
> | subscriptions | Hayır |
> | subscriptions / providers | Hayır |
> | subscriptions / resources | Hayır |
> | subscriptions / tagnames | Hayır |
> | subscriptions / tagNames / tagValues | Hayır |
> | tenants | Hayır |

## <a name="microsoftsaas"></a>Microsoft.SaaS

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | uygulama | Yes |
> | saasresources | Hayır |

## <a name="microsoftscheduler"></a>Microsoft.Scheduler

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | jobcollections | Yes |

## <a name="microsoftsearch"></a>Microsoft.Search

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
> | allowedConnections | Hayır |
> | applicationWhitelistings | Hayır |
> | assessmentMetadata | Hayır |
> | assessments | Hayır |
> | autoDismissAlertsRules | Hayır |
> | automations | Yes |
> | AutoProvisioningSettings | Hayır |
> | Compliances | Hayır |
> | dataCollectionAgents | Hayır |
> | deviceSecurityGroups | Hayır |
> | discoveredSecuritySolutions | Hayır |
> | externalSecuritySolutions | Hayır |
> | InformationProtectionPolicies | Hayır |
> | iotSecuritySolutions | Yes |
> | iotSecuritySolutions / analyticsModels | Hayır |
> | iotSecuritySolutions / analyticsModels / aggregatedAlerts | Hayır |
> | iotSecuritySolutions / analyticsModels / aggregatedRecommendations | Hayır |
> | jitNetworkAccessPolicies | Hayır |
> | networkData | Hayır |
> | policies | Hayır |
> | pricings | Hayır |
> | regulatoryComplianceStandards | Hayır |
> | regulatoryComplianceStandards / regulatoryComplianceControls | Hayır |
> | regulatoryComplianceStandards / regulatoryComplianceControls / regulatoryComplianceAssessments | Hayır |
> | securityContacts | Hayır |
> | securitySolutions | Hayır |
> | securitySolutionsReferenceData | Hayır |
> | securityStatuses | Hayır |
> | securityStatusesSummaries | Hayır |
> | serverVulnerabilityAssessments | Hayır |
> | ayarlar | Hayır |
> | subAssessments | Hayır |
> | Görevler | Hayır |
> | topologies | Hayır |
> | workspaceSettings | Hayır |

## <a name="microsoftsecuritygraph"></a>Microsoft.SecurityGraph

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | diagnosticSettings | Hayır |
> | diagnosticSettingsCategories | Hayır |

## <a name="microsoftsecurityinsights"></a>Microsoft.SecurityInsights

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | aggregations | Hayır |
> | alertRules | Hayır |
> | alertRuleTemplates | Hayır |
> | bookmarks | Hayır |
> | cases | Hayır |
> | dataConnectors | Hayır |
> | entities | Hayır |
> | entityQueries | Hayır |
> | officeConsents | Hayır |
> | ayarlar | Hayır |

## <a name="microsoftservicebus"></a>Microsoft.ServiceBus

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | namespaces | Yes |
> | namespaces / authorizationrules | Hayır |
> | namespaces / disasterrecoveryconfigs | Hayır |
> | namespaces / eventgridfilters | Hayır |
> | namespaces / networkrulesets | Hayır |
> | namespaces / queues | Hayır |
> | namespaces / queues / authorizationrules | Hayır |
> | namespaces / topics | Hayır |
> | namespaces / topics / authorizationrules | Hayır |
> | namespaces / topics / subscriptions | Hayır |
> | namespaces / topics / subscriptions / rules | Hayır |
> | premiumMessagingRegions | Hayır |

## <a name="microsoftservicefabric"></a>Microsoft.ServiceFabric

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | uygulama | Yes |
> | clusters | Yes |
> | clusters / applications | Hayır |
> | containerGroups | Yes |
> | containerGroupSets | Yes |
> | edgeclusters | Yes |
> | edgeclusters / applications | Hayır |
> | networks | Yes |
> | secretstores | Yes |
> | secretstores / certificates | Hayır |
> | secretstores / secrets | Hayır |
> | volumes | Yes |

## <a name="microsoftservicefabricmesh"></a>Microsoft.ServiceFabricMesh

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | uygulama | Yes |
> | containerGroups | Yes |
> | gateways | Yes |
> | networks | Yes |
> | secrets | Yes |
> | volumes | Yes |

## <a name="microsoftservices"></a>Microsoft.Services

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | providerRegistrations | Hayır |
> | providerRegistrations / resourceTypeRegistrations | Hayır |
> | rollouts | Yes |

## <a name="microsoftsignalrservice"></a>Microsoft.SignalRService

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | SignalR | Yes |
> | SignalR / eventGridFilters | Hayır |

## <a name="microsoftsiterecovery"></a>Microsoft.SiteRecovery

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | SiteRecoveryVault | Yes |

## <a name="microsoftsoftwareplan"></a>Microsoft.SoftwarePlan

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | hybridUseBenefits | Hayır |

## <a name="microsoftsolutions"></a>Microsoft.Solutions

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | applicationDefinitions | Yes |
> | uygulama | Yes |
> | jitRequests | Yes |

## <a name="microsoftsql"></a>Microsoft.SQL

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | managedInstances | Yes |
> | managedInstances / databases | Yes |
> | managedInstances / databases / backupShortTermRetentionPolicies | Hayır |
> | managedInstances / databases / schemas / tables / columns / sensitivityLabels | Hayır |
> | managedInstances / databases / vulnerabilityAssessments | Hayır |
> | managedInstances / databases / vulnerabilityAssessments / rules / baselines | Hayır |
> | managedInstances / encryptionProtector | Hayır |
> | managedInstances / keys | Hayır |
> | managedInstances / restorableDroppedDatabases / backupShortTermRetentionPolicies | Hayır |
> | managedInstances / vulnerabilityAssessments | Hayır |
> | sunucular | Yes |
> | servers / administrators | Hayır |
> | servers / communicationLinks | Hayır |
> | servers / databases | Yes |
> | servers / encryptionProtector | Hayır |
> | servers / firewallRules | Hayır |
> | servers / keys | Hayır |
> | servers / restorableDroppedDatabases | Hayır |
> | servers / serviceobjectives | Hayır |
> | servers / tdeCertificates | Hayır |
> | virtualClusters | Hayır |

## <a name="microsoftsqlvirtualmachine"></a>Microsoft.SqlVirtualMachine

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | SqlVirtualMachineGroups | Yes |
> | SqlVirtualMachineGroups / AvailabilityGroupListeners | Hayır |
> | SqlVirtualMachines | Yes |

## <a name="microsoftstorage"></a>Microsoft.Storage

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | storageAccounts | Yes |
> | storageAccounts / blobServices | Hayır |
> | storageAccounts / fileServices | Hayır |
> | storageAccounts / queueServices | Hayır |
> | storageAccounts / services | Hayır |
> | storageAccounts / services / metricDefinitions | Hayır |
> | storageAccounts / tableServices | Hayır |
> | usages | Hayır |

## <a name="microsoftstoragecache"></a>Microsoft.StorageCache

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | caches | Yes |
> | caches / storageTargets | Hayır |
> | usageModels | Hayır |

## <a name="microsoftstoragereplication"></a>Microsoft.StorageReplication

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | replicationGroups | Hayır |

## <a name="microsoftstoragesync"></a>Microsoft.StorageSync

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | storageSyncServices | Yes |
> | storageSyncServices / registeredServers | Hayır |
> | storageSyncServices / syncGroups | Hayır |
> | storageSyncServices / syncGroups / cloudEndpoints | Hayır |
> | storageSyncServices / syncGroups / serverEndpoints | Hayır |
> | storageSyncServices / workflows | Hayır |

## <a name="microsoftstoragesyncdev"></a>Microsoft.StorageSyncDev

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | storageSyncServices | Yes |
> | storageSyncServices / registeredServers | Hayır |
> | storageSyncServices / syncGroups | Hayır |
> | storageSyncServices / syncGroups / cloudEndpoints | Hayır |
> | storageSyncServices / syncGroups / serverEndpoints | Hayır |
> | storageSyncServices / workflows | Hayır |

## <a name="microsoftstoragesyncint"></a>Microsoft.StorageSyncInt

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | storageSyncServices | Yes |
> | storageSyncServices / registeredServers | Hayır |
> | storageSyncServices / syncGroups | Hayır |
> | storageSyncServices / syncGroups / cloudEndpoints | Hayır |
> | storageSyncServices / syncGroups / serverEndpoints | Hayır |
> | storageSyncServices / workflows | Hayır |

## <a name="microsoftstorsimple"></a>Microsoft.StorSimple

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | managers | Yes |

## <a name="microsoftstreamanalytics"></a>Microsoft.StreamAnalytics

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | streamingjobs | Yes |

## <a name="microsoftsubscription"></a>Microsoft.Subscription

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | cancel | Hayır |
> | CreateSubscription | Hayır |
> | enable | Hayır |
> | rename | Hayır |
> | SubscriptionDefinitions | Hayır |
> | SubscriptionOperations | Hayır |

## <a name="microsofttimeseriesinsights"></a>Microsoft.TimeSeriesInsights

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | environments | Yes |
> | environments / accessPolicies | Hayır |
> | environments / eventsources | Yes |
> | environments / referenceDataSets | Yes |

## <a name="microsoftvmwarecloudsimple"></a>Microsoft.VMwareCloudSimple

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | dedicatedCloudNodes | Yes |
> | dedicatedCloudServices | Yes |
> | virtualMachines | Yes |

## <a name="microsoftweb"></a>Microsoft.Web

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | apiManagementAccounts | Hayır |
> | apiManagementAccounts / apiAcls | Hayır |
> | apiManagementAccounts / apis | Hayır |
> | apiManagementAccounts / apis / apiAcls | Hayır |
> | apiManagementAccounts / apis / connectionAcls | Hayır |
> | apiManagementAccounts / apis / connections | Hayır |
> | apiManagementAccounts / apis / connections / connectionAcls | Hayır |
> | apiManagementAccounts / apis / localizedDefinitions | Hayır |
> | apiManagementAccounts / connectionAcls | Hayır |
> | apiManagementAccounts / connections | Hayır |
> | billingMeters | Hayır |
> | certificates | Yes |
> | connectionGateways | Yes |
> | connections | Yes |
> | customApis | Yes |
> | deletedSites | Hayır |
> | işlevler | Hayır |
> | hostingEnvironments | Yes |
> | hostingEnvironments / multiRolePools | Hayır |
> | hostingEnvironments / workerPools | Hayır |
> | publishingUsers | Hayır |
> | recommendations | Hayır |
> | resourceHealthMetadata | Hayır |
> | runtimes | Hayır |
> | serverFarms | Yes |
> | serverFarms / eventGridFilters | Hayır |
> | sites | Yes |
> | sites/config  | Hayır |
> | sites / eventGridFilters | Hayır |
> | sites / hostNameBindings | Hayır |
> | sites / networkConfig | Hayır |
> | sites / premieraddons | Yes |
> | sites / slots | Yes |
> | sites / slots / eventGridFilters | Hayır |
> | sites / slots / hostNameBindings | Hayır |
> | sites / slots / networkConfig | Hayır |
> | sourceControls | Hayır |
> | validate | Hayır |
> | verifyHostingEnvironmentVnet | Hayır |

## <a name="microsoftwindowsdefenderatp"></a>Microsoft.WindowsDefenderATP

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | diagnosticSettings | Hayır |
> | diagnosticSettingsCategories | Hayır |

## <a name="microsoftwindowsiot"></a>Microsoft.WindowsIoT

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | DeviceServices | Yes |

## <a name="microsoftworkloadmonitor"></a>Microsoft.WorkloadMonitor

> [!div class="mx-tableFixed"]
> | Kaynak türü | Tam modda silme |
> | ------------- | ----------- |
> | components | Hayır |
> | componentsSummary | Hayır |
> | monitorInstances | Hayır |
> | monitorInstancesSummary | Hayır |
> | monitors | Hayır |
> | notificationSettings | Hayır |

## <a name="next-steps"></a>Sonraki adımlar

To get the same data as a file of comma-separated values, download [complete-mode-deletion.csv](https://github.com/tfitzmac/resource-capabilities/blob/master/complete-mode-deletion.csv).
