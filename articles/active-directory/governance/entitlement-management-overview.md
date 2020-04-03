---
title: Yetkilendirme yönetimi nedir? - Azure REKLAM
description: Azure Active Directory yetkilendirme yönetimine genel bir bakış ve dahili ve harici kullanıcılar için gruplara, uygulamalara ve SharePoint Online sitelerine erişimi yönetmek için bunları nasıl kullanabileceğinize dikkat edin.
services: active-directory
documentationCenter: ''
author: barclayn
manager: daveba
editor: markwahl-msft
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.subservice: compliance
ms.date: 03/22/2020
ms.author: barclayn
ms.reviewer: markwahl-msft
ms.collection: M365-identity-device-management
ms.openlocfilehash: 65100e8584eba5c31edc2f9fd8c57ad8bd14c0d1
ms.sourcegitcommit: 3c318f6c2a46e0d062a725d88cc8eb2d3fa2f96a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/02/2020
ms.locfileid: "80582537"
---
# <a name="what-is-azure-ad-entitlement-management"></a>Azure AD yetkilendirme yönetimi nedir?

Azure Etkin Dizin (Azure AD) yetkilendirme yönetimi, kuruluşların erişim isteği iş akışlarını, erişim atamalarını, incelemeleri ve son kullanma tarihini otomatikleştirerek, kuruluşların kimlik ve erişim yaşam döngüsünü ölçekte yönetmesini sağlayan bir [kimlik yönetimi](identity-governance-overview.md) özelliğidir.

Kuruluşlardaki çalışanların işlerini gerçekleştirmek için çeşitli gruplara, uygulamalara ve sitelere erişmeleri gerekir. Gereksinimler değiştikçe bu erişimi yönetmek zordur - yeni uygulamalar eklenir veya kullanıcıların ek erişim haklarına ihtiyacı vardır.  Bu senaryo, dış kuruluşlarla işbirliği yaptığınızda daha karmaşık hale geliyor - diğer kuruluşta kimlerin kuruluşunuzun kaynaklarına erişmesi gerektiğini bilmiyor olabilirsiniz ve kuruluşunuzun hangi uygulamaları, grupları veya siteleri kullandığını bilmezler.

Azure AD yetkilendirme yönetimi, dahili kullanıcılar ve kuruluşunuzun dışındaki bu kaynaklara erişmeye ihtiyaç duyan kullanıcılar için gruplara, uygulamalara ve SharePoint Online sitelerine erişimi daha verimli bir şekilde yönetmenize yardımcı olabilir.

## <a name="why-use-entitlement-management"></a>Neden yetkilendirme yönetimini kullanıyorsun?

Kurumsal kuruluşlar genellikle çalışanların kaynaklara erişimini yönetirken zorluklarla karşı karşıya dır:

- Kullanıcılar hangi erişime sahip olmaları gerektiğini bilmeyebilirler ve olsalar bile, erişimlerini onaylamak için doğru kişileri bulmakta güçlük çekebilirler
- Kullanıcılar bir kaynağı bulup eriştinlerinde, iş amacıyla gerekenden daha uzun süre erişim sağlayabilirler

Bu sorunlar, tedarik zinciri kuruluşlarından veya diğer iş ortaklarından gelen dış kullanıcılar gibi başka bir kuruluştan erişime ihtiyaç duyan kullanıcılar için daha da karmaşık hale geliyor. Örnek:

- Hiç bir kişi, diğer kuruluşun dizinlerindeki belirli kişilerin tümünün onları davet edebilmesi için tanımayabilir.
- Bu kullanıcıları davet edebilseler bile, bu kuruluştaki hiç kimse kullanıcının tüm erişimini tutarlı bir şekilde yönetmeyi hatırlayabilir

Azure AD yetkilendirme yönetimi bu zorlukların üstesinden gelinmede yardımcı olabilir.  Müşterilerin Azure AD yetkilendirme yönetimini nasıl kullandıkları hakkında daha fazla bilgi edinmek için [Avanade örnek olay incelemesini](https://customers.microsoft.com/story/avanade-professional-services-azure-canada) ve [Centrica örnek olay incelemesini](https://customers.microsoft.com/story/757467-centrica-energy-azure)okuyabilirsiniz.  Bu video, yetkilendirme yönetimine ve değerine genel bir bakış sağlar:

>[!VIDEO https://www.youtube.com/embed/_Lss6bFrnQ8]

## <a name="what-can-i-do-with-entitlement-management"></a>Yetki yönetimi ile ne yapabilirim?

Yetkilendirme yönetiminin bazı yetenekleri şunlardır:

- Yönetici olmayanlara erişim paketleri oluşturma olanağını devredin. Bu erişim paketleri kullanıcıların isteyebileceği kaynakları içerir ve atanan erişim paketi yöneticileri, kullanıcıların talep edebileceği kurallarla, erişimlerini kimin onaylaması gerektiğine ve erişimin süresinin dolduğunda ilkeleri tanımlayabilir.
- Kullanıcıları erişim isteyebilecek bağlı kuruluşları seçin.  Dizininizde henüz yer olmayan bir kullanıcı erişim istediğinde ve onaylandığında, otomatik olarak dizininize davet edilir ve erişime atanmış olurlar.  Erişimlerinin süresi dolduğunda, başka erişim paketi atamaları yoksa, dizininizdeki B2B hesapları otomatik olarak kaldırılabilir.

[İlk erişim paketinizi oluşturmak için öğreticimizle](entitlement-management-access-package-first.md)yola çıkabilirsiniz. Ayrıca, sık [karşılaşılan senaryoları](entitlement-management-scenarios.md)okuyabilir veya video izleyebilirsiniz,

- [Kuruluşunuzda Azure AD yetkilendirme yönetimini dağıtma](https://www.youtube.com/watch?v=zaaKvaaYwI4)
- [Azure AD yetkilendirme yönetimini kullanımınızı izleme ve ölçeklendirme](https://www.youtube.com/watch?v=omtNJ7ySjS0)
- [Yetki yönetimi nde nasıl temsilci seçilir?](https://www.youtube.com/watch?v=Fmp1eBxzrqw)

## <a name="what-are-access-packages-and-what-resources-can-i-manage-with-them"></a>Erişim paketleri nelerdir ve hangi kaynakları onlarla yönetebilirim?

Yetkilendirme yönetimi, Azure AD'ye *bir erişim paketi*kavramını sunar. Erişim paketi, bir kullanıcının proje üzerinde çalışması veya görevini yerine getirmesi için gereken erişime sahip tüm kaynakların bir paketidir. Erişim paketleri, dahili çalışanlarınız ve kuruluşunuz dışındaki kullanıcılar için erişimi yönetmek için kullanılır.

 Yetkilendirme yönetimiyle kullanıcının erişimini yönetebileceğiniz kaynak türleri şunlardır:

- Azure AD güvenlik gruplarının üyeliği
- Office 365 Grupları ve Ekipleri Üyeliği
- SaaS uygulamaları ve federasyon/tek oturum açma ve/veya sağlamayı destekleyen özel entegre uygulamalar da dahil olmak üzere Azure AD kurumsal uygulamalarına atama
- SharePoint Online sitelerinin üyeliği

Azure AD güvenlik gruplarına veya Office 365 Gruplarına dayanan diğer kaynaklara erişimi de denetleyebilirsiniz.  Örnek:

- Bir erişim paketinde Azure AD güvenlik grubu kullanarak ve bu grup için [grup tabanlı lisanslama](../users-groups-roles/licensing-groups-assign.md) yı kyapılandırarak kullanıcılara Microsoft Office 365 için lisans verebilirsiniz
- Bir erişim paketinde bir Azure REKLAM güvenlik grubu kullanarak ve bu grup için bir [Azure rol ataması](../../role-based-access-control/role-assignments-portal.md) oluşturarak kullanıcılara Azure kaynaklarını yönetme izni verebilirsiniz

## <a name="how-do-i-control-who-gets-access"></a>Kimler tarafından erişilebildiğini nasıl kontrol edebilirim?

Bir erişim paketiyle, yönetici veya temsilci erişim paketi yöneticisi kaynakları (gruplar, uygulamalar ve siteler) ve kullanıcıların bu kaynaklar için ihtiyaç duydukları rolleri listeler.

Erişim paketleri de bir veya daha fazla *ilke*içerir. Bir ilke, pakete erişmek için atama için kuralları veya korkulukları tanımlar. Her ilke, yalnızca uygun kullanıcıların erişim talebinde bulunabilmesini, isteklerini onaylayanlar olduğundan ve bu kaynaklara erişimlerinin zaman sınırlı olduğundan ve yenilenmediyse sona ermesini sağlamak için kullanılabilir.

![Erişim paketi ve ilkeleri](./media/entitlement-management-overview/elm-overview-access-package.png)

Her ilke içinde, bir yönetici veya erişim paketi yöneticisi tanımlar

- Erişim isteğinde bulunabilecek olan mevcut kullanıcılar (genellikle çalışanlar veya zaten davet edilen konuklar) veya harici kullanıcıların ortak kuruluşları
- Onay süreci ve erişimi onaylayabilen veya reddedebilen kullanıcılar
- Atama nın süresi dolmadan önce kullanıcının erişim atamasının süresi onaylandıktan sonra

Aşağıdaki diyagram, yetkilendirme yönetimindeki farklı öğelerin bir örneğini gösterir. İki örnek erişim paketine sahip bir kataloğu gösterir.

- **Erişim paketi 1** kaynak olarak tek bir grup içerir. Erişim, dizideki bir kullanıcı kümesinin erişim istemesini sağlayan bir ilkeyle tanımlanır.
- **Erişim paketi 2,** kaynak olarak bir grup, bir uygulama ve bir SharePoint Online sitesini içerir. Erişim iki farklı ilke ile tanımlanır. İlk ilke, dizideki bir kullanıcı kümesinin erişim istemesini sağlar. İkinci ilke, harici dizindeki kullanıcıların erişim istemesine olanak tanır.

![Yetkilendirme yönetimine genel bakış](./media/entitlement-management-overview/elm-overview.png)

## <a name="when-should-i-use-access-packages"></a>Erişim paketlerini ne zaman kullanmalıyım?

Erişim paketleri, erişim ataması için diğer mekanizmalarıdeğiştirmez.  Bunlar en uygun gibi durumlarda şunlardır:

- Çalışanlar, belirli bir görev için zaman sınırlı erişime ihtiyaç duyar.  Örneğin, tüm çalışanların bir Exchange Online posta kutusuna sahip olmasını sağlamak için grup tabanlı lisanslama ve dinamik bir grup kullanabilir ve ardından çalışanların başka bir departmandaki departman kaynaklarını okumak gibi ek erişime ihtiyaç duyduğu durumlar için erişim paketlerini kullanabilirsiniz.
- Erişimin bir çalışanın yöneticisi veya diğer atanmış kişiler tarafından onaylanması gerekir.
- Departmanlar, BT katılımı olmadan kaynakları için kendi erişim ilkelerini yönetmek isterler.  
- Bir proje üzerinde işbirliği yapan iki veya daha fazla kuruluş, sonuç olarak, başka bir kuruluşun kaynaklarına erişmek için bir kuruluştan birden fazla kullanıcının Azure AD B2B aracılığıyla getirilmesi gerekir.

## <a name="how-do-i-delegate-access"></a>Erişimi nasıl devrasıyorum?

 Erişim paketleri katalog adı verilen *kapsayıcılarda*tanımlanır.  Tüm erişim paketleriniz için tek bir kataloğunuz olabilir veya bireyleri kendi kataloglarını oluşturacak ve sahip olacak şekilde belirleyebilirsiniz. Yönetici herhangi bir kataloğa kaynak ekleyebilir, ancak yönetici olmayan bir kişi yalnızca sahip oldukları kaynakları kataloğa ekleyebilir. Katalog sahibi, diğer kullanıcıları katalog ortağı veya erişim paketi yöneticisi olarak ekleyebilir.  Bu senaryolar makale [delegasyonunda ve Azure AD yetkilendirme yönetimindeki rollerde](entitlement-management-delegate.md)daha ayrıntılı olarak açıklanmıştır.

## <a name="summary-of-terminology"></a>Terminolojinin özeti

Yetkilendirme yönetimini ve belgelerini daha iyi anlamak için aşağıdaki terimler listesine başvurabilirsiniz.

| Sözleşme Dönemi | Açıklama |
| --- | --- |
| erişim paketi | Bir ekibin veya projenin ihtiyaç duyduğu ve ilkelerle yönetilen bir kaynak demeti. Bir erişim paketi her zaman bir katalogda bulunur. Kullanıcıların erişim isteğinde bulunmaları gereken bir senaryo için yeni bir erişim paketi oluşturursunuz.  |
| erişim isteği | Erişim paketindeki kaynaklara erişim isteği. İstek genellikle bir onay iş akışı geçer.  Onaylanırsa, isteyen kullanıcı bir erişim paketi ataması alır. |
| Atama | Bir erişim paketinin kullanıcıya atanması, kullanıcının bu erişim paketinin tüm kaynak rollerine sahip olmasını sağlar.  Erişim paketi atamalarının genellikle süresi dolmadan önce bir zaman sınırı vardır. |
| Katalog | İlgili kaynaklar ve erişim paketleri kapsayıcısı.  Kataloglar, yönetici olmayanların kendi erişim paketlerini oluşturabilmeleri için temsilci oluşturma için kullanılır. Katalog sahipleri, sahip oldukları kaynakları bir kataloğere ekleyebilir. |
| katalog oluşturucu | Yeni kataloglar oluşturmaya yetkili kullanıcılar topluluğu.  Katalog oluşturucusu olmaya yetkili yönetici olmayan bir kullanıcı yeni bir katalog oluşturduğunda, otomatik olarak bu kataloğun sahibi olur. |
| bağlı kuruluş | İlişkiniz olan harici bir Azure REKLAM dizini veya etki alanı. Bağlı bir kuruluştan gelen kullanıcıların bir ilkede erişim isteğinde bulunmalarına izin verildiği belirtilebilir. |
| ilke | Kullanıcıların nasıl erişebileceği, kimlerin onaylayacağı ve kullanıcıların bir atama aracılığıyla ne kadar süreyle erişebileceği gibi erişim yaşam döngüsünü tanımlayan kurallar kümesi. İlke, bir erişim paketine bağlıdır. Örneğin, bir erişim paketinin iki ilkesi olabilir : biri çalışanların erişim istemesi, ikincisi de harici kullanıcıların erişim istemesi için. |
| kaynak | Bir Office grubu, bir güvenlik grubu, uygulama veya SharePoint Online sitesi gibi bir varlık, kullanıcıya izin verilebilen bir rol. |
| kaynak dizini | Paylaşılabilen bir veya daha fazla kaynağı olan bir dizin. |
| kaynak rolü | Kaynakla ilişkili ve kaynak tarafından tanımlanan izinler topluluğu. Bir grubun iki rolü vardır : üye ve sahip. SharePoint siteleri genellikle 3 rolü vardır, ancak ek özel rolleri olabilir. Uygulamalar özel rollere sahip olabilir. |


## <a name="license-requirements"></a>Lisans gereksinimleri

[!INCLUDE [Azure AD Premium P2 license](../../../includes/active-directory-p2-license.md)]

Azure Almanya ve Azure China 21Vianet gibi özel bulutlar şu anda kullanılamıyor.

### <a name="how-many-licenses-must-you-have"></a>Kaç lisansın olmalı?

Dizininizin sahip olduğunuz en az olabildiğince fazla Azure AD Premium P2 lisansına sahip olduğundan emin olun:

- Erişim paketi talep **edebilen** üye kullanıcılar.
- Erişim paketi isteyen üye ve konuk kullanıcılar.
- Erişim paketi isteklerini onaylayan üye ve konuk kullanıcılar.
- Bir erişim paketine doğrudan atanan üye ve konuk kullanıcılar.

Azure AD Premium P2 lisansları aşağıdaki görevler için gerekli **değildir:**

- İlk katalogları kuran, paketlere ve ilkelere erişen ve yönetim görevlerini diğer kullanıcılara devreden Kullanıcılar için Global Administrator rolüne sahip kullanıcılar için lisans gerekmez.
- Katalog oluşturucu, katalog sahibi ve erişim paketi yöneticisi gibi yönetim görevlerinden devralınan kullanıcılar için lisans gerekmez.
- Erişim paketleri talep **edebilen,** ancak erişim paketi **istemeyen** konuklar için lisans gerekmez.

Üye kullanıcılarınız (çalışanlarınız) için satın aldığınız her ücretli Azure AD Premium P2 lisansı için, en fazla 5 konuk kullanıcıyı davet etmek için Azure AD B2B'yi kullanabilirsiniz. Bu konuk kullanıcılar Azure AD Premium P2 özelliklerini de kullanabilir. Daha fazla bilgi için Azure [AD B2B işbirliği lisanslama kılavuzuna](../b2b/licensing-guidance.md)bakın.

Lisanslar hakkında daha fazla bilgi için Azure [Etkin Dizin portalını kullanarak lisans atayın veya kaldırın'a](../fundamentals/license-users-groups.md)bakın.

### <a name="example-license-scenarios"></a>Örnek lisans senaryoları

Burada, sahip olduğunuz lisans sayısını belirlemenize yardımcı olacak bazı örnek lisans senaryoları verilmiştir.

| Senaryo | Hesaplama | Lisans sayısı |
| --- | --- | --- |
| Woodgrove Bank'taki Global Administrator ilk katalogları oluşturur ve yönetim görevlerini diğer 6 kullanıcıya devreder. İlkelerden biri, **Tüm çalışanların** (2.000 çalışanın) belirli bir erişim paketi kümesi isteyebileceğini belirtir. 150 çalışan erişim paketlerini talep etmektedir. | Erişim paketlerini talep **edebilen** 2.000 çalışan | 2.000 |
| Woodgrove Bank'taki Global Administrator ilk katalogları oluşturur ve yönetim görevlerini diğer 6 kullanıcıya devreder. İlkelerden biri, **Tüm çalışanların** (2.000 çalışanın) belirli bir erişim paketi kümesi isteyebileceğini belirtir. Başka bir ilke, **ortak Contoso'daki** (konuklar) Kullanıcılardan bazı kullanıcıların onaya tabi olarak aynı erişim paketlerini isteyebileceğini belirtir. Contoso'nun 30.000 kullanıcısı var. 150 çalışan erişim paketlerini, Contoso'dan ise 10.500 kullanıcı erişim talep etmektedir. | 1:5 oranını aşan Contoso'dan 2.000 çalışan + 500 konuk kullanıcı (10.500 - (2.000 * 5)) | 2,500 |

## <a name="next-steps"></a>Sonraki adımlar

- [Öğretici: İlk erişim paketinizi oluşturun](entitlement-management-access-package-first.md)
- [Genel senaryolar](entitlement-management-scenarios.md)
