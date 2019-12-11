---
title: Hesabınızı, Widevine korunan içeriğin çevrimdışı akışı için Yapılandırma-Azure
description: Bu konu başlığı altında, Widevine korumalı içeriğin çevrimdışı akışı için Azure Media Services hesabınızın nasıl yapılandırılacağı gösterilmektedir.
services: media-services
keywords: DASH, DRM, Widevine çevrimdışı modu, Exooynatıcı, Android
documentationcenter: ''
author: willzhan
manager: steveng
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/08/2019
ms.author: willzhan
ms.openlocfilehash: 1c1142f995376a8a640f33402294e20c925bbfbb
ms.sourcegitcommit: 5ab4f7a81d04a58f235071240718dfae3f1b370b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/10/2019
ms.locfileid: "74974164"
---
# <a name="offline-widevine-streaming-for-android"></a>Android için çevrimdışı Widevine akışı

Çevrimiçi akış için içeriği korumanın yanı sıra, medya içerik aboneliği ve Kiralama Hizmetleri, internet 'e bağlı değilseniz kullanılan indirilebilir içerik sunar. Ağ bağlantısı kesildiğinde Uçak modunda kayıttan yürütmek için içeriği telefonunuza veya tabletinize indirmeniz gerekebilir. İçeriği indirmek isteyebileceğiniz ek senaryolar:

- Bazı içerik sağlayıcıları, bir ülke/bölge kenarlığının ötesinde DRM lisans teslimine izin verebilir. Kullanıcı yolculukta içerik izlemek istiyorsa, çevrimdışı indirme gerekir.
- Bazı ülkelerde/bölgelerde Internet kullanılabilirliği ve/veya bant genişliği sınırlıdır. Kullanıcılar, tatmin edici görüntüleme deneyimi için yeterince yüksek çözünürlükte izleyebilmek için içerik indirmeyi seçebilirler.

Bu makalede, Android cihazlarda Widevine tarafından korunan DASH içeriği için çevrimdışı modda kayıttan yürütmenin nasıl uygulanacağı açıklanır. Çevrimdışı DRM, içeriğiniz için abonelik, kiralama ve satın alma modelleri sağlamanıza olanak tanıyarak, hizmetlerinizin müşterilerinin internet bağlantısı kesildiğinde kolayca içerik almasına imkan tanır.

Android Player uygulamalarını oluşturmak için üç seçenekten oluşan bir ana hat sunuyoruz:

> [!div class="checklist"]
> * ExoPlayer SDK 'sının Java API 'sini kullanarak oyuncu oluşturma
> * ExoPlayer SDK 'sının Xamarin bağlamasını kullanarak oyuncu oluşturma
> * Chrome Mobile Browser V62 veya üzeri sürümlerde şifreli medya uzantısı 'nı (EME) ve medya kaynak uzantısı 'nı (MSE) kullanarak bir oyuncu oluşturun

Makalede ayrıca Wıdevine korumalı içeriğin çevrimdışı akışı ile ilgili bazı yaygın soruların yanıtları da vardır.

> [!NOTE]
> Çevrimdışı DRM yalnızca içeriği indirdiğinizde lisans için tek bir istek yapmak üzere faturalandırılır. Tüm hatalar faturalandırılmaz.

## <a name="prerequisites"></a>Önkoşullar 

Android cihazlarda Widevine için çevrimdışı DRM uygulamadan önce, önce şunları yapmalısınız:

- Widevine DRM kullanarak çevrimiçi içerik koruma için tanıtılan kavramlarla ilgili bilgi sahibi olun. Bu, aşağıdaki belgelerde/örneklerde ayrıntılı olarak ele alınmıştır:
    - [Erişim denetimi ile çoklu DRM'ye sahip içerik koruma sistemi tasarlama](design-multi-drm-system-with-access-control.md)
    - [DRM dinamik şifreleme ve lisans teslim hizmetini kullanma](protect-with-drm.md)
- Kopya https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials.git.

    Widevine yapılandırması eklemek için [.NET kullanarak DRM ile birlikte şifrelemeden](https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials/tree/master/AMSV3Tutorials/EncryptWithDRM) kodu değiştirmeniz gerekir.  
- Çevrimdışı Widevine DRM kayıttan yürütmeyi destekleyebilen açık kaynaklı bir video oynatıcı SDK 'sı olan Android için Google ExoPlayer SDK 'sı hakkında bilgi sahibi olun. 
    - [Exooynatıcı SDK 'Sı](https://github.com/google/ExoPlayer)
    - [ExoPlayer Geliştirici Kılavuzu](https://google.github.io/ExoPlayer/guide.html)
    - [EoPlayer geliştirici blogu](https://medium.com/google-exoplayer)

## <a name="configure-content-protection-in-azure-media-services"></a>Azure Media Services 'da içerik korumasını yapılandırma

[Getorcreatecontentkeypolicyasync](https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials/blob/master/AMSV3Tutorials/EncryptWithDRM/Program.cs#L189) yönteminde aşağıdaki gerekli adımlar mevcuttur:

1. İçerik anahtarı tesliminin lisans teslim hizmetinde nasıl yetkilendirildiğini belirtin: 

    ```csharp
    ContentKeyPolicySymmetricTokenKey primaryKey = new ContentKeyPolicySymmetricTokenKey(tokenSigningKey);
    List<ContentKeyPolicyTokenClaim> requiredClaims = new List<ContentKeyPolicyTokenClaim>()
    {
        ContentKeyPolicyTokenClaim.ContentKeyIdentifierClaim
    };
    List<ContentKeyPolicyRestrictionTokenKey> alternateKeys = null;
    ContentKeyPolicyTokenRestriction restriction 
        = new ContentKeyPolicyTokenRestriction(Issuer, Audience, primaryKey, ContentKeyPolicyRestrictionTokenType.Jwt, alternateKeys, requiredClaims);
    ```
2. Widevine lisans şablonunu yapılandırma:  

    ```csharp
    ContentKeyPolicyWidevineConfiguration widevineConfig = ConfigureWidevineLicenseTempate();
    ```

3. ContentKeyPolicyOptions oluştur:

    ```csharp
    options.Add(
        new ContentKeyPolicyOption()
        {
            Configuration = widevineConfig,
            Restriction = restriction
        });
    ```

## <a name="enable-offline-mode"></a>Çevrimdışı modunu etkinleştir

Widevine lisansları için **çevrimdışı** modu etkinleştirmek üzere [Widevine lisans şablonu](widevine-license-template-overview.md)' nu yapılandırmanız gerekir. **Policy_overrides** nesnesinde, **Can_persist** özelliğini, [Configurewidevinelicensetempate](https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials/blob/master/AMSV3Tutorials/EncryptWithDRM/Program.cs#L563)içinde gösterildiği gibi **true** (varsayılan değeri false) olarak ayarlayın. 

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/EncryptWithDRM/Program.cs#ConfigureWidevineLicenseTempate)]

## <a name="configuring-the-android-player-for-offline-playback"></a>Android Player 'ı çevrimdışı kayıttan yürütme için yapılandırma

Android cihazlar için yerel bir oynatıcı uygulaması geliştirmenin en kolay yolu, açık kaynaklı bir video oynatıcı SDK 'sı olan [Google ExoPlayer SDK 'sını](https://github.com/google/ExoPlayer)kullanmaktır. Exooynatıcı, MPEG-DASH ve Microsoft Kesintisiz Akış teslim protokolleri dahil olmak üzere Android 'in yerel MediaPlayer API 'SI tarafından desteklenmeyen özellikleri destekler.

ExoPlayer sürüm 2,6 ve üzeri, çevrimdışı Widevine DRM kayıttan yürütmeyi destekleyen birçok sınıf içerir. Özellikle, OfflineLicenseHelper sınıfı, çevrimdışı lisansları indirmek, yenilemek ve serbest bırakmak için DefaultDrmSessionManager 'ın kullanımını kolaylaştırmak için yardımcı program işlevlerini sağlar. "Library/Core/src/Main/Java/com/Google/Android/exoplayer2/offline/" SDK klasöründe belirtilen sınıflar çevrimdışı video içeriğini indirmeyi destekliyor.

Aşağıdaki sınıfların listesi, Android için ExoPlayer SDK 'sında çevrimdışı modu kolaylaştırır:

- Library/Core/src/Main/Java/com/Google/Android/exoplayer2/DRM/OfflineLicenseHelper. Java  
- Library/Core/src/Main/Java/com/Google/Android/exoplayer2/DRM/DefaultDrmSession. Java
- Library/Core/src/Main/Java/com/Google/Android/exoplayer2/DRM/DefaultDrmSessionManager. Java
- Library/Core/src/Main/Java/com/Google/Android/exoplayer2/DRM/DrmSession. Java
- Library/Core/src/Main/Java/com/Google/Android/exoplayer2/DRM/ErrorStateDrmSession. Java
- Library/Core/src/Main/Java/com/Google/Android/exoplayer2/DRM/ExoMediaDrm. Java
- Library/Core/src/Main/Java/com/Google/Android/exoplayer2/OFFLINE/SegmentDownloader. Java
- Library/Core/src/Main/Java/com/Google/Android/exoplayer2/OFFLINE/DownloaderConstructorHelper. Java 
- Kitaplık/çekirdek/src/Main/Java/com/Google/Android/exoplayer2/OFFLINE/Downloader. Java
- Kitaplık/tire/src/Main/Java/com/Google/Android/exoplayer2/kaynak/tire/OFFLINE/Tireyükleyicisi. Java 

Geliştiriciler, bir uygulamanın geliştirilmesi sırasında [Exoplayer geliştirici kılavuzuna](https://google.github.io/ExoPlayer/guide.html) ve Ilgili [Geliştirici bloguna](https://medium.com/google-exoplayer) başvurmalıdır. Google, şu anda çevrimdışı olarak Widevine 'yı destekleyen ExoPlayer uygulaması için tam olarak belgelenmiş bir başvuru uygulaması veya örnek kod yayımlamamıştır, bu nedenle bilgiler geliştiricilerin kılavuzuyla ve bloguyla sınırlandırılmıştır. 

### <a name="working-with-older-android-devices"></a>Eski Android cihazlarla çalışma

Bazı eski Android cihazlarda, aşağıdaki **policy_overrides** özellikleri için değerler ayarlamanız gerekir ( [Widevine lisans şablonunda](widevine-license-template-overview.md)tanımlanmıştır: **rental_duration_seconds**, **playback_duration_seconds**ve **license_duration_seconds**. Alternatif olarak, sonsuz/sınırsız süre anlamına gelen sıfır olarak ayarlayabilirsiniz.  

Bir tamsayı taşma hatasını önlemek için değerlerin ayarlanması gerekir. Sorun hakkında daha fazla açıklama için bkz. https://github.com/google/ExoPlayer/issues/3150 ve https://github.com/google/ExoPlayer/issues/3112. <br/>Değerleri açıkça ayarlamazsanız, **PlaybackDurationRemaining** ve **LicenseDurationRemaining** için çok büyük değerler atanır (örneğin, 64 bit tam sayı için en büyük pozitif değer olan 9223372036854775807). Sonuç olarak, Widevine lisansının geçerliliği zaman aşımına uğradı, bu nedenle şifre çözme gerçekleşmeyecektir. 

Android 5,0, ARMv8 ([GELIŞMIŞ RISC makinesi](https://en.wikipedia.org/wiki/ARM_architecture)) ve 64-bit platformlarını tamamen destekleyecek şekilde tasarlanan ilk Android sürümü olduğundan, Android 5,0 Lollipop veya sonrasında bu sorun oluşmaz, çünkü Android 4,4 KitKat ilk olarak diğer eski Android sürümleriyle aynı şekilde ARMv7 ve 32 bit platformları desteklemek üzere tasarlanmıştır.

## <a name="using-xamarin-to-build-an-android-playback-app"></a>Android oynatma uygulaması oluşturmak için Xamarin kullanma

Aşağıdaki bağlantıları kullanarak, ExoPlayer için Xamarin bağlamalarını bulabilirsiniz:

- [Google ExoPlayer kitaplığı için Xamarin bağlamaları kitaplığı](https://github.com/martijn00/ExoPlayerXamarin)
- [ExoPlayer NuGet için Xamarin bağlamaları](https://www.nuget.org/packages/Xam.Plugins.Android.ExoPlayer/)

Ayrıca, aşağıdaki iş parçacığına bakın: [Xamarin bağlama](https://github.com/martijn00/ExoPlayerXamarin/pull/57). 

## <a name="chrome-player-apps-for-android"></a>Android için Chrome Player uygulamaları

[Android v. 62 Için Chrome](https://developers.google.com/web/updates/2017/09/chrome-62-media-updates)sürümü ile başlayarak, eme 'de kalıcı lisans desteklenir. [Wdevine L1](https://developers.google.com/web/updates/2017/09/chrome-62-media-updates#widevine_l1) artık Android için Chrome 'da da desteklenmektedir. Bu, son kullanıcılarınız Chrome 'un bu (veya üzeri) sürümü varsa, Chrome 'da çevrimdışı oynatma uygulamaları oluşturmanıza olanak tanır. 

Ayrıca, Google bir aşamalı Web uygulaması (PWA) örneği üretti ve açık kaynaklı BT: 

- [Kaynak kod](https://github.com/GoogleChromeLabs/sample-media-pwa)
- [Google hosted sürümü](https://biograf-155113.appspot.com/ttt/episode-2/) (Android cihazlarda yalnızca Chrome v 62 ve üzeri sürümlerde geçerlidir)

Mobil Chrome tarayıcınızı bir Android telefonunda V62 (veya üzeri) sürümüne yükseltir ve yukarıdaki barındırılan örnek uygulamayı test ederseniz hem çevrimiçi akış hem de çevrimdışı oynatma işinin olduğunu görürsünüz.

Yukarıdaki açık kaynaklı PWA uygulaması Node. js ' de yazılır. Bir Ubuntu sunucusunda kendi sürümünüzü barındırmak istiyorsanız, kayıttan yürütmeyi engelleyebilecek aşağıdaki yaygın sorunları göz önünde bulundurun:

1. CORS sorunu: örnek uygulamadaki örnek video https://storage.googleapis.com/biograf-video-files/videos/ içinde barındırılır. Google, Google bulut depolama demeti içinde barındırılan tüm test örnekleri için CORS 'yi ayarladı. Bunlar, özel olarak CORS https://biograf-155113.appspot.com girişi (Google 'ın örneğini barındırdığı etki alanı) tarafından başka herhangi bir sitenin erişimini engellediği CORS başlıkları ile birlikte sunulur. Denerseniz, şu HTTP hatasını görürsünüz: yüklenemedi https://storage.googleapis.com/biograf-video-files/videos/poly-sizzle-2015/mp4/dash.mpd: istenen kaynakta ' erişim-denetim-Izin-Origin ' üst bilgisi yok. ' Https:\//13.85.80.81:8080 ' kaynağına izin verilmiyor. Donuk bir yanıt ihtiyaçlarınıza hizmet veriyorsa, isteği CORS devre dışı olarak getirmek için isteğin modunu ' No-CORS ' olarak ayarlayın.
2. Sertifika sorunu: Chrome v 58 ' den başlayarak Widevine için EME, HTTPS gerektirir. Bu nedenle, örnek uygulamayı bir x509 sertifikasıyla HTTPS üzerinden barındırmanıza gerek duyarsınız. Aşağıdaki gereksinimler nedeniyle olağan bir test sertifikası çalışmıyor: aşağıdaki minimum gereksinimleri karşılayan bir sertifika edinmeniz gerekir:
    - Chrome ve Firefox, sertifikada SAN konusu alternatif adı ayarının mevcut olmasını gerektirir
    - Sertifika, güvenilir bir CA 'ya sahip olmalı ve kendinden imzalı bir geliştirme sertifikası çalışmıyor
    - Sertifika, Web sunucusunun veya ağ geçidinin DNS adıyla eşleşen bir CN 'ye sahip olmalıdır

## <a name="frequently-asked-questions"></a>Sık sorulan sorular

### <a name="question"></a>Soru

Bazı istemciler/kullanıcılar ve kalıcı olmayan lisanslar (çevrimdışı olarak devre dışı) için, diğerleri için kalıcı lisanslar (çevrimdışı etkin) teslim etme. İçeriği çoğaltmalıyım ve ayrı içerik anahtarı kullanmalıdır mi?

### <a name="answer"></a>Yanıt
Media Services v3, bir varlığın birden çok Streamingkonumlandırıcıları olmasına izin veriyor. Sahip olabilirsiniz

1.  "Persistent" ve StreamingLocator ile birlikte license_type = "persistent" ve ContentKeyPolicyRestriction ile bir ContentKeyPolicy
2.  "Kalıcı olmayan" ve StreamingLocator ile license_type = "persistent" ve ContentKeyPolicyRestriction ile başka bir ContentKeyPolicy.
3.  İki Streamingkonumlandırıcı farklı bir ContentKey 'e sahip.

Özel STS iş mantığına bağlı olarak, JWT belirtecinde farklı talepler verilir. Belirteç ile yalnızca ilgili lisans alınabilir ve yalnızca ilgili URL oynatılabilir.

### <a name="question"></a>Soru

Widevine güvenlik düzeyleri için, Google 'ın [WIDEVINE DRM mimarisine genel bakış belge](https://storage.googleapis.com/wvdocs/Widevine_DRM_Architecture_Overview.pdf) belgelerinde üç farklı güvenlik düzeyi tanımlanmaktadır. Ancak, [Widevine lisans şablonunda Azure Media Services belgelerde](widevine-license-template-overview.md), beş farklı güvenlik düzeyi özetlenmiştir. İki farklı güvenlik düzeyi kümesi arasındaki ilişki veya eşleme nedir?

### <a name="answer"></a>Yanıt

Google 'ın [Widevine DRM mimarisine genel bakış](https://storage.googleapis.com/wvdocs/Widevine_DRM_Architecture_Overview.pdf)' da, aşağıdaki üç güvenlik düzeyini tanımlar:

1.  Güvenlik düzeyi 1: tüm içerik işleme, şifreleme ve denetim, güvenilir yürütme ortamı (t) içinde gerçekleştirilir. Bazı uygulama modellerinde, güvenlik işlemleri farklı yongalar üzerinde gerçekleştirilebilir.
2.  Güvenlik düzeyi 2: t içinde şifreleme gerçekleştirir (ancak video işleme değil): şifresi çözülmüş arabellekler uygulama etki alanına döndürülür ve ayrı video donanımı veya yazılım aracılığıyla işlenir. Ancak, düzey 2 ' de, şifreleme bilgileri yalnızca t içinde işlenir.
3.  Güvenlik düzeyi 3 ' te cihazda t yok. Şifreleme bilgilerini ve konak işletim sistemindeki şifresi çözülmüş içeriği korumak için uygun ölçüler alınabilir. 3\. düzey bir uygulama de bir donanım şifreleme altyapısı içerebilir, ancak bu yalnızca performansı artırır, güvenliği etkilemez.

Aynı zamanda, [Widevine lisans şablonu 'ndaki Azure Media Services belgelerde](widevine-license-template-overview.md), content_key_specs security_level özelliği aşağıdaki beş farklı değere sahip olabilir (kayıttan yürütme için istemci sağlamlık gereksinimleri):

1.  Yazılım tabanlı beyaz kutu şifreleme gereklidir.
2.  Yazılım şifreleme ve karıştırılmış bir kod çözücü gereklidir.
3.  Anahtar malzeme ve şifre işlemleri, donanım tarafından desteklenen bir t içinde gerçekleştirilmelidir.
4.  İçeriğin şifrelenmiş ve kod çözme işlemi, donanım tarafından desteklenen bir t içinde gerçekleştirilmelidir.
5.  Şifreleme, kod çözme ve medyanın tüm işlenmesi (sıkıştırılmış ve sıkıştırılmamış), donanım tarafından desteklenen bir t içinde işlenmelidir.

Her iki güvenlik düzeyi de Google Widevine tarafından tanımlanır. Fark, kullanım düzeyidir: mimari düzeyi veya API düzeyi. Wdevine API 'sinde beş güvenlik düzeyi kullanılır. Security_level içeren content_key_specs nesnesi serisi kaldırıldı ve Azure Media Services Widevine lisans hizmeti tarafından Widevine küresel teslim hizmetine geçirilir. Aşağıdaki tabloda, iki güvenlik düzeyi kümesi arasındaki eşleme gösterilmektedir.

| **Widevine mimarisinde tanımlanan güvenlik düzeyleri** |**Widevine API 'de kullanılan güvenlik düzeyleri**|
|---|---| 
| **Güvenlik düzeyi 1**: tüm içerik işleme, şifreleme ve denetim, güvenilir yürütme ORTAMı (t) içinde gerçekleştirilir. Bazı uygulama modellerinde, güvenlik işlemleri farklı yongalar üzerinde gerçekleştirilebilir.|**security_level = 5**: şifreleme, kod çözme ve medyanın tüm işlenmesi (sıkıştırılmış ve sıkıştırılmamış), donanım tarafından desteklenen bir t içinde işlenmelidir.<br/><br/>**security_level = 4**: içeriğin şifrelenmiş ve kod çözme işlemi, donanım tarafından desteklenen bir t içinde gerçekleştirilmelidir.|
**Güvenlik düzeyi 2**: t içinde şifreleme gerçekleştirir (ancak video işleme değil): şifresi çözülmüş arabellekler uygulama etki alanına döndürülür ve ayrı video donanımı veya yazılım aracılığıyla işlenir. Ancak, düzey 2 ' de, şifreleme bilgileri yalnızca t içinde işlenir.| **security_level = 3**: Ana malzeme ve şifre işlemlerinin, donanım tarafından desteklenen bir t içinde gerçekleştirilmesi gerekir. |
| **Güvenlik düzeyi 3**: CIHAZDA bir t 'ye sahip değil. Şifreleme bilgilerini ve konak işletim sistemindeki şifresi çözülmüş içeriği korumak için uygun ölçüler alınabilir. 3\. düzey bir uygulama de bir donanım şifreleme altyapısı içerebilir, ancak bu yalnızca performansı artırır, güvenliği etkilemez. | **security_level = 2**: yazılım şifreleme ve karıştırılmış bir kod çözücü gereklidir.<br/><br/>**security_level = 1**: yazılım tabanlı beyaz kutu şifreleme gereklidir.|

### <a name="question"></a>Soru

İçerik indirme neden uzun zaman alır?

### <a name="answer"></a>Yanıt

İndirme hızını artırmanın iki yolu vardır:

1.  Son kullanıcıların, içerik indirmek için kaynak/akış uç noktası yerine CDN 'ye isabet edebilmesi için CDN 'yi etkinleştirin. Kullanıcı akış uç noktasına isabet alıyorsa, her HLS segmenti veya DASH parçası dinamik olarak paketlenir ve şifrelenir. Bu gecikme süresi her segment/parça için milisaniyelik ölçekte olsa da, saat uzun bir videoya sahip olduğunuzda birikmiş gecikme süresi büyük olabilir.
2.  Son kullanıcılara tüm içerikler yerine, video kalitesi katmanlarını ve ses izlerini seçmeli olarak indirme seçeneği sağlar. Çevrimdışı modda, tüm kalite katmanlarını indirmek için bir nokta yoktur. Bunu başarmanın iki yolu vardır:
    1.  İstemci denetimli: oynatıcı uygulaması otomatik olarak seçilir veya Kullanıcı, indirilecek video kalite katmanını ve ses izlerini seçer;
    2.  Hizmet denetimli: bir adet, HLS çalma listesini veya DASH MPD 'ı tek bir video kalite katmanına ve seçili ses izlemeleriyle sınırlayan bir (genel) filtresi oluşturmak için Azure Media Services dinamik bildirim özelliğini kullanabilir. Ardından, son kullanıcılara sunulan indirme URL 'SI bu filtreyi içerecektir.

## <a name="additional-notes"></a>Ek notlar

* Widevine, Google Inc. tarafından sunulan bir hizmettir ve Google, Inc 'nin hizmet koşullarına ve gizlilik Ilkesine tabidir.

## <a name="summary"></a>Özet

Bu makalede, Android cihazlarda Widevine tarafından korunan DASH içeriği için çevrimdışı modda kayıttan yürütmeyi nasıl uygulayacağınızı ele alınmaktadır.  Ayrıca, Widevine korumalı içeriğin çevrimdışı akışı ile ilgili bazı yaygın sorulara da yanıt verdi.
