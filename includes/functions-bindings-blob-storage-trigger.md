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
ms.openlocfilehash: 4a879c4041fe317955a07eda9dd8a3ef9f542275
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87056044"
---
Tetikleme blobu için aşağıdaki parametre türlerini kullanabilirsiniz:

* `Stream`
* `TextReader`
* `string`
* `Byte[]`
* `ICloudBlob`<sup>1</sup>
* `CloudBlockBlob`<sup>1</sup>
* `CloudPageBlob`<sup>1</sup>
* `CloudAppendBlob`<sup>1</sup>

<sup>1</sup> `direction` , bir C# sınıf kitaplığındaki veya *üzerindefunction.js* "Inout" bağlamasını gerektirir `FileAccess.ReadWrite` .

Depolama SDK türlerinden birini bağlamaya çalışırsanız ve bir hata iletisi alırsanız, [doğru depolama SDK sürümüne](../articles/azure-functions/functions-bindings-storage-blob.md#azure-storage-sdk-version-in-functions-1x)başvurunuz olduğundan emin olun.

`string` `Byte[]` Blob içeriğinin tamamı belleğe yüklendiği için, veya öğesine bağlama yalnızca BLOB boyutu küçük olduğunda önerilir. Genellikle, veya türünde kullanılması tercih edilir `Stream` `CloudBlockBlob` . Daha fazla bilgi için bu makalenin ilerleyen kısımlarında [Eşzamanlılık ve bellek kullanımı](../articles/azure-functions/functions-bindings-storage-blob-trigger.md#concurrency-and-memory-usage) bölümüne bakın.