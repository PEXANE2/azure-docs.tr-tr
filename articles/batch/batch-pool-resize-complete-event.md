---
title: Azure Batch havuzu yeniden boyutlandırma Tamam olayı
description: Batch havuzu yeniden boyutlandırma için başvuru Tamam olayı. Boyut arttığı ve başarıyla tamamlanan bir havuzun örneğine bakın.
ms.topic: reference
ms.date: 04/20/2017
ms.openlocfilehash: 94301f29fb6e7968dbe0389754fcf2a3b105d7ef
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "83723825"
---
# <a name="pool-resize-complete-event"></a>Havuz yeniden boyutlandırma tamamlama olayı

 Bu olay, bir havuz yeniden boyutlandırılması tamamlandığında veya başarısız olduğunda yayınlanır.

 Aşağıdaki örnek, boyutu arttığı ve başarıyla tamamlanan bir havuz için havuz yeniden boyutlandırma tamamlandı olayının gövdesini gösterir.

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
|`id`|Dize|Havuzun KIMLIĞI.|
|`nodeDeallocationOption`|Dize|Havuz boyutu azaltılmışsa, havuzdan düğümlerin ne zaman kaldırılabileceği belirtir.<br /><br /> Olası değerler şunlardır:<br /><br /> **yeniden kuyruğa alma** – çalışan görevleri sonlandırır ve yeniden kuyruğa alma onları sonlandırın. Görevler, iş etkinleştirildiğinde yeniden çalışacaktır. Görevler sonlandırıldıktan hemen sonra düğümleri kaldırın.<br /><br /> **Sonlandır** – çalışan görevleri sonlandırın. Görevler yeniden çalıştırılmaz. Görevler sonlandırıldıktan hemen sonra düğümleri kaldırın.<br /><br /> **taskcompletion** : Şu anda çalışan görevlerin tamamlanmasına izin verin. Beklerken yeni görev zamanlayın. Tüm görevler tamamlandığında düğümleri kaldırın.<br /><br /> **Retaineddata** -çalışmakta olan görevlerin tamamlanmasına izin verin, ardından tüm görev verilerini bekletme dönemlerinin süre sonu için bekleyin. Beklerken yeni görev zamanlayın. Tüm görev bekletme dönemlerinin süresi dolduğunda düğümleri kaldırın.<br /><br /> Varsayılan değer yeniden kuyruğa alma ' dir.<br /><br /> Havuz boyutu artarak değer **geçersiz**olarak ayarlanır.|
|`currentDedicatedNodes`|Int32|Havuza atanmış olan ayrılmış işlem düğümlerinin sayısı.|
|`targetDedicatedNodes`|Int32|Havuz için istenen ayrılmış işlem düğümlerinin sayısı.|
|`currentLowPriorityNodes`|Int32|Havuza atanmış olan düşük öncelikli işlem düğümlerinin sayısı.|
|`targetLowPriorityNodes`|Int32|Havuz için istenen düşük öncelikli işlem düğümlerinin sayısı.|
|`enableAutoScale`|Bool|Havuz boyutunun zaman içinde otomatik olarak görüntülenip görüntülenmeyeceğini belirtir.|
|`isAutoPool`|Bool|Havuzun bir işin oto havuzu mekanizması aracılığıyla oluşturulup oluşturulmayacağını belirtir.|
|`startTime`|DateTime|Havuz yeniden boyutlandırmanın başladığı zaman.|
|`endTime`|DateTime|Havuzun yeniden boyutlandırılması zamanı tamamlandı.|
|`resultCode`|Dize|Yeniden boyutlandırmanın sonucu.|
|`resultMessage`|Dize| Sonuç hakkında ayrıntılı bir ileti.<br /><br /> Yeniden boyutlandırma başarıyla tamamlanırsa işlemin başarılı olduğunu bildirir.|
