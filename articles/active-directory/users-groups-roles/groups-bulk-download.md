---
title: Azure Etkin Dizin portalındaki grupların listesini indirin | Microsoft Dokümanlar
description: Azure Etkin Dizini'ndeki Azure yönetici merkezinde grup özelliklerini toplu olarak indirin.
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
ms.openlocfilehash: 4b08e807e179270b63ca81d3777c230c3e129c3a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "72517143"
---
# <a name="bulk-download-a-list-of-groups-preview-in-azure-active-directory"></a>Azure Etkin Dizini'ndeki grupların (önizleme) listesini toplu olarak indirin

Azure Etkin Dizin (Azure AD) portalını kullanarak, kuruluşunuzdaki tüm grupların listesini virgülle ayrılmış değerler (CSV) dosyasına toplu olarak indirebilirsiniz.

## <a name="to-download-a-list-of-groups"></a>Grupların listesini indirmek için

1. Kuruluşta bir yönetici hesabı yla [Azure portalında](https://portal.azure.com) oturum açın.
1. Azure AD'de, **Gruplar** > **İndirme gruplarını**seçin.
1. Gruplar **indirme** sayfasında, gruplarınızı listeleyen bir CSV dosyasını almak için **Başlat'ı** seçin.

   ![İndirme grupları komutu Tüm Gruplar sayfasında](./media/groups-bulk-download/bulk-download.png)

## <a name="check-download-status"></a>İndirme durumunu kontrol edin

**Toplu işlem sonuçları (önizleme)** sayfasında bekleyen toplu isteklerinizin durumunu görebilirsiniz.

   ![Toplu işlem sonuçları sayfası toplu istek durumunu gösterir](./media/groups-bulk-download/bulk-center.png)

## <a name="bulk-download-service-limits"></a>Toplu indirme hizmeti sınırları

Grup listesini indirmek için her toplu etkinlik bir saate kadar çalıştırılabilir. Bu, en az 300.000 gruptan oluşan bir liste indirmenize olanak tanır.

## <a name="next-steps"></a>Sonraki adımlar

- [Toplu kaldırma grup üyeleri](groups-bulk-remove-members.md)
- [Bir grubun üyelerini indirin](groups-bulk-download-members.md)
