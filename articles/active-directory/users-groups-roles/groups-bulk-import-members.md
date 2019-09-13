---
title: Gruba üye eklemek için toplu içeri aktarma karşıya yükleme-Azure Active Directory | Microsoft Docs
description: Grup üyelerini Azure Active Directory Yönetim merkezinde toplu olarak ekleyin.
services: active-directory
author: curtand
ms.author: curtand
manager: mtillman
ms.date: 09/11/2019
ms.topic: conceptual
ms.service: active-directory
ms.subservice: users-groups-roles
ms.workload: identity
ms.custom: it-pro
ms.reviewer: jeffsta
ms.collection: M365-identity-device-management
ms.openlocfilehash: 0335522b398c22fb395305b33bac5a56ba565ee2
ms.sourcegitcommit: d70c74e11fa95f70077620b4613bb35d9bf78484
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/11/2019
ms.locfileid: "70910970"
---
# <a name="bulk-import-group-members-preview-in-azure-active-directory"></a>Azure Active Directory Grup üyelerini Toplu içe aktarma (Önizleme)

Azure Active Directory (Azure AD) portalını kullanarak Grup üyelerini toplu olarak içeri aktarmak için bir virgülle ayrılmış değerler (CSV) dosyası kullanarak gruba çok sayıda üye ekleyebilirsiniz.

> [!NOTE]
> Azure AD toplu işlemleri, Azure AD 'nin genel önizleme özelliğidir ve ücretli Azure AD lisans planıyla birlikte kullanılabilir. Önizleme kullanım koşulları hakkında daha fazla bilgi için bkz. [Microsoft Azure önizlemeleri Için ek kullanım koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="bulk-import-service-limits"></a>Toplu içeri aktarma hizmeti sınırları

Grup üyelerinin bir listesini içeri aktarmaya yönelik her toplu etkinlik, bir saate kadar çalıştırılabilir. Bu, en az 40.000 üye listesinin içe aktarılması işlemini izin vermez.

## <a name="to-bulk-import-group-members"></a>Grup üyelerini toplu olarak içeri aktarmak için

1. [Azure Portal,](https://portal.azure.com) kuruluştaki bir kullanıcı yönetici hesabıyla oturum açın. Grup sahipleri ayrıca sahip oldukları grupların üyelerini toplu olarak içeri aktarabilir.
1. Azure AD 'de **gruplar** > **tüm gruplar**' ı seçin.
1. Üyeler eklemekte olduğunuz grubu açın ve ardından **Üyeler**' i seçin.
1. **Üyeler** sayfasında, gruba aktarmak istediğiniz üyeleri listelemek üzere bir CSV dosyasını indirmek, güncelleştirmek ve karşıya yüklemek Için **üyeleri içeri aktar** ' ı seçin.

   ![Üyeleri Içeri aktar komutu, grubun profil sayfasında bulunur](./media/groups-bulk-import-members/import-panel.png)

## <a name="check-import-status"></a>İçeri aktarma durumunu denetle

Tüm bekleyen toplu isteklerinizin durumunu **toplu işlem sonuçları (Önizleme)** sayfasında görebilirsiniz.

   ![Toplu işlem sonuçları sayfasında, toplu istek durumu gösterilir](./media/groups-bulk-import-members/bulk-center.png)

## <a name="next-steps"></a>Sonraki adımlar

- [Grup üyelerini toplu kaldırma](groups-bulk-remove-members.md)
- [Bir grubun üyelerini indir](groups-bulk-download-members.md)
