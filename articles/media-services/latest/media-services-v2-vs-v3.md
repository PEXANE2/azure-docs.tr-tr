---
title: Azure Medya Hizmetleri v2'den v3'e geçirin
description: Bu makalede, Azure Media Services v3'te tanıtılan değişiklikler açıklanır ve iki sürüm arasındaki farkları gösterir.
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
ms.openlocfilehash: fd094e35ceaa718ec1b258d74106b39744cbd16f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79087824"
---
# <a name="media-services-v2-vs-v3"></a>Medya Hizmetleri v2 vs v3

Bu makalede, Azure Media Services v3'te tanıtılan değişiklikler açıklanır ve iki sürüm arasındaki farkları gösterir.

## <a name="general-changes-from-v2"></a>v2'den genel değişiklikler

* V3 ile oluşturulan varlıklar için Medya Hizmetleri yalnızca [Azure Depolama sunucu tarafındaki depolama şifrelemesini](https://docs.microsoft.com/azure/storage/common/storage-service-encryption)destekler.
    * Medya Hizmetleri tarafından sağlanan [depolama şifrelemesi](../previous/media-services-rest-storage-encryption.md) (AES 256) ile oluşturulan V2 API'leri ile oluşturulan Varlıklar ile v3 API'lerini kullanabilirsiniz.
    * V3 API'lerini kullanarak eski AES 256 [depolama şifrelemesiyle](../previous/media-services-rest-storage-encryption.md) yeni Varlıklar oluşturamazsınız.
* [Varlık'ın](assets-concept.md)v3'teki özellikleri v2'den farklıdır, [özelliklerin nasıl eşlenebildiğini](#map-v3-asset-properties-to-v2)görün.
* V3 SDK'lar artık Depolama SDK'sından ayrılmış, bu da size kullanmak istediğiniz Storage SDK sürümü üzerinde daha fazla denetim sağlar ve sürüm sorunlarını önler. 
* v3 API'lerinde, tüm kodlama bit hızları saniyede bit şeklindedir. Bu, v2 Media Encoder Standart ön ayarlardan farklıdır. Örneğin, v2'deki bit hızı 128 (kbps) olarak belirtilir, v3'te ise 128000 (bit/saniye) olur. 
* Varlıklar Varlık Dosyaları, AccessPolicies ve IngestManifests v3 yok.
* IAsset.ParentAssets özelliği v3'te yok.
* ContentKeys artık bir varlık değil, artık Akış Bulucu'nun bir özelliğidir.
* Olay Grid desteği BildirimSon Noktaları'nın yerini alır.
* Aşağıdaki varlıklar yeniden adlandırıldı
    * İş Çıktısı Görevin yerini alır ve artık bir İş'in bir parçasıdır.
    * Akış Lı Bulucatör, Konumbelirleyici'nin yerini alır.
    * Live Event, Kanal'ın yerini alır.<br/>Canlı Etkinlikler faturalandırması Canlı Kanal sayaçlarını temel alınarak yapılır. Daha fazla bilgi için faturalandırma ve fiyatlandırma ya da [faturalandırma](live-event-states-billing.md) ya da fiyatlandırma ya da [faturalandırma](https://azure.microsoft.com/pricing/details/media-services/)ya da fiyatlandırma ya da
    * Canlı Çıktı Programı değiştirir.
* Canlı Çıktılar oluşturmayla başlar ve silindiğinde durur. Programlar v2 API'lerinde farklı çalıştı, oluşturulduktan sonra başlatılması gerekiyordu.
* Bir iş hakkında bilgi almak için, işin oluşturulduğu Dönüşüm adını bilmeniz gerekir. 
* V2'de, XML [giriş](../previous/media-services-input-metadata-schema.md) ve [çıktı](../previous/media-services-output-metadata-schema.md) meta veri dosyaları bir kodlama işi sonucunda oluşturulur. V3'te meta veri biçimi XML'den JSON'a değiştirildi. 
* Medya Hizmetleri v2'de, başlatma vektörü (IV) belirtilebilir. Medya Hizmetleri v3'te FairPlay IV belirtilemez. Hem paketleme hem de lisans teslimatı için Medya Hizmetlerini kullanan müşterileri etkilemese de, FairPlay lisanslarını (karma mod) sunmak için üçüncü taraf bir DRM sistemini kullanırken sorun olabilir. Bu durumda, FairPlay IV'ün cbcs anahtar kimliğinden türetilmiş olduğunu ve bu formül kullanılarak alınabileceğini bilmek önemlidir:

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

    Daha fazla bilgi [için, Hem Canlı hem de VOD işlemleri için karma modda Medya Hizmetleri v3 için Azure İşlevleri C# koduna](https://github.com/Azure-Samples/media-services-v3-dotnet-core-functions-integration/tree/master/LiveAndVodDRMOperationsV3)bakın.
 
> [!NOTE]
> [Medya Hizmetleri v3 kaynaklarına](media-services-apis-overview.md#naming-conventions)uygulanan adlandırma kurallarını gözden geçirin. Ayrıca [arama lekeleri gözden geçirin.](assets-concept.md#naming)

## <a name="feature-gaps-with-respect-to-v2-apis"></a>v2 API'leri ile özellik farkları

V3 API v2 API ile ilgili olarak aşağıdaki özellik boşlukları vardır. Boşlukları kapatma çalışmaları devam ediyor.

* [Premium Kodlayıcı](../previous/media-services-premium-workflow-encoder-formats.md) ve eski [medya analiz işlemcilerine](../previous/media-services-analytics-overview.md) (Azure Media Services Indexer 2 Preview, Face Redactor, vb.) v3 üzerinden erişilemez.<br/>Media Indexer 1 veya 2 önizlemesinden geçiş yapmak isteyen müşteriler, v3 API'deki AudioAnalyzer ön ayarını hemen kullanabilir.  Bu yeni ön ayar, eski Media Indexer 1 veya 2'den daha fazla özellik içerir. 
* [V2 API'lerinde Media Encoder Standard'ın gelişmiş özelliklerinin](../previous/media-services-advanced-encoding-with-mes.md) çoğu şu anda v3'te mevcut değildir:
  
    * Varlıkların Dikişi
    * Bindirmeler
    * Kırpma
    * Küçük Resim Sprites
    * Girişte ses yokken sessiz bir ses parçası ekleme
    * Girişte video yokken video parçası ekleme
* Şu anda transcoding içeren Canlı Etkinlikler, API çağrısı yla Slate eklemenin orta akışı ve reklam işaretçisi eklemeyi desteklemez. 
 
## <a name="asset-specific-changes"></a>Varlığa özgü değişiklikler

### <a name="map-v3-asset-properties-to-v2"></a>V3 varlık özelliklerini v2'ye göre harita

Aşağıdaki tablo, [Varlığın](https://docs.microsoft.com/rest/api/media/assets/createorupdate#asset)v3 haritasındaki özelliklerinin V2'deki Varlıkların özelliklerine nasıl olduğunu gösterir.

|v3 özellikleri|v2 özellikleri|
|---|---|
|`id`- (benzersiz) tam Azure Kaynak Yöneticisi yolu, [Varlık](https://docs.microsoft.com/rest/api/media/assets/createorupdate) örneklerine bakın||
|`name`- (benzersiz) [bkz.](media-services-apis-overview.md#naming-conventions) ||
|`alternateId`|`AlternateId`|
|`assetId`|`Id`- (benzersiz) değer `nb:cid:UUID:` önek ile başlar.|
|`created`|`Created`|
|`description`|`Name`|
|`lastModified`|`LastModified`|
|`storageAccountName`|`StorageAccountName`|
|`storageEncryptionFormat`| `Options`(oluşturma seçenekleri)|
|`type`||

### <a name="storage-side-encryption"></a>Depolama tarafı şifreleme

Varlıklarınızı istirahatte korumak için, varlıkların depolama tarafındaki şifreleme tarafından şifrelenmeleri gerekir. Aşağıdaki tablo, Depolama tarafı şifrelemesinin Medya Hizmetleri'nde nasıl çalıştığını gösterir:

|Şifreleme seçeneği|Açıklama|Media Services v2|Media Services v3|
|---|---|---|---|
|Medya Hizmetleri Depolama Şifrelemesi|AES-256 şifreleme, Medya Hizmetleri tarafından yönetilen anahtar.|Desteklenen<sup>(1)</sup>|Desteklenmiyor<sup>(2)</sup>|
|[Veriler için Depolama Hizmeti Şifrelemesi](https://docs.microsoft.com/azure/storage/common/storage-service-encryption)|Azure Depolama tarafından sunulan sunucu tarafı şifrelemesi, anahtar Azure veya müşteri tarafından yönetilir.|Destekleniyor|Destekleniyor|
|[Depolama İstemci Tarafı Şifrelemesi](https://docs.microsoft.com/azure/storage/common/storage-client-side-encryption)|Anahtar Vault'ta müşteri tarafından yönetilen Azure depolama tarafından sunulan istemci tarafı şifrelemesi.|Desteklenmiyor|Desteklenmiyor|

<sup>1</sup> Medya Hizmetleri içeriğin açık/net bir şekilde işlenmesini desteklerken, bunu yapmak önerilmez.

<sup>2</sup> Medya Hizmetleri v3'te depolama şifrelemesi (AES-256 şifrelemesi), yalnızca Varlıklarınız Medya Hizmetleri v2 ile oluşturulduğunda geriye dönük uyumluluk için desteklenir. Yani v3 varolan depolama şifreli varlıklarla çalışır, ancak yenilerinin oluşturulmasına izin vermez.

## <a name="code-differences"></a>Kod farklılıkları

Aşağıdaki tablo, sık karşılaşılan senaryolar için v2 ve v3 arasındaki kod farklarını gösterir.

|Senaryo|V2 API|V3 API|
|---|---|---|
|Varlık oluşturma ve dosya yükleme |[v2 .NET örneği](https://github.com/Azure-Samples/media-services-dotnet-dynamic-encryption-with-aes/blob/master/DynamicEncryptionWithAES/DynamicEncryptionWithAES/Program.cs#L113)|[v3 .NET örneği](https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials/blob/master/AMSV3Tutorials/UploadEncodeAndStreamFiles/Program.cs#L169)|
|İş gönderme|[v2 .NET örneği](https://github.com/Azure-Samples/media-services-dotnet-dynamic-encryption-with-aes/blob/master/DynamicEncryptionWithAES/DynamicEncryptionWithAES/Program.cs#L146)|[v3 .NET örneği](https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials/blob/master/AMSV3Tutorials/UploadEncodeAndStreamFiles/Program.cs#L298)<br/><br/>Önce Dönüşüm'ün nasıl oluşturulup sonra bir İş gönderilen şekli gösterir.|
|AES şifrelemeli bir varlık yayımlama |1. ContentKeyAuthorizationPolicyOption oluştur<br/>2. ContentKeyAuthorizationPolicy oluşturma<br/>3. Varlık Teslim Politikası Oluşturma<br/>4. Varlık oluşturma ve içerik yükleme VEYA iş gönderme ve çıktı varlığını kullanma<br/>5. Varlık Teslim Politikası'nı Varlıkla Ilişkilendirme<br/>6. ContentKey oluştur<br/>7. İçeriği Kıymete EkleKey<br/>8. AccessPolicy Oluştur<br/>9. Bulucu Oluştur<br/><br/>[v2 .NET örneği](https://github.com/Azure-Samples/media-services-dotnet-dynamic-encryption-with-aes/blob/master/DynamicEncryptionWithAES/DynamicEncryptionWithAES/Program.cs#L64)|1. İçerik Anahtar İlkesi Oluştur<br/>2. Varlık Oluşturma<br/>3. İçeriği yükleme veya Varlığı İş Çıktısı olarak kullanma<br/>4. Akış Bulucu oluştur<br/><br/>[v3 .NET örneği](https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials/blob/master/AMSV3Tutorials/EncryptWithAES/Program.cs#L105)|
|İş ayrıntılarını alın ve işleri yönetin |[v2 ile işleri yönetme](../previous/media-services-dotnet-manage-entities.md#get-a-job-reference) |[v3 ile işleri yönetme](https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials/blob/master/AMSV3Tutorials/UploadEncodeAndStreamFiles/Program.cs#L546)|

> [!NOTE]
> Lütfen bu makaleyi yer imi ve güncellemeleri kontrol etmeye devam edin.

## <a name="ask-questions-give-feedback-get-updates"></a>Soru sorun, geri bildirimde, güncellemeler alın

Soru sormanın, geri bildirimde bulunabilir ve Medya Hizmetleri hakkında güncellemeler alabildiğiniz farklı yolları görmek için [Azure Medya Hizmetleri topluluk](media-services-community.md) makalesine göz atın.

## <a name="next-steps"></a>Sonraki adımlar

[Medya Hizmetleri v2'den v3'e geçiş için geçiş kılavuzu](migrate-from-v2-to-v3.md)
