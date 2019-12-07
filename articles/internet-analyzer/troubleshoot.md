---
title: Azure Internet Analyzer sorunlarını giderme | Microsoft Docs
description: Azure Internet Çözümleyicisi için sorun giderme başvurusu.
services: internet-analyzer
author: diego-perez-botero
ms.service: internet-analyzer
ms.topic: guide
ms.date: 12/04/2019
ms.author: dibotero
ms.openlocfilehash: a7216e697680bcc049d2ceb4caec74adfc1760b0
ms.sourcegitcommit: 8bd85510aee664d40614655d0ff714f61e6cd328
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/06/2019
ms.locfileid: "74897396"
---
# <a name="azure-internet-analyzer-troubleshooting"></a>Azure Internet Çözümleyicisi sorunlarını giderme

Bu makale, yaygın Internet çözümleyici sorunları için sorun giderme adımlarını içerir.

## <a name="azure-portal"></a>Azure Portalı
**Karneler bölümünde "Bu karne için yeterli ölçüm toplanmadı"**

Şunlara dikkat edin:
- Ölçüm verileri yalnızca Internet Çözümleyicisi profilinin istemci betiği gerçek Kullanıcı trafiği alan bir uygulamaya katıştırılmışsa toplanacaktır. Yapay trafik (örneğin, Azure WebApp performans testleri) genellikle katıştırılmış JavaScript kodunu yürütmez, bu nedenle bu tür trafik tarafından hiçbir ölçüm oluşturulmaz.
- Zaman serisi saat bir kez üretildikten sonra, yeni ölçüm verilerinin görünmesi için en az bu süreyi beklemeniz gerekir.
- Karneler günlük olarak (her günün sonunda UTC saati) oluşturulur.
- Karneler yalnızca seçili filtre birleşimi (test, zaman aralığı, ülke vb.) için 100 ' den fazla ölçüm toplanmışsa üretilir.

## <a name="next-steps"></a>Sonraki adımlar
[Internet ÇÖZÜMLEYICISI SSS](internet-analyzer-faq.md) makalesini okuyun
