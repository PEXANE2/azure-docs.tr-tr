---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 03/25/2020
ms.author: glenga
ms.openlocfilehash: 121f6ffa5c1a7c903e59be8a5bc3e1e1db0834fc
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "80673327"
---
## <a name="add-an-output-binding-definition-to-the-function"></a>İşleve çıkış bağlama tanımı ekleme

Bir işlev yalnızca bir tetikleyicisine sahip olsa da, özel tümleştirme kodu yazmadan diğer Azure hizmetlerine ve kaynaklarına bağlanmanızı sağlayan birden çok giriş ve çıkış bağlaması olabilir. 

::: zone pivot="programming-language-python,programming-language-javascript,programming-language-powershell,programming-language-typescript"  
Bu bağlamaları, işlev klasörünüzdeki *function. JSON* dosyasında bildirirsiniz. Önceki hızlı başlangıçta, *Httpexample* klasöründeki `bindings` *function. JSON* dosyanız koleksiyonda iki bağlama içerir:  
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
Her bağlamanın en az bir türü, yönü ve adı vardır. Yukarıdaki örnekte, ilk bağlama yönden `httpTrigger` `in`bir tür. `in` Yönü için, `name` tetikleyici tarafından çağrıldığında işleve gönderilen giriş parametresinin adını belirtir.  
::: zone-end

::: zone pivot="programming-language-javascript,programming-language-typescript"  
Koleksiyondaki ikinci bağlamanın adı `res`. Bu `http` bağlama, HTTP yanıtını yazmak için`out`kullanılan bir çıkış bağlamadır (). 

Bu işlevden bir Azure depolama kuyruğuna yazmak için aşağıdaki kodda gösterildiği gibi adıyla `out` `queue` `msg`bir tür bağlama ekleyin:

:::code language="json" source="~/functions-docs-javascript/functions-add-output-binding-storage-queue-cli/HttpExample/function.json" range="3-26":::
::: zone-end  

::: zone pivot="programming-language-python"  
Koleksiyondaki ikinci `http` bağlama yöndür `out`ve bu durumda özel `name` , bu bağlamanın bir giriş parametresi sağlamak yerine işlevin dönüş `$return` değerini kullandığını gösterir.

Bu işlevden bir Azure depolama kuyruğuna yazmak için aşağıdaki kodda gösterildiği gibi adıyla `out` `queue` `msg`bir tür bağlama ekleyin:

:::code language="json" source="~/functions-docs-python/functions-add-output-binding-storage-queue-cli/HttpExample/function.json" range="3-26":::
::: zone-end  

::: zone pivot="programming-language-powershell"  
Koleksiyondaki ikinci bağlamanın adı `res`. Bu `http` bağlama, HTTP yanıtını yazmak için`out`kullanılan bir çıkış bağlamadır (). 

Bu işlevden bir Azure depolama kuyruğuna yazmak için aşağıdaki kodda gösterildiği gibi adıyla `out` `queue` `msg`bir tür bağlama ekleyin:

:::code language="json" source="~/functions-docs-powershell/functions-add-output-binding-storage-queue-cli/HttpExample/function.json" range="3-26":::
::: zone-end  

::: zone pivot="programming-language-python,programming-language-javascript,programming-language-powershell,programming-language-typescript"  
Bu durumda, `msg` işlevine çıkış bağımsız değişkeni olarak verilir. `queue` Bir tür için, `queueName` içindeki kuyruğun adını da belirtmeniz ve içindeki Azure depolama bağlantısının ( *Local. Settings. JSON*' `connection`dan) *adını* sağlamanız gerekir. 
::: zone-end  
