---
title: Azure Batch havuzu silme başlangıç olayı
description: Batch havuzu silme başlangıç olayı başvurusu. Bu olay, bir havuz silme işlemi başladığında yayınlanır.
ms.topic: reference
ms.date: 12/28/2020
ms.openlocfilehash: 86f6eb8e7b269cb45f692398e9e60390375ca073
ms.sourcegitcommit: 7e97ae405c1c6c8ac63850e1b88cf9c9c82372da
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/29/2020
ms.locfileid: "97803757"
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
