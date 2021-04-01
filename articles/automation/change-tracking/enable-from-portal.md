---
title: Azure portal Azure Otomasyonu Değişiklik İzleme ve envanterini etkinleştirin
description: Bu makalede, Azure portal Değişiklik İzleme ve envanter özelliğinin nasıl etkinleştirileceği açıklanır.
services: automation
ms.subservice: change-inventory-management
ms.date: 10/14/2020
ms.topic: conceptual
ms.openlocfilehash: 63041e0b1b6e12c765299b12f28aa3637b6a6ccb
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2021
ms.locfileid: "99052796"
---
# <a name="enable-change-tracking-and-inventory-from-azure-portal"></a>Azure portal Değişiklik İzleme ve stoku etkinleştir

Bu makalede, Azure portal bir veya daha fazla Azure sanal makinesi için [değişiklik izleme ve envanteri](overview.md) nasıl etkinleştirebileceğinizi açıklanmaktadır. Azure VM 'lerini ölçekli olarak etkinleştirmek için, Değişiklik İzleme ve envanterini kullanarak var olan bir VM 'yi etkinleştirmeniz gerekir.

VM 'lerinizi yönetmek için kullanabileceğiniz kaynak gruplarının sayısı [Kaynak Yöneticisi dağıtım limitleriyle](../../azure-resource-manager/templates/deploy-to-resource-group.md)sınırlıdır. Kaynak Yöneticisi dağıtımları dağıtım başına beş kaynak grubu ile sınırlıdır. Log Analytics çalışma alanını, Otomasyon hesabını ve ilgili kaynakları yapılandırmak için bu kaynak gruplarından ikisi ayrılmıştır. Bu, Değişiklik İzleme ve envantere göre yönetim için seçebileceğiniz üç kaynak grubu ile birlikte kalır. Bu sınır, bir Otomasyon özelliği tarafından yönetilebilen kaynak gruplarının sayısını değil, yalnızca eşzamanlı kurulum için geçerlidir.

> [!NOTE]
> Değişiklik İzleme ve envanteri etkinleştirirken, bir Log Analytics çalışma alanını ve bir Otomasyon hesabını bağlamak için yalnızca belirli bölgeler desteklenir. Desteklenen eşleme çiftlerinin bir listesi için bkz. [Otomasyon hesabı ve Log Analytics çalışma alanı Için bölge eşleme](../how-to/region-mappings.md).

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği. Henüz bir hesabınız yoksa [MSDN abone avantajlarınızı etkinleştirebilir](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) veya [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)için kaydolabilirsiniz.
* Makineleri yönetmek için [Otomasyon hesabı](../automation-security-overview.md).
* Bir [sanal makine](../../virtual-machines/windows/quick-create-portal.md).

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

Adresinden Azure 'da oturum açın https://portal.azure.com .

## <a name="enable-change-tracking-and-inventory"></a>Değişiklik İzlemeyi ve Sayımı Etkinleştirme

1. Azure portal, **sanal makineler**' e gidin.

2. Değişiklik İzleme ve envantere eklenecek VM 'Leri seçmek için onay kutularını kullanın. Tek seferde en fazla üç farklı kaynak grubu için makine ekleyebilirsiniz. Otomasyon hesabınızın konumu ne olduğuna bakılmaksızın, Azure VM 'Leri herhangi bir bölgede bulunabilir.

    ![VM 'lerin listesi](media/enable-from-portal/vmlist.png)

    > [!TIP]
    > Farklı aboneliklerden, konumlardan ve kaynak gruplarından VM 'Leri seçmek için filtre denetimlerini kullanın. Bir listedeki tüm sanal makineleri seçmek için üstteki onay kutusuna tıklayabilirsiniz.

3. **Yapılandırma yönetimi** altında **değişiklik izleme** veya **Stok** ' ı seçin.

4. Sanal makinelerin listesi, yalnızca aynı abonelikte ve konumda bulunan sanal makineleri gösterecek şekilde filtrelenmiştir. Sanal makinelerinizde üçten fazla kaynak grubu varsa, ilk üç kaynak grubu seçilir.

5. Mevcut bir Log Analytics çalışma alanı ve Otomasyon hesabı varsayılan olarak seçilidir. Farklı bir Log Analytics çalışma alanı ve Otomasyon hesabı kullanmak istiyorsanız özel yapılandırma sayfasından seçmek için **özel** ' e tıklayın. Bir Log Analytics çalışma alanı seçtiğinizde, bir Otomasyon hesabıyla bağlanıp bağlanmadığını belirlemek için bir onay yapılır. Bağlı bir Otomasyon hesabı bulunursa, aşağıdaki ekranı görürsünüz. İşiniz bittiğinde **Tamam**' a tıklayın.

    ![Çalışma alanını ve hesabı seçin](media/enable-from-portal/select-workspace-and-account.png)

6. Seçilen çalışma alanı bir Otomasyon hesabına bağlı değilse, aşağıdaki ekranı görürsünüz. Bir Otomasyon hesabı seçin ve tamamlandığında **Tamam** ' a tıklayın.

    ![Çalışma alanı yok](media/enable-from-portal/no-workspace.png)

7. Etkinleştirmek istemediğiniz herhangi bir sanal makinenin yanındaki onay kutusunun işaretini kaldırın. Etkinleştirimeyen VM 'Lerin zaten seçimi kaldırılmış.

8. Seçtiğiniz özelliği etkinleştirmek için **Etkinleştir** ' e tıklayın. Kurulumun tamamlanabilmesi 15 dakika sürer.

## <a name="next-steps"></a>Sonraki adımlar

* Özelliği ile çalışma hakkında ayrıntılı bilgi için bkz. [değişiklik izleme yönetme](manage-change-tracking.md) ve [envanteri yönetme](manage-inventory-vms.md).
* Özellikle ilgili sorunları gidermek için bkz. [değişiklik izleme ve envanter sorunlarını giderme](../troubleshoot/change-tracking.md).