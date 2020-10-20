---
title: Otomasyon hesabından Azure Otomasyonu Değişiklik İzleme ve envanterini etkinleştirme
description: Bu makalede bir Otomasyon hesabından Değişiklik İzleme ve envanterin nasıl etkinleştirileceği açıklanır.
services: automation
ms.subservice: change-inventory-management
ms.date: 10/14/2020
ms.topic: conceptual
ms.openlocfilehash: c3630105e70ac28e7e9041aa9d5400f724401a5b
ms.sourcegitcommit: 957c916118f87ea3d67a60e1d72a30f48bad0db6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/19/2020
ms.locfileid: "92210408"
---
# <a name="enable-change-tracking-and-inventory-from-an-automation-account"></a>Otomasyon hesabından Değişiklik İzleme ve Stok özelliğini etkinleştirme

Bu makalede, ortamınızdaki VM 'Ler için [değişiklik izleme ve envanteri](overview.md) etkinleştirmek üzere otomasyon hesabınızı nasıl kullanabileceğiniz açıklanır. Azure VM 'lerini ölçekli olarak etkinleştirmek için, Değişiklik İzleme ve envanterini kullanarak var olan bir VM 'yi etkinleştirmeniz gerekir.

> [!NOTE]
> Değişiklik İzleme ve envanteri etkinleştirirken, bir Log Analytics çalışma alanını ve bir Otomasyon hesabını bağlamak için yalnızca belirli bölgeler desteklenir. Desteklenen eşleme çiftlerinin bir listesi için bkz. [Otomasyon hesabı ve Log Analytics çalışma alanı Için bölge eşleme](../how-to/region-mappings.md).

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği. Henüz bir hesabınız yoksa [MSDN abone avantajlarınızı etkinleştirebilir](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) veya [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)için kaydolabilirsiniz.
* Makineleri yönetmek için [Otomasyon hesabı](../index.yml).
* Bir [sanal makine](../../virtual-machines/windows/quick-create-portal.md).

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

Adresinden Azure 'da oturum açın https://portal.azure.com .

## <a name="enable-change-tracking-and-inventory"></a>Değişiklik İzlemeyi ve Sayımı Etkinleştirme

1. Otomasyon hesabınıza gidin ve **yapılandırma yönetimi**altında **Envanter** veya **değişiklik izleme** ' yi seçin.

2. Log Analytics çalışma alanını ve otomasyon hesabını seçip Değişiklik İzleme ve stoku etkinleştirmek için **Etkinleştir** ' e tıklayın. Kurulumun tamamlanabilmesi 15 dakika sürer.

    ![Değişiklik İzlemeyi ve Sayımı Etkinleştirme](media/enable-from-automation-account/enable-feature.png)

## <a name="enable-azure-vms"></a>Azure VM 'lerini etkinleştirin

1. Otomasyon hesabınızdan, **yapılandırma yönetimi**altında **Envanter** veya **değişiklik izleme** ' yi seçin.

2. **+ Azure VM 'Leri Ekle** ' ye tıklayın ve listeden bir veya daha fazla VM seçin. Etkin olmayan sanal makineler gri, seçilemez ve seçilemiyor. Azure VM 'Ler, Otomasyon hesabınızın konumu ne olduğuna bakılmaksızın herhangi bir bölgede bulunabilir. 

3. Seçilen VM 'Leri, özellik için kayıtlı bilgisayar grubu aramasına eklemek için **Etkinleştir** ' e tıklayın. Daha fazla bilgi için bkz. [Limit değişiklik izleme ve Inventory Deployment Scope](manage-scope-configurations.md).

      [![Azure VM 'lerini etkinleştirin](./media/enable-from-automation-account/enable-azure-vms.png)](./media/enable-from-automation-account/enable-azure-vms-expanded.png#lightbox)

## <a name="enable-non-azure-vms"></a>Azure dışı VM 'Leri etkinleştirme

Azure 'da bulunmayan makinelerin el ile eklenmesi gerekir.

1. Otomasyon hesabınızdan, **yapılandırma yönetimi**altında **Envanter** veya **değişiklik izleme** ' yi seçin.

2. **Azure dışı makine Ekle**' ye tıklayın. Bu eylem, makinenin Değişiklik İzleme ve envanter işlemlerine raporlamaya başlayabilmesi [için Log Analytics aracısını Windows için yüklemek ve yapılandırmak üzere yönergeler](../../azure-monitor/platform/log-analytics-agent.md) içeren yeni bir tarayıcı penceresi açar. Şu anda Operations Manager tarafından yönetilen bir makine etkinleştiriyorsanız, yeni bir aracı gerekli değildir ve çalışma alanı bilgileri mevcut aracıya girilir.

## <a name="enable-machines-in-the-workspace"></a>Çalışma alanındaki makineleri etkinleştir

Çalışma alanınıza zaten rapor veren el ile yüklenen makineler veya makineler Değişiklik İzleme için Azure Otomasyonu 'na eklenmelidir ve stokun etkinleştirilmesi gerekir.

1. Otomasyon hesabınızdan, **yapılandırma yönetimi**altında **Envanter** veya **değişiklik izleme** ' yi seçin.

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
