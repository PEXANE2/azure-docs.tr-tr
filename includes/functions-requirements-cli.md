---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 03/25/2020
ms.author: glenga
ms.openlocfilehash: 3929ffa81a8b558d8cb0dbd74135acc9cf1ea47b
ms.sourcegitcommit: f57297af0ea729ab76081c98da2243d6b1f6fa63
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2020
ms.locfileid: "82876031"
---
## <a name="configure-your-local-environment"></a>Yerel ortamınızı yapılandırma

Başlamadan önce aşağıdakilere sahip olmanız gerekir:

+ Etkin aboneliği olan bir Azure hesabı. [Ücretsiz hesap oluşturun](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio).

::: zone pivot="programming-language-csharp,programming-language-javascript,programming-language-typescript,programming-language-powershell"  
+ [Azure Functions Core Tools](../articles/azure-functions/functions-run-local.md#v2) sürüm 2.7.1846 veya sonraki bir 2. x sürümü.
::: zone-end  
::: zone pivot="programming-language-python"
+ Python 3,6 ve 3,7 için [Azure Functions Core Tools](../articles/azure-functions/functions-run-local.md#v2) sürüm 2.7.1846 veya sonraki bir 2. x sürümü gerekir. Python 3,8, çekirdek araçların [3. x sürümünü](../articles/azure-functions/functions-run-local.md#v2) gerektirir.
::: zone-end

+ [Azure CLI](/cli/azure/install-azure-cli) sürüm 2,4 veya sonraki bir sürümü. 
::: zone pivot="programming-language-javascript,programming-language-typescript"
+ [Node. js](https://nodejs.org/), ETKIN LTS ve bakım LTS sürümleri (8.11.1 ve 10.14.1 önerilir).
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
