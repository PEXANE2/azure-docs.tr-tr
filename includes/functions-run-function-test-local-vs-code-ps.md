---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 02/19/2020
ms.author: glenga
ms.openlocfilehash: edf5fc33ec14d41630462cca1a4ace0663473196
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78191075"
---
## <a name="run-the-function-locally"></a>İşlevi yerel olarak çalıştırma

Azure İşlevler Çekirdek Araçları, bir Azure İşlevler projesini yerel olarak çalıştırıp hata ayıklamanıza izin vermek için Visual Studio Kodu ile tümleşir. Visual Studio Code'da hata ayıklama hakkında ayrıntılı bilgi için [PowerShell Azure Fonksiyonlarını yerel olarak hata ayıklama](../articles/azure-functions/functions-debug-powershell-local.md)bölümüne bakın. 

1. Fonksiyon uygulaması projesini başlatmak için F5 tuşuna basın. Temel Araçlar’daki çıktı, **Terminal** panelinde görüntülenir.

1. **Terminal** panelinde, HTTP ile tetiklenen işlevinizin URL uç noktasını kopyalayın.

    ![Azure yerel çıktısı](./media/functions-run-function-test-local-vs-code-ps/functions-vscode-f5.png)

1. Sorgu dizesini `?name=<yourname>` bu URL'ye `Invoke-RestMethod` ekleyin ve ardından isteği yürütmek için ikinci bir PowerShell komut istemini aşağıdaki gibi kullanın:

    ```powershell
    PS > Invoke-RestMethod -Method Get -Uri http://localhost:7071/api/HttpTrigger?name=PowerShell
    Hello PowerShell
    ```

    Bir tarayıcıdan GET isteğini aşağıdaki URL'den de yürütebilirsiniz:

    <http://localhost:7071/api/HttpExample?name=PowerShell>

    Sorgu parametresi olarak veya gövdede bir `name` parametre geçmeden HttpTrigger bitiş noktasını çağırdığınızda, işlev bir `BadRequest` hata döndürür. run.ps1'deki kodu gözden geçirdiğinizde, bu hatanın tasarım tarafından oluştuğunu görürsünüz.

1. İstek le ilgili bilgiler **Terminal** panelinde gösterilir.

    ![Terminal panelinde işlev yürütme](./media/functions-run-function-test-local-vs-code-ps/function-execution-terminal.png)

1. Bittiğinde, Çekirdek Araçlarını durdurmak için **Ctrl + C** tuşuna basın.

İşlevin yerel bilgisayarınızda düzgün çalıştığını doğruladıktan sonra, projeyi Azure'da yayımlamanın zamanı gelmiştir.