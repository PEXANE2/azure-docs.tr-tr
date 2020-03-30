---
title: Azure Uygulama Öngörüleri'nde veri saklama ve depolama | Microsoft Dokümanlar
description: Saklama ve gizlilik politikası bildirimi
ms.topic: conceptual
ms.date: 09/29/2019
ms.openlocfilehash: 30878eecf795c85713b9f09b8325b326416022b8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79276003"
---
# <a name="data-collection-retention-and-storage-in-application-insights"></a>Uygulama Öngörüleri'nde veri toplama, saklama ve depolama

Uygulamanızda [Azure Application Insights][start] SDK'yı yüklediğinizde, uygulamanızla ilgili telemetriyi Bulut'a gönderir. Doğal olarak, sorumlu geliştiriciler tam olarak hangi verilerin gönderildiğini, verilere ne olduğunu ve verilerin denetimini nasıl tutabileceklerini bilmek isterler. Özellikle, hassas veriler gönderilebilir, nerede saklanır ve ne kadar güvenlidir? 

İlk olarak, kısa cevap:

* "Kutunun dışında" çalışan standart telemetri modüllerinin hassas verileri hizmete gönderme olasılığı düşüktür. Telemetri, yük, performans ve kullanım ölçümleri, özel durum raporları ve diğer tanılama verileriyle ilgilidir. Tanılama raporlarında görünen ana kullanıcı verileri URL'lerdir; ancak uygulamanız hiçbir durumda hassas verileri bir URL'ye düz metne koymamalıdır.
* Tanılama ve izleme kullanımı nda size yardımcı olmak için ek özel telemetri gönderen kod yazabilirsiniz. (Bu genişletilebilirlik Uygulama Öngörüleri büyük bir özelliğidir.) Bu kodu kişisel ve diğer hassas verileri içerecek şekilde yazmak yanlışlıkla mümkündür. Uygulamanız bu tür verilerle çalışıyorsa, yazdığınız tüm kodlara ayrıntılı bir inceleme işlemi uygulamanız gerekir.
* Uygulamanızı geliştirirken ve test ederken, SDK tarafından gönderilenleri kolayca inceleyebilir. Veriler, IDE ve tarayıcının hata ayıklama çıkış pencerelerinde görünür. 
* Veriler ABD veya Avrupa'daki [Microsoft Azure](https://azure.com) sunucularında tutulur. (Ancak uygulamanız her yerde çalıştırılabilir.) Azure [güçlü güvenlik süreçlerine sahiptir ve çok çeşitli uyumluluk standartlarını karşılar.](https://azure.microsoft.com/support/trust-center/) Verilerinize yalnızca siz ve atanmış ekibiniz erişebilir. Microsoft personeli, yalnızca sizin bilginiz dahilinde belirli sınırlı koşullar altında bu bilgilere sınırlı erişime sahip olabilir. Nakil ve istirahatte şifreli.
*   Toplanan verileri gözden geçirin, çünkü bu bazı durumlarda izin verilen ancak diğerlerine izin verilmeyen verileri içerebilir.  Bunun iyi bir örneği Aygıt Adı'dır. Sunucudan gelen aygıt adının gizlilik etkisi yoktur ve yararlıdır, ancak telefon veya dizüstü bilgisayardaki bir aygıt adı gizlilik etkisi ne olabilir ve daha az kullanışlı olabilir. Öncelikle hedef sunuculara geliştirilen bir SDK, varsayılan olarak aygıt adını toplar ve bunun hem normal olaylarda hem de özel durumlarda üzerine yazılması gerekebilir.

Bu makalenin geri kalanı daha tam olarak bu cevaplar üzerinde ayrıntılı. Kendi kendine yeten bir yer olması için tasarlanmıştır, böylece yakın ekibinizin bir parçası olmayan iş arkadaşlarınıza gösterebilirsiniz.

## <a name="what-is-application-insights"></a>Application Insights nedir?
[Azure Application Insights,][start] Microsoft tarafından sağlanan ve canlı uygulamanızın performansını ve kullanılabilirliğini geliştirmenize yardımcı olan bir hizmettir. Uygulamanızı hem sınama sırasında hem de yayımladıktan veya dağıttıktan sonra, her zaman çalışır durumda izler. Application Insights, örneğin, çoğu kullanıcıyı günün hangi saatlerinde aldığınızı, uygulamanın ne kadar duyarlı olduğunu ve bağlı olduğu herhangi bir harici hizmet tarafından ne kadar iyi sunulduğunu gösteren grafikler ve tablolar oluşturur. Kilitlenmeler, hatalar veya performans sorunları varsa, nedenini tanılamak için telemetri verilerini ayrıntılı olarak arayabilirsiniz. Ayrıca, uygulamanızın kullanılabilirliği ve performansında herhangi bir değişiklik olması durumunda hizmet size e-posta gönderir.

Bu işlevselliği elde etmek için, uygulamanızda kodunun bir parçası haline gelen bir Application Insights SDK yüklersiniz. Uygulamanız çalışırken, SDK işlemini izler ve Uygulama Öngörüleri hizmetine telemetri gönderir. Bu, [Microsoft Azure](https://azure.com)tarafından barındırılan bir bulut hizmetidir. (Ancak Application Insights, azure'da barındırılan uygulamalar için değil, tüm uygulamalar için çalışır.)

Application Insights servis depolarını saklar ve telemetriyi analiz eder. Depolanan telemetri de analizi ni görmek veya arama yapmak için Azure hesabınızda oturum açın ve uygulamanız için Application Insights kaynağını açın. Verilere erişimi ekibinizin diğer üyeleriyle veya belirtilen Azure aboneleriyle de paylaşabilirsiniz.

Örneğin bir veritabanına veya dış araçlara, Application Insights hizmetinden veri aktarılabilir. Her araca hizmetten elde ettiğiniz özel bir anahtar sağlarsınız. Gerekirse anahtar iptal edilebilir. 

Uygulama Öngörüleri SDK'ları çeşitli uygulama türleri için kullanılabilir: kendi Java EE veya ASP.NET sunucularınızda veya Azure'da barındırılan web hizmetleri; web istemcileri - yani, bir web sayfasında çalışan kod; masaüstü uygulamaları ve hizmetleri; Windows Phone, iOS ve Android gibi aygıt uygulamaları. Hepsi aynı servise telemetri gönderir.

## <a name="what-data-does-it-collect"></a>Hangi verileri topluyor?
Üç veri kaynağı vardır:

* Uygulamanızla [geliştirme aşamasında](../../azure-monitor/app/asp-net.md) veya [çalışma zamanında](../../azure-monitor/app/monitor-performance-live-website-now.md)entegre ettiğiniz SDK. Farklı uygulama türleri için farklı SDK'lar vardır. Ayrıca web sayfaları için bir [SDK](../../azure-monitor/app/javascript.md)var , hangi sayfa ile birlikte son kullanıcının tarayıcısına yükler.
  
  * Her [SDK'](../../azure-monitor/app/configuration-with-applicationinsights-config.md)nın farklı türde telemetrileri toplamak için farklı teknikler kullanan bir dizi modülü vardır.
  * SDK'yı geliştirmeye yüklerseniz, standart modüllere ek olarak kendi telemetrinizi göndermek için API'sini kullanabilirsiniz. Bu özel telemetri göndermek istediğiniz verileri içerebilir.
* Bazı web sunucularında, uygulamayla birlikte çalışan ve CPU, bellek ve ağ doluluğu hakkında telemetri gönderen aracılar da vardır. Örneğin, Azure VM'leri, Docker ana bilgisayarları ve [Java EE sunucuları](../../azure-monitor/app/java-agent.md) bu tür aracılar olabilir.
* [Kullanılabilirlik testleri,](../../azure-monitor/app/monitor-web-app-availability.md) microsoft tarafından çalıştırılen ve web uygulamanıza düzenli aralıklarla istek gönderen işlemlerdir. Sonuçlar Application Insights hizmetine gönderilir.

### <a name="what-kinds-of-data-are-collected"></a>Ne tür veriler toplanır?
Ana kategoriler şunlardır:

* [Web sunucusu telemetri](../../azure-monitor/app/asp-net.md) - HTTP istekleri.  Uri, isteği işlemek için zaman, yanıt kodu, istemci IP adresi. `Session id`.
* [Web sayfaları](../../azure-monitor/app/javascript.md) - Sayfa, kullanıcı ve oturum sayıları. Sayfa yükleme süreleri. Özel durum. Ajax arıyor.
* Performans sayaçları - Bellek, CPU, IO, Ağ doluluk.
* İstemci ve sunucu bağlamı - Işletim sistemi, yerel, aygıt türü, tarayıcı, ekran çözünürlüğü.
* [Özel durumlar](../../azure-monitor/app/asp-net-exceptions.md) ve kilitlenmeler `build id`- yığın **dökümleri**, CPU türü. 
* [Bağımlılıklar](../../azure-monitor/app/asp-net-dependencies.md) - REST, SQL, AJAX gibi harici hizmetlere çağrılar. URI veya bağlantı dizesi, süresi, başarı, komut.
* [Kullanılabilirlik testleri](../../azure-monitor/app/monitor-web-app-availability.md) - test süresi ve adımlar, yanıtlar.
* [Günlüklerinize](../../azure-monitor/app/asp-net-trace-logs.md) **veya telemetrinize kodladığınız günlükleri ve**özel [telemetrileri](../../azure-monitor/app/api-custom-events-metrics.md) - takip edin.

[Daha fazla ayrıntı](#data-sent-by-application-insights).

## <a name="how-can-i-verify-whats-being-collected"></a>Toplananları nasıl doğrulayabilirim?
Visual Studio'yu kullanarak uygulamayı geliştiriyorsanız, uygulamayı hata ayıklama modunda (F5) çalıştırın. Telemetri Çıkış penceresinde görünür. Oradan, bunu kopyalayabilir ve kolay inceleme için JSON olarak biçimlendirebilirsiniz. 

![](./media/data-retention-privacy/06-vs.png)

Ayrıca Tanılama penceresinde daha okunabilir bir görünüm vardır.

Web sayfaları için tarayıcınızın hata ayıklama pencereni açın.

![F12 tuşuna basın ve Ağ sekmesini açın.](./media/data-retention-privacy/08-browser.png)

### <a name="can-i-write-code-to-filter-the-telemetry-before-it-is-sent"></a>Telemetri gönderilmeden önce filtrelemek için kod yazabilir miyim?
Bu bir [telemetri işlemci eklentisi](../../azure-monitor/app/api-filtering-sampling.md)yazarak mümkün olacaktır.

## <a name="how-long-is-the-data-kept"></a>Veriler ne kadar süreyle saklanır?
Ham veri noktaları (diğer bir deyişle, Analytics'te sorgulayabildiğiniz ve Arama'da inceleyebileceğiniz öğeler) 730 güne kadar saklanır. Bekletme süresi 30, 60, 90, 120, 180, 270, 365, 550 veya 730 gün [seçebilirsiniz.](https://docs.microsoft.com/azure/azure-monitor/app/pricing#change-the-data-retention-period) Verileri 730 günden uzun tutmanız gerekiyorsa, veri alımı sırasında bir depolama hesabına kopyalamak için [Sürekli Dışa Aktarma'yı](../../azure-monitor/app/export-telemetry.md) kullanabilirsiniz. 

90 günden uzun tutulan veriler ek ücrete tabi olacaktır. [Azure Monitörfiyatlandırma sayfasında](https://azure.microsoft.com/pricing/details/monitor/)Uygulama Öngörüleri fiyatlandırması hakkında daha fazla bilgi edinin.

Toplanan veriler (diğer bir deyişle, ölçümler, ortalamalar ve Metrik Gezgin'de gördüğünüz diğer istatistiksel veriler) 90 gün boyunca 1 dakikalık bir tanede tutulur.

[Hata ayıklama anlık görüntüleri](../../azure-monitor/app/snapshot-debugger.md) 15 gün boyunca saklanır. Bu bekletme ilkesi, uygulama başına olarak ayarlanır. Bu değeri artırmanız gerekiyorsa, Azure portalında bir destek örneği açarak bir artış isteyebilirsiniz.

## <a name="who-can-access-the-data"></a>Verilere kimler erişebilir?
Veriler sizin ve bir kuruluş hesabınız varsa ekip üyeleriniz tarafından görülebilir. 

Siz ve ekip üyeleriniz tarafından dışa aktarılabilir ve başka konumlara kopyalanabilir ve diğer kişilere aktarılabilir.

#### <a name="what-does-microsoft-do-with-the-information-my-app-sends-to-application-insights"></a>Microsoft, uygulamamın Application Insights'a gönderdiği bilgilerle ne yapar?
Microsoft verileri yalnızca hizmeti size sağlamak için kullanır.

## <a name="where-is-the-data-held"></a>Veriler nerede tutuluyor?
* Yeni bir Uygulama Öngörüleri kaynağı oluşturduğunuzda konumu seçebilirsiniz. Uygulama Öngörüleri kullanılabilirliği hakkında daha fazla bilgi [için burada](https://azure.microsoft.com/global-infrastructure/services/?products=all)bölge başına .

#### <a name="does-that-mean-my-app-has-to-be-hosted-in-the-usa-europe-or-southeast-asia"></a>Bu, uygulamamın ABD, Avrupa veya Güneydoğu Asya'da barındırılması gerektiği anlamına mı geliyor?
* Hayır. Başvurunuz, kendi şirket içi ana bilgisayarlarınızda veya bulutta her yerde çalıştırılabilir.

## <a name="how-secure-is-my-data"></a>Verilerim ne kadar güvenli?
Application Insights bir Azure Hizmetidir. Güvenlik ilkeleri [Azure Güvenlik, Gizlilik ve Uyumluluk teknik raporunda](https://go.microsoft.com/fwlink/?linkid=392408)açıklanmıştır.

Veriler Microsoft Azure sunucularında depolanır. Azure portalındaki hesaplar için hesap kısıtlamaları [Azure Güvenlik, Gizlilik ve Uyumluluk belgesinde](https://go.microsoft.com/fwlink/?linkid=392408)açıklanmıştır.

Verilerinize Microsoft personeli tarafından erişim kısıtlanmıştır. Verilerinize yalnızca sizin izniniz ve Uygulama Öngörüleri'ni kullanımınızı desteklemek gerekirse erişiriz. 

Uygulama Öngörülerini geliştirmek için tüm müşterilerimizin uygulamalarında toplam veriler (veri hızları ve ortalama izleme boyutu gibi) kullanılır.

#### <a name="could-someone-elses-telemetry-interfere-with-my-application-insights-data"></a>Başka birinin telemetrisi Application Insights verilerimi etkileyebilir mi?
Web sayfalarınızın kodunda bulunan enstrümantasyon anahtarını kullanarak hesabınıza ek telemetri gönderebilirler. Yeterli ek veriyle, ölçümleriniz uygulamanızın performansını ve kullanımını doğru şekilde göstermez.

Kodu diğer projelerle paylaşıyorsanız, enstrümantasyon anahtarınızı kaldırmayı unutmayın.

## <a name="is-the-data-encrypted"></a>Veriler şifrelendi mi?
Tüm veriler istirahatte ve veri merkezleri arasında hareket ettikçe şifrelenir.

#### <a name="is-the-data-encrypted-in-transit-from-my-application-to-application-insights-servers"></a>Uygulamamdan Application Insights sunucularına aktarım sırasında şifrelenen veriler var mı?
Evet, https'yi, web sunucuları, cihazlar ve HTTPS web sayfaları da dahil olmak üzere neredeyse tüm SDK'lardan portala veri göndermek için kullanırız. 

## <a name="does-the-sdk-create-temporary-local-storage"></a>SDK geçici yerel depolama alanı oluşturuyor mu?

Evet, bir bitiş noktasına ulaşılamıyorsa, bazı Telemetri Kanalları verileri yerel olarak devam eder. Hangi çerçevelerin ve telemetri kanallarının etkilendiğini görmek için lütfen aşağıdaki bölümü inceleyin.

Yerel depolama alanını kullanan Telemetri kanalları, TEMP veya APPDATA dizinlerinde, uygulamanızı çalıştıran belirli hesapla sınırlı geçici dosyalar oluşturur. Bu, bir bitiş noktası geçici olarak kullanılamadığında veya azaltma sınırına bastığında oluşabilir. Bu sorun çözüldükten sonra, telemetri kanalı tüm yeni ve kalıcı verileri göndermeye devam edecektir.

Bu kalıcı veriler yerel olarak şifrelenmez. Bu bir sorunsa, verileri gözden geçirin ve özel verilerin toplanmasını kısıtlayın. (Daha fazla bilgi için özel [verilerin nasıl dışa aktarılabildiğini ve silinine](https://docs.microsoft.com/azure/application-insights/app-insights-customer-data#how-to-export-and-delete-private-data)bakın.)

Bir müşterinin bu dizini belirli güvenlik gereksinimleriyle yapılandırması gerekiyorsa, her çerçeve de yapılandırılabilir. Lütfen uygulamanızı çalıştıran işlemin bu dizine yazma erişimi olduğundan emin olun, ancak istenmeyen kullanıcılar tarafından telemetrinin okunmasını önlemek için bu dizinin korunduğundan da emin olun.

### <a name="java"></a>Java

`C:\Users\username\AppData\Local\Temp`kalıcı veri için kullanılır. Bu konum config dizininden yapılandırılamaz ve bu klasöre erişim izinleri gerekli kimlik bilgilerine sahip belirli kullanıcıyla sınırlıdır. (Daha fazla bilgi için [uygulamaya](https://github.com/Microsoft/ApplicationInsights-Java/blob/40809cb6857231e572309a5901e1227305c27c1a/core/src/main/java/com/microsoft/applicationinsights/internal/util/LocalFileSystemUtils.java#L48-L72)bakın.)

###  <a name="net"></a>.NET

Varsayılan `ServerTelemetryChannel` olarak geçerli kullanıcının yerel uygulama `%localAppData%\Microsoft\ApplicationInsights` veri `%TMP%`klasörünü veya geçici klasörünü kullanır. (Buraya bakın [uygulama.)](https://github.com/Microsoft/ApplicationInsights-dotnet/blob/91e9c91fcea979b1eec4e31ba8e0fc683bf86802/src/ServerTelemetryChannel/Implementation/ApplicationFolderProvider.cs#L54-L84)


Yapılandırma dosyası üzerinden:
```xml
<TelemetryChannel Type="Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel.ServerTelemetryChannel,   Microsoft.AI.ServerTelemetryChannel">
    <StorageFolder>D:\NewTestFolder</StorageFolder>
</TelemetryChannel>
```

Kod üzerinden:

- ServerTelemetryChannel'ı yapılandırma dosyasından kaldırma
- Yapılandırmanıza bu parçacığı ekleyin:
  ```csharp
  ServerTelemetryChannel channel = new ServerTelemetryChannel();
  channel.StorageFolder = @"D:\NewTestFolder";
  channel.Initialize(TelemetryConfiguration.Active);
  TelemetryConfiguration.Active.TelemetryChannel = channel;
  ```

### <a name="netcore"></a>NetCore

Varsayılan `ServerTelemetryChannel` olarak geçerli kullanıcının yerel uygulama `%localAppData%\Microsoft\ApplicationInsights` veri `%TMP%`klasörünü veya geçici klasörünü kullanır. (Buraya bakın [uygulama.)](https://github.com/Microsoft/ApplicationInsights-dotnet/blob/91e9c91fcea979b1eec4e31ba8e0fc683bf86802/src/ServerTelemetryChannel/Implementation/ApplicationFolderProvider.cs#L54-L84) Linux ortamında, depolama klasörü belirtilmediği sürece yerel depolama devre dışı bırakılır.

Aşağıdaki kod snippet sınıfının `ServerTelemetryChannel.StorageFolder` `ConfigureServices()` `Startup.cs` yönteminde nasıl ayarlanır gösterir:

```csharp
services.AddSingleton(typeof(ITelemetryChannel), new ServerTelemetryChannel () {StorageFolder = "/tmp/myfolder"});
```

(Daha fazla bilgi için Bkz. [AspNetCore Özel Yapılandırma.)](https://github.com/Microsoft/ApplicationInsights-aspnetcore/wiki/Custom-Configuration)

### <a name="nodejs"></a>Node.js

Varsayılan `%TEMP%/appInsights-node{INSTRUMENTATION KEY}` olarak kalıcı veri için kullanılır. Bu klasöre erişim izinleri geçerli kullanıcı ve Yöneticiler ile sınırlıdır. (Buraya bakın [uygulama.)](https://github.com/Microsoft/ApplicationInsights-node.js/blob/develop/Library/Sender.ts)

Klasör `appInsights-node` öneki, [Gönderen.ts'de](https://github.com/Microsoft/ApplicationInsights-node.js/blob/7a1ecb91da5ea0febf5ceab13d6a4bf01a63933d/Library/Sender.ts#L384)bulunan statik değişkenin `Sender.TEMPDIR_PREFIX` çalışma zamanı değerini değiştirerek geçersiz kılınabilir.

### <a name="javascript-browser"></a>JavaScript (tarayıcı)

[HTML5 Oturum Depolama](https://developer.mozilla.org/en-US/docs/Web/API/Window/sessionStorage) verileri kalıcı olarak sağlamak için kullanılır. İki ayrı arabellek `AI_buffer` kullanılır: ve `AI_sent_buffer`. Toplu olarak dizilmiş ve gönderilmeyi `AI_buffer`bekleyen telemetri. Yeni gönderilen telemetri, yutma `AI_sent_buffer` sunucusu başarıyla alındığı yanıtını verene kadar yerleştirilir. Telemetri başarıyla alındığı zaman, tüm arabelleklerden kaldırılır. Geçici hatalarda (örneğin, bir kullanıcı ağ bağlantısını kaybeder), telemetri başarılı bir şekilde alınana `AI_buffer` veya yutma sunucusu telemetrinin geçersiz olduğunu yanıtlayana kadar (örneğin kötü şema veya çok eski) kalır.

Telemetri arabellekleri ' [`enableSessionStorageBuffer`](https://github.com/microsoft/ApplicationInsights-JS/blob/17ef50442f73fd02a758fbd74134933d92607ecf/legacy/JavaScript/JavaScriptSDK.Interfaces/IConfig.ts#L31) ye `false`ayarlayarak devre dışı tutulabilir. Oturum depolama alanı kapatıldığında, kalıcı depolama alanı olarak yerel bir dizi kullanılır. JavaScript SDK istemci aygıtında çalıştığından, kullanıcı bu depolama konumuna tarayıcısının geliştirici araçları üzerinden erişebilir.

### <a name="opencensus-python"></a>Açık Nüfus Sayımı Python

Varsayılan olarak OpenCensus Python SDK `%username%/.opencensus/.azure/`geçerli kullanıcı klasörünü kullanır. Bu klasöre erişim izinleri geçerli kullanıcı ve Yöneticiler ile sınırlıdır. (Buraya bakın [uygulama.)](https://github.com/census-instrumentation/opencensus-python/blob/master/contrib/opencensus-ext-azure/opencensus/ext/azure/common/storage.py) Kalıcı verilerinizin yer alan klasörü, telemetriyi oluşturan Python dosyasının adını alacaktır.

Kullandığınız ihracatçının yapısındaki parametreyi `storage_path` geçirerek depolama dosyanızın konumunu değiştirebilirsiniz.

```python
AzureLogHandler(
  connection_string='InstrumentationKey=00000000-0000-0000-0000-000000000000',
  storage_path='<your-path-here>',
)
```

## <a name="how-do-i-send-data-to-application-insights-using-tls-12"></a>TLS 1.2 kullanarak Application Insights'a nasıl veri gönderirim?

Uygulama Öngörüleri uç noktalarına aktahalindeki verilerin güvenliğini sağlamak için, müşterilerin uygulamalarını en az Taşıma Katmanı Güvenliği (TLS) 1,2 kullanacak şekilde yapılandırmalarını önemle tavsiye ederiz. TLS/Secure Sockets Layer'ın (SSL) eski sürümlerinin savunmasız olduğu tespit edilmiştir ve hala geriye dönük uyumluluğa izin vermek için çalışırken, bu sürümler **önerilmez**ve endüstri bu eski protokoller için desteği bırakmak için hızla hareket etmektedir. 

[PCI Güvenlik Standartları Konseyi,](https://www.pcisecuritystandards.org/) TLS/SSL'nin eski sürümlerini devre dışı bırakıp daha güvenli protokollere yükseltmek için [30 Haziran 2018 tarihine kadar süre](https://www.pcisecuritystandards.org/pdfs/PCI_SSC_Migrating_from_SSL_and_Early_TLS_Resource_Guide.pdf) belirlemiştir. Azure eski desteği bıraktıktan sonra, uygulamanız/müşterileriniz en az TLS 1.2 üzerinden iletişim kuramazsa, Uygulama Öngörüleri'ne veri gönderemezsiniz. Uygulamanızın TLS desteğini test etmek ve doğrulamak için aldığınız yaklaşım, işletim sistemine/platformuna ve uygulamanızın kullandığı dile/çerçeveye bağlı olarak değişir.

Uygulamanızı, platform düzeyindeki güvenlik özelliklerini otomatik olarak algılayabilmenize ve daha güvenli protokollerden yararlanmanıza olanak tanıyan platform düzeyindeki güvenlik özelliklerini kırabileceği nden, gerekli olmadıkça yalnızca TLS 1.2'yi kullanacak şekilde ayarlamanızı açıkça önermiyoruz. TL1.3. Belirli TLS/SSL sürümlerinin hardcoding olup olmadığını kontrol etmek için uygulamanızın kodunu kapsamlı bir şekilde denetlemenizi öneririz.

### <a name="platformlanguage-specific-guidance"></a>Platform/Dile özel rehberlik

|Platform / Dil | Destek | Daha Fazla Bilgi |
| --- | --- | --- |
| Azure Uygulama Hizmetleri  | Desteklenen, yapılandırma gerekebilir. | Destek Nisan 2018'de duyuruldu. [Yapılandırma ayrıntıları](https://blogs.msdn.microsoft.com/appserviceteam/2018/04/17/app-service-and-functions-hosted-apps-can-now-update-tls-versions/)için duyuruyu okuyun.  |
| Azure İşlev Uygulamaları | Desteklenen, yapılandırma gerekebilir. | Destek Nisan 2018'de duyuruldu. [Yapılandırma ayrıntıları](https://blogs.msdn.microsoft.com/appserviceteam/2018/04/17/app-service-and-functions-hosted-apps-can-now-update-tls-versions/)için duyuruyu okuyun. |
|.NET | Desteklenen yapılandırma sürüme göre değişir. | .NET 4.7 ve önceki sürümler için ayrıntılı yapılandırma bilgileri için [bu talimatlara](https://docs.microsoft.com/dotnet/framework/network-programming/tls#support-for-tls-12)bakın.  |
|Durum İzleyicisi | Desteklenen, yapılandırma gerekli | Durum İzleyicisi, TLS 1.2'yi desteklemek için OS + [Configuration .NET](https://docs.microsoft.com/dotnet/framework/network-programming/tls#support-for-tls-12) [Configuration'a](https://docs.microsoft.com/windows-server/security/tls/tls-registry-settings)dayanır.
|Node.js |  V10.5.0'da desteklenen yapılandırma gerekebilir. | Uygulamaya özel yapılandırma için [resmi Node.js TLS/SSL belgelerini](https://nodejs.org/api/tls.html) kullanın. |
|Java | Desteklenen, TLS 1.2 için JDK desteği [JDK 6 güncelleme 121](https://www.oracle.com/technetwork/java/javase/overview-156328.html#R160_121) ve [JDK 7](https://www.oracle.com/technetwork/java/javase/7u131-relnotes-3338543.html)eklendi. | JDK 8 [varsayılan olarak TLS 1.2](https://blogs.oracle.com/java-platform-group/jdk-8-will-use-tls-12-as-default)kullanır.  |
|Linux | Linux dağıtımları TLS 1.2 desteği için [OpenSSL'ye](https://www.openssl.org) güvenme eğilimindedir.  | OpenSSL sürümünüzün desteklenir olduğunu doğrulamak için [OpenSSL Changelog'u](https://www.openssl.org/news/changelog.html) kontrol edin.|
| Windows 8.0 - 10 arası | Desteklenen ve varsayılan olarak etkinleştirildi. | [Varsayılan ayarları](https://docs.microsoft.com/windows-server/security/tls/tls-registry-settings)hala kullandığınızı doğrulamak için.  |
| Windows Server 2012 - 2016 | Desteklenen ve varsayılan olarak etkinleştirildi. | [Varsayılan ayarları](https://docs.microsoft.com/windows-server/security/tls/tls-registry-settings) hala kullandığınızı doğrulamak için |
| Windows 7 SP1 ve Windows Server 2008 R2 SP1 | Desteklenen, ancak varsayılan olarak etkin değil. | Nasıl etkinleştirilene ilişkin ayrıntılar için [Aktarım Katmanı Güvenliği (TLS) kayıt defteri ayarları](https://docs.microsoft.com/windows-server/security/tls/tls-registry-settings) sayfasına bakın.  |
| Windows Server 2008 SP2 | TLS 1.2 desteği için bir güncelleme gerektirir. | Windows Server 2008 SP2'de [TLS 1.2 desteği eklemek için Güncelleştirme'ye](https://support.microsoft.com/help/4019276/update-to-add-support-for-tls-1-1-and-tls-1-2-in-windows-server-2008-s) bakın. |
|Windows Vista | Desteklenmiyor. | Yok

### <a name="check-what-version-of-openssl-your-linux-distribution-is-running"></a>Linux dağıtımınızın openssl sürümünün ne şekilde çalıştığını kontrol edin

OpenSSL'nin hangi sürümünü yüklediğinizi kontrol etmek için terminali açın ve çalıştırın:

```terminal
openssl version -a
```

### <a name="run-a-test-tls-12-transaction-on-linux"></a>Linux üzerinde bir test TLS 1.2 işlem çalıştırın

Linux sisteminizin TLS 1.2 üzerinden iletişim kurup kurap kurap kurap kurap kuraap kuraap kuramayacağı konusunda bir ön test çalıştırmak için terminali açın ve çalıştırın:

```terminal
openssl s_client -connect bing.com:443 -tls1_2
```

## <a name="personal-data-stored-in-application-insights"></a>Uygulama Öngörüleri'nde depolanan kişisel veriler

[Uygulama Öngörüleri kişisel veri makalemiz](../../azure-monitor/platform/personal-data-mgmt.md) bu konuyu derinlemesine ele adabEder.

#### <a name="can-my-users-turn-off-application-insights"></a>Kullanıcılarım Application Insights'ı kapatabilir mi?
Doğrudan değil. Kullanıcılarınızın Uygulama Öngörülerini kapatmak için kullanabileceği bir anahtar sağlamayız.

Ancak, uygulamanızda böyle bir özellik uygulayabilirsiniz. Tüm SDK'lar, telemetri koleksiyonunu kapatan bir API ayarı içerir. 

## <a name="data-sent-by-application-insights"></a>Application Insights tarafından gönderilen veriler
SDK'lar platformlar arasında değişir ve yükleyebileceğiniz birkaç bileşen vardır. (Uygulama [Öngörüleri][start]bakınız - genel bakış .) Her bileşen farklı veri gönderir.

#### <a name="classes-of-data-sent-in-different-scenarios"></a>Farklı senaryolarda gönderilen veri sınıfları

| Eyleminiz | Toplanan veri sınıfları (sonraki tabloya bakın) |
| --- | --- |
| [Bir .NET web projesine Uygulama Öngörüleri SDK ekle][greenbrown] |Sunucu Bağlamı<br/>Olayla<br/>Performans sayaçları<br/>İstekler<br/>**Özel durumlar**<br/>Oturum<br/>kullanıcılar |
| [Durum Monitörünü IIS'de yükleme][redfield] |Bağımlılıklar<br/>Sunucu Bağlamı<br/>Olayla<br/>Performans sayaçları |
| [Java web uygulamasına Uygulama Öngörüleri SDK ekle][java] |Sunucu Bağlamı<br/>Olayla<br/>İstek<br/>Oturum<br/>kullanıcılar |
| [JavaScript SDK'yı web sayfasına ekleme][client] |Clientcontext <br/>Olayla<br/>Sayfa<br/>MüşteriPerf<br/>Ajax |
| [Varsayılan özellikleri tanımlama][apiproperties] |Tüm standart ve özel olaylardaki **özellikler** |
| [TrackMetric'i Arayın][api] |Sayısal değerler<br/>**Özellikler** |
| [Çağrı Parça*][api] |Olay adı<br/>**Özellikler** |
| [TrackException'ı Arayın][api] |**Özel durumlar**<br/>Yığın dökümü<br/>**Özellikler** |
| SDK veri toplayamıyor. Örnek: <br/> - perf sayaçlarına erişemiyorum<br/> - telemetri başlandırıcı istisna |SDK tanılama |

[Diğer platformlar için SDK'lar için][platforms]belgelerine bakın.

#### <a name="the-classes-of-collected-data"></a>Toplanan verilerin sınıfları

| Toplanan veri sınıfı | Içerir (ayrıntılı bir liste değil) |
| --- | --- |
| **Özellikler** |**Herhangi bir veri - kodunuz tarafından belirlenir** |
| Aygıt Bağlamı |`Id`, IP, Locale, Aygıt modeli, ağ, ağ türü, OEM adı, ekran çözünürlüğü, Rol Örneği, Rol Adı, Aygıt Türü |
| Clientcontext |İşletim sistemi, yerel, dil, ağ, pencere çözünürlüğü |
| Oturum |`session id` |
| Sunucu Bağlamı |Makine adı, yerel, işletim sistemi, cihaz, kullanıcı oturumu, kullanıcı bağlamı, çalışma |
| Olayla |IP adresi, zaman damgası, işletim sistemi, tarayıcıdan coğrafi konum |
| Ölçümler |Metrik ad ve değer |
| Olaylar |Olay adı ve değeri |
| Pageviews |URL ve sayfa adı veya ekran adı |
| İstemci perf |URL/sayfa adı, tarayıcı yükleme süresi |
| Ajax |Web sayfasından sunucuya HTTP çağrıları |
| İstekler |URL, süre, yanıt kodu |
| Bağımlılıklar |Türü (SQL, HTTP, ...), bağlantı dizesi veya URI, eşitleme/eşitleme, süre, başarı, SQL deyimi (Status Monitor ile) |
| **Özel durumlar** |Türü, **ileti,** çağrı yığınları, kaynak dosya, satır numarası,`thread id` |
| Çökü -yor |`Process id`, `parent process id`, `crash thread id`; uygulama yama, `id`inşa;  özel durum türü, adresi, nedeni; obfuscated semboller ve kayıtlar, ikili başlangıç ve bitiş adresleri, ikili adı ve yolu, cpu türü |
| İzleme |**İleti** ve önem düzeyi |
| Performans sayaçları |İşlemci süresi, kullanılabilir bellek, istek oranı, özel bayt işlemi, IO oranı, istek süresi, istek sırası uzunluğu |
| Kullanılabilirlik |Web test yanıt kodu, her test adımının süresi, test adı, zaman damgası, başarı, yanıt süresi, test yeri |
| SDK tanılama |İletiyi veya Özel Durum'u izleme |

[ApplicationInsights.config'i düzenleyerek bazı verileri kapatabilirsiniz][config]

> [!NOTE]
> İstemci IP coğrafi konumu çıkarmak için kullanılır, ancak varsayılan OLARAK IP verileri artık depolanır ve tüm sıfırlar ilişkili alana yazılır. Kişisel veri işleme hakkında daha fazla bilgi için bu [makaleyi](../../azure-monitor/platform/personal-data-mgmt.md#application-data)öneririz. IP adresi verilerini depolamanız [gerekiyorsa, IP adresi toplama makalemiz](https://docs.microsoft.com/azure/azure-monitor/app/ip-collection) seçeneklerinizi size iletecektir.

## <a name="credits"></a>Krediler
Bu ürün MaxMind tarafından oluşturulan GeoLite2 [https://www.maxmind.com](https://www.maxmind.com)verilerini içerir, kullanılabilir.



<!--Link references-->

[api]: ../../azure-monitor/app/api-custom-events-metrics.md
[apiproperties]: ../../azure-monitor/app/api-custom-events-metrics.md#properties
[client]: ../../azure-monitor/app/javascript.md
[config]: ../../azure-monitor/app/configuration-with-applicationinsights-config.md
[greenbrown]: ../../azure-monitor/app/asp-net.md
[java]: ../../azure-monitor/app/java-get-started.md
[platforms]: ../../azure-monitor/app/platforms.md
[pricing]: https://azure.microsoft.com/pricing/details/application-insights/
[redfield]: ../../azure-monitor/app/monitor-performance-live-website-now.md
[start]: ../../azure-monitor/app/app-insights-overview.md
