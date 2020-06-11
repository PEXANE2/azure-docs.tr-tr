---
title: Azure 'da bir SQL VM için lisans modelini değiştirme
description: Azure 'daki bir SQL Server VM için lisanslamayı, Azure Hibrit Avantajı kullanarak kendi lisansınıza Kullandıkça öde ' ye nasıl yükleyeceğinizi öğrenin.
services: virtual-machines-windows
documentationcenter: na
author: MashaMSFT
tags: azure-resource-manager
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 11/13/2019
ms.author: mathoma
ms.reviewer: jroth
ms.openlocfilehash: f02f31e0fc8943682af77ca6f506d15f36e88146
ms.sourcegitcommit: eeba08c8eaa1d724635dcf3a5e931993c848c633
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/10/2020
ms.locfileid: "84668912"
---
# <a name="change-the-license-model-for-a-sql-virtual-machine-in-azure"></a>Azure 'da bir SQL sanal makinesi için lisans modelini değiştirme
[!INCLUDE[appliesto-sqlvm](../../includes/appliesto-sqlvm.md)]


Bu makalede, Azure 'daki bir SQL Server sanal makinesi (VM) için lisans modelinin, **Microsoft. SqlVirtualMachine**adlı yeni SQL Server VM kaynak sağlayıcısını kullanarak nasıl değiştirileceği açıklanır.

SQL Server barındıran bir VM için üç lisans modeli vardır: Kullandıkça öde, Azure Hibrit Avantajı (AHB) ve olağanüstü durum kurtarma (DR). SQL Server VM lisans modelini, Azure portal, Azure CLı veya PowerShell kullanarak değiştirebilirsiniz. 

- **Kullandıkça Öde** modeli, Azure VM 'yi çalıştırmanın ikinci başına maliyetinin SQL Server lisansın maliyetini içerir.
- [Azure hibrit avantajı](https://azure.microsoft.com/pricing/hybrid-benefit/) , SQL Server ÇALıŞTıRAN bir VM ile kendi SQL Server lisansınızı kullanmanıza olanak sağlar. 
- **Olağanüstü durum kurtarma** lisans türü, Azure 'DAKI [ücretsiz Dr çoğaltması](business-continuity-high-availability-disaster-recovery-hadr-overview.md#free-dr-replica-in-azure) için kullanılır. 

Azure Hibrit Avantajı, Azure sanal makinelerinde yazılım güvencesi ("nitelikli lisans") ile SQL Server lisansların kullanılmasına izin verir. Azure Hibrit Avantajı, müşteriler bir sanal makinede SQL Server Lisansı kullanımı için ücretlendirilmez. Ancak yine de temel alınan bulut işlem maliyeti (yani, taban fiyat), depolama ve yedeklemeler için ücret ödemelidir. Ayrıca, Hizmetleri kullanımıyla ilişkili g/ç için de ödeme yapılmalıdır (geçerli olduğu gibi).

Microsoft Ürün koşullarına göre: "müşteriler Azure 'da iş yüklerini yapılandırırken SQL Server Azure hibrit avantajı altındaki Azure SQL veritabanı (yönetilen örnek, Elastik Havuz ve Tek Veritabanı), Azure Data Factory, SQL Server Integration Services veya SQL Server sanal makineler kullandığını göstermelidir."

Azure VM 'de SQL Server Azure Hibrit Avantajı kullanımını ve uyumlu olduğunu göstermek için üç seçeneğiniz vardır:

- Azure Marketi 'nden kendi lisansını getir SQL Server görüntüsünü kullanarak bir sanal makine sağlayın. Bu seçenek yalnızca Kurumsal Anlaşma olan müşteriler için kullanılabilir.
- Azure Marketi 'nden Kullandıkça Öde SQL Server görüntüsünü kullanarak bir sanal makine sağlayın ve Azure Hibrit Avantajı etkinleştirin.
- Azure VM 'de kendi kendine SQL Server, [SQL VM kaynak sağlayıcısına](sql-vm-resource-provider-register.md)el ile kaydolun ve Azure hibrit avantajı etkinleştirin.

SQL Server lisans türü, VM sağlandığında veya daha sonra dilediğiniz zaman yapılandırılabilir. Lisans modelleri arasında geçiş kesinti olmaz, VM 'yi veya SQL Server hizmetini yeniden başlatmaz, hiçbir ek maliyet eklemez ve hemen geçerli olur. Aslında Azure Hibrit Avantajı etkinleştirilmesi maliyeti *azaltır* .

## <a name="prerequisites"></a>Ön koşullar

SQL Server VM lisanslama modelinin değiştirilmesi aşağıdaki gereksinimlere sahiptir: 

- Bir [Azure aboneliği](https://azure.microsoft.com/free/).
- [SQL VM kaynak sağlayıcısına](sql-vm-resource-provider-register.md)kayıtlı bir [SQL Server VM](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-server-provision) .
- [Yazılım güvencesi](https://www.microsoft.com/licensing/licensing-programs/software-assurance-default) [Azure hibrit avantajı](https://azure.microsoft.com/pricing/hybrid-benefit/)kullanmak için bir gereksinimdir. 


## <a name="vms-already-registered-with-the-resource-provider"></a>Kaynak sağlayıcısına zaten kayıtlı VM 'Ler 

# <a name="the-azure-portal"></a>[Azure portal](#tab/azure-portal)

[!INCLUDE [windows-virtual-machines-sql-use-new-management-blade](../../../../includes/windows-virtual-machines-sql-new-resource.md)]

Lisans modelini doğrudan portaldan değiştirebilirsiniz: 

1. [Azure Portal](https://portal.azure.com) açın ve SQL Server VM [SQL sanal makineler kaynağını](manage-sql-vm-portal.md#access-the-sql-virtual-machines-resource) açın. 
1. **Ayarlar**altında **Yapılandır** ' ı seçin. 
1. **Azure hibrit avantajı** seçeneğini belirleyin ve yazılım güvencesi ile SQL Server lisansınızın olduğunu onaylamak için onay kutusunu işaretleyin. 
1. **Yapılandır** sayfasının alt kısmındaki **Uygula** ' yı seçin. 

![Portalda Azure Hibrit Avantajı](./media/licensing-model-azure-hybrid-benefit-ahb-change/ahb-in-portal.png)


# <a name="the-azure-cli"></a>[Azure CLI](#tab/azure-cli)

Azure CLı 'yi, lisans modelinizi değiştirmek için kullanabilirsiniz.  


**Azure Hibrit Avantajı**

```azurecli-interactive
# Switch your SQL Server VM license from pay-as-you-go to bring-your-own
# example: az sql vm update -n AHBTest -g AHBTest --license-type AHUB

az sql vm update -n <VMName> -g <ResourceGroupName> --license-type AHUB
```

**Kullandıkça Öde**: 

```azurecli-interactive
# Switch your SQL Server VM license from bring-your-own to pay-as-you-go
# example: az sql vm update -n AHBTest -g AHBTest --license-type PAYG

az sql vm update -n <VMName> -g <ResourceGroupName> --license-type PAYG
```

**Olağanüstü durum kurtarma (DR)**

```azurecli-interactive
# Switch your SQL Server VM license from bring-your-own to pay-as-you-go
# example: az sql vm update -n AHBTest -g AHBTest --license-type DR

az sql vm update -n <VMName> -g <ResourceGroupName> --license-type DR
```

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

PowerShell 'i, lisans modelinizi değiştirmek için kullanabilirsiniz.

**Azure Hibrit Avantajı**

```powershell-interactive
# Switch your SQL Server VM license from pay-as-you-go to bring-your-own
Update-AzSqlVM -ResourceGroupName <resource_group_name> -Name <VM_name> -LicenseType AHUB
```

**Kullandıkça öde**

```powershell-interactive
# Switch your SQL Server VM license from bring-your-own to pay-as-you-go
Update-AzSqlVM -ResourceGroupName <resource_group_name> -Name <VM_name> -LicenseType PAYG
```

**Olağanüstü Durum Kurtarma** 

```powershell-interactive
# Switch your SQL Server VM license from bring-your-own to pay-as-you-go
Update-AzSqlVM -ResourceGroupName <resource_group_name> -Name <VM_name> -LicenseType DR
```

---

## <a name="vms-not-registered-with-the-resource-provider"></a>Kaynak sağlayıcısına kayıtlı olmayan VM 'Ler

Kullandıkça Öde Azure Marketi görüntülerinden bir SQL Server VM sağladıysanız, SQL Server lisans türü Kullandıkça Öde olacaktır. Azure Marketi 'nden kendi lisansını getir görüntüsünü kullanarak bir SQL Server VM sağladıysanız, lisans türü AHUB olur. Varsayılan (Kullandıkça öde) veya kendi lisansını getir Azure Marketi görüntülerinin sağlandığı tüm SQL Server VM 'Ler otomatik olarak SQL VM kaynak sağlayıcısına kaydedilir ve bu sayede [lisans türünü](#vms-already-registered-with-the-resource-provider)değiştirebilirler.

Azure VM üzerinde SQL Server Azure Hibrit Avantajı aracılığıyla yalnızca kendi kendine yüklemeye uygunsunuz. Bu VM 'Leri, Microsoft Ürün koşullarına göre Azure Hibrit Avantajı kullanımını göstermek için SQL Server lisansını Azure Hibrit Avantajı olarak ayarlayarak [SQL VM kaynak sağlayıcısı ile kaydetmeniz](sql-vm-resource-provider-register.md) gerekir.

SQL Server VM lisans türünü, yalnızca SQL Server VM SQL VM kaynak sağlayıcısı 'na kayıtlıysa, Kullandıkça Öde veya Azure Hibrit Avantajı olarak değiştirebilirsiniz.

## <a name="remarks"></a>Açıklamalar

- Azure bulut çözümü sağlayıcısı (CSP) müşterileri, önce bir Kullandıkça Öde sanal makinesi dağıtarak ve ardından etkin yazılım güvencesi varsa, kendi lisansını getir 'e dönüştürerek Azure Hibrit Avantajı kullanabilir.
- SQL Server VM kaynağınızı düşürülebiliyorsanız, görüntünün sabit kodlu lisans ayarına geri dönebilirsiniz. 
- Lisans modelini değiştirme özelliği, SQL VM kaynak sağlayıcısı 'nın bir özelliğidir. Azure Marketi görüntüsünü Azure portal aracılığıyla dağıtmak, SQL Server VM otomatik olarak kaynak sağlayıcısıyla kaydeder. Ancak kendi kendini yükleyen SQL Server müşterilerin SQL Server VM el ile [kaydetmesi](sql-vm-resource-provider-register.md)gerekir. 
- Bir kullanılabilirlik kümesine SQL Server VM eklendiğinde VM 'nin yeniden oluşturulması gerekir. Bu nedenle, bir kullanılabilirlik kümesine eklenen tüm VM 'Ler varsayılan Kullandıkça Öde lisans türüne geri döner. Azure Hibrit Avantajı yeniden etkinleştirilmesi gerekir. 


## <a name="limitations"></a>Sınırlamalar

Lisans modelinin değiştirilmesi:
   - Yalnızca [yazılım güvencesi](https://www.microsoft.com/en-us/licensing/licensing-programs/software-assurance-overview)olan müşteriler tarafından kullanılabilir.
   - Yalnızca SQL Server Standard ve Enterprise sürümleri için desteklenir. Express, Web ve Developer için lisans değişiklikleri desteklenmez. 
   - Yalnızca Azure Resource Manager modeli aracılığıyla dağıtılan sanal makineler için desteklenir. Klasik model aracılığıyla dağıtılan sanal makineler desteklenmez. 
   - Yalnızca genel veya Azure Kamu bulutları için kullanılabilir. 
   - Yalnızca tek bir ağ arabirimine (NIC) sahip sanal makinelerde desteklenir. 


## <a name="known-errors"></a>Bilinen hatalar

### <a name="the-resource-microsoftsqlvirtualmachinesqlvirtualmachinesresource-group-under-resource-group-resource-group-was-not-found"></a>' ' Kaynak grubunun altındaki ' Microsoft. SqlVirtualMachine/SqlVirtualMachines/ \<resource-group> ' kaynağı \<resource-group> bulunamadı.

Bu hata, SQL VM kaynak sağlayıcısına kayıtlı olmayan bir SQL Server VM lisans modelini değiştirmeye çalıştığınızda oluşur:

`The Resource 'Microsoft.SqlVirtualMachine/SqlVirtualMachines/\<resource-group>' under resource group '\<resource-group>' was not found. The property 'sqlServerLicenseType' cannot be found on this object. Verify that the property exists and can be set.`

Aboneliğinizi kaynak sağlayıcısına kaydetmeniz ve ardından [SQL Server VM kaynak sağlayıcısına kaydetmeniz](sql-vm-resource-provider-register.md)gerekir. 


### <a name="the-virtual-machine-vmname-has-more-than-one-nic-associated"></a>' ' Sanal makinesi \<vmname\> ilişkili birden fazla NIC 'ye sahip

Bu hata, birden fazla NIC içeren sanal makinelerde oluşur. Lisanslama modelini değiştirmeden önce NIC 'lerden birini kaldırın. Lisans modelini değiştirdikten sonra NIC 'i sanal makineye geri ekleyebilseniz de, otomatik yedekleme ve düzeltme eki uygulama gibi Azure portal işlemler artık desteklenmeyecektir. 


## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi için aşağıdaki makaleleri inceleyin: 

* [Windows VM 'de SQL Server genel bakış](sql-server-on-azure-vm-iaas-what-is-overview.md)
* [Windows VM 'de SQL Server hakkında SSS](frequently-asked-questions-faq.md)
* [Windows VM üzerinde SQL Server için fiyatlandırma Kılavuzu](pricing-guidance.md)
* [Windows VM 'de SQL Server için sürüm notları](../../database/doc-changes-updates-release-notes.md)


