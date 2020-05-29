---
title: include dosyası
description: include dosyası
services: batch
author: LauraBrenner
ms.service: batch
ms.topic: include
ms.date: 05/28/2019
ms.author: labrenne
ms.custom: include file
ms.openlocfilehash: a0d74aa38dffdd41fbe617066391ef271a507349
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81734099"
---
| **Kaynak** | **Varsayılan sınır** | **Üst sınır** |
| --- | --- | --- |
| Her abonelikteki bölge başına Azure Batch hesabı sayısı | 1-3 |50 |
| Batch hesabı başına ayrılmış çekirdek sayısı | 90-900 | Desteğe başvurun |
| Batch hesabı başına düşük öncelikli çekirdek sayısı | 10-100 | Desteğe başvurun |
| Batch hesabı başına **[Etkin](https://docs.microsoft.com/rest/api/batchservice/job/get#jobstate)** işler ve iş zamanlamaları (**tamamlanan** işlerin sınırı yoktur) | 100-300 | 1\.000<sup>1</sup> |
| Batch hesabı başına havuz sayısı | 20-100 | 500<sup>1</sup> |

<sup>1</sup>Bu sınırlarda artış istemek için Azure Desteği’ne ulaşın.

> [!NOTE]
> Varsayılan sınırlar, Batch hesabı oluşturmak için kullandığınız abonelik türüne bağlı olarak değişiklik gösterir. Gösterilen çekirdek kotaları, Batch hizmet modundaki Batch hesapları içindir. [Batch hesabınızdaki kotaları görüntüleme](../articles/batch/batch-quota-limit.md#view-batch-quotas).

> [!IMPORTANT]
> Küresel pandemi sırasında kapasiteyi daha iyi yönetmemize yardımcı olması için bazı bölgelerdeki yeni Batch hesaplarına ve bazı abonelik türlerine yönelik varsayılan çekirdek kotaları, yukarıdaki değer aralıklarından azaltılmıştır ve bazılarında çekirdek yoktur. Yeni Batch hesabı oluşturduğunuzda [çekirdek kotanızı denetleyin](../articles/batch/batch-quota-limit.md#view-batch-quotas) ve gerekirse [çekirdek kotasında artış talep edin](../articles/batch/batch-quota-limit.md#increase-a-quota). 
