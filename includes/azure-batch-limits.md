---
title: include dosyası
description: include dosyası
services: batch
author: JnHs
ms.service: batch
ms.topic: include
ms.date: 06/03/2020
ms.author: jenhayes
ms.custom: include file
ms.openlocfilehash: 2a35180e4c5ca26d53be135718e345f9657af6a2
ms.sourcegitcommit: 271601d3eeeb9422e36353d32d57bd6e331f4d7b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/20/2020
ms.locfileid: "88655089"
---
| **Kaynak** | **Varsayılan sınır** | **Üst sınır** |
| --- | --- | --- |
| Her abonelikteki bölge başına Azure Batch hesabı sayısı | 1-3 |50 |
| Batch hesabı başına ayrılmış çekirdek sayısı | 90-900 | Desteğe başvurun |
| Batch hesabı başına düşük öncelikli çekirdek sayısı | 10-100 | Desteğe başvurun |
| Batch hesabı başına **[Etkin](https://docs.microsoft.com/rest/api/batchservice/job/get#jobstate)** işler ve iş zamanlamaları (**tamamlanan** işlerin sınırı yoktur) | 100-300 | 1\.000<sup>1</sup> |
| Batch hesabı başına havuz sayısı | 20-100 | 500<sup>1</sup> |

<sup>1</sup>Bu sınırlarda artış istemek için Azure Desteği’ne ulaşın.

> [!IMPORTANT]
> Adanmış kotayı isteme ve yönetme şeklini değiştirdik.  Toplam ayrılmış vCPU sayısı şu anda zorlanan değerdir, ancak yakında VM Serisi başına adanmış kotayı zorlayacağız. Düşük öncelikli kota, toplam sınıra göre zorlanmaya devam eder; Bu, VM serisi tarafından zorlanmaz.

> [!NOTE]
> Varsayılan sınırlar, Batch hesabı oluşturmak için kullandığınız abonelik türüne bağlı olarak değişiklik gösterir. Gösterilen çekirdek kotaları, Batch hizmet modundaki Batch hesapları içindir. [Batch hesabınızdaki kotaları görüntüleme](../articles/batch/batch-quota-limit.md#view-batch-quotas).

> [!IMPORTANT]
> Küresel pandemi sırasında kapasiteyi daha iyi yönetmemize yardımcı olması için bazı bölgelerdeki yeni Batch hesaplarına ve bazı abonelik türlerine yönelik varsayılan çekirdek kotaları, yukarıdaki değer aralıklarından azaltılmıştır ve bazılarında çekirdek yoktur. Yeni Batch hesabı oluşturduğunuzda [çekirdek kotanızı denetleyin](../articles/batch/batch-quota-limit.md#view-batch-quotas) ve gerekirse [çekirdek kotasında artış talep edin](../articles/batch/batch-quota-limit.md#increase-a-quota). Alternatif olarak, yeterli kotayı zaten olan toplu Iş hesaplarını yeniden kullanmayı düşünün. 
