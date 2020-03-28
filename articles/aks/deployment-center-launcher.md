---
title: Azure Kubernetes dağıtım merkezi
description: Azure DevOps'teki Dağıtım Merkezi, uygulamanız için sağlam bir Azure DevOps ardışık hattı oluşturmayı kolaylaştırır
ms.author: puagarw
ms.topic: tutorial
ms.date: 07/12/2019
author: pulkitaggarwl
ms.openlocfilehash: b0a9597e370648faab3787218c7d038798dbd455
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "80048119"
---
# <a name="deployment-center-for-azure-kubernetes"></a>Azure Kubernetes dağıtım merkezi

Azure DevOps'teki Dağıtım Merkezi, uygulamanız için sağlam bir Azure DevOps ardışık hattı oluşturmayı kolaylaştırır. Dağıtım Merkezi varsayılan olarak, uygulama güncelleştirmelerinizi Kubernetes kümesine dağıtmak için bir Azure DevOps ardışık hattı yapılandırır. Varsayılan olarak yapılandırılan Azure DevOps ardışık hattını genişletebilir ve daha zengin özellikler de ekleyebilirsiniz: dağıtmadan önce onay alma, ek Azure kaynakları sağlama, komut dosyalarını çalıştırma, uygulamanızı yükseltme ve hatta daha fazla doğrulama testi çalıştırma.

Bu öğreticide şunları yapacaksınız:

> [!div class="checklist"]
> * Uygulama güncelleştirmelerinizi Kubernetes kümesine dağıtmak için bir Azure DevOps ardışık hattı yapılandırın.
> * Sürekli tümleştirme (CI) ardışık hattını inceleyin.
> * Sürekli teslimat (CD) ardışık hattını inceleyin.
> * Kaynakları temizleyin.

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği. [Visual Studio Dev Essentials](https://visualstudio.microsoft.com/dev-essentials/) aracılığıyla ücretsiz bir abonelik alabilirsiniz.

* Azure Kubernetes Hizmeti (AKS) kümesi.

## <a name="create-an-aks-cluster"></a>AKS kümesi oluşturma

1. [Azure portalınızda](https://portal.azure.com/)oturum açın.

1. Azure portalındaki menü çubuğunun sağ tarafındaki [Bulut Kabuğu](https://docs.microsoft.com/azure/cloud-shell/overview) seçeneğini seçin.

1. AKS kümesini oluşturmak için aşağıdaki komutları çalıştırın:

    ```azurecli
    # Create a resource group in the South India location:

    az group create --name azooaks --location southindia

    # Create a cluster named azookubectl with one node.

    az aks create --resource-group azooaks --name azookubectl --node-count 1 --enable-addons monitoring --generate-ssh-keys
    ```

## <a name="deploy-application-updates-to-a-kubernetes-cluster"></a>Uygulama güncelleştirmelerini bir Kubernetes kümesine dağıtma

1. Önceki bölümde oluşturduğunuz kaynak grubuna gidin.

1. AKS kümesini seçin ve ardından sol bıçaktaki **Dağıtım Merkezi'ni (önizleme)** seçin. **Başlat'ı**seçin.

   ![ayarlar](media/deployment-center-launcher/settings.png)

1. Kodun konumunu seçin ve **İleri'yi**seçin. Ardından, şu anda desteklenen depolardan birini seçin: **[Azure Repos](https://docs.microsoft.com/azure/devops/repos/index?view=azure-devops)** veya **GitHub.**

    Azure Repos, kodunuzu yönetmenize yardımcı olan bir sürüm denetim araçları kümesidir. Yazılım projenizin büyük veya küçük olup olmadığı, sürüm denetimini mümkün olduğunca erken kullanmak iyi bir fikirdir.

    - **Azure Deposu**: Varolan projenizden ve kuruluşunuzdan bir depo seçin.

        ![Azure Repos](media/deployment-center-launcher/azure-repos.gif)

    - **GitHub**: GitHub hesabınız için depoyu yetkilendirmek ve seçin.

        ![GitHub](media/deployment-center-launcher/github.gif)


1. Deployment Center depoyu analiz eder ve Dockerfile'ınızı algılar. Dockerfile'ı güncelleştirmek istiyorsanız, tanımlanan bağlantı noktası numarasını da atabilirsiniz.

    ![Uygulama Ayarları](media/deployment-center-launcher/application-settings.png)

    Depo Dockerfile içermiyorsa, sistem bir tane işlemek için bir ileti görüntüler.

    ![Dockerdosyası](media/deployment-center-launcher/dockerfile.png)

1. Varolan bir kapsayıcı kayıt defterini seçin veya bir tane oluşturun ve sonra **Finish'i**seçin. Ardışık iş, otomatik olarak oluşturulur ve [Azure Ardışık Hatları'nda](https://docs.microsoft.com/azure/devops/pipelines/index?view=azure-devops)bir yapıyı sıraya alır.

    Azure Ardışık Hatları, kod projenizi otomatik olarak oluşturmak ve test etmek ve diğer kullanıcılar tarafından kullanılabilir hale getirmek için kullanabileceğiniz bir bulut hizmetidir. Azure Pipelines sürekli tümleştirme ve sürekli teslimatı birleştirerek kodunuzu sürekli ve sürekli olarak test eder, oluşturur ve herhangi bir hedefe sevk eder.

    ![Container Kayıt Defteri](media/deployment-center-launcher/container-registry.png)

1. Devam eden ardışık hattı görmek için bağlantıyı seçin.

1. Dağıtım tamamlandıktan sonra başarılı günlükleri görürsünüz.

    ![Günlükler](media/deployment-center-launcher/logs.png)

## <a name="examine-the-ci-pipeline"></a>CI işlem hattını inceleme

Dağıtım Merkezi, Azure DevOps kuruluşunuzun CI/CD ardışık hattını otomatik olarak yapılandırır. Boru hattı keşfedilebilir ve özelleştirilebilir.

1. Dağıtım Merkezi panosuna gidin.  

1. Projenizin yapı ardışık hattını görüntülemek için başarılı günlükler listesinden yapı numarasını seçin.

1. Sağ üst köşedeki elipsleri (...) seçin. Menü, yeni bir yapıyı sıraya alma, yapıyı tutma ve yapı ardışık hattını düzenleme gibi çeşitli seçenekler gösterir. **Ardışık hattı edit'i**seçin. 

1. Yapı ardışık hattınız için farklı görevleri bu bölmede inceleyebilirsiniz. Yapı, Git deposundan kaynak toplama, görüntü oluşturma, görüntüyü kapsayıcı kayıt defterine itme ve dağıtımlar için kullanılan çıktıları yayımlama gibi çeşitli görevleri gerçekleştirir.

1. Ardışık listenin üst kısmındaki yapı ardışık hattının adını seçin.

1. Yapı ardışık yapı adınızı daha açıklayıcı bir şeyle değiştirin, **& sırasını kaydet'i**seçin ve sonra **Kaydet'i**seçin.

1. Yapı ardınız altında **Geçmiş'i**seçin. Bu bölme, son yapı değişikliklerinizin denetim izini gösterir. Azure DevOps, yapı denetim hattında yapılan değişiklikleri izler ve sürümleri karşılaştırmanızı sağlar.

1. **Tetikleyiciler**’i seçin. Dalları CI işleminden dahil edebilir veya hariç tutabilirsiniz.

1. **Saklama**’yı seçin. Senaryonuza bağlı olarak bir dizi yapıyı tutmak veya kaldırmak için ilkeler belirtebilirsiniz.

## <a name="examine-the-cd-pipeline"></a>CD işlem hattını inceleme

Deployment Center, Azure DevOps kuruluşunuzla Azure aboneliğiniz arasındaki ilişkiyi otomatik olarak oluşturur ve yapılandırır. Söz konusu adımlar arasında Azure DevOps ile Azure aboneliğinizin kimliğini doğrulamak için bir Azure hizmet bağlantısı kurulması yer almaktadır. Otomatikleştirilmiş işlem, Azure'a sürekli teslimat sağlayan bir sürüm ardışık hattı da oluşturur.

1. **Ardışık Hatlar'ı**seçin ve ardından **Sürümler'i**seçin.

1. Sürüm ardışık hattını sağlamak için **Edit'i**seçin.

1. **Yapılar** listesinden **Bırak'ı** seçin. Önceki adımlarda, incelediğiniz inşaat boru hattı yapı için kullanılan çıktıyı üretir. 

1. **Bırak** seçeneğinin sağındaki **Sürekli dağıtım** tetikleyicisini seçin. Bu sürüm ardışık yapısı, yeni bir yapı artifakı kullanılabilir olduğunda dağıtımı çalıştıran etkin bir CD tetikleyicisi vardır. Ayrıca, dağıtımlarınız için el ile yürütme gerektirecek şekilde tetikleyiciyi devre dışı kullanabilirsiniz.

1. Ardışık alanınızın tüm görevlerini incelemek için **Görevler'i**seçin. Sürüm çapa makinesi ortamını ayarlar, `imagePullSecrets` parametreyi yapılandırır, Helm araçlarını yükler ve Miğfer grafiklerini Kubernetes kümesine dağıtır.

1. Sürüm geçmişini görüntülemek **için, görünüm sürümlerini**seçin.

1. Özeti görmek için **Yayın 'ı**seçin. Sürüm özeti, ilişkili iş öğeleri ve testler gibi birden çok menüyü keşfetmek için aşamalardan birini seçin. 

1. **İşlemeler**'i seçin. Bu görünüm, bu dağıtımla ilgili kod taahhütlerini gösterir. Dağıtımlar arasındaki işleme farklarını görmek için sürümleri karşılaştırın.

1. **Günlükleri**seçin. Günlükler, dağıtımlar sırasında ve sonrasında görüntülediğiniz yararlı dağıtım bilgileri içerir.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık ihtiyacınız olmadığında oluşturduğunuz ilgili kaynakları silebilirsiniz. DevOps Projects panosundaki silme işlevini kullanın.

## <a name="next-steps"></a>Sonraki adımlar

Ekibinizin ihtiyaçlarını karşılamak için bu derleme ve yayın işlem hatlarını değiştirebilirsiniz. Veya, bu CI/CD modelini diğer ardışık hatlar için şablon olarak kullanabilirsiniz.
