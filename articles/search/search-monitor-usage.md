---
title: Operasyonları ve faaliyetleri izleme
titleSuffix: Azure Cognitive Search
description: Bir Azure Bilişsel Arama hizmetinden günlüğe kaydetmeyi etkinleştirin, sorgu etkinliği ölçümlerini, kaynak kullanımını ve diğer sistem verilerini alın.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 02/15/2020
ms.openlocfilehash: 353e00f902a7314e5e5b7c8ee03e8b925a510b26
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77462335"
---
# <a name="monitor-operations-and-activity-of-azure-cognitive-search"></a>Azure Bilişsel Arama'nın işlemlerini ve etkinliğini izleme

Bu makalede, hizmet (kaynak) düzeyinde, iş yükü düzeyinde (sorgular ve dizin oluşturma) izleme tanıtır ve kullanıcı erişimini izlemek için bir çerçeve önerir.

Spektrum genelinde, yerleşik altyapı ve Azure Monitor gibi temel hizmetlerin yanı sıra istatistikleri, sayıları ve durumu döndüren hizmet API'lerinin bir birleşimini kullanırsınız. Yeteneklerin aralığını anlamak, sorunları ortaya çıktıkça ele alabilmeniz için bir geri bildirim döngüsü oluşturmanıza yardımcı olabilir.

## <a name="use-azure-monitor"></a>Azure İzleyici’yi kullanma

Azure Bilişsel Arama da dahil olmak üzere birçok hizmet, uyarılar, ölçümler ve günlük tanılama verileri için [Azure Monitor'dan](https://docs.microsoft.com/azure/azure-monitor/) yararlanır. Azure Bilişsel Arama için yerleşik izleme altyapısı öncelikle kaynak düzeyinde izleme (hizmet durumu) ve [sorgu izleme](search-monitor-queries.md)için kullanılır.

Aşağıdaki ekran görüntüsü, portaldaki Azure Monitor özelliklerini bulmanıza yardımcı olur.

+ Ana genel bakış sayfasında bulunan **izleme** sekmesi, önemli ölçümleri bir bakışta gösterir.
+ **Etkinlik günlüğü**, Genel Bakış'ın hemen altında, kaynak düzeyindeki eylemlerle ilgili raporlar: hizmet durumu ve API anahtar istek bildirimleri.
+ **İzleme,** listenin daha aşağısında, yapılandırılabilir uyarılar, ölçümler ve tanılama günlükleri sağlar. İhtiyacınız olduğunda bunları oluşturun. Veriler toplanıp depolandıktan sonra, bilgileri öngörüler için sorgulayabilir veya görselleştirebilirsiniz.

![Bir arama hizmetinde Azure Monitör tümleştirmesi](./media/search-monitor-usage/azure-monitor-search.png
 "Bir arama hizmetinde Azure Monitör tümleştirmesi")

### <a name="precision-of-reported-numbers"></a>Bildirilen sayıların kesinliği

Portal sayfaları birkaç dakikada bir yenilenir. Bu nedenle, portalda bildirilen sayılar yaklaşık, sisteminizin istekleri ne kadar iyi hizmet olduğunu genel bir fikir vermek için tasarlanmıştır. Saniyebaşına sorgular (QPS) gibi gerçek ölçümler, sayfada gösterilen sayıdan daha yüksek veya daha düşük olabilir.

## <a name="activity-logs-and-service-health"></a>Etkinlik günlükleri ve hizmet sağlığı

[**Etkinlik günlüğü**](https://docs.microsoft.com/azure/azure-monitor/platform/activity-log-view) Azure Kaynak Yöneticisi'nden bilgi toplar ve hizmet durumu yla ilgili değişiklikler hakkında raporlar sunar. Hizmet durumuyla ilgili kritik, hata ve uyarı koşulları için etkinlik günlüğünü izleyebilirsiniz.

Sorgular, dizin oluşturma veya nesne oluşturma gibi hizmet içi görevlerde, her istek için *Yönetici Anahtarı Al* ve Sorgu Al *anahtarları* gibi genel bilgilendirme bildirimlerini görürsünüz, ancak belirli bir eylemin kendisini göremezsiniz. Bu tane hakkında bilgi için tanılama günlüğe kaydetmeyi yapılandırmanız gerekir.

**Etkinlik günlüğüne** sol gezinti bölmesinden veya üst pencere komut çubuğundaki Bildirimler'den veya **Tanıla ve sorunları çöz sayfasından** erişebilirsiniz.

## <a name="monitor-storage"></a>Depolamayı izleyin

Kaynak kullanımına ilişkin Genel Bakış sayfasına yerleşik sekmeli sayfalar rapor verir. Bu bilgiler, hizmeti kullanmaya başladığınız anda kullanılabilir hale gelir ve yapılandırma gerekmez ve sayfa birkaç dakikada bir yenilenir. 

[Üretim iş yükleri için hangi katmanı kullanacağınız](search-sku-tier.md)veya etkin [yineleme ve bölüm sayısını ayarlayıp ayarlamadığınız](search-capacity-planning.md)la ilgili kararları sonuçlandırıyorsanız, bu ölçümler kaynakların ne kadar hızlı tüketildiği ve geçerli yapılandırmanın varolan yükü ne kadar iyi işlettiğini göstererek bu kararlarda size yardımcı olabilir.

Depolamayla ilgili uyarılar şu anda kullanılamıyor; depolama tüketimi Azure Monitor'da Toplanmaz veya **AzureMetrics** tablosuna kaydedilmez. Kodunuzu depolama boyutunu denetler ve yanıtı işler, kaynakla ilgili bildirimler yayan [özel bir çözüm oluşturmanız](https://docs.microsoft.com/azure/azure-monitor/insights/solutions-creating) gerekir. Depolama ölçümleri hakkında daha fazla bilgi için [Hizmet İstatistikleri Al'a](https://docs.microsoft.com/rest/api/searchservice/get-service-statistics#response)bakın.

Portaldaki görsel izleme için **Kullanım** sekmesi, hizmet katmanı tarafından uygulanan geçerli [sınırlara](search-limits-quotas-capacity.md) göre kaynak kullanılabilirliğini gösterir. 

Aşağıdaki resimde, her türde 3 nesne ve 50 MB depolama alanı yla kapatılan ücretsiz hizmet içindir. Temel veya Standart hizmetin daha yüksek sınırları vardır ve bölüm sayılarını artırırsanız, maksimum depolama orantılı olarak artar.

![Katman sınırlarına göre kullanım durumu](./media/search-monitor-usage/usage-tab.png
 "Katman sınırlarına göre kullanım durumu")

## <a name="monitor-workloads"></a>İş yüklerini izleme

Günlüğe kaydedilmiş olaylar, dizin oluşturma ve sorgularla ilgili olanları içerir. Log Analytics'teki **Azure Diagnostics** tablosu, sorgular ve dizin oluşturmayla ilgili operasyonel verileri toplar.

Günlüğe kaydedilen verilerin çoğu salt okunur işlemler içindir. Günlükte yakalanan olmayan diğer create-update-delete işlemleri için, sistem bilgileri için arama hizmetini sorgulayabilirsiniz.

| ThrottledRequests | Açıklama |
|---------------|-------------|
| ServiceStats | Bu işlem, bir portala genel bakış sayfasını yüklendiğinde veya yenilendiğinde doldurmak için doğrudan veya dolaylı olarak çağrılan [Hizmet İstatistiklerini Almak](https://docs.microsoft.com/rest/api/searchservice/get-service-statistics)için rutin bir çağrıdır. |
| Sorgula.Ara |  Dizin karşısındaki sorgu isteklerini [bkz.](search-monitor-queries.md)|
| Dizin oluşturma.Dizin  | Bu [işlem, Belge Ekle, Güncelleştir veya Sil'e](https://docs.microsoft.com/rest/api/searchservice/addupdate-or-delete-documents)yapılan bir çağrıdır. |
| Dizin. Prototip | Bu, Veri Alma sihirbazı tarafından oluşturulan bir dizindir. |
| Dizin Oluştur.Oluştur | İçe Aktarma Verisi sihirbazı aracılığıyla açık veya örtülü olarak bir dizin oluştur. |
| Dizin leyiciler.Get | Dizinleyici çalıştırıldığında dizinleyicinin adını verir. |
| Dizin leyiciler.Durum | Dizinleyici çalıştırıldığında dizinleyici durumunu verir. |
| DataSources.Get | Dizinleyici çalıştırıldığında veri kaynağının adını verir.|
| Dizinler.Get | Dizinleyici çalıştırıldığında dizin adını verir. |

### <a name="kusto-queries-about-workloads"></a>İş yükleri hakkında Kusto sorguları

Günlüğe kaydetmeyi etkinleştirdiyseniz, Hizmetinizde ve ne zaman çalıştırılabilen işlemlerin listesi için **AzureDiagnostics'i** sorgulayabilirsiniz. Performans değişikliklerini araştırmak için etkinliği de ilişkilendirebilirsiniz.

#### <a name="example-list-operations"></a>Örnek: Liste işlemleri 

Bir işlem listesi ve her birinin sayısını döndürün.

```
AzureDiagnostics
| summarize count() by OperationName
```

#### <a name="example-correlate-operations"></a>Örnek: İşlemleri ilişkilendirme

Sorgu isteğini dizin oluşturma işlemleriyle ilişkilendirin ve işlemlerin çakıştığını görmek için veri noktalarını bir zaman grafiğinde işleyin.

```
AzureDiagnostics
| summarize OperationName, Count=count()
| where OperationName in ('Query.Search', 'Indexing.Index')
| summarize Count=count(), AvgLatency=avg(DurationMs) by bin(TimeGenerated, 1h), OperationName
| render timechart
```

### <a name="use-search-apis"></a>Arama API'lerini kullanma

Hem Azure Bilişsel Arama REST API'si hem de .NET SDK, hizmet ölçümlerine, dizin ve dizin leyici bilgilerine ve belge sayımlarına programlı erişim sağlar.

+ [GET Hizmet İstatistikleri](/rest/api/searchservice/get-service-statistics)
+ [GET Endeks İstatistikleri](/rest/api/searchservice/get-index-statistics)
+ [Belge Sayımlarını AL](/rest/api/searchservice/count-documents)
+ [GET Dizinleyici Durumu](/rest/api/searchservice/get-indexer-status)

## <a name="monitor-user-access"></a>Kullanıcı erişimini izleme

Arama dizinleri daha büyük bir istemci uygulamasının bir bileşeni olduğundan, bir dizin için kullanıcı başına erişimi denetlemek veya izlemek için yerleşik bir metodoloji yoktur. İsteklerin yönetici veya sorgu istekleri için bir istemci uygulamasından geldiği varsayılır. Yönetici okuma yazma işlemleri, tüm hizmet genelinde nesneleri oluşturma, güncelleştirme ve silme yi içerir. Salt okunur işlemler, tek bir dizin kapsamına giren belge koleksiyonuna karşı sorgulardır. 

Bu nedenle, etkinlik günlüklerinde göreceğiniz şey, yönetici anahtarlarını veya sorgu anahtarlarını kullanan aramalara yapılan atıflardır. İstemci kodundan kaynaklanan isteklere uygun anahtar dahildir. Hizmet, kimlik belirteçlerini veya kimliğe bürünme işlemlerini işleyebilir şekilde donatılmış değildir.

Kullanıcı başına yetkilendirme için iş gereksinimleri varsa, öneri Azure Active Directory ile tümleştirmedir. Kullanıcının görmemesi gereken belgelerin [arama sonuçlarını kırpmak](search-security-trimming-for-azure-search-with-aad.md) için $filter ve kullanıcı kimliklerini kullanabilirsiniz. 

Bu bilgileri, $filter parametresini içeren sorgu dizesinden ayrı olarak kaydetmenin bir yolu yoktur. Sorgu dizelerini raporlamayla ilgili ayrıntılar için [sorguları izleyin'](search-monitor-queries.md) e bakın.

## <a name="next-steps"></a>Sonraki adımlar

Azure Monitör'de akıcılık, Azure Bilişsel Arama gibi kaynaklar da dahil olmak üzere tüm Azure hizmetinin gözetimi için gereklidir. Azure Monitor'u bilmiyorsanız, kaynaklarla ilgili makaleleri gözden geçirmek için zaman ayırın. Öğreticiler ek olarak, aşağıdaki makale başlamak için iyi bir yerdir.

> [!div class="nextstepaction"]
> [Azure Monitörü ile Azure kaynaklarını izleme](https://docs.microsoft.com/azure/azure-monitor/insights/monitor-azure-resource)
