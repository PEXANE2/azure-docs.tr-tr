---
title: Verileri bir HTTP kaynağından taşıma - Azure
description: Azure Veri Fabrikası'nı kullanarak şirket içi veya bulut HTTP kaynağından verileri nasıl taşıyarak nasıl taşıyarak verileri nasıl taşıyarak öğrenin.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: shwang
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 05/22/2018
ms.author: jingwang
robots: noindex
ms.openlocfilehash: 36592151385a08d75b9b34e85bfa9d62342fc8cd
ms.sourcegitcommit: a53fe6e9e4a4c153e9ac1a93e9335f8cf762c604
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/09/2020
ms.locfileid: "80991578"
---
# <a name="move-data-from-an-http-source-by-using-azure-data-factory"></a>Azure Veri Fabrikası'nı kullanarak verileri bir HTTP kaynağından taşıma

> [!div class="op_single_selector" title1="Kullandığınız Veri Fabrikası hizmetisürümünü seçin:"]
> * [Sürüm 1](data-factory-http-connector.md)
> * [Sürüm 2 (geçerli sürüm)](../connector-http.md)

> [!NOTE]
> Bu makale, Data Factory’nin 1. sürümü için geçerlidir. Azure Veri Fabrikası hizmetinin geçerli sürümünü kullanıyorsanız, [V2'deki HTTP bağlayıcısı bölümüne](../connector-http.md)bakın.


Bu makalede, verileri şirket içi veya bulut HTTP bitiş noktasından desteklenen bir lavabo veri deposuna taşımak için Azure Veri Fabrikası'nda Kopyalama Etkinliği'nin nasıl kullanılacağı özetlenmiştir. Bu makalede, Kopyalama Etkinliği kullanarak veri hareketine genel bir genel bakış sunan [Kopyalama Etkinliği'ni kullanarak Verileri Taşı](data-factory-data-movement-activities.md)üzerine oluşturur. Makalede ayrıca, Kopyalama Etkinliği'nin desteklediği veri depoları kaynak ve lavabo olarak da listelenir.

Veri Fabrikası şu anda yalnızca bir HTTP kaynağından diğer veri depolarına veri taşımayı destekler. Verilerin diğer veri depolarından bir HTTP hedefine taşınmasını desteklemez.

## <a name="supported-scenarios-and-authentication-types"></a>Desteklenen senaryolar ve kimlik doğrulama türleri

Bu HTTP bağlayıcısını, http **GET** veya **POST** yöntemlerini kullanarak *hem buluttan hem de şirket içi HTTP/S bitiş noktasından* veri almak için kullanabilirsiniz. Aşağıdaki kimlik doğrulama türleri desteklenir: **Anonim,** **Temel,** **Özet,** **Windows**ve **ClientCertificate.** Bu bağlayıcı ile [Web tablosu bağlayıcısı](data-factory-web-table-connector.md)arasındaki farka dikkat edin. Web tablosu bağlayıcısı tablo içeriğini bir HTML web sayfasından ayıklar.

Şirket içi bir HTTP bitiş noktasından verileri kopyaladiğinizde, Veri Yönetimi Ağ Geçidi'ni şirket içi ortama veya bir Azure VM'de yüklemeniz gerekir. Veri Yönetimi Ağ Geçidi hakkında bilgi edinmek ve ağ geçidinin nasıl ayarlanacacağına ilişkin adım adım talimatlar için, [verileri şirket içi konumlar ve bulut arasında taşıma'ya](data-factory-move-data-between-onprem-and-cloud.md)bakın.

## <a name="get-started"></a>başlarken

Farklı araçlar veya API'ler kullanarak verileri bir HTTP kaynağından taşımak için kopyalama etkinliği olan bir ardışık kaynak oluşturabilirsiniz:

- Bir ardışık iş yolu oluşturmanın en kolay yolu Veri Kopyala sihirbazını kullanmaktır. Veri Kopyalama sihirbazını kullanarak bir ardışık kaynak oluşturmanın hızlı bir şekilde gözden geçmesi için [Bkz.](data-factory-copy-data-wizard-tutorial.md)

- Bir ardışık kaynak oluşturmak için aşağıdaki araçları da kullanabilirsiniz: **Visual Studio**, **Azure PowerShell**, **Azure Kaynak Yöneticisi şablonu,** **.NET API**veya **REST API**. Kopyalama etkinliği olan bir ardışık yapının nasıl oluşturulacağına ilişkin adım adım yönergeler için Etkinlik [Kopyala öğreticisine](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)bakın. Verileri bir HTTP kaynağından Azure Blob depolamasına kopyalayan [JSON örnekleri için JSON örneklerine](#json-examples)bakın.

## <a name="linked-service-properties"></a>Bağlantılı hizmet özellikleri

Aşağıdaki tabloda HTTP bağlantılı hizmete özgü JSON öğeleri açıklanmaktadır:

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| type | **Tür** özelliği **Http**olarak ayarlanmalıdır. | Evet |
| url | Web sunucusunun temel URL'si. | Evet |
| authenticationType | Kimlik doğrulama türünü belirtir. İzin verilen değerler **Anonim,** **Temel**, **Özet,** **Windows**ve **ClientCertificate'tır.** <br><br> Daha fazla özellik için bu makaledeki sonraki bölümlere ve bu kimlik doğrulama türleri için JSON örneklerine bakın. | Evet |
| enableServerCertificateValidation | Kaynak bir HTTPS web sunucusuysa sunucu TLS/SSL sertifika doğrulamasının etkinleştirilip etkinleştirilemeyeceğini belirtir. HTTPS sunucunuz kendi imzalı bir sertifika kullandığında, bunu **yanlış**olarak ayarlayın. | Hayır<br /> (varsayılan **değer doğrudur)** |
| ağ geçidiAdı | Şirket içi bir HTTP kaynağına bağlanmak için kullanılacak Veri Yönetimi Ağ Geçidi örneğinin adı. | Evet, şirket içi http kaynağından veri kopyalıyorsanız |
| şifreli Credential | HTTP bitiş noktasına erişmek için şifrelenmiş kimlik bilgisi. Değer, Kopya sihirbazındaki kimlik doğrulama bilgilerini yapılandırdığınızda veya **ClickOnce** iletişim kutusunu kullanarak otomatik olarak oluşturulur. | Hayır<br /> (yalnızca şirket içi bir HTTP sunucusundan veri kopyalarken uygulayın) |

Şirket içi BIR HTTP bağlayıcı veri kaynağı için kimlik bilgilerini ayarlama hakkında ayrıntılı bilgi [için](data-factory-move-data-between-onprem-and-cloud.md)bkz.

### <a name="using-basic-digest-or-windows-authentication"></a>Temel, Özet veya Windows kimlik doğrulamayı kullanma

**Kimlik doğrulamasını** **Temel,** **Özet**veya **Windows**olarak ayarlayın. Önceki bölümlerde açıklanan genel HTTP bağlayıcı özelliklerine ek olarak, aşağıdaki özellikleri ayarlayın:

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| userName | HTTP bitiş noktasına erişmek için kullanılacak kullanıcı adı. | Evet |
| password | Kullanıcının şifresi (**kullanıcı adı).** | Evet |

**Örnek: Temel, Özet veya Windows kimlik doğrulamayı kullanma**

```json
{
    "name": "HttpLinkedService",
    "properties":
    {
        "type": "Http",
        "typeProperties":
        {
            "authenticationType": "basic",
            "url" : "https://en.wikipedia.org/wiki/",
            "userName": "user name",
            "password": "password"
        }
    }
}
```

### <a name="using-clientcertificate-authentication"></a>ClientCertificate kimlik doğrulamasını kullanma

Temel kimlik doğrulamasını kullanmak için **kimlik doğrulamasını** **ClientCertificate'a**yazın. Önceki bölümlerde açıklanan genel HTTP bağlayıcı özelliklerine ek olarak, aşağıdaki özellikleri ayarlayın:

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| embeddedCertData | PFX dosyasının ikili verilerinin Base64 kodlanmış içeriği. | **EmbeddedCertData** veya **certThumbprint'i belirtin** |
| certThumbprint | Ağ geçidi makinenizin sertifika deposuna yüklenen sertifikanın parmak izi. Yalnızca şirket içi bir HTTP kaynağından veri kopyalarken uygulayın. | **EmbeddedCertData** veya **certThumbprint'i belirtin** |
| password | Sertifikayla ilişkili parola. | Hayır |

Kimlik doğrulaması için **sertifika kullanırsanız** ve sertifika yerel bilgisayarın kişisel deposuna yüklenirse, ağ geçidi hizmetine okuma izinleri verir:

1. Microsoft Yönetim Konsolu'nu (MMC) açın. **Yerel Bilgisayarı**hedefleyen **Sertifikaları** ekleme.
2. **Kişisel Sertifikaları** > **Personal**Genişletin ve ardından **Sertifikaları**seçin.
3. Kişisel mağazadan sertifikayı sağ tıklatın ve ardından **Tüm Görevler** >**Özel Tuşları Yönet'i**seçin.
3. **Güvenlik** sekmesinde, sertifikaya okuma erişimi yle birlikte Veri Yönetimi Ağ Geçidi Ana Bilgisayarı Hizmetinin çalıştırıldığı kullanıcı hesabını ekleyin.  

**Örnek: İstemci sertifikası kullanma**

Bu bağlantılı hizmet, veri fabrikanızı şirket içi BIR HTTP web sunucusuna bağlar. Veri Yönetimi Ağ Geçidi yüklü olan makineye yüklenen bir istemci sertifikası kullanır.

```json
{
    "name": "HttpLinkedService",
    "properties":
    {
        "type": "Http",
        "typeProperties":
        {
            "authenticationType": "ClientCertificate",
            "url": "https://en.wikipedia.org/wiki/",
            "certThumbprint": "thumbprint of certificate",
            "gatewayName": "gateway name"

        }
    }
}
```

**Örnek: Bir dosyada istemci sertifikası kullanma**

Bu bağlantılı hizmet, veri fabrikanızı şirket içi BIR HTTP web sunucusuna bağlar. Veri Yönetimi Ağ Geçidi yüklü olan makinede bir istemci sertifika dosyası kullanır.

```json
{
    "name": "HttpLinkedService",
    "properties":
    {
        "type": "Http",
        "typeProperties":
        {
            "authenticationType": "ClientCertificate",
            "url": "https://en.wikipedia.org/wiki/",
            "embeddedCertData": "Base64-encoded cert data",
            "password": "password of cert"
        }
    }
}
```

## <a name="dataset-properties"></a>Veri kümesi özellikleri

Bir veri kümesi JSON dosyasının yapı, kullanılabilirlik ve ilke gibi bazı bölümleri tüm veri kümesi türleri (Azure SQL Veritabanı, Azure Blob depolama, Azure Tablo depolama) için benzerdir.

Veri kümelerini tanımlamak için kullanılabilen bölümlerin ve özelliklerin tam listesi için [bkz.](data-factory-create-datasets.md)

**typeProperties** bölümü her veri kümesi türü için farklıdır. **typeProperties** bölümü, veri deposundaki verilerin konumu hakkında bilgi sağlar. **Http** türündeki bir veri kümesinin **typeProperties** bölümü aşağıdaki özelliklere sahiptir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Veri kümesinin **türü** **Http**olarak ayarlanmalıdır. | Evet |
| Relativeurl | Verileri içeren kaynağa göreli bir URL. Yol belirtilmediği zaman, yalnızca bağlantılı hizmet tanımında belirtilen URL kullanılır. <br><br> Dinamik bir URL oluşturmak için [Veri Fabrikası işlevlerini ve sistem değişkenlerini](data-factory-functions-variables.md)kullanabilirsiniz. Örnek: **relativeUrl**: **$$Text.Format('/my/report?month={0:yyyy}-{0:MM}&fmt=csv', SliceStart)**. | Hayır |
| requestMethod | HTTP yöntemi. İzin verilen değerler **GET** ve **POST'tür.** | Hayır <br />(varsayılan **GET'** dir) |
| ek Headers | Ek HTTP istek üstbilgi. | Hayır |
| requestBody | HTTP isteği için gövde. | Hayır |
| biçim | Verileri ayrıştırmadan *olduğu gibi BIR HTTP bitiş noktasından almak* istiyorsanız, **biçim** ayarını atlayın. <br><br> Kopyalama sırasında HTTP yanıt içeriğini ayrışdırmak istiyorsanız, aşağıdaki biçim türleri desteklenir: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, ve **ParkquetFormat**. Daha fazla bilgi için [Metin biçimi,](data-factory-supported-file-and-compression-formats.md#text-format) [JSON biçimi,](data-factory-supported-file-and-compression-formats.md#json-format) [Avro formatı,](data-factory-supported-file-and-compression-formats.md#avro-format) [Ork formatı](data-factory-supported-file-and-compression-formats.md#orc-format)ve [Parke formatına](data-factory-supported-file-and-compression-formats.md#parquet-format)bakın. |Hayır |
| sıkıştırma | Verilerin sıkıştırma türünü ve düzeyini belirtin. Desteklenen türleri: **GZip**, **Deflate**, **BZip2**, ve **ZipDeflate**. Desteklenen seviyeler: **Optimal** ve **En Hızlı**. Daha fazla bilgi için [Azure Veri Fabrikası'ndaki Dosya ve sıkıştırma biçimlerine](data-factory-supported-file-and-compression-formats.md#compression-support)bakın. |Hayır |

**Örnek: GET (varsayılan) yöntemini kullanma**

```json
{
    "name": "HttpSourceDataInput",
    "properties": {
        "type": "Http",
        "linkedServiceName": "HttpLinkedService",
        "typeProperties": {
            "relativeUrl": "XXX/test.xml",
            "additionalHeaders": "Connection: keep-alive\nUser-Agent: Mozilla/5.0\n"
        },
        "external": true,
        "availability": {
            "frequency": "Hour",
            "interval":  1
        }
    }
}
```

**Örnek: POST yöntemini kullanma**

```json
{
    "name": "HttpSourceDataInput",
    "properties": {
        "type": "Http",
        "linkedServiceName": "HttpLinkedService",
        "typeProperties": {
            "relativeUrl": "/XXX/test.xml",
           "requestMethod": "Post",
            "requestBody": "body for POST HTTP request"
        },
        "external": true,
        "availability": {
            "frequency": "Hour",
            "interval":  1
        }
    }
}
```

## <a name="copy-activity-properties"></a>Etkinlik özelliklerini kopyalama

Ad, açıklama, giriş ve çıktı tabloları ve ilke gibi özellikler tüm etkinlik türleri için kullanılabilir.

Etkinlikleri tanımlamak için kullanılabilen bölümlerin ve özelliklerin tam listesi için [bkz.](data-factory-create-pipelines.md) 

Etkinliğin **typeProperties** bölümünde bulunan özellikler her etkinlik türüne göre değişir. Kopyalama etkinliği için özellikler kaynak ve lavabo türlerine bağlı olarak değişir.

Şu anda, Kopyalama Etkinliği'ndeki kaynak **HttpSource** türünde olduğunda, aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
| -------- | ----------- | -------- |
| httpRequestTimeout | Yanıt almak için HTTP isteğiiçin zaman aralığı **(TimeSpan** değeri). Yanıt verilerini okumak için zaman arası değil, yanıt almak için zaman aralarıdır. | Hayır<br />(varsayılan değer: **00:01:40**) |

## <a name="supported-file-and-compression-formats"></a>Desteklenen dosya ve sıkıştırma biçimleri

Daha fazla bilgi için [Azure Veri Fabrikası'ndaki Dosya ve sıkıştırma biçimlerine](data-factory-supported-file-and-compression-formats.md) bakın.

## <a name="json-examples"></a>JSON örnekleri

Aşağıdaki örnekler, [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) veya [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md)kullanarak bir ardışık hat lar oluşturmak için kullanabileceğiniz örnek JSON tanımları sağlar. Örnekler, bir HTTP kaynağından Azure Blob depolama alanına verilerin nasıl kopyalanır olduğunu gösterir. Ancak, veriler *doğrudan* kaynaklardan herhangi birinden Azure Veri Fabrikası'nda Kopyalama Etkinliği kullanılarak [desteklenen](data-factory-data-movement-activities.md#supported-data-stores-and-formats) lavabolara kopyalanabilir.

**Örnek: Verileri bir HTTP kaynağından Azure Blob depolama alanına kopyalama**

Bu örnek için Veri Fabrikası çözümü aşağıdaki Veri Fabrikası varlıklarını içerir:

*   TÜR [HTTP](#linked-service-properties)bağlantılı bir hizmet .
*   [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties)türüne bağlı bir hizmet.
*   [Tür http](#dataset-properties)bir giriş [veri kümesi](data-factory-create-datasets.md) .
*   [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties)türünden bir çıktı [veri kümesi.](data-factory-create-datasets.md)
*   [HttpSource](#copy-activity-properties) ve [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties)kullanan bir kopyalama etkinliği olan bir [ardışık kaynak.](data-factory-create-pipelines.md)

Örnek, verileri bir HTTP kaynağından her saat başı Azure blob'una kopyalar. Bu örneklerde kullanılan JSON özellikleri, örnekleri izleyen bölümlerde açıklanmıştır.

### <a name="http-linked-service"></a>HTTP bağlantılı hizmet

Bu örnek, anonim kimlik doğrulaması ile HTTP bağlantılı hizmeti kullanır. Kullanabileceğiniz farklı kimlik doğrulama türleri için [HTTP bağlantılı hizmete](#linked-service-properties) bakın.

```json
{
    "name": "HttpLinkedService",
    "properties":
    {
        "type": "Http",
        "typeProperties":
        {
            "authenticationType": "Anonymous",
            "url" : "https://en.wikipedia.org/wiki/"
        }
    }
}
```

### <a name="azure-storage-linked-service"></a>Azure depolama bağlantılı hizmet

```json
{
  "name": "AzureStorageLinkedService",
  "properties": {
    "type": "AzureStorage",
    "typeProperties": {
      "connectionString": "DefaultEndpointsProtocol=https;AccountName=<account name>;AccountKey=<account key>"
    }
  }
}
```

### <a name="http-input-dataset"></a>HTTP giriş veri seti

**True'nun** **dışında** ayar, Veri Fabrikası hizmetine veri kümesinin veri fabrikasının dışında olduğunu ve veri fabrikasındaki bir etkinlik tarafından üretilmediğini bildirir.

```json
{
    "name": "HttpSourceDataInput",
    "properties": {
        "type": "Http",
        "linkedServiceName": "HttpLinkedService",
        "typeProperties": {
            "relativeUrl": "$$Text.Format('/my/report?month={0:yyyy}-{0:MM}&fmt=csv', SliceStart)",
            "additionalHeaders": "Connection: keep-alive\nUser-Agent: Mozilla/5.0\n"
        },
        "external": true,
        "availability": {
            "frequency": "Hour",
            "interval":  1
        }
    }
}

```

### <a name="azure-blob-output-dataset"></a>Azure blob çıktı veri kümesi

Veriler her saat yeni bir blob yazılır (**frekans**: **saat**, **aralık**: **1**).

```json
{
    "name": "AzureBlobOutput",
    "properties":
    {
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
        "typeProperties":
        {
            "folderPath": "adfgetstarted/Movies"
        },
        "availability":
        {
            "frequency": "Hour",
            "interval": 1
        }
    }
}
```

### <a name="pipeline-that-uses-a-copy-activity"></a>Kopyalama etkinliği kullanan ardışık hatlar

Ardışık iş, giriş ve çıktı veri kümelerini kullanacak şekilde yapılandırılan bir kopyalama etkinliği içerir. Kopyalama etkinliği her saat çalışacak şekilde zamanlanır. Boru hattı JSON tanımında, **kaynak** türü **HttpSource** olarak ayarlanır ve **lavabo** türü **BlobSink**olarak ayarlanır.

**HttpSource'un** desteklediği özellikler listesi için [Bkz. HttpSource.](#copy-activity-properties)

```json
{  
    "name":"SamplePipeline",
    "properties":{  
    "start":"2014-06-01T18:00:00",
    "end":"2014-06-01T19:00:00",
    "description":"pipeline with a copy activity",
    "activities":[  
      {
        "name": "HttpSourceToAzureBlob",
        "description": "Copy from an HTTP source to an Azure blob",
        "type": "Copy",
        "inputs": [
          {
            "name": "HttpSourceDataInput"
          }
        ],
        "outputs": [
          {
            "name": "AzureBlobOutput"
          }
        ],
        "typeProperties": {
          "source": {
            "type": "HttpSource"
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
> Bir kaynak veri kümesinden sütunlara sütunları eşlemek için Azure [Veri Fabrikası'nda veri kümesi sütunlarını eşleme](data-factory-map-columns.md)konusuna bakın.

## <a name="performance-and-tuning"></a>Performans ve ayar

Azure Veri Fabrikası'ndaki veri hareketinin (Kopyalama Etkinliği) performansını etkileyen önemli etkenler ve bunu optimize etmenin çeşitli yolları hakkında bilgi edinmek için [Kopyalama Etkinliği performansı ve atokslama kılavuzuna](data-factory-copy-activity-performance.md)bakın.
