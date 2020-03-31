---
title: Azure Media Services v3 ile tanımlayıcı ses parçaları sinyali | Microsoft Dokümanlar
description: Bir dosya yüklemek, videoyu kodlamak, açıklayıcı ses parçaları eklemek ve Medya Hizmetleri v3 ile içeriğinizi aktarmak için bu öğreticinin adımlarını izleyin.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.custom: ''
ms.date: 09/25/2019
ms.author: juliako
ms.openlocfilehash: 0d8f88e6c2fe273efa969278146de67ba18eaecf
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "72392195"
---
# <a name="signal-descriptive-audio-tracks"></a>Sinyal tanımlayıcı ses parçaları

Görsel engelli istemcilerin anlatımı dinleyerek video kaydını takip edemelerine yardımcı olmak için videonuza bir anlatım parçası ekleyebilirsiniz. Media Services v3'te, bildirim dosyasındaki ses parçasına açıklama ekleyerek açıklayıcı ses parçalarını işaret emzebilirsiniz.

Bu makalede, bir videonun nasıl kodlanılması, çıktı varlığına açıklayıcı ses içeren yalnızca ses mp4 dosyasının (AAC codec) nasıl yüklenilen ve .ism dosyasını açıklayıcı sesi içerecek şekilde nasıl düzeltilir.

## <a name="prerequisites"></a>Ön koşullar

- [Bir Medya Hizmetleri hesabı oluşturun.](create-account-cli-how-to.md)
- [Azure CLI ile Azure Medya Hizmetleri API'sindeki](access-api-cli-how-to.md) adımları izleyin ve kimlik bilgilerini kaydedin. API'ye erişmek için bunları kullanmanız gerekir.
- Dinamik ambalajı gözden [geçirin.](dynamic-packaging-overview.md)
- Video [yükle, kodlama ve akış videolarını](stream-files-tutorial-with-api.md) inceleyin.

## <a name="create-an-input-asset-and-upload-a-local-file-into-it"></a>Bir giriş varlığı oluşturma ve içine yerel dosya yükleme 

**CreateInputAsset** işlevi yeni bir giriş [Varlığı](https://docs.microsoft.com/rest/api/media/assets) oluşturur ve içine belirtilen yerel video dosyasını yükler. Bu **Varlık,** kodlama İşinize giriş olarak kullanılır. Medya Hizmetleri v3'te, **Bir İş'e** giriş bir **Varlık**veya HTTPS URL'leri aracılığıyla Medya Hizmetleri hesabınıza sunduğunuz içerik olabilir. 

HTTPS URL'sinden kodlama yı öğrenmek istiyorsanız, [bu makaleye](job-input-from-http-how-to.md) bakın.  

Media Services v3’te dosyaları karşıya yüklemek için Azure Depolama API’lerini kullanırsınız. Aşağıdaki .NET kod parçacığı bunun nasıl yapıldığını gösterir.

Aşağıdaki işlev şu eylemleri gerçekleştirir:

* **Varlık** Oluşturur 
* Depodaki varlığın konteynerine yazılabilir bir [SAS](https://docs.microsoft.com/azure/storage/common/storage-dotnet-shared-access-signature-part-1) [URL'si](https://docs.microsoft.com/azure/storage/blobs/storage-quickstart-blobs-dotnet#upload-blobs-to-a-container) alır
* SAS URL’sini kullanarak dosyayı depolamadaki kapsayıcıya yükler

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/UploadEncodeAndStreamFiles/Program.cs#CreateInputAsset)]

Oluşturulan giriş kıymetinin adını diğer yöntemlere geçirmeniz gerekiyorsa, dönen `Name` varlık nesnesi `CreateInputAssetAsync`üzerindeki özelliği (örneğin, inputAsset.Name. 

## <a name="create-an-output-asset-to-store-the-result-of-the-encoding-job"></a>Kodlama işinin sonucunu depolamak için bir çıktı kıymeti oluşturma

Çıktı [Varlığı](https://docs.microsoft.com/rest/api/media/assets), kodlama işinizin sonucunu depolar. Aşağıdaki işlev, bir çıktı varlığının nasıl oluşturulabildiğini gösterir.

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/UploadEncodeAndStreamFiles/Program.cs#CreateOutputAsset)]

Oluşturulan çıktı varlığının adını diğer yöntemlere geçirmeniz gerekiyorsa, örneğin `Name` outputAsset.Name' dan döndürülen varlık nesnesindeki özelliği kullandığınızdan `CreateIOutputAssetAsync`emin olun. 

Bu makalede, `outputAsset.Name` değeri `SubmitJobAsync` ve `UploadAudioIntoOutputAsset` işlevleri geçmek.

## <a name="create-a-transform-and-a-job-that-encodes-the-uploaded-file"></a>Yüklenen dosyayı kodlayan bir dönüşüm ve iş oluşturma

Media Services’te içerik kodlarken veya işlerken, kodlama ayarlarını bir tarif olarak ayarlamak yaygın bir modeldir. Daha sonra bu tarifi bir videoya uygulamak üzere bir **İş** gönderirsiniz. Her yeni video için yeni iş göndererek, bu tarifi kitaplığınızdaki tüm videolara uyguluyorsunuz. Media Services içinde tarif, **Dönüşüm** olarak adlandırılır. Daha fazla bilgi için [Dönüşümler ve İşler'e](transform-concept.md)bakın. Bu öğreticide açıklanan örnek, videoyu çeşitli iOS ve Android cihazlarına akışla aktarmak için kodlayan bir tarifi tanımlar. 

Aşağıdaki örnek bir dönüştürme oluşturur (varsa).

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/UploadEncodeAndStreamFiles/Program.cs#EnsureTransformExists)]

Aşağıdaki işlev bir iş gönderir.

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/UploadEncodeAndStreamFiles/Program.cs#SubmitJob)]

## <a name="wait-for-the-job-to-complete"></a>İşin tamamlanmasını bekleyin

İşin tamamlanması biraz sürüyor ve tamamlandığında bildirim almak istiyorsunuz. İşin tamamlanmasını beklemek için Olay Izgarasını kullanmanızı öneririz.

İş genellikle aşağıdaki durumlardan geçer: **Zamanlanmış**, **Sıralı**, **İşleme**, **Bitmiş** (son durum). İş bir hatayla karşılaştıysa **Hata** durumunu alırsınız. İş iptal edilme sürecindeyse **İptal Ediliyor** ve **İptal Edildi** durumunu alırsınız.

Daha fazla bilgi için olay [kılavuzlarını işleme konusuna](reacting-to-media-services-events.md)bakın.

## <a name="upload-the-audio-only-mp4-file"></a>Yalnızca sese özel MP4 dosyasını yükleme

Çıkış varlığına açıklayıcı ses içeren yalnızca sese özel MP4 dosyasını (AAC codec) yükleyin.  

```csharp
private static async Task UpoadAudioIntoOutputAsset(
    IAzureMediaServicesClient client,
    string resourceGroupName,
    string accountName,
    string outputAssetName,
    string fileToUpload)
{
    // Use the Assets.Get method to get the existing asset. 
    // In Media Services v3, the Get method on entities returns null 
    // if the entity doesn't exist (a case-insensitive check on the name).

    // Call Media Services API to create an Asset.
    // This method creates a container in storage for the Asset.
    // The files (blobs) associated with the asset will be stored in this container.
    Asset asset = await client.Assets.GetAsync(resourceGroupName, accountName, outputAssetName);
    
    if (asset != null)
    {
      // Use Media Services API to get back a response that contains
      // SAS URL for the Asset container into which to upload blobs.
      // That is where you would specify read-write permissions 
      // and the exparation time for the SAS URL.
      var response = await client.Assets.ListContainerSasAsync(
          resourceGroupName,
          accountName,
          outputAssetName,
          permissions: AssetContainerPermission.ReadWrite,
          expiryTime: DateTime.UtcNow.AddHours(4).ToUniversalTime());

      var sasUri = new Uri(response.AssetContainerSasUrls.First());

      // Use Storage API to get a reference to the Asset container
      // that was created by calling Asset's CreateOrUpdate method.  
      CloudBlobContainer container = new CloudBlobContainer(sasUri);
      var blob = container.GetBlockBlobReference(Path.GetFileName(fileToUpload));

      // Use Strorage API to upload the file into the container in storage.
      await blob.UploadFromFileAsync(fileToUpload);
    }
}
```

Burada `UpoadAudioIntoOutputAsset` işlev için bir çağrı örneği:

```csharp
await UpoadAudioIntoOutputAsset(client, config.ResourceGroup, config.AccountName, outputAsset.Name, "audio_description.m4a");
```

## <a name="edit-the-ism-file"></a>.ism dosyasını edin

Kodlama işiniz bittiğinde, çıktı kıymeti kodlama işi tarafından oluşturulan dosyaları içerir. 

1. Azure portalında, Medya Hizmetleri hesabınızla ilişkili depolama hesabına gidin. 
1. Çıktı kıymetinizin adını içeren kapsayıcıyı bulun. 
1. Kapsayıcıda .ism dosyasını bulun ve (sağ pencerede) **blob'u düzelt'i** tıklatın. 
1. Açıklayıcı ses içeren yüklenen yalnızca ses eit MP4 dosyası (AAC codec) hakkındaki bilgileri ekleyerek .ism dosyasını düzenleme ve bittiğinde **Kaydet** tuşuna basın.

    Tanımlayıcı ses parçalarını işaret etmek için .ism dosyasına "erişilebilirlik" ve "rol" parametreleri eklemeniz gerekir. Bir ses parçasına ses açıklaması olarak sinyal vermek için bu parametreleri doğru şekilde ayarlamak sizin sorumluluğunuzdadır. Örneğin, aşağıdaki `<param name="accessibility" value="description" />` `<param name="role" value="alternate" />` örnekte gösterildiği gibi, belirli bir ses parçası için .ism dosyasına ekleyin ve .ism dosyasına ekleyin.
 
```xml
<?xml version="1.0" encoding="utf-8"?>
<smil xmlns="http://www.w3.org/2001/SMIL20/Language">
  <head>
    <meta name="clientManifestRelativePath" content="ignite.ismc" />
    <meta name="formats" content="mp4-v3" />
  </head>
  <body>
    <switch>
      <audio src="ignite_320x180_AACAudio_381.mp4" systemBitrate="128041" systemLanguage="eng">
        <param name="systemBitrate" value="128041" valuetype="data" />
        <param name="trackID" value="2" valuetype="data" />
        <param name="trackName" value="aac_eng_2_128041_2_1" valuetype="data" />
        <param name="systemLanguage" value="eng" valuetype="data" />
        <param name="trackIndex" value="ignite_320x180_AACAudio_381_2.mpi" valuetype="data" />
      </audio>
      <audio src="audio_description.m4a" systemBitrate="194000" systemLanguage="eng">
        <param name="trackName" value="aac_eng_audio_description" />
        <param name="accessibility" value="description" />
        <param name="role" value="alternate" />     
      </audio>          
      <video src="ignite_1280x720_AACAudio_3549.mp4" systemBitrate="3549855">
        <param name="systemBitrate" value="3549855" valuetype="data" />
        <param name="trackID" value="1" valuetype="data" />
        <param name="trackName" value="video" valuetype="data" />
        <param name="trackIndex" value="ignite_1280x720_AACAudio_3549_1.mpi" valuetype="data" />
      </video>
      <video src="ignite_960x540_AACAudio_2216.mp4" systemBitrate="2216764">
        <param name="systemBitrate" value="2216764" valuetype="data" />
        <param name="trackID" value="1" valuetype="data" />
        <param name="trackName" value="video" valuetype="data" />
        <param name="trackIndex" value="ignite_960x540_AACAudio_2216_1.mpi" valuetype="data" />
      </video>
      <video src="ignite_640x360_AACAudio_1154.mp4" systemBitrate="1154569">
        <param name="systemBitrate" value="1154569" valuetype="data" />
        <param name="trackID" value="1" valuetype="data" />
        <param name="trackName" value="video" valuetype="data" />
        <param name="trackIndex" value="ignite_640x360_AACAudio_1154_1.mpi" valuetype="data" />
      </video>
      <video src="ignite_480x270_AACAudio_721.mp4" systemBitrate="721893">
        <param name="systemBitrate" value="721893" valuetype="data" />
        <param name="trackID" value="1" valuetype="data" />
        <param name="trackName" value="video" valuetype="data" />
        <param name="trackIndex" value="ignite_480x270_AACAudio_721_1.mpi" valuetype="data" />
      </video>
      <video src="ignite_320x180_AACAudio_381.mp4" systemBitrate="381027">
        <param name="systemBitrate" value="381027" valuetype="data" />
        <param name="trackID" value="1" valuetype="data" />
        <param name="trackName" value="video" valuetype="data" />
        <param name="trackIndex" value="ignite_320x180_AACAudio_381_1.mpi" valuetype="data" />
      </video>
    </switch>
  </body>
</smil>
```

## <a name="get-a-streaming-locator"></a>Akış bulucusu alın

Kodlama tamamlandıktan sonra sıradaki adım, çıktı Varlığındaki videoyu yürütmek için istemcilerin kullanımına sunmaktır. Bunu iki adımda gerçekleştirebilirsiniz: birincisi, [bir Akış Bulucu](https://docs.microsoft.com/rest/api/media/streaminglocators)oluşturun ve ikincisi, istemcilerin kullanabileceği akış URL'lerini oluşturun. 

**Akış Lı Bulucu** oluşturma işlemine yayımlama denir. Varsayılan olarak, **Akış Bulucu,** API aramalarını yaptıktan hemen sonra geçerlidir ve isteğe bağlı başlangıç ve bitiş saatlerini yapılandırmadığınız sürece silinene kadar sürer. 

Bir [StreamingLocator](https://docs.microsoft.com/rest/api/media/streaminglocators) oluştururken istenen **StreamingPolicyName** değerini belirtmeniz gerekir. Bu örnekte, temiz (şifrelenmemiş) içeriğin akışını yapacağınız için önceden tanımlı temiz akış ilkesi **PredefinedStreamingPolicy.ClearStreamingOnly** kullanılır.

> [!IMPORTANT]
> Özel bir [Akış İlkesi](https://docs.microsoft.com/rest/api/media/streamingpolicies)kullanırken, Medya Hizmeti hesabınız için sınırlı sayıda bu tür ilke ler tasarlamalı ve aynı şifreleme seçenekleri ve protokolleri gerektiğinde Bunları StreamingLocators'Larınız için yeniden kullanmalısınız. Medya Hizmeti hesabınızda Akış İlkesi girişi sayısı için bir kota vardır. Her Akış Bulucu için yeni bir Akış İlkesi oluşturmamalısınız.

Aşağıdaki kod, benzersiz bir locatorName ile işlevi çağırdığınızı varsayar.

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/UploadEncodeAndStreamFiles/Program.cs#CreateStreamingLocator)]

Bu konudaki örnek akıştan bahsederken, aşamalı indirme yoluyla video sunmak için bir Akış Bulucu oluşturmak için aynı aramayı kullanabilirsiniz.

### <a name="get-streaming-urls"></a>Akış URL'leri alma

[Artık Akış Bulucu](https://docs.microsoft.com/rest/api/media/streaminglocators) oluşturulduğuna göre, **GetStreamingURLs'da**gösterildiği gibi akış URL'lerini alabilirsiniz. URL oluşturmak [için, Akış Uç Nokta](https://docs.microsoft.com/rest/api/media/streamingendpoints) ana bilgisayar adını ve **Akış Lı Konum belirleme** yolunu oluşturmanız gerekir. Bu örnekte, *varsayılan* **Akış Bitiş Noktası** kullanılır. Bir Medya Hizmeti hesabı ilk oluşturduğunuzda, bu *varsayılan* **Akış Bitiş Noktası** durdurulmuş durumda olacaktır, bu nedenle **Başlat'ı**aramanız gerekir.

> [!NOTE]
> Bu yöntemde, çıkış Varlığı için **Akış Bulucu'yu** oluştururken kullanılan yer bulucu Adı gerekir.

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/UploadEncodeAndStreamFiles/Program.cs#GetStreamingURLs)]

## <a name="test-with-azure-media-player"></a>Azure Media Player ile test etme

Bu makalede, akışı test etmek için Azure Media Player kullanılmaktadır. 

> [!NOTE]
> Oynatıcı bir https sitesinde barındırılıyorsa, "https" URL’sini güncelleştirdiğinizden emin olun.

1. Bir web tarayıcısı [https://aka.ms/azuremediaplayer/](https://aka.ms/azuremediaplayer/)açın ve 'ye gidin.
2. **URL'ye:** kutu, uygulamanızdan aldığınız akışlı URL değerlerinden birini yapıştırın. 
 
     URL'yi HLS, Dash veya Smooth biçiminde yapıştırabilirsiniz ve Azure Media Player cihazınızda otomatik olarak oynatılmak üzere uygun bir akış protokolüne geçer.
3. **Oynatıcıyı Güncelleştir** düğmesine basın.

Azure Media Player, test için kullanılabilir, ancak üretim ortamında kullanılmamalıdır. 

## <a name="next-steps"></a>Sonraki adımlar

[Videoları analiz etme](analyze-videos-tutorial-with-api.md)
