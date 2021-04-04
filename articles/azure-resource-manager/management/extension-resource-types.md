---
title: Uzantı kaynak türleri
description: Azure Kaynak türleri, diğer kaynak türlerinin yeteneklerini genişletmek için kullanılır.
ms.topic: conceptual
ms.date: 11/14/2020
ms.openlocfilehash: 5561c480dd5a2849588ed2288eb5bcc35fc1446c
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2021
ms.locfileid: "94658460"
---
# <a name="resource-types-that-extend-capabilities-of-other-resources"></a>Diğer kaynakların özelliklerini genişleten kaynak türleri

Uzantı kaynağı, başka bir kaynağın özelliklerine ekleyen bir kaynaktır. Örneğin, kaynak kilidi bir uzantı kaynağıdır. Başka bir kaynağa bir kaynak kilidi uygulayarak bunun silinmesini veya değiştirilmesini önleyebilirsiniz. Kendi başına bir kaynak kilidi oluşturmak mantıklı değildir. Uzantı kaynağı her zaman başka bir kaynağa uygulanır.

## <a name="microsoftadvisor"></a>Microsoft. Advisor

- Microsoft. Advisor/Configurations
- Microsoft. Advisor/öneriler
- Microsoft. Advisor/suppressions

## <a name="microsoftalertsmanagement"></a>Microsoft. AlertsManagement

- Microsoft. AlertsManagement/uyarılar

## <a name="microsoftauthorization"></a>Microsoft.Authorization

- Microsoft. Authorization/Denyasatamaları
- Microsoft. Authorization/kilitleri
- Microsoft. Authorization/Poliyasatamaları
- Microsoft. Authorization/policyDefinitions
- Microsoft. Authorization/Policymuafiyetleri
- Microsoft. Authorization/policySetDefinitions
- Microsoft. Authorization/privateLinkAssociations
- Microsoft. Authorization/Roleatamalar
- Microsoft. Authorization/roleDefinitions

## <a name="microsoftautomanage"></a>Microsoft. oto Yönet

- Microsoft. oto Yönet/Configurationprofileatamalar

## <a name="microsoftbilling"></a>Microsoft. Faturalandırma

- Microsoft. faturalandırma/Billingdönemler
- Microsoft. faturalandırma/billingPermissions
- Microsoft. faturalandırma/Billingroleatamaları
- Microsoft. faturalandırma/billingRoleDefinitions
- Microsoft. faturalandırma/Createbillingroleatama

## <a name="microsoftblueprint"></a>Microsoft. Blueprint

- Microsoft. Blueprint/Blueprintasbir
- Microsoft. Blueprint/planlar

## <a name="microsoftconsumption"></a>Microsoft. tüketim

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
- Microsoft. tüketim/Pricesheets
- Microsoft. tüketim/ürünler
- Microsoft. tüketim/Rezervationdetails
- Microsoft. tüketim/ReservationRecommendationDetails
- Microsoft. tüketim/Rezervationönerilere
- Microsoft. tüketim/Rezervationözetler
- Microsoft. tüketim/Rezervationtransactions

## <a name="microsoftcontainerinstance"></a>Microsoft. Containerınstance

- Microsoft. Containerınstance/serviceAssociationLinks

## <a name="microsoftcostmanagement"></a>Microsoft. CostManagement

- Microsoft. CostManagement/uyarılar
- Microsoft. CostManagement/bütçeleri
- Microsoft. CostManagement/Dimensions
- Microsoft. CostManagement/dışarı aktarmalar
- Microsoft. CostManagement/Externalabonelikleri
- Microsoft. CostManagement/tahmin
- Microsoft. CostManagement/Insights
- Microsoft. CostManagement/Query
- Microsoft. CostManagement/Reportconfigs
- Microsoft. CostManagement/Reports
- Microsoft. CostManagement/views

## <a name="microsoftcustomproviders"></a>Microsoft. CustomProviders

- Microsoft. CustomProviders/ilişkilendirmeleri

## <a name="microsofteventgrid"></a>Microsoft. EventGrid

- Microsoft. EventGrid/Eventabonelikleri
- Microsoft. EventGrid/Extensionkonuları

## <a name="microsoftguestconfiguration"></a>Microsoft. GuestConfiguration

- Microsoft. GuestConfiguration/Configurationprofileatamalar
- Microsoft. GuestConfiguration/Guestconfigurationatamaları
- Microsoft. GuestConfiguration/yazılım

## <a name="microsoftinsights"></a>Microsoft. Insights

- Microsoft. Insights/Baseline
- Microsoft. Insights/Datacollectionruleassociation
- Microsoft. Insights/diagnosticSettings
- Microsoft. Insights/diagnosticSettingsCategories
- Microsoft. Insights/eventTypes
- Microsoft. Insights/extendedDiagnosticSettings
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

## <a name="microsoftkubernetesconfiguration"></a>Microsoft. KubernetesConfiguration

- Microsoft. KubernetesConfiguration/uzantıları
- Microsoft. KubernetesConfiguration/sourceControlConfigurations

## <a name="microsoftmaintenance"></a>Microsoft. Maintenance

- Microsoft. Maintenance/applyUpdates
- Microsoft. Maintenance/Configurationatamalar
- Microsoft. Maintenance/Updates

## <a name="microsoftmanagedidentity"></a>Microsoft. Managedıdentity

- Microsoft. Managedıdentity/kimlikler

## <a name="microsoftmanagedservices"></a>Microsoft. ManagedServices

- Microsoft. ManagedServices/Registrationatamaları
- Microsoft. ManagedServices/registrationDefinitions

## <a name="microsoftoperationalinsights"></a>Microsoft. Operationalınsights

- Microsoft. Operationalınsights/Storageınsiizconfigs

## <a name="microsoftoperationsmanagement"></a>Microsoft. OperationsManagement

- Microsoft. OperationsManagement/managementassociations

## <a name="microsoftpolicyinsights"></a>Microsoft. Poliyelei

- Microsoft. Poliyeleghts/belirlediğimizi karşıladığımızı
- Microsoft. Poliyeleghts/Poliyevents
- Microsoft. Poliyeleghts/policyStates
- Microsoft. Poliyeleghts/policyTrackedResources
- Microsoft. Poliyeleghts/düzeltmelere

## <a name="microsoftrecoveryservices"></a>Microsoft. RecoveryServices

- Microsoft. RecoveryServices/Backupkoruyucuteditems
- Microsoft. RecoveryServices/replicationEligibilityResults

## <a name="microsoftresourcehealth"></a>Microsoft. ResourceHealth

- Microsoft. ResourceHealth/childResources
- Microsoft. ResourceHealth/olayları
- Microsoft. ResourceHealth/ımpactedresources
- Microsoft. ResourceHealth/bildirimleri

## <a name="microsoftresources"></a>Microsoft. resources

- Microsoft. resources/Links
- Microsoft. resources/etiketleri

## <a name="microsoftsecurity"></a>Microsoft.Security

- Microsoft. Security/adaptiveNetworkHardenings
- Microsoft. Security/advancedThreatProtectionSettings
- Microsoft. Security/assessmentMetadata
- Microsoft. Security/değerlendirmeleri
- Microsoft. Security/Zorluslar
- Microsoft. Security/dataCollectionAgents
- Microsoft. Security/Devices
- Microsoft. Security/deviceSecurityGroups
- Microsoft. Security/ınformationprotectionpolicies
- Microsoft. Security/ıotalgılayıcılar
- Microsoft. Security/Jpolicies
- Microsoft. Security/serversecurity, açıklıkları
- Microsoft. Security/Sqlgüvenlik açılar,

## <a name="microsoftsecurityinsights"></a>Microsoft. Securityınsights

- Microsoft. Securityınsights/toplamalar
- Microsoft. Securityınsights/alertRules
- Microsoft. Securityınsights/Alertrutatemplates
- Microsoft. Securityınsights/automationRules
- Microsoft. Securityınsights/yer işaretleri
- Microsoft. Securityınsights/durumlar
- Microsoft. Securityınsights/veri bağlayıcıları
- Microsoft. Securityınsights/dataConnectorsCheckRequirements
- Microsoft. Securityınsights/varlıklar
- Microsoft. Securityınsights/olaylar
- Microsoft. Securityınsights/ayarlar
- Microsoft. Securityınsights/Threatıntelligence
- Microsoft. Securityınsights/Watchlists

## <a name="microsoftserialconsoleppe"></a>Microsoft. SerialConsole. PPE

- Microsoft. SerialConsole. PPE/seri bağlantı noktaları

## <a name="microsoftsoftwareplan"></a>Microsoft. SoftwarePlan

- Microsoft. SoftwarePlan/Hybriduseavantajlar

## <a name="microsoftsupport"></a>Microsoft. support

- Microsoft. support/supportbiletleri

## <a name="microsoftworkloadmonitor"></a>Microsoft. WorkloadMonitor

- Microsoft. WorkloadMonitor/bileşenleri
- Microsoft. WorkloadMonitor/Monitorınstances
- Microsoft. WorkloadMonitor/izleyicileri
- Microsoft. WorkloadMonitor/notificationSettings

## <a name="next-steps"></a>Sonraki adımlar

- Bir Azure Resource Manager şablonundaki uzantı kaynağı için kaynak KIMLIĞINI almak üzere [Extensionresourceıd](../templates/template-functions-resource.md#extensionresourceid)kullanın.
- Bir şablonda uzantı kaynağı oluşturma örneği için [Event Grid olay abonelikleri](/azure/templates/microsoft.eventgrid/2019-06-01/eventsubscriptions)' ne bakın.
