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
ms.openlocfilehash: fa3888d28c52625684676036def7e2920b77b5ca
ms.sourcegitcommit: 333af18fa9e4c2b376fa9aeb8f7941f1b331c11d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/13/2020
ms.locfileid: "77202118"
---
Tetikleme blobu için aşağıdaki parametre türlerini kullanabilirsiniz:

* `Stream`
* `TextReader`
* `string`
* `Byte[]`
* JSON olarak seri hale getirilebilir bir POCO
* `ICloudBlob`<sup>1</sup>
* `CloudBlockBlob`<sup>1</sup>
* `CloudPageBlob`<sup>1</sup>
* `CloudAppendBlob`<sup>1</sup>

<sup>1</sup> , bir C# Sınıf kitaplığındaki *function. JSON* veya `FileAccess.ReadWrite` içindeki "Inout" bağlamasını `direction` gerektirir.

Depolama SDK türlerinden birini bağlamaya çalışırsanız ve bir hata iletisi alırsanız, [doğru depolama SDK sürümüne](../articles/azure-functions/functions-bindings-storage-blob.md#azure-storage-sdk-version-in-functions-1x)başvurunuz olduğundan emin olun.

`string`, `Byte[]`veya POCO 'a bağlama yalnızca BLOB boyutu küçük olduğunda önerilir, çünkü tüm blob içerikleri belleğe yüklenir. Genellikle, bir `Stream` veya `CloudBlockBlob` türü kullanmak tercih edilir. Daha fazla bilgi için bu makalenin ilerleyen kısımlarında [Eşzamanlılık ve bellek kullanımı](../articles/azure-functions/functions-bindings-storage-blob-trigger.md#concurrency-and-memory-usage) bölümüne bakın.