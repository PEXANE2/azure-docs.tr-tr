---
title: Azure Veri Kutusu Ağ Geçidi güvenliği | Microsoft Dokümanlar
description: Azure Veri Kutusu Ağ Geçidi sanal cihazınızı, hizmetinizi ve verilerinizi şirket içinde ve bulutta koruyan güvenlik ve gizlilik özelliklerini açıklar.
services: databox
author: alkohli
ms.service: databox
ms.subservice: gateway
ms.topic: article
ms.date: 08/21/2019
ms.author: alkohli
ms.openlocfilehash: 89f981fcda8f40daff49ebdf796b896d90ce1754
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "69900611"
---
# <a name="azure-data-box-gateway-security-and-data-protection"></a>Azure Veri Kutusu Ağ Geçidi güvenliği ve veri koruması

Güvenlik, yeni bir teknolojiyi benimserken, özellikle de teknoloji gizli veya özel verilerle kullanılıyorsa, önemli bir sorundur. Azure Veri Kutusu Ağ Geçidi, verilerinizi yalnızca yetkili kuruluşların verilerinizi görüntüleyebilmesini, değiştirebilmesini veya silebilmesini sağlamanıza yardımcı olur.

Bu makalede, çözüm bileşenlerinin her birinin ve bunların depolanan verilerinin korunmasına yardımcı olan Azure Veri Kutusu Ağ Geçidi güvenlik özellikleri açıklanmaktadır.

Veri Kutusu Ağ Geçidi çözümü, birbiriyle etkileşim edebilen dört ana bileşenden oluşur:

- **Veri Kutusu Ağ Geçidi hizmeti, Azure'da barındırılan.** Aygıt siparişini oluşturmak, aygıtı yapılandırmak ve ardından tamamlama sırasını izlemek için kullandığınız yönetim kaynağı.
- **Veri Kutusu Ağ Geçidi aygıtı.** Sağladığınız sistemin hipervizöründe sağladığınız sanal aygıt. Bu sanal cihaz, şirket içi verilerinizi Azure'a almak için kullanılır.
- **Aygıta bağlı istemciler/ana bilgisayarlar.** Altyapınızdaki istemciler, Veri Kutusu Ağ Geçidi aygıtına bağlanır ve korunması gereken verileri içerir.
- **Bulut depolama.** Verilerin depolandığı Azure bulut platformundaki konum. Bu konum genellikle oluşturduğunuz Veri Kutusu Ağ Geçidi kaynağına bağlı depolama hesabıdır.


## <a name="data-box-gateway-service-protection"></a>Veri Kutusu Ağ Geçidi hizmet koruması

Veri Kutusu Ağ Geçidi hizmeti, Azure'da barındırılan bir yönetim hizmetidir. Hizmet, aygıtı yapılandırmak ve yönetmek için kullanılır.

[!INCLUDE [data-box-edge-gateway-service-protection](../../includes/data-box-edge-gateway-service-protection.md)]

## <a name="data-box-gateway-device-protection"></a>Veri Kutusu Ağ Geçidi aygıt koruması

Veri Kutusu Ağ Geçidi aygıtı, sağladığınız şirket içi sistemin hipervizöründe bulunan sanal bir aygıttır. Aygıt Azure'a veri gönderilmesine yardımcı olur. Cihazınız:

- Veri Kutusu Kenarı/Veri Kutusu Ağ Geçidi hizmetine erişmek için bir etkinleştirme anahtarına ihtiyaç duyar.
- Her zaman bir aygıt parolası ile korunur.
<!---  secure boot enabled.
- Runs Windows Defender Device Guard. Device Guard allows you to run only trusted applications that you define in your code integrity policies.-->

### <a name="protect-the-device-via-activation-key"></a>Etkinleştirme tuşu ile cihazı koruyun

Azure aboneliğinizde oluşturduğunuz Veri Kutusu Ağ Geçidi hizmetine yalnızca yetkili bir Veri Kutusu Ağ Geçidi aygıtı katılabilir. Bir aygıtı yetkilendirmek için, aygıtı Veri Kutusu Ağ Geçidi hizmetiyle etkinleştirmek için bir etkinleştirme anahtarı kullanmanız gerekir.

[!INCLUDE [data-box-edge-gateway-activation-key](../../includes/data-box-edge-gateway-activation-key.md)]

Daha fazla bilgi için [bkz.](data-box-gateway-deploy-prep.md#get-the-activation-key)

### <a name="protect-the-device-via-password"></a>Cihazı parola yla koruyun

Parolalar, verilerinize yalnızca yetkili kullanıcıların erişmesini sağlar. Veri Kutusu Ağ Geçidi aygıtları kilitli bir durumda önyükleme.

Şunları yapabilirsiniz:

- Tarayıcı aracılığıyla aygıtın yerel web ui bağlantısına bağlanın ve ardından aygıtta oturum açmanın bir parolasını sağlayın.
- Cihazın PowerShell arayüzüne HTTP üzerinden uzaktan bağlanın. Uzaktan yönetim varsayılan olarak açık. Daha sonra aygıtta oturum açabilmek için aygıt parolasını sağlayabilirsiniz. Daha fazla bilgi için bkz: [Veri Kutusu Ağ Geçidi aygıtınıza uzaktan bağlanın.](data-box-gateway-connect-powershell-interface.md#connect-to-the-powershell-interface)

[!INCLUDE [data-box-edge-gateway-password-best-practices](../../includes/data-box-edge-gateway-password-best-practices.md)]
- [Parolayı değiştirmek](data-box-gateway-manage-access-power-connectivity-mode.md#manage-device-access)için yerel web ui'sini kullanın. Parolayı değiştirirseniz, oturum açmada sorun yaşamaması için tüm uzaktan erişim kullanıcılarına bildirimde bulunun.


## <a name="protect-your-data"></a>Verilerinizi koruma

Bu bölümde, aktarım ve depolanan verileri koruyan Veri Kutusu Ağ Geçidi güvenlik özellikleri açıklanmaktadır.

### <a name="protect-data-at-rest"></a>Bekleyen verileri koruma

[!INCLUDE [data-box-edge-gateway-data-rest](../../includes/data-box-edge-gateway-data-rest.md)]

### <a name="protect-data-in-flight"></a>Uçuştaki verileri koruyun

[!INCLUDE [data-box-edge-gateway-data-flight](../../includes/data-box-edge-gateway-data-flight.md)]

### <a name="protect-data-via-storage-accounts"></a>Depolama hesapları aracılığıyla verileri koruma

[!INCLUDE [data-box-edge-gateway-data-storage-accounts](../../includes/data-box-edge-gateway-protect-data-storage-accounts.md)]
- Depolama hesabınızı yetkisiz kullanıcılardan korumaya yardımcı olmak için [depolama hesabınızın anahtarlarını](data-box-gateway-manage-shares.md#sync-storage-keys) düzenli olarak döndürün ve senkronize edin.

## <a name="manage-personal-information"></a>Kişisel bilgileri yönetme

Veri Kutusu Ağ Geçidi hizmeti aşağıdaki senaryolarda kişisel bilgileri toplar:

[!INCLUDE [data-box-edge-gateway-manage-personal-data](../../includes/data-box-edge-gateway-manage-personal-data.md)]

Bir paylaşıma erişebilen veya silebilen kullanıcıların listesini görüntülemek [için, Veri Kutusu Ağ Geçidi'ndeki paylaşımları yönet'teki](data-box-gateway-manage-shares.md)adımları izleyin.

Daha fazla bilgi için [Güven Merkezi'ndeki](https://www.microsoft.com/trustcenter)Microsoft gizlilik ilkesini inceleyin.

## <a name="next-steps"></a>Sonraki adımlar

[Veri Kutusu Ağ Geçidi cihazınızı dağıtma](data-box-gateway-deploy-prep.md)
