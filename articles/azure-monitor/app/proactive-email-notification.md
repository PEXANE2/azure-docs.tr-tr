---
title: Akıllı algılama bildirim değişikliği-Azure Application Insights
description: Akıllı algılamada varsayılan bildirim alıcılarına geçiş yapın. Akıllı algılama, izleme telemetride olağandışı desenler için Azure Application Insights ile uygulama izlemelerini izlemenize olanak tanır.
ms.service: azure-monitor
ms.subservice: application-insights
ms.topic: conceptual
author: harelbr
ms.author: harelbr
ms.date: 03/13/2019
ms.reviewer: mbullwin
ms.openlocfilehash: 493deea89586347d5847895acd5eb73a866f84ac
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75432448"
---
# <a name="smart-detection-e-mail-notification-change"></a>Akıllı algılama e-posta bildirimi değişikliği

Müşteri geri bildirimlerine bağlı olarak, 1 Nisan 2019 ' de, akıllı algılamada e-posta bildirimleri alan varsayılan rolleri değiştiriyoruz.

## <a name="what-is-changing"></a>Değişen nedir?

Şu anda, akıllı algılama e-posta bildirimleri varsayılan olarak _abonelik sahibine_, _abonelik katkıya_ve _abonelik okuyucu_ rollerine gönderilir. Bu roller genellikle izlemeyle aktif olarak ilgilenmeyen kullanıcıları içerdiğinden bu durum, bu kullanıcıların gereksiz bildirimler almasına neden olur. Bu deneyimi geliştirmek için, e-posta bildirimlerinin yalnızca [Izleme okuyucusuna](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#monitoring-reader) gitmesi ve varsayılan olarak [katkıda bulunan rollerinin izlenmesi](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#monitoring-contributor) için bir değişiklik yapıyoruz.

## <a name="scope-of-this-change"></a>Bu değişikliğin kapsamı

Bu değişiklik aşağıdakiler dışında tüm Akıllı Algılama kurallarını etkileyecek:

* Önizleme olarak işaretlenen Akıllı Algılama kuralları. Bu akıllı algılama kuralları, e-posta bildirimlerini bugün desteklemez.

* Hata Anomalileri kuralı. Bu kural, klasik bir uyarıdan birleştirilmiş uyarılar platformuna geçirildikten sonra yeni varsayılan rolleri hedeflemeye başlar ( [burada](https://docs.microsoft.com/azure/azure-monitor/platform/monitoring-classic-retirement)daha fazla bilgi bulunur.)

## <a name="how-to-prepare-for-this-change"></a>Bu değişiklik için nasıl hazırlandınız?

Akıllı algılamada e-posta bildirimlerinin ilgili kullanıcılara gönderilmesini sağlamak için, bu kullanıcıların [Izleme okuyucusuna](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#monitoring-reader) atanması veya aboneliğin [katkıda bulunan rollerinin izlenmesi](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#monitoring-contributor) gerekir.

Kullanıcıları Izleme okuyucusuna atamak veya Azure portal ile katkıda bulunan rolleri Izlemek için [rol ataması ekleme](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal#add-a-role-assignment) makalesinde açıklanan adımları izleyin. _Izleme okuyucusunu_ veya _izleme katkı izlemeyi_ kullanıcıların atandığı rol olarak seçtiğinizden emin olun.

> [!NOTE]
> Kural ayarlarındaki _ek e-posta alıcıları_ seçeneği kullanılarak yapılandırılmış akıllı algılama bildirimlerinin belirli alıcıları bu değişiklikten etkilenmez. Bu alıcılar e-posta bildirimlerini almaya devam edecektir.

Bu değişiklik hakkında sorularınız veya endişeleriniz varsa [bizimle iletişime](mailto:smart-alert-feedback@microsoft.com)geçin.

## <a name="next-steps"></a>Sonraki adımlar

Akıllı algılama hakkında daha fazla bilgi edinin:

- [Hata anormallikleri](../../azure-monitor/app/proactive-failure-diagnostics.md)
- [Bellek sızıntıları](../../azure-monitor/app/proactive-potential-memory-leak.md)
- [Performans anormallikleri](../../azure-monitor/app/proactive-performance-diagnostics.md)
