---
title: Azure Otomasyonu hesabınızı başka bir aboneliğe taşıma
description: Bu makalede Otomasyon hesabınızı başka bir aboneliğe nasıl taşıyacağınız açıklanmaktadır
services: automation
ms.service: automation
ms.subservice: process-automation
author: mgoedtel
ms.author: magoedte
ms.date: 03/11/2019
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 1aa759a2984764169eb28935e095d0f7c0f90c08
ms.sourcegitcommit: 3dc1a23a7570552f0d1cc2ffdfb915ea871e257c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/15/2020
ms.locfileid: "75969841"
---
# <a name="move-your-azure-automation-account-to-another-subscription"></a>Azure Otomasyonu hesabınızı başka bir aboneliğe taşıma

Azure, bazı kaynakları yeni bir kaynak grubuna veya aboneliğe taşıyabilme olanağı sağlar. Kaynakları Azure portal, PowerShell, Azure CLı veya REST API üzerinden taşıyabilirsiniz. İşlem hakkında daha fazla bilgi edinmek için bkz. [kaynakları yeni bir kaynak grubuna veya aboneliğe taşıma](../../azure-resource-manager/management/move-resource-group-and-subscription.md).

Azure Otomasyonu hesapları, taşınabilecek kaynaklardan biridir. Bu makalede, Otomasyon hesaplarını başka bir kaynağa veya aboneliğe taşıma adımlarını öğreneceksiniz.

Otomasyon hesabınızı taşımaya yönelik üst düzey adımlar şunlardır:

1. Çözümlerinizi kaldırın.
2. Çalışma alanınızın bağlantısını kaldırın.
3. Otomasyon hesabını taşıyın.
4. Farklı Çalıştır hesaplarını silin ve yeniden oluşturun.
5. Çözümlerinizi yeniden etkinleştirin.

## <a name="remove-solutions"></a>Çözümleri kaldır

Çalışma alanınızın Otomasyon hesabınızla bağlantısını kaldırmak için bu çözümlerin çalışma alanınızdan kaldırılması gerekir:
- **Değişiklik İzleme ve stok**
- **Güncelleştirme yönetimi**
- **VM 'Leri çalışma saatleri dışında Başlat/Durdur**

Kaynak grubunuzda, her bir çözümü bulun ve **Sil**' i seçin. **Kaynakları Sil** sayfasında, kaldırılacak kaynakları onaylayın ve **Sil**' i seçin.

![Azure portal çözümleri silme](../media/move-account/delete-solutions.png)

Aşağıdaki örnekte gösterildiği gibi [Remove-AzureRmResource](/powershell/module/azurerm.resources/remove-azurermresource) cmdlet 'i ile aynı görevi gerçekleştirebilirsiniz:

```azurepowershell-interactive
$workspaceName = <myWorkspaceName>
$resourceGroupName = <myResourceGroup>
Remove-AzureRmResource -ResourceType 'Microsoft.OperationsManagement/solutions' -ResourceName "ChangeTracking($workspaceName)" -ResourceGroupName $resourceGroupName
Remove-AzureRmResource -ResourceType 'Microsoft.OperationsManagement/solutions' -ResourceName "Updates($workspaceName)" -ResourceGroupName $resourceGroupName
Remove-AzureRmResource -ResourceType 'Microsoft.OperationsManagement/solutions' -ResourceName "Start-Stop-VM($workspaceName)" -ResourceGroupName $resourceGroupName
```

### <a name="additional-steps-for-startstop-vms"></a>VM 'Leri başlatma/durdurma için ek adımlar

**VM 'Leri Başlat/Durdur** çözümü için, çözüm tarafından oluşturulan uyarı kurallarını da kaldırmanız gerekir.

Azure portal, kaynak grubunuza gidin ve **Uyarı kurallarını yönetmek** >  > **uyarılarını** **izleme** ' yi seçin.

![Uyarı kurallarını yönet seçimini gösteren uyarılar sayfası](../media/move-account/alert-rules.png)

**Kurallar** sayfasında, bu kaynak grubunda yapılandırılmış uyarıların bir listesini görmeniz gerekir. **VM 'Leri Başlat/Durdur** çözümü üç uyarı kuralı oluşturur:

* AutoStop_VM_Child
* ScheduledStartStop_Parent
* SequencedStartStop_Parent

Bu üç uyarı kuralını seçin ve ardından **Sil**' i seçin. Bu eylem, bu uyarı kurallarını kaldırır.

![Kurallar sayfası seçili kuralların silinme onayını istiyor](../media/move-account/delete-rules.png)

> [!NOTE]
> **Kurallar** sayfasında herhangi bir uyarı kuralı görmüyorsanız, devre dışı bırakılmış olabileceğiniz Için **durumu** **devre dışı bırakılmış** uyarıları gösterecek şekilde değiştirin.

Uyarı kuralları kaldırıldığında, **VM 'Leri Başlat/Durdur** çözüm bildirimleri için oluşturulan eylem grubunu kaldırın.

Azure portal, **eylem gruplarını yönetmek** >  > **uyarılarını** **İzle** ' yi seçin.

Listeden **StartStop_VM_Notification** seçin. Eylem grubu sayfasında **Sil**' i seçin.

![Eylem grubu sayfasında Sil ' i seçin.](../media/move-account/delete-action-group.png)

Benzer şekilde, aşağıdaki örnekte görüldüğü gibi, PowerShell 'i [Remove-AzureRmActionGroup](/powershell/module/azurerm.insights/remove-azurermactiongroup) cmdlet 'i ile birlikte kullanarak eylem grubunuzu silebilirsiniz:

```azurepowershell-interactive
Remove-AzureRmActionGroup -ResourceGroupName <myResourceGroup> -Name StartStop_VM_Notification
```

## <a name="unlink-your-workspace"></a>Çalışma alanınızın bağlantısını kaldırma

Azure portal, **bağlantılı çalışma alanı** > **Ilgili kaynaklar** > **Otomasyon hesabı** ' nı seçin. Çalışma alanının Otomasyon hesabından bağlantısını kaldırmak için **çalışma alanının bağlantısını kaldır** ' ı seçin.

![Bir çalışma alanının Otomasyon hesabından bağlantısını kaldırma](../media/move-account/unlink-workspace.png)

## <a name="move-your-automation-account"></a>Otomasyon hesabınızı taşıyın

Önceki öğeleri kaldırdıktan sonra, Otomasyon hesabınızı ve Runbook 'larını kaldırmaya devam edebilirsiniz. Azure portal, Otomasyon hesabınızın kaynak grubuna gidin. **Taşıma** > **başka bir aboneliğe taşıma**' yı seçin.

![Kaynak grubu sayfası, başka bir aboneliğe taşıma](../media/move-account/move-resources.png)

Kaynak grubunuzda, taşımak istediğiniz kaynakları seçin. **Otomasyon hesabınızı**, **Runbook**'u ve **Log Analytics çalışma alanı** kaynaklarınızı dahil edin.

Taşıma işlemi tamamlandıktan sonra, her şeyin çalışmasını sağlamak için ek adımlar gereklidir.

## <a name="re-create-run-as-accounts"></a>Farklı Çalıştır hesaplarını yeniden oluşturma

[Farklı Çalıştır hesapları](../manage-runas-account.md) Azure kaynaklarıyla kimlik doğrulaması yapmak için Azure Active Directory bir hizmet sorumlusu oluşturur. Abonelikleri değiştirdiğinizde, Otomasyon hesabı artık mevcut farklı çalıştır hesabını kullanmaz.

Yeni abonelikte Otomasyon hesabınıza gidin ve **Hesap ayarları**altında **Farklı Çalıştır hesapları** ' nı seçin. Farklı Çalıştır hesaplarının şimdi tamamlanmamış olarak gösterileceğini görürsünüz.

![Farklı Çalıştır hesapları eksik](../media/move-account/run-as-accounts.png)

Her bir farklı çalıştır hesabını seçin. **Özellikler** sayfasında, farklı çalıştır hesabını silmek için **Sil** ' i seçin.

> [!NOTE]
> Farklı Çalıştır hesaplarını oluşturma veya görüntüleme izniniz yoksa şu iletiyi görürsünüz `You do not have permissions to create an Azure Run As account (service principal) and grant the Contributor role to the service principal.`: farklı çalıştır hesabını yapılandırmak için gerekli izinler hakkında bilgi edinmek Için, bkz. [Farklı Çalıştır hesaplarını yapılandırmak için gereken izinler](../manage-runas-account.md#permissions).

Farklı Çalıştır hesapları silindikten sonra **Azure farklı çalıştır hesabı**altında **Oluştur** ' u seçin. Farklı Çalıştır hesabı ve hizmet sorumlusu oluşturmak için **Azure farklı çalıştır hesabı ekle** sayfasında **Oluştur** ' u seçin. Önceki adımları **Azure klasik farklı çalıştır hesabıyla**tekrarlayın.

## <a name="enable-solutions"></a>Çözümleri etkinleştirme

Farklı Çalıştır hesaplarını yeniden oluşturduktan sonra, taşımadan önce kaldırdığınız çözümleri yeniden etkinleştireceksiniz. **Değişiklik izleme ve envanteri** ve **güncelleştirme yönetimi**açmak için Otomasyon hesabınızda ilgili özelliği seçin. Üzerinde taşınan Log Analytics çalışma alanını seçin ve **Etkinleştir**' i seçin.

![Taşınan Otomasyon hesabınızda çözümleri yeniden etkinleştirin](../media/move-account/reenable-solutions.png)

Çözümlerinizle eklendi olan makineler, var olan Log Analytics çalışma alanını bağladığınızda görünür olur.

VM 'Leri çalışma saatleri dışında **Başlat/Durdur** çözümünü açmak için çözümü yeniden dağıtmanız gerekir. **Ilgili kaynaklar**altında, **sanal makineleri Başlat/Durdur** ' u seçin > **daha fazla bilgi edinin ve** dağıtımı başlatmak için > **oluşturun** çözümü etkinleştirin.

**Çözüm Ekle** sayfasında, Log Analytics çalışma alanınızı ve otomasyon hesabınızı seçin.

![Çözüm Ekle menüsü](../media/move-account/add-solution-vm.png)

Çözümü yapılandırma hakkında ayrıntılı yönergeler için bkz. [Azure Otomasyonu 'nda VM'leri çalışma saatleri dışında Başlat/Durdur çözümü](../automation-solution-vm-management.md).

## <a name="post-move-verification"></a>Taşıma sonrası doğrulama

Taşıma işlemi tamamlandığında, doğrulanması gereken görevler listesini kontrol edin:

|Özellik|Testler|Sorun Giderme bağlantısı|
|---|---|---|
|Runbook'lar|Runbook, Azure kaynaklarına başarıyla çalıştırıp bağlanabilir.|[Runbook sorunlarını giderme](../troubleshoot/runbooks.md)
|Kaynak denetimi|Kaynak denetimi deponuz üzerinde el ile eşitleme gerçekleştirebilirsiniz.|[Kaynak denetimi tümleştirmesi](../source-control-integration.md)|
|Değişiklik izleme ve stok|Makinelerinizden geçerli envanter verilerini gördiğinizi doğrulayın.|[Değişiklik izleme sorunlarını giderme](../troubleshoot/change-tracking.md)|
|Güncelleştirme yönetimi|Makinelerinizi görmediğinizi ve sağlıklı olduğunu doğrulayın.</br>Bir test yazılım güncelleştirme dağıtımı çalıştırın.|[Güncelleştirme yönetimi sorunlarını giderme](../troubleshoot/update-management.md)|
|Paylaşılan kaynaklar|[Kimlik bilgileri](../shared-resources/credentials.md), [değişkenler](../shared-resources/variables.md)vb. gibi tüm paylaşılan kaynaklarınızı gördiğinizi doğrulayın.|

## <a name="next-steps"></a>Sonraki adımlar

Azure 'da kaynakları taşıma hakkında daha fazla bilgi için bkz. [Azure 'da kaynakları taşıma](../../azure-resource-manager/management/move-support-resources.md).
