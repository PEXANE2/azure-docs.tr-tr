---
title: CDN tümleştirmesi ile içeriği akışla aktarın
titleSuffix: Azure Media Services
description: CDN tümleştirmesi ile içerik akışı nın yanı sıra ön alma ve Origin-Assist CDN-Prefetch hakkında bilgi edinin.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 02/13/2020
ms.author: juliako
ms.openlocfilehash: 4ed8ada306720b7a8b44ddd59cefe399238c906a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80128061"
---
# <a name="stream-content-with-cdn-integration"></a>CDN tümleştirmesi ile içeriği akışla aktarın

Azure Content Delivery Network (CDN), dünya genelindeki stratejik olarak yerleştirilen fiziksel düğümlerde içeriği önbelleğe alarak, yüksek bant genişliği içeriğinin hızlı bir şekilde kullanıcılara teslimi konusunda geliştiricilere genel bir çözüm sunar.  

CDN, bir Media Services [Streaming Endpoint'ten (başlangıç)](streaming-endpoint-concept.md) codec başına, akış protokolü başına, bit hızı başına, kapsayıcı biçimi başına ve şifreleme/DRM başına akışlı içeriği önbelleğe alır. Codec-streaming protokol-kapsayıcı biçimi-bitrate-şifrelemeher birleşimi için ayrı bir CDN önbelleği olacaktır.

Popüler içerik, video parçası önbelleğe alındıkça doğrudan CDN önbelleğinden sunulur. Genellikle aynı şeyi izleyen çok sayıda kişi olduğundan, canlı içeriğin önbelleğe alınması olasıdır. İsteğe bağlı içerik biraz yanıltıcı olabilir, çünkü popüler, bazıları da popüler olmayan içeriklere sahip olabilir. Bunların hiçbirinin popüler olmadığı milyonlarca video varlığınız varsa (haftada sadece bir veya iki izleyici) ancak tüm farklı videoları izleyen binlerce kişi varsa, CDN çok daha az etkili hale gelir.

Ayrıca, uyarlanabilir akışın nasıl çalıştığını da göz önünde bulundurmanız gerekir. Her video parçası kendi varlığı olarak önbelleğe alınmış. Örneğin, belirli bir videonun ilk kez izlenişini düşünün. İzleyici burada ve orada sadece birkaç saniye izlerken etrafında atlar, yalnızca kişinin CDN önbelleğe almak ne izledim ile ilişkili video parçaları. Uyarlanabilir akış ile genellikle 5 ila 7 farklı bitrates video var. Bir kişi bir bitat izliyorsa ve diğeri farklı bir bithızı izliyorsa, her biri CDN'de ayrı ayrı önbelleğe alınmış demektir. İki kişi aynı bitrate izliyor olsa bile, farklı protokoller üzerinden akış olabilir. Her protokol (HLS, MPEG-DASH, Düzgün Akış) ayrı olarak önbelleğe alınr. Böylece her bit hızı ve protokol ayrı ayrı önbelleğe alınır ve yalnızca istenen video parçaları önbelleğe alınır.

Medya Hizmetleri [akışı bitiş noktasında](streaming-endpoint-concept.md)CDN'yi etkinleştirip etkinleştirmeye karar verirken, beklenen görüntüleyenlerin sayısını göz önünde bulundurun. CDN yalnızca içeriğiniz için çok sayıda görüntüleyen bekliyorsanız yardımcı olur. Görüntüleyenlerin maksimum eşzamanlılığı 500'den düşükse, CDN eşzamanlılıkla en iyi ölçeklendirildiklerinden CDN'nin devre dışı sayılması önerilir.

Bu konu [CDN tümleştirme](#enable-azure-cdn-integration)sağlayan tartışır. Ayrıca prefetching (etkin önbelleğe alma) ve [Origin-Assist CDN-Prefetch](#origin-assist-cdn-prefetch) kavramını açıklar.

## <a name="considerations"></a>Dikkat edilmesi gerekenler

* CDN'yi etkinleştirseniz de etkinleştirmeseniz de [akış bitiş noktası](streaming-endpoint-concept.md) `hostname` ve akış URL'si aynı kalır.
* İçeriğinizi CDN'li veya CDN'siz test etme yeteneğine ihtiyacınız varsa, CDN etkin olmayan başka bir akış bitiş noktası oluşturun.

## <a name="enable-azure-cdn-integration"></a>Azure CDN tümleştirmesini etkinleştirme

> [!IMPORTANT]
> Deneme veya öğrenci Azure hesapları için CDN'yi etkinleştiremezsiniz.
>
> CDN tümleştirmesi Federal Hükümet ve Çin bölgeleri dışındaki tüm Azure veri merkezlerinde etkinleştirilir.

Akış bitiş noktası CDN etkinleştirilmiş olarak sağlandıktan sonra, akış bitiş noktasını CDN bitiş ucuna eşlemek için DNS güncelleştirmesi yapılmadan önce Medya Hizmetleri'nde tanımlanmış bir bekleme süresi vardır.

Daha sonra CDN'yi devre dışı/etkinleştirmek istiyorsanız, akış bitiş noktanızın **durdurulmuş** durumda olması gerekir. Azure CDN tümleştirmesinin etkinleştirilmesi ve değişikliklerin tüm CDN AP'leri arasında etkin olması iki saat kadar sürebilir. Ancak, akış bitiş noktanızı başlatmave akış son noktasından kesintisiz olarak başlatabilirsiniz. Tümleştirme tamamlandıktan sonra akış CDN'den teslim edilir. Sağlama döneminde, akış bitiş noktanız **başlangıç** durumunda olacaktır ve bozulmuş performans gözlemleebilirsiniz.

Standart akış uç noktası oluşturulduğunda, Varsayılan olarak Standart Verizon ile yapılandırılır. Premium Verizon veya Standart Akamai sağlayıcılarını REST API'lerini kullanarak yapılandırabilirsiniz.

Azure CDN ile Azure Medya Hizmetleri entegrasyonu, standart akış uç noktaları için **Verizon'dan Azure CDN'de** uygulanır. Premium akış bitiş noktaları tüm **Azure CDN fiyatlandırma katmanları ve sağlayıcıları**kullanılarak yapılandırılabilir.

> [!NOTE]
> Azure CDN hakkında ayrıntılı bilgi için [CDN'ye genel bakış'a](../../cdn/cdn-overview.md)bakın.

## <a name="determine-if-a-dns-change-was-made"></a>DNS değişikliği yapIlip yapılmadığını belirleme

DNS değişikliğinin akış bitiş noktasında yapIlip yapılmadığını (trafik Azure CDN'sine yönlendiriliyor) kullanarak <https://www.digwebinterface.com>belirleyebilirsiniz. Sonuçlarda azureedge.net alan adlarını görürseniz, trafik şimdi CDN'ye işaret ediliyor.

## <a name="origin-assist-cdn-prefetch"></a>Origin-Assist CDN-Prefetch

CDN önbelleğe alma reaktif bir işlemdir. CDN bir sonraki nesnenin ne isteyeceğini tahmin edebilirse, CDN bir sonraki nesneyi proaktif olarak isteyebilir ve önbelleğe alabilir. Bu işlemle, tüm (veya çoğu) için performansı artıran bir önbellek isabet elde edebilirsiniz.

Prefetching kavramı, bu yakın oyuncu tarafından talep edilecek beklentisiyle "internet kenarında" nesneleri konumlandırmak için çaba, böylece oyuncuya bu nesneyi teslim etmek için zaman azaltarak.

Bu amaca ulaşmak için, akış uç noktası (başlangıç) ve CDN'nin birkaç şekilde el ele çalışması gerekir:

- Medya Hizmetleri menşei, CDN'yi bir sonraki nesneyi önceden getirmek için bilgilendirmek için "istihbarat" (Origin-Assist) olmalıdır.
- CDN prefetch ve önbelleğe alma (CDN-prefetch parçası) yok. CDN de bir prefetch ya da düzenli bir fetch olup olmadığını kökeni bilgilendirmek için "istihbarat" olması gerekir, 404 yanıtları işlemek, ve sonsuz prefetch döngü önlemek için bir yol.

### <a name="benefits"></a>Avantajlar

*Origin-Assist CDN-Prefetch* özelliğinin avantajları şunları içerir:

- Prefetch, oynatma sırasında beklenen video segmentlerini kenarda önceden konumlandırarak, izleyicinin gecikme süresini azaltarak ve video segmenti indirme sürelerini iyileştirerek video oynatma kalitesini artırır. Bu, daha hızlı video başlatma süresi ve daha düşük yeniden arabelleğe alma oluşumları ile sonuçlanır.
- Bu kavram genel CDN kökenli senaryo için geçerlidir ve medya ile sınırlı değildir.
- Akamai bu özelliği [Akamai Cloud Embed (ACE)](https://learn.akamai.com/en-us/products/media_delivery/cloud_embed.html)'ye ekledi.

> [!NOTE]
> Bu özellik, Medya Hizmetleri akış bitiş noktası ile entegre Akamai CDN için henüz geçerli değildir. Ancak, önceden varolan bir Akamai sözleşmesi olan ve Akamai CDN ile Medya Hizmetleri menşei arasında özel entegrasyon gerektiren Medya Hizmetleri müşterileri için kullanılabilir.

### <a name="how-it-works"></a>Nasıl çalışır?

`Origin-Assist CDN-Prefetch` Üstbilgi için CDN desteği (hem canlı hem de isteğe bağlı video akışı için) Akamai CDN ile doğrudan sözleşmesi olan müşteriler tarafından kullanılabilir. Bu özellik, Akamai CDN ve Medya Hizmetleri kökeni arasındaki aşağıdaki HTTP üstbilgi alışverişini içerir:

|HTTP üstbilgi|Değerler|Gönderen|Alıcı|Amaç|
| ---- | ---- | ---- | ---- | ----- |
|`CDN-Origin-Assist-Prefetch-Enabled` | 1 (varsayılan) veya 0 |CDN|Kaynak|CDN'nin ön getirme etkin olduğunu belirtmek için.|
|`CDN-Origin-Assist-Prefetch-Path`| Örnek: <br/>Parçalar(video=1400000000,format=mpd-time-cmaf)|Kaynak|CDN|CDN'ye ön alma yolu sağlamak için.|
|`CDN-Origin-Assist-Prefetch-Request`|1 (ön alma isteği) veya 0 (normal istek)|CDN|Kaynak|CDN'den gelen isteği belirtmek için bir ön getirmedir.|

Üstbilgi değişiminin bir bölümünü çalışırken görmek için aşağıdaki adımları deneyebilirsiniz:

1. Bir ses veya video bölümü veya parçası için Medya Hizmetleri'nin kaynağına istekte bulunmak için Postacı veya cURL'yi kullanın. İsteğe üstbilgi `CDN-Origin-Assist-Prefetch-Enabled: 1` eklediğinizden emin olun.
2. Yanıtta, değeri olarak göreceli `CDN-Origin-Assist-Prefetch-Path` bir yolu olan üstbilgi görmeniz gerekir.

### <a name="supported-streaming-protocols"></a>Desteklenen akış protokolleri

Özellik, `Origin-Assist CDN-Prefetch` canlı ve isteğe bağlı akış için aşağıdaki akış protokollerini destekler:

* HLS v3
* HLS v4
* HLS CMAF
* DASH (BOS)
* TIRE (CMAF)
* Düzgün akış

### <a name="faqs"></a>SSS

* CdN prefetch 404 almak için bir prefetch yolu URL geçersiz se, ne olur?

    CDN yalnızca 10 saniye (veya diğer yapılandırılmış değer) için bir 404 yanıtı önbelleğe alacak.

* İsteğe bağlı bir videonuz olduğunu varsayalım. CDN-prefetch etkinse, bu özellik, bir istemci ilk video segmentini istediğinde, sonraki tüm video segmentlerini aynı bithızında önceden getirmek için bir döngü başlatacağı anlamına gelir mi?

    Hayır, CDN-prefetch yalnızca istemci tarafından başlatılan bir istek/yanıttan sonra yapılır. CDN-prefetch, bir prefetch döngüsünden kaçınmak için hiçbir zaman bir prefetch tarafından tetiklenmez.

* Origin-Assist CDN-Prefetch özelliği her zaman açık mıdır? Nasıl açılabilir/kapatılabilir?

    Bu özellik varsayılan olarak kapalıdır. Müşterilerin Akamai API üzerinden açmaları gerekmektedir.

* Canlı akış için, bir sonraki bölüm veya parça henüz kullanılamıyorsa Origin-Assist'e ne olur?

    Bu durumda, Medya Hizmetleri kaynağı üstbilgi sağlamaz `CDN-Origin-Assist-Prefetch-Path` ve CDN-prefetch oluşmaz.

* Dinamik `Origin-Assist CDN-Prefetch` bildirim filtreleri ile nasıl çalışır?

    Bu özellik, bildirim filtresinden bağımsız olarak çalışır. Bir sonraki parça filtre penceresinden çıktığında, URL'si ham istemci bildirimine bakarak da bulunur ve cdn ön alma yanıtı üstbilgisi olarak döndürülür. Yani CDN DASH / HLS / Pürüzsüz tezahür filtrelenmiş bir parçanın URL alırsınız. Ancak, bu parça oyuncu tarafından tutulan DASH/HLS/Smooth manifestosuna dahil olmadığından (oyuncu o parçanın varlığını bilmez) bu parçayı almak için CDN'ye get isteğinde bulunmaz.

* DASH MPD/HLS çalma listesi/Düzgün manifesto önceden alınabilir mi?

    Hayır, DASH MPD, HLS ana çalma listesi, HLS varyant çalma listesi veya düzgün manifest URL prefetch üstbilgisine eklenmez.

* Prefetch URL'leri göreceli veya mutlak mı?

    Akamai CDN her ikisine de izin verirken, Medya Hizmetleri menşei yalnızca ön alma yolu için göreli URL'ler sağlar, çünkü mutlak URL'leri kullanmanın belirgin bir yararı yoktur.

* Bu özellik DRM korumalı içeriklerle çalışıyor mu?

    Evet, bu özellik HTTP düzeyinde çalıştığından, herhangi bir kesimi/parçayı çözmez veya ayrıştırmaz. İçeriğin şifrelenip şifrelenmemesi önemli değildir.

* Bu özellik Server Side Reklam Ekleme (SSAI) ile çalışır mı?
    
    SSAI, Medya Hizmetleri kökenli kaynak içeriğin zaman damgasını değiştirmediğinden, orijinal/ana içerik (reklam eklemeden önceki orijinal video içeriği) çalışır. Bu özelliğin reklam içeriğiyle çalışıp çalışmadığına bağlı olarak, reklam kaynağının Origin-Assist'i destekleyip desteklemediğine bağlıdır. Örneğin, reklam içeriği Azure Medya Hizmetleri'nde de (aynı veya ayrı kaynak) barındırılıyorsa, reklam içeriği de önceden alınır.

* Bu özellik UHD/HEVC içeriğinde çalışıyor mu?

    Evet.

## <a name="ask-questions-give-feedback-get-updates"></a>Soru sorun, geri bildirimde, güncellemeler alın

Soru sormanın, geri bildirimde bulunabilir ve Medya Hizmetleri hakkında güncellemeler alabildiğiniz farklı yolları görmek için [Azure Medya Hizmetleri topluluk](media-services-community.md) makalesine göz atın.

## <a name="next-steps"></a>Sonraki adımlar

* [Akış Bitiş Noktası (kaynak)](streaming-endpoint-concept.md) belgesini gözden geçirdiğinizden emin olun.
* Bu [depodaki](https://github.com/Azure-Samples/media-services-v3-dotnet-quickstarts/blob/master/AMSV3Quickstarts/EncodeAndStreamFiles/Program.cs) örnek, varsayılan akış bitiş noktasının .NET ile nasıl başlatılabildiğini gösterir.
