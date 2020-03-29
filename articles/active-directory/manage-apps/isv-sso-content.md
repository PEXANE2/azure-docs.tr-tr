---
title: Çok kiracılı uygulamanız için SSO’yu etkinleştirme
description: Azure etkin Dizin ile tümleştirme konusunda bağımsız yazılım satıcıları için kılavuz
services: active-directory
author: barbaraselden
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.topic: conceptual
ms.workload: identity
ms.date: 05/22/2019
ms.author: baselden
ms.reviewer: jeeds
ms.collection: M365-identity-device-management
ms.openlocfilehash: 4c89a83ade6305579e700afb86f0b9e3aca2695e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "67795175"
---
# <a name="enable-single-sign-on-for-your-multi-tenant-application"></a>Çok kiracılı uygulamanız için Tek Oturum Açma'yı etkinleştirin  

Uygulamanızı bir satın alma veya abonelik yoluyla diğer şirketler tarafından kullanılmak üzere sunduğunuzda, uygulamanızı kendi Azure kiracıları içindeki müşterilerin kullanımına sunarsınız. Bu, çok kiracılı bir uygulama oluşturma olarak bilinir. Bu konsepte genel bakış için [Azure'daki Çok Kiracılı Uygulamalar](https://docs.microsoft.com/azure/dotnet-develop-multitenant-applications) ve [Azure Etkin Dizini'nde Kiraya Vereme'ye](../develop/single-and-multi-tenant-apps.md)bakın.

## <a name="what-is-single-sign-on"></a>Tek İşaret-On Nedir

Tek oturum açma (SSO), kullanıcılar Azure Active Directory ve diğer kimlikleri kullanarak uygulamalarda oturum açtıklarında güvenlik ve kolaylık sağlar. Bir uygulama SSO etkinleştirildiğinde, kullanıcıların bu uygulamaya erişmek için ayrı kimlik bilgileri girmeleri gerekmez. Tek oturum açmanın tam bir açıklaması için. [Azure Etkin Dizini'ndeki uygulamalarda tek oturum](what-is-single-sign-on.md)açma'ya bakın.

## <a name="why-enable-single-sign-on-in-your-application"></a>Uygulamanızda Neden Tek Oturum Açma'yı Etkinleştirin?

Çok kiracılı uygulamanızda SSO'yu etkinleştirmenin birçok avantajı vardır. Uygulamanız için SSO'yu etkinleştirdiğinizde:

* Uygulamanız, uygulamanızın Azure Active Directory'yi kullanan milyonlarca kuruluş tarafından bulunabildiği Azure Marketi'nde listelenebilir.
  * Müşterilerin uygulamayı Azure AD ile hızlı bir şekilde yapılandırmasını sağlar.

* Uygulamanız Office 365 App Gallery, Office 365 App Launcher ve Microsoft Search içinde Office.com

* Uygulamanız, Microsoft Graph'ta bulunan kullanıcı üretkenliğini artıran verilere erişmek için Microsoft Graph REST API'sini kullanabilir.

* Müşterileriniz için daha kolay hale getirerek destek maliyetlerini düşürürsunuz.
  * Ortak müşterilerimiz için Azure AD ekibiyle birlikte üretilen uygulamaya özel belgeler benimsenmesini kolaylaştırır.
  * Tek tıklatma SSO etkinse, müşterilerinizin BT Yöneticileri'nin uygulamanızı kuruluşlarında kullanılmak üzere nasıl yapılandıracaklarını öğrenmesi gerekmez.

* Müşterilerinize çalışan ve konuk kimliklerinin kimlik doğrulamasını ve yetkilendirmesini tamamen yönetme olanağı sağlarsınız.

  * Tüm hesap yönetimi ve uyumluluk sorumluluğunu niçin bu kimliklerin müşteri sahibine yükleniyor.

  * Belirli kimlik sağlayıcıları, grupları veya kullanıcıların iş gereksinimlerini karşılaması için SSO'ları etkinleştirme veya devre dışı etme olanağı sağlar.

* Pazarlanabilirliğinizi ve benimsenebilirliğinizi arttırırsınız. Birçok büyük kuruluş, çalışanlarının tüm uygulamalarda sorunsuz SSO deneyimlerine sahip olmasını (veya talip olmasını) gerektirir. SSO'nun kolay hale getirilmesi önemlidir.

* Son kullanıcı sürtünmesini azaltırsınız, bu da son kullanıcı kullanımını artırabilir ve gelirinizi artırır.

## <a name="how-to-enable-single-sign-on-in-your-published-application"></a>Yayınlanan uygulamanızda Tek Oturum Açma'yı etkinleştirme

1. [Çok kiracılı uygulamanız için doğru federasyon protokolünü seçin.](isv-choose-multi-tenant-federation.md)
1. Uygulamanızda SSO'yu uygulayın
   - [Kimlik doğrulama desenleri hakkındaki yönergeye](../develop/v2-app-types.md) bakın
   - OIDC ve OAuth protokolleri için [Azure etkin Dizin kodu örneklerine](../develop/sample-v2-code.md) bakın
1. [Azure Kiracınızı oluşturun](isv-tenant-multi-tenant-app.md) ve uygulamanızı test edin
1. [Sitenizde SSO belgeleri oluşturun ve yayınlayın.](isv-create-sso-documentation.md)
1. Microsoft'un sitesinde belgeler oluşturmak için [başvuru kaydınızı](https://microsoft.sharepoint.com/teams/apponboarding/Apps/SitePages/Default.aspx) ve iş ortağınızı Microsoft ile gönderin.
1. [Microsoft İş Ortağı Ağı'na katılın (ücretsiz) ve pazar agit planınızı oluşturun.](https://partner.microsoft.com/en-us/explore/commercial#gtm)
