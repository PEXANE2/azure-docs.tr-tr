---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 03/25/2020
ms.author: glenga
ms.openlocfilehash: f102a5dd5b7dccba6643176d06d17a2a65171c90
ms.sourcegitcommit: 80c1056113a9d65b6db69c06ca79fa531b9e3a00
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/09/2020
ms.locfileid: "96904083"
---
## <a name="configure-your-local-environment"></a>Yerel ortamınızı yapılandırma

Başlamadan önce aşağıdakilere sahip olmanız gerekir:

+ Etkin aboneliği olan bir Azure hesabı. [Ücretsiz hesap oluşturun](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio).

::: zone pivot="programming-language-csharp,programming-language-javascript,programming-language-typescript,programming-language-powershell,programming-language-java,programming-language-other"  
+ [Azure Functions Core Tools](../articles/azure-functions/functions-run-local.md#v2) sürüm 2.7.1846 veya sonraki bir sürümü.
::: zone-end  
::: zone pivot="programming-language-python"
+ Yüklü Python sürümüne karşılık gelen Azure Functions Core Tools sürümü:

   | Python sürümü | Çekirdek araçları sürümü |
   | -------------- | ------------------ |
   | Python 3.8     | [sürüm 3. x](../articles/azure-functions/functions-run-local.md#v2) |
   | Python 3,6<br/>Python 3.7 | [Sürüm 2.7.1846 veya sonraki bir sürüm](../articles/azure-functions/functions-run-local.md#v2) |
  
::: zone-end

+ [Azure CLI](/cli/azure/install-azure-cli) sürüm 2,4 veya sonraki bir sürümü. 
::: zone pivot="programming-language-javascript,programming-language-typescript"
+ [Node.js](https://nodejs.org/), ETKIN LTS ve bakım LTS sürümleri (8.11.1 ve 10.14.1 önerilir).
::: zone-end

::: zone pivot="programming-language-python"
+ Azure Işlevleri tarafından desteklenen Python [3,8 (64-bit)](https://www.python.org/downloads/release/python-382/), [python 3,7 (64-](https://www.python.org/downloads/release/python-375/)bit), [Python 3,6 (64-bit](https://www.python.org/downloads/release/python-368/)). 
::: zone-end
::: zone pivot="programming-language-powershell"
+ [.NET Core SDK 3,1](https://www.microsoft.com/net/download)
::: zone-end
::: zone pivot="programming-language-java"  
+ [Java geliştirici seti](/azure/developer/java/fundamentals/java-jdk-long-term-support), sürüm 8 veya 11. 

+ [Apache Maven](https://maven.apache.org), sürüm 3,0 veya üzeri.

::: zone-end
::: zone pivot="programming-language-other"
+ Kullanmakta olduğunuz dilin geliştirme araçları. Bu öğretici, örnek olarak [R programlama dilini](https://www.r-project.org/) kullanır.
::: zone-end