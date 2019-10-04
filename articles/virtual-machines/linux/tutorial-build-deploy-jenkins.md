---
title: Eğitim-Jenkins 'den Azure VM 'lerine Azure DevOps Services | Microsoft Docs
description: Bu öğreticide, Visual Studio Team Services veya Microsoft Team Foundation Server Release Management Azure VM 'lerinde Jenkins kullanarak bir Node. js uygulaması için sürekli tümleştirme (CI) ve sürekli dağıtım (CD) ayarlamayı öğreneceksiniz.
author: tomarchermsft
manager: jpconnock
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-linux
ms.topic: tutorial
ms.tgt_pltfrm: jenkins
ms.workload: infrastructure
ms.date: 07/31/2018
ms.author: tarcher
ms.custom: jenkins
ms.openlocfilehash: 42e1ede7c762fef1f33436ea4c757b0c0531a8f2
ms.sourcegitcommit: 7c2dba9bd9ef700b1ea4799260f0ad7ee919ff3b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/02/2019
ms.locfileid: "71828439"
---
# <a name="tutorial-deploy-your-app-to-linux-virtual-machines-in-azure-with-using-jenkins-and-azure-devops-services"></a>Öğretici: Azure 'da Jenkins ve Azure DevOps Services kullanarak uygulamanızı Linux sanal makinelerine dağıtın

Sürekli tümleştirme (CI) ve sürekli dağıtım (CD), kodunuzu oluşturabileceğiniz, yayıntabileceğiniz ve dağıtabileceğiniz bir işlem hattı oluşturur. Azure DevOps Services Azure 'a dağıtmak için eksiksiz ve tam özellikli bir CI/CD otomasyon araçları kümesi sağlar. Jenkins, CI/CD Otomasyonu da sağlayan, popüler bir üçüncü taraf CI/CD sunucu tabanlı araçtır. Bulut uygulamanızı veya hizmetinizi nasıl teslim etmek istediğinizi özelleştirmek için Azure DevOps Services ve Jenkins 'i birlikte kullanabilirsiniz.

Bu öğreticide, bir Node. js web uygulaması oluşturmak için Jenkins kullanırsınız. Daha sonra bu uygulamayı dağıtmak için Azure DevOps kullanın

Linux sanal makineleri (VM 'Ler) içeren bir [dağıtım grubuna](https://docs.microsoft.com/azure/devops/pipelines/release/deployment-groups/index?view=vsts) . Aşağıdakilerin nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Örnek uygulamayı alın.
> * Jenkins eklentilerini yapılandırın.
> * Node. js için Jenkins Freestyle projesi yapılandırın.
> * Azure DevOps Services tümleştirme için Jenkins 'i yapılandırın.
> * Jenkins hizmet uç noktası oluşturun.
> * Azure sanal makineleri için bir dağıtım grubu oluşturun.
> * Azure Pipelines bir yayın işlem hattı oluşturun.
> * El ile ve CI tarafından tetiklenen dağıtımları yürütün.

## <a name="before-you-begin"></a>Başlamadan önce

* Jenkins sunucusuna erişmeniz gerekir. Henüz bir Jenkins sunucusu oluşturmadıysanız, bkz. [Azure sanal makinesi üzerinde Jenkins ana şablonu oluşturma](https://docs.microsoft.com/azure/jenkins/install-jenkins-solution-template). 

* Azure DevOps Services kuruluşunuzda oturum açın (**https://{yourorganization}. VisualStudio. com**). 
  [Ücretsiz bir Azure DevOps Services kuruluş](https://go.microsoft.com/fwlink/?LinkId=307137&clcid=0x409&wt.mc_id=o~msft~vscom~home-vsts-hero~27308&campaign=o~msft~vscom~home-vsts-hero~27308)edinebilirsiniz.

  > [!NOTE]
  > Daha fazla bilgi için bkz. [Connect Azure DevOps Services](https://docs.microsoft.com/azure/devops/organizations/projects/connect-to-projects?view=vsts).

*  Dağıtım hedefi için bir Linux sanal makinesine ihtiyacınız vardır.  Daha fazla bilgi için bkz. [Azure CLI Ile Linux VM 'Leri oluşturma ve yönetme](https://docs.microsoft.com/azure/virtual-machines/linux/tutorial-manage-vm).

*  Sanal makineniz için 80 numaralı bağlantı noktasını açın. Daha fazla bilgi için, bkz. [Azure Portal kullanarak ağ güvenlik grupları oluşturma](https://docs.microsoft.com/azure/virtual-network/tutorial-filter-network-traffic).

## <a name="get-the-sample-app"></a>Örnek uygulamayı al

Dağıtım için bir git deposunda depolanan bir uygulamaya ihtiyacınız vardır.
Bu öğreticide, [GitHub 'dan kullanılabilen bu örnek uygulamayı](https://github.com/azure-devops/fabrikam-node)kullanmanızı öneririz. Bu öğreticide, Node. js ve bir uygulamayı yüklemek için kullanılan örnek bir betik yer almaktadır. Kendi deponuzla çalışmak istiyorsanız benzer bir örnek yapılandırmanız gerekir.

Bu uygulamanın bir çatalını oluşturun ve Bu öğreticinin sonraki adımlarında kullanmak üzere konumu (URL) göz önünde faydalanın. Daha fazla bilgi için bkz. [bir depoyu çatalla](https://help.github.com/articles/fork-a-repo/).    

> [!NOTE]
> Uygulama [Yeumman](https://yeoman.io/learning/index.html)ile oluşturulmuştur. Express, Bower ve grsıt kullanır. Bağımlılıklar olarak bazı NPM paketleri vardır.
> Örnek ayrıca NGINX 'i ayarlayan ve uygulamayı dağıtan bir komut dosyası içerir. Sanal makinelerde yürütülür. Özel olarak, komut dosyası:
> 1. Node, NGINX ve PM2 'yi yüklüyor.
> 2. NGINX ve PM2 yapılandırır.
> 3. Düğüm uygulamasını başlatır.

## <a name="configure-jenkins-plug-ins"></a>Jenkins eklentilerini yapılandırma

İlk olarak, iki Jenkins eklentisi yapılandırmanız gerekir: **NodeJS** ve **vs Team Services sürekli dağıtımı**.

1. Jenkins hesabınızı açın ve **Jenkins 'ı Yönet**' i seçin.
2. **Jenkins yönetme** sayfasında, **Eklentileri Yönet**' i seçin.
3. **NodeJS** eklentisini bulmak için listeyi filtreleyin ve **yeniden başlatma olmadan Install** seçeneğini belirleyin.
    ![, Jenkins @ no__t-1 ' e NodeJS eklentisi ekleniyor
4. **Vs Team Services sürekli dağıtım** eklentisini bulmak için listeyi filtreleyin ve **yeniden başlatma olmadan yüklemeyi** seçin.
5. Jenkins panosuna dönün ve **Jenkins 'ı Yönet**' i seçin.
6. **Genel araç yapılandırması**' nı seçin. **NodeJS** bulun ve **NodeJS yüklemelerini**seçin.
7. **Otomatik olarak Install** seçeneğini belirleyip bir **ad** değeri girin.
8. **Kaydet**' i seçin.

## <a name="configure-a-jenkins-freestyle-project-for-nodejs"></a>Node. js için Jenkins Freestyle projesi yapılandırma

1. **Yeni öğe**' yi seçin. Bir öğe adı girin.
2. **Freestyle projesi**seçin. **Tamam ' ı**seçin.
3. **Kaynak kodu yönetimi** sekmesinde **Git** ' i seçin ve deponun ayrıntılarını ve uygulama kodunuzu içeren dalı girin.    
    ![Derlemenize bir depo ekleyin @ no__t-1
4. **Derleme tetikleyicileri** sekmesinde, her üç dakikada bir değişiklikler için Git deposunu YOKLAMAK Için **SCM yoklama** ' yı seçin ve `H/03 * * * *` zamanlamasını girin. 
5. **Yapı ortamı** sekmesinde, **düğüm &amp; NPM BIN/klasör yolu sağla** ' yı seçin ve **NodeJS yükleme** değerini seçin. **Sistem varsayılanını kullanmak**için **npmrc dosya** kümesini bırakın.
6. Tüm bağımlılıkların güncelleştirildiğinden emin olmak için **derleme** sekmesinde **kabuğu Çalıştır** ' ı seçin ve `npm install` komutunu girin.


## <a name="configure-jenkins-for-azure-devops-services-integration"></a>Azure DevOps Services tümleştirme için Jenkins yapılandırma

> [!NOTE]
> Aşağıdaki adımlar için kullandığınız kişisel erişim belirtecinin (PAT) Azure DevOps Services içindeki *yayın* (okuma, yazma, yürütme ve yönetme) iznini içerdiğinden emin olun.
 
1.  Henüz bir tane yoksa Azure DevOps Services kuruluşunuzda bir PAT oluşturun. Jenkins bu bilgilerin Azure DevOps Services kuruluşunuza erişmesini gerektirir. Yaklaşan adımlarla ilgili belirteç bilgilerini bu bölümde sakladığınızdan emin olun.
  
    Belirteç oluşturmayı öğrenmek için [Nasıl yaparım? Azure DevOps Services kişisel erişim belirteci oluşturma](https://docs.microsoft.com/azure/devops/organizations/accounts/use-personal-access-tokens-to-authenticate?view=vsts)' yı okuyun.
2. **Derleme sonrası eylemler** sekmesinde, **oluşturma sonrası eylem Ekle**' yi seçin. **Yapıtları Arşivle**' yi seçin.
3. **Arşivlenecek dosyalar**için tüm dosyaları dahil etmek için `**/*` girin.
4. Başka bir eylem oluşturmak için, **oluşturma sonrası eylem Ekle**' yi seçin.
5. **TFS/Team Services 'Ta tetikleyici yayını**seçin. Azure DevOps Services kuruluşunuzun URI 'sini girin (örneğin, **https://{kuruluşunuzun-adı}. VisualStudio. com**).
6. **Proje** adını girin.
7. Yayın işlem hattı için bir ad seçin. (Bu yayın işlem hattını daha sonra Azure DevOps Services oluşturun.)
8. Azure DevOps Services veya Team Foundation Server ortamınıza bağlanmak için kimlik bilgilerini seçin:
   - Azure DevOps Services kullanıyorsanız **Kullanıcı adını** boş bırakın. 
   - Team Foundation Server şirket içi bir sürümünü kullanıyorsanız, bir Kullanıcı adı ve parola girin.    
   ![Jenkins oluşturma sonrası eylemleri yapılandırma](media/tutorial-build-deploy-jenkins/trigger-release-from-jenkins.png)
5. Jenkins projesini kaydedin.


## <a name="create-a-jenkins-service-endpoint"></a>Jenkins hizmet uç noktası oluşturma

Hizmet uç noktası, Azure DevOps Services Jenkins 'e bağlanmasına izin verir.

1. Azure DevOps Services ' de **Hizmetler** sayfasını açın, **yeni hizmet uç noktası** listesini açın ve **Jenkins**' i seçin.
   ![Bir Jenkins uç noktası ekleyin @ no__t-1
2. Bağlantı için bir ad girin.
3. Jenkins sunucunuzun URL 'sini girin ve **GÜVENILMEYEN SSL sertifikalarını kabul et** seçeneğini belirleyin. Örnek bir URL **http://{YourJenkinsURL}. westcentralus. cloudapp. Azure. com**' dır.
4. Jenkins hesabınız için Kullanıcı adını ve parolayı girin.
5. Bilgilerin doğru olup olmadığını denetlemek için **bağlantıyı doğrula** ' yı seçin.
6. Hizmet uç noktasını oluşturmak için **Tamam ' ı** seçin.

## <a name="create-a-deployment-group-for-azure-virtual-machines"></a>Azure sanal makineleri için bir dağıtım grubu oluşturma

Yayın işlem hattının sanal makinenize dağıtılması için Azure DevOps Services aracısını kaydettirmek üzere bir [dağıtım grubuna](https://www.visualstudio.com/docs/build/concepts/definitions/release/deployment-groups/) ihtiyacınız vardır. Dağıtım grupları, dağıtım için hedef makinelerin mantıksal gruplarını tanımlamanızı ve gerekli aracıyı her makineye yüklemeyi kolaylaştırır.

   > [!NOTE]
   > Aşağıdaki yordamda, önkoşulları yüklediğinizden ve *betiği sudo ayrıcalıklarıyla çalıştırmayın.*

1. **Derleme &amp; yayın** hub 'ının **yayınlar** sekmesini açın, **dağıtım grupları**' nı açın ve **+ Yeni**' yi seçin.
2. Dağıtım grubu için bir ad ve isteğe bağlı bir açıklama girin. Ardından **Oluştur**' u seçin.
3. Dağıtım hedefi sanal makineniz için işletim sistemini seçin. Örneğin, **Ubuntu 16.04 +** öğesini seçin.
4. **Kimlik doğrulaması için betikte kişisel erişim belirteci kullan '** ı seçin.
5. **Sistem önkoşulları** bağlantısını seçin. İşletim sisteminiz için önkoşulları yükler.
6. Betiği kopyalamak için **betiği panoya kopyala** ' yı seçin.
7. Dağıtım hedefi sanal makinenizde oturum açın ve betiği çalıştırın. Betiği sudo ayrıcalıklarıyla çalıştırmayın.
8. Yüklemeden sonra dağıtım grubu etiketleri istenir. Varsayılanları kabul edin.
9. Azure DevOps Services ' de, **dağıtım grupları**altındaki **hedefler** ' de yeni kayıtlı sanal makinenize bakın.

## <a name="create-an-azure-pipelines-release-pipeline"></a>Azure Pipelines yayın işlem hattı oluşturma

Yayın ardışık düzeni, Azure Pipelines uygulamayı dağıtmak için kullandığı işlemi belirtir. Bu örnekte, bir kabuk betiği yürütülecektir.

Azure Pipelines sürümünde yayın işlem hattını oluşturmak için:

1. **Build &amp; yayın** hub 'ının **yayınlar** sekmesini açın ve **yayın işlem hattı oluştur**' u seçin. 
2. Boş bir **işlemle**başlatmayı seçerek **boş** şablonu seçin.
3. **Yapıtlar** bölümünde **+ yapıt Ekle** ' yi seçin ve **kaynak türü**için **Jenkins** ' i seçin. Jenkins hizmet uç noktası bağlantınızı seçin. Sonra Jenkins kaynak işini seçin ve **Ekle**' yi seçin.
4. **Ortam 1**' in yanındaki üç noktayı seçin. **Dağıtım grubu Ekle aşamasını**seçin.
5. Dağıtım grubunuzu seçin.
5. **Dağıtım grubu aşamasına**bir görev eklemek için **+** ' i seçin.
6. **Kabuk betiği** görevi ' ni seçin ve **Ekle**' yi seçin. **Kabuk betiği** görevi, Node. js ' nin yüklenebilmesi ve uygulamayı başlatmak için her sunucuda çalışacak bir betik için yapılandırma sağlar.
8. **Betik yolu**için **$ (System. DefaultWorkingDirectory)/Fabrikam-node/deployscript.sh**girin.
9. **Gelişmiş**' i seçin ve ardından **çalışma dizinini belirt**' i etkinleştirin.
10. **Çalışma dizini**için **$ (System. DefaultWorkingDirectory)/Fabrikam-node**girin.
11. Yayın Işlem hattının adını Jenkins 'teki derleme **sonrası eylemler** sekmesinde belirttiğiniz adla düzenleyin. Jenkins, kaynak yapıtları güncelleştirilirken bu adın yeni bir yayın tetikleyebilmesini gerektirir.
12. **Kaydet** ' i seçin ve sürüm ardışık düzenini kaydetmek için **Tamam** ' ı seçin.

## <a name="execute-manual-and-ci-triggered-deployments"></a>El ile ve CI tarafından tetiklenen dağıtımları yürütün

1. **+ Yayın** ' ı seçin ve **yayın oluştur**' u seçin.
2. Vurgulanan açılan listede tamamladığınız derlemeyi seçin ve **kuyruk**' ı seçin.
3. Açılan iletideki yayın bağlantısını seçin. Örneğin: "yayın **yayını-1** oluşturuldu."
4. Yayın konsolu çıktısını izlemek için **Günlükler** sekmesini açın.
5. Tarayıcınızda, dağıtım grubunuza eklediğiniz sunuculardan birinin URL 'sini açın. Örneğin, **http://{-Server-ip-Address}** yazın.
6. Kaynak git deposuna gidin ve App/views/index. Jade dosyasındaki **H1** başlığının içeriğini değişen metinle değiştirin.
7. Değişiklerinizi işleyin.
8. Birkaç dakika sonra Azure DevOps 'ın **yayınlar** sayfasında oluşturulan yeni bir sürüm görürsünüz. Dağıtım gerçekleşmesini görmek için yayını açın. Mühendisi!

## <a name="troubleshooting-the-jenkins-plugin"></a>Jenkins eklentisinin sorunlarını giderme

Jenkins eklentileriyle herhangi bir hatayla karşılaşırsanız, belirli bir bileşen için [Jenkins JIRA](https://issues.jenkins-ci.org/) 'de bir sorun verin.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, derleme için Jenkins 'i kullanarak bir uygulamanın Azure 'a dağıtımını otomatik hale getirerek yayın için Azure DevOps Services. Şunları öğrendiniz:

> [!div class="checklist"]
> * Uygulamanızı Jenkins 'te derleyin.
> * Azure DevOps Services tümleştirme için Jenkins 'i yapılandırın.
> * Azure sanal makineleri için bir dağıtım grubu oluşturun.
> * VM 'Leri yapılandıran ve uygulamayı dağıtan bir yayın işlem hattı oluşturun.

LAMBA (Linux, Apache, MySQL ve PHP) yığınının nasıl dağıtılacağı hakkında bilgi edinmek için sonraki öğreticiye ilerleyin.

> [!div class="nextstepaction"]
> [LAMBA yığınını dağıt](tutorial-lamp-stack.md)
