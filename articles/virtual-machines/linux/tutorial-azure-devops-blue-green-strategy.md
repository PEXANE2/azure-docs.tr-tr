---
title: Öğretici-Azure Linux sanal makineleri için kanarya dağıtımlarını yapılandırma
description: Bu öğreticide, mavi yeşil dağıtım stratejisini kullanarak bir Azure sanal makinesi grubunu güncelleştiren sürekli dağıtım (CD) işlem hattı ayarlamayı öğreneceksiniz.
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
ms.openlocfilehash: b1a57245434bb188ffaab56a8891b4b0ee27f044
ms.sourcegitcommit: f7d057377d2b1b8ee698579af151bcc0884b32b4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/24/2020
ms.locfileid: "82120495"
---
# <a name="tutorial---configure-blue-green-deployment-strategy-for-azure-linux-virtual-machines"></a>Öğretici-Azure Linux Sanal Makineleri için mavi yeşil dağıtım stratejisini yapılandırma


## <a name="iaas---configure-cicd"></a>IaaS-CI/CD 'yi yapılandırma 
Azure Pipelines, sanal makinelere dağıtımlar için tam ve tam özellikli bir CI/CD otomasyon araçları kümesi sağlar. Azure VM için bir sürekli teslim işlem hattını doğrudan Azure portal yapılandırabilirsiniz. Bu belge, çok makineli dağıtımlar için mavi-yeşil stratejiyi kullanan bir CI/CD işlem hattı ayarlama ile ilgili adımları içerir. Ayrıca, [Azure Portal ' den](https://aka.ms/AA7jlh8) kullanıma hazır olarak desteklenen, sıralı ve [kanarya](https://aka.ms/AA7jdrz)gibi diğer stratejilere de göz atabilirsiniz. 

 
 **Sanal makinelerde CI/CD 'yi yapılandırma**

Sanal makineler, bir [dağıtım grubuna](https://docs.microsoft.com/azure/devops/pipelines/release/deployment-groups) hedef olarak eklenebilir ve çok makineli güncelleştirmeler için hedeflenebilir. Dağıtım grubundaki **dağıtım geçmişi** , dağıtıldıktan sonra, VM 'den işlem hattına ve sonra işlemeye izlenebilirlik sağlar. 
 
  
**Mavi-yeşil dağıtımlar**: mavi yeşil bir dağıtım, aynı bekleme ortamına sahip olma süresini azaltır. Herhangi bir zamanda ortamlarından biri canlı olur. Yeni bir sürüm hazırlandıktan sonra, yeşil ortamda testin son aşamasını tamamlarsınız. Yazılım yeşil ortamda çalışmaya başladıktan sonra gelen tüm isteklerin yeşil ortama gitmesi için trafiği değiştirin; mavi ortam artık boşta olur.
Sürekli teslim seçeneğini kullanarak Azure portal "**sanal makineleriniz**" için mavi-yeşil dağıtımları yapılandırabilirsiniz. 

İşte adım adım yönergeler.

1. Azure portal oturum açın ve bir sanal makineye gidin 
2. VM sol bölmesinde **sürekli teslim**' ya gidin. Ardından **Yapılandır**' a tıklayın. 

   ![AzDevOps_configure](media/tutorial-devops-azure-pipelines-classic/azure-devops-configure.png) 
3. Yapılandırma panelinde, mevcut bir hesabı seçmek veya bir hesap oluşturmak için **Azure DevOps organizasyonu** ' na tıklayın. Ardından, ardışık düzeni yapılandırmak istediğiniz projeyi seçin.  


   ![AzDevOps_project](media/tutorial-devops-azure-pipelines-classic/azure-devops-rolling.png) 
4. Dağıtım grubu, fiziksel ortamları temsil eden bir dağıtım hedefi makineleri kümesidir; Örneğin, "dev", "test", "UAT" ve "üretim". Yeni bir dağıtım grubu oluşturabilir veya mevcut bir dağıtım grubunu seçebilirsiniz. 
5. Sanal makineye dağıtılacak paketi yayımlayan derleme işlem hattını seçin. Yayımlanan paketin, paket kökünde bir dağıtım betiği _Deploy. ps1_ veya _Deploy.sh_ `deployscripts` olması gerektiğini unutmayın. Bu dağıtım betiği, çalışma zamanında Azure DevOps işlem hattı tarafından yürütülür.
6. Tercih ettiğiniz dağıtım stratejisini seçin. **Mavi-yeşil**' i seçin.
7. Mavi-yeşil dağıtımların parçası olacak VM 'lere "mavi" veya "yeşil" bir etiket ekleyin. VM bir bekleme rolü için ise, bunu "yeşil" olarak etiketlemelisiniz, aksi takdirde "mavi" olarak etiketleyin.
![AzDevOps_bluegreen_configure](media/tutorial-devops-azure-pipelines-classic/azure-devops-blue-green-configure.png)

8. Sürekli teslim işlem hattını yapılandırmak için **Tamam** ' ı tıklatın. Artık sanal makineye dağıtmak için yapılandırılmış bir sürekli teslim işlem hattına sahipsiniz.
![AzDevOps_bluegreen_pipeline](media/tutorial-devops-azure-pipelines-classic/azure-devops-blue-green-pipeline.png)


9. İşlem hattı yapılandırmasını görmek için Azure DevOps 'daki yayın işlem hattını **Düzenle** ' ye tıklayın. İşlem hattı üç aşamadan oluşur. İlk aşama bir dağıtım grubu aşamasıdır ve _yeşil_ (bekleme) olarak etiketlenen VM 'lere dağıtılır. İkinci aşamada işlem hattı duraklatılır ve el ile müdahale çalışmasının çalışmayı sürdürmesini bekler. Bir Kullanıcı dağıtımın kararlı hale geldiğinde, trafiği artık _yeşil_ VM 'lere yönlendirebilir ve ardından VM 'lerde _mavi_ ve _yeşil_ etiketleri takas eden işlem hattı çalıştırmasını sürdürür. Bu, eski uygulama sürümü olan VM 'Lerin _yeşil_ olarak etiketlendiğinden ve sonraki işlem hattı çalıştırmasında ' de dağıtıldığından emin olmanızı sağlar.
![AzDevOps_bluegreen_task](media/tutorial-devops-azure-pipelines-classic/azure-devops-blue-green-tasks.png)


10. Dağıtımı betiği Yürüt görevi varsayılan olarak, yayımlanan paketin kök dizinindeki `deployscripts` klasöründe _Deploy. ps1_ veya _Deploy.sh_ dağıtım betiğini yürütür. Seçili derleme ardışık düzeninin bunu paketin kök klasöründe yayımladığınızdan emin olun.
![AzDevOps_publish_package](media/tutorial-deployment-strategy/package.png)




## <a name="other-deployment-strategies"></a>Diğer dağıtım stratejileri
- [Sıralı dağıtım stratejisini yapılandırma](https://aka.ms/AA7jlh8)
- [Canary dağıtım stratejisini yapılandırma](https://aka.ms/AA7jdrz)

## <a name="azure-devops-project"></a>Azure DevOps projesi 
Azure 'ı her zamankinden daha kolay bir şekilde kullanmaya başlayın.
 
DevOps Projeleri, uygulamanızı yalnızca üç adımda herhangi bir Azure hizmetinde çalıştırmaya başlayın: bir uygulama dili, çalışma zamanı ve bir Azure hizmeti seçin.
 
[Daha fazla bilgi edinin](https://azure.microsoft.com/features/devops-projects/ ).
 
## <a name="additional-resources"></a>Ek kaynaklar 
- [DevOps projesini kullanarak Azure sanal makinelerine dağıtma](https://docs.microsoft.com/azure/devops-project/azure-devops-project-vms)
- [Azure sanal makine ölçek kümesine uygulamanızın sürekli dağıtımını uygulama](https://docs.microsoft.com/azure/devops/pipelines/apps/cd/azure/deploy-azure-scaleset)
