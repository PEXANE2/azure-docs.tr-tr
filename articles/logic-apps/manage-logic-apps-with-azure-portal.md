---
title: Azure portal Logic Apps 'i yönetme
description: Azure portal kullanarak mantıksal uygulamaları düzenleyin, etkinleştirin, devre dışı bırakın veya silin.
services: logic-apps
ms.suite: integration
author: lauradolan
ms.author: ladolan
ms.reviewer: estfan, jonfan, logicappspm
ms.topic: article
ms.custom: mvc
ms.date: 07/20/2020
ms.openlocfilehash: d50f577a7170982be004cc8957114f79675fbc6e
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87078619"
---
# <a name="manage-logic-apps-in-the-azure-portal"></a>Azure portal Logic Apps 'i yönetme

[Azure Portal](https://portal.azure.com) veya [Visual Studio](manage-logic-apps-with-visual-studio.md)kullanarak mantıksal uygulamaları yönetebilirsiniz. Bu makalede, Azure portal mantıksal uygulamaları düzenleme, devre dışı bırakma, etkinleştirme veya silme işlemlerinin nasıl yapılacağı gösterilir. Azure Logic Apps yeni başladıysanız, bkz. [Azure Logic Apps](logic-apps-overview.md)nedir?

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği. Azure aboneliğiniz yoksa [ücretsiz bir Azure hesabı için kaydolun](https://azure.microsoft.com/free/).

* Mevcut bir mantıksal uygulama. Azure portal mantıksal uygulama oluşturmayı öğrenmek için bkz. [hızlı başlangıç: Azure Logic Apps-Azure Portal kullanarak ilk iş akışınızı oluşturma](quickstart-create-first-logic-app-workflow.md).

<a name="find-logic-app"></a>

## <a name="find-your-logic-apps"></a>Mantıksal uygulamalarınızı bulun

Mantıksal uygulamanızı bulup açmak için şu adımları izleyin:

1. Azure hesabınızla [Azure portalında](https://portal.azure.com) oturum açın.

1. Azure Arama çubuğunda yazın `logic apps` ve **Logic Apps**seçin.

   ![Arama çubuğunda Logic Apps arandığı ve seçildiği Azure portal menüsünün ekran görüntüsü.](./media/manage-logic-apps-with-azure-portal/find-select-logic-apps.png)

1. **Logic Apps** sayfasında, yönetmek istediğiniz mantıksal uygulamayı bulun ve seçin.

   Mantıksal uygulamanın **genel bakış** bölmesi açıldıktan sonra, **Logic Apps** sayfasında görünen listeyi şu yollarla filtreleyebilirsiniz:

   * Mantıksal uygulamaları ada göre ara
   * Mantıksal uygulamaları aboneliğe, kaynak grubuna, konuma ve etiketlere göre filtrele
   * Kaynak grubu, tür, abonelik ve konuma göre mantıksal uygulamaları gruplandırın

## <a name="view-logic-app-properties"></a>Mantıksal uygulama özelliklerini görüntüle

1. Azure portal, [mantıksal uygulamanızı bulun ve açın](#find-logic-app).

1. Mantıksal uygulamanızın menüsünde, **Ayarlar**' ın altında **Özellikler**' i seçin.

1. **Özellikler** bölmesinde, mantıksal uygulamanız hakkında aşağıdaki bilgileri görüntüleyebilir ve kopyalayabilirsiniz:

   * **Ad**
   * **Kaynak kimliği**
   * **Kaynak grubu**
   * **Konum**
   * **Tür** 
   * **Abonelik Adı**
   * **Abonelik kimliği**
   * **Erişim uç noktası**
   * **Çalışma zamanı giden IP adresleri**
   * **Uç nokta IP adreslerine erişin**
   * **Bağlayıcının giden IP adresleri**

## <a name="disable-or-enable-logic-apps"></a>Mantıksal uygulamaları devre dışı bırakma veya etkinleştirme

[Tek bir mantıksal uygulamayı](#disable-enable-single-logic-app) veya [birden çok logic Apps 'i Azure Portal aynı anda](#disable-or-enable-multiple-logic-apps) etkinleştirebilir veya devre dışı bırakabilirsiniz. Ayrıca, [Visual Studio 'da Logic Apps 'i etkinleştirebilir veya devre dışı](manage-logic-apps-with-visual-studio.md#disable-or-enable-logic-app)bırakabilirsiniz.

Mantıksal uygulamanızı devre dışı bırakmak, iş akışı örneklerinizi etkiler ve şu yollarla çalışır:

* Tüm devam eden ve bekleyen çalıştırmalar tamamlanana kadar devam eder. Bu çalıştırmaların sayısına bağlı olarak, bu işlem biraz zaman alabilir.

* Logic Apps altyapısı yeni iş akışı örnekleri oluşturmaz veya çalıştırmaz.

* Tetikleyici, koşullarının bir sonraki karşılanışında harekete geçmeyecektir.

* Tetikleyici durumu, mantıksal uygulamanın durdurulduğu noktayı anımsar. Bu nedenle, mantıksal uygulamayı yeniden etkinleştirirseniz tetikleyici, son çalıştırmasından bu yana işlenmemiş tüm öğeler için ateşlenir.

  Mantıksal uygulamanızı, son çalıştırmadan bu yana işlenmemiş öğelerde tetiklemeyi durdurmak için, mantıksal uygulamayı yeniden etkinleştirmeden önce tetikleyicinin durumunu temizleyin:

  1. Azure portal, [mantıksal uygulamanızı bulun ve açın](#find-logic-app).

  1. Mantıksal uygulama tetikleyicisinin herhangi bir bölümünü düzenleyin.

  1. Yaptığınız değişiklikleri kaydedin. Bu adım, tetikleyicinizin geçerli durumunu sıfırlar.

  1. [Mantıksal uygulamanızı yeniden etkinleştirin](#disable-enable-single-logic-app).

<a name="disable-enable-single-logic-app"></a>

### <a name="disable-or-enable-single-logic-app"></a>Tek mantıksal uygulamayı devre dışı bırakma veya etkinleştirme

1. Azure portal, [mantıksal uygulamanızı bulun ve açın](#find-logic-app).

1. Mantıksal uygulamanızın menüsünde **genel bakış**' ı seçin. Aşağıdaki seçeneklerden birini belirleyin:

   * Araç çubuğunda **devre dışı bırak**' ı seçin.

     ![Mantıksal uygulamanın araç çubuğunun ekran görüntüsü, devre dışı bırak düğmesi seçili.](./media/manage-logic-apps-with-azure-portal/disable-single-logic-app.png)

     Mantıksal uygulamanız zaten devre dışıysa yalnızca **Etkinleştir** seçeneğini görürsünüz.

   * Araç çubuğunda **Etkinleştir**' i seçin.

     ![Mantıksal uygulamanın araç çubuğunun, etkinleştir düğmesinin seçili olduğunu gösteren ekran görüntüsü.](./media/manage-logic-apps-with-azure-portal/enable-single-logic-app.png)

     Mantıksal uygulamanız zaten etkinse, yalnızca **devre dışı bırak** seçeneğini görürsünüz. 

   Azure portal, işlemin başarılı veya başarısız olup olmadığını doğrulayan ana Azure araç çubuğunda bir bildirim gösterir.

   ![İşlem durumunu onaylamak için bildirimi gösteren Azure portal ekran görüntüsü](./media/manage-logic-apps-with-azure-portal/operation-confirmation-notification.png)

<a name="disable-or-enable-multiple-logic-apps"></a>

### <a name="disable-or-enable-multiple-logic-apps"></a>Birden çok mantıksal uygulamayı devre dışı bırakma veya etkinleştirme

1. Azure portal, devre dışı bırakmak veya etkinleştirmek istediğiniz [mantıksal uygulamaları bulun](#find-logic-app) .

1. Mantıksal uygulamanın Şu anda etkin veya devre dışı olduğunu denetlemek için, **Logic Apps** sayfasında, bu mantıksal uygulamanın **durum** sütununu gözden geçirin. 

   ![Durum sütununa göre sıralanmış Logic Apps listesini gösteren Azure portal Logic Apps sayfasının ekran görüntüsü.](./media/manage-logic-apps-with-azure-portal/view-logic-app-status.png)

   **Durum** sütunu görünür değilse, **Logic Apps** araç çubuğunda **önizlemeyi dene**' yi seçin.

   ![Azure portal Logic Apps sayfasının ekran görüntüsü, önizlemeyi dene düğmesinin seçili olduğunu gösterir.](./media/manage-logic-apps-with-azure-portal/select-try-preview.png)

1. Onay kutusu sütununda, devre dışı bırakmak veya etkinleştirmek istediğiniz Logic Apps ' i seçin. Araç çubuğunda **devre dışı bırak** veya **Etkinleştir**' i seçin.

   ![Birden çok Logic Apps için etkinleştir ve devre dışı bırak düğmelerini gösteren Azure portal Logic Apps sayfasının ekran görüntüsü.](./media/manage-logic-apps-with-azure-portal/enable-disable-multiple-logic-apps.png)

1. Onay kutusu göründüğünde, devam etmek için **Evet** ' i seçin.

   Azure portal, işlemin başarılı veya başarısız olup olmadığını doğrulayan ana Azure araç çubuğunda bir bildirim gösterir.

## <a name="delete-logic-apps"></a>Mantıksal uygulamaları silme

[Tek bir mantıksal uygulamayı silebilir](#delete-single-logic-app) veya Azure Portal [aynı anda birden çok Logic Apps](#delete-multiple-logic-apps) 'i silebilirsiniz. Ayrıca, [mantıksal uygulamanızı Visual Studio 'da da silebilirsiniz](manage-logic-apps-with-visual-studio.md#delete-your-logic-app).

Mantıksal uygulamanızı silmek, iş akışı örneklerinizi şu yollarla etkiler:

* Tüm devam eden ve bekleyen çalıştırmalar tamamlanana kadar devam eder. Bu çalıştırmaların sayısına bağlı olarak, bu işlem biraz zaman alabilir.

* Logic Apps altyapısı yeni iş akışı örnekleri oluşturmaz veya çalıştırmaz.

> [!NOTE]
> Bir alt mantıksal uygulamayı silip yeniden oluşturursanız, ana mantıksal uygulamayı yeniden kaydetmeniz gerekir. Yeniden oluşturulan alt uygulamanın farklı meta verileri olacak.
> Üst mantıksal uygulamayı alt öğesini yeniden oluşturduktan sonra yeniden kaydetmezseniz, alt mantıksal uygulamaya yapılan çağrılarınız "yetkilendirilmemiş" hatası ile başarısız olur. Bu davranış, örneğin tümleştirme hesaplarında yapıtlar kullanan veya Azure işlevleri çağıran ana alt mantıksal uygulamalar için geçerlidir.

<a name="delete-single-logic-app"></a>

### <a name="delete-single-logic-app"></a>Tek mantıksal uygulamayı Sil

1. Azure portal, [mantıksal uygulamanızı bulun ve açın](#find-logic-app).

1. Mantıksal uygulamanızın menüsünde **genel bakış**' ı seçin. Mantıksal uygulamanızın araç çubuğunda **Sil**' i seçin.

   ![Mantıksal uygulamanın araç çubuğunun ekran görüntüsü, Sil düğmesinin seçili olduğunu gösterir.](./media/manage-logic-apps-with-azure-portal/delete-single-logic-app.png)

1. Onay kutusu göründüğünde, mantıksal uygulamanızın adını girin ve **Sil**' i seçin.

   ![Tek Logic App 'in silinmesini onaylamak için Logic Apps isteminin ekran görüntüsü.](./media/manage-logic-apps-with-azure-portal/delete-confirmation-single-logic-app.png)

   Azure portal, işlemin başarılı veya başarısız olup olmadığını doğrulayan ana Azure araç çubuğunda bir bildirim gösterir.

<a name="delete-multiple-logic-apps"></a>

### <a name="delete-multiple-logic-apps"></a>Birden çok Logic Apps silme

1. Azure portal, [silmek istediğiniz mantıksal uygulamaları bulun](#find-logic-app).

1. Onay kutusu sütununda, silmek istediğiniz Logic Apps ' i seçin. Araç çubuğunda **Sil**' i seçin.

   ![Silinmek üzere seçilen bir listede birden çok mantıksal uygulamayı gösteren Logic Apps sayfasının ekran görüntüsü.](./media/manage-logic-apps-with-azure-portal/delete-multiple-logic-apps.png)

1. Onay kutusu göründüğünde, girin `yes` ve **Sil**' i seçin.

   ![Birden çok Logic Apps 'in silinmesini onaylamak için Logic Apps isteminin ekran görüntüsü.](./media/manage-logic-apps-with-azure-portal/delete-confirmation-multiple-logic-apps.png)

   Azure portal, işlemin başarılı veya başarısız olup olmadığını doğrulayan ana Azure araç çubuğunda bir bildirim gösterir.

<a name="manage-logic-app-versions"></a>

## <a name="manage-logic-app-versions"></a>Mantıksal uygulama sürümlerini yönetme

Mantıksal uygulamalarınızın sürüm denetimi için Azure portal kullanabilirsiniz. Mantıksal uygulamanızın sürüm geçmişini bulabilir ve önceki sürümleri yükseltebilirsiniz.

<a name="find-version-history"></a>

### <a name="find-and-view-previous-versions"></a>Önceki sürümleri bul ve görüntüle

1. Azure portal, yönetmek istediğiniz [mantıksal uygulamayı bulun](#find-logic-app).

1. Mantıksal uygulamanızın menüsünde, **geliştirme araçları**altında **sürümler**' i seçin.

   ![Azure portal 'de mantıksal uygulamanın, geliştirme araçları altındaki sürümler sayfasının ekran görüntüsü.](./media/manage-logic-apps-with-azure-portal/logic-apps-menu-versions.png)

1. Listeden yönetilecek mantıksal uygulamanızın **sürümünü** seçin. Listeyi filtrelemek için arama çubuğuna **Sürüm** tanımlayıcıyı girebilirsiniz.

1. **Geçmiş sürümü** sayfasında, önceki sürümün ayrıntılarını salt okuma modunda görürsünüz. Logic Apps **Tasarımcı** ve **kod görünümü** modları arasında seçim yapabilirsiniz.

   ![Kod görünümü ve tasarımcı görünümü seçeneklerini gösteren Logic Apps geçmiş sürümü sayfasının ekran görüntüsü.](./media/manage-logic-apps-with-azure-portal/history-version.png)

<a name="promote-previous-versions"></a>

### <a name="promote-previous-versions"></a>Önceki sürümleri yükselt

1. Mantıksal uygulamanızın sürüm geçmişinde, [yükseltmek istediğiniz sürümü bulun ve seçin](#find-version-history).

1. **Geçmiş sürümü** sayfasında, **Yükselt**' i seçin.

   ![Önceki bir sürümü yükseltmek için düğme gösteren mantıksal uygulamanın sürüm geçmişi ekran görüntüsü.](./media/manage-logic-apps-with-azure-portal/promote-button.png)

1. Açılan **Logic Apps tasarımcı** sayfasında, yükseltmekte olduğunuz sürümü gerektiği gibi düzenleyin. **Tasarımcı** ve **kod görünümü** modları arasında geçiş yapabilirsiniz. **Parametreleri**, **şablonları**ve **bağlayıcıları**da güncelleştirebilirsiniz.

   ![Bir mantıksal uygulamanın önceki bir sürümünü yükseltmek için düğme gösteren Logic Apps Tasarımcısı ekran görüntüsü.](./media/manage-logic-apps-with-azure-portal/promote-page.png)

1. Tüm güncelleştirmeleri kaydetmek ve önceki sürümü yükseltme işlemi tamamlandıktan sonra **Kaydet**' i seçin. (Veya değişikliklerinizi iptal etmek için **at**' ı seçin.) 

   [Mantıksal uygulamanızın sürüm geçmişini yeniden görüntülediğinizde](#find-version-history) , yükseltilen sürüm listenin en üstünde görünür ve yeni bir tanımlayıcıya sahiptir.

## <a name="next-steps"></a>Sonraki adımlar

* [Mantıksal uygulamaları izleme](monitor-logic-apps.md)
