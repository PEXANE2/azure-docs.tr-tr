---
title: Azure Toplu İşlem havuzu tüm olayı silme
description: Toplu iş havuzu için başvuru, tüm olayı siler. Bu olay, bir havuz silme işlemi tamamlandığında yayılır.
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
ms.openlocfilehash: d317d7395a8246c109073a72338b55960cb50954
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77023625"
---
# <a name="pool-delete-complete-event"></a>Havuz silme tamamlama olayı

 Bu olay, bir havuz silme işlemi tamamlandığında yayılır.

 Aşağıdaki örnek, bir havuzun gövdesini tam olayı siler.

```
{
    "id": "myPool1",
    "startTime": "2016-09-09T22:13:48.579Z",
    "endTime": "2016-09-09T22:14:08.836Z"
}
```

|Öğe|Tür|Notlar|
|-------------|----------|-----------|
|`id`|Dize|Havuzun kimliği.|
|`startTime`|DateTime|Havuz silmenin başladığı saat.|
|`endTime`|DateTime|Havuzun silinme süresi.|

## <a name="remarks"></a>Açıklamalar
Havuz yeniden boyutlandırma işlemi için durumlar ve hata kodları hakkında daha fazla bilgi için [bkz.](https://docs.microsoft.com/rest/api/batchservice/delete-a-pool-from-an-account)
