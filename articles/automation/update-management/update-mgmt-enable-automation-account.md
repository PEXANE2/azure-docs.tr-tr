---
title: Otomasyon hesabından Azure Otomasyonu Güncelleştirme Yönetimi etkinleştirme
description: Bu makalede bir Otomasyon hesabından Güncelleştirme Yönetimi nasıl etkinleştirileceği açıklanır.
services: automation
ms.date: 09/09/2020
ms.topic: conceptual
ms.custom: mvc
ms.openlocfilehash: 787338be06c2e30aabb6421a42e7cb3aaabf8a2a
ms.sourcegitcommit: 5d7f8c57eaae91f7d9cf1f4da059006521ed4f9f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89669502"
---
# <a name="enable-update-management-from-an-automation-account"></a>Otomasyon hesabından Güncelleştirme Yönetimi’ni etkinleştirme

Bu makalede, [Azure Arc etkin sunucularla](../../azure-arc/servers/overview.md) (Önizleme) kayıtlı makineler veya sunucular dahil olmak üzere ortamınızdaki VM 'ler için [güncelleştirme yönetimi](update-mgmt-overview.md) özelliğini etkinleştirmek üzere otomasyon hesabınızı nasıl kullanabileceğiniz açıklanır. Azure VM 'lerini ölçekli olarak etkinleştirmek için, Güncelleştirme Yönetimi kullanarak mevcut bir Azure VM 'yi etkinleştirmeniz gerekir.

> [!NOTE]
> Güncelleştirme Yönetimi etkinleştirilirken, bir Log Analytics çalışma alanını ve bir Otomasyon hesabını bağlamak için yalnızca belirli bölgeler desteklenir. Desteklenen eşleme çiftlerinin bir listesi için bkz. [Otomasyon hesabı ve Log Analytics çalışma alanı Için bölge eşleme](../how-to/region-mappings.md).

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği. Henüz bir hesabınız yoksa [MSDN abone avantajlarınızı etkinleştirebilir](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) veya [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)için kaydolabilirsiniz.
* Makineleri yönetmek için [Otomasyon hesabı](../index.yml).
* Bir [Azure sanal makinesi](../../virtual-machines/windows/quick-create-portal.md)veya Arc etkin sunucularla (Önizleme) kayıtlı VM veya sunucu. Azure dışı VM 'Ler veya sunucular, Windows veya Linux için [Log Analytics aracısına](../../azure-monitor/platform/log-analytics-agent.md) sahip olmalıdır ve otomasyon hesabına bağlı olan çalışma alanına rapor verebilir güncelleştirme yönetimi ' de etkinleştirilir. Aracı, Azure Arc ile [azure log ANALYTICS VM Uzantısı](../../azure-arc/servers/manage-vm-extensions.md) dağıtarak, Arc etkin sunuculara yüklenebilir.

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

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

3. Çalışma alanına rapor veren tüm kullanılabilir makineler için Güncelleştirme Yönetimi etkinleştirmek üzere makineleri Yönet sayfasında **kullanılabilir tüm makinelerde etkinleştir** ' i seçin. Bu eylem, tek tek makineleri eklemek için denetimi devre dışı bırakır. Bu görev, raporlayan makinelerin tüm adlarını çalışma alanına kayıtlı bilgisayar grubu arama sorgusuna ekler `MicrosoftDefaultComputerGroup` . Seçildiğinde, bu eylem **makineleri Yönet** düğmesini devre dışı bırakır.

4. Tüm kullanılabilir makineler ve gelecekteki makineler için özelliği etkinleştirmek üzere **tüm kullanılabilir ve gelecekteki makinelerde etkinleştir**' i seçin. Bu seçenek, kaydedilen arama ve kapsam yapılandırmasını çalışma alanından siler ve özelliğin, şu anda veya gelecekte olan tüm Azure dışı makineleri, çalışma alanına rapor olarak içermesini sağlar. Seçildiğinde, bu eylem, kullanılabilir kapsam yapılandırması olmadığından **makineleri Yönet** düğmesini kalıcı olarak devre dışı bırakır.

    > [!NOTE]
    > Bu seçenek Log Analytics içindeki kayıtlı aramaları ve kapsam yapılandırmasını sildiği için, bu seçeneği seçmeden önce Log Analytics çalışma alanındaki tüm silme kilitlerini kaldırmak önemlidir. Bunu yapmazsanız, bu seçenek yapılandırmaların kaldırılmasına neden olur ve bunları el ile kaldırmanız gerekir.

5. Gerekirse, ilk kaydedilmiş arama sorgusunu yeniden ekleyerek kapsam yapılandırmalarının geri eklenmesini sağlayabilirsiniz. Daha fazla bilgi için bkz. [sınır güncelleştirme yönetimi dağıtım kapsamı](update-mgmt-scope-configuration.md).

6. Bir veya daha fazla makine için özelliği etkinleştirmek üzere **Seçili makinelerde etkinleştir** ' i seçin ve her makinenin yanındaki **Ekle** ' yi seçin. Bu görev, seçilen makine adlarını, bu özellik için bilgisayar grubu kayıtlı arama sorgusuna ekler.

## <a name="next-steps"></a>Sonraki adımlar

* VM 'Ler için Güncelleştirme Yönetimi kullanmak için bkz. [VM 'niz için güncelleştirmeleri ve düzeltme eklerini yönetme](update-mgmt-manage-updates-for-vm.md).

* Artık Güncelleştirme Yönetimi olan VM 'Leri veya sunucuları yönetmeniz gerekmiyorsa bkz. [güncelleştirme yönetimi VM 'leri kaldırma](update-mgmt-remove-vms.md).

* Genel Güncelleştirme Yönetimi hatalarıyla ilgili sorunları gidermek için bkz. [güncelleştirme yönetimi sorunlarını giderme](../troubleshoot/update-management.md).
