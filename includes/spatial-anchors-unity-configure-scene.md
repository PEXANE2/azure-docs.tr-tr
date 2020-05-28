---
author: craigktreasure
ms.service: azure-spatial-anchors
ms.topic: include
ms.date: 1/2/2019
ms.author: crtreasu
ms.openlocfilehash: 7e7825e8247e78cbc0c0e9e22bdbd9326939e0a8
ms.sourcegitcommit: 64fc70f6c145e14d605db0c2a0f407b72401f5eb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/27/2020
ms.locfileid: "83998113"
---
Bir sonraki adım, uygulamayı hesap tanımlayıcıyı ve hesap anahtarınızı kullanacak şekilde yapılandırmaktır. [Uzamsal Tutturucuların kaynağını ayarlarken](#create-a-spatial-anchors-resource)bunları bir metin düzenleyicisine kopyaladınız.

**Proje** bölmesinde öğesine gidin `Assets\AzureSpatialAnchors.SDK\Resources` . `SpatialAnchorConfig` öğesini seçin. Ardından, **Inspector** bölmesine için `Account Key` değeri olarak değerini `Spatial Anchors Account Key` ve `Account ID` değeri olarak değerini girin `Spatial Anchors Account Id` .

Sonra, öğesini açın `SpatialAnchorManager.cs` . `CreateSessionAsync()`Aşağıdaki satırı bulun ve daha önce hesap etki alanında yerine ekleyin: `session.Configuration.AccountDomain = "MyAccountDomain";` . Bu satırı doğrudan bu açıklamadan önce ekleyebilirsiniz `// Configure authentication` .
