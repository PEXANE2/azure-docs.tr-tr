---
title: Microsoft kimlik platformu kimlik doğrulama protokolleri
description: Microsoft kimlik platformu tarafından desteklenen kimlik doğrulama protokollerine genel bakış
author: rwike77
services: active-directory
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 12/18/2019
ms.author: ryanwi
ms.custom: aaddev
ms.reviewer: hirsin
ms.openlocfilehash: 41ea41b4d7c181dad9246653a68c329387ac5381
ms.sourcegitcommit: d187fe0143d7dbaf8d775150453bd3c188087411
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80884690"
---
# <a name="microsoft-identity-platform-authentication-protocols"></a>Microsoft kimlik platformu kimlik doğrulama protokolleri

Microsoft kimlik platformu, en yaygın olarak kullanılan kimlik doğrulama ve yetkilendirme protokollerinden birkaçını destekler. Bu bölümdeki konular, desteklenen protokolleri ve bunların Microsoft kimlik platformunda uygulanmasını açıklar. Konular desteklenen talep türlerinin gözden geçirilmesi, federasyon meta verilerinin kullanımına giriş, ayrıntılı OAuth 2.0 dahil. ve SAML 2.0 protokolü başvuru belgeleri ve sorun giderme bölümü.

## <a name="authentication-protocols-articles-and-reference"></a>Kimlik doğrulama protokolleri makaleleri ve başvurusu

* [Microsoft kimlik platformunda Anahtar Rollover'ı İmzalama Hakkında Önemli Bilgiler](active-directory-signing-key-rollover.md) – Microsoft kimlik platformunun imzalama anahtarı rollover cadence'ı, anahtarı otomatik olarak güncelleştirmek için yapabileceğiniz değişiklikler ve en yaygın uygulama senaryolarını nasıl güncelleştirebileceğiniz hakkında bilgi edinin.
* [Desteklenen Belirteç ve Talep Türleri](id-tokens.md) - Microsoft kimlik platformunun sorunlarının belirteçleri ile ilgili iddialar hakkında bilgi edinin.
* [Microsoft kimlik platformunda OAuth 2.0](v2-oauth2-auth-code-flow.md) - Microsoft kimlik platformunda OAuth 2.0 uygulaması hakkında bilgi edinin.
* [OpenID Connect 1.0](v2-protocols-oidc.md) - Kimlik doğrulama için bir yetkilendirme protokolü olan OAuth 2.0'ı nasıl kullanacağınızı öğrenin.
* [Müşteri Kimlik Bilgilerine Sahip Hizmet Çağrılarına](v2-oauth2-client-creds-grant-flow.md) Hizmet - Servis çağrılarına hizmet için Hizmet için OAuth 2.0 istemci kimlik bilgilerini hibe akışını nasıl kullanacağınızı öğrenin.
* [On-Behalf-Of Flow ile Servis Çağrıları](v2-oauth2-on-behalf-of-flow.md) - Servis çağrılarına hizmet için OAuth 2.0 On-Behalf-Of akış akışını nasıl kullanacağınızı öğrenin.
* [SAML Protocol Reference](active-directory-saml-protocol-reference.md) - Microsoft kimlik platformunun Tek Oturum Açma ve Tek Oturum Açma SAML profilleri hakkında bilgi edinin.

## <a name="see-also"></a>Ayrıca bkz.

* [Microsoft kimlik platformuna genel bakış](v2-overview.md)
* [Aktif Dizin Kodu Örnekleri](sample-v2-code.md)
