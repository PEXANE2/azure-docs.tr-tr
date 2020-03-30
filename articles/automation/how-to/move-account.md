---
title: Azure Otomasyon hesabınızı başka bir aboneye taşıyın
description: Bu makalede, Otomasyon hesabınızın başka bir aboneye nasıl taşınır
services: automation
ms.service: automation
ms.subservice: process-automation
author: mgoedtel
ms.author: magoedte
ms.date: 03/11/2019
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 1aa759a2984764169eb28935e095d0f7c0f90c08
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75969841"
---
# <a name="move-your-azure-automation-account-to-another-subscription"></a>Azure Otomasyon hesabınızı başka bir aboneye taşıyın

Azure, bazı kaynakları yeni bir kaynak grubuna veya aboneliğe taşıma olanağı sağlar. Kaynakları Azure portalı, PowerShell, Azure CLI veya REST API'de taşıyabilirsiniz. İşlem hakkında daha fazla bilgi edinmek [için bkz.](../../azure-resource-manager/management/move-resource-group-and-subscription.md)

Azure Otomasyon hesapları taşınabilen kaynaklardan biridir. Bu makalede, Otomasyon hesaplarını başka bir kaynağa veya aboneye taşıma adımlarını öğreneceksiniz.

Otomasyon hesabınızı taşımak için üst düzey adımlar şunlardır:

1. Çözümlerinizi kaldırın.
2. Çalışma alanınızın bağlantısını boşaltın.
3. Otomasyon hesabını taşıyın.
4. Run As hesaplarını silin ve yeniden oluşturun.
5. Çözümlerinizi yeniden etkinleştirin.

## <a name="remove-solutions"></a>Çözümleri kaldırma

Çalışma alanınızı Otomasyon hesabınızdan kaldırmak için bu çözümlerin çalışma alanınızdan kaldırılması gerekir:
- **Değişiklik İzleme ve Stok**
- **Güncelleme Yönetimi**
- **Kapalı saatlerde VM'leri Başlatma/Durdurma**

Kaynak grubunuzda, her çözümü bulun ve **Sil'i**seçin. Kaynakları **Sil** sayfasında, kaldırılacak kaynakları onaylayın ve **Sil'i**seçin.

![Azure portalından çözümleri silme](../media/move-account/delete-solutions.png)

Aşağıdaki örnekte gösterildiği gibi [Remove-AzureRmResource](/powershell/module/azurerm.resources/remove-azurermresource) cmdlet ile aynı görevi gerçekleştirebilirsiniz:

```azurepowershell-interactive
$workspaceName = <myWorkspaceName>
$resourceGroupName = <myResourceGroup>
Remove-AzureRmResource -ResourceType 'Microsoft.OperationsManagement/solutions' -ResourceName "ChangeTracking($workspaceName)" -ResourceGroupName $resourceGroupName
Remove-AzureRmResource -ResourceType 'Microsoft.OperationsManagement/solutions' -ResourceName "Updates($workspaceName)" -ResourceGroupName $resourceGroupName
Remove-AzureRmResource -ResourceType 'Microsoft.OperationsManagement/solutions' -ResourceName "Start-Stop-VM($workspaceName)" -ResourceGroupName $resourceGroupName
```

### <a name="additional-steps-for-startstop-vms"></a>Başlat/Durdur VM'leri için ek adımlar

**Başlat/Durdur VM** çözümü için, çözümün oluşturduğu uyarı kurallarını da kaldırmanız gerekir.

Azure portalında, kaynak grubunuza gidin ve **İzleme** > **Uyarıları** > **Yönetme uyarı kurallarını**seçin.

![Uyarıyı Yönet kurallarının seçimini gösteren uyarılar sayfası](../media/move-account/alert-rules.png)

**Kurallar** sayfasında, bu kaynak grubunda yapılandırılan uyarıların listesini görmeniz gerekir. **Başlat/Durdur VM** çözümü üç uyarı kuralı oluşturur:

* AutoStop_VM_Child
* ScheduledStartStop_Parent
* SequencedStartStop_Parent

Bu üç uyarı kuralını seçin ve sonra **Sil'i**seçin. Bu eylem, bu uyarı kurallarını kaldırır.

![Seçili kurallar için silme onayı isteyen kurallar sayfası](../media/move-account/delete-rules.png)

> [!NOTE]
> **Kurallar** sayfasında herhangi bir uyarı kuralı görmüyorsanız, devre dışı bırakılmış olabileceğiniz için **Devre Dışı Bırakılan** uyarıları göstermek için **Durum'u** değiştirin.

Uyarı kuralları kaldırıldığında, **Başlat/Durdur VM** çözüm bildirimleri için oluşturulan eylem grubunu kaldırın.

Azure portalında,**Uyarıları** > **Yönet eylem gruplarını** **izleyin'i** > seçin.

Listeden **StartStop_VM_Notification'ı** seçin. Eylem grubu sayfasında **Sil'i**seçin.

![Eylem grubu sayfası, sil seçin](../media/move-account/delete-action-group.png)

Benzer şekilde, aşağıdaki örnekte görüldüğü [gibi, Remove-AzureRmActionGroup](/powershell/module/azurerm.insights/remove-azurermactiongroup) cmdlet ile PowerShell kullanarak eylem grubunuzu silebilirsiniz:

```azurepowershell-interactive
Remove-AzureRmActionGroup -ResourceGroupName <myResourceGroup> -Name StartStop_VM_Notification
```

## <a name="unlink-your-workspace"></a>Çalışma alanınızın bağlantısını boşaltın

Azure portalında, **Otomasyon hesabı** > **Yla İlgili Kaynaklar** > **Bağlantılı çalışma alanını**seçin. Çalışma alanını Otomasyon hesabınızdan çıkarmak için **Bağlantıyı Aç çalışma alanını** seçin.

![Bir Otomasyon hesabından çalışma alanını açma](../media/move-account/unlink-workspace.png)

## <a name="move-your-automation-account"></a>Otomasyon hesabınızı taşıyın

Önceki öğeleri kaldırdıktan sonra, Otomasyon hesabınızı ve runbook'larını kaldırmaya devam edebilirsiniz. Azure portalında, Otomasyon hesabınızın kaynak grubuna göz atın. **Başka bir aboneye Taşı'yı** **Move** > seçin.

![Kaynak grubu sayfası, başka bir aboneye taşıyın](../media/move-account/move-resources.png)

Kaynak grubunuzdaki taşımak istediğiniz kaynakları seçin. **Otomasyon hesabınızı,** **Runbook'unuzu**ve **Log Analytics çalışma alanı kaynaklarınızı** dahil ettiğinizden emin olun.

Taşıma tamamlandıktan sonra, her şeyin çalışması için gereken ek adımlar vardır.

## <a name="re-create-run-as-accounts"></a>Hesapları Olarak Çalıştır'ı yeniden oluşturma

[Hesaplar Olarak Çalıştır,](../manage-runas-account.md) Azure kaynaklarıyla kimlik doğrulaması yapmak için Azure Etkin Dizini'nde bir hizmet ilkesi oluşturun. Abonelikleri değiştirdiğinizde, Otomasyon hesabı artık varolan Run As hesabını kullanmaz.

Yeni abonelikte Otomasyon hesabınıza gidin ve **Hesap Ayarları**altında hesap **olarak Çalıştır'ı** seçin. Run As hesaplarının artık eksik olarak gösteriş olduğunu görürsünüz.

![Hesaplar eksik olduğu için çalıştır](../media/move-account/run-as-accounts.png)

Her Run As hesabını seçin. **Özellikler** sayfasında, Hesabı Çalıştır'ı silmek için **Sil'i** seçin.

> [!NOTE]
> Run As hesapları oluşturma veya görüntüleme izniniz yoksa, aşağıdaki iletiyi `You do not have permissions to create an Azure Run As account (service principal) and grant the Contributor role to the service principal.` görürsünüz: Bir Run As hesabını yapılandırmak için gereken izinler hakkında bilgi edinmek [için, Run As hesaplarını yapılandırmak için gereken İzinler'e](../manage-runas-account.md#permissions)bakın.

Çalıştır Hesapları silindikten sonra, **Azure Run As hesabı**altında **Oluştur'u** seçin. Hesap **Olarak Çalıştır** Ekle sayfasında, Hesap Olarak Çalıştır ve hizmet sorumlusu oluşturmak için **Oluştur'u** seçin. **Azure Klasik Çalıştır Hesabı**ile önceki adımları yineleyin.

## <a name="enable-solutions"></a>Çözümleri etkinleştirme

Run As hesaplarını yeniden oluşturduktan sonra, taşımadan önce kaldırdığınız çözümleri yeniden etkinleştirin. **Değişiklik İzleme ve Envanter ve** Güncelleme **Yönetimi'ni**açmak için Otomasyon hesabınızdaki ilgili yeteneği seçin. Taşındığınız Günlük Analizi çalışma alanını seçin ve **Etkinleştir'i**seçin.

![Taşınan Otomasyon hesabınızda çözümleri yeniden etkinleştirme](../media/move-account/reenable-solutions.png)

Mevcut Log Analytics çalışma alanını bağladığınızda çözümleriniz ile yerleşik makineler görünür olacaktır.

Mesai saatleri dışında **başlat/durdur** un vm'lerini açmak için çözümü yeniden dağıtmanız gerekir. **İlgili Kaynaklar**altında, **Başlat/Durdur VM'leri** > seçin Dağıtıma başlamak için**çözüm Oluştur** hakkında daha fazla bilgi edinin ve**etkinleştirin.** > 

Çözüm **Ekle** sayfasında, Log Analytics Çalışma Alanı ve Otomasyon hesabınızı seçin.

![Çözüm ekle menüsü](../media/move-account/add-solution-vm.png)

Çözümü yapılandırma yla ilgili ayrıntılı talimatlar için [Azure Otomasyonu'nda mesai dışı çözüm sırasında Başlat/Durdur VM'leri'ne](../automation-solution-vm-management.md)bakın.

## <a name="post-move-verification"></a>Taşıma sonrası doğrulama

Taşıma tamamlandığında, doğrulaması gereken görevlerin aşağıdaki listesini kontrol edin:

|Özellik|Testler|Sorun giderme bağlantısı|
|---|---|---|
|Runbook'lar|Bir runbook başarıyla çalıştırılabilir ve Azure kaynaklarına bağlanabilir.|[Sorun giderme runbook'ları](../troubleshoot/runbooks.md)
|Kaynak denetimi|Kaynak denetimi repo'nuzda manuel eşitleme çalıştırabilirsiniz.|[Kaynak kontrol entegrasyonu](../source-control-integration.md)|
|İzleme ve envanteri değiştirme|Makinelerinizden geçerli stok verilerini gördüğünüzden doğrulayın.|[Sorun giderme değişiklik izleme](../troubleshoot/change-tracking.md)|
|Güncelleştirme yönetimi|Makinelerinizi gördüğünüzden ve sağlıklı olduklarını doğrulayın.</br>Bir test yazılımı güncelleştirme dağıtımı çalıştırın.|[Sorun giderme güncelleştirme yönetimi](../troubleshoot/update-management.md)|
|Paylaşılan kaynaklar|[Kimlik Bilgileri,](../shared-resources/credentials.md) [Değişkenler,](../shared-resources/variables.md)vb. gibi paylaşılan tüm kaynaklarınızı gördüğünüzü doğrulayın.|

## <a name="next-steps"></a>Sonraki adımlar

Azure'da kaynak taşıma hakkında daha fazla bilgi edinmek için [azure'da kaynakları taşı 'ya](../../azure-resource-manager/management/move-support-resources.md)bakın.
