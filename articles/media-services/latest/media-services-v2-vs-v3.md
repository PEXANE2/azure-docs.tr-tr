---
title: Azure Media Services V2 'den v3 'e geçiş
description: Bu makalede, Azure Media Services v3 sürümünde tanıtılan ve iki sürüm arasındaki farklılıklar gösteren değişiklikler açıklanmaktadır.
services: media-services
documentationcenter: na
author: Juliako
manager: femila
editor: ''
tags: ''
keywords: azure media services, akış, yayın, canlı, çevrimdışı
ms.service: media-services
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: media
ms.date: 03/09/2020
ms.author: juliako
ms.openlocfilehash: 091a5d33e49e2abe811bf3cc250d04d69506165d
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87011643"
---
# <a name="media-services-v2-vs-v3"></a>Media Services V2 ve v3 karşılaştırması

Bu makalede, Azure Media Services v3 sürümünde tanıtılan ve iki sürüm arasındaki farklılıklar gösteren değişiklikler açıklanmaktadır.

## <a name="general-changes-from-v2"></a>V2 'den genel değişiklikler

* V3 ile oluşturulan varlıklar için Media Services yalnızca [Azure Storage sunucu tarafı depolama şifrelemesini](../../storage/common/storage-service-encryption.md)destekler.
    * V3 API 'Leri, Media Services tarafından sağlanmış [depolama şifrelemesi](../previous/media-services-rest-storage-encryption.md) (AES 256) olan v2 API 'Leriyle oluşturulmuş varlıklarla birlikte kullanabilirsiniz.
    * V3 API 'Leri kullanarak eski AES 256 [depolama şifrelemesi](../previous/media-services-rest-storage-encryption.md) Ile yeni varlıklar oluşturamazsınız.
* [Varlıkların](assets-concept.md)v3 'deki özellikleri v2 'den farklıdır, bkz. [Özellikler nasıl eşlenir](#map-v3-asset-properties-to-v2).
* V3 SDK 'Ları artık, kullanmak istediğiniz depolama SDK 'Sı üzerinde daha fazla denetim sağlayan ve sürüm oluşturma sorunlarını önleyip depolama SDK 'sının içine ayrılır. 
* V3 API 'Lerinde, tüm kodlama bit hızları saniyede bit/saniye cinsinden. Bu, v2 Media Encoder Standard önayarlarından farklıdır. Örneğin, v2 'deki bit hızı 128 (Kbps) olarak belirtilir, ancak v3 'de 128000 (bit/saniye) olur. 
* AssetFiles, AccessPolicies ve IngestManifests varlıkları v3 'te bulunmamaktadır.
* Ivarlık. ParentAssets özelliği v3 içinde yok.
* ContentKeys artık bir varlık değil, artık akış bulucunun bir özelliğidir.
* Event Grid desteği, NotificationEndpoints yerini alır.
* Aşağıdaki varlıklar yeniden adlandırıldı
    * İş çıkışı görevin yerini alır ve artık bir Işin parçasıdır.
    * Akış Bulucu Konumlandırıcı 'nın yerini alır.
    * Canlı olay kanal yerini alır.<br/>Canlı olaylar, canlı kanal ölçümlerine göre faturalandırılır. Daha fazla bilgi için bkz. [faturalandırma](live-event-states-billing.md) ve [fiyatlandırma](https://azure.microsoft.com/pricing/details/media-services/).
    * Canlı çıkış programın yerini alır.
* Canlı çıktılar oluşturma sırasında başlar ve silindiğinde durdurulur. Programlar v2 API 'Lerinde farklı çalıştık, oluşturulduktan sonra başlatılmaları gerekiyordu.
* Bir iş hakkında bilgi almak için işin altında oluşturulduğu dönüştürme adını bilmeniz gerekir. 
* V2 'de XML [giriş](../previous/media-services-input-metadata-schema.md) ve [Çıkış](../previous/media-services-output-metadata-schema.md) meta verileri dosyaları, bir kodlama işinin sonucu olarak üretilir. V3 'de meta veri biçimi XML olarak JSON olarak değiştirilmiştir. 
* Media Services V2 'de, başlatma vektörü (IV) belirtilebilir. Media Services v3 'de, FairPlay IV belirtilemez. Hem paketleme hem de lisans teslimi için Media Services kullanan müşterileri etkilemediğinden, FairPlay lisanslarını (karma mod) sunmak için üçüncü taraf DRM sistemi kullanılırken bir sorun olabilir. Bu durumda, FairPlay IV 'nin cibh anahtar KIMLIĞINDEN türetildiğinden ve şu formül kullanılarak alınabileceğinizi bilmemiz önemlidir:

    ```
    string cbcsIV =  Convert.ToBase64String(HexStringToByteArray(cbcsGuid.ToString().Replace("-", string.Empty)));
    ```

    örneklerini şununla değiştirin:

    ``` 
    public static byte[] HexStringToByteArray(string hex)
    {
        return Enumerable.Range(0, hex.Length)
            .Where(x => x % 2 == 0)
            .Select(x => Convert.ToByte(hex.Substring(x, 2), 16))
            .ToArray();
    }
    ```

    Daha fazla bilgi için bkz. [hem canlı hem de VOD işlemleri için karma modda Media Services v3 Için Azure Işlevleri C# kodu](https://github.com/Azure-Samples/media-services-v3-dotnet-core-functions-integration/tree/master/LiveAndVodDRMOperationsV3).
 
> [!NOTE]
> [Media Services v3 kaynaklarına](media-services-apis-overview.md#naming-conventions)uygulanan adlandırma kurallarını gözden geçirin. Ayrıca [adlandırma bloblarını](assets-concept.md#naming)gözden geçirin.

## <a name="feature-gaps-with-respect-to-v2-apis"></a>v2 API'leri ile özellik farkları

V3 API 'si, v2 API 'sine göre aşağıdaki özellik boşluklarını içerir. Boşlukların kapatılması devam ediyor.

* [Premium kodlayıcı](../previous/media-services-premium-workflow-encoder-formats.md) ve eski [Medya analizi Işlemcileri](../previous/media-services-analytics-overview.md) (Azure Media Services Dizin Oluşturucu 2 Preview, yüz Redactor vb.) v3 aracılığıyla erişilemez.<br/>Media Indexer 1 veya 2 Preview 'dan geçiş yapmak isteyen müşteriler, v3 API 'sindeki AudioAnalyzer ön ayarını hemen kullanabilir.  Bu yeni önayar, eski Media Indexer 1 veya 2 ' den daha fazla özellik içeriyor. 
* V2 API 'lerinde [Media Encoder Standard gelişmiş özelliklerin](../previous/media-services-advanced-encoding-with-mes.md) birçoğu şu anda v3 'de bulunmamaktadır:
  
    * Kıymetlerin dikiş
    * Kaplayan
    * Manın
    * Küçük resim Sprites
    * Girişte ses olmadığında sessiz ses izi ekleme
    * Girişte video yoksa video izlemesi ekleme
* Transkodlamaya sahip canlı olaylar şu anda kurşun ekleme orta akış ve ad işaretçisi ekleme işlemini API çağrısı aracılığıyla desteklemez. 
 
## <a name="asset-specific-changes"></a>Varlığa özgü değişiklikler

### <a name="map-v3-asset-properties-to-v2"></a>V3 varlık özelliklerini v2 'ye eşleyin

Aşağıdaki tabloda, [varlıkların](/rest/api/media/assets/createorupdate#asset)sürüm 2 ' deki varlık özelliklerinin v2 'de nasıl eşlenme şekli gösterilmektedir.

|V3 Özellikleri|v2 özellikleri|
|---|---|
|`id`-(benzersiz) tam Azure Resource Manager yolu, bkz. [varlık](/rest/api/media/assets/createorupdate) içindeki örnekler||
|`name`-(benzersiz) bkz. [adlandırma kuralları](media-services-apis-overview.md#naming-conventions) ||
|`alternateId`|`AlternateId`|
|`assetId`|`Id`-(unique) değeri `nb:cid:UUID:` önekiyle başlar.|
|`created`|`Created`|
|`description`|`Name`|
|`lastModified`|`LastModified`|
|`storageAccountName`|`StorageAccountName`|
|`storageEncryptionFormat`| `Options`(oluşturma seçenekleri)|
|`type`||

### <a name="storage-side-encryption"></a>Depolama tarafı şifrelemesi

Varlıkları bekleyen bir şekilde korumak için, varlıkların depolama tarafı şifrelemesi tarafından şifrelenmesi gerekir. Aşağıdaki tabloda, depolama tarafı şifrelemesinin Media Services ' de nasıl çalıştığı gösterilmektedir:

|Şifreleme seçeneği|Açıklama|Media Services v2|Media Services v3|
|---|---|---|---|
|Media Services depolama şifrelemesi|AES-256 şifrelemesi, anahtar Media Services tarafından yönetiliyor.|Desteklenen<sup>(1)</sup>|Desteklenmiyor<sup>(2)</sup>|
|[Bekleyen veriler için Depolama Hizmeti Şifrelemesi](../../storage/common/storage-service-encryption.md)|Azure depolama tarafından sunulan ve Azure tarafından yönetilen veya müşteri tarafından yönetilen sunucu tarafı şifrelemesi.|Desteklenir|Desteklenir|
|[Depolama Istemci tarafı şifrelemesi](../../storage/common/storage-client-side-encryption.md)|Azure depolama tarafından sunulan ve Key Vault ' de müşteri tarafından yönetilen anahtar olan istemci tarafı şifreleme.|Desteklenmez|Desteklenmez|

<sup>1</sup> Media Services hiçbir şifreleme formu olmadan açık/olmayan içerik işlemeyi desteklese de, bunu yapmanız önerilmez.

<sup>2</sup> Media Services v3 'de, depolama ŞIFRELEMESI (AES-256 şifrelemesi) yalnızca varlıklarınız Media Services V2 ile oluşturulduysa geriye dönük uyumluluk için desteklenir. V3, mevcut depolama şifrelenmiş varlıklarla birlikte çalışarak, yenilerini oluşturulmasına izin vermez.

## <a name="code-differences"></a>Kod farklılıkları

Aşağıdaki tabloda yaygın senaryolar için v2 ve v3 arasındaki kod farklılıkları gösterilmektedir.

|Senaryo|V2 APı 'SI|V3 APı 'SI|
|---|---|---|
|Bir varlık oluşturun ve bir dosyayı karşıya yükleyin |[v2 .NET örneği](https://github.com/Azure-Samples/media-services-dotnet-dynamic-encryption-with-aes/blob/master/DynamicEncryptionWithAES/DynamicEncryptionWithAES/Program.cs#L113)|[v3 .NET örneği](https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials/blob/master/AMSV3Tutorials/UploadEncodeAndStreamFiles/Program.cs#L169)|
|İş gönder|[v2 .NET örneği](https://github.com/Azure-Samples/media-services-dotnet-dynamic-encryption-with-aes/blob/master/DynamicEncryptionWithAES/DynamicEncryptionWithAES/Program.cs#L146)|[v3 .NET örneği](https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials/blob/master/AMSV3Tutorials/UploadEncodeAndStreamFiles/Program.cs#L298)<br/><br/>İlk olarak bir dönüştürme oluşturmayı ve sonra bir Işi göndermeyi gösterir.|
|AES şifrelemesi ile varlık yayımlama |1. ContentKeyAuthorizationPolicyOption oluşturun<br/>2. ContentKeyAuthorizationPolicy oluşturma<br/>3. AssetDeliveryPolicy oluşturma<br/>4. varlık oluşturma ve içerik yükleme veya işi gönderme ve çıkış varlığını kullanma<br/>5. AssetDeliveryPolicy 'i varlıkla ilişkilendir<br/>6. ContentKey oluştur<br/>7. varlığa ContentKey iliştirme<br/>8. AccessPolicy oluşturma<br/>9. Bulucu oluştur<br/><br/>[v2 .NET örneği](https://github.com/Azure-Samples/media-services-dotnet-dynamic-encryption-with-aes/blob/master/DynamicEncryptionWithAES/DynamicEncryptionWithAES/Program.cs#L64)|1. Içerik anahtar Ilkesi oluştur<br/>2. varlık oluştur<br/>3. içeriği karşıya yükleyin veya varlığı Joi put olarak kullanın<br/>4. akış Bulucu oluşturma<br/><br/>[v3 .NET örneği](https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials/blob/master/AMSV3Tutorials/EncryptWithAES/Program.cs#L105)|
|İş ayrıntılarını alma ve işleri yönetme |[V2 ile işleri yönetme](../previous/media-services-dotnet-manage-entities.md#get-a-job-reference) |[V3 ile işleri yönetme](https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials/blob/master/AMSV3Tutorials/UploadEncodeAndStreamFiles/Program.cs#L546)|

> [!NOTE]
> Lütfen bu makaleye yer işareti ekleyin ve güncelleştirmeleri kontrol edin.

## <a name="ask-questions-give-feedback-get-updates"></a>Soru sorun, geri bildirimde bulunun, güncelleştirmeleri al

Soru sormak, geri bildirimde bulunmak ve Media Services hakkında güncelleştirmeler almak için [Azure Media Services Community](media-services-community.md) makalesine göz atın.

## <a name="next-steps"></a>Sonraki adımlar

[Media Services V2 'den v3 'e geçmek için geçiş kılavuzu](migrate-from-v2-to-v3.md)
