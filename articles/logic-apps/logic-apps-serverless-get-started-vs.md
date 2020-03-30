---
title: Visual Studio'da ilk sunucusuz uygulamayı oluşturun
description: Visual Studio'da Azure Logic Apps ve Azure İşlevlerini kullanarak sunucusuz bir uygulama oluşturun, dağıtın ve yönetin
services: logic-apps
ms.suite: integration
ms.reviewer: klam, logicappspm
ms.topic: article
ms.date: 06/20/2019
ms.openlocfilehash: 2e27958dd9379a26ca7e7f4d7e427e5afa216e29
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75981158"
---
# <a name="build-your-first-serverless-app-by-using-azure-logic-apps-and-azure-functions-in-visual-studio"></a>Visual Studio'da Azure Logic Apps ve Azure Fonksiyonlarını kullanarak ilk sunucusuz uygulamanızı oluşturun

[Azure'daki Azure'daki](../logic-apps/logic-apps-overview.md) sunucusuz araçları ve yetenekleri kullanarak Bulut Uygulamalarını hızla geliştirebilir ve dağıtabilirsiniz( Azure Mantık Uygulamaları ve [Azure İşlevleri).](../azure-functions/functions-overview.md) Bu makalede, Visual Studio'da Azure işlevi çağıran bir mantık uygulaması kullanan sunucusuz bir uygulama oluşturmaya nasıl başlayacağız gösterilmektedir. Azure'da sunucusuz çözümler hakkında daha fazla bilgi edinmek [için, İşlevler ve Mantık Uygulamaları ile Azure Serverless'a](../logic-apps/logic-apps-serverless-overview.md)bakın.

## <a name="prerequisites"></a>Ön koşullar

Visual Studio'da sunucusuz bir uygulama oluşturmak için şunları yapmanız gerekir:

* Azure aboneliği. Azure aboneliğiniz yoksa [ücretsiz bir Azure hesabı için kaydolun](https://azure.microsoft.com/free/).

* Aşağıdaki araçlar. Zaten yoksa, indirin ve yükleyin.

  * [Visual Studio 2019, 2017 veya 2015 (Topluluk veya diğer sürüm)](https://aka.ms/download-visual-studio). 
  Bu hızlı başlangıçta ücretsiz olan Visual Studio Community 2017 kullanılmaktadır.

    > [!IMPORTANT]
    > Visual Studio 2019 veya 2017'yi yüklediğinizde, **Azure geliştirme** iş yükünü seçtiğinizden emin olun.

  * [.NET için Microsoft Azure SDK (sürüm 2.9.1 veya sonrası)](https://azure.microsoft.com/downloads/). 
  [Azure SDK for .NET](https://docs.microsoft.com/dotnet/azure/dotnet-tools?view=azure-dotnet) hakkında daha fazla bilgi edinin.

  * [Azure PowerShell](https://github.com/Azure/azure-powershell#installation).

  * İstediğindiğiniz Visual Studio sürümü için Azure Logic Apps Araçları:

    * [Visual Studio 2019](https://aka.ms/download-azure-logic-apps-tools-visual-studio-2019)

    * [Visual Studio 2017](https://aka.ms/download-azure-logic-apps-tools-visual-studio-2017)

    * [Visual Studio 2015](https://aka.ms/download-azure-logic-apps-tools-visual-studio-2015)
  
    Azure Logic Apps Araçlarını doğrudan Visual Studio Market’ten indirip yükleyebilir veya [bu uzantıyı Visual Studio’nun içinden yükleme](https://docs.microsoft.com/visualstudio/ide/finding-and-using-visual-studio-extensions) hakkında bilgi edinebilirsiniz. 
    Yükleme işlemini tamamladıktan sonra Visual Studio’yu yeniden başlattığınızdan emin olun.

  * Yerel hata ayıklama Işlevleri için [Azure İşlevler Çekirdek Araçları.](https://www.npmjs.com/package/azure-functions-core-tools)

* Gömülü Logic App Designer'ı kullanırken web'e erişim.

  Tasarımcının Azure'da kaynak oluşturması ve mantıksal uygulamanızdaki bağlayıcılardan özellik ve verileri okuması için İnternet bağlantısı gerekir. 
  Örneğin, Dynamics CRM Online bağlayıcısını kullanıyorsanız, tasarımcı CRM örneğinizdeki varsayılan ve özel kullanılabilir özellikleri denetler.

## <a name="create-a-resource-group-project"></a>Kaynak grubu projesi oluşturma

Başlamak için, sunucusuz uygulamanız için bir [Azure Kaynak Grubu projesi](../azure-resource-manager/templates/create-visual-studio-deployment-project.md) oluşturun. Azure'da, bir *uygulamanın*tamamının kaynaklarını tek bir varlık olarak düzenlemek, yönetmek ve dağıtmak için kullandığınız mantıksal bir koleksiyon olan bir kaynak grubunda kaynaklar oluşturursunuz. Azure'daki sunucusuz bir uygulama için kaynak grubunuz hem Azure Mantık Uygulamaları hem de Azure Fonksiyonları için kaynaklar içerir. [Azure kaynak grupları ve kaynakları](../azure-resource-manager/management/overview.md) hakkında daha fazla bilgi edinin.

1. Azure hesabınızı kullanarak Visual Studio'yı başlatın ve oturum açın.

1. **Dosya** menüsünde **Yeni** > **Proje**’yi seçin.

   ![Visual Studio'da yeni proje oluşturun](./media/logic-apps-serverless-get-started-vs/create-new-project-visual-studio.png)

1. **Yüklü** altında **Visual C#** veya **Visual Basic**’i seçin. Ardından Bulut**Azure Kaynak Grubu'nun'u** **seçin.** > 

   > [!NOTE]
   > **Bulut** kategorisi veya **Azure Kaynak Grubu** projesi yoksa, Visual Studio için Azure SDK'sını yüklediğinizden emin olun.

   Visual Studio 2019 kullanıyorsanız aşağıdaki adımları izleyin:

   1. Yeni **proje oluştur** kutusunda, Visual C# veya Visual Basic için **Azure Kaynak Grubu** proje şablonu'nu seçin ve ardından **İleri'yi**seçin.

   1. Azure kaynak grubu için kullanmak istediğiniz ad ve diğer proje bilgilerini sağlayın. İşiniz bittiğinde **Oluştur**’u seçin.

1. Projenize bir ad ve konum verin ve ardından **Tamam'ı**seçin.

   Visual Studio şablonlar listesinden bir şablon seçmenizi ister. 
   Bu örnekte, bir mantık uygulaması ve bir Azure işlevine çağrı içeren sunucusuz bir uygulama oluşturabilmeniz için bir Azure QuickStart şablonu kullanır.

   > [!TIP]
   > Çözümünüzü Azure kaynak grubuna önceden dağıtmak istemediğiniz senaryolarda, boş bir mantık uygulaması oluşturan boş **Mantık Uygulaması** şablonunu kullanabilirsiniz.

1. Bu **konum listesindeki Göster şablonlarından** **Azure QuickStart (github.com/Azure/azure-quickstart-templates)** seçeneğini belirleyin.

1. Arama kutusuna filtreniz olarak "mantık-uygulama" girin. **Sonuçlardan, 101-logic-app-and-function-app** şablonunu seçin.

   ![Azure QuickStart şablonu seçin](./media/logic-apps-serverless-get-started-vs/select-template.png)

   Visual Studio kaynak grubu projeniz için bir çözüm oluşturur ve açar. 
   Seçtiğiniz Azure QuickStart şablonu, kaynak grubu projenizde azuredeployment.json adlı bir dağıtım şablonu oluşturur. Bu dağıtım şablonu, bir HTTP isteği tarafından tetiklenen, Bir Azure işlevini çağıran ve sonucu http yanıtı olarak döndüren basit bir mantık uygulamasının tanımını içerir.

   ![Yeni sunucusuz çözüm](./media/logic-apps-serverless-get-started-vs/create-serverless-solution.png)

1. Ardından, çözümünüzü Azure'a dağıtın. Dağıtım şablonunu açıp sunucusuz uygulamanızın kaynaklarını gözden geçirmeden önce bunu yapmanız gerekir.

## <a name="deploy-your-solution"></a>Çözümünüzü dağıtın

Visual Studio'daki Logic App Designer'da mantık uygulamanızı açamadan önce Azure'da zaten dağıtılmış bir Azure kaynak grubuna sahip olmalısınız. Tasarımcı daha sonra mantık uygulamanızdaki kaynaklara ve hizmetlere bağlantılar oluşturabilir. Bu görev için, çözümünüzü Visual Studio'dan Azure portalına dağıtmak için aşağıdaki adımları izleyin:

1. Çözüm Gezgini'nde, kaynak projenizin kısayol menüsünden**Yeni**Dağıt'ı'nı **Deploy** > seçin.

   ![Kaynak grubu için yeni dağıtım oluşturma](./media/logic-apps-serverless-get-started-vs/deploy.png)

1. Henüz seçilmediyse, Azure aboneliğinizi ve dağıtmak istediğiniz kaynak grubunu seçin. Ardından, **Dağıt'ı**seçin.

   ![Dağıtım ayarları](./media/logic-apps-serverless-get-started-vs/deploy-to-resource-group.png)

1. **Parametreleri Edit** kutusu görünüyorsa, dağıtım sırasında mantık uygulamanız ve Azure işlev uygulamanız için kullanılacak kaynak adlarını sağlayın ve ayarlarınızı kaydedin. İşlev uygulamanız için benzersiz bir ad kullandığınızdan emin olun.

   ![Mantık uygulamanız ve işlev uygulamanız için adlar sağlayın](./media/logic-apps-serverless-get-started-vs/logic-function-app-name-parameters.png)

   Visual Studio belirtilen kaynak grubunuza dağıtım başladığında, çözümünüzün dağıtım durumu Visual Studio **Çıktı** penceresinde görünür. 
   Dağıtım bittikten sonra, mantık uygulamanız Azure portalında canlı olarak yayınlanır.

## <a name="edit-your-logic-app-in-visual-studio"></a>Visual Studio'da mantık uygulamanızı edin

Dağıtımdan sonra mantık uygulamanızı düzenlemek için Visual Studio'daki Logic App Designer'ı kullanarak mantık uygulamanızı açın.

1. Solution Explorer'da, azuredeploy.json dosyasının kısayol **menüsünden, Mantık uygulama tasarımcısıyla aç'ı**seçin.

   ![Logic App Designer'da azuredeploy.json'ı açın](./media/logic-apps-serverless-get-started-vs/open-logic-app-designer.png)

   > [!TIP]
   > Visual Studio 2019'da bu komuta sahip değilseniz, Visual Studio için en son güncelleştirmelerin olup olmadığını kontrol edin.

1. Logic **App Properties** kutusu göründükten sonra, **Abonelik**altında, azure aboneliğinizi seçin, önceden seçilmemişse. **Kaynak Grubu**altında, çözümünüzü dağıttığınız kaynak grubunu ve konumu seçin ve ardından **Tamam'ı**seçin.

   ![Mantık uygulaması özellikleri](./media/logic-apps-serverless-get-started-vs/logic-app-properties.png)

   Logic App Designer açıldıktan sonra, adım eklemeye devam edebilir veya iş akışını değiştirebilir ve güncelleştirmelerinizi kaydedebilirsiniz.

   ![Logic App Designer'da mantık uygulaması açıldı](./media/logic-apps-serverless-get-started-vs/opened-logic-app.png)

## <a name="create-your-azure-functions-project"></a>Azure İşlevler projenizi oluşturun

JavaScript, Python, F#, PowerShell, Batch veya Bash kullanarak İşlevlerinizi projenizi ve işlevinizi oluşturmak için [Azure İşleriyle Çalışma'daki](../azure-functions/functions-run-local.md)adımları izleyin. Çözümünüzünüzün içinde C# kullanarak Azure işlevinizi geliştirmek [için,..NET sınıf kitaplığını İşlev Uygulaması olarak](https://blogs.msdn.microsoft.com/appserviceteam/2017/03/16/publishing-a-net-class-library-as-a-function-app/)yayımla'daki adımları izleyerek C# sınıfı kitaplığı kullanın.

## <a name="deploy-functions-from-visual-studio"></a>Visual Studio'dan işlevleri dağıtma

Dağıtım şablonunuz, azuredeploy.json dosyasındaki değişkenler tarafından belirtilen Git repo'sundan çözümünüzde bulunan tüm Azure işlevlerini dağıtıyor. Çözümde İşlevler projenizi oluşturur ve yazarsanız, bu projeyi Git kaynak denetimine (örneğin, GitHub `repo` veya Azure DevOps) denetleyebilir ve şablonun Azure işlevinizi dağıtması için değişkeni güncelleştirebilirsiniz.

## <a name="manage-logic-apps-and-view-run-history"></a>Mantık uygulamalarını yönetme ve çalışma geçmişini görüntüleme

Azure'da zaten dağıtılan mantıksal uygulamalar için, çalışma geçmişini yine de dinleyebilir, yönetebilir, görüntüleyebilir ve Visual Studio'dan bu uygulamaları devre dışı kullanabilirsiniz.

1. Visual Studio'daki **Görünüm** menüsünden **Cloud Explorer'ı**açın.

1. **Tüm aboneliklerde,** yönetmek istediğiniz mantık uygulamalarıyla ilişkili Azure aboneliğini seçin ve ardından **Uygula'yı**seçin.

1. **Logic Apps**altında, mantık uygulamanızı seçin. Bu uygulamanın kısayol menüsünden **Mantık Uygulaması Düzenleyicisi ile Aç'ı**seçin.

   > [!TIP]
   > Visual Studio 2019'da bu komuta sahip değilseniz, Visual Studio için en son güncelleştirmelerin olup olmadığını kontrol edin.

Artık zaten yayınlanmış olan mantık uygulamasını kaynak grubu projenize indirebilirsiniz. Bu nedenle, Azure portalında bir mantık uygulaması başlatmış olabilirsiniz, ancak visual studio'da bu uygulamayı almaya ve yönetmeye devam edebilirsiniz. Daha fazla bilgi için Visual [Studio ile mantık uygulamalarını yönet'e](../logic-apps/manage-logic-apps-with-visual-studio.md)bakın.

## <a name="next-steps"></a>Sonraki adımlar

* [Visual Studio ile mantıksal uygulamaları yönetme](manage-logic-apps-with-visual-studio.md)
