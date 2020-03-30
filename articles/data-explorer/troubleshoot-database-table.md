---
title: Azure Veri Gezgini'nde DB veya tablo oluşturmama veya silme
description: Bu makalede, Azure Veri Gezgini'nde veritabanları ve tablolar oluşturmak ve silen sorun giderme adımlarını açıklanmaktadır.
author: orspod
ms.author: orspodek
ms.reviewer: mblythe
ms.service: data-explorer
ms.topic: conceptual
ms.date: 09/24/2018
ms.openlocfilehash: 67e4c33498b05884fe667a7977ddb40e647ab4c8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77562403"
---
# <a name="troubleshoot-failure-to-create-or-delete-a-database-or-table-in-azure-data-explorer"></a>Sorun Giderme: Azure Veri Gezgini'nde bir veritabanı veya tablo oluşturulamaz veya silinme

Azure Veri Gezgini'nde veritabanları ve tablolarla düzenli olarak çalışırsınız. Bu makalede, gündeme gelebilecek sorunlar için sorun giderme adımları sağlar.

## <a name="creating-a-database"></a>Veritabanı oluşturma

1. Uygun izinlerinizin olduğundan emin olun. Veritabanı oluşturmak için Azure aboneliği için *Katılımcı* veya *Sahip* rolünün bir üyesi olmalısınız. Gerekirse, sizi uygun role ekleyebilmeleri için abonelik yöneticinizle birlikte çalışın.

1. Veritabanı adı için ad doğrulama hatası olmadığından emin olun. Ad, maksimum uzunluğu 260 karakter olan alfasayısal olmalıdır.

1. Veritabanı bekletme ve önbelleğe alma değerlerinin izin verilebilen aralıklar içinde olduğundan emin olun. Bekletme 1 ile 36500 gün (100 yıl) arasında olmalıdır. Önbelleğe alma, bekletme için belirlenen maksimum değer ile 1 arasında olmalıdır.

## <a name="deleting-or-renaming-a-database"></a>Veritabanını silme veya yeniden adlandırma

Uygun izinlerinizin olduğundan emin olun. Bir veritabanını silmek veya yeniden adlandırmak için Azure aboneliği için *Katılımcı* veya *Sahip* rolünün bir üyesi olmalısınız. Gerekirse, sizi uygun role ekleyebilmeleri için abonelik yöneticinizle birlikte çalışın.

## <a name="creating-a-table"></a>Tablo oluşturma

1. Uygun izinlerinizin olduğundan emin olun. Tablo oluşturmak için, veritabanındaki veritabanı *yöneticisinin* veya *veritabanı kullanıcı* rolünün veya Azure aboneliği için *Katılımcı* veya *Sahip* rolünün bir üyesi olmalısınız. Gerekirse, sizi uygun role ekleyebilmeleri için aboneliğiniz veya küme yöneticinizle çalışın.

    İzinler hakkında daha fazla bilgi için bkz. [Veritabanı izinlerini yönetme](manage-database-permissions.md).

1. Aynı ada sahip bir tablonun zaten var olmadığından emin olun. Varsa, o zaman yapabilirsiniz: Farklı bir ada sahip bir tablo oluşturun; varolan tabloyu yeniden adlandırma *(tablo yöneticisi* rolü gerektirir); veya varolan tabloyu bırakın *(veritabanı yöneticisi* rolü gerektirir). Aşağıdaki komutları kullanın.

    ```Kusto
    .drop table <TableName>

   .rename table <OldTableName> to <NewTableName>
    ```

## <a name="deleting-or-renaming-a-table"></a>Tabloyu silme veya yeniden adlandırma

Uygun izinlerinizin olduğundan emin olun. Bir tabloyu silmek veya yeniden adlandırmak için veritabanı yöneticisi veya *tablo* *yöneticisi* rolünün bir üyesi olmalısınız. Gerekirse, sizi uygun role ekleyebilmeleri için aboneliğiniz veya küme yöneticinizle çalışın.

İzinler hakkında daha fazla bilgi için bkz. [Veritabanı izinlerini yönetme](manage-database-permissions.md).

## <a name="general-guidance"></a>Genel rehber

1. [Azure hizmet durumu panosunu](https://azure.microsoft.com/status/) denetleyin. Bir veritabanı veya tabloyla çalışmaya çalıştığınız bölgedeki Azure Veri Gezgini'nin durumunu arayın.

    Durum **İyi** değilse (yeşil onay işareti), durum iyileştikten sonra yeniden deneyin.

1. Sorununuzu çözmek için hala yardıma ihtiyacınız varsa, lütfen [Azure portalında](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview)bir destek isteği açın.

## <a name="next-steps"></a>Sonraki adımlar

[Küme durumunu denetleme](check-cluster-health.md)