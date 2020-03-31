---
title: Azure Röle Düğümü API'lere Genel Bakış | Microsoft Dokümanlar
description: Bu makalede, Azure Röle hizmeti için Düğüm.js API'si genel bir bakış sağlar. Ayrıca hyco-ws Düğüm paketinin nasıl kullanılacağını gösterir.
services: service-bus-relay
documentationcenter: na
author: spelluru
manager: timlt
editor: ''
ms.assetid: b7d6e822-7c32-4cb5-a4b8-df7d009bdc85
ms.service: service-bus-relay
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/21/2020
ms.author: spelluru
ms.openlocfilehash: 2877284c419da4999e23490fc986e5da44e5d92e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76514519"
---
# <a name="relay-hybrid-connections-node-api-overview"></a>Röle Hibrit Bağlantılar Düğümü API'ye genel bakış

## <a name="overview"></a>Genel Bakış

[`hyco-ws`](https://www.npmjs.com/package/hyco-ws) Azure Röle Karma Bağlantıları için Düğüm paketi üzerine kuruludur ve ['ws'](https://www.npmjs.com/package/ws) NPM paketini genişletir. Bu paket, söz temel paketin tüm dışa aktarımlarını yeniden dışa aktarıyor ve Azure Relay hizmeti Karma Bağlantılar özelliğiyle tümleştirmeyi sağlayan yeni dışa aktarımlar ekler. 

Bunun `require('hyco-ws')` yerine `require('ws')` bu paketi kullanabilirsiniz varolan uygulamalar, aynı zamanda bir uygulama websocket bağlantıları için yerel olarak dinleyebilirsiniz karma senaryolar sağlar "güvenlik duvarı nın içinde" ve Karma Bağlantılar üzerinden, hepsi aynı anda.
  
## <a name="documentation"></a>Belgeler

API'ler [ana 'ws' paketinde belgelenmiştir.](https://github.com/websockets/ws/blob/master/doc/ws.md) Bu makalede, bu paketin bu taban çizgisinden nasıl farkı açıklanmaktadır. 

Temel paket ve bu 'hyco-ws' arasındaki temel farklar, yeni bir `require('hyco-ws').RelayedServer`sunucu sınıfı, üzerinden dışa aktarılan ve birkaç yardımcı yöntemleri ekler olmasıdır.

### <a name="package-helper-methods"></a>Paket yardımcı yöntemleri

Paket dışa aktarırken aşağıdaki gibi başvuruda bulunabileceğiniz çeşitli yardımcı program yöntemleri vardır:

```JavaScript
const WebSocket = require('hyco-ws');

var listenUri = WebSocket.createRelayListenUri('namespace.servicebus.windows.net', 'path');
listenUri = WebSocket.appendRelayToken(listenUri, 'ruleName', '...key...')
...

```

Yardımcı yöntemler bu paketle birlikte kullanılmak üzeredir, ancak web veya aygıt istemcilerinin dinleyici veya gönderenler oluşturmasını sağlamak için bir Düğüm sunucusu tarafından da kullanılabilir. Sunucu, kısa ömürlü belirteçleri gömen URI'leri geçirerek bu yöntemleri kullanır. Bu URI'ler, WebSocket el sıkışması için HTTP üstbilgilerinin ayarını desteklemeyen ortak WebSocket yığınlarıyla da kullanılabilir. Uri'ye yetkilendirme belirteçleri katıştırma, öncelikle bu kitaplık-dış kullanım senaryoları için desteklenir. 

#### <a name="createrelaylistenuri"></a>createRelayListenUri

```JavaScript
var uri = createRelayListenUri([namespaceName], [path], [[token]], [[id]])
```

Verilen ad alanı ve yol için geçerli bir Azure Relay Hybrid Connection dinleyicisi URI oluşturur. Bu URI daha sonra WebSocketServer sınıfının röle sürümü ile kullanılabilir.

- `namespaceName`(gerekli) - Kullanılacak Azure Röle ad alanının etki alanına uygun adı.
- `path`(gerekli) - Bu ad alanında varolan bir Azure Relay Hybrid Connection'ın adı.
- `token`(isteğe bağlı) - dinleyici URI'ye katıştırılmış daha önce verilmiş bir Röle erişim belirteci (aşağıdaki örneğe bakın).
- `id`(isteğe bağlı) - isteklerin uçtan uca tanılama izlemesini sağlayan bir izleme tanımlayıcısı.

Değer `token` isteğe bağlıdır ve w3C WebSocket yığınında olduğu gibi, Yalnızca WebSocket el sıkışması ile birlikte HTTP üstbilgilerini göndermek mümkün olmadığında kullanılmalıdır.                  


#### <a name="createrelaysenduri"></a>createRelaySendUri
 
```JavaScript
var uri = createRelaySendUri([namespaceName], [path], [[token]], [[id]])
```

Belirli ad alanı ve yol için URI gönderen geçerli bir Azure Röle Karma Bağlantısı oluşturur. Bu URI herhangi bir WebSocket istemcisi ile kullanılabilir.

- `namespaceName`(gerekli) - Kullanılacak Azure Röle ad alanının etki alanına uygun adı.
- `path`(gerekli) - Bu ad alanında varolan bir Azure Relay Hybrid Connection'ın adı.
- `token`(isteğe bağlı) - URI gönder'e katıştırılmış daha önce verilmiş bir Röle erişim belirteci (aşağıdaki örneğe bakın).
- `id`(isteğe bağlı) - isteklerin uçtan uca tanılama izlemesini sağlayan bir izleme tanımlayıcısı.

Değer `token` isteğe bağlıdır ve w3C WebSocket yığınında olduğu gibi, Yalnızca WebSocket el sıkışması ile birlikte HTTP üstbilgilerini göndermek mümkün olmadığında kullanılmalıdır.                   


#### <a name="createrelaytoken"></a>createRelayToken 

```JavaScript
var token = createRelayToken([uri], [ruleName], [key], [[expirationSeconds]])
```

Verilen hedef URI, SAS kuralı ve SAS kural anahtarı için, belirli bir süre önce bağımsız değişken atlanırsa geçerli andan itibaren bir saat veya geçerli andan itibaren geçerli olan Bir Azure Röle Paylaşılan Erişim İmzası (SAS) belirteci oluşturur.

- `uri`(gerekli) - belirteç verilecek uri. URI, HTTP düzenini kullanmak üzere normalleştirilmiştir ve sorgu dize bilgileri çıkarılır.
- `ruleName`(gerekli) - Verilen URI tarafından temsil edilen varlık veya URI ana bilgisayar kısmı tarafından temsil edilen ad alanı için SAS kural adı.
- `key`(gerekli) - SAS kuralı için geçerli anahtar. 
- `expirationSeconds`(isteğe bağlı) - oluşturulan belirteç süresi dolana kadar saniye sayısı. Belirtilmemişse, varsayılan değer 1 saattir (3600).

Verilen belirteç, verilen süre için belirtilen SAS kuralıyla ilişkili hakları verir.

#### <a name="appendrelaytoken"></a>appendRelayToken

```JavaScript
var uri = appendRelayToken([uri], [ruleName], [key], [[expirationSeconds]])
```

Bu yöntem işlevsel olarak `createRelayToken` daha önce belgelenen yönteme eşdeğerdir, ancak uri girişine doğru eklenen belirteci döndürür.

### <a name="class-wsrelayedserver"></a>Sınıf ws. RelayedServer

Sınıf, `hycows.RelayedServer` yerel ağda `ws.Server` dinlemeyen, ancak Azure Röle hizmetini dinleyen temsilciler sınıfın alternatifidir.

İki sınıf çoğunlukla sözleşme uyumludur, bu da `ws.Server` sınıfı kullanan varolan bir uygulamanın aktarılan sürümü kullanmak üzere kolayca değiştirilebildiği anlamına gelir. Temel farklar yapıcı ve kullanılabilir seçeneklerdir.

#### <a name="constructor"></a>Oluşturucu  

```JavaScript 
var ws = require('hyco-ws');
var server = ws.RelayedServer;

var wss = new server(
    {
        server: ws.createRelayListenUri(ns, path),
        token: function() { return ws.createRelayToken('http://' + ns, keyrule, key); }
    });
```

`Server`Oluşturucu, `RelayedServer` bağımsız bir dinleyici olmadığı veya varolan bir HTTP dinleyici çerçevesine katıştırılması mümkün olduğundan, bağımsız bir dinleyici olmadığından farklı bir bağımsız değişken kümesini destekler. WebSocket yönetimi büyük ölçüde Röle hizmetine devredildiklerden daha az seçenek de vardır.

Yapıcı bağımsız değişkenler:

- `server`(gerekli) - genellikle WebSocket.createRelayListenUri() yardımcı yöntemi ile inşa hangi dinlemek için bir Hibrid Bağlantı adı için tam nitelikli URI.
- `token`(gerekli) - Bu bağımsız değişken, önceden verilmiş bir belirteç dizesini veya böyle bir belirteç dizesini elde etmek için çağrılabilen bir geri arama işlevini tutar. Belirteç yenilemesi sağladığından geri arama seçeneği tercih edilir.

#### <a name="events"></a>Olaylar

`RelayedServer`örnekler, gelen istekleri işlemenizi, bağlantılar kurmanızı ve hata koşullarını algılamanızı sağlayan üç olay yontur. İletileri işlemek `connect` için etkinliğe abone olmalısınız. 

##### <a name="headers"></a>Üstbilgi

```JavaScript 
function(headers)
```

Olay, `headers` gelen bağlantı kabul edilmeden hemen önce yükseltilir ve üstbilgideğişikliğinin istemciye gönderilmesine olanak sağlar. 

##### <a name="connection"></a>bağlantı

```JavaScript
function(socket)
```

Yeni bir WebSocket bağlantısı kabul edildiğinde yayılan. Nesne, temel `ws.WebSocket`paketle aynı türdedir.


##### <a name="error"></a>error

```JavaScript
function(error)
```

Altta yatan sunucu bir hata yayırsa, buraya iletilir.  

#### <a name="helpers"></a>Yardımcıları

Aktarılan bir sunucuyu başlatmayı ve gelen bağlantılara hemen abone olmayı kolaylaştırmak için paket, örneklerde de kullanılan basit bir yardımcı işlevini aşağıdaki gibi ortaya çıkarır:

##### <a name="createrelayedlistener"></a>createRelayedListener

```JavaScript
var WebSocket = require('hyco-ws');

var wss = WebSocket.createRelayedServer(
    {
        server: WebSocket.createRelayListenUri(ns, path),
        token: function() { return WebSocket.createRelayToken('http://' + ns, keyrule, key); }
    }, 
    function (ws) {
        console.log('connection accepted');
        ws.onmessage = function (event) {
            console.log(JSON.parse(event.data));
        };
        ws.on('close', function () {
            console.log('connection closed');
        });       
});
``` 

##### <a name="createrelayedserver"></a>createRelayedServer

```javascript
var server = createRelayedServer([options], [connectCallback] )
```

Bu yöntem, oluşturucuyu RelayedServer'ın yeni bir örneğini oluşturması için çağırır ve ardından sağlanan geri aramayı 'bağlantı' olayına abone eder.
 
##### <a name="relayedconnect"></a>röleConnect

Yalnızca yardımcıyı `createRelayedServer` işlevde yansıtmak, `relayedConnect` bir istemci bağlantısı oluşturur ve ortaya çıkan soketteki 'açık' olayına abone olur.

```JavaScript
var uri = WebSocket.createRelaySendUri(ns, path);
WebSocket.relayedConnect(
    uri,
    WebSocket.createRelayToken(uri, keyrule, key),
    function (socket) {
        ...
    }
);
```

## <a name="next-steps"></a>Sonraki adımlar
Azure Röle hakkında daha fazla bilgi edinmek için aşağıdaki bağlantıları ziyaret edin:
* [Azure Geçiş nedir?](relay-what-is-it.md)
* [Kullanılabilir Röle API'leri](relay-api-overview.md)
