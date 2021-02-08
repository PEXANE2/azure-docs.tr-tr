---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 01/28/2021
ms.author: glenga
ms.openlocfilehash: 121b10cc568cce089c90e66b9c6f292c74f4acbe
ms.sourcegitcommit: 4784fbba18bab59b203734b6e3a4d62d1dadf031
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/08/2021
ms.locfileid: "99809578"
---
## <a name="run-the-function-in-azure"></a>İşlevi Azure 'da çalıştırma

1. Yan çubuktaki **Azure: Functions** alanına geri döndüğünüzde aboneliğinizi, yeni işlev uygulamanızı ve **işlevlerinizi** genişletin. İşlevi sağ tıklatın (Windows) veya <kbd>CTRL</kbd> + tıklatın (MacOS) `HttpExample` ve **Şimdi işlevi Çalıştır...** seçeneğini belirleyin.

    :::image type="content" source="media/functions-vs-code-run-remote/execute-function-now.png" alt-text="İşlevi Azure 'da şimdi çalıştırın Visual Studio Code":::

1. **İstek gövdesi gir** bölümünde istek iletisi gövdesi değerini görürsünüz `{ "name": "Azure" }` . Bu istek iletisini işlevinizde göndermek için ENTER tuşuna basın.  

1. İşlev Azure 'da yürütüldüğünde ve bir yanıt döndürdüğünde, Visual Studio Code bir bildirim oluşturulur.
