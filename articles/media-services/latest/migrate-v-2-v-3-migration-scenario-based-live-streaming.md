---
title: Canlı akış geçiş kılavuzunu Media Services
description: Bu makale, Azure Media Services V2 'den v3 'e geçiş yaparken size en az yardımcı olacak canlı akış senaryosu tabanlı rehberlik sağlar.
services: media-services
author: IngridAtMicrosoft
manager: femila
ms.service: media-services
ms.devlang: multiple
ms.topic: conceptual
ms.tgt_pltfrm: multiple
ms.workload: media
ms.date: 1/14/2020
ms.author: inhenkel
ms.openlocfilehash: 6a2c6495ca3685aec1bc132ec7f8a88809ad2d87
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "104598295"
---
# <a name="live-streaming-scenario-based-migration-guidance"></a>Canlı akış senaryosu tabanlı geçiş kılavuzu

![Geçiş Kılavuzu logosu](./media/migration-guide/azure-media-services-logo-migration-guide.svg)

<hr color="#5ea0ef" size="10">

![geçiş adımları 2](./media/migration-guide/steps-4.svg)

Azure portal artık canlı olay ayarlama ve yönetimini desteklemektedir.  V2 'yi v3 geçişine sınarken bu uygulamayı denemeniz önerilir.

## <a name="test-the-v3-live-event-workflow"></a>V3 canlı etkinlik iş akışını test etme

> [!NOTE]
> V2 ile oluşturulan kanallar ve programlar (yani, canlı olaylar ve v3 'deki canlı çıktılar ile eşleştirilir) v3 API 'siyle yönetilir. Bu kılavuz, mevcut v2 kanalını durdurduğunuzda, uygun bir zamanda v3 canlı olaylara ve canlı çıkışlara geçiş yapmak. Şu anda sürekli olarak çalışan 7/24 canlı bir kanalı yeni v3 Live olaylarına sorunsuz bir şekilde geçirme desteği yoktur. Bu nedenle, bakım kapalı kalma süresinin hedef kitlesi ve işletmeniz için en iyi kolaylığa göre koordine olması gerekir.

İçeriğinizi v2 'den v3 'ye taşımadan önce Media Services canlı olaylar sunmanın yeni yolunu test edin. İşte çalışmak ve geçiş için göz önünde bulundurmanız gereken v3 özellikleri aşağıda verilmiştir.

- Kodlama için yeni bir v3 [Live olayı](live-events-outputs-concept.md#live-events) oluşturun. [1080p ve 720p kodlama ön ayarlarını](live-event-types-comparison.md#system-presets)etkinleştirebilirsiniz.
- Programlar yerine [canlı çıkış](live-events-outputs-concept.md#live-outputs) varlığını kullanın
- [Akış Konumlandırıcı](streaming-locators-concept.md)oluştur.
- [HLS ve Dash](dynamic-packaging-overview.md) canlı akış ihtiyacını göz önünde bulundurun.
- Canlı olaylar için hızlı başlangıç yapmanız gerekiyorsa, yeni [Bekleme modu](live-events-outputs-concept.md#standby-mode) özelliklerini gözden geçirebilirsiniz.
- Canlı olaylarınızı çalışırken eklemek istiyorsanız, yeni [canlı](live-transcription.md) döküm özelliğini bulun.
- Daha uzun bir akış süresine ihtiyacınız varsa, v3 'de 24x7x365 canlı olay oluşturun.
- Canlı olaylarınızı izlemek için [Event Grid](monitoring/monitor-events-portal-how-to.md) kullanın.

Belirli adımlar için bkz. canlı etkinlik kavramları, öğreticiler ve aşağıdaki kılavuzlar.

## <a name="live-events-concepts-tutorials-and-how-to-guides"></a>Canlı olaylar kavramları, öğreticiler ve nasıl yapılır kılavuzlarından

### <a name="concepts"></a>Kavramlar

- [Azure Media Services v3 ile canlı akış](live-streaming-overview.md)
- [Canlı etkinlikler ve canlı çıktılar Media Services](live-events-outputs-concept.md)
- [Doğrulanan şirket içi canlı akış kodlayıcıları](recommended-on-premises-live-encoders.md)
- [İsteğe bağlı video oynatma oluşturmak için zaman değiştirme ve canlı çıktıları kullanma](live-event-cloud-dvr.md)
- [Canlı döküm (Önizleme)](live-transcription.md)
- [Canlı olay türleri karşılaştırması](live-event-types-comparison.md)
- [Canlı olay durumları ve faturalandırma](live-event-states-billing.md)
- [Canlı olay düşük gecikme süresi ayarları](live-event-latency.md)
- [Media Services canlı olay hata kodları](live-event-error-codes.md)

### <a name="tutorials-and-quickstarts"></a>Öğreticiler ve hızlı başlangıç

- [Öğretici: Media Services ile canlı akış](stream-live-tutorial-with-api.md)
- [OBS ile Azure Media Services canlı akış oluşturma](live-events-obs-quickstart.md)
- [Hızlı başlangıç: portala karşıya içerik yükleme, kodlama ve akışla](manage-assets-quickstart.md)
- [Hızlı başlangıç: kablolu atama ile Azure Media Services canlı akış oluşturma](live-events-wirecast-quickstart.md)

## <a name="samples"></a>Örnekler

Ayrıca [kod örneklerinde v2 ve v3 kodunu da karşılaştırabilirsiniz](migrate-v-2-v-3-migration-samples.md).

## <a name="next-steps"></a>Sonraki adımlar

[!INCLUDE [migration guide next steps](./includes/migration-guide-next-steps.md)]
