---
title: Depolama Alanları Doğrudan ile FCı oluşturma
description: Azure sanal makinelerinde SQL Server bir yük devretme kümesi örneği (FCı) oluşturmak için Depolama Alanları Doğrudan kullanın.
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
ms.date: 06/18/2020
ms.author: mathoma
ms.openlocfilehash: fb253845330a139b04fa79090a27a135f67cab46
ms.sourcegitcommit: 845a55e6c391c79d2c1585ac1625ea7dc953ea89
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2020
ms.locfileid: "85954800"
---
# <a name="create-an-fci-with-storage-spaces-direct-sql-server-on-azure-vms"></a>Depolama Alanları Doğrudan (Azure VM 'lerinde SQL Server) ile bir FCı oluşturma
[!INCLUDE[appliesto-sqlvm](../../includes/appliesto-sqlvm.md)]

Bu makalede, Azure sanal makinelerinde (VM) SQL Server ile [depolama alanları doğrudan](/windows-server/storage/storage-spaces/storage-spaces-direct-overview) kullanarak yük devretme kümesi örneği (FCI) oluşturma işlemi açıklanmaktadır. Depolama Alanları Doğrudan, bir Windows kümesindeki düğümler (Azure VM 'Ler) arasındaki depolamayı (veri disklerini) eşitleyen yazılım tabanlı bir sanal depolama alanı ağı (VSAN) görevi görür. 

Daha fazla bilgi edinmek için bkz. Azure VM 'lerde ve [küme en iyi uygulamalarında](hadr-cluster-best-practices.md) [SQL Server ile FCI](failover-cluster-instance-overview.md) 'ye genel bakış. 


## <a name="overview"></a>Genel Bakış 

[Depolama alanları doğrudan (S2D)](/windows-server/storage/storage-spaces/storage-spaces-direct-overview) iki tür mimariyi destekler: yakınsanmış ve hiper yakınsama. Hiper yakınsama altyapısı, depolamayı kümelenmiş uygulamayı barındıran aynı sunuculara koyar, böylece depolama her SQL Server FCı düğümünde olur. 

Aşağıdaki diyagramda, Azure VM 'lerinde SQL Server ile hiper yakınsanmış Depolama Alanları Doğrudan kullanan tüm çözüm gösterilmektedir: 

![Hiper yakınsanmış Depolama Alanları Doğrudan kullanarak tüm çözümün diyagramı](./media/failover-cluster-instance-storage-spaces-direct-manually-configure/00-sql-fci-s2d-complete-solution.png)

Yukarıdaki diyagramda, aynı kaynak grubundaki aşağıdaki kaynaklar gösterilmektedir:

- Bir Windows Server yük devretme kümesindeki iki sanal makine. Bir sanal makine bir yük devretme kümesinde olduğunda, *küme düğümü* veya *düğüm*da denir.
- Her sanal makinenin iki veya daha fazla veri diski vardır.
- Depolama Alanları Doğrudan veri disklerindeki verileri eşitler ve eşitlenmiş depolamayı bir depolama havuzu olarak gösterir.
- Depolama havuzu, yük devretme kümesine bir Küme Paylaşılan Birimi (CSV) gösterir.
- SQL Server FCı kümesi rolü, veri sürücüleri için CSV 'yi kullanır.
- SQL Server FCı için IP adresini tutacak bir Azure yük dengeleyici.
- Azure kullanılabilirlik kümesi tüm kaynakları tutar.

   > [!NOTE]
   > Bu çözümün tamamını, bir şablondan Azure 'da oluşturabilirsiniz. GitHub [Azure hızlı başlangıç şablonları](https://github.com/MSBrett/azure-quickstart-templates/tree/master/sql-server-2016-fci-existing-vnet-and-ad) sayfasında bir şablon örneği bulunur. Bu örnek, belirli bir iş yükü için tasarlanmamıştır veya test edilmemiştir. Etki alanına bağlı Depolama Alanları Doğrudan depolama alanı ile bir SQL Server FCı oluşturmak için şablonu çalıştırabilirsiniz. Şablonu değerlendirebilir ve sizin amacınıza göre değiştirebilirsiniz.


## <a name="prerequisites"></a>Ön koşullar

Bu makaledeki yönergeleri tamamlamadan önce Şu durumda olmalıdır:

- Azure aboneliği. [Ücretsiz](https://azure.microsoft.com/free/)olarak kullanmaya başlayın. 
- Bir [kullanılabilirlik kümesinde](../../../virtual-machines/windows/tutorial-availability-sets.md#create-an-availability-set) [iki veya daha fazla hazırlanmış Windows Azure sanal makinesi](failover-cluster-instance-prepare-vm.md) .
- Hem Azure sanal makinelerinde hem de Active Directory nesne oluşturma izinlerine sahip olan bir hesap.
- En son [PowerShell](/powershell/azure/install-az-ps?view=azps-4.2.0)sürümü. 


## <a name="add-the-windows-cluster-feature"></a>Windows küme özelliğini ekleme

1. Yerel yöneticilerin üyesi olan ve Active Directory nesneleri oluşturma izni olan bir etki alanı hesabıyla Uzak Masaüstü Protokolü (RDP) kullanarak ilk sanal makineye bağlanın. Yapılandırmanın geri kalanı için bu hesabı kullanın.

1. Yük devretme kümelemesini her bir sanal makineye ekleyin.

   Yük devretme kümelemesini kullanıcı arabiriminden yüklemek için her iki sanal makinede şunları yapın:

   1. **Sunucu Yöneticisi**, **Yönet**' i seçin ve ardından **rol ve Özellik Ekle**' yi seçin.
   1. **Rol ve özellik ekleme** Sihirbazı ' nda, **özellikleri seçerken** **İleri** ' yi seçin.
   1. **Özellikleri Seç**bölümünde **Yük Devretme Kümelemesi**' ni seçin. Tüm gerekli özellikleri ve yönetim araçlarını dahil edin. 
   1. **Özellik Ekle**' yi seçin.
   1. **İleri**' yi seçin ve sonra özellikleri yüklemek için **son** ' u seçin.

   PowerShell 'i kullanarak Yük Devretme Kümelemesi 'ni yüklemek için sanal makinelerden birindeki yönetici PowerShell oturumundan aşağıdaki betiği çalıştırın:

   ```powershell
   $nodes = ("<node1>","<node2>")
   Invoke-Command  $nodes {Install-WindowsFeature Failover-Clustering -IncludeAllSubFeature -IncludeManagementTools}
   ```

Sonraki adımlar hakkında daha fazla bilgi için [Windows Server 2016 ' de depolama alanları doğrudan kullanarak hiper yakınsama çözümünün](https://technet.microsoft.com/windows-server-docs/storage/storage-spaces/hyper-converged-solution-using-storage-spaces-direct#step-3-configure-storage-spaces-direct)"Adım 3: yapılandırma depolama alanları doğrudan" bölümündeki yönergelere bakın.


## <a name="validate-the-cluster"></a>Kümeyi doğrulama

Kümeyi Kullanıcı arabiriminde veya PowerShell kullanarak doğrulayın.

Kullanıcı arabirimini kullanarak kümeyi doğrulamak için sanal makinelerden birinde şunları yapın:

1. **Sunucu Yöneticisi**altında **Araçlar**' ı seçin ve **Yük devretme kümesi Yöneticisi**' i seçin.
1. **Yük devretme kümesi Yöneticisi**altında **eylem**' i seçin ve ardından **Yapılandırmayı Doğrula**' yı seçin.
1. **İleri**’yi seçin.
1. **Sunucu veya küme Seç**altında, her iki sanal makinenin adını da girin.
1. **Test seçenekleri**altında **yalnızca Seçdiğim Testleri Çalıştır**' ı seçin. 
1. **İleri**’yi seçin.
1. **Test seçimi**altında, aşağıda gösterildiği gibi, **depolama**hariç tüm testleri seçin:

   ![Küme doğrulama testlerini seçin](./media/failover-cluster-instance-storage-spaces-direct-manually-configure/10-validate-cluster-test.png)

1. **İleri**’yi seçin.
1. **Onay**altında **İleri**' yi seçin.

    **Yapılandırma doğrulama** Sihirbazı doğrulama testlerini çalıştırır.

PowerShell 'i kullanarak kümeyi doğrulamak için sanal makinelerden birindeki yönetici PowerShell oturumundan aşağıdaki betiği çalıştırın:

   ```powershell
   Test-Cluster –Node ("<node1>","<node2>") –Include "Storage Spaces Direct", "Inventory", "Network", "System Configuration"
   ```

Kümeyi doğruladıktan sonra, yük devretme kümesini oluşturun.


## <a name="create-failover-cluster"></a>Yük devretme kümesi oluşturma

Yük devretme kümesini oluşturmak için şunlar gerekir:

- Küme düğümleri olacak sanal makinelerin adları.
- Yük devretme kümesi için bir ad.
- Yük devretme kümesi için bir IP adresi. Küme düğümleri ile aynı Azure sanal ağı ve alt ağı üzerinde kullanılmayan bir IP adresi kullanabilirsiniz.


# <a name="windows-server-2012---2016"></a>[Windows Server 2012-2016](#tab/windows2012)

Aşağıdaki PowerShell betiği Windows Server 2016 aracılığıyla Windows Server 2012 için bir yük devretme kümesi oluşturur. Betiği, düğümlerin adlarıyla (sanal makine adları) ve kullanılabilir bir IP adresini Azure sanal ağından güncelleştirin.

```powershell
New-Cluster -Name <FailoverCluster-Name> -Node ("<node1>","<node2>") –StaticAddress <n.n.n.n> -NoStorage
```   

# <a name="windows-server-2019"></a>[Windows Server 2019](#tab/windows2019)

Aşağıdaki PowerShell betiği Windows Server 2019 için bir yük devretme kümesi oluşturur.  Betiği, düğümlerin adlarıyla (sanal makine adları) ve kullanılabilir bir IP adresini Azure sanal ağından güncelleştirin.

```powershell
New-Cluster -Name <FailoverCluster-Name> -Node ("<node1>","<node2>") –StaticAddress <n.n.n.n> -NoStorage -ManagementPointNetworkType Singleton 
```

Daha fazla bilgi için bkz. [Yük devretme kümesi: küme ağ nesnesi](https://blogs.windows.com/windowsexperience/2018/08/14/announcing-windows-server-2019-insider-preview-build-17733/#W0YAxO8BfwBRbkzG.97).

---


## <a name="configure-quorum"></a>Çekirdeği yapılandırma

İşletmenizin ihtiyaçlarına en uygun olan çekirdek çözümünü yapılandırın. Bir [disk tanığı](/windows-server/failover-clustering/manage-cluster-quorum#configure-the-cluster-quorum), [bulut tanığı](/windows-server/failover-clustering/deploy-cloud-witness)veya [dosya paylaşma tanığı](/windows-server/failover-clustering/manage-cluster-quorum#configure-the-cluster-quorum)yapılandırabilirsiniz. Daha fazla bilgi için bkz. [SQL Server VM 'Lerle çekirdek](hadr-cluster-best-practices.md#quorum). 

## <a name="add-storage"></a>Depolama ekleme

Depolama Alanları Doğrudan disklerin boş olması gerekir. Bunlar bölüm veya diğer verileri içeremez. Diskleri temizlemek için, [depolama alanları doğrudan dağıtma](https://docs.microsoft.com/windows-server/storage/storage-spaces/deploy-storage-spaces-direct?redirectedfrom=MSDN#step-31-clean-drives)' daki yönergeleri izleyin.

1. [Depolama alanları doğrudan etkinleştirin](https://technet.microsoft.com/windows-server-docs/storage/storage-spaces/hyper-converged-solution-using-storage-spaces-direct#step-35-enable-storage-spaces-direct).

   Aşağıdaki PowerShell betiği Depolama Alanları Doğrudan şunları sunar:  

   ```powershell
   Enable-ClusterS2D
   ```

   **Yük devretme kümesi Yöneticisi**, artık depolama havuzunu görebilirsiniz.

1. [Bir birim oluşturun](https://technet.microsoft.com/windows-server-docs/storage/storage-spaces/hyper-converged-solution-using-storage-spaces-direct#step-36-create-volumes).

   Depolama Alanları Doğrudan, etkinleştirdiğinizde otomatik olarak bir depolama havuzu oluşturur. Artık bir birim oluşturmaya hazırsınız. PowerShell cmdlet 'i `New-Volume` , birim oluşturma işlemini otomatikleştirir. Bu işlem biçimlendirme, birimi kümeye ekleme ve CSV oluşturma işlemlerini içerir. Bu örnek, 800 gigabaytlık (GB) bir CSV oluşturur:

   ```powershell
   New-Volume -StoragePoolFriendlyName S2D* -FriendlyName VDisk01 -FileSystem CSVFS_REFS -Size 800GB
   ```   

   Yukarıdaki komutu çalıştırdıktan sonra, bir küme kaynağı olarak 800 GB bir birim bağlanır. Birim: `C:\ClusterStorage\Volume1\` .

   Bu ekran görüntüsünde Depolama Alanları Doğrudan CSV gösterilmektedir:

   ![Depolama Alanları Doğrudan Küme Paylaşılan Birimi ekran görüntüsü](./media/failover-cluster-instance-storage-spaces-direct-manually-configure/15-cluster-shared-volume.png)



## <a name="test-cluster-failover"></a>Test kümesi yük devretmesi

Kümenizin yük devretmesini test edin. **Yük devretme kümesi Yöneticisi**, kümenize sağ tıklayın, **diğer eylemler**' i  >  **taşıyın çekirdek küme kaynağı**  >  **Seç düğümünü**seçin ve ardından kümenin diğer düğümünü seçin. Çekirdek küme kaynağını kümenin her düğümüne taşıyın ve ardından birincil düğüme geri taşıyın. Kümeyi her düğüme başarıyla taşıyabiliyorsanız SQL Server yüklemeye hazırsınız demektir.  

:::image type="content" source="media/failover-cluster-instance-premium-file-share-manually-configure/test-cluster-failover.png" alt-text="Çekirdek kaynağı diğer düğümlere taşıyarak küme yük devretmesini test etme":::

## <a name="create-sql-server-fci"></a>SQL Server FCı oluştur

Yük devretme kümesini ve depolama dahil tüm küme bileşenlerini yapılandırdıktan sonra, SQL Server FCı 'yi oluşturabilirsiniz.

1. RDP kullanarak ilk sanal makineye bağlanın.

1. **Yük devretme kümesi Yöneticisi**, tüm çekirdek küme kaynaklarının ilk sanal makinede olduğundan emin olun. Gerekirse, tüm kaynakları bu sanal makineye taşıyın.

1. Yükleme medyasını bulun. Sanal makine Azure Marketi görüntülerinden birini kullanıyorsa medya konumunda bulunur `C:\SQLServer_<version number>_Full` . **Kurulum 'u**seçin.

1. **SQL Server Yükleme Merkezi**'nde **yükleme**' yi seçin.

1. **Yeni SQL Server yük devretme kümesi yüklemesi ' ni**seçin. SQL Server FCı 'yi yüklemek için sihirbazdaki yönergeleri izleyin.

   FCı veri dizinlerinin kümelenmiş depolamada olması gerekir. Depolama Alanları Doğrudan, bu, paylaşılan bir disk değildir ancak her bir sunucudaki birime bağlama noktasıdır. Depolama Alanları Doğrudan, birimi her iki düğüm arasında eşitler. Birim, kümeye bir CSV olarak sunulur. Veri dizinleri için CSV bağlama noktasını kullanın.

   ![Veri dizinleri](./media/failover-cluster-instance-storage-spaces-direct-manually-configure/20-data-dicrectories.png)

1. Sihirbazdaki yönergeleri tamamladıktan sonra, kurulum ilk düğümde bir SQL Server FCı yüklemesi yapar.

1. Kurulum, ilk düğümde FCı 'yı yükledikten sonra, RDP kullanarak ikinci düğüme bağlanın.

1. **SQL Server yükleme merkezini**açın. **Yükleme**' yi seçin.

1. **SQL Server yük devretme kümesine düğüm Ekle**' yi seçin. SQL Server yüklemek ve sunucuyu FCı 'ye eklemek için sihirbazdaki yönergeleri izleyin.

   >[!NOTE]
   >SQL Server içeren bir Azure Marketi Galeri görüntüsü kullandıysanız, görüntüye SQL Server Araçlar eklenmiştir. Bu görüntülerden birini kullanmıyorsanız, SQL Server araçlarını ayrı olarak yükleyebilirsiniz. Daha fazla bilgi için bkz. [Download SQL Server Management Studio (SSMS)](https://msdn.microsoft.com/library/mt238290.aspx).
   >


## <a name="register-with-the-sql-vm-rp"></a>SQL VM RP ile kaydolun

SQL Server VM portaldan yönetmek için, SQL VM kaynak sağlayıcısı 'nı (RP) [basit yönetim modunda](sql-vm-resource-provider-register.md#lightweight-management-mode)kaydedin, şu anda yalnızca FCI ile desteklenen tek mod ve Azure vm 'lerinde SQL Server. 


PowerShell ile hafif modda bir SQL Server VM kaydetme:  

```powershell-interactive
# Get the existing compute VM
$vm = Get-AzVM -Name <vm_name> -ResourceGroupName <resource_group_name>
         
# Register SQL VM with 'Lightweight' SQL IaaS agent
New-AzSqlVM -Name $vm.Name -ResourceGroupName $vm.ResourceGroupName -Location $vm.Location `
   -LicenseType PAYG -SqlManagementType LightWeight  
```

## <a name="configure-connectivity"></a>Bağlantı yapılandırma 

Trafiği geçerli birincil düğüme uygun bir şekilde yönlendirmek için, ortamınız için uygun olan bağlantı seçeneğini yapılandırın. [Azure yük dengeleyici](hadr-vnn-azure-load-balancer-configure.md) oluşturabilir veya SQL Server 2019 ve Windows Server 2019 kullanıyorsanız, [dağıtılmış ağ adı](hadr-distributed-network-name-dnn-configure.md) özelliğinin önizlemesini yapabilirsiniz. 

## <a name="limitations"></a>Sınırlamalar

- Azure sanal makineleri, Windows Server 2019 ' de CSV ve [Standart yük dengeleyici](../../../load-balancer/load-balancer-standard-overview.md)depolama ile Microsoft Dağıtılmış işlem DÜZENLEYICISI (MSDTC) destekler.
- NTFS biçimli diskler olarak eklenmiş diskler, yalnızca disk uygunluk seçeneği işaretli değilse ya da depolama alanı kümeye eklenirken silinirse Depolama Alanları Doğrudan ile kullanılabilir. 
- Yalnızca [basit yönetim MODUNDAKI](sql-vm-resource-provider-register.md#management-modes) SQL VM kaynak sağlayıcısı ile kaydolma desteklenir.

## <a name="next-steps"></a>Sonraki adımlar

Daha önce yapmadıysanız, bir [sanal ağ adı ve bir Azure yük dengeleyici](hadr-vnn-azure-load-balancer-configure.md) veya [dağıtılmış ağ adı (DNN)](hadr-distributed-network-name-dnn-configure.md)ile FCI 'nize bağlantı yapılandırın. 

Sizin için uygun FCı depolama çözümü değilse, bunun yerine [Azure paylaşılan diskler](failover-cluster-instance-azure-shared-disks-manually-configure.md) veya [Premium dosya paylaşımları](failover-cluster-instance-premium-file-share-manually-configure.md) kullanarak FCI 'nizi oluşturmayı düşünün. depolama alanları doğrudan 

Daha fazla bilgi edinmek için bkz. Azure VM 'lerde SQL Server ve [küme yapılandırması en iyi yöntemleri](hadr-cluster-best-practices.md) [ile FCI](failover-cluster-instance-overview.md) 'ye genel bakış. 

Daha fazla bilgi için bkz. 
- [Windows küme teknolojileri](/windows-server/failover-clustering/failover-clustering-overview)   
- [SQL Server yük devretme kümesi örnekleri](/sql/sql-server/failover-clusters/windows/always-on-failover-cluster-instances-sql-server)
