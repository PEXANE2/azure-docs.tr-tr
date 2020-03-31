---
title: (AmortismanA Uğradı) Azure Konteyner Hizmeti ve Sürülü CI/CD
description: Sürekli olarak çok kapsayıcı .NET Core uygulaması sunmak için Docker Swarm, Azure Konteyner Kayıt Defteri ve Azure DevOps ile Azure Kapsayıcı Hizmetini kullanın
author: jcorioland
ms.service: container-service
ms.topic: conceptual
ms.date: 12/08/2016
ms.author: jucoriol
ms.custom: mvc
ms.openlocfilehash: 11a6debe735459b617f6f93c3f67a32350dd4623
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76549062"
---
# <a name="deprecated-full-cicd-pipeline-to-deploy-a-multi-container-application-on-azure-container-service-with-docker-swarm-using-azure-devops-services"></a>(AmortismanA Uğradı) Azure DevOps Hizmetlerini kullanarak Docker Swarm ile Azure Konteyner Hizmeti'nde çoklu kapsayıcı uygulaması dağıtmak için tam CI/CD ardışık

[!INCLUDE [ACS deprecation](../../../includes/container-service-deprecation.md)]

Bulut için modern uygulamalar geliştirirken karşılaşılan en büyük zorluklardan biri, bu uygulamaları sürekli olarak sunabilmektir. Bu makalede, Docker Swarm, Azure Konteyner Kayıt Defteri ve Azure Boru Hatları yönetimiyle Azure Konteyner Hizmeti'ni kullanarak tam sürekli tümleştirme ve dağıtım (CI/CD) ardışık hattını nasıl uygulayacağınızı öğreneceksiniz.


![MyShop örnek uygulama](./media/container-service-docker-swarm-setup-ci-cd/myshop-application.png)

Amaç, Azure DevOps Hizmetlerini kullanarak bu uygulamayı sürekli olarak Docker Swarm kümesinde sunmaktır. Aşağıdaki şekil bu sürekli teslimat boru hattı ayrıntıları:

![MyShop örnek uygulama](./media/container-service-docker-swarm-setup-ci-cd/full-ci-cd-pipeline.png)

İşte adımların kısa bir açıklaması:

1. Kod değişiklikleri kaynak kod deposuna adamıştır (burada, GitHub) 
1. GitHub, Azure DevOps Hizmetlerinde bir yapıyı tetikler 
1. Azure DevOps Hizmetleri kaynakların en son sürümünü alır ve uygulamayı oluşturan tüm görüntüleri oluşturur 
1. Azure DevOps Hizmetleri, her görüntüyü Azure Kapsayıcı Kayıt Defteri hizmeti kullanılarak oluşturulan docker kayıt defterine iter 
1. Azure DevOps Hizmetleri yeni bir sürümü tetikler 
1. Sürüm, Azure kapsayıcı hizmet küme ana düğümünde SSH kullanarak bazı komutları çalıştırıyor 
1. Kümedeki Docker Swarm görüntülerin en son sürümünü çekiyor 
1. Uygulamanın yeni sürümü Docker Compose kullanılarak dağıtılır 

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiye başlamadan önce aşağıdaki görevleri tamamlamanız gerekir:

- [Azure Container Service'te Swarm kümesi oluşturma](container-service-deployment.md)
- [Azure Container Service'teki Swarm kümesine bağlanma](../container-service-connect.md)
- [Azure kapsayıcı kayıt defteri oluşturma](../../container-registry/container-registry-get-started-portal.md)
- [Azure DevOps Hizmetleri organizasyonu ve projesi oluşturuldu](https://docs.microsoft.com/azure/devops/organizations/accounts/create-organization-msa-or-work-student)
- [GitHub deposunu GitHub hesabınıza çatal](https://github.com/jcorioland/MyShop/)

[!INCLUDE [container-service-swarm-mode-note](../../../includes/container-service-swarm-mode-note.md)]

Ayrıca Docker yüklü bir Ubuntu (14.04 veya 16.04) makine gerekir. Bu makine, Azure Ardışık İşlemleri işlemleri sırasında Azure DevOps Hizmetleri tarafından kullanılır. Bu makineyi oluşturmanın bir yolu, Azure Marketi'nde bulunan görüntüyü kullanmaktır. 

## <a name="step-1-configure-your-azure-devops-services-organization"></a>Adım 1: Azure DevOps Hizmetleri kuruluşunuzu yapılandırın 

Bu bölümde, Azure DevOps Hizmetleri kuruluşunuzu yapılandırın.

### <a name="configure-an-azure-devops-services-linux-build-agent"></a>Azure DevOps Services Linux yapı aracısı yapılandırma

Docker görüntüleri oluşturmak ve bu görüntüleri Bir Azure DevOps Hizmetleri yapısından bir Azure kapsayıcı kayıt defterine itmek için bir Linux aracısı kaydetmeniz gerekir. Bu yükleme seçenekleri var:

* [Linux'ta bir aracı dağıtma](https://www.visualstudio.com/docs/build/admin/agents/v2-linux)

* [Azure DevOps Hizmetleri aracısını çalıştırmak için Docker'ı kullanın](https://hub.docker.com/r/microsoft/vsts-agent)

### <a name="install-the-docker-integration-azure-devops-services-extension"></a>Docker TümleştirmeAzure DevOps Hizmetleri uzantısını yükleme

Microsoft, Azure Pipelines işlemlerinde Docker ile çalışmak için bir Azure DevOps Hizmetleri uzantısı sağlar. Bu uzantı Azure [DevOps Hizmetleri Marketplace'te](https://marketplace.visualstudio.com/items?itemName=ms-vscs-rm.docker)kullanılabilir. Bu uzantıyı Azure DevOps Hizmetleri kuruluşunuza eklemek için **Yükle'yi** tıklatın:

![Docker Tümleştirmesini Yükleyin](./media/container-service-docker-swarm-setup-ci-cd/install-docker-vsts.png)

Kimlik bilgilerinizi kullanarak Azure DevOps Hizmetleri kuruluşunuza bağlanmanız istenir. 

### <a name="connect-azure-devops-services-and-github"></a>Azure DevOps Hizmetlerini ve GitHub'ı bağlayın

Azure DevOps Hizmetleri projeniz ile GitHub hesabınız arasında bir bağlantı kurun.

1. Azure DevOps Hizmetleri projenizde araç çubuğundaki **Ayarlar** simgesini tıklatın ve **Hizmetler'i**seçin.

    ![Azure Devops Hizmetleri - Dış Bağlantı](./media/container-service-docker-swarm-setup-ci-cd/vsts-services-menu.png)

1. Solda, Yeni **Hizmet Bitiş Noktası** > **GitHub'ı**tıklatın.

    ![Azure Devops Hizmetleri - GitHub](./media/container-service-docker-swarm-setup-ci-cd/vsts-github.png)

1. Azure DevOps Hizmetleri'nin GitHub hesabınızla çalışmasına izin vermek için **Yet'i** tıklatın ve açılan pencerede yordamı izleyin.

    ![Azure DevOps Hizmetleri - GitHub'ı Yetkilendirme](./media/container-service-docker-swarm-setup-ci-cd/vsts-github-authorize.png)

### <a name="connect-azure-devops-services-to-your-azure-container-registry-and-azure-container-service-cluster"></a>Azure DevOps Hizmetlerini Azure kapsayıcı kayıt defterinize ve Azure Kapsayıcı Hizmeti kümenize bağlayın

CI/CD ardışık şirketine girmeden önceki son adımlar, konteyner kayıt defterinize ve Azure'daki Docker Swarm kümenize dış bağlantıları yapılandırmaktır. 

1. Azure DevOps Hizmetleri projenizin **Hizmetler** ayarlarında Docker **Kayıt Defteri**türüne bir hizmet bitiş noktası ekleyin. 

1. Açılan açılır pencerede, URL'yi ve Azure konteyner kayıt defterinizin kimlik bilgilerini girin.

    ![Azure DevOps Hizmetleri - Docker Kayıt Defteri](./media/container-service-docker-swarm-setup-ci-cd/vsts-registry.png)

1. Docker Swarm kümesi için **SSH**türünün bir bitiş noktası ekleyin. Ardından Swarm kümenizin SSH bağlantı bilgilerini girin.

    ![Azure Devops Hizmetleri - SSH](./media/container-service-docker-swarm-setup-ci-cd/vsts-ssh.png)

Tüm yapılandırma şimdi yapılır. Sonraki adımlarda, uygulamayı Docker Swarm kümesine oluşturan ve dağıtan CI/CD ardışık bir yapı oluşturursunuz. 

## <a name="step-2-create-the-build-pipeline"></a>Adım 2: Yapı ardışık hattını oluşturma

Bu adımda, Azure DevOps Hizmetleri projeniz için bir yapı ardışık hattı ayarlar sınız ve konteyner resimleriniz için yapı iş akışını tanımlarsınız

### <a name="initial-pipeline-setup"></a>İlk ardışık düzen kurulumu

1. Yapı ardışık bir yapı oluşturmak için Azure DevOps Hizmetleri projenize bağlanın ve **& Sürümü Oluştur'u**tıklatın. 

1. Yapı **tanımları** bölümünde **+ Yeni'yi**tıklatın. Boş **şablonu** seçin.

    ![Azure Devops - Yeni Yapı Boru Hattı](./media/container-service-docker-swarm-setup-ci-cd/create-build-vsts.png)

1. Yeni yapıyı bir GitHub deposu kaynağıyla yapılandırın, **Sürekli tümleştirmeyi**denetleyin ve Linux aracınızı kaydettiğiniz aracı kuyruğunu seçin. Yapı ardışık hattını oluşturmak için **Oluştur'u** tıklatın.

    ![Azure Devops Hizmetleri - Yapı Ardışık Hattı Oluştur](./media/container-service-docker-swarm-setup-ci-cd/vsts-create-build-github.png)

1. Yapı **Tanımları** sayfasında, önce **Depo** sekmesini açın ve önkoşullarda oluşturduğunuz MyShop projesinin çatalını kullanacak şekilde yapıyı yapılandırın. **Varsayılan dal**olarak *acs-docs* seçtiğinizden emin olun.

    ![Azure DevOps Hizmetleri - Repository Configuration oluştur](./media/container-service-docker-swarm-setup-ci-cd/vsts-github-repo-conf.png)

1. **Tetikleyiciler** sekmesinde, yapıyı her işlemeden sonra tetiklenecek şekilde yapılandırın. **Sürekli tümleştirme** ve **Toplu İşlem değişikliklerini**seçin.

    ![Azure Devops Hizmetleri - Tetikleyici Yapılandırması Oluştur](./media/container-service-docker-swarm-setup-ci-cd/vsts-github-trigger-conf.png)

### <a name="define-the-build-workflow"></a>Yapı iş akışını tanımlama
Sonraki adımlar yapı iş akışını tanımlar. *MyShop* uygulaması için oluşturmak için beş konteyner görüntüleri vardır. Her resim, proje klasörlerinde bulunan Dockerfile kullanılarak oluşturulur:

* ÜrünlerApi
* Ara sunucu
* DerecelendirmeApi
* ÖnerilerApi
* ShopFront

Her görüntü için iki Docker adımı eklemeniz, bir resmi oluşturmak için ve bir de görüntüyü Azure kapsayıcı kayıt defterine itmek için eklemeniz gerekir. 

1. Yapı iş akışına bir adım eklemek için **+ Yapı adımı ekle'yi** tıklatın ve **Docker'ı**seçin.

    ![Azure Devops Hizmetleri - Yapı Adımları Ekle](./media/container-service-docker-swarm-setup-ci-cd/vsts-build-add-task.png)

1. Her görüntü için, komutu `docker build` kullanan bir adımı yapılandırın.

    ![Azure Devops Hizmetleri - Docker Build](./media/container-service-docker-swarm-setup-ci-cd/vsts-docker-build.png)

    Yapı işlemi için Azure kapsayıcı kayıt defterinizi, görüntü eylemi **oluşturun'u** ve her görüntüyü tanımlayan Dockerfile'ı seçin. Yapı **bağlamını** Dockerfile kök dizini olarak ayarlayın ve **Görüntü Adı'nı**tanımlayın. 
    
    Önceki ekranda gösterildiği gibi, görüntü adını Azure kapsayıcı kayıt defterinizin URI'si ile başlatın. (Bu örnekteki yapı tanımlayıcısı gibi görüntünün etiketini parametrelendirmek için bir yapı değişkeni de kullanabilirsiniz.)

1. Her görüntü için komutu `docker push` kullanan ikinci bir adımı yapılandırın.

    ![Azure Devops Hizmetleri - Docker Push](./media/container-service-docker-swarm-setup-ci-cd/vsts-docker-push.png)

    Push işlemi için Azure kapsayıcı kayıt defterinizi, **görüntü eylemini İtirahat la** ve önceki adımda oluşturulmuş **Görüntü Adı'nı** girin.

1. Beş görüntünün her biri için yapı ve itme adımlarını yapılandırıldıktan sonra, yapı iş akışına iki adım daha ekleyin.

    a. Docker-compose.yml dosyasındaki *BuildNumber* oluşumunu geçerli yapı Kimliği ile değiştirmek için bash komut dosyası kullanan bir komut satırı görevi. Ayrıntılar için aşağıdaki ekrana bakın.

    ![Azure DevOps Hizmetleri - Dosyayı Güncelleştir](./media/container-service-docker-swarm-setup-ci-cd/vsts-build-replace-build-number.png)

    b. Sürümde kullanılabilecek şekilde, güncelleştirilmiş Derleme dosyasını yapı yapı olarak düşüren bir görev. Ayrıntılar için aşağıdaki ekrana bakın.

    ![Azure DevOps Hizmetleri - Oluştur dosyası yaz](./media/container-service-docker-swarm-setup-ci-cd/vsts-publish-compose.png) 

1. **Kaydet'i** tıklatın ve yapı ardışık adınızı adlandırın.

## <a name="step-3-create-the-release-pipeline"></a>Adım 3: Sürüm ardışık hattını oluşturma

Azure DevOps Hizmetleri, [ortamlar arasında sürümleri yönetmenize](https://www.visualstudio.com/team-services/release-management/)olanak tanır. Uygulamanızın farklı ortamlarınızda (geliştirme, test, ön üretim ve üretim gibi) sorunsuz bir şekilde dağıtıldığınızdan emin olmak için sürekli dağıtımı etkinleştirebilirsiniz. Azure Kapsayıcı Hizmeti Docker Swarm kümenizi temsil eden yeni bir ortam oluşturabilirsiniz.

![Azure DevOps Hizmetleri - ACS sürümü](./media/container-service-docker-swarm-setup-ci-cd/vsts-release-acs.png) 

### <a name="initial-release-setup"></a>İlk sürüm kurulumu

1. Sürüm ardışık bir sözcük oluşturmak **için, Sürümler** > **+ Sürüm'ü** tıklatın

1. Yapı kaynağını yapılandırmak **için, Yapı** > **Link an artifact source**Kaynağı'nı tıklatın. Burada, bu yeni sürüm ardışık hattını önceki adımda tanımladığınız yapıya bağlayabilirsiniz. Bunu yaparak, docker-compose.yml dosyası sürüm sürecinde kullanılabilir.

    ![Azure DevOps Hizmetleri - Sürüm Yapıları](./media/container-service-docker-swarm-setup-ci-cd/vsts-release-artefacts.png) 

1. Sürüm tetikleyicisini yapılandırmak için **Tetikleyiciler'i** tıklatın ve **Sürekli Dağıtım'ı**seçin. Tetikleyiciyi aynı yapı kaynağına ayarlayın. Bu ayar, yapı başarıyla tamamlanır tamamlanmaz yeni bir sürümün başlatılmasını sağlar.

    ![Azure DevOps Hizmetleri - Sürüm Tetikleyicileri](./media/container-service-docker-swarm-setup-ci-cd/vsts-release-trigger.png) 

### <a name="define-the-release-workflow"></a>Sürüm iş akışını tanımlama

Sürüm iş akışı, eklediğiniz iki görevden oluşur.

1. Daha önce yapılandırdığınız SSH bağlantısını kullanarak, oluşturma dosyasını Docker Swarm ana düğümündeki bir *dağıtım* klasörüne güvenli bir şekilde kopyalamak için bir görevi yapılandırın. Ayrıntılar için aşağıdaki ekrana bakın.

    ![Azure Devops Hizmetleri - Sürüm SCP](./media/container-service-docker-swarm-setup-ci-cd/vsts-release-scp.png)

1. Çalıştırmak `docker` için bir bash komutu yürütmek `docker-compose` için ikinci bir görev yapılandırın ve ana düğüm üzerinde komutları. Ayrıntılar için aşağıdaki ekrana bakın.

    ![Azure Devops Hizmetleri - Yayın Bash](./media/container-service-docker-swarm-setup-ci-cd/vsts-release-bash.png)

    Ana görevde yürütülen komut, aşağıdaki görevleri yapmak için Docker CLI ve Docker-Compose CLI'yi kullanır:

   - Azure kapsayıcı kayıt defterine giriş yapın **(Değişkenler** sekmesinde tanımlanan üç yapı variab'les kullanır)
   - Swarm bitiş noktasıyla çalışmak için **DOCKER_HOST** değişkeni tanımlayın (:2375)
   - Önceki güvenli kopyalama görevi tarafından oluşturulan ve docker-compose.yml dosyasını içeren *dağıtım* klasörüne gidin 
   - Yeni `docker-compose` görüntüleri çeken, hizmetleri durduran, hizmetleri kaldıran ve kapsayıcıları oluşturan komutları yürütün.

     >[!IMPORTANT]
     > Önceki ekranda gösterildiği gibi, **STDERR** onay kutusunda Fail'i işaretsiz bırakın. Bu önemli bir ayardır, çünkü `docker-compose` kapsayıcılar gibi birkaç tanılama iletisini yazdırır, standart hata çıktısı üzerinde durdurulabilir veya silinir. Onay kutusunu işaretleseniz, Azure DevOps Hizmetleri her şey yolunda gitse bile sürüm sırasında hatalar oluştuğunu bildirir.
     >
1. Bu yeni sürüm ardışık hattını kaydedin.


>[!NOTE]
>Eski hizmetleri durdurup yenisini çalıştırdığımız için bu dağıtım biraz kapalı kalma süresi içerir. Mavi-yeşil bir dağıtım yaparak bunu önlemek mümkündür.
>

## <a name="step-4-test-the-cicd-pipeline"></a>4. Adım. CI/CD ardışık hattını test edin

Şimdi yapılandırma ile yapılır, bu yeni CI / CD ardışık zaman. Bunu test etmenin en kolay yolu kaynak kodu güncelleştirmek ve değişiklikleri GitHub deponuza işlemektir. Kodu ittikten birkaç saniye sonra Azure DevOps Hizmetlerinde çalışan yeni bir yapı görürsünüz. Başarıyla tamamlandığında, yeni bir sürüm tetiklenir ve uygulamanın yeni sürümünü Azure Kapsayıcı Hizmeti kümesine dağıtacaktır.

## <a name="next-steps"></a>Sonraki Adımlar

* Azure DevOps Hizmetleri ile CI/CD hakkında daha fazla bilgi için [Azure Ardışık Hatları Belgeleri](/azure/devops/pipelines/?view=azure-devops) makalesine bakın.
