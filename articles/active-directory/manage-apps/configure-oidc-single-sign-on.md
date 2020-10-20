---
title: Azure Active Directory uygulamalar için OıDC tabanlı çoklu oturum açma (SSO) anlayın
description: Azure Active Directory uygulamalar için OıDC tabanlı çoklu oturum açma (SSO) anlayın.
services: active-directory
author: kenwith
manager: celestedg
ms.service: active-directory
ms.subservice: app-mgmt
ms.topic: conceptual
ms.workload: identity
ms.date: 10/19/2020
ms.author: kenwith
ms.reviewer: arajpathak7
ms.openlocfilehash: 825f79b0a1c132fb7a15d643c3487dfb7d6a9abd
ms.sourcegitcommit: 957c916118f87ea3d67a60e1d72a30f48bad0db6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/19/2020
ms.locfileid: "92210483"
---
# <a name="understand-oidc-based-single-sign-on"></a>OıDC tabanlı çoklu oturum açmayı anlama
Uygulama yönetiminde [hızlı başlangıç serisinde](view-applications-portal.md) , Azure AD 'yi bir uygulama Için kimlik sağlayıcısı (IDP) olarak kullanmayı öğrendiniz. Bu makale, OpenID Connect standardını kullanan uygulamalar hakkında çoklu oturum açma uygulamak için daha fazla ayrıntıya gider. 

## <a name="before-you-begin"></a>Başlamadan önce
Azure Active Directory kiracınıza uygulama ekleme işlemi, uygulamanın uygulandığı çoklu oturum açma türüne bağlıdır. Kimlik yönetimi için Azure AD kullanan uygulamalar için kullanılabilen çoklu oturum açma seçenekleri hakkında daha fazla bilgi edinmek için bkz. [Çoklu oturum açma seçenekleri](sso-options.md). Bu makalede, OıDC tabanlı uygulamalar ele alınmaktadır.


## <a name="basic-oidc-configuration"></a>Temel OıDC yapılandırması
[Hızlı başlangıç serisinde](add-application-portal-setup-oidc-sso.md), çoklu oturum açmayı yapılandırma hakkında bir makale vardır. Burada, Azure kiracınıza OıDC tabanlı bir uygulama eklemeyi öğreneceksiniz.

Çoklu oturum açma için OıDC standardını kullanan bir uygulama eklemenin en iyi şeyi, yapılandırmanın en düşük düzeyde olmasıdır. Kiracınıza OıDC tabanlı bir uygulamanın nasıl ekleneceğini gösteren kısa bir video aşağıda verilmiştir.

Azure Active Directory bir OıDC tabanlı uygulama ekleme

> [!VIDEO https://www.youtube.com/embed/4kv-upsZCI0]

## <a name="next-steps"></a>Sonraki adımlar

- [Uygulama yönetiminde hızlı başlangıç serisi](add-application-portal-setup-oidc-sso.md)
- [Çoklu oturum açma seçenekleri](sso-options.md)
- [Nasıl yapılır: Çok kiracılı uygulama desenini kullanarak istediğiniz bir Azure Active Directory kullanıcısıyla oturum açma](../develop/howto-convert-app-to-be-multi-tenant.md)