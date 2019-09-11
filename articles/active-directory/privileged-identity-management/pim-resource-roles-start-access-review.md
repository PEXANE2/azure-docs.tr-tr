---
title: PıM-Azure Active Directory 'de Azure Kaynak rolleri için erişim gözden geçirmesi oluşturma | Microsoft Docs
description: Azure AD Privileged Identity Management (PıM) içinde Azure Kaynak rolleri için erişim gözden geçirmesi oluşturmayı öğrenin.
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.subservice: pim
ms.date: 04/29/2019
ms.author: curtand
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: ba674c143cd8d85b9764c8ade776b52dd1d3598d
ms.sourcegitcommit: 95b180c92673507ccaa06f5d4afe9568b38a92fb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/08/2019
ms.locfileid: "70804150"
---
# <a name="create-an-access-review-of-azure-resource-roles-in-pim"></a>PıM 'de Azure Kaynak rolleri için erişim gözden geçirmesi oluşturma

Çalışanların zaman içindeki değişiklikleri için ayrıcalıklı Azure Kaynak rollerine erişim. Eski rol atamalarıyla ilişkili riski azaltmak için, erişimi düzenli olarak gözden geçirmeniz gerekir. Ayrıcalıklı Azure Kaynak rolleri için erişim gözden geçirmeleri oluşturmak üzere Azure Active Directory (Azure AD) Privileged Identity Management (PıM) kullanabilirsiniz. Ayrıca, otomatik olarak gerçekleşen yinelenen erişim incelemelerini yapılandırabilirsiniz.

Bu makalede, ayrıcalıklı Azure Kaynak rolleri için bir veya daha fazla erişim incelemesi oluşturma açıklanır.

## <a name="prerequisites"></a>Önkoşullar

- [Ayrıcalıklı rol yöneticisi](../users-groups-roles/directory-assign-admin-roles.md#privileged-role-administrator)

## <a name="open-access-reviews"></a>Açık erişim İncelemeleri

1. Ayrıcalıklı rol yöneticisi rolünün üyesi olan bir kullanıcıyla [Azure Portal](https://portal.azure.com/) oturum açın.

1. **Azure AD Privileged Identity Management**açın.

1. Sol taraftaki menüden **Azure kaynakları**' na tıklayın.

1. Yönetmek istediğiniz kaynağa (abonelik veya yönetim grubu gibi) tıklayın.

1. Yönet altında **erişim gözden geçirmeleri**' na tıklayın.

    ![Azure kaynakları-tüm incelemelerinin durumunu gösteren erişim gözden geçirmeleri listesi](./media/pim-resource-roles-start-access-review/access-reviews.png)


[!INCLUDE [Privileged Identity Management access reviews](../../../includes/active-directory-privileged-identity-management-access-reviews.md)]


## <a name="start-the-access-review"></a>Erişim gözden geçirmesini Başlat

Erişim gözden geçirmesi ayarlarını belirttikten sonra **Başlat**' a tıklayın. Erişim incelemesi, listenizde durumunun bir göstergesi olacak şekilde görünür.

![Başlangıç incelemesi durumunu gösteren erişim gözden geçirmeleri listesi](./media/pim-resource-roles-start-access-review/access-reviews-list.png)

Varsayılan olarak, Azure AD gözden geçirenlere İnceleme başladıktan kısa bir bir e-posta gönderir. Azure AD 'ye e-posta gönderme seçeneğini belirlerseniz, gözden geçirenlere bir erişim incelemesinin tamamlanmasını beklediğini bildirin. [Azure Kaynak rollerine erişimi gözden geçirme](pim-resource-roles-perform-access-review.md)yönergelerini görüntüleyebilirsiniz.

## <a name="manage-the-access-review"></a>Erişim gözden geçirmesini yönetme

Gözden geçirenler, erişim incelemesinin **genel bakış** sayfasında İncelemeleri tamamlamalarını izleyerek ilerlemeyi izleyebilirsiniz. [İnceleme tamamlanana](pim-resource-roles-complete-access-review.md)kadar dizinde erişim hakkı değiştirilmez.

![İnceleme ayrıntılarını gösteren erişim gözden geçirmeleri genel bakış sayfası](./media/pim-resource-roles-start-access-review/access-review-overview.png)

Bu bir kerelik bir gözden geçirime sahip olursa, erişim gözden geçirme süresi dolduktan sonra veya yönetici erişim gözden geçirmesini durdurduktan sonra, sonuçları görmek ve uygulamak için [Azure Kaynak rollerinin erişim Incelemesini tamamlayın](pim-resource-roles-complete-access-review.md) .  

Bir dizi erişim incelemesini yönetmek için, erişim gözden geçirmelerine gidin ve zamanlanan incelemelerde yaklaşan örnekleri bulacaksınız ve son tarihi düzenleyin ya da gözden geçirenleri buna uygun olarak ekleyin/kaldırın.

**Tamamlanma ayarları sırasında**yaptığınız seçimlere bağlı olarak, otomatik uygulama İnceleme bitiş tarihinden sonra veya gözden geçirmeyi el ile durdurduğunuzda yürütülür. İncelemesinin durumu, **uygulama** ve son durum **uygulandı**gibi ara durumlar aracılığıyla **tamamlandı** olarak değişir. Birkaç dakika içinde rollerden kaldırılan reddedilen kullanıcıları görmeyi beklemelisiniz.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Kaynak rollerine erişimi gözden geçirme](pim-resource-roles-perform-access-review.md)
- [Azure Kaynak rolleri için erişim gözden geçirmesini doldurun](pim-resource-roles-complete-access-review.md)
- [Azure AD rolleri için erişim gözden geçirmesi oluşturma](pim-how-to-start-security-review.md)
