---
title: Canlı olay durumları ve faturalama Azure Media Services | Microsoft Docs
description: Bu konu, Azure Media Services LiveEvent durumları ve faturalandırma hakkında genel bakış sunar.
services: media-services
documentationcenter: ''
author: IngridAtMicrosoft
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: ne
ms.topic: article
ms.date: 08/31/2020
ms.author: inhenkel
ms.openlocfilehash: 37098b8268d5a451935c925114fb762c574fa55c
ms.sourcegitcommit: bcda98171d6e81795e723e525f81e6235f044e52
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/01/2020
ms.locfileid: "89265318"
---
# <a name="live-event-states-and-billing"></a>Canlı olay durumları ve faturalandırma

[!INCLUDE [media services api v3 logo](./includes/v3-hr.md)]

Azure Media Services, canlı bir olay, durum geçişleri **çalışmaya**hemen sonra faturalandırmaya başlar. Hizmetten herhangi bir video akışı olmasa bile faturalandırılırsınız. Canlı etkinliği faturalandırmayı durdurmak için canlı olayı durdurmanız gerekir. Canlı döküm, canlı olayla aynı şekilde faturalandırılır.

[Canlı olayınızdaki](/rest/api/media/liveevents) **Liveeventencodingtype** standart veya Premium1080p olarak ayarlandığında Media Services, giriş akışı kaybolduktan sonra çalışmakta olan 12 saat içinde hala **çalışır** durumda olan canlı olayları otomatik olarak kapatır ve çalışan **canlı bir çıktı**yoktur. Ancak, hala canlı olayın **çalışır** durumda olduğu zaman faturalandırılırsınız.

> [!NOTE]
> Doğrudan geçiş canlı olayları otomatik olarak kapatılmaz ve aşırı faturalandırmayı önlemek için API aracılığıyla açıkça durdurulmalıdır. 

## <a name="states"></a>Durumlar

Canlı olay, aşağıdaki durumlardan birinde olabilir.

|Durum|Açıklama|
|---|---|
|**Durduruldu**| Bu, oluşturulduktan sonra canlı etkinliğin ilk durumudur (autostart, true olarak ayarlanmadığı müddetçe).) Bu durumda faturalandırma yapılmaz. Bu durumda, canlı olay özellikleri güncelleştirilemeyebilir ancak akışa izin verilmez.|
|**Başlatılıyor**| Canlı etkinlik başlatılıyor ve kaynaklar ayrıldı. Bu durumda faturalandırma yapılmaz. Bu durum sırasında güncelleştirmelere veya akışa izin verilmez. Bir hata oluşursa canlı olay durdurulmuş duruma geri döner.|
|**Çalışıyor**| Canlı olay kaynakları ayrıldı, alma ve önizleme URL 'Leri oluşturulmuştur ve canlı akışlar alabilir. Bu noktada faturalandırma etkin olur. Daha fazla faturalandırmayı durdurmak için canlı olay kaynağında durdurmayı açıkça çağırmanız gerekir.|
|**Durduruluyor**| Canlı etkinlik durduruluyor ve kaynaklar zaten sağlanıyor. Bu geçici durumda faturalandırma yapılmaz. Bu durum sırasında güncelleştirmelere veya akışa izin verilmez.|
|**Siliniyor**| Canlı etkinlik siliniyor. Bu geçici durumda faturalandırma yapılmaz. Bu durum sırasında güncelleştirmelere veya akışa izin verilmez.|

Canlı olayları oluştururken canlı onayları etkinleştirmeyi seçebilirsiniz. Bunu yaparsanız, canlı olay **çalışır** durumda olduğunda canlı döküm için faturalandırılırsınız. Canlı olay aracılığıyla bir ses akışı olmasa bile faturalandırılacağını unutmayın.

## <a name="next-steps"></a>Sonraki adımlar

- [Canlı akışa genel bakış](live-streaming-overview.md)
- [Canlı akış öğreticisi](stream-live-tutorial-with-api.md)
