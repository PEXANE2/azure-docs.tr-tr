---
title: Azure VM 'den Azure Otomasyonu Güncelleştirme Yönetimi etkinleştirme
description: Bu makalede bir Azure VM 'den Güncelleştirme Yönetimi nasıl etkinleştirileceği açıklanır.
services: automation
ms.date: 03/04/2020
ms.topic: conceptual
ms.custom: mvc
ms.openlocfilehash: 481d59bd2a06103629dee88868d9e33de810fdab
ms.sourcegitcommit: 0fa52a34a6274dc872832560cd690be58ae3d0ca
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/29/2020
ms.locfileid: "84204914"
---
# <a name="enable-update-management-from-an-azure-vm"></a>Azure VM’den Güncelleştirme Yönetimi’ni etkinleştirme

Bu makalede, diğer makinelerde [güncelleştirme yönetimi](automation-update-management.md) özelliğini etkinleştirmek Için BIR Azure VM 'yi nasıl kullanabileceğiniz açıklanır. Azure VM 'lerini ölçekli olarak etkinleştirmek için, Güncelleştirme Yönetimi kullanarak var olan bir VM 'yi etkinleştirmeniz gerekir. 

> [!NOTE]
> Güncelleştirme Yönetimi etkinleştirilirken, bir Log Analytics çalışma alanını ve bir Otomasyon hesabını bağlamak için yalnızca belirli bölgeler desteklenir. Desteklenen eşleme çiftlerinin bir listesi için bkz. [Otomasyon hesabı ve Log Analytics çalışma alanı Için bölge eşleme](how-to/region-mappings.md).

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği. Henüz bir hesabınız yoksa [MSDN abone avantajlarınızı etkinleştirebilir](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) veya [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)için kaydolabilirsiniz.
* Makineleri yönetmek için [Otomasyon hesabı](automation-offering-get-started.md).
* Bir [sanal makine](../virtual-machines/windows/quick-create-portal.md).

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

https://portal.azure.com adresinden Azure portalında oturum açın.

## <a name="enable-the-feature-for-deployment"></a>Dağıtım için özelliği etkinleştirin

1. [Azure Portal](https://portal.azure.com), **sanal makineler** ' i seçin veya giriş sayfasından **sanal makineler** ' i arayıp seçin.

2. Güncelleştirme Yönetimi etkinleştirmek istediğiniz VM 'yi seçin. Otomasyon hesabınızın konumu ne olduğuna bakılmaksızın, VM 'Ler herhangi bir bölgede bulunabilir. Siz

3. VM sayfasında, **işlemler**altında **güncelleştirme yönetimi**' yi seçin.

4. `Microsoft.OperationalInsights/workspaces/read`VM 'nin bir çalışma alanı için etkinleştirilip etkinleştirilmediğini belirleme izninizin olması gerekir. Gerekli ek izinler hakkında daha fazla bilgi edinmek için bkz. [makineleri etkinleştirmek için gereken izinler](automation-role-based-access-control.md#feature-setup-permissions). Aynı anda birden çok makineyi etkinleştirmeyi öğrenmek için bkz. [Otomasyon hesabından güncelleştirme yönetimi etkinleştirme](automation-onboard-solutions-from-automation-account.md).

5. Log Analytics çalışma alanını ve otomasyon hesabını seçin ve Güncelleştirme Yönetimi etkinleştirmek için **Etkinleştir** ' e tıklayın. Kurulumun tamamlanabilmesi 15 dakika sürer. 

    ![Güncelleştirme Yönetimi’ni etkinleştirme](media/automation-tutorial-update-management/manageupdates-update-enable.png)

## <a name="next-steps"></a>Sonraki adımlar

* VM 'Ler için Güncelleştirme Yönetimi kullanmak için bkz. [Azure VM 'leriniz için güncelleştirmeleri ve düzeltme eklerini yönetme](automation-tutorial-update-management.md).
* Genel Güncelleştirme Yönetimi hatalarıyla ilgili sorunları gidermek için bkz. [güncelleştirme yönetimi sorunlarını giderme](troubleshoot/update-management.md).
* Windows Update Aracısı sorunlarını gidermek için bkz. [Windows Update Aracısı sorunlarını giderme](troubleshoot/update-agent-issues.md).
* Linux Güncelleştirme Aracısı sorunlarını gidermek için bkz. [Linux Güncelleştirme Aracısı sorunlarını giderme](troubleshoot/update-agent-issues-linux.md).
