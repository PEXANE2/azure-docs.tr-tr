---
title: include dosyası
description: include dosyası
services: batch
author: laurenhughes
ms.service: batch
ms.topic: include
ms.date: 05/28/2019
ms.author: lahugh
ms.custom: include file
ms.openlocfilehash: 22bfc3c86605f4c2eed4c022919b3643f394ea95
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "67080918"
---
| **Kaynak** | **Varsayılan limit** | **Maksimum limit** |
| --- | --- | --- |
| Abonelik başına bölge başına Azure Toplu İş hesapları | 1-3 |50 |
| Toplu iş hesabı başına ayrılmış çekirdekler | 90-900 | Desteğe başvurun |
| Toplu iş hesabı başına düşük öncelikli çekirdekler | 10-100 | Desteğe başvurun |
| Toplu iş hesabı başına **[etkin](https://docs.microsoft.com/rest/api/batchservice/job/get#jobstate)** işler ve iş zamanlamaları **(tamamlanan** işlerin sınırı yoktur) | 100-300 | 1.000<sup>1</sup> |
| Batch hesabı başına havuz sayısı | 20-100 | 500<sup>1</sup> |

> [!NOTE]
> Varsayılan sınırlar, Toplu Iş hesabı oluşturmak için kullandığınız abonelik türüne bağlı olarak değişir. Gösterilen çekirdek kotaları Toplu hizmet modundaki Toplu hesaplar içindir. [Toplu Iş hesabınızdaki kotaları görüntüleyin.](../articles/batch/batch-quota-limit.md#view-batch-quotas)

<sup>1.1.2</sup> Bu sınırın üzerinde bir artış istemek için Azure Desteği'ne başvurun.
