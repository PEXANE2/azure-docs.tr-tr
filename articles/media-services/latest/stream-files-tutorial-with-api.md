---
title: Medya Hizmetleri v3 ile yükleme, kodlama ve akış
titleSuffix: Azure Media Services
description: Azure Media Services v3 ile dosya yüklemeyi, videoyu kodlamayı ve içeriği nasıl aktarabileceğinizi gösteren öğretici.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: tutorial
ms.custom: mvc
ms.date: 03/22/2019
ms.author: juliako
ms.openlocfilehash: 4e40d26e392219fb751328bc54855d87e80bae19
ms.sourcegitcommit: 8a9c54c82ab8f922be54fb2fcfd880815f25de77
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80346005"
---
# <a name="tutorial-upload-encode-and-stream-videos-with-media-services-v3"></a>Öğretici: Medya Hizmetleri v3 ile video yükleme, kodlama ve akış

> [!NOTE]
> Bu öğretici [.NET SDK](https://docs.microsoft.com/dotnet/api/microsoft.azure.management.media.models.liveevent?view=azure-dotnet) örneklerini kullansa da, genel adımlar [REST API](https://docs.microsoft.com/rest/api/media/liveevents), [CLI](https://docs.microsoft.com/cli/azure/ams/live-event?view=azure-cli-latest)veya diğer desteklenen [SDK'lar](media-services-apis-overview.md#sdks)için aynıdır.

Azure Medya Hizmetleri, medya dosyalarınızı çok çeşitli tarayıcılarda ve cihazlarda oynatılır biçimlere kodlamanızı sağlar. Örneğin, içeriğinizi Apple'ın HLS veya MPEG DASH biçimlerinde akışla göndermek isteyebilirsiniz. Akışla göndermeden önce yüksek kaliteli dijital medya dosyanızı kodlamanız gerekir. Kodlama ile ilgili yardım için [bkz.](encoding-concept.md) Bu öğretici yerel video dosyasını karşıya yükler ve karşıya yüklenen dosyayı kodlar. Ayrıca, https URL üzerinden erişilebilir hale getirebileceğiniz içerikleri de kodlayabilirsiniz. Daha fazla bilgi için bkz. [HTTP(s) URL'sinde iş girişi oluşturma](job-input-from-http-how-to.md).

![Azure Media Player ile video oynatma](./media/stream-files-tutorial-with-api/final-video.png)

Bu öğretici şunların nasıl yapıldığını gösterir:

> [!div class="checklist"]
> * Konuyla açıklanan örnek uygulamayı indirin.
> * Yüklenen, kodlayan ve akış yapan kodu inceleyin.
> * Uygulamayı çalıştırın.
> * Akış URL'sini test edin.
> * Kaynakları temizleyin.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Ön koşullar

- Visual Studio yüklü değilseniz Visual [Studio Community 2017'yi](https://www.visualstudio.com/thank-you-downloading-visual-studio/?sku=Community&rel=15)alabilirsiniz.
- [Bir Medya Hizmetleri hesabı oluşturun.](create-account-cli-how-to.md)<br/>Kaynak grubu adı ve Medya Hizmetleri hesap adı için kullandığınız değerleri hatırladığınızdan emin olun.
- [Azure CLI ile Azure Medya Hizmetleri API'sindeki](access-api-cli-how-to.md) adımları izleyin ve kimlik bilgilerini kaydedin. API'ye erişmek için bunları kullanmanız gerekir.

## <a name="download-and-set-up-the-sample"></a>Örneği indirin ve ayarlayın

Akış .NET örneğini aşağıdaki komutu kullanarak makinenize içeren bir GitHub deposunu klonla:  

 ```bash
 git clone https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials.git
 ```

Örnek, [UploadEncodeAndStreamFiles](https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials/tree/master/AMSV3Tutorials/UploadEncodeAndStreamFiles) klasöründe yer alır.

İndirilen projede [appsettings.json'ı](https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials/blob/master/AMSV3Tutorials/UploadEncodeAndStreamFiles/appsettings.json) açın. Değerleri [API'lara erişen](access-api-cli-how-to.md)kimlik bilgileriyle değiştirin.

## <a name="examine-the-code-that-uploads-encodes-and-streams"></a>Karşıya yüklenen, kodlanan ve akışı yapılan kodu inceleme

Bu bölümde, *UploadEncodeAndStreamFiles* projesinin [Program.cs](https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials/blob/master/AMSV3Tutorials/UploadEncodeAndStreamFiles/Program.cs) dosyasında tanımlı işlevler incelenmektedir.

Örnek aşağıdaki eylemleri gerçekleştirir:

1. Yeni bir **Dönüşüm** oluşturur (ilk olarak, belirtilen Dönüşüm var olup olmadığını denetler).
2. Kodlama **İşi**çıktısı olarak kullanılan bir çıktı **Kıymeti** oluşturur.
3. Bir giriş **Varlığı** oluşturun ve belirtilen yerel video dosyasını bu dosyaya yükler. Varlık, işin girişi olarak kullanılır.
4. Oluşturulan giriş ve çıktıyı kullanarak kodlama işini gönderir.
5. İşin durumunu denetler.
6. **Akış Lı Yer Bulucu**oluşturur.
7. Akış URL'leri oluşturur.

### <a name="start-using-media-services-apis-with-net-sdk"></a><a id="start_using_dotnet" />.NET SDK ile Media Services API’sini kullanmaya başlama

.NET ile Media Services API’lerini kullanmaya başlamak için bir **AzureMediaServicesClient** nesnesi oluşturmanız gerekir. Nesneyi oluşturmak için, istemcinin Azure AD'yi kullanarak Azure'a bağlanması için gereken kimlik bilgilerini sağlamanız gerekir. Makalenin başlangıcında kopyaladığınız kodda, **GetCredentialsAsync** işlevi, yerel yapılandırma dosyasında sağlanan kimlik bilgilerini temel alarak ServiceClientCredentials nesnesi oluşturur.

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/UploadEncodeAndStreamFiles/Program.cs#CreateMediaServicesClient)]

### <a name="create-an-input-asset-and-upload-a-local-file-into-it"></a>Bir giriş varlığı oluşturma ve içine yerel dosya yükleme

**CreateInputAsset** işlevi yeni bir giriş [Varlığı](https://docs.microsoft.com/rest/api/media/assets) oluşturur ve içine belirtilen yerel video dosyasını yükler. Bu **Varlık,** kodlama işinize giriş olarak kullanılır. Medya Hizmetleri v3'te, **Bir İş'e** giriş, HTTPS URL'leri aracılığıyla Medya Hizmetleri hesabınıza sunduğunuz bir **Varlık** veya içerik olabilir. HTTPS URL'sinden nasıl kodkodlamayı öğrenmek için [bu](job-input-from-http-how-to.md) makaleye bakın.

Media Services v3’te dosyaları karşıya yüklemek için Azure Depolama API’lerini kullanırsınız. Aşağıdaki .NET kod parçacığı bunun nasıl yapıldığını gösterir.

Aşağıdaki işlev şu eylemleri gerçekleştirir:

* **Bir Varlık**oluşturur.
* Depodaki varlığın konteynerine yazılabilir bir [SAS](https://docs.microsoft.com/azure/storage/common/storage-dotnet-shared-access-signature-part-1) [URL'si](https://docs.microsoft.com/azure/storage/blobs/storage-quickstart-blobs-dotnet#upload-blobs-to-a-container)alır.

    SAS URL'lerini almak için varlığın [ListContainerSas](https://docs.microsoft.com/rest/api/media/assets/listcontainersas) işlevini kullanıyorsanız, her depolama hesabı için iki depolama hesabı anahtarı olduğundan işlevin birden çok SAS URL'si döndürtettiğini unutmayın. Depolama hesabının iki anahtarı vardır, çünkü depolama hesabı anahtarlarının sorunsuz bir şekilde döndürülmesine izin verir (örneğin, birini diğerini kullanırken değiştirin ve sonra yeni anahtarı kullanmaya başlayın ve diğer anahtarı döndürün). 1. SAS URL depolama anahtarı1 ve ikinci bir depolama anahtarı2 temsil eder.
* Dosyayı SAS URL'sini kullanarak depolama alanında kapsayıcıya yükler.

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/UploadEncodeAndStreamFiles/Program.cs#CreateInputAsset)]

### <a name="create-an-output-asset-to-store-the-result-of-a-job"></a>Bir işin sonucunu depolamak için çıktı varlığı oluşturma

Çıktı [Varlığı](https://docs.microsoft.com/rest/api/media/assets), kodlama işinizin sonucunu depolar. Proje, bu çıktı varlığının sonuçlarını "output" klasörüne indiren **DownloadResults** işlevini tanımlar, böylece elinizde neyin olduğunu görebilirsiniz.

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/UploadEncodeAndStreamFiles/Program.cs#CreateOutputAsset)]

### <a name="create-a-transform-and-a-job-that-encodes-the-uploaded-file"></a>Bir Dönüşüm ve karşıya yüklenen dosyayı kodlayan İş oluşturma

Medya Hizmetleri'nde içeriği kodlarken veya işlerken, kodlama ayarlarını bir reçete olarak ayarlamak yaygın bir deönemlidir. Daha sonra bu tarifi bir videoya uygulamak üzere bir **İş** gönderirsiniz. Her yeni video için yeni iş göndererek, bu tarifi kitaplığınızdaki tüm videolara uyguluyorsunuz. Medya Hizmetleri bir tarifi **dönüşüm**denir. Daha fazla bilgi için [Dönüşümler ve İşler'e](transform-concept.md)bakın. Bu öğreticide açıklanan örnek, videoyu çeşitli iOS ve Android cihazlarına akışla aktarmak için kodlayan bir tarifi tanımlar.

#### <a name="transform"></a>Dönüşüm

Yeni bir [Dönüşüm](https://docs.microsoft.com/rest/api/media/transforms) örneği oluştururken çıktı olarak neyi üretmesi istediğinizi belirtmeniz gerekir. Gerekli parametre, aşağıdaki kodda gösterildiği gibi bir **TransformOutput** nesnesidir. Her **TransformOutput** bir **Ön ayar** içerir. **Ön ayar**, video ve/veya ses işleme işlemlerinin istenen **TransformOutput** nesnesini oluşturmak üzere kullanılacak adım adım yönergelerini açıklar. Bu makalede açıklanan örnek, **AdaptiveStreaming** adlı yerleşik bir Ön Ayar kullanır. Ön Ayar, giriş çözünürlüğü ve bit hızını temel alarak, giriş videosunu otomatik olarak oluşturulan bir bit hızı basamağına (bit hızı-çözünürlük çiftleri) kodlar ve her bir bit hızı-çözünürlük çiftine karşılık gelen H.264 video ve AAC sesi ile ISO MP4 dosyaları üretir. Bu Ön Ayar hakkında bilgi için bkz. [otomatik oluşturulan bit hızı basamağı](autogen-bitrate-ladder.md).

Yerleşik bir EncoderNamedPreset ön ayarını veya özel ön ayarları kullanabilirsiniz. Daha fazla bilgi için bkz. [Kodlayıcı önayarlarını özelleştirme](customize-encoder-presets-how-to.md).

Bir [Dönüşüm](https://docs.microsoft.com/rest/api/media/transforms) oluştururken ilk olarak aşağıdaki kodda gösterildiği gibi **Get** yöntemi ile bir dönüşümün zaten var olup olmadığını denetlemeniz gerekir. Media Services v3’te varlıklar üzerindeki **Get** yöntemleri, varlığın mevcut olmaması durumunda **null** değerini döndürür (büyük/küçük harfe duyarlı ad denetimi).

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/UploadEncodeAndStreamFiles/Program.cs#EnsureTransformExists)]

#### <a name="job"></a>İş

Yukarıda bahsedildiği gibi [Transform](https://docs.microsoft.com/rest/api/media/transforms) nesnesi tarif, [Job](https://docs.microsoft.com/rest/api/media/jobs) ise bu **Transform** nesnesini belirli bir giriş videosu veya ses içeriğine uygulamak için Media Services’e gönderilen gerçek istektir. **İş** giriş videosunun konumu ve çıktının konumu gibi bilgileri belirtir.

Bu örnekte giriş videosu, yerel makinenizden yüklenmiştir. HTTPS URL'sinden kodlama yapmayı öğrenmek istiyorsanız, [bu](job-input-from-http-how-to.md) makaleye bakın.

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/UploadEncodeAndStreamFiles/Program.cs#SubmitJob)]

### <a name="wait-for-the-job-to-complete"></a>İşin tamamlanmasını bekleyin

İşin tamamlanması biraz sürüyor ve tamamlandığında bildirim almak istiyorsunuz. Aşağıdaki kod örneği, [İş](https://docs.microsoft.com/rest/api/media/jobs)’in durumu için hizmette nasıl yoklama yapılacağını gösterir. Yoklama, potansiyel gecikme nedeniyle üretim uygulamaları için önerilen en iyi uygulama değildir. Yoklama, bir hesap üzerinde gereğinden fazla kullanılırsa kısıtlanabilir. Geliştiricilerin onun yerine Event Grid kullanmalıdır.

Event Grid yüksek kullanılabilirlik, tutarlı performans ve dinamik ölçek için tasarlanmıştır. Event Grid ile uygulamalarınız neredeyse tüm Azure hizmetleri ve özel kaynaklardan gelen olayları takip edip bu olaylara yanıt verebilir. Basit, HTTP tabanlı reaktif olay işleme özelliği, olayların akıllı filtrelenmesi ve yönlendirilmesi sayesinde etkili çözümler oluşturmanıza yardımcı olur.  Bkz. [Olayları özel bir web uç noktasına yönlendirme](job-state-events-cli-how-to.md).

**İş** genellik şu aşamalardan geçer: **Zamanlandı**, **Kuyruğa Alındı**, **İşleniyor**, **Tamamlandı** (son aşama). İş bir hatayla karşılaştıysa **Hata** durumunu alırsınız. İş iptal edilme sürecindeyse, iş bittiğinde **İptal** edilir ve **İptal Edilir.**

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/UploadEncodeAndStreamFiles/Program.cs#WaitForJobToFinish)]

### <a name="job-error-codes"></a>İş hata kodları

[Bkz. Hata kodları.](https://docs.microsoft.com/rest/api/media/jobs/get#joberrorcode)

### <a name="get-a-streaming-locator"></a>Akış Lı Bulucu'su alın

Kodlama tamamlandıktan sonra sıradaki adım, çıktı Varlığındaki videoyu yürütmek için istemcilerin kullanımına sunmaktır. İki adımda kullanılabilir hale getirebilirsiniz: birincisi, bir [Akış Bulucu](https://docs.microsoft.com/rest/api/media/streaminglocators)oluşturmak ve ikincisi, istemcilerin kullanabileceği akış URL'leri oluşturun.

**Akış Lı Bulucu** oluşturma işlemine yayımlama denir. Varsayılan olarak, **Akış Bulucu,** API aramalarını yaptıktan hemen sonra geçerlidir ve isteğe bağlı başlangıç ve bitiş saatlerini yapılandırmadığınız sürece silinene kadar sürer.

[Bir StreamingLocator](https://docs.microsoft.com/rest/api/media/streaminglocators)oluştururken, istediğiniz **StreamingPolicyName**belirtmeniz gerekir. Bu örnekte, önceden tanımlanmış açık akış ilkesi **(PredefinedStreamingPolicy.ClearStreamingOnly)** kullanıldığından, açık (veya şifrelenmemiş içerik) akışı olursunuz.

> [!IMPORTANT]
> Özel bir [Akış İlkesi](https://docs.microsoft.com/rest/api/media/streamingpolicies)kullanırken, Medya Hizmeti hesabınız için sınırlı sayıda bu tür ilke ler tasarlamalı ve aynı şifreleme seçenekleri ve protokolleri gerektiğinde Bunları StreamingLocators'Larınız için yeniden kullanmalısınız. Medya Hizmeti hesabınızda Akış İlkesi girişi sayısı için bir kota vardır. Her Akış Bulucu için yeni bir Akış İlkesi oluşturmamalısınız.

Aşağıdaki kod, işlevi benzersiz bir konumbelirleyici Adı ile aradığınızı varsayar.

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/UploadEncodeAndStreamFiles/Program.cs#CreateStreamingLocator)]

Bu konudaki örnek akıştan bahsederken, aşamalı indirme yoluyla video sunmak için bir Akış Bulucu oluşturmak için aynı aramayı kullanabilirsiniz.

### <a name="get-streaming-urls"></a>Akış URL'leri alma

[Artık Akış Bulucu](https://docs.microsoft.com/rest/api/media/streaminglocators) oluşturulduğuna göre, **GetStreamingURLs'da**gösterildiği gibi akış URL'lerini alabilirsiniz. URL oluşturmak [için, Akış Uç Nokta](https://docs.microsoft.com/rest/api/media/streamingendpoints) ana bilgisayar adını ve **Akış Lı Konum belirleme** yolunu oluşturmanız gerekir. Bu örnekte, *varsayılan* **Akış Bitiş Noktası** kullanılır. Bir Medya Hizmeti hesabı ilk oluşturduğunuzda, bu *varsayılan* **Akış Bitiş Noktası** durdurulmuş durumda olacaktır, bu nedenle **Başlat'ı**aramanız gerekir.

> [!NOTE]
> Bu yöntemde, çıkış Varlığı için **Akış Bulucu'yu** oluştururken kullanılan yer bulucu Adı gerekir.

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/UploadEncodeAndStreamFiles/Program.cs#GetStreamingURLs)]

### <a name="clean-up-resources-in-your-media-services-account"></a>Media Services hesabınızdaki kaynakları temizleme

Genellikle, yeniden kullanmayı planladığınız nesneler dışındaki her şeyi temizlemeniz gerekir (genellikle Transforms'leri yeniden kullanırsınız ve StreamingLocators,vb.) devam eve gidersiniz. Denemeden sonra hesabınızın temiz olmasını istiyorsanız, yeniden kullanmayı planlamadığınız kaynakları silin. Örneğin, aşağıdaki kod İşler'i siler:

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/UploadEncodeAndStreamFiles/Program.cs#CleanUp)]

## <a name="run-the-sample-app"></a>Örnek uygulamayı çalıştırma

1. *EncodeAndStreamFiles* uygulamasını çalıştırmak için Ctrl+F5 tuşuna basın.
2. Akış URL’lerinden birini konsoldan kopyalayın.

Bu örnekte, farklı protokolleri kullanarak videoyu kayıttan yürütmek için kullanılabilen URL’ler gösterilir:

![Medya Hizmetleri akış videosu için URL'leri gösteren örnek çıktı](./media/stream-files-tutorial-with-api/output.png)

## <a name="test-the-streaming-url"></a>Akış URL’sini test etme

Bu makalede, akışı test etmek için Azure Media Player kullanılmaktadır.

> [!NOTE]
> Oynatıcı bir https sitesinde barındırılıyorsa, "https" URL’sini güncelleştirdiğinizden emin olun.

1. Bir web tarayıcısı [https://aka.ms/azuremediaplayer/](https://aka.ms/azuremediaplayer/)açın ve 'ye gidin.
2. **URL'ye:** kutu, uygulamayı çalıştırırken aldığınız akışurl değerlerinden birini yapıştırın.
3. **Güncelle Player'ı**seçin.

Azure Media Player sınama için kullanılabilir, ancak üretim ortamında kullanılmamalıdır.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu öğreticide oluşturduğunuz Media Services ve depolama hesapları dahil olmak üzere, kaynak grubunuzdaki kaynaklardan herhangi birine artık ihtiyacınız yoksa kaynak grubunu silebilirsiniz.

Aşağıdaki CLI komutunu uygulayın:

```azurecli
az group delete --name amsResourceGroup
```

## <a name="multithreading"></a>Çoklu iş parçacığı kullanımı

Azure Medya Hizmetleri v3 SDK'ları iş parçacığı için güvenli değildir. Çok iş parçacığı uygulaması geliştirirken, iş parçacığı başına yeni bir AzureMediaServicesClient nesnesi oluşturmalı ve kullanmalısınız.

## <a name="ask-questions-give-feedback-get-updates"></a>Soru sorun, geri bildirimde, güncellemeler alın

Soru sormanın, geri bildirimde bulunabilir ve Medya Hizmetleri hakkında güncellemeler alabildiğiniz farklı yolları görmek için [Azure Medya Hizmetleri topluluk](media-services-community.md) makalesine göz atın.

## <a name="next-steps"></a>Sonraki adımlar

Videonuzu karşıya yükleme, kodlama ve akışla aktarma hakkında bilgi edindiğinize göre aşağıdaki makaleye bakabilirsiniz: 

> [!div class="nextstepaction"]
> [Videoları analiz etme](analyze-videos-tutorial-with-api.md)
