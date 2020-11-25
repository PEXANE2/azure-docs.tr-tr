---
title: Yönetilen özel uç noktalar
description: Azure SYNAPSE Analytics 'te yönetilen özel uç noktaları açıklayan bir makale
author: RonyMSFT
ms.service: synapse-analytics
ms.topic: overview
ms.subservice: security
ms.date: 10/16/2020
ms.author: ronytho
ms.reviewer: jrasnick
ms.openlocfilehash: 930c1f94446eb145a093bf7a8706485d40eec210
ms.sourcegitcommit: a43a59e44c14d349d597c3d2fd2bc779989c71d7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/25/2020
ms.locfileid: "95998184"
---
# <a name="synapse-managed-private-endpoints-preview"></a>SYNAPSE yönetilen özel uç noktaları (Önizleme)

Bu makalede, Azure SYNAPSE Analytics 'te yönetilen özel uç noktalar açıklanmaktadır.

## <a name="managed-private-endpoints"></a>Yönetilen özel uç noktalar

Yönetilen özel uç noktalar, Azure kaynaklarına özel bir bağlantı kurmak Microsoft Azure Sanal Ağ yönetilen çalışma alanında oluşturulan özel uç noktalardır. Azure SYNAPSE, bu özel uç noktaları sizin adınıza yönetir.

Azure SYNAPSE özel bağlantıları destekler. Özel bağlantı, Azure hizmetlerine (Azure depolama ve Azure Cosmos DB gibi) ve Azure 'da barındırılan müşteri/iş ortağı hizmetlerine Azure sanal ağınızdan güvenli bir şekilde erişmenizi sağlar.

Özel bir bağlantı kullandığınızda, sanal ağınız ile çalışma alanınız arasındaki trafik tamamen Microsoft omurga ağı üzerinden geçer. Özel bağlantı, veri savunma risklerine karşı koruma sağlar. Özel bir uç nokta oluşturarak kaynağa özel bir bağlantı kurarsınız.

Özel uç nokta, hizmeti sanal ağınıza etkin bir şekilde taşımak için sanal ağınızdan özel bir IP adresi kullanır. Özel uç noktalar Azure 'daki belirli bir kaynakla eşlenir ve hizmetin tamamı değildir. Müşteriler, şirket tarafından onaylanan belirli bir kaynakla bağlantıyı sınırlayabilir. 

[Özel bağlantılar ve özel uç noktalar](https://docs.microsoft.com/azure/private-link/)hakkında daha fazla bilgi edinin.

>[!IMPORTANT]
>Yönetilen özel uç noktalar yalnızca yönetilen çalışma alanı sanal ağı olan Azure SYNAPSE çalışma alanlarında desteklenir.

>[!NOTE]
>Yönetilen özel uç noktaları hariç yönetilen çalışma alanı sanal ağından gelen tüm giden trafik gelecekte engellenir. Çalışma alanı dışındaki tüm Azure veri kaynaklarınıza bağlanmak için yönetilen özel uç noktalar oluşturmanız önerilir. 

Azure SYNAPSE 'de yönetilen özel uç nokta oluşturduğunuzda bir "bekleyen" durumunda özel bir uç nokta bağlantısı oluşturulur. Bir onay iş akışı başlatılır. Özel bağlantı kaynağı sahibi bağlantıyı onaylaması veya reddetmekten sorumludur.

Sahip bağlantıyı onayladığında, özel bağlantı oluşturulur. Ancak, sahip bağlantıyı onaylamazsa özel bağlantı kurulamayacaktır. Her iki durumda da, yönetilen özel uç nokta bağlantının durumuyla güncelleştirilir.

Yalnızca onaylanan bir durumdaki yönetilen özel uç nokta, belirli bir özel bağlantı kaynağına trafik gönderebilir.

## <a name="managed-private-endpoints-for-dedicated-sql-pool-and-serverless-sql-pool"></a>Adanmış SQL havuzu ve sunucusuz SQL havuzu için yönetilen özel uç noktalar

Adanmış SQL havuzu ve sunucusuz SQL havuzu, Azure SYNAPSE çalışma alanınızdaki analitik yeteneklerdir. Bu yetenekler, [yönetilen çalışma alanı sanal ağına](./synapse-workspace-managed-vnet.md)dağıtılmamış çok kiracılı altyapıyı kullanır.

Bir çalışma alanı oluşturulduğunda, Azure SYNAPSE çalışma alanında, biri adanmış SQL havuzu ve sunucusuz SQL havuzu için olmak üzere iki yönetilen özel uç nokta oluşturur. 

Bu iki yönetilen özel uç nokta SYNAPSE Studio 'da listelenmiştir. Sol gezinti bölmesinde **Yönet** ' i seçin ve ardından bu dosyaları Studio 'da görmek için **yönetilen özel uç noktalar** ' ı seçin.

SQL havuzunu hedefleyen yönetilen özel uç nokta *SYNAPSE-WS- \<workspacename\> SQL* olarak adlandırılır ve sunucusuz SQL havuzunu hedefleyen bir tane *SYNAPSE-WS-sqlondemand-- \<workspacename\>* olarak adlandırılır.

![Adanmış SQL havuzu ve sunucusuz SQL havuzu için yönetilen özel uç noktalar](./media/synapse-workspace-managed-private-endpoints/managed-pe-for-sql-1.png)

Bu iki yönetilen özel uç nokta, Azure SYNAPSE çalışma alanınızı oluştururken sizin için otomatik olarak oluşturulur. Bu iki yönetilen özel uç nokta için ücret ödemeniz gerekmez.

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi edinmek için [veri kaynaklarınıza yönetilen özel uç noktalar oluşturma](./how-to-create-managed-private-endpoints.md) makalesine ilerleyin.
