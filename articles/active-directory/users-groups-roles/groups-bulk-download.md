---
title: Azure Active Directory portalında grupların bir listesini indirin | Microsoft Docs
description: Azure Active Directory içindeki Azure Yönetim Merkezi 'nde Grup özelliklerini toplu olarak indirin.
services: active-directory
author: curtand
ms.author: curtand
manager: mtillman
ms.date: 04/16/2020
ms.topic: how-to
ms.service: active-directory
ms.subservice: users-groups-roles
ms.workload: identity
ms.custom: it-pro
ms.reviewer: jeffsta
ms.collection: M365-identity-device-management
ms.openlocfilehash: 00fdd94e8a8cd4b8769260cd595dfee5ff898039
ms.sourcegitcommit: c4ad4ba9c9aaed81dfab9ca2cc744930abd91298
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/12/2020
ms.locfileid: "84732627"
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

[![](media/groups-bulk-download/bulk-center.png "Check status in the Bulk Operations Results page")](media/groups-bulk-download/bulk-center.png#lightbox)

## <a name="bulk-download-service-limits"></a>Hizmet sınırlarını toplu indirme

Bir grup listesini indirmek için her toplu etkinlik, bir saate kadar çalıştırılabilir. Bu, en az 300.000 grubun bir listesini indirmenizi sağlar.

## <a name="next-steps"></a>Sonraki adımlar

- [Grup üyelerini toplu kaldırma](groups-bulk-remove-members.md)
- [Bir grubun üyelerini indir](groups-bulk-download-members.md)
