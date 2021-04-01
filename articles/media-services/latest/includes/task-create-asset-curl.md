---
author: IngridAtMicrosoft
ms.service: media-services
ms.topic: include
ms.date: 08/18/2020
ms.author: inhenkel
ms.custom: cURL
ms.openlocfilehash: ca7f0dab2302386a9cb7d21ebb3224d24afe5e19
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2021
ms.locfileid: "88608902"
---
<!--Create a media services asset cURL-->

Aşağıdaki Azure kıvrımlı komutu yeni bir Media Services varlığı oluşturur. , Ve değerlerini `subscriptionID` `resourceGroup` `amsAccountName` Şu anda çalıştığınız değerlerle değiştirin. Burada ayarlayarak varlığınıza bir ad verin `assetName` .

```cURL
curl -X PUT 'https://management.azure.com/subscriptions/00000000-0000-0000-000000000000/resourceGroups/resourceGroupName/providers/Microsoft.Media/mediaServices/amsAccountName/assets/myOutputAsset?api-version=2018-07-01' -H 'Accept: application/json' -H 'Content-Type: application/json' -d '{"properties": {"description": "",}}'
```
