---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 01/12/2020
ms.author: glenga
ms.openlocfilehash: f4af3c202d4f00c4ac3041921175c92226f0db7c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "76964152"
---
## <a name="run-the-function-locally"></a>İşlevi yerel olarak çalıştırma

Visual Studio Code, Azure'da yayımlamadan önce bu projeyi yerel geliştirme bilgisayarınızda çalıştırmanıza izin vermek için [Azure İşlevler Temel Araçları](../articles/azure-functions/functions-run-local.md) ile tümleşir.

1. İşlevinizi aramak için, işlev uygulaması projesini başlatmak için F5 tuşuna basın. Temel Araçlar’daki çıktı, **Terminal** panelinde görüntülenir.

1. Azure İşlevler Temel Araçları'nı henüz yüklemediyseniz, komut istemiyle **Yükle'yi** seçin. Çekirdek Araçları yüklendiğinde, uygulamanız **Terminal** panelinde başlar. HTTP tetiklenen işlevinizin URL bitiş noktasının yerel olarak çalıştığını görebilirsiniz. 

    ![Azure yerel çıktısı](./media/functions-run-function-test-local-vs-code/functions-vscode-f5.png)

1. Çekirdek Araçları çalışırken, sorgu dizesini içeren `?name=Functions` bir GET isteğini yürütmek için aşağıdaki URL'ye gidin.

    <http://localhost:7071/api/HttpExample?name=Functions>

1. Bir yanıt, bir tarayıcıda aşağıdaki gibi görünür döndürülür:

    ![Tarayıcıdaki işlev localhost yanıtı](./media/functions-run-function-test-local-vs-code/functions-test-local-browser.png)

1. İstek le ilgili bilgiler **Terminal** panelinde gösterilir.

    ![Terminal panelinde işlev yürütme](./media/functions-run-function-test-local-vs-code/function-execution-terminal.png)

1. Çekirdek Araçları durdurmak ve hata ayıklamanın bağlantısını kesmek için Ctrl + C tuşuna basın.
