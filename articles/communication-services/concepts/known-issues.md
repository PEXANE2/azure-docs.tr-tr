---
title: Azure Iletişim Hizmetleri-bilinen sorunlar
description: Azure Iletişim hizmetleriyle ilgili bilinen sorunlar hakkında bilgi edinin
author: mikben
manager: jken
services: azure-communication-services
ms.author: mikben
ms.date: 10/03/2020
ms.topic: troubleshooting
ms.service: azure-communication-services
ms.openlocfilehash: e9e4b747d9d0ab39a1d0ecef6cf45e4cc0f9e2c5
ms.sourcegitcommit: 59cfed657839f41c36ccdf7dc2bee4535c920dd4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/06/2021
ms.locfileid: "99628163"
---
# <a name="known-issues-azure-communication-services"></a>Bilinen sorunlar: Azure Iletişim Hizmetleri

Bu makalede, Azure Iletişim hizmetleriyle ilişkili bilinen sorunlar hakkında bilgi sağlanır.

## <a name="video-streaming-quality-on-chromeandroid"></a>Chrome/Android üzerinde video akış kalitesi 

Video akış performansı, Chrome Android üzerinde azaltılabilir.

### <a name="possible-causes"></a>Olası nedenler
Uzak akışların kalitesi, bu akışı başlatmak için kullanılan istemci tarafı oluşturucunun çözünürlüğüne bağlıdır. Bir uzak akışa abone olurken, alıcı, gönderenin istemci tarafı işleyici boyutlarına göre kendi çözümünü tespit eder.

## <a name="bluetooth-headset-microphones-are-not-detected"></a>Bluetooth kulaklık mikrofonları algılanmıyor

Bluetooth kulaklıklarınızı bir Iletişim Hizmetleri çağrısına bağlama sorunlarıyla karşılaşabilirsiniz.

### <a name="possible-causes"></a>Olası nedenler
İOS üzerinde Bluetooth mikrofonu seçme seçeneği yoktur.


## <a name="repeatedly-switching-video-devices-may-cause-video-streaming-to-temporarily-stop"></a>Video cihazlarının sürekli olarak değiştirilmesi video akışının geçici olarak durdurulmasına neden olabilir

Video cihazları arasında geçiş yapmak, akış seçili cihazdan elde edilirken video akışının duraklatılmasına neden olabilir.

### <a name="possible-causes"></a>Olası nedenler
Medya cihazları arasında akış ve geçiş yapmak yoğun bir işlemdir. Sık sık geçiş performansı azalmasına neden olabilir. Geliştiricilerin bir cihaz akışını başka bir başlatmadan önce durdurması önerilir.
