---
title: Azure VM 'den Azure Otomasyonu Değişiklik İzleme ve envanterini etkinleştirme
description: Bu makalede bir Azure VM 'den Değişiklik İzleme ve envanterin nasıl etkinleştirileceği açıklanır.
services: automation
ms.subservice: change-inventory-management
ms.date: 10/14/2020
ms.topic: conceptual
ms.openlocfilehash: 9b14dcb9ce2f2426d8d1496541022602a114cb6e
ms.sourcegitcommit: 957c916118f87ea3d67a60e1d72a30f48bad0db6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/19/2020
ms.locfileid: "92210407"
---
# <a name="enable-change-tracking-and-inventory-from-an-azure-vm"></a>Azure VM’den Değişiklik İzleme ve Stok özelliğini etkinleştirme

Bu makalede, diğer makinelerde [değişiklik izleme ve envanteri](overview.md) etkinleştirmek Için BIR Azure VM 'yi nasıl kullanabileceğiniz açıklanır. Azure VM 'lerini ölçekli olarak etkinleştirmek için, Değişiklik İzleme ve envanterini kullanarak var olan bir VM 'yi etkinleştirmeniz gerekir.

> [!NOTE]
> Değişiklik İzleme ve envanteri etkinleştirirken, bir Log Analytics çalışma alanını ve bir Otomasyon hesabını bağlamak için yalnızca belirli bölgeler desteklenir. Desteklenen eşleme çiftlerinin bir listesi için bkz. [Otomasyon hesabı ve Log Analytics çalışma alanı Için bölge eşleme](../how-to/region-mappings.md).

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği. Henüz bir hesabınız yoksa [MSDN abone avantajlarınızı etkinleştirebilir](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) veya [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)için kaydolabilirsiniz.
* Makineleri yönetmek için [Otomasyon hesabı](../index.yml).
* Bir [sanal makine](../../virtual-machines/windows/quick-create-portal.md).

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

https://portal.azure.com adresinden Azure portalında oturum açın.

## <a name="enable-change-tracking-and-inventory"></a>Değişiklik İzlemeyi ve Sayımı Etkinleştirme

1. [Azure Portal](https://portal.azure.com), **sanal makineler** ' i seçin veya giriş sayfasından **sanal makineler** ' i arayıp seçin.

2. Değişiklik İzleme ve envanterini etkinleştirmek istediğiniz VM 'yi seçin. Otomasyon hesabınızın konumu ne olduğuna bakılmaksızın, VM 'Ler herhangi bir bölgede bulunabilir.

3. VM sayfasında, **yapılandırma yönetimi**altında **Stok** veya **değişiklik izleme** ' yi seçin.

4. `Microsoft.OperationalInsights/workspaces/read`VM 'nin bir çalışma alanı için etkinleştirilip etkinleştirilmediğini belirleme izninizin olması gerekir. Gerekli ek izinler hakkında daha fazla bilgi edinmek için bkz. [özellik kurulumu izinleri](../automation-role-based-access-control.md#feature-setup-permissions). Aynı anda birden çok makineyi etkinleştirmeyi öğrenmek için bkz. [Otomasyon hesabından değişiklik izleme ve envanteri etkinleştirme](enable-from-automation-account.md).

5. Log Analytics çalışma alanını ve otomasyon hesabını seçin ve VM için Değişiklik İzleme ve envanteri etkinleştirmek üzere **Etkinleştir** ' e tıklayın. Kurulumun tamamlanabilmesi 15 dakika sürer.

## <a name="next-steps"></a>Sonraki adımlar

* Özelliği ile çalışma hakkında ayrıntılı bilgi için bkz. [değişiklik izleme yönetme](manage-change-tracking.md) ve [envanteri yönetme](manage-inventory-vms.md).

* Özellikle ilgili sorunları gidermek için bkz. [değişiklik izleme ve envanter sorunlarını giderme](../troubleshoot/change-tracking.md).