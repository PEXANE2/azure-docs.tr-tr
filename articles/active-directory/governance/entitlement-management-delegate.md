---
title: Azure AD Yetkilendirme Yönetimi 'nde (Önizleme) görevleri devretmek-Azure Active Directory
description: Azure Active Directory yetkilendirme yönetiminde görevlere temsilci atamak için atayabileceğiniz roller hakkında bilgi edinin.
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
ms.date: 07/10/2019
ms.author: ajburnle
ms.reviewer: mwahl
ms.collection: M365-identity-device-management
ms.openlocfilehash: 4430e2115e4282ba7bb618184139a845547a06ed
ms.sourcegitcommit: 5d6c8231eba03b78277328619b027d6852d57520
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/13/2019
ms.locfileid: "68967086"
---
# <a name="delegate-tasks-in-azure-ad-entitlement-management-preview"></a>Azure AD Yetkilendirme Yönetimi 'nde görevleri temsilci seçme (Önizleme)

> [!IMPORTANT]
> Azure Active Directory (Azure AD) yetkilendirme yönetimi şu anda genel önizleme aşamasındadır.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir.
> Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Varsayılan olarak, genel Yöneticiler ve Kullanıcı yöneticileri Azure AD yetkilendirme yönetiminin tüm yönlerini oluşturabilir ve yönetebilir. Ancak, bu rollerdeki kullanıcılar erişim paketlerinin gerekli olduğu tüm senaryoları bilmiyor olabilir. Genellikle, kimin işbirliği yapmaları gerektiğini bilen departmanlar içindeki kullanıcılardır.

Yönetici olmayanlar için kısıtlanmamış izinler vermek yerine kullanıcılara, işlerini gerçekleştirmek için ihtiyaç duydukları en düşük izinleri verebilir ve çakışan veya uygunsuz erişim hakları oluşturmaktan kaçınabilirsiniz. Bu makalede, yetkilendirme yönetiminde çeşitli görevleri temsilci olarak atamak için atayabileceğiniz roller açıklanmaktadır.

## <a name="delegate-example-for-departmental-adoption"></a>Bölüm benimseme için temsilci örneği

Yetkilendirme yönetiminde görevlerin nasıl temsilciliğini anlayabileceğinizi anlamak için, bir örneği göz önünde bulundurmasına yardımcı olur. Kuruluşunuzun aşağıdaki beş Kullanıcı olduğunu varsayalım:

| Kullanıcı | Bölüm | Notlar |
| --- | --- | --- |
| Alice | IT | Genel yönetici |
| Bob | Araştırma | Bob Ayrıca bir araştırma grubunun sahibidir |
| Carole | Araştırma |  |
| Dave | Pazarlama |  |
| Elisa | Pazarlama | Elisa Ayrıca bir pazarlama uygulamasının sahibidir |

Hem araştırma hem de pazarlama departmanları, kullanıcıları için yetkilendirme yönetimini kullanmak ister. Çiğdem, diğer bölümlerin yetkilendirme yönetimini kullanması için henüz kullanıma hazırlanmamıştır. İşte, Çiğdem 'in araştırma ve pazarlama departmanlarına görevler temsilciliğini sağlayan bir yoldur.

1. Çiğdem, Katalog oluşturucuları için yeni bir Azure AD güvenlik grubu oluşturur ve bu grubun üyeleri olarak Bob, Carol, Davve ve Elisa 'yi ekler.

1. Gamze, bu grubu Katalog oluşturucuları rolüne eklemek için yetkilendirme yönetimi ayarlarını kullanır.

1. Carol, bir **araştırma** Kataloğu oluşturuyor ve emre 'yi Bu Kataloğun ortak sahibi olarak ekliyor. Bob, kataloğa ait araştırma grubunu bir kaynak olarak ekler, böylece araştırma işbirliği için bir erişim paketinde kullanılabilir.

1. Pave, bir **Pazarlama** kataloğu oluşturur ve bu kataloğun ortak sahibi olarak Elisa 'yı ekler. Elisa, kataloğa sahip olduğu pazarlama uygulamasını bir kaynak olarak ekler, böylece pazarlama işbirliği için bir erişim paketinde kullanılabilir.

Artık araştırma ve pazarlama departmanları yetkilendirme yönetimini kullanabilir. Bob, Carol, Davve ve Elisa, ilgili kataloglarında erişim paketleri oluşturabilir ve yönetebilir.

![Yetkilendirme Yönetimi temsilcisi örneği](./media/entitlement-management-delegate/elm-delegate.png)

## <a name="entitlement-management-roles"></a>Yetkilendirme Yönetimi rolleri

Yetkilendirme yönetiminin, yetkilendirme yönetimine özgü aşağıdaki rolleri vardır.

| Role | Açıklama |
| --- | --- |
| Katalog Oluşturucu | Kataloglar oluşturun ve yönetin. Genellikle genel yönetici olmayan bir BT Yöneticisi veya bir kaynak koleksiyonu için kaynak sahibi. Katalog oluşturan kişi otomatik olarak kataloğun ilk katalog sahibi olur ve ek Katalog sahipleri ekleyebilir. |
| Katalog sahibi | Mevcut katalogları düzenleyin ve yönetin. Genellikle bir BT Yöneticisi veya kaynak sahipleri veya kataloğun sahibi tarafından atanan bir kullanıcı. |
| Paket Yöneticisi 'ne erişim | Bir kataloğun içindeki tüm mevcut erişim paketlerini düzenleyin ve yönetin. |

Ayrıca, atanan bir onaylayan ve bir erişim paketinin istek sahibi de haklara sahiptir, ancak bu roller rol değildir.
 
* Kişi Erişim paketi tanımlarını değiştiremese de, paketlere erişim isteklerini onaylamak veya reddetmek için bir ilke tarafından yetkilendirilmiştir.
* Sahibinin Erişim paketi istemek için bir erişim paketi ilkesi tarafından yetkilendirilir.

Aşağıdaki tabloda, bu rollerin gerçekleştirebileceği görevler listelenmiştir.

| Görev | Katalog Oluşturucu | Katalog sahibi | Paket Yöneticisi 'ne erişim | Onaylayan |
| --- | :---: | :---: | :---: | :---: |
| [Yeni Katalog oluşturma](entitlement-management-catalog-create.md) | :heavy_check_mark: |  |  |  |
| [Kataloğa kaynak ekleme](entitlement-management-catalog-create.md#add-resources-to-a-catalog) | | :heavy_check_mark: | | |
| [Katalog düzenleme](entitlement-management-catalog-create.md#edit-a-catalog) |  | :heavy_check_mark: |  |  |
| [Katalog silme](entitlement-management-catalog-create.md#delete-a-catalog) |  | :heavy_check_mark: |  |  |
| [Kataloğa bir katalog sahibi veya erişim paketi Yöneticisi ekleme](#add-a-catalog-owner-or-an-access-package-manager) |  | :heavy_check_mark: |  |  |
| [Bir katalogda yeni bir erişim paketi oluşturma](entitlement-management-access-package-create.md) |  | :heavy_check_mark:  | :heavy_check_mark:  |  |
| [Erişim paketindeki kaynak rollerini yönetme](entitlement-management-access-package-edit.md) |  | :heavy_check_mark: | :heavy_check_mark: |  |
| [İlke oluşturma ve düzenleme](entitlement-management-access-package-edit.md#add-a-new-policy) |  | :heavy_check_mark: | :heavy_check_mark: |  |
| [Bir kullanıcıyı doğrudan bir erişim paketine atama](entitlement-management-access-package-edit.md#directly-assign-a-user) |  | :heavy_check_mark: | :heavy_check_mark: |  |
| [Erişim paketine kimin atanmasına sahip olduğunu görüntüleme](entitlement-management-access-package-edit.md#view-who-has-an-assignment) |  | :heavy_check_mark: | :heavy_check_mark: |  |
| [Bir erişim paketinin isteklerini görüntüleme](entitlement-management-access-package-edit.md#view-requests) |  | :heavy_check_mark: | :heavy_check_mark: |  |
| [Bir isteğin teslim hatalarını görüntüleme](entitlement-management-access-package-edit.md#view-a-requests-delivery-errors) |  | :heavy_check_mark: | :heavy_check_mark: |  |
| [Bekleyen bir isteği iptal etme](entitlement-management-access-package-edit.md#cancel-a-pending-request) |  | :heavy_check_mark: | :heavy_check_mark: |  |
| [Erişim paketini gizle](entitlement-management-access-package-edit.md#change-the-hidden-setting) |  | :heavy_check_mark: | :heavy_check_mark: |  |
| [Erişim paketini silme](entitlement-management-access-package-edit.md#delete) |  | :heavy_check_mark: | :heavy_check_mark: |  |
| [Erişim isteğini onaylama](entitlement-management-request-approve.md) |  |  |  | :heavy_check_mark: |

## <a name="required-roles-to-add-resources-to-a-catalog"></a>Kataloğa kaynak eklemek için gerekli roller

Genel yönetici, bir katalogda herhangi bir grup (bulutta oluşturulan güvenlik grupları veya bulut tarafından oluşturulan Office 365 grupları), uygulama veya SharePoint Online sitesi ekleyebilir veya kaldırabilir. Bir Kullanıcı Yöneticisi, bir katalogda grup veya uygulama ekleyebilir veya kaldırabilir.

Genel yönetici veya Kullanıcı Yöneticisi olmayan bir kullanıcı için bir kataloğa gruplar, uygulamalar veya SharePoint Online siteleri eklemek üzere, bu kullanıcının hem gerekli Azure AD dizin rolüne hem *de* Katalog sahibi yetkilendirme yönetimi rolüne sahip olması gerekir. Aşağıdaki tabloda, bir kataloğa kaynak eklemek için gereken rol birleşimleri listelenmektedir. Bir katalogdan kaynakları kaldırmak için aynı rollere sahip olmanız gerekir.

| Azure AD dizin rolü | Yetkilendirme Yönetimi rolü | Güvenlik grubu eklenebilir | Office 365 grubu eklenebilir | Uygulama eklenebilir | SharePoint Online sitesi eklenebilir |
| --- | :---: | :---: | :---: | :---: | :---: |
| [Genel yönetici](../users-groups-roles/directory-assign-admin-roles.md) | yok |  :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| [Kullanıcı Yöneticisi](../users-groups-roles/directory-assign-admin-roles.md) | yok |  :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |  |
| [Intune Yöneticisi](../users-groups-roles/directory-assign-admin-roles.md) | Katalog sahibi | :heavy_check_mark: | :heavy_check_mark: |  |  |
| [Exchange Yöneticisi](../users-groups-roles/directory-assign-admin-roles.md) | Katalog sahibi |  | :heavy_check_mark: |  |  |
| [Takımlar Hizmet Yöneticisi](../users-groups-roles/directory-assign-admin-roles.md) | Katalog sahibi |  | :heavy_check_mark: |  |  |
| [SharePoint Yöneticisi](../users-groups-roles/directory-assign-admin-roles.md) | Katalog sahibi |  | :heavy_check_mark: |  | :heavy_check_mark: |
| [Uygulama Yöneticisi](../users-groups-roles/directory-assign-admin-roles.md) | Katalog sahibi |  |  | :heavy_check_mark: |  |
| [Bulut uygulaması Yöneticisi](../users-groups-roles/directory-assign-admin-roles.md) | Katalog sahibi |  |  | :heavy_check_mark: |  |
| Kullanıcı | Katalog sahibi | Yalnızca Grup sahibi ise | Yalnızca Grup sahibi ise | Yalnızca uygulama sahibi ise |  |

## <a name="add-a-catalog-creator"></a>Katalog Oluşturucu Ekleme

Katalog oluşturma 'yı atamak istiyorsanız, kullanıcıları Katalog Oluşturucu rolüne eklersiniz.  Bireysel kullanıcılar ekleyebilir veya kolaylık sağlaması için Üyeler daha sonra kataloglar oluşturabilebilen bir grup ekleyebilirsiniz. Bir kullanıcıyı Katalog Oluşturucu rolüne atamak için aşağıdaki adımları izleyin.

**Önkoşul rolü:** Genel yönetici veya Kullanıcı Yöneticisi

1. Azure portal, **Azure Active Directory** ' a ve ardından **kimlik**Yönetimi ' ne tıklayın.

1. Sol taraftaki menüde, **Yetkilendirme Yönetimi** bölümünde, **Ayarlar**' a tıklayın.

1.           **Düzenle**‘ye tıklayın.

1. **Temsilci yetkilendirme yönetimi** bölümünde, bu yetkilendirme yönetimi rolünün üyeleri olacak kullanıcıları veya grupları seçmek için **Katalog oluşturucuları Ekle** ' ye tıklayın.

1. Tıklayın **seçin**.

1. **Kaydet**’e tıklayın.

## <a name="add-a-catalog-owner-or-an-access-package-manager"></a>Katalog sahibi veya erişim paketi Yöneticisi ekleme

Katalogdaki bir kataloğun veya paketlerin yönetimine temsilci atamak istiyorsanız, kullanıcıları Katalog sahibine ekler veya paket yöneticisi rollerine erişin. Herhangi bir katalog oluşturduğunda ilk katalog sahibi olur. Bir kullanıcıyı Katalog sahibine atamak veya paket yöneticisi rolüne erişmek için bu adımları izleyin.

**Önkoşul rolü:** Genel yönetici, Kullanıcı Yöneticisi veya katalog sahibi

1. Azure portal, **Azure Active Directory** ' a ve ardından **kimlik**Yönetimi ' ne tıklayın.

1. Sol taraftaki menüden **kataloglar** ' a tıklayın ve ardından Yöneticiler eklemek istediğiniz kataloğu açın.

1. Sol taraftaki menüden **Roller ve yöneticiler**' e tıklayın.

1. Bu rollerin üyelerini seçmek için **sahip Ekle** veya **erişim paketi yöneticileri Ekle** ' ye tıklayın.

1. Bu üyeleri eklemek için **Seç** ' e tıklayın.

## <a name="next-steps"></a>Sonraki adımlar

- [Onaylayanlar Ekle](entitlement-management-access-package-edit.md#policy-request)
- [Kataloğa kaynak ekleme](entitlement-management-catalog-create.md#add-resources-to-a-catalog)
