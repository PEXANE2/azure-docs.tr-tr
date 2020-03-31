---
title: Medya Hizmetleri REST API'yi kullanarak varlık teslim ilkelerini yapılandırma | Microsoft Dokümanlar
description: Bu konu, Media Services REST API'yi kullanarak farklı varlık teslim ilkelerinin nasıl yapılandırılabildiğini gösterir.
services: media-services
documentationcenter: ''
author: Juliako
manager: cfowler
editor: ''
ms.assetid: 5cb9d32a-e68b-4585-aa82-58dded0691d0
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/20/2019
ms.author: juliako
ms.openlocfilehash: 09f0371bc189fcf7b25ec3261e2e1f5eaf1892ae
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78194522"
---
# <a name="configuring-asset-delivery-policies"></a>Varlık teslim ilkelerini yapılandırma
[!INCLUDE [media-services-selector-asset-delivery-policy](../../../includes/media-services-selector-asset-delivery-policy.md)]

Dinamik olarak şifrelenmiş varlıklar sunmayı planlıyorsanız, Medya Hizmetleri içerik teslim iş akışındaki adımlardan biri varlıklar için teslim ilkelerini yapılandırmaktır. Varlık teslim ihdası ilkesi Medya Hizmetleri'ne varlığınızın nasıl teslim edilmesini istediğinizi bildirir: varlığınızın dinamik olarak paketlenmesi gereken akış protokolü (örneğin, MPEG DASH, HLS, Düzgün Akış veya tümü), dinamik olarak şifrelemek isteyip istemediğiniz varlık ve nasıl (zarf veya ortak şifreleme).

Bu konu, varlık teslim ilkelerinin neden ve nasıl oluşturulup yapılabildiğini tartışır.

> [!NOTE]
> AMS hesabınız **oluşturulduğunda,** **Durduruldu** durumunda hesabınıza varsayılan akış bitiş noktası eklenir. İçerik akışını başlatmak ve dinamik paketleme ile dinamik şifrelemeden yararlanmak için içerik akışı yapmak istediğiniz akış uç noktasının **Çalışıyor** durumda olması gerekir. 
>
> Ayrıca, dinamik paketleme ve dinamik şifreleme kullanabilmek için kıymetinizin bir dizi uyarlanabilir bithızı MP4 veya uyarlanabilir bithızı Düzgün Akış dosyaları içermesi gerekir.

Aynı varlığa farklı ilkeler uygulayabilirsiniz. Örneğin, PlayReady şifrelemesini Sorunsuz Akış ve AES Zarf şifrelemesine MPEG DASH ve HLS'ye uygulayabilirsiniz. Herhangi bir teslim ilkesinde tanımlanmayan tüm protokollerin (örneğin, protokol olarak yalnızca HLS‘yi belirten tek bir ilke ekliyorsunuz) akışla aktarılması engellenir. Bunun tek istisnası, hiçbir varlık teslim ilkesinin tanımlanmadığı durumdur. Bu halde tüm protokollere açık bir şekilde izin verilir.

Depolama şifreli bir varlık teslim etmek istiyorsanız, varlığın teslim ilkesini yapılandırmanız gerekir. Varlığınız akışa geçmeden önce, akış sunucusu depolama şifrelemesini kaldırır ve belirtilen teslim ilkesini kullanarak içeriğinizi akışı sağlar. Örneğin, Varlık durumunuzu Gelişmiş Şifreleme Standardı (AES) zarf şifreleme anahtarıyla şifrelenmiş olarak teslim etmek için ilke türünü **DynamicEnvelopeEncryption**olarak ayarlayın. Depolama şifrelemesini kaldırmak ve varlığı açık olarak aktarmak için ilke türünü **NoDinamik Şifreleme**olarak ayarlayın. Bu ilke türlerinin nasıl yapılandırılabildiğini gösteren örnekler aşağıdaki gibidir.

Varlık teslim politikasını nasıl yapılandırdığınıza bağlı olarak, aşağıdaki akış protokollerini dinamik olarak paketleyip, dinamik olarak şifrelendirebilir ve akış alabilirsiniz: Düzgün Akış, HLS, MPEG DASH akışları.

Aşağıdaki liste, Düz, HLS, DASH akışı için kullandığınız biçimleri gösterir.

Düzgün Akış:

{akış uç noktası adı-media services hesabı adı}.streaming.mediaservices.windows.net/{konum kimliği}/{dosya adı}.ism/Manifest

Hls:

{akış uç noktası adı-media services hesabı adı}.streaming.mediaservices.windows.net/{konum kimliği}/{dosya adı}.ism/Manifest(format=m3u8-aapl)

MPEG DASH

{akış uç noktası adı-media services hesabı adı}.streaming.mediaservices.windows.net/{konum kimliği}/{dosya adı}.ism/Manifest(format=mpd-time-csf)


Varlık yayımlama ve akış URL'si oluşturma yönergeleri için bkz. [Akış URL'si oluşturma](media-services-deliver-streaming-content.md).

## <a name="considerations"></a>Dikkat edilmesi gerekenler
* Bir Varlık la ilişkili bir Varlık Teslim Politikası'nı, bu varlık için OnDemand (akış) bulucu varken silemezsiniz. Öneri, ilkeyi silmeden önce ilkeyi kıymetten kaldırmaktır.
* Kıymet teslim ilkesi ayarlanmıyorsa, depolama şifreli bir varlık üzerinde akış bulucu oluşturulamaz.  Varlık depolama şifresi ne değilse, sistem bir bulucu oluşturmanıza ve varlık teslim ilkesi olmadan varlığı net olarak akışınıza izin verecektir.
* Tek bir varlıkla ilişkili birden çok varlık teslim ilkelerine sahip olabilirsiniz, ancak belirli bir Varlık Teslim Protokolü'nü işlemenin yalnızca bir yolunu belirtebilirsiniz.  Yani, bir istemci Sorunsuz Akış isteği yaptığında hangisini uygulamak istediğinizi bilmediği için hatayla sonuçlanacak AssetDeliveryProtocol.SmoothStreaming protokolünü belirten iki teslim ilkesini bağlamaya çalışırsanız.
* Varolan bir akış bulucusu olan bir kıymetiniz varsa, kıymete yeni bir ilke bağlayamaz, varolan bir ilkeyi varlıktan çıkaramaz veya varlıkla ilişkili bir teslimat ilkesini güncelleştiremezsiniz.  Önce akış bulucuyu kaldırmanız, ilkeleri ayarlamanız ve ardından akış bulucuyu yeniden oluşturmanız gerekir.  Akış bulucuyu yeniden oluştururken aynı locatorId'i kullanabilirsiniz, ancak içerik kaynak veya akış aşağı CDN tarafından önbelleğe alınabildiği için istemciler için sorunlara neden olmayacağından emin olmalısınız.

> [!NOTE]
> 
> Medya Hizmetleri'ndeki varlıklara erişirken, HTTP isteklerinizde belirli üstbilgi alanları ve değerleri belirlemeniz gerekir. Daha fazla bilgi için Medya [Hizmetleri REST API Geliştirme kurulumu'na](media-services-rest-how-to-use.md)bakın.

## <a name="connect-to-media-services"></a>Media Services’e bağlanmak

AMS API'sine nasıl bağlanabileceğiniz hakkında bilgi için Azure [AD kimlik doğrulaması yla Azure Medya Hizmetleri API'sine eriş'e](media-services-use-aad-auth-to-access-ams-api.md)bakın. 

## <a name="clear-asset-delivery-policy"></a>Varlık teslim politikasını temizle
### <a name="create-asset-delivery-policy"></a><a id="create_asset_delivery_policy"></a>Varlık teslim ilkesi oluşturma
Aşağıdaki HTTP isteği, dinamik şifreleme uygulamamayı ve akışı aşağıdaki protokollerden herhangi birinde teslim etmeyi belirten bir varlık teslim ilkesi oluşturur: MPEG DASH, HLS ve Düzgün Akış protokolleri. 

Bir Varlık Teslim Politikası oluştururken hangi değerleri belirtebileceğiniz hakkında bilgi [için, AssetDeliveryPolicy](#types) bölümünü tanımlarken kullanılan Türler bölümüne bakın.   

İstek:

    POST https://media.windows.net/api/AssetDeliveryPolicies HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer <ENCODED JWT TOKEN> 
    x-ms-version: 2.19
    x-ms-client-request-id: 4651882c-d7ad-4d5e-86ab-f07f47dcb41e
    Host: media.windows.net

    {"Name":"Clear Policy",
    "AssetDeliveryProtocol":7,
    "AssetDeliveryPolicyType":2,
    "AssetDeliveryConfiguration":null}

Yanıt:

    HTTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 363
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Location: https://media.windows.net/api/AssetDeliveryPolicies('nb%3Aadpid%3AUUID%3A92b0f6ba-3c9f-49b6-a5fa-2a8703b04ecd')
    Server: Microsoft-IIS/8.5
    x-ms-client-request-id: 4651882c-d7ad-4d5e-86ab-f07f47dcb41e
    request-id: 6aedbf93-4bc2-4586-8845-fd45590136af
    x-ms-request-id: 6aedbf93-4bc2-4586-8845-fd45590136af
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Sun, 08 Feb 2015 06:21:27 GMT

    {"odata.metadata":"https://media.windows.net/api/$metadata#AssetDeliveryPolicies/@Element",
    "Id":"nb:adpid:UUID:92b0f6ba-3c9f-49b6-a5fa-2a8703b04ecd",
    "Name":"Clear Policy",
    "AssetDeliveryProtocol":7,
    "AssetDeliveryPolicyType":2,
    "AssetDeliveryConfiguration":null,
    "Created":"2015-02-08T06:21:27.6908329Z",
    "LastModified":"2015-02-08T06:21:27.6908329Z"}

### <a name="link-asset-with-asset-delivery-policy"></a><a id="link_asset_with_asset_delivery_policy"></a>Varlığı varlık teslim ilkesiyle ilişkilendirin
Aşağıdaki HTTP isteği, belirtilen varlığı varlık teslim ilkesine bağlar.

İstek:

    POST https://media.windows.net/api/Assets('nb%3Acid%3AUUID%3A86933344-9539-4d0c-be7d-f842458693e0')/$links/DeliveryPolicies HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Content-Type: application/json
    Authorization: Bearer <ENCODED JWT TOKEN> 
    x-ms-version: 2.19
    x-ms-client-request-id: 56d2763f-6e72-419d-ba3c-685f6db97e81
    Host: media.windows.net

    {"uri":"https://media.windows.net/api/AssetDeliveryPolicies('nb%3Aadpid%3AUUID%3A92b0f6ba-3c9f-49b6-a5fa-2a8703b04ecd')"}

Yanıt:

    HTTP/1.1 204 No Content


## <a name="dynamicenvelopeencryption-asset-delivery-policy"></a>DynamicEnvelopeŞifreleme varlık teslim ilkesi
### <a name="create-content-key-of-the-envelopeencryption-type-and-link-it-to-the-asset"></a>EnvelopeEncryption türünün içerik anahtarını oluşturun ve varlığa bağla
DynamicEnvelopeEncryption teslim ilkesini belirtirken, varlığınızı EnvelopeEncryption türündeki bir içerik anahtarına bağladiğinizden emin olmanız gerekir. Daha fazla bilgi için bkz: [İçerik anahtarı oluşturma).](media-services-rest-create-contentkey.md)

### <a name="get-delivery-url"></a><a id="get_delivery_url"></a>Teslimat URL'si alın
Önceki adımda oluşturulan içerik anahtarının belirtilen teslim yönteminin teslim URL'sini alın. İstemci, korumalı içeriği oynatmak için bir AES anahtarı veya PlayReady lisansı istemek için döndürülen URL'yi kullanır.

HTTP isteğinin gövdesine girmek için URL'nin türünü belirtin. İçeriğinizi PlayReady ile koruyorsanız, keyDeliveryType için 1 kullanarak bir Medya Hizmetleri PlayReady lisans edinme URL'si isteyin: {"keyDeliveryType":1}. Zarf şifrelemesiyle içeriğinizi koruyorsanız, keyDeliveryType için 2 belirterek bir anahtar edinme URL'si isteyin: {"keyDeliveryType":2}.

İstek:

    POST https://media.windows.net/api/ContentKeys('nb:kid:UUID:dc88f996-2859-4cf7-a279-c52a9d6b2f04')/GetKeyDeliveryUrl HTTP/1.1
    Content-Type: application/json
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer <ENCODED JWT TOKEN> 
    x-ms-version: 2.19
    x-ms-client-request-id: 569d4b7c-a446-4edc-b77c-9fb686083dd8
    Host: media.windows.net
    Content-Length: 21

    {"keyDeliveryType":2}

Yanıt:

    HTTP/1.1 200 OK
    Cache-Control: no-cache
    Content-Length: 198
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Server: Microsoft-IIS/8.5
    x-ms-client-request-id: 569d4b7c-a446-4edc-b77c-9fb686083dd8
    request-id: d26f65d2-fe65-4136-8fcf-31545be68377
    x-ms-request-id: d26f65d2-fe65-4136-8fcf-31545be68377
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Sun, 08 Feb 2015 21:42:30 GMT

    {"odata.metadata":"media.windows.net/api/$metadata#Edm.String","value":"https://amsaccount1.keydelivery.mediaservices.windows.net/?KID=dc88f996-2859-4cf7-a279-c52a9d6b2f04"}


### <a name="create-asset-delivery-policy"></a>Varlık teslim ilkesi oluşturma
Aşağıdaki HTTP isteği, **HLS** protokolüne dinamik zarf şifrelemesi **(DynamicEnvelopeEncryption)** uygulamak üzere yapılandırılan **AssetDeliveryPolicy'yi** oluşturur (bu örnekte, diğer protokollerin akışı engellenir). 

Bir Varlık Teslim Politikası oluştururken hangi değerleri belirtebileceğiniz hakkında bilgi [için, AssetDeliveryPolicy](#types) bölümünü tanımlarken kullanılan Türler bölümüne bakın.   

İstek:

    POST https://media.windows.net/api/AssetDeliveryPolicies HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    User-Agent: Microsoft ADO.NET Data Services
    Authorization: Bearer <ENCODED JWT TOKEN> 
    x-ms-version: 2.19
    x-ms-client-request-id: fff319f6-71dd-4f6c-af27-b675c0066fa7
    Host: media.windows.net

    {"Name":"AssetDeliveryPolicy","AssetDeliveryProtocol":4,"AssetDeliveryPolicyType":3,"AssetDeliveryConfiguration":"[{\"Key\":2,\"Value\":\"https:\\/\\/amsaccount1.keydelivery.mediaservices.windows.net\\/\"}]"}


Yanıt:

    HTTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 460
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Location: media.windows.net/api/AssetDeliveryPolicies('nb%3Aadpid%3AUUID%3Aec9b994e-672c-4a5b-8490-a464eeb7964b')
    Server: Microsoft-IIS/8.5
    x-ms-client-request-id: fff319f6-71dd-4f6c-af27-b675c0066fa7
    request-id: c2a1ac0e-9644-474f-b38f-b9541c3a7c5f
    x-ms-request-id: c2a1ac0e-9644-474f-b38f-b9541c3a7c5f
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Mon, 09 Feb 2015 05:24:38 GMT

    {"odata.metadata":"media.windows.net/api/$metadata#AssetDeliveryPolicies/@Element","Id":"nb:adpid:UUID:ec9b994e-672c-4a5b-8490-a464eeb7964b","Name":"AssetDeliveryPolicy","AssetDeliveryProtocol":4,"AssetDeliveryPolicyType":3,"AssetDeliveryConfiguration":"[{\"Key\":2,\"Value\":\"https:\\/\\/amsaccount1.keydelivery.mediaservices.windows.net\\/\"}]","Created":"2015-02-09T05:24:38.9167436Z","LastModified":"2015-02-09T05:24:38.9167436Z"}


### <a name="link-asset-with-asset-delivery-policy"></a>Varlığı varlık teslim ilkesiyle ilişkilendirin
[Varlık teslim ilkesine sahip Bağlantı varlığına](#link_asset_with_asset_delivery_policy) bakın

## <a name="dynamiccommonencryption-asset-delivery-policy"></a>DynamicCommonEncryption varlık teslim ilkesi
### <a name="create-content-key-of-the-commonencryption-type-and-link-it-to-the-asset"></a>CommonEncryption türünde içerik anahtarı oluşturun ve varlığa bağla
DynamicCommonEncryption teslim ilkesini belirtirken, varlığınızı CommonEncryption türündeki bir içerik anahtarına bağladiğinizden emin olmanız gerekir. Daha fazla bilgi için bkz: [İçerik anahtarı oluşturma).](media-services-rest-create-contentkey.md)

### <a name="get-delivery-url"></a>Teslimat URL'si alın
Önceki adımda oluşturulan içerik anahtarının PlayReady teslim yönteminin teslim URL'sini alın. İstemci, korumalı içeriği oynatmak için playready lisansı istemek için döndürülen URL'yi kullanır. Daha fazla bilgi için teslim url'si [al'a](#get_delivery_url)bakın.

### <a name="create-asset-delivery-policy"></a>Varlık teslim ilkesi oluşturma
Aşağıdaki HTTP isteği, **Düzgün Akış** protokolüne dinamik ortak şifreleme **(DynamicCommonEncryption)** uygulamak üzere yapılandırılan **AssetDeliveryPolicy'yi** oluşturur (bu örnekte, diğer protokollerin akışı engellenir). 

Bir Varlık Teslim Politikası oluştururken hangi değerleri belirtebileceğiniz hakkında bilgi [için, AssetDeliveryPolicy](#types) bölümünü tanımlarken kullanılan Türler bölümüne bakın.   

İstek:

    POST https://media.windows.net/api/AssetDeliveryPolicies HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    User-Agent: Microsoft ADO.NET Data Services
    Authorization: Bearer <ENCODED JWT TOKEN> 
    x-ms-version: 2.19
    x-ms-client-request-id: fff319f6-71dd-4f6c-af27-b675c0066fa7
    Host: media.windows.net

    {"Name":"AssetDeliveryPolicy","AssetDeliveryProtocol":1,"AssetDeliveryPolicyType":4,"AssetDeliveryConfiguration":"[{\"Key\":2,\"Value\":\"https:\\/\\/amsaccount1.keydelivery.mediaservices.windows.net\/PlayReady\/"}]"}


Widevine DRM kullanarak içeriğinizi korumak istiyorsanız, WidevineLicenseAcquisitionUrl (7 değerine sahip) kullanmak için AssetDeliveryConfiguration değerlerini güncelleyin ve bir lisans teslim hizmetinin URL'sini belirtin. Widevine lisanslarını sunmanıza yardımcı olmak için aşağıdaki AMS ortaklarını kullanabilirsiniz: [Aksinom,](https://www.axinom.com) [EZDRM,](https://ezdrm.com/) [castLabs.](https://castlabs.com/company/partners/azure/)

Örnek: 

    {"Name":"AssetDeliveryPolicy","AssetDeliveryProtocol":2,"AssetDeliveryPolicyType":4,"AssetDeliveryConfiguration":"[{\"Key\":7,\"Value\":\"https:\\/\\/example.net\/WidevineLicenseAcquisition\/"}]"}

> [!NOTE]
> Widevine ile şifrelerken, yalnızca DASH kullanarak teslim edebilirsiniz. Varlık teslim protokolünde DASH (2) belirttiğinden emin olun.
> 
> 

### <a name="link-asset-with-asset-delivery-policy"></a>Varlığı varlık teslim ilkesiyle ilişkilendirin
[Varlık teslim ilkesine sahip Bağlantı varlığına](#link_asset_with_asset_delivery_policy) bakın

## <a name="types-used-when-defining-assetdeliverypolicy"></a><a id="types"></a>AssetDeliveryPolicy tanımlanırken kullanılan türleri

### <a name="assetdeliveryprotocol"></a>Varlık Teslim Protokolü

Aşağıdaki enum, varlık teslim protokolü için ayarlayabildiğiniz değerleri açıklar.

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

### <a name="assetdeliverypolicytype"></a>Varlık TeslimPolitikasıTürü

Aşağıdaki enum, varlık teslim ilkesi türü için ayarlayabildiğiniz değerleri açıklar.  

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

### <a name="contentkeydeliverytype"></a>İçerikKeyDeliveryType

Aşağıdaki enum, istemciye içerik anahtarının teslim yöntemini yapılandırmak için kullanabileceğiniz değerleri açıklar.
    
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


### <a name="assetdeliverypolicyconfigurationkey"></a>VarlıkDeliveryPolicyConfigurationKey

Aşağıdaki enum, varlık teslim ilkesi için belirli yapılandırma almak için kullanılan anahtarları yapılandırmak üzere ayarlayabildiğiniz değerleri açıklar.

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

## <a name="additional-notes"></a>Ek notlar

* Widevine, Google Inc. tarafından sağlanan ve Google, Inc.'in hizmet koşullarına ve Gizlilik Politikasına tabi olan bir hizmettir.

## <a name="media-services-learning-paths"></a>Media Services’i öğrenme yolları
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

