---
title: Azure Cosmos hesapları için IP güvenlik duvarı
description: Güvenlik duvarı desteği için IP erişim denetim ilkelerini kullanarak Azure Cosmos DB verileri güvenli hale getirme hakkında bilgi edinin.
author: kanshiG
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/23/2019
ms.author: govindk
ms.openlocfilehash: 9398eb4038afcd17788e750fcb5c27c76e9f3f44
ms.sourcegitcommit: fad3aaac5af8c1b3f2ec26f75a8f06e8692c94ed
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/27/2020
ms.locfileid: "66241073"
---
# <a name="ip-firewall-in-azure-cosmos-db"></a>Azure Cosmos DB'de IP güvenlik duvarı

Hesabınızda depolanan verilerin güvenliğini sağlamak için Azure Cosmos DB güçlü bir karma tabanlı İleti Kimlik Doğrulama Kodu (HMAC) kullanan gizli tabanlı bir yetkilendirme modelini destekler. Ayrıca, Azure Cosmos DB gelen güvenlik duvarı desteği için IP tabanlı erişim denetimlerini destekler. Bu model geleneksel bir veritabanı sisteminin güvenlik duvarı kurallarına benzer ve hesabınıza ek bir güvenlik düzeyi sağlar. Güvenlik duvarları sayesinde Azure Cosmos hesabınızı yalnızca onaylanan bir makine ve/veya bulut hizmeti kümesinden erişilebilen şekilde yapılandırabilirsiniz. Azure Cosmos veritabanınızda bu onaylanan makine ve hizmet kümelerinden depolanan verilere erişim, çağıranın geçerli bir yetkilendirme belirteci sunmasını gerektirir.

## <a name="ip-access-control-overview"></a><a id="ip-access-control-overview"></a>IP erişim denetimine genel bakış

Varsayılan olarak, Azure Cosmos hesabınıza, istek geçerli bir yetkilendirme belirteci eşlik ettiği sürece internet 'ten erişilebilir. IP İlkesi tabanlı erişim denetimini yapılandırmak için, kullanıcının belirli bir Azure Cosmos hesabına erişmesi için izin verilen istemci IP 'Leri listesine dahil edilecek IP adresleri veya IP adresi aralıklarını CıDR (sınıfsız etki alanları arası yönlendirme) formunda sağlaması gerekir. Bu yapılandırma uygulandıktan sonra, izin verilen bu listenin dışındaki makinelerden gelen istekler 403 (yasak) yanıtını alır. IP güvenlik duvarını kullanırken Azure portal hesabınıza erişmesine izin vermeniz önerilir. Veri Gezgini 'nin kullanımına izin vermek için erişim ve Azure portal görüntülenen hesabınıza yönelik ölçümleri almak için erişim gerekir. Veri Gezgini 'ni kullanırken Azure portal hesabınıza erişmesine izin vermenin yanı sıra, güvenlik duvarı ayarlarınızı da geçerli IP adresinizi güvenlik duvarı kurallarına eklemek için güncelleştirmeniz gerekir. Güvenlik Duvarı değişikliklerinin yayılması 15 dakika kadar sürebilir. 

IP tabanlı güvenlik duvarını alt ağ ve VNET erişim denetimiyle birleştirebilirsiniz. Bunları birleştirerek, erişimi genel IP ve/veya VNET içindeki belirli bir alt ağdan sınırlayabilirsiniz. Alt ağ ve VNET tabanlı erişim denetimi kullanma hakkında daha fazla bilgi için bkz. [sanal ağlardan erişim Azure Cosmos DB kaynakları](vnet-service-endpoint.md).

Özetlemek gerekirse, Azure Cosmos hesabına erişmek için yetkilendirme belirtecinin her zaman gereklidir. IP güvenlik duvarı ve VNET Access Control listesi (ACL 'Ler) ayarlanmamışsa Azure Cosmos hesabına yetkilendirme belirteciyle erişilebilir. IP güvenlik duvarı veya VNET ACL 'Leri veya her ikisi de Azure Cosmos hesabında kurulduktan sonra, yalnızca belirttiğiniz kaynaklardan (ve yetkilendirme belirteciyle) gelen istekler geçerli yanıtlar alır. 

## <a name="next-steps"></a>Sonraki adımlar

Ardından, aşağıdaki belgeleri kullanarak hesabınız için IP güvenlik duvarını veya VNET hizmet uç noktasını yapılandırabilirsiniz:

* [Azure Cosmos hesabınız için IP güvenlik duvarını yapılandırma](how-to-configure-firewall.md)
* [Sanal ağlardan Azure Cosmos DB kaynaklara erişme](vnet-service-endpoint.md)
* [Azure Cosmos hesabınız için sanal ağ hizmet uç noktasını yapılandırma](how-to-configure-vnet-service-endpoint.md)




