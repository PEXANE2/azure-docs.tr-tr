---
title: Kaynak Yöneticisi Mimarisi
description: Azure Hizmeti Kumaş Küme Kaynak Yöneticisi hizmeti hakkında genel bakış ve mimari bilgilere.
author: masnider
ms.topic: conceptual
ms.date: 08/18/2017
ms.author: masnider
ms.openlocfilehash: 94ed906533d108081d620e9b183ecfee249d85ca
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75551701"
---
# <a name="cluster-resource-manager-architecture-overview"></a>Küme kaynak yöneticisi mimarisine genel bakış
Hizmet Kumaş Küme Kaynak Yöneticisi kümede çalışan merkezi bir hizmettir. Özellikle kaynak tüketimi ve herhangi bir yerleşim kuralı açısından kümedeki hizmetlerin istenilen durumunu yönetir. 

Kümenizdeki kaynakları yönetmek için Hizmet Kumaş Küme Kaynak Yöneticisi'nin birkaç bilgi parçası olması gerekir:

- Şu anda hangi hizmetler var
- Her hizmetin geçerli (veya varsayılan) kaynak tüketimi 
- Kalan küme kapasitesi 
- Kümedeki düğümlerin kapasitesi 
- Her düğümde tüketilen kaynak miktarı

Belirli bir hizmetin kaynak tüketimi zaman içinde değişebilir ve hizmetler genellikle birden fazla kaynak türünü önemser. Farklı hizmetler de, hem gerçek fiziksel hem de fiziksel kaynaklar ölçülmektedir. Hizmetler bellek ve disk tüketimi gibi fiziksel ölçümleri izleyebilir. Daha yaygın olarak, hizmetler mantıksal ölçümleri önemseyebilir - "WorkQueueDepth" veya "TotalRequests" gibi şeyler. Hem mantıksal hem de fiziksel ölçümler aynı kümede kullanılabilir. Ölçümler birçok hizmette paylaşılabilir veya belirli bir hizmete özgü olabilir.

## <a name="other-considerations"></a>Diğer konular
Kümenin sahipleri ve işleçleri hizmet ve uygulama yazarlarından farklı olabilir veya en azından farklı şapkalar takan kişilerdir. Uygulamanızı geliştirdiğinizde ne gerektirdiği hakkında birkaç şey bilirsiniz. Tüketeceği kaynakların ve farklı hizmetlerin nasıl dağıtılacağı hakkında bir tahmininiz var. Örneğin, web katmanının Internet'e maruz kalan düğümlerde çalışması gerekirken, veritabanı hizmetlerinin çalışmaması gerekir. Başka bir örnek olarak, web hizmetleri büyük olasılıkla CPU ve ağ tarafından kısıtlanırken, veri katmanı hizmetleri bellek ve disk tüketimi hakkında daha fazla önem vermektedir. Ancak, üretimde bu hizmet için bir canlı site olayını işleyen veya hizmete yükseltmeyi yöneten kişinin yapması gereken farklı bir iş vardır ve farklı araçlar gerektirir. 

Hem küme hem de hizmetler dinamiktir:

- Kümedeki düğüm sayısı büyüyebilir ve küçülebilir
- Farklı boyut ve türdeki düğümler gelip gidebilir
- Hizmetler oluşturulabilir, kaldırılabilir ve istenilen kaynak tahsisleri ve yerleşim kurallarını değiştirebilir
- Yükseltmeler veya diğer yönetim işlemleri, altyapı düzeylerinde uygulamada kümede yuvarlanabilir
- Hatalar her an olabilir.

## <a name="cluster-resource-manager-components-and-data-flow"></a>Kaynak yöneticisi bileşenlerini ve veri akışını kümeleme
Küme Kaynak Yöneticisi, her hizmetin gereksinimlerini ve bu hizmetlerdeki her hizmet nesnesi tarafından kaynak tüketimini izlemek zorundadır. Küme Kaynak Yöneticisi'nin iki kavramsal bölümü vardır: her düğümde çalışan aracılar ve hataya dayanıklı bir hizmet. Her düğümdeki aracılar hizmetlerden gelen yük raporlarını toplar, toplar ve bunları periyodik olarak bildirir. Küme Kaynak Yöneticisi hizmeti, yerel aracılardan gelen tüm bilgileri toplar ve geçerli yapılandırmasına göre tepki verir.

Aşağıdaki diyagrama bakalım:

<center>

![Kaynak Dengeleyici Mimarisi][Image1]
</center>

Çalışma süresi sırasında, gerçekleşebilecek birçok değişiklik vardır. Örneğin, bazı hizmetlerin tükettiği kaynak miktarının değiştiğini, bazı hizmetlerin başarısız olduğunu ve bazı düğümlerin kümeyi birleştirip terk ettigini varsayalım. Düğümdeki tüm değişiklikler toplanır ve düzenli olarak Küme Kaynak Yöneticisi hizmetine (1,2) gönderilir ve burada yeniden toplanır, analiz edilir ve depolanır. Hizmetin değişikliklere baktığı ve herhangi bir eylemin gerekli olup olmadığını belirlediği birkaç saniyede bir (3). Örneğin, kümeye bazı boş düğümlerin eklendiğini fark edebilir. Sonuç olarak, bazı hizmetleri bu düğümlere taşımaya karar verir. Küme Kaynak Yöneticisi, belirli bir düğümün aşırı yüklendiğini veya belirli hizmetlerin başarısız olduğunu veya silindiğini fark ederek başka yerlerdeki kaynakları serbest edebilir.

Aşağıdaki diyagrama bakalım ve bundan sonra ne olacağını görelim. Küme Kaynak Yöneticisi'nin değişikliklerin gerekli olduğunu belirlediğini varsayalım. Gerekli değişiklikleri yapmak için diğer sistem hizmetleriyle (özellikle Failover Yöneticisi) eşgüdüm içindedir. Daha sonra gerekli komutlar uygun düğümlere gönderilir (4). Örneğin, Kaynak Yöneticisi'nin Node5'in aşırı yüklendiğini fark ettiğini ve bu nedenle B hizmetini Node5'ten Node4'e taşımaya karar verdiğini varsayalım. Yeniden yapılandırmanın sonunda (5), küme aşağıdaki gibi görünür:

<center>

![Kaynak Dengeleyici Mimarisi][Image2]
</center>

## <a name="next-steps"></a>Sonraki adımlar
- Küme Kaynak Yöneticisi kümeyi açıklamak için birçok seçenek vardır. Onlar hakkında daha fazla bilgi edinmek için, [Bir Hizmet Kumaş kümesi açıklayan](./service-fabric-cluster-resource-manager-cluster-description.md) bu makaleye göz atın
- Küme Kaynak Yöneticisi'nin birincil görevleri kümeyi yeniden dengelemek ve yerleşim kurallarını zorlamaktır. Bu davranışları yapılandırma hakkında daha fazla bilgi için [Hizmet Kumaşı kümenizi dengeleme](./service-fabric-cluster-resource-manager-balancing.md)

[Image1]:./media/service-fabric-cluster-resource-manager-architecture/Service-Fabric-Resource-Manager-Architecture-Activity-1.png
[Image2]:./media/service-fabric-cluster-resource-manager-architecture/Service-Fabric-Resource-Manager-Architecture-Activity-2.png
