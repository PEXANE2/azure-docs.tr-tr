---
title: Azure Active Directory Kimlik Koruma Sözlüğü
description: Azure Active Directory Kimlik Koruma Sözlüğü
services: active-directory
ms.service: active-directory
ms.subservice: identity-protection
ms.topic: reference
ms.date: 10/18/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: sahandle
ms.collection: M365-identity-device-management
ms.openlocfilehash: 9a3e2df956aaa4f9fd0af83dd2a18e04d731c714
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74232352"
---
# <a name="azure-active-directory-identity-protection-glossary"></a>Azure Active Directory Kimlik Koruma Sözlüğü

### <a name="at-risk-user"></a>Risk altında (Kullanıcı)
Bir veya daha fazla etkin risk algılaması olan bir kullanıcı. 

### <a name="atypical-sign-in-location"></a>Atipik oturum açma konumu
Belirli bir kullanıcı, benzer kullanıcılar veya kiracı için tipik olmayan bir coğrafi konumdan oturum açma.

### <a name="azure-ad-identity-protection"></a>Azure AD Kimlik Koruması
Azure Active Directory'nin risk algılamalarına ve kuruluşun kimliklerini etkileyen olası güvenlik açıklarına yönelik konsolide bir görünüm sağlayan bir güvenlik modülü.

### <a name="conditional-access"></a>Koşullu Erişim
Kaynaklara erişimi güvence altına almak için bir ilke. Koşullu Erişim kuralları Azure Etkin Dizini'nde depolanır ve kaynağa erişim izni vermeden önce Azure AD tarafından değerlendirilir.  Örnek kurallar, kullanıcı konumuna, aygıt durumuna veya kullanıcı kimlik doğrulama yöntemine göre erişimi kısıtlamayı içerir.

### <a name="credentials"></a>Kimlik Bilgileri
Yerel ve ağ kaynaklarına erişmek için kullanılan kimlik ve kimlik kanıtı içeren bilgiler. Kimlik bilgilerine örnek olarak kullanıcı adları ve parolalar, akıllı kartlar ve sertifikalar verilebilir.

### <a name="event"></a>Olay
Azure Etkin Dizini'ndeki bir etkinliğin kaydı.

### <a name="false-positive-risk-detection"></a>Yanlış pozitif (risk algılama)
Risk algılama durumunun bir Kimlik Koruması kullanıcısı tarafından el ile ayarlanması, risk tespitinin araştırıldığını ve risk tespiti olarak yanlış işaretlendiğini gösterir.

### <a name="identity"></a>Kimlik
Parola veya sertifika gibi ölçütlere bağlı olarak kimlik doğrulama yoluyla doğrulanması gereken kişi veya varlık.

### <a name="identity-risk-detection"></a>Kimlik riski algılama
Kimlik Koruması tarafından anormal olarak işaretlenmiş ve bir kimliğin ele geçirildiğini gösteren Azure AD olayı.

### <a name="ignored-risk-detection"></a>Yoksayıldı (risk algılama)
Bir Kimlik Koruması kullanıcısı tarafından el ile ayarlanan ve risk algılama nın bir düzeltme eylemi yapılmadan kapatılarak kapatılır.

### <a name="impossible-travel-from-atypical-locations"></a>Atipik yerlerden imkansız seyahat
Aynı kullanıcı için iki oturum açma algılandığında, en az birinin atipik oturum açma konumundan geldiği ve oturum açma süreleri arasındaki sürenin fiziksel olarak bu oturumaralarında seyahat etmek için gereken minimum süreden daha kısa olduğu bir risk algılaması tetiklenir. Konum.  

### <a name="investigation"></a>Araştırma
Bir risk tespiti veya azaltma adımlarının gerekli olup olmadığına karar vermek, kimliğin nasıl ve nasıl tehlikeye atıldığını anlamak ve tehlikeye atılan bilgilerin nasıl ele geçirildiğini anlamak için, risk tespitiyle ilgili etkinlikleri, günlükleri ve diğer ilgili bilgileri gözden geçirme süreci kimlik kullanılmıştır.

### <a name="leaked-credentials"></a>Sızdırılan kimlik bilgileri
Mevcut kullanıcı kimlik bilgileri (kullanıcı adı ve parola) araştırmacılarımız tarafından Dark web'de herkese açık olarak yayınlandığında tetiklenen bir risk algılaması.

### <a name="mitigation"></a>Risk azaltma
Saldırganın, kimliği veya aygıtı güvenli bir duruma geri yüklemeden gizliliği ihlal edilen bir kimlik veya aygıttan yararlanma yeteneğini sınırlamak veya ortadan kaldırmak için yapılan bir eylem. Azaltma, kimlik veya aygıtla ilişkili önceki risk algılamalarını çözmez.

### <a name="multi-factor-authentication"></a>Multi-factor authentication
Kullanıcının sahip olduğu bir şeyi( böyle bir sertifikayı) içerebilecek iki veya daha fazla kimlik doğrulama yöntemi gerektiren bir kimlik doğrulama yöntemi; kullanıcı adları, parolalar veya geçiş ifadeleri gibi kullanıcının bildiği bir şey; parmak izi gibi fiziksel özellikler; ve kişisel imza gibi kişisel özellikler.

### <a name="offline-detection"></a>Çevrimdışı algılama
Anomalilerin saptanması ve olaydan sonra oturum açma girişimi gibi bir olayın riskinin değerlendirilmesi, zaten olmuş bir olay için.

### <a name="policy-condition"></a>İlke koşulu
İlkede yer alan veya dışlanan varlıkları (gruplar, kullanıcılar, uygulamalar, aygıt platformları, Aygıt durumları, IP aralıkları, istemci türleri) tanımlayan bir güvenlik ilkesinin bir bölümü.

### <a name="policy-rule"></a>İlke kuralı
Bir güvenlik ilkesinin, ilkeyi tetikleyen koşulları ve ilke tetiklendiğinde gerçekleştirilen eylemleri açıklayan bölümü.

### <a name="prevention"></a>Önleme
Şüpheli veya gizliliğinin ihlal edildiğini bilen bir kimliğin veya aygıtın kötüye kullanımı yoluyla kuruluşa zarar vermemek için yapılan bir eylem. Önleme eylemi aygıtı veya kimliği korumaz ve önceki risk algılamalarını çözmez.

### <a name="privileged-user"></a>Ayrıcalıklı (kullanıcı)
Risk tespiti sırasında, Azure Etkin Dizini'nde Genel Yönetici, Fatura Yöneticisi, Hizmet Yöneticisi, Kullanıcı Yöneticisi ve Parola gibi bir veya daha fazla kaynağa kalıcı veya geçici yönetici izinleri olan bir kullanıcı Yönetici. 

### <a name="real-time"></a>Gerçek zamanlı
Gerçek zamanlı algılamaya bakın.

### <a name="real-time-detection"></a>Gerçek zamanlı algılama
Anomalilerin saptanması ve olayın devam etmesine izin verilmeden önce oturum açma girişimi gibi bir olayın riskinin değerlendirilmesi.

### <a name="remediated-risk-detection"></a>Düzeltilen (risk algılama)
Risk algılama durumu, risk algılamanın bu tür bir risk algılama için standart düzeltme eylemi kullanılarak düzeltıldığını belirten Kimlik Koruması tarafından otomatik olarak ayarlanır. Örneğin, kullanıcı parolası sıfırlandığında, önceki parolanın ele geçirildiğini gösteren birçok risk algılaması otomatik olarak düzeltilir.

### <a name="remediation"></a>Düzeltme
Daha önce şüphelenilen veya gizliliği ihlal edildiği bilinen bir kimliği veya aygıtı güvence altına almak için yapılan bir eylem. Düzeltme eylemi, kimliği veya aygıtı güvenli bir duruma geri yükler ve kimlik veya aygıtla ilişkili önceki risk algılamalarını çözer.

### <a name="resolved-risk-detection"></a>Çözüldü (risk algılama)
Bir Kimlik Koruma kullanıcısı tarafından el ile ayarlanan ve kullanıcının Kimlik Koruması dışında uygun bir düzeltme eylemi gerçekleştirdiğini ve risk algılamanın kapalı kabul edilmesi gerektiğini belirten bir risk algılama durumu.

### <a name="risk-detection-status"></a>Risk algılama durumu
Olayın etkin olup olmadığını ve kapatılırsa kapatma nedenini belirten bir risk algılama özelliği.

### <a name="risk-detection-type"></a>Risk algılama türü
Risk tespiti için bir kategori, olayın riskli sayılmasına neden olan anomalinin türünü gösterir.

### <a name="risk-level-risk-detection"></a>Risk düzeyi (risk tespiti)
Kimlik Koruması kullanıcılarının, kuruluşlarına yönelik riski azaltmak için yaptıkları eylemlere öncelik vermelerine yardımcı olmak için risk algılamanın ciddiyetinin (Yüksek, Orta veya Düşük) bir göstergesidir. 

### <a name="risk-level-sign-in"></a>Risk düzeyi (oturum açma)
Belirli bir oturum açma için bir başkasının kullanıcının kimliğini kullanmaya çalışma olasılığının (Yüksek, Orta veya Düşük) bir göstergesidir.

### <a name="risk-level-user-compromise"></a>Risk düzeyi (kullanıcı gizliliği)
Bir kimliğin tehlikeye girme olasılığının (Yüksek, Orta veya Düşük) göstergesi.

### <a name="risk-level-vulnerability"></a>Risk düzeyi (güvenlik açığı)
Kimlik Koruması kullanıcılarının, kuruluşları için riskleri azaltmak için yaptıkları eylemlere öncelik vermelerine yardımcı olmak için güvenlik açığının önem derecesinin (Yüksek, Orta veya Düşük) bir göstergesidir.

### <a name="secure-identity"></a>Güvenli (kimlik)
Tehlikeye girme miş olabilecek bir kimliği tehlikeye atamayan bir duruma geri yüklemek için parola değişikliği veya makine regörüntüleme gibi düzeltme eylemini alın.

### <a name="security-policy"></a>Güvenlik ilkesi
İlke kuralları ve koşulu koleksiyonu. Bir ilke, kullanıcılar, gruplar, uygulamalar, aygıtlar, aygıt platformları, aygıt durumları, IP aralıkları ve Auth2.0 istemci türleri gibi varlıklara uygulanabilir. Bir ilke etkinleştirildiğinde, ilke dahil bir varlık bir kaynak için bir belirteç verilir zaman değerlendirilir.

### <a name="sign-in-v"></a>Oturum açma (v)
Azure Etkin Dizini'nde bir kimliğe kimlik doğrulamak için.

### <a name="sign-in-n"></a>Oturum açma (n)
Azure Etkin Dizin'de bir kimliğin kimlik doğrulama işlemi veya eylemi ve bu işlemi yakalayan olay.

### <a name="sign-in-from-anonymous-ip-address"></a>Anonim IP adresinden oturum açma
Anonim proxy IP adresi olarak tanımlanan IP adresinden başarılı bir oturum açtıktan sonra tetiklenen bir risk algılama.

### <a name="sign-in-from-infected-device"></a>Virüslü cihazdan oturum açma
Bir risk algılama, bir bot sunucusuyla etkin olarak iletişim kurmaya çalışan bir veya daha fazla tehlikeye sahip aygıt tarafından kullanıldığı bilinen bir IP adresinden geldiğinde tetiklenir.

### <a name="sign-in-from-ip-address-with-suspicious-activity"></a>Şüpheli etkinlikle IP adresinden oturum açma
Kısa bir süre içinde birden fazla kullanıcı hesabında çok sayıda başarısız oturum açma denemesi olan bir IP adresinden başarılı bir oturum açma sonrasında bir risk algılaması tetiklenir.

### <a name="sign-in-from-unfamiliar-location"></a>Bilmediğiniz konumdan oturum açın
Bir kullanıcı yeni bir konumdan (IP, Latitude/Longitude ve ASN) başarılı bir şekilde kaydolduğunda bir risk algılaması tetiklenir.

### <a name="sign-in-risk"></a>Oturum açma riski
Bkz. Risk düzeyi (oturum açma)

### <a name="sign-in-risk-policy"></a>Oturum açma risk ilkesi
Belirli bir oturum açma için riski değerlendiren ve önceden tanımlanmış koşullara ve kurallara dayalı azaltıcı etkenler uygulayan Koşullu Erişim ilkesi.

### <a name="user-compromise-risk"></a>Kullanıcı uzlaşma riski
Bkz. Risk düzeyi (kullanıcı gizliliği)

### <a name="user-risk"></a>Kullanıcı riski
Bkz. Risk düzeyi (kullanıcı gizliliği).

### <a name="user-risk-policy"></a>Kullanıcı risk ilkesi
Oturum açma yı göz önünde bulundurarak ve önceden tanımlanmış koşullara ve kurallara dayalı azaltıcı etkenler uygulayan Koşullu Erişim ilkesi.

### <a name="users-flagged-for-risk"></a>Riskli oldukları belirlenen kullanıcılar
Etkin veya düzeltilen risk tespiti olan kullanıcılar

### <a name="vulnerability"></a>Güvenlik açığı
Azure Etkin Dizini'nde, dizini açık kullanıma veya tehditlere karşı duyarlı kılan bir yapılandırma veya koşul.

## <a name="see-also"></a>Ayrıca bkz.

- [Azure Active Directory Kimlik Koruması](../active-directory-identityprotection.md)
