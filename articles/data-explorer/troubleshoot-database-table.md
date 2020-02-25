---
title: Azure Veri Gezgini VERITABANı veya tablo oluşturma veya silme hatası
description: Bu makalede, Azure Veri Gezgini veritabanları ve tablolar oluşturmak ve silmek için sorun giderme adımları açıklanmaktadır.
author: orspod
ms.author: orspodek
ms.reviewer: mblythe
ms.service: data-explorer
ms.topic: conceptual
ms.date: 09/24/2018
ms.openlocfilehash: 67e4c33498b05884fe667a7977ddb40e647ab4c8
ms.sourcegitcommit: dd3db8d8d31d0ebd3e34c34b4636af2e7540bd20
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/22/2020
ms.locfileid: "77562403"
---
# <a name="troubleshoot-failure-to-create-or-delete-a-database-or-table-in-azure-data-explorer"></a>Sorun giderme: Azure Veri Gezgini bir veritabanı veya tablo oluşturma veya silme hatası

Azure Veri Gezgini 'de veritabanları ve tablolarla düzenli olarak çalışırsınız. Bu makalede, ortaya çıkan sorunlara yönelik sorun giderme adımları sunulmaktadır.

## <a name="creating-a-database"></a>Veritabanı oluşturma

1. Uygun izinlerinizin olduğundan emin olun. Bir veritabanı oluşturmak için Azure aboneliği için *katkıda bulunan* veya *sahip* rolünün bir üyesi olmanız gerekir. Gerekirse, uygun role ekleyebilmeleri için abonelik yöneticinizle birlikte çalışın.

1. Veritabanı adı için ad doğrulama hatası olmadığından emin olun. Ad, en fazla 260 karakter uzunluğunda alfasayısal olmalıdır.

1. Veritabanı saklama ve önbelleğe alma değerlerinin izin verilen aralıklar içinde olduğundan emin olun. Bekletme 1 ile 36500 gün (100 yıl) arasında olmalıdır. Önbelleğe alma, 1 ile saklama için ayarlanan maksimum değer olmalıdır.

## <a name="deleting-or-renaming-a-database"></a>Bir veritabanını silme veya yeniden adlandırma

Uygun izinlerinizin olduğundan emin olun. Bir veritabanını silmek veya yeniden adlandırmak için, Azure aboneliği için *katkıda* bulunan veya *sahip* rolünün bir üyesi olmanız gerekir. Gerekirse, uygun role ekleyebilmeleri için abonelik yöneticinizle birlikte çalışın.

## <a name="creating-a-table"></a>Tablo oluşturma

1. Uygun izinlerinizin olduğundan emin olun. Bir tablo oluşturmak için veritabanında *veritabanı yöneticisi* veya *veritabanı kullanıcı* rolünün bir üyesi ya da Azure aboneliğinin *katkıda* bulunan veya *sahip* rolünde olması gerekir. Gerekirse, aboneliğiniz veya küme yöneticinizle birlikte çalışarak uygun role ekleyebilmeleri gerekir.

    İzinler hakkında daha fazla bilgi için bkz. [Veritabanı izinlerini yönetme](manage-database-permissions.md).

1. Aynı ada sahip bir tablonun zaten mevcut olmadığından emin olun. Varsa, şunları yapabilirsiniz: farklı bir ada sahip bir tablo oluşturabilirsiniz; var olan tabloyu yeniden adlandır ( *Tablo Yöneticisi* rolü gerektirir); veya var olan tabloyu ( *veritabanı yönetici* rolü gerektirir) bırakın. Aşağıdaki komutları kullanın.

    ```Kusto
    .drop table <TableName>

   .rename table <OldTableName> to <NewTableName>
    ```

## <a name="deleting-or-renaming-a-table"></a>Tablo silme veya yeniden adlandırma

Uygun izinlerinizin olduğundan emin olun. Bir tabloyu silmek veya yeniden adlandırmak için veritabanında *veritabanı yöneticisi* veya *Tablo Yöneticisi* rolünün bir üyesi olmanız gerekir. Gerekirse, aboneliğiniz veya küme yöneticinizle birlikte çalışarak uygun role ekleyebilmeleri gerekir.

İzinler hakkında daha fazla bilgi için bkz. [Veritabanı izinlerini yönetme](manage-database-permissions.md).

## <a name="general-guidance"></a>Genel kılavuz

1. [Azure hizmet durumu panosunu](https://azure.microsoft.com/status/) denetleyin. Bir veritabanı veya tabloyla çalışmayı denediğiniz bölgedeki Azure Veri Gezgini durumunu arayın.

    Durum **iyi** değilse (yeşil onay işareti), durum artdıktan sonra yeniden deneyin.

1. Sorununuzu çözmeye hala yardım gerekiyorsa lütfen [Azure Portal](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview)bir destek isteği açın.

## <a name="next-steps"></a>Sonraki adımlar

[Küme durumunu denetleme](check-cluster-health.md)