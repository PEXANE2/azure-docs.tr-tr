---
title: Nasıl Twilio (PHP) | Microsoft Dokümanlar
description: Azure'daki Twilio API hizmetiyle nasıl telefon görüşmesi yapacağınızı ve SMS mesajı göndermeyi öğrenin. Örnekler PHP uygulaması içindir.
documentationcenter: php
services: ''
author: georgewallace
ms.assetid: 44e35adc-be06-4700-beee-8c9e2c20c540
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: PHP
ms.topic: article
ms.date: 11/25/2014
ms.author: gwallace
ms.openlocfilehash: fb1623c4a409f1c6cba94bad56d773e166d2b182
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "69637316"
---
# <a name="how-to-make-a-phone-call-using-twilio-in-a-php-application-on-azure"></a>Azure'daki PHP Uygulamasında Twilio Kullanarak Telefon Görüşmesi Nasıl Yapılır?
Aşağıdaki örnek, Azure'da barındırılan bir PHP web sayfasından arama yapmak için Twilio'yu nasıl kullanabileceğinizi gösterir. Ortaya çıkan uygulama, aşağıdaki ekran görüntüsünde gösterildiği gibi, kullanıcıdan telefon görüşmesi değerleri ister.

![Twilio ve PHP Kullanarak Azure Çağrı Formu][twilio_php]

Bu konuda kodu kullanmak için aşağıdakileri yapmanız gerekir:

1. Twilio [Konsolunuzdan][twilio_console]bir Twilio hesabı ve kimlik doğrulama belirteci edinin. Twilio ile başlamak için, [https://www.twilio.com/pricing][twilio_pricing]fiyatlandırmayı . Bir deneme hesabına [https://www.twilio.com/try-twilio][try_twilio]kaydolabilirsiniz.
2. PHP [için Twilio kitaplığını](https://github.com/twilio/twilio-php) edinin veya ARMUT paketi olarak yükleyin. Daha fazla bilgi için [okuma dosyasına](https://github.com/twilio/twilio-php/blob/master/README.md)bakın.
3. PHP için Azure SDK'sını yükleyin. 
<!-- For an overview of the SDK and instructions on installing it, see [Set up the Azure SDK for PHP](app-service-web/web-sites-php-mysql-deploy-use-git.md) -->

## <a name="create-a-web-form-for-making-a-call"></a>Arama yapmak için web formu oluşturma
Aşağıdaki HTML kodu, arama yapmak için kullanıcı verilerini alan bir web sayfasının **(callform.html)** nasıl oluşturulabildiğini gösterir:

```html
<!DOCTYPE html>
<html>
<head>
  <title>Automated call form</title>
</head>
<body>
  <h1>Automated Call Form</h1>
  <p>Fill in all fields and click <b>Make this call</b>.</p>
  <form action="makecall.php" method="post">
    <table>
      <tr>
        <td>To:</td>
        <td><input name="callTo" size="50" type="text" value=""></td>
      </tr>
      <tr>
        <td>From:</td>
        <td><input name="callFrom" size="50" type="text" value=""></td>
      </tr>
      <tr>
        <td>Call message:</td>
        <td><input name="callText" size="100" type="text" value="Hello. This is the call text. Good bye."></td>
      </tr>
      <tr>
        <td colspan="2"><input type="submit" value="Make this call"></td>
      </tr>
    </table>
  </form><br>
</body>
</html>
```

## <a name="create-the-code-to-make-the-call"></a>Arama yapmak için kodu oluşturma
Aşağıdaki kod **makecall.php**oluşturmak için nasıl gösterir , kullanıcı **callform.html**tarafından görüntülenen formu gönderdiğinde denir. Aşağıda gösterilen kod çağrı iletisini oluşturur ve aramayı oluşturur. Ayrıca, aşağıdaki kodda **$sid** ve **$token** atanan yer tutucu değerleri yerine [Twilio Konsolundan Twilio][twilio_console] hesabınızı ve kimlik doğrulama belirtecinizi kullandığınızdan emin olun.

```html
<html>
<head><title>Making call...</title></head>
<body>
<p>Your call is being made.</p>

<?php
require_once 'path/to/vendor/autoload.php';

$sid   = "your_account_sid";
$token = "your_authentication_token";

$from_number = $_POST['callFrom']; // Calls must be made from a registered Twilio number.
$to_number   = $_POST['callTo'];
$message     = $_POST['callText'];

$client = new Twilio\Rest\Client($sid, $token);

$call = $client->calls->create(
            $to_number,
            $from_number,
            array('url' => https://twimlets.com/message?Message=' . urlencode($message))
        );

echo "Call status: " . $call->status . "<br />";
echo "URI resource: " . $call->uri . "<br />";
?>
</body>
</html>
```

Arama yapmanın yanı sıra, **makecall.php** aşağıdaki resimde gösterildiği gibi bazı çağrı meta verileri görüntüler. Arama meta verileri hakkında daha [https://www.twilio.com/docs/api/rest/call#instance-properties][twilio_call_properties]fazla bilgi için bkz.

![Twilio ve PHP kullanarak Azure Çağrı Yanıtı][twilio_php_response]

## <a name="run-the-application"></a>Uygulamayı çalıştırma
Bir sonraki adım, [uygulamanızı Git ile Azure Web Apps'a dağıtmaktır](app-service/app-service-web-get-started-php.md) (ancak tüm bilgiler alakalı değildir). 

## <a name="next-steps"></a>Sonraki adımlar
Bu kod, Azure'da PHP'de Twilio kullanarak temel işlevleri göstermek için sağlanmıştır. Üretimde Azure'a dağıtılamadan önce, daha fazla hata işleme veya diğer özellikler eklemek isteyebilirsiniz. Örnek:

* Web formu kullanmak yerine, telefon numaralarını ve arama metinlerini depolamak için Azure depolama bloblarını veya SQL Veritabanı'nı kullanabilirsiniz. PHP'de Azure depolama bloblarını kullanma hakkında daha fazla bilgi [için][howto_blob_storage_php]bkz. PHP'de SQL Veritabanı'nı kullanma hakkında bilgi [için][howto_sql_azure_php]bkz.
* **Makecall.php** kodu, Twilio'ya[https://twimlets.com/message][twimlet_message_url]çağrıyı nasıl devam edebildiğini bildiren bir Twilio İşaretdili Dili (TwiML) yanıtı sağlamak için Twilio tarafından sağlanan URL'yi () kullanır. Örneğin, döndürülen TwiML, arama `<Say>` alıcısına konuşulan metinle sonuçlanan bir fiil içerebilir. Twilio tarafından sağlanan URL'yi kullanmak yerine, Twilio'nun isteğine yanıt vermek için kendi hizmetinizi oluşturabilirsiniz; daha fazla bilgi için [PHP'de Ses ve SMS Özellikleri için Twilio Nasıl Kullanılır' a][howto_twilio_voice_sms_php]bakın. TwiML hakkında daha fazla [https://www.twilio.com/docs/api/twiml][twiml]bilgi bulunabilir , `<Say>` ve hakkında daha fazla bilgi ve [https://www.twilio.com/docs/api/twiml/say][twilio_say]diğer Twilio fiiller bulunabilir .
* Twilio güvenlik yönergelerini [https://www.twilio.com/docs/security][twilio_docs_security]okuyun.

Twilio hakkında daha fazla [https://www.twilio.com/docs][twilio_docs]bilgi için bkz.

## <a name="see-also"></a>Ayrıca Bkz.
* [PHP'de Ses ve SMS Özellikleri için Twilio Nasıl Kullanılır?](partner-twilio-php-how-to-use-voice-sms.md)

[twilio_console]: https://www.twilio.com/console
[twilio_pricing]: https://www.twilio.com/pricing
[try_twilio]: https://www.twilio.com/try-twilio
[twilio_api]: https://www.twilio.com/docs/api
[verify_phone]: https://www.twilio.com/console/phone-numbers/verified
[twimlet_message_url]: https://twimlets.com/message
[twiml]: https://www.twilio.com/docs/api/twiml
[twilio_api_service]: https://api.twilio.com
[build_php_azure_app]: http://azurephp.interoperabilitybridges.com/articles/build-and-deploy-a-windows-azure-php-application
[howto_twilio_voice_sms_php]: partner-twilio-php-how-to-use-voice-sms.md
[howto_blob_storage_php]: https://azure.microsoft.com/documentation/articles/storage-php-how-to-use-blobs/
[howto_sql_azure_php]: https://azure.microsoft.com/documentation/articles/sql-database-php-how-to-use/
[twilio_call_properties]: https://www.twilio.com/docs/api/rest/call#instance-properties
[twilio_docs_security]: https://www.twilio.com/docs/security
[twilio_docs]: https://www.twilio.com/docs
[twilio_say]: https://www.twilio.com/docs/api/twiml/say
[ssl_validation]: http://readthedocs.org/docs/twilio-php/en/latest/usage/rest.html
[twilio_php]: ./media/partner-twilio-php-make-phone-call/WA_TwilioPHPCallForm.jpg
[twilio_php_response]: ./media/partner-twilio-php-make-phone-call/WA_TwilioPHPMakeCall.jpg
[twilio_php_github]: https://github.com/twilio/twilio-php
