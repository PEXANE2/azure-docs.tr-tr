---
title: Azure API Yönetimi'nde kullanıcı hesaplarını yönetme | Microsoft Dokümanlar
description: Azure API Yönetimi'nde kullanıcıları nasıl oluşturacak veya davet edebilirsiniz öğrenin
services: api-management
documentationcenter: ''
author: vladvino
manager: cfowler
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 02/13/2018
ms.author: apimpm
ms.openlocfilehash: 9de75b5537b3e267116882e7f151d893de575071
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "70072460"
---
# <a name="how-to-manage-user-accounts-in-azure-api-management"></a>Azure API Management'ta kullanıcı hesaplarını yönetme

API Yönetimi'nde geliştiriciler, API Yönetimi'ni kullanarak ortaya çıkardığınız API kullanıcılarıdır. Bu kılavuz, geliştiricileri API Yönetimi örneğinizle birlikte kullanıma sunabileceğiniz API'leri ve ürünleri kullanmaya nasıl davet edeceğimize gösterilmektedir. Kullanıcı hesaplarını programlı bir şekilde yönetme hakkında bilgi [için, API Management REST](/rest/api/apimanagement/) başvurusunda [Kullanıcı varlık](https://docs.microsoft.com/rest/api/apimanagement/2019-01-01/user) belgelerine bakın.

[!INCLUDE [premium-dev-standard-basic.md](../../includes/api-management-availability-premium-dev-standard-basic.md)]

## <a name="prerequisites"></a>Ön koşullar

Bu makaledeki görevleri tamamlayın: [Azure API Yönetimi örneği oluşturun.](get-started-create-service-instance.md)

[!INCLUDE [api-management-navigate-to-instance.md](../../includes/api-management-navigate-to-instance.md)]

## <a name="create-a-new-developer"></a><a name="create-developer"> </a>Yeni bir geliştirici oluşturma

Yeni bir kullanıcı eklemek için bu bölümdeki adımları izleyin:

1. Ekranın solundaki **Kullanıcılar** sekmesini seçin.
2. **+Ekle**tuşuna basın.
3. Kullanıcı için uygun bilgileri girin.
4. **Ekle**’ye basın.

    ![Yeni kullanıcı ekleme](./media/api-management-howto-create-or-invite-developers/api-management-create-developer.png)

Varsayılan olarak, yeni oluşturulan geliştirici hesapları **Etkindir**ve **Geliştiriciler** grubuyla ilişkilidir. **Etkin** durumda olan geliştirici hesapları, abonelikleri olan TÜM API'lere erişmek için kullanılabilir. Yeni oluşturulan geliştiriciyi ek gruplarla ilişkilendirmek [için grupları geliştiricilerle nasıl ilişkilendirin.][How to associate groups with developers]

## <a name="invite-a-developer"></a><a name="invite-developer"> </a>Geliştirici davet et
Bir geliştiriciyi davet etmek için bu bölümdeki adımları izleyin:

1. Ekranın solundaki **Kullanıcılar** sekmesini seçin.
2. **+Davet Tuşuna**Basın.

Bir onay iletisi görüntülenir, ancak yeni davet edilen geliştirici daveti kabul edene kadar listede görünmez. 

Bir geliştirici davet edildiğinde, geliştiriciye bir e-posta gönderilir. Bu e-posta bir şablon kullanılarak oluşturulur ve özelleştirilebilir. Daha fazla bilgi için [bkz.][Configure email templates]

Davet kabul edildikten sonra, hesap etkin hale gelir.

## <a name="deactivate-or-reactivate-a-developer-account"></a><a name="block-developer"> </a> Geliştirici hesabını devre dışı bırakma veya yeniden etkinleştirme

Varsayılan olarak, yeni oluşturulan veya davet edilen geliştirici hesapları **Etkindir.** Geliştirici hesabını devre dışı bırakmak için **Engelle'yi**tıklatın. Engellenen bir geliştirici hesabını yeniden etkinleştirmek için **Etkinleştir'i**tıklatın. Engellenen bir geliştirici hesabı geliştirici portalına erişemez veya herhangi bir API'yi arayamaz. Bir kullanıcı hesabını silmek için **Sil'i**tıklatın.

Bir kullanıcıyı engellemek için aşağıdaki adımları izleyin.

1. Ekranın solundaki **Kullanıcılar** sekmesini seçin.
2. Engellemek istediğiniz kullanıcıyı tıklatın.
3. **Basın Bloğu**.

## <a name="reset-a-user-password"></a>Kullanıcı parolasını sıfırlama

Kullanıcı hesaplarıyla programlı olarak çalışmak [için, API Management REST API](/rest/api/apimanagement/) referansında Kullanıcı varlık belgelerine bakın. Bir kullanıcı hesabı parolasını belirli bir değere sıfırlamak için, bir kullanıcı işlemini [güncelleştir'i](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/azure-api-management-rest-api-user-entity#UpdateUser) kullanabilir ve istenen parolayı belirtebilirsiniz.

## <a name="next-steps"></a><a name="next-steps"> </a>Sonraki adımlar
Bir geliştirici hesabı oluşturulduktan sonra, bu hesabı rollerle ilişkilendirebilir ve ürünlere ve API'lere abone olabilirsiniz. Daha fazla bilgi için [grupları nasıl oluşturup kullanacağınıza][How to create and use groups]bakın.

[api-management-management-console]: ./media/api-management-howto-create-or-invite-developers/api-management-management-console.png
[api-management-add-new-user]: ./media/api-management-howto-create-or-invite-developers/api-management-add-new-user.png
[api-management-create-developer]: ./media/api-management-howto-create-or-invite-developers/api-management-create-developer.png
[api-management-invite-developer]: ./media/api-management-howto-create-or-invite-developers/api-management-invite-developer.png
[api-management-new-developer]: ./media/api-management-howto-create-or-invite-developers/api-management-new-developer.png
[api-management-invite-developer-window]: ./media/api-management-howto-create-or-invite-developers/api-management-invite-developer-window.png
[api-management-invite-developer-confirmation]: ./media/api-management-howto-create-or-invite-developers/api-management-invite-developer-confirmation.png
[api-management-pending-verification]: ./media/api-management-howto-create-or-invite-developers/api-management-pending-verification.png
[api-management-view-developer]: ./media/api-management-howto-create-or-invite-developers/api-management-view-developer.png
[api-management-reset-password]: ./media/api-management-howto-create-or-invite-developers/api-management-reset-password.png


[Create a new developer]: #create-developer
[Invite a developer]: #invite-developer
[Deactivate or reactivate a developer account]: #block-developer
[Next steps]: #next-steps
[How to create and use groups]: api-management-howto-create-groups.md
[How to associate groups with developers]: api-management-howto-create-groups.md#associate-group-developer

[Get started with Azure API Management]: get-started-create-service-instance.md
[Create an API Management service instance]: get-started-create-service-instance.md
[Configure email templates]: api-management-howto-configure-notifications.md#email-templates
