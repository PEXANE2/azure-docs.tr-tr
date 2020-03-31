---
title: Akış Uç Noktaları (Başlangıç)
titleSuffix: Azure Media Services
description: İçeriği doğrudan bir istemci oynatıcı uygulamasına veya Bir İçerik Dağıtım Ağına (CDN) sunan dinamik bir paketleme ve akış hizmeti olan Akış Uç Noktaları (Origin) hakkında bilgi edinin.
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
ms.openlocfilehash: 1a2a370ac92ea3edf925d97af6f5e721c79d0d2d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79529705"
---
# <a name="streaming-endpoints-origin-in-azure-media-services"></a>Azure Medya Hizmetlerinde Akış Uç Noktaları (Başlangıç)

Microsoft Azure Medya Hizmetleri'nde, [Akış Bitiş Noktası,](https://docs.microsoft.com/rest/api/media/streamingendpoints) canlı ve isteğe bağlı içeriğinizi ortak akışlı ortam protokollerinden (HLS veya DASH) birini kullanarak doğrudan istemci oynatıcı uygulamasına sunabilen dinamik (tam zamanında) bir paketleme ve başlangıç hizmetini temsil eder. Buna ek olarak, **Akış Bitiş Noktası,** endüstri lideri DRM'lere dinamik (tam zamanında) şifreleme sağlar. 

Bir Medya Hizmetleri hesabı oluşturduğunuzda, durmuş durumda sizin için **varsayılan** bir Akış Bitiş Noktası oluşturulur. **Varsayılan** Akış Bitiş Noktasını silemezsiniz. Hesap altında daha fazla Akış Bitiş Noktası oluşturulabilir [(bkz. Kotalar ve sınırlamalar).](limits-quotas-constraints.md)

> [!NOTE]
> Video akışına başlamak için, videoyu akışı istediğiniz **Akış Bitiş Noktasını** başlatmanız gerekir.
>
> Yalnızca Akış Bitiş Noktanız çalışan durumdayken faturalandırılırsınız.

[Dinamik ambalaj](dynamic-packaging-overview.md) konusunu da gözden geçirdiğinizden emin olun. 

## <a name="naming-convention"></a>Adlandırma kuralı

Akış URL'sinin ana bilgisayar `{servicename}-{accountname}-{regionname}.streaming.media.azure.net`adı `servicename` biçimi: , where = akış uç noktası adı veya canlı olay adı.

Varsayılan akış bitiş noktası `servicename` nı kullanırken, URL şu `{accountname}-{regionname}.streaming.azure.net`şekilde atlanır: .

### <a name="limitations"></a>Sınırlamalar

* Akış uç noktası adı 24 karakter maksimum değeri vardır.
* Ad bu [regex](https://docs.microsoft.com/dotnet/standard/base-types/regular-expression-language-quick-reference) desen `^[a-zA-Z0-9]+(-*[a-zA-Z0-9])*$`takip etmelidir: .

## <a name="types"></a>Türler

İki **Akış Uç Nokta** türü vardır: **Standart** (önizleme) ve **Premium.** Tür, akış bitiş noktası için`scaleUnits`ayırdığınız ölçek birimi sayısına göre tanımlanır.

Türler aşağıdaki tabloda açıklanmıştır:

|Tür|Ölçek birimleri|Açıklama|
|--------|--------|--------|  
|**Standart**|0|Varsayılan Akış Bitiş Noktası **Standart** bir türdür ve ayarlayarak `scaleUnits`Premium türüne değiştirilebilir.|
|**Premium**|>0|**Premium** Akış Uç Noktaları gelişmiş iş yükleri ve özel ve ölçeklenebilir bant genişliği kapasitesi sağlamak için uygundur. (akış birimleri) ayarlayarak `scaleUnits` **Premium** türüne geçersiniz. `scaleUnits`200 Mbps'lik artışlarla satın alınabilen özel çıkış kapasitesi sağlar. **Premium** türünü kullanırken, etkinleştirilen her birim uygulamaya ek bant genişliği kapasitesi sağlar. |

> [!NOTE]
> Büyük internet hedef kitlelerine içerik sunmak isteyen müşteriler için, Akış Bitiş Noktası'nda CDN'yi etkinleştirmenizi öneririz.

SLA bilgileri için [Fiyatlandırma ve SLA'ya](https://azure.microsoft.com/pricing/details/media-services/)bakın.

## <a name="comparing-streaming-types"></a>Akış türlerini karşılaştırma

Özellik|Standart|Premium
---|---|---
Aktarım hızı |600 Mbps'ye kadar ve CDN kullanıldığında çok daha yüksek etkili bir iş çıkarımı sağlayabilir.|Akış birimi (SU) başına 200 Mbps. CDN kullanıldığında çok daha yüksek etkili bir iş elde edilebilir.
CDN|Azure CDN, üçüncü taraf CDN veya CDN yok.|Azure CDN, üçüncü taraf CDN veya CDN yok.
Faturalandırma eşit olarak eşit| Günlük|Günlük
Dinamik şifreleme|Evet|Evet
Dinamik paketleme|Evet|Evet
Ölçek|Otomatik ölçeklendirme hedeflenen iş sonuna kadar.|Ek SUs
IP filtreleme/G20/Özel ana bilgisayar <sup>1</sup>|Evet|Evet
Aşamalı indirme|Evet|Evet
Önerilen kullanım |Akış senaryolarının büyük çoğunluğu için önerilir.|Profesyonel kullanım.

<sup>1</sup> Yalnızca CDN bitiş noktasında etkinleştirilemiyorsa Doğrudan Akış Bitiş Noktası'nda kullanılır.<br/>

## <a name="streaming-endpoint-properties"></a>Akış Bitiş Noktası özellikleri

Bu bölümde, Akış Bitiş Noktası'nın bazı özellikleri hakkında ayrıntılı bilgi verilmiştir. Yeni bir akış bitiş noktası ve tüm özelliklerin açıklamaları oluşturma örnekleri için [Bkz.](https://docs.microsoft.com/rest/api/media/streamingendpoints/create)

- `accessControl`: Bu akış bitiş noktası için aşağıdaki güvenlik ayarlarını yapılandırmak için kullanılır: Akamai Signature Header Kimlik Doğrulama anahtarları ve bu bitiş noktasına bağlanmasına izin verilen IP adresleri. Bu özellik yalnızca false `cdnEnabled` olarak ayarlandığında ayarlanabilir.

- `cdnEnabled`: Bu akış bitiş noktası için Azure CDN tümleştirmesinin etkin olup olmadığını (varsayılan olarak devre dışı bırakıldığını) gösterir. Doğru ayarladıysanız, `cdnEnabled` aşağıdaki yapılandırmalar devre `customHostNames` dışı `accessControl`bırakılır: ve .

    Tüm veri merkezleri Azure CDN tümleştirmesini desteklemez. Veri merkezinizde Azure CDN tümleştirmesinin kullanılabilir olup olmadığını kontrol etmek için aşağıdaki adımları yapın:

  - Doğruyu ayarlamaya `cdnEnabled` çalış.
  - Döndürülen sonucu "Akış uç noktası CdnEtkin özelliği geçerli bölgede CDN özelliği kullanılamadığı için doğru olarak ayarlanamaz" iletisi içeren bir `HTTP Error Code 412` (PreconditionFailed) için denetleyin.

    Bu hatayı alırsanız, veri merkezi bunu desteklemez. Başka bir veri merkezi deneyin.

- `cdnProfile`: `cdnEnabled` Doğru ayarlandığında, değerleri de `cdnProfile` geçirebilirsiniz. `cdnProfile`CDN bitiş noktasının oluşturulacağı CDN profilinin adıdır. Varolan bir cdnProfile sağlayabilir veya yeni bir cdnProfile kullanabilirsiniz. Değer NULL ise `cdnEnabled` ve doğruysa, varsayılan değer "AzureMediaStreamingPlatformCdnProfile" kullanılır. Sağlanan `cdnProfile` zaten varsa, altında bir bitiş noktası oluşturulur. Profil yoksa, yeni bir profil otomatik olarak oluşturulur.
- `cdnProvider`: CDN etkinleştirildiğinde, değerleri `cdnProvider` de geçebilirsiniz. `cdnProvider`hangi sağlayıcının kullanılacağını kontrol eder. Şu anda üç değer desteklenir: "StandardVerizon", "PremiumVerizon" ve "StandardAkamai". Hiçbir değer sağlanmadıysa ve `cdnEnabled` doğruysa, "StandardVerizon" kullanılır (bu varsayılan değerdir).
- `crossSiteAccessPolicies`: Çeşitli istemciler için çapraz site erişim ilkelerini belirtmek için kullanılır. Daha fazla bilgi için, etki alanı ilkesi dosya belirtimine ve Bir Hizmeti Etki Alanı Sınırları Arasında Kullanılabilir Hale [Getirme'ye](https://www.adobe.com/devnet/articles/crossdomain_policy_file_spec.html) bakın. [Making a Service Available Across Domain Boundaries](https://msdn.microsoft.com/library/cc197955\(v=vs.95\).aspx) Ayarlar yalnızca Düzgün Akış için geçerlidir.
- `customHostNames`: Özel bir ana bilgisayar adına yönlendirilen trafiği kabul etmek için Akış Bitiş Noktası'nı yapılandırmak için kullanılır. Bu özellik Standart ve Premium Akış Bitiş Noktaları `cdnEnabled`için geçerlidir ve yanlış olduğunda ayarlanabilir.

    Etki alanı adının sahipliği Media Services tarafından onaylanmalıdır. Medya Hizmetleri, kullanımda etki alanına `CName` eklenecek bir bileşen olarak Medya Hizmetleri hesap kimliğini içeren bir kaydı nisbeten isteyerek etki alanı adı sahipliğini doğrular. Örneğin, akış bitiş noktası için özel bir ana bilgisayar adı olarak "sports.contoso.com" olarak kullanılabilmesi için, `<accountId>.contoso.com` bir kaydın Medya Hizmetleri doğrulama ana bilgisayar adlarından birine işaret edecek şekilde yapılandırılması gerekir. Doğrulama ana bilgisayar adı doğrulanmış nakışlardan oluşur. \<mediaservices-dns-zone>.

    Aşağıda, farklı Azure bölgeleri için doğrulama kaydında kullanılması beklenen DNS bölgeleri verecektir.
  
  - Kuzey Amerika, Avrupa, Singapur, Hong Kong SAR, Japonya:

    - `media.azure.net`
    - `verifydns.media.azure.net`

  - Çin:

    - `mediaservices.chinacloudapi.cn`
    - `verifydns.mediaservices.chinacloudapi.cn`

    Örneğin, "945a4c4e-28ea-45cd-8ccb-a519f6b700ad.contoso.com" ile "verifydns.media.azure.net" eşleyen bir `CName` kayıt, Medya Hizmetleri KIMLIĞI 945a4c4e-28ea-45cd-8ccb-a519f6b700ad'ın contoso.com alan adının mülkiyetine sahip olduğunu kanıtlayarak, contoso.com altındaki herhangi bir adın bu hesap altında bir akış bitiş noktası için özel bir ana bilgisayar adı olarak kullanılmasını sağlar. Medya Hizmeti Kimliği değerini bulmak için [Azure portalına](https://portal.azure.com/) gidin ve Medya Hizmeti hesabınızı seçin. **Hesap Kimliği** sayfanın sağ üst kısmında görünür.

    `CName` Kaydın doğru bir şekilde doğrulanması olmadan özel bir ana bilgisayar adı ayarlama girişimi varsa, DNS yanıtı başarısız olur ve bir süre önbelleğe alınır. Uygun bir kayıt yerleştirildikten sonra, önbelleğe alınmış yanıtın yeniden geçersiz kılınması biraz zaman alabilir. Özel etki alanı için DNS sağlayıcısına bağlı olarak, kaydın yeniden geçersiz kılınması birkaç dakika ile bir saat arasında değişir.

    `CName` Bu `<accountId>.<parent domain>` eşler ek `verifydns.<mediaservices-dns-zone>`olarak, özel `CName` ana bilgisayar adını (örneğin, `sports.contoso.com`) Medya Hizmetleri Akışı Bitiş Noktası'nın ana `amstest-usea.streaming.media.azure.net`bilgisayarı adı (örneğin,) eşleyen başka bir oluşturmanız gerekir.

    > [!NOTE]
    > Aynı veri merkezinde bulunan Akış Uç Noktaları aynı özel ana bilgisayar adını paylaşamaz.

    Şu anda, Medya Hizmetleri özel etki alanları ile SSL desteklemiyor.

- `maxCacheAge`- Varsayılan maksimum yaş HTTP önbellek denetim üstbilgisini, ortam parçaları ve isteğe bağlı bildirimlerüzerinde akış uç noktası tarafından ayarlanan geçersiz kılar. Değer saniye cinsinden ayarlanır.
- `resourceState` -

    - Durduruldu: oluşturulduktan sonra Bir Akış Bitiş Noktası'nın ilk durumu
    - Başlangıç: çalışan duruma geçiş
    - Çalışan: istemcilere içerik akışı yapabiliyor
    - Ölçekleme: ölçek birimleri artırılıyor veya azaltılıyor
    - Durdurma: durdurulan duruma geçiş
    - Silme: siliniyor

- `scaleUnits`: Size 200 Mbps'lik artışlarla satın alınabilen özel çıkış kapasitesi sağlar. **Premium** türüne geçmeniz gerekiyorsa, `scaleUnits`'yi ayarlayın.

## <a name="why-use-multiple-streaming-endpoints"></a>Neden birden çok akış uç noktası kullanıyor?

Tek bir akış bitiş noktası hem canlı hem de isteğe bağlı videoları aktarabilir ve çoğu müşteri yalnızca bir akış bitiş noktası kullanır. Bu bölümde, neden birden çok akış uç noktası kullanmanız gerekebileceğine dair bazı örnekler verilmiştir.

* Ayrılmış her birim 200 Mbps bant genişliği sağlar. 2.000 Mbps'den (2 Gbps) fazla bant genişliğine ihtiyacınız varsa, size ek bant genişliği sağlamak için ikinci akış bitiş noktasını ve yük dengesini kullanabilirsiniz.

    Ancak, CDN içerik akışı için ölçeklendirme elde etmek için en iyi yoldur ama CDN 2 Gbps'den fazla çekerek o kadar çok içerik teslim ediyorsanız o zaman ek akış bitiş noktaları (menşe) ekleyebilirsiniz. Bu durumda, iki akış uç noktası boyunca dengelenmiş içerik URL'leri dağıtmanız gerekir. Bu yaklaşım, her kökene rasgele istek göndermeye çalışmaktan daha iyi önbelleğe alma sağlar (örneğin, bir trafik yöneticisi aracılığıyla). 
    
    > [!TIP]
    > Genellikle CDN 2 Gbps'den fazla çekiyorsa, bir şey yanlış yapılandırılmış olabilir (örneğin, kaynak kalkanı yok).
    
* Farklı CDN sağlayıcıları nın dengelemesini yükleyin. Örneğin, Verizon CDN'yi kullanmak ve Akamai'yi kullanmak için ikinci bir akış bitiş noktası oluşturabilirsiniz. Sonra çoklu CDN dengeleme elde etmek için ikisi arasında bazı yük dengeleme ekleyin. 

    Ancak, müşteri genellikle tek bir menşe kullanarak birden çok CDN sağlayıcısı arasında yük dengeleme yapmak.
* Karışık içerik akışı: Canlı ve Video on Demand. 

    Canlı ve isteğe bağlı içerik için erişim şekilleri çok farklıdır. Canlı içerik aynı anda aynı içerik için çok fazla talep alma eğilimindedir. İsteğe bağlı video içeriği (örneğin uzun kuyruk arşivi içeriği) aynı içerikte düşük kullanıma sahiptir. Böylece önbelleğe alma canlı içerik üzerinde çok iyi çalışır ama uzun kuyruk içeriği de değil.

    Müşterilerinizin çoğunlukla canlı içeriği izlediği, ancak yalnızca zaman zaman isteğe bağlı içeriği izlediği ve aynı Akış Bitiş Noktası'ndan sunulduğu bir senaryo düşünün. İsteğe bağlı içeriğin düşük kullanımı, canlı içerik için daha iyi kaydedilecek önbellek alanı kaplar. Bu senaryoda, canlı içeriği bir Akış Bitiş Noktasından ve başka bir Akış Bitiş Noktasından gelen uzun kuyruk içeriğinden sunmanızı öneririz. Bu, canlı etkinlik içeriğinin performansını artırır.
    
## <a name="scaling-streaming-with-cdn"></a>CDN ile akış ölçekleme

Aşağıdaki makalelere bakın:

- [CDN'ye genel bakış](../../cdn/cdn-overview.md)
- [CDN ile akış ölçekleme](scale-streaming-cdn.md)

## <a name="ask-questions-and--get-updates"></a>Soru sorun ve güncellemeler alın

Soru sormanın, geri bildirimde bulunabilir ve Medya Hizmetleri hakkında güncellemeler alabildiğiniz farklı yolları görmek için [Azure Medya Hizmetleri topluluk](media-services-community.md) makalesine göz atın.

## <a name="see-also"></a>Ayrıca bkz.

[Dinamik paketleme](dynamic-packaging-overview.md)

## <a name="next-steps"></a>Sonraki adımlar

[Akış uç noktalarını yönetme](manage-streaming-endpoints-howto.md)
