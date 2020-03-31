---
title: Toplu indirme grubu üyelik listesi - Azure Active Directory portalı | Microsoft Dokümanlar
description: Azure yönetici merkezinde kullanıcıları toplu olarak ekleyin.
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
ms.openlocfilehash: 4e29aacb1357509e2b000a9d05c5ced8f9a30dce
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "72517166"
---
# <a name="bulk-download-members-of-a-group-preview-in-azure-active-directory"></a>Azure Etkin Dizini'nde bir grubun toplu indirme üyeleri (önizleme)

Azure Etkin Dizin (Azure AD) portalını kullanarak, kuruluşunuzdaki bir grubun üyelerini virgülle ayrılmış değerler (CSV) dosyasına toplu olarak indirebilirsiniz.

## <a name="to-bulk-download-group-membership"></a>Toplu indirme grubu üyeliği için

1. Kuruluşta bir Kullanıcı yöneticisi hesabıyla [Azure portalında](https://portal.azure.com) oturum açın. Grup sahipleri, sahip oldukları grupların toplu indirme üyelerini de toplu olarak indirebilir.
1. Azure AD'de**Tüm grupları** **gruplar** > seçin.
1. Üyeliğini indirmek istediğiniz grubu açın ve ardından **Üyeler'i**seçin.
1. **Üyeler** sayfasında, grup üyelerini listeleyen bir CSV dosyasını indirmek için **Üye İndir'i** seçin.

   ![İndirme Üyeleri komutu grubun profil sayfasında](./media/groups-bulk-download-members/download-panel.png)

## <a name="check-download-status"></a>İndirme durumunu kontrol edin

**Toplu işlem sonuçları (önizleme)** sayfasında bekleyen toplu isteklerinizin durumunu görebilirsiniz.

   ![Toplu işlem sonuçları sayfası toplu istek durumunu gösterir](./media/groups-bulk-download-members/bulk-center.png)

## <a name="bulk-download-service-limits"></a>Toplu indirme hizmeti sınırları

Grup üyelerinin listesini indirmek için her toplu etkinlik bir saate kadar çalıştırılabilir. Bu, en az 500.000 üyeden oluşan bir listeindirmenize olanak tanır.

## <a name="next-steps"></a>Sonraki adımlar

- [Toplu alma grubu üyeleri](groups-bulk-import-members.md)
- [Toplu kaldırma grup üyeleri](groups-bulk-download-members.md)
