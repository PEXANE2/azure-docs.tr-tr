---
title: Azure VM 'den Azure Otomasyonu Güncelleştirme Yönetimi etkinleştirme
description: Bu makalede bir Azure VM 'den Güncelleştirme Yönetimi nasıl etkinleştirileceği açıklanır.
services: automation
ms.date: 09/16/2020
ms.topic: conceptual
ms.custom: mvc
ms.openlocfilehash: 2d81b822986f641e64b9211300f83e3b254f316d
ms.sourcegitcommit: 8d8deb9a406165de5050522681b782fb2917762d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/20/2020
ms.locfileid: "92223048"
---
# <a name="enable-update-management-from-an-azure-vm"></a>Azure VM’den Güncelleştirme Yönetimi’ni etkinleştirme

Bu makalede, diğer makinelerde [güncelleştirme yönetimi](overview.md) özelliğini etkinleştirmek Için BIR Azure VM 'yi nasıl kullanabileceğiniz açıklanır. Azure VM 'lerini ölçekli olarak etkinleştirmek için, Güncelleştirme Yönetimi kullanarak var olan bir VM 'yi etkinleştirmeniz gerekir.

> [!NOTE]
> Güncelleştirme Yönetimi etkinleştirilirken, bir Log Analytics çalışma alanını ve bir Otomasyon hesabını bağlamak için yalnızca belirli bölgeler desteklenir. Desteklenen eşleme çiftlerinin bir listesi için bkz. [Otomasyon hesabı ve Log Analytics çalışma alanı Için bölge eşleme](../how-to/region-mappings.md).

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği. Henüz bir hesabınız yoksa [MSDN abone avantajlarınızı etkinleştirebilir](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) veya [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)için kaydolabilirsiniz.
* Makineleri yönetmek için [Otomasyon hesabı](../index.yml).
* Bir [sanal makine](../../virtual-machines/windows/quick-create-portal.md).

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

[Azure portalında](https://portal.azure.com) oturum açın.

## <a name="enable-the-feature-for-deployment"></a>Dağıtım için özelliği etkinleştirin

1. [Azure Portal](https://portal.azure.com), **sanal makineler** ' i seçin veya giriş sayfasından **sanal makineler** ' i arayıp seçin.

2. Güncelleştirme Yönetimi etkinleştirmek istediğiniz VM 'yi seçin. Otomasyon hesabınızın konumu ne olduğuna bakılmaksızın, VM 'Ler herhangi bir bölgede bulunabilir. Siz

3. VM sayfasında, **işlemler**altında **Konuk + ana bilgisayar güncelleştirmeleri**' ni seçin.

    ![Sol bölmeden Konuk + ana bilgisayar güncelleştirmelerini seçin](media/enable-from-vm/select-guest-and-os-updates.png)

4. `Microsoft.OperationalInsights/workspaces/read`VM 'nin bir çalışma alanı için etkinleştirilip etkinleştirilmediğini belirleme izninizin olması gerekir. Gerekli ek izinler hakkında daha fazla bilgi edinmek için bkz. [makineleri etkinleştirmek için gereken izinler](../automation-role-based-access-control.md#feature-setup-permissions). Aynı anda birden çok makineyi etkinleştirmeyi öğrenmek için bkz. [Otomasyon hesabından güncelleştirme yönetimi etkinleştirme](update-mgmt-enable-automation-account.md).

5. Etkinleştir Güncelleştirme Yönetimi sayfasında, Log Analytics çalışma alanı ve Otomasyon hesabı ' nı seçin ve Güncelleştirme Yönetimi etkinleştirmek için **Etkinleştir** ' e tıklayın. Güncelleştirme Yönetimi etkinleştirildikten sonra, VM 'den güncelleştirme değerlendirmesini görüntüleyebilmeniz için 15 dakika sürebilir.

    ![Güncelleştirme Yönetimi’ni etkinleştirme](media/enable-from-vm/enable-update-management.png)

## <a name="next-steps"></a>Sonraki adımlar

* VM 'Ler için Güncelleştirme Yönetimi kullanmak için bkz. [Azure VM 'leriniz için güncelleştirmeleri ve düzeltme eklerini yönetme](manage-updates-for-vm.md).

* Genel Güncelleştirme Yönetimi hatalarıyla ilgili sorunları gidermek için bkz. [güncelleştirme yönetimi sorunlarını giderme](../troubleshoot/update-management.md).
