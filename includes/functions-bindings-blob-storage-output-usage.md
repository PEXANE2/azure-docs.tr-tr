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
ms.openlocfilehash: 09e56395122f3be897d8bfb34b1d51cee034b505
ms.sourcegitcommit: 4295037553d1e407edeb719a3699f0567ebf4293
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/30/2020
ms.locfileid: "96356236"
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

<sup>1</sup> `direction` , C# sınıf kitaplığındaki veya *üzerindefunction.js* içinde "ın" bağlamasını gerektirir `FileAccess.Read` . Ancak, çalışma zamanının blob 'ları kapsayıcıya yükleme gibi yazma işlemleri için sağladığı kapsayıcı nesnesini kullanabilirsiniz.

<sup>2</sup> `direction` , bir C# sınıf kitaplığındaki veya *üzerindefunction.js* "Inout" bağlamasını gerektirir `FileAccess.ReadWrite` .

Depolama SDK türlerinden birini bağlamaya çalışırsanız ve bir hata iletisi alırsanız, [doğru depolama SDK sürümüne](../articles/azure-functions/functions-bindings-storage-blob.md#azure-storage-sdk-version-in-functions-1x)başvurunuz olduğundan emin olun.

`string`Veya `Byte[]` ' a bağlama yalnızca BLOB boyutu küçük olduğunda, Blob içeriğinin tamamı belleğe yüklendiği için önerilir. Genellikle, veya türünde kullanılması tercih edilir `Stream` `CloudBlockBlob` . Daha fazla bilgi için bu makalenin önceki kısımlarında yer alarak [Eşzamanlılık ve bellek kullanımı](../articles/azure-functions/functions-bindings-storage-blob-trigger.md#concurrency-and-memory-usage) bölümüne bakın.
