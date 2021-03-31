---
title: Grup üyeliği listesini toplu indirme-Azure Active Directory Portal | Microsoft Docs
description: Azure Yönetim Merkezi 'nde toplu olarak Kullanıcı ekleyin.
services: active-directory
author: curtand
ms.author: curtand
manager: daveba
ms.date: 12/02/2020
ms.topic: how-to
ms.service: active-directory
ms.subservice: enterprise-users
ms.workload: identity
ms.custom: it-pro
ms.reviewer: jeffsta
ms.collection: M365-identity-device-management
ms.openlocfilehash: d4055fb3bfdbcfd7e4a74d9290017a6193578146
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2021
ms.locfileid: "96547705"
---
# <a name="bulk-download-members-of-a-group-in-azure-active-directory"></a>Azure Active Directory bir grubun üyelerini toplu indirin

Azure Active Directory (Azure AD) portalını kullanarak, kuruluşunuzdaki bir grubun üyelerini bir virgülle ayrılmış değerler (CSV) dosyasına toplu olarak indirebilirsiniz.

## <a name="to-bulk-download-group-membership"></a>Grup üyeliğini toplu olarak indirmek için

1. [Azure Portal,](https://portal.azure.com) kuruluştaki bir kullanıcı yönetici hesabıyla oturum açın. Grup sahipleri, sahip oldukları grupların üyelerini de toplu olarak indirebilir.
1. Azure AD 'de **gruplar**  >  **tüm gruplar**' ı seçin.
1. Üyeliğini indirmek istediğiniz grubu açın ve ardından **Üyeler**' i seçin.
1. **Üyeler** sayfasında, grup üyelerini listelemek üzere bir CSV dosyası indirmek Için **üyeleri indir** ' i seçin.

   ![Üyeleri Indir komutu, grubun profil sayfasında bulunur](./media/groups-bulk-download-members/download-panel.png)

## <a name="check-download-status"></a>İndirme durumunu denetle

Tüm bekleyen toplu isteklerinizin durumunu **toplu işlem sonuçları** sayfasında görebilirsiniz.

[![Toplu Işlemler sonuçları sayfasında durumu kontrol edin.](./media/groups-bulk-download-members/bulk-center.png)](./media/groups-bulk-download-members/bulk-center.png#lightbox)

## <a name="bulk-download-service-limits"></a>Hizmet sınırlarını toplu indirme

Grup üyelerinin bir listesini indirmek için her toplu etkinlik, bir saate kadar çalıştırılabilir. Bu, en az 500.000 üyenin bir listesini indirmenizi sağlar.

## <a name="next-steps"></a>Sonraki adımlar

- [Grup üyelerini Toplu içe aktarma](groups-bulk-import-members.md)
- [Grup üyelerini toplu kaldırma](groups-bulk-download-members.md)
