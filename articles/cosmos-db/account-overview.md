---
title: Azure Cosmos DB hesaplarıyla çalışma
description: Bu makalede, Azure Cosmos hesaplarının nasıl oluşturulup kullanıldığı açıklanmaktadır. Ayrıca, bir Azure Cosmos hesabındaki öğelerin hiyerarşisini gösterir
author: markjbrown
ms.author: mjbrown
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.topic: conceptual
ms.date: 07/23/2019
ms.reviewer: sngun
ms.openlocfilehash: d29ed68b2945b2473b33aa88176e6f5d832a0fba
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79246909"
---
# <a name="work-with-azure-cosmos-account"></a>Azure Cosmos hesabıyla çalışma

Azure Cosmos DB, hizmet olarak tam olarak yönetilen bir platformdur (PaaS). Azure Cosmos DB'yi kullanmaya başlamak için, azure aboneliğinizde başlangıçta bir Azure Cosmos hesabı oluşturmanız gerekir. Azure Cosmos hesabınız benzersiz bir DNS adı içerir ve azure portalı, Azure CLI kullanarak veya dile özel farklı SDK'lar kullanarak bir hesabı yönetebilirsiniz. Daha fazla bilgi için [Azure Cosmos hesabınızı nasıl yönetirizene](how-to-manage-database-account.md)bakın.

Azure Cosmos hesabı, küresel dağıtım ın ve yüksek kullanılabilirliğin temel birimidir. Verilerinizi ve iş lerinizi birden çok Azure bölgesinde genel olarak dağıtmak için, Azure bölgelerini istediğiniz zaman Azure Cosmos hesabınıza ekleyebilir ve kaldırabilirsiniz. Azure Cosmos hesabınızı tek veya birden çok yazma bölgesi olacak şekilde yapılandırabilirsiniz. Daha fazla bilgi için [Azure Cosmos hesabınıza Azure bölgelerini nasıl ekleyeceğinize ve kaldırabilirsiniz.](how-to-manage-database-account.md) Azure Cosmos hesabında [varsayılan tutarlılık](consistency-levels.md) düzeyini yapılandırabilirsiniz. Azure Cosmos DB, yüzde 99'luk yüzdelik, tutarlılık ve yüksek kullanılabilirlikte iş sonu, gecikme süresi ve iş artışını kapsayan kapsamlı SLA'lar sağlar. Daha fazla bilgi için Azure [Cosmos DB SLA'lara](https://azure.microsoft.com/support/legal/sla/cosmos-db/v1_2/)bakın.

Azure Cosmos hesabınızdaki tüm verilere erişimi güvenli bir şekilde yönetmek için hesabınızla ilişkili [ana anahtarları](secure-access-to-data.md) kullanabilirsiniz. Verilerinize erişimi daha da güvenli hale getirmek için Azure Cosmos hesabınızda bir [VNET hizmet bitiş noktası](vnet-service-endpoint.md) ve [IP güvenlik duvarı](firewall-support.md) yapılandırabilirsiniz. 

## <a name="elements-in-an-azure-cosmos-account"></a>Azure Cosmos hesabındaki öğeler

Azure Cosmos kapsayıcı ölçeklenebilirliğin temel birimidir. Bir kapta sınırsız bir şekilde sağlanan iş (RU/s) ve depolama nız olabilir. Azure Cosmos DB, sağlanan iş lerinizi ve depolamanızı elastik olarak ölçeklendirmek için belirttiğiniz mantıksal bölümleme anahtarını kullanarak kabınızı saydam bir şekilde bölümlere ayırır. Daha fazla bilgi için Azure [Cosmos kapsayıcıları ve öğeleriyle çalışma](databases-containers-items.md)bilginini alın.

Şu anda, bir Azure aboneliği altında en fazla 100 Azure Cosmos hesabı oluşturabilirsiniz. Tek bir Azure Cosmos hesabı, sınırsız miktarda veriyi ve sağlanan iş miktarını neredeyse yönetebilir. Verilerinizi ve sağlanan iş lerinizi yönetmek için, hesabınız altında ve bu veritabanı içinde bir veya daha fazla Azure Veritabanı oluşturabilirsiniz, bir veya daha fazla kapsayıcı oluşturabilirsiniz. Aşağıdaki resim, bir Azure Cosmos hesabındaki öğelerin hiyerarşisini gösterir:

![Azure Cosmos hesabının hiyerarşisi](./media/account-overview/hierarchy.png)

## <a name="next-steps"></a>Sonraki adımlar

Azure Cosmos hesabınızı ve diğer kavramları nasıl yöneteceklerini öğrenin:

* [Azure Cosmos hesabınızı yönetme](how-to-manage-database-account.md)
* [Küresel dağıtım](distribute-data-globally.md)
* [Tutarlılık düzeyleri](consistency-levels.md)
* [Azure Cosmos kapsayıcıları ve öğeleriyle çalışma](databases-containers-items.md)
* [Azure Cosmos hesabınız için VNET hizmet bitiş noktası](vnet-service-endpoint.md)
* [Azure Cosmos hesabınız için IP güvenlik duvarı](firewall-support.md)
* [Azure Cosmos hesabınıza Azure bölgeleri ekleme ve kaldırma](how-to-manage-database-account.md)
* [Azure Cosmos DB SLA'lar](https://azure.microsoft.com/support/legal/sla/cosmos-db/v1_2/)
