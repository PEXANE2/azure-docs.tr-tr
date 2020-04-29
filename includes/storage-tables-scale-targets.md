---
author: tamram
ms.service: storage
ms.topic: include
ms.date: 03/09/2020
ms.author: tamram
ms.openlocfilehash: 27617da97ced9ac775beae71e4c25202944b2aba
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "78942062"
---
Aşağıdaki tabloda Tablo depolaması için kapasite, ölçeklenebilirlik ve performans hedefleri açıklanmaktadır.

| Kaynak | Hedef |
|----------|---------------|
| Bir Azure depolama hesabındaki tablo sayısı | Yalnızca depolama hesabının kapasitesine göre sınırlandırılır |
| Tablodaki bölüm sayısı | Yalnızca depolama hesabının kapasitesine göre sınırlandırılır |
| Bir bölümdeki varlıkların sayısı | Yalnızca depolama hesabının kapasitesine göre sınırlandırılır |
| Tek bir tablonun en büyük boyutu | 500 TiB |
| Tüm özellik değerleri dahil olmak üzere tek bir varlığın en büyük boyutu | 1 MIB |
| Bir tablo varlığındaki en fazla özellik sayısı | 255 (üç sistem özelliği, **Partitionkey**, **Rowkey**ve **timestamp**dahil) |
| Bir varlıktaki tek bir özelliğin en büyük toplam boyutu | Özellik türüne göre farklılık gösterir. Daha fazla bilgi için bkz. [Tablo hizmeti veri modelini anlama](/rest/api/storageservices/understanding-the-table-service-data-model)Içindeki **özellik türleri** . |
| **Partitionkey** boyutu | Boyutu 1 kıb 'ye kadar olan bir dize |
| **Rowkey** boyutu | Boyutu 1 kıb 'ye kadar olan bir dize |
| Bir varlık grubu işleminin boyutu | Bir işlem en fazla 100 varlığı içerebilir ve yükün boyutu 4 MIB 'den az olmalıdır. Bir varlık grubu işlemi, bir varlığa yalnızca bir kez güncelleştirme içerebilir. |
| Tablo başına en fazla depolanan erişim ilkesi sayısı | 5 |
| Depolama hesabı başına en fazla istek oranı | saniyede 20.000 işlem, 1-KiB varlık boyutunu varsayan |
| Tek bir tablo bölümü için hedef aktarım hızı (1 KiB-varlıklar) | Saniyede 2.000 adede kadar varlık |