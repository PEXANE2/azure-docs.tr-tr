---
title: Azure Toplu Iş Parti başlangıç olayı
description: Toplu iş başlatma olayı için başvuru bilgileri. Bir görev zamanlayıcı tarafından bir işlem düğümünde başlamak üzere zamanlandıktan sonra bu olay yayımlanır.
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
ms.openlocfilehash: bed3749e29867298f3e8258a08448b7b094055ec
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77022826"
---
# <a name="task-start-event"></a>Görev başlangıç olayı

 Bir görev zamanlayıcı tarafından bir işlem düğümünde başlamak üzere zamanlandıktan sonra bu olay yayımlanır. Görev yeniden denenir veya yeniden sıralanırsa, bu olay aynı görev için yeniden yayımlanır, ancak yeniden deneme sayısı ve sistem görev sürümü buna göre güncelleştirilir.


 Aşağıdaki örnek, görev başlatma olayının gövdesini gösterir.

```
{
    "jobId": "myJob",
    "id": "myTask",
    "taskType": "User",
    "systemTaskVersion": 220192842,
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
        "retryCount": 0
    }
}
```

|Öğe adı|Tür|Notlar|
|------------------|----------|-----------|
|`jobId`|Dize|Görevi içeren işin kimliği.|
|`id`|Dize|Görevin kimliği.|
|`taskType`|Dize|Görevin türü. Bu, bir iş yöneticisi görevi olduğunu belirten 'JobManager' veya bir iş yöneticisi görevi olmadığını belirten 'Kullanıcı' olabilir.|
|`systemTaskVersion`|Int32|Bu, bir görevin dahili yeniden deneme sayacıdır. Dahili olarak Toplu İşlem hizmeti, geçici sorunları hesaba katmak için bir görevi yeniden deneyebilir. Bu sorunlar iç zamanlama hataları veya kötü durumda işlem düğümlerinden kurtarmak için girişimleri içerebilir.|
|[`nodeInfo`](#nodeInfo)|Karmaşık Tür|Görevin çalıştırıldığı işlem düğümü hakkında bilgi içerir.|
|[`multiInstanceSettings`](#multiInstanceSettings)|Karmaşık Tür|Görevin, birden çok işlem düğümü gerektiren Çok Örnekli Görev olduğunu belirtir.  Ayrıntılar için [çoklu InstanceSettings'e](https://docs.microsoft.com/rest/api/batchservice/get-information-about-a-task) bakın.|
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
|`numberOfInstances`|int|Görevin gerektirdiği işlem düğümlerinin sayısı.|

###  <a name="constraints"></a><a name="constraints"></a>Kısıtlama -ları

|Öğe adı|Tür|Notlar|
|------------------|----------|-----------|
|`maxTaskRetryCount`|Int32|Görevin en fazla kaç kez yeniden denenebilir. Toplu İşlem hizmeti, çıkış kodu sıfır değilse görevi yeniden dener.<br /><br /> Bu değerin özellikle yeniden deneme sayısını denetlediğini unutmayın. Toplu İşlem hizmeti görevi bir kez dener ve daha sonra bu sınıra kadar yeniden deneyebilir. Örneğin, en yüksek yeniden deneme sayısı 3 ise, Toplu İşlem bir görevi en fazla 4 kez (bir ilk deneme ve 3 yeniden deneme) dener.<br /><br /> En yüksek yeniden deneme sayısı 0 ise, Toplu İşlem hizmeti görevleri yeniden denemez.<br /><br /> En yüksek yeniden deneme sayısı -1 ise, Toplu İşlem hizmeti görevleri sınırsız olarak yeniden dener.<br /><br /> Varsayılan değer 0'dır (yeniden deneme yok).|

###  <a name="executioninfo"></a><a name="executionInfo"></a>Executionınfo

|Öğe adı|Tür|Notlar|
|------------------|----------|-----------|
|`retryCount`|Int32|Görevin Toplu İşlem hizmeti tarafından yeniden denenme sayısı. Belirtilen MaxTaskRetryCount'a kadar sıfır olmayan bir çıkış koduyla çıkarsa görev yeniden denendi|
