---
title: Giriş Doğrulama - Microsoft Tehdit Modelleme Aracı - Azure | Microsoft Dokümanlar
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
ms.openlocfilehash: 712a0707826f97f29b015a2c5892f8d20577e41b
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81687891"
---
# <a name="security-frame-input-validation--mitigations"></a>Güvenlik Çerçevesi: Giriş Doğrulama | Azaltıcı etken 
| Ürün / Hizmet | Makale |
| --------------- | ------- |
| **Web Uygulaması** | <ul><li>[Güvenilmeyen stil sayfaları kullanarak tüm dönüşümler için XSLT komut dosyalarını devre dışı](#disable-xslt)</li><li>[Kullanıcı denetlenebilir içerik içerebilecek her sayfanın otomatik MIME koklamayı devre dışı bırakmasını sağlamak](#out-sniffing)</li><li>[XML Varlık Çözünürlüğünü Sertleştirme veya Devre Dışı](#xml-resolution)</li><li>[http.sys kullanan uygulamalar URL kanoniizasyon doğrulama gerçekleştirmek](#app-verification)</li><li>[Kullanıcılardan dosya kabul ederken uygun denetimlerin yerinde olduğundan emin olun](#controls-users)</li><li>[Veri erişimi için Web Application'da tür güvenliği parametrelerinin kullanıldığından emin olun](#typesafe)</li><li>[MVC toplu atama güvenlik açığını önlemek için ayrı model bağlama sınıfları veya bağlama filtresi listeleri kullanın](#binding-mvc)</li><li>[Oluşturmadan önce güvenilmeyen web çıktısını kodlama](#rendering)</li><li>[Tüm dize türü Model özelliklerinde giriş doğrulama ve filtreleme gerçekleştirin](#typemodel)</li><li>[Sanitizasyon, zengin metin düzenleyicisi gibi tüm karakterleri kabul eden form alanlarına uygulanmalıdır](#richtext)</li><li>[Dahili kodlaması olmayan lavabolara DOM öğeleri atamayın](#inbuilt-encode)</li><li>[Uygulama içindeki tüm yönlendirmelerin kapatılabileceğini veya güvenli bir şekilde yapıldığını doğrulayın](#redirect-safe)</li><li>[Denetleyici yöntemleri tarafından kabul edilen tüm dize türü parametrelerine giriş doğrulaması uygulayın](#string-method)</li><li>[Kötü normal ifadeler nedeniyle DoS'u önlemek için normal ifade işleme için üst sınır zaman anına ayarlayın](#dos-expression)</li><li>[Html.Raw'ı Razor görünümlerinde kullanmaktan kaçının](#html-razor)</li></ul> | 
| **Veritabanı** | <ul><li>[Depolanan yordamlarda dinamik sorgular kullanmayın](#stored-proc)</li></ul> |
| **Web API** | <ul><li>[Web API yöntemlerinde model doğrulamanın yapıldığından emin olun](#validation-api)</li><li>[Web API yöntemleri tarafından kabul edilen tüm dize türü parametrelerine giriş doğrulaması uygulama](#string-api)</li><li>[Veri erişimi için Web API'sinde tür güvenliği parametrelerinin kullanıldığından emin olun](#typesafe-api)</li></ul> | 
| **Azure Belge DB** | <ul><li>[Azure Cosmos DB için parametreli SQL sorgularını kullanma](#sql-docdb)</li></ul> | 
| **WCF** | <ul><li>[Schema bağlama yoluyla WCF Giriş doğrulama](#schema-binding)</li><li>[Parametre Denetçileri aracılığıyla WCF- Giriş doğrulaması](#parameters)</li></ul> |

## <a name="disable-xslt-scripting-for-all-transforms-using-untrusted-style-sheets"></a><a id="disable-xslt"></a>Güvenilmeyen stil sayfaları kullanarak tüm dönüşümler için XSLT komut dosyalarını devre dışı

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL Fazı**               | Yapı |  
| **Uygulanabilir Teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvurular**              | [XSLT Güvenlik](https://msdn.microsoft.com/library/ms763800(v=vs.85).aspx), [XsltSettings.EnableScript Özellik](https://msdn.microsoft.com/library/system.xml.xsl.xsltsettings.enablescript.aspx) |
| **Adımlar** | XSLT, öğeyi kullanarak stil `<msxml:script>` sayfaları içinde komut dosyası eklemeyi destekler. Bu, özel işlevlerin XSLT dönüşümünde kullanılmasını sağlar. Komut dosyası dönüştürme gerçekleştiren işlem bağlamında yürütülür. XSLT komut dosyası, güvenilmeyen bir ortamda, güvenilmeyen kodun yürütülmesini önlemek için devre dışı bırakılmalı. *.NET kullanıyorsanız:* XSLT komut dosyası varsayılan olarak devre dışı bırakılır; ancak, `XsltSettings.EnableScript` özellik aracılığıyla açıkça etkinleştirilemediğinden emin olmalısınız.|

### <a name="example"></a>Örnek 

```csharp
XsltSettings settings = new XsltSettings();
settings.EnableScript = true; // WRONG: THIS SHOULD BE SET TO false
```

### <a name="example"></a>Örnek
MSXML 6.0 kullanıyorsanız, XSLT komut dosyası varsayılan olarak devre dışı bırakılır; ancak, XML DOM nesne özelliği AllowXsltScript aracılığıyla açıkça etkinleştirilen olmadığından emin olmalısınız. 

```csharp
doc.setProperty("AllowXsltScript", true); // WRONG: THIS SHOULD BE SET TO false
```

### <a name="example"></a>Örnek
MSXML 5 veya altında kullanıyorsanız, XSLT komut dosyası varsayılan olarak etkinleştirilir ve bunu açıkça devre dışı düşürmeniz gerekir. XML DOM nesne özelliği AllowXsltScript'i yanlış olarak ayarlayın. 

```csharp
doc.setProperty("AllowXsltScript", false); // CORRECT. Setting to false disables XSLT scripting.
```

## <a name="ensure-that-each-page-that-could-contain-user-controllable-content-opts-out-of-automatic-mime-sniffing"></a><a id="out-sniffing"></a>Kullanıcı denetlenebilir içerik içerebilecek her sayfanın otomatik MIME koklamayı devre dışı bırakmasını sağlamak

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL Fazı**               | Yapı |  
| **Uygulanabilir Teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvurular**              | [IE8 Güvenlik Bölüm V - Kapsamlı Koruma](https://blogs.msdn.com/ie/archive/2008/07/02/ie8-security-part-v-comprehensive-protection.aspx)  |
| **Adımlar** | <p>Kullanıcı denetlenebilir içerik içerebilecek her sayfa için HTTP `X-Content-Type-Options:nosniff`Üstbilgi'ni kullanmanız gerekir. Bu gereksinime uymak için, yalnızca kullanıcı tarafından denetlenebilir içerik içerebilecek sayfalar için gerekli üstbilgi sayfasını sayfa sayfa olarak ayarlayabilir veya uygulamadaki tüm sayfalar için genel olarak ayarlayabilirsiniz.</p><p>Bir web sunucusundan teslim edilen her dosya türü, içeriğin doğasını (yani görüntü, metin, uygulama, vb.) açıklayan ilişkili bir [MIME türüne](https://en.wikipedia.org/wiki/Mime_type) *(içerik türü*olarak da adlandırılır) sahiptir.</p><p>X-İçerik Türü Seçenekleri üstbilgisi, geliştiricilerin içeriklerinin MIME koklanmaması gerektiğini belirtmelerine olanak tanıyan bir HTTP üstbilgidir. Bu üstbilgi, MIME Koklama saldırılarını azaltmak için tasarlanmıştır. Bu üstbilgi için destek Internet Explorer 8 (IE8) eklendi</p><p>Yalnızca Internet Explorer 8 (IE8) kullanıcıları X-İçerik Türü Seçenekleri'nden yararlanabilir. Internet Explorer'ın önceki sürümleri şu anda X-İçerik Türü Seçenekleri üstbilgisine saygı duymadığı</p><p>Internet Explorer 8 (ve daha sonra) bir MIME koklama opt-out özelliği uygulamak için sadece büyük tarayıcılar vardır. Diğer büyük tarayıcılar (Firefox, Safari, Chrome) benzer özellikler uygularsa, bu öneri bu tarayıcılar için sözdizimini de içerecek şekilde güncellenir</p>|

### <a name="example"></a>Örnek
Uygulamadaki tüm sayfalar için gerekli üstbilginin genel olarak etkin olmasını sağlamak için aşağıdakilerden birini yapabilirsiniz: 

* Uygulama Internet Information Services (IIS) tarafından barındırılan web.config dosyasına üstbilgi ekleyin 7 

```
<system.webServer> 
  <httpProtocol> 
    <customHeaders> 
      <add name=""X-Content-Type-Options"" value=""nosniff""/>
    </customHeaders>
  </httpProtocol>
</system.webServer> 
```

* Genel Uygulama\_BeginRequest üzerinden üstbilgi ekleme 

``` 
void Application_BeginRequest(object sender, EventArgs e)
{
  this.Response.Headers[""X-Content-Type-Options""] = ""nosniff"";
} 
```

* Özel HTTP modüllerini uygulayın 

``` 
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
        if (application.Response.Headers[""X-Content-Type-Options ""] != null) 
          return; 
        application.Response.Headers.Add(""X-Content-Type-Options "", ""nosniff""); 
      } 
  } 

``` 

* Tek tek yanıtlara ekleyerek yalnızca belirli sayfalar için gerekli üstbilgiyi etkinleştirebilirsiniz: 

```
this.Response.Headers[""X-Content-Type-Options""] = ""nosniff""; 
``` 

## <a name="harden-or-disable-xml-entity-resolution"></a><a id="xml-resolution"></a>XML Varlık Çözünürlüğünü Sertleştirme veya Devre Dışı

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL Fazı**               | Yapı |  
| **Uygulanabilir Teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvurular**              | [XML Entity Expansion](https://capec.mitre.org/data/definitions/197.html), [XML Hizmet Reddi Saldırıları ve Savunmaları](https://msdn.microsoft.com/magazine/ee335713.aspx), [MSXML Güvenlik Genel Bakış](https://msdn.microsoft.com/library/ms754611(v=VS.85).aspx), MSXML Kodunu [NSXMLParserDelege Protokolü Referansı](https://developer.apple.com/library/ios/#documentation/cocoa/reference/NSXMLParserDelegate_Protocol/Reference/Reference.html), [Dış BaşvurularıN Çözümü](https://msdn.microsoft.com/library/5fcwybb2.aspx) [için En İyi Uygulamalar](https://msdn.microsoft.com/library/ms759188(VS.85).aspx) |
| **Adımlar**| <p>Yaygın olarak kullanılmasa da, XML arayıcısının belgenin kendisi içinde veya dış kaynaklardan tanımlanan değerlerle makro varlıkları genişletmesine olanak tanıyan bir Özelliği Vardır. Örneğin, belge "Microsoft" değerine sahip bir varlık "şirket adı" tanımlayabilir, böylece belgede " "&companyname;metni her göründüğünde otomatik olarak Microsoft metniyle değiştirilir. Veya belge, Microsoft stokunun geçerli değerini almak için harici bir web hizmetine başvuran "MSFTStock" bir varlık tanımlayabilir.</p><p>Daha sonra&MSFTStock;belgede " " " göründüğü nde, otomatik olarak geçerli hisse senedi fiyatıyla değiştirilir. Ancak, bu işlevsellik hizmet reddi (DoS) koşulları oluşturmak için kötüye olabilir. Saldırgan, sistemdeki tüm kullanılabilir belleği tüketen üstel genişletme XML bombası oluşturmak için birden çok varlığı yuvalayabilir. </p><p>Alternatif olarak, sonsuz miktarda veriyi geri aktaran veya iş parçacığına asılan harici bir başvuru oluşturabilir. Sonuç olarak, uygulamaları kullanmıyorsa tüm ekipler dahili ve/veya harici XML varlık çözünürlüğünü tamamen devre dışı etmeli veya bu işlevsellik kesinlikle gerekliyse uygulamanın varlık çözümü için kullanabileceği bellek ve süre miktarını el ile sınırlamalıdır. Uygulamanız tarafından varlık çözünürlüğü gerekli değilse, devre dışı k.s. </p>|

### <a name="example"></a>Örnek
.NET Framework kodu için aşağıdaki yaklaşımları kullanabilirsiniz:

```csharp
XmlTextReader reader = new XmlTextReader(stream);
reader.ProhibitDtd = true;

XmlReaderSettings settings = new XmlReaderSettings();
settings.ProhibitDtd = true;
XmlReader reader = XmlReader.Create(stream, settings);

// for .NET 4
XmlReaderSettings settings = new XmlReaderSettings();
settings.DtdProcessing = DtdProcessing.Prohibit;
XmlReader reader = XmlReader.Create(stream, settings);
```
In varsayılan değerinin doğru olduğunu, ancak `XmlTextReader` içinde yanlış olduğunu unutmayın. `XmlReaderSettings` `ProhibitDtd` XmlReaderSettings kullanıyorsanız, açıkça doğru ProhibitDtd ayarlamak gerekmez, ancak bunu güvenlik uğruna tavsiye edilir. Ayrıca XmlDocument sınıfının varsayılan olarak varlık çözünürlüğü ne izin verdiğini unutmayın. 

### <a name="example"></a>Örnek
XmlDocuments için varlık çözümünü devre `XmlDocument.Load(XmlReader)` dışı kullanabilirsiniz, Yükleme yönteminin aşırı yükünü kullanın ve aşağıdaki kodda gösterildiği gibi çözünürlüğü devre dışı bırakabilmek için XmlReader bağımsız değişkenindeki uygun özellikleri ayarlayın: 

```csharp
XmlReaderSettings settings = new XmlReaderSettings();
settings.ProhibitDtd = true;
XmlReader reader = XmlReader.Create(stream, settings);
XmlDocument doc = new XmlDocument();
doc.Load(reader);
```

### <a name="example"></a>Örnek
Uygulamanız için varlık çözünürlüğünü devre dışı bırakmak mümkün değilse, XmlReaderSettings.MaxCharactersFromEntities özelliğini uygulamanızın ihtiyaçlarına göre makul bir değere ayarlayın. Bu, olası üstel genişleme DoS saldırılarının etkisini sınırlandıracaktır. Aşağıdaki kod bu yaklaşımın bir örneğini sağlar: 

```csharp
XmlReaderSettings settings = new XmlReaderSettings();
settings.ProhibitDtd = false;
settings.MaxCharactersFromEntities = 1000;
XmlReader reader = XmlReader.Create(stream, settings);
```

### <a name="example"></a>Örnek
Satır satırlı varlıkları çözmeniz gerekiyorsa ancak dış varlıkları çözmeniz gerekmiyorsa, XmlReaderSettings.XmlResolver özelliğini geçersiz kılacak şekilde ayarlayın. Örneğin: 

```csharp
XmlReaderSettings settings = new XmlReaderSettings();
settings.ProhibitDtd = false;
settings.MaxCharactersFromEntities = 1000;
settings.XmlResolver = null;
XmlReader reader = XmlReader.Create(stream, settings);
```
MSXML6'da ProhibitDTD'nin varsayılan olarak doğru (DTD işlemini devre dışı bırakma) olarak ayarladığını unutmayın. Apple OSX/iOS kodu için kullanabileceğiniz iki XML ayrıştırıcı vardır: NSXMLParser ve libXML2. 

## <a name="applications-utilizing-httpsys-perform-url-canonicalization-verification"></a><a id="app-verification"></a>http.sys kullanan uygulamalar URL kanoniizasyon doğrulama gerçekleştirmek

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL Fazı**               | Yapı |  
| **Uygulanabilir Teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvurular**              | Yok  |
| **Adımlar** | <p>http.sys kullanan herhangi bir uygulama aşağıdaki yönergeleri takip etmelidir:</p><ul><li>URL uzunluğunu en fazla 16.384 karakterle (ASCII veya Unicode) sınırlayın. Bu, varsayılan Internet Bilgi Hizmetleri (IIS) 6 ayarını temel alan mutlak maksimum URL uzunluğudur. Web siteleri mümkünse bundan daha kısa bir süre için çaba göstermelidir</li><li>Standart .NET Framework file G/Ç sınıflarını (FileStream gibi) kullanın, çünkü bunlar .NET FX'teki kurallılaştırma kurallarından yararlanacaktır.</li><li>Bilinen dosya adlarının izin listesini açıkça oluşturma</li><li>UrlScan reddi sunmayacağım bilinen filetipleri açıkça reddedin: exe, bat, cmd, com, htw, ida, idq, htr, idc, shtm[l], stm, yazıcı, ini, pol, dat dosyaları</li><li>Aşağıdaki özel durumları yakalayın:<ul><li>System.ArgumentException (aygıt adları için)</li><li>System.NotSupportedException (veri akışları için)</li><li>System.IO.FileNotFoundException (geçersiz kaçan dosya adları için)</li><li>System.IO.DirectoryNotFoundException (geçersiz kaçan dirs için)</li></ul></li><li>Win32 file G/O API'lerine *seslenmeyin.* Geçersiz bir URL'de, kullanıcıya bir 400 hatasını zarif bir şekilde döndürün ve gerçek hatayı günlüğe kaydedin.</li></ul>|

## <a name="ensure-appropriate-controls-are-in-place-when-accepting-files-from-users"></a><a id="controls-users"></a>Kullanıcılardan dosya kabul ederken uygun denetimlerin yerinde olduğundan emin olun

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL Fazı**               | Yapı |  
| **Uygulanabilir Teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvurular**              | [Sınırsız Dosya Yükleme](https://www.owasp.org/index.php/Unrestricted_File_Upload), [Dosya İmza Tablosu](https://www.garykessler.net/library/file_sigs.html) |
| **Adımlar** | <p>Yüklenen dosyalar uygulamalar için önemli bir risk oluşturur.</p><p>Birçok saldırının ilk adımı saldırıya uğramak için sisteme bazı kodlar almaktır. O zaman saldırının sadece kodu niçin yürütültecek bir yol bulması gerekiyor. Dosya yüklemesi kullanmak, saldırganın ilk adımı gerçekleştirmesini sağlar. Sınırsız dosya yüklemenin sonuçları, tam sistem devralma, aşırı yüklü bir dosya sistemi veya veritabanı, saldırıları arka uç sistemlerine iletme ve basit tahrifat gibi değişebilir.</p><p>Uygulamanın yüklenen dosyayla ne yaptığına ve özellikle nerede depolandığına bağlıdır. Dosya yüklemelerinin sunucu tarafı doğrulaması eksik. Dosya Yükleme işlevi için aşağıdaki güvenlik denetimleri uygulanmalıdır:</p><ul><li>Dosya Uzantısı denetimi (yalnızca geçerli bir izin verilen dosya türü kümesi kabul edilmelidir)</li><li>Maksimum dosya boyutu sınırı</li><li>Dosya webroot'a yüklenmemelidir; konum sistem dışı sürücü de bir dizin olmalıdır</li><li>Yüklenen dosya adının bazı rasgelelikleri olması, böylece dosya nın üzerine yazılmasının engellenmesi için adlandırma kuralı izlenmelidir</li><li>Dosyalar diske yazmadan önce anti-virüs için taranmalıdır</li><li>Dosya adının ve diğer meta verilerin (örn. dosya yolu) kötü amaçlı karakterler için doğrulandığından emin olun</li><li>Dosya biçimi imzası, kullanıcının maskelenmiş bir dosya yüklemesini önlemek için kontrol edilmelidir (örn. uzantıyı txt olarak değiştirerek exe dosyası yüklemek)</li></ul>| 

### <a name="example"></a>Örnek
Dosya biçimi imza doğrulaması ile ilgili son nokta için ayrıntılar için aşağıdaki sınıfa bakın: 

```csharp
        private static Dictionary<string, List<byte[]>> fileSignature = new Dictionary<string, List<byte[]>>
                    {
                    { ".DOC", new List<byte[]> { new byte[] { 0xD0, 0xCF, 0x11, 0xE0, 0xA1, 0xB1, 0x1A, 0xE1 } } },
                    { ".DOCX", new List<byte[]> { new byte[] { 0x50, 0x4B, 0x03, 0x04 } } },
                    { ".PDF", new List<byte[]> { new byte[] { 0x25, 0x50, 0x44, 0x46 } } },
                    { ".ZIP", new List<byte[]> 
                                            {
                                              new byte[] { 0x50, 0x4B, 0x03, 0x04 },
                                              new byte[] { 0x50, 0x4B, 0x4C, 0x49, 0x54, 0x55 },
                                              new byte[] { 0x50, 0x4B, 0x53, 0x70, 0x58 },
                                              new byte[] { 0x50, 0x4B, 0x05, 0x06 },
                                              new byte[] { 0x50, 0x4B, 0x07, 0x08 },
                                              new byte[] { 0x57, 0x69, 0x6E, 0x5A, 0x69, 0x70 }
                                                }
                                            },
                    { ".PNG", new List<byte[]> { new byte[] { 0x89, 0x50, 0x4E, 0x47, 0x0D, 0x0A, 0x1A, 0x0A } } },
                    { ".JPG", new List<byte[]>
                                    {
                                              new byte[] { 0xFF, 0xD8, 0xFF, 0xE0 },
                                              new byte[] { 0xFF, 0xD8, 0xFF, 0xE1 },
                                              new byte[] { 0xFF, 0xD8, 0xFF, 0xE8 }
                                    }
                                    },
                    { ".JPEG", new List<byte[]>
                                        { 
                                            new byte[] { 0xFF, 0xD8, 0xFF, 0xE0 },
                                            new byte[] { 0xFF, 0xD8, 0xFF, 0xE2 },
                                            new byte[] { 0xFF, 0xD8, 0xFF, 0xE3 }
                                        }
                                        },
                    { ".XLS", new List<byte[]>
                                            {
                                              new byte[] { 0xD0, 0xCF, 0x11, 0xE0, 0xA1, 0xB1, 0x1A, 0xE1 },
                                              new byte[] { 0x09, 0x08, 0x10, 0x00, 0x00, 0x06, 0x05, 0x00 },
                                              new byte[] { 0xFD, 0xFF, 0xFF, 0xFF }
                                            }
                                            },
                    { ".XLSX", new List<byte[]> { new byte[] { 0x50, 0x4B, 0x03, 0x04 } } },
                    { ".GIF", new List<byte[]> { new byte[] { 0x47, 0x49, 0x46, 0x38 } } }
                };

        public static bool IsValidFileExtension(string fileName, byte[] fileData, byte[] allowedChars)
        {
            if (string.IsNullOrEmpty(fileName) || fileData == null || fileData.Length == 0)
            {
                return false;
            }

            bool flag = false;
            string ext = Path.GetExtension(fileName);
            if (string.IsNullOrEmpty(ext))
            {
                return false;
            }

            ext = ext.ToUpperInvariant();

            if (ext.Equals(".TXT") || ext.Equals(".CSV") || ext.Equals(".PRN"))
            {
                foreach (byte b in fileData)
                {
                    if (b > 0x7F)
                    {
                        if (allowedChars != null)
                        {
                            if (!allowedChars.Contains(b))
                            {
                                return false;
                            }
                        }
                        else
                        {
                            return false;
                        }
                    }
                }

                return true;
            }

            if (!fileSignature.ContainsKey(ext))
            {
                return true;
            }

            List<byte[]> sig = fileSignature[ext];
            foreach (byte[] b in sig)
            {
                var curFileSig = new byte[b.Length];
                Array.Copy(fileData, curFileSig, b.Length);
                if (curFileSig.SequenceEqual(b))
                {
                    flag = true;
                    break;
                }
            }

            return flag;
        }
```

## <a name="ensure-that-type-safe-parameters-are-used-in-web-application-for-data-access"></a><a id="typesafe"></a>Veri erişimi için Web Application'da tür güvenliği parametrelerinin kullanıldığından emin olun

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL Fazı**               | Yapı |  
| **Uygulanabilir Teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvurular**              | Yok  |
| **Adımlar** | <p>Parametreler koleksiyonunu kullanırsanız, SQL girişi gerçek bir değer olarak değil, yürütülebilir kod olarak ele alır. Parametreler toplama giriş verilerinde tür ve uzunluk kısıtlamaları zorlamak için kullanılabilir. Aralığın dışındaki değerler bir özel durum tetikler. Tür açısından güvenli SQL parametreleri kullanılmazsa, saldırganlar filtreuygulanmamış girişe katıştırılmış enjeksiyon saldırılarını gerçekleştirebilir.</p><p>Filtreuygulanmamış girişle oluşabilecek olası SQL enjeksiyon saldırılarından kaçınmak için SQL sorguları yaparken tür güvenli parametrelerini kullanın. Depolanan yordamlarla ve dinamik SQL deyimleriyle güvenli parametreler ilerler. Parametreler, yürütülebilir kod olarak değil, veritabanı tarafından gerçek değerler olarak kabul edilir. Parametreler de türü ve uzunluğu için denetlenir.</p>|

### <a name="example"></a>Örnek 
Aşağıdaki kod, depolanan bir yordamı ararken SqlParameterCollection ile tür güvenli parametrelerinin nasıl kullanılacağını gösterir. 

```csharp
using System.Data;
using System.Data.SqlClient;

using (SqlConnection connection = new SqlConnection(connectionString))
{ 
DataSet userDataset = new DataSet(); 
SqlDataAdapter myCommand = new SqlDataAdapter("LoginStoredProcedure", connection); 
myCommand.SelectCommand.CommandType = CommandType.StoredProcedure; 
myCommand.SelectCommand.Parameters.Add("@au_id", SqlDbType.VarChar, 11); 
myCommand.SelectCommand.Parameters["@au_id"].Value = SSN.Text; 
myCommand.Fill(userDataset);
}  
```
Önceki kod örneğinde, giriş değeri 11 karakterden uzun olamaz. Veriler parametre tarafından tanımlanan tür veya uzunluğa uymuyorsa, SqlParameter sınıfı bir özel durum oluşturur. 

## <a name="use-separate-model-binding-classes-or-binding-filter-lists-to-prevent-mvc-mass-assignment-vulnerability"></a><a id="binding-mvc"></a>MVC toplu atama güvenlik açığını önlemek için ayrı model bağlama sınıfları veya bağlama filtresi listeleri kullanın

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL Fazı**               | Yapı |  
| **Uygulanabilir Teknolojiler** | MVC5, MVC6 |
| **Öznitelikler**              | Yok  |
| **Başvurular**              | [Meta veri öznitelikleri](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.metadatatypeattribute), [Kamu Anahtarı Güvenlik Güvenlik Açığı Ve Azaltma](https://github.com/blog/1068-public-key-security-vulnerability-and-mitigation), ASP.NET [MVC Kitle Atama komple Kılavuzu](https://odetocode.com/Blogs/scott/archive/2012/03/11/complete-guide-to-mass-assignment-in-asp-net-mvc.aspx), [MVC kullanarak EF ile başlarken](https://www.asp.net/mvc/tutorials/getting-started-with-ef-using-mvc/implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application#overpost) |
| **Adımlar** | <ul><li>**Aşırı deftere nakil güvenlik açıklarına ne zaman bakmalıyım?** Aşırı deftere nakil güvenlik açıkları, model sınıflarını kullanıcı girişine bağladiğiniz her yerde oluşabilir. MVC gibi çerçeveler, Kullanıcı verilerini Düz Eski CLR Nesneleri (POCOs) dahil olmak üzere özel .NET sınıflarında temsil edebilir. MVC, kullanıcı girişiyle başa çıkmak için kullanışlı bir gösterim sağlayarak bu model sınıflarını istekten gelen verilerle otomatik olarak doldurarak doldurur. Bu sınıflar kullanıcı tarafından ayarlanmaması gereken özellikler içerdiğinde, uygulama, uygulamanın hiçbir zaman amaçlanmadığının verilerin kullanıcı denetimine izin veren aşırı deftere nakil saldırılarına karşı savunmasız olabilir. MVC model bağlama gibi, Entity Framework gibi nesne/ilişkisel haritalayıcılar gibi veritabanı erişim teknolojileri de genellikle veritabanı verilerini temsil etmek için POCO nesnelerinin kullanılmasını destekler. Bu veri modeli sınıfları, MVC'nin kullanıcı girişiyle ilgili olarak yaptığı gibi veritabanı verileriyle başa çıkmada da aynı kolaylığı sağlar. Hem MVC hem de veritabanı POCO nesneleri gibi benzer modelleri desteklediğinden, aynı sınıfları her iki amaç için de yeniden kullanmak kolay görünüyor. Bu uygulama, kaygıların ayrılmasını korumak için başarısız olur ve istenmeyen özelliklerin model bağlamaya maruz kaldığı ve aşırı deftere nakil saldırıları sağlayan ortak bir alandır.</li><li>**Filtreuygulanmamış veritabanı modeli sınıflarımı neden MVC eylemlerimiçin parametre olarak kullanmamalıyım?** Çünkü MVC model bağlama o sınıftaki her şeyi bağlar. Veriler görünümünüzde görünmese bile, kötü niyetli bir kullanıcı bu veriler dahil edilmiş bir HTTP isteği gönderebilir ve eyleminiz veritabanı sınıfının kullanıcı girişi için kabul etmesi gereken verilerin şekli olduğunu söylediği için MVC bu isteği memnuniyetle bağlar.</li><li>**Model bağlama için kullanılan şekli neden önemsemeliyim?** Aşırı geniş modeller ile ASP.NET MVC modeli bağlama kullanarak aşırı gönderme saldırılarına bir uygulama ortaya çıkarır. Aşırı deftere nakil, saldırganların uygulama verilerini geliştiricinin amaçladığı nın (örneğin, bir öğenin fiyatını veya bir hesabın güvenlik ayrıcalıklarını geçersiz kılmak gibi) değiştirmelerine olanak sağlayabilir. Uygulamalar, model bağlama yoluyla izin verecek güvenilmeyen girdi için açık bir sözleşme sağlamak için eyleme özgü bağlama modellerini (veya belirli izin verilen özellik filtresi listelerini) kullanmalıdır.</li><li>**Ayrı bağlama modelleri olması sadece kodu çoğaltma mı?** Hayır, bu endişelerin ayrılması meselesi. Veritabanı modellerini eylem yöntemlerinde yeniden kullanırsanız, bu sınıftaki herhangi bir özelliğin (veya alt özelliğin) kullanıcı tarafından http isteğinde ayarlanabileceğini söylüyorsunuz. MVC'nin yapmasını istediğiniz bu değilse, MVC'nin kullanıcı girişinden hangi verilerin gelebileceğini göstermek için bir filtre listesine veya ayrı bir sınıf şekline ihtiyacınız olur.</li><li>**Kullanıcı girişi için ayrı bağlama modellerim varsa, tüm veri ek açıklama özniteliklerimi çoğaltmam gerekir mi?** Mutlaka. Bir model bağlama sınıfında meta verilere bağlanmak için veritabanı modeli sınıfında MetadataTypeAttribute kullanabilirsiniz. MetadataTypeAttribute tarafından başvurulan tür başvuru türünün bir alt kümesi olması gerektiğini unutmayın (daha az özelliklere sahip olabilir, ancak daha fazla değil).</li><li>**Verileri kullanıcı giriş modelleri ve veritabanı modelleri arasında ileri geri taşımak can sıkıcıdır. Yansımayı kullanarak tüm özellikleri kopyalayabilir miyim? -** Evet. Bağlama modellerinde görünen tek özellik, kullanıcı girişi için güvenli olduğunu belirlediğiniz özelliklerdir. Bu iki model arasında ortak olan tüm özellikleri kopyalamak için yansımayı kullanmayı engelleyen hiçbir güvenlik nedeni yoktur.</li><li>**Ne [Bind(Exclude ="â&euro;|")]. Bunu ayrı bağlama modelleri yerine kullanabilir miyim? -** Bu yaklaşım önerilmez. [Bind(Exclude ="â&euro;|")] kullanmak, herhangi bir yeni özelliğin varsayılan olarak bağlanabilir olduğu anlamına gelir. Yeni bir özellik eklendiğinde, tasarımın varsayılan olarak güvenli olmasını sağlamak yerine, şeyleri güvende tutmayı unutmamak için ek bir adım vardır. Geliştiricinin bu listeyi her özellik ekleninde denetlemesi risklidir.</li><li>**[Bind(Include ="â&euro;|")] Edit işlemleri için yararlı mıdır? -** №. [Bind(Include ="â&euro;|")] yalnızca INSERT tarzı işlemler (yeni veri ekleme) için uygundur. UPDATE tarzı işlemler (varolan verileri gözden geçirme) için, ayrı bağlama modellerine sahip olmak veya UpdateModel veya TryUpdateModel'e izin verilen özelliklerin açık bir listesini geçmek gibi başka bir yaklaşım kullanın. Bir Edit işlemine [Bind(Include ="â&euro;()] özniteliği eklemek, MVC'nin bir nesne örneği oluşturacağı ve yalnızca listelenen özellikleri ayarlayıp diğerlerini varsayılan değerlerinde bırakacağı anlamına gelir. Veriler kalıcı olduğunda, varsayılanlarına atlanan özellikler için değerleri sıfırlayarak, varolan varlığın tamamen yerini alır. Örneğin, Bir Düzenleme işleminde İşAdmin bir [Bind(Include&euro;="â |")] özniteliği atlanırsa, bu eylem aracılığıyla adı düzenlenen herhangi bir kullanıcı IsAdmin = false 'a sıfırlanır (düzenlenen herhangi bir kullanıcı yönetici durumunu kaybeder). Belirli özelliklerin güncelleştirmelerini engellemek istiyorsanız, yukarıdaki diğer yaklaşımlardan birini kullanın. MVC aracının bazı sürümlerinin Düzenleme eylemlerinde [Bind(Include&euro;="â ()] içeren denetleyici sınıfları oluşturduğunu ve bir özelliğin listeden kaldırılmasının aşırı deftere nakil saldırılarını önleyeceğini ima ettiğini unutmayın. Ancak, yukarıda açıklandığı gibi, bu yaklaşım beklendiği gibi çalışmaz ve bunun yerine atlanan özelliklerdeki tüm verileri varsayılan değerlerine sıfırlar.</li><li>**Oluşturma işlemleri için, ayrı bağlama modelleri yerine [Bind(Include ="â&euro;|")] kullanan herhangi bir uyarı var mı?** Evet. İlk olarak, bu yaklaşım, tüm aşırı deftere nakil güvenlik açıklarını hafifletmek için iki ayrı yaklaşım gerektiren senaryoları edit için çalışmaz. İkinci olarak, ayrı bağlama modelleri, kullanıcı girişi için kullanılan şekil ile kalıcılık için kullanılan şekil arasındaki&euro;endişelerin ayrılmasını zorunlu kılmaktadır, [Bind(Include ="â (")] bir şey yapmaz. Üçüncü olarak, [Bind(Include ="â&euro;|")] yalnızca üst düzey özellikleri işleyebilir unutmayın; öznitelikte yalnızca alt özelliklerin bölümlerine ("Details.Name" gibi) izin veremezsiniz. Son olarak, ve belki de en önemlisi, [Bind(Include ="â&euro;|")] kullanarak sınıf model bağlama için kullanılan her zaman hatırlanması gereken ekstra bir adım ekler. Yeni bir eylem yöntemi doğrudan veri sınıfına bağlanır sa ve [Bind(Include&euro;="â ()] özniteliği eklemeyi unutursa, aşırı deftere nakil&euro;saldırılarına karşı savunmasız olabilir, bu nedenle [Bind(Include ="â ()] yaklaşımı varsayılan olarak biraz daha az güvenlidir. [Bind(Include ="â&euro;|")]' kullanıyorsanız, veri sınıflarınız eylem yöntemi parametreleri olarak her göründüğünde bunu belirtmeyi her zaman unutmayın.</li><li>**Oluşturma işlemleri için, [Bind(Include ="â&euro;|"] özniteliğini model sınıfının kendisine koymaya ne dersin? Bu yaklaşım, her eylem yöntemine öznitelik koymayı hatırlama ihtiyacını önlemiyor mu? -** Bu yaklaşım bazı durumlarda çalışır. Model türünde [Bind(Include ="â&euro;(")] kullanmak (bu sınıfı kullanarak eylem parametreleri yerine), her eylem yöntemine [Bind(Include ="â&euro;(")] özniteliğini eklemeyi unutmamak gerekir. Özniteliği doğrudan sınıf üzerinde kullanmak, model bağlama amacıyla bu sınıfın ayrı bir yüzey alanını etkin bir şekilde oluşturur. Ancak, bu yaklaşım yalnızca model sınıfı başına bir model bağlama şekli sağlar. Bir eylem yönteminin bir alanın model bağlamasına (örneğin, yalnızca kullanıcı rollerini güncelleyen bir yönetici eylemi) ve bu alanın model bağlamasını önlemek için gereken diğer eylemlere izin vermek gerekiyorsa, bu yaklaşım çalışmaz. Her sınıfın yalnızca bir model bağlama şekli olabilir; farklı eylemler farklı model bağlama şekilleri gerekiyorsa, bu ayrı şekilleri ayrı model bağlama sınıfları veya&euro;ayrı [Bind(Include ="â |")] öznitelikleri eylem yöntemleri kullanarak temsil etmek gerekir.</li><li>**Bağlayıcı modeller nelerdir? Onlar görünüm modelleri ile aynı şey mi? -** Bunlar iki ilgili kavramdır. Bağlama terimi, bir eylemin parametre listesinde kullanılan bir model sınıfına (MVC modelinden eylem yöntemine bağlanan şekil) anlamına gelir. Görünüm terimi modeli, eylem yönteminden görünüme geçen bir model sınıfı anlamına gelir. Görünüme özgü bir model kullanmak, eylem yönteminden görünüme veri aktarmak için yaygın bir yaklaşımdır. Genellikle, bu şekil model bağlama için de uygundur ve terim görünümü modeli her iki yerde de kullanılan aynı modeli başvurmak için kullanılabilir. Kesin olarak, bu yordam özellikle bağlayıcı modeller hakkında, toplu atama amaçları için önemli olan eylem, geçirilen şekil odaklanarak konuşuyor.</li></ul>| 

## <a name="encode-untrusted-web-output-prior-to-rendering"></a><a id="rendering"></a>Oluşturmadan önce güvenilmeyen web çıktısını kodlama

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL Fazı**               | Yapı |  
| **Uygulanabilir Teknolojiler** | Genel, Web Formları, MVC5, MVC6 |
| **Öznitelikler**              | Yok  |
| **Başvurular**              | [Nasıl ASP.NET, Cross-site](https://msdn.microsoft.com/library/ms998274.aspx) [Scripting,](https://cwe.mitre.org/data/definitions/79.html) [XSS (Cross Site Scripting) Önleme Hile Sayfası](https://cheatsheetseries.owasp.org/cheatsheets/Cross_Site_Scripting_Prevention_Cheat_Sheet.html) çapraz site scripting önlemek için |
| **Adımlar** | Site arası komut dosyası (genellikle XSS olarak kısaltılır), çevrimiçi hizmetler veya web'den giriş tüketen herhangi bir uygulama/bileşen için bir saldırı vektörüdür. XSS güvenlik açıkları, saldırganın güvenlik açığı olan bir web uygulaması aracılığıyla başka bir kullanıcının makinesinde komut dosyası yürütmesine izin verebilir. Kötü amaçlı komut dosyaları çerezleri çalmak ve javascript aracılığıyla bir kurbanın makinesini kurcalamak için kullanılabilir. XSS, kullanıcı girişinin doğrulanması, iyi biçimlendirilmiş olması ve bir web sayfasında işlenmeden önce kodlanması ile engellenir. Giriş doğrulama ve çıktı kodlaması Web Koruma Kitaplığı kullanılarak yapılabilir. Yönetilen kod (C\#, VB.NET, vb.) için, kullanıcı girişinin ortaya çıktığı içeriğe bağlı olarak Web Koruma (Anti-XSS) Kitaplığı'ndan bir veya daha fazla uygun kodlama yöntemi kullanın:| 

### <a name="example"></a>Örnek

```csharp
* Encoder.HtmlEncode 
* Encoder.HtmlAttributeEncode 
* Encoder.JavaScriptEncode 
* Encoder.UrlEncode
* Encoder.VisualBasicScriptEncode 
* Encoder.XmlEncode 
* Encoder.XmlAttributeEncode 
* Encoder.CssEncode 
* Encoder.LdapEncode 
```

## <a name="perform-input-validation-and-filtering-on-all-string-type-model-properties"></a><a id="typemodel"></a>Tüm dize türü Model özelliklerinde giriş doğrulama ve filtreleme gerçekleştirin

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL Fazı**               | Yapı |  
| **Uygulanabilir Teknolojiler** | Genel, MVC5, MVC6 |
| **Öznitelikler**              | Yok  |
| **Başvurular**              | Doğrulama [Ekleme](https://www.asp.net/mvc/overview/getting-started/introduction/adding-validation), [MVC Uygulamasında Model Verilerini Doğrulama](https://msdn.microsoft.com/library/dd410404(v=vs.90).aspx), ASP.NET [MVC Uygulamalarınız için Kılavuz İlkeler](https://msdn.microsoft.com/magazine/dd942822.aspx) |
| **Adımlar** | <p>Uygulamanın kötü amaçlı kullanıcı girişlerine karşı korunup koruma altına alınmasını sağlamak için uygulamada kullanılmadan önce tüm giriş parametreleri doğrulanmalıdır. Giriş değerlerini, sunucu tarafında ki normal ifade doğrulamalarını kullanarak bir beyaz liste doğrulama stratejisiyle doğrulayın. Sanitize edilmemiş kullanıcı girişleri / parametreleri yöntemlere geçirilen kod enjeksiyon uyamıyorum.</p><p>Web uygulamaları için giriş noktaları form alanlarını, QueryStrings'i, tanımlama bilgilerini, HTTP üstbilgilerini ve web hizmeti parametrelerini de içerebilir.</p><p>Aşağıdaki giriş doğrulama denetimleri model bağlama üzerine yapılmalıdır:</p><ul><li>İzin verilen karakterleri ve izin verilen maksimum uzunluğu kabul etmek için model özellikleri Düzenli İfade ek açıklamaile ek açıklamalı olmalıdır</li><li>Denetleyici yöntemleri ModelState geçerliliğini gerçekleştirmelidir</li></ul>|

## <a name="sanitization-should-be-applied-on-form-fields-that-accept-all-characters-eg-rich-text-editor"></a><a id="richtext"></a>Sanitizasyon, zengin metin düzenleyicisi gibi tüm karakterleri kabul eden form alanlarına uygulanmalıdır

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL Fazı**               | Yapı |  
| **Uygulanabilir Teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvurular**              | [Güvenli Olmayan Girişi Kodla](https://msdn.microsoft.com/library/ff647397.aspx#paght000003_step3), [HTML Sanitizer](https://github.com/mganss/HtmlSanitizer) |
| **Adımlar** | <p>Kullanmak istediğiniz tüm statik biçimlendirme etiketlerini tanımlayın. Yaygın bir uygulama (kalın) ve `<b>` `<i>` (italik) gibi güvenli HTML öğeleri, biçimlendirme kısıtlamaktır.</p><p>Verileri yazmadan önce HTML kodlayın. Bu, kötü amaçlı komut dosyasını yürütülebilir kod olarak değil, metin olarak işlenebilir hale vererek güvenli hale getirir.</p><ol><li>Sayfa yönergesine ValidateRequest="false" özniteliği ekleyerek ASP.NET isteği \@ doğrulamasını devre dışı bırakma</li><li>HtmlEncode yöntemi ile dize girişini kodlama</li><li>Bir StringBuilder kullanın ve izin vermek istediğiniz HTML öğeleriüzerindeki kodlamayı seçikal olarak kaldırmak için değiştir yöntemini arayın</li></ol><p>Başvurulardaki sayfa, ASP.NET ayarlayarak `ValidateRequest="false"`doğrulama isteğini devre dışı bırakmaz. Bu HTML-giriş kodlar ve seçici `<b>` sağlar `<i>` ve Alternatif olarak, HTML sanitization için bir .NET kitaplığı da kullanılabilir.</p><p>HtmlSanitizer, XSS saldırılarına yol açabilecek yapılardaki HTML parçalarını ve belgelerini temizlemek için kullanılan bir .NET kitaplığıdır. HTML ve CSS'yi ayrışdırmak, işlemek ve işlemek için AngleSharp'ı kullanır. HtmlSanitizer bir NuGet paketi olarak yüklenebilir ve kullanıcı girişi sunucu tarafında, uygulanabilir olduğu gibi, ilgili HTML veya CSS sanitization yöntemleri ile geçirilebilir. Güvenlik denetimi olarak Sanitization'ın yalnızca son seçenek olarak düşünülmesi gerektiğini lütfen unutmayın.</p><p>Giriş doğrulama ve Çıktı Kodlaması daha iyi güvenlik denetimleri olarak kabul edilir.</p> |

## <a name="do-not-assign-dom-elements-to-sinks-that-do-not-have-inbuilt-encoding"></a><a id="inbuilt-encode"></a>Dahili kodlaması olmayan lavabolara DOM öğeleri atamayın

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL Fazı**               | Yapı |  
| **Uygulanabilir Teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvurular**              | Yok  |
| **Adımlar** | Birçok javascript işlevi varsayılan olarak kodlama yapmaz. Bu işlevler aracılığıyla DOM öğelerine güvenilmeyen giriş atadığında, site ler arası komut dosyası (XSS) yürütmelerine neden olabilir.| 

### <a name="example"></a>Örnek
Aşağıdaki güvensiz örnekler şunlardır: 

```
document.getElementByID("div1").innerHtml = value;
$("#userName").html(res.Name);
return $('<div/>').html(value)
$('body').append(resHTML);   
```
`innerHtml`Kullanmayın; bunun `innerText`yerine kullanın. Benzer şekilde, `$("#elm").html()`yerine , kullanmak`$("#elm").text()` 

## <a name="validate-all-redirects-within-the-application-are-closed-or-done-safely"></a><a id="redirect-safe"></a>Uygulama içindeki tüm yönlendirmelerin kapatılabileceğini veya güvenli bir şekilde yapıldığını doğrulayın

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL Fazı**               | Yapı |  
| **Uygulanabilir Teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvurular**              | [OAuth 2.0 Yetkilendirme Çerçevesi - Açık Yeniden Yönetmenler](https://tools.ietf.org/html/rfc6749#section-10.15) |
| **Adımlar** | <p>Kullanıcı tarafından sağlanan bir konuma yeniden yönlendirme gerektiren uygulama tasarımı, olası yeniden yönlendirme hedeflerini önceden tanımlanmış bir "güvenli" site veya etki alanı listesiyle sınırlandırmalıdır. Uygulamadaki tüm yönlendirmeler kapalı/güvenli olmalıdır.</p><p>Bunu yapmak için:</p><ul><li>Tüm yönlendirmeleri tanımlama</li><li>Her yönlendirme için uygun bir azaltma uygulayın. Uygun azaltıcı etkenler, yeniden yönlendirme beyaz listesini veya kullanıcı onayı içerir. Açık yönlendirme güvenlik açığı olan bir web sitesi veya hizmet Facebook/OAuth/OpenID kimlik sağlayıcıları kullanıyorsa, saldırgan bir kullanıcının oturum açma belirteci çalabilir ve bu kullanıcının kimliğine bürünebilir. Bu, RFC 6749 "The OAuth 2.0 Authorization Framework", Bölüm 10.15 "Açık Yönlendirmeler" ile belgelenen OAuth'u kullanırken doğal bir risktir, benzer şekilde, kullanıcıların kimlik bilgileri açık yönlendirmeler kullanarak sızdırma kimlik bilgileri tarafından tehlikeye atılabilir</li></ul>|

## <a name="implement-input-validation-on-all-string-type-parameters-accepted-by-controller-methods"></a><a id="string-method"></a>Denetleyici yöntemleri tarafından kabul edilen tüm dize türü parametrelerine giriş doğrulaması uygulayın

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL Fazı**               | Yapı |  
| **Uygulanabilir Teknolojiler** | Genel, MVC5, MVC6 |
| **Öznitelikler**              | Yok  |
| **Başvurular**              | [MVC Uygulamasında Model Verilerinin Doğrulanması](https://msdn.microsoft.com/library/dd410404(v=vs.90).aspx), [ASP.NET MVC Uygulamalarınız için Kılavuz İlkeler](https://msdn.microsoft.com/magazine/dd942822.aspx) |
| **Adımlar** | Bağımsız değişken olarak değil, yalnızca ilkel veri türünü kabul eden yöntemler için, Düzenli İfade kullanarak giriş doğrulaması yapılmalıdır. Burada Regex.IsMatch geçerli bir regex desen ile kullanılmalıdır. Giriş belirtilen Düzenli İfade ile eşleşmiyorsa, denetim daha fazla ilerlememeli ve doğrulama hatasıyla ilgili yeterli bir uyarı görüntülenmelidir.| 

## <a name="set-upper-limit-timeout-for-regular-expression-processing-to-prevent-dos-due-to-bad-regular-expressions"></a><a id="dos-expression"></a>Kötü normal ifadeler nedeniyle DoS'u önlemek için normal ifade işleme için üst sınır zaman anına ayarlayın

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL Fazı**               | Yapı |  
| **Uygulanabilir Teknolojiler** | Genel, Web Formları, MVC5, MVC6  |
| **Öznitelikler**              | Yok  |
| **Başvurular**              | [VarsayılanRegexMatchTimeout Özelliği](https://msdn.microsoft.com/library/system.web.configuration.httpruntimesection.defaultregexmatchtimeout.aspx) |
| **Adımlar** | Çok fazla geri izleme ye neden olan kötü oluşturulmuş düzenli ifadelere karşı hizmet reddi saldırılarını sağlamak için, genel varsayılan zaman anına ayarlayın. İşlem süresi tanımlanan üst sınırdan daha uzun sürerse, bir Zaman Aşımı özel durumu oluşturur. Hiçbir şey yapılandırılmamışsa, zaman ası sonsuz olacaktır.| 

### <a name="example"></a>Örnek
Örneğin, işlem 5 saniyeden fazla sürerse, aşağıdaki yapılandırma bir RegexMatchTimeoutException atar: 

```csharp
<httpRuntime targetFramework="4.5" defaultRegexMatchTimeout="00:00:05" />
```

## <a name="avoid-using-htmlraw-in-razor-views"></a><a id="html-razor"></a>Html.Raw'ı Razor görünümlerinde kullanmaktan kaçının

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL Fazı**               | Yapı |  
| **Uygulanabilir Teknolojiler** | MVC5, MVC6 |
| **Öznitelikler**              | Yok  |
| **Başvurular**              | Yok  |
| Adım | ASP.NET Web Sayfaları (Razor) otomatik HTML kodlaması gerçekleştirir. Gömülü kod külçeleri (@ bloklar) tarafından yazdırılan tüm dizeleri otomatik olarak HTML kodlanır. Ancak, `HtmlHelper.Raw` Yöntem çağrıldığı zaman, HTML kodlanmış olmayan biçimlendirme döndürür. `Html.Raw()` Yardımcı yöntem kullanılırsa, Razor'ın sağladığı otomatik kodlama korumasını atlar.|

### <a name="example"></a>Örnek
Aşağıdaki güvensiz bir örnektir: 

```csharp
<div class="form-group">
            @Html.Raw(Model.AccountConfirmText)
        </div>
        <div class="form-group">
            @Html.Raw(Model.PaymentConfirmText)
        </div>
</div>
```
Biçimlendirme `Html.Raw()` görüntülemeniz gerekmedikçe kullanmayın. Bu yöntem, çıktı kodlamasını örtülü olarak gerçekleştirmez. Diğer ASP.NET yardımcılarını kullanın, örneğin,`@Html.DisplayFor()` 

## <a name="do-not-use-dynamic-queries-in-stored-procedures"></a><a id="stored-proc"></a>Depolanan yordamlarda dinamik sorgular kullanmayın

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Veritabanı | 
| **SDL Fazı**               | Yapı |  
| **Uygulanabilir Teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvurular**              | Yok  |
| **Adımlar** | <p>SQL enjeksiyon saldırısı, veritabanında rasgele komutları çalıştırmak için giriş doğrulamagüvenlikaçıklarından yararlanır. Uygulamanız veritabanına erişmek için dinamik SQL deyimleri oluşturmak için giriş kullandığında oluşabilir. Kodunuz ham kullanıcı girişi içeren dizeleri geçirilen saklı yordamlar kullanırsa da oluşabilir. SQL enjeksiyon saldırısını kullanarak, saldırgan veritabanında rasgele komutları çalıştırabilirsiniz. Tüm SQL deyimleri (depolanan yordamlarda SQL deyimleri dahil) parametreli olmalıdır. Parametreli SQL deyimleri, güçlü bir şekilde yazıldığı için SQL'e özel bir anlamı olan karakterleri (tek teklif gibi) sorunsuz kabul eder. |

### <a name="example"></a>Örnek
Aşağıdaki güvensiz dinamik Saklı Yordam bir örnektir: 

```csharp
CREATE PROCEDURE [dbo].[uspGetProductsByCriteria]
(
  @productName nvarchar(200) = NULL,
  @startPrice float = NULL,
  @endPrice float = NULL
)
AS
 BEGIN
  DECLARE @sql nvarchar(max)
  SELECT @sql = ' SELECT ProductID, ProductName, Description, UnitPrice, ImagePath' +
       ' FROM dbo.Products WHERE 1 = 1 '
       PRINT @sql
  IF @productName IS NOT NULL
     SELECT @sql = @sql + ' AND ProductName LIKE ''%' + @productName + '%'''
  IF @startPrice IS NOT NULL
     SELECT @sql = @sql + ' AND UnitPrice > ''' + CONVERT(VARCHAR(10),@startPrice) + ''''
  IF @endPrice IS NOT NULL
     SELECT @sql = @sql + ' AND UnitPrice < ''' + CONVERT(VARCHAR(10),@endPrice) + ''''

  PRINT @sql
  EXEC(@sql)
 END
```

### <a name="example"></a>Örnek
Güvenli bir şekilde uygulanan aynı depolanmış yordam aşağıdakigibidir: 
```csharp
CREATE PROCEDURE [dbo].[uspGetProductsByCriteriaSecure]
(
             @productName nvarchar(200) = NULL,
             @startPrice float = NULL,
             @endPrice float = NULL
)
AS
       BEGIN
             SELECT ProductID, ProductName, Description, UnitPrice, ImagePath
             FROM dbo.Products where
             (@productName IS NULL or ProductName like '%'+ @productName +'%')
             AND
             (@startPrice IS NULL or UnitPrice > @startPrice)
             AND
             (@endPrice IS NULL or UnitPrice < @endPrice)         
       END
```

## <a name="ensure-that-model-validation-is-done-on-web-api-methods"></a><a id="validation-api"></a>Web API yöntemlerinde model doğrulamanın yapıldığından emin olun

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web API | 
| **SDL Fazı**               | Yapı |  
| **Uygulanabilir Teknolojiler** | MVC5, MVC6 |
| **Öznitelikler**              | Yok  |
| **Başvurular**              | [ASP.NET Web API'sinde Model Doğrulama](https://www.asp.net/web-api/overview/formats-and-model-binding/model-validation-in-aspnet-web-api) |
| **Adımlar** | İstemci bir web API'sine veri gönderdiğinde, herhangi bir işleme yapmadan önce verileri doğrulamak zorunludur. Modelleri giriş olarak kabul eden ASP.NET Web API'leri için, modelin özellikleri üzerinde doğrulama kuralları ayarlamak için modellerdeki veri ek açıklamalarını kullanın.|

### <a name="example"></a>Örnek
Aşağıdaki kod aynı şeyi gösterir: 

```csharp
using System.ComponentModel.DataAnnotations;

namespace MyApi.Models
{
    public class Product
    {
        public int Id { get; set; }
        [Required]
        [RegularExpression(@"^[a-zA-Z0-9]*$", ErrorMessage="Only alphanumeric characters are allowed.")]
        public string Name { get; set; }
        public decimal Price { get; set; }
        [Range(0, 999)]
        public double Weight { get; set; }
    }
}
```

### <a name="example"></a>Örnek
API denetleyicilerinin eylem yönteminde, modelin geçerliliği aşağıda gösterildiği gibi açıkça kontrol edilmelidir: 

```csharp
namespace MyApi.Controllers
{
    public class ProductsController : ApiController
    {
        public HttpResponseMessage Post(Product product)
        {
            if (ModelState.IsValid)
            {
                // Do something with the product (not shown).

                return new HttpResponseMessage(HttpStatusCode.OK);
            }
            else
            {
                return Request.CreateErrorResponse(HttpStatusCode.BadRequest, ModelState);
            }
        }
    }
}
```

## <a name="implement-input-validation-on-all-string-type-parameters-accepted-by-web-api-methods"></a><a id="string-api"></a>Web API yöntemleri tarafından kabul edilen tüm dize türü parametrelerine giriş doğrulaması uygulama

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web API | 
| **SDL Fazı**               | Yapı |  
| **Uygulanabilir Teknolojiler** | Genel, MVC 5, MVC 6 |
| **Öznitelikler**              | Yok  |
| **Başvurular**              | [MVC Uygulamasında Model Verilerinin Doğrulanması](https://msdn.microsoft.com/library/dd410404(v=vs.90).aspx), [ASP.NET MVC Uygulamalarınız için Kılavuz İlkeler](https://msdn.microsoft.com/magazine/dd942822.aspx) |
| **Adımlar** | Bağımsız değişken olarak değil, yalnızca ilkel veri türünü kabul eden yöntemler için, Düzenli İfade kullanarak giriş doğrulaması yapılmalıdır. Burada Regex.IsMatch geçerli bir regex desen ile kullanılmalıdır. Giriş belirtilen Düzenli İfade ile eşleşmiyorsa, denetim daha fazla ilerlememeli ve doğrulama hatasıyla ilgili yeterli bir uyarı görüntülenmelidir.|

## <a name="ensure-that-type-safe-parameters-are-used-in-web-api-for-data-access"></a><a id="typesafe-api"></a>Veri erişimi için Web API'sinde tür güvenliği parametrelerinin kullanıldığından emin olun

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web API | 
| **SDL Fazı**               | Yapı |  
| **Uygulanabilir Teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvurular**              | Yok  |
| **Adımlar** | <p>Parametreler koleksiyonunu kullanırsanız, SQL girişi gerçek bir değer olarak değil, yürütülebilir kod olarak ele alır. Parametreler toplama giriş verilerinde tür ve uzunluk kısıtlamaları zorlamak için kullanılabilir. Aralığın dışındaki değerler bir özel durum tetikler. Tür açısından güvenli SQL parametreleri kullanılmazsa, saldırganlar filtreuygulanmamış girişe katıştırılmış enjeksiyon saldırılarını gerçekleştirebilir.</p><p>Filtreuygulanmamış girişle oluşabilecek olası SQL enjeksiyon saldırılarından kaçınmak için SQL sorguları yaparken tür güvenli parametrelerini kullanın. Depolanan yordamlarla ve dinamik SQL deyimleriyle güvenli parametreler ilerler. Parametreler, yürütülebilir kod olarak değil, veritabanı tarafından gerçek değerler olarak kabul edilir. Parametreler de türü ve uzunluğu için denetlenir.</p>|

### <a name="example"></a>Örnek
Aşağıdaki kod, depolanan bir yordamı ararken SqlParameterCollection ile tür güvenli parametrelerinin nasıl kullanılacağını gösterir. 

```csharp
using System.Data;
using System.Data.SqlClient;

using (SqlConnection connection = new SqlConnection(connectionString))
{ 
DataSet userDataset = new DataSet(); 
SqlDataAdapter myCommand = new SqlDataAdapter("LoginStoredProcedure", connection); 
myCommand.SelectCommand.CommandType = CommandType.StoredProcedure; 
myCommand.SelectCommand.Parameters.Add("@au_id", SqlDbType.VarChar, 11); 
myCommand.SelectCommand.Parameters["@au_id"].Value = SSN.Text; 
myCommand.Fill(userDataset);
}  
```
Önceki kod örneğinde, giriş değeri 11 karakterden uzun olamaz. Veriler parametre tarafından tanımlanan tür veya uzunluğa uymuyorsa, SqlParameter sınıfı bir özel durum oluşturur. 

## <a name="use-parameterized-sql-queries-for-cosmos-db"></a><a id="sql-docdb"></a>Cosmos DB için parametreli SQL sorgularını kullanma

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Azure Belge DB | 
| **SDL Fazı**               | Yapı |  
| **Uygulanabilir Teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvurular**              | [Azure Cosmos DB'de SQL Parametreizasyonu Duyurusu](https://azure.microsoft.com/blog/announcing-sql-parameterization-in-documentdb/) |
| **Adımlar** | Azure Cosmos DB yalnızca salt okunur sorguları desteklese de, sorgular kullanıcı girişiyle katlanarak oluşturulursa SQL enjeksiyonu yine de mümkündür. Bir kullanıcının kötü amaçlı SQL sorguları oluşturarak aynı koleksiyonda erişmemesi gereken verilere erişmesi mümkün olabilir. Sorgular kullanıcı girişine göre oluşturulmuşsa parametreli SQL sorgularını kullanın. |

## <a name="wcf-input-validation-through-schema-binding"></a><a id="schema-binding"></a>Schema bağlama yoluyla WCF Giriş doğrulama

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | WCF | 
| **SDL Fazı**               | Yapı |  
| **Uygulanabilir Teknolojiler** | Genel, NET Framework 3 |
| **Öznitelikler**              | Yok  |
| **Başvurular**              | [MSDN](https://msdn.microsoft.com/library/ff647820.aspx) |
| **Adımlar** | <p>Doğrulama eksikliği farklı tip enjeksiyon saldırılarına yol açar.</p><p>İleti doğrulama, WCF uygulamanızın korunmasında bir savunma hattını temsil eder. Bu yaklaşımla, WCF hizmet işlemlerini kötü amaçlı bir istemcinin saldırısına karşı korumak için şemakullanarak iletileri doğrularsınız. İstemciyi kötü amaçlı bir hizmetin saldırısına karşı korumak için istemci tarafından alınan tüm iletileri doğrulayın. İleti doğrulama, işlemler parametre doğrulaması kullanılarak yapılamayacak ileti sözleşmelerini veya veri sözleşmelerini tüketirken iletilerin doğrulanmasını mümkün kılar. İleti doğrulama, şemalar içinde doğrulama mantığı oluşturmanıza olanak sağlayarak daha fazla esneklik sağlar ve geliştirme süresini azaltır. Şemalar, kuruluş içindeki farklı uygulamalar arasında yeniden kullanılabilir ve veri gösterimi için standartlar oluşturabilir. Ayrıca, ileti doğrulama, iş mantığını temsil eden sözleşmeleri içeren daha karmaşık veri türlerini tükettiklerinde işlemleri korumanıza olanak tanır.</p><p>İleti doğrulaması gerçekleştirmek için, önce hizmetinizin işlemlerini ve bu işlemler tarafından tüketilen veri türlerini temsil eden bir şema oluşturursunuz. Daha sonra, hizmete/hizmetten alınan iletileri doğrulamak için özel bir istemci ileti sn amacını ve özel sevk irsaliyesi iletisi denetçisini uygulayan bir .NET sınıfı oluşturursunuz. Ardından, hem istemci de hem de hizmette ileti doğrulaması etkinleştirmek için özel bir uç nokta davranışı uygularsınız. Son olarak, hizmetin veya istemcinin yapılandırma dosyasında genişletilmiş özel uç nokta davranışını ortaya çıkarmanızı sağlayan özel bir yapılandırma öğesi sınıfta uygularsınız."</p>|

## <a name="wcf--input-validation-through-parameter-inspectors"></a><a id="parameters"></a>Parametre Denetçileri aracılığıyla WCF- Giriş doğrulaması

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | WCF | 
| **SDL Fazı**               | Yapı |  
| **Uygulanabilir Teknolojiler** | Genel, NET Framework 3 |
| **Öznitelikler**              | Yok  |
| **Başvurular**              | [MSDN](https://msdn.microsoft.com/library/ff647875.aspx) |
| **Adımlar** | <p>Giriş ve veri doğrulama, WCF uygulamanızın korunmasında önemli bir savunma hattını temsil eder. Hizmeti kötü amaçlı bir istemcinin saldırısına karşı korumak için WCF hizmet operasyonlarında açığa çıkan tüm parametreleri doğrulamanız gerekir. Tersine, istemciyi kötü amaçlı bir hizmetin saldırısına karşı korumak için istemci tarafından alınan tüm iade değerlerini de doğrulamanız gerekir</p><p>WCF, özel uzantılar oluşturarak WCF çalışma zamanı davranışını özelleştirmenize olanak tanıyan farklı genişletilebilirlik noktaları sağlar. İleti Denetçileri ve Parametre Denetmenleri, istemci ve hizmet arasında geçen veriler üzerinde daha fazla denetim elde etmek için kullanılan iki genişletilebilirlik mekanizmasıdır. Giriş doğrulama için parametre denetmenleri kullanmalı ve ileti denetimlerini yalnızca hizmete girip çıkan tüm iletiyi denetlemeniz gerektiğinde kullanmalısınız.</p><p>Giriş doğrulaması gerçekleştirmek için, bir .NET sınıfı oluşturur ve hizmetinizdeki işlemlerdeki parametreleri doğrulamak için özel bir parametre denetçisi uygularsınız. Daha sonra hem istemci hem de hizmet üzerinde doğrulama sağlamak için özel bir bitiş noktası davranışı uygularsınız. Son olarak, hizmetin veya istemcinin yapılandırma dosyasında genişletilmiş özel uç nokta davranışını ortaya çıkarmanızı sağlayan özel bir yapılandırma öğesi uygularsınız</p>|
