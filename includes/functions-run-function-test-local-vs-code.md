---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 09/28/2020
ms.author: glenga
ms.openlocfilehash: 43da0ea4ddfc5410425465d436522523739218fe
ms.sourcegitcommit: b48e8a62a63a6ea99812e0a2279b83102e082b61
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/28/2020
ms.locfileid: "91408586"
---
## <a name="run-the-function-locally"></a>İşlevi yerel olarak çalıştırma

Visual Studio Code, Azure 'da yayımlamadan önce bu projeyi yerel geliştirme bilgisayarınızda çalıştırmanıza olanak sağlamak için [Azure Işlevleri temel araçları](../articles/azure-functions/functions-run-local.md) ile tümleşir.

1. İşlevinizi çağırmak için <kbd>F5</kbd> tuşuna basarak işlev uygulaması projesini başlatın. Temel Araçlar’daki çıktı, **Terminal** panelinde görüntülenir.

1. Azure Functions Core Tools henüz yüklemediyseniz, sorulduğunda **yükleme** ' yi seçin. Temel araçlar yüklendiğinde, uygulamanız **Terminal** panelinden başlatılır. HTTP ile tetiklenen işlevinizin URL uç noktasını yerel olarak çalışan bir şekilde görebilirsiniz.

    ![Yerel işlev VS Code çıkışı](./media/functions-run-function-test-local-vs-code/functions-vscode-f5.png)

1. Temel araçlar çalışırken, sorgu dizesi içeren bir GET isteği yürütmek için aşağıdaki URL 'ye gidin `?name=Functions` .

    `http://localhost:7071/api/HttpExample?name=Functions`

1. Bir tarayıcıda aşağıdaki gibi görünen bir yanıt döndürülür:

    ![Tarayıcı-localhost örnek çıktısı](./media/functions-run-function-test-local-vs-code/functions-test-local-browser.png)

1. İstek hakkındaki bilgiler, **Terminal** panelinde gösterilir.

    ![Görev ana bilgisayarı başlatma-VS Code Terminal çıkışı](./media/functions-run-function-test-local-vs-code/function-execution-terminal.png)

1. Temel araçları durdurmak ve hata ayıklayıcının bağlantısını kesmek için <kbd>CTRL + C</kbd> tuşlarına basın.
