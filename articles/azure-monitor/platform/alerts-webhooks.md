---
title: Azure Izleyici 'de klasik ölçüm uyarısıyla bir Web kancası çağırma
description: Azure ölçüm uyarılarını diğer Azure dışı sistemlere yeniden yönlendirme hakkında bilgi edinin.
author: harelbr
ms.author: harelbr
ms.topic: conceptual
ms.date: 04/03/2017
ms.subservice: alerts
ms.openlocfilehash: 27510871f9a022cb27c6b03b812ce1d37b47312c
ms.sourcegitcommit: 509b39e73b5cbf670c8d231b4af1e6cfafa82e5a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/05/2020
ms.locfileid: "78373383"
---
# <a name="call-a-webhook-with-a-classic-metric-alert-in-azure-monitor"></a>Azure Izleyici 'de klasik ölçüm uyarısıyla bir Web kancası çağırma

Web kancalarını kullanarak, işlem sonrası veya özel eylemler için bir Azure uyarı bildirimini diğer sistemlere yönlendirebilirsiniz. Uyarı üzerinde bir Web kancasını SMS iletileri gönderen hizmetlere yönlendirmek, hataları günlüğe kaydetmek, sohbet veya mesajlaşma hizmetleri aracılığıyla bir takıma bildirmek veya diğer çeşitli eylemler için kullanabilirsiniz. 

Bu makalede bir Azure ölçüm uyarısında Web kancasının nasıl ayarlanacağı açıklanır. Ayrıca, bir Web kancası için HTTP POST yükünün nasıl göründüğünü gösterir. Azure etkinlik günlüğü uyarısı için kurulum ve şema (olaylar üzerinde uyarı) hakkında daha fazla bilgi için bkz. [Azure etkinlik günlüğü uyarısında Web kancası çağırma](alerts-log-webhook.md).

Azure uyarıları, uyarı içeriğini JSON biçiminde, uyarıyı oluştururken sağladığınız bir Web kancası URI 'sine göndermek için HTTP POST kullanır. Şema Bu makalenin ilerleyen bölümlerinde tanımlanmıştır. URI geçerli bir HTTP veya HTTPS uç noktası olmalıdır. Azure, bir uyarı etkinleştirildiğinde istek başına bir giriş gönderir.

## <a name="configure-webhooks-via-the-azure-portal"></a>Web kancalarını Azure portal aracılığıyla yapılandırma
Web kancası URI 'sini eklemek veya güncelleştirmek için, [Azure Portal](https://portal.azure.com/) **uyarı oluştur/güncelleştir**' e gidin.

![Uyarı kuralı bölmesi Ekle](./media/alerts-webhooks/Alertwebhook.png)

Ayrıca, [Azure PowerShell cmdlet](../../azure-monitor/platform/powershell-quickstart-samples.md#create-metric-alerts)'leri, [platformlar arası CLI](../../azure-monitor/platform/cli-samples.md#work-with-alerts)veya [Azure izleyici REST API 'LERINI](https://msdn.microsoft.com/library/azure/dn933805.aspx)kullanarak bir Web kancası URI 'sine gönderilecek bir uyarı yapılandırabilirsiniz.

## <a name="authenticate-the-webhook"></a>Web kancasının kimliğini doğrulama
Web kancası, belirteç tabanlı yetkilendirme kullanarak kimlik doğrulaması yapabilir. Web kancası URI 'SI bir belirteç KIMLIĞIYLE kaydedilir. Örneğin, `https://mysamplealert/webcallback?tokenid=sometokenid&someparameter=somevalue`

## <a name="payload-schema"></a>Yük şeması
POST işlemi, ölçüm tabanlı tüm uyarılar için aşağıdaki JSON yükünü ve şemayı içerir:

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


| Alan | Zorunlu | Düzeltilen değer kümesi | Notlar |
|:--- |:--- |:--- |:--- |
| status |E |Etkinleştirildi, çözümlendi |Ayarladığınız koşullara göre uyarının durumu. |
| context |E | |Uyarı bağlamı. |
| timestamp |E | |Uyarının tetiklendiği zaman. |
| id |E | |Her uyarı kuralının benzersiz bir KIMLIĞI vardır. |
| ad |E | |Uyarı adı. |
| açıklama |E | |Uyarının açıklaması. |
| conditionType |E |Ölçüm, olay |İki tür uyarı desteklenir: ölçüm ve olay. Ölçüm uyarıları bir ölçüm koşulunu temel alır. Olay uyarıları, etkinlik günlüğündeki bir olaya dayalıdır. Uyarının bir ölçüye veya bir olaya dayalı olup olmadığını denetlemek için bu değeri kullanın. |
| condition |E | |**ConditionType** değerine göre denetlenecek belirli alanlar. |
| metricName |Ölçüm uyarıları için | |Kuralın izleyicilerini tanımlayan ölçümün adı. |
| metricUnit |Ölçüm uyarıları için |Bayt, BytesPerSecond, sayı, Sayaçpersaniye, yüzde, saniye |Ölçümde izin verilen birim. Bkz. [izin verilen değerler](https://msdn.microsoft.com/library/microsoft.azure.insights.models.unit.aspx). |
| metricValue |Ölçüm uyarıları için | |Uyarıya neden olan ölçümün gerçek değeri. |
| threshold |Ölçüm uyarıları için | |Uyarının etkinleştirildiği eşik değeri. |
| windowSize |Ölçüm uyarıları için | |Eşik temelinde uyarı etkinliğini izlemek için kullanılan süre. Değer 5 dakika ile 1 gün arasında olmalıdır. Değer ISO 8601 Duration biçiminde olmalıdır. |
| Timetoplamasını |Ölçüm uyarıları için |Ortalama, son, maksimum, en az, yok, toplam |Toplanan verilerin zaman içinde birleştirilmesi gerekir. Varsayılan değer Average değeridir. Bkz. [izin verilen değerler](https://msdn.microsoft.com/library/microsoft.azure.insights.models.aggregationtype.aspx). |
| operator |Ölçüm uyarıları için | |Geçerli ölçüm verilerini ayarlanan eşikle karşılaştırmak için kullanılan işleç. |
| subscriptionId |E | |Azure abonelik KIMLIĞI. |
| resourceGroupName |E | |Etkilenen kaynak için kaynak grubunun adı. |
| resourceName |E | |Etkilenen kaynağın kaynak adı. |
| resourceType |E | |Etkilenen kaynağın kaynak türü. |
| resourceId |E | |Etkilenen kaynağın kaynak KIMLIĞI. |
| resourceRegion |E | |Etkilenen kaynağın bölgesi veya konumu. |
| Portal bağlantısı |E | |Portal kaynağı özet sayfasına doğrudan bağlantı. |
| properties |N |İsteğe bağlı |Olayla ilgili ayrıntıları içeren bir anahtar/değer çiftleri kümesi. Örneğin, `Dictionary<String, String>`. Özellikler alanı isteğe bağlıdır. Özel bir kullanıcı arabirimi veya mantıksal uygulama tabanlı iş akışında, kullanıcılar yük aracılığıyla geçirilebilecek anahtar/değer çiftleri girebilir. Özel özellikleri Web kancasına geri almanın alternatif bir yolu, Web kancası URI 'sinin kendisi (sorgu parametreleri olarak) aracılığıyla yapılır. |

> [!NOTE]
> **Properties** alanını yalnızca [Azure Izleyici REST API 'leri](https://msdn.microsoft.com/library/azure/dn933805.aspx)kullanarak ayarlayabilirsiniz.
>
>

## <a name="next-steps"></a>Sonraki adımlar
* Azure uyarıları ve Web kancaları hakkında daha fazla bilgi edinmek için bkz. [Azure uyarılarını Pagerharle tümleştirme](https://go.microsoft.com/fwlink/?LinkId=627080).
* Azure [uyarıları üzerinde Azure Otomasyonu betikleri (runbook 'lar) yürütmeyi](https://go.microsoft.com/fwlink/?LinkId=627081)öğrenin.
* Bir [Azure uyarısından Twilio aracılığıyla SMS iletisi göndermek için mantıksal uygulamayı nasıl kullanacağınızı](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-text-message-with-logic-app)öğrenin.
* Bir [Azure uyarısından bir bolluk iletisi göndermek için mantıksal uygulamayı nasıl kullanacağınızı](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-slack-with-logic-app)öğrenin.
* Bir Azure [uyarısından Azure kuyruğuna ileti göndermek için mantıksal uygulamayı nasıl kullanacağınızı](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-queue-with-logic-app)öğrenin.

