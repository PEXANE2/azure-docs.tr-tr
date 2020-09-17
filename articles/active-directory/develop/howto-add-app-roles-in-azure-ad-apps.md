---
title: Uygulama rolleri ekleme ve bir belirteçten edinme | Mavisi
titleSuffix: Microsoft identity platform
description: Azure Active Directory kayıtlı bir uygulamaya uygulama rolleri ekleme, bu rollere Kullanıcı ve Grup atama ve bunları belirteçteki ' Roller ' talebinde alma hakkında bilgi edinin.
services: active-directory
author: kalyankrishna1
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.topic: how-to
ms.date: 09/15/2020
ms.author: kkrishna
ms.reviewer: kkrishna, jmprieur
ms.custom: aaddev
ms.openlocfilehash: 02fe3c1ebc893dea259abcda3ea1d13ab96d68d5
ms.sourcegitcommit: 7374b41bb1469f2e3ef119ffaf735f03f5fad484
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/16/2020
ms.locfileid: "90706022"
---
# <a name="how-to-add-app-roles-in-your-application-and-receive-them-in-the-token"></a>Nasıl yapılır: uygulamanıza uygulama rolleri ekleme ve bunları belirtece alma

Rol tabanlı erişim denetimi (RBAC), uygulamalarda yetkilendirmeyi zorlamak için popüler bir mekanizmadır. RBAC kullanılırken, yönetici, bireysel kullanıcılara veya gruplara değil, rollere izinler verir. Yönetici daha sonra, hangi içeriğe ve işlevlere kimlerin erişebileceğini denetlemek için farklı kullanıcılara ve gruplara roller atayabilir.

Uygulama rolleri ve rol talepleri ile RBAC kullanarak, geliştiriciler uygulamalarında daha az çabayla yetkilendirmeyi güvenli bir şekilde uygulayabilir.

Diğer bir yaklaşım, GitHub 'daki [WebApp-Groupclaim-DotNet](https://github.com/Azure-Samples/WebApp-GroupClaims-DotNet) kod örneğinde gösterildiği gıbı Azure AD gruplarını ve grup taleplerini kullanmaktır. Azure AD grupları ve uygulama rolleri birbirini dışlamalı değildir; daha da ayrıntılı erişim denetimi sağlamak için birlikte kullanılabilir.

## <a name="declare-roles-for-an-application"></a>Bir uygulama için roller bildirme

Uygulama rolleri [Azure Portal](https://portal.azure.com)tanımlanmıştır.  Kullanıcı uygulamada oturum açtığında Azure AD, `roles` her rol için kullanıcıya tek tek ve grup üyeliğinden ayrı olarak verilen bir talep yayar.  Kullanıcılara ve grupların rollere atanması, portalın kullanıcı arabiriminden ya da program aracılığıyla [Microsoft Graph](/graph/azuread-identity-access-management-concept-overview)kullanılarak yapılabilir.

Uygulama rolü oluşturmak için:

1. [Azure Portal](https://portal.azure.com) oturum açın.
1. Üst menüdeki **Dizin + abonelik** filtresi ' ni seçin ve ardından uygulama rolü eklemek istediğiniz uygulama kaydını içeren Azure Active Directory kiracıyı seçin.
1. **Azure Active Directory**'yi bulun ve seçin.
1. **Yönet**' in altında **uygulama kayıtları**' yi seçin ve ardından uygulama rollerini tanımlamak istediğiniz uygulamayı seçin.
1. **Uygulama rollerini seçin | Önizleme**yapın ve ardından **uygulama rolü oluştur**' u seçin.

   :::image type="content" source="media/howto-add-app-roles-in-azure-ad-apps/app-roles-overview-pane.png" alt-text="Azure portal bir uygulama kaydının uygulama rolleri bölmesi":::
1. **Uygulama rolü oluştur** bölmesinde, rolün ayarlarını girin. Görüntüyü izleyen tablo her bir ayarı ve parametrelerini açıklar.

    :::image type="content" source="media/howto-add-app-roles-in-azure-ad-apps/app-roles-create-context-pane.png" alt-text="Uygulama kaydının uygulama rolleri Azure portal bağlam bölmesi oluşturma":::

    | Alan | Açıklama | Örnek |
    |-------|-------------|---------|
    | **Görünen ad** | Yönetici onayı ve uygulama atama deneyimlerinde görünen uygulama rolü için görünen ad. Bu değer boşluk içerebilir.  | `Survey Writer` |
    | **İzin verilen üye türleri** | Bu uygulama rolünün kullanıcılara, uygulamalara veya her ikisine de atanıp atanamayacağını belirtir. | `Users/Groups` |
    | **Değer** | Uygulamanın belirteçte beklediği rol talebinin değerini belirtir. Değer, uygulamanın kodunda başvurulan dize ile tam olarak eşleşmelidir. Değer boşluk içeremez. | `Survey.Create` |
    | **Açıklama** | Yönetici uygulama atama ve onay deneyimleri sırasında görünen uygulama rolünün daha ayrıntılı bir açıklaması. | `Writers can create surveys.` |
    | **Bu uygulama rolünü etkinleştirmek istiyor musunuz?** | Uygulama rolünün etkin olup olmadığı. Bir uygulama rolünü silmek için bu onay kutusunun işaretini kaldırın ve silme işlemini denemeden önce değişikliği uygulayın. | *Edildikten* |

1. Yaptığınız değişiklikleri kaydetmek için **Apply**'ı (Uygula) seçin.

> [!NOTE]
> Eklediğiniz rol sayısı, bir uygulama bildirimi için tanımlanan sınırlara doğru sayılır. Bu sınırlamalar hakkında daha fazla bilgi için [Azure Active Directory uygulama bildirimi başvurusunun](reference-app-manifest.md) [bildirim sınırları](./reference-app-manifest.md#manifest-limits) bölümüne bakın.

## <a name="assign-users-and-groups-to-roles"></a>Rollere Kullanıcı ve Grup atama

Uygulamanıza uygulama rolleri ekledikten sonra, rollere kullanıcılar ve gruplar atayabilirsiniz.

1. Azure portal **Azure Active Directory** ' de, sol taraftaki gezinti menüsünde **Kurumsal uygulamalar** ' ı seçin.
1. Tüm uygulamalarınızın listesini görüntülemek için **tüm uygulamalar** ' ı seçin.

     Uygulamanız listede görünmezse, listeyi kısıtlamak için **tüm uygulamalar** listesinin en üstündeki filtreleri kullanın veya uygulamanızı bulmak için listeyi aşağı kaydırın.

1. Rollere kullanıcı veya güvenlik grubu atamak istediğiniz uygulamayı seçin.
1. **Yönet** bölümünde **Kullanıcılar ve gruplar**'ı seçin.
1. **Atama Ekle** bölmesini açmak Için **Kullanıcı Ekle** ' yi seçin.
1. **Atama Ekle** bölmesinden **Kullanıcılar ve gruplar** seçicisini seçin.

     Kullanıcıların ve güvenlik gruplarının listesi görüntülenir. Belirli bir kullanıcı veya grup için arama yapabilir ve listede görünen birden çok kullanıcı ve grubu seçebilirsiniz.

1. Kullanıcılar ve gruplar ' ı seçtikten sonra, devam etmek için **Seç** düğmesini seçin.
1. **Atama Ekle** bölmesinde **Rol Seç** ' i seçin. Uygulama için tanımladığınız tüm roller görüntülenir.
1. Bir rol seçin ve **Seç** düğmesini seçin.
1. Kullanıcılara ve grupların uygulamaya atanmasını tamamlayacak **ata** düğmesini seçin.
1. Eklediğiniz kullanıcı ve grupların **Kullanıcılar ve gruplar** listesinde göründüğünü doğrulayın.

## <a name="receive-roles-in-tokens"></a>Belirteçlerde rolleri alma

Çeşitli uygulama rollerine atanan kullanıcılar uygulamada oturum açtığında, belirteçlerinin talep içinde kendilerine atanmış rolleri olur `roles` .

## <a name="web-api-application-permissions"></a>Web API uygulama izinleri

**Kullanıcıları/grupları**, **uygulamaları**veya **her ikisini de**hedefleyen uygulama rollerini tanımlayabilirsiniz. **Uygulamalar** ' ı veya **her ikisini de**seçtiğinizde, uygulama rolü bir istemci uygulamasının **API izinlerinde**bir uygulama izni olarak görünür.

**Uygulamaları** veya **her ikisini de**hedefleyen bir rol tanımladıktan sonra bir istemci uygulaması kaydında uygulama iznini seçmek için:

1. Azure portal **Azure Active Directory** ' de, **uygulama kayıtları**' yı seçin ve ardından istemci uygulamasının kaydını seçin.
1. **Yönet**altında **API izinleri**' ni seçin.
1. **İzin Ekle**  >  **API 'lerim**' i seçin.
1. Uygulama rolünü eklediğiniz uygulamayı seçin ve ardından **Uygulama izinleri**' ni seçin.

## <a name="next-steps"></a>Sonraki adımlar

Aşağıdaki kaynaklarla uygulama rolleri hakkında daha fazla bilgi edinin:

- Kod örneği: [uygulama rollerini kullanarak yetkilendirme ekleme ASP.NET Core bir Web uygulamasına (GitHub) rol talepleri &](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/tree/master/5-WebApp-AuthZ/5-1-Roles)
- Video: [Microsoft Identity platform (1:01:15) ile uygulamalarınızda yetkilendirme uygulama](https://www.youtube.com/watch?v=LRoc-na27l0)
- [Azure Active Directory uygulama bildirimi](./reference-app-manifest.md)
- [Azure AD erişim belirteçleri](access-tokens.md)
- [Azure AD `id_tokens`](id-tokens.md)
