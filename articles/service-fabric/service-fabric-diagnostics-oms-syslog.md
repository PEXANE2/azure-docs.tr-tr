---
title: Azure Hizmet Dokusu'nda Linux küme olaylarını izleyin
description: Service Fabric platform olaylarını Syslog'a yazarak Service Fabric Linux küme olaylarını nasıl izleyeceğizi öğrenin.
author: srrengar
ms.topic: conceptual
ms.date: 10/23/2018
ms.author: srrengar
ms.openlocfilehash: 5bd3bda71943b2ba8a34cd4fbd0b20917b875670
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75645761"
---
# <a name="service-fabric-linux-cluster-events-in-syslog"></a>Syslog Hizmet Kumaş Linux küme olaylar

Service Fabric, kümenizdeki önemli etkinlikler hakkında sizi bilgilendirmek için bir dizi platform olayını ortaya çıkarır. Maruz kalan olayların tam [listesine buradan](service-fabric-diagnostics-event-generation-operational.md)ulaşabilirsiniz. Bu olayların tüketilmenin çeşitli yolları vardır. Bu makalede, Hizmet Kumaşı'nın bu olayları Syslog'a yazacak şekilde nasıl yapılandırılabildiğini tartışacağız.

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../includes/azure-monitor-log-analytics-rebrand.md)]

## <a name="introduction"></a>Giriş

6.4 sürümünde, SyslogConsumer Linux kümeleri için Syslog Hizmet Kumaş ı platform etkinlikleri göndermek için tanıtıldı. Bir kez açık, olaylar otomatik olarak Syslog hangi toplanabilir ve Log Analytics Agent tarafından gönderilen akacak.

Her Syslog olayının 4 bileşeni vardır
* Tesis
* Kimlik
* İleti
* Severity

SyslogConsumer Tesis `Local0`kullanarak tüm platform olayları yazar. Config config'i değiştirerek herhangi bir geçerli tesise güncelleyebilirsiniz. Kullanılan `ServiceFabric`kimlik. İleti alanı, çeşitli araçlar tarafından sorgulanabilmesi ve tüketilebilmek için JSON'da seri hale getirilebilen tüm olayı içerir. 

## <a name="enable-syslogconsumer"></a>SyslogConsumer etkinleştirin

SyslogConsumer'ı etkinleştirmek için kümenizin yükseltmesini gerçekleştirmeniz gerekir. Bölümün `fabricSettings` aşağıdaki kodla güncelleştirilmesi gerekir. Bu kodun sadece SyslogConsumer ile ilgili bölümleri içerdiğini unutmayın

```json
    "fabricSettings": [
        {
            "name": "Diagnostics",
            "parameters": [
            {
                "name": "ConsumerInstances",
                "value": "AzureWinFabCsv, AzureWinFabCrashDump, AzureTableWinFabEtwQueryable, SyslogConsumer"
            }
            ]
        },
        {
            "name": "SyslogConsumer",
            "parameters": [
            {
                "name": "ProducerInstance",
                "value": "WinFabLttProducer"
            },
            {
            "name": "ConsumerType",
            "value": "SyslogConsumer"
            },
            {
                "name": "IsEnabled",
                "value": "true"
            }
            ]
        },
        {
            "name": "Common",
            "parameters": [
            {
                "name": "LinuxStructuredTracesEnabled",
                "value": "true"
            }
            ]
        }
    ],
```

İşte dikkat çeki yle dikkat çeki
1. Ortak bölümde, yeni bir parametre `LinuxStructuredTracesEnabled`denir. **Bu, Linux olaylarının Syslog'a gönderildiğinde yapılandırılmış ve seri hale getirilmiş olması için gereklidir.**
2. Tanılama bölümünde, yeni bir ConsumerInstance: SyslogConsumer eklendi. Bu, platforma olayların başka bir tüketicisi olduğunu söyler. 
3. Yeni bölüm SyslogConsumer olarak `IsEnabled` `true`olması gerekir . Local0 tesisini otomatik olarak kullanacak şekilde yapılandırılmıştır. Başka bir parametre ekleyerek bunu geçersiz kılabilirsiniz.

```json
    {
        "name": "New LogFacility",
        "value": "<Valid Syslog Facility>"
    }
```

## <a name="azure-monitor-logs-integration"></a>Azure Monitor tümleştirmegünlüklerini kaydeder
Bu Syslog olaylarını Azure Monitor günlükleri gibi bir izleme aracında okuyabilirsiniz. Azure Marketi'ni kullanarak bu [yönergeleri] kullanarak bir Günlük Analizi çalışma alanı oluşturabilirsiniz. (.. /azure-monitor/learn/quick-create-workspace.md) Bu verileri toplamak ve çalışma alanına göndermek için Log Analytics aracısını kümenize eklemeniz gerekir. Bu, performans sayaçlarını toplamak için kullanılan aracının aynısi. 

1. Bıçak için `Advanced Settings` gidin

    ![Çalışma Alanı Ayarları](media/service-fabric-diagnostics-oms-syslog/workspace-settings.png)

2. Şuna tıklayın: `Data`
3. Şuna tıklayın: `Syslog`
4. Local0'ı İzlemek için Tesis olarak yapılandırın. FabricSettings'te değiştirdiyseniz başka bir Tesis ekleyebilirsiniz

    ![Syslog'u yapılandır](media/service-fabric-diagnostics-oms-syslog/syslog-configure.png)
5. Sorgulamabaşlatmak için çalışma alanı `Logs` kaynağının menüsünde tıklayarak sorgu gezginine gidin

    ![Çalışma alanı günlükleri](media/service-fabric-diagnostics-oms-syslog/workspace-logs.png)
6. ProcessName olarak `Syslog` arayan `ServiceFabric` tabloya karşı sorgu yapabilirsiniz. Aşağıdaki sorgu, JSON'un olaydaki ayrıştırış ve içeriğini görüntülemenin bir örneğidir.

```kusto
    Syslog | where ProcessName == "ServiceFabric" | extend $payload = parse_json(SyslogMessage) | project $payload
```

![Syslog sorgusu](media/service-fabric-diagnostics-oms-syslog/syslog-query.png)

Yukarıdaki örnek bir NodeDown olayıdır. [Burada](service-fabric-diagnostics-event-generation-operational.md)olayların tam listesini görüntüleyebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
* Performans sayaçları toplamak ve konteynerleriniz için docker istatistikleri ve günlükleri toplamak için [Log Analytics aracısını](service-fabric-diagnostics-oms-agent.md) düğümlerinize dağıtın
* Azure Monitor [günlüklerinin](../log-analytics/log-analytics-log-searches.md) bir parçası olarak sunulan günlük arama ve sorgulama özelliklerine aşina olun
* [Azure Monitor günlüklerinde özel görünümler oluşturmak için Görünüm Tasarımcısı'nı kullanma](../log-analytics/log-analytics-view-designer.md)
* Azure Monitor [syslog ile tümleştirme günlükleri](../log-analytics/log-analytics-data-sources-syslog.md)için başvuru .
