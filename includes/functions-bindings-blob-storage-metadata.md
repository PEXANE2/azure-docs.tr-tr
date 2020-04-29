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
ms.sourcegitcommit: fad3aaac5af8c1b3f2ec26f75a8f06e8692c94ed
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/27/2020
ms.locfileid: "69642142"
---
Blob tetikleyicisi çeşitli meta veri özellikleri sağlar. Bu özellikler, diğer bağlamalardaki veya kodunuzda parametre olarak bağlama ifadelerinin bir parçası olarak kullanılabilir. Bu değerler [Cloudblob](https://docs.microsoft.com/dotnet/api/microsoft.azure.storage.blob.cloudblob?view=azure-dotnet) türüyle aynı semantiğe sahiptir.

|Özellik  |Tür  |Açıklama  |
|---------|---------|---------|
|`BlobTrigger`|`string`|Tetikleme blobunun yolu.|
|`Uri`|`System.Uri`|Birincil konumun blob URI 'SI.|
|`Properties` |[BlobProperties](https://docs.microsoft.com/dotnet/api/microsoft.azure.storage.blob.blobproperties)|Blobun sistem özellikleri. |
|`Metadata` |`IDictionary<string,string>`|Blob için Kullanıcı tanımlı meta veriler.|

Örneğin, aşağıdaki C# betiği ve JavaScript örnekleri, kapsayıcısı da dahil olmak üzere tetikleme blobu yolunu günlüğe kaydeder:

```csharp
public static void Run(string myBlob, string blobTrigger, ILogger log)
{
    log.LogInformation($"Full blob path: {blobTrigger}");
} 
```
