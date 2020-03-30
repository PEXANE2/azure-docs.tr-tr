---
title: PIM ' de Azure kaynak rollerinin erişim incelemesi oluşturma - Azure AD | Microsoft Dokümanlar
description: Azure AD Ayrıcalıklı Kimlik Yönetimi'nde (PIM) Azure kaynak rollerinin erişim incelemesini nasıl oluşturabilirsiniz öğrenin.
services: active-directory
documentationcenter: ''
author: curtand
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.subservice: pim
ms.date: 11/08/2019
ms.author: curtand
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: ae70b8386b1dc3ebd570d2651cded3eda75dfc53
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73847083"
---
# <a name="create-an-access-review-of-azure-resource-roles-in-privileged-identity-management"></a>Ayrıcalıklı Kimlik Yönetimi'nde Azure kaynak rollerinin erişim incelemesini oluşturma

Çalışanlar için ayrıcalıklı Azure kaynak rollerine erişim zaman içinde değişir. Eski rol atamalarıyla ilişkili riski azaltmak için erişimi düzenli olarak gözden geçirmelisiniz. Ayrıcalıklı Azure kaynak rolleri için erişim incelemeleri oluşturmak için Azure Active Directory (Azure AD) Ayrıcalıklı Kimlik Yönetimi 'ni (PIM) kullanabilirsiniz. Ayrıca, otomatik olarak oluşan yinelenen erişim incelemelerini de yapılandırabilirsiniz.

Bu makalede, ayrıcalıklı Azure kaynak rolleri için bir veya daha fazla erişim incelemesi nasıl oluşturulacak açıklanmaktadır.

## <a name="prerequisites"></a>Ön koşullar

[Ayrıcalıklı Rol Yöneticisi](../users-groups-roles/directory-assign-admin-roles.md#privileged-role-administrator)

## <a name="open-access-reviews"></a>Açık erişim incelemeleri

1. Ayrıcalıklı Rol Yöneticisi rolünün bir üyesi olan bir kullanıcıyla [Azure portalında](https://portal.azure.com/) oturum açın.

1. **Azure AD Ayrıcalıklı Kimlik Yönetimi'ni**açın.

1. Sol menüde **Azure kaynaklarını**seçin.

1. Abonelik veya yönetim grubu gibi yönetmek istediğiniz kaynağı seçin.

1. Yönet altında, **Access yorumlarını**seçin.

    ![Azure kaynakları - Tüm incelemelerin durumunu gösteren incelemeler listesine eriş](./media/pim-resource-roles-start-access-review/access-reviews.png)

[!INCLUDE [Privileged Identity Management access reviews](../../../includes/active-directory-privileged-identity-management-access-reviews.md)]

## <a name="start-the-access-review"></a>Erişim incelemesini başlatma

Erişim incelemesi ayarlarını belirttikten sonra **Başlat'ı**tıklatın. Erişim incelemesi listenizde durumunun bir göstergesiyle görünür.

![Başlatılan incelemenin durumunu gösteren incelemelerlistesine erişin](./media/pim-resource-roles-start-access-review/access-reviews-list.png)

Varsayılan olarak, Azure AD inceleme başladıktan kısa bir süre sonra gözden geçirenlere bir e-posta gönderir. E-postayı Azure AD'nin göndermemesini seçerseniz, gözden geçirenlere bir erişim incelemesinin tamamlanmasını beklediğini bildirdiğinden emin olun. [Azure kaynak rollerine erişimi](pim-resource-roles-perform-access-review.md)nasıl gözden geçireceğinize yönelik yönergeleri onlara gösterebilirsiniz.

## <a name="manage-the-access-review"></a>Erişim incelemesini yönetme

Gözden geçirenler erişim incelemesinin **Genel Bakış** sayfasında incelemelerini tamamlarken ilerlemeyi izleyebilirsiniz. [İnceleme tamamlanana](pim-resource-roles-complete-access-review.md)kadar dizinde erişim hakları değiştirilmez.

![İncelemenin ayrıntılarını gösteren incelemelere genel bakış sayfasına erişin](./media/pim-resource-roles-start-access-review/access-review-overview.png)

Bu tek seferlik bir gözden geçirmeyse, erişim gözden geçirme süresi bittikten veya yönetici erişim incelemesini durdurduktan sonra, sonuçları görmek ve uygulamak için [Azure kaynak rollerinin erişim incelemesini tamamla'daki](pim-resource-roles-complete-access-review.md) adımları izleyin.  

Bir dizi erişim incelemesini yönetmek için erişim incelemesine gidin ve Zamanlanan incelemelerde yaklaşan oluşumları bulun ve bitiş tarihini veya gözden geçirenleri buna göre ekleme/kaldırmayı göreceksiniz.

**Tamamlama ayarlarındaki**seçimlerinize bağlı olarak, incelemenin bitiş tarihinden sonra veya incelemeyi el ile durdurunuzda otomatik uygulama yürütülür. İncelemenin durumu, **Uygulamalı** uygulama ve son olarak **Uygulanan**devlet e doğru **Tamamlanan'dan** değişecektir. Reddedilen kullanıcıların birkaç dakika içinde rollerden kaldırıldığını görmeyi bekleyebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure kaynak rollerine erişimi gözden geçirme](pim-resource-roles-perform-access-review.md)
- [Azure kaynak rollerinin erişim incelemesini tamamlama](pim-resource-roles-complete-access-review.md)
- [Azure AD rollerinin erişim incelemesi oluşturma](pim-how-to-start-security-review.md)
