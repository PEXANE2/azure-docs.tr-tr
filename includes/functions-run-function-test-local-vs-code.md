---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 01/12/2020
ms.author: glenga
ms.openlocfilehash: f4af3c202d4f00c4ac3041921175c92226f0db7c
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "76964152"
---
## <a name="run-the-function-locally"></a>İşlevi yerel olarak çalıştırma

Visual Studio Code, Azure 'da yayımlamadan önce bu projeyi yerel geliştirme bilgisayarınızda çalıştırmanıza olanak sağlamak için [Azure Functions Core Tools](../articles/azure-functions/functions-run-local.md) tümleştirilir.

1. İşlevinizi çağırmak için F5 tuşuna basarak işlev uygulaması projesini başlatın. Temel Araçlar’daki çıktı, **Terminal** panelinde görüntülenir.

1. Azure Functions Core Tools henüz yüklemediyseniz, sorulduğunda **yükleme** ' yi seçin. Temel araçlar yüklendiğinde, uygulamanız **Terminal** panelinden başlatılır. HTTP ile tetiklenen işlevinizin URL uç noktasını yerel olarak çalışan bir şekilde görebilirsiniz. 

    ![Azure yerel çıktısı](./media/functions-run-function-test-local-vs-code/functions-vscode-f5.png)

1. Temel araçlar çalışırken, sorgu dizesi içeren bir GET isteği yürütmek için aşağıdaki URL 'ye gidin `?name=Functions` .

    <http://localhost:7071/api/HttpExample?name=Functions>

1. Bir tarayıcıda aşağıdaki gibi görünen bir yanıt döndürülür:

    ![Tarayıcıdaki işlev localhost yanıtı](./media/functions-run-function-test-local-vs-code/functions-test-local-browser.png)

1. İstek hakkındaki bilgiler, **Terminal** panelinde gösterilir.

    ![Terminal panelinde işlev yürütme](./media/functions-run-function-test-local-vs-code/function-execution-terminal.png)

1. Temel araçları durdurmak ve hata ayıklayıcının bağlantısını kesmek için CTRL + C tuşlarına basın.
