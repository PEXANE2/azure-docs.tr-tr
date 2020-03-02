---
title: GatsbyJS ve Azure DevOps Projeleri ile PWA için bir CI/CD işlem hattı oluşturun
description: DevOps Projeleri Azure 'u kullanmaya başlamanızı kolaylaştırır. Birkaç hızlı adımda, tercih ettiğiniz bir Azure hizmetinde uygulama başlatmanıza yardımcı olur.
ms.prod: devops
ms.technology: devops-cicd
services: vsts
documentationcenter: vs-devops-build
author: arob98
manager: angrobe
editor: ''
ms.assetid: ''
ms.workload: web
ms.tgt_pltfrm: na
ms.topic: quickstart
ms.date: 02/24/2020
ms.author: angrobe
ms.custom: mvc
monikerRange: vsts
ms.openlocfilehash: 508a61d6bbb00692855e09601aed67ab3be9cc8d
ms.sourcegitcommit: 5192c04feaa3d1bd564efe957f200b7b1a93a381
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/02/2020
ms.locfileid: "78209076"
---
#  <a name="quickstart-create-a-cicd-pipeline-in-azure-pipelines-for-nodejs-with-azure-devops-projects"></a>Hızlı başlangıç: Azure DevOps Projeleri ile Node. js için Azure Pipelines bir CI/CD işlem hattı oluşturma
Bu hızlı başlangıçta, [Gatsbyjs](https://www.gatsbyjs.org/) ve Basitleştirilmiş Azure DevOps proje oluşturma deneyimini kullanarak bir NodeJS aşamalı Web UYGULAMASı (PWA) oluşturacaksınız. İşiniz bittiğinde, Azure Pipelines ' de PWA 'niz için sürekli tümleştirme (CI) ve sürekli teslim (CD) işlem hattı vardır. Azure DevOps Projeleri Geliştirme, dağıtma ve izleme için gerekenleri ayarlar.

## <a name="prerequisites"></a>Önkoşullar

- Etkin aboneliği olan bir Azure hesabı. [Ücretsiz hesap oluşturun](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio). 
- [Azure DevOps](https://azure.microsoft.com/services/devops/) organizasyonu.

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın

DevOps Projeleri, Azure Pipelines bir CI/CD işlem hattı oluşturur. Yeni bir Azure DevOps kuruluşu oluşturabilir veya var olan bir kuruluşu kullanabilirsiniz. DevOps projeleri, Azure kaynaklarını da tercih ettiğiniz bir Azure aboneliği oluşturur.

1. [Azure Portal](https://portal.azure.com)oturum açın ve sol bölmede **kaynak oluştur**' u seçin. 

   ![Azure portal Azure kaynağı oluşturma](_img/azure-devops-project-nodejs/create-azure-resource.png)

2. **DevOps Projeleri**bulun ve seçin ve ardından **Oluştur**' u seçin.

 ![DevOps projesi oluşturma](_img/azure-devops-project-nodejs/create-devops-project.png) 

## <a name="select-a-sample-application-and-azure-service"></a>Örnek uygulama ve Azure hizmeti seçme

1. Node. js örnek uygulamasını seçin.   

 ![Node. js örneğini seçin](_img/azure-devops-project-nodejs/select-nodejs-devops-project.png) 

2. Varsayılan örnek çerçeve **Express.js**’dir. Seçimi **basit Node. js uygulamasıyla** değiştirin ve ardından **İleri**' yi seçin. 

 ![Basit Node. js uygulamasını seçin](_img/azure-devops-project-nodejs/simple-nodejs-project.png) 

3. Bu adımda sunulan dağıtım hedefleri, adım 2 ' de seçilen uygulama çerçevesi tarafından belirlenir.  Bu örnekte, varsayılan dağıtım hedefi **Windows Web uygulamasıdır** .  **Kapsayıcılar için Web App** kümeyi bırakın ve **İleri ' yi**seçin.

 ![Dağıtım hedefini seçin](_img/azure-devops-project-nodejs/select-web-server.png) 

## <a name="configure-a-project-name-and-an-azure-subscription"></a>Proje adı ve Azure aboneliği yapılandırma

1. DevOps proje oluşturma iş akışının son adımında, bir proje adı atar, bir Azure aboneliği seçin ve **bitti**' yi seçin.  

 ![Proje adı atama ve abonelik seçme](_img/azure-devops-project-nodejs/assign-project-name.png) 

2. Projeniz derlenirken ve uygulamanız Azure 'a dağıtıldığında bir Özet sayfası görüntülenir. Kısa bir süre sonra, [Azure DevOps kuruluşunuzda](https://dev.azure.com/) bir git deposu, Kanban panosu, bir dağıtım işlem hattı, test planları ve uygulamanız için gereken yapıtları içeren bir proje oluşturulur.  

## <a name="managing-your-project"></a>Projenizi yönetme

1. **Tüm kaynaklara** gidin ve DevOps projenizi bulun. **DevOps projenizi**seçin.

![Kaynak listesinde Azure DevOps panosu](_img/azure-devops-project-nodejs/azure-devops-project-in-resource-list.png)

2. Proje giriş sayfanız, kod deposu, CI/CD işlem hattı ve çalışan uygulamanıza yönelik bir bağlantı için görünürlük sağlayan bir panoya yönlendirilirsiniz. Uygulamanızı **Azure DevOps** 'da görüntülemek Için **proje giriş sayfasını** seçin ve başka bir tarayıcı sekmesinde, canlı örnek uygulamayı görüntülemek için **uygulama uç noktasını** seçin.  Bu örneği daha sonra GatsbyJS tarafından oluşturulan PWA kullanacak şekilde değiştiririz.

![Azure DevOps panosu](_img/azure-devops-project-nodejs/devops-projects-dashboard.png) 

3. Azure DevOps projenizden, ekip üyelerini işbirliği yapmak ve işinizi izlemeye başlamak için bir Kanban panosu kurmak üzere davet edebilirsiniz.  Daha fazla bilgi için [buraya](https://docs.microsoft.com/azure/devops/user-guide/what-is-azure-devops?view=azure-devops)bakın.

![Azure DevOps genel bakış](_img/azure-devops-project-nodejs/azure-devops-overview.png)

## <a name="clone-the-repo-and-install-your-gatsby-pwa"></a>Depoyu kopyalama ve Gatsınızı PWA 'dan yüklemesi

DevOps Projeleri, Azure Repos veya GitHub 'da bir git deposu oluşturur. Bu örnek, bir Azure deposu oluşturdu.  Sonraki adım, depoyu klonlamadır ve değişiklikler yapar.

1. **DevOps projenizden** **depoları** seçin ve ardından **Kopyala**' ya tıklayın.  Git deposunu masaüstünüze kopyalamak için çeşitli mekanizmalar vardır.  Geliştirme deneyiminize uygun olanı seçin.  

![Depoyu kopyalama](_img/azure-devops-project-nodejs/clone-the-repo.png)

2. Depo, masaüstünüze kopyalandıktan sonra, başlangıç şablonunda bazı değişiklikler yapın. Terminalinizden GatsbyJS CLı 'yı yükleyerek başlayın.
```powershell
npm install -g gatsby
```

3. Terminalden deponuzın köküne gidin. Şöyle görünen üç klasör içermelidir:
```powershell
Mode                LastWriteTime         Length Name
----                -------------         ------ ----
d-----        2/23/2020  10:42 PM                Application
d-----        2/23/2020   3:05 PM                ArmTemplates
d-----        2/23/2020   3:05 PM                Tests
```

4. Uygulama klasöründeki tüm dosyaları bir Gatsby Starter ile değiştirecek şekilde istemiyoruz. Aşağıdaki komutları sırayla kesmek için sırayla çalıştırın.
```powershell
cp .\Application\Dockerfile .
rmdir Application
```

5. Örnek bir PWA oluşturmak için Gatsby CLı kullanın. PWA Sihirbazı ' nı başlatmak için terminalden `gatsby new` çalıştırın ve başlangıç şablonunuz için `gatsby-starter-blog` seçin. Bu örneğe benzemelidir:
```powershell
c:\myproject> gatsby new
√ What is your project called? ... my-gatsby-project
? What starter would you like to use? » - Use arrow-keys. Return to submit.
    gatsby-starter-default
    gatsby-starter-hello-world
>   gatsby-starter-blog
    (Use a different starter)
```

6. Artık `my-gatsby-project`adlı bir klasörünüz var. `Application` olarak yeniden adlandırın ve `Dockerfile` içine kopyalayın.
```powershell
mv my-gatsby-project Application
mv Dockerfile Application
```

7. En sevdiğiniz düzenleyicide Dockerfile dosyasını açın ve `FROM node:8` ilk satırı `FROM node:12`olarak değiştirin. Bu değişiklik, kapsayıcının 8. x yerine Node. js sürüm 12. x sürümünü kullanmasını sağlar. GatsbyJS, Node. js ' nin daha modern sürümlerini gerektirir.

8. Daha sonra, uygulama klasöründe Package. json dosyasını açın ve [komut dosyaları alanını](https://docs.npmjs.com/files/package.json#scripts) düzenleyerek geliştirme ve üretim sunucularınızın tüm kullanılabilir ağ arabirimlerini (örneğin 0.0.0.0) ve bağlantı noktası 80 ' i dinlemesine emin olun. Bu ayarlar olmadan, Container App Service, kapsayıcının içinde çalışan Node. js uygulamanıza trafiği yönlendiremeyen bir uygulamadır. `scripts` alan aşağıdaki gibi olmalıdır. Özellikle, `develop`, `serve`ve `start` hedeflerini varsayılandan değiştirmek istiyorsunuz.
```json
  "scripts": {
    "build": "gatsby build",
    "develop": "gatsby develop  -H 0.0.0.0 -p 80",
    "format": "prettier --write \"**/*.{js,jsx,json,md}\"",
    "start": "npm run serve",
    "serve": "npm run build && gatsby serve -H 0.0.0.0 -p 80",
    "clean": "gatsby clean",
    "test": "echo \"Write tests! -> https://gatsby.dev/unit-testing\" && exit 1"
  }
```

## <a name="edit-your-cicd-pipelines"></a>CI/CD işlem hatlarınızı düzenleyin

1. Önceki bölümde kodu işlemeden önce, derleme ve yayın işlem hatlarınız üzerinde bazı değişiklikler yapın. ' Derleme işlem hattınızı düzenleyin ve Node. js sürüm 12. x ' i kullanmak için düğüm görevini güncelleştirin. **Görev sürümü** alanını 1. x ve **Sürüm** alanı olarak 12. x olarak ayarlayın.
Node. js ' ye güncelleştirme ![](_img/azure-devops-project-nodejs/build-pipeline-update-node.png)

2. Bu hızlı başlangıçta, birim testleri oluşturmadık ve derleme işlem hattımızda bu adımları devre dışı bıraktık. Testleri yazdığınızda, bu adımları yeniden etkinleştirebilirsiniz. **Test bağımlılıklarını yüklemek** ve **birim testlerini çalıştırmak** ve devre dışı bırakmak için sağ tıklayın.

![Yapı testlerini devre dışı bırak](_img/azure-devops-project-nodejs/disable-build-unittests.png)

3. Yayın işlem hattınızı düzenleyin.
![yayın işlem hattını düzenleyin](_img/azure-devops-project-nodejs/edit-release-pipeline.png)

4. Yapı ardışık düzeninde olduğu gibi, düğüm görevini 12. x kullanacak şekilde değiştirin ve iki test görevini devre dışı bırakın. Yayınlarınızın bu ekran görüntüsüne benzemesi gerekir.

![Yayın işlem hattı tamamlandı](_img/azure-devops-project-nodejs/release-pipeline-complete.png)

1. Tarayıcının sol tarafında **Görünümler/index. Pug** dosyasına gidin.

1. **Düzenle**' yi seçin ve ardından H2 başlığında bir değişiklik yapın.  
    Örneğin, **Azure DevOps Projeleri ile hemen kullanmaya** başlayın yazın veya başka bir değişiklik yapın.

1. **Yürüt**' ü seçin ve ardından değişikliklerinizi kaydedin.

1. Tarayıcınızda DevOps Projeleri panosuna gidin.   
Artık sürmekte olan bir derleme görmeniz gerekir. Yaptığınız değişiklikler otomatik olarak bir CI/CD işlem hattı aracılığıyla oluşturulup dağıtılır.

## <a name="commit-your-changes-and-examine-the-azure-cicd-pipeline"></a>Değişikliklerinizi işleyin ve Azure CI/CD işlem hattını inceleyin

Önceki iki adımda, git deponuza bir Gatsby tarafından üretilen PWA eklediniz ve kodu derlemek ve dağıtmak için işlem hatlarınızı düzenlediniz. Kodu teslim edebilir ve derleme ve yayın işlem hattı aracılığıyla ilerlemeyi izleyebilirsiniz.

1. Projenizin git deposunun kökünden bir terminalde, kodunuzu Azure DevOps projenize göndermek için aşağıdaki komutları çalıştırın:
```powershell
git add .
git commit -m "My first Gatsby PWA"
git push
```

2. `git push` tamamlandıktan hemen sonra bir derleme başlatılır. **Azure DevOps panosundan**ilerlemeyi izleyebilirsiniz.

![Kaynak listesinde Azure DevOps panosu](_img/azure-devops-project-nodejs/azure-devops-project-in-resource-list.png)

3. Birkaç dakika sonra, derleme ve yayın işlem hatlarınız bitirilmeli ve PWA 'nın bir kapsayıcıya dağıtılması gerekir. Yukarıdaki panodan **uygulama uç noktası** bağlantısına tıklayın ve Bloglar Için bir Gatsby Starter projesi görmeniz gerekir.



## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık kaynaklara gerek duymadığınızda oluşturduğunuz Azure App Service ve diğer ilgili kaynakları silebilirsiniz. DevOps Projeleri panosundaki **silme** işlevini kullanın.


## <a name="next-steps"></a>Sonraki adımlar

CI/CD işleminizi yapılandırdığınızda, derleme ve yayın işlem hatları otomatik olarak oluşturulur. Bu derleme ve yayın işlem hatlarını takımınızın ihtiyaçlarını karşılayacak şekilde değiştirebilirsiniz. CI/CD işlem hattı hakkında daha fazla bilgi edinmek için bkz.:

> [!div class="nextstepaction"]
> [CD işlemini özelleştirme](https://docs.microsoft.com/azure/devops/pipelines/release/define-multistage-release-process?view=vsts)

