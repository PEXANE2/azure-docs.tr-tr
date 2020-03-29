---
title: SendGrid e-posta hizmeti (PHP) nasıl kullanılır | Microsoft Dokümanlar
description: Azure'daki SendGrid e-posta hizmetiyle nasıl e-posta gönderebildiğini öğrenin. PHP ile yazılmış kod örnekleri.
documentationcenter: php
services: ''
manager: sendgrid
editor: mollybos
author: thinkingserious
ms.assetid: 51a9928a-4c9e-4b0a-aca8-9a408aeb6f47
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: PHP
ms.topic: article
ms.date: 10/30/2014
ms.author: erikre
ms.reviewer: elmer.thomas@sendgrid.com; erika.berkland@sendgrid.com; vibhork; matt.bernier@sendgrid.com
ms.openlocfilehash: b3a9fee09d1eac6fb4d716af83c348cb2c21f7a9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "67870905"
---
# <a name="how-to-use-the-sendgrid-email-service-from-php"></a>PHP'den SendGrid E-posta Hizmeti Nasıl Kullanılır?

Bu kılavuz, Azure'daki SendGrid e-posta hizmetiyle ortak programlama görevlerinin nasıl gerçekleştirililebildiğini gösterir. Örnekler PHP ile yazılmıştır.
Kapsanan senaryolar arasında **e-posta oluşturma,** **e-posta gönderme**ve **ek ekleme**yer almaktadır. SendGrid ve e-posta gönderme hakkında daha fazla bilgi için [Sonraki Adımlar](#next-steps) bölümüne bakın.

## <a name="what-is-the-sendgrid-email-service"></a>SendGrid E-posta Hizmeti nedir?
SendGrid, özel entegrasyonu kolaylaştıran esnek API'lerin yanı sıra güvenilir [işlemsel e-posta teslimi,]ölçeklenebilirlik ve gerçek zamanlı analiz sağlayan bulut tabanlı bir [e-posta hizmetidir.] Sık kullanılan SendGrid kullanım senaryoları şunlardır:

* Müşterilere otomatik olarak makbuz gönderme
* Müşterilere aylık e-el ilanları ve özel teklifler göndermek için dağıtım listeleri nin yönetilmesi
* Engellenen e-posta ve müşteri yanıt verme gibi şeyler için gerçek zamanlı ölçümler toplama
* Eğilimleri belirlemeye yardımcı olacak raporlar oluşturma
* Müşteri sorgularını iletme
* Uygulamanızdan gelen e-posta bildirimleri

Daha fazla bilgi [https://sendgrid.com][https://sendgrid.com]için bkz.

## <a name="create-a-sendgrid-account"></a>SendGrid Hesabı Oluşturma

[!INCLUDE [sendgrid-sign-up](../includes/sendgrid-sign-up.md)]

## <a name="using-sendgrid-from-your-php-application"></a>PHP Uygulamanızdan SendGrid'i Kullanma

Bir Azure PHP uygulamasında SendGrid'i kullanmak için özel yapılandırma veya kodlama gerektirmez. SendGrid bir hizmet olduğundan, bir bulut uygulamasından şirket içi bir uygulamadan erişilebildiği şekilde erişilebilir.

## <a name="how-to-send-an-email"></a>Nasıl Yapılsın: E-posta Gönder

SMTP veya SendGrid tarafından sağlanan Web API'sini kullanarak e-posta gönderebilirsiniz.

### <a name="smtp-api"></a>SMTP API

SendGrid SMTP API'yi kullanarak e-posta göndermek için, PHP uygulamalarından e-posta göndermek için bileşen tabanlı bir kitaplık olan *Swift Mailer'ı*kullanın. [Swift Mailer kitaplığı](https://swiftmailer.symfony.com/) v5.3.0 'ı indirebilirsiniz (Swift Mailer'ı yüklemek için [Composer'ı] kullanın). `Swift\_SmtpTransport`Kitaplıkla e-posta göndermek, ,, `Swift\_Mailer` `Swift\_Message` ve sınıflar örnekleri oluşturmayı, `Swift\_Mailer::send` uygun özellikleri ayarlamayı ve yöntemi çağırmayı içerir.

```php
<?php
 include_once "vendor/autoload.php";
 /*
  * Create the body of the message (a plain-text and an HTML version).
  * $text is your plain-text email
  * $html is your html version of the email
  * If the receiver is able to view html emails then only the html
  * email will be displayed
  */
 $text = "Hi!\nHow are you?\n";
 $html = "<html>
       <head></head>
       <body>
           <p>Hi!<br>
               How are you?<br>
           </p>
       </body>
       </html>";
 // This is your From email address
 $from = array('someone@example.com' => 'Name To Appear');
 // Email recipients
 $to = array(
       'john@contoso.com'=>'Destination 1 Name',
       'anna@contoso.com'=>'Destination 2 Name'
 );
 // Email subject
 $subject = 'Example PHP Email';

 // Login credentials
 $username = 'yoursendgridusername';
 $password = 'yourpassword';

 // Setup Swift mailer parameters
 $transport = Swift_SmtpTransport::newInstance('smtp.sendgrid.net', 587);
 $transport->setUsername($username);
 $transport->setPassword($password);
 $swift = Swift_Mailer::newInstance($transport);

 // Create a message (subject)
 $message = new Swift_Message($subject);

 // attach the body of the email
 $message->setFrom($from);
 $message->setBody($html, 'text/html');
 $message->setTo($to);
 $message->addPart($text, 'text/plain');

 // send message
 if ($recipients = $swift->send($message, $failures))
 {
     // This will let us know how many users received this message
     echo 'Message sent out to '.$recipients.' users';
 }
 // something went wrong =(
 else
 {
     echo "Something went wrong - ";
     print_r($failures);
 }
```

### <a name="web-api"></a>Web API
SendGrid Web API'sini kullanarak e-posta göndermek için PHP'nin [kıvırma işlevini][curl function] kullanın.

```php
<?php

 $url = 'https://api.sendgrid.com/';
 $user = 'USERNAME';
 $pass = 'PASSWORD';

 $params = array(
      'api_user' => $user,
      'api_key' => $pass,
      'to' => 'john@contoso.com',
      'subject' => 'testing from curl',
      'html' => 'testing body',
      'text' => 'testing body',
      'from' => 'anna@contoso.com',
   );

 $request = $url.'api/mail.send.json';

 // Generate curl request
 $session = curl_init($request);

 // Tell curl to use HTTP POST
 curl_setopt ($session, CURLOPT_POST, true);

 // Tell curl that this is the body of the POST
 curl_setopt ($session, CURLOPT_POSTFIELDS, $params);

 // Tell curl not to return headers, but do return the response
 curl_setopt($session, CURLOPT_HEADER, false);
 curl_setopt($session, CURLOPT_RETURNTRANSFER, true);

 // obtain response
 $response = curl_exec($session);
 curl_close($session);

 // print everything out
 print_r($response);
```

SendGrid'in Web API'si REST API'ye çok benzer, ancak çoğu çağrıda hem GET hem de POST fiilleri birbirinin yerine kullanılabildiğinden, gerçekten güvenilir bir API değildir.

## <a name="how-to-add-an-attachment"></a>Nasıl Yapılır: Ek Ekleme

### <a name="smtp-api"></a>SMTP API

SMTP API kullanarak bir eki göndermek, Swift Mailer ile bir e-posta göndermek için örnek komut dosyasına ek bir kod satırı içerir.

```php
<?php
 include_once "vendor/autoload.php";
 /*
  * Create the body of the message (a plain-text and an HTML version).
  * $text is your plain-text email
  * $html is your html version of the email
  * If the receiver is able to view html emails then only the html
  * email will be displayed
  */
 $text = "Hi!\nHow are you?\n";
  $html = "<html>
      <head></head>
      <body>
         <p>Hi!<br>
            How are you?<br>
         </p>
      </body>
      </html>";

 // This is your From email address
 $from = array('someone@example.com' => 'Name To Appear');

 // Email recipients
 $to = array(
      'john@contoso.com'=>'Destination 1 Name',
      'anna@contoso.com'=>'Destination 2 Name'
 );
 // Email subject
 $subject = 'Example PHP Email';

 // Login credentials
 $username = 'yoursendgridusername';
 $password = 'yourpassword';

 // Setup Swift mailer parameters
 $transport = Swift_SmtpTransport::newInstance('smtp.sendgrid.net', 587);
 $transport->setUsername($username);
 $transport->setPassword($password);
 $swift = Swift_Mailer::newInstance($transport);

 // Create a message (subject)
 $message = new Swift_Message($subject);

 // attach the body of the email
 $message->setFrom($from);
 $message->setBody($html, 'text/html');
 $message->setTo($to);
 $message->addPart($text, 'text/plain');
 $message->attach(Swift_Attachment::fromPath("path\to\file")->setFileName("file_name"));

 // send message
 if ($recipients = $swift->send($message, $failures))
 {
      // This will let us know how many users received this message
      echo 'Message sent out to '.$recipients.' users';
 }
 // something went wrong =(
 else
 {
      echo "Something went wrong - ";
      print_r($failures);
 }
```

Ek kod satırı aşağıdaki gibidir:

```php
 $message->attach(Swift_Attachment::fromPath("path\to\file")->setFileName('file_name'));
```

Bu kod satırı nesneüzerinde ekleme `Swift\_Message` yöntemiçağırır ve `fromPath` bir `Swift\_Attachment` iletiyi almak ve eklemek için sınıfta statik yöntemi kullanır.

### <a name="web-api"></a>Web API

Web API'sini kullanarak ek göndermek, Web API'sını kullanarak e-posta göndermeye çok benzer. Ancak, aşağıdaki örnekte parametre dizisinin bu öğeyi içermesi gerektiğini unutmayın:

```php
    'files['.$fileName.']' => '@'.$filePath.'/'.$fileName
```

#### <a name="example"></a>Örnek

```php
<?php

 $url = 'https://api.sendgrid.com/';
 $user = 'USERNAME';
 $pass = 'PASSWORD';

 $fileName = 'myfile';
 $filePath = dirname(__FILE__);

 $params = array(
     'api_user' => $user,
     'api_key' => $pass,
     'to' =>'john@contoso.com',
     'subject' => 'test of file sends',
     'html' => '<p> the HTML </p>',
     'text' => 'the plain text',
     'from' => 'anna@contoso.com',
     'files['.$fileName.']' => '@'.$filePath.'/'.$fileName
 );

 print_r($params);

 $request = $url.'api/mail.send.json';

 // Generate curl request
 $session = curl_init($request);

 // Tell curl to use HTTP POST
 curl_setopt ($session, CURLOPT_POST, true);

 // Tell curl that this is the body of the POST
 curl_setopt ($session, CURLOPT_POSTFIELDS, $params);

 // Tell curl not to return headers, but do return the response
 curl_setopt($session, CURLOPT_HEADER, false);
 curl_setopt($session, CURLOPT_RETURNTRANSFER, true);

 // obtain response
 $response = curl_exec($session);
 curl_close($session);

 // print everything out
 print_r($response);
```

## <a name="how-to-use-filters-to-enable-footers-tracking-and-analytics"></a>Nasıl Kullanılır: Altlayıcıları, İzlemeyi ve Analizleri Etkinleştirmek için Filtreleri Kullanın

SendGrid *filtreleri*kullanarak ek e-posta işlevselliği sağlar. Bunlar, tıklama izlemeyi etkinleştirme, Google analitiği, abonelik izleme gibi belirli işlevleri etkinleştirmek için bir e-posta iletisine eklenebilecek ayarlardır.

Filtreler, filtreler özelliği ni kullanarak iletiye uygulanabilir. Her filtre, filtreye özgü ayarlar içeren bir karma ile belirtilir. Aşağıdaki örnek, altbilgi filtresini etkinleştirive e-posta iletisinin altına eklenecek bir kısa mesaj belirtir. Bu örnekte [sendgrid-php kitaplığını]kullanacağız.

Kitaplığı yüklemek için [Besteci'yi] kullanın:

```bash
php composer.phar require sendgrid/sendgrid 2.1.1
```

### <a name="example"></a>Örnek  

```php
<?php
 /*
  * This example is used for sendgrid-php V2.1.1 (https://github.com/sendgrid/sendgrid-php/tree/v2.1.1)
  */
 include "vendor/autoload.php";

 $email = new SendGrid\Email();
 // The list of addresses this message will be sent to
 // [This list is used for sending multiple emails using just ONE request to SendGrid]
 $toList = array('john@contoso.com', 'anna@contoso.com');

 // Specify the names of the recipients
 $nameList = array('Name 1', 'Name 2');

 // Used as an example of variable substitution
 $timeList = array('4 PM', '5 PM');

 // Set all of the above variables
 $email->setTos($toList);
 $email->addSubstitution('-name-', $nameList);
 $email->addSubstitution('-time-', $timeList);

 // Specify that this is an initial contact message
 $email->addCategory("initial");

 // You can optionally setup individual filters here, in this example, we have
 // enabled the footer filter
 $email->addFilter('footer', 'enable', 1);
 $email->addFilter('footer', "text/plain", "Thank you for your business");
 $email->addFilter('footer', "text/html", "Thank you for your business");

 // The subject of your email
 $subject = 'Example SendGrid Email';

 // Where is this message coming from. For example, this message can be from
 // support@yourcompany.com, info@yourcompany.com
 $from = 'someone@example.com';

 // If you do not specify a sender list above, you can specify the user here. If
 // a sender list IS specified above, this email address becomes irrelevant.
 $to = 'john@contoso.com';

 # Create the body of the message (a plain-text and an HTML version).
 # text is your plain-text email
 # html is your html version of the email
 # if the receiver is able to view html emails then only the html
 # email will be displayed

 /*
  * Note the variable substitution here =)
  */
 $text = "
 Hello -name-,
 Thank you for your interest in our products. We have set up an appointment to call you at -time- EST to discuss your needs in more detail.
 Regards,
 Fred";

 $html = "
 <html>
 <head></head>
 <body>
 <p>Hello -name-,<br>
 Thank you for your interest in our products. We have set up an appointment
 to call you at -time- EST to discuss your needs in more detail.

 Regards,

 Fred<br>
 </p>
 </body>
 </html>";

 // set subject
 $email->setSubject($subject);

 // attach the body of the email
 $email->setFrom($from);
 $email->setHtml($html);
 $email->addTo($to);
 $email->setText($text);

 // Your SendGrid account credentials
 $username = 'sendgridusername@yourdomain.com';
 $password = 'example';

 // Create SendGrid object
 $sendgrid = new SendGrid($username, $password);

 // send message
 $response = $sendgrid->send($email);

 print_r($response);
 ```

## <a name="next-steps"></a>Sonraki Adımlar

SendGrid E-posta hizmetinin temellerini öğrendiğiniz için, daha fazla bilgi edinmek için bu bağlantıları takip edin.

* SendGrid belgeleri:<https://sendgrid.com/docs>
* SendGrid PHP kitaplığı:<https://github.com/sendgrid/sendgrid-php>
* Azure müşterileri için SendGrid özel teklifi:<https://sendgrid.com/windowsazure.html>

Daha fazla bilgi için [PHP Geliştirici Merkezi'ne](https://azure.microsoft.com/develop/php/)de bakın.

[https://sendgrid.com]: https://sendgrid.com
[https://sendgrid.com/transactional-email/pricing]: https://sendgrid.com/transactional-email/pricing
[special offer]: https://www.sendgrid.com/windowsazure.html
[Packaging and Deploying PHP Applications for Azure]: https://msdn.microsoft.com/library/windowsazure/hh674499(v=VS.103).aspx
[http://swiftmailer.org/download]: http://swiftmailer.org/download
[curl function]: https://php.net/curl
[bulut tabanlı e-posta hizmeti]: https://sendgrid.com/email-solutions
[işlemsel e-posta teslimi]: https://sendgrid.com/transactional-email
[sendgrid-php kütüphanesi]: https://github.com/sendgrid/sendgrid-php/tree/v2.1.1
[Oluşturucu]: https://getcomposer.org/download/
