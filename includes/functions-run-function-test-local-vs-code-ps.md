---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 02/19/2020
ms.author: glenga
ms.openlocfilehash: ca8da227c97a44abe14354a5c530d508ce357884
ms.sourcegitcommit: 56cbd6d97cb52e61ceb6d3894abe1977713354d9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/20/2020
ms.locfileid: "88702957"
---
## <a name="run-the-function-locally"></a>İşlevi yerel olarak çalıştırma

Azure Functions Core Tools, bir Azure Işlevleri projesinde yerel olarak çalıştırmanıza ve hata ayıklamanıza olanak sağlamak için Visual Studio Code tümleştirilir. Visual Studio Code 'de hata ayıklama hakkında ayrıntılar için bkz. [PowerShell Azure işlevleri 'nde yerel olarak hata ayıklama](../articles/azure-functions/functions-debug-powershell-local.md). 

1. İşlev uygulaması projesini başlatmak için F5 tuşuna basın. Temel Araçlar’daki çıktı, **Terminal** panelinde görüntülenir.

1. **Terminal** panelinde, HTTP ile tetiklenen işlevinizin URL uç noktasını kopyalayın.

    ![Azure yerel çıktısı](./media/functions-run-function-test-local-vs-code-ps/functions-vscode-f5.png)

1. Sorgu dizesini `?name=<yourname>` Bu URL 'ye ekleyin ve sonra `Invoke-RestMethod` aşağıdaki gibi, isteği yürütmek için Ikinci bir PowerShell komut isteminde kullanın:

    ```powershell
    PS > Invoke-RestMethod -Method Get -Uri http://localhost:7071/api/HttpTrigger?name=PowerShell
    Hello PowerShell
    ```

    Aşağıdaki URL 'den bir tarayıcıdan GET isteğini de yürütebilirsiniz:

    `http://localhost:7071/api/HttpExample?name=PowerShell`

    `name`Sorgu parametresi ya da gövdede bir parametre geçirmeden HttpTrigger uç noktasını çağırdığınızda, işlev bir `BadRequest` hata döndürür. run.ps1 kodu gözden geçirdikten sonra, bu hatanın tasarım tarafından oluştuğunu görürsünüz.

1. İstek hakkındaki bilgiler, **Terminal** panelinde gösterilir.

    ![Terminal panelinde işlev yürütme](./media/functions-run-function-test-local-vs-code-ps/function-execution-terminal.png)

1. İşiniz bittiğinde, temel araçları durdurmak için **CTRL + C** tuşlarına basın.

İşlevin yerel bilgisayarınızda düzgün çalıştığını doğruladıktan sonra, projeyi Azure'da yayımlamanın zamanı gelmiştir.