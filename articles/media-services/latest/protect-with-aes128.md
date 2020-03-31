---
title: AES-128 ile videoyu şifreleme
titleSuffix: Azure Media Services
description: AES 128 bit şifrelemeyle videoyu nasıl şifreleyebilirsiniz ve Azure Medya Hizmetleri'nde anahtar teslim hizmetini nasıl kullanacağınızı öğrenin.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/21/2019
ms.author: juliako
ms.openlocfilehash: 126700e6290650221a9cb9711b22472301409fca
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74974181"
---
# <a name="tutorial-encrypt-video-with-aes-128-and-use-the-key-delivery-service"></a>Öğretici: AES-128 ile video şifrelemek ve anahtar teslim hizmeti kullanın

> [!NOTE]
> Öğretici [.NET SDK](https://docs.microsoft.com/dotnet/api/microsoft.azure.management.media.models.liveevent?view=azure-dotnet) örneklerini kullansa da, genel adımlar [REST API](https://docs.microsoft.com/rest/api/media/liveevents), [CLI](https://docs.microsoft.com/cli/azure/ams/live-event?view=azure-cli-latest)veya diğer desteklenen [SDK'lar](media-services-apis-overview.md#sdks)için aynıdır.

128 bit şifreleme anahtarlarını kullanarak AES ile şifrelenmiş HTTP Live Streaming (HLS), MPEG-DASH ve Smooth Streaming'i sunmak için Medya Hizmetlerini kullanabilirsiniz. Medya Hizmetleri, yetkili kullanıcılara şifreleme anahtarları sağlayan anahtar teslim hizmetini de sağlar. Medya Hizmetleri'nin videonuzu dinamik olarak şifrelemesini istiyorsanız, şifreleme anahtarını bir Akış Lı Bulucu ile ilişkilendirir ve içerik anahtarı ilkesini yapılandırabilirsiniz. Bir akış bir oynatıcı tarafından istendiğinde, Medya Hizmetleri belirtilen anahtarı kullanır ve içeriğinizi AES-128 ile dinamik olarak şifreler. Oynatıcı, akışın şifresini çözmek için anahtar teslim hizmetinden anahtarı ister. Hizmet, kullanıcının anahtarı alma yetkisine sahip olup olmadığını belirlemek için anahtar için belirlediğiniz içerik anahtarı ilkesini değerlendirir.

Her bir varlığı birden fazla şifreleme türü (AES-128, PlayReady, Widevine, FairPlay) ile şifreleyebilirsiniz. Birlikte kullanılabilecek türler hakkında bilgi almak için bkz. [Akış protokolleri ve şifreleme türleri](content-protection-overview.md#streaming-protocols-and-encryption-types). Ayrıca, [DRM ile nasıl korunacak larına](protect-with-drm.md)bakın.

Bu makalede örnekten elde edilen çıktı, Azure Media Player'ın URL'sini, manifesto URL'sini ve içeriği oynatmak için gereken AES belirteci içerir. Örnek, JSON Web Belirteci (JWT) belirteci nin sona ermesini 1 saate ayarlar. Bir tarayıcı açabilir ve ortaya çıkan URL'yi url ve sizin için doldurulmuş url ile azure media player ```https://ampdemo.azureedge.net/?url= {dash Manifest URL} &aes=true&aestoken=Bearer%3D{ JWT Token here}```demo sayfasını başlatmak için aşağıdaki biçimde yapıştırabilirsiniz: .

Bu öğretici şunların nasıl yapıldığını gösterir:

> [!div class="checklist"]
> * Makalede açıklanan [EncryptWithAES](https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials/blob/master/AMSV3Tutorials/EncryptWithAES) örneğini indirin.
> * .NET SDK ile Medya Hizmetleri API'lerini kullanmaya başlayın.
> * Bir çıktı kıymeti oluşturun.
> * Bir kodlama Dönüşümü oluşturun.
> * İş Gönder.
> * İş'in tamamlanmasını bekleyin.
> * İçerik Anahtarı İlkesi Oluşturma
> * JWT belirteç kısıtlamasını kullanacak şekilde ilkeyi yapılandırın.
> * Akış Lı Bulucu oluşturun.
> * Videoyu AES (ClearKey) ile şifrelemek için Akış Bulucu'yu yapılandırın.
> * Bir test jetonu al.
> * Akış URL'si oluşturun.
> * Kaynakları temizleyin.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Ön koşullar

Öğreticiyi tamamlamak için aşağıdakiler gereklidir.

* [İçerik korumaya genel bakış](content-protection-overview.md) makalesini gözden geçirin.
* Visual Studio Code veya Visual Studio'yı yükleyin.
* [Bir Medya Hizmetleri hesabı oluşturun.](create-account-cli-quickstart.md)
* [Access API'lerini](access-api-cli-how-to.md)izleyerek Medya Hizmetleri API'lerini kullanmak için gereken kimlik bilgilerini alın.

## <a name="download-code"></a>Kodu indirin

Aşağıdaki komutu kullanarak, bu makalede anlatılan eksiksiz .NET örneğini içeren bir GitHub havuzunu makinenize kopyalayın:

 ```bash
 git clone https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials.git
 ```

"AES-128 ile Şifreleme" örneği [EncryptWithAES](https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials/blob/master/AMSV3Tutorials/EncryptWithAES) klasöründe yer alır.

> [!NOTE]
> Örnek, uygulamayı her çalıştırdığınızda benzersiz kaynaklar oluşturur. Genellikle, dönüşümler ve ilkeler gibi varolan kaynakları yeniden kullanırsınız (varolan kaynak gerekli yapılandırmaları varsa).

## <a name="start-using-media-services-apis-with-net-sdk"></a>.NET SDK ile Media Services API’sini kullanmaya başlama

.NET ile Medya Hizmetleri API'lerini kullanmaya başlamak için bir **AzureMediaServicesClient** nesnesi oluşturun. Nesneyi oluşturmak için, Azure AD kullanarak Azure’a bağlanmak üzere istemcinin ihtiyaç duyduğu kimlik bilgilerini sağlamanız gerekir. Makalenin başlangıcında kopyaladığınız koddaki **GetCredentialsAsync** işlevi, yerel yapılandırma dosyasında sağlanan kimlik bilgilerini temel alan ServiceClientCredentials nesnesini oluşturur.

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/EncryptWithAES/Program.cs#CreateMediaServicesClient)]

## <a name="create-an-output-asset"></a>Çıktı varlığı oluşturma  

Çıktı [Varlığı](https://docs.microsoft.com/rest/api/media/assets), kodlama işinizin sonucunu depolar.  

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/EncryptWithAES/Program.cs#CreateOutputAsset)]

## <a name="get-or-create-an-encoding-transform"></a>Kodlama Dönüşümü alma veya oluşturma

Yeni bir [Dönüşüm](https://docs.microsoft.com/rest/api/media/transforms) örneği oluştururken çıktı olarak neyi üretmesi istediğinizi belirtmeniz gerekir. Gerekli parametre, aşağıdaki kodda gösterildiği gibi bir **TransformOutput** nesnesidir. Her **TransformOutput** bir **Ön ayar** içerir. **Ön ayar**, video ve/veya ses işleme işlemlerinin istenen **TransformOutput** nesnesini oluşturmak üzere kullanılacak adım adım yönergelerini açıklar. Bu makalede açıklanan örnek, **AdaptiveStreaming** adlı yerleşik bir Ön Ayar kullanır. Preset, giriş videosunu giriş çözünürlüğü ve bitat'a göre otomatik olarak oluşturulmuş bitrate merdivenine (bitrate çözünürlüğü çiftleri) kodlar ve ardından her bitrate çözünürlüğü çiftine karşılık gelen H.264 video ve AAC ses içeren ISO MP4 dosyaları üretir.

Yeni bir [Dönüştür](https://docs.microsoft.com/rest/api/media/transforms)oluşturmadan önce, aşağıdaki kodda gösterildiği **gibi,** get yöntemini kullanarak bir tanesinin zaten var olup olmadığını denetleyin. Media Services v3’te varlıklar üzerindeki **Get** yöntemleri, varlığın mevcut olmaması durumunda **null** değerini döndürür (büyük/küçük harfe duyarlı ad denetimi).

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/EncryptWithAES/Program.cs#EnsureTransformExists)]

## <a name="submit-job"></a>İşi gönderme

Yukarıda bahsedildiği gibi [Transform](https://docs.microsoft.com/rest/api/media/transforms) nesnesi tarif, [Job](https://docs.microsoft.com/rest/api/media/jobs) ise bu **Transform** nesnesini belirli bir giriş videosu veya ses içeriğine uygulamak için Media Services’e gönderilen gerçek istektir. **İş,** giriş videosunun konumu ve çıktının konumu gibi bilgileri belirtir.

Bu öğreticide, doğrudan [BIR HTTPs kaynak URL'sinden](job-input-from-http-how-to.md)yutulan bir dosyaya dayalı olarak işin girdisini oluşturuyoruz.

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/EncryptWithAES/Program.cs#SubmitJob)]

## <a name="wait-for-the-job-to-complete"></a>İşin tamamlanmasını bekleyin

İşi tamamlamak biraz zaman alır. Bu olduğunda, size haberdar olmak istiyorum. Aşağıdaki kod örneği, [İş](https://docs.microsoft.com/rest/api/media/jobs)’in durumu için hizmette nasıl yoklama yapılacağını gösterir. Yoklama, potansiyel gecikme nedeniyle üretim uygulamaları için önerilen en iyi uygulama değildir. Yoklama, bir hesap üzerinde gereğinden fazla kullanılırsa kısıtlanabilir. Geliştiricilerin onun yerine Event Grid kullanmalıdır. Daha fazla bilgi için, [olayları özel bir web bitiş noktasına yönlendir'](job-state-events-cli-how-to.md)e bakın.

**İş** genellik şu aşamalardan geçer: **Zamanlandı**, **Kuyruğa Alındı**, **İşleniyor**, **Tamamlandı** (son aşama). İş bir hatayla karşılaştıysa, **Hata** durumunu alırsınız. İş iptal edilme sürecindeyse, iş bittiğinde **İptal** edilir ve **İptal Edilir.**

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/EncryptWithAES/Program.cs#WaitForJobToFinish)]

## <a name="create-a-content-key-policy"></a>İçerik Anahtarı İlkesi Oluşturma

İçerik anahtarı, Varlıklarınıza güvenli bir şekilde erişilmesini sağlar. İçerik anahtarının son istemcilere nasıl teslim edilir şekilde yapılandıran bir **İçerik Anahtarı İlkesi** oluşturmanız gerekir. İçerik anahtarı **Akış Bulucu**ile ilişkilidir. Medya Hizmetleri, yetkili kullanıcılara şifreleme anahtarları sağlayan anahtar teslim hizmetini de sağlar.

Bir akış bir oynatıcı tarafından istendiğinde, Medya Hizmetleri içeriğinizi dinamik olarak şifrelemek için belirtilen anahtarı kullanır (bu durumda, AES şifrelemesi kullanarak.) Akışın şifresini çözmek için, oynatıcı anahtarı anahtar teslim hizmetinden ister. Hizmet, kullanıcının anahtarı alma yetkisine sahip olup olmadığını belirlemek için anahtar için belirlediğiniz içerik anahtarı ilkesini değerlendirir.

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/EncryptWithAES/Program.cs#GetOrCreateContentKeyPolicy)]

## <a name="create-a-streaming-locator"></a>Akış Lı Bulucu Oluşturma

Kodlama tamamlandıktan ve içerik anahtarı ilkesi ayarlandıktan sonra sıradaki adım, çıktı Varlığındaki videoyu yürütmek için istemcilerin kullanımına sunmaktır. Videoyu iki adımda kullanılabilir hale getirebilirsiniz:

1. Akış [Lı Bulucu](https://docs.microsoft.com/rest/api/media/streaminglocators)oluşturun.
2. İstemcilerin kullanabileceği akış URL'sini oluşturma.

**Akış Bulucu** oluşturma işlemine yayımlama denir. Varsayılan olarak, ApI aramaları yaptıktan hemen sonra **Akış Bulucu** geçerlidir. İsteğe bağlı başlangıç ve bitiş saatlerini yapılandırmadığınız sürece silinene kadar sürer.

[Akış Lı Kodlayıcı](https://docs.microsoft.com/rest/api/media/streaminglocators)oluştururken, istediğiniz **StreamingPolicyName'i**belirtmeniz gerekir. Bu eğitimde, Azure Medya Hizmetleri'ne akış için içeriği nasıl yayınlayacağını anlatan Önceden Tanımlanmış Akış İlkelerlerinden birini kullanıyoruz. Bu örnekte, AES Zarf şifrelemesi uygulanır (anahtar DRM lisansı değil, HTTPS aracılığıyla oynatma istemcisine teslim olduğundan bu şifreleme ClearKey şifrelemesi olarak da bilinir).

> [!IMPORTANT]
> Özel bir [StreamingPolicy](https://docs.microsoft.com/rest/api/media/streamingpolicies)kullanırken, Medya Hizmeti hesabınız için sınırlı sayıda bu tür ilke ler tasarlamalı ve aynı şifreleme seçenekleri ve protokolleri gerektiğinde Akış lı Konum belirleyicileriniz için bunları yeniden kullanmalısınız. Media Service hesabınızda StreamingPolicy girişlerinin sayısı için bir kota bulunur. Her Akış Bulucu için yeni bir Akış Politikası oluşturmamalısınız.

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/EncryptWithAES/Program.cs#CreateStreamingLocator)]

## <a name="get-a-test-token"></a>Test belirteci alma

Bu öğreticide içerik anahtarı ilkesi için belirteç sınırlaması getirilmektedir. Belirteç kısıtlamalı ilkenin beraberinde bir güvenlik belirteci hizmeti (STS) tarafından verilmiş bir belirteç bulunmalıdır. Medya Hizmetleri [JWT](https://msdn.microsoft.com/library/gg185950.aspx#BKMK_3) biçimindeki belirteçleri destekler ve örnekte bunu yapılandırdığımız dır.

ContentKeyIdentifierClaim İçerik Anahtarı **İlkesi'nde**kullanılır, bu da Anahtar Teslim hizmetine sunulan belirteciiçinde içerik anahtarının tanımlayıcısına sahip olması gerektiği anlamına gelir. Örnekte, Akış Lık Bulucu'yu oluştururken bir içerik anahtarı belirtmedik, sistem bizim için rastgele bir anahtar oluşturdu. Test belirteci oluşturmak için ContentKeyId'in ContentKeyIdentifierClaim iddiasını koymasını sağlamalıyız.

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/EncryptWithAES/Program.cs#GetToken)]

## <a name="build-a-dash-streaming-url"></a>DASH akış URL'si oluşturma

[Artık Akış Bulucu](https://docs.microsoft.com/rest/api/media/streaminglocators) oluşturulduğuna göre, akış URL'lerini alabilirsiniz. URL oluşturmak [için, StreamingEndpoint](https://docs.microsoft.com/rest/api/media/streamingendpoints) ana bilgisayar adını ve **Akış Lı Konum belirleme** yolunu oluşturmanız gerekir. Bu örnekte, *varsayılan* **Akış Bitiş Noktası** kullanılır. Bir Medya Hizmeti hesabı ilk oluşturduğunuzda, bu *varsayılan* **Akış Bitiş Noktası** durdurulmuş durumda olacaktır, bu nedenle **Başlat'ı**aramanız gerekir.

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/EncryptWithAES/Program.cs#GetMPEGStreamingUrl)]

## <a name="clean-up-resources-in-your-media-services-account"></a>Media Services hesabınızdaki kaynakları temizleme

Genellikle, yeniden kullanmayı planladığınız nesneler dışındaki her şeyi temizlemeniz gerekir (genellikle Transforms, Streaming Locators vb.'yi yeniden kullanırsınız). Denemeden sonra hesabınızın temiz olmasını istiyorsanız, yeniden kullanmayı planlamadığınız kaynakları silin. Örneğin, aşağıdaki kod İşler'i siler:

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/EncryptWithAES/Program.cs#CleanUp)]

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu öğreticide oluşturduğunuz Media Services ve depolama hesapları dahil olmak üzere, kaynak grubunuzdaki kaynaklardan herhangi birine artık ihtiyacınız yoksa kaynak grubunu silebilirsiniz.

Aşağıdaki CLI komutunu uygulayın:

```azurecli
az group delete --name amsResourceGroup
```

## <a name="ask-questions-give-feedback-get-updates"></a>Soru sorun, geri bildirimde, güncellemeler alın

Soru sormanın, geri bildirimde bulunabilir ve Medya Hizmetleri hakkında güncellemeler alabildiğiniz farklı yolları görmek için [Azure Medya Hizmetleri topluluk](media-services-community.md) makalesine göz atın.

## <a name="additional-notes"></a>Ek notlar

* Widevine, Google Inc. tarafından sağlanan ve Google, Inc.'in hizmet koşullarına ve Gizlilik Politikasına tabi olan bir hizmettir.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [DRM ile koruyun](protect-with-drm.md)
