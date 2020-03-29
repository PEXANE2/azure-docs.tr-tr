---
title: Azure Active Directory B2B en iyi uygulamalar ve öneriler
description: Azure Active Directory'de işletmelerarası (B2B) konuk kullanıcı erişimi için en iyi uygulamaları ve önerileri öğrenin.
services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: conceptual
ms.date: 12/18/2019
ms.author: mimart
author: msmimart
manager: celestedg
ms.reviewer: elisol
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 54f5721ef606b6ea916f5a00031c58f5e2adeb0e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80050858"
---
# <a name="azure-active-directory-b2b-best-practices"></a>Azure Active Directory B2B en iyi uygulamalar
Bu makalede, Azure Etkin Dizini'nde (Azure AD) işletmeler arası (B2B) işbirliği için öneriler ve en iyi uygulamalar yer alıyor.

   > [!IMPORTANT]
   > **31 Mart 2021'den itibaren Microsoft,** B2B işbirliği senaryoları için yönetilmeyen Azure AD hesapları ve kiracılar oluşturarak artık davetlerin itfasını desteklemez. Hazırlık olarak, müşterilerin tek [seferlik parola kimlik doğrulaması e-postasını](one-time-passcode.md)seçmelerini öneririz. Bu genel önizleme özelliği hakkındaki görüşlerinizi memnuniyetle karşılıyoruz ve işbirliği yapmak için daha fazla yol oluşturmaktan heyecan duyuyoruz.

## <a name="b2b-recommendations"></a>B2B önerileri
| Öneri | Yorumlar |
| --- | --- |
| En iyi oturum açma deneyimi için, kimlik sağlayıcılarla fetorat | Mümkün olduğunda, davet edilen kullanıcıların Microsoft Hesapları (MSAs) veya Azure AD hesapları oluşturmak zorunda kalmadan paylaşılan uygulamalarınızda ve kaynaklarınızda oturum açmalarına izin vermek için kimlik sağlayıcılarıyla doğrudan federate. B2B konuk kullanıcılarının Google hesaplarında oturum açmalarına izin vermek için [Google federasyonu özelliğini](google-federation.md) kullanabilirsiniz. Veya, kimlik sağlayıcısı (IdP) SAML 2.0 veya WS-Fed protokolünü destekleyen herhangi bir kuruluşla doğrudan federasyon kurmak için [Doğrudan federasyon (önizleme) özelliğini](direct-federation.md) kullanabilirsiniz. |
| Başka yollarla kimlik doğrulayamayan B2B konukları için E-posta parolasını (önizleme) özelliğini kullanın | [E-posta tek seferlik parola (önizleme)](one-time-passcode.md) özelliği, Azure AD, Microsoft hesabı (MSA) veya Google federasyonu gibi diğer yollarla kimlik doğrulaması olanınamadığında B2B konuk kullanıcılarının kimliğini doğrular. Konuk kullanıcı bir daveti kullandığınızda veya paylaşılan bir kaynağa eriştiğinde, e-posta adresine gönderilen geçici bir kod isteyebilir. Sonra oturum açmaya devam etmek için bu kodu girerler. |
| Oturum açma sayfanıza şirket markası ekleme | B2B konuk kullanıcılarınız için daha sezgisel olacak şekilde oturum açma sayfanızı özelleştirebilirsiniz. Oturum açma ve [Panel sayfalarına erişmek için şirket markasını nasıl ekleyeceğinize](../fundamentals/customize-branding.md)bakın. |
| B2B konuk kullanıcı kullanımı deneyimine gizlilik bildiriminizi ekleyin | Davet edilen bir kullanıcının gizlilik koşullarınızın devam etmesini kabul etmesi için kuruluşunuzun gizlilik bildiriminin URL'sini ilk kez davet kullanım işlemine ekleyebilirsiniz. [Nasıl Yapılacağını Görün: Azure Active Directory'de kuruluşunuzun gizlilik bilgilerini ekleyin.](https://aka.ms/adprivacystatement) |
| Birden fazla B2B konuk kullanıcısını aynı anda davet etmek için toplu davet (önizleme) özelliğini kullanma | Azure portalındaki toplu davet önizleme özelliğini kullanarak birden fazla konuk kullanıcıyı aynı anda kuruluşunuza davet edin. Bu özellik, B2B konuk kullanıcıları oluşturmak ve toplu olarak davetiye göndermek için bir CSV dosyası yüklemenize olanak tanır. [B2B kullanıcılarını toplu olarak davet etmek için Öğretici'ye](tutorial-bulk-invite.md)bakın. |
| Çok Faktörlü Kimlik Doğrulama (MFA) için Koşullu Erişim ilkelerini uygulayın | Ortak B2B kullanıcılarıyla paylaşmak istediğiniz uygulamalarda MFA politikalarını uygulamanızı öneririz. Bu şekilde, Ortak kuruluş MFA'yı kullanıp kullanmadığına bakılmaksızın, MFA kiracınızdaki uygulamalarüzerinde sürekli olarak uygulanır. [B2B işbirliği kullanıcıları için Koşullu Erişim'e](conditional-access.md)bakın. |
| Aygıt tabanlı Koşullu Erişim ilkelerini uyguluyorsanız, B2B kullanıcılarına erişime izin vermek için dışlama listelerini kullanın | Kuruluşunuzda aygıt tabanlı Koşullu Erişim ilkeleri etkinse, B2B konuk kullanıcı aygıtları kuruluşunuz tarafından yönetilmediği için engellenir. Aygıt tabanlı Koşullu Erişim ilkesinden hariç tutmak için belirli iş ortağı kullanıcıları içeren dışlama listeleri oluşturabilirsiniz. [B2B işbirliği kullanıcıları için Koşullu Erişim'e](conditional-access.md)bakın. |
| B2B konuk kullanıcılarınıza doğrudan bağlantılar sağlarken kiracıya özel bir URL kullanın | Davet e-postasına alternatif olarak, bir misafire uygulamanıza veya portalınıza doğrudan bağlantı verebilirsiniz. Bu doğrudan bağlantı kiracıya özgü olmalıdır, yani kiracı kimliği veya doğrulanmış etki alanı içermelidir, böylece konuk paylaşılan uygulamanın bulunduğu kiracınızda kimlik doğrulanabilir. [Konuk kullanıcı için Redemption deneyimine](redemption-experience.md)bakın. |
| Bir uygulama geliştirirken, konuk kullanıcı deneyimini belirlemek için UserType'ı kullanın  | Bir uygulama geliştiriyorsanız ve kiracı kullanıcılar ve konuk kullanıcılar için farklı deneyimler sağlamak istiyorsanız, UserType özelliğini kullanın. UserType talebi şu anda belirteç te dahil değildir. Uygulamalar, kullanıcının UserType'larını alması için dizini sorgulamak için Microsoft Graph API'sini kullanmalıdır. |
| Yalnızca kullanıcının kuruluşla ilişkisi değişirse UserType *özelliğini* değiştirme | Bir kullanıcı için UserType özelliğini Üye'den Misafire dönüştürmek için PowerShell'i kullanmak mümkün olsa da (ve tam tersi), bu özelliği yalnızca kullanıcının kuruluşunuzla ilişkisi değişirse değiştirmeniz gerekir. [B2B konuk kullanıcıözelliklerine](user-properties.md)bakın.|

## <a name="next-steps"></a>Sonraki adımlar

[B2B paylaşımını yönet](delegate-invitations.md)
