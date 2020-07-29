---
title: Azure 'da kuyruk iletileri tarafından tetiklenen bir işlev oluşturma
description: Azure Işlevleri 'ni kullanarak Azure 'da bir kuyruğa gönderilen iletiler tarafından çağrılan bir sunucusuz işlev oluşturun.
ms.assetid: 361da2a4-15d1-4903-bdc4-cc4b27fc3ff4
ms.topic: how-to
ms.date: 10/01/2018
ms.custom: mvc, cc996988-fb4f-47
ms.openlocfilehash: c4c20579f2306b61741f3c6ab1549285271435a3
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "83123352"
---
# <a name="create-a-function-triggered-by-azure-queue-storage"></a>Azure Kuyruk Depolama tarafından tetiklenen bir işlev oluşturma

Bir Azure Depolama kuyruğuna ileti gönderildiğinde tetiklenen bir işlev oluşturmayı öğrenin.

## <a name="prerequisites"></a>Ön koşullar

- Azure aboneliği. Aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="create-an-azure-function-app"></a>Azure İşlev uygulaması oluşturma

[!INCLUDE [Create function app Azure portal](../../includes/functions-create-function-app-portal.md)]

   :::image type="content" source="./media/functions-create-storage-queue-triggered-function/function-app-create-success.png" alt-text="İşlev uygulaması başarıyla oluşturuldu.." border="true":::

Ardından, yeni işlev uygulamasında bir işlev oluşturun.

<a name="create-function"></a>

## <a name="create-a-queue-triggered-function"></a>Kuyruk ile tetiklenen bir işlev oluşturma

1. **İşlevler**' i seçin ve sonra yeni bir işlev eklemek Için **+ Ekle** ' yi seçin.

   :::image type="content" source="./media/functions-create-storage-queue-triggered-function/functions-app-quickstart-choose-template.png" alt-text="Azure portal bir Işlev şablonu seçin." border="true":::

1. **Azure kuyruk depolama tetikleme** şablonunu seçin.

1. Görüntünün altındaki tabloda belirtilen ayarları kullanın.

    :::image type="content" source="./media/functions-create-storage-queue-triggered-function/functions-create-queue-storage-trigger-portal.png" alt-text="Kuyruk depolama ile tetiklenen işlevi adlandırın ve yapılandırın." border="true":::


    | Ayar | Önerilen değer | Açıklama |
    |---|---|---|
    | **Adı** | İşlev uygulamanızda benzersiz olmalıdır | Kuyruk tarafından tetiklenen bu işlevin adı. |
    | **Kuyruk adı**   | myqueue-items    | Depolama hesabınızdaki bağlantı kurulacak kuyruğun adı. |
    | **Depolama hesabı bağlantısı** | AzureWebJobsStorage | İşlev uygulamanız tarafından kullanılmakta olan depolama hesabı bağlantısını kullanabilir veya yeni bir bağlantı oluşturabilirsiniz.  |    

1. İşlevinizi oluşturmak için **Işlev oluştur** ' u seçin.

    :::image type="content" source="./media/functions-create-storage-queue-triggered-function/functions-create-queue-storage-trigger-portal-3.png" alt-text="Kuyruk depolama tarafından tetiklenen işlevi oluşturun." border="true":::

Sonra, Azure depolama hesabınıza bağlanıp **myQueue-Items** depolama kuyruğunu oluşturursunuz.

## <a name="create-the-queue"></a>Kuyruk oluşturma

1. İşlevinizde **genel bakış** sayfasında, kaynak grubunuzu seçin.

    :::image type="content" source="./media/functions-create-storage-queue-triggered-function/functions-storage-resource-group.png" alt-text="Azure portal kaynak grubunuzu seçin." border="true":::

1. Kaynak grubunuzun depolama hesabını bulun ve seçin.

    :::image type="content" source="./media/functions-create-storage-queue-triggered-function/functions-storage-account-access.png" alt-text="Depolama hesabına erişin." border="true":::

1. **Kuyruklar**' ı seçin ve ardından **+ kuyruk**' u seçin. 

    :::image type="content" source="./media/functions-create-storage-queue-triggered-function/functions-storage-add-queue.png" alt-text="Azure portal depolama hesabınıza bir kuyruk ekleyin." border="true":::

1. **Ad** alanına yazın `myqueue-items` ve ardından **Oluştur**' u seçin.

    :::image type="content" source="./media/functions-create-storage-queue-triggered-function/functions-storage-name-queue.png" alt-text="Kuyruk depolama kapsayıcısını adlandırın." border="true":::

Artık bir depolama kuyruğunuz var ve kuyruğa ileti ekleyerek işlevi test edebilirsiniz.

## <a name="test-the-function"></a>İşlevi test etme

1. Azure portalına dönün, işlevinizi bulun, sayfanın en altındaki **Günlükler** bölümünü genişletin ve günlük akışının duraklatılmış olmadığından emin olun.

    :::image type="content" source="./media/functions-create-storage-queue-triggered-function/functions-queue-storage-log-expander.png" alt-text="Azure portal günlüğü genişletin." border="true":::

1. Ayrı bir tarayıcı penceresinde, Azure portal kaynak grubunuza gidin ve depolama hesabını seçin.

1. **Kuyruklar**' ı seçin ve sonra **myQueue-Items** kapsayıcısını seçin.

    :::image type="content" source="./media/functions-create-storage-queue-triggered-function/functions-storage-queue.png" alt-text="Azure portal myQueue-Items kuyruğuna gidin." border="true":::

1. **Ileti Ekle**' yi seçin ve "Merhaba Dünya!" yazın **ileti metni**. **Tamam**’ı seçin.

    :::image type="content" source="./media/functions-create-storage-queue-triggered-function/functions-storage-queue-test.png" alt-text="Azure portal myQueue-Items kuyruğuna gidin." border="true":::

1. Birkaç saniye bekledikten sonra işlev günlüklerinize dönün ve yeni iletinin kuyruktan okunmuş olduğunu doğrulayın.

    :::image type="content" source="./media/functions-create-storage-queue-triggered-function/function-app-in-portal-editor.png" alt-text="Günlüklerde iletiyi görüntüleyin." border="true":::

1. Depolama kuyruğunuza geri dönün, **Yenile** ' yi seçin ve iletinin işlendiğini ve artık kuyrukta olmadığını doğrulayın.

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [Next steps note](../../includes/functions-quickstart-cleanup.md)]

## <a name="next-steps"></a>Sonraki adımlar

Depolama kuyruğuna bir ileti eklendiğinde çalışacak bir işlev oluşturdunuz. Kuyruk depolama tetikleyicileri hakkında daha fazla bilgi için bkz. [Azure İşlevleri Kuyruk depolama bağlamaları](functions-bindings-storage-queue.md).

İlk işlevinizi oluşturdığınıza göre, işleve başka bir kuyruğa ileti yazan bir çıkış bağlaması ekleyelim.

> [!div class="nextstepaction"]
> [İşlevleri kullanarak bir Azure Depolama kuyruğuna ileti ekleme](functions-integrate-storage-queue-output-binding.md)
