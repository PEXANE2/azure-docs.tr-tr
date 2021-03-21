---
title: SQL IaaS Aracısı Uzantısı ile otomatik kayıt
description: Otomatik kayıt özelliğinin, Azure portal kullanarak SQL IaaS Aracısı uzantısına sahip tüm geçmiş ve gelecekteki SQL Server VM 'Leri otomatik olarak kaydetmesini sağlama hakkında bilgi edinin.
author: MashaMSFT
ms.author: mathoma
tags: azure-service-management
ms.service: virtual-machines-sql
ms.subservice: management
ms.topic: conceptual
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 11/07/2020
ms.openlocfilehash: 139852949a3744fd603cb197b2e27fa32679aae0
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/20/2021
ms.locfileid: "102042434"
---
# <a name="automatic-registration-with-sql-iaas-agent-extension"></a>SQL IaaS Aracısı Uzantısı ile otomatik kayıt
[!INCLUDE[appliesto-sqlvm](../../includes/appliesto-sqlvm.md)]

Azure portal otomatik kayıt özelliğini, Azure sanal makinelerinde (VM) bulunan tüm geçerli ve gelecekteki SQL Server, [SQL IaaS aracı uzantısına](sql-server-iaas-agent-extension-automate-management.md) hafif modda otomatik olarak kaydetmek üzere etkinleştirin. 

Bu makale, otomatik kayıt özelliğini etkinleştirmenizi öğretir. Alternatif olarak, [tek BIR VM 'yi kaydedebilir](sql-agent-extension-manually-register-single-vm.md)veya VM 'Lerinizi SQL IaaS Aracısı Uzantısı ile [toplu olarak kaydedebilirsiniz](sql-agent-extension-manually-register-vms-bulk.md) . 

## <a name="overview"></a>Genel Bakış

Tam özellik avantajlarından yararlanmak için SQL Server VM [SQL IaaS Aracısı uzantısı](sql-server-iaas-agent-extension-automate-management.md) ile kaydetme. 

Otomatik kayıt etkinleştirildiğinde bir iş, abonelikteki tüm kayıtlı VM 'lerde SQL Server yüklenip yüklenmediğini algılamak için günlük olarak çalıştırılır. Bu işlem SQL IaaS Aracısı uzantısı ikili dosyalarını VM 'ye kopyalayarak ve ardından SQL Server kayıt defteri kovanını denetleyen bir kerelik yardımcı program çalıştırarak yapılır. SQL Server Hive algılanırsa, sanal makine uzantıya basit modda kaydedilir. Kayıt defterinde SQL Server Hive yoksa, ikili dosyalar kaldırılır.

Bir abonelik için otomatik kayıt etkinleştirildikten sonra, SQL Server yüklü olan tüm mevcut ve gelecekteki sanal makineler, **kapalı kalma süresi olmadan ve SQL Server hizmetini yeniden başlatmadan basit modda** SQL IaaS aracı uzantısına kaydedilir. Tam özellikler kümesinden yararlanmak için yine de [tam yönetilebilirlik moduna el ile yükseltmeniz](sql-agent-extension-manually-register-single-vm.md#upgrade-to-full) gerekir. 

> [!IMPORTANT]
> SQL IaaS Aracısı uzantısı, Azure sanal makineler 'de SQL Server kullanırken müşterilere isteğe bağlı avantajlar vermek için veri toplar. Microsoft, bu verileri müşterinin öncelikli onayı olmadan lisanslama denetimleri için kullanmaz. Daha fazla bilgi için [SQL Server Gizlilik Eki](/sql/sql-server/sql-server-privacy#non-personal-data) ' ne bakın.

## <a name="prerequisites"></a>Önkoşullar

SQL Server VM uzantıya kaydetmek için şunlar gerekir: 

- Bir [Azure aboneliği](https://azure.microsoft.com/free/) ve, en az [katkıda bulunan rol](../../../role-based-access-control/built-in-roles.md#all) izinleri.
- Azure kaynak modeli [Windows Server 2008 R2 (veya üzeri) sanal makinesini](../../../virtual-machines/windows/quick-create-portal.md) , genel veya Azure Kamu bulutuna dağıtılan [SQL Server](https://www.microsoft.com/sql-server/sql-server-downloads) . Windows Server 2008 desteklenmez. 


## <a name="enable"></a>Etkinleştir

Azure portal SQL Server sanal makinelerinizin otomatik olarak kaydedilmesini etkinleştirmek için şu adımları izleyin:

1. [Azure portal](https://portal.azure.com) oturum açın.
1. [**SQL sanal makineler**](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResource/resourceType/Microsoft.SqlVirtualMachine%2FSqlVirtualMachines) kaynak sayfasına gidin. 
1. Otomatik **kayıt** sayfasını açmak için **Otomatik SQL Server VM kaydı** ' nı seçin. 

   :::image type="content" source="media/sql-agent-extension-automatic-registration-all-vms/automatic-registration.png" alt-text="Otomatik kayıt sayfasını açmak için otomatik SQL Server VM kaydını seçin":::

1. Açılır listeden aboneliğinizi seçin. 
1. Koşulları okuyun ve **kabul ediyorsanız kabul ediyorum**' u seçin. 
1. Özelliği etkinleştirmek için **Kaydet** ' i seçin ve tüm geçerli ve gelecekteki SQL Server VM 'Leri SQL IaaS Aracısı Uzantısı ile otomatik olarak kaydedin. Bu işlem, VM 'lerden hiçbirinde SQL Server hizmetini yeniden başlatmaz. 

## <a name="disable"></a>Devre Dışı Bırak

Otomatik kayıt özelliğini devre dışı bırakmak için [Azure CLI](/cli/azure/install-azure-cli) veya [Azure PowerShell](/powershell/azure/install-az-ps) kullanın. Otomatik kayıt özelliği devre dışı bırakıldığında, aboneliğe eklenen SQL Server VM 'Lerin SQL IaaS Aracısı uzantısına el ile kaydedilmesi gerekir. Bu, zaten kayıtlı olan mevcut SQL Server VM 'lerinin kaydını siler.



# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

Azure CLı kullanarak otomatik kaydı devre dışı bırakmak için aşağıdaki komutu çalıştırın: 

```azurecli-interactive
az feature unregister --namespace Microsoft.SqlVirtualMachine --name BulkRegistration
```

# <a name="azure-powershell"></a>[Azure PowerShell](#tab/azure-powershell)

Azure PowerShell kullanarak otomatik kaydı devre dışı bırakmak için aşağıdaki komutu çalıştırın: 

```powershell-interactive
Unregister-AzProviderFeature -FeatureName BulkRegistration -ProviderNamespace Microsoft.SqlVirtualMachine
```

---

## <a name="enable-for-multiple-subscriptions"></a>Birden çok abonelik için etkinleştir

PowerShell kullanarak, birden çok Azure aboneliği için otomatik kayıt özelliğini etkinleştirebilirsiniz. 

Bunu yapmak için aşağıdaki adımları izleyin:

1. [Bu betiği](https://github.com/microsoft/tigertoolbox/blob/master/AzureSQLVM/RegisterSubscriptionsToSqlVmAutomaticRegistration.ps1) , gibi bir `.ps1` dosyaya kaydedin `EnableBySubscription.ps1` . 
1. Bir yönetim komut Istemi veya PowerShell penceresi kullanarak betiği kaydettiğiniz yere gidin. 
1. Azure 'a ( `az login` ) bağlanın.
1. Komut dosyasını yürütün ve abonelik kimliklerini şu şekilde parametreler olarak geçirerek   
   `.\EnableBySubscription.ps1 -SubscriptionList SubscriptionId1,SubscriptionId2`

   Örnek: 

   ```console
   .\EnableBySubscription.ps1 -SubscriptionList a1a1a-aa11-11aa-a1a1-a11a111a1,b2b2b2-bb22-22bb-b2b2-b2b2b2bb
   ```

Başarısız kayıt hataları, `RegistrationErrors.csv` komut dosyasını kaydettiğiniz ve yürüttüğünüz dizinde depolanır `.ps1` . 

## <a name="next-steps"></a>Sonraki adımlar

SQL IaaS Aracısı uzantısı tarafından size sunulan tam özellik kümesinin avantajlarından yararlanmak için yönetilebilirlik modınızı [tam](sql-agent-extension-manually-register-single-vm.md#upgrade-to-full) olarak yükseltin. 
