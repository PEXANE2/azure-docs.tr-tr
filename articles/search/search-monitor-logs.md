---
title: Günlük verilerini topla
titleSuffix: Azure Cognitive Search
description: Tanılama ayarını etkinleştirerek günlük verilerini toplayın ve çözümleyin ve sonra sonuçları araştırmak için kusto sorgu dilini kullanın.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 02/18/2020
ms.openlocfilehash: 86e869bc08552ea11728c508486a4784eccf4042
ms.sourcegitcommit: 6ee876c800da7a14464d276cd726a49b504c45c5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/19/2020
ms.locfileid: "77462385"
---
# <a name="collect-and-analyze-log-data-for-azure-cognitive-search"></a>Azure Bilişsel Arama günlük verilerini toplayın ve çözümleyin

Tanılama veya işletimsel Günlükler Azure Bilişsel Arama ayrıntılı işlemlerine ilişkin öngörüler sağlar ve hizmet ve iş yükü işlemlerini izlemek için yararlıdır. Dahili olarak, bir destek bileti verirseniz araştırma ve Analize yetecek kadar, günlük arka uçta kısa bir süre boyunca Günlükler bulunur. Ancak, işletimsel veriler üzerinde kendinden yönle isterseniz, günlük bilgilerinin nerede toplandığını belirtmek için bir tanılama ayarı yapılandırmanız gerekir.

Günlükleri ayarlama işlemi, tanılama ve işletimsel geçmişi koruma için yararlıdır. Günlüğe kaydetmeyi etkinleştirdikten sonra sorguları çalıştırabilir veya yapılandırılmış analiz için raporları oluşturabilirsiniz.

Aşağıdaki tabloda, verileri toplama ve kalıcı hale getirme seçenekleri numaralandırılır.

| Kaynak | Kullanıldığı yerler |
|----------|----------|
| [Log Analytics çalışma alanına gönder](https://docs.microsoft.com/azure/azure-monitor/learn/tutorial-resource-logs) | Olaylar ve ölçümler Log Analytics çalışma alanına gönderilir ve bu, ayrıntılı bilgi döndürmek için portalda sorgulanabilir. Giriş için bkz. [Azure izleyici günlükleri ile çalışmaya başlama](https://docs.microsoft.com/azure/azure-monitor/learn/tutorial-viewdata) |
| [BLOB depolama ile arşivleme](https://docs.microsoft.com/azure/storage/blobs/storage-blobs-overview) | Olaylar ve ölçümler bir blob kapsayıcısına arşivlenir ve JSON dosyalarında depolanır. Günlükler, belirli bir olayı araştırmak için faydalı olan ancak açık uçlu araştırma için kullanışlı olan oldukça ayrıntılı olabilir (saat/dakika). Ham günlük dosyasını görüntülemek için bir JSON düzenleyicisi kullanın veya günlük verilerini toplamak ve görselleştirmek için Power BI.|
| [Olay Hub 'ına akış](https://docs.microsoft.com/azure/event-hubs/) | Olaylar ve ölçümler bir Azure Event Hubs hizmetine akışla kaydedilir. Çok büyük Günlükler için bunu alternatif bir veri toplama hizmeti olarak seçin. |

Azure aboneliklerinizin kullanım ömrü boyunca ücretsiz olarak deneyebilmeniz için Azure Izleyici günlükleri ve BLOB depolama alanı ücretsiz bir hizmet olarak kullanılabilir. Application Insights, uygulama veri boyutu belirli limitlerin altında olduğu sürece, kaydolmak ve kullanmak ücretsizdir (Ayrıntılar için [fiyatlandırma sayfasına](https://azure.microsoft.com/pricing/details/monitor/) bakın).

## <a name="prerequisites"></a>Önkoşullar

Log Analytics veya Azure Storage kullanıyorsanız, kaynakları önceden oluşturabilirsiniz.

+ [Log Analytics çalışma alanı oluşturma](https://docs.microsoft.com/azure/azure-monitor/learn/quick-create-workspace)

+ [Depolama hesabı oluşturma](https://docs.microsoft.com/azure/storage/common/storage-quickstart-create-account)

## <a name="enable-data-collection"></a>Veri toplamayı etkinleştirme

Tanılama ayarları, günlüğe kaydedilen olayların ve ölçümlerin nasıl toplandığını belirtir.

1. **İzleme** bölümünde **Tanılama ayarları**'nı seçin.

   ![Tanılama ayarları](./media/search-monitor-usage/diagnostic-settings.png "Tanılama ayarları")

1. **+ Tanılama ayarı Ekle** ' yi seçin

1. **Log Analytics**denetleyin, çalışma alanınızı seçin ve **Operationlogs** ve **allölçümler**' i seçin.

   ![Veri toplamayı yapılandır](./media/search-monitor-usage/configure-storage.png "Veri toplamayı yapılandır")

1. Ayarı kaydedin.

1. Günlüğe kaydetme etkinleştirildikten sonra, günlükleri ve ölçümleri oluşturmaya başlamak için arama hizmetinizi kullanın. Günlüğe kaydedilen olaylar ve ölçümler kullanılabilir hale gelmeden önce zaman alır.

Log Analytics için, verilerin kullanılabilir olması birkaç dakika sürer ve sonrasında verileri döndürmek için kusto sorguları çalıştırabilirsiniz. Daha fazla bilgi için bkz. [sorgu Isteklerini izleme](search-monitor-logs.md).

BLOB depolama için, kapsayıcının blob depolamada görünmesi için bir saat sürer. Kapsayıcı başına saatlik bir blob yok. Kapsayıcılar yalnızca günlüğe kaydedilecek veya ölçülecek bir etkinlik olduğunda oluşturulur. Veriler bir depolama hesabına kopyalandığında, veriler JSON olarak biçimlendirilir ve iki kapsayıcıya yerleştirilir:

+ ınsights günlükleri operationlogs: arama trafiği günlükleri
+ ınsights ölçümleri pt1m: ölçümler için

## <a name="query-log-information"></a>Sorgu günlüğü bilgileri

Tanılama günlüklerinde iki tablo, Azure Bilişsel Arama yönelik Günlükler ve ölçümler içerir: **AzureDiagnostics** ve **AzureMetrics**.

1. **İzleme**altında **Günlükler**' i seçin.

1. Sorgu penceresinde **AzureMetrics** girin. Bu tabloda toplanan verilerle tanışın almak için bu basit sorguyu çalıştırın. Ölçümleri ve değerleri görüntülemek için tablo boyunca ilerleyin. En üstteki kayıt sayısına dikkat edin ve hizmetiniz bir süredir ölçüm topmışsa, yönetilebilir bir veri kümesini almak için zaman aralığını ayarlamak isteyebilirsiniz.

   ![AzureMetrics tablosu](./media/search-monitor-usage/azuremetrics-table.png "AzureMetrics tablosu")

1. Tablosal sonuç kümesi döndürmek için aşağıdaki sorguyu girin.

   ```
   AzureMetrics
    | project MetricName, Total, Count, Maximum, Minimum, Average
   ```

1. Daha ilginç bilgiler çıkaran daha seçmeli bir sorgu ile, tüm sütunları bilgilendirme amacıyla döndürmek için **AzureDiagnostics** ile başlayan önceki adımları yineleyin.

   ```
   AzureDiagnostics
   | project OperationName, resultSignature_d, DurationMs, Query_s, Documents_d, IndexName_s
   | where OperationName == "Query.Search" 
   ```

   ![AzureDiagnostics tablosu](./media/search-monitor-usage/azurediagnostics-table.png "AzureDiagnostics tablosu")

## <a name="log-schema"></a>Günlüğü şeması

Azure Bilişsel Arama günlük verilerini içeren veri yapıları aşağıdaki şemayla uyumlu değildir. 

BLOB depolama için, her Blobun bir dizi günlük nesnesi içeren **kayıtlar** adlı bir kök nesnesi vardır. Her blob aynı saat boyunca gerçekleşen tüm işlemler için kayıtlar içerir.

Aşağıdaki tablo, tanılama günlük kaydı ile ortak olan alanların kısmi bir listesidir.

| Adı | Tür | Örnek | Notlar |
| --- | --- | --- | --- |
| timeGenerated |datetime |"2018-12-07T00:00:43.6872559 Z" |İşlemin zaman damgası |
| resourceId |string |"/ SUBSCRIPTIONS/11111111-1111-1111-1111-111111111111 /<br/>VARSAYILAN/RESOURCEGROUPS/SAĞLAYICILARI /<br/> MICROSOFT. ARAMA/SEARCHSERVICES/SEARCHSERVICE" |ResourceId |
| operationName |string |"Query.Search" |İşlem adı |
| operationVersion |string |"2019-05-06" |Kullanılan api-version |
| category |string |"OperationLogs" |sabiti |
| resultType |string |"Başarılı" |Olası değerler: başarı veya başarısızlık |
| resultSignature |int |200 |HTTP Sonuç kodu |
| süre (MS) |int |50 |Milisaniye cinsinden işlem süresi |
| properties |object |aşağıdaki tabloya bakın |İşlem özgü verileri içeren nesne. |

### <a name="properties-schema"></a>Özellikler şeması

Aşağıdaki özellikler Azure Bilişsel Arama özgüdür.

| Adı | Tür | Örnek | Notlar |
| --- | --- | --- | --- |
| Description_s |string |"/İndexes('content')/docs Al" |İşlemin bitiş noktası |
| Documents_d |int |42 |İşlenen belge sayısı |
| IndexName_s |string |"test-Dizin" |İşlemle ilişkili dizinin adı |
| Query_s |string |"? Search = AzureSearch & $count = true & api-Version = 2019-05-06" |Sorgu parametreleri |

## <a name="metrics-schema"></a>Ölçüm şeması

Ölçümler, sorgu istekleri için yakalanır ve bir dakikalık aralıklarla ölçülür. Her ölçüm, dakika başına en düşük, en yüksek ve ortalama değerleri gösterir. Daha fazla bilgi için bkz. [sorgu Isteklerini izleme](search-monitor-queries.md).

| Adı | Tür | Örnek | Notlar |
| --- | --- | --- | --- |
| resourceId |string |"/ SUBSCRIPTIONS/11111111-1111-1111-1111-111111111111 /<br/>VARSAYILAN/RESOURCEGROUPS/SAĞLAYICILARI /<br/>MICROSOFT. ARAMA/SEARCHSERVICES/SEARCHSERVICE" |Kaynak KIMLIĞINIZ |
| metricName |string |"Gecikme süresi" |Ölçüm adı |
| time |datetime |"2018-12-07T00:00:43.6872559 Z" |işlemin zaman damgası |
| ortalama |int |64 |Ölçüm zaman aralığındaki ham örneklerin ortalama değeri, ölçüye göre saniye veya yüzde cinsinden birimler. |
| en az |int |37 |Ölçüm zaman aralığı içindeki (saniye cinsinden) ham örneklerin en küçük değeri. |
| maksimum |int |78 |Ölçüm zaman aralığı (saniye cinsinden) içindeki ham örneklerin maksimum değeri.  |
| toplam |int |258 |Ölçüm zaman aralığı içindeki ham örneklerin toplam değeri, birim (saniye cinsinden).  |
| count |int |4 |Bir düğümden günlüğe bir dakika aralığı içinde oluşturulan ölçüm sayısı.  |
| timegrain |string |"PT1M" |ISO 8601 ' de ölçümün zaman dilimi. |

Sorguların milisaniye cinsinden yürütülmesi yaygındır. bu nedenle, yalnızca saniyeler olarak ölçülecek sorgular QPS gibi bir ölçümde görünür.

**Saniye başına arama sorgusu** için en az, bu dakika içinde kayıtlı olan saniye başına arama sorgularının en düşük değeridir. Aynı en yüksek değeri için geçerlidir. Toplama tamamı ortalama, dakikadır. Örneğin, bir dakika içinde şu şekilde bir düzene sahip olabilirsiniz: en yüksek bir ikinci yük olan SearchQueriesPerSecond için maksimum, ortalama yükün 58 saniye ve son olarak tek bir sorgu ile bir saniye, en az bir sorgu ile.

**Kısıtlanmış arama sorguları yüzdesi**, minimum, maksimum, ortalama ve toplam için aynı değer vardır: bir dakika boyunca toplam arama sorgusu sayısından kısıtlanmış olan arama sorgularının yüzdesi.

## <a name="view-raw-log-files"></a>Ham günlük dosyalarını görüntüle

BLOB depolama, günlük dosyalarını arşivlemek için kullanılır. Günlük dosyasını görüntülemek için herhangi bir JSON düzenleyicisi kullanabilirsiniz. Hesabınız yoksa [Visual Studio Code](https://code.visualstudio.com/download)önerilir.

1. Azure portal, depolama hesabınızı açın. 

2. Sol gezinti bölmesinde **Bloblar**' a tıklayın. **Öngörüler-logs-operationlogs** ve **Insights-ölçümler-pt1m**görmeniz gerekir. Bu kapsayıcılar, günlük verileri blob depolamaya aktarıldığında Azure Bilişsel Arama tarafından oluşturulur.

3. . JSON dosyasına ulaşana kadar klasör hiyerarşisini aşağı tıklayın.  Dosyayı indirmek için bağlam menüsünü kullanın.

Dosya indirildikten sonra içeriği görüntülemek için bir JSON düzenleyicisinde açın.

## <a name="next-steps"></a>Sonraki adımlar

Daha önce yapmadıysanız, daha fazla bakış özelliği hakkında bilgi edinmek için arama hizmeti izlemenin temellerini gözden geçirin.

> [!div class="nextstepaction"]
> [Azure Bilişsel Arama işlemler ve etkinlikleri izleme](search-monitor-usage.md)