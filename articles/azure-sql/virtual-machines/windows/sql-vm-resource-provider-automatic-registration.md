---
title: SQL VM kaynak sağlayıcısı ile otomatik kayıt
description: Otomatik kayıt özelliğinin, Azure portal kullanarak SQL VM kaynak sağlayıcısı ile tüm geçmiş ve gelecekteki SQL Server VM 'Leri otomatik olarak kaydetmesini sağlama hakkında bilgi edinin.
author: MashaMSFT
ms.author: mathoma
tags: azure-service-management
ms.service: virtual-machines-sql
ms.topic: conceptual
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 09/21/2020
ms.openlocfilehash: 23ecc3bdfb0ca85caf219fc262348937923f53c3
ms.sourcegitcommit: 7863fcea618b0342b7c91ae345aa099114205b03
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/03/2020
ms.locfileid: "93286116"
---
# <a name="automatic-registration-with-sql-vm-resource-provider"></a>SQL VM kaynak sağlayıcısı ile otomatik kayıt
[!INCLUDE[appliesto-sqlvm](../../includes/appliesto-sqlvm.md)]

Azure portal otomatik kayıt özelliğini etkinleştirin ve Azure sanal makinelerinde (VM) bulunan tüm geçerli ve gelecekteki SQL Server, basit modda SQL VM kaynak sağlayıcısına otomatik olarak kaydeder. SQL VM kaynak sağlayıcısı ile kaydolmak [SQL IaaS Aracısı uzantısını](sql-server-iaas-agent-extension-automate-management.md)yüklenir.

Bu makale, otomatik kayıt özelliğini etkinleştirmenizi öğretir. Alternatif olarak, [tek BIR VM 'yi kaydedebilir](sql-vm-resource-provider-register.md)veya VM 'LERINIZI SQL VM kaynak sağlayıcısı ile [toplu olarak kaydedebilirsiniz](sql-vm-resource-provider-bulk-register.md) . 

## <a name="overview"></a>Genel Bakış

SQL Server VM SQL VM kaynak sağlayıcısı ile kaydettirmek [SQL IaaS Aracısı uzantısını](sql-server-iaas-agent-extension-automate-management.md)kurar. 

Otomatik kayıt etkinleştirildiğinde bir iş, abonelikteki tüm kayıtlı VM 'lerde SQL Server yüklenip yüklenmediğini algılamak için günlük olarak çalıştırılır. Bu işlem SQL IaaS Aracısı uzantısı ikili dosyalarını VM 'ye kopyalayarak ve ardından SQL Server kayıt defteri kovanını denetleyen bir kerelik yardımcı program çalıştırarak yapılır. SQL Server Hive algılanırsa, sanal makine [SQL VM kaynak sağlayıcısına](sql-vm-resource-provider-register.md) hafif modda kaydedilir. Kayıt defterinde SQL Server Hive yoksa, ikili dosyalar kaldırılır.

Bir abonelik için otomatik kayıt etkinleştirildikten sonra, SQL Server yüklü olan tüm mevcut ve gelecekteki VM 'Ler, SQL VM kaynak sağlayıcısı ile hafif modda kaydedilir. Tam özellikler kümesinden yararlanmak için yine de [tam yönetilebilirlik moduna el ile yükseltmeniz](sql-vm-resource-provider-register.md#upgrade-to-full) gerekir. 

> [!IMPORTANT]
> SQL IaaS Aracısı uzantısı, Azure sanal makineler 'de SQL Server kullanırken müşterilere isteğe bağlı avantajlar vermek için veri toplar. Microsoft, bu verileri müşterinin öncelikli onayı olmadan lisanslama denetimleri için kullanmaz. Daha fazla bilgi için [SQL Server Gizlilik Eki](/sql/sql-server/sql-server-privacy#non-personal-data) ' ne bakın.

## <a name="prerequisites"></a>Önkoşullar

SQL Server VM kaynak sağlayıcısına kaydetmek için şunlar gerekir: 

- Bir [Azure aboneliği](https://azure.microsoft.com/free/).
- Azure kaynak modeli [Windows Server 2008 R2 (veya üzeri) sanal makinesini](../../../virtual-machines/windows/quick-create-portal.md) , genel veya Azure Kamu bulutuna dağıtılan [SQL Server](https://www.microsoft.com/sql-server/sql-server-downloads) . Windows Server 2008 desteklenmez. 


## <a name="enable"></a>Etkinleştir

Azure portal SQL Server sanal makinelerinizin otomatik olarak kaydedilmesini etkinleştirmek için şu adımları izleyin:

1. [Azure portalında](https://portal.azure.com) oturum açın.
1. [**SQL sanal makineler**](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResource/resourceType/Microsoft.SqlVirtualMachine%2FSqlVirtualMachines) kaynak sayfasına gidin. 
1. Otomatik **kayıt** sayfasını açmak için **Otomatik SQL Server VM kaydı** ' nı seçin. 

   :::image type="content" source="media/sql-vm-resource-provider-automatic-registration/automatic-registration.png" alt-text="Otomatik kayıt sayfasını açmak için otomatik SQL Server VM kaydını seçin":::

1. Açılır listeden aboneliğinizi seçin. 
1. Koşulları okuyun ve **kabul ediyorsanız kabul ediyorum** ' u seçin. 
1. Özelliği etkinleştirmek için **Kaydet** ' i seçin ve tüm geçerli ve gelecekteki SQL Server VM 'LERI SQL VM kaynak sağlayıcısı ile otomatik olarak kaydedin. Bu işlem, VM 'lerden hiçbirinde SQL Server hizmetini yeniden başlatmaz. 

## <a name="disable"></a>Devre Dışı Bırak

Otomatik kayıt özelliğini devre dışı bırakmak için [Azure CLI](/cli/azure/install-azure-cli) veya [Azure PowerShell](/powershell/azure/install-az-ps) kullanın. Otomatik kayıt özelliği devre dışı bırakıldığında, aboneliğe eklenen SQL Server VM 'Lerin SQL VM kaynak sağlayıcısına el ile kaydedilmesi gerekir. Bu, zaten kayıtlı olan mevcut SQL Server VM 'lerinin kaydını siler.



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

   Örneğin: 

   ```console
   .\EnableBySubscription.ps1 -SubscriptionList a1a1a-aa11-11aa-a1a1-a11a111a1,b2b2b2-bb22-22bb-b2b2-b2b2b2bb
   ```

Başarısız kayıt hataları, `RegistrationErrors.csv` komut dosyasını kaydettiğiniz ve yürüttüğünüz dizinde depolanır `.ps1` . 

## <a name="next-steps"></a>Sonraki adımlar

SQL VM kaynak sağlayıcısı tarafından sunulan tam özellik kümesinden faydalanmak için yönetilebilirlik modlarınızı [tam](sql-vm-resource-provider-register.md#upgrade-to-full) olarak yükseltin. 
