---
title: .NET MVC projelerinde Azure AD ile başlayın | Azure
description: Visual Studio'ya bağlı hizmetleri kullanarak bir Azure REKLAM'ına bağlandıktan veya oluşturulduktan sonra .NET MVC projelerinde Azure Active Directory'yi kullanmaya nasıl başlarsınız?
author: ghogen
manager: jillfra
ms.prod: visual-studio-windows
ms.technology: vs-azure
ms.workload: azure-vs
ms.topic: conceptual
ms.date: 03/12/2018
ms.author: ghogen
ms.custom: aaddev, vs-azure
ms.openlocfilehash: fe408e965c528db1d82b73ee7b20bbe3b3933657
ms.sourcegitcommit: d187fe0143d7dbaf8d775150453bd3c188087411
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80886135"
---
# <a name="getting-started-with-azure-active-directory-aspnet-mvc-projects"></a>Azure Etkin Dizin (mvc projeleri ASP.NET) ile başlarken

> [!div class="op_single_selector"]
> - [Başlarken](vs-active-directory-dotnet-getting-started.md)
> - [Ne oldu](vs-active-directory-dotnet-what-happened.md)

Bu makale, Visual Studio'nun **Project > Connected Services** komutu aracılığıyla ASP.NET bir MVC projesine Active Directory ekledikten sonra ek kılavuz sağlar. Hizmeti projenize zaten eklemediyseniz, bunu istediğiniz zaman yapabilirsiniz.

MVC [projeme ne olduğunu görün?](vs-active-directory-dotnet-what-happened.md)

## <a name="requiring-authentication-to-access-controllers"></a>Denetleyicilere erişmek için kimlik doğrulama gerektirmesi

Projenizdeki tüm denetleyiciler `[Authorize]` bu özellikile süslenmiştir. Bu öznitelik, bu denetleyicilere erişmeden önce kullanıcının kimliğinin doğrulanmasını gerektirir. Denetleyiciye anonim olarak erişilebilmesi için bu özniteliği denetleyiciden kaldırın. İzinleri daha ayrıntılı bir düzeyde ayarlamak istiyorsanız, özniteliği denetleyici sınıfına uygulamak yerine yetkilendirme gerektiren her yönteme uygulayın.

## <a name="adding-signin--signout-controls"></a>Oturum Açma / Oturum Açma Denetimleri Ekleme

Görünümünüze SignIn/SignOut denetimlerini eklemek için, `_LoginPartial.cshtml` görünümlerinizden birine işlevselliği eklemek için kısmi görünümü kullanabilirsiniz. Aşağıda, standart `_Layout.cshtml` görünüme eklenen işlevsellik örneği verilmiştir. (Sınıf navbar-collapse ile div son öğeye dikkat edin):

```html
<!DOCTYPE html>
 <html>
 <head>
     <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>@ViewBag.Title - My ASP.NET Application</title>
    @Styles.Render("~/Content/css")
    @Scripts.Render("~/bundles/modernizr")
</head>
<body>
    <div class="navbar navbar-inverse navbar-fixed-top">
        <div class="container">
            <div class="navbar-header">
                <button type="button" class="navbar-toggle" data-toggle="collapse" data-target=".navbar-collapse">
                    <span class="icon-bar"></span>
                    <span class="icon-bar"></span>
                    <span class="icon-bar"></span>
                </button>
                @Html.ActionLink("Application name", "Index", "Home", new { area = "" }, new { @class = "navbar-brand" })
            </div>
            <div class="navbar-collapse collapse">
                <ul class="nav navbar-nav">
                    <li>@Html.ActionLink("Home", "Index", "Home")</li>
                    <li>@Html.ActionLink("About", "About", "Home")</li>
                    <li>@Html.ActionLink("Contact", "Contact", "Home")</li>
                </ul>
                @Html.Partial("_LoginPartial")
            </div>
        </div>
    </div>
    <div class="container body-content">
        @RenderBody() 
        <hr />
        <footer>
            <p>&copy; @DateTime.Now.Year - My ASP.NET Application</p>
        </footer>
    </div>
    @Scripts.Render("~/bundles/jquery")
    @Scripts.Render("~/bundles/bootstrap")
    @RenderSection("scripts", required: false)
</body>
</html>
```

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Etkin Dizin için kimlik doğrulama senaryoları](authentication-scenarios.md)
- [ASP.NET bir web uygulamasına Microsoft ile oturum açma ekleme](quickstart-v2-aspnet-webapp.md)
