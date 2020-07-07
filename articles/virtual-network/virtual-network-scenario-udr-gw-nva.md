---
title: 2 katmanlı uygulamayla karma bağlantı | Microsoft Docs
description: Azure 'da çok katmanlı bir uygulama ortamı oluşturmak için sanal gereçler ve UDR 'yi dağıtmayı öğrenin
services: virtual-network
documentationcenter: na
author: KumudD
manager: carmonm
ms.assetid: 1f509bec-bdd1-470d-8aa4-3cf2bb7f6134
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/05/2016
ms.author: kumud
ms.openlocfilehash: 80a9397838e90a2af504125b2dc4c4ef39251d4e
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "81455371"
---
# <a name="virtual-appliance-scenario"></a>Sanal Gereç senaryosu
Daha büyük Azure müşterisi arasındaki yaygın bir senaryo, Internet 'e açık iki katmanlı bir uygulama sağlamak ve şirket içi veri merkezinden arka katmana erişime izin vermek için gereklidir. Bu belge, aşağıdaki gereksinimleri karşılayan iki katmanlı bir ortam dağıtmak için Kullanıcı tanımlı yollar (UDR), VPN Gateway ve ağ sanal gereçlerini kullanan bir senaryoda size kılavuzluk eder:

* Web uygulamasına yalnızca genel Internet 'ten erişilebilir olması gerekir.
* Uygulamayı barındıran Web sunucusu, bir arka uç uygulama sunucusuna erişebilmelidir.
* Internet 'ten Web uygulamasına giden tüm trafik, bir güvenlik duvarı sanal gereci üzerinden gitmelidir. Bu Sanal Gereç yalnızca Internet trafiği için kullanılacaktır.
* Uygulama sunucusuna giden tüm trafik, bir güvenlik duvarı sanal gereci üzerinden gitmelidir. Bu sanal gereç, arka uç uç sunucusuna erişim ve VPN Gateway aracılığıyla şirket içi ağdan gelen erişim için kullanılacaktır.
* Yöneticiler, yönetim amaçları için özel olarak kullanılan üçüncü bir güvenlik duvarı sanal gereci kullanarak, güvenlik duvarı sanal gereçlerini şirket içi bilgisayarlarından yönetebilmelidir.

Bu, DMZ ve korumalı bir ağla standart bir çevre ağı (Ayrıca, DMZ olarak da MNS olarak da tasarlanmıştır) senaryosudur. Bu tür senaryolar NSG 'ler, güvenlik duvarı sanal cihazları veya her ikisinin bir birleşimi kullanılarak Azure 'da oluşturulabilir. Aşağıdaki tabloda, NSG 'ler ve güvenlik duvarı sanal cihazları arasındaki bazı olumlu ve olumsuz yönleri gösterilmektedir.

|  | Artıları | Simgeler |
| --- | --- | --- |
| NSG |Maliyet yok. <br/>Azure RBAC Ile tümleşiktir. <br/>Kurallar, Azure Resource Manager şablonlarda oluşturulabilir. |Karmaşıklık, daha büyük ortamlarda farklılık gösterebilir. |
| Güvenlik Duvarı |Veri düzlemi üzerinde tam denetim. <br/>Güvenlik Duvarı konsolu aracılığıyla merkezi yönetim. |Güvenlik Duvarı gerecinin maliyeti. <br/>Azure RBAC ile tümleştirildi. |

Aşağıdaki çözüm, bir çevre ağı (DMZ)/korumalı ağ senaryosu uygulamak için güvenlik duvarı sanal gereçlerini kullanır.

## <a name="considerations"></a>Önemli noktalar
Yukarıda açıklanan ortamı, aşağıdaki gibi, günümüzde bulunan farklı özellikleri kullanarak Azure 'da dağıtabilirsiniz.

* **Sanal ağ (VNet)** . Bir Azure sanal ağı, şirket içi bir ağa benzer şekilde davranır ve trafik yalıtımı sağlamak ve kaygıları ayrımı için bir veya daha fazla alt ağa ayrılabilir.
* **Sanal Gereç**. Birçok iş ortağı, yukarıda açıklanan üç güvenlik duvarı için kullanılabilen Azure Marketi 'nde sanal gereçler sağlar. 
* **Kullanıcı tanımlı yollar (UDR)**. Rota tabloları, bir sanal ağ içindeki paketlerin akışını denetlemek için Azure ağı tarafından kullanılan UDRs 'yi içerebilir. Bu rota tabloları alt ağlara uygulanabilir. Azure 'daki en yeni özelliklerden biri GatewaySubnet 'e bir rota tablosu uygulama olanağından Azure VNet 'e gelen tüm trafiği bir karma bağlantıdan sanal bir gereç ile iletmenize olanak sağlar.
* **IP iletimi**. Varsayılan olarak, Azure ağ altyapısı, paketleri yalnızca paket hedef IP adresi NIC IP adresiyle eşleşiyorsa sanal ağ arabirim kartlarına (NIC 'ler) iletir. Bu nedenle, bir UDR bir paketin belirli bir sanal gerece gönderilmesi gerektiğini tanımlıyorsa, Azure ağ altyapısı bu paketi düşürüyordu. Paketin, paketin gerçek hedefi olmayan bir VM 'ye (Bu durumda bir Sanal Gereç) teslim edildiğinden emin olmak için Sanal Gereç için IP Iletmeyi etkinleştirmeniz gerekir.
* **Ağ güvenlik grupları (NSG 'ler)**. Aşağıdaki örnekte NSG 'ler kullanılmaz, ancak bu alt ağlar ve NIC 'lerde gelen ve giden trafiği daha fazla filtrelemek için bu çözümdeki alt ağlara ve/veya NIC 'lere uygulanan NSG 'ler kullanabilirsiniz.

![IPv6 bağlantısı](./media/virtual-network-scenario-udr-gw-nva/figure01.png)

Bu örnekte, aşağıdakileri içeren bir abonelik vardır:

* 2 kaynak grubu diyagramda gösterilmez. 
  * **Onpremrg**. Şirket içi bir ağın benzetimini yapmak için gereken tüm kaynakları içerir.
  * **AZURERG**. Azure sanal ağ ortamı için gereken tüm kaynakları içerir. 
* Aşağıda listelenen bir şirket içi veri merkezini taklit etmek için kullanılan **onpremvnet** adlı VNET.
  * **onpremsn1**. Ubuntu çalıştıran ve şirket içi bir sunucuyu taklit eden bir sanal makine (VM) içeren alt ağ.
  * **onpremsn2**. Bir yönetici tarafından kullanılan şirket içi bir bilgisayarı taklit etmek için Ubuntu çalıştıran bir VM içeren alt ağ.
* **Azurevnet**için bir tüneli sürdürmek üzere kullanılan **onpremvnet** üzerinde **opfw** adlı bir güvenlik duvarı sanal gereci vardır.
* Aşağıda listelenen **azurevnet** adlı bir sanal ağ.
  * **azsn1**. Harici güvenlik duvarı için özel olarak kullanılan dış güvenlik duvarı alt ağı. Tüm Internet trafiği bu alt ağ üzerinden gönderilir. Bu alt ağ yalnızca dış güvenlik duvarıyla bağlantılı bir NIC içerir.
  * **azsn2**. Internet 'ten erişilecek bir Web sunucusu olarak çalışan bir VM barındıran ön uç alt ağı.
  * **azsn3**. Ön uç Web sunucusu tarafından erişilecek bir arka uç uygulama sunucusu çalıştıran bir VM barındıran arka uç alt ağı.
  * **azsn4**. Tüm güvenlik duvarı sanal gereçlerine yönetim erişimi sağlamak için özel olarak kullanılan yönetim alt ağı. Bu alt ağ, çözümde kullanılan her güvenlik duvarı sanal gereci için bir NIC içerir.
  * **Gatewaysubnet**. Azure sanal ağları ve diğer ağlar arasında bağlantı sağlamak için ExpressRoute ve VPN Gateway için Azure hibrit bağlantı alt ağı gerekir. 
* **Azurevnet** ağında 3 güvenlik duvarı sanal aygıtı vardır. 
  * **AZF1**. Azure 'da genel bir IP adresi kaynağı kullanarak genel Internet 'e sunulan dış güvenlik duvarı. Market 'ten veya doğrudan gereç satıcınızdan (3 NIC Sanal Gereç sağlayan) bir şablonunuz olduğundan emin olmanız gerekir.
  * **AZF2**. **Azsn2** ve **azsn3**arasındaki trafiği denetlemek için kullanılan iç güvenlik duvarı. Bu, ayrıca 3 NIC sanal gerectir.
  * **AZF3**. Şirket içi veri merkezinden yöneticiler tarafından erişilebilen yönetim güvenlik duvarı ve tüm güvenlik duvarı gereçlerini yönetmek için kullanılan bir yönetim alt ağına bağlı. Market 'te 2 NIC Sanal Gereç şablonları bulabilir veya doğrudan gereç satıcınızdan bir tane talep edebilirsiniz.

## <a name="user-defined-routing-udr"></a>Kullanıcı tanımlı yönlendirme (UDR)
Azure 'daki her alt ağ, bu alt ağda başlatılan trafiğin nasıl yönlendirildiğini tanımlamak için kullanılan bir UDR tablosuna bağlanabilir. UDRs tanımlanmazsa Azure, trafiğin bir alt ağdan diğerine akmasını sağlamak için varsayılan yolları kullanır. UDRs 'yi daha iyi anlamak için [Kullanıcı tanımlı yollar ve IP Iletimi nedir?](virtual-networks-udr-overview.md)sayfasını ziyaret edin.

Yukarıdaki son gereksinime göre doğru güvenlik duvarı gereci üzerinden iletişimin yapıldığından emin olmak için, **azurevnet**Içinde udrs 'yi içeren aşağıdaki yol tablosunu oluşturmanız gerekir.

### <a name="azgwudr"></a>azgwudr
Bu senaryoda, Şirket içinden Azure 'a akan tek trafik, **AZF3**adresine bağlanarak güvenlik duvarlarını yönetmek için kullanılır ve bu trafiğin iç güvenlik duvarı, **AZF2**ile geçmesi gerekir. Bu nedenle, **Gatewaysubnet** içinde aşağıda gösterildiği gibi yalnızca bir yol gereklidir.

| Hedef | Sonraki atlama | Açıklama |
| --- | --- | --- |
| 10.0.4.0/24 |10.0.3.11 |Şirket içi trafiğin Yönetim güvenlik duvarı 'na ulaşmasını sağlar **AZF3** |

### <a name="azsn2udr"></a>azsn2udr
| Hedef | Sonraki atlama | Açıklama |
| --- | --- | --- |
| 10.0.3.0/24 |10.0.2.11 |**AZF2** aracılığıyla uygulama sunucusunu barındıran arka uç alt ağına giden trafiğe izin verir |
| 0.0.0.0/0 |10.0.2.10 |Diğer tüm trafiğin **AZF1** aracılığıyla yönlendirilmesine izin verir |

### <a name="azsn3udr"></a>azsn3udr
| Hedef | Sonraki atlama | Açıklama |
| --- | --- | --- |
| 10.0.2.0/24 |10.0.3.10 |**Azsn2** **ile uygulama** sunucusundan Web sunucusuna Flow 'a giden trafiğe izin verir |

Şirket içi veri merkezini taklit etmek için **onpremvnet** içindeki alt ağlar için de rota tabloları oluşturmanız gerekir.

### <a name="onpremsn1udr"></a>onpremsn1udr
| Hedef | Sonraki atlama | Açıklama |
| --- | --- | --- |
| 192.168.2.0/24 |192.168.1.4 |**Opfw** aracılığıyla **onpremsn2** 'e trafik sağlar |

### <a name="onpremsn2udr"></a>onpremsn2udr
| Hedef | Sonraki atlama | Açıklama |
| --- | --- | --- |
| 10.0.3.0/24 |192.168.2.4 |**Opfw** aracılığıyla Azure 'da desteklenen alt ağa giden trafiğe izin verir |
| 192.168.1.0/24 |192.168.2.4 |**Opfw** aracılığıyla **onpremsn1** 'e trafik sağlar |

## <a name="ip-forwarding"></a>IP İletimi
UDR ve IP Iletimi, sanal gereçlerin bir Azure VNet 'te trafik akışını denetlemek için kullanılmasına izin vermek üzere kullanabileceğiniz özelliklerdir.  Sanal gereç, güvenlik duvarı veya NAT cihazı gibi ağ trafiğini işlemek için kullanılan bir uygulamayı çalıştıran bir VM'den fazlası değildir.

Bu sanal gereç VM'si, kendisine yönelik olmayan gelen trafiği alabilmelidir. Bir VM'nin başka hedeflere yönelik trafiği alabilmesine izin vermek için VM'de IP İletimini etkinleştirmeniz gerekir. Bu ayar konuk işletim sisteminin değil, Azure'ın bir ayarıdır. Sanal gerecinizin yine de gelen trafiği işlemek için bazı tür uygulamaları çalıştırması ve bunu uygun şekilde yönlendirmesi gerekir.

IP Iletimi hakkında daha fazla bilgi edinmek için [Kullanıcı tanımlı yollar ve IP Iletimi nedir?](virtual-networks-udr-overview.md)sayfasını ziyaret edin.

Örnek olarak, bir Azure VNET 'te aşağıdaki kuruluma sahip olduğunu düşünün:

* **Onpremsn1** alt ağı **ONPREMVM1**adlı bir VM içerir.
* **Onpremsn2** alt ağı **ONPREMVM2**adlı bir VM içerir.
* **Opfw** adlı sanal gereç **onpremsn1** ve **onpremsn2**'e bağlıdır.
* **Onpremsn1** ile bağlantılı Kullanıcı tanımlı yol, **onpremsn2** 'e giden tüm trafiğin **opfw**'a gönderilmesi gerektiğini belirtir.

Bu noktada, **onpremvm1** **onpremvm2**ile bir bağlantı kurmaya çalışırsa UDR kullanılır ve sonraki atlama olarak **opfw** 'e trafik gönderilir. Gerçek paket hedefinin değiştirilmediğinden emin olmak için hedefin **onpremvm2** olduğunu göz önünde bulundurun. 

**Opfw**Için IP iletimi etkinleştirilmeden, Azure sanal ağ mantığı paketleri bırakacak, çünkü VM 'nin IP adresi paketin hedefi ise, yalnızca PAKETLERIN bir VM 'ye gönderilmesine izin verir.

IP Iletimi ile Azure sanal ağ mantığı, özgün hedef adresini değiştirmeden paketleri OPFW 'e iletir. **Opfw** paketleri işlemeli ve bunlarla ne yapılacağını belirlemelidir.

Yukarıdaki senaryonun çalışması için, **yönlendirme için kullanılan** **AZF1**, **AZF2**ve **AZF3** için NIC 'lerde IP iletmeyi etkinleştirmeniz gerekir (Yönetim alt ağına bağlı olanlar hariç tüm NIC 'ler). 

## <a name="firewall-rules"></a>Güvenlik Duvarı Kuralları
Yukarıda açıklandığı gibi, IP Iletimi yalnızca paketlerin sanal gereçlere gönderilmesini sağlar. Gerecinizin yine de bu paketlerle ne yapacağına karar verilmesi gerekiyor. Yukarıdaki senaryoda, gereçleriniz için aşağıdaki kuralları oluşturmanız gerekir:

### <a name="opfw"></a>OPFW
OPFW aşağıdaki kuralları içeren bir şirket içi cihazı temsil eder:

* **Yol**: 10.0.0.0/16 (**azurevnet**) öğesine giden tüm trafik, **onprelaure**ile gönderilmelidir.
* **İlke**: **PORT2** Ile **onprelaure**arasındaki tüm çift yönlü trafiğe izin verin.

### <a name="azf1"></a>AZF1
AZF1 aşağıdaki kuralları içeren bir Azure sanal gerecini temsil eder:

* **İlke**: **PORT1** ve **PORT2**arasındaki tüm çift yönlü trafiğe izin ver.

### <a name="azf2"></a>AZF2
AZF2 aşağıdaki kuralları içeren bir Azure sanal gerecini temsil eder:

* **Yol**: 10.0.0.0/16 (**onpremvnet**) öğesine giden tüm trafik, **PORT1**üzerinden Azure Gateway IP adresine (10.0.0.1) gönderilmelidir.
* **İlke**: **PORT1** ve **PORT2**arasındaki tüm çift yönlü trafiğe izin ver.

## <a name="network-security-groups-nsgs"></a>Ağ güvenlik grupları (NSG 'ler)
Bu senaryoda NSG 'ler kullanılmıyor. Ancak, gelen ve giden trafiği kısıtlamak için her alt ağa NSG 'ler uygulayabilirsiniz. Örneğin, dış FW alt ağına aşağıdaki NSG kurallarını uygulayabilirsiniz.

**Gelen**

* Internet 'ten gelen tüm TCP trafiğine alt ağdaki herhangi bir VM üzerinde 80 numaralı bağlantı noktasına izin verin.
* Internet 'ten gelen diğer tüm trafiği reddedin.

**Tarafına**

* Internet 'e giden tüm trafiği reddedin.

## <a name="high-level-steps"></a>Üst düzey adımlar
Bu senaryoyu dağıtmak için aşağıdaki üst düzey adımları izleyin.

1. Azure aboneliğinizde oturum açın.
2. Şirket içi ağı taklit etmek için bir VNet dağıtmak istiyorsanız **Onpremrg**'nin parçası olan kaynakları sağlayın.
3. **AZURERG**'in parçası olan kaynakları sağlayın.
4. **Onpremvnet** 'ten **azurevnet**'e tünel sağlayın.
5. Tüm kaynaklar sağlandıktan sonra **onpremsn2** ve **azsn3**arasındaki bağlantıyı sınamak için **onpremvm2** ve ping 10.0.3.101 ' de oturum açın.

