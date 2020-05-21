---
title: Azure Batch havuzu silme başlangıç olayı
description: Batch havuzu silme başlangıç olayı başvurusu. Bu olay, bir havuz silme işlemi başladığında yayınlanır.
ms.topic: reference
ms.date: 04/20/2017
ms.openlocfilehash: d4fac833b6c77258e48b38838f6b7b133738a6e8
ms.sourcegitcommit: 6fd8dbeee587fd7633571dfea46424f3c7e65169
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/21/2020
ms.locfileid: "83723944"
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
