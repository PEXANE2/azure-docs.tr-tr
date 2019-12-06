---
title: Azure Active Directory kimlik doğrulama protokolleri | Microsoft Docs
description: Azure Active Directory tarafından desteklenen kimlik doğrulama protokollerine genel bakış (AD)
author: rwike77
services: active-directory
manager: CelesteDG
ms.assetid: 7a838ae2-c24c-4304-b6c0-e77fb888e6c0
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 04/27/2017
ms.author: ryanwi
ms.custom: aaddev
ms.reviewer: hirsin
ms.collection: M365-identity-device-management
ms.openlocfilehash: b370736a5e4994651499716e3a923cf59465ca96
ms.sourcegitcommit: c38a1f55bed721aea4355a6d9289897a4ac769d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/05/2019
ms.locfileid: "74845425"
---
# <a name="azure-active-directory-authentication-protocols"></a>Azure Active Directory kimlik doğrulama protokolleri

Azure Active Directory (Azure AD) en yaygın olarak kullanılan kimlik doğrulama ve yetkilendirme protokollerini destekler. Bu bölümdeki konularda desteklenen protokoller ve bunların Azure AD 'de uygulamaları açıklanır. Konular, desteklenen talep türlerini gözden geçirme, Federasyon meta verilerinin kullanımına giriş, ayrıntılı OAuth 2,0. ve SAML 2,0 protokol başvurusu belgeleri ve sorun giderme bölümü.

## <a name="authentication-protocols-articles-and-reference"></a>Kimlik doğrulama protokolleri makaleleri ve başvurusu

* [Azure AD 'de anahtar geçişi Imzalama hakkında önemli bilgiler](active-directory-signing-key-rollover.md) – Azure AD 'nin imzalama anahtar geçişi temposunda hakkında bilgi edinin, anahtarı otomatik olarak güncelleştirmek için yapabileceğiniz değişiklikler ve en sık kullanılan uygulama senaryolarını güncelleştirme tartışması.
* [Desteklenen belirteç ve talep türleri](v1-id-and-access-tokens.md) -Azure AD sorunlarının bulunduğu belirteçlerdeki talepler hakkında bilgi edinin.
* [Federasyon meta verileri](azure-ad-federation-metadata.md) -Azure AD 'nin oluşturduğu meta veri belgelerini bulmayı ve yorumlanmasını öğrenin.
* [Azure AD 'de oauth 2,0](v1-protocols-oauth-code.md) -Azure AD 'de OAuth 2,0 uygulamasını öğrenin.
* [OpenID Connect 1,0](v1-protocols-openid-connect-code.md) -kimlik doğrulaması için OAuth 2,0, yetkilendirme protokolünü nasıl kullanacağınızı öğrenin.
* [Istemci kimlik bilgileriyle](v1-oauth2-client-creds-grant-flow.md) hizmete hizmet çağrıları-Service to hizmet çağrıları için OAuth 2,0 istemci kimlik bilgileri verme akışını nasıl kullanacağınızı öğrenin.
* Service [to-on Flow ile](v1-oauth2-on-behalf-of-flow.md) hizmetten hizmete çağrılar-hizmet için hizmet çağrıları için OAuth 2,0 ' in nasıl kullanılacağını öğrenin.
* [Saml protokol başvurusu](active-directory-saml-protocol-reference.md) -Azure AD 'Nin çoklu oturum açma ve çoklu oturum açma SAML profilleri hakkında bilgi edinin.

## <a name="see-also"></a>Ayrıca bkz.

* [Geliştirici Kılavuzu Azure Active Directory](v1-overview.md)
* [Active Directory kodu örnekleri](sample-v1-code.md)
