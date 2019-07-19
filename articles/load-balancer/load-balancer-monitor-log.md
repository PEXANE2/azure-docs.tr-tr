---
title: İşlemleri, olayları ve genel temel Load Balancer sayaçlarını izleme
titlesuffix: Azure Load Balancer
description: Uyarı olaylarını etkinleştirmeyi ve genel temel Load Balancer araştırma sistem durumu günlüğe kaydetmeyi öğrenin
services: load-balancer
documentationcenter: na
author: asudbring
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.custom: seodec18
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/10/2018
ms.author: allensu
ms.openlocfilehash: 1995ad5e8179fdee11e960c2ad0e7c03602ebd31
ms.sourcegitcommit: 9a699d7408023d3736961745c753ca3cec708f23
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/16/2019
ms.locfileid: "68274810"
---
# <a name="azure-monitor-logs-for-public-basic-load-balancer"></a>Genel temel Load Balancer Azure Izleyici günlükleri

>[!IMPORTANT] 
>Azure Load Balancer iki farklı türü destekler: Temel ve Standart. Bu makalede Temel Yük Dengeleyici anlatılmaktadır. Standart Load Balancer hakkında daha fazla bilgi için bkz. Azure Izleyici 'de çok boyutlu ölçümler aracılığıyla telemetri sunan [Standart Load Balancer genel bakış](load-balancer-standard-overview.md) .

Temel yük dengeleyiciler yönetmek ve sorunlarını gidermek için Azure 'da farklı türlerde Günlükler kullanabilirsiniz. Bu günlüklerden bazılarına Portal üzerinden erişilebilir. Tüm Günlükler Azure Blob depolamadan ayıklanıp Excel ve PowerBI gibi farklı araçlarda görüntülenebilir. Aşağıdaki listeden farklı türlerdeki Günlükler hakkında daha fazla bilgi edinebilirsiniz.

* **Denetim günlükleri:** Azure aboneliğinizdeki tüm işlemleri ve bunların durumunu görüntülemek için [Azure denetim günlüklerini](../monitoring-and-diagnostics/insights-debugging-with-events.md) (eski adıyla işletimsel Günlükler) kullanabilirsiniz. Denetim günlükleri varsayılan olarak etkindir ve Azure portal görüntülenebilir.
* **Uyarı olay günlükleri:** Yük dengeleyici tarafından oluşturulan uyarıları görüntülemek için bu günlüğü kullanabilirsiniz. Yük dengeleyicinin durum her beş dakikada bir toplanır. Bu günlük yalnızca bir yük dengeleyici uyarı olayı ortaya çıktığında yazılır.
* **Durum araştırma günlükleri:** Bu günlüğü, sistem durumu araştırma hataları nedeniyle yük dengeleyiciden istek almamış arka uç havuzunuzdaki örnek sayısı gibi, sistem durumu araştırmanız tarafından algılanan sorunları görüntülemek için kullanabilirsiniz. Bu günlük, durum araştırma durumunda bir değişiklik olduğunda yazılır.

> [!IMPORTANT]
> Azure Izleyici günlükleri Şu anda yalnızca genel temel yük dengeleyiciler için geçerlidir. Günlükler yalnızca Kaynak Yöneticisi dağıtım modelinde dağıtılan kaynaklar için kullanılabilir. Klasik dağıtım modelinde kaynaklar için günlük kullanamazsınız. Dağıtım modelleri hakkında daha fazla bilgi için bkz. [Kaynak Yöneticisi dağıtımı ve klasik dağıtımı anlama](../azure-resource-manager/resource-manager-deployment-model.md).

## <a name="enable-logging"></a>Günlü kaydını etkinleştir

Her Kaynak Yöneticisi kaynağı için denetim günlüğü otomatik olarak etkinleştirilir. Bu Günlükler aracılığıyla kullanılabilir verileri toplamaya başlamak için olay ve durum araştırma günlüğünü etkinleştirmeniz gerekir. Günlüğe kaydetmeyi etkinleştirmek için aşağıdaki adımları kullanın.

[Azure Portal](https://portal.azure.com)oturum açın. Zaten bir yük dengeleyiciniz yoksa devam etmeden önce [yük dengeleyici oluşturun](load-balancer-get-started-internet-arm-ps.md) .

1. Portalda, **Araştır**' a tıklayın.
2. **Yük dengeleyiciler**' ı seçin.

    ![Portal-yük dengeleyici](./media/load-balancer-monitor-log/load-balancer-browse.png)

3. **Tüm ayarlar**> > var olan bir yük dengeleyiciyi seçin.
4. Yük dengeleyicinin adı altındaki iletişim kutusunun sağ tarafında, **izleme**' ye kaydırın, **Tanılama**' ya tıklayın.

    ![Portal-yük dengeleyici-ayarlar](./media/load-balancer-monitor-log/load-balancer-settings.png)

5. **Tanılama** bölmesinde, **durum**' un altında, **Açık**' ı seçin.
6. **Depolama hesabı**' na tıklayın.
7. **Günlükler**altında var olan bir depolama hesabını seçin veya yeni bir tane oluşturun. Olay günlüklerinde kaç gün değer bir olay verilerinin depolanacağını öğrenmek için kaydırıcıyı kullanın. 
8. **Kaydet**’e tıklayın.

Tanılama, belirtilen depolama hesabındaki tablo depolamaya kaydedilir. Günlükler kaydediliyorsa, ilgili hiçbir günlük üretilmez.

![Portal-tanılama günlükleri](./media/load-balancer-monitor-log/load-balancer-diagnostics.png)

> [!NOTE]
> Denetim günlükleri ayrı bir depolama hesabı gerektirmez. Olay ve durum araştırma günlüğü için depolama kullanımı hizmet ücretlerine neden olur.

## <a name="audit-log"></a>Denetim günlüğü

Denetim günlüğü varsayılan olarak oluşturulur. Günlükler, Azure 'un olay günlükleri deposunda 90 gün boyunca korunur. Bu Günlükler hakkında daha fazla bilgi için [olayları ve denetim günlüklerini görüntüle](../monitoring-and-diagnostics/insights-debugging-with-events.md) makalesini okuyun.

## <a name="alert-event-log"></a>Uyarı olay günlüğü

Bu günlük yalnızca, yük dengeleyici temelinde etkinleştirildiyse oluşturulur. Olaylar JSON biçiminde günlüğe kaydedilir ve günlüğü etkinleştirdiğinizde belirttiğiniz depolama hesabında depolanır. Aşağıda bir olay örneği verilmiştir.

```json
{
    "time": "2016-01-26T10:37:46.6024215Z",
    "systemId": "32077926-b9c4-42fb-94c1-762e528b5b27",
    "category": "LoadBalancerAlertEvent",
    "resourceId": "/SUBSCRIPTIONS/XXXXXXXXXXXXXXXXX-XXXX-XXXX-XXXXXXXXX/RESOURCEGROUPS/RG7/PROVIDERS/MICROSOFT.NETWORK/LOADBALANCERS/WWEBLB",
    "operationName": "LoadBalancerProbeHealthStatus",
    "properties": {
        "eventName": "Resource Limits Hit",
        "eventDescription": "Ports exhausted",
        "eventProperties": {
            "public ip address": "40.117.227.32"
        }
    }
}
```

JSON çıktısı, yük dengeleyicinin bir uyarı oluşturduğu nedeni açıklayan *EventName* özelliğini gösterir. Bu durumda, oluşturulan uyarı kaynak IP NAT sınırları (SNAT) nedeniyle oluşan TCP bağlantı noktası tükenmesi nedeniyle oluştu.

## <a name="health-probe-log"></a>Durum araştırma günlüğü

Bu günlük yalnızca, yukarıda ayrıntılandırılan yük dengeleyici temelinde etkinleştirildiyse oluşturulur. Veriler, günlüğü etkinleştirdiğinizde belirttiğiniz depolama hesabında depolanır. ' Insights-logs-loadbalancerprobehealthstatus ' adlı bir kapsayıcı oluşturulur ve aşağıdaki veriler günlüğe kaydedilir:

```json
{
    "records":[
    {
        "time": "2016-01-26T10:37:46.6024215Z",
        "systemId": "32077926-b9c4-42fb-94c1-762e528b5b27",
        "category": "LoadBalancerProbeHealthStatus",
        "resourceId": "/SUBSCRIPTIONS/XXXXXXXXXXXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXX/RESOURCEGROUPS/RG7/PROVIDERS/MICROSOFT.NETWORK/LOADBALANCERS/WWEBLB",
        "operationName": "LoadBalancerProbeHealthStatus",
        "properties": {
            "publicIpAddress": "40.83.190.158",
            "port": "81",
            "totalDipCount": 2,
            "dipDownCount": 1,
            "healthPercentage": 50.000000
        }
    },
    {
        "time": "2016-01-26T10:37:46.6024215Z",
        "systemId": "32077926-b9c4-42fb-94c1-762e528b5b27",
        "category": "LoadBalancerProbeHealthStatus",
        "resourceId": "/SUBSCRIPTIONS/XXXXXXXXXXXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXX/RESOURCEGROUPS/RG7/PROVIDERS/MICROSOFT.NETWORK/LOADBALANCERS/WWEBLB",
        "operationName": "LoadBalancerProbeHealthStatus",
        "properties": {
            "publicIpAddress": "40.83.190.158",
            "port": "81",
            "totalDipCount": 2,
            "dipDownCount": 0,
            "healthPercentage": 100.000000
        }
    }]
}
```

JSON çıktısı, araştırma sistem durumu için temel bilgileri Özellikler alanında gösterir. *Dıaltdowncount* özelliği, başarısız araştırma yanıtları nedeniyle ağ trafiği almıyor arka uçtaki toplam örnek sayısını gösterir.

## <a name="view-and-analyze-the-audit-log"></a>Denetim günlüğünü görüntüleyin ve çözümleyin

Aşağıdaki yöntemlerden herhangi birini kullanarak denetim günlüğü verilerini görüntüleyebilir ve çözümleyebilirsiniz:

* **Azure Araçları:** Azure PowerShell, Azure komut satırı arabirimi (CLı), Azure REST API veya Azure Preview Portal aracılığıyla denetim günlüklerinden bilgi alın. Her yöntemin adım adım yönergeleri [Kaynak Yöneticisi makalesinde denetim işlemlerinde](../azure-resource-manager/resource-group-audit.md) ayrıntılı olarak açıklanmıştır.
* **Power BI:** Zaten bir [Power BI](https://powerbi.microsoft.com/pricing) hesabınız yoksa ücretsiz olarak deneyebilirsiniz. [Power BI Için Azure denetim günlükleri içerik paketini](https://powerbi.microsoft.com/documentation/powerbi-content-pack-azure-audit-logs)kullanarak verilerinizi önceden yapılandırılmış panolar ile çözümleyebilir veya görünümleri gereksinimlerinize uyacak şekilde özelleştirebilirsiniz.

## <a name="view-and-analyze-the-health-probe-and-event-log"></a>Sistem durumu araştırmasını ve olay günlüğünü görüntüleyin ve çözümleyin

Depolama hesabınıza bağlanmanız ve olay ve durum araştırma günlükleri için JSON günlüğü girdilerini almanız gerekir. JSON dosyalarını indirdikten sonra Excel, PowerBI veya herhangi bir veri görselleştirme aracında CSV 'ye dönüştürebilir ve bu verileri görüntüleyebilirsiniz.

> [!TIP]
> Visual Studio ve C# ile sabit ve değişken değerlerini değiştirme konusunda temel kavramlara hakimseniz GitHub'daki [günlük dönüştürücü araçlarını](https://github.com/Azure-Samples/networking-dotnet-log-converter) kullanabilirsiniz.

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure denetim günlüklerinizi Power BI blog gönderisi Ile görselleştirin](https://blogs.msdn.com/b/powerbi/archive/2015/09/30/monitor-azure-audit-logs-with-power-bi.aspx) .
* [Power BI ve daha fazla blog gönderisine Azure denetim günlüklerini görüntüleyin ve çözümleyin](https://azure.microsoft.com/blog/analyze-azure-audit-logs-in-powerbi-more/) .

## <a name="next-steps"></a>Sonraki adımlar

[Yük dengeleyici araştırmalarını anlama](load-balancer-custom-probe-overview.md)
