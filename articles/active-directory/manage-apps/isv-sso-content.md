---
title: Çok kiracılı uygulamanız için SSO’yu etkinleştirme
description: Bağımsız yazılım satıcılarının Azure Active Directory ile tümleştirilmesine yönelik kılavuz
services: active-directory
author: kenwith
manager: celestedg
ms.service: active-directory
ms.subservice: app-mgmt
ms.topic: conceptual
ms.workload: identity
ms.date: 05/22/2019
ms.author: kenwith
ms.reviewer: jeeds
ms.collection: M365-identity-device-management
ms.openlocfilehash: 26fa2b82b89c7b9ce1e05062718f2c4d0c93197c
ms.sourcegitcommit: bc943dc048d9ab98caf4706b022eb5c6421ec459
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/14/2020
ms.locfileid: "84763287"
---
# <a name="enable-single-sign-on-for-your-multi-tenant-application"></a>Çok kiracılı uygulamanız için çoklu oturum açmayı etkinleştirme  

Uygulamanızı bir satın alma veya abonelik aracılığıyla diğer şirketler tarafından kullanılmak üzere sunmanızdan sonra, uygulamanızı kendi Azure kiracılarındaki müşteriler için kullanılabilir hale getirebilirsiniz. Bu, çok kiracılı bir uygulama oluşturma olarak bilinir. Bu kavram hakkında genel bilgi için bkz. [Azure 'Da çok kiracılı uygulamalar](https://docs.microsoft.com/azure/dotnet-develop-multitenant-applications) ve [Azure Active Directory içinde Kiralama](../develop/single-and-multi-tenant-apps.md).

## <a name="what-is-single-sign-on"></a>Çoklu oturum açma nedir?

Çoklu oturum açma (SSO), kullanıcılar Azure Active Directory ve diğer kimlikleri kullanarak uygulamalarda oturum açtığında güvenlik ve kolaylık sağlar. Bir uygulama SSO etkinken, kullanıcıların bu uygulamaya erişmek için ayrı kimlik bilgileri girmesi gerekmez. Çoklu oturum açma hakkında tam açıklama için. [Bkz. Azure Active Directory uygulamalarda çoklu oturum açma](what-is-single-sign-on.md).

## <a name="why-enable-single-sign-on-in-your-application"></a>Uygulamanızda çoklu oturum açma neden etkinleştirilsin?

Çok kiracılı uygulamanızda SSO 'yu etkinleştirmenin pek çok avantajı vardır. Uygulamanız için SSO 'yu etkinleştirdiğinizde:

* Uygulamanız Azure Marketi 'nde listelenebilir ve bu, uygulamanızın Azure Active Directory kullanan milyonlarca kuruluş tarafından bulunabilir.
  * Müşterilerin uygulamayı Azure AD ile hızlı bir şekilde yapılandırmasına olanak sağlar.

* Uygulamanız Office 365 uygulama galerisinde, Office 365 uygulama başlatıcısı ve Microsoft Search 'te Office.com üzerinde bulunabilir

* Uygulamanız, Microsoft Graph kullanılabilir kullanıcı üretkenliğini yönlendiren verilere erişmek için Microsoft Graph REST API kullanabilir.

* Müşterileriniz için daha kolay hale getirerek destek maliyetlerini azaltırsınız.
  * Karşılıklı müşterilerimiz için Azure AD ekibi ile uygulamaya özgü belge işbirliği, benimsemeyi kolaylaştırır.
  * Tek tıklamayla SSO etkinse, müşterilerinizin BT yöneticileri, uygulamanızın kuruluşunuzda kullanılmak üzere nasıl yapılandırılacağını öğrenmek zorunda kalmaz.

* Müşterilerinizin kendi çalışan ve konuk kimliklerinin kimlik doğrulama ve yetkilendirme işlemlerini tamamen yönetmesine olanak sağlarsınız.

  * Tüm hesap yönetimi ve uyumluluk sorumluluğunun bu kimliklerin müşteri sahibine yerleştirilmesi.

  * Belirli kimlik sağlayıcıları, gruplar veya kullanıcılar için kendi iş ihtiyaçlarını karşılayacak SSO 'yu etkinleştirme veya devre dışı bırakma olanağı sağlar.

* Pazardan uyumluluk ve benimseme düzeyini artırırsınız. Birçok büyük kuruluş, çalışanlarının tüm uygulamalarda sorunsuz SSO deneyimleri yaşamalarına (veya amaçlayın) gerek duyar. SSO 'yu kolay hale getirmek önemlidir.

* Son Kullanıcı artışını azaltırsınız. Bu, Son Kullanıcı kullanımını artırabilir ve gelirinizi artırabilir.

## <a name="how-to-enable-single-sign-on-in-your-published-application"></a>Yayımlanmış uygulamanızda çoklu oturum açmayı etkinleştirme

1. [Çok kiracılı uygulamanız için doğru Federasyon protokolünü seçin](isv-choose-multi-tenant-federation.md).
1. Uygulamanızda SSO uygulama
   - Bkz. [kimlik doğrulama desenlerinde rehberlik](../develop/v2-app-types.md)
   - OıDC ve OAuth protokolleri için bkz. [Azure Active Directory kod örnekleri](../develop/sample-v2-code.md)
1. [Azure kiracınızı oluşturun](isv-tenant-multi-tenant-app.md) ve uygulamanızı test edin
1. [SITENIZDE SSO belgeleri oluşturun ve yayımlayın](isv-create-sso-documentation.md).
1. Microsoft 'un sitesinde belge oluşturmak için [uygulama listelemeyi ve iş ortaklarınızı](https://microsoft.sharepoint.com/teams/apponboarding/Apps/SitePages/Default.aspx) Microsoft Ile birlikte gönderebilirsiniz.
1. [Microsoft iş ortağı ağı (ücretsiz) ekleyin ve pazara git planınızı oluşturun](https://partner.microsoft.com/en-us/explore/commercial#gtm).
