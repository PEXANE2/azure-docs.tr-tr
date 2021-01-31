---
title: RD Ağ Geçidi rolünü dağıtma Windows sanal masaüstü-Azure
description: Windows sanal masaüstü 'nde RD Ağ Geçidi rolünü dağıtma.
author: Heidilohr
ms.topic: how-to
ms.date: 01/30/2021
ms.author: helohr
manager: lizross
ms.openlocfilehash: 71bd7d38727d99c05a15c54e5141c613960d9050
ms.sourcegitcommit: 54e1d4cdff28c2fd88eca949c2190da1b09dca91
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/31/2021
ms.locfileid: "99220828"
---
# <a name="deploy-the-rd-gateway-role-in-windows-virtual-desktop-preview"></a>Windows sanal masaüstü 'nde RD Ağ Geçidi rolünü dağıtma (Önizleme)

> [!IMPORTANT]
> Bu özellik şu anda genel önizleme aşamasındadır.
> Bu önizleme sürümü, bir hizmet düzeyi sözleşmesi olmadan sağlanır ve bunu üretim iş yükleri için kullanmanızı önermiyoruz. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir.
> Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Bu makalede, ortamınızdaki Uzak Masaüstü Ağ Geçidi sunucuları dağıtmak için RD Ağ Geçidi rolün (Önizleme) nasıl kullanılacağı açıklanır. Şirket içi, bulut tabanlı ya da karma ortam oluşturma yönteminize bağlı olarak, sunucu rollerini fiziksel makinelere veya sanal makinelere yükleyebilirsiniz.

## <a name="install-the-rd-gateway-role"></a>RD Ağ Geçidi rolünü yükler

1. Hedef sunucuda yönetici kimlik bilgileriyle oturum açın.

2. **Sunucu Yöneticisi**, **Yönet**' i seçin ve ardından **rol ve Özellik Ekle**' yi seçin. **Rol ve özellik ekleme** yükleyicisi açılır.

3. **Başlamadan önce**' de, **İleri**' yi seçin.

4. **Yükleme türünü seçin** bölümünde **rol tabanlı veya özellik tabanlı yükleme**' yi seçin ve ardından **İleri**' yi seçin.

5. **Hedef sunucu Seç** için **Sunucu havuzundan bir sunucu seçin**' i seçin. **Sunucu havuzu** için yerel bilgisayarınızın adını seçin. İşiniz bittiğinde **İleri**' yi seçin.

6. **Sunucu rollerini seçin**  >  **Roller**' de **Uzak Masaüstü Hizmetleri**' yi seçin. İşiniz bittiğinde **İleri**' yi seçin.

7. **Uzak Masaüstü Hizmetleri**, ileri ' yi seçin **.**

8. **Rol Hizmetleri Seç** için yalnızca gerekli özellikleri eklemeniz istendiğinde **Uzak Masaüstü Ağ Geçidi** seçin, **Özellik Ekle**' yi seçin. İşiniz bittiğinde **İleri**' yi seçin.

9. **Ağ İlkesi ve erişim Hizmetleri** için **İleri**' yi seçin.

10. **Web sunucusu rolü (IIS)** için **İleri**' yi seçin.

11. **Rol Hizmetleri Seç** için **İleri**' yi seçin.

12. **Yükleme seçimlerini Onayla** Için, **yükleme**' yi seçin. Yükleme işlemi devam ederken yükleyiciyi kapatmayın.

## <a name="configure-rd-gateway-role"></a>RD Ağ Geçidi rolünü Yapılandır

RD Ağ Geçidi rolü yüklendikten sonra, onu yapılandırmanız gerekecektir.

RD Ağ Geçidi rolünü yapılandırmak için:

1. **Sunucu Yöneticisi** açın ve **Uzak Masaüstü Hizmetleri**' ı seçin.

2. **Sunucular**' a gidin, sunucunuzun adına sağ tıklayın ve ardından **RD Ağ Geçidi Yöneticisi**' yi seçin.

3. RD Ağ Geçidi Yöneticisi, ağ geçidinizin adına sağ tıklayın ve ardından **Özellikler**' i seçin.

4. **SSL sertifikası** sekmesini açın, **sertifikayı RD Ağ Geçidi** baloncuya Aktar ' ı seçin **ve ardından sertifikayı içeri aktar...** seçeneğini belirleyin.

5. PFX dosyanızın adını seçin ve **Aç**' ı seçin.

6. İstendiğinde PFX dosyası için parolayı girin.

7. Sertifikayı ve özel anahtarını içeri aktardıktan sonra, ekran sertifikanın anahtar özniteliklerini göstermelidir.

>[!NOTE]
>RD Ağ Geçidi rolün ortak olması gerektiğinden, genel olarak verilen bir sertifika kullanmanızı öneririz. Özel olarak verilen bir sertifika kullanıyorsanız, sertifikanın güven zinciri ile tüm istemcileri önceden yapılandırmayı unutmayın.

## <a name="next-steps"></a>Sonraki adımlar

RD Ağ Geçidi rolünüze yüksek kullanılabilirlik eklemek istiyorsanız, bkz. [RD Web ve ağ geçidi Web ön öğesine yüksek kullanılabilirlik ekleme](/windows-server/remote/remote-desktop-services/rds-rdweb-gateway-ha).