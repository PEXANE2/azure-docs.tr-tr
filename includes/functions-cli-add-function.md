---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 03/25/2020
ms.author: glenga
ms.openlocfilehash: a33cc604d45d51a63a73ea6ed58abc67269437d7
ms.sourcegitcommit: b129186667a696134d3b93363f8f92d175d51475
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80673192"
---
::: zone pivot="programming-language-csharp,programming-language-javascript,programming-language-typescript,programming-language-powershell,programming-language-python" 
`--name` Bağımsız değişkenin işlevinizin benzersiz adı (HttpÖrnek) olduğu ve `--template` bağımsız değişkenin işlevin tetikleyicisini (HTTP) belirttiği aşağıdaki komutu kullanarak projenize bir işlev ekleyin. 

```
func new --name HttpExample --template "HTTP trigger"
```  
::: zone-end  
::: zone pivot="programming-language-csharp"
`func new`HttpExample.cs kod dosyası oluşturur.
::: zone-end
::: zone pivot="programming-language-javascript,programming-language-typescript,programming-language-python,programming-language-powershell"
`func new`projenin seçilen diline uygun bir kod dosyası ve *function.json*adlı yapılandırma dosyası içeren işlev adı ile eşleşen bir alt klasör oluşturur.
::: zone-end