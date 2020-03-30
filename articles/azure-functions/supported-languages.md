---
title: Azure İşlevlerinde desteklenen diller
description: Hangi dillerin desteklenilen (GA) ve hangilerinin deneysel veya önizleme de olduğunu öğrenin.
ms.topic: conceptual
ms.date: 11/27/2019
ms.openlocfilehash: 029ea753439dca3093bf214a5adfb6d58a1fe567
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74942265"
---
# <a name="supported-languages-in-azure-functions"></a>Azure İşlevlerinde desteklenen diller

Bu makalede, Azure İşlevler ile kullanabileceğiniz diller için sunulan destek düzeyleri açıklanmaktadır.

## <a name="levels-of-support"></a>Destek düzeyleri

Üç destek düzeyi vardır:

* **Genel olarak kullanılabilir (GA)** - Tam olarak desteklenen ve üretim kullanımı için onaylanmıştır.
* **Önizleme** - Henüz desteklenmedi ancak gelecekte GA durumuna ulaşması bekleniyor.
* **Deneysel** - Desteklenmeyen ve gelecekte terk edilebilir; nihai önizleme veya GA durumunun garantisi yoktur.

## <a name="languages-by-runtime-version"></a>Çalışma zamanı sürümüne göre diller 

[Azure İşlevler çalışma zamanının üç sürümü](functions-versions.md) kullanılabilir. Aşağıdaki tablo, her çalışma zamanı sürümünde hangi dillerin destekleniyi gösterir.

[!INCLUDE [functions-supported-languages](../../includes/functions-supported-languages.md)]

### <a name="experimental-languages"></a>Deneysel diller

Sürüm 1.x'teki deneysel diller iyi ölçeklenmez ve tüm bağlamaları desteklemez.

Onlar için resmi bir destek olmadığı için, güvendiğiniz hiçbir şey için deneysel özellikler kullanmayın. Destek durumları deneysel dillerle ilgili sorunlar için açılmamalıdır. 

Daha sonraki çalışma zamanı sürümleri deneysel dilleri desteklemez. Yeni diller için destek, yalnızca dil üretimde desteklenebilirse eklenir. 

### <a name="language-extensibility"></a>Dil genişletilebilirliği

Sürüm 2.x ile başlayarak, çalışma süresi [dil genişletilebilirlik](https://github.com/Azure/azure-webjobs-sdk-script/wiki/Language-Extensibility)sunmak için tasarlanmıştır. 2.x çalışma zamanındaki JavaScript ve Java dilleri bu genişletilebilirlikle oluşturulmuştur.

## <a name="next-steps"></a>Sonraki adımlar

Desteklenen dillerde işlevlerin nasıl geliştirileceği hakkında daha fazla bilgi edinmek için aşağıdaki kaynaklara bakın:

+ [C# sınıfı kitaplık geliştirici başvurusu](functions-dotnet-class-library.md)
+ [C# komut dosyası geliştirici stoyonu](functions-reference-csharp.md)
+ [Java geliştirici başvurusu](functions-reference-java.md)
+ [JavaScript geliştirici başvurusu](functions-reference-node.md)
+ [PowerShell geliştirici başvurusu](functions-reference-powershell.md)
+ [Python geliştirici başvurusu](functions-reference-python.md)
+ [TypeScript geliştirici başvurusu](functions-reference-node.md#typescript)
