---
title: Azure Lab Services ' de sanal makinelerin otomatik olarak kapatılmasını yapılandırma
description: Bu makalede laboratuvar hesabındaki sanal makinelerin otomatik olarak kapatılmasını yapılandırma açıklanmaktadır.
ms.topic: article
ms.date: 08/17/2020
ms.openlocfilehash: 9fd93d383ba6a2d57057a3b45f8b517c84990043
ms.sourcegitcommit: 02ca0f340a44b7e18acca1351c8e81f3cca4a370
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/19/2020
ms.locfileid: "88589817"
---
# <a name="configure-automatic-shutdown-of-vms-for-a-lab-account"></a>Laboratuvar hesabı için sanal makinelerin otomatik olarak kapatılmasını yapılandırın

Sanal makineler etkin bir şekilde kullanılmadığınızda, ek maliyetleri önceden engellemek için birkaç otomatik kapalı maliyet denetimi özelliği etkinleştirebilirsiniz. Aşağıdaki üç otomatik kapatılma ve bağlantı kesme özelliğinin birleşimi, kullanıcıların sanal makinelerini yanlışlıkla bırakması için gereken durumların çoğunu yakalar:
 
- İşletim sisteminin boşta kaldığı sanal makinelerle kullanıcıların bağlantısını otomatik olarak keser (yalnızca Windows).
- Kullanıcıların bağlantısı kesildiğinde sanal makineleri otomatik olarak kapat (Windows & Linux).
- Başlatılan ancak kullanıcılar bağlanmadığında sanal makineleri otomatik olarak kapat.

Otomatik kapatılma [ayarları ile maliyet denetimini en üst düzeye çıkar](cost-management-guide.md#maximize-cost-control-with-auto-shutdown-settings) bölümünde otomatik kapatılma özellikleri hakkında daha fazla ayrıntı inceleyin.

## <a name="enable-automatic-shutdown"></a>Otomatik kapanışı etkinleştir

1. [Azure Portal](https://portal.azure.com/) **Laboratuvar hesabı** sayfasına gidin.
1. Soldaki menüden **Labs ayarları** ' nı seçin.
1. Senaryonuz için uygun olan otomatik kapalı ayarları seçin.  

    > [!div class="mx-imgBorder"]
    > ![Laboratuvar hesabındaki otomatik kapatılma ayarı](./media/how-to-configure-lab-accounts/automatic-shutdown-vm-disconnect.png)
    
    Bu ayarlar, laboratuvar hesabında oluşturulan tüm laboratuvarlar için geçerlidir. Laboratuvar Oluşturucu (eğiticator) Bu ayarı laboratuvar düzeyinde geçersiz kılabilir. Laboratuvar hesabındaki bu ayarda yapılan değişiklik yalnızca değişiklik yapıldıktan sonra oluşturulan laboratuvarları etkiler.

    Bu ayarları devre dışı bırakmak için bu sayfadaki onay kutularını temizleyin. 

## <a name="next-steps"></a>Sonraki adımlar

Laboratuvar sahibi bu ayarı laboratuvar düzeyinde nasıl yapılandırabileceğinizi veya geçersiz kılacağınızı öğrenmek için bkz. [bağlantı kesildiğinde VM 'lerin otomatik kapatılmasını etkinleştirme](how-to-enable-shutdown-disconnect.md)
