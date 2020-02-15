---
title: İşlemleri ve etkinlikleri izleme
titleSuffix: Azure Cognitive Search
description: Günlüğe kaydetmeyi etkinleştirin, sorgu etkinliği ölçümlerini, kaynak kullanımını ve Azure Bilişsel Arama hizmetinden diğer sistem verilerini alın.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 02/15/2020
ms.openlocfilehash: c4a787362089dabf9c4eda9681358e7a70d8e78a
ms.sourcegitcommit: 2823677304c10763c21bcb047df90f86339e476a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/14/2020
ms.locfileid: "77210559"
---
# <a name="monitor-operations-and-activity-of-azure-cognitive-search"></a>Azure Bilişsel Arama işlemlerini ve etkinliklerini izleme

Bu makalede, hizmet (kaynak) düzeyinde izleme iş yükü düzeyinde (sorgular ve dizin oluşturma) açıklanır ve Kullanıcı erişimini izlemek için bir çerçeve önerilir.

SPI genelinde, Azure Izleyici gibi yerleşik altyapının yanı sıra istatistik, sayı ve durum döndüren hizmet API 'Lerinin birleşimini kullanacaksınız. Özellik aralığını anlamak, ortaya çıkan sorunlara yönelik proaktif yanıtlar için etkin bir iletişim sistemi yapılandırmanıza veya oluşturmanıza yardımcı olur.

## <a name="use-azure-monitor"></a>Azure İzleyici’yi kullanma

Azure Bilişsel Arama dahil olmak üzere birçok hizmet, uyarılar, ölçümler ve günlük Tanılama verileri için [Azure izleyiciden](https://docs.microsoft.com/azure/azure-monitor/) faydalanır. Azure Bilişsel Arama için yerleşik izleme altyapısı öncelikle kaynak düzeyinde izleme (hizmet durumu) ve [sorgu izleme](search-monitor-queries.md)için kullanılır.

Aşağıdaki ekran görüntüsü portalda Azure Izleyici özelliklerini bulmanıza yardımcı olur.

+ Ana Genel Bakış sayfasında bulunan **izleme** sekmesi, önemli ölçümleri bir bakışta gösterir.
+ **Etkinlik günlüğü**, tam olarak aşağıda genel bakış, kaynak düzeyindeki eylemlerle ilgili raporlar: hizmet durumu ve API anahtar isteği bildirimleri.
+ **İzleme**, listede daha fazla azaltma, yapılandırılabilir uyarılar, ölçümler ve tanılama günlükleri sağlar. İhtiyacınız olduğunda bunları oluşturun. Veriler toplandıktan ve depolanıyorsa, Öngörüler hakkındaki bilgileri sorgulayabilir veya görselleştirebilirsiniz.

![Arama hizmetinde Azure Izleyici tümleştirmesi](./media/search-monitor-usage/azure-monitor-search.png
 "Arama hizmetinde Azure Izleyici tümleştirmesi")

### <a name="precision-of-reported-numbers"></a>Bildirilen sayıların duyarlığı

Portal sayfaları birkaç dakikada bir yenilenir. Bu nedenle, portalda bildirilen sayılar yaklaşık olarak, sisteminizin istekleri ne kadar iyi hizmet verebildiği konusunda genel bir fikir sunacak şekilde hazırlanmıştır. Saniye başına sorgu (QPS) gibi gerçek ölçümler, sayfada gösterilen sayıdan daha yüksek veya daha düşük olabilir.

## <a name="activity-logs-and-service-health"></a>Etkinlik günlükleri ve hizmet durumu

[**Etkinlik günlüğü**](https://docs.microsoft.com/azure/azure-monitor/platform/activity-log-view) , Azure Resource Manager bilgileri ve hizmet durumunu değişikliklerle ilgili raporları toplar. Hizmet durumu ile ilgili kritik, hata ve uyarı koşulları için etkinlik günlüğü 'nü izleyebilirsiniz.

Sorgular, dizin oluşturma veya nesne oluşturma gibi hizmet içi görevlerde, *yönetici anahtarı al* ve her Istek için *sorgu anahtarları al* gibi genel bilgilendirme bildirimleri görürsünüz, ancak söz konusu eylemin kendisi değildir. Bu gren hakkında bilgi edinmek için tanılama günlüğünü yapılandırmanız gerekir.

**Etkinlik günlüğüne** sol gezinti bölmesinden veya en üstteki pencere Komut çubuğundaki bildirimlerden veya **sorunları Tanıla ve çöz** sayfasından erişebilirsiniz.

## <a name="monitor-storage"></a>Depolama alanını izleme

Kaynak kullanımında genel bakış sayfası raporuna yerleştirilmiş sekmeli sayfalar. Bu bilgiler, hizmeti kullanmaya başladığınızda, hiçbir yapılandırma gerekmeden ve sayfa birkaç dakikada bir yenilendiğinde kullanılabilir hale gelir. 

[Üretim iş yükleri için hangi katmanın kullanılacağı](search-sku-tier.md)veya [etkin çoğaltma ve bölüm sayısının ayarlanmayacağı](search-capacity-planning.md)hakkında kararlar alırsanız, bu ölçümler, kaynakların ne kadar hızlı bir şekilde tüketildiğini ve geçerli yapılandırmanın var olan yükü ne kadar iyi işlediğini göstererek bu kararlara yardımcı olabilir.

Depolama ile ilgili uyarılar Şu anda kullanılamıyor; depolama alanı tüketimi toplu **AzureMetrics**veya oturum açmamış. Kaynakla ilgili bildirimleri almak için özel bir çözüm oluşturmanız gerekir.

Portalda **kullanım** sekmesi, hizmet katmanının uyguladığı geçerli [limitlere](search-limits-quotas-capacity.md) göre kaynak kullanılabilirliği gösterir. 

Aşağıdaki çizim, her bir ve 50 MB depolama alanının 3 nesnesine göre oluşan ücretsiz hizmet içindir. Temel veya standart bir hizmetin sınırları daha yüksektir ve bölüm sayılarını artırırsanız, en fazla depolama alanı orantılı bir şekilde değişir.

![Katman sınırlarına göre kullanım durumu](./media/search-monitor-usage/usage-tab.png
 "Katman sınırlarına göre kullanım durumu")

## <a name="monitor-workloads"></a>İş yüklerini izleme

Günlüğe kaydedilen olaylar, dizin oluşturma ve sorgularla ilgili olanları içerir. Log Analytics **Azure tanılama** tablosu sorgular ve dizin oluşturma ile ilgili işletimsel verileri toplar.

Günlüğe kaydedilen verilerin çoğu salt okuma işlemlerine yöneliktir. Günlükte yakalanmayan diğer oluşturma-güncelleştirme silme işlemleri için, arama hizmetini sistem bilgileri için sorgulayabilirsiniz.

| için abonelik sınırlarını aştıysanız Hizmet Azaltma gerçekleşir | Açıklama |
|---------------|-------------|
| ServiceStats | Bu işlem, bir portala genel bakış sayfası yüklendiğinde veya yenilendiğinde, doğrudan veya örtük olarak çağrılan [hizmet Istatistiklerini almak](https://docs.microsoft.com/rest/api/searchservice/get-service-statistics)için bir yordam çağrıdır. |
| Query. Search |  Bir dizinde yapılan sorgu istekleri bkz. günlüğe kaydedilen sorgular hakkında bilgi için [izleyici sorguları](search-monitor-queries.md) .|
| Dizin oluşturuluyor. Dizin  | Bu işlem, [belge eklemek, güncelleştirmek veya silmek](https://docs.microsoft.com/rest/api/searchservice/addupdate-or-delete-documents)için yapılan bir çağrıdır. |
| dizinlerde. Örneğini | Bu, verileri Içeri aktarma Sihirbazı tarafından oluşturulan bir dizindir. |
| Dizin oluşturucular. oluştur | Veri alma Sihirbazı aracılığıyla açıkça veya örtük olarak bir Dizin Oluşturucu oluşturun. |
| Indexers. Get | Dizin Oluşturucu her çalıştırıldığında bir dizin oluşturucunun adını döndürür. |
| Dizin oluşturucular. durum | Dizin Oluşturucu her çalıştırıldığında bir dizin oluşturucunun durumunu döndürür. |
| DataSources. Get | Bir Dizin Oluşturucu her çalıştırıldığında veri kaynağının adını döndürür.|
| Indexes. Get | Bir Dizin Oluşturucu çalıştırıldığında bir dizinin adını döndürür. |

### <a name="kusto-queries-about-workloads"></a>İş yükleri hakkında kusto sorguları

Günlüğe kaydetmeyi etkinleştirdiyseniz, hizmetinizdeki ve ne zaman çalıştırılan işlemlerin bir listesi için **AzureDiagnostics** sorgulama yapabilirsiniz. Ayrıca, performans değişikliklerini araştırmak için etkinlik ile bağıntı de oluşturabilirsiniz.

#### <a name="example-list-operations"></a>Örnek: listeleme işlemleri 

İşlemlerin bir listesini ve her birinin sayımını döndürün.

```
AzureDiagnostics
| summarize count() by OperationName
```

#### <a name="example-correlate-operations"></a>Örnek: işlemler bağıntılı

Sorgu isteğini dizin oluşturma işlemleriyle ilişkilendirin ve işlemlerin çakıştığı görmek için veri noktalarını bir zaman grafiğinde işleme koyun.

```
AzureDiagnostics
| summarize OperationName, Count=count()
| where OperationName in ('Query.Search', 'Indexing.Index')
| summarize Count=count(), AvgLatency=avg(DurationMs) by bin(TimeGenerated, 1h), OperationName
| render timechart
```

### <a name="use-search-apis"></a>Arama API 'Lerini kullanma

Hem Azure Bilişsel Arama REST API hem de .NET SDK, hizmet ölçümleri, dizin ve Dizin Oluşturucu bilgilerine ve belge sayılarına programlı erişim sağlar.

+ [Hizmet Istatistiklerini al](/rest/api/searchservice/get-service-statistics)
+ [Dizin Istatistiklerini al](/rest/api/searchservice/get-index-statistics)
+ [Belge sayılarını al](/rest/api/searchservice/count-documents)
+ [Dizin Oluşturucu durumunu al](/rest/api/searchservice/get-indexer-status)

## <a name="monitor-user-access"></a>Kullanıcı erişimini izleme

Arama dizinleri daha büyük bir istemci uygulamasının bileşeni olduğundan, bir dizine erişimi denetlemek için yerleşik, Kullanıcı başına yöntem yoktur. İsteklerin, yönetici ya da sorgu istekleri için bir istemci uygulamasından geldiği varsayılır. Yönetici okuma yazma işlemleri, tüm hizmet genelinde nesneleri oluşturma, güncelleştirme, silme işlemlerini içerir. Salt okuma işlemleri, tek bir dizin kapsamına alınmış belgeler koleksiyonuna yönelik sorgulardır. 

Bu nedenle günlüklerde göreceğiniz özellikler, yönetici anahtarları veya sorgu anahtarları kullanılarak yapılan çağrılardır. İlgili anahtar, istemci kodundan kaynaklanan isteklere dahildir. Hizmet, kimlik belirteçlerini veya kimliğe bürünme işlemini işleyecek şekilde donatılmış değildir.

Kullanıcı başına yetkilendirme için iş gereksinimleri mevcut olduğunda, öneri Azure Active Directory ile tümleştirilir. Kullanıcının görmemelidir belgelerinin [arama sonuçlarını kırpmak](search-security-trimming-for-azure-search-with-aad.md) için $Filter ve Kullanıcı kimliklerini kullanabilirsiniz. 

Bu bilgileri $filter parametresini içeren sorgu dizesinden ayrı olarak günlüğe kaydetme yolu yoktur. Rapor sorgu dizeleri hakkındaki ayrıntılar için bkz. [sorguları izleme](search-monitor-queries.md) .

## <a name="next-steps"></a>Sonraki adımlar

Azure Izleyici ile akıcı olma, Azure Bilişsel Arama gibi kaynaklar dahil olmak üzere tüm Azure hizmetleri hakkında daha fazla bakış için gereklidir. Azure Izleyici hakkında bilgi sahibi değilseniz kaynaklarla ilgili makaleleri gözden geçirme süresini göz atın. Öğreticilere ek olarak, aşağıdaki makale başlamak için iyi bir yerdir.

> [!div class="nextstepaction"]
> [Azure Izleyici ile Azure kaynaklarını izleme](https://docs.microsoft.com/azure/azure-monitor/insights/monitor-azure-resource)
