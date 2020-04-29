---
title: Azure 'da kuyruk iletileri tarafından tetiklenen bir işlev oluşturma
description: Bir Azure Depolama kuyruğuna gönderilmiş iletiler tarafından çağrılan sunucusuz işlev oluşturmak için Azure İşlevlerini kullanın.
ms.assetid: 361da2a4-15d1-4903-bdc4-cc4b27fc3ff4
ms.topic: how-to
ms.date: 10/01/2018
ms.custom: mvc, cc996988-fb4f-47
ms.openlocfilehash: 98f0290aad9971bdb0c0b265d96e96d8ac34b99d
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80756511"
---
# <a name="create-a-function-triggered-by-azure-queue-storage"></a>Azure Kuyruk Depolama tarafından tetiklenen bir işlev oluşturma

Bir Azure Depolama kuyruğuna ileti gönderildiğinde tetiklenen bir işlev oluşturmayı öğrenin.

![Günlüklerde iletiyi görüntüleyin.](./media/functions-create-storage-queue-triggered-function/function-app-in-portal-editor.png)

## <a name="prerequisites"></a>Ön koşullar

- [Microsoft Azure Depolama Gezgini](https://storageexplorer.com/)'ni indirip yükleme.

- Azure aboneliği. Aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="create-an-azure-function-app"></a>Azure İşlev uygulaması oluşturma

[!INCLUDE [Create function app Azure portal](../../includes/functions-create-function-app-portal.md)]

![İşlev uygulaması başarıyla oluşturuldu.](./media/functions-create-first-azure-function/function-app-create-success.png)

Ardından, yeni işlev uygulamasında bir işlev oluşturun.

<a name="create-function"></a>

## <a name="create-a-queue-triggered-function"></a>Kuyruk ile tetiklenen bir işlev oluşturma

1. İşlev uygulamanızı genişletin ve **işlevler**' in **+** yanındaki düğmesine tıklayın. Bu, işlev uygulamanızdaki ilk işlevse **Portalda**'yı ve ardından **Devam**'ı seçin. Aksi takdirde üçüncü adıma geçin.

   ![Azure portalındaki İşlevler hızlı başlangıç sayfası](./media/functions-create-storage-queue-triggered-function/function-app-quickstart-choose-portal.png)

1. **Diğer şablonlar**'ı ve ardından **Sonlandır ve şablonları görüntüle**'yi seçin.

    ![İşlevler hızlı başlangıcı diğer şablonlar](./media/functions-create-storage-queue-triggered-function/add-first-function.png)

1. Arama alanına `queue` yazıp **Kuyruk tetikleyici** şablonunu seçin.

1. İstenirse, Azure Storage uzantısını ve işlev uygulamasındaki tüm bağımlılıkları yüklemek için **Install** ' ı seçin. Yükleme başarılı olduktan sonra **Devam**'ı seçin.

    ![Bağlama uzantılarını yükleme](./media/functions-create-storage-queue-triggered-function/functions-create-queue-storage-trigger-portal.png)

1. Görüntünün altındaki tabloda belirtilen ayarları kullanın.

    ![Depolama kuyruğu ile tetiklenen işlevi yapılandırın.](./media/functions-create-storage-queue-triggered-function/functions-create-queue-storage-trigger-portal-2.png)

    | Ayar | Önerilen değer | Açıklama |
    |---|---|---|
    | **Adı** | İşlev uygulamanızda benzersiz olmalıdır | Kuyruk tarafından tetiklenen bu işlevin adı. |
    | **Kuyruk adı**   | myqueue-items    | Depolama hesabınızdaki bağlantı kurulacak kuyruğun adı. |
    | **Depolama hesabı bağlantısı** | AzureWebJobsStorage | İşlev uygulamanız tarafından kullanılmakta olan depolama hesabı bağlantısını kullanabilir veya yeni bir bağlantı oluşturabilirsiniz.  |    

1. İşlevinizi oluşturmak için **Oluştur**'a tıklayın.

Ardından Azure Depolama hesabınıza bağlanıp **myqueue-items** depolama kuyruğunu oluşturun.

## <a name="create-the-queue"></a>Kuyruk oluşturma

1. İşlevinizde **Tümleştir**'e tıklayın, **Belgeler**'i genişletin ve hem **Hesap adı** hem de **Hesap anahtarı** değerlerini kopyalayın. Azure Depolama Gezgini’nde depolama hesabına bağlanmak için bu kimlik bilgilerini kullanacaksınız. Depolama hesabınıza önceden bağlandıysanız 4. adıma geçin.

    ![Depolama hesabı bağlantısı için kimlik bilgilerini alın.](./media/functions-create-storage-queue-triggered-function/functions-storage-account-connection.png)

1. [Microsoft Azure Depolama Gezgini](https://storageexplorer.com/) aracını çalıştırın, sol taraftaki bağlan simgesine tıklayın, **Depolama hesabı adı ve anahtarı kullan**'ı seçin ve **İleri**'ye tıklayın.

    ![Depolama Hesabı Gezgini aracını çalıştırın.](./media/functions-create-storage-queue-triggered-function/functions-storage-manager-connect-1.png)

1. 1 Adımda kopyaladığınız **Hesap adı** ve **Hesap anahtarı** değerlerini girin, **İleri**'ye ve ardından **Bağlan**'a tıklayın.

    ![Depolama kimlik bilgilerini girin ve bağlanın.](./media/functions-create-storage-queue-triggered-function/functions-storage-manager-connect-2.png)

1. Bağlı depolama hesabını genişletin, **Kuyruklar**' a sağ tıklayın, **kuyruk oluştur**' a tıklayın `myqueue-items`, yazın ve ardından ENTER tuşuna basın.

    ![Depolama kuyruğu oluşturun.](./media/functions-create-storage-queue-triggered-function/functions-storage-manager-create-queue.png)

Artık bir depolama kuyruğunuz var ve kuyruğa ileti ekleyerek işlevi test edebilirsiniz.

## <a name="test-the-function"></a>İşlevi test etme

1. Azure portal geri dönüp işlevinize gidin, sayfanın altındaki **günlükleri** genişletin ve günlük akışının duraklatılmadığından emin olun.

1. Depolama Gezgini, depolama hesabınızı, **kuyrukları**ve **myQueue-Items**' ı genişletin ve ardından **ileti Ekle**' ye tıklayın.

    ![Kuyruğa bir ileti ekleyin.](./media/functions-create-storage-queue-triggered-function/functions-storage-manager-add-message.png)

1. "Hello World!" iletinizi **İleti metni** alanına yazın ve **Tamam**'a tıklayın.

1. Birkaç saniye bekledikten sonra işlev günlüklerinize dönün ve yeni iletinin kuyruktan okunmuş olduğunu doğrulayın.

    ![Günlüklerde iletiyi görüntüleyin.](./media/functions-create-storage-queue-triggered-function/functions-queue-storage-trigger-view-logs.png)

1. Depolama Gezgini'ne dönüp **Yenile**'ye tıklayın ve iletinin işlenip kuyruktan kaldırılmış olduğunu doğrulayın.

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [Next steps note](../../includes/functions-quickstart-cleanup.md)]

## <a name="next-steps"></a>Sonraki adımlar

Depolama kuyruğuna bir ileti eklendiğinde çalışacak bir işlev oluşturdunuz. Kuyruk depolama tetikleyicileri hakkında daha fazla bilgi için bkz. [Azure İşlevleri Kuyruk depolama bağlamaları](functions-bindings-storage-queue.md).

İlk işlevinizi oluşturdığınıza göre, işleve başka bir kuyruğa ileti yazan bir çıkış bağlaması ekleyelim.

> [!div class="nextstepaction"]
> [İşlevleri kullanarak bir Azure Depolama kuyruğuna ileti ekleme](functions-integrate-storage-queue-output-binding.md)
