---
title: Azure AD yetkilendirme yönetiminde bağlı bir kuruluş ekleme - Azure Etkin Dizin
description: Projelerde işbirliği yapabilmeniz için kuruluşunuz dışındaki kişilerin erişim paketleri istemesine nasıl izin verebileceğinizi öğrenin.
services: active-directory
documentationCenter: ''
author: barclayn
manager: daveba
editor: markwahl-msft
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.subservice: compliance
ms.date: 03/22/2020
ms.author: barclayn
ms.reviewer: mwahl
ms.collection: M365-identity-device-management
ms.openlocfilehash: ee370bc9c381eb11ae7cae53b31d0c987c52733c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80128602"
---
# <a name="add-a-connected-organization-in-azure-ad-entitlement-management"></a>Azure AD yetkilendirme yönetiminde bağlı bir kuruluş ekleme

Azure Etkin Dizin (Azure AD) yetkilendirme yönetimi yle kuruluşunuz dışındaki kişilerle işbirliği yapabilirsiniz. Harici bir Azure REKLAM dizininde veya etki alanında kullanıcılarla sık sık işbirliği yaparsa, bunları bağlı bir kuruluş olarak ekleyebilirsiniz. Bu makalede, kuruluşunuz dışındaki kullanıcıların dizininizde kaynak istemesine izin vermek için bağlı bir kuruluşun nasıl ekleyeceğiniz açıklanmaktadır.

## <a name="what-is-a-connected-organization"></a>Bağlı kuruluş nedir?

Bağlı kuruluş, ilişki içinde olduğunuz harici bir Azure REKLAM dizini veya etki alanıdır.

Örneğin, Woodgrove Bank'ta çalıştığınızı ve iki dış kuruluşla işbirliği yapmak istediğinizi varsayalım. Bu iki kuruluşun farklı yapılandırmaları vardır:

- Grafik Tasarım Enstitüsü Azure AD kullanır ve kullanıcılarının *graphicdesigninstitute.com*ile biten bir kullanıcı ana adı vardır.
- Contoso henüz Azure AD kullanmıyor. Contoso kullanıcıları *nın contoso.com*ile biten bir kullanıcı ana adı vardır.

Bu durumda, iki bağlı kuruluş yapılandırabilirsiniz. Grafik Tasarım Enstitüsü için bir bağlı kuruluş ve Contoso için bir tane oluşturursunuz. Daha sonra bağlı iki kuruluşu bir ilkeye eklerseniz, ilkeyle eşleşen bir kullanıcı adı olan her kuruluştan kullanıcılar erişim paketleri isteyebilir. *graphicdesigninstitute.com* etki alanına sahip bir kullanıcı ana adı olan kullanıcılar, Grafik Tasarım Enstitüsü'ne bağlı kuruluşla eşleşir ve istek göndermesine izin verilir. *Contoso.com* etki alanına sahip bir kullanıcı ana adı olan kullanıcılar Contoso'ya bağlı kuruluşla eşleşir ve paket istemelerine de izin verilir. Grafik Tasarım Enstitüsü Azure AD kullandığından, yalnızca bir ana adla eşleşen ve *yalnızca grafik tasarım enstitüsü.example*gibi doğrulanmış bir etki [alanıyla](../fundamentals/add-custom-domain.md#verify-your-custom-domain-name) eşleşen kullanıcılar da aynı ilkeyi kullanarak erişim paketleri isteyebilir.

![Bağlı kuruluş örneği](./media/entitlement-management-organization/connected-organization-example.png)

Azure AD dizinindeki veya etki alanı kimliği doğrulaması olan kullanıcıların kimlik doğrulama türüne nasıl bağlıdır. Bağlı kuruluşlar için kimlik doğrulama türleri şunlardır:

- Azure AD
- [Doğrudan federasyon](../b2b/direct-federation.md)
- [Tek seferlik geçiş kodu](../b2b/one-time-passcode.md) (etki alanı)

Bağlı bir kuruluşun nasıl ekleyeceğinigöstermek için aşağıdaki videoyu izleyin:

>[!VIDEO https://www.microsoft.com/videoplayer/embed/RE4dskS]

## <a name="add-a-connected-organization"></a>Bağlı kuruluş ekleme

Bağlı kuruluş olarak harici bir Azure REKLAM dizini veya etki alanı eklemek için bu bölümdeki yönergeleri izleyin.

**Önkoşul rolü**: *Global yönetici*, Kullanıcı *yöneticisi*veya Konuk *davetli*

1. Azure portalında **Azure Etkin Dizin'i**seçin ve ardından **Kimlik Yönetimi'ni**seçin.

1. Sol bölmede, **Bağlı kuruluşlar'ı**seçin ve ardından **bağlı kuruluş ekle'yi**seçin.

    !["Bağlı kuruluş ekle" düğmesi](./media/entitlement-management-organization/connected-organization.png)

1. Temel **Bilgiler** sekmesini seçin ve ardından kuruluş için bir görüntü adı ve açıklama girin.

    !["Bağlı kuruluş ekle" Temelleri bölmesi](./media/entitlement-management-organization/organization-basics.png)

1. **Dizin + etki alanı** sekmesini seçin ve ardından **Dizin Ekle + etki alanı'nı**seçin.

    **Seç dizinleri + etki alanları** bölmesi açılır.

1. Arama kutusuna, Azure AD dizinini veya etki alanını aramak için bir etki alanı adı girin. Tüm etki alanı adını girdiğimden emin olun.

1. Kuruluş adının ve kimlik doğrulama türünün doğru olduğunu doğrulayın. Kullanıcıların nasıl oturum açmaları kimlik doğrulama türüne bağlıdır.

    !["Dizinseçinler + etki alanları seç" bölmesi](./media/entitlement-management-organization/organization-select-directories-domains.png)

1. Azure AD dizinini veya etki alanını eklemek için **Ekle'yi** seçin. Şu anda, bağlı kuruluş başına yalnızca bir Azure REKLAM dizini veya etki alanı ekleyebilirsiniz.

    > [!NOTE]
    > Azure AD dizinindeki veya etki alanından tüm kullanıcılar bu erişim paketini talep edebilecektir. Bu, bu etki alanları Azure AD işletmesi tarafından engellenmedikçe (B2B) listeye izin verdiği veya reddetmediği sürece, dizinle ilişkili tüm alt etki alanlarından Azure AD'deki kullanıcıları içerir. Daha fazla bilgi için [bkz.](../b2b/allow-deny-list.md)

1. Azure AD dizinini veya etki alanını ekledikten sonra **Seç'i**seçin.

    Kuruluş listede görünür.

    !["Dizin + etki alanı" bölmesi](./media/entitlement-management-organization/organization-directory-domain.png)

1. **Sponsorlar** sekmesini seçin ve ardından bu bağlı kuruluş için isteğe bağlı sponsorlar ekleyin.

    Sponsorlar, bu bağlı kuruluşla ilişki için temas noktası olan dizininizde zaten iç veya dış kullanıcılardır. Dahili sponsorlar dizininizdeki üye kullanıcılardır. Dış sponsorlar, daha önce davet edilen ve zaten dizininizde olan bağlı kuruluştan konuk kullanıcılardır. Bu bağlı kuruluştaki kullanıcılar bu erişim paketine erişim istediğinde sponsorlar onaylayıcı olarak kullanılabilir. Konuk kullanıcıyı dizininize nasıl davet edeceğihakkında bilgi için Azure [portalındaki Azure Etkin Dizin B2B işbirliği kullanıcıları ekle'ye](../b2b/add-users-administrator.md)bakın.

    **Ekle/Kaldır'ı**seçtiğinizde, iç veya dış sponsorları seçebileceğiniz bir bölme açılır. Bölme, dizininizde filtre uygulanmamış bir kullanıcı ve grup listesini görüntüler.

    ![Sponsorlar bölmesi](./media/entitlement-management-organization/organization-sponsors.png)

1. Gözden **Geçir + oluştur** sekmesini seçin, kuruluş ayarlarınızı gözden geçirin ve sonra **Oluştur'u**seçin.

    !["Gözden Geçirme + oluştur" bölmesi](./media/entitlement-management-organization/organization-review-create.png)

## <a name="update-a-connected-organization"></a>Bağlı bir kuruluşu güncelleştirme 

Bağlı kuruluş farklı bir etki alanına değişirse, kuruluşun adı değişirse veya sponsorları değiştirmek isterseniz, bu bölümdeki yönergeleri izleyerek bağlı kuruluşu güncelleştirebilirsiniz.

**Önkoşul rolü**: *Global yönetici*, Kullanıcı *yöneticisi*veya Konuk *davetli*

1. Azure portalında **Azure Etkin Dizin'i**seçin ve ardından **Kimlik Yönetimi'ni**seçin.

1. Sol bölmede, **Bağlı kuruluşlar'ı**seçin ve ardından açmak için bağlı kuruluşu seçin.

1. Bağlı kuruluşun genel bakış bölmesinde, kuruluş adını veya açıklamasını değiştirmek için **Düzenleme'yi** seçin.  

1. **Dizin + etki alanı** bölmesinde, farklı bir dizin veya etki alanına değiştirmek için **Dizin + etki alanını güncelleştir'i** seçin.

1. **Sponsorlar** bölmesinde, bir kullanıcıyı sponsor olarak eklemek için **dahili sponsor ekle** veya **dış sponsor ekle'yi** seçin. Sponsoru kaldırmak için sponsoru seçin ve sağ bölmede **Sil'i**seçin.


## <a name="delete-a-connected-organization"></a>Bağlı bir kuruluşu silme

Harici bir Azure REKLAM dizini veya etki alanıyla artık bir ilişkiniz yoksa, bağlı kuruluşu silebilirsiniz.

**Önkoşul rolü**: *Global yönetici*, Kullanıcı *yöneticisi*veya Konuk *davetli*

1. Azure portalında **Azure Etkin Dizin'i**seçin ve ardından **Kimlik Yönetimi'ni**seçin.

1. Sol bölmede, **Bağlı kuruluşlar'ı**seçin ve ardından açmak için bağlı kuruluşu seçin.

1. Bağlı kuruluşun genel bakış bölmesinde, silmek için **Sil'i** seçin.

    Şu anda, bağlı bir kuruluşu yalnızca bağlı kullanıcı yoksa silebilirsiniz.

    ![Bağlı kuruluş Sil düğmesi](./media/entitlement-management-organization/organization-delete.png)

## <a name="next-steps"></a>Sonraki adımlar

- [Dış kullanıcılara yönelik idare erişimi](https://docs.microsoft.com/azure/active-directory/governance/entitlement-management-external-users)
- [Dizininizde olmayan kullanıcılar için erişimi yönetme](entitlement-management-access-package-request-policy.md#for-users-not-in-your-directory)
