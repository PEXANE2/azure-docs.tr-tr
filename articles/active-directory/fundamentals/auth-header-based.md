---
title: Azure Active Directory ile üst bilgi tabanlı kimlik doğrulaması
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
ms.openlocfilehash: 4f364e4e14dd1b7c60cb81f06051d9dedd94396a
ms.sourcegitcommit: ae6e7057a00d95ed7b828fc8846e3a6281859d40
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/16/2020
ms.locfileid: "92114454"
---
# <a name="header-based-authentication-with-azure-active-directory"></a>Azure Active Directory ile üst bilgi tabanlı kimlik doğrulaması

Eski uygulamalar genellikle üst bilgi tabanlı kimlik doğrulaması kullanır. Bu senaryoda, bir Kullanıcı (veya ileti kaynağı) bir ara kimlik çözümünde kimlik doğrular. Ara çözüm kullanıcının kimliğini doğrular ve gerekli Köprü Metni Aktarım Protokolü (HTTP) üstbilgilerini hedef Web hizmetine yayar. Azure Active Directory (AD), uygulama proxy 'Si hizmeti ve diğer ağ denetleyicisi çözümleriyle tümleştirmeler aracılığıyla bu kalıbı destekler. 

Çözümünüzde uygulama proxy 'Si uygulamaya uzaktan erişim sağlar, kullanıcının kimliğini doğrular ve uygulama için gereken üst bilgileri geçirir. 

## <a name="use-when"></a>Şu durumlarda kullanın

Uzak kullanıcıların, üst bilgi tabanlı kimlik doğrulaması gerektiren şirket içi uygulamalara güvenli bir şekilde çoklu oturum açma (SSO) yapması gerekir.

![Mimari görüntü üst bilgi tabanlı kimlik doğrulaması](./media/authentication-patterns/header-based-auth.png)

## <a name="components-of-system"></a>Sistem bileşenleri

* **Kullanıcı**: uygulama proxy 'si tarafından sunulan eski uygulamalara erişir.

* **Web tarayıcısı**: kullanıcının uygulamanın dış URL 'sine erişmek için etkileşimde bulunduğu bileşen.

* **Azure AD**: kullanıcının kimliğini doğrular. 

* **Uygulama proxy hizmeti**: kullanıcıdan şirket içi uygulamaya istek göndermek için ters proxy işlevi görür. Azure AD 'de bulunur ve tüm koşullu erişim ilkelerini de uygulayabilir.

* **Uygulama proxy Bağlayıcısı**: uygulamalara bağlantı sağlamak için Windows Server 'da şirket içinde yüklü. Yalnızca giden bağlantıları kullanır. Azure AD 'nin yanıtını döndürür.

* **Eski uygulamalar**: uygulama proxy 'sinden Kullanıcı istekleri alan uygulamalar. Eski uygulama, bir oturum ayarlamak ve bir yanıt döndürmek için gerekli HTTP üstbilgilerini alır. 

## <a name="implement-header-based-authentication-with-azure-ad"></a>Azure AD ile üst bilgi tabanlı kimlik doğrulaması uygulama

* [Azure AD 'de uygulama proxy 'Si aracılığıyla uzaktan erişim için şirket içi uygulama ekleme](https://docs.microsoft.com/azure/active-directory/manage-apps/application-proxy-add-on-premises-application)  

* [Uygulama Ara Sunucusu ve PingAccess ile çoklu oturum açmak için üst bilgi tabanlı kimlik doğrulaması](https://docs.microsoft.com/azure/active-directory/manage-apps/application-proxy-configure-single-sign-on-with-ping-access) 

* [Uygulama teslim denetleyicileri ve ağlarla eski uygulamaları güvenli hale getirme](https://docs.microsoft.com/azure/active-directory/manage-apps/secure-hybrid-access)
