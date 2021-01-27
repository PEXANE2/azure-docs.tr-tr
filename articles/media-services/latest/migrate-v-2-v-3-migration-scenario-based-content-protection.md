---
title: Content Protection senaryo tabanlı geçiş kılavuzu
description: Bu makale size, en az Azure Media Services V2 'den v3 'e geçiş konusunda size yardımcı olacak içerik koruma senaryosu tabanlı kılavuzluk sağlar.
services: media-services
author: IngridAtMicrosoft
manager: femila
ms.service: media-services
ms.topic: conceptual
ms.workload: media
ms.date: 1/14/2020
ms.author: inhenkel
ms.openlocfilehash: d1e8c8d1f7f710acff75fa1d78aab68df03c954d
ms.sourcegitcommit: 100390fefd8f1c48173c51b71650c8ca1b26f711
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/27/2021
ms.locfileid: "98898299"
---
# <a name="content-protection-scenario-based-migration-guidance"></a>Content Protection senaryo tabanlı geçiş kılavuzu

![Geçiş Kılavuzu logosu](./media/migration-guide/azure-media-services-logo-migration-guide.svg)

<hr color="#5ea0ef" size="10">

![geçiş adımları 2](./media/migration-guide/steps-4.svg)

Bu makale size, en az Azure Media Services V2 'den v3 'e geçiş konusunda size yardımcı olacak içerik koruma senaryosu tabanlı kılavuzluk sağlar.

## <a name="protect-content-in-v3-api"></a>V3 API 'de içeriği koruma

Yeni v3 API 'sindeki [çok tuşlu](design-multi-drm-system-with-access-control.md) özellikler için desteği kullanın.

Bkz. içerik koruma kavramları, öğreticiler ve belirli adımlar için aşağıdaki kılavuzlar.

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

## <a name="next-steps"></a>Sonraki adımlar

[!INCLUDE [migration guide next steps](./includes/migration-guide-next-steps.md)]