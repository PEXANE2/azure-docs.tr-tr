---
title: Power otomatikleştirmede akışları Azure Logic Apps dışa aktarma
description: Azure Resource Manager şablonları olarak dışa aktararak Power otomatikleştirmede akışları Azure Logic Apps geçirme
services: logic-apps
ms.suite: integration
ms.reviewer: jonfan, logicappspm
ms.topic: conceptual
ms.date: 07/10/2019
ms.openlocfilehash: b30a2ae8d90a193e23229dc6743c7e92ebf83b52
ms.sourcegitcommit: d118ad4fb2b66c759b70d4d8a18e6368760da3ad
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84298729"
---
# <a name="export-flows-from-power-automate-and-deploy-to-azure-logic-apps"></a>Akışları Power Automate'ten dışarı aktarma ve Azure Logic Apps’e dağıtma

Akışınızın yeteneklerini genişletmek ve genişletmek için, bu akışı [Power otomatikleştirmede](https://flow.microsoft.com) [Azure Logic Apps](../logic-apps/logic-apps-overview.md)geçirebilirsiniz. Akışınızı bir mantıksal uygulama için bir Azure Resource Manager şablonu olarak dışa aktarabilir, bu mantıksal uygulama şablonunu bir Azure Kaynak grubuna dağıtabilir ve ardından mantıksal uygulama tasarımcısında bu mantıksal uygulamayı açabilirsiniz.

> [!NOTE]
> Azure Logic Apps ' de tüm güç otomatikleştirme bağlayıcıları kullanılamaz. Yalnızca Azure Logic Apps içindeki eşit bağlayıcılara sahip güç otomatikleştirme akışlarını geçirebilirsiniz. Örneğin, düğme tetikleyicisi, onay Bağlayıcısı ve bildirim Bağlayıcısı, Power otomatikleştirmek için özeldir. Şu anda, mantıksal uygulama şablonları olarak dışa aktarma ve dağıtım için Power otomatikleştirmede OpenAPI tabanlı akışlar desteklenmez.
>
> * Hangi Power otomatikleştirmek bağlayıcılarının Logic Apps eşdeğerleri olmadığını öğrenmek için bkz. [Power otomatikleştiren bağlayıcılar](https://docs.microsoft.com/connectors/connector-reference/connector-reference-powerautomate-connectors).
>
> * Hangi Logic Apps bağlayıcılarının güç otomatikleştirme eşdeğerleri olmadığını öğrenmek için bkz. [Logic Apps bağlayıcılar](https://docs.microsoft.com/connectors/connector-reference/connector-reference-powerautomate-connectors).

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği. Azure aboneliğiniz yoksa [ücretsiz bir Azure hesabı için kaydolun](https://azure.microsoft.com/free/).

* Power otomatikleştirmede dışarı aktarmak istediğiniz akış

## <a name="export-a-flow"></a>Akışı dışarı aktarma

1. [Power otomatikleştirmek](https://flow.microsoft.com)için oturum açın ve **Akışlarım**' ı seçin. Akışınızı bulun ve seçin. Araç çubuğunda üç nokta (**...**) düğmesini seçin. Logic Apps şablonunu **dışarı aktar**  >  **(. JSON)** seçeneğini belirleyin.

   ![Akışı dışarı aktarma](./media/export-from-microsoft-flow-logic-app-template/export-flow.png)

1. Şablonunuzu istediğiniz konuma kaydedin.

Daha fazla bilgi için bkz. [Azure Logic Apps büyütme](https://flow.microsoft.com/blog/grow-up-to-logic-apps/).

## <a name="deploy-template-by-using-the-azure-portal"></a>Azure portal kullanarak şablonu dağıtma

1. [Azure Portal](https://portal.azure.com) Azure hesabınızla oturum açın.

1. Azure ana menüsünde **Kaynak oluştur**'u seçin. Arama kutusuna "şablon dağıtımı" yazın. **Şablon dağıtımı (özel şablonlar kullanarak dağıtın)** öğesini seçin ve ardından **Oluştur**' u seçin.

   !["Şablon dağıtımı" öğesini seçin](./media/export-from-microsoft-flow-logic-app-template/select-template-deployment.png)

1. **Özel dağıtım**altında, **düzenleyicide kendi şablonunuzu oluştur**' u seçin.

   !["Kendi şablonunuzu düzenleyicide oluşturun" seçeneğini belirleyin](./media/export-from-microsoft-flow-logic-app-template/build-template-in-editor.png)

1. **Şablonu Düzenle** araç çubuğundan **Dosya Yükle**' yi seçin. Power otomatikleştirmede verdiğiniz JSON şablonunu bulun ve seçin ve **Aç**' ı seçin.

   !["Dosya yükle" yi seçin](./media/export-from-microsoft-flow-logic-app-template/load-file.png)

1. Düzenleyici, şablonunuzda JSON, parametreler ve kaynakları görüntülendikten sonra **Kaydet**' i seçin.
  
   ![Şablonu kaydetme](./media/export-from-microsoft-flow-logic-app-template/save-template.png)

1. Şimdi şablon için bu giriş parametrelerini belirtin:

   * Faturalandırma için kullanılacak Azure aboneliği
   * Azure kaynak grubu
   * Azure Kaynak grubu için konum
   * Mantıksal uygulama kaynağı adı
   * Mantıksal uygulama kaynağı için Azure kaynak grubundan farklıysa konum
   * Mantıksal uygulamanın yeniden kullanabilmesi için önceden oluşturulmuş olan tüm bağlantıların adı

      İlk mantıksal uygulamanızı oluşturuyorsanız, tüm bağlantılar yeni olarak oluşturulur, böylece varsayılan adları kabul edebilirsiniz. Aksi takdirde, daha önce oluşturulan bağlantıların adlarını belirtebilirsiniz. Bu, birden çok Logic Apps genelinde kullanabileceğiniz bir ad olabilir.

   Bu bilgileri şablon için sağlamadıktan sonra, gerekli Azure kaynaklarını oluşturma ve Azure aboneliğinizi buna göre faturalama için Azure Marketi hüküm ve koşullarını gözden geçirip kabul edin ve **satın al**' ı seçin.
  
   ![Şablon için giriş parametrelerini belirtin](./media/export-from-microsoft-flow-logic-app-template/template-input-parameters.png)

   Azure, şablonunuzu belirtilen kaynak grubunuza bir mantıksal uygulama olarak dağıtır. Güç otomatikleştirmede geçiş yaptığınız tüm mantıksal uygulamalar devre dışı durumda dağıtılır.

1. Mantıksal uygulamanızı etkinleştirmeden önce aşağıdaki adımları izleyerek herhangi bir yeni bağlantı yetkisini verin:

   1. Oluşturduğunuz mantıksal uygulamayı açın. Mantıksal uygulama menüsünde **mantıksal uygulama Tasarımcısı**' nı seçin.

      Yetkilendirme gerektiren her bağlantı bir uyarı simgesi gösterir:

      ![Uyarı simgesi](./media/export-from-microsoft-flow-logic-app-template/authorize-connections.png)

   1. Yetkili bağlantı gerektiren her adım için bu adımı genişletin ve **Yeni Ekle**' yi seçin.

      ![Yeni bağlantı ekle](./media/export-from-microsoft-flow-logic-app-template/add-new-connection.png)

   1. Her bir hizmette oturum açın veya bağlantıyı yetkilendirmek için gereken kimlik bilgilerini sağlayın.

1. Mantıksal uygulamanızı kaydedin. Mantıksal uygulamanızı etkinleştirmeye hazırsanız, mantıksal uygulama menüsünde **genel bakış**' ı seçin ve ardından **Etkinleştir**' i seçin.

   ![Mantıksal uygulamayı etkinleştir](./media/export-from-microsoft-flow-logic-app-template/enable-logic-app.png)

1. Yinelenen iş akışları çalıştırmanın önüne geçmek için özgün akışınızı devre dışı bıraktığınızdan veya sildiğinizden emin olun.

## <a name="deploy-template-by-using-visual-studio"></a>Visual Studio kullanarak şablonu dağıtma

Mantıksal uygulamalar oluşturmaya yönelik [önkoşullara](../logic-apps/quickstart-create-logic-apps-with-visual-studio.md#prerequisites) sahip Visual Studio 'yu ayarladıysanız, Visual studio 'dan Azure Logic Apps 'e, verdiğiniz şablonu dağıtabilirsiniz.

1. Visual Studio 'da, Power otomatikleştirmede verdiğiniz şablon dosyasını açın.

1. Visual Studio 'da bir Azure Kaynak grubu projesi oluşturun ve [hızlı başlangıç: Azure Logic Apps Visual Studio ile otomatik görevler, işlemler ve iş akışları oluşturma](../logic-apps/quickstart-create-logic-apps-with-visual-studio.md)' daki adımları Izleyerek **Logic App** şablonunu seçin. Örneğin:

   ![Azure kaynak grubu projesi oluşturma](./media/export-from-microsoft-flow-logic-app-template/create-azure-resource-group-project.png)

1. Çözüm Gezgini, dosya zaten açık değilse **Logicapp. JSON** dosyasını açın.

1. İçe aktarılmış şablondaki içeriği kopyalayın ve **Logicapp. JSON** dosyasındaki içeriğin üzerine yazın.

1. Mantıksal uygulamanızı dağıtmadan önce, aşağıdaki adımları izleyerek herhangi bir yeni bağlantı yetkisini verin:

   1. **Logicapp. JSON** kısayol menüsünü açın ve ardından **mantıksal uygulama Tasarımcısı ile aç**' ı seçin.

      ![Mantıksal uygulama Tasarımcısı ile şablon açma](./media/export-from-microsoft-flow-logic-app-template/open-logic-app-designer.png)

   1. İstenirse, mantıksal uygulamanızı dağıtmak için kullanmak istediğiniz Azure aboneliğini ve kaynak grubunu seçin.

      ![Azure aboneliğini ve kaynak grubunu seçin](./media/export-from-microsoft-flow-logic-app-template/select-azure-subscription-resource-group-deployment.png)

      Mantıksal uygulamanız tasarımcıda görüntülendikten sonra, yetkilendirme gerektiren tüm bağlantılar uyarı simgeleri gösterir:

      ![Uyarı simgeleri olan bağlantılar](./media/export-from-microsoft-flow-logic-app-template/authorize-connections-vs.png)

   1. Yetkili bağlantı gerektiren her adım için bu adımı genişletin ve **Yeni Ekle**' yi seçin.

      ![Yeni bağlantı ekle](./media/export-from-microsoft-flow-logic-app-template/add-new-connection-vs.png)

   1. Her bir hizmette oturum açın veya bağlantıyı yetkilendirmek için gereken kimlik bilgilerini sağlayın.

   1. Mantıksal uygulamayı dağıtmadan önce çözümünüzü kaydedin.

1. Çözüm Gezgini menüsünde, proje kısayol menüsünü açın ve yeni **Dağıt**' ı seçin  >  **New**. Sorulursa Azure hesabınızla oturum açın.

1. İstendiğinde, Azure aboneliğini, Azure kaynak grubunu ve dağıtım için kullanmak istediğiniz diğer ayarları (şablon parametre değerlerini iletmek için kullanılacak [Parametreler dosyası](../azure-resource-manager/templates/parameter-files.md) gibi) onaylayın ve **Dağıt**' ı seçin.

   ![Dağıtım ayarlarını Onayla](./media/export-from-microsoft-flow-logic-app-template/confirm-azure-subscription-resource-group-deployment.png)

1. **Parametreleri Düzenle** kutusu görüntülenirse, mantıksal uygulama kaynağınızın adını Azure 'da belirtin ve **Kaydet**' i seçin.  

   ![Dağıtım parametrelerini Düzenle](./media/export-from-microsoft-flow-logic-app-template/edit-parameters-deployment.png)

   Dağıtım başladığında uygulamanızın dağıtım durumu Visual Studio **Çıktı** penceresinde görünür. Durum görünmezse **Çıktıyı göster** listesini açıp Azure kaynak grubunuzu seçin. Örneğin:

   ![Çıktı penceresi](./media/export-from-microsoft-flow-logic-app-template/output-window.png)

   Mantıksal uygulamanızdaki herhangi bir bağlantının sizin için giriş yapması gerekiyorsa, arka planda bir PowerShell penceresi açılır ve gerekli parolaların veya gizli anahtarların istenir. Bu bilgileri girdikten sonra dağıtım işlemi devam eder.

   ![Kimlik doğrulama bağlantıları](./media/export-from-microsoft-flow-logic-app-template/logic-apps-powershell-window.png)

   Dağıtım bittikten sonra mantıksal uygulamanız yayımlanır ancak Azure portal etkinleştirilmez.

1. Mantıksal uygulamanızı Azure portal etkinleştirmeye hazırsanız mantıksal uygulama tasarımcısında mantıksal uygulamanızı bulun ve açın. Mantıksal uygulama menüsünde **genel bakış**' ı seçin ve ardından **Etkinleştir**' i seçin.

1. Yinelenen iş akışları çalıştırmanın önüne geçmek için özgün akışınızı devre dışı bıraktığınızdan veya sildiğinizden emin olun.

Bu dağıtım adımları hakkında daha fazla bilgi için bkz [. hızlı başlangıç: Azure Logic Apps otomatikleştirilmiş görevler, işlemler ve iş akışları oluşturma-Visual Studio](../logic-apps/quickstart-create-logic-apps-with-visual-studio.md#deploy-to-Azure)

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Logic Apps Için bağlayıcılar](../connectors/apis-list.md) hakkında daha fazla bilgi edinin
* [Azure Logic Apps](../logic-apps/logic-apps-overview.md) hakkında daha fazla bilgi edinin
