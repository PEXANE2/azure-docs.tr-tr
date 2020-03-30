---
title: Uygulamaları izleme
description: Azure portalını kullanarak Azure Uygulama Hizmeti'ndeki uygulamaları nasıl izleyeceğinizi öğrenin. Bildirilen kotaları ve ölçümleri anlayın.
author: btardif
ms.assetid: d273da4e-07de-48e0-b99d-4020d84a425e
ms.topic: article
ms.date: 01/11/2019
ms.author: byvinyal
ms.custom: seodec18
ms.openlocfilehash: d84340730391abd7dba4d13202503d37941c09b5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79500425"
---
# <a name="monitor-apps-in-azure-app-service"></a>Azure Uygulama Hizmeti'ndeki uygulamaları izleme
[Azure Uygulama Hizmeti,](https://go.microsoft.com/fwlink/?LinkId=529714) [Azure portalında](https://portal.azure.com)web uygulamaları, mobil cihazlar ve API uygulamaları için yerleşik izleme işlevi sağlar.

Azure portalında, bir uygulama ve Uygulama Hizmeti planı için *kotaları* ve *ölçümleri* inceleyebilir ve *uyarılar* ve kurallara dayalı *ölçümleri otomatik ölçekleme* ayarlayabilirsiniz.

## <a name="understand-quotas"></a>Kotaları anlama

Uygulama Hizmeti'nde barındırılan uygulamalar, kullanabilecekleri kaynaklarla ilgili belirli sınırlara tabidir. Sınırlar, uygulamayla ilişkili Uygulama Hizmeti planı yla tanımlanır.

[!INCLUDE [app-service-dev-test-note](../../includes/app-service-dev-test-note.md)]

Uygulama *Ücretsiz* veya *Paylaşılan* bir planda barındırılıyorsa, uygulamanın kullanabileceği kaynakların sınırları kotalarla tanımlanır.

Uygulama *Temel,* *Standart*veya *Premium* planda barındırılıyorsa, kullanabilecekleri kaynakların sınırları, Uygulama Hizmeti planının *boyutuna* (Küçük, Orta, Büyük) ve *örnek sayısına* (1, 2, 3 vb.) göre ayarlanır.

Ücretsiz veya Paylaşılan uygulamalar için kotalar şunlardır:

| Kota | Açıklama |
| --- | --- |
| **Cpu (Kısa)** | Bu uygulama için izin verilen CPU miktarı 5 dakikalık bir aralıkta. Bu kota her beş dakikada bir sıfırlanır. |
| **CPU (Gün)** | Bir gün içinde bu uygulama için izin verilen toplam CPU miktarı. Bu kota, gece yarısı UTC'de her 24 saatte bir sıfırlanır. |
| **Bellek** | Bu uygulama için izin verilen toplam bellek miktarı. |
| **Bant genişliği** | Bir gün içinde bu uygulama için izin giden bant genişliği toplam miktarı. Bu kota, gece yarısı UTC'de her 24 saatte bir sıfırlanır. |
| **Dosya Sistemi** | İzin verilen toplam depolama miktarı. |

*Temel*, *Standart*ve *Premium'da* barındırılan uygulamalar için geçerli olan tek kota Filesystem'dir.

Çeşitli Uygulama Hizmeti SK'leri tarafından kullanılabilen belirli kotalar, sınırlar ve özellikler hakkında daha fazla bilgi için [Azure Abonelik hizmet sınırlarına](../azure-resource-manager/management/azure-subscription-service-limits.md#app-service-limits)bakın.

### <a name="quota-enforcement"></a>Kota uygulaması

Bir uygulama CPU *(kısa) ,* *CPU (Gün)* veya *bant genişliği* kotasını aşarsa, uygulama kota sıfırlanına kadar durdurulur. Bu süre zarfında, gelen tüm istekler bir HTTP 403 hatasıyla sonuçlanır.

![403 hata iletisi][http403]

Uygulama Bellek kotası aşılırsa, uygulama yeniden başlatılır.

Filesystem kotası aşılırsa, herhangi bir yazma işlemi başarısız olur. Yazma işlemi hataları günlükleri için herhangi bir yazma içerir.

Uygulama Hizmeti planınızı yükselterek uygulamanızdaki kotaları artırabilir veya kaldırabilirsiniz.

## <a name="understand-metrics"></a>Ölçümleri anlama

> [!NOTE]
> **Dosya Sistemi Kullanımı,** genel olarak kullanıma çıkarılan yeni bir metriktir, özel önizleme için beyaz listeye alınmadıkça veri beklenmiyor.
> 

> [!IMPORTANT]
> **Ortalama Yanıt Süresi,** metrik toplamalarla karışıklığı önlemek için amortismana kesilir. **Yanıt Süresini** yedek olarak kullanın.

Ölçümler, uygulama veya Uygulama Hizmeti planının davranışı hakkında bilgi sağlar.

Bir uygulama için kullanılabilir ölçümler şunlardır:

| Ölçüm | Açıklama |
| --- | --- |
| **Yanıt Süresi** | Uygulamanın isteklere saniyeler içinde sunması için gereken süre. |
| **Ortalama Yanıt Süresi (amortismana uğradı)** | Uygulamanın isteklere saniyeler içinde sunması için gereken ortalama süre. |
| **Ortalama bellek çalışma kümesi** | Megabayt (MiB) olarak uygulama tarafından kullanılan ortalama bellek miktarı. |
| **Bağlantılar** | Kum havuzunda bulunan bağlı soketlerin sayısı (w3wp.exe ve alt işlemleri).  Bağlı bir soket bind()/connect() API'leri çağırarak oluşturulur ve belirtilen soket CloseHandle()/closesocket() ile kapatılana kadar kalır. |
| **CPU Süresi** | Uygulama tarafından saniyeler içinde tüketilen CPU miktarı. Bu metrik hakkında daha fazla bilgi için [CPU süresi vs CPU yüzdesi](#cpu-time-vs-cpu-percentage)bakın. |
| **Geçerli Meclisler** | Bu uygulamadaki tüm AppDomain'lere yüklenen mevcut Derleme sayısı. |
| **Veri In** | MiB'de uygulama tarafından tüketilen gelen bant genişliği miktarı. |
| **Veri Çıkış** | MiB'de uygulama tarafından tüketilen giden bant genişliği miktarı. |
| **Dosya Sistemi Kullanımı** | Uygulama tarafından tüketilen dosya sistemi kotasının yüzdesi. |
| **Gen 0 Çöp Koleksiyonları** | Uygulama işleminin başlangıcından bu yana nesil 0 nesnelerinin kaç kez çöp topladığı. Yüksek nesil GC'ler tüm alt nesil GC'leri içerir.|
| **Gen 1 Çöp Koleksiyonları** | Uygulama işleminin başlangıcından bu yana nesil 1 nesnelerin kaç kez çöp topladığı. Yüksek nesil GC'ler tüm alt nesil GC'leri içerir.|
| **Gen 2 Çöp Koleksiyonları** | Uygulama sürecinin başlangıcından bu yana nesil 2 nesnelerin kaç kez çöp topladığı.|
| **Tanıtıcı Sayısı** | Uygulama işlemi tarafından şu anda açık olan toplam tutamaç sayısı.|
| **Http 2xx** | HTTP durum kodu ≥ 200 ile sonuçlanan ancak 300 < isteklerin sayısı. |
| **Http 3xx** | HTTP durum kodu ≥ 300 ile sonuçlanan ancak 400 < isteklerin sayısı. |
| **Http 401** | HTTP 401 durum koduyla sonuçlanan isteklerin sayısı. |
| **Http 403** | HTTP 403 durum koduyla sonuçlanan isteklerin sayısı. |
| **Http 404** | HTTP 404 durum koduyla sonuçlanan isteklerin sayısı. |
| **Http 406** | HTTP 406 durum koduyla sonuçlanan isteklerin sayısı. |
| **Http 4xx** | HTTP durum kodu ≥ 400 ile sonuçlanan ancak 500'< sonuçlanan isteklerin sayısı. |
| **Http Sunucu Hataları** | HTTP durum kodu ≥ 500 ile sonuçlanan ancak 600 < isteklerin sayısı. |
| **IO Saniyede Diğer Baytlar** | Uygulama işleminin denetim işlemleri gibi verileri içermeyen G/Ç işlemlerine bayt verme oranı.|
| **IO Saniyede Diğer İşlemler** | Uygulama işleminin okuma veya yazma işlemleri olmayan G/Ç işlemlerini verme oranı.|
| **IO Saniyede Bayt Oku** | Uygulama işleminin G/Ç işlemlerinden bayt okuma oranı.|
| **IO Saniyede Okuma İşlemleri** | Uygulama işleminin okuma G/Ç işlemlerini verme oranı.|
| **IO Saniyede Bayt Yaz** | Uygulama işleminin G/Ç işlemlerine bayt yazma oranı.|
| **IO Saniyede Yazma İşlemleri** | Uygulama işleminin yazma G/Ç işlemleri verme oranı.|
| **Bellek çalışma kümesi** | Uygulama tarafından mib'de kullanılan geçerli bellek miktarı. |
| **Özel Baytlar** | Özel Baytlar, uygulama işleminin diğer işlemlerle paylaşılmayan ayırdığı bellekteki geçerli boyutdur.|
| **Istek** | Ortaya çıkan HTTP durum koduna bakılmaksızın toplam istek sayısı. |
| **Uygulama Kuyruğundaki İstekler** | Uygulama istek kuyruğundaki istek sayısı.|
| **Ilmekli** | Uygulama işleminde şu anda etkin olan iş parçacığı sayısı.|
| **Toplam Uygulama Etki Alanı** | Bu uygulamaya yüklenen geçerli AppDomain sayısı.|
| **Toplam Uygulama Etki Alanı Boşaltıldı** | Uygulamanın başlangıcından bu yana boşaltılan toplam AppDomain sayısı.|


Bir Uygulama Hizmeti planı için kullanılabilir ölçümler şunlardır:

> [!NOTE]
> Uygulama Hizmeti planı ölçümleri yalnızca *Temel,* *Standart*ve *Premium* katmanlarda bulunan planlar için kullanılabilir.
> 

| Ölçüm | Açıklama |
| --- | --- |
| **CPU Yüzdesi** | Planın tüm örneklerinde kullanılan ortalama CPU. |
| **Bellek Yüzdesi** | Planın tüm örneklerinde kullanılan ortalama bellek. |
| **Veri In** | Planın tüm örneklerinde kullanılan ortalama gelen bant genişliği. |
| **Veri Çıkış** | Planın tüm örneklerinde kullanılan ortalama giden bant genişliği. |
| **Disk Sıra Uzunluğu** | Depolama da sıraya alınan ortalama okuma ve yazma istekleri sayısı. Yüksek disk sırası uzunluğu, aşırı disk G/Ç nedeniyle yavaşlayan bir uygulamanın göstergesidir. |
| **Http Sıra Uzunluğu** | Yerine getirilmeden önce kuyruğa girmek zorunda olan ortalama HTTP isteği sayısı. Yüksek veya artan HTTP Kuyruk uzunluğu, ağır yük altındaki bir planın belirtisidir. |

### <a name="cpu-time-vs-cpu-percentage"></a>CPU süresi vs CPU yüzdesi
<!-- To do: Fix Anchor (#CPU-time-vs.-CPU-percentage) -->

CPU kullanımını yansıtan iki ölçüm vardır:

**CPU Süresi**: Kotalarından biri uygulama tarafından kullanılan CPU dakikalarında tanımlandığı için, Ücretsiz veya Paylaşılan planlarda barındırılan uygulamalar için yararlıdır.

**CPU yüzdesi**: Temel, Standart ve Premium planlarında barındırılan uygulamalar için yararlıdır, çünkü bunlar ölçeklendirilebilir. CPU yüzdesi tüm örneklerde genel kullanımın iyi bir göstergesidir.

## <a name="metrics-granularity-and-retention-policy"></a>Ölçümler tanecikve bekletme ilkesi
Bir uygulama ve uygulama hizmeti planı için ölçümler, aşağıdaki ayrıntılı lıklar ve bekletme ilkeleriyle birlikte hizmet tarafından günlüğe kaydedilir ve toplanır:

* **Dakika** tanecik ölçümleri 30 saat saklanır.
* **Saat** tanecik ölçümleri 30 gün boyunca saklanır.
* **Gün** tanecik ölçümleri 30 gün boyunca saklanır.

## <a name="monitoring-quotas-and-metrics-in-the-azure-portal"></a>Azure portalındaki kotaları ve ölçümleri izleme
Bir uygulamayı etkileyen çeşitli kotaların ve ölçümlerin durumunu gözden geçirmek için [Azure portalına](https://portal.azure.com)gidin.

![Azure portalında kotalar grafiği][quotas]

Kotaları bulmak için **Ayarlar** > **Kotaları'nı**seçin. Grafikte şunları inceleyebilirsiniz: 
1. Kota adı.
1. Sıfırlama aralığı.
1. Geçerli limiti.
1. Şu anki değeri.

![Azure portalında][metrics] metrik grafik Ölçümlere doğrudan kaynak **Genel Bakış** sayfasından erişebilirsiniz. Burada bazı uygulama ölçümlerini temsil eden grafikler görürsünüz.

Bu grafiklerden herhangi birini tıklattığınızda, özel grafikler oluşturabileceğiniz, farklı ölçümleri sorgulayabileceğiniz ve çok daha fazlasını yapabileceğiniz metrikgörünümüne götürün. 

Ölçümler hakkında daha fazla bilgi edinmek için hizmet [ölçümlerini izleyin'e](../monitoring-and-diagnostics/insights-how-to-customize-monitoring.md)bakın.

## <a name="alerts-and-autoscale"></a>Uyarılar ve otomatik ölçeklendirme
Bir uygulama nın veya Uygulama Hizmeti planının ölçümleri uyarılara bağlanabilir. Daha fazla bilgi için bkz. [Uyarı bildirimleri alma](../monitoring-and-diagnostics/insights-alerts-portal.md).

Temel veya daha yüksek Uygulama Hizmeti planlarında barındırılan Uygulama Hizmeti uygulamaları otomatik ölçeği destekler. Otomatik ölçeklendirme ile, Uygulama Hizmeti planı ölçümlerini izleyen kuralları yapılandırabilirsiniz. Kurallar, gerektiğinde ek kaynaklar sağlayabilecek örnek sayısını artırabilir veya azaltabilir. Kurallar, uygulama aşırı sağlandığında da paradan tasarruf etmenizi sağlayabilir.

Otomatik ölçeklendirme hakkında daha fazla bilgi için Azure [Monitörü otomatik ölçeklendirmeiçin](../azure-monitor/platform/autoscale-best-practices.md) [nasıl ölçeklendirilip](../monitoring-and-diagnostics/insights-how-to-scale.md) en iyi uygulamalara bakın.

[fzilla]:https://go.microsoft.com/fwlink/?LinkId=247914
[vmsizes]:https://go.microsoft.com/fwlink/?LinkID=309169

<!-- Images. -->
[http403]: ./media/web-sites-monitor/http403.png
[quotas]: ./media/web-sites-monitor/quotas.png
[metrics]: ./media/web-sites-monitor/metrics.png