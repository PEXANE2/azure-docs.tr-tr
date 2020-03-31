---
title: Ses ve SMS (Java) için Twilio Nasıl Kullanılır | Microsoft Dokümanlar
description: Azure'daki Twilio API hizmetiyle nasıl telefon görüşmesi yapacağınızı ve SMS mesajı göndermeyi öğrenin. Java'da yazılan kod örnekleri.
services: ''
documentationcenter: java
author: georgewallace
ms.assetid: f3508965-5527-4255-9d51-5d5f926f4d43
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: Java
ms.topic: article
ms.date: 11/25/2014
ms.author: gwallace
ms.openlocfilehash: 18e93ce18ed746612996399dc1aeb258abd26165
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "69637218"
---
# <a name="how-to-use-twilio-for-voice-and-sms-capabilities-in-java"></a>Java'da Ses ve SMS Özellikleri için Twilio Nasıl Kullanılır?
Bu kılavuz, Azure'daki Twilio API hizmetiyle ortak programlama görevlerinin nasıl gerçekleştirildirilebildiğini gösterir. Kapsanan senaryolar arasında telefon görüşmesi yapmak ve Kısa Mesaj Hizmeti (SMS) mesajı göndermek yer almaktadır. Twilio hakkında daha fazla bilgi ve uygulamalarınızda ses ve SMS kullanmak için [Sonraki Adımlar](#NextSteps) bölümüne bakın.

## <a name="what-is-twilio"></a><a id="WhatIs"></a>Twilio nedir?
Twilio, ses ve SMS uygulamaları oluşturmak için mevcut web dillerinizi ve becerilerinizi kullanmanıza olanak tanıyan bir telefon web hizmeti API'sidir. Twilio bir üçüncü taraf hizmetidir (Azure özelliği değil, Microsoft ürünü değildir).

**Twilio Voice,** uygulamalarınızın telefon görüşmesi yapmasına ve almasını sağlar. **Twilio SMS,** uygulamalarınızın SMS mesajları yapmasına ve almasını sağlar. **Twilio Client,** uygulamalarınızın mobil bağlantılar da dahil olmak üzere mevcut Internet bağlantılarını kullanarak sesli iletişimi etkinleştirmesine olanak tanır.

## <a name="twilio-pricing-and-special-offers"></a><a id="Pricing"></a>Twilio Fiyatlandırma ve Özel Teklifler
Twilio fiyatlandırması hakkında bilgi [Twilio Fiyatlandırma][twilio_pricing]mevcuttur. Azure müşterileri [özel][special_offer]bir teklif alır: 1000 metin veya 1000 gelen dakikalık ücretsiz kredi. Bu teklife kaydolmak veya daha fazla [https://ahoy.twilio.com/azure][special_offer]bilgi almak için lütfen .

## <a name="concepts"></a><a id="Concepts"></a>Kavramlar
Twilio API, uygulamalar için ses ve SMS işlevselliği sağlayan restful API'dir. İstemci kitaplıkları birden çok dilde kullanılabilir; bir liste [için, Bkz. Twilio API Kitaplıkları.][twilio_libraries]

Twilio API'nin temel yönleri Twilio fiilleri ve Twilio Markup Language (TwiML) 'dir.

### <a name="twilio-verbs"></a><a id="Verbs"></a>Twilio Fiiller
API, Twilio fiillerini kullanır; örneğin, ** &lt;Say&gt; ** fiili Twilio'ya bir çağrıda sesli bir ileti iletmesini söyler.

Aşağıda Twilio fiillerinin bir listesi vetifer.

* Arama : Arayanı başka bir telefona bağlar. ** &lt;&gt;**
* Topla : Telefon tuş takımıüzerinde girilen sayısal rakamları toplar. ** &lt;&gt;**
* **Hangup&gt;: Bir aramayı &lt;** bitirir.
* Yürüt : Ses dosyalarını çalar. ** &lt;&gt;**
* Sıra : Arayanlar kuyruğuna ekleyin. ** &lt;&gt;**
* **Duraklatma&gt;: Belirli sayıda saniye sessizce bekler. &lt;**
* Kayıt : Arayanın sesini kaydeder ve kaydı içeren bir dosyanın URL'sini döndürür. ** &lt;&gt;**
* Yönlendirme : Bir arama veya SMS denetimini farklı bir URL'den TwiML'e aktarın. ** &lt;&gt;**
* **Reddet&gt;: Twilio numaranıza gelen bir aramayı faturalandırmadan &lt;** reddeder.
* Say : Metni, aramada yapılan konuşmaya dönüştürür. ** &lt;&gt;**
* Sms : SMS mesajı gönderir. ** &lt;&gt;**

### <a name="twiml"></a><a id="TwiML"></a>TwiML
TwiML, Twilio'ya bir çağrıveya SMS'in nasıl işlenirolduğunu bildiren Twilio fiillerine dayanan XML tabanlı talimatlar kümesidir.

Örnek olarak, aşağıdaki TwiML metin Merhaba Dünya dönüştürmek **istiyorsunuz!** konuşmaya.

```xml
    <?xml version="1.0" encoding="UTF-8" ?>
    <Response>
       <Say>Hello World!</Say>
    </Response>
```

Uygulamanız Twilio API'yi aradığında, API parametrelerinden biri TwiML yanıtını döndüren URL'dir. Geliştirme amacıyla, uygulamalarınız tarafından kullanılan TwiML yanıtlarını sağlamak için Twilio tarafından sağlanan URL'leri kullanabilirsiniz. Ayrıca, TwiML yanıtlarını oluşturmak için kendi URL'lerinizi barındırabilirsiniz ve başka bir seçenek de **TwiMLResponse** nesnesini kullanmaktır.

Twilio fiilleri, öznitelikleri ve TwiML hakkında daha fazla bilgi için Bkz. [TwiML][twiml]. Twilio API hakkında daha fazla bilgi için [Twilio API'ye][twilio_api]bakın.

## <a name="create-a-twilio-account"></a><a id="CreateAccount"></a>Twilio Hesabı Oluşturma
Bir Twilio hesabı almaya hazır olduğunuzda, [Try Twilio'ya][try_twilio]kaydolun. Ücretsiz bir hesapla başlayabilir ve hesabınızı daha sonra yükseltebilirsiniz.

Bir Twilio hesabına kaydolduğunuzda, bir hesap kimliği ve kimlik doğrulama belirteci alırsınız. Her ikisi de Twilio API aramaları yapmak için gerekli olacaktır. Hesabınıza yetkisiz erişimi önlemek için kimlik doğrulama belirtecinizi güvende tutun. Hesap kimliğiniz ve kimlik doğrulama belirteciniz, sırasıyla **ACCOUNT SID** ve **AUTH TOKEN**etiketli alanlarda [Twilio][twilio_console]Konsolu'nda görüntülenebilir.

## <a name="create-a-java-application"></a><a id="create_app"></a>Java Uygulaması Oluşturma
1. Twilio JAR'ı edinin ve Java yapı yolunuza ve WAR dağıtım derlemenize ekleyin. At [https://github.com/twilio/twilio-java][twilio_java], GitHub kaynakları indirebilir ve kendi JAR oluşturmak, ya da önceden oluşturulmuş bir JAR indirin (ya da bağımlılıkları olmadan).
2. JDK'nızın **cacerts** keystore'unda MD5 parmak izi 67:CB:9D:C0:13:24:8A:82:9B:B2:17:1E:D1:1B:D4 içeren Equifax Secure Certificate Authority sertifikası nın bulunduğundan emin olun (seri numarası 3'tur 5:DE:F4:CF ve SHA1 parmak izi D2:32:09:AD:23:D3:14:23:21:74:E4:0D:7F:9D:62:13:97:86:63:3A' dır. Bu, Twilio API'lerini kullandığınızda çağrılan [https://api.twilio.com][twilio_api_service] hizmetin sertifika yetkilisi (CA) sertifikasıdır. JDK'nızın **cacerts** keystore'unun doğru CA sertifikasını içerdiğinden emin olmak hakkında bilgi [için][add_ca_cert]bkz.

Java için Twilio istemci kitaplığını kullanmak için ayrıntılı talimatlar, [Azure'daki bir Java Uygulamasında Twilio'yu Kullanarak Telefon Görüşmesi Yapma Hakkında][howto_phonecall_java]Bilgi Edinilebilir.

## <a name="configure-your-application-to-use-twilio-libraries"></a><a id="configure_app"></a>Uygulamanızı Twilio Kitaplıklarını Kullanacak Şekilde Yapılandırın
Kodunuz içinde, uygulamanızda kullanmak istediğiniz Twilio paketleri veya sınıfları için kaynak dosyalarınızın en üstüne **alma** deyimleri ekleyebilirsiniz.

Java kaynak dosyaları için:

```java
    import com.twilio.*;
    import com.twilio.rest.api.*;
    import com.twilio.type.*;
    import com.twilio.twiml.*;
```

Java Server Page (JSP) kaynak dosyaları için:

```java
    import="com.twilio.*"
    import="com.twilio.rest.api.*"
    import="com.twilio.type.*"
    import="com.twilio.twiml.*"
 ```
 
Kullanmak istediğiniz Twilio paketlerine veya sınıflarına bağlı **olarak, alma** ekstreleriniz farklı olabilir.

## <a name="how-to-make-an-outgoing-call"></a><a id="howto_make_call"></a>Nasıl yapılır: Giden arama yapma
Aşağıda, **Arama** sınıfını kullanarak giden aramanın nasıl yapılacağını gösterilmektedir. Bu kod, Twilio İşaretdili Dili (TwiML) yanıtını döndürmek için Twilio tarafından sağlanan bir site de kullanır. Değerlerinizi **gelen** **ve** telefon numaralarıyla değiştirin ve kodu çalıştırmadan önce Twilio hesabınız için **telefon** numarasını doğruladığınızdan emin olun.

```java
    // Use your account SID and authentication token instead
    // of the placeholders shown here.
    String accountSID = "your_twilio_account_SID";
    String authToken = "your_twilio_authentication_token";

    // Initialize the Twilio client.
    Twilio.init(accountSID, authToken);

    // Use the Twilio-provided site for the TwiML response.
    URI uri = new URI("https://twimlets.com/message" +
            "?Message%5B0%5D=Hello%20World%21");

    // Declare To and From numbers
    PhoneNumber to = new PhoneNumber("NNNNNNNNNN");
    PhoneNumber from = new PhoneNumber("NNNNNNNNNN");

    // Create a Call creator passing From, To and URL values
    // then make the call by executing the create() method
    Call.creator(to, from, uri).create();
```

**Call.creator** yöntemine geçirilen parametreler hakkında daha fazla [https://www.twilio.com/docs/api/rest/making-calls][twilio_rest_making_calls]bilgi için bkz.

Belirtildiği gibi, bu kod TwiML yanıtını döndürmek için Twilio tarafından sağlanan bir site kullanır. Bunun yerine TwiML yanıtı sağlamak için kendi sitenizi kullanabilirsiniz; daha fazla bilgi için [Azure'daki bir Java Uygulamasında TwiML Yanıtları Nasıl Sağlar'](#howto_provide_twiml_responses)a bakın.

## <a name="how-to-send-an-sms-message"></a><a id="howto_send_sms"></a>Nasıl yapılsın: SMS mesajı gönder
Aşağıda, **İleti** sınıfını kullanarak SMS iletisi nasıl gönderilen bir mesaj gösterilmektedir. **Numaradan,** **4155992671**, SMS mesajları göndermek için deneme hesapları için Twilio tarafından sağlanmaktadır. Kodu **to** çalıştırmadan önce Twilio hesabınız için numara doğrulanmalıdır.

```java
    // Use your account SID and authentication token instead
    // of the placeholders shown here.
    String accountSID = "your_twilio_account_SID";
    String authToken = "your_twilio_authentication_token";

    // Initialize the Twilio client.
    Twilio.init(accountSID, authToken);

    // Declare To and From numbers and the Body of the SMS message
    PhoneNumber to = new PhoneNumber("+14159352345"); // Replace with a valid phone number for your account.
    PhoneNumber from = new PhoneNumber("+14158141829"); // Replace with a valid phone number for your account.
    String body = "Where's Wallace?";

    // Create a Message creator passing From, To and Body values
    // then send the SMS message by calling the create() method
    Message sms = Message.creator(to, from, body).create();
```

**Message.creator** yöntemine geçirilen parametreler hakkında daha fazla [https://www.twilio.com/docs/api/rest/sending-sms][twilio_rest_sending_sms]bilgi için bkz.

## <a name="how-to-provide-twiml-responses-from-your-own-website"></a><a id="howto_provide_twiml_responses"></a>Nasıl?
Uygulamanız Twilio API'ye, örneğin **CallCreator.create** yöntemi aracılığıyla bir çağrı başlattığında, Twilio isteğinizi TwiML yanıtı döndürmesi beklenen bir URL'ye gönderir. Yukarıdaki örnekte Twilio tarafından [https://twimlets.com/message][twimlet_message_url]sağlanan URL kullanır. (TwiML Web hizmetleri tarafından kullanılmak üzere tasarlanmış olsa da, Tarayıcınızda TwiML görüntüleyebilirsiniz. Örneğin, boş [https://twimlets.com/message][twimlet_message_url] ** &lt;&gt; ** bir Yanıt öğesini görmek için tıklatın; başka bir örnek [https://twimlets.com/message?Message%5B0%5D=Hello%20World%21][twimlet_message_url_hello_world] olarak, Bir ** &lt;Say&gt; ** öğesi içeren bir ** &lt;Yanıt&gt; ** öğesini görmek için tıklatın.)

Twilio tarafından sağlanan URL'ye güvenmek yerine, HTTP yanıtlarını döndüren kendi URL sitenizi oluşturabilirsiniz. Siteyi HTTP yanıtlarını döndüren herhangi bir dilde oluşturabilirsiniz; Bu konu, URL'yi bir JSP sayfasında barındıracağınız varsayar.

Aşağıdaki JSP sayfası Merhaba Dünya diyor bir TwiML yanıtı **sonuçları!** çağrı üzerine.

```xml
    <%@ page contentType="text/xml" %>
    <Response>
        <Say>Hello World!</Say>
    </Response>
```

Aşağıdaki JSP sayfası, bazı metinlerin birkaç duraklaması olduğunu ve Twilio API sürümü ve Azure rol adı hakkında bilgi söyleyen bir TwiML yanıtıyla sonuçlanır.

```xml
    <%@ page contentType="text/xml" %>
    <Response>
        <Say>Hello from Azure!</Say>
        <Pause></Pause>
        <Say>The Twilio API version is <%= request.getParameter("ApiVersion") %>.</Say>
        <Say>The Azure role name is <%= System.getenv("RoleName") %>.</Say>
        <Pause></Pause>
        <Say>Good bye.</Say>
    </Response>
```

**ApiVersion** parametresi Twilio sesli isteklerde kullanılabilir (SMS isteklerine değil). Twilio ses ve SMS istekleri için kullanılabilir <https://www.twilio.com/docs/api/twiml/twilio_request> istek <https://www.twilio.com/docs/api/twiml/sms/twilio_request>parametrelerini görmek için sırasıyla bkz. **RoleName** ortamı değişkeni, Azure dağıtımının bir parçası olarak kullanılabilir. **(System.getenv'den**alınabilecek şekilde özel ortam değişkenleri eklemek istiyorsanız, Çeşitli Rol [Yapılandırma Ayarları'ndaki][misc_role_config_settings]ortam değişkenleri bölümüne bakın.)

JSP sayfanızı TwiML yanıtları sağlamak üzere ayarladıktan sonra, URL **Call.creator** yöntemine geçerken JSP sayfasının URL'sini kullanın. Örneğin, Azure barındırılan bir hizmete dağıtılan MyTwiML adlı bir Web uygulamanız varsa ve JSP sayfasının adı mytwiml.jsp ise, URL aşağıdaki gibi **Call.creator'a** aktarılabilir:

```java
    // Declare To and From numbers and the URL of your JSP page
    PhoneNumber to = new PhoneNumber("NNNNNNNNNN");
    PhoneNumber from = new PhoneNumber("NNNNNNNNNN");
    URI uri = new URI("http://<your_hosted_service>.cloudapp.net/MyTwiML/mytwiml.jsp");

    // Create a Call creator passing From, To and URL values
    // then make the call by executing the create() method
    Call.creator(to, from, uri).create();
```

TwiML ile yanıt vermek için başka bir seçenek **com.twilio.twiml** paketinde bulunan **VoiceResponse** sınıfı üzerinden.

Twilio'yu Azure'da Java ile kullanma hakkında daha fazla bilgi için, [Azure'daki bir Java Uygulamasında Twilio'yu Kullanarak Telefon Görüşmesi Yapma][howto_phonecall_java]Hakkında bilgi .

## <a name="how-to-use-additional-twilio-services"></a><a id="AdditionalServices"></a>Nasıl Kullanılır: Ek Twilio Hizmetlerini Kullanın
Burada gösterilen örneklere ek olarak, Twilio Azure uygulamanızdan ek Twilio işlevselliği nden yararlanmak için kullanabileceğiniz web tabanlı API'ler sunar. Tüm ayrıntılar için [Twilio API belgelerine][twilio_api_documentation]bakın.

## <a name="next-steps"></a><a id="NextSteps"></a>Sonraki Adımlar
Artık Twilio hizmetinin temellerini öğrendiğiniz için, daha fazla bilgi edinmek için aşağıdaki bağlantıları izleyin:

* [Twilio Güvenlik Yönergeleri][twilio_security_guidelines]
* [Twilio HowTo's ve Örnek Kodu][twilio_howtos]
* [Twilio Quickstart Öğreticiler][twilio_quickstarts]
* [GitHub üzerinde Twilio][twilio_on_github]
* [Twilio Desteği ile konuşun][twilio_support]

[twilio_java]: https://github.com/twilio/twilio-java
[twilio_api_service]: https://api.twilio.com
[add_ca_cert]: java-add-certificate-ca-store.md
[howto_phonecall_java]: partner-twilio-java-phone-call-example.md
[misc_role_config_settings]: https://msdn.microsoft.com/library/windowsazure/hh690945.aspx
[twimlet_message_url]: https://twimlets.com/message
[twimlet_message_url_hello_world]: https://twimlets.com/message?Message%5B0%5D=Hello%20World%21
[twilio_rest_making_calls]: https://www.twilio.com/docs/api/rest/making-calls
[twilio_rest_sending_sms]: https://www.twilio.com/docs/api/rest/sending-sms
[twilio_pricing]: https://www.twilio.com/pricing
[special_offer]: https://ahoy.twilio.com/azure
[twilio_libraries]: https://www.twilio.com/docs/libraries
[twiml]: https://www.twilio.com/docs/api/twiml
[twilio_api]: https://www.twilio.com/api
[try_twilio]: https://www.twilio.com/try-twilio
[twilio_console]:  https://www.twilio.com/console
[verify_phone]: https://www.twilio.com/console/phone-numbers/verified#
[twilio_api_documentation]: https://www.twilio.com/docs
[twilio_security_guidelines]: https://www.twilio.com/docs/security
[twilio_howtos]: https://www.twilio.com/docs/howto
[twilio_on_github]: https://github.com/twilio
[twilio_support]: https://www.twilio.com/help/contact
[twilio_quickstarts]: https://www.twilio.com/docs/quickstart
