---
title: Azure Işlevlerinde desteklenen diller
description: Hangi dillerin desteklendiğini (GA) ve önizleme aşamasında olduğunu ve Işlevleri diğer dillere genişletmeyi sağlayan yolları öğrenin.
ms.topic: conceptual
ms.date: 11/27/2019
ms.openlocfilehash: 91a6ea886c3828678771b24d69bb7987af1fb105
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "83994910"
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

## <a name="custom-handlers-preview"></a>Özel işleyiciler (Önizleme) 

Özel işleyiciler, Azure Işlevleri ana bilgisayarındaki olayları alan hafif Web sunucularıdır. HTTP temel öğelerini destekleyen herhangi bir dil, özel bir işleyici uygulayabilir. Bu, Özel işleyicilerin resmi olarak desteklenmeyen dillerde işlevler oluşturmak için kullanabileceği anlamına gelir. Daha fazla bilgi için bkz. [Azure işlevleri özel işleyiciler (Önizleme)](functions-custom-handlers.md).

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
