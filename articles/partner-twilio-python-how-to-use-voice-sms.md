---
title: Ses ve SMS (Python) için Twilio Nasıl Kullanılır | Microsoft Dokümanlar
description: Azure'daki Twilio API hizmetiyle nasıl telefon görüşmesi yapacağınızı ve SMS mesajı göndermeyi öğrenin. Python'da yazılmış kod örnekleri.
services: ''
documentationcenter: python
author: georgewallace
ms.assetid: 561bc75b-4ac4-40ba-bcba-48e901f27cc3
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: python
ms.topic: article
ms.date: 02/19/2015
ms.author: gwallace
ms.openlocfilehash: edbc9eef6b5f0af2e70152b66228cdf09ef31110
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "72242195"
---
# <a name="how-to-use-twilio-for-voice-and-sms-capabilities-in-python"></a>Python Ses ve SMS Yetenekleri için Twilio Nasıl Kullanılır
Bu kılavuz, Azure'daki Twilio API hizmetiyle ortak programlama görevlerinin nasıl gerçekleştirildirilebildiğini gösterir. Kapsanan senaryolar arasında telefon görüşmesi yapmak ve Kısa Mesaj Hizmeti (SMS) mesajı göndermek yer almaktadır. Twilio hakkında daha fazla bilgi ve uygulamalarınızda ses ve SMS kullanmak için [Sonraki Adımlar](#NextSteps) bölümüne bakın.

## <a name="what-is-twilio"></a><a id="WhatIs"></a>Twilio nedir?
Twilio, iş iletişiminin geleceğine güç vererek geliştiricilerin ses, VoIP ve mesajlaşmayı uygulamalara yerleştirmesini sağlıyor. Bulut tabanlı, küresel bir ortamda ihtiyaç duyulan tüm altyapıyı sanallaştırarak Twilio iletişim API platformu aracılığıyla ortaya çıkarırlar. Uygulamalar oluşturmak için basit ve ölçeklenebilir. Gittikçe öde fiyatlandırması ile esnekliğin keyfini çıkarın ve bulut güvenilirliğinden yararlanın.

**Twilio Voice,** uygulamalarınızın telefon görüşmesi yapmasına ve almasını sağlar.
**Twilio SMS,** uygulamanızın kısa mesaj gönderip almasını sağlar.
**Twilio Client** herhangi bir telefon, tablet veya tarayıcıdan VoIP aramaları yapmanızı sağlar ve WebRTC'yi destekler.

## <a name="twilio-pricing-and-special-offers"></a><a id="Pricing"></a>Twilio Fiyatlandırma ve Özel Teklifler
Azure müşterileri, Twilio Hesabınızı yükselttiğinde 10 ABD doları lık özel bir [Teklif][special_offer] alır. Bu Twilio Kredisi herhangi bir Twilio kullanımına uygulanabilir (telefon numaranızın ve mesajınızın veya arama hedefinizin konumuna bağlı olarak 1000'e kadar SMS mesajı göndermeye veya 1000'e kadar gelen Sesli dakika almaya eşdeğer 10$'lık kredi). Bu [Twilio kredinizi][special_offer] kullan ve başla.

Twilio bir öde-as-you-go hizmetidir. Kurulum ücreti yoktur ve hesabınızı istediğiniz zaman kapatabilirsiniz. Daha fazla ayrıntıyı [Twilio Fiyatlandırma][twilio_pricing]adresinde bulabilirsiniz.

## <a name="concepts"></a><a id="Concepts"></a>Kavramlar
Twilio API, uygulamalar için ses ve SMS işlevselliği sağlayan restful API'dir. İstemci kitaplıkları birden çok dilde kullanılabilir; bir liste [için, Bkz. Twilio API Kitaplıkları.][twilio_libraries]

Twilio API'nin temel yönleri Twilio fiilleri ve Twilio Markup Language (TwiML) 'dir.

### <a name="twilio-verbs"></a><a id="Verbs"></a>Twilio Fiiller
API, Twilio fiillerini kullanır; örneğin, ** &lt;Say&gt; ** fiili Twilio'ya bir çağrıda sesli bir ileti iletmesini söyler.

Aşağıda Twilio fiillerinin bir listesi vetifer. [Twilio Markup Language belgeleri][twiml]aracılığıyla diğer fiiller ve yetenekler hakkında bilgi edinin.

* Arama : Arayanı başka bir telefona bağlar. ** &lt;&gt;**
* Topla : Telefon tuş takımıüzerinde girilen sayısal rakamları toplar. ** &lt;&gt;**
* **Hangup&gt;: Bir aramayı &lt;** bitirir.
* **Duraklatma&gt;: Belirli sayıda saniye sessizce bekler. &lt;**
* Yürüt : Ses dosyalarını çalar. ** &lt;&gt;**
* Sıra : Arayanlar kuyruğuna ekleyin. ** &lt;&gt;**
* Kayıt : Arayanın sesini kaydeder ve kaydı içeren bir dosyanın URL'sini döndürür. ** &lt;&gt;**
* Yönlendirme : Bir arama veya SMS denetimini farklı bir URL'den TwiML'e aktarın. ** &lt;&gt;**
* **Reddet&gt;: Twilio numaranıza gelen bir aramayı faturalandırmadan &lt;** reddeder.
* Say : Metni, aramada yapılan konuşmaya dönüştürür. ** &lt;&gt;**
* Sms : SMS mesajı gönderir. ** &lt;&gt;**

### <a name="twiml"></a><a id="TwiML"></a>TwiML
TwiML, Twilio'ya bir çağrıveya SMS'in nasıl işlenirolduğunu bildiren Twilio fiillerine dayanan XML tabanlı talimatlar kümesidir.

Örnek olarak, aşağıdaki TwiML metni **Hello World'ü** konuşmaya dönüştürür.

    <?xml version="1.0" encoding="UTF-8" ?>
    <Response>
      <Say>Hello World</Say>
    </Response>

Uygulamanız Twilio API'yi aradığında, API parametrelerinden biri TwiML yanıtını döndüren URL'dir. Geliştirme amacıyla, uygulamalarınız tarafından kullanılan TwiML yanıtlarını sağlamak için Twilio tarafından sağlanan URL'leri kullanabilirsiniz. Ayrıca, TwiML yanıtlarını oluşturmak için kendi URL'lerinizi barındırabilirsiniz ve `TwiMLResponse` başka bir seçenek de nesneyi kullanmaktır.

Twilio fiilleri, öznitelikleri ve TwiML hakkında daha fazla bilgi için Bkz. [TwiML][twiml]. Twilio API hakkında daha fazla bilgi için [Twilio API'ye][twilio_api]bakın.

## <a name="create-a-twilio-account"></a><a id="CreateAccount"></a>Twilio Hesabı Oluşturma
Bir Twilio hesabı almaya hazır olduğunuzda, [Twilio'ya kaydolun.][try_twilio] Ücretsiz bir hesapla başlayabilir ve hesabınızı daha sonra yükseltebilirsiniz.

Bir Twilio hesabına kaydolduğunuzda, bir hesap SID ve kimlik doğrulama belirteci alırsınız. Her ikisi de Twilio API aramaları yapmak için gerekli olacaktır. Hesabınıza yetkisiz erişimi önlemek için kimlik doğrulama belirtecinizi güvende tutun. HesabınızDAKI SID ve kimlik doğrulama belirteci, sırasıyla **ACCOUNT SID** ve **AUTH TOKEN**etiketli alanlarda [Twilio][twilio_console]Konsolu'nda görüntülenebilir.

## <a name="create-a-python-application"></a><a id="create_app"></a>Python Uygulaması Oluşturma
Twilio hizmetini kullanan ve Azure'da çalışan bir Python uygulaması, Twilio hizmetini kullanan diğer Python uygulamalardan farklı değildir. Twilio hizmetleri REST tabanlı ve çeşitli şekillerde Python çağrılabilir iken, bu makalede [GitHub Python Python için Twilio kütüphane ile Twilio][twilio_python]hizmetlerinin nasıl kullanılacağı üzerinde durulacak. Python için Twilio kitaplığını kullanma hakkında [https://www.twilio.com/docs/libraries/python][twilio_lib_docs]daha fazla bilgi için bkz.

İlk olarak, [yeni Python web uygulamanız için ana bilgisayar olarak hareket etmek için yeni bir Azure Linux VM][azure_vm_setup] ayarlayın. Sanal Makine çalıştırdıktan sonra, uygulamanızı aşağıda açıklandığı gibi ortak bir bağlantı noktasında ortaya çıkarmanız gerekir.

### <a name="add-an-incoming-rule"></a>Gelen Kural Ekle
  1. [Ağ Güvenlik Grubu][azure_nsg] sayfasına gidin.
  2. Sanal Makinenize karşılık gelen Ağ Güvenlik Grubu'nu seçin.
  3. **Bağlantı noktası 80**için Ekle ve **Giden Kural.** Herhangi bir adresten gelenizin izin emin olun.

### <a name="set-the-dns-name-label"></a>DNS Ad Etiketini ayarlama
  1. [Genel IP Adresleri][azure_ips] sayfasına gidin.
  2. Sanal Makinenize karşılık gelen Genel IP'yi seçin.
  3. **Yapılandırma** **bölümündeDNS Ad Etiketini** ayarlayın. Bu örnekte bu sizin *etki alanı etiketi*.centralus.cloudapp.azure.com gibi bir şey görünecektir

SSH üzerinden Sanal Makine'ye bağlandıktan sonra seçtiğiniz Web Çerçevesini (Python'un [Flask](http://flask.pocoo.org/) ve [Django](https://www.djangoproject.com)olmasıyla en iyi bilinen iki kişi) yükleyebilirsiniz. Her ikisini de sadece komutu `pip install` çalıştırarak yükleyebilirsiniz.

Sanal Makine'yi yalnızca 80. Bu nedenle, uygulamayı bu bağlantı noktasını kullanmak üzere yapılandırdığından emin olun.

## <a name="configure-your-application-to-use-twilio-libraries"></a><a id="configure_app"></a>Uygulamanızı Twilio Kitaplıklarını Kullanacak Şekilde Yapılandırın
Uygulamanızı Python için Twilio kitaplığını kullanacak şekilde iki şekilde yapılandırabilirsiniz:

* Python için Twilio kitaplığını Pip paketi olarak yükleyin. Aşağıdaki komutlarla yüklenebilir:
   
        $ pip install twilio

    -VEYA-

* Python için Twilio kitaplığını GitHub'dan indirin ([https://github.com/twilio/twilio-python][twilio_python]) ve şu şekilde yükleyin:

        $ python setup.py install

Python için Twilio kitaplığını yükledikten sonra `import` Python dosyalarınızda bulabilirsiniz:

        import twilio

Daha fazla bilgi için [twilio_github_readme.](https://github.com/twilio/twilio-python/blob/master/README.md)

## <a name="how-to-make-an-outgoing-call"></a><a id="howto_make_call"></a>Nasıl yapılır: Giden arama yapma
Aşağıda, giden bir aramanın nasıl yapılacağını gösterir. Bu kod, Twilio İşaretdili Dili (TwiML) yanıtını döndürmek için Twilio tarafından sağlanan bir site de kullanır. Değerlerinizi **from_number** ve **to_number** telefon numaralarıyla değiştirin ve kodu çalıştırmadan önce Twilio hesabınız için **from_number** telefon numarasını doğruladığınızdan emin olun.

    from urllib.parse import urlencode

    # Import the Twilio Python Client.
    from twilio.rest import TwilioRestClient

    # Set your account ID and authentication token.
    account_sid = "your_twilio_account_sid"
    auth_token = "your_twilio_authentication_token"

    # The number of the phone initiating the call.
    # This should either be a Twilio number or a number that you've verified
    from_number = "NNNNNNNNNNN"

    # The number of the phone receiving call.
    to_number = "NNNNNNNNNNN"

    # Use the Twilio-provided site for the TwiML response.
    url = "https://twimlets.com/message?"

    # The phone message text.
    message = "Hello world."

    # Initialize the Twilio client.
    client = TwilioRestClient(account_sid, auth_token)

    # Make the call.
    call = client.calls.create(to=to_number,
                               from_=from_number,
                               url=url + urlencode({'Message': message}))
    print(call.sid)

Belirtildiği gibi, bu kod TwiML yanıtını döndürmek için Twilio tarafından sağlanan bir site kullanır. Bunun yerine TwiML yanıtı sağlamak için kendi sitenizi kullanabilirsiniz; daha fazla bilgi için, [Kendi Web Sitenizden TwiML Yanıtları Nasıl Sağlar'](#howto_provide_twiml_responses)a bakın.

## <a name="how-to-send-an-sms-message"></a><a id="howto_send_sms"></a>Nasıl yapılsın: SMS mesajı gönder
Aşağıda sınıfı kullanarak SMS iletisi `TwilioRestClient` nasıl gönderilen bir mesaj gösterilmektedir. **from_number** numarası, SMS mesajları göndermek için deneme hesapları için Twilio tarafından sağlanmaktadır. Kodu çalıştırmadan önce Twilio hesabınız için **to_number** numarası doğrulanmalıdır.

    # Import the Twilio Python Client.
    from twilio.rest import TwilioRestClient

    # Set your account ID and authentication token.
    account_sid = "your_twilio_account_sid"
    auth_token = "your_twilio_authentication_token"

    from_number = "NNNNNNNNNNN"  # With trial account, texts can only be sent from your Twilio number.
    to_number = "NNNNNNNNNNN"
    message = "Hello world."

    # Initialize the Twilio client.
    client = TwilioRestClient(account_sid, auth_token)

    # Send the SMS message.
    message = client.messages.create(to=to_number,
                                     from_=from_number,
                                     body=message)

## <a name="how-to-provide-twiml-responses-from-your-own-website"></a><a id="howto_provide_twiml_responses"></a>Nasıl?
Başvurunuz Twilio API'ye bir çağrı başlattığında, Twilio isteğinizi TwiML yanıtı döndürmesi beklenen bir URL'ye gönderir. Yukarıdaki örnekte Twilio tarafından [https://twimlets.com/message][twimlet_message_url]sağlanan URL kullanır. (TwiML Twilio tarafından kullanılmak üzere tasarlanmış olsa da, tarayıcınızda görüntüleyebilirsiniz. Örneğin, boş [https://twimlets.com/message][twimlet_message_url] `<Response>` bir öğeyi görmek için tıklatın; başka bir örnek [https://twimlets.com/message?Message%5B0%5D=Hello%20World][twimlet_message_url_hello_world] olarak, `<Response>` bir `<Say>` öğe içeren bir öğeyi görmek için tıklatın.)

Twilio tarafından sağlanan URL'ye güvenmek yerine, HTTP yanıtlarını döndüren kendi sitenizi oluşturabilirsiniz. Siteyi XML yanıtlarını döndüren herhangi bir dilde oluşturabilirsiniz; Bu konu, TwiML'i oluşturmak için Python'u kullandığınızı varsayar.

Aşağıdaki örnekler, çağrıda **Merhaba Dünya** yazan bir TwiML yanıtı çıktıracaktır.

Flask ile:

    from flask import Response
    @app.route("/")
    def hello():
        xml = '<Response><Say>Hello world.</Say></Response>'
        return Response(xml, mimetype='text/xml')

Django ile birlikte:

    from django.http import HttpResponse
    def hello(request):
        xml = '<Response><Say>Hello world.</Say></Response>'
        return HttpResponse(xml, content_type='text/xml')

Yukarıdaki örnekten de görebileceğiniz gibi, TwiML yanıtı sadece bir XML belgesidir. Python için Twilio kitaplığı, sizin için TwiML oluşturacak sınıflar içerir. Aşağıdaki örnek, yukarıda gösterildiği gibi eşdeğer yanıtı `twiml` üretir, ancak Python için Twilio kitaplığında modülü kullanır:

    from twilio import twiml

    response = twiml.Response()
    response.say("Hello world.")
    print(str(response))

TwiML hakkında daha fazla [https://www.twilio.com/docs/api/twiml][twiml_reference]bilgi için bkz.

Python uygulamanız TwiML yanıtlarını sağlamak üzere ayarlandıktan sonra, URL `client.calls.create` yönteme geçerken uygulamanın URL'sini kullanın. Örneğin, Azure barındırılan bir hizmete dağıtılan **MyTwiML** adlı bir Web uygulamanız varsa, url'sini aşağıdaki örnekte gösterildiği gibi webhook olarak kullanabilirsiniz:

    from twilio.rest import TwilioRestClient

    account_sid = "your_twilio_account_sid"
    auth_token = "your_twilio_authentication_token"
    from_number = "NNNNNNNNNNN"
    to_number = "NNNNNNNNNNN"
    url = "http://your-domain-label.centralus.cloudapp.azure.com/MyTwiML/"

    # Initialize the Twilio client.
    client = TwilioRestClient(account_sid, auth_token)

    # Make the call.
    call = client.calls.create(to=to_number,
                               from_=from_number,
                               url=url)
    print(call.sid)

## <a name="how-to-use-additional-twilio-services"></a><a id="AdditionalServices"></a>Nasıl Kullanılır: Ek Twilio Hizmetlerini Kullanın
Burada gösterilen örneklere ek olarak, Twilio Azure uygulamanızdan ek Twilio işlevselliği nden yararlanmak için kullanabileceğiniz web tabanlı API'ler sunar. Tüm ayrıntılar için [Twilio API belgelerine][twilio_api]bakın.

## <a name="next-steps"></a><a id="NextSteps"></a>Sonraki Adımlar
Artık Twilio hizmetinin temellerini öğrendiğiniz için, daha fazla bilgi edinmek için aşağıdaki bağlantıları izleyin:

* [Twilio Güvenlik Yönergeleri][twilio_security_guidelines]
* [Twilio HowTo Kılavuzları ve Örnek Kodu][twilio_howtos]
* [Twilio Quickstart Öğreticiler][twilio_quickstarts]
* [GitHub üzerinde Twilio][twilio_on_github]
* [Twilio Desteği ile konuşun][twilio_support]

[special_offer]: https://ahoy.twilio.com/azure
[twilio_python]: https://github.com/twilio/twilio-python
[twilio_lib_docs]: https://www.twilio.com/docs/libraries/python
[twilio_github_readme]: https://github.com/twilio/twilio-python/blob/master/README.md

[twimlet_message_url]: https://twimlets.com/message
[twimlet_message_url_hello_world]: https://twimlets.com/message?Message%5B0%5D=Hello%20World
[twiml_reference]: https://www.twilio.com/docs/api/twiml
[twilio_pricing]: https://www.twilio.com/pricing

[twilio_libraries]: https://www.twilio.com/docs/libraries
[twiml]: https://www.twilio.com/docs/api/twiml
[twilio_api]: https://www.twilio.com/api
[try_twilio]: https://www.twilio.com/try-twilio
[twilio_console]:  https://www.twilio.com/console
[twilio_security_guidelines]: https://www.twilio.com/docs/security
[twilio_howtos]: https://www.twilio.com/docs/howto
[twilio_on_github]: https://github.com/twilio
[twilio_support]: https://www.twilio.com/help/contact
[twilio_quickstarts]: https://www.twilio.com/docs/quickstart
