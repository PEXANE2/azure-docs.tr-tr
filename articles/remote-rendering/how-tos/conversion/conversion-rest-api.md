---
title: Varlık dönüştürme REST API
description: REST API aracılığıyla bir varlığın nasıl dönüştürüleceğini açıklar
author: florianborn71
ms.author: flborn
ms.date: 02/04/2020
ms.topic: how-to
ms.openlocfilehash: 4f8ac72e2b598a6c7631d691cc1bfb82cdba7599
ms.sourcegitcommit: 7ec45b7325e36debadb960bae4cf33164176bc24
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/16/2021
ms.locfileid: "100530272"
---
# <a name="use-the-model-conversion-rest-api"></a>Model dönüştürme REST API’yi kullanma

[Model dönüştürme](model-conversion.md) hizmeti bir [REST API](https://en.wikipedia.org/wiki/Representational_state_transfer)ile denetlenir. Bu API, dönüştürmeler oluşturmak, dönüştürme özelliklerini almak ve var olan dönüştürmeleri listelemek için kullanılabilir.

## <a name="rest-api-reference"></a>REST API başvurusu

Uzaktan Işleme REST API başvuru belgeleri [burada](https://docs.microsoft.com/rest/api/mixedreality/2021-01-01preview/remoterendering)ve Swagger tanımlarını [burada](https://github.com/Azure/azure-rest-api-specs/tree/master/specification/mixedreality/data-plane/Microsoft.MixedReality)bulabilirsiniz.

*Conversion.ps1* adlı *betikler* klasöründeki [ARR örnekleri deposunda](https://github.com/Azure/azure-remote-rendering) , hizmetimizin kullanımını gösteren bir PowerShell betiği sağlıyoruz. Betik ve yapılandırması burada açıklanmıştır: [örnek PowerShell betikleri](../../samples/powershell-example-scripts.md). Ayrıca, [.net](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/mixedreality/Azure.MixedReality.RemoteRendering), Java ve Python için SDK 'lar sunuyoruz.

## <a name="next-steps"></a>Sonraki adımlar

- [Model dönüştürmesi için Azure Blob depolamayı kullanma](blob-storage.md)
- [Model dönüştürme](model-conversion.md)
