---
title: Azure AD'yi kullanarak uygulamalara erişimi yönetme | Microsoft Dokümanlar
description: Azure Etkin Dizininin kuruluşların her kullanıcının erişesahip olduğu uygulamaları belirtmesini nasıl sağladığını açıklar.
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
ms.openlocfilehash: 489b15423add03d69070bc32057af97396a85309
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79409075"
---
# <a name="managing-access-to-apps"></a>Uygulamalara erişimi yönetme

Bir uygulama kuruluşunuzun kimlik sistemine entegre edildikten sonra sürekli erişim yönetimi, kullanım değerlendirmesi ve raporlama zor olmaya devam eder. Çoğu durumda, BT Yöneticileri veya yardım masası, uygulamalarınız için erişimi yönetmede sürekli etkin bir rol üstlenecek. Bazen, atama genel veya tümen BT ekibi tarafından gerçekleştirilir. Genellikle, atama kararı, BT atama yapmadan önce onayını gerektiren, iş karar vericisine devredilmesi amaçlanır.  Diğer kuruluşlar, Rol Tabanlı Erişim Denetimi (RBAC) veya Öznitelik Tabanlı Erişim Denetimi (ABAC) gibi varolan bir otomatik kimlik ve erişim yönetim sistemiyle entegrasyona yatırım yapar. Hem entegrasyon hem de kural geliştirme özel ve pahalı olma eğilimindedir. Yönetim yaklaşımının izlenmesi veya raporlanması, kendi ayrı, maliyetli ve karmaşık yatırımıdır.

## <a name="how-does-azure-active-directory-help"></a>Azure Active Directory nasıl yardımcı olur?

Azure AD, yapılandırılmış uygulamalar için kapsamlı erişim yönetimini destekleyerek kuruluşların otomatik, öznitelik tabanlı atamadan (ABAC veya RBAC senaryoları) delegasyon aþalari ile ve yönetici yönetimi. Azure AD ile, tek bir uygulama için birden çok yönetim modelini birleştirerek karmaşık ilkelere kolayca ulaşabilir ve hatta uygulamalar arasında yönetim kurallarını aynı hedef kitlelerle yeniden kullanabilirsiniz.

Azure AD ile kullanım ve atama raporlaması tam olarak entegre edilerek yöneticilerin atama durumu, atama hataları ve hatta kullanım hakkında kolayca rapor lamalarına olanak tanır.

### <a name="assigning-users-and-groups-to-an-app"></a>Kullanıcıları ve grupları bir uygulamaya atama

Azure AD'nin uygulama ataması iki birincil atama moduna odaklanır:

* **Bireysel atama** Dizin Global Administrator izinleri olan bir BT yöneticisi tek tek kullanıcı hesaplarını seçebilir ve onlara uygulamaya erişim izni verebilir.

* **Grup tabanlı atama (Azure AD Premium P1 veya P2 gerektirir)** Dizin Global Administrator izinleri olan bir BT yöneticisi uygulamaya bir grup atayabilir. Belirli kullanıcıların erişimi, uygulamaya erişmeye çalıştıkları sırada grubun üyesi olup olmadıklarına göre belirlenir. Başka bir deyişle, yönetici etkili bir atama kuralı belirten "atanan grubun herhangi bir geçerli üyesi uygulamaya erişimi vardır" oluşturabilirsiniz. Yöneticiler bu atama seçeneğini kullanarak, [öznitelik tabanlı dinamik gruplar,](../fundamentals/active-directory-groups-create-azure-portal.md)dış sistem grupları (örneğin, şirket içi Etkin Dizin veya İş Günü) veya Yönetici tarafından yönetilen veya self servis tarafından yönetilen gruplar dahil olmak üzere Azure AD grup yönetimi seçeneklerinden yararlanabilir. Tek bir grup, atama yakınlığına sahip uygulamaların atama kurallarını paylaşarak genel yönetim karmaşıklığını azaltabilmesini sağlayarak birden çok uygulamaya kolayca atanabilir. İç içe geçen grup üyeliklerinin şu anda uygulamalara grup tabanlı atama için desteklenmediğini unutmayın.

Bu iki atama modunu kullanarak, yöneticiler istenen atama yönetimi yaklaşımını elde edebilir.

### <a name="requiring-user-assignment-for-an-app"></a>Bir uygulama için kullanıcı ataması gerektirme

Belirli uygulama türlerinde, kullanıcıların uygulamaya [atanmasını gerekten](assign-user-or-group-access-portal.md#configure-an-application-to-require-user-assignment)seçeneğiniz vardır. Bunu yaparak, uygulamaya açıkça atadığınız kullanıcılar dışında herkesin oturum açmasını engellersiniz. Aşağıdaki uygulama türleri bu seçeneği destekler:

* SAML tabanlı kimlik doğrulama ile federe tek oturum açma (SSO) için yapılandırılan uygulamalar
* Azure Active Directory Pre-Authentication kullanan Uygulama Proxy uygulamaları
* Bir kullanıcı veya yönetici bu uygulamayı kabul ettikten sonra OAuth 2.0 / OpenID Connect Authentication kullanan Azure AD uygulama platformunda oluşturulmuş uygulamalar. Bazı kurumsal uygulamalar, oturum açma izninin kimler olduğu üzerinde ek denetim sağlar.

Kullanıcı ataması *gerekmediğinde,* atanamayan kullanıcılar uygulamayı Uygulamalarım erişim panelinde görmezler, ancak yine de uygulamanın kendisinde oturum açabilirler (SP tarafından başlatılan oturum açma olarak da bilinir) veya uygulamanın **Özellikler** sayfasındaki Kullanıcı **Erişim URL'sini** (IDP tarafından başlatılan oturum açma olarak da bilinir) kullanabilirler.

Bazı uygulamalarda, kullanıcı ataması gerektirme seçeneği uygulamanın özelliklerinde kullanılamaz. Bu gibi durumlarda, powershell'i kullanarak hizmet ilkesindeki RoleAssignmentRequired özelliğini ayarlayabilirsiniz.

### <a name="determining-the-user-experience-for-accessing-apps"></a>Uygulamalara erişmek için kullanıcı deneyimini belirleme

Azure AD, uygulamaları kuruluşunuzdaki son kullanıcılara dağıtmak için [birkaç özelleştirilebilir yol](end-user-experiences.md) sağlar:

* Azure AD Uygulamalarım erişim paneli
* Office 365 uygulama başlatıcısı
* Federe uygulamalara doğrudan oturum açma (service-pr)
* Birleştirilmiş, parola tabanlı veya var olan uygulamalara yönelik ayrıntılı bağlantılar

Bir kurumsal uygulamaya atanan kullanıcıların bu uygulamayı erişim panelinde ve Office 365 uygulama başlatıcısında görüp göremeyeceğini belirleyebilirsiniz.

## <a name="example-complex-application-assignment-with-azure-ad"></a>Örnek: Azure AD ile karmaşık uygulama ataması
Salesforce gibi bir uygulamayı düşünün. Birçok kuruluşta Salesforce öncelikle pazarlama ve satış ekipleri tarafından kullanılır. Genellikle, pazarlama ekibinin üyeleri Salesforce'a son derece ayrıcalıklı erişime sahipken, satış ekibinin üyeleri sınırlı erişime sahiptir. Çoğu durumda, geniş bir bilgi çalışanı nüfusu uygulamaya erişimi kısıtlamıştır. Bu kuralların istisnaları işleri karmaşıklaştırır. Genellikle pazarlama veya satış liderliği ekiplerinin bir kullanıcıya erişim hakkı vermek veya rollerini bu genel kurallardan bağımsız olarak değiştirmek için bir ayrıcalıktır.

Azure AD ile Salesforce gibi uygulamalar tek oturum açma (SSO) ve otomatik sağlama için önceden yapılandırılabilir. Uygulama yapılandırıldıktan sonra, Yönetici uygun grupları oluşturmak ve atamak için tek seferlik eylemi gerçekleştirebilir. Bu örnekte, bir yönetici aşağıdaki atamaları yürütebilir:

* [Dinamik gruplar,](../fundamentals/active-directory-groups-create-azure-portal.md) departman veya rol gibi öznitelikleri kullanarak pazarlama ve satış ekiplerinin tüm üyelerini otomatik olarak temsil edecek şekilde tanımlanabilir:
  
  * Pazarlama gruplarının tüm üyeleri Salesforce'taki "pazarlama" rolüne atanacak
  * Satış ekibi gruplarının tüm üyeleri Salesforce'taki "satış" rolüne atanacak. Başka bir ayrıntılandırma, farklı Salesforce rollerine atanan bölgesel satış ekiplerini temsil eden birden çok grup kullanabilir.

* Özel durum mekanizmasını etkinleştirmek için, her rol için bir self servis grubu oluşturulabilir. Örneğin, "Salesforce pazarlama özel durumu" grubu self servis grubu olarak oluşturulabilir. Grup Salesforce pazarlama rolüne atanabilir ve pazarlama liderlik ekibi sahibi yapılabilir. Pazarlama liderlik ekibinin üyeleri kullanıcıları ekleyebilir veya kaldırabilir, bir katılım ilkesi belirleyebilir veya bireysel kullanıcıların katılma isteklerini onaylayabilir veya reddedebilir. Bu mekanizma sahipleri veya üyeleri için özel eğitim gerektirmeyen bir bilgi işçisi uygun deneyim yoluyla desteklenir.

Bu durumda, atanan tüm kullanıcılar, farklı gruplara eklendikleri için, rol atamaları Salesforce'ta güncelleştirilmeye yardımcı olur. Kullanıcılar, Microsoft uygulama erişim paneli, Office web istemcileri ve hatta kuruluş Salesforce giriş sayfalarında gezinerek Salesforce'u keşfedebilir ve bu erişime erişebilir. Yöneticiler, Azure AD raporlamasını kullanarak kullanım ve atama durumunu kolayca görüntüleyebilir.

Yöneticiler, belirli roller için erişim ilkeleri ayarlamak için [Azure AD Koşullu Erişim'i](../active-directory-conditional-access-azure-portal.md) kullanabilir. Bu ilkeler, erişimin şirket ortamı dışında izin verilip verilmeyeceğini ve hatta çeşitli durumlarda erişim sağlamak için Çok Faktörlü Kimlik Doğrulama veya aygıt gereksinimlerini içerebilir.

## <a name="access-to-microsoft-applications"></a>Microsoft uygulamalarına erişim

Microsoft Uygulamaları (Office 365 Exchange, SharePoint, Yammer, vb.) 3.

Bir kullanıcının Microsoft tarafından yayımlanan bir uygulamaya erişebilmek için üç ana yolu vardır.

- Office 365 veya diğer ücretli paketlerindeki uygulamalarda, kullanıcılara **lisans ataması** yoluyla doğrudan kullanıcı hesabına veya grup tabanlı lisans atama özelliğimizi kullanan bir grup aracılığıyla erişim hakkı verilir.
- Microsoft'un veya üçüncü bir tarafın herkesin kullanması için serbestçe yayımlayan uygulamalarda, kullanıcılara [kullanıcı onayı](configure-user-consent.md)ile erişim izni verilebilir. Bu, azure AD Work veya School hesaplarıyla uygulamada oturum açtıkları ve uygulamaların hesaplarındaki sınırlı veri kümesine erişmesine izin verdikleri anlamına gelir.
- Microsoft veya üçüncü bir tarafın herkesin kullanması için serbestçe yayımlettiği uygulamalarda, kullanıcılara [yönetici onayı](manage-consent-requests.md)ile erişim izni de verilebilir. Bu, bir yöneticinin uygulamanın kuruluştaki herkes tarafından kullanılabileceğini belirlediği anlamına gelir, bu nedenle başvuruda Global Administrator hesabıyla oturum açabilir ve kuruluştaki herkese erişim izni verir.

Bazı uygulamalar bu yöntemleri birleştirir. Örneğin, bazı Microsoft uygulamaları Bir Office 365 aboneliğinin bir parçasıdır, ancak yine de onay gerektirir.

Kullanıcılar Office 365 uygulamalarına Office 365 portalları aracılığıyla erişebilir. Dizinizin Kullanıcı **ayarlarında** [Office 365 görünürlük geçişini](hide-application-from-user-portal.md) içeren Uygulamalarımın erişim panelinde Office 365 uygulamalarını gösterebilir veya gizleyebilirsiniz. 

Kurumsal uygulamalarda olduğu gibi, kullanıcıları Azure portalı üzerinden veya portal seçeneği yoksa PowerShell'i kullanarak belirli Microsoft uygulamalarına [atayabilirsiniz.](assign-user-or-group-access-portal.md)

## <a name="next-steps"></a>Sonraki adımlar
* [Koşullu Erişim ile uygulamaları koruma](../active-directory-conditional-access-azure-portal.md)
* [Self servis grup yönetimi/SSAA](../users-groups-roles/groups-self-service-management.md)
