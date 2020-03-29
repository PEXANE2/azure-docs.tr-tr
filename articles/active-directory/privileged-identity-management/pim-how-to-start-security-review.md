---
title: PIM'de Azure REKLAM rollerinin erişim incelemesini oluşturun - Azure AD | Microsoft Dokümanlar
description: Azure AD Ayrıcalıklı Kimlik Yönetimi'nde (PIM) Azure AD rollerinin erişim incelemesini nasıl oluşturabilirsiniz öğrenin.
services: active-directory
documentationcenter: ''
author: curtand
manager: daveba
editor: ''
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.subservice: pim
ms.date: 10/22/2019
ms.author: curtand
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: 2f7e9ef503a9a3469ecbc835be8d9229fbd0167f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73847106"
---
# <a name="create-an-access-review-of-azure-ad-roles-in-privileged-identity-management"></a>Ayrıcalıklı Kimlik Yönetimi'nde Azure AD rollerinin erişim incelemesini oluşturma

Eski rol atamalarıyla ilişkili riski azaltmak için erişimi düzenli olarak gözden geçirmelisiniz. Ayrıcalıklı Azure AD rolleri için erişim incelemeleri oluşturmak için Azure AD Ayrıcalıklı Kimlik Yönetimi'ni (PIM) kullanabilirsiniz. Ayrıca, otomatik olarak oluşan yinelenen erişim incelemelerini de yapılandırabilirsiniz.

Bu makalede, ayrıcalıklı Azure REKLAM rolleri için bir veya daha fazla erişim incelemesi nasıl oluşturulacak açıklanmaktadır.

## <a name="prerequisites"></a>Ön koşullar

[Ayrıcalıklı Rol Yöneticisi](../users-groups-roles/directory-assign-admin-roles.md#privileged-role-administrator)

## <a name="open-access-reviews"></a>Açık erişim incelemeleri

1. Ayrıcalıklı rol yöneticisi rolünün bir üyesi olan bir kullanıcıyla [Azure portalında](https://portal.azure.com/) oturum açın.

1. **Azure AD Ayrıcalıklı Kimlik Yönetimi'ni**açın.

1. Sol menüde **Azure AD rollerini** seçin ve ardından **Yorumlara Eriş'i**seçin.

1. Yönet altında, **Access yorumlarını**seçin.

    ![Azure AD rolleri - Tüm incelemelerin durumunu gösteren incelemeler listesine eriş](./media/pim-how-to-start-security-review/access-reviews.png)

[!INCLUDE [Privileged Identity Management access reviews](../../../includes/active-directory-privileged-identity-management-access-reviews.md)]

## <a name="start-the-access-review"></a>Erişim incelemesini başlatma

Erişim incelemesi ayarlarını belirttikten sonra **Başlat'ı**seçin. Erişim incelemesi listenizde durumunun bir göstergesiyle görünür.

![Başlatılan incelemelerin durumunu gösteren incelemelerlistesine erişin](./media/pim-how-to-start-security-review/access-reviews-list.png)

Varsayılan olarak, Azure AD inceleme başladıktan kısa bir süre sonra gözden geçirenlere bir e-posta gönderir. E-postayı Azure AD'nin göndermemesini seçerseniz, gözden geçirenlere bir erişim incelemesinin tamamlanmasını beklediğini bildirdiğinden emin olun. [Azure AD rollerine erişimi](pim-how-to-perform-security-review.md)nasıl gözden geçireceğinize yönelik yönergeleri onlara gösterebilirsiniz.

## <a name="manage-the-access-review"></a>Erişim incelemesini yönetme

Gözden geçirenler erişim incelemesinin **Genel Bakış** sayfasında incelemelerini tamamlarken ilerlemeyi izleyebilirsiniz. [İnceleme tamamlanana](pim-how-to-complete-review.md)kadar dizinde erişim hakları değiştirilmez.

![İncelemenin ayrıntılarını gösteren incelemelere genel bakış sayfasına erişin](./media/pim-how-to-start-security-review/access-review-overview.png)

Bu tek seferlik bir gözden geçirmeyse, erişim gözden geçirme süresi bittikten veya yönetici erişim incelemesini durdurduktan sonra, sonuçları görmek ve uygulamak için [Azure REKLAM rollerinin erişim incelemesini tamamla'daki](pim-how-to-complete-review.md) adımları izleyin.  

Bir dizi erişim incelemesini yönetmek için erişim incelemesine gidin ve Zamanlanan incelemelerde yaklaşan oluşumları bulun ve bitiş tarihini veya gözden geçirenleri buna göre ekleme/kaldırmayı göreceksiniz.

**Tamamlama ayarlarındaki**seçimlerinize bağlı olarak, incelemenin bitiş tarihinden sonra veya incelemeyi el ile durdurunuzda otomatik uygulama yürütülür. İncelemenin durumu, **Uygulamalı** uygulama ve son olarak **Uygulanan**devlet e doğru **Tamamlanan'dan** değişecektir. Reddedilen kullanıcıların birkaç dakika içinde rollerden kaldırıldığını görmeyi bekleyebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure REKLAM rollerine erişimi gözden geçirin](pim-how-to-perform-security-review.md)
- [Azure AD rollerinin erişim incelemesini tamamlama](pim-how-to-complete-review.md)
- [Azure kaynak rollerinin erişim incelemesi oluşturma](pim-resource-roles-start-access-review.md)
