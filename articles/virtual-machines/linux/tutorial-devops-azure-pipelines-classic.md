---
title: Azure 'da IaaS ve PaaS için öğretici ile tümleşik DevOps
description: Bu öğreticide, Azure Pipelines kullanarak Azure VM 'lerine bir uygulamanın sürekli tümleştirme (CI) ve sürekli dağıtımı (CD) ayarlamayı öğreneceksiniz.
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
ms.sourcegitcommit: 3c925b84b5144f3be0a9cd3256d0886df9fa9dc0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/28/2020
ms.locfileid: "77912537"
---
# <a name="tutorial-integrated-devops-for-iaas-and-paas-on-azure"></a>Öğretici: Azure 'da IaaS ve PaaS için tümleşik DevOps

Azure 'da uçtan uca çözümler sayesinde takımlar her uygulama yaşam döngüsü aşamasından DevOps uygulamalarını uygulayabilir: plan, geliştirme, teslim etme ve çalıştırma. 

Aşağıda, bulut iş yüklerini basitleştiren Azure hizmetlerinden bazıları ve başaramayabiliriz güçlü senaryolar sağlamak için birleştirilebilecek.
Bu teknolojiler, kişilerle ve süreçlerle birlikte, ekiplerin müşterilere sürekli olarak değer sağlamasına imkan tanır. 

- Azure: https://portal.azure.com-bulut iş yükleri oluşturmaya yönelik portal. Basit Web uygulamalarından karmaşık bulut uygulamalarına kadar her şeyi yönetin ve izleyin 
- Azure DevOps: https://dev.azure.com-daha akıllı planlayın, daha iyi işbirliği yapın ve modern bir geliştirme hizmeti kümesiyle daha hızlı gönderin 
- Azure Machine Learning Studio: https://ml.azure.com-veri hazırlama, eğitim ve makine öğrenimi modellerini dağıtma 
 

Azure DevOps, her türlü Azure kaynağı için sürekli tümleştirme ve sürekli dağıtım ile DevOps işleminin her bir bölümünü otomatikleştiren yerleşik bir Azure hizmetidir.
Uygulamanızda sanal makineler, Web Apps, Kubernetes veya başka herhangi bir kaynak kullanılıyorsa, kod olarak altyapı, sürekli tümleştirme, sürekli test, sürekli teslim ve Azure ile Azure DevOps ile sürekli izleme uygulayabilirsiniz.  
![AzDevOps_portalView](media/tutorial-devops-azure-pipelines-classic/azdevops-view.png) 
 
 
## <a name="iaas---configure-cicd"></a>IaaS-CI/CD 'yi yapılandırma 
Azure Pipelines, sanal makinelere dağıtımlar için tam ve tam özellikli bir CI/CD otomasyon araçları kümesi sağlar. Bir Azure VM için Azure portal doğrudan bir sürekli teslim işlem hattı yapılandırabilirsiniz. Bu belge, Azure portal 'ten çok makineli dağıtımlar için bir CI/CD işlem hattı ayarlamayla ilişkili adımları içerir. Sanal makinelerde CI/CD 'yi yapılandırın.

Sanal makineler, bir [dağıtım grubuna](https://docs.microsoft.com/azure/devops/pipelines/release/deployment-groups) hedef olarak eklenebilir ve çok makineli güncelleştirmeler için hedeflenebilir. Dağıtım grupları içindeki dağıtım geçmişi görünümleri VM 'den işlem hattına ve sonra işlemeye izlenebilirlik sağlar. 
 
**Güncelleştirmeler alınıyor**: sıralı dağıtım, bir uygulamanın önceki sürümünün örneklerini, her yinelemede sabit bir makine kümesindeki (sıralı olarak ayarlanan) uygulamanın yeni sürümünün örneklerine koyar. Sanal makinelere sıralı güncelleştirme yapılandırma hakkında yol açalım.  
Sürekli teslim seçeneğini kullanarak Azure portal içindeki "**sanal makinelerinize**" yönelik güncelleştirmeleri yapılandırabilirsiniz. 

İşte adım adım yönergeler. 
1. Azure portal oturum açın ve bir sanal makineye gidin. 
2. VM sol bölmesinde, **sürekli teslim** menüsüne gidin. Ardından **Yapılandır**' a tıklayın. 
   ![AzDevOps_configure](media/tutorial-devops-azure-pipelines-classic/azdevops-configure.png) 
3. Yapılandırma panelinde, mevcut bir hesabı seçmek veya bir hesap oluşturmak için "Azure DevOps Organization" düğmesine tıklayın. Ardından, ardışık düzeni yapılandırmak istediğiniz projeyi seçin.  
   ![AzDevOps_project](media/tutorial-devops-azure-pipelines-classic/azdevops-project.png) 
4. Dağıtım grubu, fiziksel ortamları temsil eden bir dağıtım hedefi makineleri kümesidir; Örneğin, "dev", "test", "UAT" ve "üretim". Yeni bir dağıtım grubu oluşturabilir veya mevcut bir dağıtım grubunu seçebilirsiniz. Makineyi isteğe bağlı olarak rolüyle etiketleyebilir. Örneğin, ' Web ', ' DB ' vb.  
5. Sürekli teslim işlem hattını yapılandırmak için iletişim kutusunda **Tamam** ' a tıklayın. 
6. İşiniz bittiğinde, sanal makineye dağıtmak için yapılandırılmış bir sürekli teslim işlem hattı olacaktır.  
   ![AzDevOps_pipeline](media/tutorial-devops-azure-pipelines-classic/azdevops-pipeline.png)
7. Sanal makine dağıtımının devam ettiğini görürsünüz. İşlem hattına gitmek için bağlantıya tıklayabilirsiniz. Dağıtımı görüntülemek için **Release-1** ' e tıklayın. Ya da yayın işlem hattı tanımını değiştirmek için **Düzenle** ' ye tıklayabilirsiniz. 
8. Yapılandırılmış birden fazla sanal makine varsa, diğer VM 'Lerin dağıtım grubuna eklenmesi için 2-5 arasındaki adımları yineleyin. 
9. İşiniz bittiğinde, işlem hattı tanımına tıklayın, Azure DevOps kuruluşuna gidin ve yayın işlem hattını **Düzenle** ' ye tıklayın. 
   ![AzDevOps_edit_pipeline](media/tutorial-devops-azure-pipelines-classic/azdevops-edit-pipeline.png)
10. Bağlantı 1 işi ' ne tıklayın, **geliştirme** aşamasında **1 görev** . **Dağıt** aşamasına tıklayın.  
   ![AzDevOps_deploymentGroup](media/tutorial-devops-azure-pipelines-classic/azdevops-deployment-group.png)
11. Sağdaki yapılandırma bölmesinden, işlem hattının, paralel olarak tüm hedeflere yönelik bir sıralı güncelleştirme yapmak üzere yapılandırıldığını görebilirsiniz. Dağıtımları, kaydırıcıyı kullanarak bir seferde ya da yüzde bakımından gerçekleşecek şekilde yapılandırabilirsiniz.  
  
  
**Canary** , küçük bir kullanıcı alt kümesine yapılan değişikliği yavaşça ortadan kaldırarak riski azaltır. Yeni sürümde daha fazla güven elde ettiğiniz için, bunu altyapınızdaki daha fazla sunucuya serbest bırakarak daha fazla kullanıcıyı daha fazla kullanıcıya yönlendirmeye başlayabilirsiniz. Sürekli teslim seçeneğini kullanarak Azure Portal "**sanal makineleriniz**" için kanarya dağıtımlarını yapılandırabilirsiniz. İşte adım adım yönergeler. 
1. Azure portal oturum açın ve bir sanal makineye gidin 
2. Dağıtım grubuna birden çok VM eklemek için önceki bölümde 2-5 adımlarını izleyin. 
3. Sanal makinelere, Kanarya dağıtımlarının parçası olacak özel bir etiket ekleyin. Örneğin, "Canary".
4. İşlem hattı VM 'Ler için yapılandırıldıktan sonra, işlem hattına tıklayın, Azure DevOps organizasyonunu başlatın, işlem hattını **düzenleyin** ve **geliştirme** aşamasına gidin. "Canary" filtresine etiket ekleyin. 
5. Başka bir dağıtım grubu aşaması ekleyin, bu aşamayı dağıtım grubundaki kalan VM 'Leri hedeflemek için etiketlerle birlikte yapılandırın.  
6. İsteğe bağlı olarak, Kanarya dağıtımlarını yükseltebilecek/reddedebilecek bir el ile doğrulama adımı yapılandırın. 
   ![AzDevOps_Canary](media/tutorial-devops-azure-pipelines-classic/azdevops-canary-deploy.png)

**Mavi-yeşil** , aynı bekleme ortamına sahip olacak şekilde dağıtım kesinti süresini azaltır. Herhangi bir zamanda ortamlarından biri canlı olur. Yeni bir sürüm hazırlandıktan sonra, yeşil ortamda test ettiğiniz nihai aşamayı siz olursunuz. Yazılım yeşil ortamda çalışmaya başladıktan sonra gelen tüm isteklerin yeşil ortama gitmesi için trafiği değiştirin; mavi ortam artık boşta olur.
Sürekli teslim seçeneğini kullanarak Azure portal "**sanal makineleriniz**" için mavi-yeşil dağıtımları yapılandırabilirsiniz. 

İşte adım adım yönergeler. 

1. Azure portal oturum açın ve bir sanal makineye gidin 
2. Dağıtım grubuna birden çok VM eklemek için, sıralı **güncelleştirmeler** bölümündeki 2-5 adımlarını izleyin. VM 'lere, mavi yeşil dağıtımların parçası olacak özel bir etiket ekleyin. Örneğin, tek başına rol için olan VM 'Ler için "mavi" veya "yeşil". 
3. İşlem hattı VM 'Ler için yapılandırıldıktan sonra, işlem hattına tıklayın, Azure DevOps organizasyonunu başlatın, işlem hattını **düzenleyin** , **geliştirme** aşamasına gidin. "Yeşil" filtreye etiket ekleyin. 
4. Aracısız bir aşama ekleyin, aşamayı el ile doğrulama adımı ile yapılandırın ve etiketleri takas etmek için bir Invoke-REST API 'si adımını kullanın. 
   ![AzDevOps_BlueGreen](media/tutorial-devops-azure-pipelines-classic/azdevops-blue-green-deploy.png)
 
 
## <a name="azure-devops-project"></a>Azure DevOps projesi 
Azure 'ı her zamankinden daha kolay bir şekilde kullanmaya başlayın.
 
DevOps Projeleri, uygulamanızı yalnızca üç adımda herhangi bir Azure hizmetinde çalıştırmaya başlayın: bir uygulama dili, çalışma zamanı ve bir Azure hizmeti seçin.
 
[Daha fazla bilgi edinin](https://azure.microsoft.com/features/devops-projects/ ).
 
## <a name="additional-resources"></a>Ek kaynaklar 
- [DevOps projesini kullanarak Azure sanal makinelerine dağıtma](https://docs.microsoft.com/azure/devops-project/azure-devops-project-vms)
- [Azure sanal makine ölçek kümesine uygulamanızın sürekli dağıtımını uygulama](https://docs.microsoft.com/azure/devops/pipelines/apps/cd/azure/deploy-azure-scaleset)
