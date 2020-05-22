---
title: Otomasyon hesabından Azure Otomasyonu Güncelleştirme Yönetimi etkinleştirme
description: Bu makalede bir Otomasyon hesabından Güncelleştirme Yönetimi nasıl etkinleştirileceği açıklanır.
services: automation
ms.date: 4/11/2019
ms.topic: conceptual
ms.custom: mvc
ms.openlocfilehash: a521ff690f59b6beafd1113b177b43193dc7447e
ms.sourcegitcommit: 493b27fbfd7917c3823a1e4c313d07331d1b732f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/21/2020
ms.locfileid: "83744001"
---
# <a name="enable-update-management-from-an-automation-account"></a>Otomasyon hesabından Güncelleştirme Yönetimi etkinleştirme

Bu makalede, ortamınızdaki VM 'Ler için [güncelleştirme yönetimi](automation-update-management.md) özelliğini etkinleştirmek üzere otomasyon hesabınızı nasıl kullanabileceğiniz açıklanır. Azure VM 'lerini ölçekli olarak etkinleştirmek için, Güncelleştirme Yönetimi kullanarak var olan bir VM 'yi etkinleştirmeniz gerekir. 

> [!NOTE]
> Güncelleştirme Yönetimi etkinleştirilirken, bir Log Analytics çalışma alanını ve bir Otomasyon hesabını bağlamak için yalnızca belirli bölgeler desteklenir. Desteklenen eşleme çiftlerinin bir listesi için bkz. [Otomasyon hesabı ve Log Analytics çalışma alanı Için bölge eşleme](how-to/region-mappings.md).

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği. Henüz bir hesabınız yoksa [MSDN abone avantajlarınızı etkinleştirebilir](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) veya [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)için kaydolabilirsiniz.
* Makineleri yönetmek için [Otomasyon hesabı](automation-offering-get-started.md).
* Bir [sanal makine](../virtual-machines/windows/quick-create-portal.md).

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

Adresinden Azure 'da oturum açın https://portal.azure.com .

## <a name="enable-update-management"></a>Güncelleştirme Yönetimi’ni etkinleştirme

1. Otomasyon **hesabınızda güncelleştirme yönetimi altında** **güncelleştirme yönetimi** ' ni seçin.

2. Log Analytics çalışma alanını ve otomasyon hesabını seçin ve Güncelleştirme Yönetimi etkinleştirmek için **Etkinleştir** ' e tıklayın. Kurulumun tamamlanabilmesi 15 dakika sürer.

    ![Güncelleştirme Yönetimi’ni etkinleştirme](media/automation-onboard-solutions-from-automation-account/onboardsolutions2.png)

## <a name="check-the-scope-configuration"></a><a name="scope-configuration"></a>Kapsam yapılandırmasını denetleme

Güncelleştirme Yönetimi, özelliği etkinleştirmek üzere bilgisayarları hedeflemek için çalışma alanındaki kapsam yapılandırmasını kullanır. Kapsam yapılandırması, özelliğin kapsamını belirli bilgisayarlarla sınırlandırmak için kullanılan bir veya daha fazla kayıtlı arama grubudur. Daha fazla bilgi için bkz. [güncelleştirme yönetimi için kapsam yapılandırmalarına sahip çalışma](automation-scope-configurations-update-management.md).

## <a name="enable-azure-vms"></a>Azure VM 'lerini etkinleştirin

1. Otomasyon **hesabınızdan güncelleştirme yönetimi**altında **güncelleştirme yönetimi** ' ni seçin.

2. **+ Azure VM 'Leri Ekle** ' ye tıklayın ve listeden bir veya daha fazla VM seçin. Etkin olmayan sanal makineler gri, seçilemez ve seçilemiyor. Azure VM 'Ler, Otomasyon hesabınızın konumu ne olduğuna bakılmaksızın herhangi bir bölgede bulunabilir. 

3. Seçilen VM 'Leri, özellik için kayıtlı bilgisayar grubu aramasına eklemek için **Etkinleştir** ' e tıklayın.

    ![Azure VM 'lerini etkinleştirin](media/automation-onboard-solutions-from-automation-account/enable-azure-vms.png)

## <a name="enable-non-azure-vms"></a>Azure dışı VM 'Leri etkinleştirme

Azure 'da bulunmayan makinelerin el ile eklenmesi gerekir. 

1. Otomasyon **hesabınızdan güncelleştirme yönetimi altında** **güncelleştirme yönetimi** ' ni seçin.

2. **Azure dışı makine Ekle**' ye tıklayın. Bu eylem, makinenin Güncelleştirme Yönetimi işlemleri raporlamaya başlayabilmesi [Için Windows Log Analytics aracısını yükleyip yapılandırmaya yönelik yönergeler](../azure-monitor/platform/log-analytics-agent.md) içeren yeni bir tarayıcı penceresi açar. Şu anda Operations Manager tarafından yönetilen bir makine etkinleştiriyorsanız, yeni bir aracı gerekli değildir ve çalışma alanı bilgileri mevcut aracıya girilir.

## <a name="enable-machines-in-the-workspace"></a>Çalışma alanındaki makineleri etkinleştir

Güncelleştirme Yönetimi etkinleştirilmesi için, çalışma alanınıza zaten rapor veren el ile yüklenen makineler veya makineler Azure Automation 'a eklenmelidir. 

1. Otomasyon **hesabınızdan güncelleştirme yönetimi altında** **güncelleştirme yönetimi** ' ni seçin.

2. **Makineleri Yönet**' i seçin. Daha önce **tüm mevcut ve gelecekteki makinelerde etkinleştir** seçeneğini belirlediyseniz **makineleri Yönet** düğmesi gri olabilir

    ![Kayıtlı aramalar](media/automation-onboard-solutions-from-automation-account/managemachines.png)

4. Tüm kullanılabilir makineler için Güncelleştirme Yönetimi etkinleştirmek üzere makineleri Yönet sayfasında **kullanılabilir tüm makinelerde etkinleştir** ' i seçin. Bu eylem, tek tek makineleri eklemek için denetimi devre dışı bırakır. Bu görev, raporlayan makinelerin tüm adlarını çalışma alanına kayıtlı bilgisayar grubu arama sorgusuna ekler. Seçildiğinde, bu eylem **makineleri Yönet** düğmesini devre dışı bırakır.

5. Tüm kullanılabilir makineler ve gelecekteki makineler için özelliği etkinleştirmek üzere **tüm kullanılabilir ve gelecekteki makinelerde etkinleştir**' i seçin. Bu seçenek, kaydedilmiş aramaları ve kapsam yapılandırmasını çalışma alanından siler ve çalışma alanına raporlama yapan tüm Azure ve Azure dışı makineler için özelliği açar. Seçildiğinde, bu eylem, hiçbir kapsam yapılandırması bulunmadığından **makineleri Yönet** düğmesini kalıcı olarak devre dışı bırakır.

6. Gerekirse, ilk kaydedilmiş aramaları yeniden ekleyerek kapsam yapılandırma işlemleri geri eklenebilir. Daha fazla bilgi için bkz. [güncelleştirme yönetimi için kapsam yapılandırmalarına sahip çalışma](automation-scope-configurations-update-management.md).

7. Bir veya daha fazla makine için özelliği etkinleştirmek üzere **Seçili makinelerde etkinleştir** ' i seçin ve özelliği etkinleştirmek için her makinenin yanındaki **Ekle** ' ye tıklayın. Bu görev, seçilen makine adlarını, bu özellik için bilgisayar grubu kayıtlı arama sorgusuna ekler.

## <a name="next-steps"></a>Sonraki adımlar

* VM 'Ler için Güncelleştirme Yönetimi kullanmak için bkz. [Azure VM 'leriniz için güncelleştirmeleri ve düzeltme eklerini yönetme](automation-tutorial-update-management.md).
* Kapsam yapılandırmalarında bkz. [güncelleştirme yönetimi için kapsam yapılandırmalarına sahip çalışma](automation-scope-configurations-update-management.md).
* Artık Log Analytics çalışma alanına ihtiyacınız yoksa, [güncelleştirme yönetimi için çalışma alanının Otomasyon hesabından bağlantısını kaldır](automation-unlink-workspace-update-management.md)' daki yönergelere bakın.
* VM 'Leri Güncelleştirme Yönetimi silmek için bkz. [güncelleştirme yönetimi VM 'Leri kaldırma](automation-remove-vms-from-update-management.md).
* Genel Güncelleştirme Yönetimi hatalarıyla ilgili sorunları gidermek için bkz. [güncelleştirme yönetimi sorunlarını giderme](troubleshoot/update-management.md).
* Windows Update Aracısı sorunlarını gidermek için bkz. [Windows Update Aracısı sorunlarını giderme](troubleshoot/update-agent-issues.md).
* Linux Güncelleştirme Aracısı sorunlarını gidermek için bkz.[Linux Güncelleştirme Aracısı sorunlarını giderme](troubleshoot/update-agent-issues-linux.md).
