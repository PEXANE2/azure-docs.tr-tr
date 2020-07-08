---
title: Azure Kaynak günlükleri
description: Azure Kaynak günlüklerinin Azure Izleyici 'de bir Log Analytics çalışma alanına akışını nasıl sağlayacağınızı öğrenin.
author: bwren
services: azure-monitor
ms.topic: conceptual
ms.date: 12/18/2019
ms.author: bwren
ms.subservice: logs
ms.openlocfilehash: 492aae69895d62c784d15cd77405d0c52ec13e3e
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84947101"
---
# <a name="azure-resource-logs"></a>Azure Kaynak günlükleri
Azure Kaynak günlükleri, bir Azure kaynağı içinde gerçekleştirilen işlemlere ilişkin Öngörüler sağlayan [Platform günlüklerdir](platform-logs-overview.md) . Kaynak günlüklerinin içeriği, Azure hizmeti ve kaynak türüne göre farklılık gösterir. Kaynak günlükleri varsayılan olarak toplanmaz. Azure [Izleyici günlükleri](data-platform-logs.md)ile kullanmak için kaynak günlüklerini bir Log Analytics çalışma alanına göndermek üzere her bir Azure kaynağı için bir tanılama ayarı oluşturmanız gerekir, Azure 'un dışından iletmek için Azure Event Hubs veya arşivlenmek üzere Azure Storage.

Oluşturduğunuz her Azure kaynağı için otomatik olarak bir tanılama ayarı oluşturmak üzere [Azure ilkesi 'ni](deploy-scale.md) kullanma hakkında ayrıntılı bilgi için bkz. [Platform günlüklerini ve ölçümlerini farklı hedeflere göndermek için Tanılama ayarları](diagnostic-settings.md) oluşturma.

## <a name="send-to-log-analytics-workspace"></a>Log Analytics çalışma alanına gönderme
 Aşağıdakiler de dahil olmak üzere [Azure Izleyici günlüklerinin](data-platform-logs.md) özelliklerini etkinleştirmek için kaynak günlüklerini bir Log Analytics çalışma alanına gönderin:

- Kaynak günlük verilerinin Azure Izleyici tarafından toplanan diğer izleme verileriyle ilişkilendirilmesi.
- Birden çok Azure kaynağından, aboneliklerden ve kiracılardan günlük girişlerini analiz için tek bir konuma birleştirin.
- Günlük sorgularını kullanarak karmaşık analizler gerçekleştirin ve günlük verileri hakkında ayrıntılı Öngörüler elde edin.
- Karmaşık uyarı mantığı ile günlük uyarılarını kullanın.

Kaynak günlüklerini bir Log Analytics çalışma alanına göndermek için [bir tanılama ayarı oluşturun](diagnostic-settings.md) . Bu veriler, [Azure Izleyici günlüklerinin yapısı](../log-query/logs-structure.md)bölümünde açıklandığı gibi tablolarda depolanır. Kaynak günlükleri tarafından kullanılan tablolar, kaynağın kullandığı koleksiyon türüne bağlıdır:

- Azure tanılama-yazılan tüm veriler _AzureDiagnostics_ tablosuna gönderilir.
- Kaynağa özgü veriler, kaynağın her kategorisi için ayrı ayrı tabloya yazılır.

### <a name="azure-diagnostics-mode"></a>Azure tanılama modu 
Bu modda, herhangi bir tanılama ayarının tüm verileri _AzureDiagnostics_ tablosunda toplanır. Bu, bugün en çok Azure hizmeti tarafından kullanılan eski yöntemdir. Birden çok kaynak türü aynı tabloya veri gönderdiğinden, bu şema, toplanmakta olan tüm farklı veri türlerinin şemaların üst kümesidir.

Aşağıdaki veri türleri için tanılama ayarlarının aynı çalışma alanında toplandığı aşağıdaki örneği göz önünde bulundurun:

- Hizmet 1 ' in denetim günlükleri (A, B ve C sütunlarından oluşan bir şemaya sahip)  
- Hizmet 1 ' in hata günlükleri (D, E ve F sütunlarından oluşan bir şemaya sahip)  
- Hizmet 2 ' nin denetim günlükleri (G, H ve I sütunlarından oluşan bir şemaya sahip)  

AzureDiagnostics tablosu şöyle görünür:  

| ResourceProvider    | Kategori     | A  | B  | C  | D  | E  | F  | G  | H  | I  |
| -- | -- | -- | -- | -- | -- | -- | -- | -- | -- | -- |
| Microsoft. Service1 | AuditLogs    | x1 | Y1 | z1 |    |    |    |    |    |    |
| Microsoft. Service1 | Günlüklerini    |    |    |    | Q1 | W1 | E1 |    |    |    |
| Microsoft. Service2 | AuditLogs    |    |    |    |    |    |    | j1 | K1 | L1 |
| Microsoft. Service1 | Günlüklerini    |    |    |    | üç | W2 | E2 |    |    |    |
| Microsoft. Service2 | AuditLogs    |    |    |    |    |    |    | j3 | k3 | L3 |
| Microsoft. Service1 | AuditLogs    | x5 | Y5 | z5 |    |    |    |    |    |    |
| ... |

### <a name="resource-specific"></a>Kaynağa özgü
Bu modda, seçilen çalışma alanındaki tek tablolar, tanılama ayarında seçilen her kategori için oluşturulur. Bu yöntem, günlük sorgularındaki verilerle çalışmayı çok daha kolay hale getiren, şemaların ve yapısının daha iyi keşfedilmesini sağlayan, hem alma gecikmesi hem de sorgu sürelerinde performansı artıran ve belirli bir tabloda RBAC hakları verme olanağı sağladığından önerilir. Tüm Azure Hizmetleri, sonunda kaynağa özgü moda geçirilir. 

Yukarıdaki örnek, üç tablo oluşturulmasını neden olur:
 
- Tablo *Service1AuditLogs* aşağıdaki gibi:

    | Kaynak sağlayıcısı | Kategori | A | B | C |
    | -- | -- | -- | -- | -- |
    | Service1 | AuditLogs | x1 | Y1 | z1 |
    | Service1 | AuditLogs | x5 | Y5 | z5 |
    | ... |

- Tablo *Service1ErrorLogs* aşağıdaki gibi:  

    | Kaynak sağlayıcısı | Kategori | D | E | F |
    | -- | -- | -- | -- | -- | 
    | Service1 | Günlüklerini |  Q1 | W1 | E1 |
    | Service1 | Günlüklerini |  üç | W2 | E2 |
    | ... |

- Tablo *Service2AuditLogs* aşağıdaki gibi:  

    | Kaynak sağlayıcısı | Kategori | G | H | I |
    | -- | -- | -- | -- | -- |
    | Service2 | AuditLogs | j1 | K1 | L1|
    | Service2 | AuditLogs | j3 | k3 | L3|
    | ... |



### <a name="select-the-collection-mode"></a>Koleksiyon modunu seçin
Azure kaynaklarının çoğu, verileri bir seçim yapmadan **Azure tanılama** veya **kaynağa özgü modda** çalışma alanına yazar. Hangi modun kullandığı hakkında ayrıntılı bilgi edinmek için [her hizmet için belgelere](diagnostic-logs-schema.md) bakın. Tüm Azure Hizmetleri, sonunda kaynağa özgü modu kullanacaktır. Bu geçişin bir parçası olarak, bazı kaynaklar tanılama ayarında bir mod seçmenizi sağlayacak. Tüm yeni tanılama ayarları için kaynağa özgü modu belirtin çünkü bu, verileri yönetmeyi kolaylaştırır ve daha sonraki bir tarihte karmaşık geçişlere karşı kaçınmanıza yardımcı olabilir.
  
   ![Tanılama ayarları mod seçicisi](media/resource-logs-collect-workspace/diagnostic-settings-mode-selector.png)




> [!NOTE]
> Şu anda, **Azure tanılama** ve **kaynağa özgü** mod yalnızca Azure Portal tanılama ayarı yapılandırılırken seçilebilir. Ayarı CLı, PowerShell veya REST API kullanarak yapılandırırsanız, varsayılan olarak **Azure tanılama**olur.

Mevcut bir tanılama ayarını kaynağa özgü mod olarak değiştirebilirsiniz. Bu durumda, zaten toplanmış olan veriler, çalışma alanının saklama ayarına göre kaldırılana kadar _AzureDiagnostics_ tablosunda kalır. Adanmış tabloda yeni veriler toplanacak. Her iki tablo genelinde verileri sorgulamak için [UNION](https://docs.microsoft.com/azure/kusto/query/unionoperator) işlecini kullanın.

Kaynağa özgü modu destekleyen Azure hizmetleri hakkında duyurular için [Azure Updates](https://azure.microsoft.com/updates/) blogunu izlemeye devam edin.

### <a name="column-limit-in-azurediagnostics"></a>AzureDiagnostics içinde sütun sınırı
Azure Izleyici günlüklerinde herhangi bir tablo için 500 Özellik sınırı vardır. Bu sınıra ulaşıldığında, ilk 500 dışında herhangi bir özelliği olan verileri içeren tüm satırlar alma zamanında bırakılır. *AzureDiagnostics* tablosu, bu sınıra yönelik olan tüm Azure hizmetlerinin özelliklerini içerdiğinden bu sınıra açıktır.

Birden çok hizmetten kaynak günlükleri topluyorsanız, _AzureDiagnostics_ bu sınırı aşabilir ve veriler kaçırılacaktır. Tüm Azure hizmetleri kaynağa özgü modu destekleene kadar, 500 sütun sınırına ulaşma olasılığını azaltmak üzere kaynakları birden fazla çalışma alanına yazacak şekilde yapılandırmanız gerekir.

### <a name="azure-data-factory"></a>Azure Data Factory
Azure Data Factory, ayrıntılı bir Günlükler kümesi nedeniyle, çok sayıda sütun yazmak için bilinen ve _AzureDiagnostics_ 'in sınırını aşmasına neden olabilecek bir hizmettir. Kaynağa özgü mod etkinleştirilmeden önce yapılandırılan tüm Tanılama ayarları için, her bir etkinliğe karşı benzersiz olarak adlandırılan her Kullanıcı parametresi için yeni bir sütun oluşturulur. Etkinlik girişlerinin ve çıktıların ayrıntılı doğası nedeniyle daha fazla sütun oluşturulacak.
 
Günlüklerinizi kaynağa özgü modu en kısa sürede kullanmak üzere geçirmeniz gerekir. Hemen bunu yapamadıysanız, geçici bir alternatif, bu günlüklerin çalışma alanınızda toplanmakta olan diğer günlük türlerini etkileme olasılığını en aza indirmek için Azure Data Factory günlüklerini kendi çalışma alanlarında yalıtmaktır.


## <a name="send-to-azure-event-hubs"></a>Azure Event Hubs gönder
Kaynak günlüklerini bir olay hub 'ına göndererek, örneğin bir üçüncü taraf SıEM veya diğer Log Analytics çözümlerine Azure dışından gönderebilirsiniz. Olay Hub 'larından kaynak günlükleri `records` , her yükündeki kayıtları içeren bir öğesi Ile JSON biçiminde tüketilir. Şema, [Azure Kaynak günlükleri Için ortak ve hizmete özgü şemada](resource-logs-schema.md)açıklandığı gibi kaynak türüne bağlıdır.

Aşağıda bir kaynak günlüğü için Event Hubs örnek çıkış verileri verilmiştir:

```json
{
    "records": [
        {
            "time": "2019-07-15T18:00:22.6235064Z",
            "workflowId": "/SUBSCRIPTIONS/00000000-0000-0000-0000-000000000000/RESOURCEGROUPS/JOHNKEMTEST/PROVIDERS/MICROSOFT.LOGIC/WORKFLOWS/JOHNKEMTESTLA",
            "resourceId": "/SUBSCRIPTIONS/00000000-0000-0000-0000-000000000000/RESOURCEGROUPS/JOHNKEMTEST/PROVIDERS/MICROSOFT.LOGIC/WORKFLOWS/JOHNKEMTESTLA/RUNS/08587330013509921957/ACTIONS/SEND_EMAIL",
            "category": "WorkflowRuntime",
            "level": "Error",
            "operationName": "Microsoft.Logic/workflows/workflowActionCompleted",
            "properties": {
                "$schema": "2016-04-01-preview",
                "startTime": "2016-07-15T17:58:55.048482Z",
                "endTime": "2016-07-15T18:00:22.4109204Z",
                "status": "Failed",
                "code": "BadGateway",
                "resource": {
                    "subscriptionId": "00000000-0000-0000-0000-000000000000",
                    "resourceGroupName": "JohnKemTest",
                    "workflowId": "243aac67fe904cf195d4a28297803785",
                    "workflowName": "JohnKemTestLA",
                    "runId": "08587330013509921957",
                    "location": "westus",
                    "actionName": "Send_email"
                },
                "correlation": {
                    "actionTrackingId": "29a9862f-969b-4c70-90c4-dfbdc814e413",
                    "clientTrackingId": "08587330013509921958"
                }
            }
        },
        {
            "time": "2019-07-15T18:01:15.7532989Z",
            "workflowId": "/SUBSCRIPTIONS/00000000-0000-0000-0000-000000000000/RESOURCEGROUPS/JOHNKEMTEST/PROVIDERS/MICROSOFT.LOGIC/WORKFLOWS/JOHNKEMTESTLA",
            "resourceId": "/SUBSCRIPTIONS/00000000-0000-0000-0000-000000000000/RESOURCEGROUPS/JOHNKEMTEST/PROVIDERS/MICROSOFT.LOGIC/WORKFLOWS/JOHNKEMTESTLA/RUNS/08587330012106702630/ACTIONS/SEND_EMAIL",
            "category": "WorkflowRuntime",
            "level": "Information",
            "operationName": "Microsoft.Logic/workflows/workflowActionStarted",
            "properties": {
                "$schema": "2016-04-01-preview",
                "startTime": "2016-07-15T18:01:15.5828115Z",
                "status": "Running",
                "resource": {
                    "subscriptionId": "00000000-0000-0000-0000-000000000000",
                    "resourceGroupName": "JohnKemTest",
                    "workflowId": "243aac67fe904cf195d4a28297803785",
                    "workflowName": "JohnKemTestLA",
                    "runId": "08587330012106702630",
                    "location": "westus",
                    "actionName": "Send_email"
                },
                "correlation": {
                    "actionTrackingId": "042fb72c-7bd4-439e-89eb-3cf4409d429e",
                    "clientTrackingId": "08587330012106702632"
                }
            }
        }
    ]
}
```

## <a name="send-to-azure-storage"></a>Azure Depolama’ya gönderme
Kaynak günlüklerini arşivlenmek üzere sürdürmek için Azure depolama 'ya gönderin. Tanılama ayarını oluşturduktan sonra, etkin günlük kategorilerinden birinde bir olay meydana geldiğinde depolama hesabında bir depolama kapsayıcısı oluşturulur. Kapsayıcı içindeki Bloblar aşağıdaki adlandırma kuralını kullanır:

```
insights-logs-{log category name}/resourceId=/SUBSCRIPTIONS/{subscription ID}/RESOURCEGROUPS/{resource group name}/PROVIDERS/{resource provider name}/{resource type}/{resource name}/y={four-digit numeric year}/m={two-digit numeric month}/d={two-digit numeric day}/h={two-digit 24-hour clock hour}/m=00/PT1H.json
```

Örneğin, bir ağ güvenlik grubunun blobu aşağıdakine benzer bir ada sahip olabilir:

```
insights-logs-networksecuritygrouprulecounter/resourceId=/SUBSCRIPTIONS/00000000-0000-0000-0000-000000000000/RESOURCEGROUPS/TESTRESOURCEGROUP/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUP/TESTNSG/y=2016/m=08/d=22/h=18/m=00/PT1H.json
```

Her PT1H.json blobu, blob URL’sinde belirtilen saat (örneğin, h=12) içinde gerçekleşen bir JSON olay blobu içerir. Mevcut saat boyunca, olaylar meydana geldikçe PT1H.json dosyasına eklenir. Kaynak günlüğü olayları saat başına ayrı blob 'lara bölündüğü için, dakika değeri (e = 00) her zaman 00 ' dır.

Dosyadaki PT1H.jsiçinde, her olay aşağıdaki biçimde depolanır. Bu, ortak bir en üst düzey şemayı kullanır, ancak [kaynak günlükleri şemasında](diagnostic-logs-schema.md)açıklandığı gibi her bir Azure hizmeti için benzersizdir.

``` JSON
{"time": "2016-07-01T00:00:37.2040000Z","systemId": "46cdbb41-cb9c-4f3d-a5b4-1d458d827ff1","category": "NetworkSecurityGroupRuleCounter","resourceId": "/SUBSCRIPTIONS/s1id1234-5679-0123-4567-890123456789/RESOURCEGROUPS/TESTRESOURCEGROUP/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/TESTNSG","operationName": "NetworkSecurityGroupCounters","properties": {"vnetResourceGuid": "{12345678-9012-3456-7890-123456789012}","subnetPrefix": "10.3.0.0/24","macAddress": "000123456789","ruleName": "/subscriptions/ s1id1234-5679-0123-4567-890123456789/resourceGroups/testresourcegroup/providers/Microsoft.Network/networkSecurityGroups/testnsg/securityRules/default-allow-rdp","direction": "In","type": "allow","matchedConnections": 1988}}
```

> [!NOTE]
> Platform günlükleri, her olayın bir satır olduğu ve yeni bir olayı gösterdiği [JSON satırları](http://jsonlines.org/)kullanılarak BLOB depolama alanına yazılır. Bu biçim Kasım 2018 ' de uygulandı. Bu tarihten önce, [Azure izleyici platformu günlüklerine yapılan biçim değişikliği Için hazırlama](resource-logs-blob-format.md)bölümünde açıklandığı gibi bir JSON dizisi olarak blob depolamaya yazılan günlükler bir depolama hesabına arşivlendi.


## <a name="next-steps"></a>Sonraki adımlar

* [Kaynak günlükleri hakkında daha fazla bilgi edinin](platform-logs-overview.md).
* [Farklı hedeflere platform günlükleri ve ölçümleri göndermek için Tanılama ayarları oluşturun](diagnostic-settings.md).
