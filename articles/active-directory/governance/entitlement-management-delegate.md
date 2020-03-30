---
title: Yetkilendirme yönetiminde delegasyon ve roller - Azure AD
description: Bt yöneticilerinden erişim yönetimini departman yöneticilerine ve proje yöneticilerine nasıl devredebileceğinizi öğrenin, böylece erişim kendileri yönetilebilir.
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
ms.date: 10/07/2019
ms.author: ajburnle
ms.reviewer: mwahl
ms.collection: M365-identity-device-management
ms.openlocfilehash: 86d924860e97b15a0a4af46c5bc35b0e0050292b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79261846"
---
# <a name="delegation-and-roles-in-azure-ad-entitlement-management"></a>Azure AD yetkilendirme yönetiminde delegasyon ve roller

Varsayılan olarak, Global yöneticiler ve Kullanıcı yöneticileri Azure AD yetkilendirme yönetiminin tüm yönlerini oluşturabilir ve yönetebilir. Ancak, bu rollerdeki kullanıcılar erişim paketlerinin gerekli olduğu tüm durumları bilmiyor olabilir. Genellikle, ilgili departmanlar, ekipler veya projeler de dahil olmak üzere, kiminle işbirliği yaptıklarını, hangi kaynakları ve ne kadar süreyle işbirliği yaptıklarını bilen kullanıcılardır. Yönetici olmayanlara sınırsız izin vermek yerine, kullanıcılara işlerini gerçekleştirmeleri ve çakışan veya uygunsuz erişim hakları oluşturmaktan kaçınmaları için gereken en az izinleri verebilirsiniz.

Bu video, bt yöneticisinden yönetici olmayan kullanıcılara erişim yönetimini nasıl devratılayalaladığına genel bir bakış sağlar.

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE3Lq00]

## <a name="delegate-example"></a>Temsilci örneği

Yetkilendirme yönetiminde erişim yönetimini nasıl devredebileceğinizi anlamak için, bir örnek dikkate alınmasına yardımcı olur. Kuruluşunuzun aşağıdaki yönetici ve yöneticilere sahip olduğunu varsayalım.

![BT yöneticisinden yöneticilere temsilci](./media/entitlement-management-delegate/delegate-admin-dept-managers.png)

BT yöneticisi olarak Hana'nın her departmanda bağlantıları vardır - Pazarlama'da Mamta, Finans'ta Mark ve Hukuk'ta Joe, departmanlarının kaynaklarından ve iş açısından kritik içeriğinden sorumludur.

Yetkilendirme yönetimi ile, hangi kullanıcıların erişime, ne kadar süreyle ve hangi kaynaklara erişebileceğini bilen kişiler oldukları için, erişim yönetimini bu yönetici olmayanlara devredebilirsiniz. Bu, doğru kişilerin departmanları için erişimi yönetmesini sağlar.

Hana'nın pazarlama, finans ve hukuk departmanlarına erişim yönetimini devretmesinin bir yolu da burada.

1. Hana yeni bir Azure AD güvenlik grubu oluşturur ve grubun üyeleri olarak Mamta, Mark ve Joe'yu ekler.

1. Hana bu grubu katalog oluşturucular rolüne ekler.

    Mamta, Mark ve Joe artık departmanları için kataloglar oluşturabilir, departmanlarının ihtiyaç duyduğu kaynakları ekleyebilir ve katalog içinde daha fazla delegasyon yapabilir.

    Mamta, Mark ve Joe'nun birbirlerinin kataloglarını göremediğini unutmayın.

1. Mamta, bir kaynak kapsayıcısı olan bir **Pazarlama** kataloğu oluşturur.

1. Mamta, pazarlama departmanının sahip olduğu kaynakları bu kataloğa ekliyor.

1. Mamta bu katalog için katalog sahipleri olarak kendi bölümünden ek insanlar ekleyebilirsiniz. Bu, katalog yönetimi sorumluluklarının paylaşılabına yardımcı olur.

1. Mamta, Pazarlama kataloğundaki erişim paketlerinin oluşturulmasını ve yönetimini Pazarlama departmanındaki proje yöneticilerine devredebilir. Bunu, bunları erişim paketi yöneticisi rolüne atayarak yapabilir. Bir erişim paketi yöneticisi erişim paketleri oluşturabilir ve yönetebilir. 

Aşağıdaki diyagram, pazarlama, finans ve hukuk departmanları için kaynaklara sahip katalogları gösterir. Proje yöneticileri bu katalogları kullanarak ekipleri veya projeleri için erişim paketleri oluşturabilir.

![Yetkilendirme yönetimi temsilci örneği](./media/entitlement-management-delegate/elm-delegate.png)

Delegasyondan sonra, pazarlama departmanının aşağıdaki tabloya benzer rolleri olabilir.

| Kullanıcı | İş rolü | Azure AD rolü | Yetkilendirme yönetimi rolü |
| --- | --- | --- | --- |
| Hana | BT yöneticisi | Genel yönetici veya Kullanıcı yöneticisi |  |
| Mamta | Pazarlama müdürü | Kullanıcı | Katalog oluşturucusu ve Katalog sahibi |
| Bob | Pazarlama lideri | Kullanıcı | Katalog sahibi |
| Jessica | Pazarlama proje yöneticisi | Kullanıcı | Erişim paketi yöneticisi |

## <a name="entitlement-management-roles"></a>Yetkilendirme yönetimi rolleri

Yetkilendirme yönetimi, yetkilendirme yönetimine özgü aşağıdaki rollere sahiptir.

| Yetkilendirme yönetimi rolü | Açıklama |
| --- | --- |
| Katalog oluşturucusu | Kataloglar oluşturun ve yönetin. Genellikle, Genel Yönetici olmayan bir BT yöneticisi veya kaynak koleksiyonu için kaynak sahibi. Katalog oluşturan kişi otomatik olarak kataloğun ilk kataloğu sahibi olur ve ek katalog sahipleri ekleyebilir. Katalog oluşturucusu, sahip olmadıkları ve sahip olmadıkları kaynakları bir kataloğa ekleyemeyen katalogları yönetemez veya göremez. Katalog oluşturucunun başka bir kataloğu yönetmesi veya sahip olmadıkları kaynakları eklemesi gerekiyorsa, bu kataloğun veya kaynağın ortak sahibi olmayı isteyebilir. |
| Katalog sahibi | Varolan katalogları edin ve yönetin. Genellikle bir BT yöneticisi veya kaynak sahipleri veya katalog sahibinin belirlediği bir kullanıcı. |
| Erişim paketi yöneticisi | Katalogdaki tüm mevcut erişim paketlerini edin ve yönetin. |

Buna ek olarak, atanmış bir onaylayıcı ve bir erişim paketinin isteği de rolleri olmasa da, haklara sahiptir.

| Sağ | Açıklama |
| --- | --- |
| Onaylayan | Erişim paketi tanımlarını değiştiremeseler de, paketlere erişim isteklerini onaylamak veya reddetmek için bir ilke tarafından yetkilendirilen. |
| Requestor | Bu erişim paketini istemek için bir erişim paketinin ilkesi tarafından yetkilendirilen. |

Aşağıdaki tabloda, yetkilendirme yönetimi rollerinin gerçekleştirebileceği görevler listelenir.

| Görev | Yönetici | Katalog oluşturucusu | Katalog sahibi | Erişim paketi yöneticisi |
| --- | :---: | :---: | :---: | :---: |
| [Katalog oluşturucuya temsilci](entitlement-management-delegate-catalog.md) | :heavy_check_mark: |  |  |  |
| [Bağlı kuruluş ekleme](entitlement-management-organization.md) | :heavy_check_mark: |  |  |  |
| [Yeni bir katalog oluşturma](entitlement-management-catalog-create.md) | :heavy_check_mark: | :heavy_check_mark: |  |  |
| [Kataloğuna kaynak ekleme](entitlement-management-catalog-create.md#add-resources-to-a-catalog) | :heavy_check_mark: |  | :heavy_check_mark: |  |
| [Katalog sahibi ekleme](entitlement-management-catalog-create.md#add-additional-catalog-owners) | :heavy_check_mark: |  | :heavy_check_mark: |  |
| [Kataloğu edin](entitlement-management-catalog-create.md#edit-a-catalog) | :heavy_check_mark: |  | :heavy_check_mark: |  |
| [Kataloğu silme](entitlement-management-catalog-create.md#delete-a-catalog) | :heavy_check_mark: |  | :heavy_check_mark: |  |
| [Erişim paketi yöneticisine temsilci](entitlement-management-delegate-managers.md) | :heavy_check_mark: |  | :heavy_check_mark: |  |
| [Erişim paketi yöneticisini kaldırma](entitlement-management-delegate-managers.md#remove-an-access-package-manager) | :heavy_check_mark: |  | :heavy_check_mark: |  |
| [Katalogda yeni bir erişim paketi oluşturma](entitlement-management-access-package-create.md) | :heavy_check_mark: |  | :heavy_check_mark:  | :heavy_check_mark:  |
| [Erişim paketindeki kaynak rollerini değiştirme](entitlement-management-access-package-resources.md) | :heavy_check_mark: |  | :heavy_check_mark: | :heavy_check_mark: |
| [İlkeler oluşturma ve bu ilkeleri yeniden oluşturma](entitlement-management-access-package-request-policy.md) | :heavy_check_mark: |  | :heavy_check_mark: | :heavy_check_mark: |
| [Bir kullanıcıyı doğrudan bir erişim paketine atama](entitlement-management-access-package-assignments.md#directly-assign-a-user) | :heavy_check_mark: |  | :heavy_check_mark: | :heavy_check_mark: |
| [Erişim paketine kimin ataması olduğunu görüntüleme](entitlement-management-access-package-assignments.md#view-who-has-an-assignment) | :heavy_check_mark: |  | :heavy_check_mark: | :heavy_check_mark: |
| [Erişim paketinin isteklerini görüntüleme](entitlement-management-access-package-requests.md#view-requests) | :heavy_check_mark: |  | :heavy_check_mark: | :heavy_check_mark: |
| [İsteğin teslim hatalarını görüntüleme](entitlement-management-troubleshoot.md#view-a-requests-delivery-errors) | :heavy_check_mark: |  | :heavy_check_mark: | :heavy_check_mark: |
| [İsteği yeniden işleme](entitlement-management-troubleshoot.md#reprocess-a-request) | :heavy_check_mark: |  | :heavy_check_mark: | :heavy_check_mark: |
| [Bekleyen isteği iptal etme](entitlement-management-troubleshoot.md#cancel-a-pending-request) | :heavy_check_mark: |  | :heavy_check_mark: | :heavy_check_mark: |
| [Erişim paketini gizleme](entitlement-management-access-package-edit.md#change-the-hidden-setting) | :heavy_check_mark: |  | :heavy_check_mark: | :heavy_check_mark: |
| [Erişim paketini silme](entitlement-management-access-package-edit.md#delete-an-access-package) | :heavy_check_mark: |  | :heavy_check_mark: | :heavy_check_mark: |

## <a name="required-roles-to-add-resources-to-a-catalog"></a>Bir kataloğa kaynak eklemek için gerekli roller

Global yönetici, bir katalobuna herhangi bir grup (bulut tarafından oluşturulan güvenlik grupları veya bulut tarafından oluşturulan Office 365 Grupları), uygulama veya SharePoint Online sitesi ekleyebilir veya kaldırabilir. Kullanıcı yöneticisi bir katalogdaki herhangi bir grup veya uygulamayı ekleyebilir veya kaldırabilir.

Global yönetici veya Kullanıcı yöneticisi olmayan bir kullanıcının bir kataloğa gruplar, uygulamalar veya SharePoint Online siteleri eklemesi için, bu kullanıcının hem gerekli Azure REKLAM dizin *rolüne hem de* katalog sahibi yetkilendirme yönetimi rolüne sahip olması gerekir. Aşağıdaki tabloda, bir kataloğa kaynak eklemek için gereken rol birleşimleri listelenir. Kaynakları katalogdan kaldırmak için aynı rollere sahip olmalısınız.

| Azure AD dizin rolü | Yetkilendirme yönetimi rolü | Güvenlik grubu ekleyebilir | Office 365 Grubu ekleyebilir | Uygulama ekleyebilir | SharePoint Online sitesi ekleyebilir |
| --- | :---: | :---: | :---: | :---: | :---: |
| [Genel yönetici](../users-groups-roles/directory-assign-admin-roles.md) | yok |  :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| [Kullanıcı yöneticisi](../users-groups-roles/directory-assign-admin-roles.md) | yok |  :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |  |
| [Intune yöneticisi](../users-groups-roles/directory-assign-admin-roles.md) | Katalog sahibi | :heavy_check_mark: | :heavy_check_mark: |  |  |
| [Exchange yöneticisi](../users-groups-roles/directory-assign-admin-roles.md) | Katalog sahibi |  | :heavy_check_mark: |  |  |
| [Takımlar servis yöneticisi](../users-groups-roles/directory-assign-admin-roles.md) | Katalog sahibi |  | :heavy_check_mark: |  |  |
| [SharePoint yöneticisi](../users-groups-roles/directory-assign-admin-roles.md) | Katalog sahibi |  | :heavy_check_mark: |  | :heavy_check_mark: |
| [Uygulama yöneticisi](../users-groups-roles/directory-assign-admin-roles.md) | Katalog sahibi |  |  | :heavy_check_mark: |  |
| [Bulut uygulama yöneticisi](../users-groups-roles/directory-assign-admin-roles.md) | Katalog sahibi |  |  | :heavy_check_mark: |  |
| Kullanıcı | Katalog sahibi | Yalnızca grup sahibi | Yalnızca grup sahibi | Yalnızca uygulama sahibi |  |

Bir görev için en az ayrıcalıklı rolü belirlemek için, [Azure Etkin Dizini'nde yönetici görevine göre Yönetici rollerine](../users-groups-roles/roles-delegate-by-task.md#entitlement-management)de başvuruda bulunabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

- [Katalog oluşturucularına erişim yönetimini temsilci olarak](entitlement-management-delegate-catalog.md)
- [Kaynak kataloğu oluşturma ve yönetme](entitlement-management-catalog-create.md)
