---
author: craigktreasure
ms.service: azure-spatial-anchors
ms.topic: include
ms.date: 1/2/2019
ms.author: crtreasu
ms.openlocfilehash: e3ef7e7b8f527cde1fcfbb13141fd3a520921267
ms.sourcegitcommit: fdec8e8bdbddcce5b7a0c4ffc6842154220c8b90
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/19/2020
ms.locfileid: "83673433"
---
Bir sonraki adım, uygulamayı hesap tanımlayıcıyı ve hesap anahtarınızı kullanacak şekilde yapılandırmaktır. [Uzamsal Tutturucuların kaynağını ayarlarken](#create-a-spatial-anchors-resource)bunları bir metin düzenleyicisine kopyaladınız.

**Proje** bölmesinde öğesine gidin `Assets\AzureSpatialAnchors.SDK\Resources` . `SpatialAnchorConfig` öğesini seçin. Ardından, **Inspector** bölmesine için `Account Key` değeri olarak değerini `Spatial Anchors Account Key` ve `Account ID` değeri olarak değerini girin `Spatial Anchors Account Id` .

Sonra, öğesini açın `SpatialAnchorManager.cs` . `CreateSessionAsync()`Aşağıdaki satırı bulun ve daha önce hesap etki alanında yerine ekleyin: `session.Configuration.AccountId = "MyAccountDomain";` . Bu satırı doğrudan bu açıklamadan önce ekleyebilirsiniz `// Configure authentication` .
