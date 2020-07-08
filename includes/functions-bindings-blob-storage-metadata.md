---
title: dosya dahil etme
description: dosya dahil etme
services: functions
author: craigshoemaker
manager: gwallace
ms.service: azure-functions
ms.topic: include
ms.date: 08/02/2019
ms.author: cshoe
ms.custom: include file
ms.openlocfilehash: 3788fdb954917f28f64a0dfe035bed4ded0932d5
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
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
