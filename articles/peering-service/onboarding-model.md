---
title: Azure eşleme hizmeti ekleme modeli
description: Azure eşleme hizmeti 'ni ekleme hakkında bilgi edinin
services: peering-service
documentationcenter: na
author: derekolo
ms.service: peering-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: Infrastructure-services
ms.date: 05/18/2020
ms.author: derekol
ms.openlocfilehash: 7a257b4c2bea3bd3384a55c0a6b85d7fdd2ca583
ms.sourcegitcommit: cf7caaf1e42f1420e1491e3616cc989d504f0902
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/22/2020
ms.locfileid: "84872081"
---
# <a name="onboarding-peering-service-model"></a>Eşleme hizmeti modeli ekleme

Eşleme hizmeti 'nin ekleme işlemi, aşağıda listelenen iki modelden oluşur:

 - Eşleme hizmeti bağlantısını ekleme

 - Eşleme hizmeti bağlantı telemetrisini ekleme

Yukarıda listelenen modeller için eylem planları aşağıdaki şekilde açıklanmıştır:

| **Adım** | **Eylem**| **Ne alacaksınız**|
|-----------|---------|---------|---------|
|1|Bir bağlantı ortağından (Microsoft ile etkileşim olmadan) bağlantı sağlamak için müşteri. |Microsoft 'a iyi bağlanan ve Microsoft 'a güvenilir bağlantı için teknik gereksinimleri karşılayan bir Internet sağlayıcısı.  |
|2 (isteğe bağlı)|Müşteri, konumları Azure portal kaydeder. Bir konum şu şekilde tanımlanır: ISS/IXP adı, müşteri sitesinin fiziksel konumu (durum düzeyi), hizmet sağlayıcısı veya kuruluş tarafından konuma verilen IP öneki.  |Telemetri: Internet 'e yönelik trafik izleme, Microsoft 'tan kullanıcının en yakın kenar POP konumu. |



## <a name="onboarding-peering-service-connection"></a>Eşleme hizmeti bağlantısını ekleme

Eşleme hizmeti bağlantısı eklemek için aşağıdakileri yapın:

Ağınızı Microsoft Network 'e bağlamak üzere eşleme hizmeti edinmek için Internet servis sağlayıcısı (ISS) veya Internet Exchange (x) ortağıyla birlikte çalışın.

[Bağlantı sağlayıcılarının](location-partners.md) , eşleme hizmeti için Microsoft ile iş ortağı olduğundan emin olun. 

## <a name="onboarding-peering-service-connection-telemetry"></a>Eşleme hizmeti bağlantı telemetrisini ekleme

Müşteriler, Microsoft ağına erişirken ağ gecikmesini ve performansını izlemek için BGP yol analizi gibi telemetrisini kabul edebilir. Bu, bağlantıyı Azure portal kaydederek elde edilebilir.

Eşleme hizmeti bağlantı telemetrisini eklemek için müşterinin eşleme hizmeti bağlantısını Azure portal kaydetmesi gerekir. Yordamı öğrenmek için [Azure Portal eşleme hizmetine başvurun-](azure-portal.md) .

Bunu izleyerek Telemetriyi [burada](measure-connection-telemetry.md)yazarak ölçebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Eşleme hizmeti bağlantısının nasıl kaydedileceği hakkında adım adım işlem hakkında bilgi edinmek için bkz. [kayıt eşleme hizmeti-Azure Portal](azure-portal.md).

Ölçüm bağlantısı telemetrisi hakkında bilgi edinmek için bkz. [ölçüm bağlantı telemetrisi](measure-connection-telemetry.md).
