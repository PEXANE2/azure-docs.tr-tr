---
title: Bir runbook 'tan Azure Otomasyonu Değişiklik İzleme ve envanterini etkinleştirme
description: Bu makalede, bir runbook 'tan Değişiklik İzleme ve envanterin nasıl etkinleştirileceği açıklanır.
services: automation
ms.topic: conceptual
ms.date: 05/10/2018
ms.custom: mvc
ms.openlocfilehash: 3220c364a4c94f95fa26d334a95f8e0d3155d5ab
ms.sourcegitcommit: 6a9f01bbef4b442d474747773b2ae6ce7c428c1f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/27/2020
ms.locfileid: "84117987"
---
# <a name="enable-change-tracking-and-inventory-from-a-runbook"></a>Runbook’tan Değişiklik İzleme ve Stok özelliğini etkinleştirme

Bu makalede, ortamınızdaki VM 'Ler için [değişiklik izleme ve envanter](change-tracking.md) özelliğini etkinleştirmek üzere bir runbook 'u nasıl kullanabileceğiniz açıklanır. Azure VM 'lerini ölçekli olarak etkinleştirmek için, Değişiklik İzleme ve envanterini kullanarak var olan bir VM 'yi etkinleştirmeniz gerekir. 

> [!NOTE]
> Değişiklik İzleme ve envanteri etkinleştirirken, bir Log Analytics çalışma alanını ve bir Otomasyon hesabını bağlamak için yalnızca belirli bölgeler desteklenir. Desteklenen eşleme çiftlerinin bir listesi için bkz. [Otomasyon hesabı ve Log Analytics çalışma alanı Için bölge eşleme](how-to/region-mappings.md).

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği. Henüz bir hesabınız yoksa [MSDN abone avantajlarınızı etkinleştirebilir](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) veya [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)için kaydolabilirsiniz.
* Makineleri yönetmek için [Otomasyon hesabı](automation-offering-get-started.md).
* Bir [sanal makine](../virtual-machines/windows/quick-create-portal.md).

## <a name="enable-change-tracking-and-inventory"></a>Değişiklik İzlemeyi ve Sayımı Etkinleştirme 

1. Azure portal **Otomasyon hesapları**' nı seçin ve ardından listeden Otomasyon hesabınızı seçin.
1. **Yapılandırma yönetimi**altında **Stok** ' ı seçin.
1. Mevcut bir Log Analytics çalışma alanını seçin veya yenisini oluşturun. 
1. **Etkinleştir**' e tıklayın.

    ![Değişiklik İzlemeyi ve Sayımı Etkinleştirme](media/automation-enable-changes-from-runbook/inventory-onboard.png)

## <a name="select-azure-vm-to-manage"></a>Yönetmek için Azure VM 'yi seçin

Değişiklik İzleme ve envanter etkinleştirildiğinde, özelliği tarafından yönetim için bir Azure VM ekleyebilirsiniz.

1. Otomasyon hesabınızdan, **yapılandırma yönetimi**altında **değişiklik izleme** veya **Stok** ' ı seçin.

2. VM 'nizi eklemek için **Azure VM 'Leri Ekle** ' ye tıklayın.

3. Listeden VM 'nizi seçin ve **Etkinleştir**' e tıklayın. Bu eylem VM 'nin Değişiklik İzleme ve envanterini sunar.

   ![VM Değişiklik İzleme ve envanterini etkinleştirme](media/automation-enable-changes-from-runbook/enable-change-tracking.png)

    > [!NOTE]
    > Değişiklik İzleme ve envanter kurulumu tamamlanmadan önce başka bir özelliği etkinleştirmeye çalışırsanız şu iletiyi alırsınız:`Installation of another solution is in progress on this or a different virtual machine. When that installation completes the Enable button is enabled, and you can request installation of the solution on this virtual machine.`

## <a name="install-and-update-modules"></a>Modülleri yükleme ve güncelleştirme

En son Azure modüllerine güncelleştirmeniz gerekir ve [az. Operationalınsights](https://docs.microsoft.com/powershell/module/az.operationalinsights/?view=azps-3.7.0) modülünü IÇERI aktararak sanal makinenizin değişiklik izleme ve envanterini başarıyla etkinleştirin.

1. Otomasyon hesabınızda, **paylaşılan kaynaklar**altında **modüller** ' i seçin. 
2. Azure modüllerini en son sürüme güncelleştirmek için **Azure Modüllerini Güncelleştir** seçeneğini belirleyin. 
3. Var olan tüm Azure modüllerini en son sürüme güncelleştirmek için **Evet** ' e tıklayın.

    ![Modülleri güncelleştirme](media/automation-enable-changes-from-runbook/update-modules.png)

4. **Paylaşılan kaynaklar**altındaki **modüllere** geri dönün. 
5. Modül galerisini açmak için **tarayıcı Galerisi** ' ni seçin. 
6. Az. Operationalınsights araması yapın ve bu modülü Otomasyon hesabına aktarın.

    ![OperationalInsights modülünü içeri aktarma](media/automation-enable-changes-from-runbook/import-operational-insights-module.png)

## <a name="import-a-runbook-to-enable-change-tracking-and-inventory"></a>Değişiklik İzleme ve envanteri etkinleştirmek için Runbook 'u içeri aktarma

1. Otomasyon hesabınızda, **Işlem Otomasyonu**altında **runbook 'lar** ' ı seçin.
2. **Galeriye gözat** seçeneğini belirleyin.
3. `update and change tracking` arayın.
4. Runbook 'u seçin ve kaynak görüntüleme sayfasında **Içeri aktar** ' a tıklayın. 
5. Runbook 'u Otomasyon hesabına aktarmak için **Tamam** ' ı tıklatın.

   ![Kurulum için Runbook 'u içeri aktar](media/automation-enable-changes-from-runbook/import-from-gallery.png)

6. Runbook sayfasında, **Düzenle**' ye tıklayın ve ardından **Yayımla**' yı seçin. 
7. Runbook 'U Yayımla bölmesinde, runbook 'u yayımlamak için **Evet** ' i tıklatın.

## <a name="start-the-runbook"></a>Runbook’u başlatma

Bu runbook 'u başlatmak için bir Azure VM için Değişiklik İzleme ve envanteri etkinleştirmiş olmanız gerekir. Parametrelere yönelik etkinleştirilmiş özelliği olan mevcut bir VM ve kaynak grubu gerektirir.

1. **Enable-MultipleSolution** runbook 'unu açın.

   ![Çoklu çözüm runbook’ları](media/automation-enable-changes-from-runbook/runbook-overview.png)

1. Başlat düğmesine tıklayın ve parametre değerlerini aşağıdaki alanlara girin:

   * **VMName** -değişiklik izleme ve stoğa eklenecek mevcut bir sanal makinenin adı. Kaynak grubundaki tüm VM 'Leri eklemek için bu alanı boş bırakın.
   * **Vmresourcegroup** -etkinleştirilecek VM 'ler için kaynak grubunun adı.
   * **SubscriptionID** -etkinleştirilecek yeni sanal makınenın abonelik kimliği. Çalışma alanının aboneliğini kullanmak için bu alanı boş bırakın. Farklı bir abonelik KIMLIĞI kullandığınızda, Otomasyon hesabınızın farklı çalıştır hesabını abonelik için katkıda bulunan olarak ekleyin.
   * **Alreadyonboardedvd** -zaten değişiklikler için el Ile etkinleştirilen VM 'nin adı.
   * **Alreadyonboardedvmresourcegroup** -VM 'nin ait olduğu kaynak grubunun adı.
   * **Solutiontype** - **ChangeTracking**girin.

   ![Enable-MultipleSolution runbook parametreleri](media/automation-enable-changes-from-runbook/runbook-parameters.png)

1. Runbook işini başlatmak için **Tamam**’ı seçin.
1. Runbook işi sayfasında ilerlemeyi ve hataları izleyin.

## <a name="next-steps"></a>Sonraki adımlar

* Bir runbook 'u zamanlamak için bkz. [Azure Otomasyonu 'nda zamanlamaları yönetme](shared-resources/schedules.md).
* Özelliği ile çalışma hakkında ayrıntılı bilgi için bkz. [Manage değişiklik izleme and Inventory](change-tracking-file-contents.md).
* Ortamınızdaki yüklü yazılımları belirlemek için özelliğini kullanmayı öğrenmek için bkz. [sanal makinelerinize hangi yazılımın yüklendiğini keşfetme](automation-tutorial-installed-software.md).
* Özelliği etkinleştirirken Otomasyon hesabınızı bir Log Analytics çalışma alanıyla tümleştirmek istemiyorsanız, bkz. [çalışma alanının Otomasyon hesabından bağlantısını](automation-unlink-workspace-change-tracking.md)kaldırma.
* VM 'lere yapılan değişiklikleri dağıtma işlemi tamamlandığında, [değişiklik izleme ve envanterden VM 'Leri kaldırma](automation-remove-vms-from-change-tracking.md)bölümünde açıklandığı gibi bunları kaldırabilirsiniz.
* Özellikle ilgili sorunları gidermek için bkz. [değişiklik izleme ve envanter sorunlarını giderme](troubleshoot/change-tracking.md).