---
title: Kaynakları & kotaları yönetme
titleSuffix: Azure Machine Learning
description: Azure Machine Learning kaynaklar üzerinde kotalar ve daha fazla kota isteme hakkında bilgi edinin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.reviewer: jmartens
author: nishankgu
ms.author: nigup
ms.date: 10/13/2020
ms.topic: conceptual
ms.custom: troubleshooting,contperfq4, contperfq2
ms.openlocfilehash: 93cf29651110356023752f2377c9a0fc358982a7
ms.sourcegitcommit: 957c916118f87ea3d67a60e1d72a30f48bad0db6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/19/2020
ms.locfileid: "92203066"
---
# <a name="manage--increase-quotas-for-resources-with-azure-machine-learning"></a>Azure Machine Learning sahip kaynaklara yönelik kotaları yönetme & artırma

Azure, dolandırıcılığın yanı sıra Azure kapasite kısıtlamalarını dikkate almak için sınırlamaları ve kotaları kullanır. Üretim iş yükleri için ölçeklendirirseniz bu limitleri değerlendirin. Bu makalede hakkında bilgi edinebilirsiniz:

> [!div class="checklist"]
> + [Azure Machine Learning](overview-what-is-azure-ml.md)ilişkili Azure kaynakları için varsayılan sınırlamalar.
> + Kotaları ve limitlerinizi görüntüleyin.
> + Çalışma alanı düzeyi kotaları oluşturun.
> + İstek kotası artar.
> + Özel uç nokta ve DNS kotaları.

Kotaları yönetme ile birlikte, [Azure Machine Learning için maliyetleri nasıl planlayacağınızı &](concept-plan-manage-cost.md)de öğrenebilirsiniz.

## <a name="special-considerations"></a>Özel Konular

+ Kota, kapasite garantisi değil, kredi limiti olur. Büyük ölçekli kapasite gereksinimleriniz varsa, [kotayı artırmak Için Azure desteğine başvurun](#request-quota-increases).

+ Kotalar, aboneliklerinizde Azure Machine Learning dahil olmak üzere tüm hizmetler arasında paylaşılır. Kapasiteyi değerlendirirken tüm hizmetler genelinde kullanımı hesaplayın.
    + Azure Machine Learning Işlem bir özel durumdur ve çekirdek işlem kotasından ayrı bir kota içerir. 

+ Varsayılan sınırlar, ücretsiz deneme, Kullandıkça öde ve sanal makine (VM) serisi (örneğin, Dv2, F, G vb.) için teklif kategorisi türüne göre farklılık gösterir.

## <a name="default-resource-quotas"></a>Varsayılan kaynak kotaları

Bu bölümde, aşağıdaki kaynaklar için varsayılan ve en yüksek kota sınırları hakkında bilgi edineceksiniz:

+ Sanal makineler
+ Azure Machine Learning İşlemi
+ Azure Machine Learning işlem hatları
+ Container Instances
+ Depolama

> [!IMPORTANT]
> Sınırlar değişikliğe tabidir. En son, her zaman Azure için hizmet düzeyi kota [belgesinde](https://docs.microsoft.com/azure/azure-resource-manager/management/azure-subscription-service-limits/) bulunabilir.

### <a name="virtual-machines"></a>Sanal makineler
Her Azure aboneliğinin, tüm hizmetler genelinde sanal makine sayısıyla sınırlı bir limiti vardır. Sanal makine çekirdekleri bölgesel toplam sınıra ve boyut serisi başına bölgesel sınıra (dv2, F vb.) sahiptir. Her iki sınır de ayrı olarak zorlanır.

Örneğin, ABD Doğu toplam VM çekirdek limiti 30, A serisi çekirdek limiti 30 ve D serisi çekirdek limiti 30 olan bir abonelik düşünün. Bu aboneliğin 30 a1 VM 'Leri, 30 D1 VM 'yi veya bir toplam 30 çekirdeği aşmayan bir bileşimini dağıtmaya izin verilir.

Sanal makineler için sınırlar, aşağıdaki tabloda gösterilen değerin üzerinde gerçekleştirilemez.

[!INCLUDE [azure-subscription-limits-azure-resource-manager](../../includes/azure-subscription-limits-azure-resource-manager.md)]

### <a name="azure-machine-learning-compute"></a>Azure Machine Learning İşlemi
[Azure Machine Learning işlem](concept-compute-target.md#azure-machine-learning-compute-managed) , hem çekirdek sayısı hem de bir abonelikte bölge başına izin verilen benzersiz işlem kaynakları sayısı üzerinde varsayılan bir kota sınırına sahiptir. Bu kota, önceki bölümden VM çekirdek kotasından ayrıdır.

Bu bölümdeki sınırları tabloda gösterilen **en fazla sınıra** kadar yükseltmek için [bir kota artışı isteyin](#request-quota-increases) .

Kullanılabilir kaynaklar:
+ **Bölge başına adanmış çekirdekler** , abonelik teklif türüne bağlı olarak varsayılan 24-300 sınırına sahiptir.  Her bir VM ailesi için abonelik başına adanmış çekirdek sayısı artırılabilir. NCv2, NCv3 veya ND serisi gibi özelleştirilmiş sanal makine aileleri, varsayılan sıfır çekirdeklerle başlar.

+ Abonelik teklifi türüne bağlı olarak **bölge başına düşük öncelikli çekirdekler** varsayılan 100-3000 limitine sahiptir. Abonelik başına düşük öncelikli çekirdek sayısı artırılabilir ve VM aileleri arasında tek bir değer olabilir.

+ **Bölge başına kümelerin** varsayılan 200 limiti vardır. Bunlar, bir eğitim kümesi ve bir işlem örneği (Kota amaçları için tek düğümlü bir küme olarak kabul edilir) arasında paylaşılır.

Aşağıdaki tabloda, aşılmayacak ek sınırlar gösterilmektedir.

| **Kaynak** | **Üst sınır** |
| --- | --- |
| Her kaynak grubu için çalışma alanı | 800 |
| Tek bir Azure Machine Learning Işlem (AmlCompute) kaynağındaki düğümler | 100 düğümleri |
| Düğüm başına GPU MPı işlemi | 1-4 |
| Düğüm başına GPU çalışanları | 1-4 |
| İş ömrü | 21 gün<sup>1</sup> |
| Low-Priority düğümünde iş ömrü | 7 gün<sup>2</sup> |
| Düğüm başına parametre sunucuları | 1 |

<sup>1</sup> en fazla ömür süresi, bir çalıştırmanın başladığı ve bittiği zaman arasındaki süreyi belirtir. Tamamlanan çalıştırmalar süresiz olarak devam ederse. Maksimum yaşam süresi içinde tamamlanmayan çalışma verilerine erişilemiyor.
bir Low-Priority düğümündeki <sup>2</sup> iş her zaman bir kapasite kısıtlaması olduğunda geçersiz olabilir. İşinizdeki denetim noktalarını uygulamanızı öneririz.

### <a name="azure-machine-learning-pipelines"></a>Azure Machine Learning işlem hatları
[Azure Machine Learning Işlem hatları](concept-ml-pipelines.md) Aşağıdaki sınırlara sahiptir.

| **Kaynak** | **Sınırlı** |
| --- | --- |
| İşlem hattı adımları | 30.000 |
| Her kaynak grubu için çalışma alanı | 800 |

### <a name="container-instances"></a>Kapsayıcı örnekleri

Daha fazla bilgi için bkz. [Container Instances sınırları](https://docs.microsoft.com/azure/azure-resource-manager/management/azure-subscription-service-limits#container-instances-limits).

### <a name="storage"></a>Depolama
Azure depolama hesaplarının, her abonelik için bölge başına 250 depolama hesabı sınırlaması vardır. Bu, hem standart hem de Premium Depolama hesaplarını içerir.

Limiti artırmak için [Azure desteği](https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest/)aracılığıyla bir istek yapın. Azure depolama ekibi, büyük/küçük harf durumunu gözden geçicektir ve bir bölge için en fazla 250 depolama hesabı onaylayabilir.


## <a name="workspace-level-quota"></a>Çalışma alanı düzeyi kotası

Aynı abonelikteki birden çok [çalışma alanı](concept-workspace.md) arasında işlem hedefi ayırmayı Azure Machine Learning yönetmek için çalışma alanı düzeyi kotaları kullanın.

Varsayılan olarak, tüm çalışma alanları VM aileleri için abonelik düzeyi kotasıyla aynı kotayı paylaşır. Ancak, bir abonelikteki çalışma alanlarında ayrı VM aileleri için maksimum kota ayarlayabilirsiniz. Bu, kapasiteyi paylaşmanızı ve kaynak çekişmesinden kaçınmanızı sağlar:

1. Aboneliğinizdeki herhangi bir çalışma alanına gidin.
1. Sol bölmede, **kullanımlar + kotalar**' ı seçin.
1. Kotaları görüntülemek için **kotaları Yapılandır** sekmesini seçin.
1. Bir VM ailesini genişletin.
1. Bu VM ailesi altında listelenen herhangi bir çalışma alanında bir kota sınırı ayarlayın.

Negatif bir değer veya abonelik düzeyi kotasından daha yüksek bir değer ayarlayamazsınız.

[![Azure Machine Learning çalışma alanı düzeyi kotası](./media/how-to-manage-quotas/azure-machine-learning-workspace-quota.png)](./media/how-to-manage-quotas/azure-machine-learning-workspace-quota.png)

> [!NOTE]
> Çalışma alanı düzeyinde kota ayarlamak için abonelik düzeyi izinlerine sahip olmanız gerekir.

## <a name="view-your-usage-and-quotas"></a>Kullanımınızı ve kotaları görüntüleme

Sanal makineler, depolama, ağ gibi çeşitli Azure kaynaklarına yönelik kotayı görüntülemek için Azure portal kullanın:

1. Sol bölmede, **tüm hizmetler** ' i seçin ve genel kategorisi altında **abonelikler** ' i seçin.

2. Abonelikler listesinden, kotasını Aradığınız aboneliği seçin.

3. Geçerli kota limitlerinizi ve kullanımınızı görüntülemek için **kullanım + kotalar** ' ı seçin. Sağlayıcıyı ve konumları seçmek için filtreleri kullanın. 

Aboneliğinizde Azure Machine Learning Işlem kotası diğer Azure kotalarından ayrı olarak yönetilir. 

1. Azure portal **Azure Machine Learning** çalışma alanınıza gidin.

2. Sol bölmede, **destek + sorun giderme bölümü** altında, geçerli kota limitlerinizi ve kullanımınızı görüntülemek için **kullanım + kotalar** ' i seçin.

3. Kota sınırlarını görüntülemek için bir abonelik seçin. İlgilendiğiniz bölgeye filtre getirmeyi unutmayın.

4. Abonelik düzeyi görünümü ve çalışma alanı düzeyi görünümleri arasında geçiş yapabilirsiniz.

## <a name="request-quota-increases"></a>Kota artışı isteme

Sınırı veya kotayı varsayılan sınırın üzerinde yükseltmek için, ücretsiz [bir çevrimiçi müşteri destek isteği açın](https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest/) .

Sınır, yukarıdaki tablolarda gösterilen maksimum limit değeri üzerinde yer alınamaz. Maksimum sınır yoksa, kaynağın sınırını ayarlayamezsiniz.

Kota artışı istenirken, kotayı artırmak için istediğiniz hizmeti seçin. Örneğin Azure Machine Learning, Container Instances, depolama, vb. Azure Machine Learning Işlem için yukarıdaki adımları izleyerek kotayı görüntülerken kota **iste** düğmesini seçebilirsiniz.

> [!NOTE]
> [Ücretsiz deneme abonelikleri](https://azure.microsoft.com/offers/ms-azr-0044p) sınır veya kota artışına uygun değildir. [Ücretsiz bir deneme aboneliğiniz](https://azure.microsoft.com/offers/ms-azr-0044p)varsa, bir [Kullandıkça Öde](https://azure.microsoft.com/offers/ms-azr-0003p/) aboneliğine yükseltebilirsiniz. Daha fazla bilgi için bkz. [Azure Ücretsiz deneme sürümünü Kullandıkça Öde](../billing/billing-upgrade-azure-subscription.md) ve  [ücretsiz deneme aboneliği hakkında SSS](https://azure.microsoft.com/free/free-account-faq).

## <a name="private-endpoint-and-private-dns-quota-increases"></a>Özel uç nokta ve özel DNS kotası artar

Bir abonelikte oluşturulabilecek özel uç noktalar ve özel DNS bölgeleri sayısında sınırlamalar vardır.

Azure Machine Learning (müşteri) aboneliğinizde kaynak oluşturduğunda, Microsoft 'a ait abonelikte kaynak oluşturan bazı senaryolar vardır.

 Aşağıdaki senaryolarda, Microsoft 'a ait abonelikte bir kota indirimi istemeniz gerekebilir:

* __Müşteri tarafından yönetilen bir anahtarla özel bağlantı etkin çalışma alanı (CMK)__
* __Sanal ağınızın arkasındaki çalışma alanı için Azure Container Registry__
* __Çalışma alanınıza özel bir bağlantı etkin Azure Kubernetes hizmet kümesi iliştirme__.

Bu senaryolar için bir kesinti istemek üzere aşağıdaki adımları kullanın:

1. [Azure destek Isteği oluşturun](/azure/azure-portal/supportability/how-to-create-azure-support-request#create-a-support-request) ve __temel bilgiler__ bölümünde aşağıdaki seçenekleri belirleyin:

    | Alan | Seçim |
    | ----- | ----- |
    | Sorun türü | Teknik |
    | Hizmet | Hizmetlerim. Açılan listede __Machine Learning__ ' yi seçin. |
    | Sorun türü | Çalışma alanı kurulumu, SDK ve CLı |
    | Sorun alt türü | Özel uç nokta ve Özel DNS bölge kesintisi isteği |

2. __Ayrıntılar__ bölümünde, kullanmak istediğiniz Azure bölgesini ve kullanmayı planladığınız senaryoyu sağlamak için __Açıklama__ alanını kullanın. Birden çok abonelik için kota artışı istemeniz gerekiyorsa, bu alandaki abonelik kimliklerini de listeleyin.

3. İsteği oluşturmak için __Oluştur__ ' u seçin.

:::image type="content" source="media/how-to-manage-quotas/quota-increase-private-endpoint.png" alt-text="Özel uç nokta ve özel DNS kota artışı isteği ekran görüntüsü":::

## <a name="next-steps"></a>Sonraki adımlar

+ [Azure Machine Learning için maliyetleri planlayın & yönetin](concept-plan-manage-cost.md)
