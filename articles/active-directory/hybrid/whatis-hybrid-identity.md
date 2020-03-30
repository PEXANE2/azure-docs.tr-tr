---
title: Azure Active Directory ile karma kimlik nedir?
description: Karma kimlik, hem şirket içinde hem de bulutta kimlik doğrulama ve yetkilendirme için ortak bir kullanıcı kimliğine sahip.
keywords: Azure AD Connect’e giriş, Azure AD Connect’e genel bakış, Azure AD Connect nedir, active directory yükleme
services: active-directory
author: billmath
manager: daveba
ms.assetid: 59bd209e-30d7-4a89-ae7a-e415969825ea
ms.service: active-directory
ms.workload: identity
ms.topic: overview
ms.date: 05/17/2019
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: c3d681dd06f07f6174e31b59cccf42df5dc16a1e
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "68779843"
---
# <a name="what-is-hybrid-identity-with-azure-active-directory"></a>Azure Active Directory ile karma kimlik nedir?

Günümüzde işletmeler ve şirketler şirket içi ve bulut uygulamalarının daha fazla bir karışımı haline gelmektedir.  Kullanıcılar bu uygulamalara hem şirket içinde hem de bulutta erişmeye ihtiyaç duyar. Kullanıcıları hem şirket içinde hem de bulutta yönetmek zorlu senaryolar oluşturur. 

Microsoft'un kimlik çözümleri şirket içi ve bulut tabanlı özellikleri kapsar.  Bu çözümler, konumne bakılmaksızın tüm kaynaklara kimlik doğrulama ve yetkilendirme için ortak bir kullanıcı kimliği oluşturur. Biz buna **melez kimlik**diyoruz.

Azure AD'ye karma kimlik ve karma kimlik yönetimi yle bu senaryolar mümkün olur.

Azure AD ile karma kimlik elde etmek için senaryolarınıza bağlı olarak üç kimlik doğrulama yönteminden biri kullanılabilir.   Üç yöntem şunlardır: 

- **[Şifre karma senkronizasyonu (PHS)](whatis-phs.md)**  
- **[Geçiş kimlik doğrulaması (PTA)](how-to-connect-pta.md)**  
- **[Federasyon (AD FS)](whatis-fed.md)** 

Bu kimlik doğrulama yöntemleri de tek [işaretli](how-to-connect-sso.md) özellikleri sağlar.  Tek oturum açma, kullanıcılarınızı kurumsal aygıtlarında olduklarında, şirket ağınıza bağlı olduklarında otomatik olarak oturuma dahil ederler.

Daha fazla bilgi için bkz. Azure [Active Directory karma kimlik çözümünüz için doğru kimlik doğrulama yöntemini seçin.](https://docs.microsoft.com/azure/security/fundamentals/choose-ad-authn) 

## <a name="common-scenarios-and-recommendations"></a>Ortak senaryolar ve öneriler 

Aşağıda, bazı yaygın karma kimlik ve erişim yönetimi senaryoları, hangi karma kimlik seçeneğinin (veya seçeneklerinin) hangi senaryoya uygun olabileceği hakkında önerilerle birlikte verilmiştir. 

|Şunu istiyorum:|PHS ve SSO<sup>1</sup>| PTA ve SSO<sup>2</sup> | AD FS<sup>3</sup>| 
|-----|-----|-----|-----| 
|Şirket içi Active Directory ortamında oluşturulan yeni kullanıcı, kişi ve grup hesaplarının otomatik olarak bulutla eşitlenmesi.|![Önerilen](./media/whatis-hybrid-identity/ic195031.png)| ![Önerilen](./media/whatis-hybrid-identity/ic195031.png) |![Önerilen](./media/whatis-hybrid-identity/ic195031.png)| 
|Office 365 karma senaryoları için kiracımı ayarlayın.|![Önerilen](./media/whatis-hybrid-identity/ic195031.png)| ![Önerilen](./media/whatis-hybrid-identity/ic195031.png) |![Önerilen](./media/whatis-hybrid-identity/ic195031.png)| 
|Kullanıcılarımın şirket içi parolalarını kullanarak oturum açmalarına ve bulut hizmetlerine erişmelerine olanak tanır.|![Önerilen](./media/whatis-hybrid-identity/ic195031.png)| ![Önerilen](./media/whatis-hybrid-identity/ic195031.png) |![Önerilen](./media/whatis-hybrid-identity/ic195031.png)| 
|Şirket kimlik bilgilerini kullanarak tek oturum açma uygulayın.|![Önerilen](./media/whatis-hybrid-identity/ic195031.png)| ![Önerilen](./media/whatis-hybrid-identity/ic195031.png) |![Önerilen](./media/whatis-hybrid-identity/ic195031.png)|  
|Parola varoşlarının bulutta depolandırılmamasından emin olun.| |![Önerilen](./media/whatis-hybrid-identity/ic195031.png)|![Önerilen](./media/whatis-hybrid-identity/ic195031.png)| 
|Bulut tabanlı çok faktörlü kimlik doğrulama çözümleri etkinleştirin.|![Önerilen](./media/whatis-hybrid-identity/ic195031.png)|![Önerilen](./media/whatis-hybrid-identity/ic195031.png)|![Önerilen](./media/whatis-hybrid-identity/ic195031.png)| 
|Şirket içi çok faktörlü kimlik doğrulama çözümlerini etkinleştirin.| | |![Önerilen](./media/whatis-hybrid-identity/ic195031.png)| 
|Kullanıcılarım için akıllı kart kimlik doğrulamasını destekleyin. <sup>4.2.2</sup>| | |![Önerilen](./media/whatis-hybrid-identity/ic195031.png)| 
|Parola son kullanma bildirimlerini Office Portalı'nda ve Windows 10 masaüstünde görüntüleyin.| | |![Önerilen](./media/whatis-hybrid-identity/ic195031.png)| 

> <sup>1</sup> Çoklu oturum açma ile parola karması eşitleme. 
> 
> <sup>2</sup> Doğrudan kimlik doğrulama ve çoklu oturum açma.  
> 
> <sup>3</sup> AD FS ile federasyon çoklu oturum açma.  
>  
> <sup>4</sup> AD FS, kurumsal PKI çözümünüzle tümleştirilerek sertifika ile oturum açma imkanı sunulabilir. Bu sertifikalar MDM veya GPO gibi güvenilen sağlama kanalları aracılığıyla dağıtılan yazılımsal sertifikalar, akıllı kart sertifikaları (PIV/CAC kartları dahil) veya İç için Hello (sertifika güveni) olabilir. Akıllı kart kimlik doğrulaması desteği hakkında daha fazla bilgi için [bu bloga](https://blogs.msdn.microsoft.com/samueld/2016/07/19/adfs-certauth-aad-o365/) bakın. 
> 

## <a name="license-requirements-for-using-azure-ad-connect"></a>Azure AD Connect'i kullanmak için lisans gereksinimleri

[!INCLUDE [active-directory-free-license.md](../../../includes/active-directory-free-license.md)]

## <a name="next-steps"></a>Sonraki Adımlar 

- [Azure AD Connect ve Connect Health nedir?](whatis-azure-ad-connect.md) 
- [Parola karma senkronizasyonu (PHS) nedir?](whatis-phs.md) 
- [Geçiş kimlik doğrulaması (PTA) nedir?](how-to-connect-pta.md) 
- [Federasyon nedir?](whatis-fed.md) 
- [Tek işaret nedir?](how-to-connect-sso.md) 

