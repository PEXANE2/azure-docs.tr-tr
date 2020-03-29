---
title: (AmortismanA Uğradı) Azure Konteyner Servis Motoru ve Sürü Modu ile CI/CD
description: Sürekli olarak çok kapsayıcı .NET Core uygulaması sunmak için Docker Swarm Modu, Azure Konteyner Kayıt Defteri ve Azure DevOps ile Azure Konteyner Hizmet Motoru'nu kullanın
author: diegomrtnzg
ms.service: container-service
ms.topic: conceptual
ms.date: 05/27/2017
ms.author: dimart
ms.custom: mvc
ms.openlocfilehash: 1ec7ece6f5afd1bbd2613ae08af04b82e8a156b2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76277928"
---
# <a name="deprecated-full-cicd-pipeline-to-deploy-a-multi-container-application-on-azure-container-service-with-acs-engine-and-docker-swarm-mode-using-azure-devops"></a>(AmortismanA Uğradı) Azure DevOps'leri kullanarak ACS Engine ve Docker Swarm Modu ile Azure Konteyner Hizmeti'nde çok konteyner uygulaması dağıtmak için tam CI/CD boru hattı

[!INCLUDE [ACS deprecation](../../../includes/container-service-deprecation.md)]

*Bu makale, Azure DevOps belgelerini [kullanarak Docker Swarm ile Azure Konteyner Hizmeti'nde çok konteyner uygulaması dağıtmak için Tam CI/CD ardışık](container-service-docker-swarm-setup-ci-cd.md)*

Günümüzde bulut için modern uygulamalar geliştirirken karşılaşılan en büyük zorluklardan biri de bu uygulamaları sürekli olarak sunabilmektir. Bu makalede, tam bir sürekli tümleştirme ve dağıtım (CI/CD) ardışık nasıl uygulanacağını öğrenmek: 
* Docker Swarm Moduna Sahip Azure Konteyner Servis Motoru
* Azure Container Kayıt Defteri
* Azure DevOps


![MyShop örnek uygulama](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/myshop-application.png)

Amaç, azure DevOps kullanarak bu uygulamayı sürekli olarak Docker Swarm Modu kümesinde sunmaktır. Aşağıdaki şekil bu sürekli teslimat boru hattı ayrıntıları:

![MyShop örnek uygulama](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/full-ci-cd-pipeline.png)

İşte adımların kısa bir açıklaması:

1. Kod değişiklikleri kaynak kod deposuna adamıştır (burada, GitHub) 
2. GitHub, Azure DevOps'lerde bir yapıyı tetikler 
3. Azure DevOps kaynakların en son sürümünü alır ve uygulamayı oluşturan tüm görüntüleri oluşturur 
4. Azure DevOps, her görüntüyü Azure Kapsayıcı Kayıt Defteri hizmeti kullanılarak oluşturulan docker kayıt defterine iter 
5. Azure DevOps yeni bir sürümü tetikler 
6. Sürüm, Azure kapsayıcı hizmet küme ana düğümünde SSH kullanarak bazı komutları çalıştırıyor 
7. Kümedeki Docker Swarm Modu görüntülerin en son sürümünü çeker 
8. Uygulamanın yeni sürümü Docker Stack kullanılarak dağıtılır 

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiye başlamadan önce aşağıdaki görevleri tamamlamanız gerekir:

- [ACS Engine ile Azure Konteyner Hizmetinde Sürü Modu kümesi oluşturma](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acsengine-swarmmode)
- [Azure Container Service'teki Swarm kümesine bağlanma](../container-service-connect.md)
- [Azure kapsayıcı kayıt defteri oluşturma](../../container-registry/container-registry-get-started-portal.md)
- [Azure DevOps organizasyonu ve projesi oluşturuldu](https://docs.microsoft.com/azure/devops/organizations/accounts/create-organization-msa-or-work-student)
- [GitHub deposunu GitHub hesabınıza çatal](https://github.com/jcorioland/MyShop/tree/docker-linux)

>[!NOTE]
> Azure Container Service’teki Docker Swarm düzenleyicisi eski tek başına Swarm’u kullanır. Şu anda, tümleşik [Swarm modu](https://docs.docker.com/engine/swarm/) (Docker 1.12 ve daha sonraki sürümleri) Azure Container Service'te desteklenen bir düzenleyici değildir. Bu nedenle, Azure [Marketi'nde](https://azuremarketplace.microsoft.com)topluluk tarafından katkıda bulunulan bir [quickstart şablonu](https://azure.microsoft.com/resources/templates/101-acsengine-swarmmode/)veya Docker çözümü olan [ACS Engine'i](https://github.com/Azure/acs-engine/blob/master/docs/swarmmode.md)kullanıyoruz.
>

## <a name="step-1-configure-your-azure-devops-organization"></a>Adım 1: Azure DevOps kuruluşunuzu yapılandırın 

Bu bölümde, Azure DevOps kuruluşunuzu yapılandırın. Azure DevOps Hizmetleri Bitiş Noktalarını yapılandırmak için, Azure DevOps projenizde araç çubuğundaki **Ayarlar** simgesini tıklatın ve **Hizmetler'i**seçin.

![Açık Hizmetler Bitiş Noktası](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/services-vsts.PNG)

### <a name="connect-azure-devops-and-azure-account"></a>Azure DevOps ve Azure hesabını bağlayın

Azure DevOps projeniz ile Azure hesabınız arasında bir bağlantı kurun.

1. Solda, Yeni **Hizmet Bitiş Noktası** > Azure**Kaynak Yöneticisi'ni**tıklatın.
2. Azure DevOps'leri Azure hesabınızla çalışması için yetkilendirmek için **Aboneliğinizi** seçin ve **Tamam'ı**tıklatın.

    ![Azure DevOps - Azure Yetki](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-azure.PNG)

### <a name="connect-azure-devops-and-github"></a>Azure DevOps ve GitHub'ı bağlayın

Azure DevOps projeniz ile GitHub hesabınız arasında bir bağlantı kurun.

1. Solda, Yeni **Hizmet Bitiş Noktası** > **GitHub'ı**tıklatın.
2. Azure DevOps'lerin GitHub hesabınızla çalışmasına izin vermek için **Yet'i** tıklatın ve açılan pencerede yordamı izleyin.

    ![Azure DevOps - GitHub'ı Yetkilendirme](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-github.png)

### <a name="connect-azure-devops-to-your-azure-container-service-cluster"></a>Azure DevOps'leri Azure Kapsayıcı Hizmeti kümenize bağlayın

CI/CD ardışık alt aktasına girmeden önceki son adımlar, Azure'daki Docker Swarm kümenize dış bağlantıları yapılandırmaktır. 

1. Docker Swarm kümesi için **SSH**türünün bir bitiş noktası ekleyin. Ardından Sürü kümenizin (ana düğüm) SSH bağlantı bilgilerini girin.

    ![Azure DevOps - SSH](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-ssh.png)

Tüm yapılandırma şimdi yapılır. Sonraki adımlarda, uygulamayı Docker Swarm kümesine oluşturan ve dağıtan CI/CD ardışık bir yapı oluşturursunuz. 

## <a name="step-2-create-the-build-pipeline"></a>Adım 2: Yapı ardışık hattını oluşturma

Bu adımda, Azure DevOps projeniz için bir yapı ardışık hattı ayarlar sınız ve kapsayıcı resimleriniz için yapı iş akışını tanımlarsınız

### <a name="initial-pipeline-setup"></a>İlk ardışık düzen kurulumu

1. Yapı ardışık hattı oluşturmak için Azure DevOps projenize bağlanın ve **& Sürümü Oluştur'u**tıklatın. Yapı **tanımları** bölümünde **+ Yeni'yi**tıklatın. 

    ![Azure Devops - Yeni Yapı Boru Hattı](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/create-build-vsts.PNG)

2. **Boşişlemi'ni**seçin.

    ![Azure Devops - Yeni Boş Yapı Ardışık](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/create-empty-build-vsts.PNG)

4. Ardından **Değişkenler** sekmesini tıklatın ve iki yeni değişken oluşturun: **RegistryURL** ve **AgentURL**. Kayıt Defteri nizin ve Küme Aracıları DNS değerlerini yapıştırın.

    ![Azure DevOps - Yapı Değişkenleri Yapılandırması](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-build-variables.png)

5. Yapı **Tanımları** **sayfasında, Tetikleyiciler** sekmesini açın ve önkoşullarda oluşturduğunuz MyShop projesinin çatalıyla sürekli tümleştirme kullanacak şekilde yapıyı yapılandırın. Ardından, **Toplu İşlem değişikliklerini**seçin. **Şube belirtimi**olarak *docker-linux'u* seçtiğinizden emin olun.

    ![Azure DevOps - Yapı Deposu Yapılandırması](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-github-repo-conf.PNG)


6. Son olarak, **Seçenekler** sekmesini tıklatın ve **Barındırılan Linux Önizleme**Varsayılan aracı kuyruğunu yapılandırın.

    ![Azure Devops - Ana Bilgisayar Aracı Yapılandırması](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-build-agent.png)

### <a name="define-the-build-workflow"></a>Yapı iş akışını tanımlama
Sonraki adımlar yapı iş akışını tanımlar. İlk olarak, kodun kaynağını yapılandırmanız gerekir. Bunu yapmak için **GitHub'ı** ve **deponuzu** ve **şubenizi** (docker-linux) seçin.

![Azure DevOps - Kod kaynağını yapılandırma](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-source-code.png)

*MyShop* uygulaması için oluşturmak için beş konteyner görüntüleri vardır. Her resim, proje klasörlerinde bulunan Dockerfile kullanılarak oluşturulur:

* ÜrünlerApi
* Ara sunucu
* DerecelendirmeApi
* ÖnerilerApi
* ShopFront

Her görüntü için iki Docker adımına, görüntüyü oluşturmak için bir adıma ve görüntüyü Azure kapsayıcı kayıt defterine itmek için bir adıma ihtiyacınız var. 

1. Yapı iş akışına bir adım eklemek için **+ Yapı adımı ekle'yi** tıklatın ve **Docker'ı**seçin.

    ![Azure Devops - Yapı Adımları Ekle](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-build-add-task.png)

2. Her görüntü için, komutu `docker build` kullanan bir adımı yapılandırın.

    ![Azure DevOps - Docker Build](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-docker-build.png)

    Yapı işlemi için Azure Kapsayıcı Kayıt Defterinizi, görüntü eylemi **oluşturun'u** ve her görüntüyü tanımlayan Dockerfile'ı seçin. Çalışma **Dizini'ni** Dockerfile kök dizini olarak ayarlayın, **Resim Adı'nı**tanımlayın ve **En Son Etiketi Ekle'yi**seçin.
    
    Resim Adı bu biçimde olmalıdır: ```$(RegistryURL)/[NAME]:$(Build.BuildId)```. **[NAME]** resim adı ile değiştirin:
    - ```proxy```
    - ```products-api```
    - ```ratings-api```
    - ```recommendations-api```
    - ```shopfront```

3. Her görüntü için komutu `docker push` kullanan ikinci bir adımı yapılandırın.

    ![Azure DevOps - Docker Push](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-docker-push.png)

    Push işlemi için Azure kapsayıcı kayıt defterinizi, **görüntü eylemini İtirahat et,** önceki adımda oluşturulmuş **Resim Adı'nı** girin ve Son **Etiketi Ekle'yi**seçin.

4. Beş görüntünün her biri için yapı ve itme adımlarını yapılandırıldıktan sonra, yapı iş akışına üç adım daha ekleyin.

   ![Azure Devops - Komut Satırı Görev Ekle](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-build-command-task.png)

   1. Docker-compose.yml dosyasındaki RegistryURL oluşumunu *RegistryURL* değişkeniyle değiştirmek için bash komut dosyası kullanan bir komut satırı görevi. 
    
       ```-c "sed -i 's/RegistryUrl/$(RegistryURL)/g' src/docker-compose-v3.yml"```

       ![Azure DevOps - Kayıt Defteri URL'si ile Oluştur dosyalarını güncelleştir](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-build-replace-registry.png)

   2. Docker-compose.yml dosyasındaki AgentURL oluşumunu *AgentURL* değişkeniyle değiştirmek için bash komut dosyası kullanan bir komut satırı görevi.
  
       ```-c "sed -i 's/AgentUrl/$(AgentURL)/g' src/docker-compose-v3.yml"```

      1. Sürümde kullanılabilecek şekilde, güncelleştirilmiş Derleme dosyasını yapı yapı olarak düşüren bir görev. Ayrıntılar için aşağıdaki ekrana bakın.

      ![Azure Devops - Artefakt Yayınla](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-publish.png) 

      ![Azure DevOps - Oluştur dosyası yayımla](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-publish-compose.png) 

5. Yapı ardışık & test etmek için **& sırasını** kaydet'i tıklatın.

   ![Azure DevOps - Kaydet ve sıra](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-build-save.png) 

   ![Azure Devops - Yeni Kuyruk](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-build-queue.png) 

6. **Yapı** doğruysa, bu ekranı görmeniz gerekir:

   ![Azure DevOps - Yapı başarılı](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-build-succeeded.png) 

## <a name="step-3-create-the-release-pipeline"></a>Adım 3: Sürüm ardışık hattını oluşturma

Azure DevOps, [ortamlar arasında sürümleri yönetmenize](https://www.visualstudio.com/team-services/release-management/)olanak tanır. Uygulamanızın farklı ortamlarınızda (geliştirme, test, ön üretim ve üretim gibi) sorunsuz bir şekilde dağıtıldığınızdan emin olmak için sürekli dağıtımı etkinleştirebilirsiniz. Azure Kapsayıcı Hizmeti Docker Swarm Modu kümenizi temsil eden bir ortam oluşturabilirsiniz.

![Azure DevOps - ACS sürümü](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-release-acs.png) 

### <a name="initial-release-setup"></a>İlk sürüm kurulumu

1. Sürüm ardışık bir sözcük oluşturmak **için, Sürümler** > **+ Sürüm'ü** tıklatın

2. Yapı kaynağını yapılandırmak **için, Yapı** > **Link an artifact source**Kaynağı'nı tıklatın. Burada, bu yeni sürüm ardışık hattını önceki adımda tanımladığınız yapıya bağlayabilirsiniz. Bundan sonra, docker-compose.yml dosyası sürüm sürecinde kullanılabilir.

    ![Azure DevOps - Sürüm Eserler](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-release-artefacts.png) 

3. Sürüm tetikleyicisini yapılandırmak için **Tetikleyiciler'i** tıklatın ve **Sürekli Dağıtım'ı**seçin. Tetikleyiciyi aynı yapı kaynağına ayarlayın. Bu ayar, yapı başarıyla tamamlandığında yeni bir sürümün başlamasını sağlar.

    ![Azure DevOps - Sürüm Tetikleyicileri](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-release-trigger.png) 

4. Sürüm değişkenlerini yapılandırmak için **Değişkenler'i** tıklatın ve kayıt defterinin bilgileriyle birlikte üç yeni değişken oluşturmak için **+Değişken'i** seçin: **docker.username**, **docker.password**, ve **docker.registry**. Kayıt Defteri nizin ve Küme Aracıları DNS değerlerini yapıştırın.

    ![Azure DevOps - Yapı Deposu Yapılandırması](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-release-variables.png)

    >[!IMPORTANT]
    > Önceki ekranda gösterildiği gibi docker.password'teki **Kilit** onay kutusunu tıklatın. Parolayı kısıtlamak için bu ayar önemlidir.
    >

### <a name="define-the-release-workflow"></a>Sürüm iş akışını tanımlama

Sürüm iş akışı, eklediğiniz iki görevden oluşur.

1. Daha önce yapılandırdığınız SSH bağlantısını kullanarak, oluşturma dosyasını Docker Swarm ana düğümündeki bir *dağıtım* klasörüne güvenli bir şekilde kopyalamak için bir görevi yapılandırın. Ayrıntılar için aşağıdaki ekrana bakın.
    
    Kaynak klasör:```$(System.DefaultWorkingDirectory)/MyShop-CI/drop```

    ![Azure Devops - Sürüm SCP](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-release-scp.png)

2. Çalıştırmak `docker` için bir bash komutu yürütmek `docker stack deploy` için ikinci bir görev yapılandırın ve ana düğüm üzerinde komutları. Ayrıntılar için aşağıdaki ekrana bakın.

    ```
    docker login -u $(docker.username) -p $(docker.password) $(docker.registry) && export DOCKER_HOST=:2375 && cd deploy && docker stack deploy --compose-file docker-compose-v3.yml myshop --with-registry-auth
    ```

    ![Azure Devops - Yayın Bash](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-release-bash.png)

    Ana üzerinde yürütülen komut, aşağıdaki görevleri yapmak için Docker CLI ve Docker-Compose CLI'yi kullanır:

   - Azure kapsayıcı kayıt defterine giriş yapın **(Değişkenler** sekmesinde tanımlanan üç yapı değişkeni kullanır)
   - Swarm bitiş noktasıyla çalışmak için **DOCKER_HOST** değişkeni tanımlayın (:2375)
   - Önceki güvenli kopyalama görevi tarafından oluşturulan ve docker-compose.yml dosyasını içeren *dağıtım* klasörüne gidin 
   - Yeni `docker stack deploy` görüntüleri çeken ve kapsayıcıları oluşturan komutları çalıştırın.

     >[!IMPORTANT]
     > Önceki ekranda gösterildiği gibi, **STDERR** onay kutusunda Fail'i işaretsiz bırakın. Bu ayar, standart hata çıktısına `docker-compose` kapsayıcılar durma veya silinme gibi çeşitli tanılama iletilerinin yazdırılması nedeniyle sürüm işlemini tamamlamamıza olanak tanır. Onay kutusunu işaretleseniz, Azure DevOps her şey yolunda gitse bile sürüm sırasında hatalar oluştuğunu bildirir.
     >
3. Bu yeni sürüm ardışık hattını kaydedin.

## <a name="step-4-test-the-cicd-pipeline"></a>Adım 4: CI/CD ardışık hattını test edin

Şimdi yapılandırma ile yapılır, bu yeni CI / CD ardışık zaman. Bunu test etmenin en kolay yolu kaynak kodu güncelleştirmek ve değişiklikleri GitHub deponuza işlemektir. Kodu ittikten birkaç saniye sonra Azure DevOps'te çalışan yeni bir yapı görürsünüz. Başarıyla tamamlandıktan sonra, yeni bir sürüm tetiklenir ve uygulamanın yeni sürümünü Azure Kapsayıcı Hizmeti kümesine dağıtılır.

## <a name="next-steps"></a>Sonraki adımlar

* Azure DevOps ile CI/CD hakkında daha fazla bilgi için [Azure Ardışık Hatlar Belgeleri](/azure/devops/pipelines/?view=azure-devops) makalesine bakın.
* ACS Engine hakkında daha fazla bilgi için [ACS Engine GitHub repo'ya](https://github.com/Azure/acs-engine)bakın.
* Docker Swarm modu hakkında daha fazla bilgi için [Docker Swarm moduna genel bakış](https://docs.docker.com/engine/swarm/)alabiliyorum.
