---
title: Azure Iletişim Hizmetleri-SSS/bilinen sorunlar
description: Azure Iletişim Hizmetleri hakkında daha fazla bilgi edinin
author: mikben
manager: jken
services: azure-communication-services
ms.author: mikben
ms.date: 03/10/2021
ms.topic: troubleshooting
ms.service: azure-communication-services
ms.openlocfilehash: 2c6ac34d8daf00578cb1d03833a28eb8535708b7
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/20/2021
ms.locfileid: "103493654"
---
# <a name="faq--known-issues"></a>SSS/bilinen sorunlar
Bu makalede, Azure Iletişim hizmetleriyle ilgili bilinen sorunlar ve sık sorulan sorular hakkında bilgi verilmektedir.

## <a name="faq"></a>SSS

### <a name="why-is-the-quality-of-my-video-degraded"></a>Videoumun kalitesi neden düşürülmüş?

Video akışlarının kalitesi, bu akışı başlatmak için kullanılan istemci tarafı işleyicisinin boyutuna göre belirlenir. Bir uzak akışa abone olurken, alıcı, gönderenin istemci tarafı işleyici boyutlarına göre kendi çözümünü tespit eder.

### <a name="why-is-it-not-possible-to-enumerateselect-micspeaker-devices-on-safari"></a>Safari 'de Mic/konuşmacı cihazlarını neden numaralandırılamıyor/seçemezsiniz?

Uygulamalar Safari iOS/iPad üzerinde mik/hoparlör cihazlarını (Bluetooth gibi) numaralandıramaz/seçemezsiniz. Bu, işletim sisteminin bir kısıtlamasıdır. her zaman yalnızca bir cihaz olabilir.

MacOS üzerinde Safari için-uygulama, Iletişim Hizmetleri Device Manager üzerinden konuşmacı listeleyemez/seçemezsiniz. bunlar işletim sistemi aracılığıyla seçilmelidir. MacOS üzerinde Chrome kullanıyorsanız, uygulama Iletişim Hizmetleri Device Manager aracılığıyla cihazları numaralandırabilir/seçebilir.

## <a name="known-issues"></a>Bilinen sorunlar

Bu bölüm, Azure Iletişim hizmetleriyle ilişkili bilinen sorunlar hakkında bilgi sağlar.

### <a name="repeatedly-switching-video-devices-may-cause-video-streaming-to-temporarily-stop"></a>Video cihazlarının sürekli olarak değiştirilmesi video akışının geçici olarak durdurulmasına neden olabilir

Video cihazları arasında geçiş yapmak, akış seçili cihazdan elde edilirken video akışının duraklatılmasına neden olabilir.

#### <a name="possible-causes"></a>Olası nedenler
Medya cihazları arasında akış ve geçiş yapmak yoğun bir işlemdir. Sık sık geçiş performansı azalmasına neden olabilir. Geliştiricilerin bir cihaz akışını başka bir başlatmadan önce durdurması önerilir.
