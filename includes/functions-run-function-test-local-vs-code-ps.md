---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 01/16/2020
ms.author: glenga
ms.openlocfilehash: 916aa2552e5dd004ec767df98ce7c78f7320efd0
ms.sourcegitcommit: 42517355cc32890b1686de996c7913c98634e348
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/02/2020
ms.locfileid: "76964151"
---
## <a name="run-the-function-locally"></a>İşlevi yerel olarak çalıştırma

Azure Functions Core Tools, bir Azure Işlevleri projesinde yerel olarak çalıştırmanıza ve hata ayıklamanıza olanak sağlamak için Visual Studio Code tümleştirilir.  

1. İşlevinizde hata ayıklamak için, hata ayıklayıcıyı iliştirmek istediğiniz işlev kodundaki [`Wait-Debugger`](/powershell/module/microsoft.powershell.utility/wait-debugger?view=powershell-6) cmdlet 'ine bir çağrı ekleyin ve ardından F5 tuşuna basarak işlev uygulaması projesini başlatın ve hata ayıklayıcıyı ekleyin. Temel Araçlar’daki çıktı, **Terminal** panelinde görüntülenir.

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

1. Hata ayıklamayı durdurmak için, temel araçları durdurmak için CTRL + C tuşlarına basın.

İşlevin yerel bilgisayarınızda düzgün çalıştığını doğruladıktan sonra, projeyi Azure'da yayımlamanın zamanı gelmiştir.

> [!NOTE]
> İşlevlerinizi Azure 'da yayımlamadan önce `Wait-Debugger` yapılan çağrıları kaldırmayı unutmayın. 