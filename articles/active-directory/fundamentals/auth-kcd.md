---
title: Azure Active Directory ile Kerberos kısıtlanmış temsili
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
ms.openlocfilehash: 77f90cd7aa8d972226a8f134eaa7b3abfe7bea66
ms.sourcegitcommit: ae6e7057a00d95ed7b828fc8846e3a6281859d40
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/16/2020
ms.locfileid: "92114465"
---
# <a name="windows-authentication---kerberos-constrained-delegation-with-azure-active-directory"></a>Windows kimlik doğrulaması-Azure Active Directory ile Kerberos kısıtlı temsili

Kerberos kısıtlanmış temsili (KCD), kaynaklar arasında kısıtlı temsili sağlar ve hizmet asıl adlarını temel alır. Etki alanı yöneticilerinin temsilcileri oluşturmasını ve tek bir etki alanıyla sınırlı olmasını gerektirir. Kaynak tabanlı KCD genellikle, bir Active Directory ormanında birden çok etki alanında kullanıcıları olan bir Web uygulaması için Kerberos kimlik doğrulaması sağlamanın bir yolu olarak kullanılır.

Azure Active Directory Uygulama Ara Sunucusu, erişim ve Kerberos kısıtlı temsilcisi (KCD) için Kerberos bileti gerektiren KCD tabanlı uygulamalara çoklu oturum açma (SSO) ve uzaktan erişim sağlayabilir.

SSO 'yu, Active Directory kullanıcıların kimliğine bürünmek için uygulama ara sunucusu bağlayıcılarına izin vererek tümleşik Windows kimlik doğrulaması (ıWA) kullanan şirket içi KCD uygulamalarınız için etkinleştirir. Uygulama proxy Bağlayıcısı, kullanıcıların adına belirteç göndermek ve almak için bu izni kullanır.

## <a name="use-when"></a>Şu durumlarda kullanın

Uzaktan erişim sağlamak, ön kimlik doğrulamasıyla korumak ve şirket içi ıWA uygulamalarına SSO sağlamak için bir gereksinim vardır.

![Mimari diyagramı](./media/authentication-patterns/kcd-auth.png)

## <a name="components-of-system"></a>Sistem bileşenleri

* **Kullanıcı**: uygulama proxy 'si tarafından sunulan eski uygulamaya erişir.

* **Web tarayıcısı**: kullanıcının uygulamanın dış URL 'sine erişmek için etkileşimde bulunduğu bileşen.

* **Azure AD**: kullanıcının kimliğini doğrular. 

* **Uygulama proxy hizmeti**: kullanıcıdan şirket içi uygulamaya istek göndermek için ters proxy işlevi görür. Azure AD 'de bulunur. Uygulama proxy 'Si, koşullu erişim ilkelerini de uygulayabilir.

* **Uygulama proxy Bağlayıcısı**: uygulamaya bağlantı sağlamak için Windows Server 'da şirket içinde yüklü. Azure AD 'nin yanıtını döndürür. Active Directory ile KCD anlaşması gerçekleştirerek uygulamaya Kerberos belirteci almak için kullanıcıyı taklit edin.

* **Active Directory**: uygulamanın Kerberos belirtecini uygulama proxy Bağlayıcısı 'na gönderir.

* **Eski uygulamalar**: uygulama proxy 'sinden Kullanıcı istekleri alan uygulamalar. Eski uygulamalar, uygulama proxy Bağlayıcısı 'na yanıtı döndürür.

## <a name="implement-windows-authentication-kcd-with-azure-ad"></a>Azure AD ile Windows kimlik doğrulaması (KCD) uygulama

* [Uygulama Ara Sunucusu ile uygulamalarınızda çoklu oturum açmak için Kerberos Kısıtlanmış Temsil](https://docs.microsoft.com/azure/active-directory/manage-apps/application-proxy-configure-single-sign-on-with-kcd) 

* [Azure Active Directory içindeki uygulama proxy 'Si aracılığıyla uzaktan erişim için şirket içi uygulama ekleme](https://docs.microsoft.com/azure/active-directory/manage-apps/application-proxy-add-on-premises-application)

 
