---
title: Yönetilen örneğine göre sanal ağ oluşturma
description: Bu makalede, Azure SQL Veritabanı Yönetilen Örneği'ni dağıtabileceğiniz sanal ağ nasıl oluşturulacağı açıklanmaktadır.
services: sql-database
ms.service: sql-database
ms.subservice: managed-instance
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: srdan-bozovic-msft
ms.author: srbozovi
ms.reviewer: sstein, bonova, carlrab
ms.date: 09/12/2019
ms.openlocfilehash: 9f115d51657993562642391a235de79420aa434a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73823372"
---
# <a name="create-a-virtual-network-for-azure-sql-database-managed-instance"></a>Azure SQL Veritabanı Yönetilen Örneği için sanal ağ oluşturma

Bu makalede, Azure SQL Veritabanı Yönetilen Örnek dağıtabilirsiniz geçerli bir sanal ağ ve alt ağ oluşturmak için nasıl açıklanmaktadır.

Azure SQL Veritabanı Yönetilen Örnek, bir Azure [sanal ağında](../virtual-network/virtual-networks-overview.md)dağıtılmalıdır. Bu dağıtım aşağıdaki senaryoları sağlar:

- Güvenli özel IP adresi
- Yönetilen Örneğe doğrudan şirket içi ağdan bağlanma
- Yönetilen Örnek'i bağlantılı sunucuya veya başka bir şirket içi veri deposuna bağlama
- Yönetilen Örnek'i Azure kaynaklarına bağlama  

> [!Note]
> İlk örneği dağıtmadan önce [Yönetilen Örnek için alt netboyutunu belirlemeniz](sql-database-managed-instance-determine-size-vnet-subnet.md) gerekir. Kaynakları içine koyduktan sonra alt ağı yeniden boyutlandıramaz.
>
> Varolan bir sanal ağı kullanmayı planlıyorsanız, yönetilen örneğinizi barındıracak şekilde bu ağ yapılandırmasını değiştirmeniz gerekir. Daha fazla bilgi için bkz. [Yönetilen Örnek için varolan bir sanal ağı değiştir.](sql-database-managed-instance-configure-vnet-subnet.md)
>
> Yönetilen bir örnek oluşturulduktan sonra, yönetilen örneğin veya VNet'in başka bir kaynak grubuna taşınması veya aboneliği desteklenmez.


## <a name="create-a-virtual-network"></a>Sanal ağ oluşturma

Sanal ağ oluşturmanın ve yapılandırmanın en kolay yolu, Azure Kaynak Yöneticisi dağıtım şablonu kullanmaktır.

1. Azure Portal’da oturum açın.

2. **Azure'a Dağıt** düğmesini seçin:

   <a target="_blank" href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-sql-managed-instance-azure-environment%2Fazuredeploy.json" rel="noopener" data-linktype="external"> <img src="https://azuredeploy.net/deploybutton.png" data-linktype="external"> </a>

   Bu düğme, Yönetilen Örnek dağıtabilirsiniz ağ ortamı yapılandırmak için kullanabileceğiniz bir form açar.

   > [!Note]
   > Bu Azure Kaynak Yöneticisi şablonu, iki alt ağ içeren bir sanal ağ dağıtacaktır. **Yönetilen Örnekler**adlı bir alt ağ Yönetilen Örnek için ayrılmıştır ve önceden yapılandırılmış bir rota tablosu vardır. **Varsayılan**olarak adlandırılan diğer alt ağ, Yönetilen Örnek'e (örneğin, Azure Sanal Makineleri) erişmesi gereken diğer kaynaklar için kullanılır.

3. Ağ ortamını yapılandırın. Aşağıdaki formda, ağ ortamınızın parametrelerini yapılandırabilirsiniz:

   ![Azure ağını yapılandırmak için Kaynak Yöneticisi şablonu](./media/sql-database-managed-instance-vnet-configuration/create-mi-network-arm.png)

   Sanal ağ ve alt ağların adlarını değiştirebilir ve ağ kaynaklarınız ile ilişkili IP aralıklarını ayarlayabilirsiniz. **Satın Alma** düğmesini seçtikten sonra, bu form ortamınızı oluşturur ve yapılandıracaktır. İki alt ağ gerekmezse, varsayılan ağı silebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

- Genel bakış için [bkz.](sql-database-managed-instance.md)
- Yönetilen [Örnek'te bağlantı mimarisi](sql-database-managed-instance-connectivity-architecture.md)hakkında bilgi edinin.
- [Yönetilen Örnek için varolan bir sanal ağı](sql-database-managed-instance-configure-vnet-subnet.md)nasıl değiştirbekleyeceğizi öğrenin.
- Sanal ağ oluşturmayı, Yönetilen Örnek'i nasıl oluşturup veritabanı yedeklemesinden veritabanını geri yükleyin, [bkz.](sql-database-managed-instance-get-started.md)
- DNS sorunları için [bkz.](sql-database-managed-instance-custom-dns.md)
