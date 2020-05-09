---
title: Azure Otomasyonu hesabınızı başka bir aboneliğe taşıma
description: Bu makalede Otomasyon hesabınızı başka bir aboneliğe nasıl taşıyacağınız açıklanır.
services: automation
ms.service: automation
ms.subservice: process-automation
author: mgoedtel
ms.author: magoedte
ms.date: 03/11/2019
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: bfb2f2d1d0f6a0d11784847344cd3dbcafdb0959
ms.sourcegitcommit: 0fda81f271f1a668ed28c55dcc2d0ba2bb417edd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2020
ms.locfileid: "82900990"
---
# <a name="move-your-azure-automation-account-to-another-subscription"></a>Azure Otomasyonu hesabınızı başka bir aboneliğe taşıma

Azure Otomasyonu, bazı kaynakları yeni bir kaynak grubuna veya aboneliğe taşımanızı sağlar. Kaynakları Azure portal, PowerShell, Azure CLı veya REST API üzerinden taşıyabilirsiniz. İşlem hakkında daha fazla bilgi edinmek için bkz. [kaynakları yeni bir kaynak grubuna veya aboneliğe taşıma](../../azure-resource-manager/management/move-resource-group-and-subscription.md).

Otomasyon hesabı, taşıyacağınız kaynaklardan biridir. Bu makalede, Otomasyon hesaplarını başka bir kaynağa veya aboneliğe taşımayı öğreneceksiniz. Otomasyon hesabınızı taşımaya yönelik üst düzey adımlar şunlardır:

1. Çözümlerinizi kaldırın.
2. Çalışma alanınızın bağlantısını kaldırın.
3. Otomasyon hesabını taşıyın.
4. Farklı Çalıştır hesaplarını silin ve yeniden oluşturun.
5. Çözümlerinizi yeniden etkinleştirin.

>[!NOTE]
>Bu makalede Azure PowerShell az Module ile çalışırsınız. Azurerd modülünü kullanmaya devam edebilirsiniz. Az Module ve Azurerd uyumluluğu hakkında daha fazla bilgi edinmek için, bkz. [new Azure PowerShell konusuna giriş az Module](https://docs.microsoft.com/powershell/azure/new-azureps-module-az?view=azps-3.5.0). Karma runbook çalışanınız hakkında az Module yükleme yönergeleri için bkz. [Azure PowerShell modülünü yükleme](https://docs.microsoft.com/powershell/azure/install-az-ps?view=azps-3.5.0). Otomasyon hesabınız için, [Azure Otomasyonu 'nda Azure PowerShell modüllerini güncelleştirme](../automation-update-azure-modules.md)' yi kullanarak modüllerinizi en son sürüme güncelleştirebilirsiniz.

## <a name="remove-solutions"></a>Çözümleri kaldır

Çalışma alanınızın Otomasyon hesabınızla bağlantısını kaldırmak için bu çözümleri çalışma alanınızdan kaldırmanız gerekir:

- Değişiklik İzleme ve Stok
- Güncelleştirme Yönetimi
- Hizmetin kapalı olduğu saatlerde VM’leri Başlatma/Durdurma

1. Azure portalda kaynak grubunuzu bulun.
2. Her bir çözümü bulun ve **kaynakları Sil** sayfasında **Sil** ' i seçin.

    ![Azure portal çözümleri silme ekran görüntüsü](../media/move-account/delete-solutions.png)

İsterseniz, [Remove-AzResource](https://docs.microsoft.com/powershell/module/Az.Resources/Remove-AzResource?view=azps-3.7.0) cmdlet 'ini kullanarak çözümleri silebilirsiniz:

```azurepowershell-interactive
$workspaceName = <myWorkspaceName>
$resourceGroupName = <myResourceGroup>
Remove-AzResource -ResourceType 'Microsoft.OperationsManagement/solutions' -ResourceName "ChangeTracking($workspaceName)" -ResourceGroupName $resourceGroupName
Remove-AzResource -ResourceType 'Microsoft.OperationsManagement/solutions' -ResourceName "Updates($workspaceName)" -ResourceGroupName $resourceGroupName
Remove-AzResource -ResourceType 'Microsoft.OperationsManagement/solutions' -ResourceName "Start-Stop-VM($workspaceName)" -ResourceGroupName $resourceGroupName
```

### <a name="remove-alert-rules-for-the-startstop-vms-during-off-hours-solution"></a>"Saatlerde VM 'Leri Başlat/Durdur" çözümü için uyarı kurallarını kaldır

Bu çözüm için, çözüm tarafından oluşturulan uyarı kurallarını da kaldırmanız gerekir.

1. Azure Portal, kaynak grubunuza gidip **izleme** > **uyarıları** > **Uyarı kurallarını yönet**' i seçin.

   ![Uyarı kurallarını yönet seçimini gösteren uyarılar sayfasının ekran görüntüsü](../media/move-account/alert-rules.png)

2. **Kurallar** sayfasında, bu kaynak grubunda yapılandırılmış uyarıların bir listesini görmeniz gerekir. Çözüm şu kuralları oluşturur:

    * AutoStop_VM_Child
    * ScheduledStartStop_Parent
    * SequencedStartStop_Parent

3. Kuralları tek seferde seçin ve **Sil** ' i seçerek kaldırın.

    ![Seçili kurallar için silme onayı isteyen kurallar sayfasının ekran görüntüsü](../media/move-account/delete-rules.png)

    > [!NOTE]
    > **Kurallar** sayfasında herhangi bir uyarı kuralı görmüyorsanız, devre dışı bırakılmış uyarıları göstermek için **durum** alanını **devre dışı** olarak değiştirin. Bunları devre dışı bırakmış olabilirsiniz.

4. Uyarı kurallarını kaldırdığınızda, "çalışma saatleri dışında VM 'Leri Başlat/Durdur" çözüm bildirimleri için oluşturulan eylem grubunu kaldırmanız gerekir. Azure Portal, **izleme** > **uyarıları** > **işlem gruplarını yönet**' i seçin.

5. **StartStop_VM_Notification**seçin. 

6. Eylem grubu sayfasında **Sil**' i seçin.

    ![Eylem grubu sayfasının ekran görüntüsü](../media/move-account/delete-action-group.png)

İsterseniz, [Remove-AzActionGroup](https://docs.microsoft.com/powershell/module/az.monitor/remove-azactiongroup?view=azps-3.7.0) cmdlet 'ini kullanarak eylem grubunuzu silebilirsiniz:

```azurepowershell-interactive
Remove-AzActionGroup -ResourceGroupName <myResourceGroup> -Name StartStop_VM_Notification
```

## <a name="unlink-your-workspace"></a>Çalışma alanınızın bağlantısını kaldırma

Artık çalışma alanınızın bağlantısını kaldırabilirsiniz:

1. Azure Portal **Otomasyon hesabı** > **ilişkili kaynaklar** > **bağlantılı çalışma alanı**' nı seçin. 

2. Çalışma alanının Otomasyon hesabından bağlantısını kaldırmak için **çalışma alanının bağlantısını kaldır** ' ı seçin.

    ![Bir Otomasyon hesabından çalışma alanının bağlantısını kaldırma ekran görüntüsü](../media/move-account/unlink-workspace.png)

## <a name="move-your-automation-account"></a>Otomasyon hesabınızı taşıyın

Artık Otomasyon hesabınızı ve Runbook 'larını taşıyabilirsiniz. 

1. Azure portal, Otomasyon hesabınızın kaynak grubuna gidin. **Taşımayı** > **başka bir aboneliğe**taşıma ' yı seçin.

    ![Kaynak grubu sayfasının ekran görüntüsü, başka bir aboneliğe taşıma](../media/move-account/move-resources.png)

2. Kaynak grubunuzda, taşımak istediğiniz kaynakları seçin. Otomasyon hesabınızı, runbook 'larını ve Log Analytics çalışma alanı kaynaklarınızı dahil edin.

## <a name="re-create-run-as-accounts"></a>Farklı Çalıştır hesaplarını yeniden oluşturma

[Farklı Çalıştır hesapları](../manage-runas-account.md) Azure kaynaklarıyla kimlik doğrulaması yapmak için Azure Active Directory bir hizmet sorumlusu oluşturur. Abonelikleri değiştirdiğinizde, Otomasyon hesabı artık mevcut farklı çalıştır hesabını kullanmaz. Farklı Çalıştır hesaplarını yeniden oluşturmak için:

1. Yeni abonelikte Otomasyon hesabınıza gidin ve **Hesap ayarları**altında **Farklı Çalıştır hesapları** ' nı seçin. Farklı Çalıştır hesaplarının şimdi tamamlanmamış olarak gösterileceğini görürsünüz.

    ![Farklı Çalıştır hesaplarının, tamamlanmamış olduğunu gösteren ekran görüntüsü](../media/move-account/run-as-accounts.png)

2. Farklı Çalıştır hesaplarını, **Özellikler** sayfasında **Sil** ' i seçerek tek seferde silin. 

    > [!NOTE]
    > Farklı Çalıştır hesaplarını oluşturma veya görüntüleme izniniz yoksa şu iletiyi görürsünüz: `You do not have permissions to create an Azure Run As account (service principal) and grant the Contributor role to the service principal.` daha fazla bilgi Için, [Farklı Çalıştır hesaplarını yapılandırmak için gereken izinler](../manage-runas-account.md#permissions)bölümüne bakın.

3. Farklı Çalıştır hesaplarını sildikten sonra **Azure farklı çalıştır hesabı**altında **Oluştur** ' u seçin. 

4. Farklı Çalıştır hesabı ve hizmet sorumlusu oluşturmak için **Azure farklı çalıştır hesabı ekle** sayfasında **Oluştur** ' u seçin. 

5. Azure klasik farklı çalıştır hesabı ile yukarıdaki adımları tekrarlayın.

## <a name="enable-solutions"></a>Çözümleri etkinleştirme

Farklı Çalıştır hesaplarını yeniden oluşturduktan sonra, taşımadan önce kaldırdığınız çözümleri yeniden etkinleştirmeniz gerekir: 

1. "Değişiklik İzleme ve envanter" çözümünü açmak için Otomasyon hesabınızda **değişiklik izleme ve envanter** ' ı seçin. Üzerine taşıdığınız Log Analytics çalışma alanını seçin ve **Etkinleştir**' i seçin.

2. "Güncelleştirme Yönetimi" çözümü için 1. adımı yineleyin.

    ![Taşınan Otomasyon hesabınızdaki çözümlerin yeniden etkinleştirilmesinin ekran görüntüsü](../media/move-account/reenable-solutions.png)

3. Çözümlerinizle eklendi olan makineler, mevcut Log Analytics çalışma alanını bağladığınızda görülebilir. "Saatlerde VM 'Leri Başlat/Durdur" çözümünü açmak için çözümü yeniden dağıtmanız gerekir. **İlgili kaynaklar**' ın altında, **VM 'leri** > Başlat/Durdur ' u seçin ve dağıtımı başlatmak için**çözüm** > **oluşturma** ' yı etkinleştirin.

4. **Çözüm Ekle** sayfasında, Log Analytics çalışma alanınızı ve otomasyon hesabınızı seçin.

    ![Çözüm Ekle menüsünün ekran görüntüsü](../media/move-account/add-solution-vm.png)

5. Çözümü [Azure Otomasyonu 'ndaki saatlerde VM 'Leri başlatma/durdurma çözümünde](../automation-solution-vm-management.md)açıklandığı şekilde yapılandırın.

## <a name="verify-the-move"></a>Taşımayı doğrulama

Taşıma işlemi tamamlandığında, aşağıda listelenen yeteneklerin etkinleştirildiğini doğrulayın. 

|Özellik|Testler|Sorun giderme|
|---|---|---|
|Runbook'lar|Runbook, Azure kaynaklarına başarıyla çalıştırıp bağlanabilir.|[Runbook sorunlarını giderme](../troubleshoot/runbooks.md)
|Kaynak denetimi|Kaynak denetimi deponuzda el ile eşitleme gerçekleştirebilirsiniz.|[Kaynak denetimi tümleştirmesi](../source-control-integration.md)|
|Değişiklik izleme ve envanter|Makinelerinizden güncel envanter verilerini gördiğinizi doğrulayın.|[Değişiklik izleme sorunlarını giderme](../troubleshoot/change-tracking.md)|
|Güncelleştirme yönetimi|Makinelerinizi görmediğinizi ve sağlıklı olduğunu doğrulayın.</br>Bir test yazılım güncelleştirme dağıtımı çalıştırın.|[Güncelleştirme yönetimi sorunlarını giderme](../troubleshoot/update-management.md)|
|Paylaşılan kaynaklar|[Kimlik bilgileri](../shared-resources/credentials.md) ve [değişkenler](../shared-resources/variables.md)gibi tüm paylaşılan kaynaklarınızı gördiğinizi doğrulayın.|

## <a name="next-steps"></a>Sonraki adımlar

Azure 'da kaynakları taşıma hakkında daha fazla bilgi edinmek için bkz. [Azure 'da kaynakları taşıma](../../azure-resource-manager/management/move-support-resources.md).
