---
title: Azure Toplu İşlem havuzu tam etkinliği yeniden boyutlandır
description: Toplu toplu birleştirme için başvuru tam olay yeniden boyutlandırmak. Boyutu artan ve başarıyla tamamlanan bir havuz örneğine bakın.
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
ms.openlocfilehash: e2c66471ad9fe8d917d1ffddceb6e01c339d62dd
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77022231"
---
# <a name="pool-resize-complete-event"></a>Havuz yeniden boyutlandırma tamamlama olayı

 Bu olay, havuz yeniden boyutu tamamlandığında veya başarısız olduğunda yayılır.

 Aşağıdaki örnek, boyutu artan ve başarıyla tamamlanan bir havuz için tam olay yeniden boyutlandırma bir havuz gövdesini gösterir.

```
{
    "id": "myPool",
    "nodeDeallocationOption": "invalid",
        "currentDedicatedNodes": 10,
        "targetDedicatedNodes": 10,
    "currentLowPriorityNodes": 5,
        "targetLowPriorityNodes": 5,
    "enableAutoScale": false,
    "isAutoPool": false,
    "startTime": "2016-09-09T22:13:06.573Z",
    "endTime": "2016-09-09T22:14:01.727Z",
    "resultCode": "Success",
    "resultMessage": "The operation succeeded"
}
```

|Öğe|Tür|Notlar|
|-------------|----------|-----------|
|`id`|Dize|Havuzun kimliği.|
|`nodeDeallocationOption`|Dize|Havuz boyutu azalıyorsa düğümlerin havuzdan ne zaman çıkarılabileceği belirtilir.<br /><br /> Olası değerler şunlardır:<br /><br /> **requeue** – Çalışan görevleri sonlandırın ve yeniden sıraya girin. İş etkinleştirildiğinde görevler yeniden çalışır. Görevler sonlandırılır sonlandırılmaz düğümleri kaldırın.<br /><br /> **sona erdirmek** – Çalışan görevleri sonlandırın. Görevler yeniden çalışmaz. Görevler sonlandırılır sonlandırılmaz düğümleri kaldırın.<br /><br /> **görev tamamlama** – Şu anda çalışan görevlerin tamamlanmasına izin verin. Beklerken yeni görevler zamanlanın. Tüm görevler tamamlandığında düğümleri kaldırın.<br /><br /> **Tutulan Veriler** - Şu anda çalışan görevlerin tamamlanmasına izin verin, ardından tüm görev verileri saklama sürelerinin dolmasını bekleyin. Beklerken yeni görevler zamanlanın. Tüm görev bekletme süreleri dolduğunda düğümleri kaldırın.<br /><br /> Varsayılan değer yeniden sıralanır.<br /><br /> Havuz boyutu artıyorsa, değer **geçersiz**olarak ayarlanır.|
|`currentDedicatedNodes`|Int32|Şu anda havuza atanan özel işlem düğümlerinin sayısı.|
|`targetDedicatedNodes`|Int32|Havuz için istenen özel işlem düğümlerinin sayısı.|
|`currentLowPriorityNodes`|Int32|Şu anda havuza atanan düşük öncelikli işlem düğümlerinin sayısı.|
|`targetLowPriorityNodes`|Int32|Havuz için istenen düşük öncelikli işlem düğümlerinin sayısı.|
|`enableAutoScale`|Bool|Havuz boyutunun zaman içinde otomatik olarak ayarlanıp ayarlanmadığını belirtir.|
|`isAutoPool`|Bool|Havuzun bir işin AutoPool mekanizması aracılığıyla oluşturulup oluşturulmadığını belirtir.|
|`startTime`|DateTime|Havuzun yeniden boyutlandırılma zamanı.|
|`endTime`|DateTime|Havuzun yeniden boyutlandırılan zamanı.|
|`resultCode`|Dize|Yeniden boyutlandırmanın sonucu.|
|`resultMessage`|Dize| Sonuç hakkında ayrıntılı bir ileti.<br /><br /> Yeniden boyutlandırma başarıyla tamamlandıysa, işlemin başarılı olduğunu belirtir.|
