---
title: Azure Load Balancer nedir?
titleSuffix: Azure Load Balancer
description: Azure Load Balancer özelliklerine, mimarisine ve uygulamasına genel bakış. Load Balancer nasıl çalıştığını ve bulutta nasıl yararlanacağınızı öğrenin.
services: load-balancer
documentationcenter: na
author: asudbring
ms.service: load-balancer
Customer intent: As an IT administrator, I want to learn more about the Azure Load Balancer service and what I can use it for.
ms.devlang: na
ms.topic: overview
ms.custom: seodec18
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 12/05/2019
ms.author: allensu
ms.openlocfilehash: 4f6a0a2631c3f7458548a2ff435c3e565e744f41
ms.sourcegitcommit: 51ed913864f11e78a4a98599b55bbb036550d8a5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/04/2020
ms.locfileid: "75660230"
---
# <a name="what-is-azure-load-balancer"></a>Azure Load Balancer nedir?

*Yük Dengeleme* , yük veya gelen ağ trafiğini bir grup arka uç kaynağı veya sunucusu arasında verimli bir şekilde dağıtmayı gösterir. Azure, ihtiyaya göre seçebileceğiniz [çeşitli yük dengeleme seçenekleri](https://docs.microsoft.com/azure/architecture/guide/technology-choices/load-balancing-overview) sunar. Bu belgede Azure Load Balancer yer almaktadır.

Azure Load Balancer, açık sistemler arası (OSı) modelin dört katmanında çalışır. Bu, istemcilerle ilgili tek iletişim noktasıdır. Load Balancer, Load Balancer ön ucuna, belirli Yük Dengeleme kurallarına ve sistem durumu araştırmalara göre arka uç havuz örneklerine gelen yeni gelen akışları dağıtır. Arka uç havuzu örnekleri, bir sanal makine ölçek kümesindeki Azure sanal makineleri veya örnekleri olabilir. 

Azure Load Balancer, uygulamalarınızı ölçeklendirebilir ve yüksek oranda kullanılabilir hizmetler oluşturabilirsiniz. Load Balancer hem gelen hem de giden senaryoları destekler, düşük gecikme süresi ve yüksek aktarım hızı sağlar ve tüm TCP ve UDP uygulamaları için milyonlarca akışa kadar ölçeklendirme yapar.

**[Ortak Load Balancer](#publicloadbalancer)** , özel IP ADRESLERINI genel IP adreslerine çevirerek sanal ağınızdaki sanal makineler (VM) için giden bağlantılar sağlayabilir. Ortak yük dengeleyiciler, sanal makinelerinize dengeleyici internet trafiği yüklemek için kullanılır.

Ön uçta yalnızca özel IP adreslerinin gerektiği senaryolar için bir **[iç (veya özel) Load Balancer](#internalloadbalancer)** kullanılabilir. İç yük dengeleyiciler, bir sanal ağ içindeki trafiğin yükünü dengelemek için kullanılır. Karma bir senaryoda şirket içi bir ağdan Load Balancer ön uca da ulaşabilirsiniz.

## <a name="load-balancer-components"></a>Load Balancer bileşenleri
* **Ön uç IP yapılandırması**: Load Balancer IP adresi. Bu, istemciler için iletişim noktasıdır. Bunlar ortak veya özel IP adresleri olabilir, bu nedenle sırasıyla ortak veya Iç yük dengeleyiciler oluşturulur.
* **Arka uç havuzu**: sanal makine ölçek kümesindeki, gelen Isteği sunacak sanal makinelerin veya örneklerin grubu. Yüksek hacimli gelen trafik bilgi işlem en iyi uygulamaları karşılamak için maliyeti etkili bir şekilde ölçeklendirmek için, genellikle arka uç havuzuna daha fazla örnek eklenmesini önerir. Örnekleri yukarı veya aşağı ölçeklendirirseniz otomatik yeniden yapılandırma yoluyla kendisini anında yeniden yapılandırır. Load Balancer Arka uç havuzunda VM ekleme veya kaldırma işlemi gerçekleştirdiğinizde Load Balancer kaynağında ek işlem yapmanıza gerek kalmadan Load Balancer yeniden yapılandırılır.
* **Durum araştırmaları**: arka uç havuzundaki örneklerin sistem durumunu belirlemede bir sistem durumu araştırması kullanılır. Sistem durumu araştırmalarının sağlıksız eşiğini tanımlayabilirsiniz. Bir yoklama yanıt veremediğinde, Load Balancer iyi durumda olmayan örneklere yeni bağlantı göndermeyi durdurur. Bir araştırma hatası varolan bağlantıları etkilemez. Bağlantı, uygulama akışı sonlandırana kadar devam eder, boşta kalma zaman aşımı oluşur veya VM kapanır. Load Balancer, TCP, HTTP ve HTTPS uç noktaları için farklı durum araştırma türleri sağlar. Daha fazla bilgi için bkz. [araştırma türleri](load-balancer-custom-probe-overview.md#types).
* **Yük Dengeleme kuralları**: Yük Dengeleme kuralları, ne zaman yapılması gerektiğini Load Balancer söyleyen bir şeydir. 
* **Gelen NAT kuralları**: gelen NAT kuralı, belirli bir ön uç IP adresinin belirli bir bağlantı noktasından gelen trafiği, sanal ağ içindeki belirli bir arka uç örneğinin belirli bir bağlantı noktasına iletir. Bağlantı noktası iletme, Yük Dengeleme ile aynı karma tabanlı dağıtım tarafından yapılır. Bu özelliğe ilişkin yaygın senaryolar, bir Azure sanal ağı içindeki tek tek sanal makine örneklerine Uzak Masaüstü Protokolü (RDP) veya Secure Shell (SSH) oturumlardır. Aynı ön uç IP adresindeki bağlantı noktalarında birden çok iç uç noktası eşleyebilirsiniz. Ön uç IP adreslerini ek bir sıçrama kutusu olmadan sanal makinelerinizi uzaktan yönetmek için kullanabilirsiniz.
* **Giden kuralları**: giden bir kural, ön uca çevrilecek tüm sanal makineler veya arka uç havuzu tarafından tanımlanan örneklerin giden ağ adresi ÇEVIRISINI (NAT) yapılandırır.

## <a name="load-balancer-concepts"></a>Load Balancer kavramlar

Load Balancer, TCP ve UDP uygulamaları için aşağıdaki temel özellikleri sunar:

* **Yük dengeleme algoritması**: Azure Load Balancer ile, ön uca arka uç havuzu örneklerine ulaşan trafiği dağıtmak için bir yük dengeleme kuralı oluşturabilirsiniz. Load Balancer, gelen akışların dağıtılması için bir karma algoritması kullanır ve akışların üst bilgilerini arka uç havuz örneklerine yeniden yazar. Bir sistem durumu araştırması sağlıklı bir arka uç bitiş noktasını gösterdiğinde yeni akışlar almak için bir sunucu kullanılabilir.
Varsayılan olarak, Load Balancer 5 demet karma kullanır. Karma, akışları kullanılabilir sunucularla eşlemek için kaynak IP adresi, kaynak bağlantı noktası, hedef IP adresi, hedef bağlantı noktası ve IP protokol numarası içerir. Belirli bir kural için 2 veya 3 demet karma kullanarak bir kaynak IP adresine benzeşim oluşturabilirsiniz. Aynı paket akışının tüm paketleri, yük dengeleme uygulanan ön ucun arkasındaki aynı örneğe ulaşır. İstemci aynı kaynak IP 'den yeni bir akış başlattığında, kaynak bağlantı noktası değiştirilir. Sonuç olarak, 5 demet karma, trafiğin farklı bir arka uç uç noktasına geçmesine neden olabilir.
Daha fazla bilgi için bkz. [Azure Load Balancer Dağıtım modunu yapılandırma](load-balancer-distribution-mode.md). Aşağıdaki görüntüde karma tabanlı dağıtım gösterilmiştir:

  ![Karma tabanlı dağıtım](./media/load-balancer-overview/load-balancer-distribution.png)

  *Şekil: Karma tabanlı dağıtım*

* **Uygulama bağımsızlık ve saydamlık**: Load Balancer doğrudan TCP veya UDP veya uygulama katmanıyla etkileşime girmez. Herhangi bir TCP veya UDP uygulama senaryosu desteklenebilir. Load Balancer, akışların sonlandırılması veya kaynaklanmaz, akışın yüküyle etkileşime geçebilir veya herhangi bir uygulama katmanı ağ geçidi işlevi sağlar. Protokol el sıkışmaları her zaman doğrudan istemci ile arka uç havuzu örneği arasında gerçekleşir. Gelen akışa verilen yanıt her zaman sanal makineden gelen yanıttır. Akış sanal makineye ulaştığında özgün kaynak IP adresi de korunur.
  * Her uç noktaya yalnızca bir VM tarafından yanıt verilir. Örneğin, istemci ve seçilen arka uç VM arasında her zaman bir TCP el sıkışması meydana gelir. Ön uca bir istek yanıtı, arka uç VM tarafından oluşturulan bir yanıttır. Bir ön uca bağlantıyı başarıyla doğruladığınızda, en az bir adet arka uç sanal makinesine uçtan uca bağlantıyı doğrularsınız.
  * Uygulama yükleri Load Balancer için saydamdır. Herhangi bir UDP veya TCP uygulaması desteklenebilir.
  * Load Balancer TCP yükü ile etkileşimde olmadığından ve TLS yük boşaltma sağladığından uçtan uca şifrelenmiş senaryolar oluşturabilirsiniz. Load Balancer kullanmak, sanal makinenin kendisinde TLS bağlantısını sonlandırarak TLS uygulamaları için büyük ölçekte genişleme sağlar. Örneğin, TLS oturum anahtar kapasiteniz yalnızca, arka uç havuzuna eklediğiniz VM 'lerin türü ve sayısıyla sınırlıdır.

* **Giden bağlantılar (SNAT)** : sanal ağınız IÇINDEKI özel IP adreslerinden gelen tüm giden akışlar, Load Balancer ön uç IP adresine çevrilebilir. Bir genel ön uç, bir yük dengeleme kuralı aracılığıyla arka uç VM 'sine bağlı olduğunda, Azure giden bağlantıları genel ön uç IP adresine çevirir. Bu yapılandırmanın avantajları şunlardır:
  * Basit yükseltme ve olağanüstü durum kurtarma, ön uç, hizmetin başka bir örneğine dinamik olarak eşlenmeye olanak sağlar.
  * Daha kolay erişim denetim listesi (ACL) yönetimi. Hizmet ölçeği artırma veya azaltma ya da yeniden dağıtım gibi, ön uç IP 'ler olarak ifade edilen ACL 'Ler değişmez. Giden bağlantıların makinelerden daha az sayıda IP adresine dönüştürülmesi, güvenli alıcı listelerinin uygulanması yükünü azaltır.
Daha fazla bilgi için bkz. [Azure 'Da giden bağlantılar](load-balancer-outbound-connections.md).
Standart Load Balancer, aşağıda açıklandığı gibi bu temellerin ötesinde ek SKU 'ya özgü yetenekler içerir.

## <a name="skus"></a> Load Balancer SKU karşılaştırması

Load Balancer hem temel hem de standart SKU 'Ları destekler. Bu SKU 'Lar senaryo ölçeğinde, özelliklerde ve fiyatlandırmaya göre farklılık gösterir. Temel Load Balancer olabilecek herhangi bir senaryo, Standart Load Balancer ile oluşturulabilir. Her iki SKU için de API 'Ler benzerdir ve bir SKU 'nun belirtimine göre çağrılır. Load Balancer için SKU 'Ları destekleme API 'SI ve genel IP, `2017-08-01` API 'siyle başlayarak kullanılabilir. Her iki SKU da aynı genel API ve yapıyı paylaşır.

Tüm senaryo yapılandırması, SKU 'ya bağlı olarak biraz farklı görünebilir. Load Balancer belgelerinde yalnızca belirli bir SKU için geçerli olan makaleler belirtilmiştir. Farkları karşılaştırmak ve anlamak için aşağıdaki tabloya bakın. Daha fazla bilgi için bkz. [Azure Standart Load Balancer genel bakış](load-balancer-standard-overview.md).

>[!NOTE]
> Microsoft Standart Load Balancer önerir.
Tek başına VM'ler, kullanılabilirlik kümeleri ve sanal makine ölçek kümeleri yalnızca tek bir SKU'ya bağlanabilir, ikisine birden bağlanamaz. Load Balancer ve genel IP adresi SKU 'SU ortak IP adresleriyle kullandığınızda aynı olmalıdır. Load Balancer ve genel IP SKU 'Ları değişebilir değildir.

[!INCLUDE [comparison table](../../includes/load-balancer-comparison-table.md)]

Daha fazla bilgi için bkz. [yük dengeleyici sınırları](https://aka.ms/lblimits). Standart Load Balancer hakkında ayrıntılı bilgi için bkz. [genel bakış](load-balancer-standard-overview.md), [fiyatlandırma](https://aka.ms/lbpricing) ve [SLA](https://aka.ms/lbsla).

## <a name="load-balancer-types"></a>Load Balancer türleri

### <a name = "publicloadbalancer"></a>Genel Load Balancer

Ortak Load Balancer, gelen trafiğin genel IP adresini ve bağlantı noktasını, sanal makinenin özel IP adresine ve bağlantı noktasına eşler. Load Balancer trafiği VM 'den gelen yanıt trafiği için başka bir şekilde eşler. Yük Dengeleme kuralları uygulayarak, belirli trafik türlerini birden çok VM veya hizmet arasında dağıtabilirsiniz. Örneğin web isteği trafiğinin yükünü birden fazla web sunucusuna dağıtabilirsiniz.

>[!NOTE]
>Her kullanılabilirlik kümesi için yalnızca bir ortak Load Balancer ve bir iç Load Balancer uygulayabilirsiniz.

Aşağıdaki şekilde, genel ve TCP bağlantı noktası 80 için üç VM arasında paylaşılan web trafiği için yük dengeli bir uç nokta gösterilmektedir. Bu üç VM, bir yük dengeleme kümesinde bulunur.

![Genel Load Balancer örneği](./media/load-balancer-overview/IC727496.png)

*Şekil: genel Load Balancer kullanarak Web trafiğini Dengeleme*

Internet istemcileri, 80 numaralı TCP bağlantı noktasındaki bir Web uygulamasının genel IP adresine Web sayfası istekleri gönderir. Azure Load Balancer, istekleri yük dengeli küme içindeki üç sanal makineye dağıtır. Load Balancer algoritmaları hakkında daha fazla bilgi için bkz. [Load Balancer kavramlar](load-balancer-overview.md##load-balancer-concepts).

Azure Load Balancer, ağ trafiğini varsayılan olarak birden çok VM örneği arasında eşit olarak dağıtır. Oturum benzeşimini de yapılandırabilirsiniz. Daha fazla bilgi için bkz. [Azure Load Balancer Dağıtım modunu yapılandırma](load-balancer-distribution-mode.md).

### <a name = "internalloadbalancer"></a> İç Load Balancer

Bir iç Load Balancer, trafiği yalnızca bir sanal ağ içinde olan veya Azure altyapısına erişmek için bir VPN kullanan veya ortak bir Load Balancer karşılık gelen kaynaklara yönlendirir. Azure altyapısı, bir sanal ağın yük dengeli ön uç IP adreslerine erişimi kısıtlar. Ön uç IP adresleri ve sanal ağlar hiçbir şekilde doğrudan bir internet uç noktasına gösterilmez. İç iş kolu uygulamaları Azure'da çalışır ve Azure'dan veya şirket içi kaynaklardan erişim sağlanır.

İç Load Balancer, aşağıdaki yük dengeleme türlerini destekler:

* **Bir sanal ağ içinde**: sanal ağdaki VM 'lerden, aynı sanal ağda bulunan bir VM kümesine yük dengeleme.
* **Şirketler arası sanal ağ için**: şirket içi bilgisayarlardan aynı sanal ağdaki bir VM kümesine yük dengeleme.
* **Çok katmanlı uygulamalar için**: arka uç katmanlarının internet 'e açık olmadığı internet 'e yönelik çok katmanlı uygulamalar için yük dengeleme. Arka uç katmanları, internet 'e yönelik katmandan trafik yük dengelemesi gerektirir. Sonraki şekle bakın.
* **İş kolu uygulamaları için**: Ek yük dengeleyici donanım veya yazılım olmadan Azure'da barındırılan iş kolu uygulamaları için yük dengeleme. Bu senaryo, trafiği yük dengeli olan bilgisayar kümesinde bulunan şirket içi sunucuları içerir.

![İç Load Balancer örneği](./media/load-balancer-overview/IC744147.png)

*Şekil: çok katmanlı uygulamaları hem genel hem de dahili Load Balancer kullanarak Dengeleme*

## <a name="pricing"></a>Fiyatlandırma

Standart Load Balancer kullanım ücretlendirilir.

* Yapılandırılmış yük dengeleme ve giden kural sayısı. Gelen NAT kuralları toplam kural sayısı içinde sayılmaz.
* Gelen ve giden kurallardan bağımsız olarak işlenen veri miktarı.

Standart Load Balancer fiyatlandırma bilgileri için bkz. [Load Balancer fiyatlandırması](https://azure.microsoft.com/pricing/details/load-balancer/).

Temel Load Balancer ücretsiz olarak sunulur.

## <a name="sla"></a>SLA

SLA Standart Load Balancer hakkında daha fazla bilgi için bkz. [Load Balancer Için SLA](https://aka.ms/lbsla).

## <a name="limitations"></a>Sınırlamalar

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
