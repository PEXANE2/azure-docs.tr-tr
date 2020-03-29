---
title: MySQL için VNet uç noktalarını yönet - Azure portalı - Azure Veritabanı
description: Azure portalını kullanarak MySQL VNet hizmet bitiş noktaları ve kuralları için Azure Veritabanı oluşturma ve yönetme
author: bolzmj
ms.author: mbolz
ms.service: mysql
ms.topic: conceptual
ms.date: 3/18/2020
ms.openlocfilehash: fec85f7d87bbfe0039744b9ea63239083ff1c8a0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80063526"
---
# <a name="create-and-manage-azure-database-for-mysql-vnet-service-endpoints-and-vnet-rules-by-using-the-azure-portal"></a>Azure portalını kullanarak MySQL VNet hizmet bitiş noktaları ve VNet kuralları için Azure Veritabanı oluşturma ve yönetme
Sanal Ağ (VNet) hizmetleri uç noktaları ve kuralları, bir Sanal Ağın özel adres alanını MySQL için Azure Veritabanı sunucunuza genişletir. Sınırlamalar da dahil olmak üzere MySQL VNet hizmet bitiş noktaları için Azure Veritabanı'na genel bakış için [MySQL Server VNet hizmet bitiş noktaları için Azure Veritabanı'na](concepts-data-access-and-security-vnet.md)bakın. VNet hizmet bitiş noktaları MySQL için Azure Veritabanı için desteklenen tüm bölgelerde kullanılabilir.

> [!NOTE]
> VNet hizmet uç noktaları için destek yalnızca Genel Amaç ve Bellek Optimize Edilmiş sunucular içindir.
> VNet'in eşlemesi durumunda, trafik hizmet bitiş noktalarıyla birlikte ortak bir VNet Ağ Geçidi'nden akıyorsa ve eşe akması gerekiyorsa, lütfen Ağ Geçidi VNet'teki Azure Sanal Makinelerin MySQL sunucusu için Azure Veritabanı'na erişmesine izin vermek için bir ACL/VNet kuralı oluşturun.


## <a name="create-a-vnet-rule-and-enable-service-endpoints-in-the-azure-portal"></a>Azure portalında bir VNet kuralı oluşturun ve hizmet bitiş noktalarını etkinleştirin

1. MySQL sunucu sayfasında, Ayarlar başlığı altında, MySQL için Azure Veritabanı için Bağlantı Güvenliği bölmesini açmak için **Bağlantı Güvenliği'ni** tıklatın. 

2. Azure hizmetlerine Erişime İzin Ver denetiminin **OFF**olarak ayarlandığından emin olun.

> [!Important]
> Denetim kümesini AİÇ'e bırakırsanız, Azure MySQL Veritabanı sunucunuz herhangi bir alt ağdan iletişimi kabul eder. Denetim kümesini A)'ya bırakmak, güvenlik açısından aşırı erişim olabilir. Microsoft Azure Sanal Ağ hizmeti bitiş noktası özelliği, MySQL için Azure Veritabanı'nın sanal ağ kuralı özelliğiyle eşgüdüm içinde, birlikte güvenlik yüzey alanınızı azaltabilir.

3. Ardından, **+ Varolan sanal ağ ekleme'ye**tıklayın. Mevcut bir VNet yoksa tıklayın + Oluşturmak için **yeni sanal ağ oluştur.** [Bkz. Hızlı Başlangıç: Azure portalını kullanarak sanal ağ oluşturma](../virtual-network/quick-create-portal.md)

   ![Azure portalı - Bağlantı güvenliği'ni tıklatın](./media/howto-manage-vnet-using-portal/1-connection-security.png)

4. Bir VNet kural adı girin, abonelik, Sanal ağ ve Subnet adını seçin ve sonra **Etkinleştir'i**tıklatın. Bu, **Microsoft.SQL** hizmet etiketini kullanarak alt ağdaki VNet hizmeti bitiş noktalarını otomatik olarak etkinleştirir.

   ![Azure portalı - VNet'i yapılandırma](./media/howto-manage-vnet-using-portal/2-configure-vnet.png)

   Hesap, sanal ağ ve hizmet uç noktası oluşturma izinlerine sahip olmalıdır.

   Hizmet bitiş noktaları, sanal ağa yazma erişimi olan bir kullanıcı tarafından sanal ağlarda bağımsız olarak yapılandırılabilir.
    
   Azure hizmet kaynaklarını bir VNet'e sabitlemek için, kullanıcının eklenen alt ağlar için "Microsoft.Network/virtualNetworks/subnets/joinViaServiceEndpoint/" iznine sahip olması gerekir. Bu izin varsayılan olarak yerleşik hizmet yöneticisi rollerinde mevcuttur ve özel roller oluşturularak değiştirilebilir.
    
   [Yerleşik roller](https://docs.microsoft.com/azure/active-directory/role-based-access-built-in-roles) ve [özel rollere](https://docs.microsoft.com/azure/active-directory/role-based-access-control-custom-roles) belirli izinlerin atanması hakkında daha fazla bilgi edinin.
    
   Sanal ağlar ve Azure hizmet kaynakları aynı ağda veya farklı aboneliklerde olabilir. VNet ve Azure hizmet kaynakları farklı aboneliklerdeyse, kaynakların aynı Etkin Dizin (AD) kiracısı altında olması gerekir. Her iki aboneliğin de **Microsoft.Sql** kaynak sağlayıcısının kayıtlı olduğundan emin olun. Daha fazla bilgi için [kaynak yöneticisi-kayıt][resource-manager-portal]

   > [!IMPORTANT]
   > Hizmet bitiş noktalarını yapılandırmadan önce hizmet bitiş noktası yapılandırmaları ve dikkat edilmesi gerekenhususlar hakkında bu makaleyi okumanız önerilir. **Sanal Ağ hizmeti bitiş noktası:** [Sanal Ağ hizmeti bitiş noktası,](../virtual-network/virtual-network-service-endpoints-overview.md) özellik değerleri bir veya daha fazla resmi Azure hizmet türü adlarını içeren bir alt ağdır. VNet hizmetleri bitiş noktaları, SQL Veritabanı adlı Azure hizmetini ifade eden **Microsoft.Sql**hizmet türü adını kullanır. Bu hizmet etiketi, Azure SQL Veritabanı, PostgreSQL için Azure Veritabanı ve MySQL hizmetleri için de geçerlidir. **Microsoft.Sql** hizmet etiketini vnet hizmet bitiş noktasına uygularken, azure SQL Veritabanı, PostgreSQL için Azure Veritabanı ve alt ağdaki MySQL sunucuları için Azure Veritabanı dahil olmak üzere tüm Azure Veritabanı hizmetleri için hizmet bitiş noktası trafiğini yapılandırMaktadır. 
   > 

5. Etkinleştirildiğinde **Tamam'ı** tıklatın ve VNet hizmet bitiş noktalarının vnet kuralıyla birlikte etkinleştirildigini görürsünüz.

   ![VNet hizmet bitiş noktaları etkin ve VNet kuralı oluşturuldu](./media/howto-manage-vnet-using-portal/3-vnet-service-endpoints-enabled-vnet-rule-created.png)

## <a name="next-steps"></a>Sonraki adımlar
- Benzer şekilde, [VNet hizmet uç noktalarını etkinleştirmek ve Azure CLI'yi kullanarak MySQL için Azure Veritabanı için bir VNET kuralı oluşturabilirsiniz.](howto-manage-vnet-using-cli.md)
- MySQL sunucusu için bir Azure Veritabanına bağlanma konusunda yardım için [MySQL için Azure Veritabanı bağlantı kitaplıklarına](./concepts-connection-libraries.md) bakın

<!-- Link references, to text, Within this same GitHub repo. --> 
[resource-manager-portal]: ../azure-resource-manager/management/resource-providers-and-types.md