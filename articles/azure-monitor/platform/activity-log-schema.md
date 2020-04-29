---
title: Azure etkinlik günlüğü olay şeması
description: Azure etkinlik günlüğündeki her bir kategorinin olay şemasını açıklar.
author: bwren
services: azure-monitor
ms.topic: reference
ms.date: 12/04/2019
ms.author: bwren
ms.subservice: logs
ms.openlocfilehash: c2f171c79423e0cfe8b57c05b8248679f9ada9f1
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "79472750"
---
# <a name="azure-activity-log-event-schema"></a>Azure etkinlik günlüğü olay şeması
[Azure etkinlik günlüğü](platform-logs-overview.md) , Azure 'da oluşan herhangi bir abonelik düzeyindeki olay hakkında öngörüler sağlar. Bu makalede her bir kategorinin olay şeması açıklanmaktadır. 

Aşağıdaki örneklerde, Portal, PowerShell, CLı ve REST API etkinlik günlüğüne eriştiğinizde şema gösterilmektedir. [Depolama veya Event Hubs etkinlik günlüğünü akışındaki](resource-logs-stream-event-hubs.md)şema farklı olur. Özelliklerin sonunda, [kaynak günlükleri şemasının](diagnostic-logs-schema.md) özelliklerinin bir eşlemesi verilmiştir.

## <a name="administrative"></a>Yönetim
Bu kategori, Kaynak Yöneticisi aracılığıyla gerçekleştirilen tüm oluşturma, güncelleştirme, silme ve eylem işlemlerinin kaydını içerir. Bu kategoride göreceğiniz olay türlerine örnek olarak "sanal makine oluştur" ve "ağ güvenlik grubunu sil" gibi Kaynak Yöneticisi kullanarak bir kullanıcı veya uygulama tarafından gerçekleştirilen her eylem, belirli bir kaynak türü üzerinde işlem olarak modellenir. İşlem türü yazma, silme veya eylem ise, bu işlemin hem başlangıç hem de başarı veya başarısızlık kayıtları yönetim kategorisine kaydedilir. Yönetim kategorisi, bir abonelikte rol tabanlı erişim denetimine yapılan tüm değişiklikleri de içerir.

### <a name="sample-event"></a>Örnek olay
```json
{
    "authorization": {
        "action": "Microsoft.Network/networkSecurityGroups/write",
        "scope": "/subscriptions/<subscription ID>/resourcegroups/myResourceGroup/providers/Microsoft.Network/networkSecurityGroups/myNSG"
    },
    "caller": "rob@contoso.com",
    "channels": "Operation",
    "claims": {
        "aud": "https://management.core.windows.net/",
        "iss": "https://sts.windows.net/1114444b-7467-4144-a616-e3a5d63e147b/",
        "iat": "1234567890",
        "nbf": "1234567890",
        "exp": "1234567890",
        "_claim_names": "{\"groups\":\"src1\"}",
        "_claim_sources": "{\"src1\":{\"endpoint\":\"https://graph.microsoft.com/1114444b-7467-4144-a616-e3a5d63e147b/users/f409edeb-4d29-44b5-9763-ee9348ad91bb/getMemberObjects\"}}",
        "http://schemas.microsoft.com/claims/authnclassreference": "1",
        "aio": "A3GgTJdwK4vy7Fa7l6DgJC2mI0GX44tML385OpU1Q+z+jaPnFMwB",
        "http://schemas.microsoft.com/claims/authnmethodsreferences": "rsa,mfa",
        "appid": "355249ed-15d9-460d-8481-84026b065942",
        "appidacr": "2",
        "http://schemas.microsoft.com/2012/01/devicecontext/claims/identifier": "10845a4d-ffa4-4b61-a3b4-e57b9b31cdb5",
        "e_exp": "262800",
        "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname": "Robertson",
        "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname": "Rob",
        "ipaddr": "111.111.1.111",
        "name": "Rob Robertson",
        "http://schemas.microsoft.com/identity/claims/objectidentifier": "f409edeb-4d29-44b5-9763-ee9348ad91bb",
        "onprem_sid": "S-1-5-21-4837261184-168309720-1886587427-18514304",
        "puid": "18247BBD84827C6D",
        "http://schemas.microsoft.com/identity/claims/scope": "user_impersonation",
        "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier": "b-24Jf94A3FH2sHWVIFqO3-RSJEiv24Jnif3gj7s",
        "http://schemas.microsoft.com/identity/claims/tenantid": "1114444b-7467-4144-a616-e3a5d63e147b",
        "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name": "rob@contoso.com",
        "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn": "rob@contoso.com",
        "uti": "IdP3SUJGtkGlt7dDQVRPAA",
        "ver": "1.0"
    },
    "correlationId": "b5768deb-836b-41cc-803e-3f4de2f9e40b",
    "eventDataId": "d0d36f97-b29c-4cd9-9d3d-ea2b92af3e9d",
    "eventName": {
        "value": "EndRequest",
        "localizedValue": "End request"
    },
    "category": {
        "value": "Administrative",
        "localizedValue": "Administrative"
    },
    "eventTimestamp": "2018-01-29T20:42:31.3810679Z",
    "id": "/subscriptions/<subscription ID>/resourcegroups/myResourceGroup/providers/Microsoft.Network/networkSecurityGroups/myNSG/events/d0d36f97-b29c-4cd9-9d3d-ea2b92af3e9d/ticks/636528553513810679",
    "level": "Informational",
    "operationId": "04e575f8-48d0-4c43-a8b3-78c4eb01d287",
    "operationName": {
        "value": "Microsoft.Network/networkSecurityGroups/write",
        "localizedValue": "Microsoft.Network/networkSecurityGroups/write"
    },
    "resourceGroupName": "myResourceGroup",
    "resourceProviderName": {
        "value": "Microsoft.Network",
        "localizedValue": "Microsoft.Network"
    },
    "resourceType": {
        "value": "Microsoft.Network/networkSecurityGroups",
        "localizedValue": "Microsoft.Network/networkSecurityGroups"
    },
    "resourceId": "/subscriptions/<subscription ID>/resourcegroups/myResourceGroup/providers/Microsoft.Network/networkSecurityGroups/myNSG",
    "status": {
        "value": "Succeeded",
        "localizedValue": "Succeeded"
    },
    "subStatus": {
        "value": "",
        "localizedValue": ""
    },
    "submissionTimestamp": "2018-01-29T20:42:50.0724829Z",
    "subscriptionId": "<subscription ID>",
    "properties": {
        "statusCode": "Created",
        "serviceRequestId": "a4c11dbd-697e-47c5-9663-12362307157d",
        "responseBody": "",
        "requestbody": ""
    },
    "relatedEvents": []
}

```

### <a name="property-descriptions"></a>Özellik açıklamaları
| Öğe Adı | Açıklama |
| --- | --- |
| yetkilendirme |Etkinliğin RBAC özelliklerinin blobu. Genellikle "Action", "role" ve "scope" özelliklerini içerir. |
| yapana |Kullanılabilirliği temel alarak işlemi, UPN talebini veya SPN talebini gerçekleştiren kullanıcının e-posta adresi. |
| lardan |Şu değerlerden biri: "admin", "Operation" |
| belirt |Kaynak Yöneticisi içinde bu işlemi gerçekleştirmek için kullanıcının veya uygulamanın kimliğini doğrulamak üzere Active Directory tarafından kullanılan JWT belirteci. |
| correlationId |Genellikle dize biçimindeki bir GUID. Bir CorrelationId 'yi paylaşan olaylar aynı Uber eylemine aittir. |
| açıklama |Bir olayın statik metin açıklaması. |
| Eventdataıd |Bir olayın benzersiz tanımlayıcısı. |
| eventName | Yönetim olayının kolay adı. |
| category | Her zaman "Yönetici" |
| httpRequest |Http Isteğini açıklayan blob. Genellikle "Clientrequestıd", "clientIpAddress" ve "method" (HTTP yöntemi) içerir. Örneğin, PUT). |
| düzey |Etkinliğin düzeyi. Şu değerlerden biri: "kritik", "hata", "uyarı" ve "bilgilendirme" |
| resourceGroupName |Etkilenen kaynak için kaynak grubunun adı. |
| resourceProviderName |Etkilenen kaynak için kaynak sağlayıcısının adı |
| resourceType | Bir yönetim olayından etkilenen kaynak türü. |
| resourceId |Etkilenen kaynağın kaynak KIMLIĞI. |
| operationId |Tek bir işleme karşılık gelen olaylar arasında paylaşılan bir GUID. |
| operationName |İşlemin adı. |
| properties |Olayın ayrıntılarını `<Key, Value>` açıklayan çiftler (yani bir sözlük) kümesi. |
| durum |İşlemin durumunu açıklayan dize. Bazı ortak değerler şunlardır: başlatıldı, devam ediyor, başarılı, başarısız, etkin, Çözümlenmiş. |
| Dosya |Genellikle karşılık gelen REST çağrısının HTTP durum kodu, ancak alt durumu açıklayan diğer dizeleri de içerebilir. bu ortak değerler gibi: Tamam (HTTP durum kodu: 200), oluşturulan (http durum kodu: 201), kabul edildi (http durum kodu: 202), Içerik yok (http durum kodu: 204), bozuk Istek (http durum kodu: 400), bulunamadı (http durum kodu: 404), çakışma (http durum kodu : 409), iç sunucu hatası (HTTP durum kodu: 500), hizmet kullanılamıyor (HTTP durum kodu: 503), ağ geçidi zaman aşımı (HTTP durum kodu: 504). |
| eventTimestamp |Olayın, olaya karşılık gelen isteği işleyen Azure hizmeti tarafından oluşturulduğu zaman damgası. |
| submissionTimestamp |Olay sorgulama için kullanılabilir hale geldiğinde zaman damgası. |
| subscriptionId |Azure abonelik KIMLIĞI. |

## <a name="service-health"></a>Hizmet durumu
Bu kategori, Azure 'da oluşan hizmet durumu olaylarının kaydını içerir. Bu kategoride göreceğiniz olay türüne bir örnek, "SQL Azure Doğu ABD kapalı kalma süresi yaşıyor." Hizmet durumu olayları beş değişen elikler halinde gelir: eylem gerekli, yardımlı kurtarma, olay, bakım, bilgi veya güvenlik, ancak abonelikte bundan etkilenecek bir kaynağınız varsa görünür.

### <a name="sample-event"></a>Örnek olay
```json
{
  "channels": "Admin",
  "correlationId": "c550176b-8f52-4380-bdc5-36c1b59d3a44",
  "description": "Active: Network Infrastructure - UK South",
  "eventDataId": "c5bc4514-6642-2be3-453e-c6a67841b073",
  "eventName": {
      "value": null
  },
  "category": {
      "value": "ServiceHealth",
      "localizedValue": "Service Health"
  },
  "eventTimestamp": "2017-07-20T23:30:14.8022297Z",
  "id": "/subscriptions/<subscription ID>/events/c5bc4514-6642-2be3-453e-c6a67841b073/ticks/636361902148022297",
  "level": "Warning",
  "operationName": {
      "value": "Microsoft.ServiceHealth/incident/action",
      "localizedValue": "Microsoft.ServiceHealth/incident/action"
  },
  "resourceProviderName": {
      "value": null
  },
  "resourceType": {
      "value": null,
      "localizedValue": ""
  },
  "resourceId": "/subscriptions/<subscription ID>",
  "status": {
      "value": "Active",
      "localizedValue": "Active"
  },
  "subStatus": {
      "value": null
  },
  "submissionTimestamp": "2017-07-20T23:30:34.7431946Z",
  "subscriptionId": "<subscription ID>",
  "properties": {
    "title": "Network Infrastructure - UK South",
    "service": "Service Fabric",
    "region": "UK South",
    "communication": "Starting at approximately 21:41 UTC on 20 Jul 2017, a subset of customers in UK South may experience degraded performance, connectivity drops or timeouts when accessing their Azure resources hosted in this region. Engineers are investigating underlying Network Infrastructure issues in this region. Impacted services may include, but are not limited to App Services, Automation, Service Bus, Log Analytics, Key Vault, SQL Database, Service Fabric, Event Hubs, Stream Analytics, Azure Data Movement, API Management, and Azure Cognitive Search. Multiple engineering teams are engaged in multiple workflows to mitigate the impact. The next update will be provided in 60 minutes, or as events warrant.",
    "incidentType": "Incident",
    "trackingId": "NA0F-BJG",
    "impactStartTime": "2017-07-20T21:41:00.0000000Z",
    "impactedServices": "[{\"ImpactedRegions\":[{\"RegionName\":\"UK South\"}],\"ServiceName\":\"Service Fabric\"}]",
    "defaultLanguageTitle": "Network Infrastructure - UK South",
    "defaultLanguageContent": "Starting at approximately 21:41 UTC on 20 Jul 2017, a subset of customers in UK South may experience degraded performance, connectivity drops or timeouts when accessing their Azure resources hosted in this region. Engineers are investigating underlying Network Infrastructure issues in this region. Impacted services may include, but are not limited to App Services, Automation, Service Bus, Log Analytics, Key Vault, SQL Database, Service Fabric, Event Hubs, Stream Analytics, Azure Data Movement, API Management, and Azure Cognitive Search. Multiple engineering teams are engaged in multiple workflows to mitigate the impact. The next update will be provided in 60 minutes, or as events warrant.",
    "stage": "Active",
    "communicationId": "636361902146035247",
    "version": "0.1.1"
  }
}
```
Özelliklerindeki değerler hakkında belgeler için [hizmet durumu bildirimleri](./../../azure-monitor/platform/service-notifications.md) makalesine başvurun.

## <a name="resource-health"></a>Kaynak durumu
Bu kategori, Azure kaynaklarınızda oluşan herhangi bir kaynak sistem durumu olayının kaydını içerir. Bu kategoride göreceğiniz olay türüne bir örnek, "sanal makine sistem durumu kullanılamıyor olarak değiştirildi." dır. Kaynak sistem durumu olayları şu dört sistem durumunu temsil edebilir: kullanılabilir, kullanılamaz, düşürülmüş ve bilinmiyor. Ayrıca, kaynak sistem durumu olayları, platform tarafından başlatılan veya Kullanıcı tarafından başlatılan olarak kategorize edilebilir.

### <a name="sample-event"></a>Örnek olay

```json
{
    "channels": "Admin, Operation",
    "correlationId": "28f1bfae-56d3-7urb-bff4-194d261248e9",
    "description": "",
    "eventDataId": "a80024e1-883d-37ur-8b01-7591a1befccb",
    "eventName": {
        "value": "",
        "localizedValue": ""
    },
    "category": {
        "value": "ResourceHealth",
        "localizedValue": "Resource Health"
    },
    "eventTimestamp": "2018-09-04T15:33:43.65Z",
    "id": "/subscriptions/<subscription ID>/resourceGroups/<resource group>/providers/Microsoft.Compute/virtualMachines/<resource name>/events/a80024e1-883d-42a5-8b01-7591a1befccb/ticks/636716720236500000",
    "level": "Critical",
    "operationId": "",
    "operationName": {
        "value": "Microsoft.Resourcehealth/healthevent/Activated/action",
        "localizedValue": "Health Event Activated"
    },
    "resourceGroupName": "<resource group>",
    "resourceProviderName": {
        "value": "Microsoft.Resourcehealth/healthevent/action",
        "localizedValue": "Microsoft.Resourcehealth/healthevent/action"
    },
    "resourceType": {
        "value": "Microsoft.Compute/virtualMachines",
        "localizedValue": "Microsoft.Compute/virtualMachines"
    },
    "resourceId": "/subscriptions/<subscription ID>/resourceGroups/<resource group>/providers/Microsoft.Compute/virtualMachines/<resource name>",
    "status": {
        "value": "Active",
        "localizedValue": "Active"
    },
    "subStatus": {
        "value": "",
        "localizedValue": ""
    },
    "submissionTimestamp": "2018-09-04T15:36:24.2240867Z",
    "subscriptionId": "<subscription ID>",
    "properties": {
        "stage": "Active",
        "title": "Virtual Machine health status changed to unavailable",
        "details": "Virtual machine has experienced an unexpected event",
        "healthStatus": "Unavailable",
        "healthEventType": "Downtime",
        "healthEventCause": "PlatformInitiated",
        "healthEventCategory": "Unplanned"
    },
    "relatedEvents": []
}
```

### <a name="property-descriptions"></a>Özellik açıklamaları
| Öğe Adı | Açıklama |
| --- | --- |
| lardan | Her zaman "Yönetici, Işlem" |
| correlationId | Dize biçimindeki bir GUID. |
| açıklama |Uyarı olayının statik metin açıklaması. |
| Eventdataıd |Uyarı olayının benzersiz tanımlayıcısı. |
| category | Her zaman "ResourceHealth" |
| eventTimestamp |Olayın, olaya karşılık gelen isteği işleyen Azure hizmeti tarafından oluşturulduğu zaman damgası. |
| düzey |Etkinliğin düzeyi. Şu değerlerden biri: "kritik", "hata", "uyarı", "bilgilendirme" ve "verbose" |
| operationId |Tek bir işleme karşılık gelen olaylar arasında paylaşılan bir GUID. |
| operationName |İşlemin adı. |
| resourceGroupName |Kaynağı içeren kaynak grubunun adı. |
| resourceProviderName |Her zaman "Microsoft. Resourcehealth/healthevent/Action". |
| resourceType | Kaynak Durumu olayından etkilenen kaynak türü. |
| resourceId | Etkilenen kaynağın kaynak KIMLIĞI adı. |
| durum |Sistem durumu olayının durumunu açıklayan dize. Değerler şu olabilir: etkin, çözümlenmiş, sürüyor, güncelleştirildi. |
| Dosya | Genellikle uyarılar için null. |
| submissionTimestamp |Olay sorgulama için kullanılabilir hale geldiğinde zaman damgası. |
| subscriptionId |Azure abonelik KIMLIĞI. |
| properties |Olayın ayrıntılarını `<Key, Value>` açıklayan çiftler (yani bir sözlük) kümesi.|
| Properties. title | Kaynağın sistem durumunu açıklayan, Kullanıcı dostu bir dize. |
| Özellikler. Ayrıntılar | Olayla ilgili diğer ayrıntıları açıklayan, Kullanıcı dostu bir dize. |
| Properties. currentHealthStatus | Kaynağın geçerli sistem durumu. Şu değerlerden biri: "kullanılabilir", "kullanılamıyor", "düşürülmüş" ve "Unknown". |
| Properties. previousHealthStatus | Kaynağın önceki sistem durumu. Şu değerlerden biri: "kullanılabilir", "kullanılamıyor", "düşürülmüş" ve "Unknown". |
| Properties. Type | Kaynak sistem durumu olayı türünün açıklaması. |
| Özellikler. neden | Kaynak sistem durumu olayının nedeninin açıklaması. "Userınitialize" ve "Platformıbınated". |


## <a name="alert"></a>Uyarı
Bu kategori, tüm Azure uyarı etkinleştirmeleri kaydını içerir. Bu kategoride göreceğiniz olay türüne bir örnek, son 5 dakika boyunca myVM 'deki% CPU %80 üzerinde. " Çeşitli Azure sistemlerinde bir uyarı kavramı vardır; bir sıralama kuralı tanımlayabilir ve koşullar bu kuralla eşleşiyorsa bildirim alabilirsiniz. Desteklenen bir Azure uyarı türü ' etkinleşdiğinde ' her seferinde veya bir bildirim oluşturmak için koşullar karşılandığında, etkinleştirme kaydı da etkinlik günlüğünün bu kategorisine gönderilir.

### <a name="sample-event"></a>Örnek olay

```json
{
  "caller": "Microsoft.Insights/alertRules",
  "channels": "Admin, Operation",
  "claims": {
    "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/spn": "Microsoft.Insights/alertRules"
  },
  "correlationId": "/subscriptions/<subscription ID>/resourceGroups/myResourceGroup/providers/microsoft.insights/alertrules/myalert/incidents/L3N1YnNjcmlwdGlvbnMvZGY2MDJjOWMtN2FhMC00MDdkLWE2ZmItZWIyMGM4YmQxMTkyL3Jlc291cmNlR3JvdXBzL0NzbUV2ZW50RE9HRk9PRC1XZXN0VVMvcHJvdmlkZXJzL21pY3Jvc29mdC5pbnNpZ2h0cy9hbGVydHJ1bGVzL215YWxlcnQwNjM2MzYyMjU4NTM1MjIxOTIw",
  "description": "'Disk read LessThan 100000 ([Count]) in the last 5 minutes' has been resolved for CloudService: myResourceGroup/Production/Event.BackgroundJobsWorker.razzle (myResourceGroup)",
  "eventDataId": "149d4baf-53dc-4cf4-9e29-17de37405cd9",
  "eventName": {
    "value": "Alert",
    "localizedValue": "Alert"
  },
  "category": {
    "value": "Alert",
    "localizedValue": "Alert"
  },
  "id": "/subscriptions/<subscription ID>/resourceGroups/myResourceGroup/providers/Microsoft.ClassicCompute/domainNames/myResourceGroup/slots/Production/roles/Event.BackgroundJobsWorker.razzle/events/149d4baf-53dc-4cf4-9e29-17de37405cd9/ticks/636362258535221920",
  "level": "Informational",
  "resourceGroupName": "myResourceGroup",
  "resourceProviderName": {
    "value": "Microsoft.ClassicCompute",
    "localizedValue": "Microsoft.ClassicCompute"
  },
  "resourceId": "/subscriptions/<subscription ID>/resourceGroups/myResourceGroup/providers/Microsoft.ClassicCompute/domainNames/myResourceGroup/slots/Production/roles/Event.BackgroundJobsWorker.razzle",
  "resourceType": {
    "value": "Microsoft.ClassicCompute/domainNames/slots/roles",
    "localizedValue": "Microsoft.ClassicCompute/domainNames/slots/roles"
  },
  "operationId": "/subscriptions/<subscription ID>/resourceGroups/myResourceGroup/providers/microsoft.insights/alertrules/myalert/incidents/L3N1YnNjcmlwdGlvbnMvZGY2MDJjOWMtN2FhMC00MDdkLWE2ZmItZWIyMGM4YmQxMTkyL3Jlc291cmNlR3JvdXBzL0NzbUV2ZW50RE9HRk9PRC1XZXN0VVMvcHJvdmlkZXJzL21pY3Jvc29mdC5pbnNpZ2h0cy9hbGVydHJ1bGVzL215YWxlcnQwNjM2MzYyMjU4NTM1MjIxOTIw",
  "operationName": {
    "value": "Microsoft.Insights/AlertRules/Resolved/Action",
    "localizedValue": "Microsoft.Insights/AlertRules/Resolved/Action"
  },
  "properties": {
    "RuleUri": "/subscriptions/<subscription ID>/resourceGroups/myResourceGroup/providers/microsoft.insights/alertrules/myalert",
    "RuleName": "myalert",
    "RuleDescription": "",
    "Threshold": "100000",
    "WindowSizeInMinutes": "5",
    "Aggregation": "Average",
    "Operator": "LessThan",
    "MetricName": "Disk read",
    "MetricUnit": "Count"
  },
  "status": {
    "value": "Resolved",
    "localizedValue": "Resolved"
  },
  "subStatus": {
    "value": null
  },
  "eventTimestamp": "2017-07-21T09:24:13.522192Z",
  "submissionTimestamp": "2017-07-21T09:24:15.6578651Z",
  "subscriptionId": "<subscription ID>"
}
```

### <a name="property-descriptions"></a>Özellik açıklamaları
| Öğe Adı | Açıklama |
| --- | --- |
| yapana | Her zaman Microsoft. Insights/alertRules |
| lardan | Her zaman "Yönetici, Işlem" |
| belirt | Uyarı altyapısının SPN (hizmet sorumlusu adı) veya kaynak türü ile JSON blobu. |
| correlationId | Dize biçimindeki bir GUID. |
| açıklama |Uyarı olayının statik metin açıklaması. |
| Eventdataıd |Uyarı olayının benzersiz tanımlayıcısı. |
| category | Her zaman "uyar" |
| düzey |Etkinliğin düzeyi. Şu değerlerden biri: "kritik", "hata", "uyarı" ve "bilgilendirme" |
| resourceGroupName |Bir ölçüm uyarısında etkilenen kaynağın kaynak grubunun adı. Diğer uyarı türleri için, uyarının kendisini içeren kaynak grubunun adıdır. |
| resourceProviderName |Etkilenen kaynağın ölçüm uyarısında kaynak sağlayıcısının adı. Diğer uyarı türleri için, bu, uyarının kendisi için kaynak sağlayıcısının adıdır. |
| resourceId | Bir ölçüm uyarısında etkilenen kaynağın kaynak KIMLIĞI adı. Diğer uyarı türleri için, uyarı kaynağının kendisi kaynak KIMLIĞIDIR. |
| operationId |Tek bir işleme karşılık gelen olaylar arasında paylaşılan bir GUID. |
| operationName |İşlemin adı. |
| properties |Olayın ayrıntılarını `<Key, Value>` açıklayan çiftler (yani bir sözlük) kümesi. |
| durum |İşlemin durumunu açıklayan dize. Bazı ortak değerler şunlardır: başlatıldı, devam ediyor, başarılı, başarısız, etkin, Çözümlenmiş. |
| Dosya | Genellikle uyarılar için null. |
| eventTimestamp |Olayın, olaya karşılık gelen isteği işleyen Azure hizmeti tarafından oluşturulduğu zaman damgası. |
| submissionTimestamp |Olay sorgulama için kullanılabilir hale geldiğinde zaman damgası. |
| subscriptionId |Azure abonelik KIMLIĞI. |

### <a name="properties-field-per-alert-type"></a>Uyarı türü başına Özellikler alanı
Özellikler alanı, uyarı olayının kaynağına göre farklı değerler içerecektir. İki ortak uyarı olay sağlayıcısı, etkinlik günlüğü uyarıları ve ölçüm uyarılarıdır.

#### <a name="properties-for-activity-log-alerts"></a>Etkinlik günlüğü uyarıları özellikleri
| Öğe Adı | Açıklama |
| --- | --- |
| Properties. SubscriptionID | Etkinlik günlüğü olayından bu etkinlik günlüğü uyarı kuralının etkinleştirilmesini sağlayan abonelik KIMLIĞI. |
| Properties. Eventdataıd | Etkinlik günlüğü olayından bu etkinlik günlüğü uyarı kuralının etkinleştirilmesini sağlayan olay veri KIMLIĞI. |
| Properties. resourceGroup | Etkinlik günlüğü olayından bu etkinlik günlüğü uyarı kuralının etkinleştirilmesini sağlayan kaynak grubu. |
| Properties. ResourceID | Etkinlik günlüğü olayından bu etkinlik günlüğü uyarı kuralının etkinleştirilmesini sağlayan kaynak KIMLIĞI. |
| Properties. eventTimestamp | Bu etkinlik günlüğü uyarı kuralının etkinleştirilmesini sağlayan etkinlik günlüğü olayının olay zaman damgası. |
| Properties. operationName | Etkinlik günlüğü olayından bu etkinlik günlüğü uyarı kuralının etkinleştirilmesini sağlayan işlem adı. |
| Properties. Status | Etkinlik günlüğü olayından bu etkinlik günlüğü uyarı kuralının etkinleştirilmesini sağlayan durum.|

#### <a name="properties-for-metric-alerts"></a>Ölçüm uyarıları özellikleri
| Öğe Adı | Açıklama |
| --- | --- |
| özelliklerinin. RuleUri | Ölçüm uyarı kuralının kaynak KIMLIĞI. |
| özelliklerinin. RuleName | Ölçüm uyarı kuralının adı. |
| özelliklerinin. RuleDescription | Ölçüm uyarı kuralının açıklaması (uyarı kuralında tanımlandığı gibi). |
| özelliklerinin. Eşiği | Ölçüm uyarı kuralının değerlendirmesinde kullanılan eşik değeri. |
| özelliklerinin. Windowsizeınminutes | Ölçüm uyarı kuralının değerlendirmesinde kullanılan pencere boyutu. |
| özelliklerinin. Toplama | Ölçüm uyarı kuralında tanımlanan toplama türü. |
| özelliklerinin. İşlecinde | Ölçüm uyarı kuralının değerlendirmesinde kullanılan koşullu işleç. |
| özelliklerinin. MetricName | Ölçüm uyarı kuralının değerlendirmesinde kullanılan ölçümün ölçüm adı. |
| özelliklerinin. MetricUnit | Ölçüm uyarı kuralının değerlendirmesinde kullanılan ölçüm için ölçüm birimi. |

## <a name="autoscale"></a>Otomatik Ölçeklendirme
Bu kategori, aboneliğinizde tanımladığınız otomatik ölçeklendirme ayarlarına bağlı olarak otomatik ölçeklendirme altyapısının işlemiyle ilgili olayların kaydını içerir. Bu kategoride göreceğiniz olay türüne bir örnek, "otomatik ölçeklendirme ölçeği artırma eylemi başarısız oldu." Otomatik ölçeklendirme kullanarak, desteklenen bir kaynak türündeki örnek sayısını, gün ve/veya yükleme (ölçüm) verilerine göre otomatik ölçeklendirme ayarı kullanarak otomatik olarak ölçeklendirebilir veya ölçeklendirebilirsiniz. Koşulların ölçeği artırma veya azaltma için karşılandığında, başlangıç ve başarılı veya başarısız olaylar bu kategoriye kaydedilir.

### <a name="sample-event"></a>Örnek olay
```json
{
  "caller": "Microsoft.Insights/autoscaleSettings",
  "channels": "Admin, Operation",
  "claims": {
    "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/spn": "Microsoft.Insights/autoscaleSettings"
  },
  "correlationId": "fc6a7ff5-ff68-4bb7-81b4-3629212d03d0",
  "description": "The autoscale engine attempting to scale resource '/subscriptions/<subscription ID>/resourceGroups/myResourceGroup/providers/Microsoft.ClassicCompute/domainNames/myResourceGroup/slots/Production/roles/myResource' from 3 instances count to 2 instances count.",
  "eventDataId": "a5b92075-1de9-42f1-b52e-6f3e4945a7c7",
  "eventName": {
    "value": "AutoscaleAction",
    "localizedValue": "AutoscaleAction"
  },
  "category": {
    "value": "Autoscale",
    "localizedValue": "Autoscale"
  },
  "id": "/subscriptions/<subscription ID>/resourceGroups/myResourceGroup/providers/microsoft.insights/autoscalesettings/myResourceGroup-Production-myResource-myResourceGroup/events/a5b92075-1de9-42f1-b52e-6f3e4945a7c7/ticks/636361956518681572",
  "level": "Informational",
  "resourceGroupName": "myResourceGroup",
  "resourceProviderName": {
    "value": "microsoft.insights",
    "localizedValue": "microsoft.insights"
  },
  "resourceId": "/subscriptions/<subscription ID>/resourceGroups/myResourceGroup/providers/microsoft.insights/autoscalesettings/myResourceGroup-Production-myResource-myResourceGroup",
  "resourceType": {
    "value": "microsoft.insights/autoscalesettings",
    "localizedValue": "microsoft.insights/autoscalesettings"
  },
  "operationId": "fc6a7ff5-ff68-4bb7-81b4-3629212d03d0",
  "operationName": {
    "value": "Microsoft.Insights/AutoscaleSettings/Scaledown/Action",
    "localizedValue": "Microsoft.Insights/AutoscaleSettings/Scaledown/Action"
  },
  "properties": {
    "Description": "The autoscale engine attempting to scale resource '/subscriptions/<subscription ID>/resourceGroups/myResourceGroup/providers/Microsoft.ClassicCompute/domainNames/myResourceGroup/slots/Production/roles/myResource' from 3 instances count to 2 instances count.",
    "ResourceName": "/subscriptions/<subscription ID>/resourceGroups/myResourceGroup/providers/Microsoft.ClassicCompute/domainNames/myResourceGroup/slots/Production/roles/myResource",
    "OldInstancesCount": "3",
    "NewInstancesCount": "2",
    "LastScaleActionTime": "Fri, 21 Jul 2017 01:00:51 GMT"
  },
  "status": {
    "value": "Succeeded",
    "localizedValue": "Succeeded"
  },
  "subStatus": {
    "value": null
  },
  "eventTimestamp": "2017-07-21T01:00:51.8681572Z",
  "submissionTimestamp": "2017-07-21T01:00:52.3008754Z",
  "subscriptionId": "<subscription ID>"
}

```

### <a name="property-descriptions"></a>Özellik açıklamaları
| Öğe Adı | Açıklama |
| --- | --- |
| yapana | Her zaman Microsoft. Insights/oto Scalesettings |
| lardan | Her zaman "Yönetici, Işlem" |
| belirt | Otomatik ölçeklendirme altyapısının SPN (hizmet sorumlusu adı) veya kaynak türü ile JSON blobu. |
| correlationId | Dize biçimindeki bir GUID. |
| açıklama |Otomatik ölçeklendirme olayının statik metin açıklaması. |
| Eventdataıd |Otomatik ölçeklendirme olayının benzersiz tanıtıcısı. |
| düzey |Etkinliğin düzeyi. Şu değerlerden biri: "kritik", "hata", "uyarı" ve "bilgilendirme" |
| resourceGroupName |Otomatik ölçeklendirme ayarı için kaynak grubunun adı. |
| resourceProviderName |Otomatik ölçeklendirme ayarı için kaynak sağlayıcının adı. |
| resourceId |Otomatik ölçeklendirme ayarının kaynak KIMLIĞI. |
| operationId |Tek bir işleme karşılık gelen olaylar arasında paylaşılan bir GUID. |
| operationName |İşlemin adı. |
| properties |Olayın ayrıntılarını `<Key, Value>` açıklayan çiftler (yani bir sözlük) kümesi. |
| özelliklerinin. Açıklaması | Otomatik ölçeklendirme motorunun yaptığı işlemin ayrıntılı açıklaması. |
| özelliklerinin. Kaynak | Etkilenen kaynağın kaynak KIMLIĞI (ölçek eyleminin gerçekleştirildiği kaynak) |
| özelliklerinin. Oldınstancescount | Otomatik ölçeklendirme eylemi yürürlüğe girmeden önce örneklerin sayısı. |
| özelliklerinin. NewInstancesCount | Otomatik ölçeklendirme eyleminden sonraki örnek sayısı. |
| özelliklerinin. LastScaleActionTime | Otomatik ölçeklendirme eyleminin gerçekleştiği zaman damgası. |
| durum |İşlemin durumunu açıklayan dize. Bazı ortak değerler şunlardır: başlatıldı, devam ediyor, başarılı, başarısız, etkin, Çözümlenmiş. |
| Dosya | Otomatik ölçeklendirme için genellikle null. |
| eventTimestamp |Olayın, olaya karşılık gelen isteği işleyen Azure hizmeti tarafından oluşturulduğu zaman damgası. |
| submissionTimestamp |Olay sorgulama için kullanılabilir hale geldiğinde zaman damgası. |
| subscriptionId |Azure abonelik KIMLIĞI. |

## <a name="security"></a>Güvenlik
Bu kategori, Azure Güvenlik Merkezi tarafından oluşturulan uyarıların kaydını içerir. Bu kategoride göreceğiniz olay türüne bir örnek, "şüpheli çift uzantı dosyası yürütüldü."

### <a name="sample-event"></a>Örnek olay
```json
{
    "channels": "Operation",
    "correlationId": "965d6c6a-a790-4a7e-8e9a-41771b3fbc38",
    "description": "Suspicious double extension file executed. Machine logs indicate an execution of a process with a suspicious double extension.\r\nThis extension may trick users into thinking files are safe to be opened and might indicate the presence of malware on the system.",
    "eventDataId": "965d6c6a-a790-4a7e-8e9a-41771b3fbc38",
    "eventName": {
        "value": "Suspicious double extension file executed",
        "localizedValue": "Suspicious double extension file executed"
    },
    "category": {
        "value": "Security",
        "localizedValue": "Security"
    },
    "eventTimestamp": "2017-10-18T06:02:18.6179339Z",
    "id": "/subscriptions/<subscription ID>/providers/Microsoft.Security/locations/centralus/alerts/965d6c6a-a790-4a7e-8e9a-41771b3fbc38/events/965d6c6a-a790-4a7e-8e9a-41771b3fbc38/ticks/636439033386179339",
    "level": "Informational",
    "operationId": "965d6c6a-a790-4a7e-8e9a-41771b3fbc38",
    "operationName": {
        "value": "Microsoft.Security/locations/alerts/activate/action",
        "localizedValue": "Microsoft.Security/locations/alerts/activate/action"
    },
    "resourceGroupName": "myResourceGroup",
    "resourceProviderName": {
        "value": "Microsoft.Security",
        "localizedValue": "Microsoft.Security"
    },
    "resourceType": {
        "value": "Microsoft.Security/locations/alerts",
        "localizedValue": "Microsoft.Security/locations/alerts"
    },
    "resourceId": "/subscriptions/<subscription ID>/providers/Microsoft.Security/locations/centralus/alerts/2518939942613820660_a48f8653-3fc6-4166-9f19-914f030a13d3",
    "status": {
        "value": "Active",
        "localizedValue": "Active"
    },
    "subStatus": {
        "value": null
    },
    "submissionTimestamp": "2017-10-18T06:02:52.2176969Z",
    "subscriptionId": "<subscription ID>",
    "properties": {
        "accountLogonId": "0x2r4",
        "commandLine": "c:\\mydirectory\\doubleetension.pdf.exe",
        "domainName": "hpc",
        "parentProcess": "unknown",
        "parentProcess id": "0",
        "processId": "6988",
        "processName": "c:\\mydirectory\\doubleetension.pdf.exe",
        "userName": "myUser",
        "UserSID": "S-3-2-12",
        "ActionTaken": "Detected",
        "Severity": "High"
    },
    "relatedEvents": []
}

```

### <a name="property-descriptions"></a>Özellik açıklamaları
| Öğe Adı | Açıklama |
| --- | --- |
| lardan | Always "Işlem" |
| correlationId | Dize biçimindeki bir GUID. |
| açıklama |Güvenlik olayının statik metin açıklaması. |
| Eventdataıd |Güvenlik olayının benzersiz tanımlayıcısı. |
| eventName |Güvenlik olayının kolay adı. |
| category | Always "güvenlik" |
| Kimlik |Güvenlik olayının benzersiz kaynak tanımlayıcısı. |
| düzey |Etkinliğin düzeyi. Şu değerlerden biri: "kritik", "hata", "uyarı" veya "bilgilendirme" |
| resourceGroupName |Kaynak için kaynak grubunun adı. |
| resourceProviderName |Azure Güvenlik Merkezi için kaynak sağlayıcısının adı. Her zaman "Microsoft. Security". |
| resourceType |"Microsoft. Security/Locations/Alerts" gibi güvenlik olayını oluşturan kaynak türü |
| resourceId |Güvenlik uyarısının kaynak KIMLIĞI. |
| operationId |Tek bir işleme karşılık gelen olaylar arasında paylaşılan bir GUID. |
| operationName |İşlemin adı. |
| properties |Olayın ayrıntılarını `<Key, Value>` açıklayan çiftler (yani bir sözlük) kümesi. Bu özellikler, güvenlik uyarısı türüne göre değişir. Güvenlik Merkezi 'nden gelen uyarı türlerinin açıklaması için [Bu sayfaya](../../security-center/security-center-alerts-overview.md) bakın. |
| özelliklerinin. İnin |Önem düzeyi. Olası değerler şunlardır "yüksek," "Orta" veya "düşük". |
| durum |İşlemin durumunu açıklayan dize. Bazı ortak değerler şunlardır: başlatıldı, devam ediyor, başarılı, başarısız, etkin, Çözümlenmiş. |
| Dosya | Güvenlik olayları için genellikle null. |
| eventTimestamp |Olayın, olaya karşılık gelen isteği işleyen Azure hizmeti tarafından oluşturulduğu zaman damgası. |
| submissionTimestamp |Olay sorgulama için kullanılabilir hale geldiğinde zaman damgası. |
| subscriptionId |Azure abonelik KIMLIĞI. |

## <a name="recommendation"></a>Öneri
Bu kategori, hizmetleriniz için oluşturulan tüm yeni önerilerin kaydını içerir. Önerinin bir örneği "iyileştirilmiş hata toleransı için kullanılabilirlik kümeleri kullanın" olacaktır. Oluşturulabilecek dört öneri olayı türü vardır: yüksek kullanılabilirlik, performans, güvenlik ve maliyet Iyileştirmesi. 

### <a name="sample-event"></a>Örnek olay
```json
{
    "channels": "Operation",
    "correlationId": "92481dfd-c5bf-4752-b0d6-0ecddaa64776",
    "description": "The action was successful.",
    "eventDataId": "06cb0e44-111b-47c7-a4f2-aa3ee320c9c5",
    "eventName": {
        "value": "",
        "localizedValue": ""
    },
    "category": {
        "value": "Recommendation",
        "localizedValue": "Recommendation"
    },
    "eventTimestamp": "2018-06-07T21:30:42.976919Z",
    "id": "/SUBSCRIPTIONS/<Subscription ID>/RESOURCEGROUPS/MYRESOURCEGROUP/PROVIDERS/MICROSOFT.COMPUTE/VIRTUALMACHINES/MYVM/events/06cb0e44-111b-47c7-a4f2-aa3ee320c9c5/ticks/636640038429769190",
    "level": "Informational",
    "operationId": "",
    "operationName": {
        "value": "Microsoft.Advisor/generateRecommendations/action",
        "localizedValue": "Microsoft.Advisor/generateRecommendations/action"
    },
    "resourceGroupName": "MYRESOURCEGROUP",
    "resourceProviderName": {
        "value": "MICROSOFT.COMPUTE",
        "localizedValue": "MICROSOFT.COMPUTE"
    },
    "resourceType": {
        "value": "MICROSOFT.COMPUTE/virtualmachines",
        "localizedValue": "MICROSOFT.COMPUTE/virtualmachines"
    },
    "resourceId": "/SUBSCRIPTIONS/<Subscription ID>/RESOURCEGROUPS/MYRESOURCEGROUP/PROVIDERS/MICROSOFT.COMPUTE/VIRTUALMACHINES/MYVM",
    "status": {
        "value": "Active",
        "localizedValue": "Active"
    },
    "subStatus": {
        "value": "",
        "localizedValue": ""
    },
    "submissionTimestamp": "2018-06-07T21:30:42.976919Z",
    "subscriptionId": "<Subscription ID>",
    "properties": {
        "recommendationSchemaVersion": "1.0",
        "recommendationCategory": "Security",
        "recommendationImpact": "High",
        "recommendationRisk": "None"
    },
    "relatedEvents": []
}

```
### <a name="property-descriptions"></a>Özellik açıklamaları
| Öğe Adı | Açıklama |
| --- | --- |
| lardan | Always "Işlem" |
| correlationId | Dize biçimindeki bir GUID. |
| açıklama |Öneri olayının statik metin açıklaması |
| Eventdataıd | Öneri olayının benzersiz tanımlayıcısı. |
| category | Her zaman "öneri" |
| Kimlik |Öneri olayının benzersiz kaynak tanımlayıcısı. |
| düzey |Etkinliğin düzeyi. Şu değerlerden biri: "kritik", "hata", "uyarı" veya "bilgilendirme" |
| operationName |İşlemin adı.  Always "Microsoft. Advisor/Generatereyorumları/eylem"|
| resourceGroupName |Kaynak için kaynak grubunun adı. |
| resourceProviderName |Bu önerinin uygulandığı kaynak için kaynak sağlayıcının adı, örneğin "MICROSOFT. COMPUTE" |
| resourceType |Bu önerinin uygulandığı kaynak için kaynak türünün adı, örneğin "MICROSOFT. COMPUTE/virtualmachines" |
| resourceId |Önerinin uygulandığı kaynağın kaynak KIMLIĞI |
| durum | Always "etkin" |
| submissionTimestamp |Olay sorgulama için kullanılabilir hale geldiğinde zaman damgası. |
| subscriptionId |Azure abonelik KIMLIĞI. |
| properties |Önerinin `<Key, Value>` ayrıntılarını açıklayan çiftler (yani bir sözlük) kümesi.|
| Properties. recommendationSchemaVersion| Etkinlik günlüğü girişinde yayınlanan öneri özelliklerinin şema sürümü |
| Properties. recommendationCategory | Önerinin kategorisi. Olası değerler şunlardır "yüksek kullanılabilirlik", "performans", "güvenlik" ve "maliyet" |
| Properties. recommendationImpact| Önerinin etkisi. Olası değerler şunlardır "yüksek", "Orta", "düşük" |
| Properties. recommendationRisk| Önerinin riski. Olası değerler şunlardır "hata", "uyarı", "hiçbiri" |

## <a name="policy"></a>İlke

Bu kategori, [Azure ilkesi](../../governance/policy/overview.md)tarafından gerçekleştirilen tüm etki eylemi işlemlerinin kayıtlarını içerir. Bu kategoride göreceğiniz olay türlerine örnek olarak _Denetim_ ve _reddetme_dahildir. Ilke tarafından gerçekleştirilen her eylem, bir kaynaktaki işlem olarak modellenir.

### <a name="sample-policy-event"></a>Örnek Ilke olayı

```json
{
    "authorization": {
        "action": "Microsoft.Resources/checkPolicyCompliance/read",
        "scope": "/subscriptions/<subscriptionID>"
    },
    "caller": "33a68b9d-63ce-484c-a97e-94aef4c89648",
    "channels": "Operation",
    "claims": {
        "aud": "https://management.azure.com/",
        "iss": "https://sts.windows.net/1114444b-7467-4144-a616-e3a5d63e147b/",
        "iat": "1234567890",
        "nbf": "1234567890",
        "exp": "1234567890",
        "aio": "A3GgTJdwK4vy7Fa7l6DgJC2mI0GX44tML385OpU1Q+z+jaPnFMwB",
        "appid": "1d78a85d-813d-46f0-b496-dd72f50a3ec0",
        "appidacr": "2",
        "http://schemas.microsoft.com/identity/claims/identityprovider": "https://sts.windows.net/1114444b-7467-4144-a616-e3a5d63e147b/",
        "http://schemas.microsoft.com/identity/claims/objectidentifier": "f409edeb-4d29-44b5-9763-ee9348ad91bb",
        "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier": "b-24Jf94A3FH2sHWVIFqO3-RSJEiv24Jnif3gj7s",
        "http://schemas.microsoft.com/identity/claims/tenantid": "1114444b-7467-4144-a616-e3a5d63e147b",
        "uti": "IdP3SUJGtkGlt7dDQVRPAA",
        "ver": "1.0"
    },
    "correlationId": "b5768deb-836b-41cc-803e-3f4de2f9e40b",
    "description": "",
    "eventDataId": "d0d36f97-b29c-4cd9-9d3d-ea2b92af3e9d",
    "eventName": {
        "value": "EndRequest",
        "localizedValue": "End request"
    },
    "category": {
        "value": "Policy",
        "localizedValue": "Policy"
    },
    "eventTimestamp": "2019-01-15T13:19:56.1227642Z",
    "id": "/subscriptions/<subscriptionID>/resourceGroups/myResourceGroup/providers/Microsoft.Sql/servers/contososqlpolicy/events/13bbf75f-36d5-4e66-b693-725267ff21ce/ticks/636831551961227642",
    "level": "Warning",
    "operationId": "04e575f8-48d0-4c43-a8b3-78c4eb01d287",
    "operationName": {
        "value": "Microsoft.Authorization/policies/audit/action",
        "localizedValue": "Microsoft.Authorization/policies/audit/action"
    },
    "resourceGroupName": "myResourceGroup",
    "resourceProviderName": {
        "value": "Microsoft.Sql",
        "localizedValue": "Microsoft SQL"
    },
    "resourceType": {
        "value": "Microsoft.Resources/checkPolicyCompliance",
        "localizedValue": "Microsoft.Resources/checkPolicyCompliance"
    },
    "resourceId": "/subscriptions/<subscriptionID>/resourceGroups/myResourceGroup/providers/Microsoft.Sql/servers/contososqlpolicy",
    "status": {
        "value": "Succeeded",
        "localizedValue": "Succeeded"
    },
    "subStatus": {
        "value": "",
        "localizedValue": ""
    },
    "submissionTimestamp": "2019-01-15T13:20:17.1077672Z",
    "subscriptionId": "<subscriptionID>",
    "properties": {
        "isComplianceCheck": "True",
        "resourceLocation": "westus2",
        "ancestors": "72f988bf-86f1-41af-91ab-2d7cd011db47",
        "policies": "[{\"policyDefinitionId\":\"/subscriptions/<subscriptionID>/providers/Microsoft.
            Authorization/policyDefinitions/5775cdd5-d3d3-47bf-bc55-bb8b61746506/\",\"policyDefiniti
            onName\":\"5775cdd5-d3d3-47bf-bc55-bb8b61746506\",\"policyDefinitionEffect\":\"Deny\",\"
            policyAssignmentId\":\"/subscriptions/<subscriptionID>/providers/Microsoft.Authorization
            /policyAssignments/991a69402a6c484cb0f9b673/\",\"policyAssignmentName\":\"991a69402a6c48
            4cb0f9b673\",\"policyAssignmentScope\":\"/subscriptions/<subscriptionID>\",\"policyAssig
            nmentParameters\":{}}]"
    },
    "relatedEvents": []
}
```

### <a name="policy-event-property-descriptions"></a>İlke olayı Özellik açıklamaları

| Öğe Adı | Açıklama |
| --- | --- |
| yetkilendirme | Etkinliğin RBAC özelliklerinin dizisi. Yeni kaynaklar için bu işlem, değerlendirmeyi tetikleyen isteğin bir sonucudur. Mevcut kaynaklar için, eylem "Microsoft. resources/Checkpolicyuyumluluk/Read" olur. |
| yapana | Yeni kaynaklar için, bir dağıtımı Başlatan kimlik. Mevcut kaynaklar için Microsoft Azure Policy Insights RP 'nin GUID 'SI. |
| lardan | İlke olayları yalnızca "Işlem" kanalını kullanır. |
| belirt | Kaynak Yöneticisi içinde bu işlemi gerçekleştirmek için kullanıcının veya uygulamanın kimliğini doğrulamak üzere Active Directory tarafından kullanılan JWT belirteci. |
| correlationId | Genellikle dize biçimindeki bir GUID. Bir CorrelationId 'yi paylaşan olaylar aynı Uber eylemine aittir. |
| açıklama | Bu alan, Ilke olayları için boştur. |
| Eventdataıd | Bir olayın benzersiz tanımlayıcısı. |
| eventName | "BeginRequest" ya da "EndRequest". "BeginRequest" Gecikmeli Auditınotexists ve deployIfNotExists değerlendirmeleri için ve bir deployIfNotExists efekti bir şablon dağıtımı başlattığında kullanılır. Tüm diğer işlemler "EndRequest" döndürür. |
| category | Etkinlik günlüğü olayını "Ilkeye" ait olarak bildirir. |
| eventTimestamp | Olayın, olaya karşılık gelen isteği işleyen Azure hizmeti tarafından oluşturulduğu zaman damgası. |
| Kimlik | Belirli bir kaynaktaki etkinliğin benzersiz tanımlayıcısı. |
| düzey | Etkinliğin düzeyi. Denetim "uyarı" kullanır ve reddetme "hata" kullanır. Bir Auditınotexists veya deployIfNotExists hatası, önem derecesine bağlı olarak "uyarı" veya "hata" oluşturabilir. Diğer tüm Ilke olayları "bilgilendirme" kullanır. |
| operationId | Tek bir işleme karşılık gelen olaylar arasında paylaşılan bir GUID. |
| operationName | İşlemin adı ve doğrudan Ilke efektiyle ilişkili. |
| resourceGroupName | Değerlendirilen kaynak için kaynak grubunun adı. |
| resourceProviderName | Değerlendirilen kaynak için kaynak sağlayıcının adı. |
| resourceType | Yeni kaynaklar için, değerlendirilen türdür. Mevcut kaynaklar için "Microsoft. resources/Checkpolicyuyumluluğu" döndürür. |
| resourceId | Değerlendirilen kaynağın kaynak KIMLIĞI. |
| durum | Ilke değerlendirme sonucunun durumunu açıklayan dize. Çoğu Ilke değerlendirmesi "başarılı" olarak döndürülür, ancak reddetme efekti "başarısız" döndürür. Auditınotexists veya deployIfNotExists hataları da "başarısız" döndürüyor. |
| Dosya | Alan, Ilke olayları için boştur. |
| submissionTimestamp | Olay sorgulama için kullanılabilir hale geldiğinde zaman damgası. |
| subscriptionId | Azure abonelik KIMLIĞI. |
| Properties. ıskarmaşık\denetim | Yeni bir kaynak dağıtıldığında veya mevcut bir kaynağın Kaynak Yöneticisi özellikleri güncelleştirilirken "false" döndürür. Diğer tüm [değerlendirme Tetikleyicileri](../../governance/policy/how-to/get-compliance-data.md#evaluation-triggers) "true" ile sonuçlanır. |
| Properties. resourceLocation | Değerlendirilen kaynağın Azure bölgesi. |
| Properties. öncüleri | Doğrudan üst öğeden en uzak en üst öğeye sıralanmış üst yönetim gruplarının virgülle ayrılmış listesi. |
| Properties. Policies | İlke tanımı, atama, efekt ve bu Ilke değerlendirmesinin sonucu olan parametrelerle ilgili ayrıntıları içerir. |
| relatedEvents | Bu alan, Ilke olayları için boştur. |


## <a name="schema-from-storage-account-and-event-hubs"></a>Depolama hesabı ve Olay Hub 'larının şeması
Azure etkinlik günlüğü 'nü bir depolama hesabına veya Olay Hub 'ına akışta, veriler [kaynak günlüğü şemasını](diagnostic-logs-schema.md)izler. Aşağıdaki tabloda, yukarıdaki şemadan kaynak günlükleri şemasına yönelik özelliklerin bir eşlemesi verilmiştir.

> [!IMPORTANT]
> Bir depolama hesabına yazılan etkinlik günlüğü verilerinin biçimi, Kasım. 1, 2018 tarihinde JSON satırları olarak değiştirildi. Bu biçim değişikliğine ilişkin ayrıntılar için bkz. [Azure izleyici kaynak günlüklerinde biçim değişikliğine hazırlanma kaynak günlükleri bir depolama hesabına arşivlendi](diagnostic-logs-append-blobs.md) .


| Kaynak günlükleri şema özelliği | Etkinlik günlüğü REST API şeması özelliği | Notlar |
| --- | --- | --- |
| time | eventTimestamp |  |
| resourceId | resourceId | SubscriptionID, resourceType, resourceGroupName, RESOURCEID tarafından algılanır. |
| operationName | operationName. Value |  |
| category | İşlem adının parçası | İşlem türü ayırıcıları-"yaz"/"Sil"/"Action" |
| resultType | durum. değer | |
| resultSignature | alt durum. değer | |
| resultDescription | açıklama |  |
| durationMs | Yok | Her zaman 0 |
| callerIpAddress | httpRequest. clientIpAddress |  |
| correlationId | correlationId |  |
| identity | talepler ve yetkilendirme özellikleri |  |
| Düzey | Düzey |  |
| location | Yok | Olayın işlendiği konum. *Bu, kaynağın konumu değildir, ancak bunun yerine olayın işlendiği yerdir. Bu özellik gelecekteki bir güncelleştirmede kaldırılacaktır.* |
| Özellikler | Properties. eventProperties |  |
| Properties. eventCategory | category | Properties. eventCategory yoksa, Kategori "Administrative" dır |
| Properties. eventName | eventName |  |
| Properties. operationId | operationId |  |
| Properties. eventProperties | properties |  |

Aşağıda bu şemayı kullanan bir olay örneği verilmiştir.

``` JSON
{
    "records": [
        {
            "time": "2015-01-21T22:14:26.9792776Z",
            "resourceId": "/subscriptions/s1/resourceGroups/MSSupportGroup/providers/microsoft.support/supporttickets/115012112305841",
            "operationName": "microsoft.support/supporttickets/write",
            "category": "Write",
            "resultType": "Success",
            "resultSignature": "Succeeded.Created",
            "durationMs": 2826,
            "callerIpAddress": "111.111.111.11",
            "correlationId": "c776f9f4-36e5-4e0e-809b-c9b3c3fb62a8",
            "identity": {
                "authorization": {
                    "scope": "/subscriptions/s1/resourceGroups/MSSupportGroup/providers/microsoft.support/supporttickets/115012112305841",
                    "action": "microsoft.support/supporttickets/write",
                    "evidence": {
                        "role": "Subscription Admin"
                    }
                },
                "claims": {
                    "aud": "https://management.core.windows.net/",
                    "iss": "https://sts.windows.net/72f988bf-86f1-41af-91ab-2d7cd011db47/",
                    "iat": "1421876371",
                    "nbf": "1421876371",
                    "exp": "1421880271",
                    "ver": "1.0",
                    "http://schemas.microsoft.com/identity/claims/tenantid": "1e8d8218-c5e7-4578-9acc-9abbd5d23315 ",
                    "http://schemas.microsoft.com/claims/authnmethodsreferences": "pwd",
                    "http://schemas.microsoft.com/identity/claims/objectidentifier": "2468adf0-8211-44e3-95xq-85137af64708",
                    "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn": "admin@contoso.com",
                    "puid": "20030000801A118C",
                    "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier": "9vckmEGF7zDKk1YzIY8k0t1_EAPaXoeHyPRn6f413zM",
                    "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname": "John",
                    "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname": "Smith",
                    "name": "John Smith",
                    "groups": "cacfe77c-e058-4712-83qw-f9b08849fd60,7f71d11d-4c41-4b23-99d2-d32ce7aa621c,31522864-0578-4ea0-9gdc-e66cc564d18c",
                    "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name": " admin@contoso.com",
                    "appid": "c44b4083-3bq0-49c1-b47d-974e53cbdf3c",
                    "appidacr": "2",
                    "http://schemas.microsoft.com/identity/claims/scope": "user_impersonation",
                    "http://schemas.microsoft.com/claims/authnclassreference": "1"
                }
            },
            "level": "Information",
            "location": "global",
            "properties": {
                "statusCode": "Created",
                "serviceRequestId": "50d5cddb-8ca0-47ad-9b80-6cde2207f97c"
            }
        }
    ]
}
```





## <a name="next-steps"></a>Sonraki adımlar
* [Etkinlik günlüğü hakkında daha fazla bilgi edinin](platform-logs-overview.md)
* [Log Analytics çalışma alanına, Azure depolama 'ya veya Olay Hub 'larına etkinlik günlüğü göndermek için bir tanılama ayarı oluşturun](diagnostic-settings.md)

