---
title: Azure Monitör'de klasik metrik uyarı içeren bir web hook'u arama
description: Azure metrik uyarılarını Azure dışındaki diğer sistemlere nasıl yönlendireceklerini öğrenin.
author: harelbr
ms.author: harelbr
ms.topic: conceptual
ms.date: 04/03/2017
ms.subservice: alerts
ms.openlocfilehash: 27510871f9a022cb27c6b03b812ce1d37b47312c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79248989"
---
# <a name="call-a-webhook-with-a-classic-metric-alert-in-azure-monitor"></a>Azure Monitör'de klasik metrik uyarı içeren bir web hook'u arama

Bir Azure uyarı bildirimini işleme sonrası veya özel eylemler için diğer sistemlere yönlendirmek için web hooks'ları kullanabilirsiniz. Bir web hook'u, SMS iletisi gönderen hizmetlere yönlendirmek, hataları günlüğe kaydetmek, bir takımı sohbet veya mesajlaşma hizmetleri aracılığıyla bildirmek veya diğer çeşitli eylemler için bir uyarı üzerinde kullanabilirsiniz. 

Bu makalede, Azure metrik uyarısı üzerinde bir web hook ayarlamak için nasıl açıklanmaktadır. Ayrıca bir webhook için HTTP POST için yük neye benzediğini gösterir. Bir Azure etkinlik günlüğü uyarısı (olaylar hakkında uyarı) kurulumu ve şeması hakkında bilgi için [bkz.](alerts-log-webhook.md)

Azure uyarıları, JSON formatındaki uyarı içeriğini uyarıyı oluştururken sağladığınız bir webhook URI'ye göndermek için HTTP POST'u kullanır. Şema bu makalede daha sonra tanımlanır. URI geçerli bir HTTP veya HTTPS bitiş noktası olmalıdır. Azure, bir uyarı etkinleştirildiğinde istek başına bir giriş gönderir.

## <a name="configure-webhooks-via-the-azure-portal"></a>Azure portalı üzerinden web hooks yapılandırma
[Azure portalında](https://portal.azure.com/)webhook URI eklemek veya güncellemek için **Uyarılar Oluştur/Güncelleştir'e**gidin.

![Uyarı kuralı bölmesi ekleme](./media/alerts-webhooks/Alertwebhook.png)

Ayrıca, bir çapraz [platform CLI](../../azure-monitor/platform/cli-samples.md#work-with-alerts)veya [Azure Monitor REST API'leri](https://msdn.microsoft.com/library/azure/dn933805.aspx)kullanarak [bir](../../azure-monitor/platform/powershell-quickstart-samples.md#create-metric-alerts)webhook URI'ye göndermek için bir uyarı yıkabilirsiniz.

## <a name="authenticate-the-webhook"></a>Webhook'u doğrula
Webhook, belirteç tabanlı yetkilendirme kullanarak kimlik doğrulaması yapabilir. Webhook URI bir belirteç kimliği ile kaydedilir. Örneğin, `https://mysamplealert/webcallback?tokenid=sometokenid&someparameter=somevalue`

## <a name="payload-schema"></a>Yük şeması
POST işlemi, tüm metrik tabanlı uyarılar için aşağıdaki JSON yükünü ve şemasını içerir:

```JSON
{
    "status": "Activated",
    "context": {
        "timestamp": "2015-08-14T22:26:41.9975398Z",
        "id": "/subscriptions/s1/resourceGroups/useast/providers/microsoft.insights/alertrules/ruleName1",
        "name": "ruleName1",
        "description": "some description",
        "conditionType": "Metric",
        "condition": {
            "metricName": "Requests",
            "metricUnit": "Count",
            "metricValue": "10",
            "threshold": "10",
            "windowSize": "15",
            "timeAggregation": "Average",
            "operator": "GreaterThanOrEqual"
        },
        "subscriptionId": "s1",
        "resourceGroupName": "useast",
        "resourceName": "mysite1",
        "resourceType": "microsoft.foo/sites",
        "resourceId": "/subscriptions/s1/resourceGroups/useast/providers/microsoft.foo/sites/mysite1",
        "resourceRegion": "centralus",
        "portalLink": "https://portal.azure.com/#resource/subscriptions/s1/resourceGroups/useast/providers/microsoft.foo/sites/mysite1"
    },
    "properties": {
        "key1": "value1",
        "key2": "value2"
    }
}
```


| Alan | Zorunlu | Sabit değerler kümesi | Notlar |
|:--- |:--- |:--- |:--- |
| durum |E |Etkinleştirildi, Çözüldü |Belirlediğiniz koşullara göre uyarının durumu. |
| bağlam |E | |Uyarı bağlamı. |
| timestamp |E | |Uyarının tetiklendiği saat. |
| id |E | |Her uyarı kuralının benzersiz bir kimliği vardır. |
| ad |E | |Uyarı adı. |
| açıklama |E | |Uyarının açıklaması. |
| Conditiontype |E |Metrik, Olay |İki tür uyarı desteklenir: metrik ve olay. Metrik uyarılar bir metrik koşula dayanır. Olay uyarıları, etkinlik günlüğündeki bir olaya dayanır. Uyarının bir metriyi mi yoksa bir olaya mı dayandığını denetlemek için bu değeri kullanın. |
| Durum |E | |**KoşulTipi** değerini temel alan denetlenen belirli alanlar. |
| metricName |Metrik uyarılar için | |Kuralın neyi izlediğini tanımlayan metnin adı. |
| metricUnit |Metrik uyarılar için |Bayt, BaytPerİkinci, CountPerSecond, Yüzde, Saniye |Metrikte izin verilen birim. [İzin verilen değerleri](https://msdn.microsoft.com/library/microsoft.azure.insights.models.unit.aspx)görün. |
| metricValue |Metrik uyarılar için | |Uyarıya neden olan ölçümün gerçek değeri. |
| Eşik |Metrik uyarılar için | |Uyarının etkinleştirildiği eşik değeri. |
| Windowsize |Metrik uyarılar için | |Eşiğe göre uyarı etkinliğini izlemek için kullanılan süre. Değeri 5 dakika ile 1 gün arasında olmalıdır. Değer ISO 8601 süre biçiminde olmalıdır. |
| timeAggregation |Metrik uyarılar için |Ortalama, Son, Maksimum, Minimum, Yok, Toplam |Toplanan verilerin zaman içinde nasıl birleştirileceğini. Varsayılan değer Ortalama'dır. [İzin verilen değerleri](https://msdn.microsoft.com/library/microsoft.azure.insights.models.aggregationtype.aspx)görün. |
| operator |Metrik uyarılar için | |Geçerli metrik verileri küme eşikle karşılaştırmak için kullanılan işleç. |
| subscriptionId |E | |Azure abonelik kimliği. |
| resourceGroupName |E | |Etkilenen kaynak için kaynak grubunun adı. |
| resourceName |E | |Etkilenen kaynağın kaynak adı. |
| resourceType |E | |Etkilenen kaynağın kaynak türü. |
| resourceId |E | |Etkilenen kaynağın kaynak kimliği. |
| resourceRegion |E | |Etkilenen kaynağın bölgesi veya konumu. |
| portalLink |E | |Portal kaynak özeti sayfasına doğrudan bağlantı. |
| properties |N |İsteğe bağlı |Olay la ilgili ayrıntıları içeren anahtar/değer çiftleri kümesi. Örneğin, `Dictionary<String, String>`. Özellikler alanı isteğe bağlıdır. Özel kullanıcı arası veya mantıksal uygulama tabanlı iş akışında, kullanıcılar taşıma yükü üzerinden geçirilebilen anahtar/değer çiftleri girebilir. Webhook'a özel özellikleri geri geçirmenin alternatif bir yolu webhook URI'nin kendisi (sorgu parametreleri olarak) üzerinden dir. |

> [!NOTE]
> **Özellikler** alanını yalnızca [Azure Monitor REST API'lerini](https://msdn.microsoft.com/library/azure/dn933805.aspx)kullanarak ayarlayabilirsiniz.
>
>

## <a name="next-steps"></a>Sonraki adımlar
* Videodaki Azure uyarıları ve web hook'ları hakkında daha fazla bilgi edinin [Azure uyarılarını PagerDuty ile tümleştirin.](https://go.microsoft.com/fwlink/?LinkId=627080)
* [Azure uyarılarında Azure Otomasyon komut dosyalarını (runbook' lar)](https://go.microsoft.com/fwlink/?LinkId=627081)nasıl çalıştırılacıla
* [Azure uyarısından Twilio üzerinden SMS mesajı göndermek için bir mantık uygulamasını](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-text-message-with-logic-app)nasıl kullanacağınızı öğrenin.
* [Azure uyarısından Bir Bolluk iletisi göndermek için bir mantık uygulamasını](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-slack-with-logic-app)nasıl kullanacağınızı öğrenin.
* [Azure uyarısından Azure kuyruğuna ileti göndermek için bir mantık uygulamasını](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-queue-with-logic-app)nasıl kullanacağınızı öğrenin.

