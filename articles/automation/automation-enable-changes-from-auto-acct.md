---
title: Otomasyon hesabından Azure Otomasyonu Değişiklik İzleme ve envanterini etkinleştirme
description: Bu makalede bir Otomasyon hesabından Değişiklik İzleme ve envanterin nasıl etkinleştirileceği açıklanır.
services: automation
ms.date: 4/11/2019
ms.topic: conceptual
ms.custom: mvc
ms.openlocfilehash: 31390d260f51bdc1e9858d21153aa96ea7c54405
ms.sourcegitcommit: 6a9f01bbef4b442d474747773b2ae6ce7c428c1f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/27/2020
ms.locfileid: "84116646"
---
# <a name="enable-change-tracking-and-inventory-from-an-automation-account"></a>Otomasyon hesabından Değişiklik İzleme ve Stok özelliğini etkinleştirme

Bu makalede, ortamınızdaki VM 'Ler için [değişiklik izleme ve envanter](change-tracking.md) özelliğini etkinleştirmek üzere otomasyon hesabınızı nasıl kullanabileceğiniz açıklanır. Azure VM 'lerini ölçekli olarak etkinleştirmek için, Değişiklik İzleme ve envanterini kullanarak var olan bir VM 'yi etkinleştirmeniz gerekir. 

> [!NOTE]
> Değişiklik İzleme ve envanteri etkinleştirirken, bir Log Analytics çalışma alanını ve bir Otomasyon hesabını bağlamak için yalnızca belirli bölgeler desteklenir. Desteklenen eşleme çiftlerinin bir listesi için bkz. [Otomasyon hesabı ve Log Analytics çalışma alanı Için bölge eşleme](how-to/region-mappings.md).

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği. Henüz bir hesabınız yoksa [MSDN abone avantajlarınızı etkinleştirebilir](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) veya [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)için kaydolabilirsiniz.
* Makineleri yönetmek için [Otomasyon hesabı](automation-offering-get-started.md).
* Bir [sanal makine](../virtual-machines/windows/quick-create-portal.md).

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

Adresinden Azure 'da oturum açın https://portal.azure.com .

## <a name="enable-change-tracking-and-inventory"></a>Değişiklik İzlemeyi ve Sayımı Etkinleştirme

1. Otomasyon hesabınıza gidin ve **yapılandırma yönetimi**altında **Envanter** veya **değişiklik izleme** ' yi seçin.

2. Log Analytics çalışma alanını ve otomasyon hesabını seçip Değişiklik İzleme ve stoku etkinleştirmek için **Etkinleştir** ' e tıklayın. Kurulumun tamamlanabilmesi 15 dakika sürer.

    ![Değişiklik İzlemeyi ve Sayımı Etkinleştirme](media/automation-enable-changes-from-auto-acct/onboardsolutions.png)

## <a name="limit-the-scope-for-the-deployment"></a><a name="scope-configuration"></a>Dağıtımın kapsamını sınırlandırma

Değişiklik İzleme ve stok, değişiklikleri almak üzere bilgisayarları hedeflemek için çalışma alanındaki kapsam yapılandırmasını kullanır. Daha fazla bilgi için bkz. [Limit değişiklik izleme ve Inventory Deployment Scope](automation-scope-configurations-change-tracking.md).

## <a name="enable-azure-vms"></a>Azure VM 'lerini etkinleştirin

1. Otomasyon hesabınızdan, **yapılandırma yönetimi**altında **Envanter** veya **değişiklik izleme** ' yi seçin.

2. **+ Azure VM 'Leri Ekle** ' ye tıklayın ve listeden bir veya daha fazla VM seçin. Etkin olmayan sanal makineler gri, seçilemez ve seçilemiyor. Azure VM 'Ler, Otomasyon hesabınızın konumu ne olduğuna bakılmaksızın herhangi bir bölgede bulunabilir. 

3. Seçilen VM 'Leri, özellik için kayıtlı bilgisayar grubu aramasına eklemek için **Etkinleştir** ' e tıklayın. Daha fazla bilgi için bkz. [Limit değişiklik izleme ve Inventory Deployment Scope](automation-scope-configurations-change-tracking.md).

    ![Azure VM 'lerini etkinleştirin](media/automation-enable-changes-from-auto-acct/enable-azure-vms.png)

## <a name="enable-non-azure-vms"></a>Azure dışı VM 'Leri etkinleştirme

Azure 'da bulunmayan makinelerin el ile eklenmesi gerekir. 

1. Otomasyon hesabınızdan, **yapılandırma yönetimi**altında **Envanter** veya **değişiklik izleme** ' yi seçin.

2. **Azure dışı makine Ekle**' ye tıklayın. Bu eylem, makinenin Değişiklik İzleme ve envanter işlemlerine raporlamaya başlayabilmesi [için Log Analytics aracısını Windows için yüklemek ve yapılandırmak üzere yönergeler](../azure-monitor/platform/log-analytics-agent.md) içeren yeni bir tarayıcı penceresi açar. Şu anda Operations Manager tarafından yönetilen bir makine etkinleştiriyorsanız, yeni bir aracı gerekli değildir ve çalışma alanı bilgileri mevcut aracıya girilir.

## <a name="enable-machines-in-the-workspace"></a>Çalışma alanındaki makineleri etkinleştir

Çalışma alanınıza zaten rapor veren el ile yüklenen makineler veya makineler Değişiklik İzleme için Azure Otomasyonu 'na eklenmelidir ve stokun etkinleştirilmesi gerekir. 

1. Otomasyon hesabınızdan, **yapılandırma yönetimi**altında **Envanter** veya **değişiklik izleme** ' yi seçin.

2. **Makineleri Yönet**' i seçin. Daha önce **tüm mevcut ve gelecekteki makinelerde etkinleştir** seçeneğini belirlediyseniz **makineleri Yönet** düğmesi gri olabilir

    ![Kayıtlı aramalar](media/automation-enable-changes-from-auto-acct/managemachines.png)

3. Tüm kullanılabilir makineler için Değişiklik İzleme ve stoku etkinleştirmek üzere makineleri Yönet sayfasında **kullanılabilir tüm makinelerde etkinleştir** ' i seçin. Bu eylem, tek tek makineleri eklemek için denetimi devre dışı bırakır. Bu görev, raporlayan makinelerin tüm adlarını çalışma alanına kayıtlı bilgisayar grubu arama sorgusuna ekler. Seçildiğinde, bu eylem **makineleri Yönet** düğmesini devre dışı bırakır.

4. Tüm kullanılabilir makineler ve gelecekteki makineler için özelliği etkinleştirmek üzere **tüm kullanılabilir ve gelecekteki makinelerde etkinleştir**' i seçin. Bu seçenek, kaydedilmiş aramaları ve kapsam yapılandırmasını çalışma alanından siler ve çalışma alanına raporlama yapan tüm Azure ve Azure dışı makineler için özelliği açar. Seçildiğinde, bu eylem, hiçbir kapsam yapılandırması bulunmadığından **makineleri Yönet** düğmesini kalıcı olarak devre dışı bırakır.

5. Gerekirse, ilk kaydedilmiş aramaları yeniden ekleyerek kapsam yapılandırma işlemleri geri eklenebilir. Daha fazla bilgi için bkz. [değişiklik izleme ve envanter için kapsam yapılandırmalarına sahip çalışma](automation-scope-configurations-change-tracking.md).

6. Bir veya daha fazla makine için özelliği etkinleştirmek üzere **Seçili makinelerde etkinleştir** ' i seçin ve özelliği etkinleştirmek için her makinenin yanındaki **Ekle** ' ye tıklayın. Bu görev, seçilen makine adlarını, bu özellik için bilgisayar grubu kayıtlı arama sorgusuna ekler.

## <a name="next-steps"></a>Sonraki adımlar

* Özelliği hakkında daha fazla bilgi için bkz. [Manage değişiklik izleme and Inventory](change-tracking-file-contents.md).
* Kapsam konfigürasyonları hakkında daha fazla bilgi için bkz. [Limit değişiklik izleme and Inventory Deployment Scope](automation-scope-configurations-change-tracking.md).
* Ortamınızdaki yüklü yazılımları belirlemek için özelliğini kullanmayı öğrenmek için bkz. [sanal makinelerinize hangi yazılımın yüklendiğini keşfetme](automation-tutorial-installed-software.md).
* Özelliği etkinleştirirken Otomasyon hesabınızı bir Log Analytics çalışma alanıyla tümleştirmek istemiyorsanız, bkz. [çalışma alanının Otomasyon hesabından bağlantısını](automation-unlink-workspace-change-tracking.md)kaldırma.
* VM 'lere yapılan değişiklikleri dağıtma işlemi tamamlandığında, [değişiklik izleme ve envanterden VM 'Leri kaldırma](automation-remove-vms-from-change-tracking.md)bölümünde açıklandığı gibi bunları kaldırabilirsiniz.
* Özellikle ilgili sorunları gidermek için bkz. [değişiklik izleme ve envanter sorunlarını giderme](troubleshoot/change-tracking.md).