---
title: Günlük verilerini toplama
titleSuffix: Azure Cognitive Search
description: Tanılama ayarını etkinleştirerek günlük verilerini toplayın ve analiz edin ve sonuçları keşfetmek için Kusto Sorgu Dili'ni kullanın.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 02/18/2020
ms.openlocfilehash: 86e869bc08552ea11728c508486a4784eccf4042
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77462385"
---
# <a name="collect-and-analyze-log-data-for-azure-cognitive-search"></a>Azure Bilişsel Arama için günlük verilerini toplama ve analiz

Tanılama veya operasyonel günlükler Azure Bilişsel Arama'nın ayrıntılı işlemleri hakkında bilgi sağlar ve hizmet ve iş yükü süreçlerini izlemek için yararlıdır. Dahili olarak, günlükleri kısa bir süre için arka uçta var, bir destek bileti dosya eğer soruşturma ve analiz için yeterli. Ancak, operasyonel veriler üzerinde öz-yön istiyorsanız, günlük bilgilerinin nerede toplandığını belirtmek için bir tanılama ayarını yapılandırmanız gerekir.

Günlükleri ayarlama tanılama ve çalışma geçmişini korumak için yararlıdır. Günlüğe kaydetmeyi etkinleştirdikten sonra, sorguları çalıştırabilir veya yapılandırılmış çözümleme için raporlar oluşturabilirsiniz.

Aşağıdaki tablo, veri toplama ve kalıcılık seçeneklerini toplar.

| Kaynak | Kullanıldığı yerler |
|----------|----------|
| [Log Analytics çalışma alanına gönder](https://docs.microsoft.com/azure/azure-monitor/learn/tutorial-resource-logs) | Olaylar ve ölçümler, ayrıntılı bilgileri döndürmek için portalda sorgulanabilen bir Log Analytics çalışma alanına gönderilir. Giriş için Bkz. [Azure Monitör günlükleri ile başlayın](https://docs.microsoft.com/azure/azure-monitor/learn/tutorial-viewdata) |
| [Blob depolama ile arşivleme](https://docs.microsoft.com/azure/storage/blobs/storage-blobs-overview) | Olaylar ve ölçümler bir Blob kapsayıcısına arşivlenir ve JSON dosyalarında depolanır. Günlükleri oldukça ayrıntılı olabilir (saat /dakika), belirli bir olay araştırma için yararlı ama açık uçlu soruşturma için değil. Günlük verilerini toplamak ve görselleştirmek için ham günlük dosyasını veya Power BI'yi görüntülemek için bir JSON düzenleyicisi kullanın.|
| [Akış 'dan Olay Hub'ına](https://docs.microsoft.com/azure/event-hubs/) | Etkinlikler ve ölçümler bir Azure Etkinlik Hub'ları hizmetine aktarılır. Bunu çok büyük günlükler için alternatif bir veri toplama hizmeti olarak seçin. |

Azure aboneliğinizin ömrü boyunca ücretsiz olarak deneyebilmeniz için hem Azure Monitor günlükleri hem de Blob depolama alanı ücretsiz hizmet olarak kullanılabilir. Uygulama Öngörüleri, uygulama veri boyutu belirli sınırların altında olduğu sürece kaydolmak ve kullanmakta ücretsizdir (ayrıntılar için [fiyatlandırma sayfasına](https://azure.microsoft.com/pricing/details/monitor/) bakın).

## <a name="prerequisites"></a>Ön koşullar

Günlük Analizi veya Azure Depolama kullanıyorsanız, kaynakları önceden oluşturabilirsiniz.

+ [Günlük analizi çalışma alanı oluşturma](https://docs.microsoft.com/azure/azure-monitor/learn/quick-create-workspace)

+ [Depolama hesabı oluşturma](https://docs.microsoft.com/azure/storage/common/storage-quickstart-create-account)

## <a name="enable-data-collection"></a>Veri toplamayı etkinleştirme

Tanılama ayarları, günlüğe kaydedilmiş olayların ve ölçümlerin nasıl toplandığını belirtir.

1. **İzleme** bölümünde **Tanılama ayarları**'nı seçin.

   ![Tanılama ayarları](./media/search-monitor-usage/diagnostic-settings.png "Tanılama ayarları")

1. Select **+ Tanılama ayarı ekle**

1. **Günlük Analitiğini**kontrol edin, çalışma alanınızı seçin ve **OperationLogs** ve **AllMetrics'i**seçin.

   ![Veri toplamayı yapılandırma](./media/search-monitor-usage/configure-storage.png "Veri toplamayı yapılandırma")

1. Ayarı kaydedin.

1. Günlüğe kaydetme etkinleştirildikten sonra, günlükler ve ölçümler oluşturmaya başlamak için arama hizmetinizi kullanın. Günlüğe kaydedilmiş olayların ve ölçümlerin kullanılabilir hale gelmesi zaman alacaktır.

Log Analytics için, verilerin kullanılabilir hale gelmeleri birkaç dakika sürer ve ardından verileri döndürmek için Kusto sorgularını çalıştırabilirsiniz. Daha fazla bilgi için sorgu [isteklerini izleyin.](search-monitor-logs.md)

Blob depolama için, kapsayıcıların Blob depolamaalanında görünmesi bir saat sürer. Konteyner başına saatte bir damla var. Kapsayıcılar yalnızca günlüğe kaydetmek veya ölçmek için bir etkinlik olduğunda oluşturulur. Veriler bir depolama hesabına kopyalandığında, veriler JSON olarak biçimlendirilir ve iki kapsayıcıya yerleştirilir:

+ insights-logs-operationlogs: arama trafiği günlükleri için
+ insights-metrics-pt1m: ölçümler için

## <a name="query-log-information"></a>Günlük bilgilerini sorgula

Tanılama günlüklerinde, azure bilişsel arama için iki tablo günlükleri ve ölçümleri içerir: **Azure Diagnostics** ve **AzureMetrics**.

1. **İzleme**altında, **Günlükleri**seçin.

1. Sorgu penceresine **AzureMetrics** girin. Bu tabloda toplanan verileri tanımak için bu basit sorguyu çalıştırın. Ölçümleri ve değerleri görüntülemek için tablo boyunca ilerleyin. En üstteki kayıt sayısına dikkat edin ve hizmetiniz bir süredir ölçümler topluyorsa, yönetilebilir bir veri kümesi elde etmek için zaman aralığını ayarlamak isteyebilirsiniz.

   ![AzureMetrics tablosu](./media/search-monitor-usage/azuremetrics-table.png "AzureMetrics tablosu")

1. Bir tabular sonuç kümesini döndürmek için aşağıdaki sorguyu girin.

   ```
   AzureMetrics
    | project MetricName, Total, Count, Maximum, Minimum, Average
   ```

1. Tüm sütunları bilgilendirme amacıyla döndürmek için **AzureDiagnostics** ile başlayarak önceki adımları ve ardından daha ilginç bilgiler ayıklayan daha seçici bir sorguyu yineleyin.

   ```
   AzureDiagnostics
   | project OperationName, resultSignature_d, DurationMs, Query_s, Documents_d, IndexName_s
   | where OperationName == "Query.Search" 
   ```

   ![Azure Diagnostics tablosu](./media/search-monitor-usage/azurediagnostics-table.png "Azure Diagnostics tablosu")

## <a name="log-schema"></a>Günlük şema

Azure Bilişsel Arama günlük verilerini içeren veri yapıları aşağıdaki şemaya uygundur. 

Blob depolama için, her blob günlük nesneleri bir dizi içeren **kayıtları** denilen bir kök nesne vardır. Her blob, aynı saat içinde gerçekleşen tüm işlemlerin kayıtlarını içerir.

Aşağıdaki tablo, tanısal günlüğe kaydetmeye ortak alanların kısmi bir listesidir.

| Adı | Tür | Örnek | Notlar |
| --- | --- | --- | --- |
| zamanOluşturuldu |datetime |"2018-12-07T00:00:43.6872559Z" |Operasyonun zaman damgası |
| resourceId |string |"/ABONELİk/1111111-1111-1111-1111111111/<br/>KAYNAK GRUPLARı/VARSAYıLAN/SAĞLAYıCıLAR/<br/> Microsoft. ARAMA/ARAMA HIZMETLERI/SEARCHSERVICE" |ResourceId'iniz |
| operationName |string |"Sorgula.Ara" |İşlemin adı |
| operationVersion |string |"2019-05-06" |Kullanılan api versiyonu |
| category |string |"OperationLogs" | sabiti |
| resultType |string |"Başarı" |Olası değerler: Başarı veya Başarısızlık |
| resultSignature |int |200 |HTTP sonuç kodu |
| süreMS |int |50 |Milisaniye cinsinden işlemin süresi |
| properties |object |aşağıdaki tabloya bakın |İşleme özgü verileri içeren nesne |

### <a name="properties-schema"></a>Özellikleri şema

Aşağıdaki özellikler Azure Bilişsel Arama'ya özgüdir.

| Adı | Tür | Örnek | Notlar |
| --- | --- | --- | --- |
| Description_s |string |"GET /indexes('content')/dokümanlar" |İşlemin bitiş noktası |
| Documents_d |int |42 |İşlenen belge sayısı |
| IndexName_s |string |"test indeksi" |İşlemle ilişkili dizinin adı |
| Query_s |string |"?search=AzureSearch&$count=true&api-version=2019-05-06" |Sorgu parametreleri |

## <a name="metrics-schema"></a>Ölçümler şema

Ölçümler sorgu istekleri için yakalanır ve bir dakikalık aralıklarla ölçülür. Her metrik dakika başına minimum, maksimum ve ortalama değerleri ortaya çıkarır. Daha fazla bilgi için sorgu [isteklerini izleyin.](search-monitor-queries.md)

| Adı | Tür | Örnek | Notlar |
| --- | --- | --- | --- |
| resourceId |string |"/ABONELİk/1111111-1111-1111-1111111111/<br/>KAYNAK GRUPLARı/VARSAYıLAN/SAĞLAYıCıLAR/<br/>Microsoft. ARAMA/ARAMA HIZMETLERI/SEARCHSERVICE" |kaynak kimliğiniz |
| metricName |string |"Gecikme" |metrik adı |
| time |datetime |"2018-12-07T00:00:43.6872559Z" |operasyonun zaman damgası |
| ortalama |int |64 |Metrik zaman aralığındaki ham numunelerin ortalama değeri, metrik değere bağlı olarak saniye cinsinden birimler veya yüzde. |
| minimum |int |37 |Ham numunelerin metrik zaman aralığındaki minimum değeri, saniye cinsinden birimler. |
| maksimum |int |78 |Ham numunelerin metrik zaman aralığındaki maksimum değeri, saniye cinsinden birimler.  |
| toplam |int |258 |Ham numunelerin metrik zaman aralığındaki toplam değeri, saniye cinsinden birimler.  |
| count |int |4 |Bir dakika aralığında bir düğümden günlüğe yayılan ölçümlerin sayısı.  |
| zaman dilimi |string |"PT1M" |ISO 8601'deki metrik zaman tanesi. |

Sorguların milisaniye cinsinden yürütülmesi yaygındır, bu nedenle yalnızca saniye olarak ölçen sorgular QPS gibi metrikte görünür.

İkinci metrik **Başına Arama Sorguları** için minimum, o dakika içinde kaydedilen saniye başına arama sorguları için en düşük değerdir. Aynı durum maksimum değer için de geçerlidir. Ortalama, tüm dakika boyunca toplamdır. Örneğin, bir dakika içinde, bunun gibi bir desen olabilir: SearchQueriesPerSecond için maksimum yüksek yük bir saniye, ortalama yük 58 saniye takip ve son olarak bir saniye sadece bir sorgu ile, en az.

**Daraltılmış Arama Sorguları Yüzdesi**için, en az, en yüksek, ortalama ve toplam, hepsi aynı değere sahiptir: bir dakika boyunca toplam arama sorgusu sayısından daraltılmış arama sorgularının yüzdesi.

## <a name="view-raw-log-files"></a>Ham günlük dosyalarını görüntüleme

Blob depolama günlük dosyaları arşivleme için kullanılır. Günlük dosyasını görüntülemek için herhangi bir JSON düzenleyicisini kullanabilirsiniz. Eğer yoksa, [Visual Studio Code](https://code.visualstudio.com/download)öneririz.

1. Azure portalında Depolama hesabınızı açın. 

2. Sol navigasyon bölmesinde **Blobs'ı**tıklatın. **Sen insights-logs-operationlogs** ve **insights-metrics-pt1m**görmelisiniz. Bu kapsayıcılar, günlük verileri Blob depolamasına dışa aktarıldığında Azure Bilişsel Arama tarafından oluşturulur.

3. .json dosyasına ulaşana kadar klasör hiyerarşisini tıklatın.  Dosyayı indirmek için bağlam menüsünü kullanın.

Dosya indirildikten sonra, içeriğini görüntülemek için bir JSON düzenleyicisinde açın.

## <a name="next-steps"></a>Sonraki adımlar

Bunu daha önce yapmadıysanız, tüm gözetim yetenekleri hakkında bilgi edinmek için arama hizmeti izlemenin temellerini gözden geçirin.

> [!div class="nextstepaction"]
> [Azure Bilişsel Arama'daki işlemleri ve etkinlikleri izleme](search-monitor-usage.md)