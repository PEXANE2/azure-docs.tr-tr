---
title: Azure Application Insights ile ASP.NET Web uygulaması analizi ayarlama | Microsoft Docs
description: Şirket içinde veya Azure 'da barındırılan ASP.NET Web siteniz için performans, kullanılabilirlik ve Kullanıcı davranışı analizi araçlarını yapılandırın.
ms.topic: conceptual
ms.date: 05/08/2019
ms.openlocfilehash: acfba63cba520631831888a1480929be3b1897f0
ms.sourcegitcommit: 5f7b75e32222fe20ac68a053d141a0adbd16b347
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/31/2020
ms.locfileid: "87475541"
---
# <a name="set-up-application-insights-for-your-aspnet-website"></a>ASP.NET web siteniz için Application Insights'ı ayarlama

Bu yordam ASP.NET web uygulamanızı [Azure Application Insights](./app-insights-overview.md) hizmetine telemetri gönderecek şekilde yapılandırır. Kendi IIS sunucunuzda şirket içi olarak veya Bulut’ta barındırılan ASP.NET uygulamaları için çalışır. Uygulamanızın performansını ve nasıl kullanıldığını anlamanıza yardımcı olan grafikler ve güçlü bir sorgu dilinin yanı sıra hata ya da performans sorunları hakkında otomatik uyarılar alırsınız. Çoğu geliştirici, özellikleri bu haliyle mükemmel bulsa da, gerekirse telemetriyi genişletip özelleştirebilirsiniz.

Visual Studio'da kurulum yalnızca birkaç tıklama ile yapılır. Telemetri hacmini sınırlayarak ücret doğmamasını sağlayabilirsiniz. Bu işlevsellik, çok sayıda kullanıcısı olmayan bir siteyi denemenizi ve hata ayıklamanıza ya da izlemenize olanak sağlar. Devam edip üretim merkezinizi izlemeye karar verdiğinizde, sınırı daha sonra kolayca artırabilirsiniz.

## <a name="prerequisites"></a>Önkoşullar
Application Insights’ı ASP.NET web sitenize eklemek için şunu yapmanız gerekir:

- [Windows Için Visual Studio 2019](https://www.visualstudio.com/downloads/) 'yi aşağıdaki iş yükleriyle birlikte yükler:
    - ASP.NET ve Web geliştirme (isteğe bağlı bileşenlerin işaretini kaldırmayın)
    - Azure geliştirme

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz](https://azure.microsoft.com/free/) bir hesap oluşturun.

## <a name="step-1-add-the-application-insights-sdk"></a><a name="ide"></a> 1. Adım: Application Insights SDK’yı ekleme

> [!IMPORTANT]
> Bu örnekteki ekran görüntüleri, Visual Studio 2017 sürüm 15.9.9 ve üstünü temel alır. Application Insights ekleme deneyimi, Visual Studio 'nun yanı sıra ASP.NET şablon türüne göre farklılık gösterir. Eski sürümlerde "Configure Application Insights" gibi alternatif metinler olabilir.

Çözüm Gezgini Web uygulaması adına sağ tıklayın ve **Ekle**' yi seçin  >  **Application Insights telemetri**

![Application Insights’ı Yapılandır seçeneğinin vurgulandığı Çözüm Gezgini’nin ekran görüntüsü](./media/asp-net/add-telemetry-new.png)

(Application Insights SDK sürümünüze bağlı olarak, son SDK sürümüne güncelleştirme yapmanız istenebilir. İstenirse **SDK’yı güncelleştir**’i seçin.)

![Ekran görüntüsü: Yeni bir Microsoft Application Insights SDK sürümü mevcut. SDK’yı güncelleştir seçeneğinin vurgulandığı görüntü](./media/asp-net/0002-update-sdk.png)

Application Insights Yapılandırması ekranı:

**Kullanmaya**başlayın ' ı seçin.

![Uygulamanızı Application Insights'a kaydedin sayfasının ekran görüntüsü](./media/asp-net/00004-start-free.png)

Verilerin depolandığı kaynak grubunu veya konumu ayarlamak isterseniz **Ayarları yapılandır**'a tıklayın. Kaynak grupları, verilere erişimi denetlemek için kullanılır. Örneğin aynı sistemin parçalarını oluşturan birden uygulamanız varsa bunların Application Insights verilerini aynı kaynak grubuna ekleyebilirsiniz.

 **Kaydet**’i seçin.

![Uygulamanızı Application Insights'a kaydedin sayfasının ekran görüntüsü](./media/asp-net/00005-register-ed.png)

 **Proje**seçin  >  **NuGet paketleri**  >  **paket kaynağı: NuGet.org** > Application Insights SDK 'nın en son kararlı sürümüne sahip olduğunu onaylayın.

 Telemetri hem hata ayıklama sırasında hem de uygulamanızı yayımladıktan sonra [Azure portalına](https://portal.azure.com) gönderilir.
> [!NOTE]
> Hata ayıklama sırasında portala telemetri göndermek istemiyorsanız, uygulamanıza Application Insights SDK’sını ekleyin, ancak portalda bir kaynak yapılandırmayın. Hata ayıklama sırasında telemetri verilerini Visual Studio'da görebilirsiniz. Daha sonra bu yapılandırma sayfasına dönebilir veya uygulamanızı dağıtana kadar bekleyip [telemetriyi çalışma zamanında açabilirsiniz](./status-monitor-v2-overview.md).

## <a name="step-2-run-your-app"></a><a name="run"></a>2. Adım: uygulamanızı çalıştırma
F5 tuşuna basarak uygulamanızı çalıştırın. Farklı sayfalar açarak telemetri verileri oluşturun.

Visual Studio'da günlüğe kaydedilmiş etkinliklerin sayısını görürsünüz.

![Visual Studio’nun ekran görüntüsü. Hata ayıklama sırasında Application Insights düğmesi görünür.](./media/asp-net/00006-Events.png)

## <a name="step-3-see-your-telemetry"></a>Adım 3: Telemetrinize bakma
Visual Studio’da veya Application Insights web portalında telemetrinizi görebilirsiniz. Uygulamanızın hatalarını ayıklamanıza yardımcı olması için Visual Studio'da telemetri arayın. Sisteminiz canlıyken web portalında performans ve kullanımı izleyin. 

### <a name="see-your-telemetry-in-visual-studio"></a>Visual Studio'da telemetrinize bakma

Visual Studio’da Application Insights verilerini görüntülemek için şunları yapın.  **Çözüm Gezgini**  >  **bağlı hizmetler** ' i seçin > **Application Insights**' ye sağ tıklayın ve ardından **canlı telemetri ara**' ya tıklayın.

Visual Studio Application Insights Arama penceresinde, uygulamanızın sunucu tarafında oluşturulan telemetri için uygulamanızdan alınan veriler görünümü açılır. Filtrelerle denemeler yapın ve daha fazla ayrıntı için herhangi bir etkinliğe tıklayın.

![Application Insights penceresindeki Hata ayıklama oturumundan alınan veriler görünümünün ekran görüntüsü.](./media/asp-net/55.png)

> [!Tip]
> Herhangi bir veri gösterilmiyorsa zaman aralığının doğru olduğundan emin olup Ara simgesine tıklayın.

[Visual Studio’daki Application Insights araçları hakkında daha fazla bilgi edinin](./visual-studio.md).

<a name="monitor"></a>
### <a name="see-telemetry-in-web-portal"></a>Web portalında telemetriye bakma

Yalnızca SDK’yı yüklemeyi seçmediyseniz telemetriyi Application Insights web portalında da görüntüleyebilirsiniz. Portalda, Visual Studio’ya kıyasla daha çok grafik, analiz aracı ve bileşenler arası görünüm bulunur. Portal ayrıca uyarılar sağlar.

Application Insights kaynağınızı açın. [Azure portalında](https://portal.azure.com/) oturum açarak aradığınız öğeyi orada bulabilir veya **Çözüm Gezgini** > **Bağlı Hizmetler**’i seçip > **Application Insights** > **Application Insights Portal’ı aç**’a sağ tıklayarak sayfaya yönlendirilebilirsiniz.

Portal, uygulamanızdan alınan telemetri görünümünde açılır.

![Application Insights’a genel bakış sayfasının ekran görüntüsü](./media/asp-net/007.png)

Daha fazla ayrıntı görmek için portalda istediğiniz kutucuğa veya grafiğe tıklayın.

## <a name="step-4-publish-your-app"></a>4. Adım: Uygulamanızı yayımlama
Uygulamanızı IIS sunucunuza veya Azure’a yayımlayın. Her şeyin sorunsuz çalıştığından emin olmak için [Canlı Ölçümler Akışı](./live-stream.md)’nı izleyin.

Telemetriniz Application Insights portalında oluşturulur ve burada ölçümleri izleyebilir, telemetrinizde arama yapabilirsiniz. Ayrıca, güçlü [kusto sorgu dilini](/azure/kusto/query/) kullanarak kullanımı ve performansı analiz edebilir ya da belirli olayları bulabilirsiniz.

Telemetrinizi tanılama araması ve [eğilimler](./visual-studio-trends.md) gibi araçlarla [Visual Studio](./visual-studio.md)’da analiz etmeye de devam edebilirsiniz.

> [!NOTE]
> Uygulamanız [azaltma sınırlarına](./pricing.md#limits-summary) yaklaşmak için yeterli telemetri gönderiyorsa, otomatik [örnekleme](./sampling.md) etkinleştirilir. Örnekleme, tanılama amaçlı bağlantı verilerini korurken uygulamanızdan gönderilen telemetri miktarını azaltır.
>
>

## <a name="youre-all-set"></a><a name="land"></a>Hazırsınız

Tebrikler! Application Insights paketini uygulamanıza yüklediniz ve Azure üzerinde Application Insights hizmetine telemetri gönderecek şekilde yapılandırdınız.

Uygulamanızın telemetrisini alan Azure kaynağı bir *izleme anahtarı* ile tanımlanır. Bu anahtarı ApplicationInsights.config dosyasında bulabilirsiniz.


## <a name="upgrade-to-future-sdk-versions"></a>Gelecek SDK sürümlerine yükseltme

* [Sürüm Notları](./release-notes.md)

SDK 'nın yeni bir sürümüne yükseltmek için, **NuGet Paket Yöneticisi**' ni açın ve yüklü paketleri filtreleyin. **Microsoft. ApplicationInsights. Web**' i seçin ve **Yükselt**' i seçin.

ApplicationInsights.config’de herhangi bir özelleştirme gerçekleştirdiyseniz yükseltmeden önce dosyanın bir kopyasını kaydedin. Daha sonra, yaptığınız değişiklikleri yeni sürümle birleştirin.

## <a name="next-steps"></a>Sonraki adımlar

İlginizi çekiyorsa inceleyebileceğiniz alternatif konu başlıkları da mevcuttur:

* [Çalışma zamanında bir web uygulamasını izleme](./monitor-performance-live-website-now.md)
* [Azure Cloud Services](./cloudservices.md)

### <a name="more-telemetry"></a>Daha fazla telemetri

* **[Tarayıcı ve sayfa yükleme verileri](./javascript.md)** - Web sayfalarınıza bir kod parçacığı ekleyin.
* **[Daha ayrıntılı bağımlılık ve özel durum izlemesi alın](./monitor-performance-live-website-now.md)** - Sunucunuza Durum İzleyicisi yükleyin.
* Kullanıcı eylemlerini saymak, zamanlamak veya ölçmek için **[özel olaylar kodlayın](./api-custom-events-metrics.md)**.
* **[Günlük verilerini alma](./asp-net-trace-logs.md)** - Günlük verilerini telemetrinizle ilişkilendirin.

### <a name="analysis"></a>Analiz

* **[Visual Studio’da Application Insights ile çalışma](./visual-studio.md)**<br/>Telemetri, tanılama araması ve kodun detayına gitme ile hata ayıklama hakkında bilgi içerir.
* **[Analytics](../log-query/get-started-portal.md)** - Güçlü sorgu dili.

### <a name="alerts"></a>Uyarılar

* [Kullanılabilirlik testleri](./monitor-web-app-availability.md): Sitenizin web’de görünür olduğundan emin olmaya yönelik testler oluşturun.
* [Akıllı tanılama](./proactive-diagnostics.md): Bu testler otomatik olarak çalıştığından, bunları ayarlamak için herhangi bir şey yapmanız gerekmez. Uygulamanızda olağan dışı oranda başarısız istek olup olmadığını bildirirler.
* [Ölçüm uyarıları](../platform/alerts-log.md): bir ölçüm bir eşiği kesiştyolursa sizi uyarmak için Uyarılar ayarlayın. Bunları, uygulamanıza kodladığınız özel ölçümlerde ayarlayabilirsiniz.

### <a name="automation"></a>Otomasyon

* [Application Insights kaynağı oluşturmayı otomatikleştirme](./powershell.md)

