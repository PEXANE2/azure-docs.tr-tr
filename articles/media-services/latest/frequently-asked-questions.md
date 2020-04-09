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
ms.date: 04/07/2020
ms.author: juliako
ms.openlocfilehash: a4f4bd6eaa07907dd672abe068b515b5127adac9
ms.sourcegitcommit: d187fe0143d7dbaf8d775150453bd3c188087411
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80886832"
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

## <a name="offline-streaming"></a>Çevrimdışı akış

### <a name="fairplay-streaming-for-ios"></a>iOS için FairPlay Akışı

Aşağıdaki sık sorulan sorular iOS için çevrimdışı FairPlay akışı sorun giderme ile yardımcı sağlar:

#### <a name="why-does-only-audio-play-but-not-video-during-offline-mode"></a>Çevrimdışı modda neden yalnızca ses çalınmıyor, video oynanmıyor?

Bu davranış örnek uygulamanın tasarımı ile gibi görünüyor. Çevrimdışı modda alternatif bir ses parçası (HLS için geçerli olan) olduğunda, hem iOS 10 hem de iOS 11 varsayılan olarak alternatif ses parçasına geçer. FPS çevrimdışı modu için bu davranışı telafi etmek için, alternatif ses parçasını akıştan kaldırın. Bunu Medya Hizmetleri'nde yapmak için dinamik bildirim filtresini "yalnızca ses=false" ekleyin. Başka bir deyişle, HLS URL 'si .ism/manifest(format=m3u8-aapl,audio-only=false) ile biter. 

#### <a name="why-does-it-still-play-audio-only-without-video-during-offline-mode-after-i-add-audio-onlyfalse"></a>Ben yalnızca sesli=false ekledikten sonra neden çevrimdışı modda yalnızca video olmadan ses oynatmıyor?

İçerik teslim ağı (CDN) önbellek anahtarı tasarımına bağlı olarak, içerik önbelleğe alınabilir. Önbelleği temizle.

#### <a name="is-fps-offline-mode-also-supported-on-ios-11-in-addition-to-ios-10"></a>FPS çevrimdışı modu iOS 11'de de iOS 10'a ek olarak desteklenir mi?

Evet. FPS çevrimdışı modu iOS 10 ve iOS 11 için desteklenir.

#### <a name="why-cant-i-find-the-document-offline-playback-with-fairplay-streaming-and-http-live-streaming-in-the-fps-server-sdk"></a>FPS Server SDK'da neden "FairPlay Streaming ile Çevrimdışı Oynatma ve HTTP Live Streaming" belgesini bulamıyorum?

FPS Server SDK sürüm 4'ten bu yana, bu belge "FairPlay Akış Programlama Kılavuzu" ile birleştirilmiştir.

#### <a name="what-is-the-downloadedoffline-file-structure-on-ios-devices"></a>iOS aygıtlarında indirilen/çevrimdışı dosya yapısı nedir?

iOS aygıtında indirilen dosya yapısı aşağıdaki ekran görüntüsüne benzer. Klasör, `_keys` her lisans hizmeti ana bilgisayarı için bir mağaza dosyasıyla birlikte FPS lisanslarını karşıdan yükledi. Klasör `.movpkg` ses ve video içeriğini depolar. Bir çizgi ile biten bir ad ile ilk klasör ütambir sayısal video içeriği içerir. Sayısal değer, video yorumlamalarının PeakBandwidth'idir. Bir çizgi ve ardından 0 ile biten bir ada sahip ikinci klasör ses içeriği içerir. "Veri" adlı üçüncü klasör FPS içeriğinin ana çalma listesini içerir. Son olarak, boot.xml klasör `.movpkg` içeriğinin tam bir açıklamasını sağlar. 

![Çevrimdışı FairPlay iOS örnek uygulama dosya yapısı](media/offline-fairplay-for-ios/offline-fairplay-file-structure.png)

Örnek bir boot.xml dosyası:

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

#### <a name="how-can-i-deliver-persistent-licenses-offline-enabled-for-some-clientsusers-and-non-persistent-licenses-offline-disabled-for-others-do-i-have-to-duplicate-the-content-and-use-separate-content-key"></a>Bazı istemciler/kullanıcılar için kalıcı lisansları (çevrimdışı etkin) ve diğerleri için kalıcı olmayan lisansları (çevrimdışı devre dışı) nasıl teslim edebilirim? İçeriği çoğaltmak ve ayrı içerik anahtarı kullanmak zorunda mıyım?

Media Services v3 bir Varlığın birden fazla StreamingLocators'a sahip olmasını sağladığından. Sen olabilir

* license_type ile Bir ContentKeyPolicy = "kalıcı", ContentKeyPolicyRestriction "kalıcı" üzerinde iddia ile ve onun StreamingLocator;
* başka bir ContentKeyPolicy ile license_type="nonpersistent", ContentKeyPolicyRestriction "nonpersistent" üzerinde iddia ile ve onun StreamingLocator.
* İki StreamingLocators farklı ContentKey var.

Özel STS iş mantığına bağlı olarak, farklı talepler JWT belirteci verilir. Belirteç ile yalnızca ilgili lisans alınabilir ve yalnızca ilgili URL oynatılabilir.

#### <a name="what-is-the-mapping-between-the-widevine-and-media-services-drm-security-levels"></a>Widevine ve Medya Hizmetleri DRM güvenlik düzeyleri arasındaki haritalama nedir?

Google'ın "Widevine DRM Architecture Overview" üç farklı güvenlik düzeyi tanımlar. Ancak, [Widevine lisans şablonundaki Azure Medya Hizmetleri belgelerinde](widevine-license-template-overview.md)beş farklı güvenlik düzeyi özetlenmiştir. Bu bölümde, güvenlik düzeylerinin nasıl eşlenebildiğini açıklanmaktadır.

Google'ın "Widevine DRM Architecture Review" dokümanı aşağıdaki üç güvenlik düzeyine göre tanımlanır:

* Güvenlik Düzeyi 1: Tüm içerik işleme, şifreleme ve denetim Güvenilir Yürütme Ortamı (TEE) içinde gerçekleştirilir. Bazı uygulama modellerinde, güvenlik işleme farklı yongalarda gerçekleştirilebilir.
* Güvenlik Düzeyi 2: TEE içinde şifreleme (ancak video işleme) gerçekleştirir: şifresi çözülmüş arabellekleri uygulama etki alanına döndürülür ve ayrı video donanımı veya yazılımı aracılığıyla işlenir. Ancak 2.
* Güvenlik Seviyesi 3 Cihazda TEE yok. Ana bilgisayar işletim sistemindeki kriptografik bilgileri ve şifresi çözülmüş içeriği korumak için uygun önlemler alınabilir. Düzey 3 uygulaması da bir donanım şifreleme motoru içerebilir, ancak bu yalnızca performansı artırır, güvenliği değil.

Aynı zamanda, [Widevine lisans şablonundaki Azure Medya Hizmetleri belgelerinde,](widevine-license-template-overview.md)content_key_specs security_level özelliği aşağıdaki beş farklı değere (oynatma için istemci sağlamlığı gereksinimleri) sahip olabilir:

* Yazılım tabanlı beyaz kutu kripto gereklidir.
* Yazılım kripto ve bir obfuscated kod çözücü gereklidir.
* Anahtar malzeme ve kripto işlemleri donanım destekli TEE içinde yapılmalıdır.
* İçeriğin kriptolanması ve şifresi donanım destekli TEE içinde yapılmalıdır.
* Kripto, kod çözme ve ortamın tüm kullanımı (sıkıştırılmış ve sıkıştırılmamış) donanım destekli TEE içinde ele alınmalıdır.

Her iki güvenlik düzeyi de Google Widevine tarafından tanımlanır. Fark kullanım düzeyinde: mimari düzeyi veya API düzeyi. Beş güvenlik düzeyi Widevine API'sinde kullanılır. security_level içeren content_key_specs nesnesi, Azure Media Services Widevine lisans hizmeti tarafından seri olarak deserialize edilir ve Widevine global teslimat hizmetine aktarılır. Aşağıdaki tablo, iki güvenlik düzeyi kümesi arasındaki eşlemi gösterir.

| **Widevine Mimarisinde Tanımlanan Güvenlik Düzeyleri** |**Widevine API'de Kullanılan Güvenlik Düzeyleri**|
|---|---| 
| **Güvenlik Düzeyi 1**: Tüm içerik işleme, şifreleme ve denetim Güvenilir Yürütme Ortamı (TEE) içinde gerçekleştirilir. Bazı uygulama modellerinde, güvenlik işleme farklı yongalarda gerçekleştirilebilir.|**security_level=5**: Kripto, kod çözme ve ortamın tüm kullanımı (sıkıştırılmış ve sıkıştırılmamış) donanım destekli TEE içinde ele alınmalıdır.<br/><br/>**security_level=4**: İçeriğin kriptolanması ve şifresi donanım destekli TEE içinde yapılmalıdır.|
**Güvenlik Düzeyi 2**: TEE içinde kriptografi (ancak video işleme) gerçekleştirir: şifresi çözülmüş arabellekler uygulama etki alanına döndürülür ve ayrı video donanım ı veya yazılım aracılığıyla işlenir. Ancak 2.| **security_level=3**: Anahtar malzeme ve kripto işlemleri donanım destekli TEE içinde yapılmalıdır. |
| **Güvenlik Düzeyi 3**: Cihazda TEE yok. Ana bilgisayar işletim sistemindeki kriptografik bilgileri ve şifresi çözülmüş içeriği korumak için uygun önlemler alınabilir. Düzey 3 uygulaması da bir donanım şifreleme motoru içerebilir, ancak bu yalnızca performansı artırır, güvenliği değil. | **security_level=2**: Yazılım kriptosu ve gizlenmiş bir kod çözücü gereklidir.<br/><br/>**security_level=1**: Yazılım tabanlı beyaz kutu kripto gereklidir.|

#### <a name="why-does-content-download-take-so-long"></a>İçerik indirme işlemi neden bu kadar uzun sürüyor?

İndirme hızını artırmanın iki yolu vardır:

* CdN'yi etkinleştirin, böylece son kullanıcıların içerik karşıdan yükleme için başlangıç/akış bitiş noktası yerine CDN'ye basma olasılığı daha yüksektir. Kullanıcı akış bitiş noktasına vurursa, her HLS kesimi veya DASH parçası dinamik olarak paketlenir ve şifrelenir. Bu gecikme gecikmesi her kesim/parça için milisaniye ölçeğinde olsa da, bir saatlik videonuz olduğunda, birikmiş gecikme gecikmesi daha uzun indirmeye neden olabilir.
* Son kullanıcılara tüm içerikler yerine video kalitesi katmanlarını ve ses parçalarını seçikal olarak indirme seçeneği sağlayın. Çevrimdışı mod için, tüm kalite katmanlarını indirmenin bir anlamı yoktur. Bunu başarmanın iki yolu vardır:

   * İstemci kontrollü: ya oyuncu uygulaması otomatik seçer veya kullanıcı indirmek için video kalitesi katmanı ve ses parçaları seçer;
   * Hizmet denetimi: HLS çalma listesi veya DASH MPD'yi tek bir video kalitesi katmanı ve seçili ses parçalarıyla sınırlayan (genel) bir filtre oluşturmak için Azure Media Services'teki Dinamik Bildirim özelliğini kullanabilirsiniz. Ardından, son kullanıcılara sunulan indirme URL'si bu filtreyi içerir.

## <a name="next-steps"></a>Sonraki adımlar

[Medya Hizmetleri v3 genel bakış](media-services-overview.md)
