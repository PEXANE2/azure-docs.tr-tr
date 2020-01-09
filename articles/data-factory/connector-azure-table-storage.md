---
title: Azure Tablo depolamadan veri kopyalama
description: Desteklenen kaynak depolardan Azure Tablo depolama 'ya veya tablo depolamadan Data Factory kullanarak verileri desteklenen havuz depolarına kopyalamayı öğrenin.
services: data-factory
ms.author: jingwang
author: linda33wj
manager: shwang
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 08/27/2019
ms.openlocfilehash: b64b0f32b7e8d94115facf43646a5a030697d80f
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75444424"
---
# <a name="copy-data-to-and-from-azure-table-storage-by-using-azure-data-factory"></a>Azure Data Factory kullanarak Azure Tablo depolamadan veri kopyalama

> [!div class="op_single_selector" title1="Kullandığınız Data Factory hizmeti sürümünü seçin:"]
> * [Sürüm 1](v1/data-factory-azure-table-connector.md)
> * [Geçerli sürüm](connector-azure-table-storage.md)

Bu makalede, Azure Tablo depolamadan veri kopyalamak için Azure Data Factory kopyalama etkinliğinin nasıl kullanılacağı özetlenmektedir. Yapılar [kopyalama etkinliğine genel bakış](copy-activity-overview.md) kopyalama etkinliği genel bir bakış sunan makalesi.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="supported-capabilities"></a>Desteklenen özellikler

Bu Azure Tablo depolama Bağlayıcısı aşağıdaki etkinlikler için desteklenir:

- [Desteklenen kaynak/havuz matrisi](copy-activity-overview.md) ile [kopyalama etkinliği](copy-activity-overview.md)
- [Arama etkinliği](control-flow-lookup-activity.md)

Desteklenen herhangi bir kaynak veri deposundan tablo depolamaya veri kopyalayabilirsiniz. Ayrıca, tablo depolamadan desteklenen herhangi bir havuz veri deposuna veri kopyalayabilirsiniz. Kopyalama etkinliği tarafından kaynak ve havuz desteklenen veri depolarının listesi için bkz. [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats) tablo.

Özellikle, bu Azure Tablo Bağlayıcısı, hesap anahtarı ve hizmet paylaşılan erişim imzası kimlik doğrulamaları kullanılarak verilerin kopyalanmasını destekler.

## <a name="get-started"></a>Kullanmaya Başlayın

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Aşağıdaki bölümler tablo depolamaya özgü Data Factory varlıkları tanımlamak için kullanılan özellikler hakkında ayrıntılı bilgi sağlar.

## <a name="linked-service-properties"></a>Bağlı hizmeti özellikleri

### <a name="use-an-account-key"></a>Hesap anahtarı kullan

Hesap anahtarını kullanarak bir Azure depolama bağlı hizmeti oluşturabilirsiniz. Veri fabrikasına depolama için genel erişim sağlar. Aşağıdaki özellikler desteklenir.

| Özellik | Açıklama | Gereklidir |
|:--- |:--- |:--- |
| type | Type özelliği **AzureTableStorage**olarak ayarlanmalıdır. |Evet |
| connectionString | ConnectionString özelliği için depolama alanına bağlanmak için gereken bilgileri belirtin. <br/>Ayrıca hesap anahtarını Azure Key Vault yerleştirebilir ve `accountKey` yapılandırmasını bağlantı dizesinden dışarı çekebilirsiniz. Daha ayrıntılı bilgi için aşağıdaki örneklere bakın ve [kimlik bilgilerini Azure Key Vault makalesine depolayın](store-credentials-in-key-vault.md) . |Evet |
| connectVia | [Integration runtime](concepts-integration-runtime.md) veri deposuna bağlanmak için kullanılacak. Azure Integration Runtime veya şirket içinde barındırılan Integration Runtime (veri depolduğunuz özel bir ağda yer alıyorsa) kullanabilirsiniz. Belirtilmezse, varsayılan Azure Integration Runtime kullanır. |Hayır |

>[!NOTE]
>"AzureStorage" türünde bağlı hizmet kullanıyorsanız, bu yeni "AzureTableStorage" bağlı hizmet türünü ileride kullanmanız önerilirken olduğu gibi hala desteklenmektedir.

**Örnek:**

```json
{
    "name": "AzureTableStorageLinkedService",
    "properties": {
        "type": "AzureTableStorage",
        "typeProperties": {
            "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

**Örnek: Azure Key Vault hesap anahtarını depolayın**

```json
{
    "name": "AzureTableStorageLinkedService",
    "properties": {
        "type": "AzureTableStorage",
        "typeProperties": {
            "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;",
            "accountKey": { 
                "type": "AzureKeyVaultSecret", 
                "store": { 
                    "referenceName": "<Azure Key Vault linked service name>", 
                    "type": "LinkedServiceReference" 
                }, 
                "secretName": "<secretName>" 
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

### <a name="use-shared-access-signature-authentication"></a>Paylaşılan erişim imzası kimlik doğrulamasını kullanma

Ayrıca, paylaşılan erişim imzası kullanarak bir depolama bağlı hizmeti oluşturabilirsiniz. Veri fabrikasını, depolama alanındaki tüm/belirli kaynaklara kısıtlı/zamana göre erişim ile sağlar.

Paylaşılan erişim imzası, depolama hesabınızdaki kaynaklara temsilci erişimi sağlar. Bu ayarı, belirli bir süre ve belirli bir izin kümesiyle Depolama hesabınızdaki nesnelere sınırlı bir izin vermek için kullanabilirsiniz. Hesap erişim anahtarlarınızı paylaşmak zorunda değilsiniz. Paylaşılan erişim imzası için depolama kaynak kimliği doğrulanmış erişim için gerekli tüm bilgileri sorgu parametrelerini kapsayan bir URI'dir. Paylaşılan erişim imzası ile depolama kaynaklarına erişmek için istemci yalnızca uygun oluşturucu veya yöntemi için paylaşılan erişim imzasını geçmesi gerekir. Paylaşılan erişim imzaları hakkında daha fazla bilgi için bkz: [paylaşılan erişim imzaları: paylaşılan erişim imzası modelini anlama](../storage/common/storage-dotnet-shared-access-signature-part-1.md).

> [!NOTE]
> Data Factory artık destekler hem de **hizmet paylaşılan erişim imzaları** ve **hesabı paylaşılan erişim imzaları**. Paylaşılan erişim imzaları hakkında daha fazla bilgi için bkz. [paylaşılan erişim imzaları (SAS) kullanarak Azure depolama kaynaklarına sınırlı erişim verme](../storage/common/storage-sas-overview.md). 

> [!TIP]
> Depolama hesabınız için hizmet paylaşılan erişim imzası oluşturmak için aşağıdaki PowerShell komutlarını yürütebilirsiniz. Yer tutucuları değiştirmeniz ve gerekli izni verin.
> `$context = New-AzStorageContext -StorageAccountName <accountName> -StorageAccountKey <accountKey>`
> `New-AzStorageContainerSASToken -Name <containerName> -Context $context -Permission rwdl -StartTime <startTime> -ExpiryTime <endTime> -FullUri`

Paylaşılan erişim imzası kimlik doğrulamasını kullanmak için aşağıdaki özellikler desteklenir.

| Özellik | Açıklama | Gereklidir |
|:--- |:--- |:--- |
| type | Type özelliği **AzureTableStorage**olarak ayarlanmalıdır. |Evet |
| sasUri | Tabloya paylaşılan erişim imzası URI 'sinin SAS URI 'sini belirtin. <br/>Bu alanı, Data Factory güvenli bir şekilde depolamak için SecureString olarak işaretleyin. Ayrıca, otomatik dönüşten yararlanmak ve belirteç bölümünü kaldırmak için Azure Key Vault SAS belirtecini de koyabilirsiniz. Daha ayrıntılı bilgi için aşağıdaki örneklere bakın ve [kimlik bilgilerini Azure Key Vault makalesine depolayın](store-credentials-in-key-vault.md) . | Evet |
| connectVia | [Integration runtime](concepts-integration-runtime.md) veri deposuna bağlanmak için kullanılacak. (Veri deponuz özel bir ağda yer alıyorsa) Azure Integration Runtime veya şirket içinde barındırılan tümleştirme çalışma zamanının kullanabilirsiniz. Belirtilmezse, varsayılan Azure Integration Runtime kullanır. |Hayır |

>[!NOTE]
>"AzureStorage" türünde bağlı hizmet kullanıyorsanız, bu yeni "AzureTableStorage" bağlı hizmet türünü ileride kullanmanız önerilirken olduğu gibi hala desteklenmektedir.

**Örnek:**

```json
{
    "name": "AzureTableStorageLinkedService",
    "properties": {
        "type": "AzureTableStorage",
        "typeProperties": {
            "sasUri": {
                "type": "SecureString",
                "value": "<SAS URI of the Azure Storage resource e.g. https://<account>.table.core.windows.net/<table>?sv=<storage version>&amp;st=<start time>&amp;se=<expire time>&amp;sr=<resource>&amp;sp=<permissions>&amp;sip=<ip range>&amp;spr=<protocol>&amp;sig=<signature>>"
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

**Örnek: Azure Key Vault hesap anahtarını depolayın**

```json
{
    "name": "AzureTableStorageLinkedService",
    "properties": {
        "type": "AzureTableStorage",
        "typeProperties": {
            "sasUri": {
                "type": "SecureString",
                "value": "<SAS URI of the Azure Storage resource without token e.g. https://<account>.table.core.windows.net/<table>>"
            },
            "sasToken": { 
                "type": "AzureKeyVaultSecret", 
                "store": { 
                    "referenceName": "<Azure Key Vault linked service name>", 
                    "type": "LinkedServiceReference" 
                }, 
                "secretName": "<secretName>" 
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

Paylaşılan erişim imzası URI'si oluşturduğunuzda, aşağıdaki noktaları göz önünde bulundurun:

- Uygun okuma/yazma izinleri (okuma, yazma, okuma/yazma) bağlı hizmet, data factory'de nasıl kullanıldığına göre nesneler üzerinde ayarlayın.
- Ayarlama **süre sonu** uygun şekilde. İşlem hattının etkin dönemini içinde depolama nesnelerine erişim süresi sona ermiyor emin olun.
- URI, ihtiyaya göre doğru tablo düzeyinde oluşturulmalıdır.

## <a name="dataset-properties"></a>Veri kümesi özellikleri

Bölümleri ve veri kümeleri tanımlamak için mevcut özelliklerin tam listesi için bkz: [veri kümeleri](concepts-datasets-linked-services.md) makalesi. Bu bölüm, Azure Tablo veri kümesi tarafından desteklenen özelliklerin bir listesini sağlar.

Verileri Azure tablosuna kopyalamak için, veri kümesinin Type özelliğini **AzureTable**olarak ayarlayın. Aşağıdaki özellikler desteklenir.

| Özellik | Açıklama | Gereklidir |
|:--- |:--- |:--- |
| type | Veri kümesinin Type özelliği **AzureTable**olarak ayarlanmalıdır. |Evet |
| tableName |Bağlı hizmetin başvurduğu tablo depolama veritabanı örneğindeki tablonun adı. |Evet |

**Örnek:**

```json
{
    "name": "AzureTableDataset",
    "properties":
    {
        "type": "AzureTable",
        "typeProperties": {
            "tableName": "MyTable"
        },
        "schema": [],
        "linkedServiceName": {
            "referenceName": "<Azure Table storage linked service name>",
            "type": "LinkedServiceReference"
        }
    }
}
```

### <a name="schema-by-data-factory"></a>Veri fabrikası tarafından şeması

Azure tablosu gibi şema içermeyen veri depoları için, Data Factory şemayı aşağıdaki yollarla bir şekilde algılar:

* Kopyalama etkinliğinde sütun eşlemeyi belirtirseniz, verileri almak için kaynak tarafı sütun listesini kullanın Data Factory. Bu durumda, bir satır bir sütun için değer içermiyorsa, için null değer sağlanır.
* Kopyalama etkinliğinde sütun eşlemeyi belirtmezseniz, verileri verilerdeki ilk satırı kullanarak şemayı Data Factory. Bu durumda, ilk satır tam şemayı içermiyorsa (ör. bazı sütunlarda null değer varsa), kopyalama işleminin sonucunda bazı sütunlar kaçırılacaktır.

## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri

Bölümleri ve etkinlikleri tanımlamak için mevcut özelliklerin tam listesi için bkz: [işlem hatları](concepts-pipelines-activities.md) makalesi. Bu bölüm, Azure tablo kaynağı ve havuzu tarafından desteklenen özelliklerin bir listesini sağlar.

### <a name="azure-table-as-a-source-type"></a>Kaynak türü olarak Azure tablosu

Azure tablosundan veri kopyalamak için kopyalama etkinliğindeki kaynak türünü **AzureTableSource**olarak ayarlayın. Kopyalama etkinliği aşağıdaki özellikler desteklenir **kaynak** bölümü.

| Özellik | Açıklama | Gereklidir |
|:--- |:--- |:--- |
| type | Kopyalama etkinliği kaynağının Type özelliği **AzureTableSource**olarak ayarlanmalıdır. |Evet |
| azureTableSourceQuery |Verileri okumak için özel tablo depolama sorgusunu kullanın. Aşağıdaki bölümdeki örneklere bakın. |Hayır |
| azureTableSourceIgnoreTableNotFound |Tablonun özel durumunun mevcut olup olmayacağını gösterir.<br/>İzin verilen değerler **True** ve **False** (varsayılan). |Hayır |

### <a name="azuretablesourcequery-examples"></a>azureTableSourceQuery örnekleri

Azure Tablo sütunu tarih saat türünde ise:

```json
"azureTableSourceQuery": "LastModifiedTime gt datetime'2017-10-01T00:00:00' and LastModifiedTime le datetime'2017-10-02T00:00:00'"
```

Azure Tablo sütunu dize türünde ise:

```json
"azureTableSourceQuery": "LastModifiedTime ge '201710010000_0000' and LastModifiedTime le '201710010000_9999'"
```

İşlem hattı parametresini kullanırsanız, DateTime değerini önceki örneklere göre doğru biçime atayın.

### <a name="azure-table-as-a-sink-type"></a>Havuz türü olarak Azure tablosu

Verileri Azure tablosuna kopyalamak için kopyalama etkinliğindeki havuz türünü **AzureTableSink**olarak ayarlayın. Aşağıdaki özellikler, etkinlik **havuzunu** Kopyala bölümünde desteklenir.

| Özellik | Açıklama | Gereklidir |
|:--- |:--- |:--- |
| type | Kopyalama etkinliği havuzunun Type özelliği **AzureTableSink**olarak ayarlanmalıdır. |Evet |
| azureTableDefaultPartitionKeyValue |Havuz tarafından kullanılabilen varsayılan bölüm anahtarı değeri. |Hayır |
| azureTablePartitionKeyName |Değerleri bölüm anahtarları olarak kullanılan sütunun adını belirtin. Belirtilmemişse, bölüm anahtarı olarak "AzureTableDefaultPartitionKeyValue" kullanılır. |Hayır |
| azureTableRowKeyName |Sütun değerleri satır anahtarı olarak kullanılan sütunun adını belirtin. Belirtilmezse, her satır için bir GUID kullanın. |Hayır |
| azureTableInsertType |Azure tablosuna veri ekleme modu. Bu özellik, çıkış tablosunda eşleşen bölüm ve satır anahtarlarının değerlerinin değiştirilmesini veya birleştirildiğini denetler. <br/><br/>İzin verilen değerler **merge** (varsayılan) ve **Replace**. <br/><br> Bu ayar, tablo düzeyinde değil, satır düzeyinde geçerlidir. Hiçbir seçenek, girişte bulunmayan çıkış tablosundaki satırları siler. Birleştirme ve değiştirme ayarlarının nasıl çalıştığı hakkında bilgi edinmek için bkz. [varlık ekleme veya birleştirme](https://msdn.microsoft.com/library/azure/hh452241.aspx) ve [varlık ekleme veya değiştirme](https://msdn.microsoft.com/library/azure/hh452242.aspx). |Hayır |
| writeBatchSize |WriteBatchSize veya writeBatchTimeout değeri isabet edildiğinde Azure tablosuna veri ekler.<br/>İzin verilen değerler Integer (satır sayısı). |Hayır (varsayılan değer 10.000) |
| writeBatchTimeout |WriteBatchSize veya writeBatchTimeout değeri isabet edildiğinde Azure tablosuna veri ekler.<br/>İzin verilen değerler TimeSpan. Örnek olarak "00:20:00" (20 dakika) bulunur. |Hayır (varsayılan değer 90 saniyedir, depolama istemcisinin varsayılan zaman aşımı) |

**Örnek:**

```json
"activities":[
    {
        "name": "CopyToAzureTable",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<Azure Table output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "<source type>"
            },
            "sink": {
                "type": "AzureTableSink",
                "azureTablePartitionKeyName": "<column name>",
                "azureTableRowKeyName": "<column name>"
            }
        }
    }
]
```

### <a name="azuretablepartitionkeyname"></a>azureTablePartitionKeyName

Hedef sütunu azureTablePartitionKeyName olarak kullanabilmeniz için önce **"Translator"** özelliğini kullanarak bir kaynak sütunu hedef sütuna eşleyin.

Aşağıdaki örnekte, DivisionID kaynak sütunu DivisionID hedef sütunuyla eşlenir:

```json
"translator": {
    "type": "TabularTranslator",
    "columnMappings": "DivisionID: DivisionID, FirstName: FirstName, LastName: LastName"
}
```

"DivisionID", bölüm anahtarı olarak belirtilir.

```json
"sink": {
    "type": "AzureTableSink",
    "azureTablePartitionKeyName": "DivisionID"
}
```

## <a name="data-type-mapping-for-azure-table"></a>Azure tablosu için veri türü eşlemesi

Ve Azure tablosundan verileri kopyaladığınızda, Azure Tablo veri türleri arasında, geçici veri türlerini Data Factory için aşağıdaki eşlemeler kullanılır. Kopyalama etkinliğinin kaynak şemayı ve veri türünü havuza nasıl eşlediğini öğrenmek için bkz. [şema ve veri türü eşlemeleri](copy-activity-schema-and-type-mapping.md).

Verileri Azure tablosuna taşıdığınızda, Azure tablosu [tarafından tanımlanan aşağıdaki eşlemeler](https://msdn.microsoft.com/library/azure/dd179338.aspx) Azure Tablo OData türlerinden .net türüne ve tam tersi şekilde kullanılır.

| Azure Tablo veri türü | Veri Fabrikası geçici veri türü | Ayrıntılar |
|:--- |:--- |:--- |
| EDM.Binary |byte[] |64 KB 'a kadar olan bir bayt dizisi. |
| Edm.Boolean |bool |Boole değeri. |
| Edm.DateTime |Tarih Saat |Eşgüdümlü Evrensel Saat (UTC) olarak ifade edilen 64 bitlik bir değer. Desteklenen tarih saat aralığı gece yarısı başlar, 1 Ocak 1601 M.S. (C.E.), UTC. Aralık 31 Aralık 9999 ' de sona erer. |
| Edm.Double |double |64 bitlik kayan nokta değeri. |
| EDM.Guid |Guid |128 bitlik bir genel benzersiz tanımlayıcı. |
| Edm.Int32 |Int32 |32 bitlik bir tamsayı. |
| Edm.Int64 |Int64 |64 bitlik bir tamsayı. |
| Edm.String |Dize |UTF-16 kodlu bir değer. Dize değerleri 64 KB 'a kadar olabilir. |

## <a name="lookup-activity-properties"></a>Arama etkinliği özellikleri

Özelliklerle ilgili ayrıntıları öğrenmek için [arama etkinliğini](control-flow-lookup-activity.md)denetleyin.

## <a name="next-steps"></a>Sonraki adımlar
Veri fabrikasında kopyalama etkinliği tarafından kaynak ve havuz olarak desteklenen veri depolarının listesi için bkz. [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats).
