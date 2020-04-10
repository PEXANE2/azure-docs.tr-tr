---
title: Azure Veri Fabrikası'nı kullanarak bir FTP sunucusundan veri taşıma
description: Azure Veri Fabrikası'nı kullanarak bir FTP sunucusundan verileri nasıl taşıyabilirsiniz hakkında bilgi edinin.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: shwang
ms.assetid: eea3bab0-a6e4-4045-ad44-9ce06229c718
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 05/02/2018
ms.author: jingwang
robots: noindex
ms.openlocfilehash: eeeb122d240d8c3eae4ebe1650f67cf0e4b9dac6
ms.sourcegitcommit: a53fe6e9e4a4c153e9ac1a93e9335f8cf762c604
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/09/2020
ms.locfileid: "80992054"
---
# <a name="move-data-from-an-ftp-server-by-using-azure-data-factory"></a>Azure Veri Fabrikası'nı kullanarak bir FTP sunucusundan veri taşıma
> [!div class="op_single_selector" title1="Kullandığınız Veri Fabrikası hizmetisürümünü seçin:"]
> * [Sürüm 1](data-factory-ftp-connector.md)
> * [Sürüm 2 (geçerli sürüm)](../connector-ftp.md)

> [!NOTE]
> Bu makale, Data Factory’nin 1. sürümü için geçerlidir. Veri Fabrikası hizmetinin geçerli sürümünü kullanıyorsanız, [V2'deki FTP konektörüne](../connector-ftp.md)bakın.

Bu makalede, bir FTP sunucusundan veri taşımak için Azure Veri Fabrikası'ndaki kopyalama etkinliğinin nasıl kullanılacağı açıklanmaktadır. Kopya etkinliğiyle birlikte veri hareketine genel bir genel bakış sunan [Veri hareketi etkinlikleri](data-factory-data-movement-activities.md) makalesine dayanıyor.

Bir FTP sunucusundan desteklenen herhangi bir lavabo veri deposuna verileri kopyalayabilirsiniz. Kopyalama etkinliği tarafından lavabo olarak desteklenen veri depolarının listesi için [desteklenen veri depoları](data-factory-data-movement-activities.md#supported-data-stores-and-formats) tablosuna bakın. Veri Fabrikası şu anda yalnızca ftp sunucusundan diğer veri depolarına veri taşımayı destekler, ancak verileri diğer veri depolarından FTP sunucusuna taşımaz. Hem şirket içi hem de bulut FTP sunucularını destekler.

> [!NOTE]
> Kopyalama etkinliği, hedefe başarıyla kopyalandıktan sonra kaynak dosyayı silmez. Başarılı bir kopyadan sonra kaynak dosyayı silmeniz gerekiyorsa, dosyayı silmek için özel bir etkinlik oluşturun ve ardışık düzendeki etkinliği kullanın.

## <a name="enable-connectivity"></a>Bağlantıyı etkinleştirme
Verileri **şirket içi** FTP sunucusundan bir bulut veri deposuna (örneğin, Azure Blob depolamasına) taşıyorsanız, Veri Yönetimi Ağ Geçidi'ni yükleyin ve kullanın. Veri Yönetimi Ağ Geçidi, şirket içi makinenize yüklenen bir istemci aracısıdır ve bulut hizmetlerinin şirket içi bir kaynağa bağlanmasına olanak tanır. Ayrıntılar için [Veri Yönetimi Ağ Geçidi'ne](data-factory-data-management-gateway.md)bakın. Ağ geçidini kurma ve kullanma yla ilgili adım adım talimatlar için, [verileri şirket içi konumlar ve bulut arasında taşıma'ya](data-factory-move-data-between-onprem-and-cloud.md)bakın. Sunucu bir hizmet (IaaS) sanal makine (VM) olarak Bir Azure altyapısında olsa bile, bir FTP sunucusuna bağlanmak için ağ geçidini kullanırsınız.

Ağ geçidini FTP sunucusuyla aynı şirket içi makineye veya IaaS VM'ye yüklemek mümkündür. Ancak, kaynak çekişmesini önlemek ve daha iyi performans için ağ geçidini ayrı bir makineye veya IaaS VM'ye yüklemenizi öneririz. Ağ geçidini ayrı bir makineye yüklediğinizde, makine FTP sunucusuna erişebilmeli.

## <a name="get-started"></a>başlarken
Farklı araçlar veya API'ler kullanarak bir FTP kaynağından veri hareket ettiren bir kopyalama etkinliği içeren bir ardışık hatlar oluşturabilirsiniz.

Bir ardışık hatlar oluşturmanın en kolay yolu **Veri Fabrikası Kopyalama Sihirbazı'nı**kullanmaktır. Bkz. Öğretici: Hızlı bir geçiş için [Kopya Sihirbazı'nı kullanarak bir ardışık kaynak oluşturun.](data-factory-copy-data-wizard-tutorial.md)

Bir ardışık kaynak oluşturmak için aşağıdaki araçları da kullanabilirsiniz: **Visual Studio**, **PowerShell**, **Azure Kaynak Yöneticisi şablonu**, **.NET API**ve **REST API**. Kopyalama etkinliği içeren bir ardışık hatlar oluşturmak için adım adım yönergeleri için [etkinlik öğreticisini](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) kopyala'ya bakın.

Araçları veya API'leri kullanın, verileri kaynak veri deposundan bir lavabo veri deposuna aktaran bir ardışık işlem hattı oluşturmak için aşağıdaki adımları gerçekleştirin:

1. Giriş ve çıktı veri depolarını veri fabrikanıza bağlamak için **bağlantılı hizmetler** oluşturun.
2. Kopyalama işlemi için giriş ve çıktı verilerini temsil edecek **veri kümeleri** oluşturun.
3. Giriş olarak veri kümesi ve çıktı olarak veri kümesi alan bir kopyalama etkinliği içeren bir **ardışık işlem oluşturma.**

Sihirbazı kullandığınızda, bu Veri Fabrikası varlıkları (bağlantılı hizmetler, veri kümeleri ve ardışık kuruluş) için JSON tanımları sizin için otomatik olarak oluşturulur. Araçlar veya API'ler (.NET API hariç) kullandığınızda, Bu Veri Fabrikası varlıklarını JSON biçimini kullanarak tanımlarsınız. FTP veri deposundan veri kopyalamak için kullanılan Veri Fabrikası varlıkları için JSON tanımlı bir örnek [için JSON örneğine bakın: FTP sunucusundan](#json-example-copy-data-from-ftp-server-to-azure-blob) bu makalenin Azure blob bölümüne veri kopyalama.

> [!NOTE]
> Kullanılacak desteklenen dosya ve sıkıştırma biçimleri hakkında ayrıntılı bilgi için [Azure Veri Fabrikası'nda Dosya ve sıkıştırma biçimlerine](data-factory-supported-file-and-compression-formats.md)bakın.

Aşağıdaki bölümler, FTP'ye özgü Veri Fabrikası varlıklarını tanımlamak için kullanılan JSON özellikleri hakkında ayrıntılı bilgi sağlar.

## <a name="linked-service-properties"></a>Bağlantılı hizmet özellikleri
Aşağıdaki tabloda FTP bağlantılı bir hizmete özgü JSON öğeleri açıklanmaktadır.

| Özellik | Açıklama | Gerekli | Varsayılan |
| --- | --- | --- | --- |
| type |Bunu FtpServer olarak ayarlayın. |Evet |&nbsp; |
| konak |FTP sunucusunun adını veya IP adresini belirtin. |Evet |&nbsp; |
| authenticationType |Kimlik doğrulama türünü belirtin. |Evet |Temel, Anonim |
| kullanıcı adı |FTP sunucusuna erişimi olan kullanıcıyı belirtin. |Hayır |&nbsp; |
| password |Kullanıcının parolasını (kullanıcı adı) belirtin. |Hayır |&nbsp; |
| şifreli Credential |FTP sunucusuna erişmek için şifrelenmiş kimlik bilgilerini belirtin. |Hayır |&nbsp; |
| ağ geçidiAdı |Şirket içi ftp sunucusuna bağlanmak için Veri Yönetimi Ağ Geçidi'ndeki ağ geçidinin adını belirtin. |Hayır |&nbsp; |
| port |FTP sunucusunun dinlediği bağlantı noktasını belirtin. |Hayır |21 |
| sağlarSsl |FTP'yi bir SSL/TLS kanalı üzerinden kullanıp kullanmayacağını belirtin. |Hayır |true |
| enableServerCertificateValidation |SSL/TLS kanalı üzerinden FTP kullanırken sunucu TLS/SSL sertifika doğrulamasını etkinleştirip etkinleştirmeyeceğiniz belirtin. |Hayır |true |

>[!NOTE]
>FTP konektörü FTP sunucusuna şifreleme veya açık SSL/TLS şifrelemesi olmadan erişmeyi destekler; örtülü SSL/TLS şifrelemeyi desteklemez.

### <a name="use-anonymous-authentication"></a>Anonim kimlik doğrulamasını kullanma

```JSON
{
    "name": "FTPLinkedService",
    "properties": {
        "type": "FtpServer",
        "typeProperties": {
            "authenticationType": "Anonymous",
              "host": "myftpserver.com"
        }
    }
}
```

### <a name="use-username-and-password-in-plain-text-for-basic-authentication"></a>Temel kimlik doğrulama için kullanıcı adı ve parolayı düz metin olarak kullanma

```JSON
{
    "name": "FTPLinkedService",
    "properties": {
    "type": "FtpServer",
        "typeProperties": {
            "host": "myftpserver.com",
            "authenticationType": "Basic",
            "username": "Admin",
            "password": "123456"
        }
    }
}
```

### <a name="use-port-enablessl-enableservercertificatevalidation"></a>Bağlantı noktasını kullanın, enableSsl, enableServerCertificateValidation

```JSON
{
    "name": "FTPLinkedService",
    "properties": {
        "type": "FtpServer",
        "typeProperties": {
            "host": "myftpserver.com",
            "authenticationType": "Basic",
            "username": "Admin",
            "password": "123456",
            "port": "21",
            "enableSsl": true,
            "enableServerCertificateValidation": true
        }
    }
}
```

### <a name="use-encryptedcredential-for-authentication-and-gateway"></a>Kimlik doğrulama ve ağ geçidi için şifreli Credential'ı kullanma

```JSON
{
    "name": "FTPLinkedService",
    "properties": {
        "type": "FtpServer",
        "typeProperties": {
            "host": "myftpserver.com",
            "authenticationType": "Basic",
            "encryptedCredential": "xxxxxxxxxxxxxxxxx",
            "gatewayName": "mygateway"
        }
    }
}
```

## <a name="dataset-properties"></a>Veri kümesi özellikleri
Veri kümelerini tanımlamak için kullanılabilen bölümlerin ve özelliklerin tam listesi için [bkz.](data-factory-create-datasets.md) Bir veri kümesi JSON'un yapısı, kullanılabilirliği ve ilkesi gibi bölümler tüm veri kümesi türleri için benzerdir.

**typeProperties** bölümü her veri kümesi türü için farklıdır. Veri kümesi türüne özgü bilgiler sağlar. **FileShare** türünden bir veri kümesi için **typeProperties** bölümü aşağıdaki özelliklere sahiptir:

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| folderPath |Klasöre alt yol. Dizedeki özel karakterler için ' \ ' kaçış karakterini kullanın. Örnekler için Örnek bağlantılı hizmet ve veri kümesi tanımlarına bakın.<br/><br/>Bu özelliği **partitionBy** ile birleştirerek dilim başlangıç ve bitiş tarihlerine göre klasör yolları bulabilirsiniz. |Evet |
| fileName |Tablonun klasördeki belirli bir dosyaya başvurmasını istiyorsanız **klasörDeki Dosyanın** adını belirtinPath. Bu özellik için herhangi bir değer belirtmezseniz, tablo klasördeki tüm dosyaları işaret edir.<br/><br/>Bir çıktı veri kümesi için **dosya Adı** belirtilmediğinde, oluşturulan dosyanın adı aşağıdaki biçimdedir: <br/><br/>`Data.<Guid>.txt`(Örnek: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt) |Hayır |
| dosyaFiltre |Tüm dosyalar yerine **folderPath'deki**dosyaların bir alt kümesini seçmek için kullanılacak bir filtre belirtin.<br/><br/>İzin verilen `*` değerler şunlardır: `?` (birden çok karakter) ve (tek karakter).<br/><br/>Örnek 1:`"fileFilter": "*.log"`<br/>Örnek 2:`"fileFilter": 2014-1-?.txt"`<br/><br/> **fileFilter** bir giriş FileShare veri kümesi için geçerlidir. Bu özellik Hadoop Dağıtılmış Dosya Sistemi (HDFS) ile desteklenmez. |Hayır |
| bölümlemeBy |Zaman serisi verileri için dinamik bir **klasörPath** ve **fileName** belirtmek için kullanılır. Örneğin, her veri saati için parametreli bir **klasörPath** belirtebilirsiniz. |Hayır |
| biçim | Aşağıdaki biçim türleri desteklenir: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParkeFormat**. Biçim altındaki **tür** özelliğini bu değerlerden birine ayarlayın. Daha fazla bilgi için [Metin Biçimi,](data-factory-supported-file-and-compression-formats.md#text-format) [Json Formatı,](data-factory-supported-file-and-compression-formats.md#json-format) [Avro Biçimi,](data-factory-supported-file-and-compression-formats.md#avro-format) [Ork Formatı](data-factory-supported-file-and-compression-formats.md#orc-format)ve [Parke Formatı](data-factory-supported-file-and-compression-formats.md#parquet-format) bölümlerine bakın. <br><br> Dosyaları dosya tabanlı mağazalar (ikili kopya) arasında olduğu gibi kopyalamak istiyorsanız, hem giriş hem de çıktı veri kümesi tanımlarında biçim bölümünü atlayın. |Hayır |
| sıkıştırma | Verilerin sıkıştırma türünü ve düzeyini belirtin. Desteklenen türleri **GZip**vardır , **Deflate**, **BZip2**, ve **ZipDeflate**, ve desteklenen düzeyleri **Optimal** ve **Hızlı**. Daha fazla bilgi için [Azure Veri Fabrikası'ndaki Dosya ve sıkıştırma biçimlerine](data-factory-supported-file-and-compression-formats.md#compression-support)bakın. |Hayır |
| useBinaryTransfer |İkili aktarım modunu kullanıp kullanmayacağını belirtin. Değerler ikili mod için doğrudur (bu varsayılan değerdir) ve ASCII için yanlıştır. Bu özellik yalnızca ilişkili bağlantılı hizmet türü ftpServer türünde olduğunda kullanılabilir. |Hayır |

> [!NOTE]
> **fileName** ve **fileFilter** aynı anda kullanılamaz.

### <a name="use-the-partionedby-property"></a>PartionedBy özelliğini kullanma
Önceki bölümde belirtildiği gibi, **bölümlenmişBy** özelliği ile zaman serisi verileri için dinamik bir **klasörPath** ve **fileName** belirtebilirsiniz.

Zaman serisi veri kümeleri, zamanlama ve dilimler hakkında bilgi [Scheduling and execution](data-factory-scheduling-and-execution.md)edinmek için [bkz.](data-factory-create-datasets.md) [Creating pipelines](data-factory-create-pipelines.md)

#### <a name="sample-1"></a>Örnek 1

```json
"folderPath": "wikidatagateway/wikisampledataout/{Slice}",
"partitionedBy":
[
    { "name": "Slice", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyyMMddHH" } },
],
```
Bu örnekte, {Slice} veri fabrikası sistem değişkeni SliceStart değeri ile, belirtilen biçimde (YYYYMMDDHH) değiştirilir. SliceStart, dilimin başlangıç saatini ifade eder. Klasör yolu her dilim için farklıdır. (Örneğin, wikidatagateway/wikisampledataout/2014100103 veya wikidatagateway/wikisampledataout/2014100104.)

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
Bu örnekte, SliceStart'ın yılı, ayı, günü ve saati **folderPath** ve **fileName** özellikleri tarafından kullanılan ayrı değişkenlere ayıklanır.

## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri
Etkinlikleri tanımlamak için kullanılabilen bölümlerin ve özelliklerin tam listesi için [bkz.](data-factory-create-pipelines.md) Ad, açıklama, giriş ve çıktı tabloları ve ilkeler gibi özellikler tüm etkinlik türleri için kullanılabilir.

Diğer taraftan, etkinliğin **typeProperties** bölümünde bulunan özellikler her etkinlik türüne göre değişir. Kopyalama etkinliği için tür özellikleri kaynak ve lavabo türlerine bağlı olarak değişir.

Kopyalama etkinliğinde, kaynak **FileSystemSource**türünde olduğunda, **typeProperties** bölümünde aşağıdaki özellik kullanılabilir:

| Özellik | Açıklama | İzin verilen değerler | Gerekli |
| --- | --- | --- | --- |
| Özyinelemeli |Verilerin alt klasörlerden mi yoksa yalnızca belirtilen klasörden mi özyinelemeli olarak okunduğunu gösterir. |True, False (varsayılan) |Hayır |

## <a name="json-example-copy-data-from-ftp-server-to-azure-blob"></a>JSON örneği: FTP sunucusundan Azure Blob'a veri kopyalama
Bu örnek, ftp sunucusundan Azure Blob depolamaalanına verilerin nasıl kopyalanır olduğunu gösterir. Ancak, veriler, Veri Fabrikası'ndaki kopyalama etkinliği [kullanılarak, desteklenen veri depolarında ve biçimlerinde](data-factory-data-movement-activities.md#supported-data-stores-and-formats)belirtilen lavabolardan herhangi biri için doğrudan kopyalanabilir.

Aşağıdaki örnekler, [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md)veya [PowerShell](data-factory-copy-activity-tutorial-using-powershell.md)kullanarak bir boru hattı oluşturmak için kullanabileceğiniz örnek JSON tanımları sağlar:

* [FtpServer](#linked-service-properties) türünde bağlantılı bir hizmet
* [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties) türünün bağlantılı bir hizmeti
* [FileShare](#dataset-properties) türünden bir giriş [veri kümesi](data-factory-create-datasets.md)
* [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties) türünden bir çıktı [veri kümesi](data-factory-create-datasets.md)
* [FileSystemSource](#copy-activity-properties) ve [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties) kullanan kopyalama etkinliği olan bir [ardışık düzen](data-factory-create-pipelines.md)

Örnek, ftp sunucusundaki verileri her saat başı bir Azure blob'una kopyalar. Bu örneklerde kullanılan JSON özellikleri, örnekleri izleyen bölümlerde açıklanmıştır.

### <a name="ftp-linked-service"></a>FTP bağlantılı hizmet

Bu örnek, kullanıcı adı ve paroladüz metinde yer alan temel kimlik doğrulamasını kullanır. Aşağıdaki yollardan birini de kullanabilirsiniz:

* Anonim kimlik doğrulama
* Şifreli kimlik bilgileriyle temel kimlik doğrulaması
* SSL/TLS üzerinden FTP (FTPS)

Kullanabileceğiniz farklı kimlik doğrulama türleri için [FTP bağlantılı hizmet](#linked-service-properties) bölümüne bakın.

```JSON
{
    "name": "FTPLinkedService",
    "properties": {
        "type": "FtpServer",
        "typeProperties": {
            "host": "myftpserver.com",
            "authenticationType": "Basic",
            "username": "Admin",
            "password": "123456"
        }
    }
}
```
### <a name="azure-storage-linked-service"></a>Azure Storage bağlı hizmeti

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
### <a name="ftp-input-dataset"></a>FTP giriş veri seti

Bu veri kümesi FTP `mysharedfolder` klasörü `test.csv`ve dosyası anlamına gelir. Ardışık iş, dosyayı hedefe kopyalar.

**True'nun** **dışında** ayar, Veri Fabrikası hizmetine veri kümesinin veri fabrikasının dışında olduğunu ve veri fabrikasındaki bir etkinlik tarafından üretilmediğini bildirir.

```JSON
{
  "name": "FTPFileInput",
  "properties": {
    "type": "FileShare",
    "linkedServiceName": "FTPLinkedService",
    "typeProperties": {
      "folderPath": "mysharedfolder",
      "fileName": "test.csv",
      "useBinaryTransfer": true
    },
    "external": true,
    "availability": {
      "frequency": "Hour",
      "interval": 1
    }
  }
}
```

### <a name="azure-blob-output-dataset"></a>Azure Blob çıktı veri kümesi

Veriler her saat yeni bir blob 'a yazılır (sıklık: saat, aralık: 1). Blob için klasör yolu, işlenen dilimin başlangıç saatine bağlı olarak dinamik olarak değerlendirilir. Klasör yolu, başlangıç zamanının yıl, ay, gün ve saat bölümlerini kullanır.

```JSON
{
    "name": "AzureBlobOutput",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
        "typeProperties": {
            "folderPath": "mycontainer/ftp/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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


### <a name="a-copy-activity-in-a-pipeline-with-file-system-source-and-blob-sink"></a>Dosya sistemi kaynağı ve blob lavabosu olan bir boru hattında kopyalama etkinliği

Ardışık iş, giriş ve çıktı veri kümelerini kullanacak şekilde yapılandırılan ve her saat çalışacak şekilde zamanlanan bir kopyalama etkinliği içerir. Boru hattı JSON tanımında, **kaynak** türü **FileSystemSource**olarak ayarlanır ve **lavabo** türü **BlobSink**olarak ayarlanır.

```JSON
{
    "name": "pipeline",
    "properties": {
        "activities": [{
            "name": "FTPToBlobCopy",
            "inputs": [{
                "name": "FtpFileInput"
            }],
            "outputs": [{
                "name": "AzureBlobOutput"
            }],
            "type": "Copy",
            "typeProperties": {
                "source": {
                    "type": "FileSystemSource"
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
                "executionPriorityOrder": "NewestFirst",
                "retry": 1,
                "timeout": "00:05:00"
            }
        }],
        "start": "2016-08-24T18:00:00Z",
        "end": "2016-08-24T19:00:00Z"
    }
}
```
> [!NOTE]
> Kaynak veri kümesinden sütunlara kadar sütunları haritalamak için Azure [Veri Fabrikası'nda veri kümesi sütunlarını eşleme](data-factory-map-columns.md)konusuna bakın.

## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki makalelere bakın:

* Veri Fabrikası'ndaki veri hareketinin performansını (kopyalama etkinliği) etkileyen önemli faktörler ve bunu optimize etmenin çeşitli yolları hakkında bilgi edinmek için [Kopyalama etkinliği performansı ve atokskılavuzuna](data-factory-copy-activity-performance.md)bakın.

* Kopyalama etkinliği içeren bir ardışık hatlar oluşturmak için adım adım yönergeler için [Kopyalama etkinliği öğreticisine](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)bakın.
