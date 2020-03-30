---
title: include dosyası
description: include dosyası
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 09/12/2018
ms.author: glenga
ms.custom: include file
ms.openlocfilehash: 5f5f682c8c31c17f1a645bcdacdc78f32bd5c001
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77205710"
---
Belirli bir işlev uygulamasındaki tüm işlevlerin kodu, ana bilgisayar yapılandırma dosyası ve bir veya daha fazla alt klasör içeren bir kök proje klasöründe bulunur. Her alt klasör ayrı bir işleviçin kod içerir. Klasör yapısı aşağıdaki gösterimde gösterilir:

```
FunctionApp
 | - host.json
 | - MyFirstFunction
 | | - function.json
 | | - ...  
 | - MySecondFunction
 | | - function.json
 | | - ...  
 | - SharedCode
 | - bin
```

İşlevler çalışma süresinin 2.x sürümünde ve daha yükseklerinde, işlev uygulamasındaki tüm işlevler aynı dil yığınını paylaşmalıdır.  

[Host.json](../articles/azure-functions/functions-host-json.md) dosyası çalışma süresine özgü yapılandırmalar içerir ve işlev uygulamasının kök klasöründedir. Depo *gözü* klasörü, işlev uygulamasının gerektirdiği paketleri ve diğer kitaplık dosyalarını içerir. Bir işlev uygulaması projesi için dile özel gereksinimlere bakın:

* [C# sınıf kitaplığı (.csproj)](../articles/azure-functions/functions-dotnet-class-library.md#functions-class-library-project)
* [C# betiği (.csx)](../articles/azure-functions/functions-reference-csharp.md#folder-structure)
* [F# komut dosyası](../articles/azure-functions/functions-reference-fsharp.md#folder-structure)
* [Java](../articles/azure-functions/functions-reference-java.md#folder-structure)
* [Javascript](../articles/azure-functions/functions-reference-node.md#folder-structure)
* [Python](../articles/azure-functions/functions-reference-python.md#folder-structure)
