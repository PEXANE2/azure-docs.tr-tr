---
title: Power Automate'den Azure Logic Apps'a dışa aktarma akışları
description: Azure Kaynak Yöneticisi şablonları olarak dışa aktararak Power Automate'den Azure Logic Apps'a akışları geçirin
services: logic-apps
ms.suite: integration
ms.reviewer: klam, logicappspm
ms.topic: conceptual
ms.date: 07/10/2019
ms.openlocfilehash: 616f10b32d0a9c1a05d759a0e27550cd2808808b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75428877"
---
# <a name="export-flows-from-power-automate-and-deploy-to-azure-logic-apps"></a>Akışları Power Automate'ten dışarı aktarma ve Azure Logic Apps’e dağıtma

Akışınızın yeteneklerini genişletmek ve genişletmek için, bu akışı [Power Automate'den](https://flow.microsoft.com) [Azure Logic Apps'a](../logic-apps/logic-apps-overview.md)geçirebilirsiniz. Akışınızı bir mantık uygulaması için Azure Kaynak Yöneticisi şablonu olarak dışa aktarabilir, bu mantık uygulama şablonunu bir Azure kaynak grubuna dağıtabilir ve ardından Mantık Uygulama Tasarımcısı'nda bu mantık uygulamasını açabilirsiniz.

> [!NOTE]
> Tüm Power Otomatikleştirme konektörleri Azure Logic Apps'ta kullanılamaz. Azure Logic Apps'ta [eşdeğer bağlayıcılara](../connectors/apis-list.md) sahip akışları içe aktarabilirsiniz. Örneğin, Düğme tetikleyicisi, Onay bağlayıcısı ve Bildirim bağlayıcısı Power Automate'e özgür.
>
> Power Automate'den dışa aktarılan OpenAPI tabanlı akışlar şu anda mantıksal uygulama şablonları olarak dağıtım için desteklenmemektedir. 

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği. Azure aboneliğiniz yoksa [ücretsiz bir Azure hesabı için kaydolun](https://azure.microsoft.com/free/).

* Power Automate'den aktarmak istediğiniz akış

## <a name="export-a-flow"></a>Akışı dışarı aktarma

1. [Power Automate'de](https://flow.microsoft.com)oturum açın ve **akışlarımı**seçin. Akışınızı bulun ve seçin. Araç çubuğunda elips (**...**) düğmesini seçin. **Dışa Aktarma** > **Mantığı Uygulamaları şablonu (.json) seçeneğini belirleyin.**

   ![Akışı dışarı aktarma](./media/export-from-microsoft-flow-logic-app-template/export-flow.png)

1. Şablonunuzu istediğiniz konuma kaydedin.

Daha fazla bilgi için [bkz.](https://flow.microsoft.com/blog/grow-up-to-logic-apps/)

## <a name="deploy-template-by-using-the-azure-portal"></a>Azure portalını kullanarak şablonu dağıtma

1. Azure hesabınızla [Azure portalında](https://portal.azure.com) oturum açın.

1. Azure ana menüsünde **Kaynak oluştur**'u seçin. Arama kutusuna "şablon dağıtımı" girin. **Şablon dağıtımı (özel şablonları kullanarak dağıt)** seçeneğini belirleyin ve ardından **Oluştur'u**seçin.

   !["Şablon dağıtımı" seçeneğini belirleyin](./media/export-from-microsoft-flow-logic-app-template/select-template-deployment.png)

1. **Özel dağıtım**altında, **düzenleyicide kendi şablonunuzu oluştur'u**seçin.

   !["Editörde kendi şablonunuzu oluşturun" seçeneğini belirleyin](./media/export-from-microsoft-flow-logic-app-template/build-template-in-editor.png)

1. **Şablonu Edit** araç çubuğundan **Dosyayı Yükle'yi**seçin. Power Automate'den dışa aktardığınız JSON şablonunu bulun ve seçin ve **Aç'ı**seçin.

   !["Dosyayı yükle" seçeneğini belirleyin](./media/export-from-microsoft-flow-logic-app-template/load-file.png)

1. Düzenleyici şablonunuzda JSON,parametreleri ve kaynakları gösterdiğinde **Kaydet'i**seçin.
  
   ![Şablonu kaydetme](./media/export-from-microsoft-flow-logic-app-template/save-template.png)

1. Şimdi şablon için bu giriş parametrelerini belirtin:

   * Faturalandırma için kullanılacak Azure aboneliği
   * Azure kaynak grubu
   * Azure kaynak grubu için konum
   * Mantık uygulaması kaynağının adı
   * Azure kaynak grubundan farklıysa, mantıksal uygulama kaynağının konumu
   * Mantık uygulamasının yeniden kullanabileceği daha önce oluşturulmuş bağlantıların adı

      İlk mantık uygulamanızı oluşturuyorsanız, varsayılan adları kabul etmek için tüm bağlantılar yeni olarak oluşturulur. Aksi takdirde, birden çok mantık uygulamasında kullanabileceğiniz daha önce oluşturulmuş bağlantıların adlarını belirtebilirsiniz.

   Şablon için bu bilgileri sağladıktan sonra, gerekli Azure kaynaklarını oluşturmak ve Azure aboneliğinizi buna göre faturalandırmak için Azure Pazar Koşulları ve Koşullarını gözden geçirin ve kabul edin ve ardından **Satın Alma'yı**seçin.
  
   ![Şablon için giriş parametrelerini belirtin](./media/export-from-microsoft-flow-logic-app-template/template-input-parameters.png)

   Azure, şablonunuzu belirtilen kaynak grubunuza mantık uygulaması olarak dağıtır. Power Automate'den geçirdiğiniz tüm mantık uygulamaları devre dışı bırakılmış durumda dağıtılır.

1. Mantık uygulamanızı etkinleştirmeden önce aşağıdaki adımları izleyerek yeni bağlantıları yetkilendirmeyin:

   1. Oluşturduğunuz mantık uygulamasını açın. Mantık uygulamasının menüsünde **Mantık uygulaması tasarımcısını**seçin.

      Yetkilendirme gerektiren her bağlantı bir uyarı simgesi gösterir:

      ![Uyarı simgesi](./media/export-from-microsoft-flow-logic-app-template/authorize-connections.png)

   1. Yetkili bağlantı gerektiren her adım için, bu adımı genişletin ve **yeni ekle'yi**seçin.

      ![Yeni bağlantı ekleme](./media/export-from-microsoft-flow-logic-app-template/add-new-connection.png)

   1. Her hizmette oturum açın veya bağlantıyı yetkilendirmek için gerekli kimlik bilgilerini sağlayın.

1. Mantıksal uygulamanızı kaydedin. Mantık uygulamanızı etkinleştirmeye hazır olduğunuzda, mantık uygulamasının menüsünde **Genel Bakış'ı**seçin ve ardından **Etkinleştir'i**seçin.

   ![Mantık uygulamasını etkinleştirme](./media/export-from-microsoft-flow-logic-app-template/enable-logic-app.png)

1. Yinelenen iş akışlarını çalıştırmayı önlemek için, özgün akışınızı devre dışı bıraktığınızdan veya sildiğinizden emin olun.

## <a name="deploy-template-by-using-visual-studio"></a>Visual Studio'u kullanarak şablonu dağıtma

Visual Studio'yu mantıksal uygulamalar oluşturmak için [ön koşullarla](../logic-apps/quickstart-create-logic-apps-with-visual-studio.md#prerequisites) ayarladıysanız, dışa aktarılan şablonunuzu Visual Studio'dan Azure Logic Apps'a dağıtabilirsiniz.

1. Visual Studio'da Power Automate'den dışa aktardığınız şablon dosyasını açın.

1. Visual Studio'da bir Azure Kaynak Grubu projesi oluşturun ve Quickstart'taki adımları izleyerek **Mantık Uygulaması** şablonunu [seçin: Azure Logic Apps - Visual Studio ile otomatik görevler, süreçler ve iş akışları oluşturun,](../logic-apps/quickstart-create-logic-apps-with-visual-studio.md)örneğin:

   ![Azure kaynak grubu projesi oluşturma](./media/export-from-microsoft-flow-logic-app-template/create-azure-resource-group-project.png)

1. Solution Explorer'dan, dosya zaten açık değilse **LogicApp.json** dosyasını açın.

1. Dışa aktarılan şablondaki içeriği kopyalayın ve **LogicApp.json** dosyasındaki içeriğin üzerine yazın.

1. Mantık uygulamanızı dağıtmadan önce aşağıdaki adımları izleyerek yeni bağlantıları yetkilendirmeyin:

   1. **LogicApp.json** kısayol menüsünü açın ve ardından **Mantık Uygulaması Tasarımcısı ile Aç'ı**seçin.

      ![Logic App Designer ile şablonu açın](./media/export-from-microsoft-flow-logic-app-template/open-logic-app-designer.png)

   1. İstenirse, mantık uygulamanızı dağıtmak için kullanmak istediğiniz Azure aboneliğini ve kaynak grubunu seçin.

      ![Azure aboneliği ni ve kaynak grubunu seçin](./media/export-from-microsoft-flow-logic-app-template/select-azure-subscription-resource-group-deployment.png)

      Mantık uygulamanız tasarımcıda göründükten sonra, yetkilendirme gerektiren tüm bağlantılar uyarı simgelerini gösterir:

      ![Uyarı simgeleri olan bağlantılar](./media/export-from-microsoft-flow-logic-app-template/authorize-connections-vs.png)

   1. Yetkili bağlantı gerektiren her adım için, bu adımı genişletin ve **yeni ekle'yi**seçin.

      ![Yeni bağlantı ekleme](./media/export-from-microsoft-flow-logic-app-template/add-new-connection-vs.png)

   1. Her hizmette oturum açın veya bağlantıyı yetkilendirmek için gerekli kimlik bilgilerini sağlayın.

   1. Mantık uygulamasını dağıtmadan önce çözümünüzü kaydedin.

1. Çözüm Gezgini'nde proje kısayolu menüsünü açın ve**Yeni** **Dağıt'ı** > seçin. Sorulursa Azure hesabınızla oturum açın.

1. İstendiğinde, Azure aboneliğini, Azure kaynak grubunu ve şablon parametre değerlerini geçirmek için kullanılacak [bir parametre dosyası](../azure-resource-manager/templates/parameter-files.md) gibi dağıtım için kullanmak istediğiniz diğer ayarları onaylayın ve ardından **Dağıt'ı**seçin.

   ![Dağıtım ayarlarını onaylama](./media/export-from-microsoft-flow-logic-app-template/confirm-azure-subscription-resource-group-deployment.png)

1. **Parametreleri Edit** kutusu görünüyorsa, Azure'daki mantık uygulama kaynağınızın adını sağlayın ve **Kaydet'i**seçin.  

   ![Dağıtım parametrelerini edin](./media/export-from-microsoft-flow-logic-app-template/edit-parameters-deployment.png)

   Dağıtım başladığında uygulamanızın dağıtım durumu Visual Studio **Çıktı** penceresinde görünür. Durum görünmezse **Çıktıyı göster** listesini açıp Azure kaynak grubunuzu seçin. Örnek:

   ![Çıktı penceresi](./media/export-from-microsoft-flow-logic-app-template/output-window.png)

   Mantık uygulamanızdaki herhangi bir bağlantının sizden girişe ihtiyacı varsa, arka planda bir PowerShell penceresi açılır ve gerekli parolalar veya gizli anahtarlar için istekte. Bu bilgileri girdikten sonra dağıtım işlemi devam eder.

   ![Bağlantıları doğrulat](./media/export-from-microsoft-flow-logic-app-template/logic-apps-powershell-window.png)

   Dağıtım bittikten sonra mantık uygulamanız yayımlanır, ancak Azure portalında etkinleştirilmez.

1. Mantıksal uygulamanızı Azure portalında etkinleştirmeye hazır olduğunuzda, Mantık Uygulama Tasarımcısı'nda mantık uygulamanızı bulun ve açın. Mantık uygulamasının menüsünde **Genel Bakış'ı**seçin ve ardından **Etkinleştir'i**seçin.

1. Yinelenen iş akışlarını çalıştırmayı önlemek için, özgün akışınızı devre dışı bıraktığınızdan veya sildiğinizden emin olun.

Bu dağıtım adımları hakkında daha fazla bilgi için [Bkz. Hızlı Başlangıç: Azure Logic Apps - Visual Studio ile otomatik görevler, süreçler ve iş akışları oluşturun](../logic-apps/quickstart-create-logic-apps-with-visual-studio.md#deploy-to-Azure)

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Mantık Uygulamaları için Bağlayıcılar](../connectors/apis-list.md) hakkında daha fazla bilgi edinin
* [Azure Mantık Uygulamaları](../logic-apps/logic-apps-overview.md) hakkında daha fazla bilgi edinin
