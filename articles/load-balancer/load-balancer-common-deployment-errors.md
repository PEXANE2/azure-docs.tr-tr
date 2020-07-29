---
title: Sık karşılaşılan dağıtım hatalarını giderme
titleSuffix: Azure Load Balancer
description: Azure yük dengeleyiciler dağıtırken yaygın hataların nasıl çözümleneceğini açıklar
services: load-balancer
documentationcenter: na
tags: top-support-issue
author: anavinahar
ms.service: load-balancer
ms.topic: troubleshooting
ms.date: 04/27/2020
ms.author: anavin
ms.openlocfilehash: 527f71b1980b5a62d3db94fe89a1bce98142e31a
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84221003"
---
# <a name="troubleshoot-common-azure-deployment-errors-with-azure-load-balancer"></a>Azure Load Balancer ile yaygın Azure dağıtım hatalarını giderme

Bu makalede bazı yaygın Azure Load Balancer dağıtım hataları açıklanmakta ve hatalar çözümlenme bilgileri sağlanmaktadır. Bir hata kodu hakkında bilgi arıyorsanız ve bu makalede bilgi sağlanmazsa bize bize izin verin. Bu sayfanın en altında geri bildirimde olabilirsiniz. Geri bildirim, GitHub sorunlarıyla izlenir.

## <a name="error-codes"></a>Hata kodları

| Hata kodu | Ayrıntılar ve risk azaltma |
| ------- | ---------- |
|DifferentSkuLoadBalancersAndPublicIPAddressNotAllowed| Hem genel IP SKU 'SU hem de Load Balancer SKU 'SU eşleşmelidir. Azure Load Balancer ve genel IP SKU 'Larının eşleştiğinden emin olun. Standart SKU, üretim iş yükleri için önerilir. [SKU 'lardaki farklılıklar](./skus.md) hakkında daha fazla bilgi edinin  |
|DifferentSkuLoadBalancerAndPublicIPAddressNotAllowedInVMSS | SKU belirtilmediğinde veya standart genel IP 'Ler olmadan dağıtıldığında, sanal makine ölçek kümeleri varsayılan olarak temel yük dengeleyiciler olarak ayarlanır. Standart Load Balancer seçili olduğundan emin olmak için tek örneklerde standart genel IP 'Ler ile sanal makine ölçek kümesini yeniden dağıtın veya Azure portal sanal makine ölçek kümesini dağıtırken yalnızca bir standart LB seçin. |
|Maxkullanılabilirliği Bilitysetsınloadbalancerulaşıldı | Bir Load Balancer arka uç havuzunda en fazla 150 kullanılabilirlik kümesi bulunabilir. Arka uç havuzundaki sanal makinelerinize açık olarak tanımlanmış kullanılabilirlik kümeleriniz yoksa, her bir VM kendi kullanılabilirlik kümesine gider. Bu nedenle, 150 tek başına VM 'Leri dağıtmak, 150 kullanılabilirlik kümesi olduğunu ve bu nedenle sınıra vurarak olduğunu kapsıyor. Bir kullanılabilirlik kümesi dağıtabilir ve bu çözüme geçici bir çözüm olarak ek VM 'Ler ekleyebilirsiniz. |
|Networkınterfaceandloadbalancerareınfarklıentavailabilitysets | Temel SKU yük dengeleyici için, ağ arabirimi ve yük dengeleyici aynı Kullanılabilirlik kümesinde olmalıdır. |
|Rulesofsameloadbalancertypeusesamebackendportprotocolandıpconfig| Aynı sanal makine ölçek kümesi tarafından başvurulan aynı arka uç bağlantı noktası ve protokolüne sahip belirli bir yük dengeleyici türünde (iç, genel) birden fazla kural olamaz. Bu yinelenen kural oluşturmayı değiştirmek için kuralınızı güncelleştirin. |
|Rulesofsameloadbalancertypeusesamebackendportprotocolandvmssıpconfig| Aynı sanal makine ölçek kümesi tarafından başvurulan aynı arka uç bağlantı noktası ve protokolüne sahip belirli bir yük dengeleyici türünde (iç, genel) birden fazla kural olamaz. Bu yinelenen kural oluşturmayı değiştirmek için kural parametrelerinizi güncelleştirin. |
|Anotherınternalloadbalancerexists| Load Balancer arka ucunda aynı VM/ağ arabirimi kümesine iç başvuru türünden yalnızca bir Load Balancer sahip olabilirsiniz. Aynı türde yalnızca bir Load Balancer oluşturduğunuzdan emin olmak için dağıtımınızı güncelleştirin. |
|Cannotuseinactivehealtharaştırması| Sanal makine ölçek kümesi sistem durumu için yapılandırılmış herhangi bir kural tarafından kullanılmayan bir araştırmanız yok. Ayarlanan araştırmanın etkin bir şekilde kullanıldığından emin olun. |
|VMScaleSetCannotUseMultipleLoadBalancersOfSameType| Aynı türde (iç, genel) birden fazla yük dengeleyicileri olamaz. En fazla bir adet iç ve bir ortak Load Balancer olabilir. |
|VMScaleSetCannotReferenceLoadbalancerWhenLargeScaleOrCrossAZ | Temel Load Balancer, birden çok yerleştirme grubu sanal makine ölçek kümeleri veya çapraz kullanılabilirlik Bölgesi sanal makine ölçek kümesi için desteklenmez. Bunun yerine Standart Load Balancer kullanın. |
|MarketplacePurchaseEligibilityFailed | Aboneliğin EA aboneliği olması nedeniyle satın alma işlemlerini etkinleştirmek için doğru yönetici hesabına geçin. [Buradan](https://docs.microsoft.com/azure/marketplace/marketplace-faq-publisher-guide#what-could-block-a-customer-from-completing-a-purchase) daha fazla bilgi edinebilirsiniz. |
|ResourceDeploymentFailure| Yük dengeleyiciniz başarısız durumdaysa, başarısız durumundan geri getirmek için şu adımları izleyin:<ol><li>Adresine gidin https://resources.azure.com ve Azure Portal kimlik bilgilerinizle oturum açın.</li><li>**Oku/yaz**seçeneğini belirleyin.</li><li>Sol tarafta, **abonelikler**' i genişletin ve ardından güncelleştirilecek Load Balancer aboneliği genişletin.</li><li>**ResourceGroups**' ı genişletin ve sonra güncelleştirmek için Load Balancer kaynak grubunu genişletin.</li><li>**Microsoft. Network**  >  **LoadBalancers**' ı seçin ve sonra güncelleştirmek Load Balancer **LoadBalancer_1**seçin.</li><li>**LoadBalancer_1**için görüntüleme sayfasında düzenleme **Al**' ı seçin  >  **Edit**.</li><li>**Provisioningstate** değeri **başarıyla** **güncelleştirilemedi.**</li><li>**PUT**'u seçin.</li></ol>|
|  |  |

## <a name="next-steps"></a>Sonraki adımlar

* Azure Load Balancer [SKU karşılaştırma tablosuna](./skus.md) bakın
* [Azure Load Balancer sınırları](https://docs.microsoft.com/azure/azure-resource-manager/management/azure-subscription-service-limits#load-balancer) hakkında bilgi edinin
