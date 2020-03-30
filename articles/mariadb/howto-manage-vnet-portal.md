---
title: VNet uç noktalarını yönet - Azure portalı - MariaDB için Azure Veritabanı
description: Azure portalını kullanarak MariaDB VNet hizmet bitiş noktaları ve kuralları için Azure Veritabanı oluşturma ve yönetme
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.topic: conceptual
ms.date: 3/18/2020
ms.openlocfilehash: 61a8337536f55ceda9bef5b7eaa67a37644d2aca
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79530606"
---
# <a name="create-and-manage-azure-database-for-mariadb-vnet-service-endpoints-and-vnet-rules-by-using-the-azure-portal"></a>Azure portalını kullanarak MariaDB VNet hizmet bitiş noktaları ve VNet kuralları için Azure Veritabanı oluşturma ve yönetme

Sanal Ağ (VNet) hizmetleri uç noktaları ve kuralları, bir Sanal Ağın özel adres alanını MariaDB için Azure Veritabanı sunucunuza genişletir. Sınırlamalar da dahil olmak üzere MariaDB VNet hizmet bitiş noktaları için Azure Veritabanı'na genel bakış için [Bkz. MariaDB Server VNet hizmet bitiş noktaları için Azure Veritabanı.](concepts-data-access-security-vnet.md) VNet hizmet bitiş noktaları, MariaDB için Azure Veritabanı için desteklenen tüm bölgelerde kullanılabilir.

> [!NOTE]
> VNet hizmet uç noktaları için destek yalnızca Genel Amaç ve Bellek Optimize Edilmiş sunucular içindir.

## <a name="create-a-vnet-rule-and-enable-service-endpoints"></a>Bir VNet kuralı oluşturun ve hizmet bitiş noktalarını etkinleştirin

1. MariaDB sunucu sayfasında, Ayarlar başlığı altında, MariaDB için Azure Veritabanı için Bağlantı Güvenliği bölmesini açmak için **Bağlantı Güvenliği'ni** tıklatın.

2. Azure hizmetlerine Erişime İzin Ver denetiminin **OFF**olarak ayarlandığından emin olun.

> [!Important]
> A'ya ayarlarsanız, Azure MariaDB Veritabanı sunucunuz herhangi bir alt ağdan iletişimi kabul eder. Denetim kümesini A)'ya bırakmak, güvenlik açısından aşırı erişim olabilir. Microsoft Azure Sanal Ağ hizmeti bitiş noktası özelliği, MariaDB için Azure Veritabanı'nın sanal ağ kuralı özelliğiyle eşgüdüm içinde, birlikte güvenlik yüzey alanınızı azaltabilir.

3. Ardından, **+ Varolan sanal ağ ekleme'ye**tıklayın. Mevcut bir VNet yoksa tıklayın + Oluşturmak için **yeni sanal ağ oluştur.** [Bkz. Hızlı Başlangıç: Azure portalını kullanarak sanal ağ oluşturma](../virtual-network/quick-create-portal.md)

   ![Azure portalı - Bağlantı güvenliği'ni tıklatın](./media/howto-manage-vnet-portal/1-connection-security.png)

4. Bir VNet kural adı girin, abonelik, Sanal ağ ve Subnet adını seçin ve sonra **Etkinleştir'i**tıklatın. Bu, **Microsoft.SQL** hizmet etiketini kullanarak alt ağdaki VNet hizmeti bitiş noktalarını otomatik olarak etkinleştirir.

   ![Azure portalı - VNet'i yapılandırma](./media/howto-manage-vnet-portal/2-configure-vnet.png)

   Hesap, sanal ağ ve hizmet uç noktası oluşturma izinlerine sahip olmalıdır.

   Hizmet bitiş noktaları, sanal ağa yazma erişimi olan bir kullanıcı tarafından sanal ağlarda bağımsız olarak yapılandırılabilir.
    
   Azure hizmet kaynaklarını bir VNet'e sabitlemek için, kullanıcının eklenen alt ağlar için "Microsoft.Network/virtualNetworks/subnets/joinViaServiceEndpoint/" iznine sahip olması gerekir. Bu izin varsayılan olarak yerleşik hizmet yöneticisi rollerinde mevcuttur ve özel roller oluşturularak değiştirilebilir.
    
   [Yerleşik roller](https://docs.microsoft.com/azure/active-directory/role-based-access-built-in-roles) ve [özel rollere](https://docs.microsoft.com/azure/active-directory/role-based-access-control-custom-roles) belirli izinlerin atanması hakkında daha fazla bilgi edinin.
    
   Sanal ağlar ve Azure hizmet kaynakları aynı ağda veya farklı aboneliklerde olabilir. VNet ve Azure hizmet kaynakları farklı aboneliklerdeyse, kaynakların aynı Etkin Dizin (AD) kiracısı altında olması gerekir. Her iki aboneliğin de **Microsoft.Sql** kaynak sağlayıcısının kayıtlı olduğundan emin olun. Daha fazla bilgi için [kaynak yöneticisi-kayıt][resource-manager-portal]

   > [!IMPORTANT]
   > Hizmet bitiş noktalarını yapılandırmadan önce hizmet bitiş noktası yapılandırmaları ve dikkat edilmesi gerekenhususlar hakkında bu makaleyi okumanız önerilir. **Sanal Ağ hizmeti bitiş noktası:** [Sanal Ağ hizmeti bitiş noktası,](../virtual-network/virtual-network-service-endpoints-overview.md) özellik değerleri bir veya daha fazla resmi Azure hizmet türü adlarını içeren bir alt ağdır. VNet hizmetleri bitiş noktaları, SQL Veritabanı adlı Azure hizmetini ifade eden **Microsoft.Sql**hizmet türü adını kullanır. Bu hizmet etiketi, Azure SQL Veritabanı, MariaDB için Azure Veritabanı, PostgreSQL ve MySQL hizmetleri için de geçerlidir. **Microsoft.Sql** hizmet etiketini vnet hizmet bitiş noktasına uygularken Azure SQL Veritabanı, PostgreSQL için Azure Veritabanı, MariaDB için Azure Veritabanı ve alt ağdaki MySQL sunucuları için Azure Veritabanı dahil olmak üzere tüm Azure Veritabanı hizmetleri için hizmet bitiş noktası trafiğini yapılandırır.
   > 

5. Etkinleştirildiğinde **Tamam'ı** tıklatın ve VNet hizmet bitiş noktalarının vnet kuralıyla birlikte etkinleştirildigini görürsünüz.

   ![VNet hizmet bitiş noktaları etkin ve VNet kuralı oluşturuldu](./media/howto-manage-vnet-portal/3-vnet-service-endpoints-enabled-vnet-rule-created.png)

## <a name="next-steps"></a>Sonraki adımlar
- [MariaDB için Azure Veritabanı'nda SSL yapılandırma](howto-configure-ssl.md) hakkında daha fazla bilgi edinin
- Benzer şekilde, VNet hizmet uç noktalarını etkinleştirmek için komut dosyası oluşturabilir [ve Azure CLI'yi kullanarak MariaDB için Azure Veritabanı için bir VNET kuralı oluşturabilirsiniz.](howto-manage-vnet-cli.md)

<!-- Link references, to text, Within this same GitHub repo. --> 
[resource-manager-portal]: ../azure-resource-manager/management/resource-providers-and-types.md