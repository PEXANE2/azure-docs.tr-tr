---
title: Başkalarıyla işbirliği yapın - LUIS
titleSuffix: Azure Cognitive Services
description: Uygulama sahibi, yazarlık kaynağına katkıda bulunanları ekleyebilir. Bu katkıda bulunanlar modeli değiştirebilir, eğitebilir ve uygulamayı yayımlayabilir.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 11/19/2019
ms.author: diberry
ms.openlocfilehash: 913a2b26f67773d9fafbc0a8430d121fbabb97cd
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80053451"
---
# <a name="add-contributors-to-your-app"></a>Uygulamanıza katkıda bulunanları ekleme

Bir uygulama sahibi uygulamalara katkıda bulunabilir. Bu ortak çalışanlar modeli değiştirebilir, eğitebilir ve uygulamayı yayımlayabilir. 

[!INCLUDE [Manage collaborators and contributors based on migrated or not-migrated apps](./includes/manage-contributor-collaborator-migration.md)]

## <a name="add-contributor-to-azure-authoring-resource"></a>Azure yazma kaynağına katkıda bulunan ekleme

Aşağıdaki yordam, Azure yazma kaynağını kullanmak için **geçiş** yapan tüm kullanıcılar içindir.

LUIS yazma deneyiminiz, LUIS portalındaki **Yönet -> Azure kaynakları** sayfasındaki bir Yazma kaynağına bağlıysa, geçiş yapmışsınızdır.

1. Azure portalında Dil Bilgisi (LUIS) yazarlık kaynağını bulun. Bu türü `LUIS.Authoring`vardır.
1. Bu kaynağın **Erişim Denetimi (IAM)** sayfasında **+Ekle'yi** seçin ve **ardından rol ataması ekle'yi**seçin.

    ![Azure portalında, yazma kaynağına rol ataması ekleyin.](./media/luis-how-to-collaborate/authoring-resource-access-control-add-role.png)

1. Rol **atama ekle** penceresinde Katılımcının **Rolünü** seçin. **Access'e Atla seçeneğinde** **Azure AD kullanıcısını, grubunu veya hizmet sorumlusunu**seçin. **Seç** seçeneğine, kullanıcının e-posta adresini girin. Kullanıcı aynı etki alanı için 1'den fazla e-posta adresi yle tanınıyorsa, _birincil_ e-posta hesabını girdiğinden emin olun.

    ![Azure AD için katılımcı rolüne kullanıcının e-postasını ekleme](./media/luis-how-to-collaborate/add-role-assignment-for-contributor.png)

    Kullanıcının e-postası bulunduğunda hesabı seçin ve **Kaydet'i**seçin. 

    Bu rol atamasıyla ilgili sorun yaşıyorsanız, [Azure rol atamalarını](../../role-based-access-control/role-assignments-portal.md) ve [Azure erişim denetimi sorun giderme olayını](../../role-based-access-control/troubleshooting.md#problems-with-azure-role-assignments)gözden geçirin.

## <a name="add-collaborator-to-luis-app"></a>LUIS uygulamasına ortak layıcı ekleme

Aşağıdaki yordam, Azure yazma kaynağını kullanmak için **geçiş yapmayan** tüm kullanıcılar içindir.

LUIS yazma deneyiminiz, LUIS portalındaki **Yönet -> Azure kaynakları** sayfasındaki bir Yazarlık kaynağına bağlı değilse, geçiş yapmadınız.

Bir uygulamanın sahibi olan tek bir yazar vardır, ancak birçok ortak çalışan olabilir. Ortak layıcıların LUIS uygulamanızı yeniden atabilmeleri için, LUIS portalına erişmek için kullandıkları e-postayı ortak çalışanlar listesine eklemeniz gerekir. Eklendikten sonra uygulama LUIS portalında gösterir.

1. Sağ üst menüden **Yönet'i** seçin ve sol menüde **İşbirliği Yapanlar'ı** seçin.

1. Araç çubuğundan **Ortak İşbirlikçi Ekle'yi** seçin.

1. Ortak bilginin LUIS portalında oturum açarken kullandığı e-posta adresini girin.

    ![İş birlikçi sortun e-posta adresini ekleme](./media/luis-how-to-collaborate/add-collaborator-pop-up.png)


### <a name="users-with-multiple-emails"></a>Birden çok e-postası olan kullanıcılar 

Bir LUIS uygulamasına katkıda bulunanları/ortak çalışanları eklerseniz, tam e-posta adresini belirtirsiniz. Azure Etkin Dizini (Azure AD), tek bir kullanıcının birbirinin yerine birden fazla e-posta hesabı kullanmasına izin verirken, LUIS kullanıcının katılımcıyı/ortak çalışanı eklerken belirtilen e-posta adresiyle oturum açmasını gerektirir.

<a name="owner-and-collaborators"></a>

### <a name="azure-active-directory-resources"></a>Azure Etkin Dizin kaynakları

Kuruluşunuzdaki [Azure Etkin Dizin'i](https://docs.microsoft.com/azure/active-directory/) (Azure AD) kullanıyorsanız, Dil Anlayışı (LUIS) kullanıcılarınızın LUIS'i kullanmak istediklerinde erişimleri hakkındaki bilgiler için izin almanız gerekir. LUIS'in ihtiyaç duyduğu kaynaklar çok azdır. 

Yönetici onayı olan veya yönetici onayı gibi yönetici onayı gerektirmeyen bir hesapla kaydolmaya çalıştığınızda ayrıntılı açıklamayı görürsünüz:

* Kuruluş hesabınızla uygulamada oturum açmanızı ve uygulamanın profilinizi okumasına olanak tanır. Ayrıca uygulamanın temel şirket bilgilerini okumasına da olanak tanır. Bu, LUIS'e kullanıcı kimliği, e-posta, ad gibi temel profil verilerini okuma izni verir
* Uygulamayı şu anda kullanmamış olsanız bile uygulamanın verilerinizi görmesini ve güncellemesine olanak tanır. Kullanıcının erişim belirteci yenilemek için izin gereklidir.


### <a name="azure-active-directory-tenant-user"></a>Azure Active Directory kiracı kullanıcı

LUIS, standart Azure Etkin Dizin (Azure AD) onay akışını kullanır. 

Kiracı yönetici, Azure AD'de LUIS'i kullanmak için erişim ekive ihtiyacı olan kullanıcıyla doğrudan çalışmalıdır. 

* İlk olarak, kullanıcı LUIS'e girer ve yönetici onayına ihtiyaç duyan açılır iletişim kutusunu görür. Kullanıcı devam etmeden önce kiracı yöneticiyle bağlantı kurur. 
* İkinci olarak, kiracı yönetici LUIS'e girer ve bir onay akışı açılır iletişim kutusu görür. Bu, yöneticinin kullanıcı için izin vermesi gereken iletişim kutusudur. Yönetici izni kabul ettikten sonra, kullanıcı LUIS ile devam edebiliyor. Kiracı yönetici LUIS'te oturum açmazsa, yönetici aşağıdaki ekran görüntüsünde gösterilen LUIS [için onay](https://account.activedirectory.windowsazure.com/r#/applications) alabilirsiniz. Listenin adı `LUIS`içeren öğelere filtre li olduğuna dikkat edin.

![Uygulama web sitesi tarafından Azure etkin dizin izni](./media/luis-how-to-collaborate/tenant-permissions.png)

Kiracı yönetici yalnızca belirli kullanıcıların LUIS'i kullanmasını istiyorsa, birkaç olası çözüm vardır:
* "Yönetici onayı" (Azure AD'nin tüm kullanıcılarına onay vermek), ancak ardından Kurumsal Uygulama Özellikleri kapsamında "Gerekli Kullanıcı ataması" için "Evet" olarak ayarlanır ve son olarak yalnızca aranan kullanıcıları Uygulamaya atama/ekleme. Bu yöntemle, Yönetici hala Uygulamaya "yönetici onayı" sağlıyor, ancak uygulamaya erişebilen kullanıcıları denetlemek mümkündür.
* İkinci bir çözüm, her bir kullanıcıya onay sağlamak için [Microsoft Graph'taki Azure AD kimliği ve erişim yönetimi API'sini](https://docs.microsoft.com/graph/azuread-identity-access-management-concept-overview) kullanmaktır. 

Azure etkin dizin kullanıcıları ve onayı hakkında daha fazla bilgi edinin: 
* [Uygulamanızı](../../active-directory/develop/howto-restrict-your-app-to-a-set-of-users.md) bir kullanıcı kümesiyle sınırlama

## <a name="next-steps"></a>Sonraki adımlar

* Uygulama yaşam döngünüzün denetimini sağlamak için [sürümleri nasıl kullanacağınızı](luis-how-to-manage-versions.md) öğrenin.
* [Yazarlık kaynağı](luis-concept-keys.md#authoring-key) ve bu kaynaktaki [katkıda bulunanlar](luis-concept-keys.md#contributions-from-other-authors) gibi kavramları anlayın.
* Yazarlık ve çalışma zamanı kaynaklarını [nasıl oluşturup oluşturabilirsiniz](luis-how-to-azure-subscription.md) öğrenin
* Yeni [yazma kaynağına](luis-migration-authoring.md) geçirin 
