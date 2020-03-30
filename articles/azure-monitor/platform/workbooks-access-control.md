---
title: Azure Monitör Çalışma Kitapları erişim denetimi
description: Role dayalı erişim denetimine sahip önceden oluşturulmuş ve özel parametreli çalışma kitaplarıyla karmaşık raporlamayı basitleştirin
services: azure-monitor
author: mrbullwinkle
manager: carmonm
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 10/23/2019
ms.author: mbullwin
ms.openlocfilehash: 20116ab105e4eb12875ba3cb279fb261eb5c70e4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77658430"
---
# <a name="access-control"></a>Erişim denetimi

Çalışma kitaplarındaki erişim denetimi iki şeyi ifade eder:

* Çalışma kitabındaki verileri okumak için erişim gereklidir. Bu erişim, çalışma kitabında kullanılan kaynaklardaki standart [Azure rolleri](https://docs.microsoft.com/azure/role-based-access-control/overview) tarafından denetlenir. Çalışma kitapları bu kaynaklara erişimi belirtmez veya yapılandırmaz. Kullanıcılar genellikle bu kaynaklardaki İzleme [Okuyucurolünü](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#monitoring-reader) kullanarak bu kaynaklara bu erişimi elde eder.

* Çalışma kitaplarını kaydetmek için gereken erişim

    - Özel `("My")` çalışma kitaplarını kaydetmek için ek ayrıcalıklar gerektirmez. Tüm kullanıcılar özel çalışma kitaplarını kaydedebilir ve bu çalışma kitaplarını yalnızca onlar görebilir.
    - Paylaşılan çalışma kitaplarını kaydetmek için bir kaynak grubunda yazma ayrıcalıkları gerektirir. Bu ayrıcalıklar genellikle [İzleme Katılımcısı](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#monitoring-contributor) rolü yle belirtilir, ancak *Workbooks Katılımcısıfatı* rolü yle de ayarlanabilir.
    
## <a name="standard-roles-with-workbook-related-privileges"></a>Çalışma kitabıyla ilgili ayrıcalıklara sahip standart roller

[İzleme Okuyucusu,](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#monitoring-reader) kaynaklardan gelen verileri okumak için izleme araçları (çalışma kitapları dahil) tarafından kullanılacak standart /okuma ayrıcalıklarını içerir.

[İzleme](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#monitoring-contributor) Katılımcısı, `/write` öğeleri kaydetmek için çeşitli izleme araçları tarafından `workbooks/write` kullanılan genel ayrıcalıkları içerir (paylaşılan çalışma kitaplarını kaydetme ayrıcalığı dahil).
"Çalışma Kitapları Katılımcısı", paylaşılan çalışma kitaplarını kaydetmek için bir nesneye "çalışma kitapları/yazma" ayrıcalıkları ekler.
Kullanıcıların yalnızca onların görebileceği özel çalışma kitaplarını kaydetmesi için özel ayrıcalıklar gerekmez.

Özel Rol tabanlı erişim denetimi için:

Paylaşılan `microsoft.insights/workbooks/write` çalışma kitaplarını kaydetmek için ekleyin. Daha fazla bilgi için [Çalışma Kitabı Katılımcısı](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#monitoring-contributor) rolüne bakın.

## <a name="next-steps"></a>Sonraki adımlar

* Çalışma kitapları hakkında daha fazla bilgi [edinmeye başlayın](workbooks-visualizations.md) birçok zengin görselleştirme seçeneği.
