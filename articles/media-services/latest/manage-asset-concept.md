---
title: Azure Medya Hizmetleri'ndeki varlıkları yönetme
titleSuffix: Azure Media Services
description: Medya girişi yaptığınız bir varlık (örneğin, yükleme veya canlı yutma yoluyla), çıktı ortamı (bir iş çıktısından) ve (akış için) medya yayımlama. Bu konu, yeni bir varlığın nasıl oluşturulup dosyaların yükleneceğine genel bir bakış sağlar.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 03/26/2020
ms.author: juliako
ms.custom: seodec18
ms.openlocfilehash: 9136fd702fad5c12a8ec97a68ff8a592a203d7d2
ms.sourcegitcommit: 3c318f6c2a46e0d062a725d88cc8eb2d3fa2f96a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/02/2020
ms.locfileid: "80582207"
---
# <a name="manage-assets"></a>Varlıkları yönetme

Azure Medya Hizmetleri'nde, bir [Varlık](https://docs.microsoft.com/rest/api/media/assets) 

* medya dosyalarını bir varlığa yüklemek,
* canlı akışları bir varlığa sindirin ve arşivleyin,
* analitik işin bir varlığa kodlanması nın sonuçlarını çıktı,
* akış için medya yayınlamak, 
* bir varlıktan dosya indirin.

Bu konu, bir varlığa dosya yükleme ve diğer bazı ortak işlemleri gerçekleştirmek için nasıl genel bir bakış sağlar. Ayrıca kod örnekleri ve ilgili konulara bağlantılar sağlar.

## <a name="prerequisite"></a>Önkoşul 

Geliştirmeye başlamadan önce şunları gözden geçirin:

* [Kavramlar](concepts-overview.md)
* [Media Services v3 API'leri ile geliştirme](media-services-apis-overview.md) (API'lere erişim, adlandırma kuralları vb. hakkında bilgi içerir) 

## <a name="upload-media-files-into-an-asset"></a>Medya dosyalarını bir varlığa yükleme

Dijital dosyalar depolama alanına yüklendikten ve bir Varlıkla ilişkilendirildikten sonra, medya hizmetlerinin kodlanmasında, akışında ve içerik iş akışlarında kullanılabilir. Ortak Medya Hizmetleri iş akışlarından biri, bir dosyayı yüklemek, kodlamak ve akışlamaktır. Bu bölümde genel adımlar özetler.

1. Media Services v3 API'sini kullanarak yeni bir "input" Varlığı oluşturun. Bu işlem, Media Services hesabınızla ilişkilendirilen depolama hesabında bir kapsayıcı oluşturur. API kapsayıcı adını döndürür `"container": "asset-b8d8b68a-2d7f-4d8c-81bb-8c7bbbe67ee4"`(örneğin,).

    Bir Varlıkla ilişkilendirmek istediğiniz bir blob kapsayıcınız varsa, Kıymeti oluştururken kapsayıcı adını belirtebilirsiniz. Media Services şu anda blobları yalnızca kapsayıcı kökünde destekler ve dosya adında yol kullanılmasını desteklemez. Bu nedenle "input.mp4" gibi bir dosya adına sahip kapsayıcıları kullanabilirsiniz. Ancak, "videolar/girişler/input.mp4" dosya adı içeren bir kapsayıcı çalışmaz.

    Azure CLI'yi kullanarak aboneliğinizde gerekli izinlere sahip olduğunuz tüm depolama hesaplarına ve kapsayıcılara içerik yükleyebilirsiniz.

    Kapsayıcı adının benzersiz ve depolama adlandırma kurallarına uygun olması gerekir. Ad için Media Services Varlık kapsayıcısı adı (Varlık-GUID) biçimlendirmesinin kullanılması şart değildir.

    ```azurecli
    az storage blob upload -f /path/to/file -c MyContainer -n MyBlob
    ```
2. Dijital dosyaları Varlık kapsayıcısına yüklemek için kullanmak üzere okuma-yazma izinlerine sahip bir SAS URL'si alın.

    Media Services API'sini kullanarak [varlık kapsayıcısı URL'lerini listeleyebilirsiniz](https://docs.microsoft.com/rest/api/media/assets/listcontainersas).

    **AssetContainerSas.listContainerSas,** üzerinde belirlediğiniz `expiryTime` [ListContainerSasInput](https://docs.microsoft.com/rest/api/media/assets/listcontainersas#listcontainersasinput) parametresini alır. Saat 24 saat < olarak ayarlanmalıdır.

    [ListContainerSasInput,](https://docs.microsoft.com/rest/api/media/assets/listcontainersas#listcontainersasinput) her depolama hesabı için iki depolama hesabı anahtarı olduğundan birden çok SAS URL'si döndürür. Depolama hesabının iki anahtarı vardır, çünkü depolama hesabı anahtarlarının başarısız ve sorunsuz bir şekilde döndürülmesine yardımcı olur. İlk SAS URL ilk depolama hesabı anahtarını, ikinci SAS URL'si ise ikinci anahtarı temsil eder.
3. Varlık kapsayıcısına dosya yüklemek için Azure Depolama API'lerini veya SDK'larını (örneğin, [Storage REST API](../../storage/common/storage-rest-api-auth.md) veya [.NET SDK)](../../storage/blobs/storage-quickstart-blobs-dotnet.md)kullanın.
4. Media Services v3 API'lerini kullanarak "input" Varlığınızı işlemek üzere bir Dönüşüm ve bir İş oluşturun. Daha fazla bilgi için [Dönüşümler ve İşler'e](transform-concept.md)bakın.
5. İçeriği "çıktı" varlığından aktarın.

### <a name="create-a-new-asset"></a>Yeni bir varlık oluşturun

> [!NOTE]
> Bir Varlığın Datetime türündeki özellikleri her zaman UTC biçimindedir.

#### <a name="rest"></a>REST

```
PUT https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Media/mediaServices/{amsAccountName}/assets/{assetName}?api-version=2018-07-01
```

REST örneği için, [REST](https://docs.microsoft.com/rest/api/media/assets/createorupdate#examples) örneğine göre Varlık Oluştur'a bakın.

Örnek, açıklama, kapsayıcı adı, depolama hesabı ve diğer yararlı bilgileri belirtebileceğiniz **İstek Gövdesi'nin** nasıl oluşturulacağını gösterir.

#### <a name="curl"></a>cURL

```cURL
curl -X PUT \
  'https://management.azure.com/subscriptions/00000000-0000-0000-000000000000/resourceGroups/resourceGroupName/providers/Microsoft.Media/mediaServices/amsAccountName/assets/myOutputAsset?api-version=2018-07-01' \
  -H 'Accept: application/json' \
  -H 'Content-Type: application/json' \
  -d '{
  "properties": {
    "description": "",
  }
}'
```

#### <a name="net"></a>.NET

```csharp
 Asset asset = await client.Assets.CreateOrUpdateAsync(resourceGroupName, accountName, assetName, new Asset());
```

### <a name="see-also"></a>Ayrıca bkz.

* [Yerel bir dosyadan iş girişi oluşturma](job-input-from-local-file-how-to.md)
* [HTTPS URL'sinden iş girişi oluşturma](job-input-from-http-how-to.md)

## <a name="ingest-and-archive-live-streams-into-an-asset"></a>Canlı akışları bir varlığa alma ve arşivleme

Medya Hizmetleri'nde, [Canlı Çıkış](https://docs.microsoft.com/rest/api/media/liveoutputs) nesnesi, canlı akışınızı Medya Hizmetleri hesabınızdaki bir varlığa yakalayıp kaydedecek dijital video kaydedici gibidir. Kaydedilen [içerik, Varlık](https://docs.microsoft.com/rest/api/media/assets) kaynağı tarafından tanımlanan kapsayıcıda kalıcıdır.

Daha fazla bilgi için bkz.

* [Bulut DVR kullanma](live-event-cloud-dvr.md)
* [Canlı eğitim akışı](stream-live-tutorial-with-api.md)

## <a name="output-the-results-of-a-job-to-an-asset"></a>Bir işin sonuçlarını bir varlığa çıktı

Medya Hizmetleri'nde, videolarınızı işlerken (örneğin, kodlama veya analiz) [işinizin](transforms-jobs-concept.md)sonucunu depolamak için bir çıktı [varlığı](assets-concept.md) oluşturmanız gerekir.

Daha fazla bilgi için bkz.

* [Bir videoyu kodlama](encoding-concept.md)
* [Yerel bir dosyadan iş girişi oluşturma](job-input-from-local-file-how-to.md)

## <a name="publish-an-asset-for-streaming"></a>Akış için bir varlık yayımlama

Akış için bir varlık yayımlamak için bir [Akış Bulucu](streaming-locators-concept.md)oluşturmanız gerekir. Akış bulucu, yayımlamak istediğiniz varlık adını bilmesi gerekir. 

Daha fazla bilgi için bkz.

[Öğretici: Medya Hizmetleri v3 ile video yükleme, kodlama ve akış](stream-files-tutorial-with-api.md)

## <a name="download-results-of-a-job-from-an-output-asset"></a>Çıktı varlığından bir işin sonuçlarını indirme

Daha sonra Medya Hizmeti ve Depolama API'lerini kullanarak işinizin bu sonuçlarını yerel bir klasöre indirebilirsiniz. 

[İndirme dosyaları](download-results-howto.md) örneğine bakın.

## <a name="filtering-ordering-paging"></a>Filtreleme, sipariş etme, sayfalama

Bkz. [Medya Hizmetleri varlıklarınıfiltreleme, sipariş etme, sayfalama.](entities-overview.md)

## <a name="next-steps"></a>Sonraki adımlar

Nasıl yüklenir, kodlanır, analiz edin, canlı ve isteğe bağlı olarak yayınlanır: 

* [Java](https://docs.microsoft.com/samples/azure-samples/media-services-v3-java/azure-media-services-v3-samples-using-java/), 
* [.NET](https://docs.microsoft.com/samples/azure-samples/media-services-v3-dotnet/azure-media-services-v3-samples-using-net/), 
* [DINLENIN](https://docs.microsoft.com/samples/azure-samples/media-services-v3-rest-postman/azure-media-services-postman-collection/).
