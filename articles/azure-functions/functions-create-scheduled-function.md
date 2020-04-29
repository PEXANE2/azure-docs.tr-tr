---
title: Azure 'da bir zamanlamaya göre çalışan bir işlev oluşturma
description: Azure’da nasıl tanımladığınız bir zamanlamaya göre çalışan bir işlev oluşturabileceğinizi öğrenin.
ms.assetid: ba50ee47-58e0-4972-b67b-828f2dc48701
ms.topic: how-to
ms.date: 03/28/2018
ms.custom: mvc, cc996988-fb4f-47
ms.openlocfilehash: e100a2d3a3718b302a44cbdecf462a99d9c823e0
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80756502"
---
# <a name="create-a-function-in-azure-that-is-triggered-by-a-timer"></a>Azure’da bir zamanlayıcı tarafından tetiklenen bir işlev oluşturma

Azure Işlevleri 'ni kullanarak, tanımladığınız bir zamanlamaya göre çalışan [sunucusuz](https://azure.microsoft.com/solutions/serverless/) bir işlev oluşturma hakkında bilgi edinin.

![Azure portalında işlev uygulaması oluşturma](./media/functions-create-scheduled-function/function-app-in-portal-editor.png)

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlamak için:

+ Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="create-an-azure-function-app"></a>Azure İşlev uygulaması oluşturma

[!INCLUDE [Create function app Azure portal](../../includes/functions-create-function-app-portal.md)]

![İşlev uygulaması başarıyla oluşturuldu.](./media/functions-create-first-azure-function/function-app-create-success.png)

Ardından, yeni işlev uygulamasında bir işlev oluşturun.

<a name="create-function"></a>

## <a name="create-a-timer-triggered-function"></a>Zamanlayıcı ile tetiklenen işlev oluşturma

1. İşlev uygulamanızı genişletin ve **işlevler**' in **+** yanındaki düğmesine tıklayın. Bu, işlev uygulamanızdaki ilk işlevse **Portalda**'yı ve ardından **Devam**'ı seçin. Aksi takdirde 3. adıma gidin.

   ![Azure portalındaki İşlevler hızlı başlangıç sayfası](./media/functions-create-scheduled-function/function-app-quickstart-choose-portal.png)

2. **Diğer şablonlar**'ı ve ardından **Sonlandır ve şablonları görüntüle**'yi seçin.

    ![İşlevler hızlı başlangıcı diğer şablonlar](./media/functions-create-scheduled-function/add-first-function.png)

3. Arama alanına, yeni tetikleyiciyi görüntünün `timer` altındaki tabloda belirtilen ayarlarla yazın ve yapılandırın.

    ![Azure portalında zamanlayıcı tarafından tetiklenen bir işlev oluşturun.](./media/functions-create-scheduled-function/functions-create-timer-trigger-2.png)

    | Ayar | Önerilen değer | Açıklama |
    |---|---|---|
    | **Adı** | Varsayılan | Zamanlayıcı ile tetiklenen işlevinizin adını tanımlar. |
    | **Zamanlama** | 0 \*/1 \* \* \*\* | İşlevinizi her dakika çalışacak şekilde zamanlayan altı haneli bir [CRON ifadesi](functions-bindings-timer.md#ncrontab-expressions). |

4. **Oluştur**' a tıklayın. Seçtiğiniz dilde, dakikada her dakikada bir çalışan bir işlev oluşturulur.

5. Günlüklere yazılan izleme bilgilerini görüntüleyerek yürütmeyi doğrulayın.

    ![Azure portalında İşlevler günlük görüntüleyicisi.](./media/functions-create-scheduled-function/functions-timer-trigger-view-logs2.png)

Artık, işlevin zamanlamasını dakikada bir yerine saatte bir çalışacak şekilde değiştirebilirsiniz.

## <a name="update-the-timer-schedule"></a>Zamanlayıcı zamanlamasını güncelleştirme

1. İşlevinizi genişletin ve **Tümleştir**’e tıklayın. Burada, işlevinizin giriş ve çıkış bağlamalarını tanımlamanın yanı sıra zamanlamayı da ayarlarsınız. 

2. `0 0 */1 * * *` şeklinde yeni bir saatlik **Zamanlama** değeri girin ve **Kaydet**’e tıklayın.  

![İşlevler Azure portalındaki zamanlayıcı zamanlamasını güncelleştirir.](./media/functions-create-scheduled-function/functions-timer-trigger-change-schedule.png)

Artık saatte bir kez çalışan işleviniz var.

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [Next steps note](../../includes/functions-quickstart-cleanup.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bir zamanlamaya göre çalışan bir işlev oluşturdunuz. Zamanlayıcı Tetikleyicileri hakkında daha fazla bilgi için bkz. [Azure işlevleri ile kod yürütmeyi zamanlama](functions-bindings-timer.md).

[!INCLUDE [Next steps note](../../includes/functions-quickstart-next-steps.md)]
