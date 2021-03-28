---
title: İçerik koruma geçiş kılavuzu
description: Bu makale, Azure Media Services V2 'den v3 'e geçiş yaparken size yardımcı olacak içerik koruma senaryosu tabanlı kılavuzunuzu sağlar.
services: media-services
author: IngridAtMicrosoft
manager: femila
ms.service: media-services
ms.topic: conceptual
ms.workload: media
ms.date: 03/26/2021
ms.author: inhenkel
ms.openlocfilehash: 7ef41b76f343d8997feebc4a366deda7ce6a2afa
ms.sourcegitcommit: c8b50a8aa8d9596ee3d4f3905bde94c984fc8aa2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2021
ms.locfileid: "105644070"
---
# <a name="content-protection-scenario-based-migration-guidance"></a>Content Protection senaryo tabanlı geçiş kılavuzu

![Geçiş Kılavuzu logosu](./media/migration-guide/azure-media-services-logo-migration-guide.svg)

<hr color="#5ea0ef" size="10">

![geçiş adımları 2](./media/migration-guide/steps-4.svg)

Bu makalede, v2 API 'sindeki içerik koruma kullanım örneklerinin yeni Azure Media Services v3 API 'sine geçirilmesine ilişkin ayrıntılar ve yönergeler sağlanmaktadır.

## <a name="protect-content-in-v3-api"></a>V3 API 'de içeriği koruma

Yeni v3 API 'sindeki [çok tuşlu](design-multi-drm-system-with-access-control.md) özellikler için desteği kullanın.

Bkz. içerik koruma kavramları, öğreticiler ve belirli adımlar için aşağıdaki kılavuzlar.

## <a name="visibility-of-v2-assets-streaminglocators-and-properties-in-the-v3-api-for-content-protection-scenarios"></a>İçerik koruma senaryoları için v3 API 'sindeki v2 varlıklarının, Streamingkonumlandırıcı ve özelliklerin görünürlüğü

V3 API 'sine geçiş sırasında, v2 varlıklarınızdan bazı özelliklere veya içerik anahtarlarına erişmeniz gerektiğini göreceksiniz. Bir temel fark, v2 API 'sinin birincil tanımlayıcı anahtar olarak **AssetID** kullanması ve yenı v3 API 'sinin birincil tanımlayıcı olarak varlığın Azure Kaynak Yönetimi adını kullanmasıdır.  V2 **Asset.Name** özelliği genellikle benzersiz bir tanımlayıcı olarak kullanılmaz, bu nedenle v3 'e geçiş yaparken v2 varlık adlarının artık **varlık. Description** alanında göründüğünü göreceksiniz.

Örneğin, daha önce **"NB: CID: UUID: 8cb39104-122c-496e-9ac5-7f9e2c2547b8"** kimliğine sahip bir v2 varlığınız varsa, daha sonra, eski v2 VARLıKLARıNı v3 API aracılığıyla listelerken, ad artık sonda GUID bölümü olacaktır (Bu durumda, **"8cb39104-122c-496e-9ac5-7f9e2c2547b8"**.)

Varlık varlığındaki Yeni v3 yöntemi [Liststreamingkonumlandırıcı](https://docs.microsoft.com/rest/api/media/assets/liststreaminglocators) KULLANıLARAK v2 API 'Sinde oluşturulan varlıklarla Ilişkili **streamingkonumlandırıcı** 'yı sorgulayabilirsiniz.  Ayrıca [Liststreaminglocatorsasync](https://docs.microsoft.com/dotnet/api/microsoft.azure.management.media.assetsoperationsextensions.liststreaminglocatorsasync?view=azure-dotnet) öğesinin .NET istemci SDK sürümüne başvur

**Liststreaminglocator** yönteminin sonuçları, **Streamingpolicyname** Ile birlikte bulucunun **adını** ve **streaminglocatorıd** değerini sağlar.

İçerik koruması için **Streaminglocator** ' de kullanılan **contentkeys** 'ı bulmak için [Streaminglocator. listcontentkeysasync](https://docs.microsoft.com/dotnet/api/microsoft.azure.management.media.streaminglocatorsoperationsextensions.listcontentkeysasync?view=azure-dotnet) yöntemini çağırabilirsiniz.  

V2 API kullanılarak oluşturulan ve yayınlanan **varlıkların** , [akış ilkesinde](https://docs.microsoft.com/azure/media-services/latest/streaming-policy-concept)varsayılan bir içerik anahtar ilkesi kullanmak yerine, hem bir [içerik anahtar ilkesi](https://docs.microsoft.com/azure/media-services/latest/content-key-policy-concept) hem de v3 API 'sinde tanımlanmış bir içerik anahtarı olacaktır.

V3 API 'sindeki içerik koruması hakkında daha fazla bilgi için, [Media Services dinamik şifreleme ile Içeriğinizi koruma](https://docs.microsoft.com/azure/media-services/latest/content-protection-overview) makalesine bakın.

## <a name="how-to-list-your-v2-assets-and-content-protection-settings-using-the-v3-api"></a>V3 API kullanarak v2 varlıklarınızı ve içerik koruma ayarlarınızı listeleme

V2 API 'sinde, akış içeriğinizi korumak için genellikle **varlıkları**, **Streamingkonumlandırıcı** ve **contentkeys** ' i kullanırsınız.
V3 API 'sine geçiş yaparken, v2 API varlıklarınız, Streamingbulleyicileri ve ContentKeys 'in hepsi v3 API 'de otomatik olarak sunulur ve tüm veriler erişim için kullanılabilir.

## <a name="can-i-update-v2-properties-using-the-v3-api"></a>V3 API 'sini kullanarak v2 özelliklerini güncelleştirebilir miyim?

Hayır, v2 varlıklarındaki özellikleri Streammingkonumlandırıcı, StreamingPolicies, Içerik anahtar Ilkeleri ve v2 'deki Içerik anahtarları kullanılarak oluşturulan v3 API aracılığıyla güncelleştiremezsiniz.
V2 varlıklarında depolanan içeriği güncelleştirmeniz, değiştirmeniz veya değiştirmeniz gerekiyorsa, v2 API 'SI aracılığıyla güncelleştirmeniz veya yeni v3 API varlıkları oluşturmanız gerekir.

## <a name="how-do-i-change-the-contentkeypolicy-used-for-a-v2-asset-that-is-published-and-keep-the-same-content-key"></a>Nasıl yaparım? yayımlanan ve aynı içerik anahtarını tutan bir v2 varlığı için kullanılan ContentKeyPolicy değiştirilsin mi?

Bu durumda, ilk olarak varlık üzerindeki v2 SDK 'Sı aracılığıyla yayımdan kaldırmanız gerekir (tüm akış bulıcıları kaldırmanız, Içerik anahtarı yetkilendirme Ilkesinin bağlantısını kaldırmanız, varlık teslim Ilkesinin bağlantısını kaldırmanız, içerik anahtarının bağlantısını kaldırmanız, Içerik anahtarını silmeniz) ve v3 'de bir v3 [Streamingpolicy](https://docs.microsoft.com/azure/media-services/latest/streaming-policy-concept) ve [contentkeypolicy](https://docs.microsoft.com/azure/media-services/latest/content-key-policy-concept)kullanarak yeni bir **[streaminglocator](https://docs.microsoft.com/azure/media-services/latest/streaming-locators-concept)** oluşturun.

**[Streamminglocator](https://docs.microsoft.com/azure/media-services/latest/streaming-locators-concept)** oluştururken gerekli olan belirli içerik anahtarı tanımlayıcısını ve anahtar değerini belirtmeniz gerekir.

V3 API kullanarak v2 bulucunun silineceğini, ancak bu içerik anahtarını veya v2 API 'sinde oluşturulduklarında kullanılan içerik anahtarı ilkesini kaldırmadığını unutmayın.  

## <a name="using-amse-v2-and-amse-v3-side-by-side"></a>AMX v2 ve amde v3 yan yana kullanma

İçeriğinizi v2 'den v3 'e geçirirken, v2 API 'Leri aracılığıyla oluşturulup yayınlanan bir varlık için yan yana göstereceği verileri karşılaştırmanıza yardımcı olmak üzere, [v2 Azure Media Services Explorer aracı](https://github.com/Azure/Azure-Media-Services-Explorer/releases/tag/v4.3.15.0) 'nı [v3 Azure Media Services Explorer aracıyla](https://github.com/Azure/Azure-Media-Services-Explorer) birlikte yüklemek önerilir. Özelliklerin hepsi görünmeli, ancak biraz farklı konumlarda.  


## <a name="content-protection-concepts-tutorials-and-how-to-guides"></a>İçerik koruma kavramları, öğreticiler ve nasıl yapılır kılavuzlarından

### <a name="concepts"></a>Kavramlar

- [Media Services dinamik şifreleme ile içeriğinizi koruyun](content-protection-overview.md)
- [Erişim denetimi ile çoklu DRM'ye sahip içerik koruma sistemi tasarlama](design-multi-drm-system-with-access-control.md)
- [PlayReady lisans şablonuyla v3 Media Services](playready-license-template-overview.md)
- [Wıdevine lisans şablonuna genel bakış ile Media Services v3](widevine-license-template-overview.md)
- [Apple FairPlay lisansı gereksinimleri ve yapılandırması](fairplay-license-overview.md)
- [Akış Ilkeleri](streaming-policy-concept.md)
- [İçerik anahtarı Ilkeleri](content-key-policy-concept.md)

### <a name="tutorials"></a>Öğreticiler

[Hızlı başlangıç: içeriği şifrelemek için portalı kullanma](encrypt-content-quickstart.md)

### <a name="how-to-guides"></a>Nasıl yapılır kılavuzları

- [Var olan ilkeden imzalama anahtarı alma](get-content-key-policy-dotnet-howto.md)
- [İOS için Media Services v3 ile çevrimdışı FairPlay Akışı](offline-fairplay-for-ios.md)
- [Android için Media Services v3 ile çevrimdışı Widevine akışı](offline-widevine-for-android.md)
- [Media Services v3 ile Windows 10 için çevrimdışı PlayReady akışı](offline-plaready-streaming-for-windows-10.md)

## <a name="samples"></a>Örnekler

Ayrıca [kod örneklerinde v2 ve v3 kodunu da karşılaştırabilirsiniz](migrate-v-2-v-3-migration-samples.md).

## <a name="tools"></a>Araçlar

- [v3 Azure Media Services gezgin aracı](https://github.com/Azure/Azure-Media-Services-Explorer)
- [v2 Azure Media Services gezgin aracı](https://github.com/Azure/Azure-Media-Services-Explorer/releases/tag/v4.3.15.0)
