---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 03/25/2020
ms.author: glenga
ms.openlocfilehash: 9e9f9c71701ceb1c76bc162f22e166b4565e731b
ms.sourcegitcommit: bcb962e74ee5302d0b9242b1ee006f769a94cfb8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/07/2020
ms.locfileid: "86062661"
---
## <a name="configure-your-local-environment"></a>Yerel ortamınızı yapılandırma

Başlamadan önce aşağıdakilere sahip olmanız gerekir:

+ Etkin aboneliği olan bir Azure hesabı. [Ücretsiz hesap oluşturun](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio).

::: zone pivot="programming-language-csharp,programming-language-javascript,programming-language-typescript,programming-language-powershell,programming-language-java"  
+ [Azure Functions Core Tools](../articles/azure-functions/functions-run-local.md#v2) sürüm 2.7.1846 veya sonraki bir 2. x sürümü.
::: zone-end  
::: zone pivot="programming-language-python"
+ Yüklü Python sürümüne karşılık gelen Azure Functions Core Tools sürümü:

   | Python sürümü | Çekirdek araçları sürümü |
   | -------------- | ------------------ |
   | Python 3,8     | [sürüm 3. x](../articles/azure-functions/functions-run-local.md#v2) |
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
+ [PowerShell Core](/powershell/scripting/install/installing-powershell-core-on-windows)

+ [.NET Core SDK 2.2 +](https://www.microsoft.com/net/download)
::: zone-end
::: zone pivot="programming-language-java"  
+ [Java geliştirici seti](https://aka.ms/azure-jdks), sürüm 8.

+ [Apache Maven](https://maven.apache.org), sürüm 3,0 veya üzeri.

> [!IMPORTANT]
> Bu hızlı başlangıcın tamamlanabilmesi için JAVA_HOME ortam değişkeni JDK’nin yükleme konumu olarak ayarlanmalıdır.
::: zone-end
