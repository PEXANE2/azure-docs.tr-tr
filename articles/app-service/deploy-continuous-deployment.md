---
title: Sürekli dağıtımı yapılandırma
description: GitHub, BitBucket, Azure Repos veya diğer depolardan Azure App Service için CI/CD 'yi etkinleştirmeyi öğrenin. Gereksinimlerinize uyan derleme işlem hattını seçin.
ms.assetid: 6adb5c84-6cf3-424e-a336-c554f23b4000
ms.topic: article
ms.date: 03/20/2020
ms.reviewer: dariac
ms.custom: seodec18
ms.openlocfilehash: b7730558e2a660b0cf00a5b6962d1e2275dd472c
ms.sourcegitcommit: bdd5c76457b0f0504f4f679a316b959dcfabf1ef
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/22/2020
ms.locfileid: "90984406"
---
# <a name="continuous-deployment-to-azure-app-service"></a>Azure App Service için sürekli dağıtım

[Azure App Service](overview.md) , en son güncelleştirmeleri çekerek GitHub, bitbucket ve [Azure Repos](https://azure.microsoft.com/services/devops/repos/) depolarından sürekli dağıtım yapılmasını mümkün. Bu makalede, kudu derleme hizmeti veya [Azure Pipelines](https://azure.microsoft.com/services/devops/pipelines/)aracılığıyla uygulamanızı sürekli olarak dağıtmak için Azure Portal nasıl kullanılacağı gösterilmektedir. 

Kaynak denetim hizmetleri hakkında daha fazla bilgi için bkz. [depo oluşturma (GitHub)], [depo oluşturma (Bitbucket)]veya [Yeni bir git deposu oluşturma (Azure Repos)].

[!INCLUDE [Prepare repository](../../includes/app-service-deploy-prepare-repo.md)]

## <a name="authorize-azure-app-service"></a>Azure App Service yetkilendir 

Azure Repos kullanmak için Azure DevOps kuruluşunuzun Azure aboneliğinize bağlı olduğundan emin olun. Daha fazla bilgi için bkz. bir [Azure DevOps Services hesabını bir Web uygulamasına dağıtabilmesi Için ayarlama](/azure/devops/pipelines/apps/cd/deploy-webdeploy-webapps?view=azure-devops&preserve-view=true).

Bitbucket veya GitHub için Azure App Service, deponuza bağlanmak üzere yetkilendirin. Yalnızca bir kaynak denetim hizmetiyle bir kez yetkilendirme yapmanız gerekir. 

1. [Azure Portal](https://portal.azure.com), **uygulama hizmetleri** ' ni arayın ve öğesini seçin.

   ![Uygulama Hizmetleri için arama yapın.](media/app-service-continuous-deployment/search-for-app-services.png)

1. Dağıtmak istediğiniz App Service seçin.

   ![Uygulamanızı seçin.](media/app-service-continuous-deployment/select-your-app.png)
   
1. Uygulama sayfasında, sol taraftaki menüden **Dağıtım Merkezi** ' ni seçin.
   
1. **Dağıtım Merkezi** sayfasında **GitHub** veya **Bitbucket**' ı seçin ve ardından **Yetkilendir**' i seçin. 
   
   ![Kaynak denetimi hizmeti ' ni seçin, sonra Yetkilendir ' i seçin.](media/app-service-continuous-deployment/github-choose-source.png)
   
1. Gerekirse hizmette oturum açın ve yetkilendirme istemlerini izleyin. 

## <a name="enable-continuous-deployment"></a>Sürekli dağıtımı etkinleştirme 

Bir kaynak denetim hizmetine yetki verdikten sonra, uygulamanızı yerleşik [kudu App Service](#option-1-kudu-app-service) derleme sunucusu aracılığıyla veya [Azure Pipelines](#option-2-azure-pipelines)aracılığıyla sürekli dağıtım için yapılandırın. 

### <a name="option-1-kudu-app-service"></a>Seçenek 1: kudu App Service

Yerleşik kudu App Service derleme sunucusunu, GitHub, Bitbucket veya Azure Repos sürekli olarak dağıtmak için kullanabilirsiniz. 

1. [Azure Portal](https://portal.azure.com), **uygulama hizmetleri**' ni arayın ve ardından dağıtmak istediğiniz App Service seçin. 
   
1. Uygulama sayfasında, sol taraftaki menüden **Dağıtım Merkezi** ' ni seçin.
   
1. **Dağıtım Merkezi** sayfasında yetkili Kaynak denetimi sağlayıcınızı seçin ve **devam**' ı seçin. GitHub veya Bitbucket için, yetkili hesabı değiştirmek üzere **hesabı Değiştir** ' i de seçebilirsiniz. 
   
   > [!NOTE]
   > Azure Repos kullanmak için Azure DevOps Services kuruluşunuzun Azure aboneliğinize bağlı olduğundan emin olun. Daha fazla bilgi için bkz. bir [Azure DevOps Services hesabını bir Web uygulamasına dağıtabilmesi Için ayarlama](/azure/devops/pipelines/apps/cd/deploy-webdeploy-webapps?view=azure-devops&preserve-view=true).
   
1. GitHub veya Azure Repos için, **Yapı sağlayıcısı** sayfasında **App Service derleme hizmeti**' ni seçin ve ardından **devam**' ı seçin. Bitbucket her zaman App Service derleme hizmetini kullanır.
   
   ![App Service Build Service ' i seçin ve ardından devam ' ı seçin.](media/app-service-continuous-deployment/choose-kudu.png)
   
1. **Yapılandır** sayfasında:
   
   - GitHub için, aşağı açılan ve sürekli dağıtmak istediğiniz **kuruluş**, **Depo**ve **dalı** seçin.
     
     > [!NOTE]
     > Herhangi bir depo görmüyorsanız, GitHub 'da Azure App Service yetkilendirmeniz gerekebilir. GitHub deponuza gidin ve **Ayarlar**  >  **uygulamalar**  >  **yetkili OAuth uygulamalar**' a gidin. **Azure App Service**' yi seçin ve ardından **ver**' i seçin. Kuruluş depolarında, izinleri vermek için kuruluşun sahibi olmanız gerekir.
     
   - Bitbucket için, sürekli olarak dağıtmak istediğiniz Bitbucket **ekibini**, **deposunu**ve **dalını** seçin.
     
   - Azure Repos için, sürekli olarak dağıtmak istediğiniz **Azure DevOps organizasyonu**, **Proje**, **Depo**ve **dalı** seçin.
     
     > [!NOTE]
     > Azure DevOps kuruluşunuz listelenmiyorsa, Azure aboneliğinize bağlı olduğundan emin olun. Daha fazla bilgi için bkz. bir [Azure DevOps Services hesabını bir Web uygulamasına dağıtmak Için ayarlama](/azure/devops/pipelines/apps/cd/deploy-webdeploy-webapps?view=azure-devops&preserve-view=true).
     
1. **Devam**’ı seçin.
   
   ![Depo bilgilerini doldurup devam ' ı seçin.](media/app-service-continuous-deployment/configure-kudu.png)
   
1. Yapı sağlayıcısını yapılandırdıktan sonra, **Özet** sayfasındaki ayarları gözden geçirin ve ardından **son**' u seçin.
   
1. Seçili depodaki ve daldaki yeni işlemeler artık App Service uygulamanıza sürekli olarak dağıtılır. Yürütmeleri ve dağıtımları **Dağıtım Merkezi** sayfasında izleyebilirsiniz.
   
   ![Dağıtım Merkezi 'nde işlemeleri ve dağıtımları izleme](media/app-service-continuous-deployment/github-finished.png)

### <a name="option-2-azure-pipelines"></a>Seçenek 2: Azure Pipelines 

Hesabınız gerekli izinlere sahipse, GitHub 'dan veya Azure Repos sürekli olarak dağıtmak için Azure Pipelines ayarlayabilirsiniz. Azure Pipelines aracılığıyla dağıtma hakkında daha fazla bilgi için bkz. [Azure App Services 'a Web uygulaması dağıtma](/azure/devops/pipelines/apps/cd/deploy-webdeploy-webapps).

#### <a name="prerequisites"></a>Önkoşullar

Azure Pipelines kullanarak sürekli teslim oluşturmak Azure App Service için, Azure DevOps kuruluşunuzun aşağıdaki izinlere sahip olması gerekir: 

- Azure hesabınızın Azure Active Directory yazma ve hizmet oluşturma izinlerine sahip olması gerekir. 
  
- Azure hesabınızın, Azure aboneliğinizde **sahip** rolü olmalıdır.

- Kullanmak istediğiniz Azure DevOps projesinde bir yönetici olmanız gerekir.

#### <a name="github--azure-pipelines"></a>GitHub + Azure Pipelines

1. [Azure Portal](https://portal.azure.com), **uygulama hizmetleri**' ni arayın ve ardından dağıtmak istediğiniz App Service seçin. 
   
1. Uygulama sayfasında, sol taraftaki menüden **Dağıtım Merkezi** ' ni seçin.

1. **Dağıtım Merkezi** sayfasında, kaynak denetimi sağlayıcısı olarak **GitHub** ' ı seçin ve **devam**' ı seçin. **GitHub**için, yetkili hesabı değiştirmek üzere **hesabı Değiştir** ' i seçebilirsiniz.

    :::image type="content" source="media/app-service-continuous-deployment/deployment-center-src-control.png" alt-text="App Service Deployment Center sayfasının ekran görüntüsü.":::
   
1. **Yapı sağlayıcısı** sayfasında **Azure Pipelines (Önizleme)** öğesini seçin ve ardından **devam**' ı seçin.

    :::image type="content" source="media/app-service-continuous-deployment/select-build-provider.png" alt-text="Azure Pipelines (Önizleme) ile birlikte Dağıtım Merkezi sayfasını gösteren ekran görüntüsü.":::
   
1. **Yapılandırma** sayfasında, **kod** bölümünde, sürekli olarak dağıtmak istediğiniz **kuruluş**, **Depo**ve **dalı** seçin ve **devam**' ı seçin.
     
     > [!NOTE]
     > Herhangi bir depo görmüyorsanız, GitHub 'da Azure App Service yetkilendirmeniz gerekebilir. GitHub deponuza gidin ve **Ayarlar**  >  **uygulamalar**  >  **yetkili OAuth uygulamalar**' a gidin. **Azure App Service**' yi seçin ve ardından **ver**' i seçin. Kuruluş depolarında, izinleri vermek için kuruluşun sahibi olmanız gerekir.
       
    **Build** bölümünde, Azure Pipelines derleme görevlerini çalıştırmak için kullanması gereken Azure DevOps organizasyonu, proje, dil çerçevesini belirtin ve ardından **devam**' ı seçin.

   :::image type="content" source="media/app-service-continuous-deployment/build-configure.png" alt-text="Alanlara örnek metin içeren derleme bölümünün ekran görüntüsü.":::

1. Yapı sağlayıcısını yapılandırdıktan sonra, **Özet** sayfasındaki ayarları gözden geçirin ve ardından **son**' u seçin.

   :::image type="content" source="media/app-service-continuous-deployment/summary.png" alt-text="Yenileme düğmesi vurgulanmış işlemeler ve dağıtımları gösteren Dağıtım Merkezi sayfasının ekran görüntüsü.":::
   
1. Seçili depodaki ve daldaki yeni işlemeler artık App Service sürekli olarak dağıtılır. Yürütmeleri ve dağıtımları **Dağıtım Merkezi** sayfasında izleyebilirsiniz.
   
   ![Dağıtım Merkezi 'nde işlemeleri ve dağıtımları izleme](media/app-service-continuous-deployment/github-finished.png)

#### <a name="azure-repos--azure-pipelines"></a>Azure Repos + Azure Pipelines

1. [Azure Portal](https://portal.azure.com), **uygulama hizmetleri**' ni arayın ve ardından dağıtmak istediğiniz App Service seçin. 
   
1. Uygulama sayfasında, sol taraftaki menüden **Dağıtım Merkezi** ' ni seçin.

1. **Dağıtım Merkezi** sayfasında kaynak denetimi sağlayıcısı olarak **Azure Repos** seçin ve **devam**' ı seçin.

    :::image type="content" source="media/app-service-continuous-deployment/deployment-center-src-control.png" alt-text="Sürekli dağıtım (CI/CD) seçimlerini gösteren Dağıtım Merkezi sayfasının ekran görüntüsü.":::

1. **Yapı sağlayıcısı** sayfasında **Azure Pipelines (Önizleme)** öğesini seçin ve ardından **devam**' ı seçin.

    :::image type="content" source="media/app-service-continuous-deployment/azure-pipelines.png" alt-text="Azure Pipelines (Önizleme) gösteren dağıtım merkezinin ekran görüntüsü.":::

1. **Yapılandırma** sayfasında, **kod** bölümünde, sürekli olarak dağıtmak istediğiniz **kuruluş**, **Depo**ve **dalı** seçin ve **devam**' ı seçin.

   > [!NOTE]
   > Mevcut Azure DevOps kuruluşunuz listede yoksa Azure aboneliğinize bağlamanız gerekebilir. Daha fazla bilgi için bkz. [CD yayın işlem hattınızı tanımlama](/azure/devops/pipelines/apps/cd/deploy-webdeploy-webapps#cd).

   **Build** bölümünde, Azure Pipelines derleme görevlerini çalıştırmak için kullanması gereken Azure DevOps organizasyonu, proje, dil çerçevesini belirtin ve ardından **devam**' ı seçin.

   :::image type="content" source="media/app-service-continuous-deployment/build-configure.png" alt-text="Azure DevOps kuruluşunun ve proje alanlarının örneklerle doldurulmuş olarak gösterildiği Build bölümünün ekran görüntüsü.":::

1. Yapı sağlayıcısını yapılandırdıktan sonra, **Özet** sayfasındaki ayarları gözden geçirin ve ardından **son**' u seçin.  
     
   :::image type="content" source="media/app-service-continuous-deployment/summary-azure-pipelines.png" alt-text="Özet sayfasında seçili ayarları gösteren ekran görüntüsü.":::

1. Seçili depodaki ve daldaki yeni işlemeler artık App Service sürekli olarak dağıtılır. Yürütmeleri ve dağıtımları **Dağıtım Merkezi** sayfasında izleyebilirsiniz.

## <a name="disable-continuous-deployment"></a>Sürekli dağıtımı devre dışı bırakma

Sürekli dağıtımı devre dışı bırakmak için uygulamanızın **Dağıtım Merkezi** sayfasının en üstündeki **bağlantıyı kes** ' i seçin.

![Sürekli dağıtımı devre dışı bırakma](media/app-service-continuous-deployment/disable.png)

[!INCLUDE [What happens to my app during deployment?](../../includes/app-service-deploy-atomicity.md)]

## <a name="use-unsupported-repos"></a>Desteklenmeyen depoları kullan

Windows uygulamaları için, [Mercurial gibi,](https://gitlab.com/)portalın doğrudan desteklemediği bir bulut git veya deposundan sürekli dağıtımı el ile yapılandırabilirsiniz. Bunu, **Dağıtım Merkezi** sayfasındaki dış kutusunu seçerek yapabilirsiniz. Daha fazla bilgi için bkz. [el ile adımları kullanarak sürekli dağıtımı ayarlama](https://github.com/projectkudu/kudu/wiki/Continuous-deployment#setting-up-continuous-deployment-using-manual-steps).

## <a name="additional-resources"></a>Ek kaynaklar

* [Sürekli dağıtımla ilgili yaygın sorunları araştırın](https://github.com/projectkudu/kudu/wiki/Investigating-continuous-deployment)
* [Azure PowerShell kullanma](/powershell/azure/)
* [Git belgeleri](https://git-scm.com/documentation)
* [Kudu Projesi](https://github.com/projectkudu/kudu/wiki)

[Depo oluşturma (GitHub)]: https://help.github.com/articles/create-a-repo
[Depo oluşturma (BitBucket)]: https://confluence.atlassian.com/get-started-with-bitbucket/create-a-repository-861178559.html
[Yeni git deposu oluşturma (Azure Repos)]: /azure/devops/repos/git/creatingrepo