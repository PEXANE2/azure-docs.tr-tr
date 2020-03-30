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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77202118"
---
Tetikleme blob için aşağıdaki parametre türlerini kullanabilirsiniz:

* `Stream`
* `TextReader`
* `string`
* `Byte[]`
* JSON olarak serileştirilebilir bir POCO
* `ICloudBlob`<sup>1</sup>
* `CloudBlockBlob`<sup>1</sup>
* `CloudPageBlob`<sup>1</sup>
* `CloudAppendBlob`<sup>1</sup>

<sup>1</sup> `direction` *function.json'da* veya `FileAccess.ReadWrite` C# sınıfı kitaplığında "inout" bağlamayı gerektirir.

Depolama SDK türlerinden birine bağlanmaya ve bir hata iletisi almaya çalışırsanız, [doğru Depolama SDK sürümüne](../articles/azure-functions/functions-bindings-storage-blob.md#azure-storage-sdk-version-in-functions-1x)bir başvurunuz olduğundan emin olun.

Tüm `string`blob içeriği belleğe yüklendiğinden, yalnızca blob boyutu küçükse , POCO'ya `Byte[]`veya POCO'ya bağlama önerilir. Genellikle, bir `Stream` veya `CloudBlockBlob` tür kullanmak tercih edilir. Daha fazla bilgi için, bu makalenin ilerleyen saatlerinde [Eşzamanlılık ve bellek kullanımına](../articles/azure-functions/functions-bindings-storage-blob-trigger.md#concurrency-and-memory-usage) bakın.