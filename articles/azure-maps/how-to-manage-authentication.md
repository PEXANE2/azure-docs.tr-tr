---
title: Kimlik doğrulamasını yönetme | Microsoft Azure haritaları
description: Microsoft Azure haritaların kimlik doğrulamasını yönetmek için Azure portal kullanabilirsiniz.
author: walsehgal
ms.author: v-musehg
ms.date: 01/16/2020
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.openlocfilehash: 1f7f128898089292a8ccd92686af5d68fe328f3c
ms.sourcegitcommit: 984c5b53851be35c7c3148dcd4dfd2a93cebe49f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/28/2020
ms.locfileid: "76766171"
---
# <a name="manage-authentication-in-azure-maps"></a>Azure haritalar 'da kimlik doğrulamasını yönetme

Azure haritalar hesabı oluşturduktan sonra, Azure Active Directory (Azure AD) ve paylaşılan anahtar kimlik doğrulamasını desteklemek için bir istemci KIMLIĞI ve anahtarı oluşturulur.

## <a name="view-authentication-details"></a>Kimlik doğrulama ayrıntılarını görüntüle

Azure haritalar hesabı oluşturulduktan sonra birincil ve ikincil anahtarlar oluşturulur. Abonelik anahtarı olarak birincil anahtarı kullanın, bazen bu adlar birbirinin yerine kullanılır. İkincil anahtar, kayan anahtar değişiklikleri gibi senaryolarda kullanılabilir. Her iki durumda da, Azure haritalar 'ı çağırmak için bir anahtar gereklidir. Bu işleme [paylaşılan anahtar kimlik doğrulaması](https://docs.microsoft.com/azure/azure-maps/azure-maps-authentication#shared-key-authentication)adı verilir. Paylaşılan anahtar ve Azure AD kimlik doğrulaması hakkında daha fazla bilgi edinmek için bkz. [Azure Maps Ile kimlik doğrulama](https://aka.ms/amauth) .

Kimlik doğrulama ayrıntılarınızı Azure portal görüntüleyebilirsiniz. Hesabınıza gidin ve **Ayarlar** menüsünde **kimlik doğrulaması** ' nı seçin.

![Kimlik doğrulaması ayrıntıları](./media/how-to-manage-authentication/how-to-view-auth.png)


## <a name="set-up-azure-ad-app-registration"></a>Azure AD uygulama kaydını ayarlama

Azure haritalar hesabı oluşturduktan sonra Azure AD kiracınız ile Azure haritalar kaynağı arasında bir bağlantı kurmanız gerekir.

1. Portal menüsünden **Azure Active Directory** ' yi seçin. Kayıt için bir ad girin. **Uygulama kayıtları** ' ye tıklayın ve ardından **Yeni kayıt**' a tıklayın. **Yeniden yönlendirme URI 'si** kutusunda, Web uygulamasının giriş sayfasını belirtin. Örneğin, https://localhost/. Zaten kayıtlı bir uygulamanız varsa 2. adıma gidin.

    ![Uygulama kaydı](./media/how-to-manage-authentication/app-registration.png)

    ![Uygulama kaydı ayrıntıları](./media/how-to-manage-authentication/app-create.png)

2. Azure haritalar 'a temsil edilen API izinleri atamak için, **uygulama kayıtları**altındaki uygulamaya gidin ve ardından **API izinleri**' ni seçin. **Izin Ekle**' yi seçin. **BIR API seçin**altında **Azure haritaları** ' nı arayıp seçin.

    ![Uygulama API 'SI izinleri](./media/how-to-manage-authentication/app-permissions.png)

3. **Izinleri Seç**' in altında, **kullanıcı kimliğine bürünme**kutusunu Işaretleyin ve ardından alttaki **Seç** düğmesine tıklayın.

    ![Uygulama API 'SI izinlerini seçin](./media/how-to-manage-authentication/select-app-permissions.png)

4. Kimlik doğrulama yönteminize bağlı olarak, a veya b adımını tamamen doldurun.

    1. Uygulamanız Azure Maps web SDK 'Sı ile kullanıcı belirteci kimlik doğrulaması kullanıyorsa, uygulama kaydlarınızın bildirim bölümünde true olarak ayarlayarak `oauth2AllowImplicitFlow` etkinleştirin.
    
       ![Uygulama bildirimi](./media/how-to-manage-authentication/app-manifest.png)

    2. Uygulamanız sunucu/uygulama kimlik doğrulaması kullanıyorsa, uygulama kaydı ' nda **sertifikalar & gizlilikler** dikey penceresine gidin ve bir parola oluşturun ya da uygulama kaydına bir ortak anahtar sertifikası yükleyin. Bir parola oluşturursanız, daha sonra kullanmak üzere güvenli bir şekilde saklayın. Bu parolayı Azure AD 'den belirteç almak için kullanacaksınız.

       ![Uygulama anahtarları](./media/how-to-manage-authentication/app-keys.png)


## <a name="grant-role-based-access-control-rbac-to-azure-maps"></a>Azure Maps 'e rol tabanlı erişim denetimi (RBAC) verme

Azure haritalar hesabını Azure AD kiracınızla ilişkilendirdikten sonra, erişim denetimi verebilirsiniz. Bir veya daha fazla Azure Maps erişim denetimi rolüne kullanıcı, Grup veya uygulama atayarak erişim denetimi verirsiniz.

1. **Azure haritalar hesabınıza**gidin. **Erişim denetimi (IAM)** öğesini seçin ve ardından **rol ataması**' nı seçin.

    ![RBAC verme](./media/how-to-manage-authentication/how-to-grant-rbac.png)

2. **Rol atama** penceresinde, **rol**altında **Azure Maps Tarih okuyucusu (Önizleme)** öğesini seçin. **Azure AD Kullanıcı, Grup veya hizmet ilkesini**seçmek **için erişim ata** altında. Kullanıcı veya uygulamayı seçin. **Kaydet**’i seçin.

    ![Rol ataması Ekle](./media/how-to-manage-authentication/add-role-assignment.png)

## <a name="view-available-azure-maps-rbac-roles"></a>Kullanılabilir Azure Maps RBAC rollerini görüntüleme

Azure haritalar için kullanılabilen rol tabanlı erişim denetimi (RBAC) rollerini görüntülemek için **erişim denetimi (IAM)** sayfasına gidin, **Roller**' i seçin ve ardından **Azure Maps**ile başlayan roller için arama yapın. Bu roller, erişim izni vermek için kullanabileceğiniz rollerdir.

![Kullanılabilir rolleri görüntüle](./media/how-to-manage-authentication/how-to-view-avail-roles.png)


## <a name="view-azure-maps-rbac"></a>Azure haritalar RBAC 'yi görüntüleme

RBAC, ayrıntılı erişim denetimi sağlar.

Azure haritalar için RBAC verilmiş kullanıcıları ve uygulamaları görüntülemek için **Access Control (IAM)** sayfasına gidin, **rol atamaları**' nı seçin ve ardından **Azure Maps**'a göre filtreleyin.

![RBAC verilen kullanıcıları ve uygulamaları görüntüleme](./media/how-to-manage-authentication/how-to-view-amrbac.png)


## <a name="request-tokens-for-azure-maps"></a>Azure haritalar için istek belirteçleri

Uygulamanızı kaydettikten ve Azure Maps ile ilişkilendirdikten sonra, erişim belirteçleri isteyebilirsiniz.

* Uygulamanız Azure Maps web SDK 'Sı ile kullanıcı belirteci kimlik doğrulamasını kullanıyorsa, HTML sayfanızı Azure Maps istemci KIMLIĞI ve Azure AD uygulama KIMLIĞI ile yapılandırmanız gerekir.

* Uygulamanız sunucu/uygulama kimlik doğrulaması kullanıyorsa Azure ad kaynak KIMLIĞI `https://atlas.microsoft.com/`, Azure Maps istemci KIMLIĞI, Azure AD uygulama KIMLIĞI ve Azure AD uygulama kayıt parolası ya da sertifikası ile Azure AD belirteç uç noktası `https://login.microsoftonline.com` bir belirteç istemeniz gerekir.

| Azure ortamı   | Azure AD belirteç uç noktası | Azure Kaynak KIMLIĞI |
| --------------------|-------------------------|-------------------|
| Azure genel        | https://login.microsoftonline.com | https://atlas.microsoft.com/ |
| Azure Devlet Kurumları    | https://login.microsoftonline.us  | https://atlas.microsoft.com/ | 

Azure AD 'den erişim belirteçleri isteme hakkında daha fazla bilgi için, kullanıcılar ve hizmet sorumluları için bkz. [Azure AD Için kimlik doğrulama senaryoları](https://docs.microsoft.com/azure/active-directory/develop/authentication-scenarios).


## <a name="next-steps"></a>Sonraki adımlar

Azure AD kimlik doğrulaması ve Azure Maps web SDK hakkında daha fazla bilgi edinmek için bkz. [Azure AD ve Azure Maps web SDK](https://docs.microsoft.com/azure/azure-maps/how-to-use-map-control).

Azure haritalar hesabınız için API kullanım ölçümlerini nasıl görebileceğinizi öğrenin:
> [!div class="nextstepaction"] 
> [Kullanım ölçümlerini görüntüle](how-to-view-api-usage.md)

Azure Active Directory (AAD) Azure Maps ile nasıl tümleştirileceğini gösteren örneklerin listesi için bkz.:

> [!div class="nextstepaction"]
> [Azure AD kimlik doğrulaması örnekleri](https://github.com/Azure-Samples/Azure-Maps-AzureAD-Samples)
