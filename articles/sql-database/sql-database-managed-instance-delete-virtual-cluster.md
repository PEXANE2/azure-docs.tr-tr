---
title: Azure SQL veritabanı yönetilen örneğini sildikten sonra bir alt ağı silme | Microsoft Docs
description: Azure SQL veritabanı yönetilen örneğini sildikten sonra Azure sanal ağını silme hakkında bilgi edinin.
services: sql-database
ms.service: sql-database
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: danimir
ms.author: danil
ms.reviewer: douglas, carlrab, sstein
manager: craigg
ms.date: 06/26/2019
ms.openlocfilehash: ead7ea91e172f608c5364e4d5164d2a71dbf2f5f
ms.sourcegitcommit: a8b638322d494739f7463db4f0ea465496c689c6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/17/2019
ms.locfileid: "68297635"
---
# <a name="delete-a-subnet-after-deleting-an-azure-sql-database-managed-instance"></a>Azure SQL veritabanı yönetilen örneğini sildikten sonra bir alt ağı silme

Bu makalede, içinde bulunan son Azure SQL veritabanı yönetilen örneğini sildikten sonra bir alt ağın el ile nasıl silineceği hakkında yönergeler verilmektedir.

SQL veritabanı, silinen yönetilen örneği içeren bir [sanal küme](sql-database-managed-instance-connectivity-architecture.md#virtual-cluster-connectivity-architecture) kullanır. Aynı alt ağda hızlı bir şekilde yönetilen örnekler oluşturmanıza olanak tanımak için, sanal küme örnek silinmeden 12 saat sonra devam ediyor. Boş bir sanal kümenin tutulması ücretsizdir. Bu süre boyunca sanal kümeye ilişkilendirilmiş alt ağa silinemez.

12 saat beklemek istemiyorsanız ve sanal kümeyi ve alt ağını hemen silmeyi tercih ediyorsanız, bunu el ile yapabilirsiniz. Azure portal veya Sanal kümeler API 'sini kullanarak sanal kümeyi el ile silin.

> [!NOTE]
> Silme işleminin başarılı olabilmesi için sanal kümenin yönetilen örnek içermemesi gerekir.

## <a name="delete-virtual-cluster-from-the-azure-portal"></a>Azure portal sanal kümeyi silme

Azure portal kullanarak bir sanal kümeyi silmek için sanal küme kaynaklarını arayın.

![Arama kutusuyla vurgulanan Azure portal ekran görüntüsü](./media/sql-database-managed-instance-delete-virtual-cluster/virtual-clusters-search.png)

Silmek istediğiniz sanal kümeyi bulduktan sonra bu kaynağı seçin ve **Sil**' i seçin. Sanal küme silmeyi onaylamanız istenir.

![Azure portal Sanal kümeler panosu, silme seçeneği vurgulanmış şekilde ekran görüntüsü](./media/sql-database-managed-instance-delete-virtual-cluster/virtual-clusters-delete.png)

Azure portal bildirimler alanında, sanal kümenin silindiğini onaylamanız gösterilmektedir. Sanal kümenin başarıyla silinmesi, alt ağı yeniden kullanım için hemen yayınlar.

> [!TIP]
> Sanal kümede gösterilen yönetilen örnek yoksa ve sanal kümeyi silemiyorsa devam eden bir devam eden örnek dağıtımına sahip olduğunuzdan emin olun. Bu, başlatılan ve iptal edilen dağıtımları devam etmektedir. Örnek dağıtılan kaynak grubunun dağıtımlar sekmesini gözden geçirme işlemi, sürmekte olan dağıtımları gösterir. Bu durumda, dağıtımın tamamlanmasını beklemek için, yönetilen örneği ve sonra sanal kümeyi silin.

## <a name="delete-virtual-cluster-by-using-the-api"></a>Sanal kümeyi API kullanarak silme

Bir sanal kümeyi API aracılığıyla silmek için, [Sanal kümeler silme yönteminde](https://docs.microsoft.com/rest/api/sql/virtualclusters/delete)belirtilen URI parametrelerini kullanın.

## <a name="next-steps"></a>Sonraki adımlar

- Genel bakış için bkz. [yönetilen örnek nedir?](sql-database-managed-instance.md).
- [Yönetilen örnekteki bağlantı mimarisi](sql-database-managed-instance-connectivity-architecture.md)hakkında bilgi edinin.
- [Yönetilen örnek için var olan bir sanal ağı değiştirme](sql-database-managed-instance-configure-vnet-subnet.md)hakkında bilgi edinin.
- Bir sanal ağ oluşturmayı, yönetilen bir örnek oluşturmayı ve bir veritabanını bir veritabanı yedeklemesinden geri yüklemeyi gösteren bir öğretici için bkz. [Azure SQL veritabanı yönetilen örneği oluşturma](sql-database-managed-instance-get-started.md).
- DNS sorunları için bkz. [Özel BIR DNS yapılandırma](sql-database-managed-instance-custom-dns.md).
