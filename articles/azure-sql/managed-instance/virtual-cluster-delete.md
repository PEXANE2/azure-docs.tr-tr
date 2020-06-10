---
title: Yönetilen bir SQL yönetilen örneği silindikten sonra bir alt ağı silme
description: Azure SQL yönetilen örneği 'nin yönetilen bir örneğini sildikten sonra Azure sanal ağını silmeyi öğrenin.
services: sql-database
ms.service: sql-database
ms.custom: seo-lt-2019, sqldbrb=1
ms.devlang: ''
ms.topic: conceptual
author: danimir
ms.author: danil
ms.reviewer: douglas, carlrab, sstein
ms.date: 06/26/2019
ms.openlocfilehash: 595faa716aae3604033b3de35eb718a24d083fd0
ms.sourcegitcommit: 5a8c8ac84c36859611158892422fc66395f808dc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/10/2020
ms.locfileid: "84657957"
---
# <a name="delete-a-subnet-after-deleting-a-managed-instance-of-sql-managed-instance"></a>Yönetilen bir SQL yönetilen örneği silindikten sonra bir alt ağı silme
[!INCLUDE[appliesto-sqlmi](../includes/appliesto-sqlmi.md)]

Bu makalede, içinde bulunan Azure SQL yönetilen örneği 'nin son yönetilen örneğini sildikten sonra bir alt ağın el ile nasıl silineceği hakkında yönergeler verilmektedir.

Yönetilen örnekler [sanal kümelere](connectivity-architecture-overview.md#virtual-cluster-connectivity-architecture)dağıtılır. Her sanal küme bir alt ağla ilişkilendirilir. Aynı alt ağda yönetilen örnekleri daha hızlı bir şekilde oluşturmanıza olanak tanımak için sanal küme, son örnek silinmeden sonra 12 saat sonra tasarıma devam ediyor. Boş bir sanal kümenin tutulması ücretsizdir. Bu süre boyunca sanal kümeye ilişkilendirilmiş alt ağa silinemez.

12 saat beklemek istemiyorsanız ve sanal kümeyi ve alt ağını daha erken silmeyi tercih ediyorsanız, bunu el ile yapabilirsiniz. Azure portal veya Sanal kümeler API 'sini kullanarak sanal kümeyi el ile silin.

> [!IMPORTANT]
> - Silme işleminin başarılı olabilmesi için sanal kümenin yönetilen örnek içermemesi gerekir. 
> - Bir sanal kümeyi silme işlemi yaklaşık 1,5 saat boyunca uzun süredir çalışan bir işlemdir (bkz. güncel sanal küme silme zamanı için [yönetilen örnek yönetimi işlemleri](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance#managed-instance-management-operations) ). Bu işlem tamamlanana kadar, sanal küme portalda görünür olmaya devam edecektir.

## <a name="delete-a-virtual-cluster-from-the-azure-portal"></a>Azure portal bir sanal kümeyi silme

Azure portal kullanarak bir sanal kümeyi silmek için sanal küme kaynaklarını arayın.

![Arama kutusuyla vurgulanan Azure portal ekran görüntüsü](./media/virtual-cluster-delete/virtual-clusters-search.png)

Silmek istediğiniz sanal kümeyi bulduktan sonra bu kaynağı seçin ve **Sil**' i seçin. Sanal küme silmeyi onaylamanız istenir.

![Silme seçeneği vurgulanmış şekilde Azure portal Sanal kümeler panosu ekran görüntüsü](./media/virtual-cluster-delete/virtual-clusters-delete.png)

Azure portal bildirimlerde, sanal kümeyi silme isteğinin başarıyla gönderildiğini belirten bir onay gösterilir. Silme işleminin kendisi yaklaşık 1,5 saat boyunca, sanal kümenin portalda hala görünür olacağı bir şekilde sona acaktır. İşlem tamamlandıktan sonra, sanal küme artık görünür olmayacaktır ve bununla ilişkili alt ağ yeniden kullanım için serbest bırakılır.

> [!TIP]
> Sanal kümede gösterilen yönetilen örnek yoksa ve sanal kümeyi silemiyorsa devam eden bir devam eden örnek dağıtımına sahip olduğunuzdan emin olun. Bu, başlatılan ve iptal edilen dağıtımları devam etmektedir. Bunun nedeni, bu işlemlerin hala sanal kümeyi kullandığından, silme işleminden kilitlemelerdir. Örnek dağıtılan kaynak grubunun **dağıtımlar** sekmesini gözden geçirmek, sürmekte olan dağıtımları gösterir. Bu durumda, dağıtımın tamamlanmasını bekleyin, yönetilen örneği silin ve ardından sanal kümeyi silin.

## <a name="delete-a-virtual-cluster-by-using-the-api"></a>API kullanarak bir sanal kümeyi silme

Bir sanal kümeyi API aracılığıyla silmek için, [Sanal kümeler silme yönteminde](https://docs.microsoft.com/rest/api/sql/virtualclusters/delete)belirtilen URI parametrelerini kullanın.

## <a name="next-steps"></a>Sonraki adımlar

- Genel bakış için bkz. [Azure SQL yönetilen örneği nedir?](sql-managed-instance-paas-overview.md).
- [SQL yönetilen örneği 'nde bağlantı mimarisi](connectivity-architecture-overview.md)hakkında bilgi edinin.
- [SQL yönetilen örneği için var olan bir sanal ağı değiştirme](vnet-existing-add-subnet.md)hakkında bilgi edinin.
- Bir sanal ağ oluşturmayı, yönetilen bir örnek oluşturmayı ve bir veritabanını bir veritabanı yedeğinden geri yüklemeyi gösteren bir öğretici için bkz. [yönetilen örnek oluşturma](instance-create-quickstart.md).
- DNS sorunları için bkz. [Özel BIR DNS yapılandırma](custom-dns-configure.md).
