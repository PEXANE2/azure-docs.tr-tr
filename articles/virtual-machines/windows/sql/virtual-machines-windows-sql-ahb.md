---
title: Azure'da bir SQL Server VM'nin lisans modelini değiştirme
description: Azure'daki bir SQL Server sanal makinesinin lisansını kullandıkça öde'den kendi lisansınızı getire kadar Azure'daki bir SQL Server sanal makinesinin lisansını azure Karma Avantajı'nı kullanarak nasıl değiştirebilirsiniz öğrenin.
services: virtual-machines-windows
documentationcenter: na
author: MashaMSFT
manager: jroth
tags: azure-resource-manager
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 11/13/2019
ms.author: mathoma
ms.reviewer: jroth
ms.openlocfilehash: 502d1fe599accb29ccc99c9e527f8d1c8e1d52b8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77201840"
---
# <a name="change-the-license-model-for-a-sql-server-virtual-machine-in-azure"></a>Azure'da bir SQL Server sanal makinesinin lisans modelini değiştirme
Bu makalede, yeni SQL VM kaynak sağlayıcısı **Microsoft.SqlVirtualMachine**kullanarak Azure'daki bir SQL Server sanal makinesinin (VM) lisans modelini nasıl değiştireceğiniz açıklanmaktadır.

SQL Server'ı barındıran bir VM için üç lisans modeli vardır: istediğiniz kadar öde, Azure Karma Avantajı ve olağanüstü durum kurtarma (DR). Azure portalını, Azure CLI'yi veya PowerShell'i kullanarak SQL Server VM'nizin lisans modelini değiştirebilirsiniz. 

- Olabildiğince **çok öde** modeli, Azure VM'yi çalıştırmanın saniye başına maliyetinin SQL Server lisansının maliyetini içerdiği anlamına gelir.
- [Azure Karma Avantajı,](https://azure.microsoft.com/pricing/hybrid-benefit/) SQL Server çalıştıran bir VM ile kendi SQL Server lisansınızı kullanmanıza olanak tanır. 
- **Olağanüstü durum kurtarma** lisans türü, Azure'daki ücretsiz DR [yinelemesi](virtual-machines-windows-sql-high-availability-dr.md#free-dr-replica-in-azure) için kullanılır. 

Azure Karma Avantajı, Azure sanal makinelerinde Yazılım Güvencesi ("Nitelikli Lisans") ile SQL Server lisanslarının kullanılmasına olanak tanır. Azure Karma Avantajı ile müşterilerden VM'de SQL Server lisansı kullanımı için ücret alınmaz. Ancak yine de temel bulut işlem maliyeti (yani, taban oranı), depolama ve yedekleme için ödeme gerekir. Ayrıca, hizmetlerin kullanımıyla ilişkili G/Ç ücretini de ödemelidirler (mümkün olduğu şekilde).

Microsoft Ürün Koşullarına göre: "Müşteriler Azure SQL Veritabanı (Yönetilen Örnek, Elastik Havuz ve Tek Veritabanı), Azure Veri Fabrikası, SQL Server Tümleştirme Hizmetleri veya Azure altında SQL Server Sanal Makineleri kullandıklarını belirtmelidir Azure'da iş yüklerini yapılandırırken SQL Server için Karma Avantaj."

SQL Server için Azure VM'de Azure Karma Avantajı'nın kullanımını belirtmek ve uyumlu olmak için üç seçeneğiniz var:

- Azure Marketi'nden kendi lisansınızı getir SQL Server görüntüsünü kullanarak sanal bir makine sağlama. Bu seçenek yalnızca Kurumsal Sözleşmesi olan müşteriler için kullanılabilir.
- Azure Marketi'nden kullandıkça öde SQL Server görüntüsünü kullanarak sanal bir makine sağlama ve Azure Karma Avantajı'nı etkinleştirme.
- SQL Server'ı Azure VM'ye kendi kendine yükleyin, [SQL VM kaynak sağlayıcısına](virtual-machines-windows-sql-register-with-resource-provider.md)el ile kaydolun ve Azure Karma Avantajı'nı etkinleştirin.

VM sağlandığında SQL Server'ın lisans türü ayarlanır. İstediğiniz zaman değiştirebilirsiniz. Lisans modelleri arasında geçiş hiçbir kesinti ye tabi değildir, VM veya SQL Server hizmetini yeniden başlatmaz, ek maliyet eklemez ve hemen geçerli olur. Aslında, Azure Karma Avantajı'nı etkinleştirme maliyeti *azaltır.*

## <a name="prerequisites"></a>Ön koşullar

SQL Server VM'nizin lisans modelini değiştirmek için aşağıdaki gereksinimler vardır: 

- [Azure aboneliği.](https://azure.microsoft.com/free/)
- [SQL VM kaynak sağlayıcısına](virtual-machines-windows-sql-register-with-resource-provider.md)kayıtlı bir SQL Server [VM.](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-server-provision)
- [Yazılım Güvencesi,](https://www.microsoft.com/licensing/licensing-programs/software-assurance-default) Azure Karma [Avantajı'nı](https://azure.microsoft.com/pricing/hybrid-benefit/)kullanmak için bir gerekliliktir. 


## <a name="vms-already-registered-with-the-resource-provider"></a>Kaynak sağlayıcısına zaten kayıtlı olan VM'ler 

# <a name="portal"></a>[Portal](#tab/azure-portal)

[!INCLUDE [windows-virtual-machines-sql-use-new-management-blade](../../../../includes/windows-virtual-machines-sql-new-resource.md)]

Lisans modelini doğrudan portaldan değiştirebilirsiniz: 

1. Azure [portalını](https://portal.azure.com) açın ve SQL Server VM'iniz için [SQL sanal makineler kaynağını](virtual-machines-windows-sql-manage-portal.md#access-the-sql-virtual-machines-resource) açın. 
1. **Ayarlar**altında **Yapılandır'ı** seçin. 
1. Azure **Karma Avantajı** seçeneğini seçin ve Yazılım Güvencesi ile sql server lisansına sahip olduğunuzu doğrulamak için onay kutusunu seçin. 
1. **Yapıla** sayfasının alt kısmında **Uygula'yı** seçin. 

![Portalda Azure Karma Avantajı](media/virtual-machines-windows-sql-ahb/ahb-in-portal.png)


# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

Lisans modelinizi değiştirmek için Azure CLI'yi kullanabilirsiniz.  


**Azure karma avantajı**

```azurecli-interactive
# Switch your SQL Server VM license from pay-as-you-go to bring-your-own
# example: az sql vm update -n AHBTest -g AHBTest --license-type AHUB

az sql vm update -n <VMName> -g <ResourceGroupName> --license-type AHUB
```

**Gittiğiniz gibi ödeyin:** 

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

# <a name="powershell"></a>[Powershell](#tab/azure-powershell)

Lisans modelinizi değiştirmek için PowerShell'i kullanabilirsiniz.

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

## <a name="vms-not-registered-with-the-resource-provider"></a>Kaynak sağlayıcısına kayıtlı olmayan VM'ler

Gittiğinizde öde Azure Marketi görüntülerinden bir SQL Server VM verdiyseniz, SQL Server lisans türü istediğiniz kadar öde olacaktır. Azure Marketi'nden kendi lisansınızı getir görüntüsünü kullanarak bir SQL Server VM sağlarsanız, lisans türü AHUB olacaktır. Varsayılan olarak (kullandıkça öde) veya kendi lisansınızı getir Azure Marketi görüntülerinden sağlanan tüm SQL Server VM'ler otomatik olarak SQL VM kaynak sağlayıcısına kaydedilir, böylece [lisans türünü](#vms-already-registered-with-the-resource-provider)değiştirebilirler.

SQL Server'ı yalnızca Azure Karma Avantajı üzerinden Azure VM'de kendi kendine yükleyebilirsiniz. Microsoft Ürün Koşulları'na göre Azure Karma Avantajı kullanımını belirtmek için, bu VM'leri SQL Server lisansını Azure Karma Avantajı olarak ayarlayarak [SQL VM kaynak sağlayıcısına](virtual-machines-windows-sql-register-with-resource-provider.md) kaydettirmelisiniz.

Sql Server VM'nin lisans türünü yalnızca SQL Server VM kaynak sağlayıcısına kayıtlıysa, kullandıkça öde veya Azure Karma Avantajı olarak değiştirebilirsiniz.

## <a name="remarks"></a>Açıklamalar

- Azure Bulut Çözüm Sağlayıcısı (CSP) müşterileri, azure karma avantajını önce kullandıkça öde VM dağıtarak ve ardından etkin Yazılım Güvencesi varsa kendi lisansınızı getir'e dönüştürerek kullanabilir.
- SQL Server VM kaynağınızı bırakırsanız, görüntünün sabit kodlu lisans ayarına geri dönersiniz. 
- Lisans modelini değiştirme yeteneği, SQL VM kaynak sağlayıcısının bir özelliğidir. Azure portalı üzerinden Azure Marketi görüntüsünü dağıtmak, kaynak sağlayıcısına otomatik olarak bir SQL Server VM kaydeder. Ancak SQL Server'ı kendi kendine yükleyen müşterilerin [SQL Server VM'lerini](virtual-machines-windows-sql-register-with-resource-provider.md)el ile kaydetmeleri gerekir. 
- Kullanılabilirlik kümesine SQL Server VM eklemek, VM'nin yeniden oluşturulmasını gerektirir. Bu nedenle, kullanılabilirlik kümesine eklenen tüm VM'ler varsayılan kullandıkça öde lisans türüne geri döner. Azure Karma Avantajı'nın yeniden etkinleştirilmesi gerekir. 


## <a name="limitations"></a>Sınırlamalar

Lisans modelini değiştirmek:
   - Yalnızca [Yazılım Güvencesi](https://www.microsoft.com/en-us/licensing/licensing-programs/software-assurance-overview)olan müşteriler tarafından kullanılabilir.
   - Yalnızca SQL Server'ın Standart ve Kurumsal sürümleri için desteklenir. Express, Web ve Developer için lisans değişiklikleri desteklenmez. 
   - Yalnızca Azure Kaynak Yöneticisi modeli aracılığıyla dağıtılan sanal makineler için desteklenir. Klasik model üzerinden dağıtılan sanal makineler desteklenmez. 
   - Yalnızca genel veya Azure Genel bulutlar için kullanılabilir. 
   - Yalnızca tek bir ağ arabirimi (NIC) olan sanal makinelerde desteklenir. 


## <a name="known-errors"></a>Bilinen hatalar

### <a name="the-resource-microsoftsqlvirtualmachinesqlvirtualmachinesresource-group-under-resource-group-resource-group-was-not-found"></a>Kaynak grubu 'kaynak\<\<grubu>' altında 'Microsoft.SqlVirtualMachine/SqlVirtualMachines/resource-group>' kaynağı bulunamadı.

Bu hata, SQL VM kaynak sağlayıcısına kayıtlı olmayan bir SQL Server VM'deki lisans modelini değiştirmeye çalıştığınızda oluşur:

`The Resource 'Microsoft.SqlVirtualMachine/SqlVirtualMachines/\<resource-group>' under resource group '\<resource-group>' was not found. The property 'sqlServerLicenseType' cannot be found on this object. Verify that the property exists and can be set.`

Aboneliğinizi kaynak sağlayıcısına kaydetmeniz ve ardından [SQL Server VM'nizi kaynak sağlayıcısına kaydetmeniz](virtual-machines-windows-sql-register-with-resource-provider.md)gerekir. 


### <a name="the-virtual-machine-vmname-has-more-than-one-nic-associated"></a>Sanal makine\<' vmname\>' birden fazla NIC ilişkili

Bu hata, birden fazla NIC'si olan sanal makinelerde oluşur. Lisans modelini değiştirmeden önce NIC'lerden birini kaldırın. Lisans modelini değiştirdikten sonra NIC'yi VM'ye geri ekleyebiliyor olsanız da, Azure portalındaki otomatik yedekleme ve düzeltme gibi işlemler artık desteklenmez. 


## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi için aşağıdaki makalelere bakın: 

* [Windows VM'de SQL Server'a Genel Bakış](virtual-machines-windows-sql-server-iaas-overview.md)
* [Windows VM'de SQL Server için SSS](virtual-machines-windows-sql-server-iaas-faq.md)
* [Windows VM'de SQL Server için fiyatlandırma kılavuzu](virtual-machines-windows-sql-server-pricing-guidance.md)
* [Windows VM'de SQL Server için sürüm notları](virtual-machines-windows-sql-server-iaas-release-notes.md)


