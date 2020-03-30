---
title: Azure portalını kullanarak Azure'da SQL Server VM'leri yönetme | Microsoft Dokümanlar
description: Azure'da barındırılan bir SQL Server VM için Azure portalındaki SQL sanal makine kaynağına nasıl erişin.
services: virtual-machines-windows
documentationcenter: na
author: MashaMSFT
manager: craigg
tags: azure-resource-manager
ms.service: virtual-machines-sql
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 05/13/2019
ms.author: mathoma
ms.reviewer: jroth
ms.openlocfilehash: 244ad7d079fd7baf25f8079557576c42d25ca785
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79243217"
---
# <a name="manage-sql-server-vms-in-azure-by-using-the-azure-portal"></a>Azure portalını kullanarak Azure'da SQL Server VM'leri yönetme

Azure [portalında,](https://portal.azure.com) **SQL sanal makineler** kaynağı bağımsız bir yönetim hizmetidir. Tüm SQL Server VM'lerinizi aynı anda görüntülemek ve SQL Server'a adanmış ayarları değiştirmek için kullanabilirsiniz: 

![SQL sanal makineler kaynak](media/virtual-machines-windows-sql-manage-portal/sql-vm-manage.png)


## <a name="remarks"></a>Açıklamalar

- SQL Server VM'lerinizi Azure'da görüntülemek ve yönetmek için **SQL sanal makineler** kaynağını kullanmanızı öneririz. Ancak şu anda, **SQL sanal makineler** kaynağı destek [sonu](virtual-machines-windows-sql-server-2008-eos-extend-support.md) SQL Server VM yönetimini desteklemez. Destek sonu SQL Server VM'lerinizin ayarlarını yönetmek için, bunun yerine amortismana kalanmış [SQL Server yapılandırma sekmesini](#access-the-sql-server-configuration-tab) kullanın. 
- **SQL sanal makineler** kaynağı yalnızca SQL VM kaynak sağlayıcısına kayıtlı olan SQL Server [VM'ler](virtual-machines-windows-sql-register-with-resource-provider.md)tarafından kullanılabilir. 


## <a name="access-the-sql-virtual-machines-resource"></a>SQL sanal makineler kaynağına erişin
SQL sanal makineler kaynağına erişmek için **aşağıdakileri** yapın:

1. Azure [portalını](https://portal.azure.com)açın. 
1. **Tüm Hizmetleri**Seçin. 
1. Arama kutusuna **SQL sanal makineleri** girin.
1. (İsteğe bağlı): Bu seçeneği **Sık Kullanılanlar** menüsüne eklemek için **SQL sanal makinelerinyanındaki** yıldızı seçin. 
1. **SQL sanal makineleri**seçin. 

   ![Tüm hizmetlerde SQL Server sanal makinelerini bulun](media/virtual-machines-windows-sql-manage-portal/sql-vm-search.png)

1. Portal, abonelik içinde bulunan tüm SQL Server VM'leri listeler. **SQL sanal makineler** kaynağını açmak için yönetmek istediğinizi seçin. SQL Server VM'niz görünmüyorsa arama kutusunu kullanın. 

   ![Tüm mevcut SQL Server VMs](media/virtual-machines-windows-sql-manage-portal/all-sql-vms.png)

   SQL Server VM'nizi **seçmek, SQL sanal makineler** kaynağını açar: 


   ![SQL sanal makineler kaynak](media/virtual-machines-windows-sql-manage-portal/sql-vm-resource.png)

> [!TIP]
> **SQL sanal makineler** kaynağı özel SQL Server ayarları içindir. VM'ye özgü ancak SQL Server'a özel olmayan ayarları açmak için **Sanal makine** kutusundaki VM'nin adını seçin. 

## <a name="access-the-sql-server-configuration-tab"></a>SQL Server yapılandırma sekmesine erişin
**SQL Server yapılandırma** sekmesi amortismana kaldırıldı. Şu anda, [destek sonu](virtual-machines-windows-sql-server-2008-eos-extend-support.md) SQL Server VM'leri ve [SQL VM kaynak sağlayıcısına kayıtlı](virtual-machines-windows-sql-register-with-resource-provider.md)olmayan SQL Server VM'leri yönetmek için tek yöntemdir.

Amortismana yenik sql **server yapılandırma** sekmesine erişmek için Sanal **makineler** kaynağına gidin. Aşağıdaki adımları kullanın:

1. Azure [portalını](https://portal.azure.com)açın. 
1. **Tüm Hizmetleri**Seçin. 
1. Arama kutusuna **sanal makineleri** girin.
1. (İsteğe bağlı): Bu seçeneği **Sık Kullanılanlar** menüsüne eklemek için **Sanal makinelerin** yanındaki yıldızı seçin. 
1. **Sanal makineler**'i seçin. 

   ![Sanal makineleri ara](media/virtual-machines-windows-sql-manage-portal/vm-search.png)

1. Portal, abonelikteki tüm sanal makineleri listeler. **Sanal makineler** kaynağını açmak için yönetmek istediğinizi seçin. SQL Server VM'niz görünmüyorsa arama kutusunu kullanın. 
1. SQL Server VM'nizi yönetmek için **Ayarlar** bölmesinde **SQL Server yapılandırmasını** seçin. 

   ![SQL Server yapılandırması](media/virtual-machines-windows-sql-manage-portal/sql-vm-configuration.png)

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi için aşağıdaki makalelere bakın: 

* [Windows VM'de SQL Server'a Genel Bakış](virtual-machines-windows-sql-server-iaas-overview.md)
* [Windows VM'de SQL Server için SSS](virtual-machines-windows-sql-server-iaas-faq.md)
* [Windows VM'de SQL Server için fiyatlandırma kılavuzu](virtual-machines-windows-sql-server-pricing-guidance.md)
* [Windows VM'de SQL Server için sürüm notları](virtual-machines-windows-sql-server-iaas-release-notes.md)


