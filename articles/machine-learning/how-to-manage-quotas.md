---
title: Kaynakları & kotaları yönetme
titleSuffix: Azure Machine Learning
description: Azure Machine Learning için kaynaklardaki kotalar ve daha fazla kota isteme hakkında bilgi edinin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: jmartens
author: nishankgu
ms.author: nigup
ms.date: 03/05/2020
ms.openlocfilehash: 17a4652604c0faa804d24530869aac0848c972b3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78399149"
---
# <a name="manage-and-request-quotas-for-azure-resources"></a>Azure kaynakları için kotaları yönetme ve talep etme
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

Bu makalede, aboneliğiniz için Azure kaynaklarında önceden yapılandırılmış sınırlar hakkında ayrıntılar verilmektedir. Ayrıca, her kaynak türü için kota geliştirmelerinin nasıl isteneceklerine ilişkin yönergeler de dahildir. Bu sınırlar, dolandırıcılık nedeniyle bütçe fazlalarını önlemek ve Azure kapasite kısıtlamalarını onurlandırmak için uygulamaya konur.

Diğer Azure hizmetlerinde olduğu gibi, Azure Machine Learning ile ilişkili belirli kaynaklarda da sınırlamalar vardır. Bu sınırlar, çalışma alanı sayısındaki bir üst sınırdan, model eğitimi veya çıkarım/puanlama için kullanılan gerçek temel işlemdeki sınırlara kadar değişir. 

Azure Machine Learning kaynaklarınızı üretim iş yükleri için tasarlayıp ölçeklendirirken, bu sınırları göz önünde bulundurun. Örneğin, kümeniz hedef düğüm sayısına ulaşamıyorsa, aboneliğiniz için bir Azure Machine Learning Compute çekirdek sınırına ulaşmış olabilirsiniz. Sınırı veya kotayı Varsayılan Sınırın üzerine çıkarmak istiyorsanız, ücretsiz olarak çevrimiçi müşteri destek isteği açın. Azure Kapasite kısıtlamaları nedeniyle sınırlar aşağıdaki tablolarda gösterilen Maksimum Sınır değerinin üzerine yükseltilemez. Maksimum Sınır sütunu yoksa, kaynağın ayarlanabilir sınırları yoktur.

## <a name="special-considerations"></a>Özel hususlar

+ Kota kredi limitidir, kapasite garantisi değil. Büyük ölçekli kapasite gereksinimleriniz varsa Azure desteğine başvurun.

+ Kotanız, Azure Machine Learning de dahil olmak üzere aboneliklerinizdeki tüm hizmetlerde paylaşılır. Bunun tek istisnası, çekirdek bilgi işlem kotasından ayrı bir kotaya sahip olan Azure Machine Learning bilgi işlemdir. Kapasite ihtiyaçlarınızı değerlendirirken tüm hizmetlerde kota kullanımını hesapladiğinizden emin olun.

+ Varsayılan sınırlar, Serbest Deneme, You-You-Go Öde ve Dv2, F, G gibi VM serisi gibi teklif Kategori Türüne göre değişir.

## <a name="default-resource-quotas"></a>Varsayılan kaynak kotaları

Azure aboneliğinizdeki çeşitli kaynak türlerine göre kota sınırlarının dökümü aşağıda verilmiştir.

> [!IMPORTANT]
> Limitler değişebilir. En son ları her zaman tüm Azure için hizmet düzeyi kota [belgesinde](https://docs.microsoft.com/azure/azure-resource-manager/management/azure-subscription-service-limits/) bulunabilir.

### <a name="virtual-machines"></a>Sanal makineler
Her Azure aboneliği için, hizmetlerinizde veya tek başına sahip olabileceğiniz sanal makine sayısında bir sınır vardır. Bu sınır hem toplam çekirdek hem de aile bazında bölge düzeyindedir.

Sanal makine çekirdekleri bölgesel bir toplam sınıra ve her ikisi de ayrı ayrı uygulanan bölgesel boyut başına seri (Dv2, F, vb.) sınırına sahiptir. Örneğin, ABD Doğu toplam VM çekirdek limiti 30, A serisi çekirdek limiti 30 ve D serisi çekirdek limiti 30 olan bir abonelik düşünün. Bu aboneliğin 30 adet A1 sanal makinesi veya 30 adet D1 sanal makinesi ya da ikisinin toplamda 30 çekirdeği geçmeyecek bir birleşimini (örneğin, 10 adet A1 sanal makinesi ve 20 adet D1 sanal makinesi) dağıtmasına izin verilir.

[!INCLUDE [azure-subscription-limits-azure-resource-manager](../../includes/azure-subscription-limits-azure-resource-manager.md)]

Kota limitlerinin daha ayrıntılı ve güncel bir listesi için Azure genelindeki kota makalesini buradan kontrol [edin.](https://docs.microsoft.com/azure/azure-resource-manager/management/azure-subscription-service-limits)

### <a name="azure-machine-learning-compute"></a>Azure Machine Learning İşlemi
Azure Machine Learning Compute için, bir abonelikte bölge başına izin verilen çekirdek sayısı ve benzersiz bilgi işlem kaynakları nın sayısında varsayılan kota sınırı vardır. Bu kota yukarıdaki VM çekirdek kotasından ayrıdır ve AmlCompute, barındırılan bir modelde kaynakları dağıtan yönetilen bir hizmet olduğundan, temel sınırlar iki kaynak türü arasında paylaşılmaz.

Kullanılabilir kaynaklar:
+ Bölge başına özel çekirdekler, EA ve CSP teklif türleri için daha yüksek varsayılan varsayılan değere sahip abonelik teklif türünüze bağlı olarak varsayılan 24 - 300'e sahiptir.  Abonelik başına özel çekirdek sayısı artırılabilir ve her VM ailesi için farklıdır. NCv2, NCv3 veya ND serisi gibi bazı özel VM aileleri sıfır çekirdek varsayılanıyla başlar. Artır seçeneklerini tartışmak için kota isteği yükselterek Azure desteğine başvurun.

+ Bölge başına düşük öncelikli çekirdekler, EA ve CSP teklif türleri için daha yüksek varsayılan varsayılan değere sahip abonelik teklif türünüze bağlı olarak varsayılan 100 - 3000 arasında bir varsayılan sınıra sahiptir. Abonelik başına düşük öncelikli çekirdek sayısı artırılabilir ve VM aileleri arasında tek bir değerdir. Artış seçeneklerini tartışmak için Azure desteğine başvurun.

+ Bölge başına kümelerin varsayılan sınırı 200'tür. Bunlar bir eğitim kümesi ve bir işlem örneği (kota amaçları için tek bir düğüm kümesi olarak kabul edilir) arasında paylaşılır. Bu sınırın üzerinde bir artış talep etmek istiyorsanız Azure desteğine başvurun.

+ **Vurulduktan sonra aşılamayacak diğer katı sınırlar vardır.

| **Kaynak** | **Maksimum limit** |
| --- | --- |
| Kaynak grubu başına maksimum çalışma alanları | 800 |
| Tek bir Azure Machine Learning Compute (AmlCompute) kaynağında maksimum düğümler | 100 düğüm |
| Düğüm başına maksimum GPU MPI işlemleri | 1-4 |
| Düğüm başına maksimum GPU çalışanları | 1-4 |
| Maksimum iş ömrü | 90 gün<sup>1</sup> |
| Düşük Öncelikli Düğümde maksimum iş ömrü | 7 gün<sup>2</sup> |
| Düğüm başına maksimum parametre sunucuları | 1 |

<sup>1</sup> Maksimum ömür, bir çalıştırmanın başladığı ve ne zaman bityiş yaptığı anlamına gelir. Tamamlanan çalıştırmalar süresiz olarak devam ediyor; maksimum kullanım ömrü içinde tamamlanmayan çalıştırmalar için verilere erişilemez.
<sup>2</sup> Düşük Öncelikli düğümdeki işler, kapasite kısıtlaması olduğunda önceden engellenebilir. İşinizde kontrol noktası uygulamanızı öneririz.

### <a name="azure-machine-learning-pipelines"></a>Azure Makine Öğrenme Boru Hatları
Azure Machine Learning Pipelines için, bir ardışık ardışık alandaki adım sayısında ve bir abonelikte bölge başına yayımlanmış ardışık ardışık ların zamanlama tabanlı çalıştırma sayısıiçin bir kota sınırı vardır.
- Bir boru hattında izin verilen maksimum adım sayısı 30.000'dir
- Aylık abonelik başına yayımlanmış ardışık ardışık programların blog tetiklenen zamanlamaları için zamanlama tabanlı çalıştırmaların ve blob çekmelerinin toplamının toplam Sayısı 100.000'dir

> [!NOTE]
> Bu sınırı artırmak istiyorsanız, [Microsoft Destek'e](https://azure.microsoft.com/support/options/)başvurun.

### <a name="container-instances"></a>Kapsayıcı örnekleri

Ayrıca, belirli bir süre içinde (saatlik olarak kapsamlı) veya tüm aboneliğiniz boyunca dönebileceğiniz kapsayıcı örneklerinin sayısında da bir sınır vardır.

[!INCLUDE [container-instances-limits](../../includes/container-instances-limits.md)]

Kota limitlerinin daha ayrıntılı ve güncel bir listesi için Azure genelindeki kota makalesini buradan kontrol [edin.](https://docs.microsoft.com/azure/azure-resource-manager/management/azure-subscription-service-limits#container-instances-limits)

### <a name="storage"></a>Depolama
Belirli bir abonelikte bölge başına depolama hesabı sayısında da bir sınır vardır. Varsayılan sınır 250'dir ve hem Standart hem de Premium Depolama hesaplarını içerir. Belirli bir bölgede 250'den fazla depolama hesabı gerekiyorsa, [Azure Desteği](https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest/)aracılığıyla bir istekte bulunun. Azure Depolama ekibi işletme durumunuzu inceler ve belirli bir bölge için en fazla 250 depolama hesabı onaylayabilir.


## <a name="workspace-level-quota"></a>Çalışma alanı düzeyi kotası

Çeşitli çalışma alanları arasında Amlcompute için kaynak ayırmaları daha iyi yönetmek için, abonelik düzeyi kotalarını (VM ailesine göre) dağıtmanıza ve çalışma alanı düzeyinde yapılandırmanıza olanak tanıyan bir özellik sunduk. Varsayılan davranış, tüm çalışma alanlarının herhangi bir VM ailesi için abonelik düzeyi kotası ile aynı kotaya sahip olmasıdır. Ancak, çalışma alanı sayısı arttıkça ve değişen öncelikteki iş yükleri aynı kaynakları paylaşmaya başladıkça, kullanıcılar kapasiteyi daha iyi paylaşmak ve kaynak çekişmesi sorunlarını önlemek için bir yol ister. Azure Machine Learning, kullanıcıların her çalışma alanında belirli bir VM ailesi için maksimum kota belirlemesine izin vererek yönetilen bilgi işlem teklifiyle bir çözüm sağlar. Bu, kapasitenizi çalışma alanları arasında dağıtmaya benzer ve kullanıcılar maksimum kullanımı sağlamak için fazla ayırmayı da seçebilir. 

Çalışma alanı düzeyinde kota ayarlamak için aboneliğinizdeki herhangi bir çalışma alanına gidin ve sol bölmedeki **Kullanımlar + kotaları** tıklatın. Ardından kotaları görüntülemek, herhangi bir VM ailesini genişletmek ve bu VM ailesi altında listelenen herhangi bir çalışma alanında kota sınırı ayarlamak için **kotaları yapılandır** sekmesini seçin. Negatif bir değer veya abonelik düzeyi kotasının üzerinde bir değer ayarlayamayacağınızı unutmayın. Ayrıca, gözlemlediğiniz gibi, varsayılan olarak tüm çalışma alanları, ayrılan kotanın tam olarak kullanılmasına izin vermek için tüm abonelik kotasının tamamıa atanır.

[![Azure Machine Learning çalışma alanı düzeyi kotası](./media/how-to-manage-quotas/azure-machine-learning-workspace-quota.png)](./media/how-to-manage-quotas/azure-machine-learning-workspace-quota.png)


> [!NOTE]
> Bu yalnızca bir Enterprise sürümü özelliğidir. Aboneliğinizde hem Temel hem de Kurumsal sürüm çalışma alanınız varsa, bunu yalnızca Kurumsal çalışma alanlarınızda kota ayarlamak için kullanabilirsiniz. Temel çalışma alanlarınız varsayılan davranış olan abonelik düzeyi kotasına sahip olmaya devam eder.
>
> Çalışma alanı düzeyinde kota ayarlamak için abonelik düzeyi izinlerine ihtiyacınız vardır. Bu, tek tek çalışma alanı sahiplerinin kotalarını ayarlamaması veya artırmaması ve başka bir çalışma alanı için ayrılan kaynaklara tecavüz etmeye başlamaması için uygulanır. Bu nedenle, bir abonelik yöneticisi bu kotaları çalışma alanlarına ayırmak ve dağıtmak için en uygun şekilde tasarlanmıştır.



## <a name="view-your-usage-and-quotas"></a>Kullanımınızı ve kotalarınızı görüntüleyin

Sanal Makineler, Depolama, Ağ gibi çeşitli kaynaklar için kotanızı görüntülemek Azure portalı aracılığıyla kolaydır.

1. Sol bölmede Tüm **hizmetleri** seçin ve ardından Genel kategori altındaki **Abonelikleri** seçin.

1. Abonelikler listesinden, kotaaradığınız aboneliği seçin.

   Özellikle Azure Machine Learning Compute kotasını görüntülemek için **bir uyarı vardır.** Yukarıda belirtildiği gibi, bu kota aboneliğinizdeki işlem kotasından ayrıdır.

1. Sol **bölmede, Machine Learning hizmetini** seçin ve ardından gösterilen listeden herhangi bir çalışma alanı seçin

1. Bir sonraki bıçakta, **Destek + sorun giderme bölümünün** altında geçerli kota sınırlarınızı ve kullanımınızı görüntülemek için Kullanım + **kotaları** seçin.

1. Kota sınırlarını görüntülemek için bir abonelik seçin. İlgilendiğiniz bölgeye filtre uygulamayapmayı unutmayın.

1. Artık abonelik düzeyi görünümü ile çalışma alanı düzeyi görünümü arasında geçiş yapabilirsiniz:
    + **Abonelik görünümü:** Bu, vm ailesi tarafından çekirdek kota kullanımınızı görüntülemenizi sağlar, çalışma alanına göre genişletir ve gerçek küme adlarına göre daha da genişletir. Bu görünüm, belirli bir VM ailesinin çalışma alanlarına göre parçalanmayı ve bu çalışma alanlarının her biri için temel kümeler tarafından daha fazla görmek için çekirdek kullanımının ayrıntılarına hızlı bir şekilde girmek için en uygun uymaktadır. Bu görünümdeki genel kural (kullanım/kota), kullanımın geçerli ölçeklenmiş çekirdek sayısı olduğu ve kotanın kaynağın ölçeklendirebileceği mantıksal maksimum çekirdek sayısı olduğu durumdur. Her **çalışma alanı**için kota, belirli bir VM ailesi için ölçeklendirebileceğiniz maksimum çekirdek sayısını gösteren çalışma alanı düzeyi kotası (yukarıda açıklandığı gibi) olacaktır. Benzer şekilde bir **küme** için kota, kümenin max_nodes özelliği tarafından tanımlanana ölçeklendirebileceği maksimum düğüm sayısına karşılık gelen çekirdektir.

    + **Çalışma alanı görünümü:** Bu, temel kota kullanımınızı Çalışma Alanı tarafından görüntülemenize, VM ailesi tarafından genişletmenize ve gerçek küme adlarına göre daha da genişletmenize olanak tanır. Bu görünüm, VM ailelerinin parçalanınmasını ve bu ailelerin her biri için altta yatan kümeleri görmek için belirli bir çalışma alanı için çekirdek kullanımının ayrıntılarına hızlı bir şekilde girmek için en uygun görüştür.

## <a name="request-quota-increases"></a>Kota artışı isteme

Limiti veya kotayı varsayılan sınırın üzerine çıkarmak istiyorsanız, ücretsiz [olarak çevrimiçi müşteri destek isteği açın.](https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest/)

Sınırlar tablolarda gösterilen maksimum sınır değerinin üzerinde yükseltilemeyecek. Maksimum sınır yoksa, kaynağın ayarlanabilir sınırları yoktur. [Bu](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-quota-errors) makalede, kota artış sürecini daha ayrıntılı olarak kapsamaktadır.

Kota artışı talep ederken, Kotayı yükseltmek istediğiniz hizmeti seçmeniz gerekir( bu hizmet Makine Öğrenimi hizmet kotası, Konteyner örnekleri veya Depolama kotası gibi olabilir). Azure Machine Learning Compute için ek olarak, yukarıdaki adımları izleyerek kotayı görüntülerken **Kota İsteği** düğmesini tıklatabilirsiniz.

> [!NOTE]
> [Ücretsiz Deneme abonelikleri](https://azure.microsoft.com/offers/ms-azr-0044p) limit veya kota artışları için uygun değildir. Ücretsiz Deneme [aboneliğiniz](https://azure.microsoft.com/offers/ms-azr-0044p)varsa, [You-You-Go](https://azure.microsoft.com/offers/ms-azr-0003p/) Aboneliğine yükseltebilirsiniz. Daha fazla bilgi için, Azure Ücretsiz Deneme sürümünü [You-You-You-Pay](../billing/billing-upgrade-azure-subscription.md) ve [Ücretsiz Deneme aboneliği SSS'ye](https://azure.microsoft.com/free/free-account-faq)yükseltin' e bakın.
