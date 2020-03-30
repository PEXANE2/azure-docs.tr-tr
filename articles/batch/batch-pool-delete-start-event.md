---
title: Azure Toplu Birleştirme havuzu silme başlangıç olayı
description: Toplu iş havuzu silme başlangıç olayı için başvuru. Bu olay, bir havuz silme işlemi başladığında yayılır.
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77022214"
---
# <a name="pool-delete-start-event"></a>Havuz silme başlangıç olayı

 Bu olay, bir havuz silme işlemi başladığında yayılır. Havuz silme işlemi bir eşzamanlı olay olduğundan, silme işlemi tamamlandıktan sonra bir havuz silme olayının tamamının yayımlatIlmesini bekleyebilirsiniz.

 Aşağıdaki örnek, havuz silme başlangıç olayının gövdesini gösterir.

```
{
    "id": "myPool1"
}
```

|Öğe|Tür|Notlar|
|-------------|----------|-----------|
|`id`|Dize|Havuzun kimliği.|
