---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 03/06/2020
ms.author: glenga
ms.openlocfilehash: b4b2409928b6a4196738c7cc6c7040e781d34686
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80056553"
---
1. İşlevinizi çalıştırmak için Visual Studio'da F5 tuşuna basın. Araçların HTTP isteklerini işleyebileceği için bir güvenlik duvarı özel durumu etkinleştirmeniz gerekebilir. Bir işlevi yerel olarak çalıştırdığınızda yetkilendirme düzeyleri hiçbir zaman zorlanmaz.

2. Azure İşlevleri çalışma zamanı çıktısından işlevinizin URL'sini kopyalayın.

    ![Azure yerel çalışma zamanı](./media/functions-run-function-test-local-vs/functions-debug-local-vs.png)

3. HTTP isteğinin URL’sini tarayıcınızın adres çubuğuna yapıştırın. Sorgu dizesini `?name=<YOUR_NAME>` bu URL'ye ekleyin ve isteği çalıştırın. Aşağıdaki resim, tarayıcıdaki işlevi tarafından döndürülen yerel GET isteğine verilen yanıtı gösterir: 

    ![Tarayıcıdaki işlev localhost yanıtı](./media/functions-run-function-test-local-vs/functions-run-browser-local-vs.png)

4. Hata ayıklamayı durdurmak için Visual Studio'da Shift+F5 tuşuna basın.