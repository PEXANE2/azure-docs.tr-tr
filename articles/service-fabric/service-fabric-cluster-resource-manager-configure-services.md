---
title: Ölçümleri ve yerleşim ayarlarını belirtin
description: Ölçümleri, yerleşim kısıtlamalarını ve diğer yerleşim ilkelerini belirterek Bir Hizmet Kumaşı hizmetini nasıl açıklayacağız öğrenin.
author: masnider
ms.topic: conceptual
ms.date: 08/18/2017
ms.author: masnider
ms.openlocfilehash: d4dcd319000edb204ba188ed14b4c797dba5cd38
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75610106"
---
# <a name="configuring-cluster-resource-manager-settings-for-service-fabric-services"></a>Service Fabric hizmetleri için küme kaynak yöneticisi ayarlarını yapılandırma
Service Fabric Cluster Kaynak Yöneticisi, hizmet adlı her bireyi yöneten kurallar üzerinde ince taneli denetim sağlar. Adlandırılmış her hizmet, kümeye nasıl ayrılması gerektiğine bağlı kurallar belirleyebilir. Adlandırılmış her hizmet, bu hizmet için ne kadar önemli oldukları da dahil olmak üzere, bildirmek istediği ölçümler kümesini de tanımlayabilir. Hizmetleri yapılandırma üç farklı görev ayrılır:

1. Yerleşim kısıtlamalarını yapılandırma
2. Ölçümleri yapılandırma
3. Gelişmiş yerleşim ilkelerini ve diğer kuralları yapılandırma (daha az yaygın)

## <a name="placement-constraints"></a>Yerleşim kısıtlamaları
Yerleşim kısıtlamaları, bir hizmetin kümedeki hangi düğümlerin gerçekten üzerinde çalıştığını denetlemek için kullanılır. Genellikle belirli bir adlandırılmış hizmet örneği veya belirli bir türdeki tüm hizmetler belirli bir düğüm türünde çalışmak üzere sınırlandırılmıştır. Yerleşim kısıtlamaları genişletilebilir. Düğüm türü başına herhangi bir özellik kümesini tanımlayabilir ve ardından hizmet oluştururken kısıtlamalarla bunlar için seçim yapabilirsiniz. Bir hizmetin çalışırken yerleşim kısıtlamalarını da değiştirebilirsiniz. Bu, kümedeki veya hizmetgereksinimlerindeki değişikliklere yanıt vermenize olanak tanır. Belirli bir düğümün özellikleri kümede dinamik olarak da güncellenebilir. Yerleşim kısıtlamaları ve bunların nasıl yapılandırılabildiği hakkında daha fazla bilgi [bu makalede](service-fabric-cluster-resource-manager-cluster-description.md#node-properties-and-placement-constraints) bulunabilir

## <a name="metrics"></a>Ölçümler
Ölçümler, belirli bir adlandırılmış hizmetin ihtiyaç duyduğu kaynak kümesidir. Bir hizmetin metrik yapılandırması, bu kaynağın ne kadarını her bir durum lu yinelemenin veya bu hizmetin durumsuz örneğinin varsayılan olarak ne kadarını tükettiğini içerir. Ölçümler, dengelemenin gerekli olması durumunda, bu ölçümün söz konusu hizmet için ne kadar önemli olduğunu gösteren bir ağırlık da içerir.

## <a name="advanced-placement-rules"></a>Gelişmiş yerleşim kuralları
Daha az yaygın senaryolarda yararlı olan başka yerleşim kuralları türleri de vardır. Bazı örnekler şunlardır:
- Coğrafi olarak dağıtılmış kümelere yardımcı olan kısıtlamalar
- Belirli uygulama mimarileri

Diğer yerleşim kuralları Korelasyonlar veya İlkeler aracılığıyla yapılandırılır.

## <a name="next-steps"></a>Sonraki adımlar
- Ölçümler, Service Fabric Cluster Resource Manger'ın kümedeki tüketimi ve kapasiteyi nasıl yönettiğidir. Ölçümler ve bunları nasıl yapılandıracağınız hakkında daha fazla bilgi edinmek için [bu makaleye](service-fabric-cluster-resource-manager-metrics.md) göz atın
- Yakınlık, hizmetleriniz için yapılandırabileceğiniz bir moddur. Bu yaygın değildir, ama ihtiyacınız varsa [burada](service-fabric-cluster-resource-manager-advanced-placement-rules-affinity.md) bu konuda öğrenebilirsiniz
- Ek senaryoları işlemek için hizmetinizde yapılandırılabilen birçok farklı yerleşim kuralı vardır. Bu farklı yerleşim politikaları hakkında [burada](service-fabric-cluster-resource-manager-advanced-placement-rules-placement-policies.md) bilgi edinebilirsiniz
- Baştan başlayın ve [Hizmet Kumaş Küme Kaynak Yöneticisi'ne Giriş alın](service-fabric-cluster-resource-manager-introduction.md)
- Küme Kaynak Yöneticisi'nin kümedeki yükü nasıl yönettiği ve dengeler yaptığı hakkında bilgi edinmek [için, yükü dengeleme](service-fabric-cluster-resource-manager-balancing.md) makalesine göz atın
- Küme Kaynak Yöneticisi kümeyi açıklamak için birçok seçenek vardır. Onlar hakkında daha fazla bilgi edinmek için, [Bir Hizmet Kumaş kümesi açıklayan](service-fabric-cluster-resource-manager-cluster-description.md) bu makaleye göz atın
