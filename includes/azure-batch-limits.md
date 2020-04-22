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
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81734099"
---
| **Kaynak** | **Varsayılan limit** | **Maksimum limit** |
| --- | --- | --- |
| Abonelik başına bölge başına Azure Toplu İş hesapları | 1-3 |50 |
| Toplu iş hesabı başına ayrılmış çekirdekler | 90-900 | Desteğe başvurun |
| Toplu iş hesabı başına düşük öncelikli çekirdekler | 10-100 | Desteğe başvurun |
| Toplu iş hesabı başına **[etkin](https://docs.microsoft.com/rest/api/batchservice/job/get#jobstate)** işler ve iş zamanlamaları **(tamamlanan** işlerin sınırı yoktur) | 100-300 | 1.000<sup>1</sup> |
| Batch hesabı başına havuz sayısı | 20-100 | 500<sup>1</sup> |

<sup>1.1.2</sup> Bu sınırın üzerinde bir artış istemek için Azure Desteği'ne başvurun.

> [!NOTE]
> Varsayılan sınırlar, Toplu Iş hesabı oluşturmak için kullandığınız abonelik türüne bağlı olarak değişir. Gösterilen çekirdek kotaları Toplu hizmet modundaki Toplu hesaplar içindir. [Toplu Iş hesabınızdaki kotaları görüntüleyin.](../articles/batch/batch-quota-limit.md#view-batch-quotas)

> [!IMPORTANT]
> Küresel sağlık salgını sırasında kapasiteyi daha iyi yönetmenize yardımcı olmak için, bazı bölgelerdeki yeni Toplu Iş hesapları ve bazı abonelik türleri için varsayılan çekirdek kotaları, bazı durumlarda yukarıdaki değer aralığından sıfır çekirdeklere düşürüldü. Yeni bir Toplu İşlem hesabı oluşturduğunuzda, [çekirdek kotanızı kontrol edin](../articles/batch/batch-quota-limit.md#view-batch-quotas) ve gerekirse [çekirdek kota artışı isteyin.](../articles/batch/batch-quota-limit.md#increase-a-quota) 
