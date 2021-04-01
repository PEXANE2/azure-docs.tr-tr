---
title: 'Azure ExpressRoute: asimetrik yönlendirme'
description: Bu makalede, hedefe birden çok bağlantısı olan bir ağda asimetrik yönlendirmeye sahip olabileceğiniz sorunlar anlatılmaktadır.
services: expressroute
author: duongau
ms.service: expressroute
ms.topic: article
ms.date: 12/14/2020
ms.author: duau
ms.openlocfilehash: 0713c52c7f07db93d9ae9084062ef2c3b25a9074
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2021
ms.locfileid: "97560517"
---
# <a name="asymmetric-routing-with-multiple-network-paths"></a>Birden çok ağ yoluyla Asimetrik yönlendirme
Bu makalede, ağ trafiği ile hedef arasında birden çok yol olduğunda ağ trafiğinin farklı yolları nasıl ele aldığı açıklanmaktadır.

Asimetrik yönlendirmeyi anlamak için bilmeniz gereken iki kavram vardır. Birincisi, birden çok ağ yolunun etk' i. Diğer bir deyişle, güvenlik duvarı tutma durumu gibi cihazların nasıl olduğu. Bu tür cihazlara durum bilgisi olan cihazlar denir. Bu iki faktör birleştirildiğinde, ağ trafiğinin durum bilgisi olan cihaz tarafından bırakılmakta olduğu bir senaryo oluşturabilir.  Trafiğin kendisinden geldiğini algılamadığı için trafik bırakılır.

## <a name="multiple-network-paths"></a>Birden çok ağ yolu
Bir kurumsal ağın internet 'e yönelik yalnızca bir bağlantısı olduğunda, internet 'ten gelen ve giden tüm trafik aynı yolu hareket eder. Şirketlerin, ağ çalışma süresini geliştirmek üzere gereksiz yollar oluşturmak için birden çok devreler satın aldığı yaygın bir deneyimdir. Bu yapılandırma türü ile trafiğin internet 'e bir bağlantı üzerinden geçmesi ve farklı bir bağlantı üzerinden geri dönüş olasılığı vardır. Bu senaryo genellikle asimetrik yönlendirme olarak bilinir. Asimetrik yönlendirme ' de, dönüş ağ trafiği, özgün çıkış akışından farklı bir yol alır.

![Birden çok yola sahip ağ](./media/expressroute-asymmetric-routing/AsymmetricRouting3.png)

Asimetrik yönlendirme genellikle internet 'e giderken gerçekleşse de gerçekleşir. Ayrıca birden çok yolun birleşimi ortaya çıkar. İlk örnek, bir internet yolu ve aynı hedefe giden özel bir yoldur. İkinci örnek, aynı hedefe yönelik birden çok özel yolunuz olduğunda da kullanılır.

Kaynak ve hedef arasındaki yol üzerindeki her yönlendirici, hedefe ulaşmak için en iyi yolu hesaplar. Yönlendirici iki ana etkene göre olası en iyi yolu belirler:

* Dış ağlar arası yönlendirme, Sınır Ağ Geçidi Protokolü (BGP) olarak bilinen bir yönlendirme protokolüne bağlıdır. BGP, komşulardan tanıtımları toplar ve bunları bir dizi adımdan geçirip hedefe yönelik en uygun yolu belirler. BGP, bu en uygun yolu yönlendirme tablosunda depolar.
* Bir rota ile ilişkili alt ağ maskesinin uzunluğu yönlendirme yollarını etkiler. Yönlendirici aynı IP adresi için birden fazla reklam alırsa, yönlendirici daha fazla belirli bir yol olarak kabul edildiği için daha uzun alt ağ maskesiyle yolu seçer.

## <a name="stateful-devices"></a>Durum bilgisi olan cihazlar
Yönlendiriciler, yönlendirme amacıyla paketlerin IP üst bilgisine bakar. Bazı cihazlar, pakete daha da ayrıntılı şekilde bakar. Genellikle, bu cihazlar katman 4-Iletim Denetim Protokolü (TCP) veya Kullanıcı Datagram Protokolü (UDP), hatta katman 7 (uygulama katmanı) üst bilgilerine bakar. Bu tür cihazlar, güvenlik cihazları ya da bant genişliği iyileştirme cihazlarıdır. 

Durum bilgisi olan cihazlar için tipik bir örnek güvenlik duvarıdır. Bir güvenlik duvarı, paketlerin çeşitli ölçütlere göre arayüzlerini geçmesine izin verir veya reddeder. Bu ölçütler arasında protokol, TCP/UDP bağlantı noktası ve URL üstbilgileri sayılabilir ancak bunlarla sınırlı değildir. Bu paket incelemesi düzeyi, cihaza ağır bir işlem yükü yerleştirebilir. 

Güvenlik duvarı, performansı artırmak için bir akıştaki ilk paketi denetler. Paketin arabirimleri üzerinden geçmesine izin veriyorsa, akış bilgilerini durum tablosunda tutar. Bu akışla ilgili tüm paketlere, ilk belirlenme göre izin verilir. Mevcut bir akışın parçası olan bir paket, öğesinden kaynaklanmayan güvenlik duvarlarına gelebilir. İlk akış hakkında önceden durum bilgisi olmadığından, güvenlik duvarı paketi bırakır.

## <a name="asymmetric-routing-with-expressroute"></a>ExpressRoute ile asimetrik yönlendirme
Azure ExpressRoute üzerinden Microsoft’a bağlandığınızda, ağınız aşağıdaki şekilde değişir:

* Birden fazla Microsoft bağlantınız var. Bir bağlantı var olan Internet bağlantınız ve diğeri de ExpressRoute bağlantınızda. Microsoft 'a yönelik belirli trafik, internet bağlantısı üzerinden geçebilir, ancak ExpressRoute bağlantınızın üzerine dönebilir. Ayrıca, trafik ExpressRoute üzerinden geçtiğinde ancak internet yolundan geri dönzaman de aynı durum oluşabilir.
* ExpressRoute bağlantı hattı 'ndan daha belirli IP adresleri aldınız. Bu nedenle, ağınızdaki trafik ExpressRoute aracılığıyla sunulan hizmetler için Microsoft 'a gittiğinde, yönlendiricileriniz her zaman ExpressRoute bağlantısını tercih eder.

Bu iki değişikliğin bir ağ üzerinde nasıl olduğunu anlamak için bazı senaryolara göz atalım. Örnek olarak, internet 'e yönelik bir devreniz var ve tüm Microsoft hizmetlerini internet üzerinden tükettiğinizde. Ağınızdan ve Microsoft 'a giden trafik aynı internet bağlantısını geçer ve bir güvenlik duvarından geçer. Güvenlik Duvarı ilk paketi gördüğünde akışı kaydeder. Akış durum tablosunda var olduğundan, bu görüşmenin her bir öğesine yönelik paketlere izin verilir.

![ExpressRoute ile asimetrik yönlendirme](./media/expressroute-asymmetric-routing/AsymmetricRouting1.png)

Daha sonra ExpressRoute üzerinden Microsoft tarafından sunulan hizmetleri kullanmak için bir ExpressRoute devresi alırsınız. Microsoft 'un tüm diğer hizmetleri Internet üzerinden kullanılır. ExpressRoute bağlantısına bağlı olan kenarınızdan ayrı bir güvenlik duvarı dağıtırsınız. Microsoft, belirli hizmetler için ExpressRoute üzerinden ağınıza daha belirli ön ekler tanıtır. Yönlendirme altyapınız bu ön ekler için tercih edilen yol olarak ExpressRoute’u seçer. 

Genel IP adreslerinizi ExpressRoute üzerinden Microsoft 'a bildirmezseniz. Microsoft, internet üzerinden genel IP adreslerinizle iletişim kuracak. Ağınızdan Microsoft 'a gönderilen trafik ExpressRoute bağlantısını kullanır, ancak Microsoft 'un döndürdüğü trafiği Internet yolunu kullanacaktır. Kenarınızdaki güvenlik duvarı, hakkında bilgi sahibi olmadığı bir akış için bir yanıt paketi gördüğünde, bu paketleri düşürülecektir.

ExpressRoute ve internet için aynı ağ adresi çevirisi (NAT) havuzunu tanıtmayı seçerseniz. Ağınızdaki istemcilerle özel IP adresleri hakkında benzer sorunlar görürsünüz. Bu hizmetlerin IP adresleri ExpressRoute üzerinden tanıtılmadığından, Windows Update gibi hizmetlere yönelik istekler internet üzerinden gider. Ancak, dönüş trafiği ExpressRoute üzerinden geri gelir. Microsoft, internet ve ExpressRoute 'tan aynı alt ağ maskesine sahip bir IP adresi aldığından, tercih edilen yol her zaman ExpressRoute ' dır. Ağ kenarınızdaki bir güvenlik duvarı ya da ExpressRoute bağlantısına bakan başka bir durum bilgisi olan bir cihazın bir akışa ilişkin önceki bilgileri yoksa, bu paketleri bırakacaktır.

## <a name="asymmetric-routing-solutions"></a>Asimetrik yönlendirme çözümleri
Asimetrik yönlendirme sorununu çözmek için iki seçeneğiniz vardır. İlki yönlendirerek, ikincisi ise kaynak tabanlı NAT (SNAT) kullanmaktır.

### <a name="routing"></a>Yönlendirme
Genel IP adreslerinizin uygun geniş alan ağı (WAN) bağlantılarına tanıtıldığından emin olun. Örneğin, internet 'i posta trafiğiniz için kimlik doğrulama trafiği ve ExpressRoute için kullanmak istiyorsanız. Active Directory Federasyon Hizmetleri (AD FS) (AD FS) genel IP adreslerini ExpressRoute üzerinden duyurmayın. Ayrıca, şirket içi AD FS sunucunuzu, yönlendiricinin ExpressRoute üzerinden aldığı IP adreslerine sunmadığından emin olun. ExpressRoute üzerinden alınan rotalar daha belirli olduğundan, bunlar Microsoft’a yönelik kimlik doğrulama trafiği için ExpressRoute’u tercih edilen yol haline getirir. Ağınızda yönlendirmenin nasıl yapılacağı konusunda dikkat etmeniz gerekmiyorsa, asimetrik yönlendirme sorunları ortaya çıkabilir.

Kimlik doğrulaması için ExpressRoute 'u kullanmak istiyorsanız, bir NAT olmadan ExpressRoute üzerinden AD FS genel IP adreslerini tanıttığınızdan emin olun. Bu şekilde yapılandırıldığında, AD FS Microsoft 'tan kaynaklanan trafik, ExpressRoute üzerinden geçer. Ağınızdan Microsoft 'a giden dönüş trafiği, Internet üzerinden tercih edilen yol olduğundan ExpressRoute 'u kullanır.

### <a name="source-based-nat"></a>Kaynak tabanlı NAT
Asimetrik yönlendirme sorununu çözmenin bir başka yolu da SNAT kullanmaktır. Örneğin, ExpressRoute üzerinden şirket içi bir Basit Posta Aktarım Protokolü (SMTP) sunucusunun genel IP adresini tanıtmayı tercih edebilirsiniz. Bunun yerine, bu tür bir iletişim için interneti kullanmayı amaçladığınızı. Microsoft 'tan gelen ve şirket içi SMTP sunucunuza giden bir istek internet 'ten geçer. Gelen isteğe SNAT uygulayarak bir dahili IP adresine yönlendirirsiniz. SMTP sunucusundan gelen dönüş trafiği, ExpressRoute aracılığıyla değil, uç güvenlik duvarına (NAT için kullanacağınız) gider. Sonuç olarak, dönüş trafiği Internet yolunu alır.

![Kaynak tabanlı NAT ağ yapılandırması](./media/expressroute-asymmetric-routing/AsymmetricRouting2.png)

## <a name="asymmetric-routing-detection"></a>Asimetrik yönlendirmenin algılanması
Traceroute, ağ trafiğinizin beklenen yoldan gittiğinden emin olmanın en iyi yoludur. Şirket içi SMTP sunucunuzdaki trafiği Internet yolunu alacak şekilde bekliyorsanız, beklenen izleme yolu SMTP sunucusundan Microsoft 365. Sonuç, trafiğin ExpressRoute 'a doğru değil, ağınızı Internet 'e doğru bırakıyor olduğunu doğrular.

