---
title: Azure Media Services V2 'den v3 'e geçiş | Microsoft Docs
description: Bu makalede, Azure Media Services v3 sürümünde tanıtılan ve iki sürüm arasındaki farklılıklar gösteren değişiklikler açıklanmaktadır. Makale Ayrıca Media Services V2 'den v3 'e geçiş için geçiş kılavuzu sağlar.
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
ms.openlocfilehash: 72d413c5d8bc982d885d889da35b29a3607410cc
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "79472076"
---
# <a name="migration-guidance-for-moving-from-media-services-v2-to-v3"></a>Media Services V2 'den v3 'e geçmek için geçiş kılavuzu

>Bu URL 'YI kopyalayarak ve yapıştırarak bu sayfanın ne zaman yeniden ziyaret ettikleridir hakkında bildirim `https://docs.microsoft.com/api/search/rss?search=%22Migrate+from+Azure+Media+Services+v2+to+v3%22&locale=en-us` alın: RSS akışı okuyucusuna.

Bu makale, Media Services V2 'den v3 'e geçiş kılavuzu sağlar.

Bugün [eski Media Services V2 API 'lerinde](../previous/media-services-overview.md)geliştirilen bir video hizmetiniz varsa, v3 API 'lerine geçiş yapmadan önce aşağıdaki yönergeleri ve konuları gözden geçirmeniz gerekir. V3 API 'de Media Services Geliştirici deneyimini ve yeteneklerini geliştiren birçok avantaj ve yeni özellik vardır. Ancak, bu makalenin [bilinen sorunlar](#known-issues) bölümünde ÇAĞRıLDıKÇA, API sürümleri arasındaki değişikliklerden dolayı bazı sınırlamalar de vardır. Media Services ekibi, v3 API 'Lerinde devam eden geliştirmeler yaptığında ve sürümler arasındaki boşlukları adresleyen Bu sayfa korunacak. 

## <a name="prerequisites"></a>Ön koşullar

* [Media Services V2 ve v3 karşılaştırması](media-services-v2-vs-v3.md)
* [!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="benefits-of-media-services-v3"></a>Media Services v3 avantajları
  
### <a name="api-is-more-approachable"></a>API daha ulaşılabilir

*  v3, Azure Resource Manager'da yerleşik olan yönetim ve işlem işlevselliğini kullanıma sunan, birleşik bir API yüzeyini temel alır. Azure Resource Manager şablonlar, dönüşümler, akış uç noktaları, canlı olaylar ve daha fazlasını oluşturmak ve dağıtmak için kullanılabilir.
* [Openapı belirtimi (eski adı Swagger)](https://aka.ms/ams-v3-rest-sdk) belgesi.
    Dosya tabanlı kodlama da dahil olmak üzere tüm hizmet bileşenleri için şemayı gösterir.
* [.Net](https://aka.ms/ams-v3-dotnet-ref), .NET Core, [Node. js](/javascript/api/overview/azure/mediaservices/management), [Python](https://aka.ms/ams-v3-python-ref), [Java](https://aka.ms/ams-v3-java-ref), [Go](https://aka.ms/ams-v3-go-ref)ve Ruby için kullanılabilir SDK 'lar.
* Basit betik desteği için [Azure CLI](https://aka.ms/ams-v3-cli-ref) tümleştirmesi.

### <a name="new-features"></a>Yeni özellikler

* Dosya tabanlı Iş işleme için, giriş olarak bir HTTP (S) URL 'SI kullanabilirsiniz.<br/>Azure 'da zaten depolanmış içeriklere sahip olmanız veya varlık oluşturmanız gerekmez.
* Dosya tabanlı Iş işleme için [dönüşüm](transforms-jobs-concept.md) kavramını tanıtır. Bir dönüşüm yeniden kullanılabilir yapılandırma oluşturmak, Azure Resource Manager şablonları oluşturmak ve birden çok müşteri veya kiracı arasında işlem ayarlarını yalıtmak için kullanılabilir.
* Bir varlık, her biri farklı [dinamik paketleme](dynamic-packaging-overview.md) ve dinamik şifreleme ayarlarına sahip birden fazla [akış Konumlandırıcı](streaming-locators-concept.md) olabilir.
* [İçerik koruması](content-key-policy-concept.md) çoklu anahtar özelliklerini destekler.
* Tek bir bit hızlı katkı akışını birden çok bitme sahip bir çıkış akışına dönüştürmek için Media Services kullanırken, 24 saate kadar olan canlı olayları akışlara aktarabilirsiniz.
* Canlı olaylarda yeni düşük gecikmeli canlı akış desteği. Daha fazla bilgi için bkz. [gecikme](live-event-latency.md).
* Canlı etkinlik önizlemesi, [dinamik paketleme](dynamic-packaging-overview.md) ve dinamik şifrelemeyi destekler. Bu, önizleme sırasında içerik korumasının yanı sıra DASH ve HLS ambalajını de sunar.
* Canlı çıktı, v2 API 'Lerinde program varlığından daha basittir. 
* İyileştirilmiş RTMP desteği (artan kararlılık ve daha fazla kaynak Kodlayıcısı desteği).
* RTMPS güvenli alma.<br/>Canlı bir olay oluşturduğunuzda 4 alma URL 'Si alırsınız. 4 içe alınan URL 'Ler neredeyse aynıdır, aynı akış belirtecine (AppID) sahiptir, yalnızca bağlantı noktası numarası bölümü farklıdır. URL 'Lerden ikisi, RTMPS için birincil ve yedeğdir.   
* Varlıklarınız üzerinde rol tabanlı erişim denetimi (RBAC) vardır. 

## <a name="known-issues"></a>Bilinen sorunlar

*  Şu anda [Azure Portal](https://portal.azure.com/) şunları yapmak için kullanabilirsiniz:

    * Media Services v3 [canlı olaylarını](live-events-outputs-concept.md)yönetin, 
    * v3 [varlıklarını](assets-concept.md)görüntüleme (yönetme), 
    * [API 'lere erişme hakkında bilgi alın](access-api-portal.md). 

    Diğer tüm yönetim görevleri (örneğin, [dönüşümler ve işler](transforms-jobs-concept.md) ve [içerik koruması](content-protection-overview.md)) için [REST API](https://docs.microsoft.com/rest/api/media/), [CLI](https://aka.ms/ams-v3-cli-ref)veya desteklenen [SDK 'lardan](media-services-apis-overview.md#sdks)birini kullanın.
* Işlerinizin eşzamanlılık ve performansını denetlemek için, özellikle video veya ses analizine sahip olanlar için hesabınıza medya ayrılmış birimleri (MRU) sağlamanız gerekir. Daha fazla bilgi için bkz. [Medya İşlemeyi Ölçeklendirme](../previous/media-services-scale-media-processing-overview.md). [Media Services v3 Için clı 2,0](media-reserved-units-cli-how-to.md)kullanarak, [Azure Portal](../previous/media-services-portal-scale-media-processing.md)veya [v2 API 'lerini](../previous/media-services-dotnet-encoding-units.md)kullanarak MRUs 'yi yönetebilirsiniz. Media Services v2 veya v3 API 'Leri kullanıp kullanmayacağınızı MRU sağlamanız gerekir.
* V3 API ile oluşturulan Media Services varlıkları v2 API 'SI tarafından yönetilemez.  
* V2 API 'sindeki varlıkların hepsi, v3 API 'sinde otomatik olarak gösterilmez.  Aşağıda, uyumlu olmayan iki sürümde varlıkların örnekleri verilmiştir:  
    * V2 'de oluşturulan işler ve görevler, bir dönüşümle ilişkilendirilmediği için v3 'de gösterilmez. Öneri, v3 dönüştürmelerini ve Işlerine geçiş yapmak. Geçiş sırasında değerlendirmedeki v2 işlerini izlemeye ihtiyaç duyan görece kısa bir zaman dilimi olacaktır.
    * V2 ile oluşturulan kanallar ve programlar (yani, canlı olaylar ve v3 'de canlı çıktılar ile eşlenir) v3 ile yönetilmeye devam edemez. Kullanışlı bir kanal durdurulduğunda v3 canlı olaylara ve canlı çıkışlara geçiş yapma önerisi.<br/>Şu anda, sürekli olarak çalışan kanalları geçiremezsiniz.  

> [!NOTE]
> Media Services ekibi, v3 API 'Lerinde devam eden geliştirmeler yaptığında ve sürümler arasındaki boşlukları adresleyen Bu sayfa korunacak.

## <a name="ask-questions-give-feedback-get-updates"></a>Soru sorun, geri bildirimde bulunun, güncelleştirmeleri al

Soru sormak, geri bildirimde bulunmak ve Media Services hakkında güncelleştirmeler almak için [Azure Media Services Community](media-services-community.md) makalesine göz atın.

## <a name="next-steps"></a>Sonraki adımlar

[Öğretici: URL 'yi temel alarak uzak bir dosyayı kodlayın ve videoyu akışa sunun-.NET](stream-files-dotnet-quickstart.md)
