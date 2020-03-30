---
title: IaaS Agent Extension ile yönetim görevlerini otomatikleştirin
description: Bu makalede, belirli SQL Server yönetim görevlerini otomatikleştiren SQL Server IaaS Agent Extension nasıl yönetilir açıklanmaktadır. Bunlar arasında otomatik yedekleme, otomatik düzeltme ve Azure Anahtar Kasası tümleştirmesi yer almaktadır.
services: virtual-machines-windows
documentationcenter: ''
author: MashaMSFT
manager: jroth
editor: ''
tags: azure-resource-manager
ms.assetid: effe4e2f-35b5-490a-b5ef-b06746083da4
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 08/30/2019
ms.author: mathoma
ms.reviewer: jroth
ms.custom: seo-lt-2019
ms.openlocfilehash: 3d16c1950cbae0bcc7dd858e5520eb8bfc6e496d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77030787"
---
# <a name="automate-management-tasks-on-azure-virtual-machines-by-using-the-sql-server-iaas-agent-extension"></a>SQL Server IaaS Agent Extension'ı kullanarak Azure sanal makinelerinde yönetim görevlerini otomatikleştirin
> [!div class="op_single_selector"]
> * [Kaynak Yöneticisi](virtual-machines-windows-sql-server-agent-extension.md)
> * [Klasik](../sqlclassic/virtual-machines-windows-classic-sql-server-agent-extension.md)

SQL Server IaaS Aracı Uzantısı Extension (SqlIaasExtension) Azure sanal makinelerinde yönetim görevlerini otomatikleştirmek için çalıştırılır. Bu makalede, uzantının desteklediği hizmetlere genel bir bakış sağlar. Bu makalede, yükleme, durum ve uzantının kaldırılması için yönergeler de sağlar.

[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-rm-include.md)]

Bu makalenin klasik sürümünü görüntülemek [için, SQL Server VM'ler için SQL Server IaaS Agent Extension (klasik)](../sqlclassic/virtual-machines-windows-classic-sql-server-agent-extension.md)bölümüne bakın.


## <a name="supported-services"></a>Desteklenen hizmetler
SQL Server IaaS Aracı Uzantısı aşağıdaki yönetim görevlerini destekler:

| Yönetim özelliği | Açıklama |
| --- | --- |
| **SQL Server otomatik yedekleme** |Varsayılan örnek veya VM'de [düzgün yüklü](virtual-machines-windows-sql-server-iaas-faq.md#administration) bir SQL Server örneği için tüm veritabanları için yedekleme zamanlamasını otomatikleştirir. Daha fazla bilgi için Azure [sanal makinelerde SQL Server için Otomatik yedekleme (Kaynak Yöneticisi)](virtual-machines-windows-sql-automated-backup.md)bakın. |
| **SQL Server otomatik yama** |VM'nizdeki önemli Windows güncelleştirmelerinin gerçekleşebileceği bir bakım penceresi yapılandırArak iş yükünüz için yoğun saatlerde güncelleştirmelerden kaçınabilirsiniz. Daha fazla bilgi için Azure [sanal makinelerde SQL Server için Otomatik düzeltme eki (Kaynak Yöneticisi)](virtual-machines-windows-sql-automated-patching.md)bakın. |
| **Azure Anahtar Kasası entegrasyonu** |SQL Server VM'nize Azure Key Vault'u otomatik olarak yüklemenizi ve yapılandırmanızı sağlar. Daha fazla bilgi için bkz: [Azure Sanal Makinelerde SQL Server için Azure Anahtar Kasası tümleştirmesini yapılandırın (Kaynak Yöneticisi)](virtual-machines-windows-ps-sql-keyvault.md). |

SQL Server Iaas Agent Extension yüklendikten ve çalıştırıldıktan sonra yönetim özelliklerini kullanılabilir hale getirir:

* Azure portalındaki sanal makinenin SQL Server panelinde ve Azure Marketi'ndeki SQL Server görüntüleri için Azure PowerShell üzerinden.
* Uzantının manuel yüklemeleri için Azure PowerShell aracılığıyla. 

## <a name="prerequisites"></a>Ön koşullar
VM'nizde SQL Server IaaS Agent Extension'ı kullanma gereksinimleri şunlardır:

**İşletim sistemi**:

* Windows Server 2008 R2
* Windows Server 2012
* Windows Server 2012 R2
* Windows Server 2016
* Windows Server 2019 

**SQL Server sürümü**:

* SQL Server 2008 
* SQL Server 2008 R2
* SQL Server 2012
* SQL Server 2014
* SQL Server 2016
* SQL Server 2017
* SQL Server 2019

**Azure PowerShell**:

* [En son Azure PowerShell komutlarını indirin ve yapılandırır](/powershell/azure/overview)

[!INCLUDE [updated-for-az.md](../../../../includes/updated-for-az.md)]


##  <a name="installation"></a>Yükleme
SQL Server [VM'nizi SQL VM kaynak sağlayıcısına](virtual-machines-windows-sql-register-with-resource-provider.md)kaydettirdiğinizde SQL Server IaaS uzantısı yüklenir. Gerekirse, aşağıdaki PowerShell komutunu kullanarak SQL Server IaaS aracısını el ile yükleyebilirsiniz: 

  ```powershell-interactive
    Set-AzVMSqlServerExtension -VMName "sql2017" `
    -ResourceGroupName "LabsqlIAASagent" -Name "SQLIaasExtension" `
    -Version "2.0" -Location "Central US";  
  ```

> [!NOTE]
> Uzantıyı yüklemek SQL Server hizmetini yeniden başlatır. 


### <a name="install-on-a-vm-with-a-single-named-sql-server-instance"></a>SQL Server örneği adlı tek bir VM'ye yükleme
Varsayılan örnek kaldırılırsa ve IaaS uzantısı yeniden yüklenirse, SQL Server IaaS uzantısı SQL Server'da adlandırılmış bir örnekle çalışır.

SQL Server adlı bir örneğini kullanmak için aşağıdaki adımları izleyin:
   1. Azure Marketi'nden bir SQL Server VM dağıtın. 
   1. [Azure portalından](https://portal.azure.com)IaaS uzantısını kaldırın.
   1. SQL Server VM içinde SQL Server'ı tamamen kaldırın.
   1. SQL Server VM içinde adlandırılmış bir örnekle SQL Server'ı yükleyin. 
   1. Azure portalından IaaS uzantısını yükleyin.  


## <a name="get-the-status-of-the-sql-server-iaas-extension"></a>SQL Server IaaS uzantısı durumunu alın
Uzantın yüklü olduğunu doğrulamanın bir yolu, Azure portalındaki aracı durumunu görüntülemektir. Sanal makine **penceresindetüm ayarları** seçin ve ardından **Uzantılar'ı**seçin. Listelenen **SqlIaasExtension** uzantısını görmeniz gerekir.

![Azure portalındaki SQL Server IaaS Aracı Uzantısı'nın durumu](./media/virtual-machines-windows-sql-server-agent-extension/azure-rm-sql-server-iaas-agent-portal.png)

**Get-AzVMSqlServerExtension** Azure PowerShell cmdlet'ini de kullanabilirsiniz:

   ```powershell-interactive
   Get-AzVMSqlServerExtension -VMName "vmname" -ResourceGroupName "resourcegroupname"
   ```

Önceki komut aracının yüklü olduğunu onaylar ve genel durum bilgilerini sağlar. Aşağıdaki komutları kullanarak otomatik yedekleme ve yama hakkında belirli durum bilgilerini alabilirsiniz:

   ```powershell-interactive
    $sqlext = Get-AzVMSqlServerExtension -VMName "vmname" -ResourceGroupName "resourcegroupname"
    $sqlext.AutoPatchingSettings
    $sqlext.AutoBackupSettings
   ```

## <a name="removal"></a>Kaldırma
Azure portalında, sanal makine özelliklerinizin **Uzantılar** penceresindeki elipsleri seçerek uzantıyı kaldırabilirsiniz. Ardından **Sil**’i seçin.

![Azure portalında SQL Server IaaS Aracı Uzantısını Kaldırma](./media/virtual-machines-windows-sql-server-agent-extension/azure-rm-sql-server-iaas-agent-uninstall.png)

**Ayrıca Remove-AzVMSqlServerExtension** PowerShell cmdlet kullanabilirsiniz:

   ```powershell-interactive
    Remove-AzVMSqlServerExtension -ResourceGroupName "resourcegroupname" -VMName "vmname" -Name "SqlIaasExtension"
   ```

## <a name="next-steps"></a>Sonraki adımlar
Uzantının desteklediği hizmetlerden birini kullanmaya başlayın. Daha fazla bilgi için, bu makalenin [Desteklenen Hizmetler](#supported-services) bölümünde başvurulan makalelere bakın.

Azure Sanal Makinelerde SQL Server'ı çalıştırma hakkında daha fazla bilgi için [Azure Sanal Makinelerde SQL Server nedir?](virtual-machines-windows-sql-server-iaas-overview.md)
