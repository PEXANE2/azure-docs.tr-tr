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
ms.openlocfilehash: f4e6d5fb41769544b7be0f689447364988d0380d
ms.sourcegitcommit: c95e2d89a5a3cf5e2983ffcc206f056a7992df7d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/24/2020
ms.locfileid: "95998862"
---
Blob tetikleyicisi çeşitli meta veri özellikleri sağlar. Bu özellikler, diğer bağlamalardaki veya kodunuzda parametre olarak bağlama ifadelerinin bir parçası olarak kullanılabilir. Bu değerler [Cloudblob](/dotnet/api/microsoft.azure.storage.blob.cloudblob?view=azure-dotnet) türüyle aynı semantiğe sahiptir.

|Özellik  |Tür  |Description  |
|---------|---------|---------|
|`BlobTrigger`|`string`|Tetikleme blobunun yolu.|
|`Uri`|`System.Uri`|Birincil konumun blob URI 'SI.|
|`Properties` |[BlobProperties](/dotnet/api/microsoft.azure.storage.blob.blobproperties)|Blobun sistem özellikleri. |
|`Metadata` |`IDictionary<string,string>`|Blob için Kullanıcı tanımlı meta veriler.|

Örneğin, aşağıdaki C# betiği ve JavaScript örnekleri, kapsayıcısı da dahil olmak üzere tetikleme blobu yolunu günlüğe kaydeder:

```csharp
public static void Run(string myBlob, string blobTrigger, ILogger log)
{
    log.LogInformation($"Full blob path: {blobTrigger}");
} 
```