---
title: Azure Load Balancer kavramlar
description: Azure Load Balancer kavramlara genel bakış
services: load-balancer
documentationcenter: na
author: asudbring
ms.service: load-balancer
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/13/2020
ms.author: allensu
ms.openlocfilehash: 96b91c08e0bf9a176af2ed844cd63a2cfa1e233d
ms.sourcegitcommit: 02ca0f340a44b7e18acca1351c8e81f3cca4a370
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/19/2020
ms.locfileid: "88589648"
---
# <a name="azure-load-balancer-concepts"></a>Azure Load Balancer kavramlar

Yük dengeleyici hem UDP hem de TCP uygulamaları için çeşitli yetenekler sağlar. 

## <a name="load-balancing-algorithm"></a>Yük dengeleme algoritması
Ön uca trafiği bir arka uç havuzuna dağıtmak için bir yük dengeleme kuralı oluşturabilirsiniz. Azure Load Balancer gelen akışların dağıtılması için bir karma algoritması kullanır (bayt değil). Yük dengeleyici, akışların üst bilgilerini arka uç havuz örneklerine yeniden yazar. Bir sistem durumu araştırması sağlıklı bir arka uç bitiş noktasını gösterdiğinde yeni akışlar almak için bir sunucu kullanılabilir.

Varsayılan olarak, yük dengeleyici beş demet karma kullanır.

Karma değeri şunları içerir:

- **Kaynak IP adresi**
- **Kaynak bağlantı noktası**
- **Hedef IP adresi**
- **Hedef bağlantı noktası**
- **Akışların kullanılabilir sunucularla eşlenecek IP protokol numarası**

Bir kaynak IP adresine benzeşim, iki veya üç demet olmayan bir karma kullanılarak oluşturulur. Aynı akışın paketleri, yük dengeli ön ucun arkasındaki aynı örneğe ulaşır. 

İstemci aynı kaynak IP 'den yeni bir akış başlattığında kaynak bağlantı noktası değişir. Sonuç olarak, beş demet karma, trafiğin farklı bir arka uç uç noktasına geçmesine neden olabilir.
Daha fazla bilgi için bkz. [Azure Load Balancer Dağıtım modunu yapılandırma](./load-balancer-distribution-mode.md).

Aşağıdaki görüntüde karma tabanlı dağıtım gösterilmiştir:

![Karma tabanlı dağıtım](./media/load-balancer-overview/load-balancer-distribution.png)

*Şekil: Karma tabanlı dağıtım*

## <a name="application-independence-and-transparency"></a>Uygulama bağımsızlık ve saydamlığı

Yük dengeleyici, TCP veya UDP veya uygulama katmanıyla doğrudan etkileşime girmez. Herhangi bir TCP veya UDP uygulama senaryosu desteklenebilir. Yük dengeleyici akışın yükünü kapatmaz veya kaynaklanmaz veya akışın yüküne göre etkileşime geçmez. Yük Dengeleyici uygulama katmanı ağ geçidi işlevselliği sağlamaz. Protokol el sıkışmaları her zaman doğrudan istemci ile arka uç havuzu örneği arasında gerçekleşir. Gelen akışa verilen yanıt her zaman sanal makineden gelen yanıttır. Akış sanal makineye ulaştığında özgün kaynak IP adresi de korunur.

* Her uç nokta bir VM tarafından yanıtlanmıştır. Örneğin, istemcisiyle seçilen arka uç VM arasında bir TCP el sıkışması meydana gelir. Ön uca bir istek yanıtı, arka uç VM tarafından oluşturulan bir yanıttır. Bir ön uca bağlantıyı başarıyla doğruladığınızda, en az bir arka uç sanal makinesi için bağlantıyı doğrularsınız.
* Uygulama yükleri yük dengeleyicisine saydamdır. Herhangi bir UDP veya TCP uygulaması desteklenebilir.
* Yük dengeleyici TCP yükü ile etkileşimde olmadığından ve TLS yük boşaltma sağladığından, kapsamlı şifrelenmiş senaryolar oluşturabilirsiniz. Yük dengeleyici kullanmak, TLS bağlantısını VM üzerinde sonlandırarak TLS uygulamaları için büyük ölçekli genişleme sağlar. Örneğin, TLS oturum anahtar kapasiteniz yalnızca, arka uç havuzuna eklediğiniz VM 'lerin türü ve sayısıyla sınırlıdır.

## <a name="outbound-connections"></a>Giden bağlantılar 

Arka uç havuzundan genel IP 'lere olan akışlar ön uca eşlenir. Azure, giden bağlantıları Yük Dengeleme giden kuralı aracılığıyla genel ön uç IP adresine çevirir. Bu yapılandırmanın avantajları şunlardır. Basit yükseltme ve olağanüstü durum kurtarma, ön uç, hizmetin başka bir örneğine dinamik olarak eşlenmeye olanak sağlar. Daha kolay erişim denetim listesi (ACL) yönetimi. Hizmet ölçeği artırma veya azaltma ya da yeniden dağıtım gibi, ön uç IP 'ler olarak ifade edilen ACL 'Ler değişmez. Giden bağlantıların makinelerden daha az sayıda IP adresine dönüştürülmesi, güvenli alıcı listelerinin uygulanması yükünü azaltır. Kaynak ağ adresi çevirisi (SNAT) ve Azure Load Balancer hakkında daha fazla bilgi edinmek için bkz. [SNAT ve Azure Load Balancer](load-balancer-outbound-connections.md).

## <a name="availability-zones"></a>Kullanılabilirlik Alanları 

Standart yük dengeleyici Kullanılabilirlik Alanları kullanılabildiği bölgelerde ek becerileri destekler. Kullanılabilirlik Alanları yapılandırma her iki tür Standart Load Balancer için de kullanılabilir. ortak ve dahili. Bölgesel olarak yedekli bir ön uç, tüm bölgelerde aynı anda adanmış altyapı kullanarak bölge hatası. Ayrıca, belirli bir bölgenin ön uç olduğunu garanti edebilirsiniz. Bir bölgesel ön ucu, tek bir bölgedeki adanmış altyapı tarafından sunulur. Çapraz bölge yük dengelemesi, arka uç havuzu için kullanılabilir. Bir sanal ağdaki herhangi bir sanal makine kaynağı, arka uç havuzunun bir parçası olabilir. Temel yük dengeleyici bölgeleri desteklemez. [Kullanılabilirlik alanları ilgili yetenekler hakkında ayrıntılı tartışma](load-balancer-standard-availability-zones.md) ve daha fazla bilgi Için [kullanılabilirlik alanları genel bakış](../availability-zones/az-overview.md) konusunu inceleyin.

## <a name="ha-ports"></a>HA bağlantı noktaları

Uygulamanızın ölçeğini ölçekleyerek ve son derece güvenilir hale getirmek için HA bağlantı noktası yük dengeleme kurallarını yapılandırabilirsiniz. İç yük dengeleyicinin ön uç IP 'si için kısa süreli bağlantı noktalarında akış başına Yük Dengeleme bu kurallar tarafından sağlanır. Bu özellik, tek tek bağlantı noktalarını belirtmek pratik veya istenmeyen bir kullanım dışı olduğunda faydalıdır. HA bağlantı noktaları kuralı, aktif-Pasif veya aktif-aktif n + 1 senaryoları oluşturmanıza olanak sağlar. Bu senaryolar, ağ sanal gereçlerine ve çok büyük gelen bağlantı noktası aralıkları gerektiren uygulamalara yöneliktir. Bir sistem durumu araştırması, hangi arka uçların yeni akışlar alması gerektiğini tespit etmek için kullanılabilir.  Bir bağlantı noktası aralığı senaryosuna öykünmek için bir ağ güvenlik grubu kullanabilirsiniz. Temel yük dengeleyici, HA bağlantı noktalarını desteklemiyor. [Ha bağlantı noktalarının ayrıntılı tartışmasını](load-balancer-ha-ports-overview.md)gözden geçirin.

## <a name="multiple-frontends"></a>Birden çok ön uç 

Yük dengeleyici birden çok ön uç içeren birden çok kuralı destekler.  Standart Load Balancer, bu özelliği giden senaryolara genişletir. Giden kuralları bir gelen kuralının tersidir. Giden kuralı giden bağlantılar için bir ilişki oluşturur. Standart yük dengeleyici, bir yük dengeleme kuralı aracılığıyla bir sanal makine kaynağıyla ilişkili tüm ön uçları kullanır. Ek olarak, Yük Dengeleme kuralındaki bir parametre, giden bağlantı amaçları doğrultusunda bir yük dengeleme kuralını bastırarak hiçbiri dahil belirli ön uçlar seçimine izin verir. Karşılaştırma için temel yük dengeleyici rastgele tek bir ön uç seçer. Hangi ön uçta seçili olduğunu denetleme özelliği yoktur.

## <a name="floating-ip"></a>Kayan IP

Bazı uygulama senaryoları, arka uç havuzundaki tek bir VM 'de birden çok uygulama örneği tarafından kullanılacak bağlantı noktasını tercih eder veya gerektirir. Bağlantı noktası yeniden kullanımının yaygın örnekleri, yüksek kullanılabilirlik, ağ sanal cihazları için kümeleme ve yeniden şifrelemeden birden çok TLS uç noktası kullanıma sunma içerir. Arka uç bağlantı noktasını birden çok kural genelinde yeniden kullanmak istiyorsanız, kural tanımında kayan IP 'yi etkinleştirmeniz gerekir.

**Kayan IP** , doğrudan sunucu dönüşü (DSR) olarak bilinen bir kısmı için Azure 'un terminolojileridir. DSR iki bölümden oluşur: 

- Akış topolojisi
- Bir IP adresi eşleme düzeni

Bir platform düzeyinde Azure Load Balancer, kayan IP 'nin etkin olup olmamasına bakılmaksızın her zaman DSR akış topolojisinde çalışır. Bu, akışın giden bölümünün her zaman doğrudan başlangıca akışa doğru şekilde yeniden yazılması anlamına gelir.
Kayan IP olmadan Azure, kullanım kolaylığı için geleneksel bir yük dengeleme IP adresi eşleme düzeni sunar (VM örnekleri ' IP). Kayan IP 'nin etkinleştirilmesi, ek esneklik sağlamak için IP adresi eşlemesini yük dengeleyicinin ön uç IP 'si olarak değiştirir. [Burada](load-balancer-multivip-overview.md) daha fazla bilgi edinin.


## <a name="limitations"></a><a name = "limitations"></a>Sınırlamalar

- Kayan IP, Iç Yük Dengeleme senaryoları için ikincil IP yapılandırmalarında Şu anda desteklenmiyor.

- Yük dengeleyici kuralı iki sanal ağı yayılamaz.  Ön uçların ve arka uç örneklerinin aynı sanal ağda bulunması gerekir.  

- Sanal ağ ve diğer Microsoft Platformu Hizmetleri olmayan Web çalışanı rollerine yalnızca standart bir iç yük dengeleyicinin arkasındaki örneklerden erişilebilir. İlgili hizmetin kendisi veya temel alınan platform hiçbir bildirimde bulunmaksızın değişiklik yapabilmediği için bu erişilebilirliği kullanmayın. Standart bir iç yük dengeleyici kullanılırken giden bağlantı gerekliyse, [giden bağlantı](load-balancer-outbound-connections.md) yapılandırılması gerekir.

- Yük dengeleyici, belirli TCP veya UDP protokolleri için yük dengeleme ve bağlantı noktası iletme sağlar. Yük Dengeleme kuralları ve gelen NAT kuralları TCP ve UDP 'yi destekler, ancak ıCMP dahil diğer IP protokollerini desteklemez.

- Bir arka uç VM 'den bir iç Load Balancer ön uca giden akış başarısız olur.

- IP parçalarını iletme, Yük Dengeleme kurallarında desteklenmez. UDP ve TCP paketlerinin IP parçalanması, Yük Dengeleme kurallarında desteklenmez. HA bağlantı noktaları Yük Dengeleme kuralları, var olan IP parçalarını iletmek için kullanılabilir. Daha fazla bilgi için bkz. [yüksek kullanılabilirlik bağlantı noktalarına genel bakış](load-balancer-ha-ports-overview.md).

## <a name="next-steps"></a>Sonraki adımlar

- Load Balancer kullanmaya başlamak için bkz. [genel standart Load Balancer oluşturma](quickstart-load-balancer-standard-public-portal.md) : oluşturma, özel bir IIS uzantısı olan VM oluşturma ve VM 'ler arasında Web uygulamasının yükünü dengeleme.
- [Giden Azure Load Balancer bağlantıları](load-balancer-outbound-connections.md)hakkında bilgi edinin.
- [Azure Load Balancer](load-balancer-overview.md)hakkında daha fazla bilgi edinin.
- [Sistem durumu araştırmaları](load-balancer-custom-probe-overview.md)hakkında bilgi edinin.
- [Standart Load Balancer tanılama](load-balancer-standard-diagnostics.md)hakkında bilgi edinin.
- [Ağ güvenlik grupları](../virtual-network/security-overview.md)hakkında daha fazla bilgi edinin.
