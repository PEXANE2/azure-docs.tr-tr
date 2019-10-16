---
title: Azure NAT hizmeti nedir?
titlesuffix: Azure NAT
description: Azure NAT hizmeti özelliklerine, mimarisine ve uygulamasına genel bakış. NAT hizmetinin nasıl çalıştığını ve bulutta nasıl kullanılacağını öğrenin.
services: nat
documentationcenter: na
author: asudbring
ms.service: nat
Customer intent: As an IT administrator, I want to learn more about the Azure NAT service and what I can use it for.
ms.devlang: na
ms.topic: overview
ms.custom: seodec18
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/21/2019
ms.author: allensu
ms.openlocfilehash: 04464716384c1700571096443963499b0ca1b261
ms.sourcegitcommit: 0576bcb894031eb9e7ddb919e241e2e3c42f291d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/15/2019
ms.locfileid: "72376507"
---
# <a name="what-is-azure-nat-service-public-preview"></a>Azure NAT hizmeti nedir (Genel Önizleme)

Azure NAT hizmetini, sanal ağınızdaki tüm sanal makineler için genel IP adresi hedeflerine giden bağlantılar sağlamak üzere kullanabilirsiniz. NAT hizmeti, istek başına ön planlama yapmadan isteğe bağlı Çeviriler sunarak farklı türlerdeki iş yüklerine uyum sağlayabilir.

Bir sanal ağın hangi alt ağlarının NAT hizmetini kullanmasını istediğinizi tanımlayabilirsiniz. Giden bağlantıyı bir veya daha fazla IP adresiyle ölçekleyebilirsiniz. Ve boşta kalma zaman aşımını 4 ile 120 dakika arasında ayarlayabilirsiniz. NAT hizmeti Ayrıca, etkin olmayan bağlantılar yeniden kullanıldığında, boşta kalma zaman aşımı ile erişildikten sonra bu durum olduğu zaman da TCP sıfırlamaları gönderecek. 

NAT hizmetini belirli bir kullanılabilirlik alanına yerleştirerek, [kullanılabilirlik alanları](../availability-zones/az-overview.md) senaryolar oluşturabilirsiniz.

NAT hizmeti, giden bağlantı için yönetilen ve dayanıklı bir hizmettir. NAT hizmetini standart genel IP adresi kaynaklarıyla ve standart yük dengeleyiciyle birleştirerek ek gelen senaryolar oluşturabilirsiniz.

>[!NOTE] 
>Azure NAT hizmeti şu anda genel önizleme olarak kullanılabilir. Şu anda yalnızca sınırlı sayıda [bölgede](#regions)kullanılabilir. Bu önizleme, bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yükleri için önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Ayrıntılar için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.comsupport/legal/preview-supplemental-terms).

## <a name="nat-gateways"></a>NAT ağ geçitleri

Azure NAT hizmeti bir NAT ağ geçidi kaynağı sağlar. Bu kaynak, [Standart genel IP adresi](../virtual-network/virtual-network-ip-addresses-overview-arm.md#standard) veya [Standart genel IP öneki](../virtual-network/public-ip-address-prefix.md) kaynakları veya her ikisi kullanılarak bir veya daha fazla genel IP adresi ile yapılandırılabilir. Bir sanal ağ kaynağının alt ağlarını belirli bir NAT ağ geçidini kullanacak şekilde yapılandırarak NAT hizmetini kullanırsınız. Bir NAT ağ geçidi, aynı sanal ağın birden çok alt ağında kullanılabilir. Farklı sanal ağlarda ayrı NAT ağ geçitleri gerekir.

Bir alt ağ, bir sanal ağın alt ağı için yapılandırıldığında giden bağlantılar için NAT ağ geçidini kullanmaya başlar. NAT ağ geçidi, bir alt ağın varsayılan Internet hedefinin yerini alır ve bir UDR yapılandırması gerekmez. Giden Çeviriler, sanal makineler tarafından oluşturulan istek üzerine sağlanır.

>[!IMPORTANT]
>Giden akışlar, NAT ağ geçidine en az bir genel IP adresi atamakla başarılı olmayacaktır.

>[!NOTE]
> Sanal makine ayırmaları başına garanti duyuyorsanız veya örnek düzeyinde veya örnek havuzlarında giden bağlantı tanımlamak ya da kaynak ağ adresi çevirisi (SNAT) bağlantı noktalarını belirli örneğe garantilemek için ayrıntı düzeyi kullanmanız gerekiyorsa [Standart ](../load-balancer/load-balancer-standard-overview.md)Load Balancer havuz tabanlı giden kaynak ağ adresi çevirisini (SNAT) tanımlamak için [giden kurallarıyla](../load-balancer/load-balancer-outbound-rules-overview.md) Load Balancer.   Bu esneklik, farklı iş yükleri birleştirildiğinde önemli ölçüde artan karmaşıklığa ve ek planlamaya sahiptir veya iş yüklerinin giden bağlantısı doğru şekilde ayrı bir örneğe doğru şekilde boyutlandırılamaz.

## <a name="combination-of-inbound-and-outbound-scenarios-and-skus"></a>Gelen ve giden senaryoların ve SKU 'ların birleşimi

Standart genel IP ile aynı alt ağlarda gelen ve giden senaryolar oluşturabilir ve bunlara dayalı hizmetler de dahil olmak üzere sanal ve standart ortak yük dengeleyici uç noktalarına örnek düzeyinde erişim sağlayabilirsiniz. NAT ağ geçidi ve standart genel IP adresi ve standart ortak yük dengeleyici akış yönü farkındır. Giden akışlar için NAT ağ geçidi, diğer tüm giden senaryoların yerini alır ve sanal makinelerde özel olarak [giden bağlantı](../load-balancer/load-balancer-outbound-connections.md) ve [örnek düzeyi genel IP adreslerini](../load-balancer/load-balancer-outbound-connections.md) Load Balancer önceliklidir. Gelen kaynaklı akışlar için, bir yük dengeleyicideki örnek düzeyi genel IP adresleri veya genel uç noktalar NAT ağ geçidiyle aynı anda kullanılabilir.  Bu senaryo tamamlandığında, gelen/giden akışlar aynı alt ağlarda başarılı olur.  Bir NSG gereklidir ve gelen kaynaklı trafiğe açıkça izin vermeniz gerekiyorsa, gelen kaynaklı senaryolar için ortaya çıkan senaryo "varsayılan olarak güvenlidir".

Giden davranışı etkilemek için bir sanal ağdaki dağıtımlarınızı alt ağlarda parçalara ayırma olanağınız vardır. Ayrıca, sanal makinelerde örnek düzeyi genel IP adresleri olarak kullanılan genel IP adresleri ve yük dengeleyici, bir alt ağ ve NAT ağ geçidi üzerinde giden bağlantılar için giden kaynak ağ adresi çevirisi (SNAT), farklı bir alt ağ üzerinde aynı sanal ağ. NAT ağ geçitlerini, temel genel IP adresi kaynaklarını veya aynı alt ağdaki temel yük dengeleyici kaynaklarını kullanarak kaynaklarla birleştiremezsiniz. Temel SKU kaynaklarını aynı sanal ağın farklı alt ağına dağıtabilirsiniz.

## <a name="network-address-translation"></a>Ağ adresi çevirisi

Yapılandırma tamamlandıktan sonra, NAT Gateway, bir sanal makine örneğinin özel IP adresini genel IP adreslerine çevirerek tüm TCP ve UDP uygulamaları için bağlantı noktası geçici kaynak ağ adresi çevirisi (SNAT) sağlar.

Özel IP adreslerinden alınan akışlar, yapılandırmanıza bağlı olarak bir veya daha fazla genel IP adresine çevrilir. Kaynak özel IP adresi kaynak ağ adresi çevirisi (SNAT) bir genel IP adresi olarak değiştirildiği için çeviri oluşur. Bağlantı noktası, çeviri sonrasında akışları ayırt etmek ve çok bir/çok eşlemeyi basitleştirmek için kaynak bağlantı noktasının yeniden yazılması anlamına gelir. Genel IP adresi alanında elde edilen akışın kaynak bağlantı noktası numarası, kaynak sanal makinelerin başlangıç akışları olarak isteğe bağlı olarak atanır. Aynı hedef genel IP adresine, IP protokolüne ve hedef bağlantı noktası birleşimine birden çok bağlantı, akış başına ek bir bağlantı noktası kullanır.

Sanal makineler bu alt ağlarda giden bağlantılar yaparken, kaynak bağlantı noktaları isteğe bağlı olarak verilir ve akış tamamlandığında veya boşta kalma zaman aşımına ulaşıldıktan sonra serbest bırakılır. Her genel IP, kullanılacak 55.000 SNAT bağlantı noktalarını sağlar ve birden çok genel IP adresi kullanarak ölçeklendirebilirsiniz. NAT ağ geçidi, giden bağlantı ölçeğini daha fazla artırmak üzere farklı hedeflere akışlar için bağlantı noktalarını yeniden kullanmaya çalışır. 

Aynı alt ağ içinde birden çok iş yükünü kolayca ele alabilirsiniz.  Azure Load Balancer giden bağlantısının aksine, sanal makinenin SNAT bağlantı noktaları tüketiminin en kötü durum senaryosunu önceden ayırmanız veya önceden planlamanız gerekmez. Bunun yerine, yapılandırılan alt ağlardaki tüm sanal makineler için giden bağlantıların toplam hacmini planlarsınız. Her kullanılabilir bağlantı noktası, gerektiğinde tüm örnekler için paylaşılır.

## <a name="timeout-and-tcp-resets"></a>Zaman aşımı ve TCP sıfırlamaları

NAT ağ geçidi, boşta kalma zaman aşımlarını uygular ve mevcut olmayan akışlar için TCP sıfırlamaları (RST) gönderir.  Zaman aşımları 4 dakika (varsayılan) ile 120 dakika arasında yapılandırılabilir. Bir TCP RST paketi, NAT ağ geçidine ulaştığında kaynağa döndürülür ve eşleşen bir bağlantı mevcut değildir. Bir akış, boşta kalma zaman aşımına ulaştığında, akış NAT ağ geçidinden kaldırılır ve bağlantı noktası, belirlenen sonraki akış için kullanılabilir hale gelir. Örneğin, bir TCP bağlantısı boşta kalma zaman aşımına ulaşmışsa ve önceden zaman aşımına uğramış bir bağlantı için ek paketler görüriyorsa kaynağa bir TCP RST gönderilir.  Uygulamanız, gerekirse uç nokta durumunu işaret etmek ve eşitlemek için TCP keepsıca kullanabilir.

## <a name="availability-zones"></a>Kullanılabilirlik alanları

NAT ağ geçitleri, gerektiğinde belirli bir kullanılabilirlik alanına yerleştirilebilir.  

Bir genel IP adresi, NAT ağ geçidiyle aynı Kullanılabilirlik bölgesi ile eşleşmelidir. Genel IP öneki kullanılabilirlik bölgelerini desteklemez ve kullanılamaz.

Bir bölgesel NAT ağ geçidi yapılandırıldığında, NAT ağ geçidinin veri düzlemi, istenen kullanılabilirlik bölgesi ile hizalanır ve yalıtılarak yalıtılmıştır.

NAT ağ geçidinin veri düzlemini belirli bir bölgedeki bir sanal makineyle hizalayabilirsiniz.  Bu hizalamayı, yalnızca alt ağ için yapılandırılmış NAT ağ geçidiyle aynı bölgedeki sanal makineleri içeren bölgesel bir alt ağ oluşturarak yapabilirsiniz.

Bir NAT ağ geçidinin kullanılabilirlik bölgesi yerleşimi değiştirilemez ve NAT ağ geçidini bölgesel veya bölgesel 'ten bölge 'ye dönüştüremezsiniz.

## <a name="outbound-connection-service-comparison"></a>Giden bağlantı hizmeti karşılaştırması

NAT ağ geçidi ve Load Balancer giden bağlantı, farklı senaryolara yöneliktir. Basit, isteğe bağlı ayırma ve sanal ağ düzeyi ölçeğinde havuz merkezli, örnek ayrıntı düzeyi ve daha karmaşık senaryolar dikkate alınması gereken önemli bir işlemdir.  Ayrıca birçok farklı davranış farkı ve planlama konuları vardır.

NAT hizmeti, [giden kuralları](../load-balancer/load-balancer-outbound-rules-overview.md)kullanarak [Standart Load Balancer](../load-balancer/load-balancer-standard-overview.md) için havuz tabanlı tabanlı yapılandırma yerine sanal ağlar için giden bağlantıyı yapılandırmanın daha basit bir yoludur.  Bir alt ağ için giden bağlantıyı kolayca yapılandırabilir ve ölçeklendirebilir ve daha önce mümkün olmaması için senaryolar elde edebilirsiniz.

NAT hizmeti, yük dengeleyici giden SNAT üzerinden önceliklidir, ancak her ikisi de gelen ve giden kaynaklı senaryolar için birleştirilebilir. Gelen kaynaklı senaryoları genel IP adresleriyle ve yük dengeleyici uç noktalarıyla birleştirirseniz, standart genel IP adresi ve standart yük dengeleyiciler için "varsayılan olarak güvenli" ' dan da yararlanabilirsiniz.  Yalnızca bir NSG varsa ve trafiğe açıkça izin verdikten sonra gelen trafiğin akışa izin verilmesi engellenir.

NAT hizmeti, sanal ağınız için öngörülebilir yalnızca giden bağlantı oluşturabilir. Tüm ihtiyacınız olan bağlantı varsa, giden kurallarına sahip bir yük dengeleyici yapılandırmanız, makinelere havuz eklemeniz, SNAT bağlantı noktası ayırmayı hesaplamanız ve tanımlamanız ya da ayrı makinelere genel IP adresleri atamanız gerekmez. Yük dengeleyicinin Yöneticisi yerine, NAT ağ geçidi kaynağı yöneticisine giden bağlantı sorumluluğunu atayabilirsiniz.

NAT hizmeti, yoğun sanal makine örneği ölçeği yerine, içindeki bir sanal ağın veya alt ağların yoğun iş yükü ölçeğinde giden bağlantı planlaması yapmanıza olanak sağlar. Tüm sanal ağın tüm alt ağları için ortak IP adresi sayısını dinamik olarak artırarak ek talebe daha kolay bir şekilde uyum sağlayabilirsiniz. Yük dengeleyici SNAT bağlantı noktası ayırmayı hesaplamak ve yeniden planlamak yerine, NAT hizmeti, giden bağlantılar için kullanılabilir SNAT bağlantı noktalarını paylaşır ve bu kişilere, bunları kolayca öngörülebilir iş yüklerine uyum sağlaması için isteğe bağlı olarak verir.  Yük dengeleyici, belirli bir sanal makine örneği için belirli sayıda SNAT bağlantı noktasını önceden ayırır.  

NAT hizmeti her zaman mevcut olmayan akışlar (örneğin, boşta kalma zaman aşımı nedeniyle) için göndericiye TCP sıfırlama (RST) paketleri döndürür. Standart yük dengeleyici, boşta kalma zaman aşımı süresi boyunca bir bağlantının iki uç noktasına TCP RST paketleri oluşturan [boşta durumunda TCP sıfırlamayı](../load-balancer/load-balancer-tcp-reset.md)etkinleştirmek üzere yapılandırılabilir.

## <a name="regions"></a>Bölgeler

NAT hizmeti bu bölgelerde kullanılabilir
- ABD Doğu 2
- Orta Batı ABD

## <a name="limitations"></a>Sınırlamalar

- NAT hizmeti, aynı alt ağdaki temel genel IP veya temel yük dengeleyiciler ile uyumlu değildir.  Bir NAT ağ geçidi tarafından sunulmayan bir alt ağda bulunması gerekir.
- TCP ve UDP tabanlı uygulama protokolleri desteklenir.
- NAT ağ geçitleri, bir sanal ağın bir veya daha fazla alt ağı üzerinde yapılandırılabilir.
- IPv6 desteklenmez.
- Genel IP öneki bölgesel yerleştirmesini desteklemez ve bir bölgesel NAT ağ geçidiyle kullanılamaz.

## <a name="next-steps"></a>Sonraki adımlar

- [NAT hizmeti yol haritası için geri bildirim sağlayın](https://aka.ms/natuservoice) .
