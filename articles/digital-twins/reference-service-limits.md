---
title: Hizmet sınırlamaları
titleSuffix: Azure Digital Twins
description: Azure Digital TWINS hizmetinin sınırlarını gösteren grafik.
author: baanders
ms.author: baanders
ms.date: 05/05/2020
ms.topic: article
ms.service: digital-twins
ms.openlocfilehash: 46f378baad51e959f8b3c074cc24e5bbdfdd95d4
ms.sourcegitcommit: aa00fecfa3ad1c26ab6f5502163a3246cfb99ec3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/14/2021
ms.locfileid: "107389613"
---
# <a name="azure-digital-twins-service-limits"></a>Azure dijital TWINS hizmet limitleri

Bunlar Azure dijital TWINS 'in hizmet sınırlamalarıdır.

> [!NOTE]
> Bu hizmetin bazı alanlarında ayarlanabilir sınırlar vardır. Bu, aşağıdaki tabloda, *ayarlanabilir?* sütunuyla birlikte gösterilir. Sınır ayarlanabilirken, *ayarlanabilir?* değeri *Evet*' tir.
>
> İşiniz varsayılan sınırın üzerinde ayarlanabilir bir sınırı veya kotayı yükseltmeyi gerektiriyorsa, [bir destek bileti açarak](https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest)ek kaynaklar isteyebilirsiniz.

## <a name="limits-by-type"></a>Türe göre sınırlar

[!INCLUDE [Azure Digital Twins limits](../../includes/digital-twins-limits.md)]

## <a name="working-with-limits"></a>Limitlerle çalışma

Sınıra ulaşıldığında, hizmet ek istekleri kısıtlar. Bu, bu isteklerden 404 hata yanıtı oluşmasına neden olur.

Bunu yönetmek için, limitlerle çalışmaya yönelik bazı öneriler aşağıda verilmiştir.
* **Yeniden deneme mantığını kullanın.** [Azure dijital TWINS SDK 'ları](how-to-use-apis-sdks.md) başarısız istekler için yeniden deneme mantığı uygular, bu nedenle, BELIRTILEN bir SDK ile çalışıyorsanız bu zaten yerleşik olarak bulunur. Aksi takdirde, kendi uygulamanızda yeniden deneme mantığı uygulamayı düşünün. Hizmet, `Retry-After` hata yanıtında bir üst bilgi gönderir ve yeniden denemeden önce ne kadar bekleneceğini tespit edebilirsiniz.
* **Yaklaşmalar hakkında uyarı almak için eşikleri ve bildirimleri kullanın.** Azure Digital TWINS için bazı hizmet sınırlarına, bu alanlardaki kullanımı izlemek için kullanılabilecek ilgili [ölçümler](troubleshoot-metrics.md) vardır. Eşik approached olduğunda eşikleri yapılandırmak ve herhangi bir ölçümde uyarı ayarlamak için [*sorun giderme: uyarıları ayarlama*](troubleshoot-alerts.md)konusundaki yönergelere bakın. Ölçümlerin sağlanmadığı diğer sınırlara yönelik bildirimleri ayarlamak için, bu mantığı kendi uygulama kodunuzda uygulamayı düşünün.

## <a name="next-steps"></a>Sonraki adımlar

Azure dijital TWINS 'nin geçerli sürümü hakkında daha fazla bilgi için bkz. hizmet genel bakış:
* [*Genel Bakış: Azure dijital TWINS nedir?*](overview.md)
