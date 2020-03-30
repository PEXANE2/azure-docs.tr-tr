---
title: Dağıtım planları - Azure Etkin Dizini | Microsoft Dokümanlar
description: Birçok Azure Etkin Dizin özelliğinin nasıl dağıtılanacaçık olduğu hakkında uçuca kılavuz.
services: active-directory
author: msaburnley
manager: daveba
ms.service: active-directory
ms.subservice: fundamentals
ms.workload: identity
ms.topic: conceptual
ms.date: 08/20/2019
ms.author: ajburnle
ms.custom: it-pro, seodec18
ms.collection: M365-identity-device-management
ms.openlocfilehash: 17e6708225262349d56c6e261895882e9c31677f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79262223"
---
# <a name="azure-active-directory-deployment-plans"></a>Azure Active Directory dağıtım planları
Azure Etkin Dizin (Azure AD) özelliklerini dağıtma konusunda uçuca kılavuz mu arıyorsunuz? Azure AD dağıtım planları, ortak Azure AD özelliklerini başarıyla dağıtmak için gereken iş değerini, planlama hususlarını ve operasyonel yordamları gözden geçirir.

Plan sayfalarından herhangi birinden, belgelerin güncel çevrimdışı sürümünü oluşturmak için tarayıcınızın Pdf'ye Yazdır özelliğini kullanın.
## <a name="include-the-right-stakeholders"></a>Doğru paydaşları dahil edin

Yeni bir yetenek için dağıtım planlamanıza başlarken, kuruluşunuzdaki önemli hissedarları da dahil etmek önemlidir. Aşağıdaki rollerin her birini yerine getiren kişi veya kişileri tanımlamanızı ve belgelemenizi ve projeye katılımlarını belirlemek için onlarla birlikte çalışmanızı öneririz.  

Roller aşağıdakileri içerebilir 

|Rol |Açıklama |
|-|-|
|Son kullanıcı|Yeteneğin uygulanacağı temsili bir kullanıcı grubu. Genellikle bir pilot programdaki değişiklikleri önizler.
|BT Destek Yöneticisi|Bt destek kuruluş temsilcisi, bu değişikliğin desteklenebilirliği konusunda yardım masası perspektifinden girdi sağlayabilir.  
|Kimlik Mimarı veya Azure Global Yöneticisi|Bu değişikliğin kuruluşunuzdaki temel kimlik yönetimi altyapısıyla nasıl uyumlu olduğunu tanımlamaktan sorumlu kimlik yönetimi ekibi temsilcisi.|
|Uygulama İş Sahibi |Etkilenen uygulamanın (lar) genel işletme sahibi, erişim yönetimi içerebilir.Ayrıca, son kullanıcı açısından bu değişikliğin kullanıcı deneyimi ve kullanışlılığı hakkında da bilgi sağlayabilir.
|Güvenlik Sahibi|Planın kuruluşunuzun güvenlik gereksinimlerini karşılayacağını kapatabilecek güvenlik ekibinden bir temsilci.|
|Uyumluluk Yöneticisi|Kuruluşunuzdaki kişi, kurumsal, endüstri veya resmi gereksinimlere uygunluğu sağlamaktan sorumludur.|

**Katılım düzeyleri şunları içerebilir:**

- **Proje**planı nın ve sonucun uygulanması için R esponsible 

- **A**Proje planı ve sonucunun bir pproval 

- **Proje**planı ve sonucuna C ontributor 

- **Proje**planı ve sonucu hakkında bilgi verdim


## <a name="best-practices-for-a-pilot"></a>Bir pilot için en iyi uygulamalar
Bir pilot herkes için bir yetenek açmadan önce küçük bir grup ile test etmenizi sağlar. Testinizin bir parçası olarak, kuruluşunuzdaki her kullanım örneğinin kapsamlı bir şekilde test edildiğinden emin olun. Bunu bir bütün olarak kuruluşunuza vermeden önce belirli bir pilot kullanıcı grubunu hedeflemek en iyisidir.

İlk dalganızda, BT'yi, kullanılabilirliği ve geri bildirimi test edip sağlayabilen diğer uygun kullanıcıları hedef alın. Bu geri bildirim, kullanıcılarınıza gönderdiğiniz iletişimleri ve talimatları daha da geliştirmek ve destek personelinizin görebileceği sorun türleri hakkında bilgi vermek için kullanılmalıdır. 

Daha büyük kullanıcı gruplarına rollout genişletilmesi hedeflenen grup (lar) kapsamı artırılarak yapılmalıdır. Bu, dinamik [grup üyeliği](https://docs.microsoft.com/azure/active-directory/users-groups-roles/groups-dynamic-membership)veya hedeflenen gruba kullanıcıları el ile ekleyerek yapılabilir.


## <a name="deploy-authentication"></a>Kimlik doğrulamayı dağıtma

| Özellik | Açıklama|
| -| -|
| [Multi-Factor Authentication](https://aka.ms/deploymentplans/mfa)| Azure Multi-Factor Authentication (MFA) Microsoft'un iki adımlı doğrulama çözümüdür. Azure MFA, yönetici onaylı kimlik doğrulama yöntemlerini kullanarak, basit bir oturum açma işlemi talebini karşılarken verilerinize ve uygulamalarınız için erişimi korumaya yardımcı olur. |
| [Koşullu Erişim](https://aka.ms/deploymentplans/ca)| Koşullu Erişim ile, koşullara bağlı olarak bulut uygulamalarınıza kimlerin erişebileceğine ilişkin otomatik erişim denetimi kararlarını uygulayabilirsiniz. |
| [Self servis parola sıfırlama](https://aka.ms/deploymentplans/sspr)| Self servis parola sıfırlama, kullanıcılarınızın yönetici müdahalesi olmadan parolalarını ne zaman ve nerede sıfırlamalarına yardımcı olur. |
| [Parolasız](https://aka.ms/deploymentplans/passwordless) | Kuruluşunuzdaki Microsoft Authenticator uygulamasını veya FIDO2 Güvenlik anahtarlarını kullanarak parolasız kimlik doğrulaması uygulayın |

## <a name="deploy-application-management"></a>Uygulama yönetimini dağıtma

| Özellik | Açıklama|
| -| - |
| [Çoklu oturum açma](https://aka.ms/deploymentplans/sso)| Tek oturum açma, kullanıcılarınızın yalnızca bir kez oturum açarak iş yapmaları için ihtiyaç duydukları uygulamalara ve kaynaklara erişmesine yardımcı olur. Oturum açtıktan sonra, ikinci kez kimlik bilgileri girmeleri gerekmeden Microsoft Office'ten SalesForce'tan Box'a ve dahili uygulamalara gidebilirler. |
| [Erişim paneli](https://aka.ms/deploymentplans/accesspanel)| Kullanıcılarınıza tüm uygulamalarını keşfetmek ve bunlara erişmek için basit bir merkez sunun. Uygulamalara ve gruplara erişim istemek veya başkaları adına kaynaklara erişimi yönetmek gibi self servis özellikleriyle daha üretken olmalarını sağlayabilirsiniz. |


## <a name="deploy-hybrid-scenarios"></a>Karma senaryoları dağıtma

| Özellik | Açıklama|
| -| -|
| [Parola Karması Eşitleme için ADFS](https://aka.ms/deploymentplans/adfs2phs)| Parola Karma Senkronizasyonu ile, kullanıcı parolalarının karmaları şirket içi Active Directory'den Azure AD'ye senkronize edilmiştir ve Azure AD'nin çevrimiçi Active Directory ile hiçbir etkileşimi olmayan kullanıcıları doğrulamasını sağlar |
| [Doğrudan Geçiş Kimlik Doğrulaması için ADFS](https://aka.ms/deploymentplans/adfs2pta)| Azure AD Pass-through Kimlik Doğrulaması, kullanıcılarınızın aynı parolaları kullanarak hem şirket içinde hem de bulut tabanlı uygulamalarda oturum açmalarına yardımcı olur. Bu özellik, kullanıcılara daha iyi bir deneyim sağlar - hatırlanması gereken bir şifre daha az - ve kullanıcıların oturum açma şeklini unutma olasılığı daha düşük olduğundan BT yardım masası maliyetlerini azaltır. Kullanıcılar Azure AD'de oturum açtığında bu özellik parolaları doğrudan şirket için Active Directory dizininizde doğrular. |
| [Azure AD Uygulama Ara Sunucusu](https://aka.ms/deploymentplans/appproxy)| Günümüzde çalışanlar her yerden, her zaman ve tüm cihazlardan çalışmak istemektedir. Buluttaki SaaS uygulamalarına ve şirket içi kurumsal uygulamalara erişmeleri gerekir. Azure AD Application proxy, pahalı ve karmaşık sanal özel ağlar (VPN'ler) veya arındırılmış bölgeler (DMZ'ler) olmadan bu sağlam erişimi sağlar. |
| [Sorunsuz çoklu oturum açma](../hybrid/how-to-connect-sso-quick-start.md)| Azure Active Directory Sorunsuz Çoklu Oturum Açma (Azure AD Sorunsuz SSO) özelliği, kurumsal ağınıza bağlı kuruluş cihazlarını kullanan kullanıcıların otomatik olarak oturum açmasını sağlar. Bu özellik sayesinde, kullanıcıların Azure AD'de oturum açabilmek için parolalarını yazmaları gerekmez ve genellikle kullanıcı adlarını girmeleri gerekmez. Bu özellik, yetkili kullanıcılara, şirket içinde ek bileşenlere ihtiyaç duymadan bulut tabanlı uygulamalarınıza kolay erişim sağlar. |

## <a name="deploy-user-provisioning"></a>Kullanıcı sağlama yı dağıtma

| Özellik | Açıklama|
| -| -|
| [Kullanıcı sağlama](https://aka.ms/deploymentplans/userprovisioning)| Azure AD; Dropbox, Salesforce ve ServiceNow gibi bulut (SaaS) uygulamalarında kullanıcı oluşturma, bakım ve kaldırma adımlarını otomatikleştirmenize yardımcı olur. |
| [Bulut İk kullanıcı sağlama](https://aka.ms/deploymentplans/cloudhr)| Active Directory'ye Bulut İk kullanıcı sağlama, sürekli kimlik yönetimi için bir temel oluşturur ve yetkili kimlik verilerine dayanan iş süreçlerinin kalitesini artırır. İş Günü veya Başarı Faktörleri gibi bulut İk ürününüzle bu özelliği kullanarak, Joiner-Mover-Leaver süreçlerini (Yeni İşe Alma, Sonlandırma, Sonlandırma, Sonlandırma, Aktarım) BT sağlama eylemlerine (Oluşturma, Etkinleştirme, Devre Dışı Etme gibi) |

## <a name="deploy-governance-and-reporting"></a>Yönetim ve raporlamayı dağıtma

| Özellik | Açıklama|
| -| -|
| [Privileged Identity Management](https://aka.ms/deploymentplans/pim)| Azure AD Ayrıcalıklı Kimlik Yönetimi (PIM), Azure AD, Azure kaynakları ve diğer Microsoft Çevrimiçi Hizmetleri'nde ayrıcalıklı yönetim rollerini yönetmenize yardımcı olur. PIM, ayrıcalıklı rollerin kötü amaçlı etkinliklerini gerçek zamanlı olarak tanımlayabilmeniz, ortaya çıkarabilmeniz ve önleyebilmeniz için tam zamanında erişim, istek onayı iş akışları ve tam entegre erişim incelemeleri gibi çözümler sunar. |
| [Raporlama ve İzleme](https://aka.ms/deploymentplans/reporting)| Azure AD raporlama ve izleme çözümünüzün tasarımı, yasal, güvenlik ve operasyonel gereksinimleriniz ile mevcut ortamınıza ve süreçlerinize bağlıdır. Bu makalede, çeşitli tasarım seçenekleri sunar ve doğru dağıtım stratejisi için size rehberlik eder. |
