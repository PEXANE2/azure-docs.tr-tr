---
title: Azure Avustralya 'da çıkış trafiğini denetleme
description: Güvenli Internet ağ geçitleri için Avustralya kamu gereksinimlerini karşılamak üzere Azure 'da çıkış trafiği denetleme temel öğeleri
author: galey801
ms.service: azure-australia
ms.topic: conceptual
ms.date: 07/29/2019
ms.author: grgale
ms.openlocfilehash: 7922846d161b7a0cbda32101b6bbb40a1741b323
ms.sourcegitcommit: 15f7b641a67f3d6cf4fb4b4c11eaee18cf335923
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/29/2019
ms.locfileid: "68602094"
---
# <a name="controlling-egress-traffic-in-azure-australia"></a>Azure Avustralya 'da çıkış trafiğini denetleme

ICT sistemlerini güvenli hale getirmenin temel bir bileşeni, ağ trafiğini denetlemekdir. İletişimin yalnızca bir sistemin çalışması için gereken trafikle sınırlandırılması, tehlikeye aşılma olasılığını azaltır. Uygulamalarınızın ve hizmetlerinizin iletişim kuran dış sistemler üzerinde görünürlük ve denetim, güvenliği aşılmış sistemleri algılamaya ve veri alımı denendiğinde ya da başarılı olmaya yardımcı olur. Bu makale, giden (çıkış) ağ trafiğinin Azure 'da nasıl çalıştığı hakkında bilgi sağlar ve Avustralya Cyber Güvenlik Merkezi (ACSC) ile hizalanan internet 'e bağlı bir sistem için ağ güvenliği denetimleri uygulamaya yönelik öneriler sağlar Tüketici Kılavuzu ve ACSC 'nin bilgi güvenliği El Ile (ıSM) amacı.

## <a name="requirements"></a>Gereksinimler

Uluslar için genel güvenlik gereksinimleri, ıSM içinde tanımlanmıştır. Ağ güvenliği uygulama kapsamındaki varlıkların yardımcı olması için ACSC, _ACSC korumasını yayımladı: Ağ kesimlemesi ve ayrımı uygulama ve_bulut ortamlarında sistemlerin güvenliğini sağlamaya yardımcı olmak ve ACSC, _kiracılar için bulut bilgi işlem güvenliği_yayımlaması.

ACSC belgeleri ağ güvenliği uygulama ve trafiği denetleme bağlamını özetler ve ağ tasarımı ve yapılandırması için pratik öneriler sağlar.

Azure 'da çıkış trafiğini denetlemek için aşağıdaki temel gereksinimler ACSC belgelerinde tanımlanmıştır.

Açıklama|Source
--------------- |------------------
**Ağ kesimlemesini ve ayırmayı uygulayın**, örneğin, ana bilgisayar tabanlı güvenlik duvarları ve ağ erişim denetimleri kullanarak, gelen ve giden ağ bağlantısını yalnızca gerekli bağlantı noktalarıyla ve protokollerle sınırlamak için bir n katmanlı mimari kullanın.| [Kiracılar için bulut bilgi Işlem](https://acsc.gov.au/publications/protect/cloud-security-tenants.htm)
Kiracının kullanılabilirlik gereksinimlerini karşılamak için kiracı (kiracının uzak kullanıcıları dahil) ve bulut hizmeti arasında **yeterli yüksek bant genişliği, düşük gecikme süresi ve güvenilir ağ bağlantısı uygulayın**  | [ACSC koruma: Ağ kesimlemesini ve ayırmayı uygulama](https://acsc.gov.au/publications/protect/network_segmentation_segregation.htm)
**Teknolojileri yalnızca ağ katmanından daha fazlasına uygulayın**. Her ana bilgisayar ve ağ, mümkün olduğunda, mümkün olduğunda, mümkün olduğunca, en düşük düzeyde yönetilebilecek şekilde bölünmeli ve ayrılmış olmalıdır. Çoğu durumda bu, veri bağlantısı katmanından uygulama katmanını içeren ve dahil olmak üzere geçerlidir; Ancak, hassas ortamlarda fiziksel yalıtım uygun olabilir. Ana bilgisayar tabanlı ve ağ genelinde ölçümler, tamamlayıcı bir biçimde dağıtılmalıdır ve merkezi olarak izlenir. Yalnızca güvenlik ölçüsü yeterli olmadığı için bir güvenlik duvarı veya güvenlik gereci uygulamak yeterlidir. |[ACSC koruma: Ağ kesimlemesini ve ayırmayı uygulama](https://acsc.gov.au/publications/protect/network_segmentation_segregation.htm)
**En az ayrıcalık Ilkelerini kullanın ve verilere erişme to verilere erişme bildirmek için gerekir**. Bir konağın, hizmetin veya ağın başka bir konak, hizmet veya ağla iletişim kurması gerekmiyorsa, buna izin verilmemelidir. Bir ana bilgisayar, hizmet veya ağın belirli bir bağlantı noktası veya protokolde yalnızca başka bir ana bilgisayar, hizmet veya ağ ile iletişim kurmasına ihtiyacı varsa, bu bağlantı noktalarıyla ve protokollerle sınırlandırılmalıdır. Bu ilkeleri bir ağ genelinde benimseme, kullanıcı ayrıcalıklarının en düşük düzeyde kullanımını tamamlar ve ortamın genel güvenlik duruşunu önemli ölçüde artırır. |[ACSC koruma: Ağ kesimlemesini ve ayırmayı uygulama](https://acsc.gov.au/publications/protect/network_segmentation_segregation.htm)
**Ana bilgisayarları ve ağları, iş işlemlerine duyarlılık veya önem derecesine göre ayırın**. Bu, belirli konaklar veya ağlar için farklı güvenlik sınıflandırmalarına, güvenlik etki alanlarına veya kullanılabilirlik/bütünlük gereksinimlerine bağlı olarak farklı donanım veya platformları kullanmayı içerebilir. Özellikle, ayrı yönetim ağları ve hassas ortamlar için bant dışı yönetim ağlarını fiziksel olarak yalıtmayı düşünün. |[ACSC koruma: Ağ kesimlemesini ve ayırmayı uygulama](https://acsc.gov.au/publications/protect/network_segmentation_segregation.htm)
**Tüm varlıklar tarafından tüm diğer varlıklara göre erişimi belirler, kimlik doğrular ve yetkilendirin**. Tüm kullanıcılar, konaklar ve hizmetler, yalnızca belirlenen görevlerini veya işlevlerini gerçekleştirmek için gerekenlerle sınırlı olan diğer tüm kullanıcılara, ana bilgisayarlara ve hizmetlere erişimleri olmalıdır. Kimlik, kimlik doğrulama ve yetkilendirme hizmetlerinin gücünü atlayan veya indirgeen eski veya yerel hizmetler, mümkün olan yerlerde devre dışı bırakılmalıdır ve kullanımları yakından izlenir. |[ACSC koruma: Ağ kesimlemesini ve ayırmayı uygulama](https://acsc.gov.au/publications/protect/network_segmentation_segregation.htm)
**Engelleme listesi yerine ağ trafiği listesine izin ver**' i uygulayın. Bilinen hatalı ağ trafiğine erişimi reddetmek (örneğin, belirli bir adresi veya hizmeti engellemek) yerine yalnızca bilinen iyi ağ trafiğine (tanımlı, kimlik doğrulamalı ve yetkili trafik) erişime izin verir. Listelere izin ver, üstün bir güvenlik ilkesinin listeleri reddetmesine ve olası ağ erişimlerini tespit etmek ve değerlendirmek için kapasiteyi önemli ölçüde iyileştirmesine neden olacak. |[ACSC koruma: Ağ kesimlemesini ve ayırmayı uygulama](https://acsc.gov.au/publications/protect/network_segmentation_segregation.htm)
**İzin verilen Web siteleri listesini tanımlama ve tüm listelenmemiş Web sitelerini engelleme** , bir saldırgan tarafından kullanılan en yaygın veri teslimi ve kfiltration tekniklerini etkin bir şekilde kaldırır. Kullanıcıların birçok Web sitesine erişmek için meşru bir ihtiyacı varsa veya hızlı değişen bir Web sitesi listesi; Bu tür bir uygulamanın maliyetlerini göz önünde bulundurmanız gerekir. Görece izin verilenler listesi, reddetme listelerine bağlı olandan daha iyi bir güvenlik sağlar veya hiç kısıtlama gerektirmez, ancak yine de uygulama maliyetlerini azaltmaktadır. İzin verilenler listesine bir örnek, ' *. au ' olan tüm Avustralya alt etki alanı için izin verebilir veya Alexa Site derecelendirmesine (dinamik etki alanı adı sistemi (DDNS) etki alanlarını ve diğer uygun olmayan etki alanlarına filtre uyguladıktan sonra) ilk 1.000 siteye izin verebilir.| [Avustralya devlet bilgi güvenliği el kitabı (ıSM)](https://www.cyber.gov.au/ism)
|

Bu makalede, Azure ortamınızı bırakma hakkında bilgi ve öneriler sağlanır. Hizmet olarak altyapı (IaaS) ve hizmet olarak platform (PaaS) kullanılarak Azure 'da dağıtılan sistemleri ele alır.

Ağ [geçidi giriş trafiği](gateway-ingress-traffic.md) makalesi, Azure ortamınızı girerek ağ trafiğine yöneliktir ve tam ağ denetimi kapsamı için bu makaleye yardımcı olur.

## <a name="architecture"></a>Mimari

Çıkış trafiğini uygun şekilde denetlemek için, ağ güvenliğini tasarlarken ve uygularken, önce çıkış ağı trafiğinin Azure 'da IaaS ve PaaS genelinde nasıl çalıştığını anlamanız gerekir. Bu bölümde, Azure 'da barındırılan bir kaynak tarafından oluşturulan giden trafiğin nasıl işlendiği ve bu trafiği kısıtlamak ve denetlemek için kullanılabilen güvenlik denetimleri hakkında genel bir bakış sağlanmaktadır.

### <a name="architecture-components"></a>Mimari bileşenleri

Burada gösterilen mimari diyagram, sanal bir ağdaki bir alt ağa dağıtılan bir sistemden çıkarken ağ trafiğinin ulaşabileceği olası yolları gösterir. Bir sanal ağdaki trafik, içinde yer alan kaynaklara uygulanan yönlendirme ve güvenlik kuralları ile bir alt ağ düzeyinde yönetilir ve yönetilir. Çıkış trafiğiyle ilgili bileşenler sistemlere, etkin yollara, sonraki atlama türlerine, güvenlik denetimlerine ve PaaS çıkış bölümüne bölünmüştür.

![Mimari](media/egress-traffic.png)

### <a name="systems"></a>Sistemlerinden

Sistemler, bir sanal ağın parçası olan bir IP alt ağı içinde giden trafik oluşturan Azure kaynaklarıdır ve ilgili bileşenleridir.

| Bileşen | Açıklama |
| --- | ---|
|Sanal Ağ (VNet) | VNet, Azure 'da kaynak dağıtmaya ve iletişimi etkinleştirmeye yönelik bir platform ve sınır sağlayan temel bir kaynaktır. VNet bir Azure bölgesi içinde bulunur ve sanal makineler gibi VNet tümleşik kaynakları için IP adresi alanını ve yönlendirme sınırlarını tanımlar.|
|Subnet | Bir alt ağ, VNet içinde oluşturulan bir IP adresi aralığıdır. Ağ kesimlemesi için bir VNet içinde birden çok alt ağ oluşturulabilir.|
|Ağ Arabirimi| Ağ arabirimi, Azure 'da bulunan bir kaynaktır. Bir sanal makineye iliştirilir ve ilişkilendirildiği alt ağdan özel, Internet 'e ait olmayan bir IP adresi atanır. Bu IP adresi, Azure Resource Manager aracılığıyla dinamik veya statik olarak atanır.|
|Ortak IP'ler| Genel IP, Microsoft 'un sahip olduğu genel, Internet yönlendirilebilir IP adreslerinden sanal ağ içinde kullanılmak üzere belirli bir bölgeden ayrılmış bir kaynaktır. Bu, kaynağın Internet, ExpressRoute ve diğer PaaS sistemlerine iletişim kurmasını sağlayan belirli bir ağ arabirimi veya PaaS kaynağı ile ilişkilendirilebilir.|
|

### <a name="routes"></a>Rotalar

Çıkış trafiğinden elde edilen yol, bu kaynak için geçerli yollara bağımlıdır. Bu, tüm olası kaynaklardan ve Azure yönlendirme mantığının uygulamasına öğreni yolların birleşimine göre belirlenen yolların sonuç kümesidir.

| Bileşen | Açıklama |
|--- | ---|
|Sistem Yolları| Azure, sistem yollarını otomatik olarak oluşturur ve bir sanal ağ içindeki her alt ağa bu yolları atar. Sistem yolları oluşturulamaz veya kaldırılamaz, ancak bazıları özel yollarla geçersiz kılınabilir. Azure, her alt ağ için varsayılan sistem yollarını oluşturur ve belirli Azure özellikleri kullanılırken belirli alt ağlara veya her alt ağa isteğe bağlı varsayılan yollar ekler.|
|Hizmet Uç Noktaları| Hizmet uç noktaları, bir alt ağdan belirli bir PaaS özelliğine doğrudan, özel çıkış bağlantısı sağlar. PaaS yeteneklerinin yalnızca bir alt kümesi için kullanılabilen hizmet uç noktaları, PaaS 'ye erişen VNet 'teki kaynaklar için daha yüksek performans ve güvenlik sağlar.|
|Yönlendirme Tabloları| Yol tablosu, Azure 'daki bir kaynaktır ve Sınır Ağ Geçidi Protokolü aracılığıyla sistem yollarının veya geçersiz kılabileceği ya da öğrenmekte olan yollar için Kullanıcı tanımlı yollar (UDRs) belirtmek üzere oluşturulabilir. Her UDR bir ağ, bir ağ maskesi ve bir sonraki atlama belirler. Bir rota tablosu bir alt ağla ilişkilendirilebilir ve aynı rota tablosu birden fazla alt ağ ile ilişkilendirilebilir, ancak bir alt ağda yalnızca sıfır veya bir rota tablosu olabilir.|
|Sınır Ağ Geçidi Protokolü (BGP)| BGP, bir Inter-özerk sistem yönlendirme protokolüdür. Özerk sistem, ortak bir yönetim ve ortak yönlendirme ilkeleriyle bir ağ veya ağ grubudur. BGP, özerk sistemler arasında yönlendirme bilgilerini değiş tokuş etmek için kullanılır. BGP, sanal ağ geçitleri üzerinden sanal ağlarla tümleştirilebilir.|
|

### <a name="next-hop-types-defined"></a>Tanımlanan sonraki atlama türleri

Azure içindeki her yol ağ aralığı ve ilişkili alt ağ maskesini ve sonraki atlamayı içerir ve bu da trafiğin nasıl işleneceğini belirler.

Sonraki Atlama Türü | Açıklama
---- | ----
**Sanal Ağ** | Bir sanal ağın adres alanı içindeki adres aralıkları arasında trafiği yönlendirir. Azure, bir sanal ağın adres alanı içinde tanımlanmış her bir adres aralığına karşılık gelen bir adres ön eki ile yol oluşturur. Sanal ağ adres alanında tanımlanmış birden fazla adres aralığı varsa, Azure her adres aralığı için tek bir yol oluşturur. Azure, her bir adres aralığı için oluşturulan yolları kullanarak bir sanal ağ içindeki alt ağlar arasındaki trafiği otomatik olarak yönlendirir.
**Sanal ağ eşlemesi** | İki sanal ağ arasında bir sanal ağ eşlemesi oluşturulduğunda, her bir sanal ağın her bir adres aralığı için eşlenmekte olduğu sanal ağa bir yol eklenir. Trafik, eşlenen sanal ağlar arasında bir sanal ağ içindeki alt ağlarla aynı şekilde yönlendirilir.
**Sanal ağ geçidi** | Bir sanal ağa bir sanal ağ geçidi eklendiğinde, sonraki atlama türü olarak listelenen sanal ağ geçidi ile bir veya daha fazla yol eklenir. Dahil edilen yollar, yerel ağ geçidi kaynağı içinde ve BGP aracılığıyla öğrenilenler aracılığıyla yapılandırılmış olan yollardır.
**Sanal Gereç** | Sanal Gereç genellikle güvenlik duvarı gibi bir ağ uygulaması çalıştırır. Sanal Gereç, filtreleme, denetleme veya adres çevirisi gibi trafiğin oluşmasına yönelik ek işlem yapılmasına izin verir. Sanal Gereç atlama türüne sahip her bir yol, sonraki atlama IP adresini de belirtmelidir.
**VirtualNetworkServiceEndpoint** | Belirli bir hizmetin genel IP adresleri, bir hizmet uç noktası etkinleştirildiğinde VirtualNetworkServiceEndpoint 'in bir sonraki atağıyla bir alt ağa yollar olarak eklenir. Hizmet uç noktaları, bir sanal ağ içindeki tek tek alt ağlardaki bireysel hizmetler için etkinleştirilir. Azure hizmetlerinin genel IP adresleri düzenli olarak değişir. Adresler değiştiğinde Azure, yol tablosundaki adresleri otomatik olarak yönetir.
**Internet** | Internet 'in bir sonraki atlaması olan trafik sanal ağdan çıkar ve ilişkili Azure bölgesinde bulunan bir dinamik havuzdan ya da bu kaynak için yapılandırılmış bir genel IP adresi kullanılarak otomatik olarak genel bir IP adresine çevrilir. Hedef adres Azure hizmetlerinden birine ait ise, trafiği Internet 'e yönlendirmek yerine doğrudan Azure omurga ağı üzerinden hizmete yönlendirilir. Sanal ağın içinde bulunduğu Azure bölgesi veya Azure hizmeti örneğinin dağıtıldığı Azure bölgesi ne olursa olsun, Azure hizmetleri arasındaki trafik İnternet'i dolaşmaz.
**Yok.** | Bir sonraki atlama atlaması olan trafik bırakılır. Azure, sonraki atlama türü olarak None olan ayrılmış adres önekleri için sistem varsayılan yolları oluşturur. Bir sonraki atlama atlaması olan yollar, trafiğin belirli ağlara yönlendirilmesini engellemek için yol tabloları kullanılarak da eklenebilir.
|

### <a name="security-controls"></a>Güvenlik denetimleri

Denetim | Açıklama
----- | -----
**Ağ güvenlik grupları (NSG 'ler)** | NSG 'ler, Azure 'daki sanal ağ kaynaklarının içine ve dışına trafiği denetler. NSG 'ler, Azure ve şirket içi ya da Internet gibi dış ağlar arasındaki trafiği de içeren, izin verilen veya reddedilen trafik akışları için kurallar uygular. NSG 'ler bir sanal ağ içindeki alt ağlara veya tek tek ağ arabirimlerine uygulanır.
**Azure Güvenlik Duvarı** | Azure Güvenlik Duvarı, Azure sanal ağ kaynaklarını koruyan, yönetilen ve bulut tabanlı bir ağ güvenlik hizmetidir. Yerleşik yüksek kullanılabilirlik oranı ve kısıtlamasız bulut ölçeklenebilirliğiyle hizmet olarak tam durum bilgisi olan bir güvenlik duvarıdır. Azure Güvenlik Duvarı, IP adresleri, protokoller ve bağlantı noktaları temel alınarak geleneksel ağ filtreleme kuralları ile yapılandırılabilir, ancak aynı zamanda tam etki alanı adlarına (FQDN), hizmet etiketlerine ve yerleşik tehdit zekasına göre filtrelemeyi destekler.
**Ağ sanal gereci (NVA)** | Ağ sanal gereçleri, Azure 'da ağ, güvenlik ve diğer işlevler sağlayabilen sanal makine medyalardır. NVA 'lar sanal ağlardaki ve dağıtımlardaki VM 'Ler biçimindeki ağ işlevselliğini ve hizmetlerini destekler. NVA 'lar belirli gereksinimleri karşılamak, yönetim ve işletimsel araçlarla tümleştirilen ya da mevcut ürünlerle tutarlılık sağlamak için kullanılabilir. Azure, web uygulaması güvenlik duvarları (WAF), güvenlik duvarları, ağ geçitleri/yönlendiriciler, uygulama teslimi denetleyicileri (ADC) ve WAN iyileştiricileri gibi çeşitli üçüncü taraf ağ sanal gereçlerini destekler.
**Hizmet uç noktası ilkeleri (Önizleme)** | Sanal ağ hizmeti uç noktası ilkeleri, Azure hizmetlerine sanal ağ trafiğini filtrelemenizi sağlar ve hizmet uç noktaları üzerinden yalnızca belirli Azure hizmet kaynaklarına izin verir. Uç nokta ilkeleri Azure hizmetlerine yönelik sanal ağ trafiği için ayrıntılı erişim denetimi sağlar.
**Azure İlkesi** | Azure Ilkesi, ilkeleri oluşturmak, atamak ve yönetmek için Azure 'da bir hizmettir. Bu ilkeler, dağıtılabilecek kaynak türlerini ve bu kaynakların yapılandırmasını denetlemek için kuralları kullanır. İlkeler, kaynakların gereksinimleri karşılamadığında veya uyumluluk durumunu raporlamak üzere izlenmesi için kullanılabilecekleri şekilde dağıtılmasını önleyecek şekilde uyumluluk sağlamak için kullanılabilir.
|

### <a name="paas-egress"></a>PaaS çıkışı

PaaS kaynaklarının çoğunluğu çıkış trafiği oluşturmaz, ancak gelen isteklere (bir Application Gateway, depolama, SQL veritabanı, vb.) veya diğer kaynaklardan (Service Bus ve Azure Relay) geçiş verilerine yanıt verir. Depolama veya SQL veritabanlarına uygulama hizmetleri gibi PaaS kaynakları arasındaki ağ iletişim akışları, Azure tarafından denetlenir ve Azure tarafından içerilir ve ağ kesimlemesi veya ayrımı yerine kimlik ve diğer kaynak yapılandırma denetimleriyle güvenli hale getirilir.

Sanal bir ağa dağıtılan PaaS kaynakları ayrılmış IP adresleri alır ve sanal ağdaki diğer kaynaklarla aynı şekilde herhangi bir yönlendirme denetimine ve NSG 'ye tabidir. Bir sanal ağ içinde mevcut olmayan PaaS kaynakları, Microsoft belgeleri aracılığıyla yayınlanan veya Azure Resource Manager aracılığıyla belirlenebilir olan kaynağın tüm örneklerinde paylaşılan bir IP adresi havuzu kullanacaktır.

## <a name="general-guidance"></a>Genel rehberlik

Azure 'da güvenli çözümler tasarlamak ve derlemek için, ağ trafiğini yalnızca tanımlı ve yetkili iletişimin gerçekleşebilmesi için anlamak ve denetlemek önemlidir. Bu kılavuzun amacı ve sonraki bölümlerde belirtilen bileşen Kılavuzu, [ACSC korumasında özetlenen ilkeleri uygulamak için kullanılabilecek araçları ve Hizmetleri anlatmaktadır: Azure iş yükleri](https://acsc.gov.au/publications/protect/network_segmentation_segregation.htm) arasında ağ kesimlemesini ve ayırmayı uygulama. Bu, bir şirket içi ortamda mümkün olan geleneksel fiziksel ve ağ denetimlerinin uygulanması mümkün olmadığında kaynakları güvenli hale getirmek için bir sanal mimari oluşturma hakkında ayrıntılı bilgiler içerir.

### <a name="guidance"></a>Rehber

* Sanal ağların çıkış noktası sayısını sınırlayın
* Internet 'e doğrudan giden iletişim gerektirmeyen tüm alt ağlar için sistem varsayılan yolunu geçersiz kılın
* Tüm giriş ve çıkış noktalarını Azure kaynaklarına tanımlamak ve denetlemek için bir ağ mimarisi tasarlayın ve uygulayın
* Microsoft Virtual Data Center (VDC) belgelerinde anlatıldığı gibi sanal ağlar için bir hub ve bağlı bileşen ağ tasarımını kullanmayı düşünün
* Internet 'e giden bağlantılar için yerleşik güvenlik özelliklerine sahip ürünler (örneğin, Azure Güvenlik Duvarı, ağ sanal cihazları veya Web proxy 'Leri)
* Ağ yapılandırma ayrıcalıklarını sınırlamak için rol tabanlı erişim, koşullu erişim ve çok faktörlü kimlik doğrulaması (MFA) gibi kimlik denetimlerini kullanın
* Ağ yapılandırmasındaki anahtar öğelerinin değiştirilmesini veya silinmesini engellemek için kilitleri uygulayın
* Daha fazla ayrım ve denetim için VNet tümleşik yapılandırmasında PaaS dağıtma
* Şirket içi ağlarla bağlantı için ExpressRoute uygulama
* Dış ağlarla tümleştirme için VPN 'Leri uygulama
* Bölgeleri ve kaynakları yalnızca sistem işlevselliği için gerekli olanlarla kısıtlamak için Azure Ilkesi 'ni kullan
* Kaynaklar için temel güvenlik yapılandırmasını zorlamak üzere Azure Ilkesini kullan
* Azure 'da ağ trafiğinin günlüğe kaydedilmesi, denetlenmesi ve görünebilirliği için ağ Izleyicisi ve Azure Izleyici 'den yararlanın

### <a name="resources"></a>Kaynaklar

Öğe | Bağlantı
-----------| ---------
_Tüketici kılavuzu dahil olmak üzere Avustralya mevzuata ve Ilke uyumluluk belgeleri_ | [https://aka.ms/au-irap](https://aka.ms/au-irap)
_Azure sanal veri merkezi_ | [https://docs.microsoft.com/azure/architecture/vdc/networking-virtual-datacenter](https://docs.microsoft.com/azure/architecture/vdc/networking-virtual-datacenter)
_ACSC ağ kesimlemesi_ | [https://acsc.gov.au/publications/protect/network_segmentation_segregation.htm](https://acsc.gov.au/publications/protect/network_segmentation_segregation.htm)
_Kiracılar için ACSC Bulut güvenliği_ | [https://acsc.gov.au/publications/protect/cloud-security-tenants.htm](https://acsc.gov.au/publications/protect/cloud-security-tenants.htm)
_ACSC Information Security el kitabı_ | [https://acsc.gov.au/infosec/ism/index.htm](https://acsc.gov.au/infosec/ism/index.htm)
|

## <a name="component-guidance"></a>Bileşen Kılavuzu

Bu bölümde, Azure 'da dağıtılan sistemler için çıkış trafiğiyle ilgili olan tek tek bileşenler hakkında daha fazla rehberlik sunulmaktadır. Her bölümde, tasarım ve oluşturma etkinliklerine yardımcı olmak üzere kullanılabilecek belge ve yapılandırma kılavuzlarının bağlantıları ile ilgili bir bileşen amacı açıklanmaktadır.

### <a name="systems-security"></a>Sistem güvenliği

Azure 'daki kaynaklara yapılan tüm iletişimler, bağlantı ve güvenlik işlevselliği sağlayan Microsoft tarafından tutulan ağ altyapısı aracılığıyla geçer. Azure platformunu ve ağ altyapısını korumak için Microsoft tarafından otomatik olarak bir dizi koruma sağlanır ve ağ trafiğini denetlemek ve ağ kesimlenmesi ve ağ kesimlemesi oluşturmak için Azure 'da hizmetler olarak kullanılabilir ayrımı.

### <a name="virtual-network-vnet"></a>Sanal Ağ (VNet)

Sanal ağlar, Azure 'da ağ için temel yapı taşlarından biridir. Sanal ağlar, çeşitli sistemler genelinde kullanılacak bir IP adresi alanı ve yönlendirme sınırı tanımlar. Sanal ağlar alt ağlara bölünür ve bir sanal ağ içindeki tüm alt ağların birbirlerine doğrudan bir ağ rotası vardır. Sanal ağ geçitleri (ExpressRoute veya VPN) kullanarak, bir sanal ağ içindeki sistemler şirket içi ve dış ortamlarla ve Azure tarafından belirtilen ağ adresi çevirisi (NAT) ve genel IP adresi ayırma ile tümleştirilebilir, sistemler Internet 'e veya diğer Azure bölgelerine ve hizmetlerine bağlanın. Sanal ağları ve ilişkili yapılandırma parametrelerini ve yönlendirmeyi anlamak, çıkış ağı trafiğini anlamak ve denetlemek için önemlidir.

Sanal ağlar, Azure 'daki temel adres alanını ve yönlendirme sınırını oluştururken, ağ kesimlemesi ve ayrımı için birincil yapı bloğu olarak kullanılabilir.

| Resource | Bağlantı |
| --- | --- |
| *Sanal ağlara genel bakış* | [https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview) |
| *Sanal ağlar için nasıl yapılır Kılavuzu planlayın*  | [https://docs.microsoft.com/azure/virtual-network/virtual-network-vnet-plan-design-arm](https://docs.microsoft.com/azure/virtual-network/virtual-network-vnet-plan-design-arm) |
| *Sanal ağ hızlı başlangıcı oluşturma* | [https://docs.microsoft.com/azure/virtual-network/quick-create-portal](https://docs.microsoft.com/azure/virtual-network/quick-create-portal)
|

### <a name="subnet"></a>Subnet

Alt ağlar, Azure 'da ağ kesimlenmesi ve ayırma için önemli bir bileşendir. Alt ağlar, sistemler arasında ayrım sağlamak için kullanılabilir. Alt ağ, NSG 'ler, yol tabloları ve hizmet uç noktalarının uygulandığı bir sanal ağ içindeki kaynaktır. Alt ağlar, güvenlik duvarı kuralları ve erişim denetimi listeleri için hem kaynak hem de hedef adresler olarak kullanılabilir.

Bir sanal ağ içindeki alt ağlar, iş yüklerinin ve sistemlerin gereksinimlerini karşılayacak şekilde planlanmalıdır. Alt ağların tasarımına veya uygulamasına dahil olan bireyler, sistemlerin bir alt ağ içinde nasıl gruplanacağını belirlemede ağ kesimlemesi için ACSC yönergelerine başvurmalıdır.

|Resource|Bağlantı|
|---|---|
|*Sanal ağ alt ağını ekleme, değiştirme veya silme* | [https://docs.microsoft.com/azure/virtual-network/virtual-network-manage-subnet](https://docs.microsoft.com/azure/virtual-network/virtual-network-manage-subnet)
|

### <a name="network-interface"></a>Ağ arabirimi

Ağ arabirimleri, bir sanal makinedeki tüm çıkış trafiği kaynağıdır. Ağ arabirimleri IP adresleme yapılandırmasını etkinleştirir ve NSG 'ler veya bir NVA aracılığıyla trafiği yönlendirme için kullanılabilir. Sanal makineler için ağ arabirimleri, genel ağ kesimlemesi ve ayırma amaçları ile hizalamak için planlanmış ve uygun şekilde yapılandırılmalıdır.

|Resource|Bağlantı|
|---|---|
|*Ağ arabirimi oluşturma, değiştirme veya silme* | [https://docs.microsoft.com/azure/virtual-network/virtual-network-network-interface](https://docs.microsoft.com/azure/virtual-network/virtual-network-network-interface) |
|*Ağ arabirimi IP adresleme*               | [https://docs.microsoft.com/azure/virtual-network/virtual-network-ip-addresses-overview-arm#private-ip-addresses](https://docs.microsoft.com/azure/virtual-network/virtual-network-ip-addresses-overview-arm#private-ip-addresses)
|

### <a name="vnet-integrated-paas"></a>VNet tümleşik PaaS

PaaS, gelişmiş işlevsellik ve kullanılabilirlik sağlayabilir ve yönetim yükünü azaltabilir, ancak güvenli bir şekilde güvende olmalıdır. Denetimi artırmak, ağ kesimlemesini zorlamak veya uygulamalar ve hizmetler için güvenli bir çıkış noktası sağlamak için, birçok PaaS özelliği bir sanal ağ ile tümleştirilebilir.

Microsoft, System veya Application mimarisinin tümleşik bir parçası olarak PaaS 'yi kullanarak, PaaS 'yi bir sanal ağa dağıtmak için birden çok mekanizma sağlar. Dağıtım metodolojisi, iç sistemler ve uygulamalarla bağlantı sağlarken erişimi kısıtlamaya yardımcı olabilir. App Service ortamları, SQL tarafından yönetilen örnekleri ve daha fazlasını içeren örnekler.

PaaS 'yi yönlendirme ve NSG denetimlerinin uygulandığı bir sanal ağa dağıttığınızda, Microsoft hizmetlerinden yönetim erişimi ve bu yolla ilgili olarak, kaynağın belirli iletişim gereksinimlerini anlamak çok önemlidir. iletişim trafiği, bu hizmetlerden gelen isteklere yanıt verirken sürer.

| Resource  | Bağlantı  |
| --- | --- |
| *Azure hizmetleri için sanal ağ tümleştirmesi* | [https://docs.microsoft.com/azure/virtual-network/virtual-network-for-azure-services](https://docs.microsoft.com/azure/virtual-network/virtual-network-for-azure-services) |
| *Uygulamanızı bir Azure sanal ağı ile tümleştirme nasıl yapılır Kılavuzu* | [https://docs.microsoft.com/azure/app-service/web-sites-integrate-with-vnet](https://docs.microsoft.com/azure/app-service/web-sites-integrate-with-vnet)
|

### <a name="public-ip"></a>Genel IP

Genel IP adresleri, bir sanal ağ dışında iletişim kurulurken kullanılır. Bu, PaaS kaynaklarını ve bir sonraki Internet Atlayanlara herhangi bir yolu içerir. Kurumlar varlıkları, genel IP adreslerinin ayırmayı dikkatlice planlıyor ve yalnızca orijinal bir gereksinim olduğu kaynaklara atamalıdır. Genel bir tasarım uygulaması olarak, Azure Güvenlik Duvarı, VPN Gateway veya ağ sanal cihazları gibi sanal ağın denetimli çıkış noktalarına genel IP adresleri ayrılmalıdır.

|Resource|Bağlantı|
|---|---|
|*Genel IP Adresleri genel bakış*  | [https://docs.microsoft.com/azure/virtual-network/virtual-network-ip-addresses-overview-arm#public-ip-addresses](https://docs.microsoft.com/azure/virtual-network/virtual-network-ip-addresses-overview-arm#public-ip-addresses) |
|*Genel IP adresi oluşturma, değiştirme veya silme* | [https://docs.microsoft.com/azure/virtual-network/virtual-network-public-ip-address](https://docs.microsoft.com/azure/virtual-network/virtual-network-public-ip-address)
|

## <a name="effective-routes"></a>Geçerli rotalar

Etkin yollar, sistem yollarının, hizmet uç noktalarının, yol tablolarının ve BGP 'nin yanı sıra Azure yönlendirme mantığı ile belirlenen yolların sonuç kümesidir. Giden trafik bir alt ağ üzerinden gönderildiğinde, Azure en uzun ön ek eşleştirme algoritmasını kullanarak hedef IP adresine göre bir yol seçer. Birden fazla yol aynı adres ön ekini içeriyorsa, Azure aşağıdaki öncelik sırasına göre yol türünü seçer:

1. Kullanıcı tanımlı yol
2. BGP yolu
3. Sistem yolu

BGP rotaları daha belirli olsa bile, sanal ağ, sanal ağ eşlemesi veya sanal ağ hizmet uç noktaları ile ilgili trafik için sistem yollarının tercih edilen yollar olduğunu unutmayın.

Azure 'da yönlendirme topolojilerinin tasarımına veya uygulamasına dahil olan bireyler, Azure 'un trafiği nasıl yönlendirdiğini ve gerekli güvenlik ve görünürlük ile sistemlerin gerekli işlevlerini dengeleyen bir mimari geliştirme şeklini anlamalıdır. Bu karmaşıklığın artması ve sorun giderme ve hata bulma konusunda daha zor ve zaman bulma işlemleri yapabileceğinden, ortamı, yetki kaldırıldığında davranışlar yönlendirme için gereken aşırı müdahale ve özel durumları önlemek için uygun şekilde planlamanız gerekir.

| Resource | Bağlantı  |
| --- | --- |
| *Geçerli yolları görüntüle* | <https://docs.microsoft.com/azure/virtual-network/manage-route-table#view-effective-routes> |
|

### <a name="system-routes"></a>Sistem yolları

[Sistem yolları](https://docs.microsoft.com/azure/virtual-network/virtual-networks-udr-overview#system-routes)için, sanal ağların tasarımına veya uygulamasına dahil olan bireyler, varsayılan sistem yollarını ve bu yolları tamamlamak ya da geçersiz kılmak için kullanılabilen seçenekleri anlamalıdır.

### <a name="service-endpoints"></a>Hizmet uç noktaları

Bir alt ağda [hizmet uç noktalarının](https://docs.microsoft.com/azure/virtual-network/virtual-network-service-endpoints-overview) etkinleştirilmesi, Ilişkili PaaS kaynağına doğrudan bir iletişim yolu sağlar. Bu, kullanılabilen iletişim yolunu yalnızca o hizmetle kısıtlayarak daha fazla performans ve güvenlik sağlayabilir. Hizmet uç noktalarının kullanımı, varsayılan yapılandırma bir uygulama veya sistem için gerekli olan belirli örnekler yerine PaaS hizmetinin tüm örneklerine erişime izin verdiği için olası bir veri değişim yolunu ortaya çıkarabilir.

Bu varlıkların, yanlış kullanılan yolun olasılığı ve sonucu dahil olmak üzere PaaS kaynağına doğrudan erişim sağlamaya ilişkin riski değerlendirmelidir.

Hizmet uç noktaları ile ilişkili olası riskleri azaltmak için, mümkün olduğunda hizmet uç noktası ilkeleri uygulayın veya bir Azure Güvenlik duvarı ya da NVA alt ağında hizmet uç noktalarını etkinleştirmeyi ve ek olarak belirli alt ağlardan gelen trafiği yönlendirmeyi deneyin filtreleme, izleme veya İnceleme uygulanabilir.

|Resource|Bağlantı|
|---|---|
|*Öğretici: Azure portal kullanarak sanal ağ hizmet uç noktaları ile PaaS kaynaklarına ağ erişimini kısıtlama* |[https://docs.microsoft.com/azure/virtual-network/tutorial-restrict-network-access-to-resources](https://docs.microsoft.com/azure/virtual-network/tutorial-restrict-network-access-to-resources)|
|

### <a name="route-tables"></a>Rota tabloları

Rota tabloları, Azure 'da ağ trafiğini denetlemek için yönetici tarafından yapılandırılan bir mekanizma sağlar. Yönlendirme tabloları, trafiği bir Azure Güvenlik Duvarı veya NVA 'ya iletmek, doğrudan dış kaynaklara bağlanmak veya Azure sistem yollarını geçersiz kılmak için kullanılabilir. Yönlendirme tabloları, bir sanal ağ geçidi üzerinden bir sanal ağ geçidi yol yaymayı devre dışı bırakarak bir alt ağdaki kaynaklar için kullanılabilir hale getirilmesini engellemek için de kullanılabilir.

|Resource|Bağlantı|
|---|---|
|*Yönlendirme kavramları-özel yollar* |[https://docs.microsoft.com/azure/virtual-network/virtual-networks-udr-overview#custom-routes](https://docs.microsoft.com/azure/virtual-network/virtual-networks-udr-overview#custom-routes)|
|*Öğretici: Ağ trafiğini yönlendirme* |[https://docs.microsoft.com/azure/virtual-network/tutorial-create-route-table-portal](https://docs.microsoft.com/azure/virtual-network/tutorial-create-route-table-portal)|
|

### <a name="border-gateway-protocol-bgp"></a>Sınır Ağ Geçidi Protokolü (BGP)

BGP, şirket içi veya diğer dış ağlarla yönlendirme bilgilerini dinamik olarak alışverişi için sanal ağ geçitleri tarafından kullanılabilir. ExpressRoute özel eşlemesi üzerinden bir ExpressRoute sanal ağ geçidi üzerinden yapılandırıldığında ve Azure VPN Gateway etkinleştirildiğinde BGP sanal ağ için geçerlidir.

Azure 'daki sanal ağların ve sanal ağ geçitlerinin tasarımına veya uygulamasına dahil olan bireyler, Azure 'da BGP için kullanılabilen davranış ve yapılandırma seçeneklerini anlamak için zaman almalıdır.

|Resource|Bağlantı|
|---|---|
|*Yönlendirme Kavramları: B* | [https://docs.microsoft.com/azure/virtual-network/virtual-networks-udr-overview#next-hop-types-across-azure-tools](https://docs.microsoft.com/azure/virtual-network/virtual-networks-udr-overview#next-hop-types-across-azure-tools)|
|*ExpressRoute yönlendirme gereksinimleri* | [https://docs.microsoft.com/azure/expressroute/expressroute-routing](https://docs.microsoft.com/azure/expressroute/expressroute-routing)|
|*Azure VPN Gateway ile BGP hakkında* |[https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-bgp-overview](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-bgp-overview)|
|*Öğretici: ExpressRoute Microsoft eşlemesi üzerinden siteden siteye VPN yapılandırma* |[https://docs.microsoft.com/azure/expressroute/site-to-site-vpn-over-microsoft-peering](https://docs.microsoft.com/azure/expressroute/site-to-site-vpn-over-microsoft-peering)|
|

## <a name="next-hop-types"></a>Sonraki atlama türleri

### <a name="virtual-network"></a>Sanal Ağ

Sanal ağın bir sonraki atmasına sahip rotalar otomatik olarak sistem yolları olarak eklenir, ancak aynı zamanda, trafiği sistem yolunun geçersiz kılındığı örneklerdeki sanal ağa yönlendirmek için Kullanıcı tanımlı yollara da eklenebilir.

### <a name="vnet-peering"></a>VNet eşlemesi

VNet eşlemesi iki farklı sanal ağ arasında iletişime izin verebilir. Her bir sanal ağda VNet eşlemesinin yapılandırılması gerekir, ancak sanal ağların aynı bölgede, abonelikte veya aynı Azure Active Directory (Azure AD) kiracısıyla ilişkilendirilmiş olması gerekmez.

VNet eşlemesini yapılandırırken, VNet eşlemesinin tasarımına veya uygulamasına dahil olan kişilerin, ilişkili dört yapılandırma parametresini ve bu koşulların her bir tarafına nasıl uygulandığını anlamak önemlidir:

1. **Sanal ağ erişimine izin ver:** İki sanal ağ arasında iletişimi etkinleştirmek için **etkin** (varsayılan) seçeneğini belirleyin. Sanal ağlar arasındaki iletişimin etkinleştirilmesi, sanal ağa bağlı kaynakların aynı bant genişliği ve aynı sanal ağa bağlıymış gibi gecikme süresiyle birbirleriyle iletişim kurmasına olanak tanır.
2. **İletilen trafiğe izin ver:** Bu kutuyu, sanal ağdan kaynaklanmayan bir ağ trafiği tarafından *iletilen* trafiğe izin vermek için, eşleme yoluyla bu sanal ağa Flow. Bu ayar, hub ve bağlı bileşen ağ topolojisini uygulamak için temel bir ayardır.
3. **Ağ Geçidi aktarımına izin ver:** Eşlenen sanal ağın bu sanal ağa bağlı sanal ağ geçidini kullanmasına izin vermek için bu kutuyu işaretleyin. Sanal ağ geçidi kaynağıyla sanal ağda *ağ geçidi aktarımına Izin ver* , ancak diğer sanal ağ üzerinde *uzak ağ geçitleri kullan* etkinse geçerlidir.
4. **Uzak ağ geçitlerini kullan:** Bu sanal ağdan gelen trafiğin, ile ilişkilendirilen sanal ağa bağlı bir sanal ağ geçidi üzerinden akmasını sağlamak için bu kutuyu işaretleyin. *Uzak ağ geçitleri* , sanal ağ geçidi olmadan sanal ağda etkinleştirilir ve yalnızca diğer sanal ağda *ağ geçidi aktarımına izin ver* seçeneği etkinse geçerlidir.

|Resource|Bağlantı|
|---|---|
| Kavramlar: Sanal ağ eşleme                   | [https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview) |
| Sanal ağ eşlemesi oluşturma, değiştirme veya silme | [https://docs.microsoft.com/azure/virtual-network/virtual-network-manage-peering](https://docs.microsoft.com/azure/virtual-network/virtual-network-manage-peering)|
|

### <a name="virtual-network-gateway"></a>Sanal ağ geçidi

Sanal ağ geçitleri, sanal ağları şirket içi ortamlar, iş ortağı ortamları ve diğer bulut dağıtımları gibi dış ağlarla tümleştirmek için bir mekanizma sağlar. İki tür sanal ağ geçidi ExpressRoute ve VPN ' dir.

#### <a name="expressroute-gateway"></a>ExpressRoute ağ geçidi

ExpressRoute ağ geçitleri, sanal ağdan şirket içi bir ortama çıkış noktası sağlar ve güvenlik, kullanılabilirlik, finansal ve performans gereksinimlerini karşılayacak şekilde dağıtılmalıdır. ExpressRoute ağ geçitleri, tanımlı bir ağ bant genişliği sağlar ve dağıtımdan sonra kullanım maliyetleri doğurur. Sanal ağlarda yalnızca bir ExpressRoute ağ geçidi olabilir, ancak bu birden fazla ExpressRoute devresine bağlanabilir ve VNet eşlemesi aracılığıyla birden çok sanal ağ tarafından yararlanılabilir olabilir ve bu da bant genişliği ve bağlantı sağlar. Şirket içi ortamlar ve sanal ağlar arasında ExpressRoute ağ geçitleri kullanılarak yönlendirmeyi yapılandırmak için, bilinen, denetimli ağ çıkış noktalarını kullanarak uçtan uca bağlantı sağlamak için dikkatli olunmalıdır. ExpressRoute özel eşlemesi üzerinden ExpressRoute ağ geçidi kullanan, uluslar arası varlıkların, ACSC tüketici kılavuzuyla uyum sağlamak üzere şirket içi ortama VPN bağlantısı kurması için de ağ sanal gereçleri (NVA) dağıtmaları gerekir.

ExpressRoute ağ geçitlerinin, adres aralıkları, topluluklar ve BGP aracılığıyla değiştirilen diğer belirli yapılandırma öğelerinde kısıtlamalar olduğuna dikkat edin.

| Resource  | Bağlantı  |
|---|---|
| ExpressRoute Gateway 'e genel bakış | [https://docs.microsoft.com/azure/expressroute/expressroute-about-virtual-network-gateways](https://docs.microsoft.com/azure/expressroute/expressroute-about-virtual-network-gateways) |
| ExpressRoute için sanal ağ geçidi yapılandırma | [https://docs.microsoft.com/azure/expressroute/expressroute-howto-add-gateway-portal-resource-manager](https://docs.microsoft.com/azure/expressroute/expressroute-howto-add-gateway-portal-resource-manager)
|

#### <a name="vpn-gateway"></a>VPN Gateway

Azure VPN Gateway, güvenli siteden siteye bağlantı için sanal ağdan bir dış ağa çıkış ağ noktası sağlar. VPN ağ geçitleri, tanımlı bir ağ bant genişliği sağlar ve dağıtımdan sonra kullanım maliyetlerini doğurur. VPN Gateway kullanan kurumlar varlıkları ACSC tüketici kılavuzlarıyla uyumlu olarak yapılandırıldığından emin olmalıdır. Sanal ağlarda yalnızca bir VPN Gateway olabilir, ancak bu birden çok tünelle yapılandırılabilir ve VNet eşlemesi aracılığıyla birden çok sanal ağ tarafından yararlanılabilir olabilir ve birden çok sanal ağın bant genişliği ve bağlantı paylaşmasına izin verir. VPN ağ geçitleri Internet üzerinden veya Microsoft eşlemesi üzerinden ExpressRoute üzerinden kurulabilir.

| Resource  | Bağlantı |
| --- | --- |
| VPN Gateway genel bakış| [https://docs.microsoft.com/azure/vpn-gateway](https://docs.microsoft.com/azure/vpn-gateway)|
| VPN Gateway için planlama ve tasarım | [https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-plan-design](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-plan-design)|
| Azure Avustralya 'da Azure VPN Gateway | [Azure Avustralya 'da Azure VPN Gateway](vpn-gateway.md)
|

### <a name="next-hop-of-virtual-appliance"></a>Sonraki Sanal Gereç atlaması

Sanal gerecin sonraki atlaması, ağ trafiğini sanal ağlara uygulanan Azure ağ ve yönlendirme topolojisi dışında işleme yeteneği sağlar. Sanal gereçler güvenlik kuralları uygulayabilir, adresleri çevirebilir, VPN 'Ler, proxy trafiği oluşturabilir veya başka bir özellik yelpaziyeti oluşturabilir. Sanal gerecin sonraki atlaması bir yol tablosunda UDRs aracılığıyla uygulanır ve bir Azure Güvenlik Duvarı, tek bir NVA veya Azure Load Balancer birden çok NVA 'lar arasında kullanılabilirlik sağlamak için kullanılabilir. Yönlendirme için bir sanal gereç kullanmak üzere, ilişkili ağ arabirimlerinin IP iletimi için etkinleştirilmesi gerekir.

| Resource  | Bağlantı |
| --- | ---|
| Yönlendirme Kavramları: Özel yollar | [https://docs.microsoft.com/azure/virtual-network/virtual-networks-udr-overview#custom-routes](https://docs.microsoft.com/azure/virtual-network/virtual-networks-udr-overview#custom-routes) |
| IP iletmeyi etkinleştirme veya devre dışı bırakma | [https://docs.microsoft.com/azure/virtual-network/virtual-network-network-interface#enable-or-disable-ip-forwarding](https://docs.microsoft.com/azure/virtual-network/virtual-network-network-interface#enable-or-disable-ip-forwarding)
|

### <a name="next-hop-of-virtualnetworkserviceendpoint"></a>VirtualNetworkServiceEndpoint 'in sonraki atlaması

VirtualNetworkServiceEndpoint 'in bir sonraki atlama türüne sahip yollar yalnızca bir alt ağda bir hizmet uç noktası yapılandırıldığında ve yol tabloları aracılığıyla el ile yapılandırılamaz.

### <a name="next-hop-of-internet"></a>Internet 'in sonraki atlaması

Sonraki atlama Internet, Azure bölgelerindeki PaaS ve Azure hizmetleri de dahil olmak üzere genel IP adresi kullanan kaynaklara ulaşmak için kullanılır. Sonraki atlama Internet, tüm ağları kapsayan varsayılan bir yol (0.0.0.0/0) gerektirmez, ancak belirli ortak hizmetlere yönlendirme yollarını etkinleştirmek için kullanılabilir. Internet 'in sonraki atlaması, Microsoft Yönetim adresleri gibi sistem işlevleri için gereken yetkili hizmetlere ve yeteneklere yollar eklemek için kullanılmalıdır.

Internet 'in bir sonraki atlaması kullanılarak eklenebilecek hizmetlere örnek olarak şunlar verilebilir:

1. Windows etkinleştirmesi için anahtar yönetim hizmetleri
2. App Service Ortamı Yönetimi

|Resource|Bağlantı|
|---|---|
| Azure 'da giden bağlantılar | [https://docs.microsoft.com/azure/load-balancer/load-balancer-outbound-connections](https://docs.microsoft.com/azure/load-balancer/load-balancer-outbound-connections) |
| KMS etkinleştirmesini etkinleştirmek için Azure özel yollarını kullanma | [https://docs.microsoft.com/bs-latn-ba/azure/virtual-machines/troubleshooting/custom-routes-enable-kms-activation](https://docs.microsoft.com/bs-latn-ba/azure/virtual-machines/troubleshooting/custom-routes-enable-kms-activation) |
| App Service Ortamı kilitleme  | [https://docs.microsoft.com/azure/app-service/environment/firewall-integration](https://docs.microsoft.com/azure/app-service/environment/firewall-integration) |
|

### <a name="next-hop-of-none"></a>Sonraki atlama yok

Bir sonraki atlama, belirli bir ağla iletişimi engellemek için kullanılabilir. Bir NSG 'nin aksine, trafiğin kullanılabilir bir ağ yolundan geçiş yapılmasına izin verildiğini veya reddedildiğini kontrol eden bir sonraki atlama atlaması, ağ yolunu tamamen kaldırır. Bir sonraki atlama olan yollar oluşturulurken dikkatli olunmalıdır, özellikle bu durum, istenmeyen sonuçlara yol açabilir ve sistem sorunlarını karmaşık ve zaman alan sorun gidermeye neden olabilir.

## <a name="security"></a>Güvenlik

IaaS ve PaaS özelliklerine ağ kesimleme ve ayırma denetimleri uygulamak, yeteneklerin kendilerine güvenilerek ve ile iletişim kuran sistemlerden denetimli iletişim yollarını uygulayarak elde edilir. yapma.

Azure 'da çözümler tasarlama ve oluşturma, tüm Azure varlığı genelinde ağ kaynaklarını anlamak, denetlemek ve izlemek için bir mantıksal mimari oluşturma işlemidir. Bu mantıksal mimari, Azure platformunda tanımlanan yazılımdır ve geleneksel ağ ortamlarında uygulanan bir fiziksel ağ topolojisinin yerini alır.

Oluşturulan mantıksal mimari, kullanılabilirlik için gereken işlevselliği sağlamalıdır, ancak güvenlik ve bütünlük için gereken görünürlük ve denetimi de sağlamalıdır.

Bu sonucun sağlanması, gerekli ağ kesimlemesi ve ayırma araçlarının uygulanmasına ve ayrıca ağ topolojisini koruma ve zorunlu tutma ve bu araçların uygulanmasını temel alır.

### <a name="network-security-groups-nsgs"></a>Ağ güvenlik grupları (NSG 'ler)

NSG 'ler, bir alt ağ veya belirli bir ağ arabirimi için izin verilen gelen ve giden trafiği belirtmek için kullanılır. NSG 'leri yapılandırırken, bir izin verme bildirimiyle eşleşmeyen tüm trafiği reddedecek şekilde yapılandırılmış bir varsayılan kuralla izin vermek üzere kuralların yapılandırıldığı bir beyaz listeleme yaklaşımı kullanmalıdır. NSG 'leri planlarken ve yapılandırırken, tüm gerekli gelen ve giden trafiğin uygun şekilde yakalandığından emin olmak için dikkatli olunmalıdır. Bu, sanal ağlar ve şirket içi ortam içinde kullanılan tüm özel IP adresi aralıklarını ve Azure Load Balancer ve PaaS yönetim gereksinimleri gibi belirli Microsoft hizmetlerini tanımlamayı ve tanımlamayı içerir. NSG 'lerin tasarımına ve uygulamasına dahil olan bireyler, ayrıntılı, hizmet ve uygulamaya özgü güvenlik kuralları oluşturmak için hizmet etiketlerinin ve uygulama güvenlik gruplarının kullanımını da anlamalıdır.

Bir NSG 'nin varsayılan yapılandırmasının, sanal ağ içindeki tüm adreslere ve tüm genel IP adreslerine giden trafiğe izin verdiğini aklınızda olması önemlidir.

|Resource|Bağlantı|
|---|---|
|Ağ güvenliğine genel bakış | [https://docs.microsoft.com/azure/virtual-network/security-overview](https://docs.microsoft.com/azure/virtual-network/security-overview)|
|Ağ güvenlik grubu oluşturma, değiştirme veya silme | [https://docs.microsoft.com/azure/virtual-network/manage-network-security-group](https://docs.microsoft.com/azure/virtual-network/manage-network-security-group)|
|

### <a name="azure-firewall"></a>Azure Güvenlik Duvarı

Azure Güvenlik Duvarı, hub ve bağlı bileşen ağ topolojisi oluşturmak ve denetleyebileceği ağ güvenlik denetimlerini zorlamak için kullanılabilir. Azure Güvenlik Duvarı, yalnızca sistem işlevleri için gereken IP adresleri, protokoller, bağlantı noktaları ve FQDN 'lerin yetkilendirdiği bir liste yaklaşımı uygulayarak, çıkış trafiği için gereken gereksinimleri karşılamak üzere kullanılabilir. Kurumlar varlıkları, Azure Güvenlik Duvarı tarafından sunulan güvenlik yeteneklerinin gereksinimler için yeterli olup olmadığını belirlemede riske dayalı bir yaklaşım almalıdır. Azure Güvenlik Duvarı tarafından sağlananlar dışında ek güvenlik özellikleri kullanılması gerektiği senaryolarda, NVA 'lar uygulamayı dikkate almalısınız.

|Resource|Bağlantı|
|---|---|
|*Azure Güvenlik Duvarı belgeleri* | [https://docs.microsoft.com/azure/firewall](https://docs.microsoft.com/azure/firewall)|
|*Öğretici: Azure PowerShell kullanarak Azure Güvenlik duvarını karma ağda dağıtma ve yapılandırma* | [https://docs.microsoft.com/azure/firewall/tutorial-hybrid-ps](https://docs.microsoft.com/azure/firewall/tutorial-hybrid-ps)|
|

### <a name="network-virtual-appliances-nvas"></a>Ağ sanal araçları (NVA 'lar)

NVA 'lar, bir hub ve bağlı ağ topolojisi oluşturmak, gelişmiş veya tamamlayıcı ağ yetenekleri sağlamak ya da şirket içi ağ hizmetleriyle benzerlik ve tutarlı destek ve yönetim için Azure ağ mekanizmalarına bir alternatif olarak kullanılmak üzere kullanılabilir. NVA 'lar, gibi belirli güvenlik gereksinimlerini karşılamak için dağıtılabilir; ağ trafiği, HTTPS şifre çözme, içerik incelemesi, filtreleme veya diğer güvenlik özellikleri ile ilişkili kimlik tanıma için bir gereksinim olduğu senaryolar. NVA 'lar, yüksek kullanılabilirliğe sahip bir yapılandırmada dağıtılmalıdır ve NVA 'lar 'in tasarımına veya uygulamasına dahil olan bireyler, Azure 'da dağıtım hakkında yönergeler için uygun satıcı belgelerine başvurmalıdır.

|Resource|Bağlantı|
|---|---|
|*Yüksek oranda kullanılabilir ağ sanal gereçlerini dağıtma* | [https://docs.microsoft.com/azure/architecture/reference-architectures/dmz/nva-ha](https://docs.microsoft.com/azure/architecture/reference-architectures/dmz/nva-ha)|
|

### <a name="service-endpoint-policies-preview"></a>Hizmet uç noktası ilkeleri (Önizleme)

Hizmet uç noktası ilkelerini, hizmetin kullanılabilirliğine ve veri kullanımı olasılığının ve etkilerine ilişkin bir güvenlik riski değerlendirmesine bağlı olarak yapılandırın. Hizmet uç noktası ilkeleri, Azure depolama için kabul edilmelidir ve ilgili risk profilini temel alan diğer hizmetler için büyük/küçük harfe göre yönetilir.

| Resource | Bağlantı  |
| --- | --- |
| *Hizmet uç noktası ilkelerine genel bakış* | [https://docs.microsoft.com/azure/virtual-network/virtual-network-service-endpoint-policies-overview](https://docs.microsoft.com/azure/virtual-network/virtual-network-service-endpoint-policies-overview) |
| *Azure portal kullanarak hizmet uç noktası ilkesi oluşturma, değiştirme veya silme* | [https://docs.microsoft.com/azure/virtual-network/virtual-network-service-endpoint-policies-portal](https://docs.microsoft.com/azure/virtual-network/virtual-network-service-endpoint-policies-portal)
|

### <a name="azure-policy"></a>Azure İlkesi

Azure Ilkesi, Azure ortamının mantıksal mimarisinin bütünlüğünü zorlamaya ve korumaya yönelik bir temel bileşendir. Azure hizmetleri aracılığıyla kullanılabilen çeşitli hizmetler ve çıkış ağ trafiği yolları vardır. Kurumlar varlıklarının, ortamları içinde mevcut olan kaynakların ve kullanılabilir ağ çıkış noktalarında farkında olması önemlidir. Yetkili olmayan ağ çıkış noktalarının Azure ortamında oluşturulmadığından emin olmak için, bu kaynakların dağıtılabilecek kaynak türlerini ve bu kaynakların yapılandırmasını denetlemek için Azure Ilkesini kullanması gerekir. Pratik örnekler, kaynakların yalnızca yetkili ve kullanım için kısıtlanması ve NSG 'lerin alt ağlara eklenmesine gerek vardır.

|Resource | Bağlantı|
|---|---|
|*Azure Ilkesine genel bakış* | [https://docs.microsoft.com/azure/governance/policy/overview](https://docs.microsoft.com/azure/governance/policy/overview)|
|*İzin verilen kaynak türleri örnek ilkesi* | [https://docs.microsoft.com/azure/governance/policy/samples/allowed-resource-types](https://docs.microsoft.com/azure/governance/policy/samples/allowed-resource-types)|
|*Alt ağ örnek ilkesinde NSG 'yi zorla*| [https://docs.microsoft.com/azure/governance/policy/samples/nsg-on-subnet](https://docs.microsoft.com/azure/governance/policy/samples/nsg-on-subnet)|
|

## <a name="paas-egress-capabilities"></a>PaaS çıkış özellikleri

PaaS özellikleri, daha fazla işlevsellik ve basitleştirilmiş yönetim olanakları sağlar, ancak ağ kesimlemesi ve ayrımı için adres gereksinimlerinde karmaşıklıklar sağlar. PaaS özellikleri genellikle genel IP adresleriyle yapılandırılır ve Internet 'ten erişilebilir.  Sistemlerinizde ve çözümlerinde PaaS özellikleri kullanıyorsanız, bileşenler arasındaki iletişim akışını tanımlamak ve yalnızca bu iletişime izin vermek için ağ güvenlik kuralları oluşturmak üzere dikkatli olunması gerekir. Güvenlik konusunda derinlemesine savunma yaklaşımı kapsamında, PaaS özellikleri şifreleme, kimlik doğrulama ve uygun erişim denetimleri ve izinlerle yapılandırılmalıdır.  

### <a name="public-ip-for-paas"></a>PaaS için genel IP

PaaS özellikleri için genel IP adresleri, hizmetin barındırıldığı veya dağıtıldığı bölgeye göre ayrılır. Ağ kesimlemesi için uygun ağ güvenliği kuralları ve yönlendirme topolojisi ve Azure sanal ağlarını, PaaS ve ExpressRoute 'u kapsayan ve Internet bağlantısı. Azure, IP adreslerini her bir Azure bölgesine ayrılmış bir havuzdan ayırır. Microsoft, her bölgede kullanılan, düzenli ve denetimli bir şekilde güncelleştirilmiş olan adreslerin indirilmesini sağlar. Yeni hizmetler serbest bırakıldığında veya hizmetler daha yaygın olarak dağıtıldığında, her bölgede kullanılabilen hizmetler de sık değişir. Kurumlar varlıklarının bu malzemeleri düzenli olarak incelemesi gerekir ve gerektiğinde sistemleri sürdürmek için Otomasyon kullanabilir. Her bölgede barındırılan bazı hizmetlere yönelik belirli IP adresleri, Microsoft desteğiyle iletişim kurarak elde edilebilir.

| Resource | Bağlantı |
| --- | --- |
| *Microsoft Azure Veri Merkezi IP Aralıkları*                   | [https://www.microsoft.com/download/details.aspx?id=41653](https://www.microsoft.com/download/details.aspx?id=41653) |
| *Bölge başına Azure hizmetleri*                              | [https://azure.microsoft.com/global-infrastructure/services/?regions=non-regional Avustralya-orta, Avustralya-Orta-2, Avustralya-Doğu, Avustralya-Güneydoğu & Ürünler = tümü](https://azure.microsoft.com/global-infrastructure/services/?regions=non-regional,australia-central,australia-central-2,australia-east,australia-southeast&products=all) |
| *Azure App Service gelen ve giden IP adresleri* | [https://docs.microsoft.com/azure/app-service/overview-inbound-outbound-ips](https://docs.microsoft.com/azure/app-service/overview-inbound-outbound-ips)
|

## <a name="next-steps"></a>Sonraki adımlar

Genel mimarinizi ve tasarımınızı [IaaS ve PaaS Web uygulamalarına yönelik yayımlanmış korumalı](https://aka.ms/au-protected)şemalara karşılaştırın.
