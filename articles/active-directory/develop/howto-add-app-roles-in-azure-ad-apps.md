---
title: Uygulama rolleri ekleme ve bir belirteçten edinme | Mavisi
titleSuffix: Microsoft identity platform
description: Azure Active Directory kayıtlı bir uygulamaya uygulama rolleri ekleme, bu rollere Kullanıcı ve Grup atama ve bunları `roles` belirteçteki talepte alma hakkında bilgi edinin.
services: active-directory
author: kalyankrishna1
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.topic: how-to
ms.date: 07/15/2020
ms.author: kkrishna
ms.reviewer: kkrishna, jmprieur
ms.custom: aaddev
ms.openlocfilehash: 5a2acb08971bc0878c943047c42c9dc2a9525794
ms.sourcegitcommit: a2a7746c858eec0f7e93b50a1758a6278504977e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/12/2020
ms.locfileid: "88141440"
---
# <a name="how-to-add-app-roles-in-your-application-and-receive-them-in-the-token"></a>Nasıl yapılır: uygulamanıza uygulama rolleri ekleme ve bunları belirtece alma

Rol tabanlı erişim denetimi (RBAC), uygulamalarda yetkilendirmeyi zorlamak için popüler bir mekanizmadır. RBAC kullanılırken, yönetici, bireysel kullanıcılara veya gruplara değil, rollere izinler verir. Yönetici daha sonra, hangi içeriğe ve işlevlere kimlerin erişebileceğini denetlemek için farklı kullanıcılara ve gruplara roller atayabilir.

Uygulama rolleri ve rol talepleri ile RBAC kullanarak, geliştiriciler uygulamalarında daha az çaba harcayacak şekilde yetkilendirmeyi güvenli bir şekilde uygulayabilir.

Diğer bir yaklaşım ise, [WebApp-Groupclaim-DotNet](https://github.com/Azure-Samples/WebApp-GroupClaims-DotNet)bölümünde gösterildiği gıbı Azure AD gruplarını ve grup taleplerini kullanmaktır. Azure AD grupları ve uygulama rollerinin hiçbir şekilde birbirini dışlamalı olması gerekmez; daha da ayrıntılı erişim denetimi sağlamak için birlikte kullanılabilir.

## <a name="declare-roles-for-an-application"></a>Bir uygulama için roller bildirme

Bu uygulama rolleri, uygulamanın kayıt bildiriminde [Azure Portal](https://portal.azure.com) tanımlanır.  Kullanıcı uygulamada oturum açtığında Azure AD, `roles` her rol için kullanıcıya tek tek ve grup üyeliğinden ayrı olarak verilen bir talep yayar.  Kullanıcılara ve grupların rollere atanması, portalın kullanıcı arabiriminden ya da program aracılığıyla [Microsoft Graph](/graph/azuread-identity-access-management-concept-overview)kullanılarak yapılabilir.

### <a name="declare-app-roles-using-azure-portal"></a>Azure portal kullanarak uygulama rolleri bildirme

1. [Azure portalında](https://portal.azure.com) oturum açın.
1. Portal araç çubuğunda **Dizin + abonelik** simgesini seçin.
1. **Sık Kullanılanlar** veya **tüm dizinler** listesinde, uygulamanızı kaydetmek istediğiniz Active Directory kiracıyı seçin.
1. Azure portal, araması yapın ve **Azure Active Directory**seçin.
1. **Azure Active Directory** bölmesinde, tüm uygulamalarınızın listesini görüntülemek için **uygulama kayıtları** ' ı seçin.
1. İçinde uygulama rollerini tanımlamak istediğiniz uygulamayı seçin. Ardından **bildirim**' ı seçin.
1. `appRoles`Ayarı bularak ve tüm uygulama rollerinizi ekleyerek uygulama bildirimini düzenleyin.

     > [!NOTE]
     > Bu Bildirimdeki her uygulama rolü tanımı, özelliği için bildirim bağlamı içinde farklı bir GUID içermelidir `id` .
     >
     > `value`Her uygulama rolü tanımının özelliği, uygulamadaki kodda kullanılan dizelerle tam olarak eşleşmelidir. `value`Özellik boşluk içeremez. Bunu yaparsanız, bildirimi kaydettiğinizde bir hata alırsınız.

1. Bildirimi kaydedin.

### <a name="examples"></a>Örnekler

Aşağıdaki örnek, atayabileceğiniz öğesini gösterir `appRoles` `users` .

> [!NOTE]
>`id`Benzersiz BIR GUID olmalıdır.

```Json
"appId": "8763f1c4-f988-489c-a51e-158e9ef97d6a",
"appRoles": [
    {
      "allowedMemberTypes": [
        "User"
      ],
      "displayName": "Writer",
      "id": "d1c2ade8-98f8-45fd-aa4a-6d06b947c66f",
      "isEnabled": true,
      "description": "Writers Have the ability to create tasks.",
      "value": "Writer"
    }
  ],
"availableToOtherTenants": false,
```

> [!NOTE]
>`displayName`Boşluk içerebilir.

Hedef `users` , `applications` veya her ikisini de hedeflemek için uygulama rolleri tanımlayabilirsiniz. İçin kullanılabilir olduğunda `applications` , uygulama rolleri **Yönet** bölümünde uygulama izinleri olarak görünür > API **Izinleri > API 'Lerim > bir ızın ekleyin > bir API > uygulama izinleri seçin**. Aşağıdaki örnekte, öğesine hedeflenmiş bir uygulama rolü gösterilmektedir `Application` .

```Json
"appId": "8763f1c4-f988-489c-a51e-158e9ef97d6a",
"appRoles": [
    {
      "allowedMemberTypes": [
        "Application"
      ],
      "displayName": "ConsumerApps",
      "id": "47fbb575-859a-4941-89c9-0f7a6c30beac",
      "isEnabled": true,
      "description": "Consumer apps have access to the consumer data.",
      "value": "Consumer"
    }
  ],
"availableToOtherTenants": false,
```

Tanımlanan rol sayısı, uygulama bildiriminin sahip olduğu limitleri etkiler. [Bildirim limitleri](./reference-app-manifest.md#manifest-limits) sayfasında ayrıntılı olarak ele alınmıştır.

### <a name="assign-users-and-groups-to-roles"></a>Rollere Kullanıcı ve Grup atama

Uygulamanıza uygulama rolleri ekledikten sonra, bu rollere kullanıcılar ve gruplar atayabilirsiniz.

1. **Azure Active Directory** bölmesinde, **Azure Active Directory** sol taraftaki gezinti menüsünden **Kurumsal uygulamalar** ' ı seçin.
1. Tüm uygulamalarınızın listesini görüntülemek için **tüm uygulamalar** ' ı seçin.

     Burada görünmesini istediğiniz uygulamayı görmüyorsanız, listeyi kısıtlamak için **tüm uygulamalar** listesinin en üstündeki çeşitli filtreleri kullanın veya uygulamanızı bulmak için listeyi aşağı kaydırın.

1. Rollere kullanıcı veya güvenlik grubu atamak istediğiniz uygulamayı seçin.
1. Uygulamanın sol taraftaki gezinti menüsünde **Kullanıcılar ve gruplar** bölmesini seçin.
1. **Kullanıcılar ve gruplar** listesinin en üstünde, **atama Ekle** bölmesini açmak için **Kullanıcı Ekle** düğmesini seçin.
1. **Atama Ekle** bölmesinden **Kullanıcılar ve gruplar** seçicisini seçin.

     Kullanıcı ve güvenlik gruplarının listesi, belirli bir kullanıcı veya grubu aramak ve bulmak için bir metin kutusuyla birlikte gösterilir. Bu ekran, tek bir go içinde birden çok kullanıcı ve grup seçmenizi sağlar.

1. Kullanıcıları ve grupları seçmeyi tamamladıktan sonra, sonraki bölüme geçmek için alt kısımdaki **Seç** düğmesine basın.
1. **Atama Ekle** bölmesinden **rol seçicisini Seç ' i** seçin. Uygulama bildiriminde daha önce belirtilen tüm roller görünür.
1. Bir rol seçin ve **Seç** düğmesine basın.
1. Uygulamanın kullanıcı ve grupların atamalarını sona erdirmeyi sağlamak için alt kısımdaki **ata** düğmesine basın.
1. Eklediğiniz kullanıcı ve grupların, güncelleştirilmiş **Kullanıcılar ve gruplar** listesinde görüntülendiğini doğrulayın.

### <a name="receive-roles-in-tokens"></a>Belirteçlerde rolleri alma

Çeşitli uygulama rollerine atanan kullanıcılar uygulamada oturum açtığında, belirteçlerinin talep içinde kendilerine atanmış rolleri olur `roles` .

## <a name="more-information"></a>Daha fazla bilgi

- [Uygulama rollerini kullanarak yetkilendirme ekleme ASP.NET Core Web uygulaması için rol talepleri &](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/tree/master/5-WebApp-AuthZ/5-1-Roles)
- [Microsoft Identity platform (video) ile uygulamalarınızda yetkilendirme uygulama](https://www.youtube.com/watch?v=LRoc-na27l0)
- [Azure Active Directory, artık grup talepleri ve uygulama rolleriyle birlikte](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-Identity/Azure-Active-Directory-now-with-Group-Claims-and-Application/ba-p/243862)
- [Azure Active Directory uygulama bildirimi](./reference-app-manifest.md)
- [AAD erişim belirteçleri](access-tokens.md)
- [AAD`id_tokens`](id-tokens.md)