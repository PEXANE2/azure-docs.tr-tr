---
title: SQL VM kaynak sağlayıcısına kaydolun
description: Azure Marketi dışında dağıtılan SQL Server VM'lerin özelliklerinin yanı sıra uyumluluk ve gelişmiş yönetilebilirlik sağlamak için Azure SQL Server sanal makinenizi SQL VM kaynak sağlayıcısına kaydedin.
services: virtual-machines-windows
documentationcenter: na
author: MashaMSFT
manager: craigg
tags: azure-resource-manager
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 11/13/2019
ms.author: mathoma
ms.reviewer: jroth
ms.openlocfilehash: d9c1cff53d5d0f0385d3d61938c7fb6309efb7b1
ms.sourcegitcommit: 7d8158fcdcc25107dfda98a355bf4ee6343c0f5c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/09/2020
ms.locfileid: "80985397"
---
# <a name="register-a-sql-server-virtual-machine-in-azure-with-the-sql-vm-resource-provider"></a>Azure'da SQL VM kaynak sağlayıcısına sql server sanal makine kaydetme

Bu makalede, SQL Server sanal makinenizi (VM) Azure'da SQL VM kaynak sağlayıcısına nasıl kaydedebilirsiniz. Kaynak sağlayıcısına kaydolmak, aboneliğinizdeki **SQL sanal makine** _kaynağını_ oluşturur ve bu da sanal makine kaynağından ayrı bir kaynaktır. SQL Server VM'nizi kaynak sağlayıcısından **kaldırmak, SQL sanal makine** _kaynağını_ kaldırır, ancak gerçek sanal makineyi düşürmez. 

Azure portalı üzerinden bir SQL Server VM Azure Marketplace görüntüsünü dağıtmak, SQL Server VM'yi kaynak sağlayıcısına otomatik olarak kaydeder. Ancak, SQL Server'ı bir Azure sanal makinesine kendi kendine yüklemeyi veya özel bir VHD'den bir Azure sanal makinesi ni vermeyi seçerseniz, SQL Server VM'nizi kaynak sağlayıcısına kaydetmeniz gerekir:

- **Özellik avantajları**: SQL Server VM'inizi kaynak sağlayıcısına kaydettirmeniz otomatik [yama,](virtual-machines-windows-sql-automated-patching.md) [otomatik yedekleme](virtual-machines-windows-sql-automated-backup-v2.md)ve izleme ve yönetilebilirlik yeteneklerinin kilidini açar. Ayrıca [lisanslama](virtual-machines-windows-sql-ahb.md) ve [sürüm](virtual-machines-windows-sql-change-edition.md) esnekliğini de açar. Daha önce, bu özellikler yalnızca Azure Marketi'nden dağıtılan SQL Server VM görüntüleri için kullanılabilirdi. 

- **Uyumluluk**: SQL VM kaynak sağlayıcısına kaydolmak, Azure Karma Avantajının ürün koşullarında belirtildiği şekilde etkinleştirildiğini Microsoft'a bildirme gereksinimini yerine getirmek için basitleştirilmiş bir yöntem sunar. Bu işlem, her kaynak için lisans kayıt formlarını yönetme gereksinimini geçersiz k.  

- **Serbest yönetim**: Her üç yönetilebilirlik modunda da SQL VM kaynak sağlayıcısına kayıt tamamen ücretsizdir. Kaynak sağlayıcısıyla veya değişen yönetim modlarıyla ilişkili ek bir maliyet yoktur. 

- **Basitleştirilmiş lisans yönetimi**: SQL VM kaynak sağlayıcısına kaydolmak SQL Server lisans yönetimini basitleştirir ve [Azure portalı](virtual-machines-windows-sql-manage-portal.md), Az CLI veya PowerShell'i kullanarak etkinleştirilen Azure Karma Avantajı ile SQL Server VM'leri hızlı bir şekilde tanımlamanızı sağlar: 

   # <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

   ```azurecli-interactive
   $vms = az sql vm list | ConvertFrom-Json
   $vms | Where-Object {$_.sqlServerLicenseType -eq "AHUB"}
   ```

   # <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

   ```powershell-interactive
   Get-AzSqlVM | Where-Object {$_.LicenseType -eq 'AHUB'}
   ```

   ---

SQL VM kaynak sağlayıcısını kullanmak için öncelikle [aboneliğinizi kaynak sağlayıcısına kaydettirmeniz](#register-subscription-with-rp)gerekir , bu da kaynak sağlayıcısına belirli bir abonelik içinde kaynak oluşturma olanağı sağlar.

SQL VM kaynak sağlayıcısını kullanmanın yararları hakkında daha fazla bilgi için bu [kanal9](https://channel9.msdn.com/Shows/Data-Exposed/Benefit-from-SQL-VM-Resource-Provider-when-self-installing-SQL-Server-on-Azure?WT.mc_id=dataexposed-c9-niner) videosunu izleyin: 

<iframe src="https://channel9.msdn.com/Shows/Data-Exposed/Benefit-from-SQL-VM-Resource-Provider-when-self-installing-SQL-Server-on-Azure/player" width="960" height="540" allowFullScreen frameBorder="0" title="Azure'da SQL Server'ı kendi kendine yüklerken SQL VM Kaynak Sağlayıcısı'ndan yararlanın - Microsoft Channel 9 Video"></iframe>


## <a name="prerequisites"></a>Ön koşullar

SQL Server VM'nizi kaynak sağlayıcısına kaydetmek için şunları yapmanız gerekir: 

- [Azure aboneliği.](https://azure.microsoft.com/free/)
- Genel veya Azure Genel bulutuna dağıtılan bir Azure Kaynak Modeli [SQL Server VM.](virtual-machines-windows-portal-sql-server-provision.md) 
- [Azure CLI](/cli/azure/install-azure-cli) veya [PowerShell'in](/powershell/azure/new-azureps-module-az)en son sürümü. 

## <a name="management-modes"></a>Yönetim modları

SQL [IaaS uzantısı](virtual-machines-windows-sql-server-agent-extension.md) henüz yüklenmediyse, SQL VM kaynak sağlayıcısına kaydolmak, SQL Server IaaS uzantısını kayıt işlemi sırasında belirtilen üç yönetim modundan birine otomatik olarak yükler. Yönetim modunu belirtmemek, SQL IaaS uzantısını tam yönetim modunda yükler.  

SQL IaaS uzantısı zaten el ile yüklenmişse, o zaman zaten tam yönetim modundadır ve tam modda kaynak sağlayıcısına kaydolmak SQL Server hizmetini yeniden başlatmaz.

Üç yönetim modu şunlardır:

- **Hafif** mod, SQL Server'ın yeniden başlatılmasını gerektirmez, ancak yalnızca SQL Server'ın lisans türünü ve sürümünü değiştirmeyi destekler. Birden çok örneği olan VEYA başarısız küme örneğine (FCI) katılan SQL Server VM'leri için bu seçeneği kullanın. Hafif modu kullanırken bellek veya CPU üzerinde hiçbir etkisi yoktur ve ilişkili bir maliyet yoktur. SQL Server VM'nizi önce hafif modda kaydetmeniz ve ardından zamanlanan bakım penceresi sırasında Tam moda yükseltmeniz önerilir.  

- **Tam** mod tüm işlevselliği sunar, ancak SQL Server ve sistem yöneticisi izinlerinin yeniden başlatılmasını gerektirir. Bu, SQL IaaS uzantısını el ile yüklerken varsayılan olarak yüklenen seçenektir. Bir SQL Server VM'yi tek bir örnekle yönetmek için kullanın. Tam mod bellek ve CPU üzerinde en az etkiye sahip iki windows hizmetleri yükler - bu görev yöneticisi aracılığıyla izlenebilir. Tam yönetilebilirlik modunu kullanmanın bir maliyeti yoktur. 

- **NoAgent** modu, Windows Server 2008'de yüklü olan SQL Server 2008 ve SQL Server 2008 R2'ye adanmıştır. NoAgent modunu kullanırken bellek veya CPU üzerinde hiçbir etkisi yoktur. NoAgent yönetilebilirlik modunu kullanmanın bir maliyeti yoktur. 

PowerShell'i kullanarak SQL Server IaaS aracınızın geçerli modunu görüntüleyebilirsiniz: 

  ```powershell-interactive
  # Get the SqlVirtualMachine
  $sqlvm = Get-AzSqlVM -Name $vm.Name  -ResourceGroupName $vm.ResourceGroupName
  $sqlvm.SqlManagementType
  ```

## <a name="register-subscription-with-rp"></a>RP ile abonelik kaydedin

SQL Server VM'nizi SQL VM kaynak sağlayıcısına kaydettirmek için öncelikle aboneliğinizi kaynak sağlayıcısına kaydetmeniz gerekir. Bu, SQL VM kaynak sağlayıcısına aboneliğiniz içinde kaynak oluşturma olanağı sağlar.  Bunu Azure portalını, Azure CLI'yi veya PowerShell'i kullanarak yapabilirsiniz.

### <a name="azure-portal"></a>Azure portal

1. Azure portalını açın ve **Tüm Hizmetler'e**gidin. 
1. **Abonelikler'e** gidin ve ilgi çekici aboneliği seçin.  
1. **Abonelikler** sayfasında Kaynak **sağlayıcılarına**gidin. 
1. SQL ile ilgili kaynak sağlayıcılarını getirmek için filtreye **SQL** girin. 
1. İstediğiniz eyleme bağlı olarak **Microsoft.SqlVirtualMachine** sağlayıcısına **Kaydol,** **Yeniden Kaydol**veya **Kayıt dışı kaydını kaldır'ı** seçin. 

![Sağlayıcıyı değiştirme](media/virtual-machines-windows-sql-ahb/select-resource-provider-sql.png)


### <a name="command-line"></a>Komut satırı

AZ CLI veya PowerShell'i kullanarak SQL VM kaynak sağlayıcınızı Azure aboneliğinize kaydedin. 

# <a name="az-cli"></a>[AZ CLI](#tab/bash)

```azurecli-interactive
# Register the SQL VM resource provider to your subscription 
az provider register --namespace Microsoft.SqlVirtualMachine 
```

# <a name="powershell"></a>[PowerShell](#tab/powershell)

```powershell-interactive
# Register the SQL VM resource provider to your subscription
Register-AzResourceProvider -ProviderNamespace Microsoft.SqlVirtualMachine
```

---

## <a name="register-sql-vm-with-rp"></a>SQL VM'yi RP ile kaydedin 

### <a name="lightweight-management-mode"></a>Hafif yönetim modu

SQL [Server IaaS Agent Extension](virtual-machines-windows-sql-server-agent-extension.md) VM'ye yüklenmediyse, öneri hafif modda SQL VM kaynak sağlayıcısına kaydolmaktır. Bu, SQL IaaS uzantısını [hafif modda](#management-modes) yükler ve SQL Server hizmetinin yeniden başlatılmasını engeller. Daha sonra istediğiniz zaman tam moda yükseltebilirsiniz, ancak bunu yaparken zamanlanmış bir bakım penceresikadar beklemeniz önerilir, bu yüzden SQL Server hizmetini yeniden başlatacaktır. 

Kullanım başına ödeme yapmak için SQL Server lisans`PAYG`türünü kullandıkça öde (`AHUB`) kullanım başına ödeme yapmak,`DR`Azure Karma Avantajı () kendi lisansınızı kullanmak veya [ücretsiz DR çoğaltma lisansını](virtual-machines-windows-sql-high-availability-dr.md#free-dr-replica-in-azure)etkinleştirmek için olağanüstü durum kurtarma ( ) olarak sağlayın.

Failover Cluster Örnekleri ve çok örnekli dağıtımlar yalnızca hafif modda SQL VM kaynak sağlayıcısına kaydedilebilir. 

# <a name="az-cli"></a>[AZ CLI](#tab/bash)

AZ CLI ile SQL Server VM'i hafif modda kaydedin: 

  ```azurecli-interactive
  # Register Enterprise or Standard self-installed VM in Lightweight mode
  az sql vm create --name <vm_name> --resource-group <resource_group_name> --location <vm_location> --license-type PAYG 
  ```


# <a name="powershell"></a>[PowerShell](#tab/powershell)

POWERShell ile SQL Server VM'i hafif modda kaydedin:  


  ```powershell-interactive
  # Get the existing compute VM
  $vm = Get-AzVM -Name <vm_name> -ResourceGroupName <resource_group_name>
          
  # Register SQL VM with 'Lightweight' SQL IaaS agent
  New-AzSqlVM -Name $vm.Name -ResourceGroupName $vm.ResourceGroupName -Location $vm.Location `
    -LicenseType PAYG -SqlManagementType LightWeight  
  ```

---

### <a name="full-management-mode"></a>Tam yönetim modu


SQL IaaS Uzantısı VM'ye el ile yüklenmişse, SQL Server hizmetini yeniden başlatmadan SQL Server VM'yi tam modda kaydedebilirsiniz. **Ancak, SQL IaaS uzantısı yüklenmediyse, tam modda kayıt olmak SQL IaaS uzantısını tam modda yükler ve SQL Server hizmetini yeniden başlatAcaktır. Lütfen dikkatli olun.**


SQL Server VM'nizi doğrudan tam modda kaydetmek (ve muhtemelen SQL Server hizmetinizi yeniden başlatmak için) aşağıdaki PowerShell komutunu kullanın: 

  ```powershell-interactive
  # Get the existing  Compute VM
  $vm = Get-AzVM -Name <vm_name> -ResourceGroupName <resource_group_name>
        
  # Register with SQL VM resource provider in full mode
  New-AzSqlVM -Name $vm.Name -ResourceGroupName $vm.ResourceGroupName -SqlManagementType Full
  ```

### <a name="noagent-management-mode"></a>NoAgent yönetim modu 

WINDOWS Server 2008'de yüklü olan SQL Server 2008 ve 2008_R2 (R2 değil)_ [NoAgent modunda](#management-modes)SQL VM kaynak sağlayıcısına kaydedilebilir. Bu seçenek uyumluluğu sağlar ve SQL Server VM'nin sınırlı işlevsellikle Azure portalında izlenmesine olanak tanır.

`AHUB` `SQL2008-WS2008` `SQL2008R2-WS2008` `DR` **sqlLicenseType** **sqlImageOffer**SqlLicenseType olarak veya sqlImageOffer olarak belirtin. `PAYG` 

SQL Server 2008 veya 2008 R2'nizi Windows Server 2008 örneğine kaydetmek için aşağıdaki Az CLI veya PowerShell kod parçacıklarını kullanın: 


# <a name="az-cli"></a>[AZ CLI](#tab/bash)

SQL Server 2008 VM'inizi NoAgent modunda Az CLI ile kaydedin: 

  ```azurecli-interactive
   az sql vm create -n sqlvm -g myresourcegroup -l eastus |
   --license-type PAYG --sql-mgmt-type NoAgent 
   --image-sku Enterprise --image-offer SQL2008-WS2008
 ```
 
 
SQL Server 2008 R2 VM'inizi NoAgent modunda Az CLI ile kaydedin: 

  ```azurecli-interactive
   az sql vm create -n sqlvm -g myresourcegroup -l eastus |
   --license-type PAYG --sql-mgmt-type NoAgent 
   --image-sku Enterprise --image-offer SQL2008R2-WS2008
 ```

# <a name="powershell"></a>[PowerShell](#tab/powershell)

PowerShell ile NoAgent modunda SQL Server 2008 VM kaydedin: 


  ```powershell-interactive
  # Get the existing compute VM
  $vm = Get-AzVM -Name <vm_name> -ResourceGroupName <resource_group_name>
          
  New-AzSqlVM -Name $vm.Name -ResourceGroupName $vm.ResourceGroupName -Location $vm.Location `
    -LicenseType PAYG -SqlManagementType NoAgent -Sku Standard -Offer SQL2008-WS2008
  ```
  
  PowerShell ile NoAgent modunda SQL Server 2008 R2 VM kaydedin: 


  ```powershell-interactive
  # Get the existing compute VM
  $vm = Get-AzVM -Name <vm_name> -ResourceGroupName <resource_group_name>
          
  New-AzSqlVM -Name $vm.Name -ResourceGroupName $vm.ResourceGroupName -Location $vm.Location `
    -LicenseType PAYG -SqlManagementType NoAgent -Sku Standard -Offer SQL2008R2-WS2008
  ```

---

## <a name="upgrade-to-full-management-mode"></a>Tam yönetim moduna yükseltme 

*Hafif* IaaS uzantısı yüklü olan SQL Server VM'ler modu Azure portalı, Az CLI veya PowerShell'i kullanarak _tam_ olarak yükseltebilir. _NoAgent_ modundaki SQL Server VM'ler, işletim sistemi Windows 2008 R2 ve üzeri işletim sistemi yükseltildikten sonra _tam_ olarak yükseltilebilir. Düşürmek mümkün değildir - bunu yapmak için SQL VM kaynak sağlayıcısından SQL Server [VM'nin kaydını silmeniz](#unregister-vm-from-rp) gerekir. Bunu **yapmak, SQL sanal makine** _kaynağını_kaldırır, ancak gerçek sanal makineyi silmez. 

PowerShell'i kullanarak SQL Server IaaS aracınızın geçerli modunu görüntüleyebilirsiniz: 

  ```powershell-interactive
  # Get the SqlVirtualMachine
  $sqlvm = Get-AzSqlVM -Name $vm.Name  -ResourceGroupName $vm.ResourceGroupName
  $sqlvm.SqlManagementType
  ```

Aracı modunu tam olarak yükseltmek için: 


### <a name="azure-portal"></a>Azure portal

1. [Azure Portal](https://portal.azure.com) oturum açın.
1. [SQL sanal makineler](virtual-machines-windows-sql-manage-portal.md#access-the-sql-virtual-machines-resource) kaynağınıza gidin. 
1. SQL Server sanal makinenizi seçin ve **Genel Bakış'ı**seçin. 
1. NoAgent veya hafif IaaS moduna sahip SQL Server VM'ler için **SQL IaaS uzantı lı iletiyle kullanılabilen Tek lisans türü ve sürüm güncelleştirmelerini** seçin.

   ![Portaldan modu değiştirmek için seçimler](media/virtual-machines-windows-sql-server-agent-extension/change-sql-iaas-mode-portal.png)

1. Sanal makine onay kutusunda **SQL Server hizmetini yeniden başlatmayı kabul** ediyorum ve ardından IaaS modunuzu tam olarak yükseltmek için **Onayla'yı** seçin. 

    ![Sanal makinede SQL Server hizmetini yeniden başlatmayı kabul etmek için kutuyu işaretleyin](media/virtual-machines-windows-sql-server-agent-extension/enable-full-mode-iaas.png)

### <a name="command-line"></a>Komut satırı

# <a name="az-cli"></a>[AZ CLI](#tab/bash)

Aşağıdaki Az CLI kod parçacıklarını çalıştırın:

  ```azurecli-interactive
  # Update to full mode
  az sql vm update --name <vm_name> --resource-group <resource_group_name> --sql-mgmt-type full  
  ```

# <a name="powershell"></a>[PowerShell](#tab/powershell)

Aşağıdaki PowerShell kod parçacıklarını çalıştırın:

  ```powershell-interactive
  # Get the existing  Compute VM
  $vm = Get-AzVM -Name <vm_name> -ResourceGroupName <resource_group_name>
        
  # Register with SQL VM resource provider in full mode
  Update-AzSqlVM -Name $vm.Name -ResourceGroupName $vm.ResourceGroupName -SqlManagementType Full
  ```

---

## <a name="verify-registration-status"></a>Kayıt durumunu doğrulama
Azure portalını, Azure CLI'yi veya PowerShell'i kullanarak SQL Server VM'nizin SQL VM kaynak sağlayıcısına kayıtlı olup olmadığını doğrulayabilirsiniz. 

### <a name="azure-portal"></a>Azure portal 

1. [Azure Portal](https://portal.azure.com) oturum açın. 
1. [SQL Server sanal makinelerinize](virtual-machines-windows-sql-manage-portal.md)gidin.
1. Listeden SQL Server VM'inizi seçin. SQL Server VM'niz burada listelenmemişse, büyük olasılıkla SQL VM kaynak sağlayıcısına kayıtlı değildir. 
1. **Durum**altındaki değeri görüntüleyin. **Durum** **Başarılı**ise, SQL Server VM SQL VM kaynak sağlayıcısına başarıyla kaydedilmiştir. 

![SQL RP kaydı ile durumu doğrula](media/virtual-machines-windows-sql-register-with-rp/verify-registration-status.png)

### <a name="command-line"></a>Komut satırı

Az CLI veya PowerShell kullanarak geçerli SQL Server VM kayıt durumunu doğrulayın. `ProvisioningState`kaydın `Succeeded` başarılı olup olmadığını gösterir. 

# <a name="az-cli"></a>[AZ CLI](#tab/bash)


  ```azurecli-interactive
  az sql vm show -n <vm_name> -g <resource_group>
 ```

# <a name="powershell"></a>[PowerShell](#tab/powershell)

  ```powershell-interactive
  Get-AzSqlVM -Name <vm_name> -ResourceGroupName <resource_group>
  ```

---

Bir hata, SQL Server VM'nin kaynak sağlayıcısına kayıtlı olmadığını gösterir. 


## <a name="unregister-vm-from-rp"></a>RP'den VM kaydını silme

SQL Server VM'nizi SQL VM kaynak sağlayıcısına kaydetmek için Azure portalını veya Azure CLI'yi kullanarak SQL Virtual Machine *kaynağını* silin. SQL Virtual Machine *kaynağının* silinmesi SQL Server VM'yi silmez. Ancak, dikkatli olun ve *kaynağı*kaldırmaya çalışırken yanlışlıkla sanal makineyi silmek mümkün olduğundan adımları dikkatle izleyin. 

SQL VM kaynak sağlayıcısı ile SQL VM kayıt dışı yönetim modu nu tam düşürmek için gereklidir. 

### <a name="azure-portal"></a>Azure portal

SQL Server VM'nizi Azure portalını kullanarak kaynak sağlayıcısına kaydetmek için aşağıdaki adımları izleyin:

1. [Azure portalında](https://portal.azure.com)oturum açın.
1. SQL Server VM kaynağına gidin. 
  
   ![SQL sanal makineler kaynak](media/virtual-machines-windows-sql-manage-portal/sql-vm-manage.png)

1. **Sil**’i seçin. 

   ![SQL VM kaynak sağlayıcısını silme](media/virtual-machines-windows-sql-register-with-rp/delete-sql-vm-resource-provider.png)

1. SQL sanal makinenin adını yazın ve **sanal makinenin yanındaki onay kutusunu temizleyin.**

   ![SQL VM kaynak sağlayıcısını silme](media/virtual-machines-windows-sql-register-with-rp/confirm-delete-of-resource-uncheck-box.png)

   >[!WARNING]
   > Sanal makine adının yanındaki onay kutusunun temizlenememesi sanal makineyi tamamen *siler.* SQL Server VM'nin kaynak sağlayıcısından kaydını çıkarmak için onay kutusunu temizleyin, ancak *gerçek sanal makineyi silemez.* 

1. SQL Server sanal makinesinin değil, SQL sanal makine *kaynağının*silinmesini onaylamak için **Sil'i** seçin. 

### <a name="command-line"></a>Komut satırı

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)
SQL Server sanal makinenizi Azure CLI ile kaynak sağlayıcıdan çıkarmak için [az sql vm delete](/cli/azure/sql/vm?view=azure-cli-latest#az-sql-vm-delete) komutunu kullanın. Bu, SQL Server sanal makine *kaynağını* kaldırır, ancak sanal makineyi silmez. 


```azurecli-interactive
az sql vm delete 
  --name <SQL VM resource name> |
  --resource-group <Resource group name> |
  --yes 
```

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)
SQL Server sanal makinenizi Azure CLI ile kaynak sağlayıcıdan çıkarmak için [Yeni-AzSqlVM](/powershell/module/az.sqlvirtualmachine/new-azsqlvm)komutunu kullanın. Bu, SQL Server sanal makine *kaynağını* kaldırır, ancak sanal makineyi silmez. 

```powershell-interactive
Remove-AzSqlVM -ResourceGroupName <resource_group_name> -Name <VM_name>
```

---

## <a name="limitations"></a>Sınırlamalar

SQL VM kaynak sağlayıcısı yalnızca şunları destekler:
- Azure Kaynak Yöneticisi aracılığıyla dağıtılan SQL Server VM'leri. Klasik model üzerinden dağıtılan SQL Server VM'ler desteklenmez. 
- Genel veya Azure Genel bulutuna dağıtılan SQL Server VM'leri. Diğer özel veya devlet bulutlarına dağıtımlar desteklenmez. 


## <a name="frequently-asked-questions"></a>Sık sorulan sorular 

**SQL Server VM'imi Azure Marketi'nde sql server görüntüsünden kaydetmeli miyim?**

Hayır. Microsoft, Azure Marketi'ndeki SQL Server görüntülerinden sağlanan VM'leri otomatik olarak kaydeder. SQL VM kaynak sağlayıcısına kaydolmak, yalnızca Azure Marketplace'teki SQL Server görüntülerinden VM *sağlanmamışsa* ve SQL Server kendi kendine yüklenmişse gereklidir.

**SQL VM kaynak sağlayıcısını tüm müşteriler kullanabilir mi?** 

Evet. Müşteriler SQL Server VM'lerini Azure Marketplace'ten sql server görüntüsü kullanmadıklarında ve bunun yerine kendi kendine yüklemişse veya özel VHD'lerini getirdilerse SQL VM kaynak sağlayıcısına kaydettirmelidir. Her tür aboneye (Doğrudan, Kurumsal Sözleşme ve Bulut Çözüm Sağlayıcısı) ait VM'ler SQL VM kaynak sağlayıcısına kaydolabilir.

**SQL Server VM'im zaten SQL Server IaaS uzantısı yüklüyse SQL VM kaynak sağlayıcısına kaydolmalı mıyım?**

SQL Server VM'niz kendi kendine yüklenmişse ve Azure Marketi'ndeki SQL Server görüntülerinden sağlanmış değilse, SQL Server IaaS uzantısını yüklemiş olsanız bile SQL VM kaynak sağlayıcısına kaydolmanız gerekir. SQL VM kaynak sağlayıcısına kaydolmak, Microsoft.SqlVirtualMachines türünde yeni bir kaynak oluşturur. SQL Server IaaS uzantısının yüklenmesi bu kaynağı oluşturmaz.

**SQL VM kaynak sağlayıcısına kaydolurken varsayılan yönetim modu nedir?**

SQL VM kaynak sağlayıcısına kaydolduğunuzda varsayılan yönetim modu *doludur.* SQL VM kaynak sağlayıcısına kaydolduğunuzda SQL Server yönetim özelliği ayarlanmazsa, mod tam yönetilebilirlik olarak ayarlanır ve SQL Server hizmetiniz yeniden başlatılacaktır. Önce hafif modda SQL VM kaynak sağlayıcısına kaydolun ve daha sonra bakım penceresi sırasında tam olarak yükseltme önerilir. 

**SQL VM kaynak sağlayıcısına kaydolmak için ön koşullar nelerdir?**

SQL VM kaynak sağlayıcısına hafif modda veya aracısız modda kaydolmak için ön koşul yoktur. SQL VM kaynak sağlayıcısına tam modda kaydolmanın ön koşulu, SQL Server IaaS uzantısının VM'de yüklü olmasıdır, aksi takdirde SQL Server hizmeti yeniden başlatılır. 

**VM'de SQL Server IaaS uzantısı yüklü değilse, SQL VM kaynak sağlayıcısına kaydolabilir miyim?**

Evet, VM'de SQL Server IaaS uzantısı yüklü değilse, hafif yönetim modunda SQL VM kaynak sağlayıcısına kaydolabilirsiniz. Hafif modda, SQL VM kaynak sağlayıcısı, SQL Server örneğinin sürümünü ve sürümünü doğrulamak için bir konsol uygulaması kullanır. 

SQL VM kaynak sağlayıcısına kaydolurken varsayılan _Full_SQL yönetim modu Tam'dır. SQL VM kaynak sağlayıcısına kaydolurken SQL Management özelliği ayarlanmazsa, mod Tam Yönetilebilirlik olarak ayarlanır. Önce hafif modda SQL VM kaynak sağlayıcısına kaydolun ve daha sonra bakım penceresi sırasında tam olarak yükseltme önerilir. 

**SQL VM kaynak sağlayıcısına kaydolmak VM'me bir aracı yükler mi?**

Hayır. SQL VM kaynak sağlayıcısına kaydolmak yalnızca yeni bir meta veri kaynağı oluşturur. VM'ye bir aracı yüklemez.

SQL Server IaaS uzantısı yalnızca tam yönetilebilirliği sağlamak için gereklidir. Yönetilebilirlik modunu hafiften tam'a yükseltmek SQL Server IaaS uzantısını yükler ve SQL Server'ı yeniden başlatacaktır.

**SQL Server VM kaynak sağlayıcısına kayıt olmak VM'imde SQL Server'ı yeniden başlatacak mı?**

Kayıt sırasında belirtilen moda bağlıdır. Hafif veya NoAgent modu belirtilirse, SQL Server hizmeti yeniden başlatılamaz. Ancak, yönetim modunu tam olarak belirtmek veya yönetim modunu boş bırakmak, SQL Server hizmetinin yeniden başlatılmasına neden olacak tam yönetim modunda SQL IaaS uzantısını yükler. 

**SQL VM kaynak sağlayıcısına kaydolurken hafif ve aracısız yönetim modları arasındaki fark nedir?** 

Aracısız yönetim modu yalnızca Windows Server 2008'de SQL Server 2008 ve SQL Server 2008 R2 için kullanılabilir. Bu sürümler için kullanılabilir tek yönetim modudur. SQL Server'ın diğer tüm sürümlerinde kullanılabilir iki yönetilebilirlik modu hafif ve doludur. 

Aracısız mod, SQL Server sürümü ve sürüm özelliklerinin müşteri tarafından ayarlanmasını gerektirir. Hafif mod, SQL Server örneğinin sürümünü ve sürümünü bulmak için VM'yi sorgular.

**SQL Server lisans türünü belirtmeden SQL VM kaynak sağlayıcısına kaydolabilir miyim?**

Hayır. SQL VM kaynak sağlayıcısına kaydolurken SQL Server lisans türü isteğe bağlı bir özellik değildir. Tüm yönetilebilirlik modlarında (aracısız, hafif ve tam) SQL VM kaynak sağlayıcısına kaydolurken SQL Server lisans türünü istediğiniz kadar öde veya Azure Karma Avantajı olarak ayarlamanız gerekir.

**SQL Server IaaS uzantısını aracısız moddan tam moda yükseltebilir miyim?**

Hayır. Yönetilebilirlik modunu tam veya hafife yükseltmek aracısız mod için kullanılamaz. Bu, Windows Server 2008'in teknik bir sınırlamasıdır. Önce işletim sistemi Windows Server 2008 R2 veya daha büyük yükseltmeniz gerekir ve sonra tam yönetim moduna yükseltmek mümkün olacak. 

**SQL Server IaaS uzantısını hafif moddan tam moda yükseltebilir miyim?**

Evet. Yönetilebilirlik modunun hafiften toya yükseltilmesi PowerShell veya Azure portalı üzerinden desteklenir. SQL Server hizmetini yeniden başlatmayı gerektirir.

**SQL Server IaaS uzantısını tam moddan aracısız veya hafif yönetim moduna düşürebilir miyim?**

Hayır. SQL Server IaaS uzantı yönetilebilirlik modunun düşürülmesi desteklenmez. Yönetilebilirlik modu tam moddan hafif veya aracısız moda indirgenemez ve hafif moddan aracısız moduna düşürülemez. 

Yönetilebilirlik modunu tam yönetilebilirlikten değiştirmek için, SQL Server *kaynağını* bırakarak SQL Server sanal makinesini SQL Server kaynak sağlayıcısından [kaldırın](#unregister-vm-from-rp) ve SQL Server VM'yi farklı bir yönetim modunda yeniden SQL VM kaynak sağlayıcısına yeniden kaydedin.

**Azure portalından SQL VM kaynak sağlayıcısına kaydolabilir miyim?**

Hayır. SQL VM kaynak sağlayıcısına kaydolmak Azure portalında kullanılamaz. SQL VM kaynak sağlayıcısına kaydolmak yalnızca Azure CLI veya PowerShell ile desteklenir. 

**SQL Server yüklenmeden önce SQL VM kaynak sağlayıcısına bir VM kaydedebilir miyim?**

Hayır. Bir VM'nin SQL VM kaynak sağlayıcısına başarılı bir şekilde kaydolması için en az bir SQL Server (Database Engine) örneği olmalıdır. VM'de SQL Server örneği yoksa, yeni Microsoft.SqlVirtualMachine kaynağı başarısız durumda olacaktır.

**Birden fazla SQL Server örneği varsa, SQL VM kaynak sağlayıcısına bir VM kaydedebilir miyim?**

Evet. SQL VM kaynak sağlayıcısı yalnızca bir SQL Server (Database Engine) örneğini kaydeder. SQL VM kaynak sağlayıcısı, birden çok örnek olması durumunda varsayılan SQL Server örneğini kaydeder. Varsayılan bir örnek yoksa, yalnızca hafif modda kaydolmak desteklenir. Hafiften tam yönetilebilirlik moduna yükseltmek için varsayılan SQL Server örneğinin bulunması veya VM'nin yalnızca bir SQL Server örneği olması gerekir.

**SQL VM kaynak sağlayıcısına bir SQL Server failover cluster örneğini kaydedebilir miyim?**

Evet. Azure VM'deki SQL Server failover küme örnekleri hafif modda SQL VM kaynak sağlayıcısına kaydedilebilir. Ancak, SQL Server başarısız küme örnekleri tam yönetilebilirlik moduna yükseltilemez.

**Her Zaman Kullanılabilirlik Grubu yapılandırılırsa VM'mi SQL VM kaynak sağlayıcısına kaydedebilir miyim?**

Evet. Her Zaman Açık kullanılabilirlik grubu yapılandırmasına katılıyorsanız, SQL VM kaynak sağlayıcısına Azure VM'de SQL Server örneğini kaydetmenin hiçbir kısıtlaması yoktur.

**SQL VM kaynak sağlayıcısına kaydolmanın veya tam yönetilebilirlik moduna yükseltmenin maliyeti nedir?**
Yok. SQL VM kaynak sağlayıcısına kaydolmak veya üç yönetilebilirlik modundan herhangi birini kullanmakla ilgili bir ücret yoktur. SQL Server VM'nizi kaynak sağlayıcıyla yönetmek tamamen ücretsizdir. 

**Farklı yönetilebilirlik modlarını kullanmanın performans etkisi nedir?**
*NoAgent* ve *hafif* yönetilebilirlik modlarını kullanırken hiçbir etkisi yoktur. İşletim sistemi için yüklü iki hizmetten *tam* yönetilebilirlik modunu kullanırken en az etki vardır. Bunlar görev yöneticisi aracılığıyla izlenebilir ve yerleşik Windows Hizmetleri konsolunda görülebilir. 

İki hizmet adı şunlardır:
- `SqlIaaSExtensionQuery`(Ekran adı `Microsoft SQL Server IaaS Query Service`- )
- `SQLIaaSExtension`(Ekran adı `Microsoft SQL Server IaaS Agent`- )


## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi için aşağıdaki makalelere bakın: 

* [Windows VM'de SQL Server'a Genel Bakış](virtual-machines-windows-sql-server-iaas-overview.md)
* [Windows VM'de SQL Server için SSS](virtual-machines-windows-sql-server-iaas-faq.md)
* [Windows VM'de SQL Server için fiyatlandırma kılavuzu](virtual-machines-windows-sql-server-pricing-guidance.md)
* [Windows VM'de SQL Server için sürüm notları](virtual-machines-windows-sql-server-iaas-release-notes.md)
