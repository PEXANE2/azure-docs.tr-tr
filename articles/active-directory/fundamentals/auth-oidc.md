---
title: Azure Active Directory ile OpenID Connect kimlik doğrulaması
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
ms.openlocfilehash: c6f2ad3b5e86eebfc2d6f1f42f8a2ab0520144b5
ms.sourcegitcommit: ae6e7057a00d95ed7b828fc8846e3a6281859d40
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/16/2020
ms.locfileid: "92114364"
---
# <a name="openid-connect-authentication-with-azure-active-directory"></a>Azure Active Directory ile OpenID Connect kimlik doğrulaması

OpenID Connect (OıDC), OAuth2 protokolüne dayalı bir kimlik doğrulama protokolüdür (yetkilendirme için kullanılır). OıDC, kimlik hizmetleri sağlamak için OAuth2 adresinden standartlaştırılmış ileti akışlarını kullanır. 

OıDC 'in tasarım hedefi "basit şeyleri basit ve karmaşık şeyleri mümkün hale getirme" sağlamaktır. OıDC, geliştiricilerin parola dosyalarına sahip olmak ve bunları yönetmek zorunda kalmadan Web siteleri ve uygulamalar arasında kimliklerini doğrulamalarını sağlar. Bu, uygulama Oluşturucu 'yu, şu anda uygulamaya bağlı olan kişinin kimliğini doğrulamak için güvenli bir yol sağlar.

Kullanıcının kimlik doğrulaması, kullanıcının oturumunun veya kimlik bilgilerinin denetlendiği bir kimlik sağlayıcısında gerçekleşmelidir. Bunu yapmak için güvenilir bir aracıya ihtiyacınız vardır. Yerel uygulamalar genellikle bu amaçla sistem tarayıcısını başlatır. Uygulamanın kullanıcı parolası üzerinde gözetmesini önleyen bir şey olmadığından, gömülü görünümler güvenilir değil olarak kabul edilir. 

Kimlik doğrulamasına ek olarak, kullanıcıdan onay istenebilir. İzin, kullanıcının korumalı kaynaklara erişmesine izin vermek için açık olan izindir. Onay, bir kaynak için yalnızca bir kez sağlanması gerektiğinden, kimlik doğrulamasından farklıdır. Kullanıcı veya yönetici izni el ile iptal edene kadar onay geçerli kalır. 

## <a name="use-when"></a>Şu durumlarda kullanın

Kullanıcı onayı ve Web oturumu açma için bir gereksinim vardır.

![mimari diyagram](./media/authentication-patterns/oidc-auth.png)

## <a name="components-of-system"></a>Sistem bileşenleri

* **Kullanıcı**: uygulamadan bir hizmet ister.

* **Güvenilen aracı**: kullanıcının etkileşime girdiği bileşen. Bu güvenilen aracı genellikle bir web tarayıcısıdır.

* **Uygulama**: uygulama veya kaynak sunucu, kaynak veya verilerin bulunduğu yerdir. Güvenli aracıyı güvenli bir şekilde doğrulamak ve yetkilendirmek için kimlik sağlayıcısına güvenir. 

* **Azure AD**: kimlik sağlayıcısı olarak da bilinen OIDC sağlayıcısı, kullanıcının bilgileri, erişimleri ve bir akıştaki taraflar arasındaki güven ilişkilerini güvenli bir şekilde yönetir. Kullanıcı kimliğinin kimliğini doğrular, kaynaklara erişim izni verir ve erişimi iptal eder ve sorun belirteçleri. 

## <a name="implement-oidc-with-azure-ad"></a>Azure AD ile OıDC uygulama

* [Uygulamaları Azure AD ile tümleştirme](https://docs.microsoft.com/azure/active-directory/saas-apps/tutorial-list) 

* [Microsoft Identity platformunda OAuth 2,0 ve OpenID Connect protokolleri](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-protocols) 

* [Microsoft Identity platform ve OpenID Connect Protokolü](https://docs.microsoft.com/azure/active-directory/develop/v2-protocols-oidc) 

* [Azure Active Directory B2C 'de OpenID Connect ile Web oturumu açma](https://docs.microsoft.com/azure/active-directory-b2c/openid-connect) 

* [OpenID Connect ve Azure AD kullanarak uygulamanızın güvenliğini sağlama](https://docs.microsoft.com/learn/modules/secure-app-with-oidc-and-azure-ad/) 

 
