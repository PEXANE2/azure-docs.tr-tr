---
title: SQL VM'lerde yönetim görevlerini otomatikleştirin (Klasik) | Microsoft Dokümanlar
description: Bu konu, belirli SQL Server yönetim görevlerini otomatikleştiren SQL Server aracı uzantısını nasıl yönetileştirileştirileştirilebildiğini açıklar. Bunlar arasında Otomatik Yedekleme, Otomatik Yama ve Azure Anahtar Kasası Tümleştirmesi yer almaktadır. Bu konu klasik dağıtım modunu kullanır.
services: virtual-machines-windows
documentationcenter: ''
author: MashaMSFT
manager: craigg
editor: ''
tags: azure-service-management
ms.assetid: a9bda2e7-cdba-427c-bc30-77cde4376f3a
ms.service: virtual-machines-sql
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 07/12/2018
ms.author: mathoma
ms.reviewer: jroth
ms.openlocfilehash: b76ade40db1e85abc0fb42af2e6f4ab88cb092c4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75982276"
---
# <a name="automate-management-tasks-on-azure-virtual-machines-with-the-sql-server-agent-extension-classic"></a>SQL Server Agent Extension (Classic) ile Azure Sanal Makinelerde yönetim görevlerini otomatikleştirin
> [!div class="op_single_selector"]
> * [Kaynak Yöneticisi](../sql/virtual-machines-windows-sql-server-agent-extension.md)
> * [Klasik](../classic/sql-server-agent-extension.md)
> 
>
 
SQL Server IaaS Agent Extension (SQLIaaSAgent), yönetim görevlerini otomatikleştirmek için Azure sanal makinelerinde çalışır. Bu konu, uzantı tarafından desteklenen hizmetlerin yanı sıra yükleme, durum ve kaldırma talimatlarına genel bir bakış sağlar.

> [!IMPORTANT] 
> Azure'un kaynakları oluşturmak ve onlarla çalışmak için iki farklı dağıtım modeli vardır: [Kaynak Yöneticisi ve Klasik.](../../../azure-resource-manager/management/deployment-models.md) Bu makalede, Klasik dağıtım modeli kullanılarak kapsar. Microsoft, yeni dağıtımların çoğunun Resource Manager modelini kullanmasını önerir. Bu makalenin Kaynak Yöneticisi sürümünü görüntülemek için SQL [Server VMs Kaynak Yöneticisi için SQL Server Agent Extension bölümüne](../sql/virtual-machines-windows-sql-server-agent-extension.md)bakın.

## <a name="supported-services"></a>Desteklenen hizmetler
SQL Server IaaS Aracı Uzantısı aşağıdaki yönetim görevlerini destekler:

| Yönetim özelliği | Açıklama |
| --- | --- |
| **SQL Otomatik Yedekleme** |VM'deki SQL Server'ın varsayılan örneği için tüm veritabanlarının yedekleme zamanlamasını otomatikleştirir. Daha fazla bilgi [için Azure Sanal Makinelerde SQL Server için Otomatik yedekleme (Klasik)](../classic/sql-automated-backup.md)adresine bakın. |
| **SQL Otomatik Düzeltme Eki Uygulama** |VM'nizdeki önemli Windows güncelleştirmelerinin gerçekleşebileceği bir bakım penceresi yapılandırArak iş yükünüz için yoğun saatlerde güncelleştirmelerden kaçınabilirsiniz. Daha fazla bilgi [için Azure Sanal Makinelerde SQL Server için Otomatik düzeltme eki (Klasik)](../classic/sql-automated-patching.md)adresine bakın. |
| **Azure Anahtar Kasası Tümleştirme** |SQL Server VM'nize Azure Key Vault'u otomatik olarak yüklemenizi ve yapılandırmanızı sağlar. Daha fazla bilgi için bkz: [Azure VM'lerde SQL Server için Azure Anahtar Kasası Tümleştirmesini Yapılandırın (Klasik)](../classic/ps-sql-keyvault.md). |

## <a name="prerequisites"></a>Ön koşullar
VM'nizde SQL Server IaaS Aracı Uzantısı'nı kullanma gereksinimleri:

### <a name="operating-system"></a>İşletim Sistemi:
* Windows Server 2012
* Windows Server 2012 R2
* Windows Server 2016

### <a name="sql-server-versions"></a>SQL Server sürümleri:
* SQL Server 2012
* SQL Server 2014
* SQL Server 2016

### <a name="azure-powershell"></a>Azure PowerShell:
[En son Azure PowerShell komutlarını indirin ve yapılandırıyor.](/powershell/azure/overview)

Windows PowerShell'i başlatın ve Azure Aboneliğinize **Ekle-Azure Hesabı** komutuyla bağlayın.

    Add-AzureAccount

Birden çok aboneliğiniz varsa, hedef klasik VM'nizi içeren aboneliği seçmek için **Select-AzureSubscription'ı** kullanın.

    Select-AzureSubscription -SubscriptionName <subscriptionname>

Bu noktada, **Get-AzureVM** komutuyla klasik sanal makinelerin ve bunların ilişkili hizmet adlarının bir listesini alabilirsiniz.

    Get-AzureVM

## <a name="installation"></a>Yükleme
Klasik VM'ler için, SQL Server IaaS Agent Extension'ı yüklemek ve ilişkili hizmetlerini yapılandırmak için PowerShell'i kullanmanız gerekir. Uzantıyı yüklemek için **Set-AzureVMSqlSqlExtension** PowerShell cmdlet'i kullanın. Örneğin, aşağıdaki komut uzantıyı Bir Windows Server VM'ye (klasik) yükler ve "SQLIaaSExtension" olarak adlandırır.

    Get-AzureVM -ServiceName <vmservicename> -Name <vmname> | Set-AzureVMSqlServerExtension -ReferenceName "SQLIaasExtension" -Version "1.2" | Update-AzureVM

SQL IaaS Agent Extension'ın en son sürümüne güncellerseniz, uzantıyı güncelledikten sonra sanal makinenizi yeniden başlatmanız gerekir.

> [!NOTE]
> Klasik sanal makinelerin SQL IaaS Agent Extension'ı portal üzerinden yükleme ve yapılandırma seçeneği yoktur.

> [!NOTE]
> SQL Server IaaS Agent Extension yalnızca [SQL Server VM galeri görüntülerinde](../sql/virtual-machines-windows-sql-server-iaas-overview.md#get-started-with-sql-vms) (giderseniz öde veya kendi lisansınızı getir) desteklenir. SQL Server'ı yalnızca işletim sistemi yle ilgili bir Windows Server sanal makinesine el ile yüklerseniz veya özelleştirilmiş bir SQL Server VM VHD dağıtırsanız desteklenmez. Bu gibi durumlarda, PowerShell kullanarak uzantıyı el ile yüklemek ve yönetmek mümkün olabilir, ancak bunun yerine bir SQL Server VM galeri görüntüsü yüklemeniz ve ardından özelleştirmeniz önerilir.

## <a name="status"></a>Durum
Uzantın yüklü olduğunu doğrulamanın bir yolu, Azure Portalı'ndaki aracı durumunu görüntülemektir. Sanal makine bıçağında listelenen sanal bir makine seçin ve **ardından Uzantılar'ı**tıklatın. Listelenen **SQLIaaSAgent** uzantısını görmeniz gerekir.

![Azure Portalında SQL Server IaaS Aracı Uzantısı](./media/virtual-machines-windows-classic-sql-server-agent-extension/azure-sql-server-iaas-agent-portal.png)

**Get-AzureVMSqlServerExtension** Azure Powershell cmdlet'ini de kullanabilirsiniz.

    Get-AzureVM –ServiceName "service" –Name "vmname" | Get-AzureVMSqlServerExtension

## <a name="removal"></a>Kaldırma
Azure Portalı'nda, sanal makine özelliklerinizin **Uzantılar** bıçağındaki elipsleri tıklatarak uzantıyı kaldırabilirsiniz. Ardından **Kaldır'ı**tıklatın.

![Azure Portalı'ndaki SQL Server IaaS Aracı Uzantısını Kaldırın](./media/virtual-machines-windows-classic-sql-server-agent-extension/azure-sql-server-iaas-agent-uninstall.png)

**Remove-AzureVMSqlServerExtension** Powershell cmdlet'i de kullanabilirsiniz.

    Get-AzureVM –ServiceName "service" –Name "vmname" | Remove-AzureVMSqlServerExtension | Update-AzureVM

## <a name="next-steps"></a>Sonraki Adımlar
Uzantıya tarafından desteklenen hizmetlerden birini kullanmaya başlatın. Daha fazla ayrıntı için, bu makalenin [Desteklenen Hizmetler](#supported-services) bölümünde başvurulan konulara bakın.

Azure Sanal Makinelerde SQL Server'ı çalıştırma hakkında daha fazla bilgi için [Azure Sanal Makineler'de SQL Server'a genel bakış](../sql/virtual-machines-windows-sql-server-iaas-overview.md)bakın.

