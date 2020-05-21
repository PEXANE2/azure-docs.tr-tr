---
title: Yönetilen özel uç noktalar
description: Azure SYNAPSE Analytics 'te yönetilen özel uç noktaları açıklayan bir makale
author: RonyMSFT
ms.service: synapse-analytics
ms.topic: overview
ms.date: 04/15/2020
ms.author: ronytho
ms.reviewer: jrasnick
ms.openlocfilehash: dc748260c6e71eaaa469defb227d5cc2748a1345
ms.sourcegitcommit: fdec8e8bdbddcce5b7a0c4ffc6842154220c8b90
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/19/2020
ms.locfileid: "83645317"
---
# <a name="synapse-managed-private-endpoints-preview"></a>SYNAPSE yönetilen özel uç noktaları (Önizleme)

Bu makalede, Azure SYNAPSE Analytics 'te yönetilen özel uç noktalar açıklanmaktadır.

## <a name="managed-private-endpoints"></a>Yönetilen özel uç noktalar

Yönetilen özel uç noktalar, Azure kaynaklarına özel bir bağlantı oluşturan yönetilen çalışma alanı sanal ağı 'nda oluşturulan özel uç noktalardır. Azure SYNAPSE, bu özel uç noktaları sizin adınıza yönetir.

Azure SYNAPSE özel bağlantıları destekler. Özel bağlantı, Azure hizmetlerine (Azure depolama, Azure Cosmos DB ve Azure SQL veri ambarı gibi) ve Azure 'da barındırılan müşteri/iş ortağı hizmetlerine güvenli bir şekilde erişmenizi sağlar.

Özel bir bağlantı kullandığınızda, VNet ve çalışma alanınız arasındaki trafik tamamen Microsoft omurga ağı üzerinden geçer. Özel bağlantı, veri savunma risklerine karşı koruma sağlar. Özel bir uç nokta oluşturarak kaynağa özel bir bağlantı kurarsınız.

Özel uç nokta, hizmeti sanal ağınıza etkin bir şekilde taşımak için sanal ağınızdan özel bir IP adresi kullanır. Özel uç noktalar Azure 'daki belirli bir kaynakla eşlenir ve hizmetin tamamı değildir. Müşteriler, şirket tarafından onaylanan belirli bir kaynakla bağlantıyı sınırlayabilir. [Özel bağlantılar ve özel uç noktalar](https://docs.microsoft.com/azure/private-link/)hakkında daha fazla bilgi edinin.

>[!IMPORTANT]
>Yönetilen özel uç noktalar yalnızca yönetilen bir çalışma alanı VNet 'i olan Azure SYNAPSE çalışma alanlarında desteklenir.

>[!NOTE]
>Yönetilen özel uç noktalar hariç yönetilen çalışma alanı VNet 'ten giden tüm trafik gelecekte engellenir. Çalışma alanı dışındaki tüm Azure veri kaynaklarınıza bağlanmak için yönetilen özel uç noktalar oluşturmanız önerilir. 

Azure SYNAPSE 'de yönetilen özel uç nokta oluşturduğunuzda bir "bekleyen" durumunda özel bir uç nokta bağlantısı oluşturulur. Bir onay iş akışı başlatılır. Özel bağlantı kaynağı sahibi bağlantıyı onaylaması veya reddetmekten sorumludur.

Sahip bağlantıyı onayladığında, özel bağlantı oluşturulur. Aksi takdirde, özel bağlantı kurulmaz. Her iki durumda da, yönetilen özel uç nokta bağlantının durumuyla güncelleştirilir.

Yalnızca onaylanan bir durumdaki yönetilen özel uç nokta, belirli bir özel bağlantı kaynağına trafik gönderebilir.

## <a name="managed-private-endpoints-for-sql-pool-and-sql-on-demand"></a>SQL havuzu ve isteğe bağlı SQL için yönetilen özel uç noktalar

SQL havuzu ve isteğe bağlı SQL, Azure SYNAPSE çalışma alanınızda analitik yeteneklerdir. Bu yetenekler, [yönetilen çalışma alanı VNET](./synapse-workspace-managed-vnet.md)'e dağıtılan çok kiracılı altyapıyı kullanır.

Bir çalışma alanı oluşturulduğunda Azure SYNAPSE, SQL havuzunda ve bu çalışma alanında SQL isteğe bağlı olarak iki yönetilen özel uç nokta oluşturur. 

Bu iki yönetilen özel uç nokta Azure SYNAPSE Studio 'da listelenmiştir. Sol gezinti bölmesinde **Yönet** ' i seçin ve ardından Studio 'da görmek Için **yönetilen sanal ağlar** ' ı seçin.

SQL havuzunu hedefleyen yönetilen özel uç nokta *SYNAPSE-WS-SQL-- \< çalışmaadı \> * olarak ADLANDıRıLıR ve isteğe bağlı SQL 'i hedefleyen bir tane *SYNAPSE-WS-sqlondemand-- \< çalışmaadı \> *olarak adlandırılır.
![SQL havuzu ve isteğe bağlı SQL için yönetilen özel uç noktalar](./media/synapse-workspace-managed-private-endpoints/managed-pe-for-sql-1.png)

Bu iki yönetilen özel uç nokta, Azure SYNAPSE çalışma alanınızı oluştururken sizin için otomatik olarak oluşturulur. Bu iki yönetilen özel uç nokta için ücret ödemeniz gerekmez.

## <a name="next-steps"></a>Sonraki adımlar

[Veri kaynaklarınızda yönetilen özel uç noktalar oluşturun](./how-to-create-managed-private-endpoints.md)
