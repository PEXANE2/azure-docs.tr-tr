---
title: Azure Veri Fabrikası'nı kullanarak Oracle'a veri kopyalama
description: Desteklenen kaynak mağazalardaki verileri Oracle veritabanına veya Oracle'dan desteklenen lavabo mağazalarına Veri Fabrikası'nı kullanarak nasıl kopyalayın öğrenin.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: shwang
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 02/13/2020
ms.author: jingwang
ms.openlocfilehash: 874c685491774e2a318ae0a8b7394945a51b2f7f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79244517"
---
# <a name="copy-data-from-and-to-oracle-by-using-azure-data-factory"></a>Azure Veri Fabrikası'nı kullanarak verileri Oracle'dan kopyalayın
> [!div class="op_single_selector" title1="Kullandığınız Veri Fabrikası hizmetisürümünü seçin:"]
> * [Sürüm 1](v1/data-factory-onprem-oracle-connector.md)
> * [Geçerli sürüm](connector-oracle.md)

Bu makalede, Verileri bir Oracle veritabanından kopyalamak için Azure Veri Fabrikası'ndaki kopyalama etkinliğinin nasıl kullanılacağı açıklanmaktadır. [Kopyalama etkinliğine genel bakış](copy-activity-overview.md)üzerine inşa edin.

## <a name="supported-capabilities"></a>Desteklenen yetenekler

Bu Oracle bağlayıcısı aşağıdaki etkinlikler için desteklenir:

- [Desteklenen kaynak/lavabo matrisi](copy-activity-overview.md) ile [etkinliği](copy-activity-overview.md) kopyalama
- [Arama etkinliği](control-flow-lookup-activity.md)

Oracle veritabanından desteklenen herhangi bir lavabo veri deposuna verileri kopyalayabilirsiniz. Ayrıca, desteklenen herhangi bir kaynak veri deposundan oracle veritabanına veri kopyalayabilirsiniz. Kopyalama etkinliği tarafından kaynak veya lavabo olarak desteklenen veri depolarının listesi için [Desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats) tablosuna bakın.

Özellikle, bu Oracle bağlayıcı destekler:

- Oracle veritabanının aşağıdaki sürümleri:
    - Oracle 18c R1 (18.1) ve üzeri
    - Oracle 12c R1 (12.1) ve üzeri
    - Oracle 11g R1 (11.1) ve üzeri
    - Oracle 10g R1 (10.1) ve üzeri
    - Oracle 9i R2 (9.2) ve üzeri
    - Oracle 8i R3 (8.1.7) ve üzeri
    - Oracle Veritabanı Bulut Exadata Hizmeti
- Oracle kaynağından paralel kopyalama. Ayrıntılar için [Oracle bölümünden Paralel kopyaya](#parallel-copy-from-oracle) bakın.

> [!Note]
> Oracle proxy sunucusu desteklenmez.

## <a name="prerequisites"></a>Ön koşullar

[!INCLUDE [data-factory-v2-integration-runtime-requirements](../../includes/data-factory-v2-integration-runtime-requirements.md)] 

Tümleştirme çalışma süresi yerleşik bir Oracle sürücüsü sağlar. Bu nedenle, Oracle'dan ve Oracle'dan veri kopyalarken bir sürücüyü el ile yüklemeniz gerekmez.

## <a name="get-started"></a>Kullanmaya başlayın

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Aşağıdaki bölümlerde, Oracle bağlayıcısına özgü Veri Fabrikası varlıklarını tanımlamak için kullanılan özellikler hakkında ayrıntılar sağlanmaktadır.

## <a name="linked-service-properties"></a>Bağlantılı hizmet özellikleri

Oracle bağlantılı hizmet aşağıdaki özellikleri destekler:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Tür özelliği **Oracle**olarak ayarlanmalıdır. | Evet |
| Connectionstring | Oracle Veritabanı örneğine bağlanmak için gereken bilgileri belirtir. <br/>Ayrıca Azure Key Vault'a bir parola `password` koyabilir ve yapılandırmayı bağlantı dizesinin dışına çekebilirsiniz. Azure [Anahtar Kasası'nda](store-credentials-in-key-vault.md) aşağıdaki örneklere ve Mağaza kimlik bilgilerine bakın. <br><br>**Desteklenen bağlantı türü**: Veritabanınızı tanımlamak için **Oracle SID** veya Oracle **Service Name'i** kullanabilirsiniz:<br>- SID kullanıyorsanız:`Host=<host>;Port=<port>;Sid=<sid>;User Id=<username>;Password=<password>;`<br>- Hizmet Adı kullanıyorsanız:`Host=<host>;Port=<port>;ServiceName=<servicename>;User Id=<username>;Password=<password>;`<br>Gelişmiş Oracle yerel bağlantı seçenekleri için TNSNAMES'e giriş eklemeyi [seçebilirsiniz. ](http://www.orafaq.com/wiki/Tnsnames.ora)Oracle sunucusundaki ORA dosyası ve ADF Oracle bağlantılı hizmette, Oracle Service Name bağlantı türünü kullanmayı ve ilgili hizmet adını yapılandırmayı seçin. | Evet |
| connectVia | Veri deposuna bağlanmak için kullanılacak [tümleştirme çalışma süresi.](concepts-integration-runtime.md) [Önkoşullar](#prerequisites) bölümünden daha fazla bilgi edinin. Belirtilmemişse, varsayılan Azure Tümleştirme Çalışma Süresi kullanılır. |Hayır |

>[!TIP]
>"ORA-01025: UPI parametresi kapsama alanı dışında" ve Oracle sürümünüz 8i `WireProtocolMode=1` olduğunda, bağlantı dizenize ekleyin. O zaman tekrar dene.

Servis talebinize göre bağlantı dizesi olarak ayarlayabildiğiniz daha fazla bağlantı özelliği:

| Özellik | Açıklama | İzin verilen değerler |
|:--- |:--- |:--- |
| Dizi Boyutu |Konektörün tek bir ağ gidiş turunda getirebileceği bayt sayısı. Örneğin, `ArraySize=‭10485760‬`.<br/><br/>Ağ üzerinden veri alma sayısını azaltarak daha büyük değerler iş verisini artırır. Sunucunun veri iletimini bekleyen gecikme sayısı daha az olduğundan, daha küçük değerler yanıt süresini artırır. | Bir sonda 1 den 4294967296 (4 GB) arası dır. Varsayılan değer. `60000` 1 değeri bayt sayısını tanımlamaz, ancak tam olarak bir veri satırı için alan ayırmayı gösterir. |

Oracle bağlantısında şifrelemeyi etkinleştirmek için iki seçeneğiniz var:

-   Oracle sunucu tarafında **Triple-DES Şifreleme (3DES) ve Gelişmiş Şifreleme Standardı (AES)** kullanmak için Oracle Advanced Security'ye (OAS) gidin ve şifreleme ayarlarını yapılandırın. Ayrıntılar için bu [Oracle belgelerine](https://docs.oracle.com/cd/E11882_01/network.112/e40393/asointro.htm#i1008759)bakın. Oracle Uygulama Geliştirme Çerçevesi (ADF) bağlayıcısı, Oracle'a bağlantı kurarken OAS'da yapılandırdığınız şifreleme yöntemini kullanmak için otomatik olarak görüşür.

-   **SSL**kullanmak için:

    1.  SSL sertifika bilgilerini alın. SSL sertifikanızın Seçkin Kodlama Kuralları (DER) kodlanmış sertifika bilgilerini alın ve çıktıyı kaydedin (sertifikayı başlatın ----- ... Sertifikayı -----) metin dosyası olarak sonolarak.

        ```
        openssl x509 -inform DER -in [Full Path to the DER Certificate including the name of the DER Certificate] -text
        ```

        **Örnek:** CERT bilgilerini DERcert.cer'den ayıklayın ve çıktıyı cert.txt'ye kaydedin.

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
    
    2.  Oluşturun `keystore` veya `truststore`. Aşağıdaki komut, `truststore` pkcs-12 biçiminde parolalı veya parolasız dosyayı oluşturur.

        ```
        openssl pkcs12 -in [Path to the file created in the previous step] -out [Path and name of TrustStore] -passout pass:[Keystore PWD] -nokeys -export
        ```

        **Örnek:** Parolayla MyTrustStoreFile `truststore` adında bir PKCS12 dosyası oluşturun.

        ```
        openssl pkcs12 -in cert.txt -out MyTrustStoreFile -passout pass:ThePWD -nokeys -export  
        ```

    3.  Dosyayı `truststore` kendi barındırılan IR makinesine yerleştirin. Örneğin, dosyayı C:\MyTrustStoreFile'a yerleştirin.
    4.  Azure Veri Fabrikası'nda Oracle bağlantı `EncryptionMethod=1` dizesini `TrustStore` / `TrustStorePassword`ve ilgili değeri yapılandırın. Örneğin, `Host=<host>;Port=<port>;Sid=<sid>;User Id=<username>;Password=<password>;EncryptionMethod=1;TrustStore=C:\\MyTrustStoreFile;TrustStorePassword=<trust_store_password>`.

**Örnek:**

```json
{
    "name": "OracleLinkedService",
    "properties": {
        "type": "Oracle",
        "typeProperties": {
            "connectionString": "Host=<host>;Port=<port>;Sid=<sid>;User Id=<username>;Password=<password>;"
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

**Örnek: parolayı Azure Key Vault'ta depolama**

```json
{
    "name": "OracleLinkedService",
    "properties": {
        "type": "Oracle",
        "typeProperties": {
            "connectionString": "Host=<host>;Port=<port>;Sid=<sid>;User Id=<username>;",
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

Bu bölümde, Oracle veri kümesi tarafından desteklenen özelliklerin bir listesi yer almaktadır. Veri kümelerini tanımlamak için kullanılabilen bölümlerin ve özelliklerin tam listesi için [bkz.](concepts-datasets-linked-services.md) 

Verileri Oracle'dan ve Oracle'a kopyalamak için, `OracleTable`veri kümesinin tür özelliğini . Aşağıdaki özellikler desteklenir.

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Veri kümesinin tür özelliği `OracleTable`' ye ayarlanmalıdır. | Evet |
| Şema | Şema adı. |Kaynak için hayır, lavabo için Evet  |
| tablo | Tablonun/görünümün adı. |Kaynak için hayır, lavabo için Evet  |
| tableName | Şema ile tablo/görünüm adı. Bu özellik geriye dönük uyumluluk için desteklenir. Yeni iş yükü `schema` `table`için, kullanım ve . | Kaynak için hayır, lavabo için Evet |

**Örnek:**

```json
{
    "name": "OracleDataset",
    "properties":
    {
        "type": "OracleTable",
        "schema": [],
        "typeProperties": {
            "schema": "<schema_name>",
            "table": "<table_name>"
        },
        "linkedServiceName": {
            "referenceName": "<Oracle linked service name>",
            "type": "LinkedServiceReference"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri

Bu bölümde, Oracle kaynağı ve lavabo tarafından desteklenen özelliklerin bir listesini sağlar. Etkinlikleri tanımlamak için kullanılabilen bölümlerin ve özelliklerin tam listesi için [bkz.](concepts-pipelines-activities.md) 

### <a name="oracle-as-source"></a>Kaynak olarak Oracle

>[!TIP]
>Veri bölümleme kullanarak Oracle'dan veri yüklemek için, [Oracle'dan Paralel kopyadan](#parallel-copy-from-oracle)daha fazla bilgi edinin.

Oracle'dan gelen verileri kopyalamak için, kopyalama `OracleSource`etkinliğindeki kaynak türünü ' e ayarlar. Aşağıdaki özellikler kopyalama etkinliği **kaynak** bölümünde desteklenir.

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Kopyalama etkinlik kaynağının türü özelliği ' `OracleSource`ne ayarlanmalıdır. | Evet |
| oracleReaderQuery | Verileri okumak için özel SQL sorgusunu kullanın. `"SELECT * FROM MyTable"` bunun bir örneğidir.<br>Bölümlenmiş yükü etkinleştirdiğinizde, sorgunuzda karşılık gelen yerleşik bölüm parametrelerini bağlamanız gerekir. Örneğin, Oracle [bölümünden Paralel kopyaya](#parallel-copy-from-oracle) bakın. | Hayır |
| partitionOptions | Oracle'dan veri yüklemek için kullanılan veri bölümleme seçeneklerini belirtir. <br>İzin verilen değerler şunlardır: **Yok** (varsayılan), **PhysicalPartitionsOfTable** ve **DynamicRange**.<br>Bir bölüm seçeneği etkinleştirildiğinde (yani `None`değil), oracle veritabanından verileri aynı anda yüklemek için [`parallelCopies`](copy-activity-performance.md#parallel-copy) paralellik derecesi kopyalama etkinliği ayarı tarafından denetlenir. | Hayır |
| partitionAyarlar | Veri bölümleme için ayarlar grubunu belirtin. <br>Bölüm seçeneği olmadığında `None`uygulayın. | Hayır |
| partitionNames | Kopyalanması gereken fiziksel bölümlerin listesi. <br>Bölüm seçeneği . `PhysicalPartitionsOfTable` Kaynak verileri almak için bir sorgu `?AdfTabularPartitionName` kullanıyorsanız, WHERE yan tümcesini bağla. Örneğin, [Oracle bölümünden Paralel kopyaya](#parallel-copy-from-oracle) bakın. | Hayır |
| partitionColumnName | Paralel kopya için aralık bölümleme tarafından kullanılacak **tamsayı türünde** kaynak sütunun adını belirtin. Belirtilmemişse, tablonun birincil anahtarı otomatik olarak algılanır ve bölüm sütunu olarak kullanılır. <br>Bölüm seçeneği . `DynamicRange` Kaynak verileri almak için bir sorgu `?AdfRangePartitionColumnName` kullanıyorsanız, WHERE yan tümcesini bağla. Örneğin, [Oracle bölümünden Paralel kopyaya](#parallel-copy-from-oracle) bakın. | Hayır |
| partitionUpperBound | Verileri kopyalamak için bölüm sütununun maksimum değeri. <br>Bölüm seçeneği . `DynamicRange` Kaynak verileri almak için bir sorgu `?AdfRangePartitionUpbound` kullanıyorsanız, WHERE yan tümcesini bağla. Örneğin, [Oracle bölümünden Paralel kopyaya](#parallel-copy-from-oracle) bakın. | Hayır |
| partitionLowerBound | Verileri kopyalamak için bölüm sütununun minimum değeri. <br>Bölüm seçeneği . `DynamicRange` Kaynak verileri almak için bir sorgu `?AdfRangePartitionLowbound` kullanıyorsanız, WHERE yan tümcesini bağla. Örneğin, [Oracle bölümünden Paralel kopyaya](#parallel-copy-from-oracle) bakın. | Hayır |

**Örnek: bölüm olmadan temel bir sorgu kullanarak verileri kopyalama**

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

### <a name="oracle-as-sink"></a>Oracle lavabo olarak

Verileri Oracle'a kopyalamak için, kopyalama etkinliğindeki lavabo türünü ' e `OracleSink`ayarla. Aşağıdaki özellikler kopyalama etkinliği **lavabo** bölümünde desteklenir.

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Kopyalama etkinliği lavabonun türü özelliği `OracleSink`' ye ayarlanmalıdır. | Evet |
| yazmaBatchSize | Arabellek boyutuna ulaştığında `writeBatchSize`verileri SQL tablosuna ekler.<br/>İzin verilen değerler Insayger (satır sayısı) vardır. |Hayır (varsayılan değer 10.000'dir) |
| yazmaBatchTimeout | Toplu ekleme işleminin zaman dolmadan tamamlanması için bekleme süresi.<br/>İzin verilen değerler Timespan'dır. Bir örnek 00:30:00 (30 dakika). | Hayır |
| preCopyScript | Her çalıştırmada Oracle'a veri yazmadan önce çalışacak kopyalama etkinliği için bir SQL sorgusu belirtin. Önceden yüklenmiş verileri temizlemek için bu özelliği kullanabilirsiniz. | Hayır |

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

## <a name="parallel-copy-from-oracle"></a>Oracle'dan paralel kopya

Veri Fabrikası Oracle bağlayıcısı, Oracle'dan gelen verileri paralel olarak kopyalamak için yerleşik veri bölümleme sağlar. Kopyalama etkinliğinin **Kaynak** sekmesinde veri bölümleme seçeneklerini bulabilirsiniz.

![Bölüm seçeneklerinin ekran görüntüsü](./media/connector-oracle/connector-oracle-partition-options.png)

Bölümlenmiş kopyalamayı etkinleştirdiğinizde, Veri Fabrikası verileri bölümlere göre yüklemek için Oracle kaynağınıza paralel sorgular çalıştırAr. Paralel derece, kopyalama [`parallelCopies`](copy-activity-performance.md#parallel-copy) etkinliği üzerindeki ayar tarafından denetlenir. Örneğin, dörde `parallelCopies` ayarlarsanız, Veri Fabrikası aynı anda belirttiğiniz bölüm seçeneğiniz ve ayarlarınızı temel alarak dört sorgu oluşturur ve çalıştırZ ve her sorgu Oracle veritabanınızdan verilerin bir kısmını alır.

Özellikle Oracle veritabanınızdan büyük miktarda veri yüklediğinizde, veri bölümleme ile paralel kopyalamayı etkinleştirmeniz önerilir. Aşağıda farklı senaryolar için önerilen yapılandırmalar vardır. Verileri dosya tabanlı veri deposuna kopyalarken, bir klasöre birden çok dosya olarak yazmak (yalnızca klasör adını belirtin) yeniden komut verilir ve bu durumda performans tek bir dosyaya yazmaktan daha iyidir.

| Senaryo                                                     | Önerilen ayarlar                                           |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| Fiziksel bölümlerle büyük tablodan tam yük.          | **Bölüm seçeneği**: Tablonun fiziksel bölümleri. <br><br/>Yürütme sırasında, Veri Fabrikası fiziksel bölümleri otomatik olarak algılar ve verileri bölümlere göre kopyalar. |
| Büyük tablodan, fiziksel bölümler olmadan tam yük, veri bölümleme için bir tamsayı sütunu varken. | **Bölüm seçenekleri**: Dinamik aralık bölümü.<br>**Partition column**: Verileri bölmek için kullanılan sütunu belirtin. Belirtilmemişse, birincil anahtar sütunu kullanılır. |
| Fiziksel bölümlerle özel bir sorgu kullanarak büyük miktarda veri yükleyin. | **Bölüm seçeneği**: Tablonun fiziksel bölümleri.<br>**Sorgu** `SELECT * FROM <TABLENAME> PARTITION("?AdfTabularPartitionName") WHERE <your_additional_where_clause>`: .<br>**Bölüm adı**: Verileri kopyalamak için bölüm adı(lar) belirtin. Belirtilmemişse, Veri Fabrikası Oracle veri kümesinde belirttiğiniz tablodaki fiziksel bölümleri otomatik olarak algılar.<br><br>Yürütme sırasında, Veri `?AdfTabularPartitionName` Fabrikası gerçek bölüm adı ile değiştirir ve Oracle'a gönderir. |
| Veri bölümleme için bir tamsayı sütunu varken, fiziksel bölümler olmadan özel bir sorgu kullanarak büyük miktarda veri yükleyin. | **Bölüm seçenekleri**: Dinamik aralık bölümü.<br>**Sorgu** `SELECT * FROM <TABLENAME> WHERE ?AdfRangePartitionColumnName <= ?AdfRangePartitionUpbound AND ?AdfRangePartitionColumnName >= ?AdfRangePartitionLowbound AND <your_additional_where_clause>`: .<br>**Partition column**: Verileri bölmek için kullanılan sütunu belirtin. Noter veri türüne sahip sütuna karşı bölümleyebilirsiniz.<br>**Bölüm üst sınır** ve **bölüm alt sınır**: Yalnızca alt ve üst aralık arasında veri almak için bölüm sütununa karşı filtre lemek isteyip istemediğinizi belirtin.<br><br>Yürütme sırasında, Veri `?AdfRangePartitionColumnName`Fabrikası `?AdfRangePartitionUpbound`, `?AdfRangePartitionLowbound` , ve her bölüm için gerçek sütun adı ve değer aralıkları ile değiştirir ve Oracle'a gönderir. <br>Örneğin, bölüm sütununuzun "ID" alt sınırı 1 ve üst sınır 80 olarak ayarlanmışsa, paralel kopya 4 olarak ayarlanmışsa, Veri Fabrikası verileri 4 bölümle alır. Onların kimliklerini [1,20], [21, 40], [41, 60], ve [61, 80], arasındadır. |

**Örnek: fiziksel bölümlü sorgu**

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

**Örnek: dinamik aralık bölümü ile sorgu**

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

## <a name="data-type-mapping-for-oracle"></a>Oracle için veri türü eşleme

Verileri Oracle'dan ve Oracle'a kopyaladiğinizde, aşağıdaki eşlemeler geçerlidir. Kopyalama etkinliğinin kaynak şemasını ve veri türünü lavaboyla nasıl eşlenebildiğini öğrenmek için Bkz. [Şema ve veri türü eşlemeleri.](copy-activity-schema-and-type-mapping.md)

| Oracle veri türü | Veri Fabrikası geçici veri türü |
|:--- |:--- |
| Bdosya |Bayt[] |
| Blob |Bayt[]<br/>(sadece Oracle 10g ve üzeri desteklenir) |
| Char |Dize |
| Clob |Dize |
| DATE |DateTime |
| Float |Ondalık, String (hassas > 28) |
| TAMSAYI |Ondalık, String (hassas > 28) |
| Uzun |Dize |
| UZUN HAM |Bayt[] |
| Nchar |Dize |
| Nclob |Dize |
| Numarası |Ondalık, String (hassas > 28) |
| NVARCHAR2 |Dize |
| Ham |Bayt[] |
| Rowıd |Dize |
| Zaman damgası |DateTime |
| YEREL SAAT DILIMILI ZAMAN DAMGASı |Dize |
| SAAT DILIMILI ZAMAN DAMGASı |Dize |
| İmzaSıZ SONDA |Sayı |
| VARCHAR2 |Dize |
| XML |Dize |

> [!NOTE]
> Veri türleri INTERVAL YEAR to MONTH ve INTERVAL DAY to SECOND desteklenmez.

## <a name="lookup-activity-properties"></a>Arama etkinlik özellikleri

Özellikler hakkında daha fazla bilgi edinmek için [Arama etkinliğini](control-flow-lookup-activity.md)kontrol edin.

## <a name="next-steps"></a>Sonraki adımlar
Veri Fabrikası'ndaki kopyalama etkinliği tarafından kaynak ve lavabo olarak desteklenen veri depolarının listesi [için](copy-activity-overview.md#supported-data-stores-and-formats)bkz.
