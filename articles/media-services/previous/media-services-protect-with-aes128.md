---
title: AES-128 dinamik şifreleme ve anahtar teslim hizmetini kullanın | Microsoft Dokümanlar
description: Bu konu, AES-128 ile dinamik olarak nasıl şifrelenir ve anahtar teslim hizmetini nasıl kullanacağız gösterir.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.assetid: 4d2c10af-9ee0-408f-899b-33fa4c1d89b9
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/01/2019
ms.author: juliako
ms.openlocfilehash: 01153317b49e4543f10faa517bce7bcc01ce22d4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79269737"
---
# <a name="use-aes-128-dynamic-encryption-and-the-key-delivery-service"></a>AES-128 dinamik şifreleme ve anahtar dağıtım hizmetini kullanma
> [!div class="op_single_selector"]
> * [.NET](media-services-protect-with-aes128.md)
> * [Java](https://github.com/southworkscom/azure-sdk-for-media-services-java-samples)
> * [PHP](https://github.com/Azure/azure-sdk-for-php/tree/master/examples/MediaServices)
>  

> [!NOTE]
> Media Services v2’ye herhangi bir yeni özellik veya işlevsellik eklenmemektedir. <br/>En son sürümü göz atın, [Medya Hizmetleri v3](https://docs.microsoft.com/azure/media-services/latest/). Ayrıca, [v2'den v3'e geçiş kılavuzuna](../latest/migrate-from-v2-to-v3.md) bakın

128 bit şifreleme anahtarlarını kullanarak AES ile şifrelenmiş HTTP Live Streaming (HLS) ve Smooth Streaming sunmak için Medya Hizmetlerini kullanabilirsiniz. Medya Hizmetleri, yetkili kullanıcılara şifreleme anahtarları sağlayan anahtar teslim hizmetini de sağlar. Medya Hizmetleri'nin bir varlığı şifrelemesini istiyorsanız, bir şifreleme anahtarını varlıkla ilişkilendirin ve anahtar için yetkilendirme ilkelerini yapılandırın. Bir akış bir oynatıcı tarafından istendiğinde, Medya Hizmetleri AES şifrelemesi kullanarak içeriğinizi dinamik olarak şifrelemek için belirtilen anahtarı kullanır. Oynatıcı, akışın şifresini çözmek için anahtar teslim hizmetinden anahtarı ister. Kullanıcının anahtarı almaya yetkili olup olmadığını belirlemek için, hizmet anahtar için belirlediğiniz yetkilendirme ilkelerini değerlendirir.

Media Services, anahtar isteğinde bulunan kullanıcıların kimlik doğrulamasını yapmanın birden çok yöntemini destekler. İçerik anahtarı yetkilendirme ilkesinin açık veya belirteç kısıtlaması şeklinde bir veya daha fazla yetkilendirme kısıtlaması olabilir. Belirteç kısıtlamalı ilkenin beraberinde bir güvenlik belirteci hizmeti (STS) tarafından verilmiş bir belirteç bulunmalıdır. Media Services, [basit web belirteci](https://msdn.microsoft.com/library/gg185950.aspx#BKMK_2) (SWT) ve [JSON Web Token](https://msdn.microsoft.com/library/gg185950.aspx#BKMK_3) (JWT) biçimlerindeki belirteçleri destekler. Daha fazla bilgi edinmek için bkz. [İçerik anahtarının yetkilendirme ilkesini yapılandırma](media-services-protect-with-aes128.md#configure_key_auth_policy).

Dinamik şifrelemeden yararlanmak için, bir grup çoklu bit hızlı MP4 dosyası ya da çoklu bit hızlı Kesintisiz Akış kaynak dosyası içeren bir varlığınız olması gerekir. Ayrıca varlığın teslim ilkesini yapılandırmanız gerekir (bu makalede daha sonra açıklanan). Ardından, akış URL'sinde belirtilen biçime bağlı olarak, isteğe bağlı akış sunucusu akışın seçtiğiniz protokolde teslim edilmesini sağlar. Sonuç olarak, yalnızca dosyaları tek depolama biçiminde depolamanız ve ödeme yapmanız gerekir. Media Services, bir istemciden alınan isteklere göre uygun yanıtı oluşturur ve sunar.

Bu makale, korumalı ortam sağlayan uygulamalar üzerinde çalışan geliştiriciler için yararlıdır. Makalede, yalnızca yetkili istemcilerin şifreleme anahtarları alabilmeleri için anahtar teslim hizmetini yetkilendirme ilkeleriyle nasıl yapılandırabileceğinizi gösterir. Ayrıca dinamik şifrelemenin nasıl kullanılacağını da gösterir.

MacOS'ta Safari'ye teslim için Gelişmiş Şifreleme Standardı (AES) ile içeriğin nasıl şifrelenirim hakkında bilgi için [bu blog gönderisine](https://azure.microsoft.com/blog/how-to-make-token-authorized-aes-encrypted-hls-stream-working-in-safari/)bakın.
Medya içeriğinizi AES şifrelemesiyle nasıl koruyacağınıza genel bir bakış için [bu videoyu](https://channel9.msdn.com/Shows/Azure-Friday/Azure-Media-Services-Protecting-your-Media-Content-with-AES-Encryption)izleyin.


## <a name="aes-128-dynamic-encryption-and-key-delivery-service-workflow"></a>AES-128 dinamik şifreleme ve anahtar teslim hizmeti iş akışı

Medya Hizmetleri anahtar teslim hizmetini kullanarak ve ayrıca dinamik şifreleme kullanarak varlıklarınızı AES ile şifrelediğinizde aşağıdaki genel adımları gerçekleştirin:

1. [Bir varlık oluşturun ve varlığa dosya yükleyin.](media-services-protect-with-aes128.md#create_asset)

2. [Dosyayı içeren varlığı uyarlanabilir bitrate MP4 kümesine kodlayın.](media-services-protect-with-aes128.md#encode_asset)

3. [Bir içerik anahtarı oluşturun ve kodlanmış varlıkla ilişkilendirin.](media-services-protect-with-aes128.md#create_contentkey) Media Services’de, içerik anahtarı varlığın şifreleme anahtarını içerir.

4. [İçerik anahtarının yetkilendirme ilkesini yapılandırın.](media-services-protect-with-aes128.md#configure_key_auth_policy) İçerik anahtarı yetkilendirme ilkesini yapılandırmanız gerekir. İçerik anahtarının istemciye teslim edilebilmesi için önce istemcinin ilkeyi karşılaması gerekir.

5. [Bir varlığın teslim ilkesini yapılandırın.](media-services-protect-with-aes128.md#configure_asset_delivery_policy) Teslim ilkesi yapılandırması anahtar edinme URL'sini ve bir başlatma vektörü (IV) içerir. (AES-128 şifreleme ve şifre çözme için aynı IV gerektirir.) Yapılandırma da teslim protokolü (örneğin, MPEG-DASH, HLS, Düzgün Akış veya tüm) ve dinamik şifreleme türü (örneğin, zarf veya hiçbir dinamik şifreleme) içerir.

    Bir varlıktaki her bir protokole farklı birer ilke uygulayabilirsiniz. Örneğin, Kesintisiz/DASH için PlayReady şifreleme uygularken HLS için bir AES zarfı uygulayabilirsiniz. Bir teslim ilkesinde tanımlanmayan tüm protokollerin akışı engellenir. (Bir örnek, protokol olarak yalnızca HLS'yi belirten tek bir ilke eklerseniz.) İstisna, tanımlanmış hiçbir varlık teslim ilkenin olmamasıdır. Bu halde tüm protokollere açık bir şekilde izin verilir.

6. Akış URL'si almak için [Bir OnDemand bulucu oluşturun.](media-services-protect-with-aes128.md#create_locator)

Makale [ayrıca, istemci uygulamasının anahtar teslim hizmetinden nasıl bir anahtar isteyebileceğini](media-services-protect-with-aes128.md#client_request)de gösterir.

Makalenin sonunda tam bir [.NET örneği](media-services-protect-with-aes128.md#example) bulabilirsiniz.

Aşağıdaki görüntüde, daha önce açıklanan iş akışı gösterilmektedir. Burada kimlik doğrulaması için belirteç kullanılmaktadır.

![AES-128 ile koruyun](./media/media-services-content-protection-overview/media-services-content-protection-with-aes.png)

Bu makalenin geri kalanı, açıklamalar, kod örnekleri ve daha önce açıklanan görevleri nasıl başarabileceğinizi gösteren konulara bağlantılar sağlar.

## <a name="current-limitations"></a>Geçerli sınırlamalar
Varlığınızın teslim ilkesini ekler veya güncelleştirirseniz, varsa mevcut bulucuyu silip yeni bir bulucu oluşturmanız gerekir.

## <a name="create-an-asset-and-upload-files-into-the-asset"></a><a id="create_asset"></a>Varlık oluşturma ve kıymete dosya yükleme
Videolarınızı yönetmek, kodlamak ve akışla aktarmak için önce içeriğinizi Media Services’a yüklemeniz gerekir. Dosyanın karşıya yüklenmesinin ardından içeriğiniz, sonraki işleme ve akışla aktarma faaliyetleri için güvenli bir şekilde bulutta depolanır. 

Daha fazla bilgi için bkz. [Media Services hesabına dosya yükleme](media-services-dotnet-upload-files.md).

## <a name="encode-the-asset-that-contains-the-file-to-the-adaptive-bitrate-mp4-set"></a><a id="encode_asset"></a>Dosyayı içeren varlığı, bit hızı uyarlamalı MP4 kümesine kodlayın
Dinamik şifreleme ile bir grup çoklu bit hızlı MP4 dosyası ya da çoklu bit hızlı Kesintisiz Akış kaynak dosyası içeren bir varlık oluşturursunuz. Ardından, bildirimde veya parça isteğinde belirtilen biçime bağlı olarak, isteğe bağlı akış sunucusu seçtiğiniz protokolde akışı almanızı sağlar. Ardından, dosyaları tek depolama biçiminde depolamanız ve ödemeyapmanız yeterlidir. Media Services, bir istemciden alınan isteklere göre uygun yanıtı oluşturur ve sunar. Daha fazla bilgi için bkz. [Dinamik paketlemeye genel bakış](media-services-dynamic-packaging-overview.md).

>[!NOTE]
>Media Services hesabınız oluşturulduğunda hesabınıza “Durdurulmuş” durumda bir varsayılan akış uç noktası eklenir. İçerik akışını başlatmak ve dinamik paketleme ile dinamik şifrelemeden yararlanmak için içerik akışı yapmak istediğiniz akış uç noktasının “Çalışıyor” durumda olması gerekir. 
>
>Ayrıca, dinamik paketleme ve dinamik şifreleme kullanmak için, kıymetiniz bir dizi uyarlanabilir bithızı MP4 veya uyarlanabilir bithızı Düzgün Akış dosyaları içermelidir.

Kodlama yönergeleri için bkz. [Media Encoder Standard kullanarak varlık kodlama](media-services-dotnet-encode-with-media-encoder-standard.md).

## <a name="create-a-content-key-and-associate-it-with-the-encoded-asset"></a><a id="create_contentkey"></a>Bir içerik anahtarı oluşturup kodlanmış varlıkla ilişkilendirme
Media Services’de, içerik anahtarı bir varlığı şifrelerken kullanmak istediğiniz anahtarı içerir.

Daha fazla bilgi için bkz. [İçerik anahtarı oluşturma](media-services-dotnet-create-contentkey.md).

## <a name="configure-the-content-keys-authorization-policy"></a><a id="configure_key_auth_policy"></a>İçerik anahtarının yetkilendirme ilkesini yapılandırma
Media Services, anahtar isteğinde bulunan kullanıcıların kimlik doğrulamasını yapmanın birden çok yöntemini destekler. İçerik anahtarı yetkilendirme ilkesini yapılandırmanız gerekir. Anahtar istemciye teslim edilmeden önce istemcinin (oyuncu) ilkeyi karşılaması gerekir. İçerik anahtarı yetkilendirme ilkesinde açık, belirteç kısıtlaması veya IP kısıtlaması olmak üzere bir veya daha fazla yetkilendirme kısıtlaması olabilir.

Daha fazla bilgi edinmek için bkz. [İçerik anahtarı yetkilendirme ilkesi yapılandırma](media-services-dotnet-configure-content-key-auth-policy.md).

## <a name="configure-an-asset-delivery-policy"></a><a id="configure_asset_delivery_policy"></a>Varlık teslim ilkesi yapılandırma
Varlığınıza ilişkin teslim ilkesini yapılandırın. Varlık teslim ilkesi yapılandırmasının içerdiklerinden bazıları şunlardır:

* Anahtar edinme URL'si. 
* Zarf şifrelemesi için kullanılacak başlatma vektörü (IV). AES-128 şifreleme ve şifre çözme için aynı IV gerektirir. 
* Varlık teslim protokolü (örneğin MPEG DASH, HLS, Kesintisiz Akış veya tümü).
* Dinamik şifreleme türü (örneğin, AES zarfı) veya dinamik şifreleme yok. 

Daha fazla bilgi için bkz. [Varlık teslim ilkesi yapılandırma](media-services-dotnet-configure-asset-delivery-policy.md).

## <a name="create-an-ondemand-streaming-locator-to-get-a-streaming-url"></a><a id="create_locator"></a>Akış URL’si almak için bir OnDemand akış bulucusu oluşturma
Kullanıcınıza Kesintisiz Akış, DASH veya HLS için akış URL’sini sağlamanız gerekir.

> [!NOTE]
> Varlığınızın teslim ilkesini ekler veya güncelleştirirseniz, varsa mevcut bulucuyu silip yeni bir bulucu oluşturmanız gerekir.
> 
> 

Varlık yayımlama ve akış URL'si oluşturma yönergeleri için bkz. [Akış URL'si oluşturma](media-services-deliver-streaming-content.md).

## <a name="get-a-test-token"></a>Test belirteci alma
Anahtar yetkilendirme ilkesi için kullanılan belirteç kısıtlamasına dayalı olarak bir test belirteci alın.

```csharp
    // Deserializes a string containing an Xml representation of a TokenRestrictionTemplate
    // back into a TokenRestrictionTemplate class instance.
    TokenRestrictionTemplate tokenTemplate = 
        TokenRestrictionTemplateSerializer.Deserialize(tokenTemplateString);

    // Generate a test token based on the data in the given TokenRestrictionTemplate.
    //The GenerateTestToken method returns the token without the word "Bearer" in front
    //so you have to add it in front of the token string. 
    string testToken = TokenRestrictionTemplateSerializer.GenerateTestToken(tokenTemplate);
    Console.WriteLine("The authorization token is:\nBearer {0}", testToken);
```

Akışınızı test etmek için [Azure Media Services Oynatıcısı](https://aka.ms/azuremediaplayer)’nı kullanabilirsiniz.

## <a name="how-can-your-client-request-a-key-from-the-key-delivery-service"></a><a id="client_request"></a>Müşteriniz anahtar teslim hizmetinden nasıl bir anahtar isteyebilir?
Önceki adımda, bir bildirim dosyasına işaret eden URL'yi inşa ettiniz. Anahtar teslim hizmetine istekte bulunmak için istemcinizin akış bildirimi dosyalarından gerekli bilgileri ayıklaması gerekir.

### <a name="manifest-files"></a>Manifest dosyaları
İstemcinin URL'yi (ayrıca içerik anahtar kimliği [kid]) değerini bildirim dosyasından ayıklamaları gerekir. İstemci daha sonra anahtar teslim hizmetinden şifreleme anahtarını almaya çalışır. İstemcinin ayrıca IV değerini ayıklamaları ve akışın şifresini çözmek için kullanması gerekir. Aşağıdaki parçacık, Düzgün Akış `<Protection>` bildiriminin öğesini gösterir:

```xml
    <Protection>
      <ProtectionHeader SystemID="B47B251A-2409-4B42-958E-08DBAE7B4EE9">
        <ContentProtection xmlns:sea="urn:mpeg:dash:schema:sea:2012" schemeIdUri="urn:mpeg:dash:sea:2012">
          <sea:SegmentEncryption schemeIdUri="urn:mpeg:dash:sea:aes128-cbc:2013"/>
          <sea:KeySystem keySystemUri="urn:mpeg:dash:sea:keysys:http:2013"/>
          <sea:CryptoPeriod IV="0xD7D7D7D7D7D7D7D7D7D7D7D7D7D7D7D7" 
                            keyUriTemplate="https://wamsbayclus001kd-hs.cloudapp.net/HlsHandler.ashx?
                                            kid=da3813af-55e6-48e7-aa9f-a4d6031f7b4d"/>
        </ContentProtection>
      </ProtectionHeader>
    </Protection>
```

HLS durumunda, kök bildirimi segment dosyalarına bölünür. 

Örneğin, kök manifestosu: http:\//test001.origin.mediaservices.windows.net/8bfe7d6f-34e3-4d1a-b289-3e48a8762490/BigBuckBunny.ism/manifest(format=m3u8-aapl). Segment dosya adlarının listesini içerir.

    . . . 
    #EXT-X-STREAM-INF:PROGRAM-ID=1,BANDWIDTH=630133,RESOLUTION=424x240,CODECS="avc1.4d4015,mp4a.40.2",AUDIO="audio"
    QualityLevels(514369)/Manifest(video,format=m3u8-aapl)
    #EXT-X-STREAM-INF:PROGRAM-ID=1,BANDWIDTH=965441,RESOLUTION=636x356,CODECS="avc1.4d401e,mp4a.40.2",AUDIO="audio"
    QualityLevels(842459)/Manifest(video,format=m3u8-aapl)
    …

Bir metin düzenleyicisinde segment dosyalarından birini açarsanız (örneğin, http:\//test001.origin.mediaservices.windows.net/8bfe7d6f-34e3-4d1a-b289-3e48a8762490/BigBuckBunny.ism/QualityLevels(514369)/Manifest(video,format=m3u8-aapl), dosyanın şifrelenmiş olduğunu gösteren #EXT-X-KEY içerir.

    #EXTM3U
    #EXT-X-VERSION:4
    #EXT-X-ALLOW-CACHE:NO
    #EXT-X-MEDIA-SEQUENCE:0
    #EXT-X-TARGETDURATION:9
    #EXT-X-KEY:METHOD=AES-128,
    URI="https://wamsbayclus001kd-hs.cloudapp.net/HlsHandler.ashx?
         kid=da3813af-55e6-48e7-aa9f-a4d6031f7b4d",
            IV=0XD7D7D7D7D7D7D7D7D7D7D7D7D7D7D7D7
    #EXT-X-PROGRAM-DATE-TIME:1970-01-01T00:00:00.000+00:00
    #EXTINF:8.425708,no-desc
    Fragments(video=0,format=m3u8-aapl)
    #EXT-X-ENDLIST

>[!NOTE] 
>Safari'de AES şifreli bir HLS oynamayı planlıyorsanız, [bu blogu](https://azure.microsoft.com/blog/how-to-make-token-authorized-aes-encrypted-hls-stream-working-in-safari/)görün.

### <a name="request-the-key-from-the-key-delivery-service"></a>Anahtarı anahtar teslim hizmetinden isteyin

Aşağıdaki kod, önemli bir teslimat Uri (manifestodan çıkarılan) ve bir belirteç kullanarak Medya Hizmetleri anahtar teslim hizmetine nasıl bir istek gönderilen gösterir. (Bu makalede, bir STS'den SWT'ler nasıl alınılmadığını açıklamaz.)

```csharp
    private byte[] GetDeliveryKey(Uri keyDeliveryUri, string token)
    {
        HttpWebRequest request = (HttpWebRequest)WebRequest.Create(keyDeliveryUri);

        request.Method = "POST";
        request.ContentType = "text/xml";
        if (!string.IsNullOrEmpty(token))
        {
            request.Headers[AuthorizationHeader] = token;
        }
        request.ContentLength = 0;

        var response = request.GetResponse();

        var stream = response.GetResponseStream();
        if (stream == null)
        {
            throw new NullReferenceException("Response stream is null");
        }

        var buffer = new byte[256];
        var length = 0;
        while (stream.CanRead && length <= buffer.Length)
        {
            var nexByte = stream.ReadByte();
            if (nexByte == -1)
            {
                break;
            }
            buffer[length] = (byte)nexByte;
            length++;
        }
        response.Close();

        // AES keys must be exactly 16 bytes (128 bits).
        var key = new byte[length];
        Array.Copy(buffer, key, length);
        return key;
    }
```

## <a name="protect-your-content-with-aes-128-by-using-net"></a>.NET kullanarak içeriğinizi AES-128 ile koruyun

### <a name="create-and-configure-a-visual-studio-project"></a>Visual Studio projesi oluşturup yapılandırma

1. Geliştirme ortamınızı ayarlayın ve [.NET ile Medya Hizmetleri geliştirmede](media-services-dotnet-how-to-use.md)açıklandığı gibi app.config dosyasını bağlantı bilgileriyle doldurun.

2. App.config dosyanızda tanımlandığı şekilde aşağıdaki öğeleri appSettings'e ekleyin:

    ```xml
    <add key="Issuer" value="http://testissuer.com"/>
    <add key="Audience" value="urn:test"/>
    ```

### <a name="example"></a><a id="example"></a>Örnek

Bu bölümde gösterilen kodu Program.cs dosyanızdaki kodun üzerine yazın.
 
>[!NOTE]
>Farklı Medya Hizmetleri ilkeleri için 1.000.000 ilke sınırı vardır (örneğin, Konum belirleme ilkesi veya ContentKeyAuthorizationPolicy için). Her zaman aynı günleri/erişim izinlerini kullanıyorsanız aynı ilke kimliğini kullanın. Örnek olarak uzun süre olduğu gibi kalması amaçlanan bulucu ilkeleri (karşıya yükleme olmayan ilkeler) verilebilir. Daha fazla bilgi için, [Medya Hizmetleri .NET SDK ile varlıkları ve ilgili kuruluşları yönet'teki](media-services-dotnet-manage-entities.md#limit-access-policies)"Erişim ilkelerini sınırla" bölümüne bakın.

Değişkenleri, giriş dosyalarınızın bulunduğu klasörlere işaret edecek şekilde güncelleştirdiğinizden emin olun.

[!code-csharp[Main](../../../samples-mediaservices-encryptionaes/DynamicEncryptionWithAES/DynamicEncryptionWithAES/Program.cs)]

## <a name="media-services-learning-paths"></a>Media Services’i öğrenme yolları
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]
