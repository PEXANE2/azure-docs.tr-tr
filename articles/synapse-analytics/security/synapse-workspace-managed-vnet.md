---
title: Azure Synapse Analytics'te yönetilen sanal ağ
description: Azure Synapse Analytics'te Yönetilen sanal ağı açıklayan bir makale
author: RonyMSFT
ms.service: synapse-analytics
ms.topic: overview
ms.date: 04/15/2020
ms.author: ronytho
ms.reviewer: jrasnick
ms.openlocfilehash: 162d96244b01f8c5e1acf224475aadb9508f0aa5
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81423638"
---
# <a name="azure-synapse-analytics-managed-virtual-network-preview"></a>Azure Synapse Analytics Yönetilen Sanal Ağ (önizleme)

Bu makalede, Azure Synapse Analytics'te Yönetilen Sanal Ağ açıklanacaktır.

## <a name="managed-workspace-vnet"></a>Yönetilen çalışma alanı VNet

Azure Synapse çalışma alanınızı oluşturduğunuzda, bu alanı bir VNet ile ilişkilendirmeyi seçebilirsiniz. Çalışma alanınızla ilişkili VNet, Azure Synapse tarafından yönetilir. Bu VNet *yönetilen çalışma alanı VNet*olarak adlandırılır.

Yönetilen çalışma alanı VNet size dört şekilde değer sağlar:

- Yönetilen çalışma alanı VNet ile VNet'i yönetme yükünü Azure Synapse'ye yükleyebilirsiniz.
- Azure Synapse yönetim trafiğinin VNet'inize girmesine izin vermek için gelen NSG kurallarını kendi VNet'lerinizde yapılandırmanız gerekmez. Bu NSG kurallarının yanlış yapılandırılması müşteriler için hizmet kesintisine neden olur.
- En yüksek yüke dayalı Spark kümeleriniz için bir alt ağ oluşturmanız gerekmez.
- Yönetilen çalışma alanı VNet ve Yönetilen özel uç noktaları veri sızmasına karşı korur. Yönetilen özel uç noktaları yalnızca onunla ilişkili yönetilen çalışma alanı VNet'i olan bir çalışma alanında oluşturabilirsiniz.

Onunla ilişkili yönetilen bir çalışma alanı VNet ile bir çalışma alanı oluşturmak, çalışma alanınızın diğer çalışma alanlarından yalıtılmış ağ olmasını sağlar. Azure Synapse, çalışma alanında çeşitli analitik özellikler sağlar: Veri tümleştirmesi, Apache Spark, SQL havuzu ve isteğe bağlı SQL.

Çalışma alanınızda Yönetilen çalışma alanı VNet'i varsa, Veri tümleştirmesi ve Spark kaynakları bu alanda dağıtılır. Yönetilen çalışma alanı VNet, her Spark kümesi kendi alt ağına sahip olduğundan, Spark etkinlikleri için kullanıcı düzeyinde yalıtım da sağlar.

SQL havuzu ve isteğe bağlı SQL çok kiracılı özelliklerdir ve bu nedenle Yönetilen çalışma alanı VNet'in dışında yer eder. Sql havuzuna ve isteğe bağlı SQL'e çalışma alanı içi iletişim, Azure özel bağlantılarını kullanır. Bu özel bağlantılar, ilişkili yönetilen bir çalışma alanı VNet'i olan bir çalışma alanı oluşturduğunuzda sizin için otomatik olarak oluşturulur.

>[!IMPORTANT]
>Çalışma alanı oluşturulduktan sonra bu çalışma alanı yapılandırmasını değiştiremezsiniz. Örneğin, onunla ilişkili Yönetilen çalışma alanı VNet'i olmayan bir çalışma alanını yeniden yapılandıramaz ve bir VNet'i ilişkilendiremezsiniz. Benzer şekilde, bir çalışma alanını, onunla ilişkili yönetilen bir çalışma alanı VNet ile yeniden yapılandıramazsınız ve VNet'i ondan ayıramazsınız.

## <a name="create-an-azure-synapse-workspace-with-a-managed-workspace-vnet"></a>Yönetilen çalışma alanı VNet ile Azure Synapse çalışma alanı oluşturma

Onunla ilişkili Yönetilen çalışma alanı VNet'i olan bir Azure Synapse çalışma alanı oluşturmak için Azure portalında **Güvenlik + ağ sekmesini** seçin ve yönetilen sanal ağ onay kutusunu **etkinleştir'i** işaretleyin.

Onay kutusunu işaretlenmeden bırakırsanız, çalışma alanınızda bununla ilişkili bir VNet olmaz.

>[!IMPORTANT]
>Özel bağlantıları yalnızca Yönetilen çalışma alanı VNet'i olan bir çalışma alanında kullanabilirsiniz.

![Yönetilen çalışma alanı VNet'i etkinleştirme](./media/synapse-workspace-managed-vnet/enable-managed-vnet-1.png)

>[!NOTE]
>Yönetilen çalışma alanı VNet'in tüm giden trafiği gelecekte engellenir. Yönetilen özel uç noktaları kullanarak tüm veri kaynaklarınıza bağlanmanız önerilir.

Azure portalından **Genel Bakış'ı** seçerek Azure Synapse çalışma alanınızın Yönetilen çalışma alanı VNet ile ilişkili olup olmadığını kontrol edebilirsiniz.

![Azure portalında çalışma alanına genel bakış](./media/synapse-workspace-managed-vnet/enable-managed-vnet-2.png)

## <a name="next-steps"></a>Sonraki adımlar

Azure [Synapse Çalışma Alanı](../quickstart-create-workspace.md) Oluşturma

[Yönetilen özel uç noktalar](./synapse-workspace-managed-private-endpoints.md) hakkında daha fazla bilgi edinin

[Veri kaynaklarınıza yönetilen özel uç noktaları oluşturun](./how-to-create-managed-private-endpoints.md)
