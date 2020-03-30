---
title: Cloud Explorer ile Visual Studio'yi kullanarak mantık uygulamalarını düzenleme ve yönetme
description: Cloud Explorer ile Visual Studio'yi kullanarak mantık uygulamalarını düzenleme, güncelleme, yönetme, kaynak denetimine ekleme ve dağıtma
services: logic-apps
ms.suite: integration
ms.reviewer: klam, logicappspm
ms.topic: article
ms.custom: mvc
ms.date: 10/29/2019
ms.openlocfilehash: 6319d2d72df69cc9633bd2b2ff8e777c2a48966a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79270257"
---
# <a name="manage-logic-apps-with-visual-studio"></a>Visual Studio ile mantıksal uygulamaları yönetme

[Azure portalında](https://portal.azure.com)mantık uygulamaları oluşturabiliyor, bunları yönetebilir ve dağıtabiliyor sanız da, mantık uygulamalarınızı kaynak denetimine eklemek, farklı sürümler yayınlamak ve çeşitli dağıtım ortamları için [Azure Kaynak Yöneticisi](../azure-resource-manager/management/overview.md) şablonları oluşturmak istediğinizde Visual Studio'yu da kullanabilirsiniz. Visual Studio Cloud Explorer ile diğer Azure kaynaklarıyla birlikte mantık uygulamalarınızı bulabilir ve yönetebilirsiniz. Örneğin, Azure portalında zaten dağıtılmış olan mantık uygulamalarını açabilir, karşıdan yükleyebilir, edinebilir, çalıştırabilir, görüntüleyebilir, devre dışı bırakıp etkinleştirebilirsiniz. Visual Studio'da Azure Logic Apps ile çalışmaya yeni başladıysanız, [Visual Studio ile nasıl mantık uygulamaları oluşturabileceğinizi](../logic-apps/quickstart-create-logic-apps-with-visual-studio.md)öğrenin.

> [!IMPORTANT]
> Visual Studio'dan bir mantık uygulaması dağıtmak veya yayımlama, azure portalında bu uygulamanın sürümünün üzerine yazar. Bu nedenle, saklamak istediğiniz Azure portalında değişiklikler yaparsanız, Visual Studio'da bir sonraki dağıtımınızdan veya Visual Studio'dan yayımlamadan önce [Visual Studio'daki mantık uygulamasını](#refresh) Azure portalından yenilediğinizden emin olun.

<a name="requirements"></a>

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği. Azure aboneliğiniz yoksa [ücretsiz bir Azure hesabı için kaydolun](https://azure.microsoft.com/free/).

* Henüz yoksa şu araçları indirip yükleyin:

  * [Visual Studio 2019, 2017 veya 2015 - Topluluk sürümü veya daha büyük](https://aka.ms/download-visual-studio). Bu hızlı başlangıçta ücretsiz olan Visual Studio Community 2017 kullanılmaktadır.

    > [!IMPORTANT]
    > Visual Studio 2019 veya 2017'yi yüklediğinizde, **Azure geliştirme** iş yükünü seçtiğinizden emin olun.
    > Daha fazla bilgi için Bkz. [Visual Studio Cloud Explorer'da Azure hesaplarınızla ilişkili kaynakları yönet.](https://docs.microsoft.com/visualstudio/azure/vs-azure-tools-resources-managing-with-cloud-explorer?view)

    Visual Studio 2015 için Cloud Explorer'ı yüklemek için [Visual Studio Marketplace'ten Cloud Explorer'ı indirin.](https://marketplace.visualstudio.com/items?itemName=MicrosoftCloudExplorer.CloudExplorerforVisualStudio2015) Daha fazla bilgi için bkz: [Visual Studio Cloud Explorer'da (2015) Azure Hesaplarınızla ilişkili kaynakları yönet.](https://docs.microsoft.com/visualstudio/azure/vs-azure-tools-resources-managing-with-cloud-explorer?view=vs-2015)

  * [Azure SDK (2.9.1 veya sonrası)](https://azure.microsoft.com/downloads/)

  * [Azure PowerShell](https://github.com/Azure/azure-powershell#installation)

  * İstediğindiğiniz sürüm için Visual Studio uzantısı için en son Azure Mantık Uygulamaları Araçları:

    * [Visual Studio 2019](https://aka.ms/download-azure-logic-apps-tools-visual-studio-2019)

    * [Visual Studio 2017](https://aka.ms/download-azure-logic-apps-tools-visual-studio-2017)

    * [Visual Studio 2015](https://aka.ms/download-azure-logic-apps-tools-visual-studio-2015)

    Azure Logic Apps Araçlarını doğrudan Visual Studio Market’ten indirip yükleyebilir veya [bu uzantıyı Visual Studio’nun içinden yükleme](https://docs.microsoft.com/visualstudio/ide/finding-and-using-visual-studio-extensions) hakkında bilgi edinebilirsiniz. Yükleme işlemini tamamladıktan sonra Visual Studio’yu yeniden başlattığınızdan emin olun.

* Gömülü Logic Apps Tasarımcısını kullanırken web'e erişim

  Tasarımcının Azure'da kaynak oluşturması ve mantıksal uygulamanızdaki bağlayıcılardan özellik ve verileri okuması için İnternet bağlantısı gerekir. Örneğin, Dynamics CRM Online bağlayıcısını kullanıyorsanız, tasarımcı CRM örneğinizdeki varsayılan ve özel kullanılabilir özellikleri denetler.

<a name="find-logic-apps-vs"></a>

## <a name="find-your-logic-apps"></a>Mantık uygulamalarınızı bulun

Visual Studio'da, Azure aboneliğinizle ilişkili olan ve Azure portalında dağıtılan tüm mantık uygulamalarını Cloud Explorer'ı kullanarak bulabilirsiniz.

1. Visual Studio'yu açın. **Görünüm** menüsünde **Bulut Gezgini'ni**seçin.

1. Bulut Gezgini'nde **Hesap Yönetimi'ni**seçin. Mantık uygulamalarınızla ilişkili Azure aboneliğini seçin ve ardından **Uygula'yı**seçin. Örnek:

   !["Hesap Yönetimi" seçeneğini belirleyin](./media/manage-logic-apps-with-visual-studio/account-management-select-Azure-subscription.png)

1. **Kaynak Gruplarına** veya **Kaynak Türlerine**göre arama yapıp olmadığınıza bağlı olarak aşağıdaki adımları izleyin:

   * **Kaynak Grupları**: Azure aboneliğiniz altında Bulut Gezgini, bu abonelikle ilişkili tüm kaynak gruplarını gösterir. Mantık uygulamanızı içeren kaynak grubunu genişletin ve ardından mantık uygulamanızı seçin.

   * **Kaynak Türleri**: Azure aboneliğiniz altında **Mantık Uygulamalarını**genişletin. Cloud Explorer aboneliğinizle ilişkili tüm dağıtılan mantık uygulamalarını gösterip, mantık uygulamanızı seçin.

<a name="open-designer"></a>

## <a name="open-in-visual-studio"></a>Visual Studio'da aç

Visual Studio'da, daha önce oluşturulmuş ve dağıtılan mantık uygulamalarını doğrudan Azure portalı üzerinden veya Visual Studio ile Azure Kaynak Grubu projeleri olarak açabilirsiniz.

1. Cloud Explorer'ı açın ve mantık uygulamanızı bulun.

1. Mantık uygulamasının kısayol **menüsünde, Mantık Uygulaması Düzenleyicisi ile Aç'ı**seçin.

   > [!TIP]
   > Visual Studio 2019'da bu komuta sahip değilseniz, Visual Studio için en son güncelleştirmelerin olup olmadığını kontrol edin.

   Bu örnek, kaynak türüne göre mantık uygulamalarını gösterir, böylece mantık uygulamalarınız **Mantık Uygulamaları** bölümünün altında görünür.

   ![Azure portalından dağıtılan mantık uygulamasını açın](./media/manage-logic-apps-with-visual-studio/open-logic-app-in-editor.png)

   Mantık uygulaması Logic Apps Designer'da açıldıktan sonra, tasarımcının en altında, temel mantık uygulaması tanım yapısını gözden geçirebilmeniz için **Code View'ı** seçebilirsiniz. Mantık uygulaması için bir dağıtım şablonu oluşturmak istiyorsanız, bu mantık uygulaması için [azure kaynak yöneticisi şablonu nasıl indirdiğinizi](#download-logic-app) öğrenin. [Kaynak Yöneticisi şablonları](../azure-resource-manager/templates/overview.md)hakkında daha fazla bilgi edinin.

<a name="download-logic-app"></a>

## <a name="download-from-azure"></a>Azure'dan İndirme

Mantıksal uygulamaları [Azure portalından](https://portal.azure.com) indirebilir ve [Azure Kaynak Yöneticisi](../azure-resource-manager/management/overview.md) şablonları olarak kaydedebilirsiniz. Daha sonra Visual Studio ile şablonları yerel olarak dinleyebilir ve farklı dağıtım ortamları için mantık uygulamalarını özelleştirebilirsiniz.  İndirme mantık uygulamaları otomatik olarak [Kaynak Yöneticisi şablonları](../azure-resource-manager/templates/overview.md)içinde tanımlarını *parametreizeler* , ayrıca JavaScript Nesne Gösterimi (JSON).

1. Visual Studio'da Bulut Gezgini'ni açın. Azure'dan indirmek istediğiniz mantık uygulamasını bulun ve seçin.

1. Bu uygulamanın kısayol **menüsünde, Mantık App Editor ile Aç'ı**seçin.

   > [!TIP]
   > Visual Studio 2019'da bu komuta sahip değilseniz, Visual Studio için en son güncelleştirmelerin olup olmadığını kontrol edin.

   Mantık Uygulama Tasarımcısı açılır ve mantık uygulaması gösterir. Mantık uygulamasının temel tanımını ve yapısını incelemek için, tasarımcının en altında **Kod Görünümü'nü**seçin.

1. Tasarımcı araç çubuğunda **İndir'i**seçin.

   ![Azure portalından mantık uygulamasını indirin](./media/manage-logic-apps-with-visual-studio/download-logic-app-from-portal.png)

1. Bir konum için istendiğinde, bu konuma göz atın ve Kaynak Yöneticisi şablonunu JSON (.json) dosya biçimindeki mantık uygulaması tanımı için kaydedin.

   Mantık uygulama tanımınız Kaynak `resources` Yöneticisi şablonundaki alt bölümde görünür. Artık Visual Studio ile mantık uygulama tanımını ve Kaynak Yöneticisi şablonu'nu edinebilirsiniz. Şablonu Visual Studio çözümüne [Azure Kaynak Grubu projesi](../azure-resource-manager/templates/create-visual-studio-deployment-project.md) olarak da ekleyebilirsiniz. Visual [Studio'da mantıksal uygulamalar için Azure Kaynak Grubu projeleri](../logic-apps/quickstart-create-logic-apps-with-visual-studio.md)hakkında bilgi edinin.

<a name="link-integration-account"></a>

## <a name="link-to-integration-account"></a>Tümleştirme hesabına bağlantı

İşletmelerarası (B2B) kurumsal tümleştirme senaryoları için mantık uygulamaları oluşturmak için, mantık uygulamanızla aynı bölgede bulunan daha önce oluşturulmuş bir [tümleştirme hesabına](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md) mantık uygulamanızı bağlayabilirsiniz. Bir entegrasyon hesabı, ticaret ortakları, anlaşmalar, şemalar ve haritalar gibi B2B yapıları içerir ve mantık uygulamanızın XML doğrulama ve düz dosya kodlama veya kod çözme için B2B bağlayıcılarını kullanmasına olanak tanır. [Azure portalını kullanarak bu bağlantıyı oluşturabiliyor](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md#link-account)sanız da, [ön koşullarla](#requirements)tanıştıktan sonra Visual Studio'yu da kullanabilirsiniz ve mantık uygulamanız bir [Azure Kaynak Grubu projesinin](../azure-resource-manager/templates/create-visual-studio-deployment-project.md)içinde JSON (.json) dosyası olarak bulunmaktadır. Visual [Studio'da mantıksal uygulamalar için Azure Kaynak Grubu projeleri](../logic-apps/quickstart-create-logic-apps-with-visual-studio.md#create-resource-group-project)hakkında bilgi edinin.

1. Visual Studio'da, mantık uygulamanızı içeren Azure Kaynak Grubu projesini açın.

1. Solution Explorer'da, **<mantık-uygulama adı>.json** dosyasının kısayol menüsünü açın ve **Mantık Uygulama Tasarımcısı ile Aç'ı**seçin. (Klavye: Ctrl + L)

   ![Logic App Designer ile mantık uygulamasının .json dosyalarını açın](./media/manage-logic-apps-with-visual-studio/open-logic-app-designer.png)

   > [!TIP]
   > Visual Studio 2019'da bu komuta sahip değilseniz, Visual Studio ve Azure Mantık Uygulamaları Araçları uzantısında en son güncelleştirmelere sahip olup olmadığınızı kontrol edin.

1. Mantık Uygulama Tasarımcısı'nın tasarımcının sekmesini veya yüzeyini seçerek odaklandığından emin olun, böylece Özellikler penceresi mantık uygulamanızın **Tümleştirme Hesabı** özelliğini gösterir.

   ![Özellikler penceresi - "Tümleştirme Hesabı" özelliği](./media/manage-logic-apps-with-visual-studio/open-logic-app-properties-integration-account.png)

   > [!TIP]
   > Özellikler penceresi zaten açık değilse, **Görünüm** menüsünden **Özellikler Penceresi'ni**seçin. (Klavye: F4 tuşuna basın)

1. **Tümleştirme Hesabı** özellik listesini açın ve örneğin mantık uygulamanıza bağlamak istediğiniz tümleştirme hesabını seçin:

   !["Entegrasyon Hesabı" özellik listesini aç](./media/manage-logic-apps-with-visual-studio/select-integration-account.png)

1. İşinizi bitirdiğinizde Visual Studio çözümünüzü kaydetmeyi unutmayın.

Visual Studio'da **Tümleştirme Hesabı** özelliğini ayarladığınızda ve mantıksal uygulamanızı Azure Kaynak Yöneticisi şablonu olarak kaydettiğinizde, bu şablon, seçili tümleştirme hesabı için bir parametre bildirimi de içerir. Şablon parametreleri ve mantık uygulamaları hakkında daha fazla bilgi için genel [bakış: Mantık uygulaması dağıtımını otomatikleştirin.](../logic-apps/logic-apps-azure-resource-manager-templates-overview.md#template-parameters)

<a name="change-location"></a>

## <a name="change-deployment-location"></a>Dağıtım konumunu değiştirme

Visual Studio'da, mantık uygulamanız dağıtımı otomatikleştirmek için kullandığınız bir [Azure Kaynak Grubu projesinde](../azure-resource-manager/templates/create-visual-studio-deployment-project.md) JSON (.json) dosyası olarak varsa, bu mantık uygulaması bir konum türüne ve belirli bir konuma ayarlanır. Bu konum bir Azure bölgesi veya varolan bir [tümleştirme hizmeti ortamıdır (ISE)](connect-virtual-network-vnet-isolated-environment.md).

Mantık uygulamanızın konum türünü veya konumunu değiştirmek için, Mantık Uygulaması Tasarımcısı'nı kullanarak Solution Explorer'dan mantık uygulamanızın iş akışı tanımı (.json) dosyasını açmanız gerekir. Bulut Gezgini'ni kullanarak bu özellikleri değiştiremezsiniz.

> [!IMPORTANT]
> Konum türünü **Bölge'den** [**Tümleştirme Hizmet Ortamına**](connect-virtual-network-vnet-isolated-environment-overview.md) değiştirmek, mantık uygulamanızın faturalandırma, [sınırlamalar,](logic-apps-limits-and-config.md#integration-account-limits) [tümleştirme hesabı desteği](connect-virtual-network-vnet-isolated-environment-overview.md#ise-skus)ve benzeri için kullanılan [fiyatlandırma modelini](logic-apps-pricing.md#fixed-pricing) etkiler. Farklı bir konum türü seçmeden önce, mantık uygulamanız üzerinde ortaya çıkan etkiyi anladığınızdan emin olun.

1. Visual Studio'da, mantık uygulamanızı içeren Azure Kaynak Grubu projesini açın.

1. Solution Explorer'da, `<logic-app-name>.json` dosyanın kısayol menüsünü açın ve **Mantık Uygulama Tasarımcısı ile Aç'ı**seçin. (Klavye: Ctrl + L)

   ![Logic App Designer ile mantık uygulamasının .json dosyalarını açın](./media/manage-logic-apps-with-visual-studio/open-logic-app-designer.png)

   > [!TIP]
   > Visual Studio 2019'da bu komuta sahip değilseniz, Visual Studio ve Azure Mantık Uygulamaları Araçları uzantısında en son güncelleştirmelere sahip olup olmadığınızı kontrol edin.

1. Mantık Uygulama Tasarımcısı'nın tasarımcının sekmesini veya yüzeyini seçerek odaklandığından emin olun, böylece Özellikler penceresi mantık uygulamanız için **Konum Türünü** ve **Konumu** Seç özelliklerini gösterir. Projenin konum türü **Bölge** veya **Tümleştirme Hizmet Ortamı**olarak ayarlanır.

   ![Özellikler penceresi - "Konum Türünü Seçin" & "Konum" özellikleri](./media/manage-logic-apps-with-visual-studio/open-logic-app-properties-location.png)

   > [!TIP]
   > Özellikler penceresi zaten açık değilse, **Görünüm** menüsünden **Özellikler Penceresi'ni**seçin. (Klavye: F4 tuşuna basın)

1. Konum türünü değiştirmek için **Konum Türü** özelliğini seç listesini açın ve istediğiniz konum türünü seçin.

   Örneğin, konum türü **Tümleştirme Hizmet Ortamı**ise, **Bölge'yi**seçebilirsiniz.

   !["Konum Türünü Seç" özelliği - konum türünü değiştir](./media/manage-logic-apps-with-visual-studio/change-location-type.png)

1. Belirli konumu değiştirmek için **Konum** özelliği listesini açın. Konum türüne bağlı olarak, istediğiniz konumu seçin, örneğin:

   * Farklı bir Azure bölgesi seçin:

     !["Konum" özellik listesini açın, başka bir Azure bölgesi seçin](./media/manage-logic-apps-with-visual-studio/change-azure-resource-group-region.png)

   * Farklı bir Ise seçin:

     !["Konum" özellik listesini açın, başka bir ISE seçin](./media/manage-logic-apps-with-visual-studio/change-integration-service-environment.png)

1. İşinizi bitirdiğinizde Visual Studio çözümünüzü kaydetmeyi unutmayın.

Visual Studio'da konum türünü veya konumunu değiştirdiğinizde ve mantıksal uygulamanızı Azure Kaynak Yöneticisi şablonu olarak kaydettiğinizde, bu şablon, o konum türü ve konumu için parametre bildirimleri de içerir. Şablon parametreleri ve mantık uygulamaları hakkında daha fazla bilgi için genel [bakış: Mantık uygulaması dağıtımını otomatikleştirin.](../logic-apps/logic-apps-azure-resource-manager-templates-overview.md#template-parameters)

<a name="refresh"></a>

## <a name="refresh-from-azure"></a>Azure'dan yenile

Mantıksal uygulamanızı Azure portalında düzenliyorsanız ve bu değişiklikleri saklamak istiyorsanız, uygulamanın sürümünü Visual Studio'da bu değişikliklerle yenilediğinizden emin olun.

* Visual Studio'da, Logic App Designer araç çubuğunda **Yenile'yi**seçin.

  -veya-

* Visual Studio Cloud Explorer'da mantık uygulamanızın kısayol menüsünü açın ve **Yenile'yi**seçin.

![Güncellemelerle mantık uygulamasını yenile](./media/manage-logic-apps-with-visual-studio/refresh-logic-app-with-updates-from-portal.png)

## <a name="publish-logic-app-updates"></a>Mantıksal uygulama güncelleştirmelerini yayımlama

Visual Studio'dan Azure'a mantık uygulama güncelleştirmelerinizi dağıtmaya hazır olduğunuzda, Logic App Designer araç çubuğunda **Yayımla'yı**seçin.

![Güncelleştirilmiş mantık uygulamasını Azure portalında yayımlama](./media/manage-logic-apps-with-visual-studio/publish-logic-app-to-azure-portal.png)

## <a name="manually-run-your-logic-app"></a>Mantıksal uygulamanızı el ile çalıştırma

Visual Studio'dan Azure'da dağıtılan bir mantık uygulamasını el ile tetikleyebilirsiniz. Logic App Designer araç çubuğunda **Tetikle'yi Çalıştır'ı**seçin.

![Mantık uygulamanız için tetikleyiciyi el ile çalıştırın](./media/manage-logic-apps-with-visual-studio/manually-run-logic-app.png)

## <a name="review-run-history"></a>Çalıştırma geçmişini gözden geçirme

Durumu kontrol etmek ve mantık uygulaması çalıştırmalarıyla ilgili sorunları tanılamak için Visual Studio'da çalışan kişiler için giriş ve çıkışlar gibi ayrıntıları gözden geçirebilirsiniz.

1. Cloud Explorer'da, mantık uygulamanızın kısayol menüsünü açın ve **çalıştır geçmişini aç'ı**seçin.

   ![Mantık uygulamanız için çalışma geçmişini açın](./media/manage-logic-apps-with-visual-studio/open-run-history-for-logic-app.png)

1. Belirli bir çalıştırmanın ayrıntılarını görüntülemek için, bir çalıştırmayı çift tıklatın. Örnek:

   ![Belirli çalışma yla ilgili bilgileri görüntüleme](./media/manage-logic-apps-with-visual-studio/view-run-history-details.png)
  
   > [!TIP]
   > Tabloyu özelliğe göre sıralamak için, bu özellik için sütun üstbilgisini seçin.

1. Örneğin, gözden geçirmek istediğiniz girdi ve çıktıları artırın:

   ![Her adım için giriş ve çıktıları görüntüleme](./media/manage-logic-apps-with-visual-studio/view-run-history-inputs-outputs.png)

## <a name="disable-or-enable-logic-app"></a>Mantıksal uygulamayı devre dışı bırakma veya etkinleştirme

Mantık uygulamanızı silmeden, tetikleyici koşulu karşılandığında bir sonraki seferde tetikleyicinin ateşlemesini durdurabilirsiniz. Mantık uygulamanızı devre dışı bırakmak, Logic Apps altyapısının mantık uygulamanız için gelecekteki iş akışı örnekleri oluşturmasını ve çalıştırmasını engeller. Cloud Explorer'da, mantık uygulamanızın kısayol menüsünü açın ve **Devre Dışı'** yı seçin.

![Bulut Gezgini'nde mantık uygulamanızı devre dışı düşür](./media/manage-logic-apps-with-visual-studio/disable-logic-app-cloud-explorer.png)

> [!NOTE]
> Bir mantık uygulamasını devre dışı dışı bettiğinizde, hiçbir yeni çalıştırma anında yapılmaz. Devam eden ve bekleyen tüm çalıştırmalar tamamlanana kadar devam edecektir ve bu da tamamlanması zaman alabilir.

Mantık uygulamanızı Yeniden etkinleştirmek için Cloud Explorer'da mantık uygulamanızın kısayol menüsünü açın ve **Etkinleştir'i**seçin.

![Cloud Explorer'da mantık uygulamasını etkinleştirme](./media/manage-logic-apps-with-visual-studio/enable-logic-app-cloud-explorer.png)

## <a name="delete-your-logic-app"></a>Mantıksal uygulamanızı silme

Mantık uygulamanızı Azure portalından, Bulut Gezgini'nden silmek için mantık uygulamanızın kısayol menüsünü açın ve **Sil'i**seçin.

![Mantıksal uygulamanızı Azure portalından silme](./media/manage-logic-apps-with-visual-studio/delete-logic-app-from-azure-portal.png)

> [!NOTE]
> Mantıksal uygulamayı sildiğinizde yeni çalıştırma başlatılmaz. Devam eden ve bekleme durumunda olan tüm çalıştırmalar iptal edilir. Binlerce çalıştırma varsa iptal işleminin tamamlanması zaman alabilir. 

## <a name="troubleshooting"></a>Sorun giderme

Mantık Uygulamaları Tasarımcısı'nda mantık uygulama projenizi açtığınızda, Azure aboneliğinizi seçme seçeneğini elde emeyebilirsiniz. Bunun yerine, mantık uygulamanız kullanmak istemediğiniz bir Azure aboneliğiyle açılır. Bu davranış, bir mantık uygulamasının .json dosyasını açtıktan sonra Visual Studio'nun gelecekteki kullanım için seçilen ilk aboneliği önbelleğe aldığı için ortaya nedenle olur. Bu sorunu gidermek için şu adımlardan birini deneyin:

* Mantık uygulamasının .json dosyasını yeniden adlandırın. Abonelik önbelleği dosya adına bağlıdır.

* Çözümünüzdeki *tüm* mantık uygulamaları için önceden seçili abonelikleri kaldırmak için, çözümünüzün dizinindeki gizli Visual Studio ayarları klasörünü (.vs) silin. Bu konum, abonelik bilgilerinizi depolar.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, Visual Studio ile dağıtılan mantık uygulamalarını nasıl yöneteceklerini öğrendiniz. Ardından, dağıtım için mantık uygulama tanımlarını özelleştirme hakkında bilgi edinin:

> [!div class="nextstepaction"]
> [JSON'da yazar mantığı uygulama tanımları](../logic-apps/logic-apps-author-definitions.md)
