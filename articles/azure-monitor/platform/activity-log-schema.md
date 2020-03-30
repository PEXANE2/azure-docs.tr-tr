---
title: Azure Etkinlik Günlüğü etkinlik şeması
description: Azure Etkinliği günlüğündeki her kategori için olay şemasını açıklar.
author: bwren
services: azure-monitor
ms.topic: reference
ms.date: 12/04/2019
ms.author: bwren
ms.subservice: logs
ms.openlocfilehash: c2f171c79423e0cfe8b57c05b8248679f9ada9f1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79472750"
---
# <a name="azure-activity-log-event-schema"></a>Azure Etkinlik Günlüğü etkinlik şeması
[Azure Etkinliği günlüğü,](platform-logs-overview.md) Azure'da gerçekleşen abonelik düzeyindeki olaylarhakkında bilgi sağlar. Bu makalede, her kategori için olay şeması açıklanmaktadır. 

Aşağıdaki örnekler, Portal, PowerShell, CLI ve REST API'den Etkinlik günlüğüne erişdiğinizde şema gösterilmektedir. [Etkinlik günlüğünü depolama alanına veya Olay Hub'larına akışla](resource-logs-stream-event-hubs.md)aktardığınızda şema farklıdır. Makalenin sonunda, özelliklerin [kaynak günlükleri şemasına](diagnostic-logs-schema.md) eşlemesi sağlanır.

## <a name="administrative"></a>Yönetim
Bu kategori, Kaynak Yöneticisi aracılığıyla gerçekleştirilen tüm oluşturma, güncelleştirme, silme ve eylem işlemlerinin kaydını içerir. Bu kategoride göreceğiniz olay türlerine örnek olarak "sanal makine oluşturma" ve "ağ güvenlik grubunu sil" gibi örnekler arasında Kaynak Yöneticisi'ni kullanan bir kullanıcı veya uygulama tarafından gerçekleştirilen her eylem, belirli bir kaynak türünde işlem olarak modellenmiştir. İşlem türü Yazma, Silme veya Eylem ise, bu işlemin hem başlangıç hem de başarı veya başarısız kayıtları Yönetim kategorisinde kaydedilir. Yönetim kategorisi, abonelikteki rol tabanlı erişim denetiminde yapılan değişiklikleri de içerir.

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
| yetkilendirme |Olayın RBAC özelliklerinin blob'u. Genellikle "eylem", "rol" ve "kapsam" özelliklerini içerir. |
| Ara -yan |İşlemi gerçekleştiren kullanıcının e-posta adresi, UPN talebi veya kullanılabilirlik durumuna göre SPN talebi. |
| Kanal |Aşağıdaki değerlerden biri: "Yönetici", "Operasyon" |
| Iddia |Bu işlemi Kaynak Yöneticisi'nde gerçekleştirmek için kullanıcının veya uygulamanın kimliğini doğrulamak için Active Directory tarafından kullanılan JWT belirteci. |
| correlationId |Genellikle dize biçiminde bir GUID. Bir bağıntılıId'yi paylaşan olaylar aynı uber eylemine aittir. |
| açıklama |Bir olayın statik metin açıklaması. |
| olayDataId |Bir olayın benzersiz tanımlayıcısı. |
| eventName | İdari olayın dost adı. |
| category | Her zaman "İdari" |
| httpİstek |Http İsteği'ni açıklayan Blob. Genellikle "clientRequestId", "clientIpAddress" ve "method" (HTTP yöntemi) içerir. Örneğin, PUT). |
| düzey |Olayın düzeyi. Aşağıdaki değerlerden biri: "Kritik", "Hata", "Uyarı" ve "Bilgilendir" |
| resourceGroupName |Etkilenen kaynak için kaynak grubunun adı. |
| kaynakProviderName |Etkilenen kaynak için kaynak sağlayıcısının adı |
| resourceType | Yönetim olayından etkilenen kaynak türü. |
| resourceId |Etkilenen kaynağın kaynak kimliği. |
| operationId |Tek bir işlemiçin karşılık gelen olaylar arasında paylaşılan bir GUID. |
| operationName |Operasyonun adı. |
| properties |Olayın `<Key, Value>` ayrıntılarını açıklayan çiftler kümesi (diğer bir şekilde Sözlük). |
| durum |İşlemin durumunu açıklayan dize. Bazı ortak değerler şunlardır: Başlatılmış, Devam Eden, Başarılı, Başarısız, Etkin, Çözümlü. |
| altDurum |Genellikle ilgili REST çağrısının HTTP durum kodu, ancak bu ortak değerler gibi bir alt durumu açıklayan diğer dizeleri de içerebilir: Tamam (HTTP Durum Kodu: 200), Oluşturulan (HTTP Durum Kodu: 201), Kabul (HTTP Durum Kodu: 202), İçerik Yok (HTTP Durum Kod: 204), Kötü İstek (HTTP Durum Kodu: 400), Bulunamadı (HTTP Durum Kodu: 404), Çakışma (HTTP Durum Kodu: 409), İç Sunucu Hatası (HTTP Durum Kodu: 500), Hizmet Kullanılamıyor (HTTP Durum Kodu: 503), Ağ Geçidi Zaman Alameti (HTTP Durum Kodu: 504). |
| olayTimestamp |Etkinlik Azure hizmeti tarafından oluşturulduğunda, olaya karşılık gelen isteği işleyen zaman damgası. |
| teslimTimestamp |Olay sorgu için kullanılabilir hale geldiğinde zaman damgası. |
| subscriptionId |Azure Abonelik Kimliği. |

## <a name="service-health"></a>Hizmet durumu
Bu kategori, Azure'da meydana gelen hizmet durumu olaylarının kaydını içerir. Bu kategoride göreceğiniz olay türüne örnek olarak "Doğu ABD'de SQL Azure kapalı kalma süresi yaşıyor." Hizmet durumu olayları beş çeşittir: Eylem Gerekli, Yardımlı Kurtarma, Olay, Bakım, Bilgi veya Güvenlik ve yalnızca abonelikte olaydan etkilenecek bir kaynağınız varsa görünür.

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
Özelliklerdeki değerlerle ilgili belgeler için [hizmet durumu bildirimleri](./../../azure-monitor/platform/service-notifications.md) makalesine bakın.

## <a name="resource-health"></a>Kaynak durumu
Bu kategori, Azure kaynaklarınızın neden olduğu kaynak durumu olaylarının kaydını içerir. Bu kategoride göreceğiniz olay türüne bir örnek olarak "Sanal Makine sistem durumu kullanılamaz olarak değiştirilmiştir." Kaynak sistem durumu olayları dört sistem durumu durumundan birini temsil edebilir: Kullanılabilir, Kullanılamıyor, Bozulmuş ve Bilinmiyor. Ayrıca, kaynak durumu olayları Platform Başlatılan veya Kullanıcı Başlatılan olarak kategorize edilebilir.

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
| Kanal | Her zaman "Yönetici, Operasyon" |
| correlationId | Dize biçiminde bir GUID. |
| açıklama |Uyarı olayının statik metin açıklaması. |
| olayDataId |Uyarı olayının benzersiz tanımlayıcısı. |
| category | Her zaman "Kaynak Sağlık" |
| olayTimestamp |Etkinlik Azure hizmeti tarafından oluşturulduğunda, olaya karşılık gelen isteği işleyen zaman damgası. |
| düzey |Olayın düzeyi. Aşağıdaki değerlerden biri: "Kritik", "Hata", "Uyarı", "Bilgilendir", ve "Verbose" |
| operationId |Tek bir işlemiçin karşılık gelen olaylar arasında paylaşılan bir GUID. |
| operationName |Operasyonun adı. |
| resourceGroupName |Kaynağı içeren kaynak grubunun adı. |
| kaynakProviderName |Her zaman "Microsoft.Resourcehealth/healthevent/action". |
| resourceType | Kaynak Durumu olayından etkilenen kaynak türü. |
| resourceId | Etkilenen kaynak için kaynak kimliğinin adı. |
| durum |Sistem durumu olayının durumunu açıklayan dize. Değerler olabilir: Etkin, Çözümlü, Devam, Güncellenme. |
| altDurum | Genellikle uyarılar için null. |
| teslimTimestamp |Olay sorgu için kullanılabilir hale geldiğinde zaman damgası. |
| subscriptionId |Azure Abonelik Kimliği. |
| properties |Olayın `<Key, Value>` ayrıntılarını açıklayan çiftler kümesi (diğer bir şekilde Sözlük).|
| properties.title | Kaynağın sistem durumu durumunu açıklayan kullanıcı dostu bir dize. |
| properties.details | Olayla ilgili daha fazla ayrıntıyı açıklayan kullanıcı dostu bir dize. |
| properties.currentHealthStatus | Kaynağın geçerli sistem durumu. Aşağıdaki değerlerden biri: "Kullanılabilir", "Kullanılamıyor", "Bozulmuş", ve "Bilinmiyor". |
| properties.previousHealthStatus | Kaynağın önceki sistem durumu. Aşağıdaki değerlerden biri: "Kullanılabilir", "Kullanılamıyor", "Bozulmuş", ve "Bilinmiyor". |
| özellikleri.type | Kaynak durumu olayının türüyle ilgili açıklama. |
| özellikleri.neden | Kaynak durumu olayının nedeninin açıklaması. Ya "UserInitiated" ve "PlatformInitiated". |


## <a name="alert"></a>Uyarı
Bu kategori, Azure uyarılarının tüm etkinleştirmelerinin kaydını içerir. Bu kategoride göreceğiniz olay türüne örnek olarak "MYVM'deki CPU % son 5 dakikadır 80'in üzerinde olmuştur." Çeşitli Azure sistemlerinin uyarı konsepti vardır - bir tür kural tanımlayabilir ve koşullar bu kuralla eşleştiğinde bir bildirim alabilirsiniz. Desteklenen bir Azure uyarı türü 'etkinleştirilir' veya bir bildirim oluşturmak için koşullar karşılansa, etkinleştirmenin kaydı da Etkinlik Günlüğü'nün bu kategorisine itilir.

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
| Ara -yan | Her zaman Microsoft.Insights/alertRules |
| Kanal | Her zaman "Yönetici, Operasyon" |
| Iddia | Uyarı motorunun SPN (servis ana adı) veya kaynak türüne sahip JSON blob'u. |
| correlationId | Dize biçiminde bir GUID. |
| açıklama |Uyarı olayının statik metin açıklaması. |
| olayDataId |Uyarı olayının benzersiz tanımlayıcısı. |
| category | Her zaman "Uyarı" |
| düzey |Olayın düzeyi. Aşağıdaki değerlerden biri: "Kritik", "Hata", "Uyarı" ve "Bilgilendir" |
| resourceGroupName |Etkilenen kaynak için kaynak grubunun adı, metrik bir uyarıysa. Diğer uyarı türleri için, uyarının kendisini içeren kaynak grubunun adıdır. |
| kaynakProviderName |Etkilenen kaynak için kaynak sağlayıcısının adı, metrik bir uyarıysa. Diğer uyarı türleri için, uyarının kendisi için kaynak sağlayıcısının adıdır. |
| resourceId | Etkilenen kaynak için kaynak kimliğinin adı, metrik bir uyarıysa. Diğer uyarı türleri için, uyarı kaynağının kendisi kaynak kimliğidir. |
| operationId |Tek bir işlemiçin karşılık gelen olaylar arasında paylaşılan bir GUID. |
| operationName |Operasyonun adı. |
| properties |Olayın `<Key, Value>` ayrıntılarını açıklayan çiftler kümesi (diğer bir şekilde Sözlük). |
| durum |İşlemin durumunu açıklayan dize. Bazı ortak değerler şunlardır: Başlatılmış, Devam Eden, Başarılı, Başarısız, Etkin, Çözümlü. |
| altDurum | Genellikle uyarılar için null. |
| olayTimestamp |Etkinlik Azure hizmeti tarafından oluşturulduğunda, olaya karşılık gelen isteği işleyen zaman damgası. |
| teslimTimestamp |Olay sorgu için kullanılabilir hale geldiğinde zaman damgası. |
| subscriptionId |Azure Abonelik Kimliği. |

### <a name="properties-field-per-alert-type"></a>Uyarı türüne göre özellikler alanı
Özellikler alanı, uyarı olayının kaynağına bağlı olarak farklı değerler içerir. İki yaygın uyarı olay sağlayıcısı Etkinlik Günlüğü uyarıları ve metrik uyarılardır.

#### <a name="properties-for-activity-log-alerts"></a>Etkinlik Günlüğü uyarıları için özellikler
| Öğe Adı | Açıklama |
| --- | --- |
| properties.subscriptionId | Bu etkinlik günlüğü uyarı kuralının etkinleştirilmesine neden olan etkinlik günlüğü olayındaki abonelik kimliği. |
| properties.eventDataId | Bu etkinlik günlüğü uyarı kuralının etkinleştirilmesine neden olan etkinlik günlüğü olayındaki olay veri kimliği. |
| properties.resourceGroup | Bu etkinlik günlüğü uyarı kuralının etkinleştirilmesine neden olan etkinlik günlüğü olayındaki kaynak grubu. |
| properties.resourceId | Bu etkinlik günlüğü uyarı kuralının etkinleştirilmesine neden olan etkinlik günlüğü olayındaki kaynak kimliği. |
| properties.eventTimestamp | Bu etkinlik günlüğü uyarı kuralının etkinleştirilmesine neden olan etkinlik günlüğü olayının olay zaman damgası. |
| properties.operationName | Bu etkinlik günlüğü uyarı kuralının etkinleştirilmesine neden olan etkinlik günlüğü olayındaki işlem adı. |
| özellikleri.status | Bu etkinlik günlüğü uyarı kuralının etkinleştirilmesine neden olan etkinlik günlüğü olayından durum.|

#### <a name="properties-for-metric-alerts"></a>Metrik uyarılar için özellikler
| Öğe Adı | Açıklama |
| --- | --- |
| Özellikler. Ruleuri | Metrik uyarı kuralının kaynak kimliği. |
| Özellikler. Rulename | Metrik uyarı kuralının adı. |
| Özellikler. Kural Açıklaması | Metrik uyarı kuralının açıklaması (uyarı kuralında tanımlandığı gibi). |
| Özellikler. Eşik | Metrik uyarı kuralının değerlendirilmesinde kullanılan eşik değeri. |
| Özellikler. Pencere BoyutuDakika | Metrik uyarı kuralının değerlendirilmesinde kullanılan pencere boyutu. |
| Özellikler. Toplama | Metrik uyarı kuralında tanımlanan toplama türü. |
| Özellikler. Işleç | Metrik uyarı kuralının değerlendirilmesinde kullanılan koşullu işleç. |
| Özellikler. Metrik Adı | Metrik uyarı kuralının değerlendirilmesinde kullanılan metnin metrik adı. |
| Özellikler. Metrik Birim | Metrik uyarı kuralının değerlendirilmesinde kullanılan metrik birim. |

## <a name="autoscale"></a>Otomatik Ölçeklendirme
Bu kategori, aboneliğinizde tanımladığınız otomatik ölçek ayarlarını temel alan otomatik ölçeklendirme motorunun çalışmasıyla ilgili tüm olayların kaydını içerir. Bu kategoride göreceğiniz olay türüne bir örnek olarak "Otomatik ölçeklendirme eylem başarısız olur." Otomatik ölçeklendirmeyi kullanarak, otomatik ölçeklendirme ayarını kullanarak günün saatine ve/veya yük (metrik) verilerine göre desteklenen kaynak türündeki örnek sayısını otomatik olarak ölçeklendirebilir veya ölçeklendirebilirsiniz. Ölçeklendirmek veya küçültmek için koşullar karşılandığında, başlangıç ve başarılı veya başarısız olaylar bu kategoriye kaydedilir.

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
| Ara -yan | Her zaman Microsoft.Insights/autoscaleSettings |
| Kanal | Her zaman "Yönetici, Operasyon" |
| Iddia | Otomatik ölçeklendirme motorunun SPN (servis ana adı) veya kaynak türüne sahip JSON blob'u. |
| correlationId | Dize biçiminde bir GUID. |
| açıklama |Otomatik ölçek olayının statik metin açıklaması. |
| olayDataId |Otomatik ölçek olayının benzersiz tanımlayıcısı. |
| düzey |Olayın düzeyi. Aşağıdaki değerlerden biri: "Kritik", "Hata", "Uyarı" ve "Bilgilendir" |
| resourceGroupName |Otomatik ölçek ayarı için kaynak grubunun adı. |
| kaynakProviderName |Otomatik ölçek ayarı için kaynak sağlayıcısının adı. |
| resourceId |Otomatik ölçek ayarının kaynak kimliği. |
| operationId |Tek bir işlemiçin karşılık gelen olaylar arasında paylaşılan bir GUID. |
| operationName |Operasyonun adı. |
| properties |Olayın `<Key, Value>` ayrıntılarını açıklayan çiftler kümesi (diğer bir şekilde Sözlük). |
| Özellikler. Açıklama | Otomatik ölçeklendirme motorunun ne yaptığını ayrıntılı bir şekilde açıklaması. |
| Özellikler. Resourcename | Etkilenen kaynağın kaynak kimliği (ölçek eyleminin gerçekleştirildiği kaynak) |
| Özellikler. OldInstancesCount | Otomatik ölçeklendirme eyleminin yürürlüğe girmesinden önceki örneklerin sayısı. |
| Özellikler. NewInstancesCount | Otomatik ölçeklendirme eyleminin yürürlüğe girmesinden sonraki örneklerin sayısı. |
| Özellikler. LastScaleActionTime | Otomatik ölçeklendirme eyleminin ne zaman gerçekleştiği zaman damgası. |
| durum |İşlemin durumunu açıklayan dize. Bazı ortak değerler şunlardır: Başlatılmış, Devam Eden, Başarılı, Başarısız, Etkin, Çözümlü. |
| altDurum | Genellikle otomatik ölçek için null. |
| olayTimestamp |Etkinlik Azure hizmeti tarafından oluşturulduğunda, olaya karşılık gelen isteği işleyen zaman damgası. |
| teslimTimestamp |Olay sorgu için kullanılabilir hale geldiğinde zaman damgası. |
| subscriptionId |Azure Abonelik Kimliği. |

## <a name="security"></a>Güvenlik
Bu kategori, Azure Güvenlik Merkezi tarafından oluşturulan tüm uyarıların kaydını içerir. Bu kategoride göreceğiniz olay türüne bir örnek "Şüpheli çift uzantı dosyası yürütülür."

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
| Kanal | Her zaman "Operasyon" |
| correlationId | Dize biçiminde bir GUID. |
| açıklama |Güvenlik olayının statik metin açıklaması. |
| olayDataId |Güvenlik olayının benzersiz tanımlayıcısı. |
| eventName |Güvenlik olayının dost adı. |
| category | Her zaman "Güvenlik" |
| Kimlik |Güvenlik olayının benzersiz kaynak tanımlayıcısı. |
| düzey |Olayın düzeyi. Aşağıdaki değerlerden biri: "Kritik", "Hata", "Uyarı" veya "Bilgilendirici" |
| resourceGroupName |Kaynak için kaynak grubunun adı. |
| kaynakProviderName |Azure Güvenlik Merkezi için kaynak sağlayıcısının adı. Her zaman "Microsoft.Security". |
| resourceType |"Microsoft.Security/konumları/uyarıları" gibi güvenlik olayını oluşturan kaynak türü |
| resourceId |Güvenlik uyarısı kaynak kimliği. |
| operationId |Tek bir işlemiçin karşılık gelen olaylar arasında paylaşılan bir GUID. |
| operationName |Operasyonun adı. |
| properties |Olayın `<Key, Value>` ayrıntılarını açıklayan çiftler kümesi (diğer bir şekilde Sözlük). Bu özellikler, güvenlik uyarısı türüne bağlı olarak değişir. Güvenlik Merkezi'nden gelen uyarı türlerinin açıklaması için [bu sayfaya](../../security-center/security-center-alerts-overview.md) bakın. |
| Özellikler. Önem |Önem düzeyi. Olası değerler "Yüksek", "Orta" veya "Düşük" olur. |
| durum |İşlemin durumunu açıklayan dize. Bazı ortak değerler şunlardır: Başlatılmış, Devam Eden, Başarılı, Başarısız, Etkin, Çözümlü. |
| altDurum | Genellikle güvenlik olayları için null. |
| olayTimestamp |Etkinlik Azure hizmeti tarafından oluşturulduğunda, olaya karşılık gelen isteği işleyen zaman damgası. |
| teslimTimestamp |Olay sorgu için kullanılabilir hale geldiğinde zaman damgası. |
| subscriptionId |Azure Abonelik Kimliği. |

## <a name="recommendation"></a>Öneri
Bu kategori, hizmetleriniz için oluşturulan yeni önerilerin kaydını içerir. Bir öneri örneği "Geliştirilmiş hata toleransı için kullanılabilirlik kümelerini kullanın." Oluşturulabilecek dört tür Öneri olayı vardır: Yüksek Kullanılabilirlik, Performans, Güvenlik ve Maliyet Optimizasyonu. 

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
| Kanal | Her zaman "Operasyon" |
| correlationId | Dize biçiminde bir GUID. |
| açıklama |Öneri olayının statik metin açıklaması |
| olayDataId | Tavsiye olayının benzersiz tanımlayıcısı. |
| category | Her zaman "Tavsiye" |
| Kimlik |Öneri olayının benzersiz kaynak tanımlayıcısı. |
| düzey |Olayın düzeyi. Aşağıdaki değerlerden biri: "Kritik", "Hata", "Uyarı" veya "Bilgilendirici" |
| operationName |Operasyonun adı.  Her zaman "Microsoft.Advisor/generateRecommendations/action"|
| resourceGroupName |Kaynak için kaynak grubunun adı. |
| kaynakProviderName |"MICROSOFT.COMPUTE" gibi bu önerinin geçerli olduğu kaynak için kaynak sağlayıcısının adı |
| resourceType |"MICROSOFT.COMPUTE/virtualmachines" gibi bu önerinin uygulandığı kaynak için kaynak türünün adı |
| resourceId |Önerinin geçerli olduğu kaynağın kaynak kimliği |
| durum | Her zaman "Aktif" |
| teslimTimestamp |Olay sorgu için kullanılabilir hale geldiğinde zaman damgası. |
| subscriptionId |Azure Abonelik Kimliği. |
| properties |Önerinin `<Key, Value>` ayrıntılarını açıklayan çiftler kümesi (diğer bir şekilde Sözlük).|
| properties.recommendationSchemaVersion| Etkinlik Günlüğü girişinde yayınlanan öneri özelliklerinin Şema sürümü |
| properties.recommendationKategori | Önerinin kategorisi. Olası değerler "Yüksek Kullanılabilirlik", "Performans", "Güvenlik" ve "Maliyet" |
| properties.recommendationImpact| Tavsiyenin etkisi. Olası değerler "Yüksek", "Orta", "Düşük" |
| properties.recommendationRisk| Tavsiye riski. Olası değerler "Hata", "Uyarı", "Yok" |

## <a name="policy"></a>İlke

Bu kategori, [Azure İlkesi](../../governance/policy/overview.md)tarafından gerçekleştirilen tüm efekt eylem işlemlerinin kayıtlarını içerir. Bu kategoride göreceğiniz olay türlerine örnek olarak _Denetim_ ve _Reddet_verilebilir. İlke tarafından gerçekleştirilen her eylem, bir kaynak üzerinde bir işlem olarak modellenir.

### <a name="sample-policy-event"></a>Örnek İlke olayı

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

### <a name="policy-event-property-descriptions"></a>İlke olay özelliği açıklamaları

| Öğe Adı | Açıklama |
| --- | --- |
| yetkilendirme | Olayın RBAC özellikleri dizisi. Yeni kaynaklar için bu, değerlendirmeyi tetikleyen isteğin eylemi ve kapsamıdır. Varolan kaynaklar için eylem "Microsoft.Resources/checkPolicyCompliance/read" dir. |
| Ara -yan | Yeni kaynaklar için, dağıtımı başlatan kimlik. Varolan kaynaklar için Microsoft Azure İlkesi Öngörüleri RP GUID'i. |
| Kanal | İlke olayları yalnızca "İşlem" kanalını kullanır. |
| Iddia | Bu işlemi Kaynak Yöneticisi'nde gerçekleştirmek için kullanıcının veya uygulamanın kimliğini doğrulamak için Active Directory tarafından kullanılan JWT belirteci. |
| correlationId | Genellikle dize biçiminde bir GUID. Bir bağıntılıId'yi paylaşan olaylar aynı uber eylemine aittir. |
| açıklama | Bu alan İlke olayları için boştur. |
| olayDataId | Bir olayın benzersiz tanımlayıcısı. |
| eventName | "BeginRequest" veya "EndRequest". "BeginRequest" gecikmiş auditIfNotExists ve deployIfNotExists değerlendirmeleri için kullanılır ve bir deployIfNotExists efekti bir şablon dağıtımı başlattığında. Diğer tüm işlemler "EndRequest" döndürmektedir. |
| category | Etkinlik günlüğü olayını "İlke"ye ait olarak bildirir. |
| olayTimestamp | Etkinlik Azure hizmeti tarafından oluşturulduğunda, olaya karşılık gelen isteği işleyen zaman damgası. |
| Kimlik | Belirli bir kaynaktaki olayın benzersiz tanımlayıcısı. |
| düzey | Olayın düzeyi. Denetim "Uyarı" ve Reddet "Hata" kullanır. Bir auditIfNotExists veya deployIfNotExists hatası önem derecesine bağlı olarak "Uyarı" veya "Hata" oluşturabilir. Diğer tüm İlke olayları "Informational"ı kullanır. |
| operationId | Tek bir işlemiçin karşılık gelen olaylar arasında paylaşılan bir GUID. |
| operationName | İşlemin adı ve Doğrudan İlke etkisi ile ilişkilidir. |
| resourceGroupName | Değerlendirilen kaynak için kaynak grubunun adı. |
| kaynakProviderName | Değerlendirilen kaynak için kaynak sağlayıcısının adı. |
| resourceType | Yeni kaynaklar için, değerlendirilen türüdür. Varolan kaynaklar için "Microsoft.Resources/checkPolicyCompliance" döndürür. |
| resourceId | Değerlendirilen kaynağın kaynak kimliği. |
| durum | İlke değerlendirme sonucunun durumunu açıklayan dize. Çoğu İlke değerlendirmesi "Başarılı" döndürür, ancak Reddet efekti "Başarısız" olarak döndürür. auditIfNotExists veya deployIfNotExists hataları da "Başarısız" döndürür. |
| altDurum | İlke olayları için alan boş. |
| teslimTimestamp | Olay sorgu için kullanılabilir hale geldiğinde zaman damgası. |
| subscriptionId | Azure Abonelik Kimliği. |
| properties.isComplianceCheck | Yeni bir kaynak dağıtıldığında veya varolan bir kaynağın Kaynak Yöneticisi özellikleri güncelleştirildiğinde "False" döndürür. Diğer tüm [değerlendirme tetikleyicileri](../../governance/policy/how-to/get-compliance-data.md#evaluation-triggers) "True" ile sonuçlanır. |
| properties.resourceLocation | Değerlendirilen kaynağın Azure bölgesi. |
| özellikleri.ataları | Doğrudan ebeveynden en uzak büyükbabaya doğru sipariş edilen üst yönetim gruplarının virgülle ayrılmış bir listesi. |
| properties.policies | Bu İlke değerlendirmesinin sonucu olduğu ilke tanımı, atama, etki ve parametreler hakkındaki ayrıntıları içerir. |
| relatedEtkinlikler | Bu alan İlke olayları için boştur. |


## <a name="schema-from-storage-account-and-event-hubs"></a>Depolama hesabından ve etkinlik merkezlerinden şema
Azure Etkinliği günlüğünü bir depolama hesabına veya olay merkezine aktarırken, veriler [kaynak günlüğü şemasını](diagnostic-logs-schema.md)izler. Aşağıdaki tablo, yukarıdaki şemadan kaynak günlükleri şemasına kadar özelliklerin eşlemesini sağlar.

> [!IMPORTANT]
> Bir depolama hesabına yazılan Etkinlik günlüğü verilerinin biçimi 1 Kasım 2018 tarihinde JSON Lines olarak değiştirildi. Bu biçim değişikliğiyle ilgili ayrıntılar için [bir depolama hesabına arşivlenmiş Azure Monitor kaynak günlüklerine biçim değişikliği için hazırla'ya](diagnostic-logs-append-blobs.md) bakın.


| Kaynak şema özelliği günlükleri | Etkinlik Günlüğü REST API şema özelliği | Notlar |
| --- | --- | --- |
| time | olayTimestamp |  |
| resourceId | resourceId | subscriptionId, resourceType, resourceGroupName tüm kaynakKimliği çıkarılatır. |
| operationName | operationName.value |  |
| category | Operasyon adının bir parçası | İşlem türünün ayrılması - "Yazma"/"Sil"/"Eylem" |
| resultType | status.value | |
| resultSignature | substatus.value | |
| resultDescription | açıklama |  |
| durationMs | Yok | Her zaman 0 |
| callerIpAddress | httpRequest.clientIpAddress |  |
| correlationId | correlationId |  |
| identity | talepler ve yetkilendirme özellikleri |  |
| Düzey | Düzey |  |
| location | Yok | Olayın işlendiği yerin konumu. *Bu, kaynağın konumu değil, olayın işlendiği yerdir. Bu özellik gelecekteki bir güncelleştirmede kaldırılır.* |
| Özellikler | properties.eventProperties |  |
| properties.eventKategori | category | properties.eventCategory yoksa, kategori "Yönetim" |
| properties.eventName | eventName |  |
| properties.operationId | operationId |  |
| properties.eventProperties | properties |  |

Aşağıda, bu şema'yı kullanan bir olay örneği verilmiştir...

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
* [Etkinlik Günlüğü hakkında daha fazla bilgi edinin](platform-logs-overview.md)
* [Etkinlik Günlüğü'ni Günlük Analitiği çalışma alanına, Azure depolama alanına veya etkinlik hub'larına göndermek için tanılama ayarı oluşturma](diagnostic-settings.md)

