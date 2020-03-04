---
title: include dosyası
description: include dosyası
services: bastion
author: cherylmc
ms.service: bastion
ms.topic: include
ms.date: 03/02/2020
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 17d391a7e6b8ef0558fb73afe363cd96deb60a7d
ms.sourcegitcommit: e4c33439642cf05682af7f28db1dbdb5cf273cc6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/03/2020
ms.locfileid: "78262233"
---
### <a name="regions"></a>Hangi bölgeler kullanılabilir?

[!INCLUDE [region](bastion-regions-include.md)]

### <a name="publicip"></a>Sanal makinemdeki genel IP 'ye ihtiyacım var mı?

Azure savunma kullanarak bir VM 'ye bağlandığınızda, bağlandığınız Azure sanal makinesinde genel bir IP 'ye ihtiyacınız yoktur. Savunma hizmeti, sanal ağınız dahilinde sanal makinenizin özel IP 'si üzerinden sanal makinenize yönelik RDP/SSH oturumu/bağlantısını açar.

### <a name="is-ipv6-supported"></a>IPv6 destekleniyor mu?

Şu anda IPv6 desteklenmez. Azure savunma yalnızca IPv4 'Ü destekler.

### <a name="rdpssh"></a>RDP veya SSH istemcisine ihtiyacım var mı?

Azure portal Azure sanal makinenize RDP/SSH 'ye erişmek için bir RDP veya SSH istemcisine ihtiyacınız yoktur. Sanal makinenize doğrudan tarayıcıda RDP/SSH erişimi almanızı sağlamak için [Azure Portal](https://portal.azure.com) kullanın.

### <a name="rdscal"></a>Azure savunma, Azure 'da barındırılan VM 'lerde yönetim amacıyla bir RDS CAL gerektiriyor mu?
Hayır, Azure 'da Windows Server VM 'lerine erişim yalnızca yönetim amacıyla kullanıldığında bir [RDS CAL](https://www.microsoft.com/en-us/p/windows-server-remote-desktop-services-cal/dg7gmgf0dvsv?activetab=pivot:overviewtab) gerektirmez.

### <a name="agent"></a>Azure sanal makinesinde çalışan bir aracıya ihtiyacım var mı?

Tarayıcınıza veya Azure sanal makinenize bir aracı ya da herhangi bir yazılımı yüklemeniz gerekmez. Savunma hizmeti aracısız ve RDP/SSH için ek yazılım gerektirmez.

### <a name="browsers"></a>Hangi tarayıcılar destekleniyor?

Windows üzerinde Microsoft Edge tarayıcısını veya Google Chrome 'u kullanın. Apple Mac için Google Chrome tarayıcısı ' nı kullanın. Microsoft Edge Kmıum, sırasıyla hem Windows hem de Mac 'te desteklenir.

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
Aynı sanal ağda hem Azure savunma hem de Azure Güvenlik Duvarı/ağ sanal gereci (NVA) içeren senaryolarda, Azure savunma ve VM 'leriniz arasındaki iletişim özel olduğundan Azure savunma alt ağından Azure Güvenlik Duvarı 'na trafik zorlamaya gerek kalmaz. Daha fazla bilgi için bkz. [Azure Güvenlik duvarı arkasındaki sanal makinelere savunma Ile erişme](https://azure.microsoft.com/blog/accessing-virtual-machines-behind-azure-firewall-with-azure-bastion/).

### <a name="filetransfer"></a>Azure savunma RDP oturumunda dosya aktarımı destekleniyor mu?

Yeni özellikler eklemek için çok çalıştık. Şu andan itibaren dosya aktarımı desteklenmez, ancak yol haritasında bir parçası. Lütfen [Azure savunma geri bildirim sayfasındaki](https://feedback.azure.com/forums/217313-networking?category_id=367303)yeni özelliklerle ilgili görüşlerinizi paylaşmaktan çekinmeyin.
