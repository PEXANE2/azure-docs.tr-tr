---
title: Visual Studio WebApi projelerinde Azure AD ile başlayın
description: Visual Studio'ya bağlı hizmetleri kullanarak bir Azure REKLAM'ına bağlandıktan veya oluşturduktan sonra WebApi projelerinde Azure Etkin Dizini kullanmaya nasıl başlarsınız?
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
ms.sourcegitcommit: d187fe0143d7dbaf8d775150453bd3c188087411
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80886084"
---
# <a name="get-started-with-azure-active-directory-webapi-projects"></a>Azure Etkin Dizini (WebApi projeleri) ile başlayın

> [!div class="op_single_selector"]
> - [Başlarken](vs-active-directory-webapi-getting-started.md)
> - [Ne oldu](vs-active-directory-webapi-what-happened.md)

Bu makale, Visual Studio'nun **Project > Connected Services** komutu aracılığıyla ASP.NET bir WebAPI projesine Active Directory ekledikten sonra ek kılavuz sağlar. Hizmeti projenize zaten eklemediyseniz, bunu istediğiniz zaman yapabilirsiniz.

Bkz. [WebAPI projeme ne oldu?](vs-active-directory-webapi-what-happened.md) bağlı hizmeti eklerken projenizde yapılan değişiklikler için.

## <a name="requiring-authentication-to-access-controllers"></a>Denetleyicilere erişmek için kimlik doğrulama gerektirmesi

Projenizdeki tüm denetleyiciler `[Authorize]` bu özellikile süslenmiştir. Bu öznitelik, bu denetleyiciler tarafından tanımlanan API'lere erişmeden önce kullanıcının kimliğinin doğrulanmamasını gerektirir. Denetleyiciye anonim olarak erişilebilmesi için bu özniteliği denetleyiciden kaldırın. İzinleri daha ayrıntılı bir düzeyde ayarlamak istiyorsanız, özniteliği denetleyici sınıfına uygulamak yerine yetkilendirme gerektiren her yönteme uygulayın.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Etkin Dizin için kimlik doğrulama senaryoları](authentication-scenarios.md)
- [ASP.NET bir web uygulamasına Microsoft ile oturum açma ekleme](quickstart-v2-aspnet-webapp.md)
