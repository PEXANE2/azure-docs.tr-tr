---
title: Azure Veri Fabrikası şablonu kullanarak veritabanından Azure Veri Gezgini'ne toplu kopyala
description: Bu makalede, bir veritabanından Azure Veri Gezgini'ne toplu olarak kopyalamak için bir Azure Veri Fabrikası şablonu kullanmayı öğreniyorsunuz
services: data-explorer
author: orspod
ms.author: orspodek
ms.reviewer: tzgitlin
ms.service: data-explorer
ms.topic: conceptual
ms.date: 09/08/2019
ms.openlocfilehash: 884f4e956b37c2def6c25d0acdf20f15eddf7767
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76293564"
---
# <a name="copy-in-bulk-from-a-database-to-azure-data-explorer-by-using-the-azure-data-factory-template"></a>Azure Veri Fabrikası şablonu kullanarak veritabanından Azure Veri Gezgini'ne toplu kopyala 

Azure Veri Gezgini, hızlı, tam olarak yönetilen, veri analizi hizmetidir. Uygulamalar, web siteleri ve IoT aygıtları gibi birçok kaynaktan akış yapan büyük hacimli veriler üzerinde gerçek zamanlı analiz sunar. 

Oracle Server, Netezza, Teradata veya SQL Server'daki bir veritabanındaki verileri Azure Veri Gezgini'ne kopyalamak için birden çok tablodan büyük miktarda veri yüklemeniz gerekir. Genellikle, tek bir tablodan paralel olarak birden çok iş parçacığı olan satırları yükleyebilmeniz için verilerin her tabloda bölümlenmesi gerekir. Bu makalede, bu senaryolarda kullanılacak bir şablon açıklanmaktadır.

[Azure Veri Fabrikası şablonları](/azure/data-factory/solution-templates-introduction) önceden tanımlanmış Veri Fabrikası ardışık hatlarıdır. Bu şablonlar, Veri Fabrikası'na hızlı bir şekilde başlamanıza ve veri tümleştirme projelerinde geliştirme süresini kısaltmanıza yardımcı olabilir. 

*Arama* ve *ForEach* etkinliklerini kullanarak *Veritabanından Azure Veri Gezgini şablonuna Toplu Kopya* oluşturursunuz. Daha hızlı veri kopyalama için, veritabanı veya tablo başına birçok ardışık iş aktarım oluşturmak için şablonu kullanabilirsiniz. 

> [!IMPORTANT]
> Kopyalamak istediğiniz veri miktarına uygun aracı kullandığınızdan emin olun.
> * SQL sunucusu ve Google BigQuery gibi veritabanlarından Azure Veri Gezgini'ne kadar büyük miktarda veriyi kopyalamak için *Veritabanından Azure Veri Gezgini* şablonuna toplu kopyayı kullanın. 
> * Küçük veya orta miktarda veri içeren birkaç tabloyu Azure Veri Gezgini'ne kopyalamak için [*Veri Fabrikası Kopyalama Veri aracını*](data-factory-load-data.md) kullanın. 

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir Azure hesabı](https://azure.microsoft.com/free/) oluşturun.
* [Azure Veri Gezgini kümesi ve veritabanı.](create-cluster-database-portal.md)
* [Bir veri fabrikası oluşturun.](data-factory-load-data.md#create-a-data-factory)
* Veritabanındaki veri kaynağı.

## <a name="create-controltabledataset"></a>ControlTableDataset oluştur

*ControlTableDataset,* hangi verilerin kaynaktan kaynaktan hedefe kopyalanacağını gösterir. Satır sayısı, verileri kopyalamak için gereken toplam satır hattı sayısını gösterir. ControlTableDataset'i kaynak veritabanının bir parçası olarak tanımlamalısınız.

SQL Server kaynak tablo biçiminin bir örneği aşağıdaki kodda gösterilmiştir:
    
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
|Partitionıd   |  Kopyalama sırası | 1  |  
|Kaynak Sorgu   |  Ardışık hatlar çalışma süresi boyunca hangi verilerin kopyalanacağını belirten sorgu | <br>`select * from table where lastmodifiedtime  LastModifytime >= ''2015-01-01 00:00:00''>` </br>    
|ADXTableName  |  Hedef tablo adı | MyAdxTable       |  

ControlTableDataset'iniz farklı bir biçimdeyse, biçiminiz için karşılaştırılabilir bir ControlTableDataset oluşturun.

## <a name="use-the-bulk-copy-from-database-to-azure-data-explorer-template"></a>Veritabanından Azure Veri Gezgini şablonuna Toplu Kopya kullanma

1. **Başlatılalım** bölmesinde, **Şablon galeri** bölmesini açmak için **şablondan ardışık kaynak oluştur'u** seçin.

    ![Azure Veri Fabrikası "Başlayalım" bölmesi](media/data-factory-template/adf-get-started.png)

1. **Veritabanından Azure Veri Gezgini şablonuna Toplu Kopya'yı** seçin.
 
    !["Veritabanından Azure Veri Gezgini'ne Toplu Kopya" şablonu](media/data-factory-template/pipeline-from-template.png)

1.  Kullanıcı **Girişleri**altında **Veritabanından Azure Veri Gezgini bölmesine Toplu Kopya'da** aşağıdakileri yaparak veri kümelerinizi belirtin: 

    a. **ControlTableDataset** açılır listesinde, kaynaktan hedefe hangi verilerin kopyalanacağını ve hedefe nereye yerleştirileceğini gösteren denetim tablosuna bağlı hizmeti seçin. 

    b. **SourceDataset** açılır listesinde, kaynak veritabanına bağlı hizmeti seçin. 

    c. **AzureDataExplorerTable** açılır listesinde Azure Veri Gezgini tablosunu seçin. Veri kümesi yoksa, veri kümesini eklemek için [Azure Veri Gezgini bağlantılı hizmeti oluşturun.](data-factory-load-data.md#create-the-azure-data-explorer-linked-service)

    d. **Bu şablonu kullan'ı**seçin.

    !["Veritabanından Azure Veri Gezgini'ne Toplu Kopya" bölmesi](media/data-factory-template/configure-bulk-copy-adx-template.png)

1. Şablon ardışık bölgeye erişmek için tuvalde etkinliklerin dışında bir alan seçin. **Ad** (denetim tablosu adı) ve **Varsayılan değer** (sütun adları) dahil olmak üzere tablonun parametrelerini girmek için **Parametreler** sekmesini seçin.

    ![İşlem hattı parametreleri](media/data-factory-template/pipeline-parameters.png)

1.  **Arama**altında, varsayılan ayarları görüntülemek için **GetPartitionList'i** seçin. Sorgu otomatik olarak oluşturulur.
1.  Komut etkinliği, **ForEachPartition'** ı seçin, **Ayarlar** sekmesini seçin ve ardından aşağıdakileri yapın:

    a. Toplu **Iş sayısı** kutusuna 1'den 50'ye kadar bir sayı girin. Bu seçim, *ControlTableDataset* satır sayısına ulaşılıncaya kadar paralel olarak çalışan ardışık hatlar sayısını belirler. 

    b. Ardışık toplu iş aktülerinin paralel çalışmasını sağlamak için **Sıralı** onay kutusunu *seçmeyin.*

    ![ForEachPartition ayarları](media/data-factory-template/foreach-partition-settings.png)

    > [!TIP]
    > En iyi yöntem, verilerinizin daha hızlı kopyalanabilmesi için birçok ardışık hattı paralel olarak çalıştırmaktır. Verimliliği artırmak için, verileri kaynak tabloya bölün ve tarih ve tabloya göre ardışık hatlar başına bir bölüm ayırın.

1. Azure Veri Fabrikası ardışık hattını doğrulamak için **Tümünü Doğrula'yı** seçin ve ardından sonucu **Pipeline Doğrulama Çıktısı** bölmesinde görüntüleyin.

    ![Şablon ardışık hatlarını doğrulama](media/data-factory-template/validate-template-pipelines.png)

1. Gerekirse Hata **Ayıklama'yı**seçin ve ardından ardışık bölümü çalıştırmak için **tetikleyici ekle'yi** seçin.

    !["Hata Ayıklama" ve "Ardışık hattı çalıştır" düğmeleri](media/data-factory-template/trigger-run-of-pipeline.png)    

Artık veritabanı ve tablolarınızdan büyük miktarda veriyi verimli bir şekilde kopyalamak için şablonu kullanabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Veri Fabrikası'nı kullanarak verileri Azure Veri Gezgini'ne](data-factory-load-data.md)kopyalamayı öğrenin.
* Azure Veri Fabrikası'ndaki [Azure Veri Gezgini bağlayıcısı](/azure/data-factory/connector-azure-data-explorer) hakkında bilgi edinin.
* Veri sorgulama için [Azure Veri Gezgini sorguları](/azure/data-explorer/web-query-data) hakkında bilgi edinin.






