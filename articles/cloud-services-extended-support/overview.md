---
title: Azure Cloud Services hakkında (genişletilmiş destek)
description: Sanal ağ ve DNS değerlerini belirten hizmet yapılandırma dosyasının ağ yapılandırması öğesinin alt öğeleri hakkında bilgi edinin.
ms.topic: article
ms.service: cloud-services-extended-support
author: gachandw
ms.author: gachandw
ms.reviewer: mimckitt
ms.date: 10/13/2020
ms.custom: ''
ms.openlocfilehash: cf7c4b881697b664403d8c817c3b9e48fb48944d
ms.sourcegitcommit: 4d48a54d0a3f772c01171719a9b80ee9c41c0c5d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2021
ms.locfileid: "98746773"
---
# <a name="about-azure-cloud-services-extended-support"></a>Azure Cloud Services hakkında (genişletilmiş destek)

> [!IMPORTANT]
> Cloud Services (genişletilmiş destek) Şu anda genel önizleme aşamasındadır.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Cloud Services (genişletilmiş destek), [Azure Cloud Services](https://azure.microsoft.com/services/cloud-services/) ürüne yönelik yeni bir [Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/management/overview) tabanlı dağıtım modelidir ve şu anda genel önizlemededir. Cloud Services (genişletilmiş destek), Azure Service Manager kullanılarak dağıtılan Azure Cloud Services ile özellik eşliği ile birlikte bölge esnekliği sağlamanın birincil avantajına sahiptir. Ayrıca rol tabanlı erişim ve denetim (RBAC), Etiketler, ilke ve dağıtım şablonlarını destekler gibi bazı ARM özellikleri de sağlar.  

Bu değişiklik ile, Cloud Services için Azure Service Manager tabanlı dağıtım modeli [Cloud Services (klasik)](../cloud-services/cloud-services-choose-me.md)olarak yeniden adlandırılacaktır. Web ve bulut uygulamalarınızı ve hizmetlerinizi oluşturma ve hızla dağıtma imkanını koruyabilirsiniz. Bulut hizmetleri altyapınızı geçerli talebe göre ölçeklendirebilir ve uygulamalarınızın performansının aynı anda maliyetleri azaltırken devam edebilmesini sağlayabilirsiniz.  

## <a name="what-does-not-change"></a>Ne değiştirmiyor 
- Kodu oluşturur, konfigürasyonları tanımlayabilir ve Azure 'a dağıtırsınız. Azure, işlem ortamını ayarlar, kodunuzu çalıştırır ve sizin için saklar.
- Cloud Services (genişletilmiş destek) iki tür rol, [Web ve çalışan](../cloud-services/cloud-services-choose-me.md)da destekler. 
- Bulut hizmetinin üç bileşeni, hizmet tanımı (. csdef), hizmet yapılandırması (. cscfg) ve bir hizmet paketi (. cspkg) ileri taşınır ve [biçimlerinde](cloud-services-model-and-package.md)hiçbir değişiklik yapılmaz. 
- Veri düzlemi aynı olduğundan ve denetim düzlemi yalnızca değişmekte olduğundan çalışma zamanı kodu için herhangi bir değişiklik yapılması gerekmez.  

## <a name="changes-in-deployment-model"></a>Dağıtım modelindeki değişiklikler

Cloud Services dağıtmak için hizmet yapılandırma (. cscfg) ve hizmet tanımı (. csdef) dosyaları için en az değişiklikler gerekir (genişletilmiş destek). Çalışma zamanı kodu için herhangi bir değişiklik yapılması gerekmez. Ancak, yeni Azure Resource Manager tabanlı API 'Leri çağırmak için dağıtım betikleri 'nin güncellenmesi gerekir. 

:::image type="content" source="media/overview-image-1.png" alt-text="Görüntü, şablon bölümünün eklenmesiyle klasik bulut hizmeti yapılandırmasını gösterir. ":::

Dağıtıma göre Cloud Services (klasik) ve Cloud Services (genişletilmiş destek) arasındaki önemli farklılıklar şunlardır: 

- Azure Resource Manager dağıtımları, projeniz için altyapıyı ve yapılandırmayı tanımlayan bir JavaScript Nesne Gösterimi (JSON) dosyası olan [ARM şablonları](https://docs.microsoft.com/azure/azure-resource-manager/templates/overview) kullanır. Şablon, dağıtmak istediğiniz öğeyi oluşturmaya yönelik programlama komutları dizisini yazmak zorunda kalmadan bu öğeyi belirtmenize imkan tanıyan bildirim temelli söz dizimini kullanır. Cloud Services (genişletilmiş destek) dağıtımı sırasında hizmet yapılandırma ve hizmet tanımı dosyasının [ARM şablonuyla](https://docs.microsoft.com/azure/azure-resource-manager/templates/overview) tutarlı olması gerekir. Bu, [ARM şablonunu el ile oluşturarak](deploy-template.md) ya da [PowerShell](deploy-powershell.md), [Portal](deploy-portal.md) ve [Visual Studio](deploy-visual-studio.md)kullanarak elde edilebilir.  

- Müşteriler [, Cloud Services sertifikaları yönetmek için Azure Key Vault kullanmalıdır (genişletilmiş destek)](certificates-and-key-vault.md). [](https://docs.microsoft.com/azure/key-vault/general/overview) Azure Key Vault, merkezi ve güvenli bir bulut deposundaki gizli diziler, anahtarlar ve sertifikalar gibi uygulama kimlik bilgilerini güvenli bir şekilde depolamanıza ve yönetmenize olanak sağlar. Uygulamalarınız, kimlik bilgilerini almak için çalışma zamanında Key Vault kimlik doğrulaması yapabilir. 

- [Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/templates/overview) aracılığıyla dağıtılan tüm kaynaklar bir sanal ağ içinde olmalıdır. Sanal ağlar ve alt ağlar, mevcut Azure Resource Manager API 'Leri kullanılarak Azure Resource Manager oluşturulur ve Cloud Services dağıtımında. cscfg öğesinin NetworkConfiguration bölümünde başvurulmalıdır (genişletilmiş destek).   

- Her bulut hizmeti (genişletilmiş destek), tek bir bağımsız dağıtımdır. Cloud Services (genişletilmiş destek), tek bir bulut hizmeti içinde birden çok yuvası desteklemez.  
    - VIP takas <sup>*</sup> özelliği, iki bulut hizmeti arasında takas için kullanılabilir (genişletilmiş destek). Bir bulut hizmetinin yeni bir sürümünü test etmek ve hazırlamak için bir bulut hizmeti dağıtın (genişletilmiş destek) ve başka bir bulut hizmeti ile VIP swapolarak etiketleyin (genişletilmiş destek)  

- Etki alanı adı hizmeti (DNS) etiketi bir bulut hizmeti (genişletilmiş destek) için isteğe bağlıdır. Azure Resource Manager, DNS etiketi, bulut hizmetiyle ilişkili genel IP kaynağının bir özelliğidir. 


<sup>*</sup> Cloud Services için VIP takas (genişletilmiş destek) genel önizleme sırasında kullanılamaz.  

## <a name="migration-to-azure-resource-manager"></a>Azure Resource Manager geçiş

Cloud Services (genişletilmiş destek), [Azure Service Manager](https://docs.microsoft.com/powershell/azure/servicemanagement/overview?view=azuresmps-4.0.0&preserve-view=true ) 'den [Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/management/overview)'e geçiş yapmanız için iki yol sağlar. 
1) Müşteriler Cloud Services 'ı doğrudan Azure Resource Manager ' de dağıtır ve ardından Azure Service Manager eski bulut hizmetini siler. 
2) Yerinde geçiş Cloud Services (klasik) Cloud Services (genişletilmiş destek) kesinti süresi olmadan en düşük düzeyde geçiş yeteneğini destekler. 

### <a name="additional-migration-options"></a>Ek geçiş seçenekleri

Cloud Services (klasik) 'den Cloud Services (genişletilmiş destek) ' den geçiş planlarını değerlendirirken, [Sanal Makine Ölçek Kümeleri](https://docs.microsoft.com/azure/virtual-machine-scale-sets/overview), [App Service](https://docs.microsoft.com/azure/app-service/overview), [Azure Kubernetes hizmeti](https://docs.microsoft.com/azure/aks/intro-kubernetes)ve [Azure Service Fabric](https://docs.microsoft.com/azure/service-fabric/service-fabric-overview)gibi ek Azure hizmetlerini araştırmak isteyebilirsiniz. Bu hizmetler ek özellikler özelliği olmaya devam eder, ancak Cloud Services (genişletilmiş destek) birincil olarak Cloud Services (klasik) özellik eşlik eder. 

Uygulamaya bağlı olarak, Cloud Services (genişletilmiş destek), diğer seçeneklere kıyasla Azure Resource Manager geçmek için önemli ölçüde daha az çaba gerektirebilir. Uygulamanız geliştirilmemişse, Cloud Services (genişletilmiş destek) bir hızlı geçiş yolu sağladığından göz önünde bulundurmanız gereken önemli bir seçenektir. Buna karşılık, uygulamanız sürekli olarak gelişiyor ve daha modern bir özellik kümesine ihtiyaç duyuyorsa, mevcut ve gelecekteki gereksinimlerinizi daha iyi karşılamak için diğer Azure hizmetlerini araştırın. 

## <a name="next-steps"></a>Sonraki adımlar
- Cloud Services için [dağıtım önkoşullarını](deploy-prerequisite.md) gözden geçirin (genişletilmiş destek).
- [Azure Portal](deploy-portal.md), [PowerShell](deploy-powershell.md), [şablon](deploy-template.md) veya [Visual Studio](deploy-visual-studio.md)kullanarak bir bulut hizmeti (genişletilmiş destek) dağıtın.
- Cloud Services için [sık sorulan soruları](faq.md) gözden geçirin (genişletilmiş destek).
