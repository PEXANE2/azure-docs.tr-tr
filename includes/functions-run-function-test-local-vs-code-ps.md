---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 02/19/2020
ms.author: glenga
ms.openlocfilehash: edf5fc33ec14d41630462cca1a4ace0663473196
ms.sourcegitcommit: 225a0b8a186687154c238305607192b75f1a8163
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/29/2020
ms.locfileid: "78191075"
---
## <a name="run-the-function-locally"></a>İşlevi yerel olarak çalıştırma

Azure Functions Core Tools, bir Azure Işlevleri projesinde yerel olarak çalıştırmanıza ve hata ayıklamanıza olanak sağlamak için Visual Studio Code tümleştirilir. Visual Studio Code 'de hata ayıklama hakkında ayrıntılar için bkz. [PowerShell Azure işlevleri 'nde yerel olarak hata ayıklama](../articles/azure-functions/functions-debug-powershell-local.md). 

1. İşlev uygulaması projesini başlatmak için F5 tuşuna basın. Temel Araçlar’daki çıktı, **Terminal** panelinde görüntülenir.

1. **Terminal** panelinde, HTTP ile tetiklenen işlevinizin URL uç noktasını kopyalayın.

    ![Azure yerel çıktısı](./media/functions-run-function-test-local-vs-code-ps/functions-vscode-f5.png)

1. `?name=<yourname>` sorgu dizesini bu URL 'ye ekleyin ve ardından ikinci bir PowerShell komut isteminde `Invoke-RestMethod` kullanarak isteği yürütün:

    ```powershell
    PS > Invoke-RestMethod -Method Get -Uri http://localhost:7071/api/HttpTrigger?name=PowerShell
    Hello PowerShell
    ```

    Aşağıdaki URL 'den bir tarayıcıdan GET isteğini de yürütebilirsiniz:

    <http://localhost:7071/api/HttpExample?name=PowerShell>

    Sorgu parametresi veya gövdede bir `name` parametresi geçirmeden HttpTrigger uç noktasını çağırdığınızda, işlev bir `BadRequest` hatası döndürür. Run. ps1 içinde kodu gözden geçirdikten sonra, bu hatanın tasarım tarafından oluştuğunu görürsünüz.

1. İstek hakkındaki bilgiler, **Terminal** panelinde gösterilir.

    ![Terminal panelinde işlev yürütme](./media/functions-run-function-test-local-vs-code-ps/function-execution-terminal.png)

1. İşiniz bittiğinde, temel araçları durdurmak için **CTRL + C** tuşlarına basın.

İşlevin yerel bilgisayarınızda düzgün çalıştığını doğruladıktan sonra, projeyi Azure'da yayımlamanın zamanı gelmiştir.