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
ms.date: 04/07/2020
ms.author: willzhan
ms.openlocfilehash: 94edec8261d9916b7575fb247e1698273f244130
ms.sourcegitcommit: d187fe0143d7dbaf8d775150453bd3c188087411
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80887206"
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

- [Kaynak kod](https://github.com/GoogleChromeLabs/sample-media-pwa)
- [Google barındırılan sürüm](https://biograf-155113.appspot.com/ttt/episode-2/) (yalnızca Chrome v 62 ve daha yüksek Android cihazlarda çalışır)

Mobil Chrome tarayıcınızı Android telefonda v62 'ye (veya daha yükseğe) yükseltir ve yukarıdaki barındırılan örnek uygulamayı test ederseniz, hem çevrimiçi akış hem de çevrimdışı oynatmanın işe yaradığını görürsünüz.

Yukarıdaki açık kaynak PWA uygulaması Düğüm.js'de yazılır. Kendi sürümünüzü bir Ubuntu sunucusunda barındırmak istiyorsanız, oynatmayı önleyecek aşağıdaki sık karşılaşılan sorunları aklınızda bulundurun:

1. CORS sorunu: Örnek uygulamadaki örnek video https://storage.googleapis.com/biograf-video-files/videos/. Google, Google Bulut Depolama kovasında barındırılan tüm test örnekleri için CORS'u kurmuştur. Cors üstbilgilerini ile birlikte sunulurlar ve cors `https://biograf-155113.appspot.com` girişini açıkça belirtirler: (google'ın örneğini barındırdığı alan adı) diğer sitelerin erişimini engeller. Denerseniz, aşağıdaki HTTP hatasını görürsünüz:`Failed to load https://storage.googleapis.com/biograf-video-files/videos/poly-sizzle-2015/mp4/dash.mpd: No 'Access-Control-Allow-Origin' header is present on the requested resource. Origin 'https:\//13.85.80.81:8080' is therefore not allowed access. If an opaque response serves your needs, set the request's mode to 'no-cors' to fetch the resource with CORS disabled.`
2. Sertifika sorunu: Chrome v 58'den başlayarak Widevine için EME HTTPS gerektirir. Bu nedenle, örnek uygulamayı X509 sertifikasıyla HTTPS üzerinden barındırmanız gerekir. Olağan bir test sertifikası aşağıdaki gereksinimler nedeniyle çalışmıyor: Aşağıdaki minimum gereksinimleri karşılayan bir sertifika almanız gerekir:
    - Chrome ve Firefox, sertifikada YER ALMAK IÇIN SAN-Subject Alternative Name ayarının gerekli
    - Sertifika güvenilir CA'ya sahip olmalıdır ve kendi imzalı geliştirme sertifikası çalışmıyor
    - Sertifikanın web sunucusunun veya ağ geçidinin DNS adı ile eşleşen bir CN'si olmalıdır

## <a name="faqs"></a>SSS

Daha fazla bilgi için [Widevine SSS'lerine](frequently-asked-questions.md#widevine-streaming-for-android)bakın.

## <a name="additional-notes"></a>Ek notlar

Widevine, Google Inc. tarafından sağlanan ve Google, Inc.'in hizmet koşullarına ve Gizlilik Politikasına tabi olan bir hizmettir.

## <a name="summary"></a>Özet

Bu makalede, Android cihazlarda Widevine tarafından korunan DASH içeriği için çevrimdışı mod oynatma nasıl uygulanacağı tartışılmıştır.  Ayrıca Widevine korumalı içeriğin çevrimdışı akışı ile ilgili bazı yaygın soruları yanıtladı.
