---
title: Özel olarak geliştirilen uygulama için bir API bulun | Mavisi
description: Özel olarak geliştirilmiş Azure AD uygulamanızda belirli bir API 'ye erişmek için gereken izinleri yapılandırma
services: active-directory
author: rwike77
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.topic: conceptual
ms.date: 06/28/2019
ms.author: ryanwi
ROBOTS: NOINDEX
ms.openlocfilehash: 4a108f8f5c5b333a9fc7e6eabbfa516b6578ca76
ms.sourcegitcommit: 2488894b8ece49d493399d2ed7c98d29b53a5599
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98063833"
---
# <a name="how-to-find-a-specific-api-needed-for-a-custom-developed-application"></a>Özel olarak geliştirilen bir uygulama için gerekli olan belirli bir API 'yi bulma

API 'lere erişim için erişim kapsamları ve rollerinin yapılandırılması gerekir. Kaynak uygulama Web API 'lerinizi istemci uygulamalarına sunmak istiyorsanız, API için erişim kapsamlarını ve rolleri yapılandırmanız gerekir. Bir istemci uygulamasının bir Web API 'sine erişmesini istiyorsanız, uygulama kaydında API 'ye erişmek için izinleri yapılandırmanız gerekir.

## <a name="configuring-a-resource-application-to-expose-web-apis"></a>Kaynak uygulamasını web API'lerini kullanıma sunacak şekilde yapılandırma

Web API 'nizi kullanıma sunabileceğiniz API, uygulama kaydına izinler eklenirken **API Seç** listesinde görüntülenir. Erişim kapsamları eklemek için, [Web API 'lerini kullanıma sunmak için uygulama yapılandırma](quickstart-configure-app-expose-web-apis.md)bölümünde özetlenen adımları izleyin.

## <a name="configuring-a-client-application-to-access-web-apis"></a>Web API 'Lerine erişmek için bir istemci uygulamasını yapılandırma

Uygulama kaydınız için izinler eklediğinizde, sunulan Web API 'Lerine **API erişimi ekleyebilirsiniz** . Web API 'Lerine erişmek için, [Web API 'lerine erişmek için istemci uygulaması yapılandırma](quickstart-configure-app-access-web-apis.md)bölümünde özetlenen adımları izleyin.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Active Directory uygulama bildirimini anlama](./reference-app-manifest.md)