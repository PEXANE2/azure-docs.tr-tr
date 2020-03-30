---
title: Azure Medya Hizmetlerinde Akış Konumlayıcıları | Microsoft Dokümanlar
description: Bu makalede, Akış Lı Konum bulucuların ne olduğu ve Azure Medya Hizmetleri tarafından nasıl kullanıldığı hakkında bir açıklama yapılır.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 03/04/2020
ms.author: juliako
ms.openlocfilehash: 3a9568e1a0307cd1713c511ef42c065424306548
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78302891"
---
# <a name="streaming-locators"></a>Akış Bulucuları

Çıkış Varlığınızdaki videoların istemcilerde kayıttan yürütülebilmesi için bir [Akış Bulucu](https://docs.microsoft.com/rest/api/media/streaminglocators) ve ardından akış URL'si oluşturmanız gerekir. URL oluşturmak için, Akış Uç Nokta ana bilgisayar adını ve Akış Lı Konum belirleme yolunu oluşturmanız gerekir. .NET örneği için bkz. [Akış Bulucu edinme](stream-files-tutorial-with-api.md#get-a-streaming-locator).

**Akış Lı Bulucu** oluşturma işlemine yayımlama denir. Varsayılan olarak, **Akış Bulucu,** API aramalarını yaptıktan hemen sonra geçerlidir ve isteğe bağlı başlangıç ve bitiş saatlerini yapılandırmadığınız sürece silinene kadar sürer. 

**Akış Bulucu**oluştururken, bir **Varlık** adı ve **Akış İlkesi** adı belirtmeniz gerekir. Daha fazla bilgi edinmek için aşağıdaki kaynaklara bakın:

* [Varlıklar](assets-concept.md)
* [Akış İlkeleri](streaming-policy-concept.md)
* [İçerik Anahtar İlkeleri](content-key-policy-concept.md)

Ayrıca, kullanıcınızın içeriği yalnızca bu saatler arasında (örneğin, 5/1/2019 ile 5/5/2019 arasında) oynatmasına izin veren Akış Bulucu'nuzda başlangıç ve bitiş saatini de belirtebilirsiniz.  

## <a name="considerations"></a>Dikkat edilmesi gerekenler

* **Akış Lı Konum belirleyiciler** güncel değildir. 
* Datetime türünde **Akış Lı Konum belirleyicilerin** özellikleri her zaman UTC biçimindedir.
* Medya Hizmeti hesabınız için sınırlı bir dizi ilke tasarlamalı ve aynı seçenekler gerektiğinde Akış Lı Konum belirleyicileriniz için yeniden kullanmalısınız. Daha fazla bilgi için [Kotalar ve sınırlamalar](limits-quotas-constraints.md)bölümüne bakın.

## <a name="create-streaming-locators"></a>Akış Lı Konum belirleyiciler Oluşturma  

### <a name="not-encrypted"></a>Şifrelenmemiş

Dosyanızı açık (şifrelenmemiş) olarak aktarmak istiyorsanız, önceden tanımlanmış açık akış ilkesini ayarlayın: 'Predefined_ClearStreamingOnly' (.NET'te PredefinedStreamingPolicy.ClearStreamingOnly enumunu kullanabilirsiniz).

```csharp
StreamingLocator locator = await client.StreamingLocators.CreateAsync(
    resourceGroup,
    accountName,
    locatorName,
    new StreamingLocator
    {
        AssetName = assetName,
        StreamingPolicyName = PredefinedStreamingPolicy.ClearStreamingOnly
    });
```

### <a name="encrypted"></a>Şifreli 

İçeriğinizi CENC şifrelemesiyle şifrelemeniz gerekiyorsa, ilkenizi 'Predefined_MultiDrmCencStreaming' olarak ayarlayın. Widevine şifreleme, DASH akışına ve PlayReady to Smooth'a uygulanır. Anahtar, yapılandırılan DRM lisanslarına dayalı olarak bir oynatma istemcisine teslim edilir.

```csharp
StreamingLocator locator = await client.StreamingLocators.CreateAsync(
    resourceGroup,
    accountName,
    locatorName,
    new StreamingLocator
    {
        AssetName = assetName,
        StreamingPolicyName = "Predefined_MultiDrmCencStreaming",
        DefaultContentKeyPolicyName = contentPolicyName
    });
```

HLS akışınızı CBCS (FairPlay) ile şifrelemek istiyorsanız, 'Predefined_MultiDrmStreaming' kullanın.

> [!NOTE]
> Widevine, Google Inc. tarafından sağlanan ve Google, Inc.'in hizmet koşullarına ve Gizlilik Politikasına tabi olan bir hizmettir.

## <a name="associate-filters-with-streaming-locators"></a>Filtreleri Akış Lı Konumlatörler ile ilişkilendirin

Bkz. [Filtreler: Akış Lı Konumbelirleyiciler ile ilişkilendirin.](filters-concept.md#associating-filters-with-streaming-locator)

## <a name="filter-order-page-streaming-locator-entities"></a>Filtre, sipariş, sayfa Akış Bulucu varlıkları

Bkz. [Medya Hizmetleri varlıklarınıfiltreleme, sipariş etme, sayfalama.](entities-overview.md)

## <a name="list-streaming-locators-by-asset-name"></a>Varlık adına göre Akış Konumlayıcıları Listele

İlgili Varlık adını temel alan Akış Bulucuları almak için aşağıdaki işlemleri kullanın:

|Dil|API|
|---|---|
|REST|[liststreaminglocators](https://docs.microsoft.com/rest/api/media/assets/liststreaminglocators)|
|CLI|[az ams varlık listesi-streaming-locators](https://docs.microsoft.com/cli/azure/ams/asset?view=azure-cli-latest#az-ams-asset-list-streaming-locators)|
|.NET|[ListStreamingLocators](https://docs.microsoft.com/dotnet/api/microsoft.azure.management.media.assetsoperationsextensions.liststreaminglocators?view=azure-dotnet#Microsoft_Azure_Management_Media_AssetsOperationsExtensions_ListStreamingLocators_Microsoft_Azure_Management_Media_IAssetsOperations_System_String_System_String_System_String_)|
|Java|[VarlıkStreamingLocator](https://docs.microsoft.com/rest/api/media/assets/liststreaminglocators#assetstreaminglocator)|
|Node.js|[listStreamingLocators](https://docs.microsoft.com/javascript/api/@azure/arm-mediaservices/assets#liststreaminglocators-string--string--string--msrest-requestoptionsbase-)|

## <a name="see-also"></a>Ayrıca bkz.

* [Varlıklar](assets-concept.md)
* [Akış İlkeleri](streaming-policy-concept.md)
* [İçerik Anahtar İlkeleri](content-key-policy-concept.md)
* [Öğretici: .NET kullanarak video yükleme, kodlama ve akış](stream-files-tutorial-with-api.md)

## <a name="next-steps"></a>Sonraki adımlar

[Akış bulucu oluşturma ve URL oluşturma](create-streaming-locator-build-url.md)
