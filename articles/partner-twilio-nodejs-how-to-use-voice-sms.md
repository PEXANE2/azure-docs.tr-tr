---
title: Azure 'da Voice, VoIP ve SMS mesajlaşması için Twilio kullanma
description: Azure 'da bir telefon araması yapmayı ve Twilio API hizmetiyle SMS iletisi göndermenizi öğrenin. Kod örnekleri Node.js yazılır.
documentationcenter: nodejs
author: georgewallace
ms.assetid: f558cbbd-13d2-416f-b9b1-33a99c426af9
ms.service: multiple
ms.devlang: nodejs
ms.topic: article
ms.date: 11/25/2014
ms.author: gwallace
ms.openlocfilehash: c4b1c506ff0e78e5e304ceb1682c73f9cd784c95
ms.sourcegitcommit: 2ffa5bae1545c660d6f3b62f31c4efa69c1e957f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/11/2020
ms.locfileid: "88080550"
---
# <a name="using-twilio-for-voice-voip-and-sms-messaging-in-azure"></a>Azure 'da Voice, VoIP ve SMS mesajlaşması için Twilio kullanma
Bu kılavuzda, Azure 'da Twilio ve node.js ile iletişim kuran uygulamalar oluşturma işlemlerinin nasıl yapılacağı gösterilmiştir.

<a name="whatis"></a>

## <a name="what-is-twilio"></a>Twilio nedir?
Twilio, geliştiricilerin telefon aramaları yapmasını ve almasını, SMS mesajları göndermesini ve almasını ve VoIP 'e, tarayıcı tabanlı ve yerel mobil uygulamalara çağrı yapmasını kolaylaştıran bir API platformudur. Bu, içinde kullanılmadan önce bunun nasıl çalıştığını kısaca ele alalım.

### <a name="receiving-calls-and-text-messages"></a>Aramalar ve metin Iletileri alma
Twilio, geliştiricilerin hem çağrı hem de SMS mesajları göndermek ve almak için kullanılabilecek [programlanabilir telefon numaralarını satın][purchase_phone] almasına olanak tanır. Bir Twilio numarası gelen bir çağrı veya metin aldığında, Twilio, Web uygulamanızı bir HTTP POST veya GET isteği göndererek, çağrının veya metnin nasıl işleneceği hakkında yönergeler ister. Sunucunuz [Twiml][twiml]Ile Twilio HTTP isteğine yanıt verir. Bu, bir çağrının veya metnin nasıl işleneceği hakkında yönergeler içeren basıt bir XML etiketleri kümesidir. Yalnızca birkaç dakika içinde TwiML örnekleri görülecektir.

### <a name="making-calls-and-sending-text-messages"></a>Çağrı yapma ve SMS mesajı gönderme
Geliştiriciler Twilio Web hizmeti API 'sine HTTP istekleri yaparak, SMS mesajları gönderebilir veya giden telefon aramaları başlatabilir. Giden çağrılar için, geliştirici bağlantı kurulduktan sonra giden çağrının nasıl işleneceği için TwiML yönergeleri döndüren bir URL de belirtmelidir.

### <a name="embedding-voip-capabilities-in-ui-code-javascript-ios-or-android"></a>Kullanıcı arabirimi kodunda VoIP özelliklerini katıştırma (JavaScript, iOS veya Android)
Twilio, herhangi bir Masaüstü Web tarayıcısını, iOS uygulamasını veya Android uygulamasını bir VoIP telefonuna dönüştürmek için bir istemci tarafı SDK 'Sı sağlar. Bu makalede, tarayıcıda çağıran VoIP kullanma konusuna odaklanacağız. Tarayıcıda çalışan *Twilio JavaScript SDK 'sının* yanı sıra, JavaScript istemcisine "yetenek belirteci" vermek için bir sunucu tarafı uygulamanın (node.js uygulamamız) kullanılması gerekir. [Twilio dev blogu üzerinde][voipnode]node.js ile VoIP kullanma hakkında daha fazla bilgi edinebilirsiniz.

<a name="signup"></a>

## <a name="sign-up-for-twilio-microsoft-discount"></a>Twilio 'ye kaydolma (Microsoft Indirim)
Twilio hizmetlerini kullanmadan önce [bir hesap için kaydolmanız][signup]gerekir. Microsoft Azure müşteriler özel bir indirim elde [edin. buraya kaydolduğunuzdan emin olun][signup]!

<a name="azuresite"></a>

## <a name="create-and-deploy-a-nodejs-azure-website"></a>node.js Azure Web sitesi oluşturma ve dağıtma
Daha sonra, Azure üzerinde çalışan bir node.js Web sitesi oluşturmanız gerekir. [Bu işlemi gerçekleştirmek için resmi belgeler burada yer alır][azure_new_site]. Yüksek düzeyde, şunları yapabilirsiniz:

* Bir Azure hesabına kaydolma, zaten yoksa
* Yeni bir Web sitesi oluşturmak için Azure Yönetici Konsolu 'nu kullanma
* Kaynak denetimi desteği ekleme (git 'i kullandığınızı varsayacağız)
* `server.js`Basit bir node.js Web uygulamasıyla dosya oluşturma
* Bu basit uygulamayı Azure 'a dağıtma

<a name="twiliomodule"></a>

## <a name="configure-the-twilio-module"></a>Twilio modülünü yapılandırma
Daha sonra, Twilio API 'sini kullanan basit bir node.js uygulaması yazmaya başlayacağız. Başlamadan önce, Twilio hesabı kimlik bilgilerinizi yapılandırmamız gerekir.

### <a name="configuring-twilio-credentials-in-system-environment-variables"></a>Sistem ortam değişkenlerinde Twilio kimlik bilgilerini yapılandırma
Kimliği doğrulanmış istekleri Twilio arka ucuna karşı yapmak için, Twilio hesabımızın Kullanıcı adı ve parola olarak işlev gösteren hesap SID 'SI ve kimlik doğrulama belirtecimize ihtiyacımız var. Bunları Azure 'da node modülü ile kullanmak üzere yapılandırmanın en güvenli yolu, doğrudan Azure Yönetici Konsolu 'nda ayarlayabileceğiniz sistem ortam değişkenleri aracılığıyla yapılır.

node.js Web sitenizi seçin ve "YAPıLANDıR" bağlantısına tıklayın.  Bir bit aşağı kaydırırsanız, uygulamanız için yapılandırma özelliklerini ayarlayabileceğiniz bir alan görürsünüz.  Twilio hesabı kimlik bilgilerinizi ([Twilio konsolunuzun içinde bulunur][twilio_console]) gösterildiği gibi girin; bunları `TWILIO_ACCOUNT_SID` sırasıyla, ve olarak girdiğinizden emin olun `TWILIO_AUTH_TOKEN` :

![Azure Yönetici Konsolu][azure-admin-console]

Bu değişkenleri yapılandırdıktan sonra, Azure konsolu 'nda uygulamanızı yeniden başlatın.

### <a name="declaring-the-twilio-module-in-packagejson"></a>Üzerinde package.jsTwilio modülünü bildirme
Sonra, [NPM]aracılığıyla düğüm modülü bağımlılıklarımızı yönetmek için bir package.jsoluşturmanız gerekir. `server.js` *Azure/node.js* öğreticisinde oluşturduğunuz dosyayla aynı düzeyde adlı bir dosya oluşturun `package.json` .  Bu dosyanın içine şunu girin:

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

Bu, Twilio modülünü bağımlılık olarak, popüler [Express Web çerçevesi][express] ve EJS şablon altyapısını bildirir.  Artık tüm bunlar için bir kod yazalım!

<a name="makecall"></a>

## <a name="make-an-outbound-call"></a>Giden bir çağrı yapın
Seçtiğimiz bir sayıya çağrı oluşturacak basit bir form oluşturalım. Açın `server.js` ve aşağıdaki kodu girin. Burada "CHANGE_ME" ifadesinin olduğunu ve Azure Web sitenizin adını buraya yerleştirdiğini aklınızda bulabilirsiniz:

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

Sonra, bu dizinin içinde adlı bir dizin oluşturun `views` , aşağıdaki içeriklerle adlı bir dosya oluşturun `index.ejs` :

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

Şimdi, Web sitenizi Azure 'a dağıtın ve evinizin açın. Telefon numaranızı metin alanına girebilmeniz ve Twilio numaranızı bir çağrı alabilmesi gerekir!

<a name="sendmessage"></a>

## <a name="send-an-sms-message"></a>SMS Iletisi gönder
Şimdi bir kısa mesaj göndermek için bir kullanıcı arabirimi ve form işleme mantığı ayarlayalım. Açın `server.js` ve son çağrısından sonra aşağıdaki kodu ekleyin `app.post` :

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

`views/index.ejs`' De, bir sayı ve bir kısa mesaj göndermek için ilk birinin altına başka bir form ekleyin:

```html
<form action="/sms" method="POST">
  <input placeholder="Enter a phone number" name="number"/>
  <br/>
  <input placeholder="Enter a message to send" name="message"/>
  <br/>
  <input type="submit" value="Send text to the number above"/>
</form>
```

Uygulamanızı Azure 'a yeniden dağıtın ve artık bu formu gönderebilmeniz ve kendinize (ya da en yakın arkadaşlarınızın herhangi birine) bir kısa mesaj gönderebilmelisiniz!

<a name="nextsteps"></a>

## <a name="next-steps"></a>Sonraki Adımlar
Artık iletişim kuran uygulamalar oluşturmak için node.js ve Twilio kullanmanın temellerini öğrendiniz. Ancak bu örnekler, Twilio ve node.js ile mümkün olan yüzeylerin yüzeyini de yok. node.js ile Twilio kullanma hakkında daha fazla bilgi için aşağıdaki kaynaklara göz atın:

* [Resmi modül belgeleri][docs]
* [node.js uygulamalarla VoIP hakkında öğretici][voipnode]
* [Votr-node.js ve Couşdb (üç bölümden oluşan) ile gerçek zamanlı SMS oylama uygulaması][votr]
* [node.jsile tarayıcıda programlama eşleştirin][pair]

Azure 'da node.js ve Twilio 'u çok sevtireceğiz!

[purchase_phone]: https://www.twilio.com/console/phone-numbers/search
[twiml]: https://www.twilio.com/docs/api/twiml
[signup]: https://ahoy.twilio.com/azure
[azure_new_site]: app-service/quickstart-nodejs.md
[twilio_console]: https://www.twilio.com/console
[npm]: https://npmjs.org
[express]: https://expressjs.com
[voipnode]: https://www.twilio.com/blog/2013/04/introduction-to-twilio-client-with-node-js.html
[docs]: https://www.twilio.com/docs/libraries/reference/twilio-node/
[votr]: https://www.twilio.com/blog/2012/09/building-a-real-time-sms-voting-app-part-1-node-js-couchdb.html
[pair]: https://www.twilio.com/blog/2013/06/pair-programming-in-the-browser-with-twilio.html
[azure-admin-console]: ./media/partner-twilio-nodejs-how-to-use-voice-sms/twilio_1.png
