---
title: Medya Hizmetleri v3 ile canlı yayın
titleSuffix: Azure Media Services
description: Azure Media Services v3 ile nasıl canlı yayın yapacaklarını öğrenin.
services: media-services
documentationcenter: ''
author: juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.custom: mvc
ms.date: 06/13/2019
ms.author: juliako
ms.openlocfilehash: 0b6667965ddd1fce30bb2da2593e2a9274b595ed
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "79472025"
---
# <a name="tutorial-stream-live-with-media-services"></a>Öğretici: Medya Hizmetleri ile canlı yayın

> [!NOTE]
> Öğretici [.NET SDK](https://docs.microsoft.com/dotnet/api/microsoft.azure.management.media.models.liveevent?view=azure-dotnet) örneklerini kullansa da, genel adımlar [REST API](https://docs.microsoft.com/rest/api/media/liveevents), [CLI](https://docs.microsoft.com/cli/azure/ams/live-event?view=azure-cli-latest)veya diğer desteklenen [SDK'lar](media-services-apis-overview.md#sdks)için aynıdır.

Azure Medya [Hizmetlerinde, Canlı Etkinlikler](https://docs.microsoft.com/rest/api/media/liveevents) canlı akış içeriğini işlemekle yükümlüdür. Canlı Etkinlik, daha sonra canlı bir kodlayıcıya sağladığınız bir giriş bitiş noktası (URL yutma) sağlar. Canlı Etkinlik, canlı kodlayıcıdan canlı giriş akışları alır ve bir veya daha fazla [Akış Uç Noktası](https://docs.microsoft.com/rest/api/media/streamingendpoints)üzerinden akış için kullanılabilir hale getirir. Canlı Etkinlikler ayrıca, daha fazla işleme ve teslim edilmeden önce akışınızı önizlemek ve doğrulamak için kullandığınız bir önizleme bitiş noktası (önizleme URL'si) de sağlar. Bu öğretici, **geçiş** türü bir canlı olay oluşturmak için .NET Core kullanmayı göstermektedir.

Öğretici şunların nasıl yapıldığını göstermektedir:

> [!div class="checklist"]
> * Konuyla açıklanan örnek uygulamayı indirin.
> * Canlı akış gerçekleştiren kodu inceleyin.
> * Azure Media [Player](https://amp.azure.net/libs/amp/latest/docs/index.html) ile [https://ampdemo.azureedge.net](https://ampdemo.azureedge.net)etkinliği izleyin.
> * Kaynakları temizleyin.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Ön koşullar

Öğreticiyi tamamlamak için aşağıdakiler gereklidir:

- Visual Studio Code veya Visual Studio'yı yükleyin.
- [Bir Medya Hizmetleri hesabı oluşturun.](create-account-cli-how-to.md)<br/>Kaynak grubu adı ve Medya Hizmetleri hesap adı için kullandığınız değerleri hatırladığınızdan emin olun.
- [Azure CLI ile Azure Medya Hizmetleri API'sindeki](access-api-cli-how-to.md) adımları izleyin ve kimlik bilgilerini kaydedin. API'ye erişmek için bunları kullanmanız gerekir.
- Bir olayı yayınlamak için kullanılan bir kamera veya aygıt (dizüstü bilgisayar gibi).
- Kameradaki sinyalleri Medya Hizmetleri canlı akış hizmetine gönderilen akışlara dönüştüren şirket içi canlı kodlayıcı, [önerilen şirket içi canlı kodlayıcılara](recommended-on-premises-live-encoders.md)bakın. Akışın **RTMP** veya **Kesintisiz Akış** biçiminde olması gerekir.

> [!TIP]
> Devam etmeden önce [Media Services v3 ile canlı akış](live-streaming-overview.md) konusunu gözden geçirmeyi unutmayın. 

## <a name="download-and-configure-the-sample"></a>Örneği indirin ve yapılandırın

Aşağıdaki komutu kullanarak, akış .NET örneğini içeren bir GitHub havuzunu makinenize kopyalayın:  

 ```bash
 git clone https://github.com/Azure-Samples/media-services-v3-dotnet-core-tutorials.git
 ```

Canlı akış örneği [Live](https://github.com/Azure-Samples/media-services-v3-dotnet-core-tutorials/tree/master/NETCore/Live/MediaV3LiveApp) klasöründe bulunabilir.

İndirilen projede [appsettings.json'ı](https://github.com/Azure-Samples/media-services-v3-dotnet-core-tutorials/blob/master/NETCore/Live/MediaV3LiveApp/appsettings.json) açın. Değerleri [API'lara erişirerek](access-api-cli-how-to.md)aldığınız kimlik bilgileriyle değiştirin.

> [!IMPORTANT]
> Bu örnek, her kaynak için benzersiz bir sonek kullanır. Hata ayıklamayı iptal ederseniz veya uygulamayı çalıştırmadan sonlandırırsanız, hesabınızda birden fazla Canlı Etkinlik olur. <br/>Canlı Etkinlikler'i çalıştırmayı bıraktığından emin olun. Aksi takdirde, **faturalandırılırsınız!**

## <a name="examine-the-code-that-performs-live-streaming"></a>Canlı akışı gerçekleştiren kodu inceleme

Bu bölümde, *MediaV3LiveApp* projesinin [Program.cs](https://github.com/Azure-Samples/media-services-v3-dotnet-core-tutorials/blob/master/NETCore/Live/MediaV3LiveApp/Program.cs) dosyasında tanımlı işlevler incelenmektedir.

Örnek, temizlemeden örneği birden çok kez çalıştırıyorsanız ad çakışmalarının olmadığı için her kaynak için benzersiz bir sonek oluşturur.

> [!IMPORTANT]
> Bu örnek, her kaynak için benzersiz bir sonek kullanır. Hata ayıklamayı iptal ederseniz veya uygulamayı çalıştırmadan sonlandırırsanız, hesabınızda birden fazla Canlı Etkinlik olur. <br/>
> Canlı Etkinlikler'i çalıştırmayı bıraktığından emin olun. Aksi takdirde, **faturalandırılırsınız!**

### <a name="start-using-media-services-apis-with-net-sdk"></a>.NET SDK ile Media Services API’sini kullanmaya başlama

.NET ile Media Services API’lerini kullanmaya başlamak için bir **AzureMediaServicesClient** nesnesi oluşturmanız gerekir. Nesneyi oluşturmak için, Azure AD kullanarak Azure’a bağlanmak üzere istemcinin ihtiyaç duyduğu kimlik bilgilerini sağlamanız gerekir. Makalenin başlangıcında kopyaladığınız kodda, **GetCredentialsAsync** işlevi, yerel yapılandırma dosyasında sağlanan kimlik bilgilerini temel alarak ServiceClientCredentials nesnesi oluşturur. 

[!code-csharp[Main](../../../media-services-v3-dotnet-core-tutorials/NETCore/Live/MediaV3LiveApp/Program.cs#CreateMediaServicesClient)]

### <a name="create-a-live-event"></a>Canlı etkinlik oluşturma

Bu bölümde, Canlı Olay (LiveEventEncodingType None olarak ayarlanmış) **geçiş** türü nasıl oluşturulacağını gösterir. Kullanılabilir Canlı Etkinlik türleri hakkında daha fazla bilgi için [Canlı Etkinlik türlerine](live-events-outputs-concept.md#live-event-types)bakın. 
 
Canlı olay oluştururken belirtmek isteyebileceğin bazı şeyler şunlardır:

* Medya Hizmetleri konumu.
* Canlı Etkinlik için akış protokolü (şu anda RTMP ve Düzgün Akış protokolleri desteklenir).<br/>Canlı Etkinlik veya ilişkili Canlı Çıktılar çalışırken protokol seçeneğini değiştiremezsiniz. Farklı protokoller gerektiriyorsanız, her akış protokolü için ayrı Bir Canlı Olay oluşturun.  
* Alma ve önizleme için IP kısıtlamaları. Bu Canlı Etkinlik'e video yutmasına izin verilen IP adreslerini tanımlayabilirsiniz. İzin verilen IP adresleri tek bir IP adresi (örneğin '10.0.0.1'), bir IP adresi ve CIDR alt ağ maskesi kullanan bir IP aralığı (örneğin '10.0.0.1/22') veya bir IP adresi ve bir noktalı ondalık alt ağ maskesi kullanan bir IP aralığı (örneğin '10.0.0.1(255.255.252.0)') olabilir.<br/>IP adresi belirtilmemişse ve kural tanımı yoksa, IP adresine izin verilmez. Tüm IP adreslerine izin vermek için, bir kural oluşturun ve 0.0.0.0/0 olarak ayarlayın.<br/>IP adresleri aşağıdaki biçimlerden birinde olmalıdır: Dört numaralı IpV4 adresi veya CIDR adres aralığı.
* Olayı oluştururken, otomatik başlatmayı belirtebilirsiniz. <br/>Otomatik başlatma doğru ayarlandığında, Live Event oluşturulduktan sonra başlatılır. Bu, Faturalandırmanın Canlı Etkinlik çalışmaya başlar başlamaz başladığı anlamına gelir. Daha fazla faturalandırmayı durdurmak için Canlı Etkinlik kaynağında Durdur'u açıkça aramalısınız. Daha fazla bilgi için [Canlı Etkinlik durumları ve faturalandırma konusuna](live-event-states-billing.md)bakın.
* Bir url'nin tahmine uygun olması için "makyaj" modunu ayarlayın. Ayrıntılı bilgi için Canlı [Etkinlik URL'leri 'ne](live-events-outputs-concept.md#live-event-ingest-urls)bakın.

[!code-csharp[Main](../../../media-services-v3-dotnet-core-tutorials/NETCore/Live/MediaV3LiveApp/Program.cs#CreateLiveEvent)]

### <a name="get-ingest-urls"></a>Alma URL’leri alma

Canlı Etkinlik oluşturulduktan sonra, canlı kodlayıcıya sağlayacağınız URL'leri yutabilirsiniz. Kodlayıcı bu URL'leri canlı akış girişi için kullanır.

[!code-csharp[Main](../../../media-services-v3-dotnet-core-tutorials/NETCore/Live/MediaV3LiveApp/Program.cs#GetIngestURL)]

### <a name="get-the-preview-url"></a>Önizleme URL'sini alma

Kodlayıcıdan gelen girişin gerçekten alındığını önizlemek ve doğrulamak için PreviewEndpoint kullanın.

> [!IMPORTANT]
> Videonun devam etmeden önce Önizleme URL'sine aktığından emin olun.

[!code-csharp[Main](../../../media-services-v3-dotnet-core-tutorials/NETCore/Live/MediaV3LiveApp/Program.cs#GetPreviewURLs)]

### <a name="create-and-manage-live-events-and-live-outputs"></a>Canlı Etkinlikler ve Canlı Çıktılar Oluşturma ve Yönetme

Akışı Live Event'e akttındıktan sonra, bir Varlık, Canlı Çıktı ve Akış Bulucu'su oluşturarak akış etkinliğine başlayabilirsiniz. Bu olay, akışı arşivler ve akışın Akış Uç Noktası aracılığıyla izleyiciler tarafından kullanılabilmesini sağlar.

#### <a name="create-an-asset"></a>Asset oluşturma

Kullanılacak Canlı Çıktı için bir Varlık Oluşturun.

[!code-csharp[Main](../../../media-services-v3-dotnet-core-tutorials/NETCore/Live/MediaV3LiveApp/Program.cs#CreateAsset)]

#### <a name="create-a-live-output"></a>Canlı Çıktı Oluşturma

Canlı Çıktılar oluşturmayla başlar ve silindiğinde durur. Canlı Çıktıyı sildiğinizde, varlıktaki temel Varlığı ve içeriği silmezsiniz.

[!code-csharp[Main](../../../media-services-v3-dotnet-core-tutorials/NETCore/Live/MediaV3LiveApp/Program.cs#CreateLiveOutput)]

#### <a name="create-a-streaming-locator"></a>Akış Lı Bulucu Oluşturma

> [!NOTE]
> Medya Hizmetleri hesabınız oluşturulduğunda, **Durduruldu** durumunda hesabınıza **varsayılan** bir akış bitiş noktası eklenir. İçeriğinizi akışa başlamak ve [dinamik paketleme](dynamic-packaging-overview.md) ve dinamik şifrelemeden yararlanmak için, içeriği aktarmak istediğiniz akış bitiş **noktasının Çalışan** durumunda olması gerekir.

Akış Lı Konumlat'ı kullanarak Canlı Çıkış varlığını yayımladığınızda, Akış Lı Kuzak'ın süresi sona erene veya silinene kadar (hangisi önce gerçekleşirse) Canlı Olay (DVR pencere uzunluğuna kadar) görüntülenebilir olmaya devam eder.

[!code-csharp[Main](../../../media-services-v3-dotnet-core-tutorials/NETCore/Live/MediaV3LiveApp/Program.cs#CreateStreamingLocator)]

```csharp

// Get the url to stream the output
ListPathsResponse paths = await client.StreamingLocators.ListPathsAsync(resourceGroupName, accountName, locatorName);

foreach (StreamingPath path in paths.StreamingPaths)
{
    UriBuilder uriBuilder = new UriBuilder();
    uriBuilder.Scheme = "https";
    uriBuilder.Host = streamingEndpoint.HostName;

    uriBuilder.Path = path.Paths[0];
    // Get the URL from the uriBuilder: uriBuilder.ToString()
}
```

### <a name="cleaning-up-resources-in-your-media-services-account"></a>Media Services hesabınızdaki kaynakları temizleme

Olayları akışı bittiyseniz ve daha önce sağlanan kaynakları temizlemek istiyorsanız, aşağıdaki yordamı izleyin:

* Kodlayıcıdan akışı göndermeyi durdurun.
* Canlı Etkinliği Durdurun. Canlı Etkinlik durdurulduktan sonra herhangi bir ücrete tabi olmaz. Tekrar başlatmanız gerektiğinde, aynı alma URL’sine sahip olacağından kodlayıcıyı yeniden yapılandırmanız gerekmez.
* Canlı olayınızın arşivini isteğe bağlı bir akış olarak sunmaya devam etmek istemiyorsanız Akış Uç Noktanızı durdurabilirsiniz. Canlı Etkinlik durdurulmuş durumdaysa, herhangi bir ücrete tabi olmaz.

[!code-csharp[Main](../../../media-services-v3-dotnet-core-tutorials/NETCore/Live/MediaV3LiveApp/Program.cs#CleanupLiveEventAndOutput)]

[!code-csharp[Main](../../../media-services-v3-dotnet-core-tutorials/NETCore/Live/MediaV3LiveApp/Program.cs#CleanupLocatorAssetAndStreamingEndpoint)]

## <a name="watch-the-event"></a>Olayı izleme

Olayı izlemek için, Akış Bulucu Oluştur'da açıklanan kodu çalıştırdığınızda aldığınız akış URL'sini kopyalayın. Seçtiğiniz bir medya oynatıcık kullanabilirsiniz. [Azure Media Player](https://amp.azure.net/libs/amp/latest/docs/index.html) akışınızı 'da https://ampdemo.azureedge.nettest etmek için kullanılabilir.

Live Event durdurulduğunda olayları otomatik olarak isteğe bağlı içeriğe dönüştürür. Etkinliği durdurup sildikten sonra bile, kullanıcılar arşivlenmiş içeriğinizi, varlığı silmediğiniz sürece isteğe bağlı olarak video olarak aktarabilirler. Bir varlık bir olay tarafından kullanılıyorsa silinemez; olay önce silinmelidir.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu öğreticide oluşturduğunuz Media Services ve depolama hesapları dahil olmak üzere, kaynak grubunuzdaki kaynaklardan herhangi birine artık ihtiyacınız yoksa kaynak grubunu silebilirsiniz.

Aşağıdaki CLI komutunu uygulayın:

```azurecli-interactive
az group delete --name amsResourceGroup
```

> [!IMPORTANT]
> Canlı Etkinliği çalıştırmayı bırakmak faturalandırma maliyetlerine neden oluyor. Proje/program çökerse veya herhangi bir nedenle kapatılırsa, Canlı Etkinlik'in faturalandırma durumunda çalışmasını bırakabilir.

## <a name="ask-questions-give-feedback-get-updates"></a>Soru sorun, geri bildirimde, güncellemeler alın

Soru sormanın, geri bildirimde bulunabilir ve Medya Hizmetleri hakkında güncellemeler alabildiğiniz farklı yolları görmek için [Azure Medya Hizmetleri topluluk](media-services-community.md) makalesine göz atın.

## <a name="next-steps"></a>Sonraki adımlar

[Dosyaları akışla aktarma](stream-files-tutorial-with-api.md)
 
