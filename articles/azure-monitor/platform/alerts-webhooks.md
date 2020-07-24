---
title: Azure Izleyici 'de klasik ölçüm uyarısıyla bir Web kancası çağırma
description: Azure ölçüm uyarılarını diğer Azure dışı sistemlere yeniden yönlendirme hakkında bilgi edinin.
author: harelbr
ms.author: harelbr
ms.topic: conceptual
ms.date: 04/03/2017
ms.subservice: alerts
ms.openlocfilehash: 5561dfee3ede72f9cd28adbd47caf2db4e634360
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87073588"
---
# <a name="call-a-webhook-with-a-classic-metric-alert-in-azure-monitor"></a>Azure Izleyici 'de klasik ölçüm uyarısıyla bir Web kancası çağırma

Web kancalarını kullanarak, işlem sonrası veya özel eylemler için bir Azure uyarı bildirimini diğer sistemlere yönlendirebilirsiniz. Uyarı üzerinde bir Web kancasını SMS iletileri gönderen hizmetlere yönlendirmek, hataları günlüğe kaydetmek, sohbet veya mesajlaşma hizmetleri aracılığıyla bir takıma bildirmek veya diğer çeşitli eylemler için kullanabilirsiniz. 

Bu makalede bir Azure ölçüm uyarısında Web kancasının nasıl ayarlanacağı açıklanır. Ayrıca, bir Web kancası için HTTP POST yükünün nasıl göründüğünü gösterir. Azure etkinlik günlüğü uyarısı için kurulum ve şema (olaylar üzerinde uyarı) hakkında daha fazla bilgi için bkz. [Azure etkinlik günlüğü uyarısında Web kancası çağırma](alerts-log-webhook.md).

Azure uyarıları, uyarı içeriğini JSON biçiminde, uyarıyı oluştururken sağladığınız bir Web kancası URI 'sine göndermek için HTTP POST kullanır. Şema Bu makalenin ilerleyen bölümlerinde tanımlanmıştır. URI geçerli bir HTTP veya HTTPS uç noktası olmalıdır. Azure, bir uyarı etkinleştirildiğinde istek başına bir giriş gönderir.

## <a name="configure-webhooks-via-the-azure-portal"></a>Web kancalarını Azure portal aracılığıyla yapılandırma
Web kancası URI 'sini eklemek veya güncelleştirmek için, [Azure Portal](https://portal.azure.com/) **uyarı oluştur/güncelleştir**' e gidin.

![Uyarı kuralı bölmesi Ekle](./media/alerts-webhooks/Alertwebhook.png)

Ayrıca, [Azure PowerShell cmdlet](../samples/powershell-samples.md#create-metric-alerts)'leri, [platformlar arası CLI](../samples/cli-samples.md#work-with-alerts)veya [Azure izleyici REST API 'LERINI](/rest/api/monitor/alertrules)kullanarak bir Web kancası URI 'sine gönderilecek bir uyarı yapılandırabilirsiniz.

## <a name="authenticate-the-webhook"></a>Web kancasının kimliğini doğrulama
Web kancası, belirteç tabanlı yetkilendirme kullanarak kimlik doğrulaması yapabilir. Web kancası URI 'SI bir belirteç KIMLIĞIYLE kaydedilir. Örnek: `https://mysamplealert/webcallback?tokenid=sometokenid&someparameter=somevalue`

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
| durum |Y |Etkinleştirildi, çözümlendi |Ayarladığınız koşullara göre uyarının durumu. |
| bağlam |Y | |Uyarı bağlamı. |
| timestamp |Y | |Uyarının tetiklendiği zaman. |
| kimlik |Y | |Her uyarı kuralının benzersiz bir KIMLIĞI vardır. |
| name |Y | |Uyarı adı. |
| açıklama |Y | |Uyarının açıklaması. |
| ConditionType 'ı seçin |Y |Ölçüm, olay |İki tür uyarı desteklenir: ölçüm ve olay. Ölçüm uyarıları bir ölçüm koşulunu temel alır. Olay uyarıları, etkinlik günlüğündeki bir olaya dayalıdır. Uyarının bir ölçüye veya bir olaya dayalı olup olmadığını denetlemek için bu değeri kullanın. |
| koşul |Y | |**ConditionType** değerine göre denetlenecek belirli alanlar. |
| metricName |Ölçüm uyarıları için | |Kuralın izleyicilerini tanımlayan ölçümün adı. |
| metricUnit |Ölçüm uyarıları için |Bayt, BytesPerSecond, sayı, Sayaçpersaniye, yüzde, saniye |Ölçümde izin verilen birim. Bkz. [izin verilen değerler](/previous-versions/azure/reference/dn802430(v=azure.100)). |
| metricValue |Ölçüm uyarıları için | |Uyarıya neden olan ölçümün gerçek değeri. |
| eşiği |Ölçüm uyarıları için | |Uyarının etkinleştirildiği eşik değeri. |
| windowSize |Ölçüm uyarıları için | |Eşik temelinde uyarı etkinliğini izlemek için kullanılan süre. Değer 5 dakika ile 1 gün arasında olmalıdır. Değer ISO 8601 Duration biçiminde olmalıdır. |
| timeAggregation |Ölçüm uyarıları için |Ortalama, son, maksimum, en az, yok, toplam |Toplanan verilerin zaman içinde birleştirilmesi gerekir. Varsayılan değer Average değeridir. Bkz. [izin verilen değerler](/previous-versions/azure/reference/dn802410(v=azure.100)). |
| operator |Ölçüm uyarıları için | |Geçerli ölçüm verilerini ayarlanan eşikle karşılaştırmak için kullanılan işleç. |
| subscriptionId |Y | |Azure abonelik KIMLIĞI. |
| resourceGroupName |Y | |Etkilenen kaynak için kaynak grubunun adı. |
| resourceName |Y | |Etkilenen kaynağın kaynak adı. |
| resourceType |Y | |Etkilenen kaynağın kaynak türü. |
| resourceId |Y | |Etkilenen kaynağın kaynak KIMLIĞI. |
| resourceRegion |Y | |Etkilenen kaynağın bölgesi veya konumu. |
| Portal bağlantısı |Y | |Portal kaynağı özet sayfasına doğrudan bağlantı. |
| properties |N |İsteğe Bağlı |Olayla ilgili ayrıntıları içeren bir anahtar/değer çiftleri kümesi. Örneğin, `Dictionary<String, String>`. Özellikler alanı isteğe bağlıdır. Özel bir kullanıcı arabirimi veya mantıksal uygulama tabanlı iş akışında, kullanıcılar yük aracılığıyla geçirilebilecek anahtar/değer çiftleri girebilir. Özel özellikleri Web kancasına geri almanın alternatif bir yolu, Web kancası URI 'sinin kendisi (sorgu parametreleri olarak) aracılığıyla yapılır. |

> [!NOTE]
> **Properties** alanını yalnızca [Azure Izleyici REST API 'leri](/rest/api/monitor/alertrules)kullanarak ayarlayabilirsiniz.
>
>

## <a name="next-steps"></a>Sonraki adımlar
* Azure uyarıları ve Web kancaları hakkında daha fazla bilgi edinmek için bkz. [Azure uyarılarını Pagerharle tümleştirme](https://go.microsoft.com/fwlink/?LinkId=627080).
* Azure [uyarıları üzerinde Azure Otomasyonu betikleri (runbook 'lar) yürütmeyi](https://go.microsoft.com/fwlink/?LinkId=627081)öğrenin.
* Bir [Azure uyarısından Twilio aracılığıyla SMS iletisi göndermek için mantıksal uygulamayı nasıl kullanacağınızı](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-text-message-with-logic-app)öğrenin.
* Bir [Azure uyarısından bir bolluk iletisi göndermek için mantıksal uygulamayı nasıl kullanacağınızı](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-slack-with-logic-app)öğrenin.
* Bir Azure [uyarısından Azure kuyruğuna ileti göndermek için mantıksal uygulamayı nasıl kullanacağınızı](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-queue-with-logic-app)öğrenin.
