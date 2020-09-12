---
title: Kimlik yönetimi için Azure Active Directory kiracınızı kullanarak uygulamaları görüntüleme
description: Kimlik yönetimi için Azure Active Directory kiracınızı kullanarak tüm uygulamaları görüntülemeyi öğrenin.
services: active-directory
author: kenwith
manager: celestedg
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.topic: conceptual
ms.date: 07/11/2017
ms.author: kenwith
ms.openlocfilehash: 955efe45be27fa2f4e738066bb3b69b3604be33a
ms.sourcegitcommit: 5a3b9f35d47355d026ee39d398c614ca4dae51c6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/02/2020
ms.locfileid: "89400730"
---
# <a name="viewing-apps-using-your-azure-ad-tenant-for-identity-management"></a>Kimlik yönetimi için Azure AD kiracınızı kullanarak uygulamaları görüntüleme
[Uygulama yönetiminde hızlı başlangıç serisi](view-applications-portal.md) size temel bilgileri gösterir. Bu durumda, kimlik yönetimi için Azure AD kiracınızı kullanarak tüm uygulamaları görüntülemeyi öğreneceksiniz. Bu makale, bulacağınız uygulama türlerinin biraz daha ayrıntılı bir şekilde bir bölümünü ele alır.

## <a name="why-does-a-specific-application-appear-in-my-all-applications-list"></a>Tüm uygulamalar listesinde neden belirli bir uygulama görünüyor?
**Tüm uygulamalara**filtre uygulandığında **tüm uygulamalar** **listesi** kiracınızdaki tüm hizmet sorumlusu nesnelerini gösterir. Hizmet sorumlusu nesneleri, bu listede çeşitli yollarla görünebilir:
- Uygulama galerisinden herhangi bir uygulamayı eklediğinizde şunlar da dahildir:
   - **Azure AD-kurumsal uygulamalar** – Azure AD portalındaki **Kurumsal uygulamalar** seçeneği kullanılarak kiracınıza eklenen uygulamalar. Genellikle SAML standardı kullanılarak tümleştirilmiş uygulamalar.
   - **Azure AD-uygulama kayıtları** : Azure ad portalındaki **uygulama kayıtları** seçeneği kullanılarak kiracınıza eklenen uygulamalar. Genellikle açık KIMLIK Connect ve OAuth standartlarını kullanan özel geliştirmiş uygulamalar.
   - **Uygulama proxy 'Si uygulamaları** – dışarıdan güvenli çoklu oturum açma sağlamak istediğiniz şirket içi ortamınızda çalışan bir uygulamadır
- İçin kaydolurken veya ' de oturum açarken, Azure Active Directory ile tümleştirilmiş üçüncü taraf bir uygulama. [Smartsheet](https://app.smartsheet.com/b/home) veya [Docusign](https://www.docusign.net/member/MemberLogin.aspx)bir örnektir.
- Microsoft 365 veya Office 365 gibi Microsoft uygulamaları.
- [Uygulama kayıt defteri](https://docs.microsoft.com/azure/active-directory/active-directory-app-registration) 'ni kullanarak özel olarak geliştirilmiş bir uygulama oluşturarak yeni bir uygulama kaydı eklediğinizde
- [V 2.0 uygulama kayıt portalını](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-app-registration) kullanarak özel olarak geliştirilmiş bir uygulama oluşturarak yeni bir uygulama kaydı eklediğinizde
- Bir uygulama eklediğinizde, Visual Studio 'nun [ASP.NET kimlik doğrulama yöntemlerini](https://www.asp.net/visual-studio/overview/2013/creating-web-projects-in-visual-studio#orgauthoptions) veya [bağlı hizmetleri](https://devblogs.microsoft.com/visualstudio/connecting-to-cloud-services/) kullanarak geliştirdiğinizi
- [Azure AD PowerShell modülünü](/powershell/azure/active-directory/install-adv2?view=azureadps-2.0) kullanarak bir hizmet sorumlusu nesnesi oluşturduğunuzda
- [Bir uygulamayı](https://docs.microsoft.com/azure/active-directory/develop/active-directory-devhowto-multi-tenant-overview) kiracınızdaki verileri kullanmak üzere yönetici olarak kabul ettiğinizde
- Bir Kullanıcı kiracınızdaki verileri kullanmak üzere [bir uygulamaya onay verirse](https://docs.microsoft.com/azure/active-directory/develop/active-directory-devhowto-multi-tenant-overview)
- Kiracınızda veri depolayan belirli hizmetleri etkinleştirdiğinizde. Parola sıfırlama ilkenizi güvenli bir şekilde depolamak için bir hizmet sorumlusu olarak modellenen parola sıfırlama bir örnektir.

Uygulamaların nasıl ve neden eklenediğine ilişkin daha fazla bilgi edinin, bkz. [uygulamalar Azure AD 'ye nasıl eklenir](https://docs.microsoft.com/azure/active-directory/develop/active-directory-how-applications-are-added).

## <a name="next-steps"></a>Sonraki adımlar
[Uygulamaları Azure Active Directory ile yönetme](what-is-application-management.md)
