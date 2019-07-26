---
title: Azure Active Directory Kimlik Koruması (YENİLENDİ) nedir? | Microsoft Docs
description: Azure Active Directory Kimlik Koruması (YENİLENDİ) nedir?
services: active-directory
ms.service: active-directory
ms.subservice: identity-protection
ms.topic: overview
ms.date: 10/03/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: sahandle
ms.collection: M365-identity-device-management
ms.openlocfilehash: a5d82f665cc837e5dbd2d7efbf16c7ae94e1b2b7
ms.sourcegitcommit: e9c866e9dad4588f3a361ca6e2888aeef208fc35
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/19/2019
ms.locfileid: "68333906"
---
# <a name="what-is-azure-active-directory-identity-protection-refreshed"></a>Azure Active Directory Kimlik Koruması (YENİLENDİ) nedir?

Kuruluşunuzun kimliklerini daha iyi korumak için kimlik koruması deneyimi yenilendi. Bu yenilenen deneyim şunları sağlar:

- ¬-Kullanıcı riski ve oturum açma riski ile en çok alakalı riski taşıyan yeniden tasarlanan yönetici deneyimi
- Filtreleme, sıralama ve akıllı indirmeler desteğiyle güçlü araştırmalar deneyimi
- Tehlikede olabilecek kullanıcılara yönelik çabalarınızı önceliklendirmenize yardımcı olmak için geliştirilmiş Kullanıcı riski hesaplaması
- Risk verilerine programlı erişim sağlamak için yeni API desteği
- Kullanıcılarınızı hemen korumanıza olanak sağlayan basitleştirilmiş yönetici geri bildirim işlemi
- Risk değerlendirmelerinin doğruluğunu artırmak için yeni denetimli makine öğrenimi

Güvenlik, Günümüzde kuruluşlar için önemli bir konudur. Saldırganlar bir kullanıcının kimliğini çalarak bir ortama erişim kazanıyorsa güvenlik ihlallerinin çoğu gerçekleşir. Yıllar boyunca saldırganlar, üçüncü taraf ihlallerinin kullanılmasıyla ve gelişmiş kimlik avı saldırıları kullanılarak giderek daha etkili hale gelmiştir. Saldırganlar hatta düşük ayrıcalıklı kullanıcı hesaplarına erişim elde etmekle, bu kullanıcıların, yan yana hareket aracılığıyla önemli şirket kaynaklarına erişmesini nispeten daha kolay hale gelir. 

Bu tehditlere yanıt vermek için Azure AD Kimlik Koruması şunları yapabilirsiniz: 

- Güvenliği aşılmış kimliklerin karşı önlem almasını önleme 
- Şüpheli etkinlik algılandığında riski otomatik olarak azaltma 
- Olası güvenlik açıklarını gidermek için riskli kullanıcıları ve oturum açma işlemlerini araştırın  
- Bir kullanıcının riski belirtilen bir eşiğe ulaştığında uyarı olun 

Azure AD Kimlik Koruması, bir kullanıcının kimliği tehlikeye atıldığında veya hesap sahibi dışında bir kişi tarafından oturum açmaya çalışırken otomatik olarak yanıt vermek üzere ilkeleri yapılandırmanızı sağlayan Azure Active Directory Premium P2 özelliğidir. IDENTITY. Bu ilkeler, Azure AD tarafından sunulan diğer koşullu erişim denetimlerine ek olarak, erişimi otomatik olarak engelleyebilir veya parola sıfırlama veya Multi-Factor Authentication zorlaması gibi risk azaltma eylemlerini başlatabilir. Ayrıca, kimlik koruması, kuruluşunuzda risk ve potansiyel tehditlerle ilgili daha ayrıntılı bilgi edinmek için izleme ve raporlama özellikleri sağlar. 

>[!VIDEO https://www.microsoft.com/en-us/videoplayer/embed/RWsS6Q]

## <a name="risk-events"></a>Risk olayları

Azure AD Kimlik Koruması aşağıdaki risk olaylarını algılar: 

| Risk olayı türü | Açıklama | Algılama türü |
| --- | --- | --- |
| Alışılmadık seyahat | Kullanıcının en son oturum açma işlemlerini temel alarak sıradan bir konumdan oturum açın. | Çevrimdışı |
| Anonim IP adresi | Anonim IP adresinden oturum açın (örneğin: Tor tarayıcısı, anonimleştirici VPN 'Ler). | Gerçek zamanlı |
| Bilinmeyen oturum açma özellikleri | Belirtilen kullanıcı için son gördüğdiğimiz özelliklerle oturum açın. | Gerçek zamanlı |
| Kötü amaçlı yazılım bağlantısı içeren IP adresi | Kötü amaçlı yazılımdan bağlantılı IP adresinden oturum açın | Çevrimdışı |
| Sızdırılan kimlik bilgileri | Bu risk olayı kullanıcının geçerli kimlik bilgilerinin sızdırıldığını gösterir | Çevrimdışı |

## <a name="types-of-risk"></a>Risk türleri 

Kimlik koruması iki tür riske dayalıdır:

- Oturum açma riski
- Kullanıcı riski

### <a name="sign-in-risk"></a>Oturum açma riski

Bir oturum açma riski, belirli bir kimlik doğrulama isteğinin kimlik sahibi tarafından yetkilendirilmemiş olma olasılığını temsil eder.

Oturum açma riskinin iki değerlendirmesi vardır: 

- **Oturum açma riski (gerçek zamanlı)** -oturum açma riski (gerçek zamanlı), oturum açma işlemi sırasında tetiklenecek tüm gerçek zamanlı algılamaları temel alır.  
- **Oturum açma riski (toplama)** -oturum açma riski (toplama), oturum açma riskinin toplam riskidir. Şunları göz önünde bulundurur:
   - Gerçek zamanlı algılamalar (yukarıda açıklanmıştır)
   - Çevrimdışı algılamalar (oturum açma işleminden sonra harekete geçirilir) 
   - Oturum açma özelliğinin diğer tüm özellikleri

### <a name="user-risk"></a>Kullanıcı riski

Bir Kullanıcı riski, belirli bir kimliğin tehlikeye düşmesi olasılığını temsil eder. 

Kullanıcı riski, kullanıcıyla ilişkili tüm riskler göz önünde bulundurularak hesaplanır:

- Tüm riskli oturum açma işlemleri
- Bir oturum açma ile bağlantılı olmayan tüm risk olayları
- Geçerli Kullanıcı riski
- Kullanıcı tarihi bitiş tarihine kadar tüm risk düzeltmeleri veya geri alma eylemleri

## <a name="how-identity-protection-detects-risk"></a>Kimlik koruması riski nasıl algılar  

Azure AD, oturum açma işlemleri sırasında gerçek zamanlı olarak algılanan her iki sinyali de ve kullanıcılarla oturum açma etkinlikleriyle ilgili gerçek zamanlı olmayan sinyalleri kullanarak, anomali ve şüpheli etkinlikleri algılamak için makine öğrenimini kullanır. Kimlik koruması, bu verileri kullanarak, bir kullanıcının kimliğini her doğruladığında gerçek zamanlı bir oturum açma riskini hesaplar ve her kullanıcı için genel bir Kullanıcı risk düzeyini belirler. Kimlik koruması, kimlik koruması Kullanıcı riskini ve oturum açma risk ilkelerini yapılandırarak bu risk algılamalarından otomatik olarak işlem yapmanıza olanak sağlar.  

Kimlik korumasının riski nasıl algıladığını anlamak için, iki önemli kavram vardır: Kullanıcı riski ve oturum açma riski. Oturum açma riski, belirli bir kimlik doğrulama isteğinin kimlik sahibi tarafından yetkilendirilmemiş olma olasılığını yansıtır. İki tür oturum açma riski vardır: gerçek zamanlı ve toplam. Belirtilen oturum açma girişimi (anonim IP adreslerinden oturum açma işlemleri gibi) sırasında gerçek zamanlı oturum açma riski tespit edilir. Toplam oturum açma riski, algılanan gerçek zamanlı oturum açma risklerinin yanı sıra kullanıcının oturum açma işlemleri ile ilişkili sonraki gerçek zamanlı olmayan risk olaylarını (imkansız seyahat gibi) toplamaktadır. Kullanıcı riski, hatalı aktörün belirli bir kimliğin güvenliğinin aşılmasına neden olan bir genel olasılığı yansıtır. Kullanıcı riski, belirli bir kullanıcı için aşağıdakiler de dahil olmak üzere tüm risk etkinliklerini içerir:

- Gerçek zamanlı oturum açma riski
- Sonraki oturum açma riski
- Riskli Kullanıcı algılamaları.   

 ![Akış](./media/overview-v2/01.png)

Kimlik koruması risk algılaması ve belirli bir oturum açma için yanıt için temel akış, yukarıdaki grafikte özetlenir.  

## <a name="common-scenarios"></a>Genel senaryolar 

Bir contoso çalışanı örneğine bakalım. 

1. Bir çalışan Exchange Online 'da Tor tarayıcısından oturum açmaya çalışır. Azure AD, oturum açma sırasında gerçek zamanlı risk olaylarını algılar. 
2. Azure AD, çalışanın, bir ortam oturumu risk düzeyini tetikleyerek adsız bir IP adresinden oturum açmasını algılar. 
3. Contoso BT Yöneticisi kimlik koruması oturum açma riski koşullu erişim ilkesini yapılandırdığından, çalışana bir MFA istemi tarafından istem yapılır. İlke, orta veya daha yüksek bir oturum açma riski için MFA gerektirir. 
4. Çalışan MFA isteğini geçirir ve Exchange Online 'a erişir ve Kullanıcı risk düzeyi değiştirilmez. 

Arka planda ne oldu? Tor tarayıcısından oturum açma girişimi, anonim IP adresi için Azure AD 'de gerçek zamanlı bir oturum açma riski tetikledi. Azure AD isteği işlendiği için, çalışanın oturum açma risk düzeyi eşiği (orta) karşıladığı için kimlik koruması 'nda yapılandırılan oturum açma risk ilkesini uyguladık. Çalışan daha önce MFA 'ya kaydolduğundan bu yana MFA Challenge 'a yanıt verebiliyordu. Azure AD 'ye gelen MFA sınamasını, büyük olasılıkla meşru kimlik sahibi olan ve Kullanıcı risk düzeyi artmayan bir şekilde geçebilme yeteneğidir. 

Ancak çalışana oturum açmaya çalışan bir ne olursa? 

1. Çalışanların kimlik bilgileriyle kötü niyetli bir aktör, IP adreslerini gizleyecek şekilde, Tor tarayıcısından Exchange Online hesabıyla oturum açmaya çalışır. 
2. Azure AD, oturum açma girişiminin adsız bir IP adresinden olduğunu algılar ve gerçek zamanlı bir oturum açma riskini tetikler. 
3. Contoso BT Yöneticisi, kimlik koruması oturum açma riskini, oturum açma riski orta ya da daha yüksek olduğunda MFA gerektirirken koşullu erişim ilkesini yapılandırdığından, kötü amaçlı aktör bir MFA istemi tarafından istenir. 
4. Kötü amaçlı aktör MFA sınamasını başarısız olur ve çalışanın Exchange Online hesabına erişemez. 
5. Başarısız MFA istemi, kaydedilecek bir risk olayını tetikledi ve gelecekteki oturum açma işlemleri için Kullanıcı riskini artırmalıdır. 

Kötü amaçlı bir aktör Sarah hesabına erişmeye çalışmış olduğuna göre, çalışanın bir sonraki oturum açma denemesinde neler olacağını görelim. 

1. Çalışan, Exchange Online 'da Outlook 'ta oturum açmaya çalışır. Azure AD, oturum açma sırasında gerçek zamanlı risk olaylarını ve önceki Kullanıcı riskini algılar. 
2. Azure AD, herhangi bir gerçek zamanlı oturum açma riskini algılamaz, ancak önceki senaryolarda geçen riskli etkinlik nedeniyle yüksek Kullanıcı riskini algılar.  
3. Contoso BT Yöneticisi kimlik koruması Kullanıcı risk ilkesini, yüksek riskli bir Kullanıcı oturum açtığında parola değişikliği gerektirecek şekilde yapılandırdığından, çalışana bir parola sıfırlama istemi tarafından istek verilir. 
4. Çalışan SSPR ve MFA için kayıtlı olduğundan, parolalarını başarıyla sıfırlarlar. 
5. Parolasını sıfırlayarak, çalışanın kimlik bilgileri artık güvenliği tehlikeye alınmaz ve kimlikleri güvenli bir duruma geri döner. 
6. Çalışanın önceki risk olayları çözümlenir ve Kullanıcı risk düzeyi, kimlik bilgilerinin güvenliğinin azaltılmasına yanıt olarak otomatik olarak sıfırlanır. 

## <a name="how-do-i-configure-identity-protection"></a>Nasıl yaparım? kimlik koruması yapılandırmak mı istiyorsunuz? 

Kimlik koruması 'nı kullanmaya başlamak için önce bir Kullanıcı risk ilkesi ve oturum açma risk ilkesi yapılandırın. Bu ilkeler yapılandırıldıktan ve bir test grubuna uygulandıktan sonra, ortamınızda kimlik koruması 'nın nasıl yanıt vereceğini anlamak için risk olaylarının benzetimini yapabilirsiniz. Aşağıdaki hızlı başlangıç kılavuzlarında, ortamınızda belirtilen ilkelerin ve testin nasıl ayarlanacağı hakkında bir adım adım sağlanmıştır. 

Kimlik koruması, dağıtımınız etrafında yönetim etkinliklerini dengelemek için Azure AD 'de üç rolü destekler: 

| Role | Yapılabilir | Yapılamıyor |
| --- | --- | --- |
| Genel yönetici | Kimlik koruması, yerleşik kimlik koruması ile tam erişim | |
| Güvenlik yöneticisi | Kimlik korumasına tam erişim | Kimlik koruması ekleme, bir kullanıcı için parolaları sıfırlama |
| Güvenlik okuyucusu | Kimlik korumasına salt okuma erişimi | Kimlik koruması ekleme, kullanıcıları düzeltme, ilkeleri yapılandırma, parolaları sıfırlama| 

Daha fazla bilgi için bkz. [Azure Active Directory yönetici rolleri atama](../users-groups-roles/directory-assign-admin-roles.md)
 
## <a name="licensing"></a>Lisanslama

>[!NOTE]
> Kimlik korumasının (yenilenmiş) genel önizlemesi sırasında, riskli kullanıcılar raporu ve riskli oturum açma bileşenleri raporuna yalnızca Azure AD Premium P2 müşterileri erişebilir.

| Özellik | Azure AD Premium P2 | Azure AD Premium P1 | Azure AD Temel/ücretsiz |
| --- | --- | --- | --- |
| Kullanıcı riski ilkesi | Evet | Hayır | Hayır |
| Oturum açma riski ilkesi | Evet | Hayır | Hayır |
| Riskli kullanıcılar raporu | Tam erişim | Sınırlı bilgi | Sınırlı bilgi |
| Riskli oturum açma işlemleri raporu | Tam erişim | Sınırlı bilgi | Sınırlı bilgi |
| MFA kayıt ilkesi | Evet | Hayır | Hayır |

## <a name="next-steps"></a>Sonraki adımlar 

Kimlik koruması 'nı kullanmaya başlamak için bkz. [oturum açma risk Ilkesini yapılandırma](quickstart-sign-in-risk-policy.md). 
