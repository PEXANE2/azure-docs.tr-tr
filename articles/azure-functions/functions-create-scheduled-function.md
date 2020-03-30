---
title: Azure'da zamanlamada çalışan bir işlev oluşturma
description: Azure’da nasıl tanımladığınız bir zamanlamaya göre çalışan bir işlev oluşturabileceğinizi öğrenin.
ms.assetid: ba50ee47-58e0-4972-b67b-828f2dc48701
ms.topic: quickstart
ms.date: 03/28/2018
ms.custom: mvc, cc996988-fb4f-47
ms.openlocfilehash: 808f0f81f937da688a8873e5f6ee959976e9d6aa
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "75769294"
---
# <a name="create-a-function-in-azure-that-is-triggered-by-a-timer"></a>Azure’da bir zamanlayıcı tarafından tetiklenen bir işlev oluşturma

Tanımladığınız bir zamanlamaya dayalı olarak çalışan [sunucusuz](https://azure.microsoft.com/solutions/serverless/) bir işlev oluşturmak için Azure İşlevlerini nasıl kullanacağınızı öğrenin.

![Azure portalında işlev uygulaması oluşturma](./media/functions-create-scheduled-function/function-app-in-portal-editor.png)

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlamak için:

+ Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.

## <a name="create-an-azure-function-app"></a>Azure İşlev uygulaması oluşturma

[!INCLUDE [Create function app Azure portal](../../includes/functions-create-function-app-portal.md)]

![İşlev uygulaması başarıyla oluşturuldu.](./media/functions-create-first-azure-function/function-app-create-success.png)

Ardından, yeni işlev uygulamasında bir işlev oluşturun.

<a name="create-function"></a>

## <a name="create-a-timer-triggered-function"></a>Zamanlayıcı ile tetiklenen işlev oluşturma

1. İşlev uygulamanızı genişletin ve **+** **Fonksiyonlar'ın**yanındaki düğmeyi tıklatın. Bu, işlev uygulamanızdaki ilk işlevse **Portalda**'yı ve ardından **Devam**'ı seçin. Aksi takdirde, adım 3 gidin.

   ![Azure portalındaki İşlevler hızlı başlangıç sayfası](./media/functions-create-scheduled-function/function-app-quickstart-choose-portal.png)

2. **Diğer şablonlar**'ı ve ardından **Sonlandır ve şablonları görüntüle**'yi seçin.

    ![İşlevler hızlı başlangıcı diğer şablonlar](./media/functions-create-scheduled-function/add-first-function.png)

3. Arama alanında, yeni `timer` tetikleyiciyi görüntünün altındaki tabloda belirtildiği gibi ayarlarla yazın ve yapılandırın.

    ![Azure portalında zamanlayıcı tarafından tetiklenen bir işlev oluşturun.](./media/functions-create-scheduled-function/functions-create-timer-trigger-2.png)

    | Ayar | Önerilen değer | Açıklama |
    |---|---|---|
    | **Adı** | Varsayılan | Zamanlayıcı ile tetiklenen işlevinizin adını tanımlar. |
    | **Zamanlama** | 0 \*/1 \* \* \*\* | İşlevinizi her dakika çalışacak şekilde zamanlayan altı haneli bir [CRON ifadesi](functions-bindings-timer.md#ncrontab-expressions). |

4. **Oluştur'u**tıklatın. Seçtiğiniz dilde, her dakika, dakika çalışan bir işlev oluşturulur.

5. Günlüklere yazılan izleme bilgilerini görüntüleyerek yürütmeyi doğrulayın.

    ![Azure portalında İşlevler günlük görüntüleyicisi.](./media/functions-create-scheduled-function/functions-timer-trigger-view-logs2.png)

Artık, işlevin zamanlamasını dakikada bir yerine saatte bir çalışacak şekilde değiştirebilirsiniz.

## <a name="update-the-timer-schedule"></a>Zamanlayıcı zamanlamasını güncelleştirme

1. İşlevinizi genişletin ve **Tümleştir**’e tıklayın. Burada, işlevinizin giriş ve çıkış bağlamalarını tanımlamanın yanı sıra zamanlamayı da ayarlarsınız. 

2. `0 0 */1 * * *` şeklinde yeni bir saatlik **Zamanlama** değeri girin ve **Kaydet**’e tıklayın.  

![İşlevler Azure portalındaki zamanlayıcı zamanlamasını güncelleştirir.](./media/functions-create-scheduled-function/functions-timer-trigger-change-schedule.png)

Artık saatte bir, saatte bir çalışan bir işleviniz var.

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [Next steps note](../../includes/functions-quickstart-cleanup.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bir zamanlamaya göre çalışan bir işlev oluşturdunuz. Zamanlayıcı tetikleyicileri hakkında daha fazla bilgi için [Azure İşlevleriyle Zamanla kodu yürütmesini zamanlayın'](functions-bindings-timer.md)a bakın.

[!INCLUDE [Next steps note](../../includes/functions-quickstart-next-steps.md)]
