---
title: 'Öğretici: ağ denetim listesi'
description: Ağ gereksinimi önkoşulları ve ağ bağlantısı ve ağ bağlantı noktalarıyla ilgili ayrıntılar
ms.topic: tutorial
ms.date: 08/21/2020
ms.openlocfilehash: aba5d7767e420b3ade6238621487884e44fbb6e2
ms.sourcegitcommit: 62717591c3ab871365a783b7221851758f4ec9a4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/22/2020
ms.locfileid: "88750419"
---
# <a name="networking-checklist-for-azure-vmware-solution"></a>Azure VMware çözümü için ağ denetim listesi 

Azure VMware çözümü, şirket içi ve Azure tabanlı ortamların veya kaynakların kullanıcıları ve uygulamaları için erişilebilen bir VMware özel bulut ortamı sunar. Bağlantı, Azure ExpressRoute ve VPN bağlantıları gibi ağ hizmetleri aracılığıyla dağıtılır ve hizmetleri etkinleştirmek için bazı belirli ağ adresi aralıkları ve güvenlik duvarı bağlantı noktaları gerektirir. Bu makale, ağınızı Azure VMware çözümüyle çalışacak şekilde doğru şekilde yapılandırmak için bilmeniz gereken bilgileri sağlar.

Bu öğreticide şunları öğreneceksiniz:

> [!div class="checklist"]
> * Ağ bağlantısı gereksinimleri
> * Azure VMware çözümünde DHCP

## <a name="virtual--network-and-expressroute-circuit--considerations"></a>Sanal ağ ve ExpressRoute devresi konuları
Aboneliğinizdeki bir sanal ağdan bağlantı oluşturduğunuzda, ExpressRoute bağlantı hattı eşleme yoluyla oluşturulur, bir yetkilendirme anahtarı ve Azure portal isteğiniz bir eşleme KIMLIĞI kullanır. Eşleme, özel bulutunuz ile sanal ağ arasında özel, bire bir bağlantıdır.

> [!NOTE] 
> ExpressRoute bağlantı hattı, özel bir bulut dağıtımının bir parçası değildir. Şirket içi ExpressRoute bağlantı hattı, bu belgenin kapsamı dışındadır. Özel bulutunuzun şirket içi bağlantısına ihtiyacınız varsa, mevcut ExpressRoute devrelerinizi kullanabilir veya Azure portal bir tane satın alabilirsiniz.

Özel bir bulut dağıtımında, vCenter ve NSX-T Yöneticisi için IP adresleri alırsınız. Bu yönetim arabirimlerine erişmek için aboneliğinizdeki bir sanal ağda ek kaynaklar oluşturmanız gerekir. Bu kaynakları oluşturma ve öğreticilerde ExpressRoute özel eşlemesi oluşturma yordamlarını bulabilirsiniz.

Özel bulut mantıksal ağı, önceden sağlanmış NSX-T ile gelir. Katman 0 ağ geçidi ve Katman 1 ağ geçidi sizin için önceden sağlanmış. Bir segment oluşturabilir ve var olan katman 1 ağ geçidine iliştirebilir veya tanımladığınız yeni bir katman 1 ağ geçidine iliştirebilirsiniz. NSX-T mantıksal ağ bileşenleri, iş yükleri arasında Doğu Batı bağlantısı sağlar ve internet ve Azure hizmetlerine yönelik Kuzey-Güney bağlantısını sağlar.

## <a name="routing-and-subnet-considerations"></a>Yönlendirme ve alt ağ değerlendirmeleri
AVS özel bulutu, Azure ExpressRoute bağlantısı kullanılarak Azure sanal ağınıza bağlanır. Bu yüksek bant genişliği, düşük gecikmeli bağlantı, özel bulut ortamınızdan Azure aboneliğinizde çalışan hizmetlere erişmenizi sağlar. Yönlendirme Sınır Ağ Geçidi Protokolü (BGP) tabanlı, otomatik olarak sağlandı ve her özel bulut dağıtımı için varsayılan olarak etkindir. 

AVS özel bulutları `/22` , alt ağlar için aşağıda gösterilen en az bir CIDR ağ adres bloğu gerektirir. Bu ağ, şirket içi ağlarınızı tamamlar. Adres bloğu, aboneliğinizdeki ve şirket içi ağlardaki diğer sanal ağlarda kullanılan adres bloklarıyla çakışmamalıdır. Bu adres bloğu, yönetim, sağlama ve vMotion ağları içinde otomatik olarak sağlanırlar.

Örnek `/22` CIDR ağ adresi bloğu:  `10.10.0.0/22`

Alt ağlar:

| Ağ kullanımı             | Alt ağ | Örnek        |
| ------------------------- | ------ | -------------- |
| Özel bulut yönetimi  | `/24`  | `10.10.0.0/24` |
| vMotion ağı           | `/24`  | `10.10.1.0/24` |
| VM iş yükleri              | `/24`  | `10.10.2.0/24` |
| ExpressRoute eşlemesi      | `/24`  | `10.10.3.8/30` |


### <a name="network-ports-required-to-communicate-with-the-service"></a>Hizmetle iletişim kurmak için gereken ağ bağlantı noktaları

| Kaynak | Hedef | Protokol | Bağlantı noktası | Açıklama  | 
| ------ | ----------- | :------: | :---:| ------------ | 
| Özel bulut DNS sunucusu | Şirket Içi DNS sunucusu | UDP | 53 | Şirket içi DNS sorguları için BILGISAYAR vCenter 'dan DNS Istemcisi-Iletme istekleri (aşağıdaki DNS bölümünü denetle) |  
| Şirket içi DNS sunucusu   | Özel bulut DNS sunucusu | UDP | 53 | Şirket içi hizmetlerden özel bulut DNS sunucularına DNS Istemcisi-Iletme istekleri (aşağıdaki DNS bölümüne bakın) |  
| Şirket içi ağı  | Özel bulut vCenter sunucusu  | TCP (HTTP)  | 80 | vCenter Server, doğrudan HTTP bağlantıları için bağlantı noktası 80 gerektirir.Bağlantı noktası 80 istekleri HTTPS bağlantı noktası 443 ' e yeniden yönlendirir. Bu yeniden yönlendirme yerine kullanıyorsanız yardımcı olur  `http://server`    `https://server` .  <br><br>WS-Management (Ayrıca bağlantı noktası 443 ' ün açık olması gerekir) <br><br>VCenter Server paketlenmiş SQL Server 2008 veritabanı değil özel bir Microsoft SQL veritabanı kullanırsanız, bağlantı noktası 80, SQL Raporlama Hizmetleri tarafından kullanılır. VCenter Server yüklediğinizde, yükleyici vCenter Server HTTP bağlantı noktasını değiştirmenizi ister. Başarılı bir yükleme sağlamak için vCenter Server HTTP bağlantı noktasını özel bir değerle değiştirin.Microsoft Internet Information Services (IIS) Ayrıca 80 numaralı bağlantı noktasını kullanır. 80 numaralı bağlantı noktası için bkz. vCenter Server ile IIS arasındaki çakışma. |  
| Özel bulut yönetimi ağı | Şirket içi Active Directory  | TCP  | 389 | Bu bağlantı noktası, vCenter Server yerel ve tüm uzak örneklerinde açık olmalıdır. Bu bağlantı noktası, vCenter Server grubu için Dizin Hizmetleri için LDAP bağlantı noktası numarasıdır. Bu vCenter Server örneğini bağlı bir mod grubuna katsanız bile vCenter Server sistemin bağlantı noktası 389 ' e bağlanması gerekir. Bu bağlantı noktasında başka bir hizmet çalışıyorsa, onu kaldırmak veya bağlantı noktasını farklı bir bağlantı noktasıyla değiştirmek tercih edilebilir. LDAP hizmetini 1025 ile 65535 arasındaki herhangi bir bağlantı noktasında çalıştırabilirsiniz.Bu örnek Microsoft Windows Active Directory olarak sunulurken, 389 ile 65535 arasında bağlantı noktası numarasını ile kullanılabilir bir bağlantı 1025 noktasına değiştirin. Bu bağlantı noktası, şirket içi AD 'yi özel bulut vCenter üzerinde bir kimlik kaynağı olarak yapılandırmak için isteğe bağlıdır. |  
| Şirket içi ağı  | Özel bulut vCenter sunucusu  | TCP (HTTPS)  | 443 | Bu bağlantı noktası, şirket içi ağdan vCenter 'a erişmenizi sağlar.VCenter Server sisteminin vSphere Istemcisinden bağlantı dinlemek için kullandığı varsayılan bağlantı noktası. VCenter Server sisteminin vSphere Istemcisinden veri almasını sağlamak için, güvenlik duvarında 443 numaralı bağlantı noktasını açın. VCenter Server sistem SDK istemcilerinden veri aktarımını izlemek için 443 numaralı bağlantı noktasını da kullanır.Bu bağlantı noktası şu hizmetler için de kullanılır: WS-Management (Ayrıca bağlantı noktası 80 ' ün açık olması gerekir). vSphere güncelleştirme yöneticisine vSphere Istemci erişimi. VCenter Server için üçüncü taraf ağ yönetim istemcisi bağlantıları. Üçüncü taraf ağ yönetimi istemcileri konaklara erişim sağlar. |  
| Web Tarayıcısı  | Karma bulut Yöneticisi  | TCP (HTTPS) | 9443 | Hibrit Cloud Manager sistem yapılandırması için karma bulut Yöneticisi sanal gereç yönetim arabirimi. |
| Yönetici ağı  | Karma bulut Yöneticisi | SSH | 22 | Karma bulut Yöneticisi 'ne yönetici SSH erişimi. |
| HCM | Bulut ağ geçidi | TCP (HTTPS) | 8123 | Karma bulut ağ geçidine ana bilgisayar tabanlı çoğaltma hizmeti yönergeleri gönderin. |
| HCM | Bulut ağ geçidi | HTTP TCP (HTTPS) | 9443 | REST API kullanarak yönetim yönergelerini yerel karma bulut ağ geçidine gönderin. |
| Bulut ağ geçidi | L2C | TCP (HTTPS) | 443 | L2C karma bulut ağ geçidiyle aynı yolu kullandığında, bulut ağ geçidinden L2C 'ye yönetim yönergeleri gönderin. |
| Bulut ağ geçidi | ESXi Konakları | TCP | 80.902 | Yönetim ve OVF dağıtımı. |
| Bulut ağ geçidi (yerel)| Bulut ağ geçidi (uzak) | UDP | 4500 | IPSEC için gerekli<br>   Çift yönlü tünele iş yüklerini kapsüllemek için Internet anahtar değişimi (Ikev2). Ağ adresi çevirisi-çapraz geçiş (NAT-T) de desteklenir. |
| Bulut ağ geçidi (yerel) | Bulut ağ geçidi (uzak)  | UDP | 500 | IPSEC için gerekli<br> Çift yönlü tünel için Internet anahtar değişimi (ISAKMP). |
| Şirket içi vCenter ağı | Özel bulut yönetimi ağı | TCP | 8000 |  Şirket içi vCenter 'dan özel bulut vCenter 'a sanal makinelerin sanal hareketi   |     

## <a name="dhcp-and-dns-resolution-considerations"></a>DHCP ve DNS çözümleme konuları
Özel bir bulut ortamında çalışan uygulamalar ve iş yükleri, arama ve IP adresi ataması için ad çözümlemesi ve DHCP hizmetleri gerektirir. Bu hizmetleri sağlamak için uygun bir DHCP ve DNS altyapısı gereklidir. Bu hizmetleri özel bulut ortamınızda sağlamak için bir sanal makine yapılandırabilirsiniz.  

NSX ' te yerleşik olan DHCP hizmetini veya özel bulutta yerel bir DHCP sunucusunu kullanarak WAN 'dan şirket içine, WAN üzerinden yayın DHCP trafiği yönlendirmeyi kullanmanız önerilir.


## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide hakkında bilgi edindiniz:

> [!div class="checklist"]
> * Ağ bağlantısı gereksinimleri
> * Azure VMware çözümünde DHCP

Uygun ağa sahip olduğunuzda, Azure VMware çözümü özel bulutunuzu oluşturmak için sonraki öğreticiye geçin.

> [!div class="nextstepaction"]
> [Öğretici: Azure VMware çözümü özel bulutu oluşturma](tutorial-create-private-cloud.md)
