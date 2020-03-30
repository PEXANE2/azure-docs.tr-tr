---
title: Avrupalı müşteriler için kimlik veri depolama - Azure AD
description: Azure Active Directory'nin Avrupalı müşterileri için kimlikle ilgili verileri nerede depoladığı hakkında bilgi edinin.
services: active-directory
author: msaburnley
manager: daveba
ms.author: ajburnle
ms.service: active-directory
ms.subservice: fundamentals
ms.workload: identity
ms.topic: conceptual
ms.date: 03/04/2019
ms.custom: it-pro, seodec18
ms.collection: M365-identity-device-management
ms.openlocfilehash: 7360b11f41cc08c2beb3ffa227e1658881798502
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75422992"
---
# <a name="identity-data-storage-for-european-customers-in-azure-active-directory"></a>Azure Active Directory'deki Avrupalı müşteriler için kimlik veri depolama
Kimlik verileri, Azure AD tarafından kuruluşunuzun Office 365 ve Azure gibi bir Microsoft Çevrimiçi hizmetine abone olurken sağladığı adrese dayalı coğrafi bir konumda depolanır. Kimlik verilerinizin nerede depolandığı hakkında bilgi için, [verilerinizin nerede bulunduğu?](https://www.microsoft.com/trustcenter/privacy/where-your-data-is-located) bölümünü kullanabilirsiniz.

Azure AD, Avrupa'da adres sağlayan müşteriler için kimlik verilerinin çoğunu Avrupa veri merkezlerinde saklar. Bu belge, Azure AD hizmetleri tarafından Avrupa dışında depolanan veriler hakkında bilgi sağlar.

## <a name="microsoft-azure-multi-factor-authentication-mfa"></a>Microsoft Azure çok faktörlü kimlik doğrulama (MFA)
    
- Telefon görüşmeleri veya SMS kullanarak tüm iki faktörlü kimlik doğrulama abd veri merkezlerinden kaynaklanır ve aynı zamanda küresel sağlayıcılar tarafından yönlendirilir.
- Microsoft Authenticator uygulamasını kullanarak anında iletme bildirimleri ABD veri merkezlerinden kaynaklanır. Buna ek olarak, cihaz satıcısına özel hizmetler de devreye girebilir ve bu hizmetler belki Avrupa dışında olabilir.
- OATH kodları her zaman ABD'de doğrulanır. 

Azure Çok Faktörlü Authentication Server (MFA Server) ve bulut tabanlı Azure MFA tarafından hangi kullanıcı bilgilerinin toplandığı hakkında daha fazla bilgi için Azure [Çok Faktörlü Kimlik Doğrulama kullanıcı veri koleksiyonuna](https://docs.microsoft.com/azure/active-directory/authentication/howto-mfa-reporting-datacollection)bakın.

## <a name="microsoft-azure-active-directory-b2c-azure-ad-b2c"></a>Microsoft Azure Active Directory B2C (Azure AD B2C)

Azure AD B2C ilke yapılandırma verileri ve Anahtar Kapsayıcılar ABD veri merkezlerinde depolanır. Bunlar herhangi bir kullanıcı kişisel veri içermez. İlke yapılandırması hakkında daha fazla bilgi için [Azure Active Directory B2C: Yerleşik ilkeler](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-reference-policies) makalesine bakın.

## <a name="microsoft-azure-active-directory-b2b-azure-ad-b2b"></a>Microsoft Azure Active Directory B2B (Azure AD B2B) 
    
Azure AD B2B, abd veri merkezlerinde url bilgilerini kullanma ve yönlendirme ile davetiyeleri depolar. Ayrıca, B2B davetlerini almaktan aboneliğini iptal eden kullanıcıların e-posta adresi ABD veri merkezlerinde de depolanır.

## <a name="microsoft-azure-active-directory-domain-services-azure-ad-ds"></a>Microsoft Azure Active Directory Domain Services (Azure AD DS)

Azure AD DS, kullanıcı verilerini müşterinin seçtiği Azure Sanal Ağı ile aynı konumda depolar. Ağınız Avrupa dışındaysa veriler Avrupa dışında çoğaltılır ve depolanır.

## <a name="federation-in-microsoft-exchange-server-2013"></a>Microsoft Exchange Server 2013 Federasyonu
    
- Uygulama tanımlayıcısı (AppID) - Exchange kuruluşlarını tanımlamak için Azure Active Directory kimlik doğrulama sistemi tarafından oluşturulan benzersiz bir numara.
- Uygulama için Onaylı Federe etki alanları listesi
- Uygulamanın belirteci Ortak Anahtar imzalama 

Microsoft Exchange sunucusunda federasyon hakkında daha fazla bilgi için [Federasyon: Exchange 2013 Yardım](https://docs.microsoft.com/exchange/federation-exchange-2013-help) makalesine bakın.


## <a name="other-considerations"></a>Diğer konular

Azure AD ile tümleşen hizmetler ve uygulamalar kimlik verilerine erişebilir. Kimlik verilerinin söz konusu hizmet ve uygulama tarafından nasıl işlenip işlenmediğini ve şirketinizin veri depolama gereksinimlerini karşılayıp karşılamadığını belirlemek için kullandığınız her hizmet ve uygulamayı değerlendirin.

Microsoft hizmetlerinin verileri depoladığı konumlar hakkında daha fazla bilgi için Microsoft Güven Merkezi'nin [Verileriniz nerede bulunur?](https://www.microsoft.com/trustcenter/privacy/where-your-data-is-located) bölümünü inceleyebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
Yukarıda açıklanan özellikler ve işlevler hakkında daha fazla bilgi için aşağıdaki makalelere bakın:
- [Multi-Factor Authentication nedir?](https://docs.microsoft.com/azure/active-directory/authentication/multi-factor-authentication)

- [Azure AD self servis parola sıfırlama](https://docs.microsoft.com/azure/active-directory/authentication/active-directory-passwords-overview)

- [Azure Active Directory B2C nedir?](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-overview)

- [Azure AD B2B işbirliği nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-b2b-what-is-azure-ad-b2b)

- [Azure Active Directory (AD) Domain Services](https://docs.microsoft.com/azure/active-directory-domain-services/active-directory-ds-overview)
