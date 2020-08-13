---
title: Visual Studio WebApi projelerinde Azure AD ile çalışmaya başlama
description: Visual Studio bağlı hizmetleri kullanarak bir Azure AD 'ye bağlandıktan veya bağlantı kurulduktan sonra, WebApi projelerinde Azure Active Directory kullanmaya başlama
author: ghogen
manager: jillfra
ms.prod: visual-studio-windows
ms.technology: vs-azure
ms.workload: azure-vs
ms.topic: how-to
ms.date: 03/12/2018
ms.author: ghogen
ms.custom: devx-track-csharp, aaddev, vs-azure
ms.openlocfilehash: fb8c1f25c8a22c96679a0310353f474f6b9d5ba5
ms.sourcegitcommit: c28fc1ec7d90f7e8b2e8775f5a250dd14a1622a6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/13/2020
ms.locfileid: "88165439"
---
# <a name="get-started-with-azure-active-directory-webapi-projects"></a>Azure Active Directory kullanmaya başlama (WebApi projeleri)

> [!div class="op_single_selector"]
> - [Başlarken](vs-active-directory-webapi-getting-started.md)
> - [Ne oldu](vs-active-directory-webapi-what-happened.md)

Bu makalede, Visual Studio 'nun **project > bağlı hizmetler** komutu aracılığıyla bir ASP.net WebAPI projesine Active Directory eklendikten sonra ek rehberlik sunulmaktadır. Hizmeti projenize henüz eklemediyseniz, bunu dilediğiniz zaman yapabilirsiniz.

Bağlı hizmet eklenirken projenizde yapılan değişiklikler için bkz. [WebAPI projem ne oldu?](vs-active-directory-webapi-what-happened.md)

## <a name="requiring-authentication-to-access-controllers"></a>Erişim denetleyicilerine kimlik doğrulaması gerektirme

Projenizdeki tüm denetleyiciler özniteliğiyle donatılmıştı `[Authorize]` . Bu öznitelik, bu denetleyiciler tarafından tanımlanan API 'Lere erişmeden önce kullanıcının kimliğinin doğrulanmasını gerektirir. Denetleyiciye anonim olarak erişilmesine izin vermek için bu özniteliği denetleyiciden kaldırın. İzinleri daha ayrıntılı bir düzeyde ayarlamak istiyorsanız, bu özniteliği denetleyici sınıfına uygulamak yerine yetkilendirme gerektiren her bir yönteme uygulayın.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Active Directory için kimlik doğrulama senaryoları](./authentication-vs-authorization.md)
- [Microsoft 'a bir ASP.NET Web uygulamasına oturum açma ekleme](quickstart-v2-aspnet-webapp.md)