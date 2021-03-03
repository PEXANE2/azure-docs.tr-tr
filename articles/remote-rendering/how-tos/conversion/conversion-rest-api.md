---
title: Varlık dönüştürme REST API
description: REST API aracılığıyla bir varlığın nasıl dönüştürüleceğini açıklar
author: florianborn71
ms.author: flborn
ms.date: 02/04/2020
ms.topic: how-to
ms.openlocfilehash: f33e5717cd5556e72d996e7e943867c16805e71b
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/03/2021
ms.locfileid: "101705186"
---
# <a name="use-the-model-conversion-rest-api"></a>Model dönüştürme REST API’yi kullanma

[Model dönüştürme](model-conversion.md) hizmeti bir [REST API](https://en.wikipedia.org/wiki/Representational_state_transfer)ile denetlenir. Bu API, dönüştürmeler oluşturmak, dönüştürme özelliklerini almak ve var olan dönüştürmeleri listelemek için kullanılabilir.

## <a name="rest-api-reference"></a>REST API başvurusu

Uzaktan Işleme REST API başvuru belgeleri [burada](/rest/api/mixedreality/2021-01-01preview/remoterendering)ve Swagger tanımlarını [burada](https://github.com/Azure/azure-rest-api-specs/tree/master/specification/mixedreality/data-plane/Microsoft.MixedReality)bulabilirsiniz.

*Conversion.ps1* adlı *betikler* klasöründeki [ARR örnekleri deposunda](https://github.com/Azure/azure-remote-rendering) , hizmetimizin kullanımını gösteren bir PowerShell betiği sağlıyoruz. Betik ve yapılandırması burada açıklanmıştır: [örnek PowerShell betikleri](../../samples/powershell-example-scripts.md). Ayrıca, [.net](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/mixedreality/Azure.MixedReality.RemoteRendering), Java ve Python için SDK 'lar sunuyoruz.

## <a name="next-steps"></a>Sonraki adımlar

- [Model dönüştürmesi için Azure Blob depolamayı kullanma](blob-storage.md)
- [Model dönüştürme](model-conversion.md)