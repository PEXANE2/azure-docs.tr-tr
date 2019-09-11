---
title: PıM-Azure Active Directory 'de Azure AD rollerinin erişim incelemesini tamamlar | Microsoft Docs
description: Azure AD Privileged Identity Management (PıM) içinde Azure AD rollerinin erişim incelemesini tamamlamayı ve sonuçları görüntülemeyi öğrenin
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
editor: ''
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.subservice: pim
ms.date: 06/06/2017
ms.author: curtand
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: e50ccc208219896e89bcc80f40c846f69c759f9b
ms.sourcegitcommit: 95b180c92673507ccaa06f5d4afe9568b38a92fb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/08/2019
ms.locfileid: "70804407"
---
# <a name="complete-an-access-review-of-azure-ad-roles-in-pim"></a>PıM 'de Azure AD rollerinin erişim incelemesini tamamlar
Ayrıcalıklı rol yöneticileri, [erişim incelemesi başlatıldıktan](pim-how-to-start-security-review.md)sonra ayrıcalıklı erişimi gözden geçirebilir. Azure Active Directory (Azure AD) Privileged Identity Management (PıM), kullanıcıların erişimini gözden geçirmesini isteyen bir e-posta gönderir. Bir Kullanıcı bir e-posta göndermediyseniz, bunlara [erişim incelemesi gerçekleştirme](pim-how-to-perform-security-review.md)bölümündeki yönergeleri gönderebilirsiniz.

Erişim gözden geçirme süresi dolduktan sonra veya tüm kullanıcılar kendi kendini gözden geçirmeyi tamamladıktan sonra, gözden geçirmeyi yönetmek ve sonuçları görmek için bu makaledeki adımları izleyin.

## <a name="manage-access-reviews"></a>Erişim incelemelerini yönetme
1. [Azure Portal](https://portal.azure.com/) gidin ve panonuzda **Azure AD Privileged Identity Management** uygulamayı seçin.
2. Panonun **erişim İncelemeleri** bölümünü seçin.
3. Yönetmek istediğiniz erişim gözden geçirmeyi seçin.

Erişim incelemesinin ayrıntı dikey penceresinde, gözden geçirmeyi yönetmeye yönelik bir sayı seçeneği vardır.

![PıM erişimi gözden geçirme düğmeleri-ekran görüntüsü](./media/pim-how-to-complete-review/review-buttons.png)

### <a name="remind"></a>Anımsat
Kullanıcıların kendilerini gözden geçirmesi için bir erişim incelemesi ayarlandıysa, **hatırlat** düğmesi bir bildirim gönderir. 

### <a name="stop"></a>Durdur
Tüm erişim incelemelerinin bitiş tarihi vardır, ancak daha önce bitirmek için **Durdur** düğmesini kullanabilirsiniz. Herhangi bir Kullanıcı bu süre içinde incelenmediyse, gözden geçirmeyi durdurduktan sonra bu kullanıcılara izin vermez. Bir gözden geçirmeyi durdurduktan sonra yeniden başlatmazsanız.

### <a name="apply"></a>Uygula
Bir erişim incelemesi tamamlandıktan sonra, bitiş tarihine ulaştığınızdan veya el ile durdurduğundan, **Uygula** düğmesi gözden geçirme sonucunu uygular. Gözden geçirme sırasında bir kullanıcının erişimi reddedildiyse, bu adım rol atamasını kaldıracak olan adımdır.  

### <a name="export"></a>Dışarı Aktarma
Erişim gözden geçirmesi sonuçlarını el ile uygulamak istiyorsanız, incelemeyi dışarı aktarabilirsiniz. **Dışarı aktar** DÜĞMESI bir CSV dosyasını indirmeye başlayacaktır. Sonuçları Excel veya CSV dosyalarını açan diğer programlarda yönetebilirsiniz.

### <a name="delete"></a>Sil
Daha fazla inceleme ile ilgilenmiyorsanız, silin. **Sil** DÜĞMESI, PIM uygulamasından gözden geçirmeyi kaldırır.

> [!IMPORTANT]
> Silme işlemi gerçekleşmeden önce bir uyarı almaz, bu nedenle gözden geçirmeyi silmek istediğinizden emin olun. 

## <a name="next-steps"></a>Sonraki adımlar

- [PıM 'de Azure AD rolleri için erişim gözden geçirmesi başlatma](pim-how-to-start-security-review.md)
- [PıM 'de Azure AD rollerimin erişim incelemesini gerçekleştirme](pim-how-to-perform-security-review.md)
