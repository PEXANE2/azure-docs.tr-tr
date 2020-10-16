---
author: baanders
description: Azure dijital TWINS modellerinin nasıl doğrulandığını açıklayan dosyayı Ekle
ms.service: digital-twins
ms.topic: include
ms.date: 8/7/2020
ms.author: baanders
ms.openlocfilehash: da42f847bdd57c4dae0cde673c9c9e08d51a758d
ms.sourcegitcommit: 090ea6e8811663941827d1104b4593e29774fa19
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/13/2020
ms.locfileid: "91998322"
---
> [!TIP]
> Bir model oluşturduktan sonra, Azure dijital TWINS örneğinizi karşıya yüklemeden önce modellerinizi çevrimdışı doğrulamanız önerilir.

DTDL 'nin örneğinizi örnekize yüklemeden önce doğru olduğundan emin olmak için model belgelerini doğrulamak üzere dil açısından belirsiz bir örnek mevcuttur. Burada bulunur: [**Dtdl Doğrulayıcı örneği**](https://docs.microsoft.com/samples/azure-samples/dtdl-validator/dtdl-validator).

* DTDL doğrulayıcısı örneği, NuGet üzerinde bir istemci tarafı kitaplığı olarak kullanılabilen bir .NET DTDL ayrıştırıcısı kitaplığı üzerine kurulmuştur: [**Microsoft. Azure. DigitalTwins. Parser**](https://nuget.org/packages/Microsoft.Azure.DigitalTwins.Parser/). Kitaplığı doğrudan kendi doğrulama çözümünüzü tasarlamak için de kullanabilirsiniz. Ayrıştırıcı kitaplığını kullanırken, Azure Digital TWINS 'in çalıştığı sürümle uyumlu bir sürüm kullandığınızdan emin olun. Önizleme süresince bu sürüm *3.12.4*.

Kullanım örnekleri de dahil olmak üzere Doğrulayıcı örneği ve ayrıştırıcı Kitaplığı hakkında daha fazla bilgi edinebilirsiniz; [*nasıl yapılır: ayrıştırma ve doğrulama modelleri*](../articles/digital-twins/how-to-parse-models.md).