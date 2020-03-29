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
ms.openlocfilehash: 70256046089a59df1de79b78124c5d60fde77080
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76705947"
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

Sık sorulan aşağıdaki sorular sorun giderme konusunda yardımcı olabilir:

- **Çevrimdışı modda neden yalnızca ses çalınmıyor, video oynanmıyor?** Bu davranış örnek uygulamanın tasarımı ile gibi görünüyor. Çevrimdışı modda alternatif bir ses parçası (HLS için geçerli olan) olduğunda, hem iOS 10 hem de iOS 11 varsayılan olarak alternatif ses parçasına geçer. FPS çevrimdışı modu için bu davranışı telafi etmek için, alternatif ses parçasını akıştan kaldırın. Bunu Medya Hizmetleri'nde yapmak için dinamik bildirim filtresini "yalnızca ses=false" ekleyin. Başka bir deyişle, HLS URL 'si .ism/manifest(format=m3u8-aapl,audio-only=false) ile biter. 
- **Ben yalnızca sesli=false ekledikten sonra neden çevrimdışı modda yalnızca video olmadan ses oynatmıyor?** İçerik teslim ağı (CDN) önbellek anahtarı tasarımına bağlı olarak, içerik önbelleğe alınabilir. Önbelleği temizle.
- **FPS çevrimdışı modu iOS 11'de de iOS 10'a ek olarak desteklenir mi?** Evet. FPS çevrimdışı modu iOS 10 ve iOS 11 için desteklenir.
- **FPS Server SDK'da neden "FairPlay Streaming ile Çevrimdışı Oynatma ve HTTP Live Streaming" belgesini bulamıyorum?** FPS Server SDK sürüm 4'ten bu yana, bu belge "FairPlay Akış Programlama Kılavuzu" ile birleştirilmiştir.
- **iOS aygıtlarında indirilen/çevrimdışı dosya yapısı nedir?** iOS aygıtında indirilen dosya yapısı aşağıdaki ekran görüntüsüne benzer. Klasör, `_keys` her lisans hizmeti ana bilgisayarı için bir mağaza dosyasıyla birlikte FPS lisanslarını karşıdan yükledi. Klasör `.movpkg` ses ve video içeriğini depolar. Bir çizgi ile biten bir ad ile ilk klasör ütambir sayısal video içeriği içerir. Sayısal değer, video yorumlamalarının PeakBandwidth'idir. Bir çizgi ve ardından 0 ile biten bir ada sahip ikinci klasör ses içeriği içerir. "Veri" adlı üçüncü klasör FPS içeriğinin ana çalma listesini içerir. Son olarak, boot.xml klasör `.movpkg` içeriğinin tam bir açıklamasını sağlar. 

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

## <a name="next-steps"></a>Sonraki adımlar

[AES-128 ile koruma](protect-with-aes128.md) hakkında bilgi edinin
