---
title: Azure Veri Fabrikası'nda yinelenebilir kopya
description: Verileri kopyalayan bir dilim birden fazla kez çalıştırılabilse bile yinelemelerden nasıl kaçınarak önleyebilirsiniz öğrenin.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: shwang
editor: ''
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 01/10/2018
ms.author: jingwang
robots: noindex
ms.openlocfilehash: 7188cb5774699fc6e31fc3b8c78068bb33c6f552
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79281151"
---
# <a name="repeatable-copy-in-azure-data-factory"></a>Azure Veri Fabrikası'nda yinelenebilir kopya

## <a name="repeatable-read-from-relational-sources"></a>İlişkisel kaynaklardan tekrarlanabilir okuma
İlişkisel veri depolarından veri kopyalarken, istenmeyen sonuçlardan kaçınmak için tekrarlanabilirliği aklınızda bulundurun. Azure Veri Fabrikası'nda, bir dilimi el ile yeniden çalıştırabilirsiniz. Bir hata oluştuğunda dilimin yeniden çalıştırılaması için bir veri kümesi için yeniden deneme ilkesini de yapılandırabilirsiniz. Bir dilim her iki şekilde de yeniden çalıştırıldığında, bir dilim kaç kez çalıştırılırsa çalıştırın aynı verilerin okunduğundan emin olmanız gerekir.  
 
> [!NOTE]
> Aşağıdaki örnekler Azure SQL içindir, ancak dikdörtgen veri kümelerini destekleyen tüm veri depoları için geçerlidir. Veri deposu için kaynak **türünü** ve **sorgu** özelliğini (örneğin: sqlReaderQuery yerine sorgu) ayarlamanız gerekebilir.   

Genellikle, ilişkisel mağazalardan okurken, yalnızca bu dilime karşılık gelen verileri okumak istersiniz. Bunu yapmanın bir yolu, Azure Veri Fabrikası'nda bulunan WindowStart ve WindowEnd sistem değişkenlerini kullanmak olacaktır. Azure Veri Fabrikası'ndaki değişkenler ve işlevler hakkında bilgi buradan Azure Veri Fabrikası - [Fonksiyonlar ve Sistem Değişkenleri](data-factory-functions-variables.md) makalesinde. Örnek: 

```json
"source": {
    "type": "SqlSource",
    "sqlReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd HH:mm\\' AND timestampcolumn < \\'{1:yyyy-MM-dd HH:mm\\'', WindowStart, WindowEnd)"
},
```
Bu sorgu, MyTable tablosundan dilim süresi aralığında (WindowStart -> WindowEnd) düşen verileri okur. Bu dilimin yeniden çalıştırılMası da her zaman aynı verilerin okunmasını sağlar. 

Diğer durumlarda, tablonun tamamını okumak isteyebilirsiniz ve sqlReaderQuery'yi aşağıdaki gibi tanımlayabilirsiniz:

```json
"source": 
{            
    "type": "SqlSource",
    "sqlReaderQuery": "select * from MyTable"
},
```

## <a name="repeatable-write-to-sqlsink"></a>SqlSink'e tekrarlanabilir yazma
Verileri diğer veri depolarından **Azure SQL/SQL Server'a** kopyalarken, istenmeyen sonuçlardan kaçınmak için tekrarlanabilirliği aklınızda tutmanız gerekir. 

Verileri Azure SQL/SQL Server Veritabanı'na kopyalarken, kopyalama etkinliği varsayılan olarak verileri lavabo tablosuna ekler. Azure SQL/SQL Server Veritabanı'nda iki kayıt içeren bir CSV (virgülden ayrılmış değerler) dosyasındaki verileri aşağıdaki tabloya kopyaladığınızı varsa. Bir dilim çalıştığında, iki kayıt SQL tablosuna kopyalanır. 

```
ID    Product        Quantity    ModifiedDate
...    ...            ...            ...
6    Flat Washer    3            2015-05-01 00:00:00
7     Down Tube    2            2015-05-01 00:00:00
```

Kaynak dosyada hatalar bulduğunuzu ve Down Tube miktarını 2'den 4'e güncelleştirdiğinizi varsayalım. Bu döneme ait veri dilimini el ile yeniden çalıştıracaksanız, Azure SQL/SQL Server Veritabanına eklenen iki yeni kayıt bulursunuz. Bu örnek, tablodaki sütunların hiçbirinde birincil anahtar kısıtlaması olmadığını varsayar.

```
ID    Product        Quantity    ModifiedDate
...    ...            ...            ...
6    Flat Washer    3            2015-05-01 00:00:00
7     Down Tube    2            2015-05-01 00:00:00
6    Flat Washer    3            2015-05-01 00:00:00
7     Down Tube    4            2015-05-01 00:00:00
```

Bu davranışı önlemek için, aşağıdaki iki mekanizmadan birini kullanarak UPSERT semantiklerini belirtmeniz gerekir:

### <a name="mechanism-1-using-sqlwritercleanupscript"></a>Mekanizma 1: sqlWriterCleanupScript kullanarak
Bir dilim çalıştırıldığında verileri eklemeden önce lavabo tablosundaki verileri temizlemek için **sqlWriterCleanupScript** özelliğini kullanabilirsiniz. 

```json
"sink":  
{ 
  "type": "SqlSink", 
  "sqlWriterCleanupScript": "$$Text.Format('DELETE FROM table WHERE ModifiedDate >= \\'{0:yyyy-MM-dd HH:mm}\\' AND ModifiedDate < \\'{1:yyyy-MM-dd HH:mm}\\'', WindowStart, WindowEnd)"
}
```

Bir dilim çalıştığında, temizleme komut dosyası önce SQL tablosundan dilime karşılık gelen verileri silmek için çalıştırılır. Kopyalama etkinliği daha sonra verileri SQL Tablosuna ekler. Dilim yeniden çalıştırılırsa, miktar istenildiği gibi güncelleştirilir.

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

Kopyalama etkinliği, o dilimiçin karşılık gelen verileri silmek için temizleme komut dosyasını çalıştırdı. Daha sonra csv 'den gelen girişi okudu (daha sonra sadece bir kayıt içeriyordu) ve Tablo'ya ekledi. 

### <a name="mechanism-2-using-sliceidentifiercolumnname"></a>Mekanizma 2: sliceIdentifierColumnName kullanarak
> [!IMPORTANT]
> Şu anda sliceIdentifierColumnName Azure SQL Veri Ambarı için desteklenmez. 

Tekrarlanabilirliği sağlamak için ikinci mekanizma, hedef Tablo'da özel bir sütuna (sliceIdentifierColumnName) sahip olmaktır. Bu sütun, kaynak ve hedefin eşitlenmiş kalmasını sağlamak için Azure Veri Fabrikası tarafından kullanılır. Bu yaklaşım, hedef SQL Tablo şemasını değiştirme veya tanımlamada esneklik olduğunda çalışır. 

Bu sütun, Azure Veri Fabrikası tarafından tekrarlanabilirlik amacıyla kullanılır ve bu süreçte Azure Veri Fabrikası Tabloda herhangi bir şema değişikliği yapmaz. Bu yaklaşımı kullanmanın yolu:

1. Hedef SQL Tablosu'nda tür **ikili (32)** bir sütun tanımlayın. Bu sütunda kısıtlama olmamalıdır. Bu sütunu bu örnek için AdfSliceIdentifier olarak adlandıralım.


    Kaynak tablo:

    ```sql
    CREATE TABLE [dbo].[Student](
       [Id] [varchar](32) NOT NULL,
       [Name] [nvarchar](256) NOT NULL
    )
    ```

    Hedef tablo: 

    ```sql
    CREATE TABLE [dbo].[Student](
       [Id] [varchar](32) NOT NULL,
       [Name] [nvarchar](256) NOT NULL,
       [AdfSliceIdentifier] [binary](32) NULL
    )
    ```

1. Kopyalama etkinliğinde aşağıdaki gibi kullanın:
   
    ```json
    "sink":  
    { 
   
        "type": "SqlSink", 
        "sliceIdentifierColumnName": "AdfSliceIdentifier"
    }
    ```

Azure Veri Fabrikası, kaynak ve hedefin eşitlenmiş kalmasını sağlamak için bu sütunu gereksinimine göre dolduruyor. Bu sütunun değerleri bu bağlamın dışında kullanılmamalıdır. 

Mekanizma 1'e benzer şekilde, Kopyalama Etkinliği, verilen dilime ait verileri hedef SQL Tablosundan otomatik olarak temizler. Daha sonra kaynaktan hedef tabloya veri ekler. 

## <a name="next-steps"></a>Sonraki adımlar
Tam JSON örnekleri için aşağıdaki bağlayıcı makaleleri inceleyin: 

- [Azure SQL Veritabanı](data-factory-azure-sql-connector.md)
- [Azure SQL Veri Ambarı](data-factory-azure-sql-data-warehouse-connector.md)
- [SQL Server](data-factory-sqlserver-connector.md)
