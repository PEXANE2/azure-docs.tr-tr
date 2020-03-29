---
title: SendGrid e-posta hizmeti (Node.js) nasıl kullanılır | Microsoft Dokümanlar
description: Azure'daki SendGrid e-posta hizmetiyle nasıl e-posta gönderebildiğini öğrenin. Düğüm.js API kullanılarak yazılan kod örnekleri.
services: ''
documentationcenter: nodejs
author: erikre
manager: wpickett
editor: ''
ms.assetid: cac444b4-26b0-45ea-9c3d-eca28d57dacb
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: article
ms.date: 01/05/2016
ms.author: erikre
ms.openlocfilehash: f2d653441598a47986913d525057672eed24b435
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "60931726"
---
# <a name="how-to-send-email-using-sendgrid-from-nodejs"></a>Node.js'den SendGrid Kullanarak E-posta Nasıl Gönderilir?

Bu kılavuz, Azure'daki SendGrid e-posta hizmetiyle ortak programlama görevlerinin nasıl gerçekleştirililebildiğini gösterir. Örnekler Düğüm.js API kullanılarak yazılır. Kapsanan senaryolar arasında **e-posta oluşturma,** **e-posta gönderme,** **e-posta ekleme,** **filtreleri kullanma**ve **özellikleri güncelleştirme**yer alıyor. SendGrid ve e-posta gönderme hakkında daha fazla bilgi için [Sonraki Adımlar](#next-steps) bölümüne bakın.

## <a name="what-is-the-sendgrid-email-service"></a>SendGrid E-posta Hizmeti nedir?

SendGrid, özel entegrasyonu kolaylaştıran esnek API'lerin yanı sıra güvenilir [işlemsel e-posta teslimi,]ölçeklenebilirlik ve gerçek zamanlı analiz sağlayan bulut tabanlı bir [e-posta hizmetidir.] Sık kullanılan SendGrid kullanım senaryoları şunlardır:

* Müşterilere otomatik olarak makbuz gönderme
* Müşterilere aylık e-el ilanları ve özel teklifler göndermek için dağıtım listeleri nin yönetilmesi
* Engellenen e-posta ve müşteri yanıt verme gibi şeyler için gerçek zamanlı ölçümler toplama
* Eğilimleri belirlemeye yardımcı olacak raporlar oluşturma
* Müşteri sorgularını iletme
* Uygulamanızdan gelen e-posta bildirimleri

Daha fazla bilgi [https://sendgrid.com](https://sendgrid.com)için bkz.

## <a name="create-a-sendgrid-account"></a>SendGrid Hesabı Oluşturma

[!INCLUDE [sendgrid-sign-up](../includes/sendgrid-sign-up.md)]

## <a name="reference-the-sendgrid-nodejs-module"></a>SendGrid Düğümü.js Modülüne Başvurun

Düğüm.js için SendGrid modülü aşağıdaki komut kullanılarak düğüm paket yöneticisi (npm) üzerinden yüklenebilir:

```bash
npm install sendgrid
```

Kurulumdan sonra aşağıdaki kodu kullanarak uygulamanızdaki modülü gereksinim duyabilirsiniz:

```javascript
var sendgrid = require('sendgrid')(sendgrid_username, sendgrid_password);
```

SendGrid modülü SendGrid ve **E-posta** işlevlerini dışa dışa dışa **dışa** vuruyor.
**SendGrid** Web API üzerinden e-posta göndermekten sorumluyken, **E-posta** bir e-posta iletisini kapsüller.

## <a name="how-to-create-an-email"></a>Nasıl?

SendGrid modüllerini kullanarak bir e-posta iletisi oluşturma, önce E-posta işlevini kullanarak bir e-posta iletisi oluşturmayı ve ardından SendGrid işlevini kullanarak göndermeyi içerir. E-posta işlevini kullanarak yeni bir ileti oluşturmaya bir örnek aşağıda verilmiştir:

```javascript
var email = new sendgrid.Email({
    to: 'john@contoso.com',
    from: 'anna@contoso.com',
    subject: 'test mail',
    text: 'This is a sample email message.'
});
```

Ayrıca, html özelliğini ayarlayarak bu iletiyi destekleyen istemciler için bir HTML iletisi de belirtebilirsiniz. Örnek:

```javascript
html: This is a sample <b>HTML<b> email message.
```

Hem metin hem de html özelliklerini ayarlamak, HTML iletilerini destekleyemeyen istemciler için metin içeriğine zarif bir geri dönüş sağlar.

E-posta işlevi tarafından desteklenen tüm özellikler hakkında daha fazla bilgi için [sendgrid-nodejs][sendgrid-nodejs]bölümüne bakın.

## <a name="how-to-send-an-email"></a>Nasıl Yapılsın: E-posta Gönder

E-posta işlevini kullanarak bir e-posta iletisi oluşturduktan sonra, SendGrid tarafından sağlanan Web API'sini kullanarak gönderebilirsiniz. 

### <a name="web-api"></a>Web API

```javascript
sendgrid.send(email, function(err, json){
    if(err) { return console.error(err); }
    console.log(json);
});
```

> [!NOTE]
> Yukarıdaki örnekler bir e-posta nesnesi ve geri arama işlevinde geçen göstermekle birlikte, doğrudan e-posta özelliklerini belirterek doğrudan gönderme işlevini de çağırabilirsiniz. Örnek:  
> 
> ```javascript
> sendgrid.send({
> to: 'john@contoso.com',
> from: 'anna@contoso.com',
> subject: 'test mail',
> text: 'This is a sample email message.'
> });
> ```
>

## <a name="how-to-add-an-attachment"></a>Nasıl Yapılır: Ek Ekleme
Dosyalar **özelliğinde** dosya adı(lar) ve yol(lar) belirtilerek iletiye ekler eklenebilir. Aşağıdaki örnek, bir ek gönderme gösterir:

```javascript
sendgrid.send({
    to: 'john@contoso.com',
    from: 'anna@contoso.com',
    subject: 'test mail',
    text: 'This is a sample email message.',
    files: [
        {
            filename:     '',           // required only if file.content is used.
            contentType:  '',           // optional
            cid:          '',           // optional, used to specify cid for inline content
            path:         '',           //
            url:          '',           // == One of these three options is required
            content:      ('' | Buffer) //
        }
    ],
});
```

> [!NOTE]
> **Dosya** özelliğini kullanırken, dosyaya [fs.readFile](https://nodejs.org/docs/v0.6.7/api/fs.html#fs.readFile)üzerinden erişilebilmelidir. Eklemek istediğiniz dosya Blob kapsayıcısı gibi Azure Depolama'da barındırılıyorsa, **dosyaların** özelliğini kullanarak ek olarak gönderilmeden önce dosyayı yerel depolama alanına veya Azure sürücüsüne kopyalamanız gerekir.
> 
> 

## <a name="how-to-use-filters-to-enable-footers-and-tracking"></a>Nasıl Kullanılır: Altlıkları ve İzlemeyi Etkinleştirmek için Filtreleri Kullanın

SendGrid filtreleri kullanarak ek e-posta işlevselliği sağlar. Bunlar, tıklama izlemeyi etkinleştirme, Google analitiği, abonelik izleme gibi belirli işlevleri etkinleştirmek için bir e-posta iletisine eklenebilecek ayarlardır. Filtrelerin tam listesi için [Filtre Ayarları'na][Filter Settings]bakın.

Filtreler, **filtreler** özelliği ni kullanarak iletiye uygulanabilir.
Her filtre, filtreye özgü ayarlar içeren bir karma ile belirtilir.
Aşağıdaki örnekler altbilgi ve tıklama izleme filtreleri gösterir:

### <a name="footer"></a>Alt bilgi

```javascript
var email = new sendgrid.Email({
    to: 'john@contoso.com',
    from: 'anna@contoso.com',
    subject: 'test mail',
    text: 'This is a sample email message.'
});

email.setFilters({
    'footer': {
        'settings': {
            'enable': 1,
            'text/plain': 'This is a text footer.'
        }
    }
});

sendgrid.send(email);
```

### <a name="click-tracking"></a>İzleme'yi tıklatın

```javascript
var email = new sendgrid.Email({
    to: 'john@contoso.com',
    from: 'anna@contoso.com',
    subject: 'test mail',
    text: 'This is a sample email message.'
});

email.setFilters({
    'clicktrack': {
        'settings': {
            'enable': 1
        }
    }
});

sendgrid.send(email);
```

## <a name="how-to-update-email-properties"></a>Nasıl Yapılsın: E-posta Özelliklerini Güncelleştir

Bazı e-posta özellikleri **setProperty** kullanılarak üzerine yazılabilir veya **addProperty**kullanılarak eklenebilir. Örneğin, kullanarak ek alıcılar ekleyebilirsiniz

```javascript
email.addTo('jeff@contoso.com');
```

veya kullanarak bir filtre ayarlayın

```javascript
email.addFilter('footer', 'enable', 1);
email.addFilter('footer', 'text/html', '<strong>boo</strong>');
```

Daha fazla bilgi için [sendgrid-nodejs'e][sendgrid-nodejs]bakın.

## <a name="how-to-use-additional-sendgrid-services"></a>Nasıl Kullanılır: Ek SendGrid Hizmetlerini Kullanın

SendGrid, Azure uygulamanızdan ek SendGrid işlevselliği nden yararlanmak için kullanabileceğiniz web tabanlı API'ler sunar. Tüm ayrıntılar için [SendGrid API belgelerine][SendGrid API documentation]bakın.

## <a name="next-steps"></a>Sonraki Adımlar

SendGrid E-posta hizmetinin temellerini öğrendiğiniz için, daha fazla bilgi edinmek için bu bağlantıları takip edin.

* SendGrid Düğümü.js modül deposu: [sendgrid-nodejs][sendgrid-nodejs]
* SendGrid API belgeleri:<https://sendgrid.com/docs>
* Azure müşterileri için SendGrid özel teklifi:[http://sendgrid.com/azure.html](https://sendgrid.com/windowsazure.html)

[special offer]: https://sendgrid.com/windowsazure.html
[sendgrid-nodejs]: https://github.com/sendgrid/sendgrid-nodejs
[Filter Settings]: https://sendgrid.com/docs/API_Reference/SMTP_API/apps.html
[SendGrid API documentation]: https://sendgrid.com/docs
[bulut tabanlı e-posta hizmeti]: https://sendgrid.com/email-solutions
[işlemsel e-posta teslimi]: https://sendgrid.com/transactional-email
