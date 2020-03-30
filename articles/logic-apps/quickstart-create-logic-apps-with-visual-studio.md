---
title: Visual Studio'daki görev iş akışlarını otomatikleştirin
description: Azure Logic Apps ve Visual Studio'u kullanarak kurumsal tümleştirme için yinelenen iş akışları oluşturun, zamanlama ve çalıştırın
services: logic-apps
ms.suite: integration
ms.reviewer: klam, logicappspm
ms.topic: quickstart
ms.custom: mvc
ms.date: 11/08/2019
ms.openlocfilehash: 693b2fd8ac7440b67f53de0aedb9a8268a90de76
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "79241657"
---
# <a name="quickstart-create-automated-tasks-processes-and-workflows-with-azure-logic-apps---visual-studio"></a>Hızlı başlangıç: Azure Logic Apps ile otomatik görevler, süreçler ve iş akışları oluşturun - Visual Studio

[Azure Logic Apps](../logic-apps/logic-apps-overview.md) ve Visual Studio ile uygulama, veri, sistem ve hizmetleri kurum ve kuruluşlar arasında otomatik hale getiren iş akışları oluşturabilirsiniz. Bu hızlı başlangıç, Visual Studio'da mantıksal uygulamalar oluşturarak ve bu uygulamaları Azure'a dağıtarak bu iş akışlarını nasıl tasarlayıp oluşturabileceğinizi gösterir. Bu görevleri Azure portalında gerçekleştirebiliyor olsanız da Visual Studio, mantık uygulamalarınızı kaynak denetimine eklemenize, farklı sürümler yayımlamanıza ve farklı dağıtım ortamları için Azure Kaynak Yöneticisi şablonları oluşturmanıza olanak tanır.

Azure Mantık Uygulamaları'nda yeniyseniz ve yalnızca temel kavramları istiyorsanız, [Azure portalında bir mantık uygulaması oluşturmak için hızlı başlatmayı](../logic-apps/quickstart-create-first-logic-app-workflow.md)deneyin. Logic App Designer hem Azure portalında hem de Visual Studio'da benzer şekilde çalışır.

Bu hızlı başlangıçta, Visual Studio ile Azure portalı ile aynı mantık uygulamasını oluşturursunuz. Bu mantık uygulaması bir web sitesinin RSS akışını izler ve bu akıştaki her yeni öğe için e-posta gönderir. Bitmiş mantık uygulamanız bu üst düzey iş akışına benzer:

![Tamamlanmış mantıksal uygulama](./media/quickstart-create-logic-apps-with-visual-studio/high-level-workflow-overview.png)

<a name="prerequisites"></a>

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği. Aboneliğiniz yoksa, [ücretsiz bir Azure hesabı için kaydolun](https://azure.microsoft.com/free/).

* Henüz yoksa şu araçları indirip yükleyin:

  * [Visual Studio 2019, 2017 veya 2015 - Topluluk sürümü veya daha büyük](https://aka.ms/download-visual-studio). Bu hızlı başlangıç Visual Studio Community 2017'yi kullanır.

    > [!IMPORTANT]
    > Visual Studio 2019 veya 2017'yi yüklediğinizde, **Azure geliştirme** iş yükünü seçtiğinizden emin olun.

  * [.NET için Microsoft Azure SDK (2.9.1 veya sonrası)](https://azure.microsoft.com/downloads/). [Azure SDK for .NET](https://docs.microsoft.com/dotnet/azure/dotnet-tools?view=azure-dotnet) hakkında daha fazla bilgi edinin.

  * [Azure PowerShell](https://github.com/Azure/azure-powershell#installation)

  * İstediğindiğiniz sürüm için Visual Studio uzantısı için en son Azure Mantık Uygulamaları Araçları:

    * [Visual Studio 2019](https://aka.ms/download-azure-logic-apps-tools-visual-studio-2019)

    * [Visual Studio 2017](https://aka.ms/download-azure-logic-apps-tools-visual-studio-2017)

    * [Visual Studio 2015](https://aka.ms/download-azure-logic-apps-tools-visual-studio-2015)
  
    Azure Logic Apps Araçlarını doğrudan Visual Studio Market’ten indirip yükleyebilir veya [bu uzantıyı Visual Studio’nun içinden yükleme](https://docs.microsoft.com/visualstudio/ide/finding-and-using-visual-studio-extensions) hakkında bilgi edinebilirsiniz. Yükleme işlemini tamamladıktan sonra Visual Studio’yu yeniden başlattığınızdan emin olun.

* Ekli Mantıksal Uygulama Tasarımcısı kullanılırken web erişimi

  Tasarımcının Azure'da kaynak oluşturmak ve mantık uygulamanızdaki bağlayıcılardan gelen özellikleri ve verileri okumak için bir internet bağlantısına ihtiyacı vardır. Örneğin, Dynamics CRM Online bağlantıları için tasarımcı CRM örneğinizin varsayılan ve özel özellikleri için denetler.

* Logic Apps tarafından desteklenen Office 365 Outlook, Outlook.com veya Gmail gibi bir e-posta hesabı. Diğer [sağlayıcılar için, bağlayıcılar listesini buradan](https://docs.microsoft.com/connectors/)gözden geçirin. Bu örnek, Office 365 Outlook kullanır. Farklı bir sağlayıcı kullanıyorsanız genel adımlar aynıdır, ancak kullanıcı arabirimi biraz farklı olabilir.

<a name="create-resource-group-project"></a>

## <a name="create-azure-resource-group-project"></a>Azure kaynak grubu projesi oluşturma

Başlamak için bir [Azure Kaynak Grubu projesi](../azure-resource-manager/templates/create-visual-studio-deployment-project.md) oluşturun. [Azure kaynak grupları ve kaynakları](../azure-resource-manager/management/overview.md) hakkında daha fazla bilgi edinin.

1. Visual Studio’yu çalıştırın. Azure hesabınızla oturum açın.

1. **Dosya** menüsünde **Yeni** > **Proje**’yi seçin. (Klavye: Ctrl + Shift + N)

   !["Dosya" menüsünde "Yeni" > "Proje" öğesini seçin](./media/quickstart-create-logic-apps-with-visual-studio/create-new-visual-studio-project.png)

1. **Yüklü** altında **Visual C#** veya **Visual Basic**’i seçin. **Bulut** > **Azure Kaynak Grubu'ni**seçin. Projenizi adlandırın, örneğin:

   ![Azure Kaynak Grubu projesi oluşturma](./media/quickstart-create-logic-apps-with-visual-studio/create-azure-cloud-service-project.png)

   > [!NOTE]
   > Kaynak grup adları yalnızca harfleri,`.`sayıları, dönemleri ( ),`-`alt çizer`(`( `)``_`), tire ( ),`.`parantez ( ), parantez ( ), , ( ) ile son ana kadar *devam* edemez ( ).
   >
   > **Bulut** veya **Azure Kaynak Grubu** görünmüyorsa, Visual Studio için Azure SDK'sını yüklediğinizden emin olun.

   Visual Studio 2019 kullanıyorsanız aşağıdaki adımları izleyin:

   1. Yeni **proje oluştur** kutusunda, Visual C# veya Visual Basic için **Azure Kaynak Grubu** projesini seçin. **Sonraki'ni**seçin.

   1. Kullanmak istediğiniz Azure kaynak grubu ve diğer proje bilgileri için bir ad sağlayın. **Oluştur'u**seçin.

1. Şablon listesinden Mantık **Uygulaması** şablonu'nu seçin. **Tamam'ı**seçin.

   ![Mantıksal Uygulama şablonunu seçme](./media/quickstart-create-logic-apps-with-visual-studio/select-logic-app-template.png)

   Visual Studio projenizi oluşturduktan sonra, Çözüm Gezgini açılır ve çözümünüzü gösterir. Çözümünüzde, **LogicApp.json** dosyası yalnızca mantık uygulama tanımınızı de saklar, aynı zamanda dağıtım için kullanabileceğiniz bir Azure Kaynak Yöneticisi şablonudur.

   ![Çözüm Gezgini yeni mantıksal uygulama çözümünü ve dağıtım dosyasını gösterir](./media/quickstart-create-logic-apps-with-visual-studio/logic-app-solution-created.png)

## <a name="create-blank-logic-app"></a>Boş mantıksal uygulama oluşturma

Azure Kaynak Grubu projeniz olduğunda, **Boş Mantık Uygulaması** şablonuyla mantık uygulamanızı oluşturun.

1. Solution Explorer'da **LogicApp.json** dosyasının kısayol menüsünü açın. **Mantıksal Uygulama Tasarımcısı ile Aç**’ı seçin. (Klavye: Ctrl + L)

   ![Mantıksal Uygulama Tasarımcısı ile mantıksal uygulama .json dosyasını açma](./media/quickstart-create-logic-apps-with-visual-studio/open-logic-app-designer.png)

   > [!TIP]
   > Visual Studio 2019'da bu komuta sahip değilseniz, Visual Studio için en son güncelleştirmelerin olup olmadığını kontrol edin.

   Visual Studio, mantıksal uygulamanız ve bağlantılarınız için kaynak oluşturmanız ve dağıtmanız için Azure aboneliğinizi ve Azure kaynak grubunu ister.

1. **Abonelik**için Azure aboneliğinizi seçin. **Kaynak grubu**için, başka bir Azure kaynak grubu oluşturmak için Yeni Oluştur'u'nun **Create New**

   ![Azur aboneliği, kaynak grubu ve kaynak konumu seçme](./media/quickstart-create-logic-apps-with-visual-studio/select-azure-subscription-resource-group-location.png)

   | Ayar | Örnek değer | Açıklama |
   | ------- | ------------- | ----------- |
   | Kullanıcı hesabı | Fabrikam <br> sophia-owen@fabrikam.com | Visual Studio'ya oturum açken kullandığınız hesap |
   | **Abonelik** | Kullandıkça Öde <br> (sophia-owen@fabrikam.com) | Azure aboneliğinizin ve ilişkili hesabın adı |
   | **Kaynak Grubu** | MyLogicApp-RG <br> (Batı ABD) | Mantıksal uygulamanızın kaynaklarını depolamak ve dağıtmak için Azure kaynak grubu ve konumu |
   | **Konum** | **Kaynak Grubu ile aynı** | Mantık uygulamanızı dağıtmak için konum türü ve belirli bir konum. Konum türü, Azure bölgesi veya varolan bir [tümleştirme hizmeti ortamıdır (ISE)](connect-virtual-network-vnet-isolated-environment.md)olur. <p>Bu hızlı başlangıç için, konum **Region** türünü Bölge'ye, konum kümesini de **Kaynak Grubu olarak aynı**olarak ayarlayın. <p>**Not**: Kaynak grubu projenizi oluşturduktan [sonra, konum türünü ve konumunu değiştirebilirsiniz,](manage-logic-apps-with-visual-studio.md#change-location)ancak farklı konum türü mantık uygulamanızı çeşitli şekillerde etkiler. |
   ||||

1. Logic Apps Tasarımcısı, bir giriş videosu ve yaygın olarak kullanılan tetikleyicileri gösteren bir sayfa açar. Videoyu ve tetikleyicileri **şablonlara**kaydırın ve **Boş Mantık Uygulaması'nı**seçin.

   !["Boş Mantıksal Uygulama" seçme](./media/quickstart-create-logic-apps-with-visual-studio/choose-blank-logic-app-template.png)

## <a name="build-logic-app-workflow"></a>Mantıksal uygulama iş akışı derleme

Ardından, yeni bir akış öğesi göründüğünde devreye saçan bir RSS [tetikleyicisi](../logic-apps/logic-apps-overview.md#logic-app-concepts) ekleyin. Her mantık uygulaması, belirli ölçütler karşılandığında ateşlenen bir tetikleyiciyle başlar. Tetikleyici her etkinleştirildiğinde Logic Apps altyapısı iş akışınızı çalıştıran bir mantıksal uygulama örneği oluşturur.

1. Mantık Uygulama Tasarımcısı'nda, arama kutusunun altında **Tümü'nü**seçin. Arama kutusuna "rss" girin. Tetikleyiciler listesinden şu tetikleyiciyi seçin: **Bir akış öğesi yayımlandığında**

   ![Tetikleyici ve eylemler ekleyerek mantıksal uygulamanızı derleme](./media/quickstart-create-logic-apps-with-visual-studio/add-trigger-logic-app.png)

1. Tetikleyici tasarımcıda göründükten sonra, [Azure portalındaki](../logic-apps/quickstart-create-first-logic-app-workflow.md#add-rss-trigger)iş akışı adımlarını izleyerek mantık uygulamasını oluşturmayı tamamlayın ve bu makaleye geri dönün. İşiniz bittiğinde mantıksal uygulamanız şu örnekteki gibi görünür:

   ![Tamamlanmış mantıksal uygulama](./media/quickstart-create-logic-apps-with-visual-studio/finished-logic-app-workflow.png)

1. Visual Studio çözümünüzü kaydedin. (Klavye: Ctrl + S)

<a name="deploy-to-Azure"></a>

## <a name="deploy-logic-app-to-azure"></a>Mantıksal uygulamanızı Azure'a dağıtma

Mantık uygulamanızı çalıştırıp test etmeden önce uygulamayı Visual Studio'dan Azure'a dağıtın.

1. Çözüm Gezgini'nde, projenizin kısayol menüsünde**Yeni**Dağıt'ı'nı **Deploy** > seçin. Sorulursa Azure hesabınızla oturum açın.

   ![Mantıksal uygulama dağıtımı oluşturma](./media/quickstart-create-logic-apps-with-visual-studio/create-logic-app-deployment.png)

1. Bu dağıtım için varsayılan Azure aboneliğini, kaynak grubunu ve diğer ayarları saklayın. **Dağıt**'ı seçin.

   ![Mantıksal uygulamayı Azure kaynak grubuna dağıtma](./media/quickstart-create-logic-apps-with-visual-studio/select-azure-subscription-resource-group-deployment.png)

1. **Parametreleri Edit** kutusu görünüyorsa, mantık uygulamanız için bir kaynak adı sağlayın. Ayarlarınızı kaydedin.

   ![Mantıksal uygulama için dağıtım adı belirtme](./media/quickstart-create-logic-apps-with-visual-studio/edit-parameters-deployment.png)

   Dağıtım başladığında uygulamanızın dağıtım durumu Visual Studio **Çıktı** penceresinde görünür. Durum görünmezse **Çıktıyı göster** listesini açıp Azure kaynak grubunuzu seçin.

   ![Dağıtım durumu çıktısı](./media/quickstart-create-logic-apps-with-visual-studio/logic-app-output-window.png)

   Seçili bağlayıcılarınızın sizden girişe ihtiyacı varsa, arka planda bir PowerShell penceresi açılır ve gerekli parolalar veya gizli anahtarlar için istekte bulundu. Bu bilgileri girdikten sonra dağıtım işlemi devam eder.

   ![PowerShell penceresi](./media/quickstart-create-logic-apps-with-visual-studio/logic-apps-powershell-window.png)

   Dağıtım bittikten sonra, mantık uygulamanız Azure portalında canlı olarak yayınlanır ve belirtilen zamanlamanıza göre (her dakika) çalışır. Tetikleyici yeni besleme öğeleri bulursa, tetikleyici, mantık uygulamanızın eylemlerini çalıştıran bir iş akışı örneği oluşturan tetikleme devreleri. Mantık uygulamanız her yeni öğe için e-posta gönderir. Aksi takdirde, tetikleyici yeni öğeler bulamazsa, tetikleyici ateş etmez ve iş akışını anında "atlar". Mantık uygulamanız kontrol etmeden önce bir sonraki arayı bekler.

   İşte bu mantık uygulamasının gönderdiği örnek e-postalar. E-posta gelmezse istenmeyen e-posta klasörüne bakın.

   ![Outlook her yeni RSS öğesi için e-posta gönderir](./media/quickstart-create-logic-apps-with-visual-studio/outlook-email.png)

Tebrikler, Visual Studio ile mantık uygulamanızı başarıyla oluşturup dağıttınız. Mantıksal uygulamanızı yönetmek ve çalıştırma geçmişini gözden geçirmek için bkz. [Visual Studio ile mantıksal uygulamaları yönetme](../logic-apps/manage-logic-apps-with-visual-studio.md).

## <a name="add-new-logic-app"></a>Yeni mantık uygulaması ekle

Varolan bir Azure Kaynak Grubu projeniz varsa, JSON Anahat penceresini kullanarak bu projeye yeni bir boş mantık uygulaması ekleyebilirsiniz.

1. Çözüm Gezgini'nde `<logic-app-name>.json` dosyayı açın.

1. **Görünüm** menüsünden **Diğer Windows** > **JSON Anahattı'nı**seçin.

1. Şablon dosyasına kaynak eklemek için JSON Anahat penceresinin üst kısmında **Kaynak Ekle'yi** seçin. Veya JSON Anahat penceresinde, **kaynaklar** kısayol menüsünü açın ve **Yeni Kaynak Ekle'yi**seçin.

   ![JSON Anahat penceresi](./media/quickstart-create-logic-apps-with-visual-studio/json-outline-window-add-resource.png)

1. Kaynak **Ekle** iletişim kutusunda, arama kutusunda, `logic app` **Mantık Uygulaması'nı**bulun ve seçin. Mantık uygulamanızı adlandırın ve **Ekle'yi**seçin.

   ![Kaynak ekleme](./media/quickstart-create-logic-apps-with-visual-studio/add-logic-app-resource.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Mantık uygulamanızla işiniz bittiğinde, mantık uygulamanızı ve ilgili kaynaklarıiçeren kaynak grubunu silin.

1. Mantıksal uygulamanızı oluşturmak için kullandığınız hesapla [Azure portalında](https://portal.azure.com) oturum açın.

1. Azure portalı menüsünde **Kaynak gruplarını**seçin veya herhangi bir sayfadan **Kaynak gruplarını** arayın ve seçin. Mantık uygulamanızın kaynak grubunu seçin.

1. Genel **Bakış** sayfasında **kaynak grubunu sil'i**seçin. Kaynak grup adını onay olarak girin ve **Sil'i**seçin.

   !["Kaynak grupları" > "Genel bakış" > "Kaynak grubunu sil"](./media/quickstart-create-logic-apps-with-visual-studio/clean-up-resources.png)

1. Visual Studio çözümünü yerel bilgisayarınızdan silin.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede Visual Studio kullanarak mantıksal uygulamanızı derlediniz, dağıttınız ve çalıştırdınız. Visual Studio ile mantıksal uygulamalar için gelişmiş dağıtım yönetimi ve gerçekleştirme hakkında bilgi edinmek için şu makalelere bakın:

> [!div class="nextstepaction"]
> [Visual Studio ile mantıksal uygulamaları yönetme](../logic-apps/manage-logic-apps-with-visual-studio.md)
