---
title: SQL VM kaynak sağlayıcısı ile Azure 'da SQL Server sanal makinesini kaydetme | Microsoft Docs
description: Yönetilebilirlik arttırmak için SQL Server VM SQL VM kaynak sağlayıcısı ile kaydedin.
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
ms.date: 06/24/2019
ms.author: mathoma
ms.reviewer: jroth
ms.openlocfilehash: 95a167cbc8fde4488e3f46ffd850f0619cf1b651
ms.sourcegitcommit: 770b060438122f090ab90d81e3ff2f023455213b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/17/2019
ms.locfileid: "68305862"
---
# <a name="register-sql-server-virtual-machine-in-azure-with-the-sql-vm-resource-provider"></a>SQL VM kaynak sağlayıcısı ile Azure 'da SQL Server sanal makinesini kaydetme

Bu makalede, Azure SQL Server sanal makinenizin (VM) SQL VM kaynak sağlayıcısına nasıl kaydedileceği açıklanmaktadır. 

Azure portal üzerinden SQL Server VM Market görüntüsü dağıtmak, SQL Server VM kaynak sağlayıcısıyla otomatik olarak kaydeder. Bununla birlikte, Azure Marketi 'nden bir görüntü seçmek yerine bir Azure sanal makinesine SQL Server kendi kendine yüklemeyi tercih ediyorsanız, SQL Server VM şu şekilde kaynak sağlayıcısına kaydetmeniz gerekir:

-  **Uyumluluk** – Microsoft ürün koşulları uyarınca, [Azure Hibrit Avantajı](https://azure.microsoft.com/pricing/hybrid-benefit/) kullanan müşteriler Azure hibrit avantajı kullanırken Microsoft 'a işaret etmelidir ve bunu yapmak için, SQL VM kaynak sağlayıcısı 'na kaydolmaları gerekir. 

-  **Özellik avantajları** -SQL Server VM kaynak sağlayıcıyla kaydetme, [otomatik düzeltme eki uygulama](virtual-machines-windows-sql-automated-patching.md), [otomatik yedekleme](virtual-machines-windows-sql-automated-backup-v2.md), izleme ve yönetilebilirlik özelliklerinin yanı sıra [lisanslama](virtual-machines-windows-sql-ahb.md) ve [Sürüm](virtual-machines-windows-sql-change-edition.md) esnekliği de sağlar. Bu, daha önce yalnızca Azure Marketi 'ndeki SQL Server VM görüntülerle sunulmaktadır.

Azure VM 'de SQL Server kendi kendine yükleme veya bir Azure VM 'yi SQL Server ile özel bir VHD 'den sağlama, müşterilerin SQL VM kaynağı ile kaydolarak Microsoft 'a yönelik olduğunu belirtebileceği SQL Server Azure Hibrit Avantajı ile uyumludur sağlayıcısını. 

SQL VM kaynak sağlayıcısını kullanmak için, aboneliğiniz ile SQL VM kaynak sağlayıcısını da kaydetmeniz gerekir. Bu, Azure portal, Azure CLı ve PowerShell ile gerçekleştirilebilir. 

## <a name="prerequisites"></a>Önkoşullar

SQL Server VM kaynak sağlayıcısına kaydetmek için şunlar gerekir: 

- Bir [Azure aboneliği](https://azure.microsoft.com/free/).
- Bir [SQL Server VM](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-server-provision). 
- [Azure CLI](/cli/azure/install-azure-cli) ve [PowerShell](/powershell/azure/new-azureps-module-az). 

## <a name="register-with-sql-vm-resource-provider"></a>SQL VM kaynak sağlayıcısına kaydolun
[SQL IaaS UZANTıSı](virtual-machines-windows-sql-server-agent-extension.md) VM 'de zaten YÜKLÜYSE, SQL VM kaynak sağlayıcısına kaydolmak yalnızca Microsoft. SqlVirtualMachine/SqlVirtualMachines türünde bir meta veri kaynağı oluşturmaktır. SQL IaaS uzantısı VM 'de zaten yüklüyse SQL VM kaynak sağlayıcısı 'na kaydolmak için kod parçacığı aşağıda verilmiştir. SQL VM kaynak sağlayıcısı 'nı ' PAYG ' veya ' AHUB ' olarak kaydederken istediğiniz SQL Server Lisansı türünü sağlamanız gerekir. 

Aşağıdaki kod parçacığı ile PowerShell 'i kullanarak SQL Server VM kaydedin:

  ```powershell-interactive
     # Get the existing  Compute VM
     $vm = Get-AzVM -Name <vm_name> -ResourceGroupName <resource_group_name>
          
     # Register with SQL VM resource provider
     New-AzResource -Name $vm.Name -ResourceGroupName $vm.ResourceGroupName -Location $vm.Location `
        -ResourceType Microsoft.SqlVirtualMachine/SqlVirtualMachines `
        -Properties @{virtualMachineResourceId=$vm.Id;SqlServerLicenseType='AHUB'}  
  
  ```

SQL IaaS uzantısı VM 'de yüklü değilse, basit SQL yönetim modunu belirterek SQL VM kaynak sağlayıcısı ile kayıt yapabilirsiniz. Basit SQL yönetim modunda SQL VM kaynak sağlayıcısı, SQL IaaS uzantısını [hafif modda](virtual-machines-windows-sql-server-agent-extension.md#install-in-lightweight-mode) otomatik olarak yükleyecek ve SQL Server örnek meta verilerini doğrulayacaktır; Bu, SQL Server hizmeti 'ni yeniden başlatmaz. SQL VM kaynak sağlayıcısı 'nı ' PAYG ' veya ' AHUB ' olarak kaydederken istediğiniz SQL Server Lisansı türünü sağlamanız gerekir. 

PowerShell kullanarak aşağıdaki kod parçacığı ile basit SQL yönetim modunda SQL Server VM kaydedin:

  ```powershell-interactive
     # Get the existing  Compute VM
     $vm = Get-AzVM -Name <vm_name> -ResourceGroupName <resource_group_name>
          
     # Register SQL VM with 'Lightweight' SQL IaaS agent
     New-AzResource -Name $vm.Name -ResourceGroupName $vm.ResourceGroupName -Location $vm.Location `
        -ResourceType Microsoft.SqlVirtualMachine/SqlVirtualMachines `
        -Properties @{virtualMachineResourceId=$vm.Id;SqlServerLicenseType='AHUB';sqlManagement='LightWeight'}  
  
  ```

SQL VM kaynak sağlayıcısı ile [hafif modda](virtual-machines-windows-sql-server-agent-extension.md#install-in-lightweight-mode) kaydolmak uyumluluk sağlar ve esnek lisansın yanı sıra yerinde SQL Server Edition güncelleştirmelerini etkinleştirir. Yük devretme kümesi örnekleri ve çok örnekli dağıtımlar, yalnızca hafif modda SQL VM kaynak sağlayıcısı ile kaydedilebilir. [Tam moda](virtual-machines-windows-sql-server-agent-extension.md#full-mode-installation) yükseltmek için Azure Portal bulunan yönergeleri izleyerek, her zaman SQL Server yeniden başlatmaya sahip kapsamlı yönetilebilirlik özelliği kümesini etkinleştirebilirsiniz. 

## <a name="register-sql-server-2008r2-on-windows-server-2008-vms"></a>Windows Server 2008 VM 'lerinde SQL Server 2008/R2 'yi kaydetme

Windows Server 2008 ' de yüklü olan SQL Server 2008 ve 2008 R2, [Noagent](virtual-machines-windows-sql-server-agent-extension.md) modunda SQL VM kaynağına eklenebilir. Bu seçenek uyumluluk sağlar ve Azure portal sınırlı işlevlerle birlikte SQL Server VM izlenmesini sağlar.

Aşağıdaki tabloda, kayıt sırasında belirtilen parametreler için kabul edilebilir değerlerin ayrıntıları verilmiştir:

| Parametre | Kabul edilebilir değerler                                 |
| :------------------| :--------------------------------------- |
| **sqlLicenseType** | `'AHUB'`veya`'PAYG'`                    |
| **Sqlimageteklifini**  | `'SQL2008-WS2008'` veya `'SQL2008R2-WS2008`|
| &nbsp;             | &nbsp;                                   |


SQL Server 2008 veya 2008 R2 'yi Windows Server 2008 örneğine kaydetmek için aşağıdaki PowerShell kod parçacığını kullanın:  

  ```powershell-interactive
     # Get the existing  Compute VM
     $vm = Get-AzVM -Name <vm_name> -ResourceGroupName <resource_group_name>
          
    New-AzResource -Name $vm.Name -ResourceGroupName $vm.ResourceGroupName -Location $vm.Location `
      -ResourceType Microsoft.SqlVirtualMachine/SqlVirtualMachines `
      -Properties @{virtualMachineResourceId=$vm.Id;SqlServerLicenseType='AHUB'; `
       sqlManagement='NoAgent';sqlImageSku='Standard';sqlImageOffer='SQL2008R2-WS2008'}
  ```

## <a name="verify-registration-status"></a>Kayıt durumunu doğrula
SQL Server, SQL VM kaynak sağlayıcısı ile Azure portal, Azure CLı veya PowerShell kullanarak zaten kaydedilmiş olduğunu doğrulayabilirsiniz. 

### <a name="azure-portal"></a>Azure portal 
Azure portal kullanarak kayıt durumunu doğrulamak için, aşağıdakileri yapın.

1. [Azure portal](https://portal.azure.com) oturum açın. 
1. [SQL sanal makinelerinize](virtual-machines-windows-sql-manage-portal.md)gidin.
1. Listeden SQL Server VM seçin. SQL Server VM burada listelenmiyorsa, SQL Server VM SQL VM kaynak sağlayıcısına kaydedilmemiş olabilir. 
1. Değerini altında `Status`görüntüleyin. `Status = Succeeded`Daha sonra SQL Server VM, SQL VM kaynak sağlayıcısına başarıyla kaydedilir. 

    ![SQL RP kaydıyla durumu doğrulama](media/virtual-machines-windows-sql-register-with-rp/verify-registration-status.png)

### <a name="az-cli"></a>Az CLı

Şu AZ CLı komutuyla geçerli SQL Server VM kayıt durumunu doğrulayın. `ProvisioningState`, kaydın `Succeeded` başarılı olup olmadığını gösterir. 

  ```azurecli-interactive
  az sql vm show -n <vm_name> -g <resource_group>
  ```


### <a name="powershell"></a>PowerShell

Geçerli SQL Server VM kayıt durumunu aşağıdaki PowerShell cmdlet 'i ile doğrulayın. `ProvisioningState`, kaydın `Succeeded` başarılı olup olmadığını gösterir. 

  ```powershell-interactive
  Get-AzResource -ResourceName <vm_name> -ResourceGroupName <resource_group> -ResourceType Microsoft.SqlVirtualMachine/sqlVirtualMachines
  ```
Bir hata, SQL Server VM kaynak sağlayıcıya kaydedilmemiş olduğunu gösterir. 

---

## <a name="register-sql-vm-resource-provider-with-subscription"></a>SQL VM kaynak sağlayıcısını abonelikle Kaydet 

SQL Server VM SQL VM kaynak sağlayıcısına kaydetmek için, kaynak sağlayıcısını aboneliğinize kaydetmeniz gerekir. Bunu Azure portal veya Azure CLı ile yapabilirsiniz.

### <a name="azure-portal"></a>Azure portal

Aşağıdaki adımlar, Azure portal kullanarak SQL VM kaynak sağlayıcısını Azure aboneliğinize kaydeder. 

1. Azure portal açın ve **tüm hizmetlere**gidin. 
1. **Abonelikler** ' e gidin ve ilgilendiğiniz aboneliği seçin.  
1. **Abonelikler** sayfasında, **kaynak sağlayıcıları**' na gidin. 
1. SQL `sql` ile ilgili kaynak sağlayıcılarını getirmek için filtreye yazın. 
1. İstediğiniz eyleme bağlı olarak **Microsoft. SqlVirtualMachine** sağlayıcısı için *Kaydet*, *yeniden kaydet*veya *kaydı kaldır* ' ı seçin. 

   ![Sağlayıcıyı değiştirme](media/virtual-machines-windows-sql-ahb/select-resource-provider-sql.png)

### <a name="az-cli"></a>Az CLı
Aşağıdaki kod parçacığı, SQL VM kaynak sağlayıcısını Azure aboneliğinize kaydeder. 

```azurecli-interactive
# Register the new SQL VM resource provider to your subscription 
az provider register --namespace Microsoft.SqlVirtualMachine 
```

### <a name="powershell"></a>PowerShell

Aşağıdaki PowerShell kod parçacığı, SQL VM kaynak sağlayıcısını Azure aboneliğinize kaydeder.

```powershell-interactive
# Register the new SQL VM resource provider to your subscription
Register-AzResourceProvider -ProviderNamespace Microsoft.SqlVirtualMachine
```
---

## <a name="remarks"></a>Açıklamalar

 - SQL VM kaynak sağlayıcısı yalnızca ' Kaynak Yöneticisi ' kullanılarak dağıtılan SQL Server VM 'Leri destekler. ' Klasik model ' kullanılarak dağıtılan SQL Server VM 'Ler desteklenmez. 
 - SQL VM kaynak sağlayıcısı yalnızca genel buluta dağıtılan SQL Server VM 'Leri destekler. Özel veya kamu bulutuna dağıtımlar desteklenmez. 
 
## <a name="frequently-asked-questions"></a>Sık sorulan sorular 

**Azure Market 'teki bir SQL görüntüsünden sağlanan SQL Server VM kaydetmem gerekir mi?**

Hayır. Microsoft, Azure Marketi 'ndeki SQL görüntülerinden sağlanan VM 'Leri kaydeder. SQL VM kaynak sağlayıcısına kaydolmak yalnızca VM 'nin Azure Marketi 'ndeki SQL görüntülerinden sağlanması ve SQL Server kendi kendine yüklenmesi durumunda gereklidir.

**SQL VM kaynak sağlayıcısını tüm müşteriler kullanabilir mi?** 

Evet. Müşteriler, Azure Marketi 'nden bir SQL Server görüntüsü kullanmayan veya kendi özel VHD 'sini getirilen SQL Server SQL Server VM SQL VM kaynak sağlayıcısı ile kaydetmesi gerekir. Tüm abonelik türlerinin (doğrudan, EA ve CSP) sahip olduğu VM 'Ler SQL VM kaynak sağlayıcısına kaydoabilir.

**SQL Server VM SQL IaaS uzantısı zaten yüklüyse SQL VM kaynak sağlayıcısı ile kaydolmalı mı?**

SQL Server VM kendi kendine yüklenmişse, Azure Marketi 'ndeki SQL görüntülerinden sağlanmadıysa, SQL IaaS uzantısını yüklemiş olsanız bile SQL VM kaynak sağlayıcısına kaydolmanız gerekir. SQL VM kaynak sağlayıcısına kaydolmak, Microsoft. SqlVirtualMachines türünde yeni bir kaynak oluşturur. SQL IaaS uzantısını yüklemek o kaynağı oluşturmaz.

**SQL VM kaynak sağlayıcısı ile kaydolurken varsayılan SQL yönetim modu nedir?**

SQL VM RP 'ye kaydolurken varsayılan SQL yönetim modu _dolu_. SQL yönetim özelliği SQL VM kaynak sağlayıcısı ile kaydolurken ayarlanmamışsa, mod tam yönetilebilirlik olarak ayarlanır. VM 'de SQL IaaS uzantısının yüklü olması, SQL VM kaynak sağlayıcısı 'na tam yönetilebilirlik modunda kaydolma önkoşuludur.

**SQL VM kaynak sağlayıcısı 'na kaydolmak için Önkoşullar nelerdir?**

Basit modda veya aracı olmayan bir modda SQL VM kaynak sağlayıcısı 'na kaydolmak için herhangi bir önkoşul yoktur. SQL VM kaynak sağlayıcısına tam modda kaydolma önkoşulları VM 'de SQL IaaS uzantısının yüklü olmasını sağlar.

**VM 'de SQL IaaS uzantısı yüklü değilse SQL VM kaynak sağlayıcısı ile kayıt yapabilir miyim?**

Evet, VM 'de SQL IaaS uzantısı yüklü değilse, basit yönetim modunda SQL VM kaynak sağlayıcısı ile kayıt yapabilirsiniz. Basit modda SQL VM kaynağı sağlayıcısı, SQL örneğinin sürümünü doğrulamak için bir konsol uygulaması kullanacaktır. Konsol uygulaması, VM üzerinde çalışan en az bir SQL örneği olduğunu doğruladıktan sonra kendisini kapatır. SQL VM kaynak sağlayıcısı ile hafif modda kaydolma SQL Server yeniden başlatmaz ve sanal makinede bir aracı oluşturmaz.

**SQL VM kaynak sağlayıcısı ile kaydolacaktır, VM 'imde bir aracı yükler mi?**

Hayır. SQL VM kaynak sağlayıcısına kaydolmak yalnızca yeni bir meta veri kaynağı oluşturacak ve VM 'ye bir aracı yüklemecektir. SQL IaaS uzantısı yalnızca tam yönetilebilirlik sağlamak için gereklidir, bu nedenle yönetilebilirlik modunu hafif 'dan tam 'e yükseltmek SQL IaaS uzantısını yükler ve SQL Server yeniden başlatır.

**VM 'de SQL VM kaynak sağlayıcısı yeniden başlatma SQL Server kaydedilecek mı?**

Hayır. SQL VM kaynak sağlayıcısına kaydolmak yalnızca yeni bir meta veri kaynağı oluşturacak ve VM 'de SQL Server yeniden başlatmayacak. SQL Server yeniden başlatma yalnızca SQL IaaS uzantısını yüklemek için gereklidir; Tam yönetilebilirliği etkinleştirmek için ve SQL IaaS uzantısı gereklidir. Yönetilebilirlik modunu hafif 'dan tam 'e yükseltmek, SQL IaaS uzantısını yükler ve SQL Server yeniden başlatır.

**SQL VM kaynak sağlayıcısı ile kaydolurken basit ve olmayan aracı yönetimi modları arasındaki fark nedir?** 

Aracı yönetim modu, yalnızca Windows Server 2008 ' de SQL Server 2008/R2 için kullanılabilir; ve bu sürümler için kullanılabilen tek yönetim modudur. Tüm SQL Server diğer sürümlerinde, kullanılabilir iki yönetilebilirlik modu hafif ve tamamludur. Aracı modu, SQL Server sürüm ve sürüm özelliklerinin müşteri tarafından ayarlanmasını gerektirir; hafif mod, SQL örneğinin sürümü ve sürümünü bulmak için VM 'yi sorgular.

**Azure CLı ile basit veya aracı olmayan bir modda SQL VM kaynak sağlayıcısı ile kayıt yapabilir miyim?**

Hayır. SQL yönetim modu özelliği yalnızca Azure PowerShell ile SQL VM kaynak sağlayıcısı ile kaydedilirken kullanılabilir. Azure CLı, SQL yönetilebilirlik özelliğinin ayarlanmasını desteklemez ve SQL VM kaynak sağlayıcısı ile her zaman varsayılan modda tam yönetilebilirlik olarak kaydedilir.

**SQL lisans türünü belirtmeden SQL VM kaynak sağlayıcısı ile kayıt yapabilir miyim?**

Hayır. SQL VM RP 'ye kaydolurken SQL lisans türü isteğe bağlı bir özellik değildir. SQL VM kaynak sağlayıcısı 'nı yalnızca AZ CLı ve PowerShell ile her yönetilebilirlik modunda (Hayır-Agent, hafif ve tam) kaydederken, PAYG veya AHUB olarak SQL lisans türünü ayarlamanız gerekir.

**SQL IaaS uzantısını aracı olmayan moddan tam moda yükseltebilir miyim?**

Hayır. SQL yönetilebilirlik modunu tam veya hafif sürümüne yükseltmek, aracı olmayan mod için kullanılamaz. Bu, Windows Server 2008 ' in teknik sınırlamasıdır.

**SQL IaaS uzantısını hafif moddan tam moda yükseltebilir miyim?**

Evet. SQL yönetilebilirlik modunu hafif ' dan Full ' a yükseltmek PowerShell veya Azure portal ile desteklenir; Ayrıca, SQL Server yeniden başlatılmasını gerektirir.

**SQL IaaS uzantısını tam moddan aracı olmayan veya hafif yönetim modlarına indirgeyebilirim miyim?**

Hayır. SQL IaaS uzantısı yönetilebilirlik modunu eski sürüme indirme desteklenmiyor. SQL yönetilebilirlik modu, tam moddan hafif veya aracı olmayan moda düşürülemez; ve basit moddan aracı olmayan moda düşürülemez. Yönetilebilirlik modunu tam yönetilebilirlik olarak değiştirmek için SQL IaaS uzantısını kaldırın; Micorsoft. SqlVirtualMachine kaynağını bırakın ve SQL VM kaynak sağlayıcısı ile SQL Server VM yeniden kaydedin.

**Azure portal 'den SQL VM kaynak sağlayıcısı ile kayıt yapabilir miyim?**

Hayır. SQL VM kaynak sağlayıcısına kaydolma Azure portal kullanılamıyor. SQL VM kaynak sağlayıcısı ile tam yönetilebilirlik modunda kaydolmak, Azure CLı veya PowerShell ile desteklenir; ve basit veya aracı olmayan yönetilebilirlik modundaki SQL VM kaynak sağlayıcısına kaydolmak yalnızca Azure PowerShell API 'Leri tarafından desteklenir.

**SQL Server yüklenmeden önce SQL VM kaynak sağlayıcısı ile bir VM 'yi kaydedebilir miyim?**

Hayır. VM, SQL VM kaynak sağlayıcısına başarıyla kaydolmak için en az bir SQL örneğine sahip olmalıdır. VM 'de SQL örneği yoksa, yeni Micosoft. SqlVirtualMachine kaynağı başarısız durumda olur.

**Birden çok SQL örneği varsa SQL VM kaynağı ile bir VM 'yi kaydedebilir miyim?**

Evet. SQL VM kaynak sağlayıcısı yalnızca bir SQL örneğini kaydeder. SQL VM kaynak sağlayıcısı, birden çok örnek olması durumunda varsayılan SQL örneğini kaydeder. Varsayılan örnek yoksa, yalnızca hafif modda kaydolma desteklenir. Hafif ' den tam yönetilebilirlik moduna yükseltmek için, varsayılan SQL örneği var olmalıdır veya VM yalnızca bir tane adlandırılmış SQL örneğine sahip olmalıdır.

**SQL Server yük devretme kümesi örneğini SQL VM kaynak sağlayıcısı ile kaydedebilir miyim?**

Evet. Azure VM 'deki SQL FCı örnekleri, SQL VM kaynak sağlayıcısı ile hafif modda kaydedilebilir. Ancak, SQL FCı örnekleri tam yönetilebilirlik moduna yükseltilemez.

**Her zaman açık kullanılabilirlik grubu yapılandırılmışsa VM 'imi SQL VM RP ile kaydedebilir miyim?**

Evet. Her zaman açık kullanılabilirlik grubu yapılandırmasına katılırınızda SQL VM kaynak sağlayıcısı ile bir Azure VM 'de SQL Server örneğini kaydetme kısıtlaması yoktur.

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi için aşağıdaki makalelere bakın: 

* [Windows VM 'de SQL Server genel bakış](virtual-machines-windows-sql-server-iaas-overview.md)
* [Windows VM 'de SQL Server SSS](virtual-machines-windows-sql-server-iaas-faq.md)
* [Windows VM fiyatlandırma kılavuzunda SQL Server](virtual-machines-windows-sql-server-pricing-guidance.md)
* [Windows VM sürüm notlarında SQL Server](virtual-machines-windows-sql-server-iaas-release-notes.md)
