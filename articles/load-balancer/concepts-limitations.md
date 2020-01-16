---
title: Components and Limitations
titleSuffix: Azure Load Balancer
description: Overview of Azure Load Balancer components and limitations.
services: load-balancer
documentationcenter: na
author: asudbring
ms.service: load-balancer
Customer intent: As an IT administrator, I want to learn more about the Azure Load Balancer components and limitations and how it will affect my environment.
ms.devlang: na
ms.topic: overview
ms.custom: seodec18
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/14/2019
ms.author: allensu
ms.openlocfilehash: 93fce95ad73f5e93afdbb794af3279a35243e2e2
ms.sourcegitcommit: 05cdbb71b621c4dcc2ae2d92ca8c20f216ec9bc4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/16/2020
ms.locfileid: "76046257"
---
# <a name="load-balancer-components-and-limitations"></a>Load Balancer components and limitations
Azure Load Balancer contains several key components for it's operation.  These components can be configured in your subscription via the Azure portal, Azure CLI, or Azure PowerShell.  

## <a name="load-balancer-components"></a>Load Balancer components

* **Frontend IP configurations**: The IP address of the load balancer. It's the point of contact for clients. These addresses can be either: 

    - **[Public IP Address](https://docs.microsoft.com/azure/virtual-network/virtual-network-public-ip-address)**
    - **[Private IP Address](https://docs.microsoft.com/azure/virtual-network/virtual-network-ip-addresses-overview-arm#private-ip-addresses)**

* **Backend pool**: The group of virtual machines or instances in the Virtual Machine Scale Set that are going to serve the incoming request. To scale cost-effectively to meet high volumes of incoming traffic, computing guidelines generally recommend adding more instances to the backend pool. Load balancer instantly reconfigures itself via automatic reconfiguration when you scale instances up or down. Adding or removing VMs from the backend pool reconfigures the load balancer without additional operations on the load balancer resource.
* **Health probes**: A **[health probe](https://docs.microsoft.com/azure/load-balancer/load-balancer-custom-probe-overview)** is used to determine the health of the instances in the backend pool. You can define the unhealthy threshold for your health probes. Bir yoklama yanıt veremediğinde, Load Balancer iyi durumda olmayan örneklere yeni bağlantı göndermeyi durdurur. A probe failure doesn't affect existing connections. 
    
    The connection continues until the application: 
    - Ends the flow
    - Idle timeout occurs
    - The VM shuts down

    Load Balancer provides different health probe types for endpoints:
    - TCP
    - HTTP
    - HTTPS
     
    For more information, see [Probe types](load-balancer-custom-probe-overview.md#types).

* **Load-balancing rules**: Load-Balancing rules are the ones that tell the Load Balancer what needs to be done when. 
* **Inbound NAT rules**: An Inbound NAT rule forwards traffic from a specific port of a specific frontend IP address to a specific port of a specific backend instance inside the virtual network. **[Port forwarding](https://docs.microsoft.com/azure/load-balancer/tutorial-load-balancer-port-forwarding-portal)** is done by the same hash-based distribution as load balancing. Common scenarios for this capability are Remote Desktop Protocol (RDP) or Secure Shell (SSH) sessions to individual VM instances inside an Azure Virtual Network. You can map multiple internal endpoints to ports on the same front-end IP address. You can use the front-end IP addresses to remotely administer your VMs without an additional jump box.
* **Outbound rules**: An **[outbound rule](https://docs.microsoft.com/azure/load-balancer/load-balancer-outbound-rules-overview)** configures outbound Network Address Translation (NAT) for all virtual machines or instances identified by the backend pool to be translated to the frontend.

![Azure Load Balancer](./media/load-balancer-overview/load-balancer-overview.png)

## <a name = "load-balancer-concepts"></a>Load Balancer concepts

Load Balancer, TCP ve UDP uygulamaları için aşağıdaki temel özellikleri sunar:

* **Load-balancing algorithm**: With Azure Load Balancer, you can create a load-balancing rule to distribute traffic that arrives at the frontend to backend pool instances. Load Balancer, gelen akışların dağıtılması için bir karma algoritması kullanır ve akışların üst bilgilerini arka uç havuz örneklerine yeniden yazar. Bir sistem durumu araştırması sağlıklı bir arka uç bitiş noktasını gösterdiğinde yeni akışlar almak için bir sunucu kullanılabilir.
Varsayılan olarak, Load Balancer 5 demet karma kullanır. 

   Karma değeri şunları içerir: 

   - **Kaynak IP adresi**
   - **Kaynak bağlantı noktası**
   - **Hedef IP adresi**
   - **Hedef bağlantı noktası**
   - **Akışların kullanılabilir sunucularla eşlenecek IP protokol numarası** 

Belirli bir kural için 2 veya 3 demet karma kullanarak bir kaynak IP adresine benzeşim oluşturabilirsiniz. Aynı paket akışının tüm paketleri, yük dengeleme uygulanan ön ucun arkasındaki aynı örneğe ulaşır. İstemci aynı kaynak IP 'den yeni bir akış başlattığında, kaynak bağlantı noktası değiştirilir. Sonuç olarak, 5 demet karma, trafiğin farklı bir arka uç uç noktasına geçmesine neden olabilir.
Daha fazla bilgi için bkz. [Azure Load Balancer Dağıtım modunu yapılandırma](./load-balancer-distribution-mode.md). 

Aşağıdaki görüntüde karma tabanlı dağıtım gösterilmiştir:

  ![Karma tabanlı dağıtım](./media/load-balancer-overview/load-balancer-distribution.png)

  *Şekil: Karma tabanlı dağıtım*

* **Uygulama bağımsızlık ve saydamlık**: Load Balancer doğrudan TCP veya UDP veya uygulama katmanıyla etkileşime girmez. Herhangi bir TCP veya UDP uygulama senaryosu desteklenebilir. Load Balancer, akışların sonlandırılması veya kaynaklanmaz, akışın yüküyle etkileşime geçebilir veya herhangi bir uygulama katmanı ağ geçidi işlevi sağlar. Protokol el sıkışmaları her zaman doğrudan istemci ile arka uç havuzu örneği arasında gerçekleşir. Gelen akışa verilen yanıt her zaman sanal makineden gelen yanıttır. Akış sanal makineye ulaştığında özgün kaynak IP adresi de korunur.
  * Her uç noktaya yalnızca bir VM tarafından yanıt verilir. Örneğin, istemci ve seçilen arka uç VM arasında her zaman bir TCP el sıkışması meydana gelir. Ön uca bir istek yanıtı, arka uç VM tarafından oluşturulan bir yanıttır. Bir ön uca bağlantıyı başarıyla doğruladığınızda, en az bir adet arka uç sanal makinesine uçtan uca bağlantıyı doğrularsınız.
  * Uygulama yükleri Load Balancer için saydamdır. Herhangi bir UDP veya TCP uygulaması desteklenebilir.
  * Load Balancer TCP yükü ile etkileşimde olmadığından ve TLS yük boşaltma sağladığından uçtan uca şifrelenmiş senaryolar oluşturabilirsiniz. Load Balancer kullanımı, sanal makine üzerindeki TLS bağlantısını sonlandırarak TLS uygulamaları için büyük ölçekte genişleme sağlar. Örneğin, TLS oturum anahtar kapasiteniz yalnızca, arka uç havuzuna eklediğiniz VM 'lerin türü ve sayısıyla sınırlıdır.

* **Giden bağlantılar (SNAT)** : sanal ağınız IÇINDEKI özel IP adreslerinden gelen tüm giden akışlar, Load Balancer ön uç IP adresine çevrilebilir. Bir genel ön uç, bir yük dengeleme kuralı aracılığıyla arka uç VM 'sine bağlı olduğunda, Azure giden bağlantıları genel ön uç IP adresine çevirir. Bu yapılandırmanın avantajları şunlardır:
  * Basit yükseltme ve olağanüstü durum kurtarma, ön uç, hizmetin başka bir örneğine dinamik olarak eşlenmeye olanak sağlar.
  * Daha kolay erişim denetim listesi (ACL) yönetimi. Hizmet ölçeği artırma veya azaltma ya da yeniden dağıtım gibi, ön uç IP 'ler olarak ifade edilen ACL 'Ler değişmez. Giden bağlantıların makinelerden daha az sayıda IP adresine dönüştürülmesi, güvenli alıcı listelerinin uygulanması yükünü azaltır.
Daha fazla bilgi için bkz. [Azure 'Da giden bağlantılar](load-balancer-outbound-connections.md).
Standart Load Balancer, aşağıda açıklandığı gibi bu temellerin ötesinde ek SKU 'ya özgü yetenekler içerir.

## <a name="load-balancer-types"></a>Load Balancer türleri

### <a name = "publicloadbalancer"></a>Genel Load Balancer

Ortak Load Balancer, gelen trafiğin genel IP adresini ve bağlantı noktasını, sanal makinenin özel IP adresine ve bağlantı noktasına eşler. Load Balancer trafiği VM 'den gelen yanıt trafiği için başka bir şekilde eşler. Yük Dengeleme kuralları uygulayarak, belirli trafik türlerini birden çok VM veya hizmet arasında dağıtabilirsiniz. Örneğin web isteği trafiğinin yükünü birden fazla web sunucusuna dağıtabilirsiniz.

>[!NOTE]
>Her kullanılabilirlik kümesi için yalnızca bir ortak Load Balancer ve bir iç Load Balancer uygulayabilirsiniz.

Aşağıdaki şekilde, genel ve TCP bağlantı noktası 80 için üç VM arasında paylaşılan web trafiği için yük dengeli bir uç nokta gösterilmektedir. Bu üç VM, bir yük dengeleme kümesinde bulunur.

![Genel Load Balancer örneği](./media/load-balancer-overview/IC727496.png)

*Şekil: genel Load Balancer kullanarak Web trafiğini Dengeleme*

Internet istemcileri, 80 numaralı TCP bağlantı noktasındaki bir Web uygulamasının genel IP adresine Web sayfası istekleri gönderir. Azure Load Balancer, istekleri yük dengeli küme içindeki üç sanal makineye dağıtır. Load Balancer algoritmaları hakkında daha fazla bilgi için bkz. [Load Balancer kavramlar](concepts-limitations.md#load-balancer-concepts).

Azure Load Balancer, ağ trafiğini varsayılan olarak birden çok VM örneği arasında eşit olarak dağıtır. Oturum benzeşimini de yapılandırabilirsiniz. Daha fazla bilgi için bkz. [Azure Load Balancer Dağıtım modunu yapılandırma](load-balancer-distribution-mode.md).

### <a name = "internalloadbalancer"></a> İç Load Balancer

İç yük dengeleyici, trafiği yalnızca bir sanal ağ içinde olan veya Azure altyapısına erişmek için bir VPN kullanan veya bir genel yük dengeleyicisine karşılık gelen kaynaklara yönlendirir. Azure altyapısı, bir sanal ağın yük dengeli ön uç IP adreslerine erişimi kısıtlar. Ön uç IP adresleri ve sanal ağlar hiçbir şekilde doğrudan bir internet uç noktasına gösterilmez. İç iş kolu uygulamaları Azure'da çalışır ve Azure'dan veya şirket içi kaynaklardan erişim sağlanır.

İç Load Balancer, aşağıdaki yük dengeleme türlerini destekler:

* **Bir sanal ağ içinde**: sanal ağdaki VM 'lerden, aynı sanal ağda bulunan bir VM kümesine yük dengeleme.
* **Şirketler arası sanal ağ için**: şirket içi bilgisayarlardan aynı sanal ağdaki bir VM kümesine yük dengeleme.
* **Çok katmanlı uygulamalar için**: arka uç katmanlarının internet 'e açık olmadığı internet 'e yönelik çok katmanlı uygulamalar için yük dengeleme. Arka uç katmanları, internet 'e yönelik katmandan trafik yük dengelemesi gerektirir. Sonraki şekle bakın.
* **İş kolu uygulamaları için**: Ek yük dengeleyici donanım veya yazılım olmadan Azure'da barındırılan iş kolu uygulamaları için yük dengeleme. Bu senaryo, trafiği yük dengeli olan bilgisayar kümesinde bulunan şirket içi sunucuları içerir.

![İç Load Balancer örneği](./media/load-balancer-overview/IC744147.png)

*Şekil: çok katmanlı uygulamaları hem genel hem de dahili Load Balancer kullanarak Dengeleme*

## <a name="skus"></a> Load Balancer SKU karşılaştırması

Yük dengeleyici hem temel hem de standart SKU 'Ları destekler. Bu SKU 'Lar senaryo ölçeğinde, özelliklerde ve fiyatlandırmaya göre farklılık gösterir. Temel Load Balancer olabilecek herhangi bir senaryo, Standart Load Balancer ile oluşturulabilir. Her iki SKU için de API 'Ler benzerdir ve bir SKU 'nun belirtimine göre çağrılır. Yük Dengeleyici için SKU 'Ları destekleme API 'SI ve genel IP, `2017-08-01` API 'siyle başlayarak kullanılabilir. Her iki SKU da aynı genel API ve yapıyı paylaşır.

Tüm senaryo yapılandırması, SKU 'ya bağlı olarak biraz farklı görünebilir. Bir makale yalnızca belirli bir SKU için geçerliyse, yük dengeleyici belgeleri çağırır. Farkları karşılaştırmak ve anlamak için aşağıdaki tabloya bakın. Daha fazla bilgi için bkz. [Azure Standart Load Balancer genel bakış](load-balancer-standard-overview.md).

>[!NOTE]
> Microsoft Standart Load Balancer önerir.
Tek başına VM'ler, kullanılabilirlik kümeleri ve sanal makine ölçek kümeleri yalnızca tek bir SKU'ya bağlanabilir, ikisine birden bağlanamaz. Load Balancer ve genel IP adresi SKU 'SU ortak IP adresleriyle kullandığınızda aynı olmalıdır. Load Balancer ve genel IP SKU 'Ları değişebilir değildir.

[!INCLUDE [comparison table](../../includes/load-balancer-comparison-table.md)]

Daha fazla bilgi için bkz. [yük dengeleyici sınırları](https://docs.microsoft.com/azure/azure-resource-manager/management/azure-subscription-service-limits#load-balancer). Standart Load Balancer hakkında ayrıntılı bilgi için bkz. [genel bakış](load-balancer-standard-overview.md), [fiyatlandırma](https://aka.ms/lbpricing) ve [SLA](https://aka.ms/lbsla).


## <a name = "limitations"></a>Algılan

* Load Balancer, belirli TCP veya UDP protokolleri için yük dengeleme ve bağlantı noktası iletme sağlar. Yük Dengeleme kuralları ve gelen NAT kuralları TCP ve UDP 'yi destekler, ancak ıCMP dahil diğer IP protokollerini desteklemez.

  Load Balancer, bir UDP veya TCP akışının yüküyle sonlanmaz, yanıt vermez veya başka bir şekilde etkileşime geçmez. Bu bir proxy değildir. Ön uç bağlantısının başarıyla doğrulanması, Yük Dengeleme veya gelen NAT kuralında kullanılan aynı protokolle bant içinde yer almalıdır. Sanal makinelerinizden en az birinin bir istemcinin ön ucundan yanıt görmesini sağlamak için bir yanıt oluşturması gerekir.

  Load Balancer ön ucundan bir bant içi yanıt alınmıyor sanal makinelerin yanıt veremediğini belirtir. Bir sanal makine yanıt veremeden bir Load Balancer ön ucuna hiçbir şey etkileşime giremez. Bu ilke, bağlantı noktası maskesi SNAT 'nin yalnızca TCP ve UDP için desteklendiği giden bağlantılar için de geçerlidir. ICMP dahil olmak üzere diğer IP protokolleri de başarısız olur. Bu sorunu azaltmak için örnek düzeyi genel bir IP adresi atayın. Daha fazla bilgi için bkz. [SNAT ve Pat 'Yi anlama](load-balancer-outbound-connections.md#snat).

* İç yük dengeleyiciler, giden kaynaklı bağlantıları iç Load Balancer ön ucuna çevirmez çünkü her ikisi de özel IP adresi alanında bulunur. Ortak yük dengeleyiciler, sanal ağ içindeki özel IP adreslerinden genel IP adreslerine [giden bağlantılar](load-balancer-outbound-connections.md) sağlar. Bu yaklaşım, iç yük dengeleyiciler için, çeviri gerekli olmadığı, benzersiz bir iç IP adresi alanı içinde olası SNAT bağlantı noktası tükenmesi önler.

  Bir yan etkisi, arka uç havuzundaki bir VM 'den giden bir akış, havuzundaki iç Load Balancer ön uca bir akışı denerse _ve_ kendisine geri eşleniyorsa, akışın iki sanal makinesi eşleşmez. Eşleşmediği için akış başarısız olur. Akış, akışı ön uca oluşturan arka uç havuzundaki aynı VM 'ye geri eşlenmediyse akış başarılı olur.

  Akış kendisiyle eşleniyorsa, giden akış VM 'den ön uca, buna karşılık gelen gelen akış ise VM 'den kendisine doğru şekilde görünür. Konuk işletim sisteminin görünüm noktasından, aynı akışın gelen ve giden parçaları sanal makine içinde eşleşmez. TCP yığını aynı akışın parçası olarak aynı akışın bu halcüleri tanımıyor. Kaynak ve hedef eşleşmiyor. Akış, arka uç havuzundaki diğer herhangi bir sanal makineye eşleniyorsa, akışın kilitlenme miktarı eşleşir ve VM akışa yanıt verebilir.

  Bu senaryonun belirtisi, akış kaynaklı aynı arka uca döndüğünde zaman aralıklı bağlantı zaman aşımları olur. Yaygın geçici çözümler, iç Load Balancer arkasında bir proxy katmanının eklenmesini ve doğrudan sunucu dönüşü (DSR) stil kurallarını kullanmayı içerir. Daha fazla bilgi için bkz. [Azure Load Balancer Için birden çok ön](load-balancer-multivip-overview.md)uç.

  Bir iç Load Balancer herhangi bir üçüncü taraf proxy ile birleştirebilir veya HTTP/HTTPS ile proxy senaryolarında iç [Application Gateway](../application-gateway/application-gateway-introduction.md) kullanabilirsiniz. Bu sorunu gidermek için genel Load Balancer kullanabilmeniz sırasında, sonuçta elde edilen senaryo [SNAT tükenmesine](load-balancer-outbound-connections.md#snat)açıktır. Dikkatle yönetilene kadar bu ikinci yaklaşımdan kaçının.

* Genel olarak, iletme IP parçaları yük dengeleme kurallarında desteklenmez. UDP ve TCP paketlerinin IP parçalanması, Yük Dengeleme kurallarında desteklenmez. Yüksek kullanılabilirlik bağlantı noktaları Yük Dengeleme kuralları, var olan IP parçalarını iletmek için kullanılabilir. Daha fazla bilgi için bkz. [yüksek kullanılabilirlik bağlantı noktalarına genel bakış](load-balancer-ha-ports-overview.md).

## <a name="next-steps"></a>Sonraki adımlar

Load Balancer kullanmaya başlamak için bkz. [genel standart Load Balancer oluşturma](quickstart-load-balancer-standard-public-portal.md) : oluşturma, özel bir IIS uzantısı olan VM oluşturma ve VM 'ler arasında Web uygulamasının yükünü dengeleme.
