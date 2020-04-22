---
title: Azure Otomasyon hesabınızı başka bir aboneye taşıyın
description: Bu makalede, Otomasyon hesabınızın başka bir aboneye nasıl taşınır ı açıklanmaktadır.
services: automation
ms.service: automation
ms.subservice: process-automation
author: mgoedtel
ms.author: magoedte
ms.date: 03/11/2019
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 2dbe7dc171b6e0ec81c99a460a4f997eeb9e27a5
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81681886"
---
# <a name="move-your-azure-automation-account-to-another-subscription"></a>Azure Otomasyon hesabınızı başka bir aboneye taşıyın

Azure Otomasyonu, bazı kaynakları yeni bir kaynak grubuna veya aboneye taşımanızı sağlar. Kaynakları Azure portalı, PowerShell, Azure CLI veya REST API'de taşıyabilirsiniz. İşlem hakkında daha fazla bilgi edinmek [için bkz.](../../azure-resource-manager/management/move-resource-group-and-subscription.md)

Azure Otomasyon hesabı taşıyabileceğiniz kaynaklardan biridir. Bu makalede, Otomasyon hesaplarını başka bir kaynağa veya aboneye taşımayı öğreneceksiniz. Otomasyon hesabınızı taşımak için üst düzey adımlar şunlardır:

1. Çözümlerinizi kaldırın.
2. Çalışma alanınızın bağlantısını boşaltın.
3. Otomasyon hesabını taşıyın.
4. Run As hesaplarını silin ve yeniden oluşturun.
5. Çözümlerinizi yeniden etkinleştirin.

>[!NOTE]
>Bu makale yeni Azure PowerShell Az modülünü kullanacak şekilde güncelleştirilmiştir. En azından Aralık 2020'ye kadar hata düzeltmeleri almaya devam edecek olan AzureRM modülünü de kullanmaya devam edebilirsiniz. Yeni Az modülüyle AzureRM'nin uyumluluğu hakkında daha fazla bilgi edinmek için bkz. [Yeni Azure PowerShell Az modülüne giriş](https://docs.microsoft.com/powershell/azure/new-azureps-module-az?view=azps-3.5.0). Karma Runbook Worker'ınızdaki Az modül yükleme yönergeleri için Azure [PowerShell Modül'üne](https://docs.microsoft.com/powershell/azure/install-az-ps?view=azps-3.5.0)bakın. Otomasyon hesabınız için, Azure Otomasyonu'nda Azure [PowerShell modüllerini nasıl güncelleştirebileceğinizi](../automation-update-azure-modules.md)kullanarak modüllerinizi en son sürüme güncelleştirebilirsiniz.

## <a name="remove-solutions"></a>Çözümleri kaldırma

Çalışma alanınızı Otomasyon hesabınızdan kaldırmak için bu çözümleri çalışma alanınızdan kaldırmanız gerekir:

- Değişiklik İzleme ve Stok
- Güncelleştirme Yönetimi
- Hizmetin kapalı olduğu saatlerde VM’leri Başlatma/Durdurma

1. Azure portalda kaynak grubunuzu bulun.
2. Her çözümü bulun ve Kaynakları Sil sayfasında **Sil'i** tıklatın.

    ![Azure portalından çözümleri silme](../media/move-account/delete-solutions.png)

    İsterseniz, [Remove-AzResource](https://docs.microsoft.com/powershell/module/Az.Resources/Remove-AzResource?view=azps-3.7.0) cmdlet kullanarak çözümleri silebilirsiniz:

    ```azurepowershell-interactive
    $workspaceName = <myWorkspaceName>
    $resourceGroupName = <myResourceGroup>
    Remove-AzResource -ResourceType 'Microsoft.OperationsManagement/solutions' -ResourceName "ChangeTracking($workspaceName)" -ResourceGroupName $resourceGroupName
    Remove-AzResource -ResourceType 'Microsoft.OperationsManagement/solutions' -ResourceName "Updates($workspaceName)" -ResourceGroupName $resourceGroupName
    Remove-AzResource -ResourceType 'Microsoft.OperationsManagement/solutions' -ResourceName "Start-Stop-VM($workspaceName)" -ResourceGroupName $resourceGroupName
    ```

### <a name="remove-alert-rules-for-the-startstop-vms-during-off-hours-solution"></a>Mesai saatleri dışında başlat/durdur vm'leri için uyarı kurallarını kaldırma

Mesai saatleri dışında Başlat/Durdur çözümü için, çözümün oluşturduğu uyarı kurallarını da kaldırmanız gerekir.

1. Azure portalında, kaynak grubunuza gidin ve **İzleme** > **Uyarıları** > **Yönetme uyarı kurallarını**seçin.

![Uyarıyı Yönet kurallarının seçimini gösteren uyarılar sayfası](../media/move-account/alert-rules.png)

2. Kurallar sayfasında, bu kaynak grubunda yapılandırılan uyarıların listesini görmeniz gerekir. Çözüm şu kuralları oluşturur:

    * AutoStop_VM_Child
    * ScheduledStartStop_Parent
    * SequencedStartStop_Parent

3. Kuralları teker teker seçin ve kaldırmak için **Sil'i** tıklatın.

    ![Seçili kurallar için silme onayı isteyen kurallar sayfası](../media/move-account/delete-rules.png)

    > [!NOTE]
    > Kurallar sayfasında herhangi bir uyarı kuralı görmüyorsanız, devre dışı bırakılmış olabileceğiniz için devre dışı bırakılmış olabileceğiniz için **Durum** alanını Devre Dışı Bırakılmış olarak değiştirin.

4. Uyarı kuralları kaldırıldığında, mesai saatleri dışında çözüm bildirimleri sırasında VM'leri Başlat/Durdur için oluşturulan eylem grubunu kaldırmanız gerekir. Azure portalında,**Uyarıları** > **Yönet eylem gruplarını** **izleyin'i** > seçin.

5. **StartStop_VM_Notification**seçin. 

6. Eylem grubu sayfasında **Sil'i**seçin.

    ![Eylem grubu sayfası](../media/move-account/delete-action-group.png)

    İsterseniz, [Remove-AzActionGroup](https://docs.microsoft.com/powershell/module/az.monitor/remove-azactiongroup?view=azps-3.7.0) cmdlet'i kullanarak eylem grubunuzu silebilirsiniz:

    ```azurepowershell-interactive
    Remove-AzActionGroup -ResourceGroupName <myResourceGroup> -Name StartStop_VM_Notification
    ```

## <a name="unlink-your-workspace"></a>Çalışma alanınızın bağlantısını boşaltın

Artık çalışma alanınızın bağlantısını çözebilirsiniz:

1. Azure portalında, **Otomasyon hesabı** > **Yla İlgili Kaynaklar** > **Bağlantılı çalışma alanını**seçin. 

2. Çalışma alanını Otomasyon hesabınızdan çıkarmak için **Bağlantıyı Aç çalışma alanını** seçin.

    ![Bir Otomasyon hesabından çalışma alanını açma](../media/move-account/unlink-workspace.png)

## <a name="move-your-automation-account"></a>Otomasyon hesabınızı taşıyın

Artık Otomasyon hesabınızı ve runbook'larını taşıyabilirsiniz. 

1. Azure portalında, Otomasyon hesabınızın kaynak grubuna göz atın. **Başka bir aboneye Taşı'yı** **Move** > seçin.

    ![Kaynak grubu sayfası, başka bir aboneye taşıyın](../media/move-account/move-resources.png)

2. Kaynak grubunuzdaki taşımak istediğiniz kaynakları seçin. Otomasyon hesabınızı, runbook'larınızı ve Log Analytics çalışma alanı kaynaklarınızı dahil ettiğinizden emin olun.

## <a name="recreate-run-as-accounts"></a>Hesapları Yeniden Çalıştır

[Hesaplar Olarak Çalıştır,](../manage-runas-account.md) Azure kaynaklarıyla kimlik doğrulaması yapmak için Azure Etkin Dizini'nde bir hizmet ilkesi oluşturun. Abonelikleri değiştirdiğinizde, Otomasyon hesabı artık varolan Run As hesabını kullanmaz. Run As hesaplarını yeniden oluşturmak için:

1. Yeni abonelikte Otomasyon hesabınıza gidin ve **Hesap Ayarları**altında hesap **olarak Çalıştır'ı** seçin. Run As hesaplarının artık eksik olarak gösteriş olduğunu görürsünüz.

    ![Hesaplar eksik olduğu için çalıştır](../media/move-account/run-as-accounts.png)

2. Özellikler sayfasındaki **Sil** düğmesini kullanarak Hesapları Teker Teker Çalıştır'ı silin. 

    > [!NOTE]
    > Run As hesapları oluşturma veya görüntüleme izniniz yoksa, aşağıdaki iletiyi `You do not have permissions to create an Azure Run As account (service principal) and grant the Contributor role to the service principal.` görürsünüz: Bir Run As hesabını yapılandırmak için gereken izinler hakkında bilgi edinmek [için, Run As hesaplarını yapılandırmak için gereken İzinler'e](../manage-runas-account.md#permissions)bakın.

3. Run As hesaplarını sildikten sonra Azure **Run As hesabı**altında **Oluştur'u** seçin. 

4. Hesap Olarak Çalıştır Ekle sayfasında, Hesap Olarak Çalıştır ve hizmet sorumlusu oluşturmak için **Oluştur'u** seçin. 

5. Yukarıdaki adımları Azure Classic Run As hesabıyla tekrarlayın.

## <a name="enable-solutions"></a>Çözümleri etkinleştirme

Run As hesaplarını yeniden oluşturduktan sonra, taşımadan önce kaldırdığınız çözümleri yeniden etkinleştirmeniz gerekir: 

1. İzleme ve Stok Değiştirme çözümünü açmak için Otomasyon hesabınızda İzleme ve Envanter'i değiştir'i seçin. Taşındığınız Günlük Analizi çalışma alanını seçin ve **Etkinleştir'i**seçin.

2. Güncelleştirme Yönetimi çözümü için adım 1'i yineleyin.

    ![Taşınan Otomasyon hesabınızda çözümleri yeniden etkinleştirme](../media/move-account/reenable-solutions.png)

3. Çözümleriniz ile yerleşik makineler, mevcut Log Analytics çalışma alanını bağladığınızda görünür. Kapalı saatlerde Başlat/Durdur VM'lerini açmak için çözümü yeniden dağıtmanız gerekir. **İlgili Kaynaklar**altında, **Başlat/Durdur VM'leri** > seçin Dağıtıma başlamak için**çözüm Oluştur** hakkında daha fazla bilgi edinin ve**etkinleştirin.** > 

4. Çözüm Ekle sayfasında, Log Analytics çalışma alanı nızı ve Otomasyon hesabınızı seçin.

    ![Çözüm ekle menüsü](../media/move-account/add-solution-vm.png)

5. Azure Otomasyonu'nda [mesai saatleri dışında başlat/durdur vm'lerde](../automation-solution-vm-management.md)açıklandığı şekilde çözümü yapılandırın.

## <a name="verify-the-move"></a>Hareketi doğrula

Taşıma tamamlandığında, aşağıda listelenen özelliklerin etkin olduğunu doğrulayın. 

|Özellik|Testler|Sorun giderme|
|---|---|---|
|Runbook'lar|Bir runbook başarıyla çalıştırılabilir ve Azure kaynaklarına bağlanabilir.|[Runbook sorunlarını giderme](../troubleshoot/runbooks.md)
|Kaynak denetimi|Kaynak denetim deponuzda el ile eşitleme çalıştırabilirsiniz.|[Kaynak denetimi tümleştirmesi](../source-control-integration.md)|
|İzleme ve envanteri değiştirme|Makinelerinizden geçerli stok verilerini gördüğünüzden doğrulayın.|[Sorun giderme değişiklik izleme](../troubleshoot/change-tracking.md)|
|Güncelleştirme yönetimi|Makinelerinizi gördüğünüzden ve sağlıklı olduklarını doğrulayın.</br>Bir test yazılımı güncelleştirme dağıtımı çalıştırın.|[Sorun giderme güncelleştirme yönetimi](../troubleshoot/update-management.md)|
|Paylaşılan kaynaklar|[Kimlik bilgileri,](../shared-resources/credentials.md)değişkenler ve [benzerleri](../shared-resources/variables.md)gibi paylaşılan tüm kaynaklarınızı gördüğünüzden doğrulayın.|

## <a name="next-steps"></a>Sonraki adımlar

Azure'da kaynak taşıma hakkında daha fazla bilgi edinmek için [azure'da kaynakları taşı 'ya](../../azure-resource-manager/management/move-support-resources.md)bakın.
