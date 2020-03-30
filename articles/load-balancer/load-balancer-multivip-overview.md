---
title: Birden çok frontends - Azure Yük Dengeleyicisi
description: Bu öğrenme yolu ile Azure Yük Dengeleyicisi'ndeki birden fazla ön uça genel bakışla başlayın
services: load-balancer
documentationcenter: na
author: asudbring
ms.service: load-balancer
ms.custom: seodec18
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/07/2019
ms.author: allensu
ms.openlocfilehash: 0a54416a70a8561edfad5915944100e0ce686bbf
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75771266"
---
# <a name="multiple-frontends-for-azure-load-balancer"></a>Azure Yük Dengeleyicisi için birden çok ön uç

Azure Yük Dengeleyici, bakiye hizmetlerini birden çok bağlantı noktası, birden çok IP adresi veya her ikisine yüklemenize olanak tanır. Bir dizi VM üzerinden bakiye akışlarını yüklemek için genel ve dahili yük dengeleyici tanımlarını kullanabilirsiniz.

Bu makalede, bu yeteneğin temelleri, önemli kavramlar ve kısıtlamalar açıklanmaktadır. Hizmetleri yalnızca tek bir IP adresinde ifşa etmek istiyorsanız, [genel](load-balancer-get-started-internet-portal.md) veya [dahili](load-balancer-get-started-ilb-arm-portal.md) yük dengeleyici yapılandırmaları için basitleştirilmiş yönergeler bulabilirsiniz. Birden çok ön uç eklemek, tek bir frontend yapılandırmasına artımlıdır. Bu makaledeki kavramları kullanarak, istediğiniz zaman basitleştirilmiş bir yapılandırmayı genişletebilirsiniz.

Bir Azure Yük Dengeleyicisi tanımladığınızda, bir ön uç ve arka uç havuzu yapılandırması kurallara bağlanır. Kural tarafından başvurulan sistem durumu sondası, arka uç havuzundaki bir düğüme nasıl yeni akışlar gönderildiğini belirlemek için kullanılır. Ön uç (aka VIP), bir IP adresi (genel veya dahili), bir aktarım protokolü (UDP veya TCP) ve yük dengeleme kuralından bir bağlantı noktası numarasından oluşan 3-tuple ile tanımlanır. Arka uç havuzu, Yük Dengeleyici arka uç havuzuna başvuran Sanal Makine IP yapılandırmaları (NIC kaynağının bir parçası) koleksiyonudur.

Aşağıdaki tablo bazı örnek ön uç yapılandırmaları içerir:

| Ön uç | IP adresi | protokol | port |
| --- | --- | --- | --- |
| 1 |65.52.0.1 |TCP |80 |
| 2 |65.52.0.1 |TCP |*8080* |
| 3 |65.52.0.1 |*UDP* |80 |
| 4 |*65.52.0.2* |TCP |80 |

Tablo dört farklı frontends gösterir. Frontends #1, #2 ve #3 birden fazla kural ile tek bir ön uç vardır. Aynı IP adresi kullanılır, ancak bağlantı noktası veya protokol her ön uç için farklıdır. Frontends #1 ve #4 birden çok frontends, aynı ön uç protokolü ve bağlantı noktası birden çok frontends arasında yeniden kullanılan bir örnektir.

Azure Yük Dengeleyici, yük dengeleme kurallarını tanımlamada esneklik sağlar. Kural, ön uçtaki bir adresin ve bağlantı noktasının arka uçtaki hedef adrese ve bağlantı noktasına nasıl eşlendirilebildiğini bildirir. Arka uç bağlantı noktalarının kurallar arasında yeniden kullanılıp kullanılmadığı kuralın türüne bağlıdır. Her kural türü, ana bilgisayar yapılandırması ve sonda tasarımını etkileyebilecek özel gereksinimlere sahiptir. İki tür kural vardır:

1. Arka uç bağlantı noktası yeniden kullanılmadan varsayılan kural
2. Arka uç bağlantı noktalarının yeniden kullanıldığı Kayan IP kuralı

Azure Yük Dengeleyici, her iki kural türünü de aynı yük dengeleyici yapılandırması üzerinde karıştırmanıza olanak tanır. Kural kısıtlamalarına uydunuzkça, yük dengeleyicisi bunları belirli bir VM veya herhangi bir kombinasyon için aynı anda kullanabilir. Seçtiğiniz kural türü, uygulamanızın gereksinimlerine ve bu yapılandırmayı desteklemenin karmaşıklığına bağlıdır. Senaryonuz için hangi kural türlerinin en iyi olduğunu değerlendirmelisiniz.

Varsayılan davranışla başlayarak bu senaryoları daha da inceleriz.

## <a name="rule-type-1-no-backend-port-reuse"></a>Kural türü #1: Arka uç bağlantı noktası yeniden kullanımı yok

![Yeşil ve mor ön uç ile birden fazla ön uç illüstrasyon](./media/load-balancer-multivip-overview/load-balancer-multivip.png)

Bu senaryoda, ön uçlar aşağıdaki gibi yapılandırılır:

| Ön uç | IP adresi | protokol | port |
| --- | --- | --- | --- |
| ![yeşil ön uç](./media/load-balancer-multivip-overview/load-balancer-rule-green.png) 1 |65.52.0.1 |TCP |80 |
| ![mor ön uç](./media/load-balancer-multivip-overview/load-balancer-rule-purple.png) 2 |*65.52.0.2* |TCP |80 |

DIP, gelen akışın varış noktasıdır. Arka uç havuzunda, her VM bir DIP üzerinde benzersiz bir bağlantı noktasında istenen hizmeti ortaya çıkarır. Bu hizmet, bir kural tanımı ile ön uç ile ilişkilidir.

İki kural tanımlıyoruz:

| Kural | Harita ön uç | Arka uç havuzuiçin |
| --- | --- | --- |
| 1 |![yeşil ön uç](./media/load-balancer-multivip-overview/load-balancer-rule-green.png) Frontend1:80 |![arka uç](./media/load-balancer-multivip-overview/load-balancer-rule-green.png) DIP1:80, ![arka uç](./media/load-balancer-multivip-overview/load-balancer-rule-green.png) DIP2:80 |
| 2 |![VIP](./media/load-balancer-multivip-overview/load-balancer-rule-purple.png) Frontend2:80 |![arka uç](./media/load-balancer-multivip-overview/load-balancer-rule-purple.png) DIP1:81, ![arka uç](./media/load-balancer-multivip-overview/load-balancer-rule-purple.png) DIP2:81 |

Azure Yük Dengeleyicisi'ndeki tam eşleme şu şekildedir:

| Kural | Ön uç IP adresi | protokol | port | Hedef | port |
| --- | --- | --- | --- | --- | --- |
| ![yeşil kural](./media/load-balancer-multivip-overview/load-balancer-rule-green.png) 1 |65.52.0.1 |TCP |80 |DIP IP Adresi |80 |
| ![mor kural](./media/load-balancer-multivip-overview/load-balancer-rule-purple.png) 2 |65.52.0.2 |TCP |80 |DIP IP Adresi |81 |

Her kural, hedef IP adresi ve hedef bağlantı noktasının benzersiz bir birleşimi ile bir akış oluşturmalıdır. Akışın hedef bağlantı noktasını değiştirerek, birden çok kural farklı bağlantı noktalarında aynı DIP'e akışlar sağlayabilir.

Sağlık sondaları her zaman bir VM'nin DIP'sine yönlendirilir. Sondanızın VM'nin sağlığını yansıttığından emin olmalısınız.

## <a name="rule-type-2-backend-port-reuse-by-using-floating-ip"></a>Kural türü #2: Kayan IP kullanarak arka uç bağlantı noktası yeniden

Azure Yük Dengeleyici, kullanılan kural türünden bağımsız olarak ön uç bağlantı noktasını birden çok ön uçta yeniden kullanma esnekliği sağlar. Ayrıca, bazı uygulama senaryoları tercih veya arka uç havuzunda tek bir VM üzerinde birden çok uygulama örnekleri tarafından kullanılmak üzere aynı bağlantı noktası gerektirir. Bağlantı noktası yeniden kullanımına sık sık örnek olarak yüksek kullanılabilirlik için kümeleme, ağ sanal cihazları ve yeniden şifreleme olmadan birden çok TLS uç noktasını açığa çıkarmak sayılabilir.

Arka uç bağlantı noktasını birden çok kural arasında yeniden kullanmak istiyorsanız, kural tanımında Kayan IP'yi etkinleştirmeniz gerekir.

"Kayan IP", Doğrudan Sunucu İadesi (DSR) olarak bilinen şeyin bir kısmı için Azure'un terminolojisidir. DSR iki bölümden oluşur: akış topolojisi ve IP adresi eşleme şeması. Platform düzeyinde, Azure Yük Dengeleyici, Kayan IP'nin etkin olup olmadığına bakılmaksızın her zaman bir DSR akış topolojisinde çalışır. Bu, bir akışın giden kısmının her zaman doğrudan menşeine akmak üzere doğru şekilde yeniden yazıldığı anlamına gelir.

Varsayılan kural türüyle Azure, kullanım kolaylığı için geleneksel bir yük dengeleme IP adresi eşleme düzenini ortaya çıkarır. Kayan IP'yi etkinleştirmek, ip adresi eşleme düzenini değiştirerek aşağıda açıklandığı gibi ek esneklik sağlar.

Aşağıdaki diyagram bu yapılandırmayı göstermektedir:

![DSR ile yeşil ve mor ön uç ile birden fazla ön uç illüstrasyon](./media/load-balancer-multivip-overview/load-balancer-multivip-dsr.png)

Bu senaryo için, arka uç havuzundaki her VM'nin üç ağ arabirimi vardır:

* DIP: VM ile ilişkili bir Sanal NIC (Azure'un NIC kaynağının IP yapılandırması)
* Frontend 1: Konuk işletim sistemi içinde Frontend 1 IP adresi ile yapılandırılan bir döngü arabirimi
* Frontend 2: Frontend 2 IP adresi ile yapılandırılan konuk işletim sistemi içinde bir loopback arabirimi

Arka uç havuzundaki her VM için aşağıdaki komutları bir Windows Komut Komut Komut Ustem'de çalıştırın.

VM'nizde sahip olduğunuz arabirim adlarının listesini almak için şu komutu yazın:

    netsh interface show interface 

VM NIC (Azure yönetilen) için şu komutu yazın:

    netsh interface ipv4 set interface “interfacename” weakhostreceive=enabled
   (arayüz adını bu arabirimin adı ile değiştirin)

Eklediğiniz her loopback arabirimi için şu komutları yineleyin:

    netsh interface ipv4 set interface “interfacename” weakhostreceive=enabled 
   (arabirim adını bu geri dönüş arabiriminin adı ile değiştirin)
     
    netsh interface ipv4 set interface “interfacename” weakhostsend=enabled 
   (arabirim adını bu geri dönüş arabiriminin adı ile değiştirin)

> [!IMPORTANT]
> Loopback arabirimlerinin yapılandırması konuk işletim sistemi içinde gerçekleştirilir. Bu yapılandırma Azure tarafından gerçekleştirilmez veya yönetilmez. Bu yapılandırma olmadan, kurallar çalışmaz. Sistem durumu sondası tanımları, DSR Frontend'i temsil eden geri dönüş arabirimi yerine VM'nin DIP'ini kullanır. Bu nedenle, hizmetinizin DSR Frontend'i temsil eden geri dönüş arabiriminde sunulan hizmetin durumunu yansıtan bir DIP bağlantı noktasında sonda yanıtları sağlaması gerekir.


Önceki senaryodakiyle aynı ön yüz yapılandırmasını varsayalım:

| Ön uç | IP adresi | protokol | port |
| --- | --- | --- | --- |
| ![yeşil ön uç](./media/load-balancer-multivip-overview/load-balancer-rule-green.png) 1 |65.52.0.1 |TCP |80 |
| ![mor ön uç](./media/load-balancer-multivip-overview/load-balancer-rule-purple.png) 2 |*65.52.0.2* |TCP |80 |

İki kural tanımlıyoruz:

| Kural | Ön uç | Arka uç havuzuna harita |
| --- | --- | --- |
| 1 |![Kural](./media/load-balancer-multivip-overview/load-balancer-rule-green.png) Frontend1:80 |![arka uç](./media/load-balancer-multivip-overview/load-balancer-rule-green.png) Frontend1:80 (VM1 ve VM2'de) |
| 2 |![Kural](./media/load-balancer-multivip-overview/load-balancer-rule-purple.png) Frontend2:80 |![arka uç](./media/load-balancer-multivip-overview/load-balancer-rule-purple.png) Frontend2:80 (VM1 ve VM2'de) |

Aşağıdaki tablo, yük dengeleyicisindeki tam eşlemeyi gösterir:

| Kural | Ön uç IP adresi | protokol | port | Hedef | port |
| --- | --- | --- | --- | --- | --- |
| ![yeşil kural](./media/load-balancer-multivip-overview/load-balancer-rule-green.png) 1 |65.52.0.1 |TCP |80 |ön uçla aynı (65.52.0.1) |frontend (80) ile aynı |
| ![mor kural](./media/load-balancer-multivip-overview/load-balancer-rule-purple.png) 2 |65.52.0.2 |TCP |80 |ön uçla aynı (65.52.0.2) |frontend (80) ile aynı |

Gelen akışın hedefi, VM'deki geri dönüş arabirimindeki ön uç IP adresidir. Her kural, hedef IP adresi ve hedef bağlantı noktasının benzersiz bir birleşimi ile bir akış oluşturmalıdır. Akışın hedef IP adresini değiştirerek, aynı VM'de bağlantı noktası yeniden kullanımı mümkündür. Hizmetiniz, ilgili geri dönüş arabiriminin ön uç IP adresine ve bağlantı noktasına bağlanarak yük dengeleyicisine maruz kalır.

Bu örneğin hedef bağlantı noktasını değiştirmediğini unutmayın. Bu kayan bir IP senaryosu olsa da, Azure Yük Dengeleyicisi arka uç hedef bağlantı noktasını yeniden yazmak ve ön uç hedef bağlantı noktasından farklı hale getirmek için bir kural tanımlamayı da destekler.

Kayan IP kural türü, birkaç yük dengeleyici yapılandırma düzeninin temelini oluşturur. Şu anda kullanılabilen bir örnek, [Birden Çok Dinleyici yapılandırması olan SQL AlwaysOn'dur.](../virtual-machines/windows/sql/virtual-machines-windows-portal-sql-ps-alwayson-int-listener.md) Zamanla, bu senaryoların daha fazlasını belgeleeceğiz.

## <a name="limitations"></a>Sınırlamalar

* Birden çok ön uç yapılandırması yalnızca IaaS VM'lerle desteklenir.
* Kayan IP kuralı ile, uygulamanız giden SNAT akışları için birincil IP yapılandırmasını kullanmalıdır. Uygulamanız konuk işletim sistemi içinde döngü arabiriminde yapılandırılan ön uç IP adresine bağlanırsa, Azure'un giden SNAT'ı giden akışı yeniden yazmak için kullanılamaz ve akış başarısız olur.  Giden senaryoları gözden [geçirin.](load-balancer-outbound-connections.md)
* Genel IP adreslerinin faturalandırma üzerinde etkisi vardır. Daha fazla bilgi için [IP Adresi fiyatlandırması](https://azure.microsoft.com/pricing/details/ip-addresses/)
* Abonelik sınırları geçerlidir. Daha fazla bilgi için ayrıntılar için [Hizmet sınırlarına](../azure-resource-manager/management/azure-subscription-service-limits.md#networking-limits) bakın.

## <a name="next-steps"></a>Sonraki adımlar

- Birden çok ön uçların giden bağlantı davranışı üzerindeki etkisini anlamak için [Giden bağlantıları](load-balancer-outbound-connections.md) gözden geçirin.
