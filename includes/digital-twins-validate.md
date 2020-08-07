---
author: baanders
description: Azure dijital TWINS modellerinin nasıl doğrulandığını açıklayan dosyayı Ekle
ms.service: digital-twins
ms.topic: include
ms.date: 8/7/2020
ms.author: baanders
ms.openlocfilehash: 0146660fa50f9a136cd82483200ca2e6c6cc0b89
ms.sourcegitcommit: 25bb515efe62bfb8a8377293b56c3163f46122bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87985894"
---
> [!TIP]
> Bir model oluşturduktan sonra, Azure dijital TWINS örneğinizi karşıya yüklemeden önce modellerinizi çevrimdışı doğrulamanız önerilir.

DTDL 'nin doğru olduğundan emin olmak için model belgelerini doğrulamak üzere dilden bağımsız bir örnek mevcuttur. Burada bulunur: [**Dtdl Doğrulayıcı örneği**](https://docs.microsoft.com/samples/azure-samples/dtdl-validator/dtdl-validator).

DTDL doğrulayıcısı örneği, NuGet üzerinde bir istemci tarafı kitaplığı olarak kullanılabilen bir .NET DTDL ayrıştırıcısı kitaplığı üzerine kurulmuştur: [**Microsoft. Azure. DigitalTwins. Parser**](https://nuget.org/packages/Microsoft.Azure.DigitalTwins.Parser/). Kitaplığı doğrudan kendi doğrulama çözümünüzü tasarlamak için de kullanabilirsiniz. Ayrıştırıcı kitaplığını kullanırken, Azure Digital TWINS 'in çalıştığı sürümle uyumlu bir sürüm kullandığınızdan emin olun. Önizleme süresince bu sürüm *3.7.0*.

Kullanım örnekleri de dahil olmak üzere Doğrulayıcı örneği ve ayrıştırıcı Kitaplığı hakkında daha fazla bilgi edinebilirsiniz; [*nasıl yapılır: ayrıştırma ve doğrulama modelleri*](../articles/digital-twins/how-to-parse-models.md).