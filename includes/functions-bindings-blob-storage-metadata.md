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
ms.openlocfilehash: 3788fdb954917f28f64a0dfe035bed4ded0932d5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "69642142"
---
Blob tetikleyicisi çeşitli meta veri özellikleri sağlar. Bu özellikler, diğer bağlamaifadelerinin bir parçası olarak veya kodunuzda parametre olarak kullanılabilir. Bu değerler [CloudBlob](https://docs.microsoft.com/dotnet/api/microsoft.azure.storage.blob.cloudblob?view=azure-dotnet) türüyle aynı anlambilime sahiptir.

|Özellik  |Tür  |Açıklama  |
|---------|---------|---------|
|`BlobTrigger`|`string`|Tetikleme blob yolu.|
|`Uri`|`System.Uri`|Ana konum için damla URI.|
|`Properties` |[BlobProperties](https://docs.microsoft.com/dotnet/api/microsoft.azure.storage.blob.blobproperties)|Blob'un sistem özellikleri. |
|`Metadata` |`IDictionary<string,string>`|Blob için kullanıcı tanımlı meta veri.|

Örneğin, aşağıdaki C# komut dosyası ve JavaScript örnekleri kapsayıcı da dahil olmak üzere tetikleyici blob için yol günlük:

```csharp
public static void Run(string myBlob, string blobTrigger, ILogger log)
{
    log.LogInformation($"Full blob path: {blobTrigger}");
} 
```
