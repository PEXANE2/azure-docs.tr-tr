---
title: Azure AD Yetkilendirme Yönetimi 'ne bağlı bir kuruluş ekleme-Azure Active Directory
description: Kuruluşunuz dışındaki kişilerin, projelerde işbirliği yapabilmeniz için erişim paketleri istemesine nasıl izin vereceğinizi öğrenin.
services: active-directory
documentationCenter: ''
author: barclayn
manager: daveba
editor: markwahl-msft
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: how-to
ms.subservice: compliance
ms.date: 06/18/2020
ms.author: barclayn
ms.reviewer: mwahl
ms.collection: M365-identity-device-management
ms.openlocfilehash: d8928e7293f184e8eb366df6a29e50cbea6a7c93
ms.sourcegitcommit: 971a3a63cf7da95f19808964ea9a2ccb60990f64
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/19/2020
ms.locfileid: "85078201"
---
# <a name="add-a-connected-organization-in-azure-ad-entitlement-management"></a>Azure AD Yetkilendirme Yönetimi 'nde bağlı bir kuruluş ekleme

Azure Active Directory (Azure AD) yetkilendirme yönetimi sayesinde, kuruluşunuzun dışındaki kişilerle işbirliği yapabilirsiniz. Bir dış Azure AD dizini veya etki alanındaki kullanıcılarla sık sık işbirliği yaptıysanız, bunları bağlı bir kuruluş olarak ekleyebilirsiniz. Bu makalede, kuruluşunuzun dışındaki kullanıcıların dizininizde kaynak istemesine izin vermek için bağlı bir kuruluşun nasıl ekleneceği açıklanmaktadır.

## <a name="what-is-a-connected-organization"></a>Bağlı kuruluş nedir?

Bağlı bir kuruluş, ile ilişkiniz olan bir dış Azure AD dizini veya etki alanıdır.

Örneğin, Woodgrove Bank 'ta çalıştığınızı ve iki harici kurum ile işbirliği yapmak istediğinizi varsayalım. Bu iki kuruluş farklı yapılandırmalara sahiptir:

- Grafik tasarımı Enstitüsü, Azure AD 'yi kullanır ve kullanıcıların *graphicdesigninstitute.com*ile biten bir Kullanıcı asıl adı vardır.
- Contoso henüz Azure AD kullanmıyor. Contoso kullanıcıları, *contoso.com*ile biten bir Kullanıcı asıl adına sahiptir.

Bu durumda, iki bağlı kuruluş yapılandırabilirsiniz. Grafik Tasarım Enstitüsü ve contoso için bir bağlı kuruluş oluşturursunuz. Daha sonra, iki bağlı kuruluş bir ilkeye eklerseniz, her bir kuruluştan, ilkeyle eşleşen bir Kullanıcı asıl adına sahip kullanıcılar erişim paketleri isteyebilir. *Graphicdesigninstitute.com* etki alanına sahip bir Kullanıcı asıl adına sahip kullanıcılar, grafik tasarımı Enstitüsü bağlantılı kuruluşla eşleşir ve istek göndermesine izin verilir. *Contoso.com* etki alanına sahip bir Kullanıcı asıl adına sahip kullanıcılar, contoso bağlantılı kuruluşla eşleştirebilir ve ayrıca paket istemesine izin verilir. Ayrıca, grafik tasarımı Enstitüsü Azure AD 'yi kullandığından, *graphicdesignınstitute. example*gibi, kiracılarına eklenen [doğrulanmış bir etki alanıyla](../fundamentals/add-custom-domain.md#verify-your-custom-domain-name) eşleşen bir asıl adı olan tüm kullanıcılar aynı ilkeyi kullanarak erişim paketleri isteyebilecektir.

![Bağlı kuruluş örneği](./media/entitlement-management-organization/connected-organization-example.png)

Azure AD dizininden veya etki alanındaki kullanıcıların kimlik doğrulaması, kimlik doğrulama türüne bağlıdır. Bağlı kuruluşların kimlik doğrulama türleri şunlardır:

- Azure AD
- [Doğrudan federasyon](../b2b/direct-federation.md)
- [Bir kerelik geçiş kodu](../b2b/one-time-passcode.md) (etki alanı)

Bağlı bir kuruluşun nasıl ekleneceğini gösteren bir gösterim için aşağıdaki videoyu izleyin:

>[!VIDEO https://www.microsoft.com/videoplayer/embed/RE4dskS]

## <a name="add-a-connected-organization"></a>Bağlı kuruluş ekleme

Dış bir Azure AD dizini veya etki alanını bağlı bir kuruluş olarak eklemek için, bu bölümdeki yönergeleri izleyin.

**Önkoşul rolü**: *genel yönetici*, *Kullanıcı Yöneticisi*veya *Konuk davetci*

1. Azure portal **Azure Active Directory**' i seçin ve ardından **Identity idare**' ı seçin.

1. Sol bölmede **bağlı kuruluşlar**' ı seçin ve ardından **bağlı kuruluş Ekle**' yi seçin.

    !["Bağlı kuruluş Ekle" düğmesi](./media/entitlement-management-organization/connected-organization.png)

1. **Temel bilgiler** sekmesini seçin ve ardından kuruluş için bir görünen ad ve açıklama girin.

    !["Bağlı kuruluş Ekle" temelleri bölmesi](./media/entitlement-management-organization/organization-basics.png)

1. **Dizin + etki alanı** sekmesini seçin ve ardından **dizin + etki alanı Ekle**' yi seçin.

    **Dizinleri seçin + etki alanları** bölmesi açılır.

1. Arama kutusuna Azure AD dizinini veya etki alanını aramak için bir etki alanı adı girin. Tüm etki alanı adını girdiğinizden emin olun.

1. Kuruluş adının ve kimlik doğrulama türünün doğru olduğundan emin olun. Kullanıcıların oturum açması, kimlik doğrulama türüne bağlıdır.

    !["Dizinleri seçin + etki alanları" bölmesi](./media/entitlement-management-organization/organization-select-directories-domains.png)

1. Azure AD dizinini veya etki alanını eklemek için **Ekle** ' yi seçin. Şu anda, bağlı kuruluşa göre yalnızca bir Azure AD dizini veya etki alanı ekleyebilirsiniz.

    > [!NOTE]
    > Azure AD dizini veya etki alanındaki tüm kullanıcılar bu erişim paketini isteyebilecektir. Bu, Azure AD iş 'ten iş (B2B) izin verme veya reddetme listesi tarafından engellenmediği takdirde, bu etki alanları, dizinle ilişkili tüm alt etki alanlarından Azure AD 'deki kullanıcıları içerir. Daha fazla bilgi için, bkz. [belirli kuruluşlardan B2B kullanıcılarına Izin verme veya bu kullanıcıların davetlerini engelleme](../b2b/allow-deny-list.md).

1. Azure AD dizinini veya etki alanını ekledikten sonra **Seç**' i seçin.

    Kuruluş listede görüntülenir.

    !["Dizin + etki alanı" bölmesi](./media/entitlement-management-organization/organization-directory-domain.png)

1. **Sponsorlar** sekmesini seçin ve ardından bu bağlı kuruluş için isteğe bağlı sponsorlar ekleyin.

    Sponsorlar, dizininizde bulunan ve bu bağlı kuruluşla ilişki için iletişim noktası olan iç veya dış kullanıcılardır. İç sponsorlar, dizininizdeki üye kullanıcılardır. Dış sponsorlar, daha önce davet edilmiş ve dizininizde zaten olan bağlı kuruluştan Konuk kullanıcılardır. Bu bağlı kuruluştaki kullanıcılar bu erişim paketine erişim isteğinde bulunduğunda, sponsorlar olarak kullanılabilir. Bir konuk kullanıcıyı dizininize davet etme hakkında daha fazla bilgi için, bkz. [Azure portal Azure ACTIVE DIRECTORY B2B işbirliği kullanıcıları ekleme](../b2b/add-users-administrator.md).

    **Ekle/Kaldır**' ı seçtiğinizde, içinde iç veya dış sponsorları seçebileceğiniz bir bölme açılır. Bölmesinde, dizininizdeki kullanıcıların ve grupların filtrelenmemiş bir listesi görüntülenir.

    ![Sponsorlar bölmesi](./media/entitlement-management-organization/organization-sponsors.png)

1. **Gözden geçir + oluştur** sekmesini seçin, kuruluş ayarlarınızı gözden geçirin ve ardından **Oluştur**' u seçin.

    !["Gözden geçir + oluştur" bölmesi](./media/entitlement-management-organization/organization-review-create.png)

## <a name="update-a-connected-organization"></a>Bağlı bir kuruluşu güncelleştirme 

Bağlı kuruluş farklı bir etki alanına değişirse, kuruluşun adı değişir veya sponsorları değiştirmek istiyorsanız, bu bölümdeki yönergeleri izleyerek bağlı organizasyonu güncelleştirebilirsiniz.

**Önkoşul rolü**: *genel yönetici*, *Kullanıcı Yöneticisi*veya *Konuk davetci*

1. Azure portal **Azure Active Directory**' i seçin ve ardından **Identity idare**' ı seçin.

1. Sol bölmede **bağlı kuruluşlar**' ı seçin ve sonra açmak için bağlı kuruluş ' u seçin.

1. Bağlı kuruluşun Genel Bakış bölmesinde, kuruluş adını veya açıklamasını değiştirmek için **Düzenle** ' yi seçin.  

1. **Dizin + etki** alanı bölmesinde, farklı bir dizin veya etki alanına geçmek için **dizin + etki alanını güncelleştir** ' i seçin.

1. **Sponsorlar** bölmesinde, **dahili sponsorlar Ekle** ' yi seçin veya **Harici sponsorlar ekleyerek** bir kullanıcıyı sponsor olarak ekleyin. Sponsor öğesini kaldırmak için sponsor öğesini seçin ve sağ bölmedeki **Sil**' i seçin.


## <a name="delete-a-connected-organization"></a>Bağlı bir kuruluşu silme

Artık dış Azure AD dizini veya etki alanı ile bir ilişkiniz yoksa bağlı kuruluşu silebilirsiniz.

**Önkoşul rolü**: *genel yönetici*, *Kullanıcı Yöneticisi*veya *Konuk davetci*

1. Azure portal **Azure Active Directory**' i seçin ve ardından **Identity idare**' ı seçin.

1. Sol bölmede **bağlı kuruluşlar**' ı seçin ve sonra açmak için bağlı kuruluş ' u seçin.

1. Bağlı kuruluşun genel bakış **bölmesinde Sil ' i seçerek silin** .

    Şu anda bağlı bir kuruluşu yalnızca bağlı kullanıcı yoksa silebilirsiniz.

    ![Bağlı kuruluş Sil düğmesi](./media/entitlement-management-organization/organization-delete.png)

## <a name="next-steps"></a>Sonraki adımlar

- [Dış kullanıcılara yönelik idare erişimi](https://docs.microsoft.com/azure/active-directory/governance/entitlement-management-external-users)
- [Dizininizde bulunmayan kullanıcılar için erişimi yönetir](entitlement-management-access-package-request-policy.md#for-users-not-in-your-directory)
