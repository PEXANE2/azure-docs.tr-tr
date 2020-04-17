---
title: 2 katmanlı uygulama ile hibrit bağlantı | Microsoft Dokümanlar
description: Azure'da çok katmanlı bir uygulama ortamı oluşturmak için sanal cihazları ve UDR'yi nasıl dağıtılayabilirsiniz öğrenin
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
ms.sourcegitcommit: b55d7c87dc645d8e5eb1e8f05f5afa38d7574846
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81455371"
---
# <a name="virtual-appliance-scenario"></a>Sanal cihaz senaryosu
Daha büyük Azure müşterisi arasında yaygın bir senaryo, internete açık iki katmanlı bir uygulama sağlama gereksinimi ve şirket içi bir veri merkezinden arka katmana erişime izin verme gereğidir. Bu belge, aşağıdaki gereksinimleri karşılayan iki katmanlı bir ortam dağıtmak için Kullanıcı Tanımlı Rotalar (UDR), vpn ağ geçidi ve ağ sanal cihazları kullanarak bir senaryo da size yol gösterecektir:

* Web uygulamasına yalnızca genel Internet'ten erişilebilmelidir.
* Uygulamayı barındıran web sunucusu bir arka uç uygulama sunucusuna erişebilmeli.
* Internet'ten web uygulamasına kadar olan tüm trafik bir güvenlik duvarı sanal cihazından geçmelidir. Bu sanal cihaz sadece Internet trafiği için kullanılacaktır.
* Uygulama sunucusuna giden tüm trafik bir güvenlik duvarı sanal cihaz geçmesi gerekir. Bu sanal cihaz arka uç sunucusuna erişim ve vpn ağ geçidi üzerinden şirket içi ağdan gelen erişim için kullanılacaktır.
* Yöneticiler, yalnızca yönetim amacıyla kullanılan üçüncü bir güvenlik duvarı sanal cihazını kullanarak güvenlik duvarı sanal cihazlarını şirket içi bilgisayarlarından yönetebilmeli.

Bu, DMZ ve korumalı ağ içeren standart bir çevre ağı (DMZ olarak da bilinir) senaryosudur. Bu tür senaryo, NSG'ler, güvenlik duvarı sanal cihazları veya her ikisinin bir birleşimi kullanılarak Azure'da oluşturulabilir. Aşağıdaki tabloda NSG'ler ve güvenlik duvarı sanal cihazları arasındaki bazı artıları ve eksileri gösterilmektedir.

|  | Artıları | Simgeler |
| --- | --- | --- |
| NSG |Bedeli yok. <br/>Azure RBAC'a entegre edilmiştir. <br/>Kurallar Azure Kaynak Yöneticisi şablonlarında oluşturulabilir. |Karmaşıklık daha büyük ortamlarda değişebilir. |
| Güvenlik duvarı |Veri düzlemi üzerinde tam kontrol. <br/>Güvenlik duvarı konsolu aracılığıyla merkezi yönetim. |Güvenlik duvarı cihazı nın maliyeti. <br/>Azure RBAC ile tümleşik değil. |

Aşağıdaki çözüm, çevre ağı (DMZ)/korumalı ağ senaryosunu uygulamak için güvenlik duvarı sanal cihazları kullanır.

## <a name="considerations"></a>Dikkat edilmesi gerekenler
Azure'da yukarıda açıklanan ortamı, bugün mevcut olan farklı özellikleri aşağıdaki gibi kullanarak dağıtabilirsiniz.

* **Sanal ağ (VNet)**. Azure VNet, şirket içi ağa benzer şekilde davranır ve trafik yalıtımı ve endişelerin ayrılmasını sağlamak için bir veya daha fazla alt ağa ayrılabilir.
* **Sanal cihaz**. Azure Marketi'nde birden çok iş ortağı, yukarıda açıklanan üç güvenlik duvarı için kullanılabilecek sanal cihazlar sağlar. 
* **Kullanıcı Tanımlı Rotalar (UDR)**. Rota tabloları, Bir VNet içindeki paketlerin akışını denetlemek için Azure ağı tarafından kullanılan ÜT'ler içerebilir. Bu rota tabloları alt ağlara uygulanabilir. Azure'daki en yeni özelliklerden biri, Azure VNet'e gelen tüm trafiği hibrit bir bağlantıdan sanal bir cihaza iletebilme olanağı sağlayan Bir rota tablosunu GatewaySubnet'e uygulayabilmektir.
* **IP Yönlendirme**. Varsayılan olarak, Azure ağ motoru paketleri sanal ağ arabirimi kartlarına (NICs) yalnızca paket hedef IP adresi NIC IP adresiyle eşleşirse iletir. Bu nedenle, bir UDR bir paketin belirli bir sanal cihaza gönderilmesi gerektiğini tanımlarsa, Azure ağ altyapısı bu paketi bırakır. Paketin paketin gerçek hedefi olmayan bir VM'ye (bu durumda sanal bir cihaz) teslim edilmesini sağlamak için, sanal cihaz için IP Yönlendirme'yi etkinleştirmeniz gerekir.
* **Ağ Güvenlik Grupları (NSGs)**. Aşağıdaki örnek, NSG'leri kullanmaz, ancak bu çözümde alt ağlara ve/veya NIC'lere uygulanan NSG'leri kullanarak bu alt ağlara ve NIC'lere daha fazla girip çıkan trafiği filtreleyebilirsiniz.

![IPv6 bağlantısı](./media/virtual-network-scenario-udr-gw-nva/figure01.png)

Bu örnekte aşağıdakileri içeren bir abonelik vardır:

* Diyagramda gösterilmeyen 2 kaynak grubu. 
  * **ONPREMRG**. Şirket içi ağı simüle etmek için gereken tüm kaynakları içerir.
  * **AZURERG**. Azure sanal ağ ortamı için gerekli tüm kaynakları içerir. 
* Aşağıda listelenmiş olarak bölümlenmiş şirket içi bir veri merkezini taklit etmek için **onpremvnet** adlı bir VNet kullanılır.
  * **onpremsn1**. Ubuntu'nun şirket içi bir sunucuya mişretmesi için çalışan sanal bir makine (VM) içeren alt ağ.
  * **onpremsn2**. Bir yönetici tarafından kullanılan şirket içi bilgisayarı taklit etmek için Ubuntu çalıştıran bir VM içeren alt ağ.
* **Onpremvnet'te** **OPFW** adlı bir güvenlik duvarı sanal cihazı **azurevnet**vardır.
* Aşağıda listelenen **azurevnet** adlı bir VNet segmente.
  * **azsn1**. Harici güvenlik duvarı alt ağı yalnızca harici güvenlik duvarı için kullanılır. Tüm internet trafiği bu alt ağ üzerinden gelecek. Bu alt ağ yalnızca harici güvenlik duvarına bağlı bir NIC içerir.
  * **azsn2**. Internet'ten erişilecek bir web sunucusu olarak çalışan bir VM barındıran ön uç alt ağ.
  * **azsn3**. Ön uç web sunucusu tarafından erişilecek bir arka uç uygulama sunucusu çalıştıran bir VM barındırma backend alt ağ.
  * **azsn4**. Yönetim alt ağı, yalnızca tüm güvenlik duvarı sanal cihazlarına yönetim erişimi sağlamak için kullanılır. Bu alt ağ yalnızca çözümde kullanılan her güvenlik duvarı sanal cihazı için bir NIC içerir.
  * **GatewaySubnet**. Azure VNet'ler ve diğer ağlar arasında bağlantı sağlamak için ExpressRoute ve VPN Ağ Geçidi için gereken Azure karma bağlantı alt ağı. 
* **Azurevnet** ağında 3 güvenlik duvarı sanal cihazı vardır. 
  * **AZF1**. Azure'da genel bir IP adresi kaynağı kullanılarak genel Internet'e maruz kalan harici güvenlik duvarı. Market'ten veya doğrudan cihaz satıcınızdan 3-NIC sanal cihaz içeren bir şablona sahip olduğunuzdan emin olmanız gerekir.
  * **AZF2**. İç güvenlik duvarı **azsn2** ve **azsn3**arasındaki trafiği kontrol etmek için kullanılır. Bu aynı zamanda 3-NIC sanal cihazdır.
  * **AZF3**. Yönetim güvenlik duvarı, şirket içi veri merkezinden yöneticilerin erişebileceği ve tüm güvenlik duvarı cihazlarını yönetmek için kullanılan bir yönetim alt ağına bağlı. Market'te 2-NIC sanal cihaz şablonları bulabilir veya doğrudan cihaz satıcınızdan bir tane isteyebilirsiniz.

## <a name="user-defined-routing-udr"></a>Kullanıcı Tanımlı Yönlendirme (UDR)
Azure'daki her alt ağ, bu alt ağda başlatılan trafiğin nasıl yönlendirildiğini tanımlamak için kullanılan bir UDR tablosuna bağlanabilir. Hiçbir URL tanımlanmamışsa, Azure trafiğin bir alt ağdan diğerine akmasına izin vermek için varsayılan yolları kullanır. ÜT'leri daha iyi anlamak için [Kullanıcı Tanımlı Rotalar ve IP Yönlendirme nedir'i](virtual-networks-udr-overview.md)ziyaret edin.

Yukarıdaki son gereksinime bağlı olarak doğru güvenlik duvarı cihazı üzerinden iletişimin yapıldığından emin olmak için **azurevnet'te**ÜR'ler içeren aşağıdaki rota tablosunu oluşturmanız gerekir.

### <a name="azgwudr"></a>azgwudr
Bu senaryoda, şirket içinde Azure'a akan tek **trafik, AZF3'e**bağlanarak güvenlik duvarlarını yönetmek için kullanılacak ve trafiğin dahili güvenlik duvarı **AZF2'den**geçmesi gerekir. Bu nedenle, **GatewaySubnet'te** aşağıda gösterildiği gibi yalnızca bir rota gereklidir.

| Hedef | Sonraki atlama | Açıklama |
| --- | --- | --- |
| 10.0.4.0/24 |10.0.3.11 |Şirket içi trafiğin yönetim güvenlik duvarı **AZF3'e** ulaşmasını sağlar |

### <a name="azsn2udr"></a>azsn2udr
| Hedef | Sonraki atlama | Açıklama |
| --- | --- | --- |
| 10.0.3.0/24 |10.0.2.11 |**AZF2** üzerinden uygulama sunucusu barındırma arka uç alt net trafik sağlar |
| 0.0.0.0/0 |10.0.2.10 |Diğer tüm trafiğin **AZF1** üzerinden yönlendirilmesine izin verir |

### <a name="azsn3udr"></a>azsn3udr
| Hedef | Sonraki atlama | Açıklama |
| --- | --- | --- |
| 10.0.2.0/24 |10.0.3.10 |**AZF2** üzerinden **azsn2'nin** uygulama sunucusundan web sunucusuna akmasını sağlar |

Ayrıca, şirket içi veri merkezini taklit etmek için **onpremvnet'teki** alt ağlar için rota tabloları oluşturmanız gerekir.

### <a name="onpremsn1udr"></a>onpremsn1udr
| Hedef | Sonraki atlama | Açıklama |
| --- | --- | --- |
| 192.168.2.0/24 |192.168.1.4 |**OPFW** üzerinden **onpremsn2** trafiğine izin verir |

### <a name="onpremsn2udr"></a>onpremsn2udr
| Hedef | Sonraki atlama | Açıklama |
| --- | --- | --- |
| 10.0.3.0/24 |192.168.2.4 |**OPFW** aracılığıyla Azure'da desteklenen alt ağa trafik sağlar |
| 192.168.1.0/24 |192.168.2.4 |**OPFW** üzerinden **onpremsn1** trafiğini sağlar |

## <a name="ip-forwarding"></a>IP İletimi
UDR ve IP Forwarding, bir Azure VNet'teki trafik akışını denetlemek için sanal cihazların kullanılmasına izin vermek için birlikte kullanabileceğiniz özelliklerdir.  Sanal gereç, güvenlik duvarı veya NAT cihazı gibi ağ trafiğini işlemek için kullanılan bir uygulamayı çalıştıran bir VM'den fazlası değildir.

Bu sanal gereç VM'si, kendisine yönelik olmayan gelen trafiği alabilmelidir. Bir VM'nin başka hedeflere yönelik trafiği alabilmesine izin vermek için VM'de IP İletimini etkinleştirmeniz gerekir. Bu ayar konuk işletim sisteminin değil, Azure'ın bir ayarıdır. Sanal cihazınızın gelen trafiği işlemek ve uygun şekilde yönlendirmek için bir tür uygulama yürütmesi gerekir.

IP Yönlendirme hakkında daha fazla bilgi edinmek için [Kullanıcı Tanımlı Rotalar ve IP Yönlendirme nedir'i ziyaret edin.](virtual-networks-udr-overview.md)

Örnek olarak, bir Azure vnet'te aşağıdaki kuruluma sahip olduğunuzu düşünün:

* Subnet **onpremsn1** **onpremvm1**adlı bir VM içerir.
* Subnet **onpremsn2** **onpremvm2**adlı bir VM içerir.
* **OPFW** adlı bir sanal cihaz **onpremsn1** ve **onpremsn2'ye**bağlanır.
* **onpremsn1'e** bağlı kullanıcı tanımlı bir rota, **onpremsn2'ye** giden tüm trafiğin **OPFW'ye**gönderilmesi gerektiğini belirtir.

Bu noktada, **onpremvm1** **onpremvm2**ile bağlantı kurmaya çalışırsa, UDR kullanılacak ve trafik bir sonraki atlama olarak **OPFW** gönderilecektir. Gerçek paket hedef değiştirilmediğini unutmayın, hala **onpremvm2** hedef olduğunu söylüyor. 

**OPFW**için IP Yönlendirme etkin olmadan, Azure sanal ağ mantığı paketleri düşürür, çünkü paketlerin yalnızca VM'nin IP adresi paketin hedefiyse VM'ye gönderilmesine izin verir.

IP Forwarding ile Azure sanal ağ mantığı, paketleri orijinal hedef adresini değiştirmeden OPFW'ye iletir. **OPFW** paketleri işlemeli ve bunlarla ne yapacağını belirlemelidir.

Yukarıdaki senaryonun çalışması için, yönlendirme için kullanılan **OPFW,** **AZF1,** **AZF2**ve **AZF3** (yönetim alt ağına bağlı olanlar hariç tüm NIC'ler) için NIC'lerde IP Yönlendirme'yi etkinleştirmeniz gerekir. 

## <a name="firewall-rules"></a>Güvenlik Duvarı Kuralları
Yukarıda açıklandığı gibi, IP Forwarding yalnızca paketlerin sanal cihazlara gönderilmesini sağlar. Cihazınızın hala bu paketlerle ne yapacağına karar vermesi gerekiyor. Yukarıdaki senaryoda, cihazlarınızda aşağıdaki kuralları oluşturmanız gerekir:

### <a name="opfw"></a>OPFW
OPFW, aşağıdaki kuralları içeren şirket içi bir aygıtı temsil eder:

* **Rota**: 10.0.0.0/16 **(azurevnet)** tüm trafik tünel **ONPREMAZURE**üzerinden gönderilmelidir.
* **İlke**: **Port2** ve **ONPREMAZURE**arasındaki tüm çift yönlü trafiğe izin verin.

### <a name="azf1"></a>AZF1
AZF1, aşağıdaki kuralları içeren bir Azure sanal cihazını temsil eder:

* **İlke**: **Port1** ve **port2**arasındaki tüm çift yönlü trafiğe izin verin.

### <a name="azf2"></a>AZF2
AZF2, aşağıdaki kuralları içeren bir Azure sanal cihazını temsil eder:

* **Rota**: 10.0.0.0/16 'ya giden tüm trafik **(onpremvnet)** **port1**üzerinden Azure ağ geçidi IP adresine (yani 10.0.0.0.1) gönderilmelidir.
* **İlke**: **Port1** ve **port2**arasındaki tüm çift yönlü trafiğe izin verin.

## <a name="network-security-groups-nsgs"></a>Ağ Güvenlik Grupları (NSGs)
Bu senaryoda, NSG'ler kullanılmamaktadır. Ancak, gelen ve giden trafiği kısıtlamak için her alt ağa NSG uygulayabilirsiniz. Örneğin, aşağıdaki NSG kurallarını harici FW alt ağına uygulayabilirsiniz.

**Gelen**

* Internet'teki tüm TCP trafiğinin alt ağdaki herhangi bir VM'de 80 bağlantı noktasına girmesine izin verin.
* Internet'ten diğer tüm trafiği reddedin.

**Giden**

* Internet'e olan tüm trafiği reddedin.

## <a name="high-level-steps"></a>Üst düzey adımlar
Bu senaryoyu dağıtmak için aşağıdaki üst düzey adımları izleyin.

1. Azure Aboneliğinize giriş yapın.
2. Şirket içi ağı taklit etmek için bir VNet dağıtmak istiyorsanız, **ONPREMRG'nin**bir parçası olan kaynakları sağlama.
3. **AZURERG'nin**bir parçası olan kaynakları sağlama.
4. **Onpremvnet'ten** **azurevnet'e**tünel sağlama .
5. Tüm kaynaklar sağlandıktan sonra, **onpremsn2** ve **azsn3**arasındaki bağlantıyı test etmek için **onpremvm2'ye** oturum açın ve 10.0.3.101 ping ping.

