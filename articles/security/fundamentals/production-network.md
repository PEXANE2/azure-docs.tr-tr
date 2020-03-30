---
title: Azure üretim ağı
description: Bu makalede, Azure üretim ağının genel bir açıklaması verilmektedir.
services: security
documentationcenter: na
author: TerryLanfear
manager: barbkess
editor: TomSh
ms.assetid: 61e95a87-39c5-48f5-aee6-6f90ddcd336e
ms.service: security
ms.subservice: security-fundamentals
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/28/2018
ms.author: terrylan
ms.openlocfilehash: 7c0748e4ff1531649274834cb1e602c228f102e8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "68726700"
---
# <a name="the-azure-production-network"></a>Azure üretim ağı
Azure üretim ağının kullanıcıları arasında hem kendi Azure uygulamalarına erişen harici müşteriler hem de üretim ağını yöneten dahili Azure destek personeli yer alıyor. Bu makalede, Azure üretim ağına bağlantı kurmak için güvenlik erişim yöntemleri ve koruma mekanizmaları açıklanmaktadır.

## <a name="internet-routing-and-fault-tolerance"></a>Internet yönlendirme ve hata toleransı
Birden çok birincil ve ikincil DNS sunucu kümeleriyle birlikte, genel olarak gereksiz bir iç ve dış Azure Alan Adı Hizmeti (DNS) altyapısı hata toleransı sağlar. Aynı zamanda, dağıtılmış hizmet reddi (DDoS) saldırılarını önlemek ve Azure DNS hizmetlerinin bütünlüğünü korumak için NetScaler gibi ek Azure ağ güvenliği denetimleri kullanılır.

Azure DNS sunucuları birden çok veri merkezi tesisinde bulunur. Azure DNS uygulaması, Azure müşteri etki alanı adlarını genel olarak çözmek için ikincil ve birincil DNS sunucularından oluşan bir hiyerarşi içerir. Alan adları genellikle müşterinin hizmeti için sanal IP (VIP) adresini saran bir CloudApp.net adresine çözülür. Azure'a özgü, kiracı çevirisinin dahili özel IP (DIP) adresine karşılık gelen VIP, bu VIP'den sorumlu Microsoft yük dengeleyicileri tarafından yapılır.

Azure, ABD'de coğrafi olarak dağıtılmış Azure veri merkezlerinde barındırılan ve sağlam ve ölçeklenebilir mimari standartları uygulayan son teknoloji yönlendirme platformları üzerine kurulmuştur. Önemli özellikleri arasında şunlardır:

- Multiprotocol Label Switching (MPLS) tabanlı trafik mühendisliği, verimli bağlantı kullanımı ve bir kesinti varsa hizmet zarif bozulması sağlar.
- Ağlar "need plus one" (N+1) artıklık mimarileri veya daha iyisi ile uygulanır.
- Harici olarak, veri merkezlerine, birden fazla bakış noktasında dünya çapında 1.200'den fazla internet servis sağlayıcısıyla özellikleri gereksiz bir şekilde bağlayan özel, yüksek bant genişliğine sahip ağ devreleri sunulmaktadır. Bu bağlantı, saniyede 2.000 gigabayt (GBps) kenar kapasitesi sağlar.

Microsoft veri merkezleri arasında kendi ağ devrelerine sahip olduğundan, bu öznitelikler Azure teklifinin geleneksel üçüncü taraf internet servis sağlayıcılarına gerek kalmadan yüzde 99,9+ağ kullanılabilirliği elde edebilmesine yardımcı olur.

## <a name="connection-to-production-network-and-associated-firewalls"></a>Üretim ağına ve ilgili güvenlik duvarlarına bağlantı
Azure ağı internet trafiği akışı ilkesi, trafiği ABD'deki en yakın bölgesel veri merkezinde bulunan Azure üretim ağına yönlendirir. Azure üretim veri merkezleri tutarlı ağ mimarisi ve donanımı koruduğundan, izleyen trafik akışı açıklaması tüm veri merkezleri için tutarlı bir şekilde uygulanır.

Azure'un internet trafiği en yakın veri merkezine yönlendirildikten sonra, erişim yönlendiricilerine bir bağlantı kurulur. Bu erişim yönlendiricileri, Azure düğümleri ile müşteri tarafından anında verilen VM'ler arasındaki trafiği yalıtmak için hizmet vermektedir. Erişim ve kenar konumlarındaki ağ altyapısı aygıtları, giriş ve çıkış filtrelerinin uygulandığı sınır noktalarıdır. Bu yönlendiriciler, istenmeyen ağ trafiğini filtrelemek ve gerekirse trafik hızı sınırlarını uygulamak için katmanlı bir erişim denetim listesi (ACL) aracılığıyla yapılandırılır. ACL tarafından izin verilen trafik yük dengeleyicilerine yönlendirilir. Dağıtım yönlendiricileri yalnızca Microsoft onaylı IP adreslerine izin verecek, sahteciliği önlemeyi sağlayacak ve ALA'lar kullanan TCP bağlantıları kuracak şekilde tasarlanmıştır.

Harici yük dengeleme aygıtları, internet routable IP'lerinden Azure dahili IP'lerine ağ adresi çevirisi (NAT) gerçekleştirmek için erişim yönlendiricilerinin arkasında yer alır. Aygıtlar ayrıca paketleri geçerli üretim dahili IP'lere ve bağlantı noktalarına yönlendirir ve dahili üretim ağı adres alanını sınırlamak için bir koruma mekanizması görevi görür.

Varsayılan olarak, Microsoft, oturum açma ve sonrasındaki tüm trafik de dahil olmak üzere müşterilerin web tarayıcılarına aktarılan tüm trafik için Hypertext Transfer Protocol Secure 'u (HTTPS) zorlar. TLS v1.2 kullanımı trafiğin akmasını sağlayan güvenli bir tünel sağlar. Erişim ve temel yönlendiriciler üzerindeki ALA'lar, trafiğin kaynağının beklenenle tutarlı olmasını sağlar.

Bu mimaride önemli bir ayrım, geleneksel güvenlik mimarisi ile karşılaştırıldığında, hiçbir özel donanım güvenlik duvarları, özel saldırı algılama veya önleme cihazları, ya da normalde diğer güvenlik aletleri olmasıdır Azure üretim ortamına bağlantılar yapılmadan önce beklenen. Müşteriler genellikle bu donanım güvenlik duvarı aygıtlarını Azure ağında bekler; ancak azure'da hiçbiri çalışmıyor. Bu güvenlik özellikleri, güvenlik duvarı özellikleri de dahil olmak üzere sağlam, çok katmanlı güvenlik mekanizmaları sağlamak için Azure ortamını çalıştıran yazılımda yerleşiktir. Ayrıca, azure çalıştıran yazılım tarafından yönetildiği için, önceki resimde gösterildiği gibi, kritik güvenlik aygıtlarının sınırının kapsamı ve ilişkili yayılma alanı nın yönetilmesi ve envanterinin sürülmesi daha kolaydır.

## <a name="core-security-and-firewall-features"></a>Çekirdek güvenlik ve güvenlik duvarı özellikleri
Azure, temel Güvenlik İzni sınırını korumak için genellikle geleneksel bir ortamda beklenen güvenlik özelliklerini uygulamak için çeşitli düzeylerde güçlü yazılım güvenliği ve güvenlik duvarı özellikleri uygular.

### <a name="azure-security-features"></a>Azure güvenlik özellikleri
Azure, üretim ağında ana bilgisayar tabanlı yazılım güvenlik duvarları uygular. Birçok temel güvenlik ve güvenlik duvarı özelliği, temel Azure ortamında bulunmaktadır. Bu güvenlik özellikleri, Azure ortamında ki ayrıntılı bir savunma stratejisini yansıtır. Azure'daki müşteri verileri aşağıdaki güvenlik duvarları yla korunmaktadır:

**Hypervisor güvenlik duvarı (paket filtresi)**: Bu güvenlik duvarı hipervizörde uygulanır ve kumaş denetleyici (FC) aracısı tarafından yapılandırılır. Bu güvenlik duvarı, VM içinde çalışan kiracıyı yetkisiz erişime karşı korur. Varsayılan olarak, bir VM oluşturulduğunda, tüm trafik engellenir ve ardından FC aracısı yetkili trafiğe izin vermek için filtreye kurallar ve özel durumlar ekler.

İki kural kategorisi burada programlanmaktadır:

- **Makine config veya altyapı kuralları**: Varsayılan olarak, tüm iletişim engellenir. Bir VM'nin Dinamik Ana Bilgisayar Yapılandırma Protokolü (DHCP) iletişimve DNS bilgilerini göndermesine ve almasına ve trafiği FC kümesi ve OS Etkinleştirme sunucusundaki diğer VM'lere giden "ortak" internete göndermesine izin veren özel durumlar vardır. VM'lerin giden hedefler listesi Azure yönlendirici alt ağlarını ve diğer Microsoft özelliklerini içermedığından, kurallar onlar için bir savunma katmanı görevi göremez.
- **Rol yapılandırma dosya kuralları**: Kiracıların hizmet modelini temel alan gelen AA'ları tanımlar. Örneğin, bir kiracının belirli bir VM'deki bağlantı noktası 80'de bir web ön ucu varsa, bağlantı noktası 80 tüm IP adreslerine açılır. VM çalışan bir işçi rolü varsa, işçi rolü yalnızca aynı kiracı içinde VM açılır.

**Yerel ana bilgisayar güvenlik duvarı**: Azure Hizmet Dokusu ve Azure Depolama, hipervizörü olmayan ve bu nedenle Windows Güvenlik Duvarı önceki iki kural kümesiyle yapılandırılan yerel bir işletim sistemi üzerinde çalışır.

**Ana bilgisayar güvenlik duvarı**: Ana bilgisayar güvenlik duvarı, hipervizörü çalıştıran ana bilgisayar bölmesini korur. Kurallar, yalnızca FC ve atlama kutularının belirli bir bağlantı noktasındaki ana bilgisayar bölümüyle konuşmasına izin verecek şekilde programlanır. Diğer özel durumlar DHCP yanıtı ve DNS yanıtları izin vermektir. Azure, ana bilgisayar bölümü için güvenlik duvarı kuralları şablonu içeren bir makine yapılandırma dosyası kullanır. VM'lerin belirli protokol/bağlantı noktaları aracılığıyla bileşenleri, kablo sunucusu ve meta veri sunucusubarındırmak için iletişim kurmasına olanak tanıyan bir ana bilgisayar güvenlik duvarı özel durumu da vardır.

**Konuk güvenlik duvarı**: Müşteri VM'lerinde ve depolamada müşteriler tarafından yapılandırılabilen konuk işletim sistemi Windows Güvenlik Duvarı parçası.

Azure yeteneklerinde yerleşik olan ek güvenlik özellikleri şunlardır:

- DIPs'den gelen IP adresleri atanan altyapı bileşenleri. Internet'teki bir saldırgan, Microsoft'a ulaşamayacağı için bu adreslere giden trafiği ele alamaz. Internet ağ geçidi yönlendiricileri, yalnızca dahili adreslere yönelik paketleri filtreler, böylece üretim ağına girmezler. VIP'lere yönlendirilen trafiği kabul eden tek bileşen yük dengeleyicileridir.
- Tüm iç düğümlerde uygulanan güvenlik duvarları, belirli bir senaryo için üç birincil güvenlik mimarisi göz önünde bulundurulur:

   - Güvenlik duvarları yük dengeleyicisinin arkasına yerleştirilir ve her yerden paketleri kabul eder. Bu paketler harici olarak maruz kalmak için tasarlanmıştır ve geleneksel bir çevre güvenlik duvarında açık bağlantı noktalarına karşılık gelecektir.
   - Güvenlik duvarları paketleri yalnızca sınırlı sayıda adresten kabul eder. Bu husus, DDoS saldırılarına karşı derinlemesine savunma stratejisinin bir parçasıdır. Bu tür bağlantılar şifreleme olarak doğrulanır.
   - Güvenlik duvarlarına yalnızca belirli iç düğümlerden erişilebilir. Paketleri yalnızca, tümü Azure ağındaki DIPs olan numaralandırılmış kaynak IP adresleri listesinden kabul ederler. Örneğin, şirket ağına yapılan bir saldırı istekleri bu adreslere yönlendirebilir, ancak paketin kaynak adresi Azure ağında numaralandırılmış listede yer almıyorsa saldırılar engellenebilir.
     - Çevredeki erişim yönlendiricisi, yapılandırılmış statik rotaları nedeniyle Azure ağının içindeki bir adrese yönlendirilen giden paketleri engeller.

## <a name="next-steps"></a>Sonraki adımlar
Microsoft'un Azure altyapısını korumak için ne yaptığı hakkında daha fazla bilgi edinmek için bkz:

- [Azure tesisleri, tesisler ve fiziksel güvenlik](physical-security.md)
- [Azure altyapısı kullanılabilirliği](infrastructure-availability.md)
- [Azure bilgi sistemi bileşenleri ve sınırları](infrastructure-components.md)
- [Azure ağ mimarisi](infrastructure-network.md)
- [Azure SQL Veritabanı güvenlik özellikleri](infrastructure-sql.md)
- [Azure üretim işlemleri ve yönetimi](infrastructure-operations.md)
- [Azure altyapı izleme](infrastructure-monitoring.md)
- [Azure altyapı bütünlüğü](infrastructure-integrity.md)
- [Azure müşteri veri koruması](protection-customer-data.md)
