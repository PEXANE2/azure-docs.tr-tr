---
title: Öğretici-Azure Linux Sanal Makineleri için sıralı dağıtımları yapılandırma
description: Bu öğreticide, sıralı dağıtım stratejisini kullanarak bir Azure Linux Sanal Makineleri grubunu artımlı olarak güncelleştiren sürekli dağıtım (CD) işlem hattı ayarlamayı öğreneceksiniz.
author: moala
manager: jpconnock
tags: azure-devops-pipelines
ms.assetid: ''
ms.service: virtual-machines-linux
ms.topic: tutorial
ms.tgt_pltfrm: azure-pipelines
ms.workload: infrastructure
ms.date: 4/10/2020
ms.author: moala
ms.custom: devops
ms.openlocfilehash: 75888b1ebbda33891296fe0b54c5d204955e32a3
ms.sourcegitcommit: f7d057377d2b1b8ee698579af151bcc0884b32b4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/24/2020
ms.locfileid: "82113501"
---
# <a name="tutorial---configure-rolling-deployment-strategy-for-azure-linux-virtual-machines"></a>Öğretici-Azure Linux Sanal Makineleri için sıralı dağıtım stratejisini yapılandırma

Azure DevOps, her türlü Azure kaynağı için sürekli tümleştirme ve sürekli dağıtım ile DevOps işleminin her bir bölümünü otomatikleştiren yerleşik bir Azure hizmetidir.
Uygulamanızda sanal makineler, Web Apps, Kubernetes veya başka herhangi bir kaynak kullanılıyorsa, Azure ve Azure DevOps ile kod olarak altyapı, sürekli tümleştirme, sürekli test, sürekli teslim ve sürekli izleme uygulayabilirsiniz.  

![AzDevOps_portalView](media/tutorial-devops-azure-pipelines-classic/azdevops-view.png) 


## <a name="iaas---configure-cicd"></a>IaaS-CI/CD 'yi yapılandırma 
Azure Pipelines, sanal makinelere dağıtımlar için tam ve tam özellikli bir CI/CD otomasyon araçları kümesi sağlar. Azure VM için bir sürekli teslim işlem hattını doğrudan Azure portal yapılandırabilirsiniz. Bu belge, Azure portal 'ten çok makineli dağıtımlar için bir CI/CD işlem hattı ayarlamayla ilişkili adımları içerir. Ayrıca, Azure Portal ' den kullanıma hazır olarak desteklenen [kanarya](https://aka.ms/AA7jdrz) ve [mavi yeşil](https://aka.ms/AA83fwu)gibi diğer stratejileri de göz atabilirsiniz. 


**Sanal makinelerde CI/CD 'yi yapılandırma**

Sanal makineler, bir [dağıtım grubuna](https://docs.microsoft.com/azure/devops/pipelines/release/deployment-groups) hedef olarak eklenebilir ve çok makineli güncelleştirme için hedeflenebilir. Dağıtım grubu içindeki **dağıtım geçmişi** , DAĞıTıLDıKTAN sonra VM 'den işlem hattına ve sonra işlemeye izlenebilirlik sağlar. 
 

Sıralı **dağıtımlar**: sıralı dağıtım, bir uygulamanın önceki sürümünün örneklerini, her yinelemede sabit bir makine kümesindeki (sıralı olarak ayarlanan) uygulamanın yeni sürümünün örneklerine koyar. Sanal makinelere sıralı güncelleştirme yapılandırma hakkında yol açalım.  
Sürekli teslim seçeneğini kullanarak Azure portal içindeki "**sanal makinelerinize**" yönelik güncelleştirmeleri yapılandırabilirsiniz. 

İşte adım adım yönergeler. 
1. Azure portal oturum açın ve bir sanal makineye gidin. 
2. VM sol bölmesinde, **sürekli teslim** menüsüne gidin. Ardından **Yapılandır**' a tıklayın. 

   ![AzDevOps_configure](media/tutorial-devops-azure-pipelines-classic/azure-devops-configure.png) 
3. Yapılandırma panelinde, mevcut bir hesabı seçmek veya bir hesap oluşturmak için "Azure DevOps Organization" düğmesine tıklayın. Ardından, ardışık düzeni yapılandırmak istediğiniz projeyi seçin.  


   ![AzDevOps_project](media/tutorial-devops-azure-pipelines-classic/azure-devops-rolling.png) 
4. Dağıtım grubu, fiziksel ortamları temsil eden bir dağıtım hedefi makineleri kümesidir; Örneğin, "dev", "test", "UAT" ve "üretim". Yeni bir dağıtım grubu oluşturabilir veya mevcut bir dağıtım grubunu seçebilirsiniz. 
5. Sanal makineye dağıtılacak paketi yayımlayan derleme işlem hattını seçin. Yayımlanan paketin, paket kökündeki `deployscripts` klasöründe _Deploy. ps1_ veya _Deploy.sh_ dağıtım betiği olması gerektiğini unutmayın. Bu dağıtım betiği, çalışma zamanında Azure DevOps işlem hattı tarafından çalıştırılacak.
6. Tercih ettiğiniz dağıtım stratejisini seçin. Bu durumda, ' kayan ' seçeneğini belirlemenizi sağlar.
7. İsteğe bağlı olarak, makineyi rolüyle etiketleyebilir. Örneğin, ' Web ', ' DB ' vb. Bu, yalnızca belirli bir rolü olan VM 'Leri hedeflemenize yardımcı olur.
8. Sürekli teslim işlem hattını yapılandırmak için iletişim kutusunda **Tamam** ' a tıklayın. 
9. İşiniz bittiğinde, sanal makineye dağıtmak için yapılandırılmış bir sürekli teslim işlem hattı olacaktır.  


   ![AzDevOps_pipeline](media/tutorial-devops-azure-pipelines-classic/azure-devops-deployment-history.png)
10. Sanal makine dağıtımının devam ettiğini görürsünüz. İşlem hattına gitmek için bağlantıya tıklayabilirsiniz. Dağıtımı görüntülemek için **Release-1** ' e tıklayın. Ya da yayın işlem hattı tanımını değiştirmek için **Düzenle** ' ye tıklayabilirsiniz. 
11. Yapılandırılacak birden fazla sanal makine varsa, dağıtım grubuna eklenecek diğer VM 'Ler için 2-4 adımlarını yineleyin. Bir işlem hattı çalıştırmasının zaten bulunduğu bir dağıtım grubu seçerseniz, sanal makinenin yeni işlem hattı oluşturmadan önce dağıtım grubuna ekleneceğini unutmayın. 
12. İşiniz bittiğinde, işlem hattı tanımına tıklayın, Azure DevOps kuruluşuna gidin ve yayın işlem hattını **Düzenle** ' ye tıklayın. 
   ![AzDevOps_edit_pipeline](media/tutorial-devops-azure-pipelines-classic/azure-devops-rolling-pipeline.png)
13. Bağlantı 1 işi ' ne tıklayın, **geliştirme** aşamasında **1 görev** . **Dağıt** aşamasına tıklayın.
   ![AzDevOps_deploymentGroup](media/tutorial-devops-azure-pipelines-classic/azure-devops-rolling-pipeline-tasks.png)
14. Sağdaki yapılandırma bölmesinden, her yinelemede paralel olarak dağıtmak istediğiniz makine sayısını belirtebilirsiniz. Aynı anda birden çok makineye dağıtmak istiyorsanız kaydırıcıyı kullanarak bu değeri yüzde cinsinden belirtebilirsiniz.  

15. Dağıtımı betiği Yürüt görevi varsayılan olarak, yayımlanan paketin kök dizinindeki ' deployscripts ' klasöründe dağıtım betiği _Deploy. ps1_ veya _Deploy.sh_ ' i yürütür.  
![AzDevOps_publish_package](media/tutorial-deployment-strategy/package.png)

## <a name="other-deployment-strategies"></a>Diğer dağıtım stratejileri

- [Canary dağıtım stratejisini yapılandırma](https://aka.ms/AA7jdrz)
- [Mavi-yeşil dağıtım stratejisini yapılandırma](https://aka.ms/AA83fwu)

 
## <a name="azure-devops-project"></a>Azure DevOps projesi 
Azure 'ı her zamankinden daha kolay bir şekilde kullanmaya başlayın.
 
DevOps Projeleri, uygulamanızı yalnızca üç adımda herhangi bir Azure hizmetinde çalıştırmaya başlayın: bir uygulama dili, çalışma zamanı ve bir Azure hizmeti seçin.
 
[Daha fazla bilgi edinin](https://azure.microsoft.com/features/devops-projects/ ).
 
## <a name="additional-resources"></a>Ek kaynaklar 
- [DevOps projesini kullanarak Azure sanal makinelerine dağıtma](https://docs.microsoft.com/azure/devops-project/azure-devops-project-vms)
- [Azure sanal makine ölçek kümesine uygulamanızın sürekli dağıtımını uygulama](https://docs.microsoft.com/azure/devops/pipelines/apps/cd/azure/deploy-azure-scaleset)
