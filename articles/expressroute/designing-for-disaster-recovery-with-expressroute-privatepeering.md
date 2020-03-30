---
title: 'Azure ExpressRoute: Olağanüstü durum kurtarma için tasarım'
description: Bu sayfa, Azure ExpressRoute kullanırken olağanüstü durum kurtarma için mimari öneriler sağlar.
services: expressroute
author: rambk
ms.service: expressroute
ms.topic: article
ms.date: 05/25/2019
ms.author: rambala
ms.openlocfilehash: 726a014983c0da959d72b7976fef2ebb2c6e9b9e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74076696"
---
# <a name="designing-for-disaster-recovery-with-expressroute-private-peering"></a>ExpressRoute özel bakışile olağanüstü durum kurtarma için tasarım

ExpressRoute, Microsoft kaynaklarına operatör sınıfı özel ağ bağlantısı sağlamak için yüksek kullanılabilirlik için tasarlanmıştır. Başka bir deyişle, Microsoft ağındaki ExpressRoute yolunda tek bir hata noktası yoktur. ExpressRoute devresinin kullanılabilirliğini en üst düzeye çıkarmak için tasarım konuları için [ExpressRoute ile yüksek kullanılabilirlik için Tasarım][HA]bölümüne bakın.

Ancak, Murphy'nin popüler atasözü ele alarak -*eğer bir şey yanlış gidebilir, bu*dikkate - bu makalede, bize tek bir ExpressRoute devresi kullanılarak ele alınabilir hataları ötesinde çözümler üzerinde durulalım. Başka bir deyişle, bu makalede, coğrafi yedekli ExpressRoute devrelerini kullanarak olağanüstü durum kurtarma için sağlam arka uç ağ bağlantısı oluşturmak için ağ mimarisinin göz önünde bulundurulması gereken hususlara bakalım.

## <a name="need-for-redundant-connectivity-solution"></a>Gereksiz bağlantı çözümüne ihtiyaç

Tüm bölgesel hizmetin (Microsoft, ağ hizmeti sağlayıcıları, müşteri veya diğer bulut hizmeti sağlayıcılarının ki gibi) bozulduğu olanaklar ve durumlar vardır. Bu tür bölgesel geniş hizmet etkisiiçin temel nedeni doğal felaket içerir. Bu nedenle, iş sürekliliği ve görev kritik uygulamalar için olağanüstü durum kurtarma için planlamak önemlidir.   

Görev açısından kritik uygulamalarınızı bir Azure bölgesinde veya şirket içinde veya başka bir yerde çalıştırSanız çalıştırın, başarısız siteniz olarak başka bir Azure bölgesini kullanabilirsiniz. Aşağıdaki makaleler, uygulamalardan ve ön uç erişim perspektiflerinden olağanüstü durum kurtarma adreslerini:

- [Kurumsal ölçekte olağanüstü durum kurtarma][Enterprise DR]
- [Azure Site Recovery ile SMB olağanüstü durum kurtarma][SMB DR]

Görev açısından kritik işlemler için şirket içi ağınız ile Microsoft arasındaki ExpressRoute bağlantısına güveniyorsanız, olağanüstü durum kurtarma planınız coğrafi yedekli ağ bağlantısını da içermelidir. 

## <a name="challenges-of-using-multiple-expressroute-circuits"></a>Birden fazla ExpressRoute devresi kullanmanın zorlukları

Aynı ağ kümesini birden fazla bağlantı kullanarak bağladığınızda, ağlar arasında paralel yollar kurarsınız. Paralel yollar, düzgün bir şekilde tasarlanmadığında, asimetrik yönlendirmeye yol açabilir. Yolda durum sallayan varlıklar (örneğin, NAT, güvenlik duvarı) varsa, asimetrik yönlendirme trafik akışını engelleyebilir.  Genellikle, ExpressRoute özel bakış yolu üzerinde NAT veya Güvenlik Duvarları gibi devlet sel varlıklarla karşılaşmazsınız. Bu nedenle, ExpressRoute özel bakış üzerinden asimetrik yönlendirme mutlaka trafik akışını engellemez.
 
Ancak, bakiye trafiğini, durum durumu olan varlıklara sahip olup olmadığınız anına bakılmaksızın, coğrafi olarak yedekli paralel yollara yüklerseniz, tutarsız ağ performansı yla karşılaşırsınız. Bu makalede, bu zorlukların nasıl ele alınış süreceğini tartışalım.

## <a name="small-to-medium-on-premises-network-considerations"></a>Küçük ve orta ölçekli şirket içi ağ konuları

Aşağıdaki diyagramda gösterilen örnek ağı ele alalım. Örnekte, bir Contoso'nun şirket içi konumu ile Bir Azure bölgesinde Contoso'nun VNet'i arasında coğrafi yedekli ExpressRoute bağlantısı oluşturulur. Diyagramda, düz yeşil çizgi tercih edilen yolu (ExpressRoute 1 üzerinden) gösterir ve noktalı olan stand-by yolunu (ExpressRoute 2 üzerinden) temsil eder.

[![1]][1]

Olağanüstü durum kurtarma için ExpressRoute bağlantısı tasarlarken şunları göz önünde bulundurmanız gerekir:

- coğrafi yedekli ExpressRoute devrelerini kullanarak
- farklı ExpressRoute devresi için farklı servis sağlayıcı ağı(lar) kullanma
- [yüksek kullanılabilirlik][HA] için ExpressRoute devresinin her birini tasarlama
- müşteri ağında farklı bir konumda farklı ExpressRoute devresini sonlandırma

Varsayılan olarak, tüm ExpressRoute yollarında aynı şekilde rotaların reklamını yaparsa, Azure Eşit maliyetli çok (ECMP) yönlendirmesini kullanarak tüm ExpressRoute yollarında şirket içi bağlı trafiği yükler.

Ancak, coğrafi yedekli ExpressRoute devreleri ile farklı ağ yolları (özellikle ağ gecikmesi için) ile farklı ağ performanslarını dikkate almamız gerekir. Normal çalışma sırasında daha tutarlı ağ performansı elde etmek için minimum gecikme süresi sunan ExpressRoute devresini tercih etmek isteyebilirsiniz.

Azure'u aşağıdaki tekniklerden birini kullanarak bir ExpressRoute devresini diğerine tercih etmesi için etkileyebilirsiniz (etkinlik sırasına göre listelenebilir):

- diğer ExpressRoute devresi(ler) ile karşılaştırıldığında tercih edilen ExpressRoute devresi üzerinden daha spesifik bir rota reklam
- sanal ağı tercih edilen ExpressRoute devresine bağlayan bağlantıda daha yüksek Bağlantı Ağırlığını yapılandırma
- daha uzun AS Yolu (AS Path prepend) ile daha az tercih edilen ExpressRoute devresi üzerinden güzergahreklam

### <a name="more-specific-route"></a>Daha spesifik rota

Aşağıdaki diyagram, daha spesifik rota reklamını kullanarak ExpressRoute yol seçimini etkilemeyi göstermektedir. Resimli örnekte, Contoso şirket içi /24 IP aralığı tercih edilen yol (ExpressRoute 1) üzerinden iki /25 adres aralığı ve stand-by yolu (ExpressRoute 2) üzerinden /24 olarak duyurulur.

[![2]][2]

/25,/24'e kıyasla daha spesifik olduğundan, Azure trafiği normal durumda ExpressRoute 1 üzerinden 10.1.11.0/24'e gönderir. ExpressRoute 1'in her iki bağlantısı da aşağı inerse, VNet 10.1.11.0/24 rota reklamını yalnızca ExpressRoute 2 üzerinden görür; ve bu nedenle bekleme devresi bu arıza durumunda kullanılır.

### <a name="connection-weight"></a>Bağlantı ağırlığı

Aşağıdaki ekran görüntüsü, ExpressRoute bağlantısının ağırlığını Azure portalı üzerinden yapılandırılmayı göstermektedir.

[![3]][3]

Aşağıdaki diyagram, bağlantı ağırlığını kullanarak ExpressRoute yol seçimini etkilemeyi göstermektedir. Varsayılan bağlantı ağırlığı 0'dır. Aşağıdaki örnekte, ExpressRoute 1 bağlantısının ağırlığı 100 olarak yapılandırılmıştır. Bir VNet birden fazla ExpressRoute devresi üzerinden reklamı yapılan bir rota öneki aldığında, VNet en yüksek ağırlıktaki bağlantıyı tercih edecektir.

[![4]][4]

ExpressRoute 1'in her iki bağlantısı da aşağı inerse, VNet 10.1.11.0/24 rota reklamını yalnızca ExpressRoute 2 üzerinden görür; ve bu nedenle bekleme devresi bu arıza durumunda kullanılır.

### <a name="as-path-prepend"></a>AS yol prepend

Aşağıdaki diyagram, AS yol prepend kullanarak ExpressRoute yol seçimini etkileyen gösteriş. Diyagramda, ExpressRoute 1 üzerindeki rota reklamı eBGP'nin varsayılan davranışını gösterir. ExpressRoute 2 üzerindeki rota reklamında, şirket içi ağın ASN'si ayrıca rotanın AS yoluna hazırlanır. Aynı rota, eBGP rota seçim sürecine göre birden çok ExpressRoute devresi üzerinden alındığı zaman, VNet en kısa AS yoluna sahip rotayı tercih eder. 

[![5]][5]

ExpressRoute 1'in her iki bağlantısı da aşağı inerse, VNet 10.1.11.0/24 rota reklamını yalnızca ExpressRoute 2 üzerinden görür. Sonuç olarak, uzun AS yolu alakasız hale gelecekti. Bu nedenle, bekleme devresi bu hata durumunda kullanılır.

Bu tekniklerden herhangi birini kullanarak, Azure'un ExpressRoute'unuzdan birini diğerlerine tercih etmesini etkilerseniz, asimetrik akışlardan kaçınmak için şirket içi ağın Azure'a bağlı trafiği için aynı ExpressRoute yolunu da tercih etmesini sağlamanız gerekir. Genellikle, yerel tercih değeri diğerlerine göre bir ExpressRoute devresi tercih şirket içi ağı etkilemek için kullanılır. Yerel tercih, dahili bir BGP (iBGP) ölçüsüdür. En yüksek yerel tercih değerine sahip BGP rotası tercih edilir.

> [!IMPORTANT]
> Belirli ExpressRoute devrelerini bekleme olarak kullandığınızda, bunları etkin bir şekilde yönetmeniz ve başarısız olan işlemi periyodik olarak test etmeniz gerekir. 
> 

## <a name="large-distributed-enterprise-network"></a>Büyük dağıtılmış kurumsal ağ

Büyük dağıtılmış bir kuruluş ağınız olduğunda, birden çok ExpressRoute devreniz olması olasıdır. Bu bölümde, ek stand-by devrelerine gerek kalmadan etkin-etkin ExpressRoute devrelerini kullanarak olağanüstü durum kurtarmanın nasıl tasarlanabildiğini görelim. 

Aşağıdaki diyagramda gösterilen örneği ele alalım. Örnekte, Contoso'nun iki farklı bakış noktasındaki ExpressRoute devreleri aracılığıyla iki farklı Azure bölgesinde iki Contoso IaaS dağıtımına bağlı iki şirket içi konumu vardır. 

[![6]][6]

Olağanüstü durum kurtarma yı nasıl tasarladığımız, coğrafi kavşağın konum (region1/region2 to location2/location1) trafiğinin nasıl yönlendirildiğini nasıl etkilediğiüzerinde bir etkiye sahiptir. Bölge trafiğini farklı şekilde sorgulayan iki farklı felaket mimarisini ele alalım.

### <a name="scenario-1"></a>Senaryo 1

İlk senaryoda, bir Azure bölgesi ile şirket içi ağ arasındaki tüm trafiğin sabit durumda yerel ExpressRoute devresi üzerinden akması gibi olağanüstü durum kurtarma tasarlayalım. Yerel ExpressRoute devresi başarısız olursa, uzak ExpressRoute devresi Azure ve şirket içi ağ arasındaki tüm trafik akışları için kullanılır.

Senaryo 1 aşağıdaki diyagramda gösterilmiştir. Diyagramda, yeşil çizgiler VNet1 ve şirket içi ağlar arasındaki trafik akışı yollarını gösterir. Mavi çizgiler, VNet2 ile şirket içi ağlar arasındaki trafik akışı yollarını gösterir. Düz çizgiler sabit durumda istenen yolu gösterir ve kesik çizgiler sabit durum trafik akışı taşıyan ilgili ExpressRoute devresinin arızası trafik yolunu gösterir. 

[![7]][7]

Yerel gözleyen konum ExpressRoute bağlantısını şirket içi ağ bağlantısı trafiği için tercih etmek için VNet'leri etkilemek için bağlantı ağırlığını kullanarak senaryoyu tasarlayabilirsiniz. Çözümü tamamlamak için, simetrik ters trafik akışını sağlamanız gerekir. Bir ExpressRoute devresini tercih etmek için BGP yönlendiricileriniz arasındaki (ExpressRoute devrelerinin şirket içinde sonlandırıldığının) iBGP oturumunda yerel tercihi kullanabilirsiniz. Çözüm aşağıdaki diyagramda gösterilmiştir. 

[![8]][8]

### <a name="scenario-2"></a>Senaryo 2

Senaryo 2 aşağıdaki diyagramda gösterilmiştir. Diyagramda, yeşil çizgiler VNet1 ve şirket içi ağlar arasındaki trafik akışı yollarını gösterir. Mavi çizgiler, VNet2 ile şirket içi ağlar arasındaki trafik akışı yollarını gösterir. Sabit durumda (diyagramdaki düz çizgilerde), VNet'ler ve şirket içi konumlar arasındaki tüm trafik çoğunlukla Microsoft omurgası üzerinden akar ve yalnızca arıza durumundaki şirket içi konumlar arasındaki ara bağlantıdan akar (noktalı çizgiler diyagramı) bir ExpressRoute.

[![9]][9]

Çözüm aşağıdaki diyagramda gösterilmiştir. Gösterildiği gibi, VNet yol seçimini etkilemek için senaryoyu daha spesifik bir rota (Seçenek 1) veya AS-path prepend (Seçenek 2) kullanarak tasarlayabilirsiniz. Azure'a bağlı trafik için şirket içi ağ rotası seçimini etkilemek için, şirket içi konum arasındaki bağlantıyı daha az tercih edilebilir şekilde yapılandırmanız gerekir. Tercih edilen şekilde interconnection bağlantısını nasıl yapılandırdığınız, şirket içi ağ içinde kullanılan yönlendirme protokolüne bağlıdır. IGP (OSPF veya IS-IS) ile iBGP veya metrik ile yerel tercihk tesniye kullanabilirsiniz.

[![10]][10]


## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, bir ExpressRoute devresi özel eşleme bağlantısının olağanüstü durum kurtarma için nasıl tasarlanabileceğimizi tartıştık. Aşağıdaki makaleler, uygulamalardan ve ön uç erişim perspektiflerinden olağanüstü durum kurtarma adreslerini:

- [Kurumsal ölçekte olağanüstü durum kurtarma][Enterprise DR]
- [Azure Site Recovery ile SMB olağanüstü durum kurtarma][SMB DR]

<!--Image References-->
[1]: ./media/designing-for-disaster-recovery-with-expressroute-pvt/one-region.png "küçük ve orta ölçekli şirket içi ağ hususları"
[Daha]: ./media/designing-for-disaster-recovery-with-expressroute-pvt/specificroute.png "özel rotalar kullanarak" 2 etkili yol seçimi
Azure portalı üzerinden [3]: ./media/designing-for-disaster-recovery-with-expressroute-pvt/configure-weight.png "bağlantı ağırlığı yapılandırma"
Bağlantı ağırlığını kullanarak [4]: ./media/designing-for-disaster-recovery-with-expressroute-pvt/connectionweight.png "etkili yol seçimi"
[5]: ./media/designing-for-disaster-recovery-with-expressroute-pvt/aspath.png "AS yol prepend kullanarak" 5 etkileyen yol seçimi
[6]: ./media/designing-for-disaster-recovery-with-expressroute-pvt/multi-region.png "büyük dağıtılmış şirket içi ağ hususları"
[7]: ./media/designing-for-disaster-recovery-with-expressroute-pvt/multi-region-arch1.png "senaryo 1"
[8]: ./media/designing-for-disaster-recovery-with-expressroute-pvt/multi-region-sol1.png "aktif ExpressRoute devresi çözümü 1"
[9]: ./media/designing-for-disaster-recovery-with-expressroute-pvt/multi-region-arch2.png "senaryo 2"
[10]: ./media/designing-for-disaster-recovery-with-expressroute-pvt/multi-region-sol2.png "aktif ExpressRoute devresi çözümü 2"

<!--Link References-->
[HA]: https://docs.microsoft.com/azure/expressroute/designing-for-high-availability-with-expressroute
[Enterprise DR]: https://azure.microsoft.com/solutions/architecture/disaster-recovery-enterprise-scale-dr/
[SMB DR]: https://azure.microsoft.com/solutions/architecture/disaster-recovery-smb-azure-site-recovery/
[con wgt]: https://docs.microsoft.com/azure/expressroute/expressroute-optimize-routing#solution-assign-a-high-weight-to-local-connection
[AS Path Pre]: https://docs.microsoft.com/azure/expressroute/expressroute-optimize-routing#solution-use-as-path-prepending





