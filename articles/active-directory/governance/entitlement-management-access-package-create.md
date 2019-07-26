---
title: Azure AD Yetkilendirme Yönetimi 'nde yeni bir erişim paketi oluşturma (Önizleme)-Azure Active Directory
description: Azure Active Directory yetkilendirme yönetimi 'nde (Önizleme) paylaşmak istediğiniz yeni bir kaynak paketi oluşturmayı öğrenin.
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
ms.date: 07/23/2019
ms.author: ajburnle
ms.reviewer: ''
ms.collection: M365-identity-device-management
ms.openlocfilehash: 83eee019ee8530297689b85e6f3300fed4392610
ms.sourcegitcommit: bafb70af41ad1326adf3b7f8db50493e20a64926
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/25/2019
ms.locfileid: "68489189"
---
# <a name="create-a-new-access-package-in-azure-ad-entitlement-management-preview"></a>Azure AD Yetkilendirme Yönetimi 'nde yeni bir erişim paketi oluşturma (Önizleme)

> [!IMPORTANT]
> Azure Active Directory (Azure AD) yetkilendirme yönetimi şu anda genel önizleme aşamasındadır.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir.
> Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Erişim paketi, erişim paketinin ömrü boyunca erişimi otomatik olarak yöneten kaynak ve ilkelerin bir kerelik kurulumunu yapmanızı sağlar. Bu makalede, yeni bir erişim paketinin nasıl oluşturulacağı açıklanır.

## <a name="overview"></a>Genel Bakış

Tüm erişim paketleri, Katalog adlı bir kapsayıcıya yerleştirilmelidir. Katalog, erişim paketinize ekleyebileceğiniz kaynakları tanımlar. Bir katalog belirtmezseniz, erişim paketiniz genel kataloğa konur. Şu anda, var olan bir erişim paketini farklı bir kataloğa taşıyamazsınız.

Tüm erişim paketlerinde en az bir ilke olmalıdır. İlkeler, erişim paketini ve ayrıca onay ve süre sonu ayarlarını kimlerin isteyebilen belirler. Yeni bir erişim paketi oluşturduğunuzda, dizininizde kullanıcılar için, dizininizde bulunmayan kullanıcılar için bir başlangıç ilkesi oluşturabilir, yalnızca yönetici doğrudan atamaları için veya ilkeyi daha sonra oluşturmayı tercih edebilirsiniz.

Aşağıdaki diyagramda yeni bir erişim paketi oluşturmak için üst düzey işlem gösterilmektedir.

![Erişim paketi işlemi oluşturma](./media/entitlement-management-access-package-create/access-package-process.png)

## <a name="start-new-access-package"></a>Yeni erişim paketini Başlat

**Önkoşul rolü:** Kullanıcı Yöneticisi veya katalog sahibi

1. [Azure Portal](https://portal.azure.com) oturum açın.

1. **Azure Active Directory** ' a ve ardından **kimlik**Yönetimi ' ne tıklayın.

1. Sol menüde, **erişim paketleri**' ne tıklayın.

    ![Azure portal Yetkilendirme Yönetimi](./media/entitlement-management-shared/elm-access-packages.png)

1. **Yeni erişim paketi**' ne tıklayın.

## <a name="basics"></a>Temel

**Temel bilgiler** sekmesinde, erişim paketine bir ad verirsiniz ve erişim paketinin hangi katalogda oluşturulacağını belirtebilirsiniz.

1. Erişim paketi için bir görünen ad ve açıklama girin. Kullanıcılar, erişim paketi için bir istek gönderdiğinde bu bilgileri görür.

1. **Katalog** açılan listesinde, erişim paketini oluşturmak istediğiniz kataloğu seçin. Örneğin, istenmiş olan tüm pazarlama kaynaklarını yöneten bir katalog sahibi olabilirsiniz. Bu durumda, pazarlama kataloğunu seçebilirsiniz.

    Yalnızca ' de erişim paketleri oluşturma izniniz olan kataloglar görüntülenir. Mevcut bir katalogda erişim paketi oluşturmak için, en az bir Kullanıcı Yöneticisi, Katalog sahibi veya paket Yöneticisi erişim gerekir.

    ![Erişim paketi-temel bilgiler](./media/entitlement-management-access-package-create/basics.png)

    Erişim paketinizi yeni bir katalogda oluşturmak istiyorsanız **Yeni oluştur**' a tıklayın. Katalog adı ve açıklamasını girip **Oluştur**' a tıklayın.

    Oluşturmakta olduğunuz erişim paketi ve kendisine dahil edilen kaynaklar yeni kataloğa eklenecektir. Ayrıca, kataloğun ilk sahibi de otomatik olarak olacaktır. Ek katalog sahipleri ekleyebilirsiniz.

    Yeni bir katalog oluşturmak için en az bir Kullanıcı Yöneticisi veya katalog Oluşturucu olmalıdır.

1.           **İleri**'ye tıklayın.

## <a name="resource-roles"></a>Kaynak rolleri

**Kaynak rolleri** sekmesinde, erişim paketine dahil edilecek kaynakları seçersiniz.

1. Eklemek istediğiniz kaynak türüne (**gruplar**, **uygulamalar**veya **SharePoint siteleri**) tıklayın.

1. Görüntülenen seçim bölmesinde listeden bir veya daha fazla kaynak seçin.

    ![Paket-kaynak rollerine erişme](./media/entitlement-management-access-package-create/resource-roles.png)

    Erişim paketini genel katalogda veya yeni bir katalogda oluşturuyorsanız, sahip olduğunuz dizinden herhangi bir kaynak seçebilirsiniz. En az bir Kullanıcı Yöneticisi veya katalog Oluşturucu olmalıdır.

    Erişim paketini mevcut bir katalogda oluşturuyorsanız, zaten katalogda olan herhangi bir kaynağı sahip olmadan seçebilirsiniz.

    Kullanıcı yöneticisiyseniz veya katalog sahibiyseniz, sahip olduğunuz kaynakları seçme konusunda henüz katalogda olmayan ek bir seçeneğe sahip olursunuz. Seçili katalogda mevcut olmayan kaynakları seçerseniz, bu kaynaklar diğer katalog yöneticilerinin ile erişim paketleri oluşturması için kataloğa da eklenecektir. Yalnızca seçili katalogda olan kaynakları seçmek istiyorsanız, kaydırma kutusunun en üstündeki **yalnızca göster** onay kutusunu işaretleyin.

1. Kaynakları seçtikten sonra, **rol** listesinde, kullanıcıların kaynak için atanmasını istediğiniz rolü seçin.

    ![Paket-kaynak rolü seçimine erişin](./media/entitlement-management-access-package-create/resource-roles-role.png)

1.           **İleri**'ye tıklayın.

## <a name="policy"></a>İlke

**İlke** sekmesinde, erişim paketini ve ayrıca onay ve süre sonu ayarlarını kimin isteyebilen belirlemek için ilk ilkeyi oluşturursunuz. Daha sonra, ek kullanıcı gruplarının kendi onay ve sona erme ayarlarına sahip erişim paketini istemesine izin vermek için daha fazla ilke oluşturabilirsiniz. İlkeyi daha sonra oluşturmayı da tercih edebilirsiniz.

1. **İlk Ilke oluştur** ' a **Şimdi** veya **daha sonra**geç ' i ayarlayın.

    ![Erişim paketi-Ilke](./media/entitlement-management-access-package-create/policy.png)

1. **Daha sonra**' yi seçerseniz, erişim paketinizi oluşturmak Için [gözden geçir + oluştur](#review--create) bölümüne atlayın.

1. **Şimdi**' yi seçerseniz, aşağıdaki ilke bölümlerinden birindeki adımları gerçekleştirin.

[!INCLUDE [Entitlement management policy](../../../includes/active-directory-entitlement-management-policy.md)]

## <a name="review--create"></a>Gözden geçirme + oluşturma

**Gözden geçir + oluştur** sekmelerinde, ayarlarınızı gözden geçirebilir ve herhangi bir doğrulama hatası olup olmadığını denetleyebilirsiniz.

1. Erişim paketinin ayarlarını gözden geçirin

    ![Erişim paketi-Ilke-ilke ayarını etkinleştir](./media/entitlement-management-access-package-create/review-create.png)

1. Erişim paketini oluşturmak için **Oluştur** ' a tıklayın.

    Yeni erişim paketi, erişim paketleri listesinde görünür.

## <a name="next-steps"></a>Sonraki adımlar

- [Var olan bir erişim paketini düzenleme ve yönetme](entitlement-management-access-package-edit.md)
- [Katalog sahibi veya erişim paketi Yöneticisi ekleme](entitlement-management-delegate.md#add-a-catalog-owner-or-an-access-package-manager)
- [Katalog oluşturma ve yönetme](entitlement-management-catalog-create.md)
