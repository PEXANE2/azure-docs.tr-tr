---
title: Media Services v3 ile videoları analiz etme
titleSuffix: Azure Media Services
description: Azure Media Services kullanarak videoları çözümlemeyi öğrenin.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: tutorial
ms.date: 03/26/2020
ms.author: juliako
ms.custom: seodec18
ms.openlocfilehash: b7864d89cc14a1473fd43e94bfe74c368bcb391d
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80349478"
---
# <a name="tutorial-analyze-videos-with-media-services-v3"></a>Öğretici: Media Services v3 ile videoları analiz etme

> [!NOTE]
> Bu öğretici [.NET SDK](https://docs.microsoft.com/dotnet/api/microsoft.azure.management.media.models.liveevent?view=azure-dotnet) örnekleri kullanıyor olsa da, genel adımlar [REST API](https://docs.microsoft.com/rest/api/media/liveevents), [CLI](https://docs.microsoft.com/cli/azure/ams/live-event?view=azure-cli-latest)veya desteklenen diğer [SDK](media-services-apis-overview.md#sdks)'lar için aynıdır.

Bu öğreticide Azure Media Services ile videoları analiz etme işlemi gösterilir. Kayıtlı videolar veya ses içerikleri hakkında derin içgörüler kazanmak isteyebileceğiniz çok sayıda senaryo mevcuttur. Örneğin, daha yüksek müşteri memnuniyeti elde etmek isteyen kuruluşlar, müşteri destek kayıtlarını dizinler ve panolarla aranabilir bir katalog haline getirmek için konuşmayı metne dönüştürme işlemini çalıştırabilir. Daha sonra, işletdiklerinde Öngörüler elde edebilirler. Bu Öngörüler, yaygın şikayetlerin bir listesini, bu tür şikayetlerin kaynaklarını ve diğer yararlı bilgileri içerir.

Bu öğretici şunların nasıl yapıldığını gösterir:

> [!div class="checklist"]
> * Konusunda açıklanan örnek uygulamayı indirin.
> * Belirtilen videoyu çözümleyen kodu inceleyin.
> * Uygulamayı çalıştırın.
> * Çıktıyı inceleyin.
> * Kaynakları temizleyin.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="compliance-privacy-and-security"></a>Uyumluluk, Gizlilik ve Güvenlik
 
Önemli bir anımsatıcı olarak, Video Indexer ortamınızda geçerli olan tüm yasalara uymak zorundasınız ve Video Indexer ya da başka bir Azure hizmetini diğerlerinin haklarını ihlal eden veya başkalarına zararlı olabilecek bir biçimde kullanamazsınız. Biyometrik veriler de dahil olmak üzere herhangi bir videoyu, işleme ve depolama için Video Indexer hizmetine yüklemeden önce, videodaki her türden uygun tüm haklara sahip olmanız gerekir. Microsoft bilişsel [Hizmetler koşulları](https://azure.microsoft.com/support/legal/cognitive-services-compliance-and-privacy/)video Indexer, uyumluluk, gizlilik ve güvenlik hakkında bilgi edinmek için. Microsoft 'un gizlilik yükümlülüklerini ve verilerinizi işleme için lütfen Microsoft 'un [Gizlilik bildirimini](https://privacy.microsoft.com/PrivacyStatement), [çevrimiçi hizmetler koşullarını](https://www.microsoft.com/licensing/product-licensing/products) ("OST") ve [veri Işleme eki](https://www.microsoftvolumelicensing.com/DocumentSearch.aspx?Mode=3&DocumentTypeId=67) 'ni ("DPA") gözden geçirin. Veri saklama, silme/yok etme dahil olmak üzere ek gizlilik bilgileri, OST ve [burada](../video-indexer/faq.md)bulunabilir. Video Indexer kullanarak bilişsel hizmetler koşulları, OST, DPA ve gizlilik bildirimiyle bağlanmayı kabul etmiş olursunuz.

## <a name="prerequisites"></a>Ön koşullar

- Visual Studio yüklü değilse, [Visual Studio Community 2017](https://www.visualstudio.com/thank-you-downloading-visual-studio/?sku=Community&rel=15)' i alın.
- [Media Services hesabı oluşturun](create-account-cli-how-to.md).<br/>Kaynak grubu adı ve Media Services hesap adı için kullandığınız değerleri anımsadığınızdan emin olun.
- [Azure CLI Ile Access Azure Media Services API 'sindeki](access-api-cli-how-to.md) adımları izleyin ve kimlik bilgilerini kaydedin. API 'ye erişmek için bunları kullanmanız gerekir.

## <a name="download-and-configure-the-sample"></a>Örneği indirin ve yapılandırın

Aşağıdaki komutu kullanarak, .NET örneğini içeren bir GitHub havuzunu makinenize kopyalayın:  

 ```bash
 git clone https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials.git
 ```

Örnek, [AnalyzeVideos](https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials/tree/master/AMSV3Tutorials/AnalyzeVideos) klasöründe yer alır.

İndirilen projenizde [appSettings. JSON](https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials/blob/master/AMSV3Tutorials/AnalyzeVideos/appsettings.json) ' yı açın. Değerleri, [API 'lere erişirken](access-api-cli-how-to.md)aldığınız kimlik bilgileriyle değiştirin.

## <a name="examine-the-code-that-analyzes-the-specified-video"></a>Belirtilen videoyu analiz eden kodu inceleme

Bu bölümde, *AnalyzeVideos* projesinin [Program.cs](https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials/blob/master/AMSV3Tutorials/AnalyzeVideos/Program.cs) dosyasında tanımlı işlevler incelenmektedir.

Örnek aşağıdaki eylemleri tamamlar:

1. Görüntünüzü çözümleyen bir **dönüşüm** ve **iş** oluşturur.
2. Bir giriş **varlığı** oluşturur ve videoyu buna yükler. Varlık, işin girişi olarak kullanılır.
3. İşin çıkışını depolayan bir çıkış varlığı oluşturur.
4. İşi gönderir.
5. İşin durumunu denetler.
6. İşin çalıştırılması sonucu oluşan dosyaları indirir.

> [!NOTE]
> Video veya Ses Çözümleyicisi önayarlarını kullanırken, hesabınızı 10 S3 Medya Ayrılmış Birimine sahip olacak şekilde ayarlamak için Azure portalı kullanın. Daha fazla bilgi için bkz. [Medya işlemeyi ölçeklendirme](media-reserved-units-cli-how-to.md).

### <a name="start-using-media-services-apis-with-net-sdk"></a>.NET SDK ile Media Services API’sini kullanmaya başlama

.NET ile Media Services API’lerini kullanmaya başlamak için bir **AzureMediaServicesClient** nesnesi oluşturmanız gerekir. Nesneyi oluşturmak için, Azure AD kullanarak Azure’a bağlanmak üzere istemcinin ihtiyaç duyduğu kimlik bilgilerini sağlamanız gerekir. Makalenin başlangıcında kopyaladığınız kodda, **GetCredentialsAsync** işlevi, yerel yapılandırma dosyasında sağlanan kimlik bilgilerini temel alarak ServiceClientCredentials nesnesi oluşturur. 

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/AnalyzeVideos/Program.cs#CreateMediaServicesClient)]

### <a name="create-an-input-asset-and-upload-a-local-file-into-it"></a>Bir giriş varlığı oluşturma ve içine yerel dosya yükleme 

**CreateInputAsset** işlevi yeni bir giriş [Varlığı](https://docs.microsoft.com/rest/api/media/assets) oluşturur ve içine belirtilen yerel video dosyasını yükler. Bu Varlık, kodlama işinizde giriş olarak kullanılır. Media Services v3’te bir İşe yapılan giriş, Varlık olabilir veya HTTPS URL’leri üzerinden Media Services hesabınızın kullanımına açtığınız bir içerik olabilir. HTTPS URL 'sinden kodlama hakkında bilgi edinmek için [Bu](job-input-from-http-how-to.md) makaleye bakın.  

Media Services v3’te dosyaları karşıya yüklemek için Azure Depolama API’lerini kullanırsınız. Aşağıdaki .NET kod parçacığı bunun nasıl yapıldığını gösterir.

Aşağıdaki işlev bu eylemleri tamamlar:

* Bir varlık oluşturur.
* [Depolama alanındaki varlığın kapsayıcısına](https://docs.microsoft.com/azure/storage/blobs/storage-quickstart-blobs-dotnet#upload-blobs-to-a-container)yazılabilir bir [SAS URL 'si](https://docs.microsoft.com/azure/storage/common/storage-dotnet-shared-access-signature-part-1) alır.

    SAS URL 'Lerini almak için varlığın [Listcontainersas](https://docs.microsoft.com/rest/api/media/assets/listcontainersas) işlevini kullanıyorsanız, her depolama hesabı için iki depolama hesabı anahtarı olduğu için işlevin bırden çok SAS URL 'si döndürdüğünü unutmayın. Depolama hesabı anahtarlarının sorunsuz bir şekilde değiştirilmesini sağladığından bir depolama hesabının iki anahtarı vardır (örneğin, diğerini kullanırken bir tane değiştirin, yeni anahtarı kullanmaya başlayın ve diğer anahtarı döndürün). 1. SAS URL 'SI, Storage KEY1 ve ikinci bir Storage key2 öğesini temsil eder.
* SAS URL 'sini kullanarak dosyayı depolama alanındaki kapsayıcıya yükler.

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/AnalyzeVideos/Program.cs#CreateInputAsset)]

### <a name="create-an-output-asset-to-store-the-result-of-the-job"></a>İşin sonucunu depolamak için çıkış varlığı oluşturma

[Çıktı](https://docs.microsoft.com/rest/api/media/assets) varlığı, işinizin sonucunu depolar. Proje, bu çıktı varlığının sonuçlarını "output" klasörüne indiren **DownloadResults** işlevini tanımlar, böylece elinizde neyin olduğunu görebilirsiniz.

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/AnalyzeVideos/Program.cs#CreateOutputAsset)]

### <a name="create-a-transform-and-a-job-that-analyzes-videos"></a>Bir dönüşüm ve videoları analiz eden bir iş oluşturma

Media Services içeriği kodlarken veya işlerken, kodlama ayarlarını tarif olarak ayarlamak için ortak bir modeldir. Daha sonra bu tarifi bir videoya uygulamak üzere bir **İş** gönderirsiniz. Her yeni video için yeni Işler göndererek, bu tarifi kitaplığınızdaki tüm videolara uygulayacağız. Media Services bir tarif, **dönüşüm**olarak adlandırılır. Daha fazla bilgi için [Dönüşümler ve işler](transform-concept.md) konusuna bakın. Bu öğreticide açıklanan örnek, belirtilen videoyu analiz eden bir tarifi tanımlar.

#### <a name="transform"></a>Dönüşüm

Yeni bir [Dönüşüm](https://docs.microsoft.com/rest/api/media/transforms) örneği oluştururken çıktı olarak neyi üretmesi istediğinizi belirtmeniz gerekir. **Transformoutput** gerekli bir parametredir. Her **TransformOutput** bir **Ön ayar** içerir. **Ön ayar**, video ve/veya ses işleme işlemlerinin istenen **TransformOutput** nesnesini oluşturmak üzere kullanılacak adım adım yönergelerini açıklar. Bu örnekte, **videoanalizin önceden belirlenmiş** ayarı kullanılır ve dil ("en-US") oluşturucuya (`new VideoAnalyzerPreset("en-US")`) geçirilir. Bu ön ayar, bir videodan birden fazla ses ve video içgörüsü elde etmenizi sağlar. Bir videodan birden fazla ses içgörüsü elde etmeniz gerekiyorsa **AudioAnalyzerPreset** ön ayarını kullanın.

Bir **dönüşüm**oluştururken, aşağıdaki kodda gösterildiği gibi **Get** metodunu kullanarak zaten bir tane varsa işaretleyin. Media Services v3’te varlıklar üzerindeki **Get** yöntemleri, varlığın mevcut olmaması durumunda **null** değerini döndürür (büyük/küçük harfe duyarlı ad denetimi).

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/AnalyzeVideos/Program.cs#EnsureTransformExists)]

#### <a name="job"></a>İş

Yukarıda bahsedildiği gibi [Transform](https://docs.microsoft.com/rest/api/media/transforms) nesnesi tarif, [Job](https://docs.microsoft.com/rest/api/media/jobs) ise bu **Transform** nesnesini belirli bir giriş videosu veya ses içeriğine uygulamak için Media Services’e gönderilen gerçek istektir. **İş** , giriş videonun konumu ve çıktının konumu gibi bilgileri belirtir. Videonuzun konumunu, Media Services hesabınızda bulunan HTTPS URL’lerini, SAS URL’lerini veya Varlıkları kullanarak belirtebilirsiniz.

Bu örnekte, iş girdisi yerel bir videodur.  

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/AnalyzeVideos/Program.cs#SubmitJob)]

### <a name="wait-for-the-job-to-complete"></a>İşin tamamlanmasını bekleyin

İşin tamamlaması biraz zaman alır. Ne zaman sorulduğunda bildirim almak istersiniz. [İşin](https://docs.microsoft.com/rest/api/media/jobs) tamamlanması hakkında bildirim almaya ilişkin farklı seçenekler mevcuttur. En basit seçenek (burada gösterilen) yoklamayı kullanmaktır.

Yoklama, olası gecikme nedeniyle üretim uygulamaları için önerilen en iyi uygulamadır. Yoklama, bir hesap üzerinde gereğinden fazla kullanılırsa kısıtlanabilir. Geliştiricilerin onun yerine Event Grid kullanmalıdır.

Event Grid yüksek kullanılabilirlik, tutarlı performans ve dinamik ölçek için tasarlanmıştır. Event Grid ile uygulamalarınız neredeyse tüm Azure hizmetleri ve özel kaynaklardan gelen olayları takip edip bu olaylara yanıt verebilir. Basit, HTTP tabanlı reaktif olay işleme özelliği, olayların akıllı filtrelenmesi ve yönlendirilmesi sayesinde etkili çözümler oluşturmanıza yardımcı olur. Daha fazla bilgi için bkz. [olayları özel bir Web uç noktasına yönlendirme](job-state-events-cli-how-to.md).

**İş** genellik şu aşamalardan geçer: **Zamanlandı**, **Kuyruğa Alındı**, **İşleniyor**, **Tamamlandı** (son aşama). İş bir hatayla karşılaşmışsa, **hata** durumunu alırsınız. İşin iptal edilme süreciiyorsa, **iptal** edip tamamlandığında **iptal etmiş** olursunuz.

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/AnalyzeVideos/Program.cs#WaitForJobToFinish)]

### <a name="job-error-codes"></a>İş hata kodları

Bkz. [hata kodları](https://docs.microsoft.com/rest/api/media/jobs/get#joberrorcode).

### <a name="download-the-result-of-the-job"></a>İşin sonucunu indirme

Aşağıdaki işlev, iş sonuçlarını incelemenize olanak sağlamak için [Çıkış varlıkındaki sonuçları "çıkış"](https://docs.microsoft.com/rest/api/media/assets) klasörüne indirir.

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/AnalyzeVideos/Program.cs#DownloadResults)]

### <a name="clean-up-resource-in-your-media-services-account"></a>Media Services hesabınızdaki kaynakları temizleme

Genellikle, yeniden kullanmayı planladığınız nesneler hariç her şeyi temizlemeniz gerekir (genellikle dönüştürmeleri yeniden kullanacaksınız ve Streamingbulleyicileri kalıcı hale getiriyorsunuz). Deneme sonrasında hesabınızın temiz olmasını istiyorsanız, kullanmayı planlamadığınız kaynakları silin. Örneğin, aşağıdaki kod Işleri siler:

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/AnalyzeVideos/Program.cs#CleanUp)]

## <a name="run-the-sample-app"></a>Örnek uygulamayı çalıştırma

*Analiz Zevideolarını* uygulamasını çalıştırmak için CTRL + F5 tuşlarına basın.

Programı çalıştırdığınızda iş, videoda bulduğu her yüz için küçük resimler üretir. Ayrıca, insights.json dosyasını oluşturur.

## <a name="examine-the-output"></a>Çıktıyı inceleme

Analiz edilen videoların çıktı dosyası insights.json olarak adlandırılır. Bu dosya, videonuz hakkında içgörüler içerir. Json dosyasındaki öğelerin açıklamalarını [Medya zekası](intelligence-concept.md) makalesinde bulabilirsiniz.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu öğreticide oluşturduğunuz Media Services ve depolama hesapları dahil olmak üzere, kaynak grubunuzdaki kaynaklardan herhangi birine artık ihtiyacınız yoksa kaynak grubunu silebilirsiniz.

Aşağıdaki CLı komutunu yürütün:

```azurecli
az group delete --name amsResourceGroup
```

## <a name="multithreading"></a>Çoklu iş parçacığı kullanımı

Azure Media Services v3 SDK 'Ları iş parçacığı açısından güvenli değildir. Çok iş parçacıklı bir uygulamayla çalışırken, iş parçacığı başına yeni bir Azudüzeltici Istemci nesnesi oluşturmanız gerekir.

## <a name="ask-questions-give-feedback-get-updates"></a>Soru sorun, geri bildirimde bulunun, güncelleştirmeleri al

Soru sormak, geri bildirimde bulunmak ve Media Services hakkında güncelleştirmeler almak için [Azure Media Services Community](media-services-community.md) makalesine göz atın.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Öğretici: dosyaları karşıya yükleme, kodlama ve akışa alma](stream-files-tutorial-with-api.md)
