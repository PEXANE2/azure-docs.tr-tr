---
title: SQL VM kaynak sağlayıcısına kaydolun
description: Azure SQL Server sanal makinenizi SQL VM kaynak sağlayıcısı ile kaydedin ve Azure Market dışında dağıtılan SQL Server sanal makinelere yönelik özelliklerin yanı sıra uyumluluk ve gelişmiş yönetilebilirlik sağlar.
services: virtual-machines-windows
documentationcenter: na
author: MashaMSFT
tags: azure-resource-manager
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 11/13/2019
ms.author: mathoma
ms.reviewer: jroth
ms.custom: devx-track-azurecli, devx-track-azurepowershell
ms.openlocfilehash: 3f1a9a2756d81765d82938651672e5a83edc48ed
ms.sourcegitcommit: 656c0c38cf550327a9ee10cc936029378bc7b5a2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/28/2020
ms.locfileid: "89078699"
---
# <a name="register-a-sql-server-vm-in-azure-with-the-sql-vm-resource-provider-rp"></a>SQL VM kaynak sağlayıcısı (RP) ile Azure 'da bir SQL Server VM kaydetme
[!INCLUDE[appliesto-sqlvm](../../includes/appliesto-sqlvm.md)]

Bu makalede, SQL VM kaynak sağlayıcısı (RP) ile Azure 'da SQL Server sanal makinenizin (VM) nasıl kaydedileceği açıklanmaktadır. Kaynak sağlayıcısına kaydolmak, sanal makine kaynağından ayrı bir kaynak olan aboneliğinizdeki **SQL sanal makine** _kaynağını_ oluşturur. Kaynak sağlayıcıdan SQL Server VM kaydını silmek **SQL sanal makine** _kaynağını_ kaldıracak ancak gerçek sanal makineyi temizlemiyor. 

Azure portal üzerinden SQL Server VM Azure Marketi görüntüsünün dağıtımı, SQL Server VM kaynak sağlayıcısıyla otomatik olarak kaydeder. Bununla birlikte, bir Azure sanal makinesine SQL Server kendi kendine yüklemeyi veya özel bir VHD 'den bir Azure sanal makinesini sağlamayı seçerseniz, SQL Server VM için kaynak sağlayıcısına kaydolmanız gerekir:

- **Özellik avantajları**: SQL Server VM kaynak sağlayıcıyla kaydetme, [otomatik düzeltme eki uygulama](automated-patching.md), [otomatik yedekleme](automated-backup.md)ve izleme ve yönetilebilirlik yeteneklerini de kaldırır. Ayrıca [lisanslama](licensing-model-azure-hybrid-benefit-ahb-change.md) ve [Sürüm](change-sql-server-edition.md) esnekliğini de kaldırır. Daha önce, bu özellikler yalnızca Azure Marketi 'nden dağıtılan SQL Server VM görüntülerle sunulmaktadır. 

- **Uyumluluk**: SQL VM kaynak sağlayıcısı 'na kaydolmak, Microsoft 'un ürün koşullarında belirtilen Azure hibrit avantajı etkinleştirilmiş olduğunu bildirme gereksinimini karşılamakta olan basitleştirilmiş bir yöntem sunar. Bu işlem, her kaynak için lisanslama kayıt formlarını yönetmeye gerek duymasını geçersiz kılar.  

- **Ücretsiz yönetim**: SQL VM kaynak sağlayıcısı ile her üç yönetilebilirlik modunda kaydolma tamamen ücretsizdir. Kaynak sağlayıcısıyla veya değişen yönetim modlarıyla ilişkili ek bir maliyet yoktur. 

- **Basitleştirilmiş lisans yönetimi**: SQL VM kaynak sağlayıcısı 'na kaydolmak, SQL Server lisans yönetimini basitleştirir ve [Azure Portal](manage-sql-vm-portal.md), Azure clı veya PowerShell kullanarak Azure hibrit avantajı etkinleştirilen SQL Server sanal makineleri hızlıca tanımlamanızı sağlar: 

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

SQL VM kaynak sağlayıcısını kullanmak için, önce [aboneliğinizi kaynak sağlayıcısına kaydetmeniz](#register-subscription-with-rp)gerekir, bu da kaynak sağlayıcısına söz konusu abonelik içinde kaynak oluşturma yeteneği verir.

## <a name="prerequisites"></a>Önkoşullar

SQL Server VM kaynak sağlayıcısına kaydetmek için şunlar gerekir: 

- Bir [Azure aboneliği](https://azure.microsoft.com/free/).
- Azure kaynak modeli, genel veya Azure Kamu bulutuna dağıtılan [SQL Server VM](create-sql-vm-portal.md) . 
- [Azure CLI](/cli/azure/install-azure-cli) veya [PowerShell](/powershell/azure/new-azureps-module-az)'in en son sürümü. 

## <a name="management-modes"></a>Yönetim modları

[SQL IaaS uzantısı](sql-server-iaas-agent-extension-automate-management.md) zaten YÜKLENMEMIŞSE, SQL VM kaynak sağlayıcısına kaydolma, kayıt işlemi sırasında belirtilen üç yönetim modundan birine SQL Server IaaS uzantısını otomatik olarak yüklenir. Yönetim modunu belirtmeksizin SQL IaaS uzantısını tam yönetim modunda yüklersiniz.  

SQL IaaS uzantısı zaten el ile yüklenmişse, bu işlem zaten tam yönetim modunda ve kaynak sağlayıcısına tam modda kayıt, SQL Server hizmetini yeniden başlatmaz.

Üç yönetim modu:

- **Hafif** mod SQL Server yeniden başlatılmasını gerektirmez, ancak yalnızca SQL Server lisans türünü ve sürümünü değiştirmeyi destekler. Birden çok örneğe sahip veya bir yük devretme kümesi örneğine (FCı) katılan SQL Server VM 'Ler için bu seçeneği kullanın. Hafif mod kullanılırken bellek veya CPU üzerinde hiçbir etkisi yoktur ve ilişkili bir maliyet yoktur. SQL Server VM önce basit modda kaydetmeniz önerilir, sonra zamanlanmış bakım penceresi sırasında tam moda yükseltilir.  

- **Tam** mod tüm işlevleri sunar, ancak SQL Server ve Sistem Yöneticisi izinlerinin yeniden başlatılmasını gerektirir. Bu, varsayılan olarak SQL IaaS uzantısını el ile yüklerken yüklenmiş olan seçenektir. Tek bir örnekle SQL Server VM yönetmek için kullanın. Tam mod belleği ve CPU üzerinde en az etkisi olan iki Windows hizmeti yüklüyor. Bunlar, Görev Yöneticisi aracılığıyla izlenebilir. Tüm yönetilebilirlik modunu kullanmayla ilişkili bir maliyet yoktur. 

- **Noagent** modu, Windows Server 2008 ' de yüklü SQL Server 2008 ve SQL Server 2008 R2 için ayrılmıştır. NoAgent modu kullanılırken bellek veya CPU üzerinde bir etkisi yoktur. NoAgent yönetilebilirlik modunu kullanmayla ilişkili bir maliyet yoktur. 

PowerShell kullanarak SQL Server IaaS aracınızın geçerli modunu görüntüleyebilirsiniz: 

  ```powershell-interactive
  # Get the SqlVirtualMachine
  $sqlvm = Get-AzSqlVM -Name $vm.Name  -ResourceGroupName $vm.ResourceGroupName
  $sqlvm.SqlManagementType
  ```

## <a name="register-subscription-with-rp"></a>Aboneliği RP ile kaydetme

SQL Server VM SQL VM kaynak sağlayıcısına kaydetmek için öncelikle aboneliğinizi kaynak sağlayıcısına kaydetmeniz gerekir. Bu, SQL VM kaynak sağlayıcısına aboneliğinizdeki kaynakları oluşturma olanağı sağlar.  Azure portal, Azure CLı veya PowerShell kullanarak bunu yapabilirsiniz.

### <a name="azure-portal"></a>Azure portalı

1. Azure portal açın ve **tüm hizmetlere**gidin. 
1. **Abonelikler** ' e gidin ve ilgilendiğiniz aboneliği seçin.  
1. **Abonelikler** sayfasında, **kaynak sağlayıcıları**' na gidin. 
1. SQL ile ilgili kaynak sağlayıcılarını getirmek için filtreye **SQL** girin. 
1. İstediğiniz eyleme bağlı olarak **Microsoft. SqlVirtualMachine** sağlayıcısı için **Kaydet**, **yeniden kaydet**veya **kaydı kaldır** ' ı seçin. 

   ![Sağlayıcıyı değiştirme](./media/sql-vm-resource-provider-register/select-resource-provider-sql.png)


### <a name="command-line"></a>Komut satırı

Azure CLı veya PowerShell kullanarak SQL VM kaynak sağlayıcınızı Azure aboneliğinize kaydedin. 

# <a name="azure-cli"></a>[Azure CLI](#tab/bash)

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

## <a name="register-with-rp"></a>RP ile kaydolun

### <a name="lightweight-management-mode"></a>Hafif yönetim modu

[SQL Server IaaS Aracısı uzantısı](sql-server-iaas-agent-extension-automate-management.md) sanal makineye YÜKLENMEMIŞSE, SQL VM kaynak sağlayıcısına basit modda kaydolma önerisi de vardır. Bu işlem SQL IaaS uzantısını [hafif modda](#management-modes) yükleyecek ve SQL Server hizmetinin yeniden başlatılmasını engelleyecek. Dilediğiniz zaman tam moda yükseltebilirsiniz, ancak bunu yapmak SQL Server hizmeti 'ni yeniden başlatarak zamanlanan bir bakım penceresine kadar beklemeniz önerilir. 

`PAYG` `AHUB` `DR` [Ücretsiz Dr çoğaltma lisansını](business-continuity-high-availability-disaster-recovery-hadr-overview.md#free-dr-replica-in-azure)etkinleştirmek için, kullanım başına ödeme yapmak üzere Kullandıkça Öde (), kendi lisansınızı kullanmak için Azure hibrit avantajı () veya olağanüstü durum kurtarma () olarak SQL Server lisans türü sağlayın.

Yük devretme kümesi örnekleri ve çok örnekli dağıtımlar yalnızca SQL VM kaynak sağlayıcısı ile hafif modda kaydedilebilir. 

# <a name="azure-cli"></a>[Azure CLI](#tab/bash)

Azure CLı ile hafif modda bir SQL Server VM kaydetme: 

  ```azurecli-interactive
  # Register Enterprise or Standard self-installed VM in Lightweight mode
  az sql vm create --name <vm_name> --resource-group <resource_group_name> --location <vm_location> --license-type PAYG 
  ```


# <a name="powershell"></a>[PowerShell](#tab/powershell)

PowerShell ile hafif modda bir SQL Server VM kaydetme:  


  ```powershell-interactive
  # Get the existing compute VM
  $vm = Get-AzVM -Name <vm_name> -ResourceGroupName <resource_group_name>
          
  # Register SQL VM with 'Lightweight' SQL IaaS agent
  New-AzSqlVM -Name $vm.Name -ResourceGroupName $vm.ResourceGroupName -Location $vm.Location `
    -LicenseType PAYG -SqlManagementType LightWeight  
  ```

---

### <a name="full-management-mode"></a>Tam yönetim modu


SQL IaaS uzantısı zaten sanal makineye el ile yüklenmişse, SQL Server hizmetini yeniden başlatmadan SQL Server VM tam moda kaydedebilirsiniz. **Ancak, SQL IaaS uzantısı yüklenmemişse, tam modda kaydetme işlemi SQL IaaS uzantısını tam modda yükler ve SQL Server hizmetini yeniden başlatır. Lütfen dikkatli ilerleyin.**


SQL Server VM doğrudan tam modda kaydetmek için (ve SQL Server hizmetinizi büyük olasılıkla yeniden başlatmanız), aşağıdaki PowerShell komutunu kullanın: 

  ```powershell-interactive
  # Get the existing  Compute VM
  $vm = Get-AzVM -Name <vm_name> -ResourceGroupName <resource_group_name>
        
  # Register with SQL VM resource provider in full mode
  New-AzSqlVM -Name $vm.Name -ResourceGroupName $vm.ResourceGroupName -SqlManagementType Full
  ```

### <a name="noagent-management-mode"></a>NoAgent yönetim modu 

Windows Server 2008 (_R2 değil_) üzerinde yüklü olan SQL Server 2008 ve 2008 R2, [NOAGENT modundaki](#management-modes)SQL VM kaynak sağlayıcısı ile kaydedilebilir. Bu seçenek uyumluluk sağlar ve Azure portal sınırlı işlevlerle birlikte SQL Server VM izlenmesini sağlar.

`AHUB`, `PAYG` , Ya da `DR` **Sqllicensetype**olarak `SQL2008-WS2008` ya da `SQL2008R2-WS2008` **sqlimageteklifini**belirtin. 

SQL Server 2008 veya 2008 R2 'yi Windows Server 2008 örneğine kaydetmek için aşağıdaki Azure CLı veya PowerShell kod parçacığını kullanın: 


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

# <a name="powershell"></a>[PowerShell](#tab/powershell)

SQL Server 2008 sanal makinenizi PowerShell ile NoAgent modunda kaydedin: 


  ```powershell-interactive
  # Get the existing compute VM
  $vm = Get-AzVM -Name <vm_name> -ResourceGroupName <resource_group_name>
          
  New-AzSqlVM -Name $vm.Name -ResourceGroupName $vm.ResourceGroupName -Location $vm.Location `
    -LicenseType PAYG -SqlManagementType NoAgent -Sku Standard -Offer SQL2008-WS2008
  ```
  
  SQL Server 2008 R2 sanal makinenizi PowerShell ile NoAgent modunda kaydedin: 


  ```powershell-interactive
  # Get the existing compute VM
  $vm = Get-AzVM -Name <vm_name> -ResourceGroupName <resource_group_name>
          
  New-AzSqlVM -Name $vm.Name -ResourceGroupName $vm.ResourceGroupName -Location $vm.Location `
    -LicenseType PAYG -SqlManagementType NoAgent -Sku Standard -Offer SQL2008R2-WS2008
  ```

---

## <a name="upgrade-to-full"></a>Tam sürüme yükselt  

*Hafif* IaaS uzantısının yüklü olduğu SQL Server VM 'ler, Azure Portal, Azure CLI veya PowerShell kullanarak modu _tam_ olarak yükseltebilir. _Noagent_ modundaki SQL Server VM 'ler, Işletim sistemi Windows 2008 R2 ve üzeri sürümlere yükseltildikten sonra _tam_ sürümüne yükseltilebilir. Bunun için düşürme yapılamaz, bunun için SQL VM kaynak sağlayıcısı 'ndan SQL Server VM [kaydını](#unregister-from-rp) kaldırmanız gerekir. Bunun yapılması **SQL sanal makine** _kaynağını_kaldıracak, ancak gerçek sanal makineyi silmeyecektir. 

PowerShell kullanarak SQL Server IaaS aracınızın geçerli modunu görüntüleyebilirsiniz: 

  ```powershell-interactive
  # Get the SqlVirtualMachine
  $sqlvm = Get-AzSqlVM -Name $vm.Name  -ResourceGroupName $vm.ResourceGroupName
  $sqlvm.SqlManagementType
  ```

Aracı modunu tam olarak yükseltmek için: 


### <a name="azure-portal"></a>Azure portalı

1. [Azure Portal](https://portal.azure.com)’ında oturum açın.
1. [SQL sanal makineler](manage-sql-vm-portal.md#access-the-sql-virtual-machines-resource) kaynağına gidin. 
1. SQL Server VM seçin ve **genel bakış**' ı seçin. 
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

# <a name="powershell"></a>[PowerShell](#tab/powershell)

Aşağıdaki PowerShell kod parçacığını çalıştırın:

  ```powershell-interactive
  # Get the existing  Compute VM
  $vm = Get-AzVM -Name <vm_name> -ResourceGroupName <resource_group_name>
        
  # Register with SQL VM resource provider in full mode
  Update-AzSqlVM -Name $vm.Name -ResourceGroupName $vm.ResourceGroupName -SqlManagementType Full
  ```

---

## <a name="verify-registration-status"></a>Kayıt durumunu doğrula
SQL Server VM Azure portal, Azure CLı veya PowerShell kullanarak SQL VM kaynak sağlayıcısı ile zaten kaydedilmiş olup olmadığını doğrulayabilirsiniz. 

### <a name="azure-portal"></a>Azure portalı 

1. [Azure Portal](https://portal.azure.com)’ında oturum açın. 
1. [SQL Server sanal](manage-sql-vm-portal.md)makinelerinize gidin.
1. Listeden SQL Server VM seçin. SQL Server VM burada listelenmiyorsa, büyük olasılıkla SQL VM kaynak sağlayıcısına kayıtlı değildir. 
1. **Durum**altındaki değeri görüntüleyin. **Durum** **başarılı**olursa, SQL Server VM SQL VM kaynak sağlayıcısına başarıyla kaydedildi. 

   ![SQL RP kaydıyla durumu doğrulama](./media/sql-vm-resource-provider-register/verify-registration-status.png)

### <a name="command-line"></a>Komut satırı

Azure CLı veya PowerShell kullanarak geçerli SQL Server VM kayıt durumunu doğrulayın. `ProvisioningState` , `Succeeded` kaydın başarılı olup olmadığını gösterir. 

# <a name="azure-cli"></a>[Azure CLI](#tab/bash)


  ```azurecli-interactive
  az sql vm show -n <vm_name> -g <resource_group>
 ```

# <a name="powershell"></a>[PowerShell](#tab/powershell)

  ```powershell-interactive
  Get-AzSqlVM -Name <vm_name> -ResourceGroupName <resource_group>
  ```

---

Bir hata, SQL Server VM kaynak sağlayıcıya kaydedilmemiş olduğunu gösterir. 


## <a name="unregister-from-rp"></a>RP 'den kayıt silme

SQL Server VM SQL VM kaynak sağlayıcısı ile kaydını silmek için, Azure portal veya Azure CLı kullanarak SQL sanal makine *kaynağını* silin. SQL sanal makine *kaynağını* silme, SQL Server VM silmez. Bununla birlikte, *kaynağı*kaldırmaya çalışırken yanlışlıkla sanal makineyi silmek mümkün olduğundan dikkatli olun ve adımları dikkatle izleyin. 

Yönetim modunun tam olarak indirgenmesini sağlamak için SQL VM kaynak sağlayıcısı ile SQL sanal makinesini silme işlemi gereklidir. 

### <a name="azure-portal"></a>Azure portalı

Azure portal kullanarak SQL Server VM kaynak sağlayıcıyla kaydını silmek için şu adımları izleyin:

1. [Azure portalında](https://portal.azure.com) oturum açın.
1. SQL VM kaynağına gidin. 
  
   ![SQL sanal makineler kaynağı](./media/sql-vm-resource-provider-register/sql-vm-manage.png)

1. **Sil**’i seçin. 

   ![SQL VM kaynak sağlayıcısını Sil](./media/sql-vm-resource-provider-register/delete-sql-vm-resource-provider.png)

1. SQL sanal makinesinin adını yazın ve **sanal makinenin yanındaki onay kutusunu temizleyin**.

   ![SQL VM kaynak sağlayıcısını Sil](./media/sql-vm-resource-provider-register/confirm-delete-of-resource-uncheck-box.png)

   >[!WARNING]
   > Sanal makine adının yanındaki onay kutusunun temizlenmemesi, sanal makineyi tamamen *siler* . Kaynak sağlayıcıdan SQL Server VM kaydını kaldırmak, ancak *gerçek sanal makineyi silmek*için onay kutusunu temizleyin. 

1. SQL Server VM değil, SQL sanal makine *kaynağını*silmeyi onaylamak için **Sil** ' i seçin. 

### <a name="command-line"></a>Komut satırı

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)
Azure CLı ile SQL Server VM kaynak sağlayıcıdan kaydını silmek için [az SQL VM Delete](/cli/azure/sql/vm?view=azure-cli-latest#az-sql-vm-delete) komutunu kullanın. Bu işlem SQL Server VM *kaynağını* kaldırır ancak sanal makineyi silmez. 


```azurecli-interactive
az sql vm delete 
  --name <SQL VM resource name> |
  --resource-group <Resource group name> |
  --yes 
```

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)
Azure CLı ile SQL Server VM kaynak sağlayıcıdan kaydını silmek için [New-AzSqlVM](/powershell/module/az.sqlvirtualmachine/new-azsqlvm)komutunu kullanın. Bu işlem SQL Server VM *kaynağını* kaldırır ancak sanal makineyi silmez. 

```powershell-interactive
Remove-AzSqlVM -ResourceGroupName <resource_group_name> -Name <VM_name>
```

---

## <a name="limitations"></a>Sınırlamalar

SQL VM kaynak sağlayıcısı yalnızca şunları destekler:
- Azure Resource Manager aracılığıyla dağıtılan SQL Server VM 'Ler. Klasik model aracılığıyla dağıtılan SQL Server VM 'Ler desteklenmez. 
- Genel veya Azure Kamu bulutuna dağıtılan VM 'Ler SQL Server. Diğer özel veya kamu bulutlarına dağıtımlar desteklenmez. 


## <a name="frequently-asked-questions"></a>Sık sorulan sorular 

**Azure Marketi 'ndeki bir SQL Server görüntüsünden sağlanan SQL Server VM kaydetmem gerekir mi?**

Hayır. Microsoft, Azure Marketi 'ndeki SQL Server görüntülerden sağlanan VM 'Leri otomatik olarak kaydeder. SQL VM kaynak sağlayıcısına kaydolmak yalnızca VM 'nin Azure Marketi 'ndeki SQL Server görüntülerden *sağlanmaması* ve SQL Server kendi kendine yüklenmesi durumunda gereklidir.

**SQL VM kaynak sağlayıcısını tüm müşteriler kullanabilir mi?** 

Evet. Müşteriler, Azure Marketi 'nden bir SQL Server görüntüsü kullanmayan ve bunun yerine kendi kendine yüklenmiş SQL Server veya özel VHD 'leri getirdiklerinde SQL Server sanal makinelerini SQL VM kaynak sağlayıcısına kaydetmelidir. Tüm abonelik türlerinin (doğrudan, Kurumsal Anlaşma ve bulut çözümü sağlayıcısı) sahip olduğu VM 'Ler SQL VM kaynak sağlayıcısına kaydoabilir.

**SQL Server VM SQL Server IaaS uzantısı zaten yüklüyse SQL VM kaynak sağlayıcısına kaydolmalı mı?**

SQL Server VM kendi kendine yüklenmiş ve Azure Marketi 'ndeki SQL Server görüntülerden sağlanmamışsa, SQL Server IaaS uzantısını yüklemiş olsanız bile SQL VM kaynak sağlayıcısına kaydolmanız gerekir. SQL VM kaynak sağlayıcısına kaydolmak, Microsoft. SqlVirtualMachine türünde yeni bir kaynak oluşturur. SQL Server IaaS uzantısının yüklenmesi bu kaynağı oluşturmaz.

**SQL VM kaynak sağlayıcısına kaydolurken varsayılan yönetim modu nedir?**

SQL VM kaynak sağlayıcısına kayıt yaptığınızda varsayılan yönetim modu *dolu*' dır. SQL VM kaynak sağlayıcısına kaydoldığınızda SQL Server Management özelliği ayarlanmamışsa, mod tam yönetilebilirlik olarak ayarlanır ve SQL Server hizmetiniz yeniden başlatılır. Önce basit modda SQL VM kaynak sağlayıcısına kaydolmanız ve ardından bakım penceresi sırasında tam olarak yükseltmeniz önerilir. 

**SQL VM kaynak sağlayıcısına kaydolma önkoşulları nelerdir?**

Basit modda veya aracı olmayan bir modda SQL VM kaynak sağlayıcısı 'na kaydolmak için herhangi bir önkoşul yoktur. SQL VM kaynak sağlayıcısına tam modda kaydolma önkoşulu, SQL Server hizmeti yeniden başlatıldığında SQL Server IaaS uzantısının VM 'de yüklü olmasını sağlar. 

**VM 'ye SQL Server IaaS uzantısı yüklü değilse SQL VM kaynak sağlayıcısı ile kayıt yapabilir miyim?**

Evet, VM 'ye SQL Server IaaS uzantısı yüklü değilse, basit yönetim modunda SQL VM kaynak sağlayıcısına kaydolabilirsiniz. Hafif modda SQL VM kaynak sağlayıcısı, SQL Server örneğinin sürümünü ve sürümünü doğrulamak için bir konsol uygulaması kullanacaktır. 

SQL VM kaynak sağlayıcısına kaydolurken varsayılan SQL yönetim modu _dolu_. SQL yönetim özelliği SQL VM kaynak sağlayıcısı ile kaydolurken ayarlanmamışsa, mod tam yönetilebilirlik olarak ayarlanır. Önce basit modda SQL VM kaynak sağlayıcısına kaydolmanız ve ardından bakım penceresi sırasında tam olarak yükseltmeniz önerilir. 

**SQL VM kaynak sağlayıcısı ile kaydolacaktır, VM 'imde bir aracı yükler mi?**

Evet, SQL VM kaynak sağlayıcısına kaydolmak VM 'ye bir aracı yükler.

SQL Server IaaS uzantısı, SQL Server meta verilerini sorgulamak için aracıya bağımlıdır. Bir aracının yüklü olmadığı tek zaman SQL VM kaynak sağlayıcısı 'nın NoAgent modunda olması

**VM 'imde SQL VM kaynak sağlayıcısı yeniden başlatma SQL Server kaydedilecek mı?**

Kayıt sırasında belirtilen moda bağlıdır. Basit veya NoAgent modu belirtilmişse SQL Server hizmeti yeniden başlatmaz. Bununla birlikte, yönetim modunun tam olarak belirtilmesi veya yönetim modunun boş bırakılması, SQL IaaS uzantısını tam yönetim modunda yükler, bu da SQL Server hizmetinin yeniden başlatılmasına neden olur. 

**SQL VM kaynak sağlayıcısı ile kaydolurken basit ve olmayan aracı yönetimi modları arasındaki fark nedir?** 

Hiçbir aracı yönetim modu, Windows Server 2008 üzerinde yalnızca SQL Server 2008 ve SQL Server 2008 R2 için kullanılabilir. Bu sürümler için kullanılabilen tek yönetim modu vardır. Tüm SQL Server diğer sürümleri için, kullanılabilir iki yönetilebilirlik modu hafif ve tamamludur. 

Aracı modu, SQL Server sürüm ve sürüm özelliklerinin müşteri tarafından ayarlanmasını gerektirir. Hafif mod, SQL Server örneğinin sürümünü ve sürümünü bulmak için VM 'yi sorgular.

**SQL Server lisans türünü belirtmeden SQL VM kaynak sağlayıcısı ile kayıt yapabilir miyim?**

Hayır. SQL Server lisans türü, SQL VM kaynak sağlayıcısına kaydolurken isteğe bağlı bir özellik değildir. SQL Server lisans türünü, tüm yönetilebilirlik modlarında (Aracı, hafif ve tam) SQL VM kaynak sağlayıcısı 'na kaydolurken Kullandıkça Öde veya Azure Hibrit Avantajı olarak ayarlamanız gerekir.

**SQL Server IaaS uzantısını aracı olmayan moddan tam moda yükseltebilir miyim?**

Hayır. Yönetilebilirlik modunu tam veya hafif olarak yükseltmek, aracı olmayan mod için kullanılamaz. Bu, Windows Server 2008 ' in teknik sınırlamasıdır. Önce işletim sistemini önce Windows Server 2008 R2 veya sonraki bir sürüme yükseltmeniz gerekir, sonra tam yönetim moduna yükseltebilirsiniz. 

**SQL Server IaaS uzantısını hafif moddan tam moda yükseltebilir miyim?**

Evet. Yönetilebilirlik modunu hafif ' den Full ' a yükseltmek PowerShell veya Azure portal ile desteklenir. SQL Server hizmeti 'nin yeniden başlatılmasını gerektirir.

**SQL Server IaaS uzantısını tam moddan aracı olmayan veya hafif yönetim moduna indirgeyebilirim miyim?**

Hayır. SQL Server IaaS uzantısının yönetilebilirlik modunu eski sürüme düşürme desteklenmez. Yönetilebilirlik modu, tam moddan hafif veya aracı olmayan moda düşürülemez ve hafif moddan aracı olmayan moda düşürülemez. 

Yönetilebilirlik modunu tam yönetilebilirlik olarak değiştirmek için, SQL Server *kaynağını* BıRAKARAK SQL Server VM SQL VM kaynak sağlayıcısı 'ndan [KAYDıNı KALDıRıN](#unregister-from-rp) ve SQL vm kaynak sağlayıcısı ile SQL Server VM farklı bir yönetim modunda yeniden kaydedin.

**Azure portal SQL VM kaynak sağlayıcısı ile kayıt yapabilir miyim?**

Hayır. SQL VM kaynak sağlayıcısına kaydolma Azure portal kullanılamıyor. SQL VM kaynak sağlayıcısına kaydolmak yalnızca Azure CLı veya PowerShell ile desteklenir. 

**SQL Server yüklenmeden önce SQL VM kaynak sağlayıcısı ile bir VM 'yi kaydedebilir miyim?**

Hayır. VM, SQL VM kaynak sağlayıcısına başarıyla kaydolmak için en az bir SQL Server (veritabanı altyapısı) örneğine sahip olmalıdır. VM üzerinde SQL Server örneği yoksa, yeni Microsoft. SqlVirtualMachine kaynağı başarısız durumda olur.

**Birden çok SQL Server örneği varsa SQL VM kaynak sağlayıcısı ile bir VM 'yi kaydedebilir miyim?**

Evet. SQL VM kaynak sağlayıcısı yalnızca bir SQL Server (veritabanı altyapısı) örneğini kaydeder. SQL VM kaynak sağlayıcısı, birden çok örnek olması durumunda varsayılan SQL Server örneğini kaydeder. Varsayılan örnek yoksa, yalnızca hafif modda kaydolma desteklenir. Hafif ve tam yönetilebilirlik moduna yükseltmek için, varsayılan SQL Server örneği var olmalıdır veya VM yalnızca bir adlandırılmış SQL Server örneğine sahip olmalıdır.

**SQL Server yük devretme kümesi örneğini SQL VM kaynak sağlayıcısı ile kaydedebilir miyim?**

Evet. Azure VM 'de yük devretme kümesi örnekleri SQL Server, SQL VM kaynak sağlayıcısına hafif modda kaydedilebilir. Ancak, SQL Server yük devretme kümesi örnekleri tam yönetilebilirlik moduna yükseltilemez.

**Her zaman açık kullanılabilirlik grubu yapılandırılmışsa VM 'imi SQL VM kaynak sağlayıcısı ile kaydedebilir miyim?**

Evet. Her zaman açık kullanılabilirlik grubu yapılandırmasına katılıyorsanız, SQL VM kaynak sağlayıcısı ile bir Azure VM 'de SQL Server örneğini kaydetme kısıtlaması yoktur.

**SQL VM kaynak sağlayıcısına kaydolma veya tam yönetilebilirlik moduna yükseltme maliyeti nedir?**
Yok. SQL VM kaynak sağlayıcısı ile veya üç yönetilebilirlik modundan birini kullanarak kayıt ile ilişkili bir ücret alınmaz. SQL Server VM kaynak sağlayıcıyla yönetme tamamen ücretsizdir. 

**Farklı yönetilebilirlik modlarını kullanmanın performans etkisi nedir?**
*Noagent* ve *hafif* yönetilebilirlik modları kullanılırken hiçbir etkisi yoktur. İşletim sistemine yüklenen iki hizmetten *tam* yönetilebilirlik modunu kullanırken en az etkisi vardır. Bunlar, Görev Yöneticisi ile izlenebilir ve yerleşik Windows Hizmetleri konsolunda görülebilir. 

İki hizmet adı şunlardır:
- `SqlIaaSExtensionQuery` (Görünen ad- `Microsoft SQL Server IaaS Query Service` )
- `SQLIaaSExtension` (Görünen ad- `Microsoft SQL Server IaaS Agent` )


## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi için aşağıdaki makaleleri inceleyin: 

* [Windows VM 'de SQL Server genel bakış](sql-server-on-azure-vm-iaas-what-is-overview.md)
* [Windows VM 'de SQL Server hakkında SSS](frequently-asked-questions-faq.md)  
* [Windows VM üzerinde SQL Server için fiyatlandırma Kılavuzu](pricing-guidance.md)
* [Windows VM 'de SQL Server için sürüm notları](../../database/doc-changes-updates-release-notes.md)
