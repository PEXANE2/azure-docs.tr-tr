---
title: Otomasyon hesabından Azure Otomasyonu Güncelleştirme Yönetimi etkinleştirme
description: Bu makalede bir Otomasyon hesabından Güncelleştirme Yönetimi nasıl etkinleştirileceği açıklanır.
services: automation
ms.date: 07/28/2020
ms.topic: conceptual
ms.custom: mvc
ms.openlocfilehash: 930861c61843c5963c83d8fa6dc1efdce20853f4
ms.sourcegitcommit: cee72954f4467096b01ba287d30074751bcb7ff4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/30/2020
ms.locfileid: "87450881"
---
# <a name="enable-update-management-from-an-automation-account"></a>Otomasyon hesabından Güncelleştirme Yönetimi’ni etkinleştirme

Bu makalede, ortamınızdaki VM 'Ler için [güncelleştirme yönetimi](update-mgmt-overview.md) özelliğini etkinleştirmek üzere otomasyon hesabınızı nasıl kullanabileceğiniz açıklanır. Azure VM 'lerini ölçekli olarak etkinleştirmek için, Güncelleştirme Yönetimi kullanarak var olan bir VM 'yi etkinleştirmeniz gerekir.

> [!NOTE]
> Güncelleştirme Yönetimi etkinleştirilirken, bir Log Analytics çalışma alanını ve bir Otomasyon hesabını bağlamak için yalnızca belirli bölgeler desteklenir. Desteklenen eşleme çiftlerinin bir listesi için bkz. [Otomasyon hesabı ve Log Analytics çalışma alanı Için bölge eşleme](../how-to/region-mappings.md).

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği. Henüz bir hesabınız yoksa [MSDN abone avantajlarınızı etkinleştirebilir](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) veya [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)için kaydolabilirsiniz.
* Makineleri yönetmek için [Otomasyon hesabı](../index.yml).
* Bir [sanal makine](../../virtual-machines/windows/quick-create-portal.md).

## <a name="sign-in-to-azure"></a>Azure’da oturum açma

[Azure portalında](https://portal.azure.com) oturum açın.

## <a name="enable-update-management"></a>Güncelleştirme Yönetimi’ni etkinleştirme

1. Otomasyon **hesabınızda güncelleştirme yönetimi altında** **güncelleştirme yönetimi** ' ni seçin.

2. Log Analytics çalışma alanını ve otomasyon hesabını seçin ve Güncelleştirme Yönetimi etkinleştirmek için **Etkinleştir** ' i seçin. Kurulumun tamamlanabilmesi 15 dakika sürer.

    ![Güncelleştirme Yönetimi’ni etkinleştirme](media/update-mgmt-enable-automation-account/onboardsolutions2.png)

## <a name="enable-azure-vms"></a>Azure VM 'lerini etkinleştirin

1. Otomasyon **hesabınızdan güncelleştirme yönetimi**altında **güncelleştirme yönetimi** ' ni seçin.

2. **+ Azure VM Ekle** ' yi seçin ve listeden bir veya daha fazla VM seçin. Etkin olmayan sanal makineler gri, seçilemez ve seçilemiyor. Azure VM 'Ler, Otomasyon hesabınızın konumu ne olduğuna bakılmaksızın herhangi bir bölgede bulunabilir.

3. Seçilen VM 'Leri, özellik için bilgisayar grubu kayıtlı aramasına eklemek için **Etkinleştir** ' i seçin.

    ![Azure VM 'lerini etkinleştirin](media/update-mgmt-enable-automation-account/enable-azure-vms.png)

## <a name="enable-non-azure-vms"></a>Azure dışı VM 'Leri etkinleştirme

Azure 'da bulunmayan makinelerin el ile eklenmesi gerekir.

1. Otomasyon **hesabınızdan güncelleştirme yönetimi altında** **güncelleştirme yönetimi** ' ni seçin.

2. **Azure dışı makine Ekle**' yi seçin. Bu eylem, makinenin Güncelleştirme Yönetimi raporlamaya başlayabilmesi için [Windows Log Analytics aracısını yükleyip yapılandırmaya yönelik yönergeler](../../azure-monitor/platform/log-analytics-agent.md) içeren yeni bir tarayıcı penceresi açar. Şu anda Operations Manager tarafından yönetilen bir makine etkinleştiriyorsanız, yeni bir aracı gerekli değildir. Çalışma alanı bilgileri aracılar yapılandırmasına eklenir.

## <a name="enable-machines-in-the-workspace"></a>Çalışma alanındaki makineleri etkinleştir

Güncelleştirme Yönetimi etkinleştirilmesi için, çalışma alanınıza zaten rapor veren el ile yüklenen makineler veya makineler Azure Automation 'a eklenmelidir.

1. Otomasyon **hesabınızdan güncelleştirme yönetimi altında** **güncelleştirme yönetimi** ' ni seçin.

2. **Makineleri Yönet**' i seçin. Daha önce **tüm mevcut ve gelecekteki makinelerde etkinleştir** seçeneğini belirlediyseniz **makineleri Yönet** düğmesi gri olabilir

    ![Kayıtlı aramalar](media/update-mgmt-enable-automation-account/managemachines.png)

3. Tüm kullanılabilir makineler için Güncelleştirme Yönetimi etkinleştirmek üzere makineleri Yönet sayfasında **kullanılabilir tüm makinelerde etkinleştir** ' i seçin. Bu eylem, tek tek makineleri eklemek için denetimi devre dışı bırakır. Bu görev, raporlayan makinelerin tüm adlarını çalışma alanına kayıtlı bilgisayar grubu arama sorgusuna ekler. Seçildiğinde, bu eylem **makineleri Yönet** düğmesini devre dışı bırakır.

4. Tüm kullanılabilir makineler ve gelecekteki makineler için özelliği etkinleştirmek üzere **tüm kullanılabilir ve gelecekteki makinelerde etkinleştir**' i seçin. Bu seçenek, kaydedilmiş aramaları ve kapsam yapılandırmasını çalışma alanından siler ve çalışma alanına raporlama yapan tüm Azure ve Azure dışı makineler için özelliği açar. Seçildiğinde, bu eylem, hiçbir kapsam yapılandırması bulunmadığından **makineleri Yönet** düğmesini kalıcı olarak devre dışı bırakır.

5. Gerekirse, ilk kaydedilmiş aramaları yeniden ekleyerek kapsam yapılandırma işlemleri geri eklenebilir. Daha fazla bilgi için bkz. [sınır güncelleştirme yönetimi dağıtım kapsamı](update-mgmt-scope-configuration.md).

6. Bir veya daha fazla makine için özelliği etkinleştirmek üzere **Seçili makinelerde etkinleştir** ' i seçin ve her makinenin yanındaki **Ekle** ' yi seçin. Bu görev, seçilen makine adlarını, bu özellik için bilgisayar grubu kayıtlı arama sorgusuna ekler.

## <a name="next-steps"></a>Sonraki adımlar

* VM 'Ler için Güncelleştirme Yönetimi kullanmak için bkz. [VM 'niz için güncelleştirmeleri ve düzeltme eklerini yönetme](update-mgmt-manage-updates-for-vm.md).

* Genel Güncelleştirme Yönetimi hatalarıyla ilgili sorunları gidermek için bkz. [güncelleştirme yönetimi sorunlarını giderme](../troubleshoot/update-management.md).