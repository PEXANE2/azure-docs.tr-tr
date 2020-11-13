---
title: Azure Active Directory sahip Hizmetleri Uzak Masaüstü Ağ Geçidi
description: Azure Active Directory ile Uzak Masaüstü Ağ Geçidi hizmetleri elde etmeye yönelik mimari rehberlik.
services: active-directory
author: BarbaraSelden
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: fundamentals
ms.topic: conceptual
ms.date: 10/10/2020
ms.author: baselden
ms.reviewer: ajburnle
ms.custom: it-pro, seodec18
ms.collection: M365-identity-device-management
ms.openlocfilehash: 57466cccb71444d5711a9221c324a107757c5e82
ms.sourcegitcommit: 1d6ec4b6f60b7d9759269ce55b00c5ac5fb57d32
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/13/2020
ms.locfileid: "94576804"
---
# <a name="remote-desktop-gateway-services"></a>Uzak Masaüstü Ağ Geçidi Hizmetleri

Standart Uzak Masaüstü Hizmetleri (RDS) dağıtımı, Windows Server üzerinde çalışan çeşitli [Uzak Masaüstü rolü hizmetlerini](https://technet.microsoft.com/windows-server-docs/compute/remote-desktop-services/desktop-hosting-logical-architecture) içerir. Azure Active Directory (Azure AD) uygulama proxy 'Si ile RDS dağıtımı, bağlayıcı hizmetini çalıştıran sunucudan kalıcı bir giden bağlantıya sahiptir. Diğer dağıtımlar, Açık gelen bağlantıları bir yük dengeleyici aracılığıyla bırakır. Bu kimlik doğrulama stili, Uzak Masaüstü Hizmetleri aracılığıyla şirket içi uygulamaları yayımlayarak daha fazla tür uygulama sunmanıza olanak tanır. Ayrıca, Azure AD Uygulama Ara Sunucusu kullanarak dağıtımının saldırı yüzeyini de azaltır.

## <a name="use-when"></a>Şu durumlarda kullanın

Uzaktan erişim sağlamanız ve Uzak Masaüstü Hizmetleri dağıtımınızı ön kimlik doğrulamasıyla korumanız gerekir.

![mimari diyagram](./media/authentication-patterns/rdp-auth.png)

## <a name="components-of-system"></a>Sistem bileşenleri

* **Kullanıcı** : uygulama proxy 'si tarafından sunulan RDS 'ye erişir.

* **Web tarayıcısı** : kullanıcının uygulamanın dış URL 'sine erişmek için etkileşimde bulunduğu bileşen.

* **Azure AD** : kullanıcının kimliğini doğrular. 

* **Uygulama proxy hizmeti** : ISTEğI kullanıcıdan RDS 'ye iletmek için ters proxy işlevi görür. Uygulama proxy 'Si, koşullu erişim ilkelerini de uygulayabilir. 

* **Uzak Masaüstü Hizmetleri** : bağımsız sanallaştırılmış uygulamalar için bir platform görevi görür, güvenli mobil ve uzak masaüstü erişimi sağlar ve son kullanıcılara, uygulamalarını ve masaüstlerini buluttan çalıştırma yeteneği sağlar. 

## <a name="implement-remote-desktop-gateway-services-with-azure-ad"></a>Azure AD ile Uzak Masaüstü Ağ Geçidi Hizmetleri uygulama

* [Azure AD Uygulama Ara Sunucusu uzak masaüstü 'nü yayımlama](https://docs.microsoft.com/azure/active-directory/manage-apps/application-proxy-integrate-with-remote-desktop-services) 

* [Azure AD 'de uygulama proxy 'Si aracılığıyla uzaktan erişim için şirket içi uygulama ekleme](https://docs.microsoft.com/azure/active-directory/manage-apps/application-proxy-add-on-premises-application)

 
