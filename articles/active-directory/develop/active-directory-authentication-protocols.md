---
title: Microsoft Identity platform kimlik doğrulama protokolleri | Microsoft Docs
description: Microsoft Identity platform tarafından desteklenen kimlik doğrulama protokollerine genel bakış
author: rwike77
services: active-directory
manager: CelesteDG
ms.assetid: 7a838ae2-c24c-4304-b6c0-e77fb888e6c0
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 12/18/2019
ms.author: ryanwi
ms.custom: aaddev
ms.reviewer: hirsin
ms.collection: M365-identity-device-management
ms.openlocfilehash: 94917d601de24388600bae3434f1d03a35bffd33
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75424773"
---
# <a name="microsoft-identity-platform-authentication-protocols"></a>Microsoft Identity platform kimlik doğrulama protokolleri

Microsoft Identity platform, en yaygın olarak kullanılan kimlik doğrulama ve yetkilendirme protokollerinden birkaçını destekler. Bu bölümdeki konularda desteklenen protokoller ve bunların uygulamaları Microsoft Identity platformunda açıklanır. Konular, desteklenen talep türlerini gözden geçirme, Federasyon meta verilerinin kullanımına giriş, ayrıntılı OAuth 2,0. ve SAML 2,0 protokol başvurusu belgeleri ve sorun giderme bölümü.

## <a name="authentication-protocols-articles-and-reference"></a>Kimlik doğrulama protokolleri makaleleri ve başvurusu

* [Microsoft Identity platformunda anahtar geçişi Imzalama hakkında önemli bilgiler](active-directory-signing-key-rollover.md) – Microsoft Identity platformunun imzalama anahtarı aktarma temposunda hakkında bilgi edinin, anahtarı otomatik olarak güncelleştirmek için kullanabileceğiniz değişiklikler ve en sık kullanılan uygulama senaryolarını güncelleştirme tartışması.
* [Desteklenen belirteç ve talep türleri](id-tokens.md) -Microsoft Identity platform tarafından karşılaşılan belirteçlerdeki talepler hakkında bilgi edinin.
* [Microsoft Identity platform 'Da oauth 2,0](v2-oauth2-auth-code-flow.md) -Microsoft Identity platform 'da OAuth 2,0 uygulamasının uygulanması hakkında bilgi edinin.
* [OpenID Connect 1,0](v2-protocols-oidc.md) -kimlik doğrulaması için OAuth 2,0, yetkilendirme protokolünü nasıl kullanacağınızı öğrenin.
* [Istemci kimlik bilgileriyle](v2-oauth2-client-creds-grant-flow.md) hizmete hizmet çağrıları-Service to hizmet çağrıları için OAuth 2,0 istemci kimlik bilgileri verme akışını nasıl kullanacağınızı öğrenin.
* Service [to-on Flow ile](v2-oauth2-on-behalf-of-flow.md) hizmetten hizmete çağrılar-hizmet için hizmet çağrıları için OAuth 2,0 ' in nasıl kullanılacağını öğrenin.
* [Saml protokol başvurusu](active-directory-saml-protocol-reference.md) -Microsoft Identity platform 'un çoklu oturum açma ve çoklu oturum açma SAML profilleri hakkında bilgi edinin.

## <a name="see-also"></a>Ayrıca bkz.

* [Microsoft Identity platformuna genel bakış](v2-overview.md)
* [Active Directory kodu örnekleri](sample-v2-code.md)
