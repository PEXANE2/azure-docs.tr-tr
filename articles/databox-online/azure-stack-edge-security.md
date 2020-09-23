---
title: Azure Stack Edge Pro güvenliği | Microsoft Docs
description: Azure Stack Edge Pro cihazınızı, hizmetinizi ve şirket içi ve buluttaki verilerinizi koruyan güvenlik ve gizlilik özelliklerini açıklar.
services: Data Box Edge
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: article
ms.date: 08/21/2019
ms.author: alkohli
ms.openlocfilehash: b5c5143c7bc92487aeabc3cdc1afea5061466027
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/22/2020
ms.locfileid: "90903154"
---
# <a name="azure-stack-edge-pro-security-and-data-protection"></a>Azure Stack Edge Pro güvenlik ve veri koruması

Güvenlik, özellikle de teknoloji gizli veya özel verilerle kullanılıyorsa, yeni bir teknolojiyi benimseiyorsanız önemli bir konudur. Azure Stack Edge Pro, yalnızca yetkili varlıkların verilerinizi görüntülemesini, değiştirmesini veya silmesini sağlamanıza yardımcı olur.

Bu makalede, her çözüm bileşeninin ve bunlarda depolanan verilerin korunmasına yardımcı olan Azure Stack Edge Pro güvenlik özellikleri açıklanmaktadır.

Azure Stack Edge Pro, birbirleriyle etkileşime geçen dört ana bileşenden oluşur:

- **Azure 'da barındırılan Azure Stack Edge Service**. Cihaz sırasını oluşturmak için kullandığınız yönetim kaynağı, cihazı yapılandırın ve sonra tamamlanma sırasını izleyin.
- **Azure Stack Edge Pro cihazı**. Size gönderilen aktarım cihazı, şirket içi verilerinizi Azure 'a aktarabilirsiniz.
- **Cihaza bağlı istemciler/konaklar**. Altyapınızda Azure Stack Edge Pro cihazına bağlanan ve korunması gereken verileri içeren istemciler.
- **Bulut depolama alanı**. Azure bulut platformunda verilerin depolandığı konum. Bu konum genellikle oluşturduğunuz Azure Stack Edge kaynağına bağlı depolama hesabıdır.

## <a name="azure-stack-edge-service-protection"></a>Azure Stack Edge hizmeti koruması

Azure Stack Edge hizmeti, Azure 'da barındırılan bir yönetim hizmetidir. Hizmet, cihazı yapılandırmak ve yönetmek için kullanılır.

[!INCLUDE [data-box-edge-gateway-data-rest](../../includes/data-box-edge-gateway-service-protection.md)]

## <a name="azure-stack-edge-pro-device-protection"></a>Azure Stack Edge Pro cihaz koruması

Azure Stack Edge Pro cihazı, yerel olarak işleyerek ve sonra Azure 'a göndererek verilerinizi dönüştürmenizi sağlayan şirket içi bir cihazdır. Cihazınız:

- Azure Stack Edge hizmetine erişmek için bir etkinleştirme anahtarına ihtiyaç duyuyor.
- Her zaman bir cihaz parolasıyla korunur.
- Kilitli bir cihazdır. BMC ve BIOS cihaz parola korumalıdır. BIOS, sınırlı Kullanıcı erişimi tarafından korunur.
- Güvenli önyükleme etkin.
- Windows Defender Device Guard 'ı çalıştırır. Device Guard, yalnızca kod bütünlüğü ilkeleriniz içinde tanımladığınız güvenilen uygulamaları çalıştırmanızı sağlar.

### <a name="protect-the-device-via-activation-key"></a>Etkinleştirme anahtarı aracılığıyla cihazı koruma

Yalnızca yetkili bir Azure Stack Edge Pro cihazının Azure aboneliğinizde oluşturduğunuz Azure Stack Edge hizmetine katılmasına izin verilir. Bir cihazı yetkilendirmek için, Azure Stack Edge hizmeti ile cihazı etkinleştirmek üzere bir etkinleştirme anahtarı kullanmanız gerekir.

[!INCLUDE [data-box-edge-gateway-data-rest](../../includes/data-box-edge-gateway-activation-key.md)]

Daha fazla bilgi için bkz. [etkinleştirme anahtarı edinme](azure-stack-edge-deploy-prep.md#get-the-activation-key).

### <a name="protect-the-device-via-password"></a>Cihazı parola ile koruma

Parolalar yalnızca yetkili kullanıcıların verilerinize erişebilmesini güvence altına alabilir. Azure Stack Edge Pro cihazlarının ön yüklemesi kilitli durumda.

Seçenekleriniz şunlardır:

- Cihazın yerel Web Kullanıcı arabirimine bir tarayıcı aracılığıyla bağlanın ve ardından cihazda oturum açmak için bir parola sağlayın.
- HTTP üzerinden cihaz PowerShell arabirimine uzaktan bağlanın. Uzaktan yönetim varsayılan olarak açıktır. Daha sonra cihazda oturum açmak için cihaz parolasını sağlayabilirsiniz. Daha fazla bilgi için bkz. [Azure Stack Edge Pro cihazınıza uzaktan bağlanma](azure-stack-edge-connect-powershell-interface.md#connect-to-the-powershell-interface).

[!INCLUDE [data-box-edge-gateway-data-rest](../../includes/data-box-edge-gateway-password-best-practices.md)]
- [Parolayı değiştirmek](azure-stack-edge-manage-access-power-connectivity-mode.md#manage-device-access)için yerel Web Kullanıcı arabirimini kullanın. Parolayı değiştirirseniz, tüm uzaktan erişim kullanıcılarını, oturum açmada sorun yaşamadıklarından haberdar olduğunuzdan emin olun.

## <a name="protect-your-data"></a>Verilerinizi koruma

Bu bölümde, geçişi ve depolanan verileri koruyan Azure Stack Edge Pro güvenlik özellikleri açıklanmaktadır.

### <a name="protect-data-at-rest"></a>Bekleyen verileri koruma

[!INCLUDE [data-box-edge-gateway-data-rest](../../includes/data-box-edge-gateway-data-rest.md)]
- BitLocker XTS-AES 256 bit şifreleme, yerel verileri korumak için kullanılır.


### <a name="protect-data-in-flight"></a>Uçuş sırasında verileri koruma

[!INCLUDE [data-box-edge-gateway-data-rest](../../includes/data-box-edge-gateway-data-flight.md)]

### <a name="protect-data-via-storage-accounts"></a>Depolama hesapları aracılığıyla verileri koruma

[!INCLUDE [data-box-edge-gateway-data-rest](../../includes/data-box-edge-gateway-protect-data-storage-accounts.md)]
- Depolama hesabınızı yetkisiz kullanıcılara karşı korumaya yardımcı olmak için [depolama hesabı anahtarlarınızı](azure-stack-edge-manage-shares.md#sync-storage-keys) düzenli olarak döndürün ve eşitleyin.

## <a name="manage-personal-information"></a>Kişisel bilgileri yönetme

Azure Stack Edge hizmeti, aşağıdaki senaryolarda kişisel bilgileri toplar:

[!INCLUDE [data-box-edge-gateway-data-rest](../../includes/data-box-edge-gateway-manage-personal-data.md)]

Bir paylaşıma erişebilen veya silebilen kullanıcıların listesini görüntülemek için [Azure Stack Edge Pro 'da paylaşımları yönetme](azure-stack-edge-manage-shares.md)bölümündeki adımları izleyin.

Daha fazla bilgi için [Güven Merkezi](https://www.microsoft.com/trustcenter)'nde Microsoft gizlilik ilkesini gözden geçirin.

## <a name="next-steps"></a>Sonraki adımlar

[Azure Stack Edge Pro cihazınızı dağıtma](azure-stack-edge-deploy-prep.md)
