---
title: Azure portalında ilk işlevinizi oluşturma
description: Azure portalını kullanarak sunucusuz yürütme için ilk Azure İşlevinizi oluşturma hakkında bilgi edinin.
ms.assetid: 96cf87b9-8db6-41a8-863a-abb828e3d06d
ms.topic: how-to
ms.date: 03/26/2020
ms.custom: devx-track-csharp, mvc, devcenter, cc996988-fb4f-47
ms.openlocfilehash: 770b1076f1a711cd863c5d3d468a3ec87ea54e7b
ms.sourcegitcommit: 4913da04fd0f3cf7710ec08d0c1867b62c2effe7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/14/2020
ms.locfileid: "88212721"
---
# <a name="create-your-first-function-in-the-azure-portal"></a>Azure portalında ilk işlevinizi oluşturma

Azure Işlevleri, öncelikle bir sanal makine (VM) oluşturmak veya bir Web uygulaması yayımlamak zorunda kalmadan kodunuzu sunucusuz bir ortamda çalıştırmanızı sağlar. Bu makalede, Azure Işlevleri 'ni kullanarak Azure portal bir "Hello World" HTTP tetikleyici işlevi oluşturma hakkında bilgi edineceksiniz.

[İşlevlerinizi yerel olarak geliştirmenizi](functions-develop-local.md) ve Azure 'da bir işlev uygulamasında yayımlamayı öneririz.  
Seçtiğiniz yerel geliştirme ortamı ve diliniz ile çalışmaya başlamak için aşağıdaki bağlantılardan birini kullanın:

| Visual Studio Code | Terminal/komut istemi | Visual Studio |
| --- | --- | --- |
|  &bull;&nbsp;[C ile çalışmaya başlama #](./functions-create-first-function-vs-code.md?pivots=programming-language-csharp)<br/>&bull;&nbsp;[Java kullanmaya başlayın](./functions-create-first-function-vs-code.md?pivots=programming-language-java)<br/>&bull;&nbsp;[JavaScript ile çalışmaya başlama](./functions-create-first-function-vs-code.md?pivots=programming-language-javascript)<br/>&bull;&nbsp;[PowerShell 'i kullanmaya başlama](./functions-create-first-function-vs-code.md?pivots=programming-language-powershell)<br/>&bull;&nbsp;[Python ile çalışmaya başlama](./functions-create-first-function-vs-code.md?pivots=programming-language-python) |&bull;&nbsp;[C ile çalışmaya başlama #](./functions-create-first-azure-function-azure-cli.md?pivots=programming-language-csharp)<br/>&bull;&nbsp;[Java kullanmaya başlayın](./functions-create-first-azure-function-azure-cli.md?pivots=programming-language-java)<br/>&bull;&nbsp;[JavaScript ile çalışmaya başlama](./functions-create-first-azure-function-azure-cli.md?pivots=programming-language-javascript)<br/>&bull;&nbsp;[PowerShell 'i kullanmaya başlama](./functions-create-first-azure-function-azure-cli.md?pivots=programming-language-powershell)<br/>&bull;&nbsp;[Python ile çalışmaya başlama](./functions-create-first-azure-function-azure-cli.md?pivots=programming-language-python) | [C ile çalışmaya başlama #](functions-create-your-first-function-visual-studio.md) |

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="sign-in-to-azure"></a>Azure’da oturum açma

Azure hesabınızla [Azure portalında](https://portal.azure.com) oturum açın.

## <a name="create-a-function-app"></a>İşlev uygulaması oluşturma

İşlevlerinizin yürütülmesini barındıran bir işlev uygulamasına sahip olmanız gerekir. İşlev uygulaması, kaynakların daha kolay yönetilmesi, dağıtılması, ölçeklendirilmesi ve paylaşılması için işlevleri bir mantıksal birim olarak gruplandırmanıza olanak tanır.

[!INCLUDE [Create function app Azure portal](../../includes/functions-create-function-app-portal.md)]

Sonra, yeni işlev uygulamasında bir işlev oluşturun.

## <a name="create-an-http-trigger-function"></a><a name="create-function"></a>HTTP tetikleyici işlevi oluşturma

1. **İşlevler** penceresinin sol menüsünde **işlevler**' i seçin ve ardından üst menüden **Ekle** ' yi seçin. 
 
1. **Yeni işlev** penceresinden **http tetikleyicisi**' ni seçin.

    ![HTTP tetikleyici işlevini seçin](./media/functions-create-first-azure-function/function-app-select-http-trigger.png)

1. **Yeni işlev** penceresinde, **yeni işlev**için varsayılan adı kabul edin veya yeni bir ad girin. 

1. **Yetkilendirme düzeyi** açılan listesinden **anonim** ' i seçin ve ardından **işlev oluştur**' u seçin.

    Azure, HTTP tetikleyici işlevini oluşturur. Artık bir HTTP isteği göndererek yeni işlevi çalıştırabilirsiniz.

## <a name="test-the-function"></a>İşlevi test etme

1. Yeni HTTP tetikleyici işlevinizde, sol menüden **Code + test** ' i seçin ve ardından üstteki menüden **Işlev URL 'sini al** ' ı seçin.

    ![İşlev URL 'sini Al ' ı seçin](./media/functions-create-first-azure-function/function-app-select-get-function-url.png)

1. **İşlev URL 'Sini al** iletişim kutusunda, açılan listeden **varsayılan** ' ı seçin ve ardından **Panoya Kopyala** simgesini seçin. 

    ![Azure portalından işlev URL’sini kopyalama](./media/functions-create-first-azure-function/function-app-develop-tab-testing.png)

1. İşlev URL'sini tarayıcınızın adres çubuğuna yapıştırın. Sorgu dizesi değerini `?name=<your_name>` Bu URL 'nin sonuna ekleyin ve isteği çalıştırmak Için ENTER 'a basın. 

    Aşağıdaki örnekte tarayıcıdaki yanıt gösterilmektedir:

    ![Tarayıcıdaki işlev yanıtı.](./media/functions-create-first-azure-function/function-app-browser-testing.png)

    İstek URL’si, işlevinize HTTP üzerinden erişmek için varsayılan olarak gerekli olan bir anahtar içerir.

1. İşleviniz çalıştığında, izleme bilgileri günlüklere yazılır. İzleme çıktısını görmek için, portalda **Code + test** sayfasına dönüp sayfanın altındaki **Günlükler** okunu genişletin.

   ![Azure portalında İşlevler günlük görüntüleyicisi.](./media/functions-create-first-azure-function/function-view-logs.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [Clean-up resources](../../includes/functions-quickstart-cleanup.md)]

## <a name="next-steps"></a>Sonraki adımlar

[!INCLUDE [Next steps note](../../includes/functions-quickstart-next-steps.md)]

