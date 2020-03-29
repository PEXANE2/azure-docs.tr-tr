---
title: Lisansları atama veya kaldırma - Azure Etkin Dizin | Microsoft Dokümanlar
description: Azure Active Directory lisanslarını kullanıcılarınızdan veya gruplarınızdan nasıl atayabildiğiniz veya kaldıracağınız hakkında talimatlar.
services: active-directory
author: msaburnley
manager: daveba
ms.assetid: f8b932bc-8b4f-42b5-a2d3-f2c076234a78
ms.service: active-directory
ms.subservice: fundamentals
ms.workload: identity
ms.topic: conceptual
ms.date: 09/06/2018
ms.author: ajburnle
ms.reviewer: jeffsta
ms.custom: it-pro, seodec18
ms.collection: M365-identity-device-management
ms.openlocfilehash: 20ec1d1909a53a9de29e12be33957acfd1643698
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80128835"
---
# <a name="assign-or-remove-licenses-in-the-azure-active-directory-portal"></a>Azure Etkin Dizin portalında lisans atama veya kaldırma

Birçok Azure Etkin Dizin (Azure AD) hizmeti, bu hizmet için kullanıcılarınızın veya gruplarınızın (ve ilişkili üyelerin) her birini lisanslamanı gerektirir. Yalnızca etkin lisansa sahip kullanıcılar, doğru olan lisanslı Azure REKLAM hizmetlerine erişebilir ve bunları kullanabilir. Lisanslar kiracı başına uygulanır ve diğer kiracılara devredilmez. 

## <a name="available-license-plans"></a>Kullanılabilir lisans planları

Azure AD hizmeti için aşağıdakiler dahil olmak üzere çeşitli lisans planları vardır:

- Azure AD Ücretsiz

- Azure AD Premium P1

- Azure AD Premium P2

Her lisans planı ve ilişkili lisans ayrıntıları hakkında özel bilgiler için [bkz.](https://azure.microsoft.com/pricing/details/active-directory/)

Tüm Microsoft hizmetleri tüm konumlarda kullanılamaz. Bir lisansın bir gruba atanabilmesi için, tüm üyelerin **Kullanım konumunu** belirtmeniz gerekir. Bu değeri Azure AD'deki **Azure &gt; &gt; Etkin &gt; Dizin Kullanıcıları Profil Ayarları** alanında ayarlayabilirsiniz. Kullanım konumu belirtilmeyen tüm kullanıcı, Azure AD kuruluşunun konumunu devralır.

## <a name="view-license-plans-and-plan-details"></a>Lisans planlarını ve plan ayrıntılarını görüntüleme

Tek tek lisanslar da dahil olmak üzere kullanılabilir hizmet planlarınızı görüntüleyebilir, bekleyen son kullanma tarihlerini denetleyebilir ve kullanılabilir atama sayısını görüntüleyebilirsiniz.

### <a name="to-find-your-service-plan-and-plan-details"></a>Servis planınızı ve plan bilgilerinizi bulmak için

1. Azure REKLAM kuruluşunuzdaki Bir Lisans yöneticisi hesabını kullanarak [Azure portalında](https://portal.azure.com/) oturum açın.

1. **Azure Etkin Dizini'ni**seçin ve ardından **Lisansları**seçin.

    ![Satın alınan hizmet sayısı ve atanan lisansların yer aldığı lisanslar sayfası](media/license-users-groups/license-details-blade.png)

1. **Ürünler** sayfasını görüntülemek ve lisans planlarınız için **Atanan**, **Kullanılabilir**ve **Süresi Dolan** numaralarını görmek için **Satın Alınan** bağlantıyı seçin.

    ![hizmet lisans planları ve ilgili lisans bilgileri ile hizmet sayfası](media/license-users-groups/license-products-blade-with-products.png)

1. Lisanslı kullanıcılarını ve gruplarını görmek için bir plan adı seçin.

## <a name="assign-licenses-to-users-or-groups"></a>Kullanıcılara veya gruplara lisans atama

Lisanslı bir Azure AD hizmetini kullanması gereken herkesin uygun lisansa sahip olduğundan emin olun. Lisans haklarını kullanıcılara veya tüm gruba ekleyebilirsiniz.

### <a name="to-assign-a-license-to-a-user"></a>Bir kullanıcıya lisans atamak için

1. **Ürünler** sayfasında, kullanıcıya atamak istediğiniz lisans planının adını seçin.

    ![hizmetler sayfası, vurgulanmış hizmet lisans planı ile](media/license-users-groups/license-products-blade-with-product-highlight.png)

1. Lisans planına genel bakış sayfasında **Atla'yı**seçin.

    ![hizmetler sayfası, vurgulanmış Atama seçeneği ile](media/license-users-groups/license-products-blade-with-assign-option-highlight.png)

1. **Atama** **sayfasında, Kullanıcılar ve gruplar**seçin ve ardından lisansatını atadığınız kullanıcıyı arayın ve seçin.

    ![Vurgulanmış arama ve Seç seçenekleriyle lisans sayfası atama](media/license-users-groups/assign-license-blade-with-highlight.png)

1. **Atama seçeneklerini**seçin, uygun lisans seçeneklerinin açık olduğundan emin olun ve ardından **Tamam'ı**seçin.

    ![Lisans planında bulunan tüm seçeneklerle lisans seçeneği sayfası](media/license-users-groups/license-option-blade-assignments.png)

    Bir kullanıcının seçili olduğunu ve atamaların yapılandırıldığını göstermek için Lisans Sayfası **Ata.**

    > [!NOTE]
    > Tüm Microsoft hizmetleri tüm konumlarda kullanılamaz. Lisansın bir kullanıcıya atanabilmesi için kullanım **konumunu**belirtmeniz gerekir. Bu değeri Azure AD'deki **Azure &gt; &gt; Etkin &gt; Dizin Kullanıcıları Profil Ayarları** alanında ayarlayabilirsiniz. Kullanım konumu belirtilmeyen tüm kullanıcı, Azure AD kuruluşunun konumunu devralır.

1. **Ata**'yı seçin.

    Kullanıcı lisanslı kullanıcılar listesine eklenir ve dahil edilen Azure AD hizmetlerine erişebilir.
    > [!NOTE]
    > Lisanslar, kullanıcının **Lisanslar** sayfasından doğrudan bir kullanıcıya da atanabilir. Bir kullanıcının grup üyeliği aracılığıyla atanmış bir lisansı varsa ve aynı lisansı doğrudan kullanıcıya atamak istiyorsanız, bu yalnızca adım 1'de belirtilen **Ürünler** sayfasından yapılabilir.

### <a name="to-assign-a-license-to-a-group"></a>Bir gruba lisans atamak için

1. **Ürünler** sayfasında, kullanıcıya atamak istediğiniz lisans planının adını seçin.

    ![Ürünler bıçak, vurgulanmış ürün lisans planı ile](media/license-users-groups/license-products-blade-with-product-highlight.png)

1. Azure **Active Directory Premium Plan 2** sayfasında **Atama'yı**seçin.

    ![Vurgulanan Atama seçeneğiyle ürünler sayfası](media/license-users-groups/license-products-blade-with-assign-option-highlight.png)

1. **Atama** **sayfasında, Kullanıcılar ve gruplar**seçin ve ardından lisansatını atadığınız grubu arayın ve seçin.

    ![Vurgulanmış arama ve Seç seçenekleriyle lisans sayfası atama](media/license-users-groups/assign-group-license-blade-with-highlight.png)

1. **Atama seçeneklerini**seçin, uygun lisans seçeneklerinin açık olduğundan emin olun ve ardından **Tamam'ı**seçin.

    ![Lisans planında bulunan tüm seçeneklerle lisans seçeneği sayfası](media/license-users-groups/license-option-blade-group-assignments.png)

    Bir kullanıcının seçili olduğunu ve atamaların yapılandırıldığını göstermek için Lisans Sayfası **Ata.**

1. **Ata**'yı seçin.

    Grup lisanslı gruplar listesine eklenir ve tüm üyeler dahil edilen Azure AD hizmetlerine erişebilir.

## <a name="remove-a-license"></a>Lisansı kaldırma

Bir lisans, bir kullanıcının Azure AD kullanıcı sayfasından, grup ataması için gruba genel bakış sayfasından veya lisans için kullanıcıları ve grupları görmek için Azure AD **Lisansları** sayfasından başlayarak lisansı kaldırabilirsiniz.

### <a name="to-remove-a-license-from-a-user"></a>Bir kullanıcıdan lisans kaldırmak için

1. Hizmet planı için **Lisanslı kullanıcılar** sayfasında, artık lisansa sahip olmaması gereken kullanıcıyı seçin. Örneğin, _Alain Charon._

1. **Lisansı Kaldır'ı**seçin.

    ![Lisans Kaldır seçeneği vurgulanmış lisanslı kullanıcılar sayfası](media/license-users-groups/license-products-user-blade-with-remove-option-highlight.png)

> [!IMPORTANT]
> Bir kullanıcının bir gruptan devraldığı lisanslar doğrudan kaldırılamaz. Bunun yerine, kullanıcıyı lisansı devraldıkları gruptan kaldırmanız gerekir.

### <a name="to-remove-a-license-from-a-group"></a>Bir lisansı gruptan kaldırmak için

1. Lisans planı için **Lisanslı gruplar** sayfasında, artık lisansa sahip olmaması gereken grubu seçin.

1. **Lisansı Kaldır'ı**seçin.

    ![Lisans ı Kaldır seçeneği vurgulanan lisanslı gruplar sayfası](media/license-users-groups/license-products-group-blade-with-remove-option-highlight.png)
    
    > [!NOTE]
    > Azure AD ile senkronize edilen şirket içi bir kullanıcı hesabı eşitleme için kapsam dışına çıktığında veya eşitleme kaldırıldığında, kullanıcı Azure AD'de yumuşak bir şekilde silinir. Bu durumda, kullanıcıya doğrudan veya grup tabanlı lisans yoluyla atanan lisanslar **silinmek**yerine **askıya alınmış** olarak işaretlenir.

## <a name="next-steps"></a>Sonraki adımlar

Lisanslarınızı atadıktan sonra aşağıdaki işlemleri gerçekleştirebilirsiniz:

- [Lisans atama sorunlarını belirleme ve çözme](../users-groups-roles/licensing-groups-resolve-problems.md)

- [Lisans için bir gruba lisanslı kullanıcı ekleme](../users-groups-roles/licensing-groups-migrate-users.md)

- [Azure Etkin Dizini'nde lisanslamayı yönetmek için grupları kullanarak senaryolar, sınırlamalar ve bilinen sorunlar](../users-groups-roles/licensing-group-advanced.md)

- [Profil bilgileri ekleme veya değiştirme](active-directory-users-profile-azure-portal.md)
