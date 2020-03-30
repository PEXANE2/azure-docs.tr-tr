---
title: Kimlik doğrulamayı yönetme | Microsoft Azure Haritaları
description: Microsoft Azure Haritalar'da kimlik doğrulamasını yönetmek için Azure portalını kullanın.
author: philmea
ms.author: philmea
ms.date: 01/29/2020
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.openlocfilehash: dfe73971f29ea362fdd0ddd654e705b622ab1866
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80335537"
---
# <a name="manage-authentication-in-azure-maps"></a>Azure Haritalar'da kimlik doğrulamayı yönetme

Bir Azure Haritalar hesabı oluşturduktan sonra, Azure Etkin Dizin (Azure AD) kimlik doğrulaması ve Paylaşılan Anahtar kimlik doğrulamasını desteklemek için bir istemci kimliği ve anahtarları oluşturulur.

## <a name="view-authentication-details"></a>Kimlik doğrulama ayrıntılarını görüntüleme

Bir Azure Haritalar hesabı oluşturduktan sonra, birincil ve ikincil anahtarlar oluşturulur. Azure Haritalar'ı aramak için Paylaşılan Anahtar [kimlik doğrulamasını kullandığınızda](https://docs.microsoft.com/azure/azure-maps/azure-maps-authentication#shared-key-authentication)birincil anahtarı abonelik anahtarı olarak kullanmanızı öneririz. Anahtar değişikliklerini yuvarlama gibi senaryolarda ikincil bir anahtar kullanabilirsiniz. Daha fazla bilgi için [Azure Haritalar'da Kimlik Doğrulama'ya](https://aka.ms/amauth)bakın.

Kimlik doğrulama bilgilerinizi Azure portalında görüntüleyebilirsiniz. Burada, hesabınızda Ayarlar **menüsünde** **Kimlik Doğrulama'yı**seçin.

![Kimlik doğrulama ayrıntıları](./media/how-to-manage-authentication/how-to-view-auth.png)


## <a name="register-and-configure-an-azure-ad-app"></a>Azure AD uygulamasını kaydetme ve yapılandırma

1. Azure portalında, Azure hizmetleri listesinde **Azure Active Directory** > **App kayıtlarını** > seçin**Yeni kayıt**.  

    ![Uygulama kaydı](./media/how-to-manage-authentication/app-registration.png)

1. Uygulamanızı zaten kaydettiyseniz, bir sonraki adıma devam edin. Uygulamanızı kaydetmediyseniz, bir **Ad**girin, **Destek hesabı türünü**seçin ve ardından **Kaydol'u**seçin.  

    ![Uygulama kayıt bilgileri](./media/how-to-manage-authentication/app-create.png)

1. Azure Haritalar'a devredilen API izinlerini atamak için uygulamaya gidin. Sonra **Uygulama kayıtları**altında, **API izinleri** > seçin**bir izin ekleyin.** **Kuruluşum kullandığı API'ler**altında **Azure Haritalarını**arayın ve seçin.

    ![Uygulama API izinleri ekleme](./media/how-to-manage-authentication/app-permissions.png)

1. Azure Haritalar'a **Eriş'in**yanındaki onay kutusunu seçin ve ardından **İzin Ekle'yi**seçin.

    ![Uygulama API izinlerini seçin](./media/how-to-manage-authentication/select-app-permissions.png)

1. Kimlik doğrulama yönteminize bağlı olarak aşağıdaki adımlardan birini tamamlayın. 

    * Uygulamanız Azure Haritalar Web SDK ile kullanıcı belirteç `oauth2AllowImplicitFlow`kimlik doğrulaması kullanıyorsa, etkinleştirin. Etkinleştirmek için, uygulama kaydınızın **Bildirim** bölümünde `oauth2AllowImplicitFlow` doğru olarak ayarlayın. 
    
       ![Uygulama bildirimi](./media/how-to-manage-authentication/app-manifest.png)

    * Uygulamanız sunucu veya uygulama kimlik doğrulaması kullanıyorsa, uygulama kayıt sayfanızda **Sertifikalar & sırlarına**gidin. Ardından ortak anahtar sertifikası yükleyin veya **Yeni istemci sırrını**seçerek bir parola oluşturun. 
    
       ![İstemci sırrı oluşturma](./media/how-to-manage-authentication/app-keys.png)

        Bir parola oluşturursanız, **Ekle'yi**seçtikten sonra parolayı kopyalayın ve güvenli bir şekilde saklayın. Azure AD'den jeton almak için bu parolayı kullanırsınız.

       ![İstemci sırrı ekleme](./media/how-to-manage-authentication/add-key.png)


## <a name="grant-role-based-access-control-to-azure-maps"></a>Azure Haritalar'da rol tabanlı erişim denetimi yapma

Azure Haritalar hesabını Azure AD kiracınızla ilişkilendirdikten sonra erişim denetimi izni verebilirsiniz. Bir veya daha fazla Azure Açıkları erişim denetimi rolüne bir kullanıcı, grup veya uygulama atayarak *rol tabanlı erişim denetimi* (RBAC) verirsiniz. 

1. **Azure Haritalar Hesabınıza**gidin. **Access denetimi (IAM)** > **Rol ataması'nı**seçin.

    ![Hibe RBAC](./media/how-to-manage-authentication/how-to-grant-rbac.png)

1. Rol **atamaları** sekmesinde, **Rol**altında, **Azure Haritalar Tarih Okuyucu (Önizleme) seçeneğini**belirleyin. **Access'e ata nın**altında Azure **AD kullanıcısını, grubunu veya hizmet sorumlusunu**seçin. Kullanıcıyı veya uygulamayı seçin. Ardından **Kaydet'i**seçin.

    ![Rol ataması ekle](./media/how-to-manage-authentication/add-role-assignment.png)

## <a name="view-available-azure-maps-rbac-roles"></a>Kullanılabilir Azure Haritalar RBAC rollerini görüntüleme

Azure Haritalar'da kullanılabilen RBAC rollerini görüntülemek için **Access denetimine (IAM)** gidin. **Roller'i**seçin ve ardından *Azure Haritalar'la*başlayan rolleri arayın. Bu Azure Haritalar rolleri, erişim izni verebileceğiniz rollerdir.

![Kullanılabilir rolleri görüntüleme](./media/how-to-manage-authentication/how-to-view-avail-roles.png)


## <a name="view-azure-maps-rbac"></a>Azure Haritalar RBAC'ı Görüntüle

RBAC parçalı erişim denetimi sağlar.

Azure Haritalar için RBAC verilen kullanıcıları ve uygulamaları görüntülemek için **Access Control 'e (IAM)** gidin. Burada, **Rol atamaları'nı**seçin ve ardından **Azure Haritalar'a**göre filtre uygulayın.

![RBAC verilen kullanıcıları ve uygulamaları görüntüleme](./media/how-to-manage-authentication/how-to-view-amrbac.png)


## <a name="request-tokens-for-azure-maps"></a>Azure Haritalar için belirteçleri isteme

Uygulamanızı kaydettikten ve Azure Haritalar ile ilişkilendirdikten sonra, erişim belirteçleri talep edebilirsiniz.

Uygulamanız Azure Haritalar Web SDK ile kullanıcı belirteç kimlik doğrulaması kullanıyorsa, HTML sayfanızı Azure Haritalar istemci kimliği ve Azure AD uygulama kimliğiyle yapılandırın.

Uygulamanız sunucu veya uygulama kimlik doğrulaması kullanıyorsa, Azure AD `https://login.microsoftonline.com`belirteci bitiş noktasından bir belirteç isteyin. İsteğiniz için aşağıdaki ayrıntıları kullanın: 

* Azure AD kaynak kimliği`https://atlas.microsoft.com/`
* Azure Haritalar istemci kimliği
* Azure AD uygulama kimliği
* Azure AD uygulama kayıt parolası veya sertifikası

| Azure ortamı   | Azure AD belirteç bitiş noktası | Azure kaynak kimliği |
| --------------------|-------------------------|-------------------|
| Azure genel bulutu        | `https://login.microsoftonline.com` | `https://atlas.microsoft.com/` |
| Azure Devlet bulutu   | `https://login.microsoftonline.us`  | `https://atlas.microsoft.com/` | 

Kullanıcılar ve hizmet ilkeleri için Azure AD'den erişim belirteçleri isteme hakkında daha fazla bilgi için Azure [AD için Kimlik Doğrulama senaryolarına](https://docs.microsoft.com/azure/active-directory/develop/authentication-scenarios)bakın.


## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi için [Azure AD ve Azure Haritalar Web SDK'ya](https://docs.microsoft.com/azure/azure-maps/how-to-use-map-control)bakın.

Azure Haritalar hesabınız için API kullanım ölçümlerini bulun:
> [!div class="nextstepaction"] 
> [Kullanım ölçümlerini görüntüleme](how-to-view-api-usage.md)

Azure AD'yi Azure Haritalar'la nasıl entegre edilenleri gösteren örnekleri keşfedin:

> [!div class="nextstepaction"]
> [Azure AD kimlik doğrulama örnekleri](https://github.com/Azure-Samples/Azure-Maps-AzureAD-Samples)
