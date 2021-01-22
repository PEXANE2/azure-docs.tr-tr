---
title: Paketleme ve teslim senaryosu-tabanlı geçiş kılavuzu | Microsoft Docs
description: Bu makalede, Azure Media Services V2 'den v3 'e geçiş yaparken size yardımcı olacak paketleme ve dağıtım için senaryo tabanlı rehberlik sunulmaktadır.
services: media-services
author: IngridAtMicrosoft
manager: femila
ms.service: media-services
ms.topic: conceptual
ms.workload: media
ms.date: 1/14/2020
ms.author: inhenkel
ms.openlocfilehash: 74437a4adee9e2853f7b932316d57e406b7d00be
ms.sourcegitcommit: b39cf769ce8e2eb7ea74cfdac6759a17a048b331
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/22/2021
ms.locfileid: "98690505"
---
# <a name="packaging-and-delivery-scenario-based-migration-guidance"></a>Paketleme ve teslim senaryosu-tabanlı geçiş kılavuzu

![Geçiş Kılavuzu logosu](./media/migration-guide/azure-media-services-logo-migration-guide.svg)

<hr color="#5ea0ef" size="10">

![geçiş adımları 2](./media/migration-guide/steps-4.svg)

Bu makale, paketleme ve dağıtım için Azure Media Services V2 'den v3 'e geçiş yaparken size yardımcı olacak senaryo tabanlı kılavuzluk sağlar.

İçeriğin v3 API 'de Yayınlanma biçiminde önemli değişiklikler. Yeni yayımlama modeli basitleştirilmiştir ve bir akış Bulucu oluşturmak için daha az varlık kullanır. API, daha önce gerekli dört varlık ile yalnızca iki varlığa düşürülür. İçerik anahtarı ilkeleri ve akış Konumlandırıcı artık,,, ve için gereksinimi değiştirir `ContentKeyAuthoriationPolicy` `AssetDeliveyPolicy` `ContentKey` `AccessPolicy` .

## <a name="packaging-and-delivery-in-v3"></a>V3 'de paketleme ve teslim

1. [Içerik anahtar ilkeleri](content-key-policy-concept.md)oluşturun.
1. [Akış Konumlandırıcı](streaming-locators-concept.md)oluştur.
1. [Akış yollarını](create-streaming-locator-build-url.md) alma 
    1. Bir [Dash](dynamic-packaging-overview.md#mpeg-dash-protocol) veya [HLS](dynamic-packaging-overview.md#hls-protocol) oynatıcı için yapılandırın.

Belirli adımlar için bkz. kavramları, öğreticileri ve Aşağıdaki kılavuzlardan yayımlama.

## <a name="publishing-concepts-tutorials-and-how-to-guides"></a>Kavramları, öğreticileri ve nasıl yapılır kılavuzlarını yayımlama

### <a name="concepts"></a>Kavramlar

- [Media Services v3 'de dinamik paketleme](dynamic-packaging-overview.md)
- [Filtreler](filters-concept.md)
- [Dinamik Paketleyiciyi kullanarak bildirimlerinizi filtreleyin](filters-dynamic-manifest-overview.md)
- [Azure Media Services akış uç noktaları (başlangıç)](streaming-endpoint-concept.md)
- [CDN tümleştirmesi ile içerik akışı](scale-streaming-cdn.md)
- [Akış Konumlandırıcı](streaming-locators-concept.md)

### <a name="how-to-guides"></a>Nasıl yapılır kılavuzları

- [Media Services v3 ile akış uç noktalarını yönetme](manage-streaming-endpoints-howto.md)
- [CLI örneği: Varlık yayımlama](cli-publish-asset.md)
- [Akış bulucusu ve derleme URL’leri oluşturma](create-streaming-locator-build-url.md)
- [Bir işin sonuçlarını indir](download-results-howto.md)
- [Sinyal açıklayıcı ses parçaları](signal-descriptive-audio-howto.md)
- [Eksiksiz Azure Media Player kurulumu](https://docs.microsoft.com/azure/media-services/azure-media-player/azure-media-player-full-setup)
- [Video.js oynatıcıyı Azure Media Services ile kullanma](how-to-video-js-player.md)
- [Azure Media Services Shaka oynatıcıyı kullanma](how-to-shaka-player.md)

## <a name="samples"></a>Örnekler

Ayrıca [kod örneklerinde v2 ve v3 kodunu da karşılaştırabilirsiniz](migrate-v-2-v-3-migration-samples.md).

## <a name="next-steps"></a>Sonraki adımlar

[!INCLUDE [migration guide next steps](./includes/migration-guide-next-steps.md)]
