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
ms.openlocfilehash: c1e9be605a6f01695f2472ae76a9e5a786388aa0
ms.sourcegitcommit: 2823677304c10763c21bcb047df90f86339e476a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/14/2020
ms.locfileid: "77206115"
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

## <a name="properties"></a>Özellikler

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

## <a name="working-with-cdn"></a>CDN ile çalışma

Çoğu durumda CDN'yi etkinleştirmeniz gerekir. Ancak, 500 izleyicilerden en fazla eşzamanlılık benimsemeyi bekleme yapıyorsanız, CDN 'nin eşzamanlılık ile en iyi şekilde ölçeklendirilmiş olduğundan CDN 'yi devre dışı bırakmanız önerilir.

### <a name="considerations"></a>Dikkat edilmesi gerekenler

* Akış uç noktası `hostname` ve akış URL 'si, CDN 'yi etkinleştirip etkinleştirmeksizin aynı kalır.
* İçeriğinizi CDN ile veya CDN olmadan test etmek istiyorsanız, CDN etkin olmayan başka bir akış uç noktası oluşturun.

### <a name="detailed-explanation-of-how-caching-works"></a>Önbelleğe almanın nasıl çalıştığına ilişkin ayrıntılı açıklama

CDN 'nin etkinleştirildiği bir akış uç noktası için gereken bant genişliği miktarı değiştiğinden CDN eklenirken belirli bir bant genişliği değeri yoktur. Bir lot, içerik türüne, ne kadar popüler olduğuna, bitme ve protokollerden bağlıdır. CDN yalnızca istenen işlemleri önbelleğe alırken. Diğer bir deyişle, video parçası önbelleğe alındığı sürece popüler içeriğin CDN 'den doğrudan sunulması anlamına gelir. Tam olarak aynı şeyi izleyen çok sayıda kişi olduğu için canlı içeriğin önbelleğe alınması olasıdır. Çok daha popüler ve bazıları olmayan bazı içeriklere sahip olabileceğinden isteğe bağlı içerik biraz karmaşık olabilir. Bunlardan hiçbirinin popüler olduğu milyonlarca video varlığınız varsa (yalnızca bir veya iki Görüntüleyici), ancak tüm farklı videoları izlerken binlerce insan varsa, CDN çok daha az etkili hale gelir. Bu önbellek isabetsizliği ile akış uç noktasındaki yükü artırabilirsiniz.

Ayrıca, uyarlamalı akışın nasıl çalıştığını göz önünde bulundurmanız gerekir. Her tek video parçası kendi varlığı olarak önbelleğe alınır. Örneğin, belirli bir videonun ilk kez nasıl izlenen hakkında düşünün. Görüntüleyici burada yalnızca birkaç saniye izlemeyi atlar ve burada yalnızca, izlenen kişilerin CDN 'de önbelleğe alınması ile ilişkili video parçaları vardır. Uyarlamalı akış sayesinde genellikle 5 ile 7 arasında farklı bit fiyatları vardır. Bir kişi bir bit hızı izlerken ve başka bir kişi farklı bir bit hızı izlerse, her biri CDN 'de ayrı olarak önbelleğe alınır. İki kişi aynı bit hızını izliyor olsa da, farklı protokollerde akış olabilir. Her protokol (HLS, MPEG-DASH, Kesintisiz Akış) ayrı olarak önbelleğe alınır. Böylece her bit hızı ve protokol ayrı olarak önbelleğe alınır ve yalnızca istenen video parçaları önbelleğe alınır.

### <a name="enable-azure-cdn-integration"></a>Azure CDN tümleştirmeyi etkinleştir

> [!IMPORTANT]
> Deneme veya öğrenci Azure hesapları için CDN 'yi etkinleştiremezsiniz.
>
> CDN tümleştirmesi, Federal kamu ve Çin bölgeleri dışındaki tüm Azure veri merkezlerinde etkinleştirilir.

CDN etkinken bir akış uç noktası sağlandıktan sonra, akış uç noktasını CDN uç noktası ile eşlemek için DNS güncelleştirme işlemi yapılmadan önce Media Services tanımlı bir bekleme süresi vardır.

Daha sonra CDN 'yi devre dışı bırakmak/etkinleştirmek istiyorsanız, akış uç noktanızın **durdurulmuş** durumda olması gerekir. Azure CDN tümleştirmenin etkinleştirilmesi ve değişikliklerin tüm CDN pop 'larda etkin olması iki saate kadar sürebilir. Ancak, akış uç noktasından kesintiler olmadan akış uç noktanızı ve akışınızı başlatabilir ve tümleştirme tamamlandıktan sonra akış CDN 'den dağıtılır. Sağlama süresi boyunca, akış uç noktanız **Başlangıç** durumunda olur ve performans düşüklüğü gözlemleyebilirsiniz.

Standart akış uç noktası oluşturulduğunda, standart Verizon ile varsayılan olarak yapılandırılır. REST API 'Lerini kullanarak Premium Verizon veya standart Akamai sağlayıcılarını yapılandırabilirsiniz.

Azure CDN ile tümleştirme Azure Media Services standart akış uç noktaları için **Verizon 'tan Azure CDN** uygulanır. Premium akış uç noktaları, tüm **Azure CDN fiyatlandırma katmanları ve sağlayıcıları**kullanılarak yapılandırılabilir. 

> [!NOTE]
> Azure CDN hakkındaki ayrıntılar için bkz. [CDN 'ye genel bakış](../../cdn/cdn-overview.md).

### <a name="determine-if-dns-change-was-made"></a>DNS değişikliğinin yapıldığını belirleme

Bir akış uç noktasında DNS değişikliğinin (trafik Azure CDN yönlendirilmekte) https://www.digwebinterface.comkullanılarak saptanamıyor. Sonuçlarda azureedge.net etki alanı adları varsa, trafik artık CDN 'ye işaret ediyor.

## <a name="ask-questions-give-feedback-get-updates"></a>Soru sorun, geri bildirimde bulunun, güncelleştirmeleri al

Soru sormak, geri bildirimde bulunmak ve Media Services hakkında güncelleştirmeler almak için [Azure Media Services Community](media-services-community.md) makalesine göz atın.

## <a name="see-also"></a>Ayrıca bkz.

[CDN genel bakış](../../cdn/cdn-overview.md)

## <a name="next-steps"></a>Sonraki adımlar

[Bu depodaki](https://github.com/Azure-Samples/media-services-v3-dotnet-quickstarts/blob/master/AMSV3Quickstarts/EncodeAndStreamFiles/Program.cs) örnek, .NET ile varsayılan akış uç noktasının nasıl başlatılacağını gösterir.
