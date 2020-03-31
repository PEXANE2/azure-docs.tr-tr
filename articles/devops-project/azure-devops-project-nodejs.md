---
title: GatsbyJS ve Azure DevOps Projeleri ile bir PWA için CI/CD ardışık
description: DevOps Projects, Azure'a başlamayı kolaylaştırır. Birkaç hızlı adımda, tercih ettiğiniz bir Azure hizmetinde uygulama başlatmanıza yardımcı olur.
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
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "78209076"
---
#  <a name="quickstart-create-a-cicd-pipeline-in-azure-pipelines-for-nodejs-with-azure-devops-projects"></a>Hızlı başlatma: Azure DevOps Projeleri ile Node.js için Azure Ardışık Hatlarında BIR CI/CD ardışık kaynak oluşturma
Bu hızlı başlangıçta, [GatsbyJS](https://www.gatsbyjs.org/) ve basitleştirilmiş Azure DevOps proje oluşturma deneyimini kullanarak bir NodeJS aşamalı web uygulaması (PWA) oluşturursunuz. Tamamlandığında, Azure Pipelines'daki PWA'nız için sürekli tümleştirme (CI) ve sürekli teslimat (CD) ardışık hattınız olur. Azure DevOps Projeleri, geliştirme, dağıtma ve izleme için ihtiyacınız olanı ayarlar.

## <a name="prerequisites"></a>Ön koşullar

- Etkin bir aboneliği olan bir Azure hesabı. [Ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) 
- Azure [DevOps](https://azure.microsoft.com/services/devops/) kuruluşu.

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın

DevOps Projects, Azure Ardışık Hatları'nda bir CI/CD ardışık alanı oluşturur. Yeni bir Azure DevOps kuruluşu oluşturabilir veya varolan bir kuruluş kullanabilirsiniz. DevOps Projects, seçtiğiniz Azure aboneliğinde Azure kaynakları da oluşturur.

1. [Azure portalında](https://portal.azure.com)oturum açın ve sol bölmede **kaynak oluştur'u**seçin. 

   ![Azure portalında Azure kaynağı oluşturma](_img/azure-devops-project-nodejs/create-azure-resource.png)

2. **DevOps Projelerini**arayın ve seçin ve ardından **Oluştur'u**seçin.

 ![DevOps Projesi Oluşturma](_img/azure-devops-project-nodejs/create-devops-project.png) 

## <a name="select-a-sample-application-and-azure-service"></a>Örnek uygulama ve Azure hizmeti seçme

1. Node.js örnek uygulamasını seçin.   

 ![Düğüm.js örneğini seçin](_img/azure-devops-project-nodejs/select-nodejs-devops-project.png) 

2. Varsayılan örnek çerçeve **Express.js**’dir. Seçimi Simple **Node.js Uygulaması** olarak değiştirin ve ardından **İleri'yi**seçin. 

 ![Basit Düğüm.js Uygulamasını Seçin](_img/azure-devops-project-nodejs/simple-nodejs-project.png) 

3. Bu adımda kullanılabilen dağıtım hedefleri, adım 2'de seçilen uygulama çerçevesi tarafından belirlenir.  Bu örnekte, **Windows Web Uygulaması** varsayılan dağıtım hedefidir.  **Kapsayıcılar** kümesi için Web Uygulamasını bırakın ve **İleri'yi**seçin.

 ![Dağıtım hedefini seçin](_img/azure-devops-project-nodejs/select-web-server.png) 

## <a name="configure-a-project-name-and-an-azure-subscription"></a>Proje adını ve Azure aboneliğini yapılandırma

1. DevOps proje oluşturma iş akışının son adımında, bir proje adı atarsınız, bir Azure aboneliği seçin ve **Bitti'yi**seçin.  

 ![Proje adı atama ve abonelik seçme](_img/azure-devops-project-nodejs/assign-project-name.png) 

2. Projeniz oluşturulurken ve uygulamanız Azure'a dağıtılırken bir özet sayfası görüntülenir. Kısa bir süre sonra, Azure [DevOps kuruluşunuzda](https://dev.azure.com/) git repo, Kanban panosu, dağıtım ardışık hattı, test planları ve uygulamanızın gerektirdiği yapıları içeren bir proje oluşturulur.  

## <a name="managing-your-project"></a>Projenizi yönetme

1. Tüm **Kaynaklara** gidin ve DevOps Projenizi bulun. **DevOps Projenizi**seçin.

![Kaynak Listesindeki Azure DevOps Panosu](_img/azure-devops-project-nodejs/azure-devops-project-in-resource-list.png)

2. Proje ana sayfanızda, kod deponuzda, CI/CD ardışık sisteminizde ve çalışan uygulamanızın bağlantısını gösteren bir panoya yönlendirilirsiniz. Uygulamanızı **Azure DevOps'te** görüntülemek için **Project Ana Sayfasını** seçin ve başka bir tarayıcı sekmesinde canlı örnek uygulamayı görüntülemek için Uygulama Bitiş **Noktası'nı** seçin.  Bu örneği daha sonra GatsbyJS tarafından oluşturulan PWA'yı kullanmak üzere değiştiriyoruz.

![Azure Devops Pano](_img/azure-devops-project-nodejs/devops-projects-dashboard.png) 

3. Azure DevOps projenizden, ekip üyelerini işbirliği yapmaya ve çalışmanızı izlemeye başlamak için bir Kanban panosu oluşturmaya davet edebilirsiniz.  Daha fazla bilgi için [buraya](https://docs.microsoft.com/azure/devops/user-guide/what-is-azure-devops?view=azure-devops)bakın.

![Azure Devops Genel Bakış](_img/azure-devops-project-nodejs/azure-devops-overview.png)

## <a name="clone-the-repo-and-install-your-gatsby-pwa"></a>Repo'yu kopyala ve Gatsby PWA'nızı kurun

DevOps Projects, Azure Repos veya GitHub'da bir git deposu oluşturur. Bu örnek, bir Azure Repo oluşturdu.  Bir sonraki adım repo klonlamak ve değişiklikler yapmaktır.

1. **DevOps Projenizden** **Repos'u** seçin ve ardından **Klon'u**tıklatın.  Masaüstünüze git repo klonlamak için çeşitli mekanizmalar vardır.  Geliştirme deneyiminize uygun olanı seçin.  

![Depoyu kopyalama](_img/azure-devops-project-nodejs/clone-the-repo.png)

2. Repo masaüstünüze klonlandıktan sonra başlangıç şablonunda bazı değişiklikler yapın. Terminalinizden GatsbyJS CLI'yi yükleyerek başlayın.
```powershell
npm install -g gatsby
```

3. Terminalden repo'nuzun köküne gidin. Şuna benzeyen üç klasör içermelidir:
```powershell
Mode                LastWriteTime         Length Name
----                -------------         ------ ----
d-----        2/23/2020  10:42 PM                Application
d-----        2/23/2020   3:05 PM                ArmTemplates
d-----        2/23/2020   3:05 PM                Tests
```

4. Uygulama klasöründeki tüm dosyaları istemiyoruz çünkü bir Gatsby başlatıcısı ile değiştireceğiz. Kırpmak için aşağıdaki komutları sırayla çalıştırın.
```powershell
cp .\Application\Dockerfile .
rmdir Application
```

5. Örnek bir PWA oluşturmak için Gatsby CLI'yi kullanın. PWA sihirbazına başlamak için terminalden çalıştırın `gatsby new` ve başlangıç şablonunuz için seçin. `gatsby-starter-blog` Bu örneğe benzemeli:
```powershell
c:\myproject> gatsby new
√ What is your project called? ... my-gatsby-project
? What starter would you like to use? » - Use arrow-keys. Return to submit.
    gatsby-starter-default
    gatsby-starter-hello-world
>   gatsby-starter-blog
    (Use a different starter)
```

6. Artık bir klasörvar. `my-gatsby-project` Yeniden adlandırın `Application` ve `Dockerfile` kopyalayın.
```powershell
mv my-gatsby-project Application
mv Dockerfile Application
```

7. En sevdiğiniz düzenleyicide Dockerfile'ı açın ve `FROM node:8` `FROM node:12`ilk satırı 'dan ' a' olarak değiştirin Bu değişiklik, kapsayıcınızın sürüm 8.x yerine Node.js sürüm 12.x kullanmasını sağlar. GatsbyJS Node.js daha modern sürümleri gerektirir.

8. Ardından, Uygulama klasöründeki package.json dosyasını açın ve geliştirme ve üretim sunucularınızın kullanılabilir tüm ağ arabirimlerini (örneğin, 0.0.0.0) ve bağlantı noktası 80'de dinlemesini sağlamak için [komut dosyaları alanını](https://docs.npmjs.com/files/package.json#scripts) edin. Bu ayarlar olmadan, konteyner uygulaması hizmeti trafiği konteynırınızın içinde çalışan Node.js uygulamanıza yönlendiremez. Alan `scripts` aşağıdakine benzemelidir. Özellikle, `develop`,, `serve`ve `start` hedefleri varsayılanlarından değiştirmek istiyorsunuz.
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

## <a name="edit-your-cicd-pipelines"></a>CI/CD ardışık hatlarınızı edin

1. Önceki bölümde kodu işlemeden önce, yapı ve sürüm ardışık hatlarınızda bazı değişiklikler yapın. 'Yapı Hattı' nızı edin ve Düğüm görevini düğüm.js sürüm 12.x'i kullanmak üzere güncelleştirin. Görev **sürüm** alanını 1.x ve **Sürüm** alanını 12.x olarak ayarlayın.
![Node.js'yi 12.x'e güncelleştir](_img/azure-devops-project-nodejs/build-pipeline-update-node.png)

2. Bu hızlı başlangıçta, birim testleri oluşturmuyoruz ve yapı boru hattımızdaki adımları devre dışı bırakıyoruz. Testler yazarken, bu adımları yeniden etkinleştirebilirsiniz. **Test bağımlılıklarını yükley** ve birim **testlerini çalıştır** ve bunları devre dışı bırak etiketli görevleri seçmek için sağ tıklatın.

![Yapı Testlerini Devre Dışı](_img/azure-devops-project-nodejs/disable-build-unittests.png)

3. Sürüm ardışık sisteminizi edin.
![Yayın Ardışık Hattını Edin](_img/azure-devops-project-nodejs/edit-release-pipeline.png)

4. Yapı ardışık yapısında olduğu gibi, Düğüm görevini 12.x kullanmak ve iki test görevini devre dışı etmek için değiştirin. Sürümünüzün bu ekran görüntüsüne benzemesi gerekir.

![Tamamlanmış Sürüm Boru Hattı](_img/azure-devops-project-nodejs/release-pipeline-complete.png)

1. Tarayıcının sol tarafında **görünümler/index.pug** dosyasına gidin.

1. **Edit'i**seçin ve ardından h2 başlığında değişiklik yapın.  
    Örneğin, **Azure DevOps Projeleri ile hemen başlayın'ı** girin veya başka değişiklikler yapın.

1. **Commit'i**seçin ve değişikliklerinizi kaydedin.

1. Tarayıcınızda DevOps Projects panosuna gidin.   
Şimdi devam etmekte olan bir yapı görmeniz gerekir. Yaptığınız değişiklikler otomatik olarak oluşturulur ve bir CI/CD ardışık boru hattı üzerinden dağıtılır.

## <a name="commit-your-changes-and-examine-the-azure-cicd-pipeline"></a>Değişikliklerinizi gerçekleştirin ve Azure CI/CD ardışık hattını inceleyin

Önceki iki adımda, git repo'nuza bir Gatsby oluşturulan PWA eklediniz ve kodu oluşturmak ve dağıtmak için ardışık hatlarınızı düzenlediniz. Kodu işleyebilir ve yapı ve serbest bırakma boru hattında ilerlemesini izleyebiliriz.

1. Projenizin bir terminaldeki git repo'sundan, kodunuzu Azure DevOps projenize itmek için aşağıdaki komutları çalıştırın:
```powershell
git add .
git commit -m "My first Gatsby PWA"
git push
```

2. Bir yapı tamamlanır `git push` tamamlanmaz başlatılır. Azure **DevOps Panosu'ndan**ilerlemeyi takip edebilirsiniz.

![Kaynak Listesindeki Azure DevOps Panosu](_img/azure-devops-project-nodejs/azure-devops-project-in-resource-list.png)

3. Birkaç dakika sonra, yapı ve sürüm ardışık hatlarınız bitmeli ve PWA'nız bir kapsayıcıya dağıtılmalıdır. Yukarıdaki panodan **Uygulama bitiş noktası** bağlantısını tıklatın ve bloglar için bir Gatsby başlangıç projesi görmeniz gerekir.



## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık kaynaklara ihtiyacınız olmadığında Azure Uygulama Hizmeti'ni ve oluşturduğunuz diğer ilgili kaynakları silebilirsiniz. DevOps Projeleri panosundaki **Sil** işlevini kullanın.


## <a name="next-steps"></a>Sonraki adımlar

CI/CD işleminizi yapılandırdığınızda, yapı ve serbest ardışık hatlar otomatik olarak oluşturulur. Ekibinizin gereksinimlerini karşılamak için bu yapı ve sürüm ardışık hatlarını değiştirebilirsiniz. CI/CD ardışık boru hattı hakkında daha fazla bilgi için bkz:

> [!div class="nextstepaction"]
> [CD işlemini özelleştirme](https://docs.microsoft.com/azure/devops/pipelines/release/define-multistage-release-process?view=vsts)

