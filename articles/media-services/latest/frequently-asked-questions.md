---
title: Azure Media Services v3 sık sorulan sorular| Microsoft Dokümanlar
description: Bu makalede, Azure Medya Hizmetleri v3 hakkında sık sorulan soruların yanıtlarını verir.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 04/07/2020
ms.author: juliako
ms.openlocfilehash: 713acbd098255af2869d7a462c9990f3d7e10bf1
ms.sourcegitcommit: 7e04a51363de29322de08d2c5024d97506937a60
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/14/2020
ms.locfileid: "81309181"
---
# <a name="media-services-v3-frequently-asked-questions"></a>Medya Hizmetleri v3 sık sorulan sorular

Bu makalede, Azure Medya Hizmetleri v3 hakkında sık sorulan soruların yanıtlarını verir.

## <a name="general"></a>Genel

### <a name="what-azure-roles-can-perform-actions-on-azure-media-services-resources"></a>Hangi Azure rolleri Azure Medya Hizmetleri kaynaklarında eylem gerçekleştirebilir? 

[Medya Hizmetleri hesapları için Rol tabanlı erişim denetimine (RBAC)](rbac-overview.md)bakın.

### <a name="how-do-i-stream-to-apple-ios-devices"></a>Apple iOS aygıtlarına nasıl akış yaparım?

Yolunuzun sonunda (URL'nin **/manifest** bölümünden sonra) akış kaynağı sunucusuna Apple iOS yerel aygıtlarında tüketim için HTTP Live Streaming (HLS) içeriğini döndürmesini söylemek için **(format=m3u8-aapl)** olduğundan emin olun. Ayrıntılar için [bkz.](dynamic-packaging-overview.md)

### <a name="how-do-i-configure-media-reserved-units"></a>Ortam Ayrılmış Birimleri nasıl yapılandırıyorum?

Medya Hizmetleri v3 veya Video Indexer tarafından tetiklenen Ses Analizi ve Video Analizi işleri için, hesabınızı 10 S3 Media Ayrılmış Birim (MRUs) ile sağlamanızı öneririz. 10'dan fazla S3 MRUs'a ihtiyacınız varsa, [Azure portalını](https://portal.azure.com/)kullanarak bir destek bileti açın.

Ayrıntılar için [bkz.](media-reserved-units-cli-how-to.md)

### <a name="what-is-the-recommended-method-to-process-videos"></a>Videoları işlemek için önerilen yöntem nedir?

Videoları kodlamak veya çözümlemek için ortak görevleri yapılandırmak için [Dönüşümler'i](https://docs.microsoft.com/rest/api/media/transforms) kullanın. Her Dönüşüm, video veya ses dosyalarınızı işlemek için bir reçete yi veya iş akışını açıklar. [İş,](https://docs.microsoft.com/rest/api/media/jobs) Giriş videosuna veya ses içeriğine Dönüştür'ü uygulamak için Medya Hizmetleri'nin gerçek isteğidir. Dönüşüm oluşturulduktan sonra, Medya Hizmetleri API'lerini veya yayımlanmış SDK'lardan herhangi birini kullanarak İşler gönderebilirsiniz. Daha fazla bilgi için [Dönüşümler ve İşler'e](transforms-jobs-concept.md)bakın.

### <a name="i-uploaded-encoded-and-published-a-video-why-wont-the-video-play-when-i-try-to-stream-it"></a>Yükledim, kodladım ve bir video yayınladım. Akışı sağlamaya çalıştığımda video neden oynatılmıyor?

En yaygın nedenlerden biri, Çalışan durumunda oynatmaya çalıştığınız akış bitiş noktasına sahip olmadığınızdır.

### <a name="how-does-pagination-work"></a>Pagination nasıl çalışır?

Pagination kullanırken, koleksiyonu numaralandırmak ve belirli bir sayfa boyutuna bağlı olmamak için her zaman bir sonraki bağlantıyı kullanmalısınız. Ayrıntılar ve örnekler için [bkz.](entities-overview.md)

### <a name="what-features-are-not-yet-available-in-azure-media-services-v3"></a>Azure Media Services v3'te henüz hangi özellikler mevcut değil?

Ayrıntılar için [v2 API'lerine ilişkin Özellik boşluklarına](media-services-v2-vs-v3.md#feature-gaps-with-respect-to-v2-apis)bakın.

### <a name="what-is-the-process-of-moving-a-media-services-account-between-subscriptions"></a>Bir Medya Hizmetleri hesabını abonelikler arasında taşıma işlemi nedir?  

Ayrıntılar için bkz: [Medya Hizmetleri hesabını abonelikler arasında taşıma.](media-services-account-concept.md)

## <a name="live-streaming"></a>Canlı akış 

### <a name="how-do-i-stop-the-live-stream-after-the-broadcast-is-done"></a>Yayın bittikten sonra canlı yayını nasıl durduracağım?

İstemci tarafından veya sunucu tarafından yaklaşabilirsiniz.

#### <a name="client-side"></a>İstemci tarafı

Web uygulamanız, tarayıcıyı kapatırken yayını sona erdirmek isteyen kullanıcıdan istekte olmalıdır. Bu, web uygulamanızın işleyebilir bir tarayıcı olaydır.

#### <a name="server-side"></a>Sunucu tarafı

Azure Olay Ağı etkinliklerine abone olarak canlı etkinlikleri izleyebilirsiniz. Daha fazla bilgi için [EventGrid olay şemasına](media-services-event-schemas.md#live-event-types)bakın.

Şunlardan birini yapabilirsiniz:

* Akış düzeyindeki [Microsoft.Media.LiveEventEncoderDisconnected](media-services-event-schemas.md#liveeventencoderdisconnected) olaylarına [abone](reacting-to-media-services-events.md) olun ve canlı etkinliğinizi durdurmak ve silmek için bir süre yeniden bağlantı nın gelmediğini izleyin.
* Parça düzeyinde kalp [atışı](media-services-event-schemas.md#liveeventingestheartbeat) olaylarına [abone](reacting-to-media-services-events.md) olun. Tüm parçalarda 0'a düşen gelen bit hızı varsa veya son kez damga artmadıysa, canlı etkinliği güvenle kapatabilirsiniz. Kalp atışı olayları her parça için her 20 saniyede bir gelir, bu yüzden biraz ayrıntılı olabilir.

###  <a name="how-do-i-insert-breaksvideos-and-image-slates-during-a-live-stream"></a>Canlı akış sırasında molalar/videolar ve görüntü levhaları nasıl eklenir?

Media Services v3 canlı kodlama henüz canlı akış sırasında video veya görüntü kayrakları eklemeyi desteklemez. 

Kaynak videoyu değiştirmek için [şirket içinde canlı kodlayıcı](recommended-on-premises-live-encoders.md) kullanabilirsiniz. Telestream Wirecast, Switcher Studio (iOS'ta) ve OBS Studio (ücretsiz uygulama) gibi birçok uygulama kaynakları değiştirme olanağı sağlar.

## <a name="content-protection"></a>İçerik koruma

### <a name="should-i-use-aes-128-clear-key-encryption-or-a-drm-system"></a>AES-128 net anahtar şifreleme si mi kullanmalıyım?

Müşteriler genellikle AES şifreleme si mi yoksa DRM sistemi mi kullanmaları gerektiğini merak ederler. İki sistem arasındaki temel fark, AES şifrelemesi ile içerik anahtarının TLS üzerinden istemciye iletilmesidir, böylece anahtar aktarım sırasında şifrelenir, ancak ek şifreleme ("açık"). Sonuç olarak, içeriğin şifresini çözmek için kullanılan anahtar istemci oynatıcı tarafından erişilebilir ve istemci üzerinde düz metin bir ağ izleme görüntülenebilir. AES-128 net anahtar şifrelemesi, görüntüleyenin güvenilir bir taraf olduğu kullanım durumları için uygundur (örneğin, çalışanlar tarafından görüntülenecek bir şirket içinde dağıtılan şirket videolarının şifrelenmesi).

PlayReady, Widevine ve FairPlay gibi DRM sistemlerinin tümü, AES-128 net tuşuyla karşılaştırıldığında içeriğin şifresini çözmek için kullanılan anahtarda ek bir şifreleme düzeyi sağlar. İçerik anahtarı, TLS tarafından sağlanan tüm aktarım düzeyi şifrelemesine ek olarak DRM çalışma zamanı tarafından korunan bir anahtara şifrelenir. Ayrıca, şifre çözme, kötü amaçlı bir kullanıcının saldırmasının daha zor olduğu işletim sistemi düzeyinde güvenli bir ortamda işlenir. Görüntüleyicinin güvenilir bir taraf olmadığı ve en yüksek güvenlik düzeyine ihtiyacınız olduğu kullanım durumları için DRM öneririz.

### <a name="how-do-i-show-a-video-to-only-users-who-have-a-specific-permission-without-using-azure-ad"></a>Azure AD kullanmadan yalnızca belirli bir izni olan kullanıcılara videoyu nasıl gösteririm?

Azure Etkin Dizin (Azure AD) gibi belirli bir belirteç sağlayıcısı kullanmanız gerekmemektedir. Asimetrik anahtar şifrelemesini kullanarak kendi [JWT](https://jwt.io/) sağlayıcınızı (Güvenli Belirteç Hizmeti veya STS olarak adlandırılır) oluşturabilirsiniz. Özel STS'nizde, iş mantığınıza dayalı talepler ekleyebilirsiniz.

İhraççının, hedef kitlenin ve taleplerin intisatçısının JWT'de bulunanlarla kullanılan `ContentKeyPolicyRestriction` değer arasında tam olarak eşleştirdiğinden emin `ContentKeyPolicy`olun.

Daha fazla bilgi için [bkz.](content-protection-overview.md)

### <a name="how-and-where-did-i-get-a-jwt-token-before-using-it-to-request-a-license-or-key"></a>Nasıl ve nerede bir lisans veya anahtar istemek için kullanmadan önce bir JWT belirteci aldım?

Üretim için, HTTPS isteği üzerine JWT belirteci veren Güvenli Belirteç Hizmetine (diğer bir web hizmeti) sahip olmanız gerekir. Test için, [Program.cs'da](https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials/blob/master/AMSV3Tutorials/EncryptWithDRM/Program.cs)tanımlanan `GetTokenAsync` yöntemde gösterilen kodu kullanabilirsiniz.

Oyuncu, bir kullanıcı nın kimliği doğrulandıktan sonra, böyle bir belirteç için STS'ye bir istekte bulunarak belirteci değeri olarak atar. Azure Media [Player API'sını](https://amp.azure.net/libs/amp/latest/docs/)kullanabilirsiniz.

Simetrik bir anahtar veya asimetrik anahtarla STS çalıştırma örneği için [JWT aracına](https://aka.ms/jwt)bakın. Böyle bir JWT belirteci kullanan Azure Media Player'ı temel alan bir oynatıcı örneği için [Azure medya test aracına](https://aka.ms/amtest)bakın. (Belirteç girişini görmek için **player_settings** bağlantısını genişletin.)

### <a name="how-do-i-authorize-requests-to-stream-videos-with-aes-encryption"></a>AES şifrelemesi ile video akışı isteklerini nasıl yetkilendirebilirim?

Doğru yaklaşım, Güvenli Belirteç Hizmeti kullanmaktır. STS'de, kullanıcı profiline bağlı olarak farklı talepler ekleyin ("Premium Kullanıcı", "Temel Kullanıcı", "Ücretsiz Deneme Kullanıcısı") gibi). Bir JWT farklı iddiaları ile, kullanıcı farklı içeriğini görebilirsiniz. Farklı içerik ler `ContentKeyPolicyRestriction` veya varlıklar `RequiredClaims` için karşılık gelen değere sahip olacaktır.

Lisans/anahtar teslimini yapılandırmak ve varlıklarınızı şifrelemek için Azure Medya Hizmetleri API'lerini kullanın [(bu örnekte](https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials/blob/master/AMSV3Tutorials/EncryptWithAES/Program.cs)gösterildiği gibi).

Daha fazla bilgi için bkz.

- [İçerik koruma genel bakış](content-protection-overview.md)
- [Erişim denetimi ile çoklu DRM'ye sahip içerik koruma sistemi tasarlama](design-multi-drm-system-with-access-control.md)

### <a name="should-i-use-http-or-https"></a>HTTP veya HTTPS kullanmalı mıyım?
ASP.NET MVC oynatıcı uygulaması aşağıdakileri desteklemelidir:

* HTTPS kapsamında olan Azure AD aracılığıyla kullanıcı kimlik doğrulaması.
* Https altında olan istemci ve Azure AD arasında JWT alışverişi.
* Lisans teslimi Media Services tarafından sağlanıyorsa, istemci tarafından DRM lisans edinimi https altında olmalıdır. PlayReady ürün paketi, lisans teslimi için HTTPS'yi zorunlu kılmıyor. PlayReady lisans sunucunuz Medya Hizmetleri dışındaysa, HTTP veya HTTPS'yi kullanabilirsiniz.

ASP.NET player uygulaması HTTPS'yi en iyi uygulama olarak kullanır, bu nedenle Media Player HTTPS altında bir sayfadadır. Ancak, HTTP akış için tercih edilir, bu nedenle karışık içerik ile bu sorunları dikkate almanız gerekir:

* Tarayıcı karışık içeriğe izin vermez. Ancak Silverlight ve Smooth ve DASH için OSMF eklentisi gibi eklentiler buna izin verir. Karışık içerik, müşteri verilerini riske atabilen kötü amaçlı JavaScript enjekte etme tehdidi nedeniyle bir güvenlik sorunudur. Tarayıcılar varsayılan olarak bu özelliği engeller. Bunun etrafında çalışmak için tek yolu sunucu (menşe) tarafında tüm etki (ne olursa olsun HTTPS veya HTTP) izin vererek. Bu da muhtemelen iyi bir fikir değil.
* Karışık içerikten kaçının. Hem oynatıcı uygulaması hem de Media Player HTTP veya HTTPS kullanmalıdır. Karışık içerik oynatırken, SilverlightSS teknolojisi karışık içerikli bir uyarıyı temizlemeyi gerektirir. FlashSS teknolojisi karışık içerik uyarısı olmadan karışık içeriği işler.
* Akış bitiş noktanız Ağustos 2014'te oluşturulduysa, HTTPS'yi desteklemez. Bu durumda, HTTPS için yeni bir akış bitiş noktası oluşturun ve kullanın.

### <a name="what-about-live-streaming"></a>Peki ya canlı yayın?

Bir programla ilişkili varlığı BIR VOD varlığı olarak ele alarak Medya Hizmetleri'nde canlı akışı korumaya yardımcı olmak için tam olarak aynı tasarımı ve uygulamayı kullanabilirsiniz. Canlı içeriğin çoklu DRM koruması sağlamak için, varlığı canlı çıktıyla ilişkilendirmeden önce aynı kurulum/işlemeyi bir VOD varlığı gibi varlığa uygulayın.

### <a name="what-about-license-servers-outside-media-services"></a>Media Services dışındaki lisans sunucuları ne olacak?

Müşteriler genellikle bir lisans sunucusu çiftliğine kendi veri merkezlerinde veya DRM servis sağlayıcıları tarafından barındırılan bir çiftlikte yatırım yapmıştır. Medya Hizmetleri içerik koruması ile karma modda çalışabilirsiniz. İçerik Medya Hizmetlerinde barındırılabilir ve dinamik olarak korunabilir, DRM lisansları ise Medya Hizmetleri dışındaki sunucular tarafından teslim edilir. Bu durumda, aşağıdaki değişiklikleri göz önünde bulundurun:

* STS'nin kabul edilebilir ve lisans sunucusu çiftliği tarafından doğrulanabilecek belirteçleri vermesi gerekir. Örneğin, Axinom tarafından sağlanan Widevine lisans sunucuları, bir yetkilendirme iletisi içeren belirli bir JWT gerektirir. Böyle bir JWT sorunu için bir STS olması gerekir. 
* Artık Medya Hizmetleri'nde lisans teslim hizmetini yapılandırmanız gerekmez. Yapılandırma yaparken lisans edinme URL'lerini (PlayReady, Widevine ve FairPlay için) sağlamanız `ContentKeyPolicy`gerekir.

> [!NOTE]
> Widevine, Google tarafından sağlanan ve Google'ın hizmet ve gizlilik politikasına tabi olan bir hizmettir.

## <a name="media-services-v2-vs-v3"></a>Medya Hizmetleri v2 vs v3 

### <a name="can-i-use-the-azure-portal-to-manage-v3-resources"></a>V3 kaynaklarını yönetmek için Azure portalını kullanabilir miyim?

Şu anda [Azure portalını](https://portal.azure.com/) şu şekilde kullanabilirsiniz:

* Medya Hizmetleri v3 [Canlı Etkinlikler](live-events-outputs-concept.md) yönetin. 
* V3 [varlıklarını](assets-concept.md)görüntüleyin (yönetilme. 
* [API'lara erişim hakkında bilgi alın.](access-api-portal.md) 

Diğer tüm yönetim görevleri için (örneğin, [Dönüşümler ve İşler](transforms-jobs-concept.md) ve [içerik koruması),](content-protection-overview.md) [REST API'sını](https://docs.microsoft.com/rest/api/media/), [Azure CLI'yi](https://aka.ms/ams-v3-cli-ref)veya desteklenen [SDK'lardan](media-services-apis-overview.md#sdks)birini kullanın.

### <a name="is-there-an-assetfile-concept-in-v3"></a>V3'te AssetFile kavramı var mı?

Kavram, `AssetFile` Medya Hizmetlerini Depolama SDK bağımlılığından ayırmak için Medya Hizmetleri API'sinden kaldırıldı. Artık Medya Hizmetleri değil, Azure Depolama, Depolama SDK'ya ait bilgileri saklar. 

Daha fazla bilgi için bkz: [Medya Hizmetlerine Geçir v3.](media-services-v2-vs-v3.md)

### <a name="where-did-client-side-storage-encryption-go"></a>İstemci tarafı depolama şifrelemesi nereye gitti?

Şimdi sunucu tarafındaki depolama şifrelemesini kullanmanızı öneririz (varsayılan olarak açık). Daha fazla bilgi [için, veriler için Azure Depolama Hizmeti Şifrelemesi'ne](https://docs.microsoft.com/azure/storage/common/storage-service-encryption)bakın.

## <a name="offline-streaming"></a>Çevrimdışı akış

### <a name="fairplay-streaming-for-ios"></a>iOS için FairPlay Akışı

Aşağıdaki sık sorulan sorular iOS için çevrimdışı FairPlay akışı sorun giderme ile yardımcı sağlar.

#### <a name="why-does-only-audio-play-but-not-video-during-offline-mode"></a>Çevrimdışı modda neden yalnızca ses çalınmıyor, video oynanmıyor?

Bu davranış örnek uygulamanın tasarımı ile gibi görünüyor. Çevrimdışı modda alternatif bir ses parçası (HLS için geçerli olan) olduğunda, hem iOS 10 hem de iOS 11 varsayılan olarak alternatif ses parçasına geçer. FPS çevrimdışı modu için bu davranışı telafi etmek için, alternatif ses parçasını akıştan kaldırın. Medya Hizmetleri'nde bunu yapmak için, dinamik bildirim filtresi **ses-only=false**ekleyin. Başka bir deyişle, hls URL **.ism/manifest(format=m3u8-aapl,audio-only=false)** ile sona erer. 

#### <a name="why-does-it-still-play-audio-only-without-video-during-offline-mode-after-i-add-audio-onlyfalse"></a>Ben yalnızca sesli=false ekledikten sonra neden çevrimdışı modda yalnızca video olmadan ses oynatmıyor?

İçerik dağıtım ağı için önbellek anahtarı tasarımına bağlı olarak, içerik önbelleğe alınabilir. Önbelleği temizle.

#### <a name="is-fps-offline-mode-supported-on-ios-11-in-addition-to-ios-10"></a>FPS çevrimdışı modu iOS 11'de iOS 10'a ek olarak da desteklenir mi?

Evet. FPS çevrimdışı modu iOS 10 ve iOS 11 için desteklenir.

#### <a name="why-cant-i-find-the-document-offline-playback-with-fairplay-streaming-and-http-live-streaming-in-the-fps-server-sdk"></a>FPS Server SDK'da neden "FairPlay Streaming ile Çevrimdışı Oynatma ve HTTP Live Streaming" belgesini bulamıyorum?

FPS Server SDK sürüm 4'ten bu yana, bu belge "FairPlay Akış Programlama Kılavuzu" ile birleştirilmiştir.

#### <a name="what-is-the-downloadedoffline-file-structure-on-ios-devices"></a>iOS aygıtlarında indirilen/çevrimdışı dosya yapısı nedir?

iOS aygıtında indirilen dosya yapısı aşağıdaki ekran görüntüsüne benzer. Klasör, `_keys` her lisans hizmeti ana bilgisayarı için bir mağaza dosyasıyla birlikte FPS lisanslarını karşıdan yükledi. Klasör `.movpkg` ses ve video içeriğini depolar. 

Bir çizgi ile biten bir ad ile ilk klasör bir sayı ardından video içeriği içerir. Sayısal değer, video yorumlamalarının en yüksek bant genişliğidir. Bir çizgi ve ardından 0 ile biten bir ada sahip ikinci klasör ses içeriği içerir. Adlandırılmış `Data` üçüncü klasör FPS içeriğinin ana çalma listesini içerir. Son olarak, boot.xml klasör `.movpkg` içeriğinin tam bir açıklamasını sağlar. 

![FairPlay iOS örnek uygulaması için çevrimdışı dosya yapısı](media/offline-fairplay-for-ios/offline-fairplay-file-structure.png)

Burada örnek boot.xml dosyası:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<HLSMoviePackage xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance" xmlns="http://apple.com/IMG/Schemas/HLSMoviePackage" xsi:schemaLocation="http://apple.com/IMG/Schemas/HLSMoviePackage /System/Library/Schemas/HLSMoviePackage.xsd">
  <Version>1.0</Version>
  <HLSMoviePackageType>PersistedStore</HLSMoviePackageType>
  <Streams>
    <Stream ID="1-4DTFY3A3VDRCNZ53YZ3RJ2NPG2AJHNBD-0" Path="1-4DTFY3A3VDRCNZ53YZ3RJ2NPG2AJHNBD-0" NetworkURL="https://willzhanmswest.streaming.mediaservices.windows.net/e7c76dbb-8e38-44b3-be8c-5c78890c4bb4/MicrosoftElite01.ism/QualityLevels(127000)/Manifest(aac_eng_2_127,format=m3u8-aapl)">
      <Complete>YES</Complete>
    </Stream>
    <Stream ID="0-HC6H5GWC5IU62P4VHE7NWNGO2SZGPKUJ-310656" Path="0-HC6H5GWC5IU62P4VHE7NWNGO2SZGPKUJ-310656" NetworkURL="https://willzhanmswest.streaming.mediaservices.windows.net/e7c76dbb-8e38-44b3-be8c-5c78890c4bb4/MicrosoftElite01.ism/QualityLevels(161000)/Manifest(video,format=m3u8-aapl)">
      <Complete>YES</Complete>
    </Stream>
  </Streams>
  <MasterPlaylist>
    <NetworkURL>https://willzhanmswest.streaming.mediaservices.windows.net/e7c76dbb-8e38-44b3-be8c-5c78890c4bb4/MicrosoftElite01.ism/manifest(format=m3u8-aapl,audio-only=false)</NetworkURL>
  </MasterPlaylist>
  <DataItems Directory="Data">
    <DataItem>
      <ID>CB50F631-8227-477A-BCEC-365BBF12BCC0</ID>
      <Category>Playlist</Category>
      <Name>master.m3u8</Name>
      <DataPath>Playlist-master.m3u8-CB50F631-8227-477A-BCEC-365BBF12BCC0.data</DataPath>
      <Role>Master</Role>
    </DataItem>
  </DataItems>
</HLSMoviePackage>
```

### <a name="widevine-streaming-for-android"></a>Android için Widevine akışı

#### <a name="how-can-i-deliver-persistent-licenses-offline-enabled-for-some-clientsusers-and-non-persistent-licenses-offline-disabled-for-others-do-i-have-to-duplicate-the-content-and-use-separate-content-keys"></a>Bazı istemciler/kullanıcılar için kalıcı lisansları (çevrimdışı etkin) ve diğerleri için kalıcı olmayan lisansları (çevrimdışı devre dışı) nasıl teslim edebilirim? İçeriği çoğaltmak ve ayrı içerik anahtarları kullanmak zorunda mıyım?

Media Services v3 bir varlığın `StreamingLocator` birden çok örneğine sahip olmasını sağladığından, şunları yapabilirsiniz:

* Bir `ContentKeyPolicy` örnek `license_type = "persistent"` `ContentKeyPolicyRestriction` ile , `"persistent"`üzerinde `StreamingLocator`iddia ile , ve onun .
* Başka `ContentKeyPolicy` bir `license_type="nonpersistent"` `ContentKeyPolicyRestriction` örnek ile `"nonpersistent`, üzerinde `StreamingLocator`iddia ile ", ve onun .
* Farklı `StreamingLocator` `ContentKey` değerlere sahip iki örnek.

Özel STS iş mantığına bağlı olarak, farklı talepler JWT belirteci verilir. Belirteç ile yalnızca ilgili lisans alınabilir ve yalnızca ilgili URL oynatılabilir.

#### <a name="what-is-the-mapping-between-the-widevine-and-media-services-drm-security-levels"></a>Widevine ve Medya Hizmetleri DRM güvenlik düzeyleri arasındaki haritalama nedir?

Google'ın "Widevine DRM Architecture Overview" üç güvenlik düzeyi tanımlar. Ancak, [Widevine lisans şablonundaki Azure Medya Hizmetleri belgeleri](widevine-license-template-overview.md) beş güvenlik düzeyi (oynatma için istemci sağlamlığı gereksinimleri) özetler. Bu bölümde, güvenlik düzeylerinin nasıl eşlenebildiğini açıklanmaktadır.

Her iki güvenlik düzeyi kümesi de Google Widevine tarafından tanımlanır. Aradaki fark kullanım düzeyindedir: mimari veya API. Beş güvenlik düzeyi Widevine API'sinde kullanılır. İçeriğinde, `content_key_specs` `security_level`Azure Media Services Widevine lisans hizmeti tarafından deserialized ve Widevine global dağıtım hizmetine geçirilir. Aşağıdaki tablo, iki güvenlik düzeyi kümesi arasındaki eşlemi gösterir.

| **Widevine mimarisinde tanımlanan güvenlik düzeyleri** |**Widevine API'de kullanılan güvenlik düzeyleri**|
|---|---| 
| **Güvenlik Düzeyi 1**: Tüm içerik işleme, şifreleme ve denetim Güvenilir Yürütme Ortamı (TEE) içinde gerçekleştirilir. Bazı uygulama modellerinde, güvenlik işleme farklı yongalarda gerçekleştirilebilir.|**security_level=5**: Kripto, kod çözme ve ortamın tüm kullanımı (sıkıştırılmış ve sıkıştırılmamış) donanım destekli bir TEE içinde ele alınmalıdır.<br/><br/>**security_level=4**: İçeriğin kriptolanması ve şifresi donanım destekli bir TEE içinde yapılmalıdır.|
**Güvenlik Düzeyi 2**: TEE içinde kriptografi (ancak video işleme) gerçekleştirilir. Şifresi çözülmüş arabellekler uygulama etki alanına döndürülür ve ayrı video donanımı veya yazılımı aracılığıyla işlenir. Ancak Düzey 2'de, şifreleme bilgileri hala sadece TEE içinde işlenir.| **security_level=3**: Anahtar malzeme ve kripto işlemleri donanım destekli bir TEE içinde yapılmalıdır. |
| **Güvenlik Seviyesi 3**: Cihazda TEE yok. Ana bilgisayar işletim sistemindeki kriptografik bilgileri ve şifresi çözülmüş içeriği korumak için uygun önlemler alınabilir. Düzey 3 uygulaması da bir donanım şifreleme altyapısı içerebilir, ancak bu yalnızca performansı artırır, güvenliği değil. | **security_level=2**: Yazılım kriptosu ve gizlenmiş bir kod çözücü gereklidir.<br/><br/>**security_level=1**: Yazılım tabanlı beyaz kutu kripto gereklidir.|

#### <a name="why-does-content-download-take-so-long"></a>İçerik indirme işlemi neden bu kadar uzun sürüyor?

İndirme hızını artırmanın iki yolu vardır:

* Kullanıcıların içerik indirmek için başlangıç/akış bitiş noktası yerine bu noktaya çarpma olasılığının daha yüksek olması için bir içerik dağıtım ağını etkinleştirin. Bir kullanıcı akış uç noktasına çarparsa, her HLS kesimi veya DASH parçası dinamik olarak paketlenir ve şifrelenir. Bu gecikme sonu her kesim veya parça için milisaniye ölçeğinde olsa da, bir saatlik videonuz olduğunda, birikmiş gecikme gecikmesi büyük olabilir ve daha uzun bir indirmeye neden olabilir.
* Kullanıcılara tüm içerikler yerine video kalitesi katmanlarını ve ses parçalarını seçikal olarak indirme seçeneği verin. Çevrimdışı mod için, tüm kalite katmanlarını indirmenin bir anlamı yoktur. Bunu başarmanın iki yolu vardır:

  * İstemci kontrollü: Oynatıcı uygulaması otomatik olarak video kalitesi katmanını ve indirilecek ses parçalarını seçer veya kullanıcı seçer.
  * Hizmet denetimi: HLS çalma listesini veya DASH MPD'yi tek bir video kalitesi katmanı ve seçili ses parçalarıyla sınırlayan (genel) bir filtre oluşturmak için Azure Medya Hizmetleri'ndeki Dinamik Bildirim özelliğini kullanabilirsiniz. Ardından kullanıcılara sunulan indirme URL'si bu filtreyi içerir.

## <a name="next-steps"></a>Sonraki adımlar

[Medya Hizmetleri v3 genel bakış](media-services-overview.md)
