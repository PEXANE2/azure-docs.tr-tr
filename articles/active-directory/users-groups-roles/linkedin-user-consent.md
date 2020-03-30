---
title: LinkedIn veri paylaşımı ve izni - Azure Active Directory | Microsoft Dokümanlar
description: LinkedIn tümleştirmesi Azure Active Directory'deki Microsoft uygulamaları aracılığıyla verileri nasıl paylaştığını açıklar
services: active-directory
author: curtand
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: users-groups-roles
ms.topic: article
ms.date: 11/08/2019
ms.author: curtand
ms.reviewer: beengen
ms.custom: it-pro;seo-update-azuread-jan
ms.collection: M365-identity-device-management
ms.openlocfilehash: a21c986ccfe96bae5d341e0ba2ee6622d998d076
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74025387"
---
# <a name="linkedin-account-connections-data-sharing-and-consent"></a>LinkedIn hesap bağlantıları veri paylaşımı ve onayı

Active Directory (Azure AD) kuruluşunuzdaki kullanıcıların Microsoft çalışmalarını veya okul hesaplarını LinkedIn hesaplarıyla bağlamaya izin vermelerini sağlayabilirsiniz. Bir kullanıcı hesaplarını bağladıktan sonra, LinkedIn'den gelen bilgiler ve önemli noktalar bazı Microsoft uygulamalarında ve hizmetlerinde kullanılabilir. Kullanıcılar ayrıca LinkedIn'deki ağ deneyimlerinin Microsoft'tan gelen bilgilerle geliştirilmesini ve zenginleşmesini bekleyebilirler.

Microsoft uygulamaları ve hizmetlerinde LinkedIn bilgilerini görmek için kullanıcıların kendi Microsoft ve LinkedIn hesaplarını bağlamayı kabul etmeleri gerekir. Kullanıcılardan, Outlook, OneDrive veya SharePoint Online'daki bir profil kartında bir kişinin LinkedIn bilgilerini görmek için ilk kez tıkladıklarında hesaplarını bağlamaları istenir. LinkedIn hesap bağlantıları, kullanıcılarınızın bu deneyimi kabul edene ve hesaplarını bağlamasına kadar tam olarak etkinleştirilemez.

[!INCLUDE [active-directory-gdpr-note](../../../includes/gdpr-hybrid-note.md)]

## <a name="benefits-of-sharing-linkedin-information"></a>LinkedIn bilgilerini paylaşmanın yararları

Microsoft uygulamaları ve hizmetlerindeki LinkedIn bilgilerine erişim, kullanıcılarınızın kuruluşunuzun içinde ve dışında iş arkadaşları, müşteriler ve iş ortaklarıyla bağlantı kurmasını, etkileşimde bulunmasını ve onlarla profesyonel ilişkiler kurmasını kolaylaştırır. Yeni kullanıcılar iş arkadaşlarıyla bağlantı kurarak, onlar hakkında daha fazla bilgi edinerek ve daha fazla bilgiye kolayca erişerek daha hızlı hızlanabilir. Aşağıda, LinkedIn bilgilerinin Microsoft uygulamalarında profil kartında nasıl göründüğüne ilişkin bir örnek verilmiştir:

![Kuruluşunuzdaki LinkedIn tümleştirmesini etkinleştirme](./media/linkedin-user-consent/display-example.png)

## <a name="enable-and-announce-linkedin-integration"></a>LinkedIn entegrasyonunu etkinleştirme ve duyur

Kuruluşunuzun ayarını yönetmek için bir Azure Etkin Dizin Yöneticisi olmalısınız. Tüm kullanıcılar veya belirli bir kullanıcı kümesi için etkinleştirebilirsiniz.

1. Tümleştirmeyi etkinleştirmek veya devre dışı katmak [için, Azure REKLAM kuruluşunuz için LinkedIn tümleştirmesine Onay](linkedin-integration.md)adımlarını izleyin.
2. Kuruluşunuzdaki LinkedIn tümleştirmesini duyurduğunuzda, kullanıcılarınızı [Microsoft uygulamaları ve hizmetlerinde LinkedIn bilgileri](https://support.office.com/article/about-linkedin-information-and-features-in-microsoft-apps-and-services-dc81cc70-4d64-4755-9f1c-b9536e34d381)hakkında SSS'ye yönlendirin. Makale, LinkedIn bilgilerinin nerede görüldüğü, hesapların nasıl bağlanacağı ve daha fazlası hakkında bilgi sağlar.

## <a name="user-consent-for-data-access-in-microsoft-and-linkedin"></a>Microsoft ve LinkedIn'de veri erişimi için kullanıcı onayı

LinkedIn'den erişilen veriler Microsoft hizmetlerinde kalıcı olarak depolanmaz. Microsoft'tan erişilen veriler Kişiler dışında LinkedIn'de kalıcı olarak depolanmaz. Microsoft Kişileri, [kullanıcılar linkedin'den alınan kişileri silmede](https://www.linkedin.com/help/linkedin/answer/43377)açıklandığı gibi, kullanıcılar bunları kaldırana kadar LinkedIn'de depolanır.

Kullanıcılar hesaplarını bağladıklarında, LinkedIn'den gelen bilgiler ve öngörüler profil kartı gibi bazı Microsoft uygulamalarında kullanılabilir. Kullanıcılar ayrıca LinkedIn'deki ağ deneyimlerinin Microsoft'tan gelen bilgilerle geliştirilmesini ve zenginleşmesini bekleyebilirler.
Kuruluşunuzdaki kullanıcılar LinkedIn ve Microsoft iş veya okul hesaplarını bağladıklarında iki seçenekleri vardır:

* Verilere her iki hesaptan da erişilmesi için izin verin. Bu, Microsoft veya iş hesaplarının LinkedIn hesaplarından verilere erişmelerine ve [LinkedIn hesaplarının Microsoft çalışmalarından veya okul hesaplarından verilere erişmelerine](https://www.linkedin.com/help/linkedin/answer/84077)izin verdikleri anlamına gelir.
* Yalnızca LinkedIn verilerine Microsoft iş ve okul hesapları tarafından erişilmesi için izin verin.

Kullanıcılar hesapları nilerini kesebilir ve veri erişim izinlerini herhangi bir zamanda kaldırabilir ve kullanıcılar profillerinin Microsoft uygulamalarında görüntülenip görüntülenemeyeceği de dahil olmak üzere [kendi LinkedIn profillerinin nasıl görüntüleneceğini denetleyebilir.](https://www.linkedin.com/help/linkedin/answer/83)

### <a name="linkedin-account-data"></a>LinkedIn hesap verileri

Microsoft ve LinkedIn hesaplarınızı bağladığınızda, LinkedIn'in Microsoft'a aşağıdaki verileri sağlamasına izin verirsiniz:

* Profil verileri - LinkedIn kimliğini, iletişim bilgilerini ve [LinkedIn profilinizde](https://www.linkedin.com/help/linkedin/answer/15493)başkalarıyla paylaştığınız bilgileri içerir.
* İlgi alanları verileri - Takip ettiğiniz kişi ve konular, kurs grupları ve beğendiğiniz ve paylaştığınız içerik gibi LinkedIn'deki ilgi alanlarını içerir.
* Abonelik verileri - Bağlantılı uygulamalara ve hizmetlere aboneliklerin yanı sıra ilişkili verileri de içerir. 
* Bağlantılar verileri - [LinkedIn network](https://www.linkedin.com/help/linkedin/answer/110) 1.

LinkedIn'den erişilen veriler Microsoft hizmetlerinde kalıcı olarak depolanmaz. Microsoft'un kişisel verileri kullanımı hakkında daha fazla bilgi için [Microsoft Gizlilik Bildirimi'ne](https://privacy.microsoft.com/privacystatement/)bakın.

### <a name="microsoft-work-or-school-account-data"></a>Microsoft iş veya okul hesabı verileri

Microsoft ve LinkedIn hesaplarınızı bağladığınızda, Microsoft'un LinkedIn'e aşağıdaki verileri sağlamasına izin verirsiniz:

* Profil verileri - adınız, soyadınız, profil fotoğrafınız, e-posta adresiniz, yöneticiniz ve yönettiğiniz kişiler gibi bilgileri içerir.
* Takvim verileri - takvimlerinizdeki toplantıları, zamanlarını, konumlarını ve katılımcıların iletişim bilgilerini içerir. Gündem, içerik veya toplantı başlığı gibi toplantı yla ilgili bilgiler takvim verilerine dahil edilmez.
* İlgi alanları verileri - Cortana ve Bing for Business gibi Microsoft hizmetlerini kullanımınıza bağlı olarak hesabınızla ilişkili ilgi alanlarını içerir.
* Abonelikverileri - kuruluşunuz tarafından Office 365 gibi Microsoft uygulamaları ve hizmetlerine sağlanan abonelikleri içerir.
* Kişiler verileri - Sık sık iletişim kurduğunuz veya işbirliği yaptığınız kişilerin kişi bilgileri de dahil olmak üzere Outlook, Skype ve diğer Microsoft hesap hizmetlerindeki kişi listelerini içerir. Kişiler, bağlantılı ları önermek, kişileri düzenlemeye yardımcı olmak ve ilgili kişilerle ilgili güncelleştirmeleri göstermek gibi LinkedIn tarafından düzenli olarak alınır, depolanır ve kullanılır.

Microsoft'tan erişilen veriler Kişiler dışında LinkedIn'de kalıcı olarak depolanmaz. Microsoft Kişileri, kullanıcılar bunları kaldırana kadar LinkedIn'de depolanır. [LinkedIn'den alınan kişileri silme](https://www.linkedin.com/help/linkedin/answer/43377)hakkında daha fazla bilgi edinin.

LinkedIn'in kişisel verileri kullanımı hakkında daha fazla bilgi için [LinkedIn Gizlilik Politikası'na](https://www.linkedin.com/legal/privacy-policy)bakın. LinkedIn hizmetleri, veri aktarımı ve depolama için veriler Avrupa Birliği'nden Amerika Birleşik Devletleri'ne ve geri akış yapabilir ve gizliliğiniz [Avrupa Birliği veri aktarımlarında](https://www.linkedin.com/help/linkedin/answer/62533)açıklandığı şekilde korunur.

## <a name="next-steps"></a>Sonraki adımlar

* [İş veya okul hesabınızla Microsoft uygulamalarında LinkedIn](https://www.linkedin.com/help/linkedin/answer/84077)
