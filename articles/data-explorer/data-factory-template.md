---
title: Azure Data Factory şablonunu kullanarak bir veritabanından Azure Veri Gezgini toplu olarak kopyalama
description: Bu makalede, bir veritabanından Azure 'a toplu olarak kopyalamak için bir Azure Data Factory şablonu kullanmayı öğreneceksiniz Veri Gezgini
services: data-explorer
author: orspod
ms.author: orspodek
ms.reviewer: tzgitlin
ms.service: data-explorer
ms.topic: conceptual
ms.date: 09/08/2019
ms.openlocfilehash: 884f4e956b37c2def6c25d0acdf20f15eddf7767
ms.sourcegitcommit: 7221918fbe5385ceccf39dff9dd5a3817a0bd807
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/21/2020
ms.locfileid: "76293564"
---
# <a name="copy-in-bulk-from-a-database-to-azure-data-explorer-by-using-the-azure-data-factory-template"></a>Azure Data Factory şablonunu kullanarak bir veritabanından Azure Veri Gezgini toplu olarak kopyalama 

Azure Veri Gezgini, hızlı, tam olarak yönetilen bir veri analizi hizmetidir. Uygulamalar, Web siteleri ve IoT cihazları gibi birçok kaynaktan akış yapan büyük birimlerde gerçek zamanlı analizler sunar. 

Oracle Server, Netezza, Teradata veya SQL Server içindeki bir veritabanından verileri Azure Veri Gezgini kopyalamak için, birden çok tablodan çok büyük miktarlarda veri yüklemeniz gerekir. Genellikle verilerin her tabloda bölümlenmesi gerekir, böylece birden çok iş parçacığı içeren satırları tek bir tablodan paralel olarak yükleyebilirsiniz. Bu makalede, bu senaryolarda kullanılacak bir şablon açıklanmaktadır.

[Azure Data Factory şablonlar](/azure/data-factory/solution-templates-introduction) , işlem hatları Data Factory önceden tanımlanmıştır. Bu şablonlar, Data Factory hızlı bir şekilde başlamanıza ve veri tümleştirme projelerinde geliştirme süresini azaltmanıza yardımcı olabilir. 

Toplu kopyalamayı, *arama* ve *foreach* etkinliklerini kullanarak *veritabanından Azure Veri Gezgini* şablonuna oluşturursunuz. Daha hızlı veri kopyalama için şablonu, veritabanı veya tablo başına birçok işlem hattı oluşturmak üzere kullanabilirsiniz. 

> [!IMPORTANT]
> Kopyalamak istediğiniz veri miktarı için uygun olan aracı kullandığınızdan emin olun.
> * SQL Server ve Google BigQuery gibi veritabanlarından büyük miktarda veriyi Azure Veri Gezgini kopyalamak için *veritabanından toplu kopyalama ' yı azure Veri Gezgini* şablonu ' na kullanın. 
> * Küçük veya orta miktarda veri içeren birkaç tabloyu Azure Veri Gezgini 'e kopyalamak için [*Data Factory veri kopyalama aracını*](data-factory-load-data.md) kullanın. 

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir Azure hesabı](https://azure.microsoft.com/free/) oluşturun.
* [Azure Veri Gezgini kümesi ve veritabanı](create-cluster-database-portal.md).
* [Bir veri fabrikası oluşturun](data-factory-load-data.md#create-a-data-factory).
* Bir veritabanındaki veri kaynağı.

## <a name="create-controltabledataset"></a>ControlTableDataset oluşturma

*Controltabledataset* , kaynaktan işlem hattındaki hedefe hangi verilerin kopyalanacağını gösterir. Satır sayısı, verileri kopyalamak için gereken toplam işlem hattı sayısını gösterir. Denetimtabloveri kümesini kaynak veritabanının bir parçası olarak tanımlamanız gerekir.

Aşağıdaki kodda SQL Server kaynak tablo biçimine bir örnek gösterilmektedir:
    
```sql   
CREATE TABLE control_table (
PartitionId int,
SourceQuery varchar(255),
ADXTableName varchar(255)
);
```

Kod öğeleri aşağıdaki tabloda açıklanmıştır:

|Özellik  |Açıklama  | Örnek
|---------|---------| ---------|
|PartitionId   |  Kopyalama sırası | 1  |  
|SourceQuery   |  Ardışık düzen çalışma zamanı sırasında hangi verilerin kopyalanacağını belirten sorgu | <br>`select * from table where lastmodifiedtime  LastModifytime >= ''2015-01-01 00:00:00''>` </br>    
|ADXTableName  |  Hedef tablo adı | MyAdxTable       |  

ControlTableDataset, farklı bir biçimde ise, biçimlendirmeniz için karşılaştırılabilir bir ControlTableDataset oluşturun.

## <a name="use-the-bulk-copy-from-database-to-azure-data-explorer-template"></a>Veritabanından Azure Veri Gezgini şablonuna toplu kopyalama kullanma

1. **Haydi** başlayın bölmesinde **Şablon Galerisi** bölmesini açmak Için **şablondan işlem hattı oluştur** ' u seçin.

    ![Azure Data Factory "Haydi başlayalım" bölmesi](media/data-factory-template/adf-get-started.png)

1. **Veritabanından Azure Veri Gezgini** şablonu ' na toplu kopyalama ' yı seçin.
 
    !["Veritabanından Azure 'a toplu kopyalama Veri Gezgini" şablonu](media/data-factory-template/pipeline-from-template.png)

1.  **Veritabanından Azure 'A toplu kopyalama veri Gezgini** bölmesinde, **Kullanıcı girişleri**altında aşağıdakileri yaparak veri kümelerinizi belirtin: 

    a. **Controltabledataset** aşağı açılan listesinde, kaynaktan hedefe hangi verilerin kopyalanacağını ve hedefin hedefe yerleştirileceğini belirten denetim tablosuna bağlı hizmeti seçin. 

    b. **SourceDataset** açılan listesinde, kaynak veritabanına bağlı hizmeti seçin. 

    c. **AzureDataExplorerTable** açılır listesinden Azure Veri Gezgini tablosunu seçin. Veri kümesi yoksa, veri kümesini eklemek için [Azure Veri Gezgini bağlı hizmetini oluşturun](data-factory-load-data.md#create-the-azure-data-explorer-linked-service) .

    d. **Bu şablonu kullan**'ı seçin.

    !["Veritabanından Azure 'a toplu kopyalama Veri Gezgini" bölmesi](media/data-factory-template/configure-bulk-copy-adx-template.png)

1. Şablon ardışık düzenine erişmek için, etkinliklerin dışında tuvalde bir alan seçin. **Ad** (denetim tablosu adı) ve **varsayılan değer** (sütun adları) dahil olmak üzere tablonun parametrelerini girmek için **Parametreler** sekmesini seçin.

    ![İşlem hattı parametreleri](media/data-factory-template/pipeline-parameters.png)

1.  **Ara**altında, varsayılan ayarları görüntülemek Için **getpartitionlist** ' i seçin. Sorgu otomatik olarak oluşturulur.
1.  Komut etkinliğini seçin, **ForEachPartition**, **Ayarlar** sekmesini seçin ve ardından aşağıdakileri yapın:

    a. **Yığın sayısı** kutusuna 1 ile 50 arasında bir sayı girin. Bu seçim, *Controltabledataset* satırı sayısına ulaşıncaya kadar paralel olarak çalışan işlem hattı sayısını belirler. 

    b. Ardışık düzen toplu işlemlerinin paralel olarak çalışmasını sağlamak için **sıralı** onay *kutusunu seçmeyin.*

    ![ForEachPartition ayarları](media/data-factory-template/foreach-partition-settings.png)

    > [!TIP]
    > Verilerinizin daha hızlı bir şekilde kopyalanabilmesi için en iyi yöntem, paralel olarak birçok işlem hattı çalıştırmalıdır. Verimliliği artırmak için, kaynak tablodaki verileri bölümleyip tarih ve tabloya göre işlem hattı başına bir bölüm ayırın.

1. Azure Data Factory işlem hattını doğrulamak için **Tümünü doğrula** ' yı seçin ve ardından Işlem **hattı doğrulama çıkış** bölmesinde sonucu görüntüleyin.

    ![Şablon işlem hatlarını doğrulama](media/data-factory-template/validate-template-pipelines.png)

1. Gerekirse, **Hata Ayıkla**' yı seçin ve ardından işlem hattını çalıştırmak Için **tetikleyici Ekle** ' yi seçin.

    !["Hata ayıkla" ve "işlem hattı çalıştırma" düğmeleri](media/data-factory-template/trigger-run-of-pipeline.png)    

Artık, veritabanlarınızdan ve tablolardan büyük miktarlarda veriyi etkin bir şekilde kopyalamak için şablonu kullanabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Data Factory kullanarak verileri Azure Veri Gezgini kopyalama](data-factory-load-data.md)hakkında bilgi edinin.
* Azure Data Factory [Azure Veri Gezgini Bağlayıcısı](/azure/data-factory/connector-azure-data-explorer) hakkında bilgi edinin.
* Veri sorgulama için [Azure Veri Gezgini sorguları](/azure/data-explorer/web-query-data) hakkında bilgi edinin.






