---
title: Azure Otomasyonu 'nda güncelleştirme, değişiklik izleme ve envanter çözümlerini ekleme
description: Güncelleştirme ve değişiklik izleme çözümlerini Azure Otomasyonu’na nasıl ekleyeceğinizi öğrenin.
services: automation
ms.topic: tutorial
ms.date: 05/10/2018
ms.custom: mvc
ms.openlocfilehash: 721157c333e381799ef08930c667c51a51a4fd6a
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "81457629"
---
# <a name="onboard-update-change-tracking-and-inventory-solutions-to-azure-automation"></a>Azure Otomasyonu 'nda güncelleştirme, değişiklik izleme ve envanter çözümlerini ekleme

Bu öğreticide VM’lere yönelik Güncelleştirme, Değişiklik İzleme ve Sayım çözümlerini Azure Otomasyonu’na nasıl otomatik olarak ekleyeceğinizi öğreneceksiniz:

> [!div class="checklist"]
> * Bir Azure VM ekleme
> * Çözümleri etkinleştirme
> * Modülleri yükleme ve güncelleştirme
> * Ekleme runbook’unu içeri aktarma
> * Runbook’u başlatma

>[!NOTE]
>Bu makale yeni Azure PowerShell Az modülünü kullanacak şekilde güncelleştirilmiştir. En azından Aralık 2020'ye kadar hata düzeltmeleri almaya devam edecek olan AzureRM modülünü de kullanmaya devam edebilirsiniz. Yeni Az modülüyle AzureRM'nin uyumluluğu hakkında daha fazla bilgi edinmek için bkz. [Yeni Azure PowerShell Az modülüne giriş](https://docs.microsoft.com/powershell/azure/new-azureps-module-az?view=azps-3.5.0). Karma runbook çalışanınız hakkında az Module yükleme yönergeleri için bkz. [Azure PowerShell modülünü yükleme](https://docs.microsoft.com/powershell/azure/install-az-ps?view=azps-3.5.0). Otomasyon hesabınız için, [Azure Otomasyonu 'nda Azure PowerShell modüllerini güncelleştirme](automation-update-azure-modules.md)' yi kullanarak modüllerinizi en son sürüme güncelleştirebilirsiniz.

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlamak için aşağıdakiler gereklidir:

* Azure aboneliği. Henüz bir hesabınız yoksa [MSDN abone avantajlarınızı etkinleştirebilir](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) veya [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)için kaydolabilirsiniz.
* Makineleri yönetmek için [Otomasyon hesabı](automation-offering-get-started.md).
* Sisteme eklenecek bir [sanal makine](../virtual-machines/windows/quick-create-portal.md).

## <a name="onboard-an-azure-vm"></a>Bir Azure VM ekleme

Makine eklemenin birden fazla yolu vardır. Çözümü [bir sanal makineden](automation-onboard-solutions-from-vm.md), [birden çok makineye gözatmadan, ](automation-onboard-solutions-from-browse.md) [Otomasyon hesabınızdan](automation-onboard-solutions-from-automation-account.md) veya runbook ile ekleyebilirsiniz. Bu öğretici bir runbook üzerinden Güncelleştirme Yönetimi’ni etkinleştirme konusunda size yol gösterir. Uygun ölçekte Azure Sanal Makinelerin eklenmesi için mevcut bir VM’ye Değişiklik izleme veya Güncelleştirme yönetimi çözümünün eklenmesi gerekir. Bu adımda, bir sanal makineye Güncelleştirme yönetimi ve Değişiklik izleme özelliğini eklersiniz.

### <a name="enable-change-tracking-and-inventory"></a>Değişiklik İzlemeyi ve Sayımı Etkinleştirme

Değişiklik İzleme ve envanter çözümleri, sanal makinelerinizdeki değişiklikleri ve [envanteri](automation-vm-inventory.md) [izlemenize](automation-vm-change-tracking.md) olanak sağlar. Bu adımda, bir sanal makinede çözümleri etkinleştirirsiniz.

1. Azure portal **Otomasyon hesapları**' nı seçin ve ardından listeden Otomasyon hesabınızı seçin.
1. **Yapılandırma yönetimi**altında **Stok** ' ı seçin.
1. Mevcut bir Log Analytics çalışma alanını seçin veya yenisini oluşturun. 
1. **Etkinleştir**' e tıklayın.

    ![Güncelleştirme çözümü ekleme](media/automation-onboard-solutions/inventory-onboard.png)

### <a name="enable-update-management"></a>Güncelleştirme Yönetimi’ni etkinleştirme

Güncelleştirme Yönetimi çözümü, Azure Windows VM’leriniz için güncelleştirmeleri ve yamaları yönetmenizi sağlar. Kullanılabilir güncelleştirmelerin durumunu değerlendirebilir, gerekli güncelleştirmelerin yüklemesini zamanlayabilir ve güncelleştirmelerin VM’ye başarıyla uygulandığını doğrulamak için dağıtım sonuçlarını gözden geçirebilirsiniz. Bu adımda çözümü VM’niz için etkinleştirirsiniz.

1. Otomasyon hesabınızda, **güncelleştirme yönetimi** bölümünde **güncelleştirme yönetimi** ' yi seçin.
1. Seçilen Log Analytics çalışma alanı, önceki adımda kullanılan çalışma alanıdır. Güncelleştirme yönetimi çözümünü eklemek için **Etkinleştir**’e tıklayın. Güncelleştirme yönetimi çözümü yüklenirken mavi renkli bir başlık gösterilir. 

    ![Güncelleştirme çözümü ekleme](media/automation-onboard-solutions/update-onboard.png)

### <a name="select-azure-vm-to-be-managed"></a>Yönetilecek Azure VM’yi seçme

Çözümler etkinleştirildiğine göre artık bu çözümleri eklemek için bir Azure VM ekleyebilirsiniz.

1. Otomasyon hesabınızdan **yapılandırma yönetimi**altında **değişiklik izleme** ' yi seçin. 
2. Değişiklik izleme sayfasında, sanal makinelerinizi eklemek için **Azure VM 'Leri Ekle** ' ye tıklayın.

3. Listeden VM 'nizi seçin ve **Etkinleştir**' e tıklayın. Bu eylem, sanal makine için Değişiklik İzleme ve envanter çözümlerini sunar.

   ![VM için güncelleştirme çözümünü etkinleştirme](media/automation-onboard-solutions/enable-change-tracking.png)

4. VM ekleme bildirimi tamamlandığında **güncelleştirme yönetimi**altında **güncelleştirme yönetimi** ' ni seçin.

5. VM 'nizi eklemek için **Azure VM 'Leri Ekle** ' yi seçin.

6. Listeden VM’nizi seçin ve **Etkinleştir** seçeneğini belirleyin. Bu eylem, sanal makine için Güncelleştirme Yönetimi çözümüne izin vermez.

   ![VM için güncelleştirme çözümünü etkinleştirme](media/automation-onboard-solutions/enable-update.png)

> [!NOTE]
> Diğer çözümün tamamlanmasını beklerseniz, bir sonraki çözümü etkinleştirirken şu iletiyi alırsınız:`Installation of another solution is in progress on this or a different virtual machine. When that installation completes the Enable button is enabled, and you can request installation of the solution on this virtual machine.`

## <a name="install-and-update-modules"></a>Modülleri yükleme ve güncelleştirme

En son Azure modüllerine güncelleştirmeniz ve [az. Operationalınsights](https://docs.microsoft.com/powershell/module/az.operationalinsights/?view=azps-3.7.0) modülünü içeri aktararak çözüm ekleme işlemini başarıyla otomatikleştirmeniz gerekir.

## <a name="update-azure-modules"></a>Azure Modüllerini etkinleştirme

1. Otomasyon hesabınızda, **paylaşılan kaynaklar**altında **modüller** ' i seçin. 
2. Azure modüllerini en son sürüme güncelleştirmek için **Azure Modüllerini Güncelleştir** seçeneğini belirleyin. 
3. Var olan tüm Azure modüllerini en son sürüme güncelleştirmek için **Evet** ' e tıklayın.

![Modül](media/automation-onboard-solutions/update-modules.png) A güncelleştirme

### <a name="install-azoperationalinsights-module"></a>Az. Operationalınsights modülünü yüklensin

1. Otomasyon hesabında, **paylaşılan kaynaklar**altında **modüller** ' i seçin. 
2. Modül galerisini açmak için **tarayıcı Galerisi** ' ni seçin. 
3. Az. Operationalınsights araması yapın ve bu modülü Otomasyon hesabına aktarın.

![OperationalInsights modülünü içeri aktarma](media/automation-onboard-solutions/import-operational-insights-module.png)

## <a name="import-the-onboarding-runbook"></a>Ekleme runbook’unu içeri aktarma

1. Otomasyon hesabınızda, **Işlem Otomasyonu**altında **runbook 'lar** ' ı seçin.
1. **Galeriye gözat** seçeneğini belirleyin.
1. `update and change tracking` arayın.
3. Runbook 'u seçin ve kaynak görüntüleme sayfasında **Içeri aktar** ' a tıklayın. 
4. Runbook 'u Otomasyon hesabına aktarmak için **Tamam** ' ı tıklatın.

   ![Ekleme runbook’unu içeri aktarma](media/automation-onboard-solutions/import-from-gallery.png)

6. Runbook sayfasında, **Düzenle**' ye tıklayın ve ardından **Yayımla**' yı seçin. 
7. Runbook 'U Yayımla bölmesinde, runbook 'u yayımlamak için **Evet** ' i tıklatın.

## <a name="start-the-runbook"></a>Runbook’u başlatma

Bu runbook 'u başlatmak için değişiklik izleme veya güncelleştirme çözümlerini bir Azure VM 'ye eklendi sahip olmanız gerekir. Parametreler için çözümün eklenmiş olduğu mevcut bir sanal makine ve kaynak grubu gereklidir.

1. **Enable-MultipleSolution** runbook 'unu açın.

   ![Çoklu çözüm runbook’ları](media/automation-onboard-solutions/runbook-overview.png)

1. Başlat düğmesine tıklayın ve parametreler için aşağıdaki değerleri girin.

   * **VMNAME** - Boş bırakın. Güncelleştirme veya değişiklik izleme çözümünün ekleneceği mevcut bir VM’nin adıdır. Bu değeri bış bıraktığınızda kaynak grubundaki tüm VM’lere ekleme yapılır.
   * **VMRESOURCEGROUP** - Ekleme yapılacak VM’lere ilişkin kaynak grubunun adıdır.
   * **SUBSCRIPTIONID** - Boş bırakın. Ekleme yapılacak yeni VM’nin abonelik kimliğidir. Boş bırakılırsa çalışma alanının aboneliği kullanılır. Farklı bir abonelik kimliği belirtildiğinde, bu otomasyon hesabına yönelik RunAs hesabının da söz konusu abonelik için katkıda bulunan olarak eklenmesi gerekir.
   * **ALREADYONBOARDEDVM** - Güncelleştirme veya Değişiklik İzleme çözümünün el ile eklendiği VM’nin adıdır.
   * **Alreadyonboardedvmresourcegroup** -VM 'nin ait olduğu kaynak grubunun adı.
   * **Solutiontype** - **güncellemeleri** veya **ChangeTracking**girin.

   ![Enable-MultipleSolution runbook parametreleri](media/automation-onboard-solutions/runbook-parameters.png)

1. Runbook işini başlatmak için **Tamam**’ı seçin.
1. Runbook işi sayfasında ilerlemeyi ve hataları izleyin.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bir sanal makineyi Güncelleştirme Yönetimi kaldırmak için:

1. Log Analytics çalışma alanınızda, VM 'yi, kapsam yapılandırması `MicrosoftDefaultScopeConfig-Updates`için kaydedilen aramadan kaldırın. Kayıtlı aramalar, çalışma alanınızda **genel** altında bulunabilir.
2. [Windows için Log Analytics aracısını](../azure-monitor/learn/quick-collect-windows-computer.md#clean-up-resources) veya [Linux için Log Analytics aracısını](../azure-monitor/learn/quick-collect-linux-computer.md#clean-up-resources)kaldırın.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Bir Azure sanal makinesine el ile ekleme yapma.
> * Gerekli Azure modüllerini yükleme ve güncelleştirme.
> * Azure VM’lerine ekleme yapan bir runbook’u içeri aktarma.
> * Azure VM’lerine otomatik olarak ekleme yapan runbook’u başlatma.

Runbook’ların zamanlanması hakkında daha fazla bilgi edinmek için şu bağlantıyı izleyin.

> [!div class="nextstepaction"]
> [Runbook’ları zamanlama](automation-schedules.md).
