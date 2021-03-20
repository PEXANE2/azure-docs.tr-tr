---
title: Azure Batch havuzu silme Tamam olayı
description: Toplu Işlem havuzu silme Tamam olayı başvurusu. Bu olay, bir havuz silme işlemi tamamlandığında yayınlanır.
ms.topic: reference
ms.date: 12/28/2020
ms.openlocfilehash: be6411a150ae6be424c0621eed768157154c7408
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "97803740"
---
# <a name="pool-delete-complete-event"></a>Havuz silme tamamlama olayı

 Bu olay, bir havuz silme işlemi tamamlandığında yayınlanır.

 Aşağıdaki örnekte, bir havuz silme Tamam olayının gövdesi gösterilmektedir.

```
{
   "id": "myPool1",
   "startTime": "2016-09-09T22:13:48.579Z",
   "endTime": "2016-09-09T22:14:08.836Z"
}
```

|Öğe|Tür|Notlar|
|-------------|----------|-----------|
|`id`|Dize|Havuzun KIMLIĞI.|
|`startTime`|DateTime|Havuzun silmenin başladığı zaman.|
|`endTime`|DateTime|Havuzun silineceği zaman tamamlandı.|

## <a name="remarks"></a>Açıklamalar

Havuz yeniden boyutlandırma işleminin durumları ve hata kodları hakkında daha fazla bilgi için bkz. bir [hesaptan havuz silme](/rest/api/batchservice/delete-a-pool-from-an-account).
