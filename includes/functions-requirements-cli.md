---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 03/25/2020
ms.author: glenga
ms.openlocfilehash: a84f0a92703d1b626710cfc4dcfa2820bc58bda6
ms.sourcegitcommit: b129186667a696134d3b93363f8f92d175d51475
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80673171"
---
## <a name="configure-your-local-environment"></a>Yerel ortamınızı yapılandırın

Başlamadan önce aşağıdakileri almalısınız:

+ Etkin bir aboneliği olan bir Azure hesabı. [Ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio)

::: zone pivot="programming-language-csharp,programming-language-javascript,programming-language-typescript,programming-language-powershell"  
+ [Azure İşletme Temel Araçları](../articles/azure-functions/functions-run-local.md#v2) sürümü 2.7.1846 veya daha sonraki bir 2.x sürümü.
::: zone-end  
::: zone pivot="programming-language-python"
+ Python 3.6 ve 3.7, [Azure İşlevler Çekirdek Araçları](../articles/azure-functions/functions-run-local.md#v2) sürümü 2.7.1846 veya daha sonraki bir 2.x sürümü gerektirir. Python 3.8, Çekirdek Araçları'nın [3.x sürümünü](../articles/azure-functions/functions-run-local.md#v2) gerektirir.
::: zone-end

+ [Azure CLI](/cli/azure/install-azure-cli) sürümü 2.0.76 veya sonrası. 
::: zone pivot="programming-language-javascript,programming-language-typescript"
+ [Node.js](https://nodejs.org/), Active LTS ve Maintenance LTS versiyonları (8.11.1 ve 10.14.1 önerilir).
::: zone-end

::: zone pivot="programming-language-python"
+ [Python 3.8 (64-bit)](https://www.python.org/downloads/release/python-382/), [Python 3.7 (64-bit)](https://www.python.org/downloads/release/python-375/), [Python 3.6 (64-bit)](https://www.python.org/downloads/release/python-368/), Azure Fonksiyonları tarafından desteklenir. 
::: zone-end
::: zone pivot="programming-language-powershell"
+ [PowerShell Core](/powershell/scripting/install/installing-powershell-core-on-windows)

+ [.NET Çekirdek SDK 2.2+](https://www.microsoft.com/net/download)
::: zone-end
::: zone pivot="programming-language-java"  
+ [Java Geliştirici Kiti](https://aka.ms/azure-jdks), sürüm 8.

+ [Apache Maven](https://maven.apache.org), sürüm 3.0 veya üzeri.

> [!IMPORTANT]
> Bu hızlı başlangıcın tamamlanabilmesi için JAVA_HOME ortam değişkeni JDK’nin yükleme konumu olarak ayarlanmalıdır.
::: zone-end