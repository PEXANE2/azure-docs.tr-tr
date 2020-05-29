---
title: Azure portal Azure Otomasyonu Güncelleştirme Yönetimi etkinleştirme
description: Bu makalede Azure portal Güncelleştirme Yönetimi nasıl etkinleştirileceği açıklanır.
services: automation
ms.date: 04/11/2019
ms.topic: article
ms.custom: mvc
ms.openlocfilehash: d0df98e76e8f0575f14794a53c6987e12a4d05d6
ms.sourcegitcommit: 1692e86772217fcd36d34914e4fb4868d145687b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/29/2020
ms.locfileid: "84170707"
---
# <a name="enable-update-management-from-the-azure-portal"></a>Azure portalından Güncelleştirme Yönetimi’ni etkinleştirme

Bu makalede, Azure portal göz atarak VM 'Ler için [güncelleştirme yönetimi](automation-update-management.md) özelliğini nasıl etkinleştirebileceğinizi açıklanmaktadır. Azure VM 'lerini ölçekli olarak etkinleştirmek için, Güncelleştirme Yönetimi kullanarak var olan bir VM 'yi etkinleştirmeniz gerekir. 

VM 'lerinizi yönetmek için kullanabileceğiniz kaynak gruplarının sayısı [Kaynak Yöneticisi dağıtım limitleriyle](../azure-resource-manager/templates/cross-resource-group-deployment.md)sınırlıdır. Güncelleştirme dağıtımlarıyla karıştırılmamalıdır Kaynak Yöneticisi dağıtımlar, dağıtım başına beş kaynak grubu ile sınırlıdır. Log Analytics çalışma alanını, Otomasyon hesabını ve ilgili kaynakları yapılandırmak için bu kaynak gruplarından ikisi ayrılmıştır. Bu, Güncelleştirme Yönetimi tarafından yönetim için seçilecek üç kaynak grubu ile birlikte kalır. Bu sınır, bir Otomasyon özelliği tarafından yönetilebilen kaynak gruplarının sayısını değil, yalnızca eşzamanlı kurulum için geçerlidir.

> [!NOTE]
> Güncelleştirme Yönetimi etkinleştirilirken, bir Log Analytics çalışma alanını ve bir Otomasyon hesabını bağlamak için yalnızca belirli bölgeler desteklenir. Desteklenen eşleme çiftlerinin bir listesi için bkz. [Otomasyon hesabı ve Log Analytics çalışma alanı Için bölge eşleme](how-to/region-mappings.md).

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği. Henüz bir hesabınız yoksa [MSDN abone avantajlarınızı etkinleştirebilir](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) veya [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)için kaydolabilirsiniz.
* Makineleri yönetmek için [Otomasyon hesabı](automation-offering-get-started.md).
* Bir [sanal makine](../virtual-machines/windows/quick-create-portal.md).

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

Adresinden Azure 'da oturum açın https://portal.azure.com .

## <a name="enable-update-management"></a>Güncelleştirme Yönetimi’ni etkinleştirme

1. Azure portal, **sanal makineler**' e gidin.

2. Güncelleştirme Yönetimi eklenecek VM 'Leri seçmek için onay kutularını kullanın. Tek seferde en fazla üç farklı kaynak grubu için makine ekleyebilirsiniz. Otomasyon hesabınızın konumu ne olduğuna bakılmaksızın, Azure VM 'Leri herhangi bir bölgede bulunabilir.

    ![VM 'lerin listesi](media/automation-onboard-solutions-from-browse/vmlist.png)

    > [!TIP]
    > Farklı aboneliklerden, konumlardan ve kaynak gruplarından VM 'Leri seçmek için filtre denetimlerini kullanın. Bir listedeki tüm sanal makineleri seçmek için üstteki onay kutusuna tıklayabilirsiniz.

    ![Güncelleştirme Yönetimi’ni etkinleştirme](media/automation-onboard-solutions-from-browse/onboardsolutions.png)

3. **Hizmetler** ' e tıklayın ve güncelleştirme yönetimi özelliği için **güncelleştirme yönetimi** ' yi seçin. 

4. Sanal makinelerin listesi, yalnızca aynı abonelikte ve konumda bulunan sanal makineleri gösterecek şekilde filtrelenmiştir. Sanal makinelerinizde üçten fazla kaynak grubu varsa, ilk üç kaynak grubu seçilir.

5. Mevcut bir Log Analytics çalışma alanı ve Otomasyon hesabı varsayılan olarak seçilidir. Farklı bir Log Analytics çalışma alanı ve Otomasyon hesabı kullanmak istiyorsanız özel yapılandırma sayfasından seçmek için **özel** ' e tıklayın. Bir Log Analytics çalışma alanı seçtiğinizde, bir Otomasyon hesabıyla bağlanıp bağlanmadığını belirlemek için bir onay yapılır. Bağlı bir Otomasyon hesabı bulunursa, aşağıdaki ekranı görürsünüz. İşiniz bittiğinde **Tamam**' a tıklayın.

    ![Çalışma alanını ve hesabı seçin](media/automation-onboard-solutions-from-browse/selectworkspaceandaccount.png)

6. Seçilen çalışma alanı bir Otomasyon hesabına bağlı değilse, aşağıdaki ekranı görürsünüz. Bir Otomasyon hesabı seçin ve tamamlandığında **Tamam** ' a tıklayın.

    ![Çalışma alanı yok](media/automation-onboard-solutions-from-browse/no-workspace.png)

7. Etkinleştirmek istemediğiniz herhangi bir sanal makinenin yanındaki onay kutusunun işaretini kaldırın. Etkinleştirimeyen VM 'Lerin zaten seçimi kaldırılmış.

8. Seçtiğiniz özelliği etkinleştirmek için **Etkinleştir** ' e tıklayın. Kurulumun tamamlanabilmesi 15 dakika sürer.

## <a name="next-steps"></a>Sonraki adımlar

* VM 'Ler için Güncelleştirme Yönetimi kullanmak için bkz. [Azure VM 'leriniz için güncelleştirmeleri ve düzeltme eklerini yönetme](automation-tutorial-update-management.md).
* Genel Güncelleştirme Yönetimi hatalarıyla ilgili sorunları gidermek için bkz. [güncelleştirme yönetimi sorunlarını giderme](troubleshoot/update-management.md).
* Windows Update Aracısı sorunlarını gidermek için bkz. [Windows Update Aracısı sorunlarını giderme](troubleshoot/update-agent-issues.md).
* Linux Güncelleştirme Aracısı sorunlarını gidermek için bkz.[Linux Güncelleştirme Aracısı sorunlarını giderme](troubleshoot/update-agent-issues-linux.md).
