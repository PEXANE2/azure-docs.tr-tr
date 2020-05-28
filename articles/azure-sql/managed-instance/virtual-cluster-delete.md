---
title: Azure SQL yönetilen örneğini sildikten sonra bir alt ağı silme
description: Azure SQL yönetilen örneğini sildikten sonra Azure sanal ağını silmeyi öğrenin.
services: sql-database
ms.service: sql-database
ms.custom: seo-lt-2019, sqldbrb=1
ms.devlang: ''
ms.topic: conceptual
author: danimir
ms.author: danil
ms.reviewer: douglas, carlrab, sstein
ms.date: 06/26/2019
ms.openlocfilehash: 73150ee893ca23ed7996b001bd02acaabbf89dce
ms.sourcegitcommit: 6a9f01bbef4b442d474747773b2ae6ce7c428c1f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/27/2020
ms.locfileid: "84116686"
---
# <a name="delete-a-subnet-after-deleting-an-azure-sql-managed-instance"></a>Azure SQL yönetilen örneğini sildikten sonra bir alt ağı silme
[!INCLUDE[appliesto-sqlmi](../includes/appliesto-sqlmi.md)]

Bu makalede, içinde bulunan son Azure SQL yönetilen örneğini sildikten sonra bir alt ağın el ile nasıl silineceği hakkında yönergeler verilmektedir.

Yönetilen örnekler [sanal kümelere](connectivity-architecture-overview.md#virtual-cluster-connectivity-architecture)dağıtılır. Her sanal küme bir alt ağla ilişkilendirilir. Aynı alt ağda yönetilen örnekleri daha hızlı bir şekilde oluşturmanıza olanak tanımak için sanal küme, son örnek silinmeden sonra 12 saat sonra tasarıma devam ediyor. Boş bir sanal kümenin tutulması ücretsizdir. Bu süre boyunca sanal kümeye ilişkilendirilmiş alt ağa silinemez.

12 saat beklemek istemiyorsanız ve sanal kümeyi ve alt ağını daha erken silmeyi tercih ediyorsanız, bunu el ile yapabilirsiniz. Azure portal veya Sanal kümeler API 'sini kullanarak sanal kümeyi el ile silin.

> [!IMPORTANT]
> - Silme işleminin başarılı olabilmesi için sanal kümenin yönetilen örnek içermemesi gerekir. 
> - Bir sanal kümenin silinmesi, yaklaşık 1,5 saat boyunca uzun süredir çalışan bir işlemdir (Bu işlem tamamlanana kadar, sanal kümenin portalda görünür olacağı sanal küme silme zamanına yönelik [SQL yönetilen örnek yönetimi işlemlerine](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance#managed-instance-management-operations) bakın).

## <a name="delete-virtual-cluster-from-the-azure-portal"></a>Azure portal sanal kümeyi silme

Azure portal kullanarak bir sanal kümeyi silmek için sanal küme kaynaklarını arayın.

![Arama kutusuyla vurgulanan Azure portal ekran görüntüsü](./media/virtual-cluster-delete/virtual-clusters-search.png)

Silmek istediğiniz sanal kümeyi bulduktan sonra bu kaynağı seçin ve **Sil**' i seçin. Sanal küme silmeyi onaylamanız istenir.

![Azure portal Sanal kümeler panosu, silme seçeneği vurgulanmış şekilde ekran görüntüsü](./media/virtual-cluster-delete/virtual-clusters-delete.png)

Azure portal bildirimler, sanal kümeyi silme isteğinin başarıyla gönderildiğini belirten bir onay gösterecektir. Silme işleminin kendisi, sanal kümenin portalda hala görünebileceği yaklaşık 1,5 saat boyunca sona acaktır. İşlem tamamlandıktan sonra, sanal küme artık görünür olmayacaktır ve bununla ilişkili alt ağ yeniden kullanım için serbest bırakılır.

> [!TIP]
> Sanal kümede gösterilen yönetilen örnek yoksa ve sanal kümeyi silemiyorsa devam eden bir devam eden örnek dağıtımına sahip olduğunuzdan emin olun. Bu, başlatılan ve iptal edilen dağıtımları devam etmektedir. Bunun nedeni, bu işlemlerin hala silme işleminden sonra sanal küme kilitlemeyi kullanacaktır. Örnek dağıtılan kaynak grubunun dağıtımlar sekmesini gözden geçirme işlemi, sürmekte olan dağıtımları gösterir. Bu durumda, dağıtımın tamamlanmasını bekleyin, yönetilen örneği ve sonra sanal kümeyi silin.

## <a name="delete-virtual-cluster-by-using-the-api"></a>Sanal kümeyi API kullanarak silme

Bir sanal kümeyi API aracılığıyla silmek için, [Sanal kümeler silme yönteminde](https://docs.microsoft.com/rest/api/sql/virtualclusters/delete)belirtilen URI parametrelerini kullanın.

## <a name="next-steps"></a>Sonraki adımlar

- Genel bakış için bkz. [yönetilen örnek nedir?](sql-managed-instance-paas-overview.md).
- [Yönetilen örnekteki bağlantı mimarisi](connectivity-architecture-overview.md)hakkında bilgi edinin.
- [Yönetilen örnek için var olan bir sanal ağı değiştirme](vnet-existing-add-subnet.md)hakkında bilgi edinin.
- Bir sanal ağ oluşturmayı, yönetilen bir örnek oluşturmayı ve bir veritabanını bir veritabanı yedeklemesinden geri yüklemeyi gösteren bir öğretici için bkz. [Azure SQL yönetilen örneği oluşturma](instance-create-quickstart.md).
- DNS sorunları için bkz. [Özel BIR DNS yapılandırma](custom-dns-configure.md).
