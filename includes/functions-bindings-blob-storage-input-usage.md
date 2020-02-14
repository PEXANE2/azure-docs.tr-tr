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
ms.openlocfilehash: 512d05b245f1279a977ba61d5b4c4904fc0f6bf2
ms.sourcegitcommit: 333af18fa9e4c2b376fa9aeb8f7941f1b331c11d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/13/2020
ms.locfileid: "77202131"
---
Blob giriş bağlaması için aşağıdaki parametre türlerini kullanabilirsiniz:

* `Stream`
* `TextReader`
* `string`
* `Byte[]`
* `CloudBlobContainer`
* `CloudBlobDirectory`
* `ICloudBlob`<sup>1</sup>
* `CloudBlockBlob`<sup>1</sup>
* `CloudPageBlob`<sup>1</sup>
* `CloudAppendBlob`<sup>1</sup>

<sup>1</sup> , bir C# Sınıf kitaplığındaki *function. JSON* veya `FileAccess.ReadWrite` içindeki "Inout" bağlamasını `direction` gerektirir.

Depolama SDK türlerinden birini bağlamaya çalışırsanız ve bir hata iletisi alırsanız, [doğru depolama SDK sürümüne](../articles/azure-functions/functions-bindings-storage-blob.md#azure-storage-sdk-version-in-functions-1x)başvurunuz olduğundan emin olun.

`string` veya `Byte[]` bağlama yalnızca BLOB boyutu küçük olduğunda önerilir, tüm blob içerikleri belleğe yüklenir. Genellikle, bir `Stream` veya `CloudBlockBlob` türü kullanmak tercih edilir. Daha fazla bilgi için bu makalenin önceki kısımlarında yer alarak [Eşzamanlılık ve bellek kullanımı](../articles/azure-functions/functions-bindings-storage-blob-trigger.md#concurrency-and-memory-usage) bölümüne bakın.
