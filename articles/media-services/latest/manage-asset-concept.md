---
title: Azure Media Services varlıkları yönetme
titleSuffix: Azure Media Services
description: Medyayı (örneğin, karşıya yükleme veya canlı alma aracılığıyla), çıkış medyasını (bir iş çıktısından) ve medya Yayımla (akış için). Bu konu, yeni bir varlık oluşturma ve dosyaları karşıya yükleme hakkında genel bakış sunar.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 08/29/2019
ms.author: juliako
ms.custom: seodec18
ms.openlocfilehash: 1e912e8147f83571d073d5456019d043ff983ceb
ms.sourcegitcommit: 3c925b84b5144f3be0a9cd3256d0886df9fa9dc0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/28/2020
ms.locfileid: "77925624"
---
# <a name="manage-assets"></a>Varlıkları yönetme

Azure Media Services, bir [varlık](https://docs.microsoft.com/rest/api/media/assets) nerede 

* medya dosyalarını bir varlığa yükleme,
* Canlı akışları bir varlığa alma ve arşivleme,
* analiz işi kodlamasının bir varlığa ait sonuçlarının çıktısını almak için
* medyayı akış için yayımlama, 
* bir varlığın dosyalarını indirin.

Bu konu, bir varlığa dosya yükleme ve diğer yaygın işlemleri gerçekleştirme hakkında genel bakış sunar. Ayrıca kod örneklerine ve ilgili konulara bağlantılar sağlar.

## <a name="prerequisite"></a>Önkoşul 

Geliştirmeye başlamadan önce şunları gözden geçirin:

* [Kavramlar ](concepts-overview.md)
* [Media Services v3 API 'leri Ile geliştirme](media-services-apis-overview.md) (API 'lere erişme, adlandırma kuralları vb.) 

## <a name="upload-media-files-into-an-asset"></a>Bir varlığa medya dosyaları yükleme

Dijital dosyalar depolama alanına yüklendikten ve bir varlıkla ilişkilendirildikten sonra, Media Services kodlama, akış ve içerik iş akışlarını çözümleme için kullanılabilirler. Ortak Media Services iş akışlarından biri bir dosyayı karşıya yüklemek, kodlamak ve akışa almak. Bu bölümde genel adımlar özetlenmektedir.

1. Media Services v3 API'sini kullanarak yeni bir "input" Varlığı oluşturun. Bu işlem, Media Services hesabınızla ilişkilendirilen depolama hesabında bir kapsayıcı oluşturur. API, kapsayıcı adını döndürür (örneğin, `"container": "asset-b8d8b68a-2d7f-4d8c-81bb-8c7bbbe67ee4"`).

    Bir varlıkla ilişkilendirmek istediğiniz bir blob Kapsayıcınız zaten varsa, varlığı oluştururken kapsayıcı adını belirtebilirsiniz. Media Services şu anda blobları yalnızca kapsayıcı kökünde destekler ve dosya adında yol kullanılmasını desteklemez. Bu nedenle "input.mp4" gibi bir dosya adına sahip kapsayıcıları kullanabilirsiniz. Bununla birlikte, "Videolar/girişler/Input. mp4" dosya adına sahip bir kapsayıcı çalışmaz.

    Azure CLI'yi kullanarak aboneliğinizde gerekli izinlere sahip olduğunuz tüm depolama hesaplarına ve kapsayıcılara içerik yükleyebilirsiniz.

    Kapsayıcı adının benzersiz ve depolama adlandırma kurallarına uygun olması gerekir. Ad için Media Services Varlık kapsayıcısı adı (Varlık-GUID) biçimlendirmesinin kullanılması şart değildir.

    ```azurecli
    az storage blob upload -f /path/to/file -c MyContainer -n MyBlob
    ```
2. Dijital dosyaları Varlık kapsayıcısına yüklemek için kullanmak üzere okuma-yazma izinlerine sahip bir SAS URL'si alın. Media Services API'sini kullanarak [varlık kapsayıcısı URL'lerini listeleyebilirsiniz](https://docs.microsoft.com/rest/api/media/assets/listcontainersas).
3. Dosyaları varlık kapsayıcısına yüklemek için Azure depolama API 'Lerini veya SDK 'Larını (örneğin, [depolama REST API](../../storage/common/storage-rest-api-auth.md) veya [.NET SDK](../../storage/blobs/storage-quickstart-blobs-dotnet.md)) kullanın.
4. Media Services v3 API'lerini kullanarak "input" Varlığınızı işlemek üzere bir Dönüşüm ve bir İş oluşturun. Daha fazla bilgi için [Dönüşümler ve İşler](transform-concept.md) konusuna bakın.
5. "Çıkış" kıymetinin içeriğini akışla.

### <a name="create-a-new-asset"></a>Yeni varlık oluştur

> [!NOTE]
> Bir varlığın tarih saat türünün özellikleri her zaman UTC biçimindedir.

#### <a name="rest"></a>REST

```
PUT https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Media/mediaServices/{amsAccountName}/assets/{assetName}?api-version=2018-07-01
```

REST örneği için bkz. [rest Ile varlık oluşturma](https://docs.microsoft.com/rest/api/media/assets/createorupdate#examples) örneği.

Örnek, açıklama, kapsayıcı adı, depolama hesabı ve diğer yararlı bilgileri belirtebileceğiniz **Istek gövdesinin** nasıl oluşturulacağını gösterir.

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

### <a name="also-see"></a>Ayrıca bkz.

* [Yerel dosyadan iş girişi oluşturma](job-input-from-local-file-how-to.md)
* [HTTPS URL 'sinden iş girişi oluşturma](job-input-from-http-how-to.md)

## <a name="ingest-and-archive-live-streams-into-an-asset"></a>Canlı akışları bir varlığa alma ve arşivleme

Media Services, [canlı çıkış](https://docs.microsoft.com/rest/api/media/liveoutputs) nesnesi canlı akışınızı yakalayıp Media Services hesabınızdaki bir varlığa kaydeden dijital video kaydedicisi gibidir. Kaydedilen içerik, [varlık](https://docs.microsoft.com/rest/api/media/assets) kaynağı tarafından tanımlanan kapsayıcıda kalıcıdır.

Daha fazla bilgi için bkz.

* [Bulut DVR kullanma](live-event-cloud-dvr.md)
* [Canlı akış öğreticisi](stream-live-tutorial-with-api.md)

## <a name="output-the-results-of-a-job-to-an-asset"></a>Bir işin sonuçlarını bir varlığa çıkış

Media Services, videolarınızı işlerken (örneğin, kodlama veya çözümleme), [işinizin](transforms-jobs-concept.md)sonucunu depolamak için bir çıktı [varlığı](assets-concept.md) oluşturmanız gerekir.

Daha fazla bilgi için bkz.

* [Bir videoyu kodlama](encoding-concept.md)
* [Yerel dosyadan iş girişi oluşturma](job-input-from-local-file-how-to.md)

## <a name="publish-an-asset-for-streaming"></a>Bir varlığı akış için yayımlama

Bir varlığı akış için yayımlamak üzere bir [akış Bulucu](streaming-locators-concept.md)oluşturmanız gerekir. Akış bulucunun yayımlamak istediğiniz varlık adını bilmesi gerekir. 

Daha fazla bilgi için bkz.

[Öğretici: Media Services v3 ile videoları karşıya yükleme, kodlama ve akışla](stream-files-tutorial-with-api.md)

## <a name="download-results-of-a-job-from-an-output-asset"></a>Bir iş için bir çıkış varlığı aracılığıyla sonuçları indirin

Daha sonra, medya hizmetini ve depolama API 'Lerini kullanarak işinizin bu sonuçlarını yerel bir klasöre indirebilirsiniz. 

[Dosyaları indir](download-results-howto.md) örneğine bakın.

## <a name="filtering-ordering-paging"></a>Filtreleme, sıralama, sayfalama

Bkz. [Media Services varlıkların filtrelenmesi, sıralanması, sayfalama](entities-overview.md).

## <a name="next-steps"></a>Sonraki adımlar

Nasıl karşıya yükleneceğini, kodlayabileceğinizi, analiz edeceğinizi, canlı ve isteğe bağlı olarak akışı gösteren tam kod örneklerine bakın: 

* [Java](https://docs.microsoft.com/samples/azure-samples/media-services-v3-java/azure-media-services-v3-samples-using-java/), 
* [.Net](https://docs.microsoft.com/samples/azure-samples/media-services-v3-dotnet/azure-media-services-v3-samples-using-net/), 
* [Rest](https://docs.microsoft.com/samples/azure-samples/media-services-v3-rest-postman/azure-media-services-postman-collection/).
