---
title: Azure portalında ilk işlevinizi oluşturma
description: Azure portalını kullanarak sunucusuz yürütme için ilk Azure İşlevinizi oluşturma hakkında bilgi edinin.
ms.assetid: 96cf87b9-8db6-41a8-863a-abb828e3d06d
ms.topic: how-to
ms.date: 03/26/2020
ms.custom: mvc, devcenter, cc996988-fb4f-47
ms.openlocfilehash: a96d2ede80b4c57e7b85048379a4bfb66cacfd52
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80754851"
---
# <a name="create-your-first-function-in-the-azure-portal"></a>Azure portalında ilk işlevinizi oluşturma

Azure Işlevleri, öncelikle bir sanal makine (VM) oluşturmak veya bir Web uygulaması yayımlamak zorunda kalmadan kodunuzu sunucusuz bir ortamda çalıştırmanızı sağlar. Bu makalede, Azure Işlevleri 'ni kullanarak Azure portal bir "Hello World" HTTP ile tetiklenen bir işlev oluşturma hakkında bilgi edineceksiniz.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

C# geliştiricisiyseniz Portal yerine [Visual Studio 2019 ' de ilk işlevinizi oluşturmayı](functions-create-your-first-function-visual-studio.md) düşünün. 

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

Azure hesabınızla [Azure portalında](https://portal.azure.com) oturum açın.

## <a name="create-a-function-app"></a>İşlev uygulaması oluşturma

İşlevlerinizin yürütülmesini barındıran bir işlev uygulamasına sahip olmanız gerekir. İşlev uygulaması, kaynakların daha kolay yönetilmesi, dağıtılması, ölçeklendirilmesi ve paylaşılması için işlevleri bir mantıksal birim olarak gruplandırmanıza olanak tanır.

[!INCLUDE [Create function app Azure portal](../../includes/functions-create-function-app-portal.md)]

Sonra, yeni işlev uygulamasında bir işlev oluşturun.

## <a name="create-an-http-triggered-function"></a><a name="create-function"></a>HTTP ile tetiklenen bir işlev oluşturma

1. Yeni işlev uygulamanızı genişletin, **işlevler**' ın **+** yanındaki düğmeyi seçin, **Portal**' ı seçin ve ardından **devam**' ı seçin.

    ![Platform seçme için hızlı başlangıç işlevleri.](./media/functions-create-first-azure-function/function-app-quickstart-choose-portal.png)

1. **Web kancası + API**' yi seçin ve ardından **Oluştur**' u seçin.

    ![Azure portalındaki İşlevler hızlı başlangıcı.](./media/functions-create-first-azure-function/function-app-quickstart-node-webhook.png)

   Dile özgü HTTP ile tetiklenen işlev şablonu kullanılarak bir işlev oluşturulur.

Artık bir HTTP isteği göndererek yeni işlevi çalıştırabilirsiniz.

## <a name="test-the-function"></a>İşlevi test etme

1. Yeni işlevinizde, sağ üst köşedeki **</> işlev URL 'Sini al** ' ı seçin. 

1. **İşlev URL 'Sini al** iletişim kutusunda, açılan listeden **varsayılan (işlev anahtarı)** seçeneğini belirleyin ve ardından **Kopyala**' yı seçin. 

    ![Azure portalından işlev URL’sini kopyalama](./media/functions-create-first-azure-function/function-app-develop-tab-testing.png)

1. İşlev URL'sini tarayıcınızın adres çubuğuna yapıştırın. Sorgu dizesi değerini `&name=<your_name>` bu URL 'nin sonuna ekleyin ve isteği çalıştırmak için ENTER 'a basın. 

    Aşağıdaki örnekte tarayıcıdaki yanıt gösterilmektedir:

    ![Tarayıcıdaki işlev yanıtı.](./media/functions-create-first-azure-function/function-app-browser-testing.png)

    İstek URL’si, işlevinize HTTP üzerinden erişmek için varsayılan olarak gerekli olan bir anahtar içerir.

1. İşleviniz çalıştığında, izleme bilgileri günlüklere yazılır. Önceki yürütmeden alınan izleme çıktısını görmek için, portalda işlevinizin simgesine dönün ve ekranın altındaki oku seçerek **günlükleri**genişletin.

   ![Azure portalında İşlevler günlük görüntüleyicisi.](./media/functions-create-first-azure-function/function-view-logs.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [Clean-up resources](../../includes/functions-quickstart-cleanup.md)]

## <a name="next-steps"></a>Sonraki adımlar

[!INCLUDE [Next steps note](../../includes/functions-quickstart-next-steps.md)]

