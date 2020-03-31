---
title: Azure Toplu İşlem görevi başarısız olayı
description: Toplu iş başarısız olayı için başvuru. Bu olay, görev tam olay ek olarak yayılan ve bir görev başarısız olduğunda algılamak için kullanılabilir.
services: batch
author: LauraBrenner
manager: evansma
ms.assetid: ''
ms.service: batch
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: big-compute
ms.date: 08/15/2019
ms.author: labrenne
ms.openlocfilehash: 2bc958d6dca2b3caae665e6f9b080c651ace9ea0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77022894"
---
# <a name="task-fail-event"></a>Görev başarısızlık olayı

 Bu olay, bir görev bir hata ile tamamlandığında yayılır. Şu anda tüm sıfır olmayan çıkış kodları hata olarak kabul edilir. Bu olay, görev tam olay *ek olarak* yayılan ve bir görev başarısız olduğunda algılamak için kullanılabilir.


 Aşağıdaki örnek, bir görev başarısız olay gövdesini gösterir.

```
{
    "jobId": "myJob",
    "id": "myTask",
    "taskType": "User",
    "systemTaskVersion": 0,
    "nodeInfo": {
        "poolId": "pool-001",
        "nodeId": "tvm-257509324_1-20160908t162728z"
    },
    "multiInstanceSettings": {
        "numberOfInstances": 1
    },
    "constraints": {
        "maxTaskRetryCount": 2
    },
    "executionInfo": {
        "startTime": "2016-09-08T16:32:23.799Z",
        "endTime": "2016-09-08T16:34:00.666Z",
        "exitCode": 1,
        "retryCount": 2,
        "requeueCount": 0
    }
}
```

|Öğe adı|Tür|Notlar|
|------------------|----------|-----------|
|`jobId`|Dize|Görevi içeren işin kimliği.|
|`id`|Dize|Görevin kimliği.|
|`taskType`|Dize|Görevin türü. Bu, bir iş yöneticisi görevi olduğunu belirten 'JobManager' veya bir iş yöneticisi görevi olmadığını belirten 'Kullanıcı' olabilir. Bu olay, iş hazırlama görevleri, iş bırakma görevleri veya başlangıç görevleri için yayınlanmaz.|
|`systemTaskVersion`|Int32|Bu, bir görevin dahili yeniden deneme sayacıdır. Dahili olarak Toplu İşlem hizmeti, geçici sorunları hesaba katmak için bir görevi yeniden deneyebilir. Bu sorunlar iç zamanlama hataları veya kötü durumda işlem düğümlerinden kurtarmak için girişimleri içerebilir.|
|[`nodeInfo`](#nodeInfo)|Karmaşık Tür|Görevin çalıştırıldığı işlem düğümü hakkında bilgi içerir.|
|[`multiInstanceSettings`](#multiInstanceSettings)|Karmaşık Tür|Görevin, birden çok işlem düğümü gerektiren çok örnekli bir görev olduğunu belirtir.  Ayrıntılar [`multiInstanceSettings`](https://docs.microsoft.com/rest/api/batchservice/get-information-about-a-task) için bkz.|
|[`constraints`](#constraints)|Karmaşık Tür|Bu göreve uygulanan yürütme kısıtlamaları.|
|[`executionInfo`](#executionInfo)|Karmaşık Tür|Görevin yürütülmesi hakkında bilgi içerir.|

###  <a name="nodeinfo"></a><a name="nodeInfo"></a>Nodeınfo

|Öğe adı|Tür|Notlar|
|------------------|----------|-----------|
|`poolId`|Dize|Görevin çalıştırıldığı havuzun kimliği.|
|`nodeId`|Dize|Görevin çalıştırıldığı düğümün kimliği.|

###  <a name="multiinstancesettings"></a><a name="multiInstanceSettings"></a>multiInstanceSettings

|Öğe adı|Tür|Notlar|
|------------------|----------|-----------|
|`numberOfInstances`|Int32|Görevin gerektirdiği işlem düğümlerinin sayısı.|

###  <a name="constraints"></a><a name="constraints"></a>Kısıtlama -ları

|Öğe adı|Tür|Notlar|
|------------------|----------|-----------|
|`maxTaskRetryCount`|Int32|Görevin en fazla kaç kez yeniden denenebilir. Toplu İşlem hizmeti, çıkış kodu sıfır değilse görevi yeniden dener.<br /><br /> Bu değerin özellikle yeniden deneme sayısını denetlediğini unutmayın. Toplu İşlem hizmeti görevi bir kez dener ve daha sonra bu sınıra kadar yeniden deneyebilir. Örneğin, en yüksek yeniden deneme sayısı 3 ise, Toplu İşlem bir görevi en fazla 4 kez (bir ilk deneme ve 3 yeniden deneme) dener.<br /><br /> En yüksek yeniden deneme sayısı 0 ise, Toplu İşlem hizmeti görevleri yeniden denemez.<br /><br /> En yüksek yeniden deneme sayısı -1 ise, Toplu İşlem hizmeti görevleri sınırsız olarak yeniden dener.<br /><br /> Varsayılan değer 0'dır (yeniden deneme yok).|


###  <a name="executioninfo"></a><a name="executionInfo"></a>Executionınfo

|Öğe adı|Tür|Notlar|
|------------------|----------|-----------|
|`startTime`|DateTime|Görevin çalışmaya başladığı saat. 'Çalışma' **çalışan** duruma karşılık gelir, bu nedenle görev kaynak dosyalarını veya uygulama paketlerini belirtirse, başlangıç saati görevin bunları indirmeye veya dağıtmaya başladığı zamanı yansıtır.  Görev yeniden başlatıldıysa veya yeniden denendiyse, bu görevin çalışmaya başladığı en son zamandır.|
|`endTime`|DateTime|Görevin tamamlandığı saat.|
|`exitCode`|Int32|Görevin çıkış kodu.|
|`retryCount`|Int32|Görevin Toplu İşlem hizmeti tarafından yeniden denenme sayısı. Görev, belirtilen MaxTaskRetryCount'a kadar sıfır olmayan bir çıkış koduyla çıkarsa yeniden denenmez.|
|`requeueCount`|Int32|Bir kullanıcı isteği sonucunda görevin Toplu İşlem hizmeti tarafından kaç kez yeniden sıralandığı.<br /><br /> Kullanıcı bir havuzdan düğümleri kaldırdığında (havuzu yeniden boyutlandırarak veya küçülterek) veya iş devre dışı bırakıldığında, kullanıcı düğümlerde çalışan görevlerin yürütme için yeniden sıraya gireceğini belirtebilir. Bu sayım, görevin bu nedenlerle kaç kez yeniden sıralandığını izler.|
