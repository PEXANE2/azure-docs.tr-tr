---
title: Azure AD Yetkilendirme Yönetimi 'ne bağlı bir kuruluş ekleme-Azure Active Directory
description: Kuruluşunuz dışındaki kişilerin, projelerde işbirliği yapabilmeniz için erişim paketleri istemesine nasıl izin vereceğinizi öğrenin.
services: active-directory
documentationCenter: ''
author: msaburnley
manager: daveba
editor: markwahl-msft
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.subservice: compliance
ms.date: 01/22/2020
ms.author: ajburnle
ms.reviewer: mwahl
ms.collection: M365-identity-device-management
ms.openlocfilehash: 1ecf3a8819518c674a3d8bd7af55d1a3c6393c42
ms.sourcegitcommit: 98a5a6765da081e7f294d3cb19c1357d10ca333f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/20/2020
ms.locfileid: "77483865"
---
# <a name="add-a-connected-organization-in-azure-ad-entitlement-management"></a>Azure AD Yetkilendirme Yönetimi 'nde bağlı bir kuruluş ekleme

Azure AD Yetkilendirme Yönetimi, kuruluşunuzun dışındaki kişilerle işbirliği yapmanızı sağlar. Bir dış Azure AD dizini veya etki alanındaki kullanıcılarla sık sık işbirliği yaptıysanız, bunları bağlı bir kuruluş olarak ekleyebilirsiniz. Bu makalede, kuruluşunuzun dışındaki kullanıcıların dizininizde kaynak istemesine izin vermek için bağlı bir kuruluşun nasıl ekleneceği açıklanmaktadır.

## <a name="what-is-a-connected-organization"></a>Bağlı kuruluş nedir?

Bağlı bir kuruluş, ile ilişkiniz olan bir dış Azure AD dizini veya etki alanıdır.

Örneğin, Woodgrove Bank 'ta çalıştığınızı ve iki harici kurum ile işbirliği yapmak istediğinizi varsayalım. Bu iki kuruluş farklı yapılandırmalara sahiptir:

- Grafik tasarımı Enstitüsü, Azure AD kullanır ve kullanıcıların `graphicdesigninstitute.com` ile biten bir Kullanıcı asıl adı vardır
- Contoso henüz Azure AD kullanmıyor. Contoso kullanıcıları `contoso.com`ile biten bir Kullanıcı asıl adına sahiptir.

Bu durumda, iki bağlı kuruluş yapılandırabilirsiniz. Grafik Tasarım Enstitüsü için bir bağlı kuruluş ve contoso için bir tane oluşturabilirsiniz. Daha sonra bu iki bağlı kuruluşu bir ilkeye eklerseniz, her kuruluştan Kullanıcı asıl adı ilkeyle eşleşen kullanıcılar erişim paketleri isteyebilir. Graphicdesigninstitute.com etki alanına sahip bir Kullanıcı asıl adına sahip olan kullanıcılar, grafik tasarımı Enstitüsü bağlı kuruluşla eşleşir ve bir etki alanına sahip olan Kullanıcı asıl adına sahip kullanıcılar da eşleşirken istek göndermesine izin verilir Contoso bağlantılı kuruluşun ve ayrıca paket istemesine izin verilir. Ayrıca, grafik tasarımı Enstitüsü Azure AD ' yi kullandığından, bir asıl adına sahip olan ve graphicdesignınstitute gibi kiracılarına eklenen [doğrulanmış bir etki alanıyla](../fundamentals/add-custom-domain.md#verify-your-custom-domain-name) eşleşen tüm kullanıcılar. Örneğin, aynı ilkeyi kullanarak erişim paketleri de isteyebilecektir.

![Bağlı kuruluş örneği](./media/entitlement-management-organization/connected-organization-example.png)

Azure AD dizininden veya etki alanından kullanıcıların kimlik doğrulaması, kimlik doğrulama türüne bağlıdır. Bağlı kuruluşların kimlik doğrulama türleri şunlardır:

- Azure AD
- [Doğrudan Federasyon](../b2b/direct-federation.md)
- [Bir kerelik geçiş kodu](../b2b/one-time-passcode.md) (etki alanı)

Bağlı bir kuruluşun nasıl ekleneceğini gösteren bir gösterim için aşağıdaki videoyu izleyin:

>[!VIDEO https://www.microsoft.com/videoplayer/embed/RE4dskS]

## <a name="add-a-connected-organization"></a>Bağlı kuruluş ekleme

Bağlı bir kuruluş olarak dış bir Azure AD dizini veya etki alanı eklemek için bu adımları izleyin.

**Önkoşul rolü:** Genel yönetici, Kullanıcı Yöneticisi veya Konuk davetci

1. Azure portal, **Azure Active Directory** ' a ve ardından **kimlik**Yönetimi ' ne tıklayın.

1. Sol menüde **bağlı kuruluşlar** ' a ve ardından **bağlı kuruluş Ekle**' ye tıklayın.

    ![Kimlik yönetimi bağlı kuruluşlar-bağlı kuruluş ekleme](./media/entitlement-management-organization/connected-organization.png)

1. **Temel bilgiler** sekmesinde, kuruluş için bir görünen ad ve açıklama girin.

    ![Bağlı kuruluş Ekle-temel bilgiler sekmesi](./media/entitlement-management-organization/organization-basics.png)

1. **Dizin + etki** alanı sekmesinde **Dizin Ekle + etki alanı** ' na tıklayarak dizinler ve etki alanları seçin bölmesini açın.

1. Azure AD dizinini veya etki alanını aramak için bir etki alanı adı yazın. Tüm etki alanı adını yazmanız gerekir.

1. Belirtilen ad ve kimlik doğrulama türü tarafından doğru kuruluş olduğunu doğrulayın. Kullanıcıların oturum açması, kimlik doğrulama türüne bağlıdır.

    ![Bağlı kuruluş ekleme-dizinler + etki alanları seçin](./media/entitlement-management-organization/organization-select-directories-domains.png)

1. Azure AD dizinini veya etki alanını eklemek için **Ekle** ' ye tıklayın. Şu anda, bağlı kuruluşa göre yalnızca bir Azure AD dizini veya etki alanı ekleyebilirsiniz.

    > [!NOTE]
    > Azure AD dizini veya etki alanındaki tüm kullanıcılar bu erişim paketini isteyebilecektir. Bu, Azure AD 'deki kullanıcıları, bu etki alanları Azure B2B izin verilenler veya engellenenler listesi tarafından engellenmediği durumlar dışında, dizinle ilişkili tüm alt etki alanlarından de içerir. Daha fazla bilgi için, bkz. [belirli kuruluşlardan B2B kullanıcılarına Izin verme veya bu kullanıcıların davetlerini engelleme](../b2b/allow-deny-list.md).

1. Azure AD dizinini veya etki alanını ekledikten sonra **Seç**' e tıklayın.

    Kuruluş listede görüntülenir.

    ![Bağlı kuruluş-dizinler sekmesi ekleme](./media/entitlement-management-organization/organization-directory-domain.png)

1. **Sponsorlar** sekmesinde, bu bağlı kuruluş için isteğe bağlı sponsorlar ekleyin.

    Sponsorlar, dizininizde bulunan ve bu bağlı kuruluşla ilişki için iletişim noktası olan iç veya dış kullanıcılardır. İç sponsorlar, dizininizdeki üye kullanıcılardır. Dış sponsorlar, daha önce davet edilmiş ve dizininizde zaten olan bağlı kuruluştan Konuk kullanıcılardır. Bu bağlı kuruluştaki kullanıcılar bu erişim paketine erişim isteğinde bulunduğunda, sponsorlar olarak kullanılabilir. Bir konuk kullanıcıyı dizininize davet etme hakkında daha fazla bilgi için, bkz. [Azure portal Azure ACTIVE DIRECTORY B2B işbirliği kullanıcıları ekleme](../b2b/add-users-administrator.md).

    **Ekle/Kaldır**' a tıkladığınızda, iç veya dış sponsorları seçmek için bir bölme belirir. Bölmesinde, dizininizdeki kullanıcıların ve grupların filtrelenmemiş bir listesi görüntülenir.

    ![Erişim paketi-Ilke-bağlı kuruluş ekleme-sponsorlar sekmesi](./media/entitlement-management-organization/organization-sponsors.png)

1. **Gözden geçir + oluştur** sekmesinde kuruluş ayarlarınızı gözden geçirin ve ardından **Oluştur**' a tıklayın.

    ![Erişim paketi-Ilke-bağlı kuruluş ekleme-gözden geçirme + Oluştur sekmesi](./media/entitlement-management-organization/organization-review-create.png)

## <a name="update-a-connected-organization"></a>Bağlı bir kuruluşu güncelleştirme 

Bağlı kuruluş farklı bir etki alanına değişirse, bu kuruluş için yeni bir adınız varsa veya sponsorları değiştirmek istiyorsanız bağlı kuruluşu güncelleştirebilirsiniz.

**Önkoşul rolü:** Genel yönetici, Kullanıcı Yöneticisi veya Konuk davetci

1. Azure portal, **Azure Active Directory** ' a ve ardından **kimlik**Yönetimi ' ne tıklayın.

1. Sol menüde **bağlı kuruluşlar** ' a ve ardından bağlı kuruluşu açmak için ' ye tıklayın.

1. Genel Bakış sayfasında, kuruluş adını veya açıklamasını değiştirmek için **Düzenle** ' ye tıklayın.  

1. Dizin + etki alanı sayfasında, farklı bir dizin veya etki alanına geçmek için **dizin + etki alanını güncelleştir** ' e tıklayın.

1. Sponsorlar sayfasında, **dahili sponsorlar Ekle** ' ye tıklayın veya **Harici sponsorlar ekleyerek** bir kullanıcıyı sponsor olarak ekleyin.  Bir sponsor kaldırmak için, sponsor öğesine tıklayın ve sağ taraftaki menüden **Sil**' e tıklayın.


## <a name="delete-a-connected-organization"></a>Bağlı bir kuruluşu silme

Artık dış Azure AD dizini veya etki alanı ile bir ilişkiniz yoksa bağlı kuruluşu silebilirsiniz.

**Önkoşul rolü:** Genel yönetici, Kullanıcı Yöneticisi veya Konuk davetci

1. Azure portal, **Azure Active Directory** ' a ve ardından **kimlik**Yönetimi ' ne tıklayın.

1. Sol menüde **bağlı kuruluşlar** ' a ve ardından bağlı kuruluşu açmak için ' ye tıklayın.

1. Bağlı kuruluşu silmek için genel bakış sayfasında **Sil** ' e tıklayın.

    Şu anda bağlı bir kuruluşu yalnızca bağlı kullanıcı yoksa silebilirsiniz.

    ![Kimlik yönetimi bağlı kuruluşlar-bağlı kuruluşu silme](./media/entitlement-management-organization/organization-delete.png)

## <a name="next-steps"></a>Sonraki adımlar

- [Dış kullanıcılar için erişimi yönetir](https://docs.microsoft.com/azure/active-directory/governance/entitlement-management-external-users)
- [Dizininizde bulunmayan kullanıcılar için](entitlement-management-access-package-request-policy.md#for-users-not-in-your-directory)
