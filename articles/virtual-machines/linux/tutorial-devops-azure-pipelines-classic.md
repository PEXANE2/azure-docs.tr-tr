---
title: Öğretici - Azure'da IaaS ve PaaS için Entegre DevOps
description: Bu eğitimde, Azure Pipelines kullanarak bir uygulamanın Azure VM'lerine sürekli tümleştirme (CI) ve sürekli dağıtım (CD) ayarlamayı öğrenirsiniz.
author: ushan
manager: jpconnock
tags: azure-devops-pipelines
ms.assetid: ''
ms.service: virtual-machines-linux
ms.topic: tutorial
ms.tgt_pltfrm: azure-pipelines
ms.workload: infrastructure
ms.date: 1/16/2020
ms.author: ushan
ms.custom: devops
ms.openlocfilehash: 4159bf27c39087926d982552c49c606f3484de77
ms.sourcegitcommit: d187fe0143d7dbaf8d775150453bd3c188087411
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80885914"
---
# <a name="tutorial-integrated-devops-for-iaas-and-paas-on-azure"></a>Öğretici: Azure'da IaaS ve PaaS için Entegre DevOps

Azure'da uçtan uca çözümlerle, ekipler DevOps uygulamalarını uygulama yaşam döngüsü aşamalarının her birinde uygulayabilir: planlayın, geliştirin, teslim edin ve çalıştırın. 

Aşağıda, bulut iş yüklerini basitleştiren ve inanılmaz derecede güçlü senaryolar etkinleştirmek için birleştirilebilen Azure Hizmetlerinden bazıları verilmiştir.
Bu teknolojiler, kişiler ve süreçlerle birlikte ekiplerin müşterilere sürekli değer sağlamasını sağlar. 

- Azure: https://portal.azure.com – Bulut iş yüklerini oluşturmak için portal. Basit web uygulamalarından karmaşık bulut uygulamalarına kadar her şeyi yönetin ve izleyin 
- Azure DevOps: https://dev.azure.com - Bir dizi modern geliştirme hizmetiyle daha akıllı plan yapın, daha iyi işbirliği yapın ve daha hızlı gönderim yapın 
- Azure Machine Learning https://ml.azure.com stüdyosu: - Makine öğrenimi modellerini hazırlayın, eğitin ve dağıtın 
 

Azure DevOps, DevOps işleminin her bir bölümünü herhangi bir Azure kaynağı için sürekli tümleştirme ve sürekli teslimatla otomatikleştiren yerleşik bir Azure hizmetidir.
Uygulamanız sanal makineler, web uygulamaları, Kubernetes veya başka bir kaynak kullansın, altyapıyı kod olarak, sürekli tümleştirme, sürekli sınama, sürekli teslim etme ve Azure ve Azure DevOps ile sürekli izleme olarak uygulayabilirsiniz.  
![AzDevOps_portalView](media/tutorial-devops-azure-pipelines-classic/azdevops-view.png) 
 
 
## <a name="iaas---configure-cicd"></a>IaaS - CI/CD'yi yapılandır 
Azure Pipelines, sanal makinelere dağıtımlar için tam ve tam özellikli bir CI/CD otomasyon araçları seti sağlar. Bir Azure VM için sürekli bir teslim ardışık hattını doğrudan Azure portalından yapılandırabilirsiniz. Bu belge, Azure portalından çok makineli dağıtımlar için bir CI/CD ardışık nokta ayarlamayla ilişkili adımları içerir. 


**Sanal Makinelerde CI/CD'yi yapılandırma**

Sanal makineler [dağıtım grubuna](https://docs.microsoft.com/azure/devops/pipelines/release/deployment-groups) hedef olarak eklenebilir ve çoklu makine güncelleştirmesi için hedeflenebilir. İhtiyaçlarınıza bağlı olarak, out-of-box dağıtım stratejileri, _Rolling,_ _Kanarya,_ _Mavi-Yeşil_ herhangi birini seçebilirsiniz veya daha fazla özelleştirebilirsiniz. Dağıtıldıktan sonra, Dağıtım Grupları içindeki Dağıtım Geçmişi görünümleri VM'den ardışık bölgeye ve ardından işlemeye izlenebilirlik sağlar. 
 
**Yuvarlama Dağıtımları**: Bir yuvarlama dağıtımı, her yinelemede sabit bir makine kümesinde (yuvarlanma kümesi) uygulamanın yeni sürümünün örnekleriyle bir uygulamanın önceki sürümünün örneklerini değiştirir. Yuvarlanan bir güncelleştirmeyi sanal makinelere nasıl yapılandırabileceğinizi gözden geçirelim.  
Sürekli teslim seçeneğini kullanarak Azure portalındaki " sanal makinelerinize " sanal makinelerinize " yuvarlanma**güncelleştirmelerini"** yapılandırabilirsiniz. 

İşte adım adım yol. 
1. Azure portalınızda oturum açın ve sanal bir makineye gidin. 
2. VM sol bölmesinde, sürekli **teslim** menüsüne gidin. Sonra **Yapıla'ya**tıklayın. 

   ![AzDevOps_configure](media/tutorial-devops-azure-pipelines-classic/azure-devops-configure.png) 
3. Yapılandırma panelinde, varolan bir hesabı seçmek veya bir hesap oluşturmak için "Azure DevOps Organizasyonu"nu tıklatın. Ardından, ardışık alanı yapılandırmak istediğiniz projeyi seçin.  


   ![AzDevOps_project](media/tutorial-devops-azure-pipelines-classic/azure-devops-rolling.png) 
4. Dağıtım grubu, fiziksel ortamları temsil eden mantıksal bir dağıtım hedef makinesi kümesidir; örneğin, "Dev", "Test", "UAT" ve "Üretim". Yeni bir dağıtım grubu oluşturabilir veya varolan bir dağıtım grubu seçebilirsiniz. 
5. Sanal makineye dağıtılacak paketi yayımlayan yapı ardışık hattını seçin. Yayınlanan paketin paket kökünde _deployscripts_ klasöründe bir dağıtım komut dosyası _dağıtımı.ps1_ veya _deploy.sh_ olması gerektiğini unutmayın. Bu dağıtım komut dosyası, çalışma zamanında Azure DevOps ardışık tarafından yürütülür.
6. Seçtiğiniz dağıtım stratejisini seçin. Bu durumda, 'Rolling' seçeneğini belirleyin.
7. İsteğe bağlı olarak, makineyi rol ile etiketleyebilirsiniz. Örneğin, 'web', 'db' vb. Bu, yalnızca belirli bir rolü olan VM'leri hedeflemenize yardımcı olur.
8. Sürekli teslim ardışık hattını yapılandırmak için iletişim kutusunda **Tamam'ı** tıklatın. 
9. Bir kez tamamlandığında, sanal makineye dağıtmak için yapılandırılmış sürekli bir teslimat ardışık alanı olacaktır.  


   ![AzDevOps_pipeline](media/tutorial-devops-azure-pipelines-classic/azure-devops-deployment-history.png)
10. Sanal makineye dağıtımın devam ettiğini göreceksiniz. Boru hattına gitmek için bağlantıyı tıklatabilirsiniz. Dağıtımı görüntülemek için **Sürüm-1'i** tıklatın. Veya sürüm ardışık alanı tanımını değiştirmek için **Edit'i** tıklatabilirsiniz. 
11. Yapılandırılacak birden çok VM'niz varsa, dağıtım grubuna eklenecek diğer VM'ler için 2-4 adımlarını yineleyin. Bir ardışık hatlar çalışmasının zaten var olduğu bir Dağıtım Grubu seçerseniz, VM'nin yeni ardışık işler oluşturmadan dağıtım grubuna ekolacağını unutmayın. 
12. Tamamlandığında, boru hattı tanımına tıklayın, Azure DevOps kuruluşuna gidin ve yayımardi hattını **edit'i** tıklatın. 
   ![AzDevOps_edit_pipeline](media/tutorial-devops-azure-pipelines-classic/azure-devops-rolling-pipeline.png)
13. Link 1 **iş,** **1 görev dev** aşamasında tıklayın. **Dağıt** aşamasına tıklayın.
   ![AzDevOps_deploymentGroup](media/tutorial-devops-azure-pipelines-classic/azure-devops-rolling-pipeline-tasks.png)
14. Sağdaki yapılandırma bölmesinden, her yinelemede paralel olarak dağıtmak istediğiniz makine sayısını belirtebilirsiniz. Aynı anda birden çok makineye dağıtmak istediğinizde, kaydırıcıyı kullanarak bunu yüzde açısından belirtebilirsiniz.  

15. Komut Dosyasını Yürüt görevi varsayılan olarak dağıtım komut dosyası _deploy.ps1_ veya _deploy.sh_ yayımlanmış paketin kök dizininde _dağıtım komut dosyası_ klasöründe yürütülür.
  
**Kanarya Dağıtımları**: Kanarya dağıtımı, küçük bir kullanıcı alt kümesine değişimi yavaş yavaş gerçekleştirerek riski azaltır. Yeni sürüme daha fazla güven kazandıkça, bu sürümü altyapınızdaki daha fazla sunucuya yayınlamaya ve daha fazla kullanıcıyı bu sürüme yönlendirmeye başlayabilirsiniz. Sürekli teslim seçeneğini kullanarak Azure portalı ile kanarya dağıtımlarını "**sanal makinelerinize"** göre yapılandırabilirsiniz. İşte adım adım yol. 
1. Azure portalınızda oturum açın ve sanal bir makineye gidin 
2. Dağıtım grubuna birden çok VM eklemek için **Rolling Deployments** bölümünün altındaki 2-7 adımları izleyin. Dağıtım stratejisi açılır için 'Kanarya'yı seçin.
![AzDevOps_configure_canary](media/tutorial-devops-azure-pipelines-classic/azure-devops-configure-canary.png)

3. Kanarya dağıtımlarının bir parçası olacak VM'lere bir 'kanarya' etiketi ve başarılı bir şekilde kanarya dağıtımından sonra dağıtımların bir parçası olan VM'lere 'prod' etiketi ekleyin.
4. Bir kez tamamlandığında, sanal makineye dağıtmak için yapılandırılmış sürekli bir teslimat ardışık alanı olacaktır.
![AzDevOps_canary_pipeline](media/tutorial-devops-azure-pipelines-classic/azure-devops-canary-pipeline.png)


5. Rolling **Deployments** bölümünde olduğu gibi, ardışık yapı yapılandırmasını görmek için Azure DevOps'teki Yayımet ardışık hattını **edit'e** tıklayabilirsiniz. Boru hattı 3 aşamadan oluşur - ilk aşama bir DG fazıdır ve _kanarya_olarak etiketlenmiş VM'lere dağıtır. İkinci aşama, boru hattını duraklatabilir ve çalıştırmaya devam etmek için manuel müdahalebekler. Bir kullanıcı kanarya dağıtımının kararlı olduğundan emin olduktan sonra, daha sonra _prod_olarak etiketlenmiş VM'lere dağıtılan üçüncü aşamayı çalıştıracak olan boru hattı çalıştırmaişlemine devam edebilir. ![AzDevOps_canary_task](media/tutorial-devops-azure-pipelines-classic/azure-devops-canary-task.png)



**Mavi-Yeşil Dağıtımlar**: Mavi-Yeşil dağıtım, aynı bekleme ortamına sahip olarak duruş süresini azaltır. Ortamlardan biri her zaman canlı. Yeni bir sürüm için hazırlanırken, yeşil ortamda test son aşamasıyapmak. Yazılım yeşil ortamda çalıştığında, gelen tüm isteklerin yeşil ortama gitmesi için trafiği değiştirin - mavi ortam artık boşta.
Sürekli teslim seçeneğini kullanarak Azure portalından Mavi-Yeşil**dağıtımları**" sanal makinelerinize" göre yapılandırabilirsiniz. 

İşte adım adım yol.

1. Azure portalınızda oturum açın ve Sanal Makine'ye gidin 
2. Dağıtım grubuna birden çok VM eklemek için **Rolling Deployments** bölümünün altındaki 2-7 adımları izleyin. Mavi-Yeşil dağıtımların bir parçası olacak VM'lere "mavi" veya "yeşil" etiketi ekleyin. VM bekleme rolü içinse, bunu "yeşil" olarak etiketlemeniz gerekir.
![AzDevOps_bluegreen_configure](media/tutorial-devops-azure-pipelines-classic/azure-devops-blue-green-configure.png)

4. Bir kez tamamlandığında, sanal makineye dağıtmak için yapılandırılmış sürekli bir teslimat ardışık alanı olacaktır.
![AzDevOps_bluegreen_pipeline](media/tutorial-devops-azure-pipelines-classic/azure-devops-blue-green-pipeline.png)

5. Rolling **Deployments'ta**olduğu gibi, ardışık yapı yapılandırmasını görmek için Azure DevOps'te **yayımardi edit'i** tıklatabilirsiniz. Boru hattı 3 aşamadan oluşur - ilk aşama bir DG aşamasıdır ve _yeşil_ (bekleme VM' leri) olarak etiketlenen VM'lere dağıtır. İkinci aşama boru hattını duraklatabilir ve çalıştırmaya devam etmek için el ile müdahaleyi bekler. Bir kullanıcı dağıtımın kararlı olduğundan emin olduğunda, artık trafiği _yeşil_ VM'lere yönlendirebilir ve vm'lerde _mavi_ ve _yeşil_ etiketleri değiştirebilecek olan ardışık hatlar çalışmasını sürdürebilir. Bu, eski uygulama sürümüne sahip VM'lerin _yeşil_ olarak etiketlenmelerini ve bir sonraki ardışık işlem de dağıtılmalarını sağlar.
![AzDevOps_bluegreen_task](media/tutorial-devops-azure-pipelines-classic/azure-devops-blue-green-tasks.png)

6. Bu dağıtım stratejisinin her biri mavi ve yeşil olarak etiketlenmiş en az bir VM olmasını gerektirdiğini unutmayın. El Ile Müdahale adımında boru hattı çalıştırmayı devam ettirmek ten önce, _mavi_olarak etiketlenmiş en az bir VM olduğundan emin olun.





 
## <a name="azure-devops-project"></a>Azure DevOps projesi 
Azure ile her zamankinden daha kolay başlayın.
 
DevOps Projects ile uygulamanızı herhangi bir Azure hizmetinde yalnızca üç adımda çalıştırmaya başlayın: uygulama dili, çalışma zamanı ve Azure hizmeti seçin.
 
[Daha fazla bilgi edinin](https://azure.microsoft.com/features/devops-projects/ ).
 
## <a name="additional-resources"></a>Ek kaynaklar 
- [DevOps projesini kullanarak Azure Sanal Makinelere dağıtın](https://docs.microsoft.com/azure/devops-project/azure-devops-project-vms)
- [Uygulamanızın Azure Sanal Makine Ölçeği Setine sürekli dağıtımını uygulayın](https://docs.microsoft.com/azure/devops/pipelines/apps/cd/azure/deploy-azure-scaleset)
