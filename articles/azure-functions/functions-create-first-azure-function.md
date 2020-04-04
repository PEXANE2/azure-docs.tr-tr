---
title: Azure portalında ilk işlevinizi oluşturma
description: Azure portalını kullanarak sunucusuz yürütme için ilk Azure İşlevinizi oluşturma hakkında bilgi edinin.
ms.assetid: 96cf87b9-8db6-41a8-863a-abb828e3d06d
ms.topic: quickstart
ms.date: 03/26/2020
ms.custom: mvc, devcenter, cc996988-fb4f-47
ms.openlocfilehash: 417ad96dc3dea25e322dbdb4d81c034a9b9c1e80
ms.sourcegitcommit: 62c5557ff3b2247dafc8bb482256fef58ab41c17
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2020
ms.locfileid: "80656831"
---
# <a name="create-your-first-function-in-the-azure-portal"></a>Azure portalında ilk işlevinizi oluşturma

Azure İşlevleri, önce sanal bir makine (VM) oluşturmanıza veya bir web uygulaması yayınlamak zorunda kalmadan kodunuzu sunucusuz bir ortamda çalıştırmanızı sağlar. Bu makalede, Azure portalında "merhaba dünyası" HTTP tetiklenen bir işlev oluşturmak için Azure Işlevlerini nasıl kullanacağınızı öğrenirsiniz.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

C# geliştiricisiyseniz, portal yerine [Visual Studio 2019'da ilk işlevinizi oluşturmayı](functions-create-your-first-function-visual-studio.md) düşünün. 

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

Azure hesabınızla [Azure portalında](https://portal.azure.com) oturum açın.

## <a name="create-a-function-app"></a>İşlev uygulaması oluşturma

İşlevlerinizin yürütülmesini barındıran bir işlev uygulamasına sahip olmanız gerekir. İşlev uygulaması, işlevleri daha kolay yönetim, dağıtım, ölçekleme ve kaynakların paylaşımı için mantıksal bir birim olarak gruplandırmanıza olanak tanır.

[!INCLUDE [Create function app Azure portal](../../includes/functions-create-function-app-portal.md)]

Ardından, yeni işlev uygulamasında bir işlev oluşturun.

## <a name="create-an-http-triggered-function"></a><a name="create-function"></a>HTTP ile tetiklenen bir işlev oluşturma

1. **Fonksiyonlar** penceresinin sol menüsünden **Fonksiyonlar'ı**seçin ve üst menüden **Ekle'yi** seçin. 
 
1. Yeni **İşlev** penceresinden **Http tetikleyicisini**seçin.

    ![HTTP tetikleme işlevini seçin](./media/functions-create-first-azure-function/function-app-select-http-trigger.png)

1. Yeni **İşlev** penceresinde, **Yeni İşlev**için varsayılan adı kabul edin veya yeni bir ad girin. 

1. **Yetkilendirme düzeyi** açılır listesinden **Anonim'i** seçin ve ardından **İşlev Oluştur'u**seçin.

    Azure, HTTP tetikleyici işlevini oluşturur. Artık bir HTTP isteği göndererek yeni işlevi çalıştırabilirsiniz.

## <a name="test-the-function"></a>İşlevi test etme

1. Yeni HTTP tetikleyici işlevinizde, sol menüden **Kod + Test'i** seçin ve ardından üst menüden **işlev URL'sini al'ı** seçin.

    ![İşlev URL'si Al'ı seçin](./media/functions-create-first-azure-function/function-app-select-get-function-url.png)

1. **İşlev** URL'si Al iletişim kutusunda, açılan listeden **varsayılan ı** seçin ve ardından **pano simgesine** Kopyala'yı seçin. 

    ![Azure portalından işlev URL’sini kopyalama](./media/functions-create-first-azure-function/function-app-develop-tab-testing.png)

1. İşlev URL'sini tarayıcınızın adres çubuğuna yapıştırın. Bu URL'nin `?name=<your_name>` sonuna sorgu dizesi değerini ekleyin ve isteği çalıştırmak için Enter tuşuna basın. 

    Aşağıdaki örnekte tarayıcıdaki yanıt gösterilmektedir:

    ![Tarayıcıdaki işlev yanıtı.](./media/functions-create-first-azure-function/function-app-browser-testing.png)

    İstek URL’si, işlevinize HTTP üzerinden erişmek için varsayılan olarak gerekli olan bir anahtar içerir.

1. İşleviniz çalıştığında, izleme bilgileri günlüklere yazılır. İzleme çıktısını görmek için portaldaki **Kod + Test** sayfasına dönün ve sayfanın altındaki **Günlükler** okunu genişletin.

   ![Azure portalında İşlevler günlük görüntüleyicisi.](./media/functions-create-first-azure-function/function-view-logs.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [Clean-up resources](../../includes/functions-quickstart-cleanup.md)]

## <a name="next-steps"></a>Sonraki adımlar

[!INCLUDE [Next steps note](../../includes/functions-quickstart-next-steps.md)]

