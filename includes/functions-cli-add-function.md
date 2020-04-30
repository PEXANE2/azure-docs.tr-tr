---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 03/25/2020
ms.author: glenga
ms.openlocfilehash: a33cc604d45d51a63a73ea6ed58abc67269437d7
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "80673192"
---
::: zone pivot="programming-language-csharp,programming-language-javascript,programming-language-typescript,programming-language-powershell,programming-language-python" 
Aşağıdaki komutu kullanarak, `--name` bağımsız değişkenin Işlevinizin (httpexample) benzersiz adı olduğu ve `--template` bağımsız DEĞIŞKENININ işlevin tetikleyicisini (http) belirttiği, projenize bir işlev ekleyin. 

```
func new --name HttpExample --template "HTTP trigger"
```  
::: zone-end  
::: zone pivot="programming-language-csharp"
`func new`bir HttpExample.cs kod dosyası oluşturur.
::: zone-end
::: zone pivot="programming-language-javascript,programming-language-typescript,programming-language-python,programming-language-powershell"
`func new`Projenin seçilen diline ve *function. JSON*adlı yapılandırma dosyasına uygun bir kod dosyası içeren işlev adıyla eşleşen bir alt klasör oluşturur.
::: zone-end