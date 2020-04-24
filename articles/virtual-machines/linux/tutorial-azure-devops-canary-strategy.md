---
title: Öğretici-Azure Linux sanal makineleri için kanarya dağıtımlarını yapılandırma
description: Bu öğreticide, bir Azure Linux sanal makineleri grubunu kanarya dağıtım stratejisi kullanarak güncelleştiren sürekli dağıtım (CD) işlem hattı ayarlamayı öğreneceksiniz.
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
ms.openlocfilehash: b51b4aed85f737e436565ce8ba1ab4a295714734
ms.sourcegitcommit: f7d057377d2b1b8ee698579af151bcc0884b32b4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/24/2020
ms.locfileid: "82120481"
---
# <a name="tutorial---configure-canary-deployment-strategy-for-azure-linux-virtual-machines"></a>Öğretici-Azure Linux Sanal Makineleri için Canary dağıtım stratejisini yapılandırma


## <a name="iaas---configure-cicd"></a>IaaS-CI/CD 'yi yapılandırma 
Azure Pipelines, sanal makinelere dağıtımlar için tam ve tam özellikli bir CI/CD otomasyon araçları kümesi sağlar. Bir Azure VM için Azure portal doğrudan bir sürekli teslim işlem hattı yapılandırabilirsiniz. Bu belge, çok makineli dağıtımlar için Canary stratejisini kullanan bir CI/CD işlem hattı ayarlama ile ilgili adımları içerir. Ayrıca, [Azure Portal ' den](https://aka.ms/AA7jlh8) kullanıma hazır olarak desteklenen, sıralı ve [mavi yeşil](https://aka.ms/AA83fwu)gibi diğer stratejileri de göz atabilirsiniz. 


**Sanal makinelerde CI/CD 'yi yapılandırma**

Sanal makineler, bir [dağıtım grubuna](https://docs.microsoft.com/azure/devops/pipelines/release/deployment-groups) hedef olarak eklenebilir ve çok makineli güncelleştirmeler için hedeflenebilir. Dağıtım grubunuzda **dağıtım geçmişi** , DAĞıTıLDıKTAN sonra VM 'den işlem hattına ve sonra işlemeye izlenebilirlik sağlar. 
 
  
**Canarlı dağıtımlar**: bir kanarya dağıtımı, küçük bir kullanıcı alt kümesiyle değişikliği ortadan kaldırarak riski azaltır. Yeni sürümde daha fazla güven elde ettiğiniz için, bunu altyapınızdaki daha fazla sunucuya serbest bırakarak daha fazla kullanıcıyı daha fazla kullanıcıya yönlendirmeye başlayabilirsiniz. Sürekli teslim seçeneğini kullanarak Azure Portal sanal makinelerinize yönelik kanarya dağıtımlarını yapılandırmak için kullanabilirsiniz. İşte adım adım yönergeler. 
1. Azure portal oturum açın ve bir sanal makineye gidin 
2. VM sol bölmesinde, **sürekli teslim** menüsüne gidin. **Yapılandır**'a tıklayın. 

   ![AzDevOps_configure](media/tutorial-devops-azure-pipelines-classic/azure-devops-configure.png) 
3. Yapılandırma panelinde, mevcut bir hesabı seçmek veya bir hesap oluşturmak için **Azure DevOps organizasyonu** ' na tıklayın. Ardından, ardışık düzeni yapılandırmak istediğiniz projeyi seçin.  


   ![AzDevOps_project](media/tutorial-devops-azure-pipelines-classic/azure-devops-rolling.png) 
4. Dağıtım grubu, fiziksel ortamları temsil eden bir dağıtım hedefi makineleri kümesidir; Örneğin, "dev", "test", "UAT" ve "üretim". Yeni bir dağıtım grubu oluşturabilir veya mevcut bir dağıtım grubunu seçebilirsiniz. 
5. Sanal makineye dağıtılacak paketi yayımlayan derleme işlem hattını seçin. Yayımlanan paketin, paket kökündeki `deployscripts` klasöründe _Deploy. ps1_ veya _Deploy.sh_ dağıtım betiği olması gerektiğini unutmayın. Bu dağıtım betiği, çalışma zamanında Azure DevOps işlem hattı tarafından yürütülür.
6. Tercih ettiğiniz dağıtım stratejisini seçin. **Canary**' ı seçin.
7. Kanarya dağıtımlarının parçası olacak VM 'lere ' kanarya ' etiketi ve kanarya dağıtımı başarılı olduktan sonra dağıtımların parçası olan sanal makinelere bir ' üretim ' etiketi ekleyin. Etiketler yalnızca belirli bir rolü olan VM 'Leri hedefetmenize yardımcı olur.
![AzDevOps_configure_canary](media/tutorial-devops-azure-pipelines-classic/azure-devops-configure-canary.png)

8. Sürekli teslim işlem hattını yapılandırmak için iletişim kutusunda **Tamam** ' a tıklayın. Artık sanal makineye dağıtmak için yapılandırılmış bir sürekli teslim işlem hattına sahipsiniz.
![AzDevOps_canary_pipeline](media/tutorial-devops-azure-pipelines-classic/azure-devops-canary-pipeline.png)


9. İşlem hattı yapılandırmasını görmek için Azure DevOps 'daki yayın işlem hattını **Düzenle** ' ye tıklayın. İşlem hattı üç aşamadan oluşur. İlk aşama bir dağıtım grubu aşamasıdır ve _kanarya_olarak etiketlenen VM 'lere dağıtır. İkinci aşamada işlem hattı duraklatılır ve el ile müdahale çalışmasının çalışmayı sürdürmesini bekler. Bir Kullanıcı, kanarlı dağıtımın kararlı hale getirildiği bir işlem hattı çalıştırmasını sürdürtikten sonra _üretim akışı_olarak etiketlenen VM 'lere dağıtan Üçüncü aşamayı çalıştırır. ![AzDevOps_canary_task](media/tutorial-devops-azure-pipelines-classic/azure-devops-canary-task.png)

10. İşlem hattı çalıştırmasını sürdürmeden önce, en az bir VM 'nin _Üretim_olarak etiketlendiğinden emin olun. İşlem hattının üçüncü aşamasında, uygulama yalnızca _Üretim_ etiketi olan sanal makinelere dağıtılır.

11. Dağıtımı betiği Yürüt görevi varsayılan olarak, yayımlanan paketin kök dizinindeki ' deployscripts ' klasöründe dağıtım betiği _Deploy. ps1_ veya _Deploy.sh_ ' i yürütür. Seçili derleme ardışık düzeninin bunu paketin kök klasöründe yayımladığınızdan emin olun. 
![AzDevOps_publish_package](media/tutorial-deployment-strategy/package.png)




## <a name="other-deployment-strategies"></a>Diğer dağıtım stratejileri
- [Sıralı dağıtım stratejisini yapılandırma](https://aka.ms/AA7jlh8)
- [Mavi-yeşil dağıtım stratejisini yapılandırma](https://aka.ms/AA83fwu)

## <a name="azure-devops-project"></a>Azure DevOps projesi 
Azure 'ı her zamankinden daha kolay bir şekilde kullanmaya başlayın.
 
DevOps Projeleri, uygulamanızı yalnızca üç adımda herhangi bir Azure hizmetinde çalıştırmaya başlayın: bir uygulama dili, çalışma zamanı ve bir Azure hizmeti seçin.
 
[Daha fazla bilgi edinin](https://azure.microsoft.com/features/devops-projects/ ).
 
## <a name="additional-resources"></a>Ek kaynaklar 
- [DevOps projesini kullanarak Azure sanal makinelerine dağıtma](https://docs.microsoft.com/azure/devops-project/azure-devops-project-vms)
- [Azure sanal makine ölçek kümesine uygulamanızın sürekli dağıtımını uygulama](https://docs.microsoft.com/azure/devops/pipelines/apps/cd/azure/deploy-azure-scaleset)
