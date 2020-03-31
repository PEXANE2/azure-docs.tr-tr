---
title: Visual Studio Code ile görevleri otomatikleştirin
description: Visual Studio Code (VS Code) kullanarak JSON tanımlarının altında yatan mantık uygulaması oluşturma veya düzenleme
services: logic-apps
ms.suite: integration
ms.reviewer: klam, deli, logicappspm
ms.topic: quickstart
ms.custom: mvc
ms.date: 10/25/2019
ms.openlocfilehash: 819a60887743f39d3c2ffab3c955b2980cee2725
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "74784842"
---
# <a name="quickstart-create-and-manage-logic-app-workflow-definitions-by-using-visual-studio-code"></a>Quickstart: Visual Studio Code kullanarak mantık uygulaması iş akışı tanımlarını oluşturma ve yönetme

[Azure Logic Apps](../logic-apps/logic-apps-overview.md) ve Visual Studio Code ile, uygulamaları, verileri, sistemleri ve hizmetleri kuruluşlar ve kuruluşlar arasında tümleştirmek için görevleri, iş akışlarını ve süreçleri otomatikleştirmenize yardımcı olan mantıksal uygulamalar oluşturabilir ve yönetebilirsiniz. Bu hızlı başlangıç, kod tabanlı bir deneyim aracılığıyla mantık uygulamaları için JavaScript Object Notation (JSON) kullanan temel iş akışı tanımlarını nasıl oluşturabileceğinizi ve bunları nasıl edebileceğinizi gösterir. Azure'a zaten dağıtılmış olan varolan mantık uygulamaları üzerinde de çalışabilirsiniz.

Aynı görevleri [Azure portalında](https://portal.azure.com) ve Visual Studio'da gerçekleştirebiliyor olsanız da, mantıksal uygulama tanımlarını zaten bildiğiniz ve doğrudan kod la çalışmak istediğinizde Visual Studio Code'a daha hızlı başlayabilirsiniz. Örneğin, zaten oluşturulmuş mantık uygulamalarını devre dışı kılabilir, etkinleştirebilir, silebilir ve yenileyebilirsiniz. Ayrıca, Visual Studio Code'un çalıştığı Linux, Windows ve Mac gibi herhangi bir geliştirme platformundan mantık uygulamaları ve tümleştirme hesapları üzerinde çalışabilirsiniz.

Bu makale için, temel kavramlara daha fazla odaklanan bu [quickstart'tan](../logic-apps/quickstart-create-first-logic-app-workflow.md)aynı mantık uygulamasını oluşturabilirsiniz. Visual Studio Code'da mantık uygulaması aşağıdaki örnekgibi görünür:

![Örnek mantık uygulaması iş akışı tanımı](./media/quickstart-create-logic-apps-visual-studio-code/visual-studio-code-overview.png)

Başlamadan önce şunlara sahip olduğunuzdan emin olun:

* Azure hesabınız ve aboneliğiniz yoksa, [ücretsiz bir Azure hesabı için kaydolun.](https://azure.microsoft.com/free/)

* JSON ile açıklandığı gibi [mantık uygulaması iş akışı tanımları](../logic-apps/logic-apps-workflow-definition-language.md) ve yapıları hakkında temel bilgi

  Logic Apps'ta yeniyseniz, Azure portalında ilk mantık uygulamalarınızı oluşturan ve temel kavramlara daha fazla odaklanan bu [hızlı başlangıcı](../logic-apps/quickstart-create-first-logic-app-workflow.md)deneyin.

* Azure'da oturum açmak için web'e ve Azure aboneliğinize erişim

* Henüz yoksa şu araçları indirip yükleyin:

  * [Visual Studio Code sürüm 1.25.1 veya daha sonra](https://code.visualstudio.com/), hangi ücretsizdir

  * Azure Mantık Uygulamaları için Visual Studio Code uzantısı

    Bu uzantıyı [Visual Studio Marketplace'ten](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-logicapps) veya doğrudan Visual Studio Code'un içinden indirip yükleyebilirsiniz. Yükledikten sonra Visual Studio Code'u yeniden yüklediğinizden emin olun.

    !["Azure Mantık Uygulamaları için Visual Studio Code uzantısını" bulun](./media/quickstart-create-logic-apps-visual-studio-code/find-install-logic-apps-extension.png)

    Uzantınızın doğru yüklenmesini denetlemek için Visual Studio Code araç çubuğunuzda görünen Azure simgesini seçin.

    ![Uzantındoğru yüklenmiş onaylayın](./media/quickstart-create-logic-apps-visual-studio-code/confirm-installed-visual-studio-code-extension.png)

    Daha fazla bilgi için [Uzantı Marketi'ne](https://code.visualstudio.com/docs/editor/extension-gallery)bakın. Bu uzantın açık kaynak sürümüne katkıda bulunmak [için GitHub'daki Visual Studio Code için Azure Logic Apps uzantısını](https://github.com/Microsoft/vscode-azurelogicapps)ziyaret edin.

<a name="sign-in-azure"></a>

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

1. Visual Studio Code'u açın. Visual Studio Code araç çubuğunda Azure simgesini seçin.

   ![Visual Studio Code araç çubuğunda Azure simgesini seçin](./media/quickstart-create-logic-apps-visual-studio-code/open-extensions-visual-studio-code.png)

1. Azure penceresinde, **Logic Apps**altında, **Azure'da Oturum Aç'ı**seçin. Microsoft oturum açma sayfası sizden istendiğinde, Azure hesabınızla oturum açın.

   !["Azure'da Oturum Aç" seçeneğini belirleyin](./media/quickstart-create-logic-apps-visual-studio-code/sign-in-azure-visual-studio-code.png)

   1. Oturum açma normalden uzun sürüyorsa, Visual Studio Code size bir aygıt kodu sunarak microsoft kimlik doğrulama web sitesinde oturum açmanızı ister. Bunun yerine kodla oturum açabilmek için **Aygıt Kodunu Kullan'ı**seçin.

      ![Bunun yerine aygıt koduyla devam edin](./media/quickstart-create-logic-apps-visual-studio-code/use-device-code-prompt.png)

   1. Kodu kopyalamak **için, Aç&** kopyala'yı seçin.

      ![Azure oturum açma için kod kopyalama](./media/quickstart-create-logic-apps-visual-studio-code/sign-in-prompt-authentication.png)

   1. Yeni bir tarayıcı penceresi açmak ve kimlik doğrulama web sitesine devam etmek için **Bağlantıyı Aç'ı**seçin.

      ![Tarayıcı açma ve kimlik doğrulama web sitesine gitme onaylama](./media/quickstart-create-logic-apps-visual-studio-code/confirm-open-link.png)

   1. Hesap **sayfanızda Oturum Aç'ta** kimlik doğrulama kodunuzu girin ve **İleri'yi**seçin.

      ![Azure oturum açma için kimlik doğrulama kodunu girin](./media/quickstart-create-logic-apps-visual-studio-code/authentication-code-azure-sign-in.png)

1. Azure hesabınızı seçin. Oturum açmadan sonra tarayıcınızı kapatıp Visual Studio Code'a dönebilirsiniz.

   Azure bölmesinde, **Mantıksal Uygulamalar** ve **Tümleştirme Hesapları** bölümleri artık hesabınızla ilişkili Azure aboneliklerini gösterir. Ancak, beklediğiniz abonelikleri görmüyorsanız veya bölümlerde çok fazla abonelik varsa aşağıdaki adımları izleyin:

   1. İşaretçinizi **Logic Apps** etiketinin üzerine taşıyın. Araç çubuğu göründüğünde, **Abonelikleri Seç** 'i (filtre simgesi) seçin.

      ![Azure aboneliklerini bulma veya filtreleme](./media/quickstart-create-logic-apps-visual-studio-code/find-or-filter-subscriptions.png)

   1. Görünen listeden, görünmesini istediğiniz abonelikleri seçin.

1. **Logic Apps**altında, istediğiniz aboneliği seçin. Abonelik düğümü genişler ve bu abonelikte bulunan tüm mantık uygulamalarını gösterir.

   ![Azure aboneliğinizi seçin](./media/quickstart-create-logic-apps-visual-studio-code/select-azure-subscription.png)

   > [!TIP]
   > **Tümleştirme Hesapları**altında, aboneliğinizi seçmek, bu abonelikte bulunan tümleştirme hesaplarını gösterir.

<a name="create-logic-app"></a>

## <a name="create-new-logic-app"></a>Yeni mantık uygulaması oluşturma

1. Azure hesabınızda henüz oturum açmadıysanız ve Visual Studio Code'un içinden abonelik aldıysanız, [şimdi oturum açmak için önceki adımları](#sign-in-azure)izleyin.

1. Visual Studio kodunda, **Logic Apps**altında, aboneliğinizin kısayol menüsünü açın ve **Mantık Uygulaması Oluştur'u**seçin.

   ![Abonelik menüsünden "Mantık Uygulaması Oluştur" seçeneğini belirleyin](./media/quickstart-create-logic-apps-visual-studio-code/create-logic-app-visual-studio-code.png)

   Bir liste görünür ve aboneliğinizdeki tüm Azure kaynak gruplarını gösterir.

1. Kaynak grubu listesinden, yeni bir kaynak grubu veya varolan **bir kaynak grubu oluştur'u** seçin. Bu örnekte, yeni bir kaynak grubu oluşturun.

   ![Yeni bir Azure kaynak grubu oluşturma](./media/quickstart-create-logic-apps-visual-studio-code/select-or-create-azure-resource-group.png)

1. Azure kaynak grubunuz için bir ad sağlayın ve ENTER tuşuna basın.

   ![Azure kaynak grubunuz için ad sağlama](./media/quickstart-create-logic-apps-visual-studio-code/enter-name-resource-group.png)

1. Mantık uygulamanızın meta verilerini kaydetmek istediğiniz Azure bölgesini seçin.

   ![Mantık uygulaması meta verilerini kaydetmek için Azure konumunu seçin](./media/quickstart-create-logic-apps-visual-studio-code/select-azure-location-new-resources.png)

1. Mantık uygulamanız için bir ad girin ve Enter tuşuna basın.

   ![Mantık uygulamanız için ad sağlayın](./media/quickstart-create-logic-apps-visual-studio-code/enter-name-logic-app.png)

   Azure penceresinde, Azure aboneliğiniz altında yeni ve boş mantık uygulamanız görüntülenir. Visual Studio Code ayrıca mantık uygulamanız için bir iskelet iş akışı tanımı içeren bir JSON (.logicapp.json) dosyaaçar. Artık bu JSON dosyasında mantık uygulamanızın iş akışı tanımını el ile yazmaya başlayabilirsiniz. İş akışı tanımının yapısı ve sözdizimi hakkında teknik başvuru için [Azure Mantık Uygulamaları için İş Akışı Tanımı Dili şemasına](../logic-apps/logic-apps-workflow-definition-language.md)bakın.

   ![Boş mantık uygulaması iş akışı tanımı JSON dosyası](./media/quickstart-create-logic-apps-visual-studio-code/empty-logic-app-workflow-definition.png)

   Örneğin, burada bir RSS tetikleyici ve Bir Office 365 Outlook eylem ile başlayan bir örnek mantık uygulaması iş akışı tanımı vardır. Genellikle, JSON öğeleri her bölümde alfabetik olarak görünür. Ancak, bu örnek, mantık uygulamasının adımları tasarımcıgörünür sırayla kabaca bu öğeleri gösterir.

   > [!IMPORTANT]
   > Bu örnek mantık uygulaması tanımını yeniden kullanmak istiyorsanız, örneğin bir Office 365 kuruluş hesabına ihtiyacınız @fabrikam.comvar. Hayali e-posta adresini kendi e-posta adresinizle değiştirdiğinizden emin olun. Outlook.com veya Gmail gibi farklı bir e-posta `Send_an_email_action` bağlayıcısı kullanmak için, eylemi Azure Logic Apps tarafından desteklenen bir [e-posta bağlayıcısından](../connectors/apis-list.md)elde edilen benzer bir eylemle değiştirin.

   ```json
   {
      "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
         "$connections": {
            "defaultValue": {},
            "type": "Object"
         }
      },
      "triggers": {
         "When_a_feed_item_is_published": {
            "recurrence": {
               "frequency": "Minute",
               "interval": 1
            },
            "splitOn": "@triggerBody()?['value']",
            "type": "ApiConnection",
            "inputs": {
               "host": {
                  "connection": {
                     "name": "@parameters('$connections')['rss']['connectionId']"
                  }
               },
               "method": "get",
               "path": "/OnNewFeed",
               "queries": {
                  "feedUrl": "http://feeds.reuters.com/reuters/topNews"
               }
            }
         }
      },
      "actions": {
         "Send_an_email_(V2)": {
            "runAfter": {},
            "type": "ApiConnection",
            "inputs": {
               "body": {
                  "Body": "<p>Title: @{triggerBody()?['title']}<br>\n<br>\nDate published: @{triggerBody()?['updatedOn']}<br>\n<br>\nLink: @{triggerBody()?['primaryLink']}</p>",
                  "Subject": "RSS item: @{triggerBody()?['title']}",
                  "To": "sophia-owen@fabrikam.com"
               },
               "host": {
                  "connection": {
                     "name": "@parameters('$connections')['office365']['connectionId']"
                  }
               },
               "method": "post",
               "path": "/v2/Mail"
            }
         }
      },
      "outputs": {}
   }
   ```

1. İşiniz bittiğinde, mantık uygulamanızın iş akışı tanımını kaydedin. (Dosya menüsü > Kaydet veya Ctrl+S tuşuna basın)

1. Mantık uygulamanızı Azure aboneliğinize yüklemeniz istendiğinde **Yükle'yi**seçin.

   Bu adım, mantık uygulamanızı [Azure portalına](https://portal.azure.com)yayınlar ve mantığınızı Azure'da canlı ve çalışır durumda kılar.

   ![Azure aboneliğinize yeni mantık uygulaması yükleyin](./media/quickstart-create-logic-apps-visual-studio-code/upload-new-logic-app.png)

## <a name="view-logic-app-in-designer"></a>Tasarımcıda mantık uygulamasını görüntüleyin

Visual Studio Code'da mantık uygulamanızı salt okunur tasarım görünümünde açabilirsiniz. Tasarımcıda mantık uygulamanızı düzenlemenize rağmen, tasarımcı görünümünü kullanarak mantık uygulamanızın iş akışını görsel olarak kontrol edebilirsiniz.

Azure penceresinde, **Logic Apps**altında, mantık uygulamanızın kısayol menüsünü açın ve **Tasarımcı'da Aç'ı**seçin.

Salt okunur tasarımcı ayrı bir pencerede açılır ve mantık uygulamanızın iş akışını gösterir, örneğin:

![Salt okunur tasarımcıda mantık uygulamasını görüntüleme](./media/quickstart-create-logic-apps-visual-studio-code/logic-app-designer-view.png)

## <a name="view-in-azure-portal"></a>Azure portalında görünüm

Azure portalında mantık uygulamanızı incelemek için aşağıdaki adımları izleyin:

1. Mantık uygulamanızla ilişkili aynı Azure hesabını ve aboneliğini kullanarak [Azure portalında](https://portal.azure.com) oturum açın.

1. Azure portalının arama kutusuna mantık uygulamalarınızın adını girin. Sonuç listesinden mantık uygulamanızı seçin.

   ![Azure portalındaki yeni mantık uygulamanız](./media/quickstart-create-logic-apps-visual-studio-code/published-logic-app-in-azure.png)

<a name="disable-enable-logic-app"></a>

## <a name="disable-or-enable-logic-app"></a>Mantıksal uygulamayı devre dışı bırakma veya etkinleştirme

Visual Studio Code'da, yayınlanan bir mantık uygulamasını ve değişikliklerinizi kaydederseniz, zaten dağıtılan uygulamanızın *üzerine yazarsınız.* Üretimde mantık uygulamanızı kırmamak ve kesintiyi en aza indirmek için önce mantık uygulamanızı devre dışı bırakın. Daha sonra mantık uygulamanızın hala çalıştığını doğruladıktan sonra mantık uygulamanızı reaktif edebilirsiniz.

1. Azure hesabınızda henüz oturum açmadıysanız ve Visual Studio Code'un içinden abonelik aldıysanız, [şimdi oturum açmak için önceki adımları](#sign-in-azure)izleyin.

1. Azure penceresinde, **Logic Apps**altında, azure aboneliğinizi genişleterek söz sahibi abonelikteki tüm mantık uygulamalarını görüntüleyebilirsiniz.

   1. İstediğiniz mantık uygulamasını devre dışı bırakıp, mantık uygulamasının menüsünü açın ve **Devre Dışı'yı**seçin.

      ![Mantık uygulamanızı devre dışı](./media/quickstart-create-logic-apps-visual-studio-code/disable-published-logic-app.png)

   1. Mantık uygulamanızı yeniden etkinleştirmeye hazır olduğunuzda, mantık uygulamasının menüsünü açın ve **Etkinleştir'i**seçin.

      ![Mantık uygulamanızı etkinleştirin](./media/quickstart-create-logic-apps-visual-studio-code/enable-published-logic-app.png)

<a name="edit-logic-app"></a>

## <a name="edit-deployed-logic-app"></a>Dağıtılan mantık uygulamasını edin

Visual Studio Code'da, Azure'da zaten dağıtılmış bir mantık uygulaması için iş akışı tanımını açabilir ve edinebilirsiniz.

> [!IMPORTANT] 
> Üretimde aktif olarak çalışan bir mantık uygulamasını düzenlemeden önce, bu mantık uygulamasını kırma riskini önlün ve [öncelikle mantık uygulamanızı devre dışı bırakarak](#disable-enable-logic-app)kesintiyi en aza indirin.

1. Azure hesabınızda henüz oturum açmadıysanız ve Visual Studio Code'un içinden abonelik aldıysanız, [şimdi oturum açmak için önceki adımları](#sign-in-azure)izleyin.

1. Azure penceresinde, **Logic Apps**altında Azure aboneliğinizi genişletin ve istediğiniz mantık uygulamasını seçin.

1. Mantık uygulamanızın menüsünü açın ve **Editor'da Aç'ı**seçin. Veya mantık uygulamanızın adının yanında, edit simgesini seçin.

   ![Mevcut mantık uygulaması için açık düzenleyici](./media/quickstart-create-logic-apps-visual-studio-code/open-editor-existing-logic-app.png)

   Visual Studio Code, mantık uygulamanızın iş akışı tanımını görüntüleyebilmeniz için yerel geçici klasörünüzde .logicapp.json dosyasını açar.

   ![Yayınlanan mantık uygulaması için iş akışı tanımını görüntüleme](./media/quickstart-create-logic-apps-visual-studio-code/edit-published-logic-app-workflow-definition.png)

1. Mantık uygulamasının iş akışı tanımındaki değişikliklerinizi yapın.

1. İşiniz bittiğinde yaptığınız değişiklikleri kaydedin. (Dosya menüsü > Kaydet veya Ctrl+S tuşuna basın)

1. Değişikliklerinizi yüklemeniz ve Azure portalında mevcut mantık uygulamanızın *üzerine yazmanız* istendiğinde **Yükle'yi**seçin.

   Bu adım, Azure [portalında](https://portal.azure.com)mantık uygulamanıza yapılan güncelleştirmelerinizi yayımlar.

   ![Azure'da mantık uygulaması tanımına yükleme](./media/quickstart-create-logic-apps-visual-studio-code/upload-logic-app-changes.png)

## <a name="view-or-promote-other-versions"></a>Diğer sürümleri görüntüleme veya tanıtma

Visual Studio Code'da mantık uygulamanızın önceki sürümlerini açabilir ve inceleyebilirsiniz. Daha önceki bir sürümü geçerli sürüme de tanıtabilirsiniz.

> [!IMPORTANT] 
> Üretimde aktif olarak çalışan bir mantık uygulamasını değiştirmeden önce, bu mantık uygulamasını kırma riskini önlün ve [önce mantık uygulamanızı devre dışı bırakarak](#disable-enable-logic-app)kesintiyi en aza indirin.

1. Azure penceresinde, **Logic Apps**altında, azure aboneliğinizi genişleterek söz sahibi abonelikteki tüm mantık uygulamalarını görüntüleyebilirsiniz.

1. Aboneliğiniz altında, mantık uygulamanızı genişletin ve **Sürümleri**genişletin.

   **Sürümler** listesi, varsa mantık uygulamanızın önceki sürümlerini gösterir.

   ![Mantık uygulamanızın önceki sürümleri](./media/quickstart-create-logic-apps-visual-studio-code/view-previous-versions.png)

1. Önceki bir sürümü görüntülemek için aşağıdakileri seçin:

   * JSON tanımını görüntülemek için, **Sürümler**altında, bu tanım için sürüm numarasını seçin. Veya, bu sürümün kısayol menüsünü açın ve **Düzenleyici'de Aç'ı**seçin.

     Yerel bilgisayarınızda yeni bir dosya açılır ve bu sürümün JSON tanımını gösterir.

   * Yalnızca okunan tasarımcı görünümünde sürümü görüntülemek için, bu sürümün kısayol menüsünü açın ve **Designer'da Aç'ı**seçin.

1. Önceki bir sürümü geçerli sürüme tanıtmak için aşağıdaki adımları izleyin:

   1. **Sürümler**altında, önceki sürümün kısayol menüsünü açın ve **Tanıt'ı**seçin.

      ![Önceki sürümü tanıtma](./media/quickstart-create-logic-apps-visual-studio-code/promote-earlier-version.png)

   1. Visual Studio Code onay için sizden istendikten sonra devam etmek için **Evet'i**seçin.

      ![Önceki sürümün tanıtımını onaylama](./media/quickstart-create-logic-apps-visual-studio-code/confirm-promote-version.png)

      Visual Studio Code, seçili sürümü geçerli sürüme tanıtıyor ve tanıtılan sürüme yeni bir numara atar. Daha önce geçerli olan sürüm artık tanıtılan sürümün altında görünür.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Visual Studio ile mantık uygulamaları oluşturun](../logic-apps/quickstart-create-logic-apps-with-visual-studio.md)