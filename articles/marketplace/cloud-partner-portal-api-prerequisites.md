---
title: API önkoşulları-Azure Marketi
description: Bulut İş Ortağı Portalı API 'Leri kullanma önkoşulları.
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: reference
author: dsindona
ms.author: dsindona
ms.date: 07/14/2020
ms.openlocfilehash: b672afcd8f52d8293b1bb81e30097062e2ee49c3
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87053592"
---
<a name="api-prerequisites"></a>API önkoşulları
================

> [!NOTE]
> Bulut İş Ortağı Portalı API 'Leri ile tümleşiktir ve Iş Ortağı Merkezi 'nde çalışmaya devam edecektir. Geçiş küçük değişiklikler sunar. Iş Ortağı Merkezi 'ne geçtikten sonra kodunuzun çalışmaya devam ettiğinden emin olmak için [bulut iş ortağı PORTALı API başvurusunda](./cloud-partner-portal-api-overview.md) listelenen değişiklikleri gözden geçirin. CPP API 'Leri yalnızca Iş Ortağı Merkezi 'ne geçişten önce tümleştirilmiş mevcut ürünler için kullanılmalıdır; Yeni ürünlerin Iş Ortağı Merkezi gönderme API 'Leri kullanması gerekir.

Bulut İş Ortağı Portalı API 'Leri kullanmanız gereken iki adet gerekli program varlığı vardır: bir hizmet sorumlusu ve bir Azure Active Directory (Azure AD) erişim belirteci.


<a name="create-a-service-principal-in-your-azure-active-directory-tenant"></a>Azure Active Directory kiracınızda hizmet sorumlusu oluşturma
----------------------------------------------------------------

İlk olarak, Azure AD kiracınızda bir hizmet sorumlusu oluşturmanız gerekir. Bu kiracıya, Bulut İş Ortağı Portalı kendi izin kümesi atanır. Kodunuz, kişisel kimlik bilgilerinizi kullanmak yerine bu kiracı olarak kullanan API 'Leri çağırır.  Hizmet sorumlusu oluşturma hakkında tam bir açıklama için bkz. [nasıl yapılır: portalı kullanarak kaynaklara erişebilen bir Azure AD uygulaması ve hizmet sorumlusu oluşturma](../active-directory/develop/howto-create-service-principal-portal.md).


<a name="add-the-service-principal-to-your-account"></a>Hizmet sorumlusunu hesabınıza ekleyin
-----------------------------------------

Kiracınızda hizmet sorumlusunu oluşturduğunuza göre, onu Bulut İş Ortağı Portalı hesabınıza bir kullanıcı olarak ekleyebilirsiniz. Bir kullanıcı gibi, hizmet sorumlusu de Portal için bir sahip veya katılımcı olabilir.

Hizmet sorumlusunu eklemek için aşağıdaki adımları kullanın:

1. Bulut İş Ortağı Portalı oturum açın. 
2. Sol menü çubuğunda **Kullanıcılar** ' a tıklayın ve **Kullanıcı Ekle**' yi seçin.

   ![Portala kullanıcı ekleme](./media/cloud-partner-portal-api-prerequisites/add-user.jpg)

3. **Tür** açılır listesinden **hizmet sorumlusu** ' nı seçin ve aşağıdaki ayrıntıları ekleyin:

-   Hizmet sorumlusu için **kolay bir ad** (örneğin,) `spAccount` .
-   **Uygulama kimliği**. Bu tanımlayıcıyı bulmak için [Azure Portal](https://portal.azure.com)gidin, **Azure Active Directory**' a tıklayın, **uygulama kayıtları**' i seçin ve uygulamanıza tıklayın.
-   Azure AD kiracınız için **DIZIN kimliği**olarak da BILINEN **Kiracı kimliği**. Bu tanımlayıcıyı, [Azure portal](https://portal.azure.com)Azure Active Directory sayfasında, **Özellikler**altında bulabilirsiniz.
-   Hizmet sorumlusu nesnenizin **nesne kimliği** . Bu tanımlayıcıyı Azure portal edinebilirsiniz. **Azure Active Directory**' e gidin, **uygulama kayıtları**' i seçin, uygulamanıza tıklayın ve **yerel dizinde yönetilen uygulama**altında uygulama adı ' na tıklayın. Ardından, nesne KIMLIĞINI bulmak için **Özellikler** sayfasına gidin. Uygulamanızdaki ilk nesne KIMLIĞINI yakalayıp olmadığından ve bunun yerine yönetilen uygulamadaki nesne KIMLIĞI olmadığından emin olun.
-   RBAC için kullanılacak hesapla ilişkili **rol** .

     ![Portala yönetilen uygulama ekleme](./media/cloud-partner-portal-api-prerequisites/managed-app.png)

1. Hizmet sorumlusunu hesabınıza eklemek için **Ekle** ' ye tıklayın.

   ![Hizmet sorumlusu ekleme](./media/cloud-partner-portal-api-prerequisites/add-service-principal.jpg)


<a name="get-an-azure-ad-access-token"></a>Azure AD erişim belirteci alma
----------------------------

Bulut İş Ortağı Portalı API 'Leri, kimlik doğrulaması sırasında aşağıdaki varlıkları ve protokolleri kullanır:

- Kaynaklara erişim istemek için bir JSON Web Token (JWT) taşıyıcı belirteci
- Kimliği doğrulamak için [OpenID Connect](https://openid.net/connect/) (OIDC) protokolü
- Kimlik yetkilisi olarak [Azure Active Directory (Azure AD)](../active-directory/fundamentals/active-directory-whatis.md)

Program aracılığıyla JWT belirtecini almak için iki prensibi yaklaşım vardır:

- .NET için Microsoft kimlik doğrulama kitaplığı 'nı ([msal.net](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet)) kullanın.  Bu üst düzey yaklaşım .NET geliştiricileri için önerilir. 
- Şu formu alan Azure AD OAuth **belirteç** uç noktasına BIR **http post** isteği oluşturun:

``` HTTP
POST https://login.microsoftonline.com/<tenant-id>/oauth2/token
    client_id: <application-id>
    client_secret:<application-secret>
    grant_type: client_credentials
    resource: https://cloudpartner.azure.com
```

Şimdi, API istekleri için yetkilendirme üstbilgisinin bir parçası olarak bu belirteci geçirebilirsiniz.

``` HTTP
GET https://cloudpartner.azure.com/api/offerTypes?api-version=2016-08-01-preview 
    Accept: application/json
    Authorization: Bearer <access-token>
```
> [!NOTE]
> Bu başvurudaki tüm API 'Ler için, yetkilendirme üst bilgisi her zaman geçti olarak kabul edilir, bu nedenle açıkça bahsedilmez.

İstekinizdeki kimlik doğrulama hatalarıyla karşılaşırsanız bkz. [kimlik doğrulama hatalarını giderme](./cloud-partner-portal-api-troubleshooting-authentication-errors.md).
