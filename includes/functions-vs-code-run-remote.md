---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 10/01/2020
ms.author: glenga
ms.openlocfilehash: 55a75651b724a4fe975f655958e36fbd40e35db7
ms.sourcegitcommit: ad83be10e9e910fd4853965661c5edc7bb7b1f7c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/06/2020
ms.locfileid: "96748224"
---
## <a name="run-the-function-in-azure"></a>İşlevi Azure 'da çalıştırma

1. Yan çubuktaki **Azure: Functions** bölümüne geri döndüğünüzde, aboneliğinizin altındaki yeni işlev uygulamasını genişletin. **İşlevler**' i genişletin, **httpexample** üzerinde sağ tıklayın (Windows) veya <kbd>CTRL</kbd> + tıklat (MacOS) ve ardından **işlev URL 'sini Kopyala**' yı seçin.

    ![Yeni HTTP tetikleyicisinin işlev URL 'sini kopyalayın](./media/functions-vs-code-run-remote/function-copy-endpoint-url.png)

1. HTTP isteğinin bu URL 'sini tarayıcınızın adres çubuğuna yapıştırın, `name` sorgu dizesini `?name=Functions` Bu URL 'nin sonuna ekleyin ve sonra isteği yürütün. HTTP tarafından tetiklenen işlevinizi çağıran URL aşağıdaki biçimde olmalıdır:

    ```http
    http://<FUNCTION_APP_NAME>.azurewebsites.net/api/HttpExample?name=Functions
    ```

    Aşağıdaki örnek, bu işlev tarafından döndürülen uzak GET isteğine tarayıcıda yapılan yanıtı gösterir:

    ![Tarayıcıdaki işlev yanıtı](./media/functions-vs-code-run-remote/functions-test-remote-browser.png)
