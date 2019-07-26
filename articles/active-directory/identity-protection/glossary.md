---
title: Azure Active Directory Kimlik Koruması sözlüğü | Microsoft Docs
description: Azure Active Directory Kimlik Koruması sözlüğü
services: active-directory
ms.service: active-directory
ms.subservice: identity-protection
ms.topic: reference
ms.date: 01/15/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: sahandle
ms.collection: M365-identity-device-management
ms.openlocfilehash: 6751fe74bfd9b8a07aec2263582d9f1a4bc0b2ac
ms.sourcegitcommit: e9c866e9dad4588f3a361ca6e2888aeef208fc35
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/19/2019
ms.locfileid: "68333972"
---
# <a name="azure-active-directory-identity-protection-glossary"></a>Azure Active Directory Kimlik Koruması sözlüğü

### <a name="at-risk-user"></a>Risk altında (Kullanıcı)
Bir veya daha fazla etkin risk olayı olan bir kullanıcı. 

### <a name="atypical-sign-in-location"></a>Atipik oturum açma konumu
Belirli bir Kullanıcı, benzer kullanıcılar veya kiracı için tipik olmayan coğrafi bir konumdan oturum açma.

### <a name="azure-ad-identity-protection"></a>Azure AD Kimlik Koruması
Bir kuruluşun kimliklerini etkileyen risk olaylarına ve olası güvenlik açıklarına birleştirilmiş bir görünüm sağlayan Azure Active Directory güvenlik modülü.

### <a name="conditional-access"></a>Koşullu Erişim
Kaynaklara erişimi güvenli hale getirmek için bir ilke. Koşullu erişim kuralları Azure Active Directory depolanır ve kaynağa erişim izni vermeden önce Azure AD tarafından değerlendirilir.  Örnek kurallar, Kullanıcı konumuna, cihaz durumuna veya Kullanıcı kimlik doğrulama yöntemine göre erişimi kısıtlamayı içerir.

### <a name="credentials"></a>Kimlik Bilgileri
Yerel ve ağ kaynaklarına erişim kazanmak için kullanılan tanımlama ve kimlik kanıtı bilgilerini içeren bilgiler. Kimlik bilgileri örnekleri Kullanıcı adları ve parolalar, akıllı kartlar ve sertifikalardır.

### <a name="event"></a>Olay
Azure Active Directory bir etkinliğin kaydı.

### <a name="false-positive-risk-event"></a>False-pozitif (risk olayı)
Risk olayının araştırıldığına ve yanlış bir risk olayı olarak işaretlenmediğini belirten bir kimlik koruma kullanıcısı tarafından el ile ayarlanan risk olayı durumu.

### <a name="identity"></a>Kimlik
Parola veya sertifika gibi ölçütlere bağlı olarak, kimlik doğrulama yoluyla doğrulanması gereken bir kişi veya varlık.

### <a name="identity-risk-event"></a>Kimlik riski olayı
Kimlik koruması tarafından anormal olarak işaretlenen AAD olayı ve bir kimliğin tehlikeye girdiği anlamına gelebilir.

### <a name="ignored-risk-event"></a>Yoksayıldı (risk olayı)
Bir kimlik koruma kullanıcısı tarafından el ile ayarlanan bir risk olayı, bir düzeltme eylemi gerçekleştirmeden risk olayının kapatıldığını belirtir.

### <a name="impossible-travel-from-atypical-locations"></a>Atipik konumlardan mümkün olmayan seyahat
Aynı kullanıcı için iki oturum açma algılandığında tetiklenen bir risk olayı tespit edildiğinde, en az bir tane bir adet bir oturum açma konumundan olduğu ve oturum açma işlemleri arasındaki süre, bu konumlar arasında fiziksel olarak seyahat etmek için gereken en kısa süreden kısa olduğunda tetiklenir.  

### <a name="investigation"></a>Araştırma
Düzeltme ya da risk azaltma adımlarının gerekli olup olmadığına karar vermek, kimlik durumunu ve bu adımların nasıl tehlikede olduğunu anlamak ve riskli kimliğin nasıl çalıştığını anlamak için etkinlikleri, günlükleri ve bir risk olayı ile ilgili diğer bilgileri inceleme işlemi. kullanıldı.

### <a name="leaked-credentials"></a>Sızdırılan kimlik bilgileri
Geçerli Kullanıcı kimlik bilgileri (Kullanıcı adı ve parola), araştırmacılarımız tarafından koyu bir Web 'de herkese açık olarak bulunduğunda tetiklenen bir risk olayı.

### <a name="mitigation"></a>Risk azaltma
Bir saldırganın kimliği veya cihazı güvenli bir duruma geri yüklemeden güvenliği aşılmış bir kimlik veya cihazdan yararlanma yeteneğini sınırlamak veya ortadan kaldırmak için bir eylem. Risk azaltma, kimlik veya cihazla ilişkili önceki risk olaylarını çözümlemez.

### <a name="multi-factor-authentication"></a>Çok öğeli kimlik doğrulama
İki veya daha fazla kimlik doğrulama yöntemi gerektiren bir kimlik doğrulama yöntemi, örneğin bir sertifika, kullanıcının sahip olduğu bir şeyi içerebilir; Kullanıcı adları, parolalar veya geçiş ifadeleri gibi kullanıcıların bildiği bir şey; parmak izi gibi fiziksel öznitelikler; Kişisel imza gibi kişisel öznitelikleri.

### <a name="offline-detection"></a>Çevrimdışı algılama
Daha önce gerçekleşmiş bir olay için, olgudan sonra oturum açma girişimi gibi bir olay riskini algılama ve değerlendirmesi.

### <a name="policy-condition"></a>İlke koşulu
İlke içinde yer alan veya gruptan çıkarılan varlıkları (gruplar, kullanıcılar, uygulamalar, cihaz platformları, cihaz durumları, IP aralıkları, istemci türleri) tanımlayan bir güvenlik ilkesinin parçası.

### <a name="policy-rule"></a>İlke kuralı
Bir güvenlik ilkesinin, ilkeyi tetikleyecek koşulları ve ilke tetiklendiğinde gerçekleştirilecek eylemleri açıklayan bölümü.

### <a name="prevention"></a>Önleme
Bir kimlik veya cihazın kötüye kullanımı aracılığıyla kuruluşa zarar verme veya tehlikeye geçme durumunu önleme eylemi. Bir önleme eylemi, cihazın veya kimliğin güvenliğini sağlamaz ve önceki risk olaylarını çözmez.

### <a name="privileged-user"></a>Ayrıcalıklı (Kullanıcı)
Risk olayı sırasında olan bir Kullanıcı, bir genel yönetici, faturalandırma Yöneticisi, hizmet yöneticisi, Kullanıcı Yöneticisi ve parola Yöneticisi gibi Azure Active Directory bir veya daha fazla kaynak için kalıcı veya geçici yönetici izinlerine sahiptir. 

### <a name="real-time"></a>Gerçek zamanlı
Bkz. gerçek zamanlı algılama.

### <a name="real-time-detection"></a>Gerçek zamanlı algılama
Olayın devam etmesini izin vermeden önce oturum açma denemesi gibi bir olay riskini algılama ve değerlendirme.

### <a name="remediated-risk-event"></a>Düzeltilen (risk olayı)
Risk olayının bu tür risk olayı için standart Düzeltme eylemi kullanılarak düzeltildiğini gösteren kimlik koruması tarafından otomatik olarak ayarlanan risk olayı durumu. Örneğin, Kullanıcı parolası sıfırlandığında, önceki parolanın güvenliğinin aşıldığını belirten birçok risk olayı otomatik olarak düzeltilebilir.

### <a name="remediation"></a>Düzeltme
Daha önce şüpheli veya tehlikede bilinen bir kimlik veya cihazın güvenliğini sağlamaya yönelik bir eylem. Düzeltme eylemi, kimlik veya cihazı güvenli bir duruma geri yükler ve kimlik veya cihazla ilişkili önceki risk olaylarını çözümler.

### <a name="resolved-risk-event"></a>Çözümlendi (risk olayı)
Bir kimlik koruması kullanıcısı tarafından el ile ayarlanan bir risk olayı durumu, kullanıcının kimlik koruması dışında uygun bir düzeltme eylemi gerçekleşmekte olduğunu ve risk olayının kapalı kabul edilmesi gerektiğini gösterir.

### <a name="risk-event-status"></a>Risk olayı durumu
Etkinlik etkin olup olmadığını ve kapalı ise kapatma nedenini gösteren bir risk olayı özelliği.

### <a name="risk-event-type"></a>Risk olayı türü
Etkinliğin riskli olarak kabul edilmesine neden olan anomali türünü gösteren risk olayı kategorisi.

### <a name="risk-level-risk-event"></a>Risk düzeyi (risk olayı)
Kimlik koruma kullanıcılarının kuruluşlarındaki riski azaltmak için gerçekleştirdikleri eylemlere öncelik vermelerine yardımcı olmak için risk olayının önem derecesine sahip bir gösterge (yüksek, orta veya düşük). 

### <a name="risk-level-sign-in"></a>Risk düzeyi (oturum açma)
Belirli bir oturum açma ihtimalinin bir göstergesi (yüksek, orta veya düşük), başka biri de kullanıcının kimliğini kullanmaya çalışıyor.

### <a name="risk-level-user-compromise"></a>Risk düzeyi (Kullanıcı güvenliğinin aşılmasına neden)
Bir kimliğin tehlikeye düşmesi olasılığının bir göstergesi (yüksek, orta veya düşük).

### <a name="risk-level-vulnerability"></a>Risk düzeyi (güvenlik açığı)
Kimlik koruması kullanıcılarının kuruluşlarındaki riski azaltmak için gerçekleştirdikleri eylemlere öncelik vermelerine yardımcı olmak üzere güvenlik açığının önem derecesi (yüksek, orta veya düşük).

### <a name="secure-identity"></a>Güvenli (kimlik)
Bir parola değişikliği veya makine yeniden görüntüsü oluşturma gibi bir düzeltme eylemi gerçekleştirmek için, riskli olabilecek bir kimliği tehlikeye ayrılmamış bir duruma geri yükleyin.

### <a name="security-policy"></a>Güvenlik ilkesi
İlke kuralları ve koşul koleksiyonu. Kullanıcılar, gruplar, uygulamalar, cihazlar, cihaz platformları, cihaz durumları, IP aralıkları ve Auth 2.0 istemci türleri gibi varlıklara bir ilke uygulanabilir. Bir ilke etkinleştirildiğinde, ilkeye dahil edilen bir varlık bir kaynak için belirteç verildiğinde değerlendirilir.

### <a name="sign-in-v"></a>Oturum aç (v)
Azure Active Directory bir kimliğin kimliğini doğrulamak için.

### <a name="sign-in-n"></a>Oturum açma (n)
Azure Active Directory bir kimliğin kimliğini doğrulayan işlem veya eylem ve bu işlemi yakalayan olay.

### <a name="sign-in-from-anonymous-ip-address"></a>Anonim IP adresinden oturum açın
Anonim bir proxy IP adresi olarak tanımlanan IP adresinden başarılı bir oturum açma işleminden sonra tetiklenen bir risk olayı.

### <a name="sign-in-from-infected-device"></a>Virüslü cihazdan oturum aç
Bir oturum açma işlemi bir IP adresinden kaynaklanıyorsa, bir veya daha fazla güvenliği aşılmış bir cihaz tarafından kullanılmak üzere bilinen ve bir bot sunucusuyla iletişim kurmaya çalışan bir risk olayıdır.

### <a name="sign-in-from-ip-address-with-suspicious-activity"></a>Şüpheli etkinlikle IP adresinden oturum açın
Kısa bir süre içinde birden çok kullanıcı hesabında yüksek sayıda başarısız oturum açma girişimi olan bir IP adresinden başarılı bir oturum açma denemesi sonrasında tetiklenen bir risk olayı.

### <a name="sign-in-from-unfamiliar-location"></a>Bilmediğiniz konumdan oturum aç
Kullanıcı yeni bir konumdan (IP, Enlem/Boylam ve ASN) başarıyla oturum açtığında tetiklenen bir risk olayı.

### <a name="sign-in-risk"></a>Oturum açma riski
Risk düzeyine bakın (oturum açma)

### <a name="sign-in-risk-policy"></a>Oturum açma riski ilkesi
Belirli bir oturum açma riskini değerlendiren ve önceden tanımlanmış koşullara ve kurallara göre azaltmaları uygulayan koşullu erişim ilkesi.

### <a name="user-compromise-risk"></a>Kullanıcı güvenliğinin aşılmasına karşı risk
Risk düzeyine (Kullanıcı güvenliğinin aşılmasına) bakın

### <a name="user-risk"></a>Kullanıcı riski
Risk düzeyine (Kullanıcı güvenliğinin aşılmasına) bakın.

### <a name="user-risk-policy"></a>Kullanıcı riski ilkesi
Oturum açmayı göz önünde bulundurur ve önceden tanımlanmış koşullara ve kurallara göre azaltmaları uygulayan koşullu erişim ilkesi.

### <a name="users-flagged-for-risk"></a>Riskli oldukları belirlenen kullanıcılar
Etkin veya düzeltilmiş olan risk olaylarına sahip kullanıcılar

### <a name="vulnerability"></a>Güvenlik açığı
Azure Active Directory ' deki bir yapılandırma veya koşul, bu dizin, güvenlik açıklarından veya tehditlere karşı savunmasız hale gelir.

## <a name="see-also"></a>Ayrıca bkz.
* [Azure Active Directory Kimlik Koruması](../active-directory-identityprotection.md)
