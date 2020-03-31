---
title: Azure AD ile federasyon nedir? | Microsoft Belgeleri
description: Azure AD ile federasyonu açıklar.
services: active-directory
author: billmath
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: overview
ms.date: 11/28/2018
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: fd7a5b186ebc98babe1baacc9f56f39dc8fbc093
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80049369"
---
# <a name="what-is-federation-with-azure-ad"></a>Azure AD ile federasyon nedir?

Federasyon, güven kurmuş etki alanları topluluğudur. Güven düzeyi değişebilir, ancak genellikle kimlik doğrulaması içerir ve hemen hemen her zaman yetkilendirme içerir. Tipik bir federasyon, bir dizi kaynağa paylaşılan erişim için güven oluşturmuş bir dizi kuruluş içerebilir.

Azure AD ile şirket içi ortamınızı feforize edebilir ve bu federasyonu kimlik doğrulama ve yetkilendirme için kullanabilirsiniz.  Bu oturum açma yöntemi, tüm kullanıcı kimlik doğrulamasının şirket içinde gerçekleşmesini sağlar.  Bu yöntem, yöneticilerin daha sıkı erişim denetim düzeyleri uygulamasına olanak tanır. FEDERASYON AD FS ve PingFederate ile kullanılabilir.

![Federasyon kimliği](./media/whatis-hybrid-identity/federated-identity.png)


> [!TIP]
> Active Directory Federation Services (AD FS) ile federasyon seçeneğini kullanmaya karar verirseniz AD FS altyapınızın kullanım dışı kalması durumunda yedek çözüm olarak kullanmak üzere parola karması eşitleme seçeneğini de ayarlayabilirsiniz.


## <a name="next-steps"></a>Sonraki Adımlar

- [Hibrit kimlik nedir?](https://docs.microsoft.com/azure/active-directory/hybrid/whatis-hybrid-identity)
- [Azure AD Connect ve Connect Health nedir?](whatis-azure-ad-connect.md)
- [Parola karması eşitleme nedir?](whatis-phs.md)
- [Federasyon nedir?](whatis-fed.md)
- [Tek işaret nedir?](how-to-connect-sso.md)
- [Federasyon nasıl çalışır?](how-to-connect-fed-whatis.md)
- [PingFederate ile federasyon](how-to-connect-install-custom.md#configuring-federation-with-pingfederate)
