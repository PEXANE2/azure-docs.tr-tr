---
title: Runbook 'tan Azure Otomasyonu Güncelleştirme Yönetimi etkinleştirme
description: Bu makalede, runbook 'tan Güncelleştirme Yönetimi nasıl etkinleştirileceği açıklanır.
services: automation
ms.topic: conceptual
ms.date: 07/28/2020
ms.custom: mvc
ms.openlocfilehash: a5b6fe5cf83eaa18b34a00767e14e75737aa055e
ms.sourcegitcommit: cee72954f4467096b01ba287d30074751bcb7ff4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/30/2020
ms.locfileid: "87450883"
---
# <a name="enable-update-management-from-a-runbook"></a>Runbook’tan Güncelleştirme Yönetimi’ni etkinleştirme

Bu makalede, ortamınızdaki VM 'Ler için [güncelleştirme yönetimi](update-mgmt-overview.md) özelliğini etkinleştirmek üzere bir runbook 'u nasıl kullanabileceğiniz açıklanır. Azure VM 'lerini ölçekli olarak etkinleştirmek için, Güncelleştirme Yönetimi kullanarak var olan bir VM 'yi etkinleştirmeniz gerekir. 

> [!NOTE]
> Güncelleştirme Yönetimi etkinleştirilirken, bir Log Analytics çalışma alanını ve bir Otomasyon hesabını bağlamak için yalnızca belirli bölgeler desteklenir. Desteklenen eşleme çiftlerinin bir listesi için bkz. [Otomasyon hesabı ve Log Analytics çalışma alanı Için bölge eşleme](../how-to/region-mappings.md).

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği. Henüz bir hesabınız yoksa [MSDN abone avantajlarınızı etkinleştirebilir](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) veya [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)için kaydolabilirsiniz.
* Makineleri yönetmek için [Otomasyon hesabı](../index.yml).
* Bir [sanal makine](../../virtual-machines/windows/quick-create-portal.md).

## <a name="sign-in-to-azure"></a>Azure’da oturum açma

[Azure portalında](https://portal.azure.com) oturum açın.

## <a name="enable-update-management"></a>Güncelleştirme Yönetimi’ni etkinleştirme

1. Otomasyon hesabınızda **güncelleştirme yönetimi**altında **güncelleştirme yönetimi** seçin.

2. Log Analytics çalışma alanını seçin ve **Etkinleştir**' e tıklayın. Güncelleştirme Yönetimi etkinleştirildiğinde mavi bir başlık gösterilir.

    ![Güncelleştirme Yönetimi’ni etkinleştirme](media/update-mgmt-enable-runbook/update-onboard.png)

## <a name="select-azure-vm-to-manage"></a>Yönetmek için Azure VM 'yi seçin

Güncelleştirme Yönetimi etkinken, güncelleştirmeleri almak için bir Azure VM ekleyebilirsiniz.

1. Otomasyon **hesabınızdan güncelleştirme yönetimi altında** **güncelleştirme yönetimi** ' ni seçin.

2. VM 'nizi eklemek için **Azure VM 'Leri Ekle** ' yi seçin.

3. Listeden VM 'yi seçin ve güncelleştirme için VM 'yi ayarlamak üzere **Etkinleştir** ' e tıklayın.

   ![VM için Güncelleştirme Yönetimi etkinleştirme](media/update-mgmt-enable-runbook/enable-update.png)

    > [!NOTE]
    > Güncelleştirme Yönetimi Kurulumu tamamlanmadan önce başka bir özelliği etkinleştirmeye çalışırsanız şu iletiyi alırsınız:`Installation of another solution is in progress on this or a different virtual machine. When that installation completes the Enable button is enabled, and you can request installation of the solution on this virtual machine.`

## <a name="install-and-update-modules"></a>Modülleri yükleme ve güncelleştirme

En son Azure modüllerine güncelleştirmek ve [az. Operationalınsights](/powershell/module/az.operationalinsights/?view=azps-3.7.0) modülünü Içeri aktararak sanal makinelerinize yönelik güncelleştirme yönetimi başarıyla etkinleştirmeniz gerekir.

1. Otomasyon hesabınızda, **paylaşılan kaynaklar**altında **modüller** ' i seçin.
2. Azure modüllerini en son sürüme güncelleştirmek için **Azure Modüllerini Güncelleştir** seçeneğini belirleyin.
3. Var olan tüm Azure modüllerini en son sürüme güncelleştirmek için **Evet** ' e tıklayın.

    ![Modülleri güncelleştirme](media/update-mgmt-enable-runbook/update-modules.png)

4. **Paylaşılan kaynaklar**altındaki **modüllere** geri dönün.
5. Modül galerisini açmak için **tarayıcı galerisine gidin** ' i seçin.
6. `Az.OperationalInsights`Bu modülü arayıp Otomasyon hesabınızda içeri aktarın.

    ![OperationalInsights modülünü içeri aktarma](media/update-mgmt-enable-runbook/import-operational-insights-module.png)

## <a name="import-a-runbook-to-enable-update-management"></a>Güncelleştirme Yönetimi etkinleştirmek için Runbook 'u içeri aktarma

1. Otomasyon hesabınızda, **Işlem Otomasyonu**altında **runbook 'lar** ' ı seçin.
2. **Galeriye gözat** seçeneğini belirleyin.
3. `update and change tracking` arayın.
4. Runbook 'u seçin ve kaynak görüntüleme sayfasında **Içeri aktar** ' a tıklayın.
5. Runbook 'u Otomasyon hesabına aktarmak için **Tamam** ' ı tıklatın.

   ![Kurulum için Runbook 'u içeri aktar](media/update-mgmt-enable-runbook/import-from-gallery.png)

6. Runbook sayfasında, **Düzenle**' ye tıklayın ve ardından **Yayımla**' yı seçin.
7. Runbook 'U Yayımla bölmesinde, runbook 'u yayımlamak için **Evet** ' i tıklatın.

## <a name="start-the-runbook"></a>Runbook’u başlatma

Bu runbook 'u başlatmak için bir Azure VM için Güncelleştirme Yönetimi etkinleştirmiş olmanız gerekir. Parametrelere yönelik etkinleştirilmiş özelliği olan mevcut bir VM ve kaynak grubu gerektirir.

1. **Enable-MultipleSolution** runbook 'unu açın.

   ![Birden çok çözüm runbook 'u](media/update-mgmt-enable-runbook/runbook-overview.png)

2. Başlat düğmesine tıklayın ve parametre değerlerini aşağıdaki alanlara girin:

   * **VMName** -güncelleştirme yönetimi eklenecek mevcut bir sanal makinenin adı. Kaynak grubundaki tüm VM 'Leri eklemek için bu alanı boş bırakın.
   * **Vmresourcegroup** -etkinleştirilecek VM 'ler için kaynak grubunun adı.
   * **SubscriptionID** -etkinleştirilecek yeni sanal makınenın abonelik kimliği. Çalışma alanının aboneliğini kullanmak için bu alanı boş bırakın. Farklı bir abonelik KIMLIĞI kullandığınızda, Otomasyon hesabınızın farklı çalıştır hesabını abonelik için katkıda bulunan olarak ekleyin.
   * **Alreadyonboardedvd** -güncelleştirmeler için zaten etkinleştirilmiş olan sanal makinenin adı.
   * **Alreadyonboardedvmresourcegroup** -VM 'nin ait olduğu kaynak grubunun adı.
   * **Solutiontype** - **güncelleştirmeleri**girin.

   ![Enable-MultipleSolution runbook parametreleri](media/update-mgmt-enable-runbook/runbook-parameters.png)

3. Runbook işini başlatmak için **Tamam**’ı seçin.

4. Runbook işinin ilerlemesini ve **işler** sayfasındaki hataları izleyin.

## <a name="next-steps"></a>Sonraki adımlar

* VM 'Ler için Güncelleştirme Yönetimi kullanmak için bkz. [VM 'niz için güncelleştirmeleri ve düzeltme eklerini yönetme](update-mgmt-manage-updates-for-vm.md).

* Genel Güncelleştirme Yönetimi hatalarıyla ilgili sorunları gidermek için bkz. [güncelleştirme yönetimi sorunlarını giderme](../troubleshoot/update-management.md).
