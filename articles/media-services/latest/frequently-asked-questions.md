---
title: Azure Media Services v3 sık sorulan sorular| Microsoft Dokümanlar
description: Bu makalede, Azure Medya Hizmetleri v3'e sık sorulan soruların yanıtları verebisin.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 03/18/2020
ms.author: juliako
ms.openlocfilehash: 11123ee04dd02a60dff0b88e2e6e85fcd613a7d5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80068003"
---
# <a name="media-services-v3-frequently-asked-questions"></a>Medya Hizmetleri v3 sık sorulan sorular

Bu makalede, Azure Medya Hizmetleri (AMS) v3 sık sorulan soruların yanıtlarını verir.

## <a name="general"></a>Genel

### <a name="what-azure-roles-can-perform-actions-on-azure-media-services-resources"></a>Hangi Azure rolleri Azure Medya Hizmetleri kaynaklarında eylem gerçekleştirebilir? 

[Medya Hizmetleri hesapları için Rol tabanlı erişim denetimine (RBAC)](rbac-overview.md)bakın.

### <a name="how-do-you-stream-to-apple-ios-devices"></a>Apple iOS aygıtlarına nasıl akış yaparsınız?

Akış kaynağı sunucusuna Apple iOS yerel cihazlarında tüketim için HLS içeriğini geri döndürmesini söylemek için yolun uzun sonunda (URL'nin "/manifest" bölümünden sonra) "(format=m3u8-aapl)" olduğundan emin olun (ayrıntılar [için, teslim içeriği).](dynamic-packaging-overview.md)

### <a name="how-do-i-configure-media-reserved-units"></a>Ortam Ayrılmış Birimleri nasıl yapılandırıyorum?

Media Services v3 veya Video Indexer ile tetiklenen Ses Analizi ve Video Analizi İşleri için hesabınıza 10 S3 MRU sağlamanız önerilir. 10'dan fazla S3 MRUs'a ihtiyacınız varsa, [Azure portalını](https://portal.azure.com/)kullanarak bir destek bileti açın.

Ayrıntılar için [CLI ile ölçeklendirme ortamı işleme](media-reserved-units-cli-how-to.md)ye bakın.

### <a name="what-is-the-recommended-method-to-process-videos"></a>Videoları işlemek için önerilen yöntem nedir?

Videoları kodlamak veya çözümlemek için ortak görevleri yapılandırmak için [Dönüşümler'i](https://docs.microsoft.com/rest/api/media/transforms) kullanın. Her **Dönüşüm,** video veya ses dosyalarınızı işlemek için bir reçete yi veya iş akışını açıklar. [İş,](https://docs.microsoft.com/rest/api/media/jobs) Medya Hizmetleri'nin **Transform'i** belirli bir giriş videosuna veya ses içeriğine uygulamak için gerçek isteğidir. Dönüşüm oluşturulduktan sonra, Medya Hizmetleri API'lerini veya yayımlanmış SDK'lardan herhangi birini kullanarak iş gönderebilirsiniz. Daha fazla bilgi için [Dönüşümler ve İşler'e](transforms-jobs-concept.md)bakın.

### <a name="i-uploaded-encoded-and-published-a-video-what-would-be-the-reason-the-video-does-not-play-when-i-try-to-stream-it"></a>Yükledim, kodladım ve bir video yayınladım. Akışı sağlamaya çalıştığımda videonun oynamamasının nedeni ne olur?

En yaygın nedenlerden biri, Çalışan durumunda oynatmaya çalıştığınız akış bitiş noktasına sahip olmadığınızdır.

### <a name="how-does-pagination-work"></a>Pagination nasıl çalışır?

Pagination kullanırken, her zaman toplama numaralandırmak için bir sonraki bağlantıyı kullanmalısınız ve belirli bir sayfa boyutuna bağlı değildir. Ayrıntılar ve örnekler için [bkz.](entities-overview.md)

### <a name="what-features-are-not-yet-available-in-azure-media-services-v3"></a>Azure Media Services v3'te henüz hangi özellikler mevcut değil?

Ayrıntılar için [v2 API'lerine ilişkin özellik boşluklarına](media-services-v2-vs-v3.md#feature-gaps-with-respect-to-v2-apis)bakın.

### <a name="what-is-the-process-of-moving-a-media-services-account-between-subscriptions"></a>Bir Medya Hizmetleri hesabını abonelikler arasında taşıma işlemi nedir?  

Ayrıntılar için bkz: [Medya Hizmetleri hesabını abonelikler arasında taşıma.](media-services-account-concept.md)

## <a name="live-streaming"></a>Canlı akış 

### <a name="how-to-stop-the-live-stream-after-the-broadcast-is-done"></a>Yayın bittikten sonra canlı yayın nasıl durdurulabilir?

İstemci tarafından veya sunucu tarafından yaklaşabilirsiniz.

#### <a name="client-side"></a>İstemci tarafı

Web uygulamanız, tarayıcıyı kapatıyorsa yayını sona erdirmek isteyen kullanıcıdan istekte olmalıdır. Bu, web uygulamanızın işleyebilir bir tarayıcı olaydır.

#### <a name="server-side"></a>Sunucu tarafı

Olay Izgara etkinliklerine abone olarak canlı olayları izleyebilirsiniz. Daha fazla bilgi için [eventgrid olay şemasına](media-services-event-schemas.md#live-event-types)bakın.

* [Microsoft.Media.LiveEventEncoderDisconnect edilebilen](media-services-event-schemas.md#liveeventencoderdisconnected) akış düzeyine [abone](reacting-to-media-services-events.md) olabilir ve canlı etkinliğinizi durdurmak ve silmek için bir süre yeniden bağlantı nın gelmediğini izleyebilirsiniz.
* Veya, parça düzeyinde kalp [atışı](media-services-event-schemas.md#liveeventingestheartbeat) olaylarına [abone](reacting-to-media-services-events.md) olabilirsiniz. Tüm parçalar da gelen bithızı 0'a düşüyorsa; veya son zaman damgası artık artmıyorsa, canlı etkinliği de güvenli bir şekilde kapatabilirsiniz. Kalp atışı olayları her parça için her 20 saniyede bir gelir, bu yüzden biraz daha ayrıntılı olabilir.

###  <a name="how-to-insert-breaksvideos-and-image-slates-during-live-stream"></a>Canlı akış sırasında molalar/videolar ve görüntü levhaları nasıl eklenir?

Media Services v3 canlı kodlama henüz canlı akış sırasında video veya görüntü kayrakları eklemeyi desteklemez. 

Kaynak videoyu değiştirmek için [şirket içinde canlı kodlayıcı](recommended-on-premises-live-encoders.md) kullanabilirsiniz. Telestream Wirecast, Switcher Studio (iOS' da), OBS Studio (ücretsiz uygulama) ve daha birçok uygulama gibi kaynaklar arasında geçiş yapma olanağı sağlar.

## <a name="content-protection"></a>İçerik koruma

### <a name="should-i-use-an-aes-128-clear-key-encryption-or-a-drm-system"></a>AES-128 net anahtar şifrelemesi mi yoksa DRM sistemi mi kullanmalıyım?

Müşteriler genellikle AES şifreleme si mi yoksa DRM sistemi mi kullanmaları gerektiğini merak ederler. İki sistem arasındaki temel fark, AES şifrelemesi ile içerik anahtarının TLS üzerinden istemciye iletilmesidir, böylece anahtar aktarım sırasında şifrelenir, ancak ek şifreleme ("açık"). Sonuç olarak, içeriğin şifresini çözmek için kullanılan anahtar istemci oynatıcı tarafından erişilebilir ve istemci üzerinde düz metin bir ağ izleme görüntülenebilir. AES-128 net anahtar şifrelemesi, görüntüleyenin güvenilir bir taraf olduğu kullanım durumları için uygundur (örneğin, çalışanlar tarafından görüntülenecek bir şirket içinde dağıtılan şirket videolarının şifrelenmesi).

PlayReady, Widevine ve FairPlay gibi DRM sistemlerinin tümü, AES-128 net tuşuile karşılaştırıldığında içeriğin şifresini çözmek için kullanılan anahtarda ek bir şifreleme düzeyi sağlar. İçerik anahtarı, TLS tarafından sağlanan tüm aktarım düzeyi şifrelemesine ek olarak DRM çalışma süresi tarafından korunan bir anahtara şifrelenir. Ayrıca, şifre çözme, kötü amaçlı bir kullanıcının saldırmasının daha zor olduğu işletim sistemi düzeyinde güvenli bir ortamda işlenir. DRM, görüntüleyenin güvenilir bir taraf olmadığı ve en yüksek düzeyde güvenlik gerektiren kullanım durumları için önerilir.

### <a name="how-to-show-a-video-only-to-users-who-have-a-specific-permission-without-using-azure-ad"></a>Azure AD kullanmadan yalnızca belirli bir izni olan kullanıcılara video nasıl gösterilir?

Belirli bir belirteç sağlayıcısı (Azure AD gibi) kullanmanız gerekmemektedir. Asimetrik anahtar şifrelemesini kullanarak kendi [JWT](https://jwt.io/) sağlayıcınızı (STS, Güvenli Belirteç Hizmeti olarak adlandırılır) oluşturabilirsiniz. Özel STS'nizde, iş mantığınıza dayalı talepler ekleyebilirsiniz.

İhraççının, hedef kitlenin ve taleplerinin tümü JWT'de olanla ContentKeyPolicy'de kullanılan ContentKeyPolicyRestriction arasında tam olarak eşleşti.

Daha fazla bilgi için [bkz.](content-protection-overview.md)

### <a name="how-and-where-to-get-jwt-token-before-using-it-to-request-license-or-key"></a>Lisans veya anahtar istemek için kullanmadan önce JWT belirteci nasıl ve nerede alınır?

1. Üretim için, BIR HTTPS isteği üzerine JWT belirteci sorunları bir Güvenli Belirteç Hizmetleri (STS) (web hizmeti) olması gerekir. Test için, **getTokenAsync** yöntemi [Program.cs](https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials/blob/master/AMSV3Tutorials/EncryptWithDRM/Program.cs)tanımlanan gösterilen kodu kullanabilirsiniz.
2. Oyuncu, bir kullanıcı nın kimliği doğrulandıktan sonra, böyle bir belirteç için STS'ye bir istekte bulunmalı ve belirteci değeri olarak atamak zorunda kalacaktır. Azure Media [Player API'sını](https://amp.azure.net/libs/amp/latest/docs/)kullanabilirsiniz.

* Simetrik ve asimetrik anahtarla STS çalıştıran bir [https://aka.ms/jwt](https://aka.ms/jwt)örnek için lütfen bkz. 
* Bu JWT belirteci kullanan Azure Media Player'ı [https://aka.ms/amtest](https://aka.ms/amtest) temel alan bir oynatıcı örneği player_settings için bkz.

### <a name="how-do-you-authorize-requests-to-stream-videos-with-aes-encryption"></a>AES şifrelemesi ile video akışı isteklerini nasıl yetkilendirin?

Doğru yaklaşım, STS'den (Güvenli Belirteç Hizmeti) yararlanmaktır:

STS'de, kullanıcı profiline bağlı olarak farklı talepler ekleyin ("Premium Kullanıcı", "Temel Kullanıcı", "Ücretsiz Deneme Kullanıcısı") gibi). Bir JWT farklı iddiaları ile, kullanıcı farklı içeriğini görebilirsiniz. Tabii ki, farklı içerik/varlık için ContentKeyPolicyRestriction ilgili Gerekli Taleplere sahip olacaktır.

Lisans/anahtar teslimini yapılandırmak ve varlıklarınızı şifrelemek için Azure Medya Hizmetleri API'lerini kullanın [(bu örnekte](https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials/blob/master/AMSV3Tutorials/EncryptWithAES/Program.cs)gösterildiği gibi).

Daha fazla bilgi için bkz.

- [İçerik koruma genel bakış](content-protection-overview.md)
- [Erişim denetimi ile çoklu DRM'ye sahip içerik koruma sistemi tasarlama](design-multi-drm-system-with-access-control.md)

### <a name="http-or-https"></a>HTTP veya HTTPS?
ASP.NET MVC oynatıcı uygulaması aşağıdakileri desteklemelidir:

* HTTPS kapsamında olan Azure AD aracılığıyla kullanıcı kimlik doğrulaması.
* Https altında olan istemci ve Azure AD arasında JWT alışverişi.
* Lisans teslimi Media Services tarafından sağlanıyorsa, istemci tarafından DRM lisans edinimi https altında olmalıdır. PlayReady ürün paketi, lisans teslimi için HTTPS'yi zorunlu kılmıyor. PlayReady lisans sunucunuz Medya Hizmetleri dışındaysa, HTTP veya HTTPS'yi kullanabilirsiniz.

ASP.NET player uygulaması HTTPS'yi en iyi uygulama olarak kullanır, bu nedenle Media Player HTTPS altında bir sayfadadır. Ancak, HTTP akış için tercih edilir, bu nedenle karışık içerik sorunu dikkate almanız gerekir.

* Tarayıcı karışık içeriğe izin vermez. Ama Silverlight ve OSMF eklentisi gibi eklentileri pürüzsüz ve DASH için izin yok. Karışık içerik, müşteri verilerinin risk altında olması nedeniyle kötü amaçlı JavaScript enjekte etme tehdidi nedeniyle bir güvenlik sorunudur. Tarayıcılar varsayılan olarak bu özelliği engeller. Bunun etrafında çalışmak için tek yolu sunucu (menşe) tarafında tüm etki (ne olursa olsun HTTPS veya HTTP) izin vererek. Bu da muhtemelen iyi bir fikir değil.
* Karışık içerikten kaçının. Hem oynatıcı uygulaması hem de Media Player HTTP veya HTTPS kullanmalıdır. Karışık içerik oynatırken, silverlightSS teknolojisi karışık içerikli bir uyarıyı temizlemeyi gerektirir. flashSS teknolojisi karışık içerik uyarısı olmadan karışık içeriği işler.
* Akış bitiş noktanız Ağustos 2014'te oluşturulduysa, HTTPS'yi desteklemez. Bu durumda, HTTPS için yeni bir akış bitiş noktası oluşturun ve kullanın.

### <a name="what-about-live-streaming"></a>Peki ya canlı yayın?

Bir programla ilişkili varlığı BIR VOD varlığı olarak ele alarak Medya Hizmetleri'nde canlı akışı korumak için tam olarak aynı tasarımı ve uygulamayı kullanabilirsiniz. Canlı içeriğin çoklu DRM koruması sağlamak için, Varlığı Canlı Çıktı ile ilişkilendirmeden önce varlık bir VOD varlığıymuş gibi aynı kurulum/işlemeyi Varlık'a uygulayın.

### <a name="what-about-license-servers-outside-media-services"></a>Media Services dışındaki lisans sunucuları ne olacak?

Genellikle, müşteriler kendi veri merkezlerinde veya DRM servis sağlayıcıları tarafından barındırılan bir lisans sunucusu çiftliğine yatırım yapar. Medya Hizmetleri içerik koruması ile karma modda çalışabilirsiniz. İçerikler Medya Hizmetleri'nde barındırılabilir ve dinamik olarak korunabilir, DRM lisansları ise Medya Hizmetleri dışındaki sunucular tarafından teslim edilir. Bu durumda, aşağıdaki değişiklikleri göz önünde bulundurun:

* STS'nin kabul edilebilir ve lisans sunucusu çiftliği tarafından doğrulanabilecek belirteçleri vermesi gerekir. Örneğin, Axinom tarafından sağlanan Widevine lisans sunucuları, bir yetkilendirme iletisi içeren belirli bir JWT gerektirir. Bu nedenle, böyle bir JWT sorunu bir STS olması gerekir. 
* Artık Medya Hizmetleri'nde lisans teslim hizmetini yapılandırmanız gerekmez. ContentKeyPolicies'i yapılandırırken lisans edinme URL'lerini (PlayReady, Widevine ve FairPlay için) sağlamanız gerekir.

> [!NOTE]
> Widevine, Google Inc. tarafından sağlanan ve Google, Inc.'in hizmet koşullarına ve Gizlilik Politikasına tabi olan bir hizmettir.

## <a name="media-services-v2-vs-v3"></a>Medya Hizmetleri v2 vs v3 

### <a name="can-i-use-the-azure-portal-to-manage-v3-resources"></a>V3 kaynaklarını yönetmek için Azure portalını kullanabilir miyim?

Şu anda [Azure portalını](https://portal.azure.com/) şu şekilde kullanabilirsiniz:

* Medya Hizmetleri v3 [Canlı Etkinlikler](live-events-outputs-concept.md)yönetmek , 
* görünümü (yönetilmez) v3 [Varlıklar](assets-concept.md), 
* [API'lara erişim hakkında bilgi alın.](access-api-portal.md) 

Diğer tüm yönetim görevleri için (örneğin, [Dönüşümler ve İşler ve](transforms-jobs-concept.md) [İçerik koruması),](content-protection-overview.md) [REST API,](https://docs.microsoft.com/rest/api/media/) [CLI](https://aka.ms/ams-v3-cli-ref)veya desteklenen [SDK'lardan](media-services-apis-overview.md#sdks)birini kullanın.

### <a name="is-there-an-assetfile-concept-in-v3"></a>V3'te AssetFile kavramı var mı?

Varlık Dosyaları, Medya Hizmetlerini Depolama SDK bağımlılığından ayırmak için AMS API'sinden kaldırıldı. Artık Depolama, Medya Hizmetleri değil, Depolama'ya ait bilgileri saklar. 

Daha fazla bilgi için bkz: [Medya Hizmetlerine Geçir v3.](media-services-v2-vs-v3.md)

### <a name="where-did-client-side-storage-encryption-go"></a>İstemci tarafı depolama şifrelemesi nereye gitti?

Artık sunucu tarafındaki depolama şifrelemesinin kullanılması önerilir (varsayılan olarak açık). Daha fazla bilgi [için, Veriler için Azure Depolama Hizmeti Şifrelemesi'ne](https://docs.microsoft.com/azure/storage/common/storage-service-encryption)bakın.

## <a name="next-steps"></a>Sonraki adımlar

[Medya Hizmetleri v3 genel bakış](media-services-overview.md)
