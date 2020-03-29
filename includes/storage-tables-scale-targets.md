---
author: tamram
ms.service: storage
ms.topic: include
ms.date: 03/09/2020
ms.author: tamram
ms.openlocfilehash: 27617da97ced9ac775beae71e4c25202944b2aba
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78942062"
---
Aşağıdaki tabloda Tablo depolama için kapasite, ölçeklenebilirlik ve performans hedefleri açıklanmaktadır.

| Kaynak | Hedef |
|----------|---------------|
| Azure depolama hesabındaki tablo sayısı | Yalnızca depolama hesabının kapasitesiyle sınırlıdır |
| Tablodaki bölüm sayısı | Yalnızca depolama hesabının kapasitesiyle sınırlıdır |
| Bir bölümdeki varlıkların sayısı | Yalnızca depolama hesabının kapasitesiyle sınırlıdır |
| Tek bir tablonun maksimum boyutu | 500 TiB |
| Tüm özellik değerleri de dahil olmak üzere tek bir varlığın maksimum boyutu | 1 MiB |
| Tablo varlığındaki maksimum özellik sayısı | 255 (üç sistem özellikleri, **PartitionKey**, **RowKey**ve **Timestamp**dahil) |
| Bir varlıktaki tek bir özelliğin maksimum toplam boyutu | Özellik türüne göre değişir. Daha fazla bilgi için Tablo [Hizmeti Veri Modelini Anlamada](/rest/api/storageservices/understanding-the-table-service-data-model) **Özellik Türleri'ne** bakın. |
| **PartitionKey** Boyutu | Boyutu 1 KiB kadar bir dize |
| **RowKey** Boyutu | Boyutu 1 KiB kadar bir dize |
| Varlık grubu hareketinin boyutu | Bir işlem en fazla 100 varlık içerebilir ve taşıma kapasitesi 4 MiB'den küçük olmalıdır. Varlık grubu hareketi, bir varlığa yalnızca bir kez güncelleştirme içerebilir. |
| Tablo başına en fazla depolanan erişim ilkeleri sayısı | 5 |
| Depolama hesabı başına maksimum istek oranı | Saniyede 20.000 işlem, hangi bir 1-KiB varlık boyutu varsayar |
| Tek bir tablo bölümü için hedef iş ortası (1 KiB-varlıkları) | Saniyede en fazla 2.000 varlık |