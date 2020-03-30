---
title: Azure VM'ler arasındaki bağlantı sorunlarını giderme | Microsoft Dokümanlar
description: Azure VM'ler arasındaki bağlantı sorunlarını nasıl gidereceklerini öğrenin.
services: virtual-network
documentationcenter: na
author: chadmath
manager: dcscontentpm
editor: ''
tags: azure-resource-manager
ms.service: virtual-network
ms.devlang: na
ms.topic: troubleshooting
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/30/2018
ms.author: genli
ms.openlocfilehash: ab3ae45081ecc481cb90af8961174e23c86e84b5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "71056811"
---
# <a name="troubleshooting-connectivity-problems-between-azure-vms"></a>Azure VM’leri arasında bağlantı sorunlarını giderme

Azure sanal makineleri (VM' ler) arasında bağlantı sorunları yaşayabilirsiniz. Bu makalede, bu sorunu çözmenize yardımcı olacak sorun giderme adımları sağlar. 

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="symptom"></a>Belirti

Bir Azure VM başka bir Azure VM'ye bağlanamaz.

## <a name="troubleshooting-guidance"></a>Sorun giderme rehberi 

1. [NIC'nin yanlış yapılandırılıp yapılmadığını kontrol edin](#step-1-check-whether-nic-is-misconfigured)
2. [Ağ trafiğinin NSG veya UDR tarafından engellenip engellenmediğini denetleme](#step-2-check-whether-network-traffic-is-blocked-by-nsg-or-udr)
3. [Ağ trafiğinin VM güvenlik duvarı tarafından engellenip engellenmediğini denetleme](#step-3-check-whether-network-traffic-is-blocked-by-vm-firewall)
4. [VM uygulamasının veya hizmetinin bağlantı noktasında dinleyip dinlemediğini kontrol edin](#step-4-check-whether-vm-app-or-service-is-listening-on-the-port)
5. [Sorunun SNAT'ten kaynaklanıp kaynaklanıp kaynaklanıp kaynaklmadığını denetleme](#step-5-check-whether-the-problem-is-caused-by-snat)
6. [Klasik VM için aLAKs lar tarafından trafiğin engellenip engellenmediğini kontrol edin](#step-6-check-whether-traffic-is-blocked-by-acls-for-the-classic-vm)
7. [Klasik VM için bitiş noktasının oluşturulup oluşturulmadığını kontrol edin](#step-7-check-whether-the-endpoint-is-created-for-the-classic-vm)
8. [VM ağ paylaşımına bağlanmayı deneyin](#step-8-try-to-connect-to-a-vm-network-share)
9. [Inter-Vnet bağlantısını kontrol edin](#step-9-check-inter-vnet-connectivity)

## <a name="troubleshooting-steps"></a>Sorun giderme adımları

Sorunu gidermek için aşağıdaki adımları izleyin. Her adımı tamamladıktan sonra, sorunun çözülüp çözülmediğini denetleyin. 

### <a name="step-1-check-whether-nic-is-misconfigured"></a>Adım 1: NIC'nin yanlış yapılandırılıp yapılmadığını kontrol edin

Azure Windows [VM için ağ arabirimini sıfırlama adımlarını](../virtual-machines/windows/reset-network-interface.md)izleyin. 

Sorun, ağ arabirimini (NIC) değiştirdikten sonra oluşursa, aşağıdaki adımları izleyin:

**Çok NİkL'LER**

1. Bir NIC ekleyin.
2. Kötü NIC sorunları düzeltmek veya kötü NIC kaldırın.  Sonra NIC'i tekrar ekleyin.

Daha fazla bilgi için [bkz.](virtual-network-network-interface-vm.md)

**Tek NİkVV** 

- [Windows VM'yi yeniden dağıtma](../virtual-machines/windows/redeploy-to-new-node.md)
- [Linux VM'yi yeniden dağıtın](../virtual-machines/linux/redeploy-to-new-node.md)

### <a name="step-2-check-whether-network-traffic-is-blocked-by-nsg-or-udr"></a>Adım 2: Ağ trafiğinin NSG veya UDR tarafından engellenip engellenmediğini kontrol edin

Trafik akışını engelleyen bir Ağ Güvenlik Grubu (NSG) veya Kullanıcı Tanımlı Rota (UDR) olup olmadığını belirlemek için [Ağ İzleyiciip Akış Doğrulamasını](../network-watcher/network-watcher-ip-flow-verify-overview.md) ve [NSG Akış Günlüğe kaydetmeyi](../network-watcher/network-watcher-nsg-flow-logging-overview.md) kullanın.

### <a name="step-3-check-whether-network-traffic-is-blocked-by-vm-firewall"></a>Adım 3: Ağ trafiğinin VM güvenlik duvarı tarafından engellenip engellenmediğini kontrol edin

Güvenlik duvarını devre dışı bırakıp sonucu test edin. Sorun çözülürse, güvenlik duvarı ayarlarını doğrulayın ve ardından güvenlik duvarını yeniden etkinleştirin.

### <a name="step-4-check-whether-vm-app-or-service-is-listening-on-the-port"></a>Adım 4: VM uygulamasının veya hizmetinin bağlantı noktasında dinleyip dinlemediğini kontrol edin

VM uygulamasının veya hizmetinin bağlantı noktasında dinleyip dinlemediğini kontrol etmek için aşağıdaki yöntemlerden birini kullanabilirsiniz.

- Sunucunun o bağlantı noktasında dinleyip dinlemediğini kontrol etmek için aşağıdaki komutları çalıştırın.

**Windows VM**

    netstat –ano

**Linux VM**

    netstat -l

- Bağlantı noktasını test etmek için sanal makinenin üzerinde **telnet** komutunu çalıştırın. Test başarısız olursa, uygulama veya hizmet bu bağlantı noktasını dinleyecek şekilde yapılandırılmamıştır.

### <a name="step-5-check-whether-the-problem-is-caused-by-snat"></a>Adım 5: Sorunun SNAT'ten kaynaklanıp kaynaklanıp kaynaklanıp kaynaklmadığını kontrol edin

Bazı senaryolarda, VM, Azure dışındaki kaynaklara bağımlı bir yük dengesi çözümünün arkasına yerleştirilir. Bu senaryolarda, aralıklı bağlantı sorunları yla karşılaşırsanız, sorun [SNAT bağlantı noktası tükenmesi](../load-balancer/load-balancer-outbound-connections.md)neden olabilir. Sorunu çözmek için, yük dengeleyicisinin arkasında ve NSG veya ACL ile güvenli olan her VM için bir VIP (veya klasik için ILPIP) oluşturun. 

### <a name="step-6-check-whether-traffic-is-blocked-by-acls-for-the-classic-vm"></a>Adım 6: Klasik VM için aLAKs lar tarafından trafiğin engellenip engellenmediğini kontrol edin

Erişim denetim listesi (ACL), sanal makine bitiş noktası trafiğine seçici olarak izin verme veya reddetme olanağı sağlar. Daha fazla bilgi için, [bitiş noktasında ACL'yi yönet'e](/previous-versions/azure/virtual-machines/windows/classic/setup-endpoints#manage-the-acl-on-an-endpoint)bakın.

### <a name="step-7-check-whether-the-endpoint-is-created-for-the-classic-vm"></a>Adım 7: Klasik VM için bitiş noktasının oluşturulup oluşturulmadığını kontrol edin

Klasik dağıtım modelini kullanarak Azure'da oluşturduğunuz tüm SANAL'lar, aynı bulut hizmetindeki veya sanal ağdaki diğer sanal makinelerle özel bir ağ kanalı üzerinden otomatik olarak iletişim kurabilir. Ancak, diğer sanal ağlardaki bilgisayarlar, gelen ağ trafiğini sanal bir makineye yönlendirmek için uç noktalar gerektirir. Daha fazla bilgi için [uç noktaları nasıl ayarlayınız'](../virtual-machines/windows/classic/setup-endpoints.md)a bakın.

### <a name="step-8-try-to-connect-to-a-vm-network-share"></a>Adım 8: VM ağ paylaşımına bağlanmayı deneyin

Bir VM ağ payına bağlanamıyorsanız, sorun VM'deki kullanılamayan NIC'lerden kaynaklanabilir. Kullanılamayan NIC'leri silmek [için, kullanılamayan NIC'leri nasıl silebilirsiniz](../virtual-machines/troubleshooting/reset-network-interface.md#delete-the-unavailable-nics)

### <a name="step-9-check-inter-vnet-connectivity"></a>Adım 9: Inter-Vnet bağlantısını kontrol edin

Trafik akışını engelleyen bir NSG veya UDR olup olmadığını belirlemek için [Ağ İzleyiciip Akış Doğrula](../network-watcher/network-watcher-ip-flow-verify-overview.md) ve [NSG Akış Günlüğü'ni](../network-watcher/network-watcher-nsg-flow-logging-overview.md) kullanın. Inter-Vnet yapılandırmanızı [buradan](https://support.microsoft.com/en-us/help/4032151/configuring-and-validating-vnet-or-vpn-connections)da doğrulayabilirsiniz.

### <a name="need-help-contact-support"></a>Yardıma mı ihtiyacınız var? Desteğe başvurun.
Yine de yardıma ihtiyacınız varsa sorununuzun hızla çözülmesini sağlamak için [desteğe başvurun](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).