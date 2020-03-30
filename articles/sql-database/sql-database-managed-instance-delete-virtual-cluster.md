---
title: Yönetilen bir örneği sildikten sonra bir alt ağı silme
description: Azure SQL Veritabanı yönetilen örneğini sildikten sonra Azure sanal ağını nasıl silebilirsiniz öğrenin.
services: sql-database
ms.service: sql-database
ms.custom: seo-lt-2019
ms.devlang: ''
ms.topic: conceptual
author: danimir
ms.author: danil
ms.reviewer: douglas, carlrab, sstein
ms.date: 06/26/2019
ms.openlocfilehash: 496d67a73207fd17182c31c5adad25c1fbe60c4f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73820458"
---
# <a name="delete-a-subnet-after-deleting-an-azure-sql-database-managed-instance"></a>Azure SQL Veritabanı yönetilen örneğini sildikten sonra bir alt ağı silme

Bu makalede, içinde bulunan son Azure SQL Veritabanı yönetilen örneği sildikten sonra bir alt ağ el ile nasıl silinileceğine ilişkin yönergeler sağlanmaktadır.

Yönetilen örnekler [sanal kümelere](sql-database-managed-instance-connectivity-architecture.md#virtual-cluster-connectivity-architecture)dağıtılır. Her sanal küme bir alt ağile ilişkilidir. Sanal küme, aynı alt ağda daha hızlı yönetilen örnekler oluşturmanıza olanak sağlamak için son örnek silme işleminden sonra 12 saat boyunca tasarım gereği devam eder. Boş bir sanal küme tutmak için hiçbir ücret yoktur. Bu süre boyunca sanal kümeye ilişkilendirilmiş alt ağa silinemez.

12 saat beklemek istemiyorsanız ve sanal kümeyi ve alt kümesini daha erken silmeyi tercih ederseniz, bunu el ile yapabilirsiniz. Azure portalını veya sanal kümeler API'sini kullanarak sanal kümeyi el ile silin.

> [!IMPORTANT]
> - Sanal küme, silmeişleminin başarılı olması için yönetilen örnekler içermemelidir. 
> - Sanal kümenin silinmesi, sanal kümenin bu işlem tamamlanana kadar portalda görünmeye devam edeceği yaklaşık 1,5 saat süren uzun süren bir işlemdir (bkz. güncel sanal küme silme süresi için [Yönetilen örnek yönetim işlemleri).](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance#managed-instance-management-operations)

## <a name="delete-virtual-cluster-from-the-azure-portal"></a>Azure portalından sanal küme silme

Azure portalını kullanarak sanal kümeyi silmek için sanal küme kaynaklarını arayın.

![Arama kutusu vurgulanmış Azure portalının ekran görüntüsü](./media/sql-database-managed-instance-delete-virtual-cluster/virtual-clusters-search.png)

Silmek istediğiniz sanal kümeyi bulmadıktan sonra bu kaynağı seçin ve **Sil'i**seçin. Sanal küme silme işlemini onaylamanız istenir.

![Azure portalı Sanal kümeler panosunun ekran görüntüsü, Silme seçeneği vurgulanır](./media/sql-database-managed-instance-delete-virtual-cluster/virtual-clusters-delete.png)

Azure portalı bildirimleri, sanal kümeyi silme isteğinin başarıyla gönderildiğine dair bir onay gösterir. Silme işleminin kendisi, sanal kümenin portalda görünmeye devam edeceği yaklaşık 1,5 saat sürer. İşlem tamamlandıktan sonra, sanal küme artık görünmez ve onunla ilişkili alt ağ yeniden kullanılmak üzere serbest bırakılacaktır.

> [!TIP]
> Sanal kümede gösterilen yönetilen örnek yoksa ve sanal kümeyi silemiyorsanız, devam eden bir örnek dağıtımının devam etmediğinizden emin olun. Buna, hala devam etmekte olan başlatılan ve iptal edilen dağıtımlar dahildir. Bunun nedeni, bu işlemlerin silme işleminden kilitleyen sanal kümeyi kullanmaya devam edecektir. Örneğin dağıtılan kaynak grubunun Dağıtımlar sekmesini gözden geçirmek, devam eden dağıtımları gösterir. Bu durumda, dağıtımın tamamlanmasını bekleyin, yönetilen örneği ve ardından sanal kümeyi silin.

## <a name="delete-virtual-cluster-by-using-the-api"></a>API'yi kullanarak sanal kümeyi silme

API üzerinden sanal kümeyi silmek [için, sanal kümesilme yönteminde](https://docs.microsoft.com/rest/api/sql/virtualclusters/delete)belirtilen URI parametrelerini kullanın.

## <a name="next-steps"></a>Sonraki adımlar

- Genel bakış için [bkz.](sql-database-managed-instance.md)
- Yönetilen [Örnek'te bağlantı mimarisi](sql-database-managed-instance-connectivity-architecture.md)hakkında bilgi edinin.
- [Yönetilen Örnek için varolan bir sanal ağı](sql-database-managed-instance-configure-vnet-subnet.md)nasıl değiştirbekleyeceğizi öğrenin.
- Sanal ağ oluşturmayı, Yönetilen Örnek'i nasıl oluşturup veritabanı yedeklemesinden veritabanını geri yükleyin, [bkz.](sql-database-managed-instance-get-started.md)
- DNS sorunları için [bkz.](sql-database-managed-instance-custom-dns.md)
