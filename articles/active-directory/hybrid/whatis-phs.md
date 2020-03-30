---
title: Azure AD ile parola karma senkronizasyonu nedir? | Microsoft Belgeleri
description: Parola karma eşitlemesi açıklar.
services: active-directory
author: billmath
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: overview
ms.date: 12/05/2018
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 83e172e61411c7c1c098706b5ff4566f565d6bf1
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "66253853"
---
# <a name="what-is-password-hash-synchronization-with-azure-ad"></a>Azure AD ile parola karma senkronizasyonu nedir?
Parola karma eşitleme, karma kimlik gerçekleştirmek için kullanılan oturum açma yöntemlerinden biridir. Azure AD Connect, şirket içi Active Directory örneğinden bulut tabanlı Azure AD örneğine kullanıcı parolasının karmasını eşitler.

Parola karma eşitleme, Azure AD Connect eşitlemesi tarafından uygulanan dizin eşitleme özelliğinin bir uzantısıdır. Bu özelliği, Office 365 gibi Azure AD hizmetlerinde oturum açabilmek için kullanabilirsiniz. Şirket içi Active Directory örneğinde oturum açtığınızda kullandığınız parolayı kullanarak hizmette oturum açabilirsiniz.

![Azure AD Connect nedir?](./media/how-to-connect-password-hash-synchronization/arch1.png)

Parola karma senkronizasyonu parola sayısını azaltarak yardımcı olur, kullanıcılarınızın sadece bir korumak gerekir. Parola karma senkronizasyonu şunları yapabilir:

* Kullanıcılarınızın üretkenliğini artırın.
* Yardım masası maliyetlerinizi azaltın.  

İsteğe bağlı olarak, oturum açma yönteminiz olarak [Active Directory Federation Services (AD FS) ile Federasyon'u](https://channel9.msdn.com/Series/Azure-Active-Directory-Videos-Demos/Configuring-AD-FS-for-user-sign-in-with-Azure-AD-Connect) kullanmaya karar verirseniz, parola karma senkronizasyonunu yedek olarak ayarlayabilirsiniz.

Ortamınızda parola karma senkronizasyonunu kullanmak için şunları yapmanız gerekir:

* Azure AD Connect'i yükleyin.  
* Şirket içi Active Directory örneğiniz ile Azure Etkin Dizin örneğiniz arasında dizin eşitlemesi yapılandırın.
* Parola karma eşitlemesini etkinleştirin.



Daha fazla bilgi için [bkz: Karma kimlik nedir?](whatis-hybrid-identity.md)




## <a name="next-steps"></a>Sonraki Adımlar

- [Hibrit kimlik nedir?](whatis-hybrid-identity.md)
- [Azure AD Connect ve Connect Health nedir?](whatis-azure-ad-connect.md)
- [Geçiş kimlik doğrulaması (PTA) nedir?](how-to-connect-pta.md)
- [Federasyon nedir?](whatis-fed.md)
- [Tek işaret nedir?](how-to-connect-sso.md)
- [Parola karma eşitleme nasıl çalışır?](how-to-connect-password-hash-synchronization.md)
