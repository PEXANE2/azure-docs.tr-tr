---
title: Azure SignalR Hizmeti için performans kılavuzu
description: Azure SignalR Hizmeti'nin performansına ve kıyaslama sine genel bakış. Kapasite planlarken göz önünde bulundurulması gereken temel ölçümler.
author: sffamily
ms.service: signalr
ms.topic: conceptual
ms.date: 11/13/2019
ms.author: zhshang
ms.openlocfilehash: 68cad32be177fa20794399157fca89e87c2f8f59
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74157674"
---
# <a name="performance-guide-for-azure-signalr-service"></a>Azure SignalR Hizmeti için performans kılavuzu

Azure SinyalR Hizmetini kullanmanın en önemli avantajlarından biri, SignalR uygulamalarını ölçeklemenin kolaylığıdır. Büyük ölçekli bir senaryoda, performans önemli bir faktördür. 

Bu kılavuzda, SignalR uygulama performansını etkileyen faktörleri tanıtacağız. Tipik performansı farklı kullanım durumu senaryolarında açıklarız. Sonunda, bir performans raporu oluşturmak için kullanabileceğiniz ortamı ve araçları tanıtacağız.

## <a name="term-definitions"></a>Terim tanımları

*Gelen*: Azure SignalR Hizmeti'ne gelen ileti.

*Giden*: Azure SignalR Hizmeti'nden giden ileti.

*Bant Genişliği*: 1 saniyedeki tüm iletilerin toplam boyutu.

*Varsayılan mod*: Azure Sinyal Hizmeti örneği oluşturulduğunda varsayılan çalışma modu. Azure SignalR Hizmeti, uygulama sunucusunun istemci bağlantılarını kabul etmeden önce onunla bağlantı kurmasını bekler.

*Sunucusuz mod*: Azure SignalR Hizmetinin yalnızca istemci bağlantılarını kabul ettiği mod. Sunucu bağlantısına izin verilmez.

## <a name="overview"></a>Genel Bakış

Azure SignalR Hizmeti, farklı performans kapasiteleri için yedi Standart katman tanımlar. Bu kılavuz aşağıdaki soruları yanıtlar:

-   Her katman için tipik Azure Sinyal Hizmeti performansı nedir?

-   Azure SignalR Hizmeti ileti iş bilgimi (örneğin, saniyede 100.000 ileti gönderme) gereksinimlerimi karşılıyor mu?

-   Benim özel senaryom için, hangi katman benim için uygundur? Ya da uygun katmanı nasıl seçebilirim?

-   Ne tür bir uygulama sunucusu (VM boyutu) benim için uygundur? Kaç tane sini konuşlandırmalıyım?

Bu soruları yanıtlamak için, bu kılavuz ilk performansı etkileyen faktörlerin üst düzey bir açıklama verir. Daha sonra, tipik kullanım durumları için her katman için gelen ve giden maksimum iletileri gösterir: **yankı,** **yayın,** **gruba gönderme**ve **bağlantıya gönderme** (eşler arası sohbet).

Bu kılavuz tüm senaryoları (ve farklı kullanım durumlarını, ileti boyutlarını, ileti gönderme modellerini vb.) kapsayamaz. Ancak size yardımcı olacak bazı yöntemler sağlar:

- Gelen veya giden iletiler için yaklaşık gereksiniminizi değerlendirin.
- Performans tablosunu kontrol ederek uygun katmanları bulun.

## <a name="performance-insight"></a>Performans içgörüsü

Bu bölümde performans değerlendirme metodolojileri açıklanır ve sonra performansı etkileyen tüm faktörleri listeler. Sonunda, performans gereksinimlerini değerlendirmenize yardımcı olacak yöntemler sağlar.

### <a name="methodology"></a>Yöntem

*İş sonu* ve *gecikme,* performans denetiminin iki tipik yönü. Azure Sinyal R Hizmeti için her SKU katmanının kendi iş yapma azaltma ilkesi vardır. İlke, iletilerin yüzde 99'u 1 saniyeden kısa gecikme ye sahip *olduğunda, izin verilen maksimum iş çıkış (gelen ve giden bant genişliği)* en fazla elde edilen iş parçası olarak tanımlar.

Gecikme süresi, iletigönderen bağlantıdan Azure SinyalR Hizmeti'nden yanıt iletisi almaya kadar geçen süredir. **Yankıyı** örnek alalım. Her istemci bağlantısı iletiye bir zaman damgası ekler. Uygulama sunucusunun merkezi özgün iletiyi istemciye geri gönderir. Yani yayılma gecikmesi her istemci bağlantısı tarafından kolayca hesaplanır. **Yayındaki**her ileti için zaman damgası eklenir, **gruba gönderin**ve **bağlantıya gönderin.**

Binlerce eşzamanlı istemci bağlantısını simüle etmek için Azure'daki sanal özel ağda birden çok Sanal Sanal Sanal Ağ oluşturulur. Tüm bu VM'ler aynı Azure SinyalR Hizmeti örneğine bağlanır.

Azure SinyalR Hizmeti'nin varsayılan modunda, uygulama sunucusu VM'leri istemci VM'ler ile aynı sanal özel ağda dağıtılır. Tüm istemci VM'leri ve uygulama sunucusu VM'leri, bölgeler arası gecikmeyi önlemek için aynı bölgenin aynı ağında dağıtılır.

### <a name="performance-factors"></a>Performans faktörleri

Teorik olarak, Azure SignalR Hizmeti kapasitesi hesaplama kaynaklarıyla sınırlıdır: CPU, bellek ve ağ. Örneğin, Azure Sinyal RHizmeti'ne daha fazla bağlantı, hizmetin daha fazla bellek kullanmasına neden olur. Daha büyük ileti trafiği için (örneğin, her ileti 2.048 bayttan büyüktür), Azure SinyalR Hizmetinin trafiği işlemek için daha fazla CPU döngüsü harcaması gerekir. Bu arada, Azure ağ bant genişliği de maksimum trafik için bir sınır uygular.

Aktarım türü performansı etkileyen başka bir faktördür. Üç tür [WebSocket,](https://en.wikipedia.org/wiki/WebSocket) [Sunucu-Gönderildi-Olay](https://en.wikipedia.org/wiki/Server-sent_events)ve [Uzun Yoklama](https://en.wikipedia.org/wiki/Push_technology)vardır. 

WebSocket, tek bir TCP bağlantısı üzerinden çift yönlü ve tam çift yönlü iletişim protokolüdür. Sunucu-Gönderilen Olay, iletileri sunucudan istemciye itmek için tek yönlü bir iletişim kuralıdır. Uzun Yoklama, istemcilerin bir HTTP isteği aracılığıyla sunucudan düzenli olarak bilgi yoklaması gerektirir. Aynı koşullar altında aynı API için, WebSocket en iyi performansa sahiptir, Sunucu-Gönderilen Olay daha yavaştır ve Uzun Yoklama en yavaşıdır. Azure SignalR Hizmeti varsayılan olarak WebSocket önerir.

İleti yönlendirme maliyeti de performansı sınırlar. Azure SinyalR Hizmeti, iletiyi bir dizi istemci veya sunucudan diğer istemcilere veya sunuculara yönlendiren ileti yönlendiricisi olarak rol oynar. Farklı bir senaryo veya API farklı bir yönlendirme ilkesi gerektirir. 

**Yankı**için, istemci kendisine bir ileti gönderir ve yönlendirme hedefi de kendisidir. Bu desen en düşük yönlendirme maliyetine sahiptir. Ancak **yayın**için, **gruba göndermek**ve **bağlantıya göndermek için**Azure SinyalR Hizmeti'nin hedef bağlantıları dahili dağıtılmış veri yapısı üzerinden araması gerekir. Bu ekstra işlem daha fazla CPU, bellek ve ağ bant genişliği kullanır. Sonuç olarak, performans daha yavaştır.

Varsayılan modda, uygulama sunucusu belirli senaryolar için bir darboğaz alabilirsiniz. Azure SinyalR SDK hub'ı çağırmak zorunda kalırken, sinyal sinyalleri aracılığıyla her istemciyle canlı bağlantı sağlıyor.

Sunucusuz modda, istemci HTTP gönderisi tarafından WebSocket kadar verimli olmayan bir ileti gönderir.

Başka bir faktör protokol: JSON ve [MessagePack](https://msgpack.org/index.html). MessagePack boyutu daha küçük türdedir ve JSON'dan daha hızlı teslim edilir. MessagePack performansı artırmayabilir. Azure SinyalR Hizmeti'nin performansı protokollere duyarlı değildir, çünkü istemcilerden sunuculara ileti iletilmesi sırasında ileti yükünü çözemez veya bunun tersi deşifre edilmez.

Özetle, aşağıdaki faktörler gelen ve giden kapasiteyi etkiler:

-   SKU katmanı (CPU/bellek)

-   Bağlantı sayısı

-   İleti boyutu

-   İleti gönderme oranı

-   Aktarım türü (WebSocket, Sunucu-Gönderildi-Olay veya Uzun Yoklama)

-   Kullanım örneği senaryosu (yönlendirme maliyeti)

-   Uygulama sunucusu ve hizmet bağlantıları (sunucu modunda)


### <a name="finding-a-proper-sku"></a>Uygun bir SKU bulma

Gelen/giden kapasiteyi nasıl değerlendirebilirsiniz veya belirli bir kullanım örneği için hangi katmanın uygun olduğunu bulabilirsiniz?

Uygulama sunucusunun yeterince güçlü olduğunu ve performans darboğazı olmadığını varsayalım. Ardından, her katman için gelen ve giden maksimum bant genişliğini kontrol edin.

#### <a name="quick-evaluation"></a>Hızlı değerlendirme

Önce bazı varsayılan ayarları varsayarak değerlendirmeyi basitleştirelim: 

- Aktarım türü WebSocket'tir.
- İleti boyutu 2048 bayt.
- Her 1 saniyede bir ileti gönderilir.
- Azure SignalR Hizmeti varsayılan moddadır.

Her katmanın kendi maksimum gelen bant genişliği ve giden bant genişliği vardır. Gelen veya giden bağlantı sınırı aştıktan sonra sorunsuz bir kullanıcı deneyimi garanti edilmez.

En düşük yönlendirme maliyetine sahip **olduğundan, Yankı** maksimum gelen bant genişliğini verir. **Yayın,** en yüksek giden ileti bant genişliğini tanımlar.

Aşağıdaki iki tabloda vurgulanan değerleri *aşmayın.*

|       Yankı                        | Birim1 | Ünite2 | Ünite5 | Ünite10 | Ünite20 | Ünite50 | Ünite100 |
|-----------------------------------|-------|-------|-------|--------|--------|--------|---------|
| Bağlantılar                       | 1000 | 2.000 | 5.000 | 10,000 | 20.000 | 50.000 | 100.000 |
| **Gelen bant genişliği** | **2 Mb/sn**    | **4 Mb/sn**    | **10 Mb/sn**   | **20 MBps**    | **40 MBps**    | **100 MBps**   | **200 MBps**    |
| Giden bant genişliği | 2 Mb/sn   | 4 Mb/sn   | 10 Mb/sn  | 20 MBps   | 40 MBps   | 100 MBps  | 200 MBps   |


|     Yayınla             | Birim1 | Ünite2 | Ünite5  | Ünite10 | Ünite20 | Ünite50  | Ünite100 |
|---------------------------|-------|-------|--------|--------|--------|---------|---------|
| Bağlantılar               | 1000 | 2.000 | 5.000  | 10,000 | 20.000 | 50.000  | 100.000 |
| Gelen bant genişliği  | 4 KBps   | 4 KBps   | 4 KBps    | 4 KBps    | 4 KBps    | 4 KBps     | 4 KBps    |
| **Giden bant genişliği** | **4 Mb/sn**    | **8 Mb/sn**    | **20 MBps**    | **40 MBps**    | **80 MBps**    | **200 MBps**    | **400 MBps**   |

*Gelen bant genişliği* ve *giden bant genişliği* saniyede toplam ileti boyutudur.  İşte onlar için formüller şunlardır:
```
  inboundBandwidth = inboundConnections * messageSize / sendInterval
  outboundBandwidth = outboundConnections * messageSize / sendInterval
```

- *gelen Bağlantılar*: İletiyi gönderen bağlantı sayısı.

- *giden Bağlantılar*: İletiyi alan bağlantı sayısı.

- *messageSize*: Tek bir iletinin boyutu (ortalama değer). 1.024 bayttan daha az olan küçük bir iletinin performans etkisi 1.024 baytlık iletiye benzer.

- *sendInterval*: Bir ileti gönderme zamanı. Genellikle ileti başına 1 saniyedir, bu da saniyede bir ileti göndermek anlamına gelir. Daha küçük bir aralık, bir zaman diliminde daha fazla ileti göndermek anlamına gelir. Örneğin, ileti başına 0,5 saniye, saniyede iki ileti göndermek anlamına gelir.

- *Bağlantılar*: Her katman için Azure Sinyal RHizmeti için taahhüt edilen maksimum eşik. Bağlantı numarası daha da artırılırsa, bağlantı azaltma muzdarip olacaktır.

#### <a name="evaluation-for-complex-use-cases"></a>Karmaşık kullanım durumları için değerlendirme

##### <a name="bigger-message-size-or-different-sending-rate"></a>Daha büyük ileti boyutu veya farklı gönderme hızı

Gerçek kullanım örneği daha karmaşıktır. 2.048 bayttan daha büyük bir ileti gönderebilir veya ileti gönderme hızı saniyede bir ileti değildir. Unit100'ün yayınını nasıl değerlendireceklerini bulmak için örnek olarak alalım.

Aşağıdaki tabloda gerçek bir **yayın**kullanım örneği gösterilmektedir. Ancak ileti boyutu, bağlantı sayısı ve ileti gönderme oranı önceki bölümde varsaydığımızdan farklıdır. Soru, bu öğelerden herhangi birini (ileti boyutu, bağlantı sayısı veya ileti gönderme oranı) yalnızca iki sini tanıyorsak nasıl anladığımızdır.

| Yayınla  | İleti boyutu | Saniyede gelen iletiler | Bağlantılar | Aralıkları gönder |
|---|---------------------|--------------------------|-------------|-------------------------|
| 1 | 20 KB                | 1                        | 100.000     | 5 sn                      |
| 2 | 256 KB               | 1                        | 8,000       | 5 sn                      |

Aşağıdaki formülü önceki formüle göre çıkarmak kolaydır:

```
outboundConnections = outboundBandwidth * sendInterval / messageSize
```

Unit100 için, en yüksek giden bant genişliği önceki tablodan 400 MB'dır. 20-KB ileti boyutu için, maksimum giden bağlantılar 400 \* MB 5 / 20 KB = 100.000 olmalıdır, hangi gerçek değeri eşleşir.

##### <a name="mixed-use-cases"></a>Karma kullanım örnekleri

Gerçek kullanım örneği genellikle birlikte dört temel kullanım durumlarda karıştırır: **yankı**, **yayın**, **gruba göndermek**, ve **bağlantıya gönderin.** Kapasiteyi değerlendirmek için kullandığınız metodoloji aşağıdakileri yapmaktır:

1. Karma kullanım servis taleplerini dört temel kullanım örneğine bölün.
1. Önceki formülleri ayrı ayrı kullanarak en fazla gelen ve giden ileti bant genişliğini hesaplayın.
1. Toplam maksimum gelen/giden bant genişliğini elde etmek için bant genişliği hesaplamalarını özetleyin. 

Ardından, maksimum gelen/giden bant genişliği tablolarından uygun katmanı seçin.

> [!NOTE]
> Yüzlerce veya binlerce küçük gruba veya birbirine ileti gönderen binlerce istemciye ileti göndermek için yönlendirme maliyeti baskın hale gelecektir. Bu etkiyi göz önünde bulundur.

İstemcilere ileti gönderme nin kullanım durumu için, uygulama sunucusunun darboğaz *olmadığından* emin olun. Aşağıdaki "Örnek Olay İncelemesi" bölümü, kaç uygulama sunucusuna ihtiyacınız olduğu ve kaç sunucu bağlantısı yapılandırmanız gerektiği hakkında yönergeler verir.

## <a name="case-study"></a>Örnek olay incelemesi

Aşağıdaki bölümler WebSocket taşıma için dört tipik kullanım durumda geçmesi: **yankı**, **yayın**, **gruba göndermek**, ve **bağlantıya gönderin.** Her senaryo için bölüm, Azure SinyalR Hizmeti için geçerli gelen ve giden kapasiteyi listeler. Ayrıca performansı etkileyen ana faktörleri açıklar.

Varsayılan modda, uygulama sunucusu Azure SignalR Hizmeti ile beş sunucu bağlantısı oluşturur. Uygulama sunucusu varsayılan olarak Azure SignalR Hizmeti SDK'sını kullanır. Aşağıdaki performans testi sonuçlarında, sunucu bağlantıları 15'e yükseltilir (veya büyük bir gruba ileti yayınlamak ve göndermek için daha fazla).

Farklı kullanım örneklerinin uygulama sunucuları için farklı gereksinimleri vardır. **Yayının** az sayıda uygulama sunucusuna ihtiyacı vardır. **Echo** veya **bağlantı göndermek** birçok uygulama sunucusu gerekir.

Tüm kullanım durumlarında varsayılan ileti boyutu 2.048 bayt ve ileti gönderme aralığı 1 saniyedir.

### <a name="default-mode"></a>Varsayılan mod

İstemciler, web uygulaması sunucuları ve Azure SinyalR Hizmeti varsayılan modda yer alır. Her müşteri tek bir bağlantı anlamına gelir.

#### <a name="echo"></a>Yankı

İlk olarak, bir web uygulaması Azure SignalR Hizmeti'ne bağlanır. İkinci olarak, birçok istemci web uygulamasına bağlanır ve bu uygulama, erişim belirteci ve bitiş noktasıyla istemcileri Azure SignalR Hizmeti'ne yönlendirir. Ardından, istemciler Azure SignalR Hizmeti ile WebSocket bağlantıları kurar.

Tüm istemciler bağlantılar kurduktan sonra, her saniye belirli hub'a zaman damgası içeren bir ileti göndermeye başlarlar. Hub, iletiyi özgün istemcisine geri döndürer. Her istemci yankı iletisini geri aldığında gecikme yi hesaplar.

Aşağıdaki diyagramda, 5 ile 8 (kırmızı vurgulu trafik) bir döngü içindedir. Döngü varsayılan bir süre (5 dakika) çalışır ve tüm ileti gecikmesinin istatistiklerini alır.

![Yankı kullanım örneği için trafik](./media/signalr-concept-performance/echo.png)

**Yankı** davranışı, en fazla gelen bant genişliğinin en yüksek giden bant genişliğine eşit olduğunu belirler. Ayrıntılar için aşağıdaki tabloya bakın.

|       Yankı                        | Birim1 | Ünite2 | Ünite5 | Ünite10 | Ünite20 | Ünite50 | Ünite100 |
|-----------------------------------|-------|-------|-------|--------|--------|--------|---------|
| Bağlantılar                       | 1000 | 2.000 | 5.000 | 10,000 | 20.000 | 50.000 | 100.000 |
| Saniyede gelen/giden iletiler | 1000 | 2.000 | 5.000 | 10,000 | 20.000 | 50.000 | 100.000 |
| Gelen/giden bant genişliği | 2 Mb/sn   | 4 Mb/sn   | 10 Mb/sn  | 20 MBps   | 40 MBps   | 100 MBps  | 200 MBps   |

Bu kullanım durumunda, her istemci uygulama sunucusunda tanımlanan hub'ı çağırır. Hub yalnızca özgün istemci tarafında tanımlanan yöntemi çağırır. Bu hub **yankı**için en hafif hub'dır.

```
        public void Echo(IDictionary<string, object> data)
        {
            Clients.Client(Context.ConnectionId).SendAsync("RecordLatency", data);
        }
```

Bu basit hub için bile, **yankı** gelen ileti yükü arttıkça uygulama sunucusundaki trafik basıncı belirgindir. Bu trafik basıncı, büyük SKU katmanları için birçok uygulama sunucusu gerektirir. Aşağıdaki tabloda her katman için uygulama sunucusu sayısı listelenir.


|    Yankı          | Birim1 | Ünite2 | Ünite5 | Ünite10 | Ünite20 | Ünite50 | Ünite100 |
|------------------|-------|-------|-------|--------|--------|--------|---------|
| Bağlantılar      | 1000 | 2.000 | 5.000 | 10,000 | 20.000 | 50.000 | 100.000 |
| Uygulama sunucusu sayısı | 2     | 2     | 2     | 3      | 3      | 10     | 20      |

> [!NOTE]
> İstemci bağlantı numarası, ileti boyutu, ileti gönderme hızı, SKU katmanı ve uygulama sunucusunun CPU/bellek **yankı**genel performansını etkiler.

#### <a name="broadcast"></a>Yayınla

**Yayın**için, web uygulaması iletiyi aldığında, tüm istemcilere yayın eder. Yayın yapmak için ne kadar çok istemci varsa, tüm istemcilere o kadar çok ileti trafiği verilir. Aşağıdaki diyagrama bakın.

![Yayın kullanım örneği için trafik](./media/signalr-concept-performance/broadcast.png)

Az sayıda istemci yayın yapıyor. Gelen ileti bant genişliği küçüktür, ancak giden bant genişliği çok büyüktür. İstemci bağlantısı veya yayın hızı arttıkça giden ileti bant genişliği artar.

Aşağıdaki tabloda maksimum istemci bağlantıları, gelen/giden ileti sayısı ve bant genişliği özetlenmiştir.

|     Yayınla             | Birim1 | Ünite2 | Ünite5  | Ünite10 | Ünite20 | Ünite50  | Ünite100 |
|---------------------------|-------|-------|--------|--------|--------|---------|---------|
| Bağlantılar               | 1000 | 2.000 | 5.000  | 10,000 | 20.000 | 50.000  | 100.000 |
| Saniyede gelen iletiler  | 2     | 2     | 2      | 2      | 2      | 2       | 2       |
| Saniyede giden iletiler | 2.000 | 4.000 | 10,000 | 20.000 | 40,000 | 100.000 | 200,000 |
| Gelen bant genişliği  | 4 KBps   | 4 KBps   | 4 KBps    | 4 KBps    | 4 KBps    | 4 KBps     | 4 KBps     |
| Giden bant genişliği | 4 Mb/sn   | 8 Mb/sn   | 20 MBps   | 40 MBps   | 80 MBps   | 200 MBps   | 400 MBps   |

İleti gönderen yayın istemcileri en fazla dörttür. Gelen ileti miktarı küçük **olduğundan, yankı** ile karşılaştırıldığında daha az uygulama sunucusuna ihtiyaç duyarlar. Hem SLA hem de performans konuları için iki uygulama sunucusu yeterlidir. Ancak, özellikle Unit50 ve Unit100 için dengesizliği önlemek için varsayılan sunucu bağlantılarını artırmalısınız.

|   Yayınla      | Birim1 | Ünite2 | Ünite5 | Ünite10 | Ünite20 | Ünite50 | Ünite100 |
|------------------|-------|-------|-------|--------|--------|--------|---------|
| Bağlantılar      | 1000 | 2.000 | 5.000 | 10,000 | 20.000 | 50.000 | 100.000 |
| Uygulama sunucusu sayısı | 2     | 2     | 2     | 2      | 2      | 2      | 2       |

> [!NOTE]
> Azure SignalR Hizmeti'ne olası dengesiz sunucu bağlantılarından kaçınmak için varsayılan sunucu bağlantılarını her uygulama sunucusunda 5'ten 40'a çıkarın.
>
> İstemci bağlantı numarası, ileti boyutu, ileti gönderme hızı ve SKU katmanı **yayının**genel performansını etkiler.

#### <a name="send-to-group"></a>Gruba gönder

Grup kullanım **örneğine gönder'in** **yayınlanması**için benzer bir trafik deseni vardır. Aradaki fark, istemcilerin Azure SinyalR Hizmeti ile WebSocket bağlantıları kurduktan sonra, belirli bir gruba ileti gönderebilmeleri için gruplara katılmaları gerektiğidir. Aşağıdaki diyagram trafik akışını göstermektedir.

![Gruba gönderme kullanım örneği için trafik](./media/signalr-concept-performance/sendtogroup.png)

Grup üyesi ve grup sayısı performansı etkileyen iki faktörvardır. Çözümlemesi basitleştirmek için iki tür grup tanımlarız:

- **Küçük grup**: Her grubun 10 bağlantısı vardır. Grup numarası (maksimum bağlantı sayısı) / 10 eşittir. Örneğin, Birim1 için, 1.000 bağlantı sayısı varsa, o zaman 1000 / 10 = 100 grubu muz vardır.

- **Büyük grup**: Grup numarası her zaman 10'dur. Grup üye sayısı (maksimum bağlantı sayısı) / 10 eşittir. Örneğin, Birim1 için, 1.000 bağlantı sayısı varsa, her grubun 1000 / 10 = 100 üyesi vardır.

**Gruba gönder,** dağıtılmış bir veri yapısı aracılığıyla hedef bağlantıları bulması nedeniyle Azure Sinyal Hizmeti'ne yönlendirme maliyeti getirir. Gönderme bağlantıları arttıkça maliyet artar.

##### <a name="small-group"></a>Küçük grup

Yönlendirme maliyeti, birçok küçük gruba ileti göndermek için önemlidir. Şu anda, Azure Sinyal Hizmeti uygulaması Unit50'deki yönlendirme maliyet sınırına uymaktadır. Daha fazla CPU ve bellek eklemek yardımcı olmuyor, bu nedenle Unit100 tasarım la daha da geliştiremez. Daha fazla gelen bant genişliğine ihtiyacınız varsa, müşteri desteğine başvurun.

|   Küçük gruba gönder     | Birim1 | Ünite2 | Ünite5  | Ünite10 | Ünite20 | Ünite50 | Ünite100 |
|---------------------------|-------|-------|--------|--------|--------|--------|---------|
| Bağlantılar               | 1000 | 2.000 | 5.000  | 10,000 | 20.000 | 50.000 | 100.000
| Grup üye sayısı        | 10    | 10    | 10     | 10     | 10     | 10     | 10 
| Grup sayısı               | 100   | 200   | 500    | 1000  | 2.000  | 5.000  | 10,000 
| Saniyede gelen iletiler  | 200   | 400   | 1000  | 2,500  | 4.000  | 7,000  | 7,000   |
| Gelen bant genişliği  | 400 KBps  | 800 KBps  | 2 Mb/sn     | 5 Mb/sn     | 8 Mb/sn     | 14 MBps    | 14 MBps     |
| Saniyede giden iletiler | 2.000 | 4.000 | 10,000 | 25,000 | 40,000 | 70,000 | 70,000  |
| Giden bant genişliği | 4 Mb/sn    | 8 Mb/sn    | 20 MBps    | 50 MBps     | 80 MBps    | 140 MBps   | 140 MBps    |

Birçok istemci bağlantısı hub'ı aradığından, uygulama sunucusu numarası da performans için önemlidir. Aşağıdaki tabloda önerilen uygulama sunucusu sayımları listeleilmektedir.

|  Küçük gruba gönder   | Birim1 | Ünite2 | Ünite5 | Ünite10 | Ünite20 | Ünite50 | Ünite100 |
|------------------|-------|-------|-------|--------|--------|--------|---------|
| Bağlantılar      | 1000 | 2.000 | 5.000 | 10,000 | 20.000 | 50.000 | 100.000 |
| Uygulama sunucusu sayısı | 2     | 2     | 2     | 3      | 3      | 10     | 20      |

> [!NOTE]
> İstemci bağlantı numarası, ileti boyutu, ileti gönderme hızı, yönlendirme maliyeti, SKU katmanı ve uygulama sunucusunun CPU/belleği **küçük gruba göndermenin**genel performansını etkiler.

##### <a name="big-group"></a>Büyük grup

**Büyük gruba göndermek için,** giden bant genişliği yönlendirme maliyet sınırına çarpmadan önce darboğaz haline gelir. Aşağıdaki tabloda, **yayın**için neredeyse aynı olan maksimum giden bant genişliği listelenir.

|    Büyük gruba gönder      | Birim1 | Ünite2 | Ünite5  | Ünite10 | Ünite20 | Ünite50  | Ünite100 |
|---------------------------|-------|-------|--------|--------|--------|---------|---------|
| Bağlantılar               | 1000 | 2.000 | 5.000  | 10,000 | 20.000 | 50.000  | 100.000
| Grup üye sayısı        | 100   | 200   | 500    | 1000  | 2.000  | 5.000   | 10,000 
| Grup sayısı               | 10    | 10    | 10     | 10     | 10     | 10      | 10
| Saniyede gelen iletiler  | 20    | 20    | 20     | 20     | 20     | 20      | 20      |
| Gelen bant genişliği  | 80 KBps   | 40 KBps   | 40 KBps    | 20 KBps    | 40 KBps    | 40 KBps     | 40 KBps     |
| Saniyede giden iletiler | 2.000 | 4.000 | 10,000 | 20.000 | 40,000 | 100.000 | 200,000 |
| Giden bant genişliği | 8 Mb/sn    | 8 Mb/sn    | 20 MBps    | 40 MBps    | 80 MBps    | 200 MBps    | 400 MBps    |

Gönderme bağlantı sayısı en fazla 40'tır. Uygulama sunucusuüzerindeki yük küçük olduğundan, önerilen web uygulamaları sayısı küçüktür.

|  Büyük gruba gönder  | Birim1 | Ünite2 | Ünite5 | Ünite10 | Ünite20 | Ünite50 | Ünite100 |
|------------------|-------|-------|-------|--------|--------|--------|---------|
| Bağlantılar      | 1000 | 2.000 | 5.000 | 10,000 | 20.000 | 50.000 | 100.000 |
| Uygulama sunucusu sayısı | 2     | 2     | 2     | 2      | 2      | 2      | 2       |

> [!NOTE]
> Azure SignalR Hizmeti'ne olası dengesiz sunucu bağlantılarından kaçınmak için varsayılan sunucu bağlantılarını her uygulama sunucusunda 5'ten 40'a çıkarın.
> 
> İstemci bağlantı numarası, ileti boyutu, ileti gönderme hızı, yönlendirme maliyeti ve SKU katmanı **büyük gruba göndermenin**genel performansını etkiler.

#### <a name="send-to-connection"></a>Bağlantıya gönder

Bağlantı **ya gönderme** kullanım örneğinde, istemciler Azure Sinyal Hizmeti'ne bağlantılar kurduğunda, her istemci kendi bağlantı kimliklerini almak için özel bir hub çağırır. Performans ölçütü tüm bağlantı bağlantı bağlantılarını toplar, karıştırır ve gönderen hedef olarak tüm istemcilere yeniden atar. İstemciler, performans testi bitene kadar iletiyi hedef bağlantıya göndermeye devam eder.

![İstemci gönderme kullanım örneği için trafik](./media/signalr-concept-performance/sendtoclient.png)

**Bağlantıya gönderme** yönlendirme maliyeti, **küçük gruba gönderme maliyetine**benzer.

Bağlantı sayısı arttıkça, yönlendirme maliyeti genel performansı sınırlar. Birim50 sınırına ulaştı. Sonuç olarak, Unit100 daha fazla geliştiremez.

Aşağıdaki tablo, bağlantı kıyaslama **gönder** çalıştıran birçok tur sonra istatistiksel bir özettir.

|   Bağlantıya gönder   | Birim1 | Ünite2 | Ünite5 | Ünite10 | Ünite20 | Ünite50          | Ünite100         |
|------------------------------------|-------|-------|-------|--------|--------|-----------------|-----------------|
| Bağlantılar                        | 1000 | 2.000 | 5.000 | 10,000 | 20.000 | 50.000          | 100.000         |
| Saniyede gelen/giden iletiler | 1000 | 2.000 | 5.000 | 8,000  | 9,000  | 20.000 | 20.000 |
| Gelen/giden bant genişliği | 2 Mb/sn    | 4 Mb/sn    | 10 Mb/sn   | 16 MBps    | 18 MBps    | 40 MBps       | 40 MBps       |

Bu kullanım örneği, uygulama sunucusu tarafında yüksek yük gerektirir. Aşağıdaki tabloda önerilen uygulama sunucusu sayısına bakın.

|  Bağlantıya gönder  | Birim1 | Ünite2 | Ünite5 | Ünite10 | Ünite20 | Ünite50 | Ünite100 |
|------------------|-------|-------|-------|--------|--------|--------|---------|
| Bağlantılar      | 1000 | 2.000 | 5.000 | 10,000 | 20.000 | 50.000 | 100.000 |
| Uygulama sunucusu sayısı | 2     | 2     | 2     | 3      | 3      | 10     | 20      |

> [!NOTE]
> İstemci bağlantı numarası, ileti boyutu, ileti gönderme hızı, yönlendirme maliyeti, SKU katmanı ve uygulama sunucusu için CPU/bellek **bağlantıya göndermenin**genel performansını etkiler.

#### <a name="aspnet-signalr-echo-broadcast-and-send-to-small-group"></a>ASP.NET SignalR yankı, yayın ve küçük gruba göndermek

Azure SignalR Hizmeti, ASP.NET SignalR için aynı performans kapasitesini sağlar. 

Performans testi, ASP.NET SignalR için [Standart Hizmet Planı S3'teki](https://azure.microsoft.com/pricing/details/app-service/windows/) Azure Web Uygulamalarını kullanır.

Aşağıdaki tablo, SignalR **yankısı**için önerilen web uygulaması ASP.NET sayısını verir.

|   Yankı           | Birim1 | Ünite2 | Ünite5 | Ünite10 | Ünite20 | Ünite50 | Ünite100 |
|------------------|-------|-------|-------|--------|--------|--------|---------|
| Bağlantılar      | 1000 | 2.000 | 5.000 | 10,000 | 20.000 | 50.000 | 100.000 |
| Uygulama sunucusu sayısı | 2     | 2     | 4     | 4      | 8      | 32      | 40       |

Aşağıdaki tablo, ASP.NET SignalR **yayını**için önerilen web uygulaması sayısını verir.

|  Yayınla       | Birim1 | Ünite2 | Ünite5 | Ünite10 | Ünite20 | Ünite50 | Ünite100 |
|------------------|-------|-------|-------|--------|--------|--------|---------|
| Bağlantılar      | 1000 | 2.000 | 5.000 | 10,000 | 20.000 | 50.000 | 100.000 |
| Uygulama sunucusu sayısı | 2     | 2     | 2     | 2      | 2      | 2      | 2       |

Aşağıdaki tablo, SignalR'in **küçük bir gruba gönderdiği**ASP.NET için önerilen web uygulaması sayısını verir.

|  Küçük gruba gönder     | Birim1 | Ünite2 | Ünite5 | Ünite10 | Ünite20 | Ünite50 | Ünite100 |
|------------------|-------|-------|-------|--------|--------|--------|---------|
| Bağlantılar      | 1000 | 2.000 | 5.000 | 10,000 | 20.000 | 50.000 | 100.000 |
| Uygulama sunucusu sayısı | 2     | 2     | 4     | 4      | 8      | 32      | 40       |

### <a name="serverless-mode"></a>Sunucusuz mod

İstemciler ve Azure SinyalR Hizmeti sunucusuz modda yer almaktadır. Her müşteri tek bir bağlantı anlamına gelir. İstemci, REST API aracılığıyla başka bir istemciye ileti gönderir veya herkese ileti yayınlar.

REST API üzerinden yüksek yoğunluklu iletiler göndermek WebSocket kullanmak kadar verimli değildir. Her seferinde yeni bir HTTP bağlantısı oluşturmanızı gerektirir ve bu sunucusuz modda ekstra bir maliyettir.

#### <a name="broadcast-through-rest-api"></a>REST API üzerinden yayın
Tüm istemciler Azure SignalR Hizmeti ile WebSocket bağlantıları kurar. Sonra bazı istemciler REST API üzerinden yayın başlar. İleti gönderme (gelen) WebSocket ile karşılaştırıldığında verimli değildir HTTP Post, tüm geçer.

|   REST API üzerinden yayın     | Birim1 | Ünite2 | Ünite5  | Ünite10 | Ünite20 | Ünite50  | Ünite100 |
|---------------------------|-------|-------|--------|--------|--------|---------|---------|
| Bağlantılar               | 1000 | 2.000 | 5.000  | 10,000 | 20.000 | 50.000  | 100.000 |
| Saniyede gelen iletiler  | 2     | 2     | 2      | 2      | 2      | 2       | 2       |
| Saniyede giden iletiler | 2.000 | 4.000 | 10,000 | 20.000 | 40,000 | 100.000 | 200,000 |
| Gelen bant genişliği  | 4 KBps    | 4 KBps    | 4 KBps     | 4 KBps     | 4 KBps     | 4 KBps      | 4 KBps      |
| Giden bant genişliği | 4 Mb/sn    | 8 Mb/sn    | 20 MBps    | 40 MBps    | 80 MBps    | 200 MBps    | 400 MBps    |

#### <a name="send-to-user-through-rest-api"></a>REST API aracılığıyla kullanıcıya gönder
Kıyaslama, Azure SinyalR Hizmeti'ne bağlanmaya başlamadan önce tüm istemcilere kullanıcı adları atar. İstemciler Azure SinyalR Hizmeti ile WebSocket bağlantıları kurduktan sonra HTTP Post aracılığıyla başkalarına ileti göndermeye başlarlar.

|   REST API aracılığıyla kullanıcıya gönder | Birim1 | Ünite2 | Ünite5  | Ünite10 | Ünite20 | Ünite50  | Ünite100 |
|---------------------------|-------|-------|--------|--------|--------|---------|---------|
| Bağlantılar               | 1000 | 2.000 | 5.000  | 10,000 | 20.000 | 50.000  | 100.000 |
| Saniyede gelen iletiler  | 300   | 600   | 900    | 1,300  | 2.000  | 10,000  | 18.000  |
| Saniyede giden iletiler | 300   | 600   | 900    | 1,300  | 2.000  | 10,000  | 18.000 |
| Gelen bant genişliği  | 600 KBps  | 1.2 MBps  | 1.8 MBps   | 2.6 MBps   | 4 Mb/sn     | 10 Mb/sn     | 36 MBps    |
| Giden bant genişliği | 600 KBps  | 1.2 MBps  | 1.8 MBps   | 2.6 MBps   | 4 Mb/sn     | 10 Mb/sn     | 36 MBps    |

## <a name="performance-test-environments"></a>Performans testi ortamları

Daha önce listelenen tüm kullanım örnekleri için performans testlerini bir Azure ortamında gerçekleştirdik. En fazla 50 istemci VM ve 20 uygulama sunucusu VM kullandık. Aşağıda bazı ayrıntılar ve ayrıntılar veayrıntılar ve

- İstemci VM boyutu: StandardDS2V2 (2 vCPU, 7G bellek)

- Uygulama sunucusu VM boyutu: StandardF4sV2 (4 vCPU, 8G bellek)

- Azure SignalR SDK sunucu bağlantıları: 15

## <a name="performance-tools"></a>Performans araçları

Azure SinyalR Hizmeti için performans araçlarını [GitHub'da](https://github.com/Azure/azure-signalr-bench/)bulabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, tipik kullanım durumu senaryolarında Azure Sinyal Hizmeti performansına genel bir bakış elde elabilirsiniz.

Hizmetin dahili ve ölçekleme hakkında ayrıntılı bilgi almak için aşağıdaki kılavuzları okuyun:

* [Azure SignalR Hizmeti iç işlevleri](signalr-concept-internals.md)
* [Azure SignalR Hizmeti ölçekleme](signalr-howto-scale-multi-instances.md)
