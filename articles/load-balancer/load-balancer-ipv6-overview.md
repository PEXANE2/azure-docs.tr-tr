---
title: IPv6'ya Genel Bakış - Azure Yük Dengeleyicisi
description: Bu öğrenme yolu ile Azure Yük Dengeleyicisi ve yük dengesi olan VM'ler için IPv6 desteği ile başlayın.
services: load-balancer
documentationcenter: na
author: asudbring
keywords: ipv6, azure yük dengeleyici, çift yığın, genel ip, yerli ipv6, mobil, iot
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.custom: seodec18
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/24/2018
ms.author: allensu
ms.openlocfilehash: d2966f32fcf7f8be2a93d1639f0a63f49768c306
ms.sourcegitcommit: 7d8158fcdcc25107dfda98a355bf4ee6343c0f5c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/09/2020
ms.locfileid: "80981860"
---
# <a name="overview-of-ipv6-for-azure-load-balancer"></a>Azure Yük Dengeleyicisi için IPv6'ya Genel Bakış


>[!NOTE] 
>Bu içeriğin yerini Azure [VNet Genel Bakışı için IPv6](https://docs.microsoft.com/azure/virtual-network/ipv6-overview)almıştır. Azure, yeni IPv6 dağıtımlarının Azure Sanal Ağlar için yeni IPv6 özelliklerini kullanmasını önerir.

>[!NOTE]
>Azure Load Balancer iki farklı türü destekler: Temel ve Standart. Bu makalede Temel Yük Dengeleyici anlatılmaktadır. Standart Yük Dengeleyicisi hakkında daha fazla bilgi [için, Standart Yük Dengeleyicisi genel bakış](load-balancer-standard-overview.md)bakın.

Temel SKU Internet'e bakan yük dengeleyicileri Bir IPv6 adresi yle dağıtılabilir. IPv4 bağlantısına ek olarak, bu aşağıdaki yetenekleri sağlar:

* Yük dengeleyicisi aracılığıyla ortak Internet istemcileri ve Azure Sanal Makineleri (VM' ler) arasında uçtan uca IPv6 bağlantısı.
* VM'ler ve genel Internet IPv6 özellikli istemciler arasında uçtan uca IPv6 giden bağlantı.

Aşağıdaki resim, Azure Yük Dengeleyicisi için IPv6 işlevini göstermektedir.

![IPv6 ile Azure Yük Dengeleyicisi](./media/load-balancer-ipv6-overview/load-balancer-ipv6.png)

Bir IPv4 veya IPv6 özellikli Internet istemcisi dağıtıldıktan sonra, Azure Internet'e bakan Yük Dengeleyicisi'nin ortak IPv4 veya IPv6 adresleriyle (veya ana bilgisayar adları) iletişim kurabilir. Yük dengeleyicisi, ağ adresi çevirisi (NAT) kullanarak IPv6 paketlerini VM'lerin özel IPv6 adreslerine yönlendirir. IPv6 Internet istemcisi VM'lerin IPv6 adresiyle doğrudan iletişim kuramaz.

## <a name="features"></a>Özellikler

Azure Kaynak Yöneticisi aracılığıyla dağıtılan VM'ler için yerel IPv6 desteği şunları sağlar:

1. Internet'teki IPv6 istemcileri için yük dengeli IPv6 hizmetleri
2. VM'lerde yerel IPv6 ve IPv4 uç noktaları ("çift yığılmış")
3. Gelen ve giden başlatılan yerel IPv6 bağlantıları
4. TCP, UDP ve HTTP(S) gibi desteklenen protokoller, tam kapsamlı hizmet mimarisi sağlar

## <a name="benefits"></a>Yararları

Bu işlevsellik aşağıdaki önemli avantajları sağlar:

* Yalnızca IPv6 istemcileri için yeni uygulamaların erişilebilir olmasını gerektiren hükümet yönetmeliklerine uymak
* Mobil ve Nesnelerin İnterneti (IOT) geliştiricilerin, büyüyen mobil & IOT pazarlarına hitap etmek için çift yığılmış (IPv4+IPv6) Azure Sanal Makineleri kullanmalarını etkinleştirin

## <a name="details-and-limitations"></a>Ayrıntılar ve sınırlamalar

Ayrıntılar

* Azure DNS hizmeti hem IPv4 A hem de IPv6 AAAA ad kayıtlarını içerir ve yük dengeleyicisinin her iki kaydıyla da yanıt verir. İstemci hangi adresle iletişim kureceğini (IPv4 veya IPv6) seçer.
* Bir VM genel Internet IPv6 bağlı aygıta bağlantı başlattığında, VM'nin kaynak IPv6 adresi yük dengeleyicisinin ortak IPv6 adresine çevrilmiş ağ adresidir (NAT).
* Linux işletim sistemini çalıştıran VM'ler, DHCP üzerinden Bir IPv6 IP adresi alacak şekilde yapılandırılmalıdır. Azure Galerisi'ndeki Linux görüntülerinin çoğu değişiklik yapılmadan IPv6'yı destekleyecek şekilde yapılandırılmıştır. Daha fazla bilgi için Linux [VM'leri için DHCPv6 Yapılandırma'ya](load-balancer-ipv6-for-linux.md) bakın
* Yük bakiyenizle birlikte bir sistem durumu sondası kullanmayı seçerseniz, bir IPv4 probu oluşturun ve hem IPv4 hem de IPv6 uç noktalarıyla kullanın. VM'nizdeki hizmet düşerse, hem IPv4 hem de IPv6 uç noktaları döndürme dışına çıkarılır.

Sınırlamalar

* Azure portalına IPv6 yük dengeleme kuralları ekleyemezsiniz. Kurallar yalnızca şablon, CLI, PowerShell aracılığıyla oluşturulabilir.
* IPv6 adreslerini kullanmak için varolan VM'leri yükseltemeyebilirsiniz. Yeni VM'ler dağıtmanız gerekir.
* Her VM'de tek bir IPv6 adresi tek bir ağ arabirimine atanabilir.
* Ortak IPv6 adresleri bir VM'ye atanamaz. Bunlar yalnızca bir yük dengeleyicisine atanabilirler.
* Genel IPv6 adresleriniz için ters DNS aramasını yapılandıramazsınız.
* IPv6 adreslerine sahip VM'ler Bir Azure Bulut Hizmetinin üyesi olamaz. Bir Azure Sanal Ağına (VNet) bağlanabilir ve IPv4 adresleri üzerinden birbirleriyle iletişim kurabilirler.
* Özel IPv6 adresleri bir kaynak grubundaki tek tek VM'lerde dağıtılabilir, ancak Ölçek Kümeleri aracılığıyla kaynak grubuna dağıtılamaz.
* Azure VM'ler IPv6 üzerinden diğer VM'lere, diğer Azure hizmetlerine veya şirket içi aygıtlara bağlanamaz. Yalnızca IPv6 üzerinden Azure yük dengeleyicisiyle iletişim kurabilirler. Ancak, IPv4 kullanarak bu diğer kaynaklarla iletişim kurabilir.
* IPv4 için Ağ Güvenlik Grubu (NSG) koruması çift yığınlı (IPv4+IPv6) dağıtımlarda desteklenir. NSG'ler IPv6 uç noktalarıiçin geçerli değildir.
* VM'deki IPv6 bitiş noktası doğrudan internete maruz kalmamış. Bir yük dengeleyiciarkasındadır. Yalnızca yük dengeleyici kurallarında belirtilen bağlantı noktalarına IPv6 üzerinden erişilebilir.
* IPv6 için IdleTimeout parametresini değiştirme **şu anda desteklenmiyor.** Varsayılan değer dört dakikadır.
* IPv6 için loadDistributionMethod parametresinin değiştirilmesi **şu anda desteklenmiyor.**
* Ayrılmış IPv6 IP'leri (IPAllocationMethod = static) **şu anda desteklenmiyor.**
* NAT64 (IPv6'nın IPv4'e çevirisi) desteklenmez.
* Bir arka uç havuzuna IPv6 alt ağına başvuran ikincil bir NIC eklemek **şu anda desteklenmez.**

## <a name="next-steps"></a>Sonraki adımlar

IPv6 ile yük dengeleyicisini nasıl dağıtacağımız hakkında bilgi edinin.

* [Bölgelere göre IPv6 kullanılabilirliği](https://go.microsoft.com/fwlink/?linkid=828357)
* [Şablon kullanarak IPv6 ile yük dengeleyicisi dağıtma](load-balancer-ipv6-internet-template.md)
* [Azure PowerShell'i kullanarak IPv6 ile yük dengeleyicisi dağıtma](load-balancer-ipv6-internet-ps.md)
* [Azure CLI'yi kullanarak IPv6 ile yük dengeleyicisi dağıtma](load-balancer-ipv6-internet-cli.md)
