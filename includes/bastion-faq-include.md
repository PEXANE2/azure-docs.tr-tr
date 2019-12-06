---
title: include dosyası
description: include dosyası
services: bastion
author: cherylmc
ms.service: bastion
ms.topic: include
ms.date: 10/15/2019
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: d31e30991056cc891e63347a2c88e7fc4caeab28
ms.sourcegitcommit: 9405aad7e39efbd8fef6d0a3c8988c6bf8de94eb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/05/2019
ms.locfileid: "74875510"
---
### <a name="regions"></a>Hangi bölgeler kullanılabilir?

[!INCLUDE [region](bastion-regions-include.md)]

### <a name="publicip"></a>Sanal makinemdeki genel IP 'ye ihtiyacım var mı?

Azure savunma hizmeti ile bağlanmakta olduğunuz Azure sanal makinesinde genel bir IP 'ye ihtiyacınız yoktur. Savunma hizmeti, sanal ağınız dahilinde sanal makinenizin özel IP 'si üzerinden sanal makinenize yönelik RDP/SSH oturumu/bağlantısını açar.

### <a name="rdpssh"></a>RDP veya SSH istemcisine ihtiyacım var mı?

Azure portalınızda Azure sanal makinenize RDP/SSH erişimi sağlamak için RDP veya SSH istemcisine ihtiyacınız yoktur. Sanal makinenize doğrudan tarayıcıda RDP/SSH erişimi almanızı sağlamak için [Azure Portal](https://portal.azure.com) kullanın.

### <a name="agent"></a>Azure sanal makinesinde çalışan bir aracıya ihtiyacım var mı?

Tarayıcınıza veya Azure sanal makinenize bir aracı veya herhangi bir yazılım yüklemeniz gerekmez. Bastion hizmeti aracısızdır ve RDP/SSH için ek bir yazılım gerektirmez.

### <a name="browsers"></a>Hangi tarayıcılar destekleniyor?

Windows üzerinde Microsoft Edge tarayıcısını veya Google Chrome 'u kullanın. Apple Mac için Google Chrome tarayıcıyı kullanın. Microsoft Edge Chromium sırasıyla Windows ve Mac’te de desteklenir.

### <a name="roles"></a>Bir sanal makineye erişmek için gereken roller nelerdir?

Bir bağlantı oluşturmak için aşağıdaki roller gereklidir:

* Sanal makinede okuyucu rolü
* Sanal makinenin özel IP 'si ile NIC 'de okuyucu rolü
* Azure savunma kaynağında okuyucu rolü

### <a name="pricingpage"></a>Fiyatlandırma nedir?

Daha fazla bilgi edinmek için bkz. [fiyatlandırma sayfası](https://aka.ms/BastionHostPricing).

### <a name="session"></a>Savunma oturumu başlamadan önce "oturumunuzun süresi doldu" hata iletisini neden alıyorum?

Bir oturum yalnızca Azure portal başlatılmalıdır. Azure portal oturum açın ve oturumunuzu yeniden başlatın. URL 'ye doğrudan başka bir tarayıcı oturumundan veya sekmesinden giderseniz, bu hata beklenmektedir. Oturumunuzun daha güvende olduğundan ve oturum yalnızca Azure portal aracılığıyla erişilebilmesini sağlamaya yardımcı olur.

### <a name="keyboard"></a>Uzak oturum sırasında hangi klavye düzenleri destekleniyor?

Azure savunma Şu anda VM 'nin içindeki en-US-QWERTY klavye yerleşimini desteklemektedir.  Klavye düzeni için diğer yerel ayarlara yönelik destek, devam etmekte olan çalışmadır.

### <a name="udr"></a>Kullanıcı tanımlı yönlendirme (UDR), bir Azure savunma alt ağında destekleniyor mu?

Hayır. UDR, bir Azure savunma alt ağında desteklenmez.
Aynı sanal ağda hem Azure savunma hem de Azure Güvenlik Duvarı/ağ sanal gereci (NVA) içeren senaryolarda, Azure savunma ve VM 'leriniz arasındaki iletişim özel olduğundan Azure savunma alt ağından Azure Güvenlik Duvarı 'na trafik zorlamaya gerek kalmaz. Daha fazla ayrıntı için bkz. [Azure Güvenlik duvarı arkasındaki VM 'lere savunma Ile erişme](https://azure.microsoft.com/blog/accessing-virtual-machines-behind-azure-firewall-with-azure-bastion/).

### <a name="filetransfer"></a>Azure savunma RDP oturumunda dosya aktarımı destekleniyor mu?

Yeni özellikler eklemek için çok çalıştık. Şu andan itibaren dosya aktarımı desteklenmez, ancak yol haritasında bir parçası. Lütfen [Azure savunma geri bildirim sayfasındaki](https://feedback.azure.com/forums/217313-networking?category_id=367303)yeni özelliklerle ilgili görüşlerinizi paylaşmaktan çekinmeyin.
