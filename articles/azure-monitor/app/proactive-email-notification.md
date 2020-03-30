---
title: Akıllı Algılama bildirimi değişikliği - Azure Uygulama Öngörüleri
description: Smart Detection'dan varsayılan bildirim alıcılarına geçiş yapmayı n için değiştirin. Akıllı Algılama, trace telemetrideki olağandışı desenler için Azure Application Insights ile uygulama izlemelerini izlemenizi sağlar.
ms.topic: conceptual
author: harelbr
ms.author: harelbr
ms.date: 03/13/2019
ms.reviewer: mbullwin
ms.openlocfilehash: f73e5bbdd8585b3367e529a8fa00630042e56cac
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77671792"
---
# <a name="smart-detection-e-mail-notification-change"></a>Akıllı Algılama e-posta bildirimi değişikliği

Müşteri geri bildirimlerine dayanarak, 1 Nisan 2019'da Smart Detection'tan e-posta bildirimleri alan varsayılan rolleri değiştiriyoruz.

## <a name="what-is-changing"></a>Ne değişiyor?

Şu anda, Akıllı Algılama e-posta bildirimleri varsayılan olarak _Abonelik Sahibine, Abonelik_ _Katılımcısına_ve _Abonelik Okuyucu_ rollerine gönderilir. Bu roller genellikle izleme de etkin olarak yer almayan kullanıcıları içerir ve bu da bu kullanıcıların çoğunun gereksiz yere bildirim almasına neden olur. Bu deneyimi geliştirmek için, e-posta bildirimlerinin yalnızca Varsayılan Olarak [İzleme Okuyucusu](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#monitoring-reader) ve [İzleyici Katılımcısı](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#monitoring-contributor) rollerine gitmesini sağlamak için bir değişiklik yapıyoruz.

## <a name="scope-of-this-change"></a>Bu değişikliğin kapsamı

Bu değişiklik, aşağıdakiler hariç tüm Akıllı Algılama kurallarını etkileyecektir:

* Akıllı Algılama kuralları önizleme olarak işaretlenir. Bu Akıllı Algılama kuralları günümüzde e-posta bildirimlerini desteklemez.

* Başarısızlık Anomalileri kuralı. Bu kural, klasik bir uyarıdan birleşik uyarılar platformuna geçirildikten sonra yeni varsayılan rolleri hedeflemeye başlar [(daha](https://docs.microsoft.com/azure/azure-monitor/platform/monitoring-classic-retirement)fazla bilgi burada mevcuttur .)

## <a name="how-to-prepare-for-this-change"></a>Nasıl bu değişiklik için hazırlamak için?

Smart Detection'dan gelen e-posta bildirimlerinin ilgili kullanıcılara gönderilmesini sağlamak için, bu kullanıcıların aboneliğin [İzleme Okuyucusu](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#monitoring-reader) veya [İzleme Katılımcısı](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#monitoring-contributor) rollerine atanması gerekir.

Kullanıcıları Azure portalı üzerinden İzleme Okuyucusu veya İzleme Katılımcısı rollerine atamak için rol [atama ekle](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal#add-a-role-assignment) makalesinde açıklanan adımları izleyin. Kullanıcıların atandığı rol olarak _İzleme Okuyucusu_ veya _İzleme Katılımcısı'nı_ seçtiğinizden emin olun.

> [!NOTE]
> Kural ayarlarındaki _Ek e-posta alıcıları_ seçeneği kullanılarak yapılandırılan Akıllı Algılama bildirimlerinin belirli alıcıları bu değişiklikten etkilenmez. Bu alıcılar e-posta bildirimlerini almaya devam eder.

Bu değişiklik le ilgili herhangi bir sorunuz veya endişeniz varsa, [bizimle irtibata geçmekten](mailto:smart-alert-feedback@microsoft.com)çekinmeyin.

## <a name="next-steps"></a>Sonraki adımlar

Akıllı Algılama hakkında daha fazla bilgi edinin:

- [Hata anormallikleri](../../azure-monitor/app/proactive-failure-diagnostics.md)
- [Bellek Sızıntıları](../../azure-monitor/app/proactive-potential-memory-leak.md)
- [Performans anomalileri](../../azure-monitor/app/proactive-performance-diagnostics.md)
