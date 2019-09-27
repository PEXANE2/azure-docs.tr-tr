---
title: Arama hizmeti için kaynak kullanımını ve sorgu ölçümlerini izleme-Azure Search
description: Günlüğe kaydetmeyi etkinleştirin, sorgu etkinliği ölçümlerini, kaynak kullanımını ve bir Azure Search hizmetinden diğer sistem verilerini alın.
author: HeidiSteen
manager: nitinme
tags: azure-portal
services: search
ms.service: search
ms.topic: conceptual
ms.date: 05/16/2019
ms.author: heidist
ms.openlocfilehash: d0c93d941047413c5056b3718f57b360357affbd
ms.sourcegitcommit: e9936171586b8d04b67457789ae7d530ec8deebe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71327151"
---
# <a name="monitor-resource-consumption-and-query-activity-in-azure-search"></a>Kaynak tüketimini ve sorgu etkinliğini izleme Azure Search

Azure Search hizmetinizin Genel Bakış sayfasında, kaynak kullanımı ve sorgu ölçümleri hakkındaki sistem verilerini ve daha fazla dizin, Dizin Oluşturucu ve veri kaynağı oluşturmak için ne kadar kota kullanılabildiğini görüntüleyebilirsiniz. Ayrıca, Log Analytics 'i veya kalıcı veri toplama için kullanılan başka bir kaynağı yapılandırmak üzere portalını kullanabilirsiniz. 

Günlükleri ayarlama, kendi kendine tanılama ve işletimsel geçmişi koruma için kullanışlıdır. Dahili olarak, bir destek bileti verirseniz araştırma ve Analize yetecek kadar, günlük arka uçta kısa bir süre boyunca Günlükler bulunur. Denetim ve günlük bilgilerine erişim istiyorsanız, bu makalede açıklanan çözümlerden birini ayarlamanız gerekir.

Bu makalede, izleme seçenekleriniz, günlüğe kaydetme ve günlük depolamayı etkinleştirme ve günlük içeriğinin nasıl görüntüleneceği hakkında bilgi edinin.

## <a name="metrics-at-a-glance"></a>Bir bakışta ölçümler

Genel Bakış sayfasında yerleşik olarak bulunan ve kaynak tüketimine ve sorgu yürütme ölçümlerinde bulunan **kullanım** ve **izleme** bölümleri. Bu bilgiler, hizmeti kullanmaya başladığınızda yapılandırma gerekmeden kullanılabilir hale gelir. Bu sayfa birkaç dakikada bir yenilenir. [Üretim iş yükleri için hangi katmanın kullanılacağı](search-sku-tier.md)veya [etkin kopyaların ve bölümlerin sayısının ayarlanmayacağı](search-capacity-planning.md)hakkında kararlar alırsanız, bu ölçümler, kaynakların ne kadar hızlı tüketiğini göstererek bu kararlara yardımcı olabilir ve geçerli yapılandırmanın mevcut yükü ne kadar iyi işlediğini.

**Kullanım** sekmesi, geçerli [limitlere](search-limits-quotas-capacity.md)göre kaynak kullanılabilirliği gösterir. Aşağıdaki çizim, her bir ve 50 MB depolama alanının 3 nesnesine göre oluşan ücretsiz hizmet içindir. Temel veya standart bir hizmetin sınırları daha yüksektir ve bölüm sayılarını artırırsanız, en fazla depolama alanı orantılı bir şekilde değişir.

![Etkin sınırlara göre kullanım durumu](./media/search-monitor-usage/usage-tab.png
 " kullanım durumu geçerli sınırlara göre @ no__t-2

## <a name="queries-per-second-qps-and-other-metrics"></a>Saniyedeki sorgu sayısı (QPS) ve diğer ölçümler

**İzleme** sekmesi, dakikada toplanan arama *sorguları* (QPS) gibi ölçümler için hareketli ortalamaları gösterir. 
*Arama gecikmesi* , arama hizmetinin dakika başına toplanan arama sorgularını işlemesi için ihtiyaç duyduğu süredir. *Kısıtlanmış arama sorguları yüzdesi* (gösterilmez), her dakika toplanmış olan arama sorgularının yüzdesidir.

Bu sayılar yaklaşık değerlerdir ve sisteminizin istekleri ne kadar iyi bir şekilde sunmakta olduğu konusunda genel bir fikir sunacak şekilde tasarlanmıştır. Gerçek QPS, portalda raporlanan sayıdan daha yüksek veya daha düşük olabilir.

Saniyede saniye başına ![etkinlik](./media/search-monitor-usage/monitoring-tab.png "sorgusu") sorgusu

## <a name="activity-logs"></a>Etkinlik günlükleri

**Etkinlik günlüğü** Azure Resource Manager bilgi toplar. Etkinlik günlüğünde bulunan bilgilere örnek olarak bir hizmet oluşturma veya silme, kaynak grubunu güncelleştirme, ad kullanılabilirliğini denetleme veya bir isteği işlemek için hizmet erişim anahtarı alma sayılabilir. 

**Etkinlik günlüğüne** sol gezinti bölmesinden veya en üstteki pencere Komut çubuğundaki bildirimlerden veya **sorunları Tanıla ve çöz** sayfasından erişebilirsiniz.

Bir dizin oluşturma veya bir veri kaynağını silme gibi hizmet içi görevlerde, her istek için "yönetici anahtarı al" gibi genel bildirimler görürsünüz, ancak söz konusu eylemin kendisi uygulanmaz. Bu bilgi düzeyi için bir eklenti izleme çözümünü etkinleştirmeniz gerekir.

## <a name="add-on-monitoring-solutions"></a>Eklenti izleme çözümleri

Azure Search, yönettiği nesnelerden daha fazla veri depolamaz, bu da günlük verilerinin dışarıdan depolanması gerektiği anlamına gelir. Günlük verilerini kalıcı hale getirmek istiyorsanız aşağıdaki kaynakları yapılandırabilirsiniz. 

Aşağıdaki tabloda, günlükleri depolama ve Application Insights aracılığıyla hizmet işlemlerinin ve sorgu iş yüklerinin derinlemesine izlenmesini ekleme seçenekleri karşılaştırılmaktadır.

| Resource | Kullanıldığı yerler |
|----------|----------|
| [Application Insights](https://docs.microsoft.com/azure/azure-monitor/app/app-insights-overview) | Aşağıdaki şemalara bağlı olarak günlüğe kaydedilen olaylar ve sorgu ölçümleri, uygulamanızdaki Kullanıcı olaylarıyla bağıntılı. Bu, kullanıcı eylemlerini veya sinyalleri, uygulama kodu tarafından gönderilen istekleri filtrelemeye karşılık Kullanıcı tarafından başlatılan aramadan eşlemek için Kullanıcı eylemleri veya sinyalleri hesaba alan tek çözümdür. Bu yaklaşımı kullanmak için, istek bilgilerini Application Insights yönlendirmek üzere izleme kodunu kaynak dosyalarınıza kopyalayın. Daha fazla bilgi için bkz. [arama trafiği analizi](search-traffic-analytics.md). |
| [Azure İzleyici günlükleri](https://docs.microsoft.com/azure/azure-monitor/log-query/log-query-overview) | Günlüğe kaydedilen olaylar ve sorgu ölçümleri aşağıdaki şemalara göre yapılır. Olaylar Log Analytics çalışma alanına kaydedilir. Günlükte ayrıntılı bilgi döndürmek için sorguları bir çalışma alanına karşı çalıştırabilirsiniz. Daha fazla bilgi için bkz. [Azure izleyici günlükleri ile çalışmaya başlama](https://docs.microsoft.com/azure/azure-monitor/learn/tutorial-viewdata) |
| [Blob depolama](https://docs.microsoft.com/azure/storage/blobs/storage-blobs-overview) | Günlüğe kaydedilen olaylar ve sorgu ölçümleri aşağıdaki şemalara göre yapılır. Olaylar bir blob kapsayıcısına kaydedilir ve JSON dosyalarında depolanır. Dosya içeriğini görüntülemek için bir JSON düzenleyicisi kullanın.|
| [Olay Hub’ı](https://docs.microsoft.com/azure/event-hubs/) | Günlüğe kaydedilen olaylar ve sorgu ölçümleri, bu makalede belgelenen şemaları temel alır. Çok büyük Günlükler için bunu alternatif bir veri toplama hizmeti olarak seçin. |

Azure aboneliklerinizin kullanım ömrü boyunca ücretsiz olarak deneyebilmeniz için Azure Izleyici günlükleri ve BLOB depolama alanı ücretsiz bir hizmet olarak kullanılabilir. Application Insights, uygulama veri boyutu belirli limitlerin altında olduğu sürece, kaydolmak ve kullanmak ücretsizdir (Ayrıntılar için [fiyatlandırma sayfasına](https://azure.microsoft.com/pricing/details/monitor/) bakın).

Sonraki bölümde, Azure Search işlemler tarafından oluşturulan günlük verilerini toplamak ve erişmek için Azure Blob depolamayı etkinleştirme ve kullanma adımlarında size yol gösterilir.

## <a name="enable-logging"></a>Günlü kaydını etkinleştir

Dizin oluşturma ve sorgu iş yükleri için günlüğe kaydetme varsayılan olarak kapalıdır ve hem günlük altyapısı hem de uzun vadeli dış depolama için eklenti çözümlerine bağımlıdır. Yalnızca Azure Search ' deki kalıcı veriler, oluşturduğu ve yönettiği nesnelerdir, bu nedenle günlüklerin başka bir yerde depolanması gerekir.

Bu bölümde, günlüğe kaydedilen olayları ve ölçüm verilerini depolamak için blob depolamayı nasıl kullanacağınızı öğreneceksiniz.

1. Henüz yoksa [bir depolama hesabı oluşturun](https://docs.microsoft.com/azure/storage/common/storage-quickstart-create-account) . Bu alıştırmada kullanılan tüm kaynakları silmek istiyorsanız, daha sonra temizlemeyi kolaylaştırmak için Azure Search aynı kaynak grubuna yerleştirebilirsiniz.

   Depolama hesabınızın Azure Search aynı bölgede mevcut olması gerekir.

2. Arama hizmeti genel bakış sayfasını açın. Sol gezinti bölmesinde **izleme** ' ye kaydırın ve **izlemeyi etkinleştir**' e tıklayın.

   ![İzlemeyi etkinleştirme](./media/search-monitor-usage/enable-monitoring.png "izlemeyi etkinleştir")

3. Dışarı aktarmak istediğiniz verileri seçin: Günlükler, ölçümler veya her ikisi. Bir depolama hesabına kopyalayabilir, bunu bir olay hub 'ına gönderebilir veya Azure Izleyici günlüklerine aktarabilirsiniz.

   Blob depolamaya Arşiv için yalnızca depolama hesabının mevcut olması gerekir. Kapsayıcılar ve Bloblar, günlük verileri aktarıldığında gerekli olduğu gibi oluşturulur.

   ![BLOB Storage Arşivi yapılandırma](./media/search-monitor-usage/configure-blob-storage-archive.png "BLOB Storage Arşivi") yapılandırma

4. Profili kaydedin.

5. Nesneleri oluşturarak veya silerek (günlük olayları oluşturur) ve sorgular göndererek (ölçüm oluşturur) test günlüğü. 

Profil kaydedildikten sonra günlüğe kaydetme etkinleştirilir. Kapsayıcılar yalnızca günlüğe kaydedilecek veya ölçülecek bir etkinlik olduğunda oluşturulur. Veriler bir depolama hesabına kopyalandığında, veriler JSON olarak biçimlendirilir ve iki kapsayıcıya yerleştirilir:

* ınsights günlükleri operationlogs: arama trafiği günlükleri
* ınsights ölçümleri pt1m: ölçümler için

**kapsayıcının blob depolamada görünmesi için bir saat sürer. Her kapsayıcı için bir blob, saat başına bir blob vardır.**

Dosyaları görüntülemek için [Visual Studio Code](#download-and-open-in-visual-studio-code) veya başka bir JSON düzenleyicisi kullanabilirsiniz. 

### <a name="example-path"></a>Örnek yol

```
resourceId=/subscriptions/<subscriptionID>/resourcegroups/<resourceGroupName>/providers/microsoft.search/searchservices/<searchServiceName>/y=2018/m=12/d=25/h=01/m=00/name=PT1H.json
```

## <a name="log-schema"></a>Günlüğü şeması
Arama hizmeti trafik günlüklerinizi içeren Bloblar, bu bölümde açıklandığı şekilde yapılandırılır. Her Blobun bir dizi günlük nesnesi içeren **kayıtlar** adlı bir kök nesnesi vardır. Her blob aynı saat boyunca gerçekleşen tüm işlemler için kayıtlar içerir.

| Name | Tür | Örnek | Notlar |
| --- | --- | --- | --- |
| time |datetime |"2018-12-07T00:00:43.6872559 Z" |İşlemin zaman damgası |
| resourceId |dize |"/ SUBSCRIPTIONS/11111111-1111-1111-1111-111111111111 /<br/>VARSAYILAN/RESOURCEGROUPS/SAĞLAYICILARI /<br/> MICROSOFT. ARAMA/SEARCHSERVICES/SEARCHSERVICE" |ResourceId |
| operationName |dize |"Query.Search" |İşlem adı |
| operationVersion |dize |"2019-05-06" |Kullanılan api-version |
| category |dize |"OperationLogs" |Sabit |
| resultType |dize |"Başarılı" |Olası değerler: Başarı veya başarısızlık |
| resultSignature |int |200 |HTTP Sonuç kodu |
| süre (MS) |int |50 |Milisaniye cinsinden işlem süresi |
| properties |object |aşağıdaki tabloya bakın |İşlem özgü verileri içeren nesne. |

**Özellik şeması**

| Ad | Tür | Örnek | Notlar |
| --- | --- | --- | --- |
| Açıklama |dize |"/İndexes('content')/docs Al" |İşlemin bitiş noktası |
| Sorgu |dize |"? Search = AzureSearch & $count = true & api-Version = 2019-05-06" |Sorgu parametreleri |
| Belgeler |int |42 |İşlenen belge sayısı |
| indexName |dize |"testindex" |İşlemle ilişkili dizinin adı |

## <a name="metrics-schema"></a>Ölçüm şeması

Ölçümler, sorgu istekleri için yakalanır.

| Name | Tür | Örnek | Notlar |
| --- | --- | --- | --- |
| resourceId |dize |"/ SUBSCRIPTIONS/11111111-1111-1111-1111-111111111111 /<br/>VARSAYILAN/RESOURCEGROUPS/SAĞLAYICILARI /<br/>MICROSOFT. ARAMA/SEARCHSERVICES/SEARCHSERVICE" |Kaynak Kimliği |
| MetricName |dize |"Gecikme süresi" |Ölçüm adı |
| time |datetime |"2018-12-07T00:00:43.6872559 Z" |işlemin zaman damgası |
| ortalama |int |64 |Ham örnekleri ölçüm zaman aralığındaki ortalama değeri |
| en az |int |37 |En düşük değer ölçüm zaman aralığında ham örnekleri |
| en fazla |int |78 |Ölçüm zaman aralığında ham örnekleri en yüksek değeri |
| toplam |int |258 |Toplam değer ölçüm zaman aralığında ham örnekleri |
| count |int |4 |Ölçü oluşturmak için kullanılan ham örnek sayısı |
| timegrain |dize |"PT1M" |ISO 8601 ölçümü, zaman dilimi |

Tüm ölçümler, bir dakikalık aralıklar olarak raporlanır. Her ölçüm, dakika başına en düşük, en yüksek ve ortalama değerleri gösterir.

SearchQueriesPerSecond ölçüm için en az bu dakikasındaki kaydedildiği saniye başına arama sorguları için en düşük değeridir. Aynı en yüksek değeri için geçerlidir. Toplama tamamı ortalama, dakikadır.
Bir dakika boyunca bu senaryoyu düşünün: yüksek bir saniye yüklemek diğer bir deyişle en fazla SearchQueriesPerSecond 58 saniyelik ortalama yük tarafından izlenen için ve son olarak yalnızca bir sorgu ile bir saniye olan en düşük gereksinimdir.

ThrottledSearchQueriesPercentage için minimum, maksimum, ortalama ve toplam, tümü aynı değere sahip:, arama sorguları bir dakika boyunca toplam sayısı azaltıldı arama sorguları yüzdesi.

## <a name="download-and-open-in-visual-studio-code"></a>Visual Studio Code indir ve aç

Günlük dosyasını görüntülemek için herhangi bir JSON düzenleyicisi kullanabilirsiniz. Hesabınız yoksa [Visual Studio Code](https://code.visualstudio.com/download)önerilir.

1. Azure portal, depolama hesabınızı açın. 

2. Sol gezinti bölmesinde **Bloblar**' a tıklayın. **Öngörüler-logs-operationlogs** ve **Insights-ölçümler-pt1m**görmeniz gerekir. Bu kapsayıcılar, günlük verileri blob depolamaya aktarıldığında Azure Search tarafından oluşturulur.

3. . JSON dosyasına ulaşana kadar klasör hiyerarşisini aşağı tıklayın.  Dosyayı indirmek için bağlam menüsünü kullanın.

Dosya indirildikten sonra içeriği görüntülemek için bir JSON düzenleyicisinde açın.

## <a name="use-system-apis"></a>Sistem API 'Lerini kullanma
Hem Azure Search REST API hem de .NET SDK, hizmet ölçümleri, dizin ve Dizin Oluşturucu bilgilerine ve belge sayılarına programlı erişim sağlar.

* [Hizmet Istatistiklerini al](/rest/api/searchservice/get-service-statistics)
* [Dizin istatistiklerini alma](/rest/api/searchservice/get-index-statistics)
* [Belge sayısı](/rest/api/searchservice/count-documents)
* [Dizin Oluşturucu durumunu Al](/rest/api/searchservice/get-indexer-status)

PowerShell veya Azure CLI kullanarak etkinleştirmek için belgelere bakın [burada](https://docs.microsoft.com/azure/azure-monitor/platform/diagnostic-logs-overview).

## <a name="next-steps"></a>Sonraki adımlar

Hizmet yönetimi ve ayarlama Kılavuzu için [performans ve iyileştirme](search-performance-optimization.md) hakkında daha fazla bilgi için [Microsoft Azure üzerindeki arama hizmetinizi yönetin](search-manage.md) .