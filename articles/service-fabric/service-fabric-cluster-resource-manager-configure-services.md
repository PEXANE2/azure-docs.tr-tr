---
title: Ölçüm ve yerleştirme ayarlarını belirtin
description: Ölçümleri, yerleştirme kısıtlamalarını ve diğer yerleştirme ilkelerini belirterek bir Service Fabric hizmetini nasıl tanımlacağınızı öğrenin.
author: masnider
ms.topic: conceptual
ms.date: 08/18/2017
ms.author: masnider
ms.openlocfilehash: d4dcd319000edb204ba188ed14b4c797dba5cd38
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "75610106"
---
# <a name="configuring-cluster-resource-manager-settings-for-service-fabric-services"></a>Service Fabric Hizmetleri için küme kaynak yöneticisi ayarlarını yapılandırma
Service Fabric kümesi Kaynak Yöneticisi, her bir bireysel adlı hizmeti yöneten kurallar üzerinde ayrıntılı denetim sağlar. Her bir adlandırılmış hizmet, kümede nasıl ayrılabileceği için kuralları belirtebilir. Her bir adlandırılmış hizmet, bu hizmete ne kadar önemli olduğunu da içerecek şekilde raporlamak istediği ölçüm kümesini tanımlayabilir. Hizmetleri üç farklı göreve ayırır:

1. Yerleştirme kısıtlamalarını yapılandırma
2. Ölçümleri yapılandırma
3. Gelişmiş yerleştirme ilkelerini ve diğer kuralları yapılandırma (daha az ortak)

## <a name="placement-constraints"></a>Yerleştirme kısıtlamaları
Yerleştirme kısıtlamaları, kümedeki bir hizmetin gerçekten çalışacağı düğümleri denetlemek için kullanılır. Tipik olarak belirli bir adlandırılmış hizmet örneği veya belirli bir tür düğüm üzerinde çalışmak için kısıtlanmış olan tüm hizmetler. Yerleştirme kısıtlamaları genişletilebilir. Düğüm türü başına herhangi bir özellik kümesi tanımlayabilir ve ardından hizmet oluştururken kısıtlamalarla bunlarla ilgili olarak seçim yapabilirsiniz. Ayrıca, çalışırken bir hizmetin yerleştirme kısıtlamalarını da değiştirebilirsiniz. Bu, kümedeki değişikliklere veya hizmetin gereksinimlerine yanıt vermenize olanak tanır. Belirli bir düğümün özellikleri, kümede dinamik olarak da güncelleştirilebilen olabilir. Yerleştirme kısıtlamaları ve bunların nasıl yapılandırılacağı hakkında daha fazla bilgi [Bu makalede](service-fabric-cluster-resource-manager-cluster-description.md#node-properties-and-placement-constraints) bulunabilir

## <a name="metrics"></a>Ölçümler
Ölçümler, belirli bir adlandırılmış hizmetin ihtiyaç duyacağı kaynak kümesidir. Bir hizmetin ölçüm yapılandırması, her durum bilgisiz çoğaltma veya bu hizmetin durum bilgisi olmayan örneği için varsayılan olarak kullanılan kaynak miktarını içerir. Ölçümler Ayrıca, ölçümün bu hizmete ne kadar önemli dengelenmesini belirten bir ağırlık de içerir.

## <a name="advanced-placement-rules"></a>Gelişmiş yerleştirme kuralları
Daha az yaygın senaryolarda yararlı olan diğer yerleştirme kuralları türleri vardır. Bazı örnekler şunlardır:
- Coğrafi olarak dağıtılmış kümelerle yardımcı olan kısıtlamalar
- Belirli uygulama mimarileri

Diğer yerleştirme kuralları, correlations veya policies aracılığıyla yapılandırılır.

## <a name="next-steps"></a>Sonraki adımlar
- Ölçümler, Service Fabric küme kaynağı Yöneticisi 'nin kümedeki tüketimi ve kapasiteyi nasıl yönettiğini açıklamaktadır. Ölçümler ve bunların nasıl yapılandırılacağı hakkında daha fazla bilgi edinmek için [Bu makaleye](service-fabric-cluster-resource-manager-metrics.md) göz atın
- Benzeşim, hizmetleriniz için yapılandırabileceğiniz bir moddur. Bu, yaygın değildir, ancak ihtiyacınız varsa [buradan](service-fabric-cluster-resource-manager-advanced-placement-rules-affinity.md) öğrenebilirsiniz
- Ek senaryoları işlemek için hizmetinizde yapılandırılabilecek birçok farklı yerleştirme kuralı vardır. Bu farklı yerleştirme ilkeleri hakkında [buradan](service-fabric-cluster-resource-manager-advanced-placement-rules-placement-policies.md) edinebilirsiniz
- Baştan başlayın ve [Service Fabric kümesine giriş yapın Kaynak Yöneticisi](service-fabric-cluster-resource-manager-introduction.md)
- Kaynak Yöneticisi kümenin, kümedeki yükü nasıl yönettiğini ve dengelemediğini öğrenmek için [Yük Dengeleme](service-fabric-cluster-resource-manager-balancing.md) sayfasındaki makaleye göz atın
- Küme Kaynak Yöneticisi, kümeyi açıklamak için birçok seçenek içerir. Bunlarla ilgili daha fazla bilgi edinmek için [Service Fabric kümesini açıklama](service-fabric-cluster-resource-manager-cluster-description.md) konusunda bu makaleye göz atın
