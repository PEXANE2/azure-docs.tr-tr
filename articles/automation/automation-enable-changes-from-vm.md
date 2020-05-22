---
title: Azure VM 'den Azure Otomasyonu Değişiklik İzleme ve envanterini etkinleştirme
description: Bu makalede bir Azure VM 'den Değişiklik İzleme ve envanterin nasıl etkinleştirileceği açıklanır.
services: automation
ms.date: 03/04/2020
ms.topic: conceptual
ms.custom: mvc
ms.openlocfilehash: 4eccdef6bd3f2bfcd0eced8281f7b998536f22a9
ms.sourcegitcommit: 493b27fbfd7917c3823a1e4c313d07331d1b732f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/21/2020
ms.locfileid: "83749189"
---
# <a name="enable-change-tracking-and-inventory-from-an-azure-vm"></a>Azure VM 'den Değişiklik İzleme ve envanterini etkinleştirme

Bu makalede, diğer makinelerde [değişiklik izleme ve envanter](change-tracking.md) özelliğini etkinleştirmek Için BIR Azure VM 'yi nasıl kullanabileceğiniz açıklanır. Azure VM 'lerini ölçekli olarak etkinleştirmek için, Değişiklik İzleme ve envanterini kullanarak var olan bir VM 'yi etkinleştirmeniz gerekir. 

> [!NOTE]
> Değişiklik İzleme ve envanteri etkinleştirirken, bir Log Analytics çalışma alanını ve bir Otomasyon hesabını bağlamak için yalnızca belirli bölgeler desteklenir. Desteklenen eşleme çiftlerinin bir listesi için bkz. [Otomasyon hesabı ve Log Analytics çalışma alanı Için bölge eşleme](how-to/region-mappings.md).

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği. Henüz bir hesabınız yoksa [MSDN abone avantajlarınızı etkinleştirebilir](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) veya [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)için kaydolabilirsiniz.
* Makineleri yönetmek için [Otomasyon hesabı](automation-offering-get-started.md).
* Bir [sanal makine](../virtual-machines/windows/quick-create-portal.md).

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

https://portal.azure.com adresinden Azure portalında oturum açın.

## <a name="enable-change-tracking-and-inventory"></a>Değişiklik İzlemeyi ve Sayımı Etkinleştirme

1. [Azure Portal](https://portal.azure.com), **sanal makineler** ' i seçin veya giriş sayfasından **sanal makineler** ' i arayıp seçin.

2. Değişiklik İzleme ve envanterini etkinleştirmek istediğiniz VM 'yi seçin. Otomasyon hesabınızın konumu ne olduğuna bakılmaksızın, VM 'Ler herhangi bir bölgede bulunabilir.

3. VM sayfasında, **yapılandırma yönetimi**altında **Stok** veya **değişiklik izleme** ' yi seçin.

4. `Microsoft.OperationalInsights/workspaces/read`VM 'nin bir çalışma alanı için etkinleştirilip etkinleştirilmediğini belirleme izninizin olması gerekir. Gerekli ek izinler hakkında daha fazla bilgi edinmek için bkz. [özellik kurulumu izinleri](automation-role-based-access-control.md#feature-setup-permissions). Aynı anda birden çok makineyi etkinleştirmeyi öğrenmek için bkz. [Otomasyon hesabından değişiklik izleme ve envanteri etkinleştirme](automation-enable-changes-from-auto-acct.md).

5. Log Analytics çalışma alanını ve otomasyon hesabını seçin ve VM için Değişiklik İzleme ve envanteri etkinleştirmek üzere **Etkinleştir** ' e tıklayın. Kurulumun tamamlanabilmesi 15 dakika sürer. 

## <a name="check-the-scope-configuration"></a><a name="scope-configuration"></a>Kapsam yapılandırmasını denetleme

Değişiklik İzleme ve envanter, özelliği etkinleştirmek üzere bilgisayarları hedeflemek için çalışma alanındaki kapsam yapılandırmasını kullanır. Kapsam yapılandırması, özelliğin kapsamını belirli bilgisayarlarla sınırlandırmak için kullanılan bir veya daha fazla kayıtlı arama grubudur. Daha fazla bilgi için bkz. [değişiklik izleme ve envanter için kapsam yapılandırmalarına sahip çalışma](automation-scope-configurations-change-tracking.md).

## <a name="next-steps"></a>Sonraki adımlar

* [Değişiklik İzleme ve stoku yönetme](change-tracking-file-contents.md)
* [Değişiklik İzleme ve envanter için kapsam yapılandırmalarına sahip çalışma](automation-scope-configurations-change-tracking.md)
* [VM üzerinde yazılım tanımlama](automation-tutorial-installed-software.md)
* [Değişiklik İzleme ve envanter için çalışma alanının Otomasyon hesabından bağlantısını kaldırma](automation-unlink-workspace-change-tracking.md)
* [Değişiklik İzleme ve envanterden VM 'Leri kaldırma](automation-remove-vms-from-change-tracking.md)
* [Azure VM 'deki değişikliklerle ilgili sorunları giderme](automation-tutorial-troubleshoot-changes.md)
* [Değişiklik İzleme ve envanter sorunlarını giderme](troubleshoot/change-tracking.md)
