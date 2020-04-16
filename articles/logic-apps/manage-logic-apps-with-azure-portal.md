---
title: Azure portalında mantık uygulamalarını yönetme
description: Azure portalını kullanarak mantık uygulamalarını düzenleyin, etkinleştirin, devre dışı edin veya silin.
services: logic-apps
ms.suite: integration
author: lauradolan
ms.author: ladolan
ms.reviewer: estfan, logicappspm
ms.topic: article
ms.custom: mvc
ms.date: 04/13/2020
ms.openlocfilehash: f726ca90c215c4aff3734bd8022bbc1ad4dc5f87
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81415942"
---
# <a name="manage-logic-apps-in-the-azure-portal"></a>Azure portalında mantık uygulamalarını yönetme

Azure [portalını](https://portal.azure.com) veya [Visual Studio'yu](manage-logic-apps-with-visual-studio.md)kullanarak mantık uygulamalarını yönetebilirsiniz. Bu makalede, Azure portalındaki mantık uygulamalarının nasıl düzenlenebileceği, devre dışı katılmadı, etkinleştirilir veya silinebileceği gösterilmektedir. Azure Logic Apps'ta yeniyseniz, azure [mantık uygulamaları nedir?](logic-apps-overview.md)

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği. Azure aboneliğiniz yoksa [ücretsiz bir Azure hesabı için kaydolun](https://azure.microsoft.com/free/).

* Varolan bir mantık uygulaması. Azure portalında bir mantık uygulaması oluşturmayı öğrenmek için [Bkz. Quickstart: Azure Logic Apps - Azure portalını kullanarak ilk iş akışınızı oluşturun.](quickstart-create-first-logic-app-workflow.md)

<a name="find-logic-app"></a>

## <a name="find-your-logic-apps"></a>Mantık uygulamalarınızı bulun

Mantık uygulamanızı bulmak ve açmak için aşağıdaki adımları izleyin:

1. Azure hesabınızla [Azure portalında](https://portal.azure.com) oturum açın.

1. Azure arama çubuğuna `logic apps` **Logic Apps'ı**girin ve seçin.

   !["Mantık Uygulamaları"nı bulun ve seçin](./media/manage-logic-apps-with-azure-portal/find-select-logic-apps.png)

1. Logic **Apps** sayfasında yönetmek istediğiniz mantık uygulamasını bulun ve seçin.

   Mantık uygulamasının **Genel Bakış** bölmesi açıldıktan sonra, **Mantık Uygulamaları** sayfasında görünen listeye şu şekilde filtre uygulayabilirsiniz:

   * Mantık uygulamalarını ada göre ara
   * Mantık uygulamalarını aboneye, kaynak grubuna, konuma ve etiketlere göre filtreleme
   * Mantık uygulamalarını kaynak grubuna, türüne, aboneliğine ve konumuna göre gruplandırma

## <a name="view-logic-app-properties"></a>Mantık uygulaması özelliklerini görüntüleme

1. Azure portalında [mantık uygulamanızı bulun ve açın.](#find-logic-app)

1. Mantık uygulamanızın menüsünden **Ayarlar**altında **Özellikler'i**seçin.

1. **Özellikler** bölmesinde, mantık uygulamanızla ilgili aşağıdaki bilgileri görüntüleyebilir ve kopyalayabilirsiniz:

   * **Adı**
   * **Kaynak kimliği**
   * **Kaynak grubu**
   * **Konum**
   * **Tür** 
   * **Abonelik Adı**
   * **Abonelik Kimliği**
   * **Erişim Bitiş Noktası**
   * **Runtime giden IP adresleri**
   * **Uç nokta IP adreslerine erişin**
   * **Bağlayıcı giden IP adresleri**

## <a name="disable-or-enable-logic-apps"></a>Mantık uygulamalarını devre dışı veya etkinleştirme

Azure portalında tek bir [mantık uygulamasını](#disable-enable-single-logic-app) veya birden çok mantık uygulamasını aynı [anda](#disable-or-enable-multiple-logic-apps) etkinleştirebilir veya devre dışı kullanabilirsiniz. [Ayrıca Visual Studio'da mantık uygulamalarını etkinleştirebilir veya devre dışı kullanabilirsiniz.](manage-logic-apps-with-visual-studio.md#disable-or-enable-logic-app)

Mantık uygulamanızı devre dışı bırakmak iş akışı örneklerinizi etkiler ve şu şekilde çalışır:

* Devam eden ve bekleyen tüm çalıştırmalar bitene kadar devam ediyor. Bu çalıştırmaların sayısına bağlı olarak, bu işlem biraz zaman alabilir.

* Logic Apps altyapısı yeni iş akışı örnekleri oluşturmaz veya çalıştırmaz.

* Tetikleyici, koşulları yerine getirildiğinde ateş etmeyecektir.

* Tetikleyici durumu, mantık uygulamasının durdurulduğu noktayı hatırlar. Bu nedenle, mantık uygulamasını yeniden etkinleştirirseniz, tetikleyici son çalıştırmadan bu yana işlenmemiş tüm öğeler için yangınlar.

  Mantık uygulamanızın son çalıştırmadan beri işlenmemiş öğelere ateş etmesini engellemek için, mantık uygulamasını yeniden etkinleştirmeden önce tetikleyicinin durumunu temizleyin:

  1. Azure portalında [mantık uygulamanızı bulun ve açın.](#find-logic-app)

  1. Mantık uygulamasının tetikleyicisinin herhangi bir bölümünü edin.

  1. Yaptığınız değişiklikleri kaydedin. Bu adım, tetikleyicinizin geçerli durumunu sıfırlar.

  1. [Mantık uygulamanızı yeniden etkinleştirin.](#disable-enable-single-logic-app)

<a name="disable-enable-single-logic-app"></a>

### <a name="disable-or-enable-single-logic-app"></a>Tek bir mantık uygulamasını devre dışı veya etkinleştirme

1. Azure portalında [mantık uygulamanızı bulun ve açın.](#find-logic-app)

1. Mantık uygulamanızın menüsünde **Genel Bakış'ı**seçin. Bu seçeneklerden birini seçin:

   * Araç çubuğunda **Devre Dışı'** yı seçin.

     ![Azure portalında tek mantık uygulamasını devre dışı](./media/manage-logic-apps-with-azure-portal/disable-single-logic-app.png)

     Mantık uygulamanız zaten devre dışı bırakılmışsa, yalnızca **Etkinleştir** seçeneğini görürsünüz.

   * Araç çubuğunda **Etkinleştir'i**seçin.

     ![Azure portalında tek bir mantık uygulamasını etkinleştirme](./media/manage-logic-apps-with-azure-portal/enable-single-logic-app.png)

     Mantık uygulamanız zaten etkinse, **yalnızca Devre Dışı Atabilme** seçeneğini görürsünüz. 

   Azure portalı, ana Azure araç çubuğunda, işleminizin başarılı olup olmadığını veya başarısız olup olmadığını doğrulayan bir bildirim gösterir.

   ![Operasyon durumunu onaylamak için bildirim](./media/manage-logic-apps-with-azure-portal/operation-confirmation-notification.png)

<a name="disable-or-enable-multiple-logic-apps"></a>

### <a name="disable-or-enable-multiple-logic-apps"></a>Birden çok mantık uygulamalarını devre dışı veya etkinleştirme

1. Azure portalında, devre dışı kılmasını veya etkinleştirilmesini istediğiniz [mantık uygulamalarını bulun.](#find-logic-app)

1. **Mantık Uygulamaları** sayfasında, bir mantık uygulamasının şu anda etkin veya devre dışı olup olmadığını kontrol etmek için, bu mantık uygulamasının **Durum** sütununa bakın. 

   ![Mantık Uygulamaları durum sütunu](./media/manage-logic-apps-with-azure-portal/view-logic-app-status.png)

   **Durum** sütunu görünmüyorsa, **Mantık Uygulamaları** araç çubuğunda **önizlemeyi den'i**seçin.

   ![Önizlemeyi açma](./media/manage-logic-apps-with-azure-portal/select-try-preview.png)

1. Onay kutusu sütununda, devre dışı kalmak veya etkinleştirmek istediğiniz mantık uygulamalarını seçin. Araç çubuğunda **Devre Dışı Nı** veya **Etkinleştir'i**seçin.

   ![Azure portalında birden çok mantık uygulaması etkinleştirme veya devre dışı](./media/manage-logic-apps-with-azure-portal/enable-disable-multiple-logic-apps.png)

1. Onay kutusu göründüğünde, devam etmek için **Evet'i** seçin.

   Azure portalı, ana Azure araç çubuğunda, işleminizin başarılı olup olmadığını veya başarısız olup olmadığını doğrulayan bir bildirim gösterir.

## <a name="delete-logic-apps"></a>Mantık uygulamalarını silme

Azure portalında [tek bir mantık uygulamasını silebilir](#delete-single-logic-app) veya birden çok mantık uygulamasını aynı [anda silebilirsiniz.](#delete-multiple-logic-apps) [Ayrıca Visual Studio'da mantık uygulamanızı silebilirsiniz.](manage-logic-apps-with-visual-studio.md#delete-your-logic-app)

Mantık uygulamanızı semeniz iş akışı örneklerinizi şu şekilde etkiler:

* Devam eden ve bekleyen tüm çalıştırmalar bitene kadar devam ediyor. Bu çalıştırmaların sayısına bağlı olarak, bu işlem biraz zaman alabilir.

* Logic Apps altyapısı yeni iş akışı örnekleri oluşturmaz veya çalıştırmaz.

<a name="delete-single-logic-app"></a>

### <a name="delete-single-logic-app"></a>Tek mantık uygulamasını silme

1. Azure portalında [mantık uygulamanızı bulun ve açın.](#find-logic-app)

1. Mantık uygulamanızın menüsünde **Genel Bakış'ı**seçin. Mantık uygulamanızın araç çubuğunda **Sil'i**seçin.

   ![Mantık uygulaması araç çubuğunda "Sil" seçeneğini belirleyin](./media/manage-logic-apps-with-azure-portal/delete-single-logic-app.png)

1. Onay kutusu göründüğünde, mantık uygulamanızın adını girin ve **Sil'i**seçin.

   ![Mantık uygulamanızı silmek için onaylayın](./media/manage-logic-apps-with-azure-portal/delete-confirmation-single-logic-app.png)

   Azure portalı, ana Azure araç çubuğunda, işleminizin başarılı olup olmadığını veya başarısız olup olmadığını doğrulayan bir bildirim gösterir.

<a name="delete-multiple-logic-apps"></a>

### <a name="delete-multiple-logic-apps"></a>Birden çok mantık uygulaması silme

1. Azure [portalında, silmek istediğiniz mantık uygulamalarını bulun.](#find-logic-app)

1. Onay kutusu sütununda, silmek istediğiniz mantık uygulamalarını seçin. Araç çubuğunda **Sil'i**seçin.

   ![Birden çok mantık uygulaması silme](./media/manage-logic-apps-with-azure-portal/delete-multiple-logic-apps.png)

1. Onay kutusu göründüğünde, girin `yes`ve **Sil'i**seçin.

   ![Mantık uygulamalarınızı silmek için onaylayın](./media/manage-logic-apps-with-azure-portal/delete-confirmation-multiple-logic-apps.png)

   Azure portalı, ana Azure araç çubuğunda, işleminizin başarılı olup olmadığını veya başarısız olup olmadığını doğrulayan bir bildirim gösterir.

<a name="manage-logic-app-versions"></a>

## <a name="manage-logic-app-versions"></a>Mantık uygulaması sürümlerini yönetme

Mantıksal uygulamalarınızın sürüm kontrolü için Azure portalını kullanabilirsiniz. Mantık uygulamanızın sürüm geçmişini bulabilir ve önceki sürümleri tanıtabilirsiniz.

<a name="find-version-history"></a>

### <a name="find-and-view-previous-versions"></a>Önceki sürümleri bulma ve görüntüleme

1. Azure [portalında, yönetmek istediğiniz mantık uygulamasını bulun.](#find-logic-app)

1. Mantık uygulamanızın menüsünde, **Geliştirme Araçları**altında, **Sürümler'i**seçin.

   ![Mantık uygulamanızın menüsünde "Geliştirme Araçları" altında "Sürümler"i seçin](./media/manage-logic-apps-with-azure-portal/logic-apps-menu-versions.png)

1. Listeden yönetmek için mantık uygulamanızın **Sürümünü** seçin. Listeye filtre lemek için arama çubuğuna **Sürüm** tanımlayıcısını girebilirsiniz.

1. Geçmiş **sürüm** sayfasında, önceki sürümün ayrıntılarını salt okunur modunda görürsünüz. Logic Apps **Tasarımcısı** ve **Kod görüntüleme** modları arasında seçim yapabilirsiniz.

   ![Kod görünümü ve Logic uygulamaları tasarımcı görünümü ile mantık uygulaması için Geçmiş sürüm sayfası](./media/manage-logic-apps-with-azure-portal/history-version.png)

<a name="promote-previous-versions"></a>

### <a name="promote-previous-versions"></a>Önceki sürümleri tanıtma

1. Mantık uygulamanızın sürüm geçmişinde, [tanıtmak istediğiniz sürümü bulun ve seçin.](#find-version-history)

1. Geçmiş **sürüm** **sayfasında, Tanıt'ı**seçin.

   ![Mantık uygulamasının sürüm geçmişinde düğmeyi tanıtın](./media/manage-logic-apps-with-azure-portal/promote-button.png)

1. Açılan **Logic Apps Designer** sayfasında, gerektiğinde tanıtımını yaptığınız sürümü edin. **Tasarımcı** ve Kod **görünüm** modları arasında geçiş yapabilirsiniz. **Parametreleri,** **Şablonları**ve **Bağlayıcıları**da güncelleştirebilirsiniz.

   ![Önceki sürümü tanıtmak için Logic Apps Designer sayfası](./media/manage-logic-apps-with-azure-portal/promote-page.png)

1. Önceki sürümün tanıtımını yapmak ve güncelleştirmeleri kaydetmek için **Kaydet'i**seçin. (Veya değişikliklerinizi iptal etmek için **At'ı**seçin .) 

   Mantık [uygulamanızın sürüm geçmişini](#find-version-history) yeniden görüntülediğinizde, tanıtılan sürüm listenin en üstünde görünür ve yeni bir tanımlayıcıya sahiptir.

## <a name="next-steps"></a>Sonraki adımlar

* [Mantıksal uygulamaları izleme](monitor-logic-apps.md)
