---
title: Azure Monitör'de Günlük Analizi çalışma alanını taşıma | Microsoft Dokümanlar
description: Log Analytics çalışma alanınızı başka bir abonelik veya kaynak grubuna nasıl taşıyabilirsiniz öğrenin.
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 11/13/2019
ms.openlocfilehash: 9213ddf034e725f6e31c9280d47bd13e4703b3f4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77659501"
---
# <a name="move-a-log-analytics-workspace-to-different-subscription-or-resource-group"></a>Günlük Analizi çalışma alanını farklı abonelik veya kaynak grubuna taşıma

Bu makalede, Log Analytics çalışma alanını aynı bölgedeki başka bir kaynak grubuna veya aboneye taşıma adımlarını öğreneceksiniz. Azure kaynaklarını Azure portalı, PowerShell, Azure CLI veya REST API'den taşıma hakkında daha fazla bilgi edinebilirsiniz. [kaynakları yeni bir kaynak grubuna veya aboneye taşıyın.](../../azure-resource-manager/management/move-resource-group-and-subscription.md) 

> [!IMPORTANT]
> Çalışma alanını başka bir bölgeye taşıyamazsınız.

## <a name="verify-active-directory-tenant"></a>Etkin Dizin kiracıdoğrulayın
Çalışma alanı kaynağı ve hedef abonelikleri aynı Azure Etkin Dizin kiracıiçinde bulunmalıdır. Her iki aboneliğin de aynı kiracı kimliğine sahip olduğunu doğrulamak için Azure PowerShell'i kullanın.

``` PowerShell
(Get-AzSubscription -SubscriptionName <your-source-subscription>).TenantId
(Get-AzSubscription -SubscriptionName <your-destination-subscription>).TenantId
```

## <a name="workspace-move-considerations"></a>Çalışma alanı taşıma hususları
Çalışma alanına yüklenen yönetilen çözümler, Log Analytics çalışma alanı taşıma işlemiyle taşınır. Bağlı aracılar bağlı kalır ve taşımadan sonra veri göndermeyi çalışma alanına gönderir. Taşıma işlemi, çalışma alanından herhangi bir otomasyon hesabına bağlantı olmamasını gerektirdiğinden, bu bağlantıya dayanan çözümlerin kaldırılması gerekir.

Otomasyon hesabınızın bağlantısını kaldırmadan önce kaldırılması gereken çözümler:

- Güncelleştirme Yönetimi
- Değişiklik İzleme
- Hizmetin kapalı olduğu saatlerde Sanal Makineleri Başlatma/Durdurma


### <a name="delete-in-azure-portal"></a>Azure portalında silme
Azure portalını kullanarak çözümleri kaldırmak için aşağıdaki yordamı kullanın:

1. Herhangi bir çözümün yüklü olduğu kaynak grubu için menüyü açın.
2. Kaldırmak için çözümleri seçin.
3. **Kaynakları Sil'i** tıklatın ve ardından **Silin'i**tıklatarak kaldırılacak kaynakları onaylayın.

![Çözümleri silme](media/move-workspace/delete-solutions.png)

### <a name="delete-using-powershell"></a>PowerShell kullanarak silme

PowerShell kullanarak çözümleri kaldırmak için aşağıdaki örnekte gösterildiği gibi [Kaldır-AzKaynak](/powershell/module/az.resources/remove-azresource?view=azps-2.8.0) cmdlet'ini kullanın:

``` PowerShell
Remove-AzResource -ResourceType 'Microsoft.OperationsManagement/solutions' -ResourceName "ChangeTracking(<workspace-name>)" -ResourceGroupName <resource-group-name>
Remove-AzResource -ResourceType 'Microsoft.OperationsManagement/solutions' -ResourceName "Updates(<workspace-name>)" -ResourceGroupName <resource-group-name>
Remove-AzResource -ResourceType 'Microsoft.OperationsManagement/solutions' -ResourceName "Start-Stop-VM(<workspace-name>)" -ResourceGroupName <resource-group-name>
```

### <a name="remove-alert-rules"></a>Uyarı kurallarını kaldırma
**Başlat/Durdur VM** çözümü için, çözümün oluşturduğu uyarı kurallarını da kaldırmanız gerekir. Bu kuralları kaldırmak için Azure portalında aşağıdaki yordamı kullanın.

1. **Monitör** menüsünü açın ve ardından **Uyarılar'ı**seçin.
2. **Uyarı kurallarını yönet'i**tıklatın.
3. Aşağıdaki üç uyarı kuralını seçin ve sonra **Sil'i**tıklatın.

   - AutoStop_VM_Child
   - ScheduledStartStop_Parent
   - SequencedStartStop_Parent

    ![Kuralları silme](media/move-workspace/delete-rules.png)

## <a name="unlink-automation-account"></a>Unlink Otomasyon hesabı
Azure portalını kullanarak Otomasyon hesabının çalışma alanından bağlantısını çıkarmak için aşağıdaki yordamı kullanın:

1. Otomasyon **hesapları** menüsünü açın ve ardından kaldırılacak hesabı seçin.
2. Menünün **İlgili Kaynaklar** **bölümünde, Bağlantılı çalışma alanını**seçin. 
3. Çalışma alanını Otomasyon hesabınızdan çıkarmak için **Bağlantıyı Aç'ı** tıklatın.

    ![Çalışma alanının bağlantısını kaldırma](media/move-workspace/unlink-workspace.png)

## <a name="move-your-workspace"></a>Çalışma alanınızı taşıma

### <a name="azure-portal"></a>Azure portalında
Azure portalını kullanarak çalışma alanınızı taşımak için aşağıdaki yordamı kullanın:

1. Günlük **Analizi çalışma alanları** menüsünü açın ve ardından çalışma alanınızı seçin.
2. Genel **Bakış** sayfasında, Kaynak grubunun veya **Abonelik'in**yanındaki **değiştir'i** tıklatın. **change**
3. Çalışma alanıyla ilgili kaynakların bir listesiyle yeni bir sayfa açılır. Çalışma alanıyla aynı hedef abonelik ve kaynak grubuna geçmek için kaynakları seçin. 
4. Hedef **Abonelik** ve **Kaynak grubu**seçin. Çalışma alanını aynı abonelikteki başka bir kaynak grubuna taşıyorsanız, **Abonelik** seçeneğini görmezsiniz.
5. Çalışma alanını ve seçili kaynakları taşımak için **Tamam'ı** tıklatın.

    ![Portal](media/move-workspace/portal.png)

### <a name="powershell"></a>PowerShell
PowerShell'i kullanarak çalışma alanınızı taşımak için aşağıdaki örnekte olduğu gibi [Taşı-AzResource'ı](/powershell/module/AzureRM.Resources/Move-AzureRmResource) kullanın:

``` PowerShell
Move-AzResource -ResourceId "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/MyResourceGroup01/providers/Microsoft.OperationalInsights/workspaces/MyWorkspace" -DestinationSubscriptionId "00000000-0000-0000-0000-000000000000" -DestinationResourceGroupName "MyResourceGroup02"
```



> [!IMPORTANT]
> Taşıma işleminden sonra, kaldırılan çözümler ve Otomasyon hesabı bağlantısı, çalışma alanını önceki durumuna geri getirmek için yeniden yapılandırılmalıdır.


## <a name="next-steps"></a>Sonraki adımlar
- Kaynakların taşınmasını destekleyen lerin listesi [için](../../azure-resource-manager/management/move-support-resources.md)bkz.
