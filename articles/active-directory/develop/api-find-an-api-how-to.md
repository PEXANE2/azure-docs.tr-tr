---
title: Özel olarak geliştirilmiş bir uygulama için API bulun | Azure
description: Özel olarak geliştirilmiş Azure AD uygulamanızda belirli bir API'ye erişmek için gereken izinleri yapılandırma
services: active-directory
author: rwike77
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.topic: conceptual
ms.date: 06/28/2019
ms.author: ryanwi
ms.openlocfilehash: cd3b21050c6a442284647212fdf7c5707943ffc1
ms.sourcegitcommit: d187fe0143d7dbaf8d775150453bd3c188087411
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80885625"
---
# <a name="how-to-find-a-specific-api-needed-for-a-custom-developed-application"></a>Özel olarak geliştirilmiş bir uygulama için gereken belirli bir API nasıl bulabilirim?

API'lere erişim, erişim kapsamlarının ve rollerinin yapılandırmasını gerektirir. Kaynak uygulama web API'lerinizi istemci uygulamalarına maruz bırakmak istiyorsanız, API için erişim kapsamlarını ve rollerini yapılandırmanız gerekir. Bir istemci uygulamasının bir web API'sine erişmesi için, uygulama kaydındaki API'ye erişmek için izinleri yapılandırmanız gerekir.

## <a name="configuring-a-resource-application-to-expose-web-apis"></a>Kaynak uygulamasını web API'lerini kullanıma sunacak şekilde yapılandırma

Web API'nizi ortaya çıkardığınızda, api bir uygulama kaydına izin eklerken **API Seç** listesinde görüntülenir. Erişim kapsamları eklemek [için, web API'lerini ortaya çıkarmak için uygulamayı Yapılandır'da](quickstart-configure-app-expose-web-apis.md)özetlenen adımları izleyin.

## <a name="configuring-a-client-application-to-access-web-apis"></a>Web API'lerine erişmek için istemci uygulamasını yapılandırma

Uygulama kaydınıza izin ler eklediğinizde, açığa çıkan web API'lerine **API erişimi ekleyebilirsiniz.** Web API'lerine erişmek [için, web API'lerine erişmek için bir istemci uygulamasını yapılandırıldığında](quickstart-configure-app-access-web-apis.md)özetlenen adımları izleyin.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Active Directory uygulama bildirimini anlama](https://docs.microsoft.com/azure/active-directory/develop/active-directory-application-manifest)
