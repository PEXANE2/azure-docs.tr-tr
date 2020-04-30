---
title: Azure SYNAPSE Analytics 'te yönetilen sanal ağ
description: Azure SYNAPSE Analytics 'te yönetilen sanal ağı açıklayan bir makale
author: RonyMSFT
ms.service: synapse-analytics
ms.topic: overview
ms.date: 04/15/2020
ms.author: ronytho
ms.reviewer: jrasnick
ms.openlocfilehash: 162d96244b01f8c5e1acf224475aadb9508f0aa5
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "81423638"
---
# <a name="azure-synapse-analytics-managed-virtual-network-preview"></a>Azure SYNAPSE Analytics yönetilen sanal ağ (Önizleme)

Bu makalede, yönetilen sanal Ağa gelen Azure SYNAPSE Analytics açıklanmaktadır.

## <a name="managed-workspace-vnet"></a>Yönetilen çalışma alanı VNet

Azure SYNAPSE çalışma alanınızı oluşturduğunuzda bir VNet ile ilişkilendirmeyi seçebilirsiniz. Çalışma alanınız ile ilişkili VNet Azure SYNAPSE tarafından yönetiliyor. Bu sanal ağa yönetilen bir *çalışma alanı VNET*adı verilir.

Yönetilen çalışma alanı VNet size dört şekilde değer sağlar:

- Yönetilen bir çalışma alanı VNet ile VNet 'i Azure SYNAPSE 'e yönetme yükünü devretmek için yük devretme gerçekleştirebilirsiniz.
- Azure SYNAPSE Management trafiğinin sanal ağınıza girmesine izin vermek için kendi sanal ağlarınızdaki gelen NSG kurallarını yapılandırmanız gerekmez. Bu NSG kurallarının yanlış yapılandırılması, müşteriler için hizmet kesintilerine neden olur.
- En yüksek yük temelinde Spark kümeleriniz için bir alt ağ oluşturmanız gerekmez.
- Yönetilen özel uç noktaları ile birlikte yönetilen çalışma alanı sanal ağı, veri taşmakla karşı koruma sağlar. Yönetilen özel uç noktaları yalnızca kendisiyle ilişkili bir yönetilen çalışma alanı VNet 'i olan bir çalışma alanında oluşturabilirsiniz.

Yönetilen bir çalışma alanı VNet ile ilişkili bir çalışma alanı oluşturmak, çalışma alanınızın diğer çalışma alanlarından yalıtılmış olmasını sağlar. Azure SYNAPSE, bir çalışma alanında çeşitli analitik yetenekler sağlar: veri tümleştirme, Apache Spark, SQL havuzu ve isteğe bağlı SQL.

Çalışma alanınızda yönetilen bir çalışma alanı VNet varsa, veri tümleştirme ve Spark kaynakları buna dağıtılır. Yönetilen bir çalışma alanı VNet aynı zamanda Spark etkinlikleri için Kullanıcı düzeyinde yalıtım sağlar çünkü her Spark kümesi kendi alt ağında yer almaktadır.

SQL havuzu ve isteğe bağlı SQL, çok kiracılı bir yetenektir ve bu nedenle yönetilen çalışma alanı VNet 'in dışında bulunur. SQL havuzuna ve isteğe bağlı SQL 'e yönelik çalışma alanı iletişimi, Azure özel bağlantılarını kullanır. Bu özel bağlantılar, kendisiyle ilişkili yönetilen bir çalışma alanı VNet 'i olan bir çalışma alanı oluşturduğunuzda sizin için otomatik olarak oluşturulur.

>[!IMPORTANT]
>Çalışma alanı oluşturulduktan sonra bu çalışma alanı yapılandırmasını değiştiremezsiniz. Örneğin, kendisiyle ilişkili bir yönetilen çalışma alanı VNet 'i olmayan bir çalışma alanını yeniden yapılandıramaz ve bir VNet 'i onunla ilişkilendirebilirsiniz. Benzer şekilde, onunla ilişkilendirilen yönetilen bir çalışma alanı VNet ile bir çalışma alanını yeniden yapılandıramaz ve VNet 'in ilişkisini kaldırın.

## <a name="create-an-azure-synapse-workspace-with-a-managed-workspace-vnet"></a>Yönetilen çalışma alanı VNet ile Azure SYNAPSE çalışma alanı oluşturma

İlişkili bir yönetilen çalışma alanı VNet 'i olan bir Azure SYNAPSE çalışma alanı oluşturmak için, Azure portal ' deki **güvenlik + ağ** sekmesini seçin ve **yönetilen sanal ağı etkinleştir** onay kutusunu işaretleyin.

Onay kutusunu işaretlenmemiş olarak bırakırsanız, çalışma alanınızın kendisiyle ilişkili bir VNet 'i olmayacaktır.

>[!IMPORTANT]
>Özel bağlantıları yalnızca yönetilen bir çalışma alanı VNet 'i olan bir çalışma alanında kullanabilirsiniz.

![Yönetilen çalışma alanı VNet 'i etkinleştir](./media/synapse-workspace-managed-vnet/enable-managed-vnet-1.png)

>[!NOTE]
>Yönetilen çalışma alanı VNet 'ten gelen tüm giden trafik gelecekte engellenir. Yönetilen özel uç noktaları kullanarak tüm veri kaynaklarınıza bağlanmanız önerilir.

Azure portal ' den **Genel Bakış ' ı** seçerek Azure SYNAPSE çalışma alanınızın yönetilen bir çalışma alanı VNET ile ilişkili olup olmadığını kontrol edebilirsiniz.

![Azure portal çalışma alanına genel bakış](./media/synapse-workspace-managed-vnet/enable-managed-vnet-2.png)

## <a name="next-steps"></a>Sonraki adımlar

[Azure SYNAPSE çalışma alanı](../quickstart-create-workspace.md) oluşturma

[Yönetilen özel uç noktalar](./synapse-workspace-managed-private-endpoints.md) hakkında daha fazla bilgi edinin

[Veri kaynaklarınızda yönetilen özel uç noktalar oluşturun](./how-to-create-managed-private-endpoints.md)
