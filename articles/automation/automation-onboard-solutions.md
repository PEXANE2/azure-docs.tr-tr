---
title: Azure Otomasyonu için yerleşik güncelleştirme, izleme değişikliği ve envanter çözümleri
description: Güncelleştirme ve değişiklik izleme çözümlerini Azure Otomasyonu’na nasıl ekleyeceğinizi öğrenin.
services: automation
ms.topic: tutorial
ms.date: 05/10/2018
ms.custom: mvc
ms.openlocfilehash: 721157c333e381799ef08930c667c51a51a4fd6a
ms.sourcegitcommit: b55d7c87dc645d8e5eb1e8f05f5afa38d7574846
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81457629"
---
# <a name="onboard-update-change-tracking-and-inventory-solutions-to-azure-automation"></a>Azure Otomasyonu için yerleşik güncelleştirme, izleme değişikliği ve envanter çözümleri

Bu öğreticide VM’lere yönelik Güncelleştirme, Değişiklik İzleme ve Sayım çözümlerini Azure Otomasyonu’na nasıl otomatik olarak ekleyeceğinizi öğreneceksiniz:

> [!div class="checklist"]
> * Bir Azure VM ekleme
> * Çözümleri etkinleştirme
> * Modülleri yükleme ve güncelleştirme
> * Ekleme runbook’unu içeri aktarma
> * Runbook’u başlatma

>[!NOTE]
>Bu makale yeni Azure PowerShell Az modülünü kullanacak şekilde güncelleştirilmiştir. En azından Aralık 2020'ye kadar hata düzeltmeleri almaya devam edecek olan AzureRM modülünü de kullanmaya devam edebilirsiniz. Yeni Az modülüyle AzureRM'nin uyumluluğu hakkında daha fazla bilgi edinmek için bkz. [Yeni Azure PowerShell Az modülüne giriş](https://docs.microsoft.com/powershell/azure/new-azureps-module-az?view=azps-3.5.0). Karma Runbook Worker'ınızdaki Az modül yükleme yönergeleri için Azure [PowerShell Modül'üne](https://docs.microsoft.com/powershell/azure/install-az-ps?view=azps-3.5.0)bakın. Otomasyon hesabınız için, Azure Otomasyonu'nda Azure [PowerShell modüllerini nasıl güncelleştirebileceğinizi](automation-update-azure-modules.md)kullanarak modüllerinizi en son sürüme güncelleştirebilirsiniz.

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlamak için aşağıdakiler gereklidir:

* Azure aboneliği. Henüz bir hesabınız yoksa, [MSDN abone avantajlarınızı etkinleştirebilir](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) veya ücretsiz bir [hesaba](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)kaydolabilirsiniz.
* Makineleri yönetmek için [Otomasyon hesabı](automation-offering-get-started.md).
* Sisteme eklenecek bir [sanal makine](../virtual-machines/windows/quick-create-portal.md).

## <a name="onboard-an-azure-vm"></a>Bir Azure VM ekleme

Makine eklemenin birden fazla yolu vardır. Çözümü [bir sanal makineden](automation-onboard-solutions-from-vm.md), [birden çok makineye gözatmadan, ](automation-onboard-solutions-from-browse.md) [Otomasyon hesabınızdan](automation-onboard-solutions-from-automation-account.md) veya runbook ile ekleyebilirsiniz. Bu öğretici bir runbook üzerinden Güncelleştirme Yönetimi’ni etkinleştirme konusunda size yol gösterir. Uygun ölçekte Azure Sanal Makinelerin eklenmesi için mevcut bir VM’ye Değişiklik izleme veya Güncelleştirme yönetimi çözümünün eklenmesi gerekir. Bu adımda, bir sanal makineye Güncelleştirme yönetimi ve Değişiklik izleme özelliğini eklersiniz.

### <a name="enable-change-tracking-and-inventory"></a>Değişiklik İzlemeyi ve Sayımı Etkinleştirme

İzlemeyi Değiştir ve Envanter çözümleri, sanal makinelerinizdeki değişiklikleri ve [envanteri](automation-vm-inventory.md) [izlemenize](automation-vm-change-tracking.md) olanak sağlar. Bu adımda, sanal bir makinede çözümleri etkinleştirin.

1. Azure portalında **Otomasyon Hesapları'nı**seçin ve ardından listedeki otomasyon hesabınızı seçin.
1. **Yapılandırma Yönetimi**altında **Envanter'i** seçin.
1. Varolan bir Log Analytics çalışma alanını seçin veya yeni bir çalışma oluşturun. 
1. **Etkinleştir'i**tıklatın.

    ![Güncelleştirme çözümü ekleme](media/automation-onboard-solutions/inventory-onboard.png)

### <a name="enable-update-management"></a>Güncelleştirme Yönetimi’ni etkinleştirme

Güncelleştirme Yönetimi çözümü, Azure Windows VM’leriniz için güncelleştirmeleri ve yamaları yönetmenizi sağlar. Kullanılabilir güncelleştirmelerin durumunu değerlendirebilir, gerekli güncelleştirmelerin yüklemesini zamanlayabilir ve güncelleştirmelerin VM’ye başarıyla uygulandığını doğrulamak için dağıtım sonuçlarını gözden geçirebilirsiniz. Bu adımda çözümü VM’niz için etkinleştirirsiniz.

1. Otomasyon **hesabınızda, Güncelleme Yönetimi** bölümünde Yönetim **Güncelle'yi** seçin.
1. Log analitik çalışma alanı seçilen önceki adımda kullanılan çalışma alanıdır. Güncelleştirme yönetimi çözümünü eklemek için **Etkinleştir**’e tıklayın. Güncelleştirme yönetimi çözümü yüklenirken mavi renkli bir başlık gösterilir. 

    ![Güncelleştirme çözümü ekleme](media/automation-onboard-solutions/update-onboard.png)

### <a name="select-azure-vm-to-be-managed"></a>Yönetilecek Azure VM’yi seçme

Çözümler etkinleştirildiğine göre artık bu çözümleri eklemek için bir Azure VM ekleyebilirsiniz.

1. Otomasyon **hesabınızdan, Yapılandırma Yönetimi**altında Izlemeyi **Değiştir'i** seçin. 
2. İzlemeyi Değiştir sayfasında, VM'nizi eklemek için **Azure VM'leri ekle'yi** tıklatın.

3. Listeden VM'nizi seçin ve **Etkinleştir'i**tıklatın. Bu eylem, VM için Değişiklik İzleme ve Stok çözümlerini sağlar.

   ![VM için güncelleştirme çözümünü etkinleştirme](media/automation-onboard-solutions/enable-change-tracking.png)

4. VM onboarding bildirimi tamamlandığında, **Güncelleştirme Yönetimi**altında **Güncelleştirme yönetimini** seçin.

5. VM'nizi eklemek için **Azure VM** ekle'yi seçin.

6. Listeden VM’nizi seçin ve **Etkinleştir** seçeneğini belirleyin. Bu eylem, VM için Güncelleştirme Yönetimi çözümsağlar.

   ![VM için güncelleştirme çözümünü etkinleştirme](media/automation-onboard-solutions/enable-update.png)

> [!NOTE]
> Diğer çözümün tamamlanmasını beklemezseniz, sonraki çözümü etkinleştirirken iletiyi alırsınız:`Installation of another solution is in progress on this or a different virtual machine. When that installation completes the Enable button is enabled, and you can request installation of the solution on this virtual machine.`

## <a name="install-and-update-modules"></a>Modülleri yükleme ve güncelleştirme

En son Azure modüllerine güncelleştirmek ve onboarding çözümlerini başarıyla otomatikleştirmek için [Az.OperationalInsights](https://docs.microsoft.com/powershell/module/az.operationalinsights/?view=azps-3.7.0) modüllerini almak gerekir.

## <a name="update-azure-modules"></a>Azure Modüllerini etkinleştirme

1. Otomasyon hesabınızda, **Paylaşılan Kaynaklar**altında **Modülleri** seçin. 
2. Azure modüllerini en son sürüme güncelleştirmek için **Azure Modüllerini Güncelleştir** seçeneğini belirleyin. 
3. Varolan tüm Azure modüllerini en son sürüme güncellemek için **Evet'i** tıklatın.

![Modülleri](media/automation-onboard-solutions/update-modules.png) güncelleştir A

### <a name="install-azoperationalinsights-module"></a>Az.OperationalInsights modüllerini yükleyin

1. Otomasyon hesabında, **Paylaşılan Kaynaklar**altında **Modülleri** seçin. 
2. Modül galerisini açmak için **Gözat galerisini** seçin. 
3. Az.OperationalInsights'ı arayın ve bu modülü Otomasyon hesabına aktarın.

![OperationalInsights modülünü içeri aktarma](media/automation-onboard-solutions/import-operational-insights-module.png)

## <a name="import-the-onboarding-runbook"></a>Ekleme runbook’unu içeri aktarma

1. Otomasyon hesabınızda, **Proses Otomasyonu**altında **Runbook'ları** seçin.
1. **Galeriye gözat** seçeneğini belirleyin.
1. `update and change tracking` arayın.
3. Runbook'u seçin ve Kaynak Görüntüley sayfasında **İçe Aktar'ı** tıklatın. 
4. Runbook'u Otomasyon hesabına almak için **Tamam'ı** tıklatın.

   ![Ekleme runbook’unu içeri aktarma](media/automation-onboard-solutions/import-from-gallery.png)

6. Runbook **sayfasında, Edit'i**tıklatın ve ardından **Yayımla'yı**seçin. 
7. Yayımlama RunBook bölmesine, runbook'u yayımlamak için **Evet'i** tıklatın.

## <a name="start-the-runbook"></a>Runbook’u başlatma

Bu runbook'u başlatmak için izlemeyi değiştirme veya azure VM çözümlerini güncelleştirme konusunda yerleşik olmalısınız. Parametreler için çözümün eklenmiş olduğu mevcut bir sanal makine ve kaynak grubu gereklidir.

1. **Etkinleştir-ÇokÇözüm** çalışma kitabını açın.

   ![Çoklu çözüm runbook’ları](media/automation-onboard-solutions/runbook-overview.png)

1. Başlat düğmesine tıklayın ve parametreler için aşağıdaki değerleri girin.

   * **VMNAME** - Boş bırakın. Güncelleştirme veya değişiklik izleme çözümünün ekleneceği mevcut bir VM’nin adıdır. Bu değeri bış bıraktığınızda kaynak grubundaki tüm VM’lere ekleme yapılır.
   * **VMRESOURCEGROUP** - Ekleme yapılacak VM’lere ilişkin kaynak grubunun adıdır.
   * **SUBSCRIPTIONID** - Boş bırakın. Ekleme yapılacak yeni VM’nin abonelik kimliğidir. Boş bırakılırsa çalışma alanının aboneliği kullanılır. Farklı bir abonelik kimliği belirtildiğinde, bu otomasyon hesabına yönelik RunAs hesabının da söz konusu abonelik için katkıda bulunan olarak eklenmesi gerekir.
   * **ALREADYONBOARDEDVM** - Güncelleştirme veya Değişiklik İzleme çözümünün el ile eklendiği VM’nin adıdır.
   * **ALREADYONBOARDEDVMRESOURCEGROUP** - VM'nin ait olduğu kaynak grubunun adı.
   * **SOLUTIONTYPE** - **Güncelleştirmeleri** girin veya **ChangeTracking**.

   ![Enable-MultipleSolution runbook parametreleri](media/automation-onboard-solutions/runbook-parameters.png)

1. Runbook işini başlatmak için **Tamam**’ı seçin.
1. Runbook işi sayfasında ilerlemeyi ve hataları izleyin.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Güncelleştirme Yönetimi'nden bir VM kaldırmak için:

1. Günlük Analizi çalışma alanınızda, kapsam yapılandırması `MicrosoftDefaultScopeConfig-Updates`için kaydedilen aramadan VM'yi kaldırın. Kaydedilen aramalar çalışma alanınızda **Genel** altında bulunabilir.
2. Windows [için Log Analytics aracısını](../azure-monitor/learn/quick-collect-windows-computer.md#clean-up-resources) veya [Linux için Log Analytics aracısını](../azure-monitor/learn/quick-collect-linux-computer.md#clean-up-resources)kaldırın.

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
