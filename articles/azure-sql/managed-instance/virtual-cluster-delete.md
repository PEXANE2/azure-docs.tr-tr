---
title: Bir SQL yönetilen örneği sildikten sonra bir alt ağı silme
description: Azure SQL yönetilen örneğini sildikten sonra Azure sanal ağını silmeyi öğrenin.
services: sql-database
ms.service: sql-managed-instance
ms.subservice: operations
ms.custom: seo-lt-2019, sqldbrb=1
ms.devlang: ''
ms.topic: how-to
author: danimir
ms.author: danil
ms.reviewer: douglas, sstein
ms.date: 06/26/2019
ms.openlocfilehash: 496ff6c7ec39706a99bb40447b6443ca71f19e5e
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2021
ms.locfileid: "99093698"
---
# <a name="delete-a-subnet-after-deleting-an-azure-sql-managed-instance"></a>Azure SQL yönetilen örneğini sildikten sonra bir alt ağı silme
[!INCLUDE[appliesto-sqlmi](../includes/appliesto-sqlmi.md)]

Bu makalede, içinde bulunan son Azure SQL yönetilen örneğini sildikten sonra bir alt ağın el ile nasıl silineceği hakkında yönergeler verilmektedir.

SQL yönetilen örnekler [sanal kümelere](connectivity-architecture-overview.md#virtual-cluster-connectivity-architecture)dağıtılır. Her sanal küme bir alt ağla ilişkilendirilir. Aynı alt ağda SQL yönetilen örnekleri daha hızlı bir şekilde oluşturmanıza olanak tanımak için sanal küme, son örnek silinmeden sonra 12 saat sonra tasarıma devam ediyor. Boş bir sanal kümenin tutulması ücretsizdir. Bu süre boyunca sanal kümeye ilişkilendirilmiş alt ağa silinemez.

12 saat beklemek istemiyorsanız ve sanal kümeyi ve alt ağını daha erken silmeyi tercih ediyorsanız, bunu el ile yapabilirsiniz. Azure portal veya Sanal kümeler API 'sini kullanarak sanal kümeyi el ile silin.

> [!IMPORTANT]
> - Silme işleminin başarılı olabilmesi için sanal kümede SQL yönetilen örnek bulunmamalıdır. 
> - Bir sanal kümeyi silme işlemi yaklaşık 1,5 saat boyunca uzun süredir çalışan bir işlemdir (bkz. sanal küme silme zamanı için [SQL yönetilen örnek yönetimi işlemleri](./sql-managed-instance-paas-overview.md#management-operations) ). Bu işlem tamamlanana kadar, sanal küme portalda görünür olmaya devam edecektir.

## <a name="delete-a-virtual-cluster-from-the-azure-portal"></a>Azure portal bir sanal kümeyi silme

Azure portal kullanarak bir sanal kümeyi silmek için sanal küme kaynaklarını arayın.

![Arama kutusuyla vurgulanan Azure portal ekran görüntüsü](./media/virtual-cluster-delete/virtual-clusters-search.png)

Silmek istediğiniz sanal kümeyi bulduktan sonra bu kaynağı seçin ve **Sil**' i seçin. Sanal küme silmeyi onaylamanız istenir.

![Silme seçeneği vurgulanmış şekilde Azure portal Sanal kümeler panosu ekran görüntüsü](./media/virtual-cluster-delete/virtual-clusters-delete.png)

Azure portal bildirimlerde, sanal kümeyi silme isteğinin başarıyla gönderildiğini belirten bir onay gösterilir. Silme işleminin kendisi yaklaşık 1,5 saat boyunca, sanal kümenin portalda hala görünür olacağı bir şekilde sona acaktır. İşlem tamamlandıktan sonra, sanal küme artık görünür olmayacaktır ve bununla ilişkili alt ağ yeniden kullanım için serbest bırakılır.

> [!TIP]
> Sanal kümede gösterilen SQL yönetilen örneği yoksa ve sanal kümeyi silemiyorsa devam eden bir devam eden örnek dağıtımına sahip olduğunuzdan emin olun. Bu, başlatılan ve iptal edilen dağıtımları devam etmektedir. Bunun nedeni, bu işlemlerin hala sanal kümeyi kullandığından, silme işleminden kilitlemelerdir. Örnek dağıtılan kaynak grubunun **dağıtımlar** sekmesini gözden geçirmek, sürmekte olan dağıtımları gösterir. Bu durumda, dağıtımın tamamlanmasını bekleyin, SQL yönetilen örneğini silin ve ardından sanal kümeyi silin.

## <a name="delete-a-virtual-cluster-by-using-the-api"></a>API kullanarak bir sanal kümeyi silme

Bir sanal kümeyi API aracılığıyla silmek için, [Sanal kümeler silme yönteminde](/rest/api/sql/virtualclusters/delete)belirtilen URI parametrelerini kullanın.

## <a name="next-steps"></a>Sonraki adımlar

- Genel bakış için bkz. [Azure SQL yönetilen örneği nedir?](sql-managed-instance-paas-overview.md).
- [SQL yönetilen örneği 'nde bağlantı mimarisi](connectivity-architecture-overview.md)hakkında bilgi edinin.
- [SQL yönetilen örneği için var olan bir sanal ağı değiştirme](vnet-existing-add-subnet.md)hakkında bilgi edinin.
- Bir sanal ağ oluşturmayı, Azure SQL yönetilen örneği oluşturmayı ve bir veritabanını bir veritabanı yedeğinden geri yüklemeyi gösteren bir öğretici için bkz. [Azure SQL yönetilen örneği (portal) oluşturma](instance-create-quickstart.md).
- DNS sorunları için bkz. [Özel BIR DNS yapılandırma](custom-dns-configure.md).