---
title: Medya Hizmetleri v3 ile videoları analiz edin
titleSuffix: Azure Media Services
description: Azure Medya Hizmetleri'ni kullanarak videoları nasıl analiz edebilirsiniz öğrenin.
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
ms.sourcegitcommit: 8a9c54c82ab8f922be54fb2fcfd880815f25de77
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80349478"
---
# <a name="tutorial-analyze-videos-with-media-services-v3"></a>Öğretici: Medya Hizmetleri v3 ile videoları analiz edin

> [!NOTE]
> Bu öğretici [.NET SDK](https://docs.microsoft.com/dotnet/api/microsoft.azure.management.media.models.liveevent?view=azure-dotnet) örneklerini kullansa da, genel adımlar [REST API](https://docs.microsoft.com/rest/api/media/liveevents), [CLI](https://docs.microsoft.com/cli/azure/ams/live-event?view=azure-cli-latest)veya diğer desteklenen [SDK'lar](media-services-apis-overview.md#sdks)için aynıdır.

Bu öğreticide Azure Media Services ile videoları analiz etme işlemi gösterilir. Kayıtlı videolar veya ses içerikleri hakkında derin içgörüler kazanmak isteyebileceğiniz çok sayıda senaryo mevcuttur. Örneğin, daha yüksek müşteri memnuniyeti elde etmek isteyen kuruluşlar, müşteri destek kayıtlarını dizinler ve panolarla aranabilir bir katalog haline getirmek için konuşmayı metne dönüştürme işlemini çalıştırabilir. Daha sonra, kendi iş içine anlayışlar elde edebilirsiniz. Bu bilgiler, yaygın şikayetlerin bir listesini, bu tür şikayetlerin kaynaklarını ve diğer yararlı bilgileri içerir.

Bu öğretici şunların nasıl yapıldığını gösterir:

> [!div class="checklist"]
> * Konuyla açıklanan örnek uygulamayı indirin.
> * Belirtilen videoyu analiz eden kodu inceleyin.
> * Uygulamayı çalıştırın.
> * Çıktıyı inceleyin.
> * Kaynakları temizleyin.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="compliance-privacy-and-security"></a>Uyumluluk, Gizlilik ve Güvenlik
 
Önemli bir hatırlatma olarak, Video Dizinleyici'yi kullanımınızda geçerli tüm yasalara uymanız gerekir ve Video Dizinleyici'yi veya başka bir Azure hizmetini başkalarının haklarını ihlal eden veya başkalarına zarar verebilecek şekilde kullanamazsınız. Herhangi bir biyometrik veri de dahil olmak üzere herhangi bir videoyu işleme ve depolama için Video Dizinleyici hizmetine yüklemeden önce, videodaki bireyden (ler) tüm uygun izinler de dahil olmak üzere tüm uygun haklara sahip olmalısınız. Video Dizinleyici'de uyumluluk, gizlilik ve güvenlik hakkında bilgi edinmek için, Microsoft [Bilişsel Hizmetler Koşulları.](https://azure.microsoft.com/support/legal/cognitive-services-compliance-and-privacy/) Microsoft'un gizlilik yükümlülükleri ve verilerinizin işlenmesi için lütfen Microsoft'un [Gizlilik Bildirimini,](https://privacy.microsoft.com/PrivacyStatement) [Çevrimiçi Hizmet Koşullarını](https://www.microsoft.com/licensing/product-licensing/products) ("OST") ve Veri [İşleme Eki'ni](https://www.microsoftvolumelicensing.com/DocumentSearch.aspx?Mode=3&DocumentTypeId=67) ("DPA") gözden geçirin. Veri saklama, silme/imha gibi ek gizlilik bilgileri OST'de ve [burada](../video-indexer/faq.md)mevcuttur. Video Indexer'ı kullanarak Bilişsel Hizmetler Koşulları, OST, DPA ve Gizlilik Bildirimi'ne bağlı olmayı kabul edersiniz.

## <a name="prerequisites"></a>Ön koşullar

- Visual Studio yüklü değilseniz Visual [Studio Community 2017'yi](https://www.visualstudio.com/thank-you-downloading-visual-studio/?sku=Community&rel=15)alın.
- [Bir Medya Hizmetleri hesabı oluşturun.](create-account-cli-how-to.md)<br/>Kaynak grubu adı ve Medya Hizmetleri hesap adı için kullandığınız değerleri hatırladığınızdan emin olun.
- [Azure CLI ile Azure Medya Hizmetleri API'sindeki](access-api-cli-how-to.md) adımları izleyin ve kimlik bilgilerini kaydedin. API'ye erişmek için bunları kullanmanız gerekir.

## <a name="download-and-configure-the-sample"></a>Örneği indirin ve yapılandırın

Aşağıdaki komutu kullanarak, .NET örneğini içeren bir GitHub havuzunu makinenize kopyalayın:  

 ```bash
 git clone https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials.git
 ```

Örnek, [AnalyzeVideos](https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials/tree/master/AMSV3Tutorials/AnalyzeVideos) klasöründe yer alır.

İndirilen projede [appsettings.json'ı](https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials/blob/master/AMSV3Tutorials/AnalyzeVideos/appsettings.json) açın. Değerleri [API'lara erişirerek](access-api-cli-how-to.md)aldığınız kimlik bilgileriyle değiştirin.

## <a name="examine-the-code-that-analyzes-the-specified-video"></a>Belirtilen videoyu analiz eden kodu inceleme

Bu bölümde, *AnalyzeVideos* projesinin [Program.cs](https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials/blob/master/AMSV3Tutorials/AnalyzeVideos/Program.cs) dosyasında tanımlı işlevler incelenmektedir.

Örnek aşağıdaki eylemleri tamamlar:

1. Videonuzu analiz eden bir **Dönüşüm** ve **İş** oluşturur.
2. Bir giriş **Varlığı** oluşturur ve videoyu içine yükler. Varlık, işin girişi olarak kullanılır.
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

**CreateInputAsset** işlevi yeni bir giriş [Varlığı](https://docs.microsoft.com/rest/api/media/assets) oluşturur ve içine belirtilen yerel video dosyasını yükler. Bu Varlık, kodlama işinizde giriş olarak kullanılır. Media Services v3’te bir İşe yapılan giriş, Varlık olabilir veya HTTPS URL’leri üzerinden Media Services hesabınızın kullanımına açtığınız bir içerik olabilir. HTTPS URL'sinden nasıl kodkodlamayı öğrenmek için [bu](job-input-from-http-how-to.md) makaleye bakın.  

Media Services v3’te dosyaları karşıya yüklemek için Azure Depolama API’lerini kullanırsınız. Aşağıdaki .NET kod parçacığı bunun nasıl yapıldığını gösterir.

Aşağıdaki işlev bu eylemleri tamamlar:

* Bir Varlık oluşturur.
* Depodaki Varlığın konteynerine yazılabilir bir [SAS](https://docs.microsoft.com/azure/storage/common/storage-dotnet-shared-access-signature-part-1) [URL'si](https://docs.microsoft.com/azure/storage/blobs/storage-quickstart-blobs-dotnet#upload-blobs-to-a-container)alır.

    SAS URL'lerini almak için varlığın [ListContainerSas](https://docs.microsoft.com/rest/api/media/assets/listcontainersas) işlevini kullanıyorsanız, her depolama hesabı için iki depolama hesabı anahtarı olduğundan işlevin birden çok SAS URL'si döndürtettiğini unutmayın. Depolama hesabının iki anahtarı vardır, çünkü depolama hesabı anahtarlarının sorunsuz bir şekilde döndürülmesine izin verir (örneğin, birini diğerini kullanırken değiştirin ve sonra yeni anahtarı kullanmaya başlayın ve diğer anahtarı döndürün). 1. SAS URL depolama anahtarı1 ve ikinci bir depolama anahtarı2 temsil eder.
* Dosyayı SAS URL'sini kullanarak depolama alanında kapsayıcıya yükler.

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/AnalyzeVideos/Program.cs#CreateInputAsset)]

### <a name="create-an-output-asset-to-store-the-result-of-the-job"></a>İşin sonucunu depolamak için çıkış varlığı oluşturma

[Çıktı](https://docs.microsoft.com/rest/api/media/assets) varlığı, işinizin sonucunu depolar. Proje, bu çıktı varlığının sonuçlarını "output" klasörüne indiren **DownloadResults** işlevini tanımlar, böylece elinizde neyin olduğunu görebilirsiniz.

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/AnalyzeVideos/Program.cs#CreateOutputAsset)]

### <a name="create-a-transform-and-a-job-that-analyzes-videos"></a>Bir dönüşüm ve videoları analiz eden bir iş oluşturma

Medya Hizmetleri'nde içeriği kodlarken veya işlerken, kodlama ayarlarını bir reçete olarak ayarlamak yaygın bir deönemlidir. Daha sonra bu tarifi bir videoya uygulamak üzere bir **İş** gönderirsiniz. Her yeni video için yeni İşler göndererek, bu tarifi kitaplığınızdaki tüm videolara uyguluyorsunuz. Medya Hizmetleri bir tarifi **dönüşüm**denir. Daha fazla bilgi için [Dönüşümler ve işler](transform-concept.md) konusuna bakın. Bu öğreticide açıklanan örnek, belirtilen videoyu analiz eden bir tarifi tanımlar.

#### <a name="transform"></a>Dönüşüm

Yeni bir [Dönüşüm](https://docs.microsoft.com/rest/api/media/transforms) örneği oluştururken çıktı olarak neyi üretmesi istediğinizi belirtmeniz gerekir. **TransformOutput** gerekli bir parametredir. Her **TransformOutput** bir **Ön ayar** içerir. **Ön ayar**, video ve/veya ses işleme işlemlerinin istenen **TransformOutput** nesnesini oluşturmak üzere kullanılacak adım adım yönergelerini açıklar. Bu örnekte, **VideoAnalyzerPreset** önceden ayarlanmış kullanılır ve dil ("en-US") onun`new VideoAnalyzerPreset("en-US")`oluşturucu ( ) geçirilir. Bu ön ayar, bir videodan birden fazla ses ve video içgörüsü elde etmenizi sağlar. Bir videodan birden fazla ses içgörüsü elde etmeniz gerekiyorsa **AudioAnalyzerPreset** ön ayarını kullanın.

**Dönüşüm**oluştururken, aşağıdaki kodda gösterildiği **gibi, Get** yöntemini kullanarak önce bir tane nin var olup olmadığını denetleyin. Media Services v3’te varlıklar üzerindeki **Get** yöntemleri, varlığın mevcut olmaması durumunda **null** değerini döndürür (büyük/küçük harfe duyarlı ad denetimi).

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/AnalyzeVideos/Program.cs#EnsureTransformExists)]

#### <a name="job"></a>İş

Yukarıda bahsedildiği gibi [Transform](https://docs.microsoft.com/rest/api/media/transforms) nesnesi tarif, [Job](https://docs.microsoft.com/rest/api/media/jobs) ise bu **Transform** nesnesini belirli bir giriş videosu veya ses içeriğine uygulamak için Media Services’e gönderilen gerçek istektir. **İş,** giriş videosunun konumu ve çıktının konumu gibi bilgileri belirtir. Videonuzun konumunu, Media Services hesabınızda bulunan HTTPS URL’lerini, SAS URL’lerini veya Varlıkları kullanarak belirtebilirsiniz.

Bu örnekte, iş girdisi yerel bir videodur.  

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/AnalyzeVideos/Program.cs#SubmitJob)]

### <a name="wait-for-the-job-to-complete"></a>İşin tamamlanmasını bekleyin

İşi tamamlamak biraz zaman alır. Bu olduğunda, size haberdar olmak istiyorum. [İşin](https://docs.microsoft.com/rest/api/media/jobs) tamamlanması hakkında bildirim almaya ilişkin farklı seçenekler mevcuttur. En basit seçenek (burada gösterilen) yoklama kullanmaktır.

Yoklama, potansiyel gecikme nedeniyle üretim uygulamaları için önerilen en iyi uygulama değildir. Yoklama, bir hesap üzerinde gereğinden fazla kullanılırsa kısıtlanabilir. Geliştiricilerin onun yerine Event Grid kullanmalıdır.

Event Grid yüksek kullanılabilirlik, tutarlı performans ve dinamik ölçek için tasarlanmıştır. Event Grid ile uygulamalarınız neredeyse tüm Azure hizmetleri ve özel kaynaklardan gelen olayları takip edip bu olaylara yanıt verebilir. Basit, HTTP tabanlı reaktif olay işleme özelliği, olayların akıllı filtrelenmesi ve yönlendirilmesi sayesinde etkili çözümler oluşturmanıza yardımcı olur. Daha fazla bilgi için, [olayları özel bir web bitiş noktasına yönlendir'](job-state-events-cli-how-to.md)e bakın.

**İş** genellik şu aşamalardan geçer: **Zamanlandı**, **Kuyruğa Alındı**, **İşleniyor**, **Tamamlandı** (son aşama). İş bir hatayla karşılaştıysa, **Hata** durumunu alırsınız. İş iptal edilme sürecindeyse, **İptal** edilir ve bittiğinde **iptal edilir.**

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/AnalyzeVideos/Program.cs#WaitForJobToFinish)]

### <a name="job-error-codes"></a>İş hata kodları

[Bkz. Hata kodları.](https://docs.microsoft.com/rest/api/media/jobs/get#joberrorcode)

### <a name="download-the-result-of-the-job"></a>İşin sonucunu indirme

Aşağıdaki işlev, çıktı [Varlık'tan](https://docs.microsoft.com/rest/api/media/assets) gelen sonuçları "çıktı" klasörüne indirir, böylece işin sonuçlarını inceleyebilirsiniz.

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/AnalyzeVideos/Program.cs#DownloadResults)]

### <a name="clean-up-resource-in-your-media-services-account"></a>Media Services hesabınızdaki kaynakları temizleme

Genellikle, yeniden kullanmayı planladığınız nesneler dışındaki her şeyi temizlemeniz gerekir (genellikle, Transforms'ı yeniden kullanır ve StreamingLocators'ı devam ettirmeniz gerekir). Denemeden sonra hesabınızın temiz olmasını istiyorsanız, yeniden kullanmayı planlamadığınız kaynakları silin. Örneğin, aşağıdaki kod İşler'i siler:

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/AnalyzeVideos/Program.cs#CleanUp)]

## <a name="run-the-sample-app"></a>Örnek uygulamayı çalıştırma

*AnalyzeVideos* uygulamasını çalıştırmak için Ctrl+F5 tuşuna basın.

Programı çalıştırdığınızda iş, videoda bulduğu her yüz için küçük resimler üretir. Ayrıca, insights.json dosyasını oluşturur.

## <a name="examine-the-output"></a>Çıktıyı inceleme

Analiz edilen videoların çıktı dosyası insights.json olarak adlandırılır. Bu dosya, videonuz hakkında içgörüler içerir. Json dosyasındaki öğelerin açıklamalarını [Medya zekası](intelligence-concept.md) makalesinde bulabilirsiniz.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu öğreticide oluşturduğunuz Media Services ve depolama hesapları dahil olmak üzere, kaynak grubunuzdaki kaynaklardan herhangi birine artık ihtiyacınız yoksa kaynak grubunu silebilirsiniz.

Aşağıdaki CLI komutunu uygulayın:

```azurecli
az group delete --name amsResourceGroup
```

## <a name="multithreading"></a>Çoklu iş parçacığı kullanımı

Azure Medya Hizmetleri v3 SDK'ları iş parçacığı için güvenli değildir. Çok iş parçacığı uygulamasıyla çalışırken, iş parçacığı başına yeni bir AzureMediaServicesClient nesnesi oluşturmanız gerekir.

## <a name="ask-questions-give-feedback-get-updates"></a>Soru sorun, geri bildirimde, güncellemeler alın

Soru sormanın, geri bildirimde bulunabilir ve Medya Hizmetleri hakkında güncellemeler alabildiğiniz farklı yolları görmek için [Azure Medya Hizmetleri topluluk](media-services-community.md) makalesine göz atın.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Öğretici: dosyaları karşıya yükleme, kodlama ve akışa alma](stream-files-tutorial-with-api.md)
