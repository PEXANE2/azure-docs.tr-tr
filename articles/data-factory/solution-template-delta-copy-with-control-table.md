---
title: Denetim tablosunu kullanarak veritabanından delta kopyası
description: Yeni veya güncelleştirilmiş satırları yalnızca Azure Veri Fabrikası'na sahip bir veritabanından artımlı olarak kopyalamak için bir çözüm şablonu nasıl kullanacağınızı öğrenin.
services: data-factory
documentationcenter: ''
author: dearandyxu
ms.author: yexu
ms.reviewer: douglasl
manager: anandsub
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 12/24/2018
ms.openlocfilehash: 01a6d796a9a8306da5bb707111b07786136a66cc
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81414780"
---
# <a name="delta-copy-from-a-database-with-a-control-table"></a>Denetim tablosu olan bir veritabanından delta kopyası
[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Bu makalede, yüksek filigran değeri depolayan bir dış denetim tablosu kullanarak veritabanı tablosundan Azure'a yeni veya güncelleştirilmiş satırları artımlı olarak yükleyebilecek bir şablon açıklanmaktadır.

Bu şablon, kaynak veritabanının şemasını yeni veya güncelleştirilmiş satırları tanımlamak için bir zaman damgası sütunu veya artış anahtarı içerir.

>[!NOTE]
> Kaynak veritabanınızda yeni veya güncelleştirilmiş satırları tanımlamak için bir zaman damgası sütununvarsa ancak delta kopya için kullanılacak bir dış denetim tablosu oluşturmak istemiyorsanız, bunun yerine bir ardışık kaynak oluşturmak için [Azure Veri Fabrikası Kopya Veri aracını](copy-data-tool.md) kullanabilirsiniz. Bu araç, kaynak veritabanından yeni satırları okumak için bir tetikleyici zamanlanmış zamanlanmış zamanlanmış zamanlanır değişken kullanır.

## <a name="about-this-solution-template"></a>Bu çözüm şablonu hakkında

Bu şablon önce eski filigran değerini alır ve geçerli filigran değeriyle karşılaştırır. Bundan sonra, iki filigran değerleri arasındaki karşılaştırmayı temel alarak yalnızca kaynak veritabanındaki değişiklikleri kopyalar. Son olarak, yeni yüksek filigran değerini bir sonraki sefere delta veri yüklemesi için harici bir denetim tablosuna saklar.

Şablon dört etkinlik içerir:
- **Arama,** harici bir denetim tablosunda depolanan eski yüksek filigran değerini alır.
- Başka bir **Arama** etkinliği, kaynak veritabanından geçerli yüksek filigran değerini alır.
- **Kopya** kopyaları yalnızca kaynak veritabanından hedef depoya değişir. Kaynak veritabanındaki değişiklikleri tanımlayan sorgu , 'SELECT * FROM Data_Source_Table WHERE TIMESTAMP_COLUMN "son yüksek filigran" > ve <= "geçerli yüksek filigran" TIMESTAMP_Column benzer.
- **SqlServerStoredProcedure,** geçerli yüksek filigran değerini bir sonraki sefere delta kopyalamak için harici bir denetim tablosuna yazar.

Şablon aşağıdaki parametreleri tanımlar:
- *Data_Source_Table_Name,* kaynak veritabanından veri yüklemek istediğiniz tablodur.
- *Data_Source_WaterMarkColumn,* kaynak tablodaki yeni veya güncelleştirilmiş satırları tanımlamak için kullanılan sütunun adıdır. Bu sütunun türü genellikle *datetime*, *INT*veya benzer.
- *Data_Destination_Container,* hedef deponuzda verilerin kopyalandığı yerin kök yoludur.
- *Data_Destination_Directory,* hedef deponuzda verilerin kopyalandığı yerin kökü altındaki dizin yoludur.
- *Data_Destination_Table_Name,* verilerin hedef mağazanızda kopyalandığı yerdir ("Azure Synapse Analytics (eski adıyla SQL DW)" Veri Hedefi olarak seçildiğinde geçerlidir).
- *Data_Destination_Folder_Path,* verilerin hedef deponuzda kopyalandığı yerdir ("Dosya Sistemi" veya "Azure Veri Gölü Depolama Gen1" Veri Hedefi olarak seçildiğinde geçerlidir).
- *Control_Table_Table_Name,* yüksek filigran değerini depolayan dış denetim tablosudur.
- *Control_Table_Column_Name,* dış denetim tablosundaki yüksek filigran değerini depolayan sütundur.

## <a name="how-to-use-this-solution-template"></a>Bu çözüm şablonu nasıl kullanılır?

1. Yüklemek istediğiniz kaynak tabloyu keşfedin ve yeni veya güncelleştirilmiş satırları tanımlamak için kullanılabilecek yüksek filigran sütunu tanımlayın. Bu sütunun türü *datetime,* *INT*veya benzeri olabilir. Yeni satırlar eklendikçe bu sütunun değeri artar. Aşağıdaki örnek kaynak tablodan (data_source_table), *LastModifytime* sütununu yüksek filigran sütunu olarak kullanabiliriz.

    ```sql
            PersonID    Name    LastModifytime
            1   aaaa    2017-09-01 00:56:00.000
            2   bbbb    2017-09-02 05:23:00.000
            3   cccc    2017-09-03 02:36:00.000
            4   dddd    2017-09-04 03:21:00.000
            5   eeee    2017-09-05 08:06:00.000
            6   fffffff 2017-09-06 02:23:00.000
            7   gggg    2017-09-07 09:01:00.000
            8   hhhh    2017-09-08 09:01:00.000
            9   iiiiiiiii   2017-09-09 09:01:00.000
    ```
    
2. Delta veri yüklemesi için yüksek filigran değerini depolamak için SQL Server veya Azure SQL Veritabanı'nda bir denetim tablosu oluşturun. Aşağıdaki örnekte, denetim tablosunun adı *filigran.* Bu tabloda, *FiligranDeğer* yüksek filigran değerini depolayan sütundur ve türü *datetime'dır.*

    ```sql
            create table watermarktable
            (
            WatermarkValue datetime,
            );
            INSERT INTO watermarktable
            VALUES ('1/1/2010 12:00:00 AM')
    ```
    
3. Denetim tablosunu oluşturmak için kullandığınız aynı SQL Server veya Azure SQL Veritabanı örneğinde depolanmış bir yordam oluşturun. Depolanan yordam, bir sonraki sefere delta veri yüklemesi için dış denetim tablosuna yeni yüksek filigran değerini yazmak için kullanılır.

    ```sql
            CREATE PROCEDURE update_watermark @LastModifiedtime datetime
            AS

            BEGIN

                UPDATE watermarktable
                SET [WatermarkValue] = @LastModifiedtime 

            END
    ```
    
4. **Veritabanı şablonundan Delta kopyasına** gidin. Veri kopyalamak istediğiniz kaynak veritabanına **yeni** bir bağlantı oluşturun.

    ![Kaynak tabloya yeni bir bağlantı oluşturma](media/solution-template-delta-copy-with-control-table/DeltaCopyfromDB_with_ControlTable4.png)

5. Verileri kopyalamak istediğiniz hedef veri deposuna **yeni** bir bağlantı oluşturun.

    ![Hedef tabloya yeni bir bağlantı oluşturma](media/solution-template-delta-copy-with-control-table/DeltaCopyfromDB_with_ControlTable5.png)

6. 2 ve 3 adımlarında oluşturduğunuz dış denetim tablosuna ve depolanmış yordama **yeni** bir bağlantı oluşturun.

    ![Denetim tablosu veri deposuna yeni bir bağlantı oluşturma](media/solution-template-delta-copy-with-control-table/DeltaCopyfromDB_with_ControlTable6.png)

7. **Bu şablonu kullan'ı**seçin.
    
8. Aşağıdaki örnekte gösterildiği gibi kullanılabilir ardışık hattı görürsünüz:
  
    ![Boru hattını gözden geçirin](media/solution-template-delta-copy-with-control-table/DeltaCopyfromDB_with_ControlTable8.png)

9. **Depolanan Yordam'ı**seçin. **Depolanan yordam adı için** **[dbo] seçeneğini belirleyin. update_watermark]**. **Aktar parametreyi**seçin ve ardından **dinamik içerik ekle'yi**seçin.  

    ![Depolanan yordam etkinliğini ayarlama](media/solution-template-delta-copy-with-control-table/DeltaCopyfromDB_with_ControlTable9.png)  

10. İçeriği ** \@yazın {activity('LookupCurrentWaterMark').output.firstRow.NewWatermarkValue}** ve ardından **Finish'i**seçin.  

    ![Depolanan yordamın parametreleri için içeriği yazın](media/solution-template-delta-copy-with-control-table/DeltaCopyfromDB_with_ControlTable10.png)       
     
11. **Hata Ayıklama'yı**seçin, **Parametreleri**girin ve ardından **Bitir'i**seçin.

    ![**Hata Ayıklama** seçeneğini seçin](media/solution-template-delta-copy-with-control-table/DeltaCopyfromDB_with_ControlTable11.png)

12. Aşağıdaki örneğe benzer sonuçlar görüntülenir:

    ![Sonucu gözden geçirin](media/solution-template-delta-copy-with-control-table/DeltaCopyfromDB_with_ControlTable12.png)

13. Kaynak tablonuzda yeni satırlar oluşturabilirsiniz. Yeni satırlar oluşturmak için örnek SQL dili aşağıda veda edebilirsiniz:

    ```sql
            INSERT INTO data_source_table
            VALUES (10, 'newdata','9/10/2017 2:23:00 AM')

            INSERT INTO data_source_table
            VALUES (11, 'newdata','9/11/2017 9:01:00 AM')
    ```

14. Ardışık bölümü yeniden çalıştırmak için **Hata Ayıklama'yı**seçin, **Parametreleri**girin ve ardından **Finish'i**seçin.

    Yalnızca yeni satırların hedefe kopyalandığını göreceksiniz.

15. (İsteğe bağlı:) Veri hedefi olarak Azure Synapse Analytics'i (eski adıyla SQL DW) seçerseniz, sql veri ambarı Polybase tarafından gerekli olan evreleme için Azure Blob depolama alanına bağlantı sağlamanız gerekir. Şablon sizin için bir kapsayıcı yolu oluşturur. Boru hattı çalıştırıldıktan sonra, kapsayıcının Blob depolamaalanında oluşturulup oluşturulmadığını kontrol edin.
    
    ![Polybase'i yapılandır](media/solution-template-delta-copy-with-control-table/DeltaCopyfromDB_with_ControlTable15.png)
    
## <a name="next-steps"></a>Sonraki adımlar

- [Azure Veri Fabrikası ile bir denetim tablosu kullanarak veritabanından toplu kopya](solution-template-bulk-copy-with-control-table.md)
- [Azure Veri Fabrikası ile birden çok kapsayıcıdaki dosyaları kopyalama](solution-template-copy-files-multiple-containers.md)
