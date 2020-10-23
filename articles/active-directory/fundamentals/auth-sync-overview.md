---
title: Azure Active Directory kimlik doğrulaması ve eşitleme protokolüne genel bakış
description: Azure AD 'yi eski kimlik doğrulama protokolleri ve eşitleme desenleriyle tümleştirmek için mimari yönergeler
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
ms.openlocfilehash: ab63bc5bd2819a239741da525eebb2404a47bbf9
ms.sourcegitcommit: 9b8425300745ffe8d9b7fbe3c04199550d30e003
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/23/2020
ms.locfileid: "92441207"
---
# <a name="azure-active-directory-integrations-with-authentication-and-synchronization-protocols"></a>Kimlik doğrulama ve eşitleme protokolleriyle tümleştirmelere Azure Active Directory

Microsoft Azure Active Directory (Azure AD), birçok kimlik doğrulama ve eşitleme protokolleriyle tümleştirmeyi mümkün. Kimlik doğrulama tümleştirmeleri, Azure AD 'yi ve güvenlik ve yönetim özelliklerini, eski kimlik doğrulama yöntemlerini kullanan uygulamalarınızda çok az değişiklik yapmadan kullanmanıza olanak sağlar. Eşitleme tümleştirmeleri, Kullanıcı ve grup verilerini Azure AD 'ye eşitlemenize ve sonra Azure AD Yönetim Özellikleri ' ne sahip olabilirsiniz. Bazı eşitleme desenleri de otomatik sağlamayı etkinleştirir.

## <a name="legacy-authentication-protocols"></a>Eski kimlik doğrulama protokolleri

Aşağıdaki tabloda, kimlik doğrulamasının eski kimlik doğrulama protokolleriyle ve özellikleri ile Azure AD tümleştirmesi sunulmaktadır. Görmek için bir kimlik doğrulama protokolünün adını seçin

* Ayrıntılı bir açıklama

* Kullanılması gereken durumlar

* Mimari şema

* Sistem bileşenlerinin açıklaması

* Tümleştirmenin nasıl uygulanacağı ile ilgili bağlantılar

 

| Kimlik doğrulama protokolü| Kimlik Doğrulaması| Yetkilendirme| Multi-factor Authentication| Koşullu Erişim |
| - |- | - | - | - |
| [Üst bilgi tabanlı kimlik doğrulaması](auth-header-based.md)|![onay işareti](./media/authentication-patterns/check.png)| ![onay işareti](./media/authentication-patterns/check.png)| ![onay işareti](./media/authentication-patterns/check.png)| ![onay işareti](./media/authentication-patterns/check.png) |
| [LDAP kimlik doğrulaması](auth-ldap.md)| ![onay işareti](./media/authentication-patterns/check.png)| | |  |
| [OAuth 2.0 kimlik doğrulaması](auth-oauth2.md)| ![onay işareti](./media/authentication-patterns/check.png)| ![onay işareti](./media/authentication-patterns/check.png)| ![onay işareti](./media/authentication-patterns/check.png)| ![onay işareti](./media/authentication-patterns/check.png) |
| [OıDC kimlik doğrulaması](auth-oidc.md)| ![onay işareti](./media/authentication-patterns/check.png)| ![onay işareti](./media/authentication-patterns/check.png)| ![onay işareti](./media/authentication-patterns/check.png)| ![onay işareti](./media/authentication-patterns/check.png) |
| [Parola tabanlı SSO kimlik doğrulaması](auth-password-based-sso.md )| ![onay işareti](./media/authentication-patterns/check.png)| ![onay işareti](./media/authentication-patterns/check.png)| ![onay işareti](./media/authentication-patterns/check.png)| ![onay işareti](./media/authentication-patterns/check.png) |
| [Radius kimlik doğrulaması]( auth-radius.md)| ![onay işareti](./media/authentication-patterns/check.png)| | ![onay işareti](./media/authentication-patterns/check.png)| ![onay işareti](./media/authentication-patterns/check.png) |
| [Uzak Masaüstü Ağ Geçidi Hizmetleri](auth-remote-desktop-gateway.md)| ![onay işareti](./media/authentication-patterns/check.png)| ![onay işareti](./media/authentication-patterns/check.png)| ![onay işareti](./media/authentication-patterns/check.png)| ![onay işareti](./media/authentication-patterns/check.png) |
| [Secure Shell (SSH)](auth-ssh.md) |  ![onay işareti](./media/authentication-patterns/check.png)| | ![onay işareti](./media/authentication-patterns/check.png)| ![onay işareti](./media/authentication-patterns/check.png) |
| [SAML kimlik doğrulaması](auth-saml.md)| ![onay işareti](./media/authentication-patterns/check.png)| ![onay işareti](./media/authentication-patterns/check.png)| ![onay işareti](./media/authentication-patterns/check.png)| ![onay işareti](./media/authentication-patterns/check.png) |
| [Windows kimlik doğrulaması-Kerberos kısıtlı temsili](auth-kcd.md)| ![onay işareti](./media/authentication-patterns/check.png)| ![onay işareti](./media/authentication-patterns/check.png)| ![onay işareti](./media/authentication-patterns/check.png)| ![onay işareti](./media/authentication-patterns/check.png) |


 
## <a name="synchronization-patterns"></a>Eşitleme desenleri

Aşağıdaki tabloda, eşitleme desenleriyle Azure AD tümleştirmesi ve bunların özellikleri sunulmaktadır. Görmek için bir düzenin adını seçin

* Ayrıntılı bir açıklama

* Kullanılması gereken durumlar

* Mimari şema

* Sistem bileşenlerinin açıklaması

* Tümleştirmenin nasıl uygulanacağı ile ilgili bağlantılar



| Eşitleme kalıbı| Dizin eşitlemesi| Kullanıcı sağlama |
| - | - | - |
| [Dizin eşitlemesi](sync-directory.md)| ![onay işareti](./media/authentication-patterns/check.png)|  |
| [LDAP eşitlemesi](sync-ldap.md)| ![onay işareti](./media/authentication-patterns/check.png)|  |
| [SCIM eşitlemesi](sync-scim.md)| ![onay işareti](./media/authentication-patterns/check.png)| ![onay işareti](./media/authentication-patterns/check.png) |

