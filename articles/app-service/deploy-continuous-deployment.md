---
title: Sürekli dağıtımı yapılandırma
description: GitHub, BitBucket, Azure Repos veya diğer depolardan CI/CD'yi Azure Uygulama Hizmeti'ne nasıl etkinleştirin öğrenin. İhtiyaçlarınıza uygun yapı ardışık hattını seçin.
ms.assetid: 6adb5c84-6cf3-424e-a336-c554f23b4000
ms.topic: article
ms.date: 08/23/2019
ms.reviewer: dariac
ms.custom: seodec18
ms.openlocfilehash: d58eb333c930d2ffac4eb57340ea776338325181
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79266045"
---
# <a name="continuous-deployment-to-azure-app-service"></a>Azure Uygulama Hizmetine sürekli dağıtım

[Azure Uygulama Hizmeti,](overview.md) en son güncelleştirmeleri çekerek GitHub, BitBucket ve [Azure Depoları](https://azure.microsoft.com/services/devops/repos/) depolarından sürekli dağıtım sağlar. Bu makalede, Kudu yapı hizmeti veya [Azure Ardışık Hatları](https://azure.microsoft.com/services/devops/pipelines/)aracılığıyla uygulamanızı sürekli olarak dağıtmak için Azure portalını nasıl kullanacağınızı gösterilmektedir. 

Kaynak denetim hizmetleri hakkında daha fazla bilgi için bkz: [repo oluştur (GitHub)], [repo oluşturma (BitBucket)]veya [yeni bir Git repo (Azure Repos) oluşturun.]

[!INCLUDE [Prepare repository](../../includes/app-service-deploy-prepare-repo.md)]

## <a name="authorize-azure-app-service"></a>Azure Uygulama Hizmetini Yetkilendirme 

Azure Repos'unu kullanmak için Azure DevOps kuruluşunuzun Azure aboneliğinize bağlı olduğundan emin olun. Daha fazla bilgi için [bkz.](https://docs.microsoft.com/azure/devops/pipelines/apps/cd/deploy-webdeploy-webapps?view=azure-devops)

Bitbucket veya GitHub için, deponuza bağlanmak için Azure Uygulama Hizmeti'ne yetki verin. Yalnızca bir kez kaynak kontrol hizmeti ile yetkilendirmeniz gerekir. 

1. Azure [portalında,](https://portal.azure.com) **Uygulama Hizmetlerini**arayın ve seçin. 

   ![Uygulama hizmetlerini arayın.](media/app-service-continuous-deployment/search-for-app-services.png)

1. Dağıtmak istediğiniz web uygulamasını seçin.

   ![Uygulamanızı seçin.](media/app-service-continuous-deployment/select-your-app.png)
   
1. Uygulama sayfasında, sol menüde **Dağıtım Merkezi'ni** seçin.
   
1. Dağıtım **Merkezi** sayfasında **GitHub** veya **Bitbucket'ı**seçin ve ardından **Yetkilendirme'yi**seçin. 
   
   ![Kaynak denetim hizmetini seçin ve ardından Authorize'u seçin.](media/app-service-continuous-deployment/github-choose-source.png)
   
1. Gerekirse serviste oturum açın ve yetkilendirme istemlerini izleyin. 

## <a name="enable-continuous-deployment"></a>Sürekli dağıtımı etkinleştirme 

Bir kaynak kontrol hizmetine yetki verdikten sonra, uygulamanızı yerleşik [Kudu App Service build sunucusu](#option-1-use-the-app-service-build-service)veya Azure [Ardışık Hatları](#option-2-use-azure-pipelines)aracılığıyla sürekli dağıtım için yapılandırın. 

### <a name="option-1-use-the-app-service-build-service"></a>Seçenek 1: Uygulama Hizmeti oluşturma hizmetini kullanın

Yerleşik Kudu App Service yapı sunucusunu sürekli olarak GitHub, Bitbucket veya Azure Deposu'ndan dağıtmak için kullanabilirsiniz. 

1. Azure [portalında,](https://portal.azure.com) **Uygulama Hizmetlerini**arayın ve seçin ve ardından dağıtmak istediğiniz web uygulamasını seçin. 
   
1. Uygulama sayfasında, sol menüde **Dağıtım Merkezi'ni** seçin.
   
1. **Dağıtım Merkezi** sayfasında yetkili kaynak denetim sağlayıcınızı seçin ve **Devam et'i**seçin. GitHub veya Bitbucket için yetkili hesabı değiştirmek için **Hesabı Değiştir'i** de seçebilirsiniz. 
   
   > [!NOTE]
   > Azure Repos'unu kullanmak için Azure DevOps Hizmetleri kuruluşunuzun Azure aboneliğinize bağlı olduğundan emin olun. Daha fazla bilgi için [bkz.](https://docs.microsoft.com/azure/devops/pipelines/apps/cd/deploy-webdeploy-webapps?view=azure-devops)
   
1. GitHub veya Azure Repos için, **Yapı sağlayıcısı** sayfasında, Uygulama Hizmeti **oluşturma hizmetini**seçin ve ardından **Devam et'i**seçin. Bitbucket her zaman Uygulama Hizmeti oluşturma hizmetini kullanır.
   
   ![Uygulama Hizmeti oluşturma hizmetini seçin ve ardından Devam et'i seçin.](media/app-service-continuous-deployment/choose-kudu.png)
   
1. **Yapılandırılan** sayfada:
   
   - GitHub için, aşağı inin ve sürekli olarak dağıtmak istediğiniz **Kuruluş,** **Depo**ve **Şubeyi** seçin.
     
     > [!NOTE]
     > Herhangi bir depo görmüyorsanız, Azure Uygulama Hizmetini GitHub'da yetkilendirmeniz gerekebilir. GitHub deponuza göz atın ve **Ayarlar** > **Uygulamaları** > **Yetkili OAuth Apps**gidin. **Azure Uygulama Hizmeti'ni**seçin ve ardından **Hibe'yi**seçin. Kuruluş depoları için izinleri vermek için kuruluşun sahibi olmalısınız.
     
   - Bitbucket için, sürekli olarak dağıtmak istediğiniz Bitbucket **Takımını,** **Deposunu**ve **Şubeyi** seçin.
     
   - Azure Deposu için, sürekli olarak dağıtmak istediğiniz **Azure DevOps Organizasyonu,** **Proje**, **Depo**ve **Şube'yi** seçin.
     
     > [!NOTE]
     > Azure DevOps kuruluşunuz listede yoksa, Azure aboneliğinize bağlı olduğundan emin olun. Daha fazla bilgi için [bkz.](https://docs.microsoft.com/azure/devops/pipelines/apps/cd/deploy-webdeploy-webapps?view=azure-devops)
     
1. **Devam**'ı seçin.
   
   ![Depo bilgilerini doldurun ve devam et'i seçin.](media/app-service-continuous-deployment/configure-kudu.png)
   
1. Yapı sağlayıcısını yapılandırdıktan sonra **Özet** sayfasındaki ayarları gözden geçirin ve **ardından Bitir'i**seçin.
   
   Seçili depo ve şubedeki yeni taahhütler artık Uygulama Hizmeti uygulamanıza sürekli olarak dağıtılır. **Dağıtım Merkezi** sayfasında taahhütleri ve dağıtımları izleyebilirsiniz.
   
   ![Dağıtım Merkezi'ndeki taahhütleri ve dağıtımları izleme](media/app-service-continuous-deployment/github-finished.png)

### <a name="option-2-use-azure-pipelines"></a>Seçenek 2: Azure Ardışık Hatlar'ı kullanma 

Hesabınızda gerekli izinler varsa, Azure Ardışık Hatlar'ı Sürekli Olarak GitHub veya Azure Depo depolarından dağıtacak şekilde ayarlayabilirsiniz. Azure Ardışık Hatları üzerinden dağıtım hakkında daha fazla bilgi için [bkz.](/azure/devops/pipelines/apps/cd/deploy-webdeploy-webapps)

Azure Uygulama Hizmeti'nin Azure DevOps kuruluşunuzda sürekli teslimat Azure Boru Hatları oluşturması için: 

- Azure hesabınız, Azure Etkin Dizin'e yazma ve hizmet oluşturma izinlerine sahip olmalıdır. 
  
- Azure hesabınız, Azure aboneliğinizde **Sahip** rolüne sahip olmalıdır.

- Kullanmak istediğiniz Azure DevOps projesinde yönetici olmalısınız.

Azure Ardışık Hatlar 'ı yapılandırmak için (Önizleme):

1. Azure [portalında,](https://portal.azure.com) **Uygulama Hizmetlerini**arayın ve seçin ve ardından dağıtmak istediğiniz web uygulamasını seçin. 
   
1. Uygulama sayfasında, sol menüde **Dağıtım Merkezi'ni** seçin.
   
1. Sağlayıcı **Oluştur** sayfasında **Azure Ardışık Hatları (Önizleme) seçeneğini**belirleyin ve ardından Devam **et'i**seçin. 
   
1. **Yapılandırma** sayfasında, **Kod** bölümünde:
   
   - GitHub için, aşağı inin ve sürekli olarak dağıtmak istediğiniz **Kuruluş,** **Depo**ve **Şubeyi** seçin.
     
     > [!NOTE]
     > Herhangi bir depo görmüyorsanız, Azure Uygulama Hizmetini GitHub'da yetkilendirmeniz gerekebilir. GitHub deponuza göz atın ve **Ayarlar** > **Uygulamaları** > **Yetkili OAuth Apps**gidin. **Azure Uygulama Hizmeti'ni**seçin ve ardından **Hibe'yi**seçin. Kuruluş depoları için izinleri vermek için kuruluşun sahibi olmalısınız.
     
   - Azure Depoları için, sürekli olarak dağıtmak istediğiniz **Azure DevOps Organizasyonu**, **Proje**, **Depo**ve **Şube'yi** seçin veya yeni bir Azure DevOps kuruluşunu yapılandırın.
     
     > [!NOTE]
     > Mevcut Azure DevOps kuruluşunuz listede yoksa, azure aboneliğinize bağlamanız gerekebilir. Daha fazla bilgi için cd [sürüm ardışık alanınızı tanımlayın'](/azure/devops/pipelines/apps/cd/deploy-webdeploy-webapps#cd)a bakın.
     
1. **Devam**'ı seçin.
   
1. Azure Deposu için **Yapı** bölümünde, Azure Ardışık Hatlarının yapı görevlerini çalıştırmak için kullanması gereken dil çerçevesini belirtin ve ardından **Devam et'i**seçin.
   
1. **Test** sayfasında, yük testlerini etkinleştirip etkinleştirmeyeceğinizi seçin ve ardından **Devam et'i**seçin.
   
1. Uygulama Hizmeti planı [fiyatlandırma katmanınıza](https://azure.microsoft.com/pricing/details/app-service/plans/)bağlı olarak, **bir Hazırlama sayfasına Dağıt'ı** görebilirsiniz. [Dağıtım yuvalarını etkinleştirip etkinleştirmeyeceğini](deploy-staging-slots.md)seçin ve sonra **Devam et'i**seçin.
   
   > [!NOTE]
   > Azure Pipelines, üretim yuvasına sürekli olarak teslim edilebilmesine izin vermez. Bu kısıtlama, üretime yanlışlıkla dağıtımları önler. Bir evreleme yuvasına sürekli teslimat ayarlayın, değişiklikleri orada doğrulayın ve hazır olduğunuzda yuvaları değiştirin.
   
1. Yapı sağlayıcısını yapılandırdıktan sonra **Özet** sayfasındaki ayarları gözden geçirin ve **ardından Bitir'i**seçin.
   
   Seçili depo ve şubedeki yeni taahhütler artık Uygulama Hizmeti uygulamanıza sürekli olarak dağıtılır. **Dağıtım Merkezi** sayfasında taahhütleri ve dağıtımları izleyebilirsiniz.
   
   ![Dağıtım Merkezi'ndeki taahhütleri ve dağıtımları izleme](media/app-service-continuous-deployment/github-finished.png)

## <a name="disable-continuous-deployment"></a>Sürekli dağıtımı devre dışı bırakma

Sürekli dağıtımı devre dışı kayırmak için uygulamanızın **Dağıtım Merkezi** sayfasının üst kısmında ki **Bağlantıyı Kes'i** seçin.

![Sürekli dağıtımı devre dışı bırakma](media/app-service-continuous-deployment/disable.png)

[!INCLUDE [What happens to my app during deployment?](../../includes/app-service-deploy-atomicity.md)]

## <a name="use-unsupported-repos"></a>Desteklenmeyen depoları kullanma

Windows uygulamaları için, portalın Doğrudan desteklemediği bir bulut Git veya Mercurial deposundan sürekli dağıtımı el ile yapılandırabilirsiniz( [Örneğin, GitLab](https://gitlab.com/)gibi). **Dağıtım Merkezi** sayfasındaki Dış kutuyu seçerek bunu yaparsınız. Daha fazla bilgi için [bkz.](https://github.com/projectkudu/kudu/wiki/Continuous-deployment#setting-up-continuous-deployment-using-manual-steps)

## <a name="additional-resources"></a>Ek kaynaklar

* [Sürekli dağıtımla ilgili sık karşılaşılan sorunları araştırma](https://github.com/projectkudu/kudu/wiki/Investigating-continuous-deployment)
* [Azure PowerShell’i kullanma](/powershell/azureps-cmdlets-docs)
* [Git belgeleri](https://git-scm.com/documentation)
* [Kudu Projesi](https://github.com/projectkudu/kudu/wiki)

[Depo oluşturma (GitHub)]: https://help.github.com/articles/create-a-repo
[Depo oluşturma (BitBucket)]: https://confluence.atlassian.com/get-started-with-bitbucket/create-a-repository-861178559.html
[Yeni bir Git reposu oluşturma (Azure Repos)]: /azure/devops/repos/git/creatingrepo
