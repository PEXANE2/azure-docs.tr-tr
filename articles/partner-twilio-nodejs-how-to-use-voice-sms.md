---
title: Azure'da Ses, VoIP ve SMS Mesajlaşması için Twilio'nun kullanılması
description: Azure'daki Twilio API hizmetiyle nasıl telefon görüşmesi yapacağınızı ve SMS mesajı göndermeyi öğrenin. Düğüm.js ile yazılmış kod örnekleri.
services: ''
documentationcenter: nodejs
author: georgewallace
ms.assetid: f558cbbd-13d2-416f-b9b1-33a99c426af9
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: article
ms.date: 11/25/2014
ms.author: gwallace
ms.openlocfilehash: 164bedffcf9a1aca9f1fa46dea254fb928abcf04
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "69637262"
---
# <a name="using-twilio-for-voice-voip-and-sms-messaging-in-azure"></a>Azure'da Ses, VoIP ve SMS Mesajlaşması için Twilio'nun kullanılması
Bu kılavuz, Azure'da Twilio ve node.js ile iletişim kuranbilen uygulamaların nasıl geliştirilebildiğini gösterir.

<a id="whatis"/>

## <a name="what-is-twilio"></a>Twilio nedir?
Twilio, geliştiricilerin telefon görüşmeleri yapıp almasını, kısa mesaj göndermesini ve almasını ve VoIP aramalarını tarayıcı tabanlı ve yerel mobil uygulamalara yerleştirmesini kolaylaştıran bir API platformudur. Dalmadan önce bunun nasıl işlediğini kısaca inceleyelim.

### <a name="receiving-calls-and-text-messages"></a>Arama ve Kısa Mesaj Alma
Twilio, geliştiricilerin hem arama hem de kısa mesaj göndermek ve almak için kullanılabilecek [programlanabilir telefon numaraları satın][purchase_phone] almalarına olanak tanır. Bir Twilio numarası gelen bir arama veya metin aldığında, Twilio web uygulamanıza bir HTTP POST veya GET isteği göndererek arama veya metni nasıl işleyeceğiniz hakkında talimatlar ister. Sunucunuz Twilio'nun HTTP isteğine, bir çağrının veya metnin nasıl işleyeceğinigösteren yönergeler içeren basit bir XML etiketi kümesi olan [TwiML][twiml]ile yanıt verecektir. Biz sadece bir dakika içinde TwiML örnekleri göreceksiniz.

### <a name="making-calls-and-sending-text-messages"></a>Arama Yapma ve Kısa Mesaj Gönderme
Geliştiriciler, Twilio web hizmeti API'sine HTTP isteklerini gerçekleştirerek kısa mesaj gönderebilir veya giden telefon görüşmeleri başlatabilir. Giden aramalar için geliştirici, bağlı olduğunda giden aramayı nasıl işleyeceğiniz için TwiML yönergelerini döndüren bir URL de belirtmelidir.

### <a name="embedding-voip-capabilities-in-ui-code-javascript-ios-or-android"></a>VoIP Özelliklerini UI koduna (JavaScript, iOS veya Android) yerleştirme
Twilio, herhangi bir masaüstü web tarayıcısı, iOS uygulaması veya Android uygulamasını VoIP telefonuna dönüştürebilen istemci tarafındaki bir SDK sağlar. Bu makalede, tarayıcıda VoIP arama nasıl kullanılacağı üzerinde durulacak. Tarayıcıda çalışan *Twilio JavaScript SDK'ya* ek olarak, JavaScript istemcisine bir "yetenek belirteci" vermek için sunucu tarafı uygulaması (node.js uygulamamız) kullanılmalıdır. Sen [Twilio dev blogda][voipnode]node.js ile VoIP kullanma hakkında daha fazla bilgi edinebilirsiniz.

<a id="signup"/>

## <a name="sign-up-for-twilio-microsoft-discount"></a>Twilio için Kaydolun (Microsoft İndirimi)
Twilio hizmetlerini kullanmadan önce [bir hesaba kaydolmalısınız.][signup] Microsoft Azure müşterileri özel bir indirimden yararlanabilir - [buradan kaydolduğunuzdan emin olun!][signup]

<a id="azuresite"/>

## <a name="create-and-deploy-a-nodejs-azure-website"></a>Bir düğüm oluşturma ve dağıtma Azure Web Sitesi
Ardından, Azure'da çalışan bir düğüm.js web sitesi oluşturmanız gerekir. [Bunu yapmak için resmi belgeler burada yer almaktadır.][azure_new_site] Yüksek bir düzeyde, aşağıdakileri yapacaksınız:

* Azure hesabına kaydolma, zaten hesabınız yoksa
* Yeni bir web sitesi oluşturmak için Azure yönetici konsolunu kullanma
* Kaynak denetim desteği ekleme (git kullandığınızı varsayalım)
* Basit bir `server.js` düğüm.js web uygulaması ile dosya oluşturma
* Bu basit uygulamayı Azure'a dağıtma

<a id="twiliomodule"/>

## <a name="configure-the-twilio-module"></a>Twilio Modüllerini Yapılandırın
Daha sonra, Twilio API kullanan basit bir düğüm.js uygulaması yazmaya başlayacaktır. Başlamadan önce, Twilio hesap kimlik kimlik kimlik lerimizi yapılandırmamız gerekir.

### <a name="configuring-twilio-credentials-in-system-environment-variables"></a>Sistem Ortamı Değişkenlerinde Twilio Kimlik Bilgilerinin Yapılandırılması
Twilio arka uç karşı kimlik doğrulaması istekleri yapmak için, bizim hesap SID ve auth belirteci, bizim Twilio hesabı için kullanıcı adı ve şifre olarak ayarlanmış olarak işlev gerekir. Bunları Azure'daki düğüm modülüyle kullanılmak üzere yapılandırmanın en güvenli yolu, doğrudan Azure yönetici konsoluna ayarlayabildiğiniz sistem ortamı değişkenleridir.

Düğüm.js web sitenizi seçin ve "CONFIGURE" bağlantısını tıklayın.  Biraz aşağı kaydırırsanız, uygulamanız için yapılandırma özelliklerini ayarlayabileceğiniz bir alan görürsünüz.  Twilio hesap kimlik bilgilerinizi[(Twilio Konsolunuzda bulunan)][twilio_console]gösterildiği gibi `TWILIO_ACCOUNT_SID` `TWILIO_AUTH_TOKEN`girin - sırasıyla bunları adlandırın ve belirttiğinizden emin olun:

![Azure yönetici konsolu][azure-admin-console]

Bu değişkenleri yapılandırıldıktan sonra, uygulamanızı Azure konsolunda yeniden başlatın.

### <a name="declaring-the-twilio-module-in-packagejson"></a>Twilio modüllerini paket içinde bildirme.json
Sonra, [npm]üzerinden düğüm modülü bağımlılıklarını yönetmek için bir package.json oluşturmamız gerekir. *Azure/düğüm.js* öğreticisinde oluşturduğunuz `package.json` `server.js` dosyayla aynı düzeyde, '.  Bu dosyanın içine aşağıdakileri yerleştirin:

```json
{
  "name": "application-name",
  "version": "0.0.1",
  "private": true,
  "scripts": {
    "start": "node server"
  },
  "dependencies": {
    "body-parser": "^1.16.1",
    "ejs": "^2.5.5",
    "errorhandler": "^1.5.0",
    "express": "^4.14.1",
    "morgan": "^1.8.1",
    "twilio": "^2.11.1"
  }
}
```

Bu, twilio modüllerini bağımlılık olarak, popüler [Express web çerçevesi][express] ve EJS şablon motoru olarak bildirir.  Tamam, şimdi hepimiz hazırız.

<a id="makecall"/>

## <a name="make-an-outbound-call"></a>Giden Arama Yapma
Seçtiğimiz bir numaraya çağrı yapacak basit bir form oluşturalım. Açın `server.js`ve aşağıdaki kodu girin. "CHANGE_ME" yazan yere dikkat edin - azure web sitenizin adını oraya koyun:

```javascript
// Module dependencies
const express = require('express');
const path = require('path');
const http = require('http');
const twilio = require('twilio');
const logger = require('morgan');
const bodyParser = require('body-parser');
const errorHandler = require('errorhandler');
const accountSid = process.env.TWILIO_ACCOUNT_SID;
const authToken = process.env.TWILIO_AUTH_TOKEN;
// Create Express web application
const app = express();

// Express configuration
app.set('port', process.env.PORT || 3000);
app.set('views', __dirname + '/views');
app.set('view engine', 'ejs');
app.use(logger('tiny'));
app.use(bodyParser.urlencoded({ extended: false }))
app.use(bodyParser.json())
app.use(express.static(path.join(__dirname, 'public')));

if (app.get('env') !== 'production') {
  app.use(errorHandler());
}

// Render an HTML user interface for the application's home page
app.get('/', (request, response) => response.render('index'));

// Handle the form POST to place a call
app.post('/call', (request, response) => {
  var client = twilio(accountSid, authToken);

  client.makeCall({
    // make a call to this number
    to:request.body.number,

    // Change to a Twilio number you bought - see:
    // https://www.twilio.com/console/phone-numbers/incoming
    from:'+15558675309',

    // A URL in our app which generates TwiML
    // Change "CHANGE_ME" to your app's name
    url:'https://CHANGE_ME.azurewebsites.net/outbound_call'
  }, () => {
      // Go back to the home page
      response.redirect('/');
  });
});

// Generate TwiML to handle an outbound call
app.post('/outbound_call', (request, response) => {
  var twiml = new twilio.TwimlResponse();

  // Say a message to the call's receiver
  twiml.say('hello - thanks for checking out Twilio and Azure', {
      voice:'woman'
  });

  response.set('Content-Type', 'text/xml');
  response.send(twiml.toString());
});

// Start server
app.listen(app.get('port'), function(){
  console.log(`Express server listening on port ${app.get('port')}`);
});
```

Ardından, bu dizinin `views` içinde aşağıdaki `index.ejs` içerikleri içeren bir dosya oluşturun:

```html
<!DOCTYPE html>
<html>
<head>
  <title>Twilio Test</title>
  <style>
    input { height:20px; width:300px; font-size:18px; margin:5px; padding:5px; }
  </style>
</head>
<body>
  <h1>Twilio Test</h1>
  <form action="/call" method="POST">
      <input placeholder="Enter a phone number" name="number"/>
      <br/>
      <input type="submit" value="Call the number above"/>
  </form>
</body>
</html>
```

Şimdi, web sitenizi Azure'a dağıtın ve evinizi açın. Metin alanına telefon numaranızı girebilmeli ve Twilio numaranızdan bir çağrı alabilmelisiniz!

<a id="sendmessage"/>

## <a name="send-an-sms-message"></a>SMS Mesajı Gönder
Şimdi, bir kısa mesaj göndermek için bir kullanıcı arabirimi ve form işleme mantığı oluşturalım. `server.js`Açın ve son çağrıdan sonra aşağıdaki `app.post`kodu ekleyin:

```javascript
app.post('/sms', (request, response) => {
  const client = twilio(accountSid, authToken);

  client.sendSms({
      // send a text to this number
      to:request.body.number,

      // A Twilio number you bought - see:
      // https://www.twilio.com/console/phone-numbers/incoming
      from:'+15558675309',

      // The body of the text message
      body: request.body.message

  }, () => {
      // Go back to the home page
      response.redirect('/');
  });
});
```

In `views/index.ejs`, bir numara ve kısa mesaj göndermek için ilk in altında başka bir form ekleyin:

```html
<form action="/sms" method="POST">
  <input placeholder="Enter a phone number" name="number"/>
  <br/>
  <input placeholder="Enter a message to send" name="message"/>
  <br/>
  <input type="submit" value="Send text to the number above"/>
</form>
```

Uygulamanızı Azure'a yeniden dağıtın ve artık bu formu gönderip kendinize (veya en yakın arkadaşlarınızdan herhangi birini) kısa mesaj gönderebilmelisiniz!

<a id="nextsteps"/>

## <a name="next-steps"></a>Sonraki Adımlar
Artık iletişim kuranabilecek uygulamalar oluşturmak için node.js ve Twilio kullanmanın temellerini öğrendiniz. Ama bu örnekler ancak Twilio ve node.js ile mümkün olan yüzeyçizik. Node.js ile Twilio kullanarak daha fazla bilgi için aşağıdaki kaynaklara göz atın:

* [Resmi modül dokümanları][docs]
* [Node.js uygulamaları ile VoIP Üzerinde Öğretici][voipnode]
* [Votr - node.js ve CouchDB (üç bölümden) ile gerçek zamanlı SMS oylama uygulaması][votr]
* [Tarayıcıda programlamayı düğümleme.js ile eşleştirme][pair]

Umarız Azure'da düğüm.js ve Twilio'nun hacklemelerini seversiniz!

[purchase_phone]: https://www.twilio.com/console/phone-numbers/search
[twiml]: https://www.twilio.com/docs/api/twiml
[signup]: https://ahoy.twilio.com/azure
[azure_new_site]: app-service/app-service-web-get-started-nodejs.md
[twilio_console]: https://www.twilio.com/console
[npm]: https://npmjs.org
[express]: https://expressjs.com
[voipnode]: https://www.twilio.com/blog/2013/04/introduction-to-twilio-client-with-node-js.html
[docs]: https://www.twilio.com/docs/libraries/reference/twilio-node/
[votr]: https://www.twilio.com/blog/2012/09/building-a-real-time-sms-voting-app-part-1-node-js-couchdb.html
[pair]: https://www.twilio.com/blog/2013/06/pair-programming-in-the-browser-with-twilio.html
[azure-admin-console]: ./media/partner-twilio-nodejs-how-to-use-voice-sms/twilio_1.png
