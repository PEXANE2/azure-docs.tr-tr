---
title: Azure Blob Depolama'ya/Azure'dan veri kopyalama
description: "Azure Veri Fabrikası'nda blob verilerini nasıl kopyalayın öğrenin. Örneğimizi kullanın: Azure Blob Depolama ve Azure SQL Veritabanı'na veri kopyalama ve kullanma nasıl yapılır."
services: data-factory
documentationcenter: ''
author: linda33wj
manager: shwang
ms.assetid: bec8160f-5e07-47e4-8ee1-ebb14cfb805d
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 01/05/2018
ms.author: jingwang
robots: noindex
ms.openlocfilehash: eab332f102b9e39981e2d8ed6e84f73fada87a1a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79282139"
---
# <a name="copy-data-to-or-from-azure-blob-storage-using-azure-data-factory"></a>Azure Veri Fabrikası'nı kullanarak Azure Blob Depolama'ya veya Azure Blob Depolama'dan veri kopyalama
> [!div class="op_single_selector" title1="Kullandığınız Veri Fabrikası hizmetisürümünü seçin:"]
> * [Sürüm 1](data-factory-azure-blob-connector.md)
> * [Sürüm 2 (geçerli sürüm)](../connector-azure-blob-storage.md)

> [!NOTE]
> Bu makale, Data Factory’nin 1. sürümü için geçerlidir. Veri Fabrikası hizmetinin geçerli sürümünü kullanıyorsanız, [V2'deki Azure Blob Depolama bağlayıcısına](../connector-azure-blob-storage.md)bakın.


Bu makalede, Azure Blob Depolama'ya ve Azure'dan veri kopyalamak için Azure Veri Fabrikası'ndaki Kopyalama Etkinliği'nin nasıl kullanılacağı açıklanmaktadır. Kopya etkinliğiyle birlikte veri hareketine genel bir genel bakış sunan [Veri Hareketi Etkinlikleri](data-factory-data-movement-activities.md) makalesine dayanmaktadır.

## <a name="overview"></a>Genel Bakış
Desteklenen herhangi bir kaynak veri deposundan Azure Blob Depolama'ya veya Azure Blob Depolama'dan desteklenen herhangi bir lavabo veri deposuna veri kopyalayabilirsiniz. Aşağıdaki tablo, kopyalama etkinliği tarafından kaynak olarak desteklenen veya battığı veri depolarının bir listesini sağlar. Örneğin, verileri SQL Server veritabanından veya Azure SQL **veritabanından** Azure blob **depolamasına** taşıyabilirsiniz. Ayrıca, Azure blob **depolamasundaki** verileri **bir** Azure SQL Veri Ambarı'na veya Azure Cosmos DB koleksiyonuna kopyalayabilirsiniz.

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

## <a name="supported-scenarios"></a>Desteklenen senaryolar
**Azure Blob Depolama'daki** verileri aşağıdaki veri depolarına kopyalayabilirsiniz:

[!INCLUDE [data-factory-supported-sink](../../../includes/data-factory-supported-sinks.md)]

Aşağıdaki veri depolarından Azure **Blob Depolama'ya**veri kopyalayabilirsiniz:

[!INCLUDE [data-factory-supported-sources](../../../includes/data-factory-supported-sources.md)]

> [!IMPORTANT]
> Kopyalama Etkinliği, genel amaçlı Azure Depolama hesaplarından ve Sıcak/Cool Blob depolamasından/verilerinden kopyalanmasını destekler. Etkinlik **blok, ek veya sayfa lekeleri okuma**destekler, ancak **sadece blobs engellemek için yazmayı**destekler. Azure Premium Depolama, sayfa lekeleri tarafından desteklendirilebildiği için lavabo olarak desteklenmez.
>
> Kopyalama Etkinliği, veriler hedefe başarıyla kopyalandıktan sonra verileri kaynaktan silmez. Başarılı bir kopyadan sonra kaynak verileri silmeniz gerekiyorsa, verileri silmek ve ardışık düzendeki etkinliği kullanmak için özel bir [etkinlik](data-factory-use-custom-activities.md) oluşturun. Örneğin, [GitHub'daki Delete blob veya klasör örneğine](https://github.com/Azure/Azure-DataFactory/tree/master/SamplesV1/DeleteBlobFileFolderCustomActivity)bakın.

## <a name="get-started"></a>Kullanmaya başlayın
Farklı araçlar/API'ler kullanarak verileri Azure Blob Depolamasına/veya verilerinden aktaran bir kopyalama etkinliği içeren bir ardışık kaynak oluşturabilirsiniz.

Bir ardışık yol oluşturmanın en kolay yolu **Kopyalama Sihirbazı'nı**kullanmaktır. Bu makalede, bir Azure Blob Depolama konumundan başka bir Azure Blob Depolama konumuna veri kopyalamak için bir ardışık yol oluşturma için bir [izthrough](#walkthrough-use-copy-wizard-to-copy-data-tofrom-blob-storage) vardır. Azure Blob Depolama'dan Azure SQL Veritabanı'na veri kopyalamak için bir ardışık yol üzerinde bilgi hattı oluşturma konusunda bir öğretici için [Bkz.](data-factory-copy-data-wizard-tutorial.md)

Bir ardışık kaynak oluşturmak için aşağıdaki araçları da kullanabilirsiniz: **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager şablonu**, **.NET API**ve **REST API**. Kopyalama etkinliği içeren bir ardışık hatlar oluşturmak için adım adım yönergeleri için [etkinlik öğreticisini](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) kopyala'ya bakın.

Araçları veya API'leri kullanın, verileri kaynak veri deposundan bir lavabo veri deposuna aktaran bir ardışık işlem hattı oluşturmak için aşağıdaki adımları gerçekleştirirsiniz:

1. Bir **veri fabrikası**oluşturun. Bir veri fabrikası bir veya daha fazla ardışık hat lar içerebilir.
2. Giriş ve çıktı veri depolarını veri fabrikanıza bağlamak için **bağlantılı hizmetler** oluşturun. Örneğin, bir Azure blob depolama sundaki verileri Azure SQL veritabanına kopyalıyorsanız, Azure depolama hesabınızı ve Azure SQL veritabanınızı veri fabrikanıza bağlamak için iki bağlantılı hizmet oluşturursunuz. Azure Blob Depolama'ya özgü bağlantılı hizmet özellikleri için [bağlantılı hizmet özellikleri](#linked-service-properties) bölümüne bakın.
2. Kopyalama işlemi için giriş ve çıktı verilerini temsil edecek **veri kümeleri** oluşturun. Son adımda belirtilen örnekte, giriş verilerini içeren blob kapsayıcısını ve klasörünü belirtmek için bir veri kümesi oluşturursunuz. Ayrıca, Azure SQL veritabanında blob depolamadan kopyalanan verileri tutan SQL tablosunu belirtmek için başka bir veri kümesi oluşturursunuz. Azure Blob Depolama'ya özgü veri kümesi özellikleri için [veri kümesi özellikleri](#dataset-properties) bölümüne bakın.
3. Giriş olarak veri kümesi ve çıktı olarak veri kümesi alan bir kopyalama etkinliği içeren bir **ardışık işlem oluşturma.** Daha önce bahsedilen örnekte, blobSource'u kaynak olarak, SqlSink'i ise kopyalama etkinliği için lavabo olarak kullanırsınız. Benzer şekilde, Azure SQL Veritabanı'ndan Azure Blob Depolama'ya kopyalıyorsanız, kopyalama etkinliğinde SqlSource ve BlobSink'i kullanırsınız. Azure Blob Depolama'ya özgü kopyalama etkinliği özellikleri için [kopyalama etkinlik özellikleri](#copy-activity-properties) bölümüne bakın. Kaynak veya lavabo olarak veri deposunun nasıl kullanılacağı yla ilgili ayrıntılar için, veri deponuzun önceki bölümündeki bağlantıyı tıklatın.

Sihirbazı kullandığınızda, bu Veri Fabrikası varlıkları (bağlantılı hizmetler, veri kümeleri ve ardışık kuruluş) için JSON tanımları sizin için otomatik olarak oluşturulur. Araçları/API'leri (.NET API hariç) kullandığınızda, Bu Veri Fabrikası varlıklarını JSON biçimini kullanarak tanımlarsınız.  Azure Blob Depolama'ya/Azure Blob Depolamasına veri kopyalamak için kullanılan Veri Fabrikası varlıkları için JSON tanımlı örnekler için bu makalenin [JSON örnekleri](#json-examples-for-copying-data-to-and-from-blob-storage  ) bölümüne bakın.

Aşağıdaki bölümlerde, Azure Blob Depolamasına özgü Veri Fabrikası varlıklarını tanımlamak için kullanılan JSON özellikleri hakkında ayrıntılar sağlanmaktadır.

## <a name="linked-service-properties"></a>Bağlantılı hizmet özellikleri
Azure Depolama'yı Azure veri fabrikasına bağlamak için kullanabileceğiniz iki tür bağlantılı hizmet vardır. Bunlar: **AzureStorage** bağlantılı hizmet ve **AzureStorageSas** bağlantılı hizmet. Azure Depolama bağlantılı hizmet, veri fabrikasına Azure Depolama'ya genel erişim sağlar. Ancak Azure Depolama SAS (Paylaşılan Erişim İmzası) bağlantılı hizmet, veri fabrikasına Azure Depolama'ya sınırlı/zamana bağlı erişim sağlar. Bu iki bağlantılı hizmet arasında başka bir fark yoktur. İhtiyaçlarınıza uygun bağlantılı hizmeti seçin. Aşağıdaki bölümlerde bu iki bağlantılı hizmet hakkında daha fazla ayrıntı sağlayabilirsiniz.

[!INCLUDE [data-factory-azure-storage-linked-services](../../../includes/data-factory-azure-storage-linked-services.md)]

## <a name="dataset-properties"></a>Veri kümesi özellikleri
Azure Blob Depolama'daki giriş veya çıktı verilerini temsil edecek bir veri kümesi belirtmek için, veri kümesinin tür özelliğini şu şekilde ayarlarsınız: **AzureBlob**. Veri kümesinin **linkedServiceName** özelliğini Azure Depolama veya Azure Depolama SAS bağlantılı hizmetin adına ayarlayın.  Veri kümesinin tür özellikleri **blob kapsayıcısını** ve blob depolamasındaki **klasörü** belirtir.

Veri kümelerini tanımlamak için kullanılabilen JSON bölümleri& özelliklerinin tam listesi için [veri kümelerini oluşturma](data-factory-create-datasets.md) makalesine bakın. Bir veri kümesi JSON'un yapısı, kullanılabilirliği ve ilkesi gibi bölümler tüm veri kümesi türleri (Azure SQL, Azure blob, Azure tablosu, vb.) için benzerdir.

Veri fabrikası, Azure blob: Int16, Int32, Int64, Single, Double, Ondalık, Bayt[], Bool, String, Guid, Datetime, Datetimeoffset, Zaman Pan. Veri Fabrikası, verileri bir kaynak veri deposundan lavabo veri deposuna taşınırken tür dönüşümlerini otomatik olarak gerçekleştirir.

**TypeProperties** bölümü her veri kümesi türü için farklıdır ve veri deposundaki verilerin konumu, biçimi vb. hakkında bilgi sağlar. **AzureBlob** veri kümesinin veri kümesi için typeProperties bölümü aşağıdaki özelliklere sahiptir:

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| folderPath |Blob depolama sındaki kapsayıcı ve klasöre giden yol. Örnek: myblobcontainer\myblobfolder\ |Evet |
| fileName |Lekenin adı. fileName isteğe bağlıdır ve büyük/küçük harf duyarlıdır.<br/><br/>Bir dosya adı belirtirseniz, etkinlik (Kopya dahil) belirli Blob'da çalışır.<br/><br/>fileName belirtilmediğinde, Copy giriş veri kümesi için folderPath'teki tüm Blob'ları içerir.<br/><br/>Bir çıktı veri kümesi için **dosya Adı** belirtilmediğinde ve **korumaHiyerarşisi** etkinlik lavabosunda belirtilmediğinde, `Data.<Guid>.txt` oluşturulan dosyanın adı aşağıdaki biçimde olacaktır: (örneğin: : Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt |Hayır |
| bölümlemeBy |partitionedBy isteğe bağlı bir özelliktir. Zaman serisi verileri için dinamik bir klasörPath ve dosya adı belirtmek için kullanabilirsiniz. Örneğin, folderPath her saatlik veri için parametreli olabilir. Ayrıntılar ve örnekler için [bölümleme özelliğini kullanma bölümüne](#using-partitionedby-property) bakın. |Hayır |
| biçim | Aşağıdaki biçim türleri desteklenir: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParkeFormat**. Biçim altındaki **tür** özelliğini bu değerlerden birine ayarlayın. Daha fazla bilgi için [Metin Biçimi,](data-factory-supported-file-and-compression-formats.md#text-format) [Json Formatı,](data-factory-supported-file-and-compression-formats.md#json-format) [Avro Biçimi,](data-factory-supported-file-and-compression-formats.md#avro-format) [Ork Biçimi](data-factory-supported-file-and-compression-formats.md#orc-format)ve [Parke Biçimi](data-factory-supported-file-and-compression-formats.md#parquet-format) bölümlerine bakın. <br><br> Dosyaları dosya tabanlı mağazalar (ikili kopya) arasında **olduğu gibi kopyalamak** istiyorsanız, hem giriş hem de çıktı veri kümesi tanımlarında biçim bölümünü atlayın. |Hayır |
| sıkıştırma | Verilerin sıkıştırma türünü ve düzeyini belirtin. Desteklenen türleri şunlardır: **GZip**, **Deflate**, **BZip2**, ve **ZipDeflate**. Desteklenen seviyeler şunlardır: **Optimal** ve **En Hızlı.** Daha fazla bilgi için [Azure Veri Fabrikası'ndaki Dosya ve sıkıştırma biçimlerine](data-factory-supported-file-and-compression-formats.md#compression-support)bakın. |Hayır |

### <a name="using-partitionedby-property"></a>PartitionedBy özelliğini kullanma
Önceki bölümde belirtildiği gibi, **bölümlenmişBy** özelliği, [Veri Fabrikası işlevleri ve sistem değişkenleri](data-factory-functions-variables.md)ile zaman serisi verileri için dinamik bir klasörPath ve dosya adı belirtebilirsiniz.

Zaman serisi veri kümeleri, zamanlama ve dilimler hakkında daha fazla bilgi için [&](data-factory-scheduling-and-execution.md) [bkz.](data-factory-create-datasets.md)

#### <a name="sample-1"></a>Örnek 1

```json
"folderPath": "wikidatagateway/wikisampledataout/{Slice}",
"partitionedBy":
[
    { "name": "Slice", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyyMMddHH" } },
],
```

Bu örnekte, {Slice} veri fabrikası sistem değişkeni SliceStart değeri ile değiştirilir formatta (YYYYMMDDHH) belirtilir. SliceStart, dilimin başlangıç saatini ifade eder. FolderPath her dilim için farklıdır. Örneğin: wikidatagateway/wikisampledataout/2014100103 veya wikidatagateway/wikisampledataout/2014100104

#### <a name="sample-2"></a>Örnek 2

```json
"folderPath": "wikidatagateway/wikisampledataout/{Year}/{Month}/{Day}",
"fileName": "{Hour}.csv",
"partitionedBy":
[
    { "name": "Year", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyy" } },
    { "name": "Month", "value": { "type": "DateTime", "date": "SliceStart", "format": "MM" } },
    { "name": "Day", "value": { "type": "DateTime", "date": "SliceStart", "format": "dd" } },
    { "name": "Hour", "value": { "type": "DateTime", "date": "SliceStart", "format": "hh" } }
],
```

Bu örnekte, SliceStart'ın yılı, ayı, günü ve saati folderPath ve fileName özellikleri tarafından kullanılan ayrı değişkenlere ayıklanır.

## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri
Etkinlikleri tanımlamak için kullanılabilen bölümlerin & özelliklerinin tam listesi [için, Kaynak Hatları Oluşturma](data-factory-create-pipelines.md) makalesine bakın. Ad, açıklama, giriş ve çıktı veri kümeleri ve ilkeler gibi özellikler tüm etkinlik türleri için kullanılabilir. Oysa, etkinliğin **typeProperties** bölümünde bulunan özellikler her etkinlik türüne göre değişir. Kopyalama etkinliği için, kaynak ve lavabo türlerine bağlı olarak değişir. Verileri bir Azure Blob Depolama alanından taşıyorsanız, kopyalama etkinliğindeki kaynak türünü **BlobSource**olarak ayarlarsınız. Benzer şekilde, verileri Azure Blob Depolama alanına taşıyorsanız, kopyalama etkinliğindeki lavabo türünü **BlobSink**olarak ayarlarsınız. Bu bölümde BlobSource ve BlobSink tarafından desteklenen özelliklerin bir listesini sağlar.

**BlobSource** **typeProperties** bölümünde aşağıdaki özellikleri destekler:

| Özellik | Açıklama | İzin verilen değerler | Gerekli |
| --- | --- | --- | --- |
| Özyinelemeli |Verilerin alt klasörlerden mi yoksa yalnızca belirtilen klasörden mi özyinelemeli olarak okunduğunu gösterir. |True (varsayılan değer), False |Hayır |

**BlobSink** aşağıdaki özellikleri **typeProperties** bölümünü destekler:

| Özellik | Açıklama | İzin verilen değerler | Gerekli |
| --- | --- | --- | --- |
| copyBehavior |Kaynak BlobSource veya FileSystem olduğunda kopyalama davranışını tanımlar. |<b>PreserveHiyerarşi</b>: hedef klasördeki dosya hiyerarşisini korur. Kaynak dosyanın kaynak klasörüne göreli yolu hedef klasöre hedef dosyanın göreli yolu ile aynıdır.<br/><br/><b>FlattenHiyerarşiy</b>: kaynak klasöründen tüm dosyalar hedef klasörün ilk düzeyindedir. Hedef dosyaların otomatik olarak oluşturulan adı var. <br/><br/><b>MergeFiles</b>: kaynak klasördeki tüm dosyaları tek bir dosyayla birleştirir. Dosya/Blob Adı belirtilirse, birleştirilen dosya adı belirtilen ad olur; aksi takdirde, otomatik olarak oluşturulan dosya adı olacaktır. |Hayır |

**BlobSource** da geriye dönük uyumluluk için bu iki özelliği destekler.

* **treatEmptyAsNull**: Null veya boş dizeyi null değer olarak ele alıp adedinin ele alıp ait olmadığını belirtir.
* **skipHeaderLineCount** - Kaç satır Atlanmalıdır belirtir. Yalnızca giriş veri kümesi TextFormat'ı kullanırken uygulanabilir.

Benzer şekilde, **BlobSink** geriye dönük uyumluluk için aşağıdaki özelliği destekler.

* **blobWriterAddHeader**: Çıktı veri kümesine yazarken sütun tanımlarının üstbilgisini ekleyip eklemeyin ilerler.

Veri kümeleri artık aynı işlevi uygulayan aşağıdaki özellikleri destekler: **treatEmptyAsNull**, **skipLineCount**, **firstRowAsHeader**.

Aşağıdaki tablo, bu blob kaynak/lavabo özellikleri yerine yeni veri kümesi özelliklerini kullanma konusunda kılavuz sağlar.

| Etkinlik özelliğini kopyalama | Dataset özelliği |
|:--- |:--- |
| SkipHeaderLineCount BlobSource üzerinde |skipLineCount ve firstRowAsHeader. Satırlar önce atlanır ve sonra ilk satır üstbilgi olarak okunur. |
| blobSource üzerinde treatEmptyAsNull |giriş veri setinde EmptyAsNull'u tedavi |
| blobWriterAddHeader üzerinde BlobSink |çıkış veri setinde firstRowAsHeader |

Bkz. Bu özellikler hakkında ayrıntılı bilgi için TextFormat bölümünü [belirtme.](data-factory-supported-file-and-compression-formats.md#text-format)

### <a name="recursive-and-copybehavior-examples"></a>özyinelemeli ve copyBehavior örnekleri
Bu bölümde, özyinelemeli ve copyBehavior değerlerinin farklı birleşimleri için Kopyalama işleminin ortaya çıkan davranışı açıklanmaktadır.

| Özyinelemeli | copyBehavior | Sonuç davranış |
| --- | --- | --- |
| true |korumaHiyerarşisi |Aşağıdaki yapıya sahip bir kaynak klasör Folder1 için: <br/><br/>Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Alt klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya5<br/><br/>hedef klasör Folder1 kaynak la aynı yapıile oluşturulur<br/><br/>Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Alt klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya5. |
| true |flattenHiyerarşi |Aşağıdaki yapıya sahip bir kaynak klasör Folder1 için: <br/><br/>Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Alt klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya5<br/><br/>hedef Folder1 aşağıdaki yapı ile oluşturulur: <br/><br/>Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1 için otomatik olarak oluşturulan ad<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2 için otomatik olarak oluşturulan ad<br/>&nbsp;&nbsp;&nbsp;&nbsp;File3 için otomatik olarak oluşturulan ad<br/>&nbsp;&nbsp;&nbsp;&nbsp;File4 için otomatik olarak oluşturulan ad<br/>&nbsp;&nbsp;&nbsp;&nbsp;File5 için otomatik olarak oluşturulan ad |
| true |birleştirmeDosyaları |Aşağıdaki yapıya sahip bir kaynak klasör Folder1 için: <br/><br/>Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Alt klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya5<br/><br/>hedef Folder1 aşağıdaki yapı ile oluşturulur: <br/><br/>Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1 + File2 + File3 + File4 + Dosya 5 içeriği otomatik oluşturulan dosya adı ile bir dosya da birleştirilir |
| yanlış |korumaHiyerarşisi |Aşağıdaki yapıya sahip bir kaynak klasör Folder1 için: <br/><br/>Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Alt klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya5<br/><br/>hedef klasör Folder1 aşağıdaki yapı ile oluşturulur<br/><br/>Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2<br/><br/><br/>Dosya3, File4 ve File5 ile alt klasör1 alınmaz. |
| yanlış |flattenHiyerarşi |Aşağıdaki yapıya sahip bir kaynak klasör Folder1 için:<br/><br/>Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Alt klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya5<br/><br/>hedef klasör Folder1 aşağıdaki yapı ile oluşturulur<br/><br/>Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1 için otomatik olarak oluşturulan ad<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2 için otomatik olarak oluşturulan ad<br/><br/><br/>Dosya3, File4 ve File5 ile alt klasör1 alınmaz. |
| yanlış |birleştirmeDosyaları |Aşağıdaki yapıya sahip bir kaynak klasör Folder1 için:<br/><br/>Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Alt klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya5<br/><br/>hedef klasör Folder1 aşağıdaki yapı ile oluşturulur<br/><br/>Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1 + File2 içeriği otomatik olarak oluşturulan dosya adı içeren bir dosyada birleştirilir. File1 için otomatik olarak oluşturulan ad<br/><br/>Dosya3, File4 ve File5 ile alt klasör1 alınmaz. |

## <a name="walkthrough-use-copy-wizard-to-copy-data-tofrom-blob-storage"></a>Walkthrough: Blob Depolama'ya/Blob Depolama'dan verileri kopyalamak için Kopyalama Sihirbazı'nı kullanın
Azure blob depolama alanına/verilerin hızlı bir şekilde nasıl kopyalanabildiğini inceleyelim. Bu izne göre, hem kaynak hem de hedef veri türünün depoları: Azure Blob Depolama. Bu izetme deki ardışık işlem, verileri bir klasörden aynı blob kapsayıcısındabaşka bir klasöre kopyalar. Bu izbin, Blob Depolama'yı kaynak veya lavabo olarak kullanırken ayarları veya özellikleri göstermek için kasıtlı olarak basittir.

### <a name="prerequisites"></a>Ön koşullar
1. Zaten hesabınız yoksa genel amaçlı bir **Azure Depolama Hesabı** oluşturun. Bu izbinde blob depolamasını hem **kaynak** hem de **hedef** veri deposu olarak kullanırsınız. Azure depolama hesabınız yoksa, bir tane oluşturma adımları için [bir depolama hesabı oluştur](../../storage/common/storage-account-create.md) makalesine bakın.
2. Depolama hesabında **adfblobconnector** adında bir blob kapsayıcıoluşturun.
4. **Adfblobconnector** kapsayıcısında **giriş** adlı bir klasör oluşturun.
5. Aşağıdaki içerikle **emp.txt** adında bir dosya oluşturun ve [Azure Depolama Gezgini](https://azurestorageexplorer.codeplex.com/) gibi araçları kullanarak **giriş** klasörüne yükleyin
    ```json
    John, Doe
    Jane, Doe
    ```

### <a name="create-the-data-factory"></a>Veri fabrikasını oluşturma
1. [Azure portalında](https://portal.azure.com)oturum açın.
2. Sol üst köşeden **kaynak oluştur'u** tıklatın, **İstihbarat + analitik'i**tıklatın ve **Veri Fabrikası'nı**tıklatın.
3. Yeni **veri fabrikası** bölmesinde:  
    1. **Ad**için **ADFBlobConnectorDF** girin. Azure veri fabrikasının adı genel olarak benzersiz olmalıdır. Hata alırsanız: `*Data factory name “ADFBlobConnectorDF” is not available`, veri fabrikasının adını değiştirin (örneğin, adınızADFBlobConnectorDF) ve yeniden oluşturmayı deneyin. Data Factory yapıtlarının adlandırma kuralları için [Data Factory - Adlandırma Kuralları](data-factory-naming-rules.md) konusuna bakın.
    2. Azure **aboneliğinizi**seçin.
    3. Kaynak Grubu için, varolan bir kaynak grubunu seçmek için **Varolan'ı Kullan** 'ı (veya) kaynak grubu için ad girmek için **yeni oluştur'u** seçin' i seçin.
    4. Veri fabrikası için bir **konum** seçin.
    5. Dikey pencerenin alt kısmındaki **Panoya sabitle** onay kutusunu seçin.
    6. **Oluştur'u**tıklatın.
3. Oluşturma tamamlandıktan sonra, aşağıdaki resimde gösterildiği gibi **Veri** Fabrikası ![bıçak bakın: Veri fabrikası ana sayfası](./media/data-factory-azure-blob-connector/data-factory-home-page.png)

### <a name="copy-wizard"></a>Kopyalama Sihirbazı
1. Veri Fabrikası ana sayfasında, **Copy data** Veri Kopyala'yı tıklattığınızda **Veri Sihirbazı'nı** ayrı bir sekmede başlatın.  

    > [!NOTE]
    > Web tarayıcısının "Yetkilendirme..."nde sıkışıp kaldığını görürseniz, üçüncü **taraf tanımlama bilgilerini ve site veri** ayarını engelle/kaldır (veya) etkinleştirin ve **login.microsoftonline.com** için bir özel durum oluşturun ve ardından sihirbazı yeniden başlatmayı deneyin.
2. **Özellikler** sayfasında:
    1. **Görev adı**için **CopyPipeline** girin. Görev adı, veri fabrikanızdaki ardışık hattın adıdır.
    2. Görev için **açıklama** girin (isteğe bağlı).
    3. **Görev cadence veya Görev zamanlaması** **için, Çalıştır'ı düzenli olarak zamanlama seçeneğinde** tutun. Bu görevi zamanlamada tekrar tekrar çalıştırmak yerine yalnızca bir kez çalıştırmak istiyorsanız, **şimdi bir kez Çalıştır'ı**seçin. Seçerseniz, **şimdi bir kez çalıştır** seçeneğini belirlerseniz, tek seferlik bir [ardışık lık](data-factory-create-pipelines.md#onetime-pipeline) oluşturulur.
    4. **Yinelenen desen**ayarlarını tutun. Bu görev, bir sonraki adımda belirttiğiniz başlangıç ve bitiş saatleri arasında günlük olarak çalışır.
    5. Başlangıç **tarihi** **21/04/2017**olarak değiştirin.
    6. Bitiş **tarihini** **25/04/2017**olarak değiştirin. Takvime göz atmak yerine tarihi yazmak isteyebilirsiniz.
    8. **İleri**'ye tıklayın.
        ![Kopyalama Aracı - Özellikler sayfası](./media/data-factory-azure-blob-connector/copy-tool-properties-page.png)
3. **Kaynak veri deposu** sayfasında **Azure Blob Storage** kutucuğuna tıklayın. Kopyalama görevine yönelik kaynak veri deposunu belirtmek için bu sayfayı kullanın. Yeni bir veri deposu belirtmek için mevcut bir veri deposu bağlı hizmetini kullanabilirsiniz (veya) yeni bir veri deposu belirtebilirsiniz. Varolan bir bağlantılı hizmeti kullanmak için **VAROLAN BAĞLANTI HİzMETLerİnDEN** SEÇİn VE doğru bağlantılı hizmeti seçersiniz.
    ![Kopyalama Aracı - Kaynak veri deposu sayfası](./media/data-factory-azure-blob-connector/copy-tool-source-data-store-page.png)
4. **Azure Blob depolama hesabı belirtin** sayfasında:
    1. **Bağlantı adı**için otomatik oluşturulan adı tutun. Bağlantı adı, türünün bağlantılı hizmetinin adıdır: Azure Depolama.
    2. **Hesap seçme yöntemi** için **Azure aboneliklerinden** seçeneğinin belirlendiğini onaylayın.
    3. Azure aboneliğinizi seçin veya **Azure aboneliği**için **tümünü seç'i** tutun.
    4. Seçili abonelikte bulunan Azure depolama hesapları listesinden bir **Azure depolama hesabı** seçin. **Ayrıca, Hesap seçimi yöntemi**için **el ile Gir** seçeneğini seçerek depolama hesabı ayarlarını el ile girmeyi de seçebilirsiniz.
    5. **İleri**'ye tıklayın.  
        ![Kopyalama Aracı - Azure Blob depolama hesabı belirtin](./media/data-factory-azure-blob-connector/copy-tool-specify-azure-blob-storage-account.png)
5. **Girdi dosyası veya klasörü seçin** sayfasında:
    1. Çift **tıklamaadfblobcontainer**.
    2. **Giriş'i**seçin ve **Seç'i**tıklatın. Bu izbin de, giriş klasörünü seçin. Bunun yerine klasördeki emp.txt dosyasını da seçebilirsiniz.
        ![Kopyalama Aracı - Girdi dosyası veya klasörü seçin](./media/data-factory-azure-blob-connector/copy-tool-choose-input-file-or-folder.png)
6. Giriş **dosyasını veya klasör sayfasını seç:**
    1. **Dosyanın veya klasörün** **adfblobconnector/input**olarak ayarlandığına onaylayın. Dosyalar alt klasörlerdeyse, örneğin, 2017/04/01, 2017/04/02 ve benzeri durumlarda, dosya veya klasör için adfblobconnector/input/{year}/{month}/{day} girin. TEXT box'tan TAB tuşuna bastığınızda, yıl (yyyy), ay (MM) ve gün (dd) biçimlerini seçmek için üç açılır liste görürsünüz.
    2. **Kopya dosyasını özyinelemeyle**ayarlamayın. Hedefe kopyalanacak dosyalar için klasörler arasında özyinelemeli olarak geçiş yapmak için bu seçeneği belirleyin.
    3. **İkili kopyalama** seçeneğini yapmayın. Hedefe kaynak dosyanın ikili kopyasını gerçekleştirmek için bu seçeneği seçin. Sonraki sayfalarda daha fazla seçenek görebilmeniz için bu izliği seçmeyin.
    4. **Sıkıştırma türünün** **Yok**olarak ayarlı olduğunu doğrulayın. Kaynak dosyalarınız desteklenen biçimlerden birinde sıkıştırılmışsa, bu seçenek için bir değer seçin.
    5. **İleri**'ye tıklayın.
    ![Kopyalama Aracı - Girdi dosyası veya klasörü seçin](./media/data-factory-azure-blob-connector/chose-input-file-folder.png)
7. **Dosya biçimi ayarları** sayfasında sınırlayıcıları ve sihirbaz tarafından dosya ayrıştırılarak otomatik olarak algılanan düzeni görürsünüz.
    1. Aşağıdaki seçenekleri onaylayın:  
        a. **Dosya biçimi** Metin **biçimine**ayarlanır. Desteklenen tüm biçimleri açılır listede görebilirsiniz. Örneğin: JSON, Avro, ORC, Parke.
       b. **Sütun delimiter'** ı `Comma (,)`. Açılan listede Veri Fabrikası tarafından desteklenen diğer sütun sınırlayıcılarını görebilirsiniz. Ayrıca özel bir sınır layıcı belirtebilirsiniz.
       c. **Satır delimiter'** ı `Carriage Return + Line feed (\r\n)`. Açılan listede Veri Fabrikası tarafından desteklenen diğer satır sınırlayıcılarını görebilirsiniz. Ayrıca özel bir sınır layıcı belirtebilirsiniz.
       d. **Atlama satır sayısı** **0**olarak ayarlanır. Dosyanın üst kısmında birkaç satırın atlanınmasını istiyorsanız, numarayı buraya girin.
       e. **İlk veri satırı sütun adlarını içerir** ayarlanmaz. Kaynak dosyalar ilk satırda sütun adları içeriyorsa, bu seçeneği belirleyin.
       f. Null seçeneği **olarak boş sütun değeri tedavi** ayarlanır.
    2. Gelişmiş seçeneği görmek için **Gelişmiş ayarları** genişletin.
    3. Sayfanın alt kısmında, emp.txt dosyasındaki verilerin **önizlemesini** görün.
    4. Kaynak dosyadaki verilere bakarak kopya sihirbazın atıfta kullandığı şemayı görmek için alttaki **SCHEMA** sekmesini tıklatın.
    5. Sınırlayıcıları gözden geçirin verilerin önizlemesini gördükten sonra **İleri**’ye tıklayın.
    ![Kopyalama Aracı - Dosya biçimi ayarları](./media/data-factory-azure-blob-connector/copy-tool-file-format-settings.png)
8. Hedef **veri deposu sayfasında**Azure **Blob Depolama'yı**seçin ve **İleri'yi**tıklatın. Azure Blob Depolama'yı bu izbin de kaynak ve hedef veri depoları olarak kullanıyorsunuz.  
    ![Aracı Kopyala - hedef veri deposu seçin](media/data-factory-azure-blob-connector/select-destination-data-store.png)
9. Azure Blob depolama hesabı sayfasını **belirtin:**  
    1. **Bağlantı adı** alanı için **AzureStorageLinkedService** girin.
    2. **Hesap seçme yöntemi** için **Azure aboneliklerinden** seçeneğinin belirlendiğini onaylayın.
    3. Azure **aboneliğinizi**seçin.
    4. Azure depolama hesabınızı seçin.
    5. **İleri**'ye tıklayın.
10. Çıktı **dosyasını veya klasörü** seçin sayfasında:  
    1. **klasör yolunu** **adfblobconnector/output/{year}/{month}/{day}** olarak belirtin. **TAB'ı**girin.
    1. Yıl **için,** **yyyy**seçin.
    1. Ay **için,** **MM**olarak ayarlanır onaylayın.
    1. Gün **için,** **bu dd**ayarlanır onaylayın .
    1. **Sıkıştırma türünün** **Yok**olarak ayarlı olduğunu doğrulayın.
    1. **Kopyalama davranışının** **dosyaları birleştirin**olarak ayarladığını doğrulayın. Aynı ada sahip çıktı dosyası zaten varsa, sonunda aynı dosyaya yeni içerik eklenir.
    1. **İleri**'ye tıklayın.
       ![Kopyalama Aracı - Çıktı dosyasını veya klasörünü seçin](media/data-factory-azure-blob-connector/choose-the-output-file-or-folder.png)
11. Dosya **biçimi ayarları** sayfasında, ayarları gözden geçirin ve **İleri'yi**tıklatın. Buradaki ek seçeneklerden biri, çıktı dosyasına bir üstbilgi eklemektir. Bu seçeneği seçerseniz, kaynağın şemasından sütunların adlarını içeren bir üstbilgi satırı eklenir. Kaynağın şemasını görüntülerken varsayılan sütun adlarını yeniden adlandırabilirsiniz. Örneğin, ilk sütunu İlk Ad, ikinci sütunu Soyadı olarak değiştirebilirsiniz. Daha sonra, çıktı dosyası sütun adları olarak bu adlar ile bir üstbilgi ile oluşturulur.
    ![Aracı Kopyala - Hedef için dosya biçimi ayarları](media/data-factory-azure-blob-connector/file-format-destination.png)
12. Performans **ayarları** sayfasında, **bulut birimlerinin** ve **paralel kopyaların** **Otomatik**olarak ayarlandığını onaylayın ve İleri'yi tıklatın. Bu ayarlarla ilgili ayrıntılar için etkinlik [performansını ve ayar kılavuzunu kopyala'ya](data-factory-copy-activity-performance.md#parallel-copy)bakın.
    ![Kopyalama Aracı - Performans ayarları](media/data-factory-azure-blob-connector/copy-performance-settings.png)
14. **Özet** sayfasında, tüm ayarları (görev özellikleri, kaynak ve hedef ayarları ve kopyalama ayarları) gözden **geçirin**ve İleri'yi tıklatın.
    ![Kopyalama Aracı - Özet sayfası](media/data-factory-azure-blob-connector/copy-tool-summary-page.png)
15. **Özet** sayfasındaki bilgileri gözden geçirin ve **Son**’a tıklayın. Sihirbaz, veri fabrikasında (Kopyalama Sihirbazı’nı başlattığınız yer) iki bağlı hizmet, iki veri kümesi (girdi ve çıktı) ve bir işlem hattı oluşturur.
    ![Kopyalama Aracı - Dağıtım sayfası](media/data-factory-azure-blob-connector/copy-tool-deployment-page.png)

### <a name="monitor-the-pipeline-copy-task"></a>Ardışık hattı izleme (kopya görevi)

1. `Click here to monitor copy pipeline` **Dağıtım** sayfasındaki bağlantıyı tıklatın.
2. Ayrı bir sekmede **Monitör ve Yönet uygulamasını** görmeniz gerekir.  ![Uygulamayı İzle ve Yönet](media/data-factory-azure-blob-connector/monitor-manage-app.png)
3. En **start** üstteki `04/19/2017` başlangıç saatini **end** `04/27/2017`'ye ve bitiş saatine çevirin ve sonra **Uygula'yı**tıklatın.
4. **EtkİnLİk WINDOWS** listesinde beş etkinlik penceresi görmeniz gerekir. **WindowStart** saatleri, ardışık hatlar başından ardışık alan bitiş saatlerine kadar tüm günleri kapsamalıdır.
5. Tüm etkinlik pencerelerinin durumunu görene kadar **ACTIVITY WINDOWS** listesi için **Yenile** düğmesini birkaç kez tıklatın.
6. Şimdi, çıktı dosyalarının adfblobconnector kapsayıcısının çıktı klasöründe oluşturulduğunu doğrulayın. Çıktı klasöründe aşağıdaki klasör yapısını görmeniz gerekir:
    ```
    2017/04/21
    2017/04/22
    2017/04/23
    2017/04/24
    2017/04/25
    ```
   Veri fabrikalarını izleme ve yönetme hakkında ayrıntılı bilgi için Veri [Fabrikası boru hattı makalelerini izleyin ve yönetin.](data-factory-monitor-manage-app.md)

### <a name="data-factory-entities"></a>Veri Fabrikası varlıkları
Şimdi, Veri Fabrikası ana sayfasıyla sekmeye geri dön. Şu anda veri fabrikanızda iki bağlantılı hizmet, iki veri kümesi ve bir ardışık hat lar olduğuna dikkat edin.

![Varlıklarla Veri Fabrikası giriş sayfası](media/data-factory-azure-blob-connector/data-factory-home-page-with-numbers.png)

Veri Fabrikası Düzenleyicisi'ni başlatmak için **Yazar'ı tıklatın ve dağıtın.**

![Data Factory Düzenleyicisi](media/data-factory-azure-blob-connector/data-factory-editor.png)

Veri fabrikanızda aşağıdaki Veri Fabrikası varlıklarını görmeniz gerekir:

- İki bağlantılı servis. Biri kaynak için, diğeri hedef için. Her iki bağlı hizmet de bu izbin de aynı Azure Depolama hesabına başvurur.
- İki veri kümesi. Giriş veri kümesi ve çıktı veri kümesi. Bu izbarada, her ikisi de aynı blob kapsayıcısı kullanın, ancak farklı klasörlere (giriş ve çıktı) bakın.
- Bir boru hattı. Ardışık iş, Azure blob konumundan başka bir Azure blob konumuna veri kopyalamak için bir blob kaynağı ve blob lavabosu kullanan bir kopyalama etkinliği içerir.

Aşağıdaki bölümlerde bu varlıklar hakkında daha fazla bilgi sağlaymaktadır.

#### <a name="linked-services"></a>Bağlı hizmetler
İki bağlantılı hizmet görmelisiniz. Biri kaynak için, diğeri hedef için. Bu gözden geçirme de, adlar dışında her iki tanım da aynı görünür. Bağlı hizmetin **türü** **AzureStorage**olarak ayarlanır. Bağlantılı hizmet tanımının en önemli özelliği, Veri Fabrikası tarafından çalışma zamanında Azure Depolama hesabınıza bağlanmak için kullanılan **connectionString'tir.** Tanımdaki hubName özelliğini yoksay.

##### <a name="source-blob-storage-linked-service"></a>Kaynak blob depolama bağlantılı hizmet
```json
{
    "name": "Source-BlobStorage-z4y",
    "properties": {
        "type": "AzureStorage",
        "typeProperties": {
            "connectionString": "DefaultEndpointsProtocol=https;AccountName=mystorageaccount;AccountKey=**********"
        }
    }
}
```

##### <a name="destination-blob-storage-linked-service"></a>Hedef blob depolama bağlantılı hizmet

```json
{
    "name": "Destination-BlobStorage-z4y",
    "properties": {
        "type": "AzureStorage",
        "typeProperties": {
            "connectionString": "DefaultEndpointsProtocol=https;AccountName=mystorageaccount;AccountKey=**********"
        }
    }
}
```

Azure Depolama bağlantılı hizmet hakkında daha fazla bilgi için [Bkz. Bağlantılı hizmet özellikleri](#linked-service-properties) bölümüne bakın.

#### <a name="datasets"></a>Veri kümeleri
İki veri kümesi vardır: giriş veri kümesi ve çıktı veri kümesi. Veri kümesinin türü her ikisi için de **AzureBlob** olarak ayarlanır.

Giriş veri **kümesi, adfblobconnector** blob kapsayıcısının **giriş** klasörüne işaret emziyor. Veri, bu veri kümesini girdi olarak alan kopyalama etkinliğiyle birlikte ardışık hatlar tarafından üretilmeden **dış** özellik bu veri kümesi için **doğru** olarak ayarlanır.

Çıktı veri kümesi, aynı blob kapsayıcısının **çıkış** klasörüne işaret ediyor. Çıktı veri kümesi, çıktı dosyasının yolunu dinamik olarak değerlendirmek için **SliceStart** sistem değişkeninin yılını, ayını ve gününü de kullanır. Veri Fabrikası tarafından desteklenen işlevlerin ve sistem değişkenlerinin listesi için Bkz. [Veri Fabrikası işlevleri ve sistem değişkenleri.](data-factory-functions-variables.md) Bu veri kümesi ardışık hatlar tarafından üretildiği için **dış** özellik **false** (varsayılan değer) olarak ayarlanır.

Azure Blob veri kümesi tarafından desteklenen özellikler hakkında daha fazla bilgi için [Bkz. Veri Kümesi özellikleri](#dataset-properties) bölümüne bakın.

##### <a name="input-dataset"></a>Giriş veri kümesi

```json
{
    "name": "InputDataset-z4y",
    "properties": {
        "structure": [
            { "name": "Prop_0", "type": "String" },
            { "name": "Prop_1", "type": "String" }
        ],
        "type": "AzureBlob",
        "linkedServiceName": "Source-BlobStorage-z4y",
        "typeProperties": {
            "folderPath": "adfblobconnector/input/",
            "format": {
                "type": "TextFormat",
                "columnDelimiter": ","
            }
        },
        "availability": {
            "frequency": "Day",
            "interval": 1
        },
        "external": true,
        "policy": {}
    }
}
```

##### <a name="output-dataset"></a>Çıktı veri kümesi

```json
{
    "name": "OutputDataset-z4y",
    "properties": {
        "structure": [
            { "name": "Prop_0", "type": "String" },
            { "name": "Prop_1", "type": "String" }
        ],
        "type": "AzureBlob",
        "linkedServiceName": "Destination-BlobStorage-z4y",
        "typeProperties": {
            "folderPath": "adfblobconnector/output/{year}/{month}/{day}",
            "format": {
                "type": "TextFormat",
                "columnDelimiter": ","
            },
            "partitionedBy": [
                { "name": "year", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyy" } },
                { "name": "month", "value": { "type": "DateTime", "date": "SliceStart", "format": "MM" } },
                { "name": "day", "value": { "type": "DateTime", "date": "SliceStart", "format": "dd" } }
            ]
        },
        "availability": {
            "frequency": "Day",
            "interval": 1
        },
        "external": false,
        "policy": {}
    }
}
```

#### <a name="pipeline"></a>İşlem hattı
Boru hattının sadece bir aktivitesi var. Etkinlik **türü** **Kopyala**olarak ayarlanır. Etkinlik için tür özellikleri, iki bölüm, kaynak için bir ve lavabo için diğeri vardır. Etkinlik bir blob depolamadan veri kopyalama olarak kaynak türü **BlobSource** olarak ayarlanır. Lavabo türü, verileri bir blob depolamasına kopyalama etkinliği olarak **BlobSink** olarak ayarlanır. Kopyalama etkinliği giriş olarak InputDataset-z4y ve çıktı olarak OutputDataset-z4y alır.

BlobSource ve BlobSink tarafından desteklenen özellikler hakkında daha fazla bilgi için [bkz.](#copy-activity-properties)

```json
{
    "name": "CopyPipeline",
    "properties": {
        "activities": [
            {
                "type": "Copy",
                "typeProperties": {
                    "source": {
                        "type": "BlobSource",
                        "recursive": false
                    },
                    "sink": {
                        "type": "BlobSink",
                        "copyBehavior": "MergeFiles",
                        "writeBatchSize": 0,
                        "writeBatchTimeout": "00:00:00"
                    }
                },
                "inputs": [
                    {
                        "name": "InputDataset-z4y"
                    }
                ],
                "outputs": [
                    {
                        "name": "OutputDataset-z4y"
                    }
                ],
                "policy": {
                    "timeout": "1.00:00:00",
                    "concurrency": 1,
                    "executionPriorityOrder": "NewestFirst",
                    "style": "StartOfInterval",
                    "retry": 3,
                    "longRetry": 0,
                    "longRetryInterval": "00:00:00"
                },
                "scheduler": {
                    "frequency": "Day",
                    "interval": 1
                },
                "name": "Activity-0-Blob path_ adfblobconnector_input_->OutputDataset-z4y"
            }
        ],
        "start": "2017-04-21T22:34:00Z",
        "end": "2017-04-25T05:00:00Z",
        "isPaused": false,
        "pipelineMode": "Scheduled"
    }
}
```

## <a name="json-examples-for-copying-data-to-and-from-blob-storage"></a>Blob Depolama'ya ve Bu Depolama'dan veri kopyalamak için JSON örnekleri
Aşağıdaki örnekler, [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) veya [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md)kullanarak bir ardışık hat lar oluşturmak için kullanabileceğiniz örnek JSON tanımları sağlar. Azure Blob Depolama ve Azure SQL Veritabanı'na veri kopyalamanın nasıl yapılacağını gösterirler. Ancak veriler, Azure Veri Fabrikası'ndaki Kopyalama Etkinliği kullanılarak [doğrudan](data-factory-data-movement-activities.md#supported-data-stores-and-formats) herhangi bir **kaynaktan** burada belirtilen lavabolara kopyalanabilir.

### <a name="json-example-copy-data-from-blob-storage-to-sql-database"></a>JSON Örneği: Blob Depolama'dan SQL Veritabanına veri kopyalama
Aşağıdaki örnek gösterir:

1. [AzureSqlDatabase](data-factory-azure-sql-connector.md#linked-service-properties)türünde bağlantılı bir hizmet.
2. [AzureStorage](#linked-service-properties)türüne bağlı bir hizmet.
3. [AzureBlob](#dataset-properties)türünden bir giriş [veri kümesi.](data-factory-create-datasets.md)
4. [AzureSqlTable](data-factory-azure-sql-connector.md#dataset-properties)türünden bir çıktı [veri kümesi.](data-factory-create-datasets.md)
5. [BlobSource](#copy-activity-properties) ve [SqlSink](data-factory-azure-sql-connector.md#copy-activity-properties)kullanan kopyalama etkinliği olan bir [ardışık kaynak.](data-factory-create-pipelines.md)

Örnek, zaman serisi verilerini bir Azure blob'undan bir Azure SQL tablosuna saatlik olarak kopyalar. Bu örneklerde kullanılan JSON özellikleri, örnekleri izleyen bölümlerde açıklanmıştır.

**Azure SQL bağlantılı hizmet:**

```json
{
  "name": "AzureSqlLinkedService",
  "properties": {
    "type": "AzureSqlDatabase",
    "typeProperties": {
      "connectionString": "Server=tcp:<servername>.database.windows.net,1433;Database=<databasename>;User ID=<username>@<servername>;Password=<password>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
    }
  }
}
```
**Azure Depolama bağlantılı hizmet:**

```json
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
Azure Veri Fabrikası iki tür Azure Depolama bağlantılı hizmeti destekler: **AzureStorage** ve **AzureStorageSas.** İlki için, hesap anahtarını içeren bağlantı dizesini belirtirsiniz ve daha sonraki nde Paylaşılan Erişim İmzası (SAS) Uri'yi belirtirsiniz. Ayrıntılar için [Bağlantılı Hizmetler](#linked-service-properties) bölümüne bakın.

**Azure Blob giriş veri kümesi:**

Veriler her saat yeni bir damladan alınır (sıklık: saat, aralık: 1). Blob için klasör yolu ve dosya adı, işlenen dilimin başlangıç saatine göre dinamik olarak değerlendirilir. Klasör yolu, başlangıç zamanının yıl, ay ve gün kısmını kullanır ve dosya adı başlangıç zamanının saat kısmını kullanır. "dış": "true" ayarı, tablonun veri fabrikasının dışında olduğunu ve veri fabrikasındaki bir etkinlik tarafından üretilmediğini Veri Fabrikası'na bildirir.

```json
{
  "name": "AzureBlobInput",
  "properties": {
    "type": "AzureBlob",
    "linkedServiceName": "StorageLinkedService",
    "typeProperties": {
      "folderPath": "mycontainer/myfolder/yearno={Year}/monthno={Month}/dayno={Day}/",
      "fileName": "{Hour}.csv",
      "partitionedBy": [
        { "name": "Year", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyy" } },
        { "name": "Month", "value": { "type": "DateTime", "date": "SliceStart", "format": "MM" } },
        { "name": "Day", "value": { "type": "DateTime", "date": "SliceStart", "format": "dd" } },
        { "name": "Hour", "value": { "type": "DateTime", "date": "SliceStart", "format": "HH" } }
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
**Azure SQL çıktı veri kümesi:**

Örnek, verileri Azure SQL veritabanında "MyTable" adlı bir tabloya kopyalar. Azure SQL veritabanınızda, Blob CSV dosyasının içermesini beklediğiniz sütun sayısıyla tablooluşturun. Her saat başı tabloya yeni satırlar eklenir.

```json
{
  "name": "AzureSqlOutput",
  "properties": {
    "type": "AzureSqlTable",
    "linkedServiceName": "AzureSqlLinkedService",
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
**Blob kaynağı ve SQL lavabosu olan bir ardışık ardışık ardışık alanda kopyalama etkinliği:**

Ardışık iş, giriş ve çıktı veri kümelerini kullanacak şekilde yapılandırılan ve her saat çalışacak şekilde zamanlanan bir Kopyalama Etkinliği içerir. JSON ardışık hatlar **tanımında, kaynak** türü **BlobSource** olarak ayarlanır ve **lavabo** türü **SqlSink**olarak ayarlanır.

```json
{
  "name":"SamplePipeline",
  "properties":{
    "start":"2014-06-01T18:00:00",
    "end":"2014-06-01T19:00:00",
    "description":"pipeline with copy activity",
    "activities":[
      {
        "name": "AzureBlobtoSQL",
        "description": "Copy Activity",
        "type": "Copy",
        "inputs": [
          {
            "name": "AzureBlobInput"
          }
        ],
        "outputs": [
          {
            "name": "AzureSqlOutput"
          }
        ],
        "typeProperties": {
          "source": {
            "type": "BlobSource"
          },
          "sink": {
            "type": "SqlSink"
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
### <a name="json-example-copy-data-from-azure-sql-to-azure-blob"></a>JSON Örneği: Azure SQL'den Azure Blob'a veri kopyalama
Aşağıdaki örnek gösterir:

1. [AzureSqlDatabase](data-factory-azure-sql-connector.md#linked-service-properties)türünde bağlantılı bir hizmet.
2. [AzureStorage](#linked-service-properties)türüne bağlı bir hizmet.
3. [AzureSqlTable](data-factory-azure-sql-connector.md#dataset-properties)türünden bir giriş [veri kümesi.](data-factory-create-datasets.md)
4. [AzureBlob](#dataset-properties)türünden bir çıktı [veri kümesi.](data-factory-create-datasets.md)
5. [SqlSource](data-factory-azure-sql-connector.md#copy-activity-properties) ve [BlobSink](#copy-activity-properties)kullanan Kopyalama etkinliği olan bir [ardışık kaynak.](data-factory-create-pipelines.md)

Örnek, zaman serisi verilerini Bir Azure SQL tablosundan bir Azure tablosuna saatlik olarak kopyalar. Bu örneklerde kullanılan JSON özellikleri, örnekleri izleyen bölümlerde açıklanmıştır.

**Azure SQL bağlantılı hizmet:**

```json
{
  "name": "AzureSqlLinkedService",
  "properties": {
    "type": "AzureSqlDatabase",
    "typeProperties": {
      "connectionString": "Server=tcp:<servername>.database.windows.net,1433;Database=<databasename>;User ID=<username>@<servername>;Password=<password>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
    }
  }
}
```
**Azure Depolama bağlantılı hizmet:**

```json
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
Azure Veri Fabrikası iki tür Azure Depolama bağlantılı hizmeti destekler: **AzureStorage** ve **AzureStorageSas.** İlki için, hesap anahtarını içeren bağlantı dizesini belirtirsiniz ve daha sonraki nde Paylaşılan Erişim İmzası (SAS) Uri'yi belirtirsiniz. Ayrıntılar için [Bağlantılı Hizmetler](#linked-service-properties) bölümüne bakın.

**Azure SQL giriş veri kümesi:**

Örnek, Azure SQL'de "MyTable" tablosu oluşturduğunuzu varsayar ve zaman serisi verileri için "zaman damgası sütunu" adlı bir sütun içerir.

"Dış"ı ayarlamak: "true" veri fabrikası hizmetine tablonun veri fabrikasının dışında olduğunu ve veri fabrikasındaki bir etkinlik tarafından üretilmediğini bildirir.

```json
{
  "name": "AzureSqlInput",
  "properties": {
    "type": "AzureSqlTable",
    "linkedServiceName": "AzureSqlLinkedService",
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

```json
{
  "name": "AzureBlobOutput",
  "properties": {
    "type": "AzureBlob",
    "linkedServiceName": "StorageLinkedService",
    "typeProperties": {
      "folderPath": "mycontainer/myfolder/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}/",
      "partitionedBy": [
        {
          "name": "Year",
          "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyy" } },
        { "name": "Month", "value": { "type": "DateTime", "date": "SliceStart", "format": "MM" } },
        { "name": "Day", "value": { "type": "DateTime", "date": "SliceStart", "format": "dd" } },
        { "name": "Hour", "value": { "type": "DateTime", "date": "SliceStart", "format": "HH" } }
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

**SQL kaynağı ve Blob lavabosu olan bir ardışık ardışık ardışık alanda kopyalama etkinliği:**

Ardışık iş, giriş ve çıktı veri kümelerini kullanacak şekilde yapılandırılan ve her saat çalışacak şekilde zamanlanan bir Kopyalama Etkinliği içerir. JSON ardışık hatlar **tanımında, kaynak** türü **SqlSource** olarak ayarlanır ve **lavabo** türü **BlobSink**olarak ayarlanır. **SqlReaderQuery** özelliği için belirtilen SQL sorgusu, kopyalanacak son bir saat içinde verileri seçer.

```json
{
    "name":"SamplePipeline",
    "properties":{
        "start":"2014-06-01T18:00:00",
        "end":"2014-06-01T19:00:00",
        "description":"pipeline for copy activity",
        "activities":[
            {
                "name": "AzureSQLtoBlob",
                "description": "copy activity",
                "type": "Copy",
                "inputs": [
                    {
                        "name": "AzureSQLInput"
                    }
                ],
                "outputs": [
                    {
                        "name": "AzureBlobOutput"
                    }
                ],
                "typeProperties": {
                    "source": {
                        "type": "SqlSource",
                        "SqlReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd HH:mm}\\' AND timestampcolumn < \\'{1:yyyy-MM-dd HH:mm}\\'', WindowStart, WindowEnd)"
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
> Kaynak veri kümesinden sütunlara kadar sütunları haritalamak için Azure [Veri Fabrikası'nda veri kümesi sütunlarını eşleme](data-factory-map-columns.md)konusuna bakın.

## <a name="performance-and-tuning"></a>Performans ve Tuning
Azure Veri Fabrikası'ndaki veri hareketinin performansını etkileyen önemli faktörler (Kopyalama Etkinliği) ve bunu optimize etmenin çeşitli yolları hakkında bilgi edinmek için [Etkinlik performansını & Tuning Kılavuzu'na](data-factory-copy-activity-performance.md) bakın.
