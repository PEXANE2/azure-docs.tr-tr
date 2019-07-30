---
title: Başkalarıyla işbirliği yap-LUSıS
titleSuffix: Azure Cognitive Services
description: Uygulamanın sahibi ortak çalışanlar için uygulama ekleyebilirsiniz. Bu ortak çalışanlar modeli değiştirirseniz eğitmek ve uygulamayı yayımlayın.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: article
ms.date: 07/29/2019
ms.author: diberry
ms.openlocfilehash: dbc27176e7a300c0799e326acb10b99ac663fa89
ms.sourcegitcommit: 3877b77e7daae26a5b367a5097b19934eb136350
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/30/2019
ms.locfileid: "68638130"
---
# <a name="how-to-manage-authors-and-collaborators"></a>Yazarlar ve ortak çalışanlar yönetme 

Uygulamanın sahibi ortak çalışanlar için uygulama ekleyebilirsiniz. Bu ortak çalışanlar modeli değiştirirseniz eğitmek ve uygulamayı yayımlayın. 

<a name="owner-and-collaborators"></a>

## <a name="add-collaborator"></a>Ortak çalışanı Ekle

Uygulama sahibi, tek bir yazar vardır ancak birçok ortak çalışanlar çıkarabilirsiniz. LUIS uygulamanızı düzenlemek ortak çalışanlar izin vermek için ortak çalışanlar listesini LUIS portala erişmek için kullandıkları e-posta eklemeniz gerekir. Bunlar eklendikten sonra uygulama kendi LUIS portalda görüntülenir.

1. Seçin **Yönet** seçip sağ üstteki menüden **ortak çalışanlar** soldaki menüde.

2. Seçin **ortak çalışanı Ekle** araç çubuğundan.

    [![Ekle ortak çalışanı](./media/luis-how-to-collaborate/add-collaborator.png "Ekle ortak çalışanı")](./media/luis-how-to-collaborate/add-collaborator.png#lightbox)

3. Ortak çalışanı LUIS portalda oturum açmak için kullandığı e-posta adresi girin.

    ![Ortak çalışan kişinin e-posta adresi ekleyin](./media/luis-how-to-collaborate/add-collaborator-pop-up.png)

## <a name="transfer-of-ownership"></a>Sahipliğin aktarılması

LUIS sahipliğin aktarılması şu anda desteklemiyor, ancak uygulamanızı dışa aktarabilir ve LUIS kullanıcı başka bir uygulamayı içeri aktarabilirsiniz. LUIS puanları iki uygulama arasındaki küçük farklılıklar olabilir. 

## <a name="azure-active-directory-resources"></a>Azure Active Directory kaynakları

Kuruluşunuzda [Azure Active Directory](https://docs.microsoft.com/azure/active-directory/) (Azure AD) kullanıyorsanız, Language UNDERSTANDING (lusıs), BASIS kullanmak istediklerinde kullanıcılarınızın erişimi hakkındaki bilgilere izin ister. LUIS gerektirdiği kaynakların düşüktür. 

Yönetici onayı sahip veya yönetici onayı gibi yönetici onayı gerektirmeyen bir hesapla oturum açmaya çalıştığında ayrıntılı açıklamasına bakın:

* Uygulamada Kurumsal hesabınızla oturum açın ve profilinizi okuyabilmesine sağlar. Temel şirket bilgilerini okumasına izin verir. Bu, LUO 'NUN Kullanıcı KIMLIĞI, e-posta, ad gibi temel profil verilerini okumasına izin verir
* Uygulamanın bakın ve bile uygulama kullanmakta olduğunuz değil, verilerinizin güncelleştirmesine izin verir. Kullanıcının erişim belirtecini yenilemek için izin gerekir.


## <a name="azure-active-directory-tenant-user"></a>Azure Active Directory Kiracı Kullanıcı

LUIS standart Azure Active Directory (Azure AD) onay akışı kullanır. 

Kiracı yönetici, doğrudan Azure AD'de LUIS kullanmak için verilen erişmesi gereken kullanıcı ile çalışması gerekir. 

* İlk olarak, kullanıcı HLUISimzalar ve yönetici onayı gerek açılan iletişim görür. Kullanıcı, Kiracı Yöneticisi devam etmeden önce bağlantı kurar. 
* İkinci olarak, Kiracı yönetici HLUISimzalar ve bir onay akış açılan iletişim görür. Yönetici kullanıcı için izin vermek için gereken iletişim budur. Yönetici izni kabul ettiğinde, kullanıcı ile LUIS devam edebilirsiniz. Kiracı Yöneticisi LUO 'da oturum açamayacak, yönetici, aşağıdaki ekran görüntüsünde gösterilen Lua 'ya erişim [izni](https://account.activedirectory.windowsazure.com/r#/applications) verebilir. Listenin adı `LUIS`içeren öğelere filtrelendiğine dikkat edin.

![Uygulama Web sitesi tarafından Azure active directory izni](./media/luis-how-to-collaborate/tenant-permissions.png)

Kiracı Yöneticisi yalnızca belirli kullanıcıların LUSıS kullanmasını istiyorsa, olası birçok çözüm vardır:
* "Yönetici onayı" (Azure AD 'nin tüm kullanıcılarına onay) verme, ancak ardından kurumsal uygulama özellikleri altında "Kullanıcı Ataması gerekli" olarak ayarlanır ve son olarak yalnızca istenen kullanıcıları uygulamaya atayın/ekleyin. Bu yöntemde, yönetici uygulamaya "yönetici onayı" sağlamaya devam eder, ancak buna erişebilen kullanıcıları denetlemek mümkündür.
* İkinci bir çözüm, belirli bir kullanıcıya onay sağlamak için [Azure AD Graph API](https://docs.microsoft.com/graph/azuread-identity-access-management-concept-overview) kullanmaktır. 

Azure Active Directory Kullanıcıları ve onayı hakkında daha fazla bilgi edinin: 
* [Uygulamanızı](../../active-directory/develop/howto-restrict-your-app-to-a-set-of-users.md) bir Kullanıcı kümesiyle sınırlayın

### <a name="user-accounts-with-multiple-emails-for-collaborators"></a>Ortak Çalışanlar için birden çok e-posta ile kullanıcı hesapları

Bir LUIS uygulaması için ortak çalışanlar eklerseniz, bir ortak çalışanı LUIS ortak çalışan kullanması gereken tam e-posta adresi belirtiyorsunuz. LUIS, Azure Active Directory (Azure AD) tek bir kullanıcı kullanılabileceğinden, birden fazla e-posta hesabına sahip olmasını sağlar, ancak ortak çalışanı'nın listesinde belirtilen e-posta adresiyle oturum açmasını gerektirir.

