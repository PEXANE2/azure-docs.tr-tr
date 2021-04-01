---
title: Otomasyon hesabından Azure Otomasyonu Değişiklik İzleme ve envanterini etkinleştirme
description: Bu makalede bir Otomasyon hesabından Değişiklik İzleme ve envanterin nasıl etkinleştirileceği açıklanır.
services: automation
ms.subservice: change-inventory-management
ms.date: 10/14/2020
ms.topic: conceptual
ms.openlocfilehash: 32fb95c88d632cc2c51cd2390f0244e9c1927051
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2021
ms.locfileid: "100585886"
---
# <a name="enable-change-tracking-and-inventory-from-an-automation-account"></a>Otomasyon hesabından Değişiklik İzleme ve Stok özelliğini etkinleştirme

Bu makalede, ortamınızdaki VM 'Ler için [değişiklik izleme ve envanteri](overview.md) etkinleştirmek üzere otomasyon hesabınızı nasıl kullanabileceğiniz açıklanır. Azure VM 'lerini ölçekli olarak etkinleştirmek için, Değişiklik İzleme ve envanterini kullanarak var olan bir VM 'yi etkinleştirmeniz gerekir.

> [!NOTE]
> Değişiklik İzleme ve envanteri etkinleştirirken, bir Log Analytics çalışma alanını ve bir Otomasyon hesabını bağlamak için yalnızca belirli bölgeler desteklenir. Desteklenen eşleme çiftlerinin bir listesi için bkz. [Otomasyon hesabı ve Log Analytics çalışma alanı Için bölge eşleme](../how-to/region-mappings.md).

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği. Henüz bir hesabınız yoksa [MSDN abone avantajlarınızı etkinleştirebilir](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) veya [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)için kaydolabilirsiniz.
* Makineleri yönetmek için [Otomasyon hesabı](../automation-security-overview.md).
* Bir [sanal makine](../../virtual-machines/windows/quick-create-portal.md).

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

Adresinden Azure 'da oturum açın https://portal.azure.com .

## <a name="enable-change-tracking-and-inventory"></a>Değişiklik İzlemeyi ve Sayımı Etkinleştirme

1. Otomasyon hesabınıza gidin ve **yapılandırma yönetimi** altında **Envanter** veya **değişiklik izleme** ' yi seçin.

2. Log Analytics çalışma alanını ve otomasyon hesabını seçip Değişiklik İzleme ve stoku etkinleştirmek için **Etkinleştir** ' e tıklayın. Kurulumun tamamlanabilmesi 15 dakika sürer.

    ![Değişiklik İzlemeyi ve Sayımı Etkinleştirme](media/enable-from-automation-account/enable-feature.png)

## <a name="enable-azure-vms"></a>Azure VM 'lerini etkinleştirin

1. Otomasyon hesabınızdan, **yapılandırma yönetimi** altında **Envanter** veya **değişiklik izleme** ' yi seçin.

2. **+ Azure VM 'Leri Ekle** ' ye tıklayın ve listeden bir veya daha fazla VM seçin. Etkin olmayan sanal makineler gri, seçilemez ve seçilemiyor. Azure VM 'Ler, Otomasyon hesabınızın konumu ne olduğuna bakılmaksızın herhangi bir bölgede bulunabilir. 

3. Seçilen VM 'Leri, özellik için kayıtlı bilgisayar grubu aramasına eklemek için **Etkinleştir** ' e tıklayın. Daha fazla bilgi için bkz. [Limit değişiklik izleme ve Inventory Deployment Scope](manage-scope-configurations.md).

      [![Azure VM 'lerini etkinleştirin](./media/enable-from-automation-account/enable-azure-vms.png)](./media/enable-from-automation-account/enable-azure-vms-expanded.png#lightbox)

## <a name="enable-non-azure-vms"></a>Azure dışı VM 'Leri etkinleştirme

Azure 'da bulunmayan makinelerin el ile eklenmesi gerekir. Windows veya Linux için Log Analytics aracısını, önce makinenizi [Azure Arc etkin sunucularına](../../azure-arc/servers/overview.md)bağlayarak ve ardından Azure ilkesi 'ni kullanarak, [ *Linux* veya *Windows* Azure Arc makineler yerleşik ilkesine dağıtım Log Analytics aracısını](../../governance/policy/samples/built-in-policies.md#monitoring) atamak için önerilir. Makineleri VM'ler için Azure İzleyici de izlemeyi planlıyorsanız, bunun yerine [Enable VM'ler için Azure izleyici](../../governance/policy/samples/built-in-initiatives.md#monitoring) girişimi kullanın.

1. Otomasyon hesabınızdan, **yapılandırma yönetimi** altında **Envanter** veya **değişiklik izleme** ' yi seçin.

2. **Azure dışı makine Ekle**' ye tıklayın. Bu eylem, makinenin Değişiklik İzleme ve envanter işlemlerine raporlamaya başlayabilmesi [için Log Analytics aracısını Windows için yüklemek ve yapılandırmak üzere yönergeler](../../azure-monitor/agents/log-analytics-agent.md) içeren yeni bir tarayıcı penceresi açar. Şu anda Operations Manager tarafından yönetilen bir makine etkinleştiriyorsanız, yeni bir aracı gerekli değildir ve çalışma alanı bilgileri mevcut aracıya girilir.

## <a name="enable-machines-in-the-workspace"></a>Çalışma alanındaki makineleri etkinleştir

Çalışma alanınıza zaten rapor veren el ile yüklenen makineler veya makineler Değişiklik İzleme için Azure Otomasyonu 'na eklenmelidir ve stokun etkinleştirilmesi gerekir.

1. Otomasyon hesabınızdan, **yapılandırma yönetimi** altında **Envanter** veya **değişiklik izleme** ' yi seçin.

2. **Makineleri Yönet**' i seçin. Daha önce **tüm mevcut ve gelecekteki makinelerde etkinleştir** seçeneğini belirlediyseniz, **makineleri Yönet** seçeneği gri olabilir

    ![Kayıtlı aramalar](media/enable-from-automation-account/manage-machines.png)

3. Tüm kullanılabilir makineler için Değişiklik İzleme ve stoku etkinleştirmek üzere **makineleri Yönet** sayfasında **kullanılabilir tüm makinelerde etkinleştir** ' i seçin. Bu eylem, tek başına makineleri eklemek için denetimi devre dışı bırakır ve çalışma alanına raporlayan tüm makineleri, bilgisayar grubu kayıtlı arama sorgusuna ekler. Seçildiğinde, bu eylem **makineleri Yönet** seçeneğini devre dışı bırakır.

4. Tüm kullanılabilir makineler ve gelecekteki makineler için özelliği etkinleştirmek üzere **tüm kullanılabilir ve gelecekteki makinelerde etkinleştir**' i seçin. Bu seçenek, kayıtlı arama ve kapsam yapılandırmasını çalışma alanından siler ve çalışma alanına raporlama yapan tüm Azure ve Azure dışı makineler için özelliği açar. Seçildiğinde, bu eylem, hiçbir kapsam yapılandırması bulunmadığından **makineleri Yönet** seçeneğini kalıcı olarak devre dışı bırakır.

    > [!NOTE]
    > Bu seçenek Log Analytics içindeki kayıtlı arama ve kapsam yapılandırmasını sildiği için, bu seçeneği seçmeden önce Log Analytics çalışma alanındaki tüm silme kilitlerini kaldırmak önemlidir. Bunu yapmazsanız, bu seçenek yapılandırmaların kaldırılmasına neden olur ve bunları el ile kaldırmanız gerekir.

5. Gerekirse, ilk kaydedilmiş aramayı yeniden ekleyerek kapsam yapılandırmasını geri ekleyebilirsiniz. Daha fazla bilgi için bkz. [Limit değişiklik izleme ve Inventory Deployment Scope](manage-scope-configurations.md).

6. Bir veya daha fazla makine için özelliği etkinleştirmek üzere **Seçili makinelerde etkinleştir** ' i seçin ve özelliği etkinleştirmek için her makinenin yanındaki **Ekle** ' ye tıklayın. Bu görev, seçilen makine adlarını, bu özellik için bilgisayar grubu kayıtlı arama sorgusuna ekler.

## <a name="next-steps"></a>Sonraki adımlar

* Özelliği ile çalışma hakkında ayrıntılı bilgi için bkz. [değişiklik izleme yönetme](manage-change-tracking.md) ve [envanteri yönetme](manage-inventory-vms.md).

* Özellikle ilgili sorunları gidermek için bkz. [değişiklik izleme ve envanter sorunlarını giderme](../troubleshoot/change-tracking.md).
