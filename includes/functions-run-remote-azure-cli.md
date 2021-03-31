---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 10/18/2020
ms.author: glenga
ms.openlocfilehash: d1931614356a313334d712713965346e843a403d
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2021
ms.locfileid: "93425196"
---
## <a name="invoke-the-function-on-azure"></a>Azure 'da işlevi çağırma

İşleviniz bir HTTP tetikleyicisi kullandığından, tarayıcıda URL 'sine veya kıvrıcı gibi bir araçla HTTP isteği yaparak bu uygulamayı çağırabilirsiniz. 

# <a name="browser"></a>[Tarayıcı](#tab/browser)

Yayımla komutunun çıktısında gösterilen tüm **ÇAĞıRMA URL** 'sini, sorgu parametresini ekleyerek bir tarayıcı adres çubuğuna kopyalayın `&name=Functions` . , İşlevi yerel olarak çalıştırdığınızda tarayıcı benzer bir çıktı görüntülemelidir.

![Azure üzerinde çalışan işlevin çıktısı bir tarayıcıda](./media/functions-run-remote-azure-cli/function-test-cloud-browser.png)

# <a name="curl"></a>[curl](#tab/curl)

[`curl`](https://curl.haxx.se/)Parametresini ekleyerek **Invoke URL 'si** ile çalıştırın `&name=Functions` . Komutun çıktısı, "Hello Functions" metni olmalıdır.

![İşlevin çıktısı, Azure 'da kıvrımlı kullanarak çalıştırıldı](./media/functions-run-remote-azure-cli/function-test-cloud-curl.png)

---
