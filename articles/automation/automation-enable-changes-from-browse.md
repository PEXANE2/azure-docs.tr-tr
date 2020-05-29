---
title: Azure portal Azure Otomasyonu Değişiklik İzleme ve envanterini etkinleştirin
description: Bu makalede, Azure portal Değişiklik İzleme ve envanter özelliğinin nasıl etkinleştirileceği açıklanır.
services: automation
ms.date: 04/11/2019
ms.topic: article
ms.custom: mvc
ms.openlocfilehash: f84a609ea2821546c4001b98ad11495305ac101a
ms.sourcegitcommit: 1692e86772217fcd36d34914e4fb4868d145687b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/29/2020
ms.locfileid: "84171081"
---
# <a name="enable-change-tracking-and-inventory-from-azure-portal"></a>Azure portal Değişiklik İzleme ve stoku etkinleştir

Bu makalede, Azure portal göz atarak VM 'Ler için [değişiklik izleme ve stok](change-tracking.md) özelliğini nasıl etkinleştirebileceğinizi açıklanmaktadır. Azure VM 'lerini ölçekli olarak etkinleştirmek için, Değişiklik İzleme ve envanterini kullanarak var olan bir VM 'yi etkinleştirmeniz gerekir. 

VM 'lerinizi yönetmek için kullanabileceğiniz kaynak gruplarının sayısı [Kaynak Yöneticisi dağıtım limitleriyle](../azure-resource-manager/templates/cross-resource-group-deployment.md)sınırlıdır. Güncelleştirme dağıtımlarıyla karıştırılmamalıdır Kaynak Yöneticisi dağıtımlar, dağıtım başına beş kaynak grubu ile sınırlıdır. Log Analytics çalışma alanını, Otomasyon hesabını ve ilgili kaynakları yapılandırmak için bu kaynak gruplarından ikisi ayrılmıştır. Bu, Değişiklik İzleme ve envantere göre yönetim için seçebileceğiniz üç kaynak grubu ile birlikte kalır. Bu sınır, bir Otomasyon özelliği tarafından yönetilebilen kaynak gruplarının sayısını değil, yalnızca eşzamanlı kurulum için geçerlidir.

> [!NOTE]
> Değişiklik İzleme ve envanteri etkinleştirirken, bir Log Analytics çalışma alanını ve bir Otomasyon hesabını bağlamak için yalnızca belirli bölgeler desteklenir. Desteklenen eşleme çiftlerinin bir listesi için bkz. [Otomasyon hesabı ve Log Analytics çalışma alanı Için bölge eşleme](how-to/region-mappings.md).

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği. Henüz bir hesabınız yoksa [MSDN abone avantajlarınızı etkinleştirebilir](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) veya [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)için kaydolabilirsiniz.
* Makineleri yönetmek için [Otomasyon hesabı](automation-offering-get-started.md).
* Bir [sanal makine](../virtual-machines/windows/quick-create-portal.md).

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

Adresinden Azure 'da oturum açın https://portal.azure.com .

## <a name="enable-change-tracking-and-inventory"></a>Değişiklik İzlemeyi ve Sayımı Etkinleştirme

1. Azure portal, **sanal makineler**' e gidin.

2. Değişiklik İzleme ve envantere eklenecek VM 'Leri seçmek için onay kutularını kullanın. Tek seferde en fazla üç farklı kaynak grubu için makine ekleyebilirsiniz. Otomasyon hesabınızın konumu ne olduğuna bakılmaksızın, Azure VM 'Leri herhangi bir bölgede bulunabilir.

    ![VM 'lerin listesi](media/automation-enable-changes-from-browse/vmlist.png)

    > [!TIP]
    > Farklı aboneliklerden, konumlardan ve kaynak gruplarından VM 'Leri seçmek için filtre denetimlerini kullanın. Bir listedeki tüm sanal makineleri seçmek için üstteki onay kutusuna tıklayabilirsiniz.

3. **Yapılandırma yönetimi**altında **değişiklik izleme** veya **Stok** ' ı seçin.

4. Sanal makinelerin listesi, yalnızca aynı abonelikte ve konumda bulunan sanal makineleri gösterecek şekilde filtrelenmiştir. Sanal makinelerinizde üçten fazla kaynak grubu varsa, ilk üç kaynak grubu seçilir.

5. Mevcut bir Log Analytics çalışma alanı ve Otomasyon hesabı varsayılan olarak seçilidir. Farklı bir Log Analytics çalışma alanı ve Otomasyon hesabı kullanmak istiyorsanız özel yapılandırma sayfasından seçmek için **özel** ' e tıklayın. Bir Log Analytics çalışma alanı seçtiğinizde, bir Otomasyon hesabıyla bağlanıp bağlanmadığını belirlemek için bir onay yapılır. Bağlı bir Otomasyon hesabı bulunursa, aşağıdaki ekranı görürsünüz. İşiniz bittiğinde **Tamam**' a tıklayın.

    ![Çalışma alanını ve hesabı seçin](media/automation-enable-changes-from-browse/selectworkspaceandaccount.png)

6. Seçilen çalışma alanı bir Otomasyon hesabına bağlı değilse, aşağıdaki ekranı görürsünüz. Bir Otomasyon hesabı seçin ve tamamlandığında **Tamam** ' a tıklayın.

    ![Çalışma alanı yok](media/automation-enable-changes-from-browse/no-workspace.png)

7. Etkinleştirmek istemediğiniz herhangi bir sanal makinenin yanındaki onay kutusunun işaretini kaldırın. Etkinleştirimeyen VM 'Lerin zaten seçimi kaldırılmış.

8. Seçtiğiniz özelliği etkinleştirmek için **Etkinleştir** ' e tıklayın. Kurulumun tamamlanabilmesi 15 dakika sürer.

## <a name="next-steps"></a>Sonraki adımlar

* Özelliği ile çalışma hakkında ayrıntılı bilgi için bkz. [Manage değişiklik izleme and Inventory](change-tracking-file-contents.md).
* Özellikle ilgili sorunları gidermek için bkz. [değişiklik izleme ve envanter sorunlarını giderme](troubleshoot/change-tracking.md).