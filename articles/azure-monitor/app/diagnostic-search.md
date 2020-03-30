---
title: Azure Uygulama Öngörülerinde Arama Kullanma | Microsoft Dokümanlar
description: Web uygulamanız tarafından gönderilen ham telemetriyi arayın ve filtreleyin.
ms.topic: conceptual
ms.date: 07/30/2019
ms.openlocfilehash: 8039a55784f63030f330d6c1e2061e99b8b63bbf
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79275990"
---
# <a name="using-search-in-application-insights"></a>Uygulama Öngörülerinde Arama'yı Kullanma

Arama, sayfa [görünümleri,](../../azure-monitor/app/app-insights-overview.md) özel durumlar veya web istekleri gibi tek tek telemetri öğelerini bulmak ve keşfetmek için kullandığınız Uygulama Öngörüleri'nin bir özelliğidir. Ayrıca, kodladığınız günlük izlerini ve olayları görüntüleyebilirsiniz.

(Verileriniz üzerinde daha karmaşık sorgular için [Analytics'i](../../azure-monitor/log-query/get-started-portal.md)kullanın.)

## <a name="where-do-you-see-search"></a>Arama'yı nerede görüyorsunuz?

### <a name="in-the-azure-portal"></a>Azure portalında

Uygulama Öngörüleri Genel Bakış sekmesinden (üst çubukta bulunan) veya sol daki araştırma altında tanılama aramasını açabilirsiniz.

![Arama sekmesi](./media/diagnostic-search/view-custom-events.png)

Telemetri öğelerinin listesini görmek için Olay türlerinin açılır menüsüne gidin, sunucu istekleri, sayfa görünümleri, kodladığınız özel olaylar ve benzeri şeyler. Sonuçlar listesinin en üstünde, zaman içinde olayların sayısını gösteren bir özet grafik bulunur.

Açılan menüden veya yeni etkinlikler almak için Yenile'yi tıklatın.

### <a name="in-visual-studio"></a>Visual Studio’da

Visual Studio'da bir Uygulama Öngörüleri Arama penceresi de vardır. Hata ayıklama yaptığınız uygulama tarafından oluşturulan telemetri olaylarını görüntülemek için en kullanışlıdır. Ancak, Azure portalında yayınlanan uygulamanızdan toplanan etkinlikleri de gösterebilir.

Visual Studio'da Arama penceresini açın:

![Visual Studio açık Uygulama Öngörüleri arama](./media/diagnostic-search/32.png)

Arama penceresi web portalına benzer özelliklere sahiptir:

![Visual Studio Application Insights arama penceresi](./media/diagnostic-search/34.png)

Bir isteği veya sayfa görünümünü açtığınızda İşlemi İzleme sekmesi kullanılabilir. 'İşlem' tek bir istek veya sayfa görünümüyle ilişkili olaylar dizisidir. Örneğin, bağımlılık çağrıları, özel durumlar, izleme günlükleri ve özel olaylar tek bir işlemin parçası olabilir. İzleme İşlemi sekmesi, istek veya sayfa görünümüyle ilgili olarak bu olayların zamanlamasını ve süresini grafik olarak gösterir.

## <a name="inspect-individual-items"></a>Tek tek öğeleri inceleyin

Anahtar alanlarını ve ilgili öğeleri görmek için herhangi bir telemetri öğesini seçin.

![Bireysel bağımlılık isteğinin ekran görüntüsü](./media/diagnostic-search/telemetry-item.png)

Bu, uçuça işlem ayrıntıları görünümünü başlatacaktır.

## <a name="filter-event-types"></a>Filtre olay türleri

Olay türlerinin açılır menüsünü açın ve görmek istediğiniz etkinlik türlerini seçin. (Daha sonra filtreleri geri yüklemek istiyorsanız, Sıfırla'yı tıklatın.)

Olay türleri şunlardır:

* **Trace** - TrackTrace, log4Net, NLog ve System.Diagnostic.Trace aramaları dahil olmak üzere[Tanılama günlüklerini](../../azure-monitor/app/asp-net-trace-logs.md) izleyin.
* **İstek** - Sayfalar, komut dosyaları, resimler, stil dosyaları ve veriler de dahil olmak üzere sunucu uygulamanız tarafından alınan HTTP istekleri. Bu olaylar, istek ve yanıt genel bakış grafiklerini oluşturmak için kullanılır.
* **Sayfa Görünümü** - [Telemetri web istemcisi tarafından gönderilen](../../azure-monitor/app/javascript.md), sayfa görünümü raporları oluşturmak için kullanılır.
* **Özel Etkinlik** - [Kullanımı izlemek](../../azure-monitor/app/api-custom-events-metrics.md)için TrackEvent() çağrıları eklediyseniz, burada arama yapabilirsiniz.
* **Özel Durum** - [Sunucuda](../../azure-monitor/app/asp-net-exceptions.md)yakalanmamış özel durumlar ve TrackException().
* **Bağımlılık** - [Sunucu uygulamanızdan](../../azure-monitor/app/asp-net-dependencies.md) REST API'leri veya veritabanları gibi diğer hizmetlere ve [istemci kodunuzdan](../../azure-monitor/app/javascript.md)AJAX çağrılarına çağrı verir.
* **Kullanılabilirlik** - [Kullanılabilirlik testlerinin](../../azure-monitor/app/monitor-web-app-availability.md)sonuçları.

## <a name="filter-on-property-values"></a>Özellik değerlerine filtre uygulayın

Olayları özelliklerinin değerlerine filtre uygulayabilirsiniz. Kullanılabilir özellikler seçtiğiniz olay türlerine bağlıdır. Filtre simgesine tıklayın ![Filtre simgesi](./media/diagnostic-search/filter-icon.png) başlamak için.

Belirli bir özelliğin hiçbir değer seçme, tüm değerleri seçmekle aynı etkiye sahiptir. O mülkte filtreleme kapanır.

Filtre değerlerinin sağındaki sayımların geçerli filtrelenmiş kümede kaç tane oluşum olduğunu gösterdiğine dikkat edin.

## <a name="find-events-with-the-same-property"></a>Aynı özelliğe sahip olayları bulma

Aynı özellik değerine sahip tüm öğeleri bulmak için, arama çubuğuna yazın veya filtre sekmesindeki özelliklere bakarken onay kutusunu tıklatın.

![Filtre sekmesindeki bir özelliğin onay kutusunu tıklatın](./media/diagnostic-search/filter-property.png)

## <a name="search-the-data"></a>Verileri arama

> [!NOTE]
> Daha karmaşık sorgular yazmak için, Arama bıçağının üstünden [**Günlükler'i (Analytics)**](../../azure-monitor/log-query/get-started-portal.md) açın.
>

Özellik değerlerinden herhangi birinde terimler arayabilirsiniz. Özellik değerleriyle özel [olaylar](../../azure-monitor/app/api-custom-events-metrics.md) yazdıysanız bu yararlıdır.

Daha kısa aralıktaki aramalar daha hızlı olduğundan, bir zaman aralığı ayarlamak isteyebilirsiniz.

![Tanıarama aramayı aç](./media/diagnostic-search/search-property.png)

Alt dizeleri değil, tam sözcükleri arayın. Özel karakterleri içine almak için tırnak işaretlerini kullanın.

| Dize | *Bulunamadı* | Bulundu |
| --- | --- | --- |
| HomeController.About |`home`<br/>`controller`<br/>`out` | `homecontroller`<br/>`about`<br/>`"homecontroller.about"`|
|Amerika Birleşik Devletleri|`Uni`<br/>`ted`|`united`<br/>`states`<br/>`united AND states`<br/>`"united states"`

Kullanabileceğiniz arama ifadeleri şunlardır:

| Örnek sorgu | Etki |
| --- | --- |
| `apple` |Alanları "elma" sözcüğünün bulunduğu zaman aralığındaki tüm olayları bulma |
| `apple AND banana` <br/>`apple banana` |Her iki sözcük içeren olayları bulun. "ve" değil, "VE" sermaye kullanın. <br/>Kısa form. |
| `apple OR banana` |Her iki sözcüğü içeren olayları bulun. "VEYA" kullanın, "veya" değil. |
| `apple NOT banana` |Bir sözcüğü içeren ancak diğerini içermeyen olayları bulun. |

## <a name="sampling"></a>Örnekleme

Uygulamanız büyük miktarda telemetri oluşturuyorsa (ve ASP.NET SDK sürüm 2.0.0-beta3 veya sonraki ASP.NET kullanıyorsanız), uyarlanabilir örnekleme modülü, olayların yalnızca temsili bir kısmını göndererek portala gönderilen hacmi otomatik olarak azaltır. Ancak, ilgili olaylar arasında gezinmek için aynı istekle ilgili olaylar grup olarak seçilir veya seçilir.

[Örnekleme hakkında bilgi edinin](../../azure-monitor/app/sampling.md).

## <a name="create-work-item"></a>İş öğesi oluşturma

Herhangi bir telemetri öğesinden ayrıntılarla GitHub veya Azure DevOps'lerde bir hata oluşturabilirsiniz.

Herhangi bir telemetri öğesini tıklatarak uçlardan uca hareket ayrıntısı görünümüne gidin ve **ardından çalışma öğesi oluştur'u**seçin.

![Yeni Çalışma Öğesi'ni tıklatın, alanları tıklatın ve sonra Tamam'ı tıklatın.](./media/diagnostic-search/work-item.png)

Bunu ilk yaptığınızda, Azure DevOps kuruluşunuza ve projenize bir bağlantı yapılandırmanız istenir.

(İş Öğeleri sekmesindeki bağlantıyı da yapılandırabilirsiniz.)

## <a name="send-more-telemetry-to-application-insights"></a>Uygulama Öngörüleri'ne daha fazla telemetri gönderin

Application Insights SDK tarafından gönderilen kutudan çıkmış telemetriye ek olarak şunları yapabilirsiniz:

* [.NET](../../azure-monitor/app/asp-net-trace-logs.md) veya [Java'da](../../azure-monitor/app/java-trace-logs.md)en sevdiğiniz günlük çerçevenizden günlük izlemelerini yakalayın. Bu, günlük izlemelerinizde arama yapabileceğiniz ve bunları sayfa görünümleri, özel durumlar ve diğer olaylarla ilişkilendirebileceğiniz anlamına gelir.
* Özel olaylar, sayfa görünümleri ve özel durumlar göndermek için [kod yazın.](../../azure-monitor/app/api-custom-events-metrics.md)

[Günlükleri ve özel telemetriyi Uygulama Öngörüleri'ne nasıl göndereceğini öğrenin.](../../azure-monitor/app/asp-net-trace-logs.md)

## <a name="q--a"></a><a name="questions"></a>S & A

### <a name="how-much-data-is-retained"></a><a name="limits"></a>Ne kadar veri tutulur?

Sınırlar [özetine](../../azure-monitor/app/pricing.md#limits-summary)bakın.

### <a name="how-can-i-see-post-data-in-my-server-requests"></a>Sunucu isteklerimde POST verilerini nasıl görebiliyorum?

POST verilerini otomatik olarak kaydetmeyiz, ancak [TrackTrace'i veya günlük aramalarını](../../azure-monitor/app/asp-net-trace-logs.md)kullanabilirsiniz. POST verilerini ileti parametreye koyun. İletiye, özelliklere filtre uygulayabileceğiniz şekilde filtre uygulayamazsınız, ancak boyut sınırı daha uzundur.

## <a name="next-steps"></a><a name="add"></a>Sonraki adımlar

* [Analytics'te karmaşık sorgular yazma](../../azure-monitor/log-query/get-started-portal.md)
* [Günlükleri ve özel telemetriyi Uygulama Öngörüleri'ne gönderme](../../azure-monitor/app/asp-net-trace-logs.md)
* [Kullanılabilirlik ve yanıt verme testlerini ayarlama](../../azure-monitor/app/monitor-web-app-availability.md)
* [Sorun giderme](../../azure-monitor/app/troubleshoot-faq.md)
