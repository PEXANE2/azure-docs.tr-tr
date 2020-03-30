---
title: Yerel Git repo'sundan dağıt
description: Azure Uygulama Hizmeti'ne yerel Git dağıtımını nasıl etkinleştirin öğrenin. Yerel makinenizden kod dağıtmanın en basit yollarından biri.
ms.assetid: ac50a623-c4b8-4dfd-96b2-a09420770063
ms.topic: article
ms.date: 06/18/2019
ms.reviewer: dariac
ms.custom: seodec18
ms.openlocfilehash: efe4c07a6231e0b2c95b049db056a4e5d055db98
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77153001"
---
# <a name="local-git-deployment-to-azure-app-service"></a>Azure Uygulama Hizmetine Yerel Git dağıtımı

Bu nasıl yapılsa kılavuzu, uygulamanızı yerel bilgisayarınızdaki bir Git deposundan [Azure Uygulama Hizmetine](overview.md) nasıl dağıtabileceğinizi gösterir.

## <a name="prerequisites"></a>Ön koşullar

Bu nasıl yapIlir kılavuzundaki adımları izlemek için:

- [!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]
  
- [Git'i yükleyin.](https://www.git-scm.com/downloads)
  
- Dağıtmak istediğiniz kodiçeren yerel bir Git deposuna sahip. Örnek bir depo indirmek için yerel terminal pencerenizde aşağıdaki komutu çalıştırın:
  
  ```bash
  git clone https://github.com/Azure-Samples/nodejs-docs-hello-world.git
  ```

[!INCLUDE [Prepare repository](../../includes/app-service-deploy-prepare-repo.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="deploy-with-kudu-build-server"></a>Kudu build sunucusu ile dağıt

Kudu App Service build sunucusuyla uygulamanız için yerel Git dağıtımını etkinleştirmenin en kolay yolu Azure Bulut Shell'i kullanmaktır. 

### <a name="configure-a-deployment-user"></a>Dağıtım kullanıcısı yapılandırma

[!INCLUDE [Configure a deployment user](../../includes/configure-deployment-user-no-h.md)]

### <a name="get-the-deployment-url"></a>Dağıtım URL'sini alın

Mevcut bir uygulama için yerel Git dağıtımını [`az webapp deployment source config-local-git`](/cli/azure/webapp/deployment/source?view=azure-cli-latest#az-webapp-deployment-source-config-local-git) etkinleştirmek için URL'yi almak için Bulut Kabuğu'nda çalıştırın. Uygulama \<adı> \<ve grup adı> uygulamanızın adlarını ve Azure kaynak grubunun adlarını değiştirin.

```azurecli-interactive
az webapp deployment source config-local-git --name <app-name> --resource-group <group-name>
```
> [!NOTE]
> Linux uygulama hizmeti planı kullanıyorsanız, şu parametreyi eklemeniz gerekir: --runtime python|3.7


Veya Git özellikli yeni bir uygulama [`az webapp create`](/cli/azure/webapp?view=azure-cli-latest#az-webapp-create) oluşturmak için `--deployment-local-git` Bulut Kabuğu'nda parametreyle birlikte çalıştırın. Uygulama \<adı>, \<grup adı> \<ve plan adı> yeni Git uygulamanızın, Azure kaynak grubunuzun ve Azure Uygulama Hizmeti planının adlarıyla değiştirin.

```azurecli-interactive
az webapp create --name <app-name> --resource-group <group-name> --plan <plan-name> --deployment-local-git
```

Ya komutu gibi `https://<deployment-username>@<app-name>.scm.azurewebsites.net/<app-name>.git`bir URL döndürür: . Uygulamanızı bir sonraki adımda dağıtmak için bu URL'yi kullanın.

Bu hesap düzeyindeki URL'yi kullanmak yerine, uygulama düzeyindekimlik bilgilerini kullanarak yerel Git'i de etkinleştirebilirsiniz. Azure Uygulama Hizmeti, her uygulama için bu kimlik bilgilerini otomatik olarak oluşturur. 

Bulut Kabuğu'nda aşağıdaki komutu çalıştırarak uygulama kimlik bilgilerini alın. Uygulama \<adı> \<ve grup adı> uygulamanızın adı ve Azure kaynak grup adı ile değiştirin.

```azurecli-interactive
az webapp deployment list-publishing-credentials --name <app-name> --resource-group <group-name> --query scmUri --output tsv
```

Uygulamanızı bir sonraki adımda dağıtmak için dönen URL'yi kullanın.

### <a name="deploy-the-web-app"></a>Web uygulamasını dağıtma

1. Yerel Git deponuza yerel bir terminal penceresi açın ve bir Azure uzaktan kumandası ekleyin. Aşağıdaki komutta, \<url> önceki adımdan aldığınız dağıtım kullanıcıya özgü URL veya uygulamaya özgü URL ile değiştirin.
   
   ```bash
   git remote add azure <url>
   ```
   
1. `git push azure master`'ile Azure uzaktan kumandasına itin. 
   
1. Git **Kimlik Bilgisi Yöneticisi** penceresinde, Azure oturum açma parolanızı [değil, dağıtım kullanıcı parolanızı](#configure-a-deployment-user)girin.
   
1. Çıktıyı gözden geçirin. ASP.NET için MSBuild, `npm install` Node.js ve `pip install` Python için çalışma süresine özgü otomasyonu görebilirsiniz. 
   
1. İçeriğin dağıtılmış olduğunu doğrulamak için Azure portalındaki uygulamanıza göz atın.

## <a name="deploy-with-azure-pipelines-builds"></a>Azure Pipelines ile dağıt'lar oluşturur

Hesabınızda gerekli izinler varsa, uygulamanız için yerel Git dağıtımını etkinleştirmek için Azure Ardışık Hatları (Önizleme) ayarlayabilirsiniz. 

- Azure hesabınız, Azure Etkin Dizin'e yazma ve hizmet oluşturma izinlerine sahip olmalıdır. 
  
- Azure hesabınız, Azure aboneliğinizde **Sahip** rolüne sahip olmalıdır.

- Kullanmak istediğiniz Azure DevOps projesinde yönetici olmalısınız.

Azure Ardışık Hatları (Önizleme) ile uygulamanız için yerel Git dağıtımını etkinleştirmek için:

1. Azure [portalında,](https://portal.azure.com) **Uygulama Hizmetlerini**arayın ve seçin. 

1. Azure Uygulama Hizmeti uygulamanızı seçin ve sol menüde **Dağıtım Merkezi'ni** seçin.
   
1. Dağıtım **Merkezi** sayfasında Yerel **Git'i**seçin ve ardından **Devam et'i**seçin. 
   
   ![Yerel Git'i seçin ve ardından Devam et'i seçin](media/app-service-deploy-local-git/portal-enable.png)
   
1. Sağlayıcı **Oluştur** sayfasında **Azure Ardışık Hatları (Önizleme) seçeneğini**belirleyin ve ardından Devam **et'i**seçin. 
   
   ![Azure Ardışık Hatları (Önizleme) seçeneğini belirleyin ve ardından Devam et'i seçin.](media/app-service-deploy-local-git/pipeline-builds.png)

1. **Yapılandırılan** sayfada, yeni bir Azure DevOps kuruluşunu yapılandırın veya varolan bir kuruluş belirtin ve ardından **Devam et'i**seçin.
   
   > [!NOTE]
   > Mevcut Azure DevOps kuruluşunuz listede yoksa, azure aboneliğinize bağlamanız gerekebilir. Daha fazla bilgi için cd [sürüm ardışık alanınızı tanımlayın'](/azure/devops/pipelines/apps/cd/deploy-webdeploy-webapps#cd)a bakın.
   
1. Uygulama Hizmeti planı [fiyatlandırma katmanınıza](https://azure.microsoft.com/pricing/details/app-service/plans/)bağlı olarak, **bir Hazırlama sayfasına Dağıt'ı** görebilirsiniz. [Dağıtım yuvalarını etkinleştirip etkinleştirmeyeceğini](deploy-staging-slots.md)seçin ve sonra **Devam et'i**seçin.
   
1. **Özet** sayfasında, ayarları gözden geçirin ve ardından **Bitir'i**seçin.
   
1. Azure Ardışık Alanı hazır olduğunda, bir sonraki adımda kullanmak üzere **Dağıtım Merkezi** sayfasından Git deposu URL'sini kopyalayın. 
   
   ![Git deposu URL'sini kopyalama](media/app-service-deploy-local-git/vsts-repo-ready.png)

1. Yerel terminal pencerenizde, yerel Git deponuza bir Azure uzaktan kumandası ekleyin. Komutta, url \<> önceki adımdan aldığınız Git deposunun URL'si ile değiştirin.
   
   ```bash
   git remote add azure <url>
   ```
   
1. `git push azure master`'ile Azure uzaktan kumandasına itin. 
   
1. Git **Kimlik Bilgileri Yöneticisi** sayfasında, visualstudio.com kullanıcı adınız ile oturum açın. Diğer kimlik doğrulama yöntemleri için [Azure DevOps Hizmetleri kimlik doğrulama genel bakışına](/vsts/git/auth-overview?view=vsts)bakın.
   
1. Dağıtım tamamlandıktan sonra, yapı `https://<azure_devops_account>.visualstudio.com/<project_name>/_build`ilerlemesini ve dağıtım `https://<azure_devops_account>.visualstudio.com/<project_name>/_release`devamını ' da görüntüleyin.
   
1. İçeriğin dağıtılmış olduğunu doğrulamak için Azure portalındaki uygulamanıza göz atın.

[!INCLUDE [What happens to my app during deployment?](../../includes/app-service-deploy-atomicity.md)]

## <a name="troubleshoot-deployment"></a>Sorun giderme dağıtımı

Azure'da bir Uygulama Hizmeti uygulamasında yayımlamak için Git'i kullandığınızda aşağıdaki yaygın hata iletilerini görebilirsiniz:

|İleti|Nedeni|Çözüm
---|---|---|
|`Unable to access '[siteURL]': Failed to connect to [scmAddress]`|Uygulama çalışır durumda değil.|Uygulamayı Azure portalında başlatın. Web uygulaması durdurulduğunda Git dağıtımı kullanılamaz.|
|`Couldn't resolve host 'hostname'`|'Azure' uzaktan kumandanın adres bilgileri yanlış.|İlişkili `git remote -v` URL ile birlikte tüm uzaktan kumandaları listelemek için komutu kullanın. 'Azure' uzaktan kumandanın URL'sinin doğru olduğunu doğrulayın. Gerekirse, doğru URL'yi kullanarak bu uzaktan kumandayı kaldırın ve yeniden oluşturun.|
|`No refs in common and none specified; doing nothing. Perhaps you should specify a branch such as 'master'.`|Sırasında bir dal `git push`belirtmedin veya ''de `push.default` `.gitconfig`değeri ayarlamadın.|Ana `git push` dalı belirterek yeniden `git push azure master`çalıştırın: .|
|`src refspec [branchname] does not match any.`|'Azure' kumandası üzerinde ana makineden başka bir dala itmeye çalıştınız.|Ana `git push` dalı belirterek yeniden `git push azure master`çalıştırın: .|
|`RPC failed; result=22, HTTP code = 5xx.`|Bu hata, HTTPS üzerinde büyük bir git deposu zorlamaya çalışırsanız gerçekleşebilir.|Daha büyük yapmak için yerel makinede git yapılandırmasını değiştirin. `postBuffer` Örneğin: `git config --global http.postBuffer 524288000`.|
|`Error - Changes committed to remote repository but your web app not updated.`|Bir Node.js uygulamasını, gerekli ek modülleri belirten bir _package.json_ dosyasıyla dağıttınız.|Hata `npm ERR!` yla ilgili daha fazla bağlam için bu hatadan önce hata iletilerini gözden geçirin. Bu hatanın bilinen nedenleri ve ilgili `npm ERR!` iletiler şunlardır:<br /><br />**Malformed package.json dosyası**:`npm ERR! Couldn't read dependencies.`<br /><br />**Yerel modülün Windows için ikili bir dağıtımı yoktur:**<br />`npm ERR! \cmd "/c" "node-gyp rebuild"\ failed with 1` <br />or <br />`npm ERR! [modulename@version] preinstall: \make || gmake\ `|

## <a name="additional-resources"></a>Ek kaynaklar

- [Proje Kudu belgeleri](https://github.com/projectkudu/kudu/wiki)
- [Azure Uygulama Hizmetine sürekli dağıtım](deploy-continuous-deployment.md)
- [Örnek: Yerel bir Git deposundan (Azure CLI) bir web uygulaması oluşturun ve kod dağıtın](./scripts/cli-deploy-local-git.md?toc=%2fcli%2fazure%2ftoc.json)
- [Örnek: Bir web uygulaması oluşturun ve yerel bir Git deposundan (PowerShell) kod dağıtın](./scripts/powershell-deploy-local-git.md?toc=%2fpowershell%2fmodule%2ftoc.json)
