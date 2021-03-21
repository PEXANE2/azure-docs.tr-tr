---
title: Azure Işlevlerinde desteklenen diller
description: Hangi dillerin desteklendiğini (GA) ve önizleme aşamasında olduğunu ve Işlevleri diğer dillere genişletmeyi sağlayan yolları öğrenin.
ms.topic: conceptual
ms.date: 11/27/2019
ms.openlocfilehash: da3100521faae61e9803aaa82dbf91045a7ecbc9
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "98108560"
---
# <a name="supported-languages-in-azure-functions"></a>Azure Işlevlerinde desteklenen diller

Bu makalede, Azure Işlevleri ile kullanabileceğiniz diller için sunulan destek düzeyleri açıklanmaktadır. Ayrıca, yerel olarak desteklenmeyen diller kullanılarak işlev oluşturmaya yönelik stratejileri açıklar.

## <a name="levels-of-support"></a>Destek düzeyleri

İki destek düzeyi vardır:

* **Genel olarak kullanılabilir (GA)** -üretim kullanımı için tam olarak desteklenir ve onaylanır.
* **Önizleme** -henüz desteklenmiyor, ancak gelecekte GA durumuna ulaşması bekleniyor.

## <a name="languages-by-runtime-version"></a>Çalışma zamanı sürümüne göre diller 

[Azure işlevleri çalışma zamanının üç sürümü](functions-versions.md) kullanılabilir. Aşağıdaki tabloda, her çalışma zamanı sürümünde hangi dillerin desteklendiği gösterilmektedir.

[!INCLUDE [functions-supported-languages](../../includes/functions-supported-languages.md)]

## <a name="custom-handlers"></a>Özel işleyiciler

Özel işleyiciler, Azure Işlevleri ana bilgisayarındaki olayları alan hafif Web sunucularıdır. HTTP temel öğelerini destekleyen herhangi bir dil, özel bir işleyici uygulayabilir. Bu, Özel işleyicilerin resmi olarak desteklenmeyen dillerde işlevler oluşturmak için kullanılabileceği anlamına gelir. Daha fazla bilgi için bkz. [Azure işlevleri özel işleyiciler](functions-custom-handlers.md).

## <a name="language-extensibility"></a>Dil genişletilebilirliği

Sürüm 2. x ile başlayarak, çalışma zamanı [dil genişletilebilirliği](https://github.com/Azure/azure-webjobs-sdk-script/wiki/Language-Extensibility)sunacak şekilde tasarlanmıştır. 2. x çalışma zamanının JavaScript ve Java dilleri, bu genişletilebilirlik ile oluşturulmuştur.

## <a name="next-steps"></a>Sonraki adımlar

Desteklenen dillerde işlevleri geliştirme hakkında daha fazla bilgi edinmek için aşağıdaki kaynaklara bakın:

+ [C# sınıf kitaplığı Geliştirici Başvurusu](functions-dotnet-class-library.md)
+ [C# betik geliştirici başvurusu](functions-reference-csharp.md)
+ [Java geliştirici başvurusu](functions-reference-java.md)
+ [JavaScript geliştirici başvurusu](functions-reference-node.md)
+ [PowerShell geliştirici başvurusu](functions-reference-powershell.md)
+ [Python geliştirici başvurusu](functions-reference-python.md)
+ [TypeScript geliştirici başvurusu](functions-reference-node.md#typescript)
