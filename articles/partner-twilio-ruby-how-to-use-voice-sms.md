---
title: Ses ve SMS (Ruby) için Twilio Nasıl Kullanılır | Microsoft Dokümanlar
description: Azure'daki Twilio API hizmetiyle nasıl telefon görüşmesi yapacağınızı ve SMS mesajı göndermeyi öğrenin. Ruby'de yazılmış kod örnekleri.
services: ''
documentationcenter: ruby
author: georgewallace
ms.assetid: 60e512f6-fa47-47c0-aedc-f19bb72a1158
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: ruby
ms.topic: article
ms.date: 11/25/2014
ms.author: gwallace
ms.openlocfilehash: 4822e6feb29f5a17c653a60937b895ec584e0ee4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "69637194"
---
# <a name="how-to-use-twilio-for-voice-and-sms-capabilities-in-ruby"></a>Ruby Ses ve SMS Yetenekleri için Twilio Nasıl Kullanılır
Bu kılavuz, Azure'daki Twilio API hizmetiyle ortak programlama görevlerinin nasıl gerçekleştirildirilebildiğini gösterir. Kapsanan senaryolar arasında telefon görüşmesi yapmak ve Kısa Mesaj Hizmeti (SMS) mesajı göndermek yer almaktadır. Twilio hakkında daha fazla bilgi ve uygulamalarınızda ses ve SMS kullanmak için [Sonraki Adımlar](#NextSteps) bölümüne bakın.

## <a name="what-is-twilio"></a><a id="WhatIs"></a>Twilio nedir?
Twilio, ses ve SMS uygulamaları oluşturmak için mevcut web dillerinizi ve becerilerinizi kullanmanıza olanak tanıyan bir telefon web hizmeti API'sidir. Twilio bir üçüncü taraf hizmetidir (Azure özelliği değil, Microsoft ürünü değildir).

**Twilio Voice,** uygulamalarınızın telefon görüşmesi yapmasına ve almasını sağlar. **Twilio SMS,** uygulamalarınızın SMS mesajları yapmasına ve almasını sağlar. **Twilio Client,** uygulamalarınızın mobil bağlantılar da dahil olmak üzere mevcut Internet bağlantılarını kullanarak sesli iletişimi etkinleştirmesine olanak tanır.

## <a name="twilio-pricing-and-special-offers"></a><a id="Pricing"></a>Twilio Fiyatlandırma ve Özel Teklifler
Twilio fiyatlandırması hakkında bilgi [Twilio Fiyatlandırma][twilio_pricing]mevcuttur. Azure müşterileri [özel][special_offer]bir teklif alır: 1000 metin veya 1000 gelen dakikalık ücretsiz kredi. Bu teklife kaydolmak veya daha fazla [https://ahoy.twilio.com/azure][special_offer]bilgi almak için lütfen .  

## <a name="concepts"></a><a id="Concepts"></a>Kavramlar
Twilio API, uygulamalar için ses ve SMS işlevselliği sağlayan restful API'dir. İstemci kitaplıkları birden çok dilde kullanılabilir; bir liste [için, Bkz. Twilio API Kitaplıkları.][twilio_libraries]

### <a name="twiml"></a><a id="TwiML"></a>TwiML
TwiML, Twilio'ya bir çağrıveya SMS'in nasıl işlenirileceği konusunda bilgi veren XML tabanlı talimatlar kümesidir.

Örnek olarak, aşağıdaki TwiML metni **Hello World'ü** konuşmaya dönüştürür.

    <?xml version="1.0" encoding="UTF-8" ?>
    <Response>
       <Say>Hello World</Say>
    </Response>

Tüm TwiML `<Response>` belgeleri kök öğesi olarak var. Buradan, uygulamanızın davranışını tanımlamak için Twilio Verbs'i kullanırsınız.

### <a name="twiml-verbs"></a><a id="Verbs"></a>TwiML Fiiller
Twilio Verbs, Twilio'ya ne **yapması**gerektiğini söyleyen XML etiketleridir. Örneğin, ** &lt;Say&gt; ** fiili Twilio'ya bir çağrıda sesli bir ileti iletmesini söyler. 

Aşağıda Twilio fiillerinin bir listesi vetifer.

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

Twilio fiilleri, öznitelikleri ve TwiML hakkında daha fazla bilgi için Bkz. [TwiML][twiml]. Twilio API hakkında daha fazla bilgi için [Twilio API'ye][twilio_api]bakın.

## <a name="create-a-twilio-account"></a><a id="CreateAccount"></a>Twilio Hesabı Oluşturma
Bir Twilio hesabı almaya hazır olduğunuzda, [Try Twilio'ya][try_twilio]kaydolun. Ücretsiz bir hesapla başlayabilir ve hesabınızı daha sonra yükseltebilirsiniz.

Bir Twilio hesabına kaydolduğunuzda, başvurunuz için ücretsiz bir telefon numarası alırsınız. Ayrıca bir hesap SID ve auth belirteci alırsınız. Her ikisi de Twilio API aramaları yapmak için gerekli olacaktır. Hesabınıza yetkisiz erişimi önlemek için kimlik doğrulama belirtecinizi güvende tutun. Hesabınız SID ve auth belirteci, sırasıyla **ACCOUNT SID** ve **AUTH TOKEN**etiketli alanlarda [Twilio hesap sayfasında][twilio_account]görüntülenebilir.

### <a name="verify-phone-numbers"></a><a id="VerifyPhoneNumbers"></a>Telefon Numaralarını Doğrula
Twilio tarafından verilen numaraya ek olarak, uygulamalarınızda kullanmak için kontrol ettiğiniz numaraları (örn. cep telefonu veya ev telefonu numaranız) da doğrulayabilirsiniz. 

Telefon numarasını nasıl doğrulayınız hakkında bilgi için [Bkz.][verify_phone]

## <a name="create-a-ruby-application"></a><a id="create_app"></a>Ruby Uygulaması Oluşturma
Twilio hizmetini kullanan ve Azure'da çalışan bir Ruby uygulaması, Twilio hizmetini kullanan diğer Ruby uygulamalardan farklı değildir. Twilio hizmetleri restful ve çeşitli şekillerde Ruby çağrılabilir iken, bu makalede Ruby için [Twilio yardımcı kütüphane ile Twilio][twilio_ruby]hizmetleri nasıl kullanılacağı üzerinde durulacak.

İlk olarak, yeni Ruby web uygulamanız için ana bilgisayar olarak hareket edecek yeni bir [Azure Linux VM ayarlayın.][azure_vm_setup] Bir Rails uygulamasının oluşturulmasıyla ilgili adımları yoksay, VM'yi ayarlamanın yeterlidir. Harici bağlantı noktası 80 ve 5000 dahili bağlantı noktası içeren bir Bitiş Noktası oluşturduğunuzdan emin olun.

Aşağıdaki örneklerde, [Biz Sinatra][sinatra]kullanarak olacak , Ruby için çok basit bir web çerçevesi. Ama kesinlikle Ruby rails Ruby de dahil olmak üzere başka bir web çerçevesi ile Ruby için Twilio yardımcı kütüphane kullanabilirsiniz.

Yeni VM'inize SSH ve yeni uygulamanız için bir dizin oluşturun. Bu dizinin içinde, Gemfile adlı bir dosya oluşturun ve aşağıdaki kodu kopyalayın:

    source 'https://rubygems.org'
    gem 'sinatra'
    gem 'thin'

Komut satırında `bundle install`çalıştırın. Bu yukarıdaki bağımlılıkları yükler. Sonraki adlı `web.rb`bir dosya oluşturmak. Web uygulamanızın kodunun bulunduğu yer burası olacaktır. Aşağıdaki kodu yapıştırın:

    require 'sinatra'

    get '/' do
        "Hello Monkey!"
    end

Bu noktada komutu `ruby web.rb -p 5000`çalıştırmak gerekir. Bu, 5000 portundaki küçük bir web sunucusunun dönüşünü sağlayacaktır. Azure VM'niz için ayarladığınız URL'yi ziyaret ederek tarayıcınızda bu uygulamaya göz atabilmelisiniz. Web uygulamanıza tarayıcıdan erişebildiğinizde, bir Twilio uygulaması oluşturmaya başlamaya hazırsınız.

## <a name="configure-your-application-to-use-twilio"></a><a id="configure_app"></a>Uygulamanızı Twilio Kullanacak Şekilde Yapılandırın
Web uygulamanızı twilio kitaplığını kullanacak şekilde yapılandırarak bu `Gemfile` satırı da içerecek şekilde güncelleyebilirsiniz:

    gem 'twilio-ruby'

Komut satırında, `bundle install`çalıştırın. Şimdi `web.rb` açık ve üst kısmında bu satırı da dahil olmak üzere:

    require 'twilio-ruby'

Artık web uygulamanızda Ruby için Twilio yardımcı kitaplığını kullanmaya hazırsınız.

## <a name="how-to-make-an-outgoing-call"></a><a id="howto_make_call"></a>Nasıl yapılır: Giden arama yapma
Aşağıda, giden bir aramanın nasıl yapılacağını gösterir. Anahtar kavramlar arasında REST API aramaları yapmak için Ruby için Twilio yardımcı kitaplığını kullanmak ve TwiML'i işlemek yer almaktadır. **Değerlerinizi, Gönderen** ve **To** telefon numaralarıyla değiştirin ve kodu çalıştırmadan önce Twilio hesabınız için **Telefon** numaranızı doğruladığınızdan emin olun.

Bu işlevi `web.md`ekleyin:

    # Set your account ID and authentication token.
    sid = "your_twilio_account_sid";
    token = "your_twilio_authentication_token";

    # The number of the phone initiating the call.
    # This should either be a Twilio number or a number that you've verified
    from = "NNNNNNNNNNN";

    # The number of the phone receiving call.
    to = "NNNNNNNNNNN";

    # Use the Twilio-provided site for the TwiML response.
    url = "http://yourdomain.cloudapp.net/voice_url";

    get '/make_call' do
      # Create the call client.
      client = Twilio::REST::Client.new(sid, token);

      # Make the call
      client.account.calls.create(to: to, from: from, url: url)
    end

    post '/voice_url' do
      "<Response>
         <Say>Hello Monkey!</Say>
       </Response>"
    end

Bir `http://yourdomain.cloudapp.net/make_call` tarayıcıda açılırsanız, bu aramayı yapmak için Twilio API'ye çağrıyı tetikler. İlk iki parametre oldukça kendini `client.account.calls.create` açıklayıcıdır: aramanın `from` sayısı ve aramanın `to`numaradır. 

Üçüncü parametre`url`( ) Twilio'nun arama bağlandıktan sonra ne yapacağına dair talimat almak istediği URL'dir. Bu durumda, basit bir TwiML belgesini döndüren ve`http://yourdomain.cloudapp.net` `<Say>` fiili metinden konuşmaya yapmak ve çağrıyı alan kişiye "Merhaba Maymun" demek için kullanan bir URL () ayarlıyoruz.

## <a name="how-to-receive-an-sms-message"></a><a id="howto_receive_sms"></a>Nasıl yapılsın: SMS mesajı alma
Önceki örnekte **giden** bir telefon görüşmesi başlattık. Bu sefer, Twilio'nun **gelen** SMS mesajını işlemek için kayıt sırasında bize verdiği telefon numarasını kullanalım.

İlk olarak, [Twilio panonuza][twilio_account]giriş yapın. Üst teki "Sayılar"a tıklayın ve ardından size sağlanan Twilio numarasına tıklayın. Yapılandırabileceğiniz iki URL görürsünüz. Sesli İstek URL'si ve SMS İstek URL'si. Bunlar, Twilio'nun telefon görüşmesi yapıldığında veya numaranıza SMS gönderildiğinde aradığı URL'lerdir. URL'ler "web kancaları" olarak da bilinir.

Gelen SMS mesajlarını işlemek istiyoruz, bu nedenle URL'yi `http://yourdomain.cloudapp.net/sms_url`'' ile güncelleyelim. Devam edin ve sayfanın altındaki Değişiklikleri Kaydet'i tıklatın. Şimdi, geri `web.rb` bu işlemek için uygulama programı yapalım:

    post '/sms_url' do
      "<Response>
         <Message>Hey, thanks for the ping! Twilio and Azure rock!</Message>
       </Response>"
    end

Değişikliği yaptıktan sonra, web uygulamanızı yeniden başlattığınızdan emin olun. Şimdi, telefonunuzu çıkar ve Twilio numaranıza bir SMS gönderin. Hemen "Hey, ping için teşekkürler! Twilio ve Azure rock!".

## <a name="how-to-use-additional-twilio-services"></a><a id="additional_services"></a>Nasıl Kullanılır: Ek Twilio Hizmetlerini Kullanın
Burada gösterilen örneklere ek olarak, Twilio Azure uygulamanızdan ek Twilio işlevselliği nden yararlanmak için kullanabileceğiniz web tabanlı API'ler sunar. Tüm ayrıntılar için [Twilio API belgelerine][twilio_api_documentation]bakın.

### <a name="next-steps"></a><a id="NextSteps"></a>Sonraki Adımlar
Artık Twilio hizmetinin temellerini öğrendiğiniz için, daha fazla bilgi edinmek için aşağıdaki bağlantıları izleyin:

* [Twilio Güvenlik Yönergeleri][twilio_security_guidelines]
* [Twilio HowTos ve Örnek Kodu][twilio_howtos]
* [Twilio Quickstart Öğreticiler][twilio_quickstarts] 
* [GitHub üzerinde Twilio][twilio_on_github]
* [Twilio Desteği ile konuşun][twilio_support]

[twilio_ruby]: https://www.twilio.com/docs/ruby/install





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
[sinatra]: http://www.sinatrarb.com/
[azure_vm_setup]: https://docs.microsoft.com/azure/virtual-machines/linux/classic/ruby-rails-web-app
