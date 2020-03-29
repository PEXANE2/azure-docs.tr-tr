---
title: Azure Veri Fabrikası Kopyalama Etkinliğinden depolanan yordamı çağırma
description: Azure Veri Fabrikası kopyalama etkinliğinden Azure SQL Veritabanı'nda veya SQL Server'da depolanan yordamı nasıl çağırın öğrenin.
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
ms.openlocfilehash: d05c2b03a0c498144f37c9b6205053120a596b09
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74924091"
---
# <a name="invoke-stored-procedure-from-copy-activity-in-azure-data-factory"></a>Azure Veri Fabrikası'ndaki kopyalama etkinliğinden depolanan yordamı çağırma
> [!NOTE]
> Bu makale, Data Factory’nin 1. sürümü için geçerlidir. Veri Fabrikası hizmetinin geçerli sürümünü kullanıyorsanız, [Veri Fabrikası'nda depolanan yordam etkinliğini kullanarak verileri dönüştürme bölümüne](../transform-data-using-stored-procedure.md)bakın.


Verileri [SQL Server](data-factory-sqlserver-connector.md) veya [Azure SQL Veritabanı'na](data-factory-azure-sql-connector.md)kopyalarken, **sqlsink'i** kopya etkinliğinde depolanan yordamı çağırmak için yapılandırabilirsiniz. Veri yi hedef tabloya eklemeden önce herhangi bir ek işlem (sütunları birleştirme, değerleri arama, birden çok tabloya ekleme vb.) gerçekleştirmek için depolanan yordamı kullanmak isteyebilirsiniz. Bu [özellik, Tablo Değeri Nene Parametrelerden](https://msdn.microsoft.com/library/bb675163.aspx)yararlanır. 

Aşağıdaki örnek, Bir Veri Fabrikası ardışık kaynaktan (kopyalama etkinliği) bir SQL Server veritabanında depolanan yordamı nasıl çağırılanın gösterir:  

## <a name="output-dataset-json"></a>Çıktı veri kümesi JSON
Çıktı veri kümesi JSON'da, **türü** şu şekilde ayarlayın: **SqlServerTable**. Azure Sql veritabanıyla kullanmak üzere **AzureSqlTable** olarak ayarlayın. **tabloName** özelliğinin değeri, depolanan yordamın ilk parametresinin adıyla eşleşmelidir.  

```json
{
  "name": "SqlOutput",
  "properties": {
    "type": "SqlServerTable",
    "linkedServiceName": "SqlLinkedService",
    "typeProperties": {
      "tableName": "Marketing"
    },
    "availability": {
      "frequency": "Hour",
      "interval": 1
    }
  }
}
```

## <a name="sqlsink-section-in-copy-activity-json"></a>Kopyalama etkinliğinde SqlSink bölümü JSON
JSON kopyalama **etkinliğindeki SqlSink** bölümünü aşağıdaki gibi tanımlayın. Lavabo/hedef veritabanına veri eklerken depolanan yordamı çağırmak için hem **SqlWriterStoredProcedureName** hem de **SqlWriterTableType** özellikleri için değerler belirtin. Bu özelliklerin açıklamaları için [SQL Server bağlayıcısı makalesindeki SqlSink bölümüne](data-factory-sqlserver-connector.md#sqlsink)bakın.

```json
"sink":
{
    "type": "SqlSink",
    "SqlWriterTableType": "MarketingType",
    "SqlWriterStoredProcedureName": "spOverwriteMarketing", 
    "storedProcedureParameters":
            {
                "stringData": 
                {
                    "value": "str1"     
                }
            }
}
```

## <a name="stored-procedure-definition"></a>Depolanan yordam tanımı 
Veritabanınızda, **sqlwriterstoredProcedurename**ile aynı ada sahip depolanan yordamı tanımlayın. Depolanan yordam, kaynak veri deposundan gelen giriş verilerini işler ve verileri hedef veritabanındaki bir tabloya ekler. Saklanan yordamın ilk parametresinin adı, JSON (Pazarlama) veri kümesinde tanımlanan tablo Adı ile eşleşmelidir.

```sql
CREATE PROCEDURE spOverwriteMarketing @Marketing [dbo].[MarketingType] READONLY, @stringData varchar(256)
AS
BEGIN
    DELETE FROM [dbo].[Marketing] where ProfileID = @stringData
    INSERT [dbo].[Marketing](ProfileID, State)
    SELECT * FROM @Marketing
END
```

## <a name="table-type-definition"></a>Tablo türü tanımı
Veritabanınızda, **SqlWriterTableType**ile aynı ada sahip tablo türünü tanımlayın. Tablo türünün şeması, giriş veri kümesinin şemasıyla eşleşmelidir.

```sql
CREATE TYPE [dbo].[MarketingType] AS TABLE(
    [ProfileID] [varchar](256) NOT NULL,
    [State] [varchar](256) NOT NULL
)
```

## <a name="next-steps"></a>Sonraki adımlar
Tam JSON örnekleri için aşağıdaki bağlayıcı makaleleri inceleyin: 

- [Azure SQL Veritabanı](data-factory-azure-sql-connector.md)
- [SQL Server](data-factory-sqlserver-connector.md)
