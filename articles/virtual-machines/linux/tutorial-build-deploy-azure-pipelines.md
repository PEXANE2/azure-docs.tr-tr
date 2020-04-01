---
title: Öğretici - Azure Ardışık Hatlar'ı kullanarak Azure VM'lere CI/CD
description: Bu eğitimde, YAML tabanlı Azure ardışık hattını kullanarak Bir Node.js uygulamasının Azure VM'lerine sürekli tümleştirme (CI) ve sürekli dağıtım (CD) ayarlamayı öğrenirsiniz.
author: ushan
tags: azure-devops-pipelines
ms.assetid: ''
ms.service: virtual-machines-linux
ms.topic: tutorial
ms.tgt_pltfrm: azure-pipelines
ms.workload: infrastructure
ms.date: 1/3/2020
ms.author: ushan
ms.custom: devops
ms.openlocfilehash: bb7c773d02c5da5c115af79cd9e90c78e71eb6bf
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "76988337"
---
# <a name="tutorial-deploy-your-app-to-linux-virtual-machines-in-azure-using-azure-devops-services-and-azure-pipelines"></a>Öğretici: Azure DevOps Hizmetleri ve Azure Boru Hatları kullanarak uygulamanızı Azure'daki Linux sanal makinelerine dağıtın

Sürekli tümleştirme (CI) ve sürekli dağıtım (CD), her kod işledikten sonra kodunuzu oluşturabileceğiniz, serbest bırakabileceğiniz ve dağıtabileceğiniz bir ardışık hatlar oluşturur. Bu belge, Azure Ardışık Lıkları kullanarak çok makineli dağıtımlar yapmak için bir CI/CD ardışık nokta ayarlamayla ilişkili adımları içerir.

Azure Pipelines, sanal makinelere hem ön gösterimde hem de herhangi bir bulutta dağıtımlar için eksiksiz ve tam özellikli bir CI/CD otomasyon araçları seti sağlar.

Bu eğitimde, uygulamanızı, her biri uygulamayı çalıştırmak için web sunucusu olarak hizmet veren Linux Sanal makineleriyle bir Azure Ardışık Lık [Ortamına](https://docs.microsoft.com/azure/devops/pipelines/process/environments?view=azure-devops) dağıtmak için YAML tabanlı bir CI/CD ardışık hattı ayarlarsınız.

Aşağıdakileri nasıl yapacağınızı öğrenirsiniz:

> [!div class="checklist"]
> * Örnek bir uygulama alın.
> * Örnek uygulamayı oluşturmak için YAML tabanlı bir Azure Pipelines CI ardışık hattı oluşturun.
> * Azure sanal makineleri için Bir Azure Ardışık Ortam Oluşturma
> * Bir Azure Pipelines CD ardışık alanı oluşturun.
> * El ile ve CI ile tetiklenen dağıtımlar yürütme.

## <a name="before-you-begin"></a>Başlamadan önce

* Azure DevOps Hizmetleri kuruluşunuzda**https://dev.azure.com/** oturum açın ( ). 
  Ücretsiz bir [Azure DevOps Services kuruluşu](https://go.microsoft.com/fwlink/?LinkId=307137&clcid=0x409&wt.mc_id=o~msft~vscom~home-vsts-hero~27308&campaign=o~msft~vscom~home-vsts-hero~27308) edinebilirsiniz.

  > [!NOTE]
  > Daha fazla bilgi için, bkz. [Azure DevOps Services’a bağlanma](https://docs.microsoft.com/azure/devops/organizations/projects/connect-to-projects?view=vsts).

*  Dağıtım hedefi için bir Linux sanal makinesi gerekir.  Daha fazla bilgi için bkz. [Azure CLI ile Linux sanal makineleri oluşturma ve yönetme](https://docs.microsoft.com/azure/virtual-machines/linux/tutorial-manage-vm).

*  Sanal makineniz için 80 numaralı gelen bağlantı noktasını açın. Daha fazla bilgi için bkz. [Azure portalını kullanarak ağ güvenlik grupları oluşturma](https://docs.microsoft.com/azure/virtual-network/tutorial-filter-network-traffic).

## <a name="get-your-sample-app-code"></a>Örnek uygulama kodunuzu alın

GitHub'da dağıtmak istediğiniz bir uygulamanız varsa, bu kod için bir ardışık iş hattı oluşturmayı deneyebilirsiniz.

Ancak, yeni bir kullanıcı ysanız, örnek kodumuzu kullanarak daha iyi bir başlangıç alabilirsiniz. Bu durumda, GitHub bu repo çatal:

#### <a name="java"></a>[Java](#tab/java)

```
https://github.com/spring-projects/spring-petclinic
```

> [!NOTE]
> Petclinic Bir [Java Bahar Boot](https://spring.io/guides/gs/spring-boot) uygulaması [Maven](https://spring.io/guides/gs/maven/)kullanılarak inşa edilmiştir.

#### <a name="javascript"></a>[JavaScript](#tab/java-script)

```
https://github.com/azure-devops/fabrikam-node
```

> [!NOTE]
> Bu Node.js uygulaması [Yeoman](https://yeoman.io/learning/index.html)üzerinden inşa edilmiştir. Express, bower ve grunt kullanır. Ayrıca bağımlılıklar olarak bazı npm paketlerini içerir.
> Örnek, Nginx’i ayarlayan ve uygulamayı dağıtan bir betik de içerir. Sanal makinelerde yürütülür. Betik özellikle:
> 1. Node, Nginx ve PM2'yi yükler.
> 2. Nginx ve PM2’yi yapılandırır.
> 3. Düğüm uygulamasını başlatır.

* * * 

## <a name="prerequisites-for-the-linux-vm"></a>Linux VM için ön koşullar

Yukarıda belirtilen örnek uygulamalar Ubuntu 16.04 üzerinde test edilmiştir ve bu hızlı başlangıç için Linux VM'nin aynı sürümünü kullanmanızı öneririz.
Uygulama için kullanılan çalışma zamanı yığınını temel alan aşağıda açıklanan ek adımları izleyin.

#### <a name="java"></a>[Java](#tab/java)

- Java Bahar Önyükleme ve Bahar Bulutu tabanlı uygulamaları dağıtmak için, tam olarak desteklenen OpenJDK tabanlı çalışma süresi sağlayan [bu](https://azuremarketplace.microsoft.com/marketplace/apps/azul.azul-zulu8-ubuntu-1804) şablonu kullanarak Azure'da bir Linux VM oluşturun.
- Tomcat sunucusunda Java servletleri dağıtmak için, [bu](https://azuremarketplace.microsoft.com/marketplace/apps/azul.azul-zulu8-ubuntu-1804) Azure şablonu kullanarak Java 8 ile bir Linux VM oluşturun ve [Hizmet olarak Tomcat 9.x'i yapılandırın.](https://tomcat.apache.org/tomcat-9.0-doc/setup.html)
- Java EE tabanlı uygulamayı dağıtmak [için, Linux VM + Java + WebSphere 9.x](https://azuremarketplace.microsoft.com/marketplace/apps/midvision.websphere-application-server-nde-90) veya [Linux VM + Java + WebLogic 12.x](https://azuremarketplace.microsoft.com/marketplace/apps/oracle.20191009-arm-oraclelinux-wls-admin) veya [Linux VM +Java +](https://azuremarketplace.microsoft.com/marketplace/apps/azul.azul-zulu8-ubuntu-1804) WildFly/JBoss 14 oluşturmak için bir Azure şablonu kullanın 

#### <a name="javascript"></a>[JavaScript](#tab/java-script)

Javascript uygulamasını veya Node.js uygulamasını yüklemek için uygulamayı dağıtmak için Nginx web sunucusuna sahip bir Linux VM'ye ihtiyacınız vardır.
Nginx ile zaten bir Linux VM'inseniz yoksa, [bu örnekteki](/azure/virtual-machines/linux/quick-create-cli)adımları kullanarak Azure'da bir tane oluşturun.

* * * 

## <a name="create-an-azure-pipelines-environment-with-azure-virtual-machines"></a>Azure sanal makineleriyle Azure Ardışık Hatları ortamı oluşturma

Sanal makineler [ortamlara](https://docs.microsoft.com/azure/devops/pipelines/process/environments) kaynak olarak eklenebilir ve çok makineli dağıtımlar için hedeflenebilir. Ortam içindeki dağıtım geçmişi görünümleri VM'den ardışık hatlara ve daha sonra işlemeye kadar izlenebilirlik sağlar.

"**Boru Hatları**" bölümündeki "**Ortamlar**" hub'ında bir ortam oluşturabilirsiniz.
1.  Azure DevOps kuruluşunuzla oturum açın ve projenize gidin.
2.  Projenizde, **Ardışık Hatlar** sayfasına gidin. Ardından **Ortamlar'ı** seçin ve **Ortam Oluştur'u**tıklatın. Ortam ve Açıklama için bir **Ad** (gerekli) **belirtin.**
3.  Ortama eklenecek **kaynak** olarak **Sanal Makineler'i** seçin ve **İleri'yi**tıklatın.
4.  İşletim Sistemi'ni (Windows/Linux) seçin ve **PS kayıt komut dosyasını kopyalayın.** 
5.  Şimdi bu Ortama kaydedilecek hedef VM'lerin her birinde bir yönetici PowerShell komut isteminden kopyalanan komut dosyasını çalıştırın.
    > [!NOTE]
    > - Oturum açan kullanıcının Kişisel Erişim Belirteci, kopyalanan komut dosyasının kullanılamaz hale geldiği aynı gün sona eren komut dosyasına önceden eklenir.
    > - VM'nizde zaten çalışan bir aracı varsa, ortama kaydolmak için "aracı" için benzersiz bir ad sağlayın.
6.  VM kaydedildikten sonra, ortamın "kaynaklar" sekmesi altında bir ortam kaynağı olarak görünmeye başlar.

    ![VMcreation](media/tutorial-deploy-vms-azure-pipelines/vm-creation.png)

7.  Daha fazla VM eklemek için, "Kaynak ekle"ye tıklayarak ve kaynak olarak "Sanal Makineler"i seçerek komut dosyasını yeniden görüntüleyebilir ve kopyalayabilirsiniz. Bu komut dosyası, bu ortama eklenecek tüm VM'ler için aynı kalır. 
8.  Uygulamanızın dağıtımını koordine etmek için her makine Azure Pipelines ile etkileşimde direnir.

    ![VMresource_view](media/tutorial-deploy-vms-azure-pipelines/vm-resourceview.png)

9. Etkileşimli PS kayıt komut dosyasının bir parçası olarak VM'ye etiketler ekleyebilirsiniz (veya) kaynak görünümündeki her VM kaynağının sonundaki üçlü noktalara tıklayarak aynı bölümü kaynak görünümünden de ekleyebilir/kaldırabilirsiniz.

   Atadığınız etiketler, dağıtım işinde ortam kullanıldığında dağıtımı belirli sanal makinelerle sınırlamanıza olanak sağlar. Etiketlerin her biri 256 karakterle sınırlıdır, ancak kullanabileceğiniz etiket sayısı nın sınırı yoktur.

   ![VMtags](media/tutorial-deploy-vms-azure-pipelines/vm-tags.png)

* * * 

## <a name="define-your-ci-build-pipeline"></a>CI yapı ardışık sisteminizi tanımlayın

Web uygulamanızı yayınlayan sürekli bir tümleştirme (CI) yapı ardışık hattının yanı sıra Ubuntu sunucusunda yerel olarak çalıştırılabilen bir dağıtım komut dosyasına ihtiyacınız vardır. Kullanmak istediğiniz çalışma süresine göre bir CI yapı ardışık hattı ayarlayın. 

1. Azure DevOps kuruluşunuzla oturum açın ve projenize gidin.

1. Projenizde, **Ardışık Hatlar** sayfasına gidin. Ardından, yeni bir ardışık etki alanı oluşturmak için eylemi seçin.

1. İlk olarak kaynak kodunuzun konumu olarak **GitHub'ı** seçerek sihirbazın adımlarını gözden geçirin.

1. Oturum açmanız için GitHub'a yönlendirilmiş olabilirsiniz. Bu ysa, GitHub kimlik bilgilerinizi girin.

1. Depolistesi göründüğünde, istediğiniz örnek uygulama deposunu seçin.

1. Azure Pipelines deponuzu analiz eder ve uygun bir boru hattı şablonu önerir.

#### <a name="java"></a>[Java](#tab/java)

**Başlangıç** şablonunu seçin ve Java projenizi oluşturan ve Apache Maven ile testler yürüten aşağıdaki YAML snippet'ini kopyalayın:

```YAML
- job: Build
  displayName: Build Maven Project
  steps:
  - task: Maven@3
    displayName: 'Maven Package'
    inputs:
      mavenPomFile: 'pom.xml'
  - task: CopyFiles@2
    displayName: 'Copy Files to artifact staging directory'
    inputs:
      SourceFolder: '$(System.DefaultWorkingDirectory)'
      Contents: '**/target/*.?(war|jar)'
      TargetFolder: $(Build.ArtifactStagingDirectory)
  - upload: $(Build.ArtifactStagingDirectory)
    artifact: drop
```

Daha fazla rehberlik için, [Maven ile Java uygulamanızı oluşturun'da](https://docs.microsoft.com/azure/devops/pipelines/ecosystems/java)belirtilen adımları izleyin.

#### <a name="javascript"></a>[JavaScript](#tab/java-script)

**Başlangıç** şablonu seçin ve npm ile genel bir Düğüm.js projesi oluşturur aşağıdaki YAML snippet kopyalayın.

```YAML
- stage: Build
  displayName: Build stage
  jobs:  
  - job: Build
    displayName: Build
    - task: NodeTool@0
      inputs:
        versionSpec: '10.x'
      displayName: 'Install Node.js'
    - script: |
        npm install
        npm run build --if-present
        npm run test --if-present
      displayName: 'npm install, build and test'
    - task: ArchiveFiles@2
      displayName: 'Archive files'
      inputs:
        rootFolderOrFile: '$(System.DefaultWorkingDirectory)'
        includeRootFolder: false
        archiveType: zip
        archiveFile: $(Build.ArtifactStagingDirectory)/$(Build.BuildId).zip
        replaceExistingArchive: true
    - upload: $(Build.ArtifactStagingDirectory)/$(Build.BuildId).zip
      artifact: drop
```

Daha fazla kılavuz için, [Gulp ile Düğüm.js uygulamanızı oluştur'daki](https://docs.microsoft.com/azure/devops/pipelines/ecosystems/javascript)adımları izleyin.

- Ne yaptığını görmek için boru hattına bir göz atın. Tüm varsayılan girişlerin kodunuz için uygun olduğundan emin olun.

- **Kaydet ve çalıştır'ı**seçin, ardından **doğrudan ana dala Bağlan'ı**seçin ve sonra **Kaydet'i ve yeniden çalıştır'ı** seçin.

- Yeni bir çalışma başlatıldı. Koşunun bitmesini bekle.

* * * 

## <a name="define-cd-steps-to-deploy-to-the-linux-vm"></a>Linux VM'ye dağıtmak için CD adımlarını tanımlama

1. Yukarıdaki ardışık düzenleme ve aşağıdaki YAML sözdizimini kullanarak daha önce sahip olduğunuz ortamı ve VM kaynaklarına başvurarak bir [dağıtım işi](https://docs.microsoft.com/azure/devops/pipelines/process/deployment-jobs) ekleyin:

   ```YAML
   jobs:  
   - deployment: VMDeploy
     displayName: web
     environment:
       name:  <environment name>
       resourceType: VirtualMachine
       tags: web1
     strategy:
   ```
2. Ortamdaki her sanal makine için tanımladığınız **etiketleri** belirterek dağıtımı almak için ortamdan belirli sanal makine kümelerini seçebilirsiniz.
[Burada](https://docs.microsoft.com/azure/devops/pipelines/yaml-schema?view=azure-devops&tabs=schema#deployment-job) Dağıtım iş için tam YAML şema olduğunu.

3. Eithor'u `runOnce` veya `rolling` dağıtım stratejisini belirtebilirsiniz. 

   `runOnce`tüm yaşam döngüsü kancaları, yani `preDeploy` `deploy`, `routeTraffic`ve `postRouteTraffic`, bir kez yürütülür basit dağıtım stratejisidir. Sonra, `on:` `success` ya `on:` `failure` da yürütülür.

   Aşağıda örnek YAML snippet `runOnce` için:
   ```YAML
   jobs:
   - deployment: VMDeploy
     displayName: web
     pool:
       vmImage: 'Ubuntu-16.04'
     environment:
       name: <environment name>
       resourceType: VirtualMachine
     strategy:
         runOnce:
           deploy:
             steps:
             - script: echo my first deployment
   ```

4. Aşağıda, Sanal makineler için bir yuvarlama stratejisi tanımlamak için kullanabileceğiniz YAML parçacığının bir örneği, her yinelemede 5 hedefe kadar güncellenir. `maxParallel`paralel olarak dağıtılabilen hedef sayısını belirleyecektir. Seçim, dağıtılmakta olan hedefler hariç, herhangi bir zamanda kullanılabilir kalması gereken hedeflerin mutlak sayısını veya yüzdesini hesaplar. Dağıtım sırasında başarı ve hata koşullarını belirlemek için de kullanılır.

   ```YAML
   jobs: 
   - deployment: VMDeploy
     displayName: web
     environment:
       name: <environment name>
       resourceType: VirtualMachine
     strategy:
         rolling:
           maxParallel: 5  #for percentages, mention as x%
           preDeploy:
             steps:
             - download: current
               artifact: drop
             - script: echo initialize, cleanup, backup, install certs
           deploy:
             steps:
             - task: Bash@3
               inputs:
                 targetType: 'inline'
                 script: |
                   # Modify deployment script based on the app type
                   echo "Starting deployment script run"
                   sudo java -jar '$(Pipeline.Workspace)/drop/**/target/*.jar'
           routeTraffic:
             steps:
             - script: echo routing traffic
           postRouteTraffic:
             steps:
             - script: echo health check post-route traffic
           on:
             failure:
               steps:
               - script: echo Restore from backup! This is on failure
             success:
               steps:
               - script: echo Notify! This is on success
   ```

   Bu işin her çalıştırışta, dağıtım `<environment name>` geçmişi, oluşturduğunuz ve Kaydettiğiniz ortama karşı kaydedilir.

## <a name="run-your-pipeline-and-get-traceability-views-in-environment"></a>Ardışık hattınızı çalıştırın ve çevrede izlenebilirlik görünümleri elde edin
Dağıtımlar ortamının görünümü, taahhütlerin ve iş öğelerinin tam izlenebilirliğini ve ortam/kaynak başına bir çapraz ardışık dağıtım geçmişi sağlar.

![VMDeployments_view](media/tutorial-deploy-vms-azure-pipelines/vm-deployments.png)
  
![VMjobs_view](media/tutorial-deploy-vms-azure-pipelines/vm-jobsview.png)

## <a name="next-steps"></a>Sonraki adımlar
- Oluşturduğunuz [ardışık hattı özelleştirmeye](https://docs.microsoft.com/azure/devops/pipelines/customize-pipeline) devam edebilirsiniz.
- YAML boru hatlarında başka neler yapabileceğinizi öğrenmek için [YAML şema referansına](https://docs.microsoft.com/azure/devops/pipelines/yaml-schema)bakın.
- LAMP (Linux, Apache, MySQL ve PHP) yığınını dağıtma hakkında bilgi edinmek için sonraki öğreticiye ilerleyin.

> [!div class="nextstepaction"]
> [LAMP yığını dağıtma](tutorial-lamp-stack.md)
