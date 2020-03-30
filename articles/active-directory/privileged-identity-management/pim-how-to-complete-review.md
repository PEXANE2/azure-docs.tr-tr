---
title: PIM'deki Azure REKLAM rollerinin erişim incelemesini bitiş - Azure AD | Microsoft Dokümanlar
description: Azure AD Ayrıcalıklı Kimlik Yönetimi'nde (PIM) Azure AD rollerinin erişim incelemesini nasıl tamamlayamanızı ve sonuçları nasıl görüntüleyin
services: active-directory
documentationcenter: ''
author: curtand
manager: daveba
editor: ''
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.subservice: pim
ms.date: 11/08/2019
ms.author: curtand
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: fe2d85d605b9ee418a5709ddcdb448c56be1d918
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74022274"
---
# <a name="complete-an-access-review-of-azure-ad-roles-in-privileged-identity-management"></a>Ayrıcalıklı Kimlik Yönetimi'nde Azure AD rollerinin erişim incelemesini tamamlayın

Ayrıcalıklı rol [yöneticileri, bir erişim incelemesi başlatıldıktan](pim-how-to-start-security-review.md)sonra ayrıcalıklı erişimi gözden geçirebilirsiniz.  Ayrıcalıklı Kimlik Yönetimi (PIM), Azure Etkin Dizin (Azure AD) kuruluşunuzdaki kullanıcılara otomatik olarak bir e-posta göndererek erişimlerini gözden geçirmelerini sağlar. Bir kullanıcı bir e-posta almadıysa, onlara [erişim incelemesinin nasıl gerçekleştirilebileceğiyle](pim-how-to-perform-security-review.md)ilgili yönergeleri gönderebilirsiniz.

Erişim inceleme süresi bittikten veya tüm kullanıcılar kendi incelemelerini tamamladıktan sonra, incelemeyi yönetmek ve sonuçları görmek için bu makaledeki adımları izleyin.

## <a name="manage-access-reviews"></a>Erişim yorumlarını yönetme

1. [Azure portalına](https://portal.azure.com/) gidin ve panonuzdaki **Azure AD Ayrıcalıklı Kimlik Yönetimi** hizmetini seçin.
1. Panodaki **Access değerlendirmeleri** bölümünü seçin.
1. Yönetmek istediğiniz erişim incelemesini seçin.

Erişim incelemesinin ayrıntı bıçak üzerinde, bu inceleme yönetmek için bir dizi seçenek vardır.

![Ayrıcalıklı Kimlik Yönetimi erişim inceleme düğmeleri - ekran görüntüsü](./media/pim-how-to-complete-review/review-buttons.png)

### <a name="remind"></a>Hatırlat

Kullanıcıların kendilerini gözden geçirmeleri için bir erişim incelemesi ayarlanırsa, **Hatırlat** düğmesi bir bildirim gönderir.

### <a name="stop"></a>Durdur

Tüm erişim yorumlarının bir bitiş tarihi vardır, ancak erken bitirmek için **Durdur** düğmesini kullanabilirsiniz. Bu zamana kadar herhangi bir kullanıcı gözden geçirilmezse, incelemeyi durdurduktan sonra bunu yapamazlar. Bir inceleme durdurulduktan sonra yeniden başlatamazsınız.

### <a name="apply"></a>Uygula

Erişim incelemesi tamamlandıktan sonra, bitiş tarihine ulaştığınız veya el ile durdurduğunuz **için, Uygula** düğmesi incelemenin sonucunu uygular. Gözden geçirmede bir kullanıcının erişimi reddedildiyse, bu, rol atamasını kaldıracak adımdır.  

### <a name="export"></a>Dışarı Aktarma

Erişim incelemesinin sonuçlarını el ile uygulamak istiyorsanız, incelemeyi dışa aktarabilirsiniz. **Dışa Aktarma** düğmesi bir CSV dosyasını indirmeye başlar. Sonuçları Excel'de veya CSV dosyalarını açan diğer programlarda yönetebilirsiniz.

### <a name="delete"></a>Sil

İncelemeyle daha fazla ilgilenmiyorsanız, silin. **Silme** düğmesi, incelemeyi Ayrıcalıklı Kimlik Yönetimi hizmetinden kaldırır.

> [!IMPORTANT]
> Bu yıkıcı değişikliği onaylamanız gerekmez, bu nedenle bu incelemeyi silmek istediğinizi doğrulayın.

## <a name="next-steps"></a>Sonraki adımlar

- [Ayrıcalıklı Kimlik Yönetimi'nde Azure AD rolleri için bir erişim incelemesi başlatma](pim-how-to-start-security-review.md)
- [Ayrıcalıklı Kimlik Yönetimi'ndeki Azure REKLAM rollerimin erişim incelemesini gerçekleştirin](pim-how-to-perform-security-review.md)
