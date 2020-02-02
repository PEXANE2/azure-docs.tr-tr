---
title: Azure Batch havuzu silme Tamam olayı
description: Toplu Işlem havuzu silme Tamam olayı başvurusu. Bu olay, bir havuz silme işlemi tamamlandığında yayınlanır.
services: batch
author: ju-shim
manager: gwallace
ms.assetid: ''
ms.service: batch
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: big-compute
ms.date: 04/20/2017
ms.author: jushiman
ms.openlocfilehash: 56dc4c91566cc5c41de6c91618d09e7a0ebb7172
ms.sourcegitcommit: fa6fe765e08aa2e015f2f8dbc2445664d63cc591
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/01/2020
ms.locfileid: "76929851"
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
|`startTime`|Tarih Saat|Havuzun silmenin başladığı zaman.|
|`endTime`|Tarih Saat|Havuzun silineceği zaman tamamlandı.|

## <a name="remarks"></a>Açıklamalar
Havuz yeniden boyutlandırma işleminin durumları ve hata kodları hakkında daha fazla bilgi için bkz. bir [hesaptan havuz silme](https://docs.microsoft.com/rest/api/batchservice/delete-a-pool-from-an-account).
