---
title: Azure VM için ayrıntılı SSH sorun giderme | Microsoft Dokümanlar
description: Azure sanal makinesine bağlanan sorunlar için daha ayrıntılı SSH sorun giderme adımları
keywords: ssh bağlantısı reddedildi,ssh hatası,azure ssh,SSH bağlantısı başarısız oldu
services: virtual-machines-linux
documentationcenter: ''
author: genlin
manager: dcscontentpm
editor: ''
tags: top-support-issue,azure-service-management,azure-resource-manager
ms.assetid: b8e8be5f-e8a6-489d-9922-9df8de32e839
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.topic: troubleshooting
ms.date: 10/31/2018
ms.author: genli
ms.openlocfilehash: ee6d437915f6c87ce9ef5f9c711d90793a96048c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77920136"
---
# <a name="detailed-ssh-troubleshooting-steps-for-issues-connecting-to-a-linux-vm-in-azure"></a>Azure'da Linux VM'ye bağlanmayla ilgili ayrıntılı SSH sorun giderme adımları
SSH istemcisinin VM'deki SSH hizmetine ulaşamayabilmesinin birçok nedeni olabilir. Daha [genel SSH sorun giderme adımlarını](troubleshoot-ssh-connection.md)izlediyseniz, bağlantı sorununu daha da gidermeniz gerekir. Bu makalede, SSH bağlantısının nerede başarısız olduğunu ve nasıl çözüleceğini belirlemek için ayrıntılı sorun giderme adımlarında size rehberlik eder.

## <a name="take-preliminary-steps"></a>Ön adımları atın
Aşağıdaki diyagram, ilgili bileşenleri gösterir.

![SSH hizmetinin bileşenlerini gösteren diyagram](./media/detailed-troubleshoot-ssh-connection/ssh-tshoot1.png)

Aşağıdaki adımlar, hatanın kaynağını yalıtmanıza ve çözümleri veya geçici çözümleri bulmanıza yardımcı olur.

1. Portaldaki VM'nin durumunu kontrol edin.
   Azure [portalında](https://portal.azure.com) **Sanal makineler** > *VM adını*seçin.

   VM için durum bölmesi **Running**göstermelidir. Bilgi işlem, depolama ve ağ kaynakları için son etkinliği göstermek için aşağı kaydırın.

2. Uç noktaları, IP adreslerini, ağ güvenlik gruplarını ve diğer ayarları incelemek için **Ayarlar'ı** seçin.

   VM'nin **SSH** trafiği için tanımlanan bir bitiş noktası olmalıdır ve Uç Noktalar'da veya **[Ağ güvenlik grubunda](../../virtual-network/security-overview.md)** görüntülenebilir. Kaynak Yöneticisi kullanılarak oluşturulan VM'lerde uç noktalar bir ağ güvenlik grubunda depolanır. Kuralların ağ güvenlik grubuna uygulandığını ve alt ağda başvurulduğunu doğrulayın.

Ağ bağlantısını doğrulamak için, yapılandırılan uç noktaları denetleyin ve HTTP veya başka bir hizmet gibi başka bir protokol aracılığıyla VM'ye bağlanıp bağlanamayacağınızı görün.

Bu adımlardan sonra SSH bağlantısını yeniden deneyin.

## <a name="find-the-source-of-the-issue"></a>Sorunun kaynağını bulma
Bilgisayarınızdaki SSH istemcisi, aşağıdaki alanlardaki sorunlar veya yanlış yapılandırmalar nedeniyle Azure VM'deki SSH hizmetine bağlanamayabilir:

* [SSH istemci bilgisayar](#source-1-ssh-client-computer)
* [Kuruluş kenar aygıtı](#source-2-organization-edge-device)
* [Bulut hizmeti bitiş noktası ve erişim denetim listesi (ACL)](#source-3-cloud-service-endpoint-and-acl)
* [Ağ güvenlik grupları](#source-4-network-security-groups)
* [Linux tabanlı Azure VM](#source-5-linux-based-azure-virtual-machine)

## <a name="source-1-ssh-client-computer"></a>Kaynak 1: SSH istemci bilgisayar
Bilgisayarınızı hatanın kaynağı olarak ortadan kaldırmak için, şirket içinde başka bir Linux tabanlı bilgisayara SSH bağlantıları kurabileceğini doğrulayın.

![SSH istemci bilgisayar bileşenlerini vurgulayan diyagram](./media/detailed-troubleshoot-ssh-connection/ssh-tshoot2.png)

Bağlantı başarısız olursa, bilgisayarınızdaaşağıdaki sorunları denetleyin:

* Gelen veya giden SSH trafiğini engelleyen yerel bir güvenlik duvarı ayarı (TCP 22)
* SSH bağlantılarını engelleyen yerel olarak yüklenmiş istemci proxy yazılımı
* SSH bağlantılarını engelleyen yerel olarak yüklenmiş ağ izleme yazılımı
* Trafiği izleyen veya belirli trafik türlerine izin veren/izin veren diğer güvenlik yazılımı türleri

Bu koşullardan biri uygulanırsa, yazılımı geçici olarak devre dışı kaldırın ve bağlantının bilgisayarınızda neden engellendiğini öğrenmek için şirket içi bir bilgisayara SSH bağlantısı deneyin. Ardından, SSH bağlantılarına izin vermek için yazılım ayarlarını düzeltmek için ağ yöneticinizle birlikte çalışın.

Sertifika kimlik doğrulamasını kullanıyorsanız, ev dizininizdeki .ssh klasörüne bu izinlere sahip olduğunuzu doğrulayın:

* Chmod 700 ~/.ssh
* Chmod 644 ~/.ssh/\*.pub
* Chmod 600 ~/.ssh/id_rsa (veya özel anahtarlarınızın içinde depolanan diğer dosyalar)
* Chmod 644 ~/.ssh/known_hosts (SSH üzerinden bağlandığınız ana bilgisayarları içerir)

## <a name="source-2-organization-edge-device"></a>Kaynak 2: Organizasyon kenar aygıtı
Kuruluşunuzun kenar aygıtını hatanın kaynağı olarak ortadan kaldırmak için, Doğrudan Internet'e bağlı bir bilgisayarın Azure VM'nize SSH bağlantısı kurabileceğini doğrulayın. VM'ye siteden siteye VPN veya Azure ExpressRoute bağlantısı üzerinden erişiyorsanız, [Kaynak 4: Ağ güvenlik gruplarına](#nsg)atlayın.

![Kuruluş kenar aygıtını vurgulayan diyagram](./media/detailed-troubleshoot-ssh-connection/ssh-tshoot3.png)

Doğrudan Internet'e bağlı bir bilgisayarınız yoksa, kendi kaynak grubunda veya bulut hizmetinde yeni bir Azure VM oluşturun ve bu yeni VM'yi kullanın. Daha fazla bilgi için bkz: [Azure'da Linux çalıştıran sanal bir makine oluştur.](../linux/quick-create-cli.md) Testini bitirdiğinizde kaynak grubunu veya VM ve bulut hizmetini silin.

Doğrudan Internet'e bağlı bir bilgisayarla SSH bağlantısı oluşturabiliyorsanız, kuruluş kenar aygıtınızı aşağıdakiler için denetleyin:

* Internet ile SSH trafiğini engelleyen dahili bir güvenlik duvarı
* SSH bağlantılarını engelleyen bir proxy sunucusu
* SSH bağlantılarını engelleyen kenar ağınızdaki aygıtlarda çalışan izinsiz giriş algılama veya ağ izleme yazılımı

Internet ile SSH trafiğine izin vermek için kuruluş kenar aygıtlarınızın ayarlarını düzeltmek için ağ yöneticinizle birlikte çalışın.

## <a name="source-3-cloud-service-endpoint-and-acl"></a>Kaynak 3: Bulut hizmeti bitiş noktası ve ACL

[!INCLUDE [classic-vm-deprecation](../../../includes/classic-vm-deprecation.md)]

> [!NOTE]
> Bu kaynak yalnızca klasik dağıtım modeli kullanılarak oluşturulan VM'ler için geçerlidir. Kaynak Yöneticisi kullanılarak oluşturulan VM'ler için [kaynak 4'e atlayın: Ağ güvenlik grupları.](#nsg)

Bulut hizmeti bitiş noktasını ve hatanın kaynağı olarak ACL'yi ortadan kaldırmak için, aynı sanal ağdaki başka bir Azure VM'sinin SSH kullanarak bağlanabildiğinizi doğrulayın.

![Bulut hizmeti bitiş noktasını ve ACL'yi vurgulayan diyagram](./media/detailed-troubleshoot-ssh-connection/ssh-tshoot4.png)

Aynı sanal ağda başka bir VM'niz yoksa, kolayca bir VM oluşturabilirsiniz. Daha fazla bilgi için [CLI'yi kullanarak Azure'da Linux VM oluştur'a](../linux/quick-create-cli.md)bakın. Testiniz bittiğinde ekstra VM'yi silin.

Aynı sanal ağda bir VM ile SSH bağlantısı oluşturabiliyorsanız, aşağıdaki alanları denetleyin:

* **Hedef VM'deki SSH trafiği için uç nokta yapılandırması.** Bitiş noktasının özel TCP bağlantı noktası, VM'deki SSH hizmetinin dinlediği TCP bağlantı noktasıyla eşleşmelidir. (Varsayılan bağlantı noktası 22'dir). **Sanal makineler** > *VM adı* > **Ayarları** > **Bitiş Noktaları'nı**seçerek Azure portalındaki SSH TCP bağlantı noktası numarasını doğrulayın.
* **Hedef sanal makinedeki SSH trafik bitiş noktası için ACL.** ACL, kaynak IP adresine bağlı olarak Internet'ten gelen veya reddedilen trafiği belirtmenizi sağlar. Yanlış yapılandırılmış ALA'lar, bitiş noktasına gelen SSH trafiğini engelleyebilir. Proxy veya diğer kenar sunucunuzun genel IP adreslerinden gelen trafiğe izin verilebilmesini sağlamak için ALA'larınızı kontrol edin. Daha fazla bilgi için [ağ erişim denetim listeleri (ALA'lar) hakkında](../../virtual-network/virtual-networks-acl.md)bkz.

Sorunun kaynağı olarak bitiş noktasını ortadan kaldırmak için geçerli bitiş noktasını kaldırın, başka bir bitiş noktası oluşturun ve SSH adını belirtin (ortak ve özel bağlantı noktası numarası için TCP bağlantı noktası 22). Daha fazla bilgi için bkz: [Azure'da sanal bir makinede uç noktaları ayarlama.](../windows/classic/setup-endpoints.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)

<a id="nsg"></a>

## <a name="source-4-network-security-groups"></a>Kaynak 4: Ağ güvenlik grupları
Ağ güvenlik grupları, izin verilen gelen ve giden trafik üzerinde daha ayrıntılı denetime sahip olduğunuzu sağlar. Bir Azure sanal ağında alt ağları ve bulut hizmetlerini kapsayan kurallar oluşturabilirsiniz. Internet'e giriş ve çıkış trafiğine izin verilebilmek için ağ güvenliği grubu kurallarınızı denetleyin.
Daha fazla bilgi için [ağ güvenlik grupları hakkında](../../virtual-network/security-overview.md)bkz.

NSG yapılandırmasını doğrulamak için IP Verify'i de kullanabilirsiniz. Daha fazla bilgi için Azure [ağ izleme genel bakışına](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview)bakın. 

## <a name="source-5-linux-based-azure-virtual-machine"></a>Kaynak 5: Linux tabanlı Azure sanal makine
Olası sorunların son kaynağı Azure sanal makinenin kendisidir.

![Linux tabanlı Azure sanal makinesini vurgulayan diyagram](./media/detailed-troubleshoot-ssh-connection/ssh-tshoot5.png)

Bunu daha önce yapmadıysanız, linux [tabanlı sanal makineleri sıfırlamak için](../linux/reset-password.md)yönergeleri izleyin.

Bilgisayarınızdan yeniden bağlanmayı deneyin. Hala başarısız olursa, olası sorunlardan bazıları şunlardır:

* SSH hizmeti hedef sanal makinede çalışmıyor.
* SSH hizmeti TCP port 22'yi dinlemiyor. Test etmek için yerel bilgisayarınıza bir telnet istemcisi yükleyin ve "telnet *cloudServiceName*.cloudapp.net 22" çalıştırın. Bu adım, sanal makinenin SSH bitiş noktasına gelen ve giden iletişime izin verilip verilip verilip verilip vermeyini belirler.
* Hedef sanal makinedeki yerel güvenlik duvarı, gelen veya giden SSH trafiğini engelleyen kurallara sahiptir.
* Azure sanal makinesinde çalışan izinsiz giriş algılama veya ağ izleme yazılımı SSH bağlantılarını engelliyor.

## <a name="additional-resources"></a>Ek kaynaklar
Sorun giderme uygulama erişimi hakkında daha fazla bilgi için Azure [sanal makinesinde çalışan bir uygulamaya Sorun Giderme erişimi](../linux/troubleshoot-app-connection.md)
