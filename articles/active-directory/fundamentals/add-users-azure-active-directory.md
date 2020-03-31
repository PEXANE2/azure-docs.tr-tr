---
title: Kullanıcı ekleme veya silme - Azure Etkin Dizini | Microsoft Dokümanlar
description: Azure Active Directory'yi kullanarak yeni kullanıcılar ekleme veya varolan kullanıcıları silme hakkında yönergeler.
services: active-directory
author: msaburnley
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: fundamentals
ms.topic: conceptual
ms.date: 11/12/2019
ms.author: ajburnle
ms.reviewer: jeffsta
ms.custom: it-pro, seodec18
ms.collection: M365-identity-device-management
ms.openlocfilehash: 3d72616422934501e042375edfb10a25aa27c527
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79262119"
---
# <a name="add-or-delete-users-using-azure-active-directory"></a>Azure Etkin Dizin'i kullanarak kullanıcı ekleme veya silme

Azure Etkin Dizin (Azure AD) kuruluşunuzdan yeni kullanıcılar ekleyin veya mevcut kullanıcıları silin. Kullanıcı eklemek veya silmek için Kullanıcı yöneticisi veya Global yöneticisi olmalısınız.

## <a name="add-a-new-user"></a>Yeni kullanıcı ekleme

Azure Active Directory portalını kullanarak yeni bir kullanıcı oluşturabilirsiniz.

Yeni bir kullanıcı eklemek için aşağıdaki adımları izleyin:

1. Kuruluş için Kullanıcı yöneticisi olarak [Azure portalında](https://portal.azure.com/) oturum açın.

1. Herhangi bir sayfadan *Azure Active Directory'yi* arayın ve seçin.

1. **Kullanıcıları**seçin ve ardından **Yeni kullanıcıyı**seçin.

    ![Kullanıcılar aracılığıyla kullanıcı ekleme - Azure AD'deki tüm kullanıcılar](media/add-users-azure-active-directory/add-user-in-users-all-users.png)

1. **Kullanıcı** sayfasında, bu kullanıcı için bilgi girin:

   - **Adı**. Gereklidir. Yeni kullanıcının adı ve soyadı. Örneğin, *Mary Parker.*

   - **Kullanıcı adı**. Gereklidir. Yeni kullanıcının kullanıcı adı. Örneğin, `mary@contoso.com`.

     Kullanıcı adının etki alanı bölümü, ilk varsayılan etki alanı adını, * \<alan adınızı>.onmicrosoft.com*veya *contoso.com*gibi özel bir etki alanı adını kullanmalıdır. Özel bir etki alanı adı oluşturma hakkında daha fazla bilgi için azure [Etkin Dizin portalını kullanarak özel alan adınızı ekle'ye](add-custom-domain.md)bakın.

   - **Gruplar**. İsteğe bağlı olarak, kullanıcıyı varolan bir veya daha fazla gruba ekleyebilirsiniz. Kullanıcıyı daha sonra gruplara da ekleyebilirsiniz. Kullanıcıları gruplara ekleme hakkında daha fazla bilgi için [bkz.](active-directory-groups-create-azure-portal.md)

   - **Dizin rolü**: Kullanıcı için Azure AD yönetim izinleri gerekiyorsa, bunları Azure AD rolüne ekleyebilirsiniz. Kullanıcıyı Global yönetici veya Azure AD'deki sınırlı yönetici rollerinden biri veya birkaçı olarak atayabilirsiniz. Rolleratama hakkında daha fazla bilgi [için, kullanıcılara nasıl rol atayabildiğinize](active-directory-users-assign-role-azure-portal.md)bakın.

   - **İş bilgileri**: Burada kullanıcı hakkında daha fazla bilgi ekleyebilir veya daha sonra yapabilirsiniz. Kullanıcı bilgileri ekleme hakkında daha fazla bilgi [için, kullanıcı profili bilgilerini nasıl ekleyeceğiniz veya değiştireceğiniz bilgisine](active-directory-users-profile-azure-portal.md)bakın.

1. **Parola** kutusunda sağlanan otomatik olarak oluşturulan parolayı kopyalayın. Oturum açmanız için bu parolayı kullanıcıya vermeniz gerekir.

1. **Oluştur'u**seçin.

Kullanıcı oluşturulur ve Azure REKLAM kuruluşunuza eklenir.

## <a name="add-a-new-guest-user"></a>Yeni konuk kullanıcı ekleme

Ayrıca, **Yeni kullanıcı** sayfasından **Davet kullanıcısını** seçerek yeni konuk kullanıcıyı kuruluşunuzla işbirliği yapmaya da davet edebilirsiniz. Kuruluşunuzun dış işbirliği ayarları, konukları davet etme izniniz olacak şekilde yapılandırılırsa, kullanıcıya işbirliğine başlamak için kabul etmeleri gereken bir davet e-posta yla gönderilecektir. B2B işbirliği kullanıcılarını davet etme hakkında daha fazla bilgi için [B2B kullanıcılarını Azure Etkin Dizini'ne davet etme](../b2b/add-users-administrator.md)

## <a name="add-a-consumer-user"></a>Tüketici kullanıcısı ekleme

Azure Active Directory B2C (Azure AD B2C) dizininizde tüketici hesaplarını el ile oluşturmak istediğiniz senaryolar olabilir. Tüketici hesapları oluşturma hakkında daha fazla bilgi için Azure [AD B2C'de tüketici kullanıcıları oluştur ve silme](../../active-directory-b2c/manage-users-portal.md)'ye bakın.

## <a name="add-a-new-user-within-a-hybrid-environment"></a>Karma ortam içinde yeni bir kullanıcı ekleme

Hem Azure Etkin Dizini (bulut) hem de Windows Server Active Directory (şirket içinde) içeren bir ortamınız varsa, varolan kullanıcı hesabı verilerini eşitleyerek yeni kullanıcılar ekleyebilirsiniz. Karma ortamlar ve kullanıcılar hakkında daha fazla bilgi için bkz. Şirket [içi dizinlerinizi Azure Etkin Dizini ile tümleştirin.](../hybrid/whatis-hybrid-identity.md)

## <a name="delete-a-user"></a>Kullanıcı silme

Azure Active Directory portalLarını kullanarak varolan bir kullanıcıyı silebilirsiniz.

Bir kullanıcıyı silmek için aşağıdaki adımları izleyin:

1. Kuruluşun Kullanıcı yöneticisi hesabını kullanarak [Azure portalında](https://portal.azure.com/) oturum açın.

1. Herhangi bir sayfadan *Azure Active Directory'yi* arayın ve seçin.

1. Azure AD kiracınızdan silmek istediğiniz kullanıcıyı arayın ve seçin. Örneğin, _Mary Parker._

1. **Kullanıcı sil**’i seçin.

    ![Kullanıcılar - Delete kullanıcı vurgulanan tüm kullanıcılar sayfası](media/add-users-azure-active-directory/delete-user-all-users-blade.png)

Kullanıcı silinir ve artık Kullanıcılar **- Tüm kullanıcılar** sayfasında görünmez. Kullanıcı, sonraki 30 gün boyunca **Silinen kullanıcılar** sayfasında görülebilir ve bu süre içinde geri yüklenebilir. Bir kullanıcıyı geri yükleme hakkında daha fazla bilgi için Azure [Etkin Dizin'i kullanarak yeni silinen bir kullanıcıyı geri yükleme veya kaldırma](active-directory-users-restore.md)'ya bakın.

Bir kullanıcı silindiğinde, kullanıcı tarafından tüketilen tüm lisanslar diğer kullanıcılar için kullanılabilir hale getirilir.

>[!Note]
>Yetki kaynağı Windows Server Active Directory olan kullanıcıların kimlik, iletişim bilgilerini veya iş bilgilerini güncelleştirmek için Windows Server Active Directory'yi kullanmanız gerekir. Güncelleştirmenizi tamamladıktan sonra, değişiklikleri görmeden önce bir sonraki eşitleme döngüsünün tamamlanmasını beklemeniz gerekir.

## <a name="next-steps"></a>Sonraki adımlar

Kullanıcılarınızı ekledikten sonra aşağıdaki temel işlemleri yapabilirsiniz:

- [Profil bilgileri ekleme veya değiştirme](active-directory-users-profile-azure-portal.md)

- [Kullanıcılara rol atama](active-directory-users-assign-role-azure-portal.md)

- [Temel bir grup oluşturma ve üye ekleme](active-directory-groups-create-azure-portal.md)

- [Dinamik gruplar ve kullanıcılarla çalışın](../users-groups-roles/groups-create-rule.md)

Veya [başka bir dizinden konuk kullanıcı eklemek](../b2b/what-is-b2b.md) veya [silinmiş bir kullanıcıyı geri getirmek](active-directory-users-restore.md)gibi diğer kullanıcı yönetimi görevlerini de yapabilirsiniz. Diğer kullanılabilir eylemler hakkında daha fazla bilgi için [Azure Etkin Dizin kullanıcı yönetimi belgelerine](../users-groups-roles/index.yml)bakın.
