---
title: SQL Server Sanal Makineler için Otomatik Yedekleme (Klasik) | Microsoft Dokümanlar
description: "Kaynak Yöneticisi'ni kullanarak Azure Sanal Makinelerde çalışan SQL Server için Otomatik Yedekleme özelliğini açıklar. "
services: virtual-machines-windows
documentationcenter: na
author: MashaMSFT
manager: craigg
editor: ''
tags: azure-service-management
ms.assetid: 3333e830-8a60-42f5-9f44-8e02e9868d7b
ms.service: virtual-machines-sql
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 01/23/2018
ms.author: mathoma
ms.reviewer: jroth
ms.openlocfilehash: 43ff230d4769a23c9007b3da29858d2105366f9f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75978091"
---
# <a name="automated-backup-for-sql-server-in-azure-virtual-machines-classic"></a>Azure Sanal Makinelerde SQL Server için Otomatik Yedekleme (Klasik)
> [!div class="op_single_selector"]
> * [Kaynak Yöneticisi](../sql/virtual-machines-windows-sql-automated-backup.md)
> * [Klasik](../classic/sql-automated-backup.md)
> 
> 

Otomatik Yedekleme, SQL Server 2014 Standard veya Enterprise çalıştıran bir Azure VM'deki mevcut ve yeni veritabanları için [Yönetilen Yedekleme'yi Microsoft Azure'a](https://msdn.microsoft.com/library/dn449496.aspx) otomatik olarak yapılandırır. Bu, dayanıklı Azure blob depolama kullanan düzenli veritabanı yedeklemelerini yapılandırmanızı sağlar. Otomatik [Yedekleme, SQL Server IaaS Aracı Uzantısı'na](../classic/sql-server-agent-extension.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)bağlıdır.

> [!IMPORTANT] 
> Azure'un kaynakları oluşturmak ve onlarla çalışmak için iki farklı dağıtım modeli vardır: [Kaynak Yöneticisi ve Klasik.](../../../azure-resource-manager/management/deployment-models.md) Bu makalede, Klasik dağıtım modeli kullanılarak kapsar. Microsoft, yeni dağıtımların çoğunun Resource Manager modelini kullanmasını önerir. Bu makalenin Kaynak Yöneticisi sürümünü görüntülemek [için Azure Sanal Makineler Kaynak Yöneticisi'nde SQL Server için Otomatik Yedekleme bölümüne](../sql/virtual-machines-windows-sql-automated-backup.md)bakın.

## <a name="prerequisites"></a>Ön koşullar
Otomatik Yedekleme'yi kullanmak için aşağıdaki ön koşulları göz önünde bulundurun:

**İşletim Sistemi**:

* Windows Server 2012
* Windows Server 2012 R2
* Windows Server 2016

**SQL Server sürümü/sürümü**:

* SQL Server 2014 Standardı
* SQL Server 2014 Kurumsal

> [!NOTE]
> SQL Server 2016 için Otomatik Yedekleme, Resource Manager sanal makineleri ile desteklenir. Daha fazla bilgi için [SQL Server 2016 Azure Sanal Makineler (Kaynak Yöneticisi) için Otomatik Yedekleme v2'ye](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-automated-backup-v2)bakın.

**Veritabanı yapılandırması**:

* Hedef veritabanları tam kurtarma modelini kullanmalıdır.

**Azure PowerShell**:

* [En son Azure PowerShell komutlarını yükleyin.](/powershell/azure/overview)

**SQL Server IaaS Uzantısı**:

* [SQL Server IaaS Uzantısı'nı yükleyin.](../classic/sql-server-agent-extension.md)

## <a name="settings"></a>Ayarlar
Aşağıdaki tabloda Otomatik Yedekleme için yapılandırılabilen seçenekler açıklanmaktadır. Klasik VM'ler için bu ayarları yapılandırmak için PowerShell'i kullanmanız gerekir.

| Ayar | Aralık (Varsayılan) | Açıklama |
| --- | --- | --- |
| **Otomatik Yedekleme** |Etkinleştirme/Devre Dışı Bırakma (Devre Dışı) |SQL Server 2014 Standard veya Enterprise çalıştıran bir Azure VM için Otomatik Yedekleme'yi etkinleştiriler veya devre dışı kılabilir. |
| **Bekletme Süresi** |1-30 gün (30 gün) |Yedeklemeyi tutmak için gün sayısı. |
| **Depolama Hesabı** |Azure depolama hesabı (belirtilen VM için oluşturulan depolama hesabı) |Otomatik Yedekleme dosyalarını blob depolamada depolamak için kullanılacak bir Azure depolama hesabı. Tüm yedekleme dosyalarını depolamak için bu konumda bir kapsayıcı oluşturulur. Yedekleme dosyası adlandırma kuralı tarih, saat ve makine adını içerir. |
| **Şifreleme** |Etkinleştirme/Devre Dışı Bırakma (Devre Dışı) |Şifrelemeyi etkinleştirir veya devre dışı kılabilir. Şifreleme etkinleştirildiğinde, yedeklemeyi geri yüklemek için kullanılan sertifikalar, aynı adlandırma kuralını kullanarak aynı otomatik yedekleme kapsayıcısında belirtilen depolama hesabında bulunur. Parola değişirse, bu parolayla yeni bir sertifika oluşturulur, ancak önceki yedeklemeleri geri yüklemek için eski sertifika kalır. |
| **Parola** |Parola metni (Yok) |Şifreleme anahtarları için bir şifre. Bu yalnızca şifreleme etkinse gereklidir. Şifreli bir yedeklemeyi geri yüklemek için, yedeklemenin alındığı sırada kullanılan doğru parolaya ve ilgili sertifikaya sahip olmalısınız. |
| **Backup sistem veritabanları** | Etkinleştirme/Devre Dışı Bırakma (Devre Dışı) | Master, Model ve MSDB'nin tam yedeklerini alın |
| **Yedekleme zamanlamasını yapılandırma** | Manuel/Otomatik (Otomatik) | Günlük büyümesine bağlı olarak yedeklemeleri otomatik olarak tam olarak almak ve yedeklemeleri günlüğe kaydetmek için **Otomatik'i** seçin. Tam ve günlük yedeklemelerinin zamanlamasını belirtmek için **El Kitabı'nı** seçin. |

## <a name="configuration-with-powershell"></a>PowerShell ile Yapılandırma
Aşağıdaki PowerShell örneğinde, Otomatik Yedekleme varolan bir SQL Server 2014 VM için yapılandırılmıştır. **Yeni AzureVMSqlSqlAutoBackupConfig** komutu, yedeklemeleri $storageaccount değişkentarafından belirtilen Azure depolama hesabında depolamak için Otomatik Yedekleme ayarlarını yapılandırır. Bu yedeklemeler 10 gün boyunca saklanır. **Set-AzureVMSqlServerExtension** komutu belirtilen Azure VM'yi bu ayarlarla güncelleştirir.

    $storageaccount = "<storageaccountname>"
    $storageaccountkey = (Get-AzureStorageKey -StorageAccountName $storageaccount).Primary
    $storagecontext = New-AzureStorageContext -StorageAccountName $storageaccount -StorageAccountKey $storageaccountkey
    $autobackupconfig = New-AzureVMSqlServerAutoBackupConfig -StorageContext $storagecontext -Enable -RetentionPeriod 10

    Get-AzureVM -ServiceName <vmservicename> -Name <vmname> | Set-AzureVMSqlServerExtension -AutoBackupSettings $autobackupconfig | Update-AzureVM

SQL Server IaaS Aracısını yüklemek ve yapılandırmak birkaç dakika sürebilir.

Şifrelemeyi etkinleştirmek için, Önceki komut dosyasını değiştirerek SertifikaParola parametresi için bir parola (güvenli dize) ile birlikte Şifrelemeyi Etkinleştirme parametresini geçirin. Aşağıdaki komut dosyası, önceki örnekte Otomatik Yedekleme ayarlarını etkinleştiriyor ve şifreleme ekler.

    $storageaccount = "<storageaccountname>"
    $storageaccountkey = (Get-AzureStorageKey -StorageAccountName $storageaccount).Primary
    $storagecontext = New-AzureStorageContext -StorageAccountName $storageaccount -StorageAccountKey $storageaccountkey
    $password = "P@ssw0rd"
    $encryptionpassword = $password | ConvertTo-SecureString -AsPlainText -Force  
    $autobackupconfig = New-AzureVMSqlServerAutoBackupConfig -StorageContext $storagecontext -Enable -RetentionPeriod 10 -EnableEncryption -CertificatePassword $encryptionpassword

    Get-AzureVM -ServiceName <vmservicename> -Name <vmname> | Set-AzureVMSqlServerExtension -AutoBackupSettings $autobackupconfig | Update-AzureVM

Otomatik yedeklemeyi devre dışı kalmak için, **Yeni AzureVMSqlSqlAutoBackupConfig**için **-Enable** parametresi olmadan aynı komut dosyasını çalıştırın. Yüklemede olduğu gibi, Otomatik Yedekleme'yi devre dışı bırakın birkaç dakika sürebilir.

> [!NOTE]
> SQL Server IaaS Aracısını devre dışı bırakmak ve kaldırmak önceden yapılandırılmış Yönetilen Yedekleme ayarlarını kaldırmaz. SQL Server IaaS Aracısını devre dışı bırakmadan veya kaldırmadan önce Otomatik Yedekleme'yi devre dışı bırakmalısınız.
> 
> 

## <a name="next-steps"></a>Sonraki adımlar
Otomatik Yedekleme, Azure VM'lerinde Yönetilen Yedekleme'yi yapılandırır. Bu nedenle, davranışı ve sonuçlarını anlamak [için Yönetilen Yedekleme için belgeleri gözden geçirmek](https://msdn.microsoft.com/library/dn449496.aspx) önemlidir.

Azure Sanal M'lerde SQL Server için ek yedekleme ve geri yükleme kılavuzunu aşağıdaki başlıkta bulabilirsiniz: [Azure Sanal Makinelerde SQL Server için Yedekleme ve Geri Yükleme.](../sql/virtual-machines-windows-sql-backup-recovery.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fsqlclassic%2ftoc.json)

Mevcut diğer otomasyon görevleri hakkında daha fazla bilgi için [SQL Server IaaS Agent Extension'a](../classic/sql-server-agent-extension.md)bakın.

Azure VM'lerde SQL Server'ı çalıştırma hakkında daha fazla bilgi için [Azure Sanal Makinelere genel bakışta SQL Server'a](../sql/virtual-machines-windows-sql-server-iaas-overview.md)bakın.

