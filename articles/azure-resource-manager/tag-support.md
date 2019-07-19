---
title: Kaynaklar için Azure Resource Manager etiketi desteği
description: Hangi Azure kaynak türlerinin etiketleri desteklediğini gösterir. Tüm Azure hizmetleri için ayrıntılar sağlar.
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: reference
ms.date: 06/07/2019
ms.author: tomfitz
ms.openlocfilehash: 72bb11cd064c90c2bbe1e9e6452dcbf07fe37817
ms.sourcegitcommit: 770b060438122f090ab90d81e3ff2f023455213b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/17/2019
ms.locfileid: "68304861"
---
# <a name="tag-support-for-azure-resources"></a>Azure kaynakları için etiket desteği
Bu makalede, bir kaynak türünün [etiketleri](resource-group-using-tags.md)destekleyip desteklemediğini açıklanmaktadır. Etiketi **destekleyen** sütun, kaynak türünün etiket için bir özelliğe sahip olup olmadığını gösterir. **Maliyet raporundaki etiket** etiketli sütun, kaynak türünün etiketi maliyet raporuna geçirip geçirmediğini belirtir.

Aynı verileri bir virgülle ayrılmış değerler dosyası ile almak için, [tag-support. csv](https://github.com/tfitzmac/resource-capabilities/blob/master/tag-support.csv)dosyasını indirin.

## <a name="microsoftaad"></a>Microsoft.AAD
| Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
| ------------- | ----------- | ----------- |
| DomainServices | Evet | Evet |
| DomainServices/oucontainer | Hayır | Hayır |

## <a name="microsoftaadiam"></a>microsoft.aadiam
| Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
| ------------- | ----------- | ----------- |
| diagnosticSettings | Hayır |  Hayır |
| diagnosticSettingsCategories | Hayır |  Hayır |

## <a name="microsoftaddons"></a>Microsoft. addons
| Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
| ------------- | ----------- | ----------- |
| Destek sağlayıcıları | Hayır |  Hayır |

## <a name="microsoftadhybridhealthservice"></a>Microsoft.ADHybridHealthService
| Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
| ------------- | ----------- | ----------- |
| aadsupportcases | Hayır |  Hayır |
| addsservices | Hayır |  Hayır |
| Aracısını | Hayır |  Hayır |
| anonymousapiusers | Hayır |  Hayır |
| yapılandırma | Hayır |  Hayır |
| logs | Hayır |  Hayır |
| raporlar | Hayır |  Hayır |
| services | Hayır |  Hayır |

## <a name="microsoftadvisor"></a>Microsoft.Advisor
| Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
| ------------- | ----------- | ----------- |
| konfigürasyonları | Hayır |  Hayır |
| Generatereyorumgeçişleri | Hayır |  Hayır |
| Önerileri | Hayır |  Hayır |
| gizlemeleri | Hayır |  Hayır |

## <a name="microsoftalertsmanagement"></a>Microsoft.AlertsManagement
| Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
| ------------- | ----------- | ----------- |
| actionRules | Hayır |  Hayır |
| alerts | Hayır |  Hayır |
| alertsList | Hayır |  Hayır |
| alertsSummary | Hayır |  Hayır |
| alertsSummaryList | Hayır |  Hayır |
| smartDetectorAlertRules | Hayır |  Hayır |
| Smartdetectorruntimeortamortamları | Hayır |  Hayır |
| smartGroups | Hayır |  Hayır |

## <a name="microsoftanalysisservices"></a>Microsoft.AnalysisServices
| Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
| ------------- | ----------- | ----------- |
| Larý | Evet | Evet |

## <a name="microsoftapimanagement"></a>Microsoft.ApiManagement
| Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
| ------------- | ----------- | ----------- |
| reportFeedback | Hayır |  Hayır |
| hizmet | Evet | Evet |
| validateServiceName | Hayır |  Hayır |

## <a name="microsoftattestation"></a>Microsoft. kanıtlama
| Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
| ------------- | ----------- | ----------- |
| attestationProviders | Hayır |  Hayır |

## <a name="microsoftauthorization"></a>Microsoft. Authorization
| Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
| ------------- | ----------- | ----------- |
| classicAdministrators | Hayır |  Hayır |
| Denyasatamaları | Hayır |  Hayır |
| Erişimi yükseltme | Hayır |  Hayır |
| kaynaktaki | Hayır |  Hayır |
| izinler | Hayır |  Hayır |
| Poliyasatamaları | Hayır |  Hayır |
| policyDefinitions | Hayır |  Hayır |
| policySetDefinitions | Hayır |  Hayır |
| providerOperations | Hayır |  Hayır |
| roleAssignments | Hayır |  Hayır |
| roleDefinitions | Hayır |  Hayır |

## <a name="microsoftautomation"></a>Microsoft. Automation
| Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
| ------------- | ----------- | ----------- |
| automationAccounts | Evet | Evet |
| automationAccounts/Configurations | Evet | Evet |
| automationAccounts/Jobs | Hayır |  Hayır |
| automationAccounts/runbook 'lar | Evet | Evet |
| automationAccounts/softwareUpdateConfigurations | Hayır | Hayır |
| automationAccounts/Web kancaları | Hayır |  Hayır |

## <a name="microsoftazuregeneva"></a>Microsoft. Azure. Genfiliz
| Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
| ------------- | ----------- | ----------- |
| lý | Hayır |  Hayır |
| ortamlar/hesaplar | Hayır |  Hayır |
| ortamlar/hesaplar/ad alanları | Hayır |  Hayır |
| ortamlar/hesaplar/ad alanları/yapılandırma | Hayır |  Hayır |

## <a name="microsoftazureactivedirectory"></a>Microsoft.AzureActiveDirectory
| Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
| ------------- | ----------- | ----------- |
| b2cDirectories | Evet | Hayır |

## <a name="microsoftazurestack"></a>Microsoft.AzureStack
| Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
| ------------- | ----------- | ----------- |
| kayıtlarında | Evet | Evet |
| kayıt/müşteri abonelikleri | Hayır |  Hayır |
| kayıtlar/ürünler | Hayır |  Hayır |

## <a name="microsoftbatch"></a>Microsoft.Batch
| Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
| ------------- | ----------- | ----------- |
| batchAccounts | Evet | Evet |

## <a name="microsoftbilling"></a>Microsoft.Billing
| Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
| ------------- | ----------- | ----------- |
| billingAccounts | Hayır |  Hayır |
| billingAccounts/billingProfiles | Hayır |  Hayır |
| billingAccounts/billingProfiles/Billingabonelikleri | Hayır |  Hayır |
| billingAccounts/billingProfiles/faturalar | Hayır |  Hayır |
| billingAccounts/billingProfiles/faturalar/fiyat listesi | Hayır |  Hayır |
| billingAccounts/billingProfiles/operationStatus | Hayır |  Hayır |
| billingAccounts/billingProfiles/paymentMethods | Hayır |  Hayır |
| billingAccounts/billingProfiles/ilkeler | Hayır |  Hayır |
| billingAccounts/billingProfiles/fiyat listesi | Hayır |  Hayır |
| billingAccounts/billingProfiles/ürünler | Hayır |  Hayır |
| billingAccounts/billingProfiles/işlemler | Hayır |  Hayır |
| billingAccounts/Billingabonelikleri | Hayır |  Hayır |
| billingAccounts/departmanlar | Hayır |  Hayır |
| billingAccounts/Eligibletekliflere | Hayır |  Hayır |
| billingAccounts/KayıtSayısı | Hayır |  Hayır |
| billingAccounts/faturalar | Hayır |  Hayır |
| billingAccounts/ınvoicesections | Hayır |  Hayır |
| billingAccounts/ınvoicesections/Billingabonelikleri | Hayır |  Hayır |
| billingAccounts/ınvoicesections/Billingabonelikleri/aktarımı | Hayır |  Hayır |
| billingAccounts/ınvoicesections/ımportrequests | Hayır |  Hayır |
| billingAccounts/ınvoicesections/ınitiateımportrequest | Hayır |  Hayır |
| billingAccounts/ınvoicesections/ınitiatetransfer | Hayır |  Hayır |
| billingAccounts/ınvoicesections/operationStatus | Hayır |  Hayır |
| billingAccounts/Ürünler/Ürünler | Hayır |  Hayır |
| billingAccounts/ınvoicesections/aktarımlar | Hayır |  Hayır |
| billingAccounts/ürünler | Hayır |  Hayır |
| billingAccounts/projeler | Hayır |  Hayır |
| billingAccounts/projeler/Billingabonelikleri | Hayır |  Hayır |
| billingAccounts/projeler/ımportrequests | Hayır |  Hayır |
| billingAccounts/projeler/ınitiateımportrequest | Hayır |  Hayır |
| billingAccounts/projeler/operationStatus | Hayır |  Hayır |
| billingAccounts/projeler/ürünler | Hayır |  Hayır |
| billingAccounts/işlemler | Hayır |  Hayır |
| Billingdönemler | Hayır |  Hayır |
| BillingPermissions | Hayır |  Hayır |
| billingProperty | Hayır |  Hayır |
| Billingroleatamaları | Hayır |  Hayır |
| BillingRoleDefinitions | Hayır |  Hayır |
| Createbillingroleatama | Hayır |  Hayır |
| bölümlerinin | Hayır |  Hayır |
| kayıt sayısı | Hayır |  Hayır |
| ımportrequests | Hayır |  Hayır |
| ımportrequests/Acceptımportrequest | Hayır |  Hayır |
| ımportrequests/Declineımportrequest | Hayır |  Hayır |
| faturalardan | Hayır |  Hayır |
| girişinde | Hayır |  Hayır |
| aktarımlar/acceptTransfer | Hayır |  Hayır |
| aktarımlar/declineTransfer | Hayır |  Hayır |
| aktarımlar/operationStatus | Hayır |  Hayır |
| Usageplanlar | Hayır |  Hayır |

## <a name="microsoftbingmaps"></a>Microsoft.BingMaps
| Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
| ------------- | ----------- | ----------- |
| Mapapsıs | Evet | Evet |
| updateCommunicationPreference | Hayır |  Hayır |

## <a name="microsoftbiztalkservices"></a>Microsoft. BizTalkServices
| Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
| ------------- | ----------- | ----------- |
| BizTalk | Evet | Evet |

## <a name="microsoftblueprint"></a>Microsoft. Blueprint
| Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
| ------------- | ----------- | ----------- |
| Şema tasmi | Hayır |  Hayır |
| Blueprintasbir/Atamaperations | Hayır |  Hayır |
| Blueprintasbir/işlemleri | Hayır |  Hayır |
| Blueprint | Hayır |  Hayır |
| planlar/yapıtlar | Hayır |  Hayır |
| planlar/sürümler | Hayır |  Hayır |
| planlar/sürümler/yapılar | Hayır |  Hayır |

## <a name="microsoftbotservice"></a>Microsoft. BotService
| Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
| ------------- | ----------- | ----------- |
| botServices | Evet | Evet |
| botServices/kanallar | Hayır |  Hayır |
| botServices/Connections | Hayır |  Hayır |

## <a name="microsoftcache"></a>Microsoft.Cache
| Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
| ------------- | ----------- | ----------- |
| Redis | Evet | Evet |
| RedisConfigDefinition | Hayır |  Hayır |

## <a name="microsoftcapacity"></a>Microsoft. Capacity
| Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
| ------------- | ----------- | ----------- |
| appliedReservations | Hayır |  Hayır |
| calculatePrice | Hayır |  Hayır |
| larına | Hayır |  Hayır |
| Ticari Vaalrezervler | Hayır |  Hayır |
| Rezervler | Hayır |  Hayır |
| Rezervler/Hesaplaizterefund | Hayır |  Hayır |
| Rezervler/Birleştir | Hayır |  Hayır |
| Rezervler/rezervasyonlar | Hayır |  Hayır |
| Rezervler/rezervasyonlar/düzeltmeler | Hayır |  Hayır |
| Rezervler/geri dönüş | Hayır |  Hayır |
| Rezervler/Böl | Hayır |  Hayır |
| Rezervler/takas | Hayır |  Hayır |
| oluşturamaz | Hayır |  Hayır |
| Kaynakların | Hayır |  Hayır |
| validateReservationOrder | Hayır |  Hayır |

## <a name="microsoftcdn"></a>Microsoft.Cdn
| Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
| ------------- | ----------- | ----------- |
| edgenodes | Hayır |  Hayır |
| profiles | Evet | Evet |
| Profiller/uç noktalar | Evet | Evet |
| Profiller/uç noktalar/customdomains | Hayır |  Hayır |
| Profiller/uç noktalar/kaynaklar | Hayır |  Hayır |
| Validatearaştırması | Hayır |  Hayır |

## <a name="microsoftcertificateregistration"></a>Microsoft.CertificateRegistration
| Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
| ------------- | ----------- | ----------- |
| Sertifikadüzenleri | Evet | Evet |
| certificateOrders/Certificates | Hayır |  Hayır |
| Validatecertificateregistrationınformation | Hayır |  Hayır |

## <a name="microsoftclassiccompute"></a>Microsoft.ClassicCompute
| Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
| ------------- | ----------- | ----------- |
| Yetenek | Hayır |  Hayır |
| domainNames | Hayır |  Hayır |
| domainNames/yetenekleri | Hayır |  Hayır |
| domainNames/internalLoadBalancers | Hayır |  Hayır |
| domainNames/serviceCertificates | Hayır |  Hayır |
| domainNames/Yuvaları | Hayır |  Hayır |
| domainNames/yuvalar/roller | Hayır |  Hayır |
| moveSubscriptionResources | Hayır |  Hayır |
| operatingSystemFamilies | Hayır |  Hayır |
| operatingSystems | Hayır |  Hayır |
| quotas | Hayır |  Hayır |
| resourceTypes | Hayır |  Hayır |
| Validatesubscriptionmoveavaılabılıty | Hayır |  Hayır |
| VirtualMachines | Hayır |  Hayır |
| virtualMachines/diagnosticSettings | Hayır |  Hayır |

## <a name="microsoftclassicinfrastructuremigrate"></a>Microsoft. ClassicInfrastructureMigrate
| Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
| ------------- | ----------- | ----------- |
| classicInfrastructureResources | Hayır |  Hayır |

## <a name="microsoftclassicnetwork"></a>Microsoft.ClassicNetwork
| Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
| ------------- | ----------- | ----------- |
| Yetenek | Hayır |  Hayır |
| expressRouteCrossConnections | Hayır |  Hayır |
| expressRouteCrossConnections/peerler | Hayır |  Hayır |
| gatewaySupportedDevices | Hayır |  Hayır |
| networkSecurityGroups | Hayır |  Hayır |
| quotas | Hayır |  Hayır |
| Rezervler | Hayır |  Hayır |
| virtualNetworks | Hayır |  Hayır |
| virtualNetworks/Remotevirtualnetworkpeeringproxy 'Leri | Hayır |  Hayır |
| virtualNetworks/Virtualnetworkpeerler | Hayır |  Hayır |

## <a name="microsoftclassicstorage"></a>Microsoft.ClassicStorage
| Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
| ------------- | ----------- | ----------- |
| Yetenek | Hayır |  Hayır |
| Disklerinden | Hayır |  Hayır |
| images | Hayır |  Hayır |
| osImages | Hayır |  Hayır |
| Osplatformımages | Hayır |  Hayır |
| Publicımages | Hayır |  Hayır |
| quotas | Hayır |  Hayır |
| storageAccounts | Hayır |  Hayır |
| storageAccounts/Services | Hayır |  Hayır |
| storageAccounts/Services/diagnosticSettings | Hayır |  Hayır |
| storageAccounts/Vmımages | Hayır |  Hayır |
| Vmımages | Hayır |  Hayır |

## <a name="microsoftcognitiveservices"></a>Microsoft.CognitiveServices
| Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
| ------------- | ----------- | ----------- |
| accounts | Evet | Evet |

## <a name="microsoftcommerce"></a>Microsoft. Commerce
| Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
| ------------- | ----------- | ----------- |
| RateCard | Hayır |  Hayır |
| Usagetoplamaları | Hayır |  Hayır |

## <a name="microsoftcompute"></a>Microsoft.Compute
| Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
| ------------- | ----------- | ----------- |
| availabilitySets | Evet | Evet |
| Disklerinden | Evet | Evet |
| images | Evet | Evet |
| restorePointCollections | Evet | Evet |
| restorePointCollections/restorePoints | Hayır |  Hayır |
| Sharedvmımages | Evet | Evet |
| Sharedvmımages/sürümler | Evet | Evet |
| görüntüsünü | Evet | Evet |
| VirtualMachines | Evet | Evet |
| virtualMachines/diagnosticSettings | Hayır |  Hayır |
| virtualMachines/uzantıları | Evet | Evet |
| virtualMachineScaleSets | Evet | Evet |
| virtualMachineScaleSets/uzantılar | Hayır |  Hayır |
| virtualMachineScaleSets/NetworkInterfaces | Hayır |  Hayır |
| virtualMachineScaleSets/Publicıpaddresses | Hayır |  Hayır |
| virtualMachineScaleSets/virtualMachines | Hayır |  Hayır |
| virtualMachineScaleSets/virtualMachines/NetworkInterfaces | Hayır |  Hayır |

## <a name="microsoftconsumption"></a>Microsoft. tüketim
| Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
| ------------- | ----------- | ----------- |
| Aggregmalyt maliyeti | Hayır |  Hayır |
| Bakiyeler | Hayır |  Hayır |
| Bütçeler | Hayır |  Hayır |
| Ücretleriyle | Hayır |  Hayır |
| CostTags | Hayır |  Hayır |
| iler | Hayır |  Hayır |
| etkinlikler | Hayır |  Hayır |
| Tahminler | Hayır |  Hayır |
| oluş | Hayır |  Hayır |
| Marketlerinden | Hayır |  Hayır |
| Pricesheets | Hayır |  Hayır |
| ürün | Hayır |  Hayır |
| Rezervde ayrıntıları | Hayır |  Hayır |
| Rezervationönerilere | Hayır |  Hayır |
| Rezervlerin Özeti | Hayır |  Hayır |
| Rezervlik Işlemleri | Hayır |  Hayır |
| Tags | Hayır |  Hayır |
| Koşullar | Hayır |  Hayır |
| UsageDetails | Hayır |  Hayır |

## <a name="microsoftcontainerinstance"></a>Microsoft.ContainerInstance
| Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
| ------------- | ----------- | ----------- |
| Kapsayıcı grupları | Evet | Evet |
| serviceAssociationLinks | Hayır |  Hayır |

## <a name="microsoftcontainerregistry"></a>Microsoft.ContainerRegistry
| Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
| ------------- | ----------- | ----------- |
| kayıt | Evet | Evet |
| kayıt defterleri/derlemeler | Hayır |  Hayır |
| kayıt defterleri/derlemeler/iptal | Hayır |  Hayır |
| kayıt defterleri/derlemeler/getLogLink | Hayır |  Hayır |
| kayıt defterleri/buildTasks | Evet | Evet |
| kayıt defterleri/buildTasks/Steps | Hayır |  Hayır |
| kayıt defterleri/eventGridFilters | Hayır |  Hayır |
| kayıt defterleri/getBuildSourceUploadUrl 'Si | Hayır |  Hayır |
| kayıt defterleri/GetCredentials | Hayır |  Hayır |
| kayıt defterleri/ımportımage | Hayır |  Hayır |
| kayıt defterleri/queueBuild | Hayır |  Hayır |
| kayıt defterleri/regenerateCredential | Hayır |  Hayır |
| kayıt defterleri/regenerateCredentials | Hayır |  Hayır |
| kayıt defterleri/çoğaltmalar | Evet | Evet |
| kayıt defterleri/çalıştırmalar | Hayır |  Hayır |
| kayıt defterleri/çalıştırmalar/iptal | Hayır |  Hayır |
| kayıt defterleri/scheduleRun | Hayır |  Hayır |
| kayıt defterleri/görevler | Evet | Evet |
| kayıt defterleri/updatePolicies | Hayır |  Hayır |
| kayıt defterleri/Web kancaları | Evet | Evet |
| kayıt defterleri/Web kancaları/getCallbackConfig | Hayır |  Hayır |
| kayıt defterleri/Web kancaları/ping | Hayır |  Hayır |

## <a name="microsoftcontainerservice"></a>Microsoft.ContainerService
| Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
| ------------- | ----------- | ----------- |
| containerServices | Evet | Evet |
| Managedkümeler | Evet | Evet |

## <a name="microsoftcontentmoderator"></a>Microsoft.ContentModerator
| Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
| ------------- | ----------- | ----------- |
| uygulamalar | Evet | Evet |
| updateCommunicationPreference | Hayır |  Hayır |

## <a name="microsoftcortanaanalytics"></a>Microsoft. Cortanaanalizi
| Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
| ------------- | ----------- | ----------- |
| accounts | Evet | Evet |

## <a name="microsoftcostmanagement"></a>Microsoft. CostManagement
| Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
| ------------- | ----------- | ----------- |
| Uyarılar | Hayır |  Hayır |
| billingAccounts | Hayır |  Hayır |
| Bağlayıcılar | Evet | Evet |
| bölümlerinin | Hayır |  Hayır |
| Boyutlar | Hayır |  Hayır |
| kayıt sayısı | Hayır |  Hayır |
| Sorgu | Hayır |  Hayır |
| kaydolunamadı | Hayır |  Hayır |
| Reportconfigs | Hayır |  Hayır |
| Raporlar | Hayır |  Hayır |

## <a name="microsoftcustomerinsights"></a>Microsoft.CustomerInsights
| Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
| ------------- | ----------- | ----------- |
| Lara | Evet | Evet |
| Hub/authorizationPolicies | Hayır |  Hayır |
| Hub 'lar/bağlayıcılar | Hayır |  Hayır |
| Hub 'lar/bağlayıcılar/eşlemeler | Hayır |  Hayır |
| Hub 'lar/etkileşimler | Hayır |  Hayır |
| Hub/KPI | Hayır |  Hayır |
| Hub 'lar/bağlantılar | Hayır |  Hayır |
| Hub 'lar/profiller | Hayır |  Hayır |
| Hub/Roleatamaları | Hayır |  Hayır |
| Hub 'lar/roller | Hayır |  Hayır |
| Hub 'lar/suggestTypeSchema | Hayır |  Hayır |
| Hub 'lar/görünümler | Hayır |  Hayır |
| Hub 'lar/widgetTypes | Hayır |  Hayır |

## <a name="microsoftdatabox"></a>Microsoft. DataBox
| Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
| ------------- | ----------- | ----------- |
| Çizelge | Evet | Evet |

## <a name="microsoftdataboxedge"></a>Microsoft. DataBoxEdge
| Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
| ------------- | ----------- | ----------- |
| DataBoxEdgeDevices | Evet | Evet |

## <a name="microsoftdatabricks"></a>Microsoft. Databricks
| Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
| ------------- | ----------- | ----------- |
| Onlarla | Evet | Hayır |
| çalışma alanları/Virtualnetworkpeerler | Hayır |  Hayır |

## <a name="microsoftdatacatalog"></a>Microsoft.DataCatalog
| Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
| ------------- | ----------- | ----------- |
| larına | Evet | Evet |

## <a name="microsoftdataconnect"></a>Microsoft. DataConnect
| Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
| ------------- | ----------- | ----------- |
| Connectionyöneticileri | Evet | Evet |

## <a name="microsoftdatafactory"></a>Microsoft.DataFactory
| Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
| ------------- | ----------- | ----------- |
| Veri fabrikaları | Evet | Hayır |
| DataFactory/diagnosticSettings | Hayır |  Hayır |
| dataFactorySchema | Hayır |  Hayır |
| larının | Evet | Hayır |
| Fabrika/tümleştirme çalışma zamanları | Hayır |  Hayır |

## <a name="microsoftdatalakeanalytics"></a>Microsoft.DataLakeAnalytics
| Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
| ------------- | ----------- | ----------- |
| accounts | Evet | Evet |
| hesaplar/dataLakeStoreAccounts | Hayır |  Hayır |
| hesaplar/storageAccounts | Hayır |  Hayır |
| hesaplar/storageAccounts/kapsayıcılar | Hayır |  Hayır |

## <a name="microsoftdatalakestore"></a>Microsoft.DataLakeStore
| Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
| ------------- | ----------- | ----------- |
| accounts | Evet | Evet |
| hesaplar/eventGridFilters | Hayır |  Hayır |
| hesaplar/firewallRules | Hayır |  Hayır |

## <a name="microsoftdatamigration"></a>Microsoft. DataMigration
| Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
| ------------- | ----------- | ----------- |
| services | Evet | Evet |
| Hizmetler/Projeler | Evet | Evet |

## <a name="microsoftdbformariadb"></a>Microsoft. Dbformarıdb
| Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
| ------------- | ----------- | ----------- |
| Larý | Evet | Evet |
| sunucular/recoverableServers | Hayır |  Hayır |
| sunucular/virtualNetworkRules | Hayır |  Hayır |

## <a name="microsoftdbformysql"></a>Microsoft.DBforMySQL
| Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
| ------------- | ----------- | ----------- |
| Larý | Evet | Evet |
| sunucular/recoverableServers | Hayır |  Hayır |
| sunucular/virtualNetworkRules | Hayır |  Hayır |

## <a name="microsoftdbforpostgresql"></a>Microsoft.DBforPostgreSQL
| Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
| ------------- | ----------- | ----------- |
| Larý | Evet | Evet |
| sunucular/danışmanları | Hayır |  Hayır |
| sunucular/Querymetinmetinleri | Hayır |  Hayır |
| sunucular/recoverableServers | Hayır |  Hayır |
| sunucular/topQueryStatistics | Hayır |  Hayır |
| sunucular/virtualNetworkRules | Hayır |  Hayır |
| sunucular/waitStatistics | Hayır |  Hayır |

## <a name="microsoftdevices"></a>Microsoft. Devices
| Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
| ------------- | ----------- | ----------- |
| Iothubs | Evet | Evet |
| IotHubs/eventGridFilters | Hayır |  Hayır |
| ProvisioningServices | Evet | Evet |
| vardır | Hayır |  Hayır |

## <a name="microsoftdevspaces"></a>Microsoft. DevSpaces
| Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
| ------------- | ----------- | ----------- |
| Örleri | Evet | Evet |

## <a name="microsoftdevtestlab"></a>Microsoft.DevTestLab
| Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
| ------------- | ----------- | ----------- |
| larda | Evet | Evet |
| Labs/Servicerunanlar | Evet | Evet |
| Labs/virtualMachines | Evet | Evet |
| cağını | Evet | Evet |

## <a name="microsoftdocumentdb"></a>Microsoft.DocumentDB
| Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
| ------------- | ----------- | ----------- |
| databaseAccountNames | Hayır |  Hayır |
| Veritabanı hesapları | Evet | Evet |

## <a name="microsoftdomainregistration"></a>Microsoft.DomainRegistration
| Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
| ------------- | ----------- | ----------- |
| etki alanları | Evet | Evet |
| Domains/Domainownershiptanýmlayýcýlarý | Hayır |  Hayır |
| generateSsoRequest | Hayır |  Hayır |
| topLevelDomains | Hayır |  Hayır |
| Validatedomainregistrationınformation | Hayır |  Hayır |

## <a name="microsoftdynamicslcs"></a>Microsoft.DynamicsLcs
| Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
| ------------- | ----------- | ----------- |
| lcsprojects | Hayır |  Hayır |
| lcsprojects/clouddağıtımları | Hayır |  Hayır |
| lcsprojects/bağlayıcılar | Hayır |  Hayır |

## <a name="microsofteventgrid"></a>Microsoft.EventGrid
| Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
| ------------- | ----------- | ----------- |
| etki alanları | Evet | Hayır |
| etki alanları/konular | Hayır |  Hayır |
| Eventabonelikleri | Hayır |  Hayır |
| Extensionkonuları | Hayır |  Hayır |
| konularıyla | Evet | Hayır |
| topicTypes | Hayır |  Hayır |

## <a name="microsofteventhub"></a>Microsoft.EventHub
| Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
| ------------- | ----------- | ----------- |
| leriniz | Evet | Hayır |
| Öznitelikleri | Evet | Hayır |
| ad alanları/authorizationrules | Hayır |  Hayır |
| ad alanları/diskalrecoveryconfigs | Hayır |  Hayır |
| ad alanları/eventhubs | Hayır |  Hayır |
| ad alanları/eventhubs/authorizationrules | Hayır |  Hayır |
| ad alanları/eventhubs/consumergroups | Hayır |  Hayır |

## <a name="microsoftfeatures"></a>Microsoft. Features
| Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
| ------------- | ----------- | ----------- |
| SaaS Uygulamaları Geliştirme | Hayır |  Hayır |
| sağlayıcıları | Hayır |  Hayır |

## <a name="microsoftgallery"></a>Microsoft. Gallery
| Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
| ------------- | ----------- | ----------- |
| kaydedemez | Hayır |  Hayır |
| gallergıtems | Hayır |  Hayır |
| generateartifactaccessuri | Hayır |  Hayır |
| myarea | Hayır |  Hayır |
| myarea/alan | Hayır |  Hayır |
| myarea/alan/alan | Hayır |  Hayır |
| myareas/Areas/Areas/gallergıtems | Hayır |  Hayır |
| myareas/Areas/gallergıtems | Hayır |  Hayır |
| myarea/gallergıtems | Hayır |  Hayır |
| kaydolunamadı | Hayır |  Hayır |
| Kaynakların | Hayır |  Hayır |
| elde edilecek esourcesbyıd | Hayır |  Hayır |

## <a name="microsoftguestconfiguration"></a>Microsoft. GuestConfiguration
| Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
| ------------- | ----------- | ----------- |
| Guestconfigurationatamaları | Hayır |  Hayır |
| Yazılımıdır | Hayır |  Hayır |

## <a name="microsofthanaonazure"></a>Microsoft. HanaOnAzure
| Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
| ------------- | ----------- | ----------- |
| Hanaınstances | Evet |  Evet |

## <a name="microsofthdinsight"></a>Microsoft.HDInsight
| Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
| ------------- | ----------- | ----------- |
| leriniz | Evet | Evet |
| kümeler/uygulamalar | Hayır |  Hayır |

## <a name="microsoftimportexport"></a>Microsoft.ImportExport
| Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
| ------------- | ----------- | ----------- |
| Çizelge | Evet | Evet |

## <a name="microsoftinformationprotection"></a>Microsoft. ınformationprotection
| Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
| ------------- | ----------- | ----------- |
| labelGroups | Hayır |  Hayır |
| labelGroups/Etiketler | Hayır |  Hayır |
| labelGroups/Labels/koşullar | Hayır |  Hayır |
| labelGroups/Labels/alt etiketleri | Hayır |  Hayır |
| labelGroups/Labels/alt etiketleri/koşulları | Hayır |  Hayır |

## <a name="microsoftinsights"></a>Microsoft. Insights
| Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
| ------------- | ----------- | ----------- |
| actiongroups | Evet | Evet |
| activityLogAlerts | Evet | Evet |
| alertrules | Evet | Evet |
| automatedExportSettings | Hayır |  Hayır |
| autoscalesettings | Evet | Evet |
| çizgisi | Hayır |  Hayır |
| calculatebaseline | Hayır |  Hayır |
| Bileşenleri | Evet | Evet |
| bileşenler/olaylar | Hayır |  Hayır |
| bileşenler/Pricingplanlar | Hayır |  Hayır |
| bileşenler/sorgu | Hayır |  Hayır |
| diagnosticSettings | Hayır |  Hayır |
| diagnosticSettingsCategories | Hayır |  Hayır |
| eventCategories | Hayır |  Hayır |
| eventTypes | Hayır |  Hayır |
| extendedDiagnosticSettings | Hayır |  Hayır |
| logDefinitions | Hayır |  Hayır |
| günlüğe kaydetme profilleri | Hayır |  Hayır |
| logs | Hayır |  Hayır |
| metricAlerts | Evet | Evet |
| migrateToNewPricingModel | Hayır |  Hayır |
| Çalışma kitapları | Hayır |  Hayır |
| sorgu | Hayır |  Hayır |
| rollbackToLegacyPricingModel | Hayır |  Hayır |
| scheduledqueryrules | Evet | Evet |
| vmInsightsOnboardingStatuses | Hayır |  Hayır |
| Web testleri | Evet | Evet |
| Kitabı | Evet | Evet |

## <a name="microsoftintune"></a>Microsoft. Intune
| Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
| ------------- | ----------- | ----------- |
| diagnosticSettings | Hayır |  Hayır |
| diagnosticSettingsCategories | Hayır |  Hayır |

## <a name="microsoftiotcentral"></a>Microsoft. ıotcentral
| Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
| ------------- | ----------- | ----------- |
| Iotapps | Evet | Evet |

## <a name="microsoftiotspaces"></a>Microsoft. ıotspaces
| Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
| ------------- | ----------- | ----------- |
| Graf | Evet | Evet |

## <a name="microsoftkeyvault"></a>Microsoft.KeyVault
| Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
| ------------- | ----------- | ----------- |
| Silinkaults | Hayır |  Hayır |
| kasaları | Evet | Evet |
| kasa/erişim Ilkeleri | Hayır |  Hayır |
| kasa/gizlilikler | Hayır |  Hayır |

## <a name="microsoftkusto"></a>Microsoft. kusto
| Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
| ------------- | ----------- | ----------- |
| leriniz | Evet | Evet |
| kümeler/veritabanları | Hayır |  Hayır |
| kümeler/veritabanları/veri bağlantıları | Hayır |  Hayır |
| kümeler/veritabanları/eventhubconnections | Hayır |  Hayır |

## <a name="microsoftlabservices"></a>Microsoft. LabServices
| Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
| ------------- | ----------- | ----------- |
| labaccounts | Evet | Evet |
| kullanıcılar | Hayır |  Hayır |

## <a name="microsoftlocationbasedservices"></a>Microsoft.LocationBasedServices
| Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
| ------------- | ----------- | ----------- |
| accounts | Evet | Evet |

## <a name="microsoftlocationservices"></a>Microsoft. LocationServices
| Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
| ------------- | ----------- | ----------- |
| accounts | Evet | Evet |

## <a name="microsoftloganalytics"></a>Microsoft. LogAnalytics
| Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
| ------------- | ----------- | ----------- |
| logs | Hayır |  Hayır |

## <a name="microsoftlogic"></a>Microsoft.Logic
| Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
| ------------- | ----------- | ----------- |
| Tümleştirme hesapları | Evet | Evet |
| sürdürülen | Evet | Evet |

## <a name="microsoftmachinelearning"></a>Microsoft.MachineLearning
| Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
| ------------- | ----------- | ----------- |
| Commitmentplanlar | Evet | Evet |
| Hizmetleri | Evet | Evet |
| Çalışma Alanları | Evet | Evet |

## <a name="microsoftmachinelearningexperimentation"></a>Microsoft. MachineLearningExperimentation
| Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
| ------------- | ----------- | ----------- |
| accounts | Evet | Evet |
| hesaplar/çalışma alanları | Evet | Evet |
| hesaplar/çalışma alanları/projeler | Evet | Evet |
| teamAccounts | Evet | Evet |
| teamAccounts/çalışma alanları | Evet | Evet |
| teamAccounts/çalışma alanları/projeler | Evet | Evet |

## <a name="microsoftmachinelearningmodelmanagement"></a>Microsoft.MachineLearningModelManagement
| Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
| ------------- | ----------- | ----------- |
| accounts | Evet | Evet |

## <a name="microsoftmachinelearningservices"></a>Microsoft. MachineLearningServices
| Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
| ------------- | ----------- | ----------- |
| Onlarla | Evet | Evet |
| çalışma alanları/hesaplar | Hayır |  Hayır |

## <a name="microsoftmanagedidentity"></a>Microsoft. Managedıdentity
| Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
| ------------- | ----------- | ----------- |
| Lerinizde | Hayır |  Hayır |
| Userassignedıdentities | Evet | 

## <a name="microsoftmanagement"></a>Microsoft. Management
| Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
| ------------- | ----------- | ----------- |
| getEntities | Hayır |  Hayır |
| Yönetim grupları | Hayır |  Hayır |
| Kaynakların | Hayır |  Hayır |
| startTenantBackfill | Hayır |  Hayır |
| tenantBackfillStatus | Hayır |  Hayır |

## <a name="microsoftmaps"></a>Microsoft. Maps
| Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
| ------------- | ----------- | ----------- |
| accounts | Evet | Evet |
| hesaplar/eventGridFilters | Hayır |  Hayır |

## <a name="microsoftmarketplace"></a>Microsoft. Market
| Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
| ------------- | ----------- | ----------- |
| sunar | Hayır |  Hayır |
| offerTypes | Hayır |  Hayır |
| offerTypes/yayımcılar | Hayır |  Hayır |
| offerTypes/yayımcılar/teklifler | Hayır |  Hayır |
| offerTypes/yayımcılar/teklifler/planlar | Hayır |  Hayır |
| offerTypes/yayımcılar/teklifler/planlar/anlaşmalar | Hayır |  Hayır |
| offerTypes/yayımcılar/teklifler/planlar/configs | Hayır |  Hayır |
| offerTypes/yayımcılar/teklifler/planlar/configs/ımportımage | Hayır |  Hayır |
| privategallergıtems | Hayır |  Hayır |
| ürün | Hayır |  Hayır |

## <a name="microsoftmarketplaceapps"></a>Microsoft. MarketplaceApps
| Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
| ------------- | ----------- | ----------- |
| classicDevServices | Evet | Evet |
| updateCommunicationPreference | Hayır |  Hayır |

## <a name="microsoftmarketplaceordering"></a>Microsoft. Marketplacesıralaması
| Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
| ------------- | ----------- | ----------- |
| anlaşmalar | Hayır |  Hayır |
| offertypes | Hayır |  Hayır |

## <a name="microsoftmedia"></a>Microsoft. Media
| Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
| ------------- | ----------- | ----------- |
| mediaservices | Evet | Evet |
| mediaservices/accountFilters | Hayır |  Hayır |
| mediaservices/varlıklar | Hayır |  Hayır |
| mediaservices/varlıklar/assetFilters | Hayır |  Hayır |
| mediaservices/contentKeyPolicies | Hayır |  Hayır |
| mediaservices/eventGridFilters | Hayır |  Hayır |
| mediaservices/liveEventOperations | Hayır |  Hayır |
| mediaservices/liveEvents | Evet | Evet |
| mediaservices/liveEvents/Liveçıktılar | Hayır |  Hayır |
| mediaservices/liveOutputOperations | Hayır |  Hayır |
| mediaservices/streamingEndpointOperations | Hayır |  Hayır |
| mediaservices/streamingEndpoints | Evet | Evet |
| mediaservices/Streamingkonumlandırıcı | Hayır |  Hayır |
| mediaservices/streamingPolicies | Hayır |  Hayır |
| mediaservices/dönüşümler | Hayır |  Hayır |
| mediaservices/dönüşümler/işler | Hayır |  Hayır |

## <a name="microsoftmigrate"></a>Microsoft. Migrate
| Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
| ------------- | ----------- | ----------- |
| projeyle | Evet | Evet |

## <a name="microsoftnetwork"></a>Microsoft.Network
| Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
| ------------- | ----------- | ----------- |
| Applicationgateway 'ler | Evet | Hayır |
| applicationSecurityGroups | Evet | Evet |
| azureFirewallFqdnTags | Hayır |  Hayır |
| azureFirewalls | Evet | Hayır |
| bgpServiceCommunities | Hayır |  Hayır |
| bağlantının | Evet | Evet |
| ddosCustomPolicies | Evet | Evet |
| Ddosprotectionplanlar | Evet | Evet |
| Dnsoperationdurumları | Hayır |  Hayır |
| dnszones | Evet | Evet |
| dnszones/A | Hayır |  Hayır |
| dnszones/AAAA | Hayır |  Hayır |
| dnszones/tümü | Hayır |  Hayır |
| dnszones/CAA | Hayır |  Hayır |
| dnszones/CNAME | Hayır |  Hayır |
| dnszones/MX | Hayır |  Hayır |
| dnszones/NS | Hayır |  Hayır |
| dnszones/PTR | Hayır |  Hayır |
| dnszones/kayıt kümeleri | Hayır |  Hayır |
| dnszones/SOA | Hayır |  Hayır |
| dnszones/SRV | Hayır |  Hayır |
| dnszones/TXT | Hayır |  Hayır |
| Expressroutedevreleri | Evet  | Hayır |
| expressRouteServiceProviders | Hayır |  Hayır |
| frontkapıların | Evet, ancak sınırlı ( [aşağıdaki nota](#frontdoor)bakın) | Evet |
| frontdoorWebApplicationFirewallPolicies | Evet, ancak sınırlı ( [aşağıdaki nota](#frontdoor)bakın) | Evet |
| getDnsResourceReference | Hayır |  Hayır |
| ınterfaceendpoints | Evet | Evet |
| ınternalnotify | Hayır |  Hayır |
| loadBalancers | Evet | Hayır |
| Localnetworkgateway 'ler | Evet | Evet |
| Natgateway 'ler | Evet | Evet |
| Networkıntpolicies Ilkeleri | Evet | Evet |
| NetworkInterfaces | Evet | Evet |
| networkProfiles | Evet | Evet |
| networkSecurityGroups | Evet | Evet |
| networkWatchers | Evet | Hayır |
| networkWatchers/Connectionmonitörleri | Evet | Hayır |
| networkWatchers/uzunluler | Evet | Hayır |
| networkWatchers/Pingkafesler | Evet | Hayır |
| privateLinkServices | Evet | Evet |
| Publicıpaddresses | Evet | Evet |
| Publicıpöneklerini | Evet | Evet |
| routeFilters | Evet | Evet |
| routeTables | Evet | Evet |
| serviceEndpointPolicies | Evet | Evet |
| trafficManagerGeographicHierarchies | Hayır |  Hayır |
| trafficmanagerprofiles | Evet | Evet |
| trafficmanagerprofiles/heatMaps | Hayır |  Hayır |
| Virtualhub 'Lar | Evet | Evet |
| virtualNetworkGateways | Evet | Evet |
| virtualNetworks | Evet | Evet |
| virtualNetworks/alt ağları | Hayır |  Hayır |
| virtualNetworkTaps | Evet | Evet |
| Virtualwan | Evet | Evet |
| Vpngateway 'ler | Evet | Hayır |
| vpnSites | Evet | Evet |
| webApplicationFirewallPolicies | Evet | Evet |

<a id="frontdoor" />

Azure ön kapı hizmeti için, kaynak oluştururken Etiketler uygulayabilirsiniz, ancak bu durumda etiketleri güncelleştirmek veya eklemek Şu anda desteklenmemektedir.

## <a name="microsoftnotificationhubs"></a>Microsoft.NotificationHubs
| Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
| ------------- | ----------- | ----------- |
| Öznitelikleri | Evet | Hayır |
| ad alanları/Notificationhub 'Lar | Evet | Hayır |

## <a name="microsoftoperationalinsights"></a>Microsoft.OperationalInsights
| Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
| ------------- | ----------- | ----------- |
| cihazlar | Hayır |  Hayır |
| Bağlantı hedefleri | Hayır |  Hayır |
| Storageınsii configs | Hayır |  Hayır |
| Onlarla | Evet | Evet |
| çalışma alanları/veri kaynakları | Hayır |  Hayır |
| çalışma alanları/linkedServices | Hayır |  Hayır |
| çalışma alanları/sorgu | Hayır |  Hayır |

## <a name="microsoftoperationsmanagement"></a>Microsoft.OperationsManagement
| Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
| ------------- | ----------- | ----------- |
| managementassociations | Hayır |  Hayır |
| managementconfigurations | Evet | Evet |
| çözümler | Evet | Evet |
| Görünümler | Evet | Evet |

## <a name="microsoftpolicyinsights"></a>Microsoft. Poliyelei
| Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
| ------------- | ----------- | ----------- |
| Poliyevents | Hayır |  Hayır |
| policyStates | Hayır |  Hayır |
| policyTrackedResources | Hayır |  Hayır |
| düzeltmeler | Hayır |  Hayır |

## <a name="microsoftportal"></a>Microsoft. Portal
| Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
| ------------- | ----------- | ----------- |
| konsolları | Hayır |  Hayır |
| Panoların | Evet | Evet |
| userSettings | Hayır |  Hayır |

## <a name="microsoftpowerbi"></a>Microsoft. PowerBI
| Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
| ------------- | ----------- | ----------- |
| workspaceCollections | Evet | Evet |

## <a name="microsoftpowerbidedicated"></a>Microsoft.PowerBIDedicated
| Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
| ------------- | ----------- | ----------- |
| kapasiteler | Evet | Evet |

## <a name="microsoftprojectoxford"></a>Microsoft. ProjectOxford
| Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
| ------------- | ----------- | ----------- |
| accounts | Evet | Evet |

## <a name="microsoftrecoveryservices"></a>Microsoft. RecoveryServices
| Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
| ------------- | ----------- | ----------- |
| Backupkorunabilir | Hayır |  Hayır |
| kasaları | Evet | Evet |

## <a name="microsoftrelay"></a>Microsoft. Relay
| Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
| ------------- | ----------- | ----------- |
| Öznitelikleri | Evet | Evet |
| ad alanları/authorizationrules | Hayır |  Hayır |
| ad alanları/hybridconnections | Hayır |  Hayır |
| ad alanları/hybridconnections/authorizationrules | Hayır |  Hayır |
| ad alanları/wcfreyerleştiri | Hayır |  Hayır |
| ad alanları/wcfreyerleştirme/authorizationrules | Hayır |  Hayır |

## <a name="microsoftresourcegraph"></a>Microsoft. ResourceGraph
| Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
| ------------- | ----------- | ----------- |
| Kaynakların | Hayır |  Hayır |
| subscriptionsStatus | Hayır |  Hayır |

## <a name="microsoftresourcehealth"></a>Microsoft. ResourceHealth
| Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
| ------------- | ----------- | ----------- |
| Kullanılabilirlik durumları | Hayır |  Hayır |
| Childadvailabilitydurumlar | Hayır |  Hayır |
| childResources | Hayır |  Hayır |
| etkinlikler | Hayır |  Hayır |
| ımpactedresources | Hayır |  Hayır |
| Bildirimi | Hayır |  Hayır |

## <a name="microsoftresources"></a>Microsoft. resources
| Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
| ------------- | ----------- | ----------- |
| dağıtımlar | Hayır |  Hayır |
| dağıtımlar/işlemler | Hayır |  Hayır |
| Köprü | Hayır |  Hayır |
| notifyResourceJobs | Hayır |  Hayır |
| sağlayıcıları | Hayır |  Hayır |
| resourceGroups | Hayır |  Hayır |
| Kaynakların | Hayır |  Hayır |
| Aboneliklerin | Hayır |  Hayır |
| Abonelikler/sağlayıcılar | Hayır |  Hayır |
| Abonelikler/resourceGroups | Hayır |  Hayır |
| Abonelikler/ResourceGroups/kaynaklar | Hayır |  Hayır |
| Abonelikler/kaynaklar | Hayır |  Hayır |
| Abonelikler/etiket adları | Hayır |  Hayır |
| Abonelikler/etiket adları/tagValues | Hayır |  Hayır |
| Kira | Hayır |  Hayır |

## <a name="microsoftsaas"></a>Microsoft. SaaS
| Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
| ------------- | ----------- | ----------- |
| uygulamalar | Evet | Evet |
| saasresources | Hayır |  Hayır |

## <a name="microsoftscheduler"></a>Microsoft. Scheduler
| Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
| ------------- | ----------- | ----------- |
| var | Evet | Evet |
| işlere | Evet | Evet |

## <a name="microsoftsearch"></a>Microsoft.Search
| Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
| ------------- | ----------- | ----------- |
| resourceHealthMetadata | Hayır |  Hayır |
| searchServices | Evet | Evet |

## <a name="microsoftsecurity"></a>Microsoft.Security
| Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
| ------------- | ----------- | ----------- |
| advancedThreatProtectionSettings | Hayır |  Hayır |
| alerts | Hayır |  Hayır |
| allowedConnections | Hayır |  Hayır |
| yaptı | Hayır |  Hayır |
| Applicationwhitedökümler | Hayır |  Hayır |
| Oto Provisioningsettings | Hayır |  Hayır |
| Uyumluluklarına | Hayır |  Hayır |
| dataCollectionAgents | Hayır |  Hayır |
| discoveredSecuritySolutions | Hayır |  Hayır |
| externalSecuritySolutions | Hayır |  Hayır |
| Informationprotectionpolicies | Hayır |  Hayır |
| Jağaccesspolicies | Hayır |  Hayır |
| İzlemesinin | Hayır |  Hayır |
| izleme/kötü amaçlı yazılımdan koruma | Hayır |  Hayır |
| izleme/temel | Hayır |  Hayır |
| izleme/düzeltme eki uygulama | Hayır |  Hayır |
| Elerindeki | Hayır |  Hayır |
| fiyatlandırmalar | Hayır |  Hayır |
| securityContacts | Hayır |  Hayır |
| securitySolutions | Hayır |  Hayır |
| securitySolutionsReferenceData | Hayır |  Hayır |
| securityStatus | Hayır |  Hayır |
| securityStatus/endpoints | Hayır |  Hayır |
| securityStatus/alt ağları | Hayır |  Hayır |
| securityStatus/virtualMachines | Hayır |  Hayır |
| Securitydurumlardan | Hayır |  Hayır |
| securityStatusesSummaries | Hayır |  Hayır |
| ayarlar | Hayır |  Hayır |
| Görevler | Hayır |  Hayır |
| anlatır | Hayır |  Hayır |
| çalışma alanı ayarları | Hayır |  Hayır |

## <a name="microsoftsecuritygraph"></a>Microsoft. SecurityGraph
| Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
| ------------- | ----------- | ----------- |
| diagnosticSettings | Hayır |  Hayır |
| diagnosticSettingsCategories | Hayır |  Hayır |

## <a name="microsoftservicebus"></a>Microsoft.ServiceBus
| Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
| ------------- | ----------- | ----------- |
| Öznitelikleri | Evet | Hayır |
| ad alanları/authorizationrules | Hayır |  Hayır |
| ad alanları/diskalrecoveryconfigs | Hayır |  Hayır |
| ad alanları/eventgridfilters | Hayır |  Hayır |
| ad alanları/kuyruklar | Hayır |  Hayır |
| ad alanları/kuyruklar/authorizationrules | Hayır |  Hayır |
| ad alanları/konular | Hayır |  Hayır |
| ad alanları/konular/authorizationrules | Hayır |  Hayır |
| ad alanları/konular/abonelikler | Hayır |  Hayır |
| ad alanları/konular/abonelikler/kurallar | Hayır |  Hayır |
| premiumMessagingRegions | Hayır |  Hayır |

## <a name="microsoftservicefabric"></a>Microsoft.ServiceFabric
| Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
| ------------- | ----------- | ----------- |
| leriniz | Evet | Evet |
| kümeler/uygulamalar | Hayır |  Hayır |

## <a name="microsoftservicefabricmesh"></a>Microsoft. Servicefabrickafesi
| Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
| ------------- | ----------- | ----------- |
| uygulamalar | Evet | Evet |
| geçidinin | Evet | Evet |
| Mamak | Evet | Evet |
| kaynaklanır | Evet | Evet |
| volumes | Evet | Evet |

## <a name="microsoftsignalrservice"></a>Microsoft. SignalRService
| Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
| ------------- | ----------- | ----------- |
| SignalR | Evet | Evet |

## <a name="microsoftsolutions"></a>Microsoft. Solutions
| Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
| ------------- | ----------- | ----------- |
| applianceDefinitions | Evet | Evet |
| yaptı | Evet | Evet |
| applicationDefinitions | Evet | Evet |
| uygulamalar | Evet | Evet |
| Jistekleri | Evet | Evet |

## <a name="microsoftsql"></a>Microsoft. SQL
| Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
| ------------- | ----------- | ----------- |
| ManagedInstances | Evet | Evet |
| ManagedInstances/veritabanları | Evet (aşağıdaki nota bakın) | Evet |
| ManagedInstances/veritabanları/backupShortTermRetentionPolicies | Hayır | Hayır |
| ManagedInstances/veritabanları/şemalar/tablolar/sütunlar/sensitivityLabels | Hayır | Hayır |
| ManagedInstances/veritabanları/ | Hayır | Hayır |
| ManagedInstances/veritabanları/ek | Hayır | Hayır |
| ManagedInstances/encryptionProtector | Hayır | Hayır |
| ManagedInstances/anahtarlar | Hayır | Hayır |
| ManagedInstances/Restokbledroppeddatabases/backupShortTermRetentionPolicies | Hayır | Hayır |
| ManagedInstances/ | Hayır | Hayır |
| Larý | Evet | Evet |
| sunucular/Yöneticiler | Hayır |  Hayır |
| sunucular/communicationLinks | Hayır |  Hayır |
| sunucular/veritabanları | Evet (aşağıdaki nota bakın) | Evet |
| sunucular/encryptionProtector | Hayır |  Hayır |
| sunucular/firewallRules | Hayır |  Hayır |
| sunucular/anahtarlar | Hayır |  Hayır |
| sunucular/Restokbledroppeddatabases | Hayır |  Hayır |
| Sunucu/hizmet hedefleri | Hayır |  Hayır |
| sunucular/tdeCertificates | Hayır |  Hayır |

> [!NOTE]
> Ana veritabanı etiketleri desteklemez, ancak Azure SQL veri ambarı veritabanları da dahil olmak üzere diğer veritabanları, Etiketler ' i destekler. Azure SQL veri ambarı veritabanlarının etkin (duraklatılmış değil) durumda olması gerekir.


## <a name="microsoftsqlvirtualmachine"></a>Microsoft. SqlVirtualMachine
| Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
| ------------- | ----------- | ----------- |
| SqlVirtualMachineGroups | Evet | Evet |
| SqlVirtualMachineGroups/AvailabilityGroupListeners | Hayır |  Hayır |
| SqlVirtualMachines | Evet | Evet |

## <a name="microsoftstorage"></a>Microsoft.Storage
| Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
| ------------- | ----------- | ----------- |
| storageAccounts | Evet | Evet |
| storageAccounts/blobServices | Hayır |  Hayır |
| storageAccounts/fileServices | Hayır |  Hayır |
| storageAccounts/queueServices | Hayır |  Hayır |
| storageAccounts/Services | Hayır |  Hayır |
| storageAccounts/tableServices | Hayır |  Hayır |
| vardır | Hayır |  Hayır |

## <a name="microsoftstoragesync"></a>Microsoft.StorageSync
| Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
| ------------- | ----------- | ----------- |
| storageSyncServices | Evet | Evet |
| storageSyncServices/registeredServers | Hayır |  Hayır |
| storageSyncServices/syncGroups | Hayır |  Hayır |
| storageSyncServices/syncGroups/cloudEndpoints | Hayır |  Hayır |
| storageSyncServices/syncGroups/serverEndpoints | Hayır |  Hayır |
| storageSyncServices/iş akışları | Hayır |  Hayır |

## <a name="microsoftstorsimple"></a>Microsoft.StorSimple
| Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
| ------------- | ----------- | ----------- |
| ilerinde | Evet | Evet |

## <a name="microsoftstreamanalytics"></a>Microsoft.StreamAnalytics
| Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
| ------------- | ----------- | ----------- |
| streammingjobs | Evet (aşağıdaki nota bakın) | Evet |
| streamingjobs/diagnosticSettings | Hayır |  Hayır |

> [!NOTE]
> Streamingjobs çalışırken bir etiket ekleyemezsiniz. Etiketi eklemek için kaynağı durdurun.

## <a name="microsoftsubscription"></a>Microsoft. Subscription
| Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
| ------------- | ----------- | ----------- |
| CreateSubscription | Hayır |  Hayır |
| SubscriptionDefinitions | Hayır |  Hayır |
| SubscriptionOperations | Hayır |  Hayır |

## <a name="microsoftsupport"></a>Microsoft. support
| Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
| ------------- | ----------- | ----------- |
| destek biletleri | Hayır |  Hayır |

## <a name="microsoftterraformoss"></a>Microsoft. Teroyformoss
| Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
| ------------- | ----------- | ----------- |
| Providerkayıtları | Evet | Evet |
| Kaynakların | Evet | Evet |

## <a name="microsofttimeseriesinsights"></a>Microsoft.TimeSeriesInsights
| Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
| ------------- | ----------- | ----------- |
| lý | Evet | Hayır |
| ortamlar/accessPolicies | Hayır |  Hayır |
| ortamlar/EventSources | Evet | Hayır |
| ortamlar/Referencedataset 'ler | Evet | Hayır |

## <a name="microsoftvisualstudio"></a>Microsoft. VisualStudio
| Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
| ------------- | ----------- | ----------- |
| account | Evet | Evet |
| Hesap/uzantı | Evet | Evet |
| hesap/proje | Evet | Evet |

## <a name="microsoftweb"></a>Microsoft. Web
| Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
| ------------- | ----------- | ----------- |
| apiManagementAccounts | Hayır |  Hayır |
| apiManagementAccounts/Apiacl 'Ler | Hayır |  Hayır |
| apiManagementAccounts/API 'ler | Hayır |  Hayır |
| apiManagementAccounts/API/Apiacl 'Ler | Hayır |  Hayır |
| apiManagementAccounts/API/Connectionacl 'Ler | Hayır |  Hayır |
| apiManagementAccounts/API/bağlantı | Hayır |  Hayır |
| apiManagementAccounts/API/Connections/Connectionacl 'Ler | Hayır |  Hayır |
| apiManagementAccounts/API/localizedDefinitions | Hayır |  Hayır |
| apiManagementAccounts/Connectionacl 'Ler | Hayır |  Hayır |
| apiManagementAccounts/bağlantılar | Hayır |  Hayır |
| billingMeters | Hayır |  Hayır |
| Sertifika | Evet | Evet |
| Connectiongateway 'ler | Evet | Evet |
| bağlantının | Evet | Evet |
| Customapsıs | Evet | Evet |
| Silinmi siteleri | Hayır |  Hayır |
| işlevleri | Hayır |  Hayır |
| hostingEnvironments | Evet | Evet |
| hostingEnvironments/multiRolePools | Hayır |  Hayır |
| hostingEnvironments/multiRolePools/örnekler | Hayır |  Hayır |
| hostingEnvironments/workerPools | Hayır |  Hayır |
| hostingEnvironments/workerPools/örnekleri | Hayır |  Hayır |
| publishingUsers | Hayır |  Hayır |
| Önerileri | Hayır |  Hayır |
| resourceHealthMetadata | Hayır |  Hayır |
| zamanları | Hayır |  Hayır |
| serverFarms | Evet | Evet |
| Sunucu grupları/çalışanlar | Hayır |  Hayır |
| barındıra | Evet | Evet |
| Sites/Domainownershiptanýmlayýcýlarý | Hayır |  Hayır |
| siteler/hostNameBindings | Hayır |  Hayır |
| siteler/örnekler | Hayır |  Hayır |
| siteler/örnekler/uzantılar | Hayır |  Hayır |
| siteler/premieraddons | Evet | Evet |
| siteler/öneriler | Hayır |  Hayır |
| siteler/resourceHealthMetadata | Hayır |  Hayır |
| siteler/yuvalar | Evet | Evet |
| siteler/yuvalar/hostNameBindings | Hayır |  Hayır |
| siteler/yuvalar/örnekler | Hayır |  Hayır |
| siteler/yuvalar/örnekler/uzantılar | Hayır |  Hayır |
| sourceControls | Hayır |  Hayır |
| doğrulamalısınız | Hayır |  Hayır |
| verifyHostingEnvironmentVnet | Hayır |  Hayır |

## <a name="microsoftwindowsdefenderatp"></a>Microsoft. Windowssavunma Deratp
| Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
| ------------- | ----------- | ----------- |
| diagnosticSettings | Hayır |  Hayır |
| diagnosticSettingsCategories | Hayır |  Hayır |

## <a name="microsoftwindowsiot"></a>Microsoft. Windowsıot
| Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
| ------------- | ----------- | ----------- |
| DeviceServices | Evet | Evet |

## <a name="microsoftworkloadmonitor"></a>Microsoft. WorkloadMonitor
| Kaynak türü | Etiketleri destekler | Maliyet raporundaki etiket |
| ------------- | ----------- | ----------- |
| Bileşenleri | Hayır |  Hayır |
| componentsSummary | Hayır |  Hayır |
| Izleme örnekleri | Hayır |  Hayır |
| Izleme ınstancessummary | Hayır |  Hayır |
| Monitörün | Hayır |  Hayır |
| notificationSettings | Hayır |  Hayır |

## <a name="next-steps"></a>Sonraki adımlar
Kaynaklara etiketlerin nasıl uygulanacağını öğrenmek için bkz. [Azure kaynaklarınızı düzenlemek için etiketleri kullanma](resource-group-using-tags.md).
