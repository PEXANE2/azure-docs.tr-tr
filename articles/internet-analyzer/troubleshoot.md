---
title: Azure Internet Analyzer sorunlarını giderme | Microsoft Docs
description: Azure Internet Çözümleyicisi için sorun giderme başvurusu.
services: internet-analyzer
author: diego-perez-botero
ms.service: internet-analyzer
ms.topic: guide
ms.date: 12/04/2019
ms.author: dibotero
ms.openlocfilehash: 74bf0422bbe6c2c1c84365c1e8f9329a01ff9fdd
ms.sourcegitcommit: f9601bbccddfccddb6f577d6febf7b2b12988911
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/12/2020
ms.locfileid: "75909032"
---
# <a name="azure-internet-analyzer-troubleshooting"></a>Azure Internet Çözümleyicisi sorunlarını giderme

Bu makale, yaygın Internet çözümleyici sorunları için sorun giderme adımlarını içerir.

## <a name="azure-portal"></a>Azure Portalı
**Karneler bölümünde "Bu karne için yeterli ölçüm toplanmadı"**

Şunlara dikkat edin:
- İstemci betiği bir **https** Web sitesine katıştırmalıdır. Betik bir düz metin (**http://** ) veya yerel (**File://** ) Web sitesinde çalışıyorsa ölçümler toplanmaz.
- Ölçüm verileri yalnızca Internet Çözümleyicisi profilinin istemci betiği gerçek Kullanıcı trafiği alan bir uygulamaya katıştırılmışsa toplanacaktır. Yapay trafik (örneğin, Azure WebApp performans testleri) genellikle katıştırılmış JavaScript kodunu yürütmez, bu nedenle bu tür trafik tarafından hiçbir ölçüm oluşturulmaz.
- Zaman serisi saat bir kez üretildikten sonra, yeni ölçüm verilerinin görünmesi için en az bu süreyi beklemeniz gerekir.
- Karneler günlük olarak (her günün sonunda UTC saati) oluşturulur.
- Karneler yalnızca seçili filtre birleşimi (test, zaman aralığı, ülke vb.) için 100 ' den fazla ölçüm toplanmışsa üretilir.

## <a name="next-steps"></a>Sonraki adımlar
[Internet ÇÖZÜMLEYICISI SSS](internet-analyzer-faq.md) makalesini okuyun
