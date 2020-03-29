---
title: Azure İşlevlerinde Tüketim planı maliyetlerini tahmin etme
description: Azure'da bir Tüketim planında işlev uygulamanızı çalıştırırken karşılaşabileceğiniz maliyetleri nasıl daha iyi tahmin edebilirsiniz öğrenin.
ms.date: 9/20/2019
ms.topic: conceptual
ms.openlocfilehash: 0e3177d7c65eb1624441427f123e6f95095bdbbd
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76963997"
---
# <a name="estimating-consumption-plan-costs"></a>Tüketim planı maliyetlerinin tahmini

Şu anda Azure İşlevleri'nde çalışan bir uygulama için üç tür barındırma planı vardır ve her planın kendi fiyatlandırma modeli vardır: 

| Planlama | Açıklama |
| ---- | ----------- |
| [**Tüketim**](functions-scale.md#consumption-plan) | Yalnızca işlev uygulamanızın çalıştığı süre için ücretlendirilirsiniz. Bu plan, abonelik bazında [ücretsiz bir hibe]fiyatlandırma[sayfası] içerir.|
| [**Premium**](functions-scale.md#premium-plan) | Tüketim planıyla aynı özellikleri ve ölçekleme mekanizmasını sağlar, ancak gelişmiş performans ve VNET erişimi sağlar. Maliyet, seçtiğiniz fiyatlandırma katmanına bağlıdır. Daha fazla bilgi için Azure [İşlevler Premium planına](functions-premium-plan.md)bakın. |
| [**Özel (Uygulama Hizmeti)**](functions-scale.md#app-service-plan) <br/>(temel katman veya daha yüksek) | Özel VM'lerde veya yalıtımda çalışmanız gerektiğinde, özel görüntüler kullanın veya fazla Uygulama Hizmeti planı kapasitenizi kullanmak istediğinizde. [Düzenli Uygulama Hizmeti planı faturalandırmasını](https://azure.microsoft.com/pricing/details/app-service/)kullanır. Maliyet, seçtiğiniz fiyatlandırma katmanına bağlıdır.|

İşlev performansınızı ve maliyet gereksinimlerinizi en iyi destekleyen planı seçtiniz. Daha fazla bilgi edinmek için Azure [İşlevlerini ölçeklendirme ve barındırma](functions-scale.md)'ya bakın.

Bu makale, değişken maliyetlerle sonuçladığından, yalnızca Tüketim planıyla ilgilidir. Bu [makalede, Tüketim planı maliyet faturalandırma SSS](https://github.com/Azure/Azure-Functions/wiki/Consumption-Plan-Cost-Billing-FAQ) makalesinin yerini almaktadır.

Dayanıklı Fonksiyonlar, Tüketim planında da çalıştırılabilir. Dayanıklı İşlevler kullanırken maliyet hususları hakkında daha fazla bilgi edinmek için, [Bkz. Dayanıklı Fonksiyonlar faturalandırma.](./durable/durable-functions-billing.md)

## <a name="consumption-plan-costs"></a>Tüketim planı maliyetleri

Tek bir işlev yürütmeyürütme *maliyeti* *GB-saniye*cinsinden ölçülür. Yürütme maliyeti, bellek kullanımı ile yürütme süresi birleştirilerek hesaplanır. Daha uzun süre çalışan bir işlev, daha fazla bellek tüketen bir işlev gibi daha fazla maliyeti. 

İşlev tarafından kullanılan bellek miktarının sabit kaldığı bir durum düşünün. Bu durumda, maliyet hesaplanması basit çarpma olduğunu. Örneğin, işlevinizin 3 saniye boyunca 0,5 GB tükettiğinizi varsayın. O zaman yürütme `0.5GB * 3s = 1.5 GB-seconds`maliyeti. 

Bellek kullanımı zaman içinde değiştiğinden, hesaplama aslında zaman içinde bellek kullanımının ayrılmaz bir parçasıdır.  Sistem bu hesaplamayı, işlemin bellek kullanımını (alt işlemlerle birlikte) düzenli aralıklarla örnekleyerek yapar. [Fiyatlandırma sayfasında]belirtildiği gibi, bellek kullanımı en yakın 128 MB kovayu yuvarlanır. İşleminiz 160 MB kullanırken, 256 MB ücretlendirilirsiniz. Hesaplama, aynı işlemde birden çok eşzamanlı işlev yürütmesi olan eşzamanlılığı dikkate alır.

> [!NOTE]
> CPU kullanımı yürütme maliyetinde doğrudan dikkate alınmasa da, işlevin yürütme süresini etkilediğinde maliyet üzerinde bir etkisi olabilir.

## <a name="other-related-costs"></a>Diğer ilgili maliyetler

İşlevlerinizi herhangi bir planda çalıştırmanın toplam maliyetini tahmin ederken, İşlevler çalışma zamanının her biri ayrı olarak faturalanan birkaç başka Azure hizmeti kullandığını unutmayın. İşlev uygulamaları için fiyatlandırma hesaplanırken, diğer Azure hizmetleriyle tümlolan tümtetikleyiciler ve bağlamalar, bu ek hizmetleri oluşturmanızı ve ödemenizi gerektirir. 

Tüketim planında çalışan işlevler için toplam maliyet, işlevlerinizin yürütme maliyetinin yanı sıra bant genişliği ve ek hizmetlerin maliyetidir. 

İşlev uygulamanızın ve ilgili hizmetlerin genel maliyetlerini tahmin ederken Azure [fiyatlandırma hesaplayıcısını](https://azure.microsoft.com/pricing/calculator/?service=functions)kullanın. 

| İlgili maliyet | Açıklama |
| ------------ | ----------- |
| **Depolama hesabı** | Her işlev uygulaması, [ayrı olarak faturalandırılan](https://azure.microsoft.com/pricing/details/storage/)ilişkili bir Genel Amaçlı [Azure Depolama hesabınız](../storage/common/storage-introduction.md#types-of-storage-accounts)olması gerekir. Bu hesap, çalışma zamanı işlevleri tarafından dahili olarak kullanılır, ancak depolama tetikleyicileri ve bağlamaları için de kullanabilirsiniz. Depolama hesabınız yoksa, işlev uygulaması oluşturulduğunda sizin için bir tane oluşturulur. Daha fazla bilgi için [Depolama hesabı gereksinimlerine](storage-considerations.md#storage-account-requirements)bakın.|
| **Uygulama Bilgileri** | Fonksiyonlar, işlev uygulamalarınız için yüksek performanslı bir izleme deneyimi sağlamak için [Uygulama Öngörüleri'ne](../azure-monitor/app/app-insights-overview.md) dayanır. Gerekli olmasa da, [Uygulama Öngörüleri tümleştirmesini etkinleştirmelisiniz.](functions-monitoring.md#enable-application-insights-integration) Telemetri verilerinin ücretsiz olarak verilmesi her ay dahildir. Daha fazla bilgi için [Azure Monitörü fiyatlandırma sayfasına](https://azure.microsoft.com/pricing/details/monitor/)bakın. |
| **Ağ bant genişliği** | Aynı bölgedeki Azure hizmetleri arasında veri aktarımı için ödeme yapmazsınız. Ancak, başka bir bölgeye veya Azure dışında giden veri aktarımları için maliyete neden olabilirsiniz. Daha fazla bilgi için [Bant Genişliği fiyatlandırma ayrıntılarına](https://azure.microsoft.com/pricing/details/bandwidth/)bakın. |

## <a name="behaviors-affecting-execution-time"></a>Yürütme süresini etkileyen davranışlar

İşlevlerinizin aşağıdaki davranışları yürütme süresini etkileyebilir:

+ **Tetikleyiciler ve bağlamalar**: [İşlev bağlamalarınıza](functions-triggers-bindings.md) giriş okumak ve çıktı yazmak için alınan süre yürütme süresi olarak sayılır. Örneğin, işleviniz bir Azure depolama kuyruğuna ileti yazmak için bir çıktı bağlama kullanırsa, yürütme süreniz, işlev maliyetinin hesaplanmasına dahil olan iletiyi kuyruğa yazmak için gereken süreyi içerir. 

+ **Eş zamanlı yürütme**: Işlevinizin bir async isteğinin`await` (C#'da) sonuçlarını beklediği süre yürütme süresi olarak sayılır. GB saniyehesaplaması, işlevin başlangıç ve bitiş saatini ve bu dönemdeki bellek kullanımını temel alır. CPU etkinliği açısından bu süre içinde neler oluyor hesaplamada hesaba katılmıyor. [Dayanıklı Fonksiyonlar](durable/durable-functions-overview.md)kullanarak eşzamanlı işlemler sırasında maliyetleri düşürebilirsiniz. Orkestratör lük te seve seve harcanan zaman için faturalandırılmamanız.

## <a name="view-execution-data"></a>Yürütme verilerini görüntüleme

[Faturanızda,](/azure/billing/billing-download-azure-invoice) **toplam yürütmelerin maliyetle** ilgili verilerini - İşlevler ve **Yürütme Süresi - Fonksiyonlar'ı**ve fiili faturalanan maliyetleri görüntüleyebilirsiniz. Ancak, bu fatura verileri geçmiş bir fatura dönemine ait aylık bir toplamdır. 

İşlevlerinizin maliyet etkisini daha iyi anlamak için, işlev uygulamalarınız tarafından oluşturulan maliyetle ilgili ölçümleri görüntülemek için Azure Monitor'u kullanabilirsiniz. Bu verileri almak için [Azure portalındaki] [Azure Monitor ölçümleri gezginini](../azure-monitor/platform/metrics-getting-started.md) veya REST API'lerini kullanabilirsiniz.

### <a name="monitor-metrics-explorer"></a>İzleme ölçümleri gezgini

Tüketim planı işlev uygulamalarınız için maliyetle ilgili verileri grafik biçiminde görüntülemek için [Azure Monitor ölçümleri gezginini](../azure-monitor/platform/metrics-getting-started.md) kullanın. 

1. Arama hizmetlerinde [Azure portalının] en **üstünde, kaynaklar ve dokümanlar** `monitor` **Hizmetler**altında **Monitör'ü** arayın ve seçin.

1. Soldaki **Ölçümler'i** > seçin**Bir kaynak seçin,** ardından işlev uygulamanızı seçmek için resmin altındaki ayarları kullanın.

    ![İşlev uygulama kaynağınızı seçin](media/functions-consumption-costing/select-a-resource.png)

      
    |Ayar  |Önerilen değer  |Açıklama  |
    |---------|---------|---------|
    | Abonelik    |  Aboneliğiniz  | İşlev uygulamanızla abonelik.  |
    | Kaynak grubu     | Kaynak grubunuz  | İşlev uygulamanızı içeren kaynak grubu.   |
    | Kaynak türü     |  Uygulama Hizmetleri | İşlev uygulamaları Monitor'da Uygulama Hizmetleri örnekleri olarak gösterilir. |
    | Kaynak     |  İşlev uygulamanız  | İzlenecek fonksiyon uygulaması.        |

1. İzlenecek kaynak olarak işlev uygulamanızı seçmek için **Uygula'yı** seçin.

1. **Metrik'ten,** **Toplama**Için **İşlev Yürütme Sayısı** ve **Toplamı'nı** seçin. Bu, seçilen dönemdeki yürütme sayımlarının toplamını grafiğe ekler.

    ![Grafiğe eklemek için bir fonksiyon uygulaması ölçümü tanımlama](media/functions-consumption-costing/monitor-metrics-add-metric.png)

1. **İşlev Yürütme Birimleri'ni** grafiğe eklemek için metrik ve yineleme adımlarını 2-4 **ekle'yi** seçin. 

Elde edilen grafik, seçilen zaman aralığındaki her iki yürütme ölçümünün toplamlarını içerir ve bu durumda iki saattir.

![Fonksiyon yürütme sayımlarının ve yürütme birimlerinin grafiği](media/functions-consumption-costing/monitor-metrics-execution-sum.png)

Yürütme birimlerinin sayısı yürütme sayısından çok daha büyük olduğundan, grafik yalnızca yürütme birimlerini gösterir.

Bu grafik, MB milisaniye cinsinden `Function Execution Units` ölçülen iki saatlik bir süre içinde toplam 1,11 milyar tüketilen gösterir. GB saniyesine dönüştürmek için 1024000'e bölün. Bu örnekte, işlev `1110000000 / 1024000 = 1083.98` uygulaması GB saniye tüketildi. Bu değeri alabilir ve zaten yürütme süresi herhangi bir ücretsiz hibe kullandım varsayarak, bu iki saat in maliyeti verir [Fonksiyonlar fiyatlandırma sayfası][fiyatlandırma sayfasında]yürütme süresi geçerli fiyat ile çarpabilirsiniz. 

### <a name="azure-cli"></a>Azure CLI

[Azure CLI'nin](/cli/azure/) ölçümleri almak için komutları vardır. CLI'yi yerel bir komut ortamından veya doğrudan [Azure Bulut Bulutu'nu](../cloud-shell/overview.md)kullanarak portaldan kullanabilirsiniz. Örneğin, aşağıdaki [az monitör ölçümleri listesi](/cli/azure/monitor/metrics#az-monitor-metrics-list) komutu, daha önce kullanılan aynı zaman diliminde saatlik verileri döndürür.

Komutu çalıştıran Azure abonelik kimliğinizle değiştirdiğinizden `<AZURE_SUBSCRIPTON_ID>` emin olun.

```azurecli-interactive
az monitor metrics list --resource /subscriptions/<AZURE_SUBSCRIPTION_ID>/resourceGroups/metrics-testing-consumption/providers/Microsoft.Web/sites/metrics-testing-consumption --metric FunctionExecutionUnits,FunctionExecutionCount --aggregation Total --interval PT1H --start-time 2019-09-11T21:46:00Z --end-time 2019-09-11T23:18:00Z
```

Bu komut, aşağıdaki örnek gibi görünen bir JSON yükünü döndürür:

```json
{
  "cost": 0.0,
  "interval": "1:00:00",
  "namespace": "Microsoft.Web/sites",
  "resourceregion": "centralus",
  "timespan": "2019-09-11T21:46:00Z/2019-09-11T23:18:00Z",
  "value": [
    {
      "id": "/subscriptions/XXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXX/resourceGroups/metrics-testing-consumption/providers/Microsoft.Web/sites/metrics-testing-consumption/providers/Microsoft.Insights/metrics/FunctionExecutionUnits",
      "name": {
        "localizedValue": "Function Execution Units",
        "value": "FunctionExecutionUnits"
      },
      "resourceGroup": "metrics-testing-consumption",
      "timeseries": [
        {
          "data": [
            {
              "average": null,
              "count": null,
              "maximum": null,
              "minimum": null,
              "timeStamp": "2019-09-11T21:46:00+00:00",
              "total": 793294592.0
            },
            {
              "average": null,
              "count": null,
              "maximum": null,
              "minimum": null,
              "timeStamp": "2019-09-11T22:46:00+00:00",
              "total": 316576256.0
            }
          ],
          "metadatavalues": []
        }
      ],
      "type": "Microsoft.Insights/metrics",
      "unit": "Count"
    },
    {
      "id": "/subscriptions/XXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXX/resourceGroups/metrics-testing-consumption/providers/Microsoft.Web/sites/metrics-testing-consumption/providers/Microsoft.Insights/metrics/FunctionExecutionCount",
      "name": {
        "localizedValue": "Function Execution Count",
        "value": "FunctionExecutionCount"
      },
      "resourceGroup": "metrics-testing-consumption",
      "timeseries": [
        {
          "data": [
            {
              "average": null,
              "count": null,
              "maximum": null,
              "minimum": null,
              "timeStamp": "2019-09-11T21:46:00+00:00",
              "total": 33538.0
            },
            {
              "average": null,
              "count": null,
              "maximum": null,
              "minimum": null,
              "timeStamp": "2019-09-11T22:46:00+00:00",
              "total": 13040.0
            }
          ],
          "metadatavalues": []
        }
      ],
      "type": "Microsoft.Insights/metrics",
      "unit": "Count"
    }
  ]
}
```
Bu özel `2019-09-11T21:46` `2019-09-11T23:18`yanıt, uygulamanın 11100000000 MB milisaniye (1083,98 GB-saniye) tüketdiğini gösterir.

## <a name="determine-memory-usage"></a>Bellek kullanımını belirleme

İşlev yürütme birimleri yürütme süresi ve bellek kullanımının bir birleşimidir, bu da bellek kullanımını anlamak için zor bir metrik oluşturur. Bellek verileri şu anda Azure Monitor tarafından kullanılabilen bir metrik değildir. Ancak, uygulamanızın bellek kullanımını optimize etmek istiyorsanız, Application Insights tarafından toplanan performans sayacı verilerini kullanabilir.  

Bunu daha önce yapmadıysanız, [işlev uygulamanızda Uygulama Öngörüleri'ni etkinleştirin.](functions-monitoring.md#enable-application-insights-integration) Bu tümleştirme etkinleştirildiğinde, [bu telemetri verilerini portalda](functions-monitoring.md#query-telemetry-data)sorgulayabilirsiniz.  

**İzleme**altında, **Günlükleri (Analytics)** seçin, ardından aşağıdaki telemetri sorgusunu kopyalayın ve sorgu penceresine yapıştırın ve **Çalıştır'ı**seçin. Bu sorgu, örnekalınan her zamantoplam bellek kullanımını döndürür.

```
performanceCounters
| where name == "Private Bytes"
| project timestamp, name, value
```

Sonuçlar aşağıdaki örnek gibi görünür:

| zaman \[damgası UTC\]          | ad          | value       |
|----------------------------|---------------|-------------|
| 09.12.2019, 01:05:14\.947 | Özel Baytlar | 209,932,288 |
| 09.12.2019, 01:06:14\.994 | Özel Baytlar | 212,189,184 |
| 09.12.2019, 01:06:30\.010 | Özel Baytlar | 231,714,816 |
| 09.12.2019, 01:07:15\.040 | Özel Baytlar | 210,591,744 |
| 09.12.2019, 01:12:16\.285 | Özel Baytlar | 216,285,184 |
| 09.12.2019, 01:12:31\.376 | Özel Baytlar | 235,806,720 |

## <a name="function-level-metrics"></a>İşlev düzeyinde ölçümler

Azure Monitor, işlevler için işlev uygulaması olan kaynak düzeyindeki ölçümleri izler. Application Insights tümleştirmesi ölçümleri işlev başına temel alarak yayır. Bir işlevin ortalama süresini almak için örnek bir analiz sorgusu aşağıda verilmiştir:

```
customMetrics
| where name contains "Duration"
| extend averageDuration = valueSum / valueCount
| summarize averageDurationMilliseconds=avg(averageDuration) by name
```

| ad                       | ortalamaDurationMilliseconds |
|----------------------------|-----------------------------|
| QueueTrigger AvgDurationMs | 16\.087 087 087                     |
| QueueTrigger MaxDurationMs | 90\.249                     |
| QueueTrigger MinDurationMs | 8\.522 sayılı                      |

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Fonksiyon uygulamalarını izleme hakkında daha fazla bilgi edinin](functions-monitoring.md)

[fiyatlandırma sayfası]:https://azure.microsoft.com/pricing/details/functions/
[Azure portalında]: https://portal.azure.com
