---
title: Azure Tablo depolama alanına ve bu depolama dan veri kopyalama
description: Desteklenen kaynak mağazalardan Azure Tablo depolamasına veya Tablo depolamadan desteklenen lavabo depolarına veri fabrikasını kullanarak nasıl kopyalayış edilebildiğini öğrenin.
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
ms.openlocfilehash: 6edd32f8f3579238d1f08f55ce9fb1528fa5d211
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81417489"
---
# <a name="copy-data-to-and-from-azure-table-storage-by-using-azure-data-factory"></a>Azure Veri Fabrikası'nı kullanarak Verileri Azure Table depolamasına ve azure tablo depolamasına kopyalama

> [!div class="op_single_selector" title1="Kullandığınız Veri Fabrikası hizmetisürümünü seçin:"]
> * [Sürüm 1](v1/data-factory-azure-table-connector.md)
> * [Geçerli sürüm](connector-azure-table-storage.md)

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Bu makalede, Azure Tablo depolama alanına ve Azure Tablo depolamadan veri kopyalamak için Azure Veri Fabrikası'nda Kopyalama Etkinliği'nin nasıl kullanılacağı açıklanmaktadır. Kopyalama Etkinliği'ne genel bir genel bakış sunan [Kopyalama Etkinliği genel bakış](copy-activity-overview.md) makalesine dayanmaktadır.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="supported-capabilities"></a>Desteklenen yetenekler

Bu Azure Tablo depolama konektörü aşağıdaki etkinlikler için desteklenir:

- [Desteklenen kaynak/lavabo matrisi](copy-activity-overview.md) ile [etkinliği](copy-activity-overview.md) kopyalama
- [Arama etkinliği](control-flow-lookup-activity.md)

Desteklenen herhangi bir kaynak veri deposundan Tablo depolamasına verileri kopyalayabilirsiniz. Ayrıca, Tablo depolamadan desteklenen herhangi bir lavabo veri deposuna verileri kopyalayabilirsiniz. Kopyalama etkinliği tarafından kaynak veya lavabo olarak desteklenen veri depolarının listesi için [Desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats) tablosuna bakın.

Özellikle, bu Azure Tablosu bağlayıcısı hesap anahtarı nı ve hizmet paylaşılan erişim imzası kimlik doğrularını kullanarak verilerin kopyalanmasına destek verir.

## <a name="get-started"></a>başlarken

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Aşağıdaki bölümler, Tablo depolamasına özgü Veri Fabrikası varlıklarını tanımlamak için kullanılan özellikler hakkında ayrıntılı bilgi sağlar.

## <a name="linked-service-properties"></a>Bağlantılı hizmet özellikleri

### <a name="use-an-account-key"></a>Hesap anahtarı kullanma

Hesap anahtarını kullanarak Azure Depolama bağlantılı bir hizmet oluşturabilirsiniz. Veri fabrikasına Depolama'ya küresel erişim sağlar. Aşağıdaki özellikler desteklenir.

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Tür özelliği **AzureTableStorage**olarak ayarlanmalıdır. |Evet |
| Connectionstring | ConnectionString özelliği için Depolama'ya bağlanmak için gereken bilgileri belirtin. <br/>Hesap anahtarını Azure Key Vault'a `accountKey` koyabilir ve yapılandırmayı bağlantı dizesinin dışına çekebilirsiniz. Azure Key Vault [makalesinde](store-credentials-in-key-vault.md) daha fazla ayrıntı içeren aşağıdaki örneklere ve Mağaza kimlik bilgilerine bakın. |Evet |
| connectVia | Veri deposuna bağlanmak için kullanılacak [tümleştirme çalışma süresi.](concepts-integration-runtime.md) Azure Tümleştirme Çalışma Süresi'ni veya Kendi kendine barındırılan Tümleştirme Çalışma Süresini (veri deponuz özel bir ağda bulunuyorsa) kullanabilirsiniz. Belirtilmemişse, varsayılan Azure Tümleştirme Çalışma Süresini kullanır. |Hayır |

>[!NOTE]
>"AzureStorage" türüne bağlı hizmeti kullanıyorsanız, bu yeni "AzureTableStorage" bağlantılı hizmet türünü ileriye dönük olarak kullanmanız önerilirken, bu hizmet yine de olduğu gibi desteklenir.

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

**Örnek: Azure Key Vault'ta mağaza hesabı anahtarı**

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

Ayrıca paylaşılan bir erişim imzası kullanarak Depolama bağlantılı bir hizmet oluşturabilirsiniz. Veri fabrikasına depolamadaki tüm/belirli kaynaklara sınırlı/zamana bağlı erişim sağlar.

Paylaşılan erişim imzası, depolama hesabınızdaki kaynaklara temsilci erişimi sağlar. Bunu, bir istemciye depolama hesabınızdaki nesnelere belirli bir süre ve belirli bir izin kümesiyle sınırlı izinler vermek için kullanabilirsiniz. Hesap erişim anahtarlarınızı paylaşmak zorunda değilsiniz. Paylaşılan erişim imzası, sorgu parametrelerinde bir depolama kaynağına kimlik doğrulaması için gerekli tüm bilgileri kapsayan bir URI'dir. Paylaşılan erişim imzasıyla depolama kaynaklarına erişmek için istemcinin yalnızca paylaşılan erişim imzasını uygun oluşturucuya veya yönteme geçirmesi gerekir. Paylaşılan erişim imzaları hakkında daha fazla bilgi için bkz: [Paylaşılan erişim imzaları: Paylaşılan erişim imza modelini anlayın.](../storage/common/storage-dotnet-shared-access-signature-part-1.md)

> [!NOTE]
> Veri Fabrikası artık hem **hizmet paylaşılan erişim imzalarını** hem de **hesap paylaşılan erişim imzalarını**destekler. Paylaşılan erişim imzaları hakkında daha fazla bilgi için bkz: [Paylaşılan erişim imzalarını (SAS) kullanarak Azure Depolama kaynaklarına sınırlı erişim izni](../storage/common/storage-sas-overview.md)ver. 

> [!TIP]
> Depolama hesabınız için paylaşılan bir hizmet erişim imzası oluşturmak için aşağıdaki PowerShell komutlarını gerçekleştirebilirsiniz. Yer tutucuları değiştirin ve gerekli izni ver.
> `$context = New-AzStorageContext -StorageAccountName <accountName> -StorageAccountKey <accountKey>`
> `New-AzStorageContainerSASToken -Name <containerName> -Context $context -Permission rwdl -StartTime <startTime> -ExpiryTime <endTime> -FullUri`

Paylaşılan erişim imzası kimlik doğrulamasını kullanmak için aşağıdaki özellikler desteklenir.

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Tür özelliği **AzureTableStorage**olarak ayarlanmalıdır. |Evet |
| sasUri | Tabloya paylaşılan erişim imzası URI'nin SAS URI'sini belirtin. <br/>Bu alanı Güvenli Bir Şekilde Veri Fabrikası'nda depolamak için SecureString olarak işaretleyin. Otomatik döndürmeden yararlanmak ve belirteç kısmını kaldırmak için Azure Key Vault'a SAS belirteci de koyabilirsiniz. Azure Key Vault [makalesinde](store-credentials-in-key-vault.md) daha fazla ayrıntı içeren aşağıdaki örneklere ve Mağaza kimlik bilgilerine bakın. | Evet |
| connectVia | Veri deposuna bağlanmak için kullanılacak [tümleştirme çalışma süresi.](concepts-integration-runtime.md) Azure Tümleştirme Çalışma Zamanı'nı veya Kendi kendine barındırılan Tümleştirme Çalışma Süresini (veri deponuz özel bir ağda bulunuyorsa) kullanabilirsiniz. Belirtilmemişse, varsayılan Azure Tümleştirme Çalışma Süresini kullanır. |Hayır |

>[!NOTE]
>"AzureStorage" türüne bağlı hizmeti kullanıyorsanız, bu yeni "AzureTableStorage" bağlantılı hizmet türünü ileriye dönük olarak kullanmanız önerilirken, bu hizmet yine de olduğu gibi desteklenir.

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

**Örnek: Azure Key Vault'ta mağaza hesabı anahtarı**

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

Paylaşılan bir erişim imzası URI oluşturduğunuzda, aşağıdaki noktaları göz önünde bulundurun:

- Veri fabrikanızda bağlantılı hizmetin (okuma, yazma, okuma/yazma) nasıl kullanıldığına bağlı olarak nesneler üzerinde uygun okuma/yazma izinleri ayarlayın.
- **Son kullanma süresini** uygun şekilde ayarlayın. Depolama nesnelerine erişimin ardışık ardışık etkin süre içinde sona ermediğinden emin olun.
- URI ihtiyaca göre doğru tablo düzeyinde oluşturulmalıdır.

## <a name="dataset-properties"></a>Veri kümesi özellikleri

Veri kümelerini tanımlamak için kullanılabilen bölümlerin ve özelliklerin tam listesi için [Datasets](concepts-datasets-linked-services.md) makalesine bakın. Bu bölümde, Azure Tablosu veri kümesi tarafından desteklenen özelliklerin bir listesi yer almaktadır.

Azure Table'a veri kopyalamak için, veri kümesinin tür özelliğini **AzureTable**olarak ayarlayın. Aşağıdaki özellikler desteklenir.

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Veri kümesinin tür özelliği **AzureTable**olarak ayarlanmalıdır. |Evet |
| tableName |Bağlı hizmetin başvurulması yla tablonun adı Tablo depolama veritabanı örneğinde yer almaktadır. |Evet |

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

### <a name="schema-by-data-factory"></a>Veri Fabrikası'na Göre Şema

Azure Tablo gibi şema içermeyen veri depoları için, Veri Fabrikası şema'yı aşağıdaki yollardan biriyle çıkartır:

* Kopyalama etkinliğinde sütun eşleciliğini belirtirseniz, Veri Fabrikası verileri almak için kaynak yan sütun listesini kullanır. Bu durumda, bir satır bir sütun için bir değer içermiyorsa, bunun için null bir değer sağlanır.
* Kopyalama etkinliğinde sütun eşlemesini belirtmezseniz, Veri Fabrikası verilerdeki ilk satırı kullanarak şemayı çıkartır. Bu durumda, ilk satır tam şema içermiyorsa (örneğin, bazı sütunların null değeri vardır), kopyalama işlemi sonucunda bazı sütunlar gözden kaçırılır.

## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri

Etkinlikleri tanımlamak için kullanılabilen bölümlerin ve özelliklerin tam listesi [için, Pipelines](concepts-pipelines-activities.md) makalesine bakın. Bu bölümde, Azure Tablo kaynağı ve lavabo tarafından desteklenen özelliklerin bir listesi bulunmaktadır.

### <a name="azure-table-as-a-source-type"></a>Kaynak türü olarak Azure Tablosu

Azure Tablosu'ndaki verileri kopyalamak için, kopyalama etkinliğindeki kaynak türünü **AzureTableSource**olarak ayarlayın. Aşağıdaki özellikler kopyalama etkinliği **kaynak** bölümünde desteklenir.

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Kopyalama etkinlik kaynağının tür özelliği **AzureTableSource**olarak ayarlanmalıdır. |Evet |
| azureTableSourceQuery |Verileri okumak için özel Tablo depolama sorgusunu kullanın. Aşağıdaki bölümdeki örneklere bakın. |Hayır |
| azureTableSourceIgnoreTableNotFound |Tablonun özel durum var olmasına izin verilip vermeyeceğini gösterir.<br/>İzin verilen değerler **True** ve **False** (varsayılan) olarak verilir. |Hayır |

### <a name="azuretablesourcequery-examples"></a>azureTableSourceQuery örnekleri

>[!NOTE]
>Azure Tablo sorgusu işlemi, Azure [Tablo hizmeti tarafından zorlanan](https://docs.microsoft.com/rest/api/storageservices/setting-timeouts-for-table-service-operations)30 saniye içinde zamanlanır. Makaleyi [sorgulamak için Tasarım'dan](../storage/tables/table-storage-design-for-query.md) sorguyu nasıl optimize edebilirsiniz öğrenin.

Azure Veri Fabrikası'nda, verileri bir datetime türü sütununa filtrelemek istiyorsanız, aşağıdaki örneğe bakın:

```json
"azureTableSourceQuery": "LastModifiedTime gt datetime'2017-10-01T00:00:00' and LastModifiedTime le datetime'2017-10-02T00:00:00'"
```

Verileri dize türü sütununa filtrelemek istiyorsanız, aşağıdaki örneğe bakın:

```json
"azureTableSourceQuery": "LastModifiedTime ge '201710010000_0000' and LastModifiedTime le '201710010000_9999'"
```

Ardışık etki hattı parametresini kullanıyorsanız, tarih saati değerini önceki örneklere göre uygun biçime atın.

### <a name="azure-table-as-a-sink-type"></a>Lavabo türü olarak Azure Tablosu

Verileri Azure Tablosu'na kopyalamak için, kopyalama etkinliğindeki lavabo türünü **AzureTableSink**olarak ayarlayın. Aşağıdaki özellikler kopyalama etkinliği **lavabo** bölümünde desteklenir.

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Kopyalama etkinliği lavabonun türü özelliği **AzureTableSink**olarak ayarlanmalıdır. |Evet |
| azureTableDefaultPartitionKeyValue |Lavabo tarafından kullanılabilecek varsayılan bölüm anahtar değeri. |Hayır |
| azureTablePartitionKeyName |Değerleri bölüm anahtarları olarak kullanılan sütunun adını belirtin. Belirtilmemişse, "AzureTableDefaultPartitionKeyValue" bölüm anahtarı olarak kullanılır. |Hayır |
| azureTableTableKeyName |Sütun değerleri satır anahtarı olarak kullanılan sütunun adını belirtin. Belirtilmemişse, her satır için bir GUID kullanın. |Hayır |
| azureTableInsertType |Azure Tablosu'na veri ekleme modu. Bu özellik, eşleşen bölüm ve satır anahtarları ile çıktı tablosunda varolan satırların değerlerinin değiştirilip değiştirilmediğini veya birleştirilip birleştirilmediğini denetler. <br/><br/>İzin verilen değerler **birleştirilir** (varsayılan) ve **değiştirilir.** <br/><br> Bu ayar satır düzeyinde değil, tablo düzeyinde geçerlidir. Her iki seçenek de girişte bulunmayan çıktı tablosundaki satırları siler. Birleştirme ve değiştirme ayarlarının nasıl çalıştığı hakkında bilgi edinmek için, [varlığı Ekle veya birleştir](https://msdn.microsoft.com/library/azure/hh452241.aspx) ve varlığı girin veya [değiştirin'e](https://msdn.microsoft.com/library/azure/hh452242.aspx)bakın. |Hayır |
| yazmaBatchSize |Toplu Boyut veya writeBatchTimeout vurulduğunda Azure Tablosu'na veri ekler.<br/>İzin verilen değerler insa (satır sayısı) olan tümsecidir. |Hayır (varsayılan değer 10.000'dir) |
| yazmaBatchTimeout |Toplu Boyut veya writeBatchTimeout vurulduğunda Azure Tablosu'na veri ekler.<br/>İzin verilen değerler zaman ası dır. Bir örnek "00:20:00" (20 dakika). |Hayır (varsayılan değer 90 saniyedir, depolama istemcisi varsayılan zaman amı) |

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

Hedef sütunu azureTablePartitionKeyName olarak kullanmadan önce **"çevirmen"** özelliğini kullanarak bir hedef sütunu hedef sütunla eşle.

Aşağıdaki örnekte, kaynak sütun DivisionID hedef sütun DivisionID eşlenir:

```json
"translator": {
    "type": "TabularTranslator",
    "columnMappings": "DivisionID: DivisionID, FirstName: FirstName, LastName: LastName"
}
```

"DivisionID" bölüm anahtarı olarak belirtilir.

```json
"sink": {
    "type": "AzureTableSink",
    "azureTablePartitionKeyName": "DivisionID"
}
```

## <a name="data-type-mapping-for-azure-table"></a>Azure Tablosu için veri türü eşleme

Verileri Azure Tablosu'ndan ve Azure Tablosu'na kopyaladiğinizde, Azure Tablo veri türlerinden Veri Fabrikası geçici veri türlerine aşağıdaki eşlemeler kullanılır. Kopyalama etkinliğinin kaynak şemasını ve veri türünü lavaboyla nasıl eşlenebildiğini öğrenmek için Bkz. [Şema ve veri türü eşlemeleri.](copy-activity-schema-and-type-mapping.md)

Verileri Azure Tablosu'na taşıdığınızda, Azure Tablosu tarafından tanımlanan aşağıdaki [eşlemeler](https://msdn.microsoft.com/library/azure/dd179338.aspx) Azure Tablo OData türlerinden .NET türüne veya tam tersi olarak kullanılır.

| Azure Tablo veri türü | Veri Fabrikası geçici veri türü | Ayrıntılar |
|:--- |:--- |:--- |
| Edm.İkili |bayt[] |64 KB'ye kadar bayt dizisi. |
| Edm.Boolean |bool |Boole değeri. |
| Edm.DateTime |DateTime |Eşgüdümlü Evrensel Zaman (UTC) olarak ifade edilen 64 bitlik bir değer. Desteklenen DateTime aralığı gece yarısı başlar, Ocak 1, 1601 Ad. (C.E.), UTC. Aralık 31 Aralık 9999 biter. |
| Edm.Double |double |64 bit kayan nokta değeri. |
| Edm.Guid |Guid |128 bit küresel olarak benzersiz tanımlayıcı. |
| Edm.Int32 |Int32 |32 bit'lik bir tamsayı. |
| Edm.Int64 |Int64 |64 bit'lik bir tamsayı. |
| Edm.String |Dize |UTF-16 kodlanmış bir değer. Dize değerleri 64 KB'ye kadar olabilir. |

## <a name="lookup-activity-properties"></a>Arama etkinlik özellikleri

Özellikler hakkında daha fazla bilgi edinmek için [Arama etkinliğini](control-flow-lookup-activity.md)kontrol edin.

## <a name="next-steps"></a>Sonraki adımlar
Veri Fabrikası'ndaki kopyalama etkinliği tarafından kaynak ve lavabo olarak desteklenen veri depolarının listesi [için](copy-activity-overview.md#supported-data-stores-and-formats)bkz.
