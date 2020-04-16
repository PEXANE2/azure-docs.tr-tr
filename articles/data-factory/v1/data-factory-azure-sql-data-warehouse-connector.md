---
title: Azure SQL Veri Ambarına/Azure'dan veri kopyalama
description: Azure Veri Fabrikası'nı kullanarak Azure SQL Veri Ambarına verileri kopyalamayı öğrenin
services: data-factory
documentationcenter: ''
author: linda33wj
manager: shwang
ms.assetid: d90fa9bd-4b79-458a-8d40-e896835cfd4a
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 01/10/2018
ms.author: jingwang
robots: noindex
ms.openlocfilehash: 4335763269f4a39b4893d9022f4789296b178e92
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81419332"
---
# <a name="copy-data-to-and-from-azure-sql-data-warehouse-using-azure-data-factory"></a>Azure Veri Fabrikası'nı kullanarak Azure SQL Veri Ambarına veri kopyalama
> [!div class="op_single_selector" title1="Kullandığınız Veri Fabrikası hizmetisürümünü seçin:"]
> * [Sürüm 1](data-factory-azure-sql-data-warehouse-connector.md)
> * [Sürüm 2 (geçerli sürüm)](../connector-azure-sql-data-warehouse.md)

> [!NOTE]
> Bu makale, Data Factory’nin 1. sürümü için geçerlidir. Veri Fabrikası hizmetinin geçerli sürümünü kullanıyorsanız, [V2'deki Azure SQL Veri Ambarı bağlayıcısı'na](../connector-azure-sql-data-warehouse.md)bakın.

Bu makalede, verileri Azure SQL Veri Ambarına/Azure Veri Ambarına taşımak için Azure Veri Fabrikası'ndaki Kopyalama Etkinliği'nin nasıl kullanılacağı açıklanmaktadır. Kopya etkinliğiyle birlikte veri hareketine genel bir genel bakış sunan [Veri Hareketi Etkinlikleri](data-factory-data-movement-activities.md) makalesine dayanmaktadır.

> [!TIP]
> En iyi performansı elde etmek için Azure SQL Veri Ambarı'na veri yüklemek için PolyBase'i kullanın. [Verileri Azure SQL Veri Ambarı bölümüne yüklemek için PolyBase'i kullanın](data-factory-azure-sql-data-warehouse-connector.md#use-polybase-to-load-data-into-azure-sql-data-warehouse) ayrıntıları vardır. Kullanım örneğine sahip bir geçiş için, [Azure Veri Fabrikası ile 15 dakikanın altında Azure SQL Veri Ambarına 1 TB Yükle'ye](data-factory-load-sql-data-warehouse.md)bakın.

## <a name="supported-scenarios"></a>Desteklenen senaryolar
Verileri Azure **SQL Veri Ambarı'ndan** aşağıdaki veri depolarına kopyalayabilirsiniz:

[!INCLUDE [data-factory-supported-sinks](../../../includes/data-factory-supported-sinks.md)]

Aşağıdaki veri depolarından Azure **SQL Veri Ambarı'na**veri kopyalayabilirsiniz:

[!INCLUDE [data-factory-supported-sources](../../../includes/data-factory-supported-sources.md)]

> [!TIP]
> SQL Server veya Azure SQL Veritabanı'ndan Azure SQL Veri Ambarı'na veri kopyalanırken, tablo hedef deposunda yoksa, Veri Fabrikası kaynak veri deposundaki tabloşemasını kullanarak SQL Veri Ambarı'ndaki tabloyu otomatik olarak oluşturabilir. Ayrıntılar için [Otomatik tablo oluşturma konusuna](#auto-table-creation) bakın.

## <a name="supported-authentication-type"></a>Desteklenen kimlik doğrulama türü
Azure SQL Veri Ambarı bağlayıcısı temel kimlik doğrulamasını destekler.

## <a name="getting-started"></a>Başlarken
Farklı araçlar/API'ler kullanarak verileri Azure SQL Veri Ambarına/Azure Sql Veri Ambarına aktaran bir kopyalama etkinliği içeren bir ardışık kaynak oluşturabilirsiniz.

Azure SQL Veri Ambarı'na/Azure'dan veri kopyalayan bir ardışık kaynak oluşturmanın en kolay yolu Kopya veri sihirbazını kullanmaktır. Bkz. Öğretici: Veri Kopyalama sihirbazını kullanarak bir ardışık yol oluşturma konusunda hızlı bir geçiş için Veri Fabrikası ile SQL Veri Ambarı'na [veri yükleyin.](../../sql-data-warehouse/sql-data-warehouse-load-with-data-factory.md)

Bir ardışık kaynak oluşturmak için aşağıdaki araçları da kullanabilirsiniz: **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager şablonu**, **.NET API**ve **REST API**. Kopyalama etkinliği içeren bir ardışık hatlar oluşturmak için adım adım yönergeleri için [etkinlik öğreticisini](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) kopyala'ya bakın.

Araçları veya API'leri kullanın, verileri kaynak veri deposundan bir lavabo veri deposuna aktaran bir ardışık işlem hattı oluşturmak için aşağıdaki adımları gerçekleştirirsiniz:

1. Bir **veri fabrikası**oluşturun. Bir veri fabrikası bir veya daha fazla ardışık hat lar içerebilir. 
2. Giriş ve çıktı veri depolarını veri fabrikanıza bağlamak için **bağlantılı hizmetler** oluşturun. Örneğin, bir Azure blob deposundan Azure SQL veri ambarına veri kopyalıyorsanız, Azure depolama hesabınızı ve Azure SQL veri ambarınızı veri fabrikanıza bağlamak için iki bağlantılı hizmet oluşturursunuz. Azure SQL Veri Ambarı'na özgü bağlantılı hizmet özellikleri için [bağlantılı hizmet özellikleri](#linked-service-properties) bölümüne bakın. 
3. Kopyalama işlemi için giriş ve çıktı verilerini temsil edecek **veri kümeleri** oluşturun. Son adımda belirtilen örnekte, giriş verilerini içeren blob kapsayıcısını ve klasörünü belirtmek için bir veri kümesi oluşturursunuz. Ayrıca, tabloyu blob depolamadan kopyalanan verileri tutan Azure SQL veri ambarında belirtmek için başka bir veri kümesi oluşturursunuz. Azure SQL Veri Ambarı'na özgü veri kümesi özellikleri için [bkz.](#dataset-properties)
4. Giriş olarak veri kümesi ve çıktı olarak veri kümesi alan bir kopyalama etkinliği içeren bir **ardışık işlem oluşturma.** Daha önce bahsedilen örnekte, blobSource'u kaynak olarak, SqlDWSink'i ise kopyalama etkinliği için lavabo olarak kullanırsınız. Benzer şekilde, Azure SQL Veri Ambarı'ndan Azure Blob Depolama'ya kopyalıyorsanız, kopyalama etkinliğinde SqlDWSource ve BlobSink'i kullanırsınız. Azure SQL Veri Ambarı'na özgü kopyalama etkinlik özellikleri için [kopyalama etkinlik özellikleri](#copy-activity-properties) bölümüne bakın. Kaynak veya lavabo olarak veri deposunun nasıl kullanılacağı yla ilgili ayrıntılar için, veri deponuzun önceki bölümündeki bağlantıyı tıklatın.

Sihirbazı kullandığınızda, bu Veri Fabrikası varlıkları (bağlantılı hizmetler, veri kümeleri ve ardışık kuruluş) için JSON tanımları sizin için otomatik olarak oluşturulur. Araçları/API'leri (.NET API hariç) kullandığınızda, Bu Veri Fabrikası varlıklarını JSON biçimini kullanarak tanımlarsınız. Azure SQL Veri Ambarı'na/Azure Sql Veri Ambarından veri kopyalamak için kullanılan Veri Fabrikası varlıkları için JSON tanımlı örnekler için bu makalenin [JSON örnekleri](#json-examples-for-copying-data-to-and-from-sql-data-warehouse) bölümüne bakın.

Aşağıdaki bölümler, Azure SQL Veri Ambarı'na özgü Veri Fabrikası varlıklarını tanımlamak için kullanılan JSON özellikleri hakkında ayrıntılı bilgi sağlar:

## <a name="linked-service-properties"></a>Bağlantılı hizmet özellikleri
Aşağıdaki tablo, Azure SQL Veri Ambarı bağlantılı hizmete özgü JSON öğeleri için açıklama sağlar.

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| type |Tür özelliği şu şekilde ayarlanmalıdır: **AzureSqlDW** |Evet |
| Connectionstring |ConnectionString özelliği için Azure SQL Veri Ambarı örneğine bağlanmak için gereken bilgileri belirtin. Yalnızca temel kimlik doğrulaması desteklenir. |Evet |

> [!IMPORTANT]
> [Azure Hizmetlerinin sunucuya erişmesine izin verecek](https://msdn.microsoft.com/library/azure/ee621782.aspx#ConnectingFromAzure)şekilde Azure SQL Veritabanı Güvenlik [Duvarı'nı](https://msdn.microsoft.com/library/azure/ee621782.aspx#ConnectingFromAzure) ve veritabanı sunucusunu yapılandırın. Ayrıca, veri fabrikası ağ geçidine sahip şirket içi veri kaynakları da dahil olmak üzere Azure dışından Azure SQL Veri Ambarı'na veri kopyalıyorsanız, Verileri Azure SQL Veri Ambarı'na gönderen makine için uygun IP adresi aralığını yapılandırın.

## <a name="dataset-properties"></a>Veri kümesi özellikleri
Veri kümelerini tanımlamak için kullanılabilen bölümlerin & özelliklerin tam listesi için [veri kümelerini oluşturma](data-factory-create-datasets.md) makalesine bakın. Bir veri kümesi JSON'un yapısı, kullanılabilirliği ve ilkesi gibi bölümler tüm veri kümesi türleri (Azure SQL, Azure blob, Azure tablosu, vb.) için benzerdir.

typeProperties bölümü her veri kümesi türü için farklıdır ve veri deposundaki verilerin konumu hakkında bilgi sağlar. **AzureSqlDWTable** türünün veri kümesinin **typeProperties** bölümü aşağıdaki özelliklere sahiptir:

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| tableName |Bağlı hizmetin atıfta bulunduğu Azure SQL Veri Ambarı veritabanında tablonun veya görünümün adı. |Evet |

## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri
Etkinlikleri tanımlamak için kullanılabilen bölümlerin & özelliklerinin tam listesi [için, Kaynak Hatları Oluşturma](data-factory-create-pipelines.md) makalesine bakın. Ad, açıklama, giriş ve çıktı tabloları ve ilke gibi özellikler tüm etkinlik türleri için kullanılabilir.

> [!NOTE]
> Kopyalama Etkinliği yalnızca bir giriş alır ve yalnızca bir çıktı üretir.

Oysa, etkinliğin typeProperties bölümünde bulunan özellikler her etkinlik türüne göre değişir. Kopyalama etkinliği için, kaynak ve lavabo türlerine bağlı olarak değişir.

### <a name="sqldwsource"></a>SqlDWSource
Kaynak **SqlDWSource**türünde olduğunda, **typeProperties** bölümünde aşağıdaki özellikler mevcuttur:

| Özellik | Açıklama | İzin verilen değerler | Gerekli |
| --- | --- | --- | --- |
| sqlReaderQuery |Verileri okumak için özel sorguyu kullanın. |SQL sorgu dizesi. Örneğin: MyTable'dan * seçin. |Hayır |
| sqlReaderStoredProcedureNameName |Kaynak tablodaki verileri okuyan depolanan yordamın adı. |Depolanan yordamın adı. Son SQL deyimi, depolanan yordamda bir SELECT deyimi olmalıdır. |Hayır |
| depolanmışProsedürParametreleri |Depolanan yordam için parametreler. |Ad/değer çiftleri. Parametrelerin adları ve kasası, depolanan yordam parametrelerinin adları ve kasalarıyla eşleşmelidir. |Hayır |

SqlDWSource için **sqlReaderQuery** belirtilirse, Kopyalama Etkinliği verileri almak için bu sorguyu Azure SQL Veri Ambarı kaynağına göre çalıştırır.

Alternatif olarak, **sqlReaderStoredProcedureName** ve **storedProcedureParametreleri** (depolanan yordam parametreleri alıyorsa) belirterek depolanmış bir yordam belirtebilirsiniz.

sqlReaderQuery veya sqlReaderStoredProcedureName belirtmezseniz, JSON veri kümesinin yapı bölümünde tanımlanan sütunlar Azure SQL Veri Ambarı'na karşı çalışacak bir sorgu oluşturmak için kullanılır. Örnek: `select column1, column2 from mytable`. Veri kümesi tanımıyapısı yoksa, tüm sütunlar tablodan seçilir.

#### <a name="sqldwsource-example"></a>SqlDWSource örneği

```JSON
"source": {
    "type": "SqlDWSource",
    "sqlReaderStoredProcedureName": "CopyTestSrcStoredProcedureWithParameters",
    "storedProcedureParameters": {
        "stringData": { "value": "str3" },
        "identifier": { "value": "$$Text.Format('{0:yyyy}', SliceStart)", "type": "Int"}
    }
}
```
**Depolanan yordam tanımı:**

```SQL
CREATE PROCEDURE CopyTestSrcStoredProcedureWithParameters
(
    @stringData varchar(20),
    @identifier int
)
AS
SET NOCOUNT ON;
BEGIN
    select *
    from dbo.UnitTestSrcTable
    where dbo.UnitTestSrcTable.stringData != stringData
    and dbo.UnitTestSrcTable.identifier != identifier
END
GO
```

### <a name="sqldwsink"></a>SqlDWSink
**SqlDWSink** aşağıdaki özellikleri destekler:

| Özellik | Açıklama | İzin verilen değerler | Gerekli |
| --- | --- | --- | --- |
| sqlWriterCleanupScript |Belirli bir dilimin verilerinin temizlenmesi ni gerçekleştirmek için Kopyalama Etkinliği için bir sorgu belirtin. Ayrıntılar için [tekrarlanabilirlik bölümüne](#repeatability-during-copy)bakın. |Sorgu deyimi. |Hayır |
| Izin PolyBase |BULKINSERT mekanizması yerine PolyBase'in (varsa) kullanılıp kullanılmayacağını gösterir. <br/><br/> **PolyBase'i kullanmak, verileri SQL Veri Ambarı'na yüklemenin önerilen yoludur.** Kısıtlamalar ve ayrıntılar için [verileri Azure SQL Veri Ambarı bölümüne yüklemek için PolyBase'i kullanın'](#use-polybase-to-load-data-into-azure-sql-data-warehouse) a bakın. |True <br/>False (varsayılan) |Hayır |
| polyBaseSettings |**İzin veren Polybase** özelliği **doğru**ayarlandığında belirtilebilen özellikler grubu. |&nbsp; |Hayır |
| rejectValue |Sorgu başarısız olmadan önce reddedilebilen satırların sayısını veya yüzdesini belirtir. <br/><br/>[CREATE EXTERNAL TABLE (Transact-SQL)](https://msdn.microsoft.com/library/dn935021.aspx) konusunun **Bağımsız Değişkenler** bölümünde PolyBase'in reddetme seçenekleri hakkında daha fazla bilgi edinin. |0 (varsayılan), 1, 2, ... |Hayır |
| rejectType |RejectValue seçeneğinin gerçek bir değer mi yoksa yüzde olarak mı belirtildiğini belirtir. |Değer (varsayılan), Yüzde |Hayır |
| rejectSampleValue |PolyBase reddedilen satırların yüzdesini yeniden hesaplamadan önce alınacak satır sayısını belirler. |1, 2, ... |Evet, **rejectType** **yüzde** ise |
| useTypeDefault |PolyBase metin dosyasından veri aldığında sınırlı metin dosyalarındaki eksik değerlerin nasıl işleyeceğini belirtir.<br/><br/>[CREATE EXTERNAL FILE FORMAT (Transact-SQL)](https://msdn.microsoft.com/library/dn935026.aspx)'deki Bağımsız Değişkenler bölümünden bu özellik hakkında daha fazla bilgi edinin. |True, False (varsayılan) |Hayır |
| yazmaBatchSize |Arabellek boyutu writeBatchSize ulaştığında SQL tablosuna veri ekler |Sonsayı (satır sayısı) |Hayır (varsayılan: 10000) |
| yazmaBatchTimeout |Toplu ekleme işleminin zaman dolmadan tamamlanması için bekleme süresi. |Timespan<br/><br/> Örnek: "00:30:00" (30 dakika). |Hayır |

#### <a name="sqldwsink-example"></a>SqlDWSink örneği

```JSON
"sink": {
    "type": "SqlDWSink",
    "allowPolyBase": true
}
```

## <a name="use-polybase-to-load-data-into-azure-sql-data-warehouse"></a>Verileri Azure SQL Veri Ambarı'na yüklemek için PolyBase'i kullanın
**[PolyBase'i](https://docs.microsoft.com/sql/relational-databases/polybase/polybase-guide)** kullanmak, yüksek iş ortası ile Azure SQL Veri Ambarı'na büyük miktarda veri yüklemenin etkili bir yoludur. Varsayılan BULKINSERT mekanizması yerine PolyBase kullanarak iş elde edilen iş başına büyük bir kazanç görebilirsiniz. Ayrıntılı karşılaştırma ile [kopyalama performans başvuru numarasına](data-factory-copy-activity-performance.md#performance-reference) bakın. Kullanım örneğine sahip bir geçiş için, [Azure Veri Fabrikası ile 15 dakikanın altında Azure SQL Veri Ambarına 1 TB Yükle'ye](data-factory-load-sql-data-warehouse.md)bakın.

* Kaynak verileriniz **Azure Blob veya Azure Veri Gölü**Deposu'ndaysa ve biçim PolyBase ile uyumluysa, Doğrudan PolyBase'i kullanarak Azure SQL Veri Ambarı'na kopyalayabilirsiniz. Ayrıntılarla **[birlikte PolyBase'i kullanarak Doğrudan kopyaya](#direct-copy-using-polybase)** bakın.
* Kaynak veri depolamanız ve biçiminiz başlangıçta PolyBase tarafından desteklenmiyorsa, Bunun yerine **[PolyBase](#staged-copy-using-polybase)** özelliğini kullanarak Aşamalı Kopya'yı kullanabilirsiniz. Ayrıca, verileri otomatik olarak PolyBase uyumlu biçime dönüştürerek ve verileri Azure Blob depolama alanında depolayarak daha iyi iş elde etme nizi sağlar. Daha sonra verileri SQL Veri Ambarı'na yükler.

Azure `allowPolyBase` Veri Fabrikası için aşağıdaki örnekte gösterildiği gibi özelliği **gerçeğe ayarla** ve verileri Azure SQL Veri Ambarı'na kopyalamak için PolyBase'i kullanın. Allow PolyBase'in doğru olmasını ayarladığınızda, `polyBaseSettings` özellik grubunu kullanarak PolyBase'e özgü özellikleri belirtebilirsiniz. polyBaseSettings ile kullanabileceğiniz özellikler hakkında ayrıntılar için [SqlDWSink](#sqldwsink) bölümüne bakın.

```JSON
"sink": {
    "type": "SqlDWSink",
    "allowPolyBase": true,
    "polyBaseSettings":
    {
        "rejectType": "percentage",
        "rejectValue": 10.0,
        "rejectSampleValue": 100,
        "useTypeDefault": true
    }
}
```

### <a name="direct-copy-using-polybase"></a>PolyBase kullanarak doğrudan kopyalama
SQL Veri Ambarı PolyBase, kaynak olarak ve belirli dosya biçimi gereksinimleriyle Azure Blob ve Azure Veri Gölü Deposu'na (hizmet ilkesini kullanarak) doğrudan destek verir. Kaynak verileriniz bu bölümde açıklanan ölçütleri karşılıyorsa, Doğrudan kaynak veri deposundan Azure SQL Veri Ambarı'na PolyBase'i kullanarak kopyalayabilirsiniz. Aksi takdirde, [PolyBase kullanarak Staged Copy](#staged-copy-using-polybase)kullanabilirsiniz.

> [!TIP]
> Data Lake Store'dan SQL Veri Ambarı'na verileri verimli bir şekilde kopyalamak, Azure Veri Fabrikası'ndan daha fazla bilgi edinmek, [SQL Veri Ambarı ile Data Lake Store'u kullanırken verilerden elde edilen istatistikleri ortaya](https://blogs.msdn.microsoft.com/azuredatalake/2017/04/08/azure-data-factory-makes-it-even-easier-and-convenient-to-uncover-insights-from-data-when-using-data-lake-store-with-sql-data-warehouse/)çıkarmak için daha da kolay ve kullanışlı hale getirir.

Gereksinimler karşılanmazsa, Azure Veri Fabrikası ayarları denetler ve veri hareketi için OTOMATIK olarak BULKINSERT mekanizmasına geri döner.

1. **Kaynak bağlantılı hizmet** türündedir: Hizmet temel kimlik doğrulaması ile **AzureStorage** veya **AzureDataLakeStore.**
2. **Giriş veri kümesi** türündedir: **AzureBlob** veya **AzureDataLakeStore**ve `type` özellikleri altında biçim türü **OrcFormat**, **ParkquetFormat**veya **TextFormat** aşağıdaki yapılandırmaları ile:

   1. `rowDelimiter`**\n**olmalıdır.
   2. `nullValue`**dize** ("") boş `treatEmptyAsNull` olarak ayarlanır veya **doğru**olarak ayarlanır.
   3. `encodingName`**varsayılan** değer olan **utf-8**olarak ayarlanır.
   4. `escapeChar`, `quoteChar` `firstRowAsHeader`, `skipLineCount` , ve belirtilmemiştir.
   5. `compression`hiçbir **sıkıştırma**olabilir , **GZip**, veya **Deflate**.

      ```JSON
      "typeProperties": {
       "folderPath": "<blobpath>",
       "format": {
           "type": "TextFormat",
           "columnDelimiter": "<any delimiter>",
           "rowDelimiter": "\n",
           "nullValue": "",
           "encodingName": "utf-8"
       },
       "compression": {
           "type": "GZip",
           "level": "Optimal"
       }
      },
      ```

3. `skipHeaderLineCount` **BlobSource** veya **AzureDataLakeStore** altında, ardışık ardışık ardışık alandaki Kopyalama etkinliği için ayar yoktur.
4. Pipeline'daki `sliceIdentifierColumnName` Kopyalama etkinliği için **SqlDWSink** altında ayar yoktur. (PolyBase, tüm verilerin güncelleştirilmelerini veya hiçbir şeyin tek bir çalıştırmada güncellenmediğini garanti eder. **Tekrarlanabilirlik**elde etmek `sqlWriterCleanupScript`için, kullanabilirsiniz).
5. İlişkili `columnMapping` Kopya etkinliğinde kullanılmaz.

### <a name="staged-copy-using-polybase"></a>PolyBase kullanarak Aşamalı Kopya
Kaynak verileriniz önceki bölümde tanıtılan ölçütlere uymadığında, geçici bir evreleme Azure Blob Depolama yoluyla veri kopyalamayı etkinleştirebilirsiniz (Premium Depolama olamaz). Bu durumda, Azure Veri Fabrikası, PolyBase'in veri biçimi gereksinimlerini karşılamak için verilerüzerinde otomatik olarak dönüşümler gerçekleştirir, ardından verileri SQL Veri Ambarı'na yüklemek için PolyBase'i kullanır ve en sonunda Geçici Verilerinizi Blob depolamadan temizler. Bir evreleme Azure Blob aracılığıyla veri kopyalamanın genel olarak nasıl çalıştığına ilişkin ayrıntılar için [Aşamalı Kopya'ya](data-factory-copy-activity-performance.md#staged-copy) bakın.

> [!NOTE]
> Şirket içi bir veri deposundan PolyBase ve evreleme kullanarak Azure SQL Veri Ambarı'na veri kopyalarken, Veri Yönetimi Ağ Geçidi sürümünüz 2,4'ün altındaysa, kaynak verilerinizi uygun biçime dönüştürmek için kullanılan ağ geçidi makinenizde JRE (Java Runtime Environment) gereklidir. Bu tür bağımlılıktan kaçınmak için ağ geçidinizi en sona yükseltmenizi öneririz.
>

Bu özelliği kullanmak için, geçici blob depolama alanına sahip Azure Depolama Hesabı'na atıfta `enableStaging` `stagingSettings` bulunan bir Azure Depolama [bağlantılı hizmet](data-factory-azure-blob-connector.md#azure-storage-linked-service) oluşturun ve ardından Kopyalama Etkinliği'nin özelliklerini ve özelliklerini aşağıdaki kodda gösterildiği gibi belirtin:

```json
"activities":[
{
    "name": "Sample copy activity from SQL Server to SQL Data Warehouse via PolyBase",
    "type": "Copy",
    "inputs": [{ "name": "OnpremisesSQLServerInput" }],
    "outputs": [{ "name": "AzureSQLDWOutput" }],
    "typeProperties": {
        "source": {
            "type": "SqlSource",
        },
        "sink": {
            "type": "SqlDwSink",
            "allowPolyBase": true
        },
        "enableStaging": true,
        "stagingSettings": {
            "linkedServiceName": "MyStagingBlob"
        }
    }
}
]
```

## <a name="best-practices-when-using-polybase"></a>PolyBase kullanırken en iyi uygulamalar
Aşağıdaki bölümler, [Azure SQL Veri Ambarı için En İyi Uygulamalar'da](../../synapse-analytics/sql-data-warehouse/sql-data-warehouse-best-practices.md)belirtilenlere ek en iyi uygulamalar sağlar.

### <a name="required-database-permission"></a>Gerekli veritabanı izni
PolyBase'i kullanmak için, SQL Veri Ambarı'na veri yüklemek için kullanılan kullanıcının hedef veritabanında ["CONTROL" iznine](https://msdn.microsoft.com/library/ms191291.aspx) sahip olmasını gerektirir. Bunu başarabilmek için bir yolu "db_owner" rolü bir üyesi olarak bu kullanıcı eklemektir. [Bu bölümü](../../synapse-analytics/sql-data-warehouse/sql-data-warehouse-overview-manage-security.md#authorization)izleyerek bunu nasıl yapacağınızı öğrenin.

### <a name="row-size-and-data-type-limitation"></a>Satır boyutu ve veri türü sınırlaması
Polybase yükleri **hem 1 MB'dan** küçük yükleme satırlarıyla sınırlıdır hem de VARCHR(MAX), NVARCHAR(MAX) veya VARBINARY(MAX)'e yüklenemez. [Buraya](../../synapse-analytics/sql-data-warehouse/sql-data-warehouse-service-capacity-limits.md#loads)bakın .

Boyutu 1 MB'dan büyük satırları olan kaynak verileriniz varsa, kaynak tablolarını her birinin en büyük satır boyutunun sınırı aşmadığı birkaç küçük tabloya dikey olarak bölmek isteyebilirsiniz. Daha küçük tablolar Daha sonra PolyBase kullanılarak yüklenebilir ve Azure SQL Veri Ambarı'nda birleştirilebilir.

### <a name="sql-data-warehouse-resource-class"></a>SQL Veri Ambarı kaynak sınıfı
Mümkün olan en iyi iş verisini elde etmek için, PolyBase üzerinden SQL Veri Ambarı'na veri yüklemek için kullanılan kullanıcıya daha büyük kaynak sınıfı atamayı düşünün. [Kullanıcı kaynağı sınıfı örneğini](../../sql-data-warehouse/sql-data-warehouse-develop-concurrency.md)değiştir'i izleyerek bunu nasıl yapacağınızı öğrenin.

### <a name="tablename-in-azure-sql-data-warehouse"></a>azure SQL Veri Ambarında tablo Adı
Aşağıdaki tablo, çeşitli şema ve tablo adı kombinasyonları için veri kümesi JSON'daki **tablo Adı** özelliğinin nasıl belirtilen örnekler sağlar.

| DB Şema | Tablo adı | tableName JSON özelliği |
| --- | --- | --- |
| Dbo |Mytable |MyTable veya dbo. MyTable veya [dbo]. [MyTable] |
| dbo1 |Mytable |dbo1. MyTable veya [dbo1]. [MyTable] |
| Dbo |My.Table |[My.Table] veya [dbo]. [My.Table] |
| dbo1 |My.Table |[dbo1]. [My.Table] |

Aşağıdaki hatayı görürseniz, tabloAdı özelliği için belirttiğiniz değerle ilgili bir sorun olabilir. Tablo Adı JSON özelliği için değerleri belirtmenin doğru yolu için tabloya bakın.

```
Type=System.Data.SqlClient.SqlException,Message=Invalid object name 'stg.Account_test'.,Source=.Net SqlClient Data Provider
```

### <a name="columns-with-default-values"></a>Varsayılan değerlere sahip sütunlar
Şu anda, Veri Fabrikası'ndaki PolyBase özelliği yalnızca hedef tablodaki sütun sayısını kabul eder. Diyelim ki, dört sütunlu bir tablonuz var ve bunlardan biri varsayılan değerle tanımlanır. Giriş verileri yine de dört sütun içermelidir. 3 sütunlu bir giriş veri kümesi sağlamak, aşağıdaki iletiye benzer bir hata verir:

```
All columns of the table must be specified in the INSERT BULK statement.
```
NULL değeri varsayılan değerin özel bir biçimidir. Sütun nullable ise, bu sütuniçin giriş verileri (blob) boş olabilir (giriş veri kümesinden eksik olamaz). PolyBase, Azure SQL Veri Ambarı'nda onlar için NULL ekler.

## <a name="auto-table-creation"></a>Otomatik tablo oluşturma
SQL Server veya Azure SQL Veritabanı'ndan Azure SQL Veri Ambarı'na veri kopyalamak için Copy Sihirbazı kullanıyorsanız ve kaynak tabloya karşılık gelen tablo hedef deposunda yoksa, Veri Fabrikası kaynak tablo şemasını kullanarak veri ambarındaki tabloyu otomatik olarak oluşturabilir.

Veri Fabrikası, kaynak veri deposunda aynı tablo adı ile hedef deposunda tablo oluşturur. Sütunlar için veri türleri aşağıdaki tür eşleme temel alınarak seçilir. Gerekirse, kaynak ve hedef mağazalar arasındaki uyumsuzlukları gidermek için tür dönüşümleri gerçekleştirir. Ayrıca Round Robin tablo dağılımını kullanır.

| Kaynak SQL Veritabanı sütun türü | Hedef SQL DW sütun türü (boyut sınırlaması) |
| --- | --- |
| int | int |
| Bigint | Bigint |
| Smallint | Smallint |
| Tinyint | Tinyint |
| Bit | Bit |
| Ondalık | Ondalık |
| Sayısal | Ondalık |
| Kayan | Kayan |
| Para | Para |
| Gerçek | Gerçek |
| Smallmoney | Smallmoney |
| İkili | İkili |
| Varbinary | Varbinary (8000'e kadar) |
| Tarih | Tarih |
| DateTime | DateTime |
| DateTime2 | DateTime2 |
| Zaman | Zaman |
| DateTimeOffset | DateTimeOffset |
| Smalldatetime | Smalldatetime |
| Metin | Varchar (8000'e kadar) |
| Ntext | NVarChar (4000'e kadar) |
| Görüntü | VarBinary (8000'e kadar) |
| Uniqueidentifier | Uniqueidentifier |
| Char | Char |
| Nchar | Nchar |
| Varchar | VarChar (8000'e kadar) |
| Nvarchar | NVarChar (4000'e kadar) |
| Xml | Varchar (8000'e kadar) |

[!INCLUDE [data-factory-type-repeatability-for-sql-sources](../../../includes/data-factory-type-repeatability-for-sql-sources.md)]

## <a name="type-mapping-for-azure-sql-data-warehouse"></a>Azure SQL Veri Ambarı için tür eşleme
[Veri hareketi etkinlikleri](data-factory-data-movement-activities.md) makalesinde belirtildiği gibi, Kopyalama etkinliği aşağıdaki 2 adımlı yaklaşımla kaynak türlerinden lavabo türlerine otomatik tür dönüştürmeleri gerçekleştirir:

1. Yerel kaynak türlerinden .NET türüne dönüştürme
2. .NET türünden yerel lavabo türüne dönüştürme

Verileri Azure SQL Veri Ambarı'ndan & aktarırken, SQL türünden .NET türüne aşağıdaki eşlemeler kullanılır ve bunun tersi de kullanılır.

Eşleme, ADO.NET [için SQL Server Veri Türü Eşlemi](https://msdn.microsoft.com/library/cc716729.aspx)ile aynıdır.

| SQL Server Veritabanı Motoru türü | .NET Framework türü |
| --- | --- |
| bigint |Int64 |
| ikili |Bayt[] |
| bit |Boole |
| char |Dize, Char[] |
| date |DateTime |
| Tarih saat |DateTime |
| datetime2 |DateTime |
| Datetimeoffset |DateTimeOffset |
| Ondalık |Ondalık |
| FILESTREAM özniteliği (varbinary(max)) |Bayt[] |
| Kayan |Çift |
| image |Bayt[] |
| int |Int32 |
| Para |Ondalık |
| Nchar |Dize, Char[] |
| Ntext |Dize, Char[] |
| sayısal |Ondalık |
| nvarchar |Dize, Char[] |
| gerçek |Tek |
| Rowversion |Bayt[] |
| Smalldatetime |DateTime |
| smallint |Int16 |
| Smallmoney |Ondalık |
| Sql_variant |Nesne * |
| metin |Dize, Char[] |
| time |TimeSpan |
| timestamp |Bayt[] |
| tinyint |Bayt |
| uniqueidentifier |Guid |
| Varbinary |Bayt[] |
| varchar |Dize, Char[] |
| xml |Xml |

Ayrıca, kopya etkinliği tanımında kaynak veri kümesinden sütunlara, lavabo veri kümesinden sütunlara kadar sütunları eşleyebilirsiniz. Ayrıntılar için Azure [Veri Fabrikası'ndaki veri kümesi sütunlarını eşleme](data-factory-map-columns.md)bilgisine bakın.

## <a name="json-examples-for-copying-data-to-and-from-sql-data-warehouse"></a>SQL Veri Ambarı'na ve SQL Veri Ambarından veri kopyalamak için JSON örnekleri
Aşağıdaki örnekler, [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) veya [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md)kullanarak bir ardışık hat lar oluşturmak için kullanabileceğiniz örnek JSON tanımları sağlar. Azure SQL Veri Ambarı ve Azure Blob Depolama'ya verilerin nasıl kopyalanır olduğunu gösterirler. Ancak veriler, Azure Veri Fabrikası'ndaki Kopyalama Etkinliği kullanılarak [doğrudan](data-factory-data-movement-activities.md#supported-data-stores-and-formats) herhangi bir **kaynaktan** burada belirtilen lavabolara kopyalanabilir.

### <a name="example-copy-data-from-azure-sql-data-warehouse-to-azure-blob"></a>Örnek: Azure SQL Veri Ambarı'ndan Azure Blob'a veri kopyalama
Örnek, aşağıdaki Veri Fabrikası varlıklarını tanımlar:

1. [AzureSqlDW](#linked-service-properties)türünde bağlantılı bir hizmet.
2. [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties)türüne bağlı bir hizmet.
3. [AzureSqlDWTable](#dataset-properties)türünden bir giriş [veri kümesi.](data-factory-create-datasets.md)
4. [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties)türünden bir çıktı [veri kümesi.](data-factory-create-datasets.md)
5. [SqlDWSource](#copy-activity-properties) ve [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties)kullanan Kopyalama Etkinliği ile bir [ardışık.](data-factory-create-pipelines.md)

Örnek, Zaman serisi (saatlik, günlük vb.) verilerini Azure SQL Veri Ambarı veritabanındaki bir tablodan her saat bir blob'a kopyalar. Bu örneklerde kullanılan JSON özellikleri, örnekleri izleyen bölümlerde açıklanmıştır.

**Azure SQL Veri Ambarı bağlantılı hizmet:**

```JSON
{
  "name": "AzureSqlDWLinkedService",
  "properties": {
    "type": "AzureSqlDW",
    "typeProperties": {
      "connectionString": "Server=tcp:<servername>.database.windows.net,1433;Database=<databasename>;User ID=<username>@<servername>;Password=<password>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
    }
  }
}
```
**Azure Blob depolama bağlantılı hizmet:**

```JSON
{
  "name": "StorageLinkedService",
  "properties": {
    "type": "AzureStorage",
    "typeProperties": {
      "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
    }
  }
}
```
**Azure SQL Veri Ambarı giriş veri kümesi:**

Örnek, Azure SQL Veri Ambarı'nda "MyTable" tablosu oluşturduğunuzu ve zaman serisi verileri için "zaman damgası sütunu" adlı bir sütun içerdiğini varsayar.

"Dış"ı ayarlamak: "true" veri kümesinin veri fabrikasının dışında olduğunu ve veri fabrikasındaki bir etkinlik tarafından üretilmediğini Veri Fabrikası hizmetine bildirir.

```JSON
{
  "name": "AzureSqlDWInput",
  "properties": {
    "type": "AzureSqlDWTable",
    "linkedServiceName": "AzureSqlDWLinkedService",
    "typeProperties": {
      "tableName": "MyTable"
    },
    "external": true,
    "availability": {
      "frequency": "Hour",
      "interval": 1
    },
    "policy": {
      "externalData": {
        "retryInterval": "00:01:00",
        "retryTimeout": "00:10:00",
        "maximumRetry": 3
      }
    }
  }
}
```
**Azure Blob çıktı veri seti:**

Veriler her saat yeni bir blob 'a yazılır (sıklık: saat, aralık: 1). Blob için klasör yolu, işlenen dilimin başlangıç saatine göre dinamik olarak değerlendirilir. Klasör yolu, başlangıç zamanının yıl, ay, gün ve saat bölümlerini kullanır.

```JSON
{
  "name": "AzureBlobOutput",
  "properties": {
    "type": "AzureBlob",
    "linkedServiceName": "StorageLinkedService",
    "typeProperties": {
      "folderPath": "mycontainer/myfolder/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
      "partitionedBy": [
        {
          "name": "Year",
          "value": {
            "type": "DateTime",
            "date": "SliceStart",
            "format": "yyyy"
          }
        },
        {
          "name": "Month",
          "value": {
            "type": "DateTime",
            "date": "SliceStart",
            "format": "MM"
          }
        },
        {
          "name": "Day",
          "value": {
            "type": "DateTime",
            "date": "SliceStart",
            "format": "dd"
          }
        },
        {
          "name": "Hour",
          "value": {
            "type": "DateTime",
            "date": "SliceStart",
            "format": "HH"
          }
        }
      ],
      "format": {
        "type": "TextFormat",
        "columnDelimiter": "\t",
        "rowDelimiter": "\n"
      }
    },
    "availability": {
      "frequency": "Hour",
      "interval": 1
    }
  }
}
```

**SqlDWSource ve BlobSink ile bir ardışık ardışık alanda etkinliği kopyalama:**

Ardışık iş, giriş ve çıktı veri kümelerini kullanacak şekilde yapılandırılan ve her saat çalışacak şekilde zamanlanan bir Kopyalama Etkinliği içerir. Boru hattı JSON tanımında, **kaynak** türü **SqlDWSource** olarak ayarlanır ve **lavabo** türü **BlobSink**olarak ayarlanır. **SqlReaderQuery** özelliği için belirtilen SQL sorgusu, kopyalanacak son bir saat içinde verileri seçer.

```JSON
{
  "name":"SamplePipeline",
  "properties":{
    "start":"2014-06-01T18:00:00",
    "end":"2014-06-01T19:00:00",
    "description":"pipeline for copy activity",
    "activities":[
      {
        "name": "AzureSQLDWtoBlob",
        "description": "copy activity",
        "type": "Copy",
        "inputs": [
          {
            "name": "AzureSqlDWInput"
          }
        ],
        "outputs": [
          {
            "name": "AzureBlobOutput"
          }
        ],
        "typeProperties": {
          "source": {
            "type": "SqlDWSource",
            "sqlReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd HH:mm}\\' AND timestampcolumn < \\'{1:yyyy-MM-dd HH:mm}\\'', WindowStart, WindowEnd)"
          },
          "sink": {
            "type": "BlobSink"
          }
        },
        "scheduler": {
          "frequency": "Hour",
          "interval": 1
        },
        "policy": {
          "concurrency": 1,
          "executionPriorityOrder": "OldestFirst",
          "retry": 0,
          "timeout": "01:00:00"
        }
      }
    ]
  }
}
```
> [!NOTE]
> Örnekte, **sqlReaderQuery** SqlDWSource için belirtilir. Kopyalama Etkinliği, verileri almak için bu sorguyu Azure SQL Veri Ambarı kaynağına karşı çalıştırAr.
>
> Alternatif olarak, **sqlReaderStoredProcedureName** ve **storedProcedureParametreleri** (depolanan yordam parametreleri alıyorsa) belirterek depolanmış bir yordam belirtebilirsiniz.
>
> SqlReaderQuery veya sqlReaderStoredProcedureName'i belirtmezseniz, JSON veri kümesinin yapı bölümünde tanımlanan sütunlar, Azure SQL Veri Ambarı'na karşı çalışmak üzere bir sorgu oluşturmak için (sütun1, mytable'dan sütun2 seçin) kullanılır. Veri kümesi tanımıyapısı yoksa, tüm sütunlar tablodan seçilir.
>
>

### <a name="example-copy-data-from-azure-blob-to-azure-sql-data-warehouse"></a>Örnek: Azure Blob'dan Azure SQL Veri Ambarı'na veri kopyalama
Örnek, aşağıdaki Veri Fabrikası varlıklarını tanımlar:

1. [AzureSqlDW](#linked-service-properties)türünde bağlantılı bir hizmet.
2. [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties)türüne bağlı bir hizmet.
3. [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties)türünden bir giriş [veri kümesi.](data-factory-create-datasets.md)
4. [AzureSqlDWTable](#dataset-properties)türünden bir çıktı [veri kümesi.](data-factory-create-datasets.md)
5. [BlobSource](data-factory-azure-blob-connector.md#copy-activity-properties) ve [SqlDWSink](#copy-activity-properties)kullanan Kopyalama etkinliği olan bir [ardışık kaynak.](data-factory-create-pipelines.md)

Örnek, zaman serisi verilerini (saatlik, günlük vb.) Azure blob'undan Azure SQL Veri Ambarı veritabanındaki bir tabloya her saat kopyalar. Bu örneklerde kullanılan JSON özellikleri, örnekleri izleyen bölümlerde açıklanmıştır.

**Azure SQL Veri Ambarı bağlantılı hizmet:**

```JSON
{
  "name": "AzureSqlDWLinkedService",
  "properties": {
    "type": "AzureSqlDW",
    "typeProperties": {
      "connectionString": "Server=tcp:<servername>.database.windows.net,1433;Database=<databasename>;User ID=<username>@<servername>;Password=<password>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
    }
  }
}
```
**Azure Blob depolama bağlantılı hizmet:**

```JSON
{
  "name": "StorageLinkedService",
  "properties": {
    "type": "AzureStorage",
    "typeProperties": {
      "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
    }
  }
}
```
**Azure Blob giriş veri kümesi:**

Veriler her saat yeni bir damladan alınır (sıklık: saat, aralık: 1). Blob için klasör yolu ve dosya adı, işlenen dilimin başlangıç saatine göre dinamik olarak değerlendirilir. Klasör yolu, başlangıç zamanının yıl, ay ve gün kısmını kullanır ve dosya adı başlangıç zamanının saat kısmını kullanır. "dış": "true" ayarı, veri fabrikasının dışında olduğunu ve veri fabrikasındaki bir etkinlik tarafından üretilmediğini Veri Fabrikası hizmetine bildirir.

```JSON
{
  "name": "AzureBlobInput",
  "properties": {
    "type": "AzureBlob",
    "linkedServiceName": "StorageLinkedService",
    "typeProperties": {
      "folderPath": "mycontainer/myfolder/yearno={Year}/monthno={Month}/dayno={Day}",
      "fileName": "{Hour}.csv",
      "partitionedBy": [
        {
          "name": "Year",
          "value": {
            "type": "DateTime",
            "date": "SliceStart",
            "format": "yyyy"
          }
        },
        {
          "name": "Month",
          "value": {
            "type": "DateTime",
            "date": "SliceStart",
            "format": "MM"
          }
        },
        {
          "name": "Day",
          "value": {
            "type": "DateTime",
            "date": "SliceStart",
            "format": "dd"
          }
        },
        {
          "name": "Hour",
          "value": {
            "type": "DateTime",
            "date": "SliceStart",
            "format": "HH"
          }
        }
      ],
      "format": {
        "type": "TextFormat",
        "columnDelimiter": ",",
        "rowDelimiter": "\n"
      }
    },
    "external": true,
    "availability": {
      "frequency": "Hour",
      "interval": 1
    },
    "policy": {
      "externalData": {
        "retryInterval": "00:01:00",
        "retryTimeout": "00:10:00",
        "maximumRetry": 3
      }
    }
  }
}
```
**Azure SQL Veri Ambarı çıktı veri kümesi:**

Örnek, verileri Azure SQL Veri Ambarı'nda "MyTable" adlı bir tabloya kopyalar. Tabloyu Azure SQL Veri Ambarı'nda Blob CSV dosyasının içermesini beklediğiniz sütunlarla aynı sayıda sütunla oluşturun. Her saat başı tabloya yeni satırlar eklenir.

```JSON
{
  "name": "AzureSqlDWOutput",
  "properties": {
    "type": "AzureSqlDWTable",
    "linkedServiceName": "AzureSqlDWLinkedService",
    "typeProperties": {
      "tableName": "MyOutputTable"
    },
    "availability": {
      "frequency": "Hour",
      "interval": 1
    }
  }
}
```
**BlobSource ve SqlDWSink ile bir ardışık ardışık alanda etkinliği kopyalama:**

Ardışık iş, giriş ve çıktı veri kümelerini kullanacak şekilde yapılandırılan ve her saat çalışacak şekilde zamanlanan bir Kopyalama Etkinliği içerir. Boru hattı JSON tanımında, **kaynak** türü **BlobSource** olarak ayarlanır ve **lavabo** türü **SqlDWSink**olarak ayarlanır.

```JSON
{
  "name":"SamplePipeline",
  "properties":{
    "start":"2014-06-01T18:00:00",
    "end":"2014-06-01T19:00:00",
    "description":"pipeline with copy activity",
    "activities":[
      {
        "name": "AzureBlobtoSQLDW",
        "description": "Copy Activity",
        "type": "Copy",
        "inputs": [
          {
            "name": "AzureBlobInput"
          }
        ],
        "outputs": [
          {
            "name": "AzureSqlDWOutput"
          }
        ],
        "typeProperties": {
          "source": {
            "type": "BlobSource",
            "blobColumnSeparators": ","
          },
          "sink": {
            "type": "SqlDWSink",
            "allowPolyBase": true
          }
        },
        "scheduler": {
          "frequency": "Hour",
          "interval": 1
        },
        "policy": {
          "concurrency": 1,
          "executionPriorityOrder": "OldestFirst",
          "retry": 0,
          "timeout": "01:00:00"
        }
      }
    ]
  }
}
```
Bir gözden geçirme için, Azure Veri Ambarı ile Azure Veri Fabrikası ve Azure Veri Ambarı [makalesiyle verileri Azure](../../sql-data-warehouse/sql-data-warehouse-get-started-load-with-azure-data-factory.md) SQL Veri Ambarı belgelerinde yükleyin ile [15 dakikanın altında Azure SQL Veri](data-factory-load-sql-data-warehouse.md) Ambarı'na Yükleyin'e bakın.

## <a name="performance-and-tuning"></a>Performans ve Tuning
Azure Veri Fabrikası'ndaki veri hareketinin performansını etkileyen önemli faktörler (Kopyalama Etkinliği) ve bunu optimize etmenin çeşitli yolları hakkında bilgi edinmek için [Etkinlik performansını & Tuning Kılavuzu'na](data-factory-copy-activity-performance.md) bakın.
