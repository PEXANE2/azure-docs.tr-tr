---
title: Azure AD ile Federasyon nedir? | Microsoft Belgeleri
description: Azure AD ile Federasyonu açıklar.
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
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "80049369"
---
# <a name="what-is-federation-with-azure-ad"></a>Azure AD ile Federasyon nedir?

Federasyon, güvenilen bir etki alanı koleksiyonudur. Güven düzeyi farklılık gösterebilir, ancak genellikle kimlik doğrulaması içerir ve neredeyse her zaman yetkilendirmeyi içerir. Tipik bir Federasyon, bir dizi kaynağa paylaşılan erişim için güven oluşturmuş bir sayıda kuruluş içerebilir.

Şirket içi ortamınızı Azure AD ile federasyona ekleyebilir ve bu Federasyonu kimlik doğrulama ve yetkilendirme için kullanabilirsiniz.  Bu oturum açma yöntemi, tüm Kullanıcı kimlik doğrulamasının şirket içinde gerçekleşmesini sağlar.  Bu yöntem, yöneticilerin daha ayrıntılı erişim denetimi düzeylerini uygulamasına olanak tanır. AD FS ve PingFederate ile Federasyon kullanılabilir.

![Federasyon kimliği](./media/whatis-hybrid-identity/federated-identity.png)


> [!TIP]
> Active Directory Federation Services (AD FS) ile federasyon seçeneğini kullanmaya karar verirseniz AD FS altyapınızın kullanım dışı kalması durumunda yedek çözüm olarak kullanmak üzere parola karması eşitleme seçeneğini de ayarlayabilirsiniz.


## <a name="next-steps"></a>Sonraki Adımlar

- [Hibrit kimlik nedir?](https://docs.microsoft.com/azure/active-directory/hybrid/whatis-hybrid-identity)
- [Azure AD Connect ve Connect Health nedir?](whatis-azure-ad-connect.md)
- [Parola karması eşitleme nedir?](whatis-phs.md)
- [Federasyon nedir?](whatis-fed.md)
- [Çoklu oturum açma nedir?](how-to-connect-sso.md)
- [Federasyon nasıl çalışır?](how-to-connect-fed-whatis.md)
- [PingFederate ile federasyon](how-to-connect-install-custom.md#configuring-federation-with-pingfederate)
