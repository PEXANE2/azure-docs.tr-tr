---
title: Azure Batch havuzu yeniden boyutlandırma Tamam olayı | Microsoft Docs
description: Batch havuzu yeniden boyutlandırma için başvuru Tamam olayı.
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
ms.openlocfilehash: c2544bd2be683b731c3dac0bea651d4b64dff75e
ms.sourcegitcommit: 4b431e86e47b6feb8ac6b61487f910c17a55d121
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/18/2019
ms.locfileid: "68323199"
---
# <a name="pool-resize-complete-event"></a>Havuz yeniden boyutlandırma tamamlama olayı

 Bu olay, bir havuz yeniden boyutlandırılması tamamlandığında veya başarısız olduğunda yayınlanır.

 Aşağıdaki örnek, boyutu arttığı ve başarıyla tamamlanan bir havuz için havuz yeniden boyutlandırma tamamlandı olayının gövdesini gösterir.

```
{
    "id": "p_1_0_01503750-252d-4e57-bd96-d6aa05601ad8",
    "nodeDeallocationOption": "invalid",
    "currentDedicated": 4,
    "targetDedicated": 4,
    "enableAutoScale": false,
    "isAutoPool": false,
    "startTime": "2016-09-09T22:13:06.573Z",
    "endTime": "2016-09-09T22:14:01.727Z",
    "result": "Success",
    "resizeError": "The operation succeeded"
}
```

|Öğe|Type|Notlar|
|-------------|----------|-----------|
|id|Dize|Havuzun kimliği.|
|Nodeanlaşlocationoption|Dize|Havuz boyutu azaltılmışsa, havuzdan düğümlerin ne zaman kaldırılabileceği belirtir.<br /><br /> Olası değerler şunlardır:<br /><br /> **yeniden kuyruğa alma** – çalışan görevleri sonlandırır ve yeniden kuyruğa alma onları sonlandırın. Görevler, iş etkinleştirildiğinde yeniden çalışacaktır. Görevler sonlandırıldıktan hemen sonra düğümleri kaldırın.<br /><br /> **Sonlandır** – çalışan görevleri sonlandırın. Görevler yeniden çalıştırılmaz. Görevler sonlandırıldıktan hemen sonra düğümleri kaldırın.<br /><br /> **taskcompletion** : Şu anda çalışan görevlerin tamamlanmasına izin verin. Beklerken yeni görev zamanlayın. Tüm görevler tamamlandığında düğümleri kaldırın.<br /><br /> **Retaineddata** -çalışmakta olan görevlerin tamamlanmasına izin verin, ardından tüm görev verilerini bekletme dönemlerinin süre sonu için bekleyin. Beklerken yeni görev zamanlayın. Tüm görev bekletme dönemlerinin süresi dolduğunda düğümleri kaldırın.<br /><br /> Varsayılan değer yeniden kuyruğa alma ' dir.<br /><br /> Havuz boyutu artarak değer **geçersiz**olarak ayarlanır.|
|Currentadanmış|Int32|Havuza Şu anda atanmış olan işlem düğümlerinin sayısı.|
|targetDedicated|Int32|Havuz için istenen işlem düğümlerinin sayısı.|
|Enableotomatik ölçeklendirme|Bool|Havuz boyutunun zaman içinde otomatik olarak görüntülenip görüntülenmeyeceğini belirtir.|
|isAutoPool|Bool|Havuzun bir işin oto havuzu mekanizması aracılığıyla oluşturulup oluşturulmayacağını belirtir.|
|startTime|Datetime|Havuz yeniden boyutlandırmanın başladığı zaman.|
|endTime|Datetime|Havuzun yeniden boyutlandırılması zamanı tamamlandı.|
|resultCode|Dize|Yeniden boyutlandırmanın sonucu.|
|Sonuç iletisi|Dize|Yeniden boyutlandırma hatası, sonucun ayrıntılarını içerir.<br /><br /> Yeniden boyutlandırma başarıyla tamamlanırsa işlemin başarılı olduğunu bildirir.|