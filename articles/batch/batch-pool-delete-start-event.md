---
title: Azure Batch havuzu silme başlangıç olayı | Microsoft Docs
description: Batch havuzu silme başlangıç olayı başvurusu.
services: batch
author: laurenhughes
manager: gwallace
ms.assetid: ''
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: big-compute
ms.date: 04/20/2017
ms.author: lahugh
ms.openlocfilehash: 65d20f2194b2bf83ecf32e19c1ab5b0f7bc7a004
ms.sourcegitcommit: 4b431e86e47b6feb8ac6b61487f910c17a55d121
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/18/2019
ms.locfileid: "68323207"
---
# <a name="pool-delete-start-event"></a>Havuz silme başlangıç olayı

 Bu olay, bir havuz silme işlemi başladığında yayınlanır. Havuz silme zaman uyumsuz bir olay olduğundan, silme işlemi tamamlandıktan sonra bir havuz silme tamamlandı olayının yayınlandığını bekleyebilir.

 Aşağıdaki örnekte, bir havuz silme başlangıç olayının gövdesi gösterilmektedir.

```
{
    "id": "myPool1"
}
```

|Öğe|Type|Notlar|
|-------------|----------|-----------|
|id|Dize|Havuzun kimliği.|