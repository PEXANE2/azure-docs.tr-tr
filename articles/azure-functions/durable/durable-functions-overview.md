---
title: Dayanıklı İşlevler genel bakış-Azure
description: Azure Işlevleri için Dayanıklı İşlevler uzantısına giriş.
services: functions
author: ggailey777
manager: jeconnoc
keywords: ''
ms.service: azure-functions
ms.topic: overview
ms.date: 12/22/2018
ms.author: glenga
ms.reviewer: azfuncdf
ms.openlocfilehash: 323ab530d8199dd154e5d3568c09f86f6f52d702
ms.sourcegitcommit: 44e85b95baf7dfb9e92fb38f03c2a1bc31765415
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/28/2019
ms.locfileid: "70087205"
---
# <a name="what-are-durable-functions"></a>Dayanıklı İşlevler nedir?

*Dayanıklı işlevler* , bir sunucusuz ortamda durum bilgisi olan işlevler yazmanıza olanak tanıyan [Azure işlevleri](../functions-overview.md) uzantısıdır. Uzantı sizin için durumu, denetim noktalarını ve yeniden başlatmaları yönetir.

## <a name="benefits"></a>Avantajlar

Uzantı, aşağıdaki avantajları sağlayabilen bir [*Orchestrator işlevi*](durable-functions-types-features-overview.md#orchestrator-functions)kullanarak durum bilgisi olan iş akışları tanımlamanızı sağlar:

* İş akışlarınızı kodda tanımlayabilirsiniz. JSON şemaları veya tasarımcıları gerekli değildir.
* Diğer işlevler hem zaman uyumlu hem de zaman uyumsuz olarak çağrılabilir. Çağrılan işlevlerden alınan çıkış, yerel değişkenlere kaydedilebilir.
* İşlev await olduğunda ilerleme otomatik olarak onay işareti yapılır. İşlem geri dönüştürüldüğünde veya VM yeniden başlatıldığında yerel durum hiçbir zaman kaybedilmez.

## <a name="application-patterns"></a>Uygulama desenleri

Dayanıklı İşlevler için birincil kullanım örneği sunucusuz uygulamalarda karmaşık, durum bilgisi olan düzenleme gereksinimlerini basitleştirir. Dayanıklı İşlevler avantajlarından faydalanabilecek bazı tipik uygulama desenleri aşağıda verilmiştir:

* [İlerek](durable-functions-concepts.md#chaining)
* [Fan-çıkış/fan](durable-functions-concepts.md#fan-in-out)
* [Zaman uyumsuz HTTP API 'Leri](durable-functions-concepts.md#async-http)
* [İzleme](durable-functions-concepts.md#monitoring)
* [İnsan etkileşimi](durable-functions-concepts.md#human)

## <a name="language-support"></a>Desteklenen diller

Dayanıklı İşlevler Şu anda aşağıdaki dilleri desteklemektedir:

* **C#** : hem [önceden derlenmiş sınıf kitaplıkları](../functions-dotnet-class-library.md) hem [ C# de betiği](../functions-reference-csharp.md).
* **F#** : önceden derlenmiş sınıf kitaplıkları F# ve betiği. F#betik yalnızca Azure Işlevleri çalışma zamanının sürüm 1. x 'i için desteklenir.
* **JavaScript**: yalnızca Azure işlevleri çalışma zamanının 2. x sürümü için desteklenir. Dayanıklı İşlevler uzantısının veya sonraki bir sürümün sürüm 1.7.0 gerektirir. 

Dayanıklı İşlevler tüm [Azure işlevleri dillerini](../supported-languages.md)destekleme amacını içerir. Ek dilleri desteklemek için işin en son durumunun [dayanıklı işlevler sorunlar listesine](https://github.com/Azure/azure-functions-durable-extension/issues) bakın.

Azure Işlevleri gibi, [Visual Studio 2019](durable-functions-create-first-csharp.md), [Visual Studio Code](quickstart-js-vscode.md)ve [Azure Portal](durable-functions-create-portal.md)kullanarak dayanıklı işlevler geliştirmenize yardımcı olacak şablonlar vardır.

## <a name="billing"></a>Faturalandırma

Dayanıklı İşlevler Azure Işlevleri ile aynı faturalandırılır. Daha fazla bilgi için [Azure işlevleri fiyatlandırması](https://azure.microsoft.com/pricing/details/functions/).

## <a name="jump-right-in"></a>Hemen geç

Bu dile özgü hızlı başlangıç öğreticilerden birini tamamlayarak 10 dakikadan kısa bir süre içinde Dayanıklı İşlevler kullanmaya başlamanızı sağlayabilirsiniz:

* [C#Visual Studio 2019 kullanma](durable-functions-create-first-csharp.md)
* [Visual Studio Code kullanan JavaScript](quickstart-js-vscode.md)

Her iki hızlı başlangıçlarda, "Hello World" dayanıklı işlevini yerel olarak oluşturup test edersiniz. Ardından işlev kodunu Azure’da yayımlayacaksınız. Oluşturduğunuz işlev, diğer işlevlere yapılan çağrıları düzenler ve birbirine zincirler.

## <a name="learn-more"></a>Daha fazla bilgi edinin

Aşağıdaki videoda Dayanıklı İşlevler avantajları vurgulanmıştır:

> [!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Durable-Functions-in-Azure-Functions/player] 

Dayanıklı İşlevler [Azure işlevleri](../functions-overview.md)için gelişmiş bir uzantı olduğundan, tüm uygulamalar için uygun değildir. Dayanıklı İşlevler hakkında daha fazla bilgi edinmek için bkz. [dayanıklı işlevler desenler ve teknik kavramlar](durable-functions-concepts.md). Diğer Azure düzenleme teknolojileriyle bir karşılaştırma için bkz. [Azure Işlevlerini karşılaştırma ve Azure Logic Apps](../functions-compare-logic-apps-ms-flow-webjobs.md#compare-azure-functions-and-azure-logic-apps).

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Dayanıklı İşlevler desenler ve teknik kavramlar](durable-functions-concepts.md)
