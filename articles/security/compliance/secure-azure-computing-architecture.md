---
title: Güvenli Azure Bilgi İşlem Mimarisi
description: Kurumsal düzeyde bir DMZ mimarisine yönelik bu başvuru mimarisi, ağ sanal gereçlerini ve diğer araçları kullanır. Bu mimari, savunma 'nın güvenli bulut bilgi Işlem mimarisi Işlev gereksinimlerinin departmanını karşılayacak şekilde tasarlandı. Ayrıca, herhangi bir kuruluş için de kullanılabilir. Bu başvuru, Citrix veya F5 gereçlerini kullanan iki otomatik seçenek içerir.
author: jahender
ms.author: jahender
ms.date: 4/9/2019
ms.topic: article
ms.service: security
ms.openlocfilehash: 3a27eac3d4609f1054b0ef6a9417fe2f1ca53ae4
ms.sourcegitcommit: bb8e9f22db4b6f848c7db0ebdfc10e547779cccc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/20/2019
ms.locfileid: "69656638"
---
# <a name="secure-azure-computing-architecture"></a>Güvenli Azure Bilgi İşlem Mimarisi

ABD İş yüklerini Azure 'a dağıtan Savunma Bakanlığı (DoD) müşterileri, güvenli sanal ağlar ayarlamayı ve DoD standartları ve uygulaması tarafından kullanılabilen güvenlik araçlarını ve hizmetlerini yapılandırmayı istedi. 

Savunma bilgileri sistem Kurumu (DıŞA), 2017 içinde [güvenli bulut bilgi Işlem mimarisi (SCCA) Işlevsel gereksinimler belgesi (FRD)](https://dl.dod.cyber.mil/wp-content/uploads/cloud/pdf/SCCA_FRD_v2-9.pdf) yayımlandı. SCCA, savunma bilgileri sistem ağının (DISN) ve ticari bulut sağlayıcısı bağlantı noktalarının güvenliğini sağlamaya yönelik işlevsel hedefleri açıklar. SCCA Ayrıca, görev sahiplerinin bağlantı sınırında bulut uygulamalarının güvenliğini nasıl sağladığını açıklar. Ticari buluta bağlanan her DoD varlığı, SCCA FRD 'da belirtilen yönergeleri izlemelidir.
 
SCCA 'da dört bileşen vardır:
 
- Sınır bulutu erişim noktası (BCAP)
- Sanal veri merkezi güvenlik yığını (VDSS)
- Sanal veri merkezi yönetilen hizmeti (VDMS)
- Güvenilen bulut kimlik bilgileri Yöneticisi (TCCM) 

Microsoft, hem IL4 hem de IL5 iş yükleri için Azure 'da çalışan SCCA gereksinimlerini karşılayan bir çözüm geliştirmiştir. Bu Azure 'a özel çözüm, güvenli Azure bilgi Işlem mimarisi (SACA) olarak adlandırılır. SACA dağıtan müşteriler, SCCA FRD ile uyumlu. Bu kişiler, bağlantı kurulduktan sonra DoD müşterilerinin iş yüklerini Azure 'a taşımasını olanaklı hale getiribilirler.

SCCA Kılavuzu ve mimarileri DoD müşterilerine özeldir, ancak SACA 'daki en son düzeltmeler, güvenilen Internet bağlantısı (TIC) kılavuzlarıyla uyumlu olacak şekilde tasarlanmıştır. En son düzeltmeler Ayrıca, Azure ortamlarını korumak üzere güvenli bir DMZ uygulamak isteyen ticari müşterilere yardımcı olur.


## <a name="secure-cloud-computing-architecture-components"></a>Güvenli bulut bilgi Işlem mimarisi bileşenleri

### <a name="bcap"></a>BCAP

BCAP 'in amacı, bulut ortamındaki kaynaklı saldırıları korumamıza yardımcı olur. BCAP, yetkisiz giriş algılama ve önleme gerçekleştirir. Ayrıca, yetkisiz trafiğe filtre uygular. Bu bileşen, SCCA 'nın diğer bileşenleriyle birlikte bulunabilir. Bu bileşeni fiziksel donanım kullanarak dağıtmanızı öneririz. BCAP güvenlik gereksinimleri aşağıdaki tabloda listelenmiştir.

#### <a name="bcap-security-requirements"></a>BCAP güvenlik gereksinimleri

![BCAP gereksinimleri matrisi](media/bcapreqs.png)


### <a name="vdss"></a>VDSS

VDSS amacı, Azure 'da barındırılan DoD görev sahibi uygulamalarının korunmasından oluşur. VDSS, SCCA 'daki güvenlik işlemlerinin toplu işlemini gerçekleştirir. Azure 'da çalışan uygulamaların güvenliğini sağlamak için trafik incelemesini çalıştırır. Bu bileşen Azure ortamınızda bulunabilir.

#### <a name="vdss-security-requirements"></a>VDSS güvenlik gereksinimleri

![VDSS gereksinimler matrisi](media/vdssreqs.png)

### <a name="vdms"></a>VDMS

VDMS amacı, ana bilgisayar güvenliği ve paylaşılan veri merkezi hizmetleri sağlamaktır. VDMS işlevleri, SCCA 'nizin merkezinde çalıştırılabilir veya görev sahibi, kendi belirli Azure aboneliklerinde bu parçaların parçalarını dağıtabilir. Bu bileşen Azure ortamınızda bulunabilir.

#### <a name="vdms-security-requirements"></a>VDMS güvenlik gereksinimleri

![VDMS gereksinimler matrisi](media/vdmsreqs.png)


### <a name="tccm"></a>TCCM

TCCM bir iş rolüdür. Bu kişi, SCCA 'Yı yönetmekten sorumludur. Görevleri şunlardır: 

- Bulut ortamına hesap erişimi için planlar ve ilkeler oluşturun. 
- Kimlik ve erişim yönetiminin düzgün şekilde çalıştığından emin olun. 
- Bulut kimlik bilgileri yönetim planını koruyun. 

Bu kişi, yetkilendirme resmi tarafından hazırlanmıştır. BCAP, VDSS ve VDMS, TCCM 'nin işlerini gerçekleştirmesi için ihtiyaç duyacağı özellikleri sağlar.

#### <a name="tccm-security-requirements"></a>TCCM güvenlik gereksinimleri

![TCCM gereksinimleri matrisi](media/tccmreqs.png) 

## <a name="saca-components-and-planning-considerations"></a>SACA bileşenleri ve planlama konuları 

SACA başvuru mimarisi, VDSS ve VDMS bileşenlerini Azure 'da dağıtmak ve TCCM 'yi etkinleştirmek için tasarlanmıştır. Bu mimari modüler bir değer. Tüm VDSS ve VDMS parçaları merkezi bir hub 'da bulunabilir. Denetimlerden bazıları görev sahibi alanında veya hatta şirket içinde karşılanabileceği. Microsoft, VDSS ve VDMS bileşenlerini tüm görev sahiplerinin bağlanabilmesi için bir Merkezi Sanal ağda birlikte bulmanızı önerir. Aşağıdaki diyagramda bu mimari gösterilmektedir: 


![SACA başvuru mimarisi diyagramı](media/sacav2generic.png)

SCCA karmaşık pozisyon stratejinizi ve teknik mimarinizi planlarken, her müşteriyi etkilediği için başlangıçtan itibaren aşağıdaki konuları göz önünde bulundurun. Aşağıdaki sorunlar DoD müşterileri ile birlikte geldiğinden planlama ve yürütmeyi yavaşlatacak şekilde eğilimlidir. 

#### <a name="which-bcap-will-your-organization-use"></a>Kuruluşunuzun hangi BCAP kullanması gerekir?
   - DIŞA BUCU:
        - DıŞA, beşgen ve Camp Roberts, CA 'da iki işlemsel BCAPs 'e sahiptir. Üçüncü bir süre içinde çevrimiçi olacak şekilde planlanır. 
        - KCaps 'in hepsi, Azure 'da DoD müşterileri tarafından bağlantı için kullanılabilecek Azure ExpressRoute devreleri vardır. 
        - DıAS, Office 365 gibi Microsoft hizmet olarak yazılım (SaaS) araçlarına abone olmak isteyen DoD müşterileri için kurumsal düzeyde bir Microsoft eşleme oturumuna sahiptir. DıŞA BCAP 'i kullanarak, SACA örneğinizle bağlantı ve eşleme sağlayabilirsiniz. 
    - Kendi BCAP 'nizi oluşturun:
        - Bu seçenek, birlikte bulunan bir veri merkezinde alan kiralayabilmeniz ve Azure 'a bir ExpressRoute bağlantı hattı ayarlamanıza gerek duyar. 
        - Bu seçenek ek onay gerektirir. 
        - Ek onay ve fiziksel bir derleme nedeniyle bu seçenek en çok zaman alır. 
    - DıŞA BCAP kullanmanızı öneririz. Bu seçenek hazır, yerleşik artıklığı vardır ve bugün üretimde çalışan müşterilere sahip olur.
- DoD yönlendirilebilir IP alanı:
    - Kenarunuzda DoD yönlendirilebilir IP alanı kullanmanız gerekir. Bu alanları Azure 'da özel IP alanına bağlamak için NAT kullanma seçeneği kullanılabilir.
    - IP alanı almak için DoD ağ bilgi merkezi (NIC) ile iletişim kurun. Bunun, sistem/ağ onay Işlemi (SNAP) gönderiminizin bir parçası olarak DıŞA 
    - Azure 'da özel adres alanını bağlamak için NAT kullanmayı planlıyorsanız, SACA dağıtmayı planladığınız her bölge için NIC 'den atanan en az bir/24 alt ağ adresi alanı gerekir.
- Yedeklilik:
    - Bir SACA örneğini en az iki bölgeye dağıtın. DoD bulutu 'nda, hem kullanılabilir DoD bölgelerine dağıtın.
    - Ayrı bir ExpressRoute devreleri aracılığıyla en az iki BCAPs 'e bağlanın. Her iki ExpressRoute bağlantısı da her bir bölgenin SACA örneğiyle bağlantılı olabilir. 
- DoD bileşenine özgü gereksinimler:
    - Kuruluşunuz SCCA gereksinimleri dışında belirli gereksinimlere sahip mi? Bazı kuruluşların belirli IP gereksinimleri vardır.
- SACA modüler bir mimaridir:
    - Yalnızca ortamınız için gereken bileşenleri kullanın. 
        - Ağ sanal gereçlerini tek bir katmanda veya çok katmanlı olarak dağıtın.
        - Tümleşik IP 'leri kullanın veya kendi IP 'lerini getirin.
- Uygulamalarınızın ve verilerinizin DoD etkisi düzeyi:
    - Microsoft IL5 bölgelerinde çalışan uygulamalar varsa, IL5 'de SACA örneğinizi oluşturun. Örnek, IL4 uygulamaları ve IL5 önünde kullanılabilir. Büyük olasılıkla bir IL5 uygulamasının önündeki IL4 SACA örneği, acstasyon almaz.

#### <a name="which-network-virtual-appliance-vendor-will-you-use-for-vdss"></a>VDSS için hangi ağ sanal gereç satıcısı kullanacağınızı kullanacaksınız?
Daha önce bahsedildiği gibi, bu SACA başvurusunu çeşitli gereçler ve Azure hizmetlerini kullanarak oluşturabilirsiniz. Microsoft, SACA mimarisini hem F5 hem de Citrix ile dağıtmak için otomatikleştirilmiş çözüm şablonlarına sahiptir. Bu çözümler aşağıdaki bölümde ele alınmıştır.

#### <a name="which-azure-services-will-you-use"></a>Hangi Azure hizmetlerini kullanacaksınız?
- Log Analytics, ana bilgisayar tabanlı koruma ve KIMLIK işlevleri gereksinimlerini karşılayabilecek Azure hizmetleri vardır. Bazı Hizmetler Microsoft IL5 bölgelerinde genel kullanıma açık olmayabilir. Bu durumda, bu Azure hizmetleri gereksinimlerinizi karşılamazsa üçüncü taraf araçları kullanmanız gerekebilir. Rahatımız araçlara ve Azure Native Tooling kullanmanın rahatsıztığa bakın.
- Mümkün olduğunca çok sayıda Azure yerel aracı kullanmanızı öneririz. Bulut güvenliği göz önünde bulundurularak derlenirler ve Azure platformunun geri kalanı ile sorunsuz bir şekilde tümleşir. Farklı SCCA gereksinimlerini karşılamak için aşağıdaki listede Azure yerel araçlarını kullanın:

    - [Azure İzleyici](https://docs.microsoft.com/azure/azure-monitor/overview )
    - [Azure Güvenlik Merkezi](https://docs.microsoft.com/azure/security-center/security-center-intro) 
    - [Azure ağ Izleyicisi](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview) 
    - [Azure Anahtar Kasası.](https://docs.microsoft.com/azure/key-vault/key-vault-whatis) 
    - [Azure Active Directory](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-whatis)
    - [Azure Application Gateway](https://docs.microsoft.com/azure/application-gateway/overview)
    - [Azure Güvenlik Duvarı](https://docs.microsoft.com/azure/firewall/overview) 
    - [Azure ön kapısı](https://docs.microsoft.com/azure/frontdoor/front-door-overview)
    - [Azure Güvenlik grupları](https://docs.microsoft.com/azure/virtual-network/security-overview)
    - [Azure DDoS koruması](https://docs.microsoft.com/azure/virtual-network/ddos-protection-overview)
    - [Azure Sentinel](https://docs.microsoft.com/azure/sentinel/overview)
- Boyutlandırma
    - Bir boyutlandırma alıştırması tamamlanması gerekir. SACA örneği ve ağ aktarım hızı gereksinimleri ile sahip olabileceğiniz eşzamanlı bağlantı sayısına bakın. 
    - Bu adım kritiktir. VM 'Leri boyutlandırmanıza ve SACA örneğiniz için kullandığınız çeşitli satıcıların gerektirdiği lisansları belirlemesine yardımcı olur. 
    - Bu boyutlandırma alıştırması olmadan iyi bir maliyet analizi yapılamaz. Doğru boyutlandırma, en iyi performansa de olanak tanır. 


## <a name="most-common-deployment-scenario"></a>En yaygın dağıtım senaryosu

 Birçok Microsoft müşterisi, tam dağıtımdan veya en azından SACA ortamlarının planlama aşamalarından çıktı. Deneyimleri, en yaygın dağıtım senaryosuna ilişkin Öngörüler ortaya çıkarır. Aşağıdaki diyagramda en yaygın mimari gösterilmektedir: 


![SACA başvuru mimarisi diyagramı](media/sacav2commonscenario.png) 


Diyagramdan görebileceğiniz gibi DoD müşterileri genellikle DıŞA BCAPs 'in iki birine abone olur. Bunlardan biri Batı Yakası ve diğer yaşamda Doğu Yakası üzerinde bulunur. Her bir DıŞA bir ExpressRoute özel eşi, her bir DıŞA BCAP konumunda Azure için etkinleştirilmiştir. Bu ExpressRoute eşleri daha sonra DoD Doğu ve DoD merkezi Azure bölgelerindeki sanal ağ geçidine bağlanır. Bir SACA örneği DoD Doğu ve DoD merkezi Azure bölgelerinde dağıtılır. Tüm giriş ve çıkış trafiği, üzerinden ve ExpressRoute bağlantısı üzerinden DıŞA giden trafik üzerinden akar.

Görev sahibi uygulamalar daha sonra uygulamalarını dağıtmayı planlayabilecekleri Azure bölgelerini seçer. Bu kişiler, uygulamasının sanal ağını SACA sanal ağına bağlamak için sanal ağ eşlemesi kullanır. Ardından, VDSS örneği aracılığıyla tüm trafiğini tünele zorlayacaktır.

SCCA gereksinimlerini karşıladığından bu mimariyi öneririz. Yüksek oranda kullanılabilir ve kolayca ölçeklenebilir. Ayrıca dağıtım ve yönetimi basitleştirir.

## <a name="automated-saca-deployment-options"></a>Otomatik SACA dağıtım seçenekleri

 Daha önce belirtildiği gibi, Microsoft otomatik bir SACA altyapı şablonu oluşturmak için iki satıcı ile işbirliği yaptı. Her iki şablon da aşağıdaki Azure bileşenlerini dağıtır: 

- SACA sanal ağı
    - Yönetim alt ağı
        - Bu alt ağ, bir geçiş kutuları olarak da bilinen yönetim VM 'lerinin ve hizmetlerinin dağıtıldığı yerdir.
        - VDMS alt ağı
            - Bu alt ağ, VDMS için kullanılan VM 'Lerin ve hizmetlerin dağıtıldığı yerdir.
        - Güvenilmeyen ve güvenilen alt ağlar
            - Bu alt ağlar, sanal gereçlerden dağıtılmakta olduğu yerdir.
        - Ağ geçidi alt ağı
            - Bu alt ağ, ExpressRoute ağ geçidinin dağıtıldığı yerdir.
- Yönetim hızlı geçiş kutusu sanal makineleri
    - Ortamın bant dışı yönetimi için kullanılırlar.
- Ağ sanal cihazları
    - Dağıttığınız şablona göre Citrix veya F5 kullanın.
- Ortak IP'ler
    - ExpressRoute çevrimiçi duruma gelene kadar ön uç için kullanılırlar. Bu IP 'Ler, arka uç Azure özel adres alanına çeviri yapar.
- Rota tabloları 
    - Otomasyon sırasında uygulanan bu yol tabloları, Sanal Gereç aracılığıyla tüm trafiği tünele zorlar.
- Azure yük dengeleyiciler-standart SKU
    - Bunlar, gereçler genelinde trafiğin yükünü dengelemek için kullanılırlar.
- Ağ güvenlik grupları
    - Belirli uç noktalarda hangi trafik türlerinin geçebileceği denetlemek için kullanılırlar.


### <a name="citrix-saca-deployment"></a>Citrix SACA dağıtımı

Citrix dağıtım şablonu, yüksek oranda kullanılabilir Citrix ADC gereçlerinin iki katmanını dağıtır. Bu mimari VDSS gereksinimlerini karşılar. 

![Citrix SACA diyagramı](media/citrixsaca.png)


Citrix belgeleri ve dağıtım betiği için [Bu GitHub bağlantısına](https://github.com/citrix/netscaler-azure-templates/tree/master/templates/saca)bakın.


 ### <a name="f5-saca-deployment"></a>F5 SACA dağıtımı

İki ayrı F5 dağıtım şablonu iki farklı mimariyi kapsar. İlk şablonda etkin-etkin yüksek kullanılabilirliğe sahip bir yapılandırmada yalnızca bir F5 gereçi katmanı vardır. Bu mimari VDSS gereksinimlerini karşılar. İkinci şablon, etkin-etkin yüksek oranda kullanılabilir F5s ikinci katmanını ekler. Bu ikinci katman, müşterilerin F5 katmanları arasındaki F5 'ten ayrı IP 'leri eklemesini sağlar. Tüm DoD bileşenlerinde kullanılmak üzere belirlenmiş belirli IP 'ler yoktur. Böyle bir durum söz konusu olduğunda, Bu mimaride F5 cihazlarındaki IP 'ler dahil olmak üzere F5 gereçlerinin tek katmanı çoğu için geçerlidir.

![F5 SACA diyagramı](media/f5saca.png)

F5 belgeleri ve dağıtım betiği için [Bu GitHub bağlantısına](https://github.com/f5devcentral/f5-azure-saca)bakın.












