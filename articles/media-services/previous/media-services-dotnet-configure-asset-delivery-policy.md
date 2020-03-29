---
title: .NET SDK ile varlık teslim ilkelerini yapılandırma | Microsoft Dokümanlar
description: Bu konu, Azure Media Services .NET SDK ile farklı varlık teslim ilkelerinin nasıl yapılandırılabildiğini gösterir.
services: media-services
documentationcenter: ''
author: Mingfeiy
manager: femila
editor: ''
ms.assetid: 3ec46f58-6cbb-4d49-bac6-1fd01a5a456b
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 03/18/2019
ms.author: juliako
ms.openlocfilehash: ab3c40ee408498453bb137c63c440d980b0b7255
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74974521"
---
# <a name="configure-asset-delivery-policies-with-net-sdk"></a>.NET SDK ile varlık teslim ilkelerini yapılandırma
[!INCLUDE [media-services-selector-asset-delivery-policy](../../../includes/media-services-selector-asset-delivery-policy.md)]

## <a name="overview"></a>Genel Bakış
Şifreli varlıklar teslim etmeyi planlıyorsanız, Medya Hizmetleri içerik teslim iş akışındaki adımlardan biri varlıklar için teslim ilkelerini yapılandırmaktır. Varlık teslim ihdası ilkesi Medya Hizmetleri'ne varlığınızın nasıl teslim edilmesini istediğinizi bildirir: varlığınızın dinamik olarak paketlenmesi gereken akış protokolü (örneğin, MPEG DASH, HLS, Düzgün Akış veya tümü), dinamik olarak şifrelemek isteyip istemediğiniz varlık ve nasıl (zarf veya ortak şifreleme).

Bu makalede, varlık teslim ilkelerinin neden ve nasıl oluşturulup yapılandırılması tartışılmaktadır.

>[!NOTE]
>AMS hesabınız oluşturulduğunda, hesabınıza **Durdurulmuş** durumda bir **varsayılan** akış uç noktası eklenir. İçerik akışını başlatmak ve dinamik paketleme ile dinamik şifrelemeden yararlanmak için içerik akışı yapmak istediğiniz akış uç noktasının **Çalışıyor** durumda olması gerekir. 
>
>Ayrıca, dinamik paketleme ve dinamik şifreleme kullanabilmek için kıymetinizin bir dizi uyarlanabilir bithızı MP4 veya uyarlanabilir bithızı Düzgün Akış dosyaları içermesi gerekir.

Aynı varlığa farklı ilkeler uygulayabilirsiniz. Örneğin, PlayReady şifrelemesini Sorunsuz Akış ve AES Zarf şifrelemesine MPEG DASH ve HLS'ye uygulayabilirsiniz. Herhangi bir teslim ilkesinde tanımlanmayan tüm protokollerin (örneğin, protokol olarak yalnızca HLS‘yi belirten tek bir ilke ekliyorsunuz) akışla aktarılması engellenir. Bunun tek istisnası, hiçbir varlık teslim ilkesinin tanımlanmadığı durumdur. Bu halde tüm protokollere açık bir şekilde izin verilir.

Depolama şifreli bir varlık teslim etmek istiyorsanız, varlığın teslim ilkesini yapılandırmanız gerekir. Varlığınız akışa geçmeden önce, akış sunucusu depolama şifrelemesini kaldırır ve belirtilen teslim ilkesini kullanarak içeriğinizi akışı sağlar. Örneğin, Varlık durumunuzu Gelişmiş Şifreleme Standardı (AES) zarf şifreleme anahtarıyla şifrelenmiş olarak teslim etmek için ilke türünü **DynamicEnvelopeEncryption**olarak ayarlayın. Depolama şifrelemesini kaldırmak ve varlığı açık olarak aktarmak için ilke türünü **NoDinamik Şifreleme**olarak ayarlayın. Bu ilke türlerinin nasıl yapılandırılabildiğini gösteren örnekler aşağıdaki gibidir.

Varlık teslim ilkesini nasıl yapılandırdığınıza bağlı olarak, aşağıdaki akış protokollerini dinamik olarak paketleyebilir, şifreleyebilir ve aktarabilirsiniz: Düzgün Akış, HLS ve MPEG DASH.

Aşağıdaki liste, Düz, HLS ve DASH akış için kullandığınız biçimleri gösterir.

Düzgün Akış:

{akış uç noktası adı-media services hesabı adı}.streaming.mediaservices.windows.net/{konum kimliği}/{dosya adı}.ism/Manifest

Hls:

{akış uç noktası adı-media services hesabı adı}.streaming.mediaservices.windows.net/{konum kimliği}/{dosya adı}.ism/Manifest(format=m3u8-aapl)

MPEG DASH

{akış uç noktası adı-media services hesabı adı}.streaming.mediaservices.windows.net/{konum kimliği}/{dosya adı}.ism/Manifest(format=mpd-time-csf)

## <a name="considerations"></a>Dikkat edilmesi gerekenler
* Varlık Teslim Politikası'nı silmeden önce, varlıkla ilişkili tüm akış bulucularını silmeniz gerekir. Daha sonra yeni bir AssetDeliveryPolicy ile istenirse yeni akış bulucuları oluşturabilirsiniz.
* Kıymet teslim ilkesi ayarlanmıyorsa, depolama şifreli bir varlık üzerinde akış bulucu oluşturulamaz.  Varlık depolama şifresi ne değilse, sistem bir bulucu oluşturmanıza ve varlık teslim ilkesi olmadan varlığı net olarak akışınıza izin verecektir.
* Tek bir varlıkla ilişkili birden çok varlık teslim ilkelerine sahip olabilirsiniz, ancak belirli bir Varlık Teslim Protokolü'nü işlemenin yalnızca bir yolunu belirtebilirsiniz.  Yani, bir istemci Sorunsuz Akış isteği yaptığında hangisini uygulamak istediğinizi bilmediği için hatayla sonuçlanacak AssetDeliveryProtocol.SmoothStreaming protokolünü belirten iki teslim ilkesini bağlamaya çalışırsanız.
* Varolan bir akış bulucusu olan bir kıymetiniz varsa, kıymete yeni bir ilke bağlayamazsınız (varlığa varolan bir ilkeyi bağlayabilirsiniz veya varlıkla ilişkili bir teslimat ilkesini güncelleştirebilirsiniz).  Önce akış bulucuyu kaldırmanız, ilkeleri ayarlamanız ve ardından akış bulucuyu yeniden oluşturmanız gerekir.  Akış bulucuyu yeniden oluştururken aynı locatorId'i kullanabilirsiniz, ancak içerik kaynak veya akış aşağı CDN tarafından önbelleğe alınabildiği için istemciler için sorunlara neden olmayacağından emin olmalısınız.

## <a name="clear-asset-delivery-policy"></a>Varlık teslim politikasını temizle

Aşağıdaki **ConfigureClearAssetDeliveryPolicy** yöntemi dinamik şifreleme uygulamamak ve aşağıdaki protokollerden herhangi birinde akışı teslim etmek için belirtir: MPEG DASH, HLS ve Düzgün Akış protokolleri. Bu ilkeyi depolama şifrelenmiş varlıklarınıza uygulamak isteyebilirsiniz.

Bir Varlık Teslim Politikası oluştururken hangi değerleri belirtebileceğiniz hakkında bilgi [için, AssetDeliveryPolicy](#types) bölümünü tanımlarken kullanılan Türler bölümüne bakın.

```csharp
    static public void ConfigureClearAssetDeliveryPolicy(IAsset asset)
    {
        IAssetDeliveryPolicy policy =
        _context.AssetDeliveryPolicies.Create("Clear Policy",
        AssetDeliveryPolicyType.NoDynamicEncryption,
        AssetDeliveryProtocol.HLS | AssetDeliveryProtocol.SmoothStreaming | AssetDeliveryProtocol.Dash, null);
        
        asset.DeliveryPolicies.Add(policy);
    }
```
## <a name="dynamiccommonencryption-asset-delivery-policy"></a>DynamicCommonEncryption varlık teslim ilkesi

Aşağıdaki **CreateAssetDeliveryPolicy** yöntemi, düzgün bir akış protokolüne dinamik ortak şifreleme **(DynamicCommonEncryption)** uygulamak üzere yapılandırılan **AssetDeliveryPolicy'yi** oluşturur (diğer protokollerin akışı engellenir). Yöntem iki parametre alır: **Varlık** (teslimat ilkesini uygulamak istediğiniz varlık) ve **IContentKey** (Daha fazla bilgi için **CommonEncryption** türünün içerik anahtarı, bkz: [İçerik anahtarı oluşturma).](media-services-dotnet-create-contentkey.md#common_contentkey)

Bir Varlık Teslim Politikası oluştururken hangi değerleri belirtebileceğiniz hakkında bilgi [için, AssetDeliveryPolicy](#types) bölümünü tanımlarken kullanılan Türler bölümüne bakın.

```csharp
    static public void CreateAssetDeliveryPolicy(IAsset asset, IContentKey key)
    {
        Uri acquisitionUrl = key.GetKeyDeliveryUrl(ContentKeyDeliveryType.PlayReadyLicense);
        
        Dictionary<AssetDeliveryPolicyConfigurationKey, string> assetDeliveryPolicyConfiguration =
                new Dictionary<AssetDeliveryPolicyConfigurationKey, string>
            {
                {AssetDeliveryPolicyConfigurationKey.PlayReadyLicenseAcquisitionUrl, acquisitionUrl.ToString()},
            };
    
            var assetDeliveryPolicy = _context.AssetDeliveryPolicies.Create(
                    "AssetDeliveryPolicy",
                AssetDeliveryPolicyType.DynamicCommonEncryption,
                AssetDeliveryProtocol.SmoothStreaming,
                assetDeliveryPolicyConfiguration);
    
            // Add AssetDelivery Policy to the asset
            asset.DeliveryPolicies.Add(assetDeliveryPolicy);
    
            Console.WriteLine();
            Console.WriteLine("Adding Asset Delivery Policy: " +
                assetDeliveryPolicy.AssetDeliveryPolicyType);
     }
```

Azure Medya Hizmetleri ayrıca Widevine şifrelemesi eklemenize de olanak tanır. Aşağıdaki örnek, hem PlayReady hem de Widevine'in varlık teslim ilkesine eklendiğini göstermektedir.

```csharp
    static public void CreateAssetDeliveryPolicy(IAsset asset, IContentKey key)
    {
        // Get the PlayReady license service URL.
        Uri acquisitionUrl = key.GetKeyDeliveryUrl(ContentKeyDeliveryType.PlayReadyLicense);


        // GetKeyDeliveryUrl for Widevine attaches the KID to the URL.
        // For example: https://amsaccount1.keydelivery.mediaservices.windows.net/Widevine/?KID=268a6dcb-18c8-4648-8c95-f46429e4927c.  
        // The WidevineBaseLicenseAcquisitionUrl (used below) also tells Dynamic Encryption 
        // to append /? KID =< keyId > to the end of the url when creating the manifest.
        // As a result Widevine license acquisition URL will have KID appended twice, 
        // so we need to remove the KID that in the URL when we call GetKeyDeliveryUrl.

        Uri widevineUrl = key.GetKeyDeliveryUrl(ContentKeyDeliveryType.Widevine);
        UriBuilder uriBuilder = new UriBuilder(widevineUrl);
        uriBuilder.Query = String.Empty;
        widevineUrl = uriBuilder.Uri;

        Dictionary<AssetDeliveryPolicyConfigurationKey, string> assetDeliveryPolicyConfiguration =
            new Dictionary<AssetDeliveryPolicyConfigurationKey, string>
        {
            {AssetDeliveryPolicyConfigurationKey.PlayReadyLicenseAcquisitionUrl, acquisitionUrl.ToString()},
            {AssetDeliveryPolicyConfigurationKey.WidevineLicenseAcquisitionUrl, widevineUrl.ToString()}

        };

        var assetDeliveryPolicy = _context.AssetDeliveryPolicies.Create(
                "AssetDeliveryPolicy",
            AssetDeliveryPolicyType.DynamicCommonEncryption,
            AssetDeliveryProtocol.Dash,
            assetDeliveryPolicyConfiguration);


        // Add AssetDelivery Policy to the asset
        asset.DeliveryPolicies.Add(assetDeliveryPolicy);

    }
```
> [!NOTE]
> Widevine ile şifrelerken, yalnızca DASH kullanarak teslim edebilirsiniz. Varlık teslim protokolünde DASH'i belirttiğinden emin olun.
> 
> 

## <a name="dynamicenvelopeencryption-asset-delivery-policy"></a>DynamicEnvelopeŞifreleme varlık teslim ilkesi
Aşağıdaki **CreateAssetDeliveryPolicy** yöntemi, Düzgün Akış, HLS ve DASH protokollerine dinamik zarf şifrelemesi **(DynamicEnvelopeEncryption)** uygulamak üzere yapılandırılan **AssetDeliveryPolicy'yi** oluşturur (bazı protokolleri belirtmemeye karar verirseniz, bunların akışı engellenir). Yöntem iki parametre alır: **Varlık** (teslimat ilkesini uygulamak istediğiniz varlık) ve **IContentKey** **(ZarfŞifreleme** türünün içerik anahtarı, daha fazla bilgi için bkz: [İçerik anahtarı oluşturma).](media-services-dotnet-create-contentkey.md#envelope_contentkey)

Bir Varlık Teslim Politikası oluştururken hangi değerleri belirtebileceğiniz hakkında bilgi [için, AssetDeliveryPolicy](#types) bölümünü tanımlarken kullanılan Türler bölümüne bakın.   

```csharp
    private static void CreateAssetDeliveryPolicy(IAsset asset, IContentKey key)
    {

        //  Get the Key Delivery Base Url by removing the Query parameter.  The Dynamic Encryption service will
        //  automatically add the correct key identifier to the url when it generates the Envelope encrypted content
        //  manifest.  Omitting the IV will also cause the Dynamic Encryption service to generate a deterministic
        //  IV for the content automatically.  By using the EnvelopeBaseKeyAcquisitionUrl and omitting the IV, this
        //  allows the AssetDelivery policy to be reused by more than one asset.
        //
        Uri keyAcquisitionUri = key.GetKeyDeliveryUrl(ContentKeyDeliveryType.BaselineHttp);
        UriBuilder uriBuilder = new UriBuilder(keyAcquisitionUri);
        uriBuilder.Query = String.Empty;
        keyAcquisitionUri = uriBuilder.Uri;

        // The following policy configuration specifies: 
        //   key url that will have KID=<Guid> appended to the envelope and
        //   the Initialization Vector (IV) to use for the envelope encryption.
        Dictionary<AssetDeliveryPolicyConfigurationKey, string> assetDeliveryPolicyConfiguration =
            new Dictionary<AssetDeliveryPolicyConfigurationKey, string> 
        {
            {AssetDeliveryPolicyConfigurationKey.EnvelopeBaseKeyAcquisitionUrl, keyAcquisitionUri.ToString()},
        };

        IAssetDeliveryPolicy assetDeliveryPolicy =
            _context.AssetDeliveryPolicies.Create(
                        "AssetDeliveryPolicy",
                        AssetDeliveryPolicyType.DynamicEnvelopeEncryption,
                        AssetDeliveryProtocol.SmoothStreaming | AssetDeliveryProtocol.HLS | AssetDeliveryProtocol.Dash,
                        assetDeliveryPolicyConfiguration);

        // Add AssetDelivery Policy to the asset
        asset.DeliveryPolicies.Add(assetDeliveryPolicy);

        Console.WriteLine();
        Console.WriteLine("Adding Asset Delivery Policy: " + assetDeliveryPolicy.AssetDeliveryPolicyType);
    }
```

## <a name="types-used-when-defining-assetdeliverypolicy"></a><a id="types"></a>AssetDeliveryPolicy tanımlanırken kullanılan türleri

### <a name="assetdeliveryprotocol"></a><a id="AssetDeliveryProtocol"></a>Varlık Teslim Protokolü

Aşağıdaki enum, varlık teslim protokolü için ayarlayabildiğiniz değerleri açıklar.

```csharp
    [Flags]
    public enum AssetDeliveryProtocol
    {
        /// <summary>
        /// No protocols.
        /// </summary>
        None = 0x0,

        /// <summary>
        /// Smooth streaming protocol.
        /// </summary>
        SmoothStreaming = 0x1,

        /// <summary>
        /// MPEG Dynamic Adaptive Streaming over HTTP (DASH)
        /// </summary>
        Dash = 0x2,

        /// <summary>
        /// Apple HTTP Live Streaming protocol.
        /// </summary>
        HLS = 0x4,

        ProgressiveDownload = 0x10, 
 
        /// <summary>
        /// Include all protocols.
        /// </summary>
        All = 0xFFFF
    }
```
### <a name="assetdeliverypolicytype"></a><a id="AssetDeliveryPolicyType"></a>Varlık TeslimPolitikasıTürü

Aşağıdaki enum, varlık teslim ilkesi türü için ayarlayabildiğiniz değerleri açıklar.  
```csharp
    public enum AssetDeliveryPolicyType
    {
        /// <summary>
        /// Delivery Policy Type not set.  An invalid value.
        /// </summary>
        None,

        /// <summary>
        /// The Asset should not be delivered via this AssetDeliveryProtocol. 
        /// </summary>
        Blocked, 

        /// <summary>
        /// Do not apply dynamic encryption to the asset.
        /// </summary>
        /// 
        NoDynamicEncryption,  

        /// <summary>
        /// Apply Dynamic Envelope encryption.
        /// </summary>
        DynamicEnvelopeEncryption,

        /// <summary>
        /// Apply Dynamic Common encryption.
        /// </summary>
        DynamicCommonEncryption
        }
```
### <a name="contentkeydeliverytype"></a><a id="ContentKeyDeliveryType"></a>İçerikKeyDeliveryType

Aşağıdaki enum, istemciye içerik anahtarının teslim yöntemini yapılandırmak için kullanabileceğiniz değerleri açıklar.
  ```csharp  
    public enum ContentKeyDeliveryType
    {
        /// <summary>
        /// None.
        ///
        </summary>
        None = 0,

        /// <summary>
        /// Use PlayReady License acquisition protocol
        ///
        </summary>
        PlayReadyLicense = 1,

        /// <summary>
        /// Use MPEG Baseline HTTP key protocol.
        ///
        </summary>
        BaselineHttp = 2,

        /// <summary>
        /// Use Widevine License acquisition protocol
        ///
        </summary>
        Widevine = 3

    }
```
### <a name="assetdeliverypolicyconfigurationkey"></a><a id="AssetDeliveryPolicyConfigurationKey"></a>VarlıkDeliveryPolicyConfigurationKey

Aşağıdaki enum, varlık teslim ilkesi için belirli yapılandırma almak için kullanılan anahtarları yapılandırmak üzere ayarlayabildiğiniz değerleri açıklar.
```csharp
    public enum AssetDeliveryPolicyConfigurationKey
    {
        /// <summary>
        /// No policies.
        /// </summary>
        None,

        /// <summary>
        /// Exact Envelope key URL.
        /// </summary>
        EnvelopeKeyAcquisitionUrl,

        /// <summary>
        /// Base key url that will have KID=<Guid> appended for Envelope.
        /// </summary>
        EnvelopeBaseKeyAcquisitionUrl,

        /// <summary>
        /// The initialization vector to use for envelope encryption in Base64 format.
        /// </summary>
        EnvelopeEncryptionIVAsBase64,

        /// <summary>
        /// The PlayReady License Acquisition Url to use for common encryption.
        /// </summary>
        PlayReadyLicenseAcquisitionUrl,

        /// <summary>
        /// The PlayReady Custom Attributes to add to the PlayReady Content Header
        /// </summary>
        PlayReadyCustomAttributes,

        /// <summary>
        /// The initialization vector to use for envelope encryption.
        /// </summary>
        EnvelopeEncryptionIV,

        /// <summary>
        /// Widevine DRM acquisition url
        /// </summary>
        WidevineLicenseAcquisitionUrl
    }
```

## <a name="additional-notes"></a>Ek notlar

* Widevine, Google Inc. tarafından sağlanan ve Google, Inc.'in hizmet koşullarına ve Gizlilik Politikasına tabi olan bir hizmettir.

## <a name="media-services-learning-paths"></a>Media Services’i öğrenme yolları
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

