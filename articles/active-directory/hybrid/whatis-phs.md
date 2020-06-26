---
title: Azure AD ile parola karması eşitlemesi nedir? | Microsoft Belgeleri
description: Parola karması eşitlemesini açıklar.
services: active-directory
author: billmath
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: overview
ms.date: 06/25/2020
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 3e40eecce3ff0ea5b252fd135ae553fa20e0a77b
ms.sourcegitcommit: b56226271541e1393a4b85d23c07fd495a4f644d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/26/2020
ms.locfileid: "85386360"
---
# <a name="what-is-password-hash-synchronization-with-azure-ad"></a>Azure AD ile parola karması eşitlemesi nedir?
Parola karması eşitleme, karma kimlik gerçekleştirmek için kullanılan oturum açma yöntemlerinden biridir. Azure AD Connect, kullanıcının bir Kullanıcı parolasının karmasını, şirket içi Active Directory örneğinden bulut tabanlı bir Azure AD örneğine eşitler.

Parola karması eşitleme, Azure AD Connect eşitleme tarafından uygulanan Dizin eşitleme özelliğinin bir uzantısıdır. Bu özelliği, Office 365 gibi Azure AD hizmetlerinde oturum açmak için kullanabilirsiniz. Şirket içi Active Directory örneğinde oturum açmak için kullandığınız parolayı kullanarak hizmette oturum açın.

![Azure AD Connect nedir?](./media/how-to-connect-password-hash-synchronization/arch1.png)

Parola karma eşitlemesi, kullanıcılarınızın yalnızca bir tane üzerinde saklanması gereken parola sayısını azaltarak yardımcı olur. Parola karması eşitleme şunları yapabilir:

* Kullanıcılarınızın üretkenliğini artırın.
* Yardım Masası maliyetinizi azaltın.  

Parola karması eşitleme, karma hesaplarınız için [sızdırılan kimlik bilgisi algılamayı](../identity-protection/concept-identity-protection-risks.md#user-risk) de mümkün hale getirmenizi. Microsoft, genel olarak kullanılabilen Kullanıcı adı/parola çiftlerini bulmak için koyu Web araştırmacıları ve yasalar zorlama kuruluşlarıyla birlikte çalışmaktadır. Bu çiftlerin herhangi biri kullanıcılarımızla eşleşiyorsa, ilişkili hesap yüksek riske taşınır. 

>[!NOTE]
> Yalnızca yeni sızdırılan kimlik bilgileri, kiracının kiracınızda işlenmesini etkinleştirdikten sonra bulunur. Daha önce bulunan kimlik bilgisi çiftlerine karşı doğrulama yapılmaz.


İsteğe bağlı olarak, oturum açma yönteminiz olarak [Active Directory Federasyon Hizmetleri (AD FS) (AD FS) Ile Federasyon](https://channel9.msdn.com/Series/Azure-Active-Directory-Videos-Demos/Configuring-AD-FS-for-user-sign-in-with-Azure-AD-Connect) kullanmaya karar verirseniz, Parola karması eşitlemesini yedekleme olarak ayarlayabilirsiniz.

Ortamınızda Parola karması eşitlemesini kullanmak için şunları yapmanız gerekir:

* Azure AD Connect 'i yükler.  
* Şirket içi Active Directory örneğiniz ve Azure Active Directory örneğiniz arasında dizin eşitlemeyi yapılandırın.
* Parola karması eşitlemesini etkinleştirin.



Daha fazla bilgi için bkz. [karma kimlik nedir?](whatis-hybrid-identity.md).




## <a name="next-steps"></a>Sonraki Adımlar

- [Hibrit kimlik nedir?](whatis-hybrid-identity.md)
- [Azure AD Connect ve Connect Health nedir?](whatis-azure-ad-connect.md)
- [Geçişli kimlik doğrulaması (PTA) nedir?](how-to-connect-pta.md)
- [Federasyon nedir?](whatis-fed.md)
- [Çoklu oturum açma nedir?](how-to-connect-sso.md)
- [Parola karması eşitlemesi nasıl çalışacaktır?](how-to-connect-password-hash-synchronization.md)
