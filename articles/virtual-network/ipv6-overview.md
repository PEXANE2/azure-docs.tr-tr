---
title: Azure Sanal Ağı için IPv6'ya Genel Bakış
titlesuffix: Azure Virtual Network
description: Bir Azure sanal ağında iPv6 uç noktalarının ve veri yollarının IPv6 açıklaması.
services: virtual-network
documentationcenter: na
author: KumudD
manager: twooley
ms.service: virtual-network
ms.devlang: NA
ms.topic: article
ms.workload: infrastructure-services
ms.date: 03/31/2020
ms.author: kumud
ms.openlocfilehash: 887f9e100a60561271a4c15777e7131ea1f1f722
ms.sourcegitcommit: d597800237783fc384875123ba47aab5671ceb88
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2020
ms.locfileid: "80631443"
---
# <a name="what-is-ipv6-for-azure-virtual-network"></a>Azure Sanal Ağ için IPv6 nedir?

Azure Sanal Ağ için IPv6 (VNet), Hem sanal ağ içinde hem de Internet'ten IPv6 ve IPv4 bağlantısıyla Azure'daki uygulamaları barındırmanızı sağlar. Genel IPv4 adreslerinin tükenmesi nedeniyle, mobilite ve Nesnelerin İnterneti (IoT) için yeni ağlar genellikle IPv6 üzerine kurulur. Hatta uzun köklü ISS ve mobil ağlar IPv6 dönüştürülmektedir. Yalnızca IPv4 hizmetleri, hem mevcut hem de gelişmekte olan pazarlarda kendilerini gerçek bir dezavantajda bulabilirler. Çift yığın IPv4/IPv6 bağlantısı, Azure barındırılan hizmetlerin hem mevcut IPv4 hem de bu yeni IPv6 aygıtları ve ağlarıyla kolayca bağlanabilen, küresel olarak kullanılabilen çift yığınlı hizmetlerle bu teknoloji boşluğundan geçiş inmesini sağlar.

Azure'un orijinal IPv6 bağlantısı, Azure'da barındırılan uygulamalar için çift yığın (IPv4/IPv6) Internet bağlantısı sağlamayı kolaylaştırır. Hem gelen hem de giden başlatılan bağlantılar için yük dengeli IPv6 bağlantısına sahip VM'lerin basit bir şekilde dağıtılmasını sağlar. Bu özellik hala mevcuttur ve daha fazla bilgi [burada](../load-balancer/load-balancer-ipv6-overview.md)mevcuttur.
Azure için IPv6 sanal ağı çok daha dolu özelliklidir ve tam IPv6 çözüm mimarilerinin Azure'da dağıtılmasını sağlar.


Aşağıdaki diyagram, Azure'da basit bir çift yığın (IPv4/IPv6) dağıtımını görüntüler:

![IPv6 ağ dağıtım diyagramı](./media/ipv6-support-overview/ipv6-sample-diagram.png)

## <a name="benefits"></a>Yararları

Azure VNET avantajları için IPv6 avantajları:

- Azure'da barındırılan uygulamalarınızın erişimini büyüyen mobil ve Nesnelerin İnterneti pazarlarına genişletmeye yardımcı olur.
- Çift yığılmış IPv4/IPv6 VM'ler maksimum hizmet dağıtım esnekliği sağlar. Tek bir hizmet örneği hem IPv4 hem de IPv6 özellikli Internet istemcilerine bağlanabilir.
- Köklü, kararlı Azure VM'den Internet'e IPv6 bağlantısına dayalıdır.
- IPv6'nın Internet bağlantısı yalnızca dağıtımınızda açıkça istediğinizde kurulduğundan, varsayılan olarak güvenlidir.

## <a name="capabilities"></a>Özellikler

Azure VNet için IPv6 aşağıdaki özellikleri içerir:

- Azure müşterileri, uygulamalarının, müşterilerinin gereksinimlerini karşılamak veya şirket içi IP alanına sorunsuz bir şekilde entegre olmak için kendi IPv6 sanal ağ adres alanlarını tanımlayabilir.
- Çift yığın (IPv4 ve IPv6) çift yığın alt ağları ile sanal ağlar, uygulamaların hem Sanal ağlarında hem De IPv4 hem de IPv6 kaynaklarına bağlanmasını sağlar.
    > [!IMPORTANT]
    > IPv6'nın alt ağları tam /64 boyutunda olmalıdır.  Bu, bazı yönlendiriciler yalnızca /64 IPv6 rotalarını kabul edebileceğinden, alt ağın şirket içi ağa yönlendirilmesini etkinleştirmeye karar vermeniz gelecekteki uyumluluğu sağlar.  
- Ağ Güvenlik Grupları için IPv6 kurallarıyla kaynaklarınızı koruyun.
    - Azure platformunun Dağıtılmış Hizmet Reddi (DDoS) korumaları Internet'e bakan Public IP'leri
- Özellikle uygulamanızı artırmak için Ağ Sanal Cihazları'ndan yararlanarak Kullanıcı Tanımlı Rotalar ile sanal ağınızdaki IPv6 trafiğinin yönlendirmesini özelleştirin.
- Linux ve Windows Sanal Makineler tüm Azure VNET için IPv6 kullanabilirsiniz
- Aşağıdakileri içeren esnek, ölçeklenebilir uygulamalar oluşturmak için [Standart IPv6 genel Yük Dengeleyici](virtual-network-ipv4-ipv6-dual-stack-standard-load-balancer-powershell.md) desteği:
    - Hangi arka uç havuzu örneklerinin sistem durumu olduğunu ve böylece yeni akışlar alabileceğini belirlemek için isteğe bağlı IPv6 sistem sondası.
    - Bu yeteneği özel gereksinimlerinize göre ölçeklendirmek ve ayarlamak için giden bağlantı üzerinde tam bildirimsel denetim sağlayan isteğe bağlı giden kurallar.
    - Tek bir yük dengeleyicisinin birden çok IPv6 genel IP adresi kullanmasını sağlayan isteğe bağlı birden çok ön uç yapılandırmaları- aynı ön uç protokolü ve bağlantı noktası ön uç adresleri arasında yeniden kullanılabilir.
    - İsteğe bağlı IPv6 bağlantı noktaları, yük dengeleme kurallarının *Kayan IP* özelliği kullanılarak arka uç örneklerinde yeniden kullanılabilir 
    - Not: Yük dengeleme herhangi bir protokol çevirisi gerçekleştirmez (NAT64 yok). 
- Azure VNET'lerde esnek çok katmanlı uygulamalar oluşturmak için [standart IPv6 dahili Yük Dengeleyici](ipv6-dual-stack-standard-internal-load-balancer-powershell.md) desteği.   
- Eski dağıtımlarla uyumluluk için temel IPv6 genel Yük Dengeleyici desteği
- [Ayrılmış IPv6 Genel IP adresleri ve adres aralıkları,](ipv6-public-ip-address-prefix.md) şirketiniz ve müşterileriniz için azure tarafından barındırılan uygulamalarınızın beyaz listesini kolaylaştıran kararlı, öngörülebilir IPv6 adresleri sağlar.
- Örnek düzeyindeki Public IP, IPv6 Internet bağlantısını doğrudan tek tek VM'lere sağlar.
- [IPv6'yı yalnızca Varolan IPv4 dağıtımlarına ekleyin](ipv6-add-to-existing-vnet-powershell.md)- bu özellik, dağıtımları yeniden oluşturmanıza gerek kalmadan mevcut IPv4 yalnızca dağıtımlara kolayca IPv6 bağlantısı eklemenizi sağlar.  IPv4 ağ trafiği bu işlem sırasında etkilenmez, bu nedenle uygulamanıza ve işletim sisteminize bağlı olarak canlı hizmetlere bile IPv6 ekleyebilirsiniz.    
- IPv6 (AAAA) kayıtları için Azure DNS desteği ile seçtikleri protokolü kullanarak Internet istemcilerinin çift yığın uygulamanıza sorunsuz bir şekilde erişmelerine izin verin. 
- IPv6 ile sanal makine ölçek setleri ile otomatik olarak yükünüzü ölçeklendirecek çift yığın uygulamaları oluşturun.
- [Sanal Ağ (VNET) Peering](virtual-network-peering-overview.md) - hem bölgesel hem de küresel peering - görünüşte çift yığın VNETs bağlamak için izin - hem IPv4 ve IPv6 uç noktaları VMs üzerinde eşlenen ağlarda birbirleriyle iletişim kurmak mümkün olacak. Dağıtımlarınızı çift yığına taşırken, yalnızca IPv4 vnet'lerle çift desteyi eşleyebilirsiniz. 
- IPv6 Sorun Giderme ve Tanılama, paket yakalama, NSG akış günlükleri, bağlantı sorun giderme ve bağlantı izleme gibi yük dengeleyici ölçümleri/uyarı ve Ağ İzleyicisi özellikleriyle kullanılabilir.   

## <a name="scope"></a>Kapsam
Azure VNET için IPv6, müşterilerin Azure'da çift yığın (IPv4+IPv6) uygulamalarını barındırmasını sağlayan temel bir özellik kümesidir.  Zaman içinde daha fazla Azure ağ özelliğine IPv6 desteği eklemeyi ve sonunda Azure PaaS hizmetlerinin çift yığın sürümlerini sunmayı amaçlıyoruz, ancak bu arada tüm Azure PaaS hizmetlerine çift yığın Sanal Makinelerdeki IPv4 uç noktaları üzerinden erişilebiliyor.   

## <a name="limitations"></a>Sınırlamalar
Azure sanal ağ sürümü için geçerli IPv6 aşağıdaki sınırlamaları vardır:
- Azure sanal ağı için IPv6, tüm küresel Azure Ticari bölgelerinde tüm dağıtım yöntemlerini kullanarak kullanılabilir.  ABD Hükümeti bulutunda dağıtım geçici olarak ARM (JSON) şablonu, Command Line Interface (CLI) ve Powershell ile sınırlıdır.  ABD Hükümeti bulut portalında IPv6 desteği kısa süre içinde kullanılabilir olacaktır.  
- ExpressRoute ağ geçitleri, IPv6 etkin leştirilmiş bir VNET'teki yalnızca IPv4 trafiği için kullanılabilir.  IPv6 trafiğine destek yol haritamızda yer almaktadır.   
- VPN ağ geçitleri, Doğrudan veya "UseRemoteGateway" ile bakan IPv6 etkin leştirilmiş bir VNET'te kullanılamaz.
- Azure platformu (AKS, vb.) Kapsayıcılar için IPv6 iletişimini desteklemez.  

## <a name="pricing"></a>Fiyatlandırma

IPv6 Azure kaynakları ve bant genişliği, IPv4 ile aynı fiyatlarla ücretlendirilir. IPv6 için ek veya farklı bir ücret yoktur. [Genel IP adresleri,](https://azure.microsoft.com/pricing/details/ip-addresses/) [ağ bant genişliği](https://azure.microsoft.com/pricing/details/bandwidth/)veya Yük [Dengeleyicisi](https://azure.microsoft.com/pricing/details/load-balancer/)fiyatlandırması hakkında ayrıntılı bilgi bulabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure PowerShell'i kullanarak bir IPv6 çift yığın uygulamasını nasıl dağıtılayacağım öğrenin.](virtual-network-ipv4-ipv6-dual-stack-standard-load-balancer-powershell.md)
- [Azure CLI'yi kullanarak Bir IPv6 çift yığın uygulamasını nasıl dağıtılayacağım öğrenin.](virtual-network-ipv4-ipv6-dual-stack-standard-load-balancer-cli.md)
- [Kaynak Yöneticisi Şablonlarını (JSON) kullanarak Bir IPv6 çift yığın uygulamasını nasıl dağıtılayacağımöğrenin](ipv6-configure-standard-load-balancer-template-json.md)
