---
title: Dağıtım planı - Azure Active Directory | Microsoft Docs
description: Birçok Azure Active Directory özellikleri dağıtmak nasıl uçtan uca yönergeler.
services: active-directory
author: msaburnley
manager: daveba
ms.service: active-directory
ms.subservice: fundamentals
ms.workload: identity
ms.topic: conceptual
ms.date: 02/08/2019
ms.author: ajburnle
ms.custom: it-pro, seodec18
ms.collection: M365-identity-device-management
ms.openlocfilehash: a19c78efb7d81dcdcd3c221f58d4e4cc7be5ccd1
ms.sourcegitcommit: 85b3973b104111f536dc5eccf8026749084d8789
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/01/2019
ms.locfileid: "68722185"
---
# <a name="azure-active-directory-deployment-plans"></a>Azure Active Directory dağıtım planları
Azure Active Directory (AD) özelliklerini dağıtma konusunda ayrıntılı bir rehbere mi ihtiyacınız var? Aşağıdaki dağıtım planları yaygın Azure AD özelliklerinin başarıyla kullanıma alınması için gerekli olan iş değeri, planlama noktaları, tasarım ve operasyon yordamlarını anlatmaktadır. 

Belgelerin içinde e-posta şablonlarını, sistem mimarisi diyagramlarını, yaygın test çalışmalarını ve daha fazlasını bulacaksınız. 

Belgeler hakkındaki geri bildirimlerinizi bekliyoruz. Belgeler hakkındaki fikirlerinizi paylaşmak için bu kısa [anketi](https://aka.ms/deploymentplanfeedback) yanıtlayın. 

## <a name="include-the-right-stakeholders"></a>Doğru paydaşları dahil et

Dağıtım planlamasına yeni bir özellik için Başlarken, kuruluşunuzda önemli paydaşlar eklemek önemlidir. Aşağıdaki rollerin her birini karşılayan kişiyi veya kişileri tanımlamanızı ve belgeleyeceğini ve projede katılımlarını belirlemek için bunlarla çalışmayı öneririz.  

Roller aşağıdaki gibi bulunabilir 

|Role |Açıklama |
|-|-|
|Son Kullanıcı|Özelliği uygulanacak olan temsili Kullanıcı grubu. Genellikle bir pilot programdaki değişikliklere önizleme gösterir.
|BT Destek Yöneticisi|Bu değişikliğin bir yardım masası perspektifinden desteklenebilirliği hakkında giriş sağlayabilen kuruluş temsilcisini destekler.  
|Kimlik mimarı veya Azure genel Yöneticisi|Bu değişikliğin kuruluşunuzdaki çekirdek kimlik yönetimi altyapısına nasıl hizalandığını tanımlamaya yönelik kimlik yönetimi ekibi temsilcisi.|
|Uygulama Iş sahibi |Erişim yönetimi de içerebilen, etkilenen uygulamaların genel iş sahibidir.  Ayrıca, son kullanıcının perspektifinden bu değişikliğin kullanıcı deneyiminde ve yararlılığı üzerinde giriş sağlayabilir.
|Güvenlik sahibi|Güvenlik ekibinden, planın kuruluşunuzun güvenlik gereksinimlerini karşıladığını kapatan bir temsilci.|
|Uyumluluk Yöneticisi|Kuruluşunuzda kurumsal, sektör veya kamu gereksinimleriyle uyumluluk sağlamaktan sorumlu kişi.|

**Katılma düzeyleri şunları içerebilir:**

- Proje planı ve sonucu uygulamak için **R**esponerişilebilir 

- Proje planının ve sonucunun pproval 'i 

- **C**ontributor-proje planı ve sonucu 

- Proje planı ve sonucu **oluşturdum**
 
## <a name="deployment-plans"></a>Dağıtım planları



|Senaryo |Açıklama |
|-|-|
|[Multi-Factor Authentication](../authentication/howto-mfa-getstarted.md)|Azure Multi-Factor Authentication (MFA) Microsoft'un iki adımlı doğrulama çözümüdür. Yönetici onaylı kimlik doğrulama yöntemlerini kullanan Azure MFA, oturum açma sürecini karmaşık hale getirmeden verilerinize ve uygulamalarınıza erişimin güvenli hale getirilmesine yardımcı olur.|
|[Koşullu erişim](https://aka.ms/deploymentplans/ca)|Koşullu erişimle, koşullara göre bulut uygulamalarınıza kimlerin erişebileceği otomatik erişim denetimi kararları uygulayabilirsiniz.|
|[Self servis parola sıfırlama](https://aka.ms/deploymentplans/sspr)|Self servis parola sıfırlama, kullanıcılarınızın istediği yerden ve istediği zaman yönetici müdahalesi olmadan parolalarını sıfırlamasına yardımcı olur.|
|[Privileged Identity Management](https://aka.ms/deploymentplans/pim)|Azure AD Privileged Identity Management (PıM), Azure AD, Azure kaynakları ve diğer Microsoft Online Services genelinde ayrıcalıklı yönetim rollerini yönetmenize yardımcı olur. PıM, tam zamanında erişim, onay iş akışları isteme ve tam olarak tümleşik erişim incelemeleri gibi çözümler sağlayarak ayrıcalıklı rollerin kötü amaçlı etkinliklerini belirlemek, açmak ve gerçek zamanlı olarak engellemek için.|
|[Çoklu oturum açma](https://aka.ms/deploymentplans/sso)|Çoklu oturum açma, bir kez oturum açarak ve tek bir kullanıcı hesabı kullanarak işinizi yapmak için gerekli tüm uygulamalara ve kaynaklara erişmenize yardımcı olur. Oturum açtıktan sonra Microsoft Office'ten SalesForce ve Box uygulamalarına yeniden kimlik doğrulamasından (parola yazma gibi) geçmeden erişebilirsiniz.|
|[Sorunsuz çoklu oturum açma](https://aka.ms/SeamlessSSODPDownload)|Azure Active Directory Sorunsuz Çoklu Oturum Açma (Azure AD Sorunsuz SSO) özelliği, kurumsal ağınıza bağlı kuruluş cihazlarını kullanan kullanıcıların otomatik olarak oturum açmasını sağlar. Bu özelliği açtığınızda kullanıcılarınızın Azure AD oturumu açmak için parolalarını ve hatta kullanıcı adlarını yazmalarına gerek kalmaz. Bu özellik kullanıcılarınızın şirket içi ek bileşenlere ihtiyaç duymadan bulut tabanlı uygulamalarınıza kolayca erişmesini sağlar.|
|[Erişim Paneli](https://aka.ms/AccessPanelDPDownload)|Kullanıcılarınızın bulmak ve tüm uygulamalara erişmek için basit bir hub'ı sunar. Yeni uygulamalar ve gruplar için erişim isteme imkanını gibi Self Servis özellikler ile daha üretken olmalarını sağlamak veya diğerleri adına bu kaynaklara erişimi yönetin.|
|[Parola Karması Eşitleme için ADFS](https://aka.ms/deploymentplans/adfs2phs)|Parola Karması Eşitleme özelliği ile kullanıcı parolalarının karmaları şirket içi Active Directory ile Azure AD arasında eşitlenerek kullanıcıların şirket içi Active Directory ile etkileşim kurmadan kimlik doğrulamasından geçmesi sağlanır.|
|[Doğrudan Geçiş Kimlik Doğrulaması için ADFS](https://aka.ms/deploymentplans/adfs2pta)|Azure AD Doğrudan Geçiş Kimlik Doğrulaması, kullanıcılarınızın aynı parolalarla hem şirket içinde hem de bulut tabanlı uygulamalarda oturum açmasına yardımcı olur. Bu özellik hatırlamaları gereken parola sayısını azaltarak kullanıcılarınıza daha iyi bir deneyim sağlar ve oturum açma bilgileri muhtemelen daha az unutulacağından BT yardım masası maliyetlerinizi de düşürür. Kullanıcılar Azure AD'de oturum açtığında bu özellik parolaları doğrudan şirket için Active Directory dizininizde doğrular.|
|[Azure AD Uygulama Ara Sunucusu](https://aka.ms/deploymentplans/appproxy)|Günümüzde çalışanlar her yerden, her zaman ve tüm cihazlardan çalışmak istemektedir. Kendi tabletlerinden, telefonlarından ve dizüstü bilgisayarlarından da iş yapma istekleri vardır. Çalışanlar ayrıca hem buluttaki SaaS uygulamalarına hem de şirket içi kurumsal uygulamalarına erişmek istemektedir. Şirket içi uygulamalara erişmek için bugüne kadar sanal özel ağlar (VPN) veya temiz bölgeler (DMZ) kullanılıyordu. Bunlar yalnızca karmaşık ve güvenliği zor sağlanan çözümler değil aynı zamanda kurulumu ve yönetimi yüksek maliyetli seçeneklerdir. Artık çok daha iyi bir seçenek var! - Azure AD Uygulama Ara Sunucusu|
|[Kullanıcı sağlama](https://aka.ms/UserProvisioningDPDownload)|Azure AD; Dropbox, Salesforce ve ServiceNow gibi bulut (SaaS) uygulamalarında kullanıcı oluşturma, bakım ve kaldırma adımlarını otomatikleştirmenize yardımcı olur.|
|[Workday odaklı gelen kullanıcı sağlama](https://aka.ms/WorkdayDeploymentPlan)|Workday temelli gelen kullanıcı sağlamayı Active Directory, devam eden kimlik yönetimi için bir temel oluşturur ve yetkili kimlik verilere dayanan iş süreçlerini kalitesini artırır. Bu özelliği kullanarak, sorunsuz bir şekilde çalışanları ve bağlı çalışanları kimlik yaşam döngüsü Birleştirici taşıyıcısı Leaver işlemler (örneğin, yeni işe, sonlandırma, aktarım) BT sağlama Eylemler (örneğin, oluşturma, etkinleştir, eşleyen kuralları yapılandırarak yönetebilirsiniz "Devre dışı bırakmak, hesapları silme).|
|[Raporlama ve Izleme](https://aka.ms/deploymentplans/reporting)| Azure AD raporlama ve izleme çözümünüzün tasarımı, yasal, güvenlik ve işletimsel gereksinimlerinize ek olarak mevcut ortamınız ve süreçlerinize bağlıdır. Bu makalede, çeşitli tasarım seçenekleri sunulmakta ve doğru dağıtım stratejisinde size rehberlik sunulmaktadır.|
