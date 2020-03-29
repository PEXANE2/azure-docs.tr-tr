---
title: Azure Röle Hibrit Bağlantılar protokol kılavuzu | Microsoft Dokümanlar
description: Bu makalede, dinleyici ve gönderen rollerindeki istemcileri bağlamak için Karma Bağlantılar rölesi ile istemci tarafı etkileşimleri açıklanmaktadır.
services: service-bus-relay
documentationcenter: na
author: clemensv
manager: timlt
editor: ''
ms.assetid: 149f980c-3702-4805-8069-5321275bc3e8
ms.service: service-bus-relay
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/21/2020
ms.author: clemensv
ms.openlocfilehash: 68668452152064584d1c419a3053ccb642b103f8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76514961"
---
# <a name="azure-relay-hybrid-connections-protocol"></a>Azure Röle Karma Bağlantılar protokolü

Azure Röle, Azure Hizmet Veri Servisi platformunun temel özellik direklerinden biridir. _Röle'nin_ yeni Karma Bağlantılar özelliği, HTTP ve WebSockets'e dayalı güvenli, açık protokol evrimidir. Bu özel bir protokol temeli üzerine inşa edilmiş eski, eşit adlı _BizTalk Hizmetleri_ özelliği yerini almıştır. Karma Bağlantıların Azure Uygulama Hizmetlerine entegrasyonu olduğu gibi çalışmaya devam edecektir.

Karma Bağlantılar, ağ bağlantılı iki uygulama arasında çift yönlü, ikili akış iletişimi ve basit veri gram akışı sağlar. Ya da her iki taraf DA NaTs veya güvenlik duvarları arkasında ikamet edebilir.

Bu makalede, dinleyici ve gönderen rollerindeki istemcileri bağlamak için Karma Bağlantılar rölesi ile istemci tarafı etkileşimleri açıklanmaktadır. Ayrıca, dinleyicilerin yeni bağlantıları ve istekleri nasıl kabul ettiği de açıklanır.

## <a name="interaction-model"></a>Etkileşim modeli

Karma Bağlantılar geçişi, Azure bulutunda tarafların kendi ağlarının bakış açısından keşfedebileceği ve bağlanabileceği bir buluşma noktası sağlayarak iki tarafı birbirine bağlar. Buluşma noktası, bu ve diğer belgelerde, API'lerde ve azure portalında "Karma Bağlantı" olarak adlandırılır. Karma Bağlantılar hizmet bitiş noktası, bu makalenin geri kalanı için "hizmet" olarak adlandırılır.

Hizmet, Web Soket bağlantılarını ve HTTP(S) isteklerini ve yanıtlarını aktarmaya olanak tanır.

Etkileşim modeli, diğer birçok ağ API'sı tarafından oluşturulan terminolojiye bağlıdır. İlk olarak gelen bağlantıları işlemeye hazır olduğunu belirten ve daha sonra geldiklerinde bunları kabul eden bir dinleyici vardır. Diğer tarafta, bir istemci dinleyiciye bağlanır ve bu bağlantının çift yönlü bir iletişim yolu oluşturmak için kabul edilmesini bekleyiş eder. "Bağlan", "Dinle" ve "Kabul Et", çoğu soket API'sinde bulduğunuz terimlerdir.

Aktarılan herhangi bir iletişim modeli, bir hizmet bitiş noktasına doğru giden bağlantıları yapan taraflardan herhangi biri vardır. Bu "dinleyici" de konuşma dilinde kullanımı bir "istemci" yapar ve aynı zamanda diğer terminoloji aşırı neden olabilir. Bu nedenle Hibrit Bağlantılar için kullanılan kesin terminoloji aşağıdaki gibidir:

Bir bağlantının her iki tarafındaki programlara "istemciler" denir, çünkü hizmete istemciler. Bağlantıları bekleyen ve kabul eden istemci "dinleyici" veya "dinleyici rolünde" olduğu söylenir. Hizmet aracılığıyla dinleyiciye karşı yeni bir bağlantı başlatan istemciye "gönderen" denir veya "gönderen rolü" bulunmaktadır.

### <a name="listener-interactions"></a>Dinleyici etkileşimleri

Dinleyicinin hizmetle beş etkileşimi vardır; tüm tel ayrıntıları daha sonra bu makalede referans bölümünde açıklanmıştır.

Dinleme, Kabul et ve İstek iletileri hizmetten alınır. Yenileme ve Ping işlemleri dinleyici tarafından gönderilir.

#### <a name="listen-message"></a>İletiyi dinleme

Dinleyicinin bağlantıları kabul etmeye hazır olduğunu servise hazır göstermek için giden bir WebSocket bağlantısı oluşturur. Bağlantı el sıkışması, Röle ad alanında yapılandırılan bir Karma Bağlantı'nın adını ve bu adüzerinde "Dinle" hakkını veren bir güvenlik belirteci taşır.

WebSocket hizmet tarafından kabul edildiğinde, kayıt tamamlanır ve kurulan WebSocket sonraki tüm etkileşimleri etkinleştirmek için "kontrol kanalı" olarak canlı tutulur. Bu hizmet, tek bir Karma Bağlantı için en fazla 25 eşzamanlı dinleyiciye izin verir. AppHooks için kota belirlenecektir.

Karma Bağlantılar için, iki veya daha fazla etkin dinleyici varsa, gelen bağlantılar aralarında rastgele sırayla dengelenir; adil dağıtım en iyi çaba ile denenir.

#### <a name="accept-message"></a>İletiyi kabul et

Gönderen hizmette yeni bir bağlantı açtığında, hizmet Hibrit Bağlantı'daki etkin dinleyicilerden birini seçer ve not eder. Bu bildirim, dinleyiciye açık denetim kanalı üzerinden JSON iletisi olarak gönderilir. İleti, dinleyicinin bağlantıyı kabul etmek için bağlanması gereken WebSocket bitiş noktasının URL'sini içerir.

URL, herhangi bir ekstra çalışma olmadan doğrudan dinleyici tarafından kullanılabilir ve kullanılmalıdır.
Kodlanmış bilgiler yalnızca kısa bir süre için geçerlidir, gönderen bağlantının uçtan uca kurulmasını beklemek istediği sürece. Varsayılması gereken en fazla 30 saniyedir. URL yalnızca başarılı bir bağlantı denemesi için kullanılabilir. Randevu URL'si ile WebSocket bağlantısı kurulur kurulmaz, bu WebSocket'teki diğer tüm etkinlikler gönderene iletilir. Bu hizmet tarafından herhangi bir müdahale veya yorum olmadan olur.

### <a name="request-message"></a>İleti isteği

WebSocket bağlantılarına ek olarak, bu özellik Karma Bağlantı'da açıkça etkinleştirilmişse, dinleyici bir gönderenden DE HTTP istek çerçeveleri alabilir.

HTTP desteğiyle Hibrit Bağlantılara bağlanan `request` dinleyiciler hareketi işlemelidir. Bağlı yken işlemeyen `request` ve bu nedenle tekrarlanan zaman acısı hatalarına neden olan bir dinleyici, gelecekte hizmet tarafından kara listeye alınabilir.

HTTP çerçeve üstbilgi meta verileri, dinleyici çerçevesi tarafından daha basit kullanım için JSON'a çevrilir, ayrıca HTTP üstbilgi ayrıştırma kitaplıkları JSON ayrıştırıcılarından daha nadirdir. Yalnızca gönderen ile Röle HTTP ağ geçidi arasındaki ilişkiyle ilgili olan ve yetkilendirme bilgileri de dahil olmak üzere http meta verileri iletilir. HTTP istek gövdeleri saydam olarak ikili WebSocket çerçeveleri olarak aktarılır.

Dinleyici, eşdeğer bir yanıt hareketi kullanarak HTTP isteklerine yanıt verebilir.

İstek/yanıt akışı varsayılan olarak denetim kanalını kullanır, ancak gerektiğinde farklı bir randevu WebSocket'e "yükseltilebilir". Farklı WebSocket bağlantıları her istemci konuşması için iş bilgisini artırır, ancak dinleyiciye, hafif istemciler için arzu edilen şekilde mümkün olmayan daha fazla bağlantıyla yüklenir.

Kontrol kanalında, istek ve yanıt gövdeleri en fazla 64 kB boyutuyla sınırlıdır. HTTP üstbilgi meta verileri toplam 32 kB ile sınırlıdır. İstek veya yanıt bu eşiği aşarsa, dinleyici [Kabul'ü](#accept-message)işlemeye eşdeğer bir hareket kullanarak randevu websocket'e yükseltmeli.

İstekler için, hizmet istekleri denetim kanalı üzerinden yönlendirip yönlendirmemeye karar verir. Buna, bir isteğin 64 kB'yi (üstbilgi artı gövde) aşıldığı veya isteğin ["tıklımtıklı" aktarım kodlaması](https://tools.ietf.org/html/rfc7230#section-4.1) ile gönderildiği ve hizmetin isteğin 64kB'yi aşmasını beklemek için nedeni varsa veya isteği okumanın anlık olmadığı durumlarda sınırlı olmayabilir. Hizmet randevu üzerinde isteği iletmeyi seçerse, yalnızca randevu adresini dinleyiciye geçirir.
Dinleyici sonra randevu WebSocket kurmak gerekir ve hizmet derhal randevu WebSocket üzerinde organları da dahil olmak üzere tam istek sunar. Yanıt MUST da randevu WebSocket kullanın.

Denetim kanalı üzerinden gelen istekler için dinleyici, kontrol kanalı üzerinden mi yoksa randevu yoluyla mı yanıt vereceğine karar verir. Hizmet, kontrol kanalı üzerinden yönlendirilen her isteğin bulunduğu bir randevu adresi içermelidir. Bu adres yalnızca geçerli istekten yükseltme için geçerlidir.

Dinleyici yükseltmeyi seçerse, bağlanır ve yanıtı kurulan randevu soketi üzerinden hemen iletilir.

Randevu WebSocket kurulduktan sonra, dinleyici aynı istemciden gelen istek ve yanıtları daha fazla işlemek için bunu korumak GEREKIR. Hizmet, gönderenle HTTPS soketi bağlantısı devam ettiği sürece WebSocket'i koruyacak ve gönderenden gelen sonraki tüm istekleri korunan WebSocket üzerinden yönlendirecektir. Dinleyici randevu WebSocket'i kendi tarafından bırakmayı seçerse, sonraki bir isteğin devam edip etmediğine bakılmaksızın hizmet gönderene olan bağlantıyı da düşürür.

#### <a name="renew-operation"></a>İşlemi yenileme

Dinleyiciyi kaydetmek ve denetim kanalının tutulması için kullanılması gereken güvenlik belirteci, dinleyici etkinken sona erebilir. Belirteç süresi nin sona ermesi devam eden bağlantıları etkilemez, ancak denetim kanalının son kullanma anında veya hemen sonrasında hizmet tarafından düşürülmesine neden olur. "Yenileme" işlemi, denetim kanalının uzun süre korunabilmesi için dinleyicinin denetim kanalıyla ilişkili belirteci değiştirmek için gönderebileceği bir JSON iletisidir.

#### <a name="ping-operation"></a>Ping işlemi

Kontrol kanalı uzun süre boşta kalırsa, yük dengeleyicileri veya NAT'ler gibi yoldaki aracılar TCP bağlantısını düşürebilir. "Ping" işlemi, kanala bağlantının canlı olması gerektiğini hatırlatan kanala az miktarda veri göndererek bunu önler ve aynı zamanda dinleyici için "canlı" bir test görevi de vermektedir. Ping başarısız olursa, denetim kanalı kullanılamaz olarak kabul edilmeli ve dinleyici yeniden bağlanmalıdır.

### <a name="sender-interaction"></a>Gönderen etkileşimi

Gönderenin hizmetle iki etkileşimi vardır: bir Web Soketi bağlanır veya https üzerinden istek gönderir. İstekler gönderen rolünden bir Web Soketi üzerinden gönderilemez.

#### <a name="connect-operation"></a>Bağlanma işlemi

"Bağlan" işlemi, Hizmette Hibrit Bağlantı'nın adını ve (isteğe bağlı olarak, varsayılan olarak gerekli olan) sorgu dizesinde "Gönder" izni veren bir güvenlik belirteci sağlayan bir WebSocket açar. Hizmet daha sonra daha önce açıklandığı şekilde dinleyici ile etkileşime girerken, dinleyici bu WebSocket ile birleştirilmiş bir randevu bağlantısı oluşturur. WebSocket kabul edildikten sonra, bu WebSocket üzerindeki diğer tüm etkileşimler bağlı bir dinleyiciyle yapılır.

#### <a name="request-operation"></a>İstek işlemi

Özelliğin etkin hale alındığı Karma Bağlantılar için gönderen dinleyicilere büyük ölçüde sınırsız HTTP istekleri gönderebilir.

Sorgu dizesinde veya isteğin BIR HTTP üstbilgisinde gömülü olan Bir Röle erişim belirteci dışında, Röle adresindeki tüm HTTP işlemleri ve Röle adres yolunun tüm sonekleri için tamamen saydamdır ve dinleyiciyi uçtan uca yetkilendirme yi ve hatta [CORS](https://www.w3.org/TR/cors/)gibi HTTP uzantısı özelliklerini tamamen kontrol eder.

Relay bitiş ucuile gönderen yetkilendirmesi varsayılan olarak açık, ancak IsteğE BAĞLı. Karma Bağlantı'nın sahibi anonim gönderenlere izin vermeyi seçebilir. Hizmet, yetkilendirme bilgilerini aşağıdaki gibi yakalar, inceler ve şeritlendirir:

1. Sorgu dizesi `sb-hc-token` bir ifade içeriyorsa, ifade HER ZAMAN sorgu dizesinden çıkarılır. Röle yetkisi açıksa değerlendirilecektir.
2. İstek üstbilgileri bir `ServiceBusAuthorization` üstbilgi içeriyorsa, üstbilgi ifadesi HER ZAMAN üstbilgi koleksiyonundan çıkarılır.
   Röle yetkisi açıksa değerlendirilecektir.
3. Yalnızca Röle yetkilendirmesi açıksa ve `Authorization` istek üstbilgileri üstbilgi içeriyorsa ve önceki ifadelerden hiçbiri yoksa, üstbilgi değerlendirilir ve silinecektir. Aksi takdirde, her zaman olduğu gibi `Authorization`geçirilir.

Etkin bir dinleyici yoksa, hizmet 502 "Kötü Ağ Geçidi" hata kodu döndürecek. Hizmet isteği işleyecek gibi görünmüyorsa, hizmet 60 saniye sonra 504 "Ağ Geçidi Zaman Alacaktır" döndürdü.

### <a name="interaction-summary"></a>Etkileşim özeti

Bu etkileşim modelinin sonucu, gönderen istemcinin bir dinleyiciye bağlı olan ve daha fazla önsöz veya hazırlık gerektirmeyen "temiz" bir WebSocket ile el sıkışmadan çıkmasıdır. Bu model, varolan tüm WebSocket istemci uygulamalarının, WebSocket istemci katmanına doğru şekilde oluşturulmuş bir URL sağlayarak Karma Bağlantılar hizmetinden kolayca yararlanmasını sağlar.

Dinleyicinin kabul etkileşimi aracılığıyla elde ettiği randevu bağlantısı WebSocket de temizdir ve "kabul et" işlemleri arasında ayrım yapan bazı minimal ekstra soyutlama ile mevcut websocket sunucu uygulamasına teslim edilebilir kendi çerçeveyerel ağ dinleyicive Hibrid Bağlantılar uzaktan "kabul" işlemleri.

HTTP istek/yanıt modeli gönderene isteğe bağlı yetkilendirme katmanı içeren büyük ölçüde sınırsız BIR HTTP protokol yüzey alanı sağlar. Dinleyici, http gövdeleri taşıyan ikili çerçeveler ile, bir downstream HTTP isteğine dönüştürülebilen veya olduğu gibi ele alınabilen önceden ayrıştırılmış bir HTTP istek üstbilgi bölümü alır. Yanıtlar aynı biçimi kullanır. 64 KB'den az istek ve yanıt gövdesiile etkileşimler, tüm gönderenler için paylaşılan tek bir Web Soketi üzerinden işlenebilir. Daha büyük istekler ve yanıtlar randevu modeli kullanılarak işlenebilir.

## <a name="protocol-reference"></a>Protokol başvurusu

Bu bölümde, daha önce açıklanan protokol etkileşimlerinin ayrıntıları açıklanmaktadır.

Tüm WebSocket bağlantıları, genellikle bazı WebSocket çerçevesi veya API tarafından soyutlanan HTTPS 1.1'den bir yükseltme olarak 443 bağlantı noktasında yapılır. Burada açıklama belirli bir çerçeve önermeden, uygulama tarafsız tutulur.

### <a name="listener-protocol"></a>Dinleyici protokolü

Dinleyici protokolü iki bağlantı hareketi ve üç ileti hareketinden oluşur.

#### <a name="listener-control-channel-connection"></a>Dinleyici kontrol kanalı bağlantısı

Denetim kanalı, aşağıdakiler için bir WebSocket bağlantısı oluşturarak açılır:

`wss://{namespace-address}/$hc/{path}?sb-hc-action=...[&sb-hc-id=...]&sb-hc-token=...`

Genellikle `namespace-address` formun `{myname}.servicebus.windows.net`Karma Bağlantısını barındıran Azure Röle ad alanının tam nitelikli alan adıdır.

Sorgu dize parametresi seçenekleri aşağıdaki gibidir.

| Parametre        | Gerekli | Açıklama
| ---------------- | -------- | -------------------------------------------
| `sb-hc-action`   | Evet      | Dinleyici rolü için parametre **sb-hc-action=listen** olmalıdır
| `{path}`         | Evet      | Bu dinleyiciyi kaydetmek için önceden yapılandırılmış Karma Bağlantı'nın URL kodlanmış ad alanı yolu. Bu ifade sabit `$hc/` yol bölümüne eklenir.
| `sb-hc-token`    | Evet\*    | Dinleyici, **Dinleme** hakkını veren ad alanı veya Karma Bağlantı için geçerli, URL kodlu Hizmet Veri Yoluna Paylaşılan Erişim Belirteci sağlamalıdır.
| `sb-hc-id`       | Hayır       | İstemci tarafından sağlanan bu isteğe bağlı kimlik, uçtan uca tanılama izleme sağlar.

WebSocket bağlantısı, Karma Bağlantı yolunun kaydedilememesi veya geçersiz veya eksik bir belirteci veya başka bir hata nedeniyle başarısız olursa, hata geri bildirimi normal HTTP 1.1 durum geri bildirimi modeli kullanılarak sağlanır. Durum açıklaması, Azure destek personeline iletilebilecek bir hata izleme kimliği içerir:

| Kod | Hata          | Açıklama
| ---- | -------------- | -------------------------------------------------------------------
| 404  | Bulunamadı      | Karma Bağlantı yolu geçersiz veya temel URL hatalı biçimlendirilmiş.
| 401  | Yetkisiz   | Güvenlik belirteci eksik veya yanlış bilgilendirilmiş veya geçersiz.
| 403  | Yasak      | Güvenlik belirteci, bu eylem için bu yol için geçerli değildir.
| 500  | İç Hata | Serviste bir şeyler ters gitti.

WebSocket bağlantısı başlangıçta kurulduktan sonra hizmet tarafından kasıtlı olarak kapatılırsa, bunu yapmanın nedeni, izleme kimliği de içeren açıklayıcı bir hata iletisi ile birlikte uygun bir WebSocket iletişim kuralı hata kodu kullanılarak iletilir. Hizmet, bir hata durumuyla karşılaşmadan denetim kanalını kapatmaz. Herhangi bir temiz kapatma istemci tarafından denetlenir.

| WS Durumu | Açıklama
| --------- | -------------------------------------------------------------------------------
| 1001      | Karma Bağlantı yolu silindi veya devre dışı bırakıldı.
| 1008      | Güvenlik belirteci süresi doldu, bu nedenle yetkilendirme ilkesi ihlal edildi.
| 1011      | Serviste bir şeyler ters gitti.

#### <a name="accept-handshake"></a>El sıkışmayı kabul et

"Kabul et" bildirimi, hizmet tarafından websocket metin çerçevesinde daha önce kurulmuş bir denetim kanalı üzerinden JSON iletisi olarak dinleyiciye gönderilir. Bu iletiyi yanıtlamak yok.

İleti, şu anda aşağıdaki özellikleri tanımlayan "kabul et" adlı bir JSON nesnesi içerir:

* **adres** – gelen bir bağlantıyı kabul etmek için hizmete WebSocket'i kurmak için kullanılacak URL dizesi.
* **id** – bu bağlantı için benzersiz tanımlayıcı. Kimlik gönderen istemci tarafından sağlanmışsa, gönderenin sağladığı değerdir, aksi takdirde sistem tarafından oluşturulan değerdir.
* **connectHeaders** - gönderen tarafından Röle bitiş noktasına sağlanan tüm HTTP üstbilgi, ayrıca Sec-WebSocket-Protokolü ve Sec-WebSocket-Extensions üstbilgi içerir.

```json
{
    "accept" : {
        "address" : "wss://dc-node.servicebus.windows.net:443/$hc/{path}?..."
        "id" : "4cb542c3-047a-4d40-a19f-bdc66441e736",
        "connectHeaders" : {
            "Host" : "...",
            "Sec-WebSocket-Protocol" : "...",
            "Sec-WebSocket-Extensions" : "..."
        }
     }
}
```

JSON iletisinde sağlanan adres URL'si, gönderen soketi kabul etmek veya reddetmek için WebSocket'i oluşturmak için dinleyici tarafından kullanılır.

##### <a name="accepting-the-socket"></a>Soketi kabul etme

Kabul etmek için dinleyici, sağlanan adrese bir WebSocket bağlantısı kurar.

"Kabul et" iletisi bir `Sec-WebSocket-Protocol` üstbilgi taşıyorsa, dinleyicinin yalnızca bu protokolü destekliyorsa WebSocket'i kabul edin. Ayrıca, WebSocket kuruldukça üstbilgi ayarlar.

Aynı `Sec-WebSocket-Extensions` üstbilgi için de geçerlidir. Çerçeve bir uzantıyı destekliyorsa, üstbilgi uzantısı için gerekli `Sec-WebSocket-Extensions` el sıkışmanın sunucu tarafındaki yanıtına ayarlamalıdır.

URL kabul soketi kurmak için olduğu gibi kullanılmalıdır, ancak aşağıdaki parametreleri içerir:

| Parametre      | Gerekli | Açıklama
| -------------- | -------- | -------------------------------------------------------------------
| `sb-hc-action` | Evet      | Bir soketi kabul etmek için parametre`sb-hc-action=accept`
| `{path}`       | Evet      | (aşağıdaki paragrafa bakın)
| `sb-hc-id`     | Hayır       | **Id'nin**önceki açıklamasına bakın.

`{path}`bu dinleyiciyi kaydetmek için önceden yapılandırılmış Karma Bağlantı'nın URL kodlanmış ad alanı yoludur. Bu ifade sabit `$hc/` yol bölümüne eklenir.

İfade, `path` bir sonek ve bir ayırma ileri eğik çizgi sonra kayıtlı adı izleyen bir sorgu dize ifadesi ile uzatılabilir.
Bu, GÖNDEREN istemcinin, HTTP üstbilgilerini eklemesi mümkün olmadığında, gönderme bağımsız değişkenlerini kabul eden dinleyiciye geçirmesini sağlar. Beklenti, dinleyici çerçevesinin sabit yol bölümünü ve kayıtlı adı yoldan ayırıp, kalan adı, muhtemelen bağlantının `sb-`kabul edip etmeyeceğine karar vermek için uygulama için önceden belirlenmiş herhangi bir sorgu dize bağımsız değişkeni olmadan, geri kalanı yapar.

Daha fazla bilgi için aşağıdaki "Gönderen Protokolü" bölümüne bakın.

Bir hata varsa, hizmet aşağıdaki gibi yanıtlayabilir:

| Kod | Hata          | Açıklama
| ---- | -------------- | -----------------------------------
| 403  | Yasak      | URL geçerli değil.
| 500  | İç Hata | Serviste bir şeyler ters gitti.

 Bağlantı kurulduktan sonra, gönderen WebSocket kapandığında veya aşağıdaki durumla sunucu WebSocket'i kapatır:

| WS Durumu | Açıklama                                                                     |
| --------- | ------------------------------------------------------------------------------- |
| 1001      | Gönderen istemci bağlantıyı kapatır.                                    |
| 1001      | Karma Bağlantı yolu silindi veya devre dışı bırakıldı.                        |
| 1008      | Güvenlik belirteci süresi doldu, bu nedenle yetkilendirme ilkesi ihlal edildi. |
| 1011      | Serviste bir şeyler ters gitti.                                            |

##### <a name="rejecting-the-socket"></a>Soketi reddetme

 İletiyi `accept` inceledikten sonra soketin reddedilmesi, durum kodunun ve durum açıklamasının reddedilme nedenini gönderene geri alabilmeleri için benzer bir el sıkışma gerektirir.

 Buradaki protokol tasarımı seçeneği, dinleyici istemci uygulamalarının bir WebSocket istemcisine güvenmeye devam edebilmeleri ve fazladan, çıplak bir HTTP istemcisi kullanması gerekmemesi için Bir WebSocket el sıkışması (tanımlı hata durumunda sona erecek şekilde tasarlanmıştır) kullanmaktır.

 Soketi reddetmek için istemci URI adresini `accept` iletiden alır ve iki sorgu dize sinimetresini aşağıdaki gibi ekler:

| Param                   | Gerekli | Açıklama                              |
| ----------------------- | -------- | ---------------------------------------- |
| sb-hc-statusCode        | Evet      | Sayısal HTTP durum kodu.                |
| sb-hc-statusAçıklama | Evet      | Reddedilmenin insan tarafından okunabilir bir nedeni var. |

Elde edilen URI daha sonra bir WebSocket bağlantısı kurmak için kullanılır.

Doğru şekilde tamamlanırken, websocket kurulmadığından, bu el sıkışma kasıtlı olarak bir HTTP hata kodu 410 ile başarısız olur. Bir şeyler yanlış giderse, aşağıdaki kodlar hatayı açıklar:

| Kod | Hata          | Açıklama                          |
| ---- | -------------- | ------------------------------------ |
| 403  | Yasak      | URL geçerli değil.                |
| 500  | İç Hata | Serviste bir şeyler ters gitti. |

#### <a name="request-message"></a>İleti isteği

İleti, `request` servis tarafından denetim kanalı üzerinden dinleyiciye gönderilir. Aynı ileti, kurulduktan sonra randevu WebSocket üzerinden de gönderilir.

İki `request` bölümden oluşur: bir üstbilgi ve ikili gövde çerçevesi(ler).
Gövde yoksa, gövde çerçeveleri atlanır. Bir gövdenin var olup olmadığının `body` göstergesi, istek iletisindeki boolean özelliğidir.

İstek gövdesiolan bir istek için yapı aşağıdaki gibi görünebilir:

``` text
----- Web Socket text frame ----
{
    "request" :
    {
        "body" : true,
        ...
    }
}
----- Web Socket binary frame ----
FEFEFEFEFEFEFEFEFEFEF...
----- Web Socket binary frame ----
FEFEFEFEFEFEFEFEFEFEF...
----- Web Socket binary frame -FIN
FEFEFEFEFEFEFEFEFEFEF...
----------------------------------
```

Dinleyici MUST istek gövdesini birden çok ikili çerçeveye bölünmüş olarak alma işlemlerini işlemelidir (Bkz. [WebSocket parçaları).](https://tools.ietf.org/html/rfc6455#section-5.4)
Fin bayrak kümesine sahip ikili bir çerçeve alındığı zaman istek sona erer.

Gövdesi olmayan bir istek için yalnızca bir metin çerçevesi vardır.

``` text
----- Web Socket text frame ----
{
    "request" :
    {
        "body" : false,
        ...
    }
}
----------------------------------
```

JSON içeriği `request` aşağıdaki gibidir:

* **adres** - URI dizesi. Bu, bu istek için kullanılacak randevu adresidir. Gelen istek 64 kB'den büyükse, bu iletinin geri kalanı boş bırakılır ve istemci aşağıda açıklanan `accept` işlemle eşdeğer bir randevu el sıkışma sıcağa başlamalı. Hizmet daha sonra kurulan `request` Web soketinin tamını koyar. Yanıtın 64 kB'yi aşması bekleniyorsa, dinleyici MUST da bir randevu el sıkışması başlatın ve yanıtı kurulan Web soketi üzerinden aktarır.
* **id** – dize. Bu istek için benzersiz tanımlayıcı.
* **requestHeaders** – bu nesne, yukarıda açıklandığı gibi yetkilendirme bilgileri ve [ağ](#request-operation)geçidiyle kesinlikle bağlantıyla ilgili üstbilgiler hariç olmak üzere, gönderen tarafından bitiş noktasına verilen tüm HTTP üstbilgilerini içerir. Özellikle, [RFC7230'da](https://tools.ietf.org/html/rfc7230)tanımlanan veya ayrılmış `Via`TÜM üstbilgi , hariç , soyulmuş ve iledilmez:

  * `Connection`(RFC7230, Bölüm 6.1)
  * `Content-Length`(RFC7230, Bölüm 3.3.2)
  * `Host`(RFC7230, Bölüm 5.4)
  * `TE`(RFC7230, Bölüm 4.3)
  * `Trailer`(RFC7230, Bölüm 4.4)
  * `Transfer-Encoding`(RFC7230, Bölüm 3.3.1)
  * `Upgrade`(RFC7230, Bölüm 6.7)
  * `Close`(RFC7230, Bölüm 8.1)

* **requestTarget** – dize. Bu özellik, isteğin ["İstek Hedefi" (RFC7230, Bölüm 5.3)](https://tools.ietf.org/html/rfc7230#section-5.3) tutar. Bu, TÜM `sb-hc-` önceden belirlenmiş parametrelerden arındırılmış sorgu dize bölümünü içerir.
* **yöntem** - dize. Bu [rfc7231, Bölüm 4](https://tools.ietf.org/html/rfc7231#section-4)başına istek yöntemidir. Yöntem `CONNECT` KULLANıLMAMALıDıR.
* **vücut** – boolean. Bir veya daha fazla ikili gövde çerçevesinin aşağıdakileri yapıp olmadığını gösterir.

``` JSON
{
    "request" : {
        "address" : "wss://dc-node.servicebus.windows.net:443/$hc/{path}?...",
        "id" : "42c34cb5-7a04-4d40-a19f-bdc66441e736",
        "requestTarget" : "/abc/def?myarg=value&otherarg=...",
        "method" : "GET",
        "requestHeaders" : {
            "Host" : "...",
            "Content-Type" : "...",
            "User-Agent" : "..."
        },
        "body" : true
     }
}
```

##### <a name="responding-to-requests"></a>İsteklere yanıt verme

Alıcı yanıt vermelidir. Bağlantıyı korurken isteklere yanıt verilmemesi dinleyicinin kara listeye alınmasına neden olabilir.

Yanıtlar herhangi bir sırada gönderilebilir, ancak her isteğe 60 saniye içinde yanıt verilmesi gerekir veya teslimat başarısız olduğu bildirilir. Çerçeve servis tarafından alınana kadar `response` 60 saniyelik süre sayılır. Birden çok ikili kare içeren devam eden bir yanıt 60 saniyeden fazla boşta olamaz veya sonlandırılır.

İstek denetim kanalı üzerinden alınırsa, yanıt ya istek alındığı kontrol kanalında gönderilmeli ya da bir randevu kanalı üzerinden gönderilmesi gerekir.

Yanıt , "yanıt" adlı bir JSON nesnesidir. Gövde içeriğini işleme kuralları, `request` iletide ve `body` özelliğe göre aynı dır.

* **requestId** – dize. Gerekli. Yanıtlanan `id` iletinin `request` özellik değeri.
* **statusCode** – numara. Gerekli. bildirimin sonucunu gösteren sayısal bir HTTP durum kodu. [502 "Bad Gateway"](https://tools.ietf.org/html/rfc7231#section-6.6.3) ve [504 "Gateway Timeout"](https://tools.ietf.org/html/rfc7231#section-6.6.5)dışında [RFC7231, Bölüm 6](https://tools.ietf.org/html/rfc7231#section-6) tüm durum kodları izin verilir.
* **statusDescription** - string. Isteğe bağlı. [RFC7230, Bölüm 3.1.2](https://tools.ietf.org/html/rfc7230#section-3.1.2) başına HTTP durum kodu neden tümcecik
* **responseHeaders** – HTTP başlıkları harici bir HTTP yanıtında ayarlanacak.
  `request`RFC7230 tanımlı üstbilgide olduğu gibi kullanılmamalıdır.
* **vücut** – boolean. İkili gövde çerçevesi(ler)in izleyip izlemediğini gösterir.

``` text
----- Web Socket text frame ----
{
    "response" : {
        "requestId" : "42c34cb5-7a04-4d40-a19f-bdc66441e736",
        "statusCode" : "200",
        "responseHeaders" : {
            "Content-Type" : "application/json",
            "Content-Encoding" : "gzip"
        }
         "body" : true
     }
}
----- Web Socket binary frame -FIN
{ "hey" : "mydata" }
----------------------------------
```

##### <a name="responding-via-rendezvous"></a>Randevu yoluyla yanıt verme

64 kB'yi aşan yanıtlar için yanıt bir randevu soketi üzerinden teslim edilmelidir. Ayrıca, istek 64 kB'yi aşıyorsa ve `request` yalnızca adres alanını içeriyorsa, bir randevu `request`soketi oluşturulmalıdır. Bir randevu soketi kurulduktan sonra, ilgili istemciye verilen yanıtlar ve ilgili istemciden gelen sonraki istekler devam ederken randevu soketi üzerinden teslim edilmelidir.

`request` URL `address` olarak kullanılmalıdır-randevu soketi kurmak için, ancak aşağıdaki parametreleri içerir:

| Parametre      | Gerekli | Açıklama
| -------------- | -------- | -------------------------------------------------------------------
| `sb-hc-action` | Evet      | Bir soketi kabul etmek için parametre`sb-hc-action=request`

Bir hata varsa, hizmet aşağıdaki gibi yanıtlayabilir:

| Kod | Hata           | Açıklama
| ---- | --------------- | -----------------------------------
| 400  | Geçersiz İstek | Tanınmayan eylem veya URL geçerli değildir.
| 403  | Yasak       | URL'nin süresi doldu.
| 500  | İç Hata  | Serviste bir şeyler ters gitti.

 Bağlantı kurulduktan sonra, istemcinin HTTP soketi kapandığında veya aşağıdaki durumla birlikte sunucu WebSocket'i kapatır:

| WS Durumu | Açıklama                                                                     |
| --------- | ------------------------------------------------------------------------------- |
| 1001      | Gönderen istemci bağlantıyı kapatır.                                    |
| 1001      | Karma Bağlantı yolu silindi veya devre dışı bırakıldı.                        |
| 1008      | Güvenlik belirteci süresi doldu, bu nedenle yetkilendirme ilkesi ihlal edildi. |
| 1011      | Serviste bir şeyler ters gitti.                                            |


#### <a name="listener-token-renewal"></a>Dinleyici belirteç yenileme

Dinleyici belirteci süresi dolmak üzereyken, kurulan denetim kanalı üzerinden hizmete metin çerçevesi iletisi göndererek değiştirebilirsiniz. `renewToken`İleti, şu anda aşağıdaki özelliği tanımlayan bir JSON nesnesi içerir:

* **belirteci** – **Dinleme** hakkını veren ad alanı veya Karma Bağlantı için geçerli, URL kodlu Hizmet Veri Yolunda Paylaşılan Erişim belirteci.

```json
{
  "renewToken": {
    "token":
      "SharedAccessSignature sr=http%3a%2f%2fcontoso.servicebus.windows.net%2fhyco%2f&amp;sig=XXXXXXXXXX%3d&amp;se=1471633754&amp;skn=SasKeyName"
  }
}
```

Belirteç doğrulaması başarısız olursa, erişim reddedilir ve bulut hizmeti bir hatayla denetim kanalı WebSocket'i kapatır. Aksi takdirde cevap yoktur.

| WS Durumu | Açıklama                                                                     |
| --------- | ------------------------------------------------------------------------------- |
| 1008      | Güvenlik belirteci süresi doldu, bu nedenle yetkilendirme ilkesi ihlal edildi. |

### <a name="web-socket-connect-protocol"></a>Web Soket ikbağlama protokolü

Gönderen protokolü, dinleyicinin oluşturulme şekliyle etkin bir şekilde aynıdır.
Amaç, uçten uca WebSocket için maksimum saydamlıktır. Bağlanacak adres dinleyiciyle aynıdır, ancak "eylem" farklıdır ve belirteci farklı bir izne ihtiyaç duyar:

```
wss://{namespace-address}/$hc/{path}?sb-hc-action=...&sb-hc-id=...&sbc-hc-token=...
```

_Ad alanı adresi,_ genellikle formun `{myname}.servicebus.windows.net`Karma Bağlantısını barındıran Azure Röle ad alanının tam nitelikli alan adıdır.

İstek, uygulama tanımlı olanlar da dahil olmak üzere rasgele ekstra HTTP üstbilgileri içerebilir. Verilen tüm üstbilgi dinleyiciye akar ve **kabul** `connectHeader` denetimi iletisinin nesnesi üzerinde bulunabilir.

Sorgu dize parametresi seçenekleri aşağıdaki gibidir:

| Param          | Gerekli mi? | Açıklama
| -------------- | --------- | -------------------------- |
| `sb-hc-action` | Evet       | Gönderen rolü için parametre `sb-hc-action=connect`.
| `{path}`       | Evet       | (aşağıdaki paragrafa bakın)
| `sb-hc-token`  | Evet\*     | Dinleyici, **Gönder** hakkını veren ad alanı veya Karma Bağlantı için geçerli, URL kodlu Hizmet Veri Gönder'i Paylaşılan Erişim Jetonu sağlamalıdır.
| `sb-hc-id`     | Hayır        | Uçtan uca tanılama izleme sağlayan ve kabul el sıkışması sırasında dinleyicinin kullanımına sunulan isteğe bağlı bir kimlik.

 Bu `{path}` dinleyiciyi kaydetmek için önceden yapılandırılmış Karma Bağlantı'nın URL kodlanmış ad alanı yoludur. İfade, `path` daha fazla iletişim kurmak için bir sonek ve sorgu dize ifadesi ile genişletilebilir. Karma Bağlantı yol `hyco`altında kayıtlıysa, `path` ifade `hyco/suffix?param=value&...` burada tanımlanan sorgu dize parametreleri tarafından izlenebilir. Daha sonra tam bir ifade aşağıdaki gibi olabilir:

```
wss://{namespace-address}/$hc/hyco/suffix?param=value&sb-hc-action=...[&sb-hc-id=...&]sbc-hc-token=...
```

İfade, `path` URI'nin "kabul et" denetim iletisinde yer alan adresindeki dinleyiciye iletilir.

WebSocket bağlantısı, Karma Bağlantı yolunun kaydedilememesi, geçersiz veya eksik bir belirteç veya başka bir hata nedeniyle başarısız olursa, hata geri bildirimi normal HTTP 1.1 durum geri bildirimi modeli kullanılarak sağlanır. Durum açıklaması, Azure destek personeline iletilebilecek bir hata izleme kimliği içerir:

| Kod | Hata          | Açıklama
| ---- | -------------- | -------------------------------------------------------------------
| 404  | Bulunamadı      | Karma Bağlantı yolu geçersiz veya temel URL hatalı biçimlendirilmiş.
| 401  | Yetkisiz   | Güvenlik belirteci eksik veya yanlış bilgilendirilmiş veya geçersiz.
| 403  | Yasak      | Güvenlik belirteci bu yol ve bu eylem için geçerli değildir.
| 500  | İç Hata | Serviste bir şeyler ters gitti.

WebSocket bağlantısı ilk kurulduktan sonra hizmet tarafından kasıtlı olarak kapatılırsa, bunu yapmanın nedeni, izleme kimliği de içeren açıklayıcı bir hata iletisi ile birlikte uygun bir WebSocket protokol hata kodu kullanılarak iletilir .

| WS Durumu | Açıklama
| --------- | ------------------------------------------------------------------------------- 
| 1000      | Dinleyici soketi kapattı.
| 1001      | Karma Bağlantı yolu silindi veya devre dışı bırakıldı.
| 1008      | Güvenlik belirteci süresi doldu, bu nedenle yetkilendirme ilkesi ihlal edildi.
| 1011      | Serviste bir şeyler ters gitti.

### <a name="http-request-protocol"></a>HTTP istek protokolü

HTTP istek protokolü, protokol yükseltmeleri dışında rasgele HTTP isteklerine izin verir.
HTTP istekleri, karma bağlantılar WebSocket istemcileri için kullanılan $hc düzeltmesi olmadan varlığın normal çalışma zamanı adresine işaret edilir.

```
https://{namespace-address}/{path}?sbc-hc-token=...
```

_Ad alanı adresi,_ genellikle formun `{myname}.servicebus.windows.net`Karma Bağlantısını barındıran Azure Röle ad alanının tam nitelikli alan adıdır.

İstek, uygulama tanımlı olanlar da dahil olmak üzere rasgele ekstra HTTP üstbilgileri içerebilir. RFC7230'da doğrudan tanımlananlar dışında [(istek iletisine](#Request message)bakın) dinleyiciye akıve `requestHeader` **istek** iletisinin nesnesinde bulunabilir.

Sorgu dize parametresi seçenekleri aşağıdaki gibidir:

| Param          | Gerekli mi? | Açıklama
| -------------- | --------- | ---------------- |
| `sb-hc-token`  | Evet\*     | Dinleyici, **Gönder** hakkını veren ad alanı veya Karma Bağlantı için geçerli, URL kodlu Hizmet Veri Gönder'i Paylaşılan Erişim Jetonu sağlamalıdır.

Belirteç, http üstbilgide `ServiceBusAuthorization` `Authorization` de taşınabilir. Hibrit Bağlantı anonim isteklere izin vermek üzere yapılandırılırsa belirteç atlanabilir.

Hizmet, gerçek bir HTTP proxy olarak olmasa bile etkili bir proxy `Via` olarak hareket ettiği için, `Via` bir üstbilgi ekler veya [RFC7230, Bölüm 5.7.1](https://tools.ietf.org/html/rfc7230#section-5.7.1)ile uyumlu varolan üstbilgi açıklama.
Hizmet, `Via`Relay ad alanı ana adını .

| Kod | İleti  | Açıklama                    |
| ---- | -------- | ------------------------------ |
| 200  | Tamam       | İstek en az bir dinleyici tarafından ele alınmıştır.  |
| 202  | Accepted | İstek en az bir dinleyici tarafından kabul edilmiştir. |

Bir hata varsa, hizmet aşağıdaki gibi yanıtlayabilir. Yanıtın hizmetten mi yoksa dinleyiciden mi kaynaklandığı `Via` üstbilginin varlığı yla tanımlanabilir. Üstbilgi varsa, yanıt dinleyiciden dir.

| Kod | Hata           | Açıklama
| ---- | --------------- |--------- |
| 404  | Bulunamadı       | Karma Bağlantı yolu geçersiz veya temel URL hatalı biçimlendirilmiş.
| 401  | Yetkisiz    | Güvenlik belirteci eksik veya yanlış bilgilendirilmiş veya geçersiz.
| 403  | Yasak       | Güvenlik belirteci bu yol ve bu eylem için geçerli değildir.
| 500  | İç Hata  | Serviste bir şeyler ters gitti.
| 503  | Kötü Ağ Geçidi     | İstek herhangi bir dinleyiciye yönlendirilemedi.
| 504  | Ağ Geçidi Zaman | İstek bir dinleyiciye yönlendirildi, ancak dinleyici gerekli süre içinde makbuz kabul etmedi.

## <a name="next-steps"></a>Sonraki adımlar

* [Geçiş hakkında SSS](relay-faq.md)
* [Ad alanı oluşturma](relay-create-namespace-portal.md)
* [.NET kullanmaya başlama](relay-hybrid-connections-dotnet-get-started.md)
* [Node kullanmaya başlama](relay-hybrid-connections-node-get-started.md)
