---
title: Veritabanından Azure 'a toplu kopyalama için Azure Data Factory şablonu kullanın Veri Gezgini
description: Bu konu başlığında, veritabanından Azure 'a toplu kopyalama için bir Azure Data Factory şablonu kullanmayı öğreneceksiniz Veri Gezgini
services: data-explorer
author: orspod
ms.author: orspodek
ms.reviewer: tzgitlin
ms.service: data-explorer
ms.topic: conceptual
ms.date: 09/08/2019
ms.openlocfilehash: 5a6aebd276ef8658da9ca763be7da5c38a9c772a
ms.sourcegitcommit: 23389df08a9f4cab1f3bb0f474c0e5ba31923f12
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/10/2019
ms.locfileid: "70873424"
---
# <a name="use-azure-data-factory-template-for-bulk-copy-from-database-to-azure-data-explorer"></a>Veritabanından Azure 'a toplu kopyalama için Azure Data Factory şablonu kullanın Veri Gezgini

Azure Veri Gezgini, uygulamalar, Web siteleri ve IoT cihazları gibi birçok kaynaktan büyük miktarlarda veri akışı için gerçek zamanlı analizler için hızlı ve tam olarak yönetilen bir veri analizi hizmetidir. Azure Data Factory, tam olarak yönetilen bulut tabanlı bir veri tümleştirme hizmetidir. Azure Data Factory hizmetini kullanarak Azure Veri Gezgini veritabanınızı mevcut sisteminizdeki verilerle doldurabilir ve analiz çözümlerinizi oluştururken zamandan tasarruf edebilirsiniz. 

[Azure Data Factory şablonlar](/azure/data-factory/solution-templates-introduction) , Data Factory hızlı bir şekilde başlamanıza ve veri tümleştirme projeleri oluşturmaya yönelik geliştirme süresini azaltmanıza olanak sağlayan işlem hatları Azure Data Factory önceden tanımlanmıştır. **Veritabanından Azure Veri Gezgini şablonuna toplu kopyalama** , **arama** ve **foreach** etkinlikleri kullanılarak oluşturulur. Şablonu, verilerin daha hızlı kopyalanması için veritabanı veya tablo başına birçok işlem hattı oluşturmak üzere kullanabilirsiniz. 

> [!IMPORTANT]
> * SQL Server ve Google BigQuery gibi veritabanlarından büyük miktarda veriyi Azure Veri Gezgini kopyalamak için **veritabanından toplu kopyalama ' yı azure Veri Gezgini** şablonu ' na kullanın. 
> * Küçük veya orta miktarda veri içeren birkaç tabloyu Azure Veri Gezgini 'e kopyalamak için [kopyalama aracını](data-factory-load-data.md) kullanın. 

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir Azure hesabı](https://azure.microsoft.com/free/) oluşturun.
* [Azure Veri Gezgini kümesi ve veritabanı](create-cluster-database-portal.md)
* [Veri Fabrikası oluşturma](data-factory-load-data.md#create-a-data-factory)
* Veritabanındaki verilerin kaynağı

## <a name="create-controltabledataset"></a>ControlTableDataset oluşturma

**Controltabledataset** , ardışık düzende kaynaktan hedefe hangi verilerin kopyalanacağını gösterir. Satır sayısı, verileri kopyalamak için gereken toplam işlem hattı sayısını belirtir. **Controltabledataset** , kaynak veritabanının bir parçası olarak tanımlanmalıdır.

SQL Server kaynak tablo biçimi örneği:
    
```sql   
CREATE TABLE control_table (
PartitionId int,
SourceQuery varchar(255),
ADXTableName varchar(255)
);
```
    
|Özellik  |Açıklama  | Örnek
|---------|---------| ---------|
|PartitionID   |   siparişi Kopyala | 1\.  |  
|SourceQuery   |   ardışık düzen çalışma zamanı sırasında hangi verilerin kopyalanacağını belirten sorgu | <br>`select * from table where lastmodifiedtime  LastModifytime >= ''2015-01-01 00:00:00''>` </br>    
|ADXTableName  |  hedef tablo adı | MyAdxTable       |  

**Controltabledataset** , farklı bir biçimde ise, biçimlendirmeniz için karşılaştırılabilir bir **controltabledataset** oluşturun.

## <a name="use-bulk-copy-from-database-to-azure-data-explorer-template"></a>Veritabanından Azure Veri Gezgini şablonuna toplu kopyalama kullanma

1. **Başlarken** sayfasında, şablondan işlem **hattı oluştur** kutucuğunu seçin **veya şablondan sağ >** **+**  > işlem hattındaki kalem simgesini (**Yazar** sekmesi) seçerek şablon galerisini açın.

    ![Azure Data Factory başlayalım](media/data-factory-template/adf-get-started.png)

1. Şablondan **Azure Veri Gezgini toplu kopyalama**' yı seçin.
 
    ![Şablondan işlem hattı Seç](media/data-factory-template/pipeline-from-template.png)

1.  **Veritabanından Azure 'A toplu kopyalama veri Gezgini** penceresinde, açılan listeden var olan veri kümelerini seçin. 

    * **Denetimtabloveri kümesi** , kaynaktan hedefe hangi verilerin kopyalanacağını ve hedefe nereye yerleştirileceğini belirten denetim tablosuna bağlı hizmet. 
    * **SourceDataset** : kaynak veritabanına bağlı hizmet. 
    * **AzureDataExplorerTable** -Azure Veri Gezgini tablosu. Veri kümesi yoksa, veri kümesini eklemek için [Azure Veri Gezgini bağlı hizmetini oluşturun](data-factory-load-data.md#create-the-azure-data-explorer-linked-service) .
    * **Bu şablonu kullan**' ı seçin.

    ![toplu kopyalama Azure Veri Gezgini şablonunu yapılandırma](media/data-factory-template/configure-bulk-copy-adx-template.png)

1. Şablon ardışık düzenine erişmek için, etkinliklerin dışında tuvalde bir alan seçin. **Ad** (denetim tablosu adı) ve **varsayılan değer** (sütun adları) dahil olmak üzere tablonun parametrelerini girmek için **Parametreler** ' i seçin.

    ![İşlem hattı parametreleri](media/data-factory-template/pipeline-parameters.png)

1.  Varsayılan ayarları görüntülemek için arama etkinliği, **Getpartitionlist**öğesini seçin. Sorgu otomatik olarak oluşturulur.
1.  Komut etkinliği **ForEachPartition**seç, **ayarları** seçin
    * **Yığın sayısı**: 1-50 ' den sayı ' yı seçin. Bu seçim, **Denetim tabloveri kümesi** satır sayısına ulaşıncaya kadar paralel olarak çalışan işlem hattı sayısını belirler. 
    * Ardışık düzen toplu işlemlerinin paralel olarak çalışması için **sıralı** onay kutusunu seçmeyin.

    ![ForEachPartition ayarları](media/data-factory-template/foreach-partition-settings.png)

    > [!TIP]
    > En iyi yöntem, verilerin daha hızlı bir şekilde kopyalanabilmesi için paralel olarak birçok işlem hattı çalıştırmalıdır. Kaynak tablodaki verileri bölümleyin ve verimliliği artırmak için tarih ve tabloya göre işlem hattı başına bir bölüm ayırın.

1. ADF işlem hattını doğrulamak için **Tümünü doğrula** ' yı seçin. Bkz. işlem **hattı doğrulama çıktısı**.

    ![Şablon işlem hatlarını doğrulama](media/data-factory-template/validate-template-pipelines.png)

1. Gerekirse **Hata Ayıkla**' yı seçin ve ardından işlem hattını çalıştırmak Için **tetikleyici ekleyin** .

    ![İşlem hattını Çalıştır](media/data-factory-template/trigger-run-of-pipeline.png)    


Artık veritabanlarınızdan ve tablolardan büyük miktarlarda veriyi verimli bir şekilde kopyalamak için **veritabanından toplu kopyalama ' yı Azure Veri Gezgini** şablonu ' na kullanabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Data Factory kullanarak verileri Azure Veri Gezgini kopyalama](data-factory-load-data.md)yordamı hakkında bilgi edinin.

* Azure Data Factory [Azure Veri Gezgini Bağlayıcısı](/azure/data-factory/connector-azure-data-explorer) hakkında bilgi edinin.

* Veri sorgulama için [Azure Veri Gezgini sorguları](/azure/data-explorer/web-query-data) hakkında bilgi edinin.






