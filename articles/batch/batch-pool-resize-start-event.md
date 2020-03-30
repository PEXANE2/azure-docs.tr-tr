---
title: Azure Toplu Toplu Birleştirme havuzu yeniden boyutlandırma etkinliği
description: Toplu küme yeniden boyutlandırma başlat olayı için başvuru. Örnek, 0'dan 2 düğümlere yeniden boyutlandırılatan bir havuz yeniden boyutlandırma olayının gövdesini gösterir.
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
ms.openlocfilehash: 1866e51da30fe5ed148d019c8720755e99757df7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77023591"
---
# <a name="pool-resize-start-event"></a>Havuz yeniden boyutlandırma başlangıç olayı

 Bu olay, bir havuz yeniden boyutlandırması başladığında yayılır. Havuz yeniden boyutlandırması bir eşzamanlı olay olduğundan, yeniden boyutlandırma işlemi tamamlandıktan sonra bir havuz yeniden boyutlandırma olayının yayımını alabilirsiniz.

 Aşağıdaki örnek, 0'dan 2 düğümlere yeniden boyutlandırılatan bir havuz yeniden boyutlandırma olayının gövdesini gösterir.

```
{
    "id": "myPool1",
    "nodeDeallocationOption": "Invalid",
    "currentDedicatedNodes": 0,
    "targetDedicatedNodes": 2,
    "currentLowPriorityNodes": 0,
    "targetLowPriorityNodes": 2,
    "enableAutoScale": false,
    "isAutoPool": false
}
```

|Öğe|Tür|Notlar|
|-------------|----------|-----------|
|`id`|Dize|Havuzun kimliği.|
|`nodeDeallocationOption`|Dize|Havuz boyutu azalıyorsa düğümlerin havuzdan ne zaman çıkarılabileceği belirtilir.<br /><br /> Olası değerler şunlardır:<br /><br /> **requeue** – Çalışan görevleri sonlandırın ve yeniden sıraya girin. İş etkinleştirildiğinde görevler yeniden çalışır. Görevler sonlandırılır sonlandırılmaz düğümleri kaldırın.<br /><br /> **sona erdirmek** – Çalışan görevleri sonlandırın. Görevler yeniden çalışmaz. Görevler sonlandırılır sonlandırılmaz düğümleri kaldırın.<br /><br /> **görev tamamlama** – Şu anda çalışan görevlerin tamamlanmasına izin verin. Beklerken yeni görevler zamanlanın. Tüm görevler tamamlandığında düğümleri kaldırın.<br /><br /> **Tutulan Veriler** - Şu anda çalışan görevlerin tamamlanmasına izin verin, ardından tüm görev verileri saklama sürelerinin dolmasını bekleyin. Beklerken yeni görevler zamanlanın. Tüm görev bekletme süreleri dolduğunda düğümleri kaldırın.<br /><br /> Varsayılan değer yeniden sıralanır.<br /><br /> Havuz boyutu artıyorsa, değer **geçersiz**olarak ayarlanır.|
|`currentDedicatedNodes`|Int32|Şu anda havuza atanan işlem düğümlerinin sayısı.|
|`targetDedicatedNodes`|Int32|Havuz için istenen işlem düğümü sayısı.|
|`currentLowPriorityNodes`|Int32|Şu anda havuza atanan işlem düğümlerinin sayısı.|
|`targetLowPriorityNodes`|Int32|Havuz için istenen işlem düğümü sayısı.|
|`enableAutoScale`|Bool|Havuz boyutunun zaman içinde otomatik olarak ayarlanıp ayarlanmadığını belirtir.|
|`isAutoPool`|Bool|Havuzun bir işin AutoPool mekanizması aracılığıyla oluşturulup oluşturulmadığını belirtir.|
