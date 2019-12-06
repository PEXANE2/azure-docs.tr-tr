---
title: Azure Izleyici çalışma kitapları erişim denetimi
description: Rol tabanlı erişim denetimi ile önceden oluşturulmuş ve özel parametreli çalışma kitapları ile karmaşık raporlamayı kolaylaştırın
services: azure-monitor
author: mrbullwinkle
manager: carmonm
ms.service: azure-monitor
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 10/23/2019
ms.author: mbullwin
ms.openlocfilehash: c9b9f9ca2a9c08c05a3ce32230a39ca79625cd72
ms.sourcegitcommit: 9405aad7e39efbd8fef6d0a3c8988c6bf8de94eb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/05/2019
ms.locfileid: "74872936"
---
# <a name="access-control"></a>Erişim denetimi

Çalışma kitaplarında erişim denetimi iki şeyi ifade eder:

* Çalışma kitabındaki verileri okumak için erişim gerekiyor. Bu erişim, çalışma kitabında kullanılan kaynaklarda standart [Azure rolleri](https://docs.microsoft.com/azure/role-based-access-control/overview) tarafından denetlenir. Çalışma kitapları, bu kaynaklara erişim belirtmez veya yapılandırmaz. Kullanıcılar bu kaynaklara genellikle bu kaynaklara ait [Izleme okuyucu](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#monitoring-reader) rolünü kullanarak bu kaynaklara erişir.

* Çalışma kitaplarını kaydetmek için erişim gerekli

    - Özel `("My")` çalışma kitaplarını kaydetmek ek ayrıcalıklar gerektirmez. Tüm kullanıcılar özel çalışma kitaplarını kaydedebilir ve yalnızca bu çalışma kitaplarını görebilirler.
    - Paylaşılan çalışma kitaplarını kaydetme, çalışma kitabını kaydetmek için bir kaynak grubunda yazma ayrıcalıkları gerektirir. Bu ayrıcalıklar genellikle [Izleme katılımcısı](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#monitoring-contributor) rolü tarafından belirtilir, ancak *çalışma kitaplarının katkıda* bulunan rolü aracılığıyla da ayarlanabilir.
    
## <a name="standard-roles-with-workbook-related-privileges"></a>Çalışma kitabıyla ilgili ayrıcalıklar içeren standart roller

[Izleme okuyucu](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#monitoring-reader) , kaynaklardan verileri okumak için izleme araçları (çalışma kitapları dahil) tarafından kullanılan standart/Read ayrıcalıklarını içerir.

[Katkıda](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#monitoring-contributor) bulunanı izlemek, öğeleri kaydetmek için çeşitli izleme araçları tarafından kullanılan genel `/write` ayrıcalıklarını içerir (paylaşılan çalışma kitaplarını kaydetmek için `workbooks/write` ayrıcalığına dahil).
"Çalışma kitapları katılımcısı" paylaşılan çalışma kitaplarını kaydetmek için bir nesneye "çalışma kitapları/yazma" ayrıcalıklarını ekler.
Kullanıcıların yalnızca görebileceği özel çalışma kitaplarını kaydedebilmeleri için özel ayrıcalıklar gerekmez.

Özel rol tabanlı erişim denetimi için:

Paylaşılan çalışma kitaplarını kaydetmek için `microsoft.insights/workbooks/write` ekleyin. Daha ayrıntılı bilgi için bkz. [çalışma kitabı katılımcısı](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#monitoring-contributor) rolü.

## <a name="next-steps"></a>Sonraki adımlar

* Çok sayıda zengin görselleştirmeler seçeneği hakkında daha fazla [bilgi edinmeye başlayın](workbooks-visualizations.md) .
