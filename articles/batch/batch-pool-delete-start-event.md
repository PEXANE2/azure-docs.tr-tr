---
title: Azure Batch havuzu silme başlangıç olayı
description: Batch havuzu silme başlangıç olayı başvurusu. Bu olay, bir havuz silme işlemi başladığında yayınlanır.
services: batch
author: LauraBrenner
manager: evansma
ms.assetid: ''
ms.service: batch
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: big-compute
ms.date: 04/20/2017
ms.author: labrenne
ms.openlocfilehash: 24a68c6656bd13f0c353d53870a51cdc940fd141
ms.sourcegitcommit: 21e33a0f3fda25c91e7670666c601ae3d422fb9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/05/2020
ms.locfileid: "77022214"
---
# <a name="pool-delete-start-event"></a>Havuz silme başlangıç olayı

 Bu olay, bir havuz silme işlemi başladığında yayınlanır. Havuz silme zaman uyumsuz bir olay olduğundan, silme işlemi tamamlandıktan sonra bir havuz silme tamamlandı olayının yayınlandığını bekleyebilir.

 Aşağıdaki örnekte, bir havuz silme başlangıç olayının gövdesi gösterilmektedir.

```
{
    "id": "myPool1"
}
```

|Öğe|Tür|Notlar|
|-------------|----------|-----------|
|`id`|Dize|Havuzun KIMLIĞI.|
