---
title: Konsept kanıtı sunun - Azure DevTest Labs | Microsoft Dokümanlar
description: Azure DevTest Laboratuvarlarının kurumsal bir ortama başarıyla dahil edilebilmeleri için kavram kanıtı nasıl sunabileceğinizi öğrenin.
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
ms.openlocfilehash: ca843213760cee60799568a6f33059c2bd91c835
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75643296"
---
# <a name="deliver-a-proof-of-concept"></a>Konsept inkanıtı sunun 

Azure DevTest Labs için önemli senaryolardan biri, bulutta geliştirme ve test ortamlarını etkinleştirmektir. Örneklere şunlar dahildir:

* Bulutta geliştirici masaüstü oluşturma.
* Sınama için ortamları yapılandırma.
* Sanal makinelere ve diğer Azure kaynaklarına erişimi etkinleştirme.
* Geliştiricilerin öğrenmesi ve denemesi için bir kum havuzu alanı ayarlama.

DevTest Labs ayrıca, kurumsal güvenlik, mevzuat ve uyumluluk ilkelerine uygun geliştiricilere "self-servis Azure" sağlaması için işletmeyi güçlendirmek için ilke korkulukları ve maliyet denetimleri de sağlar. 

Her kuruluş, Azure DevTest Laboratuvarlarının ortamlarına başarıyla dahil edilebildiği için farklı gereksinimlere sahiptir. Bu makalede, işletmelerin kavram başarılı bir kanıt sağlamak için tamamlaması gereken en yaygın adımları açıklanır. Kavramın kanıtı, başarılı bir uçtan uca dağıtıma doğru atılmış ilk adımdır. 

## <a name="getting-started"></a>Başlarken 

Konseptin kanıtını sunmaya başlamak için. Azure ve DevTest Labs hakkında bilgi edinmek için biraz zaman harcamak önemlidir.  Bazı başlangıç kaynakları şunlardır: 

* [Azure portalını anlama](https://azure.microsoft.com/features/azure-portal/)
* [DevTest Laboratuvarlarının Temelleri](devtest-lab-overview.md)
* [DevTest Labs desteklenen senaryolar](devtest-lab-guidance-get-started.md)
* [DevTest Labs kurumsal dokümantasyon](devtest-lab-guidance-prescriptive-adoption.md)
* [Azure ağına giriş](../virtual-network/virtual-networks-overview.md)

## <a name="prerequisites"></a>Ön koşullar 

DevTest Labs ile bir pilotu veya konsept ini başarıyla tamamlamak için birkaç ön koşul vardır: 

* **Azure aboneliği**: Kuruluşların azure erişimine olanak tanıyan mevcut bir [Kurumsal Sözleşme'ye](https://azure.microsoft.com/pricing/purchase-options/enterprise-agreement/) sahiptir ve DevTest Labs için varolan veya yeni bir aboneliği kullanabilirler. Alternatif olarak, işletmeler pilot uygulama sırasında [Visual Studio aboneliği](https://azure.microsoft.com/pricing/member-offers/visual-studio-subscriptions/) kullanabilir (ücretsiz Azure kredilerinden yararlanır). Bu seçeneklerden hiçbiri yoksa, bir kuruluş ücretsiz bir [Azure hesabı](https://azure.microsoft.com/free/search/?&OCID=AID719825_SEM_g4lyBqgB&lnkd=Bing_Azure_Brand&msclkid=ecc4275a31b61375749e7a5322c20de8&dclid=CMGW5-m78-ICFaLt4QodmUwGtQ)oluşturabilir ve kullanabilir. Bir Kurumsal Sözleşme varsa, Enterprise [Dev/Test aboneliği](https://azure.microsoft.com/offers/ms-azr-0148p/) kullanmak, Windows 10/Windows 8.1 istemci işletim sistemlerine ve geliştirme ve test iş yüklerine yönelik indirimli fiyatlara erişmek için harika bir seçenektir. 
* **Azure Etkin Dizin kiracı**: Kullanıcıları yönetmeyi etkinleştirmek için (örneğin, kullanıcı ekleme veya laboratuvar sahipleri ekleme), bu kullanıcıların pilot için Azure aboneliğinde kullanılan [Azure Etkin Dizin kiracısının](https://azure.microsoft.com/services/active-directory/) bir parçası olması gerekir. Genellikle işletmeler, kullanıcıların şirket içi kimliklerini bulutta kullanmalarını sağlamak için [hibrit kimlik](../active-directory/hybrid/whatis-hybrid-identity.md) ayarlar, ancak bu DevTest Labs pilotu için gerekli değildir. 

## <a name="scoping-of-the-pilot"></a>Pilotun kapsam alamı 

Uygulamaya başlamadan önce bir pilot planlamak önemlidir. Kaynakların süresiz olarak kalmayacağını önceden bilmek, pilot kullanıcı için uygun beklentileri belirler. 

> [!IMPORTANT]
> Pilotu net bir şekilde kapsama ve beklentileri ön plana çıkarabilmenin önemini yeterince vurgulayamayız.

Pilot yola başlamadan önce bu anahtar soruları cevaplayın: 

* Ne öğrenmek istiyorsunuz ve pilot için başarı nasıl görünüyor? 
* Pilotta hangi iş yükleri veya senaryolar ele alınacak? Pilotun hızla incelenebilmesini ve tamamlanabilmesini sağlamak için yalnızca küçük bir küme tanımlamak önemlidir. 
* Laboratuvarda hangi kaynaklar mevcut olmalıdır? Örneğin: özel görüntüler, pazar görüntüleri, ilkeler, ağ topolojisi. 
* Deneyimi doğrulamak için pilot uygulamada yer alacak kullanıcılar ve takımlar kimlerdir?  
* Pilotun süresi nedir? İki hafta veya bir ay gibi planlanan kapsama iyi hizalanmış bir zaman dilimi seçin. 
* Pilot tamamlandıktan sonra, pilot sırasında kullanılan ayrılan kaynaklarne ne olacak? Pilot kaynaklarını atmayı planlıyor musunuz? Şöyle düşünebilirsiniz:
   
   "Pilot un sonundaki sanal makineleri ve laboratuvarları çöpe atmayı planlayabilirsek, pilot için tek bir abonelik kurabilir ve ölçek lendirme sorularını paralel olarak çözerken tüm çalışmalarımızı orada yapabiliriz." 

Pilot "mükemmel" yapmak için genel bir eğilim var, bu yüzden aynı şekilde hizmet şirkette dışarı haddelenmiş sonra son durum ne olacağını temsil eder. Bu yanlış bir varsayım. "Mükemmel"e ne kadar yaklaşırsanız, pilot avede başlamadan *önce* o kadar çok tamamlamanız gerekir. Pilotun amacı, ölçekleme ve son hizmeti verme konusunda doğru kararları vermektir. 

Pilot uygulamanın odak noktası, Azure DevTest Labs'ın işletmeniz için doğru hizmet olup olmadığı sorusunu yanıtlamak için gerekli en az iş yükünü ve bağımlılıkları seçmek olmalıdır. Hızlı ve temiz bir başarı elde etmeye yardımcı olmak için en az bağımlılıkla en basit iş yükünü seçmenizi öneririz. Bu mümkün değilse, pilot aşamadaki başarının ölçeklendirme aşamasında çoğaltılabilmesi için olası karmaşıklıkları ortaya çıkaran en temsili iş yükünü seçin. 

Aşağıdaki örnek, kavramın iyi kapsamlı bir kanıt göstermektedir. 

## <a name="example-proof-of-concept-plan"></a>Örnek: kavram kanıtı planı 

Bu bölümde, DevTest Labs için pilot kavramının bir kanıtı nın belgesini kapsama almak için kullanılacak bir örnek gösterilmektedir. 

> [!TIP]
> Projenizi başarısızlığa göre ayarlama olasılığını en aza indirmek için, bu bölümde açıklanan örneği atlamamanızı öneririz. 

### <a name="overview"></a>Genel Bakış 

Satıcıların şirket ağından izole bir ortam olarak kullanmaları için DevTest Labs'ı temel alan Azure'da yeni bir ortam geliştirmeyi planlıyoruz. Çözümün gereksinimleri karşılayıp karşılamadığını belirlemek için, uçtan uca çözümü doğrulamak için bir kavram kanıtı geliştireceğiz. Deneyimi denemek ve doğrulamak için birkaç satıcı dahil ettik. 

### <a name="outcomes"></a>Sonuç -ları 

Bir kavram kanıtı inşa ederken, biz ilk sonuçlar (ne elde etmeye çalışıyoruz) odaklanmak. Kavramın kanıtı sonunda, bekliyoruz: 

* Azure'da yalıtılmış bir ortama erişmek için satıcıların Azure Etkin Dizini'ndeki (Azure AD) konuk hesaplarını kullanmaları için uçtan uca çalışan bir çözüm. Çevre, üretken olmaları için gerekli kaynaklara sahiptir. 
* Daha geniş ölçekli kullanımı ve benimsenmesini etkileyen olası engelleme sorunları numaralandırılır ve anlaşılır.
* Kavram kanıtı geliştirme katılan bireyler tüm kod iyi bir anlayışa sahip. Onlar da ilgili teminat anlamak ve daha geniş benimsenmesi eminiz.

### <a name="open-questions-and-prerequisites"></a>Açık sorular ve ön koşullar 

* Bu proje için kullanabileceğimiz bir abonelik oluşturduk mu? 
* Azure AD kiracımız ve Azure AD ile ilgili sorular için kimler inyardımı ve rehberliği sağlayabileceğini tanımlayan bir Azure AD yöneticisimiz var mı? 
* Projede çalışan bireyler için işbirliği yapacak bir yerimiz var mı? 

   * Kaynak kodu ve komut dosyaları (Azure Deposu gibi) 
   * Belgeler (Microsoft Teams veya SharePoint gibi)  
   * Konuşmalar (Microsoft Teams gibi) 
   * Çalışma öğeleri (Azure Panoları gibi) 
* Satıcılar için gerekli kaynaklar nelerdir? Bu, hem sanal makinelerde hem de gerekli diğer sunucularda ağda kullanılabilen uygulamaları içerir. 
* Sanal makineler Azure'daki bir etki alanına katılacak mı? Öyleyse, bu Azure Active Directory Etki Alanı Hizmetleri (Azure AD DS) veya başka bir şey olacak mı? 
* Konseptkanıtının hedefi olacak ekibi veya satıcıları belirledik mi? Çevrenin müşterileri kim olacak?
* Konseptin kanıtı için hangi Azure bölgesini kullanacağız? 
* IaaS (VMs) dışında devTest Labs aracılığıyla satıcıların kullanmasına izin verilen hizmetlerin bir listesi var mı? 
* Laboratuvarkullanarak satıcıları/kullanıcıları nasıl eğitmeyi planlıyoruz? 

### <a name="components-of-the-proof-of-concept-solution"></a>Kavram kanıtı çözümünün bileşenleri 

Çözümün aşağıdaki bileşenlere sahip olmasını bekliyoruz: 

* Çeşitli satıcı ekipleri Azure'da bir dizi laboratuvar kullanır.
* Laboratuarlar gereksinimleri destekleyen bir ağ altyapısına bağlıdır.
* Satıcılar, Azure AD ve rol atamaları aracılığıyla laboratuvarlara erişebilir.
* Satıcıların kaynaklarına başarıyla bağlanmanın bir yolu var. Özellikle, siteden siteye VPN, ortak IP adresleri olmadan sanal makinelere doğrudan erişmesini sağlar.
* Bir dizi yapı, satıcıların sanal makinelerde ihtiyaç duyduğu gerekli yazılımı kapsar.

## <a name="additional-planning-and-design-decisions"></a>Ek planlama ve tasarım kararları 

Tam bir DevTest Labs çözümü yayınlamadan önce, bazı önemli planlama ve tasarım kararları vermek zorunda. Kavram kanıtı üzerinde çalışma deneyimi bu kararları almanıza yardımcı olabilir. Daha fazla göz içerir: 

* **Abonelik topolojisi**: Azure'daki kaynaklar için kurumsal düzeydeki [gereksinimler, tek bir abonelikteki kullanılabilir kotaların](https://docs.microsoft.com/azure/azure-resource-manager/management/azure-subscription-service-limits)ötesine uzanabilir. Bu, ilk abonelik sınırlarını artırmak için birden çok Azure aboneliği ve/veya hizmet isteği gerektirir. Kaynakların abonelikler arasında nasıl dağıtılacayacaklarına önceden karar vermek önemlidir. Kaynakları daha sonra başka bir aboneye taşımak zor olduğundan, değerli kaynaklardan biri [abonelik karar kılavuzudur.](https://docs.microsoft.com/azure/architecture/cloud-adoption/decision-guides/subscriptions/) Örneğin, bir laboratuvar oluşturulduktan sonra başka bir aboneye taşınamaz.  
* **Ağ topolojisi**: DevTest Labs'ın otomatik olarak oluşturduğu [varsayılan ağ altyapısı,](../app-service/networking-features.md) kurumsal kullanıcılar için gereksinimleri ve kısıtlamaları karşılamak için yeterli olmayabilir. [Azure ExpressRoute'a bağlı sanal ağları](https://docs.microsoft.com/azure/architecture/reference-architectures/hybrid-networking/), abonelikler arasında bağlantı için hub ve [spoke'yi](https://docs.microsoft.com/azure/architecture/reference-architectures/hybrid-networking/hub-spoke) ve hatta yalnızca şirket içi bağlantı sağlamak için [zorunlu yönlendirmeyi](../vpn-gateway/vpn-gateway-forced-tunneling-rm.md) görmek yaygındır. DevTest Labs, laboratuvarda yeni sanal makineler oluştururken bunların kullanımını etkinleştirmek için mevcut sanal ağların laboratuvara bağlanmasına olanak tanır. 
* **Sanal makinelerin uzaktan erişimi**: DevTest Labs'da bulunan sanal makinelere uzaktan erişmek için birçok seçenek vardır. En kolayı, ortak IP'leri veya paylaşılan genel IP'leri kullanmaktır. Bunlar [laboratuarda bulunan ayarlardır.](devtest-lab-shared-ip.md) Bu seçenekler yeterli değilse, uzaktan erişim ağ geçidi kullanmak da bir seçenektir. Bu seçenek [DevTest Labs kurumsal başvuru mimarisinde](devtest-lab-reference-architecture.md) gösterilir ve [DevTest Labs uzak masaüstü ağ geçidi belgelerinde](configure-lab-remote-desktop-gateway.md)daha ayrıntılı olarak açıklanmıştır. İşletmeler, laboratuvarlarını şirket içi ağlarına bağlamak için ExpressRoute veya siteden siteye VPN de kullanabilir. Bu seçenek, internete maruz kalmadan özel IP adreslerine göre sanal makinelere doğrudan uzak masaüstü veya SSH bağlantıları sağlar. 
* **İşleme izinleri**: DevTest Labs'da yaygın olarak kullanılan iki temel izin [Sahibi ve Laboratuvar Kullanıcısıdır.](devtest-lab-add-devtest-user.md) DevTest Labs'ı geniş bir şekilde kullanıma almadan önce laboratuvardaki her erişim düzeyine kimin emanet olacağına karar vermek önemlidir. Ortak bir model, laboratuvar sahibi ve laboratuvar kullanıcıları olarak ekip üyeleri olarak bütçe sahibidir (örneğin, takım lideri). Bu model, bütçeden sorumlu kişinin (takım müşteri adayının) ilke ayarlarını ayarlamasını ve ekibi bütçe içinde tutmasını sağlar.  

## <a name="completing-the-proof-of-concept"></a>Kavramın kanıtını tamamlama 

Beklenen öğrenmeler tamamlandıktan sonra, pilot tamamlamak için zamanı. Bu, kullanıcılardan geri bildirim toplama, pilotun başarılı olup olmadığını belirleme ve kuruluşun kuruluştaki DevTest Labs'ın ölçekli bir lansmanında ilerleyip ilerleyeceğine karar verme zamanıdır. Ayrıca, ölçek dağıtımı boyunca tutarlılık sağlamak için DevTest Labs ve ilişkili kaynakların dağıtım otomatikleştirilmesi ni de göz önünde bulundurmak için harika bir zaman. 

## <a name="next-steps"></a>Sonraki adımlar 

* [DevTest Labs kurumsal dokümantasyon](devtest-lab-guidance-prescriptive-adoption.md)
* [Bir kuruluş için referans mimarisi](devtest-lab-reference-architecture.md)
* [DevTest Labs dağıtımınızı ölçekleme](devtest-lab-guidance-orchestrate-implementation.md)
* [Azure DevTest Labs'ın uygulanmasını düzenleme](devtest-lab-guidance-orchestrate-implementation.md)
