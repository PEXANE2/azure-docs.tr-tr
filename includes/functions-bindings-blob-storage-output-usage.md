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
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
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
* `ICloudBlob`<sup>iki</sup>
* `CloudBlockBlob`<sup>iki</sup>
* `CloudPageBlob`<sup>iki</sup>
* `CloudAppendBlob`<sup>iki</sup>

<sup>1</sup> , *function. JSON* `FileAccess.Read` içinde `direction` veya C# sınıf kitaplığında "ın" bağlamasını gerektirir. Ancak, çalışma zamanının blob 'ları kapsayıcıya yükleme gibi yazma işlemleri için sağladığı kapsayıcı nesnesini kullanabilirsiniz.

<sup>2</sup> , *function. JSON* içinde `FileAccess.ReadWrite` veya `direction` C# sınıf kitaplığında "Inout" bağlamayı gerektirir.

Depolama SDK türlerinden birini bağlamaya çalışırsanız ve bir hata iletisi alırsanız, [doğru depolama SDK sürümüne](../articles/azure-functions/functions-bindings-storage-blob.md#azure-storage-sdk-version-in-functions-1x)başvurunuz olduğundan emin olun.

Zaman uyumsuz işlevlerde, dönüş değeri veya `IAsyncCollector` bir `out` parametre yerine kullanın.

Veya `Byte[]` ' `string` a bağlama yalnızca BLOB boyutu küçük olduğunda, Blob içeriğinin tamamı belleğe yüklendiği için önerilir. Genellikle, `Stream` veya `CloudBlockBlob` türünde kullanılması tercih edilir. Daha fazla bilgi için bu makalenin önceki kısımlarında yer alarak [Eşzamanlılık ve bellek kullanımı](../articles/azure-functions/functions-bindings-storage-blob-trigger.md#concurrency-and-memory-usage) bölümüne bakın.
