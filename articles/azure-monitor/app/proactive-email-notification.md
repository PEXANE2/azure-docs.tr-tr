---
title: Akıllı algılama bildirim değişikliği-Azure Application Insights
description: Akıllı algılamada varsayılan bildirim alıcılarına geçiş yapın. Akıllı algılama, izleme telemetride olağandışı desenler için Azure Application Insights ile uygulama izlemelerini izlemenize olanak tanır.
ms.topic: conceptual
author: harelbr
ms.author: harelbr
ms.date: 03/13/2019
ms.reviewer: mbullwin
ms.openlocfilehash: f73e5bbdd8585b3367e529a8fa00630042e56cac
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "77671792"
---
# <a name="smart-detection-e-mail-notification-change"></a>Akıllı algılama e-posta bildirimi değişikliği

Müşteri geri bildirimlerine bağlı olarak, 1 Nisan 2019 ' de, akıllı algılamada e-posta bildirimleri alan varsayılan rolleri değiştiriyoruz.

## <a name="what-is-changing"></a>Ne değişiyor?

Şu anda, akıllı algılama e-posta bildirimleri varsayılan olarak _abonelik sahibine_, _abonelik katkıya_ve _abonelik okuyucu_ rollerine gönderilir. Bu roller genellikle izlemeye dahil olmayan kullanıcıları içerir. Bu, bu kullanıcıların çoğunun gereksiz bildirimleri almasına neden olur. Bu deneyimi geliştirmek için, e-posta bildirimlerinin yalnızca [Izleme okuyucusuna](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#monitoring-reader) gitmesi ve varsayılan olarak [katkıda bulunan rollerinin izlenmesi](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#monitoring-contributor) için bir değişiklik yapıyoruz.

## <a name="scope-of-this-change"></a>Bu değişikliğin kapsamı

Bu değişiklik, tüm akıllı algılama kurallarını etkiler ve aşağıdakiler hariç olur:

* Önizleme olarak işaretlenen akıllı algılama kuralları. Bu akıllı algılama kuralları, e-posta bildirimlerini bugün desteklemez.

* Hata bozuklukları kuralı. Bu kural, klasik bir uyarıdan birleştirilmiş uyarılar platformuna geçirildikten sonra yeni varsayılan rolleri hedeflemeye başlar ( [burada](https://docs.microsoft.com/azure/azure-monitor/platform/monitoring-classic-retirement)daha fazla bilgi bulunur.)

## <a name="how-to-prepare-for-this-change"></a>Bu değişiklik için nasıl hazırlandınız?

Akıllı algılamada e-posta bildirimlerinin ilgili kullanıcılara gönderilmesini sağlamak için, bu kullanıcıların [Izleme okuyucusuna](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#monitoring-reader) atanması veya aboneliğin [katkıda bulunan rollerinin izlenmesi](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#monitoring-contributor) gerekir.

Kullanıcıları Izleme okuyucusuna atamak veya Azure portal ile katkıda bulunan rolleri Izlemek için [rol ataması ekleme](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal#add-a-role-assignment) makalesinde açıklanan adımları izleyin. _Izleme okuyucusunu_ veya _izleme katkı izlemeyi_ kullanıcıların atandığı rol olarak seçtiğinizden emin olun.

> [!NOTE]
> Kural ayarlarındaki _ek e-posta alıcıları_ seçeneği kullanılarak yapılandırılmış akıllı algılama bildirimlerinin belirli alıcıları bu değişiklikten etkilenmez. Bu alıcılar e-posta bildirimlerini almaya devam edecektir.

Bu değişiklik hakkında sorularınız veya endişeleriniz varsa [bizimle iletişime](mailto:smart-alert-feedback@microsoft.com)geçin.

## <a name="next-steps"></a>Sonraki adımlar

Akıllı algılama hakkında daha fazla bilgi edinin:

- [Hata anomalileri](../../azure-monitor/app/proactive-failure-diagnostics.md)
- [Bellek sızıntıları](../../azure-monitor/app/proactive-potential-memory-leak.md)
- [Performans anomalileri](../../azure-monitor/app/proactive-performance-diagnostics.md)
