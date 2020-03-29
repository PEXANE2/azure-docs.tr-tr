---
title: Azure Güvenlik Merkezi uyarıları için şemalar
description: Bu makalede, güvenlik uyarıları için Azure Güvenlik Merkezi tarafından kullanılan farklı şemalar açıklanmaktadır.
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/19/2020
ms.author: memildin
ms.openlocfilehash: 19ca17f66f6818ed4c3ef532e2030cc03f0e73ce
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80062960"
---
# <a name="security-alerts-schemas"></a>Güvenlik şemaları uyarır

Güvenlik Merkezi kaynaklarına yönelik tehditleri algıladığında Azure Güvenlik Merkezi'nin standart katmanının kullanıcıları güvenlik uyarıları alır.

Bu güvenlik uyarılarını Azure Güvenlik Merkezi'nin **Tehdit Koruması** sayfalarında veya şu gibi harici araçlar aracılığıyla görüntüleyebilirsiniz:

- [Azure Sentinel](https://docs.microsoft.com/azure/sentinel/) - Microsoft'un bulut ayarı olan SIEM'i. Sentinel Bağlayıcısı, Azure Güvenlik Merkezi'nden uyarılar alır ve bunları Azure Sentinel için [Log Analytics çalışma alanına](https://docs.microsoft.com/azure/azure-monitor/learn/quick-create-workspace) gönderir.
- Üçüncü taraf SIEM'ler - [Azure Etkinlik Hub'larına](https://docs.microsoft.com/azure/event-hubs/)veri göndermek için Güvenlik Merkezi'nin [sürekli dışa aktarma](continuous-export.md) araçlarını kullanın. Ardından Olay Hub verilerinizi bir üçüncü taraf SIEM ile tümleştirin.
- [REST API](https://docs.microsoft.com/rest/api/securitycenter/) - Uyarılara erişmek için REST API kullanıyorsanız, [çevrimiçi Uyarılar API belgelerine](https://docs.microsoft.com/rest/api/securitycenter/alerts)bakın.

Uyarıları kullanmak için herhangi bir programyöntemi kullanıyorsanız, sizinle alakalı alanları bulmak için doğru şemaya ihtiyacınız vardır. Ayrıca, bir Olay Hub'ına dışa aktarıyorsanız veya genel HTTP bağlayıcılarıyla İş Akışı Otomasyonu'nu tetiklemeye çalışıyorsanız, JSON nesnelerini düzgün bir şekilde ayrışdırmak için şemaları kullanın.

>[!IMPORTANT]
> Şema bu senaryoların her biri için biraz farklıdır, bu nedenle aşağıdaki ilgili sekmeyi seçtiğinizden emin olun.


## <a name="the-schemas"></a>Şemalar 


### <a name="workflow-automation-and-continuous-export-to-event-hub"></a>[İş akışı otomasyonu ve Event Hub'a sürekli ihracat](#tab/schema-continuousexport)

### <a name="sample-json-for-alerts-sent-to-logic-apps-event-hub-and-third-party-siems"></a>Logic Apps, Event Hub ve üçüncü taraf SIEM'lere gönderilen uyarılar için örnek JSON

Aşağıda uyarı olaylarının şemasını bulacaksınız:

- Güvenlik Merkezi'nin iş akışı otomasyonunda yapılandırılan Azure Mantık Uygulaması örnekleri
- Güvenlik Merkezi'nin sürekli dışa aktarma özelliğini kullanan Azure Etkinlik Merkezi

İş akışı otomasyonu özelliği hakkında daha fazla bilgi için [uyarılara ve önerilere verilen yanıtları otomatikleştir'e](workflow-automation.md)bakın.
Sürekli dışa aktarma hakkında daha fazla bilgi için [Dışa Aktarma uyarıları ve önerilerine](continuous-export.md)bakın.

[!INCLUDE [Workflow schema](../../includes/security-center-alerts-schema-workflow-automation.md)]




### <a name="azure-sentinel-and-log-analytics-workspaces"></a>[Azure Sentinel ve Log Analytics çalışma alanları](#tab/schema-sentinel)

Sentinel Bağlayıcısı, Azure Güvenlik Merkezi'nden uyarılar alır ve bunları Azure Sentinel için Log Analytics Çalışma Alanına gönderir. 

Güvenlik Merkezi uyarılarını kullanarak bir Sentinel vakası veya olay oluşturmak için, aşağıda gösterilen bu uyarılar için şemaya ihtiyacınız vardır. 

Azure Sentinel hakkında daha fazla bilgi için [belgelere](https://docs.microsoft.com/azure/sentinel/)bakın.

[!INCLUDE [Sentinel and workspace schema](../../includes/security-center-alerts-schema-log-analytics-workspace.md)]




### <a name="azure-activity-log"></a>[Azure Etkinlik Günlüğü](#tab/schema-activitylog)

Azure Güvenlik Merkezi, Azure Etkinlik Günlüğü'nde etkinlik olarak Güvenlik uyarıları oluşturdu.

Etkinliği Etkinleştir'le uyarı olayını gösterildiği gibi arayarak Etkinlik Günlüğü'ndeki güvenlik uyarılarını görüntüleyebilirsiniz:

[![Etkinleştir Uyarısı olayı için Etkinlik günlüğünde arama](media/alerts-schemas/sample-activity-log-alert.png)](media/alerts-schemas/sample-activity-log-alert.png#lightbox)


### <a name="sample-json-for-alerts-sent-to-azure-activity-log"></a>Azure Etkinlik Günlüğü'ne gönderilen uyarılar için örnek JSON

```json
{
    "channels": "Operation",
    "correlationId": "2518250008431989649_e7313e05-edf4-466d-adfd-35974921aeff",
    "description": "PREVIEW - Role binding to the cluster-admin role detected. Kubernetes audit log analysis detected a new binding to the cluster-admin role which gives administrator privileges.\r\nUnnecessary administrator privileges might cause privilege escalation in the cluster.",
    "eventDataId": "2518250008431989649_e7313e05-edf4-466d-adfd-35974921aeff",
    "eventName": {
        "value": "PREVIEW - Role binding to the cluster-admin role detected",
        "localizedValue": "PREVIEW - Role binding to the cluster-admin role detected"
    },
    "category": {
        "value": "Security",
        "localizedValue": "Security"
    },
    "eventTimestamp": "2019-12-25T18:52:36.801035Z",
    "id": "/subscriptions/SUBSCRIPTION_ID/resourceGroups/RESOURCE_GROUP_NAME/providers/Microsoft.Security/locations/centralus/alerts/2518250008431989649_e7313e05-edf4-466d-adfd-35974921aeff/events/2518250008431989649_e7313e05-edf4-466d-adfd-35974921aeff/ticks/637128967568010350",
    "level": "Informational",
    "operationId": "2518250008431989649_e7313e05-edf4-466d-adfd-35974921aeff",
    "operationName": {
        "value": "Microsoft.Security/locations/alerts/activate/action",
        "localizedValue": "Activate Alert"
    },
    "resourceGroupName": "RESOURCE_GROUP_NAME",
    "resourceProviderName": {
        "value": "Microsoft.Security",
        "localizedValue": "Microsoft.Security"
    },
    "resourceType": {
        "value": "Microsoft.Security/locations/alerts",
        "localizedValue": "Microsoft.Security/locations/alerts"
    },
    "resourceId": "/subscriptions/SUBSCRIPTION_ID/resourceGroups/RESOURCE_GROUP_NAME/providers/Microsoft.Security/locations/centralus/alerts/2518250008431989649_e7313e05-edf4-466d-adfd-35974921aeff",
    "status": {
        "value": "Active",
        "localizedValue": "Active"
    },
    "subStatus": {
        "value": "",
        "localizedValue": ""
    },
    "submissionTimestamp": "2019-12-25T19:14:03.5507487Z",
    "subscriptionId": "SUBSCRIPTION_ID",
    "properties": {
        "clusterRoleBindingName": "cluster-admin-binding",
        "subjectName": "for-binding-test",
        "subjectKind": "ServiceAccount",
        "username": "masterclient",
        "actionTaken": "Detected",
        "resourceType": "Kubernetes Service",
        "severity": "Low",
        "intent": "[\"Persistence\"]",
        "compromisedEntity": "ASC-IGNITE-DEMO",
        "remediationSteps": "[\"Review the user in the alert details. If cluster-admin is unnecessary for this user, consider granting lower privileges to the user.\"]",
        "attackedResourceType": "Kubernetes Service"
    },
    "relatedEvents": []
}
```

### <a name="the-data-model-of-the-schema"></a>Şema veri modeli

|Alan|Açıklama|
|----|----|
|**Kanal**|Sabit, "Operasyon"|
|**correlationId**|Azure Güvenlik Merkezi uyarı kimliği|
|**Açıklama**|Uyarının açıklaması|
|**olayDataId**|Bkz. korelasyonId|
|**eventName**|Değer ve yerelleştirilmiş Değer alt alanları uyarı görüntü adı içerir|
|**category**|Değer ve yerelleştirilmiş Değer alt alanları sabittir - "Güvenlik"|
|**olayTimestamp**|Uyarının oluşturulduğu zaman için UTC zaman damgası|
|**Kimliği**|Tam nitelikli uyarı kimliği|
|**Düzey**|Sabit, "Bilgi"|
|**operationId**|Bkz. korelasyonId|
|**operationName**|Değer alanı sabittir - "Microsoft.Security/locations/alerts/activate/action", ve yerelleştirilmiş değer "Uyarıyı Etkinleştir" olacaktır (kullanıcı yerelalanı nın yerelleştirilmiş olması olasıdır)|
|**resourceGroupName**|Kaynak grup adını içerecektir|
|**kaynakProviderName**|Değer ve yerelleştirilmiş Değer alt alanları sabittir - "Microsoft.Security"|
|**resourceType**|Değer ve yerelleştirilmiş Değer alt alanları sabittir - "Microsoft.Security/konumları/uyarıları"|
|**Resourceıd**|Tam nitelikli Azure kaynak kimliği|
|**durum**|Değer ve yerelleştirilmiş Değer alt alanları sabittir - "Etkin"|
|**altDurum**|Değer ve yerelleştirilmiş Değer alt alanları boş|
|**teslimTimestamp**|Etkinlik Günlüğü'ne olay göndermeUTC zaman damgası|
|**subscriptionId**|Gizliliği ihlal edilen kaynağın abonelik kimliği|
|**Özellikler**|Uyarıile ilgili ek özelliklerden oluşan bir JSON çantası. Bunlar bir uyarıdan diğerine değişebilir, ancak aşağıdaki alanlar tüm uyarılarda görünür:<br>- şiddeti: Saldırının şiddeti<br>- tehlikeye Varlık: tehlikeye kaynağın adı<br>- düzeltme Adımları: Yapılacak düzeltme adımları dizisi<br>- niyet: Uyarının öldürme zinciri niyeti. Olası niyetler [Niyetler tablosunda](alerts-reference.md#intentions) belgelenmiştir|
|**relatedEtkinlikler**|Sabit - boş dizi|
|||





### <a name="ms-graph-api"></a>[MS Grafik API](#tab/schema-graphapi)

Microsoft Graph, Microsoft 365'te veri ve istihbarata açılan ağ geçididir. Office 365, Windows 10 ve Enterprise Mobility + Security'deki muazzam miktardaki verilere erişmek için kullanabileceğiniz birleşik bir programlanabilirlik modeli sağlar. Milyonlarca kullanıcıyla etkileşimde bulunan kuruluşlar ve tüketiciler için uygulamalar oluşturmak için Microsoft Graph'taki veri zenginliğini kullanın.

MS Graph'a gönderilen güvenlik uyarıları için şema ve JSON gösterimi [Microsoft Graph belgelerinde](https://docs.microsoft.com/graph/api/resources/alert?view=graph-rest-1.0)mevcuttur.

---


## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, Azure Güvenlik Merkezi'nin tehdit koruma araçlarının güvenlik uyarı bilgileri gönderirken kullandığı şemalar açıklanmıştır.

Güvenlik Merkezi'nin dışından güvenlik uyarılarına erişme yolları hakkında daha fazla bilgi için aşağıdaki sayfalara bakın:

- [Azure Sentinel](https://docs.microsoft.com/azure/sentinel/) - Microsoft'un bulut ayarı olan SIEM
- [Azure Etkinlik Hub'ları](https://docs.microsoft.com/azure/event-hubs/) - Microsoft'un tamamen yönetilen, gerçek zamanlı veri alma hizmeti
- Güvenlik Merkezi'nin [sürekli ihracat özelliği](continuous-export.md)
- [Log Analytics çalışma alanları](https://docs.microsoft.com/azure/azure-monitor/learn/quick-create-workspace) - Azure Monitor günlük verilerini veri ve yapılandırma bilgilerini içeren bir kapsayıcı olan Log Analytics çalışma alanında saklar
