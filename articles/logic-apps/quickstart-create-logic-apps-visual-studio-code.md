---
title: Visual Studio Code Azure Logic Apps otomatik iş akışları oluşturma ve yönetme
description: Visual Studio Code 'de JSON ile Logic Apps oluşturma ve yönetme için hızlı başlangıç (VS Code)
services: logic-apps
ms.service: logic-apps
ms.suite: integration
ms.workload: azure-vs
author: ecfan
ms.author: estfan
ms.manager: carmonm
ms.reviewer: klam, deli, LADocs
ms.topic: quickstart
ms.custom: mvc
ms.date: 10/05/2018
ms.openlocfilehash: b9f9a402ecde09dd00c2b070f784858a141e556b
ms.sourcegitcommit: f176e5bb926476ec8f9e2a2829bda48d510fbed7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/04/2019
ms.locfileid: "70308998"
---
# <a name="quickstart-create-and-manage-automated-logic-app-workflows---visual-studio-code"></a>Hızlı Başlangıç: Otomatik mantıksal uygulama iş akışları oluşturma ve yönetme-Visual Studio Code

[Azure Logic Apps](../logic-apps/logic-apps-overview.md) ve Visual Studio Code sayesinde, kuruluşlar ve kuruluşlar genelinde uygulamaları, verileri, sistemleri ve Hizmetleri tümleştirmek için görevleri, iş akışlarını ve işlemleri otomatikleştirmenize yardımcı olan Logic Apps oluşturabilir ve yönetebilirsiniz. Bu hızlı başlangıçta, kod tabanlı bir deneyim aracılığıyla JavaScript Nesne Gösterimi (JSON) ' de iş akışı tanımı şemasıyla çalışarak mantıksal uygulama iş akışı tanımlarını nasıl oluşturabileceğiniz ve düzenleyebileceğiniz gösterilmektedir. Ayrıca, Azure 'a zaten dağıtılan mevcut Logic Apps üzerinde de çalışabilirsiniz. 

Aynı görevleri Azure Portal de gerçekleştirebilseniz de (https://portal.azure.com) ve Visual Studio 'da, mantıksal uygulama tanımlarını zaten öğreniyor ve doğrudan kodda çalışmak istediğinizde Visual Studio Code daha hızlı bir şekilde çalışmaya başlayabilir. Örneğin, önceden oluşturulmuş mantıksal uygulamaları devre dışı bırakabilir, etkinleştirebilir, silebilir ve yenileyebilirsiniz. Ayrıca, Linux, Windows ve Mac gibi Visual Studio Code çalıştığı tüm geliştirme platformlarından Logic Apps ve tümleştirme hesapları üzerinde çalışabilirsiniz.

Bu makalede, temel kavramlara daha fazla odaklanan [Azure Portal bir mantıksal uygulama oluşturmak için hızlı](../logic-apps/quickstart-create-first-logic-app-workflow.md)başlangıçta aynı mantıksal uygulamayı oluşturabilirsiniz. Visual Studio Code, mantıksal uygulama şu örneğe benzer şekilde görünür:

![Tamamlanmış mantıksal uygulama](./media/create-logic-apps-visual-studio-code/overview.png)

Başlamadan önce Şu öğelerin bulunduğundan emin olun:

* Azure aboneliğiniz yoksa [ücretsiz bir Azure hesabı için kaydolun](https://azure.microsoft.com/free/).

* [Mantıksal uygulama iş akışı tanımları](../logic-apps/logic-apps-workflow-definition-language.md) ve JavaScript nesne gösterimi kullanan yapısı hakkında temel BILGILER (JSON) 

  Logic Apps yeni başladıysanız, temel kavramlara odaklanmakta olan [Azure Portal ilk mantıksal uygulamanızı oluşturma konusunda](../logic-apps/quickstart-create-first-logic-app-workflow.md)size yol gösteren hızlı başlangıcı deneyin. 

* Azure 'da ve Azure aboneliğinizde oturum açmak için Web 'e erişin

* Henüz yoksa şu araçları indirip yükleyin: 

  * [Visual Studio Code sürüm 1.25.1 veya üzeri](https://code.visualstudio.com/), ücretsiz

  * Azure Logic Apps için Visual Studio Code uzantısı

    Bu uzantıyı [Visual Studio Market](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-logicapps) indirebilir ve doğrudan Visual Studio Code içinden yükleyebilirsiniz. 
    Yükledikten sonra Visual Studio Code yeniden yüklediğinizden emin olun. 

    !["Azure Logic Apps için Visual Studio Code uzantısını bul"](./media/create-logic-apps-visual-studio-code/find-install-logic-apps-extension.png)

    Uzantının doğru şekilde yüklenip yüklenmediğini denetlemek için, Visual Studio Code araç çubuğinizdeki Azure simgesi görüntülenir. 

    ![Uzantı yüklendi](./media/create-logic-apps-visual-studio-code/installed-extension.png)

    Daha fazla bilgi için bkz. [uzantı marketi](https://code.visualstudio.com/docs/editor/extension-gallery). Ayrıca, [GitHub 'daki Visual Studio Code Azure Logic Apps uzantısını](https://github.com/Microsoft/vscode-azurelogicapps)ziyaret ederek bu uzantının açık kaynaklı sürümüne katkılarını görüntüleyebilir ve gönderebilirsiniz. 

<a name="sign-in-azure"></a>

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

1. Visual Studio Code'u açın. Visual Studio Code araç çubuğunda Azure simgesini seçin. 

   ![Azure simgesini seçin](./media/create-logic-apps-visual-studio-code/open-extension.png)

1. Azure penceresinde, **Logic Apps**altında **Azure 'da oturum aç**' ı seçin. 

   !["Azure 'da oturum aç" ı seçin](./media/create-logic-apps-visual-studio-code/sign-in-azure.png)

   Artık, belirtilen kimlik doğrulama kodunu kullanarak oturum açmanız istenir. 

1. Kimlik doğrulama kodunu kopyalayın ve ardından yeni bir tarayıcı penceresi açan **& aç**' ı seçin.

   ![Oturum açma istemi](./media/create-logic-apps-visual-studio-code/sign-in-prompt.png)

1. Kimlik doğrulama kodunuzu girin. Sorulduğunda **devam**' ı seçin.

   ![Kodu girin](./media/create-logic-apps-visual-studio-code/authentication-code.png)

1. Azure hesabınızı seçin. Oturum açtıktan sonra tarayıcınızı kapatabilir ve Visual Studio Code geri dönebilirsiniz.

   Azure penceresinde, Logic Apps bölmesi ve tümleştirme hesapları bölmesi artık hesabınızdaki Azure aboneliklerini gösterir. 

   ![Abonelik seçme](./media/create-logic-apps-visual-studio-code/select-azure-subscription.png)

   Tahmin ettiğiniz abonelikleri görmüyorsanız **Logic Apps** etiket ' ın yanındaki **abonelikleri Seç** (filtre simgesi) öğesini seçin. İstediğiniz abonelikleri bulun ve seçin.

1. Azure aboneliğinizdeki mevcut mantıksal uygulamaları veya tümleştirme hesaplarını görüntülemek için aboneliğinizi genişletin.

   ![Mantıksal uygulamaları ve tümleştirme hesaplarını görüntüleme](./media/create-logic-apps-visual-studio-code/existing-logic-apps.png)

<a name="create-logic-app"></a>

## <a name="create-logic-app"></a>Mantıksal uygulama oluşturma

1. Visual Studio Code içinden Azure aboneliğinizde oturum açmadıysanız, [Şimdi oturum açmak](#sign-in-azure)için bu makaledeki adımları izleyin.

1. Aboneliğinizin bağlam menüsünden **Oluştur**' u seçin.

   !["Oluştur" u seçin](./media/create-logic-apps-visual-studio-code/create-logic-app.png)

1. Aboneliğinizdeki Azure kaynak gruplarını gösteren listeden, var olan bir kaynak grubunu seçin veya **Yeni bir kaynak grubu oluşturun**. 

   Bu örnek, yeni bir kaynak grubu oluşturur:

   ![Yeni kaynak grubu oluştur](./media/create-logic-apps-visual-studio-code/select-or-create-azure-resource-group.png)

1. Azure Kaynak grubunuz için bir ad girin ve ardından ENTER tuşuna basın.

   ![Kaynak grubunuzu adlandırın](./media/create-logic-apps-visual-studio-code/enter-name-resource-group.png)

1. Mantıksal uygulamanızın meta verilerinin kaydedileceği veri merkezi konumunu seçin.

   ![Konum seçin](./media/create-logic-apps-visual-studio-code/select-location.png)

1. Mantıksal uygulamanız için bir ad girin ve ENTER tuşuna basın.

   ![Mantıksal uygulamanızı adlandırın](./media/create-logic-apps-visual-studio-code/enter-name-logic-app.png)

   Yeni mantıksal uygulamanız, Azure aboneliğiniz altında Azure penceresinde görüntülenir. Artık mantıksal uygulamanızın iş akışı tanımını oluşturmaya başlayabilirsiniz.

1. Mantıksal uygulamanızın kısayol menüsünde **düzenleyicide aç**' ı seçin. 

   ![Mantıksal uygulamayı düzenleyicide aç](./media/create-logic-apps-visual-studio-code/open-new-logic-app.png)

   Visual Studio Code mantıksal uygulamanızın iş akışını oluşturmaya başlayabilmeniz için bir mantıksal uygulama iş akışı Tanım şablonu (. logicapp. JSON dosyası) açar.

   ![Yeni mantıksal uygulama iş akışı tanımı](./media/create-logic-apps-visual-studio-code/blank-logic-app-workflow-definition.png)

1. Mantıksal uygulama iş akışı tanımı şablon dosyasında, mantıksal uygulamanızın iş akışı tanımını oluşturmaya başlayın. Teknik Başvuru için, [Azure Logic Apps Için Iş akışı Tanım Dili şemasına](../logic-apps/logic-apps-workflow-definition-language.md)bakın.

   Örnek bir mantık tanımı aşağıda verilmiştir. Genellikle, JSON öğeleri her bölüm içinde alfabetik olarak görünür, ancak bu örnek bu öğeleri kabaca bir şekilde mantıksal uygulamanın adımlarının tasarımcıda göründüğü sırada gösterir.

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
         "Send_an_email": {
            "runAfter": {},
            "type": "ApiConnection",
            "inputs": {
               "body": {
                  "Body": "Title: @{triggerBody()?['title']}\n\nDate published: @{triggerBody()?['publishDate']}\n\nLink: @{triggerBody()?['primaryLink']}",
                  "Subject": "New RSS item: @{triggerBody()?['title']}",
                  "To": "Sophie.Owen@contoso.com"
               },
               "host": {
                  "connection": {
                     "name": "@parameters('$connections')['outlook']['connectionId']"
                  }
               },
               "method": "post",
               "path": "/Mail"
            }
         }
      },
      "outputs": {}
   }   
   ```

1. İşiniz bittiğinde, mantıksal uygulama tanımı dosyanızı kaydedin. Visual Studio Code, mantıksal uygulama tanımınızı Azure aboneliğinize yüklemeyi onaylamanızı isterse **karşıya yükle**' yi seçin.

   ![Yeni mantıksal uygulamanızı karşıya yükleyin](./media/create-logic-apps-visual-studio-code/upload-new-logic-app.png)

   Visual Studio Code mantıksal uygulamanızı Azure 'a yayımladıktan sonra, uygulamanızı şimdi canlı olarak bulabilir ve Azure portal çalıştırabilirsiniz. 

   ![Azure portal içinde Yayınlanan Logic App](./media/create-logic-apps-visual-studio-code/published-logic-app.png)

<a name="edit-logic-app"></a>

## <a name="edit-logic-app"></a>Mantıksal uygulamayı Düzenle

Azure 'da zaten dağıtılmış olan mevcut bir mantıksal uygulama üzerinde çalışmak için, bu uygulamanın iş akışı Tanım dosyasını Visual Studio Code açabilirsiniz.

1. Visual Studio Code içinden Azure aboneliğinizde oturum açmadıysanız, [Şimdi oturum açmak](#sign-in-azure)için bu makaledeki adımları izleyin.

1. Azure penceresinde, **Logic Apps**altında Azure aboneliğinizi genişletin ve istediğiniz mantıksal uygulamayı seçin. 

1. Mantıksal uygulamanızın menüsünden **düzenleyicide aç**' ı seçin. Ya da mantıksal uygulamanızın adının yanında Düzenle simgesini seçin.

   ![Mevcut mantıksal uygulama için düzenleyiciyi aç](./media/create-logic-apps-visual-studio-code/open-editor-existing-logic-app.png)

   Visual Studio Code mantıksal uygulamanızın iş akışı tanımı için. logicapp. json dosyasını açar.

   ![Mantıksal uygulama iş akışı tanımı açıldı](./media/create-logic-apps-visual-studio-code/edit-logic-app-workflow-definition-file.png)

1. Mantıksal uygulamanızın tanımında yaptığınız değişiklikleri yapın.

1. İşiniz bittiğinde yaptığınız değişiklikleri kaydedin.

1. Visual Studio Code Azure aboneliğinizde mantıksal uygulama tanımınızı güncelleştirmenizi isterse, **karşıya yükle**' yi seçin. 

   ![Düzenlemelerinizi karşıya yükleyin](./media/create-logic-apps-visual-studio-code/upload-logic-app-changes.png)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> * [Visual Studio ile mantıksal uygulamalar oluşturma](../logic-apps/quickstart-create-logic-apps-with-visual-studio.md)