---
title: CSV dosyası karşıya yükleyerek grup üyelerini toplu kaldırma-Azure Active Directory | Microsoft Docs
description: Azure Yönetim Merkezi 'nde toplu olarak Kullanıcı ekleyin.
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
ms.openlocfilehash: b153db3570f10ad5ad130dedd0bd20fe22776ed6
ms.sourcegitcommit: d70c74e11fa95f70077620b4613bb35d9bf78484
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/11/2019
ms.locfileid: "70911055"
---
# <a name="bulk-remove-group-members-preview-in-azure-active-directory"></a>Azure Active Directory Grup üyelerini toplu kaldırma (Önizleme)

Azure Active Directory (Azure AD) portalını kullanarak, grup üyelerini toplu olarak kaldırmak için bir virgülle ayrılmış değerler (CSV) dosyası kullanarak bir gruptan çok sayıda üyeyi kaldırabilirsiniz.

> [!NOTE]
> Azure AD toplu işlemleri, Azure AD 'nin genel önizleme özelliğidir ve ücretli Azure AD lisans planıyla birlikte kullanılabilir. Önizleme kullanım koşulları hakkında daha fazla bilgi için bkz. [Microsoft Azure önizlemeleri Için ek kullanım koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="bulk-removal-service-limits"></a>Toplu kaldırma hizmeti sınırları

Grup üyelerinin bir listesini kaldırmak için her toplu etkinlik, bir saate kadar çalıştırılabilir. Bu, en az 40.000 üyenin listesini kaldırmaya izin vermez.

## <a name="to-bulk-remove-group-members"></a>Grup üyelerini toplu olarak kaldırmak için

1. [Azure Portal,](https://portal.azure.com) kuruluştaki bir kullanıcı yönetici hesabıyla oturum açın. Grup sahipleri, sahip oldukları grupların üyelerini de toplu olarak kaldırabilir.
1. Azure AD 'de **gruplar** > **tüm gruplar**' ı seçin.
1. Üyelerini kaldırdığınız grubu açın ve ardından **Üyeler**' i seçin.
1. **Üyeler** sayfasında, gruptan kaldırmak istediğiniz üyeleri listelemek üzere bir CSV dosyasını indirmek, güncelleştirmek ve karşıya yüklemek Için **üyeleri kaldır** ' ı seçin.

   ![Üyeleri Kaldır komutu, grubun profil sayfasında bulunur](./media/groups-bulk-remove-members/remove-panel.png)

## <a name="check-removal-status"></a>Kaldırma durumunu denetle

Tüm bekleyen toplu isteklerinizin durumunu **toplu işlem sonuçları (Önizleme)** sayfasında görebilirsiniz.

   ![Toplu işlem sonuçları sayfasında, toplu istek durumu gösterilir](./media/groups-bulk-remove-members/bulk-center.png)

## <a name="next-steps"></a>Sonraki adımlar

- [Grup üyelerini Toplu içe aktarma](groups-bulk-import-members.md)
- [Bir grubun üyelerini indir](groups-bulk-download-members.md)
