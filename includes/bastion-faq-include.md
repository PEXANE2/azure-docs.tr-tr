---
title: include dosyası
description: include dosyası
services: bastion
author: cherylmc
ms.service: bastion
ms.topic: include
ms.date: 03/25/2020
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 57a764b62fcda333f042794e176c24c8e6cc5526
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80374002"
---
### <a name="which-regions-are-available"></a><a name="regions"></a>Hangi bölgeler kullanılabilir?

[!INCLUDE [region](bastion-regions-include.md)]

### <a name="do-i-need-a-public-ip-on-my-virtual-machine"></a><a name="publicip"></a>Sanal makinemde herkese açık bir IP'ye ihtiyacım var mı?

Azure Bastion kullanarak bir VM'ye bağlandığınızda, bağlandığınız Azure Sanal Makine'de genel bir IP'ye ihtiyacınız yoktur. Bastion hizmeti, SANAL MAKINEnizin özel IP'si üzerinden sanal makinenize rdp/SSH oturumunu/bağlantısını sanal ağınız içinde açacaktır.

### <a name="is-ipv6-supported"></a>IPv6 desteklendi mi?

Şu anda, IPv6 desteklenmez. Azure Bastion yalnızca IPv4'u destekler.

### <a name="do-i-need-an-rdp-or-ssh-client"></a><a name="rdpssh"></a>RDP veya SSH istemcisi gerekiyor mu?

Azure portalınızdaki Azure sanal makinenize RDP/SSH'ye erişmek için bir RDP veya SSH istemcisine ihtiyacınız yoktur. Sanal makinenize doğrudan tarayıcıda RDP/SSH erişimi sağlamak için [Azure portalını](https://portal.azure.com) kullanın.

### <a name="does-azure-bastion-require-an-rds-cal-for-administrative-purposes-on-azure-hosted-vms"></a><a name="rdscal"></a>Azure Kalesi, Azure tarafından barındırılan VM'lerde yönetimsel amaçlar için bir RDS CAL gerektirir mi?
Hayır, Azure Bastion tarafından Windows Server VM'lere erişim yalnızca yönetim amaçlı kullanıldığında [RDS CAL](https://www.microsoft.com/en-us/p/windows-server-remote-desktop-services-cal/dg7gmgf0dvsv?activetab=pivot:overviewtab) gerektirmez.

### <a name="how-many-concurrent-rdp-and-ssh-sessions-does-each-azure-bastion-support"></a><a name="limits"></a>Her Azure Kalesi kaç eşzamanlı RDP ve SSH oturumlarını destekler?
Hem RDP hem de SSH kullanım tabanlı bir protokoldür. Oturumların yüksek kullanımı, burç ana bilgisayarının toplam oturum sayısının daha düşük olmasını desteklemesine neden olur. Aşağıdaki sayılar normal günlük iş akışlarını varsayar.

[!INCLUDE [limits](bastion-limits.md)]

### <a name="do-i-need-an-agent-running-in-the-azure-virtual-machine"></a><a name="agent"></a>Azure sanal makinesinde çalışan bir aracıya ihtiyacım var mı?

Tarayıcınıza veya Azure sanal makinenize bir aracı veya yazılım yüklemeniz gerekmez. Bastion hizmeti aracısızdır ve RDP/SSH için ek bir yazılım gerektirmez.

### <a name="which-browsers-are-supported"></a><a name="browsers"></a>Hangi tarayıcılar desteklenir?

Windows'da Microsoft Edge tarayıcısını veya Google Chrome'u kullanın. Apple Mac için Google Chrome tarayıcısı kullanın. Microsoft Edge Chromium, sırasıyla hem Windows hem de Mac'te de desteklenir.

### <a name="are-any-roles-required-to-access-a-virtual-machine"></a><a name="roles"></a>Sanal bir makineye erişmek için herhangi bir rol gerekli midir?

Bağlantı kurmak için aşağıdaki roller gereklidir:

* Sanal makinede okuyucu rolü
* Sanal makinenin özel IP'si ile NIC'de okuyucu rolü
* Azure Bastion kaynağında okuyucu rolü

### <a name="what-is-the-pricing"></a><a name="pricingpage"></a>Fiyatlandırma nedir?

Daha fazla bilgi edinmek için bkz. [fiyatlandırma sayfası](https://aka.ms/BastionHostPricing).

### <a name="why-do-i-get-your-session-has-expired-error-message-before-the-bastion-session-starts"></a><a name="session"></a>Bastion oturumu başlamadan önce neden "Oturumunuzun süresi doldu" hata iletisini alıyorum?

Oturum yalnızca Azure portalından başlatılmalıdır. Azure portalında oturum açın ve oturumunuza yeniden başlayın. URL'ye doğrudan başka bir tarayıcı oturumundan veya sekmesinden giderseniz, bu hata bekleniyor. Oturumunuzun daha güvenli olmasını ve oturuma yalnızca Azure portalı üzerinden erişilebilmesini sağlar.

### <a name="what-keyboard-layouts-are-supported-during-the-bastion-remote-session"></a><a name="keyboard"></a>Bastion uzak oturumu sırasında hangi klavye düzenleri desteklenir?

Azure Bastion şu anda VM içindeki en-us-qwerty klavye düzenini destekler.  Klavye düzeni için diğer yerel yönetimler için destek devam ediyor.

### <a name="is-user-defined-routing-udr-supported-on-an-azure-bastion-subnet"></a><a name="udr"></a>Kullanıcı tanımlı yönlendirme (UDR) bir Azure Bastion alt netinde mi desteklenir?

Hayır. UDR, Azure Bastion alt netinde desteklenmez.
Aynı sanal ağda hem Azure Kalesi hem de Azure Güvenlik Duvarı/Ağ Sanal Cihazı (NVA) içeren senaryolarda, Azure Bastion ile SANAL M'leriniz arasındaki iletişim özel olduğundan, bir Azure Bastion alt netinden Azure Güvenlik Duvarı'na trafik zorlamanız gerekmez. Daha fazla bilgi için Bkz. [Bastion ile Azure Güvenlik Duvarı'nın arkasındaki VM'lere erişme.](https://azure.microsoft.com/blog/accessing-virtual-machines-behind-azure-firewall-with-azure-bastion/)

### <a name="is-file-transfer-supported-with-azure-bastion-rdp-session"></a><a name="filetransfer"></a>Dosya aktarımı Azure Bastion RDP oturumuyla desteklenir mi?

Yeni özellikler eklemek için çok çalışıyoruz. Şu andan itibaren, dosya aktarımı desteklenmez, ancak yol haritamızın bir parçasıdır. Azure [Bastion Geri Bildirim sayfasında](https://feedback.azure.com/forums/217313-networking?category_id=367303)yeni özellikler hakkındaki görüşlerinizi paylaşmakten çekinmeyin.

### <a name="how-do-i-handle-deployment-failures"></a><a name="udr"></a>Dağıtım hatalarını nasıl işlerim?

Tüm hata iletilerini gözden geçirin ve [gerektiğinde Azure Portalı'nda bir destek isteği yükseltin.](https://docs.microsoft.com/azure/azure-portal/supportability/how-to-create-azure-support-request) Dağıtım hataları Azure [abonelik sınırlarından, kotalardan ve kısıtlamalardan](https://docs.microsoft.com/azure/azure-resource-manager/management/azure-subscription-service-limits)kaynaklanabilir. Özellikle, müşteriler abonelik başına izin verilen ve Azure Bastion dağıtımının başarısız olması yla ilgili genel IP adreslerinin sayısında bir sınırla karşılaşabilir.
