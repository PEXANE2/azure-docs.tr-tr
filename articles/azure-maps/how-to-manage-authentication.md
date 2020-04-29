---
title: Kimlik doğrulamasını yönetme | Microsoft Azure haritaları
description: Microsoft Azure haritaların kimlik doğrulamasını yönetmek için Azure portal kullanın.
author: philmea
ms.author: philmea
ms.date: 01/29/2020
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.openlocfilehash: dfe73971f29ea362fdd0ddd654e705b622ab1866
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80335537"
---
# <a name="manage-authentication-in-azure-maps"></a>Azure haritalar 'da kimlik doğrulamasını yönetme

Azure haritalar hesabı oluşturduktan sonra, Azure Active Directory (Azure AD) kimlik doğrulamasını ve paylaşılan anahtar kimlik doğrulamasını desteklemek için bir istemci KIMLIĞI ve anahtarı oluşturulur.

## <a name="view-authentication-details"></a>Kimlik doğrulama ayrıntılarını görüntüle

Azure haritalar hesabı oluşturduktan sonra, birincil ve ikincil anahtarlar oluşturulur. [Azure haritalar 'ı çağırmak Için paylaşılan anahtar kimlik doğrulaması](https://docs.microsoft.com/azure/azure-maps/azure-maps-authentication#shared-key-authentication)kullandığınızda bir birincil anahtarı abonelik anahtarı olarak kullanmanızı öneririz. İkincil bir anahtarı, kayan anahtar değişiklikleri gibi senaryolarda kullanabilirsiniz. Daha fazla bilgi için bkz. [Azure haritalar 'Da kimlik doğrulaması](https://aka.ms/amauth).

Kimlik doğrulama ayrıntılarınızı Azure portal görüntüleyebilirsiniz. Burada, hesabınızda, **Ayarlar** menüsünde **kimlik doğrulaması**' nı seçin.

![Kimlik doğrulaması ayrıntıları](./media/how-to-manage-authentication/how-to-view-auth.png)


## <a name="register-and-configure-an-azure-ad-app"></a>Azure AD uygulamasını kaydetme ve yapılandırma

1. Azure Portal Azure hizmetleri listesinde,**Yeni kayıt****uygulama kayıtları** >  **Azure Active Directory** > ' ni seçin.  

    ![Uygulama kaydı](./media/how-to-manage-authentication/app-registration.png)

1. Uygulamanızı zaten kaydolduysanız bir sonraki adımla devam edin. Uygulamanızı kaydolmadıysanız, bir **ad**girin, bir **destek hesabı türü**seçin ve ardından **Kaydet**' i seçin.  

    ![Uygulama kaydı ayrıntıları](./media/how-to-manage-authentication/app-create.png)

1. Azure haritalar 'a temsil edilen API izinleri atamak için uygulamaya gidin. **Uygulama kayıtları**altında, **API izinleri** > **izin Ekle**' yi seçin. **Kuruluşumun kullandığı API 'ler**altında **Azure haritaları**' nı arayıp seçin.

    ![Uygulama API 'SI izinleri ekleme](./media/how-to-manage-authentication/app-permissions.png)

1. **Azure haritalar**' ın yanındaki onay kutusunu işaretleyin ve ardından **izin Ekle**' yi seçin.

    ![Uygulama API 'SI izinlerini seçin](./media/how-to-manage-authentication/select-app-permissions.png)

1. Kimlik doğrulama yönteminize bağlı olarak aşağıdaki adımlardan birini doldurun. 

    * Uygulamanız Azure Maps web SDK 'Sı ile kullanıcı belirteci kimlik doğrulaması kullanıyorsa, öğesini etkinleştirin `oauth2AllowImplicitFlow`. Etkinleştirmek için, uygulama kaydlarınızın **bildirim** bölümünde doğru olarak ayarlayın `oauth2AllowImplicitFlow` . 
    
       ![Uygulama bildirimi](./media/how-to-manage-authentication/app-manifest.png)

    * Uygulamanız sunucu veya uygulama kimlik doğrulaması kullanıyorsa, uygulama kayıt sayfanızda, **sertifikalar & parolalar**' a gidin. Sonra da bir ortak anahtar sertifikası yükleyin veya **yeni istemci parolası**' nı seçerek bir parola oluşturun. 
    
       ![İstemci parolası oluşturma](./media/how-to-manage-authentication/app-keys.png)

        Bir parola oluşturursanız, **Ekle**' yi seçtikten sonra parolayı kopyalayın ve güvenli bir şekilde saklayın. Bu parolayı Azure AD 'den belirteç almak için kullanacaksınız.

       ![İstemci parolası ekleme](./media/how-to-manage-authentication/add-key.png)


## <a name="grant-role-based-access-control-to-azure-maps"></a>Azure Maps 'e rol tabanlı erişim denetimi verme

Azure haritalar hesabını Azure AD kiracınızla ilişkilendirdikten sonra, erişim denetimi verebilirsiniz. Bir veya daha fazla Azure Maps erişim denetimi rolüne kullanıcı, Grup veya uygulama atayarak *rol tabanlı erişim denetimi* (RBAC) verirsiniz. 

1. **Azure haritalar hesabınıza**gidin. **Erişim denetimi (IAM)** > **rol atamasını**seçin.

    ![RBAC verme](./media/how-to-manage-authentication/how-to-grant-rbac.png)

1. **Rol atamaları** sekmesinde **rol**altında **Azure Maps Tarih okuyucusu (Önizleme)** seçeneğini belirleyin. **Erişim ata**' nın altında **Azure AD Kullanıcı, Grup veya hizmet sorumlusu**' nı seçin. Kullanıcı veya uygulamayı seçin. Sonra **Kaydet**' i seçin.

    ![Rol ataması ekle](./media/how-to-manage-authentication/add-role-assignment.png)

## <a name="view-available-azure-maps-rbac-roles"></a>Kullanılabilir Azure Maps RBAC rollerini görüntüleme

Azure haritalar için kullanılabilen RBAC rollerini görüntülemek için **erişim denetimi (IAM)** sayfasına gidin. **Roller**' i seçin ve ardından *Azure Maps*ile başlayan roller için arama yapın. Bu Azure Maps rolleri, erişim sağlayabilmeniz için kullanabileceğiniz rollerdir.

![Kullanılabilir rolleri görüntüle](./media/how-to-manage-authentication/how-to-view-avail-roles.png)


## <a name="view-azure-maps-rbac"></a>Azure haritalar RBAC 'yi görüntüleme

RBAC, ayrıntılı erişim denetimi sağlar.

Azure haritalar için RBAC verilen kullanıcıları ve uygulamaları görüntülemek için **Access Control (IAM)** sayfasına gidin. Burada **rol atamaları**' nı seçin ve ardından **Azure Maps**'a göre filtreleyin.

![RBAC verilen kullanıcıları ve uygulamaları görüntüleme](./media/how-to-manage-authentication/how-to-view-amrbac.png)


## <a name="request-tokens-for-azure-maps"></a>Azure haritalar için istek belirteçleri

Uygulamanızı kaydettikten ve Azure Maps ile ilişkilendirdikten sonra, erişim belirteçleri isteyebilirsiniz.

Uygulamanız Azure Maps web SDK 'Sı ile kullanıcı belirteci kimlik doğrulamasını kullanıyorsa, HTML sayfanızı Azure Maps istemci KIMLIĞI ve Azure AD uygulama KIMLIĞIYLE yapılandırın.

Uygulamanız sunucu veya uygulama kimlik doğrulaması kullanıyorsa Azure AD belirteç uç noktasından `https://login.microsoftonline.com`bir belirteç isteyin. İsteğiniz içinde aşağıdaki ayrıntıları kullanın: 

* Azure AD kaynak KIMLIĞI`https://atlas.microsoft.com/`
* Azure haritalar istemci KIMLIĞI
* Azure AD uygulama KIMLIĞI
* Azure AD uygulama kaydı parolası veya sertifikası

| Azure ortamı   | Azure AD belirteç uç noktası | Azure Kaynak KIMLIĞI |
| --------------------|-------------------------|-------------------|
| Azure genel bulutu        | `https://login.microsoftonline.com` | `https://atlas.microsoft.com/` |
| Azure Kamu Bulutu   | `https://login.microsoftonline.us`  | `https://atlas.microsoft.com/` | 

Kullanıcılar ve hizmet sorumluları için Azure AD 'den erişim belirteçleri isteme hakkında daha fazla bilgi için bkz. [Azure AD Için kimlik doğrulama senaryoları](https://docs.microsoft.com/azure/active-directory/develop/authentication-scenarios).


## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi için bkz. [Azure AD ve Azure Maps web SDK](https://docs.microsoft.com/azure/azure-maps/how-to-use-map-control).

Azure haritalar hesabınız için API kullanım ölçümlerini bulun:
> [!div class="nextstepaction"] 
> [Kullanım ölçümlerini görüntüleme](how-to-view-api-usage.md)

Azure AD 'nin Azure Maps ile nasıl tümleştirileceğini gösteren örnekleri araştırma:

> [!div class="nextstepaction"]
> [Azure AD kimlik doğrulaması örnekleri](https://github.com/Azure-Samples/Azure-Maps-AzureAD-Samples)
