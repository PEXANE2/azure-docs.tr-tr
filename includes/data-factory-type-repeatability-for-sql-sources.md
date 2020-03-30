---
author: linda33wj
ms.service: data-factory
ms.topic: include
ms.date: 11/09/2018
ms.author: jingwang
ms.openlocfilehash: 24bb7a1fcb1569922fb34034fb3c0d003cdd7061
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "67188838"
---
## <a name="repeatability-during-copy"></a>Kopyalama sırasında tekrarlanabilirlik
Verileri diğer veri depolarından Azure SQL/SQL Server'a kopyalarken istenmeyen sonuçlardan kaçınmak için tekrarlanabilirliği göz önünde bulundurmak gerekir. 

Verileri Azure SQL/SQL Server Veritabanı'na kopyalarken, kopyalama etkinliği varsayılan olarak veri kümesini varsayılan olarak lavabo tablosuna ekler. Örneğin, iki kayıt içeren bir CSV (virgülle ayrılmış değerler verileri) dosya kaynağından Azure SQL/SQL Server Veritabanı'na veri kopyalanırken, tablo şu şekilde görünür:

```
ID    Product        Quantity    ModifiedDate
...    ...            ...            ...
6    Flat Washer    3            2015-05-01 00:00:00
7     Down Tube    2            2015-05-01 00:00:00
```

Kaynak dosyada hatalar bulduğunuzu ve Kaynak dosyadaki Down Tube miktarını 2'den 4'e kadar güncelleştirdiğinizi varsayalım. Bu döneme ait veri dilimini yeniden çalıştıran tutarsanız, Azure SQL/SQL Server Veritabanı'na eklenen iki yeni kayıt bulursunuz. Aşağıdaki tablodaki sütunların hiçbirinde birincil anahtar kısıtlaması olduğunu varsayar.

```
ID    Product        Quantity    ModifiedDate
...    ...            ...            ...
6    Flat Washer    3            2015-05-01 00:00:00
7     Down Tube    2            2015-05-01 00:00:00
6    Flat Washer    3            2015-05-01 00:00:00
7     Down Tube    4            2015-05-01 00:00:00
```

Bunu önlemek için, aşağıda belirtilen 2 mekanizmadan birini yararlanarak UPSERT semantiklerini belirtmeniz gerekir.

> [!NOTE]
> Bir dilim, belirtilen yeniden deneme ilkesine göre Azure Veri Fabrikası'nda otomatik olarak yeniden çalıştırılabilir.
> 
> 

### <a name="mechanism-1"></a>Mekanizma 1
Bir dilim çalıştırıldığında ilk temizleme eylemi gerçekleştirmek için **sqlWriterCleanupScript** özelliğikaldıraç olabilir. 

```json
"sink":  
{ 
  "type": "SqlSink", 
  "sqlWriterCleanupScript": "$$Text.Format('DELETE FROM table WHERE ModifiedDate >= \\'{0:yyyy-MM-dd HH:mm}\\' AND ModifiedDate < \\'{1:yyyy-MM-dd HH:mm}\\'', WindowStart, WindowEnd)"
}
```

Temizleme komut dosyası, belirli bir dilimin kopyalanması sırasında ilk olarak, bu dilime karşılık gelen SQL Tablosundaki verileri silecek şekilde yürütülür. Etkinlik daha sonra verileri SQL Tablosuna ekler. 

Dilim şimdi yeniden çalıştırılırsa, miktarın istenilen şekilde güncelleştirildiği göreceksiniz.

```
ID    Product        Quantity    ModifiedDate
...    ...            ...            ...
6    Flat Washer    3            2015-05-01 00:00:00
7     Down Tube    4            2015-05-01 00:00:00
```

Düz Yıkayılabilir kaydının orijinal csv'den kaldırıldığını varsayalım. Daha sonra dilimin yeniden çalıştırılması aşağıdaki sonucu doğuracak: 

```
ID    Product        Quantity    ModifiedDate
...    ...            ...            ...
7     Down Tube    4            2015-05-01 00:00:00
```
Yeni bir şey yapılmamagerek yoktu. Kopyalama etkinliği, o dilimiçin karşılık gelen verileri silmek için temizleme komut dosyasını çalıştırdı. Daha sonra csv 'den gelen girişi okudu (daha sonra sadece 1 kayıt içeriyordu) ve Tablo'ya ekledi. 

### <a name="mechanism-2"></a>Mekanizma 2
> [!IMPORTANT]
> sliceIdentifierColumnName şu anda Azure SQL Veri Ambarı için desteklenmez. 

Tekrarlanabilirliği sağlamak için başka bir mekanizma hedef Tablo'da özel bir sütun **(sliceIdentifierColumnName)** sahip olmaktır. Bu sütun, kaynak ve hedefin eşitlenmiş kalmasını sağlamak için Azure Veri Fabrikası tarafından kullanılır. Bu yaklaşım, hedef SQL Tablo şemasını değiştirme veya tanımlamada esneklik olduğunda çalışır. 

Bu sütun, Azure Veri Fabrikası tarafından tekrarlanabilirlik amacıyla kullanılır ve bu süreçte Azure Veri Fabrikası Tabloda herhangi bir şema değişikliği yapmaz. Bu yaklaşımı kullanmanın yolu:

1. Hedef SQL Tablosu'nda tür ikili (32) bir sütun tanımlayın. Bu sütunda kısıtlama olmamalıdır. Bu sütunu bu örnek için 'ColumnForADFuseOnly' olarak adlandıralım.
2. Kopyalama etkinliğinde aşağıdaki gibi kullanın:
   
    ```json
    "sink":  
    { 
   
        "type": "SqlSink", 
        "sliceIdentifierColumnName": "ColumnForADFuseOnly"
    }
    ```

Azure Veri Fabrikası, kaynağın ve hedefin eşitlenmiş kalmasını sağlamak için bu sütunu gereksinimine göre dolduracaktır. Bu sütunun değerleri kullanıcı tarafından bu bağlam ın dışında kullanılmamalıdır. 

Mekanizma 1'e benzer şekilde, Kopyalama Etkinliği önce hedef SQL Tablosundan verilen dilime ait verileri otomatik olarak temizler ve ardından verileri o dilimiçin kaynaktan hedefe eklemek için normal olarak kopyalama etkinliğini çalıştırır. 

