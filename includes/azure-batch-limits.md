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
| **Kaynak** | **Varsayılan limit** | **Maksimum sınır** |
| --- | --- | --- |
| Abonelik başına bölge başına Azure Batch hesapları | 1-3 |50 |
| Batch hesabı başına adanmış çekirdekler | 90-900 | Desteğe başvurun |
| Batch hesabı başına düşük öncelikli çekirdekler | 10-100 | Desteğe başvurun |
| Batch hesabı başına **[etkin](https://docs.microsoft.com/rest/api/batchservice/job/get#jobstate)** işler ve iş zamanlamaları (**Tamamlanan** işlerin sınırı yoktur) | 100-300 | 1.000<sup>1</sup> |
| Batch hesabı başına havuz sayısı | 20-100 | 500<sup>1</sup> |

<sup>1</sup> Bu sınırın ötesinde bir artış istemek için Azure desteği 'ne başvurun.

> [!NOTE]
> Varsayılan sınırlar, bir Batch hesabı oluşturmak için kullandığınız abonelik türüne bağlı olarak farklılık gösterir. Toplu hizmet modundaki Batch hesapları için gösterilen çekirdekler kotaları. [Batch hesabınızdaki kotaları görüntüleyin](../articles/batch/batch-quota-limit.md#view-batch-quotas).

> [!IMPORTANT]
> Küresel sistem durumu pandemik sırasında kapasiteyi daha iyi yönetmemize yardımcı olmak için, bazı bölgelerde yeni Batch hesapları için varsayılan çekirdek kotaları ve bazı abonelik türleri yukarıdaki değer aralığından düşürüldü, bazı durumlarda ise sıfır çekirdeğe sahip olur. Yeni bir Batch hesabı oluşturduğunuzda, [çekirdek kotayı denetleyin](../articles/batch/batch-quota-limit.md#view-batch-quotas) ve gerekirse [çekirdek kota artışı isteyin](../articles/batch/batch-quota-limit.md#increase-a-quota). 
