---
author: tamram
ms.service: storage
ms.topic: include
ms.date: 03/09/2020
ms.author: tamram
ms.openlocfilehash: 27617da97ced9ac775beae71e4c25202944b2aba
ms.sourcegitcommit: 8f4d54218f9b3dccc2a701ffcacf608bbcd393a6
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/09/2020
ms.locfileid: "78942062"
---
Aşağıdaki tabloda Tablo depolaması için kapasite, ölçeklenebilirlik ve performans hedefleri açıklanmaktadır.

| Kaynak | Hedef |
|----------|---------------|
| Bir Azure depolama hesabındaki tablo sayısı | Yalnızca depolama hesabının kapasitesine göre sınırlandırılır |
| Tablodaki bölüm sayısı | Yalnızca depolama hesabının kapasitesine göre sınırlandırılır |
| Bölümdeki varlık sayısı | Yalnızca depolama hesabının kapasitesine göre sınırlandırılır |
| Tek tablo için maksimum boyut | 500 TiB |
| Tüm özellik değerleri dahil olmak üzere tek bir varlığın en büyük boyutu | 1 MiB |
| Bir tablo varlığındaki en fazla özellik sayısı | 255 (**PartitionKey**, **RowKey** ve **Timestamp** adli üç sistem özelliği de dahil olmak üzere) |
| Bir varlıktaki tek bir özelliğin en büyük toplam boyutu | Özellik türüne göre farklılık gösterir. Daha fazla bilgi için bkz. [Tablo Hizmeti Veri Modelini anlama](/rest/api/storageservices/understanding-the-table-service-data-model) içindeki **Özellik Türleri**. |
| **PartitionKey** boyutu | Boyutu 1 KiB'ye kadar olan bir dize |
| **RowKey** boyutu | Boyutu 1 KiB'ye kadar olan bir dize |
| Bir varlık grubu işleminin boyutu | Bir işlem en fazla 100 varlığı içerebilir ve yük boyutunun 4 MiB’den az olması gerekir. Bir varlık grubu işlemi, bir varlığa yalnızca bir kez güncelleştirme içerebilir. |
| Tablo başına en fazla depolanan erişim ilkesi sayısı | 5 |
| Depolama hesabı başına istek hızı üst sınırı | Saniyede 20.000 işlem, 1 KiB varlık boyutu varsayılır |
| Tek bir tablo bölümü için hedef aktarım hızı (1 KiB varlıklar) | Saniyede 2.000 adede kadar varlık |