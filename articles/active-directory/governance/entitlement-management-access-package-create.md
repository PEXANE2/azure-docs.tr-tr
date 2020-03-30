---
title: Yetkilendirme yönetiminde yeni bir erişim paketi oluşturma - Azure AD
description: Azure Active Directory yetkilendirme yönetiminde paylaşmak istediğiniz yeni bir erişim kaynakları paketini nasıl oluşturabileceğinizi öğrenin.
services: active-directory
documentationCenter: ''
author: msaburnley
manager: daveba
editor: ''
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.subservice: compliance
ms.date: 10/15/2019
ms.author: ajburnle
ms.reviewer: ''
ms.collection: M365-identity-device-management
ms.openlocfilehash: 7c858a17d4574e6e45283df7c1276cd303f25297
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79262015"
---
# <a name="create-a-new-access-package-in-azure-ad-entitlement-management"></a>Azure AD yetkilendirme yönetiminde yeni bir erişim paketi oluşturma

Erişim paketi, erişim paketinin ömrü boyunca erişimi otomatik olarak yöneten kaynakların ve ilkelerin bir kerelik kurulum yapmasını sağlar. Bu makalede, yeni bir erişim paketi oluşturmak için nasıl açıklanmaktadır.

## <a name="overview"></a>Genel Bakış

Tüm erişim paketleri katalog adı verilen bir kapsayıcıya konulmalıdır. Katalog, erişim paketinize hangi kaynakları ekleyebileceğinizi tanımlar. Bir katalog belirtmezseniz, erişim paketiniz Genel kataloga konur. Şu anda, varolan bir erişim paketini farklı bir kataloga taşıyamazsınız.

Bir erişim paketi yöneticisiyseniz, sahip olduğunuz kaynakları bir kataloğere ekleyemezsiniz. Katalogda bulunan kaynakları kullanmakla sınırlıdır. Bir kataloğuna kaynak eklemeniz gerekiyorsa, katalog sahibine sorabilirsiniz.

Tüm erişim paketlerinin en az bir ilkesi olmalıdır. İlkeler, erişim paketini ve ayrıca onay ve yaşam döngüsü ayarlarını kimlerin isteyebileceğini belirtir. Yeni bir erişim paketi oluşturduğunuzda, dizininizdeki kullanıcılar, dizininizde olmayan kullanıcılar için, yalnızca yönetici doğrudan atamaları için bir başlangıç ilkesi oluşturabilirsiniz veya daha sonra ilkeyi oluşturmayı seçebilirsiniz.

![Erişim paketi oluşturma](./media/entitlement-management-access-package-create/access-package-create.png)

Yeni bir erişim paketi oluşturmak için üst düzey adımlar aşağıda veda edebilirsiniz.

1. Kimlik Yönetimi'nde, yeni bir erişim paketi oluşturmak için işlemi başlatın.

1. Erişim paketini oluşturmak istediğiniz kataloğu seçin.

1. Katalogdan erişim paketinize kaynak ekleyin.

1. Her kaynak için kaynak rolleri atayın.

1. Erişim isteyebilecek kullanıcıları belirtin.

1. Onay ayarlarını belirtin.

1. Yaşam döngüsü ayarlarını belirtin.

## <a name="start-new-access-package"></a>Yeni erişim paketini başlatın

**Önkoşul rolü:** Genel yönetici, Kullanıcı yöneticisi, Katalog sahibi veya Access paket yöneticisi

1. [Azure portalında](https://portal.azure.com)oturum açın.

1. **Azure Etkin Dizini'ni** tıklatın ve ardından **Kimlik Yönetimi'ni**tıklatın.

1. Sol menüde, **Access paketlerine**tıklayın.

1. **Yeni erişim paketini**tıklatın.
   
    ![Azure portalında yetkilendirme yönetimi](./media/entitlement-management-shared/access-packages-list.png)

## <a name="basics"></a>Temel Bilgiler

Temel **Bilgiler** sekmesinde, erişim paketine bir ad verir ve erişim paketini hangi katalogda oluşturabileceğinizi belirtirsiniz.

1. Erişim paketi için bir görüntü adı ve açıklama girin. Kullanıcılar erişim paketi için bir istek gönderdiklerinde bu bilgileri görürler.

1. **Katalog** açılır listesinde, erişim paketini oluşturmak istediğiniz kataloğu seçin. Örneğin, istenebilecek tüm pazarlama kaynaklarını yöneten bir katalog sahibiniz olabilir. Bu durumda, pazarlama kataloğunu seçebilirsiniz.

    Yalnızca giriş paketleri oluşturma iznine sahip olduğunuz katalogları görürsünüz. Varolan bir katalogda bir erişim paketi oluşturmak için, Global yönetici veya Kullanıcı yöneticisi olmalısınız veya bu katalogda katalog sahibi veya erişim paketi yöneticisi olmalısınız.

    ![Erişim paketi - Temel Bilgiler](./media/entitlement-management-access-package-create/basics.png)

    Global yönetici, Kullanıcı yöneticisi veya katalog oluşturucuiseniz ve erişim paketinizi listelenmemiş yeni bir katalogda oluşturmak istiyorsanız, **yeni katalog oluştur'u**tıklatın. Katalog adını ve açıklamasını girin ve sonra **Oluştur'u**tıklatın.

    Oluşturduğunuz erişim paketi ve içinde yer alan tüm kaynaklar yeni kataloga eklenir. Daha sonra ek katalog sahipleri de ekleyebilirsiniz.

1. **İleri**'ye tıklayın.

## <a name="resource-roles"></a>Kaynak rolleri

Kaynak **rolleri** sekmesinde, erişim paketine dahil edilecek kaynakları seçersiniz. Erişim paketini isteyen ve alan kullanıcılar, erişim paketindeki tüm kaynak rollerini alır.

1. Eklemek istediğiniz kaynak türünü tıklatın **(Gruplar ve Takımlar,** **Uygulamalar**veya **SharePoint siteleri).**

1. Görünen Seç bölmesinde, listeden bir veya daha fazla kaynak seçin.

    ![Erişim paketi - Kaynak rolleri](./media/entitlement-management-access-package-create/resource-roles.png)

    Erişim paketini Genel katalogda veya yeni bir katalogda oluşturuyorsanız, sahip olduğunuz dizinden herhangi bir kaynak seçebilirsiniz. En az bir Global yöneticisi, kullanıcı yöneticisi veya Katalog oluşturucusu olmalısınız.

    Erişim paketini varolan bir katalogda oluşturuyorsanız, kataloğun sahibi olmadan katalogda bulunan tüm kaynakları seçebilirsiniz.

    Global yönetici, Kullanıcı yöneticisi veya katalog sahibiyseniz, kataloğunuzda henüz olmayan kaynakları seçme seçeneğiniz vardır. Şu anda seçili katalogda olmayan kaynakları seçerseniz, bu kaynaklar diğer katalog yöneticilerinin erişim paketleri oluşturması için de kataloga eklenir. Yalnızca seçili katalogda bulunan kaynakları seçmek istiyorsanız, Seç bölmesinin üst kısmındaki **Yalnızca Bkz.** onay kutusunu işaretleyin.

1. **Rol** listesindeki kaynakları seçtikten sonra, kullanıcıların kaynak için atanmasını istediğiniz rolü seçin.

    ![Erişim paketi - Kaynak rol seçimi](./media/entitlement-management-access-package-create/resource-roles-role.png)

1. **İleri**'ye tıklayın.

## <a name="requests"></a>İstekler

**İstekler** sekmesinde, erişim paketini ve onay ayarlarını kimin isteyebileceğini belirtmek için ilk ilkeyi oluşturursunuz. Daha sonra, ek kullanıcı gruplarının erişim paketini kendi onay ayarlarıyla istemesine izin vermek için daha fazla istek ilkeleri oluşturabilirsiniz.

![Erişim paketi - İstekler sekmesi](./media/entitlement-management-access-package-create/requests.png)

Bu erişim paketini kime istediğinize bağlı olarak, aşağıdaki bölümlerden birinde adımları gerçekleştirin.

[!INCLUDE [Entitlement management request policy](../../../includes/active-directory-entitlement-management-request-policy.md)]

[!INCLUDE [Entitlement management lifecycle policy](../../../includes/active-directory-entitlement-management-lifecycle-policy.md)]

## <a name="review--create"></a>İnceleme + oluşturma

Gözden **Geçir + oluştur** sekmesinde, ayarlarınızı gözden geçirebilir ve doğrulama hatalarını kontrol edebilirsiniz.

1. Erişim paketinin ayarlarını gözden geçirme

    ![Erişim paketi - İlke- İlke ayarını etkinleştir](./media/entitlement-management-access-package-create/review-create.png)

1. Erişim paketini oluşturmak için **Oluştur'u** tıklatın.

    Yeni erişim paketi erişim paketleri listesinde görünür.

## <a name="creating-an-access-package-programmatically"></a>Programlı bir erişim paketi oluşturma

Microsoft Graph'ı kullanarak bir erişim paketi de oluşturabilirsiniz.  Temsilci izniolan `EntitlementManagement.ReadWrite.All` bir uygulamayla uygun bir roldeki bir kullanıcı, API'yi

1. [Katalogdaki accessPackageResources'ı listele](https://docs.microsoft.com/graph/api/accesspackagecatalog-list-accesspackageresources?view=graph-rest-beta) ve henüz katalogda olmayan kaynaklar için [bir accessPackageResourceRequest oluşturun.](https://docs.microsoft.com/graph/api/accesspackageresourcerequest-post?view=graph-rest-beta)
1. [AccessPackageResource'ın accessPackageResourceRol'larını](https://docs.microsoft.com/graph/api/accesspackagecatalog-list-accesspackageresourceroles?view=graph-rest-beta) bir accessPackageCatalog'da listele. Bu rol listesi daha sonra bir accessPackageResourceRoleScope oluştururken bir rol seçmek için kullanılır.
1. [Bir accessPackage oluşturun.](https://docs.microsoft.com/graph/api/accesspackage-post?view=graph-rest-beta)
1. [AccessPackageAssignmentPolicy oluşturun.](https://docs.microsoft.com/graph/api/accesspackageassignmentpolicy-post?view=graph-rest-beta)
1. Erişim paketinde gereken her kaynak rolü için [bir accessPackageResourceRoleScope oluşturun.](https://docs.microsoft.com/graph/api/accesspackage-post-accesspackageresourcerolescopes?view=graph-rest-beta)

## <a name="next-steps"></a>Sonraki adımlar

- [Erişim paketi istemek için bağlantıyı paylaşma](entitlement-management-access-package-settings.md)
- [Erişim paketi için kaynak rollerini değiştirme](entitlement-management-access-package-resources.md)
- [Kullanıcıyı doğrudan erişim paketine atama](entitlement-management-access-package-assignments.md)
