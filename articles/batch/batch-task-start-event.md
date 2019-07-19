---
title: Azure Batch görevi başlatma olayı | Microsoft Docs
description: Batch görevi başlatma olayı için başvuru.
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
ms.openlocfilehash: 76100f1457123ac88055fddd55eb22a102201adf
ms.sourcegitcommit: 4b431e86e47b6feb8ac6b61487f910c17a55d121
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/18/2019
ms.locfileid: "68322815"
---
# <a name="task-start-event"></a>Görev başlangıç olayı

 Bu olay, bir Görev Zamanlayıcı tarafından bir işlem düğümünde başlamak üzere zamanlandıktan sonra yayınlanır. Görev yeniden deneniyorsa veya yeniden kuyruğa bu olay aynı görev için tekrar yayınlanacaktır, ancak yeniden deneme sayısı ve sistem görevinin sürümü uygun şekilde güncelleştirilir.


 Aşağıdaki örnekte, bir görev başlatma olayının gövdesi gösterilmektedir.

```
{
    "jobId": "job-0000000001",
    "id": "task-5",
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
        "retryCount": 0
    }
}
```

|Öğe adı|Type|Notlar|
|------------------|----------|-----------|
|No|Dize|Görevi içeren işin kimliği.|
|id|Dize|Görevin kimliği.|
|taskType|Dize|Görevin türü. Bu, bir iş Yöneticisi görevi olduğunu ya da bir iş Yöneticisi görevi olmadığını belirten ' user ' olduğunu belirten ' JobManager ' olabilir.|
|systemTaskVersion|Int32|Bu, bir görevde iç yeniden deneme sayacıdır. Toplu olarak Batch hizmeti, geçici sorunlar için bir görevi hesaba yeniden deneyebilir. Bu sorunlar, iç zamanlama hataları içerebilir veya işlem düğümlerinden hatalı bir durumda kurtarmaya çalışır.|
|[nodeInfo](#nodeInfo)|Karmaşık tür|Görevin çalıştırıldığı işlem düğümüyle ilgili bilgiler içerir.|
|[multiInstanceSettings](#multiInstanceSettings)|Karmaşık tür|Görevin birden çok işlem düğümü gerektiren çok örnekli bir görev olduğunu belirtir.  Ayrıntılar için bkz. [Multiınstancesettings](https://docs.microsoft.com/rest/api/batchservice/get-information-about-a-task) .|
|[kısıtlamaları](#constraints)|Karmaşık tür|Bu görev için uygulanan yürütme kısıtlamaları.|
|[executionInfo](#executionInfo)|Karmaşık tür|Görevin yürütülmesi hakkındaki bilgileri içerir.|

###  <a name="nodeInfo"></a>NodeInfo

|Öğe adı|Type|Notlar|
|------------------|----------|-----------|
|poolId|Dize|Görevin çalıştırıldığı havuzun kimliği.|
|NodeId|Dize|Görevin çalıştırıldığı düğümün kimliği.|

###  <a name="multiInstanceSettings"></a>Multiınstancesettings

|Öğe adı|Type|Notlar|
|------------------|----------|-----------|
|Numberofınstances|Int|Görevin gerektirdiği işlem düğümü sayısı.|

###  <a name="constraints"></a>kısıtlamaları

|Öğe adı|Type|Notlar|
|------------------|----------|-----------|
|maxTaskRetryCount|Int32|Görevin en fazla yeniden denenme sayısı. Batch hizmeti, çıkış kodu sıfır değilse bir görevi yeniden dener.<br /><br /> Bu değerin özellikle yeniden deneme sayısını denetlediğine unutmayın. Batch hizmeti görevi bir kez dener ve daha sonra bu sınıra kadar yeniden deneyebilir. Örneğin, en fazla yeniden deneme sayısı 3 ise Batch, 4 kata kadar bir görevi dener (bir başlangıç denemesi ve 3 yeniden deneme).<br /><br /> En fazla yeniden deneme sayısı 0 ise, Batch hizmeti görevleri yeniden denemeyecek.<br /><br /> En fazla yeniden deneme sayısı-1 ise, Batch hizmeti görevleri sınırlı olmadan yeniden dener.<br /><br /> Varsayılan değer 0 ' dır (yeniden deneme yok).|

###  <a name="executionInfo"></a>ExecutionInfo

|Öğe adı|Type|Notlar|
|------------------|----------|-----------|
|retryCount|Int32|Görevin Batch hizmeti tarafından yeniden denenme sayısı. Bu görev, belirtilen MaxTaskRetryCount 'a kadar sıfır olmayan çıkış kodu ile çıktıktan sonra yeniden denenir|
