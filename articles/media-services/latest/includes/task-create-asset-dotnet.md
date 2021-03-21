---
author: IngridAtMicrosoft
ms.service: media-services
ms.topic: include
ms.date: 08/18/2020
ms.author: inhenkel
ms.custom: dotnet
ms.openlocfilehash: dcd2cda3bad2a13a83c5f3f6700e5a57471e2065
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "88653909"
---
<!--Create a media services asset REST-->

Aşağıdaki Azure .NET komutu yeni bir Media Services varlığı oluşturur. , Ve değerlerini `subscriptionID` `resourceGroup` `amsAccountName` Şu anda çalıştığınız değerlerle değiştirin. Burada ayarlayarak varlığınıza bir ad verin `assetName` .

[!code-csharp[Main](../../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/UploadEncodeAndStreamFiles/Program.cs#CreateInputAsset)]
