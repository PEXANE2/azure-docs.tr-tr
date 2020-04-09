---
title: Azure Medya Hizmetleri v3 ile iOS için Çevrimdışı FairPlay Akışı
description: Bu konu genel bir bakış sağlar ve http Live Streaming (HLS) içeriğinizi çevrimdışı modda Apple FairPlay ile dinamik olarak şifrelemek için Azure Medya Hizmetlerinin nasıl kullanılacağını gösterir.
services: media-services
keywords: HLS, DRM, FairPlay Streaming (FPS), Çevrimdışı, iOS 10
documentationcenter: ''
author: willzhan
manager: steveng
editor: ''
ms.assetid: 7c3b35d9-1269-4c83-8c91-490ae65b0817
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/08/2019
ms.author: willzhan
ms.openlocfilehash: 41893c2460ecb2d17e3893f867bc460105d57bbd
ms.sourcegitcommit: d187fe0143d7dbaf8d775150453bd3c188087411
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80887223"
---
# <a name="offline-fairplay-streaming-for-ios-with-media-services-v3"></a>Medya Hizmetleri v3 ile iOS için Çevrimdışı FairPlay Akışı

 Azure Medya Hizmetleri, şunları kapsayan iyi tasarlanmış bir dizi [içerik koruma hizmeti](https://azure.microsoft.com/services/media-services/content-protection/) sağlar:

- Microsoft PlayReady
- Google Widevine
    
    Widevine, Google Inc. tarafından sağlanan ve Google, Inc.'in hizmet koşullarına ve Gizlilik Politikasına tabi olan bir hizmettir.
- Apple FairPlay
- AES-128 şifrelemesi

Dijital haklar yönetimi (DRM)/Gelişmiş Şifreleme Standardı (AES) içeriğin şifrelemesi, çeşitli akış protokolleri için istek üzerine dinamik olarak gerçekleştirilir. DRM lisansı/AES şifre çözme anahtar teslim hizmetleri de Media Services tarafından sağlanmaktadır.

Çeşitli akış protokolleri üzerinden çevrimiçi akış için içeriği korumanın yanı sıra, korumalı içerik için çevrimdışı mod da sık sık istenen bir özelliktir. Aşağıdaki senaryolar için çevrimdışı mod desteği gereklidir:

* Seyahat sırasında olduğu gibi internet bağlantısı olmadığında oynatma.
* Bazı içerik sağlayıcılar, bir ülke/bölge sınırının ötesinde DRM lisans teslimine izin verebilir. Kullanıcılar ülke/bölge dışında seyahat ederken içeriği izlemek istiyorlarsa, çevrimdışı indirme gereklidir.
* Bazı ülkelerde/bölgelerde, internet kullanılabilirliği ve/veya bant genişliği hala sınırlıdır. Kullanıcılar, tatmin edici bir görüntüleme deneyimi için yeterince yüksek bir çözünürlükte içeriği izleyebilmek için önce indirmeyi seçebilir. Bu durumda, sorun genellikle ağ kullanılabilirliği değil, sınırlı ağ bant genişliğidir. Over-the-top (OTT)/çevrimiçi video platformu (OVP) sağlayıcıları çevrimdışı mod desteği talep.

Bu makale, iOS 10 veya daha yeni çalıştıran aygıtları hedefleyen FairPlay Streaming (FPS) çevrimdışı mod desteğini kapsar. Bu özellik, macOS'ta watchOS, tvOS veya Safari gibi diğer Apple platformları için desteklenmez.

> [!NOTE]
> Çevrimdışı DRM, yalnızca içeriği karşıdan yüklediğinizde lisans için tek bir istekte bulunmak için faturalandırılır. Hatalar faturalandırılmez.

## <a name="prerequisites"></a>Ön koşullar

FairPlay için çevrimdışı DRM'yi bir iOS 10+ cihazda uygulamadan önce:

* FairPlay için çevrimiçi içerik korumayı inceleyin: 

    - [Apple FairPlay lisansı gereksinimleri ve yapılandırması](fairplay-license-overview.md)
    - [DRM dinamik şifreleme ve lisans teslim hizmetini kullanma](protect-with-drm.md)
    - Çevrimiçi FPS akışının yapılandırması içeren bir .NET örneği: [ConfigureFairPlayPolicyOptions](https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials/blob/master/AMSV3Tutorials/EncryptWithDRM/Program.cs#L505)
* FPS SDK'yı Apple Geliştirici Ağı'ndan edinin. FPS SDK iki bileşeniçerir:

    - Anahtar Güvenlik Modülü (KSM), istemci örnekleri, bir belirtim ve bir dizi test vektörü içeren FPS Server SDK.
    - D işlev belirtimi içeren FPS Dağıtım Paketi ve FPS Sertifikası, müşteriye özel özel anahtar ve Uygulama Gizli Anahtarının nasıl üretileceklerine ilişkin yönergeler. Apple, FPS Dağıtım Paketini yalnızca lisanslı içerik sağlayıcılarına verir.
* Klon https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials.git. 

    FairPlay yapılandırmaları eklemek için [.NET'i kullanarak DRM ile](https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials/tree/master/AMSV3Tutorials/EncryptWithDRM) Şifrele'deki kodu değiştirmeniz gerekir.  

## <a name="configure-content-protection-in-azure-media-services"></a>Azure Medya Hizmetlerinde içerik korumayı yapılandırma

[GetOrCreateContentKeyPolicyAsync](https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials/blob/master/AMSV3Tutorials/EncryptWithDRM/Program.cs#L189) yönteminde aşağıdakileri yapın:

FairPlay ilkesi seçeneğini yapılandıran koduaçıklamadan ayırın:

```csharp
ContentKeyPolicyFairPlayConfiguration fairplayConfig = ConfigureFairPlayPolicyOptions();
```

Ayrıca, ContentKeyPolicyOptions listesine CBCS ContentKeyPolicyOption ekleyen kodun yorumsuzluğu

```csharp
options.Add(
    new ContentKeyPolicyOption()
    {
        Configuration = fairplayConfig,
        Restriction = restriction,
        Name = "ContentKeyPolicyOption_CBCS"
    });
```

## <a name="enable-offline-mode"></a>Çevrimdışı modu etkinleştirme

Çevrimdışı modu etkinleştirmek için, özel bir StreamingPolicy oluşturun ve [CreateStreamingLocatorAsync](https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials/blob/master/AMSV3Tutorials/EncryptWithDRM/Program.cs#L563)bir StreamingLocator oluştururken adını kullanın.
 
```csharp
CommonEncryptionCbcs objStreamingPolicyInput= new CommonEncryptionCbcs()
{
    Drm = new CbcsDrmConfiguration()
    {
        FairPlay = new StreamingPolicyFairPlayConfiguration()
        {
            AllowPersistentLicense = true  //this enables offline mode
        }
    },
    EnabledProtocols = new EnabledProtocols()
    {
        Hls = true,
        Dash = true //Even though DASH under CBCS is not supported for either CSF or CMAF, HLS-CMAF-CBCS uses DASH-CBCS fragments in its HLS playlist
    },

    ContentKeys = new StreamingPolicyContentKeys()
    {
        //Default key must be specified if keyToTrackMappings is present
        DefaultKey = new DefaultKey()
        {
            Label = "CBCS_DefaultKeyLabel"
        }
    }

```

Artık Medya Hizmetleri hesabınız çevrimdışı FairPlay lisansları sunacak şekilde yapılandırıldı.

## <a name="sample-ios-player"></a>Örnek iOS Oynatıcı

FPS çevrimdışı mod desteği yalnızca iOS 10 ve sonraki saatlerde kullanılabilir. FPS Server SDK (sürüm 3.0 veya sonraki) FPS çevrimdışı modu için belge ve örnek içerir. Özellikle, FPS Server SDK (sürüm 3.0 veya sonraki sürüm) çevrimdışı modile ilgili aşağıdaki iki öğeyi içerir:

* Belge: "FairPlay Streaming ve HTTP Live Streaming ile Çevrimdışı Oynatma." Apple, 14 Eylül 2016. FPS Server SDK sürüm 4.0'da bu belge ana FPS belgesinde birleştirilir.
* Örnek kod: \FairPlay Streaming Server SDK sürüm 3.1\Development\Client\HLSCatalog_With_FPS\HLSCatalog\. HLSCatalog örnek uygulamasında, çevrimdışı mod özelliklerini uygulamak için aşağıdaki kod dosyaları kullanılır:

    - AssetPersistenceManager.swift code file: AssetPersistenceManager bu örnekte nasıl yapılacağını gösteren ana sınıftır:

        - İndirmeleri başlatmak ve iptal etmek ve aygıtlardan varolan varlıkları silmek için kullanılan API'ler gibi HLS akışlarını indirmeyi yönetin.
        - İndirme ilerlemesini izleyin.
    - AssetListTableViewController.swift ve AssetListTableViewCell.swift kod dosyaları: AssetListTableViewController bu örneğin ana arabirimidir. Bir indirmeyi oynatmak, indirmek, silmek veya iptal etmek için örneğin kullanabileceği varlıkların bir listesini sağlar. 

Bu adımlar, çalışan bir iOS oynatıcının nasıl ayarlandığını gösterir. FPS Server SDK sürüm 4.0.1'deki HLSCatalog örneğinden başladığınızı varsayarsak, aşağıdaki kod değişikliklerini yapın:

HLSCatalog\Shared\Managers\ContentKeyDelegate.swift'te aşağıdaki `requestContentKeyFromKeySecurityModule(spcData: Data, assetID: String)` kodu kullanarak yöntemi uygulayın. "drmUr" HLS URL'ye atanmış bir değişken olsun.

```swift
    var ckcData: Data? = nil
    
    let semaphore = DispatchSemaphore(value: 0)
    let postString = "spc=\(spcData.base64EncodedString())&assetId=\(assetIDString)"
    
    if let postData = postString.data(using: .ascii, allowLossyConversion: true), let drmServerUrl = URL(string: self.drmUrl) {
        var request = URLRequest(url: drmServerUrl)
        request.httpMethod = "POST"
        request.setValue(String(postData.count), forHTTPHeaderField: "Content-Length")
        request.setValue("application/x-www-form-urlencoded", forHTTPHeaderField: "Content-Type")
        request.httpBody = postData
        
        URLSession.shared.dataTask(with: request) { (data, _, error) in
            if let data = data, var responseString = String(data: data, encoding: .utf8) {
                responseString = responseString.replacingOccurrences(of: "<ckc>", with: "").replacingOccurrences(of: "</ckc>", with: "")
                ckcData = Data(base64Encoded: responseString)
            } else {
                print("Error encountered while fetching FairPlay license for URL: \(self.drmUrl), \(error?.localizedDescription ?? "Unknown error")")
            }
            
            semaphore.signal()
            }.resume()
    } else {
        fatalError("Invalid post data")
    }
    
    semaphore.wait()
    return ckcData
```

HLSCatalog\Shared\Managers\ContentKeyDelegate.swift'te yöntemi `requestApplicationCertificate()`uygulayın. Bu uygulama, sertifikayı aygıta katıştırıp gömmediğinize (yalnızca ortak anahtar) veya sertifikayı web'de barındırıp barındırmadığınıza bağlıdır. Aşağıdaki uygulama, test örneklerinde kullanılan barındırılan uygulama sertifikasını kullanır. "Sertifika" uygulama sertifikasının URL'sini içeren bir değişken olsun.

```swift
func requestApplicationCertificate() throws -> Data {

        var applicationCertificate: Data? = nil
        do {
            applicationCertificate = try Data(contentsOf: URL(string: certUrl)!)
        } catch {
            print("Error loading FairPlay application certificate: \(error)")
        }
        
        return applicationCertificate
    }
```

Son tümleşik test için hem video URL'si hem de uygulama sertifikası URL'si "Tümleşik Test" bölümünde verilmiştir.

HLSCatalog\Shared\Resources\Streams.plist'te test video URL'nizi ekleyin. İçerik anahtarı kimliği için, benzersiz değer olarak skd protokolü ile FairPlay lisans edinme URL'sini kullanın.

![Çevrimdışı FairPlay iOS Uygulama Akışları](media/offline-fairplay-for-ios/offline-fairplay-ios-app-streams.png)

Kendi test video URL'nizi, FairPlay lisans edinme URL'nizi ve uygulama sertifikası URL'nizi kullanın. Veya test örnekleri içeren bir sonraki bölüme devam edebilirsiniz.

## <a name="integrated-test"></a>Entegre test

Medya Hizmetleri'ndeki üç test örneği aşağıdaki üç senaryoyu kapsar:

* FPS korumalı, video, ses ve alternatif ses parçası
* FPS korumalı, video ve ses ile, ancak alternatif ses parçası
* FPS korumalı, yalnızca video ve ses olmadan

Bu [örnekleri,](https://aka.ms/poc#22)azure web uygulamasında barındırılan ilgili uygulama sertifikasıyla bu demo sitesinde bulabilirsiniz.
FPS Server SDK'nın sürüm 3 veya sürüm 4 örneğinde, bir ana çalma listesi alternatif ses içeriyorsa, çevrimdışı modda yalnızca ses çalar. Bu nedenle, alternatif ses şerit gerekir. Başka bir deyişle, daha önce listelenen ikinci ve üçüncü örnekler çevrimiçi ve çevrimdışı modda çalışır. Listelenen örnek, çevrimiçi akış düzgün çalışırken yalnızca çevrimdışı modda ses çalar.

## <a name="faq"></a>SSS

Sık sorulan sorulara bakın [sorun giderme ile yardımcı sağlar.](frequently-asked-questions.md#why-does-only-audio-play-but-not-video-during-offline-mode)

## <a name="next-steps"></a>Sonraki adımlar

[AES-128 ile koruma](protect-with-aes128.md) hakkında bilgi edinin
