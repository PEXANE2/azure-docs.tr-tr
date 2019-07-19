---
title: Azure Batch havuzu silme Tamam olayı | Microsoft Docs
description: Toplu Işlem havuzu silme Tamam olayı başvurusu.
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
ms.openlocfilehash: fd32554866d1e2130fd0833adc1b286fb6bc07a5
ms.sourcegitcommit: 4b431e86e47b6feb8ac6b61487f910c17a55d121
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/18/2019
ms.locfileid: "68323237"
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

|Öğe|Type|Notlar|
|-------------|----------|-----------|
|id|Dize|Havuzun kimliği.|
|startTime|Datetime|Havuzun silmenin başladığı zaman.|
|endTime|Datetime|Havuzun silineceği zaman tamamlandı.|

## <a name="remarks"></a>Açıklamalar
Havuz yeniden boyutlandırma işleminin durumları ve hata kodları hakkında daha fazla bilgi için bkz. bir [hesaptan havuz silme](https://docs.microsoft.com/rest/api/batchservice/delete-a-pool-from-an-account).