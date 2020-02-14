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
ms.sourcegitcommit: 333af18fa9e4c2b376fa9aeb8f7941f1b331c11d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/13/2020
ms.locfileid: "77202105"
---
Blob 'ları yazmak için aşağıdaki türlere bağlayabilirsiniz:

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

<sup>1</sup> , bir C# Sınıf kitaplığındaki *function. JSON* veya `FileAccess.Read` içindeki "ın" bağlama `direction` gerektirir. Ancak, çalışma zamanının blob 'ları kapsayıcıya yükleme gibi yazma işlemleri için sağladığı kapsayıcı nesnesini kullanabilirsiniz.

<sup>2</sup> bir C# Sınıf kitaplığındaki *function. JSON* veya `FileAccess.ReadWrite` içindeki "Inout" bağlama `direction` gerektirir.

Depolama SDK türlerinden birini bağlamaya çalışırsanız ve bir hata iletisi alırsanız, [doğru depolama SDK sürümüne](../articles/azure-functions/functions-bindings-storage-blob.md#azure-storage-sdk-version-in-functions-1x)başvurunuz olduğundan emin olun.

Zaman uyumsuz işlevlerde, `out` parametresi yerine dönüş değeri veya `IAsyncCollector` kullanın.

`string` veya `Byte[]` bağlama yalnızca BLOB boyutu küçük olduğunda önerilir, tüm blob içerikleri belleğe yüklenir. Genellikle, bir `Stream` veya `CloudBlockBlob` türü kullanmak tercih edilir. Daha fazla bilgi için bu makalenin önceki kısımlarında yer alarak [Eşzamanlılık ve bellek kullanımı](../articles/azure-functions/functions-bindings-storage-blob-trigger.md#concurrency-and-memory-usage) bölümüne bakın.
