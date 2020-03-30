---
title: Azure Site Kurtarma ile başarısız olduktan sonra genel IP adresleri atama
description: Olağanüstü durum kurtarma ve geçiş için Azure Site Kurtarma ve Azure Trafik Yöneticisi ile ortak IP adreslerinin nasıl ayarlanır açıklanır
services: site-recovery
author: mayurigupta13
manager: rochakm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 04/08/2019
ms.author: mayg
ms.openlocfilehash: b1f3ffa6fc90fc0cab0217d1b71907342f2dbd0d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79281957"
---
# <a name="set-up-public-ip-addresses-after-failover"></a>Başarısız olduktan sonra genel IP adreslerini ayarlama

Genel IP adresleri, İnternet kaynaklarının Azure kaynakları ile gelen iletişimi kurmasına izin verir. Genel IP adresleri ayrıca Azure kaynaklarının İnternet ve kaynağa atanmış bir IP adresi ile genel kullanıma yönelik Azure hizmetleri ile giden iletişimi kurmasını sağlar.
- Azure Sanal Makineler (VM), Azure Uygulama Ağ Geçitleri, Azure Yük Dengeleyicileri, Azure VPN Ağ Geçitleri ve diğerleri gibi Internet'ten kaynağa gelen iletişim. VM yük dengeleyici arka uç havuzunun bir parçası olduğu ve yük bakiyeleyiciye ortak bir IP adresi atandığı sürece, VM'nin kendisine atanmış genel bir IP adresi yoksa, Internet'ten VM'ler gibi bazı kaynaklarla iletişim kurmaya devam edebilirsiniz.
- Öngörülebilir bir IP adresi kullanarak Internet'e giden bağlantı. Örneğin, sanal bir makine internete giden, herkese açık bir IP adresi olmadan iletişim kurabilir, ancak adresi varsayılan olarak Azure tarafından öngörülemeyen bir genel adrese çevrilen ağ adresidir. Bir kaynağa ortak bir IP adresi atamak, giden bağlantı için hangi IP adresinin kullanıldığını bilmenizi sağlar. Tahmin edilebilir olsa da, seçilen atama yöntemine bağlı olarak adres değişebilir. Daha fazla bilgi için [bkz.](../virtual-network/virtual-network-public-ip-address.md#create-a-public-ip-address) Azure kaynaklarından giden bağlantılar hakkında daha fazla bilgi edinmek için [bkz.](../load-balancer/load-balancer-outbound-connections.md?toc=%2fazure%2fvirtual-network%2ftoc.json)

Azure Kaynak Yöneticisi'nde, Genel IP adresi kendi özellikleri olan bir kaynaktır. Genel bir IP adresini ilişkilendirebileceğiniz kaynakların bazıları şunlardır:

* Sanal makine ağ arabirimleri
* İnternet'e yönelik yük dengeleyiciler
* VPN ağ geçitleri
* Uygulama ağ geçitleri

Bu makalede, Site Kurtarma ile Ortak IP adreslerini nasıl kullanabileceğiniz açıklanmaktadır.

## <a name="public-ip-address-assignment-using-recovery-plan"></a>Kurtarma Planı'nı kullanarak genel IP adresi ataması

Üretim uygulamasının genel IP adresi **başarısız lıkta tutulamaz.** Başarısız olma işleminin bir parçası olarak gündeme getirilen iş yüklerine hedef bölgede kullanılabilen bir Azure Genel IP kaynağı atanmalıdır. Bu adım el ile yapılabilir veya kurtarma planları ile otomatikleştirilir. Kurtarma planı makineleri kurtarma gruplarına toplar. Sistematik bir kurtarma işlemini tanımlamanıza yardımcı olur. Azure'da veya komut dosyasına başarısız olmak için Azure Otomasyon runbook'larını kullanarak sipariş vermek ve her adımda gereken eylemleri otomatikleştirmek için bir kurtarma planı kullanabilirsiniz.

Kurulum aşağıdaki gibidir:
- Bir [kurtarma planı](../site-recovery/site-recovery-create-recovery-plans.md#create-a-recovery-plan) oluşturun ve iş yüklerinizi planlandığı gibi gruplandırın.
- [Azure Automation runbooks](../site-recovery/site-recovery-runbook-automation.md#customize-the-recovery-plan) komut dosyalarını kullanarak ortak bir IP adresi eklemek için bir adım ekleyerek planı özelleştirin.

 
## <a name="public-endpoint-switching-with-dns-level-routing"></a>DNS düzeyi Yönlendirme ile ortak uç nokta geçişi

Azure Trafik Yöneticisi, uç noktalar arasında DNS düzeyinde yönlendirmeyi sağlar ve DR senaryosu için [RTO'larınızı düşürmeye](../site-recovery/concepts-traffic-manager-with-site-recovery.md#recovery-time-objective-rto-considerations) yardımcı olabilir. 

Trafik Yöneticisi ile başarısız senaryolar hakkında daha fazla bilgi edinin:
1. Trafik Yöneticisi ile [Azure'a şirket içi başarısız](../site-recovery/concepts-traffic-manager-with-site-recovery.md#on-premises-to-azure-failover) 
2. [Azure'dan Azure'a trafik](../site-recovery/concepts-traffic-manager-with-site-recovery.md#azure-to-azure-failover) yöneticisi ile başarısız 

Kurulum aşağıdaki gibidir:
- Trafik [Yöneticisi profili](../traffic-manager/traffic-manager-create-profile.md)oluşturun.
- **Öncelik** yönlendirme yöntemini kullanarak, kaynak için **birincil** ve Azure için **Failover** olmak üzere iki uç nokta oluşturun. **Birincil** Öncelik 1 atanır ve **Failover** Öncelik 2 atanır.
- **Birincil** bitiş noktası, kaynak ortamınızın Azure içinde veya dışında olup olmadığına bağlı olarak [Azure](../traffic-manager/traffic-manager-endpoint-types.md#azure-endpoints) veya [Dış](../traffic-manager/traffic-manager-endpoint-types.md#external-endpoints) olabilir.
- **Failover** bitiş noktası **Azure** bitiş noktası olarak oluşturulur. Bu durum felaket durumunda Trafik Yöneticisi için dış bakan uç noktası olacak gibi statik bir **ortak IP adresi** kullanın.

## <a name="next-steps"></a>Sonraki adımlar
- [Azure Site Kurtarma ile Trafik Yöneticisi](../site-recovery/concepts-traffic-manager-with-site-recovery.md) hakkında daha fazla bilgi edinin
- Trafik Yöneticisi [yönlendirme yöntemleri](../traffic-manager/traffic-manager-routing-methods.md)hakkında daha fazla bilgi edinin.
- Uygulama başarısızlığını otomatikleştirmek için [kurtarma planları](site-recovery-create-recovery-plans.md) hakkında daha fazla bilgi edinin.
