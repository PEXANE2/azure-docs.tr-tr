---
title: Uzantı kaynak türleri
description: Azure Kaynak türleri, diğer kaynak türlerinin yeteneklerini genişletmek için kullanılır.
ms.topic: conceptual
ms.date: 09/22/2020
ms.openlocfilehash: 8b80c63d361f3ad8199fd669178f7bf88dabe02e
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "90969738"
---
# <a name="resource-types-that-extend-capabilities-of-other-resources"></a>Diğer kaynakların özelliklerini genişleten kaynak türleri

Uzantı kaynağı, başka bir kaynağın özelliklerine ekleyen bir kaynaktır. Örneğin, kaynak kilidi bir uzantı kaynağıdır. Başka bir kaynağa bir kaynak kilidi uygulayarak bunun silinmesini veya değiştirilmesini önleyebilirsiniz. Kendi başına bir kaynak kilidi oluşturmak mantıklı değildir. Uzantı kaynağı her zaman başka bir kaynağa uygulanır.

## <a name="extension-resource-types"></a>Uzantı kaynak türleri

- Microsoft. Advisor/Configurations
- Microsoft. Advisor/öneriler
- Microsoft. Advisor/suppressions
- Microsoft. AlertsManagement/uyarılar
- Microsoft. AlertsManagement/alertsSummary
- Microsoft. Authorization/Accessbelgeno Scheduledefinitions
- Microsoft. Authorization/Accessbelgeayarlarý Schedulesettings
- Microsoft. Authorization/checkAccess
- Microsoft. Authorization/Denyasatamaları
- Microsoft. Authorization/Findorphanroleatamalar
- Microsoft. Authorization/kilitleri
- Microsoft. Authorization/Permissions
- Microsoft. Authorization/Poliyasatamaları
- Microsoft. Authorization/policyDefinitions
- Microsoft. Authorization/Policymuafiyetleri
- Microsoft. Authorization/policySetDefinitions
- Microsoft. Authorization/privateLinkAssociations
- Microsoft. Authorization/Roleatamalar
- Microsoft. Authorization/Roleatamasussusageölçümleri
- Microsoft. Authorization/roleDefinitions
- Microsoft. oto Yönet/Configurationprofileatamalar
- Microsoft. faturalandırma/Billingdönemler
- Microsoft. faturalandırma/billingPermissions
- Microsoft. faturalandırma/Billingroleatamaları
- Microsoft. faturalandırma/billingRoleDefinitions
- Microsoft. faturalandırma/Createbillingroleatama
- Microsoft. Blueprint/Blueprintasbir
- Microsoft. Blueprint/planlar
- Microsoft. ChangeAnalysis/resourceChanges
- Microsoft. tüketim/Aggregmalyt maliyeti
- Microsoft. tüketim/bakiyeleri
- Microsoft. tüketim/bütçeler
- Microsoft. tüketim/ücretler
- Microsoft. tüketim/CostTags
- Microsoft. tüketim/krediler
- Microsoft. tüketim/olaylar
- Microsoft. tüketim/tahminler
- Microsoft. tüketim/lotlar
- Microsoft. tüketim/pazar yerleri
- Microsoft. tüketim/OperationResults
- Microsoft. tüketim/OperationStatus
- Microsoft. tüketim/Pricesheets
- Microsoft. tüketim/ürünler
- Microsoft. tüketim/Rezervationdetails
- Microsoft. tüketim/ReservationRecommendationDetails
- Microsoft. tüketim/Rezervationönerilere
- Microsoft. tüketim/Rezervationözetler
- Microsoft. tüketim/Rezervationtransactions
- Microsoft. tüketim/Etiketler
- Microsoft. tüketim/kiracılar
- Microsoft. tüketim/koşullar
- Microsoft. tüketim/UsageDetails
- Microsoft. Containerınstance/serviceAssociationLinks
- Microsoft. CostManagement/uyarılar
- Microsoft. CostManagement/bütçeleri
- Microsoft. CostManagement/costAllocationRules
- Microsoft. CostManagement/Dimensions
- Microsoft. CostManagement/dışarı aktarmalar
- Microsoft. CostManagement/Externalabonelikleri
- Microsoft. CostManagement/tahmin
- Microsoft. CostManagement/Insights
- Microsoft. CostManagement/Query
- Microsoft. CostManagement/Reportconfigs
- Microsoft. CostManagement/Reports
- Microsoft. CostManagement/showbackRules
- Microsoft. CostManagement/views
- Microsoft. CustomProviders/ilişkilendirmeleri
- Microsoft. EventGrid/Eventabonelikleri
- Microsoft. EventGrid/Extensionkonuları
- Microsoft. GuestConfiguration/Configurationprofileatamalar
- Microsoft. GuestConfiguration/Guestconfigurationatamaları
- Microsoft. GuestConfiguration/yazılım
- Microsoft. GuestConfiguration/softwareUpdateProfile
- Microsoft. GuestConfiguration/softwareUpdates
- Microsoft. Insights/Baseline
- Microsoft. Insights/calculatebaseline
- Microsoft. Insights/Datacollectionruleassociation
- Microsoft. Insights/diagnosticSettings
- Microsoft. Insights/diagnosticSettingsCategories
- Microsoft. Insights/eventTypes
- Microsoft. Insights/extendedDiagnosticSettings
- Microsoft. Insights/generateLiveToken
- Microsoft. Insights/guestDiagnosticSettingsAssociation
- Microsoft. Insights/logDefinitions
- Microsoft. Insights/Günlükler
- Microsoft. Insights/metrictemeller
- Microsoft. Insights/metricDefinitions
- Microsoft. Insights/metricNamespaces
- Microsoft. Insights/ölçümler
- Microsoft. Insights/Mykitaplarında
- Microsoft. Insights/topoloji
- Microsoft. Insights/işlemler
- Microsoft. Insights/vmInsightsOnboardingStatuses
- Microsoft. KubernetesConfiguration/uzantıları
- Microsoft. KubernetesConfiguration/sourceControlConfigurations
- Microsoft. Maintenance/applyUpdates
- Microsoft. Maintenance/Configurationatamalar
- Microsoft. Maintenance/Updates
- Microsoft. Managedıdentity/kimlikler
- Microsoft. ManagedServices/Registrationatamaları
- Microsoft. ManagedServices/registrationDefinitions
- Microsoft. Operationalınsights/Storageınsiizconfigs
- Microsoft. OperationsManagement/managementassociations
- Microsoft. Poliyeleghts/belirlediğimizi karşıladığımızı
- Microsoft. Poliyeleghts/Poliyevents
- Microsoft. Poliyeleghts/policyStates
- Microsoft. Poliyeleghts/policyTrackedResources
- Microsoft. Poliyeleghts/düzeltmelere
- Microsoft. RecoveryServices/Backupkoruyucuteditems
- Microsoft. RecoveryServices/replicationEligibilityResults
- Microsoft. ResourceHealth/kullanılabilirliği Bilitydurumlar
- Microsoft. ResourceHealth/Childadvailabilitydurumlar
- Microsoft. ResourceHealth/childResources
- Microsoft. ResourceHealth/olayları
- Microsoft. ResourceHealth/ımpactedresources
- Microsoft. ResourceHealth/bildirimleri
- Microsoft. resources/Links
- Microsoft. resources/etiketleri
- Microsoft. Security/adaptiveNetworkHardenings
- Microsoft. Security/advancedThreatProtectionSettings
- Microsoft. Security/assessmentMetadata
- Microsoft. Security/değerlendirmeleri
- Microsoft. Security/Karmaşıkanceresults
- Microsoft. Security/Zorluslar
- Microsoft. Security/dataCollectionAgents
- Microsoft. Security/deviceSecurityGroups
- Microsoft. Security/ınformationprotectionpolicies
- Microsoft. Security/ıotalgılayıcılar
- Microsoft. Security/Jpolicies
- Microsoft. Security/serversecurity, açıklıkları
- Microsoft. Security/Sqlgüvenlik açılar,
- Microsoft. Securityınsights/toplamalar
- Microsoft. Securityınsights/alertRules
- Microsoft. Securityınsights/Alertrutatemplates
- Microsoft. Securityınsights/automationRules
- Microsoft. Securityınsights/yer işaretleri
- Microsoft. Securityınsights/durumlar
- Microsoft. Securityınsights/veri bağlayıcıları
- Microsoft. Securityınsights/dataConnectorsCheckRequirements
- Microsoft. Securityınsights/varlıklar
- Microsoft. Securityınsights/entityQueries
- Microsoft. Securityınsights/olaylar
- Microsoft. Securityınsights/officeConsents
- Microsoft. Securityınsights/ayarlar
- Microsoft. Securityınsights/Threatıntelligence
- Microsoft. Securityınsights/Watchlists
- Microsoft. SoftwarePlan/Hybriduseavantajlar
- Microsoft. Subscription/CreateSubscription
- Microsoft. support/supportbiletleri
- Microsoft. WorkloadMonitor/bileşenleri
- Microsoft. WorkloadMonitor/Monitorınstances
- Microsoft. WorkloadMonitor/izleyicileri
- Microsoft. WorkloadMonitor/notificationSettings

## <a name="next-steps"></a>Sonraki adımlar

- Bir Azure Resource Manager şablonundaki uzantı kaynağı için kaynak KIMLIĞINI almak üzere [Extensionresourceıd](../templates/template-functions-resource.md#extensionresourceid)kullanın.
- Bir şablonda uzantı kaynağı oluşturma örneği için [Event Grid olay abonelikleri](/azure/templates/microsoft.eventgrid/2019-06-01/eventsubscriptions)' ne bakın.
