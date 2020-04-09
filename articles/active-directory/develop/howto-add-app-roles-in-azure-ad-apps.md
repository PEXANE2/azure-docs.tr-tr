---
title: Uygulama rolleri ekleme ve bunları bir belirteçten alma | Azure
titleSuffix: Microsoft identity platform
description: Azure Active Directory'de kayıtlı bir uygulamada uygulama rollerini nasıl ekleyeceğinizi, kullanıcıları `roles` ve grupları bu rollere nasıl atayacağız ve bunları belirteçteki talepte nasıl alacağınızı öğrenin.
services: active-directory
author: kkrishna
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.topic: conceptual
ms.date: 09/24/2018
ms.author: kkrishna
ms.reviewer: kkrishna, jmprieur
ms.custom: aaddev
ms.openlocfilehash: 144fad249011d547ac6a8cf2d404cb3f8fe74f96
ms.sourcegitcommit: d187fe0143d7dbaf8d775150453bd3c188087411
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80884265"
---
# <a name="how-to-add-app-roles-in-your-application-and-receive-them-in-the-token"></a>Nasıl yapilir: Uygulamanızda uygulama rolleri ekleyin ve bunları belirteçle alın

Rol tabanlı erişim denetimi (RBAC), uygulamalarda yetkilendirmeyi zorlamak için popüler bir mekanizmadır. RBAC kullanırken, yönetici rollere izin verir, tek tek kullanıcılara veya gruplara vermez. Yönetici daha sonra, hangi içeriğe ve işlevsellikte kimlerin erişebileceğini denetlemek için farklı kullanıcılara ve gruplara roller atayabilir.

RBAC'ı Uygulama Rolleri ve Rol İddiaları ile kullanarak, geliştiriciler kendi uygulamalarında çok az çaba sarf edemeden uygulamalarında yetkilendirmeyi güvenli bir şekilde uygulayabilirler.

Diğer bir yaklaşım da [WebApp-GroupClaims-DotNet'te](https://github.com/Azure-Samples/WebApp-GroupClaims-DotNet)gösterildiği gibi Azure REKLAM Grupları ve Grup İddiaları kullanmaktır. Azure REKLAM Grupları ve Uygulama Rolleri hiçbir şekilde birbirini dışlamıyor; daha da ince taneli erişim kontrolü sağlamak için birlikte kullanılabilirler.

## <a name="declare-roles-for-an-application"></a>Uygulama için rolleri bildirme

Bu uygulama rolleri, uygulamanın kayıt bildirimindeki [Azure portalında](https://portal.azure.com) tanımlanır.  Bir kullanıcı uygulamaya girdiğinde, Azure AD kullanıcıya ve grup üyeliğinden ayrı ayrı verilen her rol için bir `roles` hak talebinde bulunarak gelir.  Kullanıcıların ve grupların rollere atanması portalın Kullanıcı Arabirimi üzerinden veya [Programlı](https://developer.microsoft.com/graph/docs/concepts/azuread-identity-access-management-concept-overview)olarak Microsoft Graph kullanılarak yapılabilir.

### <a name="declare-app-roles-using-azure-portal"></a>Azure portalLarını kullanarak uygulama rollerini bildirme

1. [Azure Portal](https://portal.azure.com) oturum açın.
1. Portal araç çubuğundaki **Dizin + Abonelik** simgesini seçin.
1. Sık **Kullanılanlar** veya **Tüm Dizinler** listesinde, başvurunuzu kaydetmek istediğiniz Etkin Dizin kiracısını seçin.
1. Azure portalında Azure Etkin **Dizini'ni**arayın ve seçin.
1. Azure **Etkin Dizin** bölmesinde, tüm uygulamalarınızın listesini görüntülemek için **Uygulama kayıtlarını** seçin.
1. Uygulama rollerini tanımlamak istediğiniz uygulamayı seçin. Ardından **Manifest'i**seçin.
1. Ayarı bularak ve tüm `appRoles` Uygulama Rollerinizi ekleyerek uygulama bildirimini düzenleme.

     > [!NOTE]
     > Bu bildirimdeki her uygulama rol tanımı, `id` özellik için bildirim bağlamında farklı bir geçerli GUID olmalıdır.
     >
     > Her `value` uygulama rol tanımının özelliği, uygulamadaki kodda kullanılan dizeleri tam olarak eşleştirmelidir. Özellik `value` boşluk içeremez. Varsa, bildirimi kaydettiğinizde bir hata alırsınız.

1. Manifestoyu kaydedin.

### <a name="examples"></a>Örnekler

Aşağıdaki örnek, `appRoles` `users`atama yapabileceğinizi gösterir.

> [!NOTE]
>Benzersiz `id` bir GUID olmalıdır.

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
>Boşluk `displayName` lar içeremez.

Uygulama rollerini hedef `users` `applications`almak için , veya her ikisini de tanımlayabilirsiniz. Kullanılabilir `applications`olduğunda, uygulama rolleri **Gerekli İzinler** bıçakta uygulama izinleri olarak görünür. Aşağıdaki örnekte, bir `Application`.

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

Tanımlanan rol sayısı, uygulama bildiriminin sahip olduğu sınırları etkiler. Bunlar manifesto limitleri sayfasında ayrıntılı olarak ele [alınmıştır.](https://docs.microsoft.com/azure/active-directory/develop/reference-app-manifest#manifest-limits)

### <a name="assign-users-and-groups-to-roles"></a>Kullanıcıları ve grupları rollere atama

Uygulamanıza uygulama rolleri ekledikten sonra, kullanıcıları ve grupları bu rollere atayabilirsiniz.

1. Azure **Etkin Dizin** bölmesinde, Azure Active **Directory** sol navigasyon menüsünden **Kurumsal uygulamaları** seçin.
1. **Tüm uygulamalarınızın** listesini görüntülemek için Tüm uygulamaları seçin.

     Burada gösterilmesini istediğiniz uygulamayı göremiyorsanız, listeyi kısıtlamak veya uygulamanızı bulmak için listeyi aşağı kaydırmak için **Tüm uygulamalar** listesinin en üstündeki çeşitli filtreleri kullanın.

1. Rolleri ne zaman kullanıcı veya güvenlik grubu atamak istediğiniz uygulamayı seçin.
1. Uygulamanın sol navigasyon menüsünde **Kullanıcılar ve gruplar** bölmesini seçin.
1. **Kullanıcılar ve gruplar** listesinin en üstünde, Atama **Ekle** bölmesini açmak için **Kullanıcı Ekle** düğmesini seçin.
1. **Atama Ekle** bölmesinden Kullanıcılar **ve gruplar** seçiciyi seçin.

     Belirli bir kullanıcıyı veya grubu aramak ve bulmak için bir textbox ile birlikte kullanıcıların ve güvenlik gruplarının listesi gösterilir. Bu ekran, tek bir denemede birden çok kullanıcı ve grup seçmenize olanak tanır.

1. Kullanıcıları ve grupları seçtikten sonra, bir sonraki bölüme geçmek için alttaki **Seç** düğmesine basın.
1. **Atama ekle** bölmesinden Rol seçisini **seçin.** Uygulama bildiriminde daha önce bildirilen tüm roller gösterilecek.
1. Bir rol seçin ve **Seç** düğmesine basın.
1. Kullanıcıların ve grupların uygulamaya yönelik ödevlerini tamamlamak için alttaki **Atla** düğmesine basın.
1. Eklediğiniz kullanıcıların ve grupların güncelleştirilmiş Kullanıcılar **ve gruplar** listesinde yer aldığına onaylayın.

## <a name="more-information"></a>Daha fazla bilgi

- [ASP.NET Core web uygulamasına uygulama rollerini & rolleri ASP.NET iddialarını kullanarak yetkilendirme ekleme](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/tree/master/5-WebApp-AuthZ/5-1-Roles)
- [Uygulamalarınızda Güvenlik Gruplarını ve Uygulama Rollerini Kullanma (Video)](https://www.youtube.com/watch?v=V8VUPixLSiM)
- [Azure Active Directory, şimdi Grup Talepleri ve Uygulama Rolleri ile](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-Identity/Azure-Active-Directory-now-with-Group-Claims-and-Application/ba-p/243862)
- [Azure Active Directory uygulama bildirimi](https://docs.microsoft.com/azure/active-directory/develop/reference-app-manifest)
- [AAD Erişim belirteçleri](access-tokens.md)
- [Acar`id_tokens`](id-tokens.md)
