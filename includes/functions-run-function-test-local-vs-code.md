---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 01/12/2020
ms.author: glenga
ms.openlocfilehash: f4075b8d05c179e8115ff46c9f82751817372491
ms.sourcegitcommit: 5d6ce6dceaf883dbafeb44517ff3df5cd153f929
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2020
ms.locfileid: "76842225"
---
## <a name="run-the-function-locally"></a>İşlevi yerel olarak çalıştırma

Visual Studio Code, Azure 'da yayımlamadan önce bu projeyi yerel geliştirme bilgisayarınızda çalıştırmanıza olanak sağlamak için [Azure Functions Core Tools](../articles/azure-functions/functions-run-local.md) tümleştirilir.

1. İşlevinizi çağırmak için F5 tuşuna basarak işlev uygulaması projesini başlatın. Temel Araçlar’daki çıktı, **Terminal** panelinde görüntülenir.

1. Azure Functions Core Tools henüz yüklemediyseniz, sorulduğunda **yükleme** ' yi seçin. Temel araçlar yüklendiğinde, uygulamanız **Terminal** panelinden başlatılır.

1. **Terminal** panelinde, HTTP ile tetiklenen işlevinizin URL uç noktasını kopyalayın. 

    ![Azure yerel çıktısı](./media/functions-run-function-test-local-vs-code/functions-vscode-f5.png)

1. HTTP isteğinin URL’sini tarayıcınızın adres çubuğuna yapıştırın. `?name=<yourname>` sorgu dizesini bu URL 'ye ekleyin ve GET isteğini yürütün. 

1. Bir tarayıcıda aşağıdaki gibi görünen bir yanıt döndürülür:

    ![Tarayıcıdaki işlev localhost yanıtı](./media/functions-run-function-test-local-vs-code/functions-test-local-browser.png)

1. Ana araçları durdurmak ve hata ayıklayıcının bağlantısını kesmek için SHIFT + F5 tuşlarına basın.
