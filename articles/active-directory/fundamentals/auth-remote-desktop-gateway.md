---
title: Azure Active Directory sahip Hizmetleri Uzak Masaüstü Ağ Geçidi
description: Bu kimlik doğrulama modelini elde etmek için mimari yönergeler
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
ms.openlocfilehash: ff099d41970ad4d78d5c6035a60f448f29ab24b1
ms.sourcegitcommit: ae6e7057a00d95ed7b828fc8846e3a6281859d40
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/16/2020
ms.locfileid: "92114352"
---
# <a name="remote-desktop-gateway-services"></a>Uzak Masaüstü Ağ Geçidi Hizmetleri

Standart Uzak Masaüstü Hizmetleri (RDS) dağıtımı, Windows Server üzerinde çalışan çeşitli [Uzak Masaüstü rolü hizmetlerini](https://technet.microsoft.com/windows-server-docs/compute/remote-desktop-services/desktop-hosting-logical-architecture) içerir. Azure Active Directory (Azure AD) uygulama proxy 'Si ile RDS dağıtımı, bağlayıcı hizmetini çalıştıran sunucudan kalıcı bir giden bağlantıya sahiptir. Diğer dağıtımlar, Açık gelen bağlantıları bir yük dengeleyici aracılığıyla bırakır. Bu kimlik doğrulama stili, Uzak Masaüstü Hizmetleri aracılığıyla şirket içi uygulamaları yayımlayarak daha fazla tür uygulama sunmanıza olanak tanır. Ayrıca, Azure AD Uygulama Ara Sunucusu kullanarak dağıtımının saldırı yüzeyini de azaltır.

## <a name="use-when"></a>Şu durumlarda kullanın

Uzaktan erişim sağlamanız ve Uzak Masaüstü Hizmetleri dağıtımınızı ön kimlik doğrulamasıyla korumanız gerekir.

![mimari diyagram](./media/authentication-patterns/rdp-auth.png)

## <a name="components-of-system"></a>Sistem bileşenleri

* **Kullanıcı**: uygulama proxy 'si tarafından sunulan RDS 'ye erişir.

* **Web tarayıcısı**: kullanıcının uygulamanın dış URL 'sine erişmek için etkileşimde bulunduğu bileşen.

* **Azure AD**: kullanıcının kimliğini doğrular. 

* **Uygulama proxy hizmeti**: ISTEğI kullanıcıdan RDS 'ye iletmek için ters proxy işlevi görür. Uygulama proxy 'Si, koşullu erişim ilkelerini de uygulayabilir. 

* **Uzak Masaüstü Hizmetleri**: bağımsız sanallaştırılmış uygulamalar için bir platform görevi görür, güvenli mobil ve uzak masaüstü erişimi sağlar ve son kullanıcılara, uygulamalarını ve masaüstlerini buluttan çalıştırma yeteneği sağlar. 

## <a name="implement-remote-desktop-gateway-services-with-azure-ad"></a>Azure AD ile Uzak Masaüstü Ağ Geçidi Hizmetleri uygulama

* [Azure AD Uygulama Ara Sunucusu uzak masaüstü 'nü yayımlama](https://docs.microsoft.com/azure/active-directory/manage-apps/application-proxy-integrate-with-remote-desktop-services) 

* [Azure AD 'de uygulama proxy 'Si aracılığıyla uzaktan erişim için şirket içi uygulama ekleme](https://docs.microsoft.com/azure/active-directory/manage-apps/application-proxy-add-on-premises-application)

 
