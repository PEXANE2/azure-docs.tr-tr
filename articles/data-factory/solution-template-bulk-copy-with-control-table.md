---
title: Denetim tablosunu kullanarak veritabanından toplu kopya
description: Azure Veri Fabrikası'nı kullanarak kaynak tabloların bölüm listesini depolamak için harici bir denetim tablosu kullanarak veritabanından toplu verileri kopyalamak için çözüm şablonunun nasıl kullanılacağını öğrenin.
services: data-factory
author: dearandyxu
ms.author: yexu
ms.reviewer: douglasl
manager: anandsub
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 12/14/2018
ms.openlocfilehash: 3a42d7da21cfb2e3066fbdd81b27c82155d8456f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75439973"
---
# <a name="bulk-copy-from-a-database-with-a-control-table"></a>Denetim tablosu olan bir veritabanından toplu kopya

Oracle Server, Netezza, Teradata veya SQL Server'daki bir veri ambarından Azure SQL Veri Ambarı'na veri kopyalamak için birden çok tablodan büyük miktarda veri yüklemeniz gerekir. Genellikle, tek bir tablodan paralel olarak birden çok iş parçacığı olan satırları yükleyebilmeniz için verilerin her tabloda bölümlenmesi gerekir. Bu makalede, bu senaryolarda kullanılacak bir şablon açıklanmaktadır.

 >! NOT Göreceli olarak küçük veri hacmine sahip az sayıda tablodan verileri SQL Veri Ambarı'na kopyalamak istiyorsanız, [Azure Veri Fabrikası Kopyalama Veri aracını](copy-data-tool.md)kullanmak daha verimliolur. Bu makalede açıklanan şablon, bu senaryo için gerekenden daha fazladır.

## <a name="about-this-solution-template"></a>Bu çözüm şablonu hakkında

Bu şablon, dış denetim tablosundan kopyalanması için kaynak veritabanı bölümleri listesini alır. Daha sonra kaynak veritabanındaki her bölüm üzerinde yinelemeler ve verileri hedefe kopyalar.

Şablon üç etkinlik içerir:
- **Arama,** emin veritabanı bölümlerinin listesini harici bir denetim tablosundan alır.
- **ForEach,** Arama etkinliğinden bölüm listesini alır ve her bölümü Kopyalama etkinliğine yineler.
- Kaynak veritabanı deposundan hedef mağazaya her bölümü **kopyalayın.**

Şablon aşağıdaki parametreleri tanımlar:
- *Control_Table_Name,* kaynak veritabanıiçin bölüm listesini depolayan dış denetim tablonuzdur.
- *Control_Table_Schema_PartitionID,* dış denetim tablonuzdaki her bölüm kimliğini depolayan sütun adının adıdır. Bölüm kimliğinin kaynak veritabanındaki her bölüm için benzersiz olduğundan emin olun.
- *Control_Table_Schema_SourceTableName,* her tablo adını kaynak veritabanından depolayan dış denetim tablonuzdur.
- *Control_Table_Schema_FilterQuery,* kaynak veritabanındaki her bölümden verileri almak için filtre sorgusunu depolayan dış denetim tablonuzdaki sütunun adıdır. Örneğin, verileri yıla göre bölümlediyseniz, her satırda depolanan sorgu , LastModifytime >= ''2015-01 00:00'' ve LastModifytime <= ''2015-12-31 23:59:59.999'' veri kaynağından 'select *'a benzer olabilir.
- *Data_Destination_Folder_Path,* verilerin hedef deponuza kopyalandığı yoldur (seçtiğiniz hedef "Dosya Sistemi" veya "Azure Veri Gölü Depolama Gen1" olduğunda geçerlidir). 
- *Data_Destination_Container,* hedef deponuzda verilerin kopyalandığı kök klasör yoludur. 
- *Data_Destination_Directory,* verilerin hedef deponuza kopyalandığı kök altındaki dizin yoludur. 

Hedef deponuzdaki yolu tanımlayan son üç parametre yalnızca seçtiğiniz hedef dosya tabanlı depolama ysa görünür. Hedef mağaza olarak "Azure Synapse Analytics (eski adıyla SQL DW)" seçeneğini seçerseniz, bu parametreler gerekli değildir. Ancak SQL Veri Ambarı'ndaki tablo adları ve şema, kaynak veritabanındakilerle aynı olmalıdır.

## <a name="how-to-use-this-solution-template"></a>Bu çözüm şablonu nasıl kullanılır?

1. Kaynak veritabanı bölüm listesini toplu kopya için depolamak için SQL Server veya Azure SQL Veritabanı'nda bir denetim tablosu oluşturun. Aşağıdaki örnekte, kaynak veritabanında beş bölüm vardır. Üç bölüm *datasource_table*içindir ve iki *project_table*içindir. *LastModifytime* sütunu, *tablodaki* verileri kaynak veritabanından datasource_table bölmek için kullanılır. İlk bölümü okumak için kullanılan sorgu 'select * from datasource_table lastModifytime >= ''2015-01-01 00:00'' ve LastModifytime <= ''2015-12-31 23:59:59.999''. Benzer bir sorguyı diğer bölümlerden gelen verileri okumak için kullanabilirsiniz.

     ```sql
            Create table ControlTableForTemplate
            (
            PartitionID int,
            SourceTableName  varchar(255),
            FilterQuery varchar(255)
            );

            INSERT INTO ControlTableForTemplate
            (PartitionID, SourceTableName, FilterQuery)
            VALUES
            (1, 'datasource_table','select * from datasource_table where LastModifytime >= ''2015-01-01 00:00:00'' and LastModifytime <= ''2015-12-31 23:59:59.999'''),
            (2, 'datasource_table','select * from datasource_table where LastModifytime >= ''2016-01-01 00:00:00'' and LastModifytime <= ''2016-12-31 23:59:59.999'''),
            (3, 'datasource_table','select * from datasource_table where LastModifytime >= ''2017-01-01 00:00:00'' and LastModifytime <= ''2017-12-31 23:59:59.999'''),
            (4, 'project_table','select * from project_table where ID >= 0 and ID < 1000'),
            (5, 'project_table','select * from project_table where ID >= 1000 and ID < 2000');
    ```

2. **Veritabanı şablonundan Toplu Kopya'ya** gidin. Adım 1'de oluşturduğunuz dış denetim tablosuna **yeni** bir bağlantı oluşturun.

    ![Denetim tablosuna yeni bir bağlantı oluşturma](media/solution-template-bulk-copy-with-control-table/BulkCopyfromDB_with_ControlTable2.png)

3. Verileri kopyaladığınız kaynak veritabanına **yeni** bir bağlantı oluşturun.

    ![Kaynak veritabanına yeni bir bağlantı oluşturma](media/solution-template-bulk-copy-with-control-table/BulkCopyfromDB_with_ControlTable3.png)
    
4. Verileri kopyaladığınız hedef veri deposuna **yeni** bir bağlantı oluşturun.

    ![Hedef mağazaya yeni bir bağlantı oluşturma](media/solution-template-bulk-copy-with-control-table/BulkCopyfromDB_with_ControlTable4.png)

5. **Bu şablonu kullan'ı**seçin.

6. Aşağıdaki örnekte gösterildiği gibi, ardışık hattı görüyorsunuz:

    ![Boru hattını gözden geçirin](media/solution-template-bulk-copy-with-control-table/BulkCopyfromDB_with_ControlTable6.png)

7. **Hata Ayıklama'yı**seçin, **Parametreleri**girin ve ardından **Bitir'i**seçin.

    ![**Hata Ayıklama** seçeneğini tıklatın](media/solution-template-bulk-copy-with-control-table/BulkCopyfromDB_with_ControlTable7.png)

8. Aşağıdaki örneğe benzer sonuçlar görürsünüz:

    ![Sonucu gözden geçirin](media/solution-template-bulk-copy-with-control-table/BulkCopyfromDB_with_ControlTable8.png)

9. (İsteğe bağlı) Veri hedefi olarak "Azure Synapse Analytics (eski adıyla SQL DW)" seçtiyseniz, SQL Data Warehouse Polybase tarafından gerekli olduğu gibi evreleme için Azure Blob depolama alanına bir bağlantı girmeniz gerekir. Şablon, Blob depolama alanınız için otomatik olarak bir kapsayıcı yolu oluşturur. Kapsayıcının boru hattı çalıştırıldıktan sonra oluşturulıp oluşturulmamasını denetleyin.
    
    ![Polybase ayarı](media/solution-template-bulk-copy-with-control-table/BulkCopyfromDB_with_ControlTable9.png)
       
## <a name="next-steps"></a>Sonraki adımlar

- [Azure Data Factory'ye giriş](introduction.md)
