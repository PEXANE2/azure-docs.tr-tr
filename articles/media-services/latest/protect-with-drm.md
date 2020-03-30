---
title: Azure Media Services DRM şifreleme ve lisans teslim hizmeti
titleSuffix: Azure Media Services
description: Microsoft PlayReady, Google Widevine veya Apple FairPlay lisansları ile şifrelenmiş akışları sunmak için DRM dinamik şifreleme ve lisans teslim hizmetini nasıl kullanacağınızı öğrenin.
services: media-services
documentationcenter: ''
author: juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/25/2019
ms.author: juliako
ms.custom: seodec18
ms.openlocfilehash: 14ba5f270138db22a76fd697b264046e22577427
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79086741"
---
# <a name="tutorial-use-drm-dynamic-encryption-and-license-delivery-service"></a>Öğretici: DRM dinamik şifreleme ve lisans teslim hizmetini kullanın

> [!NOTE]
> Bu öğretici [.NET SDK](https://docs.microsoft.com/dotnet/api/microsoft.azure.management.media.models.liveevent?view=azure-dotnet) örneklerini kullansa da, genel adımlar [REST API](https://docs.microsoft.com/rest/api/media/liveevents), [CLI](https://docs.microsoft.com/cli/azure/ams/live-event?view=azure-cli-latest)veya diğer desteklenen [SDK'lar](media-services-apis-overview.md#sdks)için aynıdır.

Azure Media Services'ı kullanarak Microsoft PlayReady, Google Widevine veya Apple FairPlay lisansları ile şifrelenmiş akışlarınızı sunabilirsiniz. Ayrıntılı açıklama için [dinamik şifrelemeile İçerik koruması'na](content-protection-overview.md)bakın.

Medya Hizmetleri ayrıca PlayReady, Widevine ve FairPlay DRM lisansları sunmak için de bir hizmet sunmaktadır. Bir kullanıcı DRM korumalı içerik istediğinde, oynatıcı uygulama Medya Hizmetleri lisans hizmetinden lisans ister. Oynatıcı uygulaması yetkilendirilense, Medya Hizmetleri lisans hizmeti oynatıcıya bir lisans verir. Lisans, istemci oynatıcısının içeriğin şifresini çözmek ve akışını yapmak için kullanabileceği şifre çözme anahtarını içerir.

Bu makalede, [DRM ile şifreleme](https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials/blob/master/AMSV3Tutorials/EncryptWithDRM) örneği kullanılmıştır.

Bu makalede anlatılan örnek aşağıdaki sonucu üretir:

![Azure Media Player'da DRM korumalı video içeren AMS](./media/protect-with-drm/ams_player.png)

Bu öğretici şunların nasıl yapıldığını gösterir:

> [!div class="checklist"]
> * Bir kodlama Dönüşümü oluşturun.
> * Belirtecinizin doğrulanması için kullanılan imzalama anahtarını ayarlama.
> * İçerik anahtarı ilkesinin gereksinimlerini ayarlayın.
> * Belirtilen akış ilkesiyle bir StreamingLocator oluşturun.
> * Dosyanızı oynatmak için kullanılan bir URL oluşturun.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Ön koşullar

Öğreticiyi tamamlamak için aşağıdakiler gereklidir:

* [İçerik korumaya genel bakış](content-protection-overview.md) makalesini gözden geçirin.
* Erişim [denetimi ile Tasarım çoklu DRM içerik koruma sistemini gözden geçirin.](design-multi-drm-system-with-access-control.md)
* Visual Studio Code veya Visual Studio'yı yükleyin.
* [Bu hızlı başlangıçta](create-account-cli-quickstart.md) açıklandığı gibi yeni bir Azure Media Services hesabı oluşturun.
* [Erişim API'lerini](access-api-cli-how-to.md) kullanarak Media Services API'lerini kullanmak için gerekli kimlik bilgilerini edinin.
* Uygulama yapılandırma dosyasındaki (appsettings.json) uygun değerleri ayarlayın.

## <a name="download-code"></a>Kodu indirin

Aşağıdaki komutu kullanarak, bu makalede anlatılan eksiksiz .NET örneğini içeren bir GitHub havuzunu makinenize kopyalayın:

 ```bash
 git clone https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials.git
 ```
 
"Encrypt with DRM" örneği [EncryptWithDRM](https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials/blob/master/AMSV3Tutorials/EncryptWithDRM) klasöründe bulunur.

> [!NOTE]
> Örnek, uygulamayı her çalıştırdığınızda benzersiz kaynaklar oluşturur. Genellikle, dönüşümler ve ilkeler gibi varolan kaynakları yeniden kullanırsınız (varolan kaynak gerekli yapılandırmaları varsa).

## <a name="start-using-media-services-apis-with-net-sdk"></a>.NET SDK ile Media Services API’sini kullanmaya başlama

.NET ile Medya Hizmetleri API'lerini kullanmaya başlamak için bir **AzureMediaServicesClient** nesnesi oluşturun. Nesneyi oluşturmak için, Azure AD kullanarak Azure’a bağlanmak üzere istemcinin ihtiyaç duyduğu kimlik bilgilerini sağlamanız gerekir. Makalenin başlangıcında kopyaladığınız koddaki **GetCredentialsAsync** işlevi, yerel yapılandırma dosyasında sağlanan kimlik bilgilerini temel alan ServiceClientCredentials nesnesini oluşturur.

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/EncryptWithDRM/Program.cs#CreateMediaServicesClient)]

## <a name="create-an-output-asset"></a>Çıktı varlığı oluşturma  

Çıktı [Varlığı](assets-concept.md), kodlama işinizin sonucunu depolar.  

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/EncryptWithDRM/Program.cs#CreateOutputAsset)]

## <a name="get-or-create-an-encoding-transform"></a>Kodlama Dönüşümü alma veya oluşturma

Yeni bir [Dönüşüm](transforms-jobs-concept.md) örneği oluştururken çıktı olarak neyi üretmesi istediğinizi belirtmeniz gerekir. Gerekli parametre, `transformOutput` aşağıdaki kodda gösterildiği gibi bir nesnedir. Her TransformOutput bir **Ön ayar** içerir. Ön ayar, video ve/veya ses işleme işlemlerinin istenen TransformOutput nesnesini oluşturmak üzere kullanılacak adım adım yönergelerini açıklar. Bu makalede açıklanan örnek, **AdaptiveStreaming** adlı yerleşik bir Ön Ayar kullanır. Preset, giriş videosunu giriş çözünürlüğü ve bitat'a göre otomatik olarak oluşturulmuş bitrate merdivenine (bitrate çözünürlüğü çiftleri) kodlar ve her bitrate çözünürlüğü çiftine karşılık gelen H.264 video ve AAC ses içeren ISO MP4 dosyaları üretir. 

Yeni bir **Dönüşüm** oluşturmadan önce ilk olarak aşağıdaki kodda gösterildiği gibi **Get** yöntemi ile bir dönüşümün zaten var olup olmadığını denetlemeniz gerekir.  Media Services v3’te varlıklar üzerindeki **Get** yöntemleri, varlığın mevcut olmaması durumunda **null** değerini döndürür (büyük/küçük harfe duyarlı ad denetimi).

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/EncryptWithDRM/Program.cs#EnsureTransformExists)]

## <a name="submit-job"></a>İşi gönderme

Yukarıda bahsedildiği gibi **Transform** nesnesi tarif, [Job](transforms-jobs-concept.md) ise bu **Transform** nesnesini belirli bir giriş videosu veya ses içeriğine uygulamak için Media Services’e gönderilen gerçek istektir. **İş,** giriş videosunun konumu ve çıktının konumu gibi bilgileri belirtir.

Bu öğreticide, doğrudan [BIR HTTPs kaynak URL'sinden](job-input-from-http-how-to.md)yutulan bir dosyaya dayalı olarak işin girdisini oluşturuyoruz.

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/EncryptWithDRM/Program.cs#SubmitJob)]

## <a name="wait-for-the-job-to-complete"></a>İşin tamamlanmasını bekleyin

İşi tamamlamak biraz zaman alır. Bu olduğunda, size haberdar olmak istiyorum. Aşağıdaki kod örneği, **İş**’in durumu için hizmette nasıl yoklama yapılacağını gösterir. Yoklama, potansiyel gecikme nedeniyle üretim uygulamaları için önerilen en iyi uygulama değildir. Yoklama, bir hesap üzerinde gereğinden fazla kullanılırsa kısıtlanabilir. Geliştiricilerin onun yerine Event Grid kullanmalıdır. Bkz. [Olayları özel bir web uç noktasına yönlendirme](job-state-events-cli-how-to.md).

**İş** genellik şu aşamalardan geçer: **Zamanlandı**, **Kuyruğa Alındı**, **İşleniyor**, **Tamamlandı** (son aşama). İş bir hatayla karşılaştıysa, **Hata** durumunu alırsınız. İş iptal edilme sürecindeyse, iş bittiğinde **İptal** edilir ve **İptal Edilir.**

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/EncryptWithDRM/Program.cs#WaitForJobToFinish)]

## <a name="create-a-content-key-policy"></a>İçerik Anahtarı İlkesi Oluşturma

İçerik anahtarı, Varlıklarınıza güvenli bir şekilde erişilmesini sağlar. İçeriğinizi bir DRM ile şifrelerken bir [İçerik Anahtarı İlkesi](content-key-policy-concept.md) oluşturmanız gerekir. İlke, içerik anahtarının son istemcilere nasıl teslim edilir şekilde yapılandırılır. İçerik anahtarı bir Akış Bulucu ile ilişkilidir. Media Services ayrıca şifreleme anahtarlarını ve lisansları yetkili kullanıcılara teslim eden anahtar teslim hizmetini de sunar.

Belirtilen yapılandırma ile anahtarları teslim etmek için karşılanması gereken **İçerik Anahtarı İlkesi'ndeki** gereksinimleri (kısıtlamaları) ayarlamanız gerekir. Bu örnekte aşağıdaki yapılandırmaları ve gereksinimleri belirleyeceğiz:

* Yapılandırma

    [PlayReady](playready-license-template-overview.md) ve [Widevine](widevine-license-template-overview.md) lisansları, Media Services lisans teslim hizmetiyle teslim edilebilecek şekilde yapılandırılmıştır. Bu örnek uygulama FairPlay lisansını yapılandırmasa da, [FairPlay'i](fairplay-license-overview.md) yapılandırmak için kullanabileceğiniz bir yöntem içerir. FairPlay yapılandırmalarını başka bir seçenek olarak ekleyebilirsiniz.

* Kısıtlama

    Uygulama, ilke üzerinde Bir JSON Web Belirteci (JWT) belirteç türü kısıtlaması ayarlar.

Oynatıcı bir akış isteğinde bulunduğunda Media Services belirtilen anahtarı kullanarak içeriğinizi dinamik olarak şifreler. Oynatıcı, akışın şifresini çözmek için anahtar teslim hizmetinden anahtarı ister. Hizmet, kullanıcının anahtarı alma yetkisine sahip olup olmadığını belirlemek için anahtar için belirlediğiniz içerik anahtarı ilkesini değerlendirir.

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/EncryptWithDRM/Program.cs#GetOrCreateContentKeyPolicy)]

## <a name="create-a-streaming-locator"></a>Akış Lı Bulucu Oluşturma

Kodlama tamamlandıktan ve içerik anahtarı ilkesi ayarlandıktan sonra sıradaki adım, çıktı Varlığındaki videoyu yürütmek için istemcilerin kullanımına sunmaktır. Videoyu iki adımda kullanılabilir hale getirebilirsiniz:

1. Akış [Lı Bulucu](streaming-locators-concept.md)oluşturun.
2. İstemcilerin kullanabileceği akış URL'sini oluşturma.

**Akış Bulucu** oluşturma işlemine yayımlama denir. Varsayılan olarak, ApI aramaları yaptıktan hemen sonra **Akış Bulucu** geçerlidir. İsteğe bağlı başlangıç ve bitiş saatlerini yapılandırmadığınız sürece silinene kadar sürer.

**Akış Lı Bulucu**oluştururken, istediğinizi `StreamingPolicyName`belirtmeniz gerekir. Bu eğitimde, Azure Medya Hizmetleri'ne akış için içeriği nasıl yayınlayacağını anlatan önceden tanımlanmış Akış İlkelerlerinden birini kullanıyoruz. Bu örnekte StreamingLocator.StreamingPolicyName öğesini "Predefined_MultiDrmCencStreaming" ilkesi olarak belirliyoruz. PlayReady ve Widevine şifrelemeleri uygulanır ve anahtar yapılandırılmış DRM lisanslarına göre oynatma istemcisine teslim edilir. Akışınızı CBCS (FairPlay) ile de şifrelemek isterseniz "Predefined_MultiDrmStreaming" öğesini kullanın.

> [!IMPORTANT]
> Özel bir [Akış İlkesi](streaming-policy-concept.md)kullanırken, Medya Hizmeti hesabınız için sınırlı sayıda bu tür ilke ler tasarlamalı ve aynı şifreleme seçenekleri ve protokolleri gerektiğinde Bunları StreamingLocators'Larınız için yeniden kullanmalısınız. Media Service hesabınızda StreamingPolicy girişlerinin sayısı için bir kota bulunur. Her StreamingLocator için yeni bir Akış Politikası oluşturmamalısınız.

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/EncryptWithDRM/Program.cs#CreateStreamingLocator)]

## <a name="get-a-test-token"></a>Test belirteci alma

Bu öğreticide içerik anahtarı ilkesi için belirteç sınırlaması getirilmektedir. Belirteç kısıtlamalı ilkenin beraberinde bir güvenlik belirteci hizmeti (STS) tarafından verilmiş bir belirteç bulunmalıdır. Medya Hizmetleri [JWT](https://msdn.microsoft.com/library/gg185950.aspx#BKMK_3) biçimlerindeki belirteçleri destekler ve örnekte bunu yapılandırdığımız dır.

ContentKeyPolicy içinde kullanılan ContentKeyIdentifierClaim, anahtar teslim hizmetine sunulan belirtecin içinde ContentKey tanımlayıcısına sahip olması gerektiğini belirtir. Örnekte, Akış Lık Bulucu'yu oluştururken bir içerik anahtarı belirtmeyiz, sistem bizim için rastgele bir anahtar oluşturur. Test belirteci oluşturmak için ContentKeyId'in ContentKeyIdentifierClaim iddiasını koymasını sağlamalıyız.

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/EncryptWithDRM/Program.cs#GetToken)]

## <a name="build-a-streaming-url"></a>Akış URL'si oluşturma

[Artık StreamingLocator](https://docs.microsoft.com/rest/api/media/streaminglocators) oluşturulduğuna göre, akış URL'lerini alabilirsiniz. URL oluşturmak [için, StreamingEndpoint](https://docs.microsoft.com/rest/api/media/streamingendpoints) ana bilgisayar adını ve **Akış Lı Konum belirleme** yolunu oluşturmanız gerekir. Bu örnekte, *varsayılan* **Akış Bitiş Noktası** kullanılır. Bir Medya Hizmeti hesabı ilk oluşturduğunuzda, bu *varsayılan* **Akış Bitiş Noktası** durdurulmuş durumda olacaktır, bu nedenle **Başlat'ı**aramanız gerekir.

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/EncryptWithDRM/Program.cs#GetMPEGStreamingUrl)]

Uygulamayı çalıştırdığınızda aşağıdaki ekranı görürsünüz:

![DRM ile koruyun](./media/protect-with-drm/playready_encrypted_url.png)

Bir tarayıcı açın ve oluşturulan URL'yi yapıştırarak URL'nin ve belirtecin sizin için doldurulmuş olduğu Azure Media Player tanıtım sayfasını açabilirsiniz.

## <a name="clean-up-resources-in-your-media-services-account"></a>Media Services hesabınızdaki kaynakları temizleme

Genellikle, yeniden kullanmayı planladığınız nesneler dışındaki her şeyi temizlemeniz gerekir (genellikle Transforms, StreamingLocators vb.'yi yeniden kullanırsınız). Denemeden sonra hesabınızın temiz olmasını istiyorsanız, yeniden kullanmayı planlamadığınız kaynakları silin. Örneğin, aşağıdaki kod İşler'i siler:

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/EncryptWithDRM/Program.cs#CleanUp)]

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu öğreticide oluşturduğunuz Media Services ve depolama hesapları dahil olmak üzere, kaynak grubunuzdaki kaynaklardan herhangi birine artık ihtiyacınız yoksa kaynak grubunu silebilirsiniz.

Aşağıdaki CLI komutunu uygulayın:

```azurecli
az group delete --name amsResourceGroup
```

## <a name="additional-notes"></a>Ek notlar

* Widevine, Google Inc. tarafından sağlanan ve Google, Inc.'in hizmet koşullarına ve Gizlilik Politikasına tabi olan bir hizmettir.

## <a name="ask-questions-give-feedback-get-updates"></a>Soru sorun, geri bildirimde, güncellemeler alın

Soru sormanın, geri bildirimde bulunabilir ve Medya Hizmetleri hakkında güncellemeler alabildiğiniz farklı yolları görmek için [Azure Medya Hizmetleri topluluk](media-services-community.md) makalesine göz atın.

## <a name="next-steps"></a>Sonraki adımlar

Kullanıma al

> [!div class="nextstepaction"]
> [AES-128 ile koruyun](protect-with-aes128.md)
