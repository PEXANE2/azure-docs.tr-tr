---
title: Geçmiş verilerini bekletme ilkesiyle yönetme-Azure SQL Edge (Önizleme)
description: Azure SQL Edge 'de bekletme ilkesiyle geçmiş verileri yönetmeyi öğrenin (Önizleme)
keywords: SQL Edge, veri saklama
services: sql-edge
ms.service: sql-edge
ms.topic: conceptual
author: SQLSourabh
ms.author: sourabha
ms.reviewer: sstein
ms.date: 09/04/2020
ms.openlocfilehash: 9acec467819f159623176edf2f3f763a55019eb4
ms.sourcegitcommit: c52e50ea04dfb8d4da0e18735477b80cafccc2cf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/08/2020
ms.locfileid: "89550761"
---
# <a name="manage-historical-data-with-retention-policy"></a>Geçmiş verilerini bekletme ilkesiyle yönetme

Veri saklama, veritabanında ve temel alınan tablolardan birinde etkinleştirilebilir ve kullanıcıların tabloları ve veritabanları için esnek eskime kuralları oluşturmalarına olanak tanır. Veri bekletmenin uygulanması basittir: tablo oluşturma sırasında veya alter table işleminin bir parçası olarak yalnızca bir parametre ayarlanmasını gerektirir. 

Veri saklama ilkesi bir veritabanı ve temel alınan tablo için devre dışı olduktan sonra, bir arka plan zaman Zamanlayıcı görevi, veri saklama için etkin olan tablodan kullanılmayan kayıtları kaldırmak üzere çalışır. Eşleşen satırların tanımlanması ve tablodan kaldırılması, sistem tarafından zamanlanan ve çalıştırılan arka plan görevinde saydam bir şekilde gerçekleşir. Tablo satırları için yaş koşulu, tablo tanımında olarak kullanılan sütuna göre denetlenir `filter_column` . Örneğin, saklama dönemi bir hafta olarak ayarlandıysa, temizleme için uygun tablo satırları aşağıdaki koşulu karşılar: 

```sql
filter_column < DATEADD(WEEK, -1, SYSUTCDATETIME())
```

## <a name="data-retention-cleanup-phases"></a>Veri bekletme Temizleme aşamaları

Veri saklama Temizleme işlemi iki aşamadan oluşur. 
- Keşif aşaması-bu aşamada Temizleme işlemi, temizleme için bir liste oluşturmak üzere Kullanıcı veritabanları içindeki tüm tabloları tanımlar. Bulma günde bir kez çalışır.
- Temizleme aşaması-bu aşamada Temizleme işlemi, bulma aşamasında tanımlanan, sınırlı veri saklama olan tüm tablolarda çalışır. Temizleme işlemi bir tabloda gerçekleştirilemiyorsa, bu tablo geçerli çalıştırmada atlanır ve bir sonraki yinelemede yeniden denenir. Temizleme sırasında aşağıdaki ilkeler kullanılır
    - Eski bir satır başka bir işlem tarafından kilitliyse, bu satır atlanır. 
    - Çalıştırmaları varsayılan 5 saniye kilit zaman aşımı ayarıyla temizler. Zaman aşımı penceresindeki tablolarda kilitler alınamadığından, tablo geçerli çalıştırmada atlanır ve sonraki yinelemede yeniden denenir.
    - Bir tablonun temizlenmesi sırasında bir hata oluşursa, bu tablo atlanır ve sonraki yinelemede alınacaktır.

## <a name="manual-cleanup"></a>El ile temizlik

Bir tablodaki veri bekletme ayarlarına ve veritabanındaki iş yükünün yapısına bağlı olarak, otomatik temizleme iş parçacığı çalışma sırasında tüm eski satırları tamamen kaldıramayabilir. Bu sorun için yardımcı olmak ve kullanıcıların eski satırları el ile kaldırmasına izin vermek için, `sys.sp_cleanup_data_retention` saklı yordam Azure SQL Edge 'de (Önizleme) tanıtılmıştır. 

Bu saklı yordam üç parametre alır. 
    - Şema adı-tablo için sahip olan şemanın adı. Bu gerekli bir parametredir. 
    - Tablo adı-el ile temizleme işleminin çalıştırıldığı tablonun adı. Bu gerekli bir parametredir. 
    - rowcount (çıkış)-çıkış değişkeni. El ile temizleme SP tarafından temizlenen satır sayısını döndürür. Bu, isteğe bağlı bir parametredir. 

Aşağıdaki örnekte, tablosu için el ile temizleme SP 'nin yürütülmesi gösterilmektedir `dbo.data_retention_table` .

```sql
declare @rowcnt bigint 
EXEC sys.sp_cleanup_data_retention 'dbo', 'data_retention_table', @rowcnt output 
select @rowcnt 
```

## <a name="how-obsolete-rows-are-deleted"></a>Eski satırlar silinir

Temizleme işlemi, tablonun Dizin düzenine bağlıdır. Sınırlı saklama süresine sahip tüm tablolar için eski veri temizleme işlemini gerçekleştirmek üzere bir arka plan görevi oluşturulur. Rowstore için Temizleme mantığı (B-ağaç veya yığın) dizin, daha küçük öbeklerdeki eski satırı (10.000 'ye kadar), veritabanı günlüğünde ve GÇ alt sisteminde basınç için en aza indirir. Temizleme mantığı gerekli B-ağacı dizinini kullanmasına karşın, saklama süresinden eski olan satırlarda silme işlemlerinin sırası en kesin garanti edilemez. Bu nedenle, uygulamalarınızda Temizleme sırası üzerinde hiçbir bağımlılık kullanmayın.

Kümelenmiş columnstore için temizleme görevi tüm satır gruplarını tek seferde kaldırır (genellikle, her biri 1.000.000 satır içerir), özellikle veriler oluşturulup yüksek bir hızda yaşlanır.

![Veri bekletme Temizleme](./media/data-retention-cleanup/data-retention-cleanup.png)

Mükemmel veri sıkıştırma ve verimli bekletme Temizleme, iş yükünüz hızlı bir şekilde yüksek miktarda veri oluşturduğunda, kümelenmiş columnstore dizinini senaryolar için kusursuz bir tercih haline getirir.

> [!Note]
> B ağaç dizinleri ve yığınlarında, veri saklama temel tablolarda bir silme sorgusu çalıştırır ve bu, tablolardaki silme tetikleyicilerle çakışabilir. Silme tetiklerinin tablolardan kaldırılması veya silme DML tetikleyicisine sahip tablolarda veri bekletmesini etkinleştirmek önerilir.

## <a name="monitoring-data-retention-cleanup"></a>Veri bekletme temizleme işlemini izleme

Veri bekletme ilkesi temizleme işlemleri, Azure SQL Edge 'de (Önizleme) genişletilmiş olaylar (XEvents) kullanılarak izlenebilir. Genişletilmiş olaylar hakkında daha fazla bilgi için bkz. [XEvents genel bakış](https://docs.microsoft.com/sql/relational-databases/extended-events/extended-events).

Aşağıdaki altı genişletilmiş olay Temizleme işlemlerinin durumunu izlemeye yardımcı olur. 

| Ad | Açıklama |
|------| ------------|
| data_retention_task_started  | Saklama ilkesi ile tabloların temizlenmesi için arka plan görevi başladığında gerçekleşir. |
| data_retention_task_completed  | Bekletme ilkesi ile tabloların temizlenmesi için arka plan görevi sona erdiğinde gerçekleşir. |
| data_retention_task_exception  | Saklama ilkesi içeren tabloların temizlenmesi için arka plan görevi, tablosuna özgü bekletme Temizleme işlemi dışında başarısız olduğunda gerçekleşir. |
| data_retention_cleanup_started  | Veri saklama ilkesi başladığında tablo işlemini Temizleme işlemi başladığında gerçekleşir. |
| data_retention_cleanup_exception  | Bekletme ilkesi olan tablonun Temizleme işlemi başarısız olur. |
| data_retention_cleanup_completed  | Veri saklama ilkesi sona erdiğinde tablo işlemini Temizleme işlemi tamamlandığında gerçekleşir. |


## <a name="next-steps"></a>Sonraki Adımlar
- [Veri bekletme Ilkesi](data-retention-overview.md)
- [Veri saklama Ilkelerini etkinleştirme ve devre dışı bırakma](data-retention-enable-disable.md)
