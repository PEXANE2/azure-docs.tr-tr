---
title: include dosyası
description: include dosyası
services: functions
author: craigshoemaker
manager: gwallace
ms.service: azure-functions
ms.topic: include
ms.date: 08/02/2019
ms.author: cshoe
ms.custom: include file
ms.openlocfilehash: 0c0ab0e62a5d951f0bc0e237f44cf55c5b8e16cc
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77202105"
---
Blobs yazmak için aşağıdaki türe bağlayabilirsiniz:

* `TextWriter`
* `out string`
* `out Byte[]`
* `CloudBlobStream`
* `Stream`
* `CloudBlobContainer`<sup>1</sup>
* `CloudBlobDirectory`
* `ICloudBlob`<sup>2</sup>
* `CloudBlockBlob`<sup>2</sup>
* `CloudPageBlob`<sup>2</sup>
* `CloudAppendBlob`<sup>2</sup>

<sup>1</sup> `direction` *function.json'da* veya `FileAccess.Read` C# sınıfı kitaplığında "in" bağlamayı gerektirir. Ancak, çalışma zamanının kapsayıcıya blobyükleme gibi yazma işlemleri yapmak için sağladığı kapsayıcı nesnesini kullanabilirsiniz.

<sup>2</sup> `direction` *function.json'da* veya `FileAccess.ReadWrite` C# sınıfı kitaplığında "inout" bağlamayı gerektirir.

Depolama SDK türlerinden birine bağlanmaya ve bir hata iletisi almaya çalışırsanız, [doğru Depolama SDK sürümüne](../articles/azure-functions/functions-bindings-storage-blob.md#azure-storage-sdk-version-in-functions-1x)bir başvurunuz olduğundan emin olun.

Async işlevlerinde, parametre `IAsyncCollector` yerine `out` iade değerini kullanın.

`string` Tüm blob içeriği belleğe yüklendiğinden, yalnızca blob boyutu küçükse veya yalnızca gerekirse bağlanma `Byte[]` önerilir. Genellikle, bir `Stream` veya `CloudBlockBlob` tür kullanmak tercih edilir. Daha fazla bilgi için, bu makalenin başlarında [Eşzamanlılık ve bellek kullanımına](../articles/azure-functions/functions-bindings-storage-blob-trigger.md#concurrency-and-memory-usage) bakın.
