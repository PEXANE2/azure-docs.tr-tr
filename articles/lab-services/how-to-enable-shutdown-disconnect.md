---
title: Azure Lab Services ' de laboratuvar için sanal makinelerin otomatik olarak kapatılmasını yapılandırın
description: Uzak Masaüstü bağlantısı kesildiğinde sanal makinelerin otomatik olarak kapatılmasını etkinleştirmeyi veya devre dışı bırakmayı öğrenin.
ms.topic: article
ms.date: 10/01/2020
ms.openlocfilehash: 67b51941207fdd4913df9a92362959bbd468d336
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "91649890"
---
# <a name="configure-automatic-shutdown-of-vms-for-a-lab"></a>Laboratuvar için sanal makinelerin otomatik olarak kapatılmasını yapılandırma

Bu makalede, bir laboratuvar için sanal makinelerin otomatik olarak kapatılmasını nasıl yapılandırabileceğiniz gösterilmektedir.

Sanal makineler etkin bir şekilde kullanılmadığınızda, ek maliyetleri önceden engellemek için birkaç otomatik kapalı maliyet denetimi özelliği etkinleştirebilirsiniz. Aşağıdaki üç otomatik kapatılma ve bağlantı kesme özelliğinin birleşimi, kullanıcıların sanal makinelerini yanlışlıkla bırakması için gereken durumların çoğunu yakalar:
 
* İşletim sisteminin boşta kaldığı sanal makinelerle kullanıcıların bağlantısını otomatik olarak keser.
* Kullanıcıların bağlantısı kesildiğinde sanal makineleri otomatik olarak kapat.
* Başlatılan ancak kullanıcılar bağlanmadığında sanal makineleri otomatik olarak kapat.

Otomatik kapatılma [ayarları ile maliyet denetimini en üst düzeye çıkar](cost-management-guide.md#automatic-shutdown-settings-for-cost-control) bölümünde otomatik kapatılma özellikleri hakkında daha fazla ayrıntı inceleyin.

Laboratuvar hesabı Yöneticisi, laboratuvarları oluşturduğunuz laboratuvar hesabı için bu ayarı yapılandırabilir. Daha fazla bilgi için bkz. [Laboratuvar hesabı Için sanal makinelerin otomatik olarak kapatılmasını yapılandırma](how-to-configure-lab-accounts.md). Laboratuvar sahibi olarak, laboratuvar oluştururken veya laboratuvar oluşturulduktan sonra ayarı geçersiz kılabilirsiniz. 

## <a name="configure-for-the-lab-level"></a>Laboratuvar düzeyi için yapılandırma

[Azure Lab Services](https://labs.azure.com/)otomatik kapatılma ayarını yapılandırabilirsiniz.

* Laboratuvar oluştururken ( **Laboratuvar ilkelerinde**) veya
* Laboratuvar oluşturulduktan sonra ( **Ayarlar**halinde)

> [!div class="mx-imgBorder"]
> ![Laboratuvar oluşturma sırasında yapılandırma](./media/how-to-enable-shutdown-disconnect/configure-lab-creation.png)

Otomatik [kapanmayla ilgili ayrıntıları otomatik kapatmalar ayarları Ile en yüksek maliyetli](cost-management-guide.md#automatic-shutdown-settings-for-cost-control) bir şekilde gözden geçirmeyi unutmayın.

> [!WARNING]
> Bir sanal makineye bir RDP oturumunun bağlantısını kesmeden önce Linux veya Windows işletim sistemini (OS) kapatırsanız, oto kapatma özelliği düzgün çalışmaz.  
## <a name="next-steps"></a>Sonraki adımlar

[Sınıf laboratuvarları panosu](use-dashboard.md)
