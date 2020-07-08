---
title: Azure AD federasyonu uyumluluk listesi
description: Bu sayfada çoklu oturum açma uygulamak için kullanılabilecek Microsoft dışı kimlik sağlayıcıları vardır.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: curtand
ms.assetid: 22c8693e-8915-446d-b383-27e9587988ec
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: how-to
ms.date: 08/23/2018
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: f3ca659535c225ca6736fe7bba8aa5b0120c684c
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85359832"
---
# <a name="azure-ad-federation-compatibility-list"></a>Azure AD federasyonu uyumluluk listesi
Azure Active Directory, üçüncü taraf çözüm gerekmeden karma ve yalnızca bulut uygulamalarında Office 365 ve diğer Microsoft Çevrimiçi Hizmetleri için çoklu oturum açma ve gelişmiş uygulama erişim güvenliği sağlar. Microsoft 'un çevrimiçi hizmetlerinin birçoğu gibi Office 365, Dizin Hizmetleri, kimlik doğrulama ve yetkilendirme için Azure Active Directory tümleşiktir. Azure Active Directory ayrıca binlerce SaaS uygulamasında ve şirket içi Web uygulamalarında çoklu oturum açma olanağı da sağlar. Desteklenen SaaS uygulamaları için Azure Active Directory [uygulama galerisine](https://azuremarketplace.microsoft.com/marketplace/apps/category/azure-active-directory-apps) bakın. 

## <a name="idp-validation"></a>IDP doğrulaması
Kuruluşunuz bir üçüncü taraf Federasyon çözümü kullanıyorsa, üçüncü taraf Federasyon çözümünün Azure Active Directory ile uyumlu olması şartıyla, Office 365 gibi Microsoft Online Services ile şirket içi Active Directory kullanıcılarınız için çoklu oturum açmayı yapılandırabilirsiniz.  Uyumlulukla ilgili sorular için lütfen kimlik sağlayıcınızla iletişim kurun.  Daha önce Azure AD ile uyumluluk için test edilmiş kimlik sağlayıcılarının bir listesini görmek isterseniz, Microsoft tarafından [buraya](https://www.microsoft.com/download/details.aspx?id=56843)tıklayın. 

>[!NOTE]
>Microsoft artık Azure Active Directory uyumluluk için bağımsız kimlik sağlayıcılarına doğrulama testi sağlamaz. Ürününüzü birlikte çalışabilirlik için test etmek istiyorsanız lütfen bu [yönergelere](https://www.microsoft.com/download/details.aspx?id=56843)başvurun. 

## <a name="next-steps"></a>Sonraki Adımlar

- [Şirket içi dizinlerinizi Azure Active Directory ile tümleştirme](whatis-hybrid-identity.md)
- [Azure AD Connect ve Federasyon](how-to-connect-fed-whatis.md)
