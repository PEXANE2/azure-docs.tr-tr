---
title: include dosyası
description: include dosyası
services: functions
author: ggailey777
manager: jeconnoc
ms.service: azure-functions
ms.topic: include
ms.date: 09/12/2018
ms.author: glenga
ms.custom: include file
ms.openlocfilehash: fd220a4a955a8b2cd583c2fe9e3899cb36c1587d
ms.sourcegitcommit: a5ebf5026d9967c4c4f92432698cb1f8651c03bb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/08/2019
ms.locfileid: "74935950"
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

* [C#sınıf kitaplığı (. csproj)](../articles/azure-functions/functions-dotnet-class-library.md#functions-class-library-project)
* [C# betiği (.csx)](../articles/azure-functions/functions-reference-csharp.md#folder-structure)
* [F#SCRIPT](../articles/azure-functions/functions-reference-fsharp.md#folder-structure)
* [Java](../articles/azure-functions/functions-reference-java.md#folder-structure)
* [JavaScript](../articles/azure-functions/functions-reference-node.md#folder-structure)
