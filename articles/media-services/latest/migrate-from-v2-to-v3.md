---
title: Azure Medya Hizmetleri v2'den v3'e geçiş | Microsoft Dokümanlar
description: Bu makalede, Azure Media Services v3'te tanıtılan değişiklikler açıklanır ve iki sürüm arasındaki farkları gösterir. Makale ayrıca Medya Hizmetleri v2 v3 taşımak için geçiş kılavuzu sağlar.
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79472076"
---
# <a name="migration-guidance-for-moving-from-media-services-v2-to-v3"></a>Medya Hizmetleri v2'den v3'e geçiş için geçiş kılavuzu

>RsS özet akışı okuyucunuza bu URL'yi `https://docs.microsoft.com/api/search/rss?search=%22Migrate+from+Azure+Media+Services+v2+to+v3%22&locale=en-us` kopyalayıp yapıştırarak güncellemeler için bu sayfayı ne zaman tekrar ziyaret edeceğiz konusunda bilgilendirilin.

Bu makalede, Medya Hizmetleri v2 v3 geçiş kılavuzu sağlar.

Bugün [eski Media Services v2 API'lerinin](../previous/media-services-overview.md)üzerine geliştirilen bir video hizmetiniz varsa, v3 API'lerine geçiş ten önce aşağıdaki yönergeleri ve hususları gözden geçirmelisiniz. V3 API'de Medya Hizmetleri'nin geliştirici deneyimini ve yeteneklerini geliştiren birçok avantaj ve yeni özellik vardır. Ancak, bu makalenin [Bilinen Sorunlar](#known-issues) bölümünde çağrıldığı gibi, API sürümleri arasındaki değişiklikler nedeniyle bazı sınırlamalar da vardır. Medya Hizmetleri ekibi v3 API'lerinde sürekli iyileştirmeler yaptığı ve sürümler arasındaki boşlukları gidermede bu sayfa korunacaktır. 

## <a name="prerequisites"></a>Ön koşullar

* [Medya Hizmetleri v2 vs. v3'ü](media-services-v2-vs-v3.md) gözden geçirin
* [!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="benefits-of-media-services-v3"></a>Medya Hizmetlerinin Faydaları v3
  
### <a name="api-is-more-approachable"></a>API daha ulaşılabilir

*  v3, Azure Resource Manager'da yerleşik olan yönetim ve işlem işlevselliğini kullanıma sunan, birleşik bir API yüzeyini temel alır. Azure Kaynak Yöneticisi şablonları Dönüşümler, Akış Uç Noktaları, Canlı Etkinlikler ve daha fazlasını oluşturmak ve dağıtmak için kullanılabilir.
* [OpenAPI Belirtimi (eski adıyla Swagger)](https://aka.ms/ams-v3-rest-sdk) belge.
    Dosya tabanlı kodlama da dahil olmak üzere tüm hizmet bileşenleri için şema ortaya çıkarır.
* .NET , [.NET](https://aka.ms/ams-v3-dotnet-ref)Core, [Node.js,](/javascript/api/overview/azure/mediaservices/management) [Python](https://aka.ms/ams-v3-python-ref), [Java](https://aka.ms/ams-v3-java-ref), [Go](https://aka.ms/ams-v3-go-ref)ve Ruby için kullanılabilir SDK'lar.
* Basit komut dosyası desteği için [Azure CLI](https://aka.ms/ams-v3-cli-ref) tümleştirmesi.

### <a name="new-features"></a>Yeni özellikler

* Dosya tabanlı İş işleme için giriş olarak bir HTTP(S) URL'si kullanabilirsiniz.<br/>İçeriğin Azure'da zaten depolanmış olması gerekmez veya Varlıklar oluşturmanız gerekmez.
* Dosya tabanlı İş işleme için [Dönüşümler](transforms-jobs-concept.md) kavramını tanır. Dönüşüm, yeniden kullanılabilir yapılandırmalar oluşturmak, Azure Kaynak Yöneticisi Şablonları oluşturmak ve işlem ayarlarını birden çok müşteri veya kiracı arasında yalıtmak için kullanılabilir.
* Bir Varlığın her biri farklı [Dinamik Ambalaj](dynamic-packaging-overview.md) ve Dinamik Şifreleme ayarlarına sahip birden çok [Akış Lı Konumbul'a](streaming-locators-concept.md) sahip olabilir.
* [İçerik koruması](content-key-policy-concept.md) çok temel özellikleri destekler.
* Tek bir bithızı katkı akışını birden çok bit hızı olan bir çıktı akışına aktarmak için Medya Hizmetlerini kullanırken 24 saate kadar olan Canlı Etkinlikler akışı yapabilirsiniz.
* Live Events'te Yeni Düşük Gecikme Liyakası canlı akış desteği. Daha fazla bilgi için [gecikme sürelerini](live-event-latency.md)görün.
* Live Event Preview [Dinamik Paketleme](dynamic-packaging-overview.md) ve Dinamik Şifrelemeyi destekler. Bu, Önizleme'de içerik korumasının yanı sıra DASH ve HLS ambalajı sağlar.
* Canlı Çıktıv2 API'lerinde Program varlığından daha kolay kullanılır. 
* Geliştirilmiş RTMP desteği (artırılmış kararlılık ve daha fazla kaynak kodlayıcı desteği).
* RTMPS güvenli yutmak.<br/>Bir Canlı Etkinlik oluşturduğunuzda, en çok 4 URL alırsınız. 4 yutma URL'leri hemen hemen aynıdır, aynı akış belirteci (AppId) var, sadece bağlantı noktası numarası bölümü farklıdır. URL'lerden ikisi RTMPS için birincil ve yedektir.   
* Varlıklarınız üzerinde rol tabanlı erişim denetimine (RBAC) sahipsiniz. 

## <a name="known-issues"></a>Bilinen sorunlar

*  Şu anda [Azure portalını](https://portal.azure.com/) şu şekilde kullanabilirsiniz:

    * Medya Hizmetleri v3 [Canlı Etkinlikler](live-events-outputs-concept.md)yönetmek , 
    * görünümü (yönetilmez) v3 [Varlıklar](assets-concept.md), 
    * [API'lara erişim hakkında bilgi alın.](access-api-portal.md) 

    Diğer tüm yönetim görevleri için (örneğin, [Dönüşümler ve İşler ve](transforms-jobs-concept.md) [İçerik koruması),](content-protection-overview.md) [REST API,](https://docs.microsoft.com/rest/api/media/) [CLI](https://aka.ms/ams-v3-cli-ref)veya desteklenen [SDK'lardan](media-services-apis-overview.md#sdks)birini kullanın.
* Özellikle Video veya Ses Analizi içeren İşlerinizin eşzamanlılığını ve performansını kontrol etmek için hesabınızda Ortam Ayrılmış Birimler (MRUs) sağlamanız gerekir. Daha fazla bilgi için bkz. [Medya İşlemeyi Ölçeklendirme](../previous/media-services-scale-media-processing-overview.md). Medya [Hizmetleri v3 için CLI 2.0](media-reserved-units-cli-how-to.md)kullanarak, [Azure portalını](../previous/media-services-portal-scale-media-processing.md)kullanarak veya [v2 API'lerini kullanarak MR'ları](../previous/media-services-dotnet-encoding-units.md)yönetebilirsiniz. Medya Hizmetleri v2 veya v3 API'lerini kullanıyor olun, MR'ları sağlamanız gerekir.
* v3 API ile oluşturulan Medya Hizmetleri varlıkları v2 API tarafından yönetilemez.  
* V2 API'deki tüm varlıklar V3 API'de otomatik olarak gösterilmez.  İki sürümdeki uyumsuz varlıkların örnekleri aşağıda verilmiştir:  
    * V2'de oluşturulan İşler ve Görevler, Dönüşüm'le ilişkilendirilmedikleri için v3'te gösterilmez. Öneri v3 Transforms and Jobs'a geçmektir. Geçiş sırasında uçuş v2 Jobs izlemek için ihtiyaç nispeten kısa bir süre olacaktır.
    * V2 ile oluşturulan kanallar ve Programlar (v3'te Canlı Etkinlikler ve Canlı Çıktılar eşlenir) v3 ile yönetilmeye devam edemez. Öneri, uygun bir Kanal durağında v3 Canlı Etkinlikler ve Canlı Çıktılar'a geçmektir.<br/>Şu anda, sürekli çalışan Kanallar geçiremezsiniz.  

> [!NOTE]
> Medya Hizmetleri ekibi v3 API'lerinde sürekli iyileştirmeler yaptığı ve sürümler arasındaki boşlukları gidermede bu sayfa korunacaktır.

## <a name="ask-questions-give-feedback-get-updates"></a>Soru sorun, geri bildirimde, güncellemeler alın

Soru sormanın, geri bildirimde bulunabilir ve Medya Hizmetleri hakkında güncellemeler alabildiğiniz farklı yolları görmek için [Azure Medya Hizmetleri topluluk](media-services-community.md) makalesine göz atın.

## <a name="next-steps"></a>Sonraki adımlar

[Öğretici: URL'ye dayalı uzak bir dosyayı kodlayın ve videoyu akışlayın - .NET](stream-files-dotnet-quickstart.md)
