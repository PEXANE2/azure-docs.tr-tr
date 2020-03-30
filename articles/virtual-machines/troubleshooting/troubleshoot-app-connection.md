---
title: Azure'da VM uygulama erişimini giderme | Microsoft Dokümanlar
description: Azure'da sanal makinelerde çalışan uygulamalara bağlanmayla ilgili sorunları yalıtmak için bu ayrıntılı sorun giderme adımlarını kullanın.
services: virtual-machines
documentationcenter: ''
author: genlin
manager: dcscontentpm
editor: ''
tags: top-support-issue,azure-service-management,azure-resource-manager
keywords: uygulamayı başlatamıyor, program açılmıyor, bağlantı noktası engellenemiyor, programı başlatamıyor, bağlantı noktası engellenemiyor
ms.assetid: b9ff7cd0-0c5d-4c3c-a6be-3ac47abf31ba
ms.service: virtual-machines
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.topic: troubleshooting
ms.date: 10/31/2018
ms.author: genli
ms.openlocfilehash: caf73ffbc18a603ace22acfbd0da490048da698a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "71058128"
---
# <a name="troubleshoot-application-connectivity-issues-on-virtual-machines-in-azure"></a>Azure'da sanal makinelerdeki uygulama bağlantısı sorunlarını giderme

Azure sanal makinesinde (VM) çalışan bir uygulamayı başlatamamanız veya bunlara bağlanamamanız için çeşitli nedenler vardır. Nedenler arasında uygulamanın beklenen bağlantı noktalarında çalışmaması veya dinlememesi, dinleme bağlantı noktasının engellenmesi veya uygulama trafiğini doğru şekilde geçmeme ağ kuralları yer alır. Bu makalede, sorunu bulmak ve düzeltmek için metodik bir yaklaşım açıklanmaktadır.

RDP veya SSH kullanarak VM'nize bağlanmada sorun yaşıyorsanız, önce aşağıdaki makalelerden birine bakın:

* [Windows tabanlı Azure Sanal Makine'ye uzak masaüstü bağlantılarını sorun giderme](troubleshoot-rdp-connection.md)
* [Linux tabanlı Bir Azure sanal makinesine Güvenli Kabuk (SSH) bağlantılarının giderin.](troubleshoot-ssh-connection.md)

Bu makalenin herhangi bir noktasında daha fazla yardıma ihtiyacınız varsa, [MSDN Azure ve Yığın Taşma forumlarında](https://azure.microsoft.com/support/forums/)Azure uzmanlarıyla iletişime geçebilirsiniz. Alternatif olarak, bir Azure destek olayı da dosyalayabilirsiniz. [Azure destek sitesine](https://azure.microsoft.com/support/options/) gidin ve Destek **Al'ı**seçin.

## <a name="quick-start-troubleshooting-steps"></a>Hızlı başlatma sorun giderme adımları
Bir uygulamaya bağlanmada sorun yaşıyorsanız, aşağıdaki genel sorun giderme adımlarını deneyin. Her adımdan sonra, uygulamanıza yeniden bağlanmayı deneyin:

* Sanal makineyi yeniden başlatma
* Bitiş noktası / güvenlik duvarı kuralları / ağ güvenlik grubu (NSG) kurallarını yeniden oluşturma
  * [Kaynak Yöneticisi modeli - Ağ Güvenlik Gruplarını Yönet](../../virtual-network/manage-network-security-group.md)
  * [Klasik model - Bulut Hizmetleri bitiş noktalarını yönet](../../cloud-services/cloud-services-enable-communication-role-instances.md)
* Farklı bir Azure sanal ağı gibi farklı konumdan bağlanma
* Sanal makineyi yeniden dağıtma
  * [Windows VM'yi yeniden dağıtma](redeploy-to-new-node-windows.md)
  * [Linux VM'yi yeniden dağıtın](redeploy-to-new-node-linux.md)
* Sanal makineyi yeniden oluşturma

Daha fazla bilgi için sorun [giderme Uç Nokta Bağlantısı (RDP/SSH/HTTP, vb. hataları)](https://social.msdn.microsoft.com/Forums/azure/en-US/538a8f18-7c1f-4d6e-b81c-70c00e25c93d/troubleshooting-endpoint-connectivity-rdpsshhttp-etc-failures?forum=WAVirtualMachinesforWindows)bakın.

## <a name="detailed-troubleshooting-overview"></a>Ayrıntılı sorun giderme genel bakış
Azure sanal makinesinde çalışan bir uygulamanın erişimini gidermek için dört ana alan vardır.

![sorun giderme uygulaması başlatılamaz](./media/virtual-machines-common-troubleshoot-app-connection/tshoot_app_access1.png)

1. Azure sanal makinesinde çalışan uygulama.
   * Uygulamanın kendisi doğru çalışıyor mu?
2. Azure sanal makinesi.
   * VM'nin kendisi doğru çalışıyor ve isteklere yanıt veriyor mu?
3. Azure ağ bitiş noktaları.
   * Klasik dağıtım modelindeki sanal makineler için bulut hizmeti bitiş noktaları.
   * Kaynak Yöneticisi dağıtım modelinde sanal makineler için Ağ Güvenlik Grupları ve gelen NAT kuralları.
   * Beklenen bağlantı noktalarında kullanıcılardan VM/uygulamaya trafik akabilir mi?
4. Internet edge cihazınız.
   * Güvenlik duvarı kuralları, trafiğin doğru akmasını engelliyor mu?

Siteye vpn veya ExpressRoute bağlantısı üzerinden uygulamaya erişen istemci bilgisayarlarda, sorunlara neden olabilecek başlıca alanlar uygulama ve Azure sanal makinesidir.

Sorunun kaynağını ve düzeltmesini belirlemek için aşağıdaki adımları izleyin.

## <a name="step-1-access-application-from-target-vm"></a>Adım 1: Hedef VM'den erişim uygulaması
Uygulama, üzerinde çalıştığı VM'den uygun istemci programıyla uygulamaya erişmeye çalışın. Yerel ana bilgisayar adını, yerel IP adresini veya geri dönüş adresini (127.0.0.1) kullanın.

![uygulamayı doğrudan VM'den başlatın](./media/virtual-machines-common-troubleshoot-app-connection/tshoot_app_access2.png)

Örneğin, uygulama bir web sunucusuysa, VM'de bir tarayıcı açın ve VM'de barındırılan bir web sayfasına erişmeye çalışın.

Uygulamaya erişebiliyorsanız, [Adım 2'ye](#step2)gidin.

Uygulamaya erişemiyorsanız, aşağıdaki ayarları doğrulayın:

* Uygulama hedef sanal makineüzerinde çalışıyor.
* Uygulama beklenen TCP ve UDP bağlantı noktalarını dinliyor.

Hem Windows hem de Linux tabanlı sanal makinelerde, etkin dinleme bağlantı noktalarını göstermek için **netstat -a** komutunu kullanın. Uygulamanızın dinlemesi gereken beklenen bağlantı noktalarının çıktısını inceleyin. Uygulamayı yeniden başlatın veya beklenen bağlantı noktalarını gerektiği gibi kullanacak şekilde yapılandırın ve uygulamaya yerel olarak yeniden erişmeye çalışın.

## <a name="step-2-access-application-from-another-vm-in-the-same-virtual-network"></a><a id="step2"></a>Adım 2: Aynı sanal ağdaki başka bir VM'den uygulama erişim
Uygulamaya farklı bir VM'den ancak aynı sanal ağda, VM'nin ana bilgisayar adını veya Azure tarafından atanan genel, özel veya sağlayıcı IP adresini kullanarak erişmeye çalışın. Klasik dağıtım modeli kullanılarak oluşturulan sanal makineler için bulut hizmetinin genel IP adresini kullanmayın.

![uygulamayı farklı bir VM'den başlatma](./media/virtual-machines-common-troubleshoot-app-connection/tshoot_app_access3.png)

Örneğin, uygulama bir web sunucusuysa, aynı sanal ağdaki farklı bir VM'deki bir tarayıcıdan bir web sayfasına erişmeye çalışın.

Uygulamaya erişebiliyorsanız, [Adım 3'e](#step3)gidin.

Uygulamaya erişemiyorsanız, aşağıdaki ayarları doğrulayın:

* Hedef VM'deki ana bilgisayar güvenlik duvarı gelen istek ve giden yanıt trafiğine izin verir.
* Hedef VM üzerinde çalışan izinsiz giriş algılama veya ağ izleme yazılımı trafiğe izin verir.
* Bulut Hizmetleri bitiş noktaları veya Ağ Güvenlik Grupları trafiğe izin verir:
  * [Klasik model - Bulut Hizmetleri bitiş noktalarını yönet](../../cloud-services/cloud-services-enable-communication-role-instances.md)
  * [Kaynak Yöneticisi modeli - Ağ Güvenlik Gruplarını Yönet](../../virtual-network/manage-network-security-group.md)
* VM'nizde, vm testi ile VM'niz arasındaki yük dengeleyicisi veya güvenlik duvarı gibi ayrı bir bileşen trafiğe izin verir.

Windows tabanlı sanal bir makinede, güvenlik duvarı kurallarının uygulamanızın gelen ve giden trafiğini dışlayıp kapsamadığını belirlemek için Gelişmiş Güvenlikli Windows Güvenlik Duvarı'nı kullanın.

## <a name="step-3-access-application-from-outside-the-virtual-network"></a><a id="step3"></a>Adım 3: Sanal ağ dışından uygulamaya erişin
Uygulamanın çalıştığı VM olarak sanal ağ dışındaki bir bilgisayardan uygulamaya erişmeye çalışın. Özgün istemci bilgisayarınız olarak farklı bir ağ kullanın.

![sanal ağ dışındaki bir bilgisayardan uygulamayı başlatma](./media/virtual-machines-common-troubleshoot-app-connection/tshoot_app_access4.png)

Örneğin, uygulama bir web sunucusuysa, sanal ağda olmayan bir bilgisayarda çalışan bir tarayıcıdan web sayfasına erişmeye çalışın.

Uygulamaya erişemiyorsanız, aşağıdaki ayarları doğrulayın:

* Klasik dağıtım modeli kullanılarak oluşturulan VM'ler için:
  
  * VM için uç nokta yapılandırmasının, özellikle protokol (TCP veya UDP) ve ortak ve özel bağlantı noktası numaraları olmak üzere gelen trafiğe izin verdiğini doğrulayın.
  * Bitiş noktasındaki erişim denetim listelerinin (ALA'lar) Internet'ten gelen trafiği engellemediğini doğrulayın.
  * Daha fazla bilgi için, [Sanal Makine'nin Uç Noktalarını Nasıl](../windows/classic/setup-endpoints.md)Ayarlayınız' a bakın.
* Kaynak Yöneticisi dağıtım modeli kullanılarak oluşturulan VM'ler için:
  
  * VM için gelen NAT kural yapılandırmasının, özellikle protokol (TCP veya UDP) ve ortak ve özel bağlantı noktası numaraları olmak üzere gelen trafiğe izin verdiğini doğrulayın.
  * Ağ Güvenlik Grupları'nın gelen istek ve giden yanıt trafiğine izin verdiğini doğrulayın.
  * Daha fazla bilgi için ağ [güvenlik grubunun ne olduğunu](../../virtual-network/security-overview.md) görün

Sanal makine veya uç nokta yük dengeli bir kümenin üyesiyse:

* Sonda protokolü (TCP veya UDP) ve bağlantı noktası numarasının doğru olduğunu doğrulayın.
* Sonda protokolü ve bağlantı noktası yük dengeli küme protokolü ve bağlantı noktası farklıysa:
  * Uygulamanın sonda protokolü (TCP veya UDP) ve bağlantı noktası numarasını (hedef VM'de **netstat -a** kullanın) dinlediğini doğrulayın.
  * Hedef VM'deki ana bilgisayar güvenlik duvarının gelen sonda isteğine ve giden sonda yanıt trafiğine izin verdiğini doğrulayın.

Uygulamaya erişebiliyorsanız, Internet edge cihazınızın aşağıdakileri sağladığından emin olun:

* Giden uygulama, istemci bilgisayarınızdan Azure sanal makinesine trafik isteğinde bulundu.
* Azure sanal makinesinden gelen uygulama yanıt trafiği.

## <a name="step-4-if-you-cannot-access-the-application-use-ip-verify-to-check-the-settings"></a>Adım 4 Uygulamaya erişemiyorsanız, ayarları kontrol etmek için IP Doğrula'yı kullanın. 

Daha fazla bilgi için Azure [ağ izleme genel bakışına](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview)bakın. 

## <a name="additional-resources"></a>Ek kaynaklar
[Windows tabanlı Azure Sanal Makine'ye uzak masaüstü bağlantılarını sorun giderme](troubleshoot-rdp-connection.md)

[Linux tabanlı Azure sanal makinesine Güvenli Kabuk (SSH) bağlantılarıyla sorun giderme](troubleshoot-ssh-connection.md)


