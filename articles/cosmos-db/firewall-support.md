---
title: Azure Cosmos hesapları için IP güvenlik duvarı
description: Güvenlik duvarı desteği için IP erişim denetim ilkelerini kullanarak Azure Cosmos DB verilerini nasıl güvene aldığınızı öğrenin.
author: kanshiG
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/23/2019
ms.author: govindk
ms.openlocfilehash: 9398eb4038afcd17788e750fcb5c27c76e9f3f44
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "66241073"
---
# <a name="ip-firewall-in-azure-cosmos-db"></a>Azure Cosmos DB'de IP güvenlik duvarı

Azure Cosmos DB, hesabınızda depolanan verileri güvenli hale getirmek için, güçlü bir Karma tabanlı İleti Kimlik Doğrulama Kodu (HMAC) kullanan gizli tabanlı yetkilendirme modelini destekler. Ayrıca, Azure Cosmos DB gelen güvenlik duvarı desteği için IP tabanlı erişim denetimlerini destekler. Bu model, geleneksel veritabanı sisteminin güvenlik duvarı kurallarına benzer ve hesabınıza ek bir güvenlik düzeyi sağlar. Güvenlik duvarlarıyla Azure Cosmos hesabınızı yalnızca onaylanmış bir makine ve/veya bulut hizmeti kümesinden erişebilecek şekilde yapılandırabilirsiniz. Azure Cosmos veritabanınızda depolanan bu onaylı makine ve hizmet kümelerinden depolanan verilere erişim, arayanın geçerli bir yetkilendirme belirteci sunmasını gerektirir.

## <a name="ip-access-control-overview"></a><a id="ip-access-control-overview"></a>IP erişim kontrolüne genel bakış

Varsayılan olarak, isteğe geçerli bir yetkilendirme belirteci eşlik ettiği sürece Azure Cosmos hesabınıza Internet'ten erişilebilir. IP ilkesi tabanlı erişim denetimini yapılandırmak için, kullanıcının belirli bir Azure Cosmos hesabına erişmek için istemci IP'lerin izin verilen listesi olarak eklenecek CIDR (Sınıfsız Etki Alanları Arası Yönlendirme) formundaki IP adresleri kümesini veya IP adresi aralıklarını sağlaması gerekir. Bu yapılandırma uygulandıktan sonra, izin verilen listenin dışındaki makinelerden gelen istekler 403 (Yasak) yanıt alır. IP güvenlik duvarını kullanırken, Azure portalının hesabınıza erişmesine izin vermeniz önerilir. Azure portalında görünen hesabınız için veri gezgini kullanımına izin vermenin yanı sıra ölçümleri almak için erişim gereklidir. Veri gezgini kullanırken, Azure portalının hesabınıza erişmesine izin vermenin yanı sıra, geçerli IP adresinizi güvenlik duvarı kurallarına eklemek için güvenlik duvarı ayarlarınızı da güncelleştirmeniz gerekir. Güvenlik duvarı değişikliklerinin yayılması 15 dakika kadar sürebilir. 

IP tabanlı güvenlik duvarını alt ağ ve VNET erişim denetimiyle birleştirebilirsiniz. Bunları birleştirerek, ortak IP'si olan herhangi bir kaynağa ve/veya VNET içindeki belirli bir alt ağdan erişimi sınırlandırabilirsiniz. Alt ağ ve VNET tabanlı erişim denetimi kullanma hakkında daha fazla bilgi edinmek için [sanal ağlardaki Access Azure Cosmos DB kaynaklarına](vnet-service-endpoint.md)bakın.

Özetlemek gerekirse, bir Azure Cosmos hesabına erişmek için her zaman yetkilendirme belirteci gereklidir. IP güvenlik duvarı ve VNET Erişim Denetim Listesi (ALA'lar) ayarlanmazsa, yetkilendirme belirteciyle Azure Cosmos hesabına erişilebilir. IP güvenlik duvarı veya VNET ALA'ları veya her ikisi de Azure Cosmos hesabında ayarlandıktan sonra, yalnızca belirttiğiniz kaynaklardan (ve yetkilendirme belirteciyle) gelen istekler geçerli yanıtlar alır. 

## <a name="next-steps"></a>Sonraki adımlar

Ardından, aşağıdaki dokümanları kullanarak hesabınız için IP güvenlik duvarı veya VNET hizmet bitiş noktasını yapılandırabilirsiniz:

* [Azure Cosmos hesabınız için IP güvenlik duvarı nasıl yapılandırılabilen](how-to-configure-firewall.md)
* [Azure Cosmos DB kaynaklarına sanal ağlardan erişin](vnet-service-endpoint.md)
* [Azure Cosmos hesabınız için sanal ağ hizmeti bitiş noktası nasıl yapılandırılabilen](how-to-configure-vnet-service-endpoint.md)




