---
title: Azure 'da bir SQL Server VM için lisans modelini değiştirme
description: Azure 'daki bir SQL sanal makinesine yönelik lisanslamayı, Azure Hibrit Avantajı kullanarak "Kullandıkça öde" yerine "kendi lisansını getir" olarak nasıl değiştireceğinizi öğrenin.
services: virtual-machines-windows
documentationcenter: na
author: MashaMSFT
manager: jroth
tags: azure-resource-manager
ms.assetid: aa5bf144-37a3-4781-892d-e0e300913d03
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 08/05/2019
ms.author: mathoma
ms.reviewer: jroth
ms.openlocfilehash: 37457d8ce1189f9282f4763633e944e3c2d639c9
ms.sourcegitcommit: c8a102b9f76f355556b03b62f3c79dc5e3bae305
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/06/2019
ms.locfileid: "68816727"
---
# <a name="how-to-change-licensing-model-for-a-sql-server-virtual-machine-in-azure"></a>Azure 'da SQL Server bir sanal makine için lisans modelini değiştirme
Bu makalede, Azure 'daki bir SQL Server sanal makinesi için lisanslama modelinin **Microsoft. SqlVirtualMachine**ADLı yenı SQL VM kaynak sağlayıcısı kullanılarak nasıl değiştirileceği açıklanır.

SQL Server-Kullandıkça öde ve Azure Hibrit Avantajı (AHB) barındıran bir sanal makine (VM) için iki lisans modeli vardır. Artık Azure portal, Azure CLı veya PowerShell 'i kullanarak SQL Server VM lisans modelini değiştirebilirsiniz. 

**Kullandıkça Öde** (PAYG) modeli, Azure VM 'yi çalıştırmanın ikinci başına maliyetinin SQL Server lisansın maliyetini içerir.
[Azure hibrit avantajı (AHB)](https://azure.microsoft.com/pricing/hybrid-benefit/) , SQL Server ÇALıŞTıRAN bir VM ile kendi SQL Server lisansınızı kullanmanıza olanak tanır. 

SQL Server hibrit avantajı Microsoft Azure Azure sanal makinelerinde yazılım güvencesi ("nitelikli lisans") ile SQL Server lisansların kullanılmasına olanak tanır. SQL Server için Azure Hibrit Avantajı sayesinde, müşteriler VM 'de SQL Server lisansının kullanımı için ücretlendirilmeyecektir, ancak temel alınan bulut işlem maliyeti (yani, taban fiyatı), depolama alanı ve arka UPS 'nin yanı sıra u ile ilişkili g/ç maliyeti için de ödeme yapılmalıdır hizmet örneği (uygun şekilde).

Microsoft Ürün koşullarına bağlı olarak, müşterilerin Azure hibrit altında Azure SQL veritabanı (yönetilen örnek, Elastik Havuz ve Tek Veritabanı), Azure Data Factory, SQL Server Integration Services veya SQL Server sanal makineler kullandığını belirtmesi gerekir Azure 'da iş yüklerini yapılandırırken SQL Server avantajı. "

Azure sanal makinesinde SQL Server için Azure Hibrit Avantajı kullanımını göstermek ve uyumlu olmak için üç seçenek vardır:

1. Azure Marketi 'nden bir KLG SQL Server görüntüsünü kullanarak bir sanal makine sağlayın ve yalnızca Kurumsal Anlaşma olan müşteriler için kullanılabilir.
1. Azure Marketi 'nden bir PAYG SQL Server görüntüsü kullanarak bir sanal makine sağlayın ve AHB 'yi etkinleştirin.
1. Azure VM 'de kendi kendine SQL Server, SQL Server VM el ile [kaydedin](virtual-machines-windows-sql-register-with-resource-provider.md) ve AHB 'yi etkinleştirin.

SQL Server lisans türü, VM sağlandığında ayarlanır ve bundan sonra değişiklik yapılabilir. Lisans modelleri arasında geçiş **kesinti**yapmaz, sanal makineyi yeniden başlatmaz, **ek maliyet** eklemez (aslında AHB 'nin etkinleştirilmesi maliyeti *azaltır* ) ve **hemen geçerli**olur. 

## <a name="prerequisites"></a>Önkoşullar

SQL VM kaynak sağlayıcısı kullanımı SQL IaaS uzantısını gerektirir. Bu nedenle, SQL VM kaynak sağlayıcısı 'nı kullanmaya devam etmek için şunlar gerekir:
- Bir [Azure aboneliği](https://azure.microsoft.com/free/).
- [Yazılım güvencesi](https://www.microsoft.com/licensing/licensing-programs/software-assurance-default). 
- [SQL VM kaynak sağlayıcısı](virtual-machines-windows-sql-register-with-resource-provider.md) yüklenmiş bir [SQL Server VM](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-server-provision) . 


## <a name="change-license-for-vms-already-registered-with-resource-provider"></a>Kaynak sağlayıcısına zaten kayıtlı VM 'Ler için lisans değiştirme 

# <a name="azure-portaltabazure-portal"></a>[Azure portal](#tab/azure-portal)

[!INCLUDE [windows-virtual-machines-sql-use-new-management-blade](../../../../includes/windows-virtual-machines-sql-new-resource.md)]

Lisanslama modelini doğrudan portaldan değiştirebilirsiniz. 

1. [Azure Portal](https://portal.azure.com) açın ve SQL Server VM [SQL sanal makineler kaynağını](virtual-machines-windows-sql-manage-portal.md#access-sql-virtual-machine-resource) başlatın. 
1. **Ayarlar**altında **Yapılandır** ' ı seçin. 
1. **Azure hibrit avantajı** seçeneğini belirleyin ve yazılım güvencesi ile SQL Server lisansınızın olduğunu onaylamak için onay kutusunu işaretleyin. 
1. **Yapılandır** sayfasının alt kısmındaki **Uygula** ' yı seçin. 

![Portalda AHB](media/virtual-machines-windows-sql-ahb/ahb-in-portal.png)


# <a name="az-clitabbash"></a>[AZ CLI](#tab/bash)

Lisans modelinizi değiştirmek için Azure CLı kullanabilirsiniz.  

Aşağıdaki kod parçacığı, Kullandıkça Öde lisans modelinizi KLG (veya Azure Hibrit Avantajı kullanarak) olarak değiştirir:

```azurecli-interactive
# Switch your SQL Server VM license from pay-as-you-go to bring-your-own
# example: az sql vm update -n AHBTest -g AHBTest --license-type AHUB

az sql vm update -n <VMName> -g <ResourceGroupName> --license-type AHUB
```

Aşağıdaki kod parçacığı, kendi lisans modelinizi Kullandıkça Öde için değiştirir: 

```azurecli-interactive
# Switch your SQL Server VM license from bring-your-own to pay-as-you-go
# example: az sql vm update -n AHBTest -g AHBTest --license-type PAYG

az sql vm update -n <VMName> -g <ResourceGroupName> --license-type PAYG
```

# <a name="powershelltabpowershell"></a>[PowerShell](#tab/powershell)
Lisanslama modelinizi değiştirmek için PowerShell 'i kullanabilirsiniz.

Aşağıdaki kod parçacığı, Kullandıkça Öde lisans modelinizi KLG (veya Azure Hibrit Avantajı kullanarak) olarak değiştirir:

```powershell-interactive
# Switch your SQL Server VM license from pay-as-you-go to bring-your-own
#example: $SqlVm = Get-AzResource -ResourceType Microsoft.SqlVirtualMachine/SqlVirtualMachines -ResourceGroupName AHBTest -ResourceName AHBTest
$SqlVm = Get-AzResource -ResourceType Microsoft.SqlVirtualMachine/SqlVirtualMachines -ResourceGroupName <resource_group_name> -ResourceName <VM_name>
$SqlVm.Properties.sqlServerLicenseType="AHUB"
<# the following code snippet is only necessary if using Azure Powershell version > 4
$SqlVm.Kind= "LicenseChange"
$SqlVm.Plan= [Microsoft.Azure.Management.ResourceManager.Models.Plan]::new()
$SqlVm.Sku= [Microsoft.Azure.Management.ResourceManager.Models.Sku]::new() #>
$SqlVm | Set-AzResource -Force 
```

Aşağıdaki kod parçacığı KLG modelinize Kullandıkça Öde için geçiş yapar:

```powershell-interactive
# Switch your SQL Server VM license from bring-your-own to pay-as-you-go
#example: $SqlVm = Get-AzResource -ResourceType Microsoft.SqlVirtualMachine/SqlVirtualMachines -ResourceGroupName AHBTest -ResourceName AHBTest
$SqlVm = Get-AzResource -ResourceType Microsoft.SqlVirtualMachine/SqlVirtualMachines -ResourceGroupName <resource_group_name> -ResourceName <VM_name>
$SqlVm.Properties.sqlServerLicenseType="PAYG"
<# the following code snippet is only necessary if using Azure Powershell version > 4
$SqlVm.Kind= "LicenseChange"
$SqlVm.Plan= [Microsoft.Azure.Management.ResourceManager.Models.Plan]::new()
$SqlVm.Sku= [Microsoft.Azure.Management.ResourceManager.Models.Sku]::new() #>
$SqlVm | Set-AzResource -Force 
```
---

## <a name="change-license-for-vms-not-registered-with-resource-provider"></a>Kaynak sağlayıcısına kayıtlı olmayan VM 'Ler için lisans değiştirme

PAYG Azure Marketi görüntülerinden bir SQL Server VM sağladıysanız SQL lisans türü PAYG olur. Azure Marketi 'nden bir KLG görüntüsü kullanarak bir SQL Server VM sağladıysanız, lisans türü AHUB olur. Varsayılan (PAYG) veya KLG Azure Marketi görüntülerinden sağlanan tüm SQL Server VM 'Ler otomatik olarak SQL VM kaynak sağlayıcısına kaydedilir ve bu sayede [lisans türünü](#change-license-for-vms-already-registered-with-resource-provider) değiştirebilirler

Azure VM üzerinde SQL Server yalnızca Azure Hibrit Avantajı aracılığıyla kendi kendine yüklemeye uygunsunuz ve Microsoft Ürün koşullarına göre AHB kullanımını belirtmek için AHB olarak SQL Server lisansını ayarlayarak [Bu VM 'LERI SQL VM kaynak sağlayıcısı ile kaydetmeniz](virtual-machines-windows-sql-register-with-resource-provider.md) gerekir.

SQL VM, SQL VM kaynak sağlayıcısı ile kayıtlıysa bir SQL Server VM lisans türünü yalnızca PAYG veya AHB olarak değiştirebilirsiniz; ve tüm SQL VM 'lerinin lisans uyumluluğu için SQL VM RP 'ye kayıtlı olması gerekir.

## <a name="remarks"></a>Açıklamalar

 - Azure bulut çözümü Iş ortağı (CSP) müşterileri, önce bir Kullandıkça Öde sanal makinesi dağıtarak ve sonra etkin SA 'larsa kendi lisansını getir 'e dönüştürerek Azure Hibrit Avantajı kullanabilir.
 - SQL Server VM kaynağınızı düşürülebiliyorsanız, görüntünün sabit kodlu lisans ayarına geri dönebilirsiniz. 
  - Lisanslama modelini değiştirme özelliği, SQL VM kaynak sağlayıcısı 'nın bir özelliğidir. Azure portal aracılığıyla Market görüntüsü dağıtmak, SQL Server VM otomatik olarak kaynak sağlayıcısıyla kaydeder. Ancak, kendi kendini yükleyen SQL Server müşterilerin SQL Server VM el ile [kaydetmesi](virtual-machines-windows-sql-register-with-resource-provider.md)gerekir. 
- Bir kullanılabilirlik kümesine SQL Server VM eklemek VM 'nin yeniden oluşturulmasını gerektirir. Bu nedenle, bir kullanılabilirlik kümesine eklenen tüm VM 'Ler varsayılan Kullandıkça Öde lisans türüne geri döner ve AHB 'nin yeniden etkinleştirilmesi gerekir. 


## <a name="limitations"></a>Sınırlamalar

 - Lisanslama modelinin değiştirilmesi yalnızca yazılım güvencesi olan müşteriler tarafından kullanılabilir.
 - Lisanslama modelinin değiştirilmesi yalnızca SQL Server Standard ve Enterprise Edition için desteklenir. Express, Web ve geliştirici için lisans değişiklikleri desteklenmez. 
 - Lisanslama modelinin değiştirilmesi yalnızca Kaynak Yöneticisi modeli kullanılarak dağıtılan sanal makineler için desteklenir. Klasik model kullanılarak dağıtılan VM 'Ler desteklenmez. VM 'nizi klasik sunucudan Resource Manager (ARM) modeline geçirebilir ve SQL VM kaynak sağlayıcısına kaydolabilirsiniz. VM, SQL VM kaynak sağlayıcısına kaydedildikten sonra, VM 'de lisans modeli değişiklikleri kullanılabilir olacaktır. 
 - Lisanslama modelinin değiştirilmesi yalnızca genel bulut yüklemeleri için etkinleştirilmiştir.
 - Lisanslama modelinin değiştirilmesi yalnızca tek bir NIC (ağ arabirimi) olan sanal makinelerde desteklenir. Birden fazla NIC içeren sanal makinelerde, yordamı denemeden önce önce NIC 'lerden birini kaldırmanız gerekir (Azure portal kullanarak). Aksi takdirde, aşağıdakine benzer bir hata ile çalışmaya çalışacaktır: `The virtual machine '\<vmname\>' has more than one NIC associated.`Lisanslama modunu değiştirdikten sonra NIC 'i sanal makineye geri ekleyebilseniz de, otomatik düzeltme eki uygulama ve yedekleme gibi Azure portal SQL yapılandırma sayfasından gerçekleştirilen işlemler artık desteklenmeyecektir.


## <a name="known-errors"></a>Bilinen hatalar

### <a name="the-resource-microsoftsqlvirtualmachinesqlvirtualmachinesresource-group-under-resource-group-resource-group-was-not-found-the-property-sqlserverlicensetype-cannot-be-found-on-this-object-verify-that-the-property-exists-and-can-be-set"></a>\<'\<Resource-Group > ' kaynak grubundaki ' Microsoft. sqlvirtualmachine/sqlvirtualmachines/Resource-Group > ' kaynağı bulunamadı. ' SqlServerLicenseType ' özelliği bu nesnede bulunamıyor. Özelliğin var olduğunu ve ayarlanbildiğini doğrulayın.
Bu hata, SQL VM kaynak sağlayıcısına kayıtlı olmayan bir SQL Server VM lisans modelini değiştirmeye çalışırken oluşur. Kaynak sağlayıcısını [aboneliğinize](virtual-machines-windows-sql-register-with-resource-provider.md#register-sql-vm-resource-provider-with-subscription)kaydetmeniz ve sonra SQL Server VM SQL [kaynak sağlayıcısı](virtual-machines-windows-sql-register-with-resource-provider.md)ile kaydetmeniz gerekir. 

### <a name="cannot-validate-argument-on-parameter-sku"></a>' SKU ' parametresindeki bağımsız değişken doğrulanamıyor
Azure PowerShell > 4,0 ' i kullanırken SQL Server VM lisanslama modelinizi değiştirmeye çalışırken bu hatayla karşılaşabilirsiniz:`Set-AzResource: Cannot validate argument on parameter 'Sku'. The argument is null or empty. Provide an argument that is not null or empty, and then try the command again.`

Bu hatayı çözmek için, lisans modelinize geçiş yaparken, daha önce bahsedilen PowerShell kod parçacığındaki bu satırların açıklamasını kaldırın:

  ```powershell-interactive
  # the following code snippet is necessary if using Azure Powershell version > 4
  $SqlVm.Kind= "LicenseChange"
  $SqlVm.Plan= [Microsoft.Azure.Management.ResourceManager.Models.Plan]::new()
  $SqlVm.Sku= [Microsoft.Azure.Management.ResourceManager.Models.Sku]::new()
  ```
  
Azure PowerShell sürümünüzü doğrulamak için aşağıdaki kodu kullanın:
  
  ```powershell-interactive
  Get-Module -ListAvailable -Name Azure -Refresh
  ```

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi için aşağıdaki makalelere bakın: 

* [Windows VM 'de SQL Server genel bakış](virtual-machines-windows-sql-server-iaas-overview.md)
* [Windows VM 'de SQL Server SSS](virtual-machines-windows-sql-server-iaas-faq.md)
* [Windows VM fiyatlandırma kılavuzunda SQL Server](virtual-machines-windows-sql-server-pricing-guidance.md)
* [Windows VM sürüm notlarında SQL Server](virtual-machines-windows-sql-server-iaas-release-notes.md)


