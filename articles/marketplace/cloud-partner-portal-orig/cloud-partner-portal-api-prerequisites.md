---
title: API Ön Koşulları | Azure Marketi
description: Önkoşullar, Bulut İş Ortağı Portalı API'lerini kullanarak fro.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 04/08/2020
ms.author: dsindona
ms.openlocfilehash: df94cba1f77ae3ea8cf595e7c651af7a69108bb6
ms.sourcegitcommit: 8dc84e8b04390f39a3c11e9b0eaf3264861fcafc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/13/2020
ms.locfileid: "81255983"
---
<a name="api-prerequisites"></a>API Ön Koşulları
================

> [!NOTE]
> Bulut İş Ortağı Portalı API'leri İş Ortağı Merkezi ile entegre edilmiştir ve teklifleriniz İş Ortağı Merkezi'ne geçtikten sonra çalışmaya devam edecektir. Entegrasyon küçük değişiklikler sunar. Kodunuzu İş Ortağı Merkezi'ne geçişten sonra çalışmaya devam etmesini sağlamak için [Bulut İş Ortağı Portalı API Başvurusu'nda](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal-orig/cloud-partner-portal-api-overview) listelenen değişiklikleri gözden geçirin.

Bulut İş Ortağı Portal API'lerini kullanmanız için gereken iki programatik varlık vardır: bir hizmet yöneticisi ve Bir Azure Etkin Dizin (Azure AD) erişim belirteci.


<a name="create-a-service-principal-in-your-azure-active-directory-tenant"></a>Azure Etkin Dizin kiracınızda bir hizmet ilkesi oluşturma
----------------------------------------------------------------

Öncelikle, Azure AD kiracınızda bir hizmet ilkesi oluşturmanız gerekir. Bu kiracıya Bulut İş Ortağı Portalı'nda kendi izinleri atanır. Kodunuz, kişisel kimlik bilgilerinizi kullanmak yerine bu kiracı olarak kullanan API'leri çağırır.  Bir hizmet ilkesi oluşturmanın tam açıklaması için, [kaynaklara erişebilen bir Azure Etkin Dizin uygulaması ve hizmet ilkesi oluşturmak için portalı kullan'a](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-create-service-principal-portal)bakın.


<a name="add-the-service-principal-to-your-account"></a>Hizmet ilkesini hesabınıza ekleme
-----------------------------------------

Kiracınızda hizmet ilkesini oluşturduğunuza göre, bunu Bulut İş Ortağı Portalı hesabınıza kullanıcı olarak ekleyebilirsiniz. Tıpkı bir kullanıcı gibi, hizmet sorumlusu da portalın sahibi veya katılımcısı olabilir.

Hizmet ilkesini eklemek için aşağıdaki adımları kullanın:

1. Bulut İş Ortağı Portalı'nda oturum açın. 
2. Sol menü çubuğundaki **Kullanıcılar'a** tıklayın ve **Kullanıcı Ekle'yi**seçin.

   ![Portala kullanıcı ekleme](./media/cloud-partner-portal-api-prerequisites/add-user.jpg)

3. **Tür** açılır tarihinden **Hizmet Sorumlusu'nu** seçin ve aşağıdaki ayrıntıları ekleyin:

-   Hizmet sorumlusu için bir Friendly `spAccount` **Name,** örneğin.
-   **Başvuru Kimliği**. Bu tanımlayıcıyı bulmak için [Azure portalına](https://portal.azure.com)gidin , **Azure Active Directory'yi**tıklatın, **Uygulama kayıtlarını**seçin ve uygulamanızı tıklatın.
-   Azure AD kiracınız için **Dizin Kimliği**olarak da bilinen Kiracı **Kimliği.** Bu tanımlayıcıyı Azure Etkin Dizin sayfasında, **Özellikler** [altında, Azure portalında](https://portal.azure.com)bulabilirsiniz.
-   Hizmet ana nesnenizin **Nesne Kimliği.** Bu tanımlayıcıyı Azure portalından alabilirsiniz. Azure **Active Directory'ye**gidin, **Uygulama kayıtlarını**seçin, uygulamanızı tıklayın ve **yerel dizinde Yönetilen uygulama**altında uygulama adını tıklayın. Ardından, Nesne Kimliğini bulmak için **Özellikler** sayfasına gidin. Uygulamanızdaki ilk Nesne Kimliğini değil, yönetilen uygulamadaki Nesne Kimliğini aldığınızdan emin olun.
-   RBAC için kullanılacak hesapla ilişkili **rol.**

     ![Portala yönetilen bir uygulama ekleme](./media/cloud-partner-portal-api-prerequisites/managedapp.png)

1. Hizmet ilkesini hesabınıza eklemek için **Ekle'yi** tıklatın.

   ![Hizmet ilkesi ekleme](./media/cloud-partner-portal-api-prerequisites/add-service-principal.jpg)


<a name="get-an-azure-ad-access-token"></a>Azure AD erişim belirteci alma
----------------------------

Bulut İş Ortağı Portalı API'leri kimlik doğrulama sırasında aşağıdaki varlıkları ve protokolleri kullanır:

- Kaynaklara erişim istemek için bir JSON Web Belirteci (JWT) taşıyıcısı belirteci
- Kimliği doğrulamak için [OpenID Connect](https://openid.net/connect/) (OIDC) protokolü
- Kimlik yetkilisi olarak [Azure Active Directory (Azure AD)](https://docs.microsoft.com/azure/active-directory/active-directory-whatis)

Programlı bir JWT belirteci edinme için iki ilke yaklaşımı vardır:

- .NET ([MSAL.NET](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet)için Microsoft Kimlik Doğrulama Kitaplığını kullanın.  Bu üst düzey yaklaşım .NET geliştiricileri için önerilir. 
- Formu alan Azure AD oauth **belirteç** bitiş noktasına **bir HTTP POST** isteğinde bulunun:

``` HTTP
POST https://login.microsoftonline.com/<tenant-id>/oauth2/token
    client_id: <application-id>
    client_secret:<application-secret>
    grant_type: client_credentials
    resource: https://cloudpartner.azure.com
```

Şimdi, bu belirteci API istekleri için yetkilendirme üstbilgisinin bir parçası olarak geçirebilirsiniz.

``` HTTP
GET https://cloudpartner.azure.com/api/offerTypes?api-version=2016-08-01-preview 
    Accept: application/json
    Authorization: Bearer <access-token>
```
> [!NOTE]
> Bu başvurudaki tüm API'ler için, yetkilendirme üstbilgisinin her zaman geçtiği varsayıldığından açıkça belirtilmez.

İsteğinizde kimlik doğrulama hatalarıyla karşılanırsanız, [Sorun Giderme kimlik doğrulama hatalarına](./cloud-partner-portal-api-troubleshooting-authentication-errors.md)bakın.
