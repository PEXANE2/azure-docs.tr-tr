---
title: Ses ve SMS (PHP) için Twilio Nasıl Kullanılır | Microsoft Dokümanlar
description: Azure'daki Twilio API hizmetiyle nasıl telefon görüşmesi yapacağınızı ve SMS mesajı göndermeyi öğrenin. PHP ile yazılmış kod örnekleri.
documentationcenter: php
services: ''
author: georgewallace
ms.assetid: 007f22e3-ac75-4868-8315-da000c2e0dd0
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: PHP
ms.topic: article
ms.date: 11/25/2014
ms.author: gwallace
ms.openlocfilehash: f9fb250109a1c9000eae8da0d6337c96f19f0f89
ms.sourcegitcommit: 27bbda320225c2c2a43ac370b604432679a6a7c0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/31/2020
ms.locfileid: "80410551"
---
# <a name="how-to-use-twilio-for-voice-and-sms-capabilities-in-php"></a>PHP'de Ses ve SMS Özellikleri için Twilio Nasıl Kullanılır?
Bu kılavuz, Azure'daki Twilio API hizmetiyle ortak programlama görevlerinin nasıl gerçekleştirildirilebildiğini gösterir. Kapsanan senaryolar arasında telefon görüşmesi yapmak ve Kısa Mesaj Hizmeti (SMS) mesajı göndermek yer almaktadır. Twilio hakkında daha fazla bilgi ve uygulamalarınızda ses ve SMS kullanmak için [Sonraki Adımlar](#NextSteps) bölümüne bakın.

## <a name="what-is-twilio"></a><a id="WhatIs"></a>Twilio nedir?
Twilio, iş iletişiminin geleceğine güç vererek geliştiricilerin ses, VoIP ve mesajlaşmayı uygulamalara yerleştirmesini sağlıyor. Bulut tabanlı, küresel bir ortamda ihtiyaç duyulan tüm altyapıyı sanallaştırarak Twilio iletişim API platformu aracılığıyla ortaya çıkarırlar. Uygulamalar oluşturmak için basit ve ölçeklenebilir. Gittikçe öde fiyatlandırması ile esnekliğin keyfini çıkarın ve bulut güvenilirliğinden yararlanın.

**Twilio Voice,** uygulamalarınızın telefon görüşmesi yapmasına ve almasını sağlar. **Twilio SMS,** uygulamanızın kısa mesaj gönderip almasını sağlar. **Twilio Client** herhangi bir telefon, tablet veya tarayıcıdan VoIP aramaları yapmanızı sağlar ve WebRTC'yi destekler.

## <a name="twilio-pricing-and-special-offers"></a><a id="Pricing"></a>Twilio Fiyatlandırma ve Özel Teklifler
Azure müşterilerine [özel](https://www.twilio.com/azure)bir teklif alır: Twilio Hesabınızı yükselttiğinde ücretsiz $10 Twilio Kredisi. Bu Twilio Kredisi herhangi bir Twilio kullanımına uygulanabilir (telefon numaranızın ve mesajınızın veya arama hedefinizin konumuna bağlı olarak 1000'e kadar SMS mesajı göndermeye veya 1000'e kadar gelen Sesli dakika almaya eşdeğer 10$'lık kredi). Bu Twilio kredi nizi kullanın ve şu anda başlayın: [https://ahoy.twilio.com/azure](https://ahoy.twilio.com/azure).

Twilio bir öde-as-you-go hizmetidir. Kurulum ücreti yoktur ve hesabınızı istediğiniz zaman kapatabilirsiniz. Daha fazla ayrıntıyı [Twilio Fiyatlandırma][twilio_pricing]adresinde bulabilirsiniz.

## <a name="concepts"></a><a id="Concepts"></a>Kavramlar
Twilio API, uygulamalar için ses ve SMS işlevselliği sağlayan restful API'dir. İstemci kitaplıkları birden çok dilde kullanılabilir; bir liste [için, Bkz. Twilio API Kitaplıkları.][twilio_libraries]

Twilio API'nin temel yönleri Twilio fiilleri ve Twilio Markup Language (TwiML) 'dir.

### <a name="twilio-verbs"></a><a id="Verbs"></a>Twilio Fiiller
API, Twilio fiillerini kullanır; örneğin, ** &lt;Say&gt; ** fiili Twilio'ya bir çağrıda sesli bir ileti iletmesini söyler.

Aşağıda Twilio fiillerinin bir listesi vetifer. [Twilio Markup Language belgeleri](https://www.twilio.com/docs/api/twiml)aracılığıyla diğer fiiller ve yetenekler hakkında bilgi edinin.

* Arama : Arayanı başka bir telefona bağlar. ** &lt;&gt;**
* Topla : Telefon tuş takımıüzerinde girilen sayısal rakamları toplar. ** &lt;&gt;**
* **Hangup&gt;: Bir aramayı &lt;** bitirir.
* Yürüt : Ses dosyalarını çalar. ** &lt;&gt;**
* **Duraklatma&gt;: Belirli sayıda saniye sessizce bekler. &lt;**
* Kayıt : Arayanın sesini kaydeder ve kaydı içeren bir dosyanın URL'sini döndürür. ** &lt;&gt;**
* Yönlendirme : Bir arama veya SMS denetimini farklı bir URL'den TwiML'e aktarın. ** &lt;&gt;**
* **Reddet&gt;: Twilio numaranıza gelen bir aramayı faturalandırmadan &lt;** reddeder
* Say : Metni, aramada yapılan konuşmaya dönüştürür. ** &lt;&gt;**
* Sms : SMS mesajı gönderir. ** &lt;&gt;**

### <a name="twiml"></a><a id="TwiML"></a>TwiML
TwiML, Twilio'ya bir çağrıveya SMS'in nasıl işlenirolduğunu bildiren Twilio fiillerine dayanan XML tabanlı talimatlar kümesidir.

Örnek olarak, aşağıdaki TwiML metni **Hello World'ü** konuşmaya dönüştürür.

    <?xml version="1.0" encoding="UTF-8" ?>
    <Response>
       <Say>Hello World</Say>
    </Response>

Uygulamanız Twilio API'yi aradığında, API parametrelerinden biri TwiML yanıtını döndüren URL'dir. Geliştirme amacıyla, uygulamalarınız tarafından kullanılan TwiML yanıtlarını sağlamak için Twilio tarafından sağlanan URL'leri kullanabilirsiniz. Ayrıca, TwiML yanıtlarını oluşturmak için kendi URL'lerinizi barındırabilirsiniz ve başka bir seçenek de **TwiMLResponse** nesnesini kullanmaktır.

Twilio fiilleri, öznitelikleri ve TwiML hakkında daha fazla bilgi için Bkz. [TwiML][twiml]. Twilio API hakkında daha fazla bilgi için [Twilio API'ye][twilio_api]bakın.

## <a name="create-a-twilio-account"></a><a id="CreateAccount"></a>Twilio Hesabı Oluşturma
Bir Twilio hesabı almaya hazır olduğunuzda, [Try Twilio'ya][try_twilio]kaydolun. Ücretsiz bir hesapla başlayabilir ve hesabınızı daha sonra yükseltebilirsiniz.

Bir Twilio hesabına kaydolduğunuzda, bir hesap kimliği ve kimlik doğrulama belirteci alırsınız. Her ikisi de Twilio API aramaları yapmak için gerekli olacaktır. Hesabınıza yetkisiz erişimi önlemek için kimlik doğrulama belirtecinizi güvende tutun. Hesap kimliğiniz ve kimlik doğrulama belirteciniz, sırasıyla **ACCOUNT SID** ve **AUTH TOKEN**etiketli alanlarda, [Twilio hesap sayfasında][twilio_account]görüntülenebilir.

## <a name="create-a-php-application"></a><a id="create_app"></a>PHP Uygulaması Oluşturma
Twilio hizmetini kullanan ve Azure'da çalışan bir PHP uygulaması, Twilio hizmetini kullanan diğer PHP uygulamalardan farklı değildir. Twilio hizmetleri REST tabanlı ve çeşitli şekillerde PHP çağrılabilir iken, bu makalede [GitHub PHP için Twilio kütüphane ile Twilio][twilio_php]hizmetlerinin nasıl kullanılacağı üzerinde durulacak. PHP için Twilio kitaplığını kullanma hakkında [https://www.twilio.com/docs/libraries/php][twilio_lib_docs]daha fazla bilgi için bkz.

Azure'da bir Twilio/PHP uygulaması oluşturmak ve dağıtmak için ayrıntılı talimatlar, [Azure'daki bir PHP Uygulamasında Twilio'yu Kullanarak Telefon Görüşmesi Yapma Hakkında][howto_phonecall_php]Bilgi Edinilebilir.

## <a name="configure-your-application-to-use-twilio-libraries"></a><a id="configure_app"></a>Uygulamanızı Twilio Kitaplıklarını Kullanacak Şekilde Yapılandırın
Uygulamanızı PHP için Twilio kitaplığını kullanacak şekilde iki şekilde yapılandırabilirsiniz:

1. PHP için Twilio kitaplığını GitHub'dan indirin ([https://github.com/twilio/twilio-php][twilio_php]) ve **Uygulamanıza Hizmetler** dizinini ekleyin.
   
    -VEYA-
2. PHP için Twilio kitaplığını ARMUT paketi olarak yükleyin. Aşağıdaki komutlarla yüklenebilir:
   
        $ pear channel-discover twilio.github.com/pear
        $ pear install twilio/Services_Twilio

PHP için Twilio kitaplığını yükledikten sonra, kitaplık başvurusu için PHP dosyalarınızın en üstüne **bir require_once** ifadesi ekleyebilirsiniz:

        require_once 'Services/Twilio.php';

Daha fazla bilgi [https://github.com/twilio/twilio-php/blob/master/README.md][twilio_github_readme]için bkz.

## <a name="how-to-make-an-outgoing-call"></a><a id="howto_make_call"></a>Nasıl yapılır: Giden arama yapma
Aşağıda, **Services_Twilio** sınıfı kullanılarak giden bir aramanın nasıl yapılacağını gösterilmektedir. Bu kod, Twilio İşaretdili Dili (TwiML) yanıtını döndürmek için Twilio tarafından sağlanan bir site de kullanır. **Değerlerinizi, Gönderen** ve **To** telefon numaralarıyla değiştirin ve kodu çalıştırmadan önce Twilio hesabınız için **Telefon** numaranızı doğruladığınızdan emin olun.

    // Include the Twilio PHP library.
    require_once 'Services/Twilio.php';

    // Library version.
    $version = "2010-04-01";

    // Set your account ID and authentication token.
    $sid = "your_twilio_account_sid";
    $token = "your_twilio_authentication_token";

    // The number of the phone initiating the call.
    $from_number = "NNNNNNNNNNN";

    // The number of the phone receiving call.
    $to_number = "NNNNNNNNNNN";

    // Use the Twilio-provided site for the TwiML response.
    $url = "https://twimlets.com/message";

    // The phone message text.
    $message = "Hello world.";

    // Create the call client.
    $client = new Services_Twilio($sid, $token, $version);

    //Make the call.
    try
    {
        $call = $client->account->calls->create(
            $from_number, 
            $to_number,
              $url.'?Message='.urlencode($message)
        );
    }
    catch (Exception $e) 
    {
        echo 'Error: ' . $e->getMessage();
    }

Belirtildiği gibi, bu kod TwiML yanıtını döndürmek için Twilio tarafından sağlanan bir site kullanır. Bunun yerine TwiML yanıtı sağlamak için kendi sitenizi kullanabilirsiniz; daha fazla bilgi için, [Kendi Web Sitenizden TwiML Yanıtları Nasıl Sağlar'](#howto_provide_twiml_responses)a bakın.

* **Not**: TLS/SSL sertifika doğrulama hatalarını gidermek için bkz.[http://readthedocs.org/docs/twilio-php/en/latest/usage/rest.html][ssl_validation] 

## <a name="how-to-send-an-sms-message"></a><a id="howto_send_sms"></a>Nasıl yapılsın: SMS mesajı gönder
Aşağıda, **Services_Twilio** sınıfını kullanarak SMS iletisi nasıl gönderilen bir mesaj gösterilmektedir. **From** numarası, SMS mesajları göndermek için deneme hesapları için Twilio tarafından sağlanmaktadır. Kodu çalıştırmadan önce Twilio hesabınız için **To** numarası doğrulanmalıdır.

    // Include the Twilio PHP library.
    require_once 'Services/Twilio.php';

    // Library version.
    $version = "2010-04-01";

    // Set your account ID and authentication token.
    $sid = "your_twilio_account_sid";
    $token = "your_twilio_authentication_token";


    $from_number = "NNNNNNNNNNN"; // With trial account, texts can only be sent from your Twilio number.
    $to_number = "NNNNNNNNNNN";
    $message = "Hello world.";

    // Create the call client.
    $client = new Services_Twilio($sid, $token, $version);

    // Send the SMS message.
    try
    {
        $client->$client->account->messages->sendMessage($from_number, $to_number, $message);
    }
    catch (Exception $e) 
    {
        echo 'Error: ' . $e->getMessage();
    }

## <a name="how-to-provide-twiml-responses-from-your-own-website"></a><a id="howto_provide_twiml_responses"></a>Nasıl?
Başvurunuz Twilio API'ye bir çağrı başlattığında, Twilio isteğinizi TwiML yanıtı döndürmesi beklenen bir URL'ye gönderir. Yukarıdaki örnekte Twilio tarafından [https://twimlets.com/message][twimlet_message_url]sağlanan URL kullanır. (TwiML Twilio tarafından kullanılmak üzere tasarlanmış olsa da, tarayıcınızda görüntüleyebilirsiniz. Örneğin, boş [https://twimlets.com/message][twimlet_message_url] `<Response>` bir öğeyi görmek için tıklatın; başka bir örnek [https://twimlets.com/message?Message%5B0%5D=Hello%20World][twimlet_message_url_hello_world] olarak, `<Response>` bir `<Say>` öğe içeren bir öğeyi görmek için tıklatın.)

Twilio tarafından sağlanan URL'ye güvenmek yerine, HTTP yanıtlarını döndüren kendi sitenizi oluşturabilirsiniz. Siteyi XML yanıtlarını döndüren herhangi bir dilde oluşturabilirsiniz; Bu konu, TwiML oluşturmak için PHP kullanacağınızı varsayar.

Aşağıdaki PHP sayfası, çağrıda **Merhaba Dünya** yazan bir TwiML yanıtıyla sonuçlanır.

    <?php    
        header("content-type: text/xml");    
        echo "<?xml version=\"1.0\" encoding=\"UTF-8\"?>\n";
    ?>
    <Response>    
        <Say>Hello world.</Say>
    </Response>

Yukarıdaki örnekten de görebileceğiniz gibi, TwiML yanıtı sadece bir XML belgesidir. PHP için Twilio kitaplığı, sizin için TwiML oluşturacak sınıflar içerir. Aşağıdaki örnekte, yukarıda gösterildiği gibi eşdeğer yanıt üretir, ancak PHP için Twilio kitaplığında **Hizmetler\_Twilio\_Twiml** sınıfını kullanır:

    require_once('Services/Twilio.php');

    $response = new Services_Twilio_Twiml();
    $response->say("Hello world.");
    print $response;

TwiML hakkında daha fazla [https://www.twilio.com/docs/api/twiml][twiml_reference]bilgi için bkz. 

PHP sayfanızı TwiML yanıtları sağlamak üzere ayarladıktan sonra, URL `Services_Twilio->account->calls->create` yönteme geçerken PHP sayfasının URL'sini kullanın. Örneğin, Azure barındırılan bir hizmete dağıtılan **MyTwiML** adlı bir Web uygulamanız varsa ve PHP sayfasının adı **mytwiml.php**ise, URL aşağıdaki örnekte gösterildiği gibi **Services_Twilio >hesap >çağrıları>oluşturulabilir:**

    require_once 'Services/Twilio.php';

    $sid = "your_twilio_account_sid";
    $token = "your_twilio_authentication_token";
    $from_number = "NNNNNNNNNNN";
    $to_number = "NNNNNNNNNNN";
    $url = "http://<your_hosted_service>.cloudapp.net/MyTwiML/mytwiml.php";

    // The phone message text.
    $message = "Hello world.";

    $client = new Services_Twilio($sid, $token, "2010-04-01");

    try
    {
        $call = $client->account->calls->create(
            $from_number, 
            $to_number,
              $url.'?Message='.urlencode($message)
        );
    }
    catch (Exception $e) 
    {
        echo 'Error: ' . $e->getMessage();
    }

Twilio'yu Azure'da PHP ile kullanma hakkında daha fazla bilgi için, [Azure'daki PHP Uygulamasında Twilio'yu Kullanarak Telefon Görüşmesi Yapma][howto_phonecall_php]Hakkında Bilgi .

## <a name="how-to-use-additional-twilio-services"></a><a id="AdditionalServices"></a>Nasıl Kullanılır: Ek Twilio Hizmetlerini Kullanın
Burada gösterilen örneklere ek olarak, Twilio Azure uygulamanızdan ek Twilio işlevselliği nden yararlanmak için kullanabileceğiniz web tabanlı API'ler sunar. Tüm ayrıntılar için [Twilio API belgelerine][twilio_api_documentation]bakın.

## <a name="next-steps"></a><a id="NextSteps"></a>Sonraki Adımlar
Artık Twilio hizmetinin temellerini öğrendiğiniz için, daha fazla bilgi edinmek için aşağıdaki bağlantıları izleyin:

* [Twilio Güvenlik Yönergeleri][twilio_security_guidelines]
* [Twilio HowTo's ve Örnek Kodu][twilio_howtos]
* [Twilio Quickstart Öğreticiler][twilio_quickstarts] 
* [GitHub üzerinde Twilio][twilio_on_github]
* [Twilio Desteği ile konuşun][twilio_support]

[twilio_php]: https://github.com/twilio/twilio-php
[twilio_lib_docs]: https://www.twilio.com/docs/libraries/php
[twilio_github_readme]: https://github.com/twilio/twilio-php/blob/master/README.md
[ssl_validation]: https://www.twilio.com/docs/api/errors
[twilio_api_service]: https://api.twilio.com
[howto_phonecall_php]: partner-twilio-php-make-phone-call.md
[twilio_voice_request]: https://www.twilio.com/docs/api/twiml/twilio_request
[twilio_sms_request]: https://www.twilio.com/docs/api/twiml/sms/twilio_request
[misc_role_config_settings]: https://msdn.microsoft.com/library/windowsazure/hh690945.aspx
[twimlet_message_url]: https://twimlets.com/message
[twimlet_message_url_hello_world]: https://twimlets.com/message?Message%5B0%5D=Hello%20World
[twiml_reference]: https://www.twilio.com/docs/api/twiml
[twilio_pricing]: https://www.twilio.com/pricing
[special_offer]: https://ahoy.twilio.com/azure
[twilio_libraries]: https://www.twilio.com/docs/libraries
[twiml]: https://www.twilio.com/docs/api/twiml
[twilio_api]: https://www.twilio.com/api
[try_twilio]: https://www.twilio.com/try-twilio
[twilio_account]:  https://www.twilio.com/user/account
[verify_phone]: https://www.twilio.com/user/account/phone-numbers/verified#
[twilio_api_documentation]: https://www.twilio.com/api
[twilio_security_guidelines]: https://www.twilio.com/docs/security
[twilio_howtos]: https://www.twilio.com/docs/howto
[twilio_on_github]: https://github.com/twilio
[twilio_support]: https://www.twilio.com/help/contact
[twilio_quickstarts]: https://www.twilio.com/docs/quickstart
