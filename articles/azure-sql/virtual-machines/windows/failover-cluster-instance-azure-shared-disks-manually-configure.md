---
title: Azure Paylaşılan disklerle bir FCı oluşturma (Önizleme)
description: Azure sanal makinelerinde SQL Server bir yük devretme kümesi örneği (FCı) oluşturmak için Azure Paylaşılan diskleri kullanın.
services: virtual-machines
documentationCenter: na
author: MashaMSFT
editor: monicar
tags: azure-service-management
ms.service: virtual-machines-sql
ms.custom: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 06/26/2020
ms.author: mathoma
ms.openlocfilehash: ffb739affac68898f6ed5ff1d972d3fd4a70df2f
ms.sourcegitcommit: 420c30c760caf5742ba2e71f18cfd7649d1ead8a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/28/2020
ms.locfileid: "89055269"
---
# <a name="create-an-fci-with-azure-shared-disks-sql-server-on-azure-vms"></a>Azure Paylaşılan disklerle (Azure VM 'lerinde SQL Server) bir FCı oluşturma
[!INCLUDE[appliesto-sqlvm](../../includes/appliesto-sqlvm.md)]

Bu makalede, Azure sanal makinelerinde (VM) SQL Server ile Azure paylaşılan diskler kullanılarak yük devretme kümesi örneği (FCı) oluşturma işlemi açıklanmaktadır. 

Daha fazla bilgi edinmek için bkz. Azure VM 'lerde ve [küme en iyi uygulamalarında](hadr-cluster-best-practices.md) [SQL Server ile FCI](failover-cluster-instance-overview.md) 'ye genel bakış. 


## <a name="prerequisites"></a>Ön koşullar 

Bu makaledeki yönergeleri tamamlamadan önce Şu durumda olmalıdır:

- Azure aboneliği. [Ücretsiz](https://azure.microsoft.com/free/)olarak kullanmaya başlayın. 
- [İki veya daha fazla Orta Batı ABD-](failover-cluster-instance-prepare-vm.md) aynı [kullanılabilirlik kümesindeki](../../../virtual-machines/linux/tutorial-availability-sets.md) Windows Azure sanal makineleri ve bir [yakınlık yerleşimi grubu](../../../virtual-machines/windows/co-location.md#proximity-placement-groups), hata etki alanı ile oluşturulan ve etki alanı **1**olarak ayarlanmış olan kullanılabilirlik kümesi. 
- Hem Azure sanal makinelerinde hem de Active Directory nesne oluşturma izinlerine sahip olan bir hesap.
- En son [PowerShell](/powershell/azure/install-az-ps?view=azps-4.2.0)sürümü. 


## <a name="add-azure-shared-disk"></a>Azure Paylaşılan diski ekleme
Paylaşılan disk özelliği etkinken yönetilen bir Premium SSD diski dağıtın. `maxShares`Diski her IKI FCı düğümünde paylaşılabilir hale getirmek için **2** olarak ayarlayın. 

Aşağıdakileri yaparak bir Azure Paylaşılan diski ekleyin: 


1. Aşağıdaki betiği *SharedDiskConfig.js*olarak kaydedin: 

   ```JSON
   { 
     "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
     "contentVersion": "1.0.0.0",
     "parameters": {
       "dataDiskName": {
         "type": "string",
         "defaultValue": "mySharedDisk"
       },
       "dataDiskSizeGB": {
         "type": "int",
         "defaultValue": 1024
       },
       "maxShares": {
         "type": "int",
         "defaultValue": 2
       }
     },
     "resources": [
       {
         "type": "Microsoft.Compute/disks",
         "name": "[parameters('dataDiskName')]",
         "location": "[resourceGroup().location]",
         "apiVersion": "2019-07-01",
         "sku": {
           "name": "Premium_LRS"
         },
         "properties": {
           "creationData": {
             "createOption": "Empty"
           },
           "diskSizeGB": "[parameters('dataDiskSizeGB')]",
           "maxShares": "[parameters('maxShares')]"
         }
       }
     ] 
   }
   ```


2. PowerShell kullanarak * üzerindeSharedDiskConfig.js* çalıştırın: 

   ```powershell
   $rgName = < specify your resource group name>
       $location = 'westcentralus'
       New-AzResourceGroupDeployment -ResourceGroupName $rgName `
   -TemplateFile "SharedDiskConfig.json"
   ```

3. Her VM için, ekli Paylaşılan diskleri GUID bölümleme tablosu (GPT) olarak başlatın ve bu komutu çalıştırarak onları yeni teknoloji dosya sistemi (NTFS) olarak biçimlendirin: 

   ```powershell
   $resourceGroup = "<your resource group name>"
       $location = "<region of your shared disk>"
       $ppgName = "<your proximity placement groups name>"
       $vm = Get-AzVM -ResourceGroupName "<your resource group name>" `
           -Name "<your VM node name>"
       $dataDisk = Get-AzDisk -ResourceGroupName $resourceGroup `
           -DiskName "<your shared disk name>"
       $vm = Add-AzVMDataDisk -VM $vm -Name "<your shared disk name>" `
           -CreateOption Attach -ManagedDiskId $dataDisk.Id `
           -Lun <available LUN  check disk setting of the VM>
    update-AzVm -VM $vm -ResourceGroupName $resourceGroup
   ```


## <a name="create-failover-cluster"></a>Yük devretme kümesi oluşturma

Yük devretme kümesini oluşturmak için şunlar gerekir:

- Küme düğümleri olacak sanal makinelerin adları.
- Yük devretme kümesi için bir ad.
- Yük devretme kümesi için bir IP adresi. Küme düğümleri ile aynı Azure sanal ağı ve alt ağı üzerinde kullanılmayan bir IP adresi kullanabilirsiniz.


# <a name="windows-server-2012-2016"></a>[Windows Server 2012-2016](#tab/windows2012)

Aşağıdaki PowerShell betiği bir yük devretme kümesi oluşturur. Betiği, düğümlerin adlarıyla (sanal makine adları) ve kullanılabilir bir IP adresini Azure sanal ağından güncelleştirin.

```powershell
New-Cluster -Name <FailoverCluster-Name> -Node ("<node1>","<node2>") –StaticAddress <n.n.n.n> -NoStorage
```   

# <a name="windows-server-2019"></a>[Windows Server 2019](#tab/windows2019)

Aşağıdaki PowerShell betiği bir yük devretme kümesi oluşturur. Betiği, düğümlerin adlarıyla (sanal makine adları) ve kullanılabilir bir IP adresini Azure sanal ağından güncelleştirin.

```powershell
New-Cluster -Name <FailoverCluster-Name> -Node ("<node1>","<node2>") –StaticAddress <n.n.n.n> -NoStorage -ManagementPointNetworkType Singleton 
```

Daha fazla bilgi için bkz. [Yük devretme kümesi: küme ağ nesnesi](https://blogs.windows.com/windowsexperience/2018/08/14/announcing-windows-server-2019-insider-preview-build-17733/#W0YAxO8BfwBRbkzG.97).

---


## <a name="configure-quorum"></a>Çekirdeği yapılandırma

İşletmenizin ihtiyaçlarına en uygun olan çekirdek çözümünü yapılandırın. Bir [disk tanığı](/windows-server/failover-clustering/manage-cluster-quorum#configure-the-cluster-quorum), [bulut tanığı](/windows-server/failover-clustering/deploy-cloud-witness)veya [dosya paylaşma tanığı](/windows-server/failover-clustering/manage-cluster-quorum#configure-the-cluster-quorum)yapılandırabilirsiniz. Daha fazla bilgi için bkz. [SQL Server VM 'Lerle çekirdek](hadr-cluster-best-practices.md#quorum). 

## <a name="validate-cluster"></a>Kümeyi Doğrula
Kümeyi Kullanıcı arabiriminde veya PowerShell kullanarak doğrulayın.

Kullanıcı arabirimini kullanarak kümeyi doğrulamak için sanal makinelerden birinde şunları yapın:

1. **Sunucu Yöneticisi**altında **Araçlar**' ı seçin ve **Yük devretme kümesi Yöneticisi**' i seçin.
1. **Yük devretme kümesi Yöneticisi**altında **eylem**' i seçin ve ardından **Yapılandırmayı Doğrula**' yı seçin.
1. **İleri**’yi seçin.
1. **Sunucu veya küme Seç**altında, her iki sanal makinenin adını da girin.
1. **Test seçenekleri**altında **yalnızca Seçdiğim Testleri Çalıştır**' ı seçin. 
1. **İleri**’yi seçin.
1. **Test seçimi**altında, **depolama** *hariç* tüm testleri seçin

## <a name="test-cluster-failover"></a>Test kümesi yük devretmesi

Kümenizin yük devretmesini test edin. **Yük devretme kümesi Yöneticisi**, kümenize sağ tıklayın, **diğer eylemler**' i  >  **taşıyın çekirdek küme kaynağı**  >  **Seç düğümünü**seçin ve ardından kümenin diğer düğümünü seçin. Çekirdek küme kaynağını kümenin her düğümüne taşıyın ve ardından birincil düğüme geri taşıyın. Kümeyi her düğüme başarıyla taşıyabiliyorsanız SQL Server yüklemeye hazırsınız demektir.  

:::image type="content" source="media/failover-cluster-instance-premium-file-share-manually-configure/test-cluster-failover.png" alt-text="Çekirdek kaynağı diğer düğümlere taşıyarak küme yük devretmesini test etme":::

## <a name="create-sql-server-fci"></a>SQL Server FCı oluştur

Yük devretme kümesini ve depolama dahil tüm küme bileşenlerini yapılandırdıktan sonra, SQL Server FCı 'yi oluşturabilirsiniz.

1. Uzak Masaüstü Protokolü (RDP) kullanarak ilk sanal makineye bağlanın.

1. **Yük devretme kümesi Yöneticisi**, tüm çekirdek küme kaynaklarının ilk sanal makinede olduğundan emin olun. Gerekirse, tüm kaynakları bu sanal makineye taşıyın.

1. Yükleme medyasını bulun. Sanal makine Azure Marketi görüntülerinden birini kullanıyorsa medya konumunda bulunur `C:\SQLServer_<version number>_Full` . 

1. **Kurulum 'u**seçin.

1. **SQL Server Yükleme Merkezi**'nde **yükleme**' yi seçin.

1. **Yeni SQL Server yük devretme kümesi yüklemesi ' ni**seçin. SQL Server FCı 'yi yüklemek için sihirbazdaki yönergeleri izleyin.

FCı veri dizinlerinin Azure Paylaşılan disklerinde olması gerekir. 

1. Sihirbazdaki yönergeleri tamamladıktan sonra, kurulum ilk düğümde bir SQL Server FCı yükler.

1. Kurulum, ilk düğümde FCı 'yı yükledikten sonra, RDP kullanarak ikinci düğüme bağlanın.

1. **SQL Server yükleme merkezini**açın ve ardından **yükleme**' yi seçin.

1. **SQL Server yük devretme kümesine düğüm Ekle**' yi seçin. SQL Server yüklemek ve sunucuyu FCı 'ye eklemek için sihirbazdaki yönergeleri izleyin.

   >[!NOTE]
   >SQL Server içeren bir Azure Marketi Galeri görüntüsü kullandıysanız, görüntüye SQL Server Araçlar eklenmiştir. Bu görüntülerden birini kullanmıyorsanız, SQL Server araçlarını ayrı olarak yükleyebilirsiniz. Daha fazla bilgi için bkz. [Download SQL Server Management Studio (SSMS)](https://msdn.microsoft.com/library/mt238290.aspx).
   >

## <a name="register-with-the-sql-vm-rp"></a>SQL VM RP ile kaydolun

SQL Server VM portaldan yönetmek için, SQL VM kaynak sağlayıcısı 'nı (RP) [basit yönetim modunda](sql-vm-resource-provider-register.md#lightweight-management-mode)kaydedin, şu anda, FCI ile desteklenen tek mod ve Azure vm 'lerinde SQL Server. 


PowerShell ile hafif modda bir SQL Server VM kaydetme:  

```powershell-interactive
# Get the existing compute VM
$vm = Get-AzVM -Name <vm_name> -ResourceGroupName <resource_group_name>
         
# Register SQL VM with 'Lightweight' SQL IaaS agent
New-AzSqlVM -Name $vm.Name -ResourceGroupName $vm.ResourceGroupName -Location $vm.Location `
   -LicenseType PAYG -SqlManagementType LightWeight  
```

## <a name="configure-connectivity"></a>Bağlantı yapılandırma 

Trafiği geçerli birincil düğüme uygun bir şekilde yönlendirmek için, ortamınız için uygun olan bağlantı seçeneğini yapılandırın. [Azure yük dengeleyici](hadr-vnn-azure-load-balancer-configure.md) oluşturabilir veya SQL Server 2019 ve Windows Server 2016 (veya üzeri) kullanıyorsanız, [dağıtılmış ağ adı](hadr-distributed-network-name-dnn-configure.md) özelliğinin önizlemesini yapabilirsiniz. 

## <a name="limitations"></a>Sınırlamalar

- Yalnızca Windows Server 2019 ' de 2019 SQL Server desteklenir. 
- Yalnızca [basit yönetim MODUNDAKI](sql-vm-resource-provider-register.md#management-modes) SQL VM kaynak sağlayıcısı ile kaydolma desteklenir.

## <a name="next-steps"></a>Sonraki adımlar

Daha önce yapmadıysanız, bir [sanal ağ adı ve bir Azure yük dengeleyici](hadr-vnn-azure-load-balancer-configure.md) veya [dağıtılmış ağ adı (DNN)](hadr-distributed-network-name-dnn-configure.md)ile FCI 'nize bağlantı yapılandırın. 

Azure paylaşılan diskler sizin için uygun FCı depolama çözümü değilse, bunun yerine [Premium dosya paylaşımlarını](failover-cluster-instance-premium-file-share-manually-configure.md) veya [depolama alanları doğrudan](failover-cluster-instance-storage-spaces-direct-manually-configure.md) kullanarak FCI 'nizi oluşturmayı düşünün. 

Daha fazla bilgi edinmek için bkz. Azure VM 'lerde SQL Server ve [küme yapılandırması en iyi yöntemleri](hadr-cluster-best-practices.md) [ile FCI](failover-cluster-instance-overview.md) 'ye genel bakış.

Daha fazla bilgi için bkz.: 
- [Windows küme teknolojileri](/windows-server/failover-clustering/failover-clustering-overview)   
- [SQL Server yük devretme kümesi örnekleri](/sql/sql-server/failover-clusters/windows/always-on-failover-cluster-instances-sql-server)
