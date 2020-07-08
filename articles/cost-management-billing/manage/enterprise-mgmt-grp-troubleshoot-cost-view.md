---
title: Azure kurumsal maliyet görünümleri ile ilgili sorunları giderme
description: Azure portalındaki kurumsal maliyet görünümleriyle ilgili karşılaşabileceğiniz sorunların nasıl çözüleceğini öğrenin.
author: bandersmsft
ms.reviewer: amberb
ms.service: cost-management-billing
ms.topic: troubleshooting
ms.date: 06/05/2019
ms.author: banders
ms.custom: seodec18
ms.openlocfilehash: 6c4fe563057e8d7fedbd68280bc8059867d997ed
ms.sourcegitcommit: d7008edadc9993df960817ad4c5521efa69ffa9f
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/08/2020
ms.locfileid: "86120254"
---
# <a name="troubleshoot-enterprise-cost-views"></a>Kurumsal maliyet görünümleri ile ilgili sorunları giderme

Kurumsal kayıtlar içinde, kayıttaki kullanıcıların maliyetleri görmemesine neden olabilecek birçok ayar vardır.  Bu ayarlar, kayıt yöneticisi tarafından yönetilir. Veya kayıt doğrudan Microsoft üzerinden satın alınmadıysa ayarlar iş ortağı tarafından yönetilir.  Bu makale, ayarların ne olduğunu ve kaydı nasıl etkilediğini anlamanıza yardımcı olur. Bu ayarlar, Azure Rol tabanlı erişim denetimi (RBAC) rollerinden bağımsızdır.

## <a name="enable-access-to-costs"></a>Maliyetlere erişimi etkinleştirme

Maliyet bilgilerini ararken Yetkisiz veya *“Kaydınızda maliyet görünümleri devre dışı bırakıldı.”* gibi bir ileti görüyor musunuz?
![Abonelik için Geçerli Maliyet alanında “yetkisiz” ifadesini gösteren ekran görüntüsü.](./media/enterprise-mgmt-grp-troubleshoot-cost-view/unauthorized.png)

Bunun nedeni aşağıdakilerden biri olabilir:

1. Azure’u bir kurumsal iş ortağı aracılığıyla satın aldınız ve iş ortağı henüz fiyatlandırmayı yayınlamadı. [Enterprise portal](https://ea.azure.com) içinde fiyatlandırma ayarını güncelleştirmesi için iş ortağınızla görüşün.
2. EA Direct müşterisiyseniz birkaç olasılık vardır:
    * Hesap Sahibisiniz ve Kayıt Yöneticiniz, **Hesap Sahibi için ücretleri görüntüleme** ayarını devre dışı bıraktı.  
    * Bölüm Yöneticisisiniz ve Kayıt Yöneticiniz, **Bölüm Yöneticisi için ücretleri görüntüleme** ayarını devre dışı bıraktı.
    * Erişim elde etmek için Kayıt Yöneticinizle görüşün. Kayıt Yöneticisi, [Enterprise portal](https://ea.azure.com/manage/enrollment)’daki ayarları güncelleştirebilir.

      ![Ücretleri görüntüleme için Enterprise Portal Ayarlarını gösteren ekran görüntüsü.](./media/enterprise-mgmt-grp-troubleshoot-cost-view/ea-portal-settings.png)

## <a name="asset-is-unavailable"></a>Varlık kullanılamıyor

Bir abonelik veya yönetim grubuna erişmeye çalışırken **Bu varlık kullanılamıyor** ifadesini gösteren bir hata iletisi alırsanız, bu öğeyi görüntülemek için doğru role sahip değilsiniz demektir.  

!["Varlık kullanılamıyor" iletisini gösteren ekran görüntüsü.](./media/enterprise-mgmt-grp-troubleshoot-cost-view/asset-not-found.png)

Azure abonelik veya yönetim grubu yöneticinizden erişim izni vermesini isteyin. Daha fazla bilgi için bkz. [RBAC ve Azure portalını kullanarak erişimi yönetme](../../role-based-access-control/role-assignments-portal.md).

## <a name="next-steps"></a>Sonraki adımlar
- Sorularınız varsa ya da yardıma gereksinim duyuyorsanız [destek isteği oluşturun](https://go.microsoft.com/fwlink/?linkid=2083458).
