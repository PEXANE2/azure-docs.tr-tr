---
title: Azure Active Directory portalında grupların bir listesini indirin | Microsoft Docs
description: Azure Active Directory içindeki Azure Yönetim Merkezi 'nde Grup özelliklerini toplu olarak indirin.
services: active-directory
author: curtand
ms.author: curtand
manager: daveba
ms.date: 11/15/2020
ms.topic: how-to
ms.service: active-directory
ms.workload: identity
ms.custom: it-pro
ms.reviewer: jeffsta
ms.collection: M365-identity-device-management
ms.openlocfilehash: bd624abdcd358dfc1d26c3092e53691ad51cf75f
ms.sourcegitcommit: b8eba4e733ace4eb6d33cc2c59456f550218b234
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/23/2020
ms.locfileid: "95503547"
---
# <a name="bulk-download-a-list-of-groups-in-azure-active-directory"></a>Azure Active Directory grupların listesini toplu olarak indirme

Azure Active Directory (Azure AD) portalını kullanarak, kuruluşunuzdaki tüm grupların listesini bir virgülle ayrılmış değerler (CSV) dosyasına toplu olarak indirebilirsiniz.

## <a name="to-download-a-list-of-groups"></a>Grupların listesini indirmek için

1. [Azure Portal,](https://portal.azure.com) kuruluştaki bir yönetici hesabıyla oturum açın.
1. Azure AD 'de **gruplar**  >  **karşıdan yükleme grupları**' nı seçin.
1. Grupları **indirme** sayfasında, gruplarınızı listelemek için **Başlat** ' ı seçin.

   ![Grupları indir komutu tüm gruplar sayfasında bulunur](./media/groups-bulk-download/bulk-download.png)

## <a name="check-download-status"></a>İndirme durumunu denetle

Tüm bekleyen toplu isteklerinizin durumunu **toplu işlem sonuçları** sayfasında görebilirsiniz.

[![Toplu Işlemler sonuçları sayfasında durumu kontrol edin.](./media/groups-bulk-download/bulk-center.png)](./media/groups-bulk-download/bulk-center.png#lightbox)

## <a name="bulk-download-service-limits"></a>Hizmet sınırlarını toplu indirme

Bir grup listesini indirmek için her toplu etkinlik, bir saate kadar çalıştırılabilir. Bu, en az 300.000 grubun bir listesini indirmenizi sağlar.

## <a name="next-steps"></a>Sonraki adımlar

- [Grup üyelerini toplu kaldırma](groups-bulk-remove-members.md)
- [Bir grubun üyelerini indir](groups-bulk-download-members.md)
