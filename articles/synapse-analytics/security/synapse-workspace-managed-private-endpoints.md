---
title: Yönetilen özel uç noktaları
description: Azure Synapse Analytics'te Yönetilen özel uç noktaları açıklayan bir makale
author: RonyMSFT
ms.service: synapse-analytics
ms.topic: overview
ms.date: 04/15/2020
ms.author: ronytho
ms.reviewer: jrasnick
ms.openlocfilehash: 08c6610541d987cddd7cf2aeb71c526cb2359598
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81423687"
---
# <a name="synapse-managed-private-endpoints-preview"></a>Synapse Yönetilen özel uç noktaları (önizleme)

Bu makalede, Azure Synapse Analytics'te Yönetilen özel uç noktalar açıklanacaktır.

## <a name="managed-private-endpoints"></a>Yönetilen özel uç noktaları

Yönetilen özel uç noktaları, Azure kaynaklarına özel bir bağlantı oluşturan Yönetilen çalışma alanı VNet'te oluşturulan özel uç noktalarıdır. Azure Synapse bu özel uç noktaları sizin adınıza yönetir.

Azure Synapse özel bağlantıları destekler. Özel bağlantı, Azure hizmetlerine (Azure Depolama, Azure Cosmos DB ve Azure SQL Veri Ambarı gibi) ve Azure VNet'inizden azure barındırılan müşteri/iş ortağı hizmetlerine güvenli bir şekilde erişmenizi sağlar.

Özel bir bağlantı kullandığınızda, VNet'iniz ve çalışma alanınız arasındaki trafik tamamen Microsoft omurga ağı üzerinden geçer. Private Link veri sızma risklerine karşı korur. Özel bir bitiş noktası oluşturarak kaynağa özel bir bağlantı kurarsınız.

Özel uç nokta, hizmeti VNet'inize etkin bir şekilde getirmek için VNet'inizden özel bir IP adresi kullanır. Özel uç noktalar, hizmetin tamamına değil, Azure'daki belirli bir kaynağa eşlenir. Müşteriler, kuruluşları tarafından onaylanan belirli bir kaynakla bağlantıyı sınırlayabilir. [Özel bağlantılar ve özel uç noktalar](https://docs.microsoft.com/azure/private-link/)hakkında daha fazla bilgi edinin.

>[!IMPORTANT]
>Yönetilen özel uç noktalar yalnızca Yönetilen çalışma alanı VNet'i ile Azure Synapse çalışma alanlarında desteklenir.
>[!NOTE]
>Tüm Azure veri kaynaklarınıza bağlanmak için Yönetilen özel uç noktaları oluşturmanız önerilir. yönetilen çalışma alanı VNet'>gelen tüm giden trafik gelecekte engellenir.

Azure Sinaps'ta Yönetilen özel bitiş noktası oluşturduğunuzda "Beklemede" durumunda özel bir uç nokta bağlantısı oluşturulur. Bir onay iş akışı başlatılır. Özel bağlantı kaynağı sahibi bağlantıyı onaylamak veya reddetmekle yükümlüdür.

Sahibi bağlantıyı onaylarsa, özel bağlantı kurulur. Aksi takdirde, özel bağlantı kurulamaz. Her iki durumda da Yönetilen özel bitiş noktası bağlantının durumuyla güncelleştirilir.

Yalnızca onaylanmış bir durumda yönetilen özel bitiş noktası, trafiği belirli bir özel bağlantı kaynağına gönderebilir.

## <a name="managed-private-endpoints-for-sql-pool-and-sql-on-demand"></a>SQL havuzu ve isteğe bağlı SQL için yönetilen özel uç noktaları

SQL havuzu ve isteğe bağlı SQL, Azure Synapse çalışma alanınızdaki analitik özelliklerdir. Bu özellikler, [Yönetilen çalışma alanı VNet'ine](./synapse-workspace-managed-vnet.md)dağıtılmamayla birlikte çok kiracılı altyapıyı kullanır.

Bir çalışma alanı oluşturulduğunda, Azure Synapse, söz bu çalışma alanında SQL havuzuna ve isteğe bağlı SQL'e iki Yönetilen özel uç noktası oluşturur. 

Bu yönetilen iki özel uç nokta Azure Synapse Studio'da listelenir. Sol navigasyonda **Yönet'i** seçin ve ardından Stüdyo'da görmek için **Yönetilen Sanal Ağlar'ı** seçin.

SQL havuzunu hedefleyen Yönetilen özel bitiş noktasına *sinaps-ws-sql denir--\<çalışma alanı adı\> * ve SQL on-demand'i hedefleyene *sinaps-ws-sqlOnDemand denir.\<\>*
![SQL havuzu ve isteğe bağlı SQL için yönetilen özel uç noktaları](./media/synapse-workspace-managed-private-endpoints/managed-pe-for-sql-1.png)

Azure Synapse çalışma alanınızı oluşturduğunuzda bu iki Yönetilen özel uç nokta sizin için otomatik olarak oluşturulur. Yönetilen bu iki özel uç nokta için ücretlendirilmezsiniz.

## <a name="next-steps"></a>Sonraki adımlar

[Veri kaynaklarınıza yönetilen özel uç noktaları oluşturun](./how-to-create-managed-private-endpoints.md)
