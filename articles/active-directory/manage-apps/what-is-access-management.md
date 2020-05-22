---
title: Azure AD kullanarak uygulamalara erişimi yönetme | Microsoft Docs
description: Azure Active Directory kuruluşların her kullanıcının erişimi olan uygulamaları nasıl belirtmesini sağladığını açıklar.
services: active-directory
documentationcenter: ''
author: msmimart
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/16/2017
ms.author: mimart
ms.collection: M365-identity-device-management
ms.openlocfilehash: 0a694c9f6fc9d3a0b7e73c1565501915aa1470bc
ms.sourcegitcommit: 493b27fbfd7917c3823a1e4c313d07331d1b732f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/21/2020
ms.locfileid: "83739834"
---
# <a name="managing-access-to-apps"></a>Uygulamalara erişimi yönetme

Devam eden erişim yönetimi, kullanım değerlendirmesi ve raporlama, bir uygulama kuruluşunuzun kimlik sistemiyle tümleştirildiğinde bir zorluk yapmaya devam eder. Çoğu durumda BT yöneticileri veya yardım masası, uygulamalarınıza erişimi yönetirken devam eden bir etkin rol almak zorunda olur. Bazen, atama genel veya bir BT ekibi tarafından gerçekleştirilir. Genellikle, atama kararı iş kararı Oluşturucu için temsilci olarak atanmak üzere tasarlanmıştır ve atamayı yapmadan önce onayını gerekli hale getirir.  Diğer kuruluşlar, rol tabanlı Access Control (RBAC) veya öznitelik tabanlı Access Control (ATZM) gibi mevcut bir otomatik kimlik ve erişim yönetimi sistemiyle tümleştirmeyi yatırım yapar. Tümleştirme ve kural geliştirmenin her ikisi de özel ve pahalı olma eğilimindedir. Yönetim yaklaşımına göre izleme veya raporlama kendi ayrı, maliyetli ve karmaşık yatırımdır.

## <a name="how-does-azure-active-directory-help"></a>Nasıl yardım Azure Active Directory?

Azure AD, yapılandırılan uygulamalar için kapsamlı erişim yönetimini destekleyerek, kuruluşların, yetkilendirme ve yönetici yönetimi dahil olmak üzere otomatik, öznitelik tabanlı atama (ATZYA da RBAC senaryolarından) arasında doğru erişim ilkelerini kolayca elde etmelerini sağlar. Azure AD ile kolayca karmaşık ilkeler elde edebilir, tek bir uygulama için birden çok yönetim modeli birleştirerek aynı hedef kitlelere sahip uygulamalarda yönetim kurallarını da yeniden kullanabilirsiniz.

Azure AD ile kullanım ve atama raporlaması, yöneticilerin atama durumu, atama hataları ve hatta kullanımı kolayca rapor vermesine olanak tanıyan tam olarak tümleşiktir.

### <a name="assigning-users-and-groups-to-an-app"></a>Bir uygulamaya Kullanıcı ve Grup atama

Azure AD 'nin uygulama ataması iki birincil atama moduna odaklanır:

* **Bireysel atama** Dizin genel Yöneticisi izinlerine sahip bir BT Yöneticisi, bireysel kullanıcı hesapları seçebilir ve uygulamaya erişim izni verebilir.

* **Grup tabanlı atama (Azure AD Premium P1 veya P2 gerekir)** Dizin genel Yöneticisi izinlerine sahip bir BT Yöneticisi, uygulamaya bir grup atayabilir. Belirli kullanıcıların erişimi, uygulamaya erişmeye çalıştıkları sırada grubun üyesi olup olmadıkları belirlenir. Diğer bir deyişle, yönetici etkin bir şekilde "atanmış grubun herhangi bir üyesinin uygulamaya erişimi vardır" belirten bir atama kuralı oluşturabilir. Yöneticiler, bu atama seçeneğini kullanarak, [öznitelik tabanlı dinamik gruplar](../fundamentals/active-directory-groups-create-azure-portal.md), dış sistem grupları (örneğin, şirket içi Active Directory veya Workday) ya da yönetici tarafından yönetilen ya da self servis tarafından yönetilen gruplar dahil olmak üzere Azure AD Grup Yönetimi seçeneklerinden faydalanabilir. Tek bir grup birden çok uygulamaya kolayca atanabilir, böylece atama benzeşimine sahip uygulamaların atama kurallarını paylaştığından emin olabilir ve bu da genel yönetim karmaşıklığını azaltır. İç içe geçmiş grup üyelikleri Şu anda uygulamalara grup tabanlı atama için desteklenmediğini unutmayın.

Yöneticiler, bu iki atama modunu kullanarak, istenen herhangi bir atama yönetimi yaklaşımını elde edebilir.

### <a name="requiring-user-assignment-for-an-app"></a>Uygulama için Kullanıcı Ataması gerektirme

Belirli uygulama türlerinde, [kullanıcıların uygulamaya atanmasını isteme](assign-user-or-group-access-portal.md#configure-an-application-to-require-user-assignment)seçeneğiniz vardır. Bunu yaptığınızda, açıkça uygulamaya atadığınız kullanıcılar hariç herkesin oturum açmasını önlemektir. Aşağıdaki uygulama türleri bu seçeneği destekler:

* SAML tabanlı kimlik doğrulaması ile federe çoklu oturum açma (SSO) için yapılandırılmış uygulamalar
* Azure Active Directory ön kimlik doğrulaması kullanan uygulama proxy uygulamaları
* Bir kullanıcı veya yönetici bu uygulamaya alındıktan sonra OAuth 2,0/OpenID Connect kimlik doğrulaması kullanan Azure AD uygulama platformunda oluşturulan uygulamalar. Belirli kurumsal uygulamalar, oturum açmaya izin verilen kişiler üzerinde ek denetim sağlar.

Kullanıcı Ataması *gerekli olmadığında*, atanmamış kullanıcılar uygulamayı uygulamalarımın erişim panelinden görmez, ancak uygulamanın kendisinde oturum AÇABILIRLER (SP tarafından başlatılan oturum açma olarak da bilinir) veya uygulamanın **Özellikler** sayfasında (IDP tarafından başlatılan oturum açma olarak da BILINIR) **Kullanıcı erişim URL** 'sini kullanabilirler.

Bazı uygulamalarda, Kullanıcı Ataması gerektirme seçeneği uygulamanın özelliklerinde kullanılamaz. Bu durumlarda, hizmet sorumlusu üzerinde Approleatamamentrequired özelliğini ayarlamak için PowerShell kullanabilirsiniz.

### <a name="determining-the-user-experience-for-accessing-apps"></a>Uygulamalara erişmek için Kullanıcı deneyimini belirleme

Azure AD, kuruluşunuzdaki son kullanıcılara [uygulama dağıtmak için çeşitli özelleştirilebilir yollar](end-user-experiences.md) sağlar:

* Azure AD My Apps erişim paneli
* Office 365 uygulama başlatıcısı
* Federasyon uygulamalarında doğrudan oturum açma (Service-PR)
* Birleştirilmiş, parola tabanlı veya var olan uygulamalara yönelik ayrıntılı bağlantılar

Bir kurumsal uygulamaya atanan kullanıcıların bu dosyayı erişim panelinde ve Office 365 uygulama başlatıcısı 'nda göremeyeceğini belirleyebilirsiniz.

## <a name="example-complex-application-assignment-with-azure-ad"></a>Örnek: Azure AD ile karmaşık uygulama ataması
Salesforce gibi bir uygulamayı düşünün. Birçok kuruluşta Salesforce öncelikle pazarlama ve satış ekipleri tarafından kullanılır. Genellikle, pazarlama ekibinin üyelerinin Salesforce erişimine son derece ayrıcalıklı erişimi vardır, ancak satış ekibinin üyeleri sınırlı erişime sahip olur. Birçok durumda, büyük bir bilgi çalışanı popülasyonu uygulamaya erişimi kısıtlamıştır. Bu kuralların özel durumları önemli bir şekilde karmaşıklaştırır. Genellikle pazarlama veya satış liderlik ekiplerinin, Kullanıcı erişimi vermek veya rollerini bu genel kurallardan bağımsız olarak değiştirmesi için Prerogative.

Azure AD ile, Salesforce gibi uygulamalar çoklu oturum açma (SSO) ve otomatik sağlama için önceden yapılandırılmış olabilir. Uygulama yapılandırıldıktan sonra, yönetici uygun grupları oluşturup atamak için tek seferlik eylemi gerçekleştirebilir. Bu örnekte, bir yönetici aşağıdaki atamaları yürütebilir:

* [Dinamik Gruplar](../fundamentals/active-directory-groups-create-azure-portal.md) , departman veya rol gibi öznitelikleri kullanarak pazarlama ve satış ekiplerinin tüm üyelerini otomatik olarak temsil edecek şekilde tanımlanabilir:
  
  * Pazarlama gruplarının tüm üyeleri Salesforce 'ta "pazarlama" rolüne atanır
  * Satış ekibi gruplarının tüm üyeleri Salesforce 'ta "Sales" rolüne atanır. Daha fazla bir iyileştirme, farklı Salesforce rollerine atanan bölgesel satış ekiplerini temsil eden birden çok grup kullanabilir.

* Özel durum mekanizmasını etkinleştirmek için, her rol için bir self servis grubu oluşturulabilir. Örneğin, "Salesforce Marketing Exception" grubu bir self servis grubu olarak oluşturulabilir. Grup Salesforce pazarlama rolüne atanabilir ve pazarlama liderliği ekibi sahip olabilir. Pazarlama liderliği ekibinin üyeleri, kullanıcı ekleyebilir veya kaldırabilir, bir JOIN ilkesi ayarlayabilir veya hatta bireysel kullanıcıların katılması isteklerini onaylayabilir veya reddedebilir. Bu mekanizma, sahipler veya Üyeler için uzman eğitim gerektirmeyen bir bilgi çalışanı uygun deneyimi aracılığıyla desteklenir.

Bu durumda, atanan tüm kullanıcılar otomatik olarak Salesforce olarak sağlanır, çünkü farklı gruplara eklendikçe rol atamaları Salesforce 'ta güncelleştirilir. Kullanıcılar, Microsoft uygulama erişim paneli, Office Web istemcileri ve hatta kurumsal Salesforce oturum açma sayfasına giderek Salesforce 'ı bulabilir ve bunlara erişebilir. Yöneticiler, Azure AD raporlama kullanarak kullanımı ve atama durumunu kolayca görüntüleyebilir.

Yöneticiler, belirli roller için erişim ilkeleri ayarlamak üzere [Azure AD koşullu erişimi](../conditional-access/concept-conditional-access-users-groups.md) kullanabilir. Bu ilkeler, şirket ortamının dışında erişime izin verilip verilmeyeceğini, hatta çeşitli durumlarda erişim elde etmek için Multi-Factor Authentication veya cihaz gereksinimlerini içerebilir.

## <a name="access-to-microsoft-applications"></a>Microsoft uygulamalarına erişim

Microsoft uygulamaları (Office 365 Exchange, SharePoint, Yammer vb. gibi), üçüncü taraf SaaS uygulamalarından veya çoklu oturum açma için Azure AD ile tümleştirmiş olduğunuz diğer uygulamalardan farklı bir şekilde atanır ve yönetilir.

Bir kullanıcının Microsoft tarafından yayımlanan bir uygulamaya erişebileceği üç ana yol vardır.

- Office 365 veya diğer ücretli paketlerde bulunan uygulamalarda, kullanıcılara doğrudan kendi kullanıcı hesaplarına veya grup tabanlı lisans atama becerimizi kullanan bir **Grup aracılığıyla erişim** izni verilir.
- Microsoft veya 3. taraf herkes tarafından kullanılmak üzere ücretsiz olarak yayımlayan uygulamalar için [Kullanıcı izni](configure-user-consent.md)aracılığıyla kullanıcılara erişim izni verilebilir. Bu, Azure AD Iş veya okul hesabıyla uygulamada oturum açtıklarında ve hesabın hesabında sınırlı miktarda veri kümesine erişmesine izin veren anlamına gelir.
- Microsoft 'un veya bir üçüncü taraf tarafından herkes tarafından kullanılabilecek uygulamalar için, kullanıcılara [yönetici izni](manage-consent-requests.md)aracılığıyla da erişim izni verilebilir. Bu, bir yöneticinin uygulamayı kuruluştaki herkes tarafından kullanılabileceğini, böylece bir genel yönetici hesabıyla uygulamada oturum açmasını ve kuruluştaki herkese erişim vermesini belirlediği anlamına gelir.

Bazı uygulamalar bu yöntemleri birleştirir. Örneğin, bazı Microsoft uygulamaları Office 365 aboneliğinin bir parçasıdır, ancak yine de onay gerektirir.

Kullanıcılar Office 365 uygulamalarına Office 365 portalları aracılığıyla erişebilir. Ayrıca, dizininizin **Kullanıcı ayarlarındaki**Office [365 görünürlüğü](hide-application-from-user-portal.md) ' nı kullanarak, uygulamalarım erişim panelinde Office 365 uygulamalarını gösterebilir veya gizleyebilirsiniz. 

Kurumsal uygulamalarda olduğu gibi, kullanıcıları Azure portal aracılığıyla belirli Microsoft uygulamalarına [atayabilir](assign-user-or-group-access-portal.md) veya Portal seçeneği kullanılabilir değilse PowerShell 'i kullanabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
* [Koşullu erişimle uygulamaları koruma](../conditional-access/concept-conditional-access-cloud-apps.md)
* [Self Servis Grup Yönetimi/SSAA](../users-groups-roles/groups-self-service-management.md)
