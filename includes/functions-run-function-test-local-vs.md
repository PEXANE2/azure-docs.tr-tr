---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 03/06/2020
ms.author: glenga
ms.openlocfilehash: b4b2409928b6a4196738c7cc6c7040e781d34686
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "80056553"
---
1. İşlevinizi çalıştırmak için, Visual Studio 'da F5 tuşuna basın. Araçların HTTP isteklerini işleyebilmesi için bir güvenlik duvarı özel durumu etkinleştirmeniz gerekebilir. Yerel olarak bir işlev çalıştırdığınızda yetkilendirme düzeyleri hiçbir zaman zorlanmaz.

2. Azure İşlevleri çalışma zamanı çıktısından işlevinizin URL'sini kopyalayın.

    ![Azure yerel çalışma zamanı](./media/functions-run-function-test-local-vs/functions-debug-local-vs.png)

3. HTTP isteğinin URL’sini tarayıcınızın adres çubuğuna yapıştırın. Sorgu dizesini `?name=<YOUR_NAME>` bu URL 'ye ekleyin ve isteği çalıştırın. Aşağıdaki görüntüde, bu işlevin döndürdüğü yerel GET isteğine tarayıcıda yapılan yanıt gösterilmektedir: 

    ![Tarayıcıdaki işlev localhost yanıtı](./media/functions-run-function-test-local-vs/functions-run-browser-local-vs.png)

4. Hata ayıklamayı durdurmak için, Visual Studio 'da SHIFT + F5 tuşlarına basın.