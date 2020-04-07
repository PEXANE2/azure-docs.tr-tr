---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 03/25/2020
ms.author: glenga
ms.openlocfilehash: 121f6ffa5c1a7c903e59be8a5bc3e1e1db0834fc
ms.sourcegitcommit: b129186667a696134d3b93363f8f92d175d51475
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80673327"
---
## <a name="add-an-output-binding-definition-to-the-function"></a>İşleviçin çıktı bağlama tanımı ekleme

Bir işlevin yalnızca bir tetikleyicisi olsa da, özel tümleştirme kodu yazmadan diğer Azure hizmetlerine ve kaynaklarına bağlanmanızı sağlayan birden çok giriş ve çıktı bağlaması olabilir. 

::: zone pivot="programming-language-python,programming-language-javascript,programming-language-powershell,programming-language-typescript"  
Bu bağlamaları işlev klasörünüzde *işlev.json* dosyasında bildirirsiniz. Önceki hızlı başlangıçtan itibaren, *HttpExample* klasöründeki `bindings` *function.json* dosyanız koleksiyonda iki bağlama içerir:  
::: zone-end

::: zone pivot="programming-language-javascript,programming-language-typescript"  
:::code language="json" source="~/functions-quickstart-templates/Functions.Templates/Templates/HttpTrigger-JavaScript/function.json" range="2-18":::  
::: zone-end

::: zone pivot="programming-language-python"  
:::code language="json" source="~/functions-quickstart-templates/Functions.Templates/Templates/HttpTrigger-Python/function.json" range="2-18":::  
::: zone-end

::: zone pivot="programming-language-powershell"  
:::code language="json" source="~/functions-quickstart-templates/Functions.Templates/Templates/HttpTrigger-PowerShell/function.json" range="2-18":::
::: zone-end  

::: zone pivot="programming-language-python,programming-language-javascript, programming-language-powershell, programming-language-typescript"  
Her bağlamanın en az bir türü, yönü ve adı vardır. Yukarıdaki örnekte, ilk bağlama yönü `httpTrigger` `in`ile türüdür. `in` Yön için, `name` tetikleyici tarafından çağrıldığınızda işleve gönderilen bir giriş parametresinin adını belirtir.  
::: zone-end

::: zone pivot="programming-language-javascript,programming-language-typescript"  
Koleksiyondaki ikinci bağlama adı `res`. Bu `http` bağlama, HTTP`out`yanıtını yazmak için kullanılan bir çıktı bağlama ( ) 

Bu işlevden bir Azure Depolama kuyruğuna `out` yazmak `queue` için, `msg`aşağıdaki kodda gösterildiği gibi, adında bir tür bağlama ekleyin:

:::code language="json" source="~/functions-docs-javascript/functions-add-output-binding-storage-queue-cli/HttpExample/function.json" range="3-26":::
::: zone-end  

::: zone pivot="programming-language-python"  
Koleksiyondaki ikinci `http` bağlama yönü `out`ile tür , bu durumda `name` `$return` özel bu bağlama bir giriş parametresi sağlamak yerine işlevin dönüş değerini kullandığını gösterir.

Bu işlevden bir Azure Depolama kuyruğuna `out` yazmak `queue` için, `msg`aşağıdaki kodda gösterildiği gibi, adında bir tür bağlama ekleyin:

:::code language="json" source="~/functions-docs-python/functions-add-output-binding-storage-queue-cli/HttpExample/function.json" range="3-26":::
::: zone-end  

::: zone pivot="programming-language-powershell"  
Koleksiyondaki ikinci bağlama adı `res`. Bu `http` bağlama, HTTP`out`yanıtını yazmak için kullanılan bir çıktı bağlama ( ) 

Bu işlevden bir Azure Depolama kuyruğuna `out` yazmak `queue` için, `msg`aşağıdaki kodda gösterildiği gibi, adında bir tür bağlama ekleyin:

:::code language="json" source="~/functions-docs-powershell/functions-add-output-binding-storage-queue-cli/HttpExample/function.json" range="3-26":::
::: zone-end  

::: zone pivot="programming-language-python,programming-language-javascript,programming-language-powershell,programming-language-typescript"  
Bu durumda, `msg` bir çıktı bağımsız değişkeni olarak işlev verilir. Bir `queue` tür için, sıranın adını belirtmeniz `queueName` ve Azure Depolama bağlantısının *adını* *(local.settings.json'dan)*.'da `connection`sağlamanız gerekir. 
::: zone-end  
