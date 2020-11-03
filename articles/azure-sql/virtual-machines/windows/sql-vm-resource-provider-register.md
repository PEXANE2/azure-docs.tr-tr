---
title: SQL Server IaaS Aracısı uzantısını yükler
description: Azure SQL Server sanal makinenizi SQL VM kaynak sağlayıcısı ile kaydedin ve Azure Market dışında dağıtılan SQL Server sanal makinelere yönelik özelliklerin yanı sıra uyumluluk ve gelişmiş yönetilebilirlik sağlar.
services: virtual-machines-windows
documentationcenter: na
author: MashaMSFT
tags: azure-resource-manager
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 10/30/2020
ms.author: mathoma
ms.reviewer: jroth
ms.custom: devx-track-azurecli, devx-track-azurepowershell
ms.openlocfilehash: 8099bf87410e8359894d8b40d7637451891ad664
ms.sourcegitcommit: 7863fcea618b0342b7c91ae345aa099114205b03
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/03/2020
ms.locfileid: "93286134"
---
# <a name="install-sql-server-iaas-agent-extension"></a>SQL Server IaaS Aracısı uzantısını yükler
[!INCLUDE[appliesto-sqlvm](../../includes/appliesto-sqlvm.md)]

SQL Server VM SQL VM kaynak sağlayıcısı ile kaydettirmek, [SQL Server IaaS Aracısı uzantısını](sql-server-iaas-agent-extension-automate-management.md)kurar. 

Bu makale, SQL VM kaynak sağlayıcısı ile tek bir SQL Server VM kaydetmenizi öğretir. Alternatif olarak, tüm SQL Server sanal makinelerini [otomatik olarak](sql-vm-resource-provider-automatic-registration.md) veya [komut dosyası toplu](sql-vm-resource-provider-bulk-register.md)olarak kaydedebilirsiniz.


## <a name="overview"></a>Genel Bakış

Kaynak sağlayıcısına kaydolmak, [SQL Server IaaS Aracısı uzantısını](sql-server-iaas-agent-extension-automate-management.md) ve ayrıca, sanal makine kaynağından _ayrı_ bir kaynak olan aboneliğinizdeki **SQL sanal makine** _kaynağını_ oluşturur. Kaynak sağlayıcıdan SQL Server VM kaydını silmek **SQL sanal makine** _kaynağını_ kaldıracak ancak gerçek sanal makineyi temizlemiyor.

Azure portal üzerinden SQL Server VM Azure Marketi görüntüsünün dağıtımı, SQL Server VM kaynak sağlayıcısıyla otomatik olarak kaydeder. Bununla birlikte, bir Azure sanal makinesine SQL Server kendi kendine yüklemeyi veya özel bir VHD 'den bir Azure sanal makinesini sağlamayı seçerseniz, SQL IaaS Aracısı uzantısını yüklemek için SQL Server VM SQL VM kaynak sağlayıcısı ile kaydetmeniz gerekir. 

SQL VM kaynak sağlayıcısını kullanmak için, önce [aboneliğinizi kaynak sağlayıcısına kaydetmeniz](#register-subscription-with-rp)gerekir, bu da kaynak sağlayıcısına söz konusu abonelik içinde kaynak oluşturma yeteneği verir.

> [!IMPORTANT]
> SQL IaaS Aracısı uzantısı, Azure sanal makineler 'de SQL Server kullanırken müşterilere isteğe bağlı avantajlar vermek için veri toplar. Microsoft, bu verileri müşterinin öncelikli onayı olmadan lisanslama denetimleri için kullanmaz. Daha fazla bilgi için [SQL Server Gizlilik Eki](/sql/sql-server/sql-server-privacy#non-personal-data) ' ne bakın.

## <a name="prerequisites"></a>Önkoşullar

SQL Server VM kaynak sağlayıcısına kaydetmek için şunlar gerekir: 

- Bir [Azure aboneliği](https://azure.microsoft.com/free/).
- Azure kaynak modeli [Windows Server 2008 (veya üzeri)](../../../virtual-machines/windows/quick-create-portal.md) , genel veya Azure Kamu bulutuna dağıtılan [SQL Server 2008 (veya daha büyük](https://www.microsoft.com/sql-server/sql-server-downloads) ) bir sanal makineye sahiptir. 
- [Azure CLI](/cli/azure/install-azure-cli) veya Azure PowerShell 'nin en son sürümü [(5,0 en az)](/powershell/azure/install-az-ps). 


## <a name="register-subscription-with-rp"></a>Aboneliği RP ile kaydetme

SQL Server VM SQL VM kaynak sağlayıcısına kaydetmek için öncelikle aboneliğinizi kaynak sağlayıcısına kaydetmeniz gerekir. Bu, SQL VM kaynak sağlayıcısına aboneliğinizdeki kaynakları oluşturma olanağı sağlar.  Azure portal, Azure CLı veya Azure PowerShell kullanarak bunu yapabilirsiniz.

### <a name="azure-portal"></a>Azure portal

1. Azure portal açın ve **tüm hizmetlere** gidin. 
1. **Abonelikler** ' e gidin ve ilgilendiğiniz aboneliği seçin.  
1. **Abonelikler** sayfasında, **kaynak sağlayıcıları** ' na gidin. 
1. SQL ile ilgili kaynak sağlayıcılarını getirmek için filtreye **SQL** girin. 
1. İstediğiniz eyleme bağlı olarak **Microsoft. SqlVirtualMachine** sağlayıcısı için **Kaydet** , **yeniden kaydet** veya **kaydı kaldır** ' ı seçin. 

   ![Sağlayıcıyı değiştirme](./media/sql-vm-resource-provider-register/select-resource-provider-sql.png)


### <a name="command-line"></a>Komut satırı

Azure CLı veya Azure PowerShell kullanarak SQL VM kaynak sağlayıcınızı Azure aboneliğinize kaydedin. 

# <a name="azure-cli"></a>[Azure CLI](#tab/bash)

```azurecli-interactive
# Register the SQL VM resource provider to your subscription 
az provider register --namespace Microsoft.SqlVirtualMachine 
```

# <a name="azure-powershell"></a>[Azure PowerShell](#tab/powershell)

```powershell-interactive
# Register the SQL VM resource provider to your subscription
Register-AzResourceProvider -ProviderNamespace Microsoft.SqlVirtualMachine
```

---

## <a name="register-with-rp"></a>RP ile kaydolun

[SQL Server IaaS Aracısı uzantısı](sql-server-iaas-agent-extension-automate-management.md#management-modes)için üç yönetim modu vardır. 

Uzantının tam yönetim modunda yüklenmesi SQL Server hizmetini yeniden başlatır. böylece, uzantıyı önce basit modda yüklemeniz önerilir, sonra bir bakım penceresi sırasında [tam olarak yükseltin](#upgrade-to-full) . 

### <a name="lightweight-management-mode"></a>Hafif yönetim modu

SQL Server VM kaynak sağlayıcısına kaydetmek ve SQL IaaS uzantısını hafif modda yüklemek için Azure CLı veya Azure PowerShell kullanın. Bu işlem SQL Server hizmetini yeniden başlatmaz. Dilediğiniz zaman tam moda yükseltebilirsiniz, ancak bunu yapmak SQL Server hizmeti 'ni yeniden başlatarak zamanlanan bir bakım penceresine kadar beklemeniz önerilir. 

`PAYG` `AHUB` `DR` [Ücretsiz Dr çoğaltma lisansını](business-continuity-high-availability-disaster-recovery-hadr-overview.md#free-dr-replica-in-azure)etkinleştirmek için, kullanım başına ödeme yapmak üzere Kullandıkça Öde (), kendi lisansınızı kullanmak için Azure hibrit avantajı () veya olağanüstü durum kurtarma () olarak SQL Server lisans türü sağlayın.

Yük devretme kümesi örnekleri ve çok örnekli dağıtımlar yalnızca SQL VM kaynak sağlayıcısı ile hafif modda kaydedilebilir. 

# <a name="azure-cli"></a>[Azure CLI](#tab/bash)

Azure CLı ile hafif modda bir SQL Server VM kaydetme: 

  ```azurecli-interactive
  # Register Enterprise or Standard self-installed VM in Lightweight mode
  az sql vm create --name <vm_name> --resource-group <resource_group_name> --location <vm_location> --license-type PAYG 
  ```


# <a name="azure-powershell"></a>[Azure PowerShell](#tab/powershell)

SQL Server VM Azure PowerShell basit modda kaydetme:  


  ```powershell-interactive
  # Get the existing compute VM
  $vm = Get-AzVM -Name <vm_name> -ResourceGroupName <resource_group_name>
          
  # Register SQL VM with 'Lightweight' SQL IaaS agent
  New-AzSqlVM -Name $vm.Name -ResourceGroupName $vm.ResourceGroupName -Location $vm.Location `
    -LicenseType PAYG -SqlManagementType LightWeight  
  ```

---

### <a name="full-management-mode"></a>Tam yönetim modu

SQL Server VM tam modda kaydetmek SQL Server hizmetini yeniden başlatır. Lütfen dikkatli ilerleyin. 

SQL Server VM doğrudan tam modda kaydetmek için (ve SQL Server hizmetinizi büyük olasılıkla yeniden başlatmanız), aşağıdaki Azure PowerShell komutunu kullanın: 

  ```powershell-interactive
  # Get the existing  Compute VM
  $vm = Get-AzVM -Name <vm_name> -ResourceGroupName <resource_group_name>
        
  # Register with SQL VM resource provider in full mode
  New-AzSqlVM -Name $vm.Name -ResourceGroupName $vm.ResourceGroupName -SqlManagementType Full
  ```

### <a name="noagent-management-mode"></a>NoAgent yönetim modu 

Windows Server 2008 ( _R2 değil_ ) üzerinde yüklü olan SQL Server 2008 ve 2008 R2, [NOAGENT modundaki](sql-server-iaas-agent-extension-automate-management.md#management-modes)SQL VM kaynak sağlayıcısı ile kaydedilebilir. Bu seçenek uyumluluk sağlar ve Azure portal sınırlı işlevlerle birlikte SQL Server VM izlenmesini sağlar.

`AHUB`, `PAYG` , Ya da `DR` **Sqllicensetype** olarak `SQL2008-WS2008` ya da `SQL2008R2-WS2008` **sqlimageteklifini** belirtin. 

SQL Server 2008 veya 2008 R2 'yi Windows Server 2008 örneğine kaydetmek için aşağıdaki Azure CLı veya Azure PowerShell kod parçacığını kullanın: 


# <a name="azure-cli"></a>[Azure CLI](#tab/bash)

Azure CLı ile SQL Server 2008 sanal makinenizi NoAgent modunda kaydedin: 

  ```azurecli-interactive
   az sql vm create -n sqlvm -g myresourcegroup -l eastus |
   --license-type PAYG --sql-mgmt-type NoAgent 
   --image-sku Enterprise --image-offer SQL2008-WS2008
 ```
 
 
Azure CLı ile SQL Server 2008 R2 sanal makinenizi NoAgent modunda kaydedin: 

  ```azurecli-interactive
   az sql vm create -n sqlvm -g myresourcegroup -l eastus |
   --license-type PAYG --sql-mgmt-type NoAgent 
   --image-sku Enterprise --image-offer SQL2008R2-WS2008
 ```

# <a name="azure-powershell"></a>[Azure PowerShell](#tab/powershell)

SQL Server 2008 sanal makinenizi NoAgent moduna Azure PowerShell ile kaydedin: 

  ```powershell-interactive
  # Get the existing compute VM
  $vm = Get-AzVM -Name <vm_name> -ResourceGroupName <resource_group_name>
          
  New-AzSqlVM -Name $vm.Name -ResourceGroupName $vm.ResourceGroupName -Location $vm.Location `
    -LicenseType PAYG -SqlManagementType NoAgent -Sku Standard -Offer SQL2008-WS2008
  ```
  

SQL Server 2008 R2 sanal makinenizi Azure PowerShell ile NoAgent moduna kaydedin: 

  ```powershell-interactive
  # Get the existing compute VM
  $vm = Get-AzVM -Name <vm_name> -ResourceGroupName <resource_group_name>
          
  New-AzSqlVM -Name $vm.Name -ResourceGroupName $vm.ResourceGroupName -Location $vm.Location `
    -LicenseType PAYG -SqlManagementType NoAgent -Sku Standard -Offer SQL2008R2-WS2008
  ```

---

## <a name="verify-mode"></a>Doğrulama modu

SQL Server IaaS aracınızın geçerli modunu Azure PowerShell kullanarak görüntüleyebilirsiniz: 

```powershell-interactive
# Get the SqlVirtualMachine
$sqlvm = Get-AzSqlVM -Name $vm.Name  -ResourceGroupName $vm.ResourceGroupName
$sqlvm.SqlManagementType
```

## <a name="upgrade-to-full"></a>Tam sürüme yükselt  

*Hafif* IaaS uzantısının yüklü olduğu SQL Server VM 'ler, Azure Portal, Azure clı veya Azure PowerShell kullanarak modu _tam_ olarak yükseltebilir. _Noagent_ modundaki SQL Server VM 'ler, Işletim sistemi Windows 2008 R2 ve üzeri sürümlere yükseltildikten sonra _tam_ sürümüne yükseltilebilir. Bunun için düşürme yapılamaz, bunun için SQL VM kaynak sağlayıcısı 'ndan SQL Server VM [kaydını](#unregister-from-rp) kaldırmanız gerekir. Bunun yapılması **SQL sanal makine** _kaynağını_ kaldıracak, ancak gerçek sanal makineyi silmeyecektir. 


### <a name="azure-portal"></a>Azure portal

1. [Azure Portal](https://portal.azure.com) oturum açın.
1. [SQL sanal makineler](manage-sql-vm-portal.md#access-the-sql-virtual-machines-resource) kaynağına gidin. 
1. SQL Server VM seçin ve **genel bakış** ' ı seçin. 
1. NoAgent veya Lightweight IaaS modundaki sanal makineler SQL Server için, **SQL IaaS uzantı Iletisiyle tek lisans türünü seçin ve sürüm güncelleştirmelerini** seçin.

   ![Portalın modunu değiştirme seçimleri](./media/sql-vm-resource-provider-register/change-sql-iaas-mode-portal.png)

1. **Sanal makinede SQL Server hizmetini yeniden başlatmayı kabul** ediyorum onay kutusunu seçin ve ardından IaaS modumu tam olarak yükseltmek için **Onayla** ' yı seçin. 

    ![Sanal makinede SQL Server hizmetini yeniden başlatmak için kabul etmiş onay kutusu](./media/sql-vm-resource-provider-register/enable-full-mode-iaas.png)

### <a name="command-line"></a>Komut satırı

# <a name="azure-cli"></a>[Azure CLI](#tab/bash)

Aşağıdaki Azure CLı kod parçacığını çalıştırın:

  ```azurecli-interactive
  # Update to full mode
  az sql vm update --name <vm_name> --resource-group <resource_group_name> --sql-mgmt-type full  
  ```

# <a name="azure-powershell"></a>[Azure PowerShell](#tab/powershell)

Aşağıdaki Azure PowerShell kod parçacığını çalıştırın:

  ```powershell-interactive
  # Get the existing  Compute VM
  $vm = Get-AzVM -Name <vm_name> -ResourceGroupName <resource_group_name>
        
  # Register with SQL VM resource provider in full mode
  Update-AzSqlVM -Name $vm.Name -ResourceGroupName $vm.ResourceGroupName -SqlManagementType Full
  ```

---

## <a name="verify-registration-status"></a>Kayıt durumunu doğrula
SQL Server VM SQL VM kaynak sağlayıcısı ile Azure portal, Azure CLı veya Azure PowerShell kullanarak zaten kaydedilmiş olduğunu doğrulayabilirsiniz. 

### <a name="azure-portal"></a>Azure portal 

1. [Azure Portal](https://portal.azure.com) oturum açın. 
1. [SQL Server sanal](manage-sql-vm-portal.md)makinelerinize gidin.
1. Listeden SQL Server VM seçin. SQL Server VM burada listelenmiyorsa, büyük olasılıkla SQL VM kaynak sağlayıcısına kayıtlı değildir. 
1. **Durum** altındaki değeri görüntüleyin. **Durum** **başarılı** olursa, SQL Server VM SQL VM kaynak sağlayıcısına başarıyla kaydedildi. 

   ![SQL RP kaydıyla durumu doğrulama](./media/sql-vm-resource-provider-register/verify-registration-status.png)

### <a name="command-line"></a>Komut satırı

Azure CLı veya Azure PowerShell kullanarak geçerli SQL Server VM kayıt durumunu doğrulayın. `ProvisioningState` , `Succeeded` kaydın başarılı olup olmadığını gösterir. 

# <a name="azure-cli"></a>[Azure CLI](#tab/bash)


  ```azurecli-interactive
  az sql vm show -n <vm_name> -g <resource_group>
 ```

# <a name="azure-powershell"></a>[Azure PowerShell](#tab/powershell)

  ```powershell-interactive
  Get-AzSqlVM -Name <vm_name> -ResourceGroupName <resource_group>
  ```

---

Bir hata, SQL Server VM kaynak sağlayıcıya kaydedilmemiş olduğunu gösterir. 


## <a name="unregister-from-rp"></a>RP 'den kayıt silme

SQL Server VM SQL VM kaynak sağlayıcısı ile kaydını silmek için, Azure portal veya Azure CLı kullanarak SQL sanal makine *kaynağını* silin. SQL sanal makine *kaynağını* silme, SQL Server VM silmez. Bununla birlikte, *kaynağı* kaldırmaya çalışırken yanlışlıkla sanal makineyi silmek mümkün olduğundan dikkatli olun ve adımları dikkatle izleyin. 

Yönetim modunun tam olarak indirgenmesini sağlamak için SQL VM kaynak sağlayıcısı ile SQL sanal makinesini silme işlemi gereklidir. 

### <a name="azure-portal"></a>Azure portal

Azure portal kullanarak SQL Server VM kaynak sağlayıcıyla kaydını silmek için şu adımları izleyin:

1. [Azure portalında](https://portal.azure.com) oturum açın.
1. SQL VM kaynağına gidin. 
  
   ![SQL sanal makineler kaynağı](./media/sql-vm-resource-provider-register/sql-vm-manage.png)

1. **Sil** ’i seçin. 

   ![Üstteki gezinmede Sil ' i seçin](./media/sql-vm-resource-provider-register/delete-sql-vm-resource-provider.png)

1. SQL sanal makinesinin adını yazın ve **sanal makinenin yanındaki onay kutusunu temizleyin**.

   ![Gerçek sanal makinenin silinmesini engellemek için VM 'nin işaretini kaldırın ve ardından SQL VM kaynağını silmeye devam etmek için Sil 'i seçin](./media/sql-vm-resource-provider-register/confirm-delete-of-resource-uncheck-box.png)

   >[!WARNING]
   > Sanal makine adının yanındaki onay kutusunun temizlenmemesi, sanal makineyi tamamen *siler* . Kaynak sağlayıcıdan SQL Server VM kaydını kaldırmak, ancak *gerçek sanal makineyi silmek* için onay kutusunu temizleyin. 

1. SQL Server VM değil, SQL sanal makine *kaynağını* silmeyi onaylamak için **Sil** ' i seçin. 

### <a name="command-line"></a>Komut satırı

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)
Azure CLı ile SQL Server VM kaynak sağlayıcıdan kaydını silmek için [az SQL VM Delete](/cli/azure/sql/vm?view=azure-cli-latest&preserve-view=true#az-sql-vm-delete) komutunu kullanın. Bu işlem SQL Server VM *kaynağını* kaldırır ancak sanal makineyi silmez. 


```azurecli-interactive
az sql vm delete 
  --name <SQL VM resource name> |
  --resource-group <Resource group name> |
  --yes 
```

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)
Kaynak sağlayıcıdan Azure PowerShell SQL Server VM kaydını silmek için [Remove-AzSqlVM](/powershell/module/az.sqlvirtualmachine/remove-azsqlvm)komutunu kullanın. Bu işlem SQL Server VM *kaynağını* kaldırır ancak sanal makineyi silmez. 

```powershell-interactive
Remove-AzSqlVM -ResourceGroupName <resource_group_name> -Name <VM_name>
```

---



## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi için aşağıdaki makalelere bakın: 

* [Windows VM 'de SQL Server genel bakış](sql-server-on-azure-vm-iaas-what-is-overview.md)
* [Windows VM 'de SQL Server hakkında SSS](frequently-asked-questions-faq.md)  
* [Windows VM üzerinde SQL Server için fiyatlandırma Kılavuzu](pricing-guidance.md)
* [Windows VM 'de SQL Server için sürüm notları](../../database/doc-changes-updates-release-notes.md)
