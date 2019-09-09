---
title: PıM-Azure Active Directory 'de Azure Kaynak rolleri için etkinlik ve denetim geçmişini görüntüleme | Microsoft Docs
description: Azure AD Privileged Identity Management (PıM) içindeki Azure Kaynak rolleri için etkinlik ve denetim geçmişini görüntüleyin.
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
editor: ''
ms.assetid: ''
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: pim
ms.date: 04/09/2019
ms.author: curtand
ms.collection: M365-identity-device-management
ms.openlocfilehash: 40e1ed018f66d4f5eefd02d587504cf64969f47b
ms.sourcegitcommit: 95b180c92673507ccaa06f5d4afe9568b38a92fb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/08/2019
ms.locfileid: "70804057"
---
# <a name="view-activity-and-audit-history-for-azure-resource-roles-in-pim"></a>PıM 'de Azure Kaynak rolleri için etkinlik ve denetim geçmişini görüntüleme

Azure Active Directory (Azure AD) Privileged Identity Management (PıM) ile kuruluşunuzdaki Azure kaynakları rollerinin etkinlik, etkinleştirmeler ve denetim geçmişini görüntüleyebilirsiniz. Buna abonelikler, kaynak grupları ve hatta sanal makineler dahildir. Azure rol tabanlı erişim denetimi (RBAC) işlevselliğiyle ilgili Azure portal içindeki herhangi bir kaynak, PıM 'deki güvenlik ve yaşam döngüsü yönetim özelliğinden yararlanabilir.

## <a name="view-activity-and-activations"></a>Etkinlik ve etkinleştirmeleri görüntüleme

Belirli bir kullanıcının çeşitli kaynaklarda hangi işlemleri yaptığını görmek için, belirli bir etkinleştirme süresiyle ilişkili Azure Kaynak etkinliğini görüntüleyebilirsiniz.

1. **Azure AD Privileged Identity Management**açın.

1. **Azure kaynakları**' na tıklayın.

1. Etkinlik ve etkinleştirmeleri görüntülemek istediğiniz kaynağa tıklayın.

1. **Roller** veya **Üyeler**' e tıklayın.

1. Bir kullanıcıya tıklayın.

    Azure kaynaklarında tarihe göre Kullanıcı eylemlerinin grafik görünümünü görürsünüz. Ayrıca, aynı dönemdeki son rol etkinleştirmeleri da gösterilmektedir.

    ![Kaynak etkinlik Özeti ve rol etkinleştirmeleri ile kullanıcı ayrıntıları](media/azure-pim-resource-rbac/rbac-user-details.png)

1. Ayrıntıları ve ilgili Kullanıcı etkinken gerçekleşen Azure Kaynak etkinliğini görmek için belirli bir rol etkinleştirmesine tıklayın.

    ![Seçilen rol etkinleştirme ve etkinlik ayrıntıları tarihe göre görüntülendi](media/azure-pim-resource-rbac/rbac-user-resource-activity.png)

## <a name="export-role-assignments-with-children"></a>Rol atamalarını alt öğeler ile dışarı aktarma

Denetçilerin rol atamalarının tamamen bir listesini sağlamanız gereken bir uyumluluk gereksinimine sahip olabilirsiniz. PıM, tüm alt kaynaklar için rol atamaları içeren belirli bir kaynaktaki rol atamalarını sorgulamanızı sağlar. Daha önce, yöneticilerin bir abonelik için rol atamalarının tamamen bir listesini alması ve her belirli kaynak için rol atamalarını dışarı aktarmaları gerekiyordu. PıM 'yi kullanarak tüm kaynak grupları ve kaynakları için rol atamaları dahil olmak üzere bir abonelikte tüm etkin ve uygun rol atamalarını sorgulayabilirsiniz.

1. **Azure AD Privileged Identity Management**açın.

1. **Azure kaynakları**' na tıklayın.

1. Abonelik gibi rol atamalarını dışarı aktarmak istediğiniz kaynağa tıklayın.

1. **Üyeler**' e tıklayın.

1. Dışarı aktar üyelik bölmesini açmak için **dışarı aktar** ' a tıklayın.

    ![Tüm üyeleri dışarı aktarmak için üyelik bölmesini dışarı aktar](media/azure-pim-resource-rbac/export-membership.png)

1. Bir CSV dosyasındaki tüm rol atamalarını dışarı aktarmak için **tüm üyeleri dışarı aktar** ' a tıklayın.

    ![Excel 'de CSV fil içindeki rol atamaları verildi](media/azure-pim-resource-rbac/export-csv.png)

## <a name="view-resource-audit-history"></a>Kaynak denetim geçmişini görüntüle

Kaynak denetimi size bir kaynak için tüm rol etkinliklerinin görünümünü sağlar.

1. **Azure AD Privileged Identity Management**açın.

1. **Azure kaynakları**' na tıklayın.

1. Denetim geçmişini görüntülemek istediğiniz kaynağa tıklayın.

1. **Kaynak denetimi**' ne tıklayın.

1. Geçmişi önceden tanımlanmış bir tarih veya özel Aralık kullanarak filtreleyin.

    ![Filtrelerle kaynak denetim listesi](media/azure-pim-resource-rbac/rbac-resource-audit.png)

1. **Denetim türü**için Etkinleştir ' i **(atanan + etkin)** seçin.

    ![Etkinleştirme denetim türüne göre filtrelenen kaynak denetim listesi](media/azure-pim-resource-rbac/rbac-audit-activity.png)

1. **Eylem**' in altında, kullanıcının Azure kaynaklarında etkinlik ayrıntılarını görmek için **(etkinlik)** seçeneğine tıklayın.

    ![Belirli bir eylem için Kullanıcı Etkinliği Ayrıntıları](media/azure-pim-resource-rbac/rbac-audit-activity-details.png)

## <a name="view-my-audit"></a>Denetmi görüntüle

My Audit, kişisel rol etkinliğinizi görüntülemenize olanak sağlar.

1. **Azure AD Privileged Identity Management**açın.

1. **Azure kaynakları**' na tıklayın.

1. Denetim geçmişini görüntülemek istediğiniz kaynağa tıklayın.

1. **Denetlememe**tıklayın.

1. Geçmişi önceden tanımlanmış bir tarih veya özel Aralık kullanarak filtreleyin.

    ![Geçerli Kullanıcı için denetim listesi](media/azure-pim-resource-rbac/my-audit-time.png)

## <a name="next-steps"></a>Sonraki adımlar

- [PıM 'de Azure Kaynak rolleri atama](pim-resource-roles-assign-roles.md)
- [PıM 'de Azure Kaynak rolleri için istekleri onaylama veya reddetme](pim-resource-roles-approval-workflow.md)
- [PıM 'de Azure AD rollerinin denetim geçmişini görüntüleme](pim-how-to-use-audit-log.md)
