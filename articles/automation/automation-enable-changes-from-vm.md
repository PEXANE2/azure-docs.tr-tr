---
title: Azure VM 'den Azure Otomasyonu Değişiklik İzleme ve envanterini etkinleştirme
description: Bu makalede bir Azure VM 'den Değişiklik İzleme ve envanterin nasıl etkinleştirileceği açıklanır.
services: automation
ms.date: 03/04/2020
ms.topic: conceptual
ms.custom: mvc
ms.openlocfilehash: 7e87d5b3a4488dac05758e160b09f8fa8b393d09
ms.sourcegitcommit: 6a9f01bbef4b442d474747773b2ae6ce7c428c1f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/27/2020
ms.locfileid: "84117938"
---
# <a name="enable-change-tracking-and-inventory-from-an-azure-vm"></a>Azure VM’den Değişiklik İzleme ve Stok özelliğini etkinleştirme

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

## <a name="limit-the-scope-for-the-deployment"></a><a name="scope-configuration"></a>Dağıtımın kapsamını sınırlandırma

Değişiklik İzleme ve stok, değişiklikleri almak üzere bilgisayarları hedeflemek için çalışma alanındaki kapsam yapılandırmasını kullanır. Daha fazla bilgi için bkz. [Limit değişiklik izleme ve Inventory Deployment Scope](automation-scope-configurations-change-tracking.md).

## <a name="next-steps"></a>Sonraki adımlar

* Özelliği ile çalışma hakkında ayrıntılı bilgi için bkz. [Manage değişiklik izleme and Inventory](change-tracking-file-contents.md).
* Kapsam konfigürasyonları hakkında daha fazla bilgi için bkz. [Limit değişiklik izleme and Inventory Deployment Scope](automation-scope-configurations-change-tracking.md).
* Ortamınızdaki yüklü yazılımları belirlemek için özelliğini kullanmayı öğrenmek için bkz. [sanal makinelerinize hangi yazılımın yüklendiğini keşfetme](automation-tutorial-installed-software.md).
* Özelliği etkinleştirirken Otomasyon hesabınızı bir Log Analytics çalışma alanıyla tümleştirmek istemiyorsanız, bkz. [çalışma alanının Otomasyon hesabından bağlantısını](automation-unlink-workspace-change-tracking.md)kaldırma.
* VM 'lere yapılan değişiklikleri dağıtma işlemi tamamlandığında, [değişiklik izleme ve envanterden VM 'Leri kaldırma](automation-remove-vms-from-change-tracking.md)bölümünde açıklandığı gibi bunları kaldırabilirsiniz.
* Özellikle ilgili sorunları gidermek için bkz. [değişiklik izleme ve envanter sorunlarını giderme](troubleshoot/change-tracking.md).
