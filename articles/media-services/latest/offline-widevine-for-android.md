---
title: Azure Medya Hizmetleri v3 ile Stream Widevine Android Çevrimdışı
description: Bu konu, Azure Medya Hizmetleri hesabınızı Widevine korumalı içeriğin çevrimdışı akışı için nasıl yapılandırılabildiğini gösterir.
services: media-services
keywords: DASH, DRM, Widevine Çevrimdışı Mod, ExoPlayer, Android
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
ms.openlocfilehash: 64cd93acc78f4cb5b7ebc4266e7359aec662890c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80295430"
---
# <a name="offline-widevine-streaming-for-android-with-media-services-v3"></a>Medya Hizmetleri v3 ile Android için Çevrimdışı Widevine akışı

Çevrimiçi akış için içeriği korumanın yanı sıra, medya içeriği aboneliği ve kiralama hizmetleri, internete bağlı olmadığınız zaman çalışan indirilebilir içerik sunar. Ağdan bağlantısı kesiliyorken uçak modunda oynatmak için telefonunuza veya tabletinize içerik indirmeniz gerekebilir. İçerik indirmek isteyebileceğin ek senaryolar:

- Bazı içerik sağlayıcılar, bir ülke/bölge sınırının ötesinde DRM lisans teslimine izin verebilir. Bir kullanıcı yurt dışına seyahat ederken içeriği izlemek istiyorsa, çevrimdışı indirme gereklidir.
- Bazı ülkelerde/bölgelerde, Internet kullanılabilirliği ve/veya bant genişliği sınırlıdır. Kullanıcılar tatmin edici görüntüleme deneyimi için yeterince yüksek çözünürlükte izleyebilmek için içeriği indirmeyi seçebilir.

Bu makalede, Android cihazlarda Widevine tarafından korunan DASH içeriği için çevrimdışı mod oynatma nasıl uygulanacağı açıklanmaktadır. Çevrimdışı DRM, içeriğiniz için abonelik, kiralama ve satın alma modelleri sağlamanıza olanak sağlayarak, hizmetlerinizin müşterilerinin internetbağlantısı kesildiğinde kolayca içerik almalarını sağlar.

Android player uygulamalarını oluşturmak için üç seçenek sıralıyoruz:

> [!div class="checklist"]
> * ExoPlayer SDK'nın Java API'sini kullanarak oyuncu oluşturma
> * ExoPlayer SDK Xamarin bağlama kullanarak bir oyuncu oluşturun
> * Chrome mobil tarayıcı v62 veya sonraki şifreli ortam uzantısı (EME) ve Medya Kaynak Uzantısı (MSE) kullanarak bir oynatıcı oluşturma

Makale ayrıca Widevine korumalı içeriğin çevrimdışı akışı ile ilgili bazı sık sorulan soruları yanıtlar.

> [!NOTE]
> Çevrimdışı DRM, yalnızca içeriği karşıdan yüklediğinizde lisans için tek bir istekte bulunmak için faturalandırılır. Hatalar faturalandırılmez.

## <a name="prerequisites"></a>Ön koşullar 

Android cihazlarda Widevine için çevrimdışı DRM uygulamadan önce şunları belirtmelisiniz:

- Widevine DRM kullanarak çevrimiçi içerik koruması için tanıtılan kavramlara aşina olun. Bu, aşağıdaki belgelerde/örneklerde ayrıntılı olarak ele alınmıştır:
    - [Erişim denetimi ile çoklu DRM'ye sahip içerik koruma sistemi tasarlama](design-multi-drm-system-with-access-control.md)
    - [DRM dinamik şifreleme ve lisans teslim hizmetini kullanma](protect-with-drm.md)
- Klon https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials.git.

    Widevine yapılandırmaları eklemek için [.NET'i kullanarak DRM ile](https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials/tree/master/AMSV3Tutorials/EncryptWithDRM) Şifrele'deki kodu değiştirmeniz gerekir.  
- Çevrimdışı Widevine DRM oynatmayı destekleyebilecek açık kaynak kodlu bir video oynatıcı Olan Android için Google ExoPlayer SDK'ya aşina olun. 
    - [ExoPlayer SDK](https://github.com/google/ExoPlayer)
    - [ExoPlayer Geliştirici Kılavuzu](https://google.github.io/ExoPlayer/guide.html)
    - [EoPlayer Geliştirici Blog](https://medium.com/google-exoplayer)

## <a name="configure-content-protection-in-azure-media-services"></a>Azure Medya Hizmetlerinde içerik korumayı yapılandırma

[GetOrCreateContentKeyPolicyAsync](https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials/blob/master/AMSV3Tutorials/EncryptWithDRM/Program.cs#L189) yönteminde aşağıdaki gerekli adımlar mevcuttur:

1. Lisans teslim hizmetinde içerik anahtarı teslimine nasıl izin verilebildiğini belirtin: 

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
2. Widevine lisans şablonu yapılandırın:  

    ```csharp
    ContentKeyPolicyWidevineConfiguration widevineConfig = ConfigureWidevineLicenseTempate();
    ```

3. ContentKeyPolicyOptions oluşturun:

    ```csharp
    options.Add(
        new ContentKeyPolicyOption()
        {
            Configuration = widevineConfig,
            Restriction = restriction
        });
    ```

## <a name="enable-offline-mode"></a>Çevrimdışı modu etkinleştirme

Widevine lisansları için **çevrimdışı** modu etkinleştirmek için [Widevine lisans şablonunu](widevine-license-template-overview.md)yapılandırmanız gerekir. **policy_overrides** nesnesinde, [ConfigureWidevineLicenseTempate'de](https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials/blob/master/AMSV3Tutorials/EncryptWithDRM/Program.cs#L563)gösterildiği gibi **can_persist** **özelliğini doğru** (varsayılan yanlıştır) ayarlayın. 

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/EncryptWithDRM/Program.cs#ConfigureWidevineLicenseTempate)]

## <a name="configuring-the-android-player-for-offline-playback"></a>Çevrimdışı oynatma için Android oynatıcıyı yapılandırma

Android cihazlar için bir yerli oyuncu uygulaması geliştirmek için en kolay yolu [Google ExoPlayer SDK](https://github.com/google/ExoPlayer)kullanmaktır , bir açık kaynak video oynatıcı SDK. ExoPlayer şu anda MPEG-DASH ve Microsoft Smooth Streaming dağıtım protokolleri de dahil olmak üzere Android'in yerel MediaPlayer API tarafından desteklenmeyen özellikleri destekler.

ExoPlayer sürüm 2.6 ve üzeri çevrimdışı Widevine DRM oynatma destekleyen birçok sınıf içerir. Özellikle, ÇevrimdışıLisansHelper sınıfı, çevrimdışı lisansları indirmek, yenilemek ve serbest bırakmak için DefaultDrmSessionManager'ın kullanımını kolaylaştırmak için yardımcı program işlevleri sağlar. SDK klasöründe sağlanan sınıflar "library/core/src/main/java/com/google/android/exoplayer2/offline/" çevrimdışı video içeriği indirmeyi destekler.

Aşağıdaki sınıflar listesi Android için ExoPlayer SDK'da çevrimdışı modu kolaylaştırır:

- kütüphane/çekirdek/src/main/java/com/google/android/exoplayer2/drm/OfflineLicenseHelper.java  
- kütüphane/çekirdek/src/main/java/com/google/android/exoplayer2/drm/DefaultDrmSession.java
- kütüphane/çekirdek/src/main/java/com/google/android/exoplayer2/drm/DefaultDrmSessionManager.java
- kütüphane/çekirdek/src/main/java/com/google/android/exoplayer2/drm/DrmSession.java
- kütüphane/çekirdek/src/main/java/com/google/android/exoplayer2/drm/ErrorStateDrmSession.java
- kütüphane/çekirdek/src/main/java/com/google/android/exoplayer2/drm/ExoMediaDrm.java
- kütüphane/çekirdek/src/main/java/com/google/android/exoplayer2/offline/SegmentDownloader.java
- kütüphane/çekirdek/src/main/java/com/google/android/exoplayer2/offline/DownloaderConstructorHelper.java 
- kütüphane/çekirdek/src/main/java/com/google/android/exoplayer2/offline/Downloader.java
- kütüphane/dash/src/main/java/com/google/android/exoplayer2/source/dash/offline/DashDownloader.java 

Geliştiriciler, bir uygulamanın geliştirilmesi sırasında [ExoPlayer Geliştirici Kılavuzu'na](https://google.github.io/ExoPlayer/guide.html) ve ilgili [Geliştirici Bloguna](https://medium.com/google-exoplayer) başvurmalıdır. Google şu anda Widevine çevrimdışı destekleyen ExoPlayer uygulaması için tam belgelenmiş bir referans uygulaması veya örnek kod yayımladı değil, bu nedenle bilgi geliştiricilerin kılavuzu ve blog ile sınırlıdır. 

### <a name="working-with-older-android-devices"></a>Eski Android cihazlarla çalışma

Bazı eski Android cihazlariçin aşağıdaki **policy_overrides** özellikleri [(Widevine lisans şablonu:](widevine-license-template-overview.md) **rental_duration_seconds**, **playback_duration_seconds**ve **license_duration_seconds**tanımlanan değerleri ayarlamanız gerekir. Alternatif olarak, onları sıfıra ayarlayabilirsiniz, bu da sonsuz/sınırsız süre anlamına gelir.  

Bir veyasede taşması hatasını önlemek için değerler ayarlanmalıdır. Konu hakkında daha fazla https://github.com/google/ExoPlayer/issues/3150 açıklama https://github.com/google/ExoPlayer/issues/3112için bkz. <br/>Değerleri açıkça ayarlamazsanız, **PlaybackDurationRemaining** ve **LicenseDurationRemaining** için çok büyük değerler atanır (örneğin, 9223372036854775807, 64 bitlik bir tamsayı için maksimum pozitif değerdir). Sonuç olarak, Widevine lisansının süresi dolmuş gibi görünür ve bu nedenle şifre çözme gerçekleşmez. 

Android 4.4 KitKat aslında diğer eski Android sürümlerinde olduğu gibi ARMv7 ve 32-bit platformları desteklemek için tasarlanmış iken Bu sorun Android 5.0 Lollipop veya daha sonra Android 5.0 tamamen ARMv8 ([Advanced RISC Machine](https://en.wikipedia.org/wiki/ARM_architecture)) ve 64-bit platformları desteklemek için tasarlanmıştır ilk Android sürümü, bu yana oluşmaz.

## <a name="using-xamarin-to-build-an-android-playback-app"></a>Android oynatma uygulaması oluşturmak için Xamarin'i kullanma

Aşağıdaki bağlantıları kullanarak ExoPlayer için Xamarin ciltlerini bulabilirsiniz:

- [Xamarin, Google ExoPlayer kitaplığı için kitaplığı bağlayıcı](https://github.com/martijn00/ExoPlayerXamarin)
- [ExoPlayer NuGet için Xamarin ciltler](https://www.nuget.org/packages/Xam.Plugins.Android.ExoPlayer/)

Ayrıca, aşağıdaki iş parçacığıbakın: [Xamarin bağlama](https://github.com/martijn00/ExoPlayerXamarin/pull/57). 

## <a name="chrome-player-apps-for-android"></a>Android için Chrome player uygulamaları

[Android v. 62 için Chrome](https://developers.google.com/web/updates/2017/09/chrome-62-media-updates)sürümü ile başlayarak, EME kalıcı lisans desteklenir. [Widevine L1](https://developers.google.com/web/updates/2017/09/chrome-62-media-updates#widevine_l1) artık Android için Chrome'da da desteklendi. Bu, son kullanıcılarınız Chrome'un bu (veya daha yüksek) sürümüne sahipse Chrome'da çevrimdışı oynatma uygulamaları oluşturmanıza olanak tanır. 

Buna ek olarak, Google bir Progressive Web App (PWA) örnek üretti ve açık kaynaklı: 

- [Kaynak kodu](https://github.com/GoogleChromeLabs/sample-media-pwa)
- [Google barındırılan sürüm](https://biograf-155113.appspot.com/ttt/episode-2/) (yalnızca Chrome v 62 ve daha yüksek Android cihazlarda çalışır)

Mobil Chrome tarayıcınızı Android telefonda v62 'ye (veya daha yükseğe) yükseltir ve yukarıdaki barındırılan örnek uygulamayı test ederseniz, hem çevrimiçi akış hem de çevrimdışı oynatmanın işe yaradığını görürsünüz.

Yukarıdaki açık kaynak PWA uygulaması Düğüm.js'de yazılır. Kendi sürümünüzü bir Ubuntu sunucusunda barındırmak istiyorsanız, oynatmayı önleyecek aşağıdaki sık karşılaşılan sorunları aklınızda bulundurun:

1. CORS sorunu: Örnek uygulamadaki örnek video https://storage.googleapis.com/biograf-video-files/videos/. Google, Google Bulut Depolama kovasında barındırılan tüm test örnekleri için CORS'u kurmuştur. Cors üstbilgilerini ile birlikte sunulurlar ve cors `https://biograf-155113.appspot.com` girişini açıkça belirtirler: (google'ın örneğini barındırdığı alan adı) diğer sitelerin erişimini engeller. Denerseniz, aşağıdaki HTTP hatasını görürsünüz:`Failed to load https://storage.googleapis.com/biograf-video-files/videos/poly-sizzle-2015/mp4/dash.mpd: No 'Access-Control-Allow-Origin' header is present on the requested resource. Origin 'https:\//13.85.80.81:8080' is therefore not allowed access. If an opaque response serves your needs, set the request's mode to 'no-cors' to fetch the resource with CORS disabled.`
2. Sertifika sorunu: Chrome v 58'den başlayarak Widevine için EME HTTPS gerektirir. Bu nedenle, örnek uygulamayı X509 sertifikasıyla HTTPS üzerinden barındırmanız gerekir. Olağan bir test sertifikası aşağıdaki gereksinimler nedeniyle çalışmıyor: Aşağıdaki minimum gereksinimleri karşılayan bir sertifika almanız gerekir:
    - Chrome ve Firefox, sertifikada YER ALMAK IÇIN SAN-Subject Alternative Name ayarının gerekli
    - Sertifika güvenilir CA'ya sahip olmalıdır ve kendi imzalı geliştirme sertifikası çalışmıyor
    - Sertifikanın web sunucusunun veya ağ geçidinin DNS adı ile eşleşen bir CN'si olmalıdır

## <a name="frequently-asked-questions"></a>Sık sorulan sorular

### <a name="question"></a>Soru

Bazı istemciler/kullanıcılar için kalıcı lisansları (çevrimdışı etkin) ve diğerleri için kalıcı olmayan lisansları (çevrimdışı devre dışı) nasıl teslim edebilirim? İçeriği çoğaltmak ve ayrı içerik anahtarı kullanmak zorunda mıyım?

### <a name="answer"></a>Yanıt
Media Services v3 bir Varlığın birden fazla StreamingLocators'a sahip olmasını sağladığından. Sen olabilir

1.    license_type ile Bir ContentKeyPolicy = "kalıcı", ContentKeyPolicyRestriction "kalıcı" üzerinde iddia ile ve onun StreamingLocator;
2.    başka bir ContentKeyPolicy ile license_type="nonpersistent", ContentKeyPolicyRestriction "nonpersistent" üzerinde iddia ile ve onun StreamingLocator.
3.    İki StreamingLocators farklı ContentKey var.

Özel STS iş mantığına bağlı olarak, farklı talepler JWT belirteci verilir. Belirteç ile yalnızca ilgili lisans alınabilir ve yalnızca ilgili URL oynatılabilir.

### <a name="question"></a>Soru

Widevine güvenlik düzeyleri için, Google'ın "Widevine DRM Architecture Overview" dokümanı üç farklı güvenlik düzeyi tanımlar. Ancak, [Widevine lisans şablonundaki Azure Medya Hizmetleri belgelerinde](widevine-license-template-overview.md)beş farklı güvenlik düzeyi özetlenmiştir. İki farklı güvenlik düzeyi kümesi arasındaki ilişki veya eşleme nedir?

### <a name="answer"></a>Yanıt

Google'ın "Widevine DRM Architecture Review" dokümanı aşağıdaki üç güvenlik düzeyine göre tanımlanır:

1.  Güvenlik Düzeyi 1: Tüm içerik işleme, şifreleme ve denetim Güvenilir Yürütme Ortamı (TEE) içinde gerçekleştirilir. Bazı uygulama modellerinde, güvenlik işleme farklı yongalarda gerçekleştirilebilir.
2.  Güvenlik Düzeyi 2: TEE içinde şifreleme (ancak video işleme) gerçekleştirir: şifresi çözülmüş arabellekleri uygulama etki alanına döndürülür ve ayrı video donanımı veya yazılımı aracılığıyla işlenir. Ancak 2.
3.  Güvenlik Seviyesi 3 Cihazda TEE yok. Ana bilgisayar işletim sistemindeki kriptografik bilgileri ve şifresi çözülmüş içeriği korumak için uygun önlemler alınabilir. Düzey 3 uygulaması da bir donanım şifreleme motoru içerebilir, ancak bu yalnızca performansı artırır, güvenliği değil.

Aynı zamanda, [Widevine lisans şablonundaki Azure Medya Hizmetleri belgelerinde,](widevine-license-template-overview.md)content_key_specs security_level özelliği aşağıdaki beş farklı değere (oynatma için istemci sağlamlığı gereksinimleri) sahip olabilir:

1.  Yazılım tabanlı beyaz kutu kripto gereklidir.
2.  Yazılım kripto ve bir obfuscated kod çözücü gereklidir.
3.  Anahtar malzeme ve kripto işlemleri donanım destekli TEE içinde yapılmalıdır.
4.  İçeriğin kriptolanması ve şifresi donanım destekli TEE içinde yapılmalıdır.
5.  Kripto, kod çözme ve ortamın tüm kullanımı (sıkıştırılmış ve sıkıştırılmamış) donanım destekli TEE içinde ele alınmalıdır.

Her iki güvenlik düzeyi de Google Widevine tarafından tanımlanır. Fark kullanım düzeyinde: mimari düzeyi veya API düzeyi. Beş güvenlik düzeyi Widevine API'sinde kullanılır. security_level içeren content_key_specs nesnesi, Azure Media Services Widevine lisans hizmeti tarafından seri olarak deserialize edilir ve Widevine global teslimat hizmetine aktarılır. Aşağıdaki tablo, iki güvenlik düzeyi kümesi arasındaki eşlemi gösterir.

| **Widevine Mimarisinde Tanımlanan Güvenlik Düzeyleri** |**Widevine API'de Kullanılan Güvenlik Düzeyleri**|
|---|---| 
| **Güvenlik Düzeyi 1**: Tüm içerik işleme, şifreleme ve denetim Güvenilir Yürütme Ortamı (TEE) içinde gerçekleştirilir. Bazı uygulama modellerinde, güvenlik işleme farklı yongalarda gerçekleştirilebilir.|**security_level=5**: Kripto, kod çözme ve ortamın tüm kullanımı (sıkıştırılmış ve sıkıştırılmamış) donanım destekli TEE içinde ele alınmalıdır.<br/><br/>**security_level=4**: İçeriğin kriptolanması ve şifresi donanım destekli TEE içinde yapılmalıdır.|
**Güvenlik Düzeyi 2**: TEE içinde kriptografi (ancak video işleme) gerçekleştirir: şifresi çözülmüş arabellekler uygulama etki alanına döndürülür ve ayrı video donanım ı veya yazılım aracılığıyla işlenir. Ancak 2.| **security_level=3**: Anahtar malzeme ve kripto işlemleri donanım destekli TEE içinde yapılmalıdır. |
| **Güvenlik Düzeyi 3**: Cihazda TEE yok. Ana bilgisayar işletim sistemindeki kriptografik bilgileri ve şifresi çözülmüş içeriği korumak için uygun önlemler alınabilir. Düzey 3 uygulaması da bir donanım şifreleme motoru içerebilir, ancak bu yalnızca performansı artırır, güvenliği değil. | **security_level=2**: Yazılım kriptosu ve gizlenmiş bir kod çözücü gereklidir.<br/><br/>**security_level=1**: Yazılım tabanlı whitebox kripto gereklidir.|

### <a name="question"></a>Soru

İçerik indirme işlemi neden bu kadar uzun sürüyor?

### <a name="answer"></a>Yanıt

İndirme hızını artırmanın iki yolu vardır:

1.  CdN'yi etkinleştirin, böylece son kullanıcıların içerik karşıdan yükleme için başlangıç/akış bitiş noktası yerine CDN'ye basma olasılığı daha yüksektir. Kullanıcı akış bitiş noktasına vurursa, her HLS kesimi veya DASH parçası dinamik olarak paketlenir ve şifrelenir. Bu gecikme gecikmesi her kesim/parça için milisaniye ölçeğinde olsa da, bir saatlik videonuz olduğunda, birikmiş gecikme gecikmesi daha uzun indirmeye neden olabilir.
2.  Son kullanıcılara tüm içerikler yerine video kalitesi katmanlarını ve ses parçalarını seçikal olarak indirme seçeneği sağlayın. Çevrimdışı mod için, tüm kalite katmanlarını indirmenin bir anlamı yoktur. Bunu başarmanın iki yolu vardır:
    1.  İstemci kontrollü: ya oyuncu uygulaması otomatik seçer veya kullanıcı indirmek için video kalitesi katmanı ve ses parçaları seçer;
    2.  Hizmet denetimi: HLS çalma listesi veya DASH MPD'yi tek bir video kalitesi katmanı ve seçili ses parçalarıyla sınırlayan (genel) bir filtre oluşturmak için Azure Media Services'teki Dinamik Bildirim özelliğini kullanabilirsiniz. Ardından, son kullanıcılara sunulan indirme URL'si bu filtreyi içerir.

## <a name="additional-notes"></a>Ek notlar

* Widevine, Google Inc. tarafından sağlanan ve Google, Inc.'in hizmet koşullarına ve Gizlilik Politikasına tabi olan bir hizmettir.

## <a name="summary"></a>Özet

Bu makalede, Android cihazlarda Widevine tarafından korunan DASH içeriği için çevrimdışı mod oynatma nasıl uygulanacağı tartışılmıştır.  Ayrıca Widevine korumalı içeriğin çevrimdışı akışı ile ilgili bazı yaygın soruları yanıtladı.
