---
title: Azure Service Fabric Linux kümesi olaylarını izleme
description: Service Fabric platform olaylarını Syslog 'a yazarak Service Fabric Linux kümesi olaylarını izlemeyi öğrenin.
author: srrengar
ms.topic: conceptual
ms.date: 10/23/2018
ms.author: srrengar
ms.openlocfilehash: 5bd3bda71943b2ba8a34cd4fbd0b20917b875670
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "75645761"
---
# <a name="service-fabric-linux-cluster-events-in-syslog"></a>Syslog 'da Linux kümesi olaylarını Service Fabric

Service Fabric kümenizdeki önemli etkinlikleri bilgilendirmek için bir platform olayları kümesini kullanıma sunar. Sunulan olayların tam listesi [burada](service-fabric-diagnostics-event-generation-operational.md)bulunabilir. Bu olayların tüketilebilmesi için kullanabileceğiniz çeşitli yollar vardır. Bu makalede, bu olayları Syslog 'a yazmak için Service Fabric nasıl yapılandıracağınızı tartışacağız.

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../includes/azure-monitor-log-analytics-rebrand.md)]

## <a name="introduction"></a>Giriş

6,4 sürümünde, SyslogConsumer, Service Fabric platform olaylarını Linux kümeleri için Syslog 'a göndermek üzere sunulmuştur. Etkinleştirildikten sonra olaylar, Log Analytics Aracısı tarafından toplanabilecek ve gönderilebilen Syslog 'a otomatik olarak akar.

Her Syslog olayında 4 bileşen vardır
* Olanak
* Kimlik
* İleti
* Severity

SyslogConsumer, Tesis `Local0`kullanarak tüm platform olaylarını yazar. Yapılandırma yapılandırmasını değiştirerek geçerli herhangi bir tesis için güncelleştirme yapabilirsiniz. Kullanılan kimlik `ServiceFabric`. Ileti alanı, JSON 'da serileştirilmiş tüm olayı içerir, böylece bu, çeşitli araçlarla sorgulanarak tüketilebilir. 

## <a name="enable-syslogconsumer"></a>SyslogConsumer 'ı etkinleştir

SyslogConsumer 'ı etkinleştirmek için, kümenizin bir yükseltmesini yapmanız gerekir. `fabricSettings` Bölümünün aşağıdaki kodla güncelleştirilmesi gerekir. Bu kod yalnızca SyslogConsumer ile ilgili bölümleri içerir

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

Bu çağrı için değişiklikler aşağıda verilmiştir
1. Ortak bölümünde adlı `LinuxStructuredTracesEnabled`yeni bir parametre vardır. **Bu, syslog 'a gönderildiğinde Linux olaylarının yapılandırılmış ve serileştirilmesi için gereklidir.**
2. Tanılama bölümünde yeni bir ConsumerInstance: SyslogConsumer eklenmiştir. Bu, platforma bir olay müşterisi olduğunu söyler. 
3. Yeni bölüm SyslogConsumer ' ın ' a `IsEnabled` sahip `true`olması gerekir. Local0 tesis otomatik olarak kullanılacak şekilde yapılandırılmıştır. Bunu, başka bir parametre ekleyerek geçersiz kılabilirsiniz.

```json
    {
        "name": "New LogFacility",
        "value": "<Valid Syslog Facility>"
    }
```

## <a name="azure-monitor-logs-integration"></a>Azure Izleyici günlük tümleştirmesi
Bu Syslog olaylarını, Azure Izleyici günlükleri gibi bir izleme aracında okuyabilirsiniz. Azure Marketi 'ni kullanarak bu [yönergeleri] kullanarak bir Log Analytics çalışma alanı oluşturabilirsiniz. (.. /Azure-Monitor/Learn/Quick-Create-Workspace.MD) Ayrıca bu verileri toplamak ve çalışma alanına göndermek için Log Analytics aracısını kümenize eklemeniz gerekir. Bu, performans sayaçlarını toplamak için kullanılan aracıdır. 

1. `Advanced Settings` Dikey pencereye gitme

    ![Çalışma alanı ayarları](media/service-fabric-diagnostics-oms-syslog/workspace-settings.png)

2. Şuna tıklayın: `Data`
3. Şuna tıklayın: `Syslog`
4. Local0 'i izlemek üzere yapılandırın. FabricSettings içinde değiştirdiyseniz başka bir tesis ekleyebilirsiniz

    ![Syslog yapılandırma](media/service-fabric-diagnostics-oms-syslog/syslog-configure.png)
5. Sorgulama başlamak için çalışma alanı kaynağının menüsüne tıklayarak `Logs` sorgu Gezgini 'ne gidin

    ![Çalışma alanı günlükleri](media/service-fabric-diagnostics-oms-syslog/workspace-logs.png)
6. ProcessName `Syslog` `ServiceFabric` olarak görünen tabloya karşı sorgulama yapabilirsiniz. Aşağıdaki sorgu, olaydaki JSON 'ı ayrıştırmaya ve içeriğini görüntülemeye yönelik bir örnektir

```kusto
    Syslog | where ProcessName == "ServiceFabric" | extend $payload = parse_json(SyslogMessage) | project $payload
```

![Syslog sorgusu](media/service-fabric-diagnostics-oms-syslog/syslog-query.png)

Yukarıdaki örnek bir Nodebir olaydır. Olayların tam listesini [burada](service-fabric-diagnostics-event-generation-operational.md)görebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
* Performans sayaçlarını toplamak ve kapsayıcılarınız için Docker istatistiklerini ve günlüklerini toplamak için [Log Analytics aracısını](service-fabric-diagnostics-oms-agent.md) düğümleriniz üzerine dağıtın
* Azure Izleyici günlüklerinin bir parçası olarak sunulan [günlük araması ve sorgulama](../log-analytics/log-analytics-log-searches.md) özellikleriyle familiarized alın
* [Azure Izleyici günlüklerinde özel görünümler oluşturmak için görünüm tasarımcısını kullanma](../log-analytics/log-analytics-view-designer.md)
* [Azure izleyici günlük tümleştirmesi Ile syslog ile tümleştirme](../log-analytics/log-analytics-data-sources-syslog.md)için başvuru.
