---
title: Azure Lab Services ' de sanal makinelerin otomatik olarak kapatılmasını yapılandırma
description: Bu makalede laboratuvar hesabındaki sanal makinelerin otomatik olarak kapatılmasını yapılandırma açıklanmaktadır.
ms.topic: article
ms.date: 06/26/2020
ms.openlocfilehash: 783e3b310b3ad06f637453f0e1b11f6a78beec3a
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85445823"
---
# <a name="configure-automatic-shutdown-of-vms-on-disconnect-setting-for-a-lab-account"></a>Laboratuvar hesabı için bağlantı kesildiğinde VM 'lerin otomatik olarak kapatılmasını yapılandırma
Uzak Masaüstü bağlantısı kesildikten sonra Windows laboratuvar VM 'lerinin (şablon veya öğrenci) otomatik olarak kapatılmasını etkinleştirebilir veya devre dışı bırakabilirsiniz. Laboratuvar Hizmetleri 'nin otomatik olarak kapanmadan önce kullanıcının yeniden bağlanmasına ne kadar süre beklemesi gerektiğini de belirtebilirsiniz.

## <a name="enable-automatic-shutdown"></a>Otomatik kapanışı etkinleştir

1. **Laboratuvar hesabı** sayfasında, soldaki menüden **Labs ayarları** ' nı seçin.
2. **Kullanıcıların bağlantısı kesildiğinde sanal makineleri otomatik olarak kapat** seçeneğini belirleyin.
3. Laboratuvar Hizmetleri 'nin VM 'Leri otomatik olarak kapatmadan önce kullanıcının yeniden bağlanmasına ne kadar süre beklemesi gerektiğini belirtin.

    ![Laboratuvar hesabındaki otomatik kapatılma ayarı](./media/how-to-configure-lab-accounts/automatic-shutdown-vm-disconnect.png)

    Bu ayar, laboratuvar hesabında oluşturulan tüm laboratuvarlara uygulanır. Laboratuvar Oluşturucu (eğiticator) Bu ayarı laboratuvar düzeyinde geçersiz kılabilir. Laboratuvar hesabındaki bu ayarda yapılan değişiklik yalnızca değişiklik yapıldıktan sonra oluşturulan laboratuvarları etkiler.

    Bu ayarı devre dışı bırakmak için, bu sayfadaki **kullanıcıların bağlantısı kesildiğinde sanal makineleri otomatik olarak kapatır** onay kutusunun işaretini kaldırın. 

## <a name="next-steps"></a>Sonraki adımlar
Laboratuvar sahibi bu ayarı laboratuvar düzeyinde nasıl yapılandırabileceğinizi veya geçersiz kılacağınızı öğrenmek için, [Bu makaleye](how-to-enable-shutdown-disconnect.md) bakın.
