---
title: Visual Studio WebApi projelerinde Azure AD ile çalışmaya başlama
description: Visual Studio bağlı hizmetleri kullanarak bir Azure AD 'ye bağlandıktan veya bağlantı kurulduktan sonra, WebApi projelerinde Azure Active Directory kullanmaya başlama
author: ghogen
manager: jillfra
ms.prod: visual-studio-windows
ms.technology: vs-azure
ms.workload: azure-vs
ms.topic: conceptual
ms.date: 03/12/2018
ms.author: ghogen
ms.custom: aaddev, vs-azure
ms.openlocfilehash: 9a9f8aaa9e3f90e78668d2294ea9959b480e3384
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80886084"
---
# <a name="get-started-with-azure-active-directory-webapi-projects"></a>Azure Active Directory kullanmaya başlama (WebApi projeleri)

> [!div class="op_single_selector"]
> - [Başlarken](vs-active-directory-webapi-getting-started.md)
> - [Ne oldu](vs-active-directory-webapi-what-happened.md)

Bu makalede, Visual Studio 'nun **project > bağlı hizmetler** komutu aracılığıyla bir ASP.net WebAPI projesine Active Directory eklendikten sonra ek rehberlik sunulmaktadır. Hizmeti projenize henüz eklemediyseniz, bunu dilediğiniz zaman yapabilirsiniz.

Bağlı hizmet eklenirken projenizde yapılan değişiklikler için bkz. [WebAPI projem ne oldu?](vs-active-directory-webapi-what-happened.md)

## <a name="requiring-authentication-to-access-controllers"></a>Erişim denetleyicilerine kimlik doğrulaması gerektirme

Projenizdeki tüm denetleyiciler `[Authorize]` özniteliğiyle donatılmıştı. Bu öznitelik, bu denetleyiciler tarafından tanımlanan API 'Lere erişmeden önce kullanıcının kimliğinin doğrulanmasını gerektirir. Denetleyiciye anonim olarak erişilmesine izin vermek için bu özniteliği denetleyiciden kaldırın. İzinleri daha ayrıntılı bir düzeyde ayarlamak istiyorsanız, bu özniteliği denetleyici sınıfına uygulamak yerine yetkilendirme gerektiren her bir yönteme uygulayın.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Active Directory için kimlik doğrulama senaryoları](authentication-scenarios.md)
- [Microsoft 'a bir ASP.NET Web uygulamasına oturum açma ekleme](quickstart-v2-aspnet-webapp.md)
