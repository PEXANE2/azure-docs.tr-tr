---
title: Ses ve SMS için Twilio Nasıl Kullanılır (.NET) | Microsoft Dokümanlar
description: Azure'daki Twilio API hizmetiyle nasıl telefon görüşmesi yapacağınızı ve SMS mesajı göndermeyi öğrenin. .NET'te yazılan kod örnekleri.
services: ''
documentationcenter: .net
author: georgewallace
ms.assetid: 74d4f3c9-f1cb-4968-b744-36b32cd0e834
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 04/24/2015
ms.author: gwallace
ms.openlocfilehash: 22b33d7b4b0ff69a2e751cadff70453f73ed4f8e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "69876813"
---
# <a name="how-to-use-twilio-for-voice-and-sms-capabilities-from-azure"></a>Azure'dan ses ve SMS özellikleri için Twilio nasıl kullanılır?
Bu kılavuz, Azure'daki Twilio API hizmetiyle ortak programlama görevlerinin nasıl gerçekleştirildirilebildiğini gösterir. Kapsanan senaryolar arasında telefon görüşmesi yapmak ve Kısa Mesaj Hizmeti (SMS) mesajı göndermek yer almaktadır. Twilio hakkında daha fazla bilgi ve uygulamalarınızda ses ve SMS kullanmak için [Sonraki adımlar](#NextSteps) bölümüne bakın.

## <a name="what-is-twilio"></a><a id="WhatIs"></a>Twilio nedir?
Twilio, iş iletişiminin geleceğine güç vererek geliştiricilerin ses, VoIP ve mesajlaşmayı uygulamalara yerleştirmesini sağlıyor. Bulut tabanlı, küresel bir ortamda ihtiyaç duyulan tüm altyapıyı sanallaştırarak Twilio iletişim API platformu aracılığıyla ortaya çıkarırlar. Uygulamalar oluşturmak için basit ve ölçeklenebilir. Gittikçe öde fiyatlandırmasıyla esnekliğin keyfini çıkarın ve bulut güvenilirliğinden yararlanın.

**Twilio Voice,** uygulamalarınızın telefon görüşmesi yapmasına ve almasını sağlar. **Twilio SMS,** uygulamalarınızın SMS gönderip almasını sağlar. **Twilio Client** herhangi bir telefon, tablet veya tarayıcıdan VoIP aramaları yapmanızı sağlar ve WebRTC'yi destekler.

## <a name="twilio-pricing-and-special-offers"></a><a id="Pricing"></a>Twilio Fiyatlandırma ve Özel Teklifler
Azure müşterilerine [özel](https://www.twilio.com/azure)bir teklif alır: Twilio Hesabınızı yükselttiğinde ücretsiz $10 Twilio Kredisi. Bu Twilio Kredisi herhangi bir Twilio kullanımına uygulanabilir (telefon numaranızın ve mesajınızın veya arama hedefinizin konumuna bağlı olarak 1000'e kadar SMS mesajı göndermeye veya 1000'e kadar gelen Sesli dakika almaya eşdeğer 10$'lık kredi). Bu Twilio kredi nizi kullanın ve [twilio.com/azure'dan](https://twilio.com/azure)başlayın.

Twilio bir öde-as-you-go hizmetidir. Kurulum ücreti yoktur ve hesabınızı istediğiniz zaman kapatabilirsiniz. Daha fazla ayrıntıyı [Twilio Fiyatlandırma](https://www.twilio.com/voice/pricing)adresinde bulabilirsiniz.

## <a name="concepts"></a><a id="Concepts"></a>Kavramlar
Twilio API, uygulamalar için ses ve SMS işlevselliği sağlayan restful API'dir. İstemci kitaplıkları birden çok dilde kullanılabilir; bir liste [için, Bkz. Twilio API Kitaplıkları.][twilio_libraries]

Twilio API'nin temel yönleri Twilio fiilleri ve Twilio Markup Language (TwiML) 'dir.

### <a name="twilio-verbs"></a><a id="Verbs"></a>Twilio fiilleri
API, Twilio fiillerini kullanır; örneğin, ** &lt;Say&gt; ** fiili Twilio'ya bir çağrıda sesli bir ileti iletmesini söyler.

Aşağıda Twilio fiillerinin bir listesi vetifer.  [Twilio Markup Language belgeleri](https://www.twilio.com/docs/api/twiml)aracılığıyla diğer fiiller ve yetenekler hakkında bilgi edinin.

* `<Dial>`: Arayan ı başka bir telefona bağlar.
* `<Gather>`: Telefon tuş takımı üzerinde girilen sayısal rakamları toplar.
* `<Hangup>`: Aramayı bitirir.
* `<Play>`: Bir ses dosyası çalar.
* `<Pause>`: Belirli sayıda saniye sessizce bekler.
* `<Record>`: Arayanın sesini kaydeder ve kaydı içeren bir dosyanın URL'sini döndürür.
* `<Redirect>`: Bir arama veya SMS'in denetimini farklı bir URL'den TwiML'e aktarın.
* `<Reject>`: Twilio numaranıza gelen bir aramayı faturalandırmadan reddeder
* `<Say>`: Metni, aramada yapılan konuşmaya dönüştürür.
* `<Sms>`: SMS mesajı gönderir.

### <a name="twiml"></a>TwiML
TwiML, Twilio'ya bir çağrıveya SMS'in nasıl işlenirolduğunu bildiren Twilio fiillerine dayanan XML tabanlı talimatlar kümesidir.

Örnek olarak, aşağıdaki TwiML metni **Hello World'ü** konuşmaya dönüştürür.

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<Response>
  <Say>Hello World</Say>
</Response>
```

Uygulamanız Twilio API'yi aradığında, API parametrelerinden biri TwiML yanıtını döndüren URL'dir. Geliştirme amacıyla, uygulamalarınız tarafından kullanılan TwiML yanıtlarını sağlamak için Twilio tarafından sağlanan URL'leri kullanabilirsiniz. Ayrıca, TwiML yanıtlarını oluşturmak için kendi URL'lerinizi barındırabilirsiniz ve başka bir seçenek de **TwiMLResponse** nesnesini kullanmaktır.

Twilio fiilleri, öznitelikleri ve TwiML hakkında daha fazla bilgi için Bkz. [TwiML][twiml]. Twilio API hakkında daha fazla bilgi için [Twilio API'ye][twilio_api]bakın.

## <a name="create-a-twilio-account"></a><a id="CreateAccount"></a>Twilio Hesabı Oluşturma
Bir Twilio hesabı almaya hazır olduğunuzda, [Try Twilio'ya][try_twilio]kaydolun. Ücretsiz bir hesapla başlayabilir ve hesabınızı daha sonra yükseltebilirsiniz.

Bir Twilio hesabına kaydolduğunuzda, bir hesap kimliği ve kimlik doğrulama belirteci alırsınız. Her ikisi de Twilio API aramaları yapmak için gerekli olacaktır. Hesabınıza yetkisiz erişimi önlemek için kimlik doğrulama belirtecinizi güvende tutun. Hesap kimliğiniz ve kimlik doğrulama belirteciniz, sırasıyla **ACCOUNT SID** ve **AUTH TOKEN**etiketli alanlarda, [Twilio hesap sayfasında][twilio_account]görüntülenebilir.

## <a name="create-an-azure-application"></a><a id="create_app"></a>Azure Uygulaması Oluşturma
Twilio özellikli bir uygulamaya ev sahipliği yapan bir Azure uygulaması, diğer Azure uygulamalarından farklı değildir. Twilio .NET kitaplığını ekleyin ve rolü Twilio .NET kitaplıklarını kullanacak şekilde yapılandırın.
İlk Azure projesi oluşturma hakkında daha fazla bilgi için [Visual Studio ile Azure projesi oluşturma][vs_project]'ya bakın.

## <a name="configure-your-application-to-use-twilio-libraries"></a><a id="configure_app"></a>Twilio Kitaplıklarını Kullanacak Şekilde Uygulamanızı Yapılandırın
Twilio, TwiML yanıtları oluşturmak için Twilio REST API ve Twilio Client ile etkileşim kurmanın basit ve kolay yollarını sağlamak için Twilio'nun çeşitli yönlerini saran bir dizi .NET yardımcı kitaplığı sağlar.

Twilio .NET geliştiricileri için beş kitaplık sağlar:

| Kitaplık | Açıklama |
| --- | --- |
| Twilio.API | Twilio REST API'yi dost bir .NET kitaplığında saran çekirdek Twilio kütüphanesi. Bu kitaplık .NET, Silverlight ve Windows Phone 7 için kullanılabilir. |
| Twilio.TwiML | TwiML biçimlendirmesi oluşturmak için .NET dostu bir yol sağlar. |
| Twilio.MVC | MVC'ASP.NET kullanan geliştiriciler için bu kitaplık bir TwilioController, TwiML ActionResult ve istek doğrulama özniteliği içerir. |
| Twilio.WebMatrix | Microsoft'un ücretsiz WebMatrix geliştirme aracını kullanan geliştiriciler için bu kitaplık, çeşitli Twilio eylemleri için Jilet sözdizimi yardımcıları içerir. |
| Twilio.Client.Capability | Twilio Client JavaScript SDK ile kullanılmak üzere Yetenek belirteç jeneratörü içerir. |

> [!Important]
> Tüm kitaplıklar .NET 3.5, Silverlight 4 veya Windows Phone 7 veya daha sonra gerektirir.

Bu kılavuzda sağlanan örnekler Twilio.API kitaplığını kullanır.

Kütüphaneler, Visual Studio 2010 için 2015'e kadar mevcut [olan NuGet paket yöneticisi uzantısı kullanılarak yüklenebilir.](https://www.twilio.com/docs/csharp/install)  Kaynak kodu [GitHub][twilio_github_repo]barındırılan , kitaplıkları kullanmak için tam belgeler içeren bir Wiki içerir.

Varsayılan olarak, Microsoft Visual Studio 2010 NuGet sürümü 1.2 yükler. Twilio kitaplıklarının yüklenmesi için NuGet veya üstü sürüm 1.6'yı gerektirir. NuGet'i yükleme veya güncelleme [https://nuget.org/][nuget]hakkında bilgi için bkz.

> [!NOTE]
> NuGet'in en son sürümünü yüklemek için önce Visual Studio Extension Manager'ı kullanarak yüklenen sürümü kaldırmanız gerekir. Bunun için Visual Studio'u yönetici olarak çalıştırmanız gerekir. Aksi takdirde, Kaldır düğmesi devre dışı bırakılır.
>
>

### <a name="to-add-the-twilio-libraries-to-your-visual-studio-project"></a><a id="use_nuget"></a>Visual Studio projenize Twilio kitaplıklarını eklemek için:
1. Visual Studio'da çözümünüzü açın.
2. Sağ tıklatma **Başvuruları**.
3. **NuGet Paketlerini Yönet'e tıklayın...**
4. **Online'ı**tıklatın.
5. Arama çevrimiçi kutusunda, *twilio*yazın.
6. Twilio paketini **yükle'yi** tıklatın.

## <a name="how-to-make-an-outgoing-call"></a><a id="howto_make_call"></a>Nasıl yapılır: Giden arama yapma
Aşağıda, **CallResource** sınıfını kullanarak giden aramanın nasıl yapılacağını gösterilmektedir. Bu kod, Twilio İşaretdili Dili (TwiML) yanıtını döndürmek için Twilio tarafından sağlanan bir site de kullanır. Ve telefon numaraları **to** **için** değerlerinizi değiştirin ve kodu çalıştırmadan önce Twilio hesabınız için **telefon** numarasını doğruladığınızdan emin olun.

```csharp
// Use your account SID and authentication token instead
// of the placeholders shown here.
const string accountSID = "your_twilio_account";
const string authToken = "your_twilio_authentication_token";

// Initialize the TwilioClient.
TwilioClient.Init(accountSID, authToken);

// Use the Twilio-provided site for the TwiML response.
var url = "https://twimlets.com/message";
url = $"{url}?Message%5B0%5D=Hello%20World";

// Set the call From, To, and URL values to use for the call.
// This sample uses the sandbox number provided by
// Twilio to make the call.
var call = CallResource.Create(
    to: new PhoneNumber("+NNNNNNNNNN"),
    from: new PhoneNumber("NNNNNNNNNN"),
    url: new Uri(url));
    }
```

**CallResource.Create** yöntemine geçirilen parametreler hakkında daha fazla [https://www.twilio.com/docs/api/rest/making-calls][twilio_rest_making_calls]bilgi için bkz.

Belirtildiği gibi, bu kod TwiML yanıtını döndürmek için Twilio tarafından sağlanan bir site kullanır. Bunun yerine TwiML yanıtı sağlamak için kendi sitenizi kullanabilirsiniz. Daha fazla bilgi için [bkz: Kendi web sitenizden TwiML yanıtları sağlayın.](#howto_provide_twiml_responses)

## <a name="how-to-send-an-sms-message"></a><a id="howto_send_sms"></a>Nasıl yapılsın: SMS mesajı gönder
Aşağıdaki ekran görüntüsü, **MessageResource** sınıfını kullanarak SMS iletisinin nasıl gönderilebildiğini gösterir. **Numaradan sms** mesajları göndermek için deneme hesapları için Twilio tarafından sağlanmaktadır. Kodu çalıştırmadan önce Twilio hesabınız için **numara** doğrulanmalıdır.

```csharp
// Use your account SID and authentication token instead
// of the placeholders shown here.
const string accountSID = "your_twilio_account";
const string authToken = "your_twilio_authentication_token";

// Initialize the TwilioClient.
TwilioClient.Init(accountSID, authToken);

try
{
    // Send an SMS message.
    var message = MessageResource.Create(
        to: new PhoneNumber("+12069419717"),
        from: new PhoneNumber("+14155992671"),
        body: "This is my SMS message.");
}
catch (TwilioException ex)
{
    // An exception occurred making the REST call
    Console.WriteLine(ex.Message);
}
```

## <a name="how-to-provide-twiml-responses-from-your-own-website"></a><a id="howto_provide_twiml_responses"></a>Nasıl?
Başvurunuz Twilio API'ye bir çağrı başlattığında -örneğin, **CallResource.Create** yöntemi aracılığıyla - Twilio isteğinizi TwiML yanıtı döndürmesi beklenen bir URL'ye gönderir. [Nasıl Yapılır: Giden arama yapma](#howto_make_call) örneği, yanıtı döndürmek için [https://twimlets.com/message][twimlet_message_url] Twilio tarafından sağlanan URL'yi kullanır.

> [!NOTE]
> TwiML web hizmetleri tarafından kullanılmak üzere tasarlanmış olsa da, Tarayıcınızda TwiML görüntüleyebilirsiniz. Örneğin, boş [https://twimlets.com/message][twimlet_message_url] `<Response>` bir öğeyi görmek için tıklatın; başka bir örnek [https://twimlets.com/message?Message%5B0%5D=Hello%20World](https://twimlets.com/message?Message%5B0%5D=Hello%20World) olarak, `<Response>` bir &lt;Say&gt; öğesi içeren bir öğeyi görmek için tıklatın.
>

Twilio tarafından sağlanan URL'ye güvenmek yerine, HTTP yanıtlarını döndüren kendi URL sitenizi oluşturabilirsiniz. Siteyi HTTP yanıtlarını döndüren herhangi bir dilde oluşturabilirsiniz. Bu konu, URL'yi ASP.NET genel bir işleyiciden barındıracağınız varsayar.

Aşağıdaki ASP.NET İşleyici, çağrıda **Merhaba Dünya** yazan bir TwiML yanıtı hazırlatır.

```csharp
using System.Text;
using System.Web;

namespace WebRole1
{
    /// <summary>
    /// Summary description for Handler1
    /// </summary>
    public class Handler1 : IHttpHandler
    {
        public void ProcessRequest(HttpContext context)
        {
            const string twiMLResponse =
                "<Response><Say>Hello World.</Say></Response>";

            context.Response.Clear();
            context.Response.ContentType = "text/xml";
            context.Response.ContentEncoding = Encoding.UTF8;
            context.Response.Write(twiMLResponse);
            context.Response.End();
        }

        public bool IsReusable
        {
            get
            {
                return false;
            }
        }
    }
}
```
    
Yukarıdaki örnekten de görebileceğiniz gibi, TwiML yanıtı sadece bir XML belgesidir. Twilio.TwiML kitaplığı sizin için TwiML oluşturacak sınıflar içerir. Aşağıdaki örnek, yukarıda gösterildiği gibi eşdeğer yanıtı üretir, ancak **VoiceResponse** sınıfını kullanır.

```csharp
using System.Web;
using Twilio.TwiML;

namespace WebRole1
{
    /// <summary>
    /// Summary description for Handler1
    /// </summary>
    public class Handler1 : IHttpHandler
    {

        public void ProcessRequest(HttpContext context)
        {
            var twiml = new VoiceResponse();
            twiml.Say("Hello World.");

            context.Response.Clear();
            context.Response.ContentType = "text/xml";
            context.Response.Write(twiml.ToString());
            context.Response.End();
        }

        public bool IsReusable
        {
            get
            {
                return false;
            }
        }
    }
}
```

TwiML hakkında daha fazla [https://www.twilio.com/docs/api/twiml](https://www.twilio.com/docs/api/twiml)bilgi için bkz.

TwiML yanıtları sağlamanın bir yolunu ayarladıktan sonra, bu URL'yi **CallResource.Create** yöntemine geçirebilirsiniz. Örneğin, Bir Azure bulut hizmetine dağıtılan MyTwiML adlı bir web uygulamanız varsa ve ASP.NET İşleyicinizin adı mytwiml.ashx ise, URL aşağıdaki kod örneğinde gösterildiği gibi **CallResource.Create'e** geçirilebilir:

```csharp
// This sample uses the sandbox number provided by Twilio to make the call.
// Place the call.
var call = CallResource.Create(
    to: new PhoneNumber("+NNNNNNNNNN"),
    from: new PhoneNumber("NNNNNNNNNN"),
    url: new Uri("http://<your_hosted_service>.cloudapp.net/MyTwiML/mytwiml.ashx"));
    }
```

Twilio'nun Azure'da ASP.NET kullanma hakkında daha fazla bilgi için, [Azure'daki bir web rolünde Twilio'su kullanarak nasıl telefon görüşmesi yapılacağını][howto_phonecall_dotnet]öğrenin.

[!INCLUDE [twilio-additional-services-and-next-steps](../includes/twilio-additional-services-and-next-steps.md)]

[howto_phonecall_dotnet]: partner-twilio-cloud-services-dotnet-phone-call-web-role.md

[twimlet_message_url]: https://twimlets.com/message

[twilio_rest_making_calls]: https://www.twilio.com/docs/api/rest/making-calls

[vs_project]:https://docs.microsoft.com/visualstudio/azure/vs-azure-tools-azure-project-create
[nuget]:https://nuget.org/
[twilio_github_repo]:https://github.com/twilio/twilio-csharp

[twilio_libraries]: https://www.twilio.com/docs/libraries
[twiml]: https://www.twilio.com/docs/api/twiml
[twilio_api]: https://www.twilio.com/api
[try_twilio]: https://www.twilio.com/try-twilio
[twilio_account]:  https://www.twilio.com/console
[verify_phone]: https://www.twilio.com/console/phone-numbers/verified
