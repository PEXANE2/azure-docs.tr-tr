---
title: Toplu indirme grubu üyelik listesi - Azure Active Directory portalı | Microsoft Dokümanlar
description: Azure yönetici merkezinde kullanıcıları toplu olarak ekleyin.
services: active-directory
author: curtand
ms.author: curtand
manager: mtillman
ms.date: 04/16/2020
ms.topic: conceptual
ms.service: active-directory
ms.subservice: users-groups-roles
ms.workload: identity
ms.custom: it-pro
ms.reviewer: jeffsta
ms.collection: M365-identity-device-management
ms.openlocfilehash: 3faca8d1a2538ed03a917d6db8d54323fe626369
ms.sourcegitcommit: 31ef5e4d21aa889756fa72b857ca173db727f2c3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81533707"
---
# <a name="bulk-download-members-of-a-group-in-azure-active-directory"></a>Azure Etkin Dizini'nde bir grubun toplu indirme üyeleri

Azure Etkin Dizin (Azure AD) portalını kullanarak, kuruluşunuzdaki bir grubun üyelerini virgülle ayrılmış değerler (CSV) dosyasına toplu olarak indirebilirsiniz.

## <a name="to-bulk-download-group-membership"></a>Toplu indirme grubu üyeliği için

1. Kuruluşta bir Kullanıcı yöneticisi hesabıyla [Azure portalında](https://portal.azure.com) oturum açın. Grup sahipleri, sahip oldukları grupların toplu indirme üyelerini de toplu olarak indirebilir.
1. Azure AD'de**Tüm grupları** **gruplar** > seçin.
1. Üyeliğini indirmek istediğiniz grubu açın ve ardından **Üyeler'i**seçin.
1. **Üyeler** sayfasında, grup üyelerini listeleyen bir CSV dosyasını indirmek için **Üye İndir'i** seçin.

   ![İndirme Üyeleri komutu grubun profil sayfasında](./media/groups-bulk-download-members/download-panel.png)

## <a name="check-download-status"></a>İndirme durumunu kontrol edin

Bekleyen toplu isteklerinizin durumunu Toplu işlem **sonuçları** sayfasında görebilirsiniz.

[![](media/groups-bulk-download-members/bulk-center.png "Check status in the Bulk Operations Results page")](media/groups-bulk-download-members/bulk-center.png#lightbox)

## <a name="bulk-download-service-limits"></a>Toplu indirme hizmeti sınırları

Grup üyelerinin listesini indirmek için her toplu etkinlik bir saate kadar çalıştırılabilir. Bu, en az 500.000 üyeden oluşan bir listeindirmenize olanak tanır.

## <a name="next-steps"></a>Sonraki adımlar

- [Toplu alma grubu üyeleri](groups-bulk-import-members.md)
- [Toplu kaldırma grup üyeleri](groups-bulk-download-members.md)
