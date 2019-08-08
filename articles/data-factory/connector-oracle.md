---
title: Azure Data Factory kullanarak Oracle 'a ve Oracle 'a veri kopyalama | Microsoft Docs
description: Data Factory kullanarak, desteklenen kaynak depolardaki verileri bir Oracle veritabanına veya Oracle 'dan desteklenen havuz depolarına kopyalamayı öğrenin.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 06/25/2019
ms.author: jingwang
ms.openlocfilehash: 0a71c7ffe9040c3002b1f5378ce298a047554b15
ms.sourcegitcommit: 3877b77e7daae26a5b367a5097b19934eb136350
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/30/2019
ms.locfileid: "68640196"
---
# <a name="copy-data-from-and-to-oracle-by-using-azure-data-factory"></a>Azure Data Factory kullanarak verileri ve Oracle 'a kopyalama
> [!div class="op_single_selector" title1="Kullandığınız Data Factory hizmeti sürümünü seçin:"]
> * [Sürüm 1](v1/data-factory-onprem-oracle-connector.md)
> * [Geçerli sürüm](connector-oracle.md)

Bu makalede, verileri bir Oracle veritabanından ve ' den kopyalamak için Azure Data Factory kopyalama etkinliğinin nasıl kullanılacağı özetlenmektedir. [Kopyalama etkinliğine genel bakış](copy-activity-overview.md)sayfasında oluşturulur.

## <a name="supported-capabilities"></a>Desteklenen özellikler

Bir Oracle veritabanından desteklenen herhangi bir havuz veri deposuna veri kopyalayabilirsiniz. Ayrıca, desteklenen herhangi bir kaynak veri deposundan verileri bir Oracle veritabanına kopyalayabilirsiniz. Kopyalama etkinliği tarafından kaynak ve havuz desteklenen veri depolarının listesi için bkz. [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats) tablo.

Özellikle, bu Oracle Bağlayıcısı şunları destekler:

- Bir Oracle veritabanının aşağıdaki sürümleri:
  - Oracle 12c R1 (12,1)
  - Oracle 11g R1, R2 (11,1, 11,2)
  - Oracle 10G R1, R2 (10,1, 10,2)
  - Oracle 9i R1, R2 (9.0.1, 9.2)
  - Oracle 8i R3 (8.1.7)
- Temel veya OID kimlik doğrulamaları kullanarak verileri kopyalama.
- Oracle kaynağından paralel kopyalama. Ayrıntılar için [Oracle 'Dan paralel kopyalama](#parallel-copy-from-oracle) bölümüne bakın.

> [!Note]
> Oracle proxy sunucusu desteklenmez.

## <a name="prerequisites"></a>Önkoşullar

Ve herkese açık olmayan bir Oracle veritabanından veri kopyalamak için, [Şirket içinde barındırılan bir tümleştirme çalışma zamanı](create-self-hosted-integration-runtime.md)ayarlamanız gerekir. Integration Runtime, yerleşik bir Oracle sürücüsü sağlar. Bu nedenle, ve Oracle 'dan veri kopyaladığınızda sürücüyü el ile yüklemeniz gerekmez.

## <a name="get-started"></a>başlarken

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Aşağıdaki bölümlerde, Oracle bağlayıcısına özgü Data Factory varlıkları tanımlamak için kullanılan özellikler hakkında ayrıntılar sağlanmaktadır.

## <a name="linked-service-properties"></a>Bağlı hizmeti özellikleri

Oracle bağlı hizmeti aşağıdaki özellikleri destekler:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Type özelliği **Oracle**olarak ayarlanmalıdır. | Evet |
| connectionString | Oracle Database örneğine bağlanmak için gereken bilgileri belirtir. <br/>Data Factory güvenli bir şekilde depolamak `SecureString` için bu alanı bir olarak işaretleyin. Ayrıca Azure Key Vault bir parola yerleştirebilir ve `password` yapılandırmayı bağlantı dizesinin dışına çekebilirsiniz. Daha ayrıntılı bilgi için aşağıdaki örneklere bakın ve [kimlik bilgilerini Azure Key Vault depolayın](store-credentials-in-key-vault.md) . <br><br>**Desteklenen bağlantı türü**: Veritabanınızı tanımlamak için **Oracle SID** veya **Oracle hizmet adını** kullanabilirsiniz:<br>-SID kullanıyorsanız:`Host=<host>;Port=<port>;Sid=<sid>;User Id=<username>;Password=<password>;`<br>-Hizmet adı kullanıyorsanız:`Host=<host>;Port=<port>;ServiceName=<servicename>;User Id=<username>;Password=<password>;` | Evet |
| connectVia | [Integration runtime](concepts-integration-runtime.md) veri deposuna bağlanmak için kullanılacak. Şirket içinde barındırılan tümleştirme çalışma zamanını veya Azure tümleştirme çalışma zamanını (veri depoluiz herkese açık ise) kullanabilirsiniz. Belirtilmemişse, bu özellik varsayılan Azure tümleştirme çalışma zamanını kullanır. |Hayır |

>[!TIP]
>Bir hata alırsanız, "ORA-01025: UPI parametresi aralık dışında "ve Oracle sürümünüz 8i 'dir, Bağlantı dizenizi ekleyin `WireProtocolMode=1` . Sonra yeniden deneyin.

Oracle bağlantısı üzerinde şifrelemeyi etkinleştirmek için iki seçeneğiniz vardır:

-   **Üçlü DES şifreleme (3DES) ve Gelişmiş Şifreleme Standardı (AES)** kullanmak için Oracle sunucu tarafında, Oracle gelişmiş güvenlik (OAS) bölümüne gidin ve şifreleme ayarlarını yapılandırın. Ayrıntılar için bkz. [Oracle belgeleri](https://docs.oracle.com/cd/E11882_01/network.112/e40393/asointro.htm#i1008759). Oracle uygulama geliştirme çerçevesi (ADF) Bağlayıcısı, bir Oracle bağlantısı kurarken OAS 'de yapılandırdığınız birini kullanmak üzere şifreleme yöntemini otomatik olarak görüşür.

-   **SSL**kullanmak için:

    1.  SSL sertifika bilgilerini alın. SSL sertifikalarınızın Distinguished Encoding Rules (DER) ile kodlanmış sertifika bilgilerini alın ve çıktıyı kaydedin (-----sertifikayı Başlat... -----) Bir metin dosyası olarak son sertifika.

        ```
        openssl x509 -inform DER -in [Full Path to the DER Certificate including the name of the DER Certificate] -text
        ```

        **Örnek:** DERcert. cer dosyasından sertifika bilgilerini ayıklayın ve çıktıyı CERT. txt dosyasına kaydedin.

        ```
        openssl x509 -inform DER -in DERcert.cer -text
        Output:
        -----BEGIN CERTIFICATE-----
        XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
        XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
        XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
        XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
        XXXXXXXXX
        -----END CERTIFICATE-----
        ```
    
    2.  `keystore` Veya`truststore`oluşturun. Aşağıdaki komut, PKCS- `truststore` 12 biçiminde bir parola ile veya parolasız dosya oluşturur.

        ```
        openssl pkcs12 -in [Path to the file created in the previous step] -out [Path and name of TrustStore] -passout pass:[Keystore PWD] -nokeys -export
        ```

        **Örnek:** Parola ile mytruststorefile adlı bir PKCS12 `truststore` dosyası oluşturun.

        ```
        openssl pkcs12 -in cert.txt -out MyTrustStoreFile -passout pass:ThePWD -nokeys -export  
        ```

    3.  `truststore` Dosyayı kendi kendine barındırılan IR makinesine yerleştirin. Örneğin, dosyayı C:\mytruststorefilekonumundaki yere yerleştirin.
    4.  Azure Data Factory ' `EncryptionMethod=1` de Oracle bağlantı dizesini ve karşılık gelen `TrustStore` / `TrustStorePassword`değeri yapılandırın. Örneğin: `Host=<host>;Port=<port>;Sid=<sid>;User Id=<username>;Password=<password>;EncryptionMethod=1;TrustStore=C:\\MyTrustStoreFile;TrustStorePassword=<trust_store_password>`.

**Örnek:**

```json
{
    "name": "OracleLinkedService",
    "properties": {
        "type": "Oracle",
        "typeProperties": {
            "connectionString": {
                "type": "SecureString",
                "value": "Host=<host>;Port=<port>;Sid=<sid>;User Id=<username>;Password=<password>;"
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

**Örnek: Azure Key Vault parola depola**

```json
{
    "name": "OracleLinkedService",
    "properties": {
        "type": "Oracle",
        "typeProperties": {
            "connectionString": {
                "type": "SecureString",
                "value": "Host=<host>;Port=<port>;Sid=<sid>;User Id=<username>;"
            },
            "password": { 
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
## <a name="dataset-properties"></a>Veri kümesi özellikleri

Bu bölüm, Oracle veri kümesi tarafından desteklenen özelliklerin bir listesini sağlar. Veri kümelerini tanımlamaya yönelik bölümlerin ve özelliklerin tam listesi için bkz. [veri kümeleri](concepts-datasets-linked-services.md). 

Ve Oracle 'dan verileri kopyalamak için, veri kümesinin Type özelliğini olarak `OracleTable`ayarlayın. Aşağıdaki özellikler desteklenir.

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Veri kümesinin Type özelliği olarak `OracleTable`ayarlanmalıdır. | Evet |
| tableName |Oracle veritabanındaki, bağlı hizmetin başvurduğu tablonun adı. | Evet |

**Örnek:**

```json
{
    "name": "OracleDataset",
    "properties":
    {
        "type": "OracleTable",
        "linkedServiceName": {
            "referenceName": "<Oracle linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "tableName": "MyTable"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri

Bu bölüm, Oracle kaynağı ve havuzu tarafından desteklenen özelliklerin bir listesini sağlar. Etkinlikleri tanımlamaya yönelik bölümlerin ve özelliklerin tam listesi için bkz. işlem [hatları](concepts-pipelines-activities.md). 

### <a name="oracle-as-a-source-type"></a>Kaynak türü olarak Oracle

> [!TIP]
>
> Verileri bölümlemeyi kullanarak Oracle 'dan verimli bir şekilde yüklemek için bkz. [Oracle 'Dan paralel kopyalama](#parallel-copy-from-oracle).

Oracle 'dan veri kopyalamak için kopyalama etkinliğindeki kaynak türünü olarak `OracleSource`ayarlayın. Kopyalama etkinliği aşağıdaki özellikler desteklenir **kaynak** bölümü.

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Kopyalama etkinliği kaynağının Type özelliği olarak `OracleSource`ayarlanmalıdır. | Evet |
| oracleReaderQuery | Verileri okumak için özel bir SQL sorgusu kullanın. `"SELECT * FROM MyTable"` bunun bir örneğidir.<br>Bölümlenmiş yükü etkinleştirdiğinizde, sorgunuza karşılık gelen yerleşik bölüm parametrelerini de eklemeniz gerekir. Örnekler için [Oracle 'Dan paralel kopyalama](#parallel-copy-from-oracle) bölümüne bakın. | Hayır |
| partitionOptions | Oracle 'dan veri yüklemek için kullanılan veri bölümleme seçeneklerini belirtir. <br>İzin verilen değerler şunlardır: **Hiçbiri** (varsayılan), **Physicalpartitionsoftable** ve **DynamicRange**.<br>Bir bölüm seçeneği etkin olduğunda (yani, değil `None`) kopyalama etkinliğindeki [`parallelCopies`](copy-activity-performance.md#parallel-copy) ayarı da yapılandırın. Bu, bir Oracle veritabanından eşzamanlı olarak veri yükleme paralel derecesini belirler. Örneğin, bunu 4 olarak ayarlayabilirsiniz. | Hayır |
| partitionSettings | Veri bölümleme için ayarların grubunu belirtin. <br>Bölüm seçeneği `None`olmadığında uygulayın. | Hayır |
| partitionNames | Kopyalanması gereken fiziksel bölümlerin listesi. <br>Bölüm seçeneği `PhysicalPartitionsOfTable`olduğunda uygulayın. Kaynak verileri almak için bir sorgu kullanırsanız, WHERE yan tümcesinde kanca `?AdfTabularPartitionName` . Örnek için [Oracle 'Dan paralel kopyalama](#parallel-copy-from-oracle) bölümüne bakın. | Hayır |
| partitionColumnName | Paralel kopya için Aralık bölümleme tarafından kullanılacak, **tamsayı türünde** kaynak sütunun adını belirtin. Belirtilmemişse, tablonun birincil anahtarı otomatik olarak algılanır ve bölüm sütunu olarak kullanılır. <br>Bölüm seçeneği `DynamicRange`olduğunda uygulayın. Kaynak verileri almak için bir sorgu kullanırsanız, WHERE yan tümcesinde kanca `?AdfRangePartitionColumnName` . Örnek için [Oracle 'Dan paralel kopyalama](#parallel-copy-from-oracle) bölümüne bakın. | Hayır |
| partitionüstsınırı | Verilerin kopyalanacağı bölüm sütununun en büyük değeri. <br>Bölüm seçeneği `DynamicRange`olduğunda uygulayın. Kaynak verileri almak için bir sorgu kullanırsanız, WHERE yan tümcesinde kanca `?AdfRangePartitionUpbound` . Örnek için [Oracle 'Dan paralel kopyalama](#parallel-copy-from-oracle) bölümüne bakın. | Hayır |
| partitionalme sınırı | Verilerin kopyalanacağı bölüm sütununun en küçük değeri. <br>Bölüm seçeneği `DynamicRange`olduğunda uygulayın. Kaynak verileri almak için bir sorgu kullanırsanız, WHERE yan tümcesinde kanca `?AdfRangePartitionLowbound` . Örnek için [Oracle 'Dan paralel kopyalama](#parallel-copy-from-oracle) bölümüne bakın. | Hayır |

**Örnek: bölüm olmadan temel sorgu kullanarak veri kopyalama**

```json
"activities":[
    {
        "name": "CopyFromOracle",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Oracle input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "OracleSource",
                "oracleReaderQuery": "SELECT * FROM MyTable"
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

### <a name="oracle-as-a-sink-type"></a>Havuz türü olarak Oracle

Verileri Oracle 'a kopyalamak için kopyalama etkinliğindeki havuz türünü olarak `OracleSink`ayarlayın. Aşağıdaki özellikler, etkinlik **havuzunu** Kopyala bölümünde desteklenir.

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| türü | Kopyalama etkinliği havuzunun Type özelliği olarak `OracleSink`ayarlanmalıdır. | Evet |
| writeBatchSize | Arabellek boyutu ulaştığında `writeBatchSize`SQL tablosuna veri ekler.<br/>İzin verilen değerler Integer (satır sayısı). |Hayır (varsayılan değer 10.000) |
| writeBatchTimeout | Toplu iş ekleme işleminin, zaman aşımına uğramadan önce tamamlaması için bekleme süresi.<br/>İzin verilen değerler TimeSpan. Örnek olarak 00:30:00 (30 dakika). | Hayır |
| preCopyScript | Kopyalama etkinliği için, her çalıştırmada verileri Oracle 'a yazmadan önce çalıştırılacak bir SQL sorgusu belirtin. Bu özelliği, önceden yüklenmiş verileri temizlemek için kullanabilirsiniz. | Hayır |

**Örnek:**

```json
"activities":[
    {
        "name": "CopyToOracle",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<Oracle output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "<source type>"
            },
            "sink": {
                "type": "OracleSink"
            }
        }
    }
]
```

## <a name="parallel-copy-from-oracle"></a>Oracle 'dan paralel kopyalama

Data Factory Oracle Bağlayıcısı, verileri Oracle 'dan paralel olarak kopyalamak için yerleşik veri bölümlemesini sağlar. Kopyalama etkinliğinin **kaynak** sekmesinde veri bölümleme seçeneklerini bulabilirsiniz.

![Bölüm seçeneklerinin ekran görüntüsü](./media/connector-oracle/connector-oracle-partition-options.png)

Bölümlenmiş kopyayı etkinleştirdiğinizde Data Factory verileri bölümlere göre yüklemek için Oracle kaynağınızdan paralel sorgular çalıştırır. Paralel derece kopyalama etkinliğindeki [`parallelCopies`](copy-activity-performance.md#parallel-copy) ayar tarafından denetlenir. Örneğin, dört olarak ayarlarsanız `parallelCopies` , Data Factory aynı anda, belirtilen bölüm seçeneğiniz ve ayarlarınıza göre dört sorgu üretir ve çalışır. Her sorgu, Oracle veritabanınızdaki verilerin bir kısmını alır.

Özellikle de Oracle veritabanınızdan büyük miktarda veri yüklediğinizde, veri bölümleme ile paralel kopyayı etkinleştirmek iyi bir fikirdir. Farklı senaryolar için önerilen yapılandırma aşağıda verilmiştir:

| Senaryo                                                     | Önerilen ayarlar                                           |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| Fiziksel bölümlerle büyük tablodan tam yük.          | **Bölüm seçeneği**: Tablonun fiziksel bölümleri. <br><br/>Yürütme sırasında Data Factory fiziksel bölümleri otomatik olarak algılar ve verileri bölümlere göre kopyalar. |
| Fiziksel bölümler olmadan, veri bölümlendirme için bir tamsayı sütunuyla birlikte büyük tablodan tam yük. | **Bölüm seçenekleri**: Dinamik Aralık bölümü.<br>**Bölüm sütunu**: Verileri bölümlemek için kullanılan sütunu belirtin. Belirtilmemişse, birincil anahtar sütunu kullanılır. |
| Fiziksel bölümlerle özel bir sorgu kullanarak büyük miktarda veri yükleyin. | **Bölüm seçeneği**: Tablonun fiziksel bölümleri.<br>**Sorgu**: `SELECT * FROM <TABLENAME> PARTITION("?AdfTabularPartitionName") WHERE <your_additional_where_clause>`.<br>**Bölüm adı**: Verilerin kopyalanacağı bölüm adlarını belirtin. Belirtilmezse, Data Factory Oracle veri kümesinde belirttiğiniz tablodaki fiziksel bölümleri otomatik olarak algılar.<br><br>Yürütme sırasında Data Factory, gerçek `?AdfTabularPartitionName` bölüm adıyla değiştirilir ve Oracle 'a gönderilir. |
| Veri bölümlendirme için bir tamsayı sütunu ile, fiziksel bölümler olmadan, özel bir sorgu kullanarak büyük miktarda veri yükleyin. | **Bölüm seçenekleri**: Dinamik Aralık bölümü.<br>**Sorgu**: `SELECT * FROM <TABLENAME> WHERE ?AdfRangePartitionColumnName <= ?AdfRangePartitionUpbound AND ?AdfRangePartitionColumnName >= ?AdfRangePartitionLowbound AND <your_additional_where_clause>`.<br>**Bölüm sütunu**: Verileri bölümlemek için kullanılan sütunu belirtin. Tamsayı veri türünde bir sütuna göre bölümleyebilirsiniz.<br>**Bölüm üst sınırı** ve **bölüm alt sınırı**: Yalnızca alt ve üst Aralık arasında veri almak için bölüm sütununa karşı filtreleme yapmak istediğinizi belirtin.<br><br>Yürütme sırasında Data Factory, `?AdfRangePartitionColumnName` `?AdfRangePartitionUpbound`, ve `?AdfRangePartitionLowbound` değerlerini her bölüm için gerçek sütun adı ve değer aralıklarıyla değiştirir ve Oracle 'a gönderir. <br>Örneğin, "ID" adlı bölüm sütununuzu alt sınır 1 ve üst sınır 80 olarak ayarlandıysa, paralel kopyalama 4 olarak ayarlanır Data Factory verileri 4 bölümden alır. Kimlikleri sırasıyla [1, 20], [21, 40], [41, 60] ve [61, 80] arasındadır. |

**Örnek: fiziksel bölüm ile sorgulama**

```json
"source": {
    "type": "OracleSource",
    "query": "SELECT * FROM <TABLENAME> PARTITION(\"?AdfTabularPartitionName\") WHERE <your_additional_where_clause>",
    "partitionOption": "PhysicalPartitionsOfTable",
    "partitionSettings": {
        "partitionNames": [
            "<partitionA_name>",
            "<partitionB_name>"
        ]
    }
}
```

**Örnek: Dinamik Aralık bölümü ile sorgulama**

```json
"source": {
    "type": "OracleSource",
    "query": "SELECT * FROM <TABLENAME> WHERE ?AdfRangePartitionColumnName <= ?AdfRangePartitionUpbound AND ?AdfRangePartitionColumnName >= ?AdfRangePartitionLowbound AND <your_additional_where_clause>",
    "partitionOption": "DynamicRange",
    "partitionSettings": {
        "partitionColumnName": "<partition_column_name>",
        "partitionUpperBound": "<upper_value_of_partition_column>",
        "partitionLowerBound": "<lower_value_of_partition_column>"
    }
}
```

## <a name="data-type-mapping-for-oracle"></a>Oracle için veri türü eşlemesi

Ve Oracle 'a veri kopyaladığınızda aşağıdaki eşlemeler geçerlidir. Kopyalama etkinliğinin kaynak şemayı ve veri türünü havuza nasıl eşlediğini öğrenmek için bkz. [şema ve veri türü eşlemeleri](copy-activity-schema-and-type-mapping.md).

| Oracle veri türü | Veri Fabrikası geçici veri türü |
|:--- |:--- |
| BFILE |Byte[] |
| BUN |Byte[]<br/>(yalnızca Oracle 10G ve üzeri sürümlerde desteklenir) |
| CHAR |Dize |
| CLOB |Dize |
| DATE |Datetime |
| FLOAT |Decimal, String (olursa hassasiyet > 28) |
| GIR |Decimal, String (olursa hassasiyet > 28) |
| LONG |Dize |
| LONG RAW |Byte[] |
| NCHAR |Dize |
| NCLOB |Dize |
| NUMBER |Decimal, String (olursa hassasiyet > 28) |
| NVARCHAR2 |Dize |
| RAW |Byte[] |
| ROWID |Dize |
| TIMESTAMP |Datetime |
| TIMESTAMP WITH LOCAL TIME ZONE |Dize |
| TIMESTAMP WITH TIME ZONE |Dize |
| UNSIGNED INTEGER |Number |
| VARCHAR2 |Dize |
| XML |Dize |

> [!NOTE]
> Veri türleri zaman ARALıĞı ve Aralık gün/sanıye desteklenmez.


## <a name="next-steps"></a>Sonraki adımlar
Veri fabrikasında kopyalama etkinliği tarafından kaynak ve havuz olarak desteklenen veri depolarının listesi için bkz. [desteklenen veri depoları](copy-activity-overview.md##supported-data-stores-and-formats).
