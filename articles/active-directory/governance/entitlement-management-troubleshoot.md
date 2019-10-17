---
title: Azure AD yetkilendirme yönetimi sorunlarını giderme (Önizleme)-Azure Active Directory
description: Azure Active Directory yetkilendirme yönetimi (Önizleme) sorunlarını gidermenize yardımcı olması için denetlemeniz gereken bazı öğeler hakkında bilgi edinin.
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
ms.date: 05/30/2019
ms.author: ajburnle
ms.reviewer: markwahl-msft
ms.collection: M365-identity-device-management
ms.openlocfilehash: ea979731c27a8d332102c3215e80510994f2ab3f
ms.sourcegitcommit: 77bfc067c8cdc856f0ee4bfde9f84437c73a6141
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/16/2019
ms.locfileid: "72430227"
---
# <a name="troubleshoot-azure-ad-entitlement-management-preview"></a>Azure AD yetkilendirme yönetimi sorunlarını giderme (Önizleme)

> [!IMPORTANT]
> Azure Active Directory (Azure AD) yetkilendirme yönetimi şu anda genel önizleme aşamasındadır.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir.
> Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Bu makalede, Azure Active Directory (Azure AD) yetkilendirme yönetimi sorunlarını gidermenize yardımcı olması için denetlemeniz gereken bazı öğeler açıklanmaktadır.

## <a name="checklist-for-entitlement-management-administration"></a>Yetkilendirme Yönetimi yönetimi için denetim listesi

* Yetkilendirme yönetimini yapılandırırken bir erişim reddedildi iletisi alırsanız ve genel yöneticiyseniz, dizininizde [Azure AD Premium P2 (veya EMS E5) lisansına](entitlement-management-overview.md#license-requirements)sahip olduğundan emin olun.  
* Erişim paketleri oluştururken veya görüntülerken erişim engellendi iletisi alırsanız ve bir katalog Oluşturucu grubunun üyesiyseniz, ilk erişim paketinizi oluşturmadan önce bir katalog oluşturmanız gerekir.

## <a name="checklist-for-adding-a-resource"></a>Kaynak ekleme denetim listesi

* Bir uygulamanın bir erişim paketindeki kaynak olması için, atanabilecek en az bir kaynak rolü olmalıdır. Roller, uygulamanın kendisi tarafından tanımlanır ve Azure AD 'de yönetilir. Azure portal Ayrıca, uygulamalar olarak seçilebilme Hizmetleri için hizmet sorumlularını da gösterebileceğini unutmayın.  Özellikle, **Exchange Online** ve **SharePoint Online** , dizinde kaynak rollerine sahip olan uygulamalar değil, bir erişim paketine dahil edilemez.  Bunun yerine, bu hizmetlere erişmesi gereken bir kullanıcıya uygun bir lisans oluşturmak için grup tabanlı lisanslama kullanın.

* Bir grubun bir erişim paketindeki kaynak olması için Azure AD 'de değiştirilebilir olması gerekir.  Şirket içi Active Directory kaynaklı gruplar, sahip veya üye öznitelikleri Azure AD 'de değiştirilemediğinden kaynak olarak atanamaz.   Exchange Online 'da dağıtım grupları olarak gelen gruplar, Azure AD 'de değiştirilemez. 

* SharePoint Online belge kitaplıkları ve bireysel belgeler, kaynak olarak eklenemez.  Bunun yerine, bir Azure AD güvenlik grubu oluşturun, bu grubu ve bir site rolünü erişim paketine ekleyin ve SharePoint Online 'da belge kitaplığına veya belgeye erişimi denetlemek için bu grubu kullanın.

* Bir erişim paketiyle yönetmek istediğiniz bir kaynağa zaten atanmış kullanıcılar varsa, kullanıcıların erişim paketine uygun bir ilkeyle atandığından emin olun. Örneğin, grupta zaten kullanıcıları olan bir erişim paketine bir grup eklemek isteyebilirsiniz. Gruptaki bu kullanıcılar devam eden erişim gerektiriyorsa, gruba erişimleri kaybetmemesi için erişim paketleri için uygun bir ilkeye sahip olmaları gerekir. Erişim paketini, kullanıcıların söz konusu kaynağı içeren erişim paketini istemesini isteyerek veya doğrudan erişim paketine atayarak atayabilirsiniz. Daha fazla bilgi için bkz. [bir erişim paketi Için değişiklik isteği ve onay ayarları](entitlement-management-access-package-request-policy.md).

## <a name="checklist-for-providing-external-users-access"></a>Dış kullanıcılara erişim sağlama denetim listesi

* Bir B2B [izin verilenler listesi](../b2b/allow-deny-list.md)varsa, dizinlere izin verilmeyen kullanıcılara erişim isteyemeyecektir.

* Dış kullanıcıların erişim isteğinde bulunmasını engelleyecek veya erişim paketlerindeki uygulamaları kullanabilmesi için [koşullu erişim ilkelerinin](../conditional-access/require-managed-devices.md) bulunmadığından emin olun.

## <a name="checklist-for-request-issues"></a>İstek sorunları için denetim listesi

* Bir Kullanıcı bir erişim paketine erişim istemek istediğinde, erişim paketi için erişim **portalı** ' nı kullandıklarından emin olun. Daha fazla bilgi için bkz. [erişim paketi istemek için bağlantıyı paylaşma](entitlement-management-access-package-settings.md).  Bir dış Kullanıcı **myaccess.Microsoft.com**ziyaret ederse, erişim paketlerini kendi kuruluşlarındaki kullanıcılara görürler.

* Dizininizde henüz olmayan bir Kullanıcı erişim paketi istemek için erişim portalı 'nda oturum açtığında, kurumsal hesaplarını kullanarak kimlik doğruladıklarından emin olun. Kuruluş hesabı, kaynak dizinindeki bir hesap ya da erişim paketinin ilkelerinden birine dahil olan bir dizin olabilir. Kullanıcının hesabı bir kurumsal hesap değilse veya kimlik doğrulaması yaptıkları Dizin ilkede yer alıyorsa, Kullanıcı erişim paketini görmez. Daha fazla bilgi için bkz. [bir erişim paketine erişim isteme](entitlement-management-request-access.md).

* Bir kullanıcının kaynak dizininde oturum açması engellenirse, bu kişiler erişim portalından erişim isteyemeyecektir. Kullanıcının erişim isteyebilmesi için, oturum açma bloğunu kullanıcının profilinden kaldırmanız gerekir. Oturum açma bloğunu kaldırmak için, Azure portal **Azure Active Directory**, **Kullanıcılar**' a, Kullanıcı ' ya ve ardından **profil**' e tıklayın. **Ayarlar** bölümünü düzenleyin ve blok olarak **oturum aç '** a **değiştirin.** Daha fazla bilgi için, bkz. [Azure Active Directory kullanarak kullanıcının profil bilgilerini ekleme veya güncelleştirme](../fundamentals/active-directory-users-profile-azure-portal.md).  Kullanıcının bir [kimlik koruma ilkesi](../identity-protection/howto-unblock-user.md)nedeniyle engellenip engellenmediğini da denetleyebilirsiniz.

* Erişim portalındaki bir kullanıcı hem istek sahibi hem de onaylayan ise, **onaylar** sayfasında erişim paketi isteklerini görmez. Bu davranış bilerek yapılır. bir Kullanıcı kendi isteklerini onaylayamaz. İstediği erişim paketinde, ilkede yapılandırılmış ek onaylayanlar bulunduğundan emin olun. Daha fazla bilgi için bkz. [bir erişim paketi Için değişiklik isteği ve onay ayarları](entitlement-management-access-package-request-policy.md).

* Daha önce dizininizde oturum açmamış yeni bir dış Kullanıcı, SharePoint Online sitesi dahil bir erişim paketi alırsa, hesap SharePoint Online 'da sağlanıncaya kadar erişim paketleri tam olarak teslim edilmemiş olarak görünür.

## <a name="next-steps"></a>Sonraki adımlar

- [Kullanıcıların yetkilendirme yönetimi 'nde nasıl erişimi olduğunu gösteren raporları görüntüleme](entitlement-management-reports.md)
- [Dış kullanıcılar için erişimi yönetir](entitlement-management-external-users.md)
