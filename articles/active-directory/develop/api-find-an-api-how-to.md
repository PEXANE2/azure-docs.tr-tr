---
title: Özel olarak geliştirilen uygulama için bir API bulun | Mavisi
description: Özel olarak geliştirilmiş Azure AD uygulamanızda belirli bir API 'ye erişmek için gereken izinleri yapılandırma
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
ms.sourcegitcommit: af6847f555841e838f245ff92c38ae512261426a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/23/2020
ms.locfileid: "76698399"
---
# <a name="how-to-find-a-specific-api-needed-for-a-custom-developed-application"></a>Özel olarak geliştirilen bir uygulama için gerekli olan belirli bir API 'yi bulma

API 'lere erişim için erişim kapsamları ve rollerinin yapılandırılması gerekir. Kaynak uygulama Web API 'lerinizi istemci uygulamalarına sunmak istiyorsanız, API için erişim kapsamlarını ve rolleri yapılandırmanız gerekir. Bir istemci uygulamasının bir Web API 'sine erişmesini istiyorsanız, uygulama kaydında API 'ye erişmek için izinleri yapılandırmanız gerekir.

## <a name="configuring-a-resource-application-to-expose-web-apis"></a>Kaynak uygulamasını web API'lerini kullanıma sunacak şekilde yapılandırma

Web API 'nizi kullanıma sunabileceğiniz API, uygulama kaydına izinler eklenirken **API Seç** listesinde görüntülenir. Erişim kapsamları eklemek için, [Web API 'lerini kullanıma sunmak için uygulama yapılandırma](quickstart-configure-app-expose-web-apis.md)bölümünde özetlenen adımları izleyin.

## <a name="configuring-a-client-application-to-access-web-apis"></a>Web API 'Lerine erişmek için bir istemci uygulamasını yapılandırma

Uygulama kaydınız için izinler eklediğinizde, sunulan Web API 'Lerine **API erişimi ekleyebilirsiniz** . Web API 'Lerine erişmek için, [Web API 'lerine erişmek için istemci uygulaması yapılandırma](quickstart-configure-app-access-web-apis.md)bölümünde özetlenen adımları izleyin.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Active Directory uygulama bildirimini anlama](https://docs.microsoft.com/azure/active-directory/develop/active-directory-application-manifest)
