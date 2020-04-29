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
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "77205710"
---
Belirli bir işlev uygulamasındaki tüm işlevlerin kodu, ana bilgisayar yapılandırma dosyası ve bir veya daha fazla alt klasör içeren bir kök proje klasöründe bulunur. Her alt klasör ayrı bir işlevin kodunu içerir. Klasör yapısı aşağıdaki gösterimde gösterilmiştir:

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

Işlevler çalışma zamanının 2. x ve üzeri sürümlerinde, işlev uygulamasındaki tüm işlevler aynı dil yığınını paylaşmalıdır.  

[Host. JSON](../articles/azure-functions/functions-host-json.md) dosyası çalışma zamanına özgü yapılandırmalara sahiptir ve işlev uygulamasının kök klasöründedir. Bir *bin* klasörü, işlev uygulamasının gerektirdiği paketleri ve diğer kitaplık dosyalarını içerir. Bir işlev uygulaması projesi için dile özgü gereksinimlere bakın:

* [C# sınıf kitaplığı (. csproj)](../articles/azure-functions/functions-dotnet-class-library.md#functions-class-library-project)
* [C# betiği (.csx)](../articles/azure-functions/functions-reference-csharp.md#folder-structure)
* [F # betiği](../articles/azure-functions/functions-reference-fsharp.md#folder-structure)
* [Java](../articles/azure-functions/functions-reference-java.md#folder-structure)
* [JavaScript](../articles/azure-functions/functions-reference-node.md#folder-structure)
* [Python](../articles/azure-functions/functions-reference-python.md#folder-structure)
