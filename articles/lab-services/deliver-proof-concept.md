---
title: Kavram kanıtı sunma-Azure DevTest Labs | Microsoft Docs
description: Azure DevTest Labs bir kurumsal ortama başarıyla dahil edilmesi için bir kavram kanıtı sunma hakkında bilgi edinin.
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: tanmayeekamath
manager: femila
editor: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/23/2018
ms.author: takamath
ms.openlocfilehash: 6739679b1687393737dda1ded4265a95c4fce169
ms.sourcegitcommit: a0b37e18b8823025e64427c26fae9fb7a3fe355a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/25/2019
ms.locfileid: "68501552"
---
# <a name="delivering-a-proof-of-concept"></a>Kavram kanıtı sunma 

Azure DevTest Labs yönelik temel senaryolarından biri, bulutta geliştirme ve test ortamlarını etkinleştirir. Örneklere şunlar dahildir:

* bulutta geliştirici masaüstleri oluşturma,
* ortamları test için yapılandırma,
* sanal makinelere ve diğer Azure kaynaklarına erişimi etkinleştirme,
* geliştiricilerin öğrenmek ve denemek için bir korumalı alan alanı ayarlama.

DevTest Labs Ayrıca, kuruluşun kurumsal güvenlik, mevzuata ve uyumluluk ilkelerine bağlı olan geliştiricilere "self servis Azure" sağlamasına olanak tanıyan ilke ve maliyet denetimleri sağlar. 

Her kuruluşun, Azure DevTest Labs başarıyla ortamlarına nasıl dahil edilebilir olması için farklı gereksinimleri vardır. Bu makalede, kuruluşların başarılı bir uçtan uca dağıtıma yönelik ilk adım olan başarılı bir kavram kanıtı sağlamak için genellikle gereken en yaygın adımlar açıklanır. 

## <a name="getting-started"></a>Başlarken 

Kavram kanıtı sunmaya başlamak için. Azure ve DevTest Labs hakkında bilgi edinmek için biraz zaman harcamanız önemlidir.  Kaynak başlangıç kaynakları aşağıda verilmiştir: 

* [Azure portal anlama](https://azure.microsoft.com/features/azure-portal/)
* [DevTest Labs ile ilgili temel bilgiler](devtest-lab-overview.md)
* [DevTest Labs desteklenen senaryolar](devtest-lab-guidance-get-started.md)
* [DevTest Labs kurumsal belgeleri](devtest-lab-guidance-prescriptive-adoption.md)
* [Azure ağa giriş](../virtual-network/virtual-networks-overview.md)

## <a name="prerequisites"></a>Önkoşullar 

DevTest Labs ile pilot veya kavram kanıtı 'nı başarılı bir şekilde tamamlamaya yönelik birkaç önkoşul vardır: 

* **Azure aboneliği**: Kuruluşlar, Azure 'a erişim sağlayan mevcut bir [Kurumsal Anlaşma](https://azure.microsoft.com/pricing/purchase-options/enterprise-agreement/) , DevTest Labs için de mevcut veya yeni bir aboneliğe sahip olabilir. Alternatif olarak, bir [Visual Studio aboneliği](https://azure.microsoft.com/pricing/member-offers/visual-studio-subscriptions/) pilot sırasında (ücretsiz Azure kredilerinden yararlanarak) kullanılabilir. Bu seçeneklerden hiçbiri yoksa, [ücretsiz bir Azure hesabı](https://azure.microsoft.com/free/search/?&OCID=AID719825_SEM_g4lyBqgB&lnkd=Bing_Azure_Brand&msclkid=ecc4275a31b61375749e7a5322c20de8&dclid=CMGW5-m78-ICFaLt4QodmUwGtQ) oluşturulabilir ve kullanılabilir. Bir Kurumsal Anlaşma varsa, [Kurumsal Geliştirme ve test aboneliği](https://azure.microsoft.com/offers/ms-azr-0148p/) kullanmak, Windows 10/Windows 8.1 istemci işletim sistemlerine ve geliştirme ve test iş yüklerine yönelik indirimli ücretler için erişim sağlamak için harika bir seçenektir. 
* **Azure Active Directory kiracı**:  Kullanıcıları yönetmeyi etkinleştirmek için (örneğin, Kullanıcı ekleme veya laboratuvar sahipleri ekleme), bu kullanıcıların pilot için Azure aboneliğinde kullanılan [Azure Active Directory kiracının](https://azure.microsoft.com/services/active-directory/) bir parçası olması gerekir. Genellikle kuruluşlar, kullanıcıların bulutta şirket içi kimliklerini kullanmasını sağlamak için [karma kimlik](../active-directory/hybrid/whatis-hybrid-identity.md) ayarlayacaktır, ancak bu, DevTest Labs pilot için gerekli değildir. 

## <a name="scoping-of-the-pilot"></a>Pilot kapsamı 

Uygulamayı başlatmaya başlamadan önce uygun bir pilot plan planlamak çok önemlidir. Kaynakların süresiz olarak sürekli kalmayacağından önce, pilot kullanıcıları için uygun beklentileri ayarlar. 

> [!IMPORTANT]
> Pilot ve ayar beklentisi 'nin kapsam kapsamının yeterince önemli olduğunu vurgulamıyoruz.

Pilot uygulamadan önce yanıt vermek için önemli sorular vardır: 

* Ne öğrenmek istiyorsunuz ve pilot için başarı nasıl görünüyor? 
* Pilot kapsamında hangi iş yükleri veya senaryolar ele alınacaktır? Pilot 'nın kapsama ve hızla tamamlanmasını sağlamak için yalnızca küçük bir küme tanımlamanız önemlidir. 
* Laboratuvarda hangi kaynakların kullanılabilir olması gerekir? Örneğin: özel görüntüler, Market görüntüleri, ilkeler, ağ topolojisi vb. 
* Deneyimi doğrulamak üzere pilot kullanıcıya dahil edilecek son kullanıcılar/takımlar kim?  
* Pilot 'un süresi nedir? İki hafta veya bir ay gibi planlı kapsamın yanı sıra bir zaman dilimi seçin. 
* Pilot işlemi tamamlandıktan sonra, pilot sırasında kullanılan ayrılmış kaynaklarla ne olur? Pilot Kaynakları atmayı planlıyor musunuz?
   
   Pilot 'ın sonunda sanal makineleri ve laboratuvarları oluşturma konusunda planlayabilmemiz için yalnızca pilot için tek bir abonelik ayarlayabiliriz ve tüm çalışmalarımızı, paralel olarak ölçek yayma sorularını çözümliyoruz. 

Pilot "kusursuz" hale getirmek, bu sayede hizmetin şirkette kullanıma alındığı son durumun ne olduğunu belirten bir genel kullanıma sunulacaktır. Bu bir yanlış varsayım. Pilot uygulamayı kullanmaya *başlamadan önce* daha fazla "kusursuz" olmanız gerekir. Pilot 'ın amacı, son hizmeti ölçeklendirmek ve kullanıma sunmak için doğru kararları almaktır. 

Pilot 'ın odaklanmasına, Azure DevTest Labs kuruluşunuza doğru hizmet olup olmadığı konusunda yanıt vermek için gereken en düşük gerekli iş yüklerini ve bağımlılıklarını çekmeye yönelik olması gerekir. Daha hızlı ve temiz başarı sağlamak için en az bağımlılıklara sahip en basit iş yükünü seçmeniz önerilir. Bu mümkün değilse, bir sonraki en iyi seçenek, pilot aşamadaki başarı, genişleme aşamasında çoğaltılabilmesi için olası karmaşıklıkları açığa çıkaran en iyi bir iş yükü seçmektir. 

Aşağıdaki örnekte, kavram kapsamlı bir kavram kanıtı gösterilmektedir. 

## <a name="example-proof-of-concept-plan"></a>Örnek: kavram kanıtı planı 

Bu bölümde, bir pilot for DevTest Labs kavramının kapsamını belirlemek için kullanılacak bir örnek gösterilmektedir. 

> [!TIP]
> Projenizi hata için ayarlama olasılığını en aza indirmek için, bu bölümde açıklanan örneği atlamanızı kesinlikle öneririz. 

### <a name="overview"></a>Genel Bakış 

Şirket ağından yalıtılmış bir ortam olarak kullanılacak satıcıların DevTest Labs temelinde Azure 'da yeni bir ortam geliştirmeyi planlıyoruz. Çözümün gereksinimleri karşılayıp karşılamadığını öğrenmek için uçtan uca çözümü doğrulamaya yönelik bir kavram kanıtı geliştiriyoruz ve bu deneyimin belirlenmesi ve doğrulanması için birkaç satıcı ekledik. 

### <a name="outcomes"></a>Sonucunu 

Kavram kanıtı oluştururken, ilk olarak sonuçlar üzerinde odaklanıyoruz (elde edilmeye çalışıyoruz). Bunlar burada listelenir.  Kavram kanıtı 'nın sonuna kadar şunu bekliyoruz: 

* Satıcıların, üretken olmaları için gereken kaynakları içeren yalıtılmış bir ortama erişmek üzere Azure Active Directory (Azure AD) ' deki Konuk hesapların yararlanması için çalışan bir uçtan uca çözüm. 
* Daha geniş ölçekli kullanımı ve benimsemeyi etkileyen olası engelleme sorunları numaralandırılır ve anlaşılır.
* Kavram kanıtı geliştirmede yer alan kişiler tüm kodun iyi bir şekilde anlaşılmasıdır. Ayrıca, dahil edilen ve daha fazla benimseme konusunda emin olan yardımcı

### <a name="open-questions--prerequisites"></a>Önkoşulları & açık sorular 

* Bu proje için kullanabilmemiz için bir abonelik oluşturuldu mu? 
* Azure AD kiracımız var ve Azure AD Genel Yöneticisi, Azure AD ile ilgili sorular için yardım & rehberlik sağlayabildiğimiz belirlendi. 
* Projede çalışan bireyler için işbirliği yapabileceğiniz bir yer: 

   * Kaynak kodu ve betikler (Azure Repos gibi) 
   * Belgeler (takımlar veya SharePoint gibi)  
   * Konuşmalar (Microsoft ekipleri gibi) 
   * İş öğeleri (Azure Boards gibi) 
* Satıcılar için gereken kaynaklar nelerdir? Bu, hem sanal makinelerde hem de ağ üzerinde bulunan diğer gerekli sunuculara yerel olarak dahildir. 
* Sanal makineler, Azure 'daki bir etki alanına katılmalıdır mi? Öyleyse, Azure AD Domain Services veya başka bir şey olacak mı? 
* Ekip/satıcıların kavram kanıtı 'nın hedefi olacak şekilde tanımlı olduğunu belirledik mı? Ortamın müşterileri kim olacak?
* Kavram kanıtı için hangi Azure bölgesini kullanacağız? 
* Satıcıların IaaS (VM 'Ler) yanı sıra DevTest Labs aracılığıyla kullanmasına izin verilen bir hizmet listesi var mı? 
* Laboratuvarı kullanarak satıcıları/kullanıcıları eğitim konusunda nasıl planlarız? 

### <a name="proof-of-concept-solution-components"></a>Kavram kanıtı çözüm bileşenleri 

Çözümün aşağıdaki bileşenlere sahip olmasını bekliyor: 

* Çeşitli satıcı takımları için Azure 'daki DevTest Labs kümesi.
* Laboratuvarlar, gereksinimleri destekleyen bir ağ altyapısına bağlıdır.
* Satıcıların Azure AD aracılığıyla laboratuvarlara erişimi vardır ve laboratuvara rol atamaları verebilir.
* Satıcıların kaynaklarına başarıyla bağlanması için bir yol. Özellikle, doğrudan genel IP adresleri olmadan sanal makinelere erişimi etkinleştirmek için siteden siteye VPN.
* Sanal makinelerde satıcıların gerektirdiği gerekli yazılımları kapsayan bir yapıt kümesi.

## <a name="additional-planning-and-design-decisions"></a>Ek planlama ve tasarım kararları 

Bir tam DevTest Labs çözümünü serbest bırakmadan önce, yapılacak bazı önemli planlama ve tasarım kararları vardır. Kavram kanıtı üzerinde çalışma deneyimi, bu kararları almanıza yardımcı olabilir. Daha ayrıntılı bilgiler şunlardır: 

* **Abonelik topolojisi**: Azure 'daki kaynaklara yönelik kurumsal düzeyde gereksinimler, [tek bir abonelikteki kullanılabilir kotaların](https://docs.microsoft.com/azure/azure-subscription-service-limits)ötesine genişletilebilmesi için, ilk abonelik limitlerini artırmak üzere birden çok Azure aboneliğine ve/veya hizmet isteğine da sahiptir. Kaynakları abonelikler arasında nasıl dağıtacağınıza karar vermeniz önemlidir. kaynakların daha sonra başka bir aboneliğe taşınması zor olduğundan, tek değerli bir kaynak [abonelik karar kılavuzudur](https://docs.microsoft.com/azure/architecture/cloud-adoption/decision-guides/subscriptions/) . Örneğin, bir DevTest Laboratuvarı oluşturulduktan sonra başka bir aboneliğe taşınamaz.  
* **Ağ topolojisi**: DevTest Labs tarafından otomatik olarak oluşturulan [varsayılan ağ altyapısı](../app-service/networking-features.md) , kurumsal kullanıcıların gereksinimlerini ve kısıtlamalarını karşılamak için yeterli olmayabilir. [ExpressRoute 'a bağlı VNET](https://docs.microsoft.com/azure/architecture/reference-architectures/hybrid-networking/)'leri, abonelikler arasında bağlantı için [hub ve-ışınsal](https://docs.microsoft.com/azure/architecture/reference-architectures/hybrid-networking/hub-spoke) ve hatta yalnızca şirket içi bağlantı sağlayan [zorlamalı yönlendirmeye](../vpn-gateway/vpn-gateway-forced-tunneling-rm.md) bakmak yaygın bir yoldur. DevTest Labs, laboratuvarda yeni sanal makineler oluştururken kullanımını etkinleştirmek için mevcut sanal ağların laboratuvara bağlanmasını sağlar. 
* **Sanal makinelere uzaktan erişim**: DevTest Labs 'de bulunan sanal makinelere uzaktan erişim için birçok seçenek vardır. En kolay genel IP 'Leri veya paylaşılan genel IP 'Leri kullanmak, bu [Ayarlar laboratuvarda kullanılabilir](devtest-lab-shared-ip.md). Bu seçenekler yeterli değilse, bir uzaktan erişim ağ geçidi kullanmak [DevTest Labs kurumsal başvuru mimarisinde](devtest-lab-reference-architecture.md) gösterildiği gibi bir seçenektir ve [DevTest Labs Uzak Masaüstü Ağ geçidi belgelerinde](configure-lab-remote-desktop-gateway.md)daha da açıklanacaktır. Kuruluşlar, DevTest laboratuvarlarını şirket içi ağına bağlamak için Express Route veya siteden siteye VPN de kullanabilir. Bu, internet 'te hiçbir etkilenmesiz özel IP adreslerine bağlı olarak sanal makinelere doğrudan uzak masaüstü veya SSH bağlantıları sağlar. 
* **Izinleri işleme**: DevTest Labs 'de yaygın olarak kullanılan iki anahtar izinleri ["Owner" ve "Lab user"](devtest-lab-add-devtest-user.md)dır. Laboratuvardaki her erişim düzeyiyle güvenilir olacak şekilde, DevTest Labs 'nin genel kullanıma sunulmadan önce karar vermeniz önemlidir. Ortak bir model, laboratuar sahibi ve takım üyeleri olarak laboratuvar kullanıcısı olarak bütçe sahibidir (örneğin, ekip lideri). Bu, bütçesinden sorumlu kişinin (ekip lideri), takımın bütçe dahilinde kalmasını sağlamak üzere ilke ayarlarını ayarlamasını sağlar.  

## <a name="completing-the-proof-of-concept"></a>Kavram kanıtı Tamamlanıyor 

Beklenen dersleri bir kez ele alındıktan sonra pilot 'ın kaymasını ve tamamlanmasının zamanı. Bu, kullanıcılardan geri bildirim toplama zamanının, Pilot uygulamanın başarılı olup olmadığını belirleme ve kuruluşun kurumsal olarak DevTest Labs 'den bir ölçek toplaması üzerinde ilerleyeceğini belirleme kararı. Ayrıca, ölçek toplaması genelinde tutarlılık sağlamak için DevTest Labs ve ilişkili kaynakların dağıtımını otomatik hale getirmek için de harika bir süredir. 

## <a name="next-steps"></a>Sonraki Adımlar 

* [DevTest Labs kurumsal belgeleri](devtest-lab-guidance-prescriptive-adoption.md)
* [Bir kuruluş için başvuru mimarisi](devtest-lab-reference-architecture.md)
* [DevTest Labs dağıtımınızı ölçeklendirme](devtest-lab-guidance-orchestrate-implementation.md)
* [Azure DevTest Labs uygulanmasını düzenleme](devtest-lab-guidance-orchestrate-implementation.md)
