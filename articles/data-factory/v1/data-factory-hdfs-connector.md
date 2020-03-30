---
title: Verileri şirket içi HDFS'den taşıma
description: Azure Veri Fabrikası'nı kullanarak şirket içi HDFS'den verileri nasıl taşıyabildiğini öğrenin.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: shwang
ms.assetid: 3215b82d-291a-46db-8478-eac1a3219614
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 01/10/2018
ms.author: jingwang
robots: noindex
ms.openlocfilehash: 7652ab72fb972230d98913c2d7e2601737982532
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74924341"
---
# <a name="move-data-from-on-premises-hdfs-using-azure-data-factory"></a>Azure Veri Fabrikası'nı kullanarak verileri şirket içi HDFS'den taşıma
> [!div class="op_single_selector" title1="Kullandığınız Veri Fabrikası hizmetisürümünü seçin:"]
> * [Sürüm 1](data-factory-hdfs-connector.md)
> * [Sürüm 2 (geçerli sürüm)](../connector-hdfs.md)

> [!NOTE]
> Bu makale, Data Factory’nin 1. sürümü için geçerlidir. Veri Fabrikası hizmetinin geçerli sürümünü kullanıyorsanız, [V2'deki HDFS konektörüne](../connector-hdfs.md)bakın.

Bu makalede, verileri şirket içi bir HDFS'den taşımak için Azure Veri Fabrikası'ndaki Kopyalama Etkinliği'nin nasıl kullanılacağı açıklanmaktadır. Kopya etkinliğiyle birlikte veri hareketine genel bir genel bakış sunan [Veri Hareketi Etkinlikleri](data-factory-data-movement-activities.md) makalesine dayanmaktadır.

HDFS'deki verileri desteklenen herhangi bir lavabo veri deposuna kopyalayabilirsiniz. Kopyalama etkinliği tarafından lavabo olarak desteklenen veri depolarının listesi için [Desteklenen veri depoları](data-factory-data-movement-activities.md#supported-data-stores-and-formats) tablosuna bakın. Veri fabrikası şu anda yalnızca şirket içi HDFS'den diğer veri depolarına veri taşımayı destekler, ancak verileri diğer veri depolarından şirket içi HDFS'ye taşımak için değildir.

> [!NOTE]
> Copy Activity, hedefe başarıyla kopyalandıktan sonra kaynak dosyayı silmez. Başarılı bir kopyadan sonra kaynak dosyayı silmeniz gerekiyorsa, dosyayı silmek ve ardışık düzendeki etkinliği kullanmak için özel bir etkinlik oluşturun. 

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

## <a name="enabling-connectivity"></a>Bağlantı sağlama
Veri Fabrikası hizmeti, Veri Yönetimi Ağ Geçidi'ni kullanarak şirket içi HDFS'ye bağlanmayı destekler. Veri Yönetimi Ağ Geçidi ve ağ geçidini ayarlama yla ilgili adım adım talimatlar hakkında bilgi edinmek için [şirket içi konumlar ve bulut](data-factory-move-data-between-onprem-and-cloud.md) makalesi arasında taşınan verileri görün. Bir Azure IaaS VM'de barındırılan olsa bile HDFS'ye bağlanmak için ağ geçidini kullanın.

> [!NOTE]
> Veri Yönetimi Ağ Geçidi'nin Hadoop kümesinin **ALL** [ad düğümü sunucusu]:[ad düğümü bağlantı noktası] ve [veri düğümü sunucuları]:[veri düğümü bağlantı noktası]'na erişebileceğinden emin olun. Varsayılan [ad düğümü bağlantı noktası] 50070 ve varsayılan [veri düğümü bağlantı noktası] 50075'tir.

Ağ geçidini şirket içi makineye veya Azure VM'ye HDFS olarak yükleyebilirsiniz, ancak ağ geçidini ayrı bir makineye/Azure IaaS VM'ye yüklemenizi öneririz. Ayrı bir makinede ağ geçidi olması kaynak çekişmelerini azaltır ve performansı artırır. Ağ geçidini ayrı bir makineye yüklediğinizde, makine nin MAKINEye HDFS ile erişebilmesi gerekir.

## <a name="getting-started"></a>Başlarken
Farklı araçlar/API'ler kullanarak verileri bir HDFS kaynağından hareket ettiren bir kopyalama etkinliği içeren bir ardışık hatlar oluşturabilirsiniz.

Bir ardışık yol oluşturmanın en kolay yolu **Kopyalama Sihirbazı'nı**kullanmaktır. Bkz. Öğretici: Veri kopyala sihirbazını kullanarak bir ardışık yol oluşturma konusunda hızlı bir geçiş için Kopya Sihirbazı kullanarak bir [ardışık kaynak oluşturun.](data-factory-copy-data-wizard-tutorial.md)

Bir ardışık kaynak oluşturmak için aşağıdaki araçları da kullanabilirsiniz: **Azure portalı,** **Visual Studio,** **Azure PowerShell,** **Azure Kaynak Yöneticisi şablonu**, **.NET API**ve **REST API.** Kopyalama etkinliği içeren bir ardışık hatlar oluşturmak için adım adım yönergeleri için [etkinlik öğreticisini](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) kopyala'ya bakın.

Araçları veya API'leri kullanın, verileri kaynak veri deposundan bir lavabo veri deposuna aktaran bir ardışık işlem hattı oluşturmak için aşağıdaki adımları gerçekleştirirsiniz:

1. Giriş ve çıktı veri depolarını veri fabrikanıza bağlamak için **bağlantılı hizmetler** oluşturun.
2. Kopyalama işlemi için giriş ve çıktı verilerini temsil edecek **veri kümeleri** oluşturun.
3. Giriş olarak veri kümesi ve çıktı olarak veri kümesi alan bir kopyalama etkinliği içeren bir **ardışık işlem oluşturma.**

Sihirbazı kullandığınızda, bu Veri Fabrikası varlıkları (bağlantılı hizmetler, veri kümeleri ve ardışık kuruluş) için JSON tanımları sizin için otomatik olarak oluşturulur. Araçları/API'leri (.NET API hariç) kullandığınızda, Bu Veri Fabrikası varlıklarını JSON biçimini kullanarak tanımlarsınız.  BIR HDFS veri deposundan veri kopyalamak için kullanılan Veri Fabrikası varlıkları için JSON tanımlı bir örnek [için Bkz. JSON örneği: Verileri şirket içi HDFS'den Azure Blob bölümüne kopyalayın.](#json-example-copy-data-from-on-premises-hdfs-to-azure-blob)

Aşağıdaki bölümler, HDFS'ye özgü Veri Fabrikası varlıklarını tanımlamak için kullanılan JSON özellikleri hakkında ayrıntılı bilgi sağlar:

## <a name="linked-service-properties"></a>Bağlantılı hizmet özellikleri
Bağlantılı bir hizmet, bir veri deposuna veri fabrikasına bağlanır. Şirket içi bir HDFS'yi veri fabrikanıza bağlamak için **Hdfs** türünden bağlantılı bir hizmet oluşturursunuz. Aşağıdaki tablo, HDFS bağlantılı hizmete özgü JSON öğeleri için açıklama sağlar.

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| type |Tür özelliği ayarlanmalıdır: **Hdfs** |Evet |
| url |HDFS'nin URL'si |Evet |
| authenticationType |Anonim veya Windows. <br><br> HDFS bağlayıcısı için **Kerberos kimlik doğrulamasını** kullanmak için, şirket içi ortamınızı buna göre ayarlamak için [bu bölüme](#use-kerberos-authentication-for-hdfs-connector) bakın. |Evet |
| userName |Windows kimlik doğrulaması için kullanıcı adı. Kerberos kimlik doğrulaması `<username>@<domain>.com`için . |Evet (Windows Kimlik Doğrulama için) |
| password |Windows kimlik doğrulama için parola. |Evet (Windows Kimlik Doğrulama için) |
| ağ geçidiAdı |Veri Fabrikası hizmetinin HDFS'ye bağlanmak için kullanması gereken ağ geçidinin adı. |Evet |
| şifreli Credential |[Yeni-AzDataFactoryEncryptValue](https://docs.microsoft.com/powershell/module/az.datafactory/new-azdatafactoryencryptvalue) erişim kimlik bilgisi çıktısı. |Hayır |

### <a name="using-anonymous-authentication"></a>Anonim kimlik doğrulamasını kullanma

```JSON
{
    "name": "hdfs",
    "properties":
    {
        "type": "Hdfs",
        "typeProperties":
        {
            "authenticationType": "Anonymous",
            "userName": "hadoop",
            "url" : "http://<machine>:50070/webhdfs/v1/",
            "gatewayName": "mygateway"
        }
    }
}
```

### <a name="using-windows-authentication"></a>Windows kimlik doğrulamayı kullanma

```JSON
{
    "name": "hdfs",
    "properties":
    {
        "type": "Hdfs",
        "typeProperties":
        {
            "authenticationType": "Windows",
            "userName": "<username>@<domain>.com (for Kerberos auth)",
            "password": "password",
            "url" : "http://<machine>:50070/webhdfs/v1/",
            "gatewayName": "mygateway"
        }
    }
}
```
## <a name="dataset-properties"></a>Veri kümesi özellikleri
Veri kümelerini tanımlamak için kullanılabilen bölümlerin & özelliklerin tam listesi için [veri kümelerini oluşturma](data-factory-create-datasets.md) makalesine bakın. Bir veri kümesi JSON'un yapısı, kullanılabilirliği ve ilkesi gibi bölümler tüm veri kümesi türleri (Azure SQL, Azure blob, Azure tablosu, vb.) için benzerdir.

**typeProperties** bölümü her veri kümesi türü için farklıdır ve veri deposundaki verilerin konumu hakkında bilgi sağlar. **FileShare** türü veri kümesi için typeProperties bölümü (HDFS veri kümesi ni içerir) aşağıdaki özelliklere sahiptir

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| folderPath |Klasöre giden yol. Örnek: `myfolder`<br/><br/>Dizedeki özel karakterler için ' \ ' kaçış karakterini kullanın. Örneğin: folder\subfolder için klasör\\\\alt klasörünü belirtin ve d:\samplefolder için d:\\\\samplefolder belirtin.<br/><br/>Bu özelliği **partitionBy** ile birleştirerek dilim başlangıç/bitiş tarih saatlerine göre klasör yollarına sahip olabilirsiniz. |Evet |
| fileName |Tablonun klasördeki belirli bir dosyaya başvurmasını istiyorsanız **klasörDeki Dosyanın** adını belirtinPath. Bu özellik için herhangi bir değer belirtmezseniz, tablo klasördeki tüm dosyaları işaret edir.<br/><br/>Bir çıktı veri kümesi için dosya Adı belirtilmediğinde, oluşturulan dosyanın adı aşağıdaki biçimde olacaktır: <br/><br/>`Data.<Guid>.txt`(örneğin: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt |Hayır |
| bölümlemeBy |partitionedBy dinamik bir folderPath, zaman serisi verileri için dosya adı belirtmek için kullanılabilir. Örnek: klasörHer saat veri için parametreli yol. |Hayır |
| biçim | Aşağıdaki biçim türleri desteklenir: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParkeFormat**. Biçim altındaki **tür** özelliğini bu değerlerden birine ayarlayın. Daha fazla bilgi için [Metin Biçimi,](data-factory-supported-file-and-compression-formats.md#text-format) [Json Formatı,](data-factory-supported-file-and-compression-formats.md#json-format) [Avro Biçimi,](data-factory-supported-file-and-compression-formats.md#avro-format) [Ork Biçimi](data-factory-supported-file-and-compression-formats.md#orc-format)ve [Parke Biçimi](data-factory-supported-file-and-compression-formats.md#parquet-format) bölümlerine bakın. <br><br> Dosyaları dosya tabanlı mağazalar (ikili kopya) arasında **olduğu gibi kopyalamak** istiyorsanız, hem giriş hem de çıktı veri kümesi tanımlarında biçim bölümünü atlayın. |Hayır |
| sıkıştırma | Verilerin sıkıştırma türünü ve düzeyini belirtin. Desteklenen türleri şunlardır: **GZip**, **Deflate**, **BZip2**, ve **ZipDeflate**. Desteklenen seviyeler şunlardır: **Optimal** ve **En Hızlı.** Daha fazla bilgi için [Azure Veri Fabrikası'ndaki Dosya ve sıkıştırma biçimlerine](data-factory-supported-file-and-compression-formats.md#compression-support)bakın. |Hayır |

> [!NOTE]
> dosya adı ve fileFilter aynı anda kullanılamaz.

### <a name="using-partionedby-property"></a>PartionedBy özelliğini kullanma
Önceki bölümde belirtildiği gibi, **bölümlenmişBy** özelliği, [Veri Fabrikası işlevleri ve sistem değişkenleri](data-factory-functions-variables.md)ile zaman serisi verileri için dinamik bir klasörPath ve dosya adı belirtebilirsiniz.

Zaman serisi veri kümeleri, zamanlama ve dilimler hakkında daha fazla bilgi edinmek [Creating Pipelines](data-factory-create-pipelines.md) için [&](data-factory-scheduling-and-execution.md) [bkz.](data-factory-create-datasets.md)

#### <a name="sample-1"></a>Örnek 1:

```JSON
"folderPath": "wikidatagateway/wikisampledataout/{Slice}",
"partitionedBy":
[
    { "name": "Slice", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyyMMddHH" } },
],
```
Bu örnekte {Slice} veri fabrikası sistem değişkeni SliceStart değeri ile değiştirilir formatta (YYYYMMDDHH) belirtilir. SliceStart, dilimin başlangıç saatini ifade eder. FolderPath her dilim için farklıdır. Örneğin: wikidatagateway/wikisampledataout/2014100103 veya wikidatagateway/wikisampledataout/2014100104.

#### <a name="sample-2"></a>Örnek 2:

```JSON
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
Etkinlikleri tanımlamak için kullanılabilen bölümlerin & özelliklerinin tam listesi [için, Kaynak Hatları Oluşturma](data-factory-create-pipelines.md) makalesine bakın. Ad, açıklama, giriş ve çıktı tabloları ve ilkeler gibi özellikler tüm etkinlik türleri için kullanılabilir.

Oysa, etkinliğin typeProperties bölümünde bulunan özellikler her etkinlik türüne göre değişir. Kopyalama etkinliği için, kaynak ve lavabo türlerine bağlı olarak değişir.

Copy Activity için, kaynak **FileSystemSource** türünde olduğunda typeProperties bölümünde aşağıdaki özellikler mevcuttur:

**FileSystemSource** aşağıdaki özellikleri destekler:

| Özellik | Açıklama | İzin verilen değerler | Gerekli |
| --- | --- | --- | --- |
| Özyinelemeli |Verilerin alt klasörlerden mi yoksa yalnızca belirtilen klasörden mi özyinelemeli olarak okunduğunu gösterir. |True, False (varsayılan) |Hayır |

## <a name="supported-file-and-compression-formats"></a>Desteklenen dosya ve sıkıştırma biçimleri
Ayrıntılarla ilgili [Azure Veri Fabrikası makalesinde Dosya ve sıkıştırma biçimlerine](data-factory-supported-file-and-compression-formats.md) bakın.

## <a name="json-example-copy-data-from-on-premises-hdfs-to-azure-blob"></a>JSON örneği: Şirket içi HDFS'den Azure Blob'a veri kopyalama
Bu örnek, şirket içi bir HDFS'den Azure Blob Depolama'ya verilerin nasıl kopyalanır olduğunu gösterir. Ancak veriler, Azure Veri Fabrikası'ndaki Kopyalama Etkinliği kullanılarak [burada](data-factory-data-movement-activities.md#supported-data-stores-and-formats) belirtilen lavabolardan herhangi biri için **doğrudan** kopyalanabilir.  

Örnek, aşağıdaki Veri Fabrikası varlıkları için JSON tanımları sağlar. [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) veya [Azure PowerShell'i](data-factory-copy-activity-tutorial-using-powershell.md)kullanarak HDFS'den Azure Blob Depolamasına kadar verileri kopyalamak için bir ardışık kaynak oluşturmak için bu tanımları kullanabilirsiniz.

1. [OnPremisesHdfs](#linked-service-properties)türü ne bağlı bir hizmet.
2. [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties)türüne bağlı bir hizmet.
3. [FileShare](#dataset-properties)türünden bir giriş [veri kümesi.](data-factory-create-datasets.md)
4. [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties)türünden bir çıktı [veri kümesi.](data-factory-create-datasets.md)
5. [FileSystemSource](#copy-activity-properties) ve [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties)kullanan Copy Activity ile bir [ardışık düzen.](data-factory-create-pipelines.md)

Örnek, şirket içi bir HDFS'den her saat başı bir Azure blob'una kadar verileri kopyalar. Bu örneklerde kullanılan JSON özellikleri, örnekleri izleyen bölümlerde açıklanmıştır.

İlk adım olarak, veri yönetimi ağ geçidini ayarlayın. Şirket içi konumlar ve bulut makalesi [arasında taşınan verilerdeki](data-factory-move-data-between-onprem-and-cloud.md) yönergeler.

**HDFS bağlantılı hizmet:** Bu örnekte Windows kimlik doğrulaması kullanır. Kullanabileceğiniz farklı kimlik doğrulama türleri için [HDFS bağlantılı hizmet](#linked-service-properties) bölümüne bakın.

```JSON
{
    "name": "HDFSLinkedService",
    "properties":
    {
        "type": "Hdfs",
        "typeProperties":
        {
            "authenticationType": "Windows",
            "userName": "Administrator",
            "password": "password",
            "url" : "http://<machine>:50070/webhdfs/v1/",
            "gatewayName": "mygateway"
        }
    }
}
```

**Azure Depolama bağlantılı hizmet:**

```JSON
{
  "name": "AzureStorageLinkedService",
  "properties": {
    "type": "AzureStorage",
    "typeProperties": {
      "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
    }
  }
}
```

**HDFS giriş veri seti:** Bu veri kümesi, HDFS klasörü DataTransfer/UnitTest/ anlamına gelir. Ardışık işlem, bu klasördeki tüm dosyaları hedefe kopyalar.

"Dış"ı ayarlamak: "true" veri kümesinin veri fabrikasının dışında olduğunu ve veri fabrikasındaki bir etkinlik tarafından üretilmediğini Veri Fabrikası hizmetine bildirir.

```JSON
{
    "name": "InputDataset",
    "properties": {
        "type": "FileShare",
        "linkedServiceName": "HDFSLinkedService",
        "typeProperties": {
            "folderPath": "DataTransfer/UnitTest/"
        },
        "external": true,
        "availability": {
            "frequency": "Hour",
            "interval":  1
        }
    }
}
```

**Azure Blob çıktı veri seti:**

Veriler her saat yeni bir blob 'a yazılır (sıklık: saat, aralık: 1). Blob için klasör yolu, işlenen dilimin başlangıç saatine göre dinamik olarak değerlendirilir. Klasör yolu, başlangıç zamanının yıl, ay, gün ve saat bölümlerini kullanır.

```JSON
{
    "name": "OutputDataset",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
        "typeProperties": {
            "folderPath": "mycontainer/hdfs/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
            "format": {
                "type": "TextFormat",
                "rowDelimiter": "\n",
                "columnDelimiter": "\t"
            },
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
            ]
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        }
    }
}
```

**Dosya Sistemi kaynağı ve Blob lavabosu olan bir ardışık düzende kopyalama etkinliği:**

Ardışık iş, bu giriş ve çıktı veri kümelerini kullanacak şekilde yapılandırılan ve her saat çalışacak şekilde zamanlanan bir Kopyalama Etkinliği içerir. Boru hattı JSON tanımında, **kaynak** türü **FileSystemSource** olarak ayarlanır ve **lavabo** türü **BlobSink**olarak ayarlanır. **Sorgu** özelliği için belirtilen SQL sorgusu, kopyalanacak son saatteki verileri seçer.

```JSON
{
    "name": "pipeline",
    "properties":
    {
        "activities":
        [
            {
                "name": "HdfsToBlobCopy",
                "inputs": [ {"name": "InputDataset"} ],
                "outputs": [ {"name": "OutputDataset"} ],
                "type": "Copy",
                "typeProperties":
                {
                    "source":
                    {
                        "type": "FileSystemSource"
                    },
                    "sink":
                    {
                        "type": "BlobSink"
                    }
                },
                "policy":
                {
                    "concurrency": 1,
                    "executionPriorityOrder": "NewestFirst",
                    "retry": 1,
                    "timeout": "00:05:00"
                }
            }
        ],
        "start": "2014-06-01T18:00:00Z",
        "end": "2014-06-01T19:00:00Z"
    }
}
```

## <a name="use-kerberos-authentication-for-hdfs-connector"></a>HDFS konektörü için Kerberos kimlik doğrulamasını kullanma
HDFS bağlayıcısında Kerberos Kimlik Doğrulaması'nı kullanmak için şirket içi ortamı ayarlamak için iki seçenek vardır. Durumunuza daha uygun olanı seçebilirsiniz.
* Seçenek 1: [Kerberos diyarında ağ geçidi makinesine katılın](#kerberos-join-realm)
* Seçenek 2: [Windows etki alanı ile Kerberos diyarı arasında karşılıklı güveni etkinleştirme](#kerberos-mutual-trust)

### <a name="option-1-join-gateway-machine-in-kerberos-realm"></a><a name="kerberos-join-realm"></a>Seçenek 1: Kerberos diyarında ağ geçidi makinesine katılın

#### <a name="requirement"></a>Gereksinim:

* Ağ geçidi makinesinin Kerberos diyarına katılması gerekir ve herhangi bir Windows etki alanına katılamaz.

#### <a name="how-to-configure"></a>Nasıl yapılandırın:

**Ağ geçidi makinesinde:**

1.  Kerberos KDC sunucu ve bölge yapılandırmak için **Ksetup** yardımcı programı çalıştırın.

    Kerberos alemi Windows etki alanından farklı olduğundan, makine bir çalışma grubunun üyesi olarak yapılandırılmalıdır. Bu, Kerberos aleminin ayarlanması ve aşağıdaki gibi bir KDC sunucusu eklenmesiyle elde edilebilir. *gerektiğinde kendi* bölgenizle REALM.COM değiştirin.

            C:> Ksetup /setdomain REALM.COM
            C:> Ksetup /addkdc REALM.COM <your_kdc_server_address>

    Bu 2 komutu çalıştırdıktan sonra makineyi **yeniden başlatın.**

2.  Yapılandırmayı **Ksetup** komutuyla doğrulayın. Çıktı gibi olmalıdır:

            C:> Ksetup
            default realm = REALM.COM (external)
            REALM.com:
                kdc = <your_kdc_server_address>

**Azure Veri Fabrikası'nda:**

* HDFS veri kaynağına bağlanmak için **Windows kimlik doğrulamasını** kullanarak HDFS bağlayıcısını Kerberos ana adınız ve parolanızla birlikte yapılandırın. Yapılandırma ayrıntılarıyla ilgili [HDFS Bağlantılı Hizmet özellikleri](#linked-service-properties) bölümünü kontrol edin.

### <a name="option-2-enable-mutual-trust-between-windows-domain-and-kerberos-realm"></a><a name="kerberos-mutual-trust"></a>Seçenek 2: Windows etki alanı ile Kerberos diyarı arasında karşılıklı güveni etkinleştirme

#### <a name="requirement"></a>Gereksinim:
*   Ağ geçidi makinesinin bir Windows etki alanına katılması gerekir.
*   Etki alanı denetleyicisinin ayarlarını güncelleştirmek için izne ihtiyacınız vardır.

#### <a name="how-to-configure"></a>Nasıl yapılandırın:

> [!NOTE]
> Aşağıdaki öğreticide REALM.COM ve AD.COM gerektiğinde kendi bölgeniz ve etki alanı denetleyiciniz ile değiştirin.

**KDC sunucusunda:**

1. KDC'nin aşağıdaki yapılandırma şablonuna atıfta bulunarak Windows Etki Alanı'na güveneizin vermek için **Krb5.conf** dosyasındaki KDC yapılandırmasını düzenleme. Varsayılan olarak, yapılandırma **/etc/krb5.conf**adresinde bulunur.

           [logging]
            default = FILE:/var/log/krb5libs.log
            kdc = FILE:/var/log/krb5kdc.log
            admin_server = FILE:/var/log/kadmind.log

           [libdefaults]
            default_realm = REALM.COM
            dns_lookup_realm = false
            dns_lookup_kdc = false
            ticket_lifetime = 24h
            renew_lifetime = 7d
            forwardable = true

           [realms]
            REALM.COM = {
             kdc = node.REALM.COM
             admin_server = node.REALM.COM
            }
           AD.COM = {
            kdc = windc.ad.com
            admin_server = windc.ad.com
           }

           [domain_realm]
            .REALM.COM = REALM.COM
            REALM.COM = REALM.COM
            .ad.com = AD.COM
            ad.com = AD.COM

           [capaths]
            AD.COM = {
             REALM.COM = .
            }

   Yapılandırmadan sonra KDC hizmetini **yeniden başlatın.**

2. KDC sunucusunda **krbtgt/REALM.COM\@** AD.COM adlı bir müdür hazırlayın:

           Kadmin> addprinc krbtgt/REALM.COM@AD.COM

3. **hadoop.security.auth_to_local** HDFS hizmet yapılandırma `RULE:[1:$1@$0](.*\@AD.COM)s/\@.*//`dosyasında, ekleyin.

**Etki alanı denetleyicisi:**

1.  Bir bölge girişi eklemek için aşağıdaki **Ksetup** komutlarını çalıştırın:

            C:> Ksetup /addkdc REALM.COM <your_kdc_server_address>
            C:> ksetup /addhosttorealmmap HDFS-service-FQDN REALM.COM

2.  Windows Etki Alanı'ndan Kerberos Diyarı'na güven kurun. [şifre] AD.COM ana **krbtgt/REALM.COM\@** için şifredir.

            C:> netdom trust REALM.COM /Domain: AD.COM /add /realm /passwordt:[password]

3.  Kerberos'ta kullanılan şifreleme algoritması'nı seçin.

    1. Varsayılan veya Etkin Etki Alanı İlkesi > Grup İlkesi Nesneleri > Etki Alanı > Grup İlkesi > Yönetimi'ne gidin ve Düzenleme.

    2. Grup **İlkesi Yönetimi Düzenleyicisi** açılır penceresinde, Windows Ayarları > Güvenlik Ayarları > Yerel İlkeler > Güvenlik Seçenekleri > Bilgisayar Yapılandırma > İlkeleri'ne gidin ve **Ağ güvenliğini yapılandırma: Kerberos için izin verilen Şifreleme türlerini yapılandırın.**

    3. KDC'ye bağlanırken kullanmak istediğiniz şifreleme algoritmasını seçin. Genellikle, sadece tüm seçenekleri seçebilirsiniz.

        ![Kerberos için Config Şifreleme Türleri](media/data-factory-hdfs-connector/config-encryption-types-for-kerberos.png)

    4. Belirli REALM'de kullanılacak şifreleme algoritmasını belirtmek için **Ksetup** komutunu kullanın.

                C:> ksetup /SetEncTypeAttr REALM.COM DES-CBC-CRC DES-CBC-MD5 RC4-HMAC-MD5 AES128-CTS-HMAC-SHA1-96 AES256-CTS-HMAC-SHA1-96

4.  Windows Etki Alanı'nda Kerberos müdürünü kullanmak için etki alanı hesabı ile Kerberos müdürü arasındaki eşlemi oluşturun.

    1. **Etkin Dizin Kullanıcıları ve Bilgisayarları**> Yönetim araçlarını başlatın.

    2. **Gelişmiş Özellikleri** **Görüntüle'yi** > tıklatarak gelişmiş özellikleri yapılandırın.

    3. Eşlemeler oluşturmak istediğiniz hesabı bulun ve **Ad Eşlemeleri'ni** görüntülemek için sağ tıklatın > **Kerberos Adları** sekmesini tıklatın.

    4. Diyardan bir müdür ekle.

        ![Harita Güvenlik Kimliği](media/data-factory-hdfs-connector/map-security-identity.png)

**Ağ geçidi makinesinde:**

* Bir bölge girişi eklemek için aşağıdaki **Ksetup** komutlarını çalıştırın.

            C:> Ksetup /addkdc REALM.COM <your_kdc_server_address>
            C:> ksetup /addhosttorealmmap HDFS-service-FQDN REALM.COM

**Azure Veri Fabrikası'nda:**

* HDFS veri kaynağına bağlanmak için **Windows kimlik doğrulamasını** kullanarak HDFS bağlayıcısını Etki Alanı Hesabınız veya Kerberos Sorumlusuile birlikte yapılandırın. Yapılandırma ayrıntılarıyla ilgili [HDFS Bağlantılı Hizmet özellikleri](#linked-service-properties) bölümünü kontrol edin.

> [!NOTE]
> Kaynak veri kümesinden sütunlara kadar sütunları haritalamak için Azure [Veri Fabrikası'nda veri kümesi sütunlarını eşleme](data-factory-map-columns.md)konusuna bakın.


## <a name="performance-and-tuning"></a>Performans ve Tuning
Azure Veri Fabrikası'ndaki veri hareketinin performansını etkileyen önemli faktörler (Kopyalama Etkinliği) ve bunu optimize etmenin çeşitli yolları hakkında bilgi edinmek için [Etkinlik performansını & Tuning Kılavuzu'na](data-factory-copy-activity-performance.md) bakın.
