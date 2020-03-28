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
ms.openlocfilehash: 5707a99b329915b35131fe793b0dfabd02348677
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "77912537"
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
Azure Pipelines, sanal makinelere dağıtımlar için tam ve tam özellikli bir CI/CD otomasyon araçları seti sağlar. Bir Azure VM için sürekli bir teslim ardışık hattını doğrudan Azure portalından yapılandırabilirsiniz. Bu belge, Azure portalından çok makineli dağıtımlar için bir CI/CD ardışık nokta ayarlamayla ilişkili adımları içerir. Sanal Makinelerde CI/CD'yi yapılandırın.

Sanal makineler dağıtım [grubuna](https://docs.microsoft.com/azure/devops/pipelines/release/deployment-groups) hedef olarak eklenebilir ve çok makineli haddeleme güncelleştirmeleri için hedeflenebilir. Dağıtım grupları içindeki dağıtım geçmişi görünümleri VM'den ardışık almaya ve ardından işlemeye kadar izlenebilirlik sağlar. 
 
**Yuvarlama güncelleştirmeleri**: Yuvarlama dağıtımı, her yinelemede sabit bir makine kümesindeki (yuvarlanma kümesi) uygulamanın yeni sürümünün örnekleriyle bir uygulamanın önceki sürümünün örneklerini değiştirir. Yuvarlanan bir güncelleştirmeyi sanal makinelere nasıl yapılandırabileceğinizi gözden geçirelim.  
Sürekli teslim seçeneğini kullanarak Azure portalındaki " sanal makinelerinize " sanal makinelerinize " yuvarlanma**güncelleştirmelerini"** yapılandırabilirsiniz. 

İşte adım adım yol. 
1. Azure portalınızda oturum açın ve sanal bir makineye gidin. 
2. VM sol bölmesinde, sürekli **teslim** menüsüne gidin. Sonra **Yapıla'ya**tıklayın. 
   ![AzDevOps_configure](media/tutorial-devops-azure-pipelines-classic/azdevops-configure.png) 
3. Yapılandırma panelinde, varolan bir hesabı seçmek veya bir hesap oluşturmak için "Azure DevOps Organizasyonu"nu tıklatın. Ardından, ardışık alanı yapılandırmak istediğiniz projeyi seçin.  
   ![AzDevOps_project](media/tutorial-devops-azure-pipelines-classic/azdevops-project.png) 
4. Dağıtım grubu, fiziksel ortamları temsil eden mantıksal bir dağıtım hedef makinesi kümesidir; örneğin, "Dev", "Test", "UAT" ve "Üretim". Yeni bir dağıtım grubu oluşturabilir veya varolan bir dağıtım grubu seçebilirsiniz. Makineyi isteğe bağlı olarak rol ile etiketleyebilirsiniz. Örneğin, 'web', 'db' vb.  
5. Sürekli teslim ardışık hattını yapılandırmak için iletişim kutusunda **Tamam'ı** tıklatın. 
6. Bir kez tamamlandığında, sanal makineye dağıtmak için yapılandırılmış sürekli bir teslimat ardışık alanı olacaktır.  
   ![AzDevOps_pipeline](media/tutorial-devops-azure-pipelines-classic/azdevops-pipeline.png)
7. Sanal makineye dağıtımın devam ettiğini göreceksiniz. Boru hattına gitmek için bağlantıyı tıklatabilirsiniz. Dağıtımı görüntülemek için **Sürüm-1'i** tıklatın. Veya sürüm ardışık alanı tanımını değiştirmek için **Edit'i** tıklatabilirsiniz. 
8. Yapılandırılacak birden çok VM'niz varsa, dağıtım grubuna eklenecek diğer VM'ler için 2-5 adımlarını yineleyin. 
9. Tamamlandığında, boru hattı tanımına tıklayın, Azure DevOps kuruluşuna gidin ve yayımardi hattını **edit'i** tıklatın. 
   ![AzDevOps_edit_pipeline](media/tutorial-devops-azure-pipelines-classic/azdevops-edit-pipeline.png)
10. Link 1 **iş,** **1 görev dev** aşamasında tıklayın. **Dağıt** aşamasına tıklayın.  
   ![AzDevOps_deploymentGroup](media/tutorial-devops-azure-pipelines-classic/azdevops-deployment-group.png)
11. Sağdaki yapılandırma bölmesinden, varsayılan olarak ardışık yapının tüm hedeflere paralel olarak yuvarlanma güncelleştirmesi yapacak şekilde yapılandırıldığı görebilirsiniz. Kaydırıcıyı kullanarak dağıtımları teker teker veya yüzde açısından olacak şekilde yapılandırabilirsiniz.  
  
  
**Kanarya** yavaş yavaş kullanıcıların küçük bir alt kümesine değişiklik dışarı haddeleme tarafından riski azaltır. Yeni sürüme daha fazla güven kazandıkça, bu sürümü altyapınızdaki daha fazla sunucuya yayınlamaya ve daha fazla kullanıcıyı bu sürüme yönlendirmeye başlayabilirsiniz. Sürekli teslim seçeneğini kullanarak Azure portalı ile kanarya dağıtımlarını "**sanal makinelerinize"** göre yapılandırabilirsiniz. İşte adım adım yol. 
1. Azure portalınızda oturum açın ve sanal bir makineye gidin 
2. Dağıtım grubuna birden çok VM eklemek için önceki bölümdeki 2-5 adımlarını izleyin. 
3. Kanarya dağıtımlarının bir parçası olacak VM'lere özel bir etiket ekleyin. Örneğin, "kanarya".
4. Boru hattı VM'ler için yapılandırıldıktan sonra, ardışık tıklatın, Azure DevOps kuruluşunu başlatın, ardışık hattı **düzenleyin** ve **geliştirme** aşamasına gidin. "Kanarya" filtresine etiket ekleyin. 
5. Başka bir dağıtım grubu aşaması ekleyin, dağıtım grubunda kalan VM'leri hedeflemek için aşamayı etiketlerle yapılandırın.  
6. İsteğe bağlı olarak, kanarya dağıtımlarını tanıtabilecek/reddedebilecek bir el ile doğrulama adımı yapılandırın. 
   ![AzDevOps_Canary](media/tutorial-devops-azure-pipelines-classic/azdevops-canary-deploy.png)

**Mavi-Yeşil,** aynı bekleme ortamına sahip olarak dağıtım kapalı kalma süresini azaltır. Ortamlardan biri her zaman canlı. Yeni bir sürüm için hazırlanırken, yeşil ortamda test son aşamasıyapmak. Yazılım yeşil ortamda çalıştığında, gelen tüm isteklerin yeşil ortama gitmesi için trafiği değiştirin - mavi ortam artık boşta.
Sürekli teslim seçeneğini kullanarak Azure portalından Mavi-Yeşil**dağıtımları**" sanal makinelerinize" göre yapılandırabilirsiniz. 

İşte adım adım yol. 

1. Azure portalınızda oturum açın ve Sanal Makine'ye gidin 
2. Dağıtım grubuna birden çok VM eklemek için **Rolling güncelleştirmeleri** bölümünün altındaki 2-5 adımlarını izleyin. Mavi-yeşil dağıtımların bir parçası olacak VM'lere özel bir etiket ekleyin. Örneğin, stand-by rolü için olan VM'ler için "mavi" veya "yeşil". 
3. Boru hattı VM'ler için yapılandırıldıktan sonra, ardışık tıklatın, Azure DevOps kuruluşunu başlatın, ardışık hattı **düzenleyin,** **geliştirme** aşamasına gidin. "Yeşil" filtresine etiket ekleyin. 
4. Aracısız bir aşama ekleyin, fazı manuel doğrulama adımı yla ve etiketleri değiştirmek için invoke-REST api adımıyla yapılandırın. 
   ![AzDevOps_BlueGreen](media/tutorial-devops-azure-pipelines-classic/azdevops-blue-green-deploy.png)
 
 
## <a name="azure-devops-project"></a>Azure DevOps projesi 
Azure ile her zamankinden daha kolay başlayın.
 
DevOps Projects ile uygulamanızı herhangi bir Azure hizmetinde yalnızca üç adımda çalıştırmaya başlayın: uygulama dili, çalışma zamanı ve Azure hizmeti seçin.
 
[Daha fazla bilgi edinin](https://azure.microsoft.com/features/devops-projects/ ).
 
## <a name="additional-resources"></a>Ek kaynaklar 
- [DevOps projesini kullanarak Azure Sanal Makinelere dağıtın](https://docs.microsoft.com/azure/devops-project/azure-devops-project-vms)
- [Uygulamanızın Azure Sanal Makine Ölçeği Setine sürekli dağıtımını uygulayın](https://docs.microsoft.com/azure/devops/pipelines/apps/cd/azure/deploy-azure-scaleset)
