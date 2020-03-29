---
title: Azure Veri Kutusu Kenarı güvenliği | Microsoft Dokümanlar
description: Azure Veri Kutusu Edge cihazınızı, hizmetinizi ve verilerinizi şirket içinde ve bulutta koruyan güvenlik ve gizlilik özelliklerini açıklar.
services: Data Box Edge
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: article
ms.date: 08/21/2019
ms.author: alkohli
ms.openlocfilehash: 375576dd4a7897c48474fd2af00a99084292d854
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "69970894"
---
# <a name="azure-data-box-edge-security-and-data-protection"></a>Azure Data Box Edge güvenliği ve veri koruması

Güvenlik, yeni bir teknolojiyi benimserken, özellikle de teknoloji gizli veya özel verilerle kullanılıyorsa, önemli bir sorundur. Azure Veri Kutusu Kenarı, verilerinizi yalnızca yetkili kuruluşların verilerinizi görüntüleyebilmesini, değiştirebilmesini veya silebilmesini sağlamanıza yardımcı olur.

Bu makalede, çözüm bileşenlerinin her birinin ve bunların depolanan verilerinin korunmasına yardımcı olan Veri Kutusu Kenarı güvenlik özellikleri açıklanmaktadır.

Azure Veri Kutusu Kenarı, birbiriyle etkileşim edebilen dört ana bileşenden oluşur:

- **Veri Kutusu Kenarı hizmeti, Azure'da barındırılan.** Aygıt siparişini oluşturmak, aygıtı yapılandırmak ve ardından tamamlama sırasını izlemek için kullandığınız yönetim kaynağı.
- **Veri Kutusu Edge cihazı.** Şirket içi verilerinizi Azure'a aktarabilmeniz için size gönderilen aktarım aygıtı.
- **Aygıta bağlı istemciler/ana bilgisayarlar.** Altyapınızdaki veri kutusu kenar aygıtına bağlanan ve korunması gereken verileri içeren istemciler.
- **Bulut depolama.** Verilerin depolandığı Azure bulut platformundaki konum. Bu konum genellikle oluşturduğunuz Veri Kutusu Kenarı kaynağına bağlı depolama hesabıdır.

## <a name="data-box-edge-service-protection"></a>Veri Kutusu Kenarı hizmet koruması

Veri Kutusu Kenarı hizmeti, Azure'da barındırılan bir yönetim hizmetidir. Hizmet, aygıtı yapılandırmak ve yönetmek için kullanılır.

[!INCLUDE [data-box-edge-gateway-data-rest](../../includes/data-box-edge-gateway-service-protection.md)]

## <a name="data-box-edge-device-protection"></a>Veri Kutusu Kenarı cihaz koruması

Veri Kutusu Kenarı aygıtı, verilerinizi yerel olarak işleyip Azure'a göndererek dönüşüme yardımcı olan şirket içi bir aygıttır. Cihazınız:

- Veri Kutusu Kenarı hizmetine erişmek için bir etkinleştirme anahtarına ihtiyaç duyar.
- Her zaman bir aygıt parolası ile korunur.
- Kilitli bir cihazdır. BMC ve BIOS cihazı parola korumalıdır. BIOS sınırlı kullanıcı erişimi ile korunmaktadır.
- Güvenli önyükleme etkin.
- Windows Defender Aygıt Koruması çalıştırın. Aygıt Koruması, yalnızca kod bütünlüğü ilkelerinizde tanımladığınız güvenilir uygulamaları çalıştırmanızı sağlar.

### <a name="protect-the-device-via-activation-key"></a>Etkinleştirme tuşu ile cihazı koruyun

Azure aboneliğinizde oluşturduğunuz Veri Kutusu Kenarı hizmetine yalnızca yetkili bir Veri Kutusu Kenarı aygıtı katılabilir. Bir aygıtı yetkilendirmek için, aygıtı Veri Kutusu Kenarı hizmetiyle etkinleştirmek için bir etkinleştirme anahtarı kullanmanız gerekir.

[!INCLUDE [data-box-edge-gateway-data-rest](../../includes/data-box-edge-gateway-activation-key.md)]

Daha fazla bilgi için [bkz.](data-box-edge-deploy-prep.md#get-the-activation-key)

### <a name="protect-the-device-via-password"></a>Cihazı parola yla koruyun

Parolalar, verilerinize yalnızca yetkili kullanıcıların erişmesini sağlar. Veri Kutusu Kenarı aygıtları kilitli bir durumda önyükleme.

Şunları yapabilirsiniz:

- Tarayıcı aracılığıyla aygıtın yerel web ui bağlantısına bağlanın ve ardından aygıtta oturum açmanın bir parolasını sağlayın.
- HTTP üzerinden aygıt PowerShell arabirimine uzaktan bağlanın. Uzaktan yönetim varsayılan olarak açık. Daha sonra aygıtta oturum açabilmek için aygıt parolasını sağlayabilirsiniz. Daha fazla bilgi için bkz: [Veri Kutusu Kenarı aygıtınıza uzaktan bağlanın.](data-box-edge-connect-powershell-interface.md#connect-to-the-powershell-interface)

[!INCLUDE [data-box-edge-gateway-data-rest](../../includes/data-box-edge-gateway-password-best-practices.md)]
- [Parolayı değiştirmek](data-box-edge-manage-access-power-connectivity-mode.md#manage-device-access)için yerel web ui'sini kullanın. Parolayı değiştirirseniz, oturum açmada sorun yaşamaması için tüm uzaktan erişim kullanıcılarına bildirimde bulunun.

## <a name="protect-your-data"></a>Verilerinizi koruma

Bu bölümde, aktarım ve depolanan verileri koruyan Veri Kutusu Kenarı güvenlik özellikleri açıklanmaktadır.

### <a name="protect-data-at-rest"></a>Bekleyen verileri koruma

[!INCLUDE [data-box-edge-gateway-data-rest](../../includes/data-box-edge-gateway-data-rest.md)]
- BitLocker XTS-AES 256-bit şifreleme yerel verileri korumak için kullanılır.


### <a name="protect-data-in-flight"></a>Uçuştaki verileri koruyun

[!INCLUDE [data-box-edge-gateway-data-rest](../../includes/data-box-edge-gateway-data-flight.md)]

### <a name="protect-data-via-storage-accounts"></a>Depolama hesapları aracılığıyla verileri koruma

[!INCLUDE [data-box-edge-gateway-data-rest](../../includes/data-box-edge-gateway-protect-data-storage-accounts.md)]
- Depolama hesabınızı yetkisiz kullanıcılardan korumaya yardımcı olmak için [depolama hesabınızın anahtarlarını](data-box-edge-manage-shares.md#sync-storage-keys) düzenli olarak döndürün ve senkronize edin.

## <a name="manage-personal-information"></a>Kişisel bilgileri yönetme

Data Box Edge hizmeti aşağıdaki senaryolarda kişisel bilgileri toplar:

[!INCLUDE [data-box-edge-gateway-data-rest](../../includes/data-box-edge-gateway-manage-personal-data.md)]

Bir paylaşıma erişebilen veya silebilen kullanıcıların listesini görüntülemek için, [Veri Kutusu Kenarı'ndaki paylaşımları yönet'teki](data-box-edge-manage-shares.md)adımları izleyin.

Daha fazla bilgi için [Güven Merkezi'ndeki](https://www.microsoft.com/trustcenter)Microsoft gizlilik ilkesini inceleyin.

## <a name="next-steps"></a>Sonraki adımlar

[Veri Kutusu Edge cihazınızı dağıtma](data-box-edge-deploy-prep.md)
