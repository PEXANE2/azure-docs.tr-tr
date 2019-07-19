---
title: Azure Batch havuzu yeniden boyutlandırma başlangıç olayı | Microsoft Docs
description: Batch havuzu yeniden boyutlandırma başlangıç olayı başvurusu.
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
ms.openlocfilehash: c7ef6bb9550b7398a10f5b57e6009d896256666b
ms.sourcegitcommit: 4b431e86e47b6feb8ac6b61487f910c17a55d121
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/18/2019
ms.locfileid: "68323187"
---
# <a name="pool-resize-start-event"></a>Havuz yeniden boyutlandırma başlangıç olayı

 Bu olay, bir havuz yeniden boyutlandırmaya başladığında yayınlanır. Havuz yeniden boyutlandırılması zaman uyumsuz bir olay olduğundan, yeniden boyutlandırma işlemi tamamlandıktan sonra havuzun yeniden boyutlandırılması tamamlandı olayını bekleyebilir.

 Aşağıdaki örnek, el ile yeniden boyutlandırmayla 0 ' dan 2 ' ye kadar düğüm yeniden boyutlandırılması için bir havuz yeniden boyutlandırma başlangıç olayının gövdesini gösterir.

```
{
    "poolId": "myPool1",
    "nodeDeallocationOption": "invalid",
    "currentDedicated": 0,
    "targetDedicated": 2,
    "enableAutoScale": false,
    "isAutoPool": false
}
```

|Öğe|Type|Notlar|
|-------------|----------|-----------|
|poolId|Dize|Havuzun kimliği.|
|Nodeanlaşlocationoption|Dize|Havuz boyutu azaltılmışsa, havuzdan düğümlerin ne zaman kaldırılabileceği belirtir.<br /><br /> Olası değerler şunlardır:<br /><br /> **yeniden kuyruğa alma** – çalışan görevleri sonlandırır ve yeniden kuyruğa alma onları sonlandırın. Görevler, iş etkinleştirildiğinde yeniden çalışacaktır. Görevler sonlandırıldıktan hemen sonra düğümleri kaldırın.<br /><br /> **Sonlandır** – çalışan görevleri sonlandırın. Görevler yeniden çalıştırılmaz. Görevler sonlandırıldıktan hemen sonra düğümleri kaldırın.<br /><br /> **taskcompletion** : Şu anda çalışan görevlerin tamamlanmasına izin verin. Beklerken yeni görev zamanlayın. Tüm görevler tamamlandığında düğümleri kaldırın.<br /><br /> **Retaineddata** -çalışmakta olan görevlerin tamamlanmasına izin verin, ardından tüm görev verilerini bekletme dönemlerinin süre sonu için bekleyin. Beklerken yeni görev zamanlayın. Tüm görev bekletme dönemlerinin süresi dolduğunda düğümleri kaldırın.<br /><br /> Varsayılan değer yeniden kuyruğa alma ' dir.<br /><br /> Havuz boyutu artarak değer **geçersiz**olarak ayarlanır.|
|Currentadanmış|Int32|Havuza Şu anda atanmış olan işlem düğümlerinin sayısı.|
|targetDedicated|Int32|Havuz için istenen işlem düğümlerinin sayısı.|
|Enableotomatik ölçeklendirme|Bool|Havuz boyutunun zaman içinde otomatik olarak görüntülenip görüntülenmeyeceğini belirtir.|
|isAutoPool|Bool|Havuzun bir işin oto havuzu mekanizması aracılığıyla oluşturulup oluşturulmayacağını belirtir.|
