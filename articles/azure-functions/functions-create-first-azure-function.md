---
title: Azure portalında ilk işlevinizi oluşturma
description: Azure portalını kullanarak sunucusuz yürütme için ilk Azure İşlevinizi oluşturma hakkında bilgi edinin.
ms.assetid: 96cf87b9-8db6-41a8-863a-abb828e3d06d
ms.topic: quickstart
ms.date: 03/06/2020
ms.custom: mvc, devcenter, cc996988-fb4f-47
ms.openlocfilehash: e7bb5e7b387c3ab1140a3fe475911bd0e428e2a5
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80057204"
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

1. Yeni işlev uygulamanızı **+** genişletin, **Fonksiyonlar'ın**yanındaki düğmeyi seçin, **Portal Içi'ni**seçin ve sonra **Devam et'i**seçin.

    ![Bir platform seçmek için işlevler hızlı başlatın.](./media/functions-create-first-azure-function/function-app-quickstart-choose-portal.png)

1. **WebHook + API'yi**seçin ve ardından **Oluştur'u**seçin.

    ![Azure portalındaki İşlevler hızlı başlangıcı.](./media/functions-create-first-azure-function/function-app-quickstart-node-webhook.png)

   Dile özgü HTTP ile tetiklenen işlev şablonu kullanılarak bir işlev oluşturulur.

Artık bir HTTP isteği göndererek yeni işlevi çalıştırabilirsiniz.

## <a name="test-the-function"></a>İşlevi test etme

1. Yeni işlevinizde sağ üstteki **</> İşlev URL'sini** alın'ı seçin. 

1. **İşlev** URL'si Al iletişim kutusunda, açılan listeden **varsayılan (İşlev tuşu)** seçeneğini belirleyin ve ardından **Kopyala'yı**seçin. 

    ![Azure portalından işlev URL’sini kopyalama](./media/functions-create-first-azure-function/function-app-develop-tab-testing.png)

1. İşlev URL'sini tarayıcınızın adres çubuğuna yapıştırın. Bu URL'nin `&name=<your_name>` sonuna sorgu dizesi değerini ekleyin ve isteği çalıştırmak için Enter tuşuna basın. 

    Aşağıdaki örnekte tarayıcıdaki yanıt gösterilmektedir:

    ![Tarayıcıdaki işlev yanıtı.](./media/functions-create-first-azure-function/function-app-browser-testing.png)

    İstek URL’si, işlevinize HTTP üzerinden erişmek için varsayılan olarak gerekli olan bir anahtar içerir.

1. İşleviniz çalıştığında, izleme bilgileri günlüklere yazılır. Önceki yürütmeden gelen izleme çıktısını görmek için, portaldaki işlevinize dönün ve **Günlükleri**genişletmek için ekranın altındaki oku seçin.

   ![Azure portalında İşlevler günlük görüntüleyicisi.](./media/functions-create-first-azure-function/function-view-logs.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [Clean-up resources](../../includes/functions-quickstart-cleanup.md)]

## <a name="next-steps"></a>Sonraki adımlar

[!INCLUDE [Next steps note](../../includes/functions-quickstart-next-steps.md)]

