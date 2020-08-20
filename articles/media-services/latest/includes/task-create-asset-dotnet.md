---
author: IngridAtMicrosoft
ms.service: media-services
ms.topic: include
ms.date: 08/18/2020
ms.author: inhenkel
ms.custom: dotnet
ms.openlocfilehash: 820353ffbb5e23e27c3039ad27a8f6507b9d2167
ms.sourcegitcommit: d661149f8db075800242bef070ea30f82448981e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/19/2020
ms.locfileid: "88608891"
---
<!--Create a media services asset REST-->

Aşağıdaki Azure .NET komutu yeni bir Media Services varlığı oluşturur. , Ve değerlerini `subscriptionID` `resourceGroup` `amsAccountName` Şu anda çalıştığınız değerlerle değiştirin. Burada ayarlayarak varlığınıza bir ad verin `assetName` .

```csharp
 Asset asset = await client.Assets.CreateOrUpdateAsync(resourceGroupName, accountName, assetName, new Asset());
```
