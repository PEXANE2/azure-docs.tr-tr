---
title: Akış uç noktaları (başlangıç)
titleSuffix: Azure Media Services
description: Doğrudan bir istemci oynatıcı uygulamasına veya bir Content Delivery Network (CDN) içerik teslim eden bir dinamik paketleme ve akış hizmeti olan akış uç noktaları (Origin) hakkında bilgi edinin.
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
ms.openlocfilehash: 849d1187d6b854d48ad75ab1e55f600407420346
ms.sourcegitcommit: 7b25c9981b52c385af77feb022825c1be6ff55bf
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79251264"
---
# <a name="streaming-endpoints-origin-in-azure-media-services"></a>Azure Media Services akış uç noktaları (başlangıç)

Microsoft Azure Media Services, bir [akış uç noktası](https://docs.microsoft.com/rest/api/media/streamingendpoints) , canlı ve isteğe bağlı içeriğinizi ortak akış medya protokollerinden (HLS veya Dash) birini kullanarak doğrudan bir istemci oynatıcı uygulamasına teslim edebilen dinamik (tam zamanında) paketleme ve kaynak hizmetini temsil eder. Ayrıca, **akış uç noktası** sektör lideri drms için dinamik (tam zamanında) şifreleme sağlar.

Media Services bir hesap oluşturduğunuzda, bir durdurulmuş durumda sizin için **varsayılan** bir akış uç noktası oluşturulur. **Varsayılan** akış uç noktasını silemezsiniz. Hesap altında daha fazla akış uç noktası oluşturulabilir (bkz. [Kotalar ve sınırlamalar](limits-quotas-constraints.md)).

> [!NOTE]
> Video akışını başlatmak için videoyu akışını istediğiniz **akış uç noktasını** başlatmanız gerekir.
>
> Yalnızca akış uç noktanız çalışır durumdaysa faturalandırılırsınız.

## <a name="naming-convention"></a>Adlandırma kuralı

Akış URL 'sinin ana bilgisayar adı biçimi: `{servicename}-{accountname}-{regionname}.streaming.media.azure.net`, burada `servicename` = akış uç noktası adı veya canlı olay adı.

Varsayılan akış uç noktası kullanılırken, URL: `{accountname}-{regionname}.streaming.azure.net``servicename` atlanır.

### <a name="limitations"></a>Sınırlamalar

* Akış uç noktası adı, en fazla 24 karakter uzunluğunda bir değer içerir.
* Ad şu [Regex](https://docs.microsoft.com/dotnet/standard/base-types/regular-expression-language-quick-reference) düzenine uymalıdır: `^[a-zA-Z0-9]+(-*[a-zA-Z0-9])*$`.

## <a name="types"></a>Türler

İki **akış uç noktası** türü vardır: **Standart** (Önizleme) ve **Premium**. Tür, akış uç noktası için ayırdığınız ölçek birimi sayısıyla (`scaleUnits`) tanımlanır.

Türler aşağıdaki tabloda açıklanmıştır:

|Tür|Ölçek birimleri|Açıklama|
|--------|--------|--------|  
|**Standart**|0|Varsayılan akış uç noktası **Standart** bir türdür — `scaleUnits`ayarlanarak Premium türüne değiştirilebilir.|
|**Premium**|>0|**Premium** Akış uç noktaları, gelişmiş iş yükleri için uygundur ve adanmış ve ölçeklenebilir bant genişliği kapasitesi sağlar. `scaleUnits` (akış birimleri) ayarlayarak bir **Premium** türe geçebilirsiniz. `scaleUnits`, 200 Mbps 'lik artışlarla satın alınabilecek özel çıkış kapasitesi sağlar. **Premium** türü kullanılırken, etkinleştirilen her birim, uygulamaya ek bant genişliği kapasitesi sağlar. |

> [!NOTE]
> Büyük internet kitlelerine içerik teslim etmek isteyen müşteriler için, akış uç noktasında CDN 'yi etkinleştirmenizi öneririz.

SLA bilgileri için bkz. [fiyatlandırma ve SLA](https://azure.microsoft.com/pricing/details/media-services/).

## <a name="comparing-streaming-types"></a>Akış türlerini karşılaştırma

Özellik|Standart|Premium
---|---|---
Aktarım hızı |600 Mbps 'e kadar, bir CDN kullanıldığında çok daha yüksek bir verimlilik sağlar.|akış birimi başına 200 Mbps (SU). , Bir CDN kullanıldığında daha yüksek etkili bir verimlilik sağlayabilir.
CDN|Azure CDN, üçüncü taraf CDN veya CDN yok.|Azure CDN, üçüncü taraf CDN veya CDN yok.
Faturalandırma eşit olarak dağıtılır| Günlük|Günlük
Dinamik şifreleme|Yes|Yes
Dinamik paketleme|Yes|Yes
Ölçek|Hedeflenen işleme kadar otomatik olarak ölçeklendirin.|Ek SUs
IP filtreleme/G20/özel ana bilgisayar <sup>1</sup>|Yes|Yes
Aşamalı indirme|Yes|Yes
Önerilen kullanım |Akış senaryolarının çoğunluğu için önerilir.|Profesyonel kullanım.

<sup>1</sup> yalnızca CDN uç noktasında etkin olmadığında doğrudan akış uç noktasında kullanılır.<br/>

## <a name="streaming-endpoint-properties"></a>Akış uç noktası özellikleri

Bu bölüm bazı akış uç noktasının özellikleriyle ilgili ayrıntıları sağlar. Yeni bir akış uç noktası ve tüm özelliklerin açıklamalarını oluşturma örnekleri için bkz. [akış uç noktası](https://docs.microsoft.com/rest/api/media/streamingendpoints/create).

- `accessControl`: Bu akış uç noktası için şu güvenlik ayarlarını yapılandırmak için kullanılır: Akamai Imza üst bilgisi kimlik doğrulama anahtarları ve bu uç noktaya bağlanmasına izin verilen IP adresleri. Bu özellik yalnızca `cdnEnabled` false olarak ayarlandığında ayarlanabilir.

- `cdnEnabled`: Bu akış uç noktası için Azure CDN tümleştirmenin etkin olup olmadığını gösterir (varsayılan olarak devre dışı). `cdnEnabled` true olarak ayarlarsanız aşağıdaki yapılandırma devre dışı bırakılır: `customHostNames` ve `accessControl`.

    Tüm veri merkezleri Azure CDN tümleştirmeyi desteklemez. Veri merkezinizde Azure CDN tümleştirmesinin mevcut olup olmadığını denetlemek için aşağıdaki adımları uygulayın:

  - `cdnEnabled` true olarak ayarlamayı deneyin.
  - CDN özelliği geçerli bölgede kullanılamadığından, bir `HTTP Error Code 412` (PreconditionFailed) için döndürülen sonucu bir "akış uç noktası CdnEnabled özelliği true olarak ayarlanamaz" iletisiyle denetleyin. "

    Bu hatayı alırsanız veri merkezi tarafından desteklenmez. Başka bir veri merkezini deneyin.

- `cdnProfile`: `cdnEnabled` true olarak ayarlandığında `cdnProfile` değerleri de geçirebilirsiniz. `cdnProfile`, CDN uç nokta noktasının oluşturulacağı CDN profilinin adıdır. Var olan bir cdnProfile sağlayabilirsiniz veya yenisini kullanabilirsiniz. Değer NULL ise ve `cdnEnabled` true ise, "AzureMediaStreamingPlatformCdnProfile" varsayılan değeri kullanılır. Belirtilen `cdnProfile` zaten varsa, altında bir uç nokta oluşturulur. Profil yoksa, yeni bir profil otomatik olarak oluşturulur.
- `cdnProvider`: CDN etkinleştirildiğinde, `cdnProvider` değerlerini de geçirebilirsiniz. `cdnProvider` hangi sağlayıcının kullanılacağını denetler. Şu anda üç değer desteklenir: "StandardVerizon", "PremiumVerizon" ve "Standardadkamai". Değer sağlanmazsa ve `cdnEnabled` true ise, "StandardVerizon" kullanılır (varsayılan değer).
- `crossSiteAccessPolicies`: çeşitli istemciler için çapraz site erişim ilkeleri belirtmek için kullanılır. Daha fazla bilgi için bkz. [etki alanları arası ilke dosyası belirtimi](https://www.adobe.com/devnet/articles/crossdomain_policy_file_spec.html) ve [bir hizmetin etki alanı sınırları genelinde kullanılabilir hale getirilmesi](https://msdn.microsoft.com/library/cc197955\(v=vs.95\).aspx). Ayarlar yalnızca Kesintisiz Akış için geçerlidir.
- `customHostNames`: bir akış uç noktasını, özel bir ana bilgisayar adına yönlendirilmiş trafiği kabul edecek şekilde yapılandırmak için kullanılır. Bu özellik standart ve Premium akış uç noktaları için geçerlidir ve `cdnEnabled`: false olduğunda ayarlanabilir.

    Etki alanı adının sahipliğinin Media Services tarafından onaylanması gerekir. Media Services, kullanımda olan etki alanına eklenecek bir bileşen olarak Media Services hesap KIMLIĞINI içeren `CName` bir kayıt isteyerek etki alanı adı sahipliğini doğrular. Örnek olarak, "sports.contoso.com" için, akış uç noktası için özel bir ana bilgisayar adı olarak kullanılmak üzere, `<accountId>.contoso.com` için bir kayıt, Media Services doğrulama ana bilgisayar adlarından birine işaret etmek üzere yapılandırılmalıdır. Doğrulama ana bilgisayar adı verifydns ' den oluşur.\<mediaservices-DNS-Zone >.

    Aşağıda, farklı Azure bölgeleri için doğrulama kaydında kullanılacak beklenen DNS bölgeleri verilmiştir.
  
  - Kuzey Amerika, Avrupa, Singapur, Hong Kong ÖİB, Japonya:

    - `media.azure.net`
    - `verifydns.media.azure.net`

  - Arasında

    - `mediaservices.chinacloudapi.cn`
    - `verifydns.mediaservices.chinacloudapi.cn`

    Örneğin, "945a4c4e-28ea-45cd-8ccb-a519f6b700ad.contoso.com" öğesini "verifydns.media.azure.net" ile eşleyen bir `CName` kaydı, Media Services KIMLIĞI 945a4c4e-28ea-45CD-8ccb-a519f6b700ad 'nin contoso.com etki alanının sahipliğini olduğunu kanıtlar ve bu sayede contoso.com altında herhangi bir adın bu hesabın altındaki bir akış uç noktası için özel ana bilgisayar adı olarak kullanılmasını sağlar. Medya hizmeti KIMLIK değerini bulmak için [Azure Portal](https://portal.azure.com/) gidin ve medya hizmeti hesabınızı seçin. **Hesap kimliği** sayfanın sağ üst kısmında görünür.

    `CName` kaydının uygun bir şekilde doğrulanması gerekmeden özel bir ana bilgisayar adı ayarlamaya yönelik bir girişim varsa, DNS yanıtı başarısız olur ve sonra bir süre önbelleğe alınır. Uygun bir kayıt olduktan sonra, önbelleğe alınmış yanıt yeniden doğrulandıktan kadar biraz zaman alabilir. Özel etki alanı için DNS sağlayıcısına bağlı olarak, kaydın yeniden doğrulanması için birkaç dakika ile bir saate kadar her yerden zaman alır.

    `<accountId>.<parent domain>` `verifydns.<mediaservices-dns-zone>`eşleyen `CName` ek olarak, özel ana bilgisayar adını (örneğin, `sports.contoso.com`) Media Services akış uç noktasının ana bilgisayar adına (örneğin, `amstest-usea.streaming.media.azure.net`) eşleyen başka bir `CName` oluşturmanız gerekir.

    > [!NOTE]
    > Aynı veri merkezinde bulunan akış uç noktaları aynı özel ana bilgisayar adını paylaşamaz.

    Şu anda, Media Services özel etki alanlarıyla SSL 'yi desteklemez.

- `maxCacheAge`-medya parçalarında ve isteğe bağlı bildirimlerde akış uç noktası tarafından ayarlanan varsayılan en yüksek yaş HTTP önbelleği denetim üstbilgisini geçersiz kılar. Değer saniye cinsinden ayarlanır.
- `resourceState` -

    - Durduruldu: oluşturulduktan sonra akış uç noktasının başlangıç durumu
    - Başlatılıyor: çalışma durumuna geçiyor
    - Çalışıyor: istemcilere içerik akışı verebilir
    - Ölçeklendirme: ölçek birimleri artırıldı veya azaltılır
    - Durduruluyor: durdurulmuş duruma geçiyor
    - Silme: siliniyor

- `scaleUnits`: 200 Mbps 'lik artışlarla satın alınabilecek özel çıkış kapasitesi sağlar. **Premium** bir türe taşımanız gerekiyorsa `scaleUnits`ayarlayın.

## <a name="why-use-multiple-streaming-endpoints"></a>Neden birden çok akış uç noktası kullanılmalıdır?

Tek bir akış uç noktası hem canlı hem de isteğe bağlı videoları akışa alabilir ve çoğu müşteri yalnızca bir akış uç noktası kullanabilir. Bu bölümde, neden birden çok akış uç noktası kullanmanız gerektiği hakkında bazı örnekler verilmektedir.

* Her ayrılmış birim 200 Mbps bant genişliğine izin verir. 2\.000 MB/sn 'den fazla bant genişliği (2 Gbps) varsa, size ek bant genişliği sağlamak için ikinci akış uç noktasını ve yük dengeini kullanabilirsiniz.

    Ancak CDN, akış içeriği için ölçek genişletme gerçekleştirmenin en iyi yoludur, ancak CDN 'nin 2 GB 'tan fazla çıkarmakta olduğu çok sayıda içerik sunmanız durumunda ek akış uç noktaları (kaynaklar) ekleyebilirsiniz. Bu durumda, iki akış uç noktası üzerinden dengelenen içerik URL 'Lerini kullanıma açmanız gerekir. Bu yaklaşım, istekleri her kaynağa rastgele gönderme denemesi (örneğin, bir Traffic Manager aracılığıyla) için daha iyi önbelleğe alma olanağı sağlar. 
    
    > [!TIP]
    > Genellikle CDN 2 Gbps 'den fazla çekirse, bir şey yanlış yapılandırılmış olabilir (örneğin, kaynak koruma yoktur).
    
* Farklı CDN sağlayıcılarının yükünü dengeleyin. Örneğin, Verizon CDN 'yi kullanmak için varsayılan akış uç noktasını ayarlayabilir ve Akamai kullanmak için ikinci bir tane oluşturabilirsiniz. Daha sonra, çoklu CDN dengelemesi elde etmek için ikisi arasında bazı yük dengelemesi ekleyin. 

    Bununla birlikte, müşteri genellikle tek bir kaynak kullanarak birden çok CDN sağlayıcısı genelinde yük dengelemesi yapılır.
* Akıcı karışık içerik: canlı ve Isteğe bağlı video. 

    Canlı ve isteğe bağlı içerik için erişim desenleri çok farklıdır. Canlı içerik her seferinde aynı içerik için çok miktarda talep almayı eğilimindedir. İsteğe bağlı video içeriği (örneğin, örnek için uzun kuyruk Arşivi), aynı içerikte düşük kullanımdır. Böylece önbelleğe alma işlemi, canlı içerik üzerinde çok iyi sonuç verir, ancak uzun kuyruk içeriğini de içermez.

    Müşterilerinizin temel olarak canlı içerik izlerken, ancak yalnızca isteğe bağlı içerikleri izlerken ve aynı akış uç noktasından sunulduğunda bir senaryoyu düşünün. İsteğe bağlı içeriğin düşük kullanımı, canlı içerik için daha iyi kaydedilecek önbellek alanı kaplar. Bu senaryoda, bir akış uç noktasından canlı içeriği ve başka bir akış uç noktasından uzun kuyruk içeriğini sunduğumuz için önereceğiz. Bu, canlı olay içeriğinin performansını iyileştirir.
    
## <a name="scaling-streaming-with-cdn"></a>CDN ile akışı ölçeklendirme

Aşağıdaki makalelere bakın:

- [CDN genel bakış](../../cdn/cdn-overview.md)
- [CDN ile akış ölçeklendirme](scale-streaming-cdn.md)

## <a name="ask-questions-give-feedback-get-updates"></a>Soru sorun, geri bildirimde bulunun, güncelleştirmeleri al

Soru sormak, geri bildirimde bulunmak ve Media Services hakkında güncelleştirmeler almak için [Azure Media Services Community](media-services-community.md) makalesine göz atın.

## <a name="next-steps"></a>Sonraki adımlar

[Bu depodaki](https://github.com/Azure-Samples/media-services-v3-dotnet-quickstarts/blob/master/AMSV3Quickstarts/EncodeAndStreamFiles/Program.cs) örnek, .NET ile varsayılan akış uç noktasının nasıl başlatılacağını gösterir.
