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
ms.openlocfilehash: 75f68a4de2db0c4c9102a58da12d80cc273a6e80
ms.sourcegitcommit: 50802bffd56155f3b01bfb4ed009b70045131750
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "91931181"
---
# <a name="automatic-registration-with-sql-vm-resource-provider"></a>SQL VM kaynak sağlayıcısı ile otomatik kayıt
[!INCLUDE[appliesto-sqlvm](../../includes/appliesto-sqlvm.md)]

Azure portal otomatik kayıt özelliğini etkinleştirin ve Azure VM 'lerinde tüm geçerli ve gelecekteki SQL Server, basit modda SQL VM kaynak sağlayıcısı ile otomatik olarak kaydeder.

Bu makale, otomatik kayıt özelliğini etkinleştirmenizi öğretir. Alternatif olarak, [tek BIR VM 'yi kaydedebilir](sql-vm-resource-provider-register.md)veya VM 'LERINIZI SQL VM kaynak sağlayıcısı ile [toplu olarak kaydedebilirsiniz](sql-vm-resource-provider-bulk-register.md) . 

## <a name="overview"></a>Genel Bakış

[SQL VM kaynak sağlayıcısı](sql-vm-resource-provider-register.md#overview) , Azure Portal SQL Server VM yönetmenizi sağlar. Ayrıca, kaynak sağlayıcısı [otomatik düzeltme eki uygulama](automated-patching.md), [otomatik yedekleme](automated-backup.md)ve izleme ve yönetilebilirlik yeteneklerini de kapsayan güçlü bir özellik kümesi sunar. Ayrıca [lisanslama](licensing-model-azure-hybrid-benefit-ahb-change.md) ve [Sürüm](change-sql-server-edition.md) esnekliğini de kaldırır. Daha önce, bu özellikler yalnızca Azure Marketi 'nden dağıtılan SQL Server VM görüntülerle sunulmaktadır. 

Otomatik kayıt özelliği, müşterilerin Azure aboneliğindeki tüm geçerli ve gelecekteki SQL Server VM 'Leri SQL VM kaynak sağlayıcısı ile otomatik olarak kaydetmelerini sağlar. Bu, yalnızca geçerli SQL Server VM 'Lere odaklanarak el ile kayıt işleminden farklıdır. 

Otomatik kayıt SQL Server sanal makinelerinizi hafif modda kaydeder. Tam özellikler kümesinden yararlanmak için yine de [tam yönetilebilirlik moduna el ile yükseltmeniz](sql-vm-resource-provider-register.md#upgrade-to-full) gerekir. 

## <a name="prerequisites"></a>Ön koşullar

SQL Server VM kaynak sağlayıcısına kaydetmek için şunlar gerekir: 

- Bir [Azure aboneliği](https://azure.microsoft.com/free/).
- Azure kaynak modeli [Windows sanal makinesi](../../../virtual-machines/windows/quick-create-portal.md) , genel veya Azure Kamu bulutuna dağıtılan [SQL Server](https://www.microsoft.com/sql-server/sql-server-downloads) . 


## <a name="enable"></a>Etkinleştir

Azure portal SQL Server sanal makinelerinizin otomatik olarak kaydedilmesini etkinleştirmek için şu adımları izleyin:

1. [Azure portalında](https://portal.azure.com) oturum açın.
1. [**SQL sanal makineler**](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResource/resourceType/Microsoft.SqlVirtualMachine%2FSqlVirtualMachines) kaynak sayfasına gidin. 
1. Otomatik **kayıt** sayfasını açmak için **Otomatik SQL Server VM kaydı** ' nı seçin. 

   :::image type="content" source="media/sql-vm-resource-provider-automatic-registration/automatic-registration.png" alt-text="Otomatik kayıt sayfasını açmak için otomatik SQL Server VM kaydını seçin":::

1. Açılır listeden aboneliğinizi seçin. 
1. Koşulları okuyun ve **kabul ediyorsanız kabul ediyorum**' u seçin. 
1. Özelliği etkinleştirmek için **Kaydet** ' i seçin ve tüm geçerli ve gelecekteki SQL Server VM 'LERI SQL VM kaynak sağlayıcısı ile otomatik olarak kaydedin. Bu işlem, VM 'lerden hiçbirinde SQL Server hizmetini yeniden başlatmaz. 

## <a name="disable"></a>Devre Dışı Bırak

Otomatik kayıt özelliğini devre dışı bırakmak için [Azure CLI](/cli/azure/install-azure-cli) veya [Azure PowerShell](/powershell/azure/install-az-ps) kullanın. Otomatik kayıt özelliği devre dışı bırakıldığında, aboneliğe eklenen SQL Server VM 'Lerin SQL VM kaynak sağlayıcısına el ile kaydedilmesi gerekir. Bu, zaten kayıtlı olan mevcut SQL Server VM 'lerinin kaydını siler.



# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

Azure CLı kullanarak otomatik kaydı devre dışı bırakmak için aşağıdaki komutu çalıştırın: 

```azurecli-interactive
az feature unregister --namespace Microsoft.SqlVirtualMachine --name BulkRegistration
```

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

Azure PowerShell kullanarak otomatik kaydı devre dışı bırakmak için aşağıdaki komutu çalıştırın: 

```powershell-interactive
Unregister-AzProviderFeature -FeatureName BulkRegistration -ProviderNamespace Microsoft.SqlVirtualMachine
```

---


## <a name="next-steps"></a>Sonraki adımlar

SQL VM kaynak sağlayıcısı tarafından sunulan tam özellik kümesinden faydalanmak için yönetilebilirlik modlarınızı [tam](sql-vm-resource-provider-register.md#upgrade-to-full) olarak yükseltin. 
