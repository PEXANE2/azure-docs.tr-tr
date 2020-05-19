---
title: Azure 'da BLOB depolama tarafından tetiklenen bir işlev oluşturma
description: BLOB depolama kapsayıcısına eklenen öğeler tarafından çağrılan sunucusuz bir işlev oluşturmak için Azure Işlevleri 'ni kullanın.
ms.assetid: d6bff41c-a624-40c1-bbc7-80590df29ded
ms.topic: how-to
ms.date: 10/01/2018
ms.custom: mvc, cc996988-fb4f-47
ms.openlocfilehash: bf6865d2756579f457dded90b247326d2eec137c
ms.sourcegitcommit: a8ee9717531050115916dfe427f84bd531a92341
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/12/2020
ms.locfileid: "83123131"
---
# <a name="create-a-function-in-azure-thats-triggered-by-blob-storage"></a>Azure 'da BLOB depolama tarafından tetiklenen bir işlev oluşturma

Bir BLOB depolama kapsayıcısına dosyalar yüklenirken veya güncelleştirilirken tetiklenen bir işlev oluşturmayı öğrenin.

## <a name="prerequisites"></a>Önkoşullar

+ Azure aboneliği. Aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="create-an-azure-function-app"></a>Azure İşlev uygulaması oluşturma

[!INCLUDE [Create function app Azure portal](../../includes/functions-create-function-app-portal.md)]

Yeni işlev uygulamanızı başarıyla oluşturdunuz.

:::image type="content" source="./media/functions-create-storage-blob-triggered-function/function-app-create-success.png" alt-text="İşlev uygulaması başarıyla oluşturuldu." border="true":::

Ardından, yeni işlev uygulamasında bir işlev oluşturun.

<a name="create-function"></a>

## <a name="create-an-azure-blob-storage-triggered-function"></a>Azure Blob depolama ile tetiklenen bir işlev oluşturma

1. **İşlevler**' i seçin ve sonra yeni bir işlev eklemek Için **+ Ekle** ' yi seçin.

   :::image type="content" source="./media/functions-create-storage-blob-triggered-function/function-app-quickstart-choose-template.png" alt-text="Azure portal bir Işlev şablonu seçin." border="true":::

1. **Azure Blob depolama tetikleyicisi** şablonunu seçin.

1. Görüntünün altındaki tabloda belirtilen ayarları kullanın.

    :::image type="content" source="./media/functions-create-storage-blob-triggered-function/functions-create-blob-storage-trigger-portal-2.png" alt-text="BLOB depolama ile tetiklenen işlevi adlandırın ve yapılandırın." border="true":::

    | Ayar | Önerilen değer | Açıklama |
    |---|---|---|
    | **Yeni Işlev** | İşlev uygulamanızda benzersiz olmalıdır | Blob ile tetiklenen bu işlevin adı. |
    | **Yol**   | samples-workitems/{ad}    | İzlenmekte olan Blob depolamanın konumu. Blob’un dosya adı bağlamaya _name_ parametresi olarak geçirilir.  |
    | **Depolama hesabı bağlantısı** | AzureWebJobsStorage | İşlev uygulamanız tarafından kullanılmakta olan depolama hesabı bağlantısını kullanabilir veya yeni bir bağlantı oluşturabilirsiniz.  |

1. İşlevinizi oluşturmak için **Işlev oluştur** ' u seçin.

    :::image type="content" source="./media/functions-create-storage-blob-triggered-function/functions-create-blob-storage-trigger-portal-3.png" alt-text="Blob depolama ile tetiklenen bir işlev oluşturun." border="true":::

Ardından, **Samples-WorkItems** kapsayıcısını oluşturun.

## <a name="create-the-container"></a>Kapsayıcı oluşturma

1. İşlevinizde **genel bakış** sayfasında, kaynak grubunuzu seçin.

    :::image type="content" source="./media/functions-create-storage-blob-triggered-function/functions-storage-resource-group.png" alt-text="Azure portal kaynak grubunuzu seçin." border="true":::

1. Kaynak grubunuzun depolama hesabını bulun ve seçin.

    :::image type="content" source="./media/functions-create-storage-blob-triggered-function/functions-storage-account-access.png" alt-text="Depolama hesabına erişin." border="true":::

1. **Kapsayıcılar**' ı seçin ve ardından **+ kapsayıcı**' yı seçin. 

    :::image type="content" source="./media/functions-create-storage-blob-triggered-function/functions-storage-add-container.png" alt-text="Azure portal depolama hesabınıza kapsayıcı ekleyin." border="true":::

1. **Ad** alanına yazın `samples-workitems` ve ardından **Oluştur**' u seçin.

    :::image type="content" source="./media/functions-create-storage-blob-triggered-function/functions-storage-name-blob-container.png" alt-text="Depolama kapsayıcısını adlandırın." border="true":::

Artık bir blob kapsayıcısına sahip olduğunuza göre, kapsayıcıya bir dosya yükleyerek işlevi test edebilirsiniz.

## <a name="test-the-function"></a>İşlevi test etme

1. Azure portalına dönün, işlevinizi bulun, sayfanın en altındaki **Günlükler** bölümünü genişletin ve günlük akışının duraklatılmış olmadığından emin olun.

    :::image type="content" source="./media/functions-create-storage-blob-triggered-function/functions-storage-log-expander.png" alt-text="Azure portal günlüğü genişletin." border="true":::

1. Ayrı bir tarayıcı penceresinde, Azure portal kaynak grubunuza gidin ve depolama hesabını seçin.

1. **Kapsayıcılar**' ı seçin ve ardından **Samples-WorkItems** kapsayıcısını seçin.

    :::image type="content" source="./media/functions-create-storage-blob-triggered-function/functions-storage-container.png" alt-text="Azure portal, Samples-WorkItems kapsayıcısına gidin." border="true":::

1. **Karşıya yükle**' yi seçin ve ardından karşıya yüklenecek dosyayı seçmek için klasör simgesini seçin.

    :::image type="content" source="./media/functions-create-storage-blob-triggered-function/functions-storage-manager-upload-file-blob.png" alt-text="Dosyayı blob kapsayıcısına yükleyin." border="true":::

1. Yerel bilgisayarınızda bir görüntü dosyası gibi bir dosyaya gidin, dosyayı seçin. **Aç** ' ı seçin ve ardından **karşıya yükleyin**.

1. İşlev günlüklerinize geri dönün ve blob’un okunduğunu doğrulayın.

    :::image type="content" source="./media/functions-create-storage-blob-triggered-function/function-app-in-portal-editor.png" alt-text="Günlüklerde iletiyi görüntüleyin." border="true":::

    >[!NOTE]
    > İşlev uygulamanız varsayılan Tüketim planında çalıştığında, blob’un eklenmesi veya güncelleştirilmesi ile işlevin tetiklenmesi arasında birkaç dakika gecikme olabilir. Blob ile tetiklenen işlevlerde düşük gecikme süresi gerekiyorsa, işlev uygulamanızı bir App Service planında çalıştırmayı düşünün.

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [Next steps note](../../includes/functions-quickstart-cleanup.md)]

## <a name="next-steps"></a>Sonraki adımlar

Blob depolamaya bir blob eklendiğinde çalışacak bir işlev oluşturdunuz. Blob depolama tetikleyicileri hakkında daha fazla bilgi için bkz. [Azure İşlevleri Blob depolama bağlamaları](functions-bindings-storage-blob.md).

[!INCLUDE [Next steps note](../../includes/functions-quickstart-next-steps.md)]
