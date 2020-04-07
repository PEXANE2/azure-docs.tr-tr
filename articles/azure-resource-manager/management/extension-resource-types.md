---
title: Uzantı kaynak türleri
description: Azure kaynak türlerinin listeleri, diğer kaynak türlerinin yeteneklerini genişletmek için kullanılır.
ms.topic: conceptual
ms.date: 04/06/2020
ms.openlocfilehash: 3a3fbc531750bec4b16e38f1fe79f613c1b94f5e
ms.sourcegitcommit: 441db70765ff9042db87c60f4aa3c51df2afae2d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80754862"
---
# <a name="resource-types-that-extend-capabilities-of-other-resources"></a>Diğer kaynakların yeteneklerini genişleten kaynak türleri

Uzantı kaynağı, başka bir kaynağın yeteneklerine ekleyen bir kaynaktır. Örneğin, kaynak kilidi bir uzantı kaynağıdır. Bir kaynak kilidi silinmesini veya değiştirilmesini önlemek için başka bir kaynağa uygularsınız. Kendi başına bir kaynak kilidi oluşturmak mantıklı değildir. Uzantılı kaynak her zaman başka bir kaynağa uygulanır.

## <a name="extension-resource-types"></a>Uzantı kaynak türleri

- Microsoft.Advisor/yapılandırmaları
- Microsoft.Advisor/öneriler
- Microsoft.Advisor/suppressions
- Microsoft.AlertsManagement/alerts
- Microsoft.AlertsManagement/alertsSummary
- Microsoft.Authorization/checkAccess
- Microsoft.Authorization/denyAssignments
- Microsoft.Authorization/findOrphanRoleAssignments
- Microsoft.Authorization/kilitler
- Microsoft.Authorization/permissions
- Microsoft.Authorization/policyAtamalar
- Microsoft.Authorization/policyDefinitions
- Microsoft.Authorization/policySetDefinitions
- Microsoft.Authorization/roleAssignments
- Microsoft.Authorization/roleAssignmentsKullanımMetrics
- Microsoft.Authorization/roleDefinitions
- Microsoft.Faturalama/faturalama Dönemleri
- Microsoft.Billing/billingİzinler
- Microsoft.Billing/billingRoleAssignments
- Microsoft.Billing/billingRoleDefinitions
- Microsoft.Billing/createBillingRoleAssignment
- Microsoft.Blueprint/blueprintAtamaları
- Microsoft.Blueprint/planları
- Microsoft.Consumption/AggregatedCost
- Microsoft.Consumption/Bakiyeler
- Microsoft.Consumption/Bütçeler
- Microsoft.Consumption/Ücretler
- Microsoft.Consumption/CostTags
- Microsoft.Consumption/Forecasts
- Microsoft.Consumption/Marketplaces
- Microsoft.Consumption/OperationResults
- Microsoft.Consumption/OperationStatus
- Microsoft.Consumption/Pricesheets
- Microsoft.Consumption/ReservationDetails
- Microsoft.Consumption/ReservationRecommendations
- Microsoft.Consumption/ReservationSummaries
- Microsoft.Consumption/ReservationTransactions
- Microsoft.Consumption/Etiketler
- Microsoft.Consumption/Terms
- Microsoft.Consumption/UsageDetails
- Microsoft.Consumption/credits
- Microsoft.Consumption/events
- Microsoft.Consumption/lots
- Microsoft.Consumption/ürünler
- Microsoft.Consumption/kiracı
- Microsoft.ContainerInstance/serviceAssociationLinks
- Microsoft.CostManagement/Alerts
- Microsoft.CostManagement/Bütçeler
- Microsoft.CostManagement/Boyutlar
- Microsoft.CostManagement/Dışa Aktarma
- Microsoft.CostManagement/externalSubscriptions
- Microsoft.CostManagement/Tahmin
- Microsoft.CostManagement/Sorgu
- Microsoft.CostManagement/Reportconfigs
- Microsoft.CostManagement/Raporlar
- Microsoft.CostManagement/Görünümler
- Microsoft.CostManagement/showbackRules
- Microsoft.CustomProviders/associations
- Microsoft.EventGrid/eventAbonelikler
- Microsoft.EventGrid/extensionTopics
- Microsoft.GuestConfiguration/configurationProfileAssignments
- Microsoft.GuestConfiguration/guestConfigurationAssignments
- Microsoft.GuestConfiguration/yazılım
- Microsoft.GuestConfiguration/softwareUpdateProfile
- Microsoft.GuestConfiguration/softwareUpdates
- microsoft.insights/baseline
- microsoft.insights/calculatebaseline
- microsoft.insights/dataCollectionRuleAssociations
- microsoft.insights/diagnosticSettings
- microsoft.insights/diagnosticSettingsCategories
- microsoft.insights/eventtypes
- microsoft.insights/extendedDiagnosticSettings
- microsoft.insights/guestDiagnosticSettingsAssociation
- microsoft.insights/logDefinitions
- microsoft.insights/günlükleri
- microsoft.insights/metricDefinitions
- microsoft.insights/metricNamespaces
- microsoft.insights/metricbaselines
- microsoft.insights/metrics
- microsoft.insights/myWorkbooks
- microsoft.insights/topology
- microsoft.insights/transactions
- microsoft.insights/vmInsightsOnboardingStatuses
- Microsoft.KubernetesConfiguration/sourceControlConfigurations
- Microsoft.Maintenance/applyUpdates
- Microsoft.Maintenance/configurationAtamaları
- Microsoft.Maintenance/updates
- Microsoft.ManagedIdentity/Identity
- Microsoft.ManagedServices/registrationAtamaları
- Microsoft.ManagedServices/registrationDefinitions
- Microsoft.OperationalInsights/storageInsightConfigs
- Microsoft.OperationsManagement/managementassociations
- Microsoft.PolicyInsights/policyEvents
- Microsoft.PolicyInsights/policyStates
- Microsoft.PolicyInsights/policyTrackedResources
- Microsoft.PolicyInsights/düzeltmeler
- Microsoft.RecoveryServices/backupProtectedItems
- Microsoft.RecoveryServices/replicationEligibilityResults    
- Microsoft.ResourceHealth/availabilityStatuses
- Microsoft.ResourceHealth/childAvailabilityStatuses
- Microsoft.ResourceHealth/childResources
- Microsoft.ResourceHealth/events
- Microsoft.ResourceHealth/impactedResources
- Microsoft.ResourceHealth/bildirimler
- Microsoft.Resources/links
- Microsoft.Resources/tags
- Microsoft.Security/Compliances
- Microsoft.Security/InformationProtectionPolicies
- Microsoft.Security/adaptiveNetworkHardenings
- Microsoft.Security/advancedThreatProtectionSettings
- Microsoft.Security/assessmentMetadata
- Microsoft.Security/değerlendirmeler
- Microsoft.Security/complianceResults
- Microsoft.Security/dataCollectionAgents
- Microsoft.Security/deviceSecurityGroups
- Microsoft.Security/networkData
- Microsoft.Security/serverVulnerabilityAssessments
- Microsoft.SecurityInsights/toplamalar
- Microsoft.SecurityInsights/alertRuleTemplates
- Microsoft.SecurityInsights/alertRules
- Microsoft.SecurityInsights/yer imleri
- Microsoft.SecurityInsights/servis talepleri
- Microsoft.SecurityInsights/dataConnectors
- Microsoft.SecurityInsights/dataConnectorsCheckRequirements
- Microsoft.SecurityInsights/tvarlıklar
- Microsoft.SecurityInsights/entityQueries
- Microsoft.SecurityInsights/olaylar
- Microsoft.SecurityInsights/officeConsents
- Microsoft.SecurityInsights/settings
- Microsoft.SoftwarePlan/hybridUseBenefits
- Microsoft.Subscription/CreateSubscription
- microsoft.support/supporttickets
- Microsoft.WorkloadMonitor/bileşenleri
- Microsoft.WorkloadMonitor/monitorInstances
- Microsoft.WorkloadMonitor/monitörler
- Microsoft.WorkloadMonitor/notificationAyarlar

## <a name="next-steps"></a>Sonraki adımlar

- Bir Azure Kaynak Yöneticisi şablonundaki bir uzantı kaynağının kaynak kimliğini almak için [uzantıResourceId'i](../templates/template-functions-resource.md#extensionresourceid)kullanın.
- Şablonda uzantı kaynağı oluşturma örneği [için](/azure/templates/microsoft.eventgrid/2019-06-01/eventsubscriptions)bkz.
