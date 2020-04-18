---
title: Azure'da SAP HANA ağ mimarisi (Büyük Örnekler) | Microsoft Dokümanlar
description: SAP HANA'nın Azure'da (Büyük Örnekler) nasıl dağıtılanabildiğini anlatan ağ mimarisi.
services: virtual-machines-linux
documentationcenter: ''
author: msjuergent
manager: bburns
editor: ''
ms.service: virtual-machines-linux
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 07/15/2019
ms.author: juergent
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: b3bc87b183803c0854542d6925af7429b593d2af
ms.sourcegitcommit: 5e49f45571aeb1232a3e0bd44725cc17c06d1452
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/17/2020
ms.locfileid: "81605165"
---
# <a name="sap-hana-large-instances-network-architecture"></a>SAP HANA (Büyük Örnekler) ağ mimarisi

Azure ağ hizmetlerinin mimarisi, SAP uygulamalarının HANA Büyük Örneği'nde başarılı bir şekilde dağıtılmasının önemli bir bileşenidir. Genellikle, Azure'daki SAP HANA (Büyük Örnekler) dağıtımları, farklı veritabanları, CPU kaynak tüketimi ve bellek kullanımı boyutlarına sahip birkaç farklı SAP çözümüne sahip daha büyük bir SAP ortamına sahiptir. Tüm BT sistemlerinin azure'da zaten bulunmamış olması olasıdır. SAP manzara genellikle netWeaver ve S/4HANA ve SAP HANA ve diğer DBMS karışımını kullanarak bir DBMS noktası ve SAP uygulama açısından da melezdir. Azure, Azure'da farklı DBMS, NetWeaver ve S/4HANA sistemlerini çalıştırmanızı sağlayan farklı hizmetler sunar. Azure, azure'u şirket içi yazılım dağıtımlarınıza sanal bir veri merkezi gibi göstermek için ağ teknolojisi de sunar

Tam BT sistemleriniz Azure'da barındırılan sürece. Azure ağ işlevi, Azure'un sanal bir veri merkezi niz gibi görünmesi için şirket içi dünyayı Azure varlıklarınızla bağlamak için kullanılır. Kullanılan Azure ağ işlevi: 

- Azure sanal ağları, şirket içi ağ varlıklarınıza bağlanan [ExpressRoute](https://azure.microsoft.com/services/expressroute/) devresine bağlıdır.
- Şirket içinde Azure'a bağlanan bir ExpressRoute devresinin en az [1 Gbps veya daha yüksek](https://azure.microsoft.com/pricing/details/expressroute/)bant genişliği olmalıdır. Bu minimum bant genişliği, VM'lerde çalışan şirket içi sistemler ve sistemler arasında veri aktarımı için yeterli bant genişliği sağlar. Ayrıca, şirket içi kullanıcılardan Azure sistemlerine bağlantı için yeterli bant genişliği sağlar.
- Azure'daki tüm SAP sistemleri, birbirleriyle iletişim kurmak için sanal ağlarda ayarlanır.
- Şirket içinde barındırılan Etkin Dindizi ve DNS, şirket içinden ExpressRoute aracılığıyla Azure'a genişletilir veya Azure'da tamamlanır.

HANA Büyük Örnekleri'nin Azure veri merkezi ağ dokusuna entegre edilmesi durumunda Azure ExpressRoute teknolojisi de kullanılır


> [!NOTE] 
> Belirli bir Azure bölgesindeki HANA Büyük Örnek damgasında yalnızca bir Azure aboneliği ne kadar bağlı olabilir. Tam tersine, tek bir HANA Büyük Örnek damgası kiracısı yalnızca bir Azure aboneliğine bağlanabilir. Bu gereksinim, Azure'daki diğer faturalandırılabilir nesnelerle tutarlıdır.

Azure'daki SAP HANA (Büyük Örnekler) birden çok farklı Azure bölgesinde dağıtılırsa, HANA Büyük Örnek damgasında ayrı bir kiracı dağıtılır. Bu örnekler aynı SAP ortamının parçası olduğu sürece her ikisini de aynı Azure aboneliği altında çalıştırabilirsiniz. 

> [!IMPORTANT] 
> Azure'da (Büyük Örnekler) SAP HANA ile yalnızca Azure Kaynak Yöneticisi dağıtım yöntemi desteklenir.

 

## <a name="additional-virtual-network-information"></a>Ek sanal ağ bilgileri

Bir sanal ağı ExpressRoute'a bağlamak için bir Azure ExpressRoute ağ geçidi oluşturulması gerekir. Daha fazla bilgi için [ExpressRoute ağ geçitleri hakkında](../../../expressroute/expressroute-about-virtual-network-gateways.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)bakın. 

Azure ExpressRoute ağ geçidi, ExpressRoute ile Azure dışındaki bir altyapıya veya Azure Büyük Örnek damgasına kullanılır. Azure ExpressRoute ağ geçidini, bu bağlantılar farklı Microsoft kurumsal kenar yönlendiricilerinden geldiği sürece en fazla dört farklı ExpressRoute devresine bağlayabilirsiniz. Daha fazla bilgi için, [Azure'da SAP HANA (Büyük Örnekler) altyapısı na ve bağlantıya](hana-overview-infrastructure-connectivity.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)bakın. 

> [!NOTE] 
> ExpressRoute ağ geçidi ile elde edebileceğiniz maksimum iş, ExpressRoute bağlantısı kullanılarak 10 Gbps'dir. Sanal ağda bulunan bir VM ile şirket içinde bir sistem (tek bir kopya akışı olarak) arasında dosya kopyalamak, farklı ağ geçidi SUS'larının tam iş akışını sağlamaz. ExpressRoute ağ geçidinin tüm bant genişliğinden yararlanmak için birden çok akış kullanın. Veya farklı dosyaları tek bir dosyanın paralel akışlarında kopyalamanız gerekir.


## <a name="networking-architecture-for-hana-large-instance"></a>HANA Büyük Örnek için ağ mimarisi
HANA Büyük Örneği'nin ağ mimarisi dört farklı bölüme ayrılabilir:

- Şirket içi ağ ve Azure'a ExpressRoute bağlantısı. Bu bölüm müşterinin etki alanıdır ve ExpressRoute aracılığıyla Azure'a bağlanır. Bu Expressroute devresi tamamen bir müşteri olarak sizin tarafından ödenir. Bant genişliği, şirket içi varlıklarınız ile karşıya bağlandığınız Azure bölgesi arasındaki ağ trafiğini işleyecek kadar büyük olmalıdır. Aşağıdaki şekilde sağ alt bakınız.
- Azure ağ hizmetleri, daha önce tartışıldığı gibi, yine ExpressRoute ağ geçitleri eklenmesi gereken sanal ağlarla birlikte. Bu bölüm, uygulama gereksinimleriniz, güvenlik ve uyumluluk gereksinimleriniz için uygun tasarımları bulmanız gereken bir alandır. HANA Large Instance'ı kullanıp kullanmadığınız, sanal ağ sayısı ve aralarından seçim yapabileceğiniz Azure ağ geçidi SUS'ları açısından göz önünde bulundurulması gereken başka bir noktadır. Şekildeki sağ üst tekine bakın.
- HANA Large Instance'ın ExpressRoute teknolojisi ile Azure'a bağlantısı. Bu bölüm Microsoft tarafından dağıtılır ve işlenir. Tek yapmanız gereken, HANA Large Instance'daki varlıklarınızın dağıtımından sonra ExpressRoute devresini sanal ağlara bağladıktan sonra bazı IP adres aralıkları sağlamaktır. Daha fazla bilgi için, [Azure'da SAP HANA (Büyük Örnekler) altyapısı na ve bağlantıya](hana-overview-infrastructure-connectivity.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)bakın. Azure veri merkezi ağ dokusu ile HANA Büyük Örnek birimleri arasındaki bağlantı için müşteri olarak ek bir ücret yoktur.
- Çoğunlukla sizin için saydam olan HANA Büyük Örnek damgası içinde ağ.

![Azure'da SAP HANA'ya bağlı sanal ağ (Büyük Örnekler) ve şirket içi](./media/hana-overview-architecture/image1-architecture.png)

Şirket içi varlıklarınızın ExpressRoute üzerinden Azure'a bağlanması gereksinimi, HANA Large Instance'ı kullandığınız için değişmez. HANA Büyük Örnek birimlerinde barındırılan HANA örneklerine bağlanan uygulama katmanını barındıran VM'leri çalıştıran bir veya birden çok sanal acasahip olması gereksinimi de değişmez. 

Azure'daki SAP dağıtımları arasındaki farklar şunlardır:

- Müşteri kiracınızın HANA Büyük Örnek birimleri, başka bir ExpressRoute devresi üzerinden sanal ağlarınıza bağlanır. Yük koşullarını ayırmak için, Azure sanal ağı ExpressRoute devreleri ve Azure sanal ağları ile HANA Büyük Örnekleri arasındaki devreler aynı yönlendiricileri paylaşmaz.
- SAP uygulama katmanı ile HANA Büyük Örneği arasındaki iş yükü profili farklı bir niteliktedir ve SAP HANA'dan uygulama katmanına veri aktarımları (sonuç kümeleri) gibi birçok küçük istek ve patlama ile farklı bir niteliktedir.
- SAP uygulama mimarisi, şirket içi ve Azure arasında veri alışverişi yapılan tipik senaryolardan daha fazla ağ gecikmesine karşı daha duyarlıdır.
- Azure ExpressRoute ağ geçidinde en az iki ExpressRoute bağlantısı vardır. Şirket içinde bağlanan bir devre ve HANA Büyük Örnekleri'nden bağlı bir devre. Bu, ExpressRoute Ağ Geçidi'ne bağlanmak için farklı MSEE'lerden sadece iki ek devre ye yer bırakır. Bu kısıtlama ExpressRoute Fast Path kullanımından bağımsızdır. Bağlı tüm devreler, ExpressRoute ağ geçidinin gelen verileri için maksimum bant genişliğini paylaşır.

HANA Büyük Örnek pullarının Revizyon 3'ü ile, VM'ler ve HANA Büyük Örnek birimleri arasında yaşanan ağ gecikmesi, tipik bir VM-vm ağ gidiş-dönüş gecikmesinden daha yüksek olabilir. Azure bölgesine bağlı olarak, ölçülen değerler SAP Note #1100926 ortalamasının altında olarak sınıflandırılan 0,7 ms gidiş-dönüş gecikme süresini aşabilir [- SSS: Ağ performansı.](https://launchpad.support.sap.com/#/notes/1100926/E) Azure VM ve HANA Büyük Örnek birimi arasındaki ağ gidiş-dönüş gecikmesini ölçmek için Azure Bölgesine ve araca bağlı olarak, ölçülen gecikme si 2 milisaniyeye kadar olabilir. Bununla birlikte, müşteriler SAP HANA tabanlı üretim SAP uygulamalarını SAP HANA Büyük Örneği'nde başarıyla dağıtıyor. Azure HANA Büyük Örneği'nde iş süreçlerinizi kapsamlı bir şekilde test ettiğinizden emin olun. ExpressRoute Fast Path adı verilen yeni bir işlev, HANA Büyük Örnekleri ile Azure'daki uygulama katmanı VM'leri arasındaki ağ gecikmesini önemli ölçüde azaltabilir (aşağıya bakın). 

HANA Büyük Örnek damgalarının Revizyonu 4 ile, HANA Büyük Örnek damgasına yakın olarak dağıtılan Azure VM'leri arasındaki ağ gecikmesi, [SAP Note #1100926 - SSS:](https://launchpad.support.sap.com/#/notes/1100926/E) Azure ExpressRoute Hızlı Yolu yapılandırılırsa ortalama sınıflandırmayı karşılayacak şekilde yaşanır (aşağıya bakın). Azure VM'leri, REVIZYON 4'ün HANA Büyük Örnek birimlerine yakın bir yerde dağıtmak için [Azure Yakınlık Yerleşim Gruplarından](https://docs.microsoft.com/azure/virtual-machines/linux/co-location)yararlanmanız gerekir. Sapma 4 barındırılan HANA Büyük Örnek birimleriyle aynı Azure veri merkezinde SAP uygulama katmanını bulmak için yakınlık yerleşim işlemi gruplarının kullanılabilir ışığı, [SAP uygulamalarıyla en iyi ağ gecikmesi için Azure Yakınlık ışı Bulma Grupları'](sap-proximity-placement-scenarios.md)nda anlatılıyor.

VM'ler ve HANA Büyük Örnek arasında belirleyici ağ gecikmesi sağlamak için ExpressRoute ağ geçidi SKU'nun seçimi esastır. Şirket içi ve VM'ler arasındaki trafik düzeninin aksine, VM'ler ve HANA Büyük Örnek arasındaki trafik deseni, iletilecek küçük ancak yüksek istek ve veri hacimleri patlamaları gelişebilir. Bu tür patlamaları iyi işlemek için UltraPerformance ağ geçidi SKU'nun kullanılmasını şiddetle tavsiye ediyoruz. HANA Large Instance SKU'ların Type II sınıfı için, UltraPerformance ağ geçidi SKU'nun ExpressRoute ağ geçidi olarak kullanılması zorunludur.

> [!IMPORTANT] 
> SAP uygulaması ve veritabanı katmanları arasındaki genel ağ trafiği göz önüne alındığında, yalnızca sanal ağlar için HighPerformance veya UltraPerformance ağ geçidi SUS'ları Azure'da SAP HANA'ya (Büyük Örnekler) bağlanmak için desteklenir. HANA Büyük Örnek Tip II SKU'lar için, yalnızca UltraPerformance ağ geçidi SKU ExpressRoute ağ geçidi olarak desteklenir. ExpressRoute Hızlı Yolu kullanırken özel durumlar geçerlidir (aşağıya bakın)

### <a name="expressroute-fast-path"></a>ExpressRoute Hızlı Yol
Gecikme süresini azaltmak için ExpressRoute Fast Path, HANA Large Instances'ın SAP uygulaması VM'lerini barındıran Azure sanal ağlarına özel bağlantısı için Mayıs 2019'da tanıtıldı ve piyasaya sürüldü. Şimdiye kadar kullanıma alınan çözümdeki en büyük fark, VM'ler ve HANA Büyük Örnekleri arasındaki veri akışlarının artık ExpressRoute ağ geçidinden yönlendirilen olmamasıdır. Bunun yerine, Azure sanal ağının alt ağında atanan VM'ler, ilgili kuruluş kenar yönlendiricisiyle doğrudan iletişim kurar. 

> [!IMPORTANT] 
> ExpressRoute Hızlı Yol işlevi, SAP uygulaması VM'lerini çalıştıran alt ağların HANA Büyük Örnekleri'ne bağlanan aynı Azure sanal ağında olmasını gerektirir. Doğrudan HANA Büyük Örnek birimlerine bağlı Azure sanal ağına bakan Azure sanal ağlarında bulunan Sanal Ağlarda bulunan Sanal M'ler ExpressRoute Hızlı Yol'dan yararlanmıyor. Sonuç olarak, ExpressRoute devrelerinin bir hub sanal ağına ve SAP uygulama katmanını (sözcüleri) içeren sanal ağlara bağlandığı tipik hub ve kollu sanal ağ tasarımları bakılırken, ExpressRoute Fast Path tarafından optimizasyon çalışmaz. Addtion olarak, ExpressRoute Hızlı Yol kullanıcı tanımlı yönlendirme kuralları (UDR) bugün desteklemez. Daha fazla bilgi için [ExpressRoute sanal ağ ağ geçidi ve FastPath'e](https://docs.microsoft.com/azure/expressroute/expressroute-about-virtual-network-gateways)bakın. 


ExpressRoute Hızlı Yol'un nasıl yapılandırılabildiğini hakkında daha fazla bilgi için, sanal [ağı HANA'nın büyük örneklerine bağlayın](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-connect-vnet-express-route)belgesini okuyun.    

> [!NOTE]
> ExpressRoute Hızlı Yol'un çalışması için UltraPerformance ExpressRoute ağ geçidi gereklidir


## <a name="single-sap-system"></a>Tek SAP sistemi

Daha önce gösterilen şirket içi altyapı ExpressRoute üzerinden Azure'a bağlanır. ExpressRoute devresi bir Microsoft kurumsal kenar yönlendiricisine (MSEE) bağlanır. Daha fazla bilgi için [ExpressRoute teknik genel bakış](../../../expressroute/expressroute-introduction.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)bilgisine bakın. Rota oluşturulduktan sonra Azure omurgasına bağlanır.

> [!NOTE] 
> Azure'da SAP manzaralarını çalıştırmak için, SAP ortamında Azure bölgesine en yakın kurumsal kenar yönlendiricisine bağlanın. HANA Büyük Örnek damgaları, Azure IaaS'taki VM'ler ile HANA Büyük Örnek damgaları arasındaki ağ gecikmesini en aza indirmek için özel kurumsal kenar yönlendirici aygıtları aracılığıyla bağlanır.

SAP uygulama örneklerini barındıran VM'ler için ExpressRoute ağ geçidi, şirket içi bağlanan bir ExpressRoute devresine bağlanır. Aynı sanal ağ, Büyük Örnek damgalarına bağlanmaya adanmış ayrı bir kuruluş kenar yönlendiricisine bağlanır. ExpressRoute Hızlı Yol'u kullanarak, HANA Büyük Örnekleri'nden SAP uygulama katmanı VM'lere gelen veri akışı artık ExpressRoute ağ geçidinden yönlendirilen değildir ve bu da ağ gidiş-dönüş gecikme süresini azaltır.

Bu sistem, tek bir SAP sisteminin basit bir örneğidir. SAP uygulama katmanı Azure'da barındırılır. SAP HANA veritabanı Azure'da (Büyük Örnekler) SAP HANA'da çalışır. Varsayım, 2 Gbps veya 10 Gbps iş çıkışLı ExpressRoute ağ geçidi bant genişliğinin bir darboğaz teşkil etmediğidir.

## <a name="multiple-sap-systems-or-large-sap-systems"></a>Birden fazla SAP sistemi veya büyük SAP sistemleri

Azure'da (Büyük Örnekler) SAP HANA'ya bağlanmak için birden fazla SAP sistemi veya büyük SAP sistemi dağıtılırsa, ExpressRoute ağ geçidinin iş bölümü bir darboğaza dönüşebilir. Veya farklı Azure sanal ağlarında üretim ve üretim dışı sistemleri yalıtmak istiyorsunuz. Böyle bir durumda, uygulama katmanlarını birden çok sanal ağa bölün. Ayrıca, şu gibi durumlar için HANA Büyük Örnek'e bağlanan özel bir sanal ağ da oluşturabilirsiniz:

- Hana Büyük Örneği'ndeki HANA örneklerinden nfs hisselerine ev sahipliği yapan Azure'daki bir VM'ye doğrudan yedekleme yapmak.
- HANA Büyük Örnek birimlerinden Azure'da yönetilen disk alanına büyük yedeklemeleri veya diğer dosyaları kopyalama.

HANA Büyük Örnekleri ve Azure arasında toplu veri aktarımı için depolamayı yöneten VM'leri barındırmak için ayrı bir sanal ağ kullanın. Bu düzenleme, BÜYÜK Dosya veya veri aktarımının HANA Large Instance'dan Azure'a SAP uygulama katmanını çalıştıran VM'lere hizmet veren ExpressRoute ağ geçidi üzerindeki etkilerini önler. 

Daha ölçeklenebilir ağ mimarisi için:

- Tek ve daha büyük bir SAP uygulama katmanı için birden çok sanal ağdan yararlanın.
- Dağıtılan her SAP sistemi için ayrı bir sanal ağ dağıtın, bu SAP sistemlerini aynı sanal ağ altında ayrı alt ağlarda birleştirerek karşılaştırın.

  Azure'da SAP HANA (Büyük Örnekler) için daha ölçeklenebilir ağ mimarisi:

![SAP uygulama katmanını birden çok sanal ağ üzerinden dağıtma](./media/hana-overview-architecture/image4-networking-architecture.png)

Kurallara ve kısıtlamalara bağlı olarak, farklı SAP sistemlerinin VM'lerini barındıran farklı sanal ağlar arasında uygulamak istiyorsanız, bu sanal ağlara bakmalısınız. Sanal ağ eşleme hakkında daha fazla bilgi için [Sanal ağ eşleme'ye](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview)bakın.


## <a name="routing-in-azure"></a>Azure'da Yönlendirme

Varsayılan dağıtım olarak, Azure'daki SAP HANA (Büyük Örnekler) için üç ağ yönlendirme hususu önemlidir:

* Azure'daki SAP HANA'ya (Büyük Örnekler) yalnızca Azure VM'leri ve özel ExpressRoute bağlantısı üzerinden erişilebilir, doğrudan şirket içinden değil. Microsoft tarafından size teslim edildiği üzere şirket içi HANA Büyük Örnek birimlerine doğrudan erişim hemen mümkün değildir. Geçişli yönlendirme kısıtlamaları, SAP HANA Büyük Örneği için kullanılan geçerli Azure ağ mimarisinden kaynaklanmaktadır. Bazı yönetim istemcileri ve şirket içinde çalışan SAP Solution Manager gibi doğrudan erişim eki ihtiyacı olan uygulamalar SAP HANA veritabanına bağlanabilir. İstisnalar için 'HANA Büyük Örneklerine Doğrudan Yönlendirme' bölümünü kontrol edin.

* Olağanüstü durum kurtarma için iki farklı Azure bölgesinde dağıtılan HANA Büyük Örnek birimlerine sahipseniz, geçmişte uygulanan geçici yönlendirme kısıtlamaları. Başka bir deyişle, bir bölgedeki HANA Büyük Örnek biriminin IP adresleri (örneğin, ABD Batısı) başka bir bölgede (örneğin, ABD Doğusu) dağıtılan bir HANA Büyük Örnek birimine yönlendirilmedi. Bu kısıtlama, bölgeler arasında Azure ağ eşleme sinin kullanımından veya HANA Büyük Örnek birimlerini sanal ağlara bağlayan ExpressRoute devrelerini birbirine bağlamadan bağımsızdı. Grafik gösterimi için , "HANA Büyük Örnek birimlerini birden çok bölgede kullanın" bölümündeki şilete bakın. Dağıtılan mimariyle birlikte gelen bu kısıtlama, HANA Sistem Çoğaltma'nın felaket kurtarma işlevi olarak hemen kullanılmasını yasakladı. Son değişiklikler için 'HANA Büyük Örnek birimlerini birden çok bölgede kullanın' bölümüne bakın. 

* Azure'daki SAP HANA (Büyük Örnekler) birimlerinde, HANA Büyük Örnek dağıtımını talep ederken gönderdiğiniz sunucu IP havuzu adres aralığından atanmış bir IP adresi vardır. Daha fazla bilgi için, [Azure'da SAP HANA (Büyük Örnekler) altyapısı na ve bağlantıya](hana-overview-infrastructure-connectivity.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)bakın. Bu IP adresine, Azure sanal ağlarını HANA Büyük Örnekleri'ne bağlayan Azure abonelikleri ve devresi aracılığıyla erişilebilir. Bu sunucu IP havuzu adres aralığından atanan IP adresi doğrudan donanım birimine atanır. Bu çözümün ilk dağıtımlarında olduğu gibi artık NAT aracılığıyla *atanmıyor.* 

### <a name="direct-routing-to-hana-large-instances"></a>HANA Büyük Örnekleridoğrudan Yönlendirme

Varsayılan olarak, geçişli yönlendirme bu senaryolarda çalışmaz:

* HANA Büyük Örnek birimleri ve şirket içi dağıtım arasında.

* İki farklı bölgede dağıtılan HANA Büyük Örnek yönlendirmesi arasında.

Bu senaryolarda geçişli yönlendirmeyi etkinleştirmenin üç yolu vardır:

- Verileri yönlendirmek için bir ters proxy, ve gelen. Örneğin, HANA Büyük Örnekleri'ne ve sanal bir güvenlik duvarı/trafik yönlendirme çözümü olarak şirket içi ağına bağlanan Azure sanal ağında dağıtılan Trafik Yöneticisi ile F5 BIG-IP, NGINX.
- Şirket içi konumlar ve HANA Büyük Örnek birimleri arasında veya farklı bölgelerdeki HANA Büyük Örnek birimleri arasında yönlendirmeyi etkinleştirmek için Bir Linux VM'de [IPTable kurallarını](http://www.linuxhomenetworking.com/wiki/index.php/Quick_HOWTO_%3a_Ch14_%3a_Linux_Firewalls_Using_iptables#.Wkv6tI3rtaQ) kullanma. IpTable çalıştıran VM'nin HANA Büyük Örnekleri'ne ve şirket içi sanal ağa bağlanan Azure sanal ağında dağıtılması gerekir. VM'nin buna göre boyutlandırılması gerekir, böylece VM'nin ağ kullanılabilirliği beklenen ağ trafiği için yeterlidir. VM ağ bant genişliği hakkında ayrıntılı bilgi için Azure'daki Linux sanal makinelerin makale Boyutlarını kontrol [edin.](https://docs.microsoft.com/azure/virtual-machines/linux/sizes?toc=%2fazure%2fvirtual-network%2ftoc.json)
- [Azure Güvenlik Duvarı,](https://azure.microsoft.com/services/azure-firewall/) şirket içi ve HANA Büyük örnek birimleri arasında doğrudan trafiği etkinleştirmek için başka bir çözüm olacaktır. 

Bu çözümlerin tüm trafiği bir Azure sanal ağı üzerinden yönlendirilir ve bu nedenle trafik kullanılan yumuşak cihazlar veya Azure Ağ Güvenlik Grupları tarafından ayrıca sınırlandırılabilir, böylece şirket içi belirli IP adresleri veya IP adresleri nin ENGELLENMESIne veya HANA Büyük Örnekleri'ne erişilmesine açıkça izin verilebilir. 

> [!NOTE]  
> Üçüncü taraf ağ cihazları veya IPTable'ları içeren özel çözümlerin uygulanması ve desteğinin Microsoft tarafından sağlanmadığını unutmayın. Destek, kullanılan bileşenin satıcısı veya entegratörü tarafından sağlanmalıdır. 

#### <a name="express-route-global-reach"></a>Ekspres Rota Küresel Erişim
Microsoft [ExpressRoute Global Reach](https://docs.microsoft.com/azure/expressroute/expressroute-global-reach)adlı yeni bir işlev tanıttı. Global Reach, HANA Büyük Örnekleri için iki senaryoda kullanılabilir:

- Farklı bölgelerde dağıtılan HANA Büyük Örnek birimlerinize şirket içi doğrudan erişimi etkinleştirin
- Farklı bölgelerde dağıtılan HANA Büyük Örnek birimleriniz arasında doğrudan iletişimi etkinleştirin


##### <a name="direct-access-from-on-premises"></a>Şirket içinden Doğrudan Erişim
Global Erişimin sunulduğu Azure bölgelerinde, şirket içi ağınızı HANA Büyük Örnek birimlerinize de bağlanan Azure sanal ağına bağlayan ExpressRoute devreniz için Global Reach işlevini etkinleştirme isteğinde bulunabilirsiniz. ExpressRoute devrenizin şirket içi tarafı için bazı maliyet etkileri vardır. Fiyatlar için [Global Reach Eklentisi](https://azure.microsoft.com/pricing/details/expressroute/)fiyatlarını kontrol edin. HANA Büyük Örnek birimi(ler) azure'a bağlayan devreyle ilgili ek bir ücret ödemezsiniz. 

> [!IMPORTANT]  
> HANA Büyük Örnek birimleriniz ve şirket içi varlıklarınız arasında doğrudan erişim sağlamak için Global Reach'i kullanmak durumunda, ağ verileri ve denetim akışı Azure sanal ağları üzerinden değil, doğrudan Microsoft kurumsal değişim yönlendiricileri arasında **yönlendirilir.** Sonuç olarak, bir Azure sanal ağında dağıttığınız NSG veya ASG kuralları veya herhangi bir güvenlik duvarı, NVA veya proxy'ye dokunulmaz. **Şirket içi HANA Büyük örnek birimleri kısıtlamalarına ve HANA'ya erişim izinlerine doğrudan erişimi etkinleştirmek için ExpressRoute Global Reach kullanıyorsanız, şirket içi taraftaki güvenlik duvarlarında tanımlanmalıdır** 

##### <a name="connecting-hana-large-instances-in-different-azure-regions"></a>HANA Büyük Örnekleri'ni farklı Azure bölgelerinde bağlama
Aynı şekilde, ExpressRoute Global Reach şirket içi HANA Büyük Örnek birimlerine bağlanmak için kullanılabildiği için, sizin için iki farklı bölgede dağıtılan iki HANA Büyük Örnek kiracısını bağlamak için kullanılabilir. Yalıtım, HANA Büyük Örnek kiracılarınızın her iki bölgede de Azure'a bağlanmak için kullandığı ExpressRoute devreleridir. İki farklı bölgede dağıtılan iki HANA Büyük Örnek kiracıyı bağlamak için ek ücret yoktur. 

> [!IMPORTANT]  
> Farklı HANA Büyük örnek kiracılar arasındaki ağ trafiğinin veri akışı ve denetim akışı azure ağları üzerinden yönlendirilmez. Sonuç olarak, iki HANA Büyük Örneği kiracınız arasındaki iletişim kısıtlamalarını uygulamak için Azure işlevselliğini veya NV'lerini kullanamazsınız. 

ExpressRoute Global Reach'in nasıl etkinleştirileceği hakkında daha fazla bilgi için, sanal [ağı HANA'nın büyük örneklerine bağlayın](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-connect-vnet-express-route)belgesini okuyun.


## <a name="internet-connectivity-of-hana-large-instance"></a>HANA Büyük Örneği'nin Internet bağlantısı
HANA Büyük Örnek doğrudan internet bağlantısı *yok.* Örnek olarak, bu sınırlama işletim sistemi görüntüsünü doğrudan işletim sistemi satıcısına kaydetme yeteneğinizi kısıtlar. Yerel SUSE Linux Enterprise Server Subscription Management Tool sunucunuz veya Red Hat Enterprise Linux Subscription Manager ile çalışmanız gerekebilir.

## <a name="data-encryption-between-vms-and-hana-large-instance"></a>VM'ler ve HANA Büyük Örneği arasında veri şifreleme
HANA Büyük Örnek ve VM'ler arasında aktarılan veriler şifrelenmez. Ancak, tamamen HANA DBMS tarafı ile JDBC/ODBC tabanlı uygulamalar arasındaki değişim için trafiğin şifresini etkinleştirebilirsiniz. Daha fazla bilgi için [sap tarafından bu belgelere](https://help.sap.com/viewer/102d9916bf77407ea3942fef93a47da8/1.0.11/en-US/dbd3d887bb571014bf05ca887f897b99.html)bakın.

## <a name="use-hana-large-instance-units-in-multiple-regions"></a>HANA Büyük Örnek birimlerini birden çok bölgede kullanma

Olağanüstü durum kurtarma kurulumlarını gerçekleştirmek için birden çok Azure bölgesinde SHANA Büyük Örnek birimlerine sahip olmanız gerekir. Azure [Global Vnet Peering] kullanılarak bile, varsayılan olarak geçişli yönlendirme iki farklı bölgede HANA Büyük Örnek kiracılar arasında çalışmıyor. Ancak, Global Reach, iki farklı bölgede sağlamış olduğunuz HANA Büyük Örnek birimleri arasındaki iletişim yolunu açar. ExpressRoute Global Reach'in bu kullanım senaryosu şunları sağlar:

 - Herhangi bir ek yakınlık veya güvenlik duvarı olmadan HANA Sistem Çoğaltma
 - Sistem kopyalarını veya sistem yenilemelerini gerçekleştirmek için iki farklı bölgede HANA Büyük Örnek birimleri arasında yedekleme kopyalama


![Farklı Azure bölgelerindeki Azure Büyük Örnek damgalarına bağlı sanal ağ](./media/hana-overview-architecture/image8-multiple-regions.png)

Şekil, her iki bölgede de farklı sanal ağların, her iki Azure bölgesinde (gri çizgiler) Azure'da (Büyük Örnekler) SAP HANA'ya bağlanmak için kullanılan iki farklı ExpressRoute devresine nasıl bağlandığı gösterilmektedir. Bu iki çapraz bağlantının nedeni, her iki taraftaki MSEE'lerin kesintisinden korunmaktır. İki Azure bölgesinde yer alan iki sanal ağ arasındaki iletişim akışının, iki farklı bölgede (mavi noktalı çizgi) iki sanal anın [genel olarak ele](https://blogs.msdn.microsoft.com/azureedu/2018/04/24/how-to-setup-global-vnet-peering-in-azure/) alınması gerekir. Kalın kırmızı çizgi, iki farklı bölgede bulunan kiracılarınızın HANA Büyük Örnek birimlerinin birbiriyle iletişim kurmasını sağlayan ExpressRoute Global Reach bağlantısını açıklar. 

> [!IMPORTANT] 
> Birden çok ExpressRoute devresi kullandıysanız, trafiğin düzgün yönlendirilmesini sağlamak için AS Yolu ön bekleyeni ve Yerel Tercih BGP ayarları kullanılmalıdır.

**Sonraki adımlar**
- [SAP HANA (Büyük Örnekler) depolama mimarisine](hana-storage-architecture.md) bakın
