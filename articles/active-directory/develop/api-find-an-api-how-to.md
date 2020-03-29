---
title: Özel olarak geliştirilmiş bir uygulama için API bulun | Azure
description: Özel olarak geliştirilmiş Azure AD uygulamanızda belirli bir API'ye erişmek için gereken izinleri yapılandırma
services: active-directory
author: rwike77
manager: CelesteDG
ms.assetid: ''
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.topic: conceptual
ms.date: 06/28/2019
ms.author: ryanwi
ms.openlocfilehash: bc50ec86866b7fe04c549c7fd463b6de4df3444b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76698399"
---
# <a name="how-to-find-a-specific-api-needed-for-a-custom-developed-application"></a>Özel olarak geliştirilmiş bir uygulama için gereken belirli bir API nasıl bulabilirim?

API'lere erişim, erişim kapsamlarının ve rollerinin yapılandırmasını gerektirir. Kaynak uygulama web API'lerinizi istemci uygulamalarına maruz bırakmak istiyorsanız, API için erişim kapsamlarını ve rollerini yapılandırmanız gerekir. Bir istemci uygulamasının bir web API'sine erişmesi için, uygulama kaydındaki API'ye erişmek için izinleri yapılandırmanız gerekir.

## <a name="configuring-a-resource-application-to-expose-web-apis"></a>Kaynak uygulamasını web API'lerini kullanıma sunacak şekilde yapılandırma

Web API'nizi ortaya çıkardığınızda, api bir uygulama kaydına izin eklerken **API Seç** listesinde görüntülenir. Erişim kapsamları eklemek [için, web API'lerini ortaya çıkarmak için uygulamayı Yapılandır'da](quickstart-configure-app-expose-web-apis.md)özetlenen adımları izleyin.

## <a name="configuring-a-client-application-to-access-web-apis"></a>Web API'lerine erişmek için istemci uygulamasını yapılandırma

Uygulama kaydınıza izin ler eklediğinizde, açığa çıkan web API'lerine **API erişimi ekleyebilirsiniz.** Web API'lerine erişmek [için, web API'lerine erişmek için bir istemci uygulamasını yapılandırıldığında](quickstart-configure-app-access-web-apis.md)özetlenen adımları izleyin.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Active Directory uygulama bildirimini anlama](https://docs.microsoft.com/azure/active-directory/develop/active-directory-application-manifest)
