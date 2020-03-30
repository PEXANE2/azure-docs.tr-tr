---
title: HLS içeriğini çevrimdışı Apple FairPlay ile koruyun - Azure | Microsoft Dokümanlar
description: Bu konu genel bir bakış sağlar ve http Live Streaming (HLS) içeriğinizi çevrimdışı modda Apple FairPlay ile dinamik olarak şifrelemek için Azure Medya Hizmetlerinin nasıl kullanılacağını gösterir.
services: media-services
keywords: HLS, DRM, FairPlay Streaming (FPS), Çevrimdışı, iOS 10
documentationcenter: ''
author: willzhan
manager: steveng
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/16/2019
ms.author: willzhan
ms.reviewer: dwgeo
ms.openlocfilehash: 1644c00aea8eefa78550c8d0238dbedab0378492
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74968707"
---
# <a name="offline-fairplay-streaming-for-ios"></a>iOS için Çevrimdışı FairPlay Akışı 

> [!div class="op_single_selector" title1="Kullandığınız Medya Hizmetleri sürümünü seçin:"]
> * [Sürüm 3](../latest/offline-fairplay-for-ios.md)
> * [Sürüm 2](media-services-protect-hls-with-offline-fairplay.md)

> [!NOTE]
> Media Services v2’ye herhangi bir yeni özellik veya işlevsellik eklenmemektedir. <br/>En son sürümü göz atın, [Medya Hizmetleri v3](https://docs.microsoft.com/azure/media-services/latest/). Ayrıca, [v2'den v3'e geçiş kılavuzuna](../latest/migrate-from-v2-to-v3.md) bakın

Azure Medya Hizmetleri, şunları kapsayan iyi tasarlanmış bir dizi [içerik koruma hizmeti](https://azure.microsoft.com/services/media-services/content-protection/) sağlar:

- Microsoft PlayReady
- Google Widevine
- Apple FairPlay
- AES-128 şifrelemesi

Dijital haklar yönetimi (DRM)/Gelişmiş Şifreleme Standardı (AES) içeriğin şifrelemesi, çeşitli akış protokolleri için istek üzerine dinamik olarak gerçekleştirilir. DRM lisansı/AES şifre çözme anahtar teslim hizmetleri de Media Services tarafından sağlanmaktadır.

Çeşitli akış protokolleri üzerinden çevrimiçi akış için içeriği korumanın yanı sıra, korumalı içerik için çevrimdışı mod da sık sık istenen bir özelliktir. Aşağıdaki senaryolar için çevrimdışı mod desteği gereklidir:

* Seyahat sırasında olduğu gibi internet bağlantısı olmadığında oynatma.
* Bazı içerik sağlayıcılar, bir ülke/bölge sınırının ötesinde DRM lisans teslimine izin verebilir. Kullanıcılar ülke/bölge dışında seyahat ederken içeriği izlemek istiyorlarsa, çevrimdışı indirme gereklidir.
* Bazı ülkelerde/bölgelerde, internet kullanılabilirliği ve/veya bant genişliği hala sınırlıdır. Kullanıcılar, tatmin edici bir görüntüleme deneyimi için yeterince yüksek bir çözünürlükte içeriği izleyebilmek için önce indirmeyi seçebilir. Bu durumda, sorun genellikle ağ kullanılabilirliği değil, sınırlı ağ bant genişliğidir. Over-the-top (OTT)/çevrimiçi video platformu (OVP) sağlayıcıları çevrimdışı mod desteği talep.

Bu makale, iOS 10 veya daha yeni çalıştıran aygıtları hedefleyen FairPlay Streaming (FPS) çevrimdışı mod desteğini kapsar. Bu özellik, macOS'ta watchOS, tvOS veya Safari gibi diğer Apple platformları için desteklenmez.

## <a name="preliminary-steps"></a>Ön adımlar
FairPlay için çevrimdışı DRM'yi bir iOS 10+ cihazda uygulamadan önce:

* FairPlay için çevrimiçi içerik koruması hakkında bilgi edinin. Daha fazla bilgi için aşağıdaki makale ve örneklere bakın:

    - [Azure Medya Hizmetleri için Apple FairPlay Akışı genellikle kullanılabilir](https://azure.microsoft.com/blog/apple-FairPlay-streaming-for-azure-media-services-generally-available/)
    - [HLS içeriğinizi Apple FairPlay veya Microsoft PlayReady ile koruyun](https://docs.microsoft.com/azure/media-services/media-services-protect-hls-with-FairPlay)
    - [Çevrimiçi FPS akışı için bir örnek](https://azure.microsoft.com/resources/samples/media-services-dotnet-dynamic-encryption-with-FairPlay/)

* FPS SDK'yı Apple Geliştirici Ağı'ndan edinin. FPS SDK iki bileşeniçerir:

    - Anahtar Güvenlik Modülü (KSM), istemci örnekleri, bir belirtim ve bir dizi test vektörü içeren FPS Server SDK.
    - D işlev belirtimi içeren FPS Dağıtım Paketi ve FPS Sertifikası, müşteriye özel özel anahtar ve Uygulama Gizli Anahtarının nasıl üretileceklerine ilişkin yönergeler. Apple, FPS Dağıtım Paketini yalnızca lisanslı içerik sağlayıcılarına verir.

## <a name="configuration-in-media-services"></a>Medya Hizmetlerinde Yapılandırma
[Medya Hizmetleri .NET SDK](https://www.nuget.org/packages/windowsazure.mediaservices)üzerinden FPS çevrimdışı mod yapılandırması için, FPS çevrimdışı modunu yapılandırmak için gerekli API'yi sağlayan Medya Hizmetleri .NET SDK sürümünü 4.0.0.4 veya daha yeni kullanın.
Ayrıca, çevrimiçi modFPS içerik korumasını yapılandırmak için çalışma koduna da ihtiyacınız vardır. FPS için çevrimiçi mod içerik korumasını yapılandırmak için kodu aldıktan sonra, yalnızca aşağıdaki iki değişikliğe ihtiyacınız vardır.

## <a name="code-change-in-the-fairplay-configuration"></a>FairPlay yapılandırmasında kod değişikliği
İlk değişiklik, çevrimdışı DRM senaryosunu etkinleştirdiğinde doğru olan objDRMSettings.EnableOfflineMode olarak adlandırılan "çevrimdışı modu etkinleştir" Boolean'ı tanımlamaktır. Bu göstergeye bağlı olarak, FairPlay yapılandırmada aşağıdaki değişikliği yapın:

```csharp
if (objDRMSettings.EnableOfflineMode)
    {
        FairPlayConfiguration = Microsoft.WindowsAzure.MediaServices.Client.FairPlay.FairPlayConfiguration.CreateSerializedFairPlayOptionConfiguration(
            objX509Certificate2,
            pfxPassword,
            pfxPasswordId,
            askId,
            iv, 
            RentalAndLeaseKeyType.PersistentUnlimited,
            0x9999);
    }
    else
    {
        FairPlayConfiguration = Microsoft.WindowsAzure.MediaServices.Client.FairPlay.FairPlayConfiguration.CreateSerializedFairPlayOptionConfiguration(
            objX509Certificate2,
            pfxPassword,
            pfxPasswordId,
            askId,
            iv);
    }
```

## <a name="code-change-in-the-asset-delivery-policy-configuration"></a>Varlık teslim ilkesi yapılandırmasındaki kod değişikliği
İkinci değişiklik, üçüncü anahtarı Dictionary<AssetDeliveryPolicyConfigurationKey, string> eklemektir.
Burada gösterildiği gibi AssetDeliveryPolicyConfigurationKey ekleyin:
 
```csharp
// FPS offline mode
    if (drmSettings.EnableOfflineMode)
    {
        objDictionary_AssetDeliveryPolicyConfigurationKey.Add(AssetDeliveryPolicyConfigurationKey.AllowPersistentLicense, "true");
        Console.WriteLine("FPS OFFLINE MODE: AssetDeliveryPolicyConfigurationKey.AllowPersistentLicense added into asset delivery policy configuration.");
    }

    // for IAssetDelivery for FPS
    IAssetDeliveryPolicy objIAssetDeliveryPolicy = objCloudMediaContext.AssetDeliveryPolicies.Create(
            drmSettings.AssetDeliveryPolicyName,
            AssetDeliveryPolicyType.DynamicCommonEncryptionCbcs,
            AssetDeliveryProtocol.HLS,
            objDictionary_AssetDeliveryPolicyConfigurationKey);
```

Bu adımdan sonra, FPS varlık teslim işlemi ilkesindeki>> dizesini Dictionary_AssetDeliveryPolicyConfigurationKey <şeyi içer

* AssetDeliveryPolicyConfigurationKey.FairPlayBaseLicenseAcquisitionUrl veya AssetDeliveryPolicyConfigurationKey.FairPlayLicenseAcquisitionUrl, fps KSM/key sunucusu gibi faktörlere bağlı olarak ve aynı varlık teslim politikasını birden çok varlık tadahil imal edip etmediğiniz
* VarlıkDeliveryPolicyConfigurationKey.CommonEncryptionIVForCbcs
* VarlıkTeslimPolitikasıConfigurationKey.AllowPersistentLicense

Artık Medya Hizmetleri hesabınız çevrimdışı FairPlay lisansları sunacak şekilde yapılandırıldı.

## <a name="sample-ios-player"></a>Örnek iOS Oynatıcı
FPS çevrimdışı mod desteği yalnızca iOS 10 ve sonraki saatlerde kullanılabilir. FPS Server SDK (sürüm 3.0 veya sonraki) FPS çevrimdışı modu için belge ve örnek içerir. Özellikle, FPS Server SDK (sürüm 3.0 veya sonraki sürüm) çevrimdışı modile ilgili aşağıdaki iki öğeyi içerir:

* Belge: "FairPlay Streaming ve HTTP Live Streaming ile Çevrimdışı Oynatma." Apple, 14 Eylül 2016. FPS Server SDK sürüm 4.0'da bu belge ana FPS belgesinde birleştirilir.
* Örnek kod: \FairPlay Streaming Server SDK sürüm 3.1\Development\Client\HLSCatalog_With_FPS\HLSCatalog\'de FPS çevrimdışı modu için HLSCatalog örneği. HLSCatalog örnek uygulamasında, çevrimdışı mod özelliklerini uygulamak için aşağıdaki kod dosyaları kullanılır:

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

![Çevrimdışı FairPlay iOS Uygulama Akışları](media/media-services-protect-hls-with-offline-FairPlay/media-services-offline-FairPlay-ios-app-streams.png)

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
- **FPS çevrimdışı modu için aşağıdaki API'de son parametre ne anlama geliyor?**
`Microsoft.WindowsAzure.MediaServices.Client.FairPlay.FairPlayConfiguration.CreateSerializedFairPlayOptionConfiguration(objX509Certificate2, pfxPassword, pfxPasswordId, askId, iv, RentalAndLeaseKeyType.PersistentUnlimited, 0x9999);`

    Bu API için belgeler için, [FairPlayConfiguration.CreateSerializedFairPlayOptionConfiguration Method](https://docs.microsoft.com/dotnet/api/microsoft.windowsazure.mediaservices.client.FairPlay.FairPlayconfiguration.createserializedFairPlayoptionconfiguration?view=azure-dotnet)bakın. Parametre, birim olarak saat ile çevrimdışı kiralama süresini temsil eder.
- **iOS aygıtlarında indirilen/çevrimdışı dosya yapısı nedir?** iOS aygıtında indirilen dosya yapısı aşağıdaki ekran görüntüsüne benzer. Klasör, `_keys` her lisans hizmeti ana bilgisayarı için bir mağaza dosyasıyla birlikte FPS lisanslarını karşıdan yükledi. Klasör `.movpkg` ses ve video içeriğini depolar. Bir çizgi ile biten bir ad ile ilk klasör ütambir sayısal video içeriği içerir. Sayısal değer, video yorumlamalarının PeakBandwidth'idir. Bir çizgi ve ardından 0 ile biten bir ada sahip ikinci klasör ses içeriği içerir. "Veri" adlı üçüncü klasör FPS içeriğinin ana çalma listesini içerir. Son olarak, boot.xml klasör `.movpkg` içeriğinin tam bir açıklamasını sağlar. 

![Çevrimdışı FairPlay iOS örnek uygulama dosya yapısı](media/media-services-protect-hls-with-offline-FairPlay/media-services-offline-FairPlay-file-structure.png)

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

## <a name="additional-notes"></a>Ek notlar

* Widevine, Google Inc. tarafından sağlanan ve Google, Inc.'in hizmet koşullarına ve Gizlilik Politikasına tabi olan bir hizmettir.

## <a name="summary"></a>Özet
Bu belge, FPS çevrimdışı modunu uygulamak için kullanabileceğiniz aşağıdaki adımları ve bilgileri içerir:

* Media Services .NET API aracılığıyla Medya Hizmetleri içerik koruma yapılandırması, Medya Hizmetlerinde dinamik FairPlay şifrelemesini ve FairPlay lisans teslimini yapılandırır.
* FPS Server SDK'dan alınan örneğe dayalı bir iOS oynatıcı, çevrimiçi akış modunda veya çevrimdışı modda FPS içeriğini oynatabilen bir iOS oynatıcı ayarlar.
* Örnek FPS videoları çevrimdışı modu ve çevrimiçi akışı test etmek için kullanılır.
* SSS, FPS çevrimdışı modu hakkındaki soruları yanıtlar.

## <a name="next-steps"></a>Sonraki adımlar

[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]