---
title: VNet uç noktalarını yönetme-Azure portal-MariaDB için Azure veritabanı
description: Azure portal kullanarak MariaDB VNet hizmet uç noktaları ve kuralları için Azure veritabanı oluşturma ve yönetme
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.topic: conceptual
ms.date: 3/18/2020
ms.openlocfilehash: 61a8337536f55ceda9bef5b7eaa67a37644d2aca
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "79530606"
---
# <a name="create-and-manage-azure-database-for-mariadb-vnet-service-endpoints-and-vnet-rules-by-using-the-azure-portal"></a>Azure portal kullanarak MariaDB VNet hizmet uç noktaları ve VNet kuralları için Azure veritabanı oluşturun ve yönetin

Sanal Ağ (VNet) hizmetleri uç noktaları ve kuralları, bir Sanal Ağın özel adres alanını MariaDB için Azure Veritabanı sunucunuza genişletir. MariaDB VNet hizmet uç noktaları için Azure veritabanı 'nda sınırlamalar da dahil genel bir bakış için bkz. [MariaDB sunucu VNET hizmet uç noktaları Için Azure veritabanı](concepts-data-access-security-vnet.md). Sanal ağ hizmeti uç noktaları, MariaDB için Azure veritabanı için desteklenen tüm bölgelerde kullanılabilir.

> [!NOTE]
> VNet hizmet uç noktaları için destek yalnızca Genel Amaçlı ve bellek için Iyileştirilmiş sunucular içindir.

## <a name="create-a-vnet-rule-and-enable-service-endpoints"></a>VNet kuralı oluşturma ve hizmet uç noktalarını etkinleştirme

1. MariaDB sunucusu sayfasında, ayarlar başlığı altında **bağlantı güvenliği** ' ne tıklayarak MariaDB Için Azure veritabanı bağlantı güvenliği bölmesini açın.

2. Azure hizmetleri denetimine erişime Izin ver ' in **kapalı**olarak ayarlandığından emin olun.

> [!Important]
> AÇıK olarak ayarlarsanız, Azure MariaDB veritabanı sunucunuz herhangi bir alt ağdan iletişim kabul eder. Denetim kümesini açık olarak bırakmak, bir güvenlik noktasından aşırı erişim sağlayabilir. Microsoft Azure Sanal Ağ Hizmeti uç noktası özelliği, MariaDB için Azure veritabanı 'nın sanal ağ kuralı özelliğiyle birlikte, güvenlik yüzeyi alanınızı azaltabilir.

3. Ardından, **var olan sanal ağı ekleme**' ye tıklayın. Mevcut bir VNet 'iniz yoksa bir tane oluşturmak için **+ Yeni sanal ağ oluştur ' a** tıklayabilirsiniz. Bkz [. hızlı başlangıç: Azure Portal kullanarak sanal ağ oluşturma](../virtual-network/quick-create-portal.md)

   ![Azure portal-bağlantı güvenliği ' ne tıklayın](./media/howto-manage-vnet-portal/1-connection-security.png)

4. VNet kural adı girin, abonelik, sanal ağ ve alt ağ adını seçin ve ardından **Etkinleştir**' e tıklayın. Bu, alt ağdaki VNet hizmet uç noktalarını **Microsoft. SQL** hizmet etiketi kullanılarak otomatik olarak sunar.

   ![Azure portal-sanal ağı yapılandırma](./media/howto-manage-vnet-portal/2-configure-vnet.png)

   Hesap, sanal ağ ve hizmet uç noktası oluşturma izinlerine sahip olmalıdır.

   Hizmet uç noktaları sanal ağ üzerinde, sanal ağa yazma erişimi olan bir kullanıcı tarafından bağımsız olarak yapılandırılabilir.
    
   Azure hizmet kaynaklarını bir sanal ağa güvenli hale getirmek için, eklenmekte olan alt ağlar için kullanıcının "Microsoft. Network/virtualNetworks/alt ağları/Jodavetli Aserviceendpoint/" iznine sahip olması gerekir. Bu izin varsayılan olarak yerleşik hizmet yöneticisi rollerinde mevcuttur ve özel roller oluşturularak değiştirilebilir.
    
   [Yerleşik roller](https://docs.microsoft.com/azure/active-directory/role-based-access-built-in-roles) ve [özel rollere](https://docs.microsoft.com/azure/active-directory/role-based-access-control-custom-roles) belirli izinlerin atanması hakkında daha fazla bilgi edinin.
    
   Sanal ağlar ve Azure hizmet kaynakları aynı ağda veya farklı aboneliklerde olabilir. VNet ve Azure hizmet kaynakları farklı aboneliklerdeyse, kaynakların aynı Active Directory (AD) kiracısı altında olması gerekir. Her iki aboneliğin de **Microsoft. SQL** kaynak sağlayıcısı 'nın kayıtlı olduğundan emin olun. Daha fazla bilgi için [Resource-Manager-kayıt][resource-manager-portal] bölümüne bakın

   > [!IMPORTANT]
   > Hizmet uç noktalarını yapılandırmadan önce hizmet uç noktası yapılandırmalarına ve noktalara ilişkin bu makalenin okunması kesinlikle önerilir. **Sanal ağ hizmeti uç noktası:** [Sanal ağ hizmeti uç noktası](../virtual-network/virtual-network-service-endpoints-overview.md) , özellik değerleri bir veya daha fazla resmi Azure hizmet türü adı içeren bir alt ağıdır. VNet Hizmetleri uç noktaları, SQL veritabanı adlı Azure hizmetine başvuran **Microsoft. SQL**hizmet türü adını kullanır. Bu hizmet etiketi, Azure SQL veritabanı, MariaDB için Azure veritabanı, PostgreSQL ve MySQL Hizmetleri için de geçerlidir. **Microsoft. SQL** hizmet etiketi bir sanal ağ hizmeti uç noktasına uygulandığında, Azure SQL veritabanı, PostgreSQL Için Azure veritabanı, MariaDB için Azure veritabanı ve alt ağdaki MySQL sunucuları Için Azure veritabanı gibi tüm Azure veritabanı hizmetleri için hizmet uç noktası trafiğini yapılandırır.
   > 

5. Etkinleştirildikten sonra **Tamam** ' a tıkladığınızda, VNET hizmet uç noktalarının sanal ağ kuralıyla birlikte etkinleştirildiğini görürsünüz.

   ![VNet hizmet uç noktaları etkin ve VNet kuralı oluşturuldu](./media/howto-manage-vnet-portal/3-vnet-service-endpoints-enabled-vnet-rule-created.png)

## <a name="next-steps"></a>Sonraki adımlar
- [MariaDB Için Azure veritabanı 'NDA SSL 'yi yapılandırma](howto-configure-ssl.md) hakkında daha fazla bilgi edinin
- Benzer şekilde, [VNET hizmet uç noktalarını etkinleştirmek ve Azure CLI kullanarak MariaDB Için Azure veritabanı için BIR VNET kuralı oluşturmak](howto-manage-vnet-cli.md)üzere komut dosyası oluşturabilirsiniz.

<!-- Link references, to text, Within this same GitHub repo. --> 
[resource-manager-portal]: ../azure-resource-manager/management/resource-providers-and-types.md